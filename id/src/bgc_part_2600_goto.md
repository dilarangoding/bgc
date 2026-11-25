<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `goto`

[i[`goto` statement]<]

Pernyataan `goto` dipuja secara universal dan bisa disajikan di sini tanpa
perdebatan.

Bercanda! Bertahun-tahun lamanya, orang bolak-balik berdebat apakah
`goto` [flw[dianggap berbahaya|Goto#Criticism]] atau tidak (sering kali
dianggap tidak boleh).

Menurut pendapat programmer yang satu ini, kamu sebaiknya memakai konstruksi apa pun yang
menghasilkan kode _terbaik_, dengan mempertimbangkan kemudahan perawatan
dan kecepatan. Dan kadang itu mungkin `goto`!

Di bab ini, kita akan melihat cara kerja `goto` di C, lalu mengintip
beberapa kasus umum tempat ia dipakai^[Aku ingin menunjukkan bahwa memakai
`goto` pada semua kasus ini bisa dihindari. Kamu bisa memakai variabel dan
loop sebagai gantinya. Hanya saja sebagian orang menganggap `goto`
menghasilkan kode _terbaik_ pada keadaan-keadaan itu.].

## Contoh Sederhana

[i[Labels]<]

Pada contoh ini, kita memakai `goto` untuk melewati satu baris kode dan
melompat ke sebuah _label_. Label adalah identifier yang bisa menjadi
target `goto`---ia diakhiri titik dua (`:`).

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    printf("One\n");
    printf("Two\n");

    goto skip_3;

    printf("Three\n");

skip_3:

    printf("Five!\n");
}
```

Outputnya:

``` {.default}
One
Two
Five!
```

`goto` membuat eksekusi melompat ke label yang disebutkan, melewati
semua yang ada di antaranya.

Kamu bisa melompat maju maupun mundur dengan `goto`.

``` {.c}
infinite_loop:
    print("Hello, world!\n");
    goto infinite_loop;
```

Label dilewati saat eksekusi. Yang berikut akan mencetak ketiga angka
secara berurutan seolah-olah label itu tidak ada:

``` {.c}
    printf("Zero\n");
label_1:
label_2:
    printf("One\n");
label_3:
    printf("Two\n");
label_4:
    printf("Three\n");
```

Seperti yang kamu perhatikan, konvensi umumnya adalah merapatkan label
sepenuhnya ke kiri. Ini meningkatkan keterbacaan karena pembaca bisa
dengan cepat memindai untuk menemukan tujuannya.

Label punya _function scope_. Artinya, sedalam apa pun label itu berada di
dalam blok, kamu tetap bisa `goto` ke sana dari mana saja di dalam fungsi.

Artinya juga kamu hanya bisa `goto` ke label yang berada di fungsi yang
sama dengan `goto`-nya. Label di fungsi lain berada di luar scope dari
sudut pandang `goto`. Dan artinya kamu bisa memakai nama label yang sama di
dua fungsi---hanya tidak boleh nama label yang sama di fungsi yang sama.

[i[Labels]>]

## `continue` Berlabel

[i[`goto` statement-->as labeled `continue`]<]

Di beberapa bahasa, kamu sebenarnya bisa menentukan label untuk pernyataan
`continue`. C tidak mengizinkannya, tapi kamu dengan mudah bisa memakai
`goto` sebagai gantinya.

Untuk menunjukkan masalahnya, lihat `continue` pada loop bersarang ini:

``` {.c}
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        printf("%d, %d\n", i, j);
        continue;   // Selalu menuju j berikutnya
    }
}
```

Seperti yang kita lihat, `continue` itu, seperti semua `continue`,
menuju iterasi berikutnya dari loop terdekat yang melingkupinya. Bagaimana
kalau kita ingin `continue` di loop yang lebih luar, yaitu loop `i`?

Nah, kita bisa `break` untuk kembali ke loop luar, kan?

``` {.c}
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        printf("%d, %d\n", i, j);
        break;     // Membawa kita ke iterasi i berikutnya
    }
}
```

Itu membawa kita melewati dua tingkat loop bersarang. Tapi kalau kita
menyarangkan loop lagi, pilihan kita habis. Bagaimana dengan yang ini,
ketika tidak ada pernyataan yang bisa membawa kita keluar ke iterasi `i`
berikutnya?

``` {.c}
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        for (int k = 0; k < 3; k++) {
            printf("%d, %d, %d\n", i, j, k);

            continue;  // Membawa kita ke iterasi k berikutnya
            break;     // Membawa kita ke iterasi j berikutnya
            ????;      // Membawa kita ke iterasi i berikutnya???

        }
    }
}
```

Pernyataan `goto` menawarkan jalannya!

``` {.c}
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 3; k++) {
                printf("%d, %d, %d\n", i, j, k);

                goto continue_i;   // Sekarang melanjutkan loop i!!
            }
        }
