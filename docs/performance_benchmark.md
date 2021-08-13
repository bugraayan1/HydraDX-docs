---
kimlik: performans_benchmark
başlık: Performans Karşılaştırması
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

Bir performans karşılaştırması çalıştırarak makinenizin [gerekli teknik özellikleri](/node_setup#00-required-technical-specations) karşıladığından emin olabilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin:

```bash
# Fetch source of the latest stable release
$ git clone https://github.com/galacticcouncil/HydraDX-node -b stable
$ cd HydraDX-node/

# Prepare for running the benchmark
## Install Rust following https://rustup.rs
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

## Configure Rust
$ ./scripts/init.sh
$ rustup default nightly

## Install additional libraries
$ apt install python3-pip
$ apt install clang

# Run the benchmark
$ ./scripts/check_performance.sh
```

Kıyaslama yürütüldükten sonra aşağıdakine benzer bir çıktı görmelisiniz:

```
         Pallet          |   Time comparison (µs)    |  diff* (µs)   |   diff* (%)    |            |   Rerun
amm                      |     773.00 vs 680.00      |      93.00    |      12.03     |     OK     |
exchange                 |     804.00 vs 720.00      |      84.00    |      10.44     |     OK     |
transaction_multi_payment|     218.00 vs 198.00      |      20.00    |       9.17     |     OK     |

Notlar:
- fark alanlarında makinenizin referans kıyaslama zamanı ile kıyaslama zamanı arasındaki farkı görebilirsiniz
- üç paletin tümü için fark pozitifse, makineniz bir HydraDX düğümünü çalıştırmak için minimum gereksinimleri karşılar
- bazı paletler için fark -%10 veya daha fazla sapma gösteriyorsa, makineniz bir düğümü çalıştırmak için uygun olmayabilir
```

Makineniz ile gerekli minimum kurulum arasındaki performans farkını **fark* (%)** sütununda görebilirsiniz. Bu sütundaki üç değerin tümü pozitifse, makineniz bir HydraDX doğrulayıcı düğümü çalıştırmaya uygun olmalıdır. Değerlerden herhangi biri *-10 %*'un altındaysa, HydraDX düğümünün çalıştırılmasını önermeyiz.

Karşılaştırma sonuçlarınızı tartışmak isterseniz Discord'da bize katılın, topluluğumuz her zaman yardımcı olmaktan mutluluk duyar.
