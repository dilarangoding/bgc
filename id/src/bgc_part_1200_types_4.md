<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe IV: Qualifier dan Specifier

Sekarang setelah kita punya lebih banyak tipe, ternyata tipe-tipe ini bisa
diberi atribut tambahan yang mengatur perilakunya. Inilah _type qualifier_
dan _storage-class specifier_.

## Type Qualifier

[i[Type qualifiers]<]

Dengan ini kamu bisa mendeklarasikan nilai konstan, dan juga memberi
petunjuk optimasi kepada compiler yang bisa dimanfaatkannya.

### `const`

[i[`const` type qualifier]<]

Ini type qualifier yang paling umum kamu temui. Artinya variabel itu
konstan, dan setiap usaha mengubahnya akan membuat compiler sangat marah.

``` {.c}
const int x = 2;

x = 4;  // SUARA COMPILER MUNTAH, tidak bisa menugaskan ke konstanta
```

Nilai `const` tidak bisa diubah.

Sering kali kamu melihat `const` di daftar parameter fungsi:

``` {.c}
void foo(const int x)
{
    printf("%d\n", x + 30);  // OK, tidak mengubah "x"
}
```

#### `const` dan Pointer

[i[`const` type qualifier-->and pointers]<]

Ini agak membingungkan, karena ada dua pemakaian yang punya dua makna
ketika menyangkut pointer.

Pertama, kita bisa membuat agar nilai yang ditunjuk pointer tidak bisa
diubah. Caranya dengan menaruh `const` di depan nama tipe (sebelum
asterisk) pada deklarasi tipe.

``` {.c}
int x[] = {10, 20};
const int *p = x; 

p++;  // Kita bisa mengubah p, tidak masalah

*p = 30; // Error compiler! Tidak bisa mengubah yang ditunjuknya
```

Agak membingungkan, dua hal ini setara:

``` {.c}
const int *p;  // Tidak bisa mengubah yang ditunjuk p
int const *p;  // Tidak bisa mengubah yang ditunjuk p, sama seperti di atas
```

Bagus, jadi kita tidak bisa mengubah yang ditunjuk pointer, tapi kita bisa
mengubah pointer-nya sendiri. Bagaimana kalau kita ingin sebaliknya? Kita
ingin bisa mengubah yang ditunjuk pointer, tapi _tidak_ bisa mengubah
pointer-nya sendiri?

Cukup pindahkan `const` ke setelah asterisk pada deklarasi:

``` {.c}
int *const p;   // Kita tidak bisa mengubah "p" dengan aritmetika pointer

p++;  // Error compiler!
```

Tapi kita bisa mengubah yang ditunjuknya:

``` {.c}
int x = 10;
int *const p = &x;

*p = 20;   // Ubah "x" menjadi 20, tidak masalah
```

Kamu juga bisa membuat keduanya `const`:

``` {.c}
const int *const p;  // Tidak bisa mengubah p maupun *p!
```

Terakhir, kalau ada beberapa tingkat indirection, kamu harus memberi
`const` pada tingkat yang sesuai. Hanya karena sebuah pointer `const`,
bukan berarti pointer yang ditunjuknya juga harus `const`. Kamu bisa
mengaturnya secara eksplisit seperti pada contoh-contoh berikut:

``` {.c}
char **p;
p++;     // OK!
(*p)++;  // OK!

char **const p;
p++;     // Error!
(*p)++;  // OK!

char *const *p;
p++;     // OK!
(*p)++;  // Error!

char *const *const p;
p++;     // Error!
(*p)++;  // Error!
```

[i[`const` type qualifier-->and pointers]>]

#### `const` Correctness

[i[`const` type qualifier-->correctness]<]

Ada satu hal lagi yang harus kusebutkan: compiler akan memberi peringatan
untuk kode seperti ini:

``` {.c}
const int x = 20;
int *p = &x;
```

dengan pesan yang kira-kira berbunyi:

``` {.default}
initialization discards 'const' qualifier from pointer type target
```

Apa yang terjadi di sana?

Kita perlu melihat tipe di kedua sisi penugasan:

``` {.c}
    const int x = 20;
    int *p = &x;
//    ^       ^
//    |       |
//  int*    const int*
```

Compiler memperingatkan bahwa nilai di sisi kanan penugasan itu `const`,
tapi yang di sisi kiri tidak. Compiler memberi tahu bahwa ia membuang
sifat "const" dari ekspresi di sisi kanan.

Artinya, kita _masih bisa_ mencoba hal berikut, tapi itu salah. Compiler
akan memperingatkan, dan hasilnya undefined behavior:

