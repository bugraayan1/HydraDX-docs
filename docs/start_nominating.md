kimlik: start_nominasyon
başlık: Aday Olun
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

Aday olarak, HydraDX ağının güvenliğini sağlamaya ve ödüller kazanmaya yardımcı olmak için HDX jetonlarınızdan bazılarını stake edersiniz. Bir doğrulayıcı düğüm çalıştırmanın aksine, aday gösterme süreci ileri teknik beceriler gerektirmez, bu da onu doğrulayıcı olma konusunda tam olarak emin olmayan herkes için önerilen seçim haline getirir.

Aday gösterirken, adaylar paylarını kendi seçtikleri bir doğrulayıcıya atar. Bunu yaparak, adaylar aktif doğrulayıcı grubunu seçer ve katılımları için ödüller alırlar. Aday olarak aldığınız ödüllerin miktarı, seçilen doğrulayıcının ödül komisyonu yüzdesine bağlıdır - doğrulayıcının ödül komisyonu ne kadar yüksek olursa, bahis tutarınız için o kadar az ödül alırsınız.

:::uyarı

Aday gösterme, stake sürecine katılımın daha erişilebilir bir şeklidir, ancak aynı zamanda belirli bir derecede risk de taşır. Aday gösterdiğiniz doğrulayıcı uygunsuz davranırsa (örn. Bir doğrulayıcıyı aday göstermeden önce gerekli özeni göstermenizi önemle tavsiye ederiz.

:::

## 00 Staking Arayüzü {#00-stake-ui}

Stake etme arayüzüne erişmek için önce Polkadot/apps'i açmanız, onu [genel HydraDX RPC düğümlerinden](/polkadotjs_apps_public) birine bağlamanız ve hesabınızı [bakiye](https://polkadot) görebildiğinizden emin olmanız gerekir. .js.org/apps/?rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/accounts)

:::Not

Balancer LBP etkinliği sırasında satın aldığınız xHDX jetonlarına hâlâ sahip misiniz? Devam etmeden önce [HDX'inizi talep etmeniz](/claim) gerekir.

:::

HDX bakiyenizi görebildiğinizi doğruladıktan sonra, Stake Kullanıcı Arayüzüne gidebilirsiniz:

*Ağ* > *Stake*

Stake Kullanıcı Arayüzü aşağıdaki menü sekmelerine sahiptir:

* **Stake genel bakış**: burada, tüm aktif doğrulayıcıların bir listesini ve düğümde stake edilen HDX miktarı, doğrulayıcının kendi hissesinin miktarı ve ne kadar ödül gibi her bir doğrulayıcı hakkında bazı temel bilgileri bulacaksınız. komisyon alınır. Ayrıca, geçerli çağda her bir doğrulayıcı tarafından kazanılan çağ puanlarının sayısını ve doğrulayıcı tarafından üretilen son bloğun sayısını görebilirsiniz.
* **Hesap işlemleri**: burada stake edebilir ve aday gösterebilirsiniz.
* **Ödemeler**: burada stake ödüllerinizi talep edebilirsiniz.
* **Hedefler**: Burada kazançlarınızı tahmin edebilirsiniz. Bu, aday göstermek için bir doğrulayıcı düğümü seçerken başlamak için iyi bir yerdir.
* **Bekliyor**: burada, etkin olmayan doğrulayıcı setine dahil edilmeden önce etkin olmayan doğrulayıcıların yerleştirildiği bekleme kuyruğunu bulabilirsiniz. Doğrulayıcı, etkin doğrulayıcı setine girmek için yeterli miktarda stake edilmiş HDX alana kadar bekleme kuyruğunda kalacaktır.
* **Doğrulayıcı istatistikleri**: burada, kazanılan dönem puanları, seçilen bahis miktarı, ödüller ve eğik çizgiler hakkında ayrıntılı geçmiş bilgileri görmek için bir doğrulayıcının saklama adresini sorgulayabilirsiniz. Adaylığınız için bir doğrulayıcıya güvenmeden önce bu bilgileri incelemenizi önemle tavsiye ederiz.

## 01 Bond HDX belirteçleri {#01-bond-hdx-belirteçleri}

:::uyarı

Ağın güvenliğini garanti etmek için bağlı HDX belirteçleri tehlikede. Aday gösterdiğiniz doğrulayıcı düğümün uygun olmayan davranışı, paranızın istemsiz olarak kaybedilmesine yol açabilecek kesinti ile cezalandırılabilir. Hangi doğrulayıcıyı aday göstereceğinizi seçerken gerekli özeni göstermenizi şiddetle tavsiye ederiz.

:::

HDX jetonlarını bağlamak için Stake Kullanıcı Arayüzündeki *Hesap işlemleri* bölümüne gidin:

*Ağ* > *Stake* > *Hesap işlemleri* > *+ Stash*

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/bond-hdx-1.png')} />
</div>

*Stash* düğmesine tıkladıktan sonra, dört düzenlenebilir alanla birleştirme tercihlerini görmelisiniz:
* **saklama hesabı**: HDX jetonlarınızın çoğunluğunu tutan hesap. HDX bu hesaptan stake edilecektir.
* **kontrolör hesabı**: aday gösterme sürecinin başlatılması ve durdurulmasıyla ilgili ücretleri karşılamak için gereken HDX'in daha küçük bir kısmına sahip olan hesap.
* **bağlı değer**: bağladığınız HDX miktarı. Sahip olduğunuz tüm HDX'leri bağlamayın - bunun yerine daha sonra oluşacak işlem ücretlerini karşılamak için biraz bırakın.
* **ödeme hedefi**: stake ödüllerinin gönderileceği hesap.

:::uyarı

Mevcut tüm HDX belirteçlerinizi bağlamayın. İşlem ücretlerini karşılamak için küçük bir rezerv bırakın. Sahip olduğunuz tüm HDX jetonlarını bağlarsanız, adaylık sürecini başlatmak için işlemi imzalayamayabilirsiniz.

:::

Bonding tercihlerini ayarladıktan sonra **Bond**'a tıklayın ve bonding işlemini tamamlamak için işlemi imzalayın.

:::Dikkat

Güvenlik nedeniyle, aynı Stash ve Controller hesaplarına sahip olmanız önerilmez. Ancak Snakenet'te transferler devre dışı bırakıldığı için şu anda ayrı hesaplar kullanmak mümkün değil. Fu'da bu mümkün olur olmaz ayrı Stash ve Controller hesaplarına geçmenizi şiddetle tavsiye ederiz.

tur.

:::

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/bond-hdx-2.png')} />
</div>

## 02 Doğrulayıcıyı aday göster {#02-nominate-a-validator}

HDX'i bağladıktan sonra artık bir doğrulayıcı atayabilirsiniz. Devam etmeden önce, gerekli özeni göstermeli ve (geçmiş) performanslarına göre hangi doğrulayıcıları aday göstermek istediğinize karar vermelisiniz. Bunu yapmak için Stake Kullanıcı Arayüzündeki [yukarıda tartışılan](#00-stake-ui) bilgilere bakın.

:::Not

HydraDX Snakenet'in **doğrulama düğümü başına 64 aday gösterme sınırı vardır**. Aday göstermek için bir düğüm seçerken, doğrulayıcının maksimum adaylık sayısına ulaşmadığından emin olun, aksi takdirde adaylığınız geçersiz olur ve bahis tutarınız için ödül alamazsınız. Her doğrulayıcı için aday sayısı, Stake Kullanıcı Arayüzündeki *Bekliyor* menü sekmesinde bulunabilir.

:::

Bir veya daha fazla doğrulayıcıyı aday göstermek için şuraya gidin:

*Ağ* > *Stake* > *Hesap işlemleri* > *Aday* (bağlı HDX'inizin yanındaki düğme)

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-validator-1.png')} />
</div>

*Aday* düğmesine tıkladıktan sonra *nominate validators* adlı bir açılır pencere görmelisiniz. Burada, mevcut doğrulayıcılar listesinden aday göstermek için bir veya daha fazla doğrulayıcı seçebilirsiniz. Bir doğrulayıcıda yer alamazsanız (örneğin, doğrulayıcı aşırı kalabalıksa veya aktif doğrulayıcı grubuna seçilmemişse) etkin olmamayı önlemek için birden fazla doğrulayıcı atamanız önemle tavsiye edilir. En fazla 16 doğrulayıcı aday gösterebilirsiniz. Her çağda adaylarınızdan sadece biri aktif olabilir, aynı anda birden fazla onaylayıcı tarafından seçilemezsiniz. Bahsiniz, ademi merkeziyetçiliği ve karı en üst düzeye çıkaracak şekilde seçtiğiniz doğrulayıcılardan birine otomatik olarak atanacaktır. Sadece bağlı HDX miktarını ve güvendiğiniz doğrulayıcıları seçtiniz.

Seçilen doğrulayıcıları atamak için _Aday_'ı tıklayın ve işlemi imzalayın.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-validator-2.png')} />
</div>


## 03 Adaylarınızın durumunu kontrol edin {#03-adaylarınızın durumunu kontrol edin}

Aday gösterme sürecini tamamladıktan sonra, adaylıklarınız mevcut dönemin geri kalanında etkin olmayacaktır. Bir sonraki dönem başladığında, aday gösterdiğiniz doğrulayıcı düğümlerinden en az birinin aktif doğrulayıcı grubuna dahil olması ve aşırı kalabalık olmaması ve sizi dışarıda bırakmaması koşuluyla, adaylıklarınız aktif hale gelecektir. Tüm doğrulayıcılarınız bekleme kuyruğunda kalırsa, ilgili adaylarınız da etkin olmayacak ve bu dönem için herhangi bir ödül kazanmayacaksınız.

Adaylıklarınızın durumunu kontrol etmek için şuraya gidin:

*Ağ* > *Stake* > *Hesap işlemleri*

Etkin olmayan adaylarınızı *Bekleyen adaylar*: altında görebilirsiniz.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-validator-3.png')} />
</div>

Bir adaylık aktif hale geldiğinde, onu *Aktif adaylıklar* listesinde bulmalısınız.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-validator-4.png')} />
</div>

:::Not

Adaylıklarınızı arada bir gözden geçirdiğinizden emin olun. Onaylayıcılarınızdan bazılarının komisyon yüzdelerini değiştirmesi, ödülleriniz üzerinde olumsuz bir etkisi olabilir. Adaylarınızın durumunu sık sık kontrol ederek, aday gösterilen doğrulayıcılarınızın listesini güncelleyerek tepki verebileceksiniz.

:::

## 04 Adaylıklarınızı ayarlayın {#04-adaylarınızı ayarlayın}

Doğrulayıcılarınızdan bazıları fazla abone olursa veya komisyonlarını değiştirirse, adaylıklarınızı ayarlamak isteyebilirsiniz.

Bunu yapmak için Polkadot/apps'i açın ve şuraya gidin:
*Ağ* > *Stake* > *Hesap işlemleri*

Hesap ayrıntılarınızın yanındaki üç noktayı tıklayın ve _Aday belirle_'yi seçin.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-set-nominees.png')} />
</div>

Aşağıdaki pencerede, zaten tanıdık gelebilir, doğrulayıcıları kaldırabilir ve/veya yenilerini ekleyebilirsiniz.
Adaylıklarınızı anında ayarlayabilirsiniz, aday göstermeyi bırakmanıza gerek yoktur. Değişiklikler, bir sonraki dönem başladığında (24 saat) uygulanacaktır.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide/nominate-validator-2.png')} />
</div>

## 05 Rebond fonları {#05-rebond-funds}

HDX jetonlarınızın bağını yanlışlıkla çözdüyseniz, 28 günlük bekleme süresi dolmadan onları yeniden bağlayabilirsiniz.

Bunu yapmak için Polkadot/apps'i açın ve *Developer* > *Extrinsics*'e gidin. Alternatif olarak, bu bağlantıyı takip edebilirsiniz:

https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/extrinsics

_Seçili hesabı kullanarak_ açılır menüsünde hesabınızı seçin. Bundan sonra, aşağıdaki bilgileri doldurmanız gerekir:

* **dışsal**: bahis
* **eylem**: rebond_value
* **değer**: buraya yeniden bağlamak istediğiniz HDX miktarını girmeniz gerekir.


<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/nominator-guide
