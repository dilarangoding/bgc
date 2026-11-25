<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Hello, World!

## Apa yang Diharapkan dari C

> _"Where do these stairs go?"_ \
> _"They go up."_
>
> ---Ray Stantz and Peter Venkman, Ghostbusters

C adalah bahasa low-level.

Dulu tidak begitu. Di zaman ketika orang masih mengukir punch card dari
granit, C adalah cara luar biasa untuk bebas dari kerepotan bahasa
lower-level seperti [flw[assembly|Assembly_language]].

Tapi di era modern ini, bahasa-bahasa generasi sekarang menawarkan berbagai
fitur yang belum ada pada 1972, ketika C diciptakan. Artinya, C adalah
bahasa yang cukup mendasar dengan fitur yang tidak banyak. C bisa
melakukan _apa saja_, tapi kamu harus bekerja keras untuk itu.

Lalu kenapa kita masih memakainya sekarang?

* Sebagai alat belajar: C bukan hanya bagian terhormat dari sejarah
  komputasi, tapi juga terhubung langsung ke [flw[bare metal|Bare_machine]]
  dengan cara yang tidak dimiliki bahasa masa kini. Saat belajar C, kamu
  belajar bagaimana software berinteraksi dengan memori komputer di level
  rendah. Tidak ada sabuk pengaman. Dijamin, kamu akan menulis software
  yang crash. Dan itu semua bagian dari keseruannya!

* Sebagai alat yang berguna: C masih dipakai untuk aplikasi tertentu,
  seperti membangun [flw[sistem operasi|Operating_system]] atau
  [flw[embedded systems|Embedded_system]]. (Walaupun bahasa pemrograman
  [flw[Rust|Rust_(programming_language)]] sedang mengincar kedua bidang
  ini!)

Kalau kamu sudah mengenal bahasa lain, banyak hal di C yang terasa mudah.
C menginspirasi banyak bahasa lain, jadi kamu akan menemukan
potongan-potongannya di Go, Rust, Swift, Python, JavaScript, Java, dan
berbagai bahasa lainnya. Bagian-bagian itu akan terasa akrab.

_Pointer_ yang bikin orang tersandung di C. Hampir semua hal lain
sudah akrab, tapi pointer adalah yang aneh. Konsep di balik pointer
kemungkinan besar sudah kamu ketahui, tapi C memaksamu membahasnya secara
eksplisit, dengan operator yang mungkin belum pernah kamu lihat.

Bagian ini licik karena begitu kamu [flw[_grok_|Grok]] pointer, tiba-tiba
semuanya jadi mudah. Tapi sebelum saat itu tiba, pointer seperti belut
licin yang susah dipegang.

Segala hal lain di C hanyalah menghafal cara lain (kadang cara yang sama!)
untuk melakukan sesuatu yang sudah pernah kamu lakukan. Pointer-lah bagian
yang aneh. Dan bisa dibilang, pointer pun hanyalah variasi dari tema yang
mungkin sudah kamu kenal.

Jadi bersiaplah untuk petualangan seru, sedekat mungkin dengan inti
komputer tanpa perlu assembly, dalam bahasa komputer paling berpengaruh
sepanjang masa^[Penulis tahu pasti ada yang akan membantah, tapi setidaknya
masuk tiga besar, kan?]. Pegang erat!


## Hello, World!

[i[Hello, world]()]Inilah contoh program C yang paling klasik. Semua
orang memakainya. (Perhatikan bahwa angka di sebelah kiri hanya sebagai acuan
bagi pembaca dan bukan bagian dari kode sumber.)

``` {.c .numberLines}
/* Program Hello world */

#include <stdio.h>

int main(void)
{
    printf("Hello, World!\n");  // Di sinilah pekerjaan sebenarnya dilakukan
}
```

Sekarang kita pakai sarung tangan karet lengan panjang yang tangguh, ambil
scalpel, lalu bedah program ini untuk melihat apa yang membuatnya
bekerja. Bersiaplah, kita mulai. Memotong dengan sangat hati-hati...

[i[Komentar]<]

Kita selesaikan yang mudah dulu: apa pun di antara digraf `/*` dan `*/`
adalah komentar dan sepenuhnya diabaikan oleh compiler. Begitu juga apa
pun di baris yang sama setelah `//`. Dengan komentar, kamu bisa
meninggalkan pesan untuk dirimu sendiri dan orang lain, sehingga ketika
kamu membaca ulang kodenya di masa depan yang jauh, kamu tahu apa yang
dulu coba kamu lakukan. Percayalah, kamu akan lupa; itu hal biasa.

[i[Komentar]>]

[i[C Preprocessor]<]
[i[`#include` directive]<]

