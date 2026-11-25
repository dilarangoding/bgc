<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe II: Jauh Lebih Banyak Tipe!

Kita sudah terbiasa dengan tipe `char`, `int`, dan `float`, tapi sekarang
saatnya membawanya ke level berikutnya dan melihat apa lagi yang tersedia
di departemen tipe!

## Integer Signed dan Unsigned

[i[Types-->signed and unsigned]<]
Sejauh ini kita memakai `int` sebagai tipe _signed_, yaitu nilai yang bisa
negatif maupun positif. Tapi C juga punya tipe integer _unsigned_ khusus
yang hanya bisa menyimpan angka positif.

Tipe-tipe ini diawali dengan kata kunci [i[`unsigned` type]<]`unsigned`.

``` {.c}
int a;           // signed
signed int a;    // signed
signed a;        // signed, "singkatan" dari "int" atau "signed int", jarang dipakai
unsigned int b;  // unsigned
unsigned c;      // unsigned, singkatan dari "unsigned int"
```

Kenapa? Kenapa kamu memutuskan hanya ingin menyimpan angka positif?

Jawabannya: variabel unsigned bisa menampung angka yang lebih besar
dibanding variabel signed.

Tapi kenapa begitu?

Bayangkan integer direpresentasikan oleh sejumlah _bit_^["Bit" kependekan
dari _binary digit_. Biner hanyalah cara lain merepresentasikan angka.
Alih-alih digit 0-9 yang biasa kita pakai, digitnya hanya 0-1.]. Di
komputerku, sebuah `int` direpresentasikan oleh 64 bit.

Dan setiap permutasi bit yang bernilai `1` atau `0` mewakili sebuah angka.
Kita yang memutuskan bagaimana membagi angka-angka ini.

Pada angka signed, kita memakai (kurang lebih) separuh permutasi untuk
angka negatif, dan separuh lainnya untuk angka positif.

Pada unsigned, kita memakai _semua_ permutasi untuk angka positif.

Di komputerku, dengan `int` 64-bit yang memakai [flw[two's
complement|Two%27s_complement]] untuk merepresentasikan angka unsigned,
rentang integernya adalah sebagai berikut:

|Tipe|Minimum|Maksimum|
|:-|-:|-:|
|`int`|`-9,223,372,036,854,775,808`|`9,223,372,036,854,775,807`|
|`unsigned int`|`0`|`18,446,744,073,709,551,615`|

Perhatikan bahwa `unsigned int` positif terbesar kira-kira dua kali lebih
besar dari `int` positif terbesar. Jadi ada sedikit keleluasaan di sana.
[i[`unsigned` type]>]
[i[Types-->signed and unsigned]>]

## Tipe Karakter

[i[Types-->character]<]
[i[`char` type]<]
Ingat `char`? Tipe yang bisa kita pakai untuk menyimpan satu karakter?

``` {.c}
char c = 'B';

printf("%c\n", c);  // "B"
```

Ada kejutan untukmu: sebenarnya ia adalah integer.

``` {.c}
char c = 'B';

// Ubah ini dari %c menjadi %d:
printf("%d\n", c);  // 66 (!!)
```

Jauh di dalamnya, `char` hanyalah `int` kecil, yaitu integer yang hanya
memakai satu byte ruang, jadi rentangnya terbatas pada...

Di sini spesifikasi C jadi sedikit aneh. Ia menjamin bahwa `char` adalah
satu byte, yaitu `sizeof(char) == 1`. Tapi kemudian di C11 §3.6¶3 ia
berusaha keras menyatakan:

> A byte is composed of a contiguous sequence of bits, _the number of
> which is implementation-defined._

Tunggu---apa? Mungkin sebagian dari kamu terbiasa dengan anggapan bahwa
byte itu 8 bit, kan? Maksudnya, memang begitu, kan? Dan jawabannya:
"Hampir pasti."^[Istilah industri untuk deretan yang tepat dan pasti 8 bit
adalah _oktet_.] Tapi C bahasa yang tua, dan mesin-mesin zaman dulu punya
pendapat yang, katakanlah, lebih _santai_ soal berapa bit dalam satu byte.
Dan selama bertahun-tahun, C mempertahankan fleksibilitas ini.

Tapi dengan asumsi byte di C-mu berukuran 8 bit, seperti hampir semua
mesin di dunia yang akan pernah kamu temui, rentang `char` adalah...

