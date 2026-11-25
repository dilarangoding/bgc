<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Pointer III: Pointer ke Pointer dan Lainnya

Di sinilah kita membahas pemakaian pointer tingkat menengah dan lanjut.
Kalau kamu belum menguasai pointer, ulas dulu bab-bab sebelumnya tentang
[pointer](#pointers) dan [aritmetika pointer](#pointers2) sebelum mulai
dengan materi ini.

## Pointer ke Pointer

[i[Pointers-->to pointers]<]

Kalau kamu bisa punya pointer ke sebuah variabel, dan variabel bisa
berupa pointer, bisakah kamu punya pointer ke variabel yang itu sendiri
adalah pointer?

Bisa! Itulah pointer ke pointer, yang disimpan dalam variabel
bertipe pointer-pointer. 

Sebelum membedahnya, aku ingin mencoba membangun _gut feel_ tentang cara
kerja pointer ke pointer.

Ingat bahwa pointer hanyalah sebuah angka. Angka yang mewakili indeks
dalam memori komputer, biasanya indeks yang menyimpan nilai yang
menarik minat kita karena suatu alasan.

Pointer itu, yang berupa angka, harus disimpan di suatu tempat. Dan
tempat itu adalah memori, sama seperti yang lainnya^[Ada setan di
detailnya untuk nilai yang hanya disimpan di register, tapi untuk
keperluan kita di sini, hal itu aman untuk diabaikan. Lagi pula
spesifikasi C tidak mengambil sikap soal "register" ini di luar kata
kunci `register`, yang deskripsinya tidak menyebut register.].

Tapi karena disimpan di memori, ia pasti punya indeks tempat ia disimpan,
kan? Pointer itu pasti punya indeks di memori tempat ia disimpan. Dan
indeks itu adalah sebuah angka. Itulah alamat pointer tersebut. Itulah
pointer ke pointer.

Kita mulai dengan pointer biasa ke sebuah `int`, dari bab-bab sebelumnya:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int x = 3490;  // Tipe: int
    int *p = &x;   // Tipe: pointer ke int

    printf("%d\n", *p);  // 3490
}
```

Cukup lugas, kan? Ada dua tipe yang terlibat: `int` dan `int*`, dan kita
mengatur `p` agar menunjuk ke `x`. Lalu kita bisa men-dereference `p` di
baris 8 dan mencetak nilai `3490`.

Tapi, seperti yang kita katakan, kita bisa punya pointer ke variabel
apa pun... jadi apakah itu berarti kita bisa punya pointer ke `p`?

Dengan kata lain, ekspresi ini bertipe apa?


``` {.c}
int x = 3490;  // Tipe: int
int *p = &x;   // Tipe: pointer ke int

&p  // <-- Apa tipe alamat p? Alias pointer ke p?
```

Kalau `x` adalah `int`, maka `&x` adalah pointer ke `int` yang kita simpan
di `p` yang bertipe `int*`. Ikut? (Ulangi paragraf ini sampai paham!)

Dan karena itu `&p` adalah pointer ke `int*`, alias "pointer ke pointer
ke `int`". Alias "`int`-pointer-pointer".

Paham? (Ulangi paragraf sebelumnya sampai paham!)

Kita menulis tipe ini dengan dua asterisk: `int **`. Kita lihat
pemakaiannya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int x = 3490;  // Tipe: int
    int *p = &x;   // Tipe: pointer ke int
    int **q = &p;  // Tipe: pointer ke pointer ke int

    printf("%d %d\n", *p, **q);  // 3490 3490
}
```

Mari kita karang beberapa alamat pura-pura untuk nilai-nilai di atas
sebagai contoh dan lihat seperti apa ketiga variabel ini di memori. Nilai
alamat di bawah ini hanya karanganku untuk keperluan contoh:

|Variabel|Disimpan di Alamat|Nilai yang Tersimpan di Sana|
|-|-|-|
|`x`|`28350`|`3490`---nilai dari kode|
|`p`|`29122`|`28350`---alamat `x`!|
|`q`|`30840`|`29122`---alamat `p`!|

