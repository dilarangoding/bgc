<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Variabel dan Pernyataan

> _"It takes all kinds to make a world, does it not, Padre?"_ \
> _"So it does, my son, so it does."_
>
> ---Pirate Captain Thomas Bartholomew Red to the Padre, Pirates

Program C memang berisi banyak hal.

Yap.

Dan karena berbagai alasan, kita semua akan terbantu kalau
mengelompokkan jenis-jenis hal yang ada dalam sebuah program, supaya
jelas apa yang sedang dibicarakan.

## Variabel

[i[variabel]<]Orang bilang, "variabel menyimpan nilai". Tapi cara lain
memandangnya: variabel adalah nama yang mudah dibaca manusia untuk merujuk
ke sebuah data di memori.

Kita berhenti sebentar di sini untuk mengintip ke lubang kelinci bernama
pointer. Jangan khawatir dulu.

Bayangkan memori sebagai array raksasa berisi byte^[Satu "byte" biasanya
adalah bilangan biner 8-bit. Bayangkan sebagai integer yang hanya bisa
menyimpan nilai 0 sampai 255, inklusif. Secara teknis, C membolehkan byte
punya jumlah bit berapa pun, dan kalau ingin merujuk dengan jelas ke
bilangan 8-bit, istilah yang tepat adalah _oktet_. Tapi programmer akan
menganggap kamu bermaksud 8-bit kalau bilang "byte", kecuali kamu
menyebutkan yang lain.]. Data disimpan di dalam "array" ini^[Cara kerja
memori modern di sini disederhanakan sedikit berlebihan. Tapi model mental
ini cukup berhasil, jadi mohon dimaklumi.]. Kalau sebuah angka lebih besar
dari satu byte, angka itu disimpan dalam beberapa byte. Karena memori mirip
array, setiap byte memori bisa dirujuk lewat indeksnya. Indeks ke dalam
memori ini juga disebut _alamat_, atau _lokasi_, atau _pointer_.

Ketika kamu punya variabel di C, nilainya berada di memori _di suatu
tempat_, pada suatu alamat. Tentu saja. Memangnya di mana lagi? Tapi
merujuk ke sebuah nilai lewat alamat numeriknya itu merepotkan, jadi kita
memberinya nama, dan itulah yang disebut variabel.

Ada dua alasan kenapa hal ini dibahas:

1. Ini memudahkan pemahaman variabel pointer nanti---variabel yang
   menyimpan alamat variabel lain!
2. Dan, ini juga memudahkan pemahaman pointer nanti.

Jadi, variabel adalah nama untuk data yang disimpan di memori pada suatu
alamat.

### Nama Variabel

[i[variabel]<]Nama variabel boleh memakai karakter apa pun dalam rentang
0-9, A-Z, a-z, dan underscore, dengan aturan berikut:

* Nama variabel tidak boleh diawali digit 0-9.
* Nama variabel tidak boleh diawali dua underscore.
* Nama variabel tidak boleh diawali underscore yang diikuti huruf kapital
  A-Z.

Untuk Unicode, coba saja. Spesifikasi di §D.2 punya sejumlah aturan tentang
rentang codepoint Unicode mana yang boleh dipakai di bagian mana dari
identifier, tapi itu terlalu panjang untuk ditulis di sini dan kemungkinan
besar tidak perlu kamu pikirkan.[i[variabel]>]

### Tipe Variabel

[i[tipe]]Tergantung bahasa apa yang ada di gudang senjatamu, kamu mungkin
sudah akrab, mungkin juga belum, dengan konsep tipe. Tapi C cukup ketat
soal ini, jadi kita perlu mengulasnya sedikit.

Berikut beberapa contoh tipe, yang paling dasar saja:

[i[`int` tipe]][i[`float` tipe]][i[`char` tipe]][i[`char *` tipe]]

|Tipe|Contoh|Tipe C|
|:---|------:|:-----|
|Integer|`3490`|`int`|
|Floating point|`3.14159`|`float`^[Ini sedikit bohong. Secara teknis `3.14159` bertipe `double`, tapi kita belum sampai ke sana, dan tujuannya adalah supaya kamu mengasosiasikan `float` dengan "Floating Point". C dengan senang hati akan mengonversinya menjadi `float`. Singkatnya, jangan dipusingkan dulu.]|
|Karakter (tunggal)|`'c'`|`char`|
|String|`"Hello, world!"`|`char *`^[Bacanya "pointer ke char" atau "char pointer". "Char" kependekan dari karakter. Belum ada studi resmi, tapi secara anekdot kebanyakan orang melafalkannya "char" (seperti "charcoal"), sebagian kecil bilang "car", dan segelintir bilang "care". Pointer akan dibahas lebih lanjut nanti.]|

C berusaha mengonversi secara otomatis antara kebanyakan tipe numerik kalau
diminta. Selain itu, semua konversi dilakukan manual, terutama antara
string dan numerik.

Hampir semua tipe di C adalah varian dari tipe-tipe ini.

