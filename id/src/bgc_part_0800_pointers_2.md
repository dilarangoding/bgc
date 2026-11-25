<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Pointer II: Aritmetika {#pointers2}

[i[Pointers-->arithmetic]<]

Saatnya menyelam lebih dalam dengan sejumlah topik pointer baru! Kalau
kamu belum paham betul soal pointer, [lihat dulu bagian pertama panduan
ini tentang topik tersebut](#pointers).

## Aritmetika Pointer

Ternyata pointer bisa dioperasikan secara matematis, terutama penjumlahan
dan pengurangan.

Tapi apa artinya kalau itu dilakukan?

Singkatnya, kalau kamu punya pointer ke suatu tipe, menambahkan satu pada
pointer itu akan memindahkannya ke item bertipe sama yang berada tepat
setelahnya di memori.

**Penting** untuk diingat bahwa saat kita menggeser pointer dan melihat
tempat-tempat berbeda di memori, kita harus selalu memastikan pointer itu
menunjuk ke tempat yang valid sebelum di-dereference. Kalau kita nyasar
entah ke mana lalu mencoba melihat isinya, perilakunya undefined, dan
crash adalah hasil yang umum.

Ini agak seperti masalah ayam-dan-telur dengan [Kesetaraan Array/Pointer,
di bawah](#arraypointerequiv), tapi kita coba saja.

### Menambah Pointer

Pertama, kita ambil sebuah array berisi angka.

``` {.c}
int a[5] = {11, 22, 33, 44, 55};
```

Lalu kita ambil pointer ke elemen pertama array itu:

``` {.c}
int a[5] = {11, 22, 33, 44, 55};

int *p = &a[0];  // Atau "int *p = a;" sama saja hasilnya
```

Lalu kita cetak nilai di sana dengan men-dereference pointer-nya:

``` {.c}
printf("%d\n", *p);  // Mencetak 11
```

Sekarang kita pakai aritmetika pointer untuk mencetak elemen berikutnya di
array, yaitu yang berindeks 1:

``` {.c}
printf("%d\n", *(p + 1));  // Mencetak 22!!
```

Apa yang terjadi di situ? C tahu bahwa `p` adalah pointer ke `int`. Jadi ia
tahu `sizeof` sebuah `int`^[Ingat, operator `sizeof` memberi tahu ukuran
sebuah object di memori dalam byte.] dan tahu harus melompati sebanyak itu
byte untuk sampai ke `int` setelah yang pertama!

Bahkan, contoh sebelumnya bisa ditulis dengan dua cara yang setara:

``` {.c}
printf("%d\n", *p);        // Mencetak 11
printf("%d\n", *(p + 0));  // Mencetak 11
```

karena menambahkan `0` pada pointer menghasilkan pointer yang sama.

Mari kita pikirkan manfaatnya. Kita bisa menelusuri elemen array dengan
cara ini, tanpa memakai array:

``` {.c}
int a[5] = {11, 22, 33, 44, 55};

int *p = &a[0];  // Atau "int *p = a;" sama saja hasilnya

for (int i = 0; i < 5; i++) {
    printf("%d\n", *(p + i));  // Sama dengan p[i]!
}
```

Dan hasilnya sama seperti kalau kita memakai notasi array! Oooo! Makin
dekat ke soal kesetaraan array/pointer itu! Nanti dibahas lebih lanjut di
bab ini.

Tapi sebenarnya apa yang terjadi di sini? Bagaimana cara kerjanya?

Ingat dari awal tadi bahwa memori itu seperti array besar, tempat sebuah
byte disimpan di setiap indeks array?

Dan indeks array ke dalam memori itu punya beberapa nama:

* Indeks ke dalam memori
* Lokasi
* Alamat
* _Pointer!_

Jadi pointer adalah indeks ke dalam memori, di suatu tempat.

Sebagai contoh acak, katakanlah angka 3490 disimpan di alamat ("indeks")
23.237.489.202. Kalau kita punya pointer `int` ke 3490 itu, nilai pointer
itu adalah 23.237.489.202... karena pointer adalah alamat memori. Kata
berbeda untuk hal yang sama.

Sekarang katakanlah ada angka lain, 4096, yang disimpan tepat setelah 3490
di alamat 23.237.489.210 (lebih tinggi 8 dari 3490 karena setiap `int`
dalam contoh ini berukuran 8 byte).

Kalau kita tambahkan `1` pada pointer itu, ia sebenarnya melompat maju
sejauh `sizeof(int)` byte ke `int` berikutnya. Ia tahu harus melompat
sejauh itu karena ia pointer `int`. Kalau ia pointer `float`, ia akan
melompat sejauh `sizeof(float)` byte untuk sampai ke float berikutnya!

Jadi kamu bisa melihat `int` berikutnya dengan menambahkan `1` pada
pointer, yang sesudahnya dengan menambahkan `2`, dan seterusnya.

### Mengubah Pointer

Tadi kita sudah melihat bahwa sebuah integer bisa ditambahkan ke pointer.
Sekarang, mari kita _ubah pointer itu sendiri_.

Nilai integer bisa langsung ditambahkan ke (atau dikurangkan dari) pointer
mana pun!

Kita ulangi contoh tadi dengan beberapa perubahan. Pertama, kita tambahkan
`999` di akhir deretan angka sebagai sentinel value. Ini memberi tahu kita
di mana data berakhir.

``` {.c}
int a[] = {11, 22, 33, 44, 55, 999};  // Tambahkan 999 di sini sebagai sentinel

int *p = &a[0];  // p menunjuk ke 11
```

Dan `p` menunjuk ke elemen berindeks `0` dari `a`, yaitu `11`, persis
seperti tadi.

Sekarang---kita mulai _meng-increment_ `p` supaya menunjuk ke
elemen-elemen array berikutnya. Kita lakukan ini sampai `p` menunjuk ke
`999`; artinya, sampai `*p == 999`:

``` {.c}
while (*p != 999) {       // Selama yang ditunjuk p bukan 999
    printf("%d\n", *p);   // Cetak
    p++;                  // Geser p supaya menunjuk ke int berikutnya!
}
```

Gila, kan?

Kalau dijalankan, mula-mula `p` menunjuk ke `11`. Lalu `p` di-increment,
dan ia menunjuk ke `22`, lalu sekali lagi, ia menunjuk ke `33`. Begitu
seterusnya, sampai ia menunjuk ke `999` dan kita berhenti.

### Mengurangi Pointer

[i[Pointers-->subtracting]<]
Kamu bisa mengurangi pointer dengan sebuah nilai untuk sampai ke alamat
yang lebih awal, sama seperti tadi kita menambahkannya.

Tapi kita juga bisa mengurangkan dua pointer untuk mengetahui selisihnya,
misalnya untuk menghitung berapa banyak `int` yang ada di antara dua
`int*`. Syaratnya, ini hanya berlaku di dalam satu array^[Atau string, yang
sebenarnya array berisi `char`. Agak aneh, kamu juga boleh punya pointer
yang merujuk ke _satu elemen setelah_ ujung array tanpa masalah dan tetap
bisa menghitungnya. Hanya saja, pointer itu tidak boleh di-dereference
selagi berada di sana.]---kalau pointer-nya menunjuk ke hal lain, hasilnya
undefined behavior.

Ingat bahwa string di C adalah `char*`? Mari kita lihat apakah ini bisa
dipakai untuk menulis varian lain dari `strlen()` untuk menghitung panjang
string dengan pengurangan pointer.

Idenya: kalau kita punya pointer ke awal string, kita bisa mencari pointer
ke akhir string dengan menelusuri maju sampai menemukan karakter `NUL`.

Dan kalau kita punya pointer ke awal string, serta sudah menghitung
pointer ke akhir string, kita tinggal mengurangkan kedua pointer itu untuk
mendapatkan panjangnya!

``` {.c .numberLines}
#include <stdio.h>

int my_strlen(char *s)
{
    // Mulai menelusuri dari awal string
    char *p = s;

    // Telusuri sampai menemukan karakter NUL
    while (*p != '\0')
        p++;

    // Kembalikan selisih pointer
    return p - s;
}

int main(void)
{
    printf("%d\n", my_strlen("Hello, world!"));  // Mencetak "13"
}
```

Ingat, pengurangan pointer hanya boleh dilakukan antara dua pointer yang
menunjuk ke array yang sama!
[i[Pointers-->subtracting]>]

## Kesetaraan Array/Pointer {#arraypointerequiv}

[i[Pointers-->array equivalence]<]
Akhirnya kita siap membahas ini! Kita sudah melihat banyak contoh tempat
kita mencampur notasi array, tapi sekarang kita sampaikan _rumus dasar
kesetaraan array/pointer_:

``` {.c}
a[b] == *(a + b)
```

Pelajari itu! Keduanya setara dan bisa dipakai bergantian!

Ini sedikit disederhanakan, karena pada contoh di atas `a` dan `b`
sama-sama bisa berupa ekspresi, dan mungkin kita perlu beberapa tanda
kurung tambahan untuk memaksa urutan operasi kalau ekspresinya rumit.

Spesifikasi, seperti biasa, tegas soal ini, menyatakan (di C11
§6.5.2.1¶2):

> `E1[E2]` is identical to `(*((E1)+(E2)))`

tapi itu agak lebih sulit dicerna. Pokoknya pastikan kamu menambahkan
tanda kurung kalau ekspresinya rumit, supaya semua perhitunganmu terjadi
dalam urutan yang benar.

Artinya, kita bisa _memutuskan_ mau memakai notasi array atau notasi
pointer untuk array atau pointer apa pun (dengan asumsi pointer-nya
menunjuk ke elemen sebuah array).

Mari kita pakai sebuah array dan pointer dengan notasi array maupun
notasi pointer:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a[] = {11, 22, 33, 44, 55};

    int *p = a;  // p menunjuk ke elemen pertama a, yaitu 11

    // Cetak semua elemen array dengan berbagai cara:

    for (int i = 0; i < 5; i++)
        printf("%d\n", a[i]);      // Notasi array dengan a

    for (int i = 0; i < 5; i++)
        printf("%d\n", p[i]);      // Notasi array dengan p

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(a + i));  // Notasi pointer dengan a

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(p + i));  // Notasi pointer dengan p

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(p++));    // Menggeser pointer p
        //printf("%d\n", *(a++));    // Menggeser variabel array a--ERROR!
}
```

Jadi kamu bisa lihat bahwa pada umumnya, kalau kamu punya variabel array,
kamu bisa memakai notasi pointer maupun notasi array untuk mengakses
elemennya. Begitu juga dengan variabel pointer.

Satu perbedaan besarnya: sebuah pointer bisa _diubah_ agar menunjuk ke
alamat lain, tapi variabel array tidak bisa. <!--
6.3.2.1p2 --> Dengan kata lain, kamu sama sekali tidak bisa menugaskan
nilai ke variabel array---hanya ke elemen-elemen individual array itu.

Kalau kamu benar-benar ingin menyalin satu array ke array lain, kamu harus
memakai fungsi seperti `memcpy()` (atau sebuah loop).

### Kesetaraan Array/Pointer pada Pemanggilan Fungsi

Di sinilah konsep ini paling sering kamu temui, pasti.

Kalau ada fungsi yang menerima argumen pointer, misalnya:

``` {.c}
int my_strlen(char *s)
```

artinya kamu bisa mengoper array maupun pointer ke fungsi ini, dan
keduanya berfungsi!

``` {.c}
char s[] = "Antelopes";
char *t = "Wombats";