Sekarang mari kita coba sungguhan di komputerku^[Kemungkinan besar
angka di komputermu berbeda.] dan mencetak nilai pointer dengan `%p`, lalu
aku ulangi tabel yang sama dengan referensi sebenarnya (dicetak dalam hex).

|Variabel|Disimpan di Alamat|Nilai yang Tersimpan di Sana|
|-|-|-|
|`x`|`0x7ffd96a07b94`|`3490`---nilai dari kode|
|`p`|`0x7ffd96a07b98`|`0x7ffd96a07b94`---alamat `x`!|
|`q`|`0x7ffd96a07ba0`|`0x7ffd96a07b98`---alamat `p`!|

Kamu bisa lihat alamat-alamat itu sama kecuali byte terakhir, jadi fokus
saja ke sana.

Di sistemku, `int` berukuran 4 byte, itulah kenapa kita melihat
alamat naik 4 dari `x` ke `p`^[Sama sekali tidak ada di spesifikasi yang
mengatakan ini akan selalu begini, tapi kebetulan di sistemku
begitu.] lalu naik 8 dari `p` ke `q`. Di sistemku, semua pointer
berukuran 8 byte.

Apakah penting kalau itu `int*` atau `int**`? Apakah salah satunya lebih
banyak byte-nya? Tidak! Ingat bahwa semua pointer adalah alamat, yaitu
indeks ke dalam memori. Dan di mesinku, sebuah indeks bisa
direpresentasikan dengan 8 byte... tidak peduli apa yang disimpan di
indeks itu.

Sekarang lihat apa yang kita lakukan di baris 9 pada contoh sebelumnya:
kita _men-dereference ganda_ `q` untuk kembali ke `3490`.

Ini bagian penting tentang pointer dan pointer ke pointer:

* Kamu bisa mendapat pointer ke apa pun dengan `&` (termasuk ke pointer!)
* Kamu bisa mendapat yang ditunjuk sebuah pointer dengan `*` (termasuk
  pointer!)

Jadi kamu bisa menganggap `&` dipakai untuk membuat pointer, dan `*`
adalah kebalikannya---ia berjalan ke arah yang berlawanan dari `&`---untuk
sampai ke yang ditunjuk.

Dari sisi tipe, setiap kali kamu memakai `&`, itu menambah satu tingkat
pointer pada tipenya.

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int x`|`&x`|`int *`|
|`int *x`|`&x`|`int **`|
|`int **x`|`&x`|`int ***`|
|`int ***x`|`&x`|`int ****`|

Dan setiap kali kamu memakai dereference (`*`), ia melakukan kebalikannya:

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int ****x`|`*x`|`int ***`|
|`int ***x`|`*x`|`int **`|
|`int **x`|`*x`|`int *`|
|`int *x`|`*x`|`int`|

Perhatikan bahwa kamu bisa memakai beberapa `*` berturut-turut untuk
men-dereference dengan cepat, seperti yang kita lihat pada kode contoh
dengan `**q` di atas. Masing-masing mengupas satu tingkat indirection.

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int ****x`|`***x`|`int *`|
|`int ***x`|`**x`|`int *`|
|`int **x`|`**x`|`int`|

Secara umum, `&*E == E`^[Bahkan kalau `E` adalah `NULL`, ternyata,
anehnya.]. Dereference "membatalkan" address-of.

Tapi `&` tidak bekerja dengan cara yang sama---kamu hanya bisa
melakukannya satu per satu, dan harus menyimpan hasilnya di variabel
perantara:

``` {.c}
int x = 3490;     // Tipe: int
int *p = &x;      // Tipe: int *, pointer ke int
int **q = &p;     // Tipe: int **, pointer ke pointer ke int
int ***r = &q;    // Tipe: int ***, pointer ke pointer ke pointer ke int
int ****s = &r;   // Tipe: int ****, kamu paham maksudnya
int *****t = &s;  // Tipe: int *****
```

[i[Pointers-->to pointers]>]

### Pointer Pointer dan `const`

[i[Pointers-->to pointers, `const`]<]

Kalau kamu ingat, mendeklarasikan pointer seperti ini:

``` {.c}
int *const p;
```

berarti kamu tidak bisa mengubah `p`. Mencoba `p++` akan memberi error saat
kompilasi.

Tapi bagaimana ini bekerja dengan `int **` atau `int ***`? Di mana
`const` ditaruh, dan apa artinya?

Kita mulai dari bagian sederhana. `const` yang tepat di sebelah nama
variabel merujuk ke variabel itu. Jadi kalau kamu ingin `int***` yang
tidak bisa diubah, kamu bisa menulis:

``` {.c}
int ***const p;

