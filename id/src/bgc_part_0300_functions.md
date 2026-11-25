<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsi {#functions}

> _"Sir, not in an environment such as this. That's why I've also been
> programmed for over thirty secondary functions that---"_
>
> ---C3PO[i[C3PO]], sebelum diinterupsi dengan kasar, melaporkan jumlah
> fungsi tambahan yang sekarang sudah tidak mengesankan, _Star Wars_ script

[i[Fungsi]<]
Seperti bahasa lain yang sudah kamu pakai, C punya konsep _fungsi_.

Fungsi bisa menerima berbagai _argumen_[i[argumen Fungsi]] dan
mengembalikan sebuah nilai. Satu hal penting: tipe argumen dan tipe nilai
kembalian harus dideklarasikan lebih dulu---begitulah cara C menyukainya!

Mari kita lihat sebuah fungsi. Fungsi ini menerima sebuah `int` sebagai
argumen, dan mengembalikan[i[`return` pernyataan]] sebuah `int`.

``` {.c .numberLines}
#include <stdio.h>

int plus_one(int n)  // "Definisi"-nya
{
    return n + 1;
}
 
```

`int` sebelum `plus_one` menunjukkan tipe kembalian.

`int n` menunjukkan bahwa fungsi ini menerima satu argumen `int`, yang
disimpan dalam _parameter_ `n`[i[parameter Fungsi]]. Parameter adalah
variabel lokal khusus yang menjadi tempat argumen disalin.

Poin ini perlu ditegaskan: argumen disalin ke dalam parameter. Banyak hal
di C jadi lebih mudah dipahami kalau kamu ingat bahwa parameter adalah
_salinan_ argumen, bukan argumen itu sendiri. Soal ini sebentar lagi kita
bahas.

Lanjut ke `main()`, di sana ada pemanggilan fungsi, dan nilai
kembaliannya kita tugaskan ke variabel lokal `j`:

``` {.c .numberLines startFrom="8"}
int main(void)
{
    int i = 10, j;
    
    j = plus_one(i);  // "Pemanggilan"-nya

    printf("i + 1 is %d\n", j);
}
```

> Sebelum lupa: perhatikan bahwa fungsi didefinisikan sebelum dipakai.
> Kalau tidak, compiler belum mengenalnya ketika mengompilasi `main()`
> dan akan memberi error pemanggilan fungsi yang tidak dikenal. Ada cara
> yang lebih tepat untuk menulis kode di atas, yaitu dengan _function
> prototype_, tapi itu dibahas nanti.

Perhatikan juga bahwa `main()`[i[`main()` fungsi]] adalah sebuah fungsi!

Fungsi ini mengembalikan `int`.

Lalu, apa itu `void`[i[`void` tipe]]? Kata kunci ini menandakan bahwa
fungsi tidak menerima argumen.

Kamu juga bisa mengembalikan `void` untuk menandakan bahwa fungsi tidak
mengembalikan nilai:

``` {.c .numberLines}
#include <stdio.h>

// Fungsi ini tidak menerima argumen dan tidak mengembalikan nilai:

void hello(void)
{
    printf("Hello, world!\n");
}

int main(void)
{
    hello();  // Mencetak "Hello, world!"
}
```

## Passing by Value {#passvalue}

[i[Pass by value]()]Tadi sudah disebutkan bahwa saat sebuah argumen
dioper ke fungsi, salinan argumen itu dibuat dan disimpan dalam parameter
yang sesuai.

Kalau argumennya berupa variabel, yang disalin ke parameter adalah nilai
variabel tersebut.

Lebih umum lagi, seluruh ekspresi argumen dievaluasi dulu untuk
mendapatkan nilainya. Nilai itulah yang disalin ke parameter.

Apa pun yang terjadi, nilai dalam parameter itu berdiri sendiri. Ia tidak
bergantung pada nilai atau variabel apa pun yang kamu pakai sebagai
argumen saat memanggil fungsi.

Mari kita lihat contohnya. Pelajari, dan coba tebak outputnya sebelum
dijalankan:

``` {.c .numberLines}
#include <stdio.h>

void increment(int a)
{
    a++;
}

int main(void)
{
    int i = 10;

    increment(i);

    printf("i == %d\n", i);  // Apa yang dicetak ini?
}
```

Sekilas, `i` bernilai `10`, lalu kita mengopernya ke fungsi `increment()`.
Di sana nilainya di-increment, jadi waktu dicetak pasti `11`, kan?

> _"Get used to disappointment."_
>
> ---Dread Pirate Roberts, _The Princess Bride_

Tapi hasilnya bukan `11`---yang tercetak `10`! Kenapa?

Semuanya berpangkal pada kenyataan bahwa ekspresi yang kamu oper ke fungsi
_disalin_ ke parameter yang sesuai. Parameter itu salinan, bukan yang
aslinya.

Jadi `i` bernilai `10` di dalam `main()`. Lalu kita mengopernya ke
`increment()`. Parameter yang sesuai di dalam fungsi itu bernama `a`.

Penyalinan pun terjadi, seolah-olah lewat penugasan. Kurang lebih,
`a = i`. Pada titik itu `a` bernilai `10`, dan di dalam `main()`, `i` juga
`10`.

Kemudian `a` di-increment menjadi `11`. Tapi `i` sama sekali tidak
tersentuh! Nilainya tetap `10`.

Akhirnya fungsi selesai. Semua variabel lokalnya dibuang (selamat tinggal,
`a`!) dan kita kembali ke `main()`, tempat `i` masih `10`.

Lalu `i` dicetak, hasilnya `10`, dan selesai.

Itulah sebabnya pada contoh `plus_one()` tadi, kita `return` nilai yang
sudah diubah secara lokal supaya bisa dilihat lagi di `main()`.

Kelihatannya cukup membatasi, ya? Sepertinya dari sebuah fungsi kamu hanya
bisa mendapat satu data kembali. Setidaknya itu yang ada di pikiranmu.
Tapi ada cara lain untuk mendapat data kembali; orang C menyebutnya
_passing by reference_, dan itu cerita untuk lain waktu.

Namun, sehebat apa pun namanya, itu tidak mengubah kenyataan bahwa
_SEMUA_ yang kamu oper ke fungsi _TANPA PENGECUALIAN_ disalin ke
parameter yang sesuai, dan fungsi bekerja pada salinan lokal itu, _APA
PUN YANG TERJADI_. Ingat itu, bahkan ketika nanti kita membahas passing
by reference yang konon hebat itu.
[i[Pass by value]>]

## Function Prototype {#prototypes}

[i[Function prototype]<]Kalau kamu ingat, beberapa bagian lalu (rasanya
sudah zaman es) disebutkan bahwa fungsi harus didefinisikan sebelum dipakai,
kalau tidak compiler belum mengenalnya dan akan meledak dengan error.

Itu tidak sepenuhnya benar. Kamu bisa memberi tahu compiler lebih dulu
bahwa kamu akan memakai fungsi bertipe tertentu dengan daftar parameter
tertentu. Dengan begitu, fungsinya bisa didefinisikan di mana saja (bahkan
di file lain), asalkan _function prototype_ sudah dideklarasikan sebelum
fungsinya dipanggil.

Untungnya, function prototype cukup mudah. Ia hanyalah salinan baris
pertama definisi fungsi, ditambah titik koma di akhir. Misalnya, kode
berikut memanggil fungsi yang didefinisikan belakangan, karena
prototype-nya sudah dideklarasikan lebih dulu:

``` {.c .numberLines}
#include <stdio.h>

int foo(void);  // Ini prototype-nya!

int main(void)
{
    int i;
    
    // Kita bisa memanggil foo() di sini sebelum definisinya karena
    // prototype-nya sudah dideklarasikan di atas!

    i = foo();
    
    printf("%d\n", i);  // 3490
}

int foo(void)  // Ini definisinya, persis seperti prototype-nya!
{
    return 3490;
}
```

Kalau fungsi tidak dideklarasikan sebelum dipakai (baik lewat prototype
maupun definisinya), yang terjadi disebut _implicit declaration_[i[Implicit
declaration]]. Ini dulu diizinkan di standar C pertama (C89), yang punya
aturan khusus untuknya, tapi sekarang tidak lagi diizinkan. Dan tidak ada
alasan sah untuk mengandalkannya di kode baru.

Mungkin kamu menyadari sesuatu pada kode contoh yang sudah kita pakai...
Kita memakai fungsi `printf()` yang setia itu tanpa mendefinisikannya atau
mendeklarasikan prototype-nya! Bagaimana kita bisa lolos dari
pelanggaran hukum ini? Sebenarnya tidak lolos. Prototype-nya ada, yaitu di
header file `stdio.h` yang kita `#include`, ingat? Jadi kita tetap sah,
Pak Polisi![i[Function prototype]>]

## Daftar Parameter Kosong

[i[Empty parameter lists]]
Di C23, daftar parameter kosong dalam definisi atau prototype fungsi
artinya persis sama dengan menulis `void`[i[`void` tipe]] di situ.

``` {.c}
void foo();
void foo(void); // Sama saja di C23
```

Meski begitu, kode C zaman sekarang masih cenderung memilih `void`[i[`void`
tipe]] yang eksplisit di daftar parameter, terutama karena sebelum C23,
perilaku daftar parameter kosong sangat berbeda. Dan bukan perbedaan yang
menyenangkan.

Kita bahas sekarang, tapi singkatnya: kalau kodenya mungkin suatu saat
dibangun dengan compiler pra-C23, **selalu** pakai `void`[i[`void`
tipe]] untuk menyatakan daftar parameter kosong di semua deklarasi
fungsi.

Ada dua konteks yang perlu dipertimbangkan.

* Menghilangkan semua parameter di tempat fungsi didefinisikan
* Menghilangkan semua parameter dalam sebuah prototype

Kita lihat dulu definisi fungsinya:

``` {.c}
void foo()  // Seharusnya ada `void` di sini
{
    printf("Hello, world!\n");
}
```

Spesifikasi memang menyebut bahwa ini berarti fungsi `foo()` tidak
menerima parameter, tapi compiler boleh saja tidak memperingatkanmu kalau
kamu keliru mengoper argumen. Dan itu undefined behavior. Untuk
menghindari semua itu, cukup tulis `void`[i[`void` tipe]] di sana.

Pada function prototype, ada perbedaan yang _signifikan_ antara memakai
`void`[i[`void` tipe-->in function prototypes]] dan tidak:

``` {.c}
void foo();
void foo(void);  // Tidak sama!
```

Kalau `void` dihilangkan dari prototype, compiler diberi tahu bahwa tidak
ada informasi tambahan tentang parameter fungsi. Ini secara efektif
mematikan semua pengecekan tipe parameter, sehingga `void foo();` pada
dasarnya bukan "prototype" sama sekali. Ia hanyalah forward declaration.

Jadi, pada prototype **pasti** pakai `void` kalau daftar parameternya
kosong.

Dan, ya, itu praktik yang baik dalam semua kasus. Mungkin suatu hari,
ketika semua compiler pra-C23 sudah tidak dipakai lagi, kita bisa
meninggalkannya.

[i[Fungsi]>]
