---
id: staking
title: Staking
---

import useBaseUrl from '@docusaurus/useBaseUrl';

Bu bölüm, HydraDX ağında staking'in nasıl çalıştığına dair kısa bir giriş sağlar. Substrat tabanlı bir ağda stake yapmaya aşina değilseniz, katılmaya karar vermeden önce bunu okumanızı öneririz.

HydraDX tarafından kullanılan fikir birliği mekanizmasına Nominated Proof-of-Stake (NPoS) adı verilir. NPoS, Proof-of-Stake'in bir varyasyonudur ve Polkadot ve Kusama gibi Substrat tabanlı blok zincirlerinde kullanılır. Bir NPoS ortamındaki iki merkezi aktöre [**doğrulayıcılar**](#validators) ve [**nominators**](#nominators) adı verilir.

### Validators {#validators}

Doğrulayıcılar, HydraDX ağının güvenli bir şekilde çalışmasına izin veren altyapıyı sağlayan doğrulayıcı düğümleri çalıştırarak ağa katılır. Doğrulayıcı düğümler, fikir birliği mekanizması için büyük önem taşıyan üç işlevi yerine getirir. İlk etapta tarafların kimliği ve sözleşmenin konusu gibi bloklarda yer alan bilgileri doğrularlar. İkinci olarak, doğrulayıcılar, diğer doğrulayıcıların geçerlilik ifadelerine dayalı olarak yeni blokların üretimine katılırlar. Üçüncü olarak, blockchain işlemlerinin kesinliğini garanti ederler.

NPoS'un önemli bir özelliği, tüm doğrulayıcıların aynı anda doğrulama sürecine katılmamasıdır. Yalnızca *aktif doğrulayıcı kümesindeki* doğrulayıcılar yukarıda belirtilen işlemleri gerçekleştirir ve bunu yaptıkları için [ödüller](/staking_rewards) kazanır. Etkin doğrulayıcı kümesi, sabit sayıda düğümle sınırlıdır. [HydraDX Snakenet](/snakenet)'de bu sayının 300 civarında olmasını bekliyoruz ve ana ağa doğru ilerledikçe bu sayıyı artırıyoruz.

Doğrulayıcılar, *orantılı gerekçeli temsil* ilkesi izlenerek aktif kümeye seçilir. Bu ilke, mevcut yuvaları doğrulayıcılara aday gösterdikleri payla orantılı olarak atayarak ademi merkeziyetçiliği ve adil temsili korumayı amaçlar. Belirli bir doğrulayıcı ile stake edilen token miktarı ne kadar yüksek olursa, düğümün aktif sette seçilme şansı o kadar yüksek olur. Aktif kümeye dahil olmayan doğrulayıcılar bir bekleme listesine alınır. Aktif doğrulayıcılar seti her dönemin başında güncellenir ve yeni doğrulayıcılar için olası bir giriş penceresi sağlar.

:::note

Substrat tabanlı bir ağda zaman, **eras** adı verilen birimlere bölünür. [HydraDX Snakenet](/snakenet), *1 Era = 24 saat*.

:::

Doğrulayıcı olarak katılmak, bir doğrulayıcı düğümünü güvenli bir şekilde kurmak ve sürdürmek için belirli bir düzeyde teknik bilgi gerektirir. Doğrulayıcı düğümün yanlış davranışı, siz ve aday gösterenleriniz için istem dışı bir fon kaybıyla sonuçlanacak şekilde eğik çizgi ile cezalandırılabilir. Doğrulayıcı düğümü çalıştırmak için gerekli deneyime sahip olduğunuzu düşünüyorsanız [doğrulayıcı kılavuzumuza](/node_setup) başvurabilirsiniz. Aksi takdirde, aday olarak katılmayı düşünmenizi şiddetle tavsiye ederiz.

### Aday Gösterenler {# aday gösterenler}

