<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Multithreading

[i[Multithreading]<]

C11 secara resmi memperkenalkan multithreading ke bahasa C. Rasanya
sangat mirip dengan [flw[POSIX threads|POSIX_Threads]], kalau kamu pernah
memakainya.

Dan kalau belum, tidak apa-apa. Kita akan membahasnya.

Namun perlu dicatat, aku tidak bermaksud membuat panduan multithreading
klasik yang lengkap^[Aku sendiri lebih suka shared-nothing, dan
kemampuanku dengan konstruksi multithreading klasik berkarat,
paling tidak begitu.]; untuk itu kamu harus mencari buku lain yang sangat
tebal. Maaf!

[i[`__STDC_NO_THREADS__` macro]<]

Threading adalah fitur opsional. Kalau compiler C11+ mendefinisikan
`__STDC_NO_THREADS__`, thread **tidak** ada di library. Kenapa mereka
memilih makna negatif pada makro itu, aku tidak mengerti, tapi
begitulah adanya.

Kamu bisa mengujinya seperti ini:

``` {.c}
#ifdef __STDC_NO_THREADS__
#error I need threads to build this program!
#endif
```

[i[`__STDC_NO_THREADS__` macro]>]

[i[`gcc` compiler-->with threads]<]

Selain itu, kamu mungkin perlu menentukan opsi linker tertentu saat
membangun. Di sistem mirip Unix, coba tambahkan `-lpthreads` di akhir
command line untuk me-link library `pthreads`^[Ya, `pthreads` dengan
"`p`". Itu singkatan POSIX threads, library yang banyak dipinjam C11 untuk
implementasi thread-nya.]:

``` {.zsh}
gcc -std=c11 -o foo foo.c -lpthreads
```

[i[`gcc` compiler-->with threads]>]

Kalau kamu mendapat error linker di sistemmu, bisa jadi karena library
yang sesuai tidak disertakan.


## Latar Belakang

Thread adalah cara agar semua core CPU mengilap yang sudah kamu bayar
bekerja untukmu dalam program yang sama.

Biasanya, program C hanya berjalan di satu core CPU. Tapi kalau kamu tahu
cara membagi pekerjaan, kamu bisa memberikan potongannya ke sejumlah thread
dan menyuruh mereka mengerjakannya secara bersamaan.

Walaupun spesifikasi tidak menyebutnya, di sistemmu kemungkinan besar C
(atau OS atas perintahnya) akan berusaha menyeimbangkan thread di semua
core CPU-mu.

Dan kalau thread-nya lebih banyak daripada core, tidak apa-apa. Hanya saja
kamu tidak akan merasakan semua keuntungannya kalau semuanya berebut waktu
CPU.

## Hal-Hal yang Bisa Kamu Lakukan

Kamu bisa membuat thread. Ia akan mulai menjalankan fungsi yang kamu
tentukan. Parent thread yang melahirkannya juga tetap berjalan.

Dan kamu bisa menunggu thread selesai. Ini disebut _joining_.

Atau kalau kamu tidak peduli kapan thread selesai dan tidak mau menunggu,
kamu bisa _melepasnya (detach)_.

Sebuah thread bisa secara eksplisit _keluar_, atau secara implisit
berhenti dengan return dari fungsi utamanya.

Sebuah thread juga bisa _tidur_ selama jangka waktu tertentu, tidak
melakukan apa-apa selagi thread lain berjalan.

Program `main()` juga sebuah thread.

Selain itu, kita punya thread local storage, mutex, dan conditional
variable. Tapi soal itu nanti. Untuk sekarang kita lihat dasar-dasarnya
saja.

## Data Race dan Standard Library

[i[Multithreading-->and the standard library]<]

Sebagian fungsi di standard library (mis. `asctime()` dan `strtok()`)
mengembalikan atau memakai elemen data `static` yang tidak threadsafe.
Tapi secara umum, kecuali dinyatakan lain, standard library berusaha agar
threadsafe^[Menurut §7.1.4¶5.].

Tapi tetap waspada. Kalau sebuah fungsi standard library menyimpan state
antarpemanggilan di variabel yang bukan milikmu, atau kalau sebuah fungsi
mengembalikan pointer ke sesuatu yang tidak kamu oper, ia tidak
threadsafe.

[i[Multithreading-->and the standard library]>]

## Membuat dan Menunggu Thread

Mari kita buat sesuatu!

Kita akan membuat beberapa thread (create) dan menunggu mereka selesai
(join).

Tapi ada sedikit hal yang perlu dipahami dulu.

Setiap thread diidentifikasi oleh variabel opaque bertipe
[i[`thrd_t` type]] `thrd_t`. Ini identifier unik per thread di programmu.
Saat kamu membuat thread, ia diberi ID baru.

Saat membuat thread, kamu juga harus memberinya pointer ke fungsi yang
akan dijalankan, dan pointer ke argumen yang dioper kepadanya (atau `NULL`
kalau tidak ada yang dioper).

Thread akan mulai mengeksekusi fungsi yang kamu tentukan.

Saat ingin menunggu sebuah thread selesai, kamu harus menyebutkan ID
thread-nya supaya C tahu yang mana yang ditunggu.

Jadi gagasan dasarnya:

1. Tulis fungsi yang berperan sebagai "`main`" thread. Ini bukan
   `main()` yang sebenarnya, tapi analog dengannya. Thread akan mulai
   berjalan di sana.
2. Dari thread utama, jalankan thread baru dengan [i[`thrd_create()`
   function]]`thrd_create()`, dan oper pointer ke fungsi yang akan
   dijalankan.
3. Di fungsi itu, biarkan thread mengerjakan apa pun yang harus
   dikerjakannya.
4. Sementara itu, thread utama bisa melanjutkan apa pun yang harus
   dikerjakan _olehnya_.
5. Ketika thread utama memutuskan, ia bisa menunggu child thread selesai
   dengan memanggil [i[`thrd_join()` function]] `thrd_join()`. Pada
   umumnya kamu **harus** me-`thrd_join()` thread itu untuk membersihkan
   sisanya, kalau tidak memori akan bocor^[Kecuali kamu
   `thrd_detach()`. Soal ini dibahas nanti.]

[i[`thrd_create()` function]<]
[i[`thrd_start_t` type]<]

