<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Alokasi Memori Manual

[i[Manual memory management]<]
Inilah salah satu area besar tempat C kemungkinan berbeda dari bahasa yang
sudah kamu kenal: _manajemen memori manual_.

Bahasa lain memakai reference counting, garbage collection, atau cara
lain untuk menentukan kapan harus mengalokasikan memori baru untuk suatu
data---dan kapan harus mendealokasikannya ketika tidak ada lagi variabel
yang merujuknya.

Dan itu menyenangkan. Enak rasanya tidak perlu memikirkannya, cukup
melepas semua referensi ke suatu item dan percaya bahwa pada suatu saat
memori yang terkait dengannya akan dibebaskan.

Tapi C tidak sepenuhnya begitu.

[i[Automatic variables]<]
Tentu saja, di C ada variabel yang dialokasikan dan didealokasikan
otomatis ketika masuk dan keluar scope. Kita menyebutnya variabel
otomatis. Mereka adalah variabel "lokal" biasa dengan scope blok. Tidak
ada masalah.
[i[Automatic variables]>]

Tapi bagaimana kalau kamu ingin sesuatu bertahan lebih lama dari sebuah
blok tertentu? Di sinilah manajemen memori manual berperan.

Kamu bisa menyuruh C secara eksplisit untuk mengalokasikan sejumlah byte
yang bisa kamu pakai sesukamu. Dan byte-byte itu akan tetap teralokasi
sampai kamu membebaskan memori itu secara eksplisit^[Atau sampai program
berakhir, dan saat itu semua memori yang dialokasikannya dibebaskan. Tanda
bintang: beberapa sistem mengizinkan kamu mengalokasikan memori yang
bertahan setelah program berakhir, tapi itu bergantung pada sistem, di
luar cakupan panduan ini, dan kamu pasti tidak akan melakukannya secara
tidak sengaja.].

Penting untuk membebaskan memori yang sudah selesai kamu pakai! Kalau
tidak, itu disebut _memory leak_ dan prosesmu akan terus menahan memori
itu sampai berakhir.

_Kalau kamu mengalokasikannya secara manual, kamu harus membebaskannya
secara manual saat selesai memakainya._

Jadi bagaimana caranya? Kita akan belajar beberapa fungsi baru, dan
memakai operator `sizeof` untuk membantu mengetahui berapa byte yang harus
dialokasikan.

[i[The stack]<]
[i[The heap]<]
Dalam istilah umum di dunia C, para developer bilang variabel lokal
otomatis dialokasikan "di stack", dan memori yang dialokasikan manual ada
"di heap". Spesifikasi tidak membicarakan keduanya, tapi semua developer
C akan paham yang kamu maksud kalau kamu menyebutnya.
[i[The heap]>]
[i[The stack]>]

Semua fungsi yang kita pelajari di bab ini ada di `<stdlib.h>`.

## Mengalokasikan dan Mendealokasikan, `malloc()` dan `free()`

[i[`malloc()` function]<]
Fungsi `malloc()` menerima jumlah byte yang akan dialokasikan, dan
mengembalikan void pointer ke blok memori yang baru dialokasikan itu.

Karena berupa `void*`, hasilnya bisa ditugaskan ke tipe pointer apa pun
yang kamu mau... biasanya tipe ini berkaitan dengan jumlah byte yang
kamu alokasikan.

[i[`sizeof` operator-->with `malloc()`]<]
Jadi... berapa byte yang harus dialokasikan? `sizeof` bisa membantu.
Kalau kita ingin mengalokasikan ruang untuk satu `int`, kita bisa memakai
`sizeof(int)` dan mengopernya ke `malloc()`.
[i[`sizeof` operator-->with `malloc()`]>]

[i[`free()` function]<]
Setelah selesai memakai memori yang dialokasikan, kita bisa memanggil
`free()` untuk menandakan bahwa kita sudah selesai dan memori itu bisa
dipakai untuk hal lain. Sebagai argumen, oper pointer yang sama dengan
yang kamu dapat dari `malloc()` (atau salinannya). Memakai wilayah memori
setelah di-`free()` adalah undefined behavior.

Mari kita coba. Kita alokasikan memori yang cukup untuk satu `int`, lalu
simpan sesuatu di sana, lalu cetak.

