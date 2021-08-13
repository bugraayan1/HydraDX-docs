---
kimlik: tip_request
başlık: Hazine İpucu İsteyin
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

[HydraDX New Deal teşvik programının](#link-to-new-deal) başlatılmasıyla, topluluk üyeleri katkılarından dolayı bir ödül olarak Hazine'den HDX ipuçları talep edebilir. Bu kılavuz, bahşiş istekleri sürecinde size yol gösterir. Ödüllendirilen farklı etkinlik türleri hakkında daha fazla bilgiyi [bu gönderide](/new_deal) bulabilirsiniz.

Hazine bahşişi talep etme süreci iki adımdan oluşur. İlk adımda, katkıda bulunanların bahşiş taleplerini katkının bir açıklamasıyla Commonwealth.im'de yayınlamaları gerekir. İkinci adım olarak, Hazine bahşiş talebi, Polkadot/apps kullanılarak zincir üzerinde gönderilmelidir.

## 01 Bahşiş İsteğini Commonwealth.im'de Yayınlayın {#01-publish-tip-request}

Şeffaflığı korumak için tüm Hazine bahşiş talepleri [HydraDX Commonwealth sayfasındaki](https://commonwealth.im/hydradx) bir ileti dizisinde yayınlanmalıdır. Bir ileti dizisi açmadan önce, HydraDX cüzdanınızı Commonwealth.im'e bağlamanız gerekir: *Giriş yap* (sağ üst köşe) öğesine tıklayın ve *Cüzdanla bağlan > polkadot-js* öğesini seçin.

<div stili={{textAlign: 'center'}}>
  <img alt="login" src={useBaseUrl('/tip-request/login.jpg')} width="300px" />
</div>

Açılır pencerede HDX adresinizi seçtikten sonra, cüzdanınızı kullanarak mesajı imzalamanız ve bu cüzdan için bir görünen ad belirlemeniz istenecektir.

Giriş yaptıktan sonra, bahşiş talebiniz için yeni bir konu oluşturmanız gerekir. Sayfanın sağ üst kısmına gidin ve *Yeni konu > Yeni konu* üzerine tıklayın. Bu bağlantıyı doğrudan da kullanabilirsiniz: https://commonwealth.im/hydradx/new/thread.

Konu başlığını (bahşiş talebi) ve katkının konusunu belirtmek için kullanabilirsiniz. Konunun gövdesinde lütfen aşağıdaki bilgileri sağlayın:

* Katkının yapıldığı dönem
* Katkının kısa bir özeti
* İstenen bahşiş miktarı (HDX olarak)
* Katkı için harcanan süre (saat olarak)
* Uygunsa, HydraDX'e yapılan katkının alaka düzeyi ve PR bağlantısı gibi daha ayrıntılı bilgiler sağlayın (teknik katkı olması durumunda)

Referans olarak, [bu örnek ipucu talebine](https://commonwealth.im/hydradx/proposal/discussion/1165-tip-request-add-documentation-for-stake) göz atabilirsiniz.

Bilgileri doldurduktan sonra, konuyu yayınlayın ve genel listede mevcut olmalıdır.

:::Not

HDX'lerini aşırı bağlamış ve "sıkışmış" olan adaylar ve doğrulayıcılar, bazı jetonlarının bağını çözmelerine izin verecek 1 HDX'lik bir Hazine bahşişi talep edebilir. Bu durum sizin durumunuz için geçerliyse lütfen [bu örneği](https://commonwealth.im/hydradx/proposal/discussion/1166-tip-request-overbonded-staker) izleyerek bir Commonwealth ileti dizisi oluşturun.

:::

## 02 Zincir Üzerinde Bahşiş Talebi Gönderin {#02-zincirde gönder}

Hazine bahşiş talebinizi yayınladıktan sonra zincir üzerinde göndermeniz gerekir. Bu amaçla, cüzdanınızın işlem ücretlerini karşılamak için az miktarda HDX tutması gerekir. Halihazırda herhangi bir HDX'e sahip değilseniz, bu adımı uygulamanıza gerek yoktur - başka biri sizin için zincir üzerinde bahşiş talebinizi gönderecektir.

Hazine bahşişi talepleri, aşağıdaki bağlantı kullanılarak Polkadot/apps ile zincir üzerinde gönderilebilir: https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/ hazine/bahşiş.

Yeni bir bahşiş talebi göndermek için *Tip öner* seçeneğine tıklayın ve aşağıdaki bilgileri sağlayın:

* **hesapla gönder** - zincir üzerinde bahşiş talebini göndermek için işlemi imzalayacak hesabı seçin. Bu hesabın işlem maliyetleri için az miktarda HDX tutması gerekir.
* **lehdar** - Hazine bahşişini alacak hesabın adresini seçin veya girin. Bu hesap, Commonwealth dizisini açan hesaba karşılık gelmelidir.
* **ipucu nedeni** - Commonwealth ileti dizisine bir URL sağlayın.

<div stili={{textAlign: 'center'}}>
  <img alt="login" src={useBaseUrl('/tip-request/submit-on-chain.jpg')} />
</div>

Bahşiş talebini göndermek için *Bahşiş öner*'e tıklayın ve işlemi imzalayın.

İşlem onaylandıktan sonra, genel bakış sayfasında bahşiş talebini görmelisiniz.

<div stili={{textAlign: 'center'}}>
  <img alt="login" src={useBaseUrl('/tip-request/tip-requests.jpg')} />
</div>
