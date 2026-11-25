<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Input/Output File

[i[File I/O]<]
Kita sudah melihat beberapa contoh I/O dengan `printf()` untuk melakukan
I/O di konsol.

Tapi di bab ini kita akan mendorong konsep-konsep itu sedikit lebih jauh.

## Tipe Data `FILE*`

[i[`FILE*` type]<]
Setiap kali kita melakukan I/O apa pun di C, kita melakukannya lewat
sepotong data yang kita dapat dalam bentuk tipe `FILE*`. `FILE*` ini
menyimpan semua informasi yang dibutuhkan untuk berkomunikasi dengan
subsistem I/O tentang file mana yang kamu buka, di mana posisimu dalam
file, dan sebagainya.

Spesifikasi menyebut semua ini _stream_, yaitu aliran data dari sebuah
file atau dari sumber apa pun. Penulis akan memakai "file" dan "stream"
secara bergantian, tapi sebenarnya kamu sebaiknya menganggap "file"
sebagai kasus khusus dari "stream". Ada cara lain untuk mengalirkan data
ke dalam program selain membaca dari file.

Sebentar lagi kita akan melihat cara mendapatkan `FILE*` yang terbuka dari
sebuah nama file, tapi pertama-tama perlu disebutkan tiga stream yang
sudah terbuka dan siap dipakai.

[i[`stdin` standard input]<]
[i[`stdout` standard output]<]
[i[`stderr` standard error]<]

|Nama `FILE*`|Deskripsi|
|-|-|
|`stdin`|Standard Input, umumnya keyboard secara bawaan|
|`stdout`|Standard Output, umumnya layar secara bawaan|
|`stderr`|Standard Error, umumnya layar juga secara bawaan|

Ternyata kita sebenarnya sudah memakai ketiganya secara implisit. Misalnya,
dua pemanggilan ini sama:

``` {.c}
printf("Hello, world!\n");
fprintf(stdout, "Hello, world!\n");  // printf ke sebuah file
```

Tapi soal itu dibahas nanti.

Kamu juga akan memperhatikan bahwa `stdout` dan `stderr` sama-sama menuju
layar. Sekilas ini tampak seperti kelalaian atau redundansi, tapi
sebenarnya bukan. Sistem operasi pada umumnya memungkinkan kamu
_mengalihkan_ output salah satunya ke file yang berbeda, dan memisahkan
pesan error dari output biasa yang bukan error bisa sangat praktis.

Misalnya, di shell POSIX (seperti sh, ksh, bash, zsh, dan sebagainya)
pada sistem mirip Unix, kita bisa menjalankan program dan mengirim hanya
output non-error (`stdout`) ke satu file, dan semua output error
(`stderr`) ke file lain.

``` {.zsh}
./foo > output.txt 2> errors.txt   # Perintah ini khusus Unix
```

Karena alasan ini, kamu sebaiknya mengirim pesan error yang serius ke
`stderr`, bukan `stdout`.
[i[`stdin` standard input]>]
[i[`stdout` standard output]>]
[i[`stderr` standard error]>]

Cara melakukannya dibahas lebih lanjut nanti.
[i[`FILE*` type]<]

## Membaca File Teks

[i[File I/O-->text files, reading]<]
Stream sebagian besar dikategorikan dalam dua cara: _teks_ dan _biner_.

Stream teks boleh melakukan terjemahan data yang signifikan, terutama
penerjemahan newline ke berbagai representasinya^[Dulu ada tiga jenis
newline yang berlaku luas: Carriage Return (CR, dipakai Mac lama),
Linefeed (LF, dipakai sistem Unix), dan Carriage Return/Linefeed (CRLF,
dipakai sistem Windows). Untungnya kehadiran OS X, yang berbasis Unix,
mengurangi jumlah ini menjadi dua.]. File teks secara logis adalah deretan
_baris_ yang dipisahkan newline. Agar portabel, data inputmu sebaiknya
selalu diakhiri newline.