printf("%d\n", my_strlen(s));  // Berfungsi!
printf("%d\n", my_strlen(t));  // Berfungsi juga!
```

Dan itu juga alasan kenapa dua signature fungsi ini setara:

``` {.c}
int my_strlen(char *s)    // Berfungsi!
int my_strlen(char s[])   // Berfungsi juga!
```
[i[Pointers-->array equivalence]>]

## Pointer `void`

[i[`void*` void pointer]<]

Kamu sudah melihat kata kunci `void` dipakai pada fungsi untuk menandakan
tidak ada parameter atau tidak ada nilai kembalian, tapi ini makhluk yang
sama sekali berbeda dan tidak berhubungan.

`void*` jelas adalah pointer ke sebuah *benda* yang ada. Tapi bagian
`void`-nya menandakan bahwa kita tidak tahu _tipe_ benda itu. Dan kadang,
percaya atau tidak, itu sangat berguna. Dengannya kita bisa menulis kode
yang sedikit lebih agnostik terhadap tipe, fleksibilitas yang menyenangkan
di bahasa bertipe seperti C.

Pada dasarnya ada dua kasus pemakaian---mari kita lihat dan coba
mengurangi misterinya.

[i[`memcpy()` function]<]

1. Sebuah fungsi akan bekerja pada sesuatu byte demi byte. Misalnya,
   `memcpy()` menyalin byte memori dari satu pointer ke pointer lain,
   tapi pointer-pointer itu bisa menunjuk ke tipe apa pun. `memcpy()`
   memanfaatkan kenyataan bahwa kalau kita menelusuri `char*`, kita
   menelusuri byte-byte sebuah object apa pun tipenya. Pembahasan lebih
   lanjut ada di subbagian [Nilai Multibyte](#multibyte-values).

2. Fungsi lain memanggil fungsi yang kamu oper kepadanya (sebuah
   callback), dan ia mengoper data kepadamu. Kamu tahu tipe datanya,
   tapi fungsi yang memanggilmu tidak. Jadi ia mengoper `void*`---karena
   tidak tahu tipenya---dan kamu mengonversinya ke tipe yang kamu
   butuhkan. Fungsi bawaan
   [fl[`qsort()`|https://beej.us/guide/bgclr/html/split/stdlib.html#man-qsort]]
   dan
   [fl[`bsearch()`|https://beej.us/guide/bgclr/html/split/stdlib.html#man-bsearch]]
   memakai teknik ini.

Mari kita lihat contohnya, fungsi bawaan `memcpy()`:

``` {.c}
void *memcpy(void *s1, void *s2, size_t n);
```

Fungsi ini menyalin `n` byte memori mulai dari alamat `s2` ke memori yang
dimulai di alamat `s1`.

Tapi lihat! `s1` dan `s2` adalah `void*`! Kenapa? Apa artinya? Mari kita
coba contoh-contoh lain.

Misalnya, kita bisa menyalin string dengan `memcpy()` (walaupun `strcpy()`
lebih pantas untuk string):

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[] = "Goats!";
    char t[100];

    memcpy(t, s, 7);  // Salin 7 byte--termasuk terminator NUL!

    printf("%s\n", t);  // "Goats!"
}
```