p++;  // Tidak diperbolehkan
```

Tapi di sinilah semuanya jadi agak aneh.

Bagaimana kalau kita punya situasi ini:

``` {.c .numberLines}
int main(void)
{
    int x = 3490;
    int *const p = &x;
    int **q = &p;
}
```

Waktu kubangun, aku mendapat peringatan ini:

``` {.default}
warning: initialization discards ‘const’ qualifier from pointer target type
    7 |     int **q = &p;
      |               ^
```

Apa yang terjadi? Compiler memberi tahu kita di sini bahwa ada variabel
yang `const`, dan kita menugaskan nilainya ke variabel lain yang tidak
`const` dengan cara yang sama. Sifat "`const`" dibuang, yang mungkin
bukan yang kita inginkan.

Tipe `p` adalah `int *const p`, jadi `&p` bertipe `int *const *`. Dan kita
mencoba menugaskannya ke `q`.

Tapi `q` adalah `int **`! Tipe dengan sifat `const` yang berbeda pada `*`
pertama! Jadi kita mendapat peringatan bahwa `const` pada `int *const *`
milik `p` diabaikan dan dibuang.

Kita bisa memperbaikinya dengan memastikan tipe `q` setidaknya sama
`const`-nya dengan `p`.

``` {.c}
int x = 3490;
int *const p = &x;
int *const *q = &p;
```

Dan sekarang kita senang.

Kita bahkan bisa membuat `q` lebih `const` lagi. Seperti di atas, kita
berkata, "`q` itu sendiri tidak `const`, tapi yang ditunjuknya `const`."
Tapi kita bisa membuat keduanya `const`:

``` {.c}
int x = 3490;
int *const p = &x;
int *const *const q = &p;  // Lebih const!
```

Dan itu juga berfungsi. Sekarang kita tidak bisa mengubah `q`, maupun
pointer yang ditunjuk `q`.

[i[Pointers-->to pointers, `const`]>]

## Nilai Multibyte {#multibyte-values}

[i[Pointers-->to multibyte values]<]

Kita sudah menyiratkan ini di berbagai tempat sebelumnya, tapi jelas tidak
setiap nilai bisa disimpan dalam satu byte memori. Banyak hal memakai
beberapa byte memori (dengan asumsi bukan `char`). Kamu bisa mengetahui
jumlah byte dengan `sizeof`. Dan kamu bisa mengetahui alamat memori mana
yang merupakan byte _pertama_ dari object dengan operator `&` standar,
yang selalu mengembalikan alamat byte pertama.

Dan ini fakta menarik lainnya! Kalau kamu menelusuri byte-byte dari object
mana pun, kamu mendapat _object representation_-nya. Dua hal dengan object
representation yang sama di memori adalah sama.

Kalau kamu ingin menelusuri object representation, lakukan dengan
pointer ke `unsigned char`.

Mari kita buat versi kita sendiri dari
[fl[`memcpy()`|https://beej.us/guide/bgclr/html/split/stringref.html#man-memcpy]]
yang melakukan persis itu:

``` {.c}
void *my_memcpy(void *dest, const void *src, size_t n)
{
    // Buat variabel lokal untuk src dan dest, tapi bertipe unsigned char

    const unsigned char *s = src;
    unsigned char *d = dest;

    while (n-- > 0)   // Untuk jumlah byte yang diberikan
        *d++ = *s++;  // Salin byte sumber ke byte tujuan

    // Kebanyakan fungsi salin mengembalikan pointer ke dest demi
    // kemudahan caller

    return dest;
}
```

(Ada beberapa contoh bagus post-increment dan post-decrement di situ yang
bisa kamu pelajari juga.)

Penting dicatat bahwa versi di atas kemungkinan kurang efisien dibanding
yang disertakan di sistemmu.

Tapi kamu bisa mengoper pointer ke apa pun ke dalamnya, dan ia akan
menyalin object itu. Bisa `int*`, `struct animal*`, atau apa saja.

Mari kita coba contoh lain yang mencetak byte object representation dari
sebuah `struct` supaya kita bisa melihat apakah ada padding di dalamnya
dan nilainya apa^[C compiler-mu tidak wajib menambahkan padding byte, dan
nilai padding byte yang ditambahkan itu tak tentu.].

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    char a;
    int b;
};

int main(void)
{
    struct foo x = {0x12, 0x12345678};
    unsigned char *p = (unsigned char *)&x;

    for (size_t i = 0; i < sizeof x; i++) {
        printf("%02X\n", p[i]);
    }
}
```