continue_i: ;
    }
```

Ada `;` di bagian akhir di sana---itu karena label tidak boleh menunjuk ke
ujung polos dari sebuah compound statement (atau sebelum deklarasi
variabel).

[i[`goto` statement-->as labeled `continue`]>]

## Keluar Darurat

[i[`goto` statement-->for bailing out]<]

Ketika kamu berada di dalam kode yang sangat bersarang, kamu bisa memakai
`goto` untuk keluar dengan cara yang sering kali lebih bersih daripada
menyarangkan lebih banyak `if` dan memakai variabel flag.

``` {.c}
    // Pseudocode

    for(...) {
        for (...) {
            while (...) {
                do {
                    if (some_error_condition)
                        goto bail;

                } while(...);
            }
        }
    }

bail:
    // Pembersihan di sini
```

Tanpa `goto`, kamu harus memeriksa flag kondisi error di semua loop untuk
bisa keluar sepenuhnya.

[i[`goto` statement-->for bailing out]>]

## `break` Berlabel

[i[`goto` statement-->as labeled `break`]<]

Situasinya sangat mirip dengan `continue` yang hanya melanjutkan loop
paling dalam. `break` juga hanya keluar dari loop paling dalam.

``` {.c}
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d, %d\n", i, j);
            break;   // Hanya keluar dari loop j
        }
    }

    printf("Done!\n");
```

Tapi kita bisa memakai `goto` untuk keluar lebih jauh:

``` {.c}
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d, %d\n", i, j);
            goto break_i;   // Sekarang keluar dari loop i!
        }
    }

break_i:

    printf("Done!\n");
```

[i[`goto` statement-->as labeled `break`]>]

## Pembersihan Bertingkat

[i[`goto` statement-->multilevel cleanup]<]

Kalau kamu memanggil beberapa fungsi untuk menginisialisasi beberapa
sistem dan salah satunya gagal, kamu hanya boleh men-deinisialisasi yang
sudah kamu capai sejauh ini.

Mari kita buat contoh palsu ketika kita mulai menginisialisasi sistem dan
memeriksa apakah ada yang mengembalikan error (kita pakai `-1` untuk
menandakan error). Kalau ada yang begitu, kita hanya perlu mematikan
sistem yang sudah kita inisialisasi sejauh ini.

``` {.c}
    if (init_system_1() == -1)
        goto shutdown;

    if (init_system_2() == -1)
        goto shutdown_1;

    if (init_system_3() == -1)
        goto shutdown_2;

    if (init_system_4() == -1)
        goto shutdown_3;

    do_main_thing();   // Jalankan program kita

    shutdown_system4();

shutdown_3:
    shutdown_system3();

shutdown_2:
    shutdown_system2();

shutdown_1:
    shutdown_system1();

shutdown:
    print("All subsystems shut down.\n");
```

Perhatikan bahwa kita mematikan dalam urutan terbalik dari urutan
inisialisasi subsistem. Jadi kalau subsistem 4 gagal mulai, ia akan
mematikan 3, 2, lalu 1 dalam urutan itu.

[i[`goto` statement-->multilevel cleanup]>]

## Tail Call Optimization

[i[`goto` statement-->tail call optimzation]<]
[i[Tail call optimzation-->with `goto`]<]

Kurang lebih. Hanya untuk fungsi rekursif.

Kalau kamu belum akrab, [flw[Tail Call Optimization (TCO)|Tail_call]]
adalah cara agar tidak menyia-nyiakan ruang stack saat memanggil fungsi
lain pada keadaan yang sangat spesifik. Sayangnya detailnya di luar
cakupan panduan ini.

Tapi kalau kamu punya fungsi rekursif yang kamu tahu bisa dioptimasi
dengan cara ini, kamu bisa memakai teknik ini. (Perhatikan bahwa kamu
tidak bisa melakukan tail call ke fungsi lain karena label punya function
scope.)

Kita buat contoh yang lugas, faktorial.

Berikut versi rekursif yang bukan TCO, tapi bisa dijadikan TCO!

``` {.c .numberLines}
#include <stdio.h>
#include <complex.h>

