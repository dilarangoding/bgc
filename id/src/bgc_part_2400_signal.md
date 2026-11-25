<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Penanganan Signal

[i[Signal handling]<]

Sebelum mulai, aku hanya ingin menyarankan agar kamu pada umumnya mengabaikan
seluruh bab ini dan memakai fungsi penanganan signal bawaan OS-mu yang
(sangat mungkin) lebih unggul. Sistem mirip Unix punya keluarga fungsi
[i[`sigaction()` function]] `sigaction()`, dan Windows punya... entah apa
yang dilakukannya^[Tampaknya jauh di dalamnya Windows sama sekali tidak
mengenal signal ala Unix, dan untuk aplikasi konsol signal itu
disimulasikan.].

Setelah itu beres, apa itu signal?

## Apa Itu Signal?

Sebuah _signal_ di-_raise_ pada berbagai kejadian eksternal. Programmu bisa
diatur agar diinterupsi untuk _menangani_ signal itu, dan, kalau mau,
melanjutkan dari tempat ia berhenti setelah signal selesai ditangani.

Bayangkan seperti fungsi yang otomatis dipanggil ketika salah satu
kejadian eksternal ini terjadi.

Apa saja kejadian itu? Di sistemmu kemungkinan ada banyak sekali, tapi di
spesifikasi C hanya ada beberapa:

[i[`SIGABRT` signal]]
[i[`SIGFPE` signal]]
[i[`SIGILL` signal]]
[i[`SIGINT` signal]]
[i[`SIGSEGV` signal]]
[i[`SIGTERM` signal]]

|Signal|Deskripsi|
|-------|--------------------------------------------------------|
|`SIGABRT`|Terminasi abnormal---yang terjadi ketika `abort()` dipanggil.|
|`SIGFPE`|Exception floating point.|
|`SIGILL`|Instruksi ilegal.|
|`SIGINT`|Interupsi---biasanya akibat `CTRL-C` ditekan.|
|`SIGSEGV`|"Segmentation Violation": akses memori yang tidak valid.|
|`SIGTERM`|Permintaan terminasi.|

Kamu bisa mengatur programmu untuk mengabaikan, menangani, atau membiarkan
aksi bawaan untuk masing-masing signal itu dengan fungsi `signal()`.

## Menangani Signal dengan `signal()`

[i[`signal()` function]<]

Pemanggilan `signal()` menerima dua parameter: signal yang dimaksud, dan
aksi yang diambil ketika signal itu di-raise.

Aksinya bisa berupa salah satu dari tiga hal:

* Pointer ke fungsi handler.
* [i[`SIG_IGN` macro]<]`SIG_IGN` untuk mengabaikan signal.
* [i[`SIG_DFL` macro]]`SIG_DFL` untuk mengembalikan handler bawaan signal
  itu.

Mari kita tulis program yang tidak bisa di-`CTRL-C`. (Tenang saja---pada
program berikut, kamu juga bisa menekan `RETURN` dan ia akan keluar.)

[i[`SIGINT` signal]<]

``` {.c .numberLines}
#include <stdio.h>
#include <signal.h>

int main(void)
{
    char s[1024];

    signal(SIGINT, SIG_IGN);    // Abaikan SIGINT, yang disebabkan oleh ^C

    printf("Try hitting ^C... (hit RETURN to exit)\n");

    // Tunggu satu baris input supaya program tidak langsung keluar
    fgets(s, sizeof s, stdin);
}
```

Lihat baris 8---kita menyuruh program mengabaikan `SIGINT`, signal
interupsi yang di-raise ketika `CTRL-C` ditekan. Sebanyak apa pun kamu
menekannya, signal itu tetap diabaikan. Kalau baris 8 dijadikan komentar,
kamu akan melihat bahwa kamu bisa `CTRL-C` sesuka hati dan langsung keluar
dari program.

[i[`SIGINT` signal]>]
[i[`SIG_IGN` macro]>]

## Menulis Signal Handler

Tadi kusebut bahwa kamu juga bisa menulis fungsi handler yang
dipanggil ketika signal di-raise.

Ini cukup lugas, tapi kemampuannya juga sangat terbatas menurut
spesifikasi.

[i[`signal()` function]<]

Sebelum mulai, mari kita lihat function prototype untuk pemanggilan
`signal()`:

``` {.c}
void (*signal(int sig, void (*func)(int)))(int);
```

Mudah dibaca, kan?

_SALAH!_ `:)`

Mari kita bongkar sebentar untuk latihan.

`signal()` menerima dua argumen: integer `sig` yang mewakili signal, dan
pointer `func` ke handler (handler mengembalikan `void` dan menerima `int`
sebagai argumen), disorot di bawah:

``` {.c}
                sig          func
              |-----|  |---------------|
void (*signal(int sig, void (*func)(int)))(int);
```

[i[`signal()` function]>]

