<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Lingkungan di Luar Program

Ketika kamu menjalankan sebuah program, sebenarnya kamu sedang berbicara
dengan shell, berkata, "Hei, tolong jalankan ini." Dan shell menjawab,
"Siap," lalu memberi tahu sistem operasi, "Hei, bisa tolong buatkan
proses baru dan jalankan ini?" Dan kalau semuanya lancar, OS menurut dan
programmu berjalan.

Tapi ada dunia luas di luar programmu, di dalam shell, yang bisa
berinteraksi dengan C. Beberapa di antaranya kita lihat di bab ini.

## Argumen Command Line

[i[Command line arguments]<]

Banyak utilitas command line menerima _argumen command line_. Misalnya,
kalau kita ingin melihat semua file berakhiran `.txt`, kita bisa mengetik
sesuatu seperti ini di sistem mirip Unix:

``` {.zsh}
ls *.txt
```

(atau `dir` alih-alih `ls` di sistem Windows).

Dalam kasus ini, perintahnya adalah `ls`, sedangkan argumennya adalah
semua file yang berakhiran `.txt`^[Secara historis, program MS-DOS dan
Windows melakukan ini secara berbeda dari Unix. Di Unix, shell akan
_memperluas_ wildcard menjadi semua file yang cocok sebelum programmu
melihatnya, sedangkan varian Microsoft mengoper ekspresi wildcard itu ke
program untuk ditangani sendiri. Bagaimanapun juga, ada argumen yang
dioper ke program.].

Jadi bagaimana kita bisa melihat apa yang dioper ke program dari command
line?

Katakanlah kita punya program bernama `add` yang menjumlahkan semua angka
yang dioper di command line dan mencetak hasilnya:

``` {.zsh}
./add 10 30 5
45
```

Dijamin ini bisa membayar tagihan!

Tapi serius, ini alat yang bagus untuk melihat cara mengambil argumen
dari command line dan memecahnya.

Pertama, mari kita lihat dulu cara mendapatkannya. Untuk itu, kita
butuh `main()` yang baru!

Berikut program yang mencetak semua argumen command line. Misalnya, kalau
executable-nya kita beri nama `foo`, kita bisa menjalankannya seperti
ini:

``` {.zsh}
./foo i like turtles
```

dan kita akan melihat output ini:

``` {.default}
arg 0: ./foo
arg 1: i
arg 2: like
arg 3: turtles
```

Agak aneh, karena argumen ke-nol adalah nama executable itu sendiri. Tapi
itu hanya soal membiasakan diri. Argumen sebenarnya menyusul langsung
setelahnya.

Sumber:

[i[`argc` parameter]<]
[i[`argv` parameter]<]
[i[`main()` function-->command line options]<]

``` {.c .numberLines}
#include <stdio.h>

int main(int argc, char *argv[])
{
    for (int i = 0; i < argc; i++) {
        printf("arg %d: %s\n", i, argv[i]);
    }
}
```

Wah! Ada apa dengan signature fungsi `main()` itu? Apa itu `argc` dan
`argv`^[Karena keduanya hanya nama parameter biasa, kamu sebenarnya tidak
harus menamainya `argc` dan `argv`. Tapi nama-nama itu begitu idiomatis,
sehingga kalau kamu kreatif, programmer C lain pasti akan menatapmu dengan
curiga!] (dibaca _arg-cee_ dan _arg-vee_)?

Mulai dari yang mudah: `argc`. Ia singkatan dari _argument count_ (jumlah
argumen), termasuk nama program itu sendiri. Kalau kamu menganggap semua
argumen sebagai array berisi string, yang memang begitulah adanya, maka
`argc` bisa dianggap sebagai panjang array itu, yang memang begitulah
adanya.

Jadi di loop itu kita menelusuri semua `argv` dan mencetaknya satu per
satu, jadi untuk input tertentu:

``` {.zsh}
./foo i like turtles
```

kita mendapat output yang sesuai:

``` {.default}
arg 0: ./foo
arg 1: i
arg 2: like
arg 3: turtles
```

Dengan itu, kita siap membuat program penjumlah.

Rencana kita:

* Lihat semua argumen command line (setelah `argv[0]`, nama program)
* Konversi menjadi integer
* Tambahkan ke total yang berjalan
* Cetak hasilnya

