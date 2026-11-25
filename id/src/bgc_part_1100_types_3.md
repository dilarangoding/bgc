<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe III: Konversi

[i[Type conversions]<]

Di bab ini kita akan membahas konversi dari satu tipe ke tipe lain. C
punya berbagai cara untuk melakukannya, dan sebagian mungkin sedikit
berbeda dari yang biasa kamu temui di bahasa lain.

Sebelum membahas cara membuat konversi terjadi, mari kita lihat dulu
bagaimana konversi bekerja saat ia _memang_ terjadi.

## Konversi String

[i[Type conversions-->strings]<]

Tidak seperti banyak bahasa, C tidak melakukan konversi string ke angka
(dan sebaliknya) semulus konversi antartipe numerik.

Untuk itu, kita harus memanggil fungsi yang mengerjakan pekerjaan
kotornya.

### Nilai Numerik ke String

Ketika ingin mengonversi angka menjadi string, kita bisa memakai
`sprintf()` (dibaca _SPRINT-f_) atau `snprintf()`
(_s-n-print-f_)^[Keduanya sama, kecuali `snprintf()` memungkinkan kita
menentukan jumlah byte maksimum yang ditulis, sehingga mencegah
kelebihan menulis melewati ujung string. Jadi lebih aman.]

Keduanya pada dasarnya bekerja seperti `printf()`, hanya saja outputnya
ke string, dan string itu bisa dicetak nanti, atau diapakan saja.

Misalnya, mengubah sebagian nilai π menjadi string:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[10];
    float f = 3.14159;

    // Konversi "f" menjadi string, simpan di "s", tulis paling banyak 10
    // karakter termasuk terminator NUL

    snprintf(s, 10, "%f", f);

    printf("String value: %s\n", s);  // String value: 3.141590
}
```

Jadi kamu bisa memakai `%d` atau `%u` seperti biasa untuk integer.

### String ke Nilai Numerik

Ada beberapa keluarga fungsi untuk ini di C. Kita sebut keluarga `atoi`
(dibaca _a-to-i_) dan keluarga `strtol` (_stir-to-long_).

Untuk konversi dasar dari string ke angka, coba fungsi `atoi` dari
`<stdlib.h>`. Fungsi-fungsi ini punya penanganan error yang buruk
(termasuk undefined behavior kalau kamu mengoper string yang salah), jadi
pakailah dengan hati-hati.

|Fungsi|Deskripsi|
|:-|:-|
|`atoi`|String ke `int`|
|`atof`|String ke `float`|
|`atol`|String ke `long int`|
|`atoll`|String ke `long long int`|

Walaupun spesifikasi tidak mengakuinya, huruf `a` di awal nama fungsi
singkatan dari [flw[ASCII|ASCII]], jadi sebenarnya `atoi()` adalah
"ASCII-to-integer", tapi mengatakannya hari ini agak terlalu berpusat
pada ASCII.

Berikut contoh mengonversi string menjadi `float`:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *pi = "3.14159";
    float f;

    f = atof(pi);

    printf("%f\n", f);
}
```

Tapi, seperti kubilang tadi, hal-hal aneh seperti ini
menghasilkan undefined behavior:

``` {.c}
int x = atoi("what");  // "What" bukan angka yang pernah dikenal siapa pun
```

(Waktu kujalankan, hasilnya `0`, tapi jangan sekali-kali
mengandalkannya. Hasilnya bisa sama sekali berbeda.)

Untuk penanganan error yang lebih baik, mari kita lihat fungsi-fungsi
`strtol`, yang juga ada di `<stdlib.h>`. Bukan hanya itu, mereka juga
mengonversi ke lebih banyak tipe dan lebih banyak basis!

|Fungsi|Deskripsi|
|:-|:-|
|`strtol`|String ke `long int`|
|`strtoll`|String ke `long long int`|
|`strtoul`|String ke `unsigned long int`|
|`strtoull`|String ke `unsigned long long int`|
|`strtof`|String ke `float`|
|`strtod`|String ke `double`|
|`strtold`|String ke `long double`|

