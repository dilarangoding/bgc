<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe Bagian V: Compound Literal dan Generic Selection

Ini bab terakhir tentang tipe! Kita akan membahas dua hal:

* Cara memiliki object "anonim" tanpa nama dan apa gunanya.
* Cara menghasilkan kode yang bergantung pada tipe.

Keduanya tidak terlalu berkaitan, tapi masing-masing tidak cukup layak
dijadikan satu bab sendiri. Jadi aku menjejalkannya di sini seperti
pemberontak!

## Compound Literal

[i[Compound literals]<]

Ini fitur bahasa yang keren, yang memungkinkan kamu membuat object bertipe
tertentu secara langsung tanpa pernah menugaskannya ke variabel. Kamu bisa
membuat tipe sederhana, array, `struct`, apa saja.

Salah satu kegunaan utamanya adalah mengoper argumen kompleks ke fungsi
ketika kamu tidak ingin membuat variabel sementara untuk menampung
nilainya.

Cara membuat compound literal adalah menaruh nama tipe dalam tanda kurung,
lalu menaruh initializer list setelahnya. Misalnya, array `int` tanpa
nama mungkin tampak seperti ini:

``` {.c}
(int []){1,2,3,4}
```

Nah, baris kode itu sendiri tidak melakukan apa-apa. Ia membuat array
tanpa nama berisi 4 `int`, lalu membuangnya tanpa dipakai.

Kita bisa memakai pointer untuk menyimpan referensi ke array itu...

``` {.c}
int *p = (int []){1 ,2 ,3 ,4};

printf("%d\n", p[1]);  // 2
```

Tapi itu terasa seperti cara bertele-tele untuk punya array. Maksudnya, kita
bisa saja melakukan ini^[Yang tidak persis sama, karena ia array, bukan
pointer ke `int`.]:

``` {.c}
int p[] = {1, 2, 3, 4};

printf("%d\n", p[1]);  // 2
```

Jadi mari kita lihat contoh yang lebih berguna.

### Mengoper Object Tanpa Nama ke Fungsi

[i[Compound literals-->passing to functions]<]

Katakanlah kita punya fungsi untuk menjumlahkan array `int`:

``` {.c}
int sum(int p[], int count)
{
    int total = 0;

    for (int i = 0; i < count; i++)
        total += p[i];

    return total;
}
```

Kalau ingin memanggilnya, biasanya kita harus melakukan sesuatu seperti
ini, mendeklarasikan array dan mengisinya dengan nilai untuk dioper ke
fungsi:

``` {.c}
int a[] = {1, 2, 3, 4};

int s = sum(a, 4);
```

Tapi object tanpa nama memberi kita cara untuk melewati variabel itu
dengan mengopernya langsung (nama parameter tercantum di atas). Lihat
ini---kita akan mengganti variabel `a` dengan array tanpa nama yang dioper
sebagai argumen pertama:

``` {.c}
//                   p[]         count
//           |-----------------|  |
int s = sum((int []){1, 2, 3, 4}, 4);
```

Keren sekali!

[i[Compound literals-->passing to functions]>]

### `struct` Tanpa Nama

[i[Compound literals-->with `struct`]<]
[i[`struct` keyword-->compound literals]<]

Kita bisa melakukan hal serupa dengan `struct`.

Pertama, kita kerjakan tanpa object tanpa nama. Kita definisikan `struct`
untuk menyimpan koordinat `x`/`y`. Lalu kita definisikan satu, dengan
mengoper nilai ke initializer-nya. Terakhir, kita oper ke fungsi untuk
mencetak nilainya:

``` {.c .numberLines}
#include <stdio.h>

struct coord {
    int x, y;
};

void print_coord(struct coord c)
{
    printf("%d, %d\n", c.x, c.y);
}

int main(void)
{
    struct coord t = {.x=10, .y=20};

    print_coord(t);   // mencetak "10, 20"
}
```

Cukup lugas?

Mari kita ubah agar memakai object tanpa nama sebagai ganti variabel `t`
yang kita oper ke `print_coord()`.

Kita cukup mengambil `t` dari sana dan menggantinya dengan `struct` tanpa
nama:

``` {.c .numberLines startFrom="7"}
    //struct coord t = {.x=10, .y=20};

    print_coord((struct coord){.x=10, .y=20});   // mencetak "10, 20"
```

