<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Locale dan Internasionalisasi

[i[Locale]<]

_Lokalisasi_ adalah proses membuat aplikasimu siap bekerja dengan baik di
locale (atau negara) yang berbeda.

Seperti yang mungkin kamu tahu, tidak semua orang memakai karakter yang
sama untuk titik desimal atau pemisah ribuan... atau untuk mata uang.

Locale-locale ini punya nama, dan kamu bisa memilih salah satu untuk
dipakai. Misalnya, locale AS mungkin menulis angka seperti:

100,000.00

Sedangkan di Brasil, angka yang sama mungkin ditulis dengan koma dan titik
desimal bertukar tempat:

100.000,00

Jadi lebih mudah menulis kodemu supaya bisa dibawa ke negara lain dengan mulus!

Ya, kurang lebih begitu. Ternyata C hanya punya satu locale bawaan, dan
itu pun terbatas. Spesifikasi benar-benar menyisakan banyak ambiguitas di
sini; sulit untuk sepenuhnya portabel.

Tapi kita akan berusaha sebaik mungkin!

## Mengatur Lokalisasi, Cara Cepat dan Kotor

Untuk pemanggilan ini, include [i[`locale.h` header file]] `<locale.h>`.

Pada dasarnya hanya ada satu hal yang bisa kamu lakukan secara portabel
dalam hal mendeklarasikan locale tertentu. Ini kemungkinan besar yang ingin kamu
lakukan kalau kamu akan mengerjakan apa pun terkait locale:

[i[`setlocale()` function]<]

``` {.c}
setlocale(LC_ALL, "");  // Pakai locale environment ini untuk semuanya
```

Kamu perlu memanggilnya supaya program diinisialisasi dengan locale
yang sedang berlaku.

Lebih rinci, ada satu hal lagi yang bisa dilakukan dan tetap portabel:

``` {.c}
setlocale(LC_ALL, "C");  // Pakai locale C bawaan
```

tapi itu dipanggil secara bawaan setiap kali programmu mulai, jadi tidak
banyak alasan untuk melakukannya sendiri.

Pada string kedua itu, kamu bisa menentukan locale apa pun yang didukung
sistemmu. Ini sepenuhnya bergantung pada sistem, jadi bervariasi. Di
sistemku, aku bisa menentukan ini:

``` {.c}
setlocale(LC_ALL, "en_US.UTF-8");  // Tidak portabel!
```

Dan itu akan berfungsi. Tapi hanya portabel ke sistem yang punya nama yang
persis sama untuk locale yang persis sama, dan itu tidak bisa dijamin.

Dengan mengoper string kosong (`""`) sebagai argumen kedua, kamu berkata
kepada C, "Hei, cari tahu locale sistem ini saat ini apa supaya aku tidak
perlu memberi tahu."

[i[`setlocale()` function]>]

## Mengambil Pengaturan Locale Moneter

[i[Locale-->money]<]

Karena memindah-mindahkan lembaran kertas hijau menjanjikan kunci
kebahagiaan^["This planet has---or rather had---a problem, which was this:
most of the people living on it were unhappy for pretty much of the
time. Many solutions were suggested for this problem, but most of these
were largely concerned with the movement of small green pieces of paper,
which was odd because on the whole it wasn't the small green pieces of
paper that were unhappy." ---The Hitchhiker's Guide to the Galaxy,
Douglas Adams], mari bicara tentang locale moneter. Saat menulis kode
portabel, kamu harus tahu apa yang diketik untuk uang tunai, kan?
Entah itu "$", "€", "¥", atau "£".

[i[`localeconv()` function]<]

Bagaimana menulis kode itu tanpa jadi gila? Untungnya, setelah kamu
memanggil `setlocale(LC_ALL, "")`, kamu bisa mencari semuanya dengan
memanggil `localeconv()`:

``` {.c}
struct lconv *x = localeconv();
```

Fungsi ini mengembalikan pointer ke `struct lconv` yang dialokasikan
secara statis dan berisi semua informasi menarik yang kamu cari.

Berikut field-field `struct lconv` beserta artinya.

Pertama, beberapa konvensi. `_p_` berarti "positif", dan `_n_` berarti
"negatif", dan `int_` berarti "internasional". Walaupun banyak di antaranya
bertipe `char` atau `char*`, kebanyakan (atau string yang ditunjuknya)
sebenarnya diperlakukan sebagai integer^[Ingat bahwa `char` hanyalah
integer seukuran byte.].

