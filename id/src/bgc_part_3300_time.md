<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsionalitas Tanggal dan Waktu

> "Time is an illusion. Lunchtime doubly so." \
> ---Ford Prefect, The Hitchhikers Guide to the Galaxy

[i[Date and time]<]

Ini tidak terlalu rumit, tapi bisa sedikit mengintimidasi pada awalnya,
baik karena banyaknya tipe yang tersedia maupun cara kita mengonversi
antar tipe itu.

Campurkan GMT (UTC) dan waktu lokal, maka kita dapat semua _Keseruan
Biasa_™ yang selalu ada pada waktu dan tanggal.

Dan tentu saja jangan pernah lupa aturan emas tanggal dan waktu: _Jangan
pernah mencoba menulis fungsionalitas tanggal dan waktu sendiri. Pakai
saja yang disediakan library._

Waktu terlalu rumit untuk ditangani dengan benar oleh programmer manusia
biasa. Serius, kita semua berutang satu poin kepada setiap orang yang
mengerjakan library tanggal dan waktu mana pun, jadi masukkan itu ke
anggaranmu.

## Terminologi dan Informasi Singkat

Beberapa istilah singkat kalau-kalau kamu belum hafal.

* [i[Universal Coordinated Time]] **UTC**: Coordinated Universal Time
  adalah waktu absolut yang disepakati secara universal^[Di Bumi, setidaknya.
  Siapa tahu sistem gila apa yang dipakai _di luar sana_...]. Semua orang
  di planet ini menganggap sekarang waktunya sama di UTC... meskipun
  waktu lokal mereka berbeda.

* [i[Greenwich Mean time]] **GMT**: Greenwich Mean Time, secara efektif
  sama dengan UTC^[Oke, jangan hajar aku! GMT secara teknis adalah zona
  waktu, sedangkan UTC adalah sistem waktu global. Selain itu beberapa
  negara mungkin menyesuaikan GMT untuk daylight saving time, sedangkan UTC
  tidak pernah disesuaikan untuk daylight saving time.]. Kemungkinan kamu
  ingin mengatakan UTC, atau "universal time". Kalau kamu membicarakan
  zona waktu GMT secara khusus, sebut GMT. Membingungkan, banyak fungsi UTC
  di C lebih tua daripada UTC dan masih merujuk ke Greenwich Mean Time.
  Saat melihat itu, ketahuilah bahwa maksud C adalah UTC.

* [i[Local time]] **Waktu lokal**: jam berapa sekarang di tempat komputer
  yang menjalankan program berada. Ini dinyatakan sebagai selisih dari UTC.
  Walaupun ada banyak zona waktu di dunia, kebanyakan komputer bekerja
  dalam waktu lokal atau UTC.

[i[Universal Coordinated Time]<]

Sebagai aturan umum, kalau kamu menggambarkan kejadian yang terjadi satu
kali, seperti entri log, atau peluncuran roket, atau saat pointer akhirnya
klik bagimu, pakai UTC.

[i[Local time]<]

Sebaliknya, kalau itu sesuatu yang terjadi pada waktu yang sama _di setiap
zona waktu_, seperti malam Tahun Baru atau jam makan malam, pakai waktu
lokal.

Karena banyak bahasa hanya pandai mengonversi antara UTC dan waktu lokal,
kamu bisa menyakiti dirimu sendiri dengan memilih menyimpan tanggalmu dalam
bentuk yang salah. (Tanya aku bagaimana aku tahu.)

[i[Local time]>]
[i[Universal Coordinated Time]>]


## Tipe Tanggal

Ada dua^[Harus diakui, ada lebih dari dua.] tipe utama di C untuk tanggal:
`time_t` dan `struct tm`.

Spesifikasi sebenarnya tidak banyak bicara soal keduanya:

* [i[`time_t` type]] `time_t`: tipe real yang mampu menampung waktu. Jadi
  menurut spesifikasi, ini bisa berupa tipe floating atau integer. Di POSIX
  (sistem mirip Unix), ia integer. Ia menyimpan _calendar time_. Yang bisa
  dianggap sebagai waktu UTC.

* [i[`struct tm` type]] `struct tm`: menyimpan komponen sebuah calendar
  time. Ini _broken-down time_, yaitu komponen-komponen waktu, seperti jam,
  menit, detik, hari, bulan, tahun, dan sebagainya.

[i[`time_t` type]<]

Di banyak sistem, `time_t` mewakili jumlah detik sejak
[flw[_Epoch_|Unix_time]]. Epoch dalam beberapa hal adalah awal waktu dari
sudut pandang komputer, yang umumnya 1 Januari 1970 UTC.  `time_t` bisa
negatif untuk mewakili waktu sebelum Epoch. Windows berperilaku sama
dengan Unix sejauh yang kutahu.