``` {.c}
const int x = 20;
int *p = &x;

*p = 40;  // Undefined behavior--mungkin mengubah "x", mungkin tidak!

printf("%d\n", x);  // 40, kalau beruntung
```

[i[`const` type qualifier-->correctness]>]
[i[`const` type qualifier]>]

### `restrict`

[i[`restrict` type qualifier]<]

Singkatnya: kamu tidak pernah wajib memakai ini dan boleh mengabaikannya
setiap kali melihatnya. Kalau dipakai dengan benar, kemungkinan ada sedikit
peningkatan performa. Kalau dipakai dengan salah, kamu akan mendapat
undefined behavior.

`restrict` adalah petunjuk bagi compiler bahwa sepotong memori tertentu
hanya akan diakses oleh satu pointer dan tidak pernah oleh pointer lain.
(Artinya, tidak ada aliasing pada object yang ditunjuk pointer
`restrict` itu.) Kalau developer mendeklarasikan pointer sebagai
`restrict` lalu mengakses object yang ditunjuknya dengan cara lain (misalnya
lewat pointer lain), perilakunya undefined.

Pada dasarnya kamu berkata kepada C, "Hei---kujamin pointer tunggal ini
satu-satunya cara aku mengakses memori ini, dan kalau aku bohong, silakan
beri aku undefined behavior."

Dengan informasi itu, C bisa mengoptimasi hal-hal tertentu. Misalnya,
kalau kamu men-dereference pointer `restrict` berulang kali dalam loop, C
mungkin memutuskan menyimpan hasilnya di register dan hanya menyimpan
hasil akhirnya setelah loop selesai. Kalau ada pointer lain yang merujuk
memori yang sama dan mengaksesnya di dalam loop, hasilnya tidak akan
akurat.

(Perhatikan bahwa `restrict` tidak berpengaruh kalau object yang ditunjuk
tidak pernah ditulis. Semuanya tentang optimasi seputar penulisan ke
memori.)

Mari kita tulis fungsi untuk menukar dua variabel, dan kita pakai kata
kunci `restrict` untuk meyakinkan C bahwa kita tidak akan pernah mengoper
pointer ke hal yang sama. Lalu mari kita sengaja membuat kesalahan dan mencoba mengoper pointer
ke hal yang sama.

``` {.c .numberLines}
void swap(int *restrict a, int *restrict b)
{
    int t;

    t = *a;
    *a = *b;
    *b = t;
}

int main(void)
{
    int x = 10, y = 20;

    swap(&x, &y);  // OK! "a" dan "b" di atas menunjuk ke hal yang berbeda

    swap(&x, &x);  // Undefined behavior! "a" dan "b" sama!
}
```

Kalau kata kunci `restrict` di atas dihapus, kedua pemanggilan akan aman.
Tapi compiler mungkin tidak bisa mengoptimasi.

`restrict` punya block scope, artinya pembatasannya hanya berlaku selama
scope tempat ia dipakai. Kalau ada di daftar parameter fungsi, ia ada di
block scope fungsi itu.

Kalau pointer yang dibatasi menunjuk ke array, pembatasan itu hanya berlaku
pada object individual di array. Pointer lain boleh membaca dan menulis
array itu selama tidak membaca atau menulis elemen yang sama dengan yang
dibatasi.

Kalau `restrict` dipakai di file scope, di luar fungsi apa pun, pembatasannya
mencakup seluruh program.

Kamu kemungkinan besar akan melihatnya di fungsi library seperti `printf()`:

``` {.c}
int printf(const char * restrict format, ...);
```

Sekali lagi, itu hanya memberi tahu compiler bahwa di dalam fungsi
`printf()`, hanya akan ada satu pointer yang merujuk ke bagian mana pun
dari string `format` itu.

Satu catatan terakhir: kalau karena suatu alasan kamu memakai notasi array
pada parameter fungsi alih-alih notasi pointer, kamu bisa memakai
`restrict` seperti ini:

``` {.c}
void foo(int p[restrict])     // Tanpa ukuran

void foo(int p[restrict 10])  // Atau dengan ukuran
```

Tapi notasi pointer lebih umum.

[i[`restrict` type qualifier]>]

### `volatile`

[i[`volatile` type qualifier]<]

Kecil kemungkinan kamu melihat atau membutuhkan ini kecuali kamu
berurusan langsung dengan hardware.

`volatile` memberi tahu compiler bahwa sebuah nilai mungkin berubah di
luar pengetahuannya dan harus dibaca ulang setiap kali.

Contohnya ketika compiler membaca memori di sebuah alamat yang terus
diperbarui di balik layar, misalnya semacam timer hardware.

Kalau compiler memutuskan mengoptimasinya dan menyimpan nilai di register
dalam waktu lama, nilai di memori akan berubah dan tidak tercermin di
register.

