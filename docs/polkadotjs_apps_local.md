---
kimlik: polkadotjs_apps_local
başlık: Bir Yerel Düğüme Bağlan
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

Yerel HydraDX düğümünüze bağlanmak için Polkadot/uygulamaları kullanabilirsiniz. Bu amaçla, RPC websocket bağlantıları için kullanılan '9944' bağlantı noktasına erişiminiz olmalıdır.

:::uyarı

Düğümü doğrulayıcı olarak çalıştırıyorsanız, uzak bağlantılar için '9944' bağlantı noktasını kara listeye almanızı kesinlikle öneririz. Bu bağlantı noktası, düğümünüzün performansını düşürmek için üçüncü taraflarca kötüye kullanılabilir, bu da kesintiye ve istemsiz para kaybına neden olabilir. Doğrulayıcı düğümünüze yalnızca düğüm yerel ağınızdayken bağlanmak için '9944' bağlantı noktasını kullanmalısınız.

:::

### Polkadot/apps kullanarak yerel düğümünüze erişme {#erişim-your-local-node-using-polkadotapps}

Düğümünüze erişmek için [Polkadot/apps](https://polkadot.js.org/apps/) dosyasını açın ve ağı değiştirmek için sol üst köşedeki simgesine tıklayın.

<div>
  <img src={useBaseUrl('/polkadotjs-apps/PolkadotJS-APPS-1.png')} />
</div>

Menüyü açtıktan sonra **Geliştirme**'ye tıklayın ve **Yerel düğüm**'ü seçin.
<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/polkadotjs-apps/local-1.png')} />
</div>

Gerekirse IP'yi ayarlayın ve yerel düğümünüze geçmek için ***Değiştir***'e tıklayın.

<div stili={{textAlign: 'center'}}>
  <img src={useBaseUrl('/polkadotjs-apps/local-2.png')} />
</div>

Artık yerel düğümünüze bağlı olmanız ve onunla etkileşime geçebilmeniz gerekir.
