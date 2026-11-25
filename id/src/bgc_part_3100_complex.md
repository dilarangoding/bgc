<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Bilangan Kompleks

[i[Complex numbers]<]

Primer singkat tentang [flw[bilangan kompleks|Complex_number]] yang dicuri
langsung dari Wikipedia:

> A **complex number** is a number that can be expressed in the form
> $a+bi$, where $a$ and $b$ are real numbers [i.e. floating point types
> in C], and $i$ represents the imaginary unit, satisfying the equation
> $i^2=−1$. Because no real number satisfies this equation, $i$ is
> called an imaginary number. For the complex number $a+bi$, $a$ is
> called the **real part**, and $b$ is called the **imaginary part**.

Tapi hanya sampai di situ yang akan kujelaskan. Kita anggap kalau kamu membaca bab
ini, kamu sudah tahu apa itu bilangan kompleks dan apa yang ingin kamu
lakukan dengannya.

Yang perlu kita bahas hanyalah kemampuan C untuk itu.

Ternyata, dukungan bilangan kompleks di compiler adalah fitur
_opsional_. Tidak semua compiler yang patuh standar bisa melakukannya. Dan
yang bisa pun mungkin mendukungnya dengan tingkat kelengkapan yang
berbeda-beda.

Kamu bisa menguji apakah sistemmu mendukung bilangan kompleks dengan:

[i[`__STDC_NO_COMPLEX__` macro]<]

``` {.c}
#ifdef __STDC_NO_COMPLEX__
#error Complex numbers not supported!
#endif
```

[i[`__STDC_NO_COMPLEX__` macro]>]

[i[`__STDC_IEC_559_COMPLEX__` macro]<]

Selain itu, ada makro yang menandakan kepatuhan pada standar ISO 60559
(IEEE 754) untuk matematika floating point dengan bilangan kompleks, serta
adanya tipe `_Imaginary`.

``` {.c}
#if __STDC_IEC_559_COMPLEX__ != 1
#error Need IEC 60559 complex support!
#endif
```

[i[`__STDC_IEC_559_COMPLEX__` macro]>]

Detail lebih lanjut dijabarkan di Annex G spesifikasi C11.

## Tipe Kompleks

Untuk memakai bilangan kompleks, [i[`complex.h` header file]] `#include
<complex.h>`.

Dengan itu, kamu mendapat setidaknya dua tipe:

[i[`_Complex` type]]
[i[`complex` type]<]

``` {.c}
_Complex
complex
```

Keduanya berarti sama, jadi sebaiknya pakai `complex` yang lebih cantik.

[i[`complex` type]>]

Kamu juga mendapat beberapa tipe untuk bilangan imajiner kalau
implementasimu patuh IEC 60559:

[i[`_Imaginary` type]]
[i[`imaginary` type]<]

``` {.c}
_Imaginary
imaginary
```

Ini juga keduanya berarti sama, jadi sebaiknya pakai `imaginary` yang lebih
cantik.

[i[`imaginary` type]>]

Kamu juga mendapat nilai untuk bilangan imajiner $i$ itu sendiri:

[i[`I` macro]<]
[i[`_Complex_I` macro]<]
[i[`_Imaginary_I` macro]<]

``` {.c}
I
_Complex_I
_Imaginary_I
```

Makro `I` diatur menjadi `_Imaginary_I` (kalau tersedia), atau
`_Complex_I`. Jadi pakai saja `I` untuk bilangan imajiner.

[i[`I` macro]>]
[i[`_Complex_I` macro]>]

[i[`__STDC_IEC_559_COMPLEX__` macro]<]

Satu catatan sampingan: tadi kubilang bahwa kalau compiler punya
`__STDC_IEC_559_COMPLEX__` bernilai `1`, ia harus mendukung tipe
`_Imaginary` agar patuh. Itu bacaanku atas spesifikasi. Namun,
aku tidak tahu satu pun compiler yang benar-benar mendukung
`_Imaginary` meskipun `__STDC_IEC_559_COMPLEX__`-nya diatur. Jadi aku
akan menulis sebagian kode dengan tipe itu di sini tanpa bisa mengujinya.
Maaf!

[i[`__STDC_IEC_559_COMPLEX__` macro]>]
[i[`_Imaginary_I` macro]>]

Oke, sekarang kita tahu ada tipe `complex`, bagaimana cara memakainya?

## Menugaskan Bilangan Kompleks

[i[Complex numbers-->declaring]<]

Karena bilangan kompleks punya bagian real dan imajiner, dan keduanya
bergantung pada bilangan floating point untuk menyimpan nilai, kita juga
perlu memberi tahu C presisi apa yang dipakai untuk bagian-bagian bilangan
kompleks itu.

[i[`complex float` type]<]
[i[`complex double` type]<]
[i[`complex long double` type]<]

