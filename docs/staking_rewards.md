---
kimlik: stake_rewards
başlık: Stake Ödülleri
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

Stake ödülleri, doğrulayıcıları ve aday gösterenleri [HDX jetonlarını stake etmeye](/stake) teşvik eder. Bu makalede ele alınan üç tür bahis ödülü vardır: [temel ödüller](#temel ödüller), [era puanları](#era-puanları) ve [ipuçları](#ipuçları).

:::Not
Stake ödülleri, doğrulayanların ve aday gösterenlerin hesaplarına otomatik olarak dağıtılmaz. Bunun yerine, [bir ödemeyi tetikleyerek hak talebinde bulunulmalıdır](/staking_claim_rewards).
:::

## Temel Ödüller {#temel ödüller}

Her dönemin sonunda (24 saat), tüm aktif doğrulayıcı havuzları, HDX jetonları şeklinde temel ödüller alır. Doğrulayıcı havuzu, seçilmiş bir doğrulayıcıdan (kendi kendine bahis yapılan HDX'lerini elinde bulunduran) ve doğrulayıcıyı destekleyen tüm aktif adaylardan oluşur (daha fazla bilgi için bkz. [stake](/stake)). Nominated Proof-of-of-Stake (NPoS) konsensüs mekanizmasının temel bir ilkesi, **eşit çalışmanın eşit ödüller getirmesidir**. Diğer bir deyişle, tüm doğrulayıcı havuzları temelde aynı işi yaptığından, **mevcut temel ödüller aralarında eşit olarak bölünür**. Bu, doğrulayıcı havuzlarının, geleneksel PoS ağlarından önemli bir fark olan, toplam paylarıyla orantılı olarak **ödüllendirilmediği** anlamına gelir.

Temel ödüllerin tüm katılımcı doğrulayıcı havuzları arasında eşit olarak paylaşılması mekanizması, gücün birkaç doğrulayıcı havuzunda yoğunlaşmasını önleyerek ağın güvenliğine katkıda bulunur ve böylece ademi merkeziyetçiliği güçlendirir. Zamanla, adayları daha küçük bir HDX hissesiyle doğrulayıcıları aday göstermeye teşvik eder. Bu süreç sonunda ağdaki güç ilişkilerini dengeleyecek ve tüm doğrulayıcı havuzlarının kabaca eşdeğer miktarda stake edilmiş HDX'e sahip olduğu bir duruma yol açacaktır.

Ödüllerin dağılımı şu şekilde gerçekleşir. Her doğrulayıcı havuzu için (eşit) ödül miktarını hesapladıktan sonra, doğrulayıcı, düğümün bakımı için **komisyon ücretleri** biçimindeki payını alır. İkinci adım olarak, kalan jetonlar tüm hisseler arasında **orantılı olarak** (doğrulayıcının kendi hissesi dahil) dağıtılır. Bu, daha yüksek bahislerin, belirli doğrulayıcı havuzuna atfedilen ödüllerin daha büyük bir kısmını alacağı anlamına gelir.

:::Not
Snakenet adlı teşvikli test ağımızda, HDX jetonlarınızı stake etmek için alınan ödül miktarının **%50 APY** civarında olduğu tahmin edilmektedir.
:::

## Çağ noktaları {#dönem noktaları}

Doğrulayıcılar, geçmiş dönemde kazandıkları dönem puanları ile orantılı olarak ek ödüller kazanabilirler. Bu ödüller, yukarıda açıklanan temel ödüllere eklenir. Doğrulayıcılar, aşağıdakiler gibi belirli belirli eylemleri gerçekleştirerek dönem puanları kazanabilirler:

* Röle Zincirinde amca olmayan bir blok üretmek.
* daha önce referans verilmemiş bir amca bloğuna referans üretmek.
* başvurulan bir amca bloğu üretmek.

:::Not
Amca bloğu, her bakımdan geçerli olan ancak kanonik hale gelememiş bir Relay Chain bloğudur. Bu, iki veya daha fazla doğrulayıcı tek bir yuvada blok üreticisi olduğunda ve bir doğrulayıcı tarafından üretilen blok diğerlerinden önce bir sonraki blok üreticisine ulaştığında olabilir. Gecikmeli bloklara amca blokları denir.
:::

## İpuçları {#ipuçları}

Son olarak, doğrulayıcılar, her dönemin sonunda temel ödüllere de eklenen ipuçları kazanabilirler. İpuçları, işlemlerine daha yüksek bir öncelik vermek için isteğe bağlı olarak kullanıcılar tarafından ödenebilecek ek bir işlem ücretini temsil eder.