Sebelum lanjut, ketahuilah bahwa `CHAR_MAX` (dari `<limits.h>`) adalah
nilai maksimum yang bisa ditampung `char`. Dan banyak nilai `char` berikut
memakainya untuk menandakan bahwa nilai itu tidak tersedia di locale
tersebut.

|Field|Deskripsi|
|-----|-----------|
|`char *mon_decimal_point`|Karakter titik desimal untuk uang, mis. `"."`.|
|`char *mon_thousands_sep`|Karakter pemisah ribuan untuk uang, mis. `","`.|
|`char *mon_grouping`|Deskripsi pengelompokan untuk uang (lihat di bawah).|
|`char *positive_sign`|Tanda positif untuk uang, mis. `"+"` atau `""`.|
|`char *negative_sign`|Tanda negatif untuk uang, mis. `"-"`.|
|`char *currency_symbol`|Simbol mata uang, mis. `"$"`.|
|`char frac_digits`|Saat mencetak jumlah uang, berapa digit yang dicetak setelah titik desimal, mis. `2`.|
|`char p_cs_precedes`|`1` jika `currency_symbol` muncul sebelum nilai untuk jumlah uang non-negatif, `0` jika sesudah.|
|`char n_cs_precedes`|`1` jika `currency_symbol` muncul sebelum nilai untuk jumlah uang negatif, `0` jika sesudah.|
|`char p_sep_by_space`|Menentukan pemisahan `currency symbol` dari nilai untuk jumlah non-negatif (lihat di bawah).|
|`char n_sep_by_space`|Menentukan pemisahan `currency symbol` dari nilai untuk jumlah negatif (lihat di bawah).|
|`char p_sign_posn`|Menentukan posisi `positive_sign` untuk nilai non-negatif.|
|`char n_sign_posn`|Menentukan posisi `positive_sign` untuk nilai negatif.|
|`char *int_curr_symbol`|Simbol mata uang internasional, mis. `"USD "`.|
|`char int_frac_digits`|Nilai internasional untuk `frac_digits`.|
|`char int_p_cs_precedes`|Nilai internasional untuk `p_cs_precedes`.|
|`char int_n_cs_precedes`|Nilai internasional untuk `n_cs_precedes`.|
|`char int_p_sep_by_space`|Nilai internasional untuk `p_sep_by_space`.|
|`char int_n_sep_by_space`|Nilai internasional untuk `n_sep_by_space`.|
|`char int_p_sign_posn`|Nilai internasional untuk `p_sign_posn`.|
|`char int_n_sign_posn`|Nilai internasional untuk `n_sign_posn`.|

[i[`localeconv()` function]>]

### Pengelompokan Digit Moneter {#monetary-digit-grouping}

[i[`localeconv()` function-->`mon_grouping`]<]

Oke, yang ini membingungkan. `mon_grouping` adalah `char*`, jadi mungkin
kamu mengira itu string. Tapi dalam kasus ini, bukan, ia sebenarnya
array berisi `char`. Ia harus selalu berakhir dengan `0` atau `CHAR_MAX`.

Nilai-nilai ini menjelaskan cara mengelompokkan deretan angka pada mata
uang di sebelah _kiri_ desimal (bagian bilangan bulat).

Misalnya, kita mungkin punya:

``` {.default}
  2   1   0
 --- --- ---
$100,000,000.00
```

Ini kelompok-kelompok tiga. Grup 0 (tepat di kiri desimal) punya 3 digit.
Grup 1 (grup berikutnya ke kiri) punya 3 digit, dan yang terakhir juga 3.

Jadi kita bisa menggambarkan grup-grup ini, dari kanan (desimal) ke kiri
dengan serangkaian nilai integer yang mewakili ukuran grup:

``` {.default}
3 3 3
```

Dan itu akan berfungsi untuk nilai sampai $100,000,000.

Tapi bagaimana kalau lebih banyak? Kita bisa terus menambah `3`...

``` {.default}
3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
```

tapi itu gila. Untungnya, kita bisa menentukan `0` untuk menandakan bahwa
ukuran grup sebelumnya berulang:

``` {.default}
3 0
```

Yang berarti mengulang setiap 3. Itu akan menangani $100, $1,000, $10,000,
$10,000,000, $100,000,000,000, dan seterusnya.

Kamu bisa benar-benar menggila dengan ini untuk menandakan pengelompokan
yang aneh.

Misalnya:

``` {.default}
4 3 2 1 0
```

akan menandakan:

``` {.default}
$1,0,0,0,0,00,000,0000.00
```

Satu nilai lagi yang bisa muncul adalah `CHAR_MAX`. Ini menandakan bahwa
tidak ada pengelompokan lagi, dan bisa muncul di mana saja dalam array,
termasuk di nilai pertama.

