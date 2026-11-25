<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsi Variadic

[i[Variadic functions]<]

_Variadic_ adalah kata keren untuk fungsi yang menerima jumlah argumen
sembarang.

Fungsi biasa menerima jumlah argumen yang tertentu, misalnya:

``` {.c}
int add(int x, int y)
{
    return x + y;
}
```

Kamu hanya bisa memanggilnya dengan tepat dua argumen yang sesuai dengan
parameter `x` dan `y`.

``` {.c}
add(2, 3);
add(5, 12);
```

Tapi kalau kamu mencoba dengan lebih banyak, compiler tidak akan
mengizinkan:

``` {.c}
add(2, 3, 4);  // ERROR
add(5);        // ERROR
```

Fungsi variadic mengatasi batasan ini sampai tingkat tertentu.

Kita sudah melihat contoh terkenalnya di `printf()`! Kamu bisa mengoper
berbagai macam hal kepadanya.

``` {.c}
printf("Hello, world!\n");
printf("The number is %d\n", 2);
printf("The number is %d and pi is %f\n", 2, 3.14159);
```

Kelihatannya ia tidak peduli berapa banyak argumen yang diberikan!

Nah, itu tidak sepenuhnya benar. Nol argumen akan memberi error:

``` {.c}
printf();  // ERROR
```

Ini membawa kita ke salah satu batasan fungsi variadic di C: mereka harus
punya setidaknya satu argumen.

Tapi selain itu, mereka cukup fleksibel, bahkan membolehkan argumen
bertipe berbeda-beda seperti `printf()`.

Mari kita lihat cara kerjanya!

## Elipsis pada Signature Fungsi

Jadi bagaimana cara kerjanya, secara sintaksis?

[i[`...` variadic arguments]<]

Taruh semua argumen yang _harus_ dioper lebih dulu (dan ingat harus ada
setidaknya satu), lalu setelah itu taruh `...`. Seperti ini:

``` {.c}
void func(int a, ...)   // Secara harfiah 3 titik di sini
```

Berikut kode untuk mendemonstrasikannya:

``` {.c}
#include <stdio.h>

void func(int a, ...)
{
    printf("a is %d\n", a);  // Mencetak "a is 2"
}

int main(void)
{
    func(2, 3, 4, 5, 6);
}
```

[i[`...` variadic arguments]>]

Jadi, bagus, kita bisa mendapatkan argumen pertama yang ada di variabel
`a`, tapi bagaimana dengan argumen sisanya? Bagaimana cara mengaksesnya?

Di sinilah keseruan dimulai!

## Mengambil Argumen Tambahan

Kamu perlu meng-include [i[`stdarg.h` header file]] `<stdarg.h>` supaya
semua ini berfungsi.

[i[`va_list` type]<]

Pertama-tama, kita akan memakai variabel khusus bertipe `va_list`
(variable argument list) untuk melacak variabel mana yang sedang kita
akses.

[i[`va_start()` macro]<]
[i[`va_arg()` macro]<]
[i[`va_end()` macro]<]

Idenya: kita mulai memproses argumen dengan memanggil `va_start()`,
memproses tiap argumen secara berurutan dengan `va_arg()`, lalu setelah
selesai, menutupnya dengan `va_end()`.

Saat memanggil `va_start()`, kamu perlu mengoper _parameter bernama
terakhir_ (yang tepat sebelum `...`) supaya ia tahu dari mana mulai
mencari argumen tambahan.

Dan saat memanggil `va_arg()` untuk mengambil argumen berikutnya, kamu
harus memberi tahu tipe argumen yang akan diambil.

Berikut demo yang menjumlahkan sejumlah integer sembarang. Argumen pertama
adalah jumlah integer yang akan dijumlahkan. Kita memakainya untuk
mengetahui berapa kali harus memanggil `va_arg()`.

``` {.c .numberLines}
#include <stdio.h>
#include <stdarg.h>

int add(int count, ...)
{
    int total = 0;
    va_list va;

    va_start(va, count);   // Mulai dengan argumen setelah "count"

    for (int i = 0; i < count; i++) {
        int n = va_arg(va, int);   // Ambil int berikutnya

        total += n;
    }

    va_end(va);  // Selesai

    return total;
}

int main(void)
{
    printf("%d\n", add(4, 6, 2, -4, 17));  // 6 + 2 - 4 + 17 = 21
    printf("%d\n", add(2, 22, 44));        // 22 + 44 = 66
}
```

[i[`va_start()` macro]>]
[i[`va_end()` macro]>]

(Perhatikan bahwa saat `printf()` dipanggil, ia memakai jumlah `%d` (atau
sejenisnya) di format string untuk mengetahui berapa banyak argumen
tambahan!)

Kalau sintaks `va_arg()` terlihat aneh bagimu (karena ada nama tipe yang
melayang bebas di dalamnya), kamu tidak sendirian. Ini diimplementasikan
dengan makro preprocessor supaya semua sihirnya bisa masuk.

## Fungsionalitas `va_list`

Apa itu variabel `va_list` yang kita pakai di atas? Ia adalah variabel
opaque^[Maksudnya, kita para developer rendahan tidak seharusnya tahu apa
isinya atau artinya. Spesifikasi tidak menentukan secara rinci apa itu.]
yang menyimpan informasi tentang argumen mana yang akan kita ambil
berikutnya dengan `va_arg()`. Lihat bagaimana kita memanggil `va_arg()`
berulang kali? Variabel `va_list` adalah placeholder yang melacak
kemajuan sejauh ini.

[i[`va_start()` macro]<]

Tapi kita harus menginisialisasi variabel itu ke nilai yang masuk akal.
Di situlah `va_start()` berperan.