Tapi aturan umumnya: kalau kamu bisa menyunting file itu di text editor
biasa, ia file teks. Kalau tidak, ia biner. Soal biner dibahas nanti.

Jadi mari kita bekerja---bagaimana cara membuka file untuk dibaca, dan
mengambil data darinya?

Kita buat file bernama `hello.txt` yang isinya hanya ini:

``` {.default}
Hello, world!
```

Lalu kita tulis program untuk membuka file itu, membaca satu karakter
darinya, dan menutup file saat selesai. Itulah rencana permainannya!

[i[`fopen()` function]<]
[i[`fgetc()` function]<]
[i[`fclose()` function]<]
``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;                      // Variabel untuk mewakili file yang terbuka

    fp = fopen("hello.txt", "r");  // Buka file untuk dibaca

    int c = fgetc(fp);             // Baca satu karakter
    printf("%c\n", c);             // Cetak karakter ke stdout

    fclose(fp);                    // Tutup file saat selesai
}
```

Lihat bagaimana saat kita membuka file dengan `fopen()`, ia mengembalikan
`FILE*` kepada kita supaya bisa dipakai nanti.

(Pengecekan ini dihilangkan demi keringkasan, tapi `fopen()` akan
mengembalikan `NULL` kalau ada yang salah, misalnya file tidak ditemukan,
jadi kamu seharusnya benar-benar melakukan pengecekan error!)

Perhatikan juga `"r"` yang kita oper---ini berarti "buka stream teks untuk
dibaca". (Ada berbagai string yang bisa kita oper ke `fopen()` dengan
makna tambahan, seperti menulis, menambahkan, dan sebagainya.)
[i[`fopen()` function]>]

Setelah itu, kita memakai fungsi `fgetc()` untuk mengambil satu karakter
dari stream. Mungkin kamu bertanya-tanya kenapa `c` dibuat `int` dan bukan
`char`---tahan dulu pikiran itu!
[i[`fgetc()` function]>]

Terakhir, kita menutup stream saat selesai memakainya. Semua stream
otomatis ditutup saat program keluar, tapi menutup sendiri setiap file
yang sudah selesai dipakai itu sopan santun dan kebiasaan rumah tangga
yang baik.
[i[`fclose()` function]>]

[i[`FILE*` type]]`FILE*` melacak posisi kita di dalam file. Jadi
pemanggilan `fgetc()` berikutnya akan mengambil karakter berikutnya dalam
file, lalu berikutnya lagi, sampai akhir.

Tapi itu terdengar merepotkan. Mari kita lihat apakah bisa dipermudah.
[i[File I/O-->text files, reading]>]

## Akhir File: `EOF`

[i[`EOF` end of file]<]
Ada karakter khusus yang didefinisikan sebagai makro: `EOF`. Inilah yang
dikembalikan `fgetc()` ketika akhir file tercapai dan kamu mencoba membaca
karakter lagi.

Saatnya berbagi Fakta Menarik™ itu. Ternyata `EOF` adalah alasan kenapa
`fgetc()` dan fungsi sejenisnya mengembalikan `int` dan bukan `char`.
`EOF` bukan karakter yang sebenarnya, dan nilainya kemungkinan berada di
luar rentang `char`. Karena `fgetc()` harus bisa mengembalikan byte apa
pun **dan** `EOF`, ia harus bertipe lebih lebar yang bisa menampung lebih
banyak nilai. Jadi dipilihlah `int`. Tapi kecuali kamu membandingkan
nilai kembaliannya dengan `EOF`, kamu boleh tahu, jauh di dalamnya, ia
adalah `char`.

Baiklah! Kembali ke dunia nyata! Kita bisa memakai ini untuk membaca
seluruh file dalam loop.

[i[`fopen()` function]<]
[i[`fgetc()` function]<]
[i[`fclose()` function]<]
``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    int c;

    fp = fopen("hello.txt", "r");

    while ((c = fgetc(fp)) != EOF)
        printf("%c", c);

    fclose(fp);
}
```
[i[`fopen()` function]>]
[i[`fclose()` function]>]

