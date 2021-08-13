---
kimlik: node_setup
başlık: Doğrulayıcı Düğümü Ayarlayın
---

import useBaseUrl from '@docusaurus/useBaseUrl';

This section walks you through the process of setting up and running a HydraDX node.

:::warning

Doğrulayıcı düğümü çalıştırmak, düğümün doğru kurulumu ve çalışma süresini garanti etmek için gereken belirli bir teknik beceri seti gerektirir. Burada ne yaptığınızdan emin değilseniz, bunun yerine deneyimli bir doğrulayıcıya [HDX'inizi aday göstermenizi](/start_nomination) öneririz. Bunu yaparak, kendinizi ve aday gösterenlerinizi, istem dışı bir fon kaybına karşı korursunuz.
:::

## 00 Gerekli teknik özellikler {#00-gerekli-teknik-özellikler}

Bir HydraDX doğrulayıcı düğümünü çalıştırmak için minimum olarak aşağıdaki teknik özellikler gereklidir:

* İşletim Sistemi: Ubuntu 20.04
* CPU: Intel Core i7-7700K @ 4.5Ghz (veya eşdeğer tek çekirdek performansı)
* Bellek: 64GB RAM
* Depolama: En az 200 GB kapasiteli NVMe SSD (veri ayak izi zamanla büyüyecektir)

:::Not

Bunlar, ekip tarafından doğrulanan minimum teknik gereksinimlerdir. Makinenizin bir düğümü çalıştırmak için yeterli kaynağa sahip olduğundan emin olmak ister misiniz? Öğrenmek için bir [performans karşılaştırması](/performance_benchmark) çalıştırın.

:::


## 01 Sistem saatinizin senkronize olup olmadığını kontrol edin {#01-kontrol-sisteminizin-saati-senkronize olup olmadığını kontrol edin}

Bir düğümü çalıştırmadan önce sistem saatinizin senkronize olduğundan emin olmalısınız - bu önemlidir çünkü doğrulayıcılar birlikte çalışır. Ubuntu 20.04'te sistem saati varsayılan olarak senkronize edilmelidir. Doğrulamak için aşağıdaki komutu çalıştırın ve çıktıyı kontrol edin:

```bash
$ timedatectl | grep "System clock"
System clock synchronized: yes
```

Çıktı farklıysa, NTP'yi manuel olarak yükleyebilir ve sistem saatinizin senkronize olduğunu tekrar doğrulayabilirsiniz:

```bash
$ apt install ntp
$ ntpq -p
```

## 02 Güvenlik duvarı ayarlarınızı yapın {#02-güvenlik duvarınızı-ayarlarınızı yapın}
Bağlantı noktası '30333', diğer düğümlerle eşler arası bağlantılar için kullanılır. Düğümü doğrulayıcı olarak çalıştırıyorsanız, bir güvenlik duvarı kurmanızı ve yalnızca bu bağlantı noktasını uzak bağlantılar için gösterecek şekilde yapılandırmanızı öneririz.

Düğümü doğrulayıcı olarak *kullanmıyorsanız*, "9944"ü (harici uygulamalarla RPC websocket bağlantıları için) ve "9933"ü (düğümünüze yapılan HTTP istekleri için) göstermeyi de düşünebilirsiniz. [Polkadot/apps](/polkadotjs_apps_local) ile düğümünüze bağlanmak için '9944' bağlantı noktasını kullanabilirsiniz.

## 03 Bir ikili program indirin veya oluşturun {#03-download-or-build-a-binary}
En son sürümümüzün ikili dosyasını [github](https://github.com/galacticcouncil/HydraDX-node/releases) adresinden indirebilirsiniz.

Alternatif olarak, ikili dosyayı kaynaktan oluşturabilirsiniz:

```bash
# Install dependencies
$ curl https://getsubstrate.io -sSf | bash -s -- --fast

# Fetch source of the latest stable release
$ git clone https://github.com/galacticcouncil/HydraDX-node -b stable

# Build the binary
$ cd HydraDX-node/
$ cargo build --release
```

İkili dosyayı yukarıdaki adımları izleyerek oluşturduysanız, ikili dosyanızın yolu şudur:
```
target/release/hydra-dx
```

## 04 İkili dosyayı çalıştırın {#04-run-the-binary}
Aşağıdaki komutu yürüterek ikili dosyayı çalıştırabilirsiniz:

```bash
$ {PATH_TO_YOUR_BINARY} --chain lerna --name {YOUR_NODE_NAME} --validator
```

:::note

Doğrulayıcı düğümler, tüm zincir veritabanını gerektirir. Düğümü daha önce "--validator" bayrağı olmadan çalıştırdıysanız, düğümü başlatmadan önce zinciri temizleyerek veritabanını yeniden eşitlemeniz gerekir.
```bash
$ {PATH_TO_YOUR_BINARY} purge-chain --chain lerna
```

:::

İkili programınızın yolunun (yukarıya bakın) yanı sıra, [Telemetri](https://telemetry.hydradx.io/#/HydraDX%20Snakenet%20Gen2) içinde düğümünüzü tanımlamak için kullanılacak bir düğüm adı belirtmeniz gerekir. HydraDX Snakenet üzerinde çalışan tüm düğümlerin listesini bulabilirsiniz.

## 05 systemd ile çalıştırma {#05-running-with-systemd}
Makineniz yeniden başlatıldığında düğümünüzün otomatik olarak başlatıldığından emin olmak için HydraDX düğümünü bir systemd işlemi olarak çalıştırmanızı öneririz. Bunu yapmak için aşağıdaki dosyayı oluşturun ve içeriği `{VARIABLE}` olarak belirtilen değişkenleri değiştirirken ekleyin:

```bash
$ touch /etc/systemd/system/hydradx-validator.service
```

```
[Unit]
Description=HydraDX validator

[Service]
Type=exec
User={UNIX_USER}
ExecStart={PATH_TO_YOUR_BINARY} --chain lerna --name {YOUR_NODE_NAME} --validator
Restart=always
RestartSec=120

[Install]
WantedBy=multi-user.target
```

:::note
Bir kilitlenme durumunda düğümün yeniden başlatılmasını geciktirdiği için RestartSec'in ayarlanması önerilir. Bu, düğüm yeniden başlatılmadan önce kalıcı olmayan verilerin (konsensüs oyları gibi) diske yazılmasına izin verir. Düğümün çöktükten hemen sonra yeniden başlatılması, belirsizliğe veya çift imzalamaya neden olabilir ve sonunda eğik çizgi ile sonuçlanabilir.
:::

Yapılandırma dosyasını oluşturduktan sonra HydraDX doğrulayıcı düğümünüzle bir sistem işlemi olarak etkileşime girebilirsiniz:
```bash
# Start the node at system boot
$ systemctl enable hydradx-validator.service

# Start the node manually
$ systemctl start hydradx-validator.service

# Check the status of the node
$ systemctl status hydradx-validator.service

# Check the logs of the node
$ journalctl -f -u hydradx-validator.service
```

HydraDX düğümünüz artık yapılandırıldı ve çalışıyor!

Artık [doğrulamaya başlamak](/start_validating) için son adımları tamamlayabilirsiniz.