Saat memanggil `va_start(va, count)` di atas, kita berkata, "Inisialisasi
variabel `va` agar menunjuk ke argumen variabel _tepat setelah_ `count`."

[i[`va_end()` macro]<]

Dan itulah _alasan_ kita perlu punya setidaknya satu variabel bernama di
daftar argumen^[Jujur saja, batasan itu sebenarnya bisa dihapus dari
bahasa, tapi idenya adalah makro `va_start()`, `va_arg()`, dan `va_end()`
harus bisa ditulis dalam C. Dan supaya itu terwujud, kita butuh cara untuk
menginisialisasi pointer ke lokasi parameter pertama. Dan untuk itu, kita
butuh _nama_ parameter pertama. Itu akan butuh ekstensi bahasa untuk
dimungkinkan, dan sejauh ini komite belum menemukan alasan untuk
melakukannya.].

Setelah punya pointer ke parameter awal, kamu bisa dengan mudah mengambil
nilai argumen berikutnya dengan memanggil `va_arg()` berulang kali. Saat
melakukannya, kamu harus mengoper variabel `va_list` (supaya ia bisa terus
melacak posisimu), serta tipe argumen yang akan kamu salin.

Kamu sebagai programmer yang harus menentukan tipe apa yang dioper ke
`va_arg()`. Pada contoh di atas, kita hanya memakai `int`. Tapi pada
`printf()`, ia memakai format specifier untuk menentukan tipe apa yang
diambil berikutnya.

Dan saat selesai, panggil `va_end()` untuk menutupnya. Kamu **harus**
(kata spesifikasi) memanggilnya pada variabel `va_list` tertentu sebelum
memutuskan memanggil `va_start()` atau `va_copy()` lagi padanya. Aku tahu
kita belum membahas `va_copy()`.

Jadi urutan standarnya adalah:

* `va_start()` untuk menginisialisasi variabel `va_list`
* `va_arg()` berulang kali untuk mengambil nilai-nilainya
* `va_end()` untuk men-deinisialisasi variabel `va_list`

[i[`va_start()` macro]>]
[i[`va_end()` macro]>]
[i[`va_arg()` macro]>]

[i[`va_copy()` macro]<]

Aku juga sudah menyebut `va_copy()` di atas; ia membuat salinan variabel
`va_list`-mu dalam keadaan yang persis sama. Artinya, kalau kamu belum
mulai dengan `va_arg()` pada variabel sumber, yang baru juga belum
dimulai. Kalau kamu sudah mengambil 5 variabel dengan `va_arg()` sejauh
ini, salinannya juga mencerminkan itu.

`va_copy()` bisa berguna kalau kamu perlu memindai argumen lebih ke depan
tapi juga perlu mengingat posisimu saat ini.

[i[`va_copy()` macro]>]

## Fungsi Library yang Memakai `va_list`

[i[`va_list` type-->passing to functions]<]

Salah satu kegunaan lain cukup keren: menulis varian `printf()` kustommu
sendiri. Repot sekali kalau harus menangani semua format specifier itu,
kan? Semiliar jumlahnya?

Untungnya, ada varian `printf()` yang menerima `va_list` yang sudah jalan
sebagai argumen. Kamu bisa memakainya untuk membungkus dan membuat
`printf()` kustommu sendiri!

[i[`vprintf()` function]<]

Fungsi-fungsi ini diawali huruf `v`, seperti `vprintf()`, `vfprintf()`,
`vsprintf()`, dan `vsnprintf()`. Pada dasarnya semua `printf()` lawas
favoritmu, hanya dengan `v` di depan.

Mari kita buat fungsi `my_printf()` yang bekerja persis seperti `printf()`
kecuali ia menerima satu argumen ekstra di depan.

``` {.c .numberLines}
#include <stdio.h>
#include <stdarg.h>

int my_printf(int serial, const char *format, ...)
{
    va_list va;
    int rv;

    // Kerjakan bagian kustom
    printf("The serial number is: %d\n", serial);

    // Lalu serahkan sisanya ke vprintf()
    va_start(va, format);
    rv = vprintf(format, va);
    va_end(va);

    return rv;
}

int main(void)
{
    int x = 10;
    float y = 3.2;

    my_printf(3490, "x is %d, y is %f\n", x, y);
}
```

Lihat apa yang kita lakukan di situ? Di baris 12-14 kita memulai
variabel `va_list` baru, lalu langsung mengopernya ke `vprintf()`. Dan
`vprintf()` tahu persis apa yang harus dilakukan dengannya, karena ia
punya semua kecerdasan `printf()` bawaan.

[i[`vprintf()` function]>]

Kita tetap harus memanggil `va_end()` saat selesai, jadi jangan lupa!

[i[`va_list` type-->passing to functions]>]
[i[`va_list` type]>]

## Jebakan Makro Variadic

Seperti sudah kusebut, `va_start()` dan `va_end()` bisa berupa makro.
Salah satu konsekuensinya, mereka mungkin memperkenalkan scope lokal baru.
(Yaitu, kalau `va_start()` berisi `{` dan `va_end()` berisi `}`.)

[i[`va_start()` macro-->scoping issues]>]

Jadi kita perlu waspada terhadap kemungkinan masalah scope. Lihat contoh
berikut:

``` {.c}
va_start(va, format);          // mungkin berisi {
int rv = vprintf(format, va);
va_end(va);                    // mungkin berisi }

return rv;
```

Kalau `va_start()` membuka scope baru, `rv` akan lokal di scope itu dan
pernyataan `return` akan gagal. Tapi ini diam-diam hanya akan terjadi di
compiler yang kebetulan melakukan itu dengan makro `va`.

[i[Variadic functions]>]
