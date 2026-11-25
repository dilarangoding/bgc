<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Strings

[i[Strings]<]Akhirnya! Strings! Apa yang bisa lebih sederhana dari ini?

Nah, ternyata string di C sebenarnya bukan string. Betul! Mereka itu
pointer! Tentu saja mereka pointer!

Seperti array, string di C _nyaris tidak ada_.

Tapi kita lihat saja---sebenarnya tidak seseram itu.

## String Literal

[i[String literals]<]Sebelum mulai, mari kita bahas dulu string literal di
C. String literal berupa deretan karakter di dalam tanda kutip _ganda_ (`"`). (Tanda
kutip tunggal mengapit karakter, dan itu makhluk yang sama sekali
berbeda.)

Contoh:

``` {.c}
"Hello, world!\n"
"This is a test."
"When asked if this string had quotes in it, she replied, \"It does.\""
```

Yang pertama punya newline di ujungnya---pemandangan yang sangat umum.

Yang terakhir punya tanda kutip di dalamnya, dan kamu bisa lihat masing-masing
didahului (kita bilang "di-escape oleh") sebuah backslash (`\`) yang
menandakan bahwa di titik itu ada tanda kutip literal yang menjadi bagian
string. Begitulah cara C compiler membedakan antara mencetak tanda kutip
ganda dan tanda kutip ganda penutup string.[i[String literals]>]

## Variabel String

[i[String variables]<]Setelah tahu cara membuat string literal, sekarang kita
tugaskan ke sebuah variabel supaya bisa kita pakai.

``` {.c}
char *s = "Hello, world!";
```

Perhatikan tipenya: pointer ke `char`. Variabel string `s` sebenarnya
adalah pointer ke karakter pertama string itu, yaitu `H`.

Kita bisa mencetaknya dengan format specifier `%s` (untuk "string"):

``` {.c}
char *s = "Hello, world!";

printf("%s\n", s);  // "Hello, world!"
```
[i[String variables]>]

## Variabel String sebagai Array

[i[String variables-->as arrays]<]
Pilihan lainnya adalah ini, yang hampir setara dengan pemakaian `char*`
di atas:

``` {.c}
char s[14] = "Hello, world!";

// atau, kalau kita malas dengan benar dan menyuruh compiler
// menghitung panjangnya untuk kita:

char s[] = "Hello, world!";
```

Artinya, kamu bisa memakai notasi array untuk mengakses karakter dalam
string. Mari kita lakukan persis itu untuk mencetak semua karakter string
dalam satu baris:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[] = "Hello, world!";

    for (int i = 0; i < 13; i++)
        printf("%c", s[i]);
    printf("\n");
}
```

Perhatikan bahwa kita memakai format specifier `%c` untuk mencetak satu
karakter.

Lihat juga ini. Programnya tetap berjalan dengan baik kalau definisi `s`
kita ubah menjadi tipe `char*`:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char *s = "Hello, world!";   // char* di sini

    for (int i = 0; i < 13; i++)
        printf("%c", s[i]);    // Tapi masih memakai array di sini...?
    printf("\n");
}
```

Dan kita masih bisa memakai notasi array untuk mencetaknya! Ini mengejutkan,
tapi itu hanya karena kita belum membahas kesetaraan array/pointer. Meski
begitu, ini satu petunjuk lagi bahwa array dan pointer itu sama saja, jauh
di dalamnya.
[i[String variables-->as arrays]>]

## Inisialisasi String

[i[Strings-->initializers]<]
Kita sudah melihat beberapa contoh inisialisasi variabel string dengan
string literal:

``` {.c}
char *s = "Hello, world!";
char t[] = "Hello, again!";
```

Tapi kedua inisialisasi ini berbeda secara halus. Sebuah string literal,
seperti integer literal, memorinya dikelola otomatis oleh compiler untukmu!
Untuk integer, yaitu data berukuran tetap, compiler bisa mengelolanya dengan
mudah. Tapi string adalah makhluk berukuran byte yang bervariasi, yang
dijinakkan compiler dengan cara dilempar ke sepotong memori, lalu kamu
diberi pointer ke sana.

Bentuk ini menunjuk ke mana pun string itu ditaruh. Biasanya tempat itu
berada di negeri yang jauh dari memori program lainnya -- yaitu memori
read-only -- karena alasan performa dan keamanan.

``` {.c}
char *s = "Hello, world!";
```

Jadi, kalau kamu mencoba mengubah string itu dengan ini:

``` {.c}
char *s = "Hello, world!";