Atau kita bisa menyalin beberapa `int`:

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    int a[] = {11, 22, 33};
    int b[3];

    memcpy(b, a, 3 * sizeof(int));  // Salin data 3 int

    printf("%d\n", b[1]);  // 22
}
```

Yang ini agak liar---kamu lihat apa yang kita lakukan dengan `memcpy()`
di situ? Kita menyalin data dari `a` ke `b`, tapi kita harus menentukan
berapa _byte_ yang disalin, dan sebuah `int` lebih dari satu byte.

Oke, kalau begitu---berapa byte yang dipakai sebuah `int`? Jawabannya:
tergantung sistem. Tapi kita bisa mengetahui berapa byte yang dipakai
tipe apa pun dengan operator `sizeof`.

Jadi inilah jawabannya: sebuah `int` memakai `sizeof(int)` byte memori
untuk menyimpannya.

Dan kalau ada 3 `int` di array kita, seperti pada contoh tadi, seluruh
ruang yang dipakai 3 `int` itu pasti `3 * sizeof(int)`.

(Pada contoh string tadi, yang lebih akurat secara teknis adalah menyalin
`7 * sizeof(char)` byte. Tapi `char` selalu berukuran satu byte menurut
definisi, jadi itu hanya menyusut menjadi `7 * 1`.)

Kita bahkan bisa menyalin sebuah `float` atau `struct` dengan `memcpy()`!
(Walaupun ini keterlaluan---untuk itu sebaiknya pakai `=` saja):

``` {.c}
struct antelope my_antelope;
struct antelope my_clone_antelope;