Aday gösterenler, etkin doğrulayıcı kümesinde seçilecek doğrulayıcıları aday göstererek ağın güvenliğini sağlamaya yardımcı olur. Bunu, HDX jetonlarını kendi seçtikleri doğrulayıcılarla stake ederek yaparlar. Adaylık süreci, düğümlerin çalıştırılmasını ve bakımını gerektirmez, bu da bu tür stake etme biçimini herkes için daha erişilebilir hale getirir. Doğrulayıcıları aday göstermek için kullanılan jetonlar *bağlıdır*, yani bunlar dondurulur ve başka amaçlar için kullanılamaz. Mevcut dönemin sonunda yansıtılacak olan adaylıklarınızı herhangi bir zamanda değiştirmek veya durdurmak mümkündür. Aday gösterenler ayrıca jetonlarını serbest bırakabilirler, ancak bu yalnızca bağlayıcı olmayan işlemin ardından *28 günlük* bir bekleme süresinden sonra geçerli olacaktır.

Aday göstermeden önce, her zaman gerekli özeni göstermeli ve seçilen doğrulayıcıların güvenilirliğini araştırmalısınız. Bunu, kimliklerinin yanı sıra dönem puanları, seçilmiş bahis, ödüller ve eğik çizgiler gibi geçmiş bilgileri kontrol ederek yapabilirsiniz. Ancak Snakenet'in başlangıcında tüm bu bilgileri bulmak zor olabilir. Doğrulayıcıların seçimi konusunda şüpheniz varsa, bize Discord'dan ulaşın ve topluluk küratörlüğünde güvenilir doğrulayıcı listemizi sizinle paylaşalım.

Doğrulayıcı seçerken göz önünde bulundurulması gereken bir diğer nokta da *ödül komisyon yüzdesidir*. Bu, adaylara hizmetlerini sağladığı için doğrulayıcıya ödenecek ödüllerin oranını temsil eder. En düşük komisyon yüzdesi her zaman en iyisi değildir - performanslı ve kullanılabilir bir düğümü çalıştırmak, yalnızca gerçekçi bir ödül komisyonu talep ederek sürdürülebilir bir şekilde karşılanabilen yüksek operasyonel maliyetlere sahiptir.

HydraDX'te, bahis miktarınızla **maksimum 16 onaylayıcı** aday göstermek mümkündür. Bununla birlikte, birden fazla doğrulayıcıyı aday göstermek, bahsinizin her seferinde seçilen tüm doğrulayıcılara atanacağı anlamına gelmez. Bir sonraki dönem başladığında, Substrate, ağ içindeki tüm adayların en uygun dağılımını belirlemek için bir dizi karmaşık algoritma çalıştıracak ve nihai hedef, aktif doğrulayıcılar grubuna hangi doğrulayıcıların dahil edileceğine karar vermektir. Seçtiğiniz doğrulayıcılardan hiçbiri etkin kümeye dahil olmak için yeterli desteği alamazsa, **adaylarınız dönem boyunca (*24 saat*) etkin olmayacak** ve ayrıca bunun için hiçbir ödül almayacaksınız. dönem. Bahsinizin aktif doğrulayıcılar grubuna dahil olma şansını en üst düzeye çıkarmak için, **birkaç doğrulayıcı aday göstermenizi** şiddetle tavsiye ediyoruz, bu da ademi merkeziyetçiliği geliştirme çabalarımıza katkıda bulunacaktır.

:::note

Fazla abone olan doğrulayıcıları aday göstermediğinizden emin olun. Şu anda, tek bir doğrulayıcı için **64 adaylık sınırı** vardır ve sonrasında fazla abone olur. Bir sonraki dönem başladığında, fazla abone olan bir onaylayıcı yalnızca izin verilen maksimum aday sayısı kullanılarak seçilecektir. Böyle bir durumda, en yüksek paya sahip adaylar öncelik kazanırken, en düşük paya sahip adaylar dikkate alınmayacak ve bu süre boyunca herhangi bir ödül kazanmayacaktır.

Aday gösterme, daha erişilebilir bir stake etme şeklidir, ancak aynı zamanda riskleri de vardır. Ağın kurallarını ihlal eden doğrulayıcılar, hem doğrulayıcı hem de aday gösterenler için fon kaybına neden olacak şekilde eğik çizgi ile cezalandırılabilir. Bu nedenle yalnızca saygın doğrulayıcı düğümlerini aday göstermek önemlidir.

:::

Doğrulayıcıları aday göstererek HDX jetonlarınızı stake etmekle ilgileniyor musunuz? Aday göstermeye başlamak için [aday kılavuzumuza](/start_nomination) göz atın.
