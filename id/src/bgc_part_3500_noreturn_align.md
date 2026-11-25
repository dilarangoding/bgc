<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Function Specifier, Alignment Specifier dan Operator

Menurut pengalamanku, ini tidak terlalu sering dipakai, tapi kita
bahas di sini demi kelengkapan.

## Function Specifier

[i[Function specifiers]<]

Saat mendeklarasikan fungsi, kamu bisa memberi compiler beberapa petunjuk
tentang bagaimana fungsi itu mungkin atau akan dipakai. Ini memungkinkan
atau mendorong compiler melakukan optimasi tertentu.

### `inline` untuk Kecepatan---Mungkin

[i[`inline` function specifier]<]

Kamu bisa mendeklarasikan fungsi sebagai inline seperti ini:

``` {.c}
static inline int add(int x, int y) {
    return x + y;
}
```

Ini dimaksudkan untuk mendorong compiler membuat pemanggilan fungsi ini
secepat mungkin. Dan, secara historis, salah satu caranya adalah
_inlining_, artinya badan fungsi disematkan seluruhnya di tempat
pemanggilan dilakukan. Ini menghindari semua overhead menyiapkan dan
membongkar pemanggilan fungsi, dengan konsekuensi ukuran kode lebih besar
karena fungsi disalin ke mana-mana, bukan dipakai ulang.

Hal-hal cepat dan kasar yang perlu diingat:

1. Kamu kemungkinan tidak perlu memakai `inline` untuk kecepatan. Compiler
   modern tahu mana yang terbaik.

2. Kalau kamu memakainya untuk kecepatan, pakai dengan file scope, yaitu
   `static inline`. Ini menghindari aturan rumit external linkage dan
   fungsi inline.

Berhenti membaca bagian ini sekarang.

Suka menyiksa diri, ya?

Mari kita coba menghilangkan `static`.

``` {.c .numberLines}
#include <stdio.h>

inline int add(int x, int y)
{
    return x + y;
}

int main(void)
{
    printf("%d\n", add(1, 2));
}
```

`gcc` memberi error linker pada `add()`^[Kecuali kamu mengompilasi dengan
optimasi aktif (kemungkinan)! Tapi kurasa, ketika itu terjadi,
perilakunya tidak sesuai spesifikasi.]. Spesifikasi mewajibkan bahwa kalau
kamu punya fungsi inline non-`extern`, kamu juga harus menyediakan versi
dengan external linkage.

Jadi kamu harus punya versi `extern` di tempat lain agar ini berfungsi.
Kalau compiler punya fungsi `inline` di file saat ini dan versi eksternal
dari fungsi yang sama di tempat lain, ia boleh memilih mana yang
dipanggil. Jadi aku sangat menyarankan keduanya sama.

Hal lain yang bisa dilakukan adalah mendeklarasikan fungsi sebagai
`extern inline`. Ini akan mencoba meng-inline di file yang sama (demi
kecepatan), tapi juga membuat versi dengan external linkage.

[i[`inline` function specifier]>]

### `noreturn` dan `_Noreturn` {#noreturn}

[i[`noreturn` function specifier]<]
[i[`_Noreturn` function specifier]<]

Ini menandakan kepada compiler bahwa suatu fungsi tidak akan pernah
kembali ke pemanggilnya, yaitu program akan keluar lewat suatu mekanisme
sebelum fungsi itu kembali.

Ini memungkinkan compiler melakukan beberapa optimasi di sekitar
pemanggilan fungsi tersebut.

Ini juga memungkinkanmu memberi tahu developer lain bahwa sebagian logika
program bergantung pada fungsi yang _tidak_ kembali.

