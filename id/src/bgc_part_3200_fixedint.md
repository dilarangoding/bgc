<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe Integer Berlebar Tetap

[i[Fixed width integers]<]

C punya semua tipe integer kecil, lebih besar, dan terbesar seperti `int`
dan `long` dan sebagainya. Dan kamu bisa melihat [bagian tentang
batas](#limits-macros) untuk mengetahui `int` terbesar dengan `INT_MAX`
dan seterusnya.

Seberapa besar tipe-tipe itu? Artinya, berapa byte yang mereka pakai?
Kita bisa memakai `sizeof` untuk mendapat jawabannya.

Tapi bagaimana kalau aku ingin kebalikannya? Bagaimana kalau aku butuh
tipe yang persis 32 bit (4 byte) atau setidaknya 16 bit atau semacamnya?

Bagaimana kita mendeklarasikan tipe dengan ukuran tertentu?

Header [i[`stdint.h` header file]] `<stdint.h>` memberi kita caranya.

## Tipe Berukuran Bit

Untuk integer signed maupun unsigned, kita bisa menentukan tipe yang
berukuran sejumlah bit tertentu, dengan beberapa syarat, tentu saja.

Dan ada tiga kelas utama tipe ini (pada contoh-contoh ini, `N` diganti
dengan jumlah bit tertentu):

* Integer dengan ukuran persis tertentu (`intN_t`)
* Integer dengan ukuran setidaknya tertentu (`int_leastN_t`)
* Integer dengan ukuran setidaknya tertentu dan secepat mungkin
  (`int_fastN_t`)[^4582]

[^4582]: Beberapa arsitektur punya data berukuran berbeda yang bisa
diolah CPU dan RAM dengan laju lebih cepat daripada yang lain. Dalam kasus
itu, kalau kamu butuh angka 8-bit tercepat, mungkin yang diberikan adalah
tipe 16- atau 32-bit karena itu memang lebih cepat. Jadi dengan ini, kamu
tidak akan tahu seberapa besar tipenya, tapi ukurannya setidaknya sebesar
yang kamu sebutkan.

Seberapa lebih cepat `fast` itu? Pasti mungkin agak lebih cepat. Kira-kira.
Spesifikasi tidak menyebut seberapa lebih cepat, hanya bahwa mereka akan
menjadi yang tercepat di arsitektur ini. Kebanyakan C compiler cukup
pandai, jadi kemungkinan ini hanya dipakai di tempat yang butuh jaminan
kecepatan setinggi mungkin (bukan sekadar berharap compiler menghasilkan
kode yang cepat banget, yang memang begitu).

Terakhir, tipe angka unsigned ini punya awalan `u` untuk membedakannya.

Misalnya, tipe-tipe ini punya makna yang tercantum:

[i[`int_leastN_t` types]<]
[i[`uint_leastN_t` types]<]
[i[`int_fastN_t` types]<]
[i[`uint_fastN_t` types]<]
[i[`intN_t` types]<]
[i[`uintN_t` types]<]

``` {.c}
int32_t w;        // w persis 32 bit, signed
uint16_t x;       // x persis 16 bit, unsigned

int_least8_t y;   // y setidaknya 8 bit, signed

// z adalah representasi tercepat dengan setidaknya 64 bit, unsigned:
uint_fast64_t z;
```

Tipe-tipe berikut dijamin terdefinisi:

``` {.c}
int_least8_t      uint_least8_t
int_least16_t     uint_least16_t
int_least32_t     uint_least32_t
int_least64_t     uint_least64_t

int_fast8_t       uint_fast8_t
int_fast16_t      uint_fast16_t
int_fast32_t      uint_fast32_t
int_fast64_t      uint_fast64_t
```

Mungkin ada yang lain dengan lebar berbeda, tapi itu opsional.

Hei! Di mana tipe berlebar tetap seperti `int16_t`? Ternyata itu sepenuhnya
opsional...kecuali kondisi tertentu terpenuhi^[Yaitu, sistem punya integer
8, 16, 32, atau 64 bit tanpa padding yang memakai representasi two's
complement, dan dalam kasus itu varian `intN_t` untuk jumlah bit tersebut
_harus_ didefinisikan.]. Dan kalau kamu memakai sistem komputer modern
biasa, kondisi itu kemungkinan terpenuhi. Dan kalau terpenuhi, kamu akan
punya tipe-tipe ini:

``` {.c}
int8_t      uint8_t
int16_t     uint16_t
int32_t     uint32_t
int64_t     uint64_t
```

Varian lain dengan lebar berbeda mungkin didefinisikan, tapi itu opsional.

[i[`int_leastN_t` types]>]
[i[`uint_leastN_t` types]>]
[i[`int_fastN_t` types]>]
[i[`uint_fastN_t` types]>]
[i[`intN_t` types]>]
[i[`uintN_t` types]>]

## Tipe Ukuran Integer Maksimum

Ada tipe yang bisa dipakai untuk menampung integer terbesar yang bisa
direpresentasikan di sistem, baik signed maupun unsigned:

[i[`intmax_t` type]<]
[i[`uintmax_t` type]<]

``` {.c}
intmax_t
uintmax_t
```

[i[`intmax_t` type]>]
[i[`uintmax_t` type]>]

Pakai tipe ini kalau kamu ingin sebesar mungkin.

Jelas nilai dari tipe integer lain dengan tanda yang sama pasti muat di
tipe ini.

## Memakai Konstanta Berukuran Tetap

Kalau kamu punya konstanta yang ingin dimuat dalam sejumlah bit tertentu,
kamu bisa memakai makro berikut untuk menambahkan sufiks yang tepat secara
otomatis pada angka itu (mis. `22L` atau `3490ULL`).

[i[`INTn_C()` macros]<]
[i[`UINTn_C()` macros]<]
[i[`INTMAX_C()` macro]<]
[i[`UINTMAX_C()` macro]<]

``` {.c}
INT8_C(x)     UINT8_C(x)
INT16_C(x)    UINT16_C(x)
INT32_C(x)    UINT32_C(x)
INT64_C(x)    UINT64_C(x)
INTMAX_C(x)   UINTMAX_C(x)
```

[i[`INTn_C()` macros]>]
[i[`UINTMAX_C()` macro]>]

Sekali lagi, ini hanya bekerja dengan nilai integer konstan.

Misalnya, kita bisa memakai salah satunya untuk menugaskan nilai konstan
seperti ini:

``` {.c}
uint16_t x = UINT16_C(12);
intmax_t y = INTMAX_C(3490);
```

[i[`UINTn_C()` macros]>]
[i[`INTMAX_C()` macro]>]

## Batas Integer Berukuran Tetap

Kita juga punya beberapa batas yang didefinisikan agar kamu bisa
mendapat nilai maksimum dan minimum untuk tipe-tipe ini:

[i[`INTn_MAX` macros]<]
[i[`INTn_MIN` macros]<]
[i[`UINTn_MAX` macros]<]
[i[`INT_LEASTn_MAX` macros]<]
[i[`INT_LEASTn_MIN` macros]<]
[i[`UINT_LEASTn_MAX` macros]<]
[i[`INT_FASTn_MAX` macros]<]
[i[`INT_FASTn_MIN` macros]<]
[i[`UINT_FASTn_MAX` macros]<]
[i[`INTMAX_MAX` macro]<]
[i[`INTMAX_MIN` macro]<]
[i[`UINTMAX_MAX` macro]<]

``` {.c}
INT8_MAX           INT8_MIN           UINT8_MAX
INT16_MAX          INT16_MIN          UINT16_MAX
INT32_MAX          INT32_MIN          UINT32_MAX
INT64_MAX          INT64_MIN          UINT64_MAX

INT_LEAST8_MAX     INT_LEAST8_MIN     UINT_LEAST8_MAX
INT_LEAST16_MAX    INT_LEAST16_MIN    UINT_LEAST16_MAX
INT_LEAST32_MAX    INT_LEAST32_MIN    UINT_LEAST32_MAX
INT_LEAST64_MAX    INT_LEAST64_MIN    UINT_LEAST64_MAX

INT_FAST8_MAX      INT_FAST8_MIN      UINT_FAST8_MAX
INT_FAST16_MAX     INT_FAST16_MIN     UINT_FAST16_MAX
INT_FAST32_MAX     INT_FAST32_MIN     UINT_FAST32_MAX
INT_FAST64_MAX     INT_FAST64_MIN     UINT_FAST64_MAX

INTMAX_MAX         INTMAX_MIN         UINTMAX_MAX
```

[i[`INTn_MAX` macros]>]
[i[`UINTn_MAX` macros]>]
[i[`INT_LEASTn_MAX` macros]>]
[i[`UINT_LEASTn_MAX` macros]>]
[i[`INT_FASTn_MAX` macros]>]
[i[`UINT_FASTn_MAX` macros]>]
[i[`INTMAX_MAX` macro]>]
[i[`UINTMAX_MAX` macro]>]

Perhatikan bahwa `MIN` untuk semua tipe unsigned adalah `0`, sehingga tidak
ada makronya.

[i[`INTn_MIN` macros]>]
[i[`INT_LEASTn_MIN` macros]>]
[i[`INT_FASTn_MIN` macros]>]
[i[`INTMAX_MIN` macro]>]

## Format Specifier

Untuk mencetak tipe-tipe ini, kamu perlu mengirim format specifier yang
tepat ke [i[`printf()` function]] `printf()`. (Dan masalah yang sama untuk
mengambil input dengan [i[`scanf()` function]] `scanf()`.)

Tapi bagaimana kamu akan tahu ukuran tipe di baliknya? Untungnya, sekali
lagi, C menyediakan beberapa makro untuk membantu.

Semuanya ada di `<inttypes.h>`.

Nah, ada banyak sekali makro. Seperti ledakan kompleksitas makro. Jadi
aku akan berhenti mendaftar satu per satu dan cukup menaruh huruf
kecil `n` di tempat kamu seharusnya menaruh `8`, `16`, `32`, atau `64`
sesuai kebutuhan.

Mari kita lihat makro untuk mencetak integer signed:

[i[`PRIdn` macros]<]
[i[`PRIin` macros]<]
[i[`PRIdLEASTn` macros]<]
[i[`PRIiLEASTn` macros]<]
[i[`PRIdFASTn` macros]<]
[i[`PRIiFASTn` macros]<]
[i[`PRIdMAX` macro]<]
[i[`PRIiMAX` macro]<]

``` {.c}
PRIdn    PRIdLEASTn    PRIdFASTn    PRIdMAX
PRIin    PRIiLEASTn    PRIiFASTn    PRIiMAX
```

Cari polanya. Kamu bisa melihat ada varian untuk tipe fixed, least, fast,
dan max.

Dan ada juga `d` huruf kecil dan `i` huruf kecil. Keduanya sesuai dengan
format specifier `printf()` `%d` dan `%i`.

Jadi kalau aku punya sesuatu bertipe:

``` {.c}
int_least16_t x = 3490;
```

Aku bisa mencetaknya dengan padanan format specifier `%d` dengan memakai
`PRIdLEAST16`.

Tapi bagaimana caranya? Bagaimana memakai makro itu?

Pertama, makro itu menentukan string berisi huruf atau huruf-huruf yang
dibutuhkan `printf()` untuk mencetak tipe itu. Misalnya, bisa berupa `"d"`
atau `"ld"`.

Jadi yang perlu kita lakukan hanyalah menyematkannya ke format string pada
pemanggilan `printf()`.

Untuk itu, kita bisa memanfaatkan fakta tentang C yang mungkin sudah kamu
lupakan: string literal yang bersebelahan otomatis digabung menjadi satu
string. Misalnya:

``` {.c}
printf("Hello, " "world!\n");   // Mencetak "Hello, world!"
```

Dan karena makro-makro ini adalah string literal, kita bisa memakainya
seperti ini:

``` {.c .numberLines}
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void)
{
    int_least16_t x = 3490;

    printf("The value is %" PRIdLEAST16 "!\n", x);
}
```

[i[`PRIdn` macros]>]
[i[`PRIin` macros]>]
[i[`PRIdLEASTn` macros]>]
[i[`PRIiLEASTn` macros]>]
[i[`PRIdFASTn` macros]>]
[i[`PRIiFASTn` macros]>]
[i[`PRIdMAX` macro]>]
[i[`PRIiMAX` macro]>]

Kita juga punya setumpuk makro untuk mencetak tipe unsigned:

[i[`PRIon` macros]<]
[i[`PRIun` macros]<]
[i[`PRIxn` macros]<]
[i[`PRIXn` macros]<]
[i[`PRIoLEASTn` macros]<]
[i[`PRIuLEASTn` macros]<]
[i[`PRIxLEASTn` macros]<]
[i[`PRIXLEASTn` macros]<]
[i[`PRIoFASTn` macros]<]
[i[`PRIuFASTn` macros]<]
[i[`PRIxFASTn` macros]<]
[i[`PRIXFASTn` macros]<]
[i[`PRIoMAX` macros]<]
[i[`PRIuMAX` macros]<]
[i[`PRIxMAX` macros]<]
[i[`PRIXMAX` macros]<]

``` {.c}
PRIon    PRIoLEASTn    PRIoFASTn    PRIoMAX
PRIun    PRIuLEASTn    PRIuFASTn    PRIuMAX
PRIxn    PRIxLEASTn    PRIxFASTn    PRIxMAX
PRIXn    PRIXLEASTn    PRIXFASTn    PRIXMAX
```

Dalam kasus ini, `o`, `u`, `x`, dan `X` sesuai dengan format specifier
yang terdokumentasi di `printf()`.

Dan, seperti tadi, huruf kecil `n` harus diganti dengan `8`, `16`, `32`,
atau `64`.

[i[`PRIon` macros]>]
[i[`PRIun` macros]>]
[i[`PRIxn` macros]>]
[i[`PRIXn` macros]>]
[i[`PRIoLEASTn` macros]>]
[i[`PRIuLEASTn` macros]>]
[i[`PRIxLEASTn` macros]>]
[i[`PRIXLEASTn` macros]>]
[i[`PRIoFASTn` macros]>]
[i[`PRIuFASTn` macros]>]
[i[`PRIxFASTn` macros]>]
[i[`PRIXFASTn` macros]>]
[i[`PRIoMAX` macros]>]
[i[`PRIuMAX` macros]>]
[i[`PRIxMAX` macros]>]
[i[`PRIXMAX` macros]>]

Tapi tepat ketika kamu mengira sudah cukup dengan makro, ternyata kita
punya satu set pelengkap lengkap untuk [i[`scanf()` function]] `scanf()`!

[i[`SCNdn` macros]<]
[i[`SCNin` macros]<]
[i[`SCNon` macros]<]
[i[`SCNun` macros]<]
[i[`SCNxn` macros]<]
[i[`SCNdLEASTn` macros]<]
[i[`SCNiLEASTn` macros]<]
[i[`SCNoLEASTn` macros]<]
[i[`SCNuLEASTn` macros]<]
[i[`SCNxLEASTn` macros]<]
[i[`SCNdFASTn` macros]<]
[i[`SCNiFASTn` macros]<]
[i[`SCNoFASTn` macros]<]
[i[`SCNuFASTn` macros]<]
[i[`SCNxFASTn` macros]<]
[i[`SCNdMAX` macros]<]
[i[`SCNiMAX` macros]<]
[i[`SCNoMAX` macros]<]
[i[`SCNuMAX` macros]<]
[i[`SCNxMAX` macros]<]

``` {.c}
SCNdn    SCNdLEASTn    SCNdFASTn    SCNdMAX
SCNin    SCNiLEASTn    SCNiFASTn    SCNiMAX
SCNon    SCNoLEASTn    SCNoFASTn    SCNoMAX
SCNun    SCNuLEASTn    SCNuFASTn    SCNuMAX
SCNxn    SCNxLEASTn    SCNxFASTn    SCNxMAX
```

[i[`SCNdn` macros]>]
[i[`SCNin` macros]>]
[i[`SCNon` macros]>]
[i[`SCNun` macros]>]
[i[`SCNxn` macros]>]
[i[`SCNdLEASTn` macros]>]
[i[`SCNiLEASTn` macros]>]
[i[`SCNoLEASTn` macros]>]
[i[`SCNuLEASTn` macros]>]
[i[`SCNxLEASTn` macros]>]
[i[`SCNdFASTn` macros]>]
[i[`SCNiFASTn` macros]>]
[i[`SCNoFASTn` macros]>]
[i[`SCNuFASTn` macros]>]
[i[`SCNxFASTn` macros]>]
[i[`SCNdMAX` macros]>]
[i[`SCNiMAX` macros]>]
[i[`SCNoMAX` macros]>]
[i[`SCNuMAX` macros]>]
[i[`SCNxMAX` macros]>]

Ingat: saat ingin mencetak tipe integer berukuran tetap dengan `printf()`
atau `scanf()`, ambil format specifier yang sesuai dari `<inttypes.h>`.

[i[Fixed width integers]>]
