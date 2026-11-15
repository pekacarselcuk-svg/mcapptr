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

## 3. Rapor
`raporlar/YYYY-AA-GG.md` dosyasını oluştur. Aynısını `raporlar/son-rapor.md` olarak da kopyala. Rapor şu bölümlerden oluşsun:

```
# Toptancı Raporu — GG.AA.YYYY

## 🆕 Bugün yeni eklenenler (N firma)
| Puan | Firma | Instagram | Takipçi | Segment | Bölge | B2B site | Fuar | Reklam | B2C |
(Puana göre azalan sırada. Instagram ve web sitesi bağlantılı olsun.)

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

## 4. Kaydet
`leads/toptancilar.csv` ve `raporlar/` klasöründeki değişiklikleri commit'le. Commit mesajı: `Günlük rapor: GG.AA.YYYY (+N yeni)`. Ardından `main` dalına push et.
