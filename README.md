# inciDizLex
Transkripsiyon İşaretli Metinlerden Otomatik Dizin Oluşturma Yazılımı

Osmanlıca ve Türkçe metinler için geliştirilmiş kapsamlı bir dizin oluşturma aracı. Bu araç, Türkoloji çalışmaları, akademik metinler, sözlükler ve tarihî belgeler için dizin oluşturma işlemini otomatikleştirir.

## Özellikler

## Temel İşlevler

- **Dizin Oluşturma**: Metin dosyasındaki kelimeleri temel alarak alfabetik sıralı dizin oluşturur.
- **Kelime Normalizasyonu**: Kelimeleri normalleştirir (küçük harfe çevirme, transkripsiyon işaretlerini dönüştürme).
- **Akıllı Sıralama**: Transkripsiyon işaretlerini dikkate alarak doğru alfabetik sıralama yapar.
- **Hata Kontrolü**: Geçersiz karakterler içeren kelimeleri tespit eder ve ayrıntılı şekilde raporlar.
- **Madde Başı İstatistikleri**: Her kelimenin metinde kaç kez geçtiğini hesaplar ve raporlar.

---

## Dilbilimsel Özellikler

- **Ek ve Birleşik Kelime Analizi**: Kelimelerin kök ve eklerini ayırarak ayrıntılı analiz yapar.
- **Eş Sesli Kelime İşleme**: Aynı yazılışa sahip farklı anlamlı kelimeleri (`kelime+(1)`, `kelime+(2)` formunda) doğru şekilde işleyebilme.
- **Birleşik İfade Tespiti**: Metindeki bağlaçlarla (`ve`, `u`, `-ı` gibi) birleşik ifadeleri tespit eder.
- **Unicode Desteği**: Özel transkripsiyon işaretleri ve çeşitli alfabeler için tam Unicode desteği.

---

## Format Özellikleri

- **Yaprak/Satır Formatı Desteği**: Satır numaralarını standart veya "yaprak/satır" formatında (örn. `1a/02`) işleyebilme.
- **Esnek Girdi Formatı**: Farklı formatlardaki metin dosyalarını işleyebilme (`sayı.`, `sayı:`, yaprak/satır formatları).
- **Sayısal Satır Sıralama**: Satır numaralarını sıralarken sayısal değerleri dikkate alır (`1`, `2`, `10` şeklinde doğru sıralama).

---
## Girdi Dosyası

```bash
001: kitāb ve kalem aldım.
002: kitāb+ı okudum.
003: kitāb+dan bahsediyorum.
004: kitāb+(2) burada başka anlamda.
005: güzel.bir gün.
006: çalış-mak lazım.
007: çalış.tı yoruldu.
008: ḥayvān āḫūr+da duruyor.
009: ḥayvān+dan bahsediyorum.
010: ve kalem ve defter aldım.
```

## Çıktı Dosyaları

- **`...Dizin.txt`**: Ana dizin dosyası. Kelimelerin geçtiği satır numaralarını ve alt maddeleri içerir.
  
```bash
aldım: 1, 10. [=2]
bahsediyorum: 3, 9. [=2]
bir: 5. [=1]
burada: 4. [=1]
çalış: 6, 7. [=2]
    çalış-mak: 6. [=1]
    çalış.tı: 7. [=1]
defter: 10. [=1]
duruyor: 8. [=1]
gün: 5. [=1]
güzel: 5. [=1]
    güzel.bir: 5. [=1]
ḥayvān: 8. [=1]
    ḥayvān+dan: 9. [=1]
kalem: 1, 10. [=2]
kitāb: 1. [=1]
    kitāb+ı: 2. [=1]
    kitāb+dan: 3. [=1]
kitāb+(2): 4. [=1]
lazım: 6. [=1]
okudum: 2. [=1]
ve: 1, 10. [=2]
yoruldu: 7. [=1]
āḫūr+da: 8. [=1]

Toplam kelime: 30
Geçerli kelime: 29
Geçersiz kelime: 1
Alt madde: 7
Madde başı: 16
```

- **`...MBS.txt`**: Her madde başının kaç kez geçtiğini gösteren dosya (Madde Başı Sayısı).
  
