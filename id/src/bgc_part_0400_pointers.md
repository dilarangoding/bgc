<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->
# Pointer---Gemetar Ketakutan! {#pointers}

> _"How do you get to Carnegie Hall?"_ \
> _"Practice!"_
>
> ---20th-century joke of unknown origin

[i[Pointers]<]Pointer adalah salah satu hal yang paling ditakuti di bahasa
C. Bahkan, hanya pointer-lah yang membuat bahasa ini terasa menantang.
Tapi kenapa?

Karena, jujur saja, pointer bisa mengirim sengatan listrik lewat keyboard
dan secara fisik _mengelas_ lenganmu di tempat selamanya, mengutukmu
menghabiskan hidup di depan keyboard dengan bahasa dari tahun 70-an ini!

Benar begitu? Tentu tidak. Itu cuma usaha untuk menyiapkan kamu supaya
berhasil.

Tergantung bahasa asalmu, mungkin kamu sudah paham konsep _referensi_,
yaitu variabel yang merujuk ke objek bertipe tertentu.

Pointer sangat mirip dengan itu, hanya saja di C kita harus lebih eksplisit
membedakan kapan kita membicarakan referensinya dan kapan hal yang
dirujuknya.


## Memori dan Variabel {#ptmem}

Memori komputer menyimpan segala jenis data, kan? `float`, `int`, atau apa
pun yang kamu punya. Agar mudah dikelola, setiap byte memori diberi
identitas berupa sebuah integer. Integer-integer ini naik berurutan
sepanjang memori^[Biasanya. Pasti ada pengecualian di lorong-lorong gelap
sejarah komputasi.]. Bayangkan sekumpulan kotak bernomor, dan setiap kotak
menyimpan satu byte^[Byte adalah angka yang terdiri dari tidak lebih dari
8 digit biner, atau singkatnya _bit_. Dalam digit desimal seperti yang
biasa dipakai nenek kita, byte bisa menyimpan bilangan tak bertanda
antara 0 dan 255, inklusif.] data. Atau bayangkan array besar yang setiap
elemennya menyimpan satu byte, kalau kamu berasal dari bahasa yang punya
array. Angka yang menandai setiap kotak disebut _alamat_[i[Memory
address]]-nya.

Nah, tidak semua tipe data hanya memakai satu byte. Misalnya, sebuah `int`
sering kali empat byte, begitu juga `float`, tapi itu benar-benar
tergantung sistem. Kamu bisa memakai operator `sizeof` untuk mengetahui
berapa byte memori yang dipakai sebuah tipe.

``` {.c}
// %zu adalah format specifier untuk tipe size_t

printf("an int uses %zu bytes of memory\n", sizeof(int));

// Di sistem penulis ini mencetak "4", tapi bisa berbeda tiap sistem.
```

> **Fakta Menarik tentang Memori**: Kalau sebuah tipe data (seperti `int`
> biasa) memakai lebih dari satu byte memori, byte-byte penyusunnya selalu
> berdekatan di memori. Kadang urutannya sesuai harapanmu, kadang
> tidak^[Urutan byte disebut _endianness_ dari angka tersebut. Yang paling
> sering ditemui adalah _big-endian_ (byte paling signifikan di depan)
> dan _little-endian_ (byte paling signifikan di belakang), atau yang kini
> jarang, _mixed-endian_ (byte paling signifikan di tempat lain).].
> Walau C tidak menjamin urutan memori tertentu (tergantung platform),
> pada umumnya kamu tetap bisa menulis kode yang tidak bergantung platform,
> tanpa harus memikirkan urutan byte yang menjengkelkan ini.

Jadi, _anyway_, kalau sudah siap, putar drum roll dan musik menegangkan
untuk definisi pointer: _pointer adalah variabel yang menyimpan sebuah
alamat_. Bayangkan musik klasik dari 2001: A Space Odyssey di sini.
Ba bum ba bum ba bum BAAAAH!

