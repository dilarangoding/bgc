<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `struct` II: Lebih Seru dengan `struct`

[i[`struct` keyword]<]

Ternyata ada jauh lebih banyak yang bisa dilakukan dengan `struct`
daripada yang sudah kita bahas, tapi isinya hanya tumpukan besar
hal-hal serba-serbi. Jadi kita kumpulkan saja di bab ini.

Kalau kamu sudah nyaman dengan dasar-dasar `struct`, kamu bisa
melengkapi pengetahuanmu di sini.

## Initializer untuk `struct` Bersarang dan Array

[i[`struct` keyword-->initializers]<]

Ingat bagaimana kamu bisa [menginisialisasi anggota struktur dengan cara
seperti ini](#struct-initializers)?

``` {.c}
struct foo x = {.a=12, .b=3.14};
```

Ternyata initializer ini punya kemampuan lebih daripada yang awalnya
kita sampaikan. Seru!

Untuk satu hal, kalau ada substruktur bersarang seperti berikut, kamu
bisa menginisialisasi anggota substruktur itu dengan mengikuti nama
variabel berurutan ke bawah:

``` {.c}
struct foo x = {.a.b.c=12};
```

Kita lihat contohnya:

``` {.c .numberLines}
#include <stdio.h>

struct cabin_information {
    int window_count;
    int o2level;
};

struct spaceship {
    char *manufacturer;
    struct cabin_information ci;
};

int main(void)
{
    struct spaceship s = {
        .manufacturer="General Products",
        .ci.window_count = 8,   // <-- INITIALIZER BERSARANG!
        .ci.o2level = 21
    };

    printf("%s: %d seats, %d%% oxygen\n",
        s.manufacturer, s.ci.window_count, s.ci.o2level);
}
```

Lihat baris 16-17! Di situlah kita menginisialisasi anggota `struct
cabin_information` dalam definisi `s`, `struct spaceship` kita.

Dan ini opsi lain untuk initializer yang sama---kali ini kita buat yang
tampilannya lebih standar, tapi kedua cara berfungsi:

``` {.c .numberLines startFrom="15"}
    struct spaceship s = {
        .manufacturer="General Products",
        .ci={
            .window_count = 8,
            .o2level = 21
        }
    };
```

Nah, seolah-olah informasi di atas belum cukup spektakuler, kita juga bisa
mencampurkan array initializer di dalamnya.

Mari kita ubah contohnya supaya ada array informasi penumpang di dalamnya,
dan kita lihat bagaimana initializer bekerja di sana juga.

``` {.c .numberLines}
#include <stdio.h>

struct passenger {
    char *name;
    int covid_vaccinated; // Boolean
};

#define MAX_PASSENGERS 8

struct spaceship {
    char *manufacturer;
    struct passenger passenger[MAX_PASSENGERS];
};

int main(void)
{
    struct spaceship s = {
        .manufacturer="General Products",
        .passenger = {
            // Inisialisasi satu field per satu
            [0].name = "Gridley, Lewis",
            [0].covid_vaccinated = 0,

            // Atau sekaligus
            [7] = {.name="Brown, Teela", .covid_vaccinated=1},
        }
    };

    printf("Passengers for %s ship:\n", s.manufacturer);

    for (int i = 0; i < MAX_PASSENGERS; i++)
        if (s.passenger[i].name != NULL)
            printf("    %s (%svaccinated)\n",
                s.passenger[i].name,
                s.passenger[i].covid_vaccinated? "": "not ");
}
```

[i[`struct` keyword-->initializers]>]

## `struct` Anonim

[i[`struct` keyword-->anonymous]<]

Inilah "`struct` tanpa nama". Kita juga menyinggungnya di bagian
[`typedef`](#typedef-struct), tapi kita segarkan lagi di sini.

Ini `struct` biasa:

``` {.c}
struct animal {
    char *name;
    int leg_count, speed;
};
```

Dan ini padanannya yang anonim:

``` {.c}
struct {              // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
};
```

Okeeee. Jadi kita punya `struct`, tapi tanpa nama, sehingga tidak ada
cara memakainya nanti? Kelihatannya sia-sia.

Harus diakui, pada contoh itu memang begitu. Tapi kita masih bisa
memanfaatkannya dengan beberapa cara.

Satu cara yang jarang: karena `struct` anonim merepresentasikan sebuah
tipe, kita bisa menaruh nama variabel setelahnya dan memakainya.

``` {.c}
struct {                   // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
} a, b, c;                 // 3 variabel bertipe struct ini

a.name = "antelope";
c.leg_count = 4;           // misalnya
```

Tapi itu pun belum terlalu berguna.

Jauh lebih umum adalah memakai `struct` anonim bersama `typedef` supaya
bisa dipakai nanti (misalnya untuk mengoper variabel ke fungsi).

``` {.c}
typedef struct {                   // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
} animal;                          // Tipe baru: animal

animal a, b, c;

a.name = "antelope";
c.leg_count = 4;           // misalnya
```

Secara pribadi, aku tidak banyak memakai `struct` anonim. Menurutku
lebih enak melihat seluruh `struct animal` sebelum nama variabel
pada deklarasi.

Tapi itu cuma, ya, pendapatku, kawan.

[i[`struct` keyword-->anonymous]>]

## `struct` yang Merujuk Diri Sendiri

[i[`struct` keyword-->self-referential]<]

Untuk struktur data apa pun yang mirip graf, berguna sekali punya pointer
ke node/vertex yang terhubung. Tapi ini berarti pada definisi sebuah
node, kamu perlu punya pointer ke node. Seperti ayam dan telur!

Tapi ternyata di C ini bisa dilakukan tanpa masalah sama sekali.

Misalnya, ini node linked list:

``` {.c}
struct node {
    int data;
    struct node *next;
};
```

Penting dicatat bahwa `next` adalah pointer. Inilah yang membuat semuanya
bisa dibangun. Walaupun compiler belum tahu seperti apa seluruh `struct
node`, semua pointer berukuran sama.

Berikut program linked list ala kadarnya untuk mengujinya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next;
};

int main(void)
{
    struct node *head;

    // Susun linked list secara asal-asalan (11)->(22)->(33)
    head = malloc(sizeof(struct node));
    head->data = 11;
    head->next = malloc(sizeof(struct node));
    head->next->data = 22;
    head->next->next = malloc(sizeof(struct node));
    head->next->next->data = 33;
    head->next->next->next = NULL;

    // Telusuri
    for (struct node *cur = head; cur != NULL; cur = cur->next) {
        printf("%d\n", cur->data);
    }
}
```

Menjalankannya mencetak:

``` {.default}
11
22
33
```

[i[`struct` keyword-->self-referential]>]

## Flexible Array Member

[i[`struct` keyword-->flexible array members]<]

Di zaman dahulu kala, ketika orang masih mengukir kode C dari kayu,
sebagian orang berpikir alangkah kerennya kalau bisa mengalokasikan
`struct` yang punya array dengan panjang bervariasi di ujungnya.

Aku ingin menegaskan bahwa paruh pertama bagian ini adalah cara lama,
dan setelah itu kita akan memakai cara baru.

Misalnya, mungkin kamu bisa mendefinisikan `struct` untuk menyimpan string
dan panjang string itu. Isinya sebuah panjang dan sebuah array untuk
menampung data. Mungkin seperti ini:

``` {.c}
struct len_string {
    int length;
    char data[8];
};
```

Tapi itu menetapkan `8` sebagai panjang maksimum string, dan itu tidak
banyak. Bagaimana kalau kita melakukan sesuatu yang _cerdik_ dan
`malloc()` sedikit ruang ekstra di ujung setelah struct, lalu membiarkan
data meluap ke ruang itu?

Mari kita lakukan, lalu alokasikan 40 byte lagi di atasnya:

``` {.c}
struct len_string *s = malloc(sizeof *s + 40);
```

Karena `data` adalah field terakhir dari `struct`, kalau kita meluapi field
itu, ia mengalir ke ruang yang sudah kita alokasikan! Karena alasan ini,
trik ini hanya bekerja kalau array pendek itu adalah field _terakhir_
dalam `struct`.

``` {.c}
// Salin lebih dari 8 byte!

strcpy(s->data, "Hello, world!");  // Tidak akan crash. Mungkin.
```

[i[Arrays-->zero length]<]

Bahkan, dulu ada solusi umum dari compiler untuk ini, yaitu
mengalokasikan array berukuran nol di ujung:

``` {.c}
struct len_string {
    int length;
    char data[0];
};
```

Lalu setiap byte ekstra yang kamu alokasikan siap dipakai untuk string
itu.

Karena `data` adalah field terakhir dari `struct`, kalau kita meluapi field
itu, ia mengalir ke ruang yang sudah kita alokasikan!

``` {.c}
// Salin lebih dari 8 byte!

strcpy(s->data, "Hello, world!");  // Tidak akan crash. Mungkin.
```

Tapi, tentu saja, mengakses data di luar ujung array itu adalah undefined
behavior! Di zaman modern ini, kita tidak lagi sudi memakai kebiadaban
seperti itu.

[i[Arrays-->zero length]>]

Untungnya, kita masih bisa mendapat efek yang sama dengan C99 dan
sesudahnya, tapi sekarang legal.

Kita cukup mengubah definisi di atas agar array tidak punya ukuran^[Secara
teknis kita bilang ia punya _incomplete type_.]:

``` {.c}
struct len_string {
    int length;
    char data[];
};
```

Sekali lagi, ini hanya bekerja kalau flexible array member adalah field
_terakhir_ dalam `struct`.

Lalu kita bisa mengalokasikan ruang sebanyak yang kita mau untuk
string-string itu dengan `malloc()` lebih besar dari `struct len_string`,
seperti pada contoh ini yang membuat `struct len_string` baru dari string
C:


``` {.c}
struct len_string *len_string_from_c_string(char *s)
{
    int len = strlen(s);

    // Alokasikan "len" byte lebih banyak dari yang biasanya dibutuhkan
    struct len_string *ls = malloc(sizeof *ls + len);

    ls->length = len;

    // Salin string ke byte-byte ekstra itu
    memcpy(ls->data, s, len);

    return ls;
}
```

[i[`struct` keyword-->flexible array members]>]

## Padding Byte {#struct-padding-bytes}

[i[`struct` keyword-->padding bytes]<]

Waspadai bahwa C boleh menambahkan padding byte di dalam atau setelah
`struct` sesuai kehendaknya. Kamu tidak bisa memastikan bahwa field-field
itu berdekatan langsung di memori^[Walaupun beberapa compiler punya opsi
untuk memaksa hal itu terjadi---cari `__attribute__((packed))` untuk
melihat caranya di GCC.].

Mari lihat program ini. Kita mengeluarkan dua angka. Satu adalah jumlah
`sizeof` tipe-tipe field individual. Satunya lagi `sizeof` seluruh
`struct`.

Orang akan mengira keduanya sama. Ukuran total adalah ukuran jumlah
bagian-bagiannya, kan?

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    int a;
    char b;
    int c;
    char d;
};

int main(void)
{
    printf("%zu\n", sizeof(int) + sizeof(char) + sizeof(int) + sizeof(char));
    printf("%zu\n", sizeof(struct foo));
}
```

Tapi di sistemku, ini mengeluarkan:

``` {.default}
10
16
```

Ternyata tidak sama! Compiler menambahkan 6 byte padding agar performanya
lebih baik. Mungkin di compiler-mu hasilnya berbeda, tapi kecuali kamu
memaksanya, kamu tidak bisa memastikan tidak ada padding.

[i[`struct` keyword-->padding bytes]>]

## `offsetof`

[i[`offsetof()` macro]<]

Di bagian sebelumnya, kita melihat bahwa compiler bisa menyisipkan padding
byte sesukanya di dalam struktur.

Bagaimana kalau kita perlu tahu di mana letaknya? Kita bisa mengukurnya
dengan `offsetof`, yang didefinisikan di `<stddef.h>`.

Mari kita ubah kode di atas untuk mencetak offset field-field individual
dalam `struct`:

``` {.c .numberLines}
#include <stdio.h>
#include <stddef.h>

struct foo {
    int a;
    char b;
    int c;
    char d;
};

int main(void)
{
    printf("%zu\n", offsetof(struct foo, a));
    printf("%zu\n", offsetof(struct foo, b));
    printf("%zu\n", offsetof(struct foo, c));
    printf("%zu\n", offsetof(struct foo, d));
}
```

Di komputerku, ini mengeluarkan:

``` {.default}
0
4
8
12
```

yang menunjukkan bahwa kita memakai 4 byte untuk setiap field. Agak
aneh, karena `char` hanya 1 byte, kan? Compiler menaruh 3 padding byte
setelah setiap `char` agar semua field berukuran 4 byte. Mungkin ini akan
berjalan lebih cepat di CPU-ku.

[i[`offsetof()` macro]>]

<!--

6.7.2.1

15 Within a structure object, the non-bit-field members and the units in which bit-fields reside have addresses that increase in the order in which they are declared. A pointer to a structure object, suitably converted, points to its initial member (or if that member is a bit-field, then to the unit in which it resides), and vice versa. There may be unnamed padding within a structure object, but not at its beginning.

6.2.7 Compatible type and composite type

1 Two types have compatible type if their types are the same.

6.5

7 An object shall have its stored value accessed only by an lvalue expression that has one of the following types:

- a type compatible with the effective type of the object

-->

## Fake OOP {#fake-oop}

Ada hal yang agak kurang ajar dan mirip OOP yang bisa dilakukan dengan
`struct`.

Karena pointer ke `struct` sama dengan pointer ke elemen pertama
`struct`, kamu bebas meng-cast pointer ke `struct` menjadi pointer ke
elemen pertama.

Artinya kita bisa menyusun situasi seperti ini:

``` {.c}
struct parent {
    int a, b;
};

struct child {
    struct parent super;  // HARUS yang pertama
    int c, d;
};
```

Lalu kita bisa mengoper pointer ke `struct child` ke fungsi yang
mengharapkan itu _atau_ pointer ke `struct parent`!

Karena `struct parent super` adalah item pertama dalam `struct child`,
pointer ke `struct child` mana pun sama dengan pointer ke field
`super` itu^[Omong-omong, `super` bukan kata kunci. Aku cuma
meminjam sedikit terminologi OOP.].

Mari kita buat contohnya. Kita buat `struct` seperti di atas, lalu kita
oper pointer ke `struct child` ke fungsi yang butuh pointer ke `struct
parent`... dan itu tetap berfungsi.

``` {.c .numberLines}
#include <stdio.h>

struct parent {
    int a, b;
};

struct child {
    struct parent super;  // HARUS yang pertama
    int c, d;
};

// Menjadikan argumen `void*` supaya kita bisa mengoper tipe apa pun
// (yaitu struct parent atau struct child)
void print_parent(void *p)
{
    // Mengharapkan struct parent--tapi struct child juga berfungsi
    // karena pointer-nya menunjuk ke struct parent di field
    // pertama:
    struct parent *self = p;

    printf("Parent: %d, %d\n", self->a, self->b);
}

void print_child(struct child *self)
{
    printf("Child: %d, %d\n", self->c, self->d);
}

int main(void)
{
    struct child c = {.super.a=1, .super.b=2, .c=3, .d=4};

    print_child(&c);
    print_parent(&c);  // Juga berfungsi meskipun ini struct child!
}
```

Lihat apa yang kita lakukan di baris terakhir `main()`? Kita memanggil
`print_parent()` tapi mengoper `struct child*` sebagai argumen! Walaupun
`print_parent()` butuh argumen yang menunjuk ke `struct parent`, kita
_lolos_ karena field pertama dalam `struct child` adalah `struct parent`.

Sekali lagi, ini berfungsi karena pointer ke `struct` bernilai sama dengan
pointer ke field pertama dalam `struct` itu.

Semua ini bergantung pada bagian spesifikasi ini:

> **§6.7.2.1¶15** [...] A pointer to a structure object, suitably
> converted, points to its initial member [...], and vice versa.

dan

> **§6.5¶7** An object shall have its stored value accessed only by an
> lvalue expression that has one of the following types:
>
> * a type compatible with the effective type of the object
> * [...]

serta asumsiku bahwa "suitably converted" berarti "di-cast ke
effective type dari anggota pertama".

## Bit-Field

[i[`struct` keyword-->bit fields]<]

Menurut pengalamanku, ini jarang dipakai, tapi kamu mungkin
melihatnya sesekali, terutama pada aplikasi level rendah yang
mengemas bit-bit bersama ke dalam ruang yang lebih besar.

Mari kita lihat kode untuk mendemonstrasikan satu kasus pemakaian:

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    unsigned int a;
    unsigned int b;
    unsigned int c;
    unsigned int d;
};