Mari kita kerjakan!

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
    int total = 0;

    for (int i = 1; i < argc; i++) {  // Mulai dari 1, argumen pertama
        int value = atoi(argv[i]);    // strtol() penanganan error-nya lebih baik

        total += value;
    }

    printf("%d\n", total);
}
```

[i[`main()` function-->command line options]>]
[i[`argc` parameter]>]

Contoh eksekusi:

``` {.zsh}
$ ./add
0
$ ./add 1
1
$ ./add 1 2
3
$ ./add 1 2 3
6
$ ./add 1 2 3 4
10
```

Tentu saja, program ini mungkin muntah kalau yang dioper bukan integer,
tapi membuatnya tahan terhadap hal itu kita serahkan sebagai latihan untuk
pembaca.

### `argv` Terakhir adalah `NULL`

Satu trivia menyenangkan tentang `argv`: setelah string terakhir ada
pointer ke `NULL`.

Artinya:

``` {.c}
argv[argc] == NULL
```

selalu bernilai true!

Ini mungkin tampak tidak berguna, tapi ternyata berguna di beberapa
tempat; salah satunya kita lihat sekarang.

### Alternatifnya: `char **argv`

Ingat bahwa saat memanggil fungsi, C tidak membedakan notasi array dan
notasi pointer pada signature fungsi.

Artinya, keduanya sama:

``` {.c}
void foo(char a[])
void foo(char *a)
```

Nah, selama ini kita nyaman menganggap `argv` sebagai array berisi
string, yaitu array berisi `char*`, jadi ini masuk akal:

``` {.c}
int main(int argc, char *argv[])
```

tapi karena kesetaraan itu, kamu juga bisa menulis:

``` {.c}
int main(int argc, char **argv)
```

Ya, itu memang pointer ke pointer! Kalau lebih mudah, anggap saja sebagai
pointer ke string. Tapi sebenarnya ia adalah pointer ke nilai yang
menunjuk ke `char`.

Ingat juga bahwa dua hal ini setara:

``` {.c}
argv[i]
*(argv + i)
```

yang berarti kamu bisa melakukan aritmetika pointer pada `argv`.

Jadi cara alternatif untuk memproses argumen command line adalah dengan
menelusuri array `argv` sambil menaikkan pointer sampai bertemu `NULL` di
ujung.

Mari kita ubah penjumlah kita agar bekerja seperti itu:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
    int total = 0;
    
    // Trik manis supaya compiler berhenti memperingatkan soal variabel
    // argc yang tidak terpakai:
    (void)argc;

    for (char **p = argv + 1; *p != NULL; p++) {
        int value = atoi(*p);  // Pakai strtol() untuk penanganan error yang lebih baik

        total += value;
    }

    printf("%d\n", total);
}
```

Aku sendiri memakai notasi array untuk mengakses `argv`, tapi gaya
ini juga sering kulihat beredar.

### Fakta Menarik

[i[`argc` parameter]<]

Beberapa hal lagi tentang `argc` dan `argv`.

* Beberapa lingkungan mungkin tidak mengisi `argv[0]` dengan nama program.
  Kalau tidak tersedia, `argv[0]` akan berupa string kosong. Aku
  belum pernah melihat ini terjadi.

* Spesifikasi cukup longgar soal apa yang boleh dilakukan implementasi
  terhadap `argv` dan dari mana nilai-nilai itu berasal. Tapi setiap sistem
  yang pernah kupakai bekerja dengan cara yang sama, seperti yang
  kita bahas di bagian ini.

* Kamu bisa mengubah `argc`, `argv`, atau string mana pun yang ditunjuk
  `argv`. (Hanya saja jangan membuat string-string itu lebih panjang dari
  sebelumnya!)

* Di beberapa sistem mirip Unix, mengubah string `argv[0]` membuat output
  `ps` ikut berubah^[`ps`, Process Status, adalah perintah Unix untuk
  melihat proses apa saja yang sedang berjalan.].

  Biasanya, kalau kamu punya program bernama `foo` yang dijalankan dengan
  `./foo`, kamu mungkin melihat ini pada output `ps`:

  ``` {.default}
   4078 tty1     S      0:00 ./foo
  ```

  Tapi kalau kamu mengubah `argv[0]` seperti ini, dengan hati-hati agar
  string baru `"Hi!  "` panjangnya sama dengan yang lama `"./foo"`:

  ``` {.c}
  strcpy(argv[0], "Hi!  ");
  ```

  lalu menjalankan `ps` selagi program `./foo` masih berjalan, kita akan
  melihat ini sebagai gantinya:
  
  ``` {.default}
   4079 tty1     S      0:00 Hi!  
  ```

  Perilaku ini tidak ada di spesifikasi dan sangat bergantung pada sistem.