Di situ ada `struct foo` yang disusun sedemikian rupa agar compiler
terdorong menyisipkan padding byte (walaupun tidak wajib). Lalu kita
ambil `unsigned char *` ke byte pertama variabel
`struct foo` `x`.

Dari sana, yang kita perlu tahu hanya `sizeof x` dan kita bisa menelusuri
sebanyak itu byte, mencetak nilainya (dalam hex agar mudah).

Menjalankannya memberi banyak angka sebagai output. Aku sudah memberinya
anotasi di bawah untuk menunjukkan di mana nilai-nilai itu disimpan:

``` {.default}
12  | x.a == 0x12

AB  |
BF  | padding bytes with "random" value
26  |

78  | x.b == 0x12345678
56  |
34  |
12  |
```

Di semua sistem, `sizeof(char)` adalah 1, dan kita lihat byte pertama di
bagian atas output menyimpan nilai `0x12` yang kita simpan di sana.

Lalu ada beberapa padding byte---di sistemku, nilainya berubah dari
satu eksekusi ke eksekusi berikutnya.

Terakhir, di sistemku, `sizeof(int)` adalah 4, dan kita bisa
melihat 4 byte itu di bagian akhir. Perhatikan bahwa byte-nya sama dengan
nilai hex `0x12345678`, tapi anehnya dalam urutan terbalik^[Ini bervariasi
tergantung arsitektur, tapi sistemku _little endian_, artinya byte
paling tidak signifikan dari angka disimpan lebih dulu. Sistem _big
endian_ akan punya `12` di depan dan `78` di belakang. Tapi spesifikasi
tidak menentukan apa pun soal representasi ini.].

Jadi itu sedikit intipan ke bawah kap tentang byte-byte dari entitas yang
lebih kompleks di memori.

[i[Pointers-->to multibyte values]>]

## Pointer `NULL` dan Nol

[i[`NULL` pointer-->zero equivalence]<]

Hal-hal berikut bisa dipakai secara bergantian:

* `NULL`
* `0`
* `'\0'`
* `(void *)0`

Secara pribadi, aku selalu memakai `NULL` kalau maksudnya `NULL`,
tapi kamu mungkin sesekali melihat varian lain. Walaupun `'\0'` (byte
dengan semua bit bernilai nol) juga akan dibandingkan sebagai sama,
membandingkannya dengan pointer itu _aneh_; kamu seharusnya
membandingkan `NULL` dengan pointer. (Tentu saja, sering kali dalam
pemrosesan string, kamu membandingkan _yang ditunjuk pointer_ dengan
`'\0'`, dan itu benar.)

`0` disebut _null pointer constant_, dan, ketika dibandingkan atau
ditugaskan ke pointer lain, ia dikonversi menjadi null pointer dengan
tipe yang sama.

[i[`NULL` pointer-->zero equivalence]>]

## Pointer sebagai Integer

[i[Pointers-->as integers]<]

Kamu bisa meng-cast pointer ke integer dan sebaliknya (karena pointer
hanyalah indeks ke memori), tapi kemungkinan kamu hanya perlu melakukannya
kalau mengerjakan hal-hal hardware level rendah. Hasil dari akal-akalan
semacam itu ditentukan oleh implementasi, jadi tidak portabel. Dan _hal
aneh_ bisa terjadi.

Meski begitu, C memberi satu jaminan: kamu bisa mengonversi pointer ke
tipe `uintptr_t` dan kamu bisa mengonversinya kembali ke pointer tanpa
kehilangan data.