int main(void)
{
    printf("%zu\n", sizeof(struct foo));
}
```

Di sistemku, ini mencetak `16`. Itu masuk akal, karena `unsigned`
berukuran 4 byte di sistemku.

Tapi bagaimana kalau kita tahu bahwa semua nilai yang akan disimpan di
`a` dan `b` bisa disimpan dalam 5 bit, dan nilai di `c` dan `d` bisa
disimpan dalam 3 bit? Totalnya hanya 16 bit. Kenapa mencadangkan 128 bit
untuk mereka kalau kita hanya akan memakai 16?

Nah, kita bisa meminta C dengan sangat sopan untuk mencoba mengemas
nilai-nilai ini. Kita bisa menentukan jumlah bit maksimum yang bisa
dipakai nilai (dari 1 sampai ukuran tipe penampungnya).

Caranya dengan menaruh titik dua setelah nama field, diikuti lebar field
dalam bit.

``` {.c .numberLines startFrom="3"}
struct foo {
    unsigned int a:5;
    unsigned int b:5;
    unsigned int c:3;
    unsigned int d:3;
};
```

Sekarang kalau aku bertanya ke C seberapa besar `struct foo`-ku, jawabannya 4!
Tadinya 16 byte, sekarang hanya 4. Keempat nilai itu "dikemas" menjadi 4
byte, penghematan memori empat kali lipat.

Konsekuensinya, tentu saja, field 5-bit hanya bisa menampung nilai 0-31
dan field 3-bit hanya bisa menampung nilai 0-7. Tapi toh hidup memang soal
kompromi.

### Bit-Field yang Tidak Berdekatan

Satu jebakan: C hanya menggabungkan bit-field yang **berdekatan**. Kalau
mereka disela oleh non-bit-field, tidak ada penghematan:

``` {.c}
struct foo {            // sizeof(struct foo) == 16 (di sistemku)
    unsigned int a:1;   // karena a tidak berdekatan dengan c.
    unsigned int b;
    unsigned int c:1;
    unsigned int d;
};
```

Pada contoh itu, karena `a` tidak berdekatan dengan `c`, keduanya
"dikemas" dalam `int`-nya masing-masing.

Jadi kita punya satu `int` masing-masing untuk `a`, `b`, `c`, dan `d`.
Karena `int` di sistemku 4 byte, totalnya 16 byte.

Penataan ulang singkat menghemat ruang dari 16 byte menjadi 12 byte (di
sistemku):

``` {.c}
struct foo {            // sizeof(struct foo) == 12 (di sistemku)
    unsigned int a:1;
    unsigned int c:1;
    unsigned int b;
    unsigned int d;
};
```

Dan sekarang, karena `a` bersebelahan dengan `c`, compiler menaruh
keduanya dalam satu `int`.

Jadi kita punya satu `int` untuk gabungan `a` dan `c`, dan satu `int`
masing-masing untuk `b` dan `d`. Total 3 `int`, atau 12 byte.

Taruh semua bitfield-mu berdekatan supaya compiler menggabungkannya.

### `int` Signed atau Unsigned

Kalau kamu hanya mendeklarasikan bit-field sebagai `int`, compiler yang
berbeda akan memperlakukannya sebagai `signed` atau `unsigned`. Sama
seperti situasi pada `char`.

Jelaskan tanda (signedness)-nya secara spesifik saat memakai bit-field.

### Bit-Field Tanpa Nama

Pada keadaan tertentu, kamu mungkin perlu mencadangkan beberapa bit untuk
alasan hardware, tapi tidak perlu memakainya di kode.

Misalnya, katakanlah ada satu byte yang 2 bit teratasnya punya makna, 1
bit terbawahnya punya makna, tapi 5 bit di tengah tidak kamu
pakai^[Dengan asumsi `char` 8-bit, yaitu `CHAR_BIT == 8`.].

Kita _bisa_ melakukan sesuatu seperti ini:

``` {.c}
struct foo {
    unsigned char a:2;
    unsigned char dummy:5;
    unsigned char b:1;
};
```

Dan itu berfungsi---di kode kita memakai `a` dan `b`, tapi tidak pernah
`dummy`. Ia hanya ada untuk menghabiskan 5 bit supaya `a` dan `b` berada
di posisi "yang dibutuhkan" (oleh contoh yang dibuat-buat ini) di dalam
byte.

C menyediakan cara untuk merapikannya: _bit-field tanpa nama_. Kamu cukup
menghilangkan nama (`dummy`) dalam kasus ini, dan C tetap senang dengan
efek yang sama:

``` {.c}
struct foo {
    unsigned char a:2;
    unsigned char :5;   // <-- bit-field tanpa nama!
    unsigned char b:1;
};
```

### Bit-Field Tanpa Nama dengan Lebar Nol

Esoterika lagi di sini... Katakanlah kamu mengemas bit ke dalam
`unsigned int`, dan kamu butuh beberapa bit-field berdekatan untuk
dikemas ke `unsigned int` _berikutnya_.

Artinya, kalau kamu menulis ini:

``` {.c}
struct foo {
    unsigned int a:1;
    unsigned int b:2;
    unsigned int c:3;
    unsigned int d:4;
};
```

compiler mengemas semuanya ke dalam satu `unsigned int`. Tapi bagaimana
kalau kamu butuh `a` dan `b` dalam satu `int`, dan `c` dan `d` dalam `int`
yang lain?

Ada solusinya: taruh bit-field tanpa nama berlebar `0` di tempat kamu ingin
compiler mulai mengemas bit dari awal pada `int` yang berbeda:

``` {.c}
struct foo {
    unsigned int a:1;
    unsigned int b:2;
    unsigned int :0;   // <--Bit-field tanpa nama berlebar nol
    unsigned int c:3;
    unsigned int d:4;
};
```

Ini seperti page break eksplisit di word processor. Kamu memberi tahu
compiler, "Berhenti mengemas bit di `unsigned` ini, dan mulai mengemas di
yang berikutnya."

Dengan menambahkan bit-field tanpa nama berlebar nol di tempat itu,
compiler menaruh `a` dan `b` dalam satu `unsigned int`, dan `c` dan `d`
dalam `unsigned int` lain. Total dua, dengan ukuran 8 byte di sistemku (`unsigned int` masing-masing 4 byte).

[i[`struct` keyword-->bit fields]>]

## Union

[i[`union` keyword]<]

Pada dasarnya ini sama seperti `struct`, hanya saja field-nya saling
tumpang tindih di memori. `union` hanya akan sebesar field terbesarnya,
dan kamu hanya bisa memakai satu field dalam satu waktu.

Ini cara untuk memakai ulang ruang memori yang sama untuk berbagai jenis
data.

Deklarasinya sama seperti `struct`, hanya saja memakai `union`. Lihat ini:

``` {.c}
union foo {
    int a, b, c, d, e, f;
    float g, h;
    char i, j, k, l;
};
```

Nah, itu banyak sekali field. Kalau ini `struct`, sistemku akan
bilang dibutuhkan 36 byte untuk menampung semuanya.

Tapi ini `union`, jadi semua field itu saling tumpang tindih dalam
bentangan memori yang sama. Field terbesarnya `int` (atau `float`),
yang memakai 4 byte di sistemku. Dan memang, kalau kutanya `sizeof`
dari `union foo`, jawabannya 4!

Konsekuensinya, hanya satu field yang bisa dipakai secara portabel dalam
satu waktu. Namun...

### Union dan Type Punning {#union-type-punning}

[i[`union` keyword-->type punning]<]

Kamu bisa menulis ke satu field `union` dan membaca dari field lain
secara non-portabel!

Melakukannya disebut [flw[type punning|Type_punning]], dan kamu
memakainya kalau benar-benar tahu apa yang kamu lakukan, biasanya pada
pemrograman level rendah.

Karena anggota union berbagi memori yang sama, menulis ke satu anggota
pasti memengaruhi yang lain. Dan kalau kamu membaca dari satu anggota
sesuatu yang ditulis ke anggota lain, kamu mendapat efek yang aneh.

``` {.c .numberLines}
#include <stdio.h>