[i[`argc` parameter]>]
[i[`argv` parameter]>]
[i[Command line arguments]>]

## Exit Status {#exit-status}

[i[Exit status]<]

Pernahkah kamu perhatikan bahwa signature fungsi `main()` mengembalikan
tipe `int`? Untuk apa itu? Ini berkaitan dengan yang disebut _exit
status_, yaitu integer yang bisa dikembalikan ke program yang
menjalankan programmu untuk memberi tahu bagaimana hasilnya.

Ada beberapa cara program keluar di C, termasuk `return` dari `main()`,
atau memanggil salah satu varian `exit()`.

Semua cara itu menerima sebuah `int` sebagai argumen.

[i[`main()` function-->returning from]<]
Catatan sampingan: sudah lihat belum bahwa di hampir semua contohku,
meskipun `main()` seharusnya mengembalikan `int`, aku tidak menulis
`return` apa pun? Di fungsi lain, ini ilegal, tapi di C ada kasus khusus:
kalau eksekusi mencapai akhir `main()` tanpa menemukan `return`, ia
otomatis melakukan `return 0`.
[i[`main()` function-->returning from]>]

Tapi apa arti `0` itu? Angka apa lagi yang bisa kita taruh di sana? Dan
bagaimana angka-angka itu dipakai?

Spesifikasi jelas sekaligus samar soal ini, seperti biasa. Jelas karena ia
merinci apa yang bisa dilakukan, tapi samar karena tidak membatasinya
secara khusus.

Tidak ada pilihan lain selain _maju terus_ dan mencari tahu!

Mari kita ber-[flw[Inception|Inception]] sebentar: ternyata ketika
kamu menjalankan programmu, _kamu menjalankannya dari program lain_.

Biasanya program lain ini semacam [flw[shell|Shell_(computing)]] yang
tidak banyak melakukan apa-apa sendiri kecuali menjalankan program lain.

Tapi ini proses multitahap, terutama terlihat pada shell command-line:

1. Shell menjalankan programmu
2. Shell biasanya tidur (untuk shell command-line)
3. Programmu berjalan
4. Programmu berhenti
5. Shell bangun dan menunggu perintah berikutnya

Nah, ada sedikit komunikasi yang terjadi antara langkah 4 dan 5: program
bisa mengembalikan sebuah _status value_ yang bisa ditanyakan oleh shell.
Biasanya, nilai ini dipakai untuk menandakan sukses atau gagalnya
programmu, dan, kalau gagal, jenis kegagalannya.

Nilai inilah yang selama ini kita `return` dari `main()`. Itulah
statusnya.

Spesifikasi C mengizinkan dua nilai status berbeda, yang nama makronya
didefinisikan di `<stdlib.h>`:

[i[`EXIT_SUCCESS` macro]<]
[i[`EXIT_FAILURE` macro]<]

|Status|Deskripsi|
|-|-|
|`EXIT_SUCCESS` atau `0`|Program berakhir dengan sukses.|
|`EXIT_FAILURE`|Program berakhir dengan error.|

Mari kita tulis program singkat yang mengalikan dua angka dari command
line. Kamu wajib menyebutkan tepat dua nilai. Kalau tidak, kita cetak
pesan error, dan keluar dengan status error.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
    if (argc != 3) {
        printf("usage: mult x y\n");
        return EXIT_FAILURE;   // Beri tahu shell bahwa ini tidak berhasil
    }

    printf("%d\n", atoi(argv[1]) * atoi(argv[2]));

    return 0;  // sama dengan EXIT_SUCCESS, semuanya baik-baik saja.
}
```

[i[`EXIT_SUCCESS` macro]>]
[i[`EXIT_FAILURE` macro]>]

Kalau kita coba menjalankannya, kita mendapat efek yang diharapkan sampai
kita menyebutkan jumlah argumen command-line yang tepat:

``` {.zsh}
$ ./mult
usage: mult x y

