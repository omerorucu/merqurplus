# MerQuR+ 3.9.0 — değişiklik listesi (2026-11-22)

> "Çalışmadan, yorulmadan, üretmeden rahat yaşamak isteyen toplumlar; önce haysiyetlerini sonra hürriyetlerini ve daha sonra da istiklal ve istikballerini kaybetmeye mahkûmdurlar."
> — Mustafa Kemal Atatürk

MerQuR+, MerQur 2.9.0 temel sürümü üzerine kurulu, lisans anahtarıyla çalışan bir analiz ortamıdır. Bu sürüm temel sürümü 2.7.0'dan 2.9.0'a taşır; şablon sayısı 226 olur. **Önceki sürümdeki şablonların sonuçları değişmez.**

#### Temel sürüm
- MerQur 2.9.0: finans ve iktisatta 15 yeni analiz (nedensellik, ek birim kök testleri, kırılmalı eşbütünleşme, tahmin karşılaştırma, yayılım ve GFEVD, panel ek testleri, dayanıklı standart hatalı regresyon, Oaxaca-Blinder, trend ve döngü süzgeçleri, eşitsizlik ve yoksulluk, olay çalışması, Fama-MacBeth, portföy optimizasyonu, yatırım performansı, riske maruz değer ve beklenen kayıp) ve genişletilmiş GARCH ailesi. Analiz sayısı 160, kategori sayısı 20.
- MerQur 2.9.0: klinik ve epidemiyolojik raporlamada 10 yeni analiz (Tablo 1, oranlarda eğilim, toplamsal etkileşim, atfedilebilir oran, landmark sağkalım, RMST farkı, Peto meta-analizi, kümülatif meta-analiz, kalibrasyon ve karar eğrisi, minimizasyon); analiz sayısı 170, kategori sayısı 21.
- Panel ek testlerde Maddala-Wu (Fisher-ADF) p değeri için MacKinnon (1996) seçeneği (varsayılan MacKinnon 1994). `panel_ek_testler` şablonundaki "Maddala-Wu: ADF p değeri yaklaşımı" seçeneğiyle aynı değeri verir (R plm ile bağıl fark 5e-7 altında). Katsayılar yazarın yazılı izniyle kullanılmıştır.
- Güven düzeyi seçimi, arka planda çalışan hesaplar ve ilerleme penceresi yeni analizlerde kullanılır.

#### Yeni şablonlar
- **Koşullu aşırı değer VaR/ES** (`risk_var_es`): GARCH ve GARCH-POT tabanlı riske maruz değer ve beklenen kayıp (sGARCH, gjrGARCH, eGARCH; normal, Student-t, çarpık Student-t), Kupiec, Christoffersen ve Basel sınaması, McNeil-Frey ve Acerbi-Szekely ES testleri.
- **Bulanık doğrusal ve doğrusal olmayan regresyon** (`bulanik_regresyon`): Tanaka, Diamond, EKMS ve Kim-Bishu yöntemleri yan yana.
- **Eşikli regresyon ve çok değişkenli eşikli VAR** (`esik_regresyon_tvar`): Hansen (2000) ve Tsay (1998).
- **Panel ek testler** (`panel_ek_testler`): panel birim kök, Mundlak/Chamberlain, panel araç değişkenleri, Driscoll-Kraay, çift kümelenmiş standart hata, PCSE/FGLS, Hausman, Fisher-Johansen; on beş test, her biri onay kutusuyla.
- **Birim kök ailesi** (`birim_kok_ailesi`): Phillips-Perron, KPSS bantları, Fourier ADF ve Fourier KPSS.
- **Eşbütünleşme** (`esbutunlesme_kirilmali`): Engle-Granger regresyonu, Johansen belirtimi ve rank, VECM etki-tepki ve varyans ayrışımı.
- **Fourier Toda-Yamamoto nedensellik** (`nedensellik_ailesi`).
- **Şablon sayısı 226** (196 R, 30 Python), 25 kategori (yeni: Finans), 3 340 ayarlanabilir parametre.

#### Tanım kararları (yeni şablonlarda; önceki sürümde karşılığı yoktur)
Tanımı kaynaklarda ayrışan ayrıntılar için varsayılan ilgili makale ya da ders kitabındaki tanımdır; kaynağın belirlemediği yerde seçenek kullanıcıya bırakılır ve şablon açıklamasında belirtilir. Başlıcaları:
- Eşikli regresyonda sup-F bootstrap'ında bağımlı değişken N(0,1) çekimleridir, açıklayıcılar sabit tutulur (Hansen 1996, 1997); TVAR sup-LR ölçütü ln|Σ̂(γ)|, kalıntı bootstrap'ı boş hipotez VAR'dandır (Tsay 1998; Hansen ve Seo 2002).
- Panel birim kökte Maddala-Wu p değeri için MacKinnon (1994) varsayılandır; MacKinnon (1996; urca) seçilebilir.
- Bulanık regresyonda Diamond d² uzaklığı ve Kim-Bishu ölçütü kullanılır; doğrusal olmayan uyumda a + b/x işlevi dikdörtgen hiperbol olarak adlandırılır (Ratkowsky, 1990). Ortak uzaklık, alt ≤ merkez ≤ üst cezası ve kapsama oranı için kaynak bulunamamıştır, bunlar şablon seçimidir ve açıklamada belirtilir.
- Tohum alanı 0 ya da boş ise oturum tohumu kullanılır; bootstrap ve Monte Carlo sonuçları R ve Python üreteçleriyle birebir tutmaz, örnekleme hatası içinde uyuşur.

#### Çekirdekle örtüşen yöntemler
Temel sürümde bulunan yöntemler için ayrıca şablon verilmez; şablonlar yalnızca temel sürümde olmayan yöntemleri içerir (ör. Fourier Toda-Yamamoto, Fourier ADF/KPSS, McNeil-Frey ve Acerbi-Szekely testleri).

#### Düzeltme
- Zivot-Andrews kırpma oranı açıklaması (`kirilmali_birim_kok`) tanımla uyumlu olacak biçimde yazıldı; hesap değişmedi.
- Üç şablonun açıklamasında temel sürüme yapılan gönderme yalın biçimde ('MerQur') yazıldı.
