---
kimlik: start_validating
başlık: Doğrulayıcı Olun
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

[HydraDX düğümünüzü kurduktan](/node_setup) sonra, doğrulamaya başlamadan önce HDX belirteçlerini bağlamanız ve doğrulayıcı anahtarlarını ayarlamanız gerekir.

:::uyarı

Doğrulayıcı düğümü çalıştırmak, düğümün doğru kurulumu ve çalışma süresini garanti etmek için gereken belirli bir teknik beceri seti gerektirir. Ayrıca, doğrulayıcıların her zaman en son kararlı sürümü kullanarak düğümü çalıştırmasını şart koşuyoruz. Burada ne yaptığınızdan emin değilseniz, bunun yerine deneyimli bir doğrulayıcıya [HDX'inizi aday göstermenizi](/start_nomination) öneririz. Bunu yaparak, kendinizi ve aday gösterenlerinizi, istem dışı bir fon kaybına karşı korursunuz.

:::

## 01 Bond HDX belirteçleri {#01-bond-hdx-belirteçleri}

Ağda doğrulayıcı düğüm olarak yer almak için bir miktar HDX jetonunu bağlamanız gerekir. Herhangi bir HDX'iniz yoksa, testnet'in _initial_ aşamasına katılmanız mümkün değildir. Bununla birlikte, önümüzdeki haftalarda ekip tarafından bazı heyecan verici haberler duyurulacak, bu nedenle haberdar olun ve bültenimize abone olun.

:::Not

Balancer LBP etkinliği sırasında satın aldığınız xHDX jetonlarına hâlâ sahip misiniz? Devam etmeden önce [HDX'inizi talep etmeniz](/claim) gerekir.

:::

HDX'i bağlamak için Polkadot/apps'i açın ve [genel HydraDX RPC düğümlerinden](/polkadotjs_apps_public) birine bağlanın. Hesabınızı [bakiye](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/accounts) görebildiğinizden emin olun.

:::uyarı

Ağın güvenliğini garanti etmek için bağlı HDX belirteçleri tehlikede. Doğrulayıcı düğümün uygunsuz davranışı, istemsiz bir fon kaybına yol açabilecek kesme ile cezalandırılabilir. Yalnızca ne yaptığınızı gerçekten biliyorsanız ilerlemenizi şiddetle tavsiye ederiz.

:::

Bir sonraki adım için *Ağ* > *Stake* > *Hesap işlemleri* > *+ Zula* seçeneğine gidin

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/bond-hdx-1.png')} />
</div>

Stash düğmesine tıkladıktan sonra, dört düzenlenebilir alanla birleştirme tercihlerini görmelisiniz:
* **saklama hesabı**: HDX jetonlarınızın çoğunluğunu tutan hesap. HDX bu hesaptan stake edilecektir.
* **kontrolör hesabı**: doğrulama sürecini başlatmak ve durdurmakla ilgili ücretleri karşılamak için gereken HDX'in daha küçük bir bölümünü tutan bir hesap.
* **bağlı değer**: bağladığınız HDX miktarı. Sahip olduğunuz tüm HDX'leri bağlamayın - bunun yerine daha sonra oluşacak işlem ücretlerini karşılamak için biraz bırakın.
* **ödeme hedefi**: doğrulayan ödüllerin gönderileceği hesap.

:::uyarı

Mevcut tüm HDX belirteçlerinizi bağlamayın. İşlem ücretlerini karşılamak için küçük bir rezerv bırakın. Sahip olduğunuz tüm HDX belirteçlerini bağlarsanız, doğrulama sürecini başlatmak için işlemi imzalayamayabilirsiniz.

:::

Bonding tercihlerini ayarladıktan sonra, bonding işlemini tamamlamak için _Bond_'a tıklayın ve işlemi imzalayın.

:::Dikkat

Güvenlik nedeniyle, aynı Stash ve Controller hesaplarına sahip olmanız önerilmez. Ancak Snakenet'te transferler devre dışı bırakıldığı için şu anda ayrı hesaplar kullanmak mümkün değil. Gelecekte bu mümkün olur olmaz ayrı Stash ve Controller hesaplarına geçmenizi şiddetle tavsiye ederiz.

:::

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/bond-hdx-2.png')} />
</div>

## 02 Oturum anahtarları oluşturun {#02-generate-session-keys}

İkinci adım, oturum anahtarlarınızı oluşturmaktır. Oturum anahtarları, doğrulayıcı düğümünü Denetleyici hesabınız ve stake edilen HDX ile ilişkilendirmek için kullanılır. Bu nedenle doğru şekilde kurulmaları önemlidir.

Oturum anahtarlarınızı oluşturmak için düğümde çalıştırın:

```bash
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http:/ /localhost:9933

# Örnek çıktı
{ "Jsonrpc": "2.0", "Sonuç": "0x9257c7a88f94f858a6f477743b4180f0c9a0630a1cea85c3f47dc6ca78e503767089bebe02b18765232ecd67b35a7fb18fc3027613840f27aca5a5cc300775391cf298af0f0e0342d0d0d873b1ec703009c6816a471c64b5394267c6fc583c31884ac83d9fed55d5379bbe1579601872ccc577ad044dd449848da1f830dd3e45", "kimlik": 1}
```

Oturum anahtarlarınızı çıktının _result_ bölümünün altında bulabilirsiniz (yukarıdaki örnek çıktıda `0x9257...`).

## 03 Oturum anahtarlarınızı ayarlayın {#03-oturumunuzu-anahtarlarınızı} ayarlayın

Oluşturulan oturum anahtarlarını Denetleyici hesabınızla ilişkilendirmek için Polkadot/uygulamalarda aşağıdaki menü öğesine gidin:
*Geliştirici* > *Dışsal Özellikler*

Alanları doldurun:

* _seçilen hesabı kullanarak_: Denetleyici hesabınızı seçin;
* _şu dışsal iletiyi gönderin: sol tarafta 'oturum'u ve sağ tarafta 'setKeys'i seçin;
* _keys_: önceki adımdaki oturum anahtarlarınızı girin;
* _kanıt_: "0".

Tamamlamak için _İşlemi Gönder_'i tıklayın ve işlemi imzalayın.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/set-session-keys-1.png')} />
</div>