Fungsi-fungsi ini semuanya punya pola pemakaian yang mirip, dan bagi
banyak orang ini pengalaman pertama dengan pointer ke pointer! Tapi tenang
saja---tidak sesulit kelihatannya.

Mari kita coba contoh ketika string dikonversi menjadi `unsigned long`,
dengan membuang informasi error (yaitu informasi tentang karakter yang
salah di string input):

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "3490";

    // Konversi string s, angka basis 10, menjadi unsigned long int.
    // NULL berarti kita tidak peduli dengan informasi error apa pun.

    unsigned long int x = strtoul(s, NULL, 10);

    printf("%lu\n", x);  // 3490
}
```

Perhatikan beberapa hal di sana. Meskipun kita tidak sudi menangkap
informasi apa pun tentang karakter yang salah di string, `strtoul()` tidak
akan menghasilkan undefined behavior; ia hanya mengembalikan `0`.

Perhatikan juga bahwa kita menyebutkan bahwa ini angka desimal (basis 10).

Apakah ini berarti kita bisa mengonversi angka dengan basis berbeda?
Tentu! Kita coba biner!

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "101010";  // Apa makna angka ini?

    // Konversi string s, angka basis 2, menjadi unsigned long int.

    unsigned long int x = strtoul(s, NULL, 2);

    printf("%lu\n", x);  // 42
}
```

Oke, itu semua seru-seruan, tapi `NULL` di situ untuk apa?

Itu membantu kita mengetahui apakah terjadi error saat memproses string.
Ia adalah pointer ke pointer ke `char`, yang terdengar menakutkan, tapi
tidak lagi begitu setelah kamu paham.

Mari kita coba contoh dengan sengaja memberi angka yang salah, dan kita
lihat bagaimana `strtol()` memberi tahu di mana digit tidak valid yang
pertama.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "34x90";  // "x" bukan digit yang valid di basis 10!
    char *badchar;

    // Konversi string s, angka basis 10, menjadi unsigned long int.

    unsigned long int x = strtoul(s, &badchar, 10);

    // Ia mencoba mengonversi sebanyak mungkin, jadi sampai sini:

    printf("%lu\n", x);  // 34

    // Tapi kita bisa melihat karakter bermasalah itu karena badchar
    // menunjuk ke sana!

    printf("Invalid character: %c\n", *badchar);  // "x"
}
```

Jadi di sana `strtoul()` mengubah apa yang ditunjuk `badchar` untuk
menunjukkan di mana yang salah^[Kita harus mengoper pointer ke `badchar`
ke `strtoul()`, kalau tidak ia tidak akan bisa mengubahnya dengan cara yang
terlihat oleh kita, sama seperti alasan kenapa kamu harus mengoper pointer
ke `int` ke fungsi kalau ingin fungsi itu bisa mengubah nilai `int` itu.].

Tapi bagaimana kalau tidak ada yang salah? Dalam kasus itu, `badchar`
akan menunjuk ke terminator `NUL` di akhir string. Jadi kita bisa mengujinya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "3490";  // "x" bukan digit yang valid di basis 10!
    char *badchar;

    // Konversi string s, angka basis 10, menjadi unsigned long int.

    unsigned long int x = strtoul(s, &badchar, 10);

    // Periksa apakah semuanya berjalan baik

    if (*badchar == '\0') {
        printf("Success! %lu\n", x);
    } else  {
        printf("Partial conversion: %lu\n", x);
        printf("Invalid character: %c\n", *badchar);
    }
}
```

Jadi begitulah. Fungsi gaya `atoi()` cukup baik untuk keadaan terkendali,
tapi fungsi gaya `strtol()` memberimu kendali jauh lebih besar atas
penanganan error dan basis input.

[i[Type conversions-->strings]>]

## Konversi `char`

[i[Type conversions-->`char`]<]

Bagaimana kalau ada satu karakter berisi digit, seperti `'5'`... Apakah
itu sama dengan nilai `5`?