(Kalau baris 10 terlalu aneh, uraikan saja mulai dari tanda kurung yang
paling dalam. Hal pertama yang dilakukan adalah menugaskan hasil
`fgetc()` ke `c`, dan _baru kemudian_ membandingkan _nilai itu_ dengan
`EOF`. Kita hanya menjejalkannya dalam satu baris. Mungkin kelihatan
sulit dibaca, tapi pelajari---ini idiomatis di C.)
[i[`fgetc()` function]>]

Dan saat dijalankan, kita lihat:

``` {.default}
Hello, world!
```

Tapi tetap saja kita bekerja satu karakter demi satu karakter, padahal
banyak file teks lebih masuk akal dibaca per baris. Mari beralih ke itu.
[i[`EOF` end of file]>]

### Membaca Satu Baris Sekaligus

[i[File I/O-->line by line]<]
Jadi bagaimana mendapat satu baris utuh sekaligus? [i[`fgets()`
function]<]`fgets()` datang menyelamatkan! Sebagai argumen, ia menerima
pointer ke buffer `char` untuk menampung byte, jumlah byte maksimum yang
dibaca, dan `FILE*` tempat membaca. Ia mengembalikan `NULL` saat
end-of-file atau error. `fgets()` bahkan cukup baik hati untuk mengakhiri
string dengan NUL saat selesai^[Kalau buffer tidak cukup besar untuk
membaca satu baris penuh, ia akan berhenti membaca di tengah baris, dan
pemanggilan `fgets()` berikutnya akan melanjutkan sisa barisnya.].[i[`fgets()` function]>]

Kita buat loop yang mirip dengan tadi, hanya saja dengan file multibaris
yang dibaca satu baris sekaligus.

Berikut file `quote.txt`:

``` {.default}
A wise man can learn more from
a foolish question than a fool
can learn from a wise answer.
                  --Bruce Lee
```

Dan berikut kode yang membaca file itu satu baris sekaligus dan mencetak
nomor baris sebelum masing-masing baris:

[i[`fgets()` function]<]
``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    char s[1024];  // Cukup besar untuk baris apa pun yang ditemui program ini
    int linecount = 0;

    fp = fopen("quote.txt", "r");

    while (fgets(s, sizeof s, fp) != NULL) 
        printf("%d: %s", ++linecount, s);

    fclose(fp);
}
```
[i[`fgets()` function]>]

Yang memberi output:

``` {.default}
1: A wise man can learn more from
2: a foolish question than a fool
3: can learn from a wise answer.
4:                   --Bruce Lee
```
[i[File I/O-->line by line]>]

## Input Terformat

[i[File I/O-->formatted input]<]
Kamu tahu bagaimana kamu bisa mendapat output terformat dengan `printf()`
(dan, dengan demikian, `fprintf()` seperti yang akan kita lihat di
bawah)?

[i[`fscanf()` function]<]
Hal yang sama bisa dilakukan dengan `fscanf()`.

> Sebelum mulai, perlu diingatkan bahwa memakai fungsi gaya `scanf()` bisa
> berbahaya untuk input yang tidak tepercaya. Kalau kamu tidak menentukan
> lebar field pada `%s`, buffer bisa meluap. Lebih buruk lagi, konversi
> numerik yang tidak valid menghasilkan undefined behavior. Cara aman untuk
> input yang tidak tepercaya adalah memakai `%s` dengan lebar field, lalu
> memakai fungsi seperti [i[`strtol` function]] `strtol()` atau
> [i[`strtod` function]] `strtod()` untuk melakukan konversinya.

Kita siapkan file berisi serangkaian record data. Dalam hal ini, paus,
dengan nama, panjang dalam meter, dan bobot dalam ton. `whales.txt`:

``` {.default}
blue 29.9 173
right 20.7 135
gray 14.9 41
humpback 16.0 30
```

Ya, kita bisa membacanya dengan [i[`fgets()` function]]`fgets()` lalu
mem-parse string-nya dengan `sscanf()` (dan itu lebih tahan terhadap file
yang rusak), tapi dalam kasus ini, kita pakai saja `fscanf()` dan
mengambilnya langsung.

Fungsi `fscanf()` melewati whitespace di depan saat membaca, dan
mengembalikan `EOF` saat end-of-file atau error.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    char name[1024];  // Cukup besar untuk baris apa pun yang ditemui program ini
    float length;
    int mass;

    fp = fopen("whales.txt", "r");

    while (fscanf(fp, "%s %f %d", name, &length, &mass) != EOF)
        printf("%s whale, %d tonnes, %.1f meters\n", name, mass, length);

    fclose(fp);
}
```
[i[`fscanf()` function]>]