[i[`unsigned char` type]<]
[i[`signed char` type]<]
---Nah, sebelum aku bisa menyebutkannya, ternyata `char` bisa signed atau
unsigned tergantung compiler-mu. Kecuali kalau kamu menyebutkannya secara
eksplisit.

Dalam banyak kasus, cukup memakai `char` karena kamu tidak peduli soal
tanda data. Tapi kalau kamu butuh `char` signed atau unsigned, kamu
_harus_ eksplisit:

``` {.c}
char a;           // Bisa signed atau unsigned
signed char b;    // Pasti signed
unsigned char c;  // Pasti unsigned
```

Oke, sekarang akhirnya kita bisa menentukan rentang angkanya, dengan
asumsi `char` berukuran 8 bit dan sistemmu memakai representasi two's
complement yang hampir universal untuk signed dan unsigned^[Secara umum,
kalau ada bilangan two's complement $n$ bit, rentang signed-nya adalah
$-2^{n-1}$ sampai $2^{n-1}-1$. Dan rentang unsigned-nya adalah $0$ sampai
$2^n-1$.].

Jadi, dengan asumsi-asumsi itu, akhirnya kita bisa menghitung rentangnya:

|Tipe `char`|Minimum|Maksimum|
|:-|-:|-:|
|`signed char`|`-128`|`127`|
|`unsigned char`|`0`|`255`|

Dan rentang untuk `char` ditentukan oleh implementasi.
[i[`unsigned char` type]>]
[i[`signed char` type]>]

Biar kuluruskan dulu. `char` sebenarnya angka, berarti bisa dihitung secara
matematis?

Bisa! Cuma ingat untuk menjaga nilainya dalam rentang `char`!

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char a = 10, b = 20;

    printf("%d\n", a + b);  // 30!
}
```

[i[`'` single quote]<]
Bagaimana dengan konstanta karakter dalam tanda kutip tunggal, seperti
`'B'`? Bagaimana ia punya nilai numerik?

Spesifikasi juga bersikap mengambang di sini, karena C tidak dirancang
untuk berjalan di satu jenis sistem dasar saja.

Tapi anggap saja untuk sementara character set-mu berbasis
[flw[ASCII|ASCII]] setidaknya untuk 128 karakter pertama. Kalau begitu,
konstanta karakter akan dikonversi menjadi `char` yang nilainya sama
dengan nilai ASCII karakter tersebut.

Ribet juga, ya. Langsung saja contoh:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char a = 10;
    char b = 'B';  // Nilai ASCII 66

    printf("%d\n", a + b);  // 76!
}
```

Ini bergantung pada lingkungan eksekusimu dan [flw[character set yang
dipakai|List_of_information_system_character_sets]]. Salah satu character
set paling populer saat ini adalah [flw[Unicode|Unicode]] (yang merupakan
superset ASCII), jadi untuk 0-9, A-Z, a-z dan tanda baca dasar, hampir
pasti kamu mendapat nilai ASCII-nya.
[i[`'` single quote]>]
[i[`char` type]>]
[i[Types-->character]>]

## Tipe Integer Lainnya: `short`, `long`, `long long`

Sejauh ini kita hanya memakai dua tipe integer secara umum:

* `char`
* `int`

dan baru-baru ini kita belajar varian unsigned dari tipe integer. Kita
juga tahu bahwa `char` diam-diam adalah `int` kecil yang menyamar. Jadi
kita tahu `int` bisa punya berbagai ukuran bit.

Tapi ada beberapa tipe integer lagi yang perlu kita lihat, beserta
rentang minimum yang bisa mereka tampung. (Implementasimu kemungkinan
punya rentang lebih lebar daripada yang disyaratkan spesifikasi, tapi
rentang di sini adalah yang **pasti** tersedia secara portabel.)

Header file `<limits.h>` mendefinisikan makro yang menyimpan rentang
berbagai tipe; andalkan itu untuk kepastian, dan _jangan pernah
meng-hardcode atau mengasumsikan nilai-nilai ini_.

[i[`short` type]<]
[i[`long` type]<]
[i[`long long` type]<]
Tipe tambahan ini adalah `short int`, `long int`, dan `long long int`.
Biasanya, saat memakai tipe-tipe ini, developer C menghilangkan bagian
`int` (misalnya `long long`), dan compiler tetap senang.

``` {.c}
// Dua baris ini setara:
long long int x;
long long x;

// Begitu juga dua baris ini:
short int x;
short x;
```

