<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Arrays {#arrays}

> _“Should array indices start at 0 or 1?  My compromise of 0.5 was
> rejected without, I thought, proper consideration.”_
>
> ---Stan Kelly-Bootle, computer scientist

[i[Arrays]<]
Untungnya, C punya array. Memang, C dianggap bahasa low-level^[Setidaknya
zaman sekarang.], tapi konsep array sudah tertanam di dalamnya. Dan karena
banyak sekali bahasa yang terinspirasi dari sintaks C, kemungkinan besar
kamu sudah akrab dengan `[` dan `]` untuk mendeklarasikan dan memakai
array.

Tapi C hanya _nyaris_ punya array! Seperti yang akan kita lihat nanti,
array di C hanyalah syntactic sugar---jauh di dalamnya, semuanya adalah
pointer dan semacamnya. _Panik!_ Tapi untuk sekarang, kita pakai saja
sebagai array. _Fiuh_.

## Contoh Mudah

Langsung saja kita buat contohnya:

[i[Arrays-->indexing]<]
``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    float f[4];  // Deklarasikan array berisi 4 float

    f[0] = 3.14159;  // Indeks dimulai dari 0, tentu saja.
    f[1] = 1.41421;
    f[2] = 1.61803;
    f[3] = 2.71828;

    // Cetak semuanya:

    for (i = 0; i < 4; i++) {
        printf("%f\n", f[i]);
    }
}
```

Saat mendeklarasikan array, kamu harus menentukan ukurannya. Dan ukuran itu
harus tetap^[Sebenarnya tidak juga, tapi variable-length array---yang
penulis sendiri kurang suka---adalah cerita untuk lain waktu.].

Pada contoh di atas, kita membuat array berisi 4 `float`. Angka di dalam
kurung siku pada deklarasi menunjukkan hal itu.

Di baris-baris berikutnya, kita mengakses nilai di dalam array, baik untuk
mengisi maupun membacanya, lagi-lagi dengan kurung siku.
[i[Arrays-->indexing]>]

Semoga ini terlihat akrab dari bahasa-bahasa yang sudah kamu kenal!

## Mendapatkan Panjang Array

[i[Arrays-->getting the length]<]
Tidak bisa...sebenarnya. C tidak mencatat informasi ini^[Karena di balik
layar array hanyalah pointer ke elemen pertama array, tidak ada informasi
tambahan yang mencatat panjangnya.]. Kamu harus mengelolanya sendiri di
variabel lain.

Kalau dibilang "tidak bisa", maksudnya ada beberapa keadaan ketika kamu
_bisa_. Ada trik untuk mendapatkan jumlah elemen array di scope tempat
array itu dideklarasikan. Tapi pada umumnya, trik ini tidak akan
berfungsi seperti yang kamu harapkan kalau array dioper ke
fungsi^[Karena ketika sebuah array dioper ke fungsi, yang sebenarnya dioper
hanyalah pointer ke elemen pertama array itu, bukan "seluruh" array.].

Mari kita lihat triknya. Idenya, kita ambil
[i[`sizeof` operator-->with arrays]<]`sizeof` array itu, lalu dibagi
ukuran tiap elemennya untuk mendapatkan panjangnya. Misalnya, kalau sebuah
`int` berukuran 4 byte dan array-nya berukuran 32 byte, berarti di
dalamnya ada tempat untuk $\frac{32}{4}$ atau $8$ `int`.

``` {.c}
int x[12];  // 12 int

printf("%zu\n", sizeof x);     // total 48 byte
printf("%zu\n", sizeof(int));  // 4 byte per int