int factorial(int n, int a)
{
    if (n == 0)
        return a;

    return factorial(n - 1, a * n);
}

int main(void)
{
    for (int i = 0; i < 8; i++)
        printf("%d! == %ld\n", i, factorial(i, 1));
}
```

Untuk mewujudkannya, kamu bisa mengganti pemanggilan itu dengan dua
langkah:

1. Atur nilai parameter menjadi nilai yang akan dipakai pada pemanggilan
   berikutnya.
2. `goto` ke label di baris pertama fungsi.

Kita coba:

``` {.c .numberLines}
#include <stdio.h>

int factorial(int n, int a)
{
tco:  // tambahkan ini

    if (n == 0)
        return a;

    // ganti return dengan mengatur nilai parameter baru dan
    // goto ke awal fungsi

    //return factorial(n - 1, a * n);

    int next_n = n - 1;  // Lihat bagaimana ini cocok dengan
    int next_a = a * n;  // argumen rekursif di atas?

    n = next_n;   // Atur parameter ke nilai baru
    a = next_a;

    goto tco;   // Dan ulangi!
}

int main(void)
{
    for (int i = 0; i < 8; i++)
        printf("%d! == %d\n", i, factorial(i, 1));
}
```

Di atas, aku memakai variabel sementara untuk mengatur nilai
parameter berikutnya sebelum melompat ke awal fungsi. Lihat bagaimana
mereka cocok dengan argumen rekursif pada pemanggilan rekursifnya?

Nah, kenapa memakai variabel sementara? Aku bisa saja melakukan ini:

``` {.c}
    a *= n;
    n -= 1;

    goto tco;
```

dan itu sebenarnya berfungsi dengan baik. Tapi kalau aku ceroboh menukar dua baris kode itu:

``` {.c}
    n -= 1;  // KABAR BURUK
    a *= n;
