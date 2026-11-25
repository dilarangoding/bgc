<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# C Preprocessor

[i[Preprocessor]<]

Sebelum programmu dikompilasi, ia sebenarnya melewati tahap yang disebut
_preprocessing_. Hampir seperti ada bahasa _di atas_ bahasa C yang berjalan
lebih dulu. Dan ia menghasilkan kode C, yang kemudian dikompilasi.

Kita sudah melihatnya sedikit lewat `#include`! Itulah C Preprocessor! Di
tempat ia menemukan directive itu, ia menyertakan file yang disebut tepat
di situ, seolah-olah kamu mengetiknya sendiri. Dan _baru kemudian_
compiler membangun semuanya.

Tapi ternyata kemampuannya jauh lebih besar daripada sekadar menyertakan
sesuatu. Kamu bisa mendefinisikan _makro_ yang disubstitusi... bahkan
makro yang menerima argumen!

## `#include`

[i[`#include` directive]<]

Kita mulai dari yang sudah sering kita lihat. Ini tentu saja cara untuk
menyertakan sumber lain ke dalam sumbermu. Sangat umum dipakai dengan
header file.

Walaupun spesifikasi mengizinkan berbagai perilaku untuk `#include`, kita
akan mengambil pendekatan yang lebih pragmatis dan membahas cara kerjanya
di setiap sistem yang pernah kulihat.

Header file bisa dibagi dua kategori: sistem dan lokal. Yang bawaan,
seperti `stdio.h`, `stdlib.h`, `math.h`, dan sebagainya, di-include dengan
kurung siku:

``` {.c}
#include <stdio.h>
#include <stdlib.h>
```

Kurung siku memberi tahu C, "Hei, jangan cari header file ini di direktori
saat ini---cari di direktori include level sistem."

[i[`#include` directive-->local files]<]

Itu tentu saja menyiratkan bahwa harus ada cara untuk meng-include file
lokal dari direktori saat ini. Dan memang ada: dengan tanda kutip ganda:

``` {.c}
#include "myheader.h"
```

Atau kamu juga hampir pasti bisa mencari di direktori relatif memakai garis
miring dan titik, seperti ini:

``` {.c}
#include "mydir/myheader.h"
#include "../someheader.py"
```

Jangan pakai backslash (`\`) sebagai pemisah path di `#include`-mu!
Itu undefined behavior! Pakai garis miring biasa (`/`) saja, bahkan di
Windows.

Ringkasnya, pakai kurung siku (`<` dan `>`) untuk include sistem, dan
pakai tanda kutip ganda (`"`) untuk include pribadimu.

[i[`#include` directive-->local files]>]
[i[`#include` directive]>]

## Makro Sederhana

[i[Preprocessor-->macros]<]

_Makro_ adalah identifier yang _diperluas_ menjadi potongan kode lain
sebelum compiler sempat melihatnya. Anggap seperti placeholder---ketika
preprocessor melihat salah satu identifier itu, ia menggantinya dengan
nilai lain yang sudah kamu definisikan.

[i[`#define` directive]<]

Kita melakukannya dengan `#define` (sering dibaca "pound define", atau
mungkin "hash define", tapi jarang, kalaupun ada, "octothorpe define").
Berikut contohnya:

``` {.c .numberLines}
#include <stdio.h>

#define HELLO "Hello, world"
#define PI 3.14159

int main(void)
{
    printf("%s, %f\n", HELLO, PI);
}
```

Di baris 3 dan 4 kita mendefinisikan beberapa makro. Di mana pun makro itu
muncul di kode (baris 8), mereka akan disubstitusi dengan nilai yang
didefinisikan.

Dari sudut pandang C compiler, itu persis seperti kalau kita menulis ini:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    printf("%s, %f\n", "Hello, world", 3.14159);
}
```

Lihat bagaimana `HELLO` diganti dengan `"Hello, world"` dan `PI` diganti
dengan `3.14159`? Dari sudut pandang compiler, itu persis seperti
nilai-nilai itu muncul langsung di kode.

Perhatikan bahwa makro tidak punya tipe tertentu, _per se_. Yang terjadi
hanyalah mereka diganti utuh dengan apa pun yang di-`#define`-kan. Kalau
kode C hasilnya tidak valid, compiler akan muntah.

Kamu juga bisa mendefinisikan makro tanpa nilai:

``` {.c}
#define EXTRA_HAPPY
```

dalam kasus itu, makro-nya ada dan terdefinisi, tapi didefinisikan sebagai
kosong. Jadi di mana pun ia muncul di teks, akan diganti dengan kosong.
Kita akan melihat kegunaannya nanti.

Secara konvensi, nama makro ditulis `HURUF_BESAR_SEMUA` walaupun secara
teknis itu tidak wajib.

[i[`#define` directive-->versus `const`]<]

Secara keseluruhan, ini memberimu cara untuk mendefinisikan nilai konstan
yang efektif bersifat global dan bisa dipakai di tempat _mana saja_.
Bahkan di tempat yang tidak bisa memakai variabel `const`, misalnya pada
`case` di `switch` dan panjang array tetap.

Meski begitu, perdebatan masih ramai di internet soal apakah variabel
`const` bertipe lebih baik daripada makro `#define` secara umum.

Ia juga bisa dipakai untuk mengganti atau memodifikasi kata kunci, konsep
yang sama sekali asing bagi `const`, walaupun praktik ini sebaiknya
jarang dilakukan.

[i[`#define` directive-->versus `const`]>]
[i[`#define` directive]>]
[i[Preprocessor-->macros]>]

## Kompilasi Kondisional

[i[Conditional compilation]<]

Preprocessor bisa diminta menentukan apakah blok kode tertentu
ditampilkan ke compiler atau dibuang sepenuhnya sebelum kompilasi.

Caranya dengan membungkus kode dalam blok kondisional, mirip pernyataan
`if`-`else`.

### Jika Terdefinisi, `#ifdef` dan `#endif`

Pertama, mari kita coba mengompilasi kode tertentu bergantung pada apakah
sebuah makro terdefinisi atau tidak.

[i[`#ifdef` directive]<]
[i[`#endif` directive]<]

``` {.c .numberLines}
#include <stdio.h>

#define EXTRA_HAPPY

int main(void)
{

#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#endif

    printf("OK!\n");
}
```

Pada contoh itu, kita mendefinisikan `EXTRA_HAPPY` (sebagai kosong, tapi
ia _terdefinisi_), lalu di baris 8 kita memeriksa apakah ia terdefinisi
dengan directive `#ifdef`. Kalau terdefinisi, kode setelahnya disertakan
sampai `#endif`.

[i[`#ifdef` directive]>]
[i[`#endif` directive]>]

