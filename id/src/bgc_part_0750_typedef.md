<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `typedef`: Membuat Tipe Baru

[i[`typedef` keyword]<]
Sebenarnya bukan membuat tipe _baru_, melainkan memberi nama baru pada
tipe yang sudah ada. Sekilas kedengarannya sia-sia, tapi ini bisa kita
manfaatkan untuk membuat kode lebih bersih.

## `typedef` dalam Teori

Pada dasarnya, kamu mengambil tipe yang sudah ada lalu membuat alias untuk
tipe itu dengan `typedef`.

Seperti ini:

``` {.c}
typedef int antelope;  // Jadikan "antelope" alias untuk "int"

antelope x = 10;       // Tipe "antelope" sama dengan tipe "int"
```

Kamu bisa melakukannya pada tipe apa pun yang sudah ada. Kamu bahkan bisa
membuat beberapa tipe sekaligus dengan daftar yang dipisahkan koma:

``` {.c}
typedef int antelope, bagel, mushroom;  // Semuanya "int"
```

Itu sangat berguna, kan? Bisa mengetik `mushroom` alih-alih `int`? Kamu
pasti _sangat bersemangat_ dengan fitur ini!

Oke, Profesor Sarkasme---sebentar lagi kita lihat penerapan yang lebih
umum.

### Scoping