`thrd_create()` menerima pointer ke fungsi yang akan dijalankan, dan
bertipe `thrd_start_t`, yaitu `int (*)(void *)`. Itu bahasa Yunani untuk
"pointer ke fungsi yang menerima `void*` sebagai argumen, dan
mengembalikan `int`."

[i[`thrd_join()` function]<]

Mari kita buat thread! Kita akan menjalankannya dari thread utama dengan
`thrd_create()` untuk menjalankan sebuah fungsi, mengerjakan hal lain, lalu
menunggunya selesai dengan `thrd_join()`. Aku menamai fungsi utama thread ini
`run()`, tapi kamu bisa menamainya apa saja asalkan tipenya cocok
dengan `thrd_start_t`.

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

// Ini fungsi yang akan dijalankan thread. Boleh diberi nama apa saja.
//
// arg adalah pointer argumen yang dioper ke `thrd_create()`.
//
// Parent thread akan mendapat nilai kembaliannya dari `thrd_join()`'
// nanti.

int run(void *arg)
{
    int *a = arg;  // Kita akan mengoper int* dari thrd_create()

    printf("THREAD: Running thread with arg %d\n", *a);

    return 12;  // Nilai yang akan diambil thrd_join() (12 dipilih acak)
}

int main(void)
{
    thrd_t t;  // t akan menampung ID thread
    int arg = 3490;

    printf("Launching a thread\n");

    // Jalankan thread ke fungsi run(), dengan mengoper pointer ke 3490
    // sebagai argumen. ID thread juga disimpan di t:

    thrd_create(&t, run, &arg);

    printf("Doing other things while the thread runs\n");

    printf("Waiting for thread to complete...\n");

    int res;  // Menampung nilai kembalian dari keluarnya thread

    // Tunggu di sini sampai thread selesai; simpan nilai kembalian
    // di res:

    thrd_join(t, &res);

    printf("Thread exited with return value %d\n", res);
}
```

[i[`thrd_start_t` type]>]

Lihat bagaimana kita memakai `thrd_create()` di situ untuk memanggil
fungsi `run()`? Lalu kita mengerjakan hal lain di `main()` dan kemudian
berhenti dan menunggu thread selesai dengan `thrd_join()`.

[i[`thrd_create()` function]>]
[i[`thrd_join()` function]>]

Contoh output (hasilmu mungkin berbeda):

``` {.default}
Launching a thread
Doing other things while the thread runs
Waiting for thread to complete...
THREAD: Running thread with arg 3490
Thread exited with return value 12
```

`arg` yang dioper ke fungsi harus punya masa hidup yang cukup panjang agar
thread bisa mengambilnya sebelum ia hilang. Selain itu, ia tidak boleh
tertimpa oleh thread utama sebelum thread baru sempat memakainya.

Mari kita lihat contoh yang menjalankan 5 thread. Satu hal yang perlu
dicatat di sini adalah cara kita memakai array `thrd_t` untuk melacak
semua ID thread.

[i[`thrd_create()` function]<]
[i[`thrd_join()` function]<]

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

int run(void *arg)
{
    int i = *(int*)arg;

    printf("THREAD %d: running!\n", i);

    return i;
}

#define THREAD_COUNT 5

int main(void)
{
    thrd_t t[THREAD_COUNT];

    int i;

    printf("Launching threads...\n");
    for (i = 0; i < THREAD_COUNT; i++)

        // PERHATIAN! Pada baris berikut, kita mengoper pointer ke i,
        // tapi setiap thread melihat pointer yang sama. Jadi mereka akan
        // mencetak hal-hal aneh saat i berubah nilai di sini di
        // thread utama! (Lebih lanjut di teks, di bawah.)

        thrd_create(t + i, run, &i);

    printf("Doing other things while the thread runs...\n");
    printf("Waiting for thread to complete...\n");

    for (int i = 0; i < THREAD_COUNT; i++) {
        int res;
        thrd_join(t[i], &res);

        printf("Thread %d complete!\n", res);
    }

    printf("All threads complete!\n");
}
```

[i[`thrd_join()` function]>]

Saat aku menjalankan thread, aku menghitung `i` naik dari 0 sampai 4. Lalu aku
mengoper pointer ke `i` ke `thrd_create()`. Pointer ini berakhir di rutin `run()`
tempat kita membuat salinannya.

[i[`thrd_create()` function]>]

Cukup sederhana? Berikut outputnya:

``` {.default}
Launching threads...
THREAD 2: running!
THREAD 3: running!
THREAD 4: running!
THREAD 2: running!
Doing other things while the thread runs...
Waiting for thread to complete...
Thread 2 complete!
Thread 2 complete!
THREAD 5: running!
Thread 3 complete!
Thread 4 complete!
Thread 5 complete!
All threads complete!
```

Haaah---? Di mana `THREAD 0`? Dan kenapa ada `THREAD 5` padahal jelas `i`
tidak pernah lebih dari `4` saat kita memanggil `thrd_create()`? Dan ada
dua `THREAD 2`? Gila!

Ini sudah masuk ke wilayah seru [i[Multithreading-->race conditions]]
_race condition_. Thread utama mengubah `i` sebelum thread sempat
menyalinnya. Bahkan, `i` sampai ke `5` dan loop berakhir sebelum thread
terakhir sempat menyalinnya.

Kita harus punya variabel per-thread yang bisa dirujuk agar bisa dioper
sebagai `arg`.

Kita bisa membuat array besar berisi variabel itu. Atau kita bisa
`malloc()` ruangnya (dan membebaskannya di suatu tempat---mungkin di dalam
thread itu sendiri).

Kita coba:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <threads.h>

int run(void *arg)
{
    int i = *(int*)arg;  // Salin arg

    free(arg);  // Sudah selesai dengan ini

    printf("THREAD %d: running!\n", i);

    return i;
}

#define THREAD_COUNT 5