Sebelum memakai variabel, kamu harus _mendeklarasikannya_ dan memberi tahu
C tipe apa yang akan dipegangnya. Setelah dideklarasikan, tipe variabel
tidak bisa diubah lagi saat runtime. Apa yang kamu tetapkan, itulah yang
berlaku sampai variabel keluar dari scope dan diserap kembali oleh alam
semesta.

Mari kita ambil kode "Hello, world" tadi dan tambahkan beberapa variabel:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;    // Menyimpan integer bertanda, mis. -3, -2, 0, 1, 10
    float f;  // Menyimpan bilangan floating point bertanda, mis. -3.1416

    printf("Hello, World!\n");  // Ah, keakraban yang menenangkan
}
```

Nah! Kita sudah mendeklarasikan beberapa variabel. Keduanya belum kita
pakai dan belum diinisialisasi. Yang satu menyimpan integer, yang lain
menyimpan floating point number (pada dasarnya bilangan real, kalau kamu
punya latar belakang matematika).

[i[variabel-->belum diinisialisasi]]Variabel yang belum diinisialisasi
bernilai tak tentu^[Secara informal kita bilang nilainya "acak", padahal
sebenarnya bukan---bahkan bukan bilangan pseudo-acak.]. Variabel harus
diinisialisasi, atau kamu harus menganggap isinya angka sampah.

> Di sinilah C bisa "menjebak" kamu. Seringnya, dari pengalaman, nilai yang
> tak tentu itu nol... tapi bisa berubah dari satu run ke run berikutnya!
> Jangan pernah menganggap nilainya nol, meskipun kamu melihatnya nol.
> _Selalu_ inisialisasi variabel secara eksplisit sebelum dipakai^[Ini
> tidak sepenuhnya benar. Ketika kita sampai ke pembahasan static storage
> duration, kamu akan tahu bahwa beberapa variabel otomatis diinisialisasi
> ke nol. Tapi cara yang aman adalah selalu menginisialisasinya sendiri.].

Apa nih? Kamu mau menyimpan angka di variabel itu? Gila kamu!

Ayo kita lakukan:
[i[`=` operator penugasan]]

``` {.c .numberLines}
int main(void)
{
    int i;

    i = 2; // Menugaskan nilai 2 ke variabel i

    printf("Hello, World!\n");
}
```

Mantap. Kita sudah menyimpan sebuah nilai. Sekarang kita cetak.

[i[`printf()` fungsi]<]Caranya, kita oper _dua_ argumen keren ke fungsi
`printf()`. Argumen pertama adalah string yang menjelaskan apa yang akan
dicetak dan bagaimana mencetaknya (disebut _format string_), dan argumen
kedua adalah nilai yang dicetak, yaitu apa pun isi variabel `i`.

`printf()` mencari urutan-urutan khusus dalam format string yang diawali
tanda persen (`%`), yang memberi tahu apa yang harus dicetak. Misalnya,
kalau menemukan `%d`, ia mengambil parameter berikutnya dan mencetaknya
sebagai integer. Kalau menemukan `%f`, nilainya dicetak sebagai float.
Kalau menemukan `%s`, ia mencetak string.

Dengan begitu, kita bisa mencetak nilai berbagai tipe seperti ini:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 2;
    float f = 3.14;
    char *s = "Hello, world!";  // char * ("char pointer") adalah sebuah string

    printf("%s  i = %d and f = %f!\n", s, i, f);
}
```

Outputnya akan seperti ini:

``` {.default}
Hello, world!  i = 2 and f = 3.14!
```

Dengan begitu, `printf()` mungkin mirip dengan format string atau
parameterized string di bahasa lain yang sudah kamu kenal.
[i[`printf()` fungsi]>]

***CATATAN:*** Di bagian-bagian berikut, kita anggap kamu sudah
mendeklarasikan variabelnya di awal. Kalau sebuah contoh memakai integer
`i` dan `j`, bayangkan saja ada baris ini di suatu tempat di atasnya:

``` {.c}
int i, j;
```

### Tipe Boolean

[i[tipe Boolean]<]Apakah C punya tipe Boolean, ya atau tidak?

`1`!

Secara historis, C tidak punya tipe Boolean, dan sebagian orang berpendapat
sampai sekarang pun belum.

Di C, `0` berarti "false", dan nilai bukan nol berarti "true".

Jadi `1` itu true. `-37` juga true. Dan `0` adalah false.

Kamu bisa mendeklarasikan Boolean sebagai `int` biasa:

``` {.c}
int x = 1;

if (x) {
    printf("x is true!\n");
}
```

Di C23, kita mendapat `bool`, `true`, dan `false` yang sesungguhnya.
Sebelum itu, kalau versi C-mu cukup modern, kamu bisa
`#include <stdbool.h>`[i[`stdbool.h` header file]] untuk hasil yang sama.

``` {.c .numberLines}
#include <stdio.h>
#include <stdbool.h>  // tidak diperlukan di C23

int main(void) {
    bool x = true;

    if (x) {
        printf("x is true!\n");
    }
}
```

