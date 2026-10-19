# UDE (Uyap Doküman Editörü) — macOS Apple Silicon kurulumu

> **Kısa yanıt:** UYAP Doküman Editörü'nü **MacBook, MacBook Pro, MacBook Air, Mac mini ve iMac**'de (Apple Silicon / M serisi / ARM tabanlı işlemci) Rosetta olmadan, hızlı ve Retina'da keskin çalıştırmak istiyorsanız bu depo gayriresmî bir arm64 derleme sunar: gömülü Java 11, `.udf` çift tıklama, e-imza ve Türkçe PDF harf düzeltmeleri. **Resmî kurum uygulaması değildir.**
>
> **Önce resmî sürümü deneyin:** UYAP'ın resmî [Doküman Editörü 5.4.21+](https://www.uyap.gov.tr/Uyap-Editor) sürümünde Apple Silicon (ARM64) paketi yayımlandı. Resmî paketle işiniz yürüyorsa onu kullanın; bu derleme, resmî pakette karşılaşılan sorunlar ve ek düzeltmeler için alternatiftir.

## İlk kez kuranlar için

1. **Önce resmî paket:** [uyap.gov.tr/Uyap-Editor](https://www.uyap.gov.tr/Uyap-Editor) → Apple Silicon (ARM64) sürümü. Kurulum ve imza burada çalışıyorsa ek araç gerekmez.
2. **Bu derleme:** [Releases](../../releases) → `UdeMacArm64-arm64-*.dmg` (en hızlı) veya `brew install --cask adeministratorr/tap/ude-mac-arm64`.
3. **Kart sürücüsü:** İmzalama için arm64 AKİS sürücüsü gerekir: [akiskart.bilgem.tubitak.gov.tr/destek](https://akiskart.bilgem.tubitak.gov.tr/tr/destek/) → "Mac OS Arm (Apple Silicon)". Kontrol: `lipo -archs /usr/local/lib/libakisp11.dylib` → `arm64`.
4. **Başarılı kurulum:** `.udf` dosyası çift tıklamayla açılır; imzalama akışında kart ve PIN penceresi görünür.
5. **Sık hata:** "Akıllı Kart Bulunamadı" → arm64 sürücü eksik. "Geliştirici doğrulanamadı" → DMG notarize olduğu için normal açılır; kaynaktan derlemede sağ tık → **Aç**.
6. **Önemli sınır:** Build, resmî UDE paketini uyap.gov.tr'den indirip yamalar; resmî paket değişince yama güncellenmeyebilir. İmza PIN'inizi hiçbir komuta/issue'ya yazmayın.

> **Güvenlik notu:** Bu depo kaynak kod içermez; yalnızca yama/build betikleri barındırır. DMG sürümü Developer ID ile imzalı ve Apple notarizasyonundan geçmiştir; kaynaktan derleme (`kur.sh` / `make sign`) ad-hoc imzalıdır. Betiği çalıştırmadan önce inceleyin.

**Kimler için:** UYAP evrakı hazırlayan avukatlar, bilirkişiler, arabulucular; dava dilekçesi, tutanak, rapor ve her türlü `.udf` belgesini yazan tüm UYAP kullanıcıları.

## Kimler için?
- **Avukatlar:** Dava ve cevap dilekçelerini `.udf` olarak hazırlayanlar — "Mac'te UYAP Editör açılmıyor / bulanık" sorununu çözer.
- **Bilirkişiler ve arabulucular:** Rapor ve tutanak yazanlar.
- **İcra müdürlükleri ve kamu çalışanları:** UYAP evrakı üreten kullanıcılar.
- **Hukuk büroları:** MacBook (M1/M2/M3/M4) ve iMac ile UYAP evrak hazırlığı yapanlar.

## "Mac'te UYAP açılmıyor" arayanlar için kısa cevaplar
- **Resmî UDE Mac'te neden yavaş/bulanık?** Resmî paketin eski sürümleri Intel (x86_64) + Java 8 + Rosetta ile çalışıyordu; Java 8 Swing Retina'da bulanık render ediyordu. 5.4.21+ sürümünde Apple Silicon paketi de yayımlandı; onu da deneyebilirsiniz. Bu derleme native arm64 + Java 11'dir; ayrıca e-imza, Türkçe PDF harfleri ve Mac kısayolları düzeltmelerini içerir.
- **MacBook / MacBook Pro / MacBook Air / iMac / Mac mini'de çalışır mı?** Evet — Apple Silicon (M1/M2/M3/M4). Intel Mac'ler için mimari otomatik algılanır (aşağıda).
- **Java kurmam gerekir mi?** Hayır; Java uygulamaya gömülüdür.
- **e-imza çalışır mı?** Evet — akıllı kart okuyucu algılaması düzeltildi; imzalama akışı çalışıyor (arm64 AKİS sürücüsü gerekir).
- **`.udf` çift tıklama çalışır mı?** Evet; UDE `.udf` ilişkilendirmesiyle kurulur.

## Kurulum (3 yol)
1. **Hazır DMG (en hızlı):** [Releases](../../releases) → `UdeMacArm64-arm64-*.dmg`.
2. **Homebrew cask (hazır DMG, derleme beklemez):**

```bash
brew install --cask adeministratorr/tap/ude-mac-arm64
```

3. **Tek satır (kaynaktan derler; betiği önce inceleyin):** aşağıdaki "Kolay kurulum" bölümü.

Diğer araçlar (aynı tap): [e-Devlet E-İmza](https://github.com/adeministratorr/edevlet-eimza-mac-arm64) · [PTT KEP Webmail](https://github.com/adeministratorr/pttkep-eimza-mac-arm64) · [PTT UETS / e-Tebligat](https://github.com/adeministratorr/uets-eimza-mac-arm64) · [TNB Teknoloji İmza](https://github.com/adeministratorr/tnb-eimza-mac-arm64) · [UHAP İmzalama](https://github.com/adeministratorr/uhap-eimza-mac-arm64) · [UDF QuickLook](https://github.com/adeministratorr/udf-quicklook-extension)

---

## UDE — Apple Silicon (arm64) Native

[Uyap Doküman Editörü](https://uyap.gov.tr)'nü (UDE) Apple Silicon (M1/M2/M3/M4)
Mac'lerde **Rosetta olmadan** native çalıştırır. Rosetta çeviri katmanı olmadığı için
**daha hızlı** açılır ve çalışır. Java gömülü gelir; ayrıca bir şey kurmaya gerek yoktur
ve `.udf` dosyalarına **çift tıklayarak** açabilirsiniz.

> 💻 **Intel Mac'iniz mi var?** Aşağıdaki tüm mimari-bağımsız iyileştirmeler (keskin metin,
> modern ikonlar, Mac kısayolları, native pencereler, e-imza, PDF Türkçe harf) Intel'de de
> geçerli. Kurulum komutu için bkz. **[Intel işlemcili Mac'ler için](#-intel-işlemcili-macler-için)**.

![UDE — modern Material ikonlar ve Retina'da keskin metin](assets/ekran-goruntusu.jpeg)

> Modern Material ikonlar + Java 11 HiDPI ile **Retina'da keskin** metin ve arayüz.

### Önce / Sonra

Aynı belge, aynı Retina ekranda. **Solda** resmî paket (Rosetta + Java 8: tüm pencere
bulanık); **sağda** bu derleme (native + Java 11: metin keskin, modern düz Word-2026
görünümü ve koyu mod).

| Öncesi — resmî paket (bulanık) | Sonrası — bu derleme (keskin, koyu) |
|:---:|:---:|
| ![Önce: bulanık, eski turkuaz görünüm](assets/before-blurry.png) | ![Sonra: keskin, koyu Word-2026 görünümü](assets/after-word2026.png) |

> Modern görünüm, sistem teması neyse onu takip eder — açık modda da aynı düz Word-2026 arayüzü:

![Açık modda Word-2026 görünümü](assets/after-word2026-light.png)

> ⚠️ **Bu depo UYAP Doküman Editörü'nün kaynak kodunu içermez.** Tamamen bağımsız,
> **gayriresmî** bir Mac **yamasıdır**: hiçbir kamu kurumu tarafından
> geliştirilmemiş/onaylanmamıştır. Burada bulunan yalnızca yama ve build betikleridir;
> resmî UDE paketi build sırasında uyap.gov.tr'den **siz** indirir ve yamayı uygulamanın
> üstüne **siz** çalıştırırsınız. "Olduğu gibi" sunulur.

> ✅ **E-imza çalışıyor:** Akıllı kart okuyucu algılaması (`5.4.17_4`+) çözüldü —
> gömülü Java artık PCSC üzerinden kartı görüyor ve imzalama akışı baştan sona
> çalışıyor. Belge açma/düzenleme de sorunsuz.

> 🗂️ **Native macOS dosya pencereleri:** Aç / Kaydet / Farklı Kaydet artık eski
> görünümlü Java penceresi yerine macOS'un **kendi** dosya penceresini kullanır —
> Finder kenar çubuğu, iCloud Drive, son kullanılanlar ve `.udf` filtresiyle.

![UDE — native macOS Aç/Kaydet penceresi](assets/native-dosya-penceresi.png)

> Aç/Kaydet pencereleri artık macOS'un native dosya seçicisi.

> 📦 **Hazır paket bu sürümde var:** İster yukarıdaki tek satırla kaynaktan derleyin,
> ister hazır arm64 DMG'yi kullanın: [Releases](../../releases) → `UdeMacArm64-arm64-*.dmg`,
> ya da Homebrew ile: `brew install --cask adeministratorr/tap/ude-mac-arm64`.
> Orijinal upstream depo hazır paket dağıtmaz; bu dağıtım sürümü kurulumu kolaylaştırmak
> için hazır paket de sunar.

## Neden Mac'te çalışmıyordu? Nasıl çözdük?

- **Sorun:** Resmî UYAP Doküman Editörü paketi Mac'te Rosetta ile çalışıyordu: açılış yavaş, metin bulanık; e-imza kartı, dikte, pano ve Türkçe PDF harfleri gibi birçok akış bozuktu.
- **Neden:** Resmî paket Intel (x86_64) + Java 8 tabanlıydı; eski `com.apple.eawt` API'si Java 11'de kaldırıldı; `sqlite-jdbc 3.7.2`'nin arm64 native'i yoktu; gömülü Java, macOS'ta PCSC kart kütüphanesini varsayılan yolda bulamıyordu.
- **Çözüm:** Native arm64 launcher + gömülü **Java 11** (Retina keskinlik), `eawt-shim` (dosya çift-tık davranışı), `sqlite-jdbc 3.46` sürümüne geçiş, PCSC yolunun `Info.plist` → `LSEnvironment` ile uygulamaya gömülmesi ve 20+ iyileştirme (modern ikonlar, native macOS dosya pencereleri, Türkçe PDF fontları, dikte/pano yamaları…). Ayrıntılar aşağıdaki teknik bölümde.
- **Doğrulama:** Akıllı kart okuyucu algılaması + e-imza akışı ve belge açma/düzenleme uçtan uca çalışıyor.

---

## 👩‍⚖️ Kolay kurulum — tek satır

Programcı olmanıza gerek yok. **Terminal** uygulamasını açın (klavyede
`Command (⌘) + Boşluk`'a basıp açılan kutuya **Terminal** yazın ve **Enter**'a basın),
ardından aşağıdaki **tek satırı** kopyalayıp yapıştırın ve **Enter**'a basın:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/adeministratorr/ude-mac-arm64-1/main/kur.sh)"
```

> Terminal'iniz Rosetta (x86_64) modunda açılmışsa betik bunu algılayıp kendini
> otomatik olarak arm64'e geçirir — artık `arch -arm64` yazmanıza gerek yoktur.

Hepsi bu kadar. Manuel indirme, klasöre girme, Java kurma gibi adımlar **yok**. Bu komut
gerisini sizin için yapar:

- Gerekiyorsa **geliştirici araçlarını** (Xcode komut satırı araçları) kurar — bir pencere
  açılırsa yalnızca **"Yükle"**ye basıp bitmesini bekleyin, betik kendiliğinden devam eder.
- **Kaynak kodu** `~/ude-mac-arm64` klasörüne indirir (zaten varsa en güncel sürüme günceller).
- Gereken **Java** sürümlerini otomatik indirir.
- Uygulamayı modern ikonlarla **derler + imzalar** ve doğrudan **/Applications** klasörüne kurar.

İlk derleme internet hızınıza göre birkaç dakika sürebilir.

Bittiğinde uygulama **Launchpad** ve **Applications** klasöründe hazırdır; çift tıklayarak
ya da `.udf` dosyalarına çift tıklayarak açabilirsiniz. (Kendiniz derleyip imzaladığınız
için macOS "geliştirici doğrulanamadı" uyarısı **çıkmaz**; `xattr` ile uğraşmanıza gerek
yoktur.)

**Yeni Editör sürümü çıktığında yukarıdaki tek satırı yeniden çalıştırmanız yeterli. En
güncel sürüm otomatik inecek ve yamalanacak.**

### 🎨 Eski (klasik) arayüzü kurmak isterseniz

Yukarıdaki komut varsayılan olarak **modern düz görünümü** (10. maddede anlatılan) kurar.
Eski turkuaz arayüzü tercih ediyorsanız, aynı satırın başına `SKIN=0 ` ekleyin:

```bash
SKIN=0 arch -arm64 bash -c "$(curl -fsSL https://raw.githubusercontent.com/adeministratorr/ude-mac-arm64-1/main/kur.sh)"
```

Depoyu zaten indirdiyseniz, klasörün içinde `./kur.sh --klasik` de aynı işi yapar.

## 💻 Intel işlemcili Mac'ler için

Mac'iniz Intel (x86_64) işlemcili ise — yani Apple Silicon (M1/M2/M3/M4) **değilse** —
aynı kurulumu **birebir aynı tek satırla** yapın (mimariyi betik kendisi algılar):

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/adeministratorr/ude-mac-arm64-1/main/kur.sh)"
```

> Mimarinizden emin değil misiniz? Sol üstteki **Apple () menüsü → Bu Mac Hakkında**'ya
> bakın: **"Apple M…"** yazıyorsa üstteki Apple Silicon komutunu, **"Intel"** yazıyorsa
> bu komutu kullanın. (Betik mimariyi kendisi de algılar; yanlış komutu çalıştırırsanız
> sizi uyarır.)

Intel'de resmî UDE paketi zaten x86_64 olduğu için burada kazanç "Rosetta'sız native hız"
**değildir**; kazanç şunlardır: **Java 11 ile Retina'da keskin metin**, **modern Material
ikonlar**, **Mac klavye kısayolları**, **native macOS Aç/Kaydet pencereleri**, **trackpad /
⌘ ile yakınlaştırma**, **e-imza (akıllı kart) düzeltmesi**, **panodan görsel yapıştırma**,
**macOS dikte düzeltmesi** ve **PDF dışa aktarımında Türkçe harfler**. Yani mimariye bağlı olmayan tüm iyileştirmeler
Intel'de de geçerlidir.

### E-imza kullanacaksanız — AKİS sürücüsü (zorunlu)

Akıllı kart / e-imza sürücünüzün de Mac'inizin mimarisine uygun sürümünün kurulu olması
gerekir. TÜBİTAK BİLGEM AKİS — Destek/İndirme sayfasından
(<https://akiskart.bilgem.tubitak.gov.tr/destek/>):

- **Apple Silicon (M1/M2/M3/M4):** **"Mac OS Arm (Apple Silicon)"** başlığı altındaki
  güncel paketi indirin (ör. `Akia_macos_arm_6_8_9.pkg`).
- **Intel:** **"Mac OS Intel"** başlığı altındaki paketi indirin.

İndirilen `.pkg`'a çift tıklayıp kurulumu tamamlayın (yönetici şifresi ister). Mac'inizin
mimarisine **uymayan** paketi seçmeyin.

> **macOS 28 / Rosetta uyarısı:** Sistemde eski **Intel** AKİS paketi kuruluysa macOS, `/Applications/Akia.app` için "bu uygulama gelecek macOS sürümünde açılmayacak" uyarısı gösterir (Rosetta desteği macOS 28'de kaldırılıyor). TÜBİTAK'ın **Arm (Apple Silicon)** paketini kurduysanız yeni sürüm `Akia ARM.app` (arm64) olarak gelir; eski `/Applications/Akia.app`'i Finder'da Çöp'e sürükleyip kaldırın. Bu uyarı imzalama akışını etkilemez — kart erişimi yukarıdaki `libakisp11.dylib` üzerinden yürür.

### ⌨️ Klavye kısayolları

Bu derlemede kısayollar Mac'e uyarlanmıştır (kalın `⌘B`, kaydet `⌘S`, hizalama
`⌘L/E/R/J`, yazı boyutu `⌘⇧.`/`⌘⇧,` …). Tam liste için bkz. **[KISAYOLLAR.md](KISAYOLLAR.md)**.

---

## 🛠️ Mühendisler için — Teknik ayrıntı

Yukarıdaki adımlar derlemek için yeterlidir. Bu bölüm, dönüşümün **neyi nasıl** çözdüğünü
ve tek tek build hedeflerini açıklar.

## Neyi nasıl çözüyor

Build, çalıştığı Mac'in mimarisini `uname -m` ile **otomatik algılar** (Apple Silicon=arm64,
Intel=x86_64) ve gömülecek Java ile launcher'ı o mimari için üretir. Aşağıdaki çözümler
arm64'ün özgün gerekçesiyle anlatılır; **yalnızca 1, 4 ve 5** maddeleri (launcher, sqlite,
JNA) mimariye özgüdür — **geri kalan tüm maddeler (2, 3 ve 6–20) mimariden bağımsızdır**
ve Intel'de de aynen geçerlidir. Madde 4 (sqlite) arm64'te zorunluydu (3.7.2'nin arm64
native'i yoktu); Intel'de 3.7.2 zaten x86_64 native içerir ama tek tip build için 3.46.x'e
geçiş orada da uygulanır (jar hem `Mac/aarch64` hem `Mac/x86_64` dylib taşır).

Resmî paket x86_64. Native arm64 için:

1. **Launcher** → `jpackage` ile arm64 **Java 11** runtime **gömülü**, gerçek native
   launcher üretilir. Kullanıcı Java kurmaz; macOS çift-tık (dosya açma) çalışır.
2. **Retina/keskin metin** → arm64 **Java 8** Swing'i Retina'da bulanık render ediyordu.
   **Java 11** (JEP 263 otomatik HiDPI) ile metin keskin. UDE'nin Java 8 bytecode'u
   Java 11'de çalışır (WebLaF illegal-access uyarıyla geçer).
3. **eawt-shim** → UDE'nin kullandığı eski `com.apple.eawt` API'si Java 11'de kaldırılmış.
   `scripts/eawt-shim` ile bu sınıflar `--patch-module java.desktop` üzerinden sağlanır;
   dosya açma, Java 11'in native dispatcher'ına reflection ile köprülenir → çift-tık korunur.
4. **sqlite-jdbc 3.7.2** (arm64 native'i yok) → **3.46.x** ile değiştirilir.
5. **JNA** → uygulama JNA'yı hiç çağırmıyor (bytecode taramasıyla doğrulandı), dokunulmaz.
6. **Modern ikonlar** (`ICONS=1`) → UDE'nin ~324 toolbar/aksiyon ikonu modern **Material**
   setiyle değiştirilir ve **Retina-keskin** yapılır. UDE ikonları düz `ImageIcon` olarak
   yüklenip HiDPI-farkında olmadığından, yükleyici (`Utils.b`) Javassist ile
   `BaseMultiResolutionImage`'a (1x + `@2x`) köprülenir. Override görseller
   `scripts/icons/overrides`, yama `scripts/icons/IconLoaderPatch.java`. Yayınlanan
   sürümler bu modu açık derlenir.

7. **E-imza** (`5.4.17_4`+) → JDK'nın `javax.smartcardio` + `sun.security.pkcs11`
   API'leriyle çalışır (JNA değil). Gömülü Java'nın `javax.smartcardio` katmanı macOS'ta
   PCSC native kütüphanesini varsayılan yolda bulamadığından kart okuyucu görünmüyordu.
   Çözüm `-Dsun.security.smartcardio.library=/System/Library/Frameworks/PCSC.framework/Versions/A/PCSC`
   ile bu yolu vermek; ancak `jpackage`'ın `.cfg` java-option'ları çift-tıkla açılan
   launcher'da bu JVM'e ulaşmıyordu (kullanıcı `lsof` ile doğruladı: framework yüklenmiyordu).
   Bu yüzden parametre, JVM'in her zaman okuduğu `JAVA_TOOL_OPTIONS` ortam değişkenine,
   `.app`'in `Info.plist`'indeki `LSEnvironment` (Launch Services) anahtarıyla gömülür →
   çift-tık açılışta garanti uygulanır. (`Versions/Current` symlink yerine kanıtlanmış
   `Versions/A` yolu kullanılır.)

8. **Trackpad ile yakınlaştırma** → UDE'de zoom yalnızca durum çubuğundaki kaydırıcıyla
   yapılabiliyordu; trackpad jesti yoktu. `scripts/macos-zoom` javaagent'ı **⌘ + iki parmak
   kaydırma** jestini bu kaydırıcıya bağlar: Cmd basılıyken gelen `MouseWheelEvent` yakalanıp
   yutulur (belge kaymaz) ve uygulamanın zoom kaydırıcısı sürülür → belge büyür/küçülür.
   Gerçek iki-parmak *pinch* jesti modern Java'ya iletilmediğinden ⌘ tuşu gerekir; ⌘'süz
   kaydırma belgeyi normal kaydırır. Olayı yutabilmek için sistem `EventQueue`'su **ilk odak
   olayında** devralınır (premain'de erken devralma WebLaF tarafından baypas edilir).
   Ayrıca **`⌘+` / `⌘−`** klavye kısayolları da aynı kaydırıcıyı sürer (bir `KeyEventDispatcher`
   ile); macOS'un standart yakınlaştır/uzaklaştır tuşları beklendiği gibi çalışır.

9. **Standart Mac kısayolları** → UDE'nin editörü Windows kökenli; kısayollar Ctrl tabanlı ve
   bir kısmı alışılmadık tuşlarda (kalın `Ctrl+K`, italik `Ctrl+T`, altı çizili `Ctrl+Shift+A`,
   bul `Ctrl+B`). Üstelik macOS, metin bileşenlerine yerleşik **Emacs imleç bağlamaları** ekler
   (`Ctrl+A` satır başı, `Ctrl+B` harf geri, `Ctrl+N/P/O/F`, `Ctrl+V` sayfa aşağı…) ve bunlar
   uygulamanın komutlarından önce çalışır → sentetik bir `Ctrl+A` "tümünü seç" değil "satır başı"
   yapar. `scripts/macos-textkeys`'teki `MacShortcutRemap` (bir `KeyEventDispatcher`) bu yüzden
   üç katmanlı çalışır:
   - **Menü-etiketi**: Menüde karşılığı olan komutlar için odaktaki pencerenin menü ağacında
     etiketle eşleşen etkin `JMenuItem` bulunup `doClick()` edilir. Bu, uygulamanın **gerçek**
     eylemini (zengin-metin yapıştırma vb.) çağırır ve odak bileşenini kullanmadığından Emacs
     gölgesini tamamen baypas eder. (Menü öğelerinin hızlandırıcısı yoktur; eşleme etiketledir.)
   - **Metin API'si**: Seç/kopyala/kes/yapıştır için yedek olarak doğrudan `JTextComponent`.
   - **Sentetik Ctrl**: Menüde olmayan biçimlendirme (kalın/italik/altı çizili) için odaktaki
     bileşene sentetik Ctrl gönderilir (uygulama bu tuşları Emacs'in üzerine ezdiğinden çalışır).

   Eşlemeler: `⌘B/⌘I/⌘U`→kalın/italik/altı çizili, `⌘C/⌘V/⌘X/⌘A`→kopyala/yapıştır/kes/tümünü seç,
   `⌘Z/⌘⇧Z`→geri/ileri al, `⌘N/⌘O/⌘S/⌘⇧S`→yeni/aç/kaydet/farklı kaydet, `⌘P/⌘⇧P`→yazdır/önizleme,
   `⌘F`→bul, `⌘T`→yazı özellikleri. Mevcut Ctrl kısayolları aynen çalışır; `⌘Q/⌘W/⌘H/⌘M` gibi
   gerçek macOS kısayollarına dokunulmaz.

10. **Modern düz görünüm** (`SKIN=1`, varsayılan açık) → Menüler, paneller, şerit ve durum
    çubuğu degradesiz nötr renklere geçer; arayüz yazı tipi modernleşir; belge çevresindeki
    eski turkuaz arka plan nötr griye düzeltilir. Şerit düzeni ve belge içeriği değişmez.
    Ek olarak:
    - **Koyu mod**: Mac'iniz koyu görünümdeyse UDE de koyu temayla açılır (belge sayfası
      okunabilirlik için beyaz kalır). Görünümü değiştirdikten sonra uygulamayı yeniden
      başlatmanız yeterlidir.
    - **Bütünleşik başlık çubuğu**: Çifte başlık kalkar; şerit, modern Mac uygulamalarındaki
      gibi pencerenin tepesiyle kaynaşır. Durum çubuğundaki bellek göstergesi kaldırılır.
    - **Sade şerit**: "Pano/Font/Paragraf" grup kutuları ve eski kırmızı küre logosu yerine
      sade gruplar ve düz grafit UDE simgesi kullanılır.
    - Görünüm açılışta garanti kurulur; UDE'nin tema ayarındaki "standart tema" seçeneğinin
      tercihi silmesinden etkilenmez. Kapatmak için `SKIN=0 make all`.

11. **Panodan görsel yapıştırma** → Panoda bir görsel varken (ekran görüntüsü, tarayıcıdan
    "görseli kopyala") Yapıştır/⌘V macOS'ta hiçbir şey yapmıyordu: macOS Java'sı pano
    görselini Retina'ya özel `MultiResolutionCachedImage` tipiyle verir, UDE'nin imaj
    yapıştırma dalı ise `instanceof BufferedImage` denetiminde bu tipi tanımayıp dalı
    sessizce atlar (Windows/Linux'ta tip zaten `BufferedImage` olduğundan sorun görülmez).
    `scripts/macos-pasteimage` build yaması pano görselini güvenli biçimde dönüştürür
    (Retina'da en yüksek çözünürlüklü varyant), yapıştırma anındaki yıkıcı sayfaya-sığdırma
    küçültmesini kaldırır (görünen boyut değişmez, tam çözünürlük gömülür) ve görüntü
    çizimine bicubic interpolasyon ekler → ekranda/PDF'te keskin. Metin önceliği korunur:
    panoda metin de varsa eskisi gibi metin yapışır. Varsayılan açıktır (`PASTEIMG=0` ile
    kapatılır).

12. **Fare ile imaj boyutlandırma** → Belgedeki bir imaja tıkladığınızda köşelerde dört
    tutamaç belirir; Word'deki gibi bir köşeyi sürükleyerek **oran korumalı** yeniden
    boyutlandırabilirsiniz. Sürükleme sırasında kesik çizgili önizleme gösterilir; fare
    bırakıldığında görsel yeni boyutuna oturur. Yeni boyut belgeye yazılır ve UYAP'ın
    resmî editöründe de aynı şekilde görünür. Tek geri alma adımıyla (`⌘Z`) sıfırlanır.
    Varsayılan açıktır (`IMGRESIZE=0` ile kapatılır).
13. **macOS dikte düzeltmesi** → Resmî pakette dikte konuşurken yazıyor ama dikte
    kapanınca yazılan metin **kayboluyor**, ardından uygulama donup zorla kapatma
    gerektiriyordu (yeniden açılışta "düz metin olarak kurtar" uyarısı). Kök neden:
    Java, dikteyle gelen metni tuş basımı gibi işlerken editörün kelime denetimi
    geçici imleç tipine takılıp çöküyordu. Bu derlemede metin bileşenlerine eklenen
    küçük bir dinleyiciyle dikte metni editörün normal yazma yolundan işlenir;
    dikte edilen metin kalıcıdır, donma olmaz. Şapkalı harfler (`^`+a → â) ve
    emoji seçici (⌃⌘Space) etkilenmez.

14. **PDF dışa aktarımında Türkçe harfler** (`FOPFONTS=1`, varsayılan açık) → "PDF Olarak
    Kaydet" ile dışa aktarılan belgelerde `ğ ş ı İ` gibi Türkçe harfler kayboluyordu (sorun
    platformdan bağımsız). UDE'nin gömülü Apache FOP motoru, Türkçe glifleri içermeyen
    base-14 PDF fontlarını Cp1252 kodlamasıyla kullanıyordu; üstelik paketteki `fopconf.xml`
    devrede değildi. Çözüm Javassist ile FOP'a Türkçe destekli bir font yapılandırması
    (`setUserConfig`) enjekte etmek → tüm Türkçe harfler PDF'te doğru çıkar.

15. **Native macOS dosya pencereleri** (`FILEDIALOG=1`, varsayılan açık) → Aç / Kaydet /
    Farklı Kaydet eski görünümlü Java `JFileChooser` penceresini açıyordu. Javassist
    yamasıyla bunlar macOS'un kendi `FileDialog`'una köprülenir → Finder kenar çubuğu,
    iCloud Drive, son kullanılanlar ve `.udf` filtresi. (Sahipli/şeffaf başlık panelinin
    pencereyi görünmez biçimde bloklaması ve pano `⌘V` sorunu native katmanda çözüldü.)

16. **Satır-içi imaj tam çözünürlük** (`IMGFULL=1`, varsayılan açık) → Belgeye eklenen
    görseller, ekleme anında sayfaya-sığdırma için ~600×790 piksele küçültülüp gömüldüğünden
    Retina'da bulanıktı. UDE'nin imaj ekleme yolu (`utils.h.a`) Javassist ile bu yıkıcı
    küçültmeyi yapmayacak şekilde değiştirilir (görsel tam çözünürlükte gömülür, görünen
    boyut değişmez) ve çizimine bicubic interpolasyon eklenir → keskin.

17. **Otomatik düzeltme seçenekleri anında etkin** (`LIVETOGGLE=1`, varsayılan açık) →
    "Otomatik Büyük Harf", "Baş Harfleri Büyüt" ve "Kelime Denetimi" seçeneklerini açıp
    kapatmak resmî pakette **uygulamayı yeniden başlatmayı** gerektiriyordu (seçenek yalnız
    tercihi yazıp "yeniden başlatın" diyordu). Bu derlemede seçenek değiştirildiği anda
    ilgili dinleyiciler tüm açık pencerelere eklenir/sökülür → yeniden başlatma gerekmez.

18. **macOS Metin Değiştirme** (`TEXTREPLACE=1`, varsayılan açık) → **Sistem Ayarları →
    Klavye → Metin Değiştirmeleri** kısayolları (ör. "mrb" → "Merhaba!") UDE'de çalışmıyordu
    (macOS'un metin-denetim kanalı Java uygulamalarına kapalı). Bu derleme kısayol listenizi
    okur ve bir sözcük tamamlandığında (boşluk/Enter/noktalama) eşleşen kısaltmayı belgede
    genişletir. Küçük harfli kısaltma, ilk harfi büyük yazılışla da eşleşir.

19. **Harici stilli yapıştırma** (`PASTERICH=1`, varsayılan açık) → Word, tarayıcı veya
    PDF'den kopyalanan **biçimli** metin UDE'ye düz metin olarak yapışıyordu (UDE stilleri
    yalnızca kendi iç pano biçiminde korur). Bu derleme pano HTML'ini okuyup
    kalın/italik/altı çizili, font/boyut/renk/vurgu, hizalama/girinti ve **liste ile
    tabloları** koruyarak belgeye ekler. (Tablolar yapı kaybı olmadan **gerçek** UDE
    tablosu olarak kurulur; resimler de yapışır.) UYAP içi kopyalama eskisi gibi çalışır.

20. **Antetlerim** (`ANTET=1`, varsayılan açık) → "Arka Plan Resmi Düzenleme" penceresine
    kişisel **antet** bölümü eklenir: bir kez eklediğiniz antet görsellerini listeden seçip
    tek tıkla belgeye basabilirsiniz. Görsel, sayfaya taşmadan **sığdırılarak** (contain-fit,
    300 dpi hedefiyle, büyütmeden) yerleştirilir — resmî yolun görseli doğal boyutuyla gömüp
    taşırması bu sayede önlenir. Antetler `~/Library/Application Support/UDE/Antetler`
    altında saklanır.

21. **Büyük belgelerde donmayan açılış** (`BUFREAD=1`, varsayılan açık) → UDE, bir belgeyi
    açarken (ve her açılışta) dosyayı tamponsuz, **bayt bayt** okuyordu: her bayt için ayrı
    bir sistem çağrısı, üstelik arayüz iş parçacığında. Bu yüzden resimli büyük belgelerde
    uygulama saniyelerce donuyordu. Javassist yamasıyla okuma tamponlu yapılır → 15 MB'lık
    bir UDF ile açılış **15,1 sn'den ~2,5 sn'ye** iner (dosyasız açılışa birkaç onda bir
    saniye yakın).

22. **Kelime Denetimi kapalıyken sözlük yüklenmez** (`ZEMBEREKLAZY=1`, varsayılan açık) →
    UDE, Kelime Denetimi kapalı olsa bile her açılışta Türkçe sözlüğü (Zemberek) yüklüyordu
    (~0,4 sn ve kalıcı ~8 MB bellek). Artık sözlük yalnızca Kelime Denetimi açıkken, ihtiyaç
    olduğunda yüklenir; seçeneği çalışırken açmak eskisi gibi çalışır. Ayrıca sağ tık yazım
    önerileri her seferinde sözlüğü sıfırdan kurmak yerine ortak sözlüğü kullanır.

> Not: macOS codesign, `.app` adındaki Türkçe karakterlerle imzayı bozuyor; bu yüzden
> executable ASCII (`UyapDokumanEditoru`) tutulur, görünen ad sonradan Türkçe yapılır.

## Gereksinimler (yalnızca build için)

- macOS Mac (Apple Silicon **veya** Intel — mimari `uname -m` ile otomatik algılanır)
- **Java 11** (gömülecek runtime, Mac'inizin mimarisi için) — yoksa `make jdk` Azul Zulu 11'i kurar
  - Gömülecek runtime **OpenJDK derlemesi** olmalıdır (Azul Zulu, Temurin, Corretto…).
    Makinenizde **Oracle JDK 11** kurulu olsa bile kullanılmaz: Oracle JDK 11 gömülünce
    uygulama açılıştan hemen sonra çöküyor. Bu durumda `make jdk` sizi uyarır ve
    Azul Zulu 11'i indirir. Hangi runtime'ın gömüleceğini `make check-deps`
    çıktısındaki "sağlayıcı" satırından görebilirsiniz.
- **jpackage'lı 17+ JDK** (jpackage + shim derlemesi) — yoksa `make jpackage-jdk` Azul Zulu 21'i kurar
- `curl`, `unzip`, `zip`, `codesign`, `plutil` (macOS'ta hazır gelir)

> **Güncelleme:** Yeni bir UDE sürümü çıktığında kurulum komutunu (ya da klasörün
> içinden `./kur.sh`) yeniden çalıştırmanız yeterlidir; betik önce kendi kaynak
> kodunu günceller, sonra satıcının güncel paketini indirir ve kurulan sürümü
> ekrana yazar. Klasörde kendi değişiklikleriniz varsa otomatik güncelleme
> atlanır (uyarı verilir). Kurulumunuz çok eskiyse ve güncelleme gelmiyorsa
> temiz kurulum: `rm -rf ~/ude-mac-arm64` + kurulum komutu.

> Build, host mimarisi için derler: Apple Silicon'da arm64, Intel'de x86_64 (çapraz
> derleme yapılmaz; gömülecek Java ve jpackage host mimarisiyle eşleşir).

## Kullanım

```bash
make jdk           # gömülecek Java 11 yoksa kur (host mimarisi)
make jpackage-jdk  # jpackage'lı 17+ JDK yoksa kur
make all           # build/Uyap Doküman Editörü.app üret
ICONS=1 make all            # + modern Material/Retina ikonlar + modern düz görünüm (yayın sürümleri böyle; SKIN varsayılan açık)
SKIN=0 ICONS=1 make all     # modern düz görünümü kapat (eski turkuaz arka plan döner)
```

### Diğer hedefler

```
make help        # tüm hedefler
make check-deps  # araç + Java 11 (host mimarisi) + jpackage denetimi
make download    # paketi indir + kaynağı aç
make deps        # sqlite-jdbc indir + host mimarisi dylib doğrula
make shim        # eawt-shim derle (Java 11 com.apple.eawt yerine)
make patch       # editor-app.jar yamala (sqlite swap + eawt çıkar)
make package     # jpackage ile .app üret (Java 11 + shim, .udf ilişkilendirmeli)
make sign        # ad-hoc codesign (kaynak derleme; DMG release'leri Developer ID + notarize)
make clean / distclean
```

### Yeni UDE sürümü çıkınca

```bash
make distclean
UDE_URL="https://rayp.adalet.gov.tr/.../yeni-paket.zip" make all
```

> Uyarı: İleride paket yapısı (jar/sqlite sürümü) değişirse betiklerin güncellenmesi gerekebilir.

## CI build (isteğe bağlı)

`.github/workflows/release.yml` (elle tetiklenir): macOS arm64 runner'da `make all`
çalıştırır ve `.app`'i imzayı bozmadan zip'ler. Sürüm, UDE sürümünden türetilir:
`<ude_surumu>_<N>` (ör. `5.4.17_1`). Bu yalnızca derlemenin doğrulanması/kişisel kullanım
içindir; bu dağıtım sürümü ayrıca hazır DMG + Homebrew cask sunar (bkz. en üstteki not);
orijinal upstream depo hazır paket dağıtmaz.

> Not: CI yalnızca arm64 runner kullanır; Intel (x86_64) build'i yerelde bir Intel Mac'te
> (`bash kur.sh` veya `make all`) üretilir. İstenirse `macos-13` Intel runner ile ayrı bir
> CI işi eklenebilir.

---

## Teşekkür

Bu çalışmaya ilham veren ve sorunun çözüm yolunu ortaya koyan
[**tosbaha**](https://github.com/tosbaha) kullanıcısına ([tosbaha/uyap-arm](https://github.com/tosbaha/uyap-arm))
teşekkürler.

## Lisans

Bu depodaki paketleme betikleri, yamalar ve belgeler [MIT Lisansı](LICENSE) ile sunulur.
Uyap Doküman Editörü'nün kendisi UYAP / T.C. Adalet Bakanlığı'na ait olup bu lisansın
kapsamı dışındadır; ilgili kullanım koşullarına tabidir.