``` {.default}
3 2 CHAR_MAX
```

akan menandakan:

``` {.default}
100000000,00,000.00
```

misalnya.

Dan hanya dengan adanya `CHAR_MAX` di posisi pertama array, itu memberi
tahu bahwa tidak ada pengelompokan sama sekali.

[i[`localeconv()` function-->`mon_grouping`]>]

### Pemisah dan Posisi Tanda

[i[`localeconv()` function-->`sep_by_space`]<]

Semua varian `sep_by_space` berkaitan dengan spasi di sekitar tanda mata
uang. Nilai yang valid:

|Nilai|Deskripsi|
|:--:|------------|
|`0`|Tidak ada spasi antara simbol mata uang dan nilai.|
|`1`|Pisahkan simbol mata uang (dan tanda, jika ada) dari nilai dengan spasi.|
|`2`|Pisahkan simbol tanda dari simbol mata uang (jika bersebelahan) dengan spasi, selain itu pisahkan simbol tanda dari nilai dengan spasi.|

Varian `sign_posn` ditentukan oleh nilai-nilai berikut:

|Nilai|Deskripsi|
|:--:|------------|
|`0`|Taruh tanda kurung di sekitar nilai dan simbol mata uang.|
|`1`|Taruh string tanda di depan simbol mata uang dan nilai.|
|`2`|Taruh string tanda setelah simbol mata uang dan nilai.|
|`3`|Taruh string tanda tepat di depan simbol mata uang.|
|`4`|Taruh string tanda tepat di belakang simbol mata uang.|

[i[`localeconv()` function-->`sep_by_space`]>]
[i[Locale-->money]>]

### Contoh Nilai

Waktu kuambil nilai-nilainya di sistemku, inilah yang
kulihat (string pengelompokan ditampilkan sebagai nilai byte individual):

``` {.c}
mon_decimal_point  = "."
mon_thousands_sep  = ","
mon_grouping       = 3 3 0
positive_sign      = ""
negative_sign      = "-"
currency_symbol    = "$"
frac_digits        = 2
p_cs_precedes      = 1
n_cs_precedes      = 1
p_sep_by_space     = 0
n_sep_by_space     = 0
p_sign_posn        = 1
n_sign_posn        = 1
int_curr_symbol    = "USD "
int_frac_digits    = 2
int_p_cs_precedes  = 1
int_n_cs_precedes  = 1
int_p_sep_by_space = 1
int_n_sep_by_space = 1
int_p_sign_posn    = 1
int_n_sign_posn    = 1
```

## Spesifik Lokalisasi

Perhatikan bagaimana tadi kita mengoper makro `LC_ALL` ke `setlocale()`...
ini mengisyaratkan bahwa mungkin ada varian yang memungkinkan kamu lebih
presisi soal _bagian_ locale mana yang diatur.

Mari kita lihat nilai-nilai yang bisa dipakai:

[i[`setlocale()` function-->`LC_ALL` macro]]
[i[`setlocale()` function-->`LC_COLLATE` macro]]
[i[`setlocale()` function-->`LC_CTYPE` macro]]
[i[`setlocale()` function-->`LC_MONETARY` macro]]
[i[`setlocale()` function-->`LC_NUMERIC` macro]]
[i[`setlocale()` function-->`LC_TIME` macro]]

|Makro|Deskripsi|
|----|--------------|
|`LC_ALL`|Atur semua yang berikut ke locale yang diberikan.|
|`LC_COLLATE`|Mengatur perilaku fungsi `strcoll()` dan `strxfrm()`.|
|`LC_CTYPE`|Mengatur perilaku fungsi penanganan karakter^[Kecuali `isdigit()` dan `isxdigit()`.].|
|`LC_MONETARY`|Mengatur nilai yang dikembalikan `localeconv()`.|
|`LC_NUMERIC`|Mengatur titik desimal untuk keluarga fungsi `printf()`.|
|`LC_TIME`|Mengatur format waktu pada fungsi pencetak waktu dan tanggal `strftime()` dan `wcsftime()`.|

Cukup umum melihat [i[`setlocale()` function-->`LC_ALL` macro]]
`LC_ALL` yang diatur, tapi, setidaknya kamu punya pilihan.

Aku juga perlu menunjukkan bahwa [i[`setlocale()` function-->`LC_CTYPE`
macro]] `LC_CTYPE` adalah salah satu yang besar karena terkait dengan wide
character, kaleng cacing yang signifikan yang akan kita bahas nanti.

[i[Locale]>]