Tetap berfungsi!

[i[`struct` keyword-->compound literals]>]
[i[Compound literals-->with `struct`]>]

### Pointer ke Object Tanpa Nama

[i[Compound literals-->pointers to]<]

Mungkin kamu memperhatikan pada contoh terakhir bahwa walaupun kita
memakai `struct`, yang dioper ke `print_coord()` adalah salinan `struct`,
bukan pointer ke `struct`.

Ternyata kita bisa mengambil alamat object tanpa nama dengan `&` seperti
biasa.

Ini karena, secara umum, kalau suatu operator bisa bekerja pada variabel
bertipe tertentu, kamu bisa memakai operator itu pada object tanpa nama
bertipe sama.

Mari kita ubah kode di atas agar mengoper pointer ke object tanpa nama

``` {.c .numberLines}
#include <stdio.h>

struct coord {
    int x, y;
};

void print_coord(struct coord *c)
{
    printf("%d, %d\n", c->x, c->y);
}

int main(void)
{
    //     Perhatikan &
    //          |
    print_coord(&(struct coord){.x=10, .y=20});   // mencetak "10, 20"
}
```

Selain itu, ini bisa menjadi cara yang bagus untuk mengoper pointer ke
object sederhana sekalipun:

``` {.c}
// Oper pointer ke int bernilai 3490
foo(&(int){3490});
```

Semudah itu.

[i[Compound literals-->pointers to]>]

### Object Tanpa Nama dan Scope

[i[Compound literals-->scope]<]

Masa hidup object tanpa nama berakhir di akhir scope-nya. Jebakan terbesarnya:
kamu membuat object tanpa nama baru, mengambil pointer
ke sana, lalu meninggalkan scope object itu. Dalam kasus itu, pointer
akan merujuk ke object yang sudah mati.

Jadi ini undefined behavior:

``` {.c}
int *p;

{
    p = &(int){10};
}

printf("%d\n", *p);  // TIDAK VALID: (int){10} sudah keluar dari scope
```

Begitu juga, kamu tidak bisa mengembalikan pointer ke object tanpa nama
dari sebuah fungsi. Object itu didealokasikan saat keluar dari scope:

``` {.c .numberLines}
#include <stdio.h>

int *get3490(void)
{
    // Jangan lakukan ini
    return &(int){3490};
}

int main(void)
{
    printf("%d\n", *get3490());  // TIDAK VALID: (int){3490} sudah keluar dari scope
}
```

Bayangkan saja scope-nya seperti scope variabel lokal biasa. Kamu juga
tidak bisa mengembalikan pointer ke variabel lokal.

[i[Compound literals-->scope]>]

### Contoh Konyol Object Tanpa Nama

Tipe apa pun bisa ditaruh di sana untuk membuat object tanpa nama.

Misalnya, ketiga ini secara efektif setara:

``` {.c}
int x = 3490;

printf("%d\n", x);               // 3490 (variabel)
printf("%d\n", 3490);            // 3490 (konstanta)
printf("%d\n", (int){3490});     // 3490 (object tanpa nama)
```

Yang terakhir itu tanpa nama, tapi konyol. Lebih baik pakai yang sederhana
di baris sebelumnya.

Tapi semoga ini memberi sedikit kejelasan tentang sintaksnya.

[i[Compound literals]>]

## Generic Selection {#type-generics}

[i[Generic selections]<]

Ini ekspresi yang memungkinkan kamu memilih potongan kode berbeda
tergantung pada _tipe_ argumen pertama ekspresi itu.

Sebentar lagi kita lihat contohnya, tapi penting diketahui bahwa ini
diproses saat compile time, _bukan saat runtime_. Tidak ada analisis
runtime di sini.

[i[`_Generic` keyword]<]

Ekspresi ini diawali `_Generic`, bekerja mirip `switch`, dan menerima
setidaknya dua argumen.

Argumen pertama adalah ekspresi (atau variabel^[Variabel yang dipakai di
sini _adalah_ ekspresi.]) yang punya _tipe_. Semua ekspresi punya tipe.
Argumen sisanya untuk `_Generic` adalah kasus-kasus tentang apa yang
disubstitusikan sebagai hasil ekspresi kalau argumen pertama bertipe itu.

Wat?