Dengan mendeklarasikan sesuatu `volatile`, kamu berkata kepada compiler,
"Hei, yang ditunjuk ini mungkin berubah kapan saja karena alasan di luar
kode program ini."

``` {.c}
volatile int *p;
```

### `_Atomic`

Ini fitur C opsional yang akan kita bahas di [bab Atomics](#chapter-atomics).

[i[`volatile` type qualifier]>]
[i[Type qualifiers]>]

## Storage-Class Specifier

[i[Storage-Class Specifiers]<]

Storage-class specifier mirip type qualifier. Mereka memberi compiler
informasi tambahan tentang tipe sebuah variabel.

### `auto`

[i[`auto` storage class]<]

Kata kunci ini hampir tidak pernah terlihat, karena `auto` adalah bawaan
untuk variabel block scope. Ia tersirat.

Keduanya sama:

``` {.c}
{
    int a;         // auto adalah bawaannya...
    auto int a;    // Jadi ini mubazir
}
```

Kata kunci `auto` menandakan bahwa object ini punya _automatic storage
duration_. Artinya, ia ada di scope tempat ia didefinisikan, dan otomatis
didealokasikan saat scope itu ditinggalkan.

Satu jebakan pada variabel otomatis: nilainya tak tentu sampai kamu
menginisialisasinya secara eksplisit. Kita bilang isinya "acak" atau
"sampah", walau aku sendiri kurang puas dengan keduanya. Pokoknya,
kamu tidak akan tahu isinya kecuali kamu menginisialisasinya.

[i[`auto` storage class]>]

Selalu inisialisasi semua variabel otomatis sebelum dipakai!

### `static` {#static}

[i[`static` storage class]<]

Kata kunci ini punya dua makna, tergantung apakah variabelnya file scope
atau block scope.

Kita mulai dengan block scope.

#### `static` di Block Scope

[i[`static` storage class-->in block scope]<]

Dalam kasus ini, intinya kita berkata, "Aku cuma ingin satu instance
variabel ini ada, dipakai bersama antarpemanggilan."

Artinya, nilainya bertahan di antara pemanggilan.

`static` di block scope dengan initializer hanya diinisialisasi satu kali
saat program mulai, bukan setiap kali fungsi dipanggil.

Kita coba contoh:

``` {.c .numberLines}
#include <stdio.h>

void counter(void)
{
    static int count = 1;  // Ini diinisialisasi satu kali

    printf("This has been called %d time(s)\n", count);

    count++;
}

int main(void)
{
    counter();  // "This has been called 1 time(s)"
    counter();  // "This has been called 2 time(s)"
    counter();  // "This has been called 3 time(s)"
    counter();  // "This has been called 4 time(s)"
}
```

Lihat bagaimana nilai `count` bertahan di antara pemanggilan?

Satu hal yang perlu dicatat: variabel `static` block scope diinisialisasi
ke `0` secara bawaan.

``` {.c}
static int foo;      // Nilai awal bawaannya `0`...
static int foo = 0;  // Jadi penugasan `0` ini mubazir
```

Terakhir, perlu diingat bahwa kalau kamu menulis program multithread,
kamu harus memastikan beberapa thread tidak saling menginjak variabel yang
sama.

[i[`static` storage class-->in block scope]>]

#### `static` di File Scope

[i[`static` storage class-->in file scope]<]

Ketika kita sampai di file scope, di luar blok mana pun, maknanya agak
berubah.

Variabel di file scope sudah bertahan di antara pemanggilan fungsi, jadi
perilaku itu sudah ada.

Sebagai gantinya, dalam konteks ini `static` berarti variabel tersebut tidak
terlihat di luar file sumber ini. Mirip "global", tapi hanya di
file ini.

Lebih lanjut dibahas di bagian tentang membangun dengan banyak file
sumber.

[i[`static` storage class-->in file scope]>]
[i[`static` storage class]>]

### `extern` {#extern}

[i[`extern` storage class]<]

Storage-class specifier `extern` memberi kita cara untuk merujuk ke object
di file sumber lain.

Katakanlah, misalnya, file `bar.c` isinya hanya ini:

``` {.c .numberLines}
// bar.c

int a = 37;
```

Hanya itu. Mendeklarasikan `int a` baru di file scope.

Tapi bagaimana kalau ada file sumber lain, `foo.c`, dan kita ingin merujuk
ke `a` yang ada di `bar.c`?

Mudah dengan kata kunci `extern`:

``` {.c .numberLines}
// foo.c

extern int a;

int main(void)
{
    printf("%d\n", a);  // 37, dari bar.c!

    a = 99;

    printf("%d\n", a);  // "a" yang sama dari bar.c, tapi sekarang 99
}
```

Kita juga bisa menaruh `extern int a` di block scope, dan ia tetap akan
merujuk ke `a` di `bar.c`:

``` {.c .numberLines}
// foo.c

int main(void)
{
    extern int a;

    printf("%d\n", a);  // 37, dari bar.c!

    a = 99;

    printf("%d\n", a);  // "a" yang sama dari bar.c, tapi sekarang 99
}
```

Nah, kalau `a` di `bar.c` ditandai `static`, ini tidak akan berfungsi.
Variabel `static` di file scope tidak terlihat di luar file itu.

Catatan terakhir tentang `extern` pada fungsi. Untuk fungsi, `extern`
adalah bawaan, jadi mubazir. Kamu bisa mendeklarasikan fungsi `static`
kalau hanya ingin fungsi itu terlihat di satu file sumber.

[i[`extern` storage class]>]

### `register`

[i[`register` storage class]<]

Ini kata kunci untuk memberi petunjuk ke compiler bahwa variabel ini
sering dipakai, dan harus dibuat secepat mungkin diaksesnya. Compiler tidak
wajib menyetujuinya.

Sekarang, optimizer compiler C modern cukup pandai menentukan hal ini
sendiri, jadi jarang terlihat belakangan ini. Ada beberapa argumen untuk
memakainya, yang kucantumkan di bawah. Meski begitu, jujur saja, aku
belum pernah memakainya di kode C mana pun yang pernah kutulis.

Tapi kalau memang harus:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    register int a;   // Buat "a" secepat mungkin dipakai.

    for (a = 0; a < 10; a++)
        printf("%d\n", a);
}
```

Tapi ada harganya. Kamu tidak bisa mengambil alamat sebuah register:

``` {.c}
register int a;
int *p = &a;    // ERROR COMPILER! Tidak bisa mengambil alamat register
```

Hal yang sama berlaku untuk bagian mana pun dari array, karena
memperlakukan array `register` sebagai pointer adalah undefined behavior.
Kamu boleh mengambil `sizeof()`-nya. Tapi itu bukan keuntungan. Artinya
array `register` pada dasarnya tidak berguna.

GCC dan Clang sama-sama error pada semua yang berikut (kecuali GCC, anehnya,
tidak error pada `a[2]`; ia hanya memberi peringatan kalau kamu memakai
`-pedantic`):

``` {.c}
register int a[] = {11, 22, 33, 44, 55};
int *p = a;       // Undefined behavior!
int b = *(a + 2); // Undefined behavior!
int c = a[2];     // Undefined behavior!
```

Fakta bahwa alamat variabel register tidak bisa diambil membebaskan
compiler untuk melakukan optimasi berdasarkan asumsi itu kalau belum
menemukannya sendiri. Selain itu, menambahkan `register` pada variabel
`const` mencegah seseorang tanpa sengaja mengoper pointer-nya ke fungsi
lain yang dengan sengaja mengabaikan sifat
konstannya^[https://gustedt.wordpress.com/2010/08/17/a-common-misconsception-the-register-keyword/].

Sedikit latar belakang sejarah: jauh di dalam CPU ada "variabel" kecil
khusus bernama [flw[_register_|Processor_register]]. Aksesnya sangat cepat
dibanding RAM, jadi memakainya memberi dorongan kecepatan. Tapi mereka
tidak ada di RAM, jadi tidak punya alamat memori (itulah kenapa kamu tidak
bisa mengambil address-of atau pointer ke mereka).

Tapi, seperti kubilang tadi, compiler modern sangat pandai menghasilkan
kode optimal, memakai register kapan pun mungkin terlepas dari apakah kamu
menulis kata kunci `register` atau tidak. Bukan hanya itu, spesifikasi
membolehkan mereka memperlakukannya seolah-olah kamu menulis `auto`, kalau
mereka mau. Jadi tidak ada jaminan.

[i[`register` storage class]>]

### `_Thread_local`

[i[`_Thread_local` storage class]<]

Saat memakai banyak thread dan ada variabel di global atau block scope
`static`, ini cara untuk memastikan setiap thread mendapat salinannya
sendiri dari variabel itu. Ini membantu menghindari race condition dan
thread yang saling menginjak kaki.

Kalau berada di block scope, kamu harus memakainya bersama `extern` atau
`static`.

Selain itu, kalau kamu menyertakan `<threads.h>`, kamu bisa memakai
`thread_local` yang jauh lebih enak dilihat sebagai alias dari
`_Thread_local` yang lebih jelek.

Informasi lebih lanjut ada di [bagian Threads](#thread-local).

[i[`_Thread_local` storage class]>]
[i[Storage-Class Specifiers]>]
