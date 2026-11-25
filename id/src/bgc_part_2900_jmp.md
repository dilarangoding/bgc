<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Long Jump dengan `setjmp`, `longjmp` {#setjmp-longjmp}

[i[Long jumps]<]

Kita sudah melihat `goto`, yang melompat dalam function scope. Tapi
`longjmp()` memungkinkanmu melompat kembali ke titik eksekusi yang lebih
awal, kembali ke fungsi yang memanggil fungsi ini.

Ada banyak batasan dan peringatan, tapi ini bisa menjadi fungsi yang
berguna untuk keluar dari kedalaman call stack kembali ke keadaan yang
lebih awal.

Menurut pengalamanku, fungsionalitas ini sangat jarang dipakai.

## Memakai `setjmp` dan `longjmp`

[i[`setjmp()` function]<]
[i[`longjmp()` function]<]

Tarian yang akan kita lakukan di sini pada dasarnya adalah menaruh
penanda buku dalam eksekusi dengan `setjmp()`. Nanti, kita memanggil
`longjmp()` dan ia akan melompat kembali ke titik eksekusi sebelumnya
tempat kita menaruh penanda itu dengan `setjmp()`.

Dan ini bisa dilakukan bahkan kalau kamu sudah memanggil subfungsi.

Berikut demo singkat ketika kita memanggil fungsi beberapa tingkat ke
dalam lalu keluar darurat.

Kita akan memakai variabel file scope `env` untuk menyimpan _state_ saat
kita memanggil `setjmp()` sehingga bisa memulihkannya ketika memanggil
`longjmp()` nanti. Inilah variabel tempat kita mengingat "posisi" kita.

Variabel `env` bertipe `jmp_buf`, tipe opaque yang dideklarasikan di
`<setjmp.h>`.

``` {.c .numberLines}
#include <stdio.h>
#include <setjmp.h>

jmp_buf env;

void depth2(void)
{
    printf("Entering depth 2\n");
    longjmp(env, 3490);           // Keluar darurat
    printf("Leaving depth 2\n");  // Ini tidak akan terjadi
}

void depth1(void)
{
    printf("Entering depth 1\n");
    depth2();
    printf("Leaving depth 1\n");  // Ini tidak akan terjadi
}

int main(void)
{
    switch (setjmp(env)) {
      case 0:
          printf("Calling into functions, setjmp() returned 0\n");
          depth1();
          printf("Returned from functions\n");  // Ini tidak akan terjadi
          break;

      case 3490:
          printf("Bailed back to main, setjmp() returned 3490\n");
          break;
    }
}
```

Saat dijalankan, ini mengeluarkan:

``` {.default}
Calling into functions, setjmp() returned 0
Entering depth 1
Entering depth 2
Bailed back to main, setjmp() returned 3490
```

Kalau kamu mencoba mencocokkan output itu dengan kodenya, jelas ada hal yang
benar-benar _aneh_ sedang terjadi.

Salah satu yang paling mencolok: `setjmp()` mengembalikan nilai _dua kali_.
Apa-apaan ini? Sihir apa ini?!

Begini ceritanya: kalau `setjmp()` mengembalikan `0`, artinya kamu
berhasil menaruh "penanda buku" di titik itu.

Kalau ia mengembalikan nilai bukan nol, artinya kamu baru saja kembali ke
"penanda buku" yang ditaruh sebelumnya. (Dan nilai yang dikembalikan adalah
nilai yang kamu oper ke `longjmp()`.)

Dengan cara ini kamu bisa membedakan antara menaruh penanda dan kembali ke
penanda itu nanti.

Jadi ketika kode di atas memanggil `setjmp()` untuk pertama kali,
`setjmp()` _menyimpan_ state di variabel `env` dan mengembalikan `0`.
Nanti saat kita memanggil `longjmp()` dengan `env` yang sama, ia
memulihkan state itu dan `setjmp()` mengembalikan nilai yang dioper ke
`longjmp()`.

[i[`setjmp()` function]>]
[i[`longjmp()` function]>]

## Jebakan

Di balik layar, ini cukup lugas. Biasanya _stack pointer_ melacak lokasi di
memori tempat variabel lokal disimpan, dan _program counter_ melacak
alamat instruksi yang sedang dieksekusi^[Baik "stack pointer" maupun
"program counter" terkait dengan arsitektur dan implementasi C di
bawahnya, dan bukan bagian dari spesifikasi.].

Jadi kalau kita ingin melompat kembali ke fungsi yang lebih awal, pada
dasarnya tinggal memulihkan stack pointer dan program counter ke nilai yang
disimpan di variabel [i[`jmp_buf` type]] `jmp_buf`, dan memastikan nilai
kembaliannya diatur dengan benar. Lalu eksekusi akan dilanjutkan di sana.

Tapi berbagai faktor mengacaukan ini, menciptakan banyak jebakan
undefined behavior.

### Nilai Variabel Lokal

[i[`setjmp()` function]<]

Kalau kamu ingin nilai variabel lokal otomatis (bukan `static` dan bukan
`extern`) tetap bertahan di fungsi yang memanggil `setjmp()` setelah
[i[`longjmp()`]] `longjmp()` terjadi, kamu harus mendeklarasikan variabel
itu sebagai [i[`volatile` type qualifier-->with `setjmp()`]<] `volatile`.

Secara teknis, mereka hanya perlu `volatile` kalau nilainya berubah
antara saat `setjmp()` dipanggil dan `longjmp()` dipanggil^[Alasannya,
program mungkin menyimpan nilai sementara di _register CPU_ saat
mengerjakannya. Pada rentang waktu itu, register menyimpan nilai yang
benar, dan nilai di stack mungkin sudah usang. Lalu nanti nilai register
akan tertimpa dan perubahan pada variabel itu hilang.].

Misalnya, kalau kita menjalankan kode ini:

``` {.c}
int x = 20;

if (setjmp(env) == 0) {
    x = 30;
}
```

dan nanti `longjmp()` kembali, nilai `x` akan tak tentu.

Kalau ingin memperbaikinya, `x` harus `volatile`:

``` {.c}
volatile int x = 20;

if (setjmp(env) == 0) {
    x = 30;
}
```

[i[`setjmp()` function]>]

[i[`volatile` type qualifier-->with `setjmp()`]>]

Sekarang nilainya akan benar, yaitu `30`, setelah [i[`longjmp()`]]
`longjmp()` membawa kita kembali ke titik ini.

### Berapa Banyak State yang Disimpan?

[i[`setjmp()` function]<]
[i[`longjmp()` function]<]

Saat kamu `longjmp()`, eksekusi dilanjutkan di titik `setjmp()` yang
bersesuaian. Dan hanya itu.

[i[`setjmp()` function]>]

Spesifikasi menunjukkan bahwa itu sama saja seolah-olah kamu melompat
kembali ke fungsi pada titik itu dengan variabel lokal bernilai apa pun
yang mereka miliki saat pemanggilan `longjmp()` dilakukan.

[i[`longjmp()` function]>]

Hal-hal yang tidak dipulihkan, memparafrasekan spesifikasi, mencakup:

* Flag status floating point
* File yang terbuka
* Komponen lain apa pun dari abstract machine

### Kamu Tidak Boleh Menamai Apa Pun `setjmp`

Kamu tidak boleh punya identifier `extern` bernama `setjmp`. Atau, kalau
`setjmp` adalah makro, kamu tidak boleh meng-undefine-nya.

Keduanya undefined behavior.

### `setjmp()` Tidak Boleh Berada dalam Ekspresi yang Lebih Besar

[i[`setjmp()`-->in an expression]<]

Artinya, kamu tidak bisa melakukan sesuatu seperti ini:

``` {.c}
if (x == 12 && setjmp(env) == 0) { ... }
```

Itu terlalu kompleks untuk diizinkan spesifikasi karena rekayasa yang harus
terjadi saat membongkar stack dan sebagainya. Kita tidak bisa
`longjmp()` kembali ke ekspresi kompleks yang baru dieksekusi sebagian.

Jadi ada batas kompleksitas pada ekspresi itu. 

* Ia boleh menjadi seluruh ekspresi pengendali dari kondisional.

  ``` {.c}
  if (setjmp(env)) {...}
  ```

  ``` {.c}
  switch (setjmp(env)) {...}
  ```

* Ia boleh menjadi bagian dari ekspresi relasional atau kesamaan, selama
  operand lainnya adalah konstanta integer. Dan keseluruhannya adalah
  ekspresi pengendali dari kondisional.

  ``` {.c}
  if (setjmp(env) == 0) {...}
  ```

* Operand dari operasi logical NOT (`!`), yang menjadi seluruh ekspresi
  pengendali.

  ``` {.c}
  if (!setjmp(env)) {...}
  ```

* Ekspresi mandiri, mungkin di-cast ke `void`.

  ``` {.c}
  setjmp(env);
  ```
  ``` {.c}
  (void)setjmp(env);
  ```

[i[`setjmp()`-->in an expression]>]

### Kapan `longjmp()` Tidak Boleh Dilakukan?

[i[`lonjmp()`]<]

Ini undefined behavior kalau:

* Kamu belum memanggil `setjmp()` sebelumnya
* Kamu memanggil `setjmp()` dari thread lain
* Kamu memanggil `setjmp()` di scope sebuah variable length array (VLA),
  dan eksekusi sudah meninggalkan scope VLA itu sebelum `longjmp()`
  dipanggil.
* Fungsi yang berisi `setjmp()` sudah keluar sebelum `longjmp()`
  dipanggil.

Untuk yang terakhir, "keluar" mencakup return normal dari fungsi itu,
serta kasus ketika `longjmp()` lain sudah melompat kembali ke titik yang
"lebih awal" di call stack daripada fungsi yang dimaksud.

### Kamu Tidak Boleh Mengoper `0` ke `longjmp()`

Kalau kamu mencoba mengoper nilai `0` ke `longjmp()`, nilai itu akan diam-diam
diubah menjadi `1`.

Karena `setjmp()` pada akhirnya mengembalikan nilai ini, dan
`setjmp()` yang mengembalikan `0` punya makna khusus, mengembalikan `0`
dilarang.

### `longjmp()` dan Variable Length Array

Kalau kamu berada di scope sebuah VLA dan `longjmp()` keluar dari sana,
memori yang dialokasikan untuk VLA itu bisa bocor^[Artinya, tetap
teralokasi sampai program berakhir tanpa cara untuk membebaskannya.].

Hal yang sama terjadi kalau kamu `longjmp()` melewati fungsi-fungsi
sebelumnya yang masih punya VLA dalam scope.

Ini salah satu hal yang benar-benar mengganggu aku soal VLA---kamu
bisa menulis kode C yang sepenuhnya sah tapi menyia-nyiakan memori. Tapi
ya, aku bukan pemegang spesifikasi.

[i[`lonjmp()`]>]
[i[Long jumps]>]
