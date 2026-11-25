<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Keluar dari Program

[i[Exiting]<]

Ternyata ada banyak cara untuk melakukannya, bahkan ada cara untuk
memasang "hook" supaya sebuah fungsi dijalankan ketika program keluar.

Di bab ini kita akan menyelam dan melihatnya.

Kita sudah membahas makna kode exit status di bagian [Exit
Status](#exit-status), jadi lompat kembali ke sana dan ulas kalau perlu.

Semua fungsi di bagian ini ada di `<stdlib.h>`.

## Keluar Normal

Kita mulai dengan cara-cara biasa keluar dari program, lalu melompat ke
beberapa yang lebih jarang dan esoterik.

Ketika kamu keluar dari program secara normal, semua I/O stream yang
terbuka di-flush dan file sementara dihapus. Pada dasarnya ini keluar yang
rapi ketika semuanya dibereskan dan ditangani. Inilah yang kamu inginkan
hampir sepanjang waktu kecuali kamu punya alasan lain.

### Return dari `main()`

[i[Exiting-->return from `main()`]<]

Kalau kamu perhatikan, `main()` punya tipe kembalian `int`... namun aku jarang
sekali, kalaupun ada, me-`return` sesuatu dari `main()`.

Ini karena hanya untuk `main()` (dan aku tidak bisa cukup menekankan bahwa
kasus khusus ini _hanya_ berlaku untuk `main()` dan tidak ada fungsi lain
di mana pun) ada `return 0` _implisit_ kalau eksekusi jatuh melewati
ujung.

Kamu bisa `return` secara eksplisit dari `main()` kapan saja, dan sebagian
programmer merasa lebih _Benar_ untuk selalu menaruh `return` di akhir
`main()`. Tapi kalau tidak ditulis, C akan menaruhnya untukmu.

Jadi... berikut aturan `return` untuk `main()`:

* Kamu bisa mengembalikan exit status dari `main()` dengan pernyataan
  `return`. `main()` adalah satu-satunya fungsi dengan perilaku khusus ini.
  Memakai `return` di fungsi lain hanya kembali dari fungsi itu ke
  caller-nya.
* Kalau kamu tidak `return` secara eksplisit dan hanya jatuh melewati
  ujung `main()`, itu sama saja seperti kamu mengembalikan `0` atau
  `EXIT_SUCCESS`.

[i[Exiting-->return from `main()`]>]

### `exit()`

[i[Exiting-->return from `main()`]>]

Yang ini juga sudah muncul beberapa kali. Kalau kamu memanggil `exit()`
dari mana pun di programmu, program akan keluar di titik itu.

Argumen yang kamu oper ke `exit()` adalah exit status.

### Memasang Exit Handler dengan `atexit()`

[i[`atexit()` function]<]

Kamu bisa mendaftarkan fungsi yang dipanggil saat program keluar, baik
lewat return dari `main()` maupun lewat pemanggilan fungsi `exit()`.

Pemanggilan `atexit()` dengan nama fungsi handler akan melakukannya. Kamu
bisa mendaftarkan beberapa exit handler, dan mereka akan dipanggil dalam
urutan terbalik dari pendaftaran.

Berikut contohnya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

void on_exit_1(void)
{
    printf("Exit handler 1 called!\n");
}

void on_exit_2(void)
{
    printf("Exit handler 2 called!\n");
}

int main(void)
{
    atexit(on_exit_1);
    atexit(on_exit_2);
    
    printf("About to exit...\n");
}
```

Dan outputnya:

``` {.default}
About to exit...
Exit handler 2 called!
Exit handler 1 called!
```

[i[`atexit()` function]>]

## Keluar Lebih Cepat dengan `quick_exit()`

[i[`quick_exit()` function]<]

Ini mirip keluar normal, kecuali:

* File yang terbuka mungkin tidak di-flush.
* File sementara mungkin tidak dihapus.
* Handler `atexit()` tidak akan dipanggil.

Tapi ada cara untuk mendaftarkan exit handler: panggil `at_quick_exit()`
dengan cara yang analog dengan memanggil `atexit()`.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

void on_quick_exit_1(void)
{
    printf("Quick exit handler 1 called!\n");
}

void on_quick_exit_2(void)
{
    printf("Quick exit handler 2 called!\n");
}

void on_exit(void)
{
    printf("Normal exit--I won't be called!\n");
}

int main(void)
{
    at_quick_exit(on_quick_exit_1);
    at_quick_exit(on_quick_exit_2);

    atexit(on_exit);  // Ini tidak akan dipanggil

    printf("About to quick exit...\n");

    quick_exit(0);
}
```

Yang memberi output ini:

``` {.default}
About to quick exit...
Quick exit handler 2 called!
Quick exit handler 1 called!
```

Cara kerjanya persis seperti `exit()`/`atexit()`, kecuali bahwa
pem-flush-an file dan pembersihan mungkin tidak dilakukan.

[i[`quick_exit()` function]>]

## Nuke it from Orbit: `_Exit()`

[i[`_Exit()` function]<]

Memanggil `_Exit()` langsung keluar, titik. Tidak ada fungsi callback
on-exit yang dijalankan. File tidak di-flush. File sementara tidak
dihapus.

Pakai ini kalau kamu harus keluar _detik ini juga, demi apa pun_.

## Keluar Kadang-kadang: `assert()`

Pernyataan `assert()` dipakai untuk menegaskan bahwa sesuatu harus benar,
kalau tidak program akan keluar.

Developer sering memakai assert untuk menangkap error jenis
Seharusnya-Tidak-Mungkin-Terjadi.

``` {.c}
#define PI 3.14159

assert(PI > 3);   // Benar saja, jadi lanjut
```

versus:

``` {.c}
goats -= 100;

assert(goats >= 0);  // Tidak boleh ada kambing negatif
```

Dalam kasus itu, kalau kujalankan dan `goats` turun di bawah `0`, ini yang
terjadi:

``` {.default}
goat_counter: goat_counter.c:8: main: Assertion `goats >= 0' failed.
Aborted
```

dan aku dilempar kembali ke command line.

Ini kurang ramah pengguna, jadi hanya dipakai untuk hal-hal yang tidak akan
pernah dilihat pengguna. Dan sering orang [menulis makro assert sendiri
yang lebih mudah dimatikan](#my-assert).

[i[`_Exit()` function]>]

## Keluar Tidak Normal: `abort()`

[i[`abort()` function]<]

Kamu bisa memakai ini kalau ada yang salah sekali dan ingin
menandakannya kepada lingkungan luar. Ini juga tidak selalu membersihkan
file yang terbuka, dan sebagainya.

Aku jarang melihat ini dipakai.

Sedikit bocoran tentang _signal_: ini sebenarnya bekerja dengan memunculkan
[i[`SIGABRT` signal]] `SIGABRT` yang akan mengakhiri proses. 

Apa yang terjadi setelah itu tergantung sistem, tapi di sistem mirip Unix,
umumnya [flw[dump core|Core_dump]] saat program berakhir.

[i[`abort()` function]>]
[i[Exiting]>]