Caranya, tinggal menyematkan `float`, `double`, atau `long double` pada
`complex`, baik sebelum maupun sesudahnya.

[i[`complex long double` type]>]

Mari kita definisikan bilangan kompleks yang memakai `float` untuk
komponennya:

``` {.c}
float complex c;   // Spesifikasi lebih suka cara ini
complex float c;   // Sama saja--urutan tidak berpengaruh
```

Itu bagus untuk deklarasi, tapi bagaimana cara menginisialisasi atau
menugaskannya?

Ternyata kita bisa memakai notasi yang cukup alami. Contoh!

[i[`I` macro]<]

``` {.c}
double complex x = 5 + 2*I;
double complex y = 10 + 3*I;
```

[i[`I` macro]>]

Masing-masing untuk $5+2i$ dan $10+3i$.

[i[`complex float` type]>]
[i[`complex double` type]>]
[i[Complex numbers-->declaring]>]

## Membentuk, Membongkar, dan Mencetak

Kita hampir sampai...

Kita sudah melihat satu cara menulis bilangan kompleks:

``` {.c}
double complex x = 5 + 2*I;
```

Tidak masalah juga memakai bilangan floating point lain untuk
membentuknya:

``` {.c}
double a = 5;
double b = 2;
double complex x = a + b*I;
```

[i[`CMPLX()` macro]<]

Ada juga sekumpulan makro untuk membantu membentuknya. Kode di atas bisa
ditulis memakai makro `CMPLX()`, seperti ini:

``` {.c}
double complex x = CMPLX(5, 2);
```

Sejauh yang kutemukan dalam risetku, keduanya _hampir_ setara:

``` {.c}
double complex x = 5 + 2*I;
double complex x = CMPLX(5, 2);
```

Tapi makro `CMPLX()` akan selalu menangani nol negatif pada bagian imajiner
dengan benar, sedangkan cara yang lain mungkin mengonversinya menjadi nol
positif. Aku _kira_^[Yang satu ini lebih sulit diriset, dan aku
menerima informasi tambahan dari siapa pun. `I` bisa didefinisikan sebagai
`_Complex_I` atau `_Imaginary_I`, kalau yang terakhir ada. `_Imaginary_I`
akan menangani nol bertanda, tapi `_Complex_I` _mungkin_ tidak. Ini punya
implikasi pada branch cut dan hal-hal lain yang berbau
matematika-kompleks. Mungkin. Kelihatan kan aku mulai keluar dari zona
nyamanku? Bagaimanapun, makro `CMPLX()` berperilaku seolah-olah `I`
didefinisikan sebagai `_Imaginary_I`, dengan nol bertanda, bahkan kalau
`_Imaginary_I` tidak ada di sistem.] ini menyiratkan bahwa kalau ada
kemungkinan bagian imajinernya nol, sebaiknya pakai makro... tapi
seseorang tolong koreksi kalau aku keliru!

Makro `CMPLX()` bekerja pada tipe `double`. Ada dua makro lain untuk
`float` dan `long double`: [i[`CMPLXF()` macro]] `CMPLXF()` dan
[i[`CMPLXL()` macro]] `CMPLXL()`. (Sufiks "f" dan "l" ini muncul di
hampir semua fungsi terkait bilangan kompleks.)

[i[`CMPLX()` macro]>]

Sekarang kita coba kebalikannya: kalau kita punya bilangan kompleks,
bagaimana memecahnya menjadi bagian real dan imajinernya?

[i[`creal()` function]<]
[i[`cimag()` function]<]

Ada beberapa fungsi yang mengambil bagian real dan imajiner dari angka itu:
`creal()` dan `cimag()`:

``` {.c}
double complex x = 5 + 2*I;
double complex y = 10 + 3*I;

printf("x = %f + %fi\n", creal(x), cimag(x));
printf("y = %f + %fi\n", creal(y), cimag(y));
```

dengan output:

``` {.default}
x = 5.000000 + 2.000000i
y = 10.000000 + 3.000000i
```

Perhatikan bahwa `i` yang kupakai pada format string `printf()` adalah `i` literal yang
dicetak---ia bukan bagian dari format specifier. Kedua nilai kembalian
`creal()` dan `cimag()` bertipe `double`.

Dan seperti biasa, ada varian `float` dan `long double` dari fungsi-fungsi
ini: [i[`crealf()` function]] `crealf()`, [i[`cimagf()` function]]
`cimagf()`, [i[`creall()` function]] `creall()`, dan [i[`cimagl()`
function]] `cimagl()`.

[i[`creal()` function]>]
[i[`cimag()` function]>]

## Aritmetika dan Perbandingan Kompleks

[i[Complex numbers-->arithmetic]<]

Aritmetika bisa dilakukan pada bilangan kompleks, walaupun cara kerjanya
secara matematis di luar cakupan panduan ini.

