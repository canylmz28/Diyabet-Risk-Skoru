# Diyabet-Risk-Skoru

# DİYABET

Diyabet günümüz dünyasında en önemli sağlık sorunlarından biri olarak kabul edilmektedir. Dünya genelindeki artışa paralel olarak ülkemizde de görülme sıklığı ve hasta sayısı hızla artmaktadır. Bulaşıcı olmadığı hâlde salgın yapan bu hastalık tüm ülkelerde genellikle erişkin yaş grubunu etkilemekte, hem doğrudan hem de dolaylı etkileri ile sağlık sistemlerini ve toplumsal yaşamı tehdit etmektedir. Diyabetin görülme sıklığı ülkeler ve toplumlar arasında farklılık göstermekle birlikte, hasta sayısında ve hastalığın görülme oranındaki artış ve diyabet kaynaklı durumların ölüm nedenleri listesindeki yeri pek çok toplumda benzerlik göstermektedir.
Diyabet, insülin eksikliği ya da insülinin kullanımındaki sorunlar nedeniyle organizmanın karbonhidrat, yağ ve proteinlerden yeterince yararlanamadığı, sürekli tıbbi bakım gerektiren, kronik bir metabolizma hastalığıdır. Hastalığın, akut komplikasyon riskini azaltmak ve uzun dönemde tedavisi pahalı kronik sekellerinden korunmak için sağlık çalışanları ve hastaların sürekli eğitimi şarttır.

Bu çalışmanın amacı bir kişinin diyabet risk skoru değerini hesaplamaktır. Bir kişinin verilerine göre diyabet risk durumunun düşük, normal ve yüksek olup olmadığını görmeyi amaçladık.

# VERİLERİN ELDE EDİLMESİ VE VERİ SETİ TANIMI

Veri setimizde 190.655 değer ve 20 değişken vardır. Veri seti içinde aynı hastanın birden fazla değeri bulunmaktadır. İlk olarak veri setinde hasta numaralarının bulunduğu sütunda tekrarlanan satırlar veri setinden çıkarılmıştır. Böylece yeni veri setimizin boyutu 53.762’dir.
Veri seti içinde bulunan değişkenlerin ismi, türü ve açıklaması aşağıdaki tabloda gösterilmiştir.