Ok, mungkin agak berlebihan ya? Sebenarnya pointer tidak punya banyak
misteri. Pointer adalah alamat dari data. Sama seperti variabel `int`
bisa menyimpan nilai `12`, variabel pointer bisa menyimpan alamat data.

Artinya, semua ini bermakna sama, yaitu sebuah angka yang menyatakan
suatu titik di memori:

* Indeks ke dalam memori (kalau kamu membayangkan memori sebagai array
  besar)
* Alamat
* Lokasi

Ketiga istilah ini dipakai bergantian. Dan ya, _lokasi_ baru saja
ditambahkan karena kata yang bermakna sama itu tidak pernah cukup.

Variabel pointer menyimpan nomor alamat itu, sama seperti variabel `float`
bisa menyimpan `3.14159`.

Bayangkan kamu punya setumpuk Post-it® yang bernomor berurutan sebagai
alamatnya. (Yang pertama di indeks `0`, berikutnya di indeks `1`, dan
seterusnya.)

Selain angka penanda posisinya, kamu juga bisa menulis angka lain di
masing-masing Post-it. Misalnya jumlah anjingmu. Atau jumlah bulan di
sekitar Mars...

...Atau, _bisa juga indeks Post-it yang lain!_

Kalau yang kamu tulis jumlah anjingmu, itu variabel biasa. Tapi kalau yang
kamu tulis indeks Post-it lain, _itulah pointer_. Ia menunjuk ke catatan
lain!

Analogi lainnya adalah alamat rumah. Sebuah rumah punya ciri-ciri
tertentu: halaman, atap metal, panel surya, dan sebagainya. Lalu ada
alamat rumah itu. Alamat bukanlah rumahnya. Yang satu adalah rumah
lengkap, yang lain hanya beberapa baris teks. Tapi alamat rumah adalah
_pointer_ ke rumah tersebut. Ia bukan rumahnya, tapi memberi tahu di mana
rumahnya.

Hal yang sama bisa kita lakukan di komputer. Ada variabel data yang
menyimpan suatu nilai, dan nilai itu berada di memori pada suatu alamat.
Lalu ada _variabel pointer_ yang berbeda, yang menyimpan alamat variabel
data tadi.

Variabel pointer bukan variabel data itu sendiri, tapi seperti alamat
rumah, ia memberi tahu kita di mana menemukannya.

Kalau sudah begitu, kita bilang kita punya "pointer ke" data tersebut. Dan
kita bisa mengikuti pointer itu untuk mengakses datanya.

(Sekarang mungkin belum terlihat berguna, tapi semua ini jadi sangat
penting ketika dipakai bersama pemanggilan fungsi. Sabar dulu sampai kita
ke sana.)

Jadi, kalau kita punya sebuah `int` dan ingin pointer ke sana, kita butuh
cara untuk mendapatkan alamat `int` itu, kan? Sebab pointer hanya
menyimpan _alamat_ data. Operator apa yang dipakai untuk mencari _alamat_
sebuah `int`?

[i[`&` operator address-of]<]Dengan kejutan yang pasti mengejutkan kamu,
pembaca yang baik, kita memakai operator `address-of` (yang ternyata
adalah ampersand: "`&`") untuk mencari alamat data. Ampersand.