Secara teknis, variabel `bool` seharusnya diisi `true`, `false`, atau
hasil ekspresi yang bernilai true atau false. Tapi sebenarnya kamu bisa
mengonversi berbagai hal ke `bool`. Ada aturan khusus, tapi yang bernilai
nol cenderung menjadi `false`, dan yang bukan nol menjadi true.

Hati-hati kalau keduanya dicampur, karena nilai numerik `true` kemungkinan
besar adalah `1`[^B1CA]. Kalau kamu mengandalkan nilai positif lain sebagai
true, hasilnya bisa tidak cocok. Misalnya:

``` {.c .numberLines}
printf("%d\n", true == 12);  // Mencetak "0", false!
```


[^B1CA]: Secara teknis hanya satu bit dari sebuah `char` yang dipakai untuk
    merepresentasikan `bool`, jadi nilainya hanya bisa nol atau satu.
    Hanya saja, isi bit-bit sisanya (padding) dalam `char` itu tidak
    ditentukan. Untuk `false`, pasti semuanya nol. Tapi untuk `true`,
    penulis tidak yakin apakah sisanya harus nol semua.

[i[tipe Boolean]>]

## Operator dan Ekspresi {#operators}

Operator C seharusnya sudah akrab dari bahasa lain. Kita bahas sekilas
beberapa di antaranya di sini.

(Detailnya masih lebih banyak dari ini, tapi bagian ini sudah cukup untuk
memulai.)


### Aritmetika

[i[Operator Aritmetika]()]Semoga ini sudah akrab:
[i[`+` operator penjumlahan]] [i[`-` operator pengurangan]]
[i[`*` operator perkalian]] [i[`/` operator pembagian]]
[i[`%` operator modulus]]

``` {.c}
i = i + 3;  // Operator penjumlahan (+) dan penugasan (=), tambahkan 3 ke i
i = i - 8;  // Pengurangan, kurangi i dengan 8
i = i * 9;  // Perkalian
i = i / 2;  // Pembagian
i = i % 5;  // Modulo (sisa pembagian)
```

Semua itu punya varian singkat. Setiap baris di atas bisa ditulis lebih
ringkas sebagai:
[i[`+=` operator penugasan]] [i[`-=` operator penugasan]]
[i[`*=` operator penugasan]] [i[`/=` operator penugasan]]
[i[`%=` operator penugasan]]

``` {.c}
i += 3;  // Sama dengan "i = i + 3", tambahkan 3 ke i
i -= 8;  // Sama dengan "i = i - 8"
i *= 9;  // Sama dengan "i = i * 9"
i /= 2;  // Sama dengan "i = i / 2"
i %= 5;  // Sama dengan "i = i % 5"
```

Tidak ada operator pangkat. Kamu harus memakai salah satu varian fungsi
`pow()`[i[pow()]T] dari `math.h`.

Sekarang kita masuk ke hal-hal yang lebih aneh, yang mungkin tidak ada di
bahasa lain![i[Operator Aritmetika]>]

### Operator Ternary

[i[`?:` operator ternary]<]C juga punya _operator ternary_, yaitu
ekspresi yang nilainya bergantung pada hasil sebuah kondisional yang
tertanam di dalamnya.

``` {.c}
// Jika x > 10, tambahkan 17 ke y. Jika tidak, tambahkan 37 ke y.

y += x > 10? 17: 37;
```

Berantakan sekali! Tapi kamu akan terbiasa seiring sering membacanya.
Supaya lebih jelas, ekspresi di atas bisa ditulis ulang dengan pernyataan
`if`:

``` {.c}
// Ekspresi ini:

y += x > 10? 17: 37;

// setara dengan non-ekspresi ini:

if (x > 10)
    y += 17;
else
    y += 37;
```

Bandingkan keduanya sampai kamu bisa melihat masing-masing komponen
operator ternary.

Atau contoh lain, yang mencetak apakah angka di `x` ganjil atau genap:

``` {.c}
printf("The number %d is %s.\n", x, x % 2 == 0? "even": "odd");
```

Format specifier `%s` di `printf()`[i[printf()]T] berarti "cetak string".
Kalau ekspresi `x % 2` bernilai `0`, nilai seluruh ekspresi ternary adalah
string `"even"`. Kalau tidak, nilainya string `"odd"`. Keren sekali!

Perlu diingat bahwa operator ternary bukan kendali alur seperti pernyataan
`if`. Ia hanyalah ekspresi yang menghasilkan sebuah nilai.
[i[`?:` operator ternary]>]

### Pre-dan-Post Increment-dan-Decrement

[i[`++` operator increment]<] [i[`--` operator decrement]<]
Sekarang kita bahas hal lain yang mungkin belum pernah kamu lihat.

Inilah operator post-increment dan post-decrement yang legendaris:

``` {.c}
i++;        // Tambahkan satu ke i (post-increment)
i--;        // Kurangi i dengan satu (post-decrement)
```