Pada dasarnya, kita akan mengoper nomor signal yang ingin kita tangkap,
dan mengoper pointer ke fungsi berbentuk:

``` {.c}
void f(int x);
```

yang akan melakukan penangkapan sebenarnya.

Nah---bagaimana dengan sisa prototype itu? Isinya pada dasarnya semua tipe
kembalian. Begini, `signal()` akan mengembalikan apa pun yang kamu oper
sebagai `func` kalau berhasil... artinya ia mengembalikan pointer ke
fungsi yang mengembalikan `void` dan menerima `int` sebagai argumen.

``` {.c}
returned
function    indicates we're              and
returns     returning a                  that function
void        pointer to function          takes an int
|--|        |                                   |---|
void       (*signal(int sig, void (*func)(int)))(int);
```

Ia juga bisa mengembalikan [i[`SIG_ERR` macro]] `SIG_ERR` kalau terjadi
error.

Mari kita buat contoh yang membuatmu harus menekan `CTRL-C` dua kali untuk
keluar.

Aku ingin menegaskan bahwa program ini melibatkan undefined behavior dalam
beberapa hal. Tapi kemungkinan besar program ini berfungsi di sistemmu,
dan sulit membuat demo non-trivial yang portabel.

[i[`signal()` function]<]
[i[`SIG_INT` signal]<]

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>

int count = 0;

void sigint_handler(int signum)
{
    // Compiler boleh menjalankan:
    //
    //   signal(signum, SIG_DFL)
    //
    // saat handler dipanggil. Jadi kita reset handler-nya di sini:
    signal(SIGINT, sigint_handler);

    (void)signum;   // Hilangkan peringatan variabel tak terpakai

    count++;                       // Undefined behavior
    printf("Count: %d\n", count);  // Undefined behavior

    if (count == 2) {
        printf("Exiting!\n");      // Undefined behavior
        exit(0);
    }
}

int main(void)
{
    signal(SIGINT, sigint_handler);

    printf("Try hitting ^C...\n");

    for(;;);  // Tunggu di sini selamanya
}
```

[i[`SIG_INT` signal]>]

Salah satu hal yang akan kamu lihat: di baris 14 kita mereset signal
handler. Ini karena C punya opsi untuk mereset signal handler ke perilaku
[i[`SIG_DFL` macro]] `SIG_DFL`-nya sebelum menjalankan handler kustommu.
Dengan kata lain, handler itu bisa jadi sekali pakai. Jadi hal pertama yang
kita lakukan adalah mereset-nya agar signal berikutnya juga tertangani.

Dalam kasus ini kita mengabaikan nilai kembalian `signal()`. Kalau
sebelumnya kita sudah mengatur handler lain, ia akan mengembalikan pointer
ke handler itu, yang bisa kita ambil seperti ini:

``` {.c}
// old_handler bertipe "pointer ke fungsi yang menerima satu parameter
// int dan mengembalikan void":

void (*old_handler)(int);