Mari kita lihat tipe data integer dan ukurannya secara menaik, dikelompokkan
menurut tanda. Sekali lagi, batas minimum dan maksimum ini adalah yang
dijamin secara portabel oleh spesifikasi; sistemmu kemungkinan punya
rentang lebih lebar.

|Tipe|Byte Minimum|Nilai Minimum|Nilai Maksimum|
|:-|-:|-:|-:|
|`char`|1|-128 atau 0|127 atau 255^[Tergantung apakah `char` secara bawaan berupa `signed char` atau `unsigned char`]|
|`signed char`|1|-128|127|
|`short`|2|-32768|32767|
|`int`|2|-32768|32767|
|`long`|4|-2147483648|2147483647|
|`long long`|8|-9223372036854775808|9223372036854775807|
|`unsigned char`|1|0|255|
|`unsigned short`|2|0|65535|
|`unsigned int`|2|0|65535|
|`unsigned long`|4|0|4294967295|
|`unsigned long long`|8|0|18446744073709551615|

Tidak ada tipe `long long long`. Kamu tidak bisa terus menambah `long`
seperti itu. Jangan konyol.

> Penggemar two's complement mungkin menyadari ada yang janggal pada
> angka-angka itu. Kenapa, misalnya, `signed char` berhenti di -127 dan
> bukan -128? Ingat: itu hanya minimum yang disyaratkan spesifikasi.
> Beberapa representasi angka (seperti [flw[sign and
> magnitude|Signed_number_representations#Signed_magnitude_representation]])
> mentok di ±127.

Mari kita jalankan tabel yang sama pada sistem 64-bit two's complement
milikku dan lihat hasilnya:

|Tipe|Byte di Sistemku|Nilai Minimum|Nilai Maksimum|
|:-|-:|-:|-:|
|`char`|1|-128|127^[`char` di sistemku adalah signed.]|
|`signed char`|1|-128|127|
|`short`|2|-32768|32767|
|`int`|4|-2147483648|2147483647|
|`long`|8|-9223372036854775808|9223372036854775807|
|`long long`|8|-9223372036854775808|9223372036854775807|
|`unsigned char`|1|0|255|
|`unsigned short`|2|0|65535|
|`unsigned int`|4|0|4294967295|
|`unsigned long`|8|0|18446744073709551615|
|`unsigned long long`|8|0|18446744073709551615|

Ini sedikit lebih masuk akal, tapi kita bisa melihat bahwa sistemku
punya batas yang lebih besar daripada minimum di spesifikasi.

[Lalu apa saja makro di `<limits.h>`?]{#limits-macros}

|Tipe|Makro Min|Makro Maks|
|:-|:-|:-|
|`char`|`CHAR_MIN`|`CHAR_MAX`|
|`signed char`|`SCHAR_MIN`|`SCHAR_MAX`|
|`short`|`SHRT_MIN`|`SHRT_MAX`|
|`int`|`INT_MIN`|`INT_MAX`|
|`long`|`LONG_MIN`|`LONG_MAX`|
|`long long`|`LLONG_MIN`|`LLONG_MAX`|
|`unsigned char`|`0`|`UCHAR_MAX`|
|`unsigned short`|`0`|`USHRT_MAX`|
|`unsigned int`|`0`|`UINT_MAX`|
|`unsigned long`|`0`|`ULONG_MAX`|
|`unsigned long long`|`0`|`ULLONG_MAX`|

Perhatikan ada cara tersembunyi di sana untuk menentukan apakah sebuah
sistem memakai `char` signed atau unsigned. Kalau `CHAR_MAX == UCHAR_MAX`,
berarti ia unsigned.

Perhatikan juga bahwa tidak ada makro minimum untuk varian
`unsigned`---nilainya cuma `0`.
[i[`short` type]>]
[i[`long` type]>]
[i[`long long` type]>]

## Float Lainnya: `double` dan `long double`

Mari kita lihat apa kata spesifikasi tentang bilangan floating point di
§5.2.4.2.2¶1-2:

>The following parameters are used to define the model for each
>floating-point type:
>
> |Parameter|Definition|
> |:-|:-|
> |$s$|sign ($\pm1$)|
> |$b$|base or radix of exponent representation (an integer $> 1$)|
> |$e$|exponent (an integer between a minimum $e_{min}$ and a maximum $e_{max}$)|
> |$p$|precision (the number of base-$b$ digits in the significand)|
> |$f_k$|nonnegative integers less than $b$ (the significand digits)|
>
> A _floating-point number_ ($x$) is defined by the following model:
>
>> $x=sb^e\sum\limits_{k=1}^p f_kb^{-k},$\ \ \ \ $e_{min}\le e\le e_{max}$

Kuharap itu langsung bikin semuanya jelas.

Baiklah. Mundur selangkah dan lihat apa yang praktis.

Catatan: kita merujuk ke sejumlah makro di bagian ini. Makro-makro itu ada
di header `<float.h>`.

Bilangan floating point di-encode dalam urutan bit tertentu
([flw[format IEEE-754|IEEE_754]] sangat populer) di dalam byte.

Lebih dalam lagi, bilangan itu pada dasarnya direpresentasikan sebagai
_significand_ (bagian angkanya---digit-digit signifikannya sendiri,
kadang juga disebut _mantissa_) dan _exponent_, yaitu pangkat yang
dipakai untuk menaikkan digit-digit itu. Ingat bahwa eksponen negatif
bisa membuat angka menjadi lebih kecil.

Bayangkan kita memakai $10$ sebagai bilangan yang dipangkatkan. Kita bisa
merepresentasikan angka-angka berikut dengan significand $12345$ dan
eksponen $-3$, $4$, dan $0$ untuk meng-encode nilai floating point
berikut:

$12345\times10^{-3}=12.345$

$12345\times10^4=123450000$

$12345\times10^0=12345$

Untuk semua angka itu, significand-nya tetap sama. Satu-satunya perbedaan
adalah eksponennya.

Di mesinmu, basis untuk eksponen kemungkinan adalah $2$, bukan $10$,
karena komputer suka biner. Kamu bisa mengeceknya dengan mencetak makro
`FLT_RADIX`.

Jadi kita punya sebuah angka yang direpresentasikan oleh sejumlah byte,
yang di-encode dengan cara tertentu. Karena pola bit terbatas, jumlah
bilangan floating point yang bisa direpresentasikan juga terbatas.

Lebih khusus lagi, hanya sejumlah tertentu digit desimal signifikan yang
bisa direpresentasikan secara akurat.

Bagaimana cara mendapat lebih banyak? Pakai tipe data yang lebih besar!

Dan kita punya beberapa. Kita sudah kenal `float`, tapi untuk presisi
lebih besar ada `double`. Dan untuk presisi yang lebih besar lagi, ada
`long double` (tidak berhubungan dengan `long int` kecuali namanya).

Spesifikasi tidak menentukan berapa byte penyimpanan yang dipakai setiap
tipe, tapi di sistemku, kita bisa melihat kenaikan ukuran relatifnya:

[i[`double` type]<]
[i[`long double` type]<]

|Tipe|`sizeof`|
|:-|-:|
|`float`|4|
|`double`|8|
|`long double`|16|

Jadi setiap tipe (di sistemku) memakai bit tambahan itu untuk
presisi yang lebih tinggi.

Tapi _seberapa besar_ presisinya? Berapa banyak angka desimal yang bisa
direpresentasikan oleh nilai-nilai ini?

Nah, C menyediakan sejumlah makro di `<float.h>` untuk membantu kita
mengetahuinya.

Ini jadi agak aneh kalau kamu memakai sistem basis-2 (biner) untuk
menyimpan angka (yang dipakai hampir semua orang di planet ini, mungkin
termasuk kamu), tapi sabar saja sementara kita mengurainya.
[i[`double` type]>]
[i[`long double` type]>]

### Berapa Banyak Digit Desimal?

[i[Significant digits]<]
Pertanyaan sejuta dolarnya adalah, "Berapa banyak digit desimal signifikan
yang bisa disimpan dalam tipe floating point tertentu sehingga ketika
dicetak, kita mendapat angka desimal yang sama?"

Jumlah digit desimal yang bisa disimpan dalam tipe floating point dan
pasti kembali sama saat dicetak diberikan oleh makro-makro ini:

[i[`FLT_DIG` macro]<]
[i[`DBL_DIG` macro]<]
[i[`LDBL_DIG` macro]<]

|Tipe|Digit Desimal yang Bisa Disimpan|Minimum|Sistemku|
|:-|-:|-:|
|`float`|`FLT_DIG`|6|6|
|`double`|`DBL_DIG`|10|15|
|`long double`|`LDBL_DIG`|10|18|

[i[`DBL_DIG` macro]>]
[i[`LDBL_DIG` macro]>]

Di sistemku, `FLT_DIG` adalah 6, jadi aku bisa yakin kalau sebuah
`float` 6 digit dicetak, hasilnya sama. (Bisa saja lebih dari 6
digit---sebagian angka tetap kembali dengan benar dengan lebih banyak
digit. Tapi 6 pasti kembali.)

Misalnya, kalau kita mencetak `float` dengan pola digit yang bertambah
seperti ini, kelihatannya kita sampai 8 digit sebelum ada yang salah,
tapi setelah itu kembali ke 7 digit yang benar.

``` {.default}
0.12345
0.123456
0.1234567
0.12345678
0.123456791  <-- Things start going wrong
0.1234567910
```

Kita coba demo lain. Pada kode ini ada dua `float` yang masing-masing
menyimpan angka dengan `FLT_DIG` digit desimal signifikan^[Program ini
berjalan sesuai komentarnya pada sistem dengan `FLT_DIG` bernilai `6` yang
memakai bilangan floating point IEEE-754 basis-2. Kalau tidak, hasilnya
mungkin berbeda.]. Lalu keduanya dijumlahkan, yang seharusnya menghasilkan
12 digit desimal signifikan. Tapi itu lebih dari yang bisa disimpan dalam
`float` dan dipulihkan dengan benar sebagai string---jadi ketika dicetak,
terlihat ada yang mulai salah setelah digit signifikan ke-7.


``` {.c .numberLines}
#include <stdio.h>
#include <float.h>

int main(void)
{
    // Kedua angka ini punya 6 digit signifikan, jadi bisa
    // disimpan secara akurat dalam float:

    float f = 3.14159f;
    float g = 0.00000265358f;

    printf("%.5f\n", f);   // 3.14159       -- benar!
    printf("%.11f\n", g);  // 0.00000265358 -- benar!

    // Sekarang jumlahkan
    f += g;                // 3.14159265358 adalah nilai f yang _seharusnya_

    printf("%.11f\n", f);  // 3.14159274101 -- salah!
}
```

(Kode di atas punya `f` setelah konstanta numeriknya---ini menandakan
bahwa konstanta itu bertipe `float`, bukan `double` yang menjadi
bawaannya. Soal ini dibahas nanti.)

Ingat bahwa `FLT_DIG` adalah jumlah digit aman yang bisa disimpan dalam
`float` dan diambil kembali dengan benar.

Kadang kamu bisa mendapat satu atau dua digit lebih. Tapi kadang kamu
hanya mendapat `FLT_DIG` digit. Yang pasti: kalau kamu menyimpan angka
dengan jumlah digit sampai dengan `FLT_DIG` dalam `float`, dijamin
kembali dengan benar.

Jadi begitulah ceritanya. `FLT_DIG`. Tamat.

[i[`FLT_DIG` macro]>]

...Atau belum?

### Mengonversi ke Desimal dan Kembali

Tapi menyimpan bilangan basis 10 dalam bilangan floating point lalu
mengambilnya kembali baru setengah cerita.

Ternyata bilangan floating point bisa meng-encode angka yang butuh lebih
banyak angka desimal untuk dicetak sepenuhnya. Hanya saja angka desimal
besarmu mungkin tidak cocok dengan salah satu angka itu.

Artinya, kalau kamu melihat bilangan floating point dari satu ke yang
berikutnya, ada celah di antaranya. Kalau kamu mencoba meng-encode angka
desimal di celah itu, yang dipakai adalah bilangan floating point
terdekat. Itulah kenapa kamu hanya bisa meng-encode `FLT_DIG` pada
`float`.

Tapi bagaimana dengan bilangan floating point yang _tidak_ ada di celah?
Berapa tempat desimal yang dibutuhkan untuk mencetaknya secara akurat?

Cara lain menyatakan pertanyaan ini: untuk bilangan floating point
tertentu, berapa digit desimal yang harus dipertahankan kalau kita ingin
mengonversi angka desimal itu kembali menjadi bilangan floating point yang
identik? Artinya, berapa digit yang harus dicetak di basis 10 untuk
memulihkan **semua** digit basis 2 pada angka aslinya?

Kadang hanya perlu beberapa. Tapi supaya yakin, kamu perlu mengonversi ke
desimal dengan jumlah tempat desimal aman tertentu. Angka itu di-encode
dalam makro-makro berikut:

[i[`FLT_DECMIAL_DIG` macro]<]
[i[`DBL_DECMIAL_DIG` macro]<]
[i[`LDBL_DECMIAL_DIG` macro]<]
[i[`DECMIAL_DIG` macro]<]

|Makro|Deskripsi|
|-----------|---------------------------------------------------|
|`FLT_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `float`.|
|`DBL_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `double`.|
|`LDBL_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `long double`.|
|`DECIMAL_DIG`|Sama dengan encoding terlebar, `LDBL_DECIMAL_DIG`.|

[i[`LDBL_DECMIAL_DIG` macro]>]
[i[`DECMIAL_DIG` macro]>]

Mari kita lihat contoh ketika `DBL_DIG` adalah 15 (jadi hanya segitu yang
bisa ada di konstanta), tapi `DBL_DECIMAL_DIG` adalah 17 (jadi kita harus
mengonversi ke 17 angka desimal untuk mempertahankan semua bit dari
`double` aslinya).

Kita tugaskan angka 15 digit signifikan `0.123456789012345` ke `x`, dan
angka 1 digit signifikan `0.0000000000000006` ke `y`.

``` {.default}
x is exact: 0.12345678901234500    Printed to 17 decimal places
y is exact: 0.00000000000000060
```

Tapi mari kita jumlahkan. Hasilnya seharusnya `0.1234567890123456`, tapi
itu lebih dari `DBL_DIG`, jadi mungkin terjadi hal-hal aneh... kita lihat:

``` {.default}
x + y not quite right: 0.12345678901234559    Should end in 4560!
```

Itulah yang kita dapat karena mencetak lebih dari `DBL_DIG`, kan? Tapi
lihat ini... angka di atas itu bisa direpresentasikan persis seperti
adanya!

Kalau kita tugaskan `0.12345678901234559` (17 digit) ke `z` dan
mencetaknya, hasilnya:

``` {.default}
z is exact: 0.12345678901234559   17 digits correct! More than DBL_DIG!
```

Kalau `z` dipotong menjadi 15 digit, angkanya tidak akan sama. Itulah
kenapa untuk mempertahankan semua bit sebuah `double`, kita butuh
`DBL_DECIMAL_DIG` dan bukan sekadar `DBL_DIG` yang lebih kecil.

[i[`DBL_DECMIAL_DIG` macro]>]

Jadi, jelas bahwa saat bermain dengan angka desimal secara
umum, tidak aman mencetak lebih dari [i[`FLT_DIG`
macro]]`FLT_DIG`, [i[`DBL_DIG` macro]]`DBL_DIG`, atau [i[`LDBL_DIG`
macro]]`LDBL_DIG` digit agar tetap masuk akal terhadap angka basis 10
aslinya dan perhitungan matematika berikutnya.

Tapi saat mengonversi dari `float` ke representasi desimal dan _kembali_
ke `float`, pasti pakai `FLT_DECIMAL_DIG` agar semua bit terpertahankan
persis. [i[`FLT_DECMIAL_DIG` macro]>]

[i[Significant digits]>]

## Tipe Konstanta Numerik

Saat kamu menulis konstanta angka, seperti `1234`, ia punya tipe. Tapi
tipe apa? Mari kita lihat bagaimana C menentukan tipe konstanta itu, dan
cara memaksanya memilih tipe tertentu.

### Heksadesimal dan Oktal

Selain desimal biasa seperti buatan nenek, C juga mendukung konstanta
dengan basis berbeda.

[i[`0x` hexadecimal]<]

Kalau angka diawali `0x`, ia dibaca sebagai angka hex:

``` {.c}
int a = 0x1A2B;   // Heksadesimal
int b = 0x1a2b;   // Huruf besar/kecil tidak berpengaruh untuk digit hex

printf("%x", a);  // Cetak angka hex, "1a2b"
```

[i[`0x` hexadecimal]>]


[i[`0` octal]<]
Kalau angka diawali `0`, ia dibaca sebagai angka oktal:

``` {.c}
int a = 012;

printf("%o\n", a);  // Cetak angka oktal, "12"
```

Ini terutama jadi masalah bagi programmer pemula yang mencoba mengisi
angka desimal dengan `0` di kiri agar rapi dan sejajar, dan tanpa sadar
mengubah basis angkanya:

``` {.c}
int x = 11111;  // Desimal 11111
int y = 00111;  // Desimal 73 (Oktal 111)
int z = 01111;  // Desimal 585 (Oktal 1111)
```

[i[`0` octal]>]

#### Catatan tentang Biner

[i[`0b` binary]<]

Ekstensi tidak resmi^[Aku sungguh heran C belum memasukkan ini ke
spesifikasi. Di dokumen C99 Rationale, tertulis, "A proposal to add binary
constants was rejected due to lack of precedent and insufficient utility."
Padahal itu terdengar agak konyol, mengingat fitur-fitur lain yang
dijejalkan ke sana! Aku berani taruhan salah satu rilis berikutnya akan
memilikinya.] di banyak compiler C memungkinkan kita menulis bilangan
biner dengan awalan `0b`:

``` {.c}
int x = 0b101010;    // Biner 101010

printf("%d\n", x);   // Mencetak 42 desimal
```

Tidak ada format specifier `printf()` untuk mencetak bilangan biner. Kamu
harus melakukannya satu karakter demi satu karakter dengan operator
bitwise.

[i[`0b` binary]>]

### Konstanta Integer

[i[Integer constants]<]

Kamu bisa memaksa konstanta integer menjadi tipe tertentu dengan
menambahkan sufiks yang menunjukkan tipenya.

Kita akan melakukan beberapa penugasan sebagai demo, tapi paling sering
developer tidak menulis sufiks kecuali perlu presisi. Compiler cukup
pandai memastikan tipe-tipenya kompatibel.

[i[`U` unsigned constant]<]
[i[`L` long constant]<]
[i[`LL` long long constant]<]
[i[`UL` unsigned long constant]<]
[i[`ULL` unsigned long long constant]<]

``` {.c}
int           x = 1234;
long int      x = 1234L;
long long int x = 1234LL

unsigned int           x = 1234U;
unsigned long int      x = 1234UL;
unsigned long long int x = 1234ULL;
```

Sufiks boleh huruf besar maupun kecil. Dan `U` serta `L` atau `LL` boleh
muncul dalam urutan mana pun.

|Tipe|Sufiks|
|:-|:-|
|`int`|Tidak ada|
|`long int`|`L`|
|`long long int`|`LL`|
|`unsigned int`|`U`|
|`unsigned long int`|`UL`|
|`unsigned long long int`|`ULL`|

Tadi di tabel kusebut bahwa "tanpa sufiks" berarti `int`... tapi
sebenarnya lebih rumit dari itu.

Jadi apa yang terjadi kalau ada angka tanpa sufiks seperti:

``` {.c}
int x = 1234;
```

Bertipe apa angka itu?

Umumnya C memilih tipe terkecil, mulai dari `int`, yang bisa menampung
nilainya.

Tapi lebih spesifik lagi, itu juga bergantung pada basis angkanya
(desimal, hex, atau oktal).

Spesifikasi punya tabel bagus yang menunjukkan tipe mana yang dipakai
untuk nilai tanpa sufiks. Bahkan, tabel itu akan kusalin utuh di sini.

C11 §6.4.4.1¶5 berbunyi, "The type of an integer constant is the first of
the first of the corresponding list in which its value can be
represented."

Lalu dilanjutkan dengan tabel ini:

+----------------+------------------------+-------------------------+
|Sufiks          |Konstanta Desimal       |Konstanta Oktal atau\    |
|                |                        |Heksadesimal             |
+:===============+:=======================+:========================+
|tidak ada       |`int`\                  |`int`\                   |
|                |`long int`              |`unsigned int`\          |
|                |                        |`long int`\              |
|                |                        |`unsigned long int`\     |
|                |                        |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u` atau `U`    |`unsigned int`\         |`unsigned int`\          |
|                |`unsigned long int`\    |`unsigned long int`\     |
|                |`unsigned long long int`|`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`l` atau `L`    |`long int`\             |`long int`\              |
|                |`long long int`         |`unsigned long int`\     |
|                |                        |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u`/`U` dan\    |`unsigned long int`\    |`unsigned long int`\     |
|`l`/`L`         |`unsigned long long int`|`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`ll` atau `LL`  |`long long int`         |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u`/`U` dan\    |`unsigned long long int`|`unsigned long long int` |
|`ll`/`LL`       |                        |                         |
+----------------+------------------------+-------------------------+

[i[`L` long constant]>]
[i[`LL` long long constant]>]
[i[`UL` unsigned long constant]>]
[i[`ULL` unsigned long long constant]>]

Artinya, misalnya kalau kamu menulis angka seperti `123456789U`, mula-mula
C akan melihat apakah ia bisa menjadi `unsigned int`. Kalau tidak muat,
dicoba `unsigned long int`. Lalu `unsigned long long int`. Ia memakai tipe
terkecil yang bisa menampung angka itu.

[i[`U` unsigned constant]>]

[i[Integer constants]>]

### Konstanta Floating Point

[i[Floating point constants]<]

Kamu mungkin mengira konstanta floating point seperti `1.23` bertipe
bawaan `float`, kan?

Kejutan! Ternyata bilangan floating point tanpa sufiks bertipe `double`!
Selamat ulang tahun yang terlambat!

[i[`F` float constant]<]
[i[`L` long double constant]<]

Kamu bisa memaksanya menjadi `float` dengan menambahkan `f` (atau
`F`---tidak membedakan huruf besar/kecil). Kamu bisa memaksanya menjadi
`long double` dengan menambahkan `l` (atau `L`).

|Tipe|Sufiks|
|:-|:-|
|`float`|`F`|
|`double`|Tidak ada|
|`long double`|`L`|

Misalnya:

``` {.c}
float x       = 3.14f;
double x      = 3.14;
long double x = 3.14L;
```

[i[`F` float constant]>]
[i[`L` long double constant]>]

Tapi selama ini kita melakukan ini saja, kan?

``` {.c}
float x = 3.14;
```

Bukankah sisi kiri `float` dan sisi kanan `double`? Betul! Tapi C cukup
pandai soal konversi numerik otomatis, jadi lebih umum menulis konstanta
floating point tanpa sufiks daripada dengan sufiks. Nanti dibahas lagi.

[i[Floating point constants]>]

#### Notasi Ilmiah

[i[Scientific notation]<]

Ingat tadi saat kita membahas bahwa bilangan floating point bisa
direpresentasikan dengan significand, basis, dan eksponen?

Nah, ada cara umum menulis bilangan seperti itu, ditampilkan di sini
diikuti padanannya yang lebih mudah dikenali, yaitu hasil perhitungan
sebenarnya:

$1.2345\times10^3 = 1234.5$

Menulis angka dalam bentuk $s\times b^e$ disebut [flw[_notasi
ilmiah_|Scientific_notation]]. Di C, ini ditulis dengan "notasi E", jadi
dua bentuk ini setara:

|Notasi Ilmiah|Notasi E|
|:-|:-|
|$1.2345\times10^{-3}=0.0012345$|`1.2345e-3`|
|$1.2345\times10^8=123450000$|`1.2345e+8`|

Kamu bisa mencetak angka dalam notasi ini dengan `%e`:

``` {.c}
printf("%e\n", 123456.0);  // Mencetak 1.234560e+05
```

Beberapa fakta kecil yang menyenangkan tentang notasi ilmiah:

* Angka tidak harus ditulis dengan satu digit di depan titik desimal.
  Berapa pun jumlah angka boleh ditaruh di depan.

  ``` {.c}
  double x = 123.456e+3;  // 123456
  ```

  Tapi saat dicetak, eksponennya akan diubah sehingga hanya ada satu
  digit di depan titik desimal.

* Tanda plus pada eksponen boleh dihilangkan karena itu bawaannya, tapi
  dari yang pernah kulihat, ini jarang dipakai.

  ``` {.c}
  1.2345e10 == 1.2345e+10
  ```
* Kamu bisa menambahkan sufiks `F` atau `L` pada konstanta notasi E:

  ``` {.c}
  1.2345e10F
  1.2345e10L
  ```

[i[Scientific notation]>]

#### Konstanta Floating Point Heksadesimal

[i[Hex floating point constants]<]

Tapi tunggu, masih ada lagi yang mengambang!

Ternyata ada juga konstanta floating point heksadesimal!

Cara kerjanya mirip bilangan floating point desimal, tapi diawali `0x`
seperti angka integer.

Syaratnya, kamu _harus_ menyebutkan eksponen, dan eksponen ini
menghasilkan pangkat 2. Yaitu: $2^x$.

Lalu kamu memakai `p` alih-alih `e` saat menulis angkanya:

Jadi `0xa.1p3` adalah $10.0625\times2^3 == 80.5$.

Saat memakai konstanta hex floating point, 
Kita bisa mencetak notasi ilmiah hex dengan `%a`:

``` {.c}
double x = 0xa.1p3;

printf("%a\n", x);  // 0x1.42p+6
printf("%f\n", x);  // 80.500000
```

[i[Hex floating point constants]>]