```bash
kalem ve defter: 10.
kitāb ve kalem: 1.
```

- **`...Ek.txt`**: Kelime eklerini ve geçtiği satırları gösteren dosya.

```bash
    ḥayvān+dan: 9. [=1]
```

- **`...Sorun.txt`**: Geçersiz kelimeleri ve hataları içeren dosya.

```bash
5: güzel.bir (Geçersiz karakterler: {'.'})
```

- **`...Birlesik.txt`**: Tespit edilen birleşik ifadeleri ve geçtiği satırları listeleyen dosya.

```bash
kalem ve defter: 10.
kitāb ve kalem: 1.
```

---

## Geçerli Karakterler ve Transkripsiyon Desteği
Program şu karakter gruplarını destekler:

* Latin alfabesi ( `a`-`z`, `A`-`Z`)
* Türkçe karakterler ( `ç`, `ğ`, `ı`, `ö`, `ş`, `ü`, `â`)
* Desteklenen transkripsiyon işaretleri ( `ā`, `ḍ`, `é`, `ḥ`, `ī`, `ū`, `ż`, `ġ`, `ḷ`, `ō`, `å`, `ä`, `ï`, `ɵ`, `ə`, `ḳ`, `ṣ`, `ṭ`, `ẓ`, `ḏ`, `s̱`, `ẕ`, `ḫ`, `ñ`, `ŋ`, `n͡g`, `ň`, `ž`, `ý`, `ⱨ`, `ⱪ`, `ţ`, büyük harf varyasyonları olarak `Ā`, `Ē`, `Ī`, `Ō`, `Ū`, `É`, `Í`, `Å`, `Ä`, `Ï`, `Ə`, `Ɵ`, `Ḍ`, `Ḥ`, `Ḳ`, `Ṣ`, `Ṭ`, `Ẓ`, `Ḏ`, `S̱`, `Ẕ`, `Ḫ`, `Ñ`, `Ŋ`, `N͡G`, `Ň`, `Ž`, `Ý`, `Ⱨ`, `Ⱪ`, `Ţ`  vb.)
* Rakamlar ve bazı noktalama işaretleri


## Kurulum ve Kullanım

1.  **Gereksinimler:**
    *   Python 3.x
    *   Gerekli kütüphaneler: `re`, `collections`, `os`, `unicodedata` (genellikle Python ile birlikte gelir).

2.  **Kurulum:**

    *   Bu depoyu klonlayın:
        ```bash
        git clone [repo_url]
        cd [repo_dizini]
        ```

3.  **Kullanım:**

    *   Aracı çalıştırmak için:
        ```bash
        python inciDizLex.py
        ```

    *   Aracı çalıştırdıktan sonra, aşağıdaki sorulara yanıt vermeniz gerekecektir:
        *   Her madde başından sonra kaç defa geçtiği yazılsın mı? (E/H)
        *   Yaprak/satır formatı kullanılsın mı? (E/H)
        *   Birleşik ifadeler dizini oluşturulsun mu? (E/H)

4.  **Dosya Formatı:**

    *   Giriş dosyası (`.txt`) aşağıdaki formatta olmalıdır:
        ```
        SatırNumarası: Metin içeriği
        ```
        veya
         ```
        YaprakNo/SatirNo Metin içeriği
        ```
        Örneğin:
        ```
        1: Bu bir örnektir.
        2: Başka bir örnek.
        1a/01 Bu bir örnektir.
        1a/02 Başka bir örnek.
        01 Bu bir örnektir.
        02 Başka bir örnek.
        ```

## Proje Yapısı

```python
python inciDizLex.py

.
├── inciDizLex.py         # Ana Python betiği
├── test01.txt            # Örnek giriş dosyası
├── test02.txt            # Örnek giriş dosyası
├── test03.txt            # Örnek giriş dosyası
├── README.md             # Bu README dosyası
└── ...
```

## Lisans

Bu proje GNU General Public License v3 altında lisanslanmıştır. Daha fazla bilgi için `LICENSE` dosyasına bakın.

## İletişim

Sorularınız veya önerileriniz için hakanbalas@hotmail.com ile iletişime geçebilirsiniz.