``` {.c}
// Alokasikan ruang untuk satu int (sebesar sizeof(int) byte):

int *p = malloc(sizeof(int));

*p = 12;  // Simpan sesuatu di sana

printf("%d\n", *p);  // Cetak: 12

free(p);  // Selesai memakai memori itu

//*p = 3490;  // ERROR: undefined behavior! Dipakai setelah free()!
```
[i[`free()` function]>]

Nah, pada contoh yang dibuat-buat itu, sebenarnya tidak ada untungnya.
Kita bisa saja memakai `int` otomatis dan hasilnya sama. Tapi kita akan
lihat bahwa kemampuan mengalokasikan memori seperti ini punya
keunggulan, terutama untuk struktur data yang lebih rumit.

[i[`sizeof` operator-->with `malloc()`]<]
Satu hal lagi yang sering kamu lihat memanfaatkan fakta bahwa `sizeof`
bisa memberi ukuran tipe hasil dari ekspresi konstan apa pun. Jadi kamu
juga bisa menaruh nama variabel di sana dan memakainya. Berikut contohnya,
sama seperti sebelumnya:

``` {.c}
int *p = malloc(sizeof *p);  // *p adalah int, jadi sama dengan sizeof(int)
```
[i[`sizeof` operator-->with `malloc()`]>]
[i[`malloc()` function]>]

## Pengecekan Error

[i[`malloc()` function-->error checking]<]
Semua fungsi alokasi mengembalikan pointer ke bentangan memori yang baru
dialokasikan, atau `NULL` kalau memori tidak bisa dialokasikan karena
suatu alasan.

Beberapa OS seperti Linux bisa dikonfigurasi sedemikian rupa sehingga
`malloc()` tidak pernah mengembalikan `NULL`, bahkan ketika memori habis.
Tapi meskipun begitu, kamu tetap harus menulis kodenya dengan
perlindungan.

``` {.c}
int *x;

x = malloc(sizeof(int) * 10);

if (x == NULL) {
    printf("Error allocating 10 ints\n");
    // lakukan sesuatu di sini untuk menanganinya
}
```

Berikut pola umum yang akan sering kamu lihat, ketika penugasan dan
kondisi dilakukan di baris yang sama:

``` {.c}
int *x;

if ((x = malloc(sizeof(int) * 10)) == NULL) {
    printf("Error allocating 10 ints\n");
    // lakukan sesuatu di sini untuk menanganinya
}
```
[i[`malloc()` function-->error checking]>]

## Mengalokasikan Ruang untuk Array

[i[`malloc()` function-->and arrays]<]
Kita sudah melihat cara mengalokasikan ruang untuk satu benda; bagaimana
kalau untuk banyak benda dalam sebuah array?

Di C, array adalah sekumpulan benda yang sama, berurutan dalam satu
bentangan memori yang kontigu.

Kita bisa mengalokasikan bentangan memori yang kontigu---kita sudah tahu
caranya. Kalau kita ingin 3490 byte memori, tinggal minta saja:

``` {.c}
char *p = malloc(3490);  // Voila
```

Dan---memang!---itu array berisi 3490 `char` (alias string!) karena
setiap `char` berukuran 1 byte. Dengan kata lain, `sizeof(char)` adalah
`1`.

Catatan: tidak ada inisialisasi pada memori yang baru dialokasikan---isinya
penuh sampah. Bersihkan dengan `memset()` kalau perlu, atau lihat
`calloc()` di bawah.

Tapi kita bisa mengalikan ukuran benda yang diinginkan dengan jumlah
elemen yang diinginkan, lalu mengaksesnya dengan notasi pointer maupun
array. Contoh!

[i[`sizeof` operator-->with `malloc()`]<]
``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // Alokasikan ruang untuk 10 int
    int *p = malloc(sizeof(int) * 10);

    // Isi dengan nilai 0-45:
    for (int i = 0; i < 10; i++)
        p[i] = i * 5;

    // Cetak semua nilai 0, 5, 10, 15, ..., 40, 45
    for (int i = 0; i < 10; i++)
        printf("%d\n", p[i]);

    // Bebaskan ruangnya
    free(p);
}
```

Kuncinya ada di baris `malloc()` itu. Kalau kita tahu setiap `int`
membutuhkan `sizeof(int)` byte, dan kita ingin 10 buah, kita tinggal
mengalokasikan persis sebanyak itu byte dengan:

``` {.c}
sizeof(int) * 10
```

Dan trik ini berlaku untuk semua tipe. Cukup oper ke `sizeof` dan kalikan
dengan ukuran array.
[i[`sizeof` operator-->with `malloc()`]>]
[i[`malloc()` function-->and arrays]>]

## Alternatif: `calloc()`

[i[`calloc()` function]<]
Ini fungsi alokasi lain yang bekerja mirip `malloc()`, dengan dua
perbedaan utama:

* Alih-alih satu argumen, kamu mengoper ukuran satu elemen dan jumlah
  elemen yang ingin dialokasikan. Seolah-olah ia dibuat khusus untuk
  mengalokasikan array.
* Ia membersihkan memori menjadi nol.

Kamu tetap memakai `free()` untuk mendealokasikan memori yang didapat
lewat `calloc()`.

Berikut perbandingan `calloc()` dan `malloc()`.

``` {.c}
// Alokasikan ruang untuk 10 int dengan calloc(), diinisialisasi ke 0:
int *p = calloc(10, sizeof(int));