Nah, apa itu `#include`? MENJIJIKKAN! Yah, intinya ia menyuruh
C Preprocessor untuk mengambil isi file lain dan menyisipkannya ke dalam
kode tepat _di situ_.

Tunggu---apa itu C Preprocessor? Pertanyaan bagus. Kompilasi punya dua
tahap^[Secara teknis sih lebih dari dua, tapi anggap saja dua---tidak tahu
itu menyenangkan, kan?]: preprocessor dan compiler. [i[Octothorpe]]Apa pun
yang diawali tanda pagar, simbol hash, atau "octothorpe" (`#`) akan
diproses oleh preprocessor[i[Preprocessor]] sebelum compiler mulai
bekerja. _Directive_ preprocessor yang umum adalah `#include` dan
`#define`.[i[`#define` directive]] Soal itu kita bahas nanti.[i[`#include`
directive]>][i[C Preprocessor]>]

Sebelum lanjut, kenapa repot-repot menyebut bahwa tanda pagar itu namanya
octothorpe? Jawabannya sederhana: kata octothorpe itu terlalu lucu, jadi
namanya harus disebarkan setiap ada kesempatan. Octothorpe. Octothorpe,
octothorpe, octothorpe.

Nah, _anyway_. Setelah C preprocessor selesai memproses semuanya, hasilnya
siap diambil compiler untuk dijadikan [flw[kode assembly|Assembly_language]],
[flw[machine code|Machine_code]], atau apa pun yang dikerjakannya. Machine
code adalah "bahasa" yang dipahami CPU, dan CPU memahaminya _dengan sangat
cepat_. Ini salah satu alasan program C cenderung cepat.

Untuk sekarang, jangan pusingkan detail teknis kompilasi; cukup ketahui
bahwa source-mu dilewatkan ke preprocessor, lalu outputnya dilewatkan ke
compiler, yang kemudian menghasilkan executable untuk kamu jalankan.

Bagaimana dengan sisa barisnya? [i[`stdio.h` header file]<]Apa itu
`<stdio.h>`? Itulah yang disebut _header file_. Ekstensi .h di akhir nama
file jadi penandanya. Yang satu ini header file "Standard I/O" (`stdio`),
yang nantinya akan akrab dan kamu sukai. Header ini memberi kita akses ke
berbagai fungsi I/O^[Secara teknis, isinya directive preprocessor dan
function prototype (nanti dibahas) untuk kebutuhan input dan output yang
umum.]. Program demo kita mengeluarkan string "Hello, World!", jadi kita
perlu akses ke fungsi [i[`printf()` fungsi]<] `printf()`. File `<stdio.h>`
memberi kita akses itu. Intinya, kalau kita memakai `printf()` tanpa
`#include <stdio.h>`, compiler akan protes.

Dari mana tahu bahwa `printf()` butuh `#include <stdio.h>`?[i[`printf()`
fungsi]>] Jawabannya: dokumentasi. Di sistem Unix, ketik `man 3 printf`
dan di bagian paling atas halaman man akan tertulis header file apa yang
diperlukan. Atau lihat bagian referensi di buku ini. `:-)`
[i[`stdio.h` header file]>]

Astaga. Semua itu hanya untuk membahas baris pertama! Tapi harus diakui,
baris itu sudah dibedah tuntas. Tidak ada misteri yang tersisa!

Jadi tarik napas dulu...lalu lihat lagi kode contohnya. Tinggal beberapa
baris mudah.

Selamat datang kembali! Penulis tahu kamu sebenarnya tidak istirahat;
tadi hanya menuruti kemauanmu.

[i[`main()` fungsi]<]Baris berikutnya adalah `main()`. Ini definisi
fungsi `main()`; semua yang ada di antara kurung kurawal (`{` dan `}`)
adalah bagian dari definisi fungsi tersebut.

(Lalu bagaimana cara _memanggil_ fungsi lain? Jawabannya ada di baris
`printf()`, yang sebentar lagi kita bahas.)

Fungsi `main` istimewa dalam banyak hal, tapi satu hal mengalahkan yang
lain: fungsi inilah yang dipanggil otomatis ketika programmu mulai
berjalan. Tidak ada kodemu yang dipanggil sebelum `main()`. Untuk contoh
kita, ini tidak jadi masalah karena kita hanya ingin mencetak satu baris
lalu keluar.

Oh ya, satu lagi: begitu eksekusi program melewati ujung `main()`, yaitu
di kurung kurawal penutup di bawah sana, program akan keluar dan kamu
kembali ke command prompt.

Jadi sekarang kita tahu bahwa program itu mengambil sebuah header file,
`stdio.h`[i[stdio.h]T], dan mendeklarasikan fungsi `main()` yang dijalankan
saat program dimulai. Apa saja isi menarik di dalam
`main()`[i[`main()` fungsi]>]?