[i[`typedef` keyword-->scoping rules]<]
`typedef` mengikuti [aturan scope](#scope) biasa.

Karena itu, sangat umum menemukan `typedef` di file scope ("global")
supaya semua fungsi bisa memakai tipe baru itu sesuka hati.

## `typedef` dalam Praktik

Jadi mengganti nama `int` menjadi sesuatu yang lain memang tidak terlalu
menarik. Mari kita lihat di mana `typedef` biasanya muncul.

### `typedef` dan `struct` {#typedef-struct}

[i[`typedef` keyword-->with `struct`s]<]
Kadang sebuah `struct` di-`typedef` menjadi nama baru supaya kita tidak
perlu mengetik kata `struct` berulang-ulang.

``` {.c}
struct animal {
    char *name;
    int leg_count, speed;
};

//  nama asli          nama baru
//            |         |
//            v         v
//      |-----------| |----|
typedef struct animal animal;

struct animal y;  // Ini berfungsi
animal z;         // Ini juga berfungsi karena "animal" adalah alias
```

Secara pribadi, penulis tidak menyukai praktik ini. Penulis suka
kejelasan yang didapat kode ketika kata `struct` ditambahkan ke tipe;
programmer tahu apa yang mereka dapat. Tapi ini sangat umum, jadi tetap
dimasukkan di sini.

Sekarang kita jalankan contoh yang persis sama dengan cara yang mungkin
sering kamu lihat. Kita akan menaruh `struct animal` _di dalam_ `typedef`.
Semuanya bisa digabung seperti ini:

``` {.c}
//  nama asli
//            |
//            v
//      |-----------|
typedef struct animal {
    char *name;
    int leg_count, speed;
} animal;                         // <-- nama baru

struct animal y;  // Ini berfungsi
animal z;         // Ini juga berfungsi karena "animal" adalah alias
```

Itu persis sama dengan contoh sebelumnya, hanya lebih ringkas.

[i[`typedef` keyword-->with anonymous `struct`s]<]
Tapi itu belum semua! Ada jalan pintas umum lain yang mungkin kamu lihat di
kode yang memakai apa yang disebut _struktur anonim_^[Soal ini dibahas
lebih lanjut nanti.]. Ternyata di berbagai tempat kita tidak perlu memberi
nama pada struktur, dan `typedef` adalah salah satunya.

Kita buat contoh yang sama dengan struktur anonim:

``` {.c}
//  struct anonim! Tidak punya nama!
//         |
//         v
//      |----|
typedef struct {
    char *name;
    int leg_count, speed;
} animal;                         // <-- nama baru

//struct animal y;  // ERROR: ini tidak berfungsi lagi--tidak ada struct itu!
animal z;           // Ini berfungsi karena "animal" adalah alias
```

Sebagai contoh lain, kita mungkin menemukan sesuatu seperti ini:

``` {.c}
typedef struct {
    int x, y;
} point;

point p = {.x=20, .y=40};

printf("%d, %d\n", p.x, p.y);  // 20, 40
```
[i[`typedef` keyword-->with anonymous `struct`s]>]
[i[`typedef` keyword-->with `struct`s]>]
[i[`typedef` keyword-->scoping rules]>]

### `typedef` dan Tipe Lain

Bukan berarti memakai `typedef` pada tipe sederhana seperti `int` sama
sekali tidak berguna... ia membantu mengabstraksikan tipe agar lebih mudah
diubah belakangan.

Misalnya, kalau `float` tersebar di 100 zilyun tempat di kodemu, akan
menyakitkan mengubah semuanya menjadi `double` kalau belakangan kamu harus
melakukannya karena suatu alasan.

Tapi kalau kamu sudah bersiap sedikit dengan:

``` {.c}
typedef float app_float;

// dan

app_float f1, f2, f3;
```

Maka kalau belakangan kamu ingin beralih ke tipe lain, seperti `long
double`, kamu cukup mengubah `typedef`-nya:

``` {.c}
//        voila!
//      |---------|
typedef long double app_float;

// dan tidak perlu mengubah baris ini:

app_float f1, f2, f3;  // Sekarang semuanya long double
```

### `typedef` dan Pointer

[i[`typedef` keyword-->with pointers]<]
Kamu bisa membuat tipe yang berupa pointer.

``` {.c}
typedef int *intptr;

int a = 10;
intptr x = &a;  // "intptr" bertipe "int*"
```

Penulis benar-benar tidak menyukai praktik ini. Ia menyembunyikan fakta
bahwa `x` bertipe pointer karena tidak ada `*` di deklarasinya.

Menurut penulis, lebih baik secara eksplisit menunjukkan bahwa kamu
mendeklarasikan tipe pointer supaya developer lain bisa melihatnya dengan
jelas dan tidak keliru mengira `x` bukan tipe pointer.

Tapi menurut hitungan terakhir, katakanlah, 832.007 orang punya pendapat
berbeda.
[i[`typedef` keyword-->with pointers]>]

### `typedef` dan Kapitalisasi

Penulis pernah melihat segala macam kapitalisasi pada `typedef`.

``` {.c}
typedef struct {
    int x, y;
} my_point;          // snake case huruf kecil

typedef struct {
    int x, y;
} MyPoint;          // CamelCase

typedef struct {
    int x, y;
} Mypoint;          // Huruf besar di awal

typedef struct {
    int x, y;
} MY_POINT;          // SNAKE CASE HURUF BESAR
```

Spesifikasi C11 tidak menentukan salah satunya, dan menunjukkan contoh
dalam huruf besar semua maupun huruf kecil semua.

K&R2 terutama memakai huruf besar di awal, tapi menunjukkan beberapa
contoh dalam huruf besar dan snake case (dengan `_t`).

Kalau kamu punya style guide yang dipakai, ikuti saja. Kalau tidak punya,
pilih satu dan ikuti.

## Array dan `typedef`

[i[`typedef` keyword-->with arrays]<]
Sintaksnya agak aneh, dan menurut pengalaman penulis ini jarang terlihat,
tapi kamu bisa me-`typedef` array berisi sejumlah item.

``` {.c}
// Jadikan tipe five_ints sebagai array berisi 5 int
typedef int five_ints[5];

five_ints x = {11, 22, 33, 44, 55};
```

Penulis tidak menyukainya karena ia menyembunyikan sifat array dari
variabelnya, tapi itu mungkin dilakukan.
[i[`typedef` keyword-->with arrays]>]
[i[`typedef` keyword]>]