Jadi karena terdefinisi, kode itu disertakan untuk kompilasi dan
outputnya:

``` {.default}
I'm extra happy!
OK!
```

Kalau `#define`-nya kita jadikan komentar, seperti ini:

``` {.c}
//#define EXTRA_HAPPY
```

maka ia tidak terdefinisi, dan kode itu tidak disertakan dalam kompilasi.
Dan outputnya hanya:

``` {.default}
OK!
```

Penting diingat bahwa keputusan ini terjadi saat compile time! Kodenya
benar-benar dikompilasi atau dibuang tergantung kondisi. Ini berbeda
dengan pernyataan `if` biasa yang dievaluasi saat program berjalan.

### Jika Tidak Terdefinisi, `#ifndef`

Ada juga bentuk negatif dari "jika terdefinisi": "jika tidak
terdefinisi", atau `#ifndef`. Kita bisa mengubah contoh sebelumnya
supaya mengeluarkan hal berbeda tergantung apakah sesuatu terdefinisi:

[i[`#ifndef` directive]<]
[i[`#endif` directive]<]

``` {.c .numberLines startFrom="8"}
#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#endif

#ifndef EXTRA_HAPPY
    printf("I'm just regular\n");
#endif
```

Kita akan lihat cara yang lebih rapi untuk itu di bagian berikutnya.

Kalau dikaitkan kembali ke header file, kita sudah melihat bagaimana
header file bisa dibuat hanya di-include satu kali dengan
membungkusnya dalam directive preprocessor seperti ini:

``` {.c}
#ifndef MYHEADER_H  // Baris pertama myheader.h
#define MYHEADER_H

int x = 12;

#endif  // Baris terakhir myheader.h
```

[i[`#ifndef` directive]>]
[i[`#endif` directive]>]

Ini menunjukkan bagaimana sebuah makro bertahan lintas file dan beberapa
`#include`. Kalau belum terdefinisi, kita definisikan lalu kompilasi
seluruh header file.

Tapi saat di-include lagi, kita lihat bahwa `MYHEADER_H` _sudah_
terdefinisi, jadi kita tidak mengirim header file itu ke compiler---ia
secara efektif dibuang.


### `#else`

[i[`#else` directive]<]

Tapi itu belum semua yang bisa kita lakukan! Ada juga `#else` yang bisa
kita campurkan.

[i[`#ifdef` directive]<]
[i[`#endif` directive]<]

Mari kita modifikasi contoh sebelumnya:

``` {.c .numberLines startFrom="8"}
#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#else
    printf("I'm just regular\n");
#endif
```

[i[`#ifdef` directive]>]
[i[`#else` directive]>]
[i[`#endif` directive]>]

Sekarang kalau `EXTRA_HAPPY` tidak terdefinisi, ia akan masuk ke klausa
`#else` dan mencetak:

``` {.default}
I'm just regular
```

### Else-If: `#elifdef`, `#elifndef`

[i[`#elifdef` directive]<]
[i[`#elifndef` directive]<]

Fitur ini baru di C23!

Bagaimana kalau kamu ingin sesuatu yang lebih kompleks? Mungkin kamu
butuh struktur rangkaian if-else supaya kodemu terbangun dengan benar?

Untungnya directive-directive ini tersedia. Kita bisa memakai `#elifdef`
untuk "else if defined":

``` {.c}
#ifdef MODE_1
    printf("This is mode 1\n");
#elifdef MODE_2
    printf("This is mode 2\n");
#elifdef MODE_3
    printf("This is mode 3\n");
#else
    printf("This is some other mode\n");
#endif
```

[i[`#elifdef` directive]>]

Sebaliknya, kamu bisa memakai `#elifndef` untuk "else if not defined".

[i[`#elifndef` directive]>]

### Kondisional Umum: `#if`, `#elif`

[i[`#if` directive]<]
[i[`#elif` directive]<]

Ini bekerja sangat mirip directive `#ifdef` dan `#ifndef`, yaitu kamu juga
bisa punya `#else` dan semuanya ditutup dengan `#endif`.

Satu-satunya perbedaan: ekspresi konstan setelah `#if` harus bernilai true
(bukan nol) agar kode di dalam `#if` dikompilasi. Jadi alih-alih apakah
sesuatu terdefinisi atau tidak, kita ingin ekspresi yang bernilai true.

``` {.c .numberLines}
#include <stdio.h>

#define HAPPY_FACTOR 1

int main(void)
{

#if HAPPY_FACTOR == 0
    printf("I'm not happy!\n");
#elif HAPPY_FACTOR == 1
    printf("I'm just regular\n");
#else
    printf("I'm extra happy!\n");
#endif

    printf("OK!\n");
}
```

[i[`#elif` directive]>]

Sekali lagi, untuk klausa `#if` yang tidak cocok, compiler bahkan tidak
akan melihat baris-baris itu. Untuk kode di atas, setelah preprocessor
selesai, yang dilihat compiler hanyalah:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{

    printf("I'm just regular\n");

    printf("OK!\n");
}
```

Salah satu pemakaian akal-akalan dari hal ini adalah mengomentari banyak
baris dengan cepat^[Kamu tidak selalu bisa langsung membungkus kode
dengan komentar `/*` `*/` karena komentar itu tidak bisa bersarang.].

[i[`#if 0` directive]<]

Kalau kamu menaruh `#if 0` ("if false") di awal blok yang ingin
dikomentari dan `#endif` di akhir, kamu mendapat efek ini:

``` {.c}
#if 0
    printf("All this code"); /* is effectively */
    printf("commented out"); // by the #if 0
#endif
```

[i[`#if 0` directive]>]

Bagaimana kalau kamu memakai compiler pra-C23 dan tidak ada dukungan
directive `#elifdef` atau `#elifndef`? Bagaimana mendapat efek yang sama
dengan `#if`? Artinya, bagaimana kalau kita menginginkan ini:

``` {.c}
#ifdef FOO
    x = 2;
#elifdef BAR  // KEMUNGKINAN ERROR: Tidak didukung sebelum C23
    x = 3;
#endif
```

Bagaimana caranya?

Ternyata ada operator preprocessor bernama `defined` yang bisa dipakai
dengan pernyataan `#if`.

Ketiganya setara:

[i[`#if defined` directive]<]
``` {.c}
#ifdef FOO
#if defined FOO
#if defined(FOO)   // Tanda kurung opsional
```

Begitu juga ketiga ini:

``` {.c}
#ifndef FOO
#if !defined FOO
#if !defined(FOO)   // Tanda kurung opsional
```

Perhatikan bahwa kita bisa memakai operator logika NOT standar (`!`) untuk
"tidak terdefinisi".