union foo {
    float b;
    short a;
};

int main(void)
{
    union foo x;

    x.b = 3.14159;

    printf("%f\n", x.b);  // 3.14159, wajar

    printf("%d\n", x.a);  // Tapi bagaimana dengan ini?
}
```

Di sistemku, ini mencetak:

```
3.141590
4048
```

karena di balik layar, representasi object dari float `3.14159` sama
dengan representasi object dari short `4048`. Di sistemku. Hasilmu
mungkin berbeda.

[i[`union` keyword-->type punning]>]

### Pointer ke `union`

[i[`union` keyword-->pointers to]<]

Kalau kamu punya pointer ke `union`, kamu bisa meng-cast pointer itu ke
tipe field mana pun dalam `union` itu dan mengambil nilainya dengan cara
itu.

Pada contoh ini, kita lihat `union` berisi `int` dan `float`. Kita ambil
pointer ke `union`, tapi kita cast ke tipe `int*` dan `float*` (cast
membungkam peringatan compiler). Lalu kalau kita men-dereference
pointer-pointer itu, kita lihat bahwa nilainya sama dengan yang kita
simpan langsung di `union`.

``` {.c .numberLines}
#include <stdio.h>

union foo {
    int a, b, c, d, e, f;
    float g, h;
    char i, j, k, l;
};