$ ./mult 3 4 5
usage: mult x y

$ ./mult 3 4
12
```

[i[Exit status-->obtaining from shell]<]

Tapi itu belum benar-benar menunjukkan exit status yang kita kembalikan,
kan? Kita bisa menyuruh shell mencetaknya. Dengan asumsi kamu memakai Bash
atau shell POSIX lainnya, kamu bisa memakai `echo $?` untuk melihatnya^[Di
Windows `cmd.exe`, ketik `echo %errorlevel%`. Di PowerShell, ketik
`$LastExitCode`.].

Kita coba:

``` {.zsh}
$ ./mult
usage: mult x y
$ echo $?
1

$ ./mult 3 4 5
usage: mult x y
$ echo $?
1

$ ./mult 3 4
12
$ echo $?
0
```

[i[Exit status-->obtaining from shell]>]

Menarik! Kita lihat bahwa di sistemku, [i[`EXIT_FAILURE`
macro]]`EXIT_FAILURE` adalah `1`. Spesifikasi tidak menyebutkannya, jadi
bisa angka apa saja. Tapi coba saja; kemungkinan di sistemmu juga `1`.

### Nilai Exit Status Lainnya

Status `0` jelas berarti sukses, tapi bagaimana dengan semua integer lain,
bahkan yang negatif?

Di sini kita keluar dari spesifikasi C dan masuk ke wilayah Unix. Secara
umum, `0` berarti sukses, sedangkan angka positif bukan nol berarti gagal.
Jadi hanya ada satu jenis sukses, dan banyak jenis gagal. Bash
menyatakan exit code harus antara 0 dan 255, meski sejumlah kode
dicadangkan. 

Singkatnya, kalau kamu ingin menandakan exit status error yang berbeda di
lingkungan Unix, kamu bisa mulai dari `1` dan naik terus.

Di Linux, kalau kamu mencoba kode di luar rentang 0-255, kodenya akan
di-AND secara bitwise dengan `0xff`, yang secara efektif menjepitnya ke
rentang itu.

Kamu bisa membuat script shell yang memakai kode status ini untuk
memutuskan apa yang dilakukan berikutnya.

[i[Exit status]>]

## Environment Variable {#env-var}

[i[Environment variables]<]

Sebelum masuk ke bagian ini, aku harus memperingatkan bahwa C tidak
menentukan apa itu environment variable. Jadi aku akan menjelaskan sistem
environment variable yang berfungsi di semua platform besar yang kuketahui.

Pada dasarnya, environment adalah program yang akan menjalankan programmu,
misalnya shell bash. Dan ia mungkin punya beberapa variabel bash yang
didefinisikan. Kalau kamu belum tahu, shell bisa membuat variabelnya
sendiri. Setiap shell berbeda, tapi di bash kamu bisa mengetik `set` dan
ia akan menampilkan semuanya.

Berikut potongan dari 61 variabel yang didefinisikan di bash shell-ku:

``` {.default}
HISTFILE=/home/beej/.bash_history
HISTFILESIZE=500
HISTSIZE=500
HOME=/home/beej
HOSTNAME=FBILAPTOP
HOSTTYPE=x86_64
IFS=$' \t\n'
```

Perhatikan bentuknya berupa pasangan key/value. Misalnya, salah satu key-nya
adalah `HOSTTYPE` dan value-nya `x86_64`. Dari sudut pandang C, semua value
adalah string, bahkan kalau berupa angka^[Kalau kamu butuh nilai numerik,
konversikan string itu dengan sesuatu seperti `atoi()` atau `strtol()`.].

Jadi, _anyway_! Singkat cerita, nilai-nilai ini bisa diambil dari dalam
programmu.

[i[`getenv()` function]<]

Mari kita tulis program yang memakai fungsi standar `getenv()` untuk
mencari nilai yang kamu set di shell.

`getenv()` mengembalikan pointer ke string value, atau `NULL` kalau
environment variable-nya tidak ada.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *val = getenv("FROTZ");  // Coba ambil nilainya

    // Pastikan nilainya ada
    if (val == NULL) {
        printf("Cannot find the FROTZ environment variable\n");
        return EXIT_FAILURE;
    }

    printf("Value: %s\n", val);
}
```

