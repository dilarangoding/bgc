<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Unicode, Wide Character, dan Semacamnya

[i[Unicode]<]

Sebelum mulai, perlu diketahui bahwa ini area yang masih aktif
berkembang di C, karena bahasa ini sedang berusaha melewati, ehm,
_sakit pertumbuhan_. Sekarang C23 sudah terbit, pembaruan di sini
kemungkinan akan ada.

Kebanyakan orang pada dasarnya hanya tertarik pada pertanyaan yang
tampak sederhana, "Bagaimana cara memakai character set ini-itu di C?"
Kita akan sampai ke sana. Tapi seperti yang akan kita lihat, mungkin itu
sudah berfungsi di sistemmu. Atau kamu mungkin harus menyerahkannya ke
library pihak ketiga.

Kita akan membahas banyak hal di bab ini---sebagian tidak bergantung
platform, dan sebagian khusus C.

Mari kita buat garis besarnya dulu:

* Latar belakang Unicode
* Latar belakang character encoding
* Source dan Execution Character Set
* Memakai Unicode dan UTF-8
* Memakai tipe karakter lain seperti `wchar_t`, `char16_t`, dan
  `char32_t`

Kita mulai menyelam!

## Apa Itu Unicode?

Dulu, di AS dan sebagian besar dunia, populer memakai encoding 7-bit atau
8-bit untuk karakter di memori. Artinya kita hanya bisa punya total 128
atau 256 karakter (termasuk karakter yang tidak bisa dicetak). Itu cukup
untuk dunia yang berpusat pada AS, tapi ternyata ada alfabet lain di luar
sana---siapa sangka? Bahasa Mandarin punya lebih dari 50.000 karakter, dan
itu tidak muat dalam satu byte.

Maka orang-orang menciptakan berbagai cara alternatif untuk
merepresentasikan character set kustom mereka sendiri. Dan itu tidak
masalah, tapi berubah menjadi mimpi buruk kompatibilitas.

Untuk lolos darinya, Unicode diciptakan. Satu character set untuk
menguasai semuanya. Ia memanjang sampai tak terhingga (secara efektif)
sehingga kita tidak akan pernah kehabisan ruang untuk karakter baru. Ia
punya huruf Mandarin, Latin, Yunani, aksara paku, simbol catur, emoji...
hampir semuanya, sungguh! Dan terus bertambah dari waktu ke waktu!

## Code Point

[i[Unicode-->code points]<]

Aku ingin membahas dua konsep di sini. Membingungkan karena keduanya
berupa angka... angka berbeda untuk hal yang sama. Tapi sabar dulu ya.

Mari kita definisikan _code point_ secara longgar sebagai nilai numerik
yang mewakili sebuah karakter. (Code point juga bisa mewakili karakter
kontrol yang tidak bisa dicetak, tapi anggap saja yang kumaksud adalah
sesuatu seperti huruf "B" atau karakter "π".)

Setiap code point mewakili satu karakter unik. Dan setiap karakter punya
code point numerik unik yang terkait dengannya.

Misalnya, di Unicode, nilai numerik 66 mewakili "B", dan 960 mewakili
"π". Pemetaan karakter lain yang bukan Unicode mungkin memakai nilai
berbeda, tapi lupakan saja dan fokus pada Unicode, masa depan!

Jadi itu satu hal: ada angka yang mewakili setiap karakter. Di Unicode,
angka-angka ini berkisar dari 0 sampai lebih dari 1 juta.

[i[Unicode-->code points]>]

Paham?

Karena kita akan sedikit membalik meja.

## Encoding

[i[Unicode-->encoding]<]

Kalau kamu ingat, byte 8-bit bisa menampung nilai 0-255, inklusif. Itu
bagus untuk "B" yang bernilai 66---muat dalam satu byte. Tapi "π" bernilai
960, dan itu tidak muat dalam satu byte! Kita butuh byte lain. Bagaimana
menyimpan semua itu di memori? Atau bagaimana dengan angka yang lebih
besar, seperti 195.024? Itu butuh beberapa byte untuk menampungnya.

Pertanyaan Besarnya: bagaimana angka-angka ini direpresentasikan di
memori? Inilah yang kita sebut _encoding_ karakter.

Jadi kita punya dua hal: yang satu adalah code point yang secara efektif
memberi tahu nomor seri sebuah karakter. Dan satu lagi encoding yang
memberi tahu bagaimana kita merepresentasikan angka itu di memori.

Ada banyak encoding. Kamu bisa membuat sendiri sekarang juga, kalau
mau^[Misalnya, kita bisa menyimpan code point dalam integer 32-bit
big-endian. Mudah! Kita baru saja menciptakan sebuah encoding! Sebenarnya
tidak; itulah yang disebut encoding UTF-32BE. Ya sudahlah---kembali
bekerja!]. Tapi kita akan melihat beberapa encoding yang benar-benar umum
dan dipakai bersama Unicode.

[i[Unicode-->UTF-8]<]
[i[Unicode-->UTF-16]<]
[i[Unicode-->UTF-32]<]

|Encoding|Deskripsi|
|:-----------------:|:--------------------------------------------------------------|
|UTF-8|Encoding berorientasi byte yang memakai jumlah byte bervariasi per karakter. Inilah yang sebaiknya dipakai.|
|UTF-16|Encoding 16-bit per karakter[^091d].|
|UTF-32|Encoding 32-bit per karakter.|

[^091d]: Kurang lebih. Secara teknis, lebarnya bervariasi---ada cara untuk
merepresentasikan code point yang lebih tinggi dari $2^{16}$ dengan
menggabungkan dua karakter UTF-16.

Pada UTF-16 dan UTF-32, urutan byte penting, jadi kamu mungkin melihat
UTF-16BE untuk big-endian dan UTF-16LE untuk little-endian. Sama untuk
UTF-32. Secara teknis, kalau tidak disebutkan, kamu harus mengasumsikan
big-endian. Tapi karena Windows memakai UTF-16 secara luas dan
little-endian, kadang itulah yang diasumsikan^[Ada karakter khusus bernama
_Byte Order Mark_ (BOM), code point 0xFEFF, yang boleh mendahului aliran
data dan menandakan endianess-nya. Tapi ini tidak wajib.].