int main(void)
{
    union foo x;

    int *foo_int_p = (int *)&x;
    float *foo_float_p = (float *)&x;

    x.a = 12;
    printf("%d\n", x.a);           // 12
    printf("%d\n", *foo_int_p);    // 12, lagi

    x.g = 3.141592;
    printf("%f\n", x.g);           // 3.141592
    printf("%f\n", *foo_float_p);  // 3.141592, lagi
}
```

Kebalikannya juga benar. Kalau kita punya pointer ke tipe di dalam
`union`, kita bisa meng-cast-nya ke pointer ke `union` dan mengakses
anggotanya.

``` {.c}
union foo x;
int *foo_int_p = (int *)&x;             // Pointer ke field int
union foo *p = (union foo *)foo_int_p;  // Kembali ke pointer ke union

p->a = 12;  // Baris ini sama dengan...
x.a = 12;   // yang ini.
```

Semua ini hanya memberi tahu bahwa, di balik layar, semua nilai dalam
`union` dimulai di tempat yang sama di memori, dan itu sama dengan lokasi
seluruh `union`.

[i[`union` keyword-->pointers to]>]

### Common Initial Sequence pada Union

[i[`union` keyword-->common initial sequences]<]

Kalau kamu punya `union` berisi `struct`, dan semua `struct` itu diawali
dengan _common initial sequence_, boleh mengakses anggota urutan itu
dari anggota `union` mana pun.

Apa?

Berikut dua `struct` dengan common initial sequence:

``` {.c}
struct a {
	int x;     //
	float y;   // Common initial sequence