int main(void)
{
    thrd_t t[THREAD_COUNT];

    int i;

    printf("Launching threads...\n");
    for (i = 0; i < THREAD_COUNT; i++) {

        // Siapkan ruang untuk argumen per-thread:

        int *arg = malloc(sizeof *arg);
        *arg = i;

        thrd_create(t + i, run, arg);
    }

    // ...
```

Perhatikan di baris 27-30 kita `malloc()` ruang untuk `int` dan menyalin
nilai `i` ke dalamnya. Setiap thread baru mendapat variabel hasil
`malloc()` yang masih baru untuk dirinya sendiri, dan kita mengoper pointer
ke variabel itu ke fungsi `run()`.

Setelah `run()` membuat salinannya sendiri dari `arg` di baris 7, ia
men-`free()` `int` hasil `malloc()` itu. Dan karena sudah punya salinan
sendiri, ia bebas melakukan apa pun padanya.

Dan saat dijalankan, hasilnya:

``` {.default}
Launching threads...
THREAD 0: running!
THREAD 1: running!
THREAD 2: running!
THREAD 3: running!
Doing other things while the thread runs...
Waiting for thread to complete...
Thread 0 complete!
Thread 1 complete!
Thread 2 complete!
Thread 3 complete!
THREAD 4: running!
Thread 4 complete!
All threads complete!
```

Nah begitu! Thread 0-4 semuanya berjalan!

Hasil jalanmu mungkin berbeda---bagaimana thread dijadwalkan berada di
luar spesifikasi C. Pada contoh di atas kita lihat thread 4 bahkan belum
mulai sampai thread 0-1 selesai. Bahkan, kalau kujalankan lagi, kemungkinan
besar aku mendapat output berbeda. Kita tidak bisa menjamin urutan eksekusi thread.

## Melepas Thread (Detach)

Kalau kamu ingin melepas thread begitu saja (fire-and-forget), yaitu supaya
tidak perlu me-`thrd_join()`-nya nanti, kamu bisa melakukannya dengan
`thrd_detach()`.

Ini menghilangkan kemampuan parent thread untuk mengambil nilai kembalian
dari child thread, tapi kalau kamu tidak peduli soal itu dan hanya ingin
thread membereskan dirinya sendiri dengan rapi, inilah caranya.

Pada dasarnya kita akan melakukan ini:

[i[`thrd_detach()` function]<]

``` {.c}
thrd_create(&t, run, NULL);
thrd_detach(t);
```

Pemanggilan `thrd_detach()` adalah parent thread yang berkata, "Hei, aku
tidak akan menunggu child thread ini selesai dengan `thrd_join()`. Jadi
silakan bereskan sendiri saat selesai."

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

int run(void *arg)
{
    (void)arg;

    //printf("Thread running! %lu\n", thrd_current()); // tidak portabel!
    printf("Thread running!\n");

    return 0;
}

#define THREAD_COUNT 10

int main(void)
{
    thrd_t t;

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_create(&t, run, NULL);
        thrd_detach(t);               // <-- DETACH!
    }

    // Tidur selama satu detik supaya semua thread sempat selesai
    thrd_sleep(&(struct timespec){.tv_sec=1}, NULL);
}
```

[i[`thrd_detach()` function]>]

Perhatikan bahwa pada kode ini, thread utama ditidurkan selama 1 detik
dengan `thrd_sleep()`---soal itu dibahas nanti.

Di fungsi `run()` aku juga menaruh baris yang dikomentari untuk mencetak ID
thread sebagai `unsigned long`. Ini tidak portabel, karena spesifikasi
tidak menyebut tipe `thrd_t` sebenarnya apa---bisa saja ia `struct`. Tapi
baris itu berfungsi di sistemku.

Hal menarik yang kulihat saat menjalankan kode di atas dan mencetak ID
thread: sebagian thread punya ID kembar! Ini seharusnya mustahil, tapi C
boleh _memakai ulang_ ID thread setelah thread yang bersangkutan keluar.
Jadi yang kulihat adalah sebagian thread selesai berjalan sebelum thread
lain diluncurkan.

## Thread Local Data

[i[Thread local data]<]

Thread itu menarik karena mereka tidak punya memori sendiri di luar
variabel lokal. Kalau ada variabel `static` atau variabel file scope,
semua thread akan melihat variabel yang sama.

Ini bisa menyebabkan race condition, ketika _Hal-Hal Aneh_™ terjadi.

Lihat contoh ini. Ada variabel `static` `foo` di block scope pada
`run()`. Variabel ini terlihat oleh semua thread yang melewati fungsi
`run()`. Dan berbagai thread secara efektif bisa saling menginjak kaki.

Setiap thread menyalin `foo` ke variabel lokal `x` (yang tidak dibagi
antar thread---semua thread punya call stack sendiri). Jadi mereka
_seharusnya_ sama, kan?

Dan pada pencetakan pertama, memang sama^[Walaupun kurasa tidak
harus begitu. Hanya saja thread tampaknya tidak dijadwalkan ulang sampai
ada semacam system call, seperti yang mungkin terjadi pada `printf()`...
itulah kenapa ada `printf()` di situ.]. Tapi tepat setelah itu, kita
periksa untuk memastikan keduanya masih sama.

Dan _biasanya_ memang sama. Tapi tidak selalu!

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <threads.h>

int run(void *arg)
{
    int n = *(int*)arg;  // Nomor thread agar manusia bisa membedakan

    free(arg);

    static int foo = 10;  // Nilai static yang dibagi antar thread

    int x = foo;  // Variabel lokal otomatis--tiap thread punya sendiri

    // Kita baru saja menugaskan x dari foo, jadi mereka harus sama di sini.
    // (Di semua percobaanku memang sama, tapi bahkan ini tidak
    // dijamin!)

    printf("Thread %d: x = %d, foo = %d\n", n, x, foo);

    // Dan mereka seharusnya sama di sini, tapi tidak selalu!
    // (Kadang sama, kadang tidak!)

    // Yang terjadi: thread lain masuk dan meng-increment foo
    // saat ini juga, tapi x milik thread ini tetap seperti sebelumnya!

    if (x != foo) {
        printf("Thread %d: Craziness! x != foo! %d != %d\n", n, x, foo);
    }

    foo++;  // Increment nilai yang dibagi

    return 0;
}

#define THREAD_COUNT 5

int main(void)
{
    thrd_t t[THREAD_COUNT];

    for (int i = 0; i < THREAD_COUNT; i++) {
        int *n = malloc(sizeof *n);  // Menampung nomor seri thread
        *n = i;
        thrd_create(t + i, run, n);
    }

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_join(t[i], NULL);
    }
}
```

Berikut contoh outputnya (walau ini berbeda dari satu eksekusi ke
eksekusi lain):

``` {.default}
Thread 0: x = 10, foo = 10
Thread 1: x = 10, foo = 10
Thread 1: Craziness! x != foo! 10 != 11
Thread 2: x = 12, foo = 12
Thread 4: x = 13, foo = 13
Thread 3: x = 14, foo = 14
```

Pada thread 1, di antara dua `printf()`, nilai `foo` entah bagaimana
berubah dari `10` menjadi `11`, padahal jelas tidak ada increment di antara
kedua `printf()`!

Ada thread lain yang masuk ke sana (kemungkinan thread 0, kalau dilihat dari
hasilnya) dan meng-increment nilai `foo` di belakang punggung thread
1!

Mari kita selesaikan masalah ini dengan dua cara berbeda. (Kalau kamu
ingin semua thread berbagi variabel _dan_ tidak saling menginjak kaki,
kamu harus lanjut membaca bagian [mutex](#mutex).)

### Storage-Class `_Thread_local` {#thread-local}

[i[`_Thread_local` storage class]<]

Pertama-tama, mari kita lihat cara mudah mengatasinya: storage-class
`_Thread_local`.

Pada dasarnya kita tinggal menempelkannya di depan variabel `static`
block scope kita dan semuanya berfungsi! Ia memberi tahu C bahwa setiap
thread harus punya versinya sendiri dari variabel ini, sehingga tidak ada
yang saling menginjak kaki.

[i[`thread_local` storage class]<]

Header [i[`threads.h` header file]] `<threads.h>` mendefinisikan
`thread_local` sebagai alias untuk `_Thread_local` supaya kodemu tidak
perlu tampak begitu jelek.

Mari kita ambil contoh sebelumnya dan jadikan `foo` variabel
`thread_local` agar data itu tidak dibagi.

``` {.c .numberLines startFrom="5"}
int run(void *arg)
{
    int n = *(int*)arg;  // Nomor thread agar manusia bisa membedakan

    free(arg);

    thread_local static int foo = 10;  // <-- Tidak lagi dibagi!!
```

Dan saat dijalankan, kita mendapat:

``` {.default}
Thread 0: x = 10, foo = 10
Thread 1: x = 10, foo = 10
Thread 2: x = 10, foo = 10
Thread 4: x = 10, foo = 10
Thread 3: x = 10, foo = 10
```

Tidak ada lagi masalah aneh!

Satu hal: kalau variabel `thread_local` berada di block scope, ia
**harus** `static`. Begitulah aturannya. (Tapi ini tidak masalah karena
variabel non-`static` memang sudah per-thread, sebab tiap thread punya
variabel non-`static`-nya sendiri.)

Sedikit kebohongan di situ: variabel `thread_local` di block scope juga
boleh `extern`.

[i[`thread_local` storage class]>]
[i[`_Thread_local` storage class]>]
[i[Thread local data]>]

### Opsi Lain: Thread-Specific Storage

[i[Thread-specific storage]<]

Thread-specific storage (TSS) adalah cara lain untuk mendapatkan data
per-thread.

Satu fitur tambahan: fungsi-fungsi ini memungkinkanmu menentukan destructor
yang akan dipanggil pada data saat variabel TSS dihapus. Biasanya
destructor ini adalah `free()` untuk membersihkan data per-thread hasil
`malloc()` secara otomatis. Atau `NULL` kalau tidak ada yang perlu
dihancurkan.

Destructor bertipe [i[`tss_dtor_t` type]] `tss_dtor_t`, yaitu pointer ke
fungsi yang mengembalikan `void` dan menerima `void*` sebagai argumen
(`void*` itu menunjuk ke data yang disimpan di variabel). Dengan kata
lain, ia `void (*)(void*)`, kalau itu membantu menjelaskan. Dan aku akui,
kemungkinan itu tidak membantu. Lihat contoh di bawah.

Secara umum, `thread_local` kemungkinan pilihan utamamu, tapi kalau kamu
menyukai gagasan destructor, kamu bisa memanfaatkannya.

Pemakaiannya agak aneh karena kita butuh variabel bertipe [i[`tss_t`
type]] `tss_t` yang tetap hidup untuk mewakili nilai per thread. Lalu kita
menginisialisasinya dengan [i[`tss_create()` function]] `tss_create()`.
Akhirnya kita membuangnya dengan [i[`tss_delete()` function]<]
`tss_delete()`. Perhatikan bahwa memanggil `tss_delete()` tidak menjalankan
semua destructor---yang melakukannya adalah `thrd_exit()` (atau return dari
fungsi run). `tss_delete()` hanya melepas memori yang dialokasikan oleh
`tss_create()`. [i[`tss_delete()` function]>]

Di tengah-tengahnya, thread bisa memanggil [i[`tss_set()` function]]
`tss_set()` dan [i[`tss_get()` function]] `tss_get()` untuk mengisi dan
mengambil nilai.

Pada kode berikut, kita menyiapkan variabel TSS sebelum membuat thread,
lalu membersihkan setelah thread selesai.

Pada fungsi `run()`, thread melakukan `malloc()` ruang untuk string dan
menyimpan pointer itu di variabel TSS.

Saat thread keluar, fungsi destructor (`free()` dalam kasus ini) dipanggil
untuk _semua_ thread.

[i[`tss_t` type]<]
[i[`tss_get()` function]<]
[i[`tss_set()` function]<]
[i[`tss_create()` function]<]
[i[`tss_delete()` function]<]

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <threads.h>

tss_t str;

void some_function(void)
{
    // Ambil nilai per-thread dari string ini
    char *tss_string = tss_get(str);

    // Lalu cetak
    printf("TSS string: %s\n", tss_string);
}

int run(void *arg)
{
    int serial = *(int*)arg;  // Ambil nomor seri thread ini
    free(arg);

    // malloc() ruang untuk menampung data thread ini
    char *s = malloc(64);
    sprintf(s, "thread %d! :)", serial);  // String kecil yang riang

    // Atur variabel TSS ini agar menunjuk ke string
    tss_set(str, s);

    // Panggil fungsi yang akan mengambil variabelnya
    some_function();

    return 0;   // Setara dengan thrd_exit(0)
}

#define THREAD_COUNT 15

int main(void)
{
    thrd_t t[THREAD_COUNT];

    // Buat variabel TSS baru, fungsi free() adalah destructor-nya
    tss_create(&str, free);

    for (int i = 0; i < THREAD_COUNT; i++) {
        int *n = malloc(sizeof *n);  // Menampung nomor seri thread
        *n = i;
        thrd_create(t + i, run, n);
    }

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_join(t[i], NULL);
    }

    // Semua thread sudah selesai, jadi kita selesai dengan ini
    tss_delete(str);
}
```

[i[`tss_t` type]>]
[i[`tss_get()` function]>]
[i[`tss_set()` function]>]
[i[`tss_create()` function]>]
[i[`tss_delete()` function]>]

Sekali lagi, ini cara yang agak menyakitkan dibanding `thread_local`, jadi
kecuali kamu benar-benar membutuhkan fungsionalitas destructor itu,
aku akan memakai `thread_local` saja.

[i[Thread-specific storage]>]

## Mutex {#mutex}

[i[Mutexes]<]

Kalau kamu hanya ingin mengizinkan satu thread masuk ke bagian kritis kode
pada satu waktu, kamu bisa melindungi bagian itu dengan mutex^[Singkatan
"mutual exclusion", alias "lock" pada bagian kode yang hanya boleh
dieksekusi satu thread.].

Misalnya, kalau ada variabel `static` dan kita ingin bisa mengambil dan
mengisinya dalam dua operasi tanpa thread lain melompat ke tengah dan
merusaknya, kita bisa memakai mutex untuk itu.

Kamu bisa memperoleh (acquire) mutex atau melepasnya. Kalau kamu mencoba
memperoleh mutex dan berhasil, kamu boleh melanjutkan eksekusi. Kalau
mencoba dan gagal (karena orang lain memegangnya), kamu akan
_block_^[Artinya, prosesmu akan tidur.] sampai mutex dilepas.

Kalau beberapa thread diblokir menunggu mutex dilepas, salah satunya akan
dipilih untuk berjalan (secara acak, dari sudut pandang kita), dan yang
lain tetap tidur.

Rencana permainannya: pertama kita inisialisasi variabel mutex agar siap
dipakai dengan [i[`mtx_init()` function]] `mtx_init()`.

Lalu thread-thread berikutnya bisa memanggil [i[`mtx_lock()` function]]
`mtx_lock()` dan [i[`mtx_unlock` function]] `mtx_unlock()` untuk
memperoleh dan melepas mutex.

Saat kita benar-benar selesai dengan mutex, kita bisa menghancurkannya
dengan [i[`mtx_destroy()` function]] `mtx_destroy()`, kebalikan logis dari
[i[`mtx_init()` function]] `mtx_init()`.

[i[Multithreading-->race conditions]<]

Pertama, mari kita lihat kode yang _tidak_ memakai mutex, dan berusaha
mencetak nomor seri yang dibagi (`static`) lalu meng-increment-nya. Karena
kita tidak memakai mutex pada pengambilan nilai (untuk dicetak) dan
pengisiannya (untuk di-increment), thread bisa saling menghalangi di
bagian kritis itu.

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

int run(void *arg)
{
    (void)arg;

    static int serial = 0;   // Variabel static yang dibagi!

    printf("Thread running! %d\n", serial);

    serial++;

    return 0;
}

#define THREAD_COUNT 10

int main(void)
{
    thrd_t t[THREAD_COUNT];

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_create(t + i, run, NULL);
    }

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_join(t[i], NULL);
    }
}
```

Waktu kujalankan, hasilnya kira-kira seperti ini:

``` {.default}
Thread running! 0
Thread running! 0
Thread running! 0
Thread running! 3
Thread running! 4
Thread running! 5
Thread running! 6
Thread running! 7
Thread running! 8
Thread running! 9
```

Jelas beberapa thread masuk ke sana dan menjalankan `printf()` sebelum
ada yang sempat memperbarui variabel `serial`.

[i[Multithreading-->race conditions]>]

Yang ingin kita lakukan adalah membungkus pengambilan variabel dan
pengisiannya ke dalam satu bentangan kode yang dilindungi mutex.

Kita tambahkan variabel baru untuk mewakili mutex bertipe [i[`mtx_t`
type]] `mtx_t` di file scope, menginisialisasinya, lalu thread bisa
mengunci dan membukanya di fungsi `run()`.

[i[`mtx_lock()` function]<]
[i[`mtx_unlock()` function]<]
[i[`mtx_init()` function]<]
[i[`mtx_destroy()` function]<]

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

mtx_t serial_mtx;     // <-- VARIABEL MUTEX

int run(void *arg)
{
    (void)arg;

    static int serial = 0;   // Variabel static yang dibagi!

    // Peroleh mutex--semua thread akan terblokir di pemanggilan ini
    // sampai mereka mendapat lock:

    mtx_lock(&serial_mtx);           // <-- PEROLEH MUTEX

    printf("Thread running! %d\n", serial);

    serial++;

    // Selesai mengambil dan mengisi data, jadi lepas lock. Ini akan
    // membuka blokir thread pada pemanggilan mtx_lock():

    mtx_unlock(&serial_mtx);         // <-- LEPAS MUTEX

    return 0;
}

#define THREAD_COUNT 10

int main(void)
{
    thrd_t t[THREAD_COUNT];

    // Inisialisasi variabel mutex, menandakan ini mutex biasa tanpa
    // embel-embel:

    mtx_init(&serial_mtx, mtx_plain);        // <-- BUAT MUTEX

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_create(t + i, run, NULL);
    }

    for (int i = 0; i < THREAD_COUNT; i++) {
        thrd_join(t[i], NULL);
    }

    // Selesai dengan mutex, hancurkan:

    mtx_destroy(&serial_mtx);                // <-- HANCURKAN MUTEX
}
```

