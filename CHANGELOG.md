# MerQuR+: Değişiklik Günlüğü

## 3.7.0 (2026-11-16) — ilk sürüm

> "Çalışmadan, yorulmadan, üretmeden rahat yaşamak isteyen toplumlar; önce haysiyetlerini sonra hürriyetlerini ve daha sonra da istiklal ve istikballerini kaybetmeye mahkûmdurlar."
> — Mustafa Kemal Atatürk

MerQuR+, ücretsiz MerQur 2.7.0 temel sürümü üzerine kurulu, lisanslı bir analiz ortamıdır. Ücretsiz MerQur 2.7.0'ın bütün analizlerini ve düzeltmelerini içerir; bunlara doğrulanmış analiz şablonları ve Python/R ile kod çalıştırma ekler. Sondaki büyük R, şablonların R ile çalışmasından gelir.

#### Şablonlar
- **218 doğrulanmış şablon, 24 kategori** (190 R, 28 Python). Her şablonun parametre formu, örnek verisi ve örnek ayarları vardır. Şablonlar yayımlanmış kaynaklarla, eşdeğer bir yordam varsa SAS ile karşılaştırıldı; SAS karşılığının kodu da gösterilir. "Şablon olarak kaydet…" ile kişisel şablon eklenir; şablon ağacında arama kutusu vardır. Öne çıkanlar:
  - ekonometri: ARDL/NARDL, kırılmalı birim kök, ikinci kuşak panel, yapısal VAR, genişletilmiş GARCH, stokastik sınır analizi, veri zarflama analizi, kesikli seçim;
  - klinik: klinik tahmin modeli, cezalı Cox, RMST, çapraz geçiş, biyoeşdeğerlik, parametrik (binormal) ve ROC-GLM ROC, gruplara göre referans aralığı;
  - orman ağacı ıslahı ve nicel genetik: orijin–döl denemesi, kalıtım derecesi ve genetik korelasyon, seleksiyon indeksleri, QST–FST, tohum bahçesinde çeşitlilik, klon ve çok lokasyonlu denemeler;
  - genetik: dizi verisiyle AMOVA, MrBayes ile Bayesçi filogeni, DNA barkodlama, dizi hizalama;
  - tohum tescili (DUS ve VCU/TDÖ): COYD, COYU, off-type, GAIA uzaklığı, Lokasyon × Yıl birleşik tesadüf blokları;
  - ekoloji: topluluk ağırlıklı ortalamalar ve çevresel gradyan modelleri, PERMANOVA ve PERMDISP, doluluk ve yakala-yeniden yakala modelleri;
  - zaman serisi: çok fraktallı DFA (MFDFA), fraktal boyut ve Hurst üssü kestiricileri, joinpoint ve kesintili zaman serisi;
  - diğer: oran meta-analizi, RI-CLPM, dialel, karışım deneyleri, uzay-zaman krigingi, çok değişkenli kontrol kartı, yapısal konu modeli (STM), küme örneklemesi.
- **Parametre ayarları penceresi.** Yöntemin anlamlı bütün seçenekleri Değişkenler, Model, Seçenekler, Gelişmiş ve Çıktı sekmelerinde ayarlanır; **Tamam ve çalıştır** analizi bu ayarlarla çalıştırır.
- **Çok dilli şablon çıktısı.** Metin çıktıları, tablo sütun adları ve hücreleri, kod yorumları ve SAS karşılıkları Türkçe, İngilizce ve İspanyolca arayüzde o dilde gösterilir; ondalık ayırıcı dile uyar.
- **MrBayes 3.2.9 birlikte gelir** (GNU GPL v3, ayrı program; lisans ve kaynak kodu kurulum klasöründe).

#### Kod sekmesi
- Gömülü Python 3.13 ve bilgisayardaki R 4.5.x ile kod çalıştırma; isteğe bağlı olarak kendi Python yorumlayıcınız. Etkin veri `df` olarak gelir. Tablo, grafik ve metin çıktıları Sonuçlar, Tablo, Grafik ve Rapor sekmelerine kart olarak düşer, DOCX raporuna koduyla girer ve `.mqr` projesiyle kaydedilir.
- Kod ayrı işçi süreçte çalışır: İptal 1 saniyede durdurur, zaman sınırı ayarlanabilir, işçi çökse de ana pencere ayakta kalır. Sözdizimi hatası satırında vurgulanır; eksik pakette kurulum önerisi gelir; geçmiş listesi vardır.
- **`mq` modülü:** MerQur analiz motorları koddan çağrılır; `mq.veriye_yaz` yeni sütunları ya da süzülmüş satırları onay penceresiyle Veri sekmesine yazar ve geri alınabilir.
- **`.mqs` kod adımı:** arayüzde her kod adımı için onay istenir; başsız çalıştırmada kod adımları yalnız `--kod-izin` ile çalışır.

#### Yazılım ve atıflar
- Her kod ve şablon koşusunda MerQuR+, R/Python ve kullanılan paketler sürümleri ve resmî atıflarıyla listelenir (MrBayes çalıştıysa o da). Bölüm, kod eki kapalıyken de raporda kalır. `.mqs` betiği farklı paket sürümleriyle yeniden çalıştırılırsa uyarı verilir.
- Atıf: Örücü, Ö. K. (2026). *MerQuR+: Bütünleşik Akademik Veri Analizi ve Raporlama Platformu* (Sürüm 3.7.0; temel sürüm 2.7.0) [Bilgisayar yazılımı]. https://doi.org/10.53463/merqur.2026001

#### AI Yorumlayıcı
- Kendi API anahtarınızla sonuç ve rapor yorumu. Veri hücreleri gönderilmez; yapay zekâ sonuçları hesaplamaz ya da değiştirmez.

#### Lisans, kurulum ve güncelleme
- Kişiye özel, bir yıllık, çevrimdışı doğrulanan lisans anahtarı. Lisans sahibinin adı ve kurumu Hakkında penceresinde ve rapor künyesinde görünür. MerQuR+ Son Kullanıcı Lisans Sözleşmesi geçerlidir; ticari kullanım serbesttir.
- Ücretsiz MerQur ile aynı bilgisayarda **yan yana** çalışır: ayrı klasör, ayarlar, son dosyalar listesi ve simge. İlk açılışta ücretsiz sürümün dil, tema, yazı boyutu, grafik DPI ve AI sağlayıcı ayarları bir kez aktarılır.
- Program kendiliğinden ağa çıkmaz. **Yardım → Güncellemeleri Denetle** yalnız tıklanınca çalışır.
- Kurulum dosyası `MerQuRPlus-3.7.0-windows-x64-Setup.exe`; kurulum dosyası ve program Ömer Kamil Örücü adına kod imzalıdır.

İletişim: info@sekizgenacademy.com