Mari kita lihat beberapa contoh. Aku akan menulis nilainya dalam hex karena
itu tepat dua digit per byte 8-bit, dan memudahkan kita melihat bagaimana
susunannya di memori.

|Karakter|Code Point|UTF-16BE|UTF-32BE|UTF-16LE|UTF-32LE|UTF-8|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`A`|41|0041|00000041|4100|41000000|41|
|`B`|42|0042|00000042|4200|42000000|42|
|`~`|7E|007E|0000007E|7E00|7E000000|7E|
|`π`|3C0|03C0|000003C0|C003|C0030000|CF80|
|`€`|20AC|20AC|000020AC|AC20|AC200000|E282AC|

[i[Unicode-->endianess]<]

Cari pola di dalamnya. Perhatikan bahwa UTF-16BE dan UTF-32BE hanyalah
code point yang direpresentasikan langsung sebagai nilai 16- dan
32-bit^[Sekali lagi, ini hanya benar di UTF-16 untuk karakter yang muat
dalam dua byte.].

[i[Unicode-->UTF-16]>]
[i[Unicode-->UTF-32]>]

Little-endian sama saja, hanya saja urutan byte-nya little-endian.

[i[Unicode-->endianess]>]

Lalu ada UTF-8 di bagian akhir. Pertama, mungkin kamu memperhatikan bahwa
code point satu byte direpresentasikan sebagai satu byte. Itu bagus. Kamu
mungkin juga memperhatikan bahwa code point yang berbeda memakai jumlah
byte yang berbeda. Inilah encoding dengan lebar bervariasi.

Jadi begitu kita melewati nilai tertentu, UTF-8 mulai memakai byte
tambahan untuk menyimpan nilainya. Dan byte-byte itu tampaknya tidak
berkorelasi dengan nilai code point.

