<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Variable-Length Array (VLA)

[i[Variable-length array]<]

C menyediakan cara untuk mendeklarasikan array yang ukurannya ditentukan
saat runtime. Dengan begitu kamu mendapat manfaat ukuran dinamis saat
runtime seperti pada `malloc()`, tapi tanpa perlu repot men-`free()`
memorinya sesudahnya.

Nah, banyak orang tidak menyukai VLA. Misalnya, VLA dilarang di kernel
Linux. Alasannya akan dibahas lebih jauh [nanti](#vla-general-issues).

[i[`__STDC_NO_VLA__` macro]<]

Ini fitur bahasa yang opsional. Makro `__STDC_NO_VLA__` bernilai `1` kalau
VLA _tidak_ ada. (VLA wajib di C99, lalu menjadi opsional di C11.)

``` {.c}
#if __STDC_NO_VLA__ == 1
   #error Sorry, need VLAs for this program!
#endif
```

[i[`__STDC_NO_VLA__` macro]>]

Tapi karena GCC maupun Clang tidak repot-repot mendefinisikan makro ini,
manfaatnya mungkin terbatas.

Kita mulai dulu dengan contoh, lalu kita cari setan di detailnya.

## Dasar-Dasarnya

Array biasa dideklarasikan dengan ukuran konstan, seperti ini:

``` {.c}
int v[10];
```

[i[Variable-length array-->defining]<]

Tapi dengan VLA, kita bisa memakai ukuran yang ditentukan saat runtime
untuk membuat array, seperti ini:

``` {.c}
int n = 10;
int v[n];
```

Kelihatannya sama, dan dalam banyak hal memang sama, tapi cara ini memberi
keleluasaan untuk menghitung ukuran yang dibutuhkan, lalu mendapatkan
array dengan ukuran persis itu.

Mari kita minta pengguna memasukkan ukuran array, lalu menyimpan
indeks-kali-10 di setiap elemen array itu:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int n;
    char buf[32];

    printf("Enter a number: "); fflush(stdout);
    fgets(buf, sizeof buf, stdin);
    n = strtoul(buf, NULL, 10);

    int v[n];

    for (int i = 0; i < n; i++)
        v[i] = i * 10;

    for (int i = 0; i < n; i++)
        printf("v[%d] = %d\n", i, v[i]);
}
```

(Di baris 8 aku menaruh `fflush()` yang seharusnya memaksa baris itu keluar
meskipun aku tidak menaruh newline di akhirnya.)

Baris 12 adalah tempat VLA dideklarasikan---begitu eksekusi melewati baris
itu, ukuran array ditetapkan sesuai nilai `n` pada saat itu. Panjang array
tidak bisa diubah belakangan.

Kamu juga bisa menaruh ekspresi di dalam kurung siku:

``` {.c}
int v[x * 100];
```

Beberapa batasan:

* VLA tidak bisa dideklarasikan di file scope, dan tidak bisa dibuat
  `static` di block scope^[Ini karena VLA biasanya dialokasikan di stack,
  sedangkan variabel `static` ada di heap. Dan seluruh gagasan VLA adalah
  mereka otomatis didealokasikan saat stack frame di-pop di akhir
  fungsi.].
* Kamu tidak bisa memakai initializer list untuk menginisialisasi array.

Selain itu, memasukkan nilai negatif untuk ukuran array memicu undefined
behavior---setidaknya di alam semesta ini.

[i[Variable-length array-->defining]>]

## `sizeof` dan VLA

[i[Variable-length array-->and `sizeof()`]<]

Kita terbiasa dengan `sizeof` yang memberi ukuran dalam byte untuk object
tertentu, termasuk array. VLA pun tidak terkecuali.

Perbedaan utamanya: `sizeof` pada VLA dieksekusi saat _runtime_, sedangkan
pada variabel berukuran tetap ia dihitung saat _compile time_.

Tapi pemakaiannya sama.

Kamu bahkan bisa menghitung jumlah elemen dalam VLA dengan trik array
biasa:

``` {.c}
size_t num_elems = sizeof v / sizeof v[0];
```

Baris di atas menyiratkan hal yang halus dan benar: aritmetika pointer
bekerja persis seperti yang kamu harapkan pada array biasa. Jadi pakai
saja sepuasmu:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int n = 5;
    int v[n];

    int *p = v;

    *(p+2) = 12;
    printf("%d\n", v[2]);  // 12

    p[3] = 34;
    printf("%d\n", v[3]);  // 34
}
```