Lihat bagaimana di baris 38 dan 50 pada `main()` kita menginisialisasi dan
menghancurkan mutex.

[i[`mtx_init()` function]>]
[i[`mtx_destroy()` function]>]

Tapi setiap thread memperoleh mutex di baris 15 dan melepasnya di baris
24.

Di antara `mtx_lock()` dan `mtx_unlock()` adalah _critical section_,
bagian kode yang tidak ingin kita biarkan dikacau beberapa thread
sekaligus.

[i[`mtx_lock()` function]>]
[i[`mtx_unlock()` function]>]

Dan sekarang kita mendapat output yang benar!

``` {.default}
Thread running! 0
Thread running! 1
Thread running! 2
Thread running! 3
Thread running! 4
Thread running! 5
Thread running! 6
Thread running! 7
Thread running! 8
Thread running! 9
```

Kalau butuh beberapa mutex, tidak masalah: punya saja beberapa variabel
mutex.

Dan selalu ingat Aturan Nomor Satu untuk Banyak Mutex: _Buka kunci mutex
dalam urutan terbalik dari urutan saat mengunci!_

### Berbagai Tipe Mutex

[i[Mutexes-->types]<]

Seperti yang diisyaratkan tadi, ada beberapa tipe mutex yang bisa dibuat
dengan `mtx_init()`. (Sebagian tipe ini adalah hasil operasi bitwise-OR,
seperti tercatat di tabel.)