[flw[Detail encoding UTF-8|UTF-8]] di luar cakupan panduan ini, tapi cukup
diketahui bahwa ia memakai jumlah byte yang bervariasi per code point, dan
nilai byte itu tidak cocok dengan code point _kecuali untuk 128 code point
pertama_. Kalau kamu benar-benar ingin belajar lebih jauh, [fl[Computerphile
punya video UTF-8 yang bagus bersama Tom
Scott|https://www.youtube.com/watch?v=MijmeoH9LT4]].

Soal terakhir itu hal yang keren tentang Unicode dan UTF-8 dari sudut
pandang Amerika Utara: ia kompatibel mundur dengan encoding ASCII 7-bit!
Jadi kalau kamu terbiasa dengan ASCII, UTF-8 sama saja! Setiap dokumen
ber-encoding ASCII juga ber-encoding UTF-8! (Tapi tidak sebaliknya, tentu
saja.)

Kemungkinan poin terakhir itulah, lebih dari yang lain, yang mendorong
UTF-8 menguasai dunia.

[i[Unicode-->UTF-8]>]
[i[Unicode-->encoding]>]

## Source dan Execution Character Set {#src-exec-charset}

[i[Character sets]<]

Saat memprogram di C, ada (setidaknya) tiga character set yang berperan:

* Yang dipakai kodemu saat tersimpan di disk.
* [i[Character sets-->source]]Yang diterjemahkan compiler dari itu tepat
  saat kompilasi dimulai (_source character set_). Ini mungkin sama
  dengan yang di disk, mungkin juga tidak.
* [i[Character sets-->execution]]Yang menjadi tujuan terjemahan source
  character set oleh compiler untuk eksekusi (_execution character set_).
  Ini mungkin sama dengan source character set, mungkin juga tidak.

Compiler-mu kemungkinan punya opsi untuk memilih character set ini saat
build.

[i[Character sets-->basic]<]

Basic character set untuk source maupun execution berisi karakter-karakter
berikut:

``` {.default}
A B C D E F G H I J K L M
N O P Q R S T U V W X Y Z
a b c d e f g h i j k l m
n o p q r s t u v w x y z
0 1 2 3 4 5 6 7 8 9
! " # % & ' ( ) * + , - . / :
; < = > ? [ \ ] ^ _ { | } ~
space tab vertical-tab
form-feed end-of-line
```

Itulah karakter yang bisa kamu pakai di source dan tetap 100% portabel.

Execution character set akan punya tambahan karakter untuk alert
(bell/flash), backspace, carriage return, dan newline.

Tapi kebanyakan orang tidak sampai seekstrem itu dan dengan bebas memakai
extended character set di source dan executable, terutama sekarang ketika
Unicode dan UTF-8 makin umum. Maksudnya, basic character set bahkan tidak
mengizinkan `@`, `$`, atau `` ` ``!

Perlu dicatat, menulis karakter Unicode hanya dengan basic character
set itu merepotkan (walaupun bisa dengan escape sequence).

[i[Character sets-->basic]>]
[i[Character sets]>]

## Unicode di C {#unicode-in-c}

Sebelum membahas encoding di C, mari kita bahas Unicode dari sudut pandang
code point dulu. Ada cara di C untuk menentukan karakter Unicode dan
karakter-karakter ini akan diterjemahkan compiler ke execution character
set^[Mungkin compiler berusaha sebaik mungkin menerjemahkan code point ke
encoding output apa pun itu, tapi aku tidak menemukan jaminannya di
spesifikasi.].

Jadi bagaimana caranya?

Bagaimana dengan simbol euro, code point 0x20AC. (Ditulis dalam hex karena
kedua cara merepresentasikannya di C membutuhkan hex.) Bagaimana kita bisa
memasukkannya ke kode C kita?

[i[`\u` Unicode escape]<]
Pakai escape `\u` untuk memasukkannya ke string, mis. `"\u20AC"` (huruf
besar/kecil pada hex tidak berpengaruh). Kamu harus menaruh **tepat empat**
digit hex setelah `\u`, dengan nol di depan kalau perlu.

Berikut contohnya:

``` {.c}
char *s = "\u20AC1.23";

printf("%s\n", s);  // €1.23
```

[i[`\U` Unicode escape]<]

Jadi `\u` berfungsi untuk code point Unicode 16-bit, tapi bagaimana dengan
yang lebih besar dari 16 bit? Untuk itu kita butuh huruf kapital: `\U`.

Misalnya:

``` {.c}
char *s = "\U0001D4D1";

printf("%s\n", s);  // Mencetak huruf matematika "B"
```

Sama seperti `\u`, hanya dengan 32 bit alih-alih 16. Keduanya setara:

``` {.c}
\u03C0
\U000003C0
```

[i[`\u` Unicode escape]>]
[i[`\U` Unicode escape]>]

Sekali lagi, ini diterjemahkan ke [i[Character
sets-->execution]]execution character set saat kompilasi. Mereka
merepresentasikan code point Unicode, bukan encoding tertentu. Selain itu,
kalau sebuah code point Unicode tidak bisa direpresentasikan di execution
character set, compiler boleh melakukan apa saja terhadapnya.

Nah, mungkin kamu bertanya kenapa tidak langsung begini:

``` {.c}
char *s = "€1.23";

printf("%s\n", s);  // €1.23
```

Dan kemungkinan besar bisa, dengan compiler modern. [i[Character
sets-->source]]Source character set akan diterjemahkan oleh compiler ke
[i[Character sets-->execution]]execution character set untukmu. Tapi
compiler bebas muntah kalau menemukan karakter yang tidak termasuk dalam
extended character set-nya, dan simbol € jelas tidak ada di [i[Character
sets-->basic]]basic character set.

[i[`\u` Unicode escape]<]
[i[`\U` Unicode escape]<]

Peringatan dari spesifikasi: kamu tidak boleh memakai `\u` atau `\U` untuk
meng-encode code point di bawah 0xA0 kecuali 0x24 (`$`), 0x40 (`@`), dan
0x60 (`` `
``)---ya, itulah tiga serangkai tanda baca umum yang tidak ada di basic
character set. Tampaknya pembatasan ini dilonggarkan di versi spesifikasi
yang akan datang.

[i[`\u` Unicode escape]>]
[i[`\U` Unicode escape]>]

Terakhir, kamu juga bisa memakai ini di identifier dalam kodemu, dengan
beberapa batasan. Tapi aku tidak ingin membahasnya di sini. Bab ini
fokus pada penanganan string.

Dan itu saja soal Unicode di C (kecuali encoding).

## Catatan Singkat tentang UTF-8 Sebelum Kita Terjun ke Semak-Semak {#utf8-quick}

[i[Unicode-->UTF-8]<]

Bisa jadi file source di disk, extended source character, dan extended
execution character-mu semuanya berformat UTF-8. Dan library yang kamu
pakai mengharapkan UTF-8. Inilah masa depan gemilang dengan UTF-8 di
mana-mana.

Kalau begitu, dan kamu tidak keberatan tidak portabel ke sistem yang tidak
seperti itu, jalankan saja. Taruh karakter Unicode di source dan datamu
sesukamu. Pakai string C biasa dan berbahagialah.

Banyak hal akan langsung berfungsi (walaupun tidak portabel) karena string
UTF-8 aman diakhiri NUL seperti string C lainnya. Tapi mungkin kehilangan
portabilitas demi penanganan karakter yang lebih mudah adalah pertukaran
yang sepadan bagimu.

Meski begitu, ada beberapa peringatan:

* Fungsi seperti `strlen()` melaporkan jumlah byte dalam string, belum
  tentu jumlah karakternya. (Fungsi [i[`mbstowcs()`
  function-->with UTF-8]]`mbstowcs()` mengembalikan jumlah karakter dalam
  string ketika kamu mengonversinya menjadi wide character. POSIX
  memperluas ini sehingga kamu bisa mengoper `NULL` sebagai argumen
  pertama kalau hanya ingin jumlah karakter.)

* Yang berikut tidak akan bekerja dengan benar pada karakter lebih dari
  satu byte: [i[`strtok()` function-->with UTF-8]]`strtok()`, [i[`strchr()`
  function-->with UTF-8]]`strchr()` (pakai [i[`strstr()` function-->with
  UTF-8]]`strstr()` sebagai gantinya), fungsi tipe `strspn()`,
  [i[`toupper()` function-->with UTF-8]]`toupper()`, [i[`tolower()`
  function-->with UTF-8]]`tolower()`, fungsi tipe [i[`isalpha()`
  function-->with UTF-8]]`isalpha()`, dan mungkin lebih banyak lagi.
  Waspadai apa pun yang beroperasi pada byte.

* Varian [i[`printf()` function-->with UTF-8]]`printf()` menyediakan cara
  untuk hanya mencetak sebagian byte dari sebuah string^[Dengan format
  specifier seperti `"%.12s"`, misalnya.]. Pastikan kamu mencetak jumlah
  byte yang benar agar berakhir di batas karakter.

* [i[`malloc()` function-->with UTF-8]]Kalau kamu ingin `malloc()` ruang
  untuk sebuah string, atau mendeklarasikan array `char` untuknya,
  ketahuilah bahwa ukuran maksimumnya bisa lebih besar dari yang kamu
  duga. Setiap karakter bisa memakai sampai [i[`MB_LEN_MAX`
  macro]]`MB_LEN_MAX` byte (dari `<limits.h>`)---kecuali karakter di
  basic character set yang dijamin satu byte.

Dan mungkin masih ada yang lain yang belum kutemukan. Kabari
aku kalau ada jebakan lain di luar sana...

[i[Unicode-->UTF-8]>]

## Berbagai Tipe Karakter

Aku ingin memperkenalkan lebih banyak tipe karakter. Kita sudah terbiasa
dengan `char`, kan?

Tapi itu terlalu mudah. Mari kita buat semuanya jauh lebih sulit! Hore!

### Karakter Multibyte

[i[Multibyte characters]<]

Pertama-tama, aku ingin mengubah cara berpikirmu tentang apa itu
string (array `char`). Ini adalah _string multibyte_ yang terdiri dari
_karakter multibyte_.

Betul---string karakter biasa yang kamu kenal itu multibyte. Ketika
seseorang bilang "string C", maksudnya "string multibyte C".

Bahkan kalau sebuah karakter dalam string hanya satu byte, atau kalau
sebuah string hanya terdiri dari karakter tunggal, ia tetap dikenal
sebagai string multibyte.

Misalnya:

``` {.c}
char c[128] = "Hello, world!";  // String multibyte
```

Maksudnya, sebuah karakter yang tidak ada di basic character set bisa
tersusun dari beberapa byte. Sampai sebanyak [i[`MB_LEN_MAX`
macro]]`MB_LEN_MAX` (dari `<limits.h>`). Memang di layar hanya tampak
seperti satu karakter, tapi ia bisa berupa beberapa byte.

Nilai Unicode juga bisa dimasukkan ke sana, seperti yang kita lihat tadi:

``` {.c}
char *s = "\u20AC1.23";

printf("%s\n", s);  // €1.23
```

Tapi di sini kita mulai masuk ke keanehan, karena lihat ini:

[i[`strlen()` function-->with UTF-8]<]

``` {.c}
char *s = "\u20AC1.23";  // €1.23

printf("%zu\n", strlen(s));  // 7!
```

Panjang string `"€1.23"` adalah `7`?! Ya! Setidaknya di sistemku, ya!
Ingat bahwa `strlen()` mengembalikan jumlah byte dalam string, bukan
jumlah karakter. (Saat membahas "wide character" nanti, kita akan melihat
cara mendapat jumlah karakter dalam string.)

[i[`strlen()` function-->with UTF-8]>]

Perhatikan bahwa walaupun C mengizinkan konstanta `char` multibyte
individual (berbeda dari `char*`), perilakunya bervariasi menurut
implementasi dan compiler-mu mungkin memperingatkannya.

GCC, misalnya, memperingatkan soal multi-character character constant
pada dua baris berikut (dan, di sistemku, mencetak encoding
UTF-8-nya):

``` {.c}
printf("%x\n", '€');
printf("%x\n", '\u20ac');
```

[i[Multibyte characters]>]

### Wide Character {#wide-characters}

[i[Wide characters]<]

Kalau bukan karakter multibyte, berarti kamu _wide character_.

Wide character adalah satu nilai yang bisa mewakili secara unik karakter
apa pun di locale saat ini. Analog dengan code point Unicode. Tapi bisa
jadi bukan. Atau bisa jadi iya.

Pada dasarnya, kalau string karakter multibyte adalah array byte, string
wide character adalah array _karakter_. Jadi kamu bisa mulai berpikir
per karakter, bukan per byte (yang terakhir jadi berantakan ketika
karakter mulai memakai jumlah byte yang bervariasi).

[i[`wchar_t` type]<]

Wide character bisa direpresentasikan oleh sejumlah tipe, tapi yang paling
menonjol adalah `wchar_t`. Ia yang utama. Mirip `char`, hanya saja lebar.

Mungkin kamu bertanya-tanya, kalau kita tidak bisa tahu apakah itu Unicode
atau bukan, bagaimana itu memberi banyak fleksibilitas dalam menulis kode?
`wchar_t` membuka sebagian pintu itu, karena ada sekumpulan fungsi lengkap
yang bisa dipakai untuk menangani string `wchar_t` (seperti mengambil
panjang, dan sebagainya) tanpa peduli encoding-nya.

## Memakai Wide Character dan `wchar_t`

Saatnya tipe baru: `wchar_t`. Ini tipe wide character utama. Ingat bahwa
`char` hanya satu byte? Dan satu byte mungkin tidak cukup untuk
merepresentasikan semua karakter? Nah, yang ini cukup.

Untuk memakai `wchar_t`, include `<wchar.h>`.

Berapa byte ukurannya? Tidak sepenuhnya jelas. Bisa 16 bit. Bisa 32 bit.

Tapi tunggu, katamu---kalau hanya 16 bit, ia tidak cukup besar untuk
menampung semua code point Unicode, kan? Benar---memang tidak. Spesifikasi
tidak mewajibkannya. Ia hanya harus bisa merepresentasikan semua karakter
di locale saat ini.

Ini bisa menyebabkan masalah dengan Unicode pada platform yang `wchar_t`-nya
16-bit (ehem---Windows). Tapi itu di luar cakupan panduan ini.

[i[`L` wide character prefix]<]

Kamu bisa mendeklarasikan string atau karakter bertipe ini dengan awalan
`L`, dan mencetaknya dengan format specifier `%ls` ("ell ess"). Atau
mencetak satu `wchar_t` dengan `%lc`.

``` {.c}
wchar_t *s = L"Hello, world!";
wchar_t c = L'B';

printf("%ls %lc\n", s, c);
```

[i[`L` wide character prefix]>]

Nah---apakah karakter-karakter itu disimpan sebagai code point Unicode
atau tidak? Tergantung implementasi. Tapi kamu bisa mengujinya dengan makro
[i[`__STDC_ISO_10646__` macro]] `__STDC_ISO_10646__`. Kalau makro ini
terdefinisi, jawabannya, "Itu Unicode!"

Lebih rinci, nilai di makro itu adalah integer berbentuk `yyyymm` yang
memberi tahu standar Unicode mana yang bisa kamu andalkan---yang berlaku
pada tanggal itu.

Tapi bagaimana cara memakainya?

### Konversi Multibyte ke `wchar_t`

Jadi bagaimana kita berpindah dari string standar berorientasi byte ke
string lebar berorientasi karakter dan sebaliknya?

Kita bisa memakai beberapa fungsi konversi string untuk mewujudkannya.

Pertama, beberapa konvensi penamaan yang akan kamu lihat pada fungsi-fungsi
ini:

* `mb`: multibyte
* `wc`: wide character
* `mbs`: string multibyte
* `wcs`: string wide character

Jadi kalau kita ingin mengonversi string multibyte menjadi string wide
character, kita bisa memanggil `mbstowcs()`. Dan sebaliknya: `wcstombs()`.

[i[`mbtowc()` function]]
[i[`wctomb()` function]]
[i[`mbstowcs()` function]]
[i[`wcstombs()` function]]

|Fungsi Konversi|Deskripsi|
|------------|---------------------------------------------------|
|`mbtowc()`|Mengonversi karakter multibyte menjadi wide character.|
|`wctomb()`|Mengonversi wide character menjadi karakter multibyte.|
|`mbstowcs()`|Mengonversi string multibyte menjadi wide string.|
|`wcstombs()`|Mengonversi wide string menjadi string multibyte.|

Mari kita buat demo singkat yang mengonversi string multibyte menjadi
string wide character, lalu membandingkan panjang kedua string itu dengan
fungsi masing-masing.

[i[`mbstowcs()` function]<]

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>
#include <string.h>
#include <locale.h>

int main(void)
{
    // Keluar dari locale C ke locale yang kemungkinan punya simbol euro
    setlocale(LC_ALL, "");

    // String multibyte asli dengan simbol euro (code point Unicode 20ac)
    char *mb_string = "The cost is \u20ac1.23";  // €1.23
    size_t mb_len = strlen(mb_string);

    // Array wide character yang akan menampung string hasil konversi
    wchar_t wc_string[128];  // Menampung sampai 128 wide character

    // Konversi string MB ke WC; ini mengembalikan jumlah wide char
    size_t wc_len = mbstowcs(wc_string, mb_string, 128);

    // Cetak hasil--perhatikan %ls untuk string wide char
    printf("multibyte: \"%s\" (%zu bytes)\n", mb_string, mb_len);
    printf("wide char: \"%ls\" (%zu characters)\n", wc_string, wc_len);
}
```

[i[`wchar_t` type]>]

Di sistemku, ini mengeluarkan:

``` {.default}
multibyte: "The cost is €1.23" (19 bytes)
wide char: "The cost is €1.23" (17 characters)
```

(Jumlah byte di sistemmu mungkin berbeda tergantung locale.)

Satu hal menarik: `mbstowcs()`, selain mengonversi string multibyte
menjadi wide, mengembalikan panjang (dalam karakter) string wide
character itu. Pada sistem yang patuh POSIX, kamu bisa memanfaatkan mode
khusus ketika ia _hanya_ mengembalikan panjang-dalam-karakter dari
string multibyte tertentu: cukup oper `NULL` untuk tujuan, dan `0` untuk
jumlah karakter maksimum yang dikonversi (nilai ini diabaikan).

(Pada kode di bawah, aku memakai extended source character
set milikku---kamu mungkin harus menggantinya dengan escape `\u`.)

``` {.c}
setlocale(LC_ALL, "");

// String berikut punya 7 karakter
size_t len_in_chars = mbstowcs(NULL, "§¶°±π€•", 0);

printf("%zu", len_in_chars);  // 7
```

[i[`mbstowcs()` function]>]

Sekali lagi, itu ekstensi POSIX yang tidak portabel.

Dan, tentu saja, kalau ingin mengonversi ke arah sebaliknya, pakai
[i[`wcstombs()` function]] `wcstombs()`.

## Fungsionalitas Wide Character

Setelah masuk ke wilayah wide character, kita punya berbagai fungsionalitas
yang bisa dipakai. Aku hanya akan meringkas sejumlah fungsi di sini,
tapi pada dasarnya yang ada adalah versi wide character dari fungsi string
multibyte yang sudah kita kenal. (Misalnya, kita kenal `strlen()` untuk
string multibyte; ada [i[`wcslen()` function]] `wcslen()` untuk string wide
character.)

### `wint_t`

[i[`wint_t` type]<]

Banyak fungsi ini memakai `wint_t` untuk menyimpan karakter tunggal, baik
yang dioper maupun yang dikembalikan.

Secara sifat ia terkait dengan `wchar_t`. `wint_t` adalah integer yang bisa
merepresentasikan semua nilai di extended character set, dan juga karakter
end-of-file khusus, `WEOF`.

[i[`wint_t` type]>]

Ini dipakai oleh sejumlah fungsi wide character yang berorientasi satu
karakter.

### Orientasi I/O Stream {#io-stream-orientation}

[i[I/O stream orientation]<]

Ringkasnya: jangan mencampur fungsi berorientasi byte (seperti
`fprintf()`) dengan fungsi berorientasi wide (seperti `fwprintf()`).
Putuskan apakah sebuah stream akan berorientasi byte atau wide dan
tetaplah memakai jenis fungsi I/O itu.

Lebih rinci: stream bisa berorientasi byte atau wide. Saat sebuah stream
pertama kali dibuat, ia belum punya orientasi, tapi pembacaan atau
penulisan pertama akan menetapkan orientasinya.

Kalau kamu pertama kali memakai operasi wide (seperti `fwprintf()`), stream
itu berorientasi wide.

Kalau kamu pertama kali memakai operasi byte (seperti `fprintf()`), stream
itu berorientasi byte.

Kamu bisa mengatur secara manual stream yang belum berorientasi ke satu
arah atau yang lain dengan memanggil [i[`fwide()` function]] `fwide()`.
Fungsi yang sama bisa dipakai untuk mengetahui orientasi sebuah stream.

Kalau perlu mengubah orientasi di tengah jalan, bisa dilakukan dengan
`freopen()`.

[i[I/O stream orientation]>]

### Fungsi I/O

Biasanya include `<stdio.h>` dan `<wchar.h>` untuk ini.

[i[`wprintf()` function]]
[i[`wscanf()` function]]
[i[`getwchar()` function]]
[i[`putwchar()` function]]
[i[`fwprintf()` function]]
[i[`fwscanf()` function]]
[i[`fgetwc()` function]]
[i[`fputwc()` function]]
[i[`fgetws()` function]]
[i[`fputws()` function]]
[i[`swprintf()` function]]
[i[`swscanf()` function]]
[i[`vfwprintf()` function]]
[i[`vfwscanf()` function]]
[i[`vswprintf()` function]]
[i[`vswscanf()` function]]
[i[`vwprintf()` function]]
[i[`vwscanf()` function]]
[i[`ungetwc()` function]]
[i[`fwide()` function]]

|Fungsi I/O|Deskripsi|
|------------|---------------------------------------------------|
|`wprintf()`|Output terformat ke konsol.|
|`wscanf()`|Input terformat dari konsol.|
|`getwchar()`|Input konsol berbasis karakter.|
|`putwchar()`|Output konsol berbasis karakter.|
|`fwprintf()`|Output terformat ke file.|
|`fwscanf()`|Input terformat dari file.|
|`fgetwc()`|Input file berbasis karakter.|
|`fputwc()`|Output file berbasis karakter.|
|`fgetws()`|Input file berbasis string.|
|`fputws()`|Output file berbasis string.|
|`swprintf()`|Output terformat ke string.|
|`swscanf()`|Input terformat dari string.|
|`vfwprintf()`|Output terformat variadic ke file.|
|`vfwscanf()`|Input terformat variadic dari file.|
|`vswprintf()`|Output terformat variadic ke string.|
|`vswscanf()`|Input terformat variadic dari string.|
|`vwprintf()`|Output terformat variadic ke konsol.|
|`vwscanf()`|Input terformat variadic dari konsol.|
|`ungetwc()`|Mengembalikan sebuah wide character ke stream.|
|`fwide()`|Mengambil atau mengatur orientasi multibyte/wide sebuah stream.|

### Fungsi Konversi Tipe

Biasanya include `<wchar.h>` untuk ini.

[i[`wcstod()` function]]
[i[`wcstof()` function]]
[i[`wcstold()` function]]
[i[`wcstol()` function]]
[i[`wcstoll()` function]]
[i[`wcstoul()` function]]
[i[`wcstoull()` function]]

|Fungsi Konversi|Deskripsi|
|------------|---------------------------------------------------|
|`wcstod()`|Mengonversi string menjadi `double`.|
|`wcstof()`|Mengonversi string menjadi `float`.|
|`wcstold()`|Mengonversi string menjadi `long double`.|
|`wcstol()`|Mengonversi string menjadi `long`.|
|`wcstoll()`|Mengonversi string menjadi `long long`.|
|`wcstoul()`|Mengonversi string menjadi `unsigned long`.|
|`wcstoull()`|Mengonversi string menjadi `unsigned long long`.|

### Fungsi Penyalinan String dan Memori

Biasanya include `<wchar.h>` untuk ini.

[i[`wcscpy()` function]]
[i[`wcsncpy()` function]]
[i[`wmemcpy()` function]]
[i[`wmemmove()` function]]
[i[`wcscat()` function]]
[i[`wcsncat()` function]]

|Fungsi Penyalinan|Deskripsi|
|----|----------------------------------------------|
|`wcscpy()`|Menyalin string.|
|`wcsncpy()`|Menyalin string, dibatasi panjang.|
|`wmemcpy()`|Menyalin memori.|
|`wmemmove()`|Menyalin memori yang mungkin tumpang tindih.|
|`wcscat()`|Menyambung string.|
|`wcsncat()`|Menyambung string, dibatasi panjang.|

### Fungsi Perbandingan String dan Memori

Biasanya include `<wchar.h>` untuk ini.

[i[`wcscmp()` function]]
[i[`wcsncmp()` function]]
[i[`wcscoll()` function]]
[i[`wmemcmp()` function]]
[i[`wcsxfrm()` function]]

|Fungsi Perbandingan|Deskripsi|
|-------------------|---------------------------------------------------------------|
|`wcscmp()`|Membandingkan string secara leksikografis.|
|`wcsncmp()`|Membandingkan string secara leksikografis, dibatasi panjang.|
|`wcscoll()`|Membandingkan string menurut urutan kamus berdasarkan locale.|
|`wmemcmp()`|Membandingkan memori secara leksikografis.|
|`wcsxfrm()`|Mengubah string menjadi versi yang membuat `wcscmp()` berperilaku seperti `wcscoll()`[^97d0].|

[^97d0]: `wcscoll()` sama dengan `wcsxfrm()` diikuti `wcscmp()`.

### Fungsi Pencarian String

Biasanya include `<wchar.h>` untuk ini.

[i[`wcschr()` function]]
[i[`wcsrchr()` function]]
[i[`wmemchr()` function]]
[i[`wcsstr()` function]]
[i[`wcspbrk()` function]]
[i[`wcsspn()` function]]
[i[`wcscspn()` function]]
[i[`wcstok()` function]]

|Fungsi Pencarian|Deskripsi|
|-|-|
|`wcschr()`|Mencari karakter dalam string.|
|`wcsrchr()`|Mencari karakter dalam string dari belakang.|
|`wmemchr()`|Mencari karakter di memori.|
|`wcsstr()`|Mencari substring dalam string.|
|`wcspbrk()`|Mencari salah satu dari sekumpulan karakter dalam string.|
|`wcsspn()`|Mencari panjang substring yang berisi salah satu dari sekumpulan karakter.|
|`wcscspn()`|Mencari panjang substring sebelum salah satu dari sekumpulan karakter.|
|`wcstok()`|Mencari token dalam string.|

### Fungsi Panjang/Lain-lain

Biasanya include `<wchar.h>` untuk ini.

[i[`wcslen()` function]]
[i[`wmemset()` function]]
[i[`wcsftime()` function]]

|Fungsi Panjang/Lain-lain|Deskripsi|
|-|-|
|`wcslen()`|Mengembalikan panjang string.|
|`wmemset()`|Mengisi karakter di memori.|
|`wcsftime()`|Output tanggal dan waktu terformat.|

### Fungsi Klasifikasi Karakter

Include `<wctype.h>` untuk ini.

[i[`iswalnum()` function]]
[i[`iswalpha()` function]]
[i[`iswblank()` function]]
[i[`iswcntrl()` function]]
[i[`iswdigit()` function]]
[i[`iswgraph()` function]]
[i[`iswlower()` function]]
[i[`iswprint()` function]]
[i[`iswpunct()` function]]
[i[`iswspace()` function]]
[i[`iswupper()` function]]
[i[`iswxdigit()` function]]
[i[`towlower()` function]]
[i[`towupper()` function]]

|Fungsi Panjang/Lain-lain|Deskripsi|
|------------|---------------------------------------------------|
|`iswalnum()`|True jika karakter alfanumerik.|
|`iswalpha()`|True jika karakter berupa huruf.|
|`iswblank()`|True jika karakter kosong (mirip spasi, tapi bukan newline).|
|`iswcntrl()`|True jika karakter adalah karakter kontrol.|
|`iswdigit()`|True jika karakter berupa digit.|
|`iswgraph()`|True jika karakter bisa dicetak (kecuali spasi).|
|`iswlower()`|True jika karakter huruf kecil.|
|`iswprint()`|True jika karakter bisa dicetak (termasuk spasi).|
|`iswpunct()`|True jika karakter berupa tanda baca.|
|`iswspace()`|True jika karakter berupa whitespace.|
|`iswupper()`|True jika karakter huruf besar.|
|`iswxdigit()`|True jika karakter berupa digit hex.|
|`towlower()`|Mengonversi karakter menjadi huruf kecil.|
|`towupper()`|Mengonversi karakter menjadi huruf besar.|

## Parse State, Fungsi Restartable

[i[Multibyte characters-->parse state]<]

Kita akan masuk sedikit ke jeroan konversi multibyte, tapi ini hal yang
bagus untuk dipahami secara konseptual.

Bayangkan bagaimana programmu mengambil deretan karakter multibyte dan
mengubahnya menjadi wide character, atau sebaliknya. Pada suatu titik, ia
mungkin sedang di tengah mem-parse sebuah karakter, atau mungkin harus
menunggu byte tambahan sebelum menentukan nilai akhirnya.

Parse state ini disimpan dalam variabel opaque bertipe `mbstate_t` dan
dipakai setiap kali konversi dilakukan. Begitulah cara fungsi konversi
melacak posisinya di tengah pekerjaan.

Dan kalau kamu berpindah ke deretan karakter lain di tengah stream, atau
mencoba seek ke tempat lain di deretan inputmu, ia bisa bingung karenanya.

Mungkin kamu ingin menegurku soal ini: tadi kita sudah melakukan beberapa
konversi, dan aku tidak pernah menyebut `mbstate_t` di mana pun.

Itu karena fungsi konversi seperti `mbstowcs()`, `wctomb()`, dan
sebagainya masing-masing punya variabel `mbstate_t` sendiri yang mereka
pakai. Tapi hanya ada satu per fungsi, jadi kalau kamu menulis kode
multithread, fungsi-fungsi itu tidak aman dipakai.

Untungnya, C mendefinisikan versi _restartable_ dari fungsi-fungsi ini
yang memungkinkan kamu mengoper `mbstate_t`-mu sendiri per thread kalau perlu.
Kalau kamu bekerja dengan multithread, pakai ini!

Catatan singkat tentang menginisialisasi variabel `mbstate_t`: cukup
`memset()` ke nol. Tidak ada fungsi bawaan untuk memaksa inisialisasi.

``` {.c}
mbstate_t mbs;

// Atur state ke state awal
memset(&mbs, 0, sizeof mbs);
```

Berikut daftar fungsi konversi restartable---perhatikan konvensi
penamaan dengan menaruh "`r`" setelah tipe "asal":

* `mbrtowc()`---multibyte ke wide character
* `wcrtomb()`---wide character ke multibyte
* `mbsrtowcs()`---string multibyte ke string wide character
* `wcsrtombs()`---string wide character ke string multibyte

Fungsi-fungsi ini sangat mirip dengan padanannya yang non-restartable,
hanya saja mereka mewajibkan kamu mengoper pointer ke variabel `mbstate_t`
milikmu sendiri. Mereka juga mengubah pointer string sumber (untuk
membantu kalau ditemukan byte yang tidak valid), jadi mungkin berguna
menyimpan salinan aslinya.

Berikut contoh dari awal bab yang disusun ulang agar mengoper `mbstate_t`
kita sendiri.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
#include <wchar.h>
#include <string.h>
#include <locale.h>

int main(void)
{
    // Keluar dari locale C ke locale yang kemungkinan punya simbol euro
    setlocale(LC_ALL, "");

    // String multibyte asli dengan simbol euro (code point Unicode 20ac)
    char *mb_string = "The cost is \u20ac1.23";  // €1.23
    size_t mb_len = strlen(mb_string);

    // Array wide character yang akan menampung string hasil konversi
    wchar_t wc_string[128];  // Menampung sampai 128 wide character

    // Siapkan state konversi
    mbstate_t mbs;
    memset(&mbs, 0, sizeof mbs);  // State awal

    // mbsrtowcs() mengubah pointer input agar menunjuk ke karakter tidak
    // valid pertama, atau NULL jika berhasil. Kita buat salinan pointer
    // untuk diutak-atik mbsrtowcs() sehingga aslinya tidak berubah.
    //
    // Contoh ini kemungkinan berhasil, tapi kita periksa lebih ke bawah
    // untuk memastikan.
    const char *invalid = mb_string;

    // Konversi string MB ke WC; ini mengembalikan jumlah wide char
    size_t wc_len = mbsrtowcs(wc_string, &invalid, 128, &mbs);

    if (invalid == NULL) {
        printf("No invalid characters found\n");

        // Cetak hasil--perhatikan %ls untuk string wide char
        printf("multibyte: \"%s\" (%zu bytes)\n", mb_string, mb_len);
        printf("wide char: \"%ls\" (%zu characters)\n", wc_string, wc_len);
    } else {
        ptrdiff_t offset = invalid - mb_string;
        printf("Invalid character at offset %td\n", offset);
    }
}
```

Untuk fungsi konversi yang mengelola state-nya sendiri, kamu bisa mereset
state internalnya ke state awal dengan mengoper `NULL` untuk argumen
`char*`-nya, misalnya:

``` {.c}
mbstowcs(NULL, NULL, 0);   // Reset parse state untuk mbstowcs()
mbstowcs(dest, src, 100);  // Parse beberapa hal
```

Untuk I/O, setiap wide stream mengelola `mbstate_t`-nya sendiri dan
memakainya untuk konversi input dan output selama berjalan.

Dan beberapa fungsi I/O berorientasi byte seperti `printf()` dan
`scanf()` menyimpan state internalnya sendiri saat bekerja.

Terakhir, fungsi konversi restartable ini sebenarnya punya state internal
sendiri kalau kamu mengoper `NULL` sebagai parameter `mbstate_t`. Ini
membuat mereka berperilaku lebih mirip padanannya yang non-restartable.

[i[Multibyte characters-->parse state]>]
[i[Wide characters]>]

## Encoding Unicode dan C

Di bagian ini, kita akan melihat apa yang bisa (dan tidak bisa) dilakukan
C pada tiga encoding Unicode tertentu: UTF-8, UTF-16, dan UTF-32.

### UTF-8

[i[Unicode-->UTF-8]<]

Untuk menyegarkan ingatan sebelum bagian ini, baca [catatan singkat UTF-8,
di atas](#utf8-quick).

Selain itu, apa kemampuan UTF-8 di C?

Sayangnya, tidak banyak.

[i[`u8` UTF-8 prefix]<]

Kamu bisa memberi tahu C bahwa kamu secara spesifik menginginkan string
literal ber-encoding UTF-8, dan ia akan melakukannya. Caranya dengan
awalan `u8` pada string:

``` {.c}
char *s = u8"Hello, world!";

printf("%s\n", s);   // Hello, world!--kalau kamu bisa mengeluarkan UTF-8
```

Nah, bisakah karakter Unicode dimasukkan ke sana?

``` {.c}
char *s = u8"€123";
```

[i[`u8` UTF-8 prefix]>]

Tentu! Kalau extended source character set mendukungnya. (gcc mendukung.)

Bagaimana kalau tidak? Kamu bisa menentukan code point Unicode dengan
`\u` dan `\U` yang akrab di sekitar kita, [seperti dicatat di
atas](#unicode-in-c).

Tapi hanya itu. Tidak ada cara portabel di standard library untuk
mengambil input sembarang dan mengubahnya menjadi UTF-8 kecuali locale-mu
UTF-8. Atau untuk mem-parse UTF-8 kecuali locale-mu UTF-8.

Jadi kalau ingin melakukannya, berada di locale UTF-8 dan:

[i[`setlocale()` function]<]

``` {.c}
setlocale(LC_ALL, "");
```

atau cari tahu nama locale UTF-8 di mesin lokalmu dan atur secara eksplisit
seperti ini:

``` {.c}
setlocale(LC_ALL, "en_US.UTF-8");  // Nama non-portabel
```

[i[`setlocale()` function]>]

Atau pakai [library pihak ketiga](#utf-3rd-party).

[i[Unicode-->UTF-8]>]

### UTF-16, UTF-32, `char16_t`, dan `char32_t`

[i[Unicode-->UTF-16]<]
[i[Unicode-->UTF-32]<]
[i[`char16_t` type]<]
[i[`char32_t` type]<]

`char16_t` dan `char32_t` adalah dua tipe karakter yang berpotensi lebar,
masing-masing berukuran 16 bit dan 32 bit. Belum tentu lebar, karena kalau
tidak bisa merepresentasikan setiap karakter di locale saat ini, mereka
kehilangan sifat wide character-nya. Tapi spesifikasi menyebut mereka tipe
"wide character" di mana-mana, jadi begitulah.

Tipe-tipe ini ada untuk membuat segalanya sedikit lebih ramah Unicode,
mudah-mudahan.

Untuk memakainya, include `<uchar.h>`. (Itu "u", bukan "w".)

Header file ini tidak ada di OS X---sayang sekali. Kalau kamu hanya ingin
tipenya, kamu bisa:

``` {.c}
#include <stdint.h>

typedef int_least16_t char16_t;
typedef int_least32_t char32_t;
```

Tapi kalau kamu juga ingin fungsinya, itu urusanmu sendiri.

[i[`u` Unicode prefix]<]
[i[`U` Unicode prefix]<]

Dengan asumsi kamu masih bisa lanjut, kamu bisa mendeklarasikan string atau
karakter bertipe ini dengan awalan `u` dan `U`:

``` {.c}
char16_t *s = u"Hello, world!";
char16_t c = u'B';

char32_t *t = U"Hello, world!";
char32_t d = U'B';
```

[i[`char32_t` type]>]
[i[`u` Unicode prefix]>]
[i[`U` Unicode prefix]>]

Nah---apakah nilai di dalamnya disimpan dalam UTF-16 atau UTF-32?
Tergantung implementasi.

Tapi kamu bisa mengujinya. Kalau makro [i[`__STDC_UTF_16__`
macro]] `__STDC_UTF_16__` atau [i[`__STDC_UTF_32__ macro`]]
`__STDC_UTF_32__` terdefinisi (bernilai `1`), artinya tipe-tipe itu
masing-masing menyimpan UTF-16 atau UTF-32.

Kalau kamu penasaran, dan aku tahu kamu penasaran, nilainya, kalau
UTF-16 atau UTF-32, disimpan dalam endianess native. Artinya, kamu
seharusnya bisa langsung membandingkannya dengan nilai code point Unicode:

``` {.c}
char16_t pi = u"\u03C0";  // simbol pi

#if __STDC_UTF_16__
pi == 0x3C0;  // Selalu true
#else
pi == 0x3C0;  // Kemungkinan tidak true
#endif
```

[i[`char16_t` type]>]
[i[Unicode-->UTF-16]>]
[i[Unicode-->UTF-32]>]

### Konversi Multibyte

Kamu bisa mengonversi dari encoding multibyte-mu ke `char16_t` atau
`char32_t` dengan sejumlah fungsi bantu.

(Seperti kubilang tadi, hasilnya mungkin bukan UTF-16 atau UTF-32
kecuali makro yang sesuai bernilai `1`.)

Semua fungsi ini restartable (yaitu kamu mengoper `mbstate_t`-mu sendiri),
dan semuanya beroperasi karakter demi
karakter^[Kurang lebih---urusannya jadi aneh pada encoding UTF-16 dengan
beberapa `char16_t`.].

[i[`mbrtoc16()` function]]
[i[`mbrtoc32()` function]]
[i[`c16rtomb()` function]]
[i[`c32rtomb()` function]]

|Fungsi Konversi|Deskripsi|
|-|-|
|`mbrtoc16()`|Mengonversi karakter multibyte menjadi karakter `char16_t`.|
|`mbrtoc32()`|Mengonversi karakter multibyte menjadi karakter `char32_t`.|
|`c16rtomb()`|Mengonversi karakter `char16_t` menjadi karakter multibyte.|
|`c32rtomb()`|Mengonversi karakter `char32_t` menjadi karakter multibyte.|

### Library Pihak Ketiga {#utf-3rd-party}

Untuk konversi berat antara berbagai encoding spesifik, ada beberapa
library matang yang layak dicoba. Perhatikan bahwa aku belum pernah
memakai keduanya.

* [flw[iconv|Iconv]]---Internationalization Conversion, API standar POSIX
  yang umum dan tersedia di platform-platform besar.
* [fl[ICU|http://site.icu-project.org/]]---International Components for
  Unicode. Setidaknya satu blogger menganggapnya mudah dipakai.

Kalau kamu punya library lain yang patut dicatat, kabari aku.

[i[Unicode]>]