Seperti pada array biasa, kamu bisa memakai tanda kurung dengan `sizeof()`
untuk mengetahui ukuran calon VLA tanpa benar-benar mendeklarasikannya:

``` {.c}
int x = 12;

printf("%zu\n", sizeof(int [x]));  // Mencetak 48 di sistemku
```

[i[Variable-length array-->and `sizeof()`]>]

## VLA Multidimensi

[i[Variable-length array-->multidimensional]<]

Kamu boleh membuat berbagai macam VLA dengan satu dimensi atau lebih
berupa variabel

``` {.c}
int w = 10;
int h = 20;

int x[h][w];
int y[5][w];
int z[10][w][20];
```

Sekali lagi, kamu bisa menjelajahinya persis seperti array biasa.

[i[Variable-length array-->multidimensional]>]

## Mengoper VLA Satu Dimensi ke Fungsi

[i[Variable-length array-->passing to functions]<]

Mengoper VLA satu dimensi ke fungsi tidak berbeda dari mengoper array
biasa. Langsung saja.

``` {.c .numberLines}
#include <stdio.h>

int sum(int count, int *v)
{
    int total = 0;

    for (int i = 0; i < count; i++)
        total += v[i];

    return total;
}

int main(void)
{
    int x[5];   // Array standar

    int a = 5;
    int y[a];   // VLA

    for (int i = 0; i < a; i++)
        x[i] = y[i] = i + 1;

    printf("%d\n", sum(5, x));
    printf("%d\n", sum(a, y));
}
```

Tapi ada sedikit lebih banyak daripada itu. Kamu juga bisa memberi tahu C
bahwa array itu berukuran VLA tertentu dengan mengopernya lebih dulu lalu
menyebut dimensi itu di daftar parameter:

``` {.c}
int sum(int count, int v[count])
{
    // ...
}
```

[i[Variable-length array-->in function prototypes]<]
[i[`*` for VLA function prototypes]<]

Omong-omong, ada beberapa cara menulis prototype untuk fungsi di atas;
salah satunya memakai `*` kalau kamu tidak ingin menyebut nama nilai di
VLA secara spesifik. Ia hanya menandakan bahwa tipenya VLA, bukan pointer
biasa.

Prototype VLA:

``` {.c}
void do_something(int count, int v[count]);  // Dengan nama
void do_something(int, int v[*]);            // Tanpa nama
```

Sekali lagi, `*` itu hanya berfungsi di prototype---di fungsinya sendiri,
kamu harus menulis ukuran secara eksplisit.

[i[`*` for VLA function prototypes]>]
[i[Variable-length array-->in function prototypes]>]

Nah---_saatnya multidimensi_! Di sinilah keseruan dimulai.

## Mengoper VLA Multidimensi ke Fungsi

Sama seperti yang kita lakukan pada bentuk kedua VLA satu dimensi di
atas, tapi kali ini kita mengoper dua dimensi dan memakainya.

Pada contoh berikut, kita membuat matriks tabel perkalian dengan lebar dan
tinggi yang bervariasi, lalu mengopernya ke fungsi untuk dicetak.

``` {.c .numberLines}
#include <stdio.h>

void print_matrix(int h, int w, int m[h][w])
{
    for (int row = 0; row < h; row++) {
        for (int col = 0; col < w; col++)
            printf("%2d ", m[row][col]);
        printf("\n");
    }
}

int main(void)
{
    int rows = 4;
    int cols = 7;

    int matrix[rows][cols];

    for (int row = 0; row < rows; row++)
        for (int col = 0; col < cols; col++)
            matrix[row][col] = row * col;

    print_matrix(rows, cols, matrix);
}
```

### VLA Multidimensi Parsial

Sebagian dimensi bisa tetap dan sebagian variabel. Misalkan kita punya
panjang record yang tetap 5 elemen, tapi kita tidak tahu ada berapa record.

``` {.c .numberLines}
#include <stdio.h>

void print_records(int count, int record[count][5])
{
    for (int i = 0; i < count; i++) {
        for (int j = 0; j < 5; j++)
            printf("%2d ", record[i][j]);
        printf("\n");
    }
}

int main(void)
{
    int rec_count = 3;
    int records[rec_count][5];

    // Isi dengan beberapa data tiruan
    for (int i = 0; i < rec_count; i++)
        for (int j = 0; j < 5; j++)
            records[i][j] = (i+1)*(j+2);

    print_records(rec_count, records);
}
```