Senang sekali kamu bertanya. Sungguh! Isinya hanya satu: pemanggilan
fungsi [i[`printf()` fungsi]<]`printf()`. Kamu bisa
membedakan pemanggilan fungsi dari definisi fungsi dengan beberapa cara,
salah satunya adalah tidak adanya kurung kurawal setelahnya. Pemanggilan fungsi diakhiri titik
koma supaya compiler tahu ekspresinya sudah selesai. Kamu akan menaruh
titik koma di akhir hampir segalanya, seperti yang akan terlihat nanti.

Kamu mengoper satu argumen ke fungsi `printf()`[i[`printf()`
fungsi]>]: sebuah string yang akan dicetak saat fungsi dipanggil. Oh,
ya---kita memanggil fungsi! Keren! Tunggu, tunggu---jangan
sombong. [i[`\n` newline]<]Apa itu `\n` yang aneh di ujung string? Begini:
sebagian besar karakter dalam string tercetak persis seperti yang
tersimpan. Tapi ada karakter tertentu yang sulit ditampilkan di layar,
sehingga ditulis sebagai kode backslash dua karakter. Salah satu yang
paling populer adalah `\n` (dibaca "backslash-N" atau cukup "newline"),
yang mewakili karakter _newline_. Karakter ini membuat pencetakan
berikutnya dilanjutkan di awal baris selanjutnya, bukan di baris yang
sama. Mirip menekan Enter di akhir baris.[i[`\n` newline]>]

Sekarang salin kode itu ke file bernama `hello.c` lalu build. Di platform
mirip Unix (misalnya Linux, BSD, Mac, atau WSL), dari command line kamu
bisa build dengan perintah seperti ini:

[i[`gcc` compiler]]
``` {.zsh}
gcc -o hello hello.c
```

(Artinya: "kompilasi `hello.c`, dan hasilkan executable bernama `hello`".)

Setelah selesai, kamu akan punya file bernama `hello` yang bisa dijalankan
dengan perintah ini:

``` {.default}
./hello
```

(Awalan `./` memberi tahu shell untuk "jalankan dari direktori saat ini".)

Lihat apa yang terjadi:

``` {.default}
Hello, World! 
```

Selesai dan sudah teruji! Kirim![i[Hello, world]>]

## Detail Kompilasi

[i[Kompilasi]<]Mari kita bahas lebih jauh cara membangun program C dan apa
yang terjadi di balik layar.

Seperti bahasa lain, C punya _kode sumber_. Tapi tergantung bahasa asalmu,
mungkin kamu belum pernah harus _mengompilasi_ kode sumber menjadi
_executable_.

Kompilasi adalah proses mengubah kode sumber C menjadi program yang bisa
dijalankan oleh sistem operasi.

Developer JavaScript dan Python tidak terbiasa dengan langkah kompilasi
yang terpisah---padahal di balik layar kompilasi tetap terjadi! Python
mengompilasi kode sumbermu menjadi _bytecode_ yang dijalankan oleh mesin
virtual Python. Developer Java sudah terbiasa dengan kompilasi, tapi
hasilnya adalah bytecode untuk Java Virtual Machine.

Kompilasi C menghasilkan _machine code_: deretan 1 dan 0 yang langsung
dan cepat dieksekusi oleh CPU.

> Bahasa yang biasanya tidak dikompilasi disebut bahasa _interpreted_. Tapi
> seperti yang sudah disinggung soal Java dan Python, bahasa-bahasa itu
> juga punya langkah kompilasi. Dan tidak ada aturan yang melarang C
> diinterpretasikan. (Interpreter C itu ada!) Singkatnya, semua ini area
> abu-abu. Secara umum, kompilasi hanyalah mengubah kode sumber menjadi
> bentuk lain yang lebih mudah dieksekusi.

C compiler adalah program yang melakukan kompilasi.

Seperti sudah disebutkan, `gcc` adalah compiler yang terpasang di banyak
[flw[sistem operasi mirip Unix|Unix]]. Biasanya ia dijalankan dari command
line di terminal, tapi tidak selalu. Kamu juga bisa menjalankannya dari
IDE.

Jadi, bagaimana cara build lewat command line?

## Build dengan `gcc`

[i[`gcc` compiler]<]Kalau kamu punya file sumber bernama `hello.c` di
direktori saat ini, kamu bisa membangunnya menjadi program bernama `hello`
dengan mengetik perintah ini di terminal:

``` {.zsh}
gcc -o hello hello.c
```