[i[`time_t` type]>]
[i[`struct tm` type]<]

Dan apa isi `struct tm`? Field-field berikut:

``` {.c}
struct tm {
    int tm_sec;    // detik setelah menit -- [0, 60]
    int tm_min;    // menit setelah jam -- [0, 59]
    int tm_hour;   // jam sejak tengah malam -- [0, 23]
    int tm_mday;   // hari dalam bulan -- [1, 31]
    int tm_mon;    // bulan sejak Januari -- [0, 11]
    int tm_year;   // tahun sejak 1900
    int tm_wday;   // hari sejak Minggu -- [0, 6]
    int tm_yday;   // hari sejak 1 Januari -- [0, 365]
    int tm_isdst;  // Flag Daylight Saving Time
};
```

Perhatikan bahwa semuanya berbasis nol kecuali hari dalam bulan.

Penting diketahui bahwa kamu bisa memasukkan nilai apa pun yang kamu mau
ke tipe-tipe ini. Ada fungsi untuk membantu mengambil waktu _sekarang_,
tapi tipe-tipe itu menyimpan _suatu_ waktu, bukan _satu-satunya_ waktu.

[i[`struct tm` type]>]

Jadi pertanyaannya menjadi: "Bagaimana menginisialisasi data bertipe ini,
dan bagaimana mengonversi di antara mereka?"

## Inisialisasi dan Konversi Antartipe

[i[`time()` function]<]

Pertama, kamu bisa mengambil waktu saat ini dan menyimpannya dalam
`time_t` dengan fungsi `time()`.

``` {.c}
time_t now;  // Variabel untuk menampung waktu sekarang

now = time(NULL);  // Kamu bisa mengambilnya seperti ini...

time(&now);        // ...atau ini. Sama saja dengan baris sebelumnya.
```

Bagus! Kamu punya variabel yang menyimpan waktu sekarang.

[i[`time()` function]>]
[i[`ctime()` function]<]

Lucunya, hanya ada satu cara portabel untuk mencetak isi `time_t`, yaitu
fungsi `ctime()` yang jarang dipakai, yang mencetak nilai dalam waktu
lokal:


``` {.c}
now = time(NULL);
printf("%s", ctime(&now));
```

Ini mengembalikan string dengan bentuk yang sangat spesifik, termasuk
newline di akhir:

``` {.default}
Sun Feb 28 18:47:25 2021
```

[i[`ctime()` function]>]

Jadi itu agak kaku. Kalau ingin kendali lebih besar, kamu harus
mengonversi `time_t` itu menjadi `struct tm`.

### Mengonversi `time_t` ke `struct tm`

[i[`time_t` type-->conversion to `struct tm`]<]

Ada dua cara luar biasa untuk melakukan konversi ini:

[i[`localtime()` function]<]

* `localtime()`: fungsi ini mengonversi `time_t` menjadi `struct tm`
  dalam waktu lokal.

[i[`gmtime()` function]<]

* `gmtime()`: fungsi ini mengonversi `time_t` menjadi `struct tm` dalam
  UTC. (Lihat GMT kuno menyelinap ke nama fungsi ini?)

[i[`asctime()` function]<]

Mari kita lihat jam berapa sekarang dengan mencetak `struct tm` memakai
fungsi `asctime()`:

``` {.c}
printf("Local: %s", asctime(localtime(&now)));
printf("  UTC: %s", asctime(gmtime(&now)));
```

[i[`asctime()` function]>]
[i[`localtime()` function]>]
[i[`gmtime()` function]>]

Output (aku berada di zona waktu Pacific Standard Time):

``` {.default}
Local: Sun Feb 28 20:15:27 2021
  UTC: Mon Mar  1 04:15:27 2021
```

Begitu `time_t`-mu ada di `struct tm`, banyak pintu terbuka. Kamu bisa
mencetak waktu dengan berbagai cara, mencari tahu hari apa dalam seminggu
sebuah tanggal, dan sebagainya. Atau mengonversinya kembali menjadi
`time_t`.

Lebih lanjut soal itu sebentar lagi!

[i[`time_t` type-->conversion to `struct tm`]>]

### Mengonversi `struct tm` ke `time_t`

[i[`struct tm` type-->conversion to `time_t`]<]
[i[`mktime()` function]<]

Kalau ingin ke arah sebaliknya, kamu bisa memakai `mktime()` untuk
mendapatkan informasi itu.

`mktime()` mengatur nilai `tm_wday` dan `tm_yday` untukmu, jadi tidak perlu
repot mengisinya karena akan tertimpa.