```

---sekarang kita dalam masalah. Kita mengubah `n` sebelum memakainya untuk
mengubah `a`. Itu Buruk karena memang bukan begitu cara kerjanya saat
memanggil secara rekursif. Memakai variabel sementara menghindari masalah
ini bahkan kalau kamu tidak waspada. Dan compiler kemungkinan
mengoptimasi mereka hingga hilang, bagaimanapun juga.

[i[`goto` statement-->tail call optimzation]>]
[i[Tail call optimzation-->with `goto`]>]

## Memulai Ulang System Call yang Terinterupsi

[i[`goto` statement-->restarting system calls]<]

Ini di luar spesifikasi, tapi umum terlihat di sistem mirip Unix.

Beberapa system call yang berjalan lama mungkin mengembalikan error kalau
diinterupsi oleh signal, dan `errno` akan diatur ke `EINTR` untuk
menandakan bahwa syscall itu baik-baik saja; ia hanya terinterupsi.

Dalam kasus itu, sangat umum programmer ingin memulai ulang pemanggilan
itu dan mencobanya lagi.

``` {.c}
retry:
    byte_count = read(0, buf, sizeof(buf) - 1);  // syscall read() Unix

    if (byte_count == -1) {            // Terjadi error...
        if (errno == EINTR) {          // Tapi hanya karena terinterupsi
            printf("Restarting...\n");
            goto retry;
        }
```

Banyak sistem mirip Unix punya flag `SA_RESTART` yang bisa dioper ke
`sigaction()` untuk meminta OS memulai ulang secara otomatis syscall
lambat apa pun alih-alih gagal dengan `EINTR`.

Sekali lagi, ini khusus Unix dan di luar standar C.

Meski begitu, teknik serupa bisa dipakai kapan pun sebuah fungsi perlu
dimulai ulang.

[i[`goto` statement-->restarting system calls]>]

## `goto` dan Thread Preemption

[i[`goto` statement-->thread preemption]<]

Contoh ini diambil langsung dari [_Operating Systems: Three Easy
Pieces_](http://www.ostep.org/), buku luar biasa lain dari penulis yang
sepaham dan juga merasa buku bermutu seharusnya bisa diunduh gratis. Bukan
berarti aku keras kepala atau semacamnya.

``` {.c}
retry:

    pthread_mutex_lock(L1);

    if (pthread_mutex_trylock(L2) != 0) {
        pthread_mutex_unlock(L1);
        goto retry;
    }

    save_the_day();

    pthread_mutex_unlock(L2);
    pthread_mutex_unlock(L1);
```

Di situ thread dengan senang hati mendapatkan mutex `L1`, tapi kemudian
berpotensi gagal mendapatkan sumber daya kedua yang dijaga mutex `L2`
(misalnya kalau thread lain yang tidak kooperatif memegangnya). Kalau
thread kita tidak bisa mendapatkan lock `L2`, ia melepas `L1` lalu memakai
`goto` untuk mencoba ulang dengan rapi.

Kita berharap thread pahlawan kita akhirnya berhasil mendapatkan kedua
mutex dan menyelamatkan hari, sambil menghindari deadlock jahat.

[i[`goto` statement-->thread preemption]>]

## `goto` dan Scope Variabel 

[i[`goto` statement-->variable scope]<]

Kita sudah melihat bahwa label punya function scope, tapi hal-hal aneh bisa
terjadi kalau kita melompati inisialisasi variabel.

Lihat contoh ini ketika kita melompat dari tempat variabel `x` berada di
luar scope ke tengah scope-nya (di dalam blok).

``` {.c}
    goto label;

    {
        int x = 12345;

label:
        printf("%d\n", x);
    }
```

Ini akan dikompilasi dan dijalankan, tapi aku mendapat peringatan:

``` {.default}
warning: ‘x’ is used uninitialized in this function
```

Lalu mencetak `0` saat kujalankan (hasilmu mungkin berbeda).

Pada dasarnya yang terjadi: kita melompat ke scope `x` (jadi boleh
merujuknya di `printf()`) tapi melompati baris yang benar-benar
menginisialisasinya ke `12345`. Jadi nilainya tak tentu.

Perbaikannya, tentu saja, menempatkan inisialisasi _setelah_ label dengan
satu atau lain cara.

``` {.c}
    goto label;

    {
        int x;

label:
        x = 12345;
        printf("%d\n", x);
    }
```

Mari kita lihat satu contoh lagi.

``` {.c}
    {
        int x = 10;

label:

        printf("%d\n", x);
    }

    goto label;
```

Apa yang terjadi di sini?

Pada kali pertama melewati blok, semuanya baik. `x` bernilai `10` dan
itulah yang tercetak.

Tapi setelah `goto`, kita melompat ke scope `x`, tapi melewati
inisialisasinya. Artinya kita masih bisa mencetaknya, tapi nilainya tak
tentu (karena belum diinisialisasi ulang).

Di mesinku, ia mencetak `10` lagi (sampai tak terhingga), tapi itu
hanya keberuntungan. Ia bisa mencetak nilai apa pun setelah `goto` karena
`x` belum diinisialisasi.

[i[`goto` statement-->variable scope]>]

## `goto` dan Variable-Length Array

[i[`goto` statement-->with variable-length arrays]<]

Soal VLA dan `goto`, hanya ada satu aturan: kamu tidak boleh melompat dari
luar scope VLA ke dalam scope VLA itu.

Kalau aku mencoba ini:

``` {.c}
    int x = 10;

    goto label;

    {
        int v[x];

label:

        printf("Hi!\n");
    }
```

aku mendapat error:

``` {.default}
error: jump into scope of identifier with variably modified type
```

Kamu bisa melompat masuk sebelum deklarasi VLA, seperti ini:

``` {.c}
    int x = 10;

    goto label;

    {
label:  ;
        int v[x];

        printf("Hi!\n");
    }
```

Karena dengan begitu VLA dialokasikan dengan benar sebelum
dealokasinya yang tak terelakkan saat keluar dari scope.

[i[`goto` statement-->with variable-length arrays]>]
[i[`goto` statement]>]