printf("%zu\n", sizeof x / sizeof(int));  // 48/4 = 12 int!
```

Kalau array-nya berisi `char`, maka `sizeof` array itu _adalah_ jumlah
elemennya, karena `sizeof(char)` didefinisikan sebagai 1. Untuk tipe
lainnya, kamu harus membaginya dengan ukuran tiap elemen.

Tapi trik ini hanya berfungsi di scope tempat array didefinisikan. Kalau
array dioper ke fungsi, trik ini gagal. Bahkan kalau ukurannya ditulis
"besar" di signature fungsi:

``` {.c}
void foo(int x[12])
{
    printf("%zu\n", sizeof x);     // 8?! Ke mana perginya 48?
    printf("%zu\n", sizeof(int));  // 4 byte per int

    printf("%zu\n", sizeof x / sizeof(int));  // 8/4 = 2 int?? SALAH.
}
```

Ini terjadi karena ketika array "dioper" ke fungsi, yang dioper hanyalah
pointer ke elemen pertama, dan itulah yang diukur `sizeof`. Pembahasan
lebih lanjut ada di bagian [Mengoper Array Satu Dimensi ke
Fungsi](#passing1darrays) di bawah.

Satu hal lagi yang bisa dilakukan dengan `sizeof` dan array: mengetahui
ukuran array dengan jumlah elemen tertentu tanpa mendeklarasikan array-nya.
Ini seperti mengetahui ukuran `int` lewat `sizeof(int)`.

Misalnya, untuk mengetahui berapa byte yang dibutuhkan array berisi 48
`double`, tulis begini:

``` {.c}
sizeof(double [48]);
```
[i[`sizeof` operator-->with arrays]>]
[i[Arrays-->getting the length]>]

## Array Initializer

[i[Array initializers]<]
Array bisa diinisialisasi lebih dulu dengan konstanta:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    int a[5] = {22, 37, 3490, 18, 95};  // Inisialisasi dengan nilai-nilai ini

    for (i = 0; i < 5; i++) {
        printf("%d\n", a[i]);
    }
}
```

Jangan pernah memasukkan item di initializer lebih banyak daripada tempat
yang tersedia di array, kalau tidak compiler akan ngambek:

``` {.zsh}
foo.c: In function ‘main’:
foo.c:6:39: warning: excess elements in array initializer
    6 |     int a[5] = {22, 37, 3490, 18, 95, 999};
      |                                       ^~~
foo.c:6:39: note: (near initialization for ‘a’)
```

Tapi (fakta menarik!) item di initializer boleh _lebih sedikit_ daripada
tempat yang tersedia di array. Elemen sisanya otomatis diinisialisasi
dengan nol. Ini berlaku umum untuk semua jenis array initializer: kalau
ada initializer, semua yang tidak diberi nilai secara eksplisit akan
bernilai nol.

``` {.c}
int a[5] = {22, 37, 3490};

// sama dengan:

int a[5] = {22, 37, 3490, 0, 0};
```

Bentuk pintas yang sering dipakai untuk mengisi seluruh array dengan nol:

``` {.c}
int a[100] = {0};
```

Artinya, "Buat elemen pertama bernilai nol, lalu secara otomatis buat sisanya
nol juga."

Kamu juga bisa mengisi elemen tertentu di initializer dengan menyebutkan
indeksnya! Kalau kamu melakukan ini, C dengan senang hati melanjutkan
pengisian nilai berikutnya sampai initializer habis, dan mengisi sisanya
dengan `0`.

Caranya, taruh indeks di dalam kurung siku diikuti `=`, lalu tulis
nilainya.

Berikut contoh pembuatan array:

``` {.c}
int a[10] = {0, 11, 22, [5]=55, 66, 77};
```

Karena kita menetapkan indeks 5 sebagai tempat awal `55`, data hasil di
dalam array adalah:

``` {.default}
0 11 22 0 0 55 66 77 0 0
```

Ekspresi konstan sederhana juga boleh dipakai di sana.

``` {.c}
#define COUNT 5

int a[COUNT] = {[COUNT-3]=3, 2, 1};
```

yang menghasilkan:

``` {.default}
0 0 3 2 1
```

Terakhir, kamu juga bisa menyuruh C menghitung ukuran array dari
initializer-nya, cukup dengan tidak menulis ukurannya:

``` {.c}
int a[3] = {22, 37, 3490};

// sama dengan:

int a[] = {22, 37, 3490};  // Ukurannya tidak ditulis!
```
[i[Array initializers]>]

## Di Luar Batas!

[i[Arrays-->out of bounds]<]
C tidak melarang kamu mengakses array di luar batasnya. Bahkan mungkin tidak
ada peringatan sama sekali.

Kita pinjam contoh di atas dan terus mencetak melewati ujung array. Array
ini hanya punya 5 elemen, tapi mari coba cetak 10 dan lihat apa yang
terjadi:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    int a[5] = {22, 37, 3490, 18, 95};

    for (i = 0; i < 10; i++) {  // KABAR BURUK: mencetak terlalu banyak elemen!
        printf("%d\n", a[i]);
    }
}
```

Di komputer penulis, hasilnya:

``` {.default}
22
37
3490
18
95
32765
1847052032
1780534144
-56487472
21890
```

Waduh! Apa itu? Ternyata mencetak melewati ujung array menghasilkan apa
yang disebut developer C sebagai _undefined behavior_. Makhluk ini akan kita
bahas lebih lanjut nanti, tapi untuk sekarang artinya: "Kamu sudah
melakukan hal buruk, dan apa pun bisa terjadi selama programmu berjalan."

Dan "apa pun" di sini biasanya berarti menemukan angka nol, menemukan
angka sampah, atau crash. Tapi sebenarnya spesifikasi C menyatakan bahwa
dalam keadaan ini compiler boleh menghasilkan kode yang melakukan
_apa saja_^[Pada masa MS-DOS dulu, sebelum ada proteksi memori, penulis
pernah menulis kode C yang sangat kasar dan sengaja melakukan segala macam
undefined behavior. Tapi penulis tahu apa yang dilakukan, dan semuanya
berjalan cukup lancar. Sampai suatu langkah keliru menyebabkan komputer
hang dan, seperti yang ketahuan saat reboot, menghapus semua pengaturan
BIOS. Seru sekali. (Salam untuk @man atas masa-masa seru itu.)].

Singkatnya: jangan pernah melakukan apa pun yang menyebabkan undefined
behavior. Pernah^[Banyak hal yang menyebabkan undefined behavior, bukan
hanya akses array di luar batas. Inilah yang membuat bahasa C begitu
_menarik_.].
[i[Arrays-->out of bounds]>]

## Array Multidimensi

[i[Arrays-->multidimensional]<]
Kamu boleh menambahkan dimensi sebanyak apa pun pada array.

``` {.c}
int a[10];
int b[2][7];
int c[4][5][6];
```

Array ini disimpan di memori dengan [flw[row-major
order|Row-_and_column-major_order]]. Artinya, pada array 2D, indeks pertama
menunjukkan baris, dan indeks kedua menunjukkan kolom.

Array multidimensi juga bisa diberi initializer dengan cara menyarangkannya:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int row, col;

    int a[2][5] = {      // Inisialisasi array 2D
        {0, 1, 2, 3, 4},
        {5, 6, 7, 8, 9}
    };

    for (row = 0; row < 2; row++) {
        for (col = 0; col < 5; col++) {
            printf("(%d,%d) = %d\n", row, col, a[row][col]);
        }
    }
}
```

Outputnya:

``` {.default}
(0,0) = 0
(0,1) = 1
(0,2) = 2
(0,3) = 3
(0,4) = 4
(1,0) = 5
(1,1) = 6
(1,2) = 7
(1,3) = 8
(1,4) = 9
```

Dan kamu juga bisa memakai indeks eksplisit:

``` {.c}
// Buat matriks identitas 3x3

int a[3][3] = {[0][0]=1, [1][1]=1, [2][2]=1};
```

yang membangun array 2D seperti ini:

``` {.default}
1 0 0
0 1 0
0 0 1
```
[i[Arrays-->multidimensional]>]