Yang memberi hasil:

``` {.default}
blue whale, 173 tonnes, 29.9 meters
right whale, 135 tonnes, 20.7 meters
gray whale, 41 tonnes, 14.9 meters
humpback whale, 30 tonnes, 16.0 meters
```
[i[File I/O-->formatted input]>]

## Menulis File Teks

[i[File I/O-->text files, writing]<]
[i[`fputc()` function]<]
[i[`fputs()` function]<]
[i[`fprintf()` function]<]
Sama seperti kita bisa memakai `fgetc()`, `fgets()`, dan `fscanf()` untuk
membaca stream teks, kita bisa memakai `fputc()`, `fputs()`, dan
`fprintf()` untuk menulis stream teks.

Untuk itu, kita harus meng-`fopen()` file dalam mode tulis dengan
mengoper `"w"` sebagai argumen kedua. Membuka file yang sudah ada dalam
mode `"w"` akan langsung memotong file itu menjadi 0 byte untuk ditimpa
sepenuhnya.

Kita susun program sederhana yang mengeluarkan file `output.txt` dengan
berbagai fungsi output.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    int x = 32;

    fp = fopen("output.txt", "w");

    fputc('B', fp);
    fputc('\n', fp);   // newline
    fprintf(fp, "x = %d\n", x);
    fputs("Hello, world!\n", fp);

    fclose(fp);
}
```
[i[`fputc()` function]>]
[i[`fputs()` function]>]
[i[`fprintf()` function]>]

Dan ini menghasilkan file `output.txt` dengan isi:

``` {.default}
B
x = 32
Hello, world!
```

Fakta menarik: karena `stdout` adalah sebuah file, kamu bisa mengganti
baris 8 dengan:

``` {.c}
fp = stdout;
```

dan program akan mengeluarkan output ke konsol alih-alih ke file. Coba
saja!
[i[File I/O-->text files, writing]>]

## I/O File Biner

[i[File I/O-->binary files]<]
Sejauh ini kita hanya membahas file teks. Tapi ada makhluk lain yang
sudah disebut di awal, yaitu file _biner_, atau stream biner.

Cara kerjanya sangat mirip file teks, kecuali subsistem I/O tidak
melakukan terjemahan apa pun pada data seperti yang mungkin dilakukannya
pada file teks. Pada file biner, kamu mendapat aliran byte mentah, dan
hanya itu.

Perbedaan besar saat membuka file: kamu harus menambahkan `"b"` ke mode.
Artinya, untuk membaca file biner, buka dalam mode `"rb"`. Untuk menulis
file, buka dalam mode `"wb"`.

Karena ini aliran byte, dan aliran byte bisa berisi karakter NUL, dan
karakter NUL adalah penanda akhir string di C, jarang orang memakai
fungsi `fprintf()` dan kawan-kawannya untuk mengolah file biner.

[i[`fwrite()` function]<]
Sebagai gantinya, fungsi yang paling umum adalah [i[`fread()`
function]]`fread()` dan `fwrite()`. Fungsi-fungsi ini membaca dan menulis
sejumlah byte tertentu ke stream.

Untuk demo, kita tulis dua program. Yang satu menulis serangkaian nilai
byte ke disk sekaligus. Dan program kedua membaca satu byte per satu dan
mencetaknya^[Biasanya program kedua akan membaca semua byte sekaligus, lalu
_baru kemudian_ mencetaknya dalam loop. Itu lebih efisien. Tapi di sini
kita mengejar nilai demonstrasi.].

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    unsigned char bytes[6] = {5, 37, 0, 88, 255, 12};

    fp = fopen("output.bin", "wb");  // mode wb untuk "write binary"!

    // Pada pemanggilan fwrite, argumennya adalah:
    //
    // * Pointer ke data yang akan ditulis
    // * Ukuran setiap "potongan" data
    // * Jumlah "potongan" data
    // * FILE*

    fwrite(bytes, sizeof(char), 6, fp);

    fclose(fp);
}
```
[i[`fwrite()` function]>]