Kita coba dan lihat.

``` {.c}
printf("%d %d\n", 5, '5');
```

Di sistemku yang memakai UTF-8, ini mencetak:

``` {.default}
5 53
```

Jadi... bukan. Dan 53? Apa itu? Itu code point UTF-8 (dan ASCII)
untuk simbol karakter `'5'`^[Setiap karakter punya nilai yang terkait
dengannya pada skema character encoding mana pun.]

Lalu bagaimana cara mengonversi karakter `'5'` (yang ternyata bernilai 53)
menjadi nilai `5`?

Dengan satu trik cerdik, begitulah caranya!

Standar C menjamin bahwa karakter-karakter ini punya code point yang
berurutan dan dalam urutan ini:

``` {.default}
0  1  2  3  4  5  6  7  8  9
```

Renungkan sebentar--bagaimana kita bisa memakainya? Awas, spoiler...

Mari kita lihat karakter-karakter itu beserta code point-nya di UTF-8:

``` {.default}
0  1  2  3  4  5  6  7  8  9
48 49 50 51 52 53 54 55 56 57
```

Kamu bisa lihat di sana bahwa `'5'` adalah `53`, persis seperti yang kita
dapat tadi. Dan `'0'` adalah `48`.

Jadi kita bisa mengurangkan `'0'` dari karakter digit mana pun untuk
mendapatkan nilai numeriknya:

``` {.c}
char c = '6';

int x = c;  // x bernilai 54, code point untuk '6'

int y = c - '0'; // y bernilai 6, persis seperti yang kita mau
```

Dan kita bisa mengonversi ke arah sebaliknya, cukup dengan menambahkan
nilainya.

``` {.c}
int x = 6;

char c = x + '0';  // c bernilai 54

printf("%d\n", c);  // mencetak 54
printf("%c\n", c);  // mencetak 6 dengan %c
```

Mungkin kamu mengira ini cara yang aneh untuk melakukan konversi, dan
menurut standar zaman sekarang, memang begitu. Tapi di zaman dahulu kala
ketika komputer dibuat dari kayu secara harfiah, inilah metode untuk
konversi itu. Dan karena tidak rusak, C tidak pernah memperbaikinya.

[i[Type conversions-->`char`]>]

## Konversi Numerik

[i[Type conversions-->numeric]<]

### Boolean

[i[Type conversions-->Boolean]]

Kalau nol dikonversi ke `bool`, hasilnya `0`. Selain itu hasilnya `1`.

### Konversi Integer ke Integer

[i[Type conversions-->integer]<]

Kalau sebuah tipe integer dikonversi menjadi unsigned dan nilainya tidak
muat, hasil unsigned-nya berputar seperti odometer sampai muat di
unsigned itu^[Dalam praktiknya, implementasimu kemungkinan cuma membuang
bit-bit orde tinggi dari hasil, jadi angka
16-bit `0x1234` yang dikonversi menjadi angka 8-bit berakhir sebagai
`0x0034`, atau cukup `0x34`.].

Kalau sebuah tipe integer dikonversi menjadi angka signed dan tidak muat,
hasilnya ditentukan oleh implementasi! Sesuatu yang terdokumentasi akan
terjadi, tapi kamu harus mencarinya sendiri^[Lagi-lagi, dalam praktiknya,
di sistemmu kemungkinan pola bit angka aslinya
dipotong lalu dipakai begitu saja untuk merepresentasikan angka signed,
two's complement. Misalnya, sistemku mengambil `unsigned char`
`192` dan mengonversinya menjadi `signed char` `-64`. Dalam two's
complement, pola bit kedua angka ini adalah biner `11000000`.]

### Konversi Integer dan Floating Point

[i[Type conversions-->floating point]<]

Kalau tipe floating point dikonversi menjadi tipe integer, bagian
pecahannya dibuang tanpa ampun^[Sebenarnya tidak---cuma dibuang seperti
biasa.].

Tapi---dan ini jebakannya---kalau angkanya terlalu besar untuk muat di
integer, hasilnya undefined behavior. Jadi jangan lakukan itu.