## Array dan Pointer

[i[Arrays-->as pointers]<]
[_Sambil lalu_] Nah... tadi di atas mungkin ada disinggung bahwa array itu
pointer, jauh di dalamnya? Sekarang kita menyelam sebentar di perairan
dangkal supaya semuanya tidak benar-benar membingungkan. Nanti kita
bahas hubungan sebenarnya antara array dan pointer, tapi untuk saat ini
kita hanya akan melihat cara mengoper array ke fungsi.

### Mendapatkan Pointer ke Array

Ada rahasia yang perlu disampaikan. Secara umum, ketika programmer C
berbicara tentang pointer ke array, yang mereka maksud adalah pointer
_ke elemen pertama_ array^[Ini secara teknis tidak tepat, karena pointer
ke array dan pointer ke elemen pertama array punya tipe yang berbeda. Tapi
soal itu kita urus nanti saat sudah sampai.].

Jadi mari kita ambil pointer ke elemen pertama sebuah array.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a[5] = {11, 22, 33, 44, 55};
    int *p;

    p = &a[0];  // p menunjuk ke array
                // Tepatnya, ke elemen pertama

    printf("%d\n", *p);  // Mencetak "11"
}
```

Karena begitu sering dipakai di C, bahasanya menyediakan jalan pintas:

``` {.c .numberLines}
p = &a[0];  // p menunjuk ke array

// sama dengan:

p = a;      // p menunjuk ke array, tapi tampilannya jauh lebih enak!
```

Menyebut nama array saja, tanpa embel-embel, sama dengan mengambil pointer
ke elemen pertama array! Ini akan sering kita pakai di contoh-contoh
berikutnya.

Tapi tunggu sebentar---bukankah `p` itu `int*`? Dan `*p` menghasilkan
`11`, sama seperti `a[0]`? Yessss. Kamu mulai melihat sekilas hubungan
antara array dan pointer di C. (Kita akan membahasnya jauh lebih dalam di
bab [Pointer II](#pointers2), di bagian [Kesetaraan
Array/Pointer](#arraypointerequiv).)
[i[Arrays-->as pointers]>]

### Mengoper Array Satu Dimensi ke Fungsi {#passing1darrays}

[i[Arrays-->passing to functions]<]
Kita coba contoh dengan array satu dimensi. Penulis akan menulis dua
fungsi yang bisa menerima array dan melakukan hal berbeda.

Bersiaplah untuk signature fungsi yang bikin takjub!

``` {.c .numberLines}
#include <stdio.h>

// Dioper sebagai pointer ke elemen pertama
void times2(int *a, int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 2);
}

// Sama saja, tapi memakai notasi array
void times3(int a[], int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 3);
}

// Sama saja, tapi memakai notasi array dengan ukuran
void times4(int a[5], int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 4);
}