Jadi sekarang kita kembali ke dunia `#if` dan bisa memakai `#elif` sesuka
hati!

Kode yang rusak ini:

``` {.c}
#ifdef FOO
    x = 2;
#elifdef BAR  // KEMUNGKINAN ERROR: Tidak didukung sebelum C23
    x = 3;
#endif
```

bisa diganti dengan:

``` {.c}
#if defined FOO
    x = 2;
#elif defined BAR
    x = 3;
#endif
```

[i[`#if defined` directive]>]
[i[`#if` directive]>]
[i[Conditional compilation]>]

### Menghapus Makro: `#undef`

[i[`#undef` directive]<]

Kalau kamu sudah mendefinisikan sesuatu tapi tidak membutuhkannya lagi,
kamu bisa membatalkan definisinya dengan `#undef`.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
#define GOATS

#ifdef GOATS
    printf("Goats detected!\n");  // tercetak
#endif

#undef GOATS  // Buat GOATS tidak terdefinisi lagi

#ifdef GOATS
    printf("Goats detected, again!\n"); // tidak tercetak
#endif
}
```

[i[`#undef` directive]>]

## Makro Bawaan

[i[Preprocessor-->predefined macros]<]

Standar mendefinisikan banyak makro bawaan yang bisa diuji dan dipakai
untuk kompilasi kondisional. Mari kita lihat di sini.


### Makro Wajib

Semua ini terdefinisi:

[i[`__DATE__` macro]<]
[i[`__TIME__` macro]<]
[i[`__FILE__` macro]<]
[i[`__LINE__` macro]<]
[i[`__func__` identifier]<]
[i[`__STDC_VERSION__` macro]<]

[i[`__STDC__` macro]]
[i[`__STDC_HOSTED__` macro]]

|Makro|Deskripsi|
|-----------|----------------------------------------------------|
|`__DATE__`|Tanggal kompilasi---yaitu saat file ini dikompilasi---dengan format `Mmm dd yyyy`|
|`__TIME__`|Waktu kompilasi dengan format `hh:mm:ss`|
|`__FILE__`|String berisi nama file ini|
|`__LINE__`|Nomor baris di file tempat makro ini muncul|
|`__func__`|Nama fungsi tempat ini muncul, sebagai string^[Ini sebenarnya bukan makro---secara teknis ia identifier. Tapi ia satu-satunya identifier bawaan dan terasa sangat seperti makro, jadi kumasukkan di sini. Seperti pemberontak.]|
|`__STDC__`|Bernilai `1` kalau ini C compiler standar|
|`__STDC_HOSTED__`|Bernilai `1` kalau compiler adalah _hosted implementation_^[Hosted implementation pada dasarnya berarti kamu menjalankan standar C yang lengkap, kemungkinan di atas semacam sistem operasi. Dan kemungkinan besar memang begitu keadaanmu. Kalau kamu berjalan di bare metal pada semacam embedded system, kemungkinan kamu memakai _standalone implementation_.], selain itu `0`|
|`__STDC_VERSION__`|Versi C ini, berupa konstanta `long int` dengan bentuk `yyyymmL`, mis. `201710L`|

Mari kita gabungkan semuanya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    printf("This function: %s\n", __func__);
    printf("This file: %s\n", __FILE__);
    printf("This line: %d\n", __LINE__);
    printf("Compiled on: %s %s\n", __DATE__, __TIME__);
    printf("C Version: %ld\n", __STDC_VERSION__);
}
```

[i[`__DATE__` macro]>]
[i[`__TIME__` macro]>]

Output di sistemku:

``` {.default}
This function: main
This file: foo.c
This line: 7
Compiled on: Nov 23 2020 17:16:27
C Version: 201710
```

`__FILE__`, `__func__` dan `__LINE__` sangat berguna untuk melaporkan
kondisi error dalam pesan kepada developer. Makro `assert()` di
`<assert.h>` memakai ketiganya untuk menunjukkan di bagian kode mana
assertion gagal.

[i[`__FILE__` macro]>]
[i[`__LINE__` macro]>]
[i[`__func__` identifier]>]

#### `__STDC_VERSION__`

[i[Language versions]<]

Kalau kamu penasaran, berikut nomor versi untuk rilis-rilis utama
Spesifikasi Bahasa C:

|Rilis|Versi ISO/IEC|`__STDC_VERSION__`|
|-|-|-|
|C89|ISO/IEC 9899:1990|tidak terdefinisi|
|**C89**|ISO/IEC 9899:1990/Amd.1:1995|`199409L`|
|**C99**|ISO/IEC 9899:1999|`199901L`|
|**C11**|ISO/IEC 9899:2011/Amd.1:2012|`201112L`|

Perhatikan bahwa makro ini awalnya tidak ada di C89.

Perhatikan juga bahwa rencananya nomor versi akan selalu naik, jadi kamu
selalu bisa memeriksa, misalnya, "setidaknya C99" dengan:

``` {.c}
#if __STDC_VERSION__ >= 1999901L
```

[i[Language versions]>]
[i[`__STDC_VERSION__` macro]>]

### Makro Opsional

Implementasimu mungkin mendefinisikan ini juga. Atau mungkin tidak.

[i[`__STDC_ISO_10646__` macro]]
[i[`__STDC_MB_MIGHT_NEQ_WC__` macro]]
[i[`__STDC_UTF_16__` macro]]
[i[`__STDC_UTF_32__` macro]]
[i[`__STDC_ANALYZABLE__` macro]]
[i[`__STDC_IEC_559__` macro]]
[i[`__STDC_IEC_559_COMPLEX__` macro]]
[i[`__STDC_LIB_EXT1__` macro]]
[i[`__STDC_NO_ATOMICS__` macro]]
[i[`__STDC_NO_COMPLEX__` macro]]
[i[`__STDC_NO_THREADS__` macro]]
[i[`__STDC_NO_VLA__` macro]]

|Makro|Deskripsi|
|----------------|--------------------------------------------------|
|`__STDC_ISO_10646__`|Jika terdefinisi, `wchar_t` menyimpan nilai Unicode, selain itu sesuatu yang lain|
|`__STDC_MB_MIGHT_NEQ_WC__`|Nilai `1` menandakan bahwa nilai pada karakter multibyte mungkin tidak sama dengan nilai pada wide character|
|`__STDC_UTF_16__`|Nilai `1` menandakan bahwa sistem memakai encoding UTF-16 pada tipe `char16_t`|
|`__STDC_UTF_32__`|Nilai `1` menandakan bahwa sistem memakai encoding UTF-32 pada tipe `char32_t`|
|`__STDC_ANALYZABLE__`|Nilai `1` menandakan bahwa kode itu analyzable^[Oke, aku tahu itu jawaban yang mengelak. Pada dasarnya ada ekstensi opsional yang bisa diimplementasikan compiler, yaitu mereka setuju membatasi jenis undefined behavior tertentu supaya kode C lebih mudah dianalisis secara statis. Kecil kemungkinan kamu perlu memakainya.]|
|`__STDC_IEC_559__`|`1` jika floating point IEEE-754 (alias IEC 60559) didukung|
|`__STDC_IEC_559_COMPLEX__`|`1` jika floating point kompleks IEC 60559 didukung|
|`__STDC_LIB_EXT1__`|`1` jika implementasi ini mendukung berbagai fungsi standard library alternatif yang "aman" (nama mereka berakhiran `_s`)|
|`__STDC_NO_ATOMICS__`|`1` jika implementasi ini **tidak** mendukung `_Atomic` atau `<stdatomic.h>`|
|`__STDC_NO_COMPLEX__`|`1` jika implementasi ini **tidak** mendukung tipe kompleks atau `<complex.h>`|
|`__STDC_NO_THREADS__`|`1` jika implementasi ini **tidak** mendukung `<threads.h>`|
|`__STDC_NO_VLA__`|`1` jika implementasi ini **tidak** mendukung variable-length array|

[i[Preprocessor-->predefined macros]>]

## Makro dengan Argumen

[i[Preprocessor-->macros with arguments]<]

Makro lebih kuat daripada sekadar substitusi. Kamu bisa mengaturnya agar
menerima argumen yang ikut disubstitusi.

Pertanyaan yang sering muncul: kapan memakai makro berparameter versus
fungsi. Jawaban singkat: pakai fungsi. Tapi kamu akan melihat banyak makro
di alam liar dan di standard library. Orang cenderung memakainya untuk
hal-hal singkat yang bersifat matematis, dan juga untuk fitur yang mungkin
berubah dari satu platform ke platform lain. Kamu bisa mendefinisikan kata
kunci berbeda untuk satu platform atau lainnya.

### Makro dengan Satu Argumen

Kita mulai dengan yang sederhana, yang mengkuadratkan sebuah angka:

[i[`#define` directive]<]