Selain itu, kamu bisa mengatur `tm_isdst` menjadi `-1` agar ia yang
menentukannya. Atau mengaturnya secara manual menjadi true atau false.

``` {.c}
// Jangan tergoda menaruh nol di depan angka-angka ini (kecuali kamu
// memang bermaksud memakai oktal)!

struct tm some_time = {
    .tm_year=82,   // tahun sejak 1900
    .tm_mon=3,     // bulan sejak Januari -- [0, 11]
    .tm_mday=12,   // hari dalam bulan -- [1, 31]
    .tm_hour=12,   // jam sejak tengah malam -- [0, 23]
    .tm_min=0,     // menit setelah jam -- [0, 59]
    .tm_sec=4,     // detik setelah menit -- [0, 60]
    .tm_isdst=-1,  // Flag Daylight Saving Time
};

time_t some_time_epoch;

some_time_epoch = mktime(&some_time);

printf("%s", ctime(&some_time_epoch));
printf("Is DST: %d\n", some_time.tm_isdst);
```

Output:

``` {.default}
Mon Apr 12 12:00:04 1982
Is DST: 0
```

Saat memuat `struct tm` secara manual seperti itu, nilainya seharusnya
dalam waktu lokal. `mktime()` akan mengonversi waktu lokal itu menjadi
calendar time `time_t`.

[i[`mktime()` function]>]

Anehnya, standar tidak memberi kita cara untuk memuat `struct tm` dengan
waktu UTC lalu mengonversinya menjadi `time_t`. Kalau ingin melakukannya di
sistem mirip Unix, coba [i[`timegm()` Unix function]] `timegm()` yang
non-standar. Di Windows, [i[`_mkgmtime()` Windows function]]
`_mkgmtime()`.

[i[`struct tm` type-->conversion to `time_t`]>]

## Output Tanggal Terformat

Kita sudah melihat beberapa cara mencetak output tanggal terformat ke
layar. Dengan `time_t` kita bisa memakai `ctime()`, dan dengan `struct tm`
kita bisa memakai `asctime()`.

``` {.c}
time_t now = time(NULL);
struct tm *local = localtime(&now);
struct tm *utc = gmtime(&now);

printf("Local time: %s", ctime(&now));     // Waktu lokal dengan time_t
printf("Local time: %s", asctime(local));  // Waktu lokal dengan struct tm
printf("UTC       : %s", asctime(utc));    // UTC dengan struct tm
```

Tapi bagaimana kalau kukatakan kepadamu, pembaca yang budiman, bahwa ada
cara untuk punya kendali yang jauh lebih besar atas cara tanggal dicetak?

[i[`strftime()` function]<]

Memang kita bisa memancing field individual dari `struct tm`, tapi ada
fungsi hebat bernama `strftime()` yang mengerjakan banyak pekerjaan berat
untukmu. Ia seperti `printf()`, hanya untuk tanggal!

Mari kita lihat beberapa contoh. Pada masing-masing, kita mengoper buffer
tujuan, jumlah karakter maksimum yang ditulis, lalu format string (bergaya
---tapi tidak sama dengan---`printf()`) yang memberi tahu `strftime()`
komponen `struct tm` mana yang dicetak dan bagaimana.

Kamu juga bisa menambahkan karakter konstan lain untuk disertakan dalam
output pada format string, seperti pada `printf()`.

Pada kasus ini kita mendapat `struct tm` dari `localtime()`, tapi sumber
mana pun bisa dipakai.

``` {.c .numberLines}
#include <stdio.h>
#include <time.h>

int main(void)
{
    char s[128];
    time_t now = time(NULL);

    // %c: cetak tanggal sesuai locale saat ini
    strftime(s, sizeof s, "%c", localtime(&now));
    puts(s);   // Sun Feb 28 22:29:00 2021

    // %A: nama lengkap hari
    // %B: nama lengkap bulan
    // %d: hari dalam bulan
    strftime(s, sizeof s, "%A, %B %d", localtime(&now));
    puts(s);   // Sunday, February 28

    // %I: jam (jam 12-an)
    // %M: menit
    // %S: detik
    // %p: AM atau PM
    strftime(s, sizeof s, "It's %I:%M:%S %p", localtime(&now));
    puts(s);   // It's 10:29:00 PM

    // %F: ISO 8601 yyyy-mm-dd
    // %T: ISO 8601 hh:mm:ss
    // %z: selisih zona waktu ISO 8601
    strftime(s, sizeof s, "ISO 8601: %FT%T%z", localtime(&now));
    puts(s);   // ISO 8601: 2021-02-28T22:29:00-0800
}
```