Sebagai contoh cepat, kita perkenalkan _format specifier_ baru untuk
`printf()` agar bisa mencetak pointer. Kamu sudah tahu `%d` mencetak
integer desimal, kan? Nah, `%p`[i[`printf()` fungsi-->with pointers]]
mencetak pointer. Pointer ini akan tampak seperti angka sampah (dan
mungkin dicetak dalam heksadesimal^[Yaitu basis 16 dengan digit 0, 1, 2,
3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, dan F.] alih-alih desimal), tapi itu
hanyalah indeks ke memori tempat data disimpan. (Atau indeks ke memori
tempat byte pertama data disimpan, kalau datanya multi-byte.) Dalam hampir
semua keadaan, termasuk yang ini, nilai angka yang tercetak tidak penting
bagimu, dan hanya ditampilkan untuk mendemonstrasikan operator
`address-of`.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 10;

    printf("The value of i is %d\n", i);
    printf("And its address is %p\n", (void *)&i);
}
```

> **Kode di atas mengandung sebuah _cast_**, yang memaksa tipe ekspresi
> `&i` menjadi `void*`. Tujuannya mencegah compiler mengeluarkan
> peringatan. Ini semua belum kita bahas, jadi untuk sekarang abaikan
> saja `(void*)` di kode di atas dan anggap tidak ada.

Di komputer penulis, ini mencetak:

``` {.default}
The value of i is 10
And its address is 0x7ffddf7072a4
```
[i[`&` operator address-of]>]

Kalau penasaran, angka heksadesimal itu sama dengan 140.727.326.896.068
dalam desimal (basis 10, seperti yang biasa dipakai nenek kita). Itulah
indeks ke memori tempat data variabel `i` disimpan. Itulah alamat `i`.
Itulah lokasi `i`. Itulah pointer ke `i`.

> **Tunggu---RAM kamu 140 terabyte?** Ya! Kamu juga kan? Tapi itu
> bohong besar; tentu saja tidak (sekitar 2024). Komputer modern memakai
> teknologi ajaib bernama [flw[virtual memory|Virtual_memory]] yang membuat
> setiap proses mengira seluruh ruang memori komputer adalah miliknya
> sendiri, berapa pun RAM fisik yang ada di belakangnya. Jadi, meskipun
> alamatnya angka sebesar itu, oleh sistem virtual memory di CPU alamat
> itu dipetakan ke alamat memori fisik yang lebih rendah. Komputer penulis
> ini punya RAM 16 GB (sekali lagi, sekitar 2024, tapi pakai Linux, jadi
> itu sudah lebih dari cukup). Terabyte RAM? Penulis ini guru, bukan
> miliarder dotcom. Tidak ada dari semua ini yang perlu kita khawatirkan,
> kecuali soal penulis yang tidak kaya raya.

Angka ini disebut pointer karena ia memberi tahu di mana `i` berada di memori.
Seperti alamat rumah di secarik kertas yang memberi tahu di mana rumah
tertentu bisa ditemukan, angka ini menunjukkan di mana di memori nilai `i`
bisa ditemukan. Ia menunjuk ke `i`.

Sekali lagi, pada umumnya kita tidak peduli persis berapa nomor alamatnya.
Kita hanya peduli bahwa itu pointer ke `i`.

## Tipe Pointer {#pttypes}

[i[Pointer types]<]Jadi... semua ini bagus. Sekarang kamu sudah bisa
mengambil alamat variabel dan mencetaknya ke layar. Lumayan buat
dipamerkan di resume, kan? Nah, di titik ini kamu menyela dan dengan sopan
bertanya apa gunanya pointer.

Pertanyaan yang sangat bagus, dan akan kita jawab setelah pesan dari
sponsor kita.

```
ACME ROBOTIC HOUSING UNIT CLEANING SERVICES. YOUR HOMESTEAD WILL BE
DRAMATICALLY IMPROVED OR YOU WILL BE TERMINATED. MESSAGE ENDS.
```

Selamat datang kembali di edisi lain Beej's Guide. Terakhir kali kita
bertemu, kita sedang membahas cara memakai pointer. Sekarang kita akan
menyimpan pointer di dalam variabel supaya bisa dipakai nanti. _Tipe
pointer_ bisa dikenali dari tanda asterisk (`*`) setelah nama tipe dan
sebelum nama variabel:

``` {.c .numberLines}
int main(void)
{
    int i;  // tipe i adalah "int"
    int *p; // tipe p adalah "pointer ke int", atau "int-pointer"
}
```

Nah, di sini kita punya variabel bertipe pointer, yang bisa menunjuk ke
sebuah `int` lain. Artinya, ia bisa menyimpan alamat `int` lain. Kita tahu
ia menunjuk ke `int` karena tipenya `int*` (dibaca "int-pointer").

Saat melakukan penugasan ke variabel pointer, tipe di sisi kanan harus
sama dengan tipe variabel pointer-nya. Untungnya, kalau kita mengambil
`address-of` sebuah variabel, tipe hasilnya adalah pointer ke tipe
variabel itu, sehingga penugasan seperti berikut ini sempurna:

``` {.c}
int i;
int *p;  // p adalah pointer, tapi belum diinisialisasi dan menunjuk ke sampah