Dua argumen tengah `fwrite()` itu agak aneh. Tapi pada dasarnya yang ingin
kita sampaikan kepada fungsi itu: "Kita punya item berukuran _sebesar ini_,
dan kita ingin menulis _sebanyak itu_." Ini praktis kalau kamu punya
record dengan panjang tetap, dan banyak record dalam sebuah array. Kamu
cukup memberi tahu ukuran satu record dan berapa yang ditulis. 

Pada contoh di atas, kita memberi tahu bahwa setiap record berukuran
sebesar `char`, dan kita punya 6 buah.

Menjalankan program ini menghasilkan file `output.bin`, tapi membukanya di
text editor tidak menampilkan apa pun yang ramah! Itu data biner---bukan
teks. Dan data biner acak yang baru saja dikarang, pula!

Kalau kita jalankan lewat program [flw[hex dump|Hex_dump]], kita bisa
melihat outputnya sebagai byte:

``` {.default}
05 25 00 58 ff 0c
```

> Banyak sistem Unix menyertakan program bernama `hexdump` untuk ini.
> Kamu bisa memakainya seperti ini dengan switch `-C` ("canonical") untuk
> mendapat output yang rapi:
>
> ``` {.default}
> $ hexdump -C output.bin
> 00000000  05 25 00 58 ff 0c                              |.%.X..|
> ```
>
> <!-- ` -->
>
> `00000000` adalah offset dalam file tempat baris output ini dimulai.
> `05 25 00 58 ff 0c` adalah nilai-nilai byte (dan akan lebih panjang
> (sampai 16 byte per baris) kalau file berisi lebih banyak byte). Dan di
> sebelah kanan, di antara simbol pipa (`|`), adalah upaya terbaik
> `hexdump` untuk mencetak karakter yang sesuai dengan byte-byte itu. Ia
> mencetak titik kalau karakternya tidak bisa dicetak. Dalam kasus ini,
> karena kita hanya mencetak data biner acak, bagian output ini hanya
> sampah. Tapi kalau kita menulis string ASCII ke file, kita akan
> melihatnya di sana.

Dan nilai-nilai hex itu memang cocok dengan nilai-nilai (dalam desimal)
yang kita tulis.

Tapi sekarang mari kita coba membacanya kembali dengan program lain. Yang
ini akan membuka file untuk pembacaan biner (mode `"rb"`) dan membaca
byte satu per satu dalam loop.

[i[`fread()` function]<]
`fread()` punya fitur menarik: ia mengembalikan jumlah byte yang dibaca,
atau `0` saat EOF. Jadi kita bisa me-loop sampai melihat itu, sambil
mencetak angka.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    FILE *fp;
    unsigned char c;

    fp = fopen("output.bin", "rb"); // rb untuk "read binary"!

    while (fread(&c, sizeof(char), 1, fp) > 0)
        printf("%d\n", c);

    fclose(fp);
}
```
[i[`fread()` function]>]

