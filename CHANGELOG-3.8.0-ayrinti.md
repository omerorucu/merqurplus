# MerQuR+ 3.8.0 — değişiklik listesi (2026-11-20)

Yeni şablon: **NIST SP 800-22 rastgelelik testleri** (`nist_rastgelelik`): 15 test ve ikinci düzey çözümleme.

Bu sürüm, şablon kütüphanesinin satır satır incelenmesinde bulunan kusurların düzeltmelerini ve yeni form seçeneklerini içerir. Yeni seçeneklerin varsayılanı önceki davranıştır; sonucu değiştiren her düzeltme ilgili satırda belirtilmiştir.


## ag_meta

- Ağ meta-analizi: τ² için REML ya da ML seçilince çok kollu çalışmanın çalışma içi kovaryansı hesaba katılmıyor ve kestirim başvuru tedaviye göre değişiyordu → τ² çok kollu çalışmaların tam kovaryansıyla kestirilir ve başvuru tedaviden bağımsızdır; sonuç değişir mi: evet (REML/ML seçilince; örnek veride REML τ² 0,1010 → 0,0999, ML 0,0567 → 0,0564; rastgele etkiler tablosu ve P-skorları buna göre değişir); DerSimonian-Laird (varsayılan) seçilince hayır.
- Eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı yazılır; çok kollu çalışmanın yalnız bir satırı eksikse çözümleme durur (kovaryans düzeltmesi kurulamaz); sonuç değişir mi: hayır (yalnız bildirim; yarım eksik çok kollu çalışmada önceden yanlış sonuç veriyordu, şimdi hata).
- Boş başvuru tedavisi harf sırasına göre alınıyor, sayı kodlu tedavilerde "10" "2"den önce geliyordu → tedavi adları sayıya çevrilebiliyorsa sayısal sırayla, değilse harf sırasıyla dizilir; hangi tedavinin başvuru sayıldığı ve sıranın türü çıktıya yazılır; form etiketi bunu söyler; sonuç değişir mi: evet (yalnız sayısal kodlu tedavilerde tablo ve lig sırası ile boş başvuru); metin adlarda hayır.
- OR/RR/HR seçilince tablolar log ölçeğindeydi ama başlık söylemiyordu → etki ve lig tablosu başlıklarına "(log ölçeği)" eklenir; sonuç değişir mi: hayır.
- Dalsız ağda (döngü yok) Q ve tutarsızlık sayıları sayısal gürültü (1e-27) gösteriyordu → "ağda döngü yok" notu yazılır, Q değerleri 0 gösterilir; sonuç değişir mi: hayır (yalnız gösterim).
- Orman grafiğinin sütun başlığı güven düzeyini yansıtmıyordu (hep "95% CI") → başlık seçilen düzeyi yazar; sıralama, ağ çizgesi ve orman grafiği hata verirse hatanın iletisi not olarak yazılır; sonuç değişir mi: hayır.

## ag_psikometri

- Psikometrik ağ analizi (ağ karşılaştırma testi): test, formda seçilen korelasyonu (cor_auto, Spearman), ceza ızgarasını, köşegen cezasını, yeniden kestirmeyi ve eşiklemeyi dikkate almadan her grupta Pearson korelasyonlu varsayılan bir ağ kuruyordu → her grubun ağı ana ağla aynı korelasyon ve ceza ayarlarıyla kestirilir, başlıkta bu yazılır; sonuç değişir mi: evet (Pearson dışı korelasyon ya da varsayılan dışı ceza ayarında; örnek veride cor_auto ile S 0,860 → 2,447, p 0,608 → 0,161 [30 permütasyon]); Pearson ve varsayılan ayarlarda aynı.
- Eksik gözlemli satırlar sessizce atılıyordu → atılan satır sayısı, en çok eksik değeri olan maddeler yazılır; satırların %5'inden çoğu atılırsa uyarı verilir; sonuç değişir mi: hayır (yalnız bildirim).
- cor_auto seçiliyken yinelenen ya da doğrusal bağımlı madde hata vermeden ağa kenar olarak giriyordu → bağımlı/yinelenen madde her korelasyon seçeneğinde "pozitif tanımlı değil" iletisiyle durdurulur; polikorik matris yalnız sayısal nedenle pozitif tanımsızsa en yakın pozitif tanımlı matris kullanılır ve not yazılır; sonuç değişir mi: evet (yinelenen madde içeren veride; önceden anlamsız bir ağ çıkıyordu).
- Vaka düşürme sayısı (B) küçükken uyarı yoktu ve CS'nin 0,75'i aşamayacağı yazılmıyordu → B < 100 ise uyarı, CS tablo başlığında "en çok 0,75"; sonuç değişir mi: hayır.
- Bağlantısız ağda yakınlık tüm düğümlerde 0 yazılıyordu → bileşen sayısı yazılan uyarı verilir ve yakınlık boş bırakılır; sonuç değişir mi: evet (yalnız bağlantısız ağda; yakınlık sütunu boş).
- Sayı kodlu grup düzeyleri alfabetik sıralanıyordu (10, 2) → sayısal sıra (2, 10); başka paket uyarısı İngilizce yazılıyordu → yoğun ağ uyarısı Türkçe arayüzde Türkçe; yeniden kestirme açıkken λ ve EBIC'in cezalı yoldan geldiği notu; kararlılık ve NCT hatalarında paketin iletisi yazılır; sonuç değişir mi: hayır (yalnız başlık sırası ve metin).

## akrabalik

- Akrabalık (soy kütüğü): büyük yuvarlak sayısal kimlikler (100000, 200000 …) birey sütununda "100000", ebeveyn sütunlarında "1e+05" diye okunuyor, ebeveynler bulunamayıp kurucu sayılıyor ve kendileşme sıfır çıkıyordu → kimlikler sütun türünden bağımsız aynı metne çevrilir; eklenen kurucu sayısı satır sayısının yarısına varırsa kimlik yazımı için uyarı yazılır; sonuç değişir mi: evet (yuvarlak büyük sayılı kimliklerde; ör. Mrode soy kütüğünde 100000 çarpanıyla F = 0,125 doğru çıkar, eskiden 0).
- Akrabalık (genotip kipi): genotipleri ortak ayrıştırıcı okur: alel adı "0" olan `0/1` genotipleri sessizce atılmak yerine çalıştırmayı durdurur, `-9` eksik kodudur, eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (yalnız `0/1` ya da `-9` içeren veride). Üstbilgi açıklaması (TR/EN/ES) eksik kuralını yazar.
- Akrabalık (genotip kipi): Veri sekmesinde "kategorik" ya da metin türünde gelen 0/1/2 SNP sütunu "ayırıcı içermeyen genotip" iletisiyle reddediliyordu (genomik kip aynı sütunu okuyordu) → değerlerinin hepsi 0, 1, 2 olan sütun sayısal kodlama olarak okunur; sonuç değişir mi: hayır (eskiden hata); sayısal sütunla aynı sonucu verir.
- Akrabalık: 5000'den çok çift varken tablo hangi çiftlerin gösterildiğini söylemeden birey sırasıyla kesiliyordu → not "ilk 5000 çift, dosyadaki ilk 100 birey arasındaki çiftlerdir" der ve en yüksek Queller–Goodnight kestirimli 200 çift ayrı tabloda verilir; sonuç değişir mi: hayır (var olan tablo aynı; yalnız not ve ek tablo).
- Akrabalık: soy kütüğü ve genomik kipin konsol satırları ondalık nokta kullanıyordu → arayüz diline göre ondalık virgül ("F = 0,04167"); referans frekans alanında ondalık virgül anlaşılmaz bir iletiyle reddediliyordu → ileti "ondalık ayırıcı nokta olmalıdır" der; sonuç değişir mi: hayır (yalnız biçim ve ileti).
- Eksik genotip kodları (yeni alan): Akrabalık: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## alansal_bayes_car

- Bayesci alansal model (CAR): Kod panelindeki tohum, şablonun kendi tohum alanı (1) yüzünden etkisizdi → alan 0 yapılırsa çalıştırmanın tohumu kullanılır (varsayılan 1 aynı); sonuç değişir mi: hayır (varsayılan).
- Bayesci alansal model (CAR): eksik değer nedeniyle atılan alanlar sessizdi, "komşu bulunamadı" notu yanıltıcıydı → atılan alan sayısı ve kimlikleri, atılan alanlara giden bağlantılar ayrı bildirilir, %10 üstü uyarı; sonuç değişir mi: hayır (yalnız bildirim).
- Bayesci alansal model (CAR): yakınsama yalnız Geweke ile uyarılıyordu → etkin örneklem < 400 olan ve |Geweke| > 2,5 olan parametreler adlarıyla uyarılır, tek zincir (Rhat yok) belirtilir; sonuç değişir mi: hayır.
- Bayesci alansal model (CAR): sayısal sütun adı kategorik adla başlarsa katsayı etiketi bozuluyordu (type_ds → type:_ds) → doğru etiket; paket uyarısı "İngilizce" ön ekiyle, bileşen notu yalnız içsel CAR'da; sonuç değişir mi: hayır (yalnız etiket/metin).

## alfa_kafes

- Alfa kafes tasarımı: üretim kipinde 'Tarla düzeni' şekli boş çıkıyordu → şekil artık blokları ve sütunları çizer; sonuç değişir mi: hayır (yalnız şekil).

## amova

- Moleküler varyans analizi (AMOVA): permütasyon p değerleri kullanılan paketlerin testinden alınıyordu; alt popülasyon sayısı azken popülasyon düzeyinin p değeri olanaklı en küçük değerin altına iniyordu (ör. 2 + 2 alt popülasyonda 0,001; doğrusu 2/6 = 0,333) → p değerlerini artık şablon kendi permütasyon düzeniyle hesaplıyor (Excoffier, Smouse ve Quattro 1992; gözlenene eşit düzenler sayılır, p = (sayım + 1)/(B + 1); olanaklı düzen sayısı permütasyon sayısını aşmıyorsa bütün düzenler sayılır) ve bunu bir not satırıyla bildiriyor; sonuç değişir mi: evet (p değerleri ve `std_gozlenen`; alt popülasyon sayısı azken popülasyon düzeyinde belirgin, öteki durumlarda Monte Carlo farkı kadar; tabloya `sayilan_duzen` sütunu eklendi).
- AMOVA, `pegas` uygulaması: üç ve daha çok düzeyli hiyerarşide ara düzeyin p değeri veriden bağımsız B/(B + 1) çıkıyor, üst düzeyin p değeri tohuma aşırı bağlı ve yanlış hesaplanıyor, güçlü yapıda p = 0 yazılıyordu → `pegas` seçiliyken de p değerleri şablonun kendi testinden geliyor; iki uygulama aynı tohumla aynı p değerlerini veriyor; sonuç değişir mi: evet (`pegas` seçiliyken bütün p değerleri; tablo artık `ade4` uygulamasındakiyle aynı biçimde).
- AMOVA, `pegas` uygulaması: alt popülasyon ve birey içi düzey birlikteyken "Phi-alt popülasyon-popülasyon" oranının payına bireyler arası bileşen de giriyordu (ör. 0,2045; doğrusu 0,0808) → oran `ade4` uygulamasındaki tanımla hesaplanıyor; sonuç değişir mi: evet (yalnız `pegas` + alt popülasyon + birey içi düzey birleşiminde bu tek oran).
- AMOVA, `pegas` uygulaması: birey içi düzeyli tabloda satır adları kayıyordu ("Individual", "Popülasyon içi (hata)") → "… içinde bireyler arası" ve "Birey içi (hata)"; sonuç değişir mi: hayır (yalnız etiketler).
- AMOVA, dominant (0/1) veri: "Eşiği aşan lokusları çıkar" seçeneği tek bir eksik hücrede bütün lokusları çıkarıyor ve çalıştırma "AMOVA hesaplanamadı" iletisiyle duruyordu → eksik oranı eşiği aşan lokuslar şablonda çıkarılıyor, çözümleme kalan lokuslarla yapılıyor ve çıkarılan lokuslar yazılıyor; sonuç değişir mi: evet (eksik değer içeren dominant veride; eskiden sonuç yoktu).
- AMOVA: özet satırı eksik veri işleminden ve klon düzeltmesinden önceki lokus, birey ve popülasyon sayılarını yazıyordu → çözümlemeye giren sayılar yazılıyor; çıkarılan lokusların adları ve birey sayısındaki azalmanın nedeni ayrı not satırlarında; Öklid düzeltmesi uygulanmışsa özet satırında ve bir not satırında belirtiliyor; sonuç değişir mi: hayır (yalnız bildirim).
- AMOVA: birey içi düzey açıkken "Ortalama ile doldur" ya da "Sıfır ile doldur" seçilirse çalıştırma paketin ham hatasıyla duruyordu → hesaptan önce açık bir iletiyle duruyor; sonuç değişir mi: hayır.
- AMOVA: 0/1 kodlu (dominant) veri belirteç türü "kodominant" iken uyarısız çözümleniyordu → bütün lokuslar yalnız 0 ve 1 içeriyorsa uyarı yazılıyor; sonuç değişir mi: hayır.
- AMOVA: MLG süzgeci klon düzeltmesi kapalıyken de sonucu değiştirir; bunu bildiren bir not eklendi; sonuç değişir mi: hayır.
- AMOVA: birey içi düzey ve MLG süzgeci birlikte açıkken paket birey içi düzeyi kapatıyor, şablon ise "birey içi düzey eklendi" yazıyor ve satır adları kayıyordu → formdaki kural uygulanıyor: birey içi düzey varken süzgeç kapatılır; sonuç değişir mi: evet (yalnız kodominant veri + birey içi düzey + MLG süzgeci birleşiminde; tablo artık birey içi düzeyi içerir).
- AMOVA: permütasyon sayısı 1 girildiğinde paketin İngilizce hatası çıkıyordu → çalışıyor; hata iletisindeki yanıltıcı "tek bireyli popülasyon" açıklaması kaldırıldı; permütasyon sayısı alanının etiketindeki süre uyarısı kaldırıldı (test artık hızlı); sonuç değişir mi: hayır.
- AMOVA: genotipleri ortak ayrıştırıcı okuyor: `-9` eksik kodudur; alellerinden yalnız biri eksik kodu olan genotip (`0/152`) sessizce atılmak yerine çalıştırmayı durdurur; eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (yalnız `-9` kodlu ya da yarı eksik genotip içeren veride).
- Eksik genotip kodları (yeni alan): AMOVA: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken). `0` gerçek alel olduğunda poppr o aleli eksik saydığından alel adlarına `a` öneki eklenir (yalnız o durumda).

## aralik_sansur_regresyon

- Aralık sansürlü regresyon: Kod panelindeki Tohum, bootstrap standart hatasını etkilemiyordu → "Bootstrap tohumu" alanına 0 yazılırsa çalıştırmanın tohumu kullanılır (alan varsayılanı 1 aynı kalır); sonuç değişir mi: hayır (varsayılan ayarda), 0 seçilirse bootstrap SH çalıştırma tohumuna bağlanır.
- Aralık sansürlü regresyon: Log-lojistik tabanda `log_alpha` ve `log_beta` satırlarında HR/OR sütununda oran gibi bir sayı yazılıyordu → bu satırlarda sütun boş; sonuç değişir mi: hayır (yalnız bu iki hücre boş gelir, katsayılar aynı).
- Aralık sansürlü regresyon: Bilgisiz ya da ortak değişkeni eksik satırlar sessizce çıkarılıyordu → "Not: N satır çıkarıldı (…)" iletisi yazılır; sonuç değişir mi: hayır.

## ardl_nardl

- ARDL/NARDL: değişken adı `ect` (ya da `trend`) olan bir sütun, paketin hata düzeltme modelinde kurduğu `ect` sütunuyla karışıyor, "ECT (hata düzeltme terimi)" satırı ve "Düzeltme hızı" cümlesi anlamsız çıkıyordu (örnek: -0,00104541; doğrusu -0,145111) → bu adlara iç adda önek eklenir, çıktıda özgün ad yazılır; sonuç değişir mi: evet (yalnız değişken adı `ect` ya da `trend` olan veride; öteki adlarda hayır).
- ARDL/NARDL: UECM kovaryansı HAC seçilince F ve t istatistikleri klasik değerin kat kat üstüne çıkıyor, Newey–West seçimleri (ön beyazlatma, otomatik bant) ve klasik değer söylenmiyordu → sonuç kartına HAC seçimleri ve karşılaştırma için klasik (EKK) F ve t değerleri not olarak yazılır; sonuç değişir mi: hayır (yalnız not eklenir).
- ARDL/NARDL: tanı testi açıklaması "UECM artıkları üzerinde" diyordu, hesap ARDL düzey modelindedir (RESET farklı çıkar) → açıklama düzeltildi (TR/EN/ES); NARDL çarpan şeklinin göstergesindeki üst simgeli m⁺, m⁻ karakterleri bazı yazı tiplerinde kutu çıkıyordu → "m+", "m-", "m+ - m-"; tohum alanının açıklaması Kod panelindeki tohumdan bağımsız olduğunu söyler; sonuç değişir mi: hayır.

## arfima

- Uzun bellekli ARFIMA: paket Hessian standart hatasını hesaplayamayınca (ör. ARFIMA(1, d, 1), rastgele yürüyüş) çalıştırma "arguments imply differing number of rows" iletisiyle çöküyordu → tablo artık kuruluyor, Hessian SH, z ve p boş kalıyor ve uyarı yazılıyor; sonuç değişir mi: evet (bu durumlarda hata yerine tablo ve öngörü üretilir; öteki durumlarda hayır).
- Uzun bellekli ARFIMA: d kestirimi arama aralığının sınırına yapışınca kullanıcıya bir şey söylenmiyordu → sınırın 0,01 yakınında "d üst/alt sınırına yapıştı" uyarısı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Uzun bellekli ARFIMA: "Örneklem ortalamasını çıkar" kapatılınca sonuç aslında değişmiyor, yalnız başlıkta μ = 0 yazılıyordu → başlıkta her zaman kullanılan örneklem ortalaması yazılıyor, kapalıyken paketin ortalamayı kendisi çıkardığını söyleyen bir not ve alan açıklaması eklendi; sonuç değişir mi: hayır.
- Uzun bellekli ARFIMA: seçilen zaman sütunu ters sıralı ya da tekrarlı olsa da çözümleniyor, öngörü ters serinin devamı oluyordu → sayısal ya da tarih türündeki zaman sütunu artan değilse anlaşılır iletiyle duruyor; sonuç değişir mi: evet (yalnız azalan/tekrarlı zaman sütunu seçilmişse, çalıştırma artık duruyor).
- Uzun bellekli ARFIMA: negatif d alt sınırında sonlu olmayan kestirimde ileti nedeni söylemiyordu → ileti alt sınırı 0 yapmayı öneriyor; sonuç değişir mi: hayır.

## aykiri_lokus

- Aykırı lokus ve seçilim taraması: eksik genotipler pcadapt adımına 9 koduyla veriliyor, pcadapt bunları gerçek genotip sayıyordu (alel frekansı ve bütün p değerleri kayıyor; eksik oranı %1–5 olunca çalıştırma "Can't compute SVD" ile duruyordu) → eksik genotipler pcadapt'a eksik olarak verilir; hiçbir SNP'de genotipi olmayan birey varsa çalıştırma bunu söyleyerek durur; sonuç değişir mi: evet, eksik genotip içeren her veride (pcadapt p değerleri, GIF, otomatik K; önceki sonuç yanlıştı). Eksiksiz veride değişmez.
- Aykırı lokus ve seçilim taraması, "Bileşen başına" test: p değerleri bileşenler arasında karışıyordu (pcadapt 4.4.1 z²'yi genomik şişme katsayılarına bölerken katsayı vektörünü satırlar boyunca yineliyor) → bölme şablonda yapılır: her bileşen kendi katsayısına bölünür; katsayılar çıktıya not olarak yazılır; sonuç değişir mi: evet, "Bileşen başına" seçilen her çalıştırmada (örnek veride aykırı lokus 178 → 134; p ortancası 0,4229 / 0,5724 → 0,5 / 0,5). Mahalanobis (varsayılan) değişmez.
- Aykırı lokus ve seçilim taraması, FST adımı: %99 deneysel zarfın dışındaki lokuslar (tanım gereği lokusların yaklaşık %1'i) "Aykırı lokuslar" tablosuna alınıyor, hiçbir test anlamlı değilken de tablo dolu çıkıyordu → zarf betimsel sütun olarak kalır (`zarf99_disi`), aykırı lokus tablosuna girmez; zarf dışındaki lokus sayısı ayrı bir notla yazılır; sonuç değişir mi: hayır (p değerleri ve test sonuçları aynı); "Aykırı lokuslar" tablosundan yalnız zarf dışında olduğu için giren satırlar çıkar.
- Aykırı lokus ve seçilim taraması, sNMF testi: genomik kontrol kapatıldığında istatistik F dağılımının ölçeğinde değildi ((K − 1)'e bölünmüyordu; K ≥ 3'te p değerleri olması gerekenden küçüktü) → genomik kontrol kapalıyken F = z²/(K − 1) kullanılır; sonuç değişir mi: evet, yalnız "sNMF genomik kontrol" kapalı ve K ≥ 3 iken (ek veride aykırı lokus 205 → 172). Varsayılan (açık) değişmez.
- Aykırı lokus ve seçilim taraması: "Yalnız FST" yönteminde K kullanılmadığı hâlde K seçimi için pcadapt çalışıyordu → K seçimi yalnız K kullanan yöntemlerde çalışır; tarama grafiğinin dikey ekseni toplam varyans içindeki payı gösterir (önce ilk bileşenler içindeki paydı); sonuç değişir mi: hayır ("Yalnız FST"de "K seçimi" satırı ve tarama grafiği artık yok; grafikte eksen değerleri değişir, seçilen K aynı).
- Aykırı lokus ve seçilim taraması: metin ('A/G') genotiplerde ortak genotip okuma kuralı (ortak kayıt 5); çevre değişkenleri değerleriyle okunur; açıklama metni güncellendi (düzeltme seçenekleri, eksik kuralı, doğrulama değerleri); sonuç değişir mi: evet, `-9/-9` kodlu ya da "0" adlı alel içeren metin SNP sütunlarında (ortak kayıt 5).
- Eksik genotip kodları (yeni alan): Aykırı lokus taraması: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## bag_dengesizligi

- Bağlantı dengesizliği: haplotip frekansı kestirimi (EM) hep D = 0'dan başlıyordu; olabilirliğin birden çok tepesi olduğunda D = 0'da kalıyor ya da daha alçak tepeye gidiyordu (küçük örneklem, çift heterozigotların baskın olduğu çiftler) → çözüm, olabilirlik denkleminin (kübik) kökleri ve üç başlangıçlı EM arasından olabilirliği en büyük olandır; çok tepeli çiftler ve olabilirliği iki çözümde eşit olan (D'nin işareti belirlenemeyen) çiftler çıktıda bildirilir; sonuç değişir mi: evet, yalnız olabilirliği çok tepeli çiftlerde (örnek: 17 bireyli veride r² 0 → 0,653, p 1 → 0,000002). Tek tepeli çiftlerde değişmez (sınırdaki çiftlerde D' tam 1 olur: örnek veride 0,9999993 → 1).
- Bağlantı dengesizliği: EM en çok yineleme sayısına ulaşıp yakınsamadığında uyarı yazılmıyordu → not yazılır ve o çiftte sonuç olabilirlik denkleminin kökünden alınır; sonuç değişir mi: evet, yalnız "EM en çok yineleme" küçük seçildiğinde (ör. 10: D' 0,99976 → 1).
- Bağlantı dengesizliği, "Lokus konumları": ondalık virgül ayırıcı sayılıyor, sayı adedi rastlantıyla tutarsa yanlış konumlar sessizce kabul ediliyordu ('1,5; 3' → 1, 5, 3) → noktalı virgül varsa ayırıcı odur ve virgül ondalık işaretidir ('0; 1,5; 3'); etiket ve hata iletisi bunu söyler; sonuç değişir mi: evet, yalnız konumlar noktalı virgülle ve ondalık virgülle yazılmışsa (uzaklıklar ve LD azalım eğrisi).
- Bağlantı dengesizliği: açıklamadaki "Doğrulama" değerleri şablonun değil başka bir paketin (daha gevşek durma ölçütlü) çıktısıydı → açıklamada şablonun kendi değerleri yazıyor; ortak genotip okuma kuralı (ortak kayıt 5): '0/1' kodlu lokus artık sessizce "tek biçimli" sayılmaz, çalıştırma durur; sonuç değişir mi: hayır (metin); genotip kuralı için ortak kayıt 5.
- Eksik genotip kodları (yeni alan): Bağlantı dengesizliği: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## bagimli_bootstrap

- Bağımlı veri için bootstrap: 1. gecikme otokorelasyonu ve AR katsayılarında blok ek yerleri yapay (bağımsız) çiftler üretip kopyaları sıfıra çekiyordu; yüzdelik güven aralığı kestirimi dışarıda bırakıyordu (örnek seri: r₁ = 0,785 iken [0,484; 0,714], SH 0,061) → gecikmeli satırlar blok blok yeniden örneklenir ("blokların blokları"); aynı seride SH 0,030 ve aralık [0,724; 0,839] kestirimi içerir; sonuç değişir mi: evet (yalnız otokorelasyon ve AR istatistiklerinde; ortalama ve medyan değişmez).
- Bağımlı veri için bootstrap: Veri sekmesinde "kategorik" türde bırakılmış seri sütunu sessizce sıra numarasına dönüşüyordu (ortalama 2,9037 yerine 1,6134) → sütunlar sayıya çevrilir, çevrilemeyen değerde anlaşılır ileti; sonuç değişir mi: evet (yalnız bu türdeki sütunlarda; ortak katman ve şablon).
- Bağımlı veri için bootstrap: açıklama küme kipinde yüzdelik aralık ve dağılım grafiği olduğunu söylüyordu → kipe göre düzeltildi; sonuç değişir mi: hayır.

## bagimli_bootstrap_py

- Bağımlı veri için bootstrap (Python): otokorelasyon ve AR katsayılarında blok ek yerleri kopyaları sıfıra çekiyor, yüzdelik aralık kestirimi içermiyordu → R sürümüyle aynı biçimde blokların blokları; sonuç değişir mi: evet (yalnız otokorelasyon ve AR).
- Bağımlı veri için bootstrap (Python): rastgele sayı tohumu sabitti (20261029), çalıştırmanın tohumu etkisizdi → kod panelindeki tohum kullanılır; sonuç değişir mi: evet (Monte Carlo SH ve aralıklar tohuma göre değişir; tohum verilmediğinde de artık her çalıştırmada yenidir).

## barkod_tur_sinirlandirma

- DNA barkodlama, eşik ölçütü: en yakın komşusu tam eşik uzaklığında olan birey "tanımsız" yerine "yanlış" sayılıyordu (p uzaklığı ve yuvarlak eşiklerde) → komşu kümesi boşsa "tanımsız" sayılıyor ve etkilenen birey sayısı not olarak yazılıyor; sonuç değişir mi: evet (yalnız en yakın komşu uzaklığı eşiğe eşitse; K2P'de pratikte görülmez).
- DNA barkodlama, GMYC: verilen ağacın uç adları verideki örneklerle uyuşmasa da çalıştırma ilgisiz ağaçla GMYC tablosu yazıyordu → açık iletiyle duruyor; sonuç değişir mi: evet (yalnız uyuşmayan ağaçla; eskiden anlamsız sonuç).
- DNA barkodlama: "GMYC ölçekleme parametreleri üst sınırı" etiketi GMYC uyumunu sınırladığı izlenimini veriyordu; özet satırı ve şekil eksenleri modelin iç adını (K80, raw) yazıyordu → etiket "sıfır modelin ölçekleme üssü üst sınırı"; özet ve eksenlerde K2P / p; sonuç değişir mi: hayır.

## bayes_ag

- Bayes ağları: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı, kalan ve toplam satır, atılan oran ve değişken başına eksik sayısı yazılır; satırların %10'u ya da fazlası atılmışsa uyarı verilir; sonuç değişir mi: hayır (yalnız çıktı notu).
- Bayes ağları: birebir aynı (ya da tam doğrusal bağımlı) iki sayısal sütun uyarısız anlamsız bir ağ üretiyordu → çalıştırma "birbirinin doğrusal işlevi" iletisiyle durur (TR/EN/ES); sonuç değişir mi: evet (bu tür veride eskiden anlamsız ağ çıkıyordu, şimdi hata).
- Bayes ağları: Veri sekmesinde sayısal kalan 0/1 sütun sessizce Gauss düğümü oluyordu → çıktıya not düşülür ve yeni "İki değerli sayısal sütunlar" alanı eklendi (varsayılan `Gauss düğümü` = eski davranış; `Kategorik` seçilirse iki düzeyli kesikli düğüm, ağ koşullu Gauss/kesikli); sonuç değişir mi: hayır (varsayılan aynı; yalnız seçilirse).
- Bayes ağları: metin etiketli düzeyler `1, 10, 2` diye harf sırasıyla sıralanıyordu → etken sütunda Veri sekmesinin sırası, metin sütunda tüm etiketler sayıysa sayısal sıra (1, 2, 10), değilse harf sırası; sonuç değişir mi: hayır (yalnız koşullu dağılım tablosunun satır sırası).
- Bayes ağları: "Düğüm parametreleri" tablosu başlığı "artık SS" diyordu, sütun artık sd'dir → "artık sd"; sonuç değişir mi: hayır.
- Bayes ağları: bootstrap ortalama ağının mı yoksa tek ağın mı raporlandığı belli değildi; yön olasılığı 0,5 civarındaki yaylar işaretlenmiyordu → çıktı hangi ağın raporlandığını yazar ve yön olasılığı 0,45 ile 0,55 arasındaki yayları not eder; sonuç değişir mi: hayır.
- Bayes ağları: kanıtta aynı değişken iki kez yazılırsa ilk değer sessizce atılıyordu → çalıştırma "birden çok kez verilmiş" iletisiyle durur; ondalık virgül iletisi "ondalık ayırıcı nokta olmalıdır" der; bnlearn iletileri "(bnlearn iletisi, İngilizce)" ön ekiyle verilir; sonuç değişir mi: evet (tekrarlı kanıtta eskiden son değer kullanılıyordu, şimdi hata).
- Bayes ağları: `en_cok_yineleme` küçükken arama yarım kalıyor ve bildirilmiyordu → `hc` (yeniden başlatmasız) ve `tabu` için sınırda durma denetlenir ve uyarı yazılır; `en_cok_ebeveyn` sınırına ulaşan düğümler not edilir; sonuç değişir mi: hayır (yalnız uyarı notu).

## bayes_filogeni

- Bayesçi filogeni özeti: «Koşu» alanı (şimdi «Çalıştırma») boş bırakılınca iki çalıştırma tek zincir sayılıp nesle göre iç içe sıralanıyor, PSRF ve ASDSF hesaplanmıyor ve uyarı çıkmıyordu → nesil değerleri yineleniyorsa şablon açık iletiyle durur ve çalıştırmaları ayıran sütunun seçilmesini ister; sonuç değişir mi: evet (bu girdide eskiden sessizce yanlış çıkan sonuç yerine ileti).
- Bayesçi filogeni özeti: tablo girdisinde yalnız ağaç sütunu varken "Çalıştırmalarda ortak sayısal parametre yok" hatasıyla duruyordu → parametre özetleri atlanır, ağaçlar özetlenir (dosya girdisindeki gibi); sonuç değişir mi: evet (bu girdide hata yerine ağaç özeti).
- Bayesçi filogeni özeti: "Köklü ağaç = Köklü (kladlar)" köksüz ağaçlara uygulanınca ortalama dal uzunlukları sessizce kayboluyordu → durum not olarak yazılır (MCC ağacı seçilen örneğin uzunluklarıyla, uzlaşı ağacı uzunluksuz); sonuç değişir mi: hayır.
- Bayesçi filogeni özeti: kategorik türdeki nesil sütunu çalıştırmayı çevrilmemiş R hatasıyla kesiyor, kategorik türdeki parametre sütunu sessizce atılıyordu → ortak katmanda ("sayısal sütun" rolü) kapandı, şablonda `mq_sayisal` ile ayrıca güvenceye alındı; sonuç değişir mi: evet (bu iki durumda).
- Bayesçi filogeni özeti: çalıştırma uzunlukları farklıysa PSRF'nin yalnız ilk n örnekle hesaplandığı söylenmiyordu; çizim dış grubu ağaçta yoksa ya da tek soy değilse sessizce orta noktadan köklendiriliyordu; yarıda kalan MrBayes çalıştırması uyarısız özetleniyordu → üçü için not ya da uyarı; sonuç değişir mi: hayır.
- Bayesçi filogeni özeti (MrBayes girdisi): dizilerde DNA dışı karakter ya da boş dizi denetlenmiyor, MrBayes reddedince hata nedeni iletide görünmüyordu → şablon önce denetler ve hangi dizide olduğunu söyler; MrBayes hata verirse "Error" geçen satırlar iletiye girer; sonuç değişir mi: hayır (ileti).
- Bayesçi filogeni özeti: arayüz metinlerinde "koşu" sözcüğü "çalıştırma" oldu ("Koşu" alanı → "Çalıştırma", "Koşular" tablosu → "Çalıştırmalar", "Önceki koşuya devam et" → "Önceki çalıştırmaya devam et"); sonuç değişir mi: hayır (tablo ve alan adları).

## bayes_model_ortalamasi

- 17 ile 20 aday değişkende BAS modellerin yalnız 2^16 tanesini örnekliyordu ama çıktı "tam sayım (65536 model)" yazıyordu → çıktı gerçek yöntemi yazar ("BAS örnekleme (65536 / 131072 model)") ve olasılıkların yaklaşık olduğunu söyleyen bir not ekler; üstbilgi açıklaması ve "Otomatik" açıklaması düzeltildi; sonuç değişir mi: hayır (yalnız çıktı metni; sayılar aynı).
- MCMC yönteminde (tek zincir) tanı yoktu → çıktı "tek zincir, R-hat ve etkin örneklem hesaplanmaz" notunu ve MCMC sıklığı ile yeniden normalleştirilmiş dahil olma olasılıkları arasındaki en büyük farkı yazar; fark 0,05'ten büyükse yakınsama uyarısı verir; sonuç değişir mi: hayır (yalnız not ve uyarı).
- HPD aralıkları 10 000 rastgele çekimle hesaplanıyor ve tohum çıktıda yazmıyordu → 100 000 çekim kullanılır, çekim sayısı ve tohum çıktıya yazılır; sonuç değişir mi: evet, çok az (yalnız HPD sınırlarında Monte Carlo düzeyinde; sonsal ortalama, SS ve dahil olma olasılıkları aynı).
- Adaylar arasında tam doğrusal bağımlılık (ör. bir değişkenin doğrusal dönüşümü) uyarısız çalışıyor, olasılığı kopyalar arasında bölüyordu → çalıştırma durur ve bağımlı adayları adıyla söyler; sonuç değişir mi: evet (bu durumda önceden anlamsız sonuç veriyordu, şimdi hata).
- Eksik değer yüzünden atılan satır sayısı ve aday olmayan log sütunlarının yok sayıldığı yazılmıyordu → çıktıda not olarak yazılır; sonuç değişir mi: hayır.
- "Zellner-Siow Cauchy (ZS-null)" seçeneği adında tümlevin Laplace yaklaşımı olduğu söylenmiyordu → seçenek adı "(ZS-null, Laplace yaklaşımı)" olur; sonuç değişir mi: hayır.

## bayes_yem

- Bayesci yapısal eşitlik modeli: kod panelindeki Tohum alanı sonucu etkilemiyordu (yalnız formdaki "MCMC tohumu" etkiliydi) → formdaki tohum 0 ya da boşsa çalıştırma tohumu kullanılır, kullanılan tohum konsola yazılır; alanın varsayılanı (1) aynı kaldı; sonuç değişir mi: hayır (alan 1 iken aynı; yalnız alan 0 yapılınca panel tohumu belirler).
- Eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve n önce/sonra yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Veri sekmesinde "kategorik" ya da metin okunmuş sayısal gösterge yalnız "sayısal değil" hatası veriyordu → gösterge etiketinin sayı değeriyle okunur; sayıya çevrilemeyen değerde sütun ve neden söylenir; sonuç değişir mi: evet (önceden hata veren veri artık çalışır).
- BSEM çapraz yük öncülü ile göstergeler arası artık kovaryans birlikteyken yalnız genel "zinciri uzatın" uyarısı vardı → model tanımsızlığı uyarısı eklenir; sonuç değişir mi: hayır (yalnız uyarı).
- Laplace marjinal olabilirlik boş çıkınca neden söylenmiyordu → "hesaplanamadı" uyarısı; sonuç değişir mi: hayır.
- Güven düzeyi tabloda görünmüyordu → güven düzeyi 0,95 dışındaysa aralık sütunları `GA_alt_90`, `GA_ust_90` gibi adlanır; sonuç değişir mi: hayır (yalnız sütun adı, %95'te aynı).
- Yapısal katsayı grafiğinde sol eksen etiketleri kesiliyordu → kenar boşluğu genişletildi; sonuç değişir mi: hayır (yalnız görünüm).

## beta_regresyon

- Beta regresyon: karekök (ya da özdeşlik) hassasiyet bağlantısı ve hassasiyet değişkeni birlikteyken en iyileme (BFGS, Nelder-Mead) yanlış ya da ıraksak bir noktada duruyordu (örnek veride log-olabilirlik −45,10 ve 53,66; doğrusu 78,2085) ve Nelder-Mead durumunda uyarı da çıkmıyordu → şablon seçilen çözücünün durduğu noktadan Newton-Raphson ile en büyük noktaya iner, yalnız skoru sıfır ve Hessian'ı negatif tanımlı olan noktayı kabul eder, gerekirse başlangıcı log bağlantılı çözümden kurar, çözücü yeterli olmadıysa bunu notla bildirir, hiçbiri yakınsamazsa durur; sonuç değişir mi: evet (yalnız karekök/özdeşlik hassasiyet bağlantısı + hassasiyet değişkeni birlikteyken; öteki modellerde çözücüler arası 1e-6 – 1e-5'lik farklar kalkar, dördüncü basamakta değişen yok).
- Beta regresyon: "Yüzde (0-100)" ölçeği 0–1 aralığındaki oran verisinde seçilirse değerler uyarısız 100'e bölünüyordu → şablon uyarı satırı yazıyor ("ölçeği 'Oran (0-1)' yapın"); sonuç değişir mi: hayır (yalnız uyarı eklendi).
- Beta regresyon: sınır notu yalnız sınırdaki gözlem sayısını söylüyordu, oysa dönüşüm bütün yanıtlara uygulanır → not bunu açıkça yazıyor; sonuç değişir mi: hayır.

## bilesimsel_veri

- Bileşimsel veri: yordayıcıdaki eksik değer, bileşenlerin betimleme tablolarını (merkez, değişim matrisi, toplam varyans, temel bileşenler) da değiştiriyordu ve atılan satır sayısı yazılmıyordu → betimleme yalnız bileşenleri tam satırlarla, regresyon yordayıcıları da tam satırlarla hesaplanır; iki satır sayısı da yazılır; sonuç değişir mi: evet (yordayıcıda eksik değer varsa betimleme tabloları artık yordayıcıdan bağımsızdır; eksik değer yoksa aynı).
- Bileşimsel veri: yordayıcı bileşenlerden biriyse sessizce listeden çıkarılıyor ve regresyon hiç çalışmıyordu → çalıştırma "Yordayıcı bileşenlerden biri olamaz" iletisiyle durur (TR/EN/ES); sonuç değişir mi: evet (bu girdide eskiden yalnız betimleme tabloları çıkıyordu, şimdi hata).
- Bileşimsel veri: clr biplotunda puanlar sütun ortalaması çıkarılmadan hesaplanıyor, oklar ve değişken adları çizim alanının dışında kalıyordu → puanlar ortalaması çıkarılmış clr değerlerinden hesaplanır, eksen sınırları puanları, okları ve adları kapsar; sonuç değişir mi: hayır (yalnız şekil; tablolar aynı).
- Bileşimsel veri: çok değişkenli testin ardışık (tür I) olduğu formda yazmıyordu → form açıklaması ve (birden çok yordayıcıda) çıktı notu "ardışık (tür I); yordayıcı sırasına bağlıdır" der; kategorik yordayıcının düzey sırası ve başvuru düzeyi çıktıya yazılır; sonuç değişir mi: hayır.
- Bileşimsel veri: metin türündeki yordayıcıda düzey sırası, sayı etiketlerinde 1, 10, 2 biçiminde harf sırasıydı → metin sütunda tüm etiketler sayıysa sayısal sıra, değilse harf sırası; etken (kategorik) sütunda Veri sekmesinin sırası; Python ikiziyle aynı kural; sonuç değişir mi: evet (yalnız sayı etiketli metin sütununda başvuru düzeyi değişir; test ve tahmini bileşimler aynı kalır).

## bilesimsel_veri_py

- Bileşimsel veri (Python): kategorik düzeyler her zaman harf sırasıyla sıralanıyor, R ikizi Veri sekmesinin sırasını kullanıyordu; ilr katsayı tablosu (başvuru düzeyi), tahmini bileşim satır sırası ve çok kategorikte tahmini bileşimler iki ikizde farklıydı, oysa üstbilgi "R şablonuyla aynı değerler" diyordu → iki ikiz aynı kuralı kullanır (kategorik sütunda Veri sekmesinin sırası; metin sütunda tüm etiketler sayıysa sayısal sıra, değilse harf sırası), ilk düzey başvurudur ve çıktıya yazılır; sonuç değişir mi: evet (Veri sekmesinde kategorik yordayıcıda katsayı tablosu ve tahmini bileşim satır sırası değişir: örnekte kesişim 1,70245 yerine 0,74516 gelir; çok kategorikte tahmini bileşimler R ikiziyle aynı olur; Pillai ve F değişmez).
- Bileşimsel veri (Python): R ikiziyle aynı düzeltmeler: betimleme yalnız bileşenleri tam satırlarla, regresyon yordayıcıları da tam satırlarla (iki satır sayısı yazılır); yordayıcı bileşenlerden biriyse "Yordayıcı bileşenlerden biri olamaz" iletisiyle durur; biplotta puanlar ortalaması çıkarılmış ve eksen sınırları oklar ile adları kapsar; çok yordayıcıda "ardışık (tür I)" notu; sonuç değişir mi: yordayıcıda eksik değer varsa betimleme tabloları değişir, yordayıcı bileşense hata verir, öteki durumlarda aynı.
- Bileşimsel veri (Python): metin türünde ondalık virgüllü bileşen için ileti "ondalık ayırıcı nokta olmalıdır" der; sonuç değişir mi: hayır.

## bilissel_tani

- Bilişsel tanı modelleri (profil): öğrenci profili beceri sınıflarının sonsal (MAP) ya da olabilirlik (MLE) olasılığı eşit olduğunda paketin rastgele seçimine bırakılıyordu, profil tohuma bağlıydı (örnek veride 536 öğrencinin 276'sında beraberlik) → beraberlikte en küçük sınıf indeksi (beceri sınıfı sırasında ilk) seçilir, profil tohumdan bağımsızdır; profil tablosuna son sütun olarak eşit sınıf sayısı eklenir ve not yazılır; sonuç değişir mi: evet (yalnız beraberlikli öğrencilerde; tek modlu öğrencilerde ve öğrenci 1 profilinde 11101111 aynı).
- Kimlik bağlantısında monotonluk kısıtı ima edilen başarı olasılıklarını [0, 1] dışına çıkarıyor ama sonuç normal çıktı gibi yazılıyordu → olasılıklar [0, 1] dışındaysa çalıştırma "Lojit bağlantı seçin ya da kısıtı kapatın" iletisiyle durur; kimlik bağlantılı ACDM ve G-DINA'da olasılık [0, 1] dışına çıkarsa uyarı yazılır (monotonluksuz); sonuç değişir mi: evet (monotonluk + kimlik bağlantısında çıktı yerine ileti; örnek veride 20 maddenin 20'sinde ihlal).
- Yakınsama durumu okunmuyordu → en çok yineleme aşılınca "yakınsamadı" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Boş satırlar atılıp yanıtlayıcı numarası yeniden 1…n yapılıyordu → profil tablosunda özgün veri satırı numarası yazılır ve atılan satır sayısı bildirilir; beceri adı sayısı beceri sayısına eşit değilse uyarı; sonuç değişir mi: evet (tümüyle boş satır olan veride numaralar; diğer durumda aynı).
- Yeni seçenek "EM yakınsama eşiği: Paketin kendi varsayılanı" (G-DINA/ACDM için 1e-4) ve "Beceri sınıfı dağılımı" (paketin kuralı / doymuş); varsayılanlar bugünkü sonucu verir ("paket" seçeneğinin etiketi "Gevşek eşik (0,001)" olarak düzeltildi); çıktıda kullanılan sınıf modeli yazılır; sonuç değişir mi: hayır (varsayılanda); seçenekle evet (G-DINA 263 yineleme/−4236,607 → 982 yineleme/−4224,365).
- En sık 10 beceri sınıfı tablosunda eşit olasılıklı sınıflar sınıf sırasıyla dizilir (belirli); sonuç değişir mi: hayır.

## biyoesdegerlik_nca

- NCA ve biyoeşdeğerlik: PK değeri / konsantrasyon ya da zaman sütunu Veri sekmesinde "kategorik" türdeyse değerler yerine kategori sıra numaraları kullanılıyordu (örnek veride GMR %95,09 yerine %84,80, "biyoeşdeğer" yerine "biyoeşdeğer değil") → sütunlar her zaman değerleriyle okunur; sayı olmayan bir değer varsa çalıştırma sütunu bildiren iletiyle durur; sonuç değişir mi: evet, bu sütunları "kategorik" türde olan çalıştırmalarda (doğru sonuca döner).
- NCA ve biyoeşdeğerlik: kısmi AUC(0–τ), τ'ya kadar değil τ'dan önceki son ölçüme kadar hesaplanıyor, sütun yine "AUC0_τ" adını taşıyordu (teofilin örneğinde denek 1: 0–12,12 saatlik alan 92,37) → τ iki ölçüm arasına düşerse konsantrasyon seçilen yamuk kuralıyla τ'ya ara değerlenir (denek 1: 146,01); son ölçülebilir zamanı τ'dan önce olan profilde dış değerleme yapılmaz, bu profiller uyarıda sayılır ve alanın bittiği zaman yeni `kismi_auc_bitis` sütununda verilir; sonuç değişir mi: evet, τ bir ölçüm zamanına denk gelmeyen profillerde kısmi AUC büyür.
- NCA ve biyoeşdeğerlik: İngilizce (ve İspanyolca) arayüzde, bir ölçütte her iki tedavisi olan denek sayısı 3'ten azsa çalıştırma not yerine ham R hatasıyla ("invalid format '%d'") duruyordu → not yazılır; sonuç değişir mi: hayır.
- NCA ve biyoeşdeğerlik: gözlenen güç, sıralar eşit büyüklükteymiş gibi (toplam n ikiye bölünerek) hesaplanıyordu → her iki tedavisi olan deneklerin sıralara gerçek dağılımıyla hesaplanır; sonuç değişir mi: evet, yalnız dengesiz sıralarda "güç" sütunu (örnek: sıralar 4 ve 9, güç 0,002914 → 0,001415); GMR, güven aralığı, karar ve gerekli örneklem büyüklüğü değişmez.
- NCA ve biyoeşdeğerlik: form açıklamasında ticari yazılım adı geçiyordu → kaldırıldı; kısmi AUC alanının açıklaması yeni davranışı anlatır; sonuç değişir mi: hayır.

## bootstrap_ozel

- Kullanıcı tanımlı istatistik için bootstrap: düzey alanları boşken gruplar metin sırasıyla seçiliyordu (form "ilk düzey" diyor; sayı kodlu 2/5/10 → "10 − 2") → MerQuR+ düzey sırası (kullanıcı sırası, sayıysa sayısal, değilse ilk görülme); sonuç değişir mi: evet (yalnız düzey alanları boş bırakıldığında; açıkça yazılan düzeylerde hayır).
- Kullanıcı tanımlı istatistik için bootstrap: tam sayı kodlu grupta eksik hücre varken düzeyler "2.0" biçimindeydi → "2"; sonuç değişir mi: hayır.
- Kullanıcı tanımlı istatistik için bootstrap: kırpma oranı × gözlem sayısı 1'den küçükken "%1 kırpılmış ortalama" yazılıp hiç gözlem atılmıyordu → çıktıya not; eski SciPy sürümlerinde `rng` anahtar sözcüğü hatası giderildi; sonuç değişir mi: hayır.

## bos_python

- Boş Python şablonu: değişiklik yok (görünen metinde ticari ad, 'koşu' ya da tek satırlık düşük bulgu bulunmadı; SAS sekmesi gerekçesi 'sas_karsilik_yok' korundu).

## bos_r

- Boş R şablonu: değişiklik yok (görünen metinde ticari ad, 'koşu' ya da tek satırlık düşük bulgu bulunmadı; SAS sekmesi gerekçesi 'sas_karsilik_yok' korundu; B0 sayısal rol çağrısı bu grupta yapılmadı).

## brms_cok_duzeyli

- Kod panelindeki Tohum alanı etkisizdi; yalnız şablonun kendi "Rastgele sayı tohumu" alanı belirleyiciydi ve kullanılan tohum çıktıda yoktu → alan 0 verilirse çalıştırma tohumu (kod paneli) kullanılır; alanın varsayılanı 1 aynı kalır (bugünkü sonuç); kullanılan tohum çıktının ilk satırına yazılır; sonuç değişir mi: hayır (varsayılan ayarda); alan 0 yapılınca çalıştırma tohumuna göre.
- En büyük R-hat dağılım parametrelerini (sigma, nu) kapsamıyor, etkin örneklem için uyarı yoktu → R-hat bu parametreleri de içerir; tanı tablosuna sona "En küçük ESS (kütle)" ve "En küçük ESS (kuyruk)" satırları eklenir; 400'ün altında uyarı verilir; sonuç değişir mi: evet, yalnız en büyük R-hat sigma/nu'da daha büyükse o değeri gösterir.
- Tek zincirde R-hat bölünmüş R-hat olduğu söylenmiyordu → tek zincir notu yazılır; sonuç değişir mi: hayır.
- Sabit etkilere normal(0, s) öncülü yordayıcıların ham ölçeğinde uygulanıyor ve çıktıda kullanılan öncüller yazılmıyordu → çıktıya "Kullanılan öncüller" satırı (brms varsayılanları dahil) ve ham ölçek uyarı notu eklenir; sonuç değişir mi: hayır (yalnız not).
- Kategorik sabit etkinin başvuru düzeyi yazılmıyordu → düzey sırası ve başvuru düzeyi bir satırla yazılır; sonuç değişir mi: hayır.
- Sıralı ailede dağılım tablosuna sabit `disc = 1` satırı giriyordu → satır çıkarıldı; sonuç değişir mi: hayır (yalnız tabloda bir satır az).

## buyume_karisim

- Büyüme karışım modeli: eksik gözlemler sessizce atılıyordu → atılan gözlem sayısı ve tümüyle kaybolan birey sayısı yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Üç ve daha çok sınıflı model tek başlangıçla kurulup yerel en büyükte kalabiliyor, ama "yakınsadı" yazıyordu → karşılaştırma tablosunda her modelin başlangıç sayısı (`baslangic` sütunu, sona eklendi) ve konsolda başlangıç düzeni yazılır; tek başlangıçlı 3+ sınıfta "yerel en büyük olabilir, ızgara aramasını deneyin" uyarısı çıkar; sonuç değişir mi: hayır (varsayılan başlangıç düzeni aynı; çok başlangıçlı en iyi çözüm için "Izgara araması" alanı >0 yapılır, o durumda sonuç değişir).
- Zaman sütununun ya da ortak değişkenin adında "zaman" geçince parametre adları bozuluyordu (`zaman_yil_yil^2`) → tam sözcük eşleşmesiyle doğru adlar; sonuç değişir mi: hayır (yalnız ad).
- Üç ve daha çok sınıflı modelde standart hataların başlangıç yoluna duyarlı olduğu söylenmiyordu → "SH'ler yaklaşık sayılmalı" uyarısı; sonuç değişir mi: hayır.
- Yineleme sınırı düşükken tek sınıflı model yakınsamayınca ileti yalnız ölçek öneriyordu → ileti yineleme sınırını da anar ve geçerli sınırı yazar; sonuç değişir mi: hayır.

## capraz_gecis

- Çapraz geçiş: şablon açıklamasında ticari yazılım adları geçiyordu → açıklama nötr yazıldı (TR/EN/ES); sonuç değişir mi: hayır.

## cesitlilik_indeksleri

- Çeşitlilik indeksleri: örtü sınıfı gibi sıra ölçekli kodlar (ör. 0–9) birey sayısı sanılıp seyreltme ve Chao1 üretiliyor, kullanıcı uyarılmıyordu → bütün değerler 0–9 aralığında tamsayıysa "sıra ölçeği kodlaması olabilir; seyreltme ve Chao1 yalnız birey sayımında anlamlıdır" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı; örnek veri dune bu uyarıyı verir).
- Çeşitlilik indeksleri: Chao1 açıkken bolluk tamsayı değilse sütun sessizce eklenmiyordu → "Chao1 de hesaplanmadı" notu yazılır; sonuç değişir mi: hayır.
- Çeşitlilik indeksleri: eksik değerli satırlar atılırken yalnız sayısı yazılıyordu → atılan örneklerin adları da yazılır (en çok 20); sonuç değişir mi: hayır.
- Çeşitlilik indeksleri: seyreltme n'si (varsayılan en küçük satır toplamı) hangi örnekten geldiği söylenmeden kullanılıyordu → n ve onu veren örnek yazılır; sonuç değişir mi: hayır.

## cezali_cox

- Cezalı Cox (kat ataması tohumu): şablonun "Rastgele tohum" alanı kod panelindeki tohumu yok sayıyordu, künyedeki "Tohum" sonucu belirlemiyordu → alan 0 (ya da boş) verilirse çalıştırmanın tohumu kullanılır (alan varsayılanı 1 aynı kalır), kullanılan tohum çıktıya yazılır; sonuç değişir mi: hayır (alan 1 iken; λ_min = 0,0192043, λ_1se = 0,0933828 aynı). Alan 0 yapılırsa sonuç panel tohumuna bağlanır.
- Cezalı Cox (yakınsama): `glmnet` bazı λ değerlerinde yineleme sınırına dayanıp yolu kestiğinde uyarı yalnız tablolardan sonra tek satırda görünüyordu → uyarı çıktının başında "UYARI" olarak yazılır (kesilen nokta sayısı; seçilen λ kesilen yolun ucundaysa en iyi λ'nın ötesinde olabileceği), sonda yinelenmez; sonuç değişir mi: hayır (yalnız bildirim).
- Cezalı Cox (λ dizisi): "λ dizisinin uzunluğu" etiketi gerçek yolu yansıtmıyordu (örnekte 100 yerine 49 nokta) → etiket "en çok uzunluk" olur, hesaplanan λ sayısı ve aralığı çıktıya yazılır, `lambda_oran` açıklaması "istenen alt sınır" der; sonuç değişir mi: hayır.
- Cezalı Cox (bağlı süreler): olay zamanlarında bağ varken çapraz doğrulama sapması glmnet'in doymuş-model terimli tanımıyla hesaplanıyor ama bu söylenmiyordu → çıktıda not yazılır; sonuç değişir mi: hayır.
- Cezalı Cox (üstbilgi): doğrulama notu örnek ayarla yeniden üretilemeyen değerleri veriyordu → hangi ayarın (Harrell C ölçütü, λ = 0,05, tohum 1) vinyet değerlerini, hangisinin örnek ayar değerlerini (0,01920 / 0,09338) verdiğini yazar; sonuç değişir mi: hayır (metin).

## cift_engel

- Cragg çift engel modeli: bağımsız log-normal modelde (ikinci engel kapalı) σ'nın standart hatası paketin hatası yüzünden %31 küçük yazılıyordu (örnek veride 0,0304; doğrusu 0,0439; z 35,69 yerine 24,74) → şablon σ'nın SH'sini σ/√(2·n₊) kapalı formülüyle kendisi hesaplıyor, z ve p'yi ona göre güncelliyor ve çıktıya paket değeri ile kullanılan değeri yazan bir not düşüyor; sonuç değişir mi: evet (yalnız bu modelde σ'nın SH, z ve p'si; bağımlı modelde karşılaştırma olarak yazılan bağımsız model tablosunda da).
- Cragg çift engel modeli: yineleme sınırında kalan (yakınsamamış) kestirim uyarısız yazılıyordu → son aşamanın gradyanından yakınsama ölçütü hesaplanıyor; eşik aşılırsa "en büyükleme tamamlanmadı" uyarısı, LR negatifse ayrıca uyarı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Cragg çift engel modeli: yalnız ikinci engelli tek denklemli modelde "bağımlı" seçilince ham R hatası çıkıyordu → bağımlılık seçeneği bu durumda yok sayılıp not yazılıyor; mhurdle bu modelde Hessian vermediğinden standart hatalar hesaplanamazsa anlaşılır bir iletiyle duruyor; sonuç değişir mi: hayır.
- Cragg çift engel modeli: ölçekleme kapalıyken de "ölçek: geometrik ortalama" satırı yazılıyordu, ikinci engelli log-normalde `pos` satırı açıklanmıyordu → ölçek satırı yalnız ölçekleme açıkken yazılıyor, `pos` için not eklendi; sonuç değişir mi: hayır.

## cift_makine_ogrenmesi

- Çift/yansız makine öğrenmesi: formdaki "Rastgele sayı tohumu" alanı, kod panelindeki çalıştırma tohumunu sessizce eziyordu → alan 0 verilirse çalıştırma tohumu kullanılır (varsayılan 1 değişmedi), kullanılan tohum ve kaynağı çıktıda yazılır; sonuç değişir mi: hayır (varsayılanda aynı; yalnız alan 0 yapılırsa çalıştırma tohumuna göre).
- Çift/yansız makine öğrenmesi (IRM): eğilim skoru dağılımı, kırpılan gözlem sayısı ve etkin örneklem büyüklüğü hiç yazılmıyordu → "Örtüşme tanısı" tablosu eklendi; kırpılan gözlem varsa not, oran %5'i aşarsa uyarı; sonuç değişir mi: hayır (yalnız ek çıktı).
- Çift/yansız makine öğrenmesi (etiketler): ATTE'de "g" satırı yalnız kontrol grubunu belirtmiyordu; çapraz uydurma kapalıyken "dış kat" RMSE başlığı yanlıştı; bölme tekrarı > 1 iken RMSE'nin ilk bölmeden olduğu yazılmıyordu → satır adı ve tablo başlıkları düzeltildi; sonuç değişir mi: hayır (yalnız metin).

## cift_saglam_aipw

- Çift sağlam kestirim (AIPW): iki düzeyli metin tedavisinde hangi düzeyin tedavi sayıldığı sütunun Veri sekmesindeki türüne ve düzey sırasına bağlıydı (R'de "kategorik" türde ilk görülen düzey, Python'da alfabetik), ATE iki şablonda zıt işaretli çıkabiliyordu ve çıktı hangi grubun tedavi sayıldığını yazmıyordu → iki şablonda aynı kural: düzeyler sayıya benziyorsa büyük olan, değilse alfabetik son düzey tedavidir; yeni "Tedavi sayılan düzey" alanı tedaviyi açıkça seçtirir; çıktı tedavi ve kontrol düzeyini ve kuralı yazar; sonuç değişir mi: evet (R'de "kategorik" türde, ilk görülen düzeyi alfabetik sonuncu olmayan metin ya da sıralı kodlu tedavilerde; 0/1 sayısal tedavide hayır).
- Çift sağlam kestirim (AIPW): ikinci standart hata sütunu (SH_sandvic) ortak değişkenlerin ölçü birimine bağlıydı (kazançlar sent biriminde yazılınca Python %20 yanlış değer yazıyor, R "tekil türev matrisi" hatasıyla duruyor; 0/1 sonuçta varsayılan ayrı modelde de R duruyordu) → tasarım matrisi sütunları en büyük mutlak değerlerine bölünerek çalışılır (model aynı); sonuç değişir mi: evet, yalnız SH_sandvic'te ve çok küçük (örnek veride 1180,4451 → 1180,4488: eskiden sayısal türev hatasıydı); ölçü birimi değişince artık sonuç tutarlıdır, durma ortadan kalktı. Ana SH, ATE ve güven aralığı değişmez.
- Çift sağlam kestirim (AIPW, eğilim skoru kırpma): kırpma sınırı > 0 iken eğilim modelinin tahmin denklemi kırpılmış skorla yazılıyor, SH_sandvic anlamını yitiriyordu ve kırpılan gözlem sayısı yazılmıyordu → eğilim bloğu kırpılmamış skorla yazılır, kırpılan sayı çıktıda yer alır; sonuç değişir mi: evet, yalnız SH_sandvic'te ve kırpma açıkken (örnek veride ε = 0,05, ayrı modeller: 1088,8 → 1089,24); ATE ve ana SH değişmez.
- Çift sağlam kestirim (AIPW, örtüşme): uyarı yalnız eğilim skoru 1e-6'dan küçükken çıkıyordu, ağırlık özeti, etkin örneklem büyüklüğü ve ortak değişken dengesi yoktu → "Örtüşme tanısı" tablosu (en küçük/en büyük eğilim skoru, en büyük ağırlık, etkin n, kırpılan), ağırlıklı standartlaştırılmış ortalama fark (SMD) tablosu ve 0,01/0,99 eşikli uyarı eklendi; sonuç değişir mi: hayır (yalnız ek çıktı ve uyarı).
- Çift sağlam kestirim (AIPW, tam ayrışma): bir ortak değişken tedaviyi tam ayırınca NaN'lı tablo "tamam" diye dönüyor (Python yakınsama uyarılarını da gizliyordu) → eğilim modeli uyarı verirse, yakınsamazsa ya da eğilim skorları 0/1'e çakılırsa anlaşılır iletiyle durur, uyarı metni iletiye eklenir; sonuç değişir mi: evet (bu durumda sonuç tablosu yerine ileti gelir; başka durumlarda hayır).

## cift_saglam_aipw_py

- Çift sağlam kestirim (AIPW): iki düzeyli metin tedavisinde hangi düzeyin tedavi sayıldığı sütunun Veri sekmesindeki türüne ve düzey sırasına bağlıydı (R'de "kategorik" türde ilk görülen düzey, Python'da alfabetik), ATE iki şablonda zıt işaretli çıkabiliyordu ve çıktı hangi grubun tedavi sayıldığını yazmıyordu → iki şablonda aynı kural: düzeyler sayıya benziyorsa büyük olan, değilse alfabetik son düzey tedavidir; yeni "Tedavi sayılan düzey" alanı tedaviyi açıkça seçtirir; çıktı tedavi ve kontrol düzeyini ve kuralı yazar; sonuç değişir mi: evet (R'de "kategorik" türde, ilk görülen düzeyi alfabetik sonuncu olmayan metin ya da sıralı kodlu tedavilerde; 0/1 sayısal tedavide hayır).
- Çift sağlam kestirim (AIPW): ikinci standart hata sütunu (SH_sandvic) ortak değişkenlerin ölçü birimine bağlıydı (kazançlar sent biriminde yazılınca Python %20 yanlış değer yazıyor, R "tekil türev matrisi" hatasıyla duruyor; 0/1 sonuçta varsayılan ayrı modelde de R duruyordu) → tasarım matrisi sütunları en büyük mutlak değerlerine bölünerek çalışılır (model aynı); sonuç değişir mi: evet, yalnız SH_sandvic'te ve çok küçük (örnek veride 1180,4451 → 1180,4488: eskiden sayısal türev hatasıydı); ölçü birimi değişince artık sonuç tutarlıdır, durma ortadan kalktı. Ana SH, ATE ve güven aralığı değişmez.
- Çift sağlam kestirim (AIPW, eğilim skoru kırpma): kırpma sınırı > 0 iken eğilim modelinin tahmin denklemi kırpılmış skorla yazılıyor, SH_sandvic anlamını yitiriyordu ve kırpılan gözlem sayısı yazılmıyordu → eğilim bloğu kırpılmamış skorla yazılır, kırpılan sayı çıktıda yer alır; sonuç değişir mi: evet, yalnız SH_sandvic'te ve kırpma açıkken (örnek veride ε = 0,05, ayrı modeller: 1088,8 → 1089,24); ATE ve ana SH değişmez.
- Çift sağlam kestirim (AIPW, örtüşme): uyarı yalnız eğilim skoru 1e-6'dan küçükken çıkıyordu, ağırlık özeti, etkin örneklem büyüklüğü ve ortak değişken dengesi yoktu → "Örtüşme tanısı" tablosu (en küçük/en büyük eğilim skoru, en büyük ağırlık, etkin n, kırpılan), ağırlıklı standartlaştırılmış ortalama fark (SMD) tablosu ve 0,01/0,99 eşikli uyarı eklendi; sonuç değişir mi: hayır (yalnız ek çıktı ve uyarı).
- Çift sağlam kestirim (AIPW, tam ayrışma): bir ortak değişken tedaviyi tam ayırınca NaN'lı tablo "tamam" diye dönüyor (Python yakınsama uyarılarını da gizliyordu) → eğilim modeli uyarı verirse, yakınsamazsa ya da eğilim skorları 0/1'e çakılırsa anlaşılır iletiyle durur, uyarı metni iletiye eklenir; sonuç değişir mi: evet (bu durumda sonuç tablosu yerine ileti gelir; başka durumlarda hayır).

## clmm_sirali_karma

- Sıralı karma model (clmm): şablon açıklamasındaki ticari yazılım adı kaldırıldı (TR/EN/ES); sonuç değişir mi: hayır.

## cok_degiskenli_kontrol

- Çok değişkenli kontrol kartı: eksik değerli satır çıkarılınca sonraki gözlemler 1'den yeniden numaralanıyor, çıkarılan satırın yeri yazılmıyordu → gözlem numarası verideki satır numarasıdır ve notta çıkarılan satırların numarası (ilk on) yazılır; sonuç değişir mi: hayır (yalnız etiket ve not; eksik satır kovaryans kestirimini zaten değiştirir).
- Çok değişkenli kontrol kartı: bir özellik Aşama I'de sabit, sonra değişiyorsa TBA kartı ham R hatasıyla ("infinite or missing values") duruyordu → iki kartta da "Sabit sütun(lar) (Aşama I verisinde): …" iletisiyle durur; sonuç değişir mi: hayır (eskiden hata).
- Çok değişkenli kontrol kartı: "Aşama I için ilk m" veri uzunluğunu aşınca hiçbir şey söylenmeden tüm veri Aşama I oluyordu → not düşülür ("Aşama I sayısı (100) veri uzunluğunu (25) aşıyor: hepsi Aşama I alındı, Aşama II boş"); sonuç değişir mi: hayır (yalnız not).
- Çok değişkenli kontrol kartı: ondalık virgüllü metin sütunu "sayısal değil" diye reddediliyor, nedeni söylenmiyordu; kısmen sayısal olmayan sütunlarda satırlar sessizce eksik sayılıyordu → ileti "… sayıya çevrilemedi: N değer sayı değil (ör. …). Ondalık ayırıcı nokta olmalıdır." der (sütunlar ortak `mq_sayisal` ile okunur); sonuç değişir mi: evet, yalnız karışık metin içeren sütunda (eskiden bu satırlar sessizce çıkarılırdı, şimdi çalıştırma durur).
- Çok değişkenli kontrol kartı (yeni alan): sayısal alt grup kimliği her zaman sayı sırasıyla diziliyor, satırlar zaman sırasındaysa ama kimlik zaman sırasını izlemiyorsa (parti ya da kasa numarası) Aşama I yanlış gruplardan kuruluyordu → forma "Alt grupların sırası" seçeneği eklendi: Otomatik (varsayılan, bugünkü davranış) ya da Veri sırası (Aşama I verideki ilk m alt grup); sıra veri sırasından farklıysa not düşülür; sonuç değişir mi: hayır (varsayılanda sonuçlar aynı; yalnız not eklenir).

## cok_degiskenli_meta

- Çok değişkenli meta-analiz: çalışma içi kovaryans sütunları satır sırasına (konuma) göre eşleşiyordu; bir çalışmada sonuç eksikse yanlış sütun varyans sayılıyordu → j. sütun sonuç sırasındaki j. sonuca ad üzerinden eşleşir (sonuç sırası: en çok sonuçlu ilk çalışmanın satır sırası), eşleşme konsola yazılır ("v1i = PD; v2i = AL"); çalışma içindeki satır sırası artık önemsizdir; sonuç değişir mi: evet (bir çalışmada sonuç eksikse ya da satırlar farklı sırada girilmişse; her çalışmada tüm sonuçlar aynı sırada ise hayır, örnek Berkey verisi aynı).
- Kullanılmayan kovaryans sütunundaki boş hücre satırı (ve çalışmayı) sessizce düşürüyordu → yalnız çözümlemede kullanılan sütunlardaki boşluk satırı düşürür; atılan satır sayısı ve "kendi varyans sütunu boş" notu yazılır; iki sonuç arasındaki kovaryans hücresi iki yarıda da boşsa çözümleme durur; sonuç değişir mi: evet (tek sonuçlu çalışmada kullanılmayan kovaryans sütunu boşsa çalışma artık korunur).
- QM tüm katsayıları (sonuç ortalamaları dahil) sınıyordu ve serbestlik derecesi yazılmıyordu → tabloya `QM_sd` eklenir; moderatör varsa yalnız eğimler için ayrı Wald testi (`QM_egimler`, `QM_egimler_sd`, `QM_egimler_p`) eklenir (mevcut satırlar değişmez); sonuç değişir mi: hayır (yalnız ek satırlar).
- Kovaryans sütunları ile varyans sütunu birlikte seçilince varyans sütunu sessizce yok sayılıyordu → bildirilir; orman grafiği güven düzeyine uymuyordu (hep %95, 1,96) → aralıklar `guven` düzeyine göredir, eksen etiketi düzeyi ve "yalnız örnekleme hatası"nı yazar; gerçek etkiler arası korelasyon ±1'e yakın kestirilince sınır çözümü uyarısı yazılır; sonuç değişir mi: hayır (tablolar aynı; yalnız şekil ve notlar).

## cok_durumlu_msm

- Aralıklı gözlenen çok durumlu Markov modeli: Fisher puanlama panel veride en iyi noktaya ulaşmadan duruyordu (örnekte -2 log L 4119,974; doğrusu 3986,087) → artık aynı model BFGS ile de kestirilir, daha iyi olabilirlik bulunursa BFGS sonucu gösterilir ve uyarı yazılır; sonuç değişir mi: evet, yalnız `Fisher puanlama` seçiliyken ve Fisher en iyi noktaya ulaşmadığında.
- Aralıklı gözlenen çok durumlu Markov modeli: "Ortak değişkenleri ortala" kapalıyken yoğunluk tablosu başlıkta "sıfırda" dese de ortalamada hesaplanıyordu → artık yoğunluklar, ortalama kalma süreleri ve geçiş olasılıkları gerçekten ortak değişkenler sıfırdayken hesaplanır; sonuç değişir mi: evet, yalnız ortak değişken seçili ve ortalama kapalıyken (örnek: q12 0,12746 yerine 0,13612); tehlike oranları ve -2 log L değişmez.
- Aralıklı gözlenen çok durumlu Markov modeli: Hessian tersinmez olduğunda (çok düzeyli etken) ham R hatası ("$ operatörü …") çıkıyordu → artık tablolar standart hata ve güven aralığı olmadan yazılır ve uyarı verilir; sonuç değişir mi: evet, eskiden hata veren çalıştırmalar artık sonuç verir; hata vermeyenler aynı.
- Aralıklı gözlenen çok durumlu Markov modeli: doğal ölçekte büyük değerli sayısal ortak değişken (ör. yaş) "numerical overflow" hatasıyla kurulamıyordu → artık ortak değişken 10'un kuvvetine bölünerek yeniden denenir, tehlike oranları özgün birime (yıl başına) çevrilir ve bu not edilir; sonuç değişir mi: eskiden hata veren çalıştırmalar artık sonuç verir; hata vermeyenler aynı.
- Aralıklı gözlenen çok durumlu Markov modeli: form etiketi ve açıklama "tüm geçişlere orantılı etki" diyordu, kod her geçişe ayrı katsayı kestirir → metin "her geçişe ayrı tehlike oranı" olarak düzeltildi; sonuç değişir mi: hayır.
- Aralıklı gözlenen çok durumlu Markov modeli: eksik değerli satırlar haber verilmeden çıkarılıyordu → artık satır sayısı, tümüyle çıkan ve ziyaret zinciri kesilen birey sayısı bildirilir; sonuç değişir mi: hayır (yalnız bilgi notu).
- Aralıklı gözlenen çok durumlu Markov modeli: sıfıra giden yoğunluklar, sınıra giden tehlike oranları ve `optim` yakınsama kodu bildirilmiyordu → ilgili durumlarda uyarı satırı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Ek: durum, zaman ve birey sütunları ortak katmandaki `mq_sayisal` / `mq_kimlik` ile okunur (Veri sekmesinde "kategorik" olsa da değerler doğru); sonuç değişir mi: hayır.

## cok_durumlu_mstate

- Çok durumlu model (hastalık-ölüm): ara duruma geçiş zamanı son durum zamanından sonra olan satırlar hata vermeden yeniden yorumlanıyor, geçiş sayıları sessizce kayıyordu → artık "N satırda ara duruma geçiş zamanı son durum zamanından sonra" iletisiyle durur; sonuç değişir mi: evet, yalnız bozuk (olanaksız sırada) veride; düzgün veride aynı.
- Çok durumlu model: ara ve son olay aynı zamandaysa 2→3 olayı sıfır uzunluklu aralık olarak düşüyor, olasılıklar kayıyordu → ara olay önce sayılır (son zaman en küçük zaman aralığının binde biri kadar ilerletilir), etkilenen hasta sayısı ve ilerletme miktarı yazılır; sonuç değişir mi: evet, yalnız ara ve son olayın aynı zamanda olduğu veride (örnek bozulmuş veride Cox olay sayısı 1995 yerine 2010); eşit zamanı olmayan veride aynı.
- Çok durumlu model: olasılık zamanlarında ondalık virgül ("0,5") sessizce iki zaman sayılıyordu → noktalı virgül varsa yalnız o ayırıcı olur ("0,5; 2" iki zaman: 0,5 ve 2); virgülle yazımda hangi sayıların okunduğunu söyleyen bir not eklenir; sonuç değişir mi: hayır (virgüllü liste aynı okunur, yalnız not eklenir).
- Çok durumlu model: eksik değerli satırlar (hastalar) haber verilmeden çıkarılıyordu → artık çıkarılan satır sayısı ve kalan hasta sayısı bildirilir; sonuç değişir mi: hayır.
- Çok durumlu model: başlangıç zamanından küçük olasılık zamanları anlamsız satır (1, 0, 0) üretiyordu → atılır ve not düşülür; en uzun izlemin ötesindeki zamanlar için uyarı yazılır (satır son olay zamanındaki değeri yineler); sonuç değişir mi: evet, yalnız başlangıç zamanı s > 0 iken s'den küçük zaman yazılmışsa (satır artık yok); izlem ötesi zamanlarda değerler aynı, yalnız uyarı eklenir (örnek ayardaki t = 10 dahil).
- Çok durumlu model: özet satırındaki "n (risk satırı)" hasta sayısı sanılabiliyordu → özet satırı hasta sayısını da yazar; sonuç değişir mi: hayır.
- Ek: zaman ve durum sütunları ortak katmandaki `mq_sayisal` ile okunur; sonuç değişir mi: hayır.

## cok_duzeyli_atama

- Çok düzeyli çoklu atama: Gibbs örnekleyicinin yakınsaması hiç okunmuyordu → en büyük Rhat çıktıya yazılır, 1,05'i aşarsa uyarı verilir; sonuç değişir mi: hayır (yalnız yeni çıktı satırı).
- Çok küçük p değerleri 0 yazılıyordu (kesişim için "0"); orta büyüklükte p'de kesme hatası vardı → p `2 * pt(-|t|, sd)` ile hesaplanır, p < 0,0001 olan terimler ayrıca not edilir; sonuç değişir mi: yalnız p sütununda çok küçük farklarla (eğim 5,113909·10⁻¹² → 5,113944·10⁻¹²; kesişim 0 → 5,1·10⁻³¹⁴).
- Küme kimliği ya da bütün değişkenleri eksik olduğu için atılan satırların sayısı yazılmıyordu → "Atılan satır: N (…)" yazılır; sonuç değişir mi: hayır.
- Tohum çıktıda yazılmıyor, Kod paneli tohumu atamayı etkilemiyordu → "Tohum: N" yazılır; tohum alanı 0 ya da boşsa çalıştırma tohumu kullanılır (alan varsayılanı 1 değişmedi); sonuç değişir mi: hayır (varsayılanda).

## cok_duzeyli_cfa

- Çok düzeyli CFA: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve n önce/sonra yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Sağlam kestirici (MLR) seçilince CFI, TLI, RMSEA ve SRMR standart değerlerle yazılıyordu → uyum tablosunun sonuna ölçekli (`.scaled`) ve sağlam (`.robust`) CFI, TLI, RMSEA satırları eklenir, konsolda üstteki satırların ölçeklenmemiş olduğu yazılır; sonuç değişir mi: hayır (mevcut satırlar aynı; yalnız ek satırlar).
- Kümeler arası varyans çok küçükken (ICC 0,003 ile 0,013) şablon çalışıp anlamsız sonuç (negatif varyans, TLI > 1) yazıyordu → bütün göstergelerde ICC 0,02'nin altındaysa ileti ile durur, 0,05'in altındaysa uyarı verir; sonuç değişir mi: evet (yalnız ICC'si 0,02'nin altındaki veride; önceden anlamsız sonuç çıkıyordu, şimdi durur).
- "Kümelerin çoğunda tek gözlem" denetimi yalnız hepsi tek gözlemliyse çalışıyordu → kümelerin yarıdan fazlası tek gözlemliyse durur; sonuç değişir mi: evet (tek gözlemli kümeler yarıdan fazlaysa artık durur).
- Düzey içi ortak değişkenin kümeler arası değişimi denetlenmiyordu → ICC 0,1'in üstündeyse bağlamsal etki uyarısı; sonuç değişir mi: hayır (yalnız uyarı).
- Negatif varyans (Heywood) yalnız ham lavaan uyarısı olarak düşüyordu → negatif varyanslı parametreler adlandırılıp "çözüm geçersizdir" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Gösterge ve ortak değişkenler ortak yardımcıyla (`mq_sayisal`) okunur: metin okunmuş/kategorik sayılar etiket değeriyle gelir, çevrilemeyen değerde anlaşılır ileti; sonuç değişir mi: hayır (sayısal veride aynı).

## cok_duzeyli_meta

- Çok düzeyli meta-analiz: etki kimlikleri kümeler arasında yinelenince (örnek ayardaki `school` her ilçede 1'den başlar) üç düzeyli modele karşı olabilirlik oranı testinin ikinci modeli farklı kümelerin etkilerini tek rastgele etkiye bağlıyor, test yaklaşık 20 kat şişiyordu → etki kimliği küme içinde iç içe sayılır, ikinci model gerçek iki düzeyli modeldir; yinelenme çıktıya not olarak yazılır; sonuç değişir mi: evet (yalnız LRT ve p satırı; örnek veride LRT 364,5 → 17,77, p 3·10⁻⁸¹ → 2,5·10⁻⁵; σ², birleştirilmiş etki ve CR2 aynı).
- Varsayılan göreli tolerans (10⁻¹²) moderatörlü modelde ve bazı verilerde "yakınsamadı" hatasıyla çözümlemeyi durduruyordu → istenen tolerans yakınsamazsa 10⁻⁸'e gevşetilip yeniden kestirilir, o da olmazsa `optim` denenir ve çıktıya not yazılır; 10⁻⁸'den gevşek bir tolerans seçilince "yakınsama gevşek" notu yazılır; varsayılan alan değeri değişmedi; sonuç değişir mi: evet (önceden hata veren çalıştırmalar artık sonuç verir; hata vermeyenlerde hayır).
- Eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı yazılır; orman grafiğinin başlığı güven düzeyini yansıtmıyordu → başlık `guven` düzeyini yazar; sonuç değişir mi: hayır.

## cok_yanitli_karma

- Çok yanıtlı karma model: veride `value`, `trait` ya da `.birey` adlı bir sütun sabit etki ya da gruplama olarak seçilirse şablon o sütunun üzerine yazıyor, uyarısız anlamsız sonuç veriyordu (katsayı 1, standart hatalar boş, log-olabilirlik boş) → şablonun iç sütun adları kullanıcı adlarıyla çakışmaz; `trait` ve `units` gibi ayrılmış adlı sütunlar da kullanılabilir ve tablolarda özgün adlarıyla görünür; log-olabilirlik sonlu değilse çalıştırma açık iletiyle durur; sonuç değişir mi: evet, yalnız bu adları taşıyan sütunların rol aldığı çalıştırmalarda (doğru sonuca döner).
- Çok yanıtlı karma model (REML/ML): eniyileyici düz olabilirlikte tepeye varmadan durabiliyor, kovaryans kestirimleri 5.–6. basamakta (bazen 4.) tepe noktasından ayrışıyor ve eniyileyici seçeneğine göre değişiyordu → kestirim, gradyan sıfırlanana kadar Newton adımlarıyla inceltilir; iki eniyileyici seçeneği aynı sonucu verir; "Model uyumu" tablosuna "en büyük |gradyan|" satırı eklendi; sonuç değişir mi: evet, kovaryanslar ve sabit etkiler 5.–6. anlamlı basamakta (örnek: ana düzeyi sırt rengi varyansı 0,069463 → 0,069471; REML log-olabilirliği −2181,215025 → −2181,215027).
- Çok yanıtlı karma model: ML seçiliyken özet satırı "yöntem reml" yazıyordu → satır REML, ML ya da MCMC yazar; sonuç değişir mi: hayır.
- Çok yanıtlı karma model (MCMC): korelasyon sütunu sonsal ortalama kovaryansların oranıydı ve aralığı yoktu; (ko)varyans tablosu aynı öğeyi iki kez ve paketin iç adlarıyla listeliyordu → korelasyon zincirin her örneğinde hesaplanır, sonsal ortalaması ve HPD aralığı ayrı bir tabloda verilir; (ko)varyans tablosu kaynak / yanıt çifti sütunlarıyla, her öğe bir kez; çıktıya sonsal ortalamanın REML kestirimiyle aynı tanım olmadığını söyleyen not eklendi; sonuç değişir mi: evet, MCMC yolunda korelasyon sütunu (örnek veride dördüncü ondalıkta: −0,2570 → −0,2572).
- Çok yanıtlı karma model (MCMC): önsel yanıtların birimine bağlı; ölçeği 1'den uzak yanıtlarda (ör. yanıtlar 10'a bölününce ana düzeyi varyansı 0,23 yerine 1,66) uyarısız yanlı sonuç çıkıyordu → yanıtlardan birinin standart sapması 0,3'ten küçük ya da 3'ten büyükse hangi yanıt olduğunu ve SS'sini yazan, standartlaştırmayı ya da REML yolunu öneren uyarı çıkar; varsayılan önsel değişmedi; sonuç değişir mi: hayır (uyarı eklendi).
- Çok yanıtlı karma model: form açıklamasında ticari yazılım adı geçiyordu → kaldırıldı; sonuç değişir mi: hayır.

## cok_yuzeyli_rasch

- Çok yüzeyli Rasch modeli: puanlar 0'dan başlamıyor ya da bir ara değer görülmüyorsa (1–4 kodlama, 0-1-3) hata verilmiyor, anlamsız adım parametreleri normal çıktı gibi yazılıyordu → çalıştırma "puanlar 0'dan başlayıp kesintisiz artmalı" iletisiyle ve gözlenen puanlarla durur, yeniden kodlama önerilir; sonuç değişir mi: evet (bu girdilerde çıktı yerine ileti).
- Kategori sayısı farklı maddelerde TAM'ın doldurma değerleri (99, 99, −198; SH 0) parametre tablosuna kestirim gibi giriyordu → bu satırlar tablodan çıkarılır ve not yazılır; sonuç değişir mi: evet (yalnız kategori sayısı farklı maddelerde tablo satırları).
- En çok EM yinelemesi aşılsa da sonuç yakınsamış gibi yazılıyordu → UYARI; atılan ham satır sayısı (yüzey/kişi kimliği eksik, tüm maddeler boş) bildirilir; sonuç değişir mi: hayır (yalnız bildirim).
- "Yalnız madde" modelinde sıfıra sabitlenmiş yüzey satırları ve anlamsız "katılık sırası" tablosu gösteriliyordu → bu satırlar ve tablo gösterilmez, not yazılır; sonuç değişir mi: hayır (yalnız fazla satırlar).
- Sayı kodlu yüzeylerde düzeyler metin olarak sıralanıyordu ("10", "2", "3") → sayısal sıra (2, 3, 10); çıktı düzey sırasını ve toplamı sıfır tamamlayan türetilen son düzeyi yazar; standart hataların TAM'ın yaklaşık hesabı olduğu ve BIC'te N'nin kişi sayısı olduğu notu eklenir; sonuç değişir mi: hayır (kestirimler aynı; yalnız satır sırası, hangi düzeyin türetildiği ve SH'si sayısal koda göre değişir).

## coklu_test_yeniden_ornekleme

- Yeniden örneklemeli çoklu test: bootstrap ile minP seçilince ham p'si 0 olan her değişkenin düzeltilmiş minP değeri 0 yazılıyordu (örnek veride 223 gen) → gözlenen örnek minP için yeniden örnekleme kümesine eklenir (B + 1 değer), ham p'si 0 olan değişkenin minP'si artık 0 olmaz; sonuç değişir mi: evet (yalnız bootstrap + minP; permütasyon ve maxT değişmez).
- Yeniden örneklemeli çoklu test: örnek ayarda minP hiçbir hipotezi reddedemiyor, ulaşılabilir en küçük değer yazılmıyordu → minP'nin tabandaki en küçük değeri çıktıda yazılır, α'dan büyükse uyarı ve parametrik taban önerisi; sonuç değişir mi: hayır.
- Yeniden örneklemeli çoklu test: düzey sırası alfabetikti ve R'nin yerel ayarına bağlıydı → MerQuR+ düzey sırası (Veri sekmesi sırası; metinde sayıysa sayısal, değilse ilk görülme); sonuç değişir mi: evet (yalnız grup düzeylerinin alfabetik sırası ilk görülme/sayısal sıradan farklıysa; istatistiğin işareti ve tek yönlü testin yönü değişir).
- Yeniden örneklemeli çoklu test: sonuç listesi boşken "kategorik" türdeki sayısal sütunlar bildirilmeden aile dışında kalıyordu → sayı olarak aileye alınır ve adlarıyla bildirilir; sonuç değişir mi: evet (yalnız böyle sütunlar varsa; aile büyüdüğü için düzeltilmiş p değerleri değişir).
- Yeniden örneklemeli çoklu test: α = 0,3 seçilince ret tablosunda aynı düzey iki kez yazılıyordu → tek satır; Tohum alanı varsayılanı 0 = çalıştırmanın tohumu (pozitif değer eskisi gibi sabit tohum); sonuç değişir mi: hayır (tohum alanını varsayılanda bırakan kullanıcıda her çalıştırma farklı Monte Carlo örneği kullanır).

## coyd

- COYD: MJRA uygulandığında hata kareler ortalamasıyla birlikte karşılaştırılan çeşit ortalamaları da haber verilmeden model kestirimine dönüyordu → MJRA yalnız AÖF'yi (kareler ortalaması ve sd) değiştirir; farklar ve kararlar yıllar üzerinden ham çeşit ortalamalarından hesaplanır (UPOV TGP/8/5 Bölüm II §3.9.2.1). Model kestirimi ortalama tablosunda ayrı bir bilgi sütunudur (`mjra_kestirimi`); sonuç değişir mi: evet, yalnız MJRA uygulanan çalıştırmalarda farklar çok az değişir (örnekte C2 − R3 5,0029 → 5; en çok 0,05 gün), karar yalnız |fark| AÖF'ye çok yakın çiftlerde değişebilir.
- COYD: aday listesi boşken 5000'den çok çift oluştuğunda karşılaştırma tablosu uyarısız kesiliyordu → uyarı yazılır (çift sayısı; aday özeti bütün çiftlerden hesaplanır); sonuç değişir mi: hayır.
- COYD: ölçüt tablosunun satır etiketlerinde ve F3 uyarısında olasılık düzeyi Türkçe arayüzde ondalık noktayla yazılıyordu ("p_d3 = 0.01") → arayüz dilinin ondalık ayırıcısıyla yazılır ("p_d3 = 0,01"); sonuç değişir mi: hayır.
- COYD: çeşit ve yıl etiketlerinde baş ya da sondaki boşluk aynı çeşidi iki ayrı çeşit gibi gösteriyordu → etiketler boşlukları atılarak okunur; özellik sütunu değerleriyle okunur; sonuç değişir mi: evet, yalnız etiketlerinde fazladan boşluk bulunan veride (doğru eşleşme).

## coyu

- COYU: aday bazı yıllarda bulunmuyorsa değeri bulunduğu yılların ortalamasından alınıyor, sınır ve karar ise bütün yılların sayısıyla, uyarısız veriliyordu → çalıştırma "Aday … şu yıllarda yok" iletisiyle durur; test yılları adayın bulunduğu yıllarla sınırlanır ya da aday ayrı çalıştırılır; sonuç değişir mi: evet, yalnız adayı bazı yıllarda eksik olan veride (karar yerine ileti).
- COYU: özellik ortalaması eşit olan karşılaştırma çeşitlerinde eğilim değerleri, V ve UC verinin satır sırasına bağlıydı → eşit ortalamalı çeşitler çeşit adına göre dizilir (adın sonundaki sayı sayısal, gerisi alfabetik); sonuç satır sırasından bağımsızdır ve kural şablon açıklamasında yazar; sonuç değişir mi: evet, yalnız eşit ortalamalı karşılaştırma çeşitleri veride ad sırasında dizili değilse (örnek veri ad sırasında olduğundan aynı: V 0,020183, UC 2,419613, C1 2,193564; satırları ters çevrilmiş veride eskiden UC 2,4266 çıkıyordu).
- COYU: karşılaştırma çeşitleri her yıl bulunmuyorsa (dengesiz tablo) sınırdaki 1/(R·k) terimi karşılaştırma ortalamasının varyansını küçük gösteriyordu → terim 1/N_r'dir (N_r: ortalamaya giren değer sayısı); sonuç değişir mi: evet, yalnız dengesiz tabloda (UC biraz büyür); dengeli tabloda aynı.
- COYU: şablon açıklamasındaki göreli varyans doğrulama değeri (1,0526) şablonun verdiği değerle (1,0516) aynı değildi → açıklama düzeltildi (TR, EN); sonuç değişir mi: hayır.
- COYU: göreli varyans tablosunun başlığı pay serbestlik derecesi seçimini yansıtmıyor ("n − 1" seçilse de "n"); ölçüt etiketlerinde olasılık düzeyi Türkçe arayüzde ondalık noktayla yazılıyordu → başlık seçimi gösterir; sayılar arayüz dilinin ondalık ayırıcısıyla yazılır; sonuç değişir mi: hayır.
- COYU: çeşit, yıl ve tekrar etiketleri baş/son boşlukları atılarak, ölçüm sütunları değerleriyle okunur; sonuç değişir mi: evet, yalnız etiketlerinde fazladan boşluk bulunan veride.

## cusum_ewma

- CUSUM ve EWMA kartları: eksik değerli ölçüm hiçbir not düşülmeden atılıyor ve sonraki dönemler bir geri numaralanıyordu → not düşülür ("N satır eksik değer nedeniyle çıkarıldı (özgün satır no: …)") ve bireysel ölçümlerde dönem etiketi özgün satır numarasıdır; kart çıkarılan dönemi atlayarak sürer (hesap değişmez); sonuç değişir mi: hayır (yalnız etiket ve not).
- CUSUM ve EWMA kartları: CUSUM sinyal tablosundaki "Sinyal sayısı" bağımsız sinyal sayısı sanılıyordu (sinyalden sonra sayaç sıfırlanmaz) → satırın adı "Sinyal sayısı (sınırın üstünde geçen dönem sayısı)" oldu; sonuç değişir mi: hayır (yalnız etiket).
- CUSUM ve EWMA kartları: σ yazılmışken "σ kestirim yöntemi" seçimi hiçbir şey söylenmeden yok sayılıyordu → "σ girildiği için 'σ kestirim yöntemi' seçimi kullanılmadı" notu yazılır; sonuç değişir mi: hayır (yalnız not).
- CUSUM ve EWMA kartları: "hızlı başlangıç < karar aralığı" denetimi yalnız EWMA seçiliyken de çalışmayı durduruyordu → denetim yalnız CUSUM kurulurken yapılır; sonuç değişir mi: hayır (yalnız EWMA seçilip hızlı başlangıç ≥ h olan nadir girdide eskiden hata).
- CUSUM ve EWMA kartları: ölçüm sütunu ortak `mq_sayisal` ile okunur (Veri sekmesinde kategorik görünen sayısal sütun ve sayı olmayan değerler için anlaşılır ileti); sonuç değişir mi: hayır.

## cwm_gradyan

- CWM ve çevresel gradyan: alan (grup) düzeyleri her zaman harf sırasına konuyordu; Veri sekmesindeki sıra yok sayılıyor, "10" adı "2"den önce geliyordu → düzey sırası Veri sekmesindeki sıradır (kategori türündeki sütun); ham metinde hepsi sayıysa sayısal, değilse harf sırasıdır; sonuç metni sırayı ve başvuru (ilk) düzeyi yazar; sonuç değişir mi: evet, yalnız Veri sekmesi sırası harf sırasından farklıysa ya da düzey adları sayıysa (başvuru düzeyi, katsayı işaretleri ve Tukey çiftlerinin sırası değişir); örnek veride (Fontegreca, S_Agnese, Salisano) hayır.
- CWM ve çevresel gradyan: özellik kapsamı eşiğin altındaki örnek alanlar da modele giriyor, yalnız uyarı yazılıyordu → yeni seçenek "Özellik kapsamı eşiğin altındaki örnek alanları o özelliğin modelinden çıkar" (varsayılan KAPALI = bugünkü davranış; kapalıyken uyarıdan sonra "eşik altı alanlar yine de modele girdi" notu yazılır); açıkken o örnek alanların ilgili özelliğe ait CWM'si eksik sayılır ve o özelliğin model, permütasyon testi ve grafiğinden çıkar; sonuç değişir mi: varsayılanda hayır, seçenek açılınca evet (örnekte SLA için n 74 → 40).
- CWM ve çevresel gradyan: artık varsayımı bozuk yanıtlar, AIC'nin dönüşümler arası karşılaştırılamazlığı, permütasyon testlerinin tek açıklayıcılı olması ve çizginin alan etkisini içermemesi yalnız tablolarda duruyordu → konsola uyarı/not satırları eklendi; "p = 0,0000" yerine "p < 0,0001" yazılır; özellik tablosu bulunamazsa hata iletisi aranan klasörleri ve "tam yol yazın" bilgisini söyler; sonuç değişir mi: hayır (yalnız metin).

## dagilim_uydurma

- Dağılım uydurma: en çok olabilirlik aramasında durma ölçütü log-olabilirliği pozitif olan veride (çok küçük birim, (0, 1) oranları) aramayı ilk adımda durduruyor, tabloya başlangıç değerleri "ML" diye yazılıyordu → arama şimdi gerçek en büyük noktaya varır; sonuç değişir mi: evet, yalnız pozitif log-olabilirlikli veride (Beta, kilogram gibi küçük birim) ve bootstrap aralıklarında.
- Dağılım uydurma: standart hatalar ve bazı adayların uydurulabilmesi verinin birimine bağlıydı (veri × 10 ya da × 100 yazılınca SH bozuluyor, gamma/üstel atlanıyordu) → şekil parametreleri, SH ve aday kümesi birimden bağımsız, ölçek parametreleri birimle orantılı; sonuç değişir mi: evet, birim gram dışındaysa ve gamma/üstel gibi oran parametreli adaylarda SH %1e-3..%15 kadar; gram biriminde SH'ler bağımsız hesapla 5 basamağa uyuşur.
- Dağılım uydurma: tek aday kaldığında KS, CvM ve AD boş geliyordu → artık dolu; sonuç değişir mi: hayır (yalnız boş sütunlar dolar).
- Dağılım uydurma: birinci şekildeki histogram ve yoğunluk paneli "çizilemedi" yazıyordu; sınıf sayısı verilince P-P grafiği şekilden düşüyordu → panel doğrudan çiziliyor, dört panel de yerinde; sonuç değişir mi: hayır (yalnız şekil).
- Dağılım uydurma: normal, lognormal ve üstel adaylarda en iyileme ayarları (reltol, maxit) uygulanmıyor, L-BFGS-B'de yakınsama ölçütü gevşek kalıyordu; kestirim en büyük noktada değilse uyarı yoktu → ayarlar bütün adaylara uygulanır, L-BFGS-B'de factr verilir, en büyük noktaya ulaşılmadıysa not yazılır; sonuç değişir mi: evet, 1e-4..1e-6 düzeyinde (bootstrap GA'da ve L-BFGS-B/BFGS/SANN seçildiğinde).

## dalgacik

- Dalgacık (wavelet) güç spektrumu: tek seri çözümlemesinde "Benzetim AR mertebesi p" ve "ARIMA MA mertebesi q" alanları hiçbir etki yapmıyordu (her zaman AR(1)/varsayılan ARIMA kullanılıyordu) ve AR benzetimi katsayı olarak kısmi özilişkileri alıyordu → mertebeler tek ve iki seri çözümlemesinin ikisinde de uygulanır, AR katsayıları Yule-Walker ile kestirilir, çıktıya boş modelin mertebesi yazılır; sonuç değişir mi: evet, yalnız AR mertebesi 1'den ya da MA mertebesi 1'den farklı seçildiğinde (ör. güneş lekesi, tohum 2026, 20 benzetim: AR(5) p değerleri artık AR(1)'den farklı); varsayılan ayarlarda hayır (AR(1) sonucu aynı).
- Dalgacık: iki seri çözümlemesinde baskın periyottaki faz farkı ve gecikme, anlamlılık denetimi olmadan yazılıyordu → faz tablosuna baskın periyottaki çapraz güç p ve uyum p eklendi, çapraz güç p > 0,05 ise "faz farkı ve gecikme yorumlanmamalı" uyarısı yazılır; sonuç değişir mi: hayır (yalnız tablo satırı ve uyarı eklenir; varsayılan pencereler aynı).
- Dalgacık: küresel tablo kenar etkisi konisini ayırt etmiyordu ve en büyük periyot serinin süresinden uzun verilince uyarı yoktu → tabloya "koni dışı zaman oranı" ve "koni dışı ortalama güç" sütunları (sona) eklendi; en büyük periyot serinin süresini aşarsa uyarı yazılır; sonuç değişir mi: hayır (var olan sütunlar aynı).
- Dalgacık: sayısal zaman sütunu boşluklu ya da ters sıralı olsa da çözümleme hatasız bitiyor, periyotlar yanlış süre birimine bağlanıyordu → zaman sütunu artan ve eşit aralıklı değilse açık iletiyle durur; sonuç değişir mi: evet, yalnız boşluklu/ters sıralı zaman sütunu seçili iken (artık hata verir); metin ve tarih etiketleri denetlenmez.
- Dalgacık: yeni seçenek "Benzetim p değerini (k+1)/(B+1) olarak ver" (varsayılan kapalı: p = k/B); p = 0 görülürse çıktıya "p < 1/(B+1)" notu yazılır; sonuç değişir mi: hayır (seçenek kapalıyken); açıkken p değerleri (k+1)/(B+1) olur.
- Dalgacık: benzetim ilerleme çubuğu çıktı metnine karışıyordu; ikinci seri birinci ile aynı sütunsa çapraz çözümleme sessizce atlanıyordu; "En büyük periyot" etiketi ⌊n/3⌋·dt yazıyordu; belge cümleleri (koni dışı güvenilirlik, saf sinüste gücün √π'ye eşitliği) yaklaşık ifadelere çevrildi → çubuk yazılmaz, aynı sütun için uyarı yazılır, etiket ve belge metni düzeltildi; sonuç değişir mi: hayır.

## dcc_garch

- DCC-GARCH: tek değişkenli model IGARCH seçilince rmgarch β = 1 − α kısıtını tanımıyor, anlamsız parametrelerle ve çok düşük log-olabilirlikle (1780,22) "yakınsadı" diye sonuç basılıyordu → anlaşılır iletiyle durur (IGARCH için Genişletilmiş GARCH şablonu önerilir); sonuç değişir mi: evet (yalnız IGARCH seçildiğinde: eski anlamsız sonuç yerine ileti).
- DCC-GARCH: serilerin ortasındaki eksik hücre satırın tamamını sessizce düşürüyor, GARCH/DCC özyinelemesi atlanan dönemi görmüyordu → ortada eksik değerde "Serilerin ortasında eksik değer var" iletisiyle durur; baştaki ve sondaki eksik satırlar kırpılır ve not yazılır; sonuç değişir mi: evet (ortada eksik olan veride: sonuç yerine ileti; baştaki/sondaki eksikte hayır).
- DCC-GARCH: normal çok değişkenli dağılımda ortak log-olabilirlik ve bilgi ölçütleri Gauss ifadesidir, birinci aşama dağılımı Student t/GED olsa da; dağılımlar arasında seçim yanıltıyordu → "Model uyumu" tablosuna sona birinci aşama log-olabilirliği satırı ("logLik_birinci_asama") eklendi, yardım metni açıklar (TR/EN/ES); sonuç değişir mi: hayır (mevcut satırlar aynı, yeni satır eklenir).
- DCC-GARCH: çözücü nlminb seçilince birinci aşama sessizce daha düşük olabilirlikte kalabiliyordu (6377,67; solnp 6383,73) → solnp dışı çözücüde birinci aşama olabilirliği her serinin bağımsız solnp çözümüyle karşılaştırılır, 0,5'ten fazla düşükse "Uyarılar" satırına uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- DCC-GARCH: zaman sütunu azalan sıradaysa model zamanın tersine kestiriliyor, uyarı yoktu → zaman sütunu çoğunlukla azalıyorsa uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı).

## delta_yontemi

- Türetilmiş parametre (delta yöntemi): "ikinci derece terim sıfıra çok yakın" eşiği mutlaktı (|c| < 10⁻¹²); x büyük birimlerle ölçülünce (ör. doz × 10⁷) çok anlamlı bir c bile şablonu durduruyordu → eşik göreli (c'nin eğri üzerindeki katkısı yanıtın erimine göre), uydurma x ölçeklenerek yapılır; çok küçük c metinde anlamlı basamakla yazılır; sonuç değişir mi: evet (yalnız eşiğin yanlış durdurduğu/yanlış sonuç verdiği büyük birimli x'te; ötesinde sayılar 10⁻¹² düzeyinde aynıdır).
- Türetilmiş parametre (delta yöntemi): çukur biçimli (c > 0) eğride başlıklar "tepe" diyordu → "Çukur noktası konumu / Çukur değeri"; sonuç değişir mi: hayır.

## deming_passing_bablok

- Yöntem karşılaştırma (Deming ve Passing-Bablok): ilişki negatifken Passing-Bablok eğimi ilişkinin tersi yönünde ve tek noktaya çökmüş aralıkla yazılıyor, ardından "yanlılık var" kararı veriliyordu → Spearman r < 0 ise Passing-Bablok satırları boş bırakılır, karar verilmez, grafikte çizilmez ve uyarı yazılır (Deming sonucu aynen kalır); sonuç değişir mi: evet (yalnız negatif ilişkili veride; Deming değişmez).
- Yöntem karşılaştırma: x değerleri az sayıda ve çok bağlıyken Passing-Bablok sınırları sonsuz ya da eksik çıkıyor, eksik kesişim sınırı "yanlılık var" kararına dönüşüyordu → hesaplanamayan sınır NA yazılır, karar "belirsiz" olur ve uyarı verilir; çiftlerin %10'undan fazlası aynı x'e sahipse ayrıca uyarı yazılır; sonuç değişir mi: evet (yalnız sonlu olmayan sınırın çıktığı veride karar "yanlılık var" yerine "belirsiz"; sayılar aynı).
- Yöntem karşılaştırma: eksik ya da sayı olmayan değer nedeniyle çıkarılan satır sayısı yazılmıyordu ve negatif eğimli denklem "y = a + -b·x" biçiminde çıkıyordu → çıkarılan satır sayısı bir notla yazılır; denklem işareti eğimden alınır ("y = a − b·x"); sonuç değişir mi: hayır (yalnız metin).

## dengeleme_agirliklari

- Dengeleme ağırlıkları: tedavi sütunu Veri sekmesinde "kategorik" işaretliyse ve etiketler metinse (ör. tedavi/kontrol) tedavi grubu ilk görülme sırasına göre seçiliyor, ilk satırı "tedavi" olan sütunda kontrol grubu tedavi sayılıp ATT'nin işareti ters çıkıyor, çıktı da hangi düzeyin tedavi sayıldığını yazmıyordu → düzeyler sütun türünden bağımsız olarak sayısal (sayısal değilse alfabetik) sırayla dizilir, ikinci düzey tedavi sayılır (Python eşiyle aynı kural), konsolun ilk satırı tedavi ve kontrol düzeyini ve gözlem sayılarını yazar; yeni "Tedavi grubu sayılan düzey" alanı başka düzeyi tedavi saydırır; sonuç değişir mi: evet (yalnız metin etiketli ve "kategorik" işaretli, ilk satırı alfabetik ikinci düzey olmayan tedavi sütununda; ör. lalonde_metin: ATT −212,50 yerine +1273,26).
- Dengeleme ağırlıkları: ağırlıklar kırpılınca etki SH'si sessizce ağırlıkları sabit sayan HC0'a düşüyor ama tablo başlığı yine "M-kestirimi SH" diyordu → başlık kullanılan SH türünü yazar ("sağlam SH, ağırlıklar sabit") ve konsola not düşülür (kırpma, enerji dengelemesi, sonuç eksikliği); sonuç değişir mi: hayır (yalnız başlık ve not).
- Dengeleme ağırlıkları: eksik değerli satırlar sessizce atılıyor ve sonuç değişkeninin eksikliği ağırlıkları değiştiriyordu → atılan satır sayısı yazılır; yeni "Ağırlıkları yalnız tedavi ve ortak değişkenlerin tam olduğu satırlarda kestir" seçeneği (varsayılan kapalı) sonuç eksiği olan satırları yalnız sonuç modelinden çıkarır (bu durumda etki SH'si HC0'dır); sonuç değişir mi: varsayılanda hayır; seçenek açıksa evet (sonuç eksiği olan veride ağırlıklar ve etki değişir).
- Dengeleme ağırlıkları: bir ortak değişken tedaviyi tam ayırınca ağırlıklar sıfır oluyor ve çalıştırma ham İngilizce bir iletiyle duruyordu → "Ağırlıklar '…' grubunda (neredeyse) tamamen sıfır: bir ortak değişken tedavi gruplarını neredeyse tam ayırıyor" iletisiyle durur; sonuç değişir mi: hayır (yalnız ileti).
- Dengeleme ağırlıkları: kararlı ağırlık ATT/ATC ile seçilince İngilizce paket hatasıyla duruyordu → şablonun kendi iletisiyle durur; denge iletisi artık "ölçüt karşılandı / karşılanmadı" der ve fark ihmal edilebilirse değişken adını yazmaz; denge grafiğinde değişken adları kırpılmaz; sık WeightIt/cobalt uyarıları ve hata iletileri Türkçeye çevrilir; çok düzeyli kategorik ortak değişken (ör. sayısal sütunun "kategorik" yapılması) zaman aşımına dek sürmek yerine anlaşılır iletiyle reddedilir ve gözlem sayısı denetimi gösterge değişkenleri sayar; sonuç değişir mi: hayır (ileti, başlık ve grafik; yalnız önceden zaman aşımıyla biten ya da kerte yetersiz çalıştırmalar artık iletiyle durur).

## dengeleme_agirliklari_py

- Dengeleme ağırlıkları (Python): lojistik eğilim skorunda tam ayrışma uyarısız anlamsız bir sonuç yazıyordu (ör. etki 104290, SH 161594155) → model yakınsamadıysa, ayrışma uyarısı verdiyse ya da eğilim skoru 1e−6 / 1−1e−6 dışına çıktıysa "Lojistik eğilim skoru kestirilemedi: bir ortak değişken tedavi gruplarını (neredeyse) tam ayırıyor" iletisiyle durur; sonuç değişir mi: yalnız ayrışmalı verilerde (anlamsız sonuç yerine ileti).
- Dengeleme ağırlıkları (Python): etki standart hatası ortak değişkenlerin ölçeğine bağlı sayısal türevle alınıyor, kazançlar sent biriminde iken SH %0,09 ile %121 arasında sapıyordu → sandviç türev matrisi çözümsel kurulur; SH ölçekten bağımsızdır ve R şablonuyla aynıdır; sonuç değişir mi: evet, yalnız SH, z, p ve güven aralığı ve yalnız ölçek büyük olduğunda belirgin (kestirim aynı; ölçek 1'de fark 1e−5 göreli ve altı; ör. kazançlar sent iken lojistik ATE SH 275188 yerine 124462).
- Dengeleme ağırlıkları (Python): R eşiyle tedavi düzeyi kuralı tutarsızdı ve çıktıda tedavi düzeyi yazılmıyordu → iki şablon aynı kuralı kullanır (sayısal, değilse alfabetik; ikinci düzey tedavi), konsol tedavi ve kontrol düzeyini yazar, yeni "Tedavi grubu sayılan düzey" alanı başka düzeyi tedavi saydırır; sonuç değişir mi: hayır (Python'un bugünkü kuralı korunur; değişen R eşidir).
- Dengeleme ağırlıkları (Python): eksik değerli satırların sayısı yazılmıyor ve sonuç eksikliği ağırlıkları değiştiriyordu → atılan satır sayısı yazılır; yeni "Ağırlıkları yalnız tedavi ve ortak değişkenlerin tam olduğu satırlarda kestir" seçeneği (varsayılan kapalı); sonuç değişir mi: varsayılanda hayır, seçenek açıksa evet (sonuç eksiği olan veride).
- Dengeleme ağırlıkları (Python): sabit sonuç değişkeni ağırlıklar kestirildikten ve çıktı yazıldıktan sonra yakalanıyordu → ağırlıklardan önce denetlenir; denge iletisi "ölçüt karşılandı / karşılanmadı" der ve fark ihmal edilebilirse değişken adını yazmaz; çok düzeyli kategorik ortak değişken bellek hatası yerine anlaşılır iletiyle reddedilir; sonuç değişir mi: hayır (ileti ve durma sırası).

## dialel_cizgi_test

- Artırılmış blok tasarımı: "Düzeltilmiş ortalamalar" tablosunda artırılmış genotiplerin standart hatası blok düzeltmesinin belirsizliğini içermiyordu (örnek veride 5,1935) → SH = √(KO_h (1 + 1/c − 1/(b c))) (örnek veride 5,6096); grafikteki güven aralıkları da bu değerle çizilir; sonuç değişir mi: evet, artırılmış genotiplerin SH sütunu ve grafikteki aralıklar genişler (örnekte %8); düzeltilmiş ortalamalar, fark standart hataları ve ikili karşılaştırmalar aynı.
- Şablon açıklaması: artırılmış blokta Tukey seçeneğinin yaklaşık (Tukey–Kramer türü) olduğu ve düzeltilmiş ortalamanın SH tanımı açıklamaya yazıldı (TR, EN); sonuç değişir mi: hayır.
- Bütün kipler: yanıt sütunu değerleriyle, düzey sütunları tek biçimli metin olarak okunur; yanıtta sayı olmayan değer sessizce eksik sayılmak yerine çalıştırmayı iletiyle durdurur; sonuç değişir mi: evet, yalnız yanıt sütununda sayı olmayan değer bulunan veride (sonuç yerine ileti).

## dinamik_panel_gmm

- Dinamik panel GMM: aynı değişken hem "yalnız cari" hem "cari + 1 gecikme" listesinde seçilince gecikmesi sessizce modelden düşüyordu → "cari + 1 gecikme" olarak modele girer ve uyarı satırı yazılır; bağımlı değişken, birim ya da zaman sütunu açıklayıcı listesindeyse uyarı satırıyla çıkarılır; sonuç değişir mi: evet (yalnız bir değişken iki listede seçiliyse; gecikmesi artık modelde).
- Dinamik panel GMM: "İçsel açıklayıcılar" listesinde modelde açıklayıcı olmayan bir değişken seçilince sessizce yok sayılıyordu → anlaşılır iletiyle durur; sonuç değişir mi: hayır (eskiden hiçbir şey eklenmeden çalışırdı; şimdi durur).
- Dinamik panel GMM: dönemler 2'şer yıl aralıklı (1976, 1978, …) kodlanınca "system is exactly singular" ile duruyordu → eşit aralıklı dönemler 1, 2, … diye numaralanır ve not yazılır; sonuç değişir mi: hayır (1 adımlı kodlamayla aynı sonuç; eskiden hata).
- Dinamik panel GMM: eş doğrusal ya da her birimde sabit açıklayıcıda `pgmm`'in "Lapack … singular" iç hatası geliyordu → değişken adıyla "Açıklayıcılar eş doğrusal …" iletisiyle durur; sonuç değişir mi: hayır (eskiden hata).

## dizi_amova

- DNA dizileriyle AMOVA: ΦCT'nin (gruplar arası bileşenin) p değeri kullanılan paketin testinden geliyordu; bu değer Excoffier düzeninin p değeri değildi ve tohuma göre 0,000 ile 0,33 arasında oynuyordu (ör. doğrusu 0,333 olan veride 0,000) → p değerlerini şablon kendi permütasyon düzeniyle hesaplıyor (Excoffier, Smouse ve Quattro 1992: ΦST için bireyler bütün popülasyonlar arasında, ΦSC için grubun içinde popülasyonlar arasında, ΦCT için popülasyonlar bütün olarak gruplar arasında karıştırılır; gözlenene eşit düzenler sayılır, p = (sayım + 1)/(B + 1); olanaklı düzen sayısı permütasyon sayısını aşmıyorsa bütün düzenler sayılır) ve bunu not satırlarıyla bildiriyor; sonuç değişir mi: evet (bütün AMOVA ve ikili ΦST p değerleri; ΦCT'de belirgin, ötekilerde Monte Carlo farkı kadar ya da tam sayım değeri).
- DNA dizileriyle AMOVA: tek dizili bir grup varken ΦSC'nin p değeri 0,0000 çıkıyor, paketin uyarıları yalnız konsola düşüyordu → p değeri geçerli hesaplanıyor; sonuç değişir mi: evet (tek dizili grup içeren veride).
- DNA dizileriyle AMOVA: AMOVA ve ikili ΦST p değerleri güçlü yapıda 0,0000 yazılıyordu → p hiçbir zaman 0 yazılmaz; küçük popülasyon çiftlerinde ikili p değeri bütün bölünmelerin sayımından (kesin) gelir; sonuç değişir mi: evet (eskiden 0,0000 yazılan yerde 1/(B + 1) ya da tam sayım değeri).
- DNA dizileriyle AMOVA: ΦST'nin p değeri ayrı (grupsuz) bir çözümlemeden geliyordu → aynı iki düzeyli çözümlemenin düzenlerinden hesaplanıyor (bireyler bütün popülasyonlar arasında; popülasyon içi bileşen); sonuç değişir mi: evet (iki düzeyli çözümlemede ΦST'nin p değeri, Monte Carlo farkı düzeyinde).
- DNA dizileriyle AMOVA: K*ST, Hudson, Boos ve Kaplan'ın (1992) tanımıyla hesaplanmıyordu (popülasyon içi ortalamanın logaritması alınıyordu) → her dizi çiftinin fark sayısı d yerine ln(1 + d) konur, ortalamalar bunlardan alınır; sonuç değişir mi: evet (K*ST değeri ve p değeri; örnek veride 0,00636 → 0,00344).

## dizi_filogeni

- Dizi uzaklıkları ve filogeni: "Ratchet" aramasında düğüm destekleri hemen hepsi %100 yazılıyor ve "ultra hızlı bootstrap" diye etiketleniyordu (örnek veride 11 düğümün 10'u 100) → ratchet aramasında destek standart bootstrap ile (B tekrar) hesaplanır ve "standart bootstrap (%)" diye adlandırılır, B = 0 ise destek yazılmaz, çıktıya açıklayıcı not düşülür; sonuç değişir mi: evet (ratchet kipinde ML ağacının destek değerleri).
- Dizi uzaklıkları ve filogeni: stokastik ML araması tek başlangıçtan yapılıyordu ve tohuma göre farklı ağaç/log-olabilirlik veriyordu (tohum 7: −2615,3905; tohum 2026: −2615,0147) → arama 5 bağımsız başlangıçla (40'tan çok dizide 3, 100'den çok dizide 2) yinelenir, log-olabilirliği en yüksek ağaç alınır ve başlangıçların log-olabilirlikleri yazılır; model seçimi tohumdan bağımsız sabit (NJ) başlangıç ağacından yapılır; sonuç değişir mi: evet (ML ağacı ve log-olabilirlik; model seçimi tablosunda log-olabilirlikler 1e-5 göreli düzeyinde; örnek veride her iki tohum −2615,0147'ye varır); çalışma süresi başlangıç sayısı kadar uzar (örnek veride yaklaşık 15 sn → 70 sn).
- Dizi uzaklıkları ve filogeni: gamasız modelde (GTR, GTR+I) "Gama biçim parametresi" satırına 1 yazılıyordu → boş bırakılır; sonuç değişir mi: evet (yalnız gamasız modelde özet satırı).
- Dizi uzaklıkları ve filogeni: "Model seçiminde +F" DNA'da hiçbir şey değiştirmiyordu ve bunu söylemiyordu → seçenek korundu, DNA'da etkisiz olduğunu söyleyen not yazılır, form açıklaması düzeltildi; sonuç değişir mi: hayır.
- Dizi uzaklıkları ve filogeni: "ML model parametreleri" tablosunun başlığı taban frekanslarının gözlenen frekanslar olduğunu söylemiyordu → başlığa eklendi; sonuç değişir mi: hayır.
- Dizi uzaklıkları ve filogeni: yalnız N/?/- ile ayrışan diziler ayrı haplotip sayılıyor, değişken konum yokken sıfır dallı ağaç kuruluyordu → haplotip, her konumda uyumlu (eksik/belirsiz kodlar çakışabilen) dizilerin tek grubudur; yalnız bir haplotip ya da değişken konum yoksa şablon açık iletiyle durur; sonuç değişir mi: evet (eksik verili dizilerde "Ayrı haplotip" sayısı ve "aynı dizi" notu; eskiden ağaç kurulan, hiç değişken konumu olmayan veride artık durur).
- Dizi uzaklıkları ve filogeni: hiç ortak bilinen konumu olmayan iki dizi "doygunluk" iletisiyle bildiriliyordu → hangi iki dizi olduğunu söyleyen ayrı ileti; sonuç değişir mi: hayır (ileti).
- Dizi uzaklıkları ve filogeni: NJ bootstrap'ında uzaklığı tanımsız çıkan tekrarlar sessizce p uzaklığıyla kuruluyordu → kaç tekrarın düştüğü not olarak yazılır; sonuç değişir mi: hayır.
- Dizi uzaklıkları ve filogeni: NJ ve NNI desteği Newick'te oran (0,92), stokastik ML'de yüzde (97) yazılıyordu → hepsi yüzde; sonuç değişir mi: evet (NJ ve NNI ağaçlarının Newick metni; şekiller aynı).
- [3.8.0] Dizi filogenisi: yeni seçenek "ML'de taban frekanslarını olabilirlikle kestir" (varsayılan kapalı = gözlenen frekanslar): açıkken F81/HKY/GTR frekansları en çok olabilirlikle kestirilir (`statefreq = "estimate"`; örnek veride GTR log-olabilirlik −2615,329 → −2614,909); sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_dizi_filogeni.py`).

## dizi_hizalama

- Dizi hizalama: tek baz silinmiş (çerçevesi kaymış) tek dizi, çeviri üzerinden (`decipher_ceviri`) hizalamanın bütününü bozuyordu (kurgusal veride genişlik 412 yerine 430, değişken konum 68 yerine 166) ve uyarı yalnız durdurma kodonunu söylüyordu → şablon çerçeve kaymasını denetler, kaymış diziyi çeviri hizalamasından çıkarıp nükleotit düzeyinde ekler ve bunu uyarı ve notla bildirir (çeviri denetimi tablosuna `cerceve_kaymasi` sütunu eklendi); diğer yöntemlerde yalnız uyarı yazılır; sonuç değişir mi: evet (çerçevesi kaymış dizi içeren veride `decipher_ceviri`; kurgusal veride artık 412 sütun, 68 değişken konum, doğru hizalamayla birebir aynı).
- Dizi hizalama: 300'den çok dizide ortalama ikili özdeşliğin 20 000 çiftlik rastgele örneklemden hesaplandığı söylenmiyordu → satır adı bunu belirtir; sonuç değişir mi: hayır.
- Dizi hizalama: FASTA dosya yolunun klasörü yoksa dosya yazılmıyordu → klasör oluşturulur; sonuç değişir mi: hayır (dosya artık yazılır).
- Dizi hizalama: kırpma baştan 3'ün katı olmayan sayıda sütun çıkarınca okuma çerçevesinin kayabileceği bildirilmiyordu → not yazılır; sonuç değişir mi: hayır.
- Dizi hizalama: "Dışa aktarım klasörü" alanı açıklamasında "koşunun iş klasörü" → "çalıştırmanın iş klasörü"; sonuç değişir mi: hayır.

## dmf_ileri

- İleri DMF: madde arındırması yakınsamadığında hiçbir şey yazılmıyordu → şimdi uyarı yazılır ve özet başlığı "arındırma yakınsamadı" der; sonuç değişir mi: hayır (yalnız bildirim).
- İleri DMF: Zumbo-Thomas ve Jodoin-Gierl sınıfı testin anlamlı olmadığı maddede de B/C verebiliyordu → anlamsız madde A; sonuç değişir mi: evet (anlamsız maddelerin sınıfı).
- İleri DMF: şekildeki kırmızı noktalar düzeltmesiz eşiğe göre boyanıyordu → yöntemin DMF kararına göre (düzeltme dahil); sonuç değişir mi: yalnız şekil, düzeltme seçiliyse.
- İleri DMF: eksik yanıt nedeniyle atılan satır sayısı yazılmıyordu → sayı, yüzde ve %10 üstünde uyarı; sonuç değişir mi: hayır.
- İleri DMF: bir grupta herkesin aynı yanıtı verdiği maddede anlamsız sonuçlar (MH sonsuz, ETS B, lojistik ayrışma) üretiliyordu → o madde değerlendirilmez (sütunlar boş, DMF yanlış), not yöntemden bağımsız; sonuç değişir mi: evet (yalnız o madde).
- İleri DMF: grup düzey adındaki baş/son boşluk odak seçimini engelliyordu → düzeyler kırpılır; sayı kodlu grupta düzey sırası sayısal; boş odakta hangi düzeyin odak alındığı yazılır; sonuç değişir mi: boşluklu adlarda artık çalışır, öteki durumlarda hayır.

## dogrusal_olmayan_esanli

- Doğrusal olmayan eşanlı denklemler: yinelemeli SUR ve yinelemeli 3SLS dış döngüsü "en çok S güncellemesi" sınırına yakınsamadan ulaşınca sonuç yakınsamış gibi yazılıyordu → "UYARI: … yakınsamadı; kestirimler yinelemenin ara noktasıdır" satırı yazılır ve uyum ölçütleri tablosuna "Dış döngü S güncellemesi" ile "Dış döngü yakınsadı (1 = evet, 0 = hayır)" satırları eklenir (yalnız ITSUR ve IT3SLS); sonuç değişir mi: hayır (sayılar aynı; yalnız uyarı ve iki ek satır; yakınsayan çalıştırmalarda uyarı yok).
- Doğrusal olmayan eşanlı denklemler: başlangıç değerinde ondalık virgül ("d1 = -0,2") ayırıcı sayılıyor ve ileti nedeni söylemiyordu → ileti "ondalık ayırıcı nokta olmalıdır" der; sonuç değişir mi: hayır.
- Doğrusal olmayan eşanlı denklemler: eksik değer yüzünden atılan satır sayısı yazılmıyordu → "N eksik değerli satır çıkarıldı (kullanılan n = …)" satırı yazılır; sonuç değişir mi: hayır.
- Doğrusal olmayan eşanlı denklemler: Veri sekmesinde "kategorik" türdeki sayısal sütun denklem metninde "sayısal olmalı" diye reddediliyordu (aynı sütun araç listesindeyse çalışıyordu) → etiketleri sayı olan sütun sayı olarak kullanılır, sayı olmayan etiketlerde sütun ve değer adıyla ileti; sonuç değişir mi: hayır (eskiden hata; sonuç sayısal türle aynı).

## doluluk_yakalama

- Doluluk modeli, cloglog bağlantı: geri dönüştürülmüş ψ'nin standart hatası paketin yanlış değerinden (örnekte 0,0246) yazılıyordu → SH(ψ) = SH(η)·e^η·exp(−e^η) olarak şablonda delta yöntemiyle hesaplanır (0,0425; logit bağlantıyla aynı veride 0,0425) ve güven aralığı bağlantı ölçeğinde kurulup geri dönüştürülür; çıktıya not yazılır; sonuç değişir mi: evet (yalnız cloglog: ψ'nin SH'si ve GA'sı; katsayı tablosu ve ψ aynı); logit bağlantıda hayır.
- Doluluk ve N-karışım: eniyileme kötü bir noktada duruyordu (standartlaştırmasız + cloglog: AIC 371,38 yerine 356,76; Hessian tekil) ve eğri çizimi "need finite 'ylim' values" hatasıyla bütün çıktıları kaybettiriyordu → kötü nokta saptanınca (yakınsama kodu ≠ 0 ya da sonlu olmayan SH) farklı başlangıç noktalarıyla yeniden eniyilenir ve daha iyi çözüm alınır (not yazılır); sonlu olmayan güven sınırında band çizilmez; çizim hatası tabloları kaybettirmez (şekil için not yazılır); sonuç değişir mi: evet, yalnız ilk eniyilemenin kötü noktada durduğu durumlarda (düzeltilmiş çözüm); iyi yakınsayan modellerde hayır.
- Doluluk ve N-karışım, aday model tablosu: aday modellerin yakınsama durumu denetlenmiyor, bozuk aday ΔAIC ve ağırlığı bozuyordu → aday modeller de gerekirse yeniden eniyilenir, tabloya son sütun "uyum" (OK / yakınsamadı / SH yok) eklenir ve OK olmayanlar ΔAIC ve ağırlığa katılmaz; sonuç değişir mi: hayır (bütün adaylar OK ise); bozuk aday varsa evet.
- Yakala-yeniden yakala, profil güven aralığı tablosu: "bolluk" sütunu model tablosundaki kestirimden farklı bir sayıydı (hare: 81,08 yerine 80,08) → "bolluk" model tablosundan alınır, profil olabilirlik tepe noktası ek "bolluk_profil" sütunundadır, not yazılır; aralık sınırları değişmez; sonuç değişir mi: yalnız tablodaki "bolluk" sütunu (profil tepe noktası → model tablosu değeri).
- Yakala-yeniden yakala, Lincoln–Petersen ve Chapman: normal yaklaşımlı alt sınır iki dönemde yakalanan farklı birey sayısının altına inebiliyordu (örnek: 24,0 ve 37,3, oysa 40 birey yakalanmış) → alt sınır n1 + n2 − m2'ye yükseltilir ve not yazılır; sonuç değişir mi: evet, yalnız alt sınır bu sayının altına inecek durumlarda; kestirim, SH ve üst sınır aynı.
- Doluluk ve yakalama: atlanan çıktı bölümleri (görgül Bayes, bootstrap, aday model, eğri) neden eksik olduğu söylenmeden sessizce düşüyordu → her atlama için nedenli not yazılır; eksik kovaryat hücre sayısı kovaryat başına yazılır; NB/ZIP bileşenleri "yayılım (α)" ve "sıfır şişme (ψ)" diye adlandırılır; N-karışım bootstrap'ında Poisson varyanslı Pearson tipi istatistik notu yazılır; sonuç değişir mi: hayır (yalnız metin).

## dort_parametreli_lojistik

- Dört parametreli lojistik: ilk çıktı satırı alt asimptot sabitlendiğinde de 'LL.4' yazıyordu ve standart sapmaya 'artık SS' deniyordu → serbest parametre sayısına göre 'LL.4 / LL.3 / LL.2' yazılır, 'artık standart sapma' denir; sonuç değişir mi: hayır (yalnız etiket).

## doz_yanit_meta

- Doz-yanıt meta-analizi, tek aşamalı yaklaşım: Q hesaplanmadığı hâlde I² "0" yazılıyordu ("heterojenlik yok" gibi okunuyordu) → Q ve I² tanımsız (boş) yazılır ve not eklenir; sonuç değişir mi: evet (tek aşamalı yaklaşımda I² 0 → boş); iki aşamalıda hayır.
- Doz-yanıt meta-analizi: paket "yakınsamadı" uyarısı verdiği hâlde tablolar yakınsamış gibi yazılıyor, uyarı yalnız sonda görünüyordu → kestirimden hemen sonra "UYARI: kestirim yakınsamadı" yazılır ve katsayı tablosu başlığına "YAKINSAMADI" eklenir; sonuç değişir mi: hayır (sayılar aynı, uyarı eklendi).
- Doz-yanıt meta-analizi: çalışmalar arası kovaryans matrisi sınırda (|korelasyon| ≈ 1) kestirildiğinde bildirilmiyordu → "Ψ sınırda kestirildi" uyarısı yazılır; sonuç değişir mi: hayır.
- Doz-yanıt meta-analizi: SH boş, vaka sayısı 0 ve yetersiz kategori durumlarında paketin anlaşılmaz İngilizce iletileri → önceden denetlenir, açık ileti (TR/EN) ile durur; sonuç değişir mi: hayır (yalnız ileti).
- Doz-yanıt meta-analizi: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve çalışmaları yazılır; sonuç değişir mi: hayır.
- Doz-yanıt meta-analizi: çalışmaların referans dozları farklıysa tahmin tablosunun referansı belirsizdi → not yazılır; sonuç değişir mi: hayır.
- Doz-yanıt meta-analizi: çok küçük doz adımı 5000 satırda sessizce kesiliyordu → ızgara 1000 satırı aşmaz, adım büyütülür ve not yazılır; sonuç değişir mi: evet (adım ızgarayı 1000 satırdan uzun yapıyorsa).

## durum_uzay_genel

- Genel durum-uzay modeli (çok değişkenli): bir serinin bütün varyansları sıfıra yakınken olabilirlik yapay olarak çöküyor (iki bağımsız rastgele yürüyüşte log-olabilirlik 0,000000 ya da tek serinin değeri, varyanslar sıfır) → bu noktalar reddedilir, doğru eniyi bulunur (−338,05); sonuç değişir mi: evet (yalnız bir serinin varyansları sıfıra yakın çıkan çok değişkenli modellerde); örnek ayar aynıdır (log-olabilirlik 364,472692).
- Genel durum-uzay modeli: eniyileyicinin yakınsama durumu okunmuyordu (en çok yineleme küçükken eniyi olmayan sonuç uyarısız yazılıyordu) → varyans tablosundan önce "eniyileyici yakınsama bildirmedi" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Genel durum-uzay modeli (ortak açıklayıcı katsayısı): "Seriler için ortak" katsayıda bir katsayı 0 ve standart hatası boş çıkıyor, log-olabilirlik/AIC yanlış çıkıyordu (KFAS dağınık evreyi erken bitiriyordu) → katsayı tanımlanamayınca olabilirlik ve eniyileme KFAS'ın `augment` dönüşümüyle yeniden yapılır ve not düşülür; yine tanımlanamazsa log-olabilirlik, AIC ve BIC için güvenilmezlik uyarısı yazılır; sonuç değişir mi: evet (yalnız "ortak" katsayı seçildiğinde; örnek veride `law` 0 yerine −0,198, SH 0,048, log-olabilirlik 354,3 yerine 342,8).
- Genel durum-uzay modeli: adı `level`, `slope` ya da `sea_` ile başlayan açıklayıcının katsayısı "Regresyon katsayıları" tablosundan sessizce düşüyordu → açıklayıcılar iç adlarla modele girer, tabloda özgün ad yazılır; sonuç değişir mi: evet (yalnız bu adlarla başlayan açıklayıcılarda; eskiden tablo yoktu); öteki adlarda aynı.
- Genel durum-uzay modeli: aynı sütun hem seri hem açıklayıcı seçilince açıklayıcı sessizce atılıyordu → uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı).

## es_kriging

- Eş-kriging: çapraz doğrulamada ikincil değişkenler kestirilen konumda bilindiği için kazanç abartılıyordu → tabloya konumdaki bütün değişkenlerin dışarıda bırakıldığı üçüncü satır eklendi (örnek veri: RMSE 0,3918 / 0,1106 / 0,4010); ilk iki satır aynı; sonuç değişir mi: hayır (satır eklendi), yorum değişir.
- Eş-kriging: pozitif tanımlılık denetimi çok küçük özdeğerleri kaçırıp gstat'ın "yasal olmayan model" hatasıyla duruyordu (ham ölçek, Gauss model) → bağıl eşik ve her çarpanda gstat sınaması; çarpan 1,01 ile çalışır (RMSE 48,61); sonuç değişir mi: yalnız eskiden hata veren durumlarda.
- Eş-kriging: açıklama kestirim varyansı haritası çizildiğini söylüyordu → açıklama koda uyduruldu (harita kestirimdir, ortalama varyans tabloda).

## esem

- Açımlayıcı yapısal eşitlik modeli (ESEM): kestirici MLR seçiliyken özet cümlesi ölçeklenmemiş χ², p, CFI ve RMSEA'yı yazıyordu → cümle ölçeklenmiş χ² ve p ile sağlam CFI ve RMSEA'yı yazar ve bunu belirtir; sonuç değişir mi: evet, yalnız MLR özet cümlesinde (örnekte χ²(24) 42,042 → 42,967; p 0,0128 → 0,0100; CFI 0,981 → 0,980; RMSEA 0,050 → 0,051); tablolar ve ML sonuçları aynı.
- Uygunsuz (Heywood) çözüm (negatif özgül varyans ya da 1'i aşan ortak varyans) "yakınsadı" sayılıp yalnız çevrilmemiş İngilizce bir uyarıyla sunuluyordu → açık, çevrilmiş "uygunsuz (Heywood) çözüm" UYARI satırı (etkilenen göstergelerle) ve özet cümlesinden sonra çıktı notu yazılır; sonuç değişir mi: hayır (sayılar aynı; yalnız uyarı eklenir).
- Döndürme rastgele başlangıç sayısı 0 ya da 1 iken yerel (daha kötü) döndürme çözümü sessizce verilebiliyordu → 5'ten küçük değerde UYARI satırı yazılır (değer aralığı ve varsayılan 30 değişmedi); sonuç değişir mi: hayır.
- Eksik değerli satırlar ve hem gösterge hem ortak değişken seçilen sütun sessizce atılıyordu → kullanılan gözlem sayısı, atılan satır sayısı ve ortak değişken listesinden çıkarılan sütun çıktıda bildirilir; sonuç değişir mi: hayır (yalnız bildirim).
- Serbestlik derecesi hata iletisinde Türkçe sayılar yer değiştirmişti ("2 gösterge için 4 faktör") → doğru sıra ("4 gösterge için 2 faktör"); sonuç değişir mi: hayır (yalnız ileti).

## esik_ar

- Eşikli otoregresif model (en küçük AIC yöntemi): iki rejimin kesişim seçenekleri farklıyken üst rejimin mertebesi yanlış bildirilip yanlış kuruluyordu (başlıkta SETAR(2; 7, 1) ya da (2; 7, 3), oysa seçilen 2) → mertebe rejim tasarım matrisinden okunur; sonuç değişir mi: evet (yalnız MAIC ve alt/üst kesişim seçenekleri farklıyken); kesişim seçenekleri aynıyken aynı.
- Eşikli otoregresif model: doğrusallık testinin p değeri doğrusal bir seride negatif çıkabiliyordu (−1,55) → istatistik AR mertebesi + 1'in altındaysa p = 1 yazılır ve yaklaşımın geçersiz olduğu notu düşülür; sonuç değişir mi: evet (yalnız istatistik < p + 1 iken; eskiden geçersiz negatif p); öteki durumlarda aynı.
- Eşikli otoregresif model: bir rejimin AR mertebesi 0 (yalnız kesişim) seçilince "logical subscript too long" hatasıyla duruyordu → yalnız kesişimli rejim kestirilir; sonuç değişir mi: hayır (eskiden hata).
- Eşikli otoregresif model: nominal AIC dört anlamlı basamağa yuvarlanmış yazılıyordu (−34,83) → tam duyarlıkla yazılır (−34,8344); sonuç değişir mi: evet (AIC'nin yalnız yuvarlama basamakları).

## esnek_parametrik

- Esnek parametrik sağkalım: Eniyileyici yineleme sınırında durduğunda uyarı verilmiyordu → "Eniyileyici yakınsamadı" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı eklenir; varsayılan `goreli_tol` değişmedi).
- Esnek parametrik sağkalım: Zamana bağlı etki (gamma1) satırlarında HR/OR sütunu tehlike oranı gibi dolu geliyordu → bu satırlarda sütun boş, çıktıya açıklayıcı not eklendi; sonuç değişir mi: hayır (yalnız bu hücreler boş gelir).
- Esnek parametrik sağkalım: Odds ölçeğindeki OR olay odds oranıdır ama başlık "log odds oranı" idi → başlık "log olay odds oranı", çıktıya not eklendi; sonuç değişir mi: hayır (yalnız metin).
- Esnek parametrik sağkalım: Eksik değerli satırlar sessizce çıkarılıyordu → "N eksik değerli satır çıkarıldı" notu; sonuç değişir mi: hayır.

## f_istatistikleri

- F-istatistikleri: ΦST (mmod) verinin satır sırasına bağlıydı; örnek veride dosya sırasıyla −0,192 çıkıyordu, doğrusu 0,184 → bireyler pakete popülasyon düzeyine göre sıralanarak verilir; ΦST artık satır sırasından bağımsız (genel 0,18424470; bağımsız hesapla aynı); `phi_st` açıkken genel kestirimin kaç bireye dayandığı not olarak yazılır; sonuç değişir mi: evet (ΦST'yi açan ve verisi popülasyona göre sıralı olmayan her çalıştırmada; lokus başına ΦST de değişir; öteki ölçütler değişmez).
- F-istatistikleri: 5'ten az çok biçimli lokusta bootstrap istenince çalıştırma paketin İngilizce hatasıyla duruyor, hiçbir tablo verilmiyordu → bootstrap atlanır, not yazılır, nokta kestirimleri ve tablolar verilir; bootstrap yapılamayan çalıştırmada ikili tabloda boş `FST_alt`/`FST_ust` sütunları ve başlıktaki "bootstrap GA" ifadesi yer almaz; sonuç değişir mi: evet (2–4 lokuslu çalıştırmalar eskiden hata veriyordu; `B = 0` çalıştırmalarında ikili tablo iki sütun az).
- F-istatistikleri: alel adı "0" olan (0/1 kodlu) lokuslarda "0" alelli genotipler sessizce atılıyor, bütün lokuslar "tek biçimli" çıkıyordu; `-9/-9` alel sayılıyordu → ortak genotip okuma kuralı: `0/1` görülürse çalıştırma nedenini söyleyerek durur, `-9` eksik kodudur, eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (yalnız `0/1` ya da `-9` içeren veride). Üstbilgi açıklaması (TR/EN/ES) güncellendi.
- F-istatistikleri: Veri sekmesinde "kategorik" ya da metin türünde gelen 0/1/2 SNP sütunu "ayırıcı içermeyen genotip" iletisiyle reddediliyordu → değerlerinin hepsi 0, 1, 2 olan sütun sayısal kodlama olarak okunur; sonuç değişir mi: hayır (eskiden hata).
- Eksik genotip kodları (yeni alan): F-istatistikleri: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## fa_karma_model

- Faktör analitik karma model: kararlılık ölçütü (RMSD) başlıkta anılan kaynağın tanımından farklıydı: sapma toplam genotip × çevre etkisinden (özgül etkiler dahil) alınıyordu → Smith ve Cullis (2018) Eş. 7 ve 9'daki gibi ortak (faktörlerle açıklanan) etkilerin birinci faktör regresyonundan sapması; gizil regresyon grafiği de ortak etkiyi çizer; tek faktörlü modelde ölçüt sıfırdır ve not yazılır; sonuç değişir mi: evet, `kararlilik_RMSD` sütunu ve kararlılık sıralaması (örnek: AC03452-2W 0,278 → 0,167; Accumulator 1,609 → 0,825); OP, skorlar, tahminler ve kovaryanslar değişmez.
- Faktör analitik karma model: "Yakınsama: evet", Hessian pozitif tanımlı olmadığında da yazılıyor; bazı modellerde eniyileyici tepeye varmadan duruyordu → eniyileme durduğu noktadan sürdürülür (köşegen ML modelinde −2 log L 4194,5011 → 4194,4639); yakınsama yalnız eniyileyici olağan durmuş ve Hessian pozitif tanımlıysa "evet", Hessian pozitif tanımlı değilse "doğrulanamadı" yazılır; karşılaştırma tablosuna `yakinsama` sütunu, uyum tablosuna "En büyük |gradyan|" satırı eklendi; sonuç değişir mi: evet, yalnız eniyilemenin erken durduğu modellerde −2 log L, AIC, BIC ve kovaryanslar (örnek veride ML'de köşegen ve FA1; REML sonuçları aynı).
- Faktör analitik karma model: ML'de "Kovaryans parametresi" satırı sabit etkileri de sayıyordu; karşılaştırma tablosunda "(DIAG)" kod adı, özet cümlesinde yanlış iyelik eki ("%69,2'ini"), form açıklamasında ticari yazılım adları vardı → satırın adı ML'de "Parametre (kovaryans + sabit etki)"; "Köşegen"; "%60,8 kadarını"; adlar kaldırıldı; sonuç değişir mi: hayır.

## filogenetik_karsilastirmali

- Filogenetik karşılaştırmalı çözümleme, bağımsız karşıtlıklar: "dayanıklı karşıtlıklar" eşiği yalnız pozitif aykırı artıkta işliyordu, en uç artık negatifse hiçbir karşıtlık dışlanmıyordu → |öğrencileştirilmiş artık| eşiği iki yönlü uygulanıyor, tablo başlığı ve not dışlanan sayısını yazıyor, şekilde dışlananlar halkayla işaretleniyor; sonuç değişir mi: evet (eşik > 0 iken eşiği aşan negatif artık varsa; örnek veride eşit dal + eşik 3: eğim 0,6893 → 0,7155).
- Filogenetik karşılaştırmalı çözümleme, Blomberg K: permütasyon p'si, gözlenene eşit düzenleri kayan nokta farkıyla yarı yarıya sayıyordu (dört türlü ağaçta 0,166; doğrusu 0,333) → eşit düzenler 1e-9 göreli toleransla sayılıyor, n! ≤ permütasyon sayısı iken bütün düzenler sayılıyor ve bir not satırı hangi yolun kullanıldığını yazıyor; sonuç değişir mi: evet (küçük ağaçlarda p yukarı; büyük ağaçlarda Monte Carlo farkı kadar).
- Filogenetik karşılaştırmalı çözümleme: sinyal tablosundaki Pagel λ üst sınırda en büyük olabilirliğe ulaşmıyor, aynı çıktıda iki farklı λ ve log-olabilirlik görünüyordu → λ sınıra yakınsa yeniden aranıyor; sonuç değişir mi: evet, çok küçük (örnek veride λ 0,999927 → 1, log-olabilirlik −30,325145 → −30,319774; p değerleri aynı büyüklükte).
- Filogenetik karşılaştırmalı çözümleme, OU modeli: α arama aralığının üst sınırına dayanınca bildirim yoktu → not yazılıyor; sonuç değişir mi: hayır.

## fonksiyonel_veri

- Fonksiyonel veri analizi: işlev regresyonu tablosundaki "hata SS" satırı kareler toplamını değil artık standart sapmayı yazıyordu → satır "artık standart sapma" (İngilizce "residual SD") diye adlandırılır; sonuç değişir mi: hayır (yalnız etiket).
- Fonksiyonel veri analizi (B-spline temeli): temel bileşen öz değerleri ve işlev regresyonu fda'nın sayısal integralinden (göreli hata 1,7e-4) çıkıyordu, puanlar ise tam Gram matrisiyle hesaplanıyordu → öz değerler ve regresyon tam (Gauss-Legendre) integralle hesaplanır; sonuç değişir mi: evet, B-spline temelinde 1e-6 ile 5e-5 düzeyinde (öz değer 1: 15098,758 → 15098,723; F 19,5045 → 19,5036); Fourier temelinde hayır.
- Fonksiyonel veri analizi (varimax açık): "oz_deger" sütunu açıklamanın tersine döndürülmüş bileşen varyansını gösteriyordu → "oz_deger" döndürmeden önceki öz değerdir, döndürülmüş bileşen varyansı tablonun sonuna eklenen "dondurulmus_varyans" sütunundadır; sonuç değişir mi: evet (yalnız varimax açıkken "oz_deger" sütunu: 6421,39 → 15179,08 vb.; varyans oranları aynı).
- Fonksiyonel veri analizi (cezalı β(t), β pürüzlülük cezası > 0): F ve p testi nominal temel boyutuyla yazılıyordu → etkin serbestlik derecesiyle (şapka matrisi izi) hesaplanır ve tabloya "β etkin serbestlik derecesi (cezalı)" satırı eklenir; sonuç değişir mi: evet, yalnız β cezası > 0 iken (örnek: λβ = 1e12: F 19,956 → 24,96; p 1,35e-8 → 3,03e-9); ceza 0 iken hayır.
- Fonksiyonel veri analizi: eksik değer nedeniyle atılan satır sayısı ve yanıtı eksik olduğu için regresyondan çıkan eğri sayısı sessizdi → konsola not yazılır; sonuç değişir mi: hayır.

## fraktal_boyut

- Fraktal boyut kestirimleri (2B kutu sayma): ondalıklı koordinatlar her zaman 512 × 512 ızgaraya yerleştiriliyor, aynı küme ızgaraya göre farklı boyut veriyordu (Sierpinski halısı: tamsayı koordinatlarda 1,8928, ondalıklıda 1,7303) → koordinatlar sabit adımlı düzenli bir ızgaranın hücreleriyse ızgara kümenin kendisinden bulunur (halı ondalıklı koordinatlarda da 1,8928 verir); düzenli değilse 512 kullanılır ve çıktıya "ızgaraya bağlıdır" uyarısı yazılır; "2B ızgara kenarı" alanı elle verilirse davranış aynıdır; sonuç değişir mi: evet, "2B ızgara kenarı" = 0 iken düzenli ızgaralı ondalıklı koordinatlarda (eskiden yanlış ızgaradan çıkan boyut artık doğru ızgaradan çıkar); tamsayı koordinatlarda ve PNG görüntüde hayır.
- Fraktal boyut: grafik kutu sayma, belgede "fractaldim tanımı" denen şeyden farklıydı ve paket sütunuyla yan yana açıklamasız veriliyordu → belge ve kod yorumu farkı söyler, paket sütunu için çıktıya açıklayıcı not yazılır; sonuç değişir mi: hayır (yalnız metin; sütun adı ve değerleri aynı).
- Fraktal boyut: Katz değeri 2'yi aşabiliyordu (ölçeğe bağlıdır) ve bir uyarı yoktu → 2'yi aşarsa "fraktal boyut olarak yorumlanamaz" notu yazılır; "D ≈ 2" notu eşiği 1,95'ten 1,85'e indi ve Higuchi'ye ek olarak madogram ve variogram için de geçerli; regresyon yalnız 3 noktaya dayanıyorsa SH ve R² konusunda not yazılır; sonuç değişir mi: hayır (yalnız notlar).

## gaia

- GAIA: not, ölçüm ve elektroforez sütunları Veri sekmesinde "kategorik" türdeyse değerler yerine kategori sıra numaraları kullanılıyordu (örnek veride D 12,5 yerine 6,5; "ayırt edilir" yerine "ayırt edilmez") → sütunlar her zaman değerleriyle okunur; sayı olmayan bir değer varsa çalıştırma sütunu bildiren iletiyle durur; sonuç değişir mi: evet, karakter sütunu "kategorik" türde olan çalıştırmalarda (doğru sonuca döner).
- GAIA: "elektroforezi eklemek için en az not uzaklığı" koşulu sağlanmayan çiftte elektroforez ağırlığı uzaklığa eklenmediği hâlde "Karakter katkıları" tablosunda görünüyor, katkıların toplamı D'ye eşit olmuyordu → eklenmeyen ağırlık katkı tablosuna da girmez; sonuç değişir mi: hayır (uzaklıklar ve kararlar aynı; yalnız katkı tablosu).
- GAIA: ağırlık tablosundaki biçim hatası iletisi ondalık ayırıcıyı söylemiyordu → ileti "virgül girdileri ayırır; ondalık ayırıcı nokta olmalıdır" açıklamasını içerir; sonuç değişir mi: hayır.

## gamlss

- GAMLSS: Persentil alanına `2,5; 50; 97,5` yazılınca ondalık virgül ayırıcı sayılıp persentiller sessizce 2, 5, 50, 97 oluyordu → noktalı virgül varsa ayırıcı o, ondalık ayırıcı virgüldür (`2.5, 50, 97.5` yazımı eskisi gibi); okunan persentiller konsola yazılır; sonuç değişir mi: evet, yalnız ondalık virgülle yazılan persentillerde.
- GAMLSS: Karma (mixed) algoritmada "En çok dış döngü" etkisizdi ve örnekte algoritma yakınsamadan bitiyordu → alan CG döngü sayısı olarak uygulanır ve yakınsamama iletisi bu sayıyı anar; sonuç değişir mi: evet, karma algoritma seçildiğinde (örnekte 200 döngü yakınsamaz, 1000 döngü yakınsar: küresel sapma 4771,492537).
- GAMLSS: P-spline ve kübik spline terimlerinin "doğrusal kısım" satırlarındaki katsayı, SH, t ve p algoritmaya bağlıdır, bunu söyleyen bir not yoktu → tablodan sonra not yazılır; sonuç değişir mi: hayır.

## gamm_mgcv

- Genelleştirilmiş toplamsal karma model (gamm/mgcv): "Model uyumu" tablosundaki logLik, AIC ve R² ilişkili yapıda lme'den (marjinal), öteki yapılarda gam'dan (koşullu) geliyor ve satır adı bunu söylemiyordu; ilişkili yapıda REML ölçütü satırı boştu → satır adları kaynağı yazar ("marjinal, lme" / "koşullu, gam"), REML/ML ölçütü ilişkili yapıda −logLik ile dolar ve karşılaştırılamazlık notu yazılır; sonuç değişir mi: hayır (yalnız etiket ve ek bir satır; sayılar aynı).
- Genelleştirilmiş toplamsal karma model (gamm/mgcv): Taban boyutu k bütün düzgün terimlerde en az farklı değerli değişkene göre sessizce küçülüyordu → her terim kendi değişkeninin farklı değer sayısına göre sınırlanır ve küçültme çıktıya yazılır; sonuç değişir mi: evet, az farklı değerli bir değişken ile birlikte başka düzgün terim varsa (örnekte Days için k 5 → 9).

## garch_genis

- Genişletilmiş GARCH: getiri yerine fiyat düzeyi (durağan olmayan seri) verilince hiçbir uyarı çıkmıyor, kalıcılık 1'e dayanan anlamsız sonuç basılıyordu → serinin birinci derece otokorelasyonu 0,9'u aşarsa "fiyat düzeyi gibi görünüyor" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Genişletilmiş GARCH: zaman sütunu azalan sıradaysa uyarı yoktu → uyarı yazılır; sonuç değişir mi: hayır.
- Genişletilmiş GARCH: dışsal açıklayıcı katsayıları `mxreg1`, `mxreg2` adlarıyla yazılıyordu → seçilen sütun adlarıyla yazılır; sonuç değişir mi: hayır (yalnız etiket).
- Genişletilmiş GARCH: nlminb ve lbfgs seçilince kullanılmayan tol/delta denetimleri için her çalıştırmada paket uyarısı çıkıyordu → bu iki denetim yalnız solnp ve karma (hybrid) çözücüye geçirilir; sonuç değişir mi: hayır.

## genellenebilirlik

- Genellenebilirlik: dengesiz iç içe desende (yüzey 1 düzeylerinde farklı sayıda yüzey 2) D çalışması çiziminde çöküyor, tablolar kayboluyordu → yüzey 2 düzey sayısının medyanı tam sayıya yuvarlanır, notla bildirilir; sonuç değişir mi: yalnız önceden çöken durumlarda.
- Genellenebilirlik: iç içe desende "(dengeli desen)" yanlış yazılıyor, boş hücre notu verilmiyordu → dengesiz sayılır, not verilir; sonuç değişir mi: yalnız etiket.
- Genellenebilirlik: yüzey 2 gerçekte iç içeyken "çaprazlanmış" seçilirse yanlış model sessizce uyduruluyordu → anlaşılır iletiyle durur ("İç içe" seçin); sonuç değişir mi: evet (yanlış sonuç yerine ileti).
- Genellenebilirlik: lme4 yakınsama uyarıları bastırılıyordu → "Uyarı: model uydurma uyarı verdi …" yazılır; eksik puan nedeniyle atılan satır sayısı yazılır; ML seçilirse aşağı yanlılık notu; şekil alt başlığında n' yazılır; sonuç değişir mi: hayır.

## genellenebilirlik_py

- Genellenebilirlik (Python): negatif ANOVA kestiriminde tabloda negatif yazılıp yalnız D çalışmasında sıfırlanıyor, R ikiziyle farklı hata varyansı çıkıyordu → negatif bileşen 0 alınır, öteki bileşenler R ikiziyle aynı kısıtlı REML sınır çözümüyle yeniden kestirilir; ham ANOVA değeri notta; sonuç değişir mi: evet (yalnız negatif bileşen çıkan veride; örnekte σ²(δ) 0,365 → 0,336, R ile aynı).
- Genellenebilirlik (Python): iç içe veride "çaprazlanmış" seçilince yanıltıcı ileti → asıl neden söylenir ve R ikiziyle aynı ileti; eksik puan nedeniyle atılan satır sayısı yazılır; sonuç değişir mi: hayır.

## genetik_cesitlilik

- Genetik çeşitlilik: adı "0" olan alel taşıyan genotipler sessizce atılıyor (tek bir alel bütün alel zenginliği sütununu bozabiliyordu); `-9/-9` ise alel sayılıyordu → ortak genotip okuma kuralı (ortak kayıt 5): '0/143' gibi genotip görülürse çalıştırma durur; `-9` eksik kodudur; sonuç değişir mi: evet (ortak kayıt 5'teki durumlarda; `-9/-9` kodlu veride sonuçlar boş hücreli veriyle aynı olur: özel alel 28 → 26, g 22 → 20).
- Genetik çeşitlilik: tek bireyli lokus × popülasyon hücresinde F = 0 yazılıyor, tek bireyli popülasyonun F'si 0 ve bootstrap aralığı (0; 0) çıkıyordu → tek bireyli hücrede F boştur; popülasyon özetindeki F ve Hs ile bootstrap aralığı yalnız en az iki bireyli hücrelerden hesaplanır (hiç yoksa boş); uyarı hücre sayısını ve popülasyonları yazar; sonuç değişir mi: evet, yalnız tek bireyli hücresi olan popülasyonlarda (F ve Hs sütunları, bootstrap tablosu). Öteki popülasyonlar ve öteki sütunlar değişmez.
- Genetik çeşitlilik: alel zenginliğinin seyreltme büyüklüğünü tek bir küçük hücre belirlediğinde (g ≤ 4) bu yalnız özet satırından anlaşılıyordu → hücre (lokus, popülasyon, n) adıyla uyarı yazılır; g = 2'de alel zenginliğinin 1 + uHe'ye eşit olduğu belirtilir; tek biçimli lokus uyarısı ortalamaların düştüğünü söyler; sonuç değişir mi: hayır (yalnız uyarı metni).
- Genetik çeşitlilik: açıklama, yayımlanmış FIS değerinin tablonun F sütunuyla aynı olduğu izlenimini veriyordu (F = 1 − Ho/uHe; yayımlanan FIS = 1 − Ho/Hs) ve bootstrap için "boot.ppfis yaklaşımı" diyordu → açıklama hangi F'nin yazıldığını ve yayımlanan FIS'in Ho ile Hs sütunlarından nasıl elde edileceğini söyler; bootstrap açıklaması kestirici farkını belirtir; sonuç değişir mi: hayır (metin).
- Eksik genotip kodları (yeni alan): Genetik çeşitlilik: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).
- Genetik çeşitlilik: popülasyon özetine sona `F_Hs` = 1 − Ho/Hs sütunu eklendi (yayımlanan FIS; örnekte KCHB 0,079); öteki sütunlar ve değerleri aynı; sonuç değişir mi: hayır (yalnız ek sütun).

## genetik_uzaklik

- Genetik uzaklıklar: adı "0" olan alel taşıyan genotipler sessizce atılıyor, uzaklıklar ve ağaç uyarısız değişiyordu; `-9/-9` ise her popülasyonda ortak bir alel sayılıyordu → ortak genotip okuma kuralı (ortak kayıt 5): çalıştırma durur ya da `-9` eksik sayılır; sonuç değişir mi: evet (ortak kayıt 5'teki durumlarda; `-9/-9` kodlu veride uzaklıklar boş hücreli veriyle aynı olur: BTJF–CHIR Ds 0,1117 → 0,0945).
- Genetik uzaklıklar: "tek biçimli lokus uzaklığa katkı vermez" iletisi ve açıklaması yanlıştı; lokus tanım gereği (Nei 1972: ortalamalar tek biçimli lokuslar dahil bütün lokuslardan) lokus ortalamalarına girer ve Reynolds dışındaki bütün uzaklıkları küçültür (örnek: bir tek biçimli lokus eklenince Ds 0,0945 → 0,0746) → ileti ve açıklama düzeltildi; hesap değişmedi (tanımla ve başvuru paketleriyle aynı); sonuç değişir mi: hayır.
- Genetik uzaklıklar: hiçbir bootstrap ağacında görülmeyen dalın desteği tabloda boş, şekilde "NA" çıkıyordu → destek 0 yazılır; sonuç değişir mi: hayır (yalnız boş hücreler 0 olur; az tekrarlı çalıştırmalarda görülür).
- Eksik genotip kodları (yeni alan): Genetik uzaklık: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## genomik_seleksiyon

- Genomik seleksiyon (BayesB): zincirin yakınsadığına ilişkin tanı yoktu → artık varyans zincirinden Geweke z ve etkin örneklem büyüklüğü (coda tanımlarıyla) tabloya yazılır; |z| > 1,96 ya da etkin örneklem < 100 ise yakınsamamış olabileceği uyarılır; sonuç değişir mi: hayır (yalnız tanı eklendi; GEBV'ler aynı).
- Genomik seleksiyon: marker listesi boş bırakılınca cinsiyet gibi 0/1/2 sütunlar marker sayılıyor ve G'ye giriyordu; kategorik türdeki marker atlanıyordu → adı `<kromozom>_<konum>_<ad>` olan sütun varsa yalnız onlar marker sayılır, atlananların adları yazılır; kategorik türdeki 0/1/2 sütun da marker sayılır; sonuç değişir mi: evet (marker olmayan 0/1/2 sütunu bulunan ya da kategorik markerli veride; örnekte σ²e 0,23575 → 0,23640 farkı kalkar).
- Genomik seleksiyon: kategorik sabit etkide bir düzey tek bir kata sıkışınca bütün çalıştırma hata ile duruyordu → o kat atlanır, uyarı yazılır, çapraz doğrulama özeti kalan katlardan hesaplanır; sonuç değişir mi: hayır (eskiden hata).
- Genomik seleksiyon: BayesB seçili değilken ısınma sayısı yineleme sayısından büyükse çalıştırma duruyordu → denetim yalnız BayesB seçiliyken yapılır; sonuç değişir mi: hayır.
- [3.8.0] Genomik seleksiyon: BayesB "Yineleme / ısınma" alanları zaten vardı (varsayılan 3000/1000); alan açıklamalarına ve zincir tanısı uyarısına 12000/2000 önerisi eklendi (örnek veride tohum 7: 3000/1000'de Geweke z −3,92, etkin örneklem 70; 12000/2000'de −0,349 ve 539); sonuç değişir mi: hayır (varsayılanlar aynı; test `test_duzeltme_380_genomik_seleksiyon.py`).

## gizil_buyume

- Gizil büyüme modeli: kestirimde kaç gözlem kullanıldığı ve kaç satırın atıldığı hiçbir yerde yazılmıyordu → "Gözlem: … satırın … tanesi kestirimde kullanıldı; atılan satır: … (tüm zaman noktaları eksik; ortak değişken eksiği; liste boyu silme)" satırı ve uyum tablosunda `nobs`, `n_atilan` satırları (sona eklendi); sonuç değişir mi: hayır (yalnız bildirim).
- Uygunsuz (Heywood) çözüm (negatif artık ya da gizil varyans, |r| > 1) "yakınsadı" sayılıp yalnız çevrilmemiş İngilizce bir uyarıyla sunuluyordu → açık, çevrilmiş UYARI satırı ve çıktı notu; sonuç değişir mi: hayır (sayılar aynı; yalnız uyarı eklenir).
- Serbest biçimde "Zaman kodları" alanı doldurulunca sessizce kullanılmıyordu → kullanılmadığı bir not satırıyla yazılır; sonuç değişir mi: hayır.
- Kovaryatlı modelde "gizil ortalama" satırları koşullu kesişim olduğu halde öyle adlandırılmıyordu ve tabloda sabit kovaryat (ko)varyansları (SH 0) da yer alıyordu → satır "gizil kesişim (koşullu: ortak değişkenler 0'dayken)" adını alır, sabit kovaryat satırları tablodan çıkar; sonuç değişir mi: hayır (kestirimler aynı; yalnız etiket ve çıkarılan satırlar).
- Zaman kodlarında ondalık virgül (0; 0,5; 1; 1,5) ayırıcı sayılıp ondalığı anmayan bir iletiyle duruyordu → alanda ";" varsa kodlar ";" ya da boşlukla ayrılır ve virgül ondalık sayılır; ";" yoksa eski kural geçerli ve ileti ondalık için nokta ister; sonuç değişir mi: evet, yalnız ";" ile yazılan ondalıklı kodlarda (önceden hata veriyordu).
- Bireysel gidiş şeklindeki birey seçimi sabit tohumluydu → çalıştırma tohumunu kullanır; sonuç değişir mi: hayır (yalnız şekildeki 60 birey; tablolar aynı).

## gizil_etkilesim

- Gizil etkileşim modeli: LMS ya da QML yakınsamadığında (en çok yineleme sınırı) sonuçlar ve basit eğim şekli hiçbir uyarı olmadan veriliyordu → "UYARI: … yöntemi yakınsamadı" satırı ve sonuçlardan sonra yakınsama notu yazılır (sonuçlar yine gösterilir; en çok yineleme sayısını artırın); sonuç değişir mi: hayır (sayılar aynı; yalnız uyarı).
- Log-olabilirlik, AIC ve BIC yöntemler arasında yan yana duruyordu ama karşılaştırılamaz → uyum tablosunun altına "yalnız aynı yöntem içinde karşılaştırılabilir" notu; sonuç değişir mi: hayır.
- Eksik değer nedeniyle atılan satır sayısı yazılmıyordu → "n = …; yöntem …; eksik değer nedeniyle atılan satır: …" satırı; sonuç değişir mi: hayır (yalnız bildirim).

## gizil_gecis

- Gizil geçiş analizi: EM her durum sayısı için tek bir başlangıçtan çalışıyor, yerel en büyükte kalabiliyordu (örnek veride k = 3 için log-olabilirlik −6810,30; daha iyi çözüm −6798,23; "BIC'e göre 2 durum" sonucu bu yüzden yanlış çıkıyordu) → her durum sayısı için çok başlangıçlı EM (yeni "Başlangıç sayısı" alanı, varsayılan 10; 1 yazılırsa eski tek başlangıç); en yüksek log-olabilirlikli çözüm alınır, kullanılan başlangıç sayısı ve kaç başlangıcın aynı çözüme ulaştığı sonuç özetine, "Model uyumu" tablosuna ve karşılaştırma tablosuna (sona eklenen sütunlar) yazılır; sonuç değişir mi: evet (EM'in yerel en büyükte kaldığı durumlarda; örnek veride k = 3 ve BIC kararı 2 → 3 durum; k = 2 vinyet sonucu −6900,447 aynı).
- EM'in en çok yineleme sınırına ulaşıp ulaşmadığı okunmuyor, sınıra ulaşan çözüm sessizce yazılıyordu → yineleme sayısı "Model uyumu" tablosunda yazılır; sınıra ulaşılırsa uyarı verilir, karşılaştırma tablosunda `yakinsadi` sütunu; sonuç değişir mi: hayır (yalnız bildirim).
- Gizil durumların numarası kestirim sırasındaydı; rastgele başlangıçta "durum 1" çalıştırmadan çalıştırmaya değişiyordu → durumlar ilk göstergenin ilk kategorisinin yanıt olasılığına göre küçükten büyüğe numaralanır (çıktıya yazılır); ortak değişkenli modelde Be ve Ga tabloları kestirimin kendi numaralarıyla yazıldığı için eşleme notu verilir; sonuç değişir mi: evet (yalnız durum etiketleri ve satır sırası; modelin kendisi aynı; örnek veride etiketler aynı kaldı).
- Eksik değer nedeniyle atılan satır sayısı yazılmıyordu → atılan satır ve etkilenen birey sayısı yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Eşit aralıklı olmayan zaman değerleri (ör. 3. yıl yok) sessizce ardışık sayılıyordu → uyarı yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Gösterge kategorilerinin sırası sütun türüne göre değişiyordu ve yazılmıyordu → sayıya çevrilebiliyorsa sayısal sıra, değilse harf sırası; kategori sırası çıktıya yazılır; sonuç değişir mi: hayır (çok terimli model; yalnız tablo satır sırası).
- Ortak değişkenli modelde marjinal durum dağılımı şekli kişi-ortalamalı geçiş matrisiyle çiziliyordu → kişi düzeyinde özyinelenip kişiler üzerinden ortalanır; şekildeki 2. durum çizgisi okunur (6 ayrık renkli palet); "Rastgele başlangıç tohumu" alanı 0 yazılırsa kod panelindeki çalıştırma tohumu kullanılır (varsayılan 1 aynı); sonuç değişir mi: hayır (tablolar aynı; yalnız şekil).

## gizli_markov

- Gizli Markov modeli: geçiş ortak değişkeni seçilmeden çalıştırma "subscript out of bounds" hatasıyla çöküyor, hiçbir tablo üretilmiyordu → ortak değişkensiz model çalışır; geçiş olasılıkları tablosu yazılır (geçiş katsayısı tablosu yazılmaz); sonuç değişir mi: evet (eskiden hata; ortak değişkenli çalıştırmalar aynı).
- Gizli Markov modeli: Viterbi özeti ve işaretçileri olabilirlikten farklı zaman hizasıyla (varış anının ortak değişkeni) hesaplanıyordu → kalkış anı hizalı kendi Viterbi işlevi kullanılır; sonuç değişir mi: evet, geçiş ortak değişkeni varsa Viterbi durum sayıları 1 ile 3 gözlem değişir (örnek ayar: 183/256 → 184/255); ortak değişkensiz modelde hayır.
- Gizli Markov modeli: "başlangıç (yakınsayan)" satırı yakınsamayı okumadan hata vermeyen her başlangıcı sayıyordu → EM iletisinden yakınsama okunur, yakınsayan başlangıç tercih edilir, hiçbiri yakınsamazsa uyarı yazılır; tabloya "başlangıç (çalışan)" satırı eklenir; sonuç değişir mi: yalnız yakınsamayan başlangıçlı çalıştırmalarda (örnek ayarda hayır).
- Gizli Markov modeli: EM durma ölçütü katsayıları üçüncü basamakta belirsiz bırakıyordu → yeni "Yüksek duyarlık" seçeneği (tolerans en çok 1e-10, yineleme en az 2000); varsayılan kapalı = eski sonuçlar aynı; sonuç değişir mi: yalnız seçenek açılınca (örnek ayarda Pacc katsayısı 1,59855 → 1,59766; en çok olabilirlik 1,59763).
- Gizli Markov modeli: yetersiz gözlem iletisinde sayılar yer değiştirmişti ("20 durum için en az 2 gözlem") → "2 durum için en az 20 gözlem gerekli"; ayrıca hiçbir başlangıçtan çözüm alınamayınca iletiye sayısal 0/1 yanıtın normal sayıldığı eklendi; sonuç değişir mi: hayır (yalnız ileti).

## glmer_ileri

- Genelleştirilmiş doğrusal karma model (rastgele eğim, çoklu gruplama, AGQ), Laplace seçeneği: koşullu modların yinelemesi paketin varsayılan toleransında (1e-7) duruyor, standart hatalar tam Laplace değerinden %0,5–1,3 küçük, katsayılar 4. basamakta kayık çıkıyordu (cbpp: kesişim −1,39833, SH 0,23121) → tolerans 1e-9 alınıyor; sonuç tam Laplace en büyüğüdür (kesişim −1,39853, SH 0,23247; bağımsız hesap ve başka Laplace uygulamalarıyla 6–7 basamak aynı) ve çıktıya bir not satırı yazılıyor; sonuç değişir mi: evet (Laplace seçili her çalıştırmada katsayılar 4. anlamlı basamakta, standart hatalar %0,5–1,3, logLik 4. ondalıkta; rastgele eğimli modelde fark yok denecek kadar küçük; AGQ etkilenmez). lme4'ün varsayılan çıktısıyla (ve lme4 belgelerindeki cbpp örneğiyle) 4. basamakta fark vardır.
- Genelleştirilmiş doğrusal karma model (rastgele eğim, çoklu gruplama, AGQ): "Rastgele kesişim ve eğimler ilişkili" onayı kaldırılınca çalıştırma şekil aşamasında R hatasıyla duruyor, hiçbir tablo oluşmuyordu → ilişkisiz rastgele eğimli model artık tabloları ve şekliyle birlikte tamamlanıyor; sonuç değişir mi: evet (yalnız bu seçenekte; eskiden sonuç yoktu).
- Aynı şablon, AGQ + önsel ağırlık: "Model uyumu" tablosundaki logLik, −2logLik, AIC ve BIC ağırlıksız yoğunlukla hesaplanıyordu (ör. logLik −92,3953; doğrusu −129,2370; AIC 194,79 yerine 268,47) → yeniden hesap ağırlığı yoğunluğa katıyor; sonuç değişir mi: evet (yalnız AGQ ile önsel ağırlık birlikte seçildiğinde; katsayılar ve standart hatalar değişmez).
- Aynı şablon: AGQ'daki not yalnız toplulaştırılmış binomu anıyordu; konsol özetindeki logLik/AIC ile tablonun neden farklı olduğu Poisson'da açıklanmıyordu → not aileden bağımsız yazıldı (konsoldaki değer doymuş modele göre sapmadan türetilir); sonuç değişir mi: hayır.
- Aynı şablon: deneme sayısı sütunu sayısal değilse R'nin ham hatası çıkıyordu → "Deneme sayısı sayısal olmalı." iletisiyle duruyor; sonuç değişir mi: hayır.
- Aynı şablon: sabit etki listesindeki bir sütun aynı zamanda yanıt, deneme sayısı ya da gruplama faktörüyse listeden uyarısız çıkarılıyordu → çıkarılan sütunlar bir not satırıyla bildiriliyor; sonuç değişir mi: hayır.
- Aynı şablon: koşullu mod şeklindeki aralıklar güven düzeyi 0,95 iken 1,96, öteki düzeylerde tam kantille çiziliyordu → her düzeyde tam kantil; sonuç değişir mi: hayır (şekilde görünmeyecek kadar küçük fark).
- Aynı şablon: form açıklamasından ve kod yorumundan başka yazılımın adı çıkarıldı; sonuç değişir mi: hayır.

## glmmtmb_aileler

- glmmTMB aileleri (beta, Gamma, Tweedie, genelleştirilmiş Poisson, COM-Poisson, NB1): yayılım alt modelindeki bir değişken ortalama modelinde de varsa "Yayılım alt modeli" tablosu içeride ölçeklenmiş katsayıyı yazıyordu (GasolineYield, yayılım ~ temp: kesişim 6,2028; temp 1,016367; doğrusu 1,364089 ve 0,014570) → yayılım katsayıları ve standart hataları ortalama modelindekiyle aynı dönüşümle özgün ölçeğe çevriliyor; sonuç değişir mi: evet (yalnız aynı sayısal değişken hem ortalama hem yayılım modelindeyken; o değişkenin katsayısı ve SH'si ile yayılım kesişimi, kesişimin SH, z ve p değeri; eğimin z ve p değeri, logLik ve ortalama modeli değişmez).
- Aynı şablon, COM-Poisson: "Yayılım parametresi" satırındaki değer 1/ν iken açıklama ν'yü anlatıyordu; eksik yayılımlı veri "aşırı yayılım" diye okunuyordu → açıklama değerle aynı büyüklüğü anlatıyor ("φ = 1/ν: φ > 1 aşırı, φ < 1 eksik yayılım") ve ν ayrıca yazılıyor; sonuç değişir mi: hayır (tablodaki sayı aynı; yorumu düzeldi).
- Aynı şablon, COM-Poisson: çok aşırı yayılımlı veride yayılım parametresi sınıra gidiyor (ör. 1,1·10⁹), uyarı yazılmıyordu → sınır çözümü uyarısı ve aile önerisi yazılıyor; sonuç değişir mi: hayır.
- Aynı şablon: yakınsamayan modelde (log-olabilirlik hesaplanamıyor) tablolar boş değerlerle yazılıyordu → çalıştırma açık iletiyle duruyor; sonuç değişir mi: evet (yalnız yakınsamayan modelde; eskiden boş değerli tablolar çıkıyordu).
- Aynı şablon: etken sütunlarının düzey sırası ve başvuru (ilk) düzeyi çıktıya yazılıyor (iki düzeyli metin sütununda sıra alfabetiktir); sonuç değişir mi: hayır (aktarım değişmedi).
- Aynı şablon: form açıklamasından başka yazılımın adı çıkarıldı; Tweedie satırındaki iç satır adı kaldırıldı; sonuç değişir mi: hayır.

## goreli_sagkalim

- Göreli sağkalım: rapor zamanlarında "2,5; 7,5" yazımında virgül liste ayırıcısı sayılıp 2,5 yıl 2 ve 5 yıla bölünüyordu → alanda noktalı virgül varsa virgül ondalık sayılır (2,5 ve 7,5 yıl); noktalı virgülsüz "2,5,7,5" eski ayrımı korur ve çıktıya uyarı yazılır; sonuç değişir mi: evet (yalnız noktalı virgüllü ondalık virgül yazımında).
- Göreli sağkalım: Ederer I, Ederer II ve Hakulinen rapor zamanındaki değeri son gözlem zamanındaki değere yuvarlıyordu ve bunu söylemiyordu → tabloya sona "deger_zamani_yil" sütunu eklendi, fark 1 günden büyükse not yazılır (rapor zamanında kestirim için Pohar Perme); sonuç değişir mi: hayır (yalnız yeni sütun ve not).
- Göreli sağkalım: izlem nüfus tablosunun son yılını aşınca sessizce son yılın tehlikesi kullanılıyordu → çıktıya uyarı; yeni "İzlem nüfus tablosunun son yılını aşarsa" seçeneği (varsayılan "sürdür" = bugünkü; "durdur" çalıştırmayı durdurur); sonuç değişir mi: hayır.
- Göreli sağkalım: güven aralığı üst sınırı 1'i aşabiliyordu (ör. 1,0142) → [0, 1] içine çekilir ve not yazılır; sonuç değişir mi: evet (yalnız sınırın 0-1 dışına taştığı satırlarda).
- Göreli sağkalım: log-sıra testi başlığı seçilen tahminciden bağımsız (Pohar Perme tabanlı) olduğunu söylemiyordu → başlıkta yazılır; sonuç değişir mi: hayır.
- Göreli sağkalım: eksik değerli satırlar ve izlemden sonra kalan rapor zamanları bildirilmeden atılıyordu → çıktıya not; sonuç değişir mi: hayır.

## grup_ardisik

- Grup ardışık tasarım: son analizde gözlenen bilgi planlanan en büyük bilginin altında kaldığında α'nın kalanı harcanmıyor, son sınır gereğinden yüksek çıkıyordu (örnek veri, K = 3: sınır 2,3085; kullanılan toplam α 0,0219) → son analizde α'nın kalanının tamamı harcanır ve sınır gözlenen bilgiyle hesaplanır (aynı örnekte 1,9638; toplam α 0,05); sonuç değişir mi: evet, harcama fonksiyonlu sınırlarda verideki aşama sayısı K'ya eşit ve son bilgi I_maks'tan küçükse son sınır, karar, yinelenen güven aralığı ve aşama sıralı çıkarım değişir.
- Grup ardışık tasarım: gözlenen bilgi planı aştığında bütün aşamaların sınırları yeniden ölçekleniyor, önceki ara analizlerde bildirilen sınırlar son tabloda başka değerlerle görünüyordu (örnek veri, δ = 0,6: 3,9073 / 2,5799 / 1,9872) → önceki aşamaların sınırları değişmez, yalnız son sınır aşan bilgiyle hesaplanır (3,6914 / 2,4264 / 2,0145); sonuç değişir mi: evet, harcama fonksiyonlu sınırlarda son bilgi I_maks'ı aşıyorsa sınırlar, yinelenen güven aralıkları ve aşama sıralı çıkarım değişir.
- Grup ardışık tasarım: son olmayan bir aşamada bilgi planlanan en büyük bilgiye ulaşmışsa sonraki aşamalar yeniden ölçeklenmiş sınırlarla değerlendiriliyordu → çalıştırma "… aşamada bilgi planlanan en büyük bilgiye ulaşmış: α'nın tamamı o aşamada harcanır ve o aşama son analizdir" iletisiyle durur; sonuç değişir mi: evet, yalnız bu durumdaki veride (sonuç yerine ileti).
- Grup ardışık tasarım: tek satırlık veride bilgi planlanan en büyük bilgiye ulaşmışsa çalıştırma anlaşılmaz bir iletiyle duruyordu → sabit örneklem sınırı (z_{1−α}) kullanılır; sonuç değişir mi: evet, yalnız bu durumda (hata yerine sonuç).
- Grup ardışık tasarım: son analizde bilgi plandan sapmışsa çıktıya açıklayıcı bir not yazılır ("Son analiz: gözlenen bilgi … planlanan en büyük bilginin altında/üstünde …"); kestirim, standart hata ve aşama sütunları değerleriyle okunur (Veri sekmesinde "kategorik" görünse de); sonuç değişir mi: hayır.

## grup_lasso

- Grup lasso (elastik ağ, α < 1): grpreg aynı λ için yol ve tek-λ çağrılarında farklı katsayı veriyor, bağımsız çözümden 0,05'e kadar ayrılıyordu → varsayılanda sonuç aynı kalır ve çıktıya uyarı yazılır; yeni "Elastik ağ: kesin çözüm" seçeneği (Gauss ailesi ve grup lasso) seçilen λ'daki katsayıları blok koordinat inişiyle kesin çözer; sonuç değişir mi: yalnız seçenek açılınca (örnek: λ = 0,05, α = 0,5, kesişim 3,0431 → 3,0356, lwt1 1,2111 → 1,2560, ftv grubu sıfır olmaktan çıkar).
- Grup lasso: verilen λ en büyük λ'yı aşınca sessizce kırpılıyor ve çıktı "(verilen)" diyordu → kırpıldığında not yazılır; sonuç değişir mi: hayır (yalnız not).
- Grup lasso: eksik değer nedeniyle atılan satır sayısı yazılmıyordu → "Not: N satır eksik değer nedeniyle atıldı (kalan n = …)" yazılır; yanıt sütunu `mq_sayisal` ile okunur; sonuç değişir mi: hayır.

## grup_referans_araligi

- Gruplara göre referans aralığı: "sıralı gruplar" seçilince etiketler harf sırasına dizildiği için sayıyla başlayan etiketlerde ("8-39", "40-59", "60-79") komşu çift yanlış kuruluyordu → sıra Veri sekmesindeki düzey sırası, sayıyla başlayan etiketlerde alt sınıra göre sayısal sıra, bunlar yoksa harf sırasıdır; kullanılan sıra ve kaynağı sonuç metnine yazılır; sonuç değişir mi: evet (yalnız basamak sayısı farklı sayı başlangıçlı etiketlerde; ör. 8-39 / 40-59 ve 40-59 / 60-79 çiftleri artık sınanır).
- Gruplara göre referans aralığı: kesim noktalarındaki ondalık virgüllü sayı ("40,5") sessizce atılıyordu → kesimlerde noktalı virgül varsa virgül ondalıktır ("40,5; 60" üç grup kurar); noktalı virgül yoksa eski ayrım korunur ve virgüllü parça için uyarı yazılır; sayıya çevrilemeyen parça anlaşılır iletiyle durdurur; sonuç değişir mi: evet (yalnız "40,5" gibi girişlerde; eskiden nokta sessizce düşüyordu).

## guc_benzetim

- Benzetim tabanlı güç analizi: lojistik modelde kurulamayan (yakınsamayan ya da ayrışan) tekrarlar paydadan atılıyor, ayrışmış bir bölümü ise p = 1 ile "geçerli" sayılıyordu → kurulamayan tekrar reddedilmeyen sayılır, payda her n için B kalır; kural ve sayılar çıktıda yazılır; sonuç değişir mi: evet, küçük örneklemde (güç daha düşük çıkar; örnekte n = 40: 0,0685 → 0,0675), yakınsayan tasarımlarda hayır.
- Benzetim tabanlı güç analizi: "gerekli n" yalnız nokta kestirimine bakıyordu (gerçek güç 0,8015 iken "ulaşılmadı") → ayrıca kestirimi hedefin Monte Carlo hatası (1,64 SH) içinde kalan ilk n bildirilir; etki 0 iken ileti I. tür hata oranını söyler; sonuç değişir mi: hayır (yalnız bildirim).
- Benzetim tabanlı güç analizi: etki işareti ile tek yönlü test yönü çelişince uyarı yoktu → uyarı satırı; güç 0 iken Wilson alt sınırı 5,6·10⁻¹⁷ yazılıyordu → tam 0; sonuç değişir mi: hayır.

## guvenilirlik

- Güvenilirlik analizi: yüzdelik alanına Türkçe ondalıkla yazılan "2,5" sessizce 2 ve 5 olarak iki yüzdelik oluyordu → boşluksuz virgül eskisi gibi liste ayırıcısı sayılır ama not düşülür; alanda noktalı virgül varsa liste noktalı virgül ve boşlukla ayrılır, virgül ondalıktır ("2,5; 10" = %2,5 ve %10); "1, 10, 50" aynen çalışır; sonuç değişir mi: hayır (yalnız not ve yeni yazım).
- Güvenilirlik analizi: model uyumu tablosu yalnız Weibull ve lognormali içeriyor, log-lojistik seçilince seçilen modelin log-olabilirliği ve AIC'si hiçbir yerde yoktu → tablo her zaman üç satırlıdır (üçüncü satır log-lojistik: log L −113,3723, AIC 230,7445 örnek veride); sonuç değişir mi: hayır (var olan iki satır aynı; ek satır).
- Güvenilirlik analizi (yeni seçenek): sıra regresyonu konum formülüne "Tam ortanca sıra" (beta(0,5; i, n − i + 1) kantili) eklendi; varsayılan Benard aynı; yayımlı WeibullR değerleri (η = 80,947, β = 2,255) bu seçenekle yeniden üretilir; açıklama iki konumun değerlerini ayırır; sonuç değişir mi: hayır (varsayılanda).
- Güvenilirlik analizi: ömür ya da durum sütununda boş hücre olan satırlar bildirilmeden atılıyordu → "N satır eksik değer nedeniyle çıkarıldı" notu yazılır; sonuç değişir mi: hayır.
- Güvenilirlik analizi: `survreg` yakınsama uyarıları ("Ran out of iterations and did not converge" gibi) gizleniyordu → uyarı sonuçla birlikte "Yakınsama uyarısı (Weibull): …" notu olarak yazılır; sonuç değişir mi: hayır (yalnız not).
- Güvenilirlik analizi: model uyumu tablosunun başlığında bir ticari programın adı geçiyordu → "olasılık grafiği yazılımlarında olduğu gibi" denir; açıklamadaki aynı ifade de yansız yazıldı; sonuç değişir mi: hayır (yalnız metin).
- Güvenilirlik analizi: ömür ve durum sütunları ortak `mq_sayisal` ile okunur (Veri sekmesinde kategorik görünen sayısal sütun ve sayı olmayan değerler için anlaşılır ileti); sonuç değişir mi: hayır.

## gwas

- GWAS: varsayılan varyans bileşeni çözücüsü (EMMA ızgara araması) REML'in en büyüğünü bulmuyordu (σ²e'de %9'a, p değerinde %10'a varan fark; VanRaden akrabalığıyla Bonferroni ile anlamlı marker 1 iken tam REML'de 3) → varsayılan çözücü Newton–Raphson (sommer) oldu ve REML'in en büyüğünü verir; EMMA seçilebilir, çıktı hangi çözücünün kullanıldığını yazar; sommer kurulu değilse EMMA'ya düşer ve uyarır; sonuç değişir mi: evet (özellikle eşiğe yakın markerlerde ve varyans bileşenlerinde; örnek ayarda σ²e 0,2089 → 0,2084, p değerleri %0,3'e kadar).
- GWAS: yalnız 0/1 kodlu genotipte (haploit, saf hat) etki ve standart hata yarıya iniyor ve alel frekansı ön süzgeçte yanlış ölçekleniyordu → genotipler 0/2 ölçeğine çevrilerek sınanır, etki ve SH "1 kodlu sınıf ile 0 kodlu sınıf farkı" olarak verilir ve konsola yazılır; sonuç değişir mi: evet (yalnız 0/1 kodlu veride etki ve SH iki katına çıkar; p değişmez; MAF süzgeci artık doğru frekansla çalışır).
- GWAS: marker listesi boş bırakılınca cinsiyet, blok gibi 0/1/2 sütunlar marker sayılıyor, bu yüzden harita düşüyordu; kategorik türdeki marker sessizce atlanıyordu → adı `<kromozom>_<konum>_<ad>` olan sütun varsa yalnız onlar marker sayılır, atlanan sütunların adları yazılır; kategorik türde olup değerleri 0/1/2 olan sütun da marker sayılır; sonuç değişir mi: evet (marker olmayan 0/1/2 sütunu bulunan ya da kategorik markerli veride).
- GWAS: küçük alel frekansı eşiğe tam eşit olan markerler ön süzgeçten geçip taramada sessizce düşüyordu (başlıktaki sayılar toplanmıyordu) → bu markerler taranır; sonuç değişir mi: evet, yalnız frekansı eşiğe tam eşit marker bulunan veride (örnekte `maf = 0,25` ile 1341 → 1358 marker).
- GWAS: sabit bir sütun ortak değişken seçilince paketin çevrilmemiş iç hata iletisi görünüyordu → ortak değişkenlerin kesişimle doğrusal bağımlı ya da sabit olduğunu söyleyen anlaşılır bir iletiyle durur; sonuç değişir mi: hayır (eskiden hata).

## haplotip_agi

- Haplotip ağı: değişken konumunda belirsiz baz (N, ?, IUPAC) taşıyan ve birden çok haplotiple uyuşan dizi ayrı haplotip sayılıyor; haplotip sayısı ve haplotip çeşitliliği (Hd) şişiyor, ağda 0 adımlı bağlantılar çıkıyordu → böyle dizi, bilinen konumlarında uyuştuğu belirsiz bazsız haplotiplerden en çok bireyliye (eşitlikte ilk görülene) katılıyor; kural, belirsiz bazlı dizi sayısı ve katılanlar çıktıda yazılıyor, kalan 0 uzaklıklar uyarılıyor; sonuç değişir mi: evet (yalnız belirsiz bazlı dizinin birden çok haplotiple uyuştuğu veride; örnek veri değişmez).
- Haplotip ağı: "Yerleşim ve RMST için tohum" alanı rastgele RMST'yi denetlemiyordu (aynı alanla farklı ağlar) → ağlar kurulmadan önce de ayarlanıyor; rastgele RMST yalnız bu alana bağlı; kullanılan tohum çıktıda yazılıyor; sonuç değişir mi: evet (yalnız "RMST rastgele" açıkken).
- Haplotip ağı, RMST: "RMST en çok yineleme" alanı rastgele kip kapalıyken etkisizdi, rastgele kipin etiketi ve açıklaması yanlıştı, yineleme sınırına ulaşma bildirilmiyordu → etiket/açıklama düzeltildi, sınıra ulaşılırsa uyarı yazılıyor; sonuç değişir mi: hayır.
- Haplotip ağı: TCS türünde Templeton bağlantı olasılığı hesaplanıp yazılmıyordu → bağlantı tablosunda "olasilik" sütunu, %95'in altındaki ana bağlantı sayısı not olarak; ağ yine tek parça; sonuç değişir mi: hayır (yalnız ek sütun ve not).
- Haplotip ağı: MSN/RMST'de belirsiz konumların bütün çiftlerden silindiği (ortanca birleştirme ve TCS çift bazında siler) bildirilmiyordu → silinen konum sayısı ve tutarsızlık notu; sonuç değişir mi: hayır.
- Haplotip ağı: açıklamada MSN "eşit uzunluklu bütün seçenek bağlantılar" diye tanımlanıyordu, özet tablosu çizilen seçenek bağlantı sayısını vermiyordu → tanım düzeltildi, özet tablosunda "baglanti_cizilen_secenek"; dizi kipinde ortanca vektörlerin profili haplotip tablosuna yazılıyor; fark içermeyen bağlantının mutasyon etiketi boş; sonuç değişir mi: hayır (yalnız ek sütun/satırlar).
- [3.8.0] Haplotip ağı: yeni seçenek "TCS ağını %95 sınırında ayrı ağlara böl" (varsayılan kapalı = tek parça): açıkken Templeton olasılığı %95'in altındaki bağlantılar kurulmaz, ağ ayrı parçalara bölünür (örnek veride H4 ayrı; 14 ana + 54 seçenek bağlantıdan 13 + 38 kalır), özete `ayri_ag_sayisi` eklenir; sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_haplotip_agi.py`).

## hardy_weinberg

- Hardy–Weinberg: adı "0" olan alel taşıyan genotipler (ör. 0/1 kodlaması) sessizce atılıyor, test kalan alt kümede yapılıyordu; `-9/-9` ise alel sayılıyordu → ortak genotip okuma kuralı (ortak kayıt 5): '0/1' görülürse çalıştırma nedeni söyleyerek durur; `-9` eksik kodudur; eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (ortak kayıt 5'teki durumlarda).
- Hardy–Weinberg, "Otomatik" kesin test: üç ve dört alelli lokuslarda tam (ağ algoritmalı) kesin test örneklem büyüklüğüne bakılmadan seçiliyor, birkaç yüz bireyde dakikalar sürüp zaman aşımına uğruyordu → sayılacak genotip tablosu çok büyükse (tek testte yaklaşık yarım dakikayı aşacaksa) Monte Carlo kesin test kullanılır; yöntem sütununda ve bir notta belirtilir; sonuç değişir mi: evet, yalnız çok büyük tablolu üç–dört alelli lokuslarda (kesin p yerine Monte Carlo p; önce bu çalıştırmalar çok uzun sürüyor ya da bitmiyordu).
- Hardy–Weinberg: yöntem sütunu iki alelli kesin testin p türünü yazar ("kesin (Haldane–Wigginton; DOST)"); alel adları sayıysa sayısal sıralanır (genotip etiketi "98/100"); özet satırı "lokus × popülasyon = test" çarpımı yerine "… lokus, … popülasyon; … test" yazar; popülasyon adlarının baş/son boşluğu kırpılır ("Ada" ile "Ada " tek popülasyon); sonuç değişir mi: hayır (etiket ve sıra); yalnız popülasyon adlarında baş/son boşluk farkı olan veride popülasyonlar birleşir.
- Eksik genotip kodları (yeni alan): Hardy–Weinberg: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## hurst_karsilastirma

- Hurst üssü kestiricileri karşılaştırması: 128–255 gözlemli seride varsayılan dalgacık ayarı için oktav yetmediğinden bütün çalıştırma duruyordu → dalgacık satırı atlanıp uyarı yazılıyor, öteki dört kestirici çalışıyor (yalnız dalgacık seçiliyse çalıştırma iletiyle duruyor); sonuç değişir mi: evet (128–255 gözlemde çalıştırma artık sonuç veriyor; öteki durumlarda hayır).
- Hurst üssü kestiricileri karşılaştırması: GPH standart hatası açıklamada π/√(24m) diye yazılıyordu, kod fdGPH'nin √(π²/(6·S_xx)) değerini kullanıyor → açıklama koda uyduruldu; sonuç değişir mi: hayır.

## ihmal_duyarlilik

- İhmal edilmiş değişken duyarlılığı (R): q ≠ 1 ya da "karıştırıcı kestirimi azaltır" kapalıyken tablodaki t ve sınır tablosundaki duz_t, sensemakr'ın "kestirim = (1 ∓ q)·kestirim" için t'siydi (q = 0,5 iken 2,0922; kapalıyken -4,1845) ve Python eşi farklı sayı yazıyordu → iki şablonda da t = kestirim / SH, duz_t = düzeltilmiş kestirim / düzeltilmiş SH yazılır, q ≠ 1 ya da azaltma kapalıyken tanımı söyleyen bir not eklenir; sonuç değişir mi: evet (yalnız q ≠ 1 ya da azaltma kapalıyken R'nin t ve duz_t sütunları; q = 1 ve azaltma açıkken aynı).
- İhmal edilmiş değişken duyarlılığı (R ve Python): olanaksız güçteki karıştırıcı satırları (R²_YZ = 1; SH = 0, t = -∞) sınır tablosunda geçerli sonuç gibi duruyordu → bu satırlar tablodan çıkarılır, uyarı kaç satırın çıkarıldığını söyler; sonuç değişir mi: evet (yalnız çok büyük k ya da çok güçlü ölçütte; geçerli satırlar aynı).
- İhmal edilmiş değişken duyarlılığı (R ve Python): 0/1 sonuçta E-değeri sürekli sonuç yaklaşıklığıyla uyarısız hesaplanıyordu → "sonuç yalnız 0 ve 1; E-değeri yaklaşıklıktır" uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı eklendi).
- İhmal edilmiş değişken duyarlılığı (R ve Python): ondalık virgüllü sayı sütunu ("30,5") metin gelirse her değer ayrı bir düzey olarak modele giriyordu → bütün değerleri sayı biçiminde olan ve en az birinde virgül bulunan metin sütunu sayıya çevrilir ve not yazılır; sonuç değişir mi: evet (yalnız ondalık virgüllü sayı sütunu olan veride; sonuç artık sayısal sütunla aynıdır).
- İhmal edilmiş değişken duyarlılığı (Python): ortak değişken yaklaşık eş doğrusalken (ör. yaşa 10⁻⁶ gürültülü kopya) R durup Python sessizce sayı veriyordu → Python aynı ölçütü kullanır (sütunun kalan normu kendi normunun 10⁻⁷'sinden küçükse "doğrusal bağımlı"); sonuç değişir mi: evet (yalnız bu sınırdaki veride: eskiden sayı çıkan çalıştırma artık durur).
- İhmal edilmiş değişken duyarlılığı (R): tedavinin kopyası ortak değişkenler arasındaysa ileti ortak değişkenleri suçluyordu → "Tedavi, 'x' ortak değişkeninin doğrusal kopyası" iletisi; sonuç değişir mi: hayır (yalnız ileti).
- İhmal edilmiş değişken duyarlılığı (R): kontur grafiğinin eksen etiketleri arayüz diliyle yazılır ("Unadjusted" işaretinin yazısı sensemakr'ındır); konsol satırında "artık sd" yerine "artık serbestlik derecesi" (R ve Python); sensemakr paketi kurulu değilse "paket gerekli" iletisi; sonuç değişir mi: hayır.

## ihmal_duyarlilik_py

- Python ikizinin düzeltmeleri R eşiyle ortaktır; ayrıntılı kayıt `ihmal_duyarlilik.md` içindedir (madde 1–5, 7): t ve duz_t her q için sıradan t (R'de değişti, Python zaten öyleydi); olanaksız karıştırıcı satırları tablodan çıkar; 0/1 sonuçta E-değeri yaklaşıklığı uyarısı; ondalık virgüllü sayı metni sayı olarak okunur; yakın eş doğrusal ortak değişkende R ile aynı ölçüt (durur); "artık serbestlik derecesi".

## iki_degiskenli_probit

- İki değişkenli ve özyineli probit: özyineli seçenekte "Ortak olasılıklar" tablosu ve şekli modelin hücre olasılıklarını göstermiyordu (ikinci denklemin indeksi her hücrede gözlenen birinci sonuçla kuruluyordu; iyi uyan model kötü uyuyor görünüyordu) → dört hücre, ikinci denklemin indeksi hücrenin kendi birinci sonuç değeriyle kurularak hesaplanıyor; sonuç değişir mi: evet (yalnız özyineli seçenekte, "Ortak olasılıklar" tablosunun model sütunu ve şekli; katsayılar ve testler aynı).
- İki değişkenli ve özyineli probit: ρ kestirimi ±1 sınırına dayandığında ya da en büyükleme yakınsamadan durduğunda sonuç uyarısız yazılıyordu → |ρ| > 0,99 ise ya da ölçeklenmiş gradyan sıfıra inmediyse açık uyarı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- İki değişkenli ve özyineli probit: iki sonuçta hangi düzeyin 1 (olay) sayıldığı çıktıda yazmıyordu; metin etiketli sonuçta alfabetik ikinci düzey olay olduğundan işaretler beklenenin tersi çıkabiliyordu → çıktının başında "1 olarak modellenen düzey" satırı; sonuç değişir mi: hayır.
- İki değişkenli ve özyineli probit: gözlenen bilgi matrisinin sayısal türev adımı kabaydı → adım 1e-5; standart hatalar yayımlanmış değerlerle yedi anlamlı basamakta örtüşüyor; sonuç değişir mi: evet (standart hatalar, z, p, güven aralıkları ve Wald testi beşinci anlamlı basamakta; örnekte private sabiti SH 4,83789 → 4,83782).

## irt_baglama

- MTK ölçek bağlama: ortak madde satırı bir formda kısmen doluysa (örn. b hücresi boş) madde sessizce o formdan atılıyordu → atılan madde ve boş alan adıyla çıktıya yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- MTK ölçek bağlama: ortak madde tablosunda hangi maddelerin bağlamada kullanıldığı görünmüyor, seçilen alt kümede güçlükler sabitse denetim yakalamıyordu → tabloya sona `capa` sütunu eklendi; seçili alt kümede güçlükler sabitse anlaşılır iletiyle durur; sonuç değişir mi: hayır (yalnız bağlama maddeleri seçilip alt kümede güçlükler sabitken durur).
- MTK ölçek bağlama: kuadratür kapalıyken ızgara "0,05 aralıklı" yazıyordu, gerçekte 40 eşit aralıklı nokta; Haebara ve Stocking-Lord ölçütünün tek yönlü (X'in eğrileri Y ile) olduğu belirtilmiyordu → açıklamalar koda uyduruldu; sonuç değişir mi: hayır (yalnız metin).

## joinpoint_its

- Kesintili zaman serisi ve joinpoint regresyonu: kırılma sayısı tablosundaki K ≥ 1 BIC/AIC değeri, raporlanan modelin değil yeniden başlatmasız yerel en küçüğün değeriydi → tabloda artık raporlanan (bootstrap yeniden başlatmalı) modelin ölçütü yazılıyor; sonuç değişir mi: evet (tablodaki K ≥ 1 ölçütleri; örnekte K = 2 için −86,958 yerine −87,068; seçilen kırılma sayısı değişmez).
- Kesintili zaman serisi ve joinpoint regresyonu: `n_boot = 0` iken arama yerel en küçükte kalabiliyor ve bir şey söylenmiyordu → çıktıya uyarı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Kesintili zaman serisi: yarı-Poisson p değeri ve güven aralığı normal dağılımdan (z) hesaplanıyordu, `glm` özeti t kullanır → yeni "Yarı-Poisson p ve GA dağılımı" seçeneği (varsayılan z = eski sonuç; t seçilince t(n − p)); sonuç değişir mi: hayır (yalnız t seçilirse; örnekte p 0,000592 → 0,001122).
- Joinpoint regresyonu: kırılma noktası tablosundaki tahmin 6 anlamlı basamağa yuvarlıydı (12 yerine 11,999973 gibi) → bölüm tablosuyla aynı tam değer yazılıyor; sonuç değişir mi: hayır (yalnız gösterilen ondalıklar).
- Kesintili zaman serisi: eksik değerli satırlar çıkarılınca ya da zaman düzensizse ARIMA, Durbin–Watson, Ljung–Box ve Breusch–Godfrey gecikmeleri zaman aralığına değil satır sırasına göre hesaplanıyor ve bir şey söylenmiyordu → "zaman aralığı eşit değil" uyarısı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Kesintili zaman serisi: Durbin–Watson p değerinin tek yönlü olduğu (pozitif otokorelasyona karşı) tabloda yazmıyordu → tablonun ardına not eklendi; sonuç değişir mi: hayır.
- Sayısal sütun okuma: sayıya çevrilemeyen değer olan sütunda ortak yardımcıdan (`mq_sayisal`) anlaşılır ileti geliyor; sonuç değişir mi: hayır (yalnız hata iletisi).

## kademeli_did

- Kademeli DiD: grup sütununda hiç tedavi almayanlar boş (NA) bırakılınca satırlar sessizce atılıp çözümleme küçük bir örneklemle sürüyordu → şimdi anlaşılır bir iletiyle durur (0 ya da Inf ile kodlayın); sonuç değişir mi: evet (eskiden yanlış sonuç veriyordu).
- Kademeli DiD: hiç tedavi almayanlar Inf kodlanınca hata ya da "Inf" tedavi grubu görünüyordu → Inf 0 sayılır, başlıkta yalnız kestirilen gruplar; sonuç değişir mi: hayır (0 kodlamasıyla aynı).
- Kademeli DiD: başlıktaki birim sayısı did'in attığı birimleri de sayıyordu ve atma yalnız İngilizce uyarıdaydı → başlık kullanılan birim sayısını yazar, atılan satır/birim sayısı ayrı satırdır; ilk dönemde ya da son dönemden sonra tedavi edilen gruplar uyarılır; sonuç değişir mi: hayır (yalnız bildirim).
- Kademeli DiD: çarpan bootstrap'ı tohumu 1'e sabitlemişti → kod panelindeki Tohum alanı kullanılır; sonuç değişir mi: evet, yalnız bootstrap açıkken ve tohum 1'den farklıysa (SH ve GA'lar bootstrap hata payı kadar).
- Kademeli DiD: bootstrap açıkken olay çalışması tablosu ve şekli noktasal z (1,96) ile çiziliyordu → eşzamanlı güven bandı (aggte crit.val.egt, örnekte 2,85) kullanılır, tablo başlığı belirtir; sonuç değişir mi: evet, yalnız bootstrap açıkken olay çalışması aralıkları genişler.
- Kademeli DiD: birim içinde zamanla değişen ortak değişken sessizce kabul ediliyordu → uyarı yazılır ve etiket açıklaması did'in taban dönem değerini kullandığını söyler; sonuç değişir mi: hayır.

## kalibrasyon_raking

- Ağırlık kalibrasyonu: doğrusal kalibrasyonda sıfır ya da negatif ayarlı ağırlık sessizce kabul ediliyordu → açık uyarı (kaç birim, en küçük ağırlık, en küçük g) ve çözüm önerisi yazılır; sonuç değişir mi: hayır (yalnız uyarı eklenir).
- Ağırlık kalibrasyonu: raking'de marjinal toplamlar birbiriyle tutarsızsa sessizce yanlış sonuç veriyordu → çalıştırma "marjinal toplamlar tutarsız" iletisiyle durur; `N` ile düzey toplamları farklıysa not yazılır; sonuç değişir mi: evet (tutarsız toplam girilmiş raking çalıştırmaları artık sonuç vermez; tutarlı girişte hayır).
- Ağırlık kalibrasyonu: eksik değerli satırlar sessizce çıkarılıyordu → çıkarılan satır sayısı ve sütunlara göre dağılımı not olarak yazılır; sonuç değişir mi: hayır.
- Ağırlık kalibrasyonu: ayarlı birim ağırlıkları tablo olarak verilmiyordu → "Ayarlı birim ağırlıkları" tablosu (satır, w0, w, g) eklendi (5000 satırdan uzunsa kesildiği not edilir); ilk satırda yöntem adı kod değeri yerine arayüz dilinde yazılır; uygulanamaz g sınırlarında hata iletisi olası nedeni söyler ve `zorla` açıklaması düzeltildi; sonuç değişir mi: hayır (ek tablo ve ileti metinleri).

## kalitim_derecesi

- Kalıtım derecesi (hayvan modeli): büyük yuvarlak sayısal kimlikler (100000 …) birey ve ebeveyn sütunlarında farklı metne dönüşüyor, ebeveynler bulunamıyor, soy kütüğü dağılıyor ve h² yanlış çıkıyordu → kimlikler sütun türünden bağımsız aynı metne çevrilir (kategorik türdeki "570"/"570.0" dahil); eklenen kurucu sayısı kayıt sayısının yarısına varırsa kimlik yazımı için uyarı yazılır; sonuç değişir mi: evet (yuvarlak büyük sayılı kimliklerde; ör. Mrode verisi ×100000: 14 hayvan/9 kurucu yerine 8/3, cinsiyet 3,404 / 4,359).
- Kalıtım derecesi (hayvan modeli): artık varyans sıfıra giderse (h² → 1) uyarı yazılmıyor, h² için SH = 0,886 gibi anlamsız bir değer veriliyordu → uyarı yazılır, h² ve varyans bileşenlerinin SH'si boş bırakılır; sonuç değişir mi: evet (yalnız bu sınır durumunda; SH'ler boş).
- Kalıtım derecesi (hayvan modeli): aynı hayvanın birden çok kaydı varsa uyarı yoktu → kalıcı çevre etkisi olmadığı ve σ²A'nın yukarı yanlı olabileceği uyarısı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Kalıtım derecesi (yarım kardeş modeli): lme4 varsayılan toleransla çağrılıyordu; dengeli veride bileşenler kapalı biçimli çözümden beşinci basamakta sapıyordu → sıkı toleransla çağrılır (öteki genetik şablonlardaki gibi); sonuç değişir mi: evet, beşinci–altıncı basamakta (ör. σ²baba 0,120363 → 0,120361; h²(baba) 0,347079 → 0,347072; SH'ler 5. basamakta).
- [3.8.0] Kalıtım derecesi (hayvan modeli): yeni seçenek "Kalıcı çevre etkisi" (varsayılan kapalı): açıkken hayvan başına kalıcı çevre varyansı eklenir (üç bileşenli REML), h² ile tekrarlanma derecesi ayrı verilir; yinelenen kayıt yoksa ya da bilinen h² verilmişse durur; sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_kalitim_derecesi.py`).

## kantil_ileri

- Sansürlü kantil regresyon: Portnoy sürecinin durduğu düzeyin üstündeki kantillerde (örnekte τ > 0,9711) boş yerine son çözümün katsayıları yazılıyordu → bu kantillerde katsayı, SH, GA ve p boş (NA) bırakılır, veri özetine "sürecin durduğu τ" satırı ve bir not eklenir; sonuç değişir mi: evet, yalnız tanımlanamayan üst kantiller istendiğinde.
- Sansürlü kantil regresyon: Yeniden örnekleme özeti hata verince SH, GA ve p nedeni yazılmadan boş kalıyordu → hata iletisi uyarılara ve tablo başlığına yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Sansürlü kantil regresyon: "Kantil süreci ızgara adımı" yeniden örneklemeye aktarılmıyor, SH bu ayardan bağımsız kalıyordu → ızgara alt örneklerin kestirimine de verilir; sonuç değişir mi: evet, bütün SH, GA ve p değerleri (örnekte `sex`, τ = 0,5 SH 0,104 → 0,116; kestirimler değişmez).

## karisim_deneyi

- Karışım deneyi: konsol notunda, tablo satır adlarında ('NOINT'), 'güven' alanı açıklamasında ve şablon açıklamasında ticari yazılım adları geçiyordu → nötr ifade ('kesişimsiz', 'yaygın yazılımlar'); açıklamadaki aşındırma verisi R² değeri 'R² (düzeltilmiş toplam) 0,9836603; düzeltilmiş R² 0,9696549' diye açıklandı; sonuç değişir mi: hayır (yalnız metin).

## karma_etkili_cox

- Karma etkili Cox (rastgele etki sınaması): iç içe modelde (iki varyans bileşeni) p değeri ½·P(χ²₂ > LR) ile hesaplanıyordu → doğru sınır karışımı ¼χ²₀ + ½χ²₁ + ¼χ²₂ kullanılır (k bileşen için 2^-k·C(k,j) ağırlıklı karışım); satır adı bütün varyansların birlikte sıfırla karşılaştırıldığını söyler; sonuç değişir mi: evet (yalnız iç içe modelde ve p sınırdayken; LR = 5,99 için 0,0250 yerine 0,0197). Tek düzeyli modelde p aynı.
- Karma etkili Cox (seyrek yaklaşım, yeni alan): 50 ve üzeri rastgele etki sütununda coxme sessizce seyrek yaklaşıma geçiyordu → çıktıya not yazılır; forma "Bilgi matrisini yoğun çöz" seçeneği eklendi (varsayılan kapalı = bugünkü sonuç); açıkken yoğun çözüm kullanılır (iç içe örnekte entegre log-olabilirlik −10518,5611 yerine −10518,5498); sonuç değişir mi: hayır (alan kapalıyken).
- Karma etkili Cox (Laplace düzeltmesi): "benzetim sayısı" (inceltme) hiçbir çıktıyı değiştirmiyor ve sonucu yazmıyordu → kestirilen düzeltme ve benzetim standart hatası olabilirlik tablosunun son iki satırında yazılır; sonuç değişir mi: hayır (kestirimler aynı; yalnız iki satır eklenir).
- Karma etkili Cox (veri hazırlığı): eksik değerli satırlar ve sabit etkilere konan süre/olay/grup sütunu bildirilmeden çıkarılıyordu → çıkarılan satır sayısı (kalan n ile) ve çıkarılan sütun adı yazılır; sonuç değişir mi: hayır.
- Karma etkili Cox (yakınsama uyarısı): coxme uyarıları tablolardan sonra tek satırda görünüyordu → model özetinden önce, çıktının başında yazılır; sonuç değişir mi: hayır.

## karma_guc_benzetimi

- Karma model güç benzetimi: analitik güç sütunundaki Kenward-Roger serbestlik derecesi pilot ve varsayım değerlerinin karışımından hesaplanıyordu (dengesiz veri + küme düzeyi etkide sd 676, analitik güç 0,516) → serbestlik derecesi verilen varyans bileşenleriyle doğrudan hesaplanır (sd 6,0; analitik güç 0,393; 1000 benzetim 0,358); sonuç değişir mi: evet, yalnız analitik sütunlar ve yalnız dengesiz tasarım ya da küme düzeyinde etkide (dengeli tasarım + küme içi etkide değişmez; benzetim gücü hiçbir durumda değişmez).
- Karma model güç benzetimi: güç eğrisinde benzetim düzey sırasıyla, analitik sütun ilk görülme sırasıyla küme seçiyordu (aynı satırda n = 26 ↔ 20) → ikisi de düzey sırasıyla ilk k kümeyi kullanır; sonuç değişir mi: evet, küme kodları dosyada düzey sırasında değil ve tasarım dengesizse analitik sütunlarda.
- Karma model güç benzetimi: pilot model ML ile kurulunca analitik sd değişiyordu (26 yerine 24,56) → analitik sütun REML/ML seçiminden bağımsız (26); sonuç değişir mi: evet, yalnız "REML kapalı" seçildiğinde analitik sütunda.

## karma_kantil

- Doğrusal kantil karma model: rastgele eğim eklenince kestirim yerel bir noktada kalabiliyor (örnek veride log-olabilirlik −68,19'dan −81,89'a düşüyor), uyarı verilmiyordu → rastgele eğim seçildiğinde aynı veri yalnız rastgele kesişimle de uydurulur; eğimli modelin log-olabilirliği daha küçükse iki değeri yazan ve sonucun güvenilir olmadığını söyleyen uyarı çıkar; sonuç değişir mi: hayır (sayılar aynı; uyarı eklendi; rastgele eğimli çalıştırma bir ek uydurma kadar uzar).
- Doğrusal kantil karma model: sonuç (rastgele etki varyansı, sınıf içi korelasyon, log-olabilirlik) kareleme düğüm sayısına duyarlı; form bunu söylemiyordu → "Kareleme düğüm sayısı" alanına ve şablon açıklamasına not eklendi (9, 11, 15 düğümle yineleyin); sonuç değişir mi: hayır.
- Doğrusal kantil karma model: yanıt ya da rastgele eğim sütunu "kategorik" türdeyse çalıştırma R'nin ham hatasıyla ("Calling var(x) on a factor …") duruyordu → sütunlar değerleriyle okunur; sayı olmayan değer varsa sütunu bildiren iletiyle durur; sonuç değişir mi: hayır (eskiden hata veren çalıştırma artık sonuç verir).
- Doğrusal kantil karma model: rastgele eğim değişkeni sabit etkilerde yoksa model eğimin ortalamasını sıfır varsayıyor, uyarı verilmiyordu → uyarı yazılır; sonuç değişir mi: hayır.

## kesikli_secim

- Kesikli seçim deneyleri, iç içe lojit: "bütün yuvalar için tek log-toplam katsayısı" kapalıyken en büyükleme en büyük noktaya varmadan duruyor ve durduğu nokta kestirim olarak yazılıyordu (örnek veride log-olabilirlik −178,0368; doğrusu −177,8098; IV:other 0,378 yerine 0,446) → en büyükleme tek log-toplam katsayılı çözümden başlatılıyor; her modelde durma kodu denetleniyor, adım bulunamadığı için duran en büyükleme bir kez yeniden başlatılıyor, yine yakınsamadıysa çıktının başına "en büyükleme YAKINSAMADI" uyarısı yazılıyor; sonuç değişir mi: evet (yuvaya özgü log-toplam katsayılı iç içe lojitte katsayılar, standart hatalar, log-olabilirlik ve testler; öteki modellerde yalnız yakınsamayan çalıştırmalarda uyarı eklenir).
- Kesikli seçim deneyleri, gizil sınıf lojit: sınıf üyelik parametrelerinin (üyelik sabiti ve üyelik değişkenleri) standart hatası panel veride yaklaşık √(birey başına seçim sayısı) kat küçük çıkıyordu (örnekte 0,041; doğrusu 0,141); z ve p değerleri aynı ölçüde şişiyordu → kovaryans matrisini artık şablon, birey düzeyindeki log-olabilirliğin gözlenen bilgi matrisinden kendisi hesaplıyor; sonuç değişir mi: evet (gizil sınıf lojitte üyelik parametrelerinin standart hatası, z, p ve güven aralığı; sınıf içi katsayıların standart hataları ve ödeme isteği aralıkları 1e-5 göreli düzeyinde).
- Kesikli seçim deneyleri, karma lojit, log-normal dağılımlı katsayı: ödeme isteği katsayının kendisinden değil log ölçeğindeki konum parametresinden hesaplanıyordu (örnekte wk için −0,155; doğrusu 1,675) → ödeme isteği katsayının ortalamasından, exp(konum + ölçek²/2), hesaplanıyor; delta yöntemi ve Krinsky–Robb aralıkları bu dönüşümü içeriyor; tablonun altına not yazılıyor; sonuç değişir mi: evet (yalnız log-normal dağılım seçildiğinde, rastgele katsayılı niteliklerin ödeme isteği ve aralıkları).
- Kesikli seçim deneyleri, karma ve iç içe lojit: standart hatalar gradyan dış çarpımından geliyordu, panel karma lojitte dış çarpım bireyler üzerinden değil seçim durumları üzerinden alınıyordu ve hangi tanımın kullanıldığı yazmıyordu → varsayılan standart hata gözlenen bilgi matrisinden (Hessian) geliyor; yeni "Kovaryans matrisi (karma ve iç içe lojit)" alanıyla gradyan dış çarpımı seçilebiliyor (panel karma lojitte gradyanlar birey içinde toplanır); her modelde katsayı tablosunun altına standart hatanın türü yazılıyor; sonuç değişir mi: evet (karma ve iç içe lojitte bütün standart hatalar, z, p, güven aralıkları, IV = 1 testi ve ödeme isteği aralıkları; örnek ayarda pf 0,0328 → 0,0323, cl 0,0137 → 0,0212; iç içe lojitte IV 0,1797 → 0,1666; katsayılar ve log-olabilirlik aynı; dış çarpım seçilirse iç içe lojitte eski değerler, panel karma lojitte birey düzeyindeki değerler (pf 0,0233) çıkar; karma lojitte çalışma süresi uzar).
- Kesikli seçim deneyleri: nitelik sütunlarının adı x1, x2, … biçimindeyse katsayı tablosunda ve şekillerde terimler yanlış etiketleniyordu (üç satır da aynı adı alabiliyordu) → adlar tek geçişte çevriliyor; sonuç değişir mi: hayır (yalnız etiketler).
- Kesikli seçim deneyleri: nitelik ve sınıf üyelik sütunları Veri sekmesinde "kategorik" türdeyse değerleri yerine düzey sıra numaraları kullanılabiliyordu → sütunların değerleri okunuyor; sayıya çevrilemeyen değer varsa çalıştırma sütun adını ve örnek değerleri söyleyerek duruyor (eskiden o seçim durumları sessizce atılıyordu); sonuç değişir mi: evet (yalnız kategorik türe alınmış ya da metin değer içeren nitelik sütununda).
- Kesikli seçim deneyleri: seçim durumu kimliği her bireyde yeniden başlıyorsa (görev numarası 1, 2, …) bütün seçim durumları atılıyor ve çalıştırma "seçim durumu sayısı (0) çok az" iletisiyle duruyordu → birey sütunu seçiliyken seçim durumları (birey, durum) çiftiyle ayrılıyor ve bir notla bildiriliyor; sonuç değişir mi: evet (eskiden sonuç yoktu).
- Kesikli seçim deneyleri: Newton–Raphson iç içe lojitte anlaşılmaz bir hatayla düşüyordu → açık iletiyle duruyor; "Optimizasyon yöntemi" açıklaması düzeltildi (iç içe lojitte paket varsayılanı BFGS'dir); sonuç değişir mi: hayır.
- Kesikli seçim deneyleri: form ve çıktı metinlerinden ticari yazılım adı çıkarıldı; şablon açıklamasına standart hataların türü ve log-normal katsayıda ödeme isteğinin tanımı eklendi; sonuç değişir mi: hayır.

## kesikli_zaman_sagkalim

- Kesikli zaman sağkalım: Kişi-dönem verisinde bir kişinin ara dönem satırı eksikse model sessizce uydurulur, yaşam tablosunda negatif sansür çıkardı → çalıştırma "dönemleri kesintisiz değil" iletisiyle durur; sonuç değişir mi: evet, yalnız dönem boşluğu olan veride (eskiden hatalı sonuç veriyordu).
- Kesikli zaman sağkalım: Tam ya da yarı tam ayrışmada uyarı verilmiyordu → SH > 100 ya da taban dışı |β| > 10 olan terimler "olası ayrışma" uyarısıyla adlarıyla işaretlenir; sonuç değişir mi: hayır (yalnız uyarı).
- Kesikli zaman sağkalım: "İlk dönem" 0 iken negatif süre sahte dönem satırları üretiyordu → negatif süre iletisiyle durur; sonuç değişir mi: evet, yalnız negatif süreli veride (eskiden anlamsız sonuç).
- Kesikli zaman sağkalım: Profil ortalamasının kişi-dönem satırı üzerinden alındığı ve ilk dönemden başlatmanın çıkarımı koşullandırdığı söylenmiyordu → çıktıya notlar eklendi; sonuç değişir mi: hayır.

## kesin_lojistik

- Kesin koşullu lojistik regresyon: olay sayısı ya da deneme sayısı sütunu Veri sekmesinde "kategorik" türdeyse değerleri yerine düzey sıra numaraları kullanılıyordu (örnekte 29 yerine 28 olay; kesin p 0,040 yerine 0,235) → sütunların değerleri okunuyor; ortak değişkenler de aynı yoldan sayıya çevriliyor, sayıya çevrilemeyen değer varsa çalıştırma sütun adını söyleyerek duruyor; sonuç değişir mi: evet (yalnız sayım sütunu kategorik türe alınmışsa).
- Kesin koşullu lojistik regresyon: formun kabul ettiği güven düzeyi 0,5 çalıştırmayı hata iletisiyle durduruyordu → 0,5 kabul ediliyor; sonuç değişir mi: hayır (eskiden sonuç yoktu).
- Kesin koşullu lojistik regresyon: şablon açıklamasından ve iki parametre ipucundan ticari yazılım adı çıkarıldı; sonuç değişir mi: hayır.

## kesirli_faktoriyel

- Kesirli faktöriyel: Plackett-Burman'da faktör sayısı deneme sayısını aşınca verilen hata iletisinde sayılar yer değiştirmişti ('11 denemede en çok 12 faktör') → doğru sıra ('12 denemede en çok 11 faktör'); sonuç değişir mi: hayır (yalnız ileti).

## kirilmali_birim_kok

- Kırılmalı birim kök (ZA, LS, TY): ZA ve LS birlikte çalışırken ZA t dizisi grafiğinin dikey aralığı LS kritik değerlerine göre kuruluyordu, ZA %5 çizgisi görünmeyebiliyordu → ZA kritik değerleriyle kuruluyor, çizgi görünüyor; sonuç değişir mi: hayır (yalnız grafik).
- Kırılmalı birim kök: Toda–Yamamoto listesinde yalnız yanıt varsa bölüm sessizce atlanıyordu → uyarı yazılıyor ve `uyari` değişkeni artık kullanılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Kırılmalı birim kök: Toda–Yamamoto gecikme ölçütü hesaplanamazsa p = 1 sessizce kullanılıp başlıkta ölçütle seçilmiş gibi yazılıyordu → başlık "1 (ölçüt hesaplanamadı)" yazıyor ve uyarı ekleniyor; sonuç değişir mi: hayır (yalnız bu nadir durumda yazım).
- Kırılmalı birim kök: Lee-Strazicich çöküş modelinde "kritik değerler T = 100 içindir" notu yoktu → iki modelde de yazılıyor; sonuç değişir mi: hayır.
- Sayısal sütun okuma: sayıya çevrilemeyen değer olan sütunda ortak yardımcıdan (`mq_sayisal`) anlaşılır ileti geliyor; sonuç değişir mi: hayır (yalnız hata iletisi).

## kisitli_ordinasyon

- CCA'da düzeltilmiş R² vegan'ın permütasyonla kestirimiyle yazılıyordu ama çıktı bunu söylemiyordu (sonuç tohuma göre 0,2049–0,2067 arasında oynuyordu) → varsayılan davranış aynı kalır; satır artık "(CCA: 1000 permütasyonla kestirildi; sonuç tohuma bağlıdır, tohum: N)" yazar ve yeni "CCA'da düzeltilmiş R² tanımı" alanı kapalı formülü (Ezekiel; tohumdan bağımsız) seçmeye izin verir; Python ikizi artık aynı tanımı kullanır; sonuç değişir mi: hayır (varsayılan; aynı tohumda aynı değer), Ezekiel seçilirse evet (vare CCA 0,2056).
- Varyans bölümleme tablosunun başlığı "[b] ortak, [c] yalnız küme 2" diyordu ama satırlar tersiydi → başlık satırlara uydu ("[a] yalnız küme 1, [b] yalnız küme 2, [c] ortak, [d] artık"); Python ikizi aynı satır adı ve sırayı yazar; sonuç değişir mi: hayır (sayılar aynı; yalnız başlık ve Python'da satır sırası).
- Kısmi ordinasyonla birlikte ikinci küme seçilince varyans bölümleme koşul değişkenini hesaba katmıyordu → bölümleme koşul çıkarıldıktan sonra yapılır, tabloya koşulun payı ([Z]) eklenir ve not yazılır; sonuç değişir mi: evet (yalnız koşul değişkeni ile ikinci küme birlikte seçildiğinde; örnek veride [a] 0,1930 → 0,0750); koşulsuz çalıştırmada hayır.
- Koşul değişkeni ya da ikinci küme açıklayıcıyla ortak bir sütun taşıyınca sütun bildirilmeden çıkarılıyor, model kısmi sanılırken koşulsuz uydurulabiliyordu → çalıştırma açık iletiyle durur ("'Al' hem açıklayıcı değişken hem koşul değişkeni; birinden çıkarın"); sonuç değişir mi: evet (bu durumda önceden sessizce koşulsuz sonuç veriyordu, şimdi hata).
- Hellinger/Wisconsin dönüşümlü RDA'da boş satır "NA/NaN/Inf in foreign function call" gibi anlaşılmaz bir iletiyle düşüyordu → "her örnek alanında en az bir tür bulunmalı (boş satır var)" iletisiyle durur; sonuç değişir mi: hayır (yalnız ileti).
- Eksik değer yüzünden atılan örnek alanları ve toplamı sıfır olan türler bildirilmeden çıkarılıyordu → atılan satır numaraları ve çıkarılan türler not olarak yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Etken değişkenlerin düzey sırası ve başvuru (ilk) düzeyi çıktıya yazılmıyordu → her etken için "düzey sırası … ilk düzey başvurudur" notu yazılır; sonuç değişir mi: hayır.

## kisitli_ordinasyon_py

- CCA'da düzeltilmiş R² Python ikizinde kapalı formülle (Ezekiel), R ikizinde vegan'ın permütasyonlu kestirimiyle hesaplanıyordu; aynı veride farklı sayı çıkıyordu (vare 0,2056 / 0,2051; dune 0,2003 / 0,2063) → iki şablon aynı tanımı kullanır: varsayılan R'nin bugünkü tanımıdır (permütasyonla kestirim, 1000 permütasyon, tohum çıktıya yazılır; iki ikiz Monte Carlo hatası içinde aynıdır); yeni "CCA'da düzeltilmiş R² tanımı" alanı kapalı formülü seçmeye izin verir (iki ikizde birebir aynı); sonuç değişir mi: evet (Python'da CCA için varsayılan olarak, küçük fark: vare 0,2056 → 0,2046, dune 0,2003 → 0,2085; kapalı formül seçilirse eski sayı).
- Koşul değişkenli modelde düzeltilmiş R² "—" yazılıyordu → kısmi modelde de yazılır (RDA'da vegan'ın kısmi tanımı: Ezekiel(R²+R²koşul) − Ezekiel(R²koşul); CCA'da R ikiziyle aynı iki seçenek); sonuç değişir mi: evet (yalnız koşul değişkeni seçilince; "—" yerine sayı).
- Python şablonunun yalnız genel F testi yaptığı, R şablonundaki terim ve eksen testlerinin, permütasyon modeli seçeneğinin, triplot ölçeklemesinin ve alan etiketinin bulunmadığı yazılı değildi → açıklama metni (TR/EN/ES) eksikleri açıkça yazar; sonuç değişir mi: hayır (yalnız açıklama).
- Varyans bölümleme satırları R ikizinden farklı sırada ve adla çıkıyordu ("[b] ortak, [c] yalnız küme 2") → satır adları ve sırası R ikiziyle ve vegan'la aynıdır ("[a] = X1|X2", "[b] = X2|X1", "[c]", "[d] = Residuals"); `sd` sütunu vegan'la aynı; ikinci küme ve koşul birlikte seçilince koşul çıkarılarak hesaplanır ([Z] satırı); sonuç değişir mi: evet (2. ve 3. satır yer değiştirir; koşul varken sayılar değişir).
- Hellinger dönüşümlü RDA'da boş satır sessizce analize giriyordu (R ikizi duruyordu) → iki ikiz de açık iletiyle durur; sonuç değişir mi: evet (boş satırlı veride önceden sonuç veriyordu, şimdi hata).
- Etken düzeyleri alfabetik sıralanıyor, R ikizi ise veri sırasını kullanıyordu (biplot satır adları ve başvuru farklıydı) → iki ikiz aynı kuralı kullanır (kategorik türde kendi sırası; hepsi sayıysa sayısal; değilse verideki ilk görülme) ve düzey sırası notu yazılır; sonuç değişir mi: biplot satır adları ve işaretleri evet (başvuru düzeyi değişir), eylemsizlik ve F hayır.
- Eksik değerli satırlar ve koşulla ortak sütunlar bildirilmeden atılıyordu → atılan satırlar ve toplamı sıfır türler yazılır; koşul ya da ikinci küme açıklayıcıyla ortaksa açık iletiyle durur; sonuç değişir mi: hayır (bildirim); ortak sütunda evet (hata).

## klinik_tahmin

- Klinik tahmin modeli: kategorik sütunun başvuru düzeyi Veri sekmesindeki düzey sırasını yok sayıp alfabetik seçiliyordu → Veri sekmesinde kategorik sütunda ilk düzey başvurudur, metin sütununda alfabetik ilk düzey; başvuru düzeyi çıktıya yazılır; sonuç değişir mi: evet (yalnız kategorik sütunun ilk düzeyi alfabetik ilk düzeyden farklıysa; OR'lar başvuruya göre değişir).
- Klinik tahmin modeli: etiketleri sayı olan kategorik/metin sütunu (evre 1-4) sessizce tek doğrusal terim oluyordu → çıktıya not; yeni "Sayı değerli kategorik/metin sütunları" seçeneği (varsayılan "doğrusal" = bugünkü; "kategorik" düzeylere ayırır); sonuç değişir mi: hayır (varsayılanda).
- Klinik tahmin modeli: tam ayrışmada anlamsız tablolar (OR 10^18, C = 1) yazılıp çalıştırma tamamlanıyordu → olasılıkların %5'inden fazlası 0/1'e yakınsa ve standart hatalar 1000'i aşıyorsa çalıştırma anlaşılır iletiyle durur; sonuç değişir mi: evet (yalnız ayrışan veride; sonuç yerine ileti).
- Klinik tahmin modeli: rastgele sayı tohumu alanı çalıştırma tohumunu yok sayıyordu → alan 0 ise çalıştırma tohumu kullanılır (varsayılan 1 aynı sonucu verir); kullanılan tohum çıktıya yazılır; sonuç değişir mi: hayır (varsayılanda).
- Klinik tahmin modeli: konsolda "model sd" kesirli serbestlik derecesini kesiyordu (3,5 → 3) → 1 ondalıkla yazılır; .632 yöntemi ve eğim yorumu metinleri düzeltildi, yeniden örneklemeden gelen uyarılar ana model uyarılarından ayrı etiketlenir; sonuç değişir mi: hayır.

## klon_lokasyon_genetik

- Klon/lokasyon genetiği (yaş–yaş kolu): `Yaşlar` alanı boşken sütun adının sonundaki sayı sessizce yaş sayılıyordu (`oz1`…`oz4`, `d13C`, `cap_2019` gibi adlarda Lambeth ve erken seleksiyon tabloları anlamsız yaşlarla yazılıyordu) → hangi yaşların okunduğu konsola yazılır ve sayı yaş değilse gerçek yaşları alana yazmak gerektiği söylenir; sonuç değişir mi: hayır (yalnız not eklendi).
- Klon/lokasyon genetiği: lme4'ün yakınsama ve ölçek uyarıları ile en iyileyicinin işlev değerlendirme sınırına takılması kullanıcıya hiç ulaşmıyordu → tekil (sınır) uyarıları dışındaki lme4 uyarıları, en iyileyicinin sıfırdan farklı çıkış kodu ve 10⁵ değerlendirme sınırına ulaşması çalıştırmanın sonunda tek uyarı olarak yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Klon/lokasyon genetiği: genotip birimi klon iken uyarı ve notlar "aile × lokasyon", "aile varyansı" diyordu → birimin adı (klon/aile) yazılır; ayrıca lokasyon varyansı uyarısındaki bozuk cümle ("rB olduğundan düşük kestirilebilir") düzeltildi; sonuç değişir mi: hayır (yalnız metin).
- Klon/lokasyon genetiği: blok verilmediğinde konsol "parsel bs" olarak lokasyon sayısını yazıyordu → "–" yazılır; varyans bileşenleri tablosunun başlığı `p` sütununun tek yönlü Wald testi olduğunu (sınıra yakın bileşende tutucu) söyler; sonuç değişir mi: hayır (yalnız metin).
- [3.8.0] Klon/lokasyon genetiği: yeni seçenek "Yaşlar sütun adından okunmasın" (varsayılan kapalı): açıkken `Yaşlar` boşsa sütun adındaki sayılar yaş sayılmaz, çözümleme özellik–özellik sürer (Lambeth ve erken seleksiyon tabloları yazılmaz); sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_klon_lokasyon_genetik.py`).

## kontrol_kartlari

- Değişken kontrol kartları: "Kural 4" etiketi, ardışık nokta sayısı değiştirilse de hep "8 nokta" yazıyordu → etiket artık seçilen sayıyı yazıyor (ör. "art arda 9 nokta"); sonuç değişir mi: hayır (yalnız etiket; kural zaten seçilen uzunlukla çalışıyordu).
- Değişken kontrol kartları (I-MR): ölçüm sütunundaki boş hücreli satırlar haber verilmeden atılıyor, boşluğun iki yanındaki gözlemlerin farkı da hareketli aralık sayılıyordu → atılan satır numaraları not olarak yazılıyor, boşluğu aşan fark hareketli aralık sayılmıyor ve metinde "alt grup" yerine "gözlem" deniyor; sonuç değişir mi: evet (yalnız I-MR'de ölçüm sütununda boş hücre varsa; örnek: 60 gözlemin 4'ü boşken σ̂ 0,00957 → 0,00937; boş hücre yoksa hayır).
- Değişken kontrol kartları: alt grup sırası sütun türüne göre değişiyordu (sayısal kimlik artan, metin/kategorik kimlik dosya sırası) ve bu yalnız kodda yazılıydı → yeni "Alt grup sırası" seçeneği (varsayılan "sütun türüne göre" = eski davranış; "dosya sırası" her tür için dosyadaki ilk görünüş sırası), kullanılan sıra "Süreç değişkenliği" tablosunda ve form açıklamasında yazılıyor; sonuç değişir mi: hayır (varsayılanda aynı; Western Electric kural 2-4 sonuçları yalnız "dosya sırası" seçilir ve kimlikler dosyada artan sırada değilse değişir).
- Değişken kontrol kartları: ölçüm Veri sekmesinde "kategorik" görünse bile artık sayı olarak okunuyor, sayıya çevrilemeyen değer anlaşılır iletiyle durduruyor; sonuç değişir mi: hayır (yalnız yanlış türdeki sütunda).

## kopula

- Kopula modelleri: Clayton, Gumbel ve Joe için yalnız ortalama Kendall τ'ya bakılıyordu; bir çiftin τ'su negatif olsa da sınır değerde (θ = 1) anlamsız bir sonuç basılıyordu → her çiftin τ'su pozitif olmalı, değilse çift adıyla anlaşılır iletiyle durur (TR/EN/ES); aile karşılaştırmasında bu aileler atlanır ve not düşülür; sonuç değişir mi: evet (karışık işaretli çiftlerde eskiden anlamsız sonuç çıkıyordu, şimdi hata).
- Kopula modelleri: varsayılan ayarda (t, serbest ν, uyum testi açık) uyum testi hiç çalışmıyor ve ρ'ların standart hataları ν sabitmiş gibi hesaplanıyordu, bunlar yalnız İngilizce paket uyarısında geçiyordu → "uyum iyiliği testi YAPILMADI" uyarısı ve "standart hatalar ν sabit varsayılarak hesaplanır" notu Türkçe/İngilizce/İspanyolca yazılır; sonuç değişir mi: hayır (yalnız çıktı notu).
- Kopula modelleri: Likert benzeri ayrık (çok bağlı) veride kestirim yapılıp geçersiz standart hata ve z yazılıyordu → farklı değer sayısı gözlem sayısının yarısından azsa çalışma anlaşılır iletiyle durur; daha hafif bağlarda uyarı metni kestirim ve standart hataların etkilendiğini söyler; sonuç değişir mi: evet (çok bağlı veride eskiden geçersiz sonuç çıkıyordu, şimdi hata).
- Kopula modelleri: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve değişken başına eksik sayısı yazılır; sonuç değişir mi: hayır.
- Kopula modelleri: eniyilemenin yakınsayıp yakınsamadığı okunmuyordu → yakınsama olmadıysa uyarı yazılır; aile karşılaştırma tablosunun sonuna `yakinsama_kodu` sütunu eklendi ve tablo başlığı karşılaştırmanın seçilen yöntemden bağımsız MPL olduğunu söyler; sonuç değişir mi: hayır (mevcut sütunlar ve değerler aynı).

## kosullu_benzetim

- Jeoistatistiksel benzetim: koşulsuz ardışık benzetimde "bilinmeyen ortalama" etkisizdi → her gerçekleşmede ortalama GLS dağılımından çekilir (kriging sütunları GLS ortalaması ve toplam eşik + GLS varyansı; örnek veri: varyans 0,6432 → 0,6819); sonuç değişir mi: evet, yalnız koşulsuz + ardışık + bilinmeyen ortalama seçiminde.
- Jeoistatistiksel benzetim: gözlem sayısı, dönüşüm ve atılan yinelenen koordinatlar çıktıda yoktu → "Veri: n = …; değişken log(…); ortalama …" satırı; sonuç değişir mi: hayır.

## kucuk_alan

- Küçük alan kestirimi: A kestirimi yakınsamayınca "incorrect number of dimensions" ham hatasıyla çöküyordu → "A kestirimi N yinelemede yakınsamadı; … durduruldu" iletisiyle durur ve yineleme/duyarlık ayarını önerir; sonuç değişir mi: hayır (önceden de sonuç çıkmıyordu).
- Küçük alan kestirimi: gerçek A sıfıra yakınsa ML ve FH çöküyordu → A = 0 sınırı bağımsız sınanır; sınırdaysa A = 0'daki REML uydurması (katsayılar, SH, EBLUP, log-olabilirlik aynı; MSE Prasad-Rao) kullanılır ve not yazılır; sınır değilse açık iletiyle durur; sonuç değişir mi: hayır (önceden çöküyordu; yalnız çöken durumlarda sonuç üretir).
- Küçük alan kestirimi: eksik değerli alanlar sessizce çıkarılıyor, yinelenen alan adları denetlenmiyordu → çıkarılan alanlar adları ve satır numaralarıyla, yinelenen alan adları uyarıyla bildirilir; varyans türü yanlış seçilip doğrudan CV ortancası %100'ü aşarsa uyarı yazılır; üstbilgideki "12 birim" ifadesi "yaklaşık 15 birim" oldu; sonuç değişir mi: hayır.

## kur_modeli

- Kür (iyileşme oranlı) modeller: log-log bağlantıda varsayılan en iyileme kötü bir yerel noktada duruyordu (örnekte logLik -2598,750; doğrusu -2580,012; Medium ve Poor kür oranları sıfıra çöküyordu) → log-log bağlantıda (ve standart hata ya da Hessian sorunu görülen çözümlerde) kür oranının üç başlangıç değerinden yeniden en iyilenir, en yüksek olabilirlik alınır ve bu bir notla bildirilir; sonuç değişir mi: evet, log-log bağlantıda (örnek: kür oranları 0,377 ve 0,060, logLik -2580,012); lojistik ve probit bağlantıda ve sorunsuz çözümlerde aynı.
- Kür modelleri: "son olaydan sonra sansürlü gözlem yok" uyarısı hiçbir veride çalışmıyordu → son olaydan sonra hiç ya da 5'ten az sansürlü gözlem varsa uyarı yazılır (sayıyı da söyler); sonuç değişir mi: hayır (yalnız uyarı).
- Kür modelleri: kür oranı sıfıra ya da bire giden katsayıda anlamsız Wald z ve p (örnek: p = 0,949) yazılıyordu → |β| > 10 ya da SH > 10 olan katsayının z ve p değeri boş bırakılır ve uyarı yazılır; sonuç değişir mi: evet, yalnız bu sınır katsayılarında (z ve p boş); öteki sonuçlar aynı.
- Kür modelleri: log-log bağlantıda `theta` güven aralığının uçları ters yazılıyordu → uçlar sıralanır; sonuç değişir mi: evet, yalnız log-log bağlantıda (alt ve üst uç yer değiştirir).
- Kür modelleri: lojistik, probit ve log-log bağlantıda `theta` satırının doğal ölçek standart hatası boştu (başlık "delta yöntemiyle" diyordu) → delta yöntemiyle hesaplanıp yazılır (örnek: 0,0433); sonuç değişir mi: hayır (boş olan hücre dolar).
- Kür modelleri: tahmin zamanlarında ondalık virgül ("1000,5") sessizce iki zaman sayılıyordu → noktalı virgül varsa yalnız o ayırıcı olur ("1000,5; 2000"); virgülle yazımda hangi sayıların okunduğunu söyleyen not eklenir; sonuç değişir mi: hayır.
- Kür modelleri: eksik değerli satırlar haber verilmeden çıkarılıyordu → çıkarılan satır sayısı bildirilir; sonuç değişir mi: hayır.
- Kür modelleri: ortak değişken olayı kusursuz ayırdığında yalnız ham "non-finite value supplied by optim" iletisi çıkıyordu → iletiye nedenin ayrışma olabileceği ipucu eklendi; sonuç değişir mi: hayır (yalnız ileti).
- Ek: zaman ve durum sütunları ortak katmandaki `mq_sayisal` ile okunur; sonuç değişir mi: hayır.

## ld_budama_ibd

- LD budama ve IBD: "Yalnız KING" + "KING-homo" seçilince bütün ebeveyn–yavru çiftleri "1. derece: tam kardeş" etiketleniyordu (örnek veride 65 çift) → ebeveyn–yavru ↔ tam kardeş ayrımı KING-homo'nun kendi k0 kestirimiyle yapılır (çift tablosunda `k0_KING`, `k1_KING` sütunları); IBS0 ve k0'ın ikisi de yoksa etiket "1. derece: ayrım yapılamadı" olur; sonuç değişir mi: evet (yalnız KING-homo; KING-robust ve momentler yöntemi aynı).
- LD budama ve IBD: "Yalnız otozomal SNP'ler" kapatılsa da X/Y gibi otozom dışı SNP'ler çıkarılıyordu → onay kapalıyken kromozom numarası 22'den büyük SNP'ler de kullanılır ve bir not bildirir (açık olan varsayılan davranış aynı); sonuç değişir mi: evet (yalnız onayı kapatıp otozom dışı SNP'li veri kullanan çalıştırmada); açıklama metni (TR/EN/ES) güncellendi.
- LD budama ve IBD: özet tablosundaki "Süzgeçten geçen otozomal SNP" satırı MAF ve çağrı oranı süzgecinden önceki sayıydı, IBD'de kullanılan SNP sayısı hiçbir yerde yoktu → satır "Tek biçimli olmayan otozomal SNP" diye adlandırıldı ve "MAF ≥ … ve çağrı oranı ≥ … süzgecinden geçen SNP (budamadan önce)" satırı eklendi (örnekte 7077); sonuç değişir mi: hayır (yalnız tabloya satır).
- LD budama ve IBD: birey ve aile kimlikleri sayısal sütun türüne göre farklı metne dönüşebiliyordu → `mq_kimlik` ile okunur; MoM'da geometrik kısıt açıklaması (TR/EN/ES) eşitsizlik yönünü doğru söyler (k1² < 4·k0·k2 olan kestirimler sınıra taşınır); konsol cümlesi "2. dereceden yakın" yerine "2. derece ve daha yakın" der (sayı aynı); sonuç değişir mi: hayır.

## lme_uzamsal_korelasyon

- Uzamsal ve sürekli zaman korelasyon yapıları: "Aralık başlangıç değeri" 0 (otomatik) iken tek bir başlangıç deneniyor, örnek veride çözüm sınıra gidiyor (aralık ≈ 50 000; σ ≈ 268; her çeşit ortalamasının SH'si ≈ 268, p ≈ 0,93) ve hiçbir uyarı yazılmıyordu → gözlem uzaklıklarının dört yüzdeliği başlangıç olarak denenir, olabilirliği en yüksek çözüm raporlanır; çözüm sınırdaysa açık bir uyarı yazılır ve bulunan sonlu aralıklı yerel çözüm (aralık, logLik, başlangıç değeri) bildirilir; sonuç değişir mi: evet (yalnız otomatik başlangıçta ve olabilirlik çok tepeliyse: daha yüksek olabilirlikli çözüm raporlanabilir; gruplama faktörü varken otomatik başlangıç artık grup içi uzaklıklardan hesaplanır; süre yaklaşık dört katına çıkar). Başlangıç değeri verilmişse sonuç değişmez; sınır uyarısı o durumda da yazılır.
- Aynı şablon: Manhattan ve en büyük (Chebyshev) uzaklık üstel dışındaki yapılarda geçerli olmayan (pozitif tanımlı olmayan) bir korelasyon matrisi verebiliyor, sonuç uyarısız yazılıyordu (ör. Gauss + Manhattan: en küçük özdeğer −1,09) → kestirilen korelasyon matrisi denetlenir; geçersizse çalıştırma açık iletiyle durur; sonuç değişir mi: evet (yalnız geçersiz matris veren yapı–ölçü–konum birleşimlerinde; eskiden yorumlanamaz sayılar çıkıyordu).
- Aynı şablon: koordinat (ya da zaman) sütunu sabit etki listesinden uyarısız çıkarılıyordu; boylamsal veride zaman eğilimi, uzamsal veride eğilim yüzeyi modele konamıyordu → koordinat sütunu sabit etki olarak da kullanılabilir; sonuç değişir mi: evet (yalnız koordinat sütunu sabit etki listesinde de seçiliyken; artık modele girer).
- Aynı şablon: yarıvariogram şeklinin başlığı "≈1 beklenir" diyor ve başvuru çizgisi 1'e çiziliyordu; doğru uyan modelde bile noktalar 1'in altında kalır (örnekte 0,75) → başvuru çizgisi normalleştirilmiş artıkların kareler ortalamasına çizilir ve değer başlıkta yazılır; sonuç değişir mi: hayır (yalnız şekil).
- Aynı şablon: ML + gruplama faktörü seçiliyken güven aralıkları tablosu sabit etkiler tablosundaki standart hatadan daha dar bir aralık veriyordu (yarı genişlik 5,30; tablodaki SH ile 6,13) → aralık sabit etkiler tablosundaki SH ve serbestlik derecesiyle kurulur; sonuç değişir mi: evet (yalnız ML + gruplama faktörü + güven aralıkları tablosu birlikteyken; aralıklar √(N/(N − p)) kat genişler). REML'de ve gruplama faktörsüz modelde değişmez. ML'de tablonun altına not yazılır: standart hata √(N/(N − p)) ile çarpılmıştır (çarpan sayıyla); çarpansız SH = SH / çarpan (KARAR 2026-11-17).
- Aynı şablon: form açıklamasından başka yazılımın adı çıkarıldı; aralık başlangıcı, uzaklık ölçüsü ve güven aralığı alanlarının açıklamaları yeni davranışı anlatıyor; sonuç değişir mi: hayır.

## lme_varyans_yapilari

- Heteroskedastik varyans yapıları: varIdent'te varyans değişkeni aynı zamanda sayısal bir sabit etki ya da rastgele eğimse o sütun sessizce etkene çevriliyordu (ör. Time'ın doğrusal etkisi 11 düzeyli kategorik etkiye dönüşüyor; rastgele eğimle çalıştırma bitmiyordu). Sayı kodlu grup sütununda da sabit etkinin kodlaması varyans fonksiyonu seçimine bağlıydı → varyans katmanları ayrı tutulur; sabit ve rastgele kısım seçildiği gibi kalır; sonuç değişir mi: evet (yalnız varIdent'in varyans değişkeni sayısal bir sütunsa ve aynı sütun sabit etki ya da rastgele eğim olarak da seçiliyse).
- Aynı şablon, varIdent: oranın 1 olduğu başvuru düzeyi veride ilk rastlanan katmandı; gruplama faktörü yokken satır sırasına bağlıydı ve sabit etkilerin başvuru düzeyiyle uyuşmayabiliyordu → oranlar ve artık standart sapması (σ) etkenin ilk düzeyine göre yazılır; başvuru düzeyinin adı tabloda belirtilir; sonuç değişir mi: evet (yalnız paketin başvuru aldığı katman etkenin ilk düzeyi değilse: oranlar ve σ başka düzeye göre yazılır; model, olabilirlik ve sabit etkiler aynıdır). Örnek ayarda değişmez.
- Aynı şablon, varPower: ortak değişkende sıfır varsa paketin bellek hatası iletisi çıkıyordu → hesaptan önce açık iletiyle duruyor; sonuç değişir mi: hayır.
- Aynı şablon: ML + gruplama faktörü seçiliyken güven aralıkları tablosu sabit etkiler tablosundaki standart hatadan daha dar bir aralık veriyordu (kesişimde yarı genişlik 23,231; tablodaki SH ile 23,637) → aralık sabit etkiler tablosundaki SH ve serbestlik derecesiyle kurulur; sonuç değişir mi: evet (yalnız ML + gruplama faktörü + güven aralıkları tablosu birlikteyken). REML'de ve gruplama faktörsüz modelde değişmez. ML'de tablonun altına not yazılır: standart hata √(N/(N − p)) ile çarpılmıştır (çarpan sayıyla); çarpansız SH = SH / çarpan (KARAR 2026-11-17).
- Aynı şablon: form açıklamasından başka yazılımın adı çıkarıldı; güven aralığı alanının açıklaması yeni hesabı anlatıyor; sonuç değişir mi: hayır.

## lokasyon_yil_tbd

- Lokasyon × yıl birleşik analiz: lokasyon ve/ya da yıl sabit seçilince 'REML sabit etki F testleri' tablosunda etkileşim adları boş çıkıyordu → adlar yazılır; şablon açıklaması, seçenek etiketi ve uyarı iletisindeki ticari yazılım adları kaldırıldı; sonuç değişir mi: hayır (yalnız etiket ve metin).

## markov_rejim

- Markov rejim değiştirme regresyonu: hata varyansı rejime göre değişirken 10 başlangıç yerel bir en büyükte kalıyordu (log-olabilirlik -221,449) ve bir rejimin sapması sıfıra giden dejenere çözümler kabul edilebiliyordu → varyans rejime göre değişince en az 30 başlangıç kullanılır, dejenere çözümler (bir rejimde sapma standart sapmanın binde birinden küçük ya da 5 gözlemden az rejim) atılır ve geçerli çözüm kalmazsa anlaşılır iletiyle durulur; çıktıya "başlangıçların kaçı en iyi çözüme vardı" satırı eklenir; sonuç değişir mi: evet (yalnız `degisken_varyans` açıkken; örnekte -221,449 yerine -201,878), kapalıyken sayılar aynı.
- Markov rejim değiştirme regresyonu: en büyükleyicinin yakınsama durumu okunmuyor, yineleme sınırına varan çözüm uyarısız sonuç sayılıyordu → yakınsama okunur; tam yakınsama bildirilmezse çıktıya uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı eklenir).
- Markov rejim değiştirme regresyonu: zaman sütunu yalnız etiketti; ters sıralı ya da tekrarlı dönemli veri uyarısız çözümleniyor, geçişler geriye doğru kuruluyordu → zaman sütunu seçiliyse artan ve tekil olması denetlenir, değilse anlaşılır iletiyle durulur; sonuç değişir mi: evet (ters sıralı veride eskiden sayı veriyordu, şimdi hata).
- Markov rejim değiştirme regresyonu: serinin ortasındaki eksik gözlemde satırlar atılıp boşluğun iki yanı zincirde ardışık sayılıyordu → ortada eksik gözlem varsa anlaşılır iletiyle (kaç satır, ilk satır) durulur; baştaki/sondaki eksikler eskisi gibi kırpılır; sonuç değişir mi: evet (ortada eksik olan veride eskiden sayı veriyordu, şimdi hata).
- Markov rejim değiştirme regresyonu: sınıra yapışmış geçiş olasılığında (örn. 1,6e-9) standart hata 0 yazılıyordu → bu olasılığın standart hatası boş (NA) yazılır; sonuç değişir mi: hayır (yalnız o hücre; kestirimler aynı).

## markov_rejim_py

- Markov rejim değiştirme regresyonu (Python): hata varyansı rejime göre değişirken bir rejimin sapması sıfıra giden dejenere çözüm kabul ediliyor, standart hataların hepsi boş çıkıyordu (log-olabilirlik -187,663) → dejenere çözümler atılır, kendi başlangıçlarından (en az 30) en iyi dejenere olmayan çözüm seçilir (R eşiyle aynı: -201,878); hepsi dejenereyse anlaşılır iletiyle durulur; çıktıya "başlangıçların kaçı en iyi çözüme vardı" satırı eklenir; sonuç değişir mi: evet (yalnız `degisken_varyans` açıkken), kapalıyken sayılar aynı.
- Markov rejim değiştirme regresyonu (Python): üç rejimde 10 başlangıç R eşinden belirgin düşük bir yerel en büyükte kalıyordu (-190,026; R: -180,806) → üç rejimde en az 30 başlangıç kullanılır (-182,272), standart hatalar kestirilemezse çok tepeli olabilirlik ve R sürümüyle karşılaştırma uyarısı yazılır; sonuç değişir mi: evet (k = 3'te; R'nin değerine yine varılamayabilir).
- Markov rejim değiştirme regresyonu (Python): zaman sırası denetlenmiyor, ters sıralı veri sessizce çözümleniyor; serinin ortasındaki eksikler zincirde bitişik sayılıyordu → R eşiyle aynı: ters sıralı/tekrarlı dönem ve ortadaki eksik anlaşılır iletiyle durdurulur; sonuç değişir mi: evet (bu iki durumda eskiden sayı veriyordu, şimdi hata).

## mars

- MARS: Veri sekmesinde "kategorik" türde duran yanıt sütununda değerler yerine düzey sıra numaraları modelleniyordu → sütunun değerleri kullanılır, sayıya çevrilemeyen değerde sütun adıyla ileti verilir; sonuç değişir mi: evet, yalnız yanıt kategorik türdeyse (örnekte GCV 8,40 → 11,20).
- MARS: "Doğrusal girecek açıklayıcılar" listesi düzenli ifade gibi eşleşiyordu (`Girth` seçilince `Girth_kare` de doğrusal giriyordu) → ad tam eşleşir (faktör açıklayıcıda bütün göstergeler birlikte doğrusal girer); sonuç değişir mi: evet, bir adı başka adın parçası olan açıklayıcılar ya da düzenli ifade karakteri içeren adlar varsa (örnekte GCV 9,83 → 10,81).
- MARS: İkili (lojistik) ve sayım (Poisson) yanıt dağılımı her zaman "subscript out of bounds" hatasıyla bitiyordu → uyum tablosuna GLM sapması, boş sapma ve AIC yazılır, tablolar ve şekiller üretilir; sonuç değişir mi: evet (bu seçenekler daha önce hiç sonuç vermiyordu).
- MARS: earth ve GLM uyarıları bastırılıyordu, GLM yakınsama durumu okunmuyordu → uyarılar "Uyarılar:" satırında yazılır, GLM yakınsamamışsa (tam ayrışma gibi) ayrıca bildirilir; sonuç değişir mi: hayır (yalnız bildirim).

## mcmc_ozel_model

- Kod panelindeki Tohum alanı etkisizdi; yalnız şablonun "Rastgele sayı tohumu" alanı belirleyiciydi, alanın üst sınırı panelinkinden küçüktü ve tohum çıktıda yoktu → alan 0 verilirse çalıştırma tohumu kullanılır; alan varsayılanı 1 aynı kalır (aynı zincir tohumları); üst sınır 2 147 483 647 olur; kullanılan tohum zincir bilgisi satırına yazılır; sonuç değişir mi: hayır (varsayılan ayarda).
- "Zincir başına kaydedilen yineleme" etiketi gerçek davranışla uyuşmuyordu (kaydedilen = yineleme ÷ seyreltme) → etiket "Zincir başına yineleme (seyreltmeden önce; kaydedilen = yineleme ÷ seyreltme)" olur ve çıktı satırı iki sayıyı birlikte yazar; sonuç değişir mi: hayır.
- Normal öncül kesişime de uygulanıyor ve bu bilinmiyordu → öncül açıkken çıktıya "kesişim dahil bütün katsayılara uygulanır; yordayıcılar merkezlenmediğinden kesişim x = 0 noktasındaki değerdir" notu yazılır; sonuç değişir mi: hayır (yalnız not).
- Kabul oranı 0 olan (tıkanmış) zincir uyarısız özet üretiyordu → kabul oranı 0,01'in altında ise uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Tek zincirde R-hat boş kalıyor ve neden söylenmiyordu; eksik değerli satırların atıldığı yazılmıyordu → iki not eklenir; sonuç değişir mi: hayır.

## mevsimsel_x13

- Mevsimsel düzeltme (X-13): tarih sütununun yalnız ilk satırı okunuyor, ters sıralı veri ya da aylık veriye seçilen "4" frekansı sessizce çözümleniyordu → tarih sütunu seçiliyse artan, düzgün aralıklı ve seçilen frekansla uyumlu olması denetlenir, değilse anlaşılır iletiyle durulur; sonuç değişir mi: evet (bu durumlarda eskiden yanlış sayı veriyordu, şimdi hata); geçerli verilerde aynı.
- Mevsimsel düzeltme (X-13): tablodaki Box-Ljung (24) X-13'ün özet satırından farklıydı (144 artık; 26,65 ve 23,88), serbestlik derecesi ve p değeri yoktu → X-13 gibi yalnız etkin son artıklarla hesaplanır (örnekte 23,88), model özetine serbestlik derecesi (ARMA parametre sayısı kadar düşürülmüş) ve p değeri satırları eklenir; sonuç değişir mi: evet (Box-Ljung değeri; öteki satırlar aynı, iki satır sona eklenir).
- Mevsimsel düzeltme (X-13): çeyrek etiketi `1949-Q1` başlangıç olarak okunmuyordu (şablonun kendi tablosu bu etiketi yazıyor) → okunur; sonuç değişir mi: hayır (eskiden hata verirdi).
- Mevsimsel düzeltme (X-13): sıfır ya da negatif değerli seride otomatik dönüşüm uyarısız dönüşümsüz modele düşüyordu → çıktıya açık uyarı yazılır; sonuç değişir mi: hayır (yalnız uyarı).
- Mevsimsel düzeltme (X-13): şekil başlıkları ve eksen etiketi arayüz dilini izlemiyor, İngilizce kalıyordu → başlık ve eksen etiketi arayüz diliyle yazılır; sonuç değişir mi: hayır (yalnız şekil metni).
- Mevsimsel düzeltme (X-13): QS tablosunda satır adları X-13 anahtarlarıydı, hangi seri/aralık olduğu anlaşılmıyordu → tabloya sonda "aciklama" sütunu eklenir (ör. "özgün seri, son 8 yıl"); sonuç değişir mi: hayır (mevcut sütunlar aynı, bir sütun eklenir).

## mfdfa

- Çok fraktallı dalgalanma analizi (MFDFA, R): sabit ya da doğrusal bölgeli seride bölüt artık varyansı yuvarlama hatası kadar (1e-30) pozitif kaldığından denetim geçiyor, negatif q için h(q), Δh ve spektrum sessizce anlamsız çıkıyordu (ör. h(−5) = 11,67) → artık varyans profilin ortalama karesine göre sıfıra yakınsa çalıştırma anlaşılır iletiyle duruyor; sonuç değişir mi: evet (yalnız sabit/doğrusal bölgesi olan seride, anlamsız sonuç yerine ileti).
- MFDFA (R): (q_max − q_min) adımın tam katı değilse ızgara q_max'a ulaşmıyor ama özet ve başlık q_max'ı adlandırıyordu → etiketler gerçek son q'yu yazıyor ve bir not ekleniyor; sonuç değişir mi: hayır (yalnız etiket ve not; sayılar aynı).

## mfdfa_py

- Çok fraktallı dalgalanma analizi (MFDFA, Python): R ikiziyle aynı iki düzeltme: sabit/doğrusal bölgeli seride artık varyans sıfıra yakınsa çalıştırma iletiyle duruyor (eskiden anlamsız h(q), Δh çıkıyordu); q ızgarası q_max'a ulaşmıyorsa etiketler ve başlık son q'yu adlandırıyor, not ekleniyor; sonuç değişir mi: evet (yalnız sabit/doğrusal bölgeli seri; öteki durumlarda sayılar aynı).

## mgwr

- Açıklama "tek bant genişlikli GWR (AICc) de verilir" diyordu; yalnız GWR bant genişliği veriliyordu → n ≤ 800'de yeni bir tablo (en sonda) GWR ile MGWR'yi aynı tanımlarla (RSS, iz(S), AICc, R², bant genişliği) karşılaştırır; sonuç değişir mi: hayır (mevcut tablolar aynı, ek tablo).
- Bant genişliği ölçütü "AICc" ve "AIC" geri uydurmada aynı sonucu veriyordu, formda bu yazmıyordu → ölçüt açıklaması bunu söyler; sonuç değişir mi: hayır.
- Eksik değerli satırlar sayısı yazılmadan atılıyordu → atılan satır sayısı ve kalan n yazılır; sayısal çevirme `mq_sayisal` ile; sonuç değişir mi: hayır.
- Geri uydurma üst yineleme sınırında ya da R_j yinelemesi 1000 adımda yakınsamadan çıksa hiçbir ileti yoktu → uyarı yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Sabit bant genişlikleri alanında ondalık virgül liste ayırıcısı sayılıyordu → alanda ';' varsa ayırıcı ';' ve virgül ondalıktır (ör. 99999,5; 89999,5), yoksa eskisi gibi virgülle ayrılır ve ondalık için nokta gerektiği notu yazılır; sonuç değişir mi: hayır (yalnız ';' kullanan girdide; önceden hata ya da yanlış bant).

## mirt_cok_boyutlu

- Çok boyutlu MTK: 1 ve 2 gibi kodlanmış iki kategorili maddede "sabit alt asimptot (tahmin)" sessizce uygulanmıyordu (madde aşamalı modele gidiyordu) → iki kategorili madde 0/1'e çevrilir (not yazılır), tahmin uygulanır; sonuç değişir mi: evet (tahmin > 0 ve 1/2 kodlu veride; örnek: log olabilirlik -2658,8051 → -2659,3346, g = 0,2); tahmin 0 iken olabilirlik aynı, parametre sütunu d1 yerine d.
- Çok boyutlu MTK: kategorisi eksik (0, 1, 3) madde sessizce yeniden kodlanıyor, parametre sayısı yanlış ve d3 NaN çıkıyordu → madde adı ve olmayan kategoriyle durur; sonuç değişir mi: evet (bu veride: sonuç yerine anlaşılır hata).
- Çok boyutlu MTK: MHRM'de log olabilirlik, AIC, BIC NaN çıkınca "EM yakınsamadı" deniyordu; M2 hesaplanamayınca satırlar sessizce eksikti; hiç yanıtı olmayan satırlar sessiz atılıyordu → yakınsama iletisi kullanılan yöntemi söyler, NaN için açıklayıcı uyarı, M2 gerekçesi ve atılan satır sayısı yazılır; Kestirim algoritması açıklaması sonucun Tohum alanından etkilenmediğini söyler; sonuç değişir mi: hayır (yalnız bildirim).

## mnar_referans_atama

- Referansa dayalı atama, bootstrap: SH sütunu boş ve p = 0 yazılıyor, nedeni belli değildi → çıktıya "güven aralığı yüzdelik aralıktır, standart hata kestirilmez, en küçük anlamlı p ≈ 1/B" notu ve p = 0 satırları için "p < 1/B" uyarısı yazılır; sonuç değişir mi: hayır (yalnız not).
- Referans grubu boşken "ilk düzey" yalnız harf sırasına göre seçiliyordu → grup sütunu Veri sekmesinde kategorik ise Veri sekmesi sırası, değilse sayısal sıra (tüm değerler sayıysa; "2" < "10") ya da harf sırası kullanılır; seçilen düzey ve kural çıktıya yazılır; metin vizit etiketleri de aynı kurala bağlandı; sonuç değişir mi: evet, yalnız referans boşken ve (a) grup sütunu Veri sekmesinde faktör olup ilk düzeyi alfabetik ilk değilse ya da (b) etiketler sayıysa ve "10" < "2" sırası ilk düzeyi değiştiriyorsa (ya da metin vizitlerde "V10" gibi sayı etiketleri); aksi hâlde hayır.
- Bootstrap ve yaklaşık Bayes'te tohum çıktıda yazılmıyordu → "Tohum: N" yazılır; sonuç değişir mi: hayır.
- Atılan satır sayısı yazılmıyordu → "Atılan satır: N (…)" yazılır; sonuç değişir mi: hayır.

## mokken

- Mokken ölçek analizi: 100'den az yanıtlayıcıda gizil monotonluk adımı "Minsize value is too high" hatasıyla duruyor ve Hj/AISP sonuçları kayboluyordu → artık puan grubu en küçük büyüklüğü n/2'ye indirilir ve not yazılır; elle girilen değer n/2'yi aşarsa alanı anan iletiyle durur; sonuç değişir mi: evet (n < 100'de hata yerine sonuç).
- Mokken ölçek analizi: eksik değerli satırlar sessizce atılıyordu; monotonluk tablosundaki Hj iki ondalığa yuvarlanmıştı → atılan satır sayısı yazılır, Hj yuvarlanmamış değerdir; sonuç değişir mi: hayır (Hj yalnız görünüm, örn. 0,50 → 0,496104).

## msm_ipw

- Marjinal yapısal model: pay modeline zamanla değişen bir değişken konabiliyor, pay ile payda aynıysa tüm ağırlıklar 1 oluyor ve MSM sessizce ham Cox modeline dönüşüyordu → pay modelindeki sütun birim içinde değişiyorsa "birim içinde değişiyor (zamanla değişen değişken); pay modeli yalnız sabit değişkenler içermelidir" iletisiyle durur; sonuç değişir mi: evet (bu hatalı girdide sessiz sonuç yerine ileti; doğru kullanımda hayır).
- Marjinal yapısal model: ağırlık modelinden çıkarılan sütunlar (tedavi, olay, birim, zaman) iletisiz atılıyor, lojistik modelde izlem zamanı eklenemiyordu → çıkarılan sütunlar konsola yazılır; yeni "Lojistik ağırlık modeline izlem zamanını ekle" seçeneği (varsayılan kapalı) payda ve pay modeline doğal kübik spline ekler; sonuç değişir mi: varsayılanda hayır; seçenek açıksa evet (örnek: lojistik HR 0,357 yerine 0,382).
- Marjinal yapısal model: yakınsamayan ya da sonsuza giden Cox kestirimi (az olay) sonuç tablosuna yazılıyordu → yakınsama uyarısı, |katsayı|>10 ya da model SH>100 ise "Ağırlıklı Cox modeli yakınsamadı ya da kestirim sonsuza gidiyor" iletisiyle durur; Cox uyarıları Türkçeye çevrilir; sonuç değişir mi: yalnız bu geçersiz durumda (tablo yerine ileti).
- Marjinal yapısal model: eksik değerli satırlar sessizce atılıyor ve birim ortasından atılan satırlar kümülatif çarpımda boşluk bırakıyordu → atılan satır sayısı yazılır ve aralık boşlukları uyarılır; yeni "Payda değişkenlerindeki eksikleri birimin son gözlemiyle doldur" seçeneği (varsayılan kapalı) satır atmak yerine son gözlemi taşır; sonuç değişir mi: varsayılanda hayır, seçenek açıksa evet (eksik içeren veride).
- Marjinal yapısal model: kararsız (pay boş) ağırlıkta uç değer uyarısı yoktu → en büyük çarpım ağırlığı 20'yi aşarsa ya da etkin örneklem büyüklüğü n/10'un altındaysa uyarı yazılır; kırpma etiketi "tedavi ve sansür ağırlıklarına ayrı ayrı" olduğunu söyler; yeni "Sonuç (MSM) modeline pay değişkenlerini de ekle" seçeneği (varsayılan kapalı) kararlı ağırlıklarda sonucu pay değişkenlerine koşullu verir; ondalık virgüllü metin payda sütunu zaman aşımı yerine iletiyle reddedilir; ağırlık grafiğinin eksen etiketleri üst üste binmez ve bilimsel gösterim kullanmaz; sonuç değişir mi: varsayılanda hayır (uyarı, etiket, ileti, grafik); seçenek açıksa evet (örnek: log HR −0,9378 yerine −0,9617).

## nedensel_cizge

- Nedensel çizge: etki = doğrudan iken "Bütün kümelerin geçerli olduğu eşdeğer DAG sayısı" toplam etki ölçütüyle değerlendirilip 0 / 6 çıkıyordu → doğrudan etki ölçütüyle (örnekte 6 / 6); sonuç değişir mi: evet, yalnız doğrudan etki + eşdeğer DAG satırında.
- Nedensel çizge: çizge değişkeni veride yoksa (ör. büyük/küçük harf) sınanamayan bağımsızlıklara karşın "çizge veriyle tutarlı" yazılıyordu → "sınananlardan hiçbiri çelişmedi, ancak N bağımsızlık sınanamadı; tutarlı denemez" yazılır; sonuç değişir mi: hayır (yalnız karar cümlesi).
- Nedensel çizge: loess testinde çoklu test düzeltmesi uygulanmıyordu ve bu söylenmiyordu → not ve beklenen yanlış alarm sayısı yazılır; yeni "Loess testinde Bonferroni uyarlı güven aralığı" seçeneği (varsayılan kapalı = eski sonuç); sonuç değişir mi: hayır (seçenek açılırsa evet, aralıklar genişler).
- Nedensel çizge: tam doğrusal bağımlılıkta kısmi korelasyon −1 görünüyordu ve uyarı yoktu → |r| = 1 için uyarı; sonuç değişir mi: hayır.
- Nedensel çizge: Türkçe harfli değişken adı sözdizimi hatası veriyordu → ileti adları çift tırnak içine almayı söyler; sonuç değişir mi: hayır.

## nitelik_kartlari

- Nitelik kontrol kartları (np): üst kontrol sınırı alt grup büyüklüğünü aşabiliyordu (ulaşılamaz sınır) → artık alt grup büyüklüğünde kesiliyor (p kartının 1'de kesilmesi gibi); sonuç değişir mi: evet (yalnız hesaplanan üst sınır alt grup büyüklüğünün üstünde çıkıyorsa; örnek: n = 20, sınır 21,91 → 20; ihlal sayıları değişmez).
- Nitelik kontrol kartları: sayım, büyüklük ya da alt grup kimliği boş olan satırlar haber verilmeden atılıyor ve Aşama I kalan alt gruplara göre kayıyordu → atılan satır numaraları ile Aşama I'in kapsadığı alt gruplar not olarak yazılıyor; alt grup kimliği seçilmediyse etiketler özgün satır numarası oluyor; sonuç değişir mi: hayır (sayısal sonuç aynı; yalnız not ve alt grup kimliği yokken etiketler, satır atılmışsa).
- Nitelik kontrol kartları: "Kural 4" etiketi hep "8 nokta" yazıyordu → seçilen ardışık nokta sayısını yazıyor; sonuç değişir mi: hayır (yalnız etiket).
- Nitelik kontrol kartları: alt grup sırası sütun türüne göre değişiyordu → yeni "Alt grup sırası" seçeneği (varsayılan "sütun türüne göre" = eski davranış; "dosya sırası"); sonuç değişir mi: hayır (varsayılanda aynı).
- Nitelik kontrol kartları: sayım/büyüklük Veri sekmesinde "kategorik" görünse bile artık sayı olarak okunuyor, sayıya çevrilemeyen değer anlaşılır iletiyle durduruyor; sonuç değişir mi: hayır.

## nlme_karma

- Doğrusal olmayan karma model: nlme motorunda "REML" seçeneği sabit etkileri değiştirmiyor, yalnız varyansları N/(N − p) ile ölçekliyor; form ve çıktı bunu söylemiyordu → "Kestirim yöntemi" alanına açıklama ve REML çalıştırmasının çıktısına not eklendi; sonuç değişir mi: hayır.
- Doğrusal olmayan karma model: ML'de güven aralıkları tablosu, sabit etkiler tablosundakinden farklı (düzeltilmemiş) standart hatayla kuruluyordu; aynı çıktıda p değeri ile aralık tutarsız olabiliyordu → aralık sabit etkiler tablosundaki standart hatayla kurulur (kestirim ± t × SH); tablonun altındaki not ML'de SH'nin √(N/(N − p)) ile çarpıldığını ve çarpanı yazar; sonuç değişir mi: evet, ML'de güven aralıkları √(N/(N − p)) kadar genişler (örnek: Asym [159,41; 222,69] → [157,96; 224,14]); REML ve Laplace motoru sonuçları aynı.
- Doğrusal olmayan karma model: Laplace (nlmer) motorunda tolerans alanları sonucu değiştirmiyor, form açıklaması etkili olduklarını düşündürüyordu; form açıklamasında ticari yazılım adı geçiyordu → açıklamalar düzeltildi, ad kaldırıldı; yanıt ve açıklayıcı sütunu her zaman değerleriyle okunur; sonuç değişir mi: hayır.

## nokta_sureci_model

- Strauss modelinde γ > 1 (çekim) çıkınca uyarı yoktu; γ > 1 geçerli bir nokta süreci tanımlamaz → "Strauss modeli kümelenmeye uygun değil; Thomas, Matérn kümesi ya da LGCP seçin" uyarısı yazılır, etkileşim tablosunun başlığı da γ > 1'in geçersiz olduğunu söyler; sonuç değişir mi: hayır (yalnız uyarı; bei, r = 7,5: γ = 1,1205).
- Strauss standart hataları (z = 84,9 gibi) sözde olabilirlikten geliyor, nokta bağımlılığını yok sayıyor ve çok küçük çıkıyordu; uyarı yalnız özel durumda yazılıyordu → katsayı tablosunun başlığı "SH güvenilir değil" der ve her Strauss çalıştırmasında uyarı (parametrik bootstrap önerisiyle) yazılır; sonuç değişir mi: hayır (sayılar aynı).
- Strauss sözde logLik, AIC ve BIC değerleri Poisson değerleriyle ve profil tablosuyla karşılaştırılabilirmiş gibi duruyordu → not satırı karşılaştırılabilirlik koşulunu ve sözde olabilirlikte kullanılan olay sayısını (örnekte 3604 olayın 3420'si) yazar, "Model uyumu" tablosuna en sona bu sayı satırı eklenir, profil tablosunun başlığı farkı belirtir; sonuç değişir mi: hayır.
- "Kestirilen yoğunluk" tablosu Strauss ve kümelenme modellerinde yalnız trendi gösterdiği hâlde başlığında bu yoktu → başlık "(trend; etkileşim ve kümelenme hariç)" der; sonuç değişir mi: hayır. Koordinat ve ortak değişken sütunları `mq_sayisal` ile okunur (sayı olmayan değerde açık ileti).

## nominal_karma

- Nominal karma model: şablon açıklamasındaki ticari yazılım adları kaldırıldı (TR/EN/ES); sonuç değişir mi: hayır.

## notralite_testleri

- Nötralite testleri: adı "Tümü" (İngilizce "All") olan popülasyon bütün örneklem satırının yerine geçiyor, bütün örneklem satırı kayboluyordu → popülasyon satırı "Tümü (popülasyon)" adıyla ayrı veriliyor, bütün örneklem satırı korunuyor; sonuç değişir mi: evet (yalnız popülasyon bütün örneklem etiketiyle aynı adı taşıyorsa).
- Nötralite testleri, uyumsuzluk dağılımı: θ0/θ1 sınıra dayandığında ya da eniyileyici yakınsamadığında sayılar sıradan kestirim gibi yazılıyordu → ilgili gruplar için uyarı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Nötralite testleri, özet kipi: n, haplotip sayısı ve θπ sütunları kategorik türdeyse paketin ham hatası çıkıyordu → sayısal değerler ortak yardımcıyla okunuyor, çevrilemeyen değerde açık ileti; sonuç değişir mi: hayır.
- [3.8.0] Nötralite testleri: yeni alan "Uyumsuzluk uyumunda ek başlangıç sayısı" (varsayılan 0 = tek başlangıç): sıfırdan büyükse sabit ızgaradan ek başlangıçlar denenir, en küçük SSD alınır (τ, θ0, θ1 değişebilir, SSD artmaz); sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_notralite_testleri.py`).

## offtype

- Off-type karar şeması: 'k ↔ n tablosu' ve 'I. ve II. tip hata' grafiği başlıkları iki aşamalı kipte de tek test kuralına ait olduğunu söylemiyordu → başlıklara 'tek test' eklendi; açıklamadaki 'koşu' sözcüğü 'çalıştırma' oldu; sonuç değişir mi: hayır.

## olcum_hatasi

- Ölçüm hatası düzeltmesi: Veri sekmesinde "kategorik" türde duran sonuç, hatalı yordayıcı ya da referans sütununda değerler yerine düzey sıra numaraları kullanılıyordu (ör. bir ondalığa yuvarlanmış ölçümler) → sütun değerleriyle okunur, sayıya çevrilemeyen değerde ileti verilir; sonuç değişir mi: evet, yalnız bu sütunlar kategorik türdeyse (düzeltilmiş katsayı örnekte 2,243 → 0,2064).
- Ölçüm hatası düzeltmesi: tablo başlığındaki güvenilirlik oranı, ortak değişken varken düzeltmede kullanılan oranı göstermiyordu (başlık 0,7988; kullanılan 0,6835) → başlık "güvenilirlik oranı, ortak değişkenler verildiğinde" ve kullanılan değeri yazar; sonuç değişir mi: hayır (yalnız başlık).
- Ölçüm hatası düzeltmesi: SIMEX λ alanında ondalık virgülle yazılan değerlerde ileti ondalık ayırıcının nokta olduğunu söylemiyordu → ileti ekledi; sonuç değişir mi: hayır.

## optimal_olcekleme

- Optimal ölçekleme: sıralı ve nominal düzeyde varsayılan yakınsama ölçütü (1e-6) çözümü tam yakınsatmıyor, yükler ve dönüşümler üçüncü basamakta ayrılabiliyordu, bundan kullanıcı habersizdi → çıktıya "yakınsama ölçütü gevşek" notu ve yineleme sınırına ulaşılırsa uyarı eklendi; ölçütün varsayılan değeri değişmedi (kesin çözüm için 1e-10 ya da daha küçük seçilir); sonuç değişir mi: hayır (yalnız çıktı notu).
- Optimal ölçekleme: metin etiketli maddelerde düzey sırası sessizce harf sırasına ya da ilk görülme sırasına göre alınıyor, çıktıdaki not sırayı yanlış anlatıyordu → düzey sırası ortak kurala bağlandı (etken sütunda Veri sekmesi sırası; etiketler sayıysa sayısal sıra: 1, 2, 10; aksi hâlde harf sırası), not gerçek sırayı etiketlerle yazar, nicelendirme ve dönüşüm tablolarının sonuna `etiket` sütunu eklendi; sonuç değişir mi: evet (1, 2, 10 gibi sayı etiketli metin sütunlarında eskiden 1, 10, 2 sırası kullanılıyordu; metin etiketlerde sıra değişmedi, yalnız doğru anlatılıyor).
- Optimal ölçekleme: princals'ta `y` seçiliyse sessizce değişkenlerden çıkarılıyordu → bu durum notla bildirilir; sonuç değişir mi: hayır.
- Optimal ölçekleme: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve değişken başına eksik sayısı yazılır; sonuç değişir mi: hayır.
- Optimal ölçekleme: "Kategori nicelendirmeleri" ve morals "standartlaştırılmış optimal puanlar" tablolarının başlıkları gerçek ölçeği söylemiyordu → başlıklar ölçeği yazar (nicelendirme = standartlaştırılmış kategori puanı × birinci yük / √n; morals puanları birim norm, standartlaştırılmış değer = puan × √n); sonuç değişir mi: hayır (yalnız başlık metni).
- Optimal ölçekleme: konsolda Gifi'nin İngilizce çıktısı (Call, Loss value, Eigenvalues…) Türkçe arayüzde de yazılıyordu → yalnız İngilizce/İspanyolca arayüzde yazılır; aynı değerler tablolarda zaten var; sonuç değişir mi: hayır.

## optimal_tasarim

- Optimal tasarım: verimlilik tablosu başlığında ve şablon açıklamasında ticari yazılım adı geçiyordu → 'kodlu ölçekte' denildi; tasarım noktaları şeklinin başlığı 'büyüklük = yineleme' yerine 'büyüklük = bu konumdaki nokta sayısı' oldu; sonuç değişir mi: hayır.

## oran_meta

- Tek oran meta-analizi: yakınsama eşiği τ²'de mutlaktı (varsayılan 1e-5); ham oran, arksinüs ve Freeman-Tukey gibi τ²'nin çok küçük olduğu ölçeklerde yineleme erken duruyor ve yakınsama durumu hiçbir yerde bildirilmiyordu → yinelemeli kestiricilerde (REML, ML, EB, PM, SJ) sonuç eşik 1e-10 çözümüyle karşılaştırılır; τ² göreli 1e-3'ten çok farklıysa sıkı çözüm kullanılır ve "Yakınsama notu" yazılır, aksi hâlde eşik ve yakınsama durumu bir satırla bildirilir; sonuç değişir mi: yalnız eşik τ²'ye göre gevşekken (örnekte ham oran τ² 0,000399 → 0,000376, birleştirilmiş oran 0,029603 → 0,029311; Freeman-Tukey τ² 0,007137 → 0,007127); logit ve GLMM'de hayır.
- Tek oran meta-analizi: eksik değerli çalışmalar sessizce atılıyordu → atılan çalışma sayısı ve kalan sayı yazılır; sonuç değişir mi: hayır.
- Tek oran meta-analizi: alt grup düzeyleri her zaman harf sırasındaydı ("10", "2"'den önce) → ortak düzey kuralı (Veri sekmesi sırası; metinse hepsi sayıysa sayısal, değilse harf); sonuç değişir mi: yalnız sayı kodlu alt grup etiketlerinde tablo satır sırası değişir; testler ve tahminler aynı.

## orijin_dol_denemesi

- Orijin–döl denemesi: varsayılan Duncan (ve Tukey/Bonferroni) populasyon karşılaştırması artık KO (örnek veride 15,09; sd 3125) ile yapılıyordu, oysa aynı çıktıdaki populasyon F testi sentezlenmiş paydayla (91,87; sd 80,36) yapılır; karşılaştırma testin sınadığından çok daha küçük bir hata terimi kullanıyor ve farkları yaklaşık 2,5 kat kolay anlamlı buluyordu (22 populasyon 12 harf grubuna ayrılıyordu) → varsayılan hata terimi artık populasyon F testinin paydasıdır (KO(tekrar × populasyon) + KO(aile) − KO(tekrar × aile), Satterthwaite sd'si); konsol hangi terimin ve hangi sayıyla kullanıldığını yazar; "Artık (hata) KO" ve "Aile(populasyon) KO" seçenekleri forumda durur (eski sonuç için "Artık" seçilir); sonuç değişir mi: evet (varsayılan ayarda populasyon harf grupları daha az, daha geniş olur).
- Orijin–döl denemesi: anlamlılık düzeyi formda 0,5'e kadar girilebiliyor, çalıştırma ise 0,5'i reddediyordu → formun üst sınırı 0,49; sonuç değişir mi: hayır.
- Orijin–döl denemesi: kimlik sütunları (populasyon, aile, tekrar) ve iki özelliğin ortak birey sayımı ortak yardımcılarla okunur (`mq_kimlik`, `mq_sayisal`); sonuç değişir mi: hayır (yalnız "1e+05" türü gösterim ve Veri sekmesinde "kategorik" işaretli sayısal sütun durumu).

## orneklem_secimi

- Örneklem seçimi: 5000 birimden büyük çerçevede "bütün çerçeve birimleri" tablosu sessizce ilk 5000 satırla kesiliyordu → kesildiği, tabloda olmayan birim sayısı ve seçilen birimlerin tamamının "Seçilen örneklem" tablosunda olduğu not olarak yazılır; sonuç değişir mi: hayır (seçim ve olasılıklar aynı).

## ortak

- Kod sekmesi, bütün şablonlar: sayısal bir alana (yanıt, ölçüm, doz, zaman gibi) seçilen sütun Veri sekmesinde "kategorik" türdeyse R'ye etken olarak gidiyor ve birçok şablon değerler yerine kategori sıra numaralarını (1, 2, 3 …) kullanıyordu → sayısal alana seçilen sütun artık her zaman sayı olarak aktarılır; sonuç değişir mi: evet, yalnız sayısal alana "kategorik" türde bir sütun seçilmiş çalıştırmalarda (önceki sonuç yanlıştı). Sütun aynı anda gruplama ya da karma bir değişken listesinde de seçilmişse türü korunur.
- Kod sekmesi, bütün şablonlar: boş hücre içeren tam sayı sütunu (ör. soy kütüğünde baba/ana kimliği) "kategorik" yapıldığında etiketler `570.0` biçiminde aktarılıyor, tam sayı sütunundaki `570` ile eşleşmiyordu → tam sayı değerli etiketler `.0` eki olmadan aktarılır; sonuç değişir mi: evet, kimlik ya da kod sütunu "kategorik" türde ve boş hücreli olan çalıştırmalarda (soy kütüğü artık doğru kurulur; sıralı yanıt etiketleri `1.0|2.0` yerine `1|2` görünür).
- Kod sekmesi: rastgele sayı tohumu kullanıcı tarafından verilemiyor ve kullanılan tohum çıktıda görünmüyordu → araç çubuğuna isteğe bağlı "Tohum" alanı eklendi (boşsa her çalıştırmaya yeni tohum; doluysa o tohum); kullanılan tohum sonuç kartının künyesinde, konsolda ve betik adımında yazar; sonuç değişir mi: hayır (alan boşken davranış aynı; künyeye bir satır eklenir).
- Şablon yazanlar için yeni yardımcılar: R'de `mq_sayisal()`, `mq_kimlik()`, `mq_tohum`, `mq_genotip_say()`, `mq_genotip_notu()`; Python'da `mq.sayisal()`, `mq.kimlik()` (gömülü ve harici yorumlayıcı) → sayısal sütunu değerleriyle okuma, kimlikleri tek biçimli metne çevirme (`100000`, `1e+05` değil), genotip okurken atılan genotiplerin dökümü; sonuç değişir mi: hayır (yalnız yeni işlevler; şablonlar bunları kullanmaya başladıkça ilgili şablonun kaydına yazılır).
- Genetik şablonları (ortak genotip ayrıştırıcısı; şablonlar ona geçtikçe etkinleşir): alellerinden biri "0" olan genotipler (ör. 0/1 kodlaması) sessizce atılıyor, `-9/-9` ise alel sayılıyordu → "0" yine eksik kodudur, ama alellerinden yalnız biri eksik kodu olan genotip görülürse çalıştırma nedeni söyleyerek durur ("'0' gerçek bir alelin adıysa yeniden kodlayın"); `-9` eksik kodlarına eklendi; eksik sayılan genotiplerin dökümü çıktıya yazılır; sonuç değişir mi: evet, (a) 0/1 gibi "0" adlı alel içeren veride çalıştırma artık durur (önceki sonuç yanlıştı), (b) `-9` kodlu eksik içeren veride lokus artık sınanır; "0/0 = eksik" kullanan verilerde sonuç değişmez.
- Şablon yazanlar için: `mq_duzey_notu()` bir etken sütununun düzey sırasını ve başvuru düzeyini çıktıya yazar (iki düzeyli metin sütununda sıra alfabetiktir) → sonuç değişir mi: hayır (aktarım değişmedi; karar `karar/ortak_ikili_duzey_sirasi.md`, A).
- `plus_basvuru_kitabi/dogrulama/karsilastir.py`: bloklar birbirinden yalıtıldı (bir blok hata verirse iletisi yazılır, sonraki bloklar çalışır, sonda özet ve çıkış kodu 1). H1'deki `KeyError: 'kestirim'` hatasının nedeni kod değil çalıştırma adı çakışmasıydı: `cikti/br_ornek` hem beta_regresyon hem bos_r tarafından kullanılıyor, sonra çalışan öncekinin üzerine yazıyordu. Aynı çakışma `bp_ornek` (iki_degiskenli_probit ↔ bos_python) ve `kl_ornek` (kesin_lojistik ↔ klon_lokasyon_genetik) için de vardı. Yeni adlar: `bosr_ornek`, `bospy_ornek`, `kesl_ornek` (`kantil_ileri/karsilastir_h7.py:124`, `134`; `kesikli_secim/karsilastir_h3.py:131–138`); `br_ornek`, `bp_ornek`, `kesl_ornek`, `bosr_ornek`, `bospy_ornek` yeniden çalıştırıldı. Betik şimdi bütün blokları tamamlıyor; `src/_uretilen/dogrulama_*.md` dosyalarının hiçbiri değişmedi.
- Kitap: `src/001_ortak_duzen.md` (sayısal rol aktarımı, tohum alanı, yeni 1.5 "Dönüşüm yardımcıları", satır atıfları); genotip okuma kuralı ve iki düzeyli metin sütununda düzey sırası; tohum davranışını anlatan cümleler 22 bölümde yeni duruma getirildi (013, 037, 048, 051, 052, 053, 055, 056, 057, 063, 067–075, 078, 084, 087); 060 ve 061'de ortak dosya satır atıfları güncellendi.

## ortak_model

- Ortak model (nlminb): "Optimizasyon algoritması" olarak `nlminb` seçilince örnek veride "NA/NaN gradient evaluation" ile çöküyordu → model `optim` (BFGS) ile yeniden kurulur ve çıktıya not yazılır; sonuç değişir mi: hayır (varsayılan `optim` çalıştırmasıyla aynı sayılar; önceden hata veriyordu).
- Ortak model (sütun adında boşluk): adında boşluk olan sütun "Boylamsal model kurulamadı: unexpected symbol" ile duruyordu → sütun adları geçici güvenli adlara çevrilir, çıktıda özgün ad yazılır; sonuç değişir mi: hayır (önceden hata veriyordu).
- Ortak model (sağkalım kovaryatı): sağkalım ortak değişkeni denek içinde değişse de sessizce ilk satır kullanılıyordu → hesap aynen sürer, ama çıktıda "Uyarı: sağkalım ortak değişkeni '…' denek içinde değişiyor; ilk (en erken ölçüm) satırındaki değer temel değer olarak kullanıldı" yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Ortak model (çıktı adları): sütun adında ".t" geçiyorsa (ör. `drug.type`) çıktı adı bozuluyordu ("obstime:drugobstimeypeddI") → yalnız zaman terimi zaman adıyla değiştirilir ("obstime:drug.typeddI"); sonuç değişir mi: hayır (yalnız etiket).
- Ortak model (yakınsama satırı): "yakınsama kodu: 0/FALSE" başarı anlamı söylenmiyordu → "yakınsama: sağlandı/sağlanamadı (kod: …)" yazılır; sonuç değişir mi: hayır.
- Ortak model (eksik değer): eksik değer nedeniyle çıkarılan satır sayısı bildirilmiyordu → "Not: eksik değer nedeniyle N satır çıkarıldı" yazılır; sonuç değişir mi: hayır.
- Ortak model (form açıklaması): açıklamada ticari yazılım adı geçiyordu → bağımsız hesap diye yazıldı; sağkalım kovaryatı uyarısı ve ad kuralı eklendi (TR/EN/ES); sonuç değişir mi: hayır.

## ozel_kontrast

- Özel kontrastlar: sayı biçimli düzeyler metin sırasıyla dizilip (0, 100, 150, 50) ağırlık satırı ("-3 -1 1 3") yanlış düzeylere uygulanıyordu ve ikili farkların yönü de bu sıradaydı → sayı biçimli düzeyler sayısal sıraya (0, 50, 100, 150) konuyor; metin etiketli düzeylerde sıra alfabetik kalıyor ve çıktıya kullanılan sıra ile ağırlık satırlarının hangi sıraya uygulandığı yazılıyor; sonuç değişir mi: evet (yalnız sayı biçimli düzeyli grup sütununda; metin etiketli düzeylerde hayır).
- Özel kontrastlar: harici Python yorumlayıcısı seçiliyken şablon "Kontrast metni çözümlenemedi: No module named 'stats'" iletisiyle duruyordu → kontrast ayrıştırıcısı şablonun içine alındı, her yorumlayıcıda çalışıyor (yazım kuralları aynı); sonuç değişir mi: hayır.
- Özel kontrastlar: "Gübre_A - Kontrol = 1" yazıldığında tahmin ve güven aralığı kaydırılmış görünüyor, etiket sağ tarafı göstermiyordu → etikete "= 1" ekleniyor ve çıktıya "tahmin ve GA Lβ − c farkınındır; t ve p Lβ = c hipotezini sınar" notu yazılıyor; sonuç değişir mi: hayır (sayılar aynı; yalnız etiket ve not).
- Özel kontrastlar: tam sayı kodlu grup sütununda eksik hücre varsa düzeyler "1.0", "2.0" oluyor ve "2 - 1" çözümlenemiyordu → düzey adları "1", "2" yazılıyor; sonuç değişir mi: hayır (yalnız eskiden hata veren durum çalışıyor).

## ozel_olabilirlik

- Kullanıcı tanımlı olabilirlik: Powell ve gradyanlı çözücüler olabilirlik sırtında en büyük olmayan noktada durabiliyor, yakınsama denetlenmiyordu (gama örneğinde şekil 2,9758; doğrusu 2,8571) → kestirim her çözücüden sonra denetlenir, en büyük noktada değilse Nelder-Mead ile yeniden başlatılır ve sonuç (sağlandı / sağlanamadı) çıktıya yazılır; sonuç değişir mi: evet, Powell/BFGS/L-BFGS-B seçilip çözücü durağan noktaya varamadığında (Nelder-Mead varsayılanında hayır).
- Kullanıcı tanımlı olabilirlik: "Sayısal Hessian göreli adımı" 1e-7 ya da 0,1 girilince Wald standart hataları %11–13 küçük çıkıyordu → adım 1e-6 – 1e-2 aralığına çekilir ve not yazılır; sonuç değişir mi: evet, yalnız bu aralığın dışındaki adım girildiyse (varsayılan 1e-4'te hayır).

## panel_esbutunlesme

- Panel eşbütünleşme: "CADF gecikme sayısı" 0 seçilebiliyordu, ancak CIPS 0 gecikmeyi desteklemiyor ve on hata satırı yazıp tablo vermiyordu → alan en az 1 kabul eder (0 formda reddedilir); sonuç değişir mi: hayır (eskiden CIPS çıkmazdı).
- Panel eşbütünleşme: dönemler 2'şer yıl aralıklı kodlanınca CIPS tümüyle çöküyor, Dumitrescu-Hurlin güvenilmez uyarısıyla geçiyordu → eşit aralıklı dönemler 1, 2, … numaralanır (CIPS ve Dumitrescu-Hurlin 1 adımlı yılla aynı sonuç); bir dönem bütün birimlerde eksikse uyarı yazılır; sonuç değişir mi: evet (2'şer aralıklı dönemde CIPS artık hesaplanır; 1 adımlı dönemde hayır).
- Panel eşbütünleşme: dengesiz panelde Dumitrescu-Hurlin sekiz tekrarlı hata satırı yazıp sonuç vermiyordu → tek "dengeli panel ister; test atlandı" satırı; sonuç değişir mi: hayır.
- Panel eşbütünleşme: zaman sütunu metinse (P01, P02, …) Dumitrescu-Hurlin başarısız oluyordu → dönemler alfabetik sıralanıp 1, 2, … numaralanır ve not yazılır (P1…P17 gibi sıfırsız etiketlerde P10, P2'den önce gelir: sayısal zaman verin); sonuç değişir mi: evet (metin zamanda Dumitrescu-Hurlin artık hesaplanır; sıfır dolu etiketlerde sayısal zamanla aynı).
- Panel eşbütünleşme: bir birimde zaman içinde sabit olan değişken MG katsayısını sessizce NaN yapıyor, CIPS ve Dumitrescu-Hurlin o değişken için başarısız oluyordu → hangi değişken ve hangi birimde sabit olduğunu söyleyen uyarı satırı; sonuç değişir mi: hayır (yalnız uyarı).
- Panel eşbütünleşme: 10 birimden az panelde CIPS "interpolate" hatasıyla on kez başarısız oluyordu → tek ileti ("kritik değer tablosu en az 10 birim ister"), CIPS atlanır; sonuç değişir mi: hayır (eskiden CIPS çıkmazdı).
- Panel eşbütünleşme: havuzlanmış modelde CD istatistiğinin Pearson tanımıyla hesaplandığı belirtilmiyordu → CD tablosunun altına not; sonuç değişir mi: hayır.

## panel_sayim

- Panel sayım modelleri: sabit ve rastgele etkili kestirimlerin fark matrisi pozitif tanımlı değilken Hausman istatistiği ile serbestlik derecesi aynı ters matristen gelmiyor, istatistik negatif yönleri de içeriyor ve p ile "rastgele etkili model reddedilmez" cümlesi yazılabiliyordu → fark matrisi pozitif yarı tanımlı değilse (en küçük özdeğer < −10⁻⁶ × sabit etkili varyansın en büyük özdeğeri) test "hesaplanamadı" olarak bildiriliyor ve neden yazılıyor; yarı tanımlıysa istatistik ve serbestlik derecesi aynı sıradan genelleştirilmiş tersle hesaplanıyor, tablodaki `pozitif_tanimli` sütunu sırayı gösteriyor; sonuç değişir mi: evet (örnek veride eski "Hausman χ²(2) = −34,49" satırı yerine "hesaplanamadı"; fark matrisi tam tanımlıysa değişmez).
- Panel sayım modelleri: kestirimler özdeşken Hausman satırı sayısal gürültüden "negatif istatistik" yazıyordu → "kestirimler özdeş, test tanımsız" iletisi veriliyor; sonuç değişir mi: evet (yalnız bu uç durumda).
- Panel sayım modelleri: birim varyansı sınırda sıfır kestirildiğinde (LR = 0) sınır testinin p değeri 0,5 yazılıyordu → 1 yazılıyor; sonuç değişir mi: evet (yalnız LR ≤ 0 iken).

## panel_tobit

- Panel Tobit: "En çok yineleme" küçük verilince yineleme sınırında kalan kestirim uyarısız yazılıyordu → maxLik dönüş kodu okunuyor; kod 1, 2, 8 dışındaysa (ör. 4: yineleme sınırı) "en büyükleme yakınsamadı" uyarısı yazılıyor (rastgele etkili ve havuzlanmış model için); sonuç değişir mi: hayır (yalnız uyarı).
- Panel Tobit: LR ≤ 0 iken sınır testinin p değeri 0,5 yazılıyordu; LR negatifse uyarı yoktu → p = 1 yazılıyor, LR negatifse uyarı veriliyor; sonuç değişir mi: evet (yalnız LR ≤ 0 iken).

## parametrik_roc

- Parametrik ROC: koşullu ROC değerlerinde "45,5; 60" yazılınca virgül ayırıcı sayılıyor, 45, 5 ve 60 için (5 yaş veri aralığının çok dışında) sonuç üretiliyordu → tek ortak değişkende noktalı virgül varsa virgül ondalıktır (45,5 ve 60); noktalı virgül yoksa eski ayrım korunur ve uyarı yazılır; veri aralığı dışındaki değerler için dışdeğerleme uyarısı eklenir; sonuç değişir mi: evet (yalnız ondalık virgüllü girişte; "45, 60, 75" gibi girişler aynı).
- Parametrik ROC: belirteç yönü veriyle ters seçilince AUC 0,5'in altında çıkıyor ve uyarı yoktu; eksik değer nedeniyle çıkarılan satır sayısı yazılmıyordu → ampirik AUC < 0,5 ise "yön ters olabilir" uyarısı; çıkarılan satır sayısı notla yazılır; sonuç değişir mi: hayır (yalnız uyarı ve not).
- Parametrik ROC: bootstrap tohumu kod panelindeki çalıştırma tohumundan bağımsızdı → "Rastgele sayı tohumu" alanına 0 girilirse çalıştırmanın tohumu kullanılır (varsayılan 1 aynı kalır); sonuç değişir mi: hayır (varsayılan ayarlarda).

## parcali_regresyon

- Parçalı regresyon: kırılma noktası araması yalnız en iyi adayın komşuluğunu inceltiyor, aralık içindeki genel en küçüğü kaçırabiliyordu (bootstrap örneklerinin binde 1–6'sında farklı nokta; Bacon–Watts verisinde yüzdelik üst sınır 0,1254, doğrusu 0,1331) → arama kesin yapıldı: gözlenen değerler ve her aralıkta iki doğrunun kesişimi taranıyor (Hudson 1966); asıl verinin kırılma noktası, bootstrap örnekleri dahil; "arama ızgarası" ayarı artık etkisiz; sonuç değişir mi: evet (yalnız bootstrap SH'si ve güven aralığı uçları, çoğu veride dördüncü ondalıkta değişmez; asıl kestirim değişmedi).
- Parçalı regresyon: sıçramalı modelde c yalnız iki gözlem arasına kadar belirlenir ama bu söylenmiyordu → çıktıya aralık notu ("c, [x_k, x_k+1) aralığında belirlenir; sol uç raporlanır") yazılıyor; sonuç değişir mi: hayır.
- Parçalı regresyon: büyük "her parçada en az gözlem" değerinde ileti nedeni söylemiyordu → ileti "en_az_uc'u azaltın" öneriyor; kırılma noktası arama aralığının ucundaysa uyarı yazılıyor ve tekil matris durumunda genelleştirilmiş ters kullanılıyor; sonuç değişir mi: hayır.

## permanova_mantel

- Tek gözlemli grup kabul ediliyor, PERMDISP'te sıfır uzaklıkla F şişiyordu (örnekte F 3,87 → 10,59) ve "her grupta birden fazla gözlem gerekli" iletisi yanıltıcıydı → tek gözlemli grup varsa uyarı yazılır, PERMANOVA hesaplanır, PERMDISP yapılmaz ve nedeni yazılır (yanlılık düzeltmesi seçiliyse eski durma iletisi); her grup tek gözlemliyse ileti düzeltildi; sonuç değişir mi: evet (yalnız PERMDISP açıkken ve tek gözlemli grup varken: yapay F yerine açıklama); PERMANOVA hayır.
- Her grup ikişer gözlemliyken PERMDISP F'si anlamsız büyüklükte (10^30) ve "p = 0,0000" çıkıyordu → grup içi yayılım sıfırsa F tanımsız sayılır, PERMDISP atlanır ve açık ileti yazılır; ayrıca yuvarlanınca 0,0000 olan p değerleri "p < 0,0001" yazılır; sonuç değişir mi: evet (yalnız bu uç durumda: anlamsız F yerine ileti); öteki sonuçlar hayır.
- Sayı kodlu grup etiketleri metin sırasıyla yazılıyordu ("10" "2"den önce) → tümü sayıysa sayısal sırayla, değilse alfabetik sırayla yazılır; sonuç değişir mi: hayır (yalnız tablo ve gösterge sırası).
- "Dışbükey zarf + merkezler" grafiğinde göstergedeki çizgi rengi nokta renginden farklıydı → renk etkeni seçilmediyse zarf çizgisi ve gösterge noktalarla aynı renkte; sonuç değişir mi: hayır (yalnız grafik).
- Eksik değerli satırlar bildirilmeden atılıyordu → "N satır atıldı (satırlar: …)" notu yazılır; sonuç değişir mi: hayır (yalnız bildirim).

## permutasyon_ozel

- Tabakalı / bloklu permütasyon testi: blok verildiğinde ortalama ve medyan farkı blok etkisinden arındırılmıyor, iki yönlü p sıfır çevresinde simetri varsayıyordu (dengesiz tasarımda gerçek fark varken p = 1,0000) → fark istatistikleri blok ortalamalarından arındırılmış yanıttan hesaplanır, çıktı etiketi bunu yazar; iki yönlü p |T*| ≥ |T|; sonuç değişir mi: evet (yalnız bloklu tasarımda fark istatistikleri; dengeli tasarımda gözlenen fark aynı kalır, F ve bloksuz testler değişmez).
- Tabakalı / bloklu permütasyon testi: "1. grup" kod noktası sırasıyla belirleniyordu (sayı kodlu "10" < "2") → MerQuR+ düzey sırası (kullanıcı sırası, sayısal, ilk görülme); sonuç değişir mi: evet (fark istatistiklerinin işareti ve tek yönlü testin yönü, düzey sırası değişen verilerde).
- Tabakalı / bloklu permütasyon testi: her blok tek gruptan oluşurken fark istatistikleri uyarısız p = 1 veriyordu → F gibi "Grup blokla iç içe" iletisiyle durur; sonuç değişir mi: evet (o durumda p = 1 yerine ileti).

## populasyon_yapisi

- Popülasyon yapısı: ölçekleme açık ve "DAPC'de tutulacak TB sayısı" 0 (çapraz doğrulama) iken yeniden atama, ayırma özdeğerleri ve sonsal tablolar ölçeksiz kurulmuş DAPC'den geliyordu (örnekte ilk özdeğer 48,130; ölçekli 48,000) → DAPC seçilen TB sayısıyla ölçekli yeniden kurulur; sonuç değişir mi: evet (yalnız ölçekleme açıkken ve TB sayısı 0 iken).
- Popülasyon yapısı: genotipleri ortak ayrıştırıcı okur; `-9` eksik kodudur (`-9/-9` verisinde 114 alel sütunu ve dirsek K = 3 çıkıyordu; şimdi boş hücreyle özdeş: 109 sütun, K = 6), alel adı `0` olan `0/143` genotipleri sessizce eksik sayılmak yerine çalıştırmayı durdurur, eksik sayılan genotiplerin dökümü yazılır; konsoldaki "eksik genotip oranı" artık birey × lokus genotiplerinin oranıdır (örnekte %0,9 yerine %2,1; hücre oranı değil); sonuç değişir mi: evet (yalnız `-9` ya da `0/x` içeren veride; oran yalnız yazı).
- Popülasyon yapısı: k-ortalamalar 10 rastgele başlangıçla çalıştırılıyor, BIC eğrisi yerel çözümlerden oluşuyor ve en küçük BIC K'sı tohuma ve satır sırasına göre 8–10 arasında değişiyordu → varsayılan başlangıç sayısı 100 (örnek veride 200 başlangıçlı en iyi değerlerle aynı eğri; dört çalıştırmada dirsek 6, en küçük BIC 8, iyi uyum 5), BIC tablosunun başlığı başlangıç sayısını yazar; sonuç değişir mi: evet (varsayılan ayarda BIC eğrisi ve en küçük BIC'in K'sı: örnekte 10 → 8; süre birkaç saniye artar).
- Popülasyon yapısı: seçilen K'nın BIC'i K = 1'inkinden küçük değilken ek not yazılmıyordu → "BIC kümelenmeyi desteklemiyor" notu; çapraz doğrulama tablosunun başlığı sabit "eğitim %90" yazıyordu → uygulanan oranı yazar (en küçük grup nedeniyle paket oranı indirirse not); şekil lejantında "Koşular arası aralık" → "Çalıştırmalar arası aralık"; sonuç değişir mi: hayır (yalnız not ve metin).
- Eksik genotip kodları (yeni alan): Popülasyon yapısı: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).
- Popülasyon yapısı: birey kimliği yalnız `birey` adlı sütundan okunuyordu → forma "Birey kimliği sütunu" alanı eklendi; boşsa bugünkü kural (`birey` adlı sütun, yoksa satır numarası), doluysa seçilen sütun kimlik olur (sayısal kimlikte 100000 gibi değerler "1e+05" yazılmaz); sonuç değişir mi: hayır (alan boşken).

## qpcr_goreli

- qPCR göreli nicelendirme: seyreltme miktarı Veri sekmesinde "kategorik" olduğunda düzey sıra numaraları kullanılıyordu (eğim −7,894 yerine −3,388; Pfaffl 2,075 yerine 5,478) → miktar her zaman değer olarak okunur (`mq_sayisal`); sonuç değişir mi: evet, yalnız miktar sütunu kategorik türdeyken (ortak katman da bunu kapatır).
- qPCR göreli nicelendirme: kontrol grubu boşken verideki ilk grup kontrol oluyor, yön satır sırasına bağlıydı → grup sırası Veri sekmesindeki düzey sırası, yoksa sayısal sıra, yoksa ilk görülme olur; seçilen kontrol ve sıra kuralı konsola yazılır; sonuç değişir mi: evet, yalnız kontrol boşken ve düzey sırası/sayısal sıra ilk görülmeden farklıyken.
- qPCR göreli nicelendirme: çoğalma verimi 0,97 gibi kesir yazılınca (0 < E < 1) Pfaffl oranı tersine dönüyordu (0,936 yerine 5,48) → çalıştırma durur ve "1 + kesir" yazılmasını söyler; E > 2,2 uyarı verir; sonuç değişir mi: evet (artık durur).
- qPCR göreli nicelendirme: hedef ve referans farklı örnek kimliği taşıyorsa t tabanlı aralık ve testler sessizce üretilmiyordu; kısmen eşleşmede ΔΔCt tablosu ile aralık farklı örneklere dayanıyordu; tek örnekli grup ve biyolojik birim varken örneklerin bağımsız sayılması da bildirilmiyordu → her biri için not/uyarı; sonuç değişir mi: hayır (yalnız bildirim).
- [3.8.0] qPCR göreli nicelendirme: yeni seçenek "ΔΔCt'yi yalnız eşleşen örneklerle hesapla" (varsayılan kapalı; ayrı tüp): açıkken ΔΔCt tablosu yalnız iki genle de ölçülmüş örneklerden kurulur; ayrıca karma model tablosuna sona `sd` ve `p` (Satterthwaite; `lmerTest` varsa) eklendi; sonuç değişir mi: hayır (varsayılanda ΔΔCt aynı; karma tabloya yalnız sütun eklendi; test `test_duzeltme_380_qpcr_goreli.py`).

## qst_fst

- QST–FST: REML'de gözlenen QST = 0 ve FST sabitken nötral tekrarların çoğu gözlenene eşit çıkıyor, ama hiçbir kuyruğa sayılmadığından p = 0 ve "tekdüzeleştirici seçilim" yazılıyordu → eşit tekrarlar (tolerans 1e-8) iki kuyruğa da sayılır ve sayıları notla yazılır; sonuç değişir mi: evet, yalnız REML + sınırdaki popülasyon varyansı + sabit FST (altı popülasyonlu denemede p_iki 0 → 0,45, karar "nötral beklentiden ayrılmıyor"); ANOVA kipinde hayır.
- QST–FST: `-9/-9` alel sayılıyor ve `0/3` gibi "0" adlı alelli genotipler sessizce atılıyordu → ortak genotip okuma kuralı: `-9` eksiktir (FST değişmez), yarı eksik genotip (`0/3`) nedenini söyleyerek durur, eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet, yalnız bu kodlamalı verilerde (örnek: FST 0,09061 → 0,09832).
- QST–FST: varsayılan B = 1000 ile sınırdaki kararlar tohuma bağlıdır ve çıktı bunu söylemiyordu → düzeltilmiş p'nin ±2 Monte Carlo SH aralığı α'yı kesen özellikler için "karar Monte Carlo hatasına duyarlı, B'yi artırın" uyarısı yazılır (varsayılan B değişmedi); sonuç değişir mi: hayır (yalnız uyarı).
- QST–FST: aileler üzerinden yeniden örnekleme aralığının yukarı yanlılığı çıktıda belirtilmiyordu → aralık tablosunun altına yanlılık notu; sonuç değişir mi: hayır.
- QST–FST: açıklama metninde iki cümle arasında nokta yoktu ve eksik genotip kuralı yazılmamıştı → düzeltildi (TR/EN/ES); sonuç değişir mi: hayır.
- Eksik genotip kodları (yeni alan): QST–FST: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).
- QST–FST: "Rastgele sayı tohumu" alanı (varsayılan 2009) çalıştırma tohumunu eziyordu → alana 0 (ya da boş) yazılırsa kod panelindeki çalıştırma tohumu kullanılır (o da yoksa 2009); alan varsayılanı 2009 aynı kaldığından sonuç değişir mi: hayır.
- QST–FST: aileler üzerinden yeniden örneklemenin aralığı yalnız yüzdelik veriliyordu (yinelenen ailenin yarattığı yukarı yanlılık düzeltilmiyordu) → forma "Aile yeniden örnekleme aralığı yöntemi" eklendi: yüzdelik (varsayılan, bugünkü) ya da temel/pivot (2·QST − yüzdelikler, [0, 1] ile sınırlı; yüzdelik sınırlar tablo sonunda ek sütunlarda); sonuç değişir mi: hayır (varsayılanda).

## qtl_haritalama

- QTL haritalama: en yüksek LOD birden çok konumda birebir aynıysa (ör. genotipleri aynı markerler, marker regresyonu) aralığın üst ucu ikinci tepe sanılıyordu (4,4–10,9 cM yerine 4,4–35,01 cM) → alt ve üst uç `lodint` çıktısının ilk ve son satırından, tepe özet tablosundan okunur; sonuç değişir mi: evet, yalnız eşit tepeli veride (öteki durumlarda aynı).
- QTL haritalama: X kromozomu için ayrı permütasyon yapılan veride `p_genom` genom boyuna çevrilmiyor, X'te eşik altında kalan tepe 0,386 gibi anlamlıya yakın bir p taşıyordu → `p_genom = 1 − (1 − p_tür)^(L/L_tür)` (R/qtl'nin `summary.scanone` dönüşümü); sonuç değişir mi: evet, yalnız X kromozomlu veride (otozomlarda küçük, X'te büyük fark; `anlamli` sütunu değişmez).
- QTL haritalama: "Genotip kodları" etiketi ve hata iletisi dördüncü ve beşinci kodu ters tanımlıyordu → "AA, AB, BB, BB değil, AA değil" (R/qtl: D = BB değil, C = AA değil); sonuç değişir mi: hayır (yalnız metin; varsayılan `A,H,B,D,C` zaten doğruydu).
- QTL haritalama: aynı konumda iki marker varsa çalıştırma çevrilmemiş bir hatayla tablosuz bitiyordu → konumlar R/qtl'nin önerisiyle (`jittermap`, 10⁻⁶ cM) ayrılır, kaç marker için yapıldığı yazılır, aralık tablosunda konumlar üç ondalığa yuvarlanır; sonuç değişir mi: hayır (eskiden hata; yalnız bu durumda rastgele sapma tohuma bağlıdır).
- QTL haritalama: fenotipi eksik bireyler iletisiz çıkarılıyordu → kaç bireyin çıkarıldığı ve kaç bireyin çözümlendiği yazılır; QTL etkileri tablosunun başlığı R/qtl satır adlarını (a/d, .2/.3) açıklar; paket kurulu değilse anlaşılır ileti verir; sonuç değişir mi: hayır.

## r_emmeans_kontrast

- R emmeans kontrast: kullanıcıya görünen metinde ticari ad, 'koşu' ya da tek satırlık düşük bulgu bulunmadı; değişiklik yok.

## r_glmmtmb_nb

- Sıfır şişirilmiş ve engel karma sayım modelleri: sıfır bileşeni tablosundaki `sifir_olasiligi` sütunu her satırda katsayının tek başına ters lojitini yazıyordu; kesişim dışındaki satırlarda bu bir olasılık değildir (ör. madencilik satırında 0,917; o düzeyde gerçek P(Y = 0) = 0,853) → sütun yalnız kesişim satırında doldurulur, öteki satırlar boştur; bir not satırı kapsamı açıklar; sonuç değişir mi: evet (yalnız sıfır bileşeninin yordayıcıları "koşullu modeldeki sabit etkiler" iken, bu sütunun kesişim dışındaki hücreleri; katsayılar değişmez).
- Aynı şablon: sıfır şişirme olasılığı sıfıra kestirildiğinde (örnek ayarda da böyle: kesişim −15,9; SH 3103) uyarı yoktu → "sınır çözümü; model sıfır bileşensiz modele eşdeğerdir" notu yazılıyor; sonuç değişir mi: hayır.
- Aynı şablon: yanıt sütunu sayısal değilse R'nin ham hatası çıkıyordu → "Yanıt sayısal olmalı." iletisiyle duruyor; sonuç değişir mi: hayır.
- Aynı şablon: etken sütunlarının düzey sırası ve başvuru (ilk) düzeyi çıktıya yazılıyor (iki düzeyli metin sütununda sıra alfabetiktir); sonuç değişir mi: hayır (aktarım değişmedi).
- Aynı şablon: form açıklamasından başka yazılımın adı çıkarıldı; sonuç değişir mi: hayır.

## regresyon_sureksizligi

- Regresyon süreksizliği (bulanık tasarım): iki düzeyli metin tedavi sütununda hangi düzeyin tedavi sayıldığı Veri sekmesi sırasına bağlıydı ve çıktıda yazmıyordu (evet/hayir etiketinde işaret tersine çıkabiliyordu) → kural sütun türünden bağımsız: sayıya benzeyen düzeylerde büyük, değilse alfabetik son düzey; çıktı tedavi sayılan düzeyi yazar; yeni "Tedavi sayılan düzey" alanı (boş = kural) ile seçilebilir; sonuç değişir mi: evet (yalnız "kategorik" türde sayısal tedavide Veri sekmesi sırası kuraldan farklıysa; 0/1 sayısal tedavide ve tedavi/kontrol etiketlerinde aynı).
- Regresyon süreksizliği (bulanık tasarım): ilk aşama sıçraması (oranın paydası) gösterilmiyordu → aynı bant genişlikleriyle ilk aşama tablosu eklenir; sıçrama negatifse ya da sağlam GA'sı 0'ı kapsıyorsa uyarı; sonuç değişir mi: hayır (ek tablo ve uyarı).
- Regresyon süreksizliği: eksik değer nedeniyle atılan satırlar sessizdi → atılan satır sayısı ve sütun başına eksik sayısı yazılır; sonuç değişir mi: hayır.
- Regresyon süreksizliği (bulanık tasarım): 1/2 kodlu sayısal tedavide bant genişliği kodlamaya bağlıydı → iki değerli sayısal tedavi 0/1'e çevrilir; sonuç değişir mi: evet (yalnız 0/1 dışı iki değerli sayısal tedavide; ör. 1/2 kodlu veride 8,2967 yerine 8,2889).
- Regresyon süreksizliği: yoğunluk testi tablosunun başlığı hangi örneklemin (atama değişkeni dolu satırlar, n) kullanıldığını yazar; rdrobust'ın kütle noktası uyarıları Türkçe arayüzde Türkçe; eğim kırılması (turev = 1) "RK" olarak etiketlenir ve p = 1 iken p = 2 uyarısı yazılır; sonuç değişir mi: hayır.

## ri_clpm

- RI-CLPM / CLPM / LCS: X ve Y'nin ölçekleri çok farklı olduğunda (varyans oranı 1000'i aşınca) lavaan sayısal olarak bozuluyor, "yakınsadı" denen ama yanlış çözüm (ör. negatif rastgele kesişim varyansı, ölçeğe bağlı ki-kare) yazılıyor, yalnız bir uyarı çıkıyordu → X ve Y değişken başına ortak bir bölenle yeniden ölçeklenmiş veride kestirilir; kestirim, SH, GA, log-olabilirlik ve AIC/BIC özgün birime geri çevrilir ve bu not yazılır (uyum indeksleri ve standartlaştırılmış çözüm zaten ölçekten bağımsızdır). Aynı değişkenin dalgaları arasında 10 000 katından fazla varyans farkı (ör. yalnız bir dalgada birim hatası) varsa anlaşılır iletiyle durur; sonuç değişir mi: evet (yalnız değişkenler arası ölçek farkı büyükken; örnek veride X ×1000: ki-kare 82,99 ve negatif varyans → 16,178; Y→X etkisi ×1000, X→Y etkisi /1000 olarak doğru çıkar); ölçekler benzerken aynı.
- MLR seçilince CFI, TLI ve RMSEA ML ile özdeş (ölçeklenmemiş) yazılıyordu → MLR'de bu üç indeks sağlam (robust) sürümleriyle yazılır, tablonun altına not eklenir (SRMR ölçeklenmez); sonuç değişir mi: evet (yalnız MLR; örnek veride CFI 0,99747 → 0,99743, RMSEA 0,0447 → 0,0450); ML'de aynı.
- Dalga sırası seçim sırasından alınıyor, ters seçilen dalgalar (x4, x3, x2, x1) aynı uyumla farklı katsayı veriyor ve hata verilmiyordu → dalga sırası ("1 = x1 / y1; …") çıktıya yazılır; sütun adları sayıyla bitip artan değilse uyarı verilir; sonuç değişir mi: hayır (yalnız bildirim).
- Satorra-Bentler (2010) fark testi seçilince bu veride negatif Δχ² (−76,80; p = 1) çıkıyor ve uyarı kullanıcıyı zaten seçili olan 2010 yönteminden önce 2001 yöntemine yönlendirmiyordu → negatif ya da tanımsız çıkarsa fark testi Satorra-Bentler (2001) ile yeniden hesaplanır ve bu not yazılır (2001 de negatifse uyarı AIC/BIC farkını önerir); sonuç değişir mi: evet (yalnız MLR + 2010 seçili ve negatif sonuçta; örnek veride Δχ² −76,80 → 118,40, p 1,7·10⁻²⁵).
- Gizil değişim puanı modeli yakınsamadığında ileti çaresiz kalıyordu → iletiye kısıt önerisi (gecikmeli etkileri ya da ölçüm hatası varyanslarını eşit kısıtlamak) eklendi; yörünge şeklinde gösterilen kişilerin seçimi artık kod panelindeki çalıştırma tohumunu kullanır (tohum yoksa eskisi gibi 1); sonuç değişir mi: hayır (yalnız ileti ve şekildeki kişi seçimi).

## rmst_regresyon

- RMST regresyonu (etkin düzey): "Etkin grup düzeyi" boşken ikinci düzey metin sıralamasıyla seçiliyordu (grup kodları 2 ve 10 ise etkin grup 2; fark ve oranlar ters) → düzeylerin tümü sayıysa sayısal sıra, değilse harf sırası kullanılır ve çıktının başında "Etkin düzey: '10'; başvuru düzeyi: '2' (…)" yazılır; sonuç değişir mi: evet, yalnız grup kodları farklı basamak sayılı sayılar olduğunda (ör. 2 ve 10: fark +0,137 yerine −0,137); 0/1 kodlu, tek basamaklı ya da metin düzeyli veride hayır.
- RMST regresyonu (eşdoğrusal kovaryat): tam eşdoğrusal kovaryatta "computationally singular" ya da "arguments imply differing number of rows" ham hataları çıkıyordu → eşdoğrusal sütunun adı verilerek "Kovaryatlar tam eşdoğrusal: … çıkarın" iletisiyle durur (her iki yöntemde); sonuç değişir mi: hayır (önceden hata veriyordu).
- RMST regresyonu (sıfır süre): süresi 0 olan gözlemle IPCW kovaryat düzeltmesi "invalid 'times' argument" ile düşüyordu → "Süresi 0 olan N gözlem var; IPCW … 'Sözde değer regresyonu' yöntemini seçin ya da çıkarın" iletisi; sonuç değişir mi: hayır (hata iletisi).
- RMST regresyonu (τ yazımı): τ konsolda "12,38", yorumda "12,3833" yazılıyordu → her yerde dört anlamlı basamak, "τ = 10 süresine kadar"; sonuç değişir mi: hayır (yalnız metin).
- RMST regresyonu (yorum): yorum cümlesi kovaryat seçili olsa da yalnız düzeltilmemiş farkı yorumluyordu → "Yorum (düzeltilmemiş fark)" ve kovaryat seçiliyse ikinci cümle "Yorum (kovaryatlarla düzeltilmiş fark)" yazılır; şekil başlığında GA "noktasal" olarak adlandırılır; sonuç değişir mi: hayır (tablolar aynı, yalnız metin).
- RMST regresyonu (form açıklaması): açıklamada ticari yazılım adları geçiyordu → kaldırıldı; etkin düzey etiketi yeni kuralı söyler (TR/EN/ES); sonuç değişir mi: hayır.

## rnaseq_mikrobiyom

- RNA-seq ve mikrobiyom: ALDEx2 `diff.btw` ve `effect` işareti referans gruptan bağımsız, ANCOM-BC2 ile ters çıkabiliyordu → iki sütun her zaman "karşıt − referans" yönünde; sonuç değişir mi: evet, iki sütunun işareti (p değerleri ve anlamlı sayılar hayır).
- RNA-seq ve mikrobiyom: grup adı rakamla başlayınca (10mg, 24h) kat değişimi küçültmesi sessizce başka yönteme düşüyor, not "apeglm" diyordu → katsayı adı `resultsNames` içinden bulunur ve apeglm uygulanır; bulunamazsa not gerçek yöntemi yazar; sonuç değişir mi: evet, yalnız rakamla başlayan grup adlarında `log2FC_kucultulmus` (örnekte −3,968 → −4,583).
- Mikrobiyom: ALDEx2 Wilcoxon beklenen p'si tümüyle ayrışan taksonlarda 0 çıkıyordu → her Monte Carlo örneğinde kesin iki yanlı p yeniden hesaplanır (altışar örnekte en küçük 0,0022); sonuç değişir mi: evet, `wi.ep` ve `wi.eBH` (özetteki Wilcoxon anlamlı sayısı artabilir: örnekte 6 → 9).
- Mikrobiyom: başlık ve özet etiketleri seçilen düzeltmeyi/testi yansıtmıyor, özet yalnız ilk karşıtlığı sayıyordu → etiketler seçimi yazar; özette ANCOM-BC2 her karşıtlık için ayrı satırdır; YKO ondalığı arayüz diliyle yazılır; sonuç değişir mi: hayır (etiket ve özet satırları).
- RNA-seq ve mikrobiyom: referans grup boşken ilk grubun referans alındığı yalnız ilk satırda yazılıyordu → ayrı bir not (davranış aynı); ALDEx2 çalıştırma tohumuyla çalışır (tohum künyede); sonuç değişir mi: ALDEx2 Monte Carlo değerleri tohuma bağlıdır (sabit tohum 1 yerine kod panelindeki tohum).
- [3.8.0] RNA-seq ve mikrobiyom: yeni seçenek "Karşıtlık başına ayrıntı tablosu" (varsayılan kapalı; RNA-seq, 3+ grup): her karşıtlık için ayrı tablo ve sona `q_padj_kumesi` (q, padj ile aynı gen kümesinde); "Referans grup" alanı zaten vardı; sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_rnaseq_mikrobiyom.py`).

## sac_slx_gns

- Ağırlık stili B, C ya da S seçilince Lagrange çarpanı testleri geçersiz olduğu hâlde başlıkları değişmeden yazılıyordu (uyarı yalnız hata akışındaydı) → testler aynı komşulukların satır standartlaşmış (W) ağırlığıyla hesaplanır, çıktıda ve tablo başlığında açıkça yazılır; modeller seçilen stille kestirilmeye devam eder; sonuç değişir mi: evet (stil W dışındayken LM tablosu; örnek veride stil B: LM-hata 6,80 → 5,72, LM-gecikme 13,79 → 9,36); stil W'de aynı.
- Ayrıntılı model ve Moran I satırı: Matrix ya da Chebyshev yöntemi satır standartlaşmış W ile hiçbir gecikmeli/hatalı modeli kuramıyordu ("Matrix method requires symmetric weights"); model kurulamayınca Moran I satırı yine "seçilen model" diyordu → komşuluğun simetri bilgisi doğru işaretlenir, Matrix ve Chebyshev SAR, SEM, SAC, SDM, SDEM, GNS'yi kurar (örnekte ρ özdeğer yöntemiyle aynı: Matrix 0,4310, Chebyshev 0,4311); model yine de kurulamazsa Moran I satırı "EKK (OLS) modelinin" artıkları olduğunu yazar; sonuç değişir mi: evet (Matrix/Chebyshev seçiliyken NA yerine sonuç gelir); öteki yöntemlerde hayır.
- Sayısal birim kimliklerinde 100000 gibi yuvarlak değerler "1e+05" oluyor, komşu listesiyle eşleşmeyip "veride olmayan kimlikler var" hatasına yol açıyordu → kimlikler `mq_kimlik` ile çevrilir; sonuç değişir mi: hayır (önceden hata veren veri artık çalışır). Sütunlar sayıya `mq_sayisal` ile çevrilir (sayı olmayan değerde açık ileti).
- Uzaklık bandı bağlantısız alt çizgeler oluşturduğunda uyarı yalnız hata akışındaydı; koordinat tabanlı komşulukta eksik değerli satırlar sayısı yazılmadan atılıyordu → ayrı alt çizge sayısı ve atılan satır sayısı çıktıya yazılır; sonuç değişir mi: hayır (yalnız bildirim).

## saglam_anova_wrs2

- Sağlam ANOVA (WRS2), iki yönlü kontrastlar (mcp2atm): tablo başlığı p değerlerinin çoklu karşılaştırma düzeltmeli olduğunu söylüyordu; paket p'yi düzeltmez (GA eşzamanlıdır) → başlık "p düzeltilmemiş" der, her etkinin kontrast ailesi içinde Hochberg düzeltmeli ayrı bir `p_hochberg` sütunu eklendi; mevcut sütunlar değişmedi; sonuç değişir mi: hayır (yalnız başlık ve yeni sütun).
- Sağlam ANOVA (WRS2), tekrarlı ölçüm ikili karşılaştırmaları (rmmcp): "anlamlı" sütunu Hochberg yordamının kararını vermiyordu (her p kendi kritik düzeyiyle tek tek karşılaştırılıyordu) → karar Hochberg yukarı adım yordamıyla verilir; sonuç değişir mi: evet, küçük bir p kendi sıkı eşiğini geçemese de daha büyük bir p kendi eşiğini geçtiğinde (örnekte 3 karşılaştırmadan biri "anlamlı değil" iken artık hepsi anlamlı).
- Sağlam ANOVA (WRS2): lincon başlığı GA'yı Hochberg sayıyordu (GA eşzamanlı SMM), rmmcp başlığı "kırpılmış ortalama farkı" diyordu (fark puanlarının kırpılmış ortalamasıdır; GA Bonferroni) ve ξ satırı n'ler eşit değilken nokta kestiriminin de rastgele alt örneklemeye dayandığını söylemiyordu → başlıklar paketin gerçek davranışını yazar; sonuç değişir mi: hayır.

## saglam_glm

- Sağlam GLM: "Şapka değeri √(1 − h)" seçeneğinin etiketi ve açıklaması paketin uyguladığı ağırlıkla (robustbase gerçekte (1 − h)² uygular; paketin belgesi √ der) uyuşmuyordu → etiket ve açıklama "(1 − h)²" olarak düzeltildi; sonuç değişir mi: hayır (hesap değişmedi, yalnız yazı).

## saglam_karma

- Sağlam karma model: özet satırı her durumda "Huber k = …" yazıyordu (yüksek çöküş noktalı ayarda artık işlevi çift kareli olduğu hâlde); k değiştirilip RSEa ya da RSEhb ayarı da seçilince ayarın artık işlevleri uyarısız devre dışı kalıyordu → özet satırı ayarı ve gerçekten kullanılan artık ağırlık işlevini yazar; iki seçenek birlikte verilince açık uyarı çıkar; form açıklaması durumu anlatır; sonuç değişir mi: hayır (hesap aynı; karar 2026-11-17: bugünkü hesap + uyarı kalır, `karar/saglam_karma.md`).
- Sağlam karma model: rastgele eğim değişkeni sabit etkilerde yoksa model eğimin ortalamasını sıfır varsayıyor (örnek: eğim varyansı 41,7 yerine 155,2), uyarı verilmiyordu → uyarı yazılır; sonuç değişir mi: hayır.
- Sağlam karma model: kod yorumu standart hatanın tanımını eksik veriyordu → yorum düzeltildi (σ²·unsc(): etkinlik düzeltmeli sağlam kovaryans); yanıt ve rastgele eğim sütunu her zaman değerleriyle okunur; sonuç değişir mi: hayır.

## secim_sonrasi_cikarim

- Seçim sonrası çıkarım: açıklama metni "SH'ler değişen varyansa dayanıklıdır" diyordu, oysa kısmi çıkarmanın standart hatası hdm'nin klasik EKK varyansı → metin doğrusunu söyler; yeni "Kısmi çıkarmada dayanıklı SH (HC1)" seçeneği eklendi (varsayılan kapalı = eski sonuç; açıkken örnekte SH 0,013936 → 0,015391, kestirim aynı); sonuç değişir mi: hayır (varsayılan aynı; yalnız seçilirse).
- Seçim sonrası çıkarım: koşullu çıkarım tablosunda p değeri tek yönlü, güven aralığı iki yönlüydü ve başlık bunu söylemiyordu → başlık p'nin türünü yazar; yeni "Koşullu p değeri" seçeneği (varsayılan tek yönlü = eski sonuç, "iki yönlü" seçilirse 0,000094 → 0,000189); sonuç değişir mi: hayır (varsayılan aynı; yalnız seçilirse).
- Seçim sonrası çıkarım: "Lasso sonrası EKK" kapalıyken satır adları yine "lasso sonrası" diyordu → adlar "Kısmi çıkarma (lasso)" ve "Çift seçim (lasso)" olur; sonuç değişir mi: hayır (yalnız etiket).
- Seçim sonrası çıkarım: kontrol sayısı gözlem sayısına yakın ya da büyükken EKK karşılaştırma satırı nedensiz yoktu → neden notla yazılır; sonuç değişir mi: hayır.
- Seçim sonrası çıkarım: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve değişken başına eksik sayısı yazılır; sonuç değişir mi: hayır.
- Seçim sonrası çıkarım: birden çok hedefte öteki hedeflerin kontrol sayıldığı belirtilmiyordu → not eklendi; sonuç değişir mi: hayır.
- Seçim sonrası çıkarım: sabit ve tek düzeyli kontroller iletisiz atılıyor, kategorik kontrollerin başvuru düzeyi söylenmiyordu → atılanlar ve her kategorik kontrolün başvuru düzeyi yazılır; düzey sırası ortak kurala bağlandı (etken sütunda Veri sekmesi sırası; sayı etiketlerinde sayısal sıra; aksi hâlde harf sırası); sonuç değişir mi: yalnız bazı durumlarda (1, 2, 10 gibi sayı etiketli metin sütunlarında başvuru düzeyi harf sırasından sayısal sıraya geçer; karışık büyük/küçük harfli metin etiketlerinde sıra artık kod noktası sırasıdır, büyük harfler önce).
- Seçim sonrası çıkarım: grafik eksen etiketi α ne olursa olsun "%95 GA" yazıyordu → α'ya göre yazılır; sonuç değişir mi: hayır.

## seleksiyon_kazanci

- Seleksiyon kazancı: aile varyansı sınırda (≈ 0) kestirilen bir ek özellikte genetik korelasyon, ilişkili tepki ve Smith–Hazel indeksi ~10⁻¹³'lük bir varyansa bölünerek anlamsız sayılarla (ör. r_A = −934119, etkinlik −3·10¹²) hesaplanıyordu → bu özelliğin aile (ko)varyansı tam sıfıra alınır; r_A, ilişkili tepki ve etkinlik o özellik için boş (hesaplanamadı) yazılır, seleksiyon indeksi bu özellik olmadan kalan özelliklerle kurulur (ekonomik ağırlık ve istenen kazanç listelerinde o özelliğin payı atılır); uyarı hangi özelliğin dışarıda kaldığını yazar; ana özellik sınırdaysa ya da ikiden az özellik kalırsa indeks hesaplanmaz ve not yazılır; sonuç değişir mi: evet (yalnız sınırda özellik içeren çalıştırmalarda; öteki sayılar değişmez, aile seleksiyonu kazancı 20,471 olarak kalır).
- Seleksiyon kazancı: olabilirlik oranı 0 iken aile varyansı testinin p'si 0,5 yazılıyordu → 1 yazılır (½χ²₀ + ½χ²₁ karışımında Λ = 0 için kuyruk olasılığı 1'dir); sonuç değişir mi: yalnız Λ = 0 olan özelliklerde p 0,5 → 1 (uyarı eşiğini etkilemez).
- Seleksiyon kazancı: açıklamadaki Lush oranı formülü yalnız blok × aile bileşeni yokken kodun değeriyle aynıydı → açıklama bunu söyler; sonuç değişir mi: hayır (yalnız metin).
- [3.8.0] Seleksiyon kazancı: veri kipinde "Seleksiyon yoğunluğu: sonsuz/kesin" seçimi zaten vardı (varsayılan sonsuz); alanın açıklamasına deneme verisinde kesin seçimin etkisi eklendi (aile içi i 1,3998 → 1,3318; iki aşamalı kazanç 40,214 → 38,91); sonuç değişir mi: hayır (yalnız açıklama; test `test_duzeltme_380_seleksiyon_kazanci.py`).

## sentetik_kontrol

- Sentetik kontrol: V optimizasyonu çok yerel en iyilidir ve seçilen yönteme göre etki değişiyordu, bu söylenmiyordu → kullanılan yöntem ve uyarı notu yazılır; yeni "V optimizasyon yöntemlerini karşılaştır" seçeneği Nelder-Mead, BFGS ve nlminb sonuçlarını yan yana verir (varsayılan kapalı); sonuç değişir mi: hayır.
- Sentetik kontrol: plasebo havuzu tedavi edilen birimi de içeriyordu ve çıkarma seçeneği yoktu → yeni "Plasebo havuzu" seçeneği (varsayılan = tedavi edilen dahil = eski sonuç; "çıkarılır" ile yalnız tedavi edilmemiş birimler); sonuç değişir mi: hayır (seçenek değişirse oranlar değişir, örnekte Katalonya 7,44 → 3,95).
- Sentetik kontrol: öngörücü pencerede bir birimde tümüyle eksikse hata iletisi yanlış birim numarasını gösteriyordu → ileti birim adını ve öngörücüyü yazar (TR/EN); sonuç değişir mi: hayır.
- Sentetik kontrol: plasebo yeniden kestirimi hata veren donör sessizce düşüyordu → düşürülen sayı yazılır; sonuç değişir mi: hayır.

## sicak_deste

- Sıcak deste ataması: "çok değişkenli" havuzla rastgele sıcak deste her zaman "incorrect number of probabilities" hatasıyla duruyordu → eksik örüntüsü aynı olan alıcılar, aynı hücrede o değişkenlerin hepsi gözlenmiş bir bağışçıdan ortak değerleri alır (şablonun kendi uygulaması); bağışçısı olmayan örüntü eksik kalır ve özet tablosunda görünür; sonuç değişir mi: hayır (önceden bu seçenek hiç çalışmıyordu; diğer havuzlar aynı).
- Sıcak deste ataması: FEFI'de sayı değerli düzeyler metin sırasıyla (10, 11, 12, 5, …) dizilirdi → bütün düzeyleri sayı olan değişkende sayıya göre dizilir (yalnız tablo sırası); FHDI "hücre azaltma (cell)" hatası "knn" seçeneğini önerir; ağırlık ve jackknife form etiketleri hangi yöntemde işlediklerini söyler; sonuç değişir mi: hayır (yalnız çıktı sırası ve metinler).

## sirali_cfa_wlsmv

- Sıralı göstergelerle DFA: metin etiketli maddelerde (hic, az, biraz, …) kategori sırası ölçek sırası değil etiketin alfabetik ya da ilk görülme sırasıydı; model sessizce yanlış kuruluyor, hatta daha iyi uyum gösteriyordu (χ²ms 593,5 → 69,1; yükler −0,31 ile +0,47) → maddeler ölçek sırasıyla sıralı yapılır: etken sütunda Veri sekmesindeki düzey sırası, değerlerin hepsi sayıya çevrilebiliyorsa sayısal sıra, değilse harf sırası; kullanılan sıra çıktıya yazılır ve metin etiketli maddeler için sıranın ölçek sırasıyla uyuşması gerektiğini söyleyen uyarı verilir; sonuç değişir mi: evet (metin etiketli maddelerde; Veri sekmesinde düzey sırası ölçek sırasına getirilmişse sayısal kodlu verinin sonucuyla özdeş); sayısal kodlu maddelerde aynı.
- Eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve oranı yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- "Faktör varyanslarını 1'e sabitle" kapalıyken lavaan belirteç maddeyi değiştirebiliyor, tablo başlığı ise "ilk yük 1" diyordu → başlık yükü 1'e sabitlenen belirteç maddeyi yazar; sonuç değişir mi: hayır (yalnız başlık).
- Değişmezlik tablosunda kurulamayan model sessizce atlanıyor, fark satırının hangi modele göre olduğu belli değildi → "karsilastirilan_model" sütunu (sona eklendi) fark satırının karşılaştırıldığı modeli yazar, kurulamayan model notla bildirilir; sonuç değişir mi: hayır (yalnız bildirim; örnek veride üç model de kurulur).
- Aynı madde birden çok faktör listesinde seçilirse ikinci faktörden bildirimsiz çıkarılıyor, model çapraz yüklü değil tek yüklü kuruluyordu → anlaşılır iletiyle durur ("çapraz yüklü madde bu şablonda kurulmaz"); sonuç değişir mi: evet (yalnız bu seçimde; önceden sessizce başka bir model kuruluyordu).
- Eşik tablosunun başlığı parametreleştirmeyi (delta/teta) belirtmiyordu; form açıklaması değişmezlik modellerinin "faktör varyanslarını 1'e sabitle" seçimini izlediğini söylüyordu, oysa yalnız parametreleştirmeyi izler → başlık "Eşikler (delta parametreleştirmesi)" biçiminde yazılır, açıklamalar düzeltildi; sonuç değişir mi: hayır.

## sirali_nominal_gee

- Sıralı/nominal GEE: şablon açıklamasındaki ticari yazılım adları kaldırıldı (TR/EN/ES); sonuç değişir mi: hayır.

## sonlu_karisim_regresyon

- Sonlu karışım regresyonu: "İkili lojistik (0/1)" bileşen dağılımı hiçbir veride çalışmıyordu ve paketin gerçek hata iletisi gizleniyordu → ikili yanıt artık çalışır; uydurma başarısız olursa paketin iletisi de yazılır. Sonuç değişir mi: evet (ikili ailede çalışmayan seçenek çalışır hale geldi), öteki ailelerde hayır.
- Sonlu karışım regresyonu: EM yineleme sınırına ulaşan çözüm uyarısız yazılıyordu → olağan EM'de "yakınsamadı" uyarısı, CEM/SEM'de yineleme sınırına kadar çalıştığını söyleyen not. Sonuç değişmez, yalnız uyarı eklenir.
- Sonlu karışım regresyonu: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı yazılır. Sonuç değişmez.
- Sonlu karışım regresyonu: eş değişken olarak seçilen sütun açıklayıcıda/yanıtta da varsa sessizce düşüyordu → uyarı yazılır. Sonuç değişmez.
- Sonlu karışım regresyonu: CEM/SEM'de SH, z ve p boş çıkıyor ama başlık "SH ortak bilgi matrisinden" diyordu → başlık "SH hesaplanamadı" olur ve not yazılır. Sonuç değişmez.
- Sonlu karışım regresyonu: tam doğrusal bağımlı açıklayıcılarda (x ve 2x) genel "kurulamadı" iletisi çıkıyordu → bağımlı terim adı verilerek anlaşılır iletiyle durur; ondalık virgüllü metin sütunu açıklayıcı seçilirse nedeni söyleyen ileti verilir. Sonuç değişir mi: hayır (yine durur, ileti açıklayıcı).

## sosyal_ag_analizi

- Sosyal ağ analizi: ağırlıksız ağda yinelenen kenarın çokluğu yol ölçülerinde (aradalık, yakınlık, harmonik, çap, ortalama yol) uzaklık, özvektör ve PageRank'te güç olarak sayılıyordu → ağırlıksız ağda yinelenen kenar tek kenara iner, ölçüler yinelenensiz ağınkiyle aynıdır. Sonuç değişir mi: evet, yalnız ağırlıksız ağda yinelenen kenar olan verilerde (ağırlıklı ağda aynı, ağırlıklar toplanır).
- Sosyal ağ analizi: döngüsüz yönlü ağda özvektör merkeziliği anlamsız 0/1 vektör olarak yazılıyordu → sütun boş (NA) bırakılır ve uyarı yazılır. Sonuç değişir mi: evet (döngüsüz yönlü ağda).
- Sosyal ağ analizi: Louvain tek çalıştırılıyor ve sonucun tohuma bağlı olduğu söylenmiyordu → not yazılır; yeni "Louvain yeniden başlatma sayısı" alanı (varsayılan 1 = bugünkü sonuç) en yüksek modülerlikli bölünmeyi seçer. Sonuç değişir mi: varsayılanda hayır; alan 1'den büyükse evet olabilir.
- Sosyal ağ analizi: yönlü ağda walktrap/kenar aradalığı için modülerliğin yönsüz ağda hesaplandığı, ağırlıklı ağda yakınlığın 1'i aşabildiği ve yakınlığı tanımsız düğümlerin sayısı not olarak yazılır. Sonuç değişmez.

## stokastik_sinir

- Stokastik sınır analizi: frontier ile kestirilen modellerde (yarı normal, panel, zamanla değişen etkinsizlik) standart hatalar FRONTIER'in yinelemeli (DFP) yaklaşığından geliyordu ve tolerans ile ızgara ayarına göre değişiyordu → SH log-olabilirliğin gözlenen bilgi matrisinden (sayısal Hessian) hesaplanır; hesaplanamazsa frontier kovaryansı kullanılır ve not yazılır; sonuç değişir mi: evet, yalnız SH, z ve p (katsayılar aynı; örnekte γ'nın SH'si 0,1364 → 0,1385, kesişimin SH'si 0,20262 → 0,20258).

## surec_yeterliligi

- Süreç yeterliliği: kısa dönem "Cpm" alt grup içi σ ile hesaplanıyor ama standart Cpm tanımı toplam σ'ya dayanıyor, tablo bunu söylemiyordu → tablonun altına açıklayıcı not eklendi (tanım ve değerler değişmedi); sonuç değişir mi: hayır.
- Süreç yeterliliği: 5000'den fazla gözlemde Shapiro-Wilk sessizce uygulanmıyor, tablo hücreleri boş kalıyordu → "n > 5000 olduğundan Shapiro-Wilk uygulanmadı" notu yazılıyor; sonuç değişir mi: hayır.
- Süreç yeterliliği: bireysel ölçümde (alt grup yok) kısa dönem satırlarının dönem etiketi "kısa (alt grup içi)" yazıyordu → "kısa (ardışık fark)" yazıyor; sonuç değişir mi: hayır (yalnız etiket).
- Süreç yeterliliği: ölçüm Veri sekmesinde "kategorik" görünse bile artık sayı olarak okunuyor, sayıya çevrilemeyen değer anlaşılır iletiyle durduruyor; sonuç değişir mi: hayır.

## tanisal_meta

- Tanı doğruluğu meta-analizi: eşdeğer HSROC tablosunda Θ (Theta) ifadesindeki iki kök yer değiştirmişti → Θ, Rutter-Gatsonis parametrelemesinden türetilen ve mada paketinin `calc_hsroc_coef` işlevindeki ifadeyle aynı hesaplanır; sonuç değişir mi: evet (σ_A ≠ σ_B iken Θ satırı; örnekte 0,9586 → −0,0834; Λ, β, σ²_α, σ²_θ aynı).
- Tanı doğruluğu meta-analizi: "I² (Zhou-Dendukuri)" mada'nın hesabıydı ve çalışmalar arası varyans yerine sabit etkilerin standart hata karesini kullanıyordu, bunu hiçbir yer söylemiyordu → mada'nın sayısı "I² (Zhou-Dendukuri; mada hesabı)" olarak etiketlenir, tanım çıktıya yazılır ve aynı ifadenin çalışmalar arası varyanslarla (Ψ) hesaplanan "I² (Ψ'den)" satırı eklenir; sonuç değişir mi: hayır (mada değeri aynı; 0,4038); yeni satır ortak değişken yokken 0,8605.
- Tanı doğruluğu meta-analizi, meta-regresyon: başvuru düzeyi çıktıda yoktu ve ikili metin sütunu ile çok düzeyli sütun farklı kuralla sıralanıyordu → düzey sırası ortak kurala göre (Veri sekmesi sırası; metin ise hepsi sayıysa sayısal, değilse harf sırası), başvuru düzeyi ve sıra yazılır; SROC lejandı güven düzeyini gösterir (%90 seçilince %90); sonuç değişir mi: yalnız sayı kodlu metin sütunu (ör. "2", "10") için başvuru düzeyi değişir; öteki durumlarda hayır.
- Tanı doğruluğu meta-analizi: meta-regresyonda çalışma sayısı denetimi model parametre sayısına bakmıyordu (üç düzeyli sütun) → model matrisinin sütun sayısıyla denetlenir; sonuç değişir mi: hayır (yalnız yetersiz çalışma sayısında daha erken durma).
- Tanı doğruluğu meta-analizi: süreklilik düzeltmesi 0 ve sıfır hücre olduğunda anlaşılmaz paket iletisi → "düzeltme yok" ile aynı açık ileti; sonuç değişir mi: hayır (yalnız ileti).
- Tanı doğruluğu meta-analizi: çalışmalar arası kovaryans sınırda kestirilince (|ρ| ≥ 0,99, SD sıfıra yakın ya da olabilirlik yok; ör. momentler yöntemi) uyarı yoktu → uyarı notu yazılır; sonuç değişir mi: hayır.
- Tanı doğruluğu meta-analizi: eksik değerli satırlar sessizce atılıyordu → atılan satır sayısı ve kalan çalışma sayısı yazılır; sonuç değişir mi: hayır.

## tdt

- Aile tabanlı iletim/dengesizlik testi (TDT): büyük yuvarlak sayısal kimlikler (100000, 200000 …) birey sütununda "100000", ebeveyn sütunlarında "1e+05" diye okunuyor ve üçlüler sessizce düşüyordu (örnek veride 1466 yerine 1456 üçlü; χ² 14,02 yerine 13,50) → birey, baba, ana ve aile kimlikleri sütun türünden bağımsız aynı metne çevrilir; sonuç değişir mi: evet (yuvarlak büyük sayılı kimliklerde; öteki verilerde hayır).
- TDT: üçlü kurulamayan hastalar için tek satırlık belirsiz not yazılıyordu → not kaç hastanın hangi nedenle (baba kimliği veride yok, ana kimliği veride yok, yalnız bir ebeveyn kimliği yazılı) üçlü oluşturmadığını ve bulunamayan kimliklerden örnekleri yazar; hiç üçlü kurulamazsa hata iletisi aynı ayrıntıyı verir; her SNP'de eksik genotip nedeniyle kullanılmayan üçlülerin en çok sayısı ve genotip okuma dökümü de yazılır; sonuç değişir mi: hayır (yalnız not).
- TDT (genotip): genotipleri ortak ayrıştırıcı okur: alel adı "0" olan `0/1` genotipli SNP "tek biçimli" diye sessizce atlanmak yerine "alellerinden yalnız biri eksik kodu" iletisiyle çalıştırmayı durdurur; `-9` eksik kodudur (`-9/-9` içeren lokus artık "3 alelli" diye atlanmaz, sınanır); sonuç değişir mi: evet (yalnız `0/1` ya da `-9` içeren veride). Üstbilgi açıklaması (TR/EN/ES) eksik kuralını yazar.
- TDT: aile sütunu boşken özet satırı "1 aile" yazıyordu → aile sayısı baba + ana kimlik çiftinden sayılır; aynı aileden birden çok etkilenmiş çocuk varsa (üçlü sayısı aile sayısını aşarsa) üçlülerin bağımsız olmadığı ve sonucun bağlantı testi olarak geçerli, ilişkilendirme testi olarak yorumlandığında p değerinin küçük çıkabileceği notu yazılır; sonuç değişir mi: hayır (yalnız not ve özet satırı).
- Eksik genotip kodları (yeni alan): TDT: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## test_esitleme

- Test eşitleme: Tucker, Levine (gözlenen puan) ve zincirleme doğrusal yöntemde dönüşüm tablosu hata yüzünden yazılmıyor, çalıştırma "ok" bitiyordu → tablo yazılır (standart hata sütunları se_n, se_g); sonuç değişir mi: hayır (eksik tablo geldi, katsayılar aynı).
- Test eşitleme: sıklık kestirimi ve Braun/Holland'da paket sentetik ağırlığı sıklıkla çarpıyor, etkin ağırlık yazılan w_X'ten farklıydı → çıktı etkin olasılık ağırlığını yazar; yeni "Sentetik ağırlığın uygulanışı" alanı (Paket = bugünkü, Olasılık = Kolen ve Brennan tanımı); sonuç değişir mi: varsayılanda hayır; Olasılık seçilirse evet (n_X ≠ n_Y iken; örnek x = 33: 34,517 → 33,493).
- Test eşitleme: form düzeyi sırası metin sırasıydı ("10" < "2") ve "Yeni form" boşken ilk düzey sessizce X alınıyordu → düzeyler sayıysa sayısal sıra, değilse harf sırası; X boşsa hangi düzeyin alındığı yazılır; sonuç değişir mi: evet (yalnız sayısal kodlu ve sıra farkı olan formlarda, X boş bırakıldığında).
- Test eşitleme: atılan eksik değerli satır sayısı yazılır; NEAT log-doğrusal derece alanlarının etiketleri gerçek anlamlarına göre düzeltildi (üçüncü değer kullanılmıyor, çıktıya not); sonuç değişir mi: hayır.

## tmle

- TMLE: 0/1 sonuçta 'Doğrusal' dalgalanma seçilince EY1, EY0, ATE, RR ve OR olasılık dışı değerler (ör. EY1 1,09) yazıyordu → 0/1 sonuçta lojistik dalgalanma uygulanır ve çıktıya not yazılır; sonuç değişir mi: evet, yalnız 0/1 sonuçta doğrusal dalgalanma seçilmişse (artık lojistik sonuçla aynı).
- TMLE: sürekli sonuçta 'Doğrusal' dalgalanma ATT ve ATC'yi 0 yazıyordu → bu iki satır yazılmaz ve nedeni notla söylenir (ATT/ATC için lojistik dalgalanmayı seçin); sonuç değişir mi: evet, yalnız sürekli sonuç + doğrusal dalgalanmada (yanlış 0 satırları kalktı, EY1/EY0/ATE aynı).
- TMLE: hangi düzeyin tedavi sayıldığı sütun türüne (metin / kategorik) ve Veri sekmesindeki sıraya bağlıydı ve çıktıda yazılmıyordu → sayısal düzeylerde büyük, metin düzeylerinde alfabetik son düzey tedavidir (tür ve sıra etkisiz), çıktı seçilen düzeyi yazar; yeni 'Tedavi sayılan düzey' alanı ile elle seçilebilir (boş = kural); sonuç değişir mi: evet, iki düzeyli metin tedavisi 'kategorik' türde ve ilk görülen düzey kuralın seçtiğinin tersiyse (ATE işareti dönerdi).
- TMLE: 0/1 olmayan iki değerli sonuç (ör. 1/2) sessizce sürekli sayılıyordu → uyarı yazılır (sonuç sürekli işlenir, RR/OR çıkmaz); sonuç değişir mi: hayır (yalnız uyarı).
- TMLE: tedaviyi neredeyse ayıran ortak değişkende eğilim skorları 0/1'e çakılıp anlamsız ATE yazılıyordu → g sınırı dışında kalan gözlem payı %20 ve üstüyse anlaşılır iletiyle durur; sonuç değişir mi: evet, yalnız bu uç durumda (eskiden anlamsız sonuç yazılırdı).
- TMLE: 'Yalnız ana etkili GLM (deterministik)' çapraz doğrulama açıkken tohuma bağlıydı → etiket 'çapraz doğrulama kapalıyken deterministik' oldu ve bu birleşimde tohuma bağlılık notu yazılır; sonuç değişir mi: hayır.
- TMLE: ondalık virgüllü sayı sütunu metin okunup 'doğrusal bağımlı' hatası veriyordu → nedeni söyleyen iletiyle durur (ondalık ayırıcı nokta olmalı); sonuç değişir mi: hayır (hata iletisi).
- TMLE: ortak değişken ön elemesi hangi değişkenleri tuttuğunu/attığını yazmıyordu → tutulan ve elenen değişkenler yazılır; sonuç değişir mi: hayır.
- TMLE: ATT/ATC özdeşlik uyarısı her öğrenicide aynı öneriyi veriyor ve kuralı yalnız tedavi edilenler için anlatıyordu; Oransal etkiler tablosu başlığı güven düzeyini yazmıyordu → uyarı öğreniciye göre öneri verir ve kuralı ATT/ATC için ayrı anlatır, başlık güven düzeyini yazar; sonuç değişir mi: hayır (yalnız metin).

## tohum_bahcesi_cesitlilik

- Tohum bahçesi çeşitliliği: kendileşme oranı etiketi "tohum payı" diyordu, formül rastgele çiftleşmedeki kendileşmenin üstüne ekliyordu → etiket "üstüne eklenen" der ve çıktıda modeldeki toplam kendileşme payı yazılır (formül aynı); sonuç değişir mi: hayır.
- Tohum bahçesi çeşitliliği: seyreltme düzeylerinde "12,5; 25" üç düzeye (%5, %12, %25) bölünüyordu → ";" varsa virgül ondalıktır (iki düzey); ";" yoksa virgül liste ayırıcısıdır ve rakamlar arası virgül için uyarı yazılır; sonuç değişir mi: evet, yalnız ";" ile ve ondalık virgüllü girişte ("10, 20, 30" aynı).
- Tohum bahçesi çeşitliliği: erkek doğurganlık verilmediğinde "eşit tohum toplama" ve "üst sınır" senaryoları çiçek tozu payını da eşitleyerek senaryonun yararını abartıyordu → polen payları gözlenen doğurganlıktan türetilip sabit tutulur; sonuç değişir mi: evet, yalnız erkek sütunu yokken (örnek: eşit toplama Ns ürün 29,6 → 25,3).
- Tohum bahçesi çeşitliliği: doğurganlık/rameta/ıslah değeri sütunları `mq_sayisal` ile okunur (kategorik türde değer olarak, sayıya çevrilemeyen değerde anlaşılır ileti); sonuç değişir mi: hayır (kategorik sütunda düzeltme).
- [3.8.0] Tohum bahçesi çeşitliliği: yeni seçenek "Kendileşme payı tanımı" (varsayılan `ek` = bugünkü): `toplam` seçilirse s kendileşen tohumun toplam payıdır, kalan pay yalnız başka klonlara dağılır (örnek ayarda F_tohum 0,04786 → 0,02712); sonuç değişir mi: hayır (varsayılanda aynı; test `test_duzeltme_380_tohum_bahcesi_cesitlilik.py`).

## transfer_fonksiyonu

- Transfer fonksiyonu modeli (koşullu kareler toplamı, "CSS"): gürültü AR katsayıları ve standart hataları paketin dönüşüm ölçeğinde yazılıyordu (1,7237 ve −0,7448; doğrusu 1,5314 ve −0,6321) → CSS'te dönüşüm kapatılır, katsayılar ve SH'ler gerçek ölçekte yazılır; sonuç değişir mi: evet (yalnız yöntem CSS iken); "Tam en çok olabilirlik" (varsayılan) ve "CSS-ML" aynı.
- Transfer fonksiyonu modeli: eniyileyici yakınsamadığında ya da AR kestirimi durağanlık sınırına yapıştığında standart hata 0, z sonsuz, p 0 yazılıyordu → "UYARI: Yakınsama sağlanamadı" / "durağanlık sınırında" uyarıları yazılır; standart hatası sıfır ya da hesaplanamayan katsayıların z ve p değeri boş bırakılır; sonuç değişir mi: hayır (kestirimler aynı; yalnız uyarı ve sıfır SH'li satırların z, p değeri).
- Transfer fonksiyonu modeli: gürültü mertebeleri p = q = 0 (beyaz gürültü) seçilince "wrong length for 'fixed'" hatasıyla duruyordu → AR(1) katsayısı 0'a sabitlenerek aynı model kestirilir; sonuç değişir mi: hayır (eskiden hata).

## uc_deger

- Uç değer (GEV/GPD): aynı değerin çok tekrar ettiği (yuvarlanmış ölçüm) seride ölçek sıfıra çöküyor, ξ̂ = 4,04 gibi anlamsız bir sonuç olağan çıktı gibi yazılıyordu → ölçek parametresi serinin yayılımının milyonda birinden küçükse "Kestirim dejenere …" iletisiyle durur; eniyileyici yakınsadığını bildirmezse ya da |ξ| > 1 ise uyarı yazılır; sonuç değişir mi: evet (yalnız dejenere çözümde: eskiden anlamsız sayı, şimdi ileti).
- Uç değer (R, durağan olmayan GEV): ortak değişken ham yıl (1951–2010) gibi büyük ofsetliyse "arguments imply differing number of rows" hatasıyla çöküyordu → ortak değişken ortalanıp yeniden denenir ve not yazılır (μ0 ortalama zamandaki düzeydir, eğim ve olabilirlik oranı değişmez); sonuç değişir mi: evet (eskiden hata). Ortalanmış değişken verilirse sonuç aynıdır. (`uc_deger.R:143–157`; B2)
- Uç değer (GPD): dönüş düzeyi güven aralığı aşım oranı ζ'nın belirsizliğini yanlış (R) ya da hiç (Python) içeriyordu → ikisi de Coles (2001, §4.3.3) delta yöntemini ζ varyansıyla uygular; sonuç değişir mi: evet (GPD dönüş düzeyi aralıkları; Port Pirie 10 yıl: R [4,156; 4,480] ve Python [4,211; 4,425] yerine [4,198; 4,439]; nokta kestirimleri aynı). (`uc_deger.R:216–228`, `uc_deger_py.py:215–221, 253–256`; B3/P2)
- Uç değer (R, GMLE): negatif log-olabilirlik, AIC, BIC ve olabilirlik oranı cezalı olabilirlikten geliyor, ML ile karşılaştırılamıyordu → bu bir not olarak yazılır ve "Model uyumu" tablosunun sonuna cezasız değerler eklenir; sonuç değişir mi: hayır (mevcut satırlar aynı, yeni satırlar eklendi). (`uc_deger.R:165–182`; B4)
- Uç değer: dönüş periyotlarında "2,5" iki sayı (2 ve 5) sayılıyor ve bu söylenmiyordu → uyarı yazılır (ondalık için nokta); sonuç aynı. (`uc_deger.R:104`; B5)
- Uç değer (GPD): yıl başına gözlem sayısı 365,25 iken seri birkaç yıllık günlük veriden kısaysa (yıllık blok maksimumuna GPD uygulanması) uyarı yazılır; sonuç aynı. (`uc_deger.R:114`; B8)
- Uç değer: seri ve ortak değişken sayıya ortak yardımcıyla çevrilir; sonuç değişmez. (`uc_deger.R:97, 106`)

## uc_deger_py

- Uç değer (Python, GPD): dönüş düzeyi aralığı aşım oranı ζ'nın belirsizliğini içermiyordu → Coles (2001, §4.3.3) delta yöntemi ζ varyansıyla; sonuç değişir mi: evet (GPD dönüş düzeyi aralıkları; Port Pirie 10 yıl [4,211; 4,425] yerine [4,198; 4,439]; yağış 10 yıl yarı genişlik 10,05 yerine 10,29). (`uc_deger_py.py:215–221, 253–256`; P2)
- Uç değer (Python): dejenere çözümde (bağlı değerli seri) ölçek denetimiyle anlaşılır iletiyle durur; |ξ| > 1, ondalık virgüllü dönüş periyodu ve kısa seride yillik = 365,25 için uyarı satırları; sonuç değişmez. (`uc_deger_py.py:119–121, 130–132, 200–202, 212–214`)

## uzaklikla_yalitim

- Uzaklıkla yalıtım: birey düzeyindeki "paylaşılan alel uzaklığı (1 − Ps)" Bowcock ve ark. (1994) tanımıyla hesaplanmıyordu (homozigot–heterozigot çiftinde 0,50 yerine 0,75 paylaşım sayılıyordu) → Ps, ortak alel kopyalarının sayısı / (2 × karşılaştırılan lokus sayısı) olarak hesaplanıyor; sonuç değişir mi: evet (bu uzaklık seçildiğinde uzaklık matrisi, Mantel r, regresyon ve korelogram).
- Uzaklıkla yalıtım: paylaşılan alel uzaklığı veride tek bir eksik genotip varsa "Bazı birey çiftlerinin ortak genotipli lokusu yok" iletisiyle duruyordu (örnek veri dahil) → eksik genotipli çift yalnız o lokusta atlanıyor; sonuç değişir mi: evet (eskiden sonuç yoktu).
- Uzaklıkla yalıtım: koordinat ve çevre sütunları değerleriyle okunuyor (kategorik türdeki sütunda düzey sıra numarası kullanılmaz); sayıya çevrilemeyen değer varsa çalıştırma açık iletiyle durur; sonuç değişir mi: evet (koordinat sütunu kategorik türdeyse; sayı olmayan koordinat içeren satırlar eskiden sessizce atılıyordu).
- Uzaklıkla yalıtım: az birimde Mantel testi bütün düzenleri sayar; tabloda istenen permütasyon sayısı yazıyordu → gerçekleşen sayı yazılıyor ve tam sayım bir notla bildiriliyor; sonuç değişir mi: hayır (yalnız `permutasyon` sütunu ve not).
- Uzaklıkla yalıtım: Mantel korelogramında `n_uzaklik` sütunu çift sayısının iki katıydı → sütun `n_cift` oldu ve çift sayısını veriyor; tek sınıf sınandığında düzeltilmiş p sütunu NaN yerine ham p; başlık düzeltmenin aşamalı olduğunu söylüyor; sonuç değişir mi: evet (sütun adı ve değeri; tek sınıflı korelogramda düzeltilmiş p).
- Uzaklıkla yalıtım: iki boyutlu modelde regresyon ve grafik ln(uzaklık) ile, Mantel testi dönüştürülmemiş uzaklıkla yapılıyor ve çıktı bunu belirtmiyordu → var olan Mantel satırı aynen kalır; popülasyon düzeyinde (sıfır uzaklıklı çift yokken) tabloya ikinci satır eklenir: "Mantel, ln(uzaklık)" (aynı permütasyon düzenleri; Rousset 1997); tablo adı ve grafiğin alt başlığı hangi uzaklığın kullanıldığını yazar; birey düzeyinde ek satır verilmez, nedeni notta; sonuç değişir mi: hayır (var olan sayılar aynı; yeni satır eklenir, örnek veride r = 0,5483).
- Uzaklıkla yalıtım: iki boyutlu modelde 1/b = 4πDσ² için "km cinsinden" notu "birimsiz" olarak düzeltildi; sonuç değişir mi: hayır (yalnız metin).
- Uzaklıkla yalıtım: genotipleri ortak ayrıştırıcı okuyor: `-9` eksik kodudur; alellerinden yalnız biri eksik kodu olan genotip (`0/152`) sessizce atılmak yerine çalıştırmayı durdurur; eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (yalnız `-9` kodlu ya da yarı eksik genotip içeren veride).
- Eksik genotip kodları (yeni alan): Uzaklıkla yalıtım: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).

## uzamsal_panel

- Uzamsal panel: birim ya da dönem sabit etkili uzamsal hata modelinde "logLik" satırı sabit terimleri atılmış bir çekirdek değer veriyordu (örnekte 56,47; gecikme modeli 1609,72) ve gecikme modeliyle yan yana konunca yanlış model seçimine götürüyordu → tam Gauss log-olabilirlik yazılır (örnekte 1634,02) ve çıktıya bunu söyleyen bir not eklenir; sonuç değişir mi: evet (yalnız sabit etkili hata modelinde logLik satırı; katsayılar aynı).
- Uzamsal panel: sabit etkili SARAR modelinde "logLik" satırı `NaN` yazılıyordu → aynı tam tanımla hesaplanır (örnekte 1638,30) ve not düşülür; sonuç değişir mi: evet (logLik NaN yerine sayı; öteki sonuçlar aynı).
- Uzamsal panel: "Seyrek Cholesky (spam)" Jacobian yöntemi simetrik olmayan (ör. satır standartlaştırılmış) W ile uzamsal parametreyi aralık ucuna (0,999) yapıştırıp başarılı bir kestirim gibi sonuç yazıyordu → sonuç aralığın ucuna yapışırsa açık bir iletiyle durur ("özdeğer ya da LU yöntemini seçin"); sonuç değişir mi: evet (eskiden yanlış sonuç, şimdi hata iletisi; özdeğer, LU ve simetrik W ile spam aynı).
- Uzamsal panel: Moran I şekli süzülmemiş (yapısal) artıkları çiziyordu; uzamsal parametreler çıkarılınca kalan özilişki hep pozitif görünüyor, yetersiz model izlenimi veriyordu → şekil ve yeni tablo ("Süzülmüş artıklarda dönem başına Moran I") süzülmüş artığı gösterir, bir not formülü söyler; sonuç değişir mi: evet (şekildeki değerler; örnekte 0,075–0,588 yerine −0,183–0,116).
- Uzamsal panel: ağırlık sütunları ve koordinatlar yalnız ilk dönemden okunuyor, sonraki dönemlerde değişen W sessizce yok sayılıyordu → bütün dönemler ilk dönemle karşılaştırılır; fark varsa hangi dönemde olduğunu söyleyen bir iletiyle durur; sonuç değişir mi: evet (yalnız zamanla değişen W içeren veride; eskiden sessiz yok sayılırdı).
- Uzamsal panel: uzamsal Hausman testi `hata` seçiminden bağımsız olarak rastgele etki tarafını Kapoor ve ark. yapısıyla kuruyordu ve bunu söylemiyordu → varsayılan aynı kalır (Kapoor; sonuç aynı) ve hangi yapının kullanıldığını söyleyen not eklenir; yeni "Hausman testinde rastgele etki hata yapısı" alanında "Hata seçimini izle" seçilince "Uzamsal hata = b" ise Baltagi ve ark. yapısı kullanılır (örnekte χ² 35,156 yerine 30,601); uzamsal bileşensiz modelde testin uzamsal hata modeliyle yapıldığı not edilir; sonuç değişir mi: hayır varsayılanda; evet yalnız yeni seçenekte.
- Uzamsal panel: açıklayıcı listesindeki birim/zaman/bağımlı sütun sessizce çıkarılıyordu; `hata = kkp` sabit etkide `b` ile aynı olduğunu söylemiyordu → çıkarılan sütunlar ve `kkp` notu yazılır; sonuç değişir mi: hayır (yalnız ek not satırları).

## uzay_zaman_kriging

- Uzay-zaman kriging: saatlik veride gece yarısı satırları yalnız tarih olarak yazılıysa (tablo programı dışa aktarımında olağan), ilk satır bu biçimdeyse bütün saatler güne çöküyor ve "Aynı istasyon ve zamanda birden çok gözlem var" iletisiyle durulup kullanıcı saatleri günlük ortalamaya çevirmeye yöneltiliyordu → zaman damgası her satır için ayrı okunur; yalnız tarih içeren satırlar gece yarısı sayılır ve bir not düşülür; sonuç değişir mi: evet (eskiden hata; tam damgalı aynı veriyle aynı sonuç çıkar).
- Uzay-zaman kriging: Veri sekmesinde "kategorik" görünen 1, 2, 3 … adım sütunu "Zaman sütunu okunamadı" iletisiyle reddediliyordu → etiketlerin hepsi sayıysa sayısal adım olarak okunur; sayısal adımda konsol "adım = adım" yerine adım uzunluğunu yazar; sonuç değişir mi: hayır (eskiden hata; sayısal türle aynı sonuç).
- Uzay-zaman kriging: boylam-enlem derecesi "Projeksiyonlu" seçeneğiyle verilince uyarı yoktu ve uzaklıklar sessizce derece olarak hesaplanıyordu → koordinatlar derece aralığındaysa ("derece gibi görünüyor") uyarı notu yazılır ve "Coğrafi" seçilmesi önerilir; sonuç değişir mi: hayır (yalnız uyarı notu).
- Uzay-zaman kriging: uzay-zaman anizotropisi κ kestirimi aralığın ucuna yapıştığında uyarı yoktu → κ aralığın uçlarının %1'i içindeyse "κ kestirim aralığının ucunda" uyarısı yazılır ve aralığı genişletmek ya da κ'yı elle vermek önerilir; sonuç değişir mi: hayır (yalnız uyarı notu).
- Uzay-zaman kriging: form "Uydurma ağırlıklandırması" olarak 5'i (REML) kabul ediyor, gstat ise uygulamadığından hiçbir model uydurulamıyor ve ileti nedeni söylemiyordu → 5 seçilince "REML gstat paketinde uygulanmamıştır, 1–4 ya da 6–11 seçin" iletisiyle durur; alan açıklaması bunu yazar; sonuç değişir mi: hayır (eskiden de hata; ileti açık).

## vaka_kontrol_genetik

- Vaka-kontrol genetik ilişkilendirme: metin durum sütununda vaka, ad sırasındaki ikinci düzey alınıyordu; "Case/Control", "hasta/saglam", "astim/normal" yazımlarında kontroller vaka oluyor, odds oranları tersine dönüyordu (rs1422993 eş baskın OR 1,44 yerine 0,69) → vaka düzeyi sırayla "Vaka düzeyi" alanından, bilinen vaka/kontrol adlarından (case, vaka, hasta, affected, control, kontrol, saglam, healthy, normal …), sayı biçimli düzeylerde büyük değerden, Veri sekmesindeki düzey sırasından, son olarak alfabetik sıradan alınır; seçilen düzey ve ölçüt çıktıda yazılır (ölçüt tahmine dayanıyorsa "Yanlışsa 'Vaka düzeyi' alanına yazın" uyarısı); sonuç değişir mi: evet (bilinen adlarla yazılmış ve eskiden ters çözülmüş durum sütunlarında; alfabetik ikinci düzeyin vaka olduğu `kontrol/vaka`, `no/yes`, 0/1 yazımlarında hayır).
- Vaka-kontrol genetik: adı `y` ya da `z` olan ortak değişken modele girmiyor, sonuç sessizce düzeltmesiz kalıyor (z için olabilirlik oranı p'leri boş) → modelin iç sütun adları kullanıcı adlarıyla çakışmaz; sonuç değişir mi: evet (yalnız ortak değişkeni `y` ya da `z` adlı çalıştırmalarda; ör. rs1422993 eş baskın OR 1,4396 yerine 1,4408).
- Vaka-kontrol genetik (genotip): genotipleri ortak ayrıştırıcı okur: `0/1` kodlu lokus "tek biçimli" diye sessizce atlanmak yerine "alellerinden yalnız biri eksik kodu" iletisiyle çalıştırmayı durdurur; `-9/-9` eksik sayılır ve bütün çalıştırmayı "3 alelli" diye durdurmaz; eksik sayılan genotiplerin dökümü yazılır; sonuç değişir mi: evet (yalnız `0/1` ya da `-9` içeren veride). Üstbilgi açıklaması (TR/EN/ES) eksik kuralını ve vaka düzeyi seçimini yazar.
- Vaka-kontrol genetik: ortak değişkenin bir düzeyi yalnız vaka ya da yalnız kontrol içerdiğinde (tam ayrışma) uyarı yoktu → düzeyler ve birey sayısı uyarıda yazılır; hem SNP hem ortak değişken seçilen sütun için ortak değişkenlerden çıkarıldığı notu yazılır; sonuç değişir mi: hayır (yalnız uyarı/not).
- Eksik genotip kodları (yeni alan): Vaka-kontrol genetiği: eksik kodlar listesi sabitti (boş hücre, NA, 0, 00, 000, ?, -, ., -9) → forma "Eksik genotip kodları" alanı eklendi; boşsa bugünkü liste, doluysa alandaki kodlar (virgülle ayrılmış; boş hücre ve NA her zaman eksik) listenin yerine geçer ve yarı eksik durma kuralı yeni listeyle işler (VCF tarzı 0/1 veride yalnız "." yazılınca 0 gerçek alel olur); durma iletisi alanın adını söyler; sonuç değişir mi: hayır (alan boşken).
- Vaka-kontrol genetiği: metin durum sütununda vaka/kontrol düzeyini tanıyan adlar sabit listeydi → forma "Vaka adları" ve "Kontrol adları" alanları eklendi (virgülle ayrılmış, büyük/küçük harf önemsiz); boşsa bugünkü bilinen adlar, doluysa alandaki adlar o listenin yerine geçer (yalnız biri doluysa aynı adlar öteki bilinen listeden çıkarılır); "Vaka düzeyi" alanı yine önceliklidir; sonuç değişir mi: hayır (alanlar boşken).

## veri_zarflama

- Veri zarflama analizi: Veri sekmesinde "kategorik" türde duran girdi ya da çıktı sütununda değerler yerine düzey sıra numaraları kullanılıyordu → sütun değerleriyle okunur, sayıya çevrilemeyen değerde sütun adıyla ileti verilir; sonuç değişir mi: evet, yalnız girdi/çıktı sütunu kategorik türdeyse (örnekte ortalama TE 0,7276 → 0,7632).
- Veri zarflama analizi: Süper etkinlik notu olurlu olmayan çözümü yalnız "değişken ölçek getirisine" bağlıyordu → not VRS, NIRS ve FDH'de görülebileceğini söyler; sonuç değişir mi: hayır (yalnız not metni).

## yanit_yuzeyi_tasarim

- Yanıt yüzeyi tasarımı: Türkçe arayüzde üretilen tasarım tablosunun başlığında iç kod ('ccd') yazıyordu ve varyans çözümlemesi tablosunda 'Residuals / Lack of fit / Pure error' İngilizce çıkıyordu → okunur Türkçe adlar ('merkezi bileşik, dönebilir α'; 'Artık', 'Uyum eksikliği', 'Saf hata'); faktör kodlaması tablosunun satır adları tutarlı hâle getirildi; sonuç değişir mi: hayır.

## yapisal_kirilma

- Yapısal kırılma (Bai-Perron): en küçük bölüm ⌊h·n⌋ kayan nokta sapmasıyla bir eksik çıkıyordu (Nil, h = 0,29 → 28 gözlem) ve supF adayları bir gözlem erken başlıyordu → h·n tam sayıya yuvarlama payıyla hesaplanır (h = 0,29 → 29) ve supF/aveF/expF aday aralığı aynı en küçük bölümden başlar; sonuç değişir mi: yalnız h·n'nin tam sayı olduğu ayarlarda (ör. n = 100, h = 0,29); örnek ayar (h = 0,15) aynıdır.
- Yapısal kırılma: istenen kırılma sayısı en çok olanaklı sayıdan büyükse sessizce kısılıyordu → uyarı satırı yazılır; sonuç değişmez. (`yapisal_kirilma.R:141`; B2)
- Yapısal kırılma: zaman etiketi eşit aralıklı değilse (eksik yıllar) kırılma ve güven aralığının gözlem sırasına göre hesaplandığı belirtilmiyordu → uyarı yazılır; sonuç değişmez. (`yapisal_kirilma.R:150–151`; B3)
- Yapısal kırılma: zaman etiketi boşken satır sırasının zaman sırası sayıldığı belirtilmiyordu → not yazılır; sonuç değişmez. (`yapisal_kirilma.R:135`; B4)
- Yapısal kırılma: doğrusal bağımlı açıklayıcıda anlaşılmaz hata ("missing value where TRUE/FALSE needed") → "Açıklayıcılar sabit ya da doğrusal bağımlı …" iletisiyle durur; sonuç değişir mi: hayır (eskiden de çalışmıyordu). (`yapisal_kirilma.R:122`; B5)
- Yapısal kırılma: OLS-MOSUM/ME p değeri 0,01 tabanında ve F testi p değeri sayısal tabanda iken bunlar ölçülmüş değer gibi görünüyordu → tablonun altına açıklayıcı not yazılır; sonuç değişmez. (`yapisal_kirilma.R:190–193`; B6)
- Yapısal kırılma: 20 gözlemlik gibi kısa seride (en küçük bölüm < 5 parametre) uyarısız çalışıyordu → uyarı satırı yazılır; sonuç değişmez. (`yapisal_kirilma.R:136–137`; B7)
- Yapısal kırılma: seri sütunu sayıya ortak yardımcıyla çevrilir (sayıya çevrilemeyen değerde anlaşılır ileti); sonuç değişmez. (`yapisal_kirilma.R:116`)

## yapisal_kirilma_py

- Yapısal kırılma (Python): en küçük bölüm ⌊h·n⌋ ve supF kırpması (`nh / n * n`) kayan nokta sapmasından etkileniyordu → h·n tam sayıya yuvarlama payıyla hesaplanır, supF adayları en küçük bölümden başlar; sonuç değişir mi: yalnız h·n'nin tam sayı olduğu ayarlarda (ör. n = 100, h = 0,29); örnek ayar aynıdır.
- Yapısal kırılma (Python): doğrusal bağımlı açıklayıcıda ham "Singular matrix" istisnası ve iz → "Açıklayıcılar sabit ya da doğrusal bağımlı …" iletisiyle durur; sonuç değişir mi: hayır. (`yapisal_kirilma_py.py:108–110`; Y2)
- Yapısal kırılma (Python): kırılma sayısı kısılması, eşit aralıklı olmayan zaman etiketi, zaman etiketi yokluğu ve çok kısa bölüm için uyarı/not satırları eklendi (R eşiyle aynı); sonuç değişmez. (`yapisal_kirilma_py.py:112–117, 187–189, 269–271`)

## yapisal_konu_modeli

- Yapısal konu modeli, K seçimi (searchK): anlamsal tutarlılık ve dışlayıcılık sütunları içerik değişkeni olmadığı hâlde hep boş kalıyordu → içerik değişkeni yokken bu iki sütun hesaplanır (içerik değişkenli modelde stm hesaplamaz, uyarı sürer); sonuç değişir mi: evet (yalnız bu iki sütun; boştu, dolu geliyor; öteki sütunlar aynı).
- Durak sözcük listesindeki kesme işaretli sözcükler (don't, can't, i'm …) noktalama silindikten sonra eşleşmiyor, "dont/cant/im" konu sözcüğü olarak kalıyordu → bu 50 sözcük noktalama silinmeden önce metinden çıkarılır, çıkarılan geçiş sayısı yazılır; sonuç değişir mi: evet (İngilizce metinde durak sözcük silme açıkken; örnek veride 75 geçiş çıkarılır, sözlük, ELBO, iterasyon, konu sözcükleri ve yaygınlıklar değişir). Kesme işareti içermeyen dillerde ya da durak silme kapalıyken hayır.
- Tohum: alan üst sınırı 2147483 idi → 2147483647; alan 0 ya da boşsa Kod panelindeki çalıştırma tohumu kullanılır (alan varsayılanı 12345 değişmedi); sonuç değişir mi: hayır.
- estimateEffect p değerlerinin 25 θ çekimine dayandığı ve tohuma duyarlı olduğu çıktıda ve alan açıklamasında not edilir; sonuç değişir mi: hayır.

## yapisal_var

- Yapısal VAR: formdaki bootstrap tohumu kod panelinin tohumunu eziyordu ve çıktıda hangi tohumun kullanıldığı yazmıyordu → çıktıya kullanılan tohum ve kaynağı yazılıyor; yeni seçenek: tohum alanına 0 yazılırsa kod panelinin tohumu kullanılır (varsayılan 12345 = eski davranış); sonuç değişir mi: hayır (yalnız alan 0 yapılırsa bant panel tohumuna bağlanır).
- Yapısal VAR: hata kovaryansı "sd" (varsayılan) iken `logLik_VAR` ve `logLik_SVAR` VAR'ın en çok olabilirlik değeri değildi (örnekte 595,27) → her zaman U'U/T ile hesaplanıyor (606,31) ve `sd` seçiliyse çıktıya not yazılıyor; aşırı tanımlama LR testi değişmedi; sonuç değişir mi: evet (yalnız `sd` iken model özeti tablosundaki iki log olabilirlik değeri; `ml` iken hayır).
- Yapısal VAR: kısıt matrisinde ondalık virgül öğe sayısını bozunca ileti yanıltıcıydı ("3 satır") → ileti hangi satırda kaç öğe olduğunu ve ondalık ayırıcının nokta olması gerektiğini söylüyor; sonuç değişir mi: hayır (yalnız hata iletisi).
- Yapısal VAR: düzey (birim köklü) serilerle uzun dönem kısıtı sessizce anlamsız büyük etkiler veriyordu → en büyük karakteristik kök 0,98'i aşarsa "seriler durağan görünmüyor" uyarısı yazılıyor; sonuç değişir mi: hayır (yalnız uyarı).
- Yapısal VAR: tekil artık kovaryansında ham R iletisi ("system is computationally singular") görünüyordu → anlaşılır ileti ("artık kovaryansı tekil, seriler neredeyse deterministik"); sonuç değişir mi: hayır (yalnız hata iletisi).
- Sayısal sütun okuma: sayıya çevrilemeyen değer olan sütunda ortak yardımcıdan (`mq_sayisal`) anlaşılır ileti geliyor; sonuç değişir mi: hayır (yalnız hata iletisi).

## yayin_yanliligi_ileri

- Yayın yanlılığı (R): Seçimin yönü temel etkinin işaretiyle çelişse de ya da adım fonksiyonunun bir aralığında hiç çalışma kalmasa da anlamsız bir "düzeltilmiş" kestirim yazılıyordu ve metafor uyarısı görünmüyordu → Uyarı artık sonuç ekranında (konsolda) yazılıyor; boş aralıkta seçim modeli satırı ve tabloları yazılmıyor; bir aralıkta 3'ten az çalışma varsa uyarı veriliyor. Forma "Temel etkinin işaretinden (otomatik)" yön seçeneği eklendi (varsayılan "Büyük" aynı). Sonuç değişir mi: yalnız ters yönlü ya da boş aralıklı kullanımda (anlamsız satır artık yok); öteki kullanımda hayır.
- Yayın yanlılığı (R): PET ve PEESE güven aralığı normal kuantille, p değeri t dağılımıyla veriliyordu → aralık da t(k−2) kuantiliyle kuruluyor. Sonuç değişir mi: evet, PET ve PEESE güven sınırları biraz genişler (örnek veride PEESE: 0,0184–0,2218 → 0,0147–0,2255); kestirim ve p değişmez.
- Yayın yanlılığı (R): Paule-Mandel τ² kestirimi gevşek sayısal toleransla bulunuyordu → tolerans sıkılaştırıldı. Sonuç değişir mi: evet, yalnız PM seçilince, dördüncü basamakta (örnek veride τ² 0,0128986 → 0,0128767; μ 0,209198 → 0,209171).
- Yayın yanlılığı (R): Açıklama "p değerleri tek yönlüdür" diyordu, oysa tablodaki bütün p'ler iki yönlüydü → açıklama düzeltildi (yalnız koşullu PET-PEESE kararı tek yönlü p kullanır). Sonuç değişir mi: hayır.
- Yayın yanlılığı (R): Sürekli ağırlık fonksiyonlarında (beta, yarı normal vb.) tablo, aralık ağırlığı değil biçim parametrelerini "ağırlık" diye yazıyordu → tablo "ağırlık fonksiyonunun biçim parametreleri (delta)" başlığıyla, `parametre`, `kestirim`, `SH` sütunlarıyla yazılıyor (`k` ve `agirlik` sütunları sürekli türlerde yok). Sonuç değişir mi: sayılar aynı, etiket ve sütun adları değişir.

## yayin_yanliligi_ileri_py

- Yayın yanlılığı (Python): Seçimin yönü temel etkinin işaretiyle çelişince ya da bir aralık boş kalınca anlamsız bir "düzeltilmiş" kestirim ve çok büyük ağırlık yazılıyor, hiçbir uyarı verilmiyordu → R şablonuyla aynı davranış: uyarı konsola yazılıyor, boş aralıkta seçim modeli yazılmıyor, 3'ten az çalışmalı aralıkta uyarı veriliyor; "Temel etkinin işaretinden (otomatik)" yön seçeneği eklendi (varsayılan aynı). Sonuç değişir mi: yalnız ters yönlü ya da boş aralıklı kullanımda.
- Yayın yanlılığı (Python): τ² sınırdayken seçim modelinde μ'nün standart hatası R'den yaklaşık %16 büyüktü ve negatif köşegen sessiz geçiyordu → τ² sınırdayken (<1e-6) standart hata τ² sabit tutularak hesaplanıyor (R ile aynı) ve çıktıya not yazılıyor; pozitif olmayan köşegende SH NaN ve uyarı. Sonuç değişir mi: evet, yalnız τ²'nin sınırda kaldığı seçim modellerinde SH ve p (homojen örnekte SH 0,0851 → 0,0733).
- Yayın yanlılığı (Python): Eniyileyicinin yakınsama durumu okunmuyordu → okunuyor; başarısızsa gradyan küçükse not, değilse uyarı yazılıyor; ikinci tur kötüleşirse birinci turun sonucu kullanılıyor. Sonuç değişir mi: hayır (yalnız not/uyarı eklenir).
- Yayın yanlılığı (Python): PET ve PEESE güven aralığı normal kuantille, p değeri t ile veriliyordu → aralık t(k−2) kuantiliyle. Sonuç değişir mi: evet, PET/PEESE güven sınırları biraz genişler; kestirim ve p aynı.

## yeterlilik_normal_disi

- Normal olmayan süreç yeterliliği: çok aykırı değerli veride Clements (Pearson) eğrisi bozuluyor, anlamsız sonuç (ör. PPL 3,67 milyon) uyarısız tabloda kalıyordu → eğri dejenere olduğunda (üç yüzdelik çakışıyorsa) satır tablodan çıkarılıyor ve açıklayıcı not yazılıyor; öteki yöntemler etkilenmiyor; sonuç değişir mi: evet (yalnız bu durumda: Clements satırı artık yok; aykırı değersiz veride hayır).
- Normal olmayan süreç yeterliliği: Box-Cox satırının Pp/PPL/PPU/Ppk'sı dönüştürülmüş ölçekte hesaplanıyor, öteki satırlar yüzdelik tanımıyla hesaplanıyor ve bu aynı sütunlarda yan yana duruyordu → yeni "Box-Cox indeks tanımı" seçeneği (varsayılan "dönüştürülmüş ölçekte" = eski sonuç; "yüzdelik tanımı" = öteki satırlarla aynı ISO 22514-2 tanımı) ve varsayılan çıktı notunda bu fark açıkça yazılıyor; sonuç değişir mi: hayır (varsayılanda aynı; örnek veride "yüzdelik" seçilirse Ppk 0,8775 → 0,9426).
- Normal olmayan süreç yeterliliği: boş gözlemler kaç tane atıldığı söylenmeden düşüyordu → "N boş ya da sonlu olmayan gözlem atıldı" notu yazılıyor; ölçüm Veri sekmesinde "kategorik" görünse bile sayı olarak okunuyor, sayıya çevrilemeyen metin anlaşılır iletiyle durduruyor; sonuç değişir mi: hayır (sayısal sonuç aynı; metin içeren sütunda artık duruyor).

## yineleyen_olay

- Yineleyen olay (WLW, gecikmeli giriş): WLW her denegi sıfırdan itibaren riskte sayar; ilk aralığı ilk olaydan sonra başlayan (gecikmeli girişli) veride sonuç girişten bağımsız ve yanlıydı → çalıştırma "WLW marjinal modeli gecikmeli girişi desteklemez" iletisiyle durur (Andersen-Gill ya da PWP önerilir); herkes aynı noktadan giriyorsa (zaman kaymışsa) sonuç değişmez; sonuç değişir mi: evet (gecikmeli girişli veride artık sonuç yerine ileti).
- Yineleyen olay (WLW, zamanla değişen ortak değişken): WLW denek başına yalnız ilk aralığın değerini kullanıyordu ve bunu söylemiyordu → denek içinde değişen ortak değişken için "UYARI" satırı yazılır; sonuç değişir mi: hayır.
- Yineleyen olay (sayım süreci verisi): aynı denekte örtüşen aralıklar sessizce kabul ediliyordu (denek aynı anda iki kez riskte sayılıyordu) → çalıştırma "Aynı denekte örtüşen aralıklar var" iletisiyle durur; aralıklar arası boşluk (riskte olmama dönemi) geçerlidir ve not edilir; sonuç değişir mi: evet (örtüşen aralıklı veride artık ileti; boşlukta sonuç aynı).
- Yineleyen olay (WLW, en büyük olay sırası): K gözlenen en büyük olay sayısından büyükse olaysız tabakalar NaN katsayı ve SE = 0 satırı üretiyordu → K gözlenen en büyük sıraya indirilir ve not yazılır; sonuç değişir mi: hayır (öteki katsayılar aynı; NaN/SE = 0 satırları kalkar).
- Yineleyen olay (veri özeti): WLW'de özet tablosu modelin kullandığı satır ve olay sayısını göstermiyordu → iki ek satır yazılır; sonuç değişir mi: hayır.
- Yineleyen olay (eksik değer): eksik değerli satırlar bildirilmeden çıkarılıyordu → sayıları yazılır, çıkarılan satır olay içeriyorsa olay sırasının kayabileceği uyarılır; sonuç değişir mi: hayır.
- Yineleyen olay (kırılganlık): θ'nın bağ yöntemine duyarlılığı ve SH'nin yaklaşık olduğu belirtilmiyordu → çıktıya kısa not eklendi; sonuç değişir mi: hayır.

## yontem_benzetim

- Yöntem karşılaştırması (benzetim): bootstrap yeniden örnekleme sayısı α'ya göre denetlenmiyordu (α = 0,01 ile 199 kopyada bootstrap satırı yapay biçimde şişiyordu) → (boot_B + 1)·α/2 < 1 ise çalıştırma anlaşılır iletiyle durur, < 5 ise çıktıya uyarı satırı yazılır; sonuç değişir mi: hayır (yalnız uyarı/durma; varsayılan ve örnek ayarlar uyarmaz).

## zamana_bagli_roc

- Zamana bağlı ROC: değerlendirme zamanlarında "2,5; 7,5" yazımında virgül ayırıcı sayılıp 2,5 yıl 2 ve 5 yıla bölünüyordu → alanda noktalı virgül varsa virgül ondalık sayılır; noktalı virgülsüz "2,5" eski ayrımı korur ve çıktıya uyarı yazılır; sonuç değişir mi: evet (yalnız noktalı virgüllü ondalık virgül yazımında).
- Zamana bağlı ROC: durum 1 = sansür, 2 = olay kodlanınca sessizce yanlış çözümleme yapılıyordu (AUC 0,47 yerine 0,83) → çıktıya durum kodları dökümü yazılır; sansür kodu 0 yok ve birden çok olay kodu varsa açık UYARI; sonuç değişir mi: hayır (kodlama yanlışsa kullanıcı uyarılır, hesap aynı).
- Zamana bağlı ROC: tablodaki "vaka" sayısı T ≤ t sayıyordu, AUC T < t kullanır → tablo T < t sayar; sonuç değişir mi: evet (yalnız değerlendirme zamanı bir olay zamanıyla aynıysa vaka sütunu; AUC değişmez).
- Zamana bağlı ROC: güven düzeyi 0,95'te sabit 1,96 kullanılıyordu → her düzeyde normal dağılım niceli (1,959964); sonuç değişir mi: evet (GA uçlarında 10^-6 düzeyinde).
- Zamana bağlı ROC: eksik satır sayısı, çıkarılan zamanlar (bilimsel gösterimle yazılıyordu) ve AUC 0,5'in altında kalan doğrudan belirteç için uyarı eklendi; sonuç değişir mi: hayır.

## zayif_arac_saglam

- Zayıf araca sağlam IV (R): dışsal ortak değişken seçilmeyince şablon 'incompatible dimensions' ile çöküyordu → ortak değişkensiz model çalışır (Python eşiyle aynı sonuç); sonuç değişir mi: evet, yalnız ortak değişken seçilmediğinde (eskiden çalışmıyordu).
- Zayıf araca sağlam IV: 'k-sınıfı standart hataları' seçimi AR, CLR ve Sargan'a yansımaz ama bu söylenmiyordu → seçim homoskedastik değilse çıktıya açık not yazılır, AR/CLR tablosunun başlığı 'klasik hata varsayımı' der (iki şablonda); sonuç değişir mi: hayır (yalnız bildirim).
- Zayıf araca sağlam IV: araç ya da ortak değişken listesinden sonuç/içsel değişkenle aynı ya da çakışan sütunlar sessizce çıkarılıyordu → çıkarılan sütunlar notla yazılır (iki şablonda); sonuç değişir mi: hayır (yalnız bildirim).
- Zayıf araca sağlam IV: sınırsız uç 'Inf' (R) ya da Unicode eksiyle '−∞' (Python) yazılıyordu → iki şablonda '∞' ve '-∞'; sonuç değişir mi: hayır (yalnız yazım).
- Zayıf araca sağlam IV (R): k-sınıfı p değerleri 10^-16 altında 0 yazılıyordu → üst kuyruktan hesaplanır (ör. EKK p = 2,28e-92); sonuç değişir mi: evet, yalnız çok küçük p değerlerinde (diğer p değerleri aynı).
- Zayıf araca sağlam IV (R): AR güven kümesi boşken '[NA; NA]' ve parça sayısı 1 yazılıyordu → 'boş küme', parça sayısı 0 ve araçların çeliştiğini söyleyen uyarı yazılır (Python eşiyle aynı); sonuç değişir mi: evet, yalnız boş küme durumunda (sayılar doğru gösterilir).

## zayif_arac_saglam_py

- Zayıf araca sağlam IV (Python): araç ya da ortak değişken listesinden çıkarılan sütunlar sessizdi → notla yazılır; sonuç değişir mi: hayır (yalnız bildirim).
- Zayıf araca sağlam IV (Python): güven kümesi metninde sınırsız uç Unicode eksi, sonlu sayı tire ile yazılıyordu → tek biçim ('∞', '-∞'); AR/CLR tablosu başlığı 'klasik hata varsayımı' der, boş kümede uyarı yazılır; sonuç değişir mi: hayır (yalnız yazım).