`uintptr_t` didefinisikan di `<stdint.h>`^[Ini fitur opsional, jadi
mungkin tidak ada---tapi kemungkinan besar ada.].

Selain itu, kalau kamu ingin yang signed, kamu bisa memakai `intptr_t`
dengan efek yang sama.

[i[Pointers-->as integers]>]

## Meng-cast Pointer ke Pointer Lain

[i[Pointers-->casting]<]

Hanya ada satu konversi pointer yang aman:

1. Mengonversi ke `intptr_t` atau `uintptr_t`.
2. Mengonversi ke dan dari `void*`.

DUA! Dua konversi pointer yang aman.

3. Mengonversi ke dan dari `char*` (atau `signed char*`/`unsigned char*`).

TIGA! Tiga konversi yang aman!

4. Mengonversi ke dan dari pointer ke sebuah `struct` dan pointer ke
   anggota pertamanya, dan sebaliknya.

EMPAT! Empat konversi yang aman!

Kalau kamu meng-cast ke pointer bertipe lain lalu mengakses object yang
ditunjuknya, perilakunya undefined karena sesuatu yang disebut _strict
aliasing_.

_Aliasing_ biasa merujuk ke kemampuan untuk punya lebih dari satu cara
mengakses object yang sama. Titik-titik akses itu adalah alias satu sama
lain.

_Strict aliasing_ menyatakan kamu hanya boleh mengakses sebuah object lewat
pointer ke _tipe yang kompatibel_ dengan object itu.

Misalnya, ini jelas diperbolehkan:

``` {.c}
int a = 1;
int *p = &a;
```

`p` adalah pointer ke `int`, dan ia menunjuk ke tipe yang
kompatibel---yaitu `int`---jadi aman.

Tapi yang berikut tidak bagus karena `int` dan `float` bukan tipe yang
kompatibel:

``` {.c}
int a = 1;
float *p = (float *)&a;
```

Berikut program demo yang melakukan aliasing. Ia mengambil variabel `v`
bertipe `int32_t` dan meng-alias-kannya ke pointer ke `struct words`.
`struct` itu berisi dua `int16_t`. Tipe-tipe ini tidak kompatibel, jadi
kita melanggar aturan strict aliasing. Compiler akan menganggap kedua
pointer ini tidak pernah menunjuk ke object yang sama... tapi kita
membuatnya menunjuk. Nakal sekali kita ini.

Mari kita lihat apakah kita bisa merusak sesuatu.

``` {.c .numberLines}
#include <stdio.h>
#include <stdint.h>

struct words {
    int16_t v[2];
};

void fun(int32_t *pv, struct words *pw)
{
    for (int i = 0; i < 5; i++) {
        (*pv)++;

        // Cetak nilai 32-bit dan nilai-nilai 16-bit:

        printf("%x, %x-%x\n", *pv, pw->v[1], pw->v[0]);
    }
}

int main(void)
{
    int32_t v = 0x12345678;

    struct words *pw = (struct words *)&v;  // Melanggar strict aliasing

    fun(&v, pw);
}
```

Lihat bagaimana aku mengoper kedua pointer yang tidak kompatibel ke `fun()`?
Satu bertipe `int32_t*` dan yang lain `struct words*`.

Tapi keduanya menunjuk ke object yang sama: nilai 32-bit yang
diinisialisasi dengan `0x12345678`.

Jadi kalau kita lihat field-field di `struct words`, kita seharusnya
melihat dua setengah bagian 16-bit dari angka itu. Benar?

Dan di loop `fun()`, kita meng-increment pointer ke `int32_t`. Itu saja.
Tapi karena `struct` menunjuk ke memori yang sama, ia juga seharusnya
diperbarui ke nilai yang sama.

Mari kita jalankan dan dapatkan ini, dengan nilai 32-bit di kiri dan dua
bagian 16-bit di kanan. Seharusnya cocok^[Aku mencetak nilai 16-bit
secara terbalik karena aku memakai mesin little-endian dan ini membuatnya
lebih mudah dibaca di sini.]:

``` {.default}
12345679, 1234-5679
1234567a, 1234-567a
1234567b, 1234-567b
1234567c, 1234-567c
1234567d, 1234-567d
```

dan memang cocok... _SAMPAI BESOK!_