Kemungkinan kamu tidak akan pernah memakainya, tapi kamu akan melihatnya
pada beberapa pemanggilan library seperti
[fl[`exit()`|https://beej.us/guide/bgclr/html/split/stdlib.html#man-exit]]
dan
[fl[`abort()`|https://beej.us/guide/bgclr/html/split/stdlib.html#man-abort]].

Kata kunci bawaannya adalah `_Noreturn`, tapi kalau tidak merusak kode
yang sudah ada, semua orang akan menyarankan meng-include
`<stdnoreturn.h>` dan memakai `noreturn` yang lebih mudah dibaca.

Ini undefined behavior kalau fungsi yang dinyatakan `noreturn` ternyata
kembali. Itu tidak jujur secara komputasi, tahu.

Berikut contoh pemakaian `noreturn` dengan benar:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

noreturn void foo(void) // Fungsi ini tidak boleh pernah kembali!
{
    printf("Happy days\n");

    exit(1);            // Dan ia tidak kembali--ia keluar di sini!
}

int main(void)
{
    foo();
}
```

Kalau compiler mendeteksi bahwa fungsi `noreturn` bisa kembali, ia
mungkin akan memperingatkanmu, dengan baik hati.

Mengganti fungsi `foo()` dengan ini:

``` {.c}
noreturn void foo(void)
{
    printf("Breakin' the law\n");
}
```

menghasilkan peringatan:

``` {.default}
foo.c:7:1: warning: function declared 'noreturn' should not return
```

[i[`noreturn` function specifier]>]
[i[`_Noreturn` function specifier]>]
[i[Function specifiers]>]

## Alignment Specifier dan Operator

[i[Alignment]<]

[flw[_Alignment_|Data_structure_alignment]] sepenuhnya tentang kelipatan
alamat tempat object bisa disimpan. Bisakah ini disimpan di alamat mana
pun? Atau harus alamat awal yang habis dibagi 2? Atau 8? Atau 16?

Kalau kamu membuat sesuatu yang low-level seperti memory allocator yang
berinteraksi dengan OS-mu, kamu mungkin perlu mempertimbangkan ini.
Kebanyakan developer melewati seluruh kariernya tanpa memakai
fungsionalitas ini di C.

### `alignas` dan `_Alignas`

[i[`alignas` alignment specifier]<]
[i[`_Alignas` alignment specifier]<]

Ini bukan fungsi. Melainkan _alignment specifier_ yang bisa dipakai pada
deklarasi variabel.

Specifier bawaannya adalah `_Alignas`, tapi header `<stdalign.h>`
mendefinisikannya sebagai `alignas` agar lebih sedap dipandang.

Kalau kamu butuh `char`-mu ter-align seperti `int`, kamu bisa
memaksanya seperti ini saat mendeklarasikannya:

``` {.c}
char alignas(int) c;
```

Kamu juga bisa mengoper nilai konstan atau ekspresi sebagai alignment. Ini
harus sesuatu yang didukung sistem, tapi spesifikasi tidak sampai
menentukan nilai apa yang boleh dimasukkan. Pangkat 2 kecil (1, 2, 4, 8,
dan 16) umumnya taruhan yang aman.

``` {.c}
char alignas(8) c;   // align pada batas 8 byte
```

Kalau ingin meng-align pada alignment maksimum yang dipakai sistemmu,
include `<stddef.h>` dan pakai tipe `max_align_t`, seperti ini:

``` {.c}
char alignas(max_align_t) c;
```

Kamu berpotensi melakukan _over-align_ dengan menentukan alignment lebih
besar dari `max_align_t`, tapi apakah hal seperti itu diizinkan tergantung
sistem.

[i[`alignas` alignment specifier]>]
[i[`_Alignas` alignment specifier]>]

### `alignof` dan `_Alignof`

[i[`alignof` operator]<]
[i[`_Alignof` operator]<]

Operator ini mengembalikan kelipatan alamat yang dipakai tipe tertentu
untuk alignment di sistem ini. Misalnya, mungkin `char` di-align setiap 1
alamat, dan `int` di-align setiap 4 alamat.

Operator bawaannya adalah `_Alignof`, tapi header `<stdalign.h>`
mendefinisikannya sebagai `alignof` kalau kamu ingin tampak lebih keren.

Berikut program yang mencetak alignment berbagai tipe. Sekali lagi, ini
akan bervariasi dari satu sistem ke sistem lain. Perhatikan bahwa tipe
`max_align_t` akan memberi alignment maksimum yang dipakai sistem.

``` {.c .numberLines}
#include <stdalign.h>
#include <stdio.h>     // untuk printf()
#include <stddef.h>    // untuk max_align_t

struct t {
    int a;
    char b;
    float c;
};

int main(void)
{
    printf("char       : %zu\n", alignof(char));
    printf("short      : %zu\n", alignof(short));
    printf("int        : %zu\n", alignof(int));
    printf("long       : %zu\n", alignof(long));
    printf("long long  : %zu\n", alignof(long long));
    printf("double     : %zu\n", alignof(double));
    printf("long double: %zu\n", alignof(long double));
    printf("struct t   : %zu\n", alignof(struct t));
    printf("max_align_t: %zu\n", alignof(max_align_t));
}
```

Output di sistemku:

``` {.default}
char       : 1
short      : 2
int        : 4
long       : 8
long long  : 8
double     : 8
long double: 16
struct t   : 16
max_align_t: 16
```

[i[`alignof` operator]>]
[i[`_Alignof` operator]>]

## Fungsi `memalignment()`

[i[`memalignment()` function]<]

Baru di C23!

(Peringatan: tidak ada compilerku yang mendukung fungsi ini, jadi
kodenya sebagian besar belum teruji.)

`alignof` bagus kalau kamu tahu tipe datamu. Tapi bagaimana kalau kamu
_sangat tidak tahu_ tipenya, dan hanya punya pointer ke data itu?

Bagaimana itu bisa terjadi?

Tentu saja dengan teman baik kita `void*`. Kita tidak bisa mengopernya ke
`alignof`, tapi bagaimana kalau kita perlu tahu alignment sesuatu yang
ditunjuknya?

Mungkin kita ingin tahu ini kalau akan memakai memori itu untuk sesuatu
yang punya kebutuhan alignment yang signifikan. Misalnya, tipe atomic dan
floating sering berperilaku buruk kalau tidak ter-align.

Jadi dengan fungsi ini kita bisa memeriksa alignment sebuah data selama
kita punya pointer ke data itu, bahkan kalau berupa `void*`.

Mari kita buat tes cepat untuk melihat apakah void pointer ter-align
dengan baik untuk dipakai sebagai tipe atomic, dan, kalau ya, kita ambil
variabel untuk memakainya sebagai tipe itu:

``` {.c}
void foo(void *p)
{
    if (memalignment(p) >= alignof(atomic int)) {
        atomic int *i = p;
        do_things(i);
    } else
        puts("This pointer is no good as an atomic int\n");

...
```

Kurasa kamu akan jarang (sampai kemungkinan tidak pernah)
memerlukan fungsi ini kecuali kamu mengerjakan hal-hal low-level.

[i[`memalignment()` function]>]

Dan begitulah. Alignment!

[i[Alignment]>]