	char *p;
};

struct b {
	int x;     //
	float y;   // Common initial sequence

	double *p;
	short z;
};
```

Kamu lihat? Keduanya diawali `int` diikuti `float`---itulah common initial
sequence. Anggota-anggota dalam urutan pada `struct` harus bertipe
kompatibel. Dan kita lihat itu pada `x` dan `y`, yaitu `int` dan `float`
masing-masing.

Sekarang kita buat union dari keduanya:

``` {.c}
union foo {
	struct a sa;
	struct b sb;
};
```

Aturan ini menjamin bahwa anggota common initial sequence bisa saling
menggantikan di kode. Artinya:

* `f.sa.x` sama dengan `f.sb.x`.

dan

* `f.sa.y` sama dengan `f.sb.y`.

Karena field `x` dan `y` sama-sama ada di common initial sequence.

Selain itu, nama anggota dalam common initial sequence tidak penting---yang
penting hanyalah tipenya sama.

Secara keseluruhan, ini memberi kita cara untuk menambahkan informasi
bersama di antara `struct` dalam `union` secara aman. Contoh terbaiknya
mungkin memakai sebuah field untuk menentukan `struct` mana, dari semua
`struct` dalam `union`, yang sedang "dipakai".

Artinya, kalau ini tidak diizinkan dan kita mengoper `union` ke suatu
fungsi, bagaimana fungsi itu tahu anggota `union` mana yang harus
dilihatnya?

Lihat `struct`-`struct` ini. Perhatikan common initial sequence-nya:

``` {.c .numberLines}
#include <stdio.h>