// Alokasikan ruang untuk 10 int dengan malloc(), diinisialisasi ke 0:
int *q = malloc(10 * sizeof(int));
memset(q, 0, 10 * sizeof(int));   // isi dengan 0
```

Sekali lagi, hasilnya sama untuk keduanya, kecuali `malloc()` tidak
mengosongkan memori secara bawaan.
[i[`calloc()` function]>]

## Mengubah Ukuran Alokasi dengan `realloc()`

[i[`realloc()` function]<]
Kalau kamu sudah mengalokasikan 10 `int`, tapi kemudian memutuskan butuh
20, apa yang bisa dilakukan?

Satu pilihan adalah mengalokasikan ruang baru, lalu `memcpy()` isi
memorinya... tapi ternyata kadang tidak ada yang perlu dipindahkan. Dan
ada satu fungsi yang cukup pintar untuk melakukan hal yang benar di semua
keadaan yang tepat: `realloc()`.

Ia menerima pointer ke memori yang sebelumnya dialokasikan (oleh
`malloc()` atau `calloc()`) dan ukuran baru untuk wilayah memori itu.

Ia lalu membesarkan atau mengecilkan memori itu, dan mengembalikan pointer
ke sana. Kadang ia mengembalikan pointer yang sama (kalau data tidak perlu
disalin ke tempat lain), atau pointer yang berbeda (kalau data memang
harus disalin).

Pastikan saat memanggil `realloc()`, kamu menentukan jumlah _byte_ yang
dialokasikan, bukan sekadar jumlah elemen array! Yaitu:

``` {.c}
num_floats *= 2;

np = realloc(p, num_floats);  // SALAH: butuh byte, bukan jumlah elemen!

np = realloc(p, num_floats * sizeof(float));  // Lebih baik!
```

Mari kita alokasikan array berisi 20 `float`, lalu berubah pikiran dan
menjadikannya array berisi 40.

Kita akan menugaskan nilai kembalian realloc() ke pointer lain supaya
bisa memeriksa apakah ia NULL. Kalau bukan, baru kita tugaskan ulang ke
pointer asli. (Kalau nilai kembalian langsung ditugaskan ke pointer asli,
kita akan kehilangan pointer itu ketika fungsi mengembalikan `NULL`, dan
tidak ada cara untuk mendapatkannya lagi.)

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // Alokasikan ruang untuk 20 float
    float *p = malloc(sizeof *p * 20);  // sizeof *p sama dengan sizeof(float)

    // Isi dengan nilai pecahan 0.0-1.0:
    for (int i = 0; i < 20; i++)
        p[i] = i / 20.0;

    // Tapi tunggu! Ternyata kita ingin array ini berisi 40 elemen
    float *new_p = realloc(p, sizeof *p * 40);

    // Periksa apakah realokasi berhasil
    if (new_p == NULL) {
        printf("Error reallocing\n");
        return 1;
    }

    // Kalau berhasil, kita tinggal menugaskan ulang p
    p = new_p;

    // Lalu isi elemen-elemen baru dengan nilai dalam rentang 1.0-2.0
    for (int i = 20; i < 40; i++)
        p[i] = 1.0 + (i - 20) / 20.0;

    // Cetak semua nilai 0.0-2.0 dari 40 elemen:
    for (int i = 0; i < 40; i++)
        printf("%f\n", p[i]);

    // Bebaskan ruangnya
    free(p);
}
```

Perhatikan bagaimana kita mengambil nilai kembalian `realloc()` dan
menugaskannya ulang ke variabel pointer `p` yang sama dengan yang kita
oper. Itu hal yang cukup umum dilakukan.

