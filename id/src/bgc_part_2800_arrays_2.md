<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Array Bagian II

Di bab ini kita akan membahas beberapa hal serba-serbi tambahan tentang
array.

* Type qualifier pada parameter array
* Kata kunci `static` pada parameter array
* Initializer array multidimensi parsial

Hal-hal ini tidak terlalu sering terlihat, tapi kita intip saja karena
termasuk bagian spesifikasi yang lebih baru.

## Type Qualifier untuk Array dalam Daftar Parameter

[i[Type qualifiers-->arrays in parameter lists]<]
[i[Arrays-->type qualifiers in parameter lists]<]

Kalau kamu ingat dari bab sebelumnya, dua hal ini setara dalam daftar
parameter fungsi:

``` {.c}
int func(int *p) {...}
int func(int p[]) {...}
```

Dan mungkin kamu juga ingat bahwa kamu bisa menambahkan type qualifier ke
variabel pointer seperti ini:

``` {.c}
int *const p;
int *volatile p;
int *const volatile p;
// dan seterusnya.
```

Tapi bagaimana melakukannya kalau kita memakai notasi array di daftar
parameter?

Ternyata qualifier itu ditaruh di dalam kurung siku. Dan kamu bisa
menaruh jumlah opsional setelahnya. Ketiga baris berikut setara:

``` {.c}
int func(int *const volatile p) {...}
int func(int p[const volatile]) {...}
int func(int p[const volatile 10]) {...}
```

Kalau kamu punya array multidimensi, type qualifier harus ditaruh di
pasangan kurung siku pertama.

[i[Type qualifiers-->arrays in parameter lists]>]
[i[Arrays-->type qualifiers in parameter lists]>]

## `static` untuk Array dalam Daftar Parameter

[i[Arrays-->`static` in parameter lists]<]

Demikian pula, kata kunci static bisa dipakai pada array di daftar
parameter.

Aku belum pernah melihat ini di alam liar. Ia **selalu** diikuti
dimensi:

``` {.c}
int func(int p[static 4]) {...}
```

Artinya, pada contoh di atas, compiler akan menganggap bahwa array apa pun
yang dioper ke fungsi berisi _setidaknya_ 4 elemen.

Selain itu adalah undefined behavior.

``` {.c}
int func(int p[static 4]) {...}

int main(void)
{
    int a[] = {11, 22, 33, 44};
    int b[] = {11, 22, 33, 44, 55};
    int c[] = {11, 22};

    func(a);  // OK! a berisi 4 elemen, batas minimum
    func(b);  // OK! b berisi setidaknya 4 elemen
    func(c);  // Undefined behavior! c berisi kurang dari 4 elemen!
}
```

Ini pada dasarnya menetapkan ukuran minimum array yang boleh dioper.

Catatan penting: tidak ada apa pun di compiler yang melarangmu mengoper
array yang lebih kecil. Compiler kemungkinan tidak akan memperingatkan, dan
tidak akan mendeteksinya saat runtime.

Dengan menaruh `static` di sana, kamu berkata, "Aku JANJI dengan
sumpah rahasia ganda bahwa aku tidak akan pernah mengoper array yang lebih kecil
dari ini." Dan compiler menjawab, "Ya, baiklah," dan memercayaimu untuk
tidak melakukannya.

Lalu compiler bisa melakukan optimasi kode tertentu, dengan tenang karena
tahu bahwa kamu, sang programmer, akan selalu melakukan hal yang benar.

[i[Arrays-->`static` in parameter lists]>]

## Initializer yang Setara

[i[Arrays-->multidimensional initializers]<]

C itu, bolehlah dibilang, agak _fleksibel_ soal array initializer.

Kita sudah melihat sebagian, yaitu nilai yang tidak disebutkan diganti
dengan nol.

Misalnya, kita bisa menginisialisasi array 5 elemen menjadi `1,2,0,0,0`
dengan ini:

``` {.c}
int a[5] = {1, 2};
```

Atau mengisi seluruh array dengan nol memakai:

``` {.c}
int a[5] = {0};
```

Tapi segalanya jadi menarik saat menginisialisasi array multidimensi.

Kita buat array dengan 3 baris dan 2 kolom:

``` {.c}
int a[3][2];
```

Kita tulis kode untuk menginisialisasinya dan mencetak hasilnya:

``` {.c}
#include <stdio.h>

int main(void)
{
    int a[3][2] = {
        {1, 2},
        {3, 4},
        {5, 6}
    };

    for (int row = 0; row < 3; row++) {
        for (int col = 0; col < 2; col++)
            printf("%d ", a[row][col]);
        printf("\n");
    }
}
```

Dan saat dijalankan, kita mendapat hasil yang diharapkan:

``` {.default}
1 2
3 4
5 6
```

Mari kita hilangkan beberapa elemen initializer dan lihat bahwa mereka
diisi nol:


``` {.c}
    int a[3][2] = {
        {1, 2},
        {3},    // Angka 4 dihilangkan!
        {5, 6}
    };
```

yang menghasilkan:

``` {.default}
1 2
3 0
5 6
```

Sekarang kita hilangkan seluruh elemen tengah:

``` {.c}
    int a[3][2] = {
        {1, 2},
        // {3, 4},   // Potong saja seluruh bagian ini
        {5, 6}
    };
```

Dan sekarang kita mendapat ini, yang mungkin bukan yang kamu harapkan:

``` {.default}
1 2
5 6
0 0
```

Tapi kalau dipikir-pikir, kita hanya memberi initializer yang cukup untuk
dua baris, jadi initializer itu dipakai untuk dua baris pertama. Dan elemen
sisanya diinisialisasi dengan nol.

Sejauh ini bagus. Secara umum, kalau kita menghilangkan sebagian
initializer, compiler mengisi elemen yang bersesuaian dengan `0`.

Tapi sekarang mari kita menggila.

``` {.c}
    int a[3][2] = { 1, 2, 3, 4, 5, 6 };
```

Apa---? Itu array 2D, tapi initializer-nya hanya 1D!

Ternyata itu legal (walaupun GCC akan memperingatkannya kalau peringatan
yang tepat diaktifkan).

Pada dasarnya, yang terjadi: pengisian elemen dimulai di baris 0, lalu
baris 1, lalu baris 2, dari kiri ke kanan.

Jadi saat dicetak, urutannya:

``` {.default}
1 2
3 4
5 6
```

Kalau sebagian dihilangkan:

``` {.c}
    int a[3][2] = { 1, 2, 3 };
```

sisanya diisi `0`:

``` {.default}
1 2
3 0
0 0
```

Jadi kalau ingin mengisi seluruh array dengan `0`, silakan:

``` {.c}
    int a[3][2] = {0};
```

Tapi rekomendasiku: kalau punya array 2D, pakai initializer 2D. Kode jadi
lebih mudah dibaca. (Kecuali untuk menginisialisasi seluruh array dengan
`0`, yang idiomatis memakai `{0}` berapa pun dimensi arraynya.)

[i[Arrays-->multidimensional initializers]>]