Mari kita coba dan lihat.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    float f;
    char c;

    char *s = _Generic(i,
                    int: "that variable is an int",
                    float: "that variable is a float",
                    default: "that variable is some type"
                );

    printf("%s\n", s);
}
```

Lihat ekspresi `_Generic` yang dimulai di baris 9.

Saat compiler melihatnya, ia melihat tipe argumen pertama. (Pada contoh
ini, tipe variabel `i`.) Lalu ia menelusuri kasus-kasus untuk mencari yang
bertipe itu. Setelah itu ia menyubstitusikan argumen tersebut menggantikan
seluruh ekspresi `_Generic`. 

Dalam kasus ini, `i` adalah `int`, jadi cocok dengan kasus itu. Lalu string
itu disubstitusikan ke ekspresi. Jadi baris itu berubah menjadi ini saat
dilihat compiler:

``` {.c}
    char *s = "that variable is an int";
```

Kalau compiler tidak menemukan kecocokan tipe di `_Generic`, ia mencari
kasus `default` yang opsional dan memakainya.

Kalau tidak ada kecocokan tipe dan tidak ada `default`, kamu akan mendapat
error kompilasi. Ekspresi pertama **harus** cocok dengan salah satu tipe
atau `default`.

Karena merepotkan menulis `_Generic` berulang-ulang, ia sering dipakai
untuk membuat badan makro yang bisa dengan mudah dipakai ulang.

Mari kita buat makro `TYPESTR(x)` yang menerima satu argumen dan
mengembalikan string berisi tipe argumen itu.

Jadi `TYPESTR(1)` akan mengembalikan string `"int"`, misalnya.

Begini caranya:

``` {.c}
#include <stdio.h>

#define TYPESTR(x) _Generic((x), \
                        int: "int", \
                        long: "long", \
                        float: "float", \
                        double: "double", \
                        default: "something else")

int main(void)
{
    int i;
    long l;
    float f;
    double d;
    char c;

    printf("i is type %s\n", TYPESTR(i));
    printf("l is type %s\n", TYPESTR(l));
    printf("f is type %s\n", TYPESTR(f));
    printf("d is type %s\n", TYPESTR(d));
    printf("c is type %s\n", TYPESTR(c));
}
```

Ini menghasilkan output:

``` {.default}
i is type int
l is type long
f is type float
d is type double
c is type something else
```

Yang seharusnya tidak mengejutkan, karena, seperti yang kita katakan, kode
di `main()` itu diganti dengan yang berikut saat dikompilasi:

``` {.c}
    printf("i is type %s\n", "int");
    printf("l is type %s\n", "long");
    printf("f is type %s\n", "float");
    printf("d is type %s\n", "double");
    printf("c is type %s\n", "something else");
```

Dan itulah persis output yang kita lihat.

Satu contoh lagi. Di sini aku menyertakan beberapa makro agar ketika
kamu menjalankan:

``` {.c}
int i = 10;
char *s = "Foo!";

PRINT_VAL(i);
PRINT_VAL(s);
```

kamu mendapat output:

``` {.default}
i = 10
s = Foo!
```

Kita perlu memakai sedikit sihir makro untuk itu.

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

// Makro yang mengembalikan format specifier untuk sebuah tipe
#define FMTSPEC(x) _Generic((x), \
                        int: "%d", \
                        long: "%ld", \
                        float: "%f", \
                        double: "%f", \
                        char *: "%s")
                        // TODO: tambahkan tipe lain
                        
// Makro yang mencetak variabel dalam bentuk "nama = nilai"
#define PRINT_VAL(x) do { \
    char fmt[512]; \
    snprintf(fmt, sizeof fmt, #x " = %s\n", FMTSPEC(x)); \
    printf(fmt, (x)); \
} while(0)

int main(void)
{
    int i = 10;
    float f = 3.14159;
    char *s = "Hello, world!";

    PRINT_VAL(i);
    PRINT_VAL(f);
    PRINT_VAL(s);
}
```

[i[`_Generic` keyword]>]

untuk output:

``` {.default}
i = 10
f = 3.141590
s = Hello, world!
```

Kita bisa saja menjejalkan semuanya dalam satu makro besar, tapi aku
memecahnya jadi dua supaya mata tidak berdarah.

[i[Generic selections]>]
