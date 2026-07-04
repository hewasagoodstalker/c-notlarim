# C Programlama Dili — Ders Notları

> **Nasıl Kullanılır?** Her bölümde önce özet tablo/açıklama, ardından çalışan kod örneği, en altta ise ⚠️ uyarılar ve 💡 ipuçları yer alır. 🆕 işareti bu güncellemede eklenen bölüm ve alt başlıkları gösterir.

---

## İçindekiler

1. [Temel Veri Tipleri](#1-temel-veri-tipleri)
2. [Sabitler](#2-sabitler)
3. [Operatörler](#3-operatörler) 🆕
4. [Matematik Fonksiyonları](#4-matematik-fonksiyonları)
5. [Girdi Alma](#5-girdi-alma)
6. [Diziler (Arrays)](#6-diziler-arrays)
7. [String'ler ve string.h Fonksiyonları](#7-stringler-ve-stringh-fonksiyonları) 🆕
8. [Fonksiyonlar](#8-fonksiyonlar)
9. [Koşullu İfadeler](#9-koşullu-i̇fadeler)
10. [Yapılar (Struct)](#10-yapılar-struct)
11. [Döngüler (Loops)](#11-döngüler-loops)
12. [İşaretçiler (Pointers)](#12-i̇şaretçiler-pointers)
13. [Dinamik Bellek Yönetimi (malloc / free)](#13-dinamik-bellek-yönetimi-malloc--free)
14. [Dosya İşlemleri](#14-dosya-i̇şlemleri)
15. [Rastgele Sayı Üretimi (rand / srand)](#15-rastgele-sayı-üretimi-rand--srand) 🆕
16. [Temel #include Başlıkları](#16-temel-include-başlıkları)
17. [Hızlı Başvuru — Sık Yapılan Hatalar](#17-hızlı-başvuru--sık-yapılan-hatalar)

---

## 1. Temel Veri Tipleri

| Tip | Açıklama | Format Belirteci | Örnek Tanımlama |
|-----|----------|-----------------|-----------------|
| `int` | Tam sayı | `%d` | `int yas = 35;` |
| `double` | Kesirli sayı | `%f` | `double gpa = 3.2;` |
| `char` | **Tek** karakter | `%c` | `char harf = 'A';` |
| `char[]` | Karakter dizisi (string) | `%s` | `char isim[] = "John";` |

```c
#include <stdio.h>

int main() {
    int    yas            = 35;
    double gpa            = 3.2;
    char   harf           = 'A';         // TEK karakter → tek tırnak
    char   isim[]         = "John";      // String → çift tırnak
    char   sabitBoyut[20];               // 20 karakterlik boş alan (max 19 + '\0')

    printf("Yas: %d\n", yas);
    printf("GPA: %f\n", gpa);
    printf("Harf: %c\n", harf);
    printf("Isim: %s\n", isim);
    return 0;
}
```

> ⚠️ **Kritik Düzeltme:** `char harf = "A";` → **HATALIDIR!**  
> `char` tipine string (`"..."`) atanamaz, yalnızca tek karakter (`'...'`) atanabilir.  
> Doğrusu: `char harf = 'A';`

> 💡 **İpucu:** `char isim[20]` tanımlarken 20 karakterden biri string'i sonlandıran `'\0'` (null) karakterine ayrılır; yani aslında 19 karakter yazabilirsin.

### Ek Sayısal Tipler 🆕

| Tip | Açıklama | Format Belirteci | Örnek Tanımlama |
|-----|----------|-----------------|-----------------|
| `float` | Kesirli sayı (tek hassasiyet, ~6-7 basamak) | `%f` | `float boy = 1.75f;` |
| `long` | Büyük tam sayı | `%ld` | `long nufus = 84000000L;` |
| `long long` | Çok büyük tam sayı | `%lld` | `long long buyuk = 9000000000LL;` |
| `short` | Küçük tam sayı (bellek tasarrufu) | `%hd` | `short kucuk = 12;` |
| `unsigned int` | Yalnızca pozitif tam sayı | `%u` | `unsigned int adet = 42;` |

> 💡 **float mı double mı?** `double` çift hassasiyettir (~15 basamak) ve günlük kullanımda varsayılan tercihtir; `float` yazacaksan sabitin sonuna `f` eki koy (`1.75f`).  
> ⚠️ **scanf farkı:** `printf`'te `double` için `%f` yeterlidir ama `scanf`'te `double` okurken **`%lf` zorunludur**: `scanf("%lf", &gpa);` — `%f` yazarsan değer yanlış okunur!  
> 💡 C99'dan itibaren `#include <stdbool.h>` ile `bool tamam = true;` kullanılabilir. Zaten C'de kural şudur: `0` = yanlış, `0` dışı her değer = doğru.

### printf Formatları ve Kaçış Karakterleri 🆕

| Yazım | Anlamı | Örnek | Çıktı |
|-------|--------|-------|-------|
| `%.2f` | Virgülden sonra 2 basamak | `printf("%.2f", 3.14159);` | `3.14` |
| `%5d` | En az 5 karakter genişlik (sağa yaslı) | `printf("[%5d]", 42);` | `[   42]` |
| `%-5d` | Sola yaslı genişlik | `printf("[%-5d]", 42);` | `[42   ]` |
| `%%` | Yüzde işaretinin kendisi | `printf("%d%%", 50);` | `50%` |

| Kaçış | Anlamı |
|-------|--------|
| `\n` | Yeni satır |
| `\t` | Sekme (tab) |
| `\\` | Ters bölü (`\`) işaretinin kendisi |
| `\"` | Çift tırnak |
| `\0` | Null karakter — string sonlandırıcı |

```c
#include <stdio.h>

int main() {
    double pi = 3.14159;

    printf("Ham:        %f\n",   pi);    // 3.141590
    printf("2 basamak:  %.2f\n", pi);    // 3.14
    printf("Ad\tSoyad\n");               // Arada tab boşluğu bırakır
    printf("O, \"merhaba\" dedi.\n");    // Çift tırnak yazdırma
    printf("Indirim: %%20\n");           // Çıktı: Indirim: %20
    return 0;
}
```

> 💡 Koddaki `//` tek satırlık, `/* ... */` çok satırlı **yorumdur**; derleyici tarafından tamamen yok sayılır.

---

## 2. Sabitler

```c
const int MAKS_BOYUT = 100;      // int sabiti
const double PI      = 3.14159;  // double sabiti
```

> 💡 **Kural:** Sabit (değişmez) değişken isimleri geleneksel olarak **BÜYÜK HARF** ile yazılır.  
> ⚠️ `const` ile tanımlanan değişkene sonradan değer atamaya çalışırsan **derleme hatası** alırsın.

### #define ile Sabit Tanımlama 🆕

```c
#include <stdio.h>

#define PI 3.14159        // Önişlemci sabiti — tip YOK, noktalı virgül YOK
#define MAKS_BOYUT 100

int main() {
    double alan = PI * 5 * 5;   // Derlemeden ÖNCE "PI" gördüğü her yere 3.14159 yazılır
    printf("Alan: %.2f\n", alan);        // 78.54
    printf("Maks: %d\n", MAKS_BOYUT);    // 100
    return 0;
}
```

> 💡 **`const` vs `#define`:** `#define` derleme öncesi düz **metin değişimidir** (önişlemci); `const` ise tipi olan gerçek bir değişkendir ve tip denetiminden geçer. Modern C'de genellikle `const` tercih edilir.  
> ⚠️ `#define PI 3.14;` şeklinde satır sonuna **`;` koyma** — koyarsan o `;` de her kullanım yerine kopyalanır ve anlaşılması zor hatalara yol açar.

---

## 3. Operatörler

> 🆕 Bu bölüm notlara yeni eklenmiştir. Karşılaştırma (`==`, `>`) ve mantıksal (`&&`, `||`) operatörler Bölüm 9'da koşullarla birlikte zaten kullanılıyordu; burada tüm operatör ailesi toplu hâlde verilmiştir.

### 3.1 Aritmetik Operatörler

| Operatör | Anlam | Örnek | Sonuç |
|----------|-------|-------|-------|
| `+` | Toplama | `7 + 2` | `9` |
| `-` | Çıkarma | `7 - 2` | `5` |
| `*` | Çarpma | `7 * 2` | `14` |
| `/` | Bölme | `7 / 2` | `3` ⚠️ (3.5 değil!) |
| `%` | Mod — bölümden kalan | `7 % 2` | `1` |

```c
#include <stdio.h>

int main() {
    int a = 7, b = 2;

    printf("Toplam: %d\n", a + b);   // 9
    printf("Fark:   %d\n", a - b);   // 5
    printf("Carpim: %d\n", a * b);   // 14
    printf("Bolum:  %d\n", a / b);   // 3  ⚠️ int/int → küsurat ATILIR
    printf("Kalan:  %d\n", a % b);   // 1  (7 = 2*3 + 1)
    return 0;
}
```

> ⚠️ **Kritik:** İki tam sayının bölümü **her zaman tam sayıdır** — `7 / 2 = 3` olur, `3.5` değil! Küsuratlı sonuç için Tip Dönüşümü gerekir (3.4).  
> ⚠️ `%` (mod) yalnızca **tam sayılarla** çalışır; `double` ile kullanırsan derleme hatası alırsın (ondalıklı kalan için `math.h` içinde `fmod` var).  
> 💡 **Klasik kullanımlar:** `if (sayi % 2 == 0)` → sayı çifttir; `sayi % 10` → son basamağı verir.

### 3.2 Artırma / Azaltma ve Atama Kısayolları

| Operatör | Anlam | Uzun Hâli |
|----------|-------|-----------|
| `x++` / `++x` | 1 artır | `x = x + 1;` |
| `x--` / `--x` | 1 azalt | `x = x - 1;` |
| `x += 5` | 5 ekle | `x = x + 5;` |
| `x -= 5` | 5 çıkar | `x = x - 5;` |
| `x *= 2` | 2 ile çarp | `x = x * 2;` |
| `x /= 2` | 2'ye böl | `x = x / 2;` |

```c
#include <stdio.h>

int main() {
    int x = 5;

    int y = x++;   // ÖNCE ata (y = 5), SONRA artır (x = 6)
    int z = ++x;   // ÖNCE artır (x = 7), SONRA ata (z = 7)

    printf("x=%d y=%d z=%d\n", x, y, z);   // x=7 y=5 z=7
    return 0;
}
```

> 💡 Tek başına bir satırda `x++;` ile `++x;` **aynı işi yapar** (Bölüm 11'deki döngü sayaçları gibi). Fark yalnızca aynı ifade içinde **atamayla birleştiğinde** ortaya çıkar — kafa karışıklığını önlemek için artırmayı ayrı satırda yapman yeterli.

### 3.3 Karşılaştırma Operatörleri

| Operatör | Anlam | Örnek |
|----------|-------|-------|
| `==` | Eşit mi? | `x == 5` |
| `!=` | Eşit değil mi? | `x != 5` |
| `>` / `<` | Büyük / küçük | `x > 5` |
| `>=` / `<=` | Büyük eşit / küçük eşit | `x >= 5` |

> ⚠️ **Kritik:** `=` **atama**, `==` **karşılaştırmadır**!  
> `if (x = 5)` yazarsan karşılaştırma değil atama yaparsın: `x`'e 5 atanır, 5 de "0 dışı = doğru" sayıldığı için koşul **her zaman doğru** olur — derleyici çoğu zaman ses çıkarmaz, sinsi bir hatadır.  
> ⚠️ String'ler `==` ile **karşılaştırılamaz** — `strcmp` gerekir (Bölüm 7).  
> 💡 Karşılaştırmanın sonucu C'de bir sayıdır: doğruysa `1`, yanlışsa `0`.

### 3.4 Tip Dönüşümü (Type Casting)

```c
#include <stdio.h>

int main() {
    int a = 7, b = 2;

    printf("%d\n", a / b);             // 3        (int bölme)
    printf("%f\n", (double)a / b);     // 3.500000 ✅ önce a double'a çevrilir, bölme double olur
    printf("%f\n", (double)(a / b));   // 3.000000 ❌ önce int bölme biter, İŞ İŞTEN GEÇMİŞTİR

    double ort = (80 + 85) / 2.0;      // 💡 Sabitlerden birini 2.0 yazmak da dönüşümü tetikler
    printf("Ortalama: %.1f\n", ort);   // 82.5

    printf("%d\n", (int)3.9);          // 3 — double → int'te küsurat YUVARLANMAZ, ATILIR
    return 0;
}
```

> 💡 **Örtük (otomatik) dönüşüm:** `int` bir değer `double` değişkene atanırsa C bunu kendiliğinden çevirir (`double d = 5;` → `5.0` olur).  
> 💡 **Açık dönüşüm (cast):** `(hedefTip)ifade` şeklinde yazılır — Bölüm 4'teki `(int)pow(2,3)` örneği gibi.  
> ⚠️ Cast'in **yerine** dikkat: `(double)a / b` ile `(double)(a / b)` aynı şey **değildir** (yukarıdaki örnek).

---

## 4. Matematik Fonksiyonları

> ⚠️ **Zorunlu:** Bu fonksiyonları kullanmak için dosyanın en üstüne `#include <math.h>` eklemelisin.  
> Derleme komutu: `gcc program.c -o program -lm` (`-lm` bayrağı şart!)

| Fonksiyon | Açıklama | Örnek | Sonuç |
|-----------|----------|-------|-------|
| `pow(taban, us)` | Üs alma | `pow(2, 3)` | `8.0` |
| `sqrt(sayi)` | Karekök | `sqrt(36)` | `6.0` |
| `ceil(sayi)` | Yukarı yuvarlama | `ceil(36.334)` | `37.0` |
| `floor(sayi)` | Aşağı yuvarlama | `floor(36.443)` | `36.0` |

```c
#include <stdio.h>
#include <math.h>

int main() {
    printf("2^3    = %.0f\n", pow(2, 3));      // 8
    printf("√36    = %.0f\n", sqrt(36));        // 6
    printf("ceil   = %.0f\n", ceil(36.334));    // 37
    printf("floor  = %.0f\n", floor(36.443));   // 36
    return 0;
}
```

> 💡 **İpucu:** `pow()` ve `sqrt()` her zaman `double` döndürür. `int` değişkene atarsan ondalık kısım kaybolur; `(int)pow(2,3)` şeklinde cast kullanabilirsin.

---

## 5. Girdi Alma

### 5.1 `scanf` — Tek Kelime veya Sayı Okuma

```c
#include <stdio.h>

int main() {
    int sayi;
    char kelime[50];

    printf("Bir sayi girin: ");
    scanf("%d", &sayi);       // ⚠️ & işareti ZORUNLUDUR!

    printf("Bir kelime girin: ");
    scanf("%s", kelime);      // Dizi için & gerekmez (zaten adres)

    printf("Sayi: %d, Kelime: %s\n", sayi, kelime);
    return 0;
}
```

> ⚠️ **Kritik Düzeltme:** Orijinal notta `scanf("%d", sayi)` yazılmış — bu **yanlış**!  
> Sayı ve karakter değişkenlerinde `scanf` mutlaka `&` ile birlikte kullanılır: `scanf("%d", &sayi);`  
> ⚠️ `scanf("%s", ...)` boşlukta durur; boşluklu girişler için `fgets` kullan.

> 💡 🆕 `double` okurken format `%lf` olmalıdır: `scanf("%lf", &gpa);` — `printf`'teki `%f`'ten farklıdır (bkz. Bölüm 1'deki not).  
> 💡 🆕 `&`'nin neden zorunlu olduğunun asıl açıklaması Bölüm 12'de ("Fonksiyona Pointer Geçirme").

### 5.2 `fgets` — Boşluklu / Uzun Metin Okuma

```c
#include <stdio.h>

int main() {
    char isim[50];

    printf("Tam isminizi girin: ");
    fgets(isim, 50, stdin);    // Boşluk içeren metinleri okuyabilir

    printf("Merhaba, %s", isim);
    return 0;
}
```

> 💡 **Fark:** `fgets`, satır sonundaki `'\n'` karakterini de diziye dahil eder.  
> Bunu kaldırmak için: `isim[strcspn(isim, "\n")] = '\0';` (`#include <string.h>` gerekir — string fonksiyonları için bkz. Bölüm 7)

---

## 6. Diziler (Arrays)

### 6.1 Tek Boyutlu Dizi

```c
#include <stdio.h>

int main() {
    int luckyNumbers[] = {4, 5, 7, 9, 3};  // Boyut otomatik belirlenir (5)
    // veya sabit boyutlu: int luckyNumbers[5] = {4, 5, 7, 9, 3};

    printf("%d\n", luckyNumbers[0]);  // Çıktı: 4  (1. eleman)
    printf("%d\n", luckyNumbers[1]);  // Çıktı: 5  (2. eleman)
    printf("%d\n", luckyNumbers[4]);  // Çıktı: 3  (son eleman)

    // Tüm diziyi döngüyle yazdırma
    int boyut = sizeof(luckyNumbers) / sizeof(luckyNumbers[0]);  // = 5
    for (int i = 0; i < boyut; i++) {
        printf("%d ", luckyNumbers[i]);
    }
    return 0;
}
```

> ⚠️ **Dizi indeksleri 0'dan başlar!** 5 elemanlı dizinin son elemanı `[4]`'tür, `[5]` değil.  
> ⚠️ Sınır dışına çıkmak (`luckyNumbers[10]`) **tanımsız davranış** (undefined behavior) yaratır — program çöküp görünmez hata üretebilir.

### 6.2 Çok Boyutlu Dizi (2D Matris)

```c
#include <stdio.h>

int main() {
    //              Sütun→  [0] [1]
    int nums[3][2] = { {1, 2},   // Satır 0
                       {3, 4},   // Satır 1
                       {5, 6} }; // Satır 2

    printf("%d\n", nums[0][0]);  // 1  (Satır 0, Sütun 0)
    printf("%d\n", nums[1][1]);  // 4  (Satır 1, Sütun 1)
    printf("%d\n", nums[2][0]);  // 5  (Satır 2, Sütun 0)

    // Tüm matrisi döngüyle yazdırma
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 2; j++) {
            printf("%d ", nums[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

> 💡 `int nums[3][2]` → 3 satır, 2 sütun, toplamda 6 eleman.  
> Erişim: `nums[satır_indeksi][sütun_indeksi]` — her ikisi de **0'dan başlar**.

---

## 7. String'ler ve string.h Fonksiyonları

> 🆕 Bu bölüm notlara yeni eklenmiştir. C'de ayrı bir "string" tipi yoktur: string, sonu `'\0'` (null) karakteriyle biten bir **char dizisidir** (Bölüm 1'deki `char isim[]` tanımını ve Bölüm 6'daki dizileri hatırla). `strcpy` (Bölüm 10) ve `strcspn` (Bölüm 5.2) notların başka yerlerinde zaten geçiyordu; burada toplu olarak açıklanmıştır.  
> Gerekli başlık dosyası: `#include <string.h>`

| Fonksiyon | Görevi | Örnek | Sonuç |
|-----------|--------|-------|-------|
| `strlen(s)` | Uzunluk (`'\0'` hariç) | `strlen("Ali")` | `3` |
| `strcpy(hedef, kaynak)` | Kaynağı hedefe kopyalar | `strcpy(ad, "Ali")` | `ad = "Ali"` |
| `strcat(hedef, kaynak)` | Hedefin sonuna ekler | `strcat(ad, " Can")` | `"Ali Can"` |
| `strcmp(s1, s2)` | İçerikleri karşılaştırır | `strcmp("Ali", "Ali")` | `0` (eşit!) |
| `strcspn(s, "\n")` | Verilen karakterlerden ilkinin konumunu bulur | — | `fgets` temizliği (Bölüm 5.2) |

```c
#include <stdio.h>
#include <string.h>

int main() {
    char ad[50]   = "Ali";      // ⚠️ Ekleme yapılacaksa diziyi GENİŞ tanımla!
    char soyad[]  = "Yilmaz";

    printf("Uzunluk: %d\n", (int)strlen(ad));   // 3  ('\0' sayılmaz)

    strcat(ad, " ");
    strcat(ad, soyad);                    // ad artık "Ali Yilmaz"
    printf("Tam ad: %s\n", ad);

    // Karşılaştırma — SONUÇ 0 İSE EŞİTTİR!
    if (strcmp(ad, "Ali Yilmaz") == 0) {
        printf("Isimler ayni!\n");
    }

    char kopya[50];
    strcpy(kopya, ad);                    // = ile atama ÇALIŞMAZ (Bölüm 10'daki uyarı)
    printf("Kopya: %s\n", kopya);
    return 0;
}
```

> ⚠️ **Kritik:** String'ler `==` ile karşılaştırılamaz! `if (s1 == s2)` içerikleri değil, **bellek adreslerini** karşılaştırır. Doğrusu: `if (strcmp(s1, s2) == 0)`.  
> ⚠️ `strcmp`'in dönüşü: `0` → eşit; negatif → `s1` alfabetik olarak önce; pozitif → sonra gelir. `== 0` yazmayı unutma — `if (strcmp(a, b))` tam tersine "eşit **değilse**" doğru olur!  
> ⚠️ **Taşma tehlikesi:** `strcpy`/`strcat` hedef diziye sığıp sığmadığını **kontrol etmez** — hedef diziyi baştan yeterince büyük tanımla (`char ad[50]` gibi).  
> ⚠️ `gets()` fonksiyonunu **asla kullanma** — taşma kontrolü yapmaz ve standarttan tamamen kaldırılmıştır. Yerine `fgets` (Bölüm 5.2).  
> 💡 `strlen` aslında `size_t` tipinde değer döndürür; `%zu` ile yazdırılır ya da yukarıdaki gibi `(int)` cast ile `%d` kullanılır.

### Karakter Fonksiyonları (ctype.h) 🆕

| Fonksiyon | Görevi | Örnek |
|-----------|--------|-------|
| `toupper(c)` | Büyük harfe çevirir | `toupper('a')` → `'A'` |
| `tolower(c)` | Küçük harfe çevirir | `tolower('Z')` → `'z'` |
| `isdigit(c)` | Rakam mı? | `isdigit('7')` → doğru (0 dışı) |
| `isalpha(c)` | Harf mi? | `isalpha('k')` → doğru |

```c
#include <stdio.h>
#include <ctype.h>

int main() {
    char ad[] = "ali can";

    // Bu fonksiyonlar TEK karakterle çalışır; tüm string için döngü gerekir
    for (int i = 0; ad[i] != '\0'; i++) {   // 💡 '\0' görene kadar ilerle
        ad[i] = toupper(ad[i]);
    }
    printf("%s\n", ad);   // ALI CAN
    return 0;
}
```

---

## 8. Fonksiyonlar

### 8.1 `void` Fonksiyon (Değer Döndürmez)

```c
#include <stdio.h>

// ✅ Fonksiyon main'den ÖNCE tanımlanmalı ya da prototip bildirilmeli
void sayHi(char name[]) {
    printf("Hello %s\n", name);
}

int main() {
    sayHi("Mike");   // Fonksiyon çağrısı
    sayHi("Alice");
    return 0;
}
```

> ⚠️ **Kritik:** Orijinal notta fonksiyon `main`'den **sonra** tanımlanmış — bu C'de derleme uyarısı/hatasına yol açabilir.  
> Çözüm 1: Fonksiyonu `main`'den önce tanımla (önerilen yöntem).  
> Çözüm 2: `main`'den önce **prototip** bildir: `void sayHi(char name[]);`

### 8.2 Değer Döndüren Fonksiyon

```c
#include <stdio.h>

int topla(int a, int b) {
    return a + b;
}

double ort(double x, double y) {
    return (x + y) / 2.0;
}

int main() {
    printf("Toplam: %d\n",   topla(3, 5));       // 8
    printf("Ortalama: %.2f\n", ort(4.0, 6.0));   // 5.00
    return 0;
}
```

> 💡 `void` dışındaki fonksiyonlar mutlaka `return` ile değer döndürmelidir; döndürmezse tanımsız davranış oluşur.

---

## 9. Koşullu İfadeler

### 9.1 `if / else if / else`

```c
#include <stdio.h>

int main() {
    int num1 = 5, num2 = 3, num3 = 4;

    if (num1 >= num2 && num1 >= num3) {      // && = İKİSİ DE doğruysa
        printf("num1 en büyük!\n");
    } else if (num2 > num3 || num2 == 5) {   // || = EN AZ BİRİ doğruysa
        printf("İkinci koşul sağlandı\n");
    } else {
        printf("Hiçbiri değil\n");
    }
    return 0;
}
```

| Operatör | Anlam | Örnek |
|----------|-------|-------|
| `&&` | VE — iki koşul da doğruysa | `x>0 && y>0` |
| `\|\|` | VEYA — en az biri doğruysa | `x>0 \|\| y>0` |
| `!` | DEĞİL — koşulun tersini alır | `!tamam` |

### 9.2 `switch / case`

```c
#include <stdio.h>

int main() {
    char grade = 'B';

    switch (grade) {
        case 'A':
            printf("Mükemmel!\n");
            break;   // ⚠️ break ZORUNLU — yoksa sonraki case'e "düşer"!
        case 'B':
            printf("İyi!\n");
            break;
        case 'C':
            printf("Orta\n");
            break;
        case 'F':
            printf("Başarısız!\n");
            break;
        default:                              // Hiçbir case uymuyorsa
            printf("Geçersiz not!\n");
            break;
    }
    return 0;
}
```

> ⚠️ **Kritik Düzeltme:** Orijinal notta `switch` içindeki `case`'lerde `break;` eksikti.  
> `break` olmadan program, eşleşen `case`'den sonraki **tüm `case`'leri de sırayla çalıştırır** (fall-through davranışı).  
> Örnek: `grade = 'A'` iken `break` yoksa `"Mükemmel!"`, `"İyi!"`, `"Orta"` ... hepsi yazdırılır!

### 9.3 Ternary (Üçlü) Operatör `?:` 🆕

```c
#include <stdio.h>

int main() {
    int yas = 20;

    // koşul ? doğruysa_bu : yanlışsa_bu
    printf("%s\n", (yas >= 18) ? "Yetiskin" : "Cocuk");   // Yetiskin

    // if-else'in tek satırlık hâli: büyük olanı seç
    int a = 5, b = 3;
    int buyuk = (a > b) ? a : b;
    printf("Buyuk olan: %d\n", buyuk);   // 5
    return 0;
}
```

> 💡 Ternary, basit if-else'leri tek satıra indirir ve bir **değer üretir** — bu yüzden atamanın sağ tarafında kullanılabilir.  
> ⚠️ İç içe ternary (`a ? b : c ? d : e`) okunması zor koddur — karmaşık mantıkta klasik `if / else if / else` (9.1) tercih et.

---

## 10. Yapılar (Struct)

```c
#include <stdio.h>
#include <string.h>   // strcpy için

// Struct tanımı (genellikle main'in dışında yapılır)
struct Student {
    char   name[50];
    char   major[50];
    int    age;
    double gpa;
};    // ⚠️ Kapanan parantezden sonra noktalı virgül ZORUNLUDUR!

int main() {
    struct Student ogrenci1;

    ogrenci1.age = 20;
    ogrenci1.gpa = 3.7;

    // ⚠️ String atamak için = operatörü ÇALIŞMAZ:
    // ogrenci1.name = "Ali";   // HATALI!
    strcpy(ogrenci1.name,  "Ali");       // ✅ Doğrusu
    strcpy(ogrenci1.major, "Bilgisayar Müh.");

    printf("Ad: %s, Bölüm: %s, Yaş: %d, GPA: %.1f\n",
           ogrenci1.name, ogrenci1.major, ogrenci1.age, ogrenci1.gpa);
    return 0;
}
```

> ⚠️ **Kritik:** `struct` içindeki `char[]` alanlarına atama yapmak için `=` **çalışmaz**, `strcpy()` kullanılmalıdır (ayrıntı: Bölüm 7).  
> `strcpy` için `#include <string.h>` başlık dosyası gereklidir.  
> 💡 Başlatma sırasında `struct Student s1 = {"Ali", "Müh.", 20, 3.7};` şeklinde direkt atama yapılabilir.

### typedef ile Kısa İsim Tanımlama 🆕

```c
#include <stdio.h>

typedef struct {
    char name[50];
    int  age;
} Student;              // İsim EN SONDA yazılır; noktalı virgül yine ZORUNLU!

int main() {
    Student s1;          // Artık başına "struct" yazmak gerekmez
    s1.age = 20;
    printf("Yas: %d\n", s1.age);
    return 0;
}
```

> 💡 `typedef`, var olan bir tipe **takma ad** verir. En sık struct'larla karşına çıkar ama basit tiplerde de kullanılabilir: `typedef unsigned int uint;`  
> 💡 Pointer'la kullanımda hiçbir şey değişmez: `Student *s = malloc(sizeof(Student));` → yine `s->age` ile erişilir (Bölüm 13.7).

---

## 11. Döngüler (Loops)

### 11.1 `while` — Önce Kontrol Et, Sonra Çalıştır

```c
#include <stdio.h>

int main() {
    int i = 1;
    while (i <= 5) {
        printf("i = %d\n", i);
        i++;    // ⚠️ Bu satır unutulursa SONSUZ DÖNGÜ oluşur!
    }
    return 0;
}
```

### 11.2 `do-while` — Önce Çalıştır, Sonra Kontrol Et

```c
#include <stdio.h>

int main() {
    int i = 1;
    do {
        printf("i = %d\n", i);
        i++;
    } while (i <= 5);   // ⚠️ Sonunda noktalı virgül ZORUNLUDUR!
    return 0;
}
```

> 💡 **Temel Fark:** `do-while` koşul yanlış olsa bile döngü gövdesi **en az 1 kez** çalışır.  
> Kullanım senaryosu: Kullanıcıdan girdi alırken "en az bir kez sor" mantığında idealdir.

### 11.3 `for` — Sayaçlı Döngü

```c
#include <stdio.h>

int main() {
    // Klasik yazım
    int i;
    for (i = 1; i <= 5; i++) {
        printf("Döngü: %d\n", i);
    }

    // Modern C99+ yazım (değişken döngü içinde tanımlanır)
    for (int j = 0; j < 5; j++) {
        printf("j = %d\n", j);
    }
    return 0;
}
// for (başlangıç; koşul; adım) şeklinde okunur
```

> 💡 **Hangisini seç?**  
> — Kaç kez döneceğini biliyorsan → `for`  
> — Koşul ne zaman sona erer bilmiyorsan → `while`  
> — En az bir kez çalışması gerekiyorsa → `do-while`

### 11.4 `break` ve `continue` 🆕

| Komut | Etkisi |
|-------|--------|
| `break` | Döngüyü **tamamen** bitirir, döngüden çıkar |
| `continue` | Yalnızca **o anki turu** atlar, sonraki tura geçer |

```c
#include <stdio.h>

int main() {
    // break: 5'i görünce döngü TAMAMEN biter
    for (int i = 1; i <= 10; i++) {
        if (i == 5) {
            break;
        }
        printf("%d ", i);        // Çıktı: 1 2 3 4
    }
    printf("\n");

    // continue: 3'ü ATLAR ama döngü devam eder
    for (int i = 1; i <= 5; i++) {
        if (i == 3) {
            continue;
        }
        printf("%d ", i);        // Çıktı: 1 2 4 5
    }
    printf("\n");
    return 0;
}
```

> 💡 `break`'i `switch`'ten hatırlıyorsun (Bölüm 9.2) — oradaki görevi case'ler arası "düşmeyi" engellemekti; döngüdeki görevi ise döngüyü bitirmektir.  
> ⚠️ İç içe döngülerde `break` yalnızca **en içteki** döngüden çıkar, hepsinden değil.

---

## 12. İşaretçiler (Pointers)

### Temel Kavramlar

| Sembol | Rol | Örnek | Açıklama |
|--------|-----|-------|---------|
| `&` | Adres-of operatörü | `&age` | `age` değişkeninin RAM adresini verir |
| `*` (tanımda) | Pointer değişkeni bildirir | `int *p` | `p` bir pointer'dır |
| `*` (kullanımda) | Dereference — adresi takip et | `*p` | `p`'nin işaret ettiği **değeri** verir |

```c
#include <stdio.h>

int main() {
    int    age   = 30;
    double gpa   = 3.4;
    char   grade = 'A';

    // Pointer tanımlama (& ile adres alınır)
    int    *pAge   = &age;
    double *pGpa   = &gpa;
    char   *pGrade = &grade;

    // Bellek adresini yazdır
    printf("age'in adresi: %p\n",  (void*)&age);   // Örn: 0x7ffee4b2c
    printf("pAge değeri:   %p\n",  (void*)pAge);    // Aynı adres

    // Pointer üzerinden değere erişim (* = dereference)
    printf("age değeri: %d\n",  *pAge);    // 30
    printf("gpa değeri: %.1f\n", *pGpa);   // 3.4
    printf("grade:      %c\n",  *pGrade);  // A

    // Pointer üzerinden değeri değiştirme
    *pAge = 31;
    printf("Yeni age: %d\n", age);    // 31 (orijinal değişti!)

    return 0;
}
```

> 💡 **Altın Kural:** Pointer hangi tipte bir değişkeni gösteriyorsa, kendi tipi de aynı olmalıdır:  
> `int age → int *pAge` ✅ | `int age → double *pAge` ❌  
> ⚠️ Başlatılmamış pointer kullanmak (dangling pointer) program çökmesine yol açar — her zaman bir değişkenin adresiyle ilklendir.

### Fonksiyona Pointer Geçirme — swap Örneği 🆕

C'de fonksiyon parametreleri **değerle** (by value) geçer: fonksiyona değişkenin kendisi değil, **kopyası** gider. Kopya üzerinde yapılan değişiklik orijinali etkilemez. Orijinali değiştirmek için değişkenin **adresini** (pointer) göndermek gerekir:

```c
#include <stdio.h>

// ❌ ÇALIŞMAZ: a ve b, x ile y'nin KOPYALARIDIR — takas kopyalar üzerinde kalır
void swapYanlis(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}

// ✅ DOĞRU: adresler üzerinden orijinal değişkenlere ulaşılır
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 3, y = 7;

    swapYanlis(x, y);
    printf("swapYanlis sonrasi: x=%d y=%d\n", x, y);   // x=3 y=7 (değişmedi!)

    swap(&x, &y);                                       // Adresleri gönder
    printf("swap sonrasi:       x=%d y=%d\n", x, y);   // x=7 y=3 ✅
    return 0;
}
```

> 💡 **Şimdi anlam kazandı:** `scanf`'in `&` istemesinin sebebi tam olarak budur (Bölüm 5.1) — `scanf`, okuduğu değeri senin değişkenine **yazabilmek** için onun adresine ihtiyaç duyar.

### Diziler ve Pointer İlişkisi 🆕

```c
#include <stdio.h>

// Parametredeki "int dizi[]" aslında "int *dizi" ile birebir aynıdır
void ikiyeKatla(int dizi[], int boyut) {
    for (int i = 0; i < boyut; i++) {
        dizi[i] *= 2;               // Kopya değil — ORİJİNAL dizi değişir!
    }
}

int main() {
    int sayilar[] = {1, 2, 3};

    printf("%p\n", (void*)sayilar);       // Dizi adı = İLK elemanın adresi
    printf("%p\n", (void*)&sayilar[0]);   // Aynı adres!

    printf("%d\n", *(sayilar + 1));       // 2 → sayilar[1] ile tamamen aynı şey

    ikiyeKatla(sayilar, 3);
    printf("%d %d %d\n", sayilar[0], sayilar[1], sayilar[2]);   // 2 4 6
    return 0;
}
```

> 💡 **Üç kritik gerçek:** ① Dizi adı, ilk elemanın adresidir (`sayilar == &sayilar[0]`). ② `dizi[i]` ile `*(dizi + i)` aynı ifadedir (pointer aritmetiği). ③ Fonksiyona dizi "geçirmek" aslında pointer geçirmektir — bu yüzden fonksiyon diziyi değiştirebilir ve `&` yazmak gerekmez (Bölüm 5.1'de `scanf("%s", kelime)` için `&` gerekmemesinin sebebi de budur).  
> ⚠️ Fonksiyon içinde `sizeof(dizi)` dizinin değil **pointer'ın** boyutunu verir (64-bit sistemde 8 byte) — bu yüzden boyut daima ayrı parametre olarak geçilir; Bölüm 6.1'deki `sizeof` hilesi yalnızca dizinin tanımlandığı yerde çalışır.

---

## 13. Dinamik Bellek Yönetimi (malloc / free)

> 🆕 Bu bölüm derse bir önceki güncellemede eklenmiştir. İşaretçiler konusunun doğrudan devamıdır — `malloc` bir **pointer döndürür**, bu yüzden Bölüm 12'yi bilmek burayı anlamak için gereklidir.  
> Gerekli başlık dosyası: `#include <stdlib.h>`

### 13.1 Neden Dinamik Bellek?

Normal ("static") diziler derleme zamanında (compile-time) sabit boyutludur ve bellekte **stack** denen alanda tutulur:

```c
int dizi[10];   // Boyut HER ZAMAN 10'dur; programın hiçbir yerinde değiştirilemez
```

Ama bazen dizinin kaç elemanlı olacağı ancak **çalışma zamanında** (runtime) — örneğin kullanıcının girdiği bir sayıyla — belli olur. İşte bu durumda **heap** adı verilen, programcının elle yönettiği bellek alanı kullanılır.

| Fonksiyon | Görevi | Döndürdüğü |
|-----------|--------|-----------|
| `malloc(boyut)` | Belirtilen byte kadar **ilklendirilmemiş** bellek ayırır | Başarılı: adres / Başarısız: `NULL` |
| `calloc(adet, boyut)` | Bellek ayırır VE tüm baytları **0** ile doldurur | Başarılı: adres / Başarısız: `NULL` |
| `realloc(ptr, yeniBoyut)` | Önceden ayrılmış belleği büyütür/küçültür | Başarılı: yeni adres / Başarısız: `NULL` |
| `free(ptr)` | Ayrılan belleği işletim sistemine geri verir | — |

### 13.2 `malloc()` — Bellek Ayırma

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n = 5;

    // malloc, istenen byte sayısı kadar HEAP'te yer ayırır ve başlangıç adresini döner
    int *dizi = malloc(n * sizeof(int));   // 5 * 4 = 20 byte ayrılır (int genelde 4 byte)

    // ✅ ZORUNLU: malloc başarısız olursa NULL döner — kontrol etmeden kullanma!
    if (dizi == NULL) {
        printf("Hata: Bellek ayrilamadi!\n");
        return 1;
    }

    // Pointer, normal dizi gibi [] ile kullanılabilir
    for (int i = 0; i < n; i++) {
        dizi[i] = i * 10;
    }

    for (int i = 0; i < n; i++) {
        printf("dizi[%d] = %d\n", i, dizi[i]);
    }

    free(dizi);     // ⚠️ Kullanım bitince MUTLAKA serbest bırak!
    dizi = NULL;    // 💡 free sonrası NULL atamak "dangling pointer" hatasını önler

    return 0;
}
```

> ⚠️ **Kritik:** `malloc` ile ayrılan bellek **ilklendirilmemiştir** — içinde rastgele ("çöp") değerler bulunur. Kullanmadan önce mutlaka kendin doldurmalısın.  
> 💡 **`sizeof` Kuralı:** `malloc` her zaman **byte** cinsinden boyut ister; bu yüzden `n * sizeof(int)` gibi `sizeof` ile çarpım yapılır — asla `malloc(n)` gibi tek başına eleman sayısı yazılmaz.  
> 💡 C'de `malloc`'un döndürdüğü `void*` pointer'ı başka bir pointer tipine atarken **cast gerekmez** (`int *dizi = malloc(...)` yeterlidir); C++'ta ise cast zorunludur.

### 13.3 `calloc()` — Sıfırlanmış Bellek Ayırma

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n = 5;

    // calloc(eleman_sayisi, eleman_boyutu) — malloc'tan farkı: tüm baytları 0 yapar
    int *dizi = calloc(n, sizeof(int));

    if (dizi == NULL) {
        printf("Hata: Bellek ayrilamadi!\n");
        return 1;
    }

    // Tüm elemanlar otomatik olarak 0'dır (malloc'ta rastgele değer olurdu)
    for (int i = 0; i < n; i++) {
        printf("dizi[%d] = %d\n", i, dizi[i]);  // Hepsi 0 yazdırır
    }

    free(dizi);
    return 0;
}
```

> 💡 **`malloc` vs `calloc` Farkı:** `malloc(n * sizeof(int))` ham/rastgele değerli bellek verir; `calloc(n, sizeof(int))` aynı miktarda belleği **0'larla doldurarak** verir. Parametre sırası da farklıdır: `calloc(adet, tekElemanBoyutu)`.  
> ⚠️ `calloc` iki parametreyi çarpıp taşma (overflow) kontrolü de yapar; bu yüzden çok büyük dizilerde `malloc`'a göre biraz daha güvenlidir.

### 13.4 `realloc()` — Yeniden Boyutlandırma

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *dizi = malloc(3 * sizeof(int));
    if (dizi == NULL) return 1;

    dizi[0] = 1; dizi[1] = 2; dizi[2] = 3;

    // Diziyi 3'ten 6 elemana büyüt — eski veriler korunur
    int *temp = realloc(dizi, 6 * sizeof(int));

    // ⚠️ realloc başarısız olursa NULL döner ve ESKİ pointer hâlâ geçerlidir
    if (temp == NULL) {
        printf("Hata: Yeniden boyutlandirma basarisiz!\n");
        free(dizi);   // Eski belleği kaybetmemek için serbest bırak
        return 1;
    }
    dizi = temp;   // ✅ Doğrusu: geçici pointer'a atayıp sonra ana pointer'a aktar

    dizi[3] = 4; dizi[4] = 5; dizi[5] = 6;

    for (int i = 0; i < 6; i++) {
        printf("dizi[%d] = %d\n", i, dizi[i]);
    }

    free(dizi);
    return 0;
}
```

> ⚠️ **Kritik Hata:** `dizi = realloc(dizi, yeniBoyut);` şeklinde **doğrudan** atama yapma! `realloc` başarısız olup `NULL` dönerse, orijinal bellek adresini kaybedersin (bellek sızıntısı). Önce geçici bir pointer'a ata, `NULL` kontrolü yap, sonra asıl pointer'a aktar — yukarıdaki örnekteki gibi.

### 13.5 `free()` — Belleği Serbest Bırakma ve Sık Yapılan Hatalar

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *p = malloc(sizeof(int));
    if (p == NULL) return 1;

    *p = 42;
    printf("Deger: %d\n", *p);

    free(p);     // Belleği işletim sistemine geri ver
    p = NULL;    // ⚠️ free sonrası pointer'ı yanlışlıkla kullanmamak için NULL'a eşitle

    // free(p);  // ❌ HATALI: "double free" — aynı belleği iki kez serbest bırakmak çökmeye yol açar
    // *p = 5;   // ❌ HATALI: "use after free" — serbest bırakılan belleğe erişmek tanımsız davranıştır

    return 0;
}
```

> ⚠️ **Bellek Sızıntısı (Memory Leak):** `malloc`/`calloc`/`realloc` ile ayrılan **her bellek** için mutlaka bir `free()` çağrısı olmalıdır. Unutulursa program çalıştıkça bellek tüketimi artar ve sistem yavaşlayabilir.  
> ⚠️ **Double Free:** Aynı pointer'ı iki kez `free` etmek programın çökmesine (crash) sebep olabilir.  
> ⚠️ **Use After Free:** `free` edilen belleğe `*p` ile erişmeye çalışmak tanımsız davranıştır (undefined behavior). `free` sonrası pointer'ı `NULL` yapmak, yanlışlıkla tekrar kullanılırsa hatayı (çökme şeklinde) hemen ortaya çıkarır ve bu yüzden iyi bir alışkanlıktır.

### 13.6 Stack vs Heap Karşılaştırması

| Özellik | Stack (Static dizi) | Heap (Dinamik — `malloc`) |
|---|---|---|
| Ayırma | `int dizi[10];` | `malloc(10 * sizeof(int));` |
| Boyut | Derleme zamanında sabit | Çalışma zamanında belirlenebilir |
| Temizlik | Fonksiyon bitince **otomatik** silinir | `free()` ile **elle** silinmelidir |
| Hız | Çok hızlı | Stack'e göre daha yavaş |
| Risk | Sabit/sınırlı boyut | Bellek sızıntısı / dangling pointer riski |

> 💡 **Ne zaman dinamik bellek kullanmalı?** Dizinin boyutu çalışma zamanında değişebiliyorsa (kullanıcı girdisine bağlıysa) veya çok büyük veri tutuluyorsa `malloc`/`calloc` tercih edilir. Boyut baştan biliniyor ve küçükse normal (static) dizi yeterlidir.

### 13.7 Dinamik Bellek + Struct Örneği

Bölüm 10'daki `Student` struct'ı dinamik olarak da ayrılabilir. Dinamik ayrılan bir struct'ın alanlarına erişmek için `.` yerine `->` (ok) operatörü kullanılır:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    char   name[50];
    int    age;
    double gpa;
};

int main() {
    // Tek bir struct için dinamik bellek ayırma
    struct Student *s = malloc(sizeof(struct Student));
    if (s == NULL) return 1;

    // ⚠️ Dinamik ayrılan struct pointer'ına '->' ile erişilir, '.' ile DEĞİL
    strcpy(s->name, "Ali");
    s->age = 21;
    s->gpa = 3.5;

    printf("Ad: %s, Yas: %d, GPA: %.1f\n", s->name, s->age, s->gpa);

    free(s);
    s = NULL;

    // Birden fazla öğrenci için dinamik struct DİZİSİ
    int n = 3;
    struct Student *sinif = malloc(n * sizeof(struct Student));
    if (sinif == NULL) return 1;

    strcpy(sinif[0].name, "Ayse");  sinif[0].age = 20;
    strcpy(sinif[1].name, "Mehmet"); sinif[1].age = 22;
    strcpy(sinif[2].name, "Zeynep"); sinif[2].age = 19;

    for (int i = 0; i < n; i++) {
        printf("%s - %d\n", sinif[i].name, sinif[i].age);
    }

    free(sinif);
    return 0;
}
```

> 💡 **`.` mi `->` mi?** Pointer **olmayan** bir struct değişkeninde `.` kullanılır (`ogrenci1.age`, Bölüm 10'daki gibi). Bir struct **pointer'ı** varsa (`malloc` ile ayrıldıysa veya `&` ile adres alındıysa) `->` kullanılır (`s->age`). `s->age` aslında `(*s).age` ifadesinin kısayoludur.  
> 💡 Bir struct **dizisi** `malloc` ile ayrıldığında, elemanlarına yine `[]` ile erişilir ve her eleman pointer olmadığı için `.` kullanılır: `sinif[0].age` — `sinif` pointer'dır ama `sinif[0]` bir struct değeridir.

---

## 14. Dosya İşlemleri

### 14.1 Dosyaya Yazma / Ekleme

```c
#include <stdio.h>

int main() {
    // "w" = Yaz (varsa sil, yoksa oluştur)
    // "a" = Ekle (varsa sonuna ekle, yoksa oluştur)
    FILE *fpointer = fopen("employees.txt", "w");

    // ✅ ZORUNLU: NULL kontrolü — dosya açılamazsa program kilitlenir
    if (fpointer == NULL) {
        printf("Hata: Dosya açılamadı!\n");
        return 1;
    }

    fprintf(fpointer, "Jim, Salesman\n");
    fprintf(fpointer, "Pam, Receptionist\n");
    fprintf(fpointer, "Dwight, Assistant Manager\n");

    fclose(fpointer);   // ⚠️ Her zaman kapat — veri kaybolabilir!
    printf("Dosya başarıyla yazıldı.\n");
    return 0;
}
```

### 14.2 Dosyadan Okuma

```c
#include <stdio.h>

int main() {
    char  line[255];
    FILE *fpointer = fopen("employees.txt", "r");

    if (fpointer == NULL) {
        printf("Hata: Dosya bulunamadı!\n");
        return 1;
    }

    // fgets her seferinde bir satır okur; NULL → dosya bitti
    while (fgets(line, 255, fpointer) != NULL) {
        printf("%s", line);
    }

    fclose(fpointer);
    return 0;
}
```

> 💡 **Orijinal notta tek `fgets` çağrısı vardı** — bu yalnızca ilk satırı okur.  
> Tüm dosyayı okumak için `while` döngüsü içine alınmalıdır (yukarıdaki gibi).

### 14.3 Dosya Açma Modları

| Mod | Ad | Dosya varsa | Dosya yoksa |
|-----|----|-------------|-------------|
| `"r"` | Read | İçeriği okur | ❌ Hata verir (NULL döner) |
| `"w"` | Write | **Eski içeriği siler**, sıfırdan yazar | ✅ Yeni dosya oluşturur |
| `"a"` | Append | Sonuna ekler, eski içerik korunur | ✅ Yeni dosya oluşturur |
| `"r+"` | Read+Write | Hem okur hem yazar | ❌ Hata verir |
| `"w+"` | Write+Read | Siler ve yeniden yazar | ✅ Yeni dosya oluşturur |

---

## 15. Rastgele Sayı Üretimi (rand / srand)

> 🆕 Bu bölüm notlara yeni eklenmiştir. Gerekli başlıklar: `#include <stdlib.h>` (`rand`, `srand`) ve `#include <time.h>` (`time`).

| İfade | Ürettiği Aralık |
|-------|-----------------|
| `rand() % 6` | `0` – `5` |
| `rand() % 6 + 1` | `1` – `6` (zar!) |
| `rand() % 101` | `0` – `100` |
| `alt + rand() % (ust - alt + 1)` | `alt` – `ust` (genel formül) |

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));   // Tohum (seed) — programın başında YALNIZCA 1 KEZ!

    int zar    = rand() % 6 + 1;      // 1–6 arası
    int yuzde  = rand() % 101;        // 0–100 arası
    int aralik = 50 + rand() % 51;    // 50–100 arası (genel formül)

    printf("Zar:    %d\n", zar);
    printf("Yuzde:  %d\n", yuzde);
    printf("Aralik: %d\n", aralik);
    return 0;
}
```

> ⚠️ **Kritik:** `srand(time(NULL))` çağrısını **döngünün içine koyma!** `time(NULL)` saniye cinsinden çalıştığı için aynı saniyedeki tüm çağrılar aynı tohumu üretir → hep "aynı rastgele" sayıyı alırsın.  
> 💡 `srand` hiç çağrılmazsa `rand()` her program çalıştırışında **aynı sayı dizisini** üretir (varsayılan tohum 1'dir) — test / hata ayıklama sırasında bu aslında işine bile yarayabilir!

---

## 16. Temel #include Başlıkları

```c
#include <stdio.h>    // printf, scanf, fgets, fopen, fclose, fprintf, FILE
#include <stdlib.h>   // malloc, calloc, realloc, free, exit, atoi, rand, srand
#include <string.h>   // strcpy, strlen, strcmp, strcat, strcspn
#include <math.h>     // pow, sqrt, ceil, floor, fabs  → gcc'de -lm bayrağı lazım!
#include <ctype.h>    // toupper, tolower, isdigit, isalpha
#include <time.h>     // time → srand(time(NULL)) tohumlaması için  🆕
#include <stdbool.h>  // bool, true, false (C99)  🆕
```

---

## 17. Hızlı Başvuru — Sık Yapılan Hatalar

| # | ❌ Hatalı Yazım | ✅ Doğru Yazım | Neden? |
|---|----------------|----------------|--------|
| 1 | `char c = "A";` | `char c = 'A';` | Tek karakter → tek tırnak |
| 2 | `scanf("%d", sayi);` | `scanf("%d", &sayi);` | `&` operatörü zorunlu |
| 3 | `switch` içinde `break;` yok | Her `case`'e `break;` ekle | Fall-through → istenmeyen çalışma |
| 4 | `struct X {}` (`;` yok) | `struct X {};` | Noktalı virgül zorunlu |
| 5 | `ogrenci.name = "Ali";` | `strcpy(ogrenci.name, "Ali");` | Diziye `=` ile string atanamaz |
| 6 | Fonksiyon `main`'den sonra, prototipi yok | `main`'den önce tanımla veya prototip bildir | Derleme uyarısı/hatası |
| 7 | `fopen` sonrası NULL kontrolü yok | Her zaman `if (fp == NULL)` kontrol et | Program çökebilir |
| 8 | `#include <math.h>` yok ama `pow` kullanılmış | Başlık dosyasını ekle | Tanımsız fonksiyon hatası |
| 9 | Tek `fgets` ile tüm dosyayı okuma | `while (fgets(...) != NULL)` döngüsü | Sadece ilk satırı okur |
| 10 | `for` döngüsünde `i++` veya `i--` unutma | Sayacı her iterasyonda güncelle | Sonsuz döngü |
| 11 | `malloc` sonrası `NULL` kontrolü yok | Her zaman `if (p == NULL)` kontrol et | Bellek ayrılamazsa program çökebilir |
| 12 | `malloc` sonrası `free` çağrılmıyor | Kullanım bitince mutlaka `free(p);` | Bellek sızıntısı (memory leak) |
| 13 | `free(p);` sonra tekrar `free(p);` | `free` sonrası `p = NULL;` yap | Double free → program çökmesi |
| 14 | `dizi = realloc(dizi, yeniBoyut);` | `temp = realloc(...); if (temp) dizi = temp;` | `realloc` başarısız olursa orijinal pointer kaybolur |
| 15 | `malloc(n)` (sizeof unutuldu) | `malloc(n * sizeof(int))` | `malloc` byte ister, eleman sayısı değil |
| 16 | `if (x = 5)` | `if (x == 5)` | `=` atama yapar; koşul her zaman doğru olur |
| 17 | `if (s1 == s2)` (string karşılaştırma) | `if (strcmp(s1, s2) == 0)` | `==` adresleri karşılaştırır, içeriği değil |
| 18 | `double o = (a + b) / 2;` (`a`, `b` int) | `(a + b) / 2.0` veya cast kullan | Tam sayı bölmesi küsuratı atar |
| 19 | `scanf("%f", &d);` (`d` double) | `scanf("%lf", &d);` | `scanf`'te double için `%lf` zorunlu |
| 20 | `gets(isim);` | `fgets(isim, boyut, stdin);` | `gets` taşma kontrolü yapmaz (standarttan kaldırıldı) |
| 21 | `srand` döngü içinde | Programın başında 1 kez çağır | Aynı tohum → hep aynı sayı üretir |

> 🆕 16–21 numaralı satırlar bu güncellemede eklenmiştir.

---

*Son güncelleme: C99/C11 standardına göre düzenlenmiştir. Önceki güncellemede Bölüm 13 "Dinamik Bellek Yönetimi (malloc/free)" eklenmişti; bu güncellemede Bölüm 3 "Operatörler", Bölüm 7 "String'ler ve string.h Fonksiyonları" ve Bölüm 15 "Rastgele Sayı Üretimi" ile 🆕 işaretli alt başlıklar (ek tipler, printf formatları, #define, tip dönüşümü, ternary, typedef, break/continue, swap, dizi-pointer ilişkisi) eklenmiş; bölüm numaraları ve içindekiler buna göre güncellenmiştir.*