p = &i;  // p diberi alamat i--sekarang p "menunjuk ke" i
```

Di sisi kiri penugasan, ada variabel bertipe pointer-to-`int` (`int*`),
dan di sisi kanan ada ekspresi bertipe pointer-to-`int` karena `i` adalah
`int` (address-of sebuah `int` menghasilkan pointer ke `int`). Alamat
sesuatu bisa disimpan dalam pointer ke sesuatu itu.

Paham? Memang belum terlalu masuk akal, sebab kamu belum melihat
pemakaian nyata variabel pointer, tapi kita melangkah pelan-pelan supaya
tidak ada yang tersesat. Sekarang, kita kenalkan operator anti-address-of.
Kira-kira ia melakukan kebalikan dari `address-of`, seperti di Dunia
Bizarro.[i[Pointer types]>]

## Dereferencing {#deref}

[i[Dereferencing]<]Variabel pointer bisa dianggap _merujuk_ ke variabel
lain dengan cara menunjuk ke sana. Jarang sekali orang C membicarakan
"merujuk" atau "referensi", tapi itu disebut di sini supaya nama operator
ini lebih masuk akal.

Kalau kamu punya pointer ke sebuah variabel (secara kasar, "referensi ke
variabel"), kamu bisa mengakses variabel aslinya lewat pointer itu dengan
_men-dereference_-nya. (Boleh dibilang ini "de-pointer"-ing pointer, tapi
tidak ada yang pernah mengatakan itu.)

Kembali ke analogi kita, ini seperti melihat alamat rumah lalu pergi ke
rumah itu.

Lalu apa maksud "mengakses variabel aslinya"? Begini: kalau kamu punya
variabel `i` dan pointer ke `i` bernama `p`, kamu bisa memakai pointer
yang sudah di-dereference, `*p`, _persis seperti variabel asli `i`_!

[i[`*` indirection operator]<]Pengetahuanmu sudah hampir cukup untuk
mengerjakan contoh. Satu informasi terakhir: apa operator dereference-nya?
Sebenarnya ia disebut _indirection operator_, karena nilainya diakses
secara tidak langsung lewat pointer. Dan operatornya adalah tanda
asterisk, lagi: `*`. Jangan tertukar dengan asterisk di deklarasi pointer
tadi. Karakternya sama, tapi maknanya berbeda tergantung konteks^[Dan itu
belum semua! Asterisk juga dipakai dalam `/*komentar*/`, perkalian, dan
function prototype dengan variable length array! Semuanya `*` yang sama,
tapi konteksnya memberi makna yang berbeda.].

Berikut contoh lengkapnya:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    int *p;  // ini BUKAN dereference--ini tipe "int*"

    p = &i;  // p sekarang menunjuk ke i, p menyimpan alamat i

    i = 10;  // i sekarang 10
    *p = 20; // yang ditunjuk p (yaitu i!) sekarang 20!!

    printf("i is %d\n", i);   // mencetak "20"
    printf("i is %d\n", *p);  // "20"! dereference-p sama dengan i!
}
```

Ingat bahwa `p` menyimpan alamat `i`, seperti terlihat pada penugasan ke
`p` di baris 8. Indirection operator menyuruh komputer untuk _memakai
objek yang ditunjuk pointer_, bukan pointer itu sendiri. Dengan begitu,
`*p` menjadi semacam alias bagi `i`.[i[Dereferencing]>][i[`*` indirection operator]>]

Bagus, tapi _kenapa_? Kenapa repot-repot semua ini?

## Mengoper Pointer sebagai Argumen {#ptpass}

[i[Pointers-->as arguments]<]Sekarang kamu mungkin merasa sudah punya
banyak pengetahuan tentang pointer, tapi nol penerapan, kan? Maksudnya,
buat apa `*p` kalau kamu bisa langsung menulis `i`?