int main(void)
{
    int x[5] = {11, 22, 33, 44, 55};

    times2(x, 5);
    times3(x, 5);
    times4(x, 5);
}
```

Semua cara menuliskan array sebagai parameter fungsi tersebut identik.

``` {.c}
void times2(int *a, int len)
void times3(int a[], int len)
void times4(int a[5], int len)
```

Di kalangan pemakai C yang sudah berpengalaman, bentuk pertama jauh lebih
umum.

Bahkan pada bentuk terakhir, compiler tidak peduli angka berapa yang kamu
tulis (selain harus lebih besar dari nol^[C11 §6.7.6.2¶1 mensyaratkan
angkanya lebih besar dari nol. Tapi kamu mungkin menemukan kode yang
mendeklarasikan array berukuran nol di akhir `struct`, dan GCC cukup longgar
soal ini kecuali kamu mengompilasi dengan `-pedantic`. Array berukuran nol
ini dulu adalah mekanisme akal-akalan untuk membuat struktur dengan ukuran
variabel. Sayangnya, secara teknis mengakses array seperti itu adalah
undefined behavior, meskipun di mana-mana praktis berfungsi. C99
menetapkan pengganti yang terdefinisi dengan baik, yang disebut _flexible
array member_, dan akan kita bahas nanti.]). Compiler sama sekali tidak
memaksakan apa pun.

Setelah mengatakan itu, ukuran array di deklarasi fungsi sebenarnya
_penting_ ketika kita mengoper array multidimensi ke fungsi, tapi nanti
kita kembali ke sana.
[i[Arrays-->passing to functions]>]

### Mengubah Array di Dalam Fungsi

[i[Arrays-->modifying within functions]<]
Kita sudah bilang bahwa array hanyalah pointer yang menyamar. Artinya,
kalau kamu mengoper array ke fungsi, kemungkinan besar yang kamu oper
adalah pointer ke elemen pertama array.

Tapi kalau fungsi punya pointer ke data, fungsi itu bisa mengubah data
tersebut! Jadi perubahan yang dilakukan fungsi pada array akan terlihat
juga di caller.

Berikut contoh ketika kita mengoper pointer ke array ke sebuah fungsi,
fungsi itu mengubah nilai-nilai di array, dan perubahannya terlihat di
caller.

``` {.c .numberLines}
#include <stdio.h>

void double_array(int *a, int len)
{
    // Kalikan setiap elemen dengan 2
    //
    // Ini menggandakan nilai di x pada main() karena x dan a sama-sama menunjuk
    // ke array yang sama di memori!

    for (int i = 0; i < len; i++)
        a[i] *= 2;
}

int main(void)
{
    int x[5] = {1, 2, 3, 4, 5};

    double_array(x, 5);

    for (int i = 0; i < 5; i++)
        printf("%d\n", x[i]);  // 2, 4, 6, 8, 10!
}
```

Walaupun array dioper sebagai parameter `a` yang bertipe `int*`, lihat
bagaimana kita mengaksesnya dengan notasi array `a[i]`! Haaah. Ini sepenuhnya
diperbolehkan.

Nanti ketika kita membahas kesetaraan antara array dan pointer, hal ini
akan jauh lebih masuk akal. Untuk sekarang, cukup ketahui bahwa fungsi bisa
mengubah array dan perubahannya terlihat di caller.
[i[Arrays-->modifying within functions]>]

### Mengoper Array Multidimensi ke Fungsi

[i[Arrays-->passing to functions]<]
Ceritanya sedikit berubah ketika kita membicarakan array multidimensi. C
perlu tahu semua dimensi (kecuali yang pertama) supaya punya cukup
informasi untuk mengetahui di mana di memori nilai tertentu harus dicari.

Berikut contoh ketika kita eksplisit menyebutkan semua dimensi:

``` {.c .numberLines}
#include <stdio.h>

void print_2D_array(int a[2][3])
{
    for (int row = 0; row < 2; row++) {
        for (int col = 0; col < 3; col++)
            printf("%d ", a[row][col]);
        printf("\n");
    }
}

int main(void)
{
    int x[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };

    print_2D_array(x);
}
```

Tapi dalam kasus ini, kedua bentuk^[Ini juga setara: `void
print_2D_array(int (*a)[3])`, tapi itu sudah melebihi yang ingin dibahas
sekarang.] berikut setara:

``` {.c}
void print_2D_array(int a[2][3])
void print_2D_array(int a[][3])
```

Compiler sebenarnya hanya butuh dimensi kedua supaya tahu seberapa jauh
harus melompat di memori untuk setiap kenaikan dimensi pertama. Secara umum,
compiler harus tahu semua dimensi kecuali yang pertama.

Ingat juga bahwa compiler hanya melakukan pengecekan batas saat kompilasi
secara minimal (kalau beruntung), dan C sama sekali tidak mengecek batas
saat runtime. Tanpa sabuk pengaman! Jangan sampai crash karena mengakses
elemen array di luar batas!
[i[Arrays-->passing to functions]>] [i[Arrays]>]