Operator ini sangat umum dipakai sebagai versi singkat dari:

``` {.c}
i += 1;        // Tambahkan satu ke i
i -= 1;        // Kurangi i dengan satu
```

tapi perbedaannya lebih halus dari itu, dasar penipu cerdas.

Sekarang varian lainnya, pre-increment dan pre-decrement:

``` {.c}
++i;        // Tambahkan satu ke i (pre-increment)
--i;        // Kurangi i dengan satu (pre-decrement)
```

Pada pre-increment dan pre-decrement, nilai variabel di-increment atau
di-decrement _sebelum_ ekspresi dievaluasi. Setelah itu, ekspresi dievaluasi
dengan nilai yang baru.

Pada post-increment dan post-decrement, nilai ekspresi dihitung lebih dulu
dengan nilai variabel apa adanya, dan _baru kemudian_ variabelnya
di-increment atau di-decrement.

Kedua operator ini bisa disematkan dalam ekspresi, seperti ini:

``` {.c}
i = 10;
j = 5 + i++;  // Hitung 5 + i, _baru kemudian_ increment i

printf("%d, %d\n", i, j);  // Mencetak 11, 15
```

Bandingkan dengan operator pre-increment:

``` {.c}
i = 10;
j = 5 + ++i;  // Increment i, _baru kemudian_ hitung 5 + i

printf("%d, %d\n", i, j);  // Mencetak 11, 16
```

Teknik ini sering dipakai pada akses dan manipulasi array dan pointer.
Dengannya kamu bisa memakai nilai sebuah variabel sekaligus
meng-increment atau men-decrement nilai itu, sebelum atau sesudah
dipakai.

Tapi sejauh ini, tempat yang paling sering memakainya adalah loop `for`:

``` {.c}
for (i = 0; i < 10; i++)
    printf("i is %d\n", i);
```

Soal itu kita bahas nanti.
[i[`++` operator increment]>] [i[`--` operator decrement]>]

### Operator Koma

[i[`,` operator koma]<]
Ini cara yang jarang dipakai untuk memisahkan ekspresi yang akan
dijalankan dari kiri ke kanan:

``` {.c}
x = 10, y = 20;  // Mula-mula tugaskan 10 ke x, lalu 20 ke y
```

Kelihatannya agak konyol, karena koma itu bisa saja diganti titik koma,
kan?

``` {.c}
x = 10; y = 20;  // Mula-mula tugaskan 10 ke x, lalu 20 ke y
```

Tapi keduanya sedikit berbeda. Yang terakhir berupa dua ekspresi terpisah,
sedangkan yang pertama hanya satu ekspresi tunggal!

Dengan operator koma, nilai ekspresi koma adalah nilai ekspresi yang
paling kanan:

``` {.c}
x = (1, 2, 3);

printf("x is %d\n", x);  // Mencetak 3, karena 3 adalah yang paling kanan di daftar koma
```

Tapi itu pun terasa dipaksakan. Tempat yang umum untuk operator koma adalah
loop `for`, untuk melakukan beberapa hal sekaligus di setiap bagiannya:

``` {.c}
for (i = 0, j = 10; i < 100; i++, j++)
    printf("%d, %d\n", i, j);
```

Kita akan kembali ke sana nanti.
[i[`,` operator koma]>]

### Operator Kondisional

[i[Operator Kondisional]<]
Untuk nilai Boolean, tersedia banyak operator standar:
[i[operator perbandingan]] [i[`==` operator kesamaan]]
[i[`!=` operator ketidaksamaan]] [i[`<` operator kurang dari]]
[i[`>` operator lebih dari]] [i[`<=` operator kurang dari atau sama dengan]]
[i[`>=` operator lebih dari atau sama dengan]]

``` {.c}
a == b;  // True jika a sama dengan b
a != b;  // True jika a tidak sama dengan b
a < b;   // True jika a kurang dari b
a > b;   // True jika a lebih dari b
a <= b;  // True jika a kurang dari atau sama dengan b
a >= b;  // True jika a lebih dari atau sama dengan b
```

Jangan tertukar antara penugasan (`=`) dan perbandingan (`==`)! Pakai dua
tanda sama dengan untuk membandingkan, satu untuk penugasan.

Ekspresi perbandingan bisa dipakai dalam pernyataan `if`:

``` {.c}
if (a <= 10)
    printf("Success!\n");
```
[i[Operator Kondisional]>]

### Operator Boolean

[i[Operator Boolean]<]
Ekspresi kondisional bisa dirangkai atau diubah dengan operator Boolean
untuk _dan_, _atau_, dan _tidak_.
[i[`&&` boolean AND]]
[i[`!` boolean NOT]]
[i[`||` boolean OR]]

|Operator|Makna Boolean|
|:------:|:-------------:|
|`&&`|and|
|`||`|or|
|`!`|not|

Contoh Boolean "and":

``` {.c}
// Lakukan sesuatu jika x kurang dari 10 dan y lebih dari 20:

if (x < 10 && y > 20)
    printf("Doing something!\n");
```