s[0] = 'z';  // KABAR BURUK: mencoba mengubah string literal!
```

Perilakunya undefined. Tergantung sistemmu, kemungkinan besar hasilnya
crash.

Tapi mendeklarasikannya sebagai array itu berbeda. Compiler tidak
menyembunyikan byte-byte itu di bagian kota yang lain; mereka ada di
jalan seberang rumahmu. Yang satu ini _salinan_ string yang bisa diubah
-- bisa kita ubah sesuka hati:

``` {.c}
char t[] = "Hello, again!";  // t adalah salinan array dari string itu
t[0] = 'z'; //  Tidak masalah

printf("%s\n", t);  // "zello, again!"
```

Jadi ingat: kalau kamu punya pointer ke string literal, jangan coba
mengubahnya! Dan kalau kamu memakai string di dalam tanda kutip ganda
untuk menginisialisasi array, itu sebenarnya bukan string literal.
[i[Strings-->initializers]>]

## Mendapatkan Panjang String

[i[Strings-->getting the length]<]
Tidak bisa, karena C tidak mencatatnya untukmu. Dan kalau dibilang "tidak
bisa", maksudnya sebenarnya "bisa"^[Walaupun memang benar bahwa C tidak
mencatat panjang string.]. Ada fungsi di `<string.h>` bernama `strlen()`
yang bisa dipakai untuk menghitung panjang string apa pun dalam
byte^[Kalau kamu memakai character set dasar atau character set 8-bit,
kamu terbiasa dengan satu karakter sama dengan satu byte. Tapi itu tidak
berlaku di semua character encoding.].

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char *s = "Hello, world!";

    printf("The string is %zu bytes long.\n", strlen(s));
}
```

Fungsi `strlen()` mengembalikan tipe `size_t`, yaitu tipe integer sehingga
bisa dipakai untuk perhitungan integer. Kita mencetak `size_t` dengan
`%zu`.

Program di atas mencetak:

``` {.default}
The string is 13 bytes long.
```

Bagus! Jadi ternyata _bisa_ mendapatkan panjang string!
[i[Strings-->getting the length]>]

Tapi... kalau C tidak mencatat panjang string di mana pun, bagaimana ia
tahu seberapa panjang string itu?

## Terminasi String

[i[Strings-->termination]<]
C memperlakukan string sedikit berbeda dari banyak bahasa pemrograman, dan
sebenarnya berbeda dari hampir semua bahasa pemrograman modern.

Saat membuat bahasa baru, pada dasarnya ada dua pilihan untuk menyimpan
string di memori:

1. Simpan byte-byte string beserta sebuah angka yang menyatakan panjang
   string.

2. Simpan byte-byte string, dan tandai akhir string dengan byte khusus
   yang disebut _terminator_.

Kalau kamu ingin string yang lebih panjang dari 255 karakter, pilihan 1
membutuhkan setidaknya dua byte untuk menyimpan panjangnya. Sedangkan
pilihan 2 hanya butuh satu byte untuk mengakhiri string. Jadi ada sedikit
penghematan di sana.

Tentu saja, zaman sekarang menghemat satu byte (atau 3---banyak bahasa
dengan senang hati membolehkan string sepanjang 4 gigabyte) terasa konyol.
Tapi dulu, itu urusan yang lebih besar.

Jadi C memilih pendekatan #2. Di C, sebuah "string" didefinisikan oleh dua
ciri dasar:

* Sebuah pointer ke karakter pertama string.
* Sebuah byte bernilai nol (atau karakter `NUL`^[Ini berbeda dari pointer
  `NULL`, dan penulis menyingkatnya `NUL` kalau membicarakan karakternya
  dan `NULL` untuk pointer-nya.]) di suatu tempat di memori setelah pointer
  itu, yang menandai akhir string.

Karakter `NUL` bisa ditulis di kode C sebagai `\0`, meski jarang kamu
perlu melakukannya.

Ketika kamu menulis string di dalam tanda kutip ganda di kodemu, karakter
`NUL` otomatis dan implisit ikut disertakan.

``` {.c}
char *s = "Hello!";  // Sebenarnya "Hello!\0" di balik layar
```

Dengan ini, mari kita tulis fungsi `strlen()` kita sendiri yang menghitung
`char` dalam string sampai menemukan `NUL`.

Caranya, telusuri string untuk mencari satu karakter `NUL`, sambil
menghitung^[Nanti kita akan belajar cara yang lebih rapi dengan pointer
arithmetic.]:

``` {.c}
int my_strlen(char *s)
{
    int count = 0;

    while (s[count] != '\0')  // Tanda kutip tunggal untuk satu karakter
        count++;

    return count;
}
```

Dan pada dasarnya begitulah cara `strlen()` bawaan menyelesaikan tugasnya.
[i[Strings-->termination]>]

## Menyalin String

[i[Strings-->copying]<]
String tidak bisa disalin lewat operator penugasan (`=`). Yang terjadi
hanyalah menyalin pointer ke karakter pertama... sehingga akhirnya kamu
punya dua pointer ke string yang sama:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[] = "Hello, world!";
    char *t;

    // Ini menyalin pointer-nya, bukan menyalin string-nya!
    t = s;

    // Kita ubah t
    t[0] = 'z';

    // Tapi mencetak s menampilkan perubahannya!
    // Karena t dan s menunjuk ke string yang sama!

    printf("%s\n", s);  // "zello, world!"
}
```

Kalau kamu ingin menyalin string, kamu harus menyalinnya byte demi
byte---artinya kamu mengambil byte-byte string dari satu tempat di memori
dan menduplikasinya ke tempat lain di memori. Ini dipermudah oleh fungsi
`strcpy()`.[^440b]

[^440b]: Ada fungsi lain bernama `strncpy()` yang membatasi jumlah byte yang
    disalin. Sebagian orang bilang kamu harus selalu memakai `strncpy()`
    karena ada perlindungan dari buffer overrun. Sebagian lain bilang
    jangan pernah memakai `strncpy()` karena ia belum tentu mengakhiri
    string-mu, footgun mengerikan lainnya. Kalau mau benar-benar aman,
    kamu bisa menulis versi `strncpy()` sendiri yang selalu mengakhiri
    string.

Sebelum menyalin string, pastikan ada ruang untuk menampung salinannya,
yaitu array tujuan yang akan menampung karakter-karakternya harus
setidaknya sepanjang string yang disalin.

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[] = "Hello, world!";
    char t[100];  // Setiap char satu byte, jadi ruangnya lebih dari cukup

    // Ini menyalin string-nya!
    strcpy(t, s);

    // Kita ubah t
    t[0] = 'z';

    // Dan s tidak terpengaruh karena itu string yang berbeda
    printf("%s\n", s);  // "Hello, world!"

    // Tapi t sudah berubah
    printf("%s\n", t);  // "zello, world!"
}
```

Perhatikan bahwa pada `strcpy()`, pointer tujuan adalah argumen pertama,
dan pointer sumber adalah argumen kedua. Trik untuk mengingatnya: ini
urutan yang akan kamu pakai untuk menulis `t` dan `s` seandainya
penugasan `=` bekerja untuk string, dengan sumber di kanan dan tujuan di
kiri.
[i[Strings-->copying]>][i[Strings]>]
