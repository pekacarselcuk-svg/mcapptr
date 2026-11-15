---
description: Moda toptancısı taraması başlatır. Örnek: /toptanci-tara Merter kadın 20
argument-hint: [bölge] [segment] [hedef kayıt sayısı]
---

`toptanci-arastirmaci` ajanını şu görevle çalıştır:

- Bölge: $1 (boşsa Merter, Güngören, Laleli, Zeytinburnu)
- Segment: $2 (boşsa önce kadın, sonra çocuk)
- Hedef: en az $3 yeni toptancı (boşsa 20)
- Ölçüt: Instagram takipçisi 1.500'ün üzerinde olan ya da takipçisi doğrulanamayan toptancılar. B2B site durumu her kayıt için belirlenmeli.

Ajan bitince özetini kullanıcıya Türkçe aktar. Ardından `leads/toptancilar.csv` için commit hazırla. Push etmeden önce kullanıcıya sor.