Dari integer atau floating point ke floating point, C berusaha sebaik
mungkin mencari bilangan floating point terdekat dengan integer itu.

Sekali lagi, kalau nilai aslinya tidak bisa direpresentasikan, hasilnya
undefined behavior.

[i[Type conversions-->floating point]>]
[i[Type conversions-->integer]>]

## Konversi Implisit

[i[Type conversions-->implicit]<]

Konversi ini dilakukan compiler secara otomatis ketika kamu mencampur
berbagai tipe.

### Integer Promotion {#integer-promotions}

Di sejumlah tempat, kalau sebuah `int` bisa dipakai untuk merepresentasikan
nilai dari `char` atau `short` (signed maupun unsigned), nilai itu
_dipromosikan_ naik menjadi `int`. Kalau tidak muat di `int`, ia
dipromosikan menjadi `unsigned int`.

Begitulah cara kita bisa melakukan hal seperti ini:

``` {.c}
char x = 10, y = 20;
int i = x + y;
```

Dalam kasus itu, `x` dan `y` dipromosikan oleh C menjadi `int` sebelum
perhitungan terjadi.

Integer promotion terjadi saat Usual Arithmetic Conversion, pada fungsi
variadic^[Fungsi dengan jumlah argumen yang bervariasi.], operator unary
`+` dan `-`, atau saat mengoper nilai ke fungsi tanpa
prototype^[Ini jarang dilakukan karena compiler akan protes dan punya
prototype adalah _Hal yang Benar_. Kurasa ini masih berfungsi karena
alasan historis, sebelum prototype ada.].

### Usual Arithmetic Conversion {#usual-arithmetic-conversions}

Konversi otomatis ini dikerjakan C pada operasi numerik yang kamu
minta. (Memang itulah namanya, omong-omong, menurut C11 §6.3.1.8.)
Perhatikan bahwa di bagian ini kita hanya membicarakan tipe numerik---string
dibahas nanti.

Konversi ini menjawab pertanyaan tentang apa yang terjadi ketika tipe
dicampur, seperti ini:

``` {.c}
int x = 3 + 1.2;   // Mencampur int dan double
                   // 4.2 dikonversi menjadi int
                   // 4 disimpan di x

float y = 12 * 2;  // Mencampur float dan int
                   // 24 dikonversi menjadi float
                   // 24.0 disimpan di y
```

Apakah mereka menjadi `int`? Atau `float`? Bagaimana cara kerjanya?

Berikut langkah-langkahnya, diparafrasekan agar mudah dicerna.

1. Kalau salah satu bagian ekspresi bertipe floating, konversikan bagian
   lainnya ke tipe floating itu.

2. Kalau tidak, dan kedua tipe adalah tipe integer, lakukan integer
   promotion pada masing-masing, lalu jadikan tipe operand sebesar yang
   dibutuhkan untuk menampung nilai terbesar yang sama. Kadang ini
   melibatkan perubahan signed menjadi unsigned.

Kalau ingin tahu detail lengkapnya, lihat C11 §6.3.1.8. Tapi kemungkinan
besar kamu tidak perlu.

Pokoknya ingat bahwa tipe int menjadi tipe float kalau ada tipe floating
point di mana pun di dalamnya, dan compiler berusaha memastikan tipe
integer campuran tidak overflow.

Terakhir, kalau kamu mengonversi dari satu tipe floating point ke tipe
floating point lain, compiler akan berusaha membuat konversi yang tepat.
Kalau tidak bisa, ia akan membuat perkiraan terbaik yang ia bisa. Kalau
angkanya terlalu besar untuk muat di tipe tujuan, _duar_: undefined
behavior!

### `void*`

Tipe `void*` menarik karena bisa dikonversi dari atau ke tipe pointer
mana pun.

``` {.c}
int x = 10;

void *p = &x;  // &x bertipe int*, tapi kita simpan di void*

int *q = p;    // p adalah void*, tapi kita simpan di int*
```