Mari kita coba mengompilasinya dengan GCC memakai `-O3` dan `-fstrict-aliasing`:

``` {.default}
12345679, 1234-5678
1234567a, 1234-5679
1234567b, 1234-567a
1234567c, 1234-567b
1234567d, 1234-567c
```

Selisih satu! Tapi mereka menunjuk ke memori yang sama! Bagaimana bisa?
Jawabannya: melakukan aliasing memori seperti itu adalah undefined
behavior. _Apa pun mungkin terjadi_, hanya saja bukan dengan cara yang
baik.

Kalau kodemu melanggar aturan strict aliasing, apakah ia berfungsi atau
tidak bergantung pada bagaimana seseorang memutuskan mengompilasinya. Dan
itu menyebalkan karena di luar kendalimu. Kecuali kamu semacam dewa
mahakuasa.

Kemungkinan tidak, maaf.

GCC bisa dipaksa untuk tidak memakai aturan strict aliasing dengan
`-fno-strict-aliasing`. Mengompilasi program demo di atas dengan `-O3` dan
flag ini membuat outputnya sesuai harapan.

Terakhir, _type punning_ adalah memakai pointer bertipe berbeda untuk
melihat data yang sama. Sebelum ada strict aliasing, hal seperti ini cukup
umum:

``` {.c}
int a = 0x12345678;
short b = *((short *)&a);   // Melanggar strict aliasing
```