Nah, kawan, kekuatan sebenarnya dari pointer muncul ketika kamu mulai
mengopernya ke fungsi. Kenapa ini penting? Mungkin kamu ingat bahwa
argumen apa pun yang dioper ke fungsi akan dengan tekun disalin ke
parameter, lalu kamu bisa mengutak-atik salinan lokal itu di dalam
fungsi, dan akhirnya mengembalikan satu nilai.

Bagaimana kalau kamu ingin mendapat lebih dari satu data dari sebuah
fungsi? Kamu kan hanya bisa mengembalikan satu hal. Bagaimana kalau
pertanyaan itu dijawab dengan pertanyaan lain? ...Eh, dua pertanyaan?

Apa yang terjadi kalau sebuah pointer dioper sebagai argumen ke fungsi?
Apakah salinan pointer itu ditaruh di parameter yang sesuai? _Berani
taruhan apa saja: iya._ Ingat bagaimana tadi kita bahas panjang lebar
bahwa _SETIAP ARGUMEN_ disalin ke parameter dan fungsi memakai salinannya?
Hal yang sama berlaku di sini. Fungsi mendapat salinan pointer.

Tapi, dan inilah bagian cerdasnya: kita sudah mengatur pointer itu lebih
dulu agar menunjuk ke suatu variabel... sehingga fungsi bisa
men-dereference salinan pointer-nya untuk sampai ke variabel aslinya!
Fungsi tidak bisa melihat variabel itu secara langsung, tapi ia jelas bisa
men-dereference pointer ke variabel itu!

Analoginya, kamu menulis alamat rumah di secarik kertas lalu menyalinnya
ke kertas lain. Sekarang ada _dua_ pointer ke rumah itu, dan keduanya
sama-sama bisa membawamu ke rumah yang sama.

Pada pemanggilan fungsi, satu salinan disimpan di variabel pointer pada
calling scope, dan salinan lainnya di variabel pointer yang menjadi
parameter fungsi.

Contoh! Kita kembali ke fungsi `increment()` lama, tapi kali ini kita buat
ia benar-benar meng-increment nilai di luar caller.

``` {.c .numberLines}
#include <stdio.h>

void increment(int *p)  // perhatikan bahwa fungsi ini menerima pointer ke int
{
    *p = *p + 1;        // tambahkan satu ke yang ditunjuk p
}

int main(void)
{
    int i = 10;
    int *j = &i;  // perhatikan address-of; mengubahnya menjadi pointer ke i

    printf("i is %d\n", i);        // mencetak "10"
    printf("i is also %d\n", *j);  // mencetak "10"

    increment(j);                  // j adalah int*--ke i

    printf("i is %d\n", i);        // mencetak "11"!
}
```

Oke! Ada beberapa hal yang perlu diperhatikan di sini... terutama bahwa
fungsi `increment()` menerima `int*` sebagai argumen. Kita mengoper `int*`
itu dalam pemanggilan dengan mengubah variabel `int i` menjadi `int*`
lewat operator `address-of`. (Ingat, pointer menyimpan alamat, jadi kita
membuat pointer ke sebuah variabel dengan memasangkan operator `address-of`
padanya.)

Fungsi `increment()` mendapat salinan pointer. Pointer asli `j` (di
`main()`) dan salinannya `p` (parameter di `increment()`) sama-sama
menunjuk ke alamat yang sama, yaitu tempat nilai `i` tersimpan. (Lagi-lagi
secara analogi, seperti dua lembar kertas yang bertuliskan alamat rumah
yang sama.) Men-dereference salah satunya memungkinkan kamu mengubah
variabel asli `i`! Fungsi bisa mengubah variabel di scope lain! Keren!

Contoh di atas sering ditulis lebih ringkas dengan menaruh address-of
langsung di daftar argumen saat pemanggilan:

``` {.c}
printf("i is %d\n", i);  // mencetak "10"
increment(&i);
printf("i is %d\n", i);  // mencetak "11"!
```

