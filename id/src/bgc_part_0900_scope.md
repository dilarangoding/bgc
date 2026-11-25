<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Scope {#scope}

[i[Scope]<]
Scope adalah soal variabel apa saja yang terlihat di konteks mana.

## Block Scope

[i[Scope-->block]<]
Hampir semua variabel yang didefinisikan developer memakai scope ini,
termasuk yang di bahasa lain mungkin disebut "function scope", yaitu
variabel yang dideklarasikan di dalam fungsi.

Aturan dasarnya: kalau kamu mendeklarasikan variabel di dalam blok yang
dibatasi kurung kurawal, scope variabel itu adalah blok tersebut.

Kalau ada blok di dalam blok, variabel yang dideklarasikan di blok
_dalam_ bersifat lokal untuk blok itu, dan tidak terlihat di scope luar.

Begitu scope sebuah variabel berakhir, variabel itu tidak bisa lagi
dirujuk, dan nilainya boleh dianggap sudah lenyap ke [flw[bit
bucket|Bit_bucket]] raksasa di langit.

Contoh dengan scope bersarang:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a = 12;         // Lokal untuk blok luar, tapi terlihat di blok dalam

    if  (a == 12) {
        int b = 99;     // Lokal untuk blok dalam, tidak terlihat di blok luar

        printf("%d %d\n", a, b);  // OK: "12 99"
    }

    printf("%d\n", a);  // OK, kita masih di scope a

    printf("%d\n", b);  // ILEGAL, di luar scope b
}
```

### Di Mana Variabel Didefinisikan

Fakta menarik lainnya: variabel bisa didefinisikan di mana saja dalam
blok, dalam batas kewajaran---scope-nya adalah blok itu, tapi tidak bisa
dipakai sebelum didefinisikan.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 0;

    printf("%d\n", i);     // OK: "0"

    //printf("%d\n", j);   // ILEGAL--j tidak bisa dipakai sebelum didefinisikan

    int j = 5;

    printf("%d %d\n", i, j);   // OK: "0 5"
}
```

Secara historis, C mewajibkan semua variabel didefinisikan sebelum kode
apa pun di dalam blok, tapi sejak standar C99 hal itu tidak berlaku lagi.

### Variable Hiding

[i[Variable hiding]<]
Kalau ada variabel bernama sama di scope dalam dan di scope luar, yang di
scope dalam diutamakan selama kode berjalan di scope dalam. Artinya, ia
_menyembunyikan_ variabel di scope luar selama masa hidupnya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 10;

    {
        int i = 20;

        printf("%d\n", i);  // i di scope dalam, 20 (i luar tersembunyi)
    }

    printf("%d\n", i);  // i di scope luar, 10
}
```

Mungkin kamu memperhatikan bahwa pada contoh itu, penulis cuma melempar
sebuah blok begitu saja di baris 7, tanpa `for` atau `if` yang
mengawalinya! Ini sepenuhnya legal. Kadang developer ingin mengelompokkan
beberapa variabel lokal untuk perhitungan singkat dan melakukan ini,
tapi jarang terlihat.
[i[Variable hiding]>]
[i[Scope-->block]>]

## File Scope

[i[Scope-->file]<]
Kalau kamu mendefinisikan variabel di luar blok, variabel itu punya _file
scope_. Ia terlihat di semua fungsi dalam file yang muncul setelahnya, dan
dipakai bersama oleh fungsi-fungsi itu. (Pengecualiannya: kalau sebuah
blok mendefinisikan variabel bernama sama, variabel itu akan menyembunyikan
yang ada di file scope.)

Ini yang paling mendekati apa yang kamu anggap sebagai scope "global" di
bahasa lain.

Contohnya:

``` {.c .numberLines}
#include <stdio.h>

int shared = 10;    // File scope! Terlihat di seluruh file setelah ini!

void func1(void)
{
    shared += 100;  // Sekarang shared bernilai 110
}

void func2(void)
{
    printf("%d\n", shared);  // Mencetak "110"
}

int main(void)
{
    func1();
    func2();
}
```

Perhatikan bahwa kalau `shared` dideklarasikan di bagian bawah file, kode
itu tidak akan dikompilasi. Variabel harus dideklarasikan _sebelum_
dipakai oleh fungsi mana pun.

Ada cara untuk memodifikasi item di file scope lebih jauh, yaitu dengan
[static](#static) dan [extern](#extern), tapi itu kita bahas nanti.
[i[Scope-->file]>]

## Scope pada Loop `for`

[i[Scope-->`for` loop]<]
Penulis sungguh tidak tahu harus menyebut ini apa, karena C11
§6.8.5.3¶1 tidak memberinya nama yang jelas. Kita juga sudah beberapa kali
melakukannya di panduan ini. Yaitu ketika kamu mendeklarasikan variabel di
klausa pertama sebuah loop `for`:

``` {.c}
for (int i = 0; i < 10; i++)
    printf("%d\n", i);

printf("%d\n", i);  // ILEGAL--i hanya ada di scope selama for-loop
```

Pada contoh itu, masa hidup `i` dimulai saat ia didefinisikan, dan
berlanjut selama loop berjalan.

Kalau badan loop dibungkus dalam sebuah blok, variabel yang didefinisikan
di `for`-loop terlihat dari scope dalam itu.

Kecuali, tentu saja, kalau scope dalam itu menyembunyikannya. Contoh gila
ini mencetak `999` sebanyak lima kali:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    for (int i = 0; i < 5; i++) {
        int i = 999;  // Menyembunyikan i di scope for-loop
        printf("%d\n", i);
    }
}
```
[i[Scope-->`for` loop]>]

## Catatan tentang Function Scope

[i[Scope-->function]<]
Spesifikasi C memang menyebut _function scope_, tapi istilah itu dipakai
khusus untuk _label_, yang belum kita bahas. Nanti di lain hari.
[i[Scope-->function]>]
[i[Scope]>]