[i[Mutexes-->timeouts]<]

[i[`mtx_plain` macro]]
[i[`mtx_timed` macro]]
[i[`mtx_plain` macro]]
[i[`mtx_timed` macro]]
[i[`mtx_recursive` macro]]

|Tipe|Deskripsi|
|-|-|
|`mtx_plain`|Mutex biasa|
|`mtx_timed`|Mutex yang mendukung timeout|
|`mtx_plain|mtx_recursive`|Mutex rekursif|
|`mtx_timed|mtx_recursive`|Mutex rekursif yang mendukung timeout|

"Rekursif" berarti pemegang lock boleh memanggil `mtx_lock()` berkali-kali
pada lock yang sama. (Ia harus membukanya sebanyak itu juga sebelum orang
lain bisa mengambil mutex.) Ini kadang memudahkan penulisan kode,
terutama kalau kamu memanggil fungsi yang perlu mengunci mutex padahal
kamu sudah memegang mutex itu.

Dan timeout memberi thread kesempatan untuk _mencoba_ mendapat lock selama
beberapa waktu, tapi menyerah kalau tidak berhasil dalam jangka waktu itu.

[i[`mtx_timed` macro]<]

Untuk mutex dengan timeout, pastikan membuatnya dengan `mtx_timed`:

``` {.c}
mtx_init(&serial_mtx, mtx_timed);
```

