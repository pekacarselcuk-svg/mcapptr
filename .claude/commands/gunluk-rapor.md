---
description: Günlük toptancı taraması. Listeyi günceller ve yeni eklenenleri ayrıca belirten bir rapor yazar.
---

Bugünün toptancı raporunu hazırla. Bugünün tarihini `date +%F` ile al (Europe/Istanbul saatiyle: `TZ=Europe/Istanbul date +%F`).

## 1. Günün taraması
Bölge ve segmenti haftanın gününe göre seç (`TZ=Europe/Istanbul date +%u`):

| Gün | Tarama |
|---|---|
| 1 Pazartesi | Merter, kadın |
| 2 Salı | Merter, çocuk |
| 3 Çarşamba | Güngören, kadın + çocuk |
| 4 Perşembe | Laleli, kadın |
| 5 Cuma | Laleli, çocuk |
| 6 Cumartesi | Zeytinburnu, kadın + çocuk |
| 7 Pazar | Tüm bölgeler: erkek + imitasyon, ayrıca fuar katılımcı listeleri |

`toptanci-arastirmaci` ajanını bu bölge ve segmentle çalıştır. Hedef en az 15 yeni toptancı.

## 2. Eski kayıtları tazele
`takipci_tarihi` alanı 30 günden eski olan en fazla 10 kaydın takipçi sayısını yeniden kontrol et. Aynı kayıtların reklam ve B2C durumunu da güncelle. `durum` alanı `*_dogrulanacak` olan kayıtları da yeniden dene.

## 3. OnePageCRM ile eşleştir (yalnızca okuma)
OnePageCRM bağlantısı (`mcp__OnePageCRM__*`) varsa kullan. Önce `context()` çağır.

**a) Yeni bulunan her firmayı CRM'de ara.** Kişilerde `company_name` alanını `like "%<ad>%"` ile ara, ardından `urls.url` alanında Instagram hesabını ya da alan adını, `phones.number` alanında telefonu dene. Firma adlarının yazımı değişebilir (Ltd, Tekstil, Moda gibi ekleri at, Türkçe karakterleri iki biçimde de dene). Sonucu CSV'ye yaz:
- `crm_durumu`:
  - `yok`: CRM'de hiç yok. Gerçek yeni aday.
  - `aday_aksiyonsuz`: "Müşteri Adayı" durumunda ama açık aksiyonu yok. Kontrol: `actions` sorgusu, `contact_id` + `completed: false`.
  - `aday_aktif`: açık aksiyonu olan aday.
  - `musteri`: "Güncel Müşteri".
  - `eski_musteri`
  - `kapali`
- `crm_sahibi`: kaydın sahibi olan kullanıcının adı (`owner_id` değerini `context().users` ile eşleştir).
- `crm_link`: kaydın `web_url` alanı.

Puanı ayarla: `musteri` ve `kapali` kayıtların puanı 0 olsun (raporda ayrıca listelenmez). `aday_aksiyonsuz` +10 alsın (sıcak ama unutulmuş). `eski_musteri` kayıtlarını raporda "geri kazanım" olarak işaretle.

**b) CRM'de bekleyen adaylar.** Her gün CRM'den en fazla 15 aday seç. Ölçütler: "Müşteri Adayı" durumunda, açık aksiyonu yok, `last_activity_date` 90 günden eski, öncelikli bölge (`custom_fields.İlçe`: Merter, Güngören, Laleli, Zeytinburnu) ve tercihen kadın, çocuk, abiye ya da tesettür ürün grubunda (firma kaydındaki `custom_fields.Ürün Grubu`). Bu firmaları web'de hızlıca kontrol et: hâlâ faal mi, Instagram takipçisi, B2B site, reklam, B2C. Raporda listele. Aynı firmayı 30 gün içinde iki kez önerme. Bunun için daha önceki `raporlar/` dosyalarına bak.

**CRM'e yazma.** Kişi, aksiyon ya da not oluşturma veya güncelleme. Yalnızca oku. CRM'e ekleme kararı ekibindir.

## 4. Rapor
`raporlar/YYYY-AA-GG.md` dosyasını oluştur. Aynısını `raporlar/son-rapor.md` olarak da kopyala. Rapor şu bölümlerden oluşsun:

```
# Toptancı Raporu — GG.AA.YYYY

## 🆕 Bugün yeni eklenenler (N firma)
| Puan | Firma | Instagram | Takipçi | Segment | Bölge | B2B site | Fuar | Reklam | B2C | CRM |
(Puana göre azalan sırada. Instagram ve web sitesi bağlantılı olsun. CRM sütununda "🆕 CRM'de yok" ya da "⏸ aksiyonsuz aday (sahibi: X)" yazsın.)

## ⏸ CRM'de açılmış ama aksiyon alınmamış adaylar (N firma)
| Firma | Sahibi | Son aktivite | Bölge | Ürün grubu | Güncel durum (web) | Önerilen aksiyon | CRM |
Önerilen aksiyon örnekleri: "B2C'ye başlamış, MCWarehouse ile ara", "Trendyol'da mağaza açmış", "Kapanmış görünüyor, durumu Kapalı İşletme yapılabilir".

## ♻️ Geri kazanım fırsatları
Taramada bulunan, CRM'de "Eski Müşteri" görünen firmalar.

## 🔄 Güncellenenler (N firma)
Ne değişti: takipçi 3.200 → 4.100, reklam başladı, Trendyol mağazası açıldı vb.

## ⭐ Bugünün öne çıkanları
Puanı 70 ve üzeri olan yeni firmalar. Her biri için neden fırsat olduğunu (hangi MC APP ürünü uygun) 1–2 cümleyle yaz.

## 📊 Genel durum
Toplam kayıt sayısı, segment ve bölge dağılımı, doğrulanmayı bekleyen kayıt sayısı.

## 🔎 Gözlemler
Taramada fark edilen örüntüler.
```

Bugün yeni kayıt bulunamadıysa bunu raporun başında açıkça yaz.

## 5. Kaydet
`leads/toptancilar.csv` ve `raporlar/` klasöründeki değişiklikleri commit'le. Commit mesajı: `Günlük rapor: GG.AA.YYYY (+N yeni)`. Ardından `main` dalına push et.
