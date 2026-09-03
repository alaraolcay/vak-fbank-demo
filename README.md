# VakıfBank Ödeme Yönetim Prototipi

Kurumlara ait ödeme/tahsilat ekranlarını **kod yazmadan** tanımlayıp, gerçek bir kullanıcı gibi test edip, kurum mock servisinden dönen mutabakat sonuçlarını takip edebileceğiniz tek sayfalık bir prototip.

## Canlı Demo

 **[alaraolcay.github.io/vak-fbank-demo](https://alaraolcay.github.io/vak-fbank-demo/)**

Kurulum gerektirmez, tarayıcıda doğrudan açılır.

## Proje Hakkında

Bu araç, bir **Tahsilat/Ödeme analistinin** işini tek ekranda toplamak için geliştirildi:

1. **Tanımlar** — Bir kurum ekleyip, o kurumun ödeme akışını (hangi bilgiyi soracak, hangi butonlar olacak) sürükle-bırak ile tasarlarsınız.
2. **Runtime** — Tasarladığınız akışı, gerçek bir müşteri gibi baştan sona deneyip test edersiniz.
3. **Mutabakat** — Yaptığınız test ödemelerinin kurum tarafından onaylanıp onaylanmadığını (mock kurum servisi üzerinden) izlersiniz.

Backend, veritabanı veya kurulum gerektirmez — tüm veri tarayıcınızda (`localStorage`) tutulur.

##  Özellikler

### Tanımlar
- Kurum ekleme/düzenleme/silme/pasife alma (kategori, alt kategori, ödeme adı)
- Panel tasarımı: Borç Sorgulama, Borç Listesi, Ödeme Seçimi, Borç Ödeme, Otomatik Ödeme Talimatı, Abonelik Sorgulama, Bilgi Formu
- Sürükle-bırak Alan Tanım editörü (Designer): metin/sayı kutusu, tarih seçici, hesap/kart seçimi, koşullu görünürlük (örn. "İletişim Tercihi = E-posta" seçilince e-posta kutusu çıkar)
- Kurum bazında **Panel Sağlık Kontrolü**: hangi panelin eksik/taslak/alansız olduğunu gösterir
- İşlem Geçmişi (denetim kaydı): kim, ne zaman, neyi ekledi/değiştirdi/sildi

### Runtime
- Kurum ve akış seçip müşteri gözünden formu doldurma
- Kurumun mock servisinden **rastgele** üretilen borç kayıtları (Alan Tanım'daki sütunlara göre dinamik)
- Ödeme sonucu: Onaylandı / Reddedildi / Hata / Beklemede — gerçekçi kurum yanıt mesajlarıyla (mükerrer ödeme, üst limit aşımı, tutar uyuşmazlığı, sistem bakımda vb.)
- Otomatik Ödeme Talimatı: üst limit ve başlangıç tarihine göre gerçekten borç ödeyen tam işlevli akış
- T.C. Kimlik No sıfırla başlayamama kuralı, telefon numarası otomatik `+90 5XX XXX XX XX` formatı
- Panelleri eksik veya kurum pasifken akış hiç çalıştırılamaz (yanlışlıkla "sahte" mutabakat oluşmaz)

### Mutabakat
- Kurum, durum, tarih aralığı (son 1 saat / 24 saat / 7 gün / özel aralık) filtreleri
- Durum kartlarına tıklayarak hızlı filtreleme (örn. "Hata" kartına tıklayınca sadece hatalı işlemler)
- Sayfalama, tarih bazlı özet (bugün/dün/son 7 gün)
- Beklemede olan işlemleri tek tek veya toplu yeniden sorgulama
- **Transfer Tarihi**: onaylanan ödemenin kuruma T+1 iş günü (hafta sonu hariç) aktarılacağı tarih
- Excel/CSV dışa aktarma, PDF dekont indirme
- Belirli bir kurum için veya rastgele toplu test verisi üretme (demo/sunum için)

### Veri Yönetimi
- **Dışa Aktar / İçe Aktar**: tüm veriyi JSON dosyası olarak yedekleyip başka bir cihaza taşıyabilirsiniz
- İçe aktarma **ekler**, üzerine yazmaz (aynı kurum kodu varsa atlanır, çakışma olmaz)

##  Nasıl Çalıştırılır

### Yerelde
Depoyu indirip `index.html` dosyasını herhangi bir tarayıcıda açmanız yeterlidir. Sunucu, kurulum veya derleme adımı yoktur.

```bash
git clone https://github.com/alaraolcay/vak-fbank-demo.git
cd vak-fbank-demo
open index.html   # macOS
# veya dosyaya çift tıklayın
```

### GitHub Pages
Bu depo GitHub Pages ile yayınlanmaktadır — `main` dalındaki `index.html` otomatik olarak canlıya yansır. Değişiklik yapıp `main`'e commit ettiğinizde site 1-2 dakika içinde güncellenir.

##  Veri Nasıl Saklanır

Tüm kurum/panel/alan/mutabakat verisi tarayıcınızın `localStorage`'ında tutulur:

- Aynı tarayıcıda kaldığınız sürece veriler kalıcıdır (sayfayı kapatıp açmak veri kaybettirmez).
- Farklı bir tarayıcı/cihazdan açan biri **kendi** boş/varsayılan verisini görür — veri paylaşımlı değildir.
- Önemli bir değişiklik yapmadan önce üst menüden **Dışa Aktar** ile yedek almanız önerilir.
- Baştan başlamak isterseniz tarayıcı konsolunda `localStorage.clear()` çalıştırıp sayfayı yenileyin; uygulama 27 örnek kurumla birlikte varsayılan haline döner.

##  Teknoloji

- Salt HTML + CSS + Vanilla JavaScript (framework, derleme adımı veya bağımlılık yok)
- Veri: `localStorage` (istemci taraflı, sunucusuz)
- PDF dekont üretimi için [jsPDF](https://github.com/parallax/jsPDF) (CDN üzerinden yüklenir)

## Bilinen Sınırlamalar

- Çok kullanıcılı / eş zamanlı kullanım için tasarlanmamıştır — her tarayıcı kendi verisini tutar.
- Kurum mock servisi tamamen simülasyondur; gerçek kurum entegrasyonu içermez.
- Üretim (production) bankacılık sistemi değildir; iç kullanım / analist prototipi / sunum amaçlıdır.

##  Lisans

Bu proje bir iç prototip/demo çalışmasıdır.