// ...

memcpy(&my_clone_antelope, &my_antelope, sizeof my_antelope);
```

Lihat betapa serbaguna `memcpy()`! Kalau kamu punya pointer ke sumber dan
pointer ke tujuan, serta jumlah byte yang ingin disalin, kamu bisa
menyalin _data bertipe apa pun_.

Bayangkan kalau tidak ada `void*`. Kita harus menulis fungsi `memcpy()`
khusus untuk setiap tipe:

[i[`memcpy()` function]>]

``` {.c}
memcpy_int(int *a, int *b, int count);
memcpy_float(float *a, float *b, int count);
memcpy_double(double *a, double *b, int count);
memcpy_char(char *a, char *b, int count);
memcpy_unsigned_char(unsigned char *a, unsigned char *b, int count);

// dan seterusnya... iyuh!
```

Jauh lebih baik memakai `void*` dan punya satu fungsi yang bisa
mengerjakan semuanya.

Itulah kekuatan `void*`. Kamu bisa menulis fungsi yang tidak peduli tipe
variabelnya, tapi tetap bisa melakukan sesuatu dengannya.

Tapi dengan kekuatan besar datang tanggung jawab besar. Mungkin tidak
sebesar _itu_ dalam kasus ini, tapi memang ada beberapa batasan.

[i[`void*` void pointer-->caveats]<]

1. Kamu tidak bisa melakukan aritmetika pointer pada `void*`.
2. Kamu tidak bisa men-dereference `void*`.
3. Kamu tidak bisa memakai arrow operator pada `void*`, karena itu juga
   dereference.
4. Kamu tidak bisa memakai notasi array pada `void*`, karena itu juga
   dereference^[Karena ingat, notasi array hanyalah dereference ditambah
   sedikit aritmetika pointer, dan `void*` tidak bisa di-dereference!].

Dan kalau dipikir-pikir, aturan-aturan ini masuk akal. Semua operasi itu
bergantung pada pengetahuan tentang `sizeof` tipe data yang ditunjuk, dan
pada `void*`, kita tidak tahu ukuran data yang ditunjuk---bisa apa saja!

[i[`void*` void pointer-->caveats]>]

Tapi tunggu---kalau `void*` tidak bisa di-dereference, apa gunanya?

Seperti pada `memcpy()`, ia membantu menulis fungsi generik yang bisa
menangani berbagai tipe data. Tapi rahasianya: di dalamnya, _kamu
mengonversi `void*` ke tipe lain sebelum memakainya_!

Dan konversinya mudah: cukup tugaskan ke variabel bertipe yang
diinginkan^[Kamu juga bisa melakukan _cast_ pada `void*` ke tipe lain,
tapi kita belum sampai ke cast.].

``` {.c}
char a = 'X';  // Satu char

