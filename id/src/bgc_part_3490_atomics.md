<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Atomics {#chapter-atomics}

> _"They tried and failed, all of them?"_ \
> _"Oh, no." She shook her head. "They tried and died."_
>
> ---Paul Atreides and The Reverend Mother Gaius Helen Mohiam, _Dune_

[i[Atomic variables]<]

Ini salah satu aspek multithreading di C yang paling menantang. Tapi kita
akan berusaha santai.

Pada dasarnya, aku akan membahas pemakaian variabel atomic yang lebih
lugas, apa itu, bagaimana cara kerjanya, dan sebagainya. Dan aku akan
menyebut beberapa jalur yang jauh lebih rumit yang tersedia bagimu.

Tapi aku tidak akan menempuh jalur-jalur itu. Bukan hanya karena
aku nyaris tidak punya kualifikasi untuk menulis tentangnya, tapi
kupikir kalau kamu tahu kamu membutuhkannya, kamu sudah tahu lebih
banyak daripada aku.

Tapi ada hal-hal aneh bahkan di dasar-dasarnya. Jadi kencangkan sabuk
pengaman, semuanya, karena Kansas akan pamit pergi.

## Menguji Dukungan Atomic

[i[`__STDC_NO_ATOMICS__` macro]<]

Atomic adalah fitur opsional. Ada makro `__STDC_NO_ATOMICS__` yang bernilai
`1` kalau kamu _tidak_ punya atomic.

Makro itu mungkin tidak ada sebelum C11, jadi kita sebaiknya menguji versi
bahasanya dengan `__STDC_VERSION__`^[Makro `__STDC_VERSION__` tidak ada di
C89 awal, jadi kalau kamu khawatir soal itu, periksa dengan `#ifdef`.].

``` {.c}
#if __STDC_VERSION__ < 201112L || __STDC_NO_ATOMICS__ == 1
#define HAS_ATOMICS 0
#else
#define HAS_ATOMICS 1
#endif
```

[i[`__STDC_NO_ATOMICS__` macro]>]

[i[Atomic variables-->compiling with]<]

Kalau pengujian itu lolos, kamu aman meng-include `<stdatomic.h>`, header
yang menjadi dasar sisa bab ini. Tapi kalau tidak ada dukungan atomic,
header itu mungkin bahkan tidak ada.

Di beberapa sistem, kamu mungkin perlu menambahkan `-latomic` di akhir
command line kompilasi untuk memakai fungsi apa pun di header file itu.

[i[Atomic variables-->compiling with]>]

## Variabel Atomic

Berikut _sebagian_ dari cara kerja variabel atomic:

Kalau kamu punya variabel atomic yang dibagi dan kamu menulis ke sana dari
satu thread, penulisan itu akan _semua-atau-tidak-sama-sekali_ di thread
yang berbeda.

Artinya, thread lain akan melihat seluruh penulisan, misalnya nilai
32-bit. Bukan setengahnya. Tidak ada cara bagi satu thread untuk
menginterupsi thread lain yang sedang berada di _tengah_ penulisan atomic
multibyte.

Hampir seperti ada kunci kecil di sekitar pengambilan dan pengisian
variabel itu. (Dan _mungkin_ memang ada! Lihat [Variabel Atomic
Lock-Free](#lock-free-atomic), di bawah.)

Dan soal itu, kamu bisa saja tidak pernah memakai atomic kalau memakai
mutex untuk mengunci critical section-mu. Hanya saja ada kelas _struktur
data lock-free_ yang selalu mengizinkan thread lain membuat kemajuan
alih-alih terblokir oleh mutex... tapi ini sulit dibuat dengan benar dari
nol, dan merupakan salah satu hal yang di luar cakupan panduan ini,
sayangnya.

Itu baru sebagian ceritanya. Tapi itulah bagian yang kita mulai.

Sebelum lanjut, bagaimana cara mendeklarasikan variabel sebagai atomic?

Pertama, include [i[`stdatomic.h` header]] `<stdatomic.h>`.

[i[`atomic_int` type]<]

Ini memberi kita tipe seperti `atomic_int`.

Lalu kita tinggal mendeklarasikan variabel bertipe itu.

Tapi mari kita buat demo dengan dua thread. Yang pertama berjalan
sebentar lalu mengisi variabel dengan nilai tertentu, lalu keluar. Yang
lain berjalan sampai ia melihat nilai itu terisi, lalu keluar.

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>
#include <stdatomic.h>

atomic_int x;   // KEKUATAN ATOMIC! BWAHAHAHA!

int thread1(void *arg)
{
    (void)arg;

    printf("Thread 1: Sleeping for 1.5 seconds\n");
    thrd_sleep(&(struct timespec){.tv_sec=1, .tv_nsec=500000000}, NULL);

    printf("Thread 1: Setting x to 3490\n");
    x = 3490;

    printf("Thread 1: Exiting\n");
    return 0;
}

int thread2(void *arg)
{
    (void)arg;

    printf("Thread 2: Waiting for 3490\n");
    while (x != 3490) {}  // berputar di sini

    printf("Thread 2: Got 3490--exiting!\n");
    return 0;
}

int main(void)
{
    x = 0;

    thrd_t t1, t2;

    thrd_create(&t1, thread1, NULL);
    thrd_create(&t2, thread2, NULL);

    thrd_join(t1, NULL);
    thrd_join(t2, NULL);

    printf("Main    : Threads are done, so x better be 3490\n");
    printf("Main    : And indeed, x == %d\n", x);
}
```

[i[`atomic_int` type]>]

Thread kedua berputar di tempat, memandangi flag dan menunggunya diisi
dengan nilai `3490`. Dan thread pertama melakukannya.

Dan outputnya:

``` {.default}
Thread 1: Sleeping for 1.5 seconds
Thread 2: Waiting for 3490
Thread 1: Setting x to 3490
Thread 1: Exiting
Thread 2: Got 3490--exiting!
Main    : Threads are done, so x better be 3490
Main    : And indeed, x == 3490
```

Lihat, Ma! Kita mengakses variabel dari thread berbeda dan tidak memakai
mutex! Dan itu akan berhasil setiap kali berkat sifat atomic dari variabel
atomic.

Mungkin kamu bertanya-tanya apa yang terjadi kalau itu `int` biasa yang
non-atomic. Nah, di sistemku tetap berfungsi... kecuali kalau aku melakukan build
dengan optimasi, dan dalam kasus itu ia menggantung di thread 2 menunggu
melihat 3490 terisi^[Alasannya, saat dioptimasi, compilerku menaruh
nilai `x` di register agar loop `while` cepat. Tapi register tidak punya
cara untuk tahu bahwa variabel diperbarui di thread lain, jadi ia tidak
pernah melihat `3490`. Ini sebenarnya tidak terkait dengan bagian
_semua-atau-tidak-sama-sekali_ dari atomicity, tapi lebih terkait dengan
aspek sinkronisasi di bagian berikutnya.].

Tapi itu baru awal ceritanya. Bagian berikutnya akan membutuhkan lebih
banyak tenaga otak dan berkaitan dengan sesuatu yang disebut
_sinkronisasi_.

## Sinkronisasi

[i[Atomic variables-->synchronization]<]

Bagian cerita berikutnya sepenuhnya tentang kapan penulisan memori tertentu
di satu thread menjadi terlihat oleh thread lain.

Mungkin kamu mengira, langsung terlihat, kan? Tapi tidak. Banyak hal bisa
salah. Salah secara aneh.

Compiler mungkin sudah menata ulang akses memori sehingga ketika kamu
mengira sebuah nilai diisi lebih dulu dibanding yang lain, itu belum tentu
benar. Dan kalaupun compiler tidak melakukannya, CPU-mu bisa melakukannya
secara langsung. Atau mungkin ada hal lain di arsitektur ini yang membuat
penulisan di satu CPU tertunda sebelum terlihat di CPU lain.

Kabar baiknya, semua potensi masalah ini bisa kita ringkas menjadi satu:
akses memori yang tidak tersinkronisasi bisa tampak tidak berurutan
tergantung thread mana yang mengamati, seolah-olah baris kodenya sendiri
telah ditata ulang.

Sebagai contoh, mana yang terjadi lebih dulu pada kode berikut,
penulisan ke `x` atau penulisan ke `y`?

``` {.c .numberLines}
int x, y;  // global

// ...

x = 2;
y = 3;

printf("%d %d\n", x, y);
```

Jawabannya: kita tidak tahu. Compiler atau CPU bisa diam-diam menukar
baris 5 dan 6 dan kita tidak akan tahu. Kode berjalan single-thread
_seolah-olah_ dieksekusi sesuai urutan kode.

Dalam skenario multithread, kita mungkin punya sesuatu seperti pseudocode
ini:

``` {.c .numberLines}
int x = 0, y = 0;

thread1() {
    x = 2;
    y = 3;
}

thread2() {
    while (y != 3) {}  // berputar
    printf("x is now %d\n", x);  // 2? ...atau 0?
}
```

Apa output dari thread 2?

Nah, kalau `x` diisi `2` _sebelum_ `y` diisi `3`, maka aku akan
mengharapkan output yang sangat masuk akal:

``` {.default}
x is now 2 
```

Tapi sesuatu yang licik bisa menata ulang baris 4 dan 5 sehingga kita
melihat nilai `0` untuk `x` saat mencetaknya.

Dengan kata lain, semua taruhan batal kecuali kita bisa entah bagaimana
berkata, "Mulai titik ini, aku mengharapkan semua penulisan sebelumnya di
thread lain terlihat di thread ini."

Dua thread _tersinkronisasi_ ketika mereka sepakat tentang keadaan memori
bersama. Seperti yang sudah kita lihat, mereka tidak selalu sepakat dengan
kode. Jadi bagaimana mereka sepakat?

Memakai variabel atomic bisa memaksa kesepakatan itu^[Sampai kukatakan
lain, aku berbicara secara umum tentang operasi _sequentially
consistent_. Soal artinya dibahas sebentar lagi.]. Kalau sebuah thread
menulis ke variabel atomic, ia berkata "siapa pun yang membaca variabel
atomic ini di masa depan juga akan melihat semua perubahan yang kubuat pada
memori (atomic atau bukan) sampai dan termasuk variabel atomic itu".

Atau, dalam bahasa yang lebih manusiawi: mari duduk mengelilingi meja
konferensi dan pastikan kita sepakat soal bagian memori bersama mana yang
berisi nilai apa. Kamu setuju bahwa perubahan memori yang kamu buat
sampai-dan-termasuk atomic store akan terlihat olehku setelah aku
melakukan load pada variabel atomic yang sama.

Jadi kita bisa dengan mudah memperbaiki contoh kita:

``` {.c .numberLines}
int x = 0;
atomic int y = 0;  // Jadikan y atomic

thread1() {
    x = 2;
    y = 3;             // Sinkronisasi saat menulis
}

thread2() {
    while (y != 3) {}  // Sinkronisasi saat membaca
    printf("x is now %d\n", x);  // 2, titik.
}
```

Karena thread-thread itu tersinkronisasi melalui `y`, semua penulisan di
thread 1 yang terjadi _sebelum_ penulisan ke `y` terlihat di thread 2
_setelah_ pembacaan dari `y` (di loop `while`).

Penting dicatat beberapa hal di sini:

1. Tidak ada yang tidur. Sinkronisasi bukan operasi yang memblokir. Kedua
   thread berjalan penuh sampai keluar. Bahkan yang terjebak di spin loop
   pun tidak menghalangi yang lain berjalan.

2. Sinkronisasi terjadi ketika satu thread membaca variabel atomic yang
   ditulis thread lain. Jadi ketika thread 2 membaca `y`, semua penulisan
   memori sebelumnya di thread 1 (yaitu mengisi `x`) akan terlihat di
   thread 2.

3. Perhatikan bahwa `x` tidak atomic. Itu tidak masalah karena kita tidak
   menyinkronkan lewat `x`, dan sinkronisasi lewat `y` saat kita
   menulisnya di thread 1 berarti semua penulisan sebelumnya---termasuk
   `x`---di thread 1 akan terlihat oleh thread lain... kalau thread lain
   itu membaca `y` untuk sinkronisasi.

Memaksa sinkronisasi ini tidak efisien dan bisa jauh lebih lambat daripada
sekadar memakai variabel biasa. Itulah kenapa kita tidak memakai atomic
kecuali terpaksa untuk aplikasi tertentu.

Jadi itulah dasar-dasarnya. Mari kita lihat lebih dalam.

[i[Atomic variables-->synchronization]<]

## Acquire dan Release

[i[Atomic variables-->acquire]<]
[i[Atomic variables-->release]<]

Terminologi lagi! Akan bermanfaat mempelajarinya sekarang.

Ketika sebuah thread membaca variabel atomic, itu disebut operasi
_acquire_.

Ketika sebuah thread menulis variabel atomic, itu disebut operasi
_release_.

Apa maksudnya? Mari kita sejajarkan dengan istilah yang sudah kamu kenal
terkait variabel atomic:

**Read = Load = Acquire**. Seperti saat kamu membandingkan variabel atomic
atau membacanya untuk disalin ke nilai lain.

**Write = Store = Release**. Seperti saat kamu menugaskan nilai ke
variabel atomic.

Saat memakai variabel atomic dengan semantik acquire/release ini, C
menjabarkan apa yang bisa terjadi kapan.

Acquire/release membentuk dasar sinkronisasi yang baru saja kita bahas.

Ketika sebuah thread meng-acquire variabel atomic, ia bisa melihat nilai
yang diisi di thread lain yang me-release variabel yang sama.

Dengan kata lain:

Ketika sebuah thread membaca variabel atomic, ia bisa melihat nilai yang
diisi di thread lain yang menulis ke variabel yang sama.

Sinkronisasi terjadi di sepasang acquire/release.

Detail lebih lanjut:

Dengan read/load/acquire pada variabel atomic tertentu:

* Semua penulisan (atomic maupun non-atomic) di thread lain yang terjadi
  sebelum thread lain itu write/store/release variabel atomic ini kini
  terlihat di thread ini.

* Nilai baru variabel atomic yang diisi thread lain juga terlihat di
  thread ini.

* Tidak ada pembacaan atau penulisan variabel/memori apa pun di thread
  saat ini yang boleh ditata ulang untuk terjadi sebelum acquire ini.

* Acquire berperan sebagai penghalang satu arah dalam penataan ulang kode;
  pembacaan dan penulisan di thread saat ini bisa dipindah turun dari
  _sebelum_ acquire ke _sesudah_-nya. Tapi, yang lebih penting untuk
  sinkronisasi, tidak ada yang bisa pindah naik dari _sesudah_ acquire ke
  _sebelum_-nya.

Dengan write/store/release pada variabel atomic tertentu:

* Semua penulisan (atomic maupun non-atomic) di thread saat ini yang
  terjadi sebelum release ini menjadi terlihat oleh thread lain yang
  telah read/load/acquire variabel atomic yang sama.

* Nilai yang ditulis ke variabel atomic ini oleh thread ini juga terlihat
  oleh thread lain.

* Tidak ada pembacaan atau penulisan variabel/memori apa pun di thread
  saat ini yang boleh ditata ulang untuk terjadi setelah release ini.

* Release berperan sebagai penghalang satu arah dalam penataan ulang
  kode: pembacaan dan penulisan di thread saat ini bisa dipindah naik dari
  _sesudah_ release ke _sebelum_-nya. Tapi, yang lebih penting untuk
  sinkronisasi, tidak ada yang bisa pindah turun dari _sebelum_ release ke
  _sesudah_-nya.

Sekali lagi, intinya adalah sinkronisasi memori dari satu thread ke thread
lain. Thread kedua bisa yakin bahwa variabel dan memori ditulis dalam
urutan yang dimaksudkan programmer.

```
int x, y, z = 0;
atomic_int a = 0;

thread1() {
    x = 10;
    y = 20;
    a = 999;  // Release
    z = 30;
}

thread2()
{
    while (a != 999) { } // Acquire

    assert(x == 10);  // tidak pernah gagal, x selalu 10
    assert(y == 20);  // tidak pernah gagal, y selalu 20

    assert(z == 0);  // mungkin gagal!!
}
```

Pada contoh di atas, `thread2` bisa yakin akan nilai di `x` dan `y`
setelah meng-acquire `a` karena keduanya diisi sebelum `thread1`
me-release atomic `a`.

Tapi `thread2` tidak bisa yakin akan nilai `z` karena itu terjadi setelah
release. Mungkin penugasan ke `z` dipindah ke sebelum penugasan ke `a`.

Catatan penting: me-release satu variabel atomic tidak berpengaruh pada
acquire variabel atomic yang berbeda. Setiap variabel terisolasi dari yang
lain.

[i[Atomic variables-->acquire]>]
[i[Atomic variables-->release]>]

## Sequential Consistency

[i[Atomic variables-->sequential consistency]<]

Masih bertahan? Kita sudah melewati inti pemakaian atomic yang lebih
sederhana. Dan karena kita bahkan tidak akan membahas pemakaian yang lebih
kompleks di sini, kamu bisa sedikit santai.

_Sequential consistency_ adalah yang disebut _memory ordering_. Ada banyak
memory ordering, tapi sequential consistency adalah yang paling
waras^[Paling waras dari sudut pandang programmer.] yang ditawarkan C. Ia
juga yang bawaan. Kamu harus berusaha keras untuk memakai memory ordering
lain.

Semua yang sudah kita bahas sejauh ini terjadi dalam ranah sequential
consistency.

Kita sudah membahas bagaimana compiler atau CPU bisa menata ulang
pembacaan dan penulisan memori dalam satu thread selama mengikuti aturan
_as-if_.

Dan kita sudah melihat bagaimana kita bisa mengerem perilaku ini dengan
menyinkronkan lewat variabel atomic.

Mari kita formalkan sedikit lagi.

Kalau operasi-operasi _sequentially consistent_, artinya pada akhirnya,
ketika semuanya selesai, semua thread bisa menyelonjorkan kaki, membuka
minuman pilihan masing-masing, dan semuanya sepakat tentang urutan
terjadinya perubahan memori selama eksekusi. Dan urutan itu adalah yang
ditentukan oleh kode.

Tidak akan ada yang berkata, "Bukankah _B_ terjadi sebelum _A_?" kalau
yang lain berkata, "_A_ pasti terjadi sebelum _B_". Mereka semua berteman
di sini.

Khususnya, dalam satu thread, tidak ada acquire dan release yang boleh
ditata ulang terhadap satu sama lain. Ini tambahan dari aturan tentang
akses memori lain apa yang boleh ditata ulang di sekitarnya.

Aturan ini memberi tingkat kewarasan tambahan pada perkembangan
load/acquire dan store/release atomic.

Setiap memory order lain di C melibatkan pelonggaran aturan penataan
ulang, baik untuk acquire/release maupun akses memori lain, atomic atau
bukan. Kamu melakukannya kalau kamu _benar-benar_ tahu apa yang kamu
lakukan dan butuh dorongan kecepatan. _Di sini ada pasukan naga..._

Soal itu dibahas nanti, tapi untuk sekarang, kita bertahan di yang aman dan
praktis.

[i[Atomic variables-->sequential consistency]>]

## Penugasan dan Operator Atomic

[i[Atomic variables-->assignments and operators]<]

Operator tertentu pada variabel atomic bersifat atomic. Dan yang lain
tidak.

Kita mulai dengan contoh tandingan:

``` {.c}
atomic_int x = 0;

thread1() {
    x = x + 3;  // TIDAK atomic!
}
```

Karena ada pembacaan `x` di sisi kanan penugasan dan penulisan di sisi
kiri, ini dua operasi. Thread lain bisa menyelinap di tengah dan
membuatmu kesal.

Tapi kamu _bisa_ memakai bentuk singkat `+=` untuk mendapat operasi atomic:

``` {.c}
atomic_int x = 0;

thread1() {
    x += 3;   // ATOMIC!
}
```

Dalam kasus itu, `x` akan di-increment sebesar `3` secara atomic---tidak
ada thread lain yang bisa melompat ke tengah.

Khususnya, operator-operator berikut adalah operasi atomic
read-modify-write dengan sequential consistency, jadi pakailah dengan
riang gembira. (Pada contoh, `a` bersifat atomic.)

``` {.c}
a++       a--       --a       ++a
a += b    a -= b    a *= b    a /= b    a %= b
a &= b    a |= b    a ^= b    a >>= b   a <<= b
```

[i[Atomic variables-->assignments and operators]>]

## Fungsi Library yang Otomatis Menyinkronkan

[i[Atomic variables-->synchronized library functions]<]

Sejauh ini kita sudah membahas bagaimana kamu bisa menyinkronkan dengan
variabel atomic, tapi ternyata ada beberapa fungsi library yang melakukan
sinkronisasi terbatas di balik layar.

``` {.c}
call_once()      thrd_create()       thrd_join()
mtx_lock()       mtx_timedlock()     mtx_trylock()
malloc()         calloc()            realloc()
aligned_alloc()
```

**`call_once()`**---Menyinkronkan dengan semua pemanggilan `call_once()`
berikutnya untuk flag tertentu. Dengan begitu pemanggilan berikutnya bisa
tenang bahwa kalau thread lain mengisi flag, mereka akan melihatnya.

**`thrd_create()`**---Menyinkronkan dengan awal thread baru. Thread baru
bisa yakin akan melihat semua penulisan memori bersama dari parent thread
sebelum pemanggilan `thrd_create()`.

**`thrd_join()`**---Ketika sebuah thread mati, ia menyinkronkan dengan
fungsi ini. Thread yang memanggil `thrd_join()` bisa yakin bahwa ia
melihat semua penulisan bersama dari thread yang telah berpulang itu.

**`mtx_lock()`**---Pemanggilan `mtx_unlock()` sebelumnya pada mutex yang
sama menyinkronkan dengan pemanggilan ini. Inilah kasus yang paling
mencerminkan proses acquire/release yang sudah kita bahas.
`mtx_unlock()` melakukan release pada variabel mutex, menjamin thread
berikutnya yang melakukan acquire dengan `mtx_lock()` bisa melihat semua
perubahan memori bersama di critical section.

**`mtx_timedlock()`** dan **`mtx_trylock()`**---Mirip situasi
`mtx_lock()`, kalau pemanggilan ini berhasil, pemanggilan `mtx_unlock()`
sebelumnya menyinkronkan dengannya.

**Fungsi Memori Dinamis**: kalau kamu mengalokasikan memori, ia
menyinkronkan dengan dealokasi sebelumnya dari memori yang sama. Dan
alokasi serta dealokasi wilayah memori tertentu itu terjadi dalam satu
urutan total yang disepakati semua thread. Aku _kira_ idenya, dealokasi
boleh menghapus wilayah itu kalau mau, dan kita ingin yakin bahwa alokasi
berikutnya tidak melihat data yang belum dihapus. Kabari aku kalau ada
yang lebih dari itu.

[i[Atomic variables-->synchronized library functions]>]

## Type Specifier dan Qualifier Atomic

Mari kita turunkan sedikit tingkatnya dan lihat tipe apa yang tersedia,
dan bagaimana kita bahkan bisa membuat tipe atomic baru.

[i[`_Atomic` type qualifier]<]

Pertama, mari kita lihat tipe atomic bawaan dan di-`typedef` ke apa
mereka. (Bocoran: `_Atomic` adalah type qualifier!)

[i[`atomic_bool` type]]
[i[`atomic_char` type]]
[i[`atomic_schar` type]]
[i[`atomic_uchar` type]]
[i[`atomic_short` type]]
[i[`atomic_ushort` type]]
[i[`atomic_int` type]]
[i[`atomic_uint` type]]
[i[`atomic_long` type]]
[i[`atomic_ulong` type]]
[i[`atomic_llong` type]]
[i[`atomic_ullong` type]]
[i[`atomic_char16_t` type]]
[i[`atomic_char32_t` type]]
[i[`atomic_wchar_t` type]]
[i[`atomic_int_least8_t` type]]
[i[`atomic_uint_least8_t` type]]
[i[`atomic_int_least16_t` type]]
[i[`atomic_uint_least16_t` type]]
[i[`atomic_int_least32_t` type]]
[i[`atomic_uint_least32_t` type]]
[i[`atomic_int_least64_t` type]]
[i[`atomic_uint_least64_t` type]]
[i[`atomic_int_fast8_t` type]]
[i[`atomic_uint_fast8_t` type]]
[i[`atomic_int_fast16_t` type]]
[i[`atomic_uint_fast16_t` type]]
[i[`atomic_int_fast32_t` type]]
[i[`atomic_uint_fast32_t` type]]
[i[`atomic_int_fast64_t` type]]
[i[`atomic_uint_fast64_t` type]]
[i[`atomic_intptr_t` type]]
[i[`atomic_uintptr_t` type]]
[i[`atomic_size_t` type]]
[i[`atomic_ptrdiff_t` type]]
[i[`atomic_intmax_t` type]]
[i[`atomic_uintmax_t` type]]

|Tipe atomic|Padanan bentuk panjang|
|-|-|
|`atomic_bool`|`_Atomic _Bool`|
|`atomic_char`|`_Atomic char`|
|`atomic_schar`|`_Atomic signed char`|
|`atomic_uchar`|`_Atomic unsigned char`|
|`atomic_short`|`_Atomic short`|
|`atomic_ushort`|`_Atomic unsigned short`|
|`atomic_int`|`_Atomic int`|
|`atomic_uint`|`_Atomic unsigned int`|
|`atomic_long`|`_Atomic long`|
|`atomic_ulong`|`_Atomic unsigned long`|
|`atomic_llong`|`_Atomic long long`|
|`atomic_ullong`|`_Atomic unsigned long long`|
|`atomic_char16_t`|`_Atomic char16_t`|
|`atomic_char32_t`|`_Atomic char32_t`|
|`atomic_wchar_t`|`_Atomic wchar_t`|
|`atomic_int_least8_t`|`_Atomic int_least8_t`|
|`atomic_uint_least8_t`|`_Atomic uint_least8_t`|
|`atomic_int_least16_t`|`_Atomic int_least16_t`|
|`atomic_uint_least16_t`|`_Atomic uint_least16_t`|
|`atomic_int_least32_t`|`_Atomic int_least32_t`|
|`atomic_uint_least32_t`|`_Atomic uint_least32_t`|
|`atomic_int_least64_t`|`_Atomic int_least64_t`|
|`atomic_uint_least64_t`|`_Atomic uint_least64_t`|
|`atomic_int_fast8_t`|`_Atomic int_fast8_t`|
|`atomic_uint_fast8_t`|`_Atomic uint_fast8_t`|
|`atomic_int_fast16_t`|`_Atomic int_fast16_t`|
|`atomic_uint_fast16_t`|`_Atomic uint_fast16_t`|
|`atomic_int_fast32_t`|`_Atomic int_fast32_t`|
|`atomic_uint_fast32_t`|`_Atomic uint_fast32_t`|
|`atomic_int_fast64_t`|`_Atomic int_fast64_t`|
|`atomic_uint_fast64_t`|`_Atomic uint_fast64_t`|
|`atomic_intptr_t`|`_Atomic intptr_t`|
|`atomic_uintptr_t`|`_Atomic uintptr_t`|
|`atomic_size_t`|`_Atomic size_t`|
|`atomic_ptrdiff_t`|`_Atomic ptrdiff_t`|
|`atomic_intmax_t`|`_Atomic intmax_t`|
|`atomic_uintmax_t`|`_Atomic uintmax_t`|

[i[`_Atomic` type qualifier]>]

Pakai saja sesukamu! Tipe-tipe ini konsisten dengan alias atomic di C++,
kalau itu membantu.

Tapi bagaimana kalau kamu ingin lebih?

Kamu bisa melakukannya dengan type qualifier atau type specifier.

[i[`_Atomic` type specifier]<]

Pertama, specifier! Ia adalah kata kunci `_Atomic` dengan tipe dalam
kurung setelahnya^[Tampaknya C++23 menambahkan ini sebagai makro.]---cocok
dipakai dengan `typedef`:

``` {.c}
typedef _Atomic(double) atomic_double;

atomic_double f;
```

Batasan pada specifier: tipe yang dijadikan atomic tidak boleh bertipe
array atau fungsi, dan tidak boleh atomic atau sudah ber-qualifier lain.

[i[`_Atomic` type specifier]>]
[i[`_Atomic` type qualifier]<]

Berikutnya, qualifier! Ia adalah kata kunci `_Atomic` _tanpa_ tipe dalam
kurung.

Jadi keduanya melakukan hal serupa^[Spesifikasi mencatat bahwa keduanya
mungkin berbeda dalam ukuran, representasi, dan alignment.]:

``` {.c}
_Atomic(int) i;   // type specifier
_Atomic int  j;   // type qualifier
```

Bedanya, kamu bisa menyertakan type qualifier lain pada yang terakhir:

``` {.c}
_Atomic volatile int k;   // variabel atomic ber-qualifier
```

Batasan pada qualifier: tipe yang dijadikan atomic tidak boleh bertipe
array atau fungsi.

[i[`_Atomic` type qualifier]>]

## Variabel Atomic Lock-Free {#lock-free-atomic}

[i[Atomic variables-->lock-free]<]

Arsitektur hardware terbatas dalam jumlah data yang bisa dibaca dan
ditulis secara atomic. Itu tergantung bagaimana ia dirakit. Dan
bervariasi.

Kalau kamu memakai tipe atomic, kamu bisa yakin bahwa akses ke tipe itu
akan atomic... tapi ada syaratnya: kalau hardware tidak bisa melakukannya,
ia dilakukan dengan lock sebagai gantinya.

Jadi akses atomic menjadi lock-akses-unlock, yang cukup lebih lambat dan
punya implikasi untuk signal handler.

[Atomic flag](#atomic-flags), di bawah, adalah satu-satunya tipe atomic
yang dijamin lock-free di semua implementasi yang patuh. Di dunia komputer
desktop/laptop biasa, tipe lain yang lebih besar kemungkinan lock-free.

Untungnya, ada beberapa cara untuk menentukan apakah tipe tertentu adalah
atomic lock-free atau bukan.

Pertama, beberapa makro---ini bisa dipakai saat compile time dengan `#if`.
Berlaku untuk tipe signed maupun unsigned.

[i[`ATOMIC_BOOL_LOCK_FREE` macro]]
[i[`ATOMIC_CHAR_LOCK_FREE` macro]]
[i[`ATOMIC_CHAR16_T_LOCK_FREE` macro]]
[i[`ATOMIC_CHAR32_T_LOCK_FREE` macro]]
[i[`ATOMIC_WCHAR_T_LOCK_FREE` macro]]
[i[`ATOMIC_SHORT_LOCK_FREE` macro]]
[i[`ATOMIC_INT_LOCK_FREE` macro]]
[i[`ATOMIC_LONG_LOCK_FREE` macro]]
[i[`ATOMIC_LLONG_LOCK_FREE` macro]]
[i[`ATOMIC_POINTER_LOCK_FREE` macro]]

|Tipe Atomic|Makro Lock Free|
|-|-|
|`atomic_bool`|`ATOMIC_BOOL_LOCK_FREE`|
|`atomic_char`|`ATOMIC_CHAR_LOCK_FREE`|
|`atomic_char16_t`|`ATOMIC_CHAR16_T_LOCK_FREE`|
|`atomic_char32_t`|`ATOMIC_CHAR32_T_LOCK_FREE`|
|`atomic_wchar_t`|`ATOMIC_WCHAR_T_LOCK_FREE`|
|`atomic_short`|`ATOMIC_SHORT_LOCK_FREE`|
|`atomic_int`|`ATOMIC_INT_LOCK_FREE`|
|`atomic_long`|`ATOMIC_LONG_LOCK_FREE`|
|`atomic_llong`|`ATOMIC_LLONG_LOCK_FREE`|
|`atomic_intptr_t`|`ATOMIC_POINTER_LOCK_FREE`|

Makro-makro ini menariknya bisa punya _tiga_ nilai berbeda:

|Nilai|Arti|
|-|-|
|`0`|Tidak pernah lock-free.|
|`1`|_Kadang-kadang_ lock-free.|
|`2`|Selalu lock-free.|

Tunggu---bagaimana sesuatu bisa _kadang-kadang_ lock-free? Ini hanya
berarti jawabannya tidak diketahui saat compile time, tapi bisa diketahui
nanti saat runtime. Mungkin jawabannya bervariasi tergantung apakah kamu
menjalankan kode ini di Genuine Intel atau AMD, atau semacam itu^[Aku mengarang
contoh itu begitu saja. Mungkin tidak berpengaruh di Intel/AMD, tapi bisa
berpengaruh di suatu tempat, astaga!].

Tapi kamu selalu bisa menguji saat runtime dengan fungsi
[i[`atomic_is_lock_free()` function]] `atomic_is_lock_free()`. Fungsi ini
mengembalikan true atau false apakah tipe tertentu atomic saat ini.

Jadi kenapa kita peduli?

Lock-free lebih cepat, jadi mungkin ada kekhawatiran kecepatan yang akan
kamu atasi dengan cara lain. Atau mungkin kamu perlu memakai variabel
atomic di signal handler.

[i[Atomic variables-->lock-free]>]

### Signal Handler dan Atomic Lock-Free

[i[Signal handlers-->with lock-free atomics]<]
[i[Atomic variables-->with signal handlers]<]

Kalau kamu membaca atau menulis variabel bersama (static storage duration
atau `_Thread_Local`) di signal handler, itu undefined behavior [astaga!]...
Kecuali kamu melakukan salah satu dari berikut:

1. Menulis ke variabel bertipe `volatile sig_atomic_t`.

2. Membaca atau menulis variabel atomic lock-free.

Sejauh yang kutahu, variabel atomic lock-free adalah salah satu dari
sedikit cara untuk mengambil informasi dari signal handler secara
portabel.

Menurut bacaanku, spesifikasi agak samar soal memory order saat
meng-acquire atau me-release variabel atomic di signal handler. C++
menyatakan, dan itu masuk akal, bahwa akses semacam itu unsequenced
terhadap sisa program^[C++ menjelaskan bahwa kalau signal adalah hasil
pemanggilan [i[`raise()` function]] `raise()`, ia disekuensikan _setelah_
`raise()`.]. Bagaimanapun, signal bisa di-raise kapan saja. Jadi aku
menganggap perilaku C serupa.

[i[Signal handlers-->with lock-free atomics]>]
[i[Atomic variables-->with signal handlers]>]

## Atomic Flag {#atomic-flags}

[i[Atomic variables-->atomic flags]<]
[i[`atomic_flag` type]<]

Hanya ada satu tipe yang dijamin standar sebagai atomic lock-free:
`atomic_flag`. Ini tipe opaque untuk operasi
[flw[test-and-set|Test-and-set]].

Ia bisa _set_ atau _clear_. Kamu bisa menginisialisasinya menjadi clear
dengan:

[i[`ATOMIC_FLAG_INIT` macro]<]

``` {.c}
atomic_flag f = ATOMIC_FLAG_INIT;
```

[i[`ATOMIC_FLAG_INIT` macro]>]

[i[`atomic_flag_test_and_set()` function]<]

Kamu bisa men-set flag secara atomic dengan `atomic_flag_test_and_set()`,
yang akan men-set flag dan mengembalikan status sebelumnya sebagai
`_Bool` (true untuk set).

[i[`atomic_flag_clear()` function]<]

Kamu bisa meng-clear flag secara atomic dengan `atomic_flag_clear()`.

Berikut contoh ketika kita menginisialisasi flag menjadi clear,
men-set-nya dua kali, lalu meng-clear-nya lagi.

``` {.c}
#include <stdio.h>
#include <stdbool.h>   // Tidak diperlukan di C23
#include <stdatomic.h>

atomic_flag f = ATOMIC_FLAG_INIT;

int main(void)
{
    bool r = atomic_flag_test_and_set(&f);
    printf("Value was: %d\n", r);           // 0

    r = atomic_flag_test_and_set(&f);
    printf("Value was: %d\n", r);           // 1

    atomic_flag_clear(&f);
    r = atomic_flag_test_and_set(&f);
    printf("Value was: %d\n", r);           // 0
}
```

[i[`atomic_flag_clear()` function]>]
[i[`atomic_flag_test_and_set()` function]>]
[i[Atomic variables-->atomic flags]>]
[i[`atomic_flag` type]>]

## `struct` dan `union` Atomic

[i[Atomic variables-->`struct` and `union`]<]

Dengan qualifier atau specifier `_Atomic`, kamu bisa membuat `struct` atau
`union` atomic! Cukup mencengangkan.

Kalau tidak banyak data di dalamnya (yaitu segenggam byte), tipe atomic
yang dihasilkan mungkin lock-free. Ujilah dengan
`atomic_is_lock_free()`.

``` {.c .numberLines}
#include <stdio.h>
#include <stdatomic.h>

int main(void)
{
    struct point {
        float x, y;
    };

    _Atomic(struct point) p;

    printf("Is lock free: %d\n", atomic_is_lock_free(&p));
}
```

Ini syaratnya: kamu tidak bisa mengakses field dari `struct` atau `union`
atomic... lalu apa gunanya? Nah, kamu bisa _menyalin_ seluruh `struct`
secara atomic ke variabel non-atomic lalu memakainya. Kamu juga bisa
menyalin secara atomic ke arah sebaliknya.

``` {.c .numberLines}
#include <stdio.h>
#include <stdatomic.h>

int main(void)
{
    struct point {
        float x, y;
    };

    _Atomic(struct point) p;
    struct point t;

    p = (struct point){1, 2};  // Salinan atomic

    //printf("%f\n", p.x);  // Error

    t = p;   // Salinan atomic

    printf("%f\n", t.x);  // OK!
}
```

Kamu juga bisa mendeklarasikan `struct` yang field-field individualnya
atomic. Apakah tipe atomic diizinkan pada bitfield ditentukan oleh
implementasi.

[i[Atomic variables-->`struct` and `union`]>]

## Pointer Atomic

[i[Atomic variables-->pointers]<]

Sekadar catatan di sini tentang penempatan `_Atomic` untuk pointer.

Pertama, pointer ke atomic (yaitu nilai pointer-nya tidak atomic, tapi yang
ditunjuknya atomic):

``` {.c}
_Atomic int x;
_Atomic int *p;  // p adalah pointer ke int atomic

p = &x;  // OK!
```

Kedua, pointer atomic ke nilai non-atomic (yaitu nilai pointer itu sendiri
atomic, tapi yang ditunjuknya tidak):

``` {.c}
int x;
int * _Atomic p;  // p adalah pointer atomic ke int

p = &x;  // OK!
```

Terakhir, pointer atomic ke nilai atomic (yaitu pointer dan yang
ditunjuknya sama-sama atomic):

``` {.c}
_Atomic int x;
_Atomic int * _Atomic p;  // p adalah pointer atomic ke int atomic

p = &x;  // OK!
```

[i[Atomic variables-->pointers]>]

## Memory Order

[i[Atomic variables-->memory order]<]
[i[Memory order]<]

Kita sudah membahas sequential consistency, yang paling masuk akal di
antara semuanya. Tapi ada beberapa yang lain:

[i[`memory_order_seq_cst` enumerated type]]
[i[`memory_order_acq_rel` enumerated type]]
[i[`memory_order_release` enumerated type]]
[i[`memory_order_acquire` enumerated type]]
[i[`memory_order_consume` enumerated type]]
[i[`memory_order_relaxed` enumerated type]]

|`memory_order`|Deskripsi|
|-|-|
|`memory_order_seq_cst`|Sequential Consistency|
|`memory_order_acq_rel`|Acquire/Release|
|`memory_order_release`|Release|
|`memory_order_acquire`|Acquire|
|`memory_order_consume`|Consume|
|`memory_order_relaxed`|Relaxed|

Kamu bisa menentukan yang lain dengan fungsi library tertentu. Misalnya,
kamu bisa menambahkan nilai ke variabel atomic seperti ini:

``` {.c}
atomic_int x = 0;

x += 5;  // Sequential consistency, yang bawaan
```

Atau kamu bisa melakukan hal yang sama dengan fungsi library ini:

[i[`atomic_fetch_add()` function]<]

``` {.c}
atomic_int x = 0;

atomic_fetch_add(&x, 5);  // Sequential consistency, yang bawaan
```
[i[`atomic_fetch_add()` function]>]

Atau kamu bisa melakukan hal yang sama dengan memory ordering eksplisit:

[i[`atomic_fetch_add_explicit()` function]<]

``` {.c}
atomic_int x = 0;

atomic_fetch_add_explicit(&x, 5, memory_order_seq_cst);
```

Tapi bagaimana kalau kita tidak menginginkan sequential consistency? Dan
kamu menginginkan acquire/release karena suatu alasan? Sebutkan saja:

``` {.c}
atomic_int x = 0;

atomic_fetch_add_explicit(&x, 5, memory_order_acq_rel);
```

[i[`atomic_fetch_add_explicit()` function]>]

Kita akan membahas rincian memory order yang berbeda di bawah. Jangan
mengutak-atik apa pun selain sequential consistency kecuali kamu tahu apa
yang kamu lakukan. Sangat mudah membuat kesalahan yang menyebabkan
kegagalan langka yang sulit direproduksi.

### Sequential Consistency

[i[Atomic variables-->sequential consistency]<]
[i[Memory order-->sequential consistency]<]

* Operasi load melakukan acquire (lihat di bawah).
* Operasi store melakukan release (lihat di bawah).
* Operasi read-modify-write melakukan acquire lalu release.

Selain itu, untuk menjaga urutan total acquire dan release, tidak ada
acquire atau release yang akan ditata ulang terhadap satu sama lain.
(Aturan acquire/release tidak melarang penataan ulang release yang diikuti
acquire. Tapi aturan sequentially consistent melarangnya.)

[i[Memory order-->sequential consistency]>]
[i[Atomic variables-->sequential consistency]>]

### Acquire

[i[Atomic variables-->acquire]<]
[i[Memory order-->acquire]<]

Inilah yang terjadi pada operasi load/read variabel atomic.

* Kalau thread lain me-release variabel atomic ini, semua penulisan yang
  dilakukan thread itu kini terlihat di thread ini.

* Akses memori di thread ini yang terjadi setelah load ini tidak bisa
  ditata ulang ke sebelumnya.

[i[Memory order-->acquire]>]
[i[Atomic variables-->acquire]>]

### Release

[i[Atomic variables-->release]<]
[i[Memory order-->acquire]<]

Inilah yang terjadi pada store/write variabel atomic.

* Kalau thread lain nanti meng-acquire variabel atomic ini, semua
  penulisan memori di thread ini sebelum penulisan atomic-nya menjadi
  terlihat oleh thread lain itu.

* Akses memori di thread ini yang terjadi sebelum release tidak bisa
  ditata ulang ke setelahnya.

[i[Atomic variables-->release]>]
[i[Memory order-->release]>]

### Consume

[i[Atomic variables-->consume]<]
[i[Memory order-->consume]<]

Yang ini aneh, mirip versi acquire yang kurang ketat. Ia memengaruhi akses
memori yang _bergantung data_ pada variabel atomic.

Menjadi "bergantung data" secara samar berarti variabel atomic dipakai
dalam suatu perhitungan.

Artinya, kalau sebuah thread meng-consume variabel atomic, maka semua
operasi di thread itu yang kemudian memakai variabel atomic tersebut akan
bisa melihat penulisan memori di thread yang melakukan release.

Bandingkan dengan acquire, ketika penulisan memori di thread yang
melakukan release akan terlihat oleh _semua_ operasi di thread saat ini,
bukan hanya yang bergantung data.

Seperti acquire juga, ada batasan operasi mana yang boleh ditata ulang
_sebelum_ consume. Dengan acquire, tidak ada yang boleh ditata ulang ke
sebelumnya. Dengan consume, tidak ada yang bergantung pada nilai atomic
yang dimuat yang boleh ditata ulang ke sebelumnya.

[i[Atomic variables-->consume]>]
[i[Memory order-->consume]>]

### Acquire/Release

[i[Atomic variables-->acquire/release]<]
[i[Memory order-->acquire/release]<]

Ini hanya berlaku untuk operasi read-modify-write. Ia acquire dan release
yang digabung menjadi satu.

* Acquire terjadi untuk pembacaan.
* Release terjadi untuk penulisan.

[i[Atomic variables-->acquire/release]>]
[i[Memory order-->acquire/release]>]

### Relaxed

[i[Atomic variables-->relaxed]<]
[i[Memory order-->relaxed]<]

Tidak ada aturan; ini anarki! Semua orang boleh menata ulang semuanya di
mana saja! Anjing dan kucing hidup bersama---histeria massal!

Sebenarnya ada satu aturan. Pembacaan dan penulisan atomic tetap
semua-atau-tidak-sama-sekali. Tapi operasi bisa ditata ulang sesuka hati
dan sama sekali tidak ada sinkronisasi antar thread.

Ada beberapa kasus pemakaian untuk memory order ini, yang bisa kamu temukan
dengan sedikit pencarian, mis. counter sederhana.

Dan kamu bisa memakai fence untuk memaksa sinkronisasi setelah sekumpulan
penulisan relaxed.

[i[Atomic variables-->relaxed]>]
[i[Memory order-->relaxed]>]
[i[Memory order]>]
[i[Atomic variables-->memory order]>]

## Fence

[i[Atomic variables-->fences]<]

Kamu tahu bagaimana release dan acquire variabel atomic terjadi saat kamu
membaca dan menulisnya?

Nah, mungkin juga melakukan release atau acquire _tanpa_ variabel atomic.

Ini disebut _fence_. Jadi kalau kamu ingin semua penulisan di sebuah thread
terlihat di tempat lain, kamu bisa memasang release fence di satu thread
dan acquire fence di thread lain, persis seperti cara kerja variabel
atomic.

Karena operasi consume tidak benar-benar masuk akal pada fence^[Karena
consume sepenuhnya tentang operasi yang bergantung pada nilai variabel
atomic yang di-acquire, dan tidak ada variabel atomic pada fence.],
`memory_order_consume` diperlakukan sebagai acquire.

Kamu bisa memasang fence dengan order apa pun yang ditentukan:

[i[`atomic_thread_fence()` function]<]

``` {.c}
atomic_thread_fence(memory_order_release);
```

[i[`atomic_thread_fence()` function]>]
[i[`atomic_signal_fence()` function]<]

Ada juga versi ringan fence untuk dipakai dengan signal handler, bernama
`atomic_signal_fence()`.

Cara kerjanya sama persis dengan `atomic_thread_fence()`, kecuali:

* Ia hanya menangani visibilitas nilai dalam thread yang sama; tidak ada
  sinkronisasi dengan thread lain.

* Tidak ada instruksi fence hardware yang dikeluarkan.

Kalau kamu ingin yakin efek samping operasi non-atomic (dan operasi atomic
relaxed) terlihat di signal handler, kamu bisa memakai fence ini.

Idenya: signal handler dieksekusi di thread _ini_, bukan thread lain, jadi
ini cara yang lebih ringan untuk memastikan perubahan di luar signal
handler terlihat di dalamnya (yaitu belum ditata ulang).

[i[`atomic_signal_fence()` function]>]
[i[Atomic variables-->fences]>]

## Referensi

Kalau kamu ingin mempelajari hal-hal ini lebih jauh, berikut beberapa
bahan yang membantuku menembusnya:

* Ceramah Herb Sutter _`atomic<>` Weapons_:
  * [fl[Bagian 1|https://www.youtube.com/watch?v=A8eCGOqgvH4]]
  * [fl[Bagian 2|https://www.youtube.com/watch?v=KeLBd2EJLOU]]

* [fl[Materi Jeff Preshing|https://preshing.com/archives/]], khususnya:
  * [fl[An Introduction to Lock-Free Programming|https://preshing.com/20120612/an-introduction-to-lock-free-programming/]]
  * [fl[Acquire and Release Semantics|https://preshing.com/20120913/acquire-and-release-semantics/]]
  * [fl[The _Happens-Before_ Relation|https://preshing.com/20130702/the-happens-before-relation/]]
  * [fl[The _Synchronizes-With_ Relation|https://preshing.com/20130823/the-synchronizes-with-relation/]]
  * [fl[The Purpose of `memory_order_consume` in C++11|https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/]]
  * [fl[You Can Do Any Kind of Atomic Read-Modify-Write Operation|https://preshing.com/20150402/you-can-do-any-kind-of-atomic-read-modify-write-operation/]]

* CPPReference:
  * [fl[Memory Order|https://en.cppreference.com/w/c/atomic/memory_order]]
  * [fl[Atomic Types|https://en.cppreference.com/w/c/language/atomic]]

* [fl[Lockless Programming Considerations|https://docs.microsoft.com/en-us/windows/win32/dxtecharts/lockless-programming]] oleh Bruce Dawson

* Orang-orang yang suka membantu dan berpengetahuan di
  [fl[r/C_Programming|https://www.reddit.com/r/C_Programming/]]

[i[Atomic variables]>]