``` {.c .numberLines}
#include <stdio.h>

#define SQR(x) x * x  // Belum tepat, tapi ikuti saja dulu

int main(void)
{
    printf("%d\n", SQR(12));  // 144
}
```

Maksudnya: "di mana pun kamu melihat `SQR` dengan suatu nilai, ganti dengan
nilai itu dikali dirinya sendiri".

Jadi baris 7 akan diubah menjadi:

``` {.c .numberLines startFrom="7"}
    printf("%d\n", 12 * 12);  // 144
```

yang dengan nyaman dikonversi C menjadi 144.

Tapi kita membuat kesalahan elementer pada makro itu, kesalahan yang harus
dihindari.

Mari kita periksa. Bagaimana kalau kita ingin menghitung `SQR(3 + 4)`?
Nah, $3+4=7$, jadi tentu kita ingin menghitung $7^2=49$. Itu dia; `49`---
jawaban akhir.

Mari kita masukkan ke kode dan lihat bahwa kita mendapat... 19?

``` {.c .numberLines startFrom="7"}
    printf("%d\n", SQR(3 + 4));  // 19!!??
```

Apa yang terjadi?

Kalau kita ikuti perluasan makronya, kita mendapat 

``` {.c .numberLines startFrom="7"}
    printf("%d\n", 3 + 4 * 3 + 4);  // 19!
```

Waduh! Karena perkalian lebih diutamakan, kita menghitung $4\times3=12$
lebih dulu, dan mendapat $3+12+4=19$. Bukan yang kita inginkan.

Jadi kita harus memperbaikinya agar benar.

**Hal ini sangat umum, jadi lakukan saja secara otomatis setiap kali
membuat makro matematika berparameter!**

Perbaikannya mudah: tambahkan tanda kurung!

``` {.c .numberLines startFrom="3"}
#define SQR(x) (x) * (x)   // Lebih baik... tapi masih belum cukup bagus!
```

Dan sekarang makro kita diperluas menjadi:

``` {.c .numberLines startFrom="7"}
    printf("%d\n", (3 + 4) * (3 + 4));  // 49! Horeee!
```

Tapi sebenarnya kita masih punya masalah yang sama, yang bisa muncul kalau
ada operator berprioritas lebih tinggi daripada perkalian (`*`) di
dekatnya.

Jadi cara yang aman dan benar untuk menyusun makro itu adalah dengan
membungkus keseluruhannya dalam tanda kurung tambahan, seperti ini:

``` {.c .numberLines startFrom="3"}
#define SQR(x) ((x) * (x))   // Bagus!
```

Biasakan melakukan itu setiap kali membuat makro matematika dan kamu tidak
akan salah.

### Makro dengan Lebih dari Satu Argumen

Kamu bisa menumpuk ini sebanyak yang kamu mau:

``` {.c}
#define TRIANGLE_AREA(w, h) (0.5 * (w) * (h))
```

Mari kita buat beberapa makro yang menyelesaikan $x$ dengan rumus
kuadrat. Kalau-kalau kamu belum hafal, untuk persamaan
berbentuk:

$ax^2+bx+c=0$

kamu bisa mencari $x$ dengan rumus kuadrat:

$x=\displaystyle\frac{-b\pm\sqrt{b^2-4ac}}{2a}$

Rumus yang gila. Perhatikan juga plus-minus ($\pm$) di sana, yang menandakan
bahwa sebenarnya ada dua solusi.

Jadi mari kita buat makro untuk keduanya:

``` {.c}
#define QUADP(a, b, c) ((-(b) + sqrt((b)*(b) - 4 * (a) * (c))) / (2 * (a)))
#define QUADM(a, b, c) ((-(b) - sqrt((b)*(b) - 4 * (a) * (c))) / (2 * (a)))
```

Itu sudah memberi kita sedikit matematika. Tapi mari kita definisikan satu
lagi yang bisa dipakai sebagai argumen `printf()` untuk mencetak kedua
jawaban.

``` {.c}
//          makro              penggantian
//      |-----------| |----------------------------|
#define QUAD(a, b, c) QUADP(a, b, c), QUADM(a, b, c)
```

Itu hanya dua nilai yang dipisahkan koma---dan kita bisa memakainya sebagai
argumen "gabungan" untuk `printf()` seperti ini:

``` {.c}
printf("x = %f or x = %f\n", QUAD(2, 10, 5));
```

Mari kita satukan dalam kode:

``` {.c .numberLines}
#include <stdio.h>
#include <math.h>  // Untuk sqrt()

#define QUADP(a, b, c) ((-(b) + sqrt((b) * (b) - 4 * (a) * (c))) / (2 * (a)))
#define QUADM(a, b, c) ((-(b) - sqrt((b) * (b) - 4 * (a) * (c))) / (2 * (a)))
#define QUAD(a, b, c) QUADP(a, b, c), QUADM(a, b, c)

int main(void)
{
    printf("2*x^2 + 10*x + 5 = 0\n");
    printf("x = %f or x = %f\n", QUAD(2, 10, 5));
}
```

Dan ini memberi kita output:

``` {.default}
2*x^2 + 10*x + 5 = 0
x = -0.563508 or x = -4.436492
```

Memasukkan salah satu nilai itu memberi hasil kira-kira nol (agak meleset
karena angkanya tidak persis):

$2\times-0.563508^2+10\times-0.563508+5\approx0.000003$

### Makro dengan Argumen Variabel

[i[Preprocessor-->macros with variable arguments]<]

Ada juga cara untuk mengoper jumlah argumen yang bervariasi ke makro,
dengan elipsis (`...`) setelah argumen bernama yang sudah diketahui. Saat
makro diperluas, semua argumen tambahan akan berada dalam daftar yang
dipisahkan koma di makro `__VA_ARGS__`, dan bisa diganti dari sana:

``` {.c .numberLines}
#include <stdio.h>

// Gabungkan dua argumen pertama menjadi satu angka,
// lalu jadikan sisanya daftar yang dipisahkan koma:

#define X(a, b, ...) (10*(a) + 20*(b)), __VA_ARGS__

int main(void)
{
    printf("%d %f %s %d\n", X(5, 4, 3.14, "Hi!", 12));
}
```

Substitusi yang terjadi di baris 10 adalah:

``` {.c .numberLines startFrom="10"}
    printf("%d %f %s %d\n", (10*(5) + 20*(4)), 3.14, "Hi!", 12);
```

dengan output:

``` {.default}
130 3.140000 Hi! 12
```

Kamu juga bisa men-"stringify" `__VA_ARGS__` dengan menaruh `#` di depannya:

``` {.c}
#define X(...) #__VA_ARGS__

printf("%s\n", X(1,2,3));  // Mencetak "1, 2, 3"
```

[i[`#define` directive]>]
[i[Preprocessor-->macros with variable arguments]>]
[i[Preprocessor-->macros with arguments]>]

### Stringification

[i[`#` stringification]<]

Sudah disebut di atas, kamu bisa mengubah argumen apa pun menjadi string
dengan menaruh `#` di depannya pada teks penggantian.

Misalnya, kita bisa mencetak apa pun sebagai string dengan makro ini dan
`printf()`:

``` {.c}
#define STR(x) #x

printf("%s\n", STR(3.14159));
```

Dalam kasus itu, substitusinya menghasilkan:

``` {.c}
printf("%s\n", "3.14159");
```

Mari kita lihat apakah ini bisa dipakai lebih efektif sehingga kita bisa
mengoper nama variabel `int` mana pun ke makro, dan makro itu mencetak
nama dan nilainya.

``` {.c .numberLines}
#include <stdio.h>

#define PRINT_INT_VAL(x) printf("%s = %d\n", #x, x)

int main(void)
{
    int a = 5;

    PRINT_INT_VAL(a);  // mencetak "a = 5"
}
```

Di baris 9, kita mendapat penggantian makro berikut:

``` {.c .numberLines startFrom="9"}
    printf("%s = %d\n", "a", a);
```

[i[`#` stringification]>]

### Concatenation

[i[`##` concatenation]<]

Kita juga bisa menyambung dua argumen dengan `##`. Seru!

``` {.c}
#define CAT(a, b) a ## b

printf("%f\n", CAT(3.14, 1592));   // 3.141592
```

[i[`##` concatenation]>]

## Makro Multibaris

[i[Preprocessor-->multiline macros]<]

Makro bisa dilanjutkan ke beberapa baris kalau newline di-escape dengan
backslash (`\`).

Mari kita tulis makro multibaris yang mencetak angka dari `0` sampai hasil
kali dua argumen yang dioper.

[i[`do`-`while` statement-->in multiline macros]<]

``` {.c .numberLines}
#include <stdio.h>

#define PRINT_NUMS_TO_PRODUCT(a, b) do { \
    int product = (a) * (b); \
    for (int i = 0; i < product; i++) { \
        printf("%d\n", i); \
    } \
} while(0)

int main(void)
{
    PRINT_NUMS_TO_PRODUCT(2, 4);  // Mengeluarkan angka dari 0 sampai 7
}
```

Beberapa hal yang perlu dicatat:

* Escape di akhir setiap baris kecuali yang terakhir untuk menandakan
  bahwa makro berlanjut.
* Seluruhnya dibungkus dalam loop `do`-`while(0)` dengan kurung kurawal.

Poin kedua mungkin agak aneh, tapi semuanya soal menyerap `;` di
belakang yang dijatuhkan programmer setelah makro.

Awalnya aku mengira kurung kurawal saja sudah cukup, tapi ada kasus
yang gagal kalau programmer menaruh titik koma setelah makro. Inilah
kasusnya:

``` {.c .numberLines}
#include <stdio.h>

#define FOO(x) { (x)++; }

int main(void)
{
    int i = 0;

    if (i == 0)
        FOO(i);
    else
        printf(":-(\n");

    printf("%d\n", i);
}
```

Kelihatannya sederhana, tapi tidak akan terbangun tanpa syntax error:

``` {.default}
foo.c:11:5: error: ‘else’ without a previous ‘if’  
```

Kamu bisa lihat masalahnya?

Mari kita lihat perluasannya:

``` {.c}

    if (i == 0) {
        (i)++;
    };             // <-- Masalah dengan M huruf besar!

    else
        printf(":-(\n");