![image](https://user-images.githubusercontent.com/71662622/127652693-b36859d1-0afd-457c-9f60-b079332ea6b9.png)

 
HastaId ve ProtokolNo değişkeni makine öğrenmesi algoritmamızda modelin ezberlemesine sebep olacağı için veri setinden çıkarılacaktır. Cinsiyet dağılımı incelendiği zaman hastaların %51.5’i erkek, %48.5 ise kadındır. Yaş değişkenine sahip olduğumuz için hasta doğum tarihine ihtiyacımız yoktur. Yaş değişkeni incelendiği zaman yoğunluğun en fazla olduğu yaş aralığı 50-70 yaş aralığıdır. Hastaların geliş tipi incelendiği zaman %88.3’ü yatarak, %11.7’si ise günübirlik tedavi olmuştur.  BransAdi, AcilisTarihi, KapanisTarihi, LoincKodu, EkOnayTarihi, IstekTarihi değişkenleri anlamlı sonuçlar üretmeyeceği için veri setinden kaldırılmıştır. EkIslemAdi ve EkSonucText değişkeninden yapılan testlerin diyabet risk sınırına göre diyabetli ya da diyabetli değil ayrımı yapılacaktır. ICDKodu değişkeninden hastalara konulan tanılara göre kronik hastalıkların tespiti yapılacaktır. YatisGun değişkeninde aykırı değerler temizlenip modele eklenecektir. GebelikSayisi ve BMI değişkenlerinin boş değerlerinin sayısı çok fazla olduğu için veri setinden çıkarılacaktır. Soygecmis değişkeni incelendiğinde hastaların büyük çoğunluğunun soy ağacında özellik olmadığından dolayı makine öğrenmesi modeline dahil edilmeyecektir.

# VERİ TEMİZLEME İŞLEMLERİ

İlk olarak EkSonucText değişkeninde sayısal olmayan (<0.200, >1000, Grup Test) gibi değerler ve boş değerler veri setinden çıkarılmıştır. Böylece veri setimizin boyutu 190.370’e düşmüştür. HastaId sütununda tekrarlanan satırlar veri setinden çıkarılmıştır. Böylece yeni veri setimizin boyutu 53.762 olmuştur. 
Bu işlemden sonra ICDKodu değişkenine göre tanı konulan hastalıklardan hangilerinin diyabet hastalığı olduğunu bulup diyabet hastalığı konulan hastaların yeni oluşturacağımız Target isimli değişken değerine 1 atayacağız. Eğer diyabet tanısı yok ise Target değişkeni değerine 0 atayacağız. ICD kodları incelendiği zaman “E” ile başlayan her kod diyabet hastalığını göstermektedir. Bu işlemi yaptığımız zaman diyabetli olanların sayısı 5.301, diyabet hastası olmayanların sayısı ise 48.461’dir. Fakat veri seti incelendiği zaman diyabet hastası olmayan hastanın test sonuç değerinin sınırın üstünde olduğu görülür. Bizim aynı zamanda yapılan testlere göre sonuçları gruplandırmamız gerekir. 
Ek işlem testlerin yapıldığı sütundan sadece glikoz açlık test sonuçlarının olduğu satırları seçeceğiz. Çünkü veri setinde glikoz açlık test sayısı 51.980 tanedir. Diğer yapılan testlere göre sayısı oldukça fazladır. Glikoz açlık testi yaptıran hastaları seçtikten sonra EkIslemAdi değişkenini veri setinden kaldırdık. Test sonuçlarına göre gruplandırma yapmadan önce aykırı değerlerin temizlenmesi yapıldı. Böylece yeni veri setimizin boyutu 46.928 oldu. Açlık test sonucu 125’ten küçük olanlar diyabetli değil, 125’ten büyük olanlara ise diyabet hastası olarak etiketlendirdik. Diyabet hastası olarak etiketlenen hastaların Target değeri 0 ise bu değeri 1 olarak değiştirdik. Yeni Target değişkeni incelendiği zaman hastaların 34.291 tanesi diyabet hastası değil, 12.637 tanesi ise diyabet hastasıdır. 
Yaş değişkenini ergen, genç, orta yaş ve yaşlı olarak gruplandıracağız. Yaşı 0-17 arasında olanlara ergen, 18-65 arasında olanlara genç, 66-79 arasında olanlara orta yaş, 80-120 arasında olanlara ise yaşlı etiketi atıldı. Yaş grupları incelendiği zaman hastaların 25.272 tanesi genç, 9.868 tanesi orta yaş, 8.269 tanesi ergen ve geri kalan 3.505 tanesi ise yaşlıdır. 
Hipertansiyon ve obezite olmak üzere iki yeni değişken oluşturduk. ICD kodlarına göre hipertansiyon ya da obezite tanısı konulan hastaların bu değişken değerlerine 1  değeri atandı. 
Tansiyon değişkeninde veri temizliği yapmak için ilk olarak tansiyon değerlerini “/” ifadesine göre ayırıp büyük tansiyon ve küçük tansiyon olmak üzere iki yeni sütun oluşturuldu. Tansiyondaki boş değerleri doldurmak için cinsiyet ve yaş gruplarına göre gruplandırıp en çok geçen tansiyon değerleri atanmıştır. Tansiyon değerlerinin normalli ya da anormalliği yaş ve cinsiyete göre farklılık gösterdiği için hastaların yaş ve cinsiyet değerlerine göre tansiyonların düşük, normal ya da yüksek olduğu tespit edilmiştir. 

# VERİ DÖNÜŞTÜRME İŞLEMLERİ

Veri dönüştürme işlemlerinde ilk olarak hipertansiyon ve obezite değişkenlerinde boş olan satırlara 0 atanmıştır. Makine öğrenmesi algoritmalarında değişkenler sayısal olması gerektiği için kategorik değişkenlerin sayısal forma dönüştürülmesi gerekir. Cinsiyet değişkeninde erkek olanlara 1, kadın olanlara 0 atanmıştır. Geliş tipi değişkeninde yatan hastalara 1, günübirlik hastalara ise 0 atanmıştır. Büyük tansiyon ve küçük tansiyon değişkenini sayısallaştırma işleminde daha farklı bir method uygulanmıştır. Tansiyon değişkeni incelendiği zaman düşük, normal ve yüksek olarak 3 gruba ayrılmıştır. Bu iki değişken kendi içinde sıralama niteliğine sahip olduğu için ordinal encoder yöntemi kullanılacaktır. Gerekli düzenleme yapıldığında büyük tansiyon ve küçük tansiyon için 0:Düsük < 1:Normal < 2:Yüksek olarak sayısallaştırılmıştır. Son olarak yaş sınıfı değişkeni sayısallaştırılmıştır. Bu değişkende OneHotEncoder yöntemi kullanılmıştır. Diğer yöntemlerden farklı olarak değişkende bulunan gözlem değerlerinin sayısı kadar 1 ve 0’lardan oluşan yeni çoklu değişken oluşturulmuştur. 
Değişken dönüşümü işlemini tamamladıktan sonra test sonuçlarının olduğu EkSonucText değişkeni ile yatış gün sayısını belirten YatisGun değişkenindeki aykırı değerler temizlenmiştir. Böylece veri temizleme ve dönüştürme işlemleri tamamlanmıştır.

# MODEL OLUŞTURMA

Risk skoru hesaplamak için yapılmış çalışmalar incelenmiştir. Yapılan çalışmalardan esinlenerek bu çalışmada Lojistik Regresyon algoritması kullanılmıştır. Algoritmaya girdi olarak verilecek değişkenler standartlaştırılmıştır. Bu işlemden sonra statsmodel ile lojistik regresyon algoritması uygulanıp değişkenlerin anlamlılığı ve regresyon modelinin katsayıları incelenmiştir. Değişkenlerin anlamlılığı incelendiği zaman cinsiyet, geliş tipi, test sonuçları, yatış gün sayısı, hipertansiyon, obezite ve küçük tansiyon değişkenleri seçilmiştir. Risk skoru hesaplamak için her bir satırdaki değerler ile değişkenlerin katsayı değerleri çarpılıp tüm değerler toplanmıştır. Böylece skor değerleri elde edilmiştir. Bu skor değerleri veri setine Risk_Grubu değişkeni olarak eklenmiştir.
 
 # BULGULAR

Risk grubu değişkeni ile target değişkeni incelenmiştir. Risk grubuna göre target değişkenindeki değişim incelenip risk grubu etiketi belirlenmiştir. Risk grubu 2 ve 3 olanların risk grubu etiketi “düşük risk grubu”, 4 olanların etiketi “orta risk grubu” ve diğer risk grubunda olanların risk grubu etiketi ise “yüksek risk grubu” olarak belirlenmiştir. 
Risk grubu etiketi incelendiği zaman 24.031 kişi düşük risk grubunda, 7.961 kişi orta risk grubunda ve 7.644 kişi ise yüksek risk grubundadır. Risk gruplarının diyabet olup olmama durumuna göre gruplanmış hali aşağıdaki şekilde gösterilmiştir. 

![image](https://user-images.githubusercontent.com/71662622/127652805-5afe90f0-846b-4fb9-b024-ba5e9eaeb0aa.png)

 