[i[`mtx_timed` macro]>]

Lalu saat menunggunya, kamu harus menentukan waktu dalam UTC kapan ia akan
dibuka^[Mungkin kamu mengira itu "waktu dari sekarang", tapi kamu pasti
ingin berpikir begitu, kan!].

[i[`timespec_get()` function]<]

Fungsi `timespec_get()` dari `<time.h>` bisa membantu di sini. Ia
memberimu waktu saat ini dalam UTC di `struct timespec`, yang persis
kita butuhkan. Bahkan, ia tampaknya ada hanya untuk tujuan ini.

Ia punya dua field: `tv_sec` berisi waktu saat ini dalam detik sejak
epoch, dan `tv_nsec` berisi nanodetik (sepersemiliar detik) sebagai bagian
"pecahan".

Jadi kamu bisa mengisinya dengan waktu saat ini, lalu menambahkannya untuk
mendapat timeout tertentu.

[i[`mtx_timedlock()` function]<]

Lalu panggil `mtx_timedlock()` alih-alih `mtx_lock()`. Kalau ia
mengembalikan nilai `thrd_timedout`, berarti timeout.

``` {.c}
struct timespec timeout;

timespec_get(&timeout, TIME_UTC);  // Ambil waktu saat ini
timeout.tv_sec += 1;               // Timeout 1 detik setelah sekarang

int result = mtx_timedlock(&serial_mtx, &timeout));

if (result == thrd_timedout) {
    printf("Mutex lock timed out!\n");
}
```

[i[`mtx_timedlock()` function]>]
[i[`timespec_get()` function]>]

Selain itu, timed lock sama saja dengan lock biasa.

[i[Mutexes-->timeouts]>]
[i[Mutexes-->types]>]
[i[Mutexes]>]

## Condition Variable

[i[Condition variables]<]

Condition Variable adalah kepingan terakhir yang kita butuhkan untuk
membuat aplikasi multithread yang berkinerja baik dan menyusun struktur
multithread yang lebih kompleks.

Condition variable menyediakan cara agar thread tidur sampai suatu
kejadian di thread lain terjadi.

Dengan kata lain, mungkin ada sejumlah thread yang sudah tidak sabar
berangkat, tapi mereka harus menunggu sampai suatu kejadian benar sebelum
melanjutkan. Pada dasarnya mereka disuruh "tunggu dulu!" sampai
diberi tahu.

Dan ini bekerja beriringan dengan mutex karena yang kita tunggu biasanya
bergantung pada nilai suatu data, dan data itu biasanya perlu dilindungi
mutex.

Penting dicatat bahwa condition variable itu sendiri bukan penyimpan data
tertentu dari sudut pandang kita. Ia hanyalah variabel yang dipakai C untuk
melacak status menunggu/tidak menunggu dari sebuah thread atau kelompok
thread.

Mari kita tulis program rekaan yang membaca kelompok berisi 5 angka dari
thread utama satu per satu. Lalu, ketika 5 angka sudah dimasukkan, child
thread bangun, menjumlahkan kelima angka itu, dan mencetak hasilnya.

Angka-angka itu akan disimpan di array global yang dibagi, begitu juga
indeks ke array untuk angka yang akan dimasukkan.

Karena ini nilai yang dibagi, setidaknya kita harus menyembunyikannya di
balik mutex untuk thread utama maupun child thread. (Thread utama akan
menulis data ke sana dan child thread akan membaca data darinya.)

Tapi itu belum cukup. Child thread perlu diblokir ("tidur") sampai 5 angka
terbaca ke array. Lalu parent thread perlu membangunkan child thread agar
bisa bekerja.

Dan saat bangun, ia perlu memegang mutex itu. Dan memang akan! Ketika
thread menunggu pada condition variable, ia juga memperoleh mutex saat
bangun.

Semua ini terjadi seputar variabel tambahan bertipe [i[`cnd_t` type]]
`cnd_t` yang adalah _condition variable_. Kita membuat variabel ini dengan
fungsi [i[`cnd_init()` function]] `cnd_init()` dan menghancurkannya saat
selesai dengan fungsi [i[`cnd_destroy()` function]] `cnd_destroy()`.