struct common {
	int type;   // common initial sequence
};

struct antelope {
	int type;   // common initial sequence

	int loudness;
};

struct octopus {
	int type;   // common initial sequence

	int sea_creature;
	float intelligence;
};

```

Sekarang kita masukkan ke dalam `union`:

``` {.c .numberLines startFrom="20"}
union animal {
	struct common common;
	struct antelope antelope;
	struct octopus octopus;
};

```

Mohon dimaklumi dua `#define` untuk demo ini:

``` {.c .numberLines startFrom="26"}
#define ANTELOPE 1
#define OCTOPUS  2

```

Sejauh ini, belum ada yang istimewa. Tampaknya field `type` sama sekali
tidak berguna.

Tapi sekarang mari kita buat fungsi generik yang mencetak `union animal`.
Ia harus bisa membedakan apakah yang dilihatnya `struct antelope` atau
`struct octopus`.

Berkat keajaiban common initial sequence, ia bisa mencari tipe hewan di
salah satu dari tempat-tempat ini untuk `union animal x` tertentu:

``` {.c}
int type = x.common.type;    // atau...
int type = x.antelope.type;  // atau...
int type = x.octopus.type;
```

Semuanya merujuk ke nilai yang sama di memori.

Dan, seperti yang mungkin sudah kamu duga, `struct common` ada di sana
supaya kode bisa melihat tipe secara agnostik tanpa menyebut hewan
tertentu.