Sebagai aturan umum, kalau kamu ingin fungsi mengubah sesuatu yang kamu
oper dan hasilnya terlihat dari luar, oper pointer ke sesuatu itu.

## Pointer `NULL`

[i[`NULL` pointer]<]Variabel pointer dari tipe pointer apa pun bisa
diisi nilai khusus bernama `NULL`. Nilai ini menandakan bahwa pointer
tersebut tidak menunjuk ke apa pun.

``` {.c}
int *p;

p = NULL;
```

Karena tidak menunjuk ke nilai apa pun, men-dereference-nya adalah
undefined behavior, dan kemungkinan besar berakhir dengan crash:

``` {.c}
int *p = NULL;

*p = 12;  // CRASH atau SESUATU YANG BURUK. SEBAIKNYA DIHINDARI.
```

Walaupun penciptanya menyebutnya [flw[kesalahan satu miliar dolar|Null_pointer#History]],
pointer `NULL` adalah [flw[sentinel value|Sentinel_value]] yang bagus dan
penanda umum bahwa pointer belum diinisialisasi.

(Tentu saja, seperti variabel lain, pointer akan menunjuk ke sampah kecuali
kamu secara eksplisit menugaskannya untuk menunjuk ke sebuah alamat atau
`NULL`.)
[i[`NULL` pointer]>]

## Catatan tentang Mendeklarasikan Pointer

[i[Pointers-->declarations]<]Sintaks deklarasi pointer bisa terlihat
sedikit aneh. Lihat contoh ini:

``` {.c}
int a;
int b;
```

Kita bisa memadatkan itu menjadi satu baris, kan?

``` {.c}
int a, b;  // Sama saja
```

Jadi `a` dan `b` keduanya adalah `int`. Tidak masalah.

Tapi bagaimana dengan ini?

``` {.c}
int a;
int *p;
```

Bisakah kita jadikan itu satu baris? Bisa. Tapi ke mana `*` pergi?

Aturannya: `*` diletakkan di depan setiap variabel yang bertipe pointer.
Artinya, `*` _bukan_ bagian dari `int` pada contoh ini. Ia bagian dari
variabel `p`.

Dengan begitu, kita bisa menulis ini:

``` {.c}
int a, *p;  // Sama saja
```

Perlu dicatat bahwa baris berikut _tidak_ mendeklarasikan dua pointer:

``` {.c}
int *p, q;  // p adalah pointer ke int; q hanya int biasa.
```

Ini bisa sangat menjebak kalau programmer menulis baris kode berikut
(valid), yang secara fungsional identik dengan yang di atas.

``` {.c}
int* p, q;  // p adalah pointer ke int; q hanya int biasa.
```

Coba perhatikan ini dan tentukan variabel mana yang pointer dan mana
yang bukan:

``` {.c}
int *a, b, c, *d, e, *f, g, h, *i;
```

Jawabannya ada di footnote^[Variabel bertipe pointer adalah `a`, `d`, `f`,
dan `i`, karena hanya merekalah yang punya `*` di depannya.].[i[Pointers-->declarations]>]

## `sizeof` dan Pointer

[i[Pointers-->with `sizeof`]<]Ada sedikit sintaks di sini yang mungkin
membingungkan dan sesekali akan kamu temui.

Ingat bahwa `sizeof` bekerja pada _tipe_ dari ekspresi.

``` {.c}
int *p;

// Mencetak ukuran 'int'
printf("%zu\n", sizeof(int));

// p bertipe 'int *', jadi mencetak ukuran 'int*'
printf("%zu\n", sizeof p);

// *p bertipe 'int', jadi mencetak ukuran 'int'
printf("%zu\n", sizeof *p);
```

Kamu mungkin akan menemui kode dengan `sizeof` terakhir itu. Cukup ingat
bahwa `sizeof` hanya peduli pada tipe ekspresi, bukan variabel di dalam
ekspresi itu sendiri.[i[Pointers-->with `sizeof`]>][i[Pointers]>]
