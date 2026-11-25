<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Structs {#structs}

[i[`struct` keyword]<]
Di C ada yang disebut `struct`, yaitu tipe yang bisa didefinisikan sendiri
oleh pengguna dan menyimpan beberapa data sekaligus, yang bisa saja
bertipe berbeda-beda.

Ini cara praktis untuk membungkus beberapa variabel menjadi satu. Dengan
begitu, mengoper variabel ke fungsi jadi lebih ringkas (cukup satu yang
dioper, bukan banyak), data lebih rapi, dan kode lebih mudah dibaca.

Kalau kamu berasal dari bahasa lain, mungkin kamu sudah akrab dengan
konsep _class_ dan _object_. Keduanya tidak ada di C secara
bawaan^[Walaupun di C setiap item di memori seperti `int` disebut
"object", itu bukan object dalam arti pemrograman berorientasi objek.].
Kamu bisa membayangkan `struct` sebagai class yang hanya punya data member,
tanpa method.

## Mendeklarasikan Struct

[i[`struct` keyword-->declaring]<]
Sebuah `struct` bisa dideklarasikan di kodemu seperti ini:

``` {.c}
struct car {
    char *name;
    float price;
    int speed;
};
```

Ini sering dilakukan di scope global, di luar semua fungsi, supaya
`struct` itu tersedia secara global.

Dengan begitu, kamu membuat sebuah _tipe_ baru. Nama lengkap tipenya
adalah `struct car`. (Bukan sekadar `car`---itu tidak akan berfungsi.)

Belum ada variabel bertipe itu, tapi kita bisa mendeklarasikannya:

``` {.c}
struct car saturn;  // Variabel "saturn" bertipe "struct car"
```

Sekarang kita punya variabel `saturn`^[Saturn dulu adalah merek mobil
ekonomi yang populer di Amerika Serikat, sampai akhirnya gulung tikar
akibat krisis 2008, sayangnya bagi kami para penggemarnya.] yang belum
diinisialisasi, bertipe `struct car`.

Sebaiknya kita inisialisasi! Tapi bagaimana cara mengisi nilai
masing-masing field-nya?

Seperti di banyak bahasa lain yang mencurinya dari C, kita memakai
operator titik (`.`) untuk mengakses tiap field.

``` {.c}
saturn.name = "Saturn SL/2";
saturn.price = 15999.99;
saturn.speed = 175;

printf("Name:           %s\n", saturn.name);
printf("Price (USD):    %f\n", saturn.price);
printf("Top Speed (km): %d\n", saturn.speed);
```

Di baris-baris awal, kita mengisi nilai-nilai di `struct car`, lalu di
bagian berikutnya kita mencetak nilai-nilai itu.
[i[`struct` keyword-->declaring]>]

## Struct Initializer {#struct-initializers}

[i[`struct` keyword-->initializers]<]
Contoh di bagian sebelumnya agak merepotkan. Pasti ada cara yang lebih baik
untuk menginisialisasi variabel `struct` itu!

Kamu bisa memakai initializer dengan mengisi nilai untuk field-field-nya
_sesuai urutan kemunculannya di `struct`_ saat mendefinisikan variabel.
(Ini tidak bisa dilakukan setelah variabelnya didefinisikan---harus
terjadi saat definisi.)

``` {.c}
struct car {
    char *name;
    float price;
    int speed;
};

// Sekarang dengan initializer! Urutan field sama seperti di deklarasi struct:
struct car saturn = {"Saturn SL/2", 16000.99, 175};

printf("Name:      %s\n", saturn.name);
printf("Price:     %f\n", saturn.price);
printf("Top Speed: %d km\n", saturn.speed);
```

Fakta bahwa field di initializer harus berurutan sama itu agak
menyeramkan. Kalau ada yang mengubah urutan di `struct car`, semua kode
lain bisa rusak!

Kita bisa lebih spesifik dengan initializer:

``` {.c}
struct car saturn = {.speed=175, .name="Saturn SL/2"};
```

Sekarang initializer tidak bergantung pada urutan di deklarasi `struct`.
Kode seperti ini jelas lebih aman.

Seperti pada array initializer, field designator yang tidak disebutkan akan
diinisialisasi dengan nol (di sini yaitu `.price`, yang sengaja dihilangkan).
[i[`struct` keyword-->initializers]>]

## Mengoper Struct ke Fungsi

[i[`struct` keyword-->passing and returning]<]
Ada dua cara untuk mengoper `struct` ke fungsi.

1. Oper `struct`-nya.
2. Oper pointer ke `struct`-nya.

Ingat bahwa ketika sesuatu dioper ke fungsi, sebuah _salinan_ dari sesuatu
itu dibuat untuk dikerjakan oleh fungsi, baik itu salinan pointer, `int`,
`struct`, atau apa pun.

Pada dasarnya ada dua keadaan ketika kamu ingin mengoper pointer ke
`struct`:

1. Fungsi perlu bisa mengubah `struct` yang dioper, dan perubahannya harus
   terlihat di caller.
2. `struct`-nya cukup besar sehingga menyalinnya ke stack lebih mahal
   daripada menyalin sebuah pointer^[Pointer kemungkinan berukuran 8 byte
   di sistem 64-bit.].

Karena dua alasan itu, jauh lebih umum mengoper pointer ke `struct` ke
fungsi, walaupun mengoper `struct`-nya langsung sama sekali tidak
dilarang.

Mari kita coba mengoper pointer, dengan membuat fungsi yang memungkinkan
kita mengisi field `.price` dari `struct car`:

``` {.c .numberLines}
#include <stdio.h>

struct car {
    char *name;
    float price;
    int speed;
};

int main(void)
{
    struct car saturn = {.speed=175, .name="Saturn SL/2"};

    // Oper pointer ke struct car ini, beserta harga baru yang
    // lebih realistis:
    set_price(&saturn, 799.99);

    printf("Price: %f\n", saturn.price);
}
```

Kamu seharusnya bisa menyusun signature `set_price()` hanya dengan
melihat tipe argumen yang kita punya di sana.

`saturn` adalah `struct car`, jadi `&saturn` pasti alamat dari `struct
car`, alias pointer ke `struct car`, yaitu `struct car*`.

Dan `799.99` adalah `float`.

Jadi deklarasi fungsinya pasti seperti ini:

``` {.c}
void set_price(struct car *c, float new_price)
```

Tinggal menulis badannya. Satu percobaan mungkin begini:

``` {.c}
void set_price(struct car *c, float new_price) {
    c.price = new_price;  // ERROR!!
}
```

Itu tidak akan berhasil karena operator titik hanya bekerja pada
`struct`... bukan pada _pointer_ ke `struct`.

Oke, jadi kita bisa men-dereference variabel `c` untuk "de-pointer"-nya
agar sampai ke `struct`-nya. Men-dereference `struct car*` menghasilkan
`struct car` yang ditunjuk pointer itu, yang seharusnya bisa kita beri
operator titik:

``` {.c}
void set_price(struct car *c, float new_price) {
    (*c).price = new_price;  // Berfungsi, tapi jelek dan tidak idiomatis :(
}
```

Dan itu berhasil! Tapi mengetik semua tanda kurung dan asterisk itu agak
merepotkan. C punya syntactic sugar bernama _arrow operator_ yang
membantu soal ini.
[i[`struct` keyword-->passing and returning]>]

## Arrow Operator

[i[`->` arrow operator]<]
Arrow operator membantu merujuk ke field pada pointer ke `struct`.

``` {.c}
void set_price(struct car *c, float new_price) {
    // (*c).price = new_price;  // Berfungsi, tapi tidak idiomatis :(
    //
    // Baris di atas 100% setara dengan baris di bawah:

    c->price = new_price;  // Nah, yang ini!
}
```

Jadi saat mengakses field, kapan memakai titik dan kapan memakai panah?

* Kalau kamu punya `struct`, pakai titik (`.`).
* Kalau kamu punya pointer ke `struct`, pakai panah (`->`).
[i[`->` arrow operator]>]

## Menyalin dan Mengembalikan `struct`

[i[`struct` keyword-->copying]<]
Yang ini gampang!

Tinggal tugaskan satu ke yang lain!

``` {.c}
struct car a, b;

b = a;  // Salin struct-nya
```

Mengembalikan `struct` (bukan pointer ke `struct`) dari sebuah fungsi juga
membuat salinan serupa ke variabel penerimanya.

Ini bukan "deep copy"^[_Deep copy_ mengikuti pointer di dalam `struct` dan
ikut menyalin data yang ditunjuknya. _Shallow copy_ hanya menyalin
pointer-nya, bukan hal yang ditunjuk. C tidak punya fungsionalitas deep
copy bawaan.]. Semua field disalin apa adanya, termasuk pointer ke
sesuatu.
[i[`struct` keyword-->copying]>]

## Membandingkan `struct`

[i[`struct` keyword-->comparing]<]
Hanya ada satu cara yang aman: bandingkan setiap field satu per satu.

Mungkin kamu mengira bisa memakai
[fl[`memcmp()`|https://beej.us/guide/bgclr/html/split/stringref.html#man-strcmp]],
tapi itu tidak menangani kemungkinan adanya [padding
byte](#struct-padding-bytes) di dalamnya.

Kalau `struct` dikosongkan dulu dengan nol memakai
[fl[`memset()`|https://beej.us/guide/bgclr/html/split/stringref.html#man-memset]],
hasilnya _mungkin_ berhasil, tapi masih ada elemen aneh yang
[fl[mungkin tidak dibandingkan seperti yang
kamu harapkan|https://stackoverflow.com/questions/141720/how-do-you-compare-structs-for-equality-in-c]].
[i[`struct` keyword-->comparing]>] [i[`struct` keyword]>]