old_handler = signal(SIGINT, sigint_handler);
```

[i[`signal()` function]>]

Meski begitu, aku tidak yakin ada kasus pemakaian umum untuk ini. Tapi
kalau kamu butuh handler lama karena suatu alasan, kamu bisa
mendapatkannya dengan cara itu.

Catatan singkat soal baris 16---itu hanya untuk memberi tahu compiler agar
tidak memperingatkan bahwa variabel ini tidak dipakai. Seperti berkata,
"Aku tahu aku tidak memakainya; tidak perlu memperingatkanku."

Dan terakhir, kamu akan melihat bahwa aku menandai undefined behavior
di beberapa tempat. Soal itu dibahas di bagian berikutnya.

## Apa yang Sebenarnya Bisa Kita Lakukan?

Ternyata kita cukup terbatas dalam hal apa yang bisa dan tidak bisa
dilakukan di signal handler. Ini salah satu alasan aku bilang kamu
tidak perlu repot dengan ini dan sebaiknya memakai penanganan signal OS
sendiri (misalnya [i[`sigaction()` function]] `sigaction()` untuk sistem
mirip Unix).

Wikipedia bahkan menyebut satu-satunya hal yang benar-benar portabel
adalah memanggil `signal()` dengan `SIG_IGN` atau `SIG_DFL`, itu saja.

Berikut yang **tidak bisa** kita lakukan secara portabel:

[i[Signal handling--->limitations]<]

* Memanggil fungsi standard library apa pun.
  * Seperti `printf()`, misalnya.
  * Kurasa mungkin aman memanggil fungsi restartable/reentrant,
    tapi spesifikasi tidak memberikan kebebasan itu.
* Mengambil atau mengisi nilai dari variabel lokal `static`, file scope,
  atau thread-local.
  * Kecuali kalau itu atomic object yang lock-free atau...
  * Kamu menugaskan ke variabel bertipe `volatile sig_atomic_t`.

[i[`sig_atomic_t` type]<]

Bagian terakhir itu--`sig_atomic_t`--adalah tiketmu untuk mengeluarkan data
dari signal handler. (Kecuali kamu ingin memakai atomic object lock-free,
yang di luar cakupan bagian ini^[Membingungkan, `sig_atomic_t` lebih tua
daripada atomic lock-free dan tidak sama dengannya.].) Ia adalah tipe
integer yang mungkin signed, mungkin tidak. Dan ia dibatasi oleh apa yang
bisa dimasukkan ke dalamnya.

Kamu bisa melihat nilai minimum dan maksimum yang diizinkan pada makro
`SIG_ATOMIC_MIN` dan `SIG_ATOMIC_MAX`^[Kalau `sig_action_t` signed,
rentangnya setidaknya `-127` sampai `127`. Kalau unsigned, setidaknya `0`
sampai `255`.].

Membingungkannya, spesifikasi juga menyatakan kamu tidak boleh merujuk "to any
object with static or thread storage duration that is not a lock-free
atomic object other than by assigning a value to an object declared as
`volatile sig_atomic_t` [...]"

Menurut pemahamanku, kamu tidak boleh membaca atau menulis apa pun
yang bukan atomic object lock-free. Kamu juga boleh menugaskan ke object
`volatile sig_atomic_t`.

Tapi bolehkah membacanya? Jujur, aku tidak melihat kenapa tidak, hanya
saja spesifikasi sangat tegas menyebut menugaskan ke. Tapi kalau kamu harus
membacanya dan membuat keputusan berdasarkan nilainya, kamu mungkin
membuka peluang terjadinya semacam race condition.

[i[Signal handling--->limitations]>]

Dengan itu dalam pikiran, kita bisa menulis ulang kode "tekan `CTRL-C` dua
kali untuk keluar" agar sedikit lebih portabel, walaupun outputnya jadi
lebih singkat.

Mari kita ubah handler `SIGINT` kita agar tidak melakukan apa-apa selain
menaikkan nilai bertipe `volatile sig_atomic_t`. Jadi ia akan menghitung
berapa kali `CTRL-C` ditekan.

Lalu di loop utama, kita periksa apakah penghitung itu melebihi `2`, dan
keluar kalau ya.

``` {.c .numberLines}
#include <stdio.h>
#include <signal.h>

volatile sig_atomic_t count = 0;

void sigint_handler(int signum)
{
    (void)signum;                    // Peringatan variabel tak terpakai

    signal(SIGINT, sigint_handler);  // Reset signal handler

    count++;                         // Undefined behavior
}

int main(void)
{
    signal(SIGINT, sigint_handler);

    printf("Hit ^C twice to exit.\n");

    while(count < 2);
}
```

[i[`sig_atomic_t` type]>]

Undefined behavior lagi? Menurutku iya, karena kita harus membaca
nilainya untuk menaikkan dan menyimpannya. Thread lain bisa saja mengutak-atik
`count` dan bikin kesal. Tapi pada kasus sederhana ini, tidak ada thread
lain yang melakukannya, jadi pelanggaran ini bisa dimaafkan dan kita nikmati
saja demonya.

Kalau kita hanya ingin menunda keluar satu kali penekanan `CTRL-C`, itu
bisa dilakukan tanpa terlalu repot. Tapi penundaan lebih banyak
membutuhkan perangkaian fungsi yang konyol.

Yang akan kita lakukan: tangani sekali, dan handler akan mereset signal ke
perilaku bawaannya (yaitu keluar):

[i[`SIG_DFL` macro]<]

``` {.c .numberLines}
#include <stdio.h>
#include <signal.h>

void sigint_handler(int signum)
{
    (void)signum;                      // Peringatan variabel tak terpakai
    signal(SIGINT, SIG_DFL);           // Reset signal handler
}

int main(void)
{
    signal(SIGINT, sigint_handler);

    printf("Hit ^C twice to exit.\n");

    while(1);
}
```

[i[`SIG_DFL` macro]>]

Nanti saat membahas variabel atomic lock-free, kita akan melihat cara
memperbaiki versi `count` (dengan asumsi variabel atomic lock-free tersedia
di sistemmu).

Inilah alasan di awal tadi aku menyarankan melihat sistem signal bawaan
OS-mu sebagai alternatif yang kemungkinan lebih unggul.

## Teman Tidak Membiarkan Temannya `signal()`

Sekali lagi, pakai penanganan signal bawaan OS-mu atau padanannya. Itu
tidak ada di spesifikasi, kurang portabel, tapi kemungkinan jauh lebih
mumpuni. Lagi pula OS-mu mungkin punya sejumlah signal yang tidak ada di
spesifikasi C. Dan menulis kode portabel dengan `signal()` memang sulit.

[i[`signal()` function]>]
[i[Signal handling]>]