Mari kita lihat kode untuk mencetak `union animal`:

``` {.c .numberLines startFrom="29"}
void print_animal(union animal *x)
{
	switch (x->common.type) {
		case ANTELOPE:
			printf("Antelope: loudness=%d\n", x->antelope.loudness);
			break;

		case OCTOPUS:
			printf("Octopus : sea_creature=%d\n", x->octopus.sea_creature);
			printf("          intelligence=%f\n", x->octopus.intelligence);
			break;
		
		default:
			printf("Unknown animal type\n");
	}

}

int main(void)
{
	union animal a = {.antelope.type=ANTELOPE, .antelope.loudness=12};
	union animal b = {.octopus.type=OCTOPUS, .octopus.sea_creature=1,
	                                   .octopus.intelligence=12.8};

	print_animal(&a);
	print_animal(&b);
}
```

Lihat bagaimana di baris 29 kita hanya mengoper `union`-nya---kita tidak
tahu `struct` hewan apa yang sedang dipakai di dalamnya.

Tapi tidak apa-apa! Karena di baris 31 kita memeriksa tipenya untuk melihat
apakah itu antelope atau octopus. Lalu kita bisa melihat `struct` yang
tepat untuk mengambil anggotanya.

Efek yang sama pasti bisa didapat hanya dengan `struct`, tapi kamu bisa
memakai cara ini kalau menginginkan efek hemat memori dari `union`.

