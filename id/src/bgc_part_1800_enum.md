<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Enumerated Type: `enum`

[i[`enum` enumerated types]<]

C menawarkan cara lain untuk memiliki nilai integer konstan dengan nama:
`enum`.

Misalnya:

``` {.c}
enum {
  ONE=1,
  TWO=2
};

printf("%d %d", ONE, TWO);  // 1 2
```

Dalam beberapa hal, ini bisa lebih baik---atau berbeda---dibanding memakai
`#define`. Perbedaan utamanya:

* `enum` hanya bisa berupa tipe integer.
* `#define` bisa mendefinisikan apa saja.
* `enum` sering ditampilkan dengan nama identifier simbolisnya di debugger.
* Angka hasil `#define` hanya tampil sebagai angka mentah yang lebih sulit
  diketahui maknanya saat debugging.

Karena berupa tipe integer, mereka bisa dipakai di tempat mana pun
integer bisa dipakai, termasuk pada dimensi array dan pernyataan `case`.

Mari kita bedah lebih jauh.

## Perilaku `enum`

### Penomoran

[i[`enum` enumerated types-->numbering order]<]

`enum` diberi nomor otomatis kecuali kamu menimpanya.

Penomoran dimulai dari `0`, dan bertambah otomatis dari sana, secara
bawaan:

``` {.c}
enum {
    SHEEP,  // Nilainya 0
    WHEAT,  // Nilainya 1
    WOOD,   // Nilainya 2
    BRICK,  // Nilainya 3
    ORE     // Nilainya 4
};

printf("%d %d\n", SHEEP, BRICK);  // 0 3
```

Kamu bisa memaksakan nilai integer tertentu, seperti yang sudah kita
lihat tadi:

``` {.c}
enum {
  X=2,
  Y=18,
  Z=-2
};
```

Duplikat tidak masalah:

``` {.c}
enum {
  X=2,
  Y=2,
  Z=2
};
```

kalau nilai dihilangkan, penomoran melanjutkan hitungan ke arah positif
dari nilai yang terakhir ditentukan. Misalnya:

``` {.c}
enum {
  A,    // 0, nilai awal bawaan
  B,    // 1
  C=4,  // 4, diatur manual
  D,    // 5
  E,    // 6
  F=3,  // 3, diatur manual
  G,    // 4
  H     // 5
}
```

[i[`enum` enumerated types-->numbering order]>]

### Koma di Akhir

Ini sepenuhnya boleh, kalau itu gayamu:

``` {.c}
enum {
  X=2,
  Y=18,
  Z=-2,   // <-- Koma di akhir
};
```

Gaya ini makin populer di bahasa-bahasa dekade terakhir, jadi kamu mungkin
senang melihatnya.

### Scope

[i[`enum` enumerated types-->scope]<]

`enum` mengikuti scope seperti yang kamu harapkan. Kalau di file scope,
seluruh file bisa melihatnya. Kalau di dalam blok, ia lokal untuk blok itu.

Sangat umum `enum` didefinisikan di header file sehingga bisa di-`#include`
di file scope.

[i[`enum` enumerated types-->scope]>]

### Gaya Penulisan

Seperti yang kamu perhatikan, umumnya simbol `enum` dideklarasikan dengan
huruf besar (dengan underscore).

Ini bukan keharusan, tapi merupakan idiom yang sangat, sangat umum.

## `enum`-mu adalah Sebuah Tipe

Ini hal penting yang perlu diketahui tentang `enum`: ia adalah sebuah
tipe, seperti halnya `struct` adalah sebuah tipe.

Kamu bisa memberinya nama tag sehingga bisa merujuk ke tipe itu nanti dan
mendeklarasikan variabel bertipe itu.

Nah, karena `enum` adalah tipe integer, kenapa tidak pakai `int` saja?

Di C, alasan terbaiknya adalah kejelasan kode--ini cara bertipe yang rapi
untuk menjelaskan pemikiranmu dalam kode. C (tidak seperti C++) sebenarnya
tidak memaksa nilai apa pun berada dalam rentang untuk `enum` tertentu.

Mari kita coba contoh ketika kita mendeklarasikan variabel `r` bertipe
`enum resource` yang bisa menampung nilai-nilai itu:

``` {.c}
// enum bernama, tipenya "enum resource"

enum resource {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
};

// Deklarasikan variabel "r" bertipe "enum resource"

enum resource r = BRICK;

if (r == BRICK) {
    printf("I'll trade you a brick for two sheep.\n");
}
```

Kamu juga bisa me-`typedef`-nya, tentu saja, walaupun secara pribadi
aku tidak suka.


``` {.c}
typedef enum {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} RESOURCE;

RESOURCE r = BRICK;
```

Jalan pintas lain yang legal tapi jarang adalah mendeklarasikan variabel
saat mendeklarasikan `enum`:

``` {.c}
// Deklarasikan enum dan beberapa variabel terinisialisasi bertipe itu:

enum {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} r = BRICK, s = WOOD;
```

Kamu juga bisa memberi nama pada `enum` agar bisa dipakai nanti, yang
mungkin ingin kamu lakukan pada kebanyakan kasus:


``` {.c}
// Deklarasikan enum dan beberapa variabel terinisialisasi bertipe itu:

enum resource {   // <-- tipenya sekarang "enum resource"
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} r = BRICK, s = WOOD;
```

Singkatnya, `enum` adalah cara yang bagus untuk menulis kode yang rapi,
ber-scope, dan bertipe.

[i[`enum` enumerated types]>]
