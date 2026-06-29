# C Programlama Dili — Ders Notları

> **Nasıl Kullanılır?** Her bölümde önce özet tablo/açıklama, ardından çalışan kod örneği, en altta ise ⚠️ uyarılar ve 💡 ipuçları yer alır.

---

## İçindekiler

1. [Temel Veri Tipleri](#1-temel-veri-tipleri)
2. [Sabitler](#2-sabitler)
3. [Matematik Fonksiyonları](#3-matematik-fonksiyonları)
4. [Girdi Alma](#4-girdi-alma)
5. [Diziler (Arrays)](#5-diziler-arrays)
6. [Fonksiyonlar](#6-fonksiyonlar)
7. [Koşullu İfadeler](#7-koşullu-i̇fadeler)
8. [Yapılar (Struct)](#8-yapılar-struct)
9. [Döngüler (Loops)](#9-döngüler-loops)
10. [İşaretçiler (Pointers)](#10-i̇şaretçiler-pointers)
11. [Dosya İşlemleri](#11-dosya-i̇şlemleri)
12. [Temel #include Başlıkları](#12-temel-include-başlıkları)
13. [Hızlı Başvuru — Sık Yapılan Hatalar](#13-hızlı-başvuru--sık-yapılan-hatalar)

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

---

## 2. Sabitler

```c
const int MAKS_BOYUT = 100;      // int sabiti
const double PI      = 3.14159;  // double sabiti
```

> 💡 **Kural:** Sabit (değişmez) değişken isimleri geleneksel olarak **BÜYÜK HARF** ile yazılır.  
> ⚠️ `const` ile tanımlanan değişkene sonradan değer atamaya çalışırsan **derleme hatası** alırsın.

---

## 3. Matematik Fonksiyonları

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

## 4. Girdi Alma

### 4.1 `scanf` — Tek Kelime veya Sayı Okuma

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

### 4.2 `fgets` — Boşluklu / Uzun Metin Okuma

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
> Bunu kaldırmak için: `isim[strcspn(isim, "\n")] = '\0';` (`#include <string.h>` gerekir)

---

## 5. Diziler (Arrays)

### 5.1 Tek Boyutlu Dizi

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

### 5.2 Çok Boyutlu Dizi (2D Matris)

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

## 6. Fonksiyonlar

### 6.1 `void` Fonksiyon (Değer Döndürmez)

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

### 6.2 Değer Döndüren Fonksiyon

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

## 7. Koşullu İfadeler

### 7.1 `if / else if / else`

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

### 7.2 `switch / case`

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

---

## 8. Yapılar (Struct)

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

> ⚠️ **Kritik:** `struct` içindeki `char[]` alanlarına atama yapmak için `=` **çalışmaz**, `strcpy()` kullanılmalıdır.  
> `strcpy` için `#include <string.h>` başlık dosyası gereklidir.  
> 💡 Başlatma sırasında `struct Student s1 = {"Ali", "Müh.", 20, 3.7};` şeklinde direkt atama yapılabilir.

---

## 9. Döngüler (Loops)

### 9.1 `while` — Önce Kontrol Et, Sonra Çalıştır

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

### 9.2 `do-while` — Önce Çalıştır, Sonra Kontrol Et

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

### 9.3 `for` — Sayaçlı Döngü

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

---

## 10. İşaretçiler (Pointers)

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

---

## 11. Dosya İşlemleri

### 11.1 Dosyaya Yazma / Ekleme

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

### 11.2 Dosyadan Okuma

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

### 11.3 Dosya Açma Modları

| Mod | Ad | Dosya varsa | Dosya yoksa |
|-----|----|-------------|-------------|
| `"r"` | Read | İçeriği okur | ❌ Hata verir (NULL döner) |
| `"w"` | Write | **Eski içeriği siler**, sıfırdan yazar | ✅ Yeni dosya oluşturur |
| `"a"` | Append | Sonuna ekler, eski içerik korunur | ✅ Yeni dosya oluşturur |
| `"r+"` | Read+Write | Hem okur hem yazar | ❌ Hata verir |
| `"w+"` | Write+Read | Siler ve yeniden yazar | ✅ Yeni dosya oluşturur |

---

## 12. Temel #include Başlıkları

```c
#include <stdio.h>    // printf, scanf, fgets, fopen, fclose, fprintf, FILE
#include <stdlib.h>   // malloc, free, exit, atoi, rand
#include <string.h>   // strcpy, strlen, strcmp, strcat, strcspn
#include <math.h>     // pow, sqrt, ceil, floor, fabs  → gcc'de -lm bayrağı lazım!
#include <ctype.h>    // toupper, tolower, isdigit, isalpha
```

---

## 13. Hızlı Başvuru — Sık Yapılan Hatalar

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

---

*Son güncelleme: C99/C11 standardına göre düzenlenmiştir.*