[i[`getenv()` function]>]

Kalau kujalankan langsung, hasilnya begini:

``` {.zsh}
$ ./foo
Cannot find the FROTZ environment variable
```

yang masuk akal, karena belum kuset.

Di bash, aku bisa mengisinya dengan^[Di Windows CMD.EXE, pakai `set
FROTZ=value`. Di PowerShell, pakai `$Env:FROTZ=value`.]:

``` {.zsh}
$ export FROTZ="C is awesome!"
```

Lalu kalau kujalankan, hasilnya:

``` {.zsh}
$ ./foo
Value: C is awesome!
```

Dengan cara ini, kamu bisa menyiapkan data di environment variable, lalu
mengambilnya di kode C-mu dan menyesuaikan perilaku program.

### Mengatur Environment Variable

Ini bukan standar, tapi banyak sistem menyediakan cara untuk mengatur
environment variable.

Kalau di sistem mirip Unix, lihat dokumentasi [i[`putenv()`
function]]`putenv()`, [i[`setenv()`]]`setenv()`, dan [i[`unsetenv()`
function]]`unsetenv()`. Di Windows, lihat [i[`_putenv()`
function]]`_putenv()`.

### Environment Variable Alternatif di Sistem Mirip Unix

Kalau kamu di sistem mirip Unix, kemungkinan kamu punya beberapa cara lain
untuk mengakses environment variable. Perhatikan bahwa walaupun spesifikasi
menunjukkannya sebagai ekstensi umum, ini bukan benar-benar bagian dari
standar C. Meski demikian, ini bagian dari standar POSIX.

[i[`environ` variable]<]

Salah satunya adalah variabel bernama `environ` yang harus dideklarasikan
seperti ini:

``` {.c}
extern char **environ;
```

Ia adalah array berisi string yang diakhiri pointer `NULL`.

Kamu harus mendeklarasikannya sendiri sebelum memakainya, atau mungkin
kamu menemukannya di header file non-standar `<unistd.h>`.

Setiap string berbentuk `"key=value"` sehingga kamu harus memecah dan
mem-parse-nya sendiri kalau ingin mengambil key dan value-nya.

Berikut contoh menelusuri dan mencetak environment variable dengan
beberapa cara berbeda:

``` {.c .numberLines}
#include <stdio.h>

extern char **environ;  // HARUS extern DAN bernama "environ"

int main(void)
{
    for (char **p = environ; *p != NULL; p++) {
        printf("%s\n", *p);
    }

    // Atau bisa juga begini:
    for (int i = 0; environ[i] != NULL; i++) {
        printf("%s\n", environ[i]);
    }
}
```

Yang menghasilkan banyak output seperti ini:

``` {.default}
SHELL=/bin/bash
COLORTERM=truecolor
TERM_PROGRAM_VERSION=1.53.2
LOGNAME=beej
HOME=/home/beej
... etc ...
```

Pakai `getenv()` kalau memungkinkan karena lebih portabel. Tapi kalau
harus menelusuri environment variable, memakai `environ` mungkin pilihan
yang tepat.

[i[`environ` variable]>]
[i[`env` parameter]<]

Cara non-standar lain untuk mendapat environment variable adalah sebagai
parameter `main()`. Cara kerjanya hampir sama, tapi kamu tidak perlu
menambahkan variabel `extern` `environ`. [fl[Bahkan spesifikasi POSIX pun
tidak mendukung
ini|https://pubs.opengroup.org/onlinepubs/9699919799/functions/exec.html]]
sejauh yang kutahu, tapi ini umum di dunia Unix.


``` {.c .numberLines}
#include <stdio.h>

int main(int argc, char **argv, char **env)  // <-- env!
{
    (void)argc; (void)argv;  // Tekan peringatan variabel tak terpakai

    for (char **p = env; *p != NULL; p++) {
        printf("%s\n", *p);
    }

    // Atau bisa juga begini:
    for (int i = 0; env[i] != NULL; i++) {
        printf("%s\n", env[i]);
    }
}
```

Persis seperti memakai `environ` tapi _lebih tidak portabel lagi_. Bagus
punya target.

[i[`env` parameter]>]
[i[Environment variables]>]