```

`;` itu mengakhiri pernyataan `if`, jadi `else` melayang-layang di sana
secara ilegal^[_Breakin' the law... breakin' the law..._].

Jadi bungkus makro multibaris itu dengan `do`-`while(0)`.

[i[`do`-`while` statement-->in multiline macros]>]
[i[Preprocessor-->multiline macros]>]

## Contoh: Makro Assert {#my-assert}

Menambahkan assert ke kodemu adalah cara bagus untuk menangkap kondisi
yang menurutmu seharusnya tidak terjadi. C menyediakan fungsionalitas
`assert()`. Ia memeriksa sebuah kondisi, dan kalau false, program
berhenti sambil memberi tahu file dan nomor baris tempat assertion gagal.

Tapi ini kurang memadai.

1. Pertama, kamu tidak bisa menambahkan pesan tambahan pada assert.
2. Kedua, tidak ada saklar on-off yang mudah untuk semua assert.

Yang pertama bisa kita atasi dengan makro.

Pada dasarnya, kalau aku punya kode ini:

``` {.c}
ASSERT(x < 20, "x must be under 20");
```

aku ingin sesuatu seperti ini terjadi (dengan asumsi `ASSERT()` ada di
baris 220 `foo.c`):

``` {.c}
if (!(x < 20)) {
    fprintf(stderr, "foo.c:220: assertion x < 20 failed: ");
    fprintf(stderr, "x must be under 20\n");
    exit(1);
}
```

Kita bisa mengambil nama file dari makro `__FILE__`, dan nomor baris dari
`__LINE__`. Pesannya sudah berupa string, tapi `x < 20` bukan, jadi kita
harus men-stringify-nya dengan `#`. Kita bisa membuat makro multibaris
dengan escape backslash di akhir baris.

``` {.c}
#define ASSERT(c, m) \
do { \
    if (!(c)) { \
        fprintf(stderr, __FILE__ ":%d: assertion %s failed: %s\n", \
                        __LINE__, #c, m); \
        exit(1); \
    } \
} while(0)
```

(Kelihatannya agak aneh dengan `__FILE__` di depan seperti itu, tapi ingat
ia adalah string literal, dan string literal yang bersebelahan otomatis
disambung. Sedangkan `__LINE__`, ia hanyalah `int`.)

Dan itu berfungsi! Kalau kujalankan ini:

``` {.c}
int x = 30;

ASSERT(x < 20, "x must be under 20");
```

aku mendapat output ini:

```
foo.c:23: assertion x < 20 failed: x must be under 20
```

Bagus sekali!

Tinggal satu hal lagi, yaitu cara menyalakan dan mematikannya, dan itu
bisa dilakukan dengan kompilasi kondisional.

Berikut contoh lengkapnya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

#define ASSERT_ENABLED 1