[i[Type conversions-->implicit]>]

## Konversi Eksplisit

[i[Type conversions-->explicit]<]

Konversi jenis ini harus kamu minta sendiri; compiler tidak akan
melakukannya untukmu.

Kamu bisa mengonversi dari satu tipe ke tipe lain dengan menugaskan satu
tipe ke tipe lain memakai `=`.

Kamu juga bisa mengonversi secara eksplisit dengan _cast_.

[i[Type conversions-->numeric]>]

### Casting

[i[Type conversions-->casting]<]

Kamu bisa mengubah tipe sebuah ekspresi secara eksplisit dengan menaruh
tipe baru dalam tanda kurung di depannya. Sebagian developer C tidak
menyukai praktik ini kecuali benar-benar perlu, tapi kemungkinan besar
kamu akan menemui kode C yang memakainya.

Mari kita coba contoh ketika kita ingin mengonversi `int` menjadi `long`
supaya bisa disimpan di `long`.

Catatan: contoh ini dibuat-buat dan cast di sini sama sekali tidak perlu
karena ekspresi `x + 12` akan otomatis diubah menjadi `long int` agar
sesuai dengan tipe `y` yang lebih lebar.

``` {.c}
int x = 10;
long int y = (long int)x + 12;
```

Pada contoh itu, meskipun `x` tadinya bertipe `int`, ekspresi
`(long int)x` bertipe `long int`. Kita bilang, "Kita meng-cast `x`
menjadi `long int`."

Lebih sering, kamu mungkin melihat cast dipakai untuk mengonversi `void*`
menjadi tipe pointer tertentu agar bisa di-dereference.

Callback dari fungsi bawaan `qsort()` bisa menunjukkan perilaku ini
karena `void*` dioper kepadanya:

``` {.c}
int compar(const void *elem1, const void *elem2)
{
    if (*((const int*)elem2) > *((const int*)elem1)) return 1;
    if (*((const int*)elem2) < *((const int*)elem1)) return -1;
    return 0;
}
```

Tapi kamu juga bisa menulisnya dengan jelas lewat penugasan:

``` {.c}
int compar(const void *elem1, const void *elem2)
{
    const int *e1 = elem1;
    const int *e2 = elem2;

    return *e2 - *e1;
}
```

Salah satu tempat yang lebih sering memakai cast adalah untuk menghindari
peringatan saat mencetak nilai pointer dengan `%p` yang jarang dipakai,
yang rewel terhadap apa pun selain `void*`:

``` {.c}
int x = 3490;
int *p = &x;

printf("%p\n", p);
```

menghasilkan peringatan ini:

``` {.default}
warning: format ‘%p’ expects argument of type ‘void *’, but argument
         2 has type ‘int *’
```

Kamu bisa memperbaikinya dengan cast:

``` {.c}
printf("%p\n", (void *)p);
```

Tempat lainnya adalah pada perubahan pointer eksplisit, kalau kamu tidak
ingin memakai `void*` sebagai perantara, tapi ini juga cukup jarang:

``` {.c}
long x = 3490;
long *p = &x;
unsigned char *c = (unsigned char *)p;
```

Tempat ketiga yang sering mewajibkannya adalah pada fungsi konversi
karakter di
[fl[`<ctype.h>`|https://beej.us/guide/bgclr/html/split/ctype.html]]
yang mengharuskan kamu meng-cast nilai yang tandanya meragukan menjadi
`unsigned char` untuk menghindari undefined behavior.

Sekali lagi, cast jarang sekali _dibutuhkan_ dalam praktik. Kalau kamu
mendapati dirimu sering meng-cast, mungkin ada cara lain untuk melakukan
hal yang sama, atau mungkin kamu meng-cast secara tidak perlu.

Atau mungkin memang perlu. Secara pribadi, aku berusaha
menghindarinya, tapi tidak takut memakainya kalau harus.

[i[Type conversions-->explicit]>]
[i[Type conversions-->casting]>]
[i[Type conversions]>]