Contoh Boolean "not":

``` {.c}
if (!(x < 12))
    printf("x is not less than 12\n");
```

`!` punya prioritas (precedence) lebih tinggi daripada operator Boolean lainnya, jadi
dalam kasus ini kita harus memakai tanda kurung.

Tentu saja, itu sama saja dengan:

``` {.c}
if (x >= 12)
    printf("x is not less than 12\n");
```

tapi namanya juga contoh!
[i[Operator Boolean]>]

### Operator `sizeof` {#sizeof-operator}

[i[`sizeof` operator]<]
Operator ini memberi tahu ukuran (dalam byte) yang dipakai sebuah variabel
atau tipe data di memori.

Lebih tepatnya, ia memberi tahu ukuran (dalam byte) yang dipakai oleh
_tipe dari suatu ekspresi_ (yang bisa saja hanya satu variabel) di memori.

Ukurannya bisa berbeda di tiap sistem, kecuali untuk `char`[i[`char` tipe]]
dan variannya (yang selalu 1 byte).

Mungkin sekarang ini belum terlihat berguna, tapi kita akan sering
merujuknya, jadi layak dibahas.

Karena `sizeof` menghitung jumlah byte yang dibutuhkan untuk menyimpan
sebuah tipe, kamu mungkin mengira ia mengembalikan `int`. Atau... karena
ukuran tidak mungkin negatif, mungkin `unsigned`?

[i[`size_t` tipe]<]
Ternyata C punya tipe khusus untuk menampung nilai kembalian `sizeof`,
yaitu `size_t`, dibaca "_size tee_"^[`_t` kependekan dari `type`.]. Yang
kita tahu hanyalah bahwa ini tipe integer tak bertanda (unsigned) yang
sanggup menyimpan ukuran dalam byte dari apa pun yang bisa diberikan ke
`sizeof`.

`size_t` muncul di banyak tempat yang melibatkan jumlah sesuatu, baik
sebagai argumen maupun nilai kembalian. Anggap saja ia nilai yang
menyatakan "banyaknya".
[i[`size_t` tipe]>]

`sizeof` bisa diterapkan pada variabel atau ekspresi:

``` {.c}
int a = 999;

// %zu adalah format specifier untuk tipe size_t
// Jika compiler-mu menolak bagian "z", buang saja

printf("%zu\n", sizeof a);      // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof(2 + 7)); // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof 3.14);   // Mencetak 8 di sistem penulis

// Jika perlu mencetak nilai size_t negatif, pakai %zd
```

Ingat: yang dihasilkan adalah ukuran dalam byte dari _tipe_ ekspresi, bukan
ukuran ekspresi itu sendiri. Itulah sebabnya ukuran `2+7` sama dengan
ukuran `a`---keduanya bertipe `int`. Kita akan kembali ke angka `4` ini di
blok kode berikutnya...

...yang menunjukkan bahwa `sizeof` juga bisa diterapkan pada sebuah tipe
(perhatikan bahwa tanda kurung wajib di sekitar nama tipe, tidak seperti
pada ekspresi):

``` {.c}
printf("%zu\n", sizeof(int));   // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof(char));  // Mencetak 1 di semua sistem
```

Perlu dicatat bahwa `sizeof` adalah operasi _compile-time_^[Kecuali untuk
variable length array---tapi itu cerita untuk lain waktu.]. Hasilnya
ditentukan sepenuhnya saat compile-time, bukan saat runtime.

Kita akan memakainya nanti.
[i[`sizeof` operator]>]


## Kendali Alur

[i[Kendali Alur]<]
Boolean memang bagus, tapi kita tidak akan ke mana-mana kalau tidak bisa
mengendalikan alur program. Mari kita lihat beberapa konstruksi: `if`,
`for`, `while`, dan `do-while`.

Pertama, catatan umum tentang pernyataan dan blok pernyataan, dipersembahkan
oleh developer C lokal yang ramah ini:

Setelah pernyataan seperti `if` atau `while`, kamu bisa menaruh satu
pernyataan tunggal untuk dieksekusi, atau sebuah blok pernyataan yang semuanya
dieksekusi berurutan.

[i[`if` pernyataan]<]
Kita mulai dengan satu pernyataan:

``` {.c}
if (x == 10) printf("x is 10\n");
```

Ini juga kadang ditulis di baris terpisah. (Whitespace hampir tidak
berpengaruh di C---tidak seperti Python.)

``` {.c}
if (x == 10)
    printf("x is 10\n");
```

Tapi bagaimana kalau kamu ingin beberapa hal terjadi sekaligus karena satu
kondisi? Pakai kurung kurawal untuk menandai sebuah _blok_ atau
_pernyataan majemuk_.

``` {.c}
if (x == 10) {
    printf("x is 10\n");
    printf("And also this happens when x is 10\n");
}
```

Gaya yang sangat umum adalah _selalu_ memakai kurung kurawal, meskipun
sebenarnya tidak perlu:

``` {.c}
if (x == 10) {
    printf("x is 10\n");
}
```

Sebagian developer merasa kodenya lebih mudah dibaca, dan terhindar dari
error seperti ini, ketika baris yang tampak berada di dalam blok `if`
ternyata tidak.

``` {.c}
// CONTOH ERROR BURUK

if (x == 10)
    printf("This happens if x is 10\n");
    printf("This happens ALWAYS\n");  // Kejutan!! Tanpa kondisi!
```

`while`, `for`, dan konstruksi loop lainnya bekerja dengan cara yang sama
seperti contoh di atas. Kalau ingin melakukan beberapa hal dalam loop atau
setelah `if`, bungkus semuanya dalam kurung kurawal.

Dengan kata lain, `if` menjalankan satu hal yang ada setelahnya. Satu hal
itu bisa berupa satu pernyataan atau satu blok pernyataan.
[i[`if` pernyataan]>]


### Pernyataan `if`-`else` {#ifstat}

[i[`if`-`else` pernyataan]<]
Kita sudah memakai `if` di beberapa contoh, karena kemungkinan besar kamu
sudah pernah melihatnya di bahasa lain, tapi ini satu contoh lagi:

``` {.c}
int i = 10;

if (i > 10) {
    printf("Yes, i is greater than 10.\n");
    printf("And this will also print if i is greater than 10.\n");
}

if (i <= 10) printf("i is less than or equal to 10.\n");
```

Pada kode contoh, pesan dicetak kalau `i` lebih besar dari 10, kalau
tidak eksekusi lanjut ke baris berikutnya. Perhatikan kurung kurawal
setelah pernyataan `if`; kalau kondisinya true, yang dieksekusi adalah
pernyataan tunggal tepat setelah `if`, atau sekumpulan kode dalam kurung
kurawal setelah `if`. Perilaku _blok kode_ seperti ini berlaku untuk semua
pernyataan.

Tentu saja, karena C menyenangkan dengan caranya sendiri, kamu juga bisa
melakukan sesuatu saat kondisinya false lewat klausa `else` pada `if`:

``` {.c}
int i = 99;

if (i == 10)
    printf("i is 10!\n");

else {
    printf("i is decidedly not 10.\n");
    printf("Which irritates me a little, frankly.\n");
}
```

Kamu bahkan bisa merangkainya secara bertingkat untuk menguji berbagai
kondisi, seperti ini:

``` {.c}
int i = 99;

if (i == 10)
    printf("i is 10!\n");

else if (i == 20)
    printf("i is 20!\n");

else if (i == 99) {
    printf("i is 99! My favorite\n");
    printf("I can't tell you how happy I am.\n");
    printf("Really.\n");
}
    
else
    printf("i is some crazy number I've never heard of.\n");
```