[i[`union` keyword-->common initial sequences]>]

## Union dan Struct Tanpa Nama

[i[`union` keyword-->and unnamed `struct`s]<]

Kamu tahu bahwa kamu bisa punya struct tanpa nama, seperti ini:

``` {.c}
struct {
    int x, y;
} s;
```

Itu mendefinisikan variabel `s` bertipe `struct` anonim (karena `struct`
itu tidak punya name tag), dengan anggota `x` dan `y`.

Jadi hal seperti ini valid:

``` {.c}
s.x = 34;
s.y = 90;

printf("%d %d\n", s.x, s.y);
```

Ternyata `struct` tanpa nama itu bisa ditaruh dalam `union` seperti yang
mungkin kamu duga:

``` {.c}
union foo {
    struct {       // tanpa nama!
        int x, y;
    } a;

    struct {       // tanpa nama!
        int z, w;
    } b;
};
```

Lalu diakses seperti biasa:

``` {.c}
union foo f;

f.a.x = 1;
f.a.y = 2;
f.b.z = 3;
f.b.w = 4;
```

Tidak ada masalah!

[i[`union` keyword-->and unnamed `struct`s]>]

## Mengoper dan Mengembalikan `struct` dan `union`

[i[`union` keyword-->passing and returning]<]
[i[`struct` keyword-->passing and returning]<]

Kamu bisa mengoper `struct` atau `union` ke fungsi secara by value
(bukan pointer ke objeknya)---salinan objek itu ke parameter dibuat
seperti pada penugasan biasa.

Kamu juga bisa mengembalikan `struct` atau `union` dari fungsi dan ia juga
dioper balik secara by value.

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    int x, y;
};

struct foo f(void)
{
    return (struct foo){.x=34, .y=90};
}

int main(void)
{
    struct foo a = f();  // Salinan dibuat

    printf("%d %d\n", a.x, a.y);
}
```

Fakta menarik: kalau kamu melakukan ini, kamu bisa memakai operator `.`
langsung pada pemanggilan fungsi:

``` {.c .numberLines startFrom="16"}
    printf("%d %d\n", f().x, f().y);
```

(Tentu saja contoh itu memanggil fungsi dua kali, secara tidak efisien.)

Dan hal yang sama berlaku untuk mengembalikan pointer ke `struct` dan
`union`---pastikan saja memakai arrow operator `->` dalam kasus itu.

[i[`union` keyword-->passing and returning]>]
[i[`struct` keyword-->passing and returning]>]
[i[`union` keyword]>]
[i[`struct` keyword]>]
