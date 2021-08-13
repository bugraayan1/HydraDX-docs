# HydraDX'te Tesnet Dağıtımımızın Tasarımı ve Otomasyonu

Bu makalede, Kubernetes (EKS Fargate), AWS ACM, Route53, Terraform ve Github Actions kullanarak yeni bir testnet (Parachain + Relaychain) dağıtabilmek için ardışık düzenimizi nasıl tasarladığımızı ve otomatikleştirdiğimizi göstereceğiz.

## Fargate ile EKS seçimi
### Neden Fargate ile EKS?
Parachain ve Relaychain görüntülerimiz, her biri için bir veya daha fazla kapsayıcıya ihtiyaç duyan iki ayrı görüntüye dayanmaktadır. Kubernetes bugün sektörde konteyner otomasyonu ve ölçeklendirme standardı olduğu için doğal olarak bu seçimi yaptık (Docker Swarm'ın bazı ciddi ölçeklendirme sorunları var, eğer ilgilenirse ayrı bir makalede bahsedebiliriz.)

Artık altyapımız kısmen AWS'ye dayandığından, kaputun altında çalışan EC2 bulut sunucularına sahip EKS'ye sahip olmak veya Fargate kullanmak arasında seçim yapma şansımız vardı. İkisi arasındaki fark, EC2 ile kaynakları kontrol etme konusunda daha az esnekliğe sahip olmanızdır; gelecekte çalıştırmanız gereken bölme sayısı hakkında hiçbir fikriniz yoksa, büyük olasılıkla bulut sunucularınızın kapasitesini (CPU / RAM gücü ve sayı) olduğundan fazla tahmin etmeniz gerekecektir, bu da gereksiz kapasite kaybına neden olabilir. ve daha yüksek faturalar. Diğer bir neden ise, zaman ve kaynak gerektiren bu EC2 bulut sunucularının yönetilmesi gerekmesidir.

Bu nedenlerle, dağıtımlarımızla başa çıkmak ve bunları doğru bir şekilde (yukarı veya aşağı) ölçeklendirebilmek için Fargate kullanımının daha iyi bir çözüm olabileceği sonucuna vardık. Fargate'de, bulut sunucuları veya sunucular hakkında endişelenmenize gerek yok, tek yapmanız gereken (özetle) Kubernetes Manifest'lerinizi yazmak, bunları uygulamak ve gerisini AWS halledecektir; yani sunucuların sağlanması, bölmelerin planlanması vb.

AWS'de bir Kubernetes Örneği oluşturmak için EKSCTL veya Terraform kullanabilirsiniz. Burada süslü bir şey yok. İşte bir Fargate Kümesi oluşturmaya bir örnek (belgelerden):

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: fargate-cluster
  region: ap-northeast-1

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 1

fargateProfiles:
  - name: fp-default
    selectors:
      # All workloads in the "default" Kubernetes namespace will be
      # scheduled onto Fargate:
      - namespace: default
      # All workloads in the "kube-system" Kubernetes namespace will be
      # scheduled onto Fargate:
      - namespace: kube-system
  - name: fp-dev
    selectors:
      # All workloads in the "dev" Kubernetes namespace matching the following
      # label selectors will be scheduled onto Fargate:
      - namespace: dev
        labels:
          env: dev
          checks: passed
```

Tamamlandığında, tek yapmamız gereken Kubernetes Nesnelerimizi oluşturmak ve uygulamak.

### Röle Zincirimizin Dağıtımı
#### Dağıtım Örneği:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: YOUR_NAMESPACE
  name: relaychain-alice-deployment
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: relaychain-alice
  replicas: 1
  template:
    metadata:
      labels:
        app.kubernetes.io/name: relaychain-alice
    spec:
      containers:
      - image: YOUR-IMAGE-HERE
        imagePullPolicy: Always
        name: relaychain-alice
        command: ["/polkadot/polkadot"]
        args: ["--chain", "/polkadot/config.json", ..."]
        ports:
        - containerPort: 9944
        - containerPort: 30333
```

Bu bildirimde, düğümümüzün adını, açığa çıkarılacak bağlantı noktalarını, komutu ve argümanını (lütfen HydraDX belgelerini kontrol edin) ve kopya sayısını seçiyoruz. Eşitleme sorunlarını önlemek için düğüm başına yalnızca bir çoğaltma istediğimizden bu parametre önemlidir. Gerektiği kadar çok düğümünüz olabileceğini unutmayın.

#### Hizmet Örneği
Kubernetes'te Service nesnesini burada en az iki amaç için kullanırız:
1. İlk olarak, düğümlerin birbirleriyle iletişim kurabilmesi için lütfen [daha fazla bilgi için bu bağlantıyı] kontrol edin(https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)
2. Gerekirse, bir giriş denetleyicisi kullanarak hizmeti dış dünyaya gösterebilmek.

Süslü değil, sadece başka bir temel hizmet:

```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: YOUR_NAMESPACE
  name: SVC_NAME
spec:
  ports:
    - port: 9944
      name: websocket
      targetPort: 9944
      protocol: TCP
    - port: 30333
      name: custom-port
      targetPort: 30333
      protocol: TCP
  type: NodePort
  selector:
    app.kubernetes.io/name: relaychain-alice
```

Hizmeti dış dünyaya göstermek istiyorsanız, 'selektör' parametresinin çok önemli hale geldiğini lütfen unutmayın.

Ve işte! Bu kadar. Şimdi son bir adım, bir Hizmeti (belirli bir Dağıtımla ilgili) dış dünyaya göstermek istediğimiz zamandır. Bunun için Giriş Nesnesi dediğimiz şeyi kullanıyoruz:

#### Giriş Örneği:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  namespace: YOUR_NAMESPACE
  name: INGRESS_OBJECT_NAME
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/group.name: wstgroup2
    alb.ingress.kubernetes.io/load-balancer-attributes: idle_timeout.timeout_seconds=4000
    alb.ingress.kubernetes.io/auth-session-timeout: '86400'
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP":443}, {"HTTPS":443}]'
    alb.ingress.kubernetes.io/healthcheck-path: /
    alb.ingress.kubernetes.io/healthcheck-port: '80'
    alb.ingress.kubernetes.io/target-group-attributes: stickiness.enabled=true,stickiness.lb_cookie.duration_seconds=600
    alb.ingress.kubernetes.io/certificate-arn: YOUR_ARN
  labels:
    app: relaychain
spec:
  rules:
    - host: relaychain.hydration.cloud
      http:
        paths:
          - path: /ws/
            backend:
              serviceName: relaychain-bob-svc
              servicePort: 80

```

Bu nesne, yani "Giriş", hizmetimize "relaychain.hidration.cloud" ana bilgisayar adresini kullanarak dış dünyadan erişilebilmesi için kullanılır. Bunun için AWS'nin ALB Denetleyici Hizmetini kullanıyoruz [Daha fazla bilgi burada](https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html)

Bu Girişin parametreleri oldukça basittir ve olduğu gibi tutulabilir [daha fazla bilgi için lütfen bu bağlantıyı kontrol edin](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/ kılavuz/giriş/ek açıklamalar/). Değiştirilecek en önemli değer, [ACM](https://docs)'da bir giriş oluşturduğunuzda aldığınız ACM Sertifikasının tanımlayıcısı olan `alb.ingress.kubernetes.io/certificate-arn` değeridir. .aws.amazon.com/acm/latest/userguide/acm-overview.html) "ana makineniz" için. Bu makalenin ilerleyen kısımlarında daha fazla ayrıntı.

### Parachain'imizin Dağıtımı

Adımlar hemen hemen aynı olduğundan, Parachain'imizi dağıtmak için kullandığımız her nesne için basit örnekler:

#### Dağıtım Örneği (düzenleyici):
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: YOUR_NAMESPACE
  name: parachain-coll-01-deployment
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: parachain-coll-01
  replicas: 1
  template:
    metadata:
      labels:
        app.kubernetes.io/name: parachain-coll-01
    spec:
      containers:
      - image: YOUR_IMAGE
        imagePullPolicy: Always
        name: parachain-coll-01
        volumeMounts:
          - mountPath: /tmp
            name: persistent-storage
        command: ["/basilisk/basilisk"]
        args: ["--chain", "local", "--parachain-id", "", "--alice", "--base-path", "/basilisk/", "--node-key", "", "--bootnodes", "/dns/coll-01-svc.YOUR_NAMESPACE.svc.cluster.local/tcp/30333/p2p/KEY", "--", "--chain", "/tmp/rococo-local-raw.json", "--bootnodes", "/dns/coll-01-svc.YOUR_NAMESPACE.svc.cluster.local/tcp/30333/p2p/KEY", "--base-path", "/basilisk/", "--execution=wasm"]
        ports:
        - containerPort: 9944
        - containerPort: 9933
        - containerPort: 30333
      volumes:
        - name: persistent-storage
          persistentVolumeClaim:
            claimName: efs-pv  
```
#### Servis Örneği:

```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: NAMESPACE
  name: coll-01-svc
spec:
  ports:
    - port: 9944
      name: websocket
      targetPort: 9944
      protocol: TCP
    - port: 30333
      name: custom-port
      targetPort: 30333
      protocol: TCP
    - port: 9933
      name: rpc-port
      targetPort: 9933  
  type: NodePort
  selector:
    app.kubernetes.io/name: parachain-coll-01
```

#### Herkese açık RPC servisi:
```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: NAMESPACE
  name: public-rpc-svc
spec:
  ports:
    - port: 80
      name: websocket
      targetPort: 9944
      protocol: TCP
  type: NodePort    
  selector:
    app.kubernetes.io/name: public-rpc
```
#### Giriş:
Giriş Bildirimi tamamen aynı kalır.
### Karşılaştığımız zorluklar neler?
EC2 ve Fargate bulut sunucuları arasında yapmak zorunda olduğumuz seçimin yanı sıra, ele alınması o kadar kolay olmayan bir sorunumuz vardı: **hacimler**. Dağıtımımız sırasında, yapılandırmanın boyutu 4 MB'den büyük olduğu için bir yapılandırma haritasında depolanamayan bir yapılandırmayı Basilisk Komutanlığımıza iletmemiz gerektiğini öğrendik, oysa yapılandırma haritaları yalnızca depolayabilir 1MB'a kadar. Şimdi sorun şu ki, bu EC2 ile Kubernetes'te (bir Birim oluşturun, bir dosya veya klasör koyun ve diğer bölmelerden kullanın) yapmak için oldukça basit bir şey, Fargate ile görev o kadar basit değil. Fargate'de Volumes, Ağustos 2020'ye kadar desteklenmiyordu ve özellik hala olgunlaşmadı. Bu nedenle, Kubernetes Dağıtımınızda yoğun olarak hacim kullanmanız gerekiyorsa, lütfen bunu dikkate alın. Ancak bu sorunu [AWS EFS ile birlikte](https://aws.amazon.com/blogs/aws/new-aws-fargate-for-amazon-eks-now-supports-amazon-efs/) izleyerek çözebiliriz. ). Fargate ile cilt kullanmak zorunda kalırsan, bu bağlantı kıçını kurtaracak, güven bana.


## ACM ve Route53
Düğümünüzü güzel ve güvenli bir URL ile dış dünyaya göstermeniz gerekiyorsa, AWS ACM'yi kullanabilirsiniz. Temel olarak, tek yapmanız gereken URL'nizin adıyla bir sertifika oluşturmak, onu doğrulamak (DNS aracılığıyla) ve ARN sonucunu almak. Ardından, Giriş Bildirimi dosyanıza `alb.ingress.kubernetes.io/certificate-arn` parametresinin bir değeri olarak ekleyin ve voilà!

## Otomatik Dağıtım için Terraform
Tabii ki, sertifikanızı CI'nizde otomatikleştirmek istiyorsanız, Terraform aracılığıyla oluşturabilirsiniz (bu seçimi biz yapmadık, ancak muhtemelen daha sonra dağıtacağız). Ancak, bu .tf dosyası size çok yardımcı olabilir:
```
provider "aws" {
  region = "eu-west-1"
}

# DNS Zone Name: hydraction.cloud
variable "dns_zone" {
  description = "Specific to your setup, pick a domain you have in route53"
  default = "hydration.cloud"
}
# subdomain name
variable "domain_dns_name" {
  description = "domainname"
  default     = "YOUR_SUBDOMAIN"
}


# On crée une datasource à partir du nom de la zone DNS
data "aws_route53_zone" "public" {
  name         = "${var.dns_zone}"
  private_zone = false
}
resource "aws_acm_certificate" "myapp-cert" {
  domain_name       = "${var.domain_dns_name}.${data.aws_route53_zone.public.name}"
  #subject_alternative_names = ["${var.alternate_dns_name}.${data.aws_route53_zone.public.name}"]
  validation_method = "DNS"
  lifecycle {
    create_before_destroy = true
  }
}
resource "aws_route53_record" "cert_validation" {
  for_each = {
    for dvo in aws_acm_certificate.myapp-cert.domain_validation_options : dvo.domain_name => {
      name   = dvo.resource_record_name
      record = dvo.resource_record_value
      type   = dvo.resource_record_type
    }
  }
  allow_overwrite = true
  name            = each.value.name
  records         = [each.value.record]
  ttl             = 60
  type            = each.value.type
  zone_id         = data.aws_route53_zone.public.id
}
# This tells terraform to cause the route53 validation to happen
resource "aws_acm_certificate_validation" "cert" {
  certificate_arn         = aws_acm_certificate.myapp-cert.arn
  validation_record_fqdns = [for record in aws_route53_record.cert_validation : record.fqdn]
}

output "acm-arn" {
  value = aws_acm_certificate.myapp-cert.arn
}

```

Bu TF'nin çıktı değeri, `Ingress` Manifest dosyanızda kullanılacak ARN'dir.
## Hepsini sarmak için Github Eylemleri

Tabii ki, bildirim dosyalarınızı yazabilir ve Kubernetes Nesnelerinizi "kubectl application" kullanarak dağıtabilirsiniz, ancak bunu bir CI-CD aracılığıyla yapmak da isteyebilirsiniz. Github Eylemlerini kullanıyoruz ve oldukça basit:

```yaml
name: deploy app to k8s and expose
on:
  push: 
    branches:
      - main

jobs:
  deploy-prod:
    name: deploy
    runs-on: ubuntu-latest
    env:
      ACTIONS_ALLOW_UNSECURE_COMMANDS: true
      AWS_ACCESS_KEY_ID: ${{ secrets.K8S_AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.K8S_AWS_SECRET_KEY_ID }}
      AWS_REGION: ${{ secrets.AWS_REGION }}
      NAMESPACE: validators_namespace
      APPNAME1: validator1
      APPNAME2: validator2
      DOMAIN: hydration.cloud
      SUBDOMAIN: validator1
      IMAGENAME: YOUR_IMAGE
      CERTIFICATE_ARN: _CERTIFICATEARN_
    
    steps:
      - name: checkout code
        uses: actions/checkout@v2.1.0
      
      - name: run-everything
        run: |
          curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
          chmod +x ./kubectl
          sudo mv ./kubectl /usr/local/bin/kubectl
          export AWS_ACCESS_KEY_ID=${{ env.AWS_ACCESS_KEY_ID }}
          export AWS_SECRET_ACCESS_KEY=${{ env.AWS_SECRET_ACCESS_KEY }}
          curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
          sudo mv /tmp/eksctl /usr/local/bin
          eksctl version
          aws eks --region eu-west-1 update-kubeconfig --name CLUSTER_NAME
          kubectl delete all --all -n ${{ env.NAMESPACE }}
          eksctl create fargateprofile --cluster CLUSTER_NAME --region ${{ env.AWS_REGION }} --name ${{ env.NAMESPACE }} --namespace ${{ env.NAMESPACE }}
          sed -i 's/_NAMESPACE_/${{ env.NAMESPACE }}/g' components.yaml
          kubectl apply -f components.yaml
```
Bu iş akışı temel olarak fargate profilini oluşturur ve ayrıca tüm Kubernetes Nesnelerinizi içeren bildirim dosyanızı seçtiğiniz Küme'ye depolar. Tabii ki, doğru erişim ve gizli anahtarları verdiğinizden emin olun :).

İyi şanslar!