[i[Variable-length array-->passing to functions]>]

## Kompatibilitas dengan Array Biasa

[i[Variable-length array-->with regular arrays]<]

Karena VLA di memori sama saja dengan array biasa, boleh sekali mengoper
keduanya secara bergantian... selama dimensinya cocok.

Misalnya, kalau ada fungsi yang secara spesifik menginginkan array
$3\times5$, kita tetap bisa mengoper VLA ke dalamnya.

``` {.c}
int foo(int m[5][3]) {...}

\\ ...

int w = 3, h = 5;
int matrix[h][w];

foo(matrix);   // OK!
```

Begitu juga, kalau kamu punya fungsi VLA, kamu bisa mengoper array biasa
ke dalamnya:

``` {.c}
int foo(int h, int w, int m[h][w]) {...}

\\ ...

int matrix[3][5];

foo(3, 5, matrix);   // OK!
```

Tapi waspada: kalau dimensinya tidak cocok, kemungkinan besar kamu akan
mengalami undefined behavior.

[i[Variable-length array-->with regular arrays]>]

## `typedef` dan VLA

[i[Variable-length array-->with `typedef`]<]

Kamu bisa me-`typedef` VLA, tapi perilakunya mungkin tidak seperti yang
kamu harapkan.

Pada dasarnya, `typedef` membuat tipe baru dengan nilai-nilai seperti yang
ada pada saat `typedef` dieksekusi.

Jadi ini bukan `typedef` dari VLA, melainkan tipe array berukuran tetap
baru dengan dimensi pada saat itu.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int w = 10;

    typedef int goat[w];

    // goat adalah array berisi 10 int
    goat x;

    // Inisialisasi dengan kuadrat angka
    for (int i = 0; i < w; i++)
        x[i] = i*i;

    // Cetak semuanya
    for (int i = 0; i < w; i++)
        printf("%d\n", x[i]);

    // Sekarang kita ubah w...

    w = 20;

    // Tapi goat TETAP array berisi 10 int, karena itulah nilai w
    // saat typedef dieksekusi.
}
```

Jadi ia bertingkah seperti array berukuran tetap.

Tapi kamu tetap tidak bisa memakai initializer list padanya.

[i[Variable-length array-->with `typedef`]>]

## Jebakan Lompatan

[i[Variable-length array-->with `goto`]<]

Kamu harus berhati-hati saat memakai `goto` di dekat VLA karena banyak hal
yang tidak legal.

[i[Variable-length array-->with `goto`]>]
[i[Variable-length array-->with `longjmp()`]<]

Dan saat memakai `longjmp()` ada kasus ketika kamu bisa membocorkan memori
dengan VLA.

[i[Variable-length array-->with `longjmp()`]>]

Tapi kedua hal ini akan kita bahas di bab masing-masing.


## Masalah Umum {#vla-general-issues}

[i[Variable-length array-->controversy]<]

VLA dilarang di kernel Linux karena beberapa alasan:

* Banyak tempat yang memakainya seharusnya cukup dengan ukuran tetap.
* Kode di balik VLA lebih lambat (sampai tingkat yang tidak disadari
  kebanyakan orang, tapi berpengaruh pada sistem operasi).
* VLA tidak didukung sama baiknya oleh semua C compiler.
* Ukuran stack terbatas, dan VLA masuk ke stack. Kalau ada kode yang
  tidak sengaja (atau sengaja) mengoper nilai besar ke fungsi kernel yang
  mengalokasikan VLA, _Hal-Hal Buruk_™ bisa terjadi.

Orang lain di internet menunjukkan bahwa tidak ada cara mendeteksi
kegagalan alokasi VLA, dan program yang mengalami masalah seperti itu
kemungkinan besar langsung crash. Array berukuran tetap juga punya masalah
yang sama, tapi jauh lebih mungkin seseorang tanpa sengaja membuat _VLA
Berukuran Tidak Lazim_ daripada entah bagaimana tanpa sengaja
mendeklarasikan array berukuran tetap, katakanlah, 30 megabyte.

[i[Variable-length array-->controversy]>]
[i[Variable-length array]>]
