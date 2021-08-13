---
id: stake_claim_rewards
başlık: Stake Ödüllerinizi talep edin
---

useBaseUrl dosyasını '@docusaurus/useBaseUrl'den içe aktarın;

Her dönemin sonunda, doğrulayıcı havuzlarına temel ödüller, dönem puanı ödülleri ve ipuçlarından oluşan [stake ödülleri](/staking_rewards) atanır. Ancak bu ödüller, doğrulayıcının ve aday gösterenlerin hesaplarına otomatik olarak dağıtılmaz. Bu, yalnızca bahis ödüllerinin **ödemeyi tetikleyerek** talep edilmesinden sonra gerçekleşir. Staking ödülleri, kazanıldıktan sonra **84 dönem içinde** talep edilmelidir. Bu süre sona erdiğinde, ilgili ödül bilgileri zincirden silinir ve doğrulayıcı havuzu artık o dönem için ödüllerini alamaz.

Sınırlı bir zaman çerçevesi içinde bir ödemeyi manuel olarak tetikleme süreci, önemli bir güvenlik özelliğidir. Her doğrulayıcı havuzu ve her dönem için bir ödeme işleminin gönderilmesini zorunlu kılarak, ödüllerin dağıtımı birkaç bloğa yayılır. Tüm ödüller tek bir blok içinde tüm doğrulayıcılara ve aday gösterenlere dağıtılacak olsaydı, zincirin istikrarı muhtemelen tehlikeye girebilirdi.

## Ödeme nasıl tetiklenir
Polkadot/apps kullanılarak hem doğrulayıcılar hem de aday gösterenler tarafından bir ödeme kolayca tetiklenebilir. Bunun için *Ağ > Staking > Ödemeler*'e gidin. Alternatif olarak, aşağıdaki bağlantıyı kullanabilirsiniz:
https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-01.snakenet.hydradx.io#/staking/payout

*Depolarım*'ı seçerken, ilgili dönem numaralarının bir göstergesiyle birlikte stake edilen jetonlarınız için ödenebilecek tüm ödülleri görmelisiniz. *Tümünü öde*'ye tıklayarak, geçmiş dönemler için mevcut tüm ödülleri talep etmek için bir dizi işlem göndermek mümkündür.

<img src={useBaseUrl('/stake-claim-rewards/payouts.jpg')} />

Ödemeyi tetikledikten sonra, HDX hesabınızı kullanarak işlem(ler)i imzalamanız istenecektir. Onaylandıktan sonra, seçilen dönemlerin ödülleri ilgili doğrulayıcılara ve aday gösterenlere dağıtılacaktır.