void *p = &a;  // p menunjuk ke 'X'
char *q = p;   // q juga menunjuk ke 'X'

printf("%c\n", *p);  // ERROR--tidak bisa men-dereference void*!
printf("%c\n", *q);  // Mencetak "X"
```

[i[`memcpy()` function]<]
Mari kita tulis `memcpy()` sendiri untuk mencobanya. Kita bisa menyalin
byte (`char`), dan kita tahu jumlah byte-nya karena dioper sebagai
argumen.

``` {.c}
void *my_memcpy(void *dest, void *src, int byte_count)
{
    // Ubah void* menjadi char*
    char *s = src, *d = dest;

    // Setelah punya char*, kita bisa men-dereference dan menyalinnya
    while (byte_count--) {
        *d++ = *s++;
    }

    // Kebanyakan fungsi seperti ini mengembalikan tujuan, siapa tahu
    // berguna bagi caller.
    return dest;
}
```

Tepat di awal, kita menyalin `void*` ke `char*` supaya bisa dipakai sebagai
`char*`. Semudah itu.

Lalu ada keseruan di loop while, tempat kita men-decrement `byte_count`
sampai menjadi false (`0`). Ingat bahwa pada post-decrement, nilai
ekspresi dihitung lebih dulu (untuk dipakai `while`) dan _baru kemudian_
variabelnya di-decrement.

Dan ada keseruan di penyalinan, tempat kita menugaskan `*d = *s` untuk
menyalin byte, tapi memakai post-increment sehingga `d` dan `s`
sama-sama bergeser ke byte berikutnya setelah penugasan dilakukan.

Terakhir, kebanyakan fungsi memori dan string mengembalikan salinan
pointer ke tujuan, siapa tahu caller ingin memakainya.

Setelah semua itu, penulis ingin menunjukkan sekilas bahwa kita bisa
memakai teknik ini untuk menelusuri byte-byte dari object _apa pun_ di C,
`float`, `struct`, atau apa saja!
[i[`memcpy()` function]>]

[i[`qsort()` function]<]
[Mari kita]{#qsort-example} coba satu contoh dunia nyata lagi dengan
rutin bawaan `qsort()` yang bisa mengurutkan _apa saja_ berkat keajaiban
`void*`.

(Pada contoh berikut, abaikan saja kata `const`, yang belum kita bahas.)

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

// Tipe struktur yang akan kita urutkan
struct animal {
    char *name;
    int leg_count;
};

// Ini fungsi pembanding yang dipanggil oleh qsort() untuk membantunya
// menentukan apa persisnya yang dijadikan dasar pengurutan. Kita pakai
// untuk mengurutkan array struct animal berdasarkan leg_count.
int compar(const void *elem1, const void *elem2)
{
    // Kita tahu yang diurutkan adalah struct animal, jadi mari kita
    // jadikan kedua argumen pointer ke struct animal
    const struct animal *animal1 = elem1;
    const struct animal *animal2 = elem2;

    // Kembalikan <0, =0, atau >0 tergantung dasar pengurutan yang kita mau.

    // Kita urutkan menaik berdasarkan leg_count, jadi kita kembalikan
    // selisih leg_count
    if (animal1->leg_count > animal2->leg_count)
        return 1;
    
    if (animal1->leg_count < animal2->leg_count)
        return -1;

    return 0;
}

int main(void)
{
    // Kita buat array berisi 4 struct animal dengan karakteristik
    // berbeda. Array ini tidak berurutan menurut leg_count, tapi
    // sebentar lagi akan kita urutkan.
    struct animal a[4] = {
        {.name="Dog", .leg_count=4},
        {.name="Monkey", .leg_count=2},
        {.name="Antelope", .leg_count=4},
        {.name="Snake", .leg_count=0}
    };

    // Panggil qsort() untuk mengurutkan array. qsort() perlu diberi tahu
    // persis berdasarkan apa data ini diurutkan, dan itu kita lakukan
    // di dalam fungsi compar().
    //
    // Pemanggilan ini berarti: qsort array a, yang punya 4 elemen, dan
    // setiap elemen berukuran sizeof(struct animal) byte, dan ini
    // fungsi yang akan membandingkan dua elemen mana pun.
    qsort(a, 4, sizeof(struct animal), compar);

    // Cetak semuanya
    for (int i = 0; i < 4; i++) {
        printf("%d: %s\n", a[i].leg_count, a[i].name);
    }
}
```

Selama kamu memberi `qsort()` fungsi yang bisa membandingkan dua item
dalam array yang akan diurutkan, ia bisa mengurutkan apa saja. Dan itu
dilakukan tanpa perlu menanamkan tipe item di mana pun. `qsort()` hanya
menata ulang blok-blok byte berdasarkan hasil fungsi `compar()` yang kamu
oper.
[i[`qsort()` function]>]
[i[`void*` void pointer]>]
[i[Pointers-->arithmetic]>]