#if ASSERT_ENABLED
#define ASSERT(c, m) \
do { \
    if (!(c)) { \
        fprintf(stderr, __FILE__ ":%d: assertion %s failed: %s\n", \
                        __LINE__, #c, m); \
        exit(1); \
    } \
} while(0)
#else
#define ASSERT(c, m)  // Makro kosong kalau tidak diaktifkan
#endif

int main(void)
{
    int x = 30;

    ASSERT(x < 20, "x must be under 20");
}
```

Ini menghasilkan output:

``` {.default}
foo.c:23: assertion x < 20 failed: x must be under 20
```

## Directive `#error`

[i[`#error` directive]<]

Directive ini membuat compiler langsung error begitu menemukannya.

Biasanya, ini dipakai di dalam kondisional untuk mencegah kompilasi kecuali
prasyarat tertentu terpenuhi:

``` {.c}
#ifndef __STDC_IEC_559__
    #error I really need IEEE-754 floating point to compile. Sorry!
#endif
```

[i[`#error` directive]>]
[i[`#warning` directive]<]

Beberapa compiler punya directive `#warning` pelengkap yang non-standar,
yang mengeluarkan peringatan tapi tidak menghentikan kompilasi, namun ini
tidak ada di spesifikasi C11.

[i[`#warning` directive]>]

## Directive `#embed`

[i[`#embed` directive]<]

<!-- Godbolt demo: https://godbolt.org/z/Kb3ejE7q5 -->

Baru di C23!

Dan saat ini belum berfungsi di compiler mana pun milikku, jadi
jangan telan bagian ini bulat-bulat!

Intinya, kamu bisa menyertakan byte-byte dari sebuah file sebagai konstanta
integer seolah-olah kamu mengetiknya sendiri.

Misalnya, kalau kamu punya file biner bernama `foo.bin` yang berisi empat
byte bernilai desimal 11, 22, 33, dan 44, lalu kamu menulis ini:

``` {.c}
int a[] = {
#embed "foo.bin"
};
```

Hasilnya persis seperti kamu mengetik ini:

``` {.c}
int a[] = {11,22,33,44};
```

Ini cara yang sangat kuat untuk menginisialisasi array dengan data biner
tanpa perlu mengonversinya menjadi kode dulu---preprocessor yang
mengerjakannya untukmu!

Kasus pemakaian yang lebih khas mungkin file berisi gambar kecil yang
akan ditampilkan dan tidak ingin kamu muat saat runtime.

Berikut contoh lain:

``` {.c}
int a[] = {
#embed <foo.bin>
};
```

Kalau kamu memakai kurung siku, preprocessor mencari di serangkaian tempat
yang ditentukan implementasi untuk menemukan file, persis seperti
`#include`. Kalau kamu memakai tanda kutip ganda dan resource tidak
ditemukan, compiler akan mencobanya seolah-olah kamu memakai kurung siku
sebagai upaya putus asa terakhir untuk menemukan file itu.

`#embed` bekerja seperti `#include` dalam arti ia secara efektif
menempelkan nilai sebelum compiler melihatnya. Artinya kamu bisa
memakainya di berbagai tempat:

```
return
#embed "somevalue.dat"
;
```

atau

```
int x =
#embed "xvalue.dat"
;
```

Nah---apakah ini selalu byte? Maksudnya bernilai 0 sampai 255, inklusif?
Jawabannya, secara bawaan jelas "ya", kecuali saat jawabannya "tidak".

Secara teknis, elemennya selebar `CHAR_BIT` bit. Dan kemungkinan besar
itu 8 di sistemmu, jadi kamu mendapat rentang 0-255 pada nilainya.
(Nilainya selalu non-negatif.)

Mungkin juga implementasi membolehkan ini diubah dengan cara tertentu,
misalnya lewat command line atau parameter.

Ukuran file dalam bit harus kelipatan ukuran elemen. Artinya, kalau setiap
elemen 8 bit, ukuran file (dalam bit) harus kelipatan 8. Dalam pemakaian
sehari-hari, ini cara membingungkan untuk mengatakan bahwa setiap file
harus berupa jumlah byte yang bulat... yang tentu saja memang begitu.
Jujur saja, aku sendiri tidak yakin kenapa aku repot-repot menulis
paragraf ini. Baca spesifikasi kalau kamu benar-benar penasaran.

### Parameter `#embed`

Ada berbagai parameter yang bisa disebutkan pada directive `#embed`.
Berikut contoh dengan parameter `limit()` yang belum diperkenalkan:

``` {.c}
int a[] = {
#embed "/dev/random" limit(5)
};
```

Tapi bagaimana kalau `limit` sudah didefinisikan di tempat lain? Untungnya
kamu bisa menaruh `__` di sekeliling kata kuncinya dan ia bekerja dengan
cara yang sama:

``` {.c}
int a[] = {
#embed "/dev/random" __limit__(5)
};
```

Nah... apa itu `limit`?

### Parameter `limit()`

Kamu bisa menentukan batas jumlah elemen yang disematkan dengan parameter
ini.

Ini nilai maksimum, bukan nilai mutlak. Kalau file yang disematkan lebih
pendek dari batas yang ditentukan, hanya sebanyak itu byte yang diimpor.

Contoh `/dev/random` di atas adalah contoh motivasi untuk ini---di Unix,
itu _character device file_ yang mengembalikan aliran tak terbatas bilangan
yang cukup acak.

Menyematkan byte dalam jumlah tak terbatas memberatkan RAM, jadi parameter
`limit` memberi cara untuk berhenti setelah jumlah tertentu.

Terakhir, kamu boleh memakai makro `#define` di dalam `limit`, kalau kamu
penasaran.

### Parameter `if_empty`

[i[`if_empty()` embed parameter]<]

Parameter ini menentukan apa hasil embed kalau file ada tapi tidak berisi
data. Misalkan file `foo.dat` berisi satu byte bernilai 123. Kalau kita
menulis ini:

``` {.c}
int x = 
#embed "foo.dat" if_empty(999)
;
```

kita mendapat:

``` {.c}
int x = 123;   // Ketika foo.dat berisi byte 123
```

Tapi bagaimana kalau file `foo.dat` berukuran nol byte (artinya tidak
berisi data dan kosong)? Kalau begitu, hasilnya diperluas menjadi:

``` {.c}
int x = 999;   // Ketika foo.dat kosong
```

Perlu dicatat, kalau `limit` diatur ke `0`, maka `if_empty` akan
selalu dipakai. Artinya, batas nol secara efektif berarti file itu kosong.

Ini akan selalu menghasilkan `x = 999` apa pun isi `foo.dat`:

``` {.c}
int x = 
#embed "foo.dat" limit(0) if_empty(999)
;
```

[i[`if_empty()` embed parameter]>]

### Parameter `prefix()` dan `suffix()`

[i[`prefix()` embed parameter]<]
[i[`suffix()` embed parameter]<]

Ini cara untuk menambahkan sesuatu di depan embed.

Perhatikan bahwa ini hanya berpengaruh pada data yang tidak kosong! Kalau
file kosong, baik `prefix` maupun `suffix` tidak berpengaruh.

Berikut contoh ketika kita menyematkan tiga bilangan acak, tapi
mengawali angka-angka itu dengan `11,` dan mengakhirinya dengan `,99`:

``` {.c}
int x[] = {
#embed "/dev/urandom" limit(3) prefix(11,) suffix(,99)
};
```

Contoh hasil:

``` {.c}
int x[] = {11,135,116,220,99};
```

Tidak ada kewajiban memakai `prefix` dan `suffix` sekaligus. Kamu bisa
memakai keduanya, salah satu, atau tidak sama sekali.

Kita bisa memanfaatkan sifat bahwa keduanya hanya berlaku pada file yang
tidak kosong untuk efek yang rapi, seperti pada contoh berikut yang
dicuri tanpa malu dari spesifikasi.

Katakanlah kita punya file `foo.dat` yang berisi sejumlah data. Dan kita
ingin memakainya untuk menginisialisasi array, lalu kita ingin sufiks pada
array berupa elemen nol.

Tidak masalah, kan?

``` {.c}
int x[] = {
#embed "foo.dat" suffix(,0)
};
```

Kalau `foo.dat` berisi 11, 22, dan 33, kita mendapat:

``` {.c}
int x[] = {11,22,33,0};
```

Tapi tunggu! Bagaimana kalau `foo.dat` kosong? Kita mendapat:

``` {.c}
int x[] = {};
```

dan itu tidak bagus.

Tapi kita bisa memperbaikinya seperti ini:

``` {.c}
int x[] = {
#embed "foo.dat" suffix(,)
    0
};
```

Karena parameter `suffix` dihilangkan kalau file kosong, ini hanya akan
menjadi:

``` {.c}
int x[] = {0};
```

yang tidak masalah.

[i[`prefix()` embed parameter]>]
[i[`suffix()` embed parameter]>]

### Identifier `__has_embed()`

[i[`__has_embed()` identifier]<]

Ini cara yang bagus untuk menguji apakah file tertentu tersedia untuk
disematkan, dan juga apakah file itu kosong atau tidak.

Kamu memakainya dengan directive `#if`.

Berikut potongan kode yang mengambil 5 bilangan acak dari character device
generator bilangan acak. Kalau itu tidak ada, ia mencoba mengambilnya dari
file `myrandoms.dat`. Kalau itu juga tidak ada, ia memakai beberapa nilai
yang ditulis langsung:

``` {.c}
    int random_nums[] = {
#if __has_embed("/dev/urandom")
    #embed "/dev/urandom" limit(5)
#elif __has_embed("myrandoms.dat")
    #embed "myrandoms.dat" limit(5)
#else
    140,178,92,167,120
#endif
    };
```

Secara teknis, identifier `__has_embed()` bernilai salah satu dari tiga
nilai:

|Hasil `__has_embed()`|Deskripsi|
|-|-|
|`__STDC_EMBED_NOT_FOUND__`|Jika file tidak ditemukan.|
|`__STDC_EMBED_FOUND__`|Jika file ditemukan dan tidak kosong.|
|`__STDC_EMBED_EMPTY__`|Jika file ditemukan dan kosong.|

Aku punya alasan kuat untuk percaya bahwa `__STDC_EMBED_NOT_FOUND__`
adalah `0` dan yang lain bukan nol (karena itu tersirat dalam proposal dan
masuk akal secara logika), tapi aku kesulitan menemukannya di versi draf
spesifikasi ini.

[i[`__has_embed()` identifier]>]

TODO

### Parameter Lainnya

Implementasi compiler boleh mendefinisikan parameter embed lain sesukanya
---cari parameter non-standar ini di dokumentasi compiler-mu.

Misalnya:

``` {.c}
#embed "foo.bin" limit(12) frotz(lamp)
```

Parameter ini biasanya diberi awalan untuk membantu namespacing:

``` {.c}
#embed "foo.bin" limit(12) fmc::frotz(lamp)
```

Mungkin masuk akal mencoba mendeteksi apakah parameter ini tersedia
sebelum dipakai, dan untungnya kita bisa memakai `__has_embed` untuk itu.

Biasanya, `__has_embed()` hanya memberi tahu apakah file ada atau tidak.
Tapi---dan inilah bagian serunya---ia juga mengembalikan false kalau
parameter tambahan tidak didukung!

Jadi kalau kita memberinya file yang _kita tahu_ ada serta parameter yang
ingin kita uji keberadaannya, ia secara efektif memberi tahu apakah
parameter itu didukung.

Tapi file apa yang _selalu_ ada? Ternyata kita bisa memakai makro
`__FILE__`, yang diperluas menjadi nama file sumber yang merujuknya! File
itu _pasti_ ada, atau ada yang sangat salah di departemen ayam-dan-telur.

Mari kita uji parameter `frotz` itu untuk melihat apakah bisa dipakai:

``` {.c}
#if __has_embed(__FILE__ fmc::frotz(lamp))
    puts("fmc::frotz(lamp) is supported!");
#else
    puts("fmc::frotz(lamp) is NOT supported!");
#endif
```

### Menyematkan Nilai Multi-Byte

Bagaimana kalau yang dimasukkan `int`, bukan byte individual? Bagaimana
dengan nilai multi-byte di file yang disematkan?

Ini tidak didukung standar C23, tapi mungkin nanti ada ekstensi
implementasi yang mendefinisikannya.

[i[`#embed` directive]>]

## Directive `#pragma` {#pragma}

[i[`#pragma` directive]<]

Ini directive yang aneh, kependekan dari "pragmatic". Kamu bisa memakainya
untuk... ya, apa pun yang didukung compiler-mu.

Pada dasarnya satu-satunya saat kamu menambahkan ini ke kodemu adalah kalau
ada dokumentasi yang menyuruhmu melakukannya.

### Pragma Non-Standar

[i[`#pragma` directive-->nonstandard pragmas]<]

Berikut satu contoh non-standar memakai `#pragma` untuk membuat compiler
menjalankan loop `for` secara paralel dengan beberapa thread (kalau
compiler mendukung ekstensi [fl[OpenMP|https://www.openmp.org/]]):

``` {.c}
#pragma omp parallel for
for (int i = 0; i < 10; i++) { ... }
```

Ada berbagai macam directive `#pragma` yang terdokumentasi di seluruh
penjuru dunia.

Semua `#pragma` yang tidak dikenali diabaikan oleh compiler.

[i[`#pragma` directive-->nonstandard pragmas]>]

### Pragma Standar

Ada juga beberapa yang standar, dan ini diawali `STDC`, dengan bentuk
yang sama:

``` {.c}
#pragma STDC pragma_name on-off
```

Bagian `on-off` bisa berupa `ON`, `OFF`, atau `DEFAULT`.

Dan `pragma_name` bisa salah satu dari ini:

[i[`FP_CONTRACT` pragma]<]
[i[`CX_LIMITED_RANGE` pragma]<]

[i[`FENV_ACCESS` pragma]]

|Nama Pragma|Deskripsi|
|-------------|--------------------------------------------------|
|`FP_CONTRACT`|Mengizinkan ekspresi floating point diringkas menjadi satu operasi untuk menghindari galat pembulatan yang mungkin terjadi dari beberapa operasi.|
|`FENV_ACCESS`|Atur ke `ON` jika kamu berencana mengakses flag status floating point. Jika `OFF`, compiler mungkin melakukan optimasi yang membuat nilai di flag tidak konsisten atau tidak valid.|
|`CX_LIMITED_RANGE`|Atur ke `ON` untuk mengizinkan compiler melewatkan pengecekan overflow saat melakukan aritmetika kompleks. Bawaannya `OFF`.|

Misalnya:

``` {.c}
#pragma STDC FP_CONTRACT OFF
#pragma STDC CX_LIMITED_RANGE ON
```

[i[`FP_CONTRACT` pragma]>]

Soal `CX_LIMITED_RANGE`, spesifikasi menunjukkan:

> The purpose of the pragma is to allow the implementation to use the
> formulas:
>
> $(x+iy)\times(u+iv) = (xu-yv)+i(yu+xv)$
>
> $(x+iy)/(u+iv) = [(xu+yv)+i(yu-xv)]/(u^2+v^2)$
>
> $|x+iy|=\sqrt{x^2+y^2}$
>
> where the programmer can determine they are safe.

[i[`CX_LIMITED_RANGE` pragma]>]

### Operator `_Pragma`

[i[`_Pragma` operator]<]

Ini cara lain untuk mendeklarasikan pragma yang bisa dipakai di makro.

Keduanya setara:

``` {.c}
#pragma "Unnecessary" quotes
_Pragma("\"Unnecessary\" quotes")
```

[i[`_Pragma` operator-->in a macro]<]

Ini bisa dipakai di makro, kalau perlu:

``` {.c}
#define PRAGMA(x) _Pragma(#x)
```

[i[`_Pragma` operator-->in a macro]>]
[i[`_Pragma` operator]>]
[i[`#pragma` directive]>]

## Directive `#line`

[i[`#line` directive]<]
[i[`__LINE__` macro]<]

Ini memungkinkan kamu menimpa nilai `__LINE__` dan `__FILE__`. Kalau mau.

Aku belum pernah ingin melakukannya, tapi di K&R2 mereka menulis:

> For the benefit of other preprocessors that generate C programs [...]

Jadi mungkin itulah alasannya.

Untuk menimpa nomor baris menjadi, misalnya, 300:

``` {.c}
#line 300
```

dan `__LINE__` akan terus menghitung naik dari sana.

[i[`__LINE__` macro]>]

Untuk menimpa nomor baris dan nama file:

``` {.c}
#line 300 "newfilename"
```

[i[`#line` directive]>]

## Null Directive

[i[`#` null directive]<]

Tanda `#` sendirian di satu baris diabaikan oleh preprocessor. Nah, jujur
saja, aku tidak tahu apa kasus pemakaiannya.

Aku pernah melihat contoh seperti ini:

``` {.c}
#ifdef FOO
    #
#else
    printf("Something");
#endif
```

yang hanya kosmetik; baris dengan `#` sendirian itu bisa dihapus tanpa
efek buruk.

Atau mungkin demi konsistensi kosmetik, seperti ini:

``` {.c}
#
#ifdef FOO
    x = 2;
#endif
#
#if BAR == 17
    x = 12;
#endif
#
```

Tapi, soal kosmetik, itu jelek.

Satu postingan lain menyebut penghapusan komentar---bahwa di GCC, komentar
setelah `#` tidak akan dilihat compiler. Itu tidak kuragukan,
tapi spesifikasi tampaknya tidak menyebut ini sebagai perilaku standar.

Pencarianku akan alasannya tidak membuahkan banyak hasil. Jadi aku akan bilang
saja bahwa ini adalah esoterika C kuno yang sesungguhnya.

[i[`#` null directive]>]
[i[Preprocessor]>]