Kalau baris 7 tampak aneh dengan `sizeof *p` di dalamnya, ingat bahwa
`sizeof` bekerja pada ukuran tipe dari ekspresi. Dan tipe `*p` adalah
`float`, jadi baris itu setara dengan `sizeof(float)`.

Terakhir, mungkin terasa janggal bahwa tidak ada `free(new_p)` di mana
pun, padahal itu pointer yang dikembalikan `realloc()`. Alasannya,
`new_p` disalin ke `p` di baris 23, jadi keduanya bernilai sama; artinya
keduanya menunjuk ke gumpalan memori yang sama, dan gumpalannya hanya
satu. Jadi saat `free()`, kita bisa membebaskan yang mana saja dengan
efek yang sama.

[i[`realloc()` function]>]


### Membaca Baris dengan Panjang Sembarang

Dengan contoh lengkap ini, penulis ingin menunjukkan dua hal.

1. Pemakaian `realloc()` untuk membesarkan buffer saat kita membaca lebih
   banyak data.
2. Pemakaian `realloc()` untuk mengecilkan buffer ke ukuran yang pas
   setelah pembacaan selesai.

Di sini ada loop yang memanggil `fgetc()` berulang kali untuk menambah
isi buffer, sampai karakter terakhirnya newline.

Begitu newline ditemukan, buffer dikecilkan ke ukuran yang tepat lalu
dikembalikan.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

// Baca satu baris berukuran sembarang dari file
//
// Mengembalikan pointer ke baris tersebut.
// Mengembalikan NULL saat EOF atau error.
//
// Caller bertanggung jawab men-free() pointer ini setelah selesai.
//
// Perhatikan bahwa newline dibuang dari hasilnya. Kalau newline
// diperlukan, sebaiknya ubah ini menjadi do-while.

char *readline(FILE *fp)
{
    int offset = 0;   // Indeks tempat karakter berikutnya masuk ke buffer
    int bufsize = 4;  // Sebaiknya ukuran awal berupa pangkat 2
    char *buf;        // Buffer-nya
    int c;            // Karakter yang baru dibaca

    buf = malloc(bufsize);  // Alokasikan buffer awal

    if (buf == NULL)   // Cek error
        return NULL;

    // Loop utama--baca sampai newline atau EOF
    while (c = fgetc(fp), c != '\n' && c != EOF) {

        // Periksa apakah ruang di buffer habis, dengan memperhitungkan
        // satu byte ekstra untuk terminator NUL
        if (offset == bufsize - 1) {  // -1 untuk terminator NUL
            bufsize *= 2;  // Gandakan ruangnya

            char *new_buf = realloc(buf, bufsize);

            if (new_buf == NULL) {
                free(buf);   // Saat error, free lalu keluar
                return NULL;
            }

            buf = new_buf;  // Realokasi berhasil
        }

        buf[offset++] = c;  // Tambahkan byte ke buffer
    }

    // Kita menemui newline atau EOF...

    // Kalau di EOF dan tidak ada byte yang terbaca, free buffer dan
    // kembalikan NULL untuk menandakan kita di EOF:
    if (c == EOF && offset == 0) {
        free(buf);
        return NULL;
    }

    // Kecilkan sesuai ukuran
    if (offset < bufsize - 1) {  // Kalau belum sampai ujung
        char *new_buf = realloc(buf, offset + 1); // +1 untuk terminator NUL

        // Kalau berhasil, arahkan buf ke new_buf;
        // kalau tidak, biarkan buf di tempatnya
        if (new_buf != NULL)
            buf = new_buf;
    }

    // Tambahkan terminator NUL
    buf[offset] = '\0';

    return buf;
}

int main(void)
{
    FILE *fp = fopen("foo.txt", "r");

    char *line;

    while ((line = readline(fp)) != NULL) {
        printf("%s\n", line);
        free(line);
    }

    fclose(fp);
}
```

Saat membesarkan memori seperti ini, lazimnya (meski bukan aturan baku)
ruang yang dibutuhkan digandakan di setiap langkah, supaya jumlah
`realloc()` yang terjadi sesedikit mungkin.

Terakhir, kamu mungkin memperhatikan bahwa `readline()` mengembalikan
pointer ke buffer hasil `malloc()`. Karena itu, caller bertanggung jawab
untuk men-`free()` memori itu secara eksplisit saat selesai memakainya.

### `realloc()` dengan `NULL`

[i[`realloc()` function-->with `NULL` argument]<]
Waktunya trivia! Dua baris ini setara:

``` {.c}
char *p = malloc(3490);
char *p = realloc(NULL, 3490);
```

Ini bisa praktis kalau kamu punya semacam loop alokasi dan tidak ingin
memperlakukan `malloc()` pertama secara khusus.

``` {.c}
int *p = NULL;
int length = 0;