Kalau kamu sampai menempuh jalan itu, coba lihat pernyataan
[`switch`](#switch-statement) yang mungkin solusi lebih baik. Syaratnya,
`switch` hanya bisa membandingkan kesamaan dengan angka konstan. Rangkaian
`if`-`else` di atas bisa memeriksa ketidaksamaan, rentang, variabel, atau
apa pun yang bisa dibuat dalam ekspresi kondisional.
[i[`if`-`else` pernyataan]>]

### Pernyataan `while` {#whilestat}

[i[`while` pernyataan]<]
`while` adalah konstruksi loop biasa yang lazim. Lakukan sesuatu selama
ekspresi kondisinya bernilai true.

Yuk, coba!

``` {.c}
// Cetak output berikut:
//
//   i is now 0!
//   i is now 1!
//   [ sama saja untuk 2 sampai 7 ]
//   i is now 8!
//   i is now 9!

int i = 0;

while (i < 10) {
    printf("i is now %d!\n", i);
    i++;
}

printf("All done!\n");
```

Itulah loop dasar. C juga punya loop `for` yang akan membuat contoh tadi
lebih rapi.

Pemakaian `while` yang cukup sering adalah infinite loop, yaitu mengulang
selama true:

``` {.c}
while (1) {
    printf("1 is always true, so this repeats forever.\n");
}
```
[i[`while` pernyataan]>]


### Pernyataan `do`-`while` {#dowhilestat}

[i[`do`-`while` pernyataan]<]
Setelah menguasai pernyataan `while`, mari kita lihat saudara dekatnya,
`do`-`while`.

Keduanya pada dasarnya sama, kecuali kalau kondisi loop bernilai false pada
pass pertama: `do`-`while` tetap dieksekusi sekali, sedangkan `while` tidak
dieksekusi sama sekali. Dengan kata lain, pengujian apakah blok akan
dieksekusi terjadi di _akhir_ blok pada `do`-`while`, dan di _awal_ blok
pada `while`.

Kita lihat lewat contoh:

``` {.c}
// Memakai pernyataan while:

i = 10;

// ini tidak dieksekusi karena i tidak kurang dari 10:
while(i < 10) {
    printf("while: i is %d\n", i);
    i++;
}

// Memakai pernyataan do-while:

i = 10;

// ini dieksekusi sekali, karena kondisi loop baru diperiksa setelah
// badan loop dijalankan:

do {
    printf("do-while: i is %d\n", i);
    i++;
} while (i < 10);

printf("All done!\n");
```

Perhatikan bahwa pada kedua kasus, kondisi loop langsung bernilai false.
Pada `while`, kondisinya gagal, sehingga blok kodenya tidak pernah
dieksekusi. Tapi pada `do`-`while`, kondisi diperiksa _setelah_ blok kode
dieksekusi, jadi blok itu selalu berjalan setidaknya sekali. Di sini,
blok mencetak pesan, meng-increment `i`, lalu kondisinya gagal, dan program
lanjut ke output "All done!".

Intinya: kalau kamu ingin loop dieksekusi setidaknya sekali, apa pun
kondisinya, pakai `do`-`while`.

Semua contoh ini sebenarnya lebih cocok memakai loop `for`. Mari kita coba
sesuatu yang kurang deterministik---ulangi sampai muncul bilangan acak
tertentu!

``` {.c .numberLines}
#include <stdio.h>   // Untuk printf
#include <stdlib.h>  // Untuk rand

int main(void)
{
    int r;

    do {
        r = rand() % 100; // Ambil bilangan acak antara 0 dan 99
        printf("%d\n", r);
    } while (r != 37);    // Ulangi sampai 37 muncul
}
```

Catatan sampingan: pernahkah kamu menjalankannya lebih dari sekali? Kalau
ya, apakah kamu melihat urutan angka yang sama muncul lagi. Dan lagi. Dan
lagi? Itu karena `rand()` adalah pseudo-random number generator yang harus
di-_seed_ dengan angka berbeda supaya menghasilkan urutan yang berbeda.
Lihat fungsi
[fl[`srand()`|https://beej.us/guide/bgclr/html/split/stdlib.html#man-srand]]
untuk detail lebih lanjut. [i[`do`-`while` pernyataan]>]

### Pernyataan `for` {#forstat}

[i[`for` pernyataan]<]
Selamat datang di salah satu loop paling populer di dunia: loop `for`!

Loop ini bagus kalau kamu sudah tahu jumlah iterasinya di awal.

Hal yang sama bisa dilakukan hanya dengan loop `while`, tapi loop `for`
membuat kode lebih rapi.

Berikut dua potongan kode yang setara---perhatikan bahwa loop `for` hanyalah
bentuk yang lebih ringkas:

``` {.c}
// Cetak angka antara 0 dan 9, inklusif...

// Memakai pernyataan while:

i = 0;
while (i < 10) {
    printf("i is %d\n", i);
    i++;
}

// Lakukan hal yang persis sama dengan for-loop:

for (i = 0; i < 10; i++) {
    printf("i is %d\n", i);
}
```

Betul, kawan---keduanya melakukan hal yang persis sama. Tapi kamu bisa
lihat sendiri bahwa pernyataan `for` sedikit lebih ringkas dan enak
dipandang. (Pengguna JavaScript pasti paham betul asal-usul C-nya di
titik ini.)

Pernyataan `for` terbagi menjadi tiga bagian yang dipisahkan titik koma.
Bagian pertama berisi inisialisasi, bagian kedua kondisi loop, dan bagian
ketiga apa yang dijalankan di akhir blok kalau kondisi loop bernilai
true. Ketiga bagian ini opsional.

``` {.c}
for (initialize things; loop if this is true; do this after each loop)
```

Perhatikan bahwa loop tidak akan dieksekusi sama sekali kalau kondisi loop
sejak awal bernilai false.

> **Fakta menarik tentang loop `for`!**
>
> Kamu bisa memakai operator koma untuk melakukan beberapa hal di setiap
> klausa loop `for`!
>
> ``` {.c}
> for (i = 0, j = 999; i < 10; i++, j--) {
>     printf("%d, %d\n", i, j);
> }
> ```

<!-- ` -->
`for` yang kosong berjalan selamanya:

``` {.c}
for(;;) {  // "selamanya"
    printf("I will print this again and again and again\n" );
    printf("for all eternity until the heat-death of the universe.\n");

    printf("Or until you hit CTRL-C.\n");
}
```
[i[`for` pernyataan]>]

### Pernyataan `switch` {#switch-statement}

[i[`switch` pernyataan]<]
Tergantung bahasa asalmu, kamu mungkin sudah atau belum mengenal `switch`,
atau versi di C ini mungkin lebih terbatas daripada yang biasa kamu pakai.
Pernyataan ini memungkinkan kita mengambil tindakan yang berbeda tergantung
nilai sebuah ekspresi integer.

Pada dasarnya, ekspresi dievaluasi menjadi nilai integer, lalu eksekusi
melompat ke [i[`case` pernyataan]<]`case` yang cocok dengan nilai itu, dan
berlanjut dari titik tersebut. Kalau pernyataan `break`[i[`break`
pernyataan]<] ditemukan, eksekusi melompat keluar dari `switch`.

Berikut contohnya: untuk jumlah kambing tertentu, kita cetak gambaran
seberapa banyak jumlah itu.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int goat_count = 2;

    switch (goat_count) {
        case 0:
            printf("You have no goats.\n");
            break;

        case 1:
            printf("You have a singular goat.\n");
            break;

        case 2:
            printf("You have a brace of goats.\n");
            break;

        default:
            printf("You have a bona fide plethora of goats!\n");
            break;
    }
}
```

Pada contoh itu, `switch` melompat ke `case 2` dan mengeksekusi dari sana.
Ketika (kalau) menemukan `break`, ia melompat keluar dari `switch`.
[i[`break` pernyataan]>]

Kamu juga mungkin melihat label `default`[i[`default` label]] di bagian
bawah. Inilah yang dijalankan kalau tidak ada case yang cocok.

Setiap `case`, termasuk `default`, bersifat opsional. Urutannya bebas,
tapi sangat umum `default`, kalau ada, ditaruh paling akhir.[i[`case` pernyataan]>]

Jadi, secara keseluruhan ia bekerja seperti rangkaian `if`-`else`:

``` {.c}
if (goat_count == 0)
    printf("You have no goats.\n");
else if (goat_count == 1)
    printf("You have a singular goat.\n");
else if (goat_count == 2)
    printf("You have a brace of goats.\n");
else
    printf("You have a bona fide plethora of goats!\n");
```

Dengan beberapa perbedaan utama:

* `switch` sering kali lebih cepat melompat ke kode yang benar (meski
  spesifikasi tidak menjamin hal itu).
* `if`-`else` bisa memakai kondisional relasional seperti `<` dan `>=`,
  serta floating point dan tipe lainnya, sedangkan `switch` tidak bisa.

Ada satu hal lagi tentang `switch` yang kadang kamu temui dan cukup
menarik: _fall through_.

[i[`break` pernyataan]<]
Ingat bagaimana `break` membuat kita melompat keluar dari switch?

[i[Fall through]<]
Nah, apa yang terjadi kalau kita _tidak_ `break`?

Ternyata eksekusi terus masuk ke `case` berikutnya! Demo!

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        // Fall through!
    case 2:
        printf("2\n");
        break;
    case 3:
        printf("3\n");
        break;
}
```

Kalau `x == 1`, `switch` ini mula-mula mengenai `case 1`, mencetak `1`,
lalu langsung lanjut ke baris kode berikutnya... yang mencetak `2`!

Setelah itu, akhirnya kita bertemu `break` dan melompat keluar dari
`switch`.

Kalau `x == 2`, kita hanya mengenai `case 2`, mencetak `2`, dan `break`
seperti biasa.

Ketiadaan `break` inilah yang disebut _fall through_.

Kalau kamu memakai C sebelum C23, _SELALU_ taruh komentar di kode pada
tempat yang memang kamu maksudkan untuk fall through, seperti di atas.
Dengan begitu programmer lain tidak bertanya-tanya apakah kamu sengaja
melakukannya. GCC mengenali komentar ini dan menekan peringatan.

Atau kamu bisa memakai atribut GCC non-standar, yang kurang disukai
karena jelek dan non-standar, tapi dikenali oleh GCC maupun Clang:

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        __attribute__((fallthrough));
    case 2:
        ...
```

Tapi sekarang, akhirnya, di C23 kita punya cara standar lewat atribut:

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        [[fallthrough]];
    case 2:
        ...
```

Tapi ingat, ini salah satu tempat paling umum munculnya bug dalam program
C: lupa menaruh `break` di `case`. Kamu harus menaruhnya kalau tidak ingin
langsung jatuh ke case berikutnya[^19a1].

[^19a1]: Hal ini dianggap sebagai bahaya yang begitu besar, sampai-sampai
    para perancang bahasa pemrograman Go menjadikan `break` sebagai
    perilaku bawaan; kamu harus secara eksplisit memakai pernyataan
    `fallthrough` di Go kalau ingin jatuh ke case berikutnya.

[i[Fall through]>]
[i[`break` pernyataan]>]

Tadi sudah disebut bahwa `switch` bekerja dengan tipe integer---patuhi
itu. Jangan pakai floating point atau tipe string di sana. Satu celahnya:
tipe karakter boleh dipakai karena diam-diam mereka juga integer. Jadi ini
sepenuhnya sah:

``` {.c}
char c = 'b';

switch (c) {
    case 'a':
        printf("It's 'a'!\n");
        break;

    case 'b':
        printf("It's 'b'!\n");
        break;

    case 'c':
        printf("It's 'c'!\n");
        break;
}
```

Terakhir, kamu bisa memakai [i[`enum` kata kunci]]`enum` dalam `switch`
karena `enum` juga tipe integer. Soal itu dibahas lebih lanjut di bab
`enum`.

[i[`switch` pernyataan]>] [i[`break` pernyataan]>] [i[Kendali Alur]>]