Dan saat dijalankan, kita melihat angka-angka asli kita!

``` {.default}
5
37
0
88
255
12
```

Horee!
[i[File I/O-->binary files]>]

### Peringatan tentang `struct` dan Angka

[i[File I/O-->with `struct`s]<]
Seperti yang kita lihat di bagian `struct`, compiler bebas menambahkan
padding pada `struct` sesuai kehendaknya. Dan compiler yang berbeda
mungkin melakukannya secara berbeda. Dan compiler yang sama pada arsitektur
berbeda bisa melakukannya secara berbeda. Dan compiler yang sama pada
arsitektur yang sama pun bisa melakukannya secara berbeda.

Intinya: tidak portabel kalau langsung `fwrite()` seluruh `struct` ke
file ketika kamu tidak tahu di mana padding akan berakhir.
[i[File I/O-->with `struct`s]>]

Bagaimana cara memperbaikinya? Tahan dulu pikiran itu---kita akan melihat
beberapa caranya setelah membahas masalah lain yang terkait.

[i[File I/O-->with numeric values]<]
Angka!

Ternyata tidak semua arsitektur merepresentasikan angka di memori dengan
cara yang sama.

Mari kita lihat `fwrite()` sederhana untuk angka 2-byte. Kita tulis dalam
hex supaya setiap byte jelas. Byte paling signifikan bernilai `0x12` dan
yang paling tidak signifikan bernilai `0x34`.

``` {.c}
unsigned short v = 0x1234;  // Dua byte, 0x12 dan 0x34

fwrite(&v, sizeof v, 1, fp);
```

Apa yang berakhir di stream?

Kelihatannya seharusnya `0x12` diikuti `0x34`, kan?

Tapi kalau ini dijalankan di mesin penulis dan hasilnya di-hex dump,
hasilnya:

``` {.default}
34 12
```

Terbalik! Kenapa?

Ini ada hubungannya dengan yang disebut
[i[Endianess]][flw[_endianess_|Endianess]] arsitektur. Sebagian menulis
byte paling signifikan lebih dulu, dan sebagian menulis byte paling tidak
signifikan lebih dulu.

Artinya, kalau kamu menulis angka multibyte langsung dari memori, kamu
tidak bisa melakukannya secara portabel^[Dan inilah alasan penulis
dengan cerdik memakai byte individual pada contoh `fwrite()` dan
`fread()` di atas.].

Masalah serupa ada pada floating point. Kebanyakan sistem memakai format
yang sama untuk bilangan floating point, tapi sebagian tidak. Tidak ada
jaminan!

[i[File I/O-->with `struct`s]<]
Jadi... bagaimana kita memperbaiki semua masalah angka dan `struct` ini
supaya data kita tertulis secara portabel?

Ringkasnya adalah dengan men-[i[Data serialization]]_serialize_ data, yaitu
istilah umum yang berarti mengambil semua data dan menuliskannya dalam
format yang kamu kendalikan, yang terkenal, dan bisa diprogram agar
bekerja sama di semua platform.

Seperti yang mungkin kamu duga, ini masalah yang sudah terpecahkan. Ada
banyak library serialisasi yang bisa kamu manfaatkan, seperti
[flw[_protocol buffers_|Protocol_buffers]] dari Google, yang sudah tersedia
dan siap dipakai. Mereka akan mengurus semua detail rumitnya, bahkan
memungkinkan data dari programmu di C berinteroperasi dengan bahasa lain
yang mendukung metode serialisasi yang sama.

Lakukan kebaikan untuk dirimu dan semua orang! Serialize data binermu saat
menulisnya ke stream! Ini akan menjaga semuanya tetap rapi dan portabel,
bahkan kalau kamu memindahkan file data dari satu arsitektur ke
arsitektur lain.
[i[File I/O-->with `struct`s]>]
[i[File I/O-->with numeric values]>]
[i[File I/O]>]