## 04 Düğümünüzün fu olduğundan emin olun

lly senkronize edildi {#04-düğümünüzün-tamamen senkronize edildiğinden emin olun}

Devam etmeden önce, düğümünüzün çalıştığından ve senkronizasyon işleminin tam olarak tamamlandığından emin olmalısınız. Senkronizasyon durumunu kontrol etmenin en kesin yolu doğrudan düğümün kendisindedir:

```bash

$ Journalctl -f -u hidradx-validator.service

# Çıktı buna benzer olacak
22 Mart 18:37:38 Ubuntu-2010-groovy-64-minimal hydra-dx[232761]: 2021-03-22 18:37:38 💤
Boşta (52 eş), en iyi: #622028 (0x5f5a…1041), kesinleştirildi #622025 (0x5b21…a746), ⬇ 9.1kiB/s ⬆ 6.1kiB/s

```

Çıktıdaki blok numarasını (yukarıdaki örnekte: "#622025") [Polkadot/apps Explorer](https://polkadot.js.org/apps/? rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/explorer). Yazma sırasında, mevcut blok "#622240" şeklindedir, bu, örnek için kullanılan düğümün tam olarak senkronize edilmediği anlamına gelir.

Lütfen yerel günlüklerinizde gösterilen blok numarası ağın mevcut blok numarasıyla eşleşene kadar bekleyin.

## 05 Doğrulamaya başlayın {#05-start-validating}

Doğrulamaya başlamak için Polkadot/apps'de gezinin:

*Ağ* > *Stake* > *Hesap işlemleri* > *Doğrula* (bağlı HDX'inizin yanındaki düğme)

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/validate-1.png')} />
</div>

*Doğrula* düğmesine tıkladıktan sonra *doğrulayıcı tercihlerini ayarla* adlı bir açılır pencere görmelisiniz. Burada, _ödül komisyon yüzdenizi_ ayarlamanız gerekir. Bu, size ödenecek ödüllerin oranıdır. Kalan ödüller, adaylarınız arasında paylarına göre paylaştırılacaktır. Herhangi bir ödül komisyonu almamaya karar verirseniz, yüzdeyi 0 olarak ayarlayabilirsiniz.

Onaylamak için *Onayla*'ya tıklayın ve işlemi imzalayın.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/validate-2.png')} />
</div>

## 06 Doğrulayıcı düğümünüzün durumunu kontrol edin {#06-doğrulayıcı düğümünüzün durumunu kontrol edin}

Doğrulayıcı düğümünüzün durumunu aşağıdaki Polkadot/uygulamalarda kontrol edebilirsiniz:

*Ağ* > *Stake* > *Stake'e genel bakış*

Bu sekme, ağa bağlı tüm aktif doğrulayıcılara genel bir bakış sağlar. En üstte, mevcut doğrulayıcı yuvalarının miktarının yanı sıra doğrulayıcı olma niyetlerini bildiren düğümlerin sayısının bir göstergesi vardır. _Bekliyor_ sekmesine tıklayarak düğümünüzün bekleme kuyruğunda olup olmadığını onaylayabilirsiniz.

Doğrulayıcı düğümünüz, doğrulayıcı kümesine dahil edilmek üzere seçilene kadar bekleme kuyruğunda kalacaktır. Doğrulayıcı seti, boş yuvalar olması koşuluyla, yeni düğümlerin dahil edilmesine izin veren her dönemde güncellenir.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/validator-guide/validate-3.png')} />
</div>

Snakenet doğrulayıcısı olarak HydraDX'i desteklediğiniz için teşekkür ederiz! 🎉
