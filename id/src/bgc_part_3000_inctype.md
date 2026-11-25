<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Incomplete Type

[i[Incomplete types]<]

Mungkin mengejutkan bahwa kode ini dibangun tanpa error:

``` {.c}
extern int a[];

int main(void)
{
    struct foo *x;
    union bar *y;
    enum baz *z;
}
```

Kita tidak pernah memberi ukuran untuk `a`. Dan kita punya pointer ke
`struct` `foo`, `bar`, dan `baz` yang tampaknya tidak dideklarasikan di
mana pun.

Dan satu-satunya peringatan yang kudapat hanyalah bahwa `x`, `y`, dan `z`
tidak dipakai.

Inilah contoh _incomplete type_.

Incomplete type adalah tipe yang ukurannya (yaitu ukuran yang akan kamu
dapat dari `sizeof`) tidak diketahui. Cara lain memikirkannya: tipe yang
belum selesai kamu deklarasikan.

Kamu bisa punya pointer ke incomplete type, tapi tidak bisa
men-dereference-nya atau memakai aritmetika pointer padanya. Dan kamu
tidak bisa meng-`sizeof`-nya.

Jadi apa yang bisa dilakukan dengannya?

## Kasus Pemakaian: Struktur yang Merujuk Diri Sendiri

[i[Incomplete types-->self-referential `struct`s]<]

Aku hanya tahu satu kasus pemakaian yang nyata: forward reference ke
`struct` atau `union` dengan struktur yang merujuk diri sendiri atau saling
bergantung. (Untuk sisa contoh, aku akan memakai `struct`, tapi semuanya
berlaku sama untuk `union`.)

Kita kerjakan contoh klasiknya dulu.

Tapi sebelum aku mulai, ketahuilah ini! Saat kamu mendeklarasikan sebuah
`struct`, `struct` itu incomplete sampai kurung kurawal penutup tercapai!

``` {.c}
struct antelope {              // struct antelope incomplete di sini
    int leg_count;             // Masih incomplete
    float stomach_fullness;    // Masih incomplete
    float top_speed;           // Masih incomplete
    char *nickname;            // Masih incomplete
};                             // SEKARANG sudah complete.
```

Lalu kenapa? Kelihatannya cukup waras.

Tapi bagaimana kalau kita membuat linked list? Setiap node linked list
perlu punya referensi ke node lain. Tapi bagaimana kita membuat referensi
ke node lain kalau deklarasi node saja belum selesai?

Kelonggaran C untuk incomplete type memungkinkannya. Kita tidak bisa
mendeklarasikan sebuah node, tapi _bisa_ mendeklarasikan pointer ke node,
walaupun ia incomplete!

``` {.c}
struct node {
    int val;
    struct node *next;  // struct node incomplete, tapi tidak masalah!
};
```

Meskipun `struct node` incomplete di baris 3, kita tetap bisa
mendeklarasikan pointer ke sana^[Ini bisa karena di C, pointer berukuran
sama apa pun tipe data yang ditunjuknya. Jadi compiler tidak perlu tahu
ukuran `struct node` pada titik ini; ia hanya perlu tahu ukuran
pointer.].

Hal yang sama bisa dilakukan kalau kita punya dua `struct` berbeda yang
saling merujuk:

``` {.c}
struct a {
    struct b *x;  // Merujuk ke `struct b`
};

struct b {
    struct a *x;  // Merujuk ke `struct a`
};
```

Kita tidak akan pernah bisa membuat pasangan struktur itu tanpa aturan
yang dilonggarkan untuk incomplete type.

[i[Incomplete types-->self-referential `struct`s]>]

## Pesan Error Incomplete Type

Apakah kamu mendapat error seperti ini?

``` {.default}
invalid application of ‘sizeof’ to incomplete type

invalid use of undefined type

dereferencing pointer to incomplete type
```

Tersangka paling mungkin: kamu kemungkinan lupa meng-`#include` header file
yang mendeklarasikan tipe itu.

## Incomplete Type Lainnya

Mendeklarasikan `struct` atau `union` tanpa badan membuat incomplete type,
mis. `struct foo;`.

`enum` incomplete sampai kurung kurawal penutup.

`void` adalah incomplete type.

Array yang dideklarasikan `extern` tanpa ukuran adalah incomplete, mis.:

``` {.c}
extern int a[];
```

Kalau ia array non-`extern` tanpa ukuran yang diikuti initializer, ia
incomplete sampai kurung kurawal penutup initializer.

## Kasus Pemakaian: Array di Header File

Berguna mendeklarasikan tipe array incomplete di header file. Dalam kasus
itu, penyimpanan sebenarnya (tempat array lengkap dideklarasikan) harus ada
di satu file `.c`. Kalau ditaruh di file `.h`, ia akan diduplikasi setiap
kali header file di-include.

Jadi yang bisa dilakukan adalah membuat header file dengan incomplete type
yang merujuk ke array, seperti ini:

``` {.c .numberLines}
// File: bar.h

#ifndef BAR_H
#define BAR_H

extern int my_array[];  // Incomplete type

#endif
```

Lalu di file `.c`, definisikan array-nya yang sebenarnya:

``` {.c .numberLines}
// File: bar.c

int my_array[1024];     // Complete type!
```

Setelah itu kamu bisa meng-include header itu dari sebanyak mungkin tempat
yang kamu mau, dan setiap tempat itu akan merujuk ke `my_array` yang sama.

``` {.c .numberLines}
// File: foo.c

#include <stdio.h>
#include "bar.h"    // menyertakan incomplete type untuk my_array

int main(void)
{
    my_array[0] = 10;

    printf("%d\n", my_array[0]);
}
```

Saat mengompilasi banyak file, ingat untuk menyebutkan semua file `.c` ke
compiler, tapi bukan file `.h`, mis.:

``` {.zsh}
gcc -o foo foo.c bar.c
```

## Melengkapi Incomplete Type

Kalau kamu punya incomplete type, kamu bisa melengkapinya dengan
mendefinisikan `struct`, `union`, `enum`, atau array yang lengkap di scope
yang sama.

``` {.c}
struct foo;        // incomplete type

struct foo *p;     // pointer, tidak masalah

// struct foo f;   // Error: incomplete type!

struct foo {
    int x, y, z;
};                 // Sekarang struct foo sudah complete!

struct foo f;      // Berhasil!
```

Perhatikan bahwa walaupun `void` adalah incomplete type, tidak ada cara
untuk melengkapinya. Bukan berarti ada yang pernah terpikir melakukan hal
aneh itu. Tapi ini menjelaskan kenapa kamu bisa melakukan ini:

``` {.c}
void *p;             // OK: pointer ke incomplete type
```

dan tidak salah satu dari dua ini:

``` {.c}
void v;              // Error: mendeklarasikan variabel bertipe incomplete

printf("%d\n", *p);  // Error: men-dereference incomplete type
```

Makin tahu aja...

[i[Incomplete types]<]