Tapi bagaimana semuanya bekerja? Mari kita lihat garis besar apa yang akan
dilakukan child thread:

[i[`mtx_lock()` function]<]
[i[`mtx_unlock()` function]<]
[i[`cnd_wait()` function]<]
[i[`cnd_signal()` function]<]

1. Kunci mutex dengan `mtx_lock()`
2. Kalau kita belum memasukkan semua angka, tunggu pada condition variable
   dengan `cnd_wait()`
3. Kerjakan pekerjaan yang harus dikerjakan
4. Buka kunci mutex dengan `mtx_unlock()`

Sementara itu thread utama akan melakukan ini:

1. Kunci mutex dengan `mtx_lock()`
2. Simpan angka yang baru dibaca ke array
3. Kalau array penuh, beri sinyal agar child bangun dengan `cnd_signal()`
4. Buka kunci mutex dengan `mtx_unlock()`

[i[`mtx_lock()` function]>]
[i[`mtx_unlock()` function]>]

Kalau kamu tidak terlalu cepat membacanya sekilas (tidak apa-apa---aku
tidak tersinggung), mungkin kamu memperhatikan sesuatu yang aneh:
bagaimana thread utama bisa memegang kunci mutex dan memberi sinyal ke
child, kalau child harus memegang kunci mutex untuk menunggu sinyal?
Keduanya tidak bisa memegang kunci!

Dan memang tidak! Ada sihir di balik layar pada condition variable: saat
kamu `cnd_wait()`, ia melepas mutex yang kamu tentukan dan thread tidur.
Dan ketika seseorang memberi sinyal agar thread itu bangun, ia memperoleh
kembali kunci itu seolah-olah tidak terjadi apa-apa.

Di sisi `cnd_signal()` sedikit berbeda. Ia tidak melakukan apa-apa
terhadap mutex. Thread yang memberi sinyal masih harus melepas mutex secara
manual sebelum thread yang menunggu bisa bangun.

[i[`cnd_signal()` function]>]

Satu hal lagi soal `cnd_wait()`. Kamu kemungkinan akan memanggil
`cnd_wait()` kalau suatu kondisi^[Dan itulah kenapa mereka disebut
_condition variable_!] belum terpenuhi (mis. dalam kasus ini, kalau belum
semua angka dimasukkan). Intinya: kondisi ini harus berada dalam loop
`while`, bukan pernyataan `if`. Kenapa?

Itu karena fenomena misterius bernama [i[Condition variables-->spurious
wakeup]] _spurious wakeup_. Kadang, pada beberapa implementasi, sebuah
thread bisa terbangun dari tidur `cnd_wait()` tanpa _alasan_ yang jelas.
_[Musik X-Files]_^[Aku tidak bilang ini ulah alien... tapi ini ulah
alien. Oke, sebenarnya lebih mungkin thread lain sudah terbangun dan sampai
ke pekerjaannya lebih dulu.]. Karena itu kita harus memeriksa bahwa kondisi
yang kita butuhkan masih benar-benar terpenuhi saat bangun. Dan kalau
tidak, kembali tidur!

[i[`cnd_wait()` function]>]

Jadi mari kita kerjakan! Mulai dari thread utama:

* Thread utama akan menyiapkan mutex dan condition variable, dan
  meluncurkan child thread.

* Lalu ia akan, dalam loop tak terbatas, mengambil angka sebagai input dari
  konsol.

* Ia juga akan memperoleh mutex untuk menyimpan angka yang dimasukkan ke
  array global.

* Ketika array berisi 5 angka, thread utama akan memberi sinyal ke child
  thread bahwa saatnya bangun dan bekerja.

* Lalu thread utama akan membuka kunci mutex dan kembali membaca angka
  berikutnya dari konsol.

Sementara itu, child thread sibuk dengan kenakalannya sendiri:

* Child thread mengambil mutex

* Selama kondisi belum terpenuhi (yaitu selama array bersama belum berisi
  5 angka), child thread tidur dengan menunggu pada condition variable.
  Saat menunggu, ia secara implisit membuka kunci mutex.

* Begitu thread utama memberi sinyal agar child thread bangun, ia bangun
  untuk bekerja dan mendapatkan kembali kunci mutex.

* Child thread menjumlahkan angka-angka itu dan mereset variabel yang
  menjadi indeks ke array.

* Lalu ia melepas mutex dan berjalan lagi dalam loop tak terbatas.

Dan inilah kodenya! Pelajari sebentar supaya kamu bisa melihat di mana
semua bagian di atas ditangani:

[i[`mtx_lock()` function]<]
[i[`mtx_unlock()` function]<]
[i[`mtx_init()` function]<]
[i[`mtx_destroy()` function]<]
[i[`cnd_init()` function]<]
[i[`cnd_destroy()` function]<]
[i[`cnd_wait()` function]<]
[i[`cnd_signal()` function]<]
[i[`cnd_t` type]<]

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

#define VALUE_COUNT_MAX 5

int value[VALUE_COUNT_MAX];  // Global yang dibagi
int value_count = 0;   // Global yang dibagi juga

mtx_t value_mtx;   // Mutex untuk value
cnd_t value_cnd;   // Condition variable untuk value

int run(void *arg)
{
    (void)arg;

    for (;;) {
        mtx_lock(&value_mtx);      // <-- AMBIL MUTEX

        while (value_count < VALUE_COUNT_MAX) {
            printf("Thread: is waiting\n");
            cnd_wait(&value_cnd, &value_mtx);  // <-- TUNGGU KONDISI
        }

        printf("Thread: is awake!\n");

        int t = 0;

        // Jumlahkan semuanya
        for (int i = 0; i < VALUE_COUNT_MAX; i++)
            t += value[i];

        printf("Thread: total is %d\n", t);

        // Reset indeks input untuk thread utama
        value_count = 0;

        mtx_unlock(&value_mtx);   // <-- BUKA KUNCI MUTEX
    }

    return 0;
}