Kalau kamu ingin melakukan type punning dengan (relatif) aman, lihat
bagian tentang [Union dan Type Punning](#union-type-punning).

[i[Pointers-->casting]>]

## Selisih Pointer {#ptr_differences}

[i[Pointers-->subtracting]<]

Seperti yang kamu tahu dari bagian tentang aritmetika pointer, kamu bisa
mengurangkan satu pointer dari pointer lain^[Dengan asumsi keduanya
menunjuk ke object array yang sama.] untuk mendapatkan selisihnya dalam
jumlah elemen array.

Nah, _tipe selisih itu_ ditentukan oleh implementasi, jadi bisa berbeda
dari satu sistem ke sistem lain.

[i[`ptrdiff_t` type]<]

Agar lebih portabel, kamu bisa menyimpan hasilnya dalam variabel bertipe
`ptrdiff_t` yang didefinisikan di `<stddef.h>`.

``` {.c}
int cats[100];

int *f = cats + 20;
int *g = cats + 60;

ptrdiff_t d = g - f;  // selisihnya 40
```

[i[`ptrdiff_t` type-->printing]<]
Dan kamu bisa mencetaknya dengan menambahkan awalan `t` pada format
specifier integer:

``` {.c}
printf("%td\n", d);  // Cetak desimal: 40
printf("%tX\n", d);  // Cetak hex:     28
```

[i[`ptrdiff_t` type-->printing]>]
[i[`ptrdiff_t` type]>]
[i[Pointers-->subtracting]>]

## Pointer ke Fungsi

[i[Pointers-->to functions]<]

Fungsi hanyalah kumpulan instruksi mesin di memori, jadi tidak ada alasan
kita tidak bisa mengambil pointer ke instruksi pertama fungsi itu.

Lalu memanggilnya.

Ini bisa berguna untuk mengoper pointer ke sebuah fungsi ke fungsi lain
sebagai argumen. Lalu fungsi kedua itu bisa memanggil apa pun yang
dioper.

Bagian yang rumit, meski begitu, adalah C perlu tahu tipe variabel yang
berupa pointer ke fungsi itu.

Dan ia benar-benar ingin tahu semua detailnya.

Seperti "ini adalah pointer ke fungsi yang menerima dua argumen `int` dan
mengembalikan `void`".

Bagaimana menuliskan semua itu agar bisa mendeklarasikan variabel?

Ternyata bentuknya sangat mirip function prototype, hanya dengan beberapa
tanda kurung tambahan:

``` {.c}
// Deklarasikan p sebagai pointer ke fungsi.
// Fungsi ini mengembalikan float, dan menerima dua int sebagai argumen.

float (*p)(int, int);
```

Perhatikan juga bahwa kamu tidak harus memberi nama pada parameter. Tapi
boleh saja kalau mau; namanya hanya diabaikan.

``` {.c}
// Deklarasikan p sebagai pointer ke fungsi.
// Fungsi ini mengembalikan float, dan menerima dua int sebagai argumen.

float (*p)(int a, int b);
```

Jadi sekarang setelah kita tahu cara mendeklarasikan variabel, bagaimana
kita tahu apa yang ditugaskan ke dalamnya? Bagaimana mendapatkan alamat
sebuah fungsi?

Ternyata ada jalan pintas seperti saat mendapatkan pointer ke array: kamu
cukup menyebut nama fungsi tanpa kurung. (Kamu boleh menaruh `&` di
depannya kalau mau, tapi itu tidak perlu dan tidak idiomatis.)

Setelah punya pointer ke fungsi, kamu bisa memanggilnya hanya dengan
menambahkan tanda kurung dan daftar argumen.

Kita coba contoh sederhana yang secara efektif membuat alias untuk sebuah
fungsi dengan mengarahkan pointer ke sana. Lalu kita memanggilnya.

Kode ini mencetak `3490`:

``` {.c .numberLines}
#include <stdio.h>

void print_int(int n)
{
    printf("%d\n", n);
}

int main(void)
{
    // Tugaskan p agar menunjuk ke print_int:

    void (*p)(int) = print_int;

    p(3490);          // Panggil print_int lewat pointer
}
```

Perhatikan bagaimana tipe `p` merepresentasikan tipe nilai kembalian dan
parameter `print_int`. Harus begitu, kalau tidak C akan protes soal tipe
pointer yang tidak kompatibel.

Satu contoh lagi di sini menunjukkan bagaimana kita bisa mengoper pointer
ke fungsi sebagai argumen ke fungsi lain.

Kita akan menulis fungsi yang menerima beberapa argumen integer, ditambah
pointer ke fungsi yang beroperasi pada kedua argumen itu. Lalu ia
mencetak hasilnya.

``` {.c .numberLines}
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int mult(int a, int b)
{
    return a * b;
}

void print_math(int (*op)(int, int), int x, int y)
{
    int result = op(x, y);

    printf("%d\n", result);
}

int main(void)
{
    print_math(add, 5, 7);   // 12
    print_math(mult, 5, 7);  // 35
}
```

Luangkan waktu untuk mencernanya. Idenya, kita akan mengoper pointer ke
sebuah fungsi ke `print_math()`, dan ia akan memanggil fungsi itu untuk
melakukan perhitungan.

Dengan begitu kita bisa mengubah perilaku `print_math()` dengan mengoper
fungsi lain ke dalamnya. Kamu bisa lihat kita melakukannya di baris 22-23
saat mengoper pointer ke fungsi `add` dan `mult`.

Nah, di baris 13, kita semua pasti setuju signature fungsi `print_math()`
adalah pemandangan yang patut dikagumi. Dan, percaya atau tidak, yang ini
sebenarnya cukup lugas dibanding beberapa hal yang bisa kamu
susun^[Bahasa Pemrograman Go mengambil inspirasi sintaks deklarasi
tipenya dari kebalikan dari yang dilakukan C.].

Tapi mari kita cerna. Ternyata hanya ada tiga parameter, tapi agak sulit
dilihat:

``` {.c}
//                      op             x      y
//              |-----------------|  |---|  |---|
void print_math(int (*op)(int, int), int x, int y)
```

Parameter pertama, `op`, berupa pointer ke fungsi yang menerima dua `int`
sebagai argumen dan mengembalikan `int`. Ini cocok dengan signature
`add()` maupun `mult()`.

Yang kedua dan ketiga, `x` dan `y`, hanyalah parameter `int` biasa.

Pelan-pelan dan sengaja, biarkan matamu menelusuri signature itu sambil
mengenali bagian-bagian yang bekerja. Satu hal yang selalu menonjol bagiku
adalah urutan `(*op)(`, tanda kurung dan asterisk itu. Itulah
tanda bahwa ia pointer ke fungsi.

Terakhir, lompat kembali ke bab _Pointer II_ untuk melihat [contoh
pointer-ke-fungsi memakai `qsort()` bawaan](#qsort-example).

[i[Pointers-->to functions]>]