while (!done) {
    // Alokasikan 10 int lagi:
    length += 10;
    p = realloc(p, sizeof *p * length);

    // Lakukan hal-hal luar biasa
    // ...
}
```

Pada contoh itu, kita tidak butuh `malloc()` awal karena `p` memang
`NULL` sejak awal.
[i[`realloc()` function-->with `NULL` argument]>]

## Alokasi dengan Alignment

[i[Memory alignment]<]
Kemungkinan besar kamu tidak akan memerlukan ini.

Dan penulis tidak ingin terlalu melenceng membahasnya sekarang, tapi ada
yang disebut _memory alignment_, yang berkaitan dengan alamat memori
(nilai pointer) yang harus merupakan kelipatan dari angka tertentu.

Misalnya, sebuah sistem mungkin mewajibkan nilai 16-bit dimulai di alamat
memori kelipatan 2. Atau nilai 64-bit dimulai di alamat memori kelipatan
2, 4, atau 8, misalnya. Itu tergantung CPU.

Sebagian sistem membutuhkan alignment semacam ini agar akses memori cepat,
bahkan ada yang membutuhkannya agar akses memori bisa dilakukan sama
sekali.

Nah, kalau kamu memakai `malloc()`, `calloc()`, atau `realloc()`, C akan
memberimu gumpalan memori yang ter-align dengan baik untuk nilai apa pun,
bahkan `struct`. Berlaku di semua kasus.

Tapi mungkin ada saatnya kamu tahu bahwa data tertentu bisa di-align pada
batas yang lebih kecil, atau harus di-align pada batas yang lebih besar
karena suatu alasan. Penulis membayangkan ini lebih umum pada pemrograman
embedded system.

[i[`aligned_alloc()` function]<]
Dalam kasus itu, kamu bisa menentukan alignment dengan `aligned_alloc()`.

Alignment adalah integer pangkat dua yang lebih besar dari nol, jadi `2`,
`4`, `8`, `16`, dan seterusnya, dan itu diberikan ke `aligned_alloc()`
sebelum jumlah byte yang kamu inginkan.

Pembatas lainnya: jumlah byte yang dialokasikan harus merupakan kelipatan
alignment. Tapi ini mungkin akan berubah. Lihat [fl[C Defect Report
460|http://www.open-std.org/jtc1/sc22/wg14/www/docs/summary.htm#dr_460]]

Mari kita coba contoh, mengalokasikan pada batas 64 byte:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    // Alokasikan 256 byte yang ter-align pada batas 64 byte
    char *p = aligned_alloc(64, 256);  // 256 == 64 * 4

    // Salin sebuah string ke sana dan cetak
    strcpy(p, "Hello, world!");
    printf("%s\n", p);

    // Bebaskan ruangnya
    free(p);
}
```

Ada catatan soal `realloc()` dan `aligned_alloc()`. `realloc()` tidak
punya jaminan alignment apa pun, jadi kalau kamu butuh ruang hasil
realokasi yang ter-align, kamu harus melakukannya dengan cara susah
memakai `memcpy()`.
[i[`aligned_alloc()` function]>]

Berikut fungsi `aligned_realloc()` non-standar, kalau kamu membutuhkannya:

``` {.c}
void *aligned_realloc(void *ptr, size_t old_size, size_t alignment, size_t size)
{
    char *new_ptr = aligned_alloc(alignment, size);

    if (new_ptr == NULL)
        return NULL;

    size_t copy_size = old_size < size? old_size: size;  // ambil yang terkecil

    if (ptr != NULL)
        memcpy(new_ptr, ptr, copy_size);

    free(ptr);

    return new_ptr;
}
```

Perhatikan bahwa fungsi ini _selalu_ menyalin data, yang memakan waktu,
sedangkan `realloc()` yang asli akan menghindarinya kalau bisa. Jadi ini
jauh dari efisien. Hindari kebutuhan untuk merealokasi data yang
alignment-nya kustom.
[i[Memory alignment]>]
[i[Manual memory management]>]