int main(void)
{
    thrd_t t;

    // Luncurkan thread baru

    thrd_create(&t, run, NULL);
    thrd_detach(t);

    // Siapkan mutex dan condition variable

    mtx_init(&value_mtx, mtx_plain);
    cnd_init(&value_cnd);

    for (;;) {
        int n;

        scanf("%d", &n);

        mtx_lock(&value_mtx);    // <-- KUNCI MUTEX

        value[value_count++] = n;

        if (value_count == VALUE_COUNT_MAX) {
            printf("Main: signaling thread\n");
            cnd_signal(&value_cnd);  // <-- BERI SINYAL KONDISI
        }

        mtx_unlock(&value_mtx);  // <-- BUKA KUNCI MUTEX
    }

    // Bersih-bersih (loop di atas ini tak terbatas, tapi kita ingin
    // setidaknya berpura-pura rapi):

    mtx_destroy(&value_mtx);
    cnd_destroy(&value_cnd);
}
```

[i[`mtx_lock()` function]>]
[i[`mtx_unlock()` function]>]
[i[`mtx_init()` function]>]
[i[`mtx_destroy()` function]>]
[i[`cnd_init()` function]>]
[i[`cnd_destroy()` function]>]
[i[`cnd_wait()` function]>]
[i[`cnd_signal()` function]>]
[i[`cnd_t` type]>]

Dan berikut contoh output (angka-angka tunggal di baris adalah
inputku):

``` {.default}
Thread: is waiting
1
1
1
1
1
Main: signaling thread
Thread: is awake!
Thread: total is 5
Thread: is waiting
2
8
5
9
0
Main: signaling thread
Thread: is awake!
Thread: total is 24
Thread: is waiting
```

Ini pemakaian condition variable yang umum pada situasi
producer-consumer seperti ini. Kalau kita tidak punya cara untuk
menidurkan child thread selagi ia menunggu suatu kondisi terpenuhi, ia
akan terpaksa melakukan polling, yang sangat memboroskan CPU.

### Condition Wait dengan Timeout

[i[Condition variables-->timeouts]<]

Ada varian `cnd_wait()` yang memungkinkanmu menentukan timeout agar bisa
berhenti menunggu.

Karena child thread harus mengunci ulang mutex, ini belum tentu berarti
kamu langsung hidup kembali tepat saat timeout terjadi; kamu masih harus
menunggu thread lain melepas mutex.

Tapi artinya kamu tidak akan menunggu sampai `cnd_signal()` terjadi.

Agar ini berfungsi, panggil [i[`cnd_timedwait()` function]]
`cnd_timedwait()` alih-alih `cnd_wait()`. Kalau ia mengembalikan nilai
[i[`thrd_timedout` macro]] `thrd_timedout`, berarti timeout.

Timestamp-nya adalah waktu absolut dalam UTC, bukan waktu-dari-sekarang.
Untungnya fungsi [i[`timespec_get()` function]] `timespec_get()` di
`<time.h>` tampaknya dibuat khusus untuk kasus ini.

[i[`timespec_get()` function]<]
[i[`cnd_timedwait()` function]<]
[i[`thrd_timedout()` macro]<]

``` {.c}
struct timespec timeout;

timespec_get(&timeout, TIME_UTC);  // Ambil waktu saat ini
timeout.tv_sec += 1;               // Timeout 1 detik setelah sekarang

int result = cnd_timedwait(&condition, &mutex, &timeout));

if (result == thrd_timedout) {
    printf("Condition variable timed out!\n");
}
```

[i[`timespec_get()` function]>]
[i[`cnd_timedwait()` function]>]
[i[`thrd_timedout()` macro]>]
[i[Condition variables-->timeouts]>]

### Broadcast: Membangunkan Semua Thread yang Menunggu

[i[Condition variables-->broadcasting]<]

[i[`cnd_signal()` function]>] `cnd_signal()` hanya membangunkan satu
thread untuk melanjutkan pekerjaan. Tergantung bagaimana logikamu disusun,
mungkin masuk akal membangunkan lebih dari satu thread untuk melanjutkan
begitu kondisi terpenuhi.

Tentu saja hanya satu dari mereka yang bisa mengambil mutex, tapi kalau
kamu punya situasi ketika:

* Thread yang baru bangun bertanggung jawab membangunkan thread
  berikutnya, dan---

* Ada kemungkinan kondisi loop spurious-wakeup mencegahnya melakukan itu,
  maka---

kamu perlu men-broadcast pembangunan itu agar yakin setidaknya satu thread
keluar dari loop itu untuk meluncurkan thread berikutnya.

Bagaimana caranya, tanyamu?

[i[`cnd_broadcast()` function]<]

Cukup pakai `cnd_broadcast()` alih-alih `cnd_signal()`. Pemakaiannya
persis sama, hanya saja `cnd_broadcast()` membangunkan **semua** thread
yang tidur menunggu condition variable itu.

[i[`cnd_broadcast()` function]>]
[i[Condition variables-->broadcasting]>]
[i[Condition variables]>]

## Menjalankan Fungsi Satu Kali

[i[Multithreading-->one-time functions]<]

Katakanlah kamu punya fungsi yang _bisa_ dijalankan banyak thread, tapi
kamu tidak tahu kapan, dan tidak sepadan repot menulis semua logikanya.

Ada jalan keluarnya: pakai [i[`call_once()` function]] `call_once()`.
Banyak sekali thread bisa mencoba menjalankan fungsi itu, tapi hanya yang
pertama yang dihitung^[Yang kuat yang bertahan! Benar? Harus diakui ini
sebenarnya sama sekali tidak seperti itu.]

Untuk memakainya, kamu butuh variabel flag khusus yang dideklarasikan
untuk melacak apakah fungsi itu sudah dijalankan atau belum. Dan kamu
butuh fungsi yang akan dijalankan, yang tidak menerima parameter dan tidak
mengembalikan nilai.

[i[`once_flag` type]<]
[i[`ONCE_FLAG_INIT` macro]<]
[i[`call_once()` function]<]

``` {.c}
once_flag of = ONCE_FLAG_INIT;  // Inisialisasi seperti ini

void run_once_function(void)
{
    printf("I'll only run once!\n");
}

int run(void *arg)
{
    (void)arg;

    call_once(&of, run_once_function);

    // ...
```

[i[`once_flag` type]>]
[i[`ONCE_FLAG_INIT` macro]>]
[i[`call_once()` function]>]

Pada contoh ini, berapa pun banyaknya thread yang sampai ke fungsi
`run()`, `run_once_function()` hanya akan dipanggil satu kali.

[i[Multithreading-->one-time functions]>]
[i[Multithreading]>]