`-o` berarti "output ke file ini"^[Kalau nama file output tidak diberikan,
hasilnya otomatis ditulis ke file bernama `a.out`---nama ini berakar jauh
di sejarah Unix.]. Lalu `hello.c` di akhir adalah nama file yang ingin
dikompilasi.

Kalau sumbermu terbagi menjadi beberapa file, kamu bisa mengompilasi
semuanya sekaligus (hampir seperti satu file, meski aturan sebenarnya lebih
rumit) dengan menuliskan semua file `.c` di command line:

``` {.zsh}
gcc -o awesomegame ui.c characters.c npc.c items.c
```
[i[`gcc` compiler]>]

dan semuanya akan dibangun menjadi satu executable besar.

Itu cukup untuk memulai---nanti kita akan membahas detail soal banyak file
sumber, object file, dan segala macam hal menarik lainnya.[i[Kompilasi]>]

## Build dengan `clang`

Di Mac, compiler bawaannya bukan `gcc`, melainkan `clang`[i[`clang`
compiler]]. Tapi ada wrapper yang ikut terpasang, jadi menjalankan `gcc`
tetap berfungsi.

Kamu juga bisa memasang compiler `gcc`[i[`gcc` compiler]] yang asli lewat
[fl[Homebrew|https://formulae.brew.sh/formula/gcc]] atau cara lain.

## Build dari IDE

[i[Integrated Development Environment]<]Kalau kamu memakai _Integrated
Development Environment_ (IDE), kemungkinan besar kamu tidak perlu build
dari command line.

Di Visual Studio, `CTRL-F7` untuk build dan `CTRL-F5` untuk run.

Di VS Code, tekan `F5` untuk run lewat debugger. (Kamu perlu memasang
C/C++ Extension.)

Di XCode, build dengan `COMMAND-B` dan run dengan `COMMAND-R`. Untuk
mendapatkan command line tools, cari "XCode command line tools" dan kamu
akan menemukan petunjuk pemasangannya.

Sebagai permulaan, ada baiknya juga mencoba build lewat command line---itu
bagian dari sejarahnya![i[Integrated Development Environment]>]

## Versi C

[i[Language versions]<]C sudah berkembang jauh selama bertahun-tahun, dan
punya banyak nomor versi untuk menyebut dialek bahasa yang sedang dipakai.

Nomor ini biasanya merujuk ke tahun spesifikasinya.

Versi yang paling terkenal ialah C89, C99, C11, dan C23. Buku ini
berfokus pada yang terakhir.

Berikut tabel yang lebih lengkap:

|Versi|Deskripsi|
|-----|--------------|
|K&R C|1978, yang asli. Dinamai dari Brian Kernighan dan Dennis Ritchie. Ritchie merancang bahasa dan menulis kodenya, sementara Kernighan ikut menulis bukunya. Kode K&R asli jarang ditemui sekarang. Kalaupun ada, kodenya akan terlihat aneh, seperti Bahasa Inggris Pertengahan bagi pembaca Inggris modern.|
|**C89**, ANSI C, C90|Pada 1989, American National Standards Institute (ANSI) menerbitkan spesifikasi bahasa C yang menentukan corak C hingga hari ini. Setahun kemudian, kendali diserahkan ke International Organization for Standardization (ISO), yang menerbitkan C90 yang identik.|
|C95|Tambahan pada C89 yang jarang disebut, berisi dukungan wide character.|
|**C99**|Perombakan besar pertama dengan banyak tambahan bahasa. Yang paling diingat orang adalah hadirnya komentar gaya `//`. Saat tulisan ini dibuat, ini versi C yang paling populer dipakai.|
|**C11**|Pembaruan besar ini mencakup dukungan Unicode dan multi-threading. Ingat, kalau kamu mulai memakai fitur-fitur ini, portabilitas ke tempat-tempat yang masih terjebak di C99 bisa jadi korbannya. Tapi, jujur saja, 1999 sudah lama sekali.|
|C17, C18|Pembaruan perbaikan bug untuk C11. C17 tampaknya nama resminya, tapi penerbitannya tertunda sampai 2018. Setahu penulis, keduanya bisa dipertukarkan, dan C17 lebih disukai.|
|C23|Spesifikasi terbaru.||

[i[`gcc` compiler]<]Kamu bisa memaksa GCC memakai salah satu standar
ini lewat argumen command line `-std=`. Kalau ingin ketat mengikuti
standar, tambahkan `-pedantic`.

Misalnya:

``` {.zsh}
gcc -std=c11 -pedantic foo.c
```

Untuk buku ini, program dikompilasi sebagai C23 dengan semua peringatan
diaktifkan:

``` {.zsh}
gcc -Wall -Wextra -std=c23 -pedantic foo.c
```
[i[`gcc` compiler]>]