Ada _banyak sekali_ format specifier pencetakan tanggal untuk
`strftime()`, jadi pastikan melihatnya di [fl[halaman referensi
`strftime()`|https://beej.us/guide/bgclr/html/split/time.html#man-strftime]].

[i[`strftime()` function]>]

## Resolusi Lebih Tinggi dengan `timespec_get()`

[i[`timespec_get()` function]<]

Kamu bisa mendapat jumlah detik dan nanodetik sejak Epoch dengan
`timespec_get()`.

Mungkin.

Implementasi mungkin tidak punya resolusi nanodetik (itu sepersemiliar
detik) jadi tidak ada yang tahu berapa banyak tempat signifikan yang kamu
dapat, tapi coba saja dan lihat.

[i[`struct timespec` type]<]

`timespec_get()` menerima dua argumen. Satu adalah pointer ke `struct
timespec` untuk menampung informasi waktu. Yang lain adalah `base`, yang
oleh spesifikasi boleh diatur ke `TIME_UTC` untuk menandakan bahwa kamu
tertarik pada detik sejak Epoch. (Implementasi lain mungkin memberi lebih
banyak pilihan untuk `base`.)

Dan struktur itu sendiri punya dua field:

``` {.c}
struct timespec {
    time_t tv_sec;   // Detik
    long   tv_nsec;  // Nanodetik (sepersemiliar detik)
};
```

Berikut contoh ketika kita mengambil waktu dan mencetaknya sebagai nilai
integer dan juga nilai floating:

``` {.c}
struct timespec ts;

timespec_get(&ts, TIME_UTC);

printf("%ld s, %ld ns\n", ts.tv_sec, ts.tv_nsec);

double float_time = ts.tv_sec + ts.tv_nsec/1000000000.0;
printf("%f seconds since epoch\n", float_time);
```

Contoh output:

``` {.default}
1614581530 s, 806325800 ns
1614581530.806326 seconds since epoch
```

`struct timespec` juga muncul di sejumlah fungsi threading yang perlu bisa
menentukan waktu dengan resolusi itu.

[i[`struct timespec` type]>]
[i[`timespec_get()` function]>]

## Selisih Antarwaktu

[i[Date and time-->differences]<]

Satu catatan singkat soal mengambil selisih antara dua `time_t`: karena
spesifikasi tidak menentukan bagaimana tipe itu merepresentasikan waktu,
kamu mungkin tidak bisa begitu saja mengurangkan dua `time_t` dan
mendapat sesuatu yang masuk akal^[Di POSIX bisa, karena `time_t` jelas
integer. Sayangnya seluruh dunia bukan POSIX, jadi begitulah.].

[i[`difftime()` function]<]

Untungnya kamu bisa memakai `difftime()` untuk menghitung selisih dalam
detik antara dua tanggal.

Pada contoh berikut, kita punya dua kejadian yang terjadi pada waktu yang
berjauhan, dan kita memakai `difftime()` untuk menghitung selisihnya.

``` {.c .numberLines}
#include <stdio.h>
#include <time.h>

int main(void)
{
    struct tm time_a = {
        .tm_year=82,   // tahun sejak 1900
        .tm_mon=3,     // bulan sejak Januari -- [0, 11]
        .tm_mday=12,   // hari dalam bulan -- [1, 31]
        .tm_hour=4,    // jam sejak tengah malam -- [0, 23]
        .tm_min=00,    // menit setelah jam -- [0, 59]
        .tm_sec=04,    // detik setelah menit -- [0, 60]
        .tm_isdst=-1,  // Flag Daylight Saving Time
    };

    struct tm time_b = {
        .tm_year=120,  // tahun sejak 1900
        .tm_mon=10,    // bulan sejak Januari -- [0, 11]
        .tm_mday=15,   // hari dalam bulan -- [1, 31]
        .tm_hour=16,   // jam sejak tengah malam -- [0, 23]
        .tm_min=27,    // menit setelah jam -- [0, 59]
        .tm_sec=00,    // detik setelah menit -- [0, 60]
        .tm_isdst=-1,  // Flag Daylight Saving Time
    };

    time_t cal_a = mktime(&time_a);
    time_t cal_b = mktime(&time_b);

    double diff = difftime(cal_b, cal_a);

    double years = diff / 60 / 60 / 24 / 365.2425;  // cukup dekat

    printf("%f seconds (%f years) between events\n", diff, years);
}
```

Output:

``` {.default}
1217996816.000000 seconds (38.596783 years) between events
```

Nah, begitulah! Ingat untuk memakai `difftime()` guna mengambil selisih
waktu. Walaupun di sistem POSIX kamu bisa langsung mengurangkan, sekalian
saja tetap portabel.

[i[`difftime()` function]>]
[i[Date and time-->differences]>]
[i[Date and time]>]
