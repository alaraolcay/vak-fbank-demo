# VakıfBank Ödeme Platformu

Kurumlara ait ödeme/tahsilat akışlarını tanımlayıp, hem müşteri hem iş birimi
gözünden test edebileceğiniz, gerçek bankacılık mutabakat mantığıyla çalışan tek sayfalık bir
prototip.

##  Canlı Demo
 **[alaraolcay.github.io/vak-fbank-demo](https://alaraolcay.github.io/vak-fbank-demo/)**
Bu depo GitHub Pages ile yayınlanmaktadır — depo ayarlarınızdaki adres üzerinden doğrudan açılır.

##  Proje Hakkında

VakıfBank, fatura/tahsilat aracılığı yaptığı kurumlarla **senkron değil, asenkron** çalışır:
müşteriden para her zaman tahsil edilir, kurumla mutabakat ise **ayrı ve periyodik (günlük)**
bir süreçtir. Bu prototip tam olarak bu modeli simüle eder:

1. **İş Birimi Girişi** — Kurum/panel/alan tanımlama (low-code tasarım), Runtime'da test etme,
   Mutabakat takibi — analistin işini tek ekranda toplar.
2. **Müşteri Girişi** — Gerçek bir bankacılık müşteri deneyimi: ödeme yapma, geçmiş ödemeleri
   görme. Kurumla yaşanan hiçbir arka plan uyuşmazlığı müşteriye asla yansımaz.

Backend, veritabanı veya kurulum gerektirmez — tüm veri tarayıcınızda (`localStorage`) tutulur.

##  Giriş Bilgileri (Demo)

Giriş ekranında iki ayrı giriş türü vardır, örnek kullanıcılar:

| Giriş Türü | Kullanıcı Adı | Şifre |
|---|---|---|
|  İş Birimi | `analist` | `Vakif2026!` |
| Müşteri | `musteri1` | `Vakif2026!` |

(Gerçek bir kimlik doğrulama sistemi değildir, sadece iki deneyimi ayırmak için tarayıcı
tarafında kontrol edilir.)

## Özellikler

### İş Birimi — Tanımlar
- Kurum ekleme/düzenleme/silme/pasife alma
- Sürükle-bırak Alan Tanım editörü: metin/sayı kutusu, tarih seçici, hesap/kart seçimi, koşullu
  görünürlük (örn. "İletişim Tercihi = E-posta" seçilince e-posta kutusu çıkar)
- Panel Sağlık Kontrolü: hangi panelin eksik/taslak/alansız olduğunu gösterir
- İşlem Geçmişi (denetim kaydı): kim, ne zaman, neyi ekledi/değiştirdi/sildi

### İş Birimi — Runtime (test ortamı)
- Kurum ve akış seçip uçtan uca test etme (Borç Sorgulama → Borç Listesi → Ödeme)
- Otomatik Ödeme Talimatı: borç göstermeden, doğrudan ileriye dönük talimat oluşturma
- T.C. Kimlik No sıfırla başlayamama kuralı, telefon numarası otomatik `+90 5XX XXX XX XX` formatı
- Panelleri eksik veya kurum pasifken akış hiç çalıştırılamaz

### Müşteri Girişi
- Sade, gerçek bir bankacılık arayüzü — "Runtime" gibi geliştirici terimleri hiç görünmez
- Ödeme anında yalnızca **banka tarafı** hatalar olabilir (bakiye yetersiz, hesap müsait değil);
  kurumla ilgili hiçbir uyuşmazlık müşteriye asla gösterilmez, para her zaman tahsil edilir
- **Ödeme Geçmişim** ekranı: sadece kendi yaptığı ödemeler, her biri için PDF dekont

### Mutabakat (kurum bazlı, günlük/haftalık/aylık)
- Mutabakat, **tekil işlem değil, kurum + dönem bazlı toplu karşılaştırmadır**: "VakıfBank'ın
  tahsil ettiği toplam" ile "kurumun onayladığı toplam" karşılaştırılır
- Günlük / Haftalık / Aylık görünüm arasında geçiş
- Fark varsa "Farkları Gör" ile o dönemin sadece uyuşmayan işlemlerine inilir
- Her satır için "Detay": VakıfBank tahsilatı – kurum onayı – fark, açık bir eşitlik tablosu
  olarak gösterilir
- **Geçici uyuşmazlıklar** (kurum o an ulaşılamadı/bakımda) otomatik iade başlatmaz — "Yeniden
  Sorgula" ile tekrar denenir
- **Kesin uyuşmazlıklar** (mükerrer ödeme, zaten kapatılmış borç vb.) iade sürecini başlatır —
  ayrı bir **İade Takibi** ekranından tamamlandı olarak işaretlenebilir
- **Eskalasyon**: 3 günden uzun süredir çözülemeyen uyuşmazlıklar otomatik uyarı olarak öne çıkar
- Zaman aralığı filtresi (son 1 saat / 24 saat / 7 gün / özel aralık), Excel/CSV dışa aktarma

### Veri Yönetimi
- **Dışa Aktar / İçe Aktar**: tüm veriyi JSON dosyası olarak yedekleyip taşıyabilirsiniz
- İçe aktarma **ekler**, üzerine yazmaz (aynı kurum kodu varsa atlanır)

##  Nasıl Çalıştırılır

### Yerelde
```bash
git clone <bu-deponun-linki>
cd <depo-klasörü>
open index.html   # macOS, ya da dosyaya çift tıklayın
```
Sunucu, kurulum veya derleme adımı gerekmez.

### GitHub Pages
`main` dalındaki `index.html` otomatik olarak yayınlanır. Değişiklik yapıp commit ettiğinizde
site 1-2 dakika içinde güncellenir.

##  Veri Nasıl Saklanır

- Tüm veri tarayıcınızın `localStorage`'ında tutulur — aynı tarayıcıda kaldığınız sürece kalıcıdır.
- Farklı bir tarayıcı/cihazdan açan biri **kendi** boş/varsayılan verisini görür, veri paylaşımlı
  değildir.
- Önemli bir değişiklik yapmadan önce **Dışa Aktar** ile yedek almanız önerilir.
- Baştan başlamak için tarayıcı konsolunda `localStorage.clear()` çalıştırıp sayfayı yenileyin.

##  Teknoloji

- Salt HTML + CSS + Vanilla JavaScript (framework, derleme adımı veya bağımlılık yok)
- Veri: `localStorage` (istemci taraflı, sunucusuz)
- PDF dekont üretimi için [jsPDF](https://github.com/parallax/jsPDF) (CDN üzerinden yüklenir)

##  Bilinen Sınırlamalar

- Çok kullanıcılı / eş zamanlı kullanım için tasarlanmamıştır — her tarayıcı kendi verisini tutar.
- Kurum mutabakat süreci tamamen simülasyondur; gerçek kurum entegrasyonu içermez.
- Giriş ekranı gerçek bir kimlik doğrulama sistemi değildir.
- Üretim (production) bankacılık sistemi değildir; iç kullanım / analist prototipi / sunum
  amaçlıdır. Gerçek bir ürüne dönüştürmek için gereken backend, entegrasyon ve güvenlik
  gereksinimleri ayrı bir dokümanda (`URETIME_GECIS_NOTLARI.md`) özetlenmiştir.

##  Lisans

Bu proje bir iç prototip/demo çalışmasıdır.