``` {.c .numberLines}
#include <stdio.h>
#include <complex.h>

int main(void)
{
    double complex x = 1 + 2*I;
    double complex y = 3 + 4*I;
    double complex z;

    z = x + y;
    printf("x + y = %f + %fi\n", creal(z), cimag(z));

    z = x - y;
    printf("x - y = %f + %fi\n", creal(z), cimag(z));

    z = x * y;
    printf("x * y = %f + %fi\n", creal(z), cimag(z));

    z = x / y;
    printf("x / y = %f + %fi\n", creal(z), cimag(z));
}
```

dengan hasil:

``` {.default}
x + y = 4.000000 + 6.000000i
x - y = -2.000000 + -2.000000i
x * y = -5.000000 + 10.000000i
x / y = 0.440000 + 0.080000i
```

Kamu juga bisa membandingkan dua bilangan kompleks untuk kesamaan (atau
ketidaksamaan):

``` {.c .numberLines}
#include <stdio.h>
#include <complex.h>

int main(void)
{
    double complex x = 1 + 2*I;
    double complex y = 3 + 4*I;

    printf("x == y = %d\n", x == y);  // 0
    printf("x != y = %d\n", x != y);  // 1
}
```

dengan output:

``` {.default}
x == y = 0
x != y = 1
```

Keduanya sama kalau kedua komponennya sama. Perhatikan bahwa seperti pada
semua floating point, keduanya bisa dianggap sama kalau cukup dekat karena
galat pembulatan^[Kesederhanaan pernyataan ini tidak mencerminkan
besarnya upaya yang dibutuhkan hanya untuk memahami bagaimana floating
point sebenarnya bekerja.
https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/].

[i[Complex numbers-->arithmetic]>]

## Matematika Kompleks

Tapi tunggu! Ada lebih dari sekadar aritmetika kompleks sederhana!

Berikut tabel ringkasan semua fungsi matematika yang tersedia untuk
bilangan kompleks.

Aku hanya mencantumkan versi `double` dari setiap fungsi, tapi untuk
semuanya ada versi `float` yang didapat dengan menambahkan `f` pada nama
fungsi, dan versi `long double` dengan menambahkan `l`.

Misalnya, fungsi `cabs()` untuk menghitung nilai mutlak bilangan kompleks
juga punya varian `cabsf()` dan `cabsl()`. Aku menghilangkannya demi
keringkasan.

### Fungsi Trigonometri

[i[`ccos()` function]]
[i[`csin()` function]]
[i[`ctan()` function]]
[i[`cacos()` function]]
[i[`casin()` function]]
[i[`catan()` function]]
[i[`ccosh()` function]]
[i[`csinh()` function]]
[i[`ctanh()` function]]
[i[`cacosh()` function]]
[i[`casinh()` function]]
[i[`catanh()` function]]

|Fungsi|Deskripsi|
|-|-|
|`ccos()`|Kosinus|
|`csin()`|Sinus|
|`ctan()`|Tangen|
|`cacos()`|Arc kosinus|
|`casin()`|Arc sinus|
|`catan()`|Bermain _Settlers of Catan_|
|`ccosh()`|Kosinus hiperbolik|
|`csinh()`|Sinus hiperbolik|
|`ctanh()`|Tangen hiperbolik|
|`cacosh()`|Arc kosinus hiperbolik|
|`casinh()`|Arc sinus hiperbolik|
|`catanh()`|Arc tangen hiperbolik|

### Fungsi Eksponensial dan Logaritmik

[i[`cexp()` function]]
[i[`clog()` function]]

|Fungsi|Deskripsi|
|-|-|
|`cexp()`|Eksponensial basis-$e$|
|`clog()`|Logaritma natural (basis-$e$)|

### Fungsi Pangkat dan Nilai Mutlak

[i[`cabs()` function]]
[i[`cpow()` function]]
[i[`csqrt()` function]]

|Fungsi|Deskripsi|
|-|-|
|`cabs()`|Nilai mutlak|
|`cpow()`|Pangkat|
|`csqrt()`|Akar kuadrat|

### Fungsi Manipulasi

[i[`creal()` function]]
[i[`cimag()` function]]
[i[`CMPLX()` macro]]
[i[`carg()` function]]
[i[`conj()` function]]
[i[`cproj()` function]]

|Fungsi|Deskripsi|
|-|-|
|`creal()`|Mengembalikan bagian real|
|`cimag()`|Mengembalikan bagian imajiner|
|`CMPLX()`|Membentuk bilangan kompleks|
|`carg()`|Argumen/sudut fase|
|`conj()`|Konjugat[^4a34]|
|`cproj()`|Proyeksi pada bola Riemann|

[^4a34]: Ini satu-satunya yang tidak diawali `c` tambahan, anehnya.

[i[Complex numbers]>]
