<!-- BG_NEW_CHAPTER -->

# Kata Pengantar
<!-- Beej's guide to C
# vim: ts=4:sw=4:nosi:et:tw=72
-->

<!-- No hyphenation -->
\hyphenation{scalbn}
\hyphenation{scalbnf}
\hyphenation{scalbnl}
\hyphenation{scalbln}
\hyphenation{scalblnf}
\hyphenation{scalblnl}
<!-- Can't do things that aren't letters
\hyphenation{atan2}
\hyphenation{atan2f}
\hyphenation{atan2l}
-->
\hyphenation{lrint}
\hyphenation{lrintf}
\hyphenation{lrintl}
\hyphenation{llrint}
\hyphenation{llrintf}
\hyphenation{llrintl}
\hyphenation{lround}
\hyphenation{lroundf}
\hyphenation{lroundl}
\hyphenation{llround}
\hyphenation{llroundf}
\hyphenation{llroundl}

<!-- Index see alsos -->
\index{String|see {\texttt{char *}}}
\index{New line|see {\texttt{{\backslash}n} newline}}
\index{Ternary operator|see {\texttt{?:} ternary operator}}
\index{Addition operator|see {\texttt{+} addition operator}}
\index{Subtraction operator|see {\texttt{-} subtraction operator}}
\index{Multiplication operator|see {\texttt{*} multiplication operator}}
\index{Division operator|see {\texttt{/} division operator}}
\index{Modulus operator|see {\texttt{\%} modulus operator}}
\index{Boolean NOT|see {\texttt{"!} operator}}
\index{Boolean AND|see {\texttt{\&\&} operator}}
\index{Boolean OR|see {\texttt{\textbar{}\textbar{}} operator}}
\index{Bell|see {\texttt{{\backslash}a} operator}}
\index{Tab (is better)|see {\texttt{{\backslash}t} operator}}
\index{Carriage return|see {\texttt{{\backslash}r} operator}}
\index{Hexadecimal|see {\texttt{0x} hexadecimal}}

> _C bukan bahasa yang besar, dan tidak terlayani dengan baik oleh buku yang besar._
>
> --Brian W. Kernighan, Dennis M. Ritchie

Tidak ada gunanya membuang kata-kata di sini, kawan, langsung saja lompat
ke kode C:

``` {.c}
E((ck?main((z?(stat(M,&t)?P+=a+'{'?0:3:
execv(M,k),a=G,i=P,y=G&255,
sprintf(Q,y/'@'-3?A(*L(V(%d+%d)+%d,0)
```

Dan mereka hidup bahagia selamanya. Tamat.

Apa nih? Katamu masih ada yang belum jelas soal bahasa pemrograman C ini?

Baiklah, jujur saja, penulis sendiri tidak yakin apa yang dilakukan kode
di atas. Itu potongan dari salah satu peserta [International
Obfuscated C Code Contest](https://www.ioccc.org/)^[https://www.ioccc.org/]\index{International Obfuscated C Code Contest} tahun 2001, sebuah kompetisi luar biasa
tempat para peserta berlomba menulis kode C yang sesulit mungkin dibaca,
dan hasilnya sering mengejutkan.

Kabar buruknya, kalau kamu masih pemula, semua kode C yang kamu lihat
mungkin tampak membingungkan! Kabar baiknya, itu tidak akan berlangsung
lama.

Sepanjang panduan ini, kita akan berusaha membawa kamu dari kebingungan
yang benar-benar total menuju jenis kebahagiaan tercerahkan yang hanya
bisa dicapai lewat pemrograman C murni. Mantap.

Dulu, C adalah bahasa yang lebih sederhana. Banyak sekali fitur dalam buku
ini, dan _banyak_ fitur dalam volume Referensi Library, belum ada ketika
K&R menulis edisi kedua buku terkenal mereka pada tahun 1988. Meski
demikian, intinya bahasa ini tetap kecil, dan mudah-mudahan penyajian di
sini dimulai dari inti sederhana itu lalu berkembang ke luar.

Dan itulah dalih penulis menulis buku yang selucu dan sebesar ini untuk
bahasa yang sekecil dan seringkas itu.

## Pembaca

Panduan ini mengasumsikan bahwa kamu sudah punya pengetahuan pemrograman
dari bahasa lain, seperti
[Python](https://en.wikipedia.org/wiki/Python_(programming_language))^[https://en.wikipedia.org/wiki/Python_(programming_language)],
[JavaScript](https://en.wikipedia.org/wiki/JavaScript)^[https://en.wikipedia.org/wiki/JavaScript], [Java](https://en.wikipedia.org/wiki/Java_(programming_language))^[https://en.wikipedia.org/wiki/Java_(programming_language)],
[Rust](https://en.wikipedia.org/wiki/Rust_(programming_language))^[https://en.wikipedia.org/wiki/Rust_(programming_language)],
[Go](https://en.wikipedia.org/wiki/Go_(programming_language))^[https://en.wikipedia.org/wiki/Go_(programming_language)],
[Swift](https://en.wikipedia.org/wiki/Swift_(programming_language))^[https://en.wikipedia.org/wiki/Swift_(programming_language)], dan lainnya.
(Developer [Objective-C](https://en.wikipedia.org/wiki/Objective-C)^[https://en.wikipedia.org/wiki/Objective-C] akan merasa sangat mudah!)

Diasumsikan bahwa kamu sudah tahu apa itu variabel, apa yang dilakukan
loop, bagaimana cara kerja fungsi, dan seterusnya.

Kalau ternyata kamu belum punya bekal itu, yang bisa ditawarkan hanyalah
bacaan yang menghibur. Satu-satunya janji yang masuk akal: panduan ini
tidak akan berakhir dengan cliffhanger... atau justru _akan_?

## Cara Membaca Buku Ini

Panduan ini terdiri dari dua volume, dan ini yang pertama: volume
tutorial!

Volume kedua adalah [referensi
library](https://beej.us/guide/bgclr/)^[https://beej.us/guide/bgclr/], dan isinya jauh lebih banyak
referensi daripada tutorial.

Kalau kamu masih baru, bacalah bagian tutorial secara berurutan, kurang
lebih. Makin ke belakang, makin tidak penting urutannya.

Dan apa pun tingkat keahlianmu, bagian referensi selalu tersedia, lengkap
dengan contoh pemanggilan fungsi standard library untuk menyegarkan
ingatan kapan pun dibutuhkan. Cocok dibaca sambil makan sereal atau di
waktu-waktu lainnya.

Terakhir, kalau kamu membaca versi cetak, coba intip indeksnya: entri yang
berasal dari bagian referensi dicetak miring.

## Platform dan Compiler

Panduan ini akan berusaha untuk tetap pada [ISO-standard C](https://en.wikipedia.org/wiki/ANSI_C)^[https://en.wikipedia.org/wiki/ANSI_C]
yang Klasik dan Sederhana. Ya, sebagian besar. Di sana-sini mungkin ada
bahasan tentang [POSIX](https://en.wikipedia.org/wiki/POSIX)^[https://en.wikipedia.org/wiki/POSIX] atau semacamnya, tapi kita lihat nanti.

Pengguna **Unix** (misalnya Linux, BSD, dll.) coba jalankan `cc` atau `gcc`
dari command line---mungkin compiler-nya sudah terpasang. Kalau belum, cari
tahu cara memasang `gcc` atau `clang` di distribusimu.

Pengguna **Windows** sebaiknya cek [Visual Studio
Community](https://visualstudio.microsoft.com/vs/community/)^[https://visualstudio.microsoft.com/vs/community/]. Atau, jika
menginginkan pengalaman yang lebih mirip Unix (direkomendasikan!), instal
[WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)^[https://docs.microsoft.com/en-us/windows/wsl/install-win10] dan
`gcc`.

Pengguna **Mac** perlu memasang
[XCode](https://developer.apple.com/xcode/)^[https://developer.apple.com/xcode/], khususnya command line
tools-nya.

Ada banyak compiler di luar sana, dan hampir semuanya bisa dipakai untuk
buku ini. Compiler C++ juga bisa mengompilasi banyak (tapi tidak semua!)
kode C. Sebisa mungkin, pakailah compiler C yang sebenarnya.

## Halaman Utama Resmi

Lokasi resmi dokumen ini ada di
[https://beej.us/guide/bgc/](https://beej.us/guide/bgc/)^[https://beej.us/guide/bgc/]. Alamat ini
mungkin berubah di masa depan, tapi yang lebih mungkin terjadi adalah
semua panduan lainnya dipindahkan dari komputer Chico State.

## Kebijakan Email

Pada umumnya penulis bersedia membantu lewat email, jadi jangan ragu
menulis, tapi respons tidak dijamin. Hidup penulis cukup sibuk, dan ada
kalanya pertanyaanmu memang tidak sempat dijawab. Kalau begitu, biasanya
pesannya dihapus saja. Jangan diambil hati; penulis hanya tidak akan
pernah punya waktu untuk memberi jawaban serinci yang kamu butuhkan.

Sebagai aturan, makin rumit pertanyaannya, makin kecil kemungkinan
dijawab. Kalau kamu bisa mempersempit pertanyaan sebelum dikirim, dan
menyertakan informasi yang relevan (seperti platform, compiler, pesan
error yang muncul, dan hal lain yang mungkin membantu pelacakan masalah),
peluang mendapat balasan jauh lebih besar.

Kalau tidak ada balasan, coba lagi, teruslah mencari jawabannya, dan kalau
tetap buntu, kirim email lagi berisi hasil temuanmu. Mudah-mudahan itu
cukup untuk membantu.

Setelah panjang lebar soal cara menulis (dan tidak menulis) email, perlu
diketahui bahwa semua pujian yang diterima panduan ini selama bertahun-tahun
sangat dihargai. Itu dorongan moral yang nyata, dan senang rasanya tahu
panduan ini dipakai untuk kebaikan! `:-)` Terima kasih!

## Mirroring

Kamu dipersilakan me-mirror situs ini, baik secara publik maupun privat.
Kalau kamu me-mirror-nya secara publik dan ingin ditautkan dari halaman
utama, kirim pesan ke
[`beej@beej.us`](mailto:beej@beej.us).

## Catatan untuk Penerjemah

Kalau kamu ingin menerjemahkan panduan ini ke bahasa lain, tulis ke
[`beej@beej.us`](beej@beej.us) dan terjemahanmu akan ditautkan dari
halaman utama. Silakan cantumkan nama dan informasi kontakmu dalam
terjemahan.

Perhatikan pembatasan lisensi di bagian Hak Cipta dan Distribusi di bawah.

## Hak Cipta dan Distribusi

Beej's Guide to C memiliki Hak Cipta © 2021 Brian "Beej Jorgensen" Hall.

Dengan pengecualian khusus untuk kode sumber dan terjemahan di bawah, karya
ini dilisensikan di bawah Lisensi Creative Commons
Atribusi-NonKomersial-TanpaDerivasi 3.0. Untuk melihat salinan lisensi
ini, kunjungi
[`https://creativecommons.org/licenses/by-nc-nd/3.0/`](https://creativecommons.org/licenses/by-nc-nd/3.0/)
atau kirim surat ke Creative Commons, 171 Second Street, Suite 300, San
Francisco, California, 94105, USA.

Satu pengecualian khusus untuk bagian "TanpaDerivasi" dari lisensi adalah
sebagai berikut: panduan ini dapat diterjemahkan secara bebas ke bahasa
apapun, selama terjemahannya akurat dan panduan dicetak ulang secara
keseluruhan. Pembatasan lisensi yang sama berlaku untuk terjemahan seperti
untuk panduan asli. Terjemahan juga dapat mencantumkan nama dan informasi
kontak penerjemah.

Kode sumber C yang disajikan dalam dokumen ini diberikan ke domain publik
dan sepenuhnya bebas dari segala pembatasan lisensi.

Para pendidik didorong secara bebas untuk merekomendasikan atau menyediakan
salinan panduan ini kepada murid-murid mereka.

Hubungi [`beej@beej.us`](beej@beej.us) untuk informasi lebih lanjut.

Font sampul adalah Lazenby Computer oleh Andrew Young.

## Dedikasi

Hal-hal tersulit dalam menulis panduan-panduan ini adalah:

* Mempelajari materinya cukup mendalam sampai bisa menjelaskannya
* Mencari cara terbaik untuk menjelaskannya dengan jernih, sebuah proses
  iteratif yang rasanya tidak pernah selesai
* Tampil sebagai _otoritas_, padahal sebenarnya hanyalah manusia biasa
  yang berusaha memahami semuanya, sama seperti orang lain
* Tetap konsisten ketika begitu banyak hal lain menyita perhatian

Banyak orang telah membantu sepanjang proses ini, dan penulis ingin
mengucapkan terima kasih kepada mereka yang telah mewujudkan buku ini.

* Semua orang di Internet yang memutuskan berbagi pengetahuan mereka dalam
  satu bentuk atau lainnya. Berbagi informasi pembelajaran secara bebas
  itulah yang membuat Internet menjadi tempat yang luar biasa.
* Para sukarelawan di [cppreference.com](https://en.cppreference.com/)^[https://en.cppreference.com/]
  yang membangun jembatan dari spesifikasi menuju dunia nyata.
* Orang-orang yang suka membantu dan berpengetahuan luas di
  [comp.lang.c](https://groups.google.com/g/comp.lang.c)^[https://groups.google.com/g/comp.lang.c] dan
  [r/C_Programming](https://www.reddit.com/r/C_Programming/)^[https://www.reddit.com/r/C_Programming/], yang
  menemani penulis melewati bagian-bagian bahasa yang paling sulit.
* Semua orang yang mengirim koreksi dan pull-request, mulai dari
  instruksi yang menyesatkan sampai typo.

Terima kasih! ♥



<!-- BG_NEW_CHAPTER -->

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
lower-level seperti [assembly](https://en.wikipedia.org/wiki/Assembly_language)^[https://en.wikipedia.org/wiki/Assembly_language].

Tapi di era modern ini, bahasa-bahasa generasi sekarang menawarkan berbagai
fitur yang belum ada pada 1972, ketika C diciptakan. Artinya, C adalah
bahasa yang cukup mendasar dengan fitur yang tidak banyak. C bisa
melakukan _apa saja_, tapi kamu harus bekerja keras untuk itu.

Lalu kenapa kita masih memakainya sekarang?

* Sebagai alat belajar: C bukan hanya bagian terhormat dari sejarah
  komputasi, tapi juga terhubung langsung ke [bare metal](https://en.wikipedia.org/wiki/Bare_machine)^[https://en.wikipedia.org/wiki/Bare_machine]
  dengan cara yang tidak dimiliki bahasa masa kini. Saat belajar C, kamu
  belajar bagaimana software berinteraksi dengan memori komputer di level
  rendah. Tidak ada sabuk pengaman. Dijamin, kamu akan menulis software
  yang crash. Dan itu semua bagian dari keseruannya!

* Sebagai alat yang berguna: C masih dipakai untuk aplikasi tertentu,
  seperti membangun [sistem operasi](https://en.wikipedia.org/wiki/Operating_system)^[https://en.wikipedia.org/wiki/Operating_system] atau
  [embedded systems](https://en.wikipedia.org/wiki/Embedded_system)^[https://en.wikipedia.org/wiki/Embedded_system]. (Walaupun bahasa pemrograman
  [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language))^[https://en.wikipedia.org/wiki/Rust_(programming_language)] sedang mengincar kedua bidang
  ini!)

Kalau kamu sudah mengenal bahasa lain, banyak hal di C yang terasa mudah.
C menginspirasi banyak bahasa lain, jadi kamu akan menemukan
potongan-potongannya di Go, Rust, Swift, Python, JavaScript, Java, dan
berbagai bahasa lainnya. Bagian-bagian itu akan terasa akrab.

_Pointer_ yang bikin orang tersandung di C. Hampir semua hal lain
sudah akrab, tapi pointer adalah yang aneh. Konsep di balik pointer
kemungkinan besar sudah kamu ketahui, tapi C memaksamu membahasnya secara
eksplisit, dengan operator yang mungkin belum pernah kamu lihat.

Bagian ini licik karena begitu kamu [_grok_](https://en.wikipedia.org/wiki/Grok)^[https://en.wikipedia.org/wiki/Grok] pointer, tiba-tiba
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

\index{Hello, world|}Inilah contoh program C yang paling klasik. Semua
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

\index{Komentar|(}

Kita selesaikan yang mudah dulu: apa pun di antara digraf `/*` dan `*/`
adalah komentar dan sepenuhnya diabaikan oleh compiler. Begitu juga apa
pun di baris yang sama setelah `//`. Dengan komentar, kamu bisa
meninggalkan pesan untuk dirimu sendiri dan orang lain, sehingga ketika
kamu membaca ulang kodenya di masa depan yang jauh, kamu tahu apa yang
dulu coba kamu lakukan. Percayalah, kamu akan lupa; itu hal biasa.

\index{Komentar|)}

\index{C Preprocessor|(}
\index{\#include directive@\texttt{\#include} directive|(}

Nah, apa itu `#include`? MENJIJIKKAN! Yah, intinya ia menyuruh
C Preprocessor untuk mengambil isi file lain dan menyisipkannya ke dalam
kode tepat _di situ_.

Tunggu---apa itu C Preprocessor? Pertanyaan bagus. Kompilasi punya dua
tahap^[Secara teknis sih lebih dari dua, tapi anggap saja dua---tidak tahu
itu menyenangkan, kan?]: preprocessor dan compiler. \index{Octothorpe}Apa pun
yang diawali tanda pagar, simbol hash, atau "octothorpe" (`#`) akan
diproses oleh preprocessor\index{Preprocessor} sebelum compiler mulai
bekerja. _Directive_ preprocessor yang umum adalah `#include` dan
`#define`.\index{\#define directive@\texttt{\#define} directive} Soal itu kita bahas nanti.\index{\#include directive@\texttt{\#include} directive|)}\index{C Preprocessor|)}

Sebelum lanjut, kenapa repot-repot menyebut bahwa tanda pagar itu namanya
octothorpe? Jawabannya sederhana: kata octothorpe itu terlalu lucu, jadi
namanya harus disebarkan setiap ada kesempatan. Octothorpe. Octothorpe,
octothorpe, octothorpe.

Nah, _anyway_. Setelah C preprocessor selesai memproses semuanya, hasilnya
siap diambil compiler untuk dijadikan [kode assembly](https://en.wikipedia.org/wiki/Assembly_language)^[https://en.wikipedia.org/wiki/Assembly_language],
[machine code](https://en.wikipedia.org/wiki/Machine_code)^[https://en.wikipedia.org/wiki/Machine_code], atau apa pun yang dikerjakannya. Machine
code adalah "bahasa" yang dipahami CPU, dan CPU memahaminya _dengan sangat
cepat_. Ini salah satu alasan program C cenderung cepat.

Untuk sekarang, jangan pusingkan detail teknis kompilasi; cukup ketahui
bahwa source-mu dilewatkan ke preprocessor, lalu outputnya dilewatkan ke
compiler, yang kemudian menghasilkan executable untuk kamu jalankan.

Bagaimana dengan sisa barisnya? \index{stdio.h header file@\texttt{stdio.h} header file|(}Apa itu
`<stdio.h>`? Itulah yang disebut _header file_. Ekstensi .h di akhir nama
file jadi penandanya. Yang satu ini header file "Standard I/O" (`stdio`),
yang nantinya akan akrab dan kamu sukai. Header ini memberi kita akses ke
berbagai fungsi I/O^[Secara teknis, isinya directive preprocessor dan
function prototype (nanti dibahas) untuk kebutuhan input dan output yang
umum.]. Program demo kita mengeluarkan string "Hello, World!", jadi kita
perlu akses ke fungsi \index{printf() fungsi@\texttt{printf()} fungsi|(} `printf()`. File `<stdio.h>`
memberi kita akses itu. Intinya, kalau kita memakai `printf()` tanpa
`#include <stdio.h>`, compiler akan protes.

Dari mana tahu bahwa `printf()` butuh `#include <stdio.h>`?\index{printf() fungsi@\texttt{printf()} fungsi|)} Jawabannya: dokumentasi. Di sistem Unix, ketik `man 3 printf`
dan di bagian paling atas halaman man akan tertulis header file apa yang
diperlukan. Atau lihat bagian referensi di buku ini. `:-)`
\index{stdio.h header file@\texttt{stdio.h} header file|)}

Astaga. Semua itu hanya untuk membahas baris pertama! Tapi harus diakui,
baris itu sudah dibedah tuntas. Tidak ada misteri yang tersisa!

Jadi tarik napas dulu...lalu lihat lagi kode contohnya. Tinggal beberapa
baris mudah.

Selamat datang kembali! Penulis tahu kamu sebenarnya tidak istirahat;
tadi hanya menuruti kemauanmu.

\index{main() fungsi@\texttt{main()} fungsi|(}Baris berikutnya adalah `main()`. Ini definisi
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
`stdio.h`\index{stdio.h|}, dan mendeklarasikan fungsi `main()` yang dijalankan
saat program dimulai. Apa saja isi menarik di dalam
`main()`\index{main() fungsi@\texttt{main()} fungsi|)}?

Senang sekali kamu bertanya. Sungguh! Isinya hanya satu: pemanggilan
fungsi \index{printf() fungsi@\texttt{printf()} fungsi|(}`printf()`. Kamu bisa
membedakan pemanggilan fungsi dari definisi fungsi dengan beberapa cara,
salah satunya adalah tidak adanya kurung kurawal setelahnya. Pemanggilan fungsi diakhiri titik
koma supaya compiler tahu ekspresinya sudah selesai. Kamu akan menaruh
titik koma di akhir hampir segalanya, seperti yang akan terlihat nanti.

Kamu mengoper satu argumen ke fungsi `printf()`\index{printf() fungsi@\texttt{printf()} fungsi|)}: sebuah string yang akan dicetak saat fungsi dipanggil. Oh,
ya---kita memanggil fungsi! Keren! Tunggu, tunggu---jangan
sombong. \index{{\backslash}n newline@\texttt{{\backslash}n} newline|(}Apa itu `\n` yang aneh di ujung string? Begini:
sebagian besar karakter dalam string tercetak persis seperti yang
tersimpan. Tapi ada karakter tertentu yang sulit ditampilkan di layar,
sehingga ditulis sebagai kode backslash dua karakter. Salah satu yang
paling populer adalah `\n` (dibaca "backslash-N" atau cukup "newline"),
yang mewakili karakter _newline_. Karakter ini membuat pencetakan
berikutnya dilanjutkan di awal baris selanjutnya, bukan di baris yang
sama. Mirip menekan Enter di akhir baris.\index{{\backslash}n newline@\texttt{{\backslash}n} newline|)}

Sekarang salin kode itu ke file bernama `hello.c` lalu build. Di platform
mirip Unix (misalnya Linux, BSD, Mac, atau WSL), dari command line kamu
bisa build dengan perintah seperti ini:

\index{gcc compiler@\texttt{gcc} compiler}
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

Selesai dan sudah teruji! Kirim!\index{Hello, world|)}

## Detail Kompilasi

\index{Kompilasi|(}Mari kita bahas lebih jauh cara membangun program C dan apa
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
[sistem operasi mirip Unix](https://en.wikipedia.org/wiki/Unix)^[https://en.wikipedia.org/wiki/Unix]. Biasanya ia dijalankan dari command
line di terminal, tapi tidak selalu. Kamu juga bisa menjalankannya dari
IDE.

Jadi, bagaimana cara build lewat command line?

## Build dengan `gcc`

\index{gcc compiler@\texttt{gcc} compiler|(}Kalau kamu punya file sumber bernama `hello.c` di
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
\index{gcc compiler@\texttt{gcc} compiler|)}

dan semuanya akan dibangun menjadi satu executable besar.

Itu cukup untuk memulai---nanti kita akan membahas detail soal banyak file
sumber, object file, dan segala macam hal menarik lainnya.\index{Kompilasi|)}

## Build dengan `clang`

Di Mac, compiler bawaannya bukan `gcc`, melainkan `clang`\index{clang compiler@\texttt{clang} compiler}. Tapi ada wrapper yang ikut terpasang, jadi menjalankan `gcc`
tetap berfungsi.

Kamu juga bisa memasang compiler `gcc`\index{gcc compiler@\texttt{gcc} compiler} yang asli lewat
[Homebrew](https://formulae.brew.sh/formula/gcc)^[https://formulae.brew.sh/formula/gcc] atau cara lain.

## Build dari IDE

\index{Integrated Development Environment|(}Kalau kamu memakai _Integrated
Development Environment_ (IDE), kemungkinan besar kamu tidak perlu build
dari command line.

Di Visual Studio, `CTRL-F7` untuk build dan `CTRL-F5` untuk run.

Di VS Code, tekan `F5` untuk run lewat debugger. (Kamu perlu memasang
C/C++ Extension.)

Di XCode, build dengan `COMMAND-B` dan run dengan `COMMAND-R`. Untuk
mendapatkan command line tools, cari "XCode command line tools" dan kamu
akan menemukan petunjuk pemasangannya.

Sebagai permulaan, ada baiknya juga mencoba build lewat command line---itu
bagian dari sejarahnya!\index{Integrated Development Environment|)}

## Versi C

\index{Language versions|(}C sudah berkembang jauh selama bertahun-tahun, dan
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

\index{gcc compiler@\texttt{gcc} compiler|(}Kamu bisa memaksa GCC memakai salah satu standar
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
\index{gcc compiler@\texttt{gcc} compiler|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Variabel dan Pernyataan

> _"It takes all kinds to make a world, does it not, Padre?"_ \
> _"So it does, my son, so it does."_
>
> ---Pirate Captain Thomas Bartholomew Red to the Padre, Pirates

Program C memang berisi banyak hal.

Yap.

Dan karena berbagai alasan, kita semua akan terbantu kalau
mengelompokkan jenis-jenis hal yang ada dalam sebuah program, supaya
jelas apa yang sedang dibicarakan.

## Variabel

\index{variabel|(}Orang bilang, "variabel menyimpan nilai". Tapi cara lain
memandangnya: variabel adalah nama yang mudah dibaca manusia untuk merujuk
ke sebuah data di memori.

Kita berhenti sebentar di sini untuk mengintip ke lubang kelinci bernama
pointer. Jangan khawatir dulu.

Bayangkan memori sebagai array raksasa berisi byte^[Satu "byte" biasanya
adalah bilangan biner 8-bit. Bayangkan sebagai integer yang hanya bisa
menyimpan nilai 0 sampai 255, inklusif. Secara teknis, C membolehkan byte
punya jumlah bit berapa pun, dan kalau ingin merujuk dengan jelas ke
bilangan 8-bit, istilah yang tepat adalah _oktet_. Tapi programmer akan
menganggap kamu bermaksud 8-bit kalau bilang "byte", kecuali kamu
menyebutkan yang lain.]. Data disimpan di dalam "array" ini^[Cara kerja
memori modern di sini disederhanakan sedikit berlebihan. Tapi model mental
ini cukup berhasil, jadi mohon dimaklumi.]. Kalau sebuah angka lebih besar
dari satu byte, angka itu disimpan dalam beberapa byte. Karena memori mirip
array, setiap byte memori bisa dirujuk lewat indeksnya. Indeks ke dalam
memori ini juga disebut _alamat_, atau _lokasi_, atau _pointer_.

Ketika kamu punya variabel di C, nilainya berada di memori _di suatu
tempat_, pada suatu alamat. Tentu saja. Memangnya di mana lagi? Tapi
merujuk ke sebuah nilai lewat alamat numeriknya itu merepotkan, jadi kita
memberinya nama, dan itulah yang disebut variabel.

Ada dua alasan kenapa hal ini dibahas:

1. Ini memudahkan pemahaman variabel pointer nanti---variabel yang
   menyimpan alamat variabel lain!
2. Dan, ini juga memudahkan pemahaman pointer nanti.

Jadi, variabel adalah nama untuk data yang disimpan di memori pada suatu
alamat.

### Nama Variabel

\index{variabel|(}Nama variabel boleh memakai karakter apa pun dalam rentang
0-9, A-Z, a-z, dan underscore, dengan aturan berikut:

* Nama variabel tidak boleh diawali digit 0-9.
* Nama variabel tidak boleh diawali dua underscore.
* Nama variabel tidak boleh diawali underscore yang diikuti huruf kapital
  A-Z.

Untuk Unicode, coba saja. Spesifikasi di §D.2 punya sejumlah aturan tentang
rentang codepoint Unicode mana yang boleh dipakai di bagian mana dari
identifier, tapi itu terlalu panjang untuk ditulis di sini dan kemungkinan
besar tidak perlu kamu pikirkan.\index{variabel|)}

### Tipe Variabel

\index{tipe}Tergantung bahasa apa yang ada di gudang senjatamu, kamu mungkin
sudah akrab, mungkin juga belum, dengan konsep tipe. Tapi C cukup ketat
soal ini, jadi kita perlu mengulasnya sedikit.

Berikut beberapa contoh tipe, yang paling dasar saja:

\index{int tipe@\texttt{int} tipe}\index{float tipe@\texttt{float} tipe}\index{char tipe@\texttt{char} tipe}\index{char * tipe@\texttt{char *} tipe}

|Tipe|Contoh|Tipe C|
|:---|------:|:-----|
|Integer|`3490`|`int`|
|Floating point|`3.14159`|`float`^[Ini sedikit bohong. Secara teknis `3.14159` bertipe `double`, tapi kita belum sampai ke sana, dan tujuannya adalah supaya kamu mengasosiasikan `float` dengan "Floating Point". C dengan senang hati akan mengonversinya menjadi `float`. Singkatnya, jangan dipusingkan dulu.]|
|Karakter (tunggal)|`'c'`|`char`|
|String|`"Hello, world!"`|`char *`^[Bacanya "pointer ke char" atau "char pointer". "Char" kependekan dari karakter. Belum ada studi resmi, tapi secara anekdot kebanyakan orang melafalkannya "char" (seperti "charcoal"), sebagian kecil bilang "car", dan segelintir bilang "care". Pointer akan dibahas lebih lanjut nanti.]|

C berusaha mengonversi secara otomatis antara kebanyakan tipe numerik kalau
diminta. Selain itu, semua konversi dilakukan manual, terutama antara
string dan numerik.

Hampir semua tipe di C adalah varian dari tipe-tipe ini.

Sebelum memakai variabel, kamu harus _mendeklarasikannya_ dan memberi tahu
C tipe apa yang akan dipegangnya. Setelah dideklarasikan, tipe variabel
tidak bisa diubah lagi saat runtime. Apa yang kamu tetapkan, itulah yang
berlaku sampai variabel keluar dari scope dan diserap kembali oleh alam
semesta.

Mari kita ambil kode "Hello, world" tadi dan tambahkan beberapa variabel:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;    // Menyimpan integer bertanda, mis. -3, -2, 0, 1, 10
    float f;  // Menyimpan bilangan floating point bertanda, mis. -3.1416

    printf("Hello, World!\n");  // Ah, keakraban yang menenangkan
}
```

Nah! Kita sudah mendeklarasikan beberapa variabel. Keduanya belum kita
pakai dan belum diinisialisasi. Yang satu menyimpan integer, yang lain
menyimpan floating point number (pada dasarnya bilangan real, kalau kamu
punya latar belakang matematika).

\index{variabel!belum diinisialisasi}Variabel yang belum diinisialisasi
bernilai tak tentu^[Secara informal kita bilang nilainya "acak", padahal
sebenarnya bukan---bahkan bukan bilangan pseudo-acak.]. Variabel harus
diinisialisasi, atau kamu harus menganggap isinya angka sampah.

> Di sinilah C bisa "menjebak" kamu. Seringnya, dari pengalaman, nilai yang
> tak tentu itu nol... tapi bisa berubah dari satu run ke run berikutnya!
> Jangan pernah menganggap nilainya nol, meskipun kamu melihatnya nol.
> _Selalu_ inisialisasi variabel secara eksplisit sebelum dipakai^[Ini
> tidak sepenuhnya benar. Ketika kita sampai ke pembahasan static storage
> duration, kamu akan tahu bahwa beberapa variabel otomatis diinisialisasi
> ke nol. Tapi cara yang aman adalah selalu menginisialisasinya sendiri.].

Apa nih? Kamu mau menyimpan angka di variabel itu? Gila kamu!

Ayo kita lakukan:
\index{= operator penugasan@\texttt{=} operator penugasan}

``` {.c .numberLines}
int main(void)
{
    int i;

    i = 2; // Menugaskan nilai 2 ke variabel i

    printf("Hello, World!\n");
}
```

Mantap. Kita sudah menyimpan sebuah nilai. Sekarang kita cetak.

\index{printf() fungsi@\texttt{printf()} fungsi|(}Caranya, kita oper _dua_ argumen keren ke fungsi
`printf()`. Argumen pertama adalah string yang menjelaskan apa yang akan
dicetak dan bagaimana mencetaknya (disebut _format string_), dan argumen
kedua adalah nilai yang dicetak, yaitu apa pun isi variabel `i`.

`printf()` mencari urutan-urutan khusus dalam format string yang diawali
tanda persen (`%`), yang memberi tahu apa yang harus dicetak. Misalnya,
kalau menemukan `%d`, ia mengambil parameter berikutnya dan mencetaknya
sebagai integer. Kalau menemukan `%f`, nilainya dicetak sebagai float.
Kalau menemukan `%s`, ia mencetak string.

Dengan begitu, kita bisa mencetak nilai berbagai tipe seperti ini:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 2;
    float f = 3.14;
    char *s = "Hello, world!";  // char * ("char pointer") adalah sebuah string

    printf("%s  i = %d and f = %f!\n", s, i, f);
}
```

Outputnya akan seperti ini:

``` {.default}
Hello, world!  i = 2 and f = 3.14!
```

Dengan begitu, `printf()` mungkin mirip dengan format string atau
parameterized string di bahasa lain yang sudah kamu kenal.
\index{printf() fungsi@\texttt{printf()} fungsi|)}

***CATATAN:*** Di bagian-bagian berikut, kita anggap kamu sudah
mendeklarasikan variabelnya di awal. Kalau sebuah contoh memakai integer
`i` dan `j`, bayangkan saja ada baris ini di suatu tempat di atasnya:

``` {.c}
int i, j;
```

### Tipe Boolean

\index{tipe Boolean|(}Apakah C punya tipe Boolean, ya atau tidak?

`1`!

Secara historis, C tidak punya tipe Boolean, dan sebagian orang berpendapat
sampai sekarang pun belum.

Di C, `0` berarti "false", dan nilai bukan nol berarti "true".

Jadi `1` itu true. `-37` juga true. Dan `0` adalah false.

Kamu bisa mendeklarasikan Boolean sebagai `int` biasa:

``` {.c}
int x = 1;

if (x) {
    printf("x is true!\n");
}
```

Di C23, kita mendapat `bool`, `true`, dan `false` yang sesungguhnya.
Sebelum itu, kalau versi C-mu cukup modern, kamu bisa
`#include <stdbool.h>`\index{stdbool.h header file@\texttt{stdbool.h} header file} untuk hasil yang sama.

``` {.c .numberLines}
#include <stdio.h>
#include <stdbool.h>  // tidak diperlukan di C23

int main(void) {
    bool x = true;

    if (x) {
        printf("x is true!\n");
    }
}
```

Secara teknis, variabel `bool` seharusnya diisi `true`, `false`, atau
hasil ekspresi yang bernilai true atau false. Tapi sebenarnya kamu bisa
mengonversi berbagai hal ke `bool`. Ada aturan khusus, tapi yang bernilai
nol cenderung menjadi `false`, dan yang bukan nol menjadi true.

Hati-hati kalau keduanya dicampur, karena nilai numerik `true` kemungkinan
besar adalah `1`[^B1CA]. Kalau kamu mengandalkan nilai positif lain sebagai
true, hasilnya bisa tidak cocok. Misalnya:

``` {.c .numberLines}
printf("%d\n", true == 12);  // Mencetak "0", false!
```


[^B1CA]: Secara teknis hanya satu bit dari sebuah `char` yang dipakai untuk
    merepresentasikan `bool`, jadi nilainya hanya bisa nol atau satu.
    Hanya saja, isi bit-bit sisanya (padding) dalam `char` itu tidak
    ditentukan. Untuk `false`, pasti semuanya nol. Tapi untuk `true`,
    penulis tidak yakin apakah sisanya harus nol semua.

\index{tipe Boolean|)}

## Operator dan Ekspresi {#operators}

Operator C seharusnya sudah akrab dari bahasa lain. Kita bahas sekilas
beberapa di antaranya di sini.

(Detailnya masih lebih banyak dari ini, tapi bagian ini sudah cukup untuk
memulai.)


### Aritmetika

\index{Operator Aritmetika|}Semoga ini sudah akrab:
\index{+ operator penjumlahan@\texttt{+} operator penjumlahan} \index{- operator pengurangan@\texttt{-} operator pengurangan}
\index{* operator perkalian@\texttt{*} operator perkalian} \index{/ operator pembagian@\texttt{/} operator pembagian}
\index{\% operator modulus@\texttt{\%} operator modulus}

``` {.c}
i = i + 3;  // Operator penjumlahan (+) dan penugasan (=), tambahkan 3 ke i
i = i - 8;  // Pengurangan, kurangi i dengan 8
i = i * 9;  // Perkalian
i = i / 2;  // Pembagian
i = i % 5;  // Modulo (sisa pembagian)
```

Semua itu punya varian singkat. Setiap baris di atas bisa ditulis lebih
ringkas sebagai:
\index{+= operator penugasan@\texttt{+=} operator penugasan} \index{-= operator penugasan@\texttt{-=} operator penugasan}
\index{*= operator penugasan@\texttt{*=} operator penugasan} \index{/= operator penugasan@\texttt{/=} operator penugasan}
\index{\%= operator penugasan@\texttt{\%=} operator penugasan}

``` {.c}
i += 3;  // Sama dengan "i = i + 3", tambahkan 3 ke i
i -= 8;  // Sama dengan "i = i - 8"
i *= 9;  // Sama dengan "i = i * 9"
i /= 2;  // Sama dengan "i = i / 2"
i %= 5;  // Sama dengan "i = i % 5"
```

Tidak ada operator pangkat. Kamu harus memakai salah satu varian fungsi
`pow()`\index{pow()|} dari `math.h`.

Sekarang kita masuk ke hal-hal yang lebih aneh, yang mungkin tidak ada di
bahasa lain!\index{Operator Aritmetika|)}

### Operator Ternary

\index{?: operator ternary@\texttt{?:} operator ternary|(}C juga punya _operator ternary_, yaitu
ekspresi yang nilainya bergantung pada hasil sebuah kondisional yang
tertanam di dalamnya.

``` {.c}
// Jika x > 10, tambahkan 17 ke y. Jika tidak, tambahkan 37 ke y.

y += x > 10? 17: 37;
```

Berantakan sekali! Tapi kamu akan terbiasa seiring sering membacanya.
Supaya lebih jelas, ekspresi di atas bisa ditulis ulang dengan pernyataan
`if`:

``` {.c}
// Ekspresi ini:

y += x > 10? 17: 37;

// setara dengan non-ekspresi ini:

if (x > 10)
    y += 17;
else
    y += 37;
```

Bandingkan keduanya sampai kamu bisa melihat masing-masing komponen
operator ternary.

Atau contoh lain, yang mencetak apakah angka di `x` ganjil atau genap:

``` {.c}
printf("The number %d is %s.\n", x, x % 2 == 0? "even": "odd");
```

Format specifier `%s` di `printf()`\index{printf()|} berarti "cetak string".
Kalau ekspresi `x % 2` bernilai `0`, nilai seluruh ekspresi ternary adalah
string `"even"`. Kalau tidak, nilainya string `"odd"`. Keren sekali!

Perlu diingat bahwa operator ternary bukan kendali alur seperti pernyataan
`if`. Ia hanyalah ekspresi yang menghasilkan sebuah nilai.
\index{?: operator ternary@\texttt{?:} operator ternary|)}

### Pre-dan-Post Increment-dan-Decrement

\index{++ operator increment@\texttt{++} operator increment|(} \index{-- operator decrement@\texttt{--} operator decrement|(}
Sekarang kita bahas hal lain yang mungkin belum pernah kamu lihat.

Inilah operator post-increment dan post-decrement yang legendaris:

``` {.c}
i++;        // Tambahkan satu ke i (post-increment)
i--;        // Kurangi i dengan satu (post-decrement)
```

Operator ini sangat umum dipakai sebagai versi singkat dari:

``` {.c}
i += 1;        // Tambahkan satu ke i
i -= 1;        // Kurangi i dengan satu
```

tapi perbedaannya lebih halus dari itu, dasar penipu cerdas.

Sekarang varian lainnya, pre-increment dan pre-decrement:

``` {.c}
++i;        // Tambahkan satu ke i (pre-increment)
--i;        // Kurangi i dengan satu (pre-decrement)
```

Pada pre-increment dan pre-decrement, nilai variabel di-increment atau
di-decrement _sebelum_ ekspresi dievaluasi. Setelah itu, ekspresi dievaluasi
dengan nilai yang baru.

Pada post-increment dan post-decrement, nilai ekspresi dihitung lebih dulu
dengan nilai variabel apa adanya, dan _baru kemudian_ variabelnya
di-increment atau di-decrement.

Kedua operator ini bisa disematkan dalam ekspresi, seperti ini:

``` {.c}
i = 10;
j = 5 + i++;  // Hitung 5 + i, _baru kemudian_ increment i

printf("%d, %d\n", i, j);  // Mencetak 11, 15
```

Bandingkan dengan operator pre-increment:

``` {.c}
i = 10;
j = 5 + ++i;  // Increment i, _baru kemudian_ hitung 5 + i

printf("%d, %d\n", i, j);  // Mencetak 11, 16
```

Teknik ini sering dipakai pada akses dan manipulasi array dan pointer.
Dengannya kamu bisa memakai nilai sebuah variabel sekaligus
meng-increment atau men-decrement nilai itu, sebelum atau sesudah
dipakai.

Tapi sejauh ini, tempat yang paling sering memakainya adalah loop `for`:

``` {.c}
for (i = 0; i < 10; i++)
    printf("i is %d\n", i);
```

Soal itu kita bahas nanti.
\index{++ operator increment@\texttt{++} operator increment|)} \index{-- operator decrement@\texttt{--} operator decrement|)}

### Operator Koma

\index{, operator koma@\texttt{,} operator koma|(}
Ini cara yang jarang dipakai untuk memisahkan ekspresi yang akan
dijalankan dari kiri ke kanan:

``` {.c}
x = 10, y = 20;  // Mula-mula tugaskan 10 ke x, lalu 20 ke y
```

Kelihatannya agak konyol, karena koma itu bisa saja diganti titik koma,
kan?

``` {.c}
x = 10; y = 20;  // Mula-mula tugaskan 10 ke x, lalu 20 ke y
```

Tapi keduanya sedikit berbeda. Yang terakhir berupa dua ekspresi terpisah,
sedangkan yang pertama hanya satu ekspresi tunggal!

Dengan operator koma, nilai ekspresi koma adalah nilai ekspresi yang
paling kanan:

``` {.c}
x = (1, 2, 3);

printf("x is %d\n", x);  // Mencetak 3, karena 3 adalah yang paling kanan di daftar koma
```

Tapi itu pun terasa dipaksakan. Tempat yang umum untuk operator koma adalah
loop `for`, untuk melakukan beberapa hal sekaligus di setiap bagiannya:

``` {.c}
for (i = 0, j = 10; i < 100; i++, j++)
    printf("%d, %d\n", i, j);
```

Kita akan kembali ke sana nanti.
\index{, operator koma@\texttt{,} operator koma|)}

### Operator Kondisional

\index{Operator Kondisional|(}
Untuk nilai Boolean, tersedia banyak operator standar:
\index{operator perbandingan} \index{== operator kesamaan@\texttt{==} operator kesamaan}
\index{"!= operator ketidaksamaan@\texttt{"!=} operator ketidaksamaan} \index{< operator kurang dari@\texttt{<} operator kurang dari}
\index{> operator lebih dari@\texttt{>} operator lebih dari} \index{<= operator kurang dari atau sama dengan@\texttt{<=} operator kurang dari atau sama dengan}
\index{>= operator lebih dari atau sama dengan@\texttt{>=} operator lebih dari atau sama dengan}

``` {.c}
a == b;  // True jika a sama dengan b
a != b;  // True jika a tidak sama dengan b
a < b;   // True jika a kurang dari b
a > b;   // True jika a lebih dari b
a <= b;  // True jika a kurang dari atau sama dengan b
a >= b;  // True jika a lebih dari atau sama dengan b
```

Jangan tertukar antara penugasan (`=`) dan perbandingan (`==`)! Pakai dua
tanda sama dengan untuk membandingkan, satu untuk penugasan.

Ekspresi perbandingan bisa dipakai dalam pernyataan `if`:

``` {.c}
if (a <= 10)
    printf("Success!\n");
```
\index{Operator Kondisional|)}

### Operator Boolean

\index{Operator Boolean|(}
Ekspresi kondisional bisa dirangkai atau diubah dengan operator Boolean
untuk _dan_, _atau_, dan _tidak_.
\index{\&\& boolean AND@\texttt{\&\&} boolean AND}
\index{"! boolean NOT@\texttt{"!} boolean NOT}
\index{\textbar{}\textbar{} boolean OR@\texttt{\textbar{}\textbar{}} boolean OR}

|Operator|Makna Boolean|
|:------:|:-------------:|
|`&&`|and|
|`||`|or|
|`!`|not|

Contoh Boolean "and":

``` {.c}
// Lakukan sesuatu jika x kurang dari 10 dan y lebih dari 20:

if (x < 10 && y > 20)
    printf("Doing something!\n");
```

Contoh Boolean "not":

``` {.c}
if (!(x < 12))
    printf("x is not less than 12\n");
```

`!` punya prioritas (precedence) lebih tinggi daripada operator Boolean lainnya, jadi
dalam kasus ini kita harus memakai tanda kurung.

Tentu saja, itu sama saja dengan:

``` {.c}
if (x >= 12)
    printf("x is not less than 12\n");
```

tapi namanya juga contoh!
\index{Operator Boolean|)}

### Operator `sizeof` {#sizeof-operator}

\index{sizeof operator@\texttt{sizeof} operator|(}
Operator ini memberi tahu ukuran (dalam byte) yang dipakai sebuah variabel
atau tipe data di memori.

Lebih tepatnya, ia memberi tahu ukuran (dalam byte) yang dipakai oleh
_tipe dari suatu ekspresi_ (yang bisa saja hanya satu variabel) di memori.

Ukurannya bisa berbeda di tiap sistem, kecuali untuk `char`\index{char tipe@\texttt{char} tipe}
dan variannya (yang selalu 1 byte).

Mungkin sekarang ini belum terlihat berguna, tapi kita akan sering
merujuknya, jadi layak dibahas.

Karena `sizeof` menghitung jumlah byte yang dibutuhkan untuk menyimpan
sebuah tipe, kamu mungkin mengira ia mengembalikan `int`. Atau... karena
ukuran tidak mungkin negatif, mungkin `unsigned`?

\index{size\_t tipe@\texttt{size\_t} tipe|(}
Ternyata C punya tipe khusus untuk menampung nilai kembalian `sizeof`,
yaitu `size_t`, dibaca "_size tee_"^[`_t` kependekan dari `type`.]. Yang
kita tahu hanyalah bahwa ini tipe integer tak bertanda (unsigned) yang
sanggup menyimpan ukuran dalam byte dari apa pun yang bisa diberikan ke
`sizeof`.

`size_t` muncul di banyak tempat yang melibatkan jumlah sesuatu, baik
sebagai argumen maupun nilai kembalian. Anggap saja ia nilai yang
menyatakan "banyaknya".
\index{size\_t tipe@\texttt{size\_t} tipe|)}

`sizeof` bisa diterapkan pada variabel atau ekspresi:

``` {.c}
int a = 999;

// %zu adalah format specifier untuk tipe size_t
// Jika compiler-mu menolak bagian "z", buang saja

printf("%zu\n", sizeof a);      // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof(2 + 7)); // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof 3.14);   // Mencetak 8 di sistem penulis

// Jika perlu mencetak nilai size_t negatif, pakai %zd
```

Ingat: yang dihasilkan adalah ukuran dalam byte dari _tipe_ ekspresi, bukan
ukuran ekspresi itu sendiri. Itulah sebabnya ukuran `2+7` sama dengan
ukuran `a`---keduanya bertipe `int`. Kita akan kembali ke angka `4` ini di
blok kode berikutnya...

...yang menunjukkan bahwa `sizeof` juga bisa diterapkan pada sebuah tipe
(perhatikan bahwa tanda kurung wajib di sekitar nama tipe, tidak seperti
pada ekspresi):

``` {.c}
printf("%zu\n", sizeof(int));   // Mencetak 4 di sistem penulis
printf("%zu\n", sizeof(char));  // Mencetak 1 di semua sistem
```

Perlu dicatat bahwa `sizeof` adalah operasi _compile-time_^[Kecuali untuk
variable length array---tapi itu cerita untuk lain waktu.]. Hasilnya
ditentukan sepenuhnya saat compile-time, bukan saat runtime.

Kita akan memakainya nanti.
\index{sizeof operator@\texttt{sizeof} operator|)}


## Kendali Alur

\index{Kendali Alur|(}
Boolean memang bagus, tapi kita tidak akan ke mana-mana kalau tidak bisa
mengendalikan alur program. Mari kita lihat beberapa konstruksi: `if`,
`for`, `while`, dan `do-while`.

Pertama, catatan umum tentang pernyataan dan blok pernyataan, dipersembahkan
oleh developer C lokal yang ramah ini:

Setelah pernyataan seperti `if` atau `while`, kamu bisa menaruh satu
pernyataan tunggal untuk dieksekusi, atau sebuah blok pernyataan yang semuanya
dieksekusi berurutan.

\index{if pernyataan@\texttt{if} pernyataan|(}
Kita mulai dengan satu pernyataan:

``` {.c}
if (x == 10) printf("x is 10\n");
```

Ini juga kadang ditulis di baris terpisah. (Whitespace hampir tidak
berpengaruh di C---tidak seperti Python.)

``` {.c}
if (x == 10)
    printf("x is 10\n");
```

Tapi bagaimana kalau kamu ingin beberapa hal terjadi sekaligus karena satu
kondisi? Pakai kurung kurawal untuk menandai sebuah _blok_ atau
_pernyataan majemuk_.

``` {.c}
if (x == 10) {
    printf("x is 10\n");
    printf("And also this happens when x is 10\n");
}
```

Gaya yang sangat umum adalah _selalu_ memakai kurung kurawal, meskipun
sebenarnya tidak perlu:

``` {.c}
if (x == 10) {
    printf("x is 10\n");
}
```

Sebagian developer merasa kodenya lebih mudah dibaca, dan terhindar dari
error seperti ini, ketika baris yang tampak berada di dalam blok `if`
ternyata tidak.

``` {.c}
// CONTOH ERROR BURUK

if (x == 10)
    printf("This happens if x is 10\n");
    printf("This happens ALWAYS\n");  // Kejutan!! Tanpa kondisi!
```

`while`, `for`, dan konstruksi loop lainnya bekerja dengan cara yang sama
seperti contoh di atas. Kalau ingin melakukan beberapa hal dalam loop atau
setelah `if`, bungkus semuanya dalam kurung kurawal.

Dengan kata lain, `if` menjalankan satu hal yang ada setelahnya. Satu hal
itu bisa berupa satu pernyataan atau satu blok pernyataan.
\index{if pernyataan@\texttt{if} pernyataan|)}


### Pernyataan `if`-`else` {#ifstat}

\index{if-else pernyataan@\texttt{if}-\texttt{else} pernyataan|(}
Kita sudah memakai `if` di beberapa contoh, karena kemungkinan besar kamu
sudah pernah melihatnya di bahasa lain, tapi ini satu contoh lagi:

``` {.c}
int i = 10;

if (i > 10) {
    printf("Yes, i is greater than 10.\n");
    printf("And this will also print if i is greater than 10.\n");
}

if (i <= 10) printf("i is less than or equal to 10.\n");
```

Pada kode contoh, pesan dicetak kalau `i` lebih besar dari 10, kalau
tidak eksekusi lanjut ke baris berikutnya. Perhatikan kurung kurawal
setelah pernyataan `if`; kalau kondisinya true, yang dieksekusi adalah
pernyataan tunggal tepat setelah `if`, atau sekumpulan kode dalam kurung
kurawal setelah `if`. Perilaku _blok kode_ seperti ini berlaku untuk semua
pernyataan.

Tentu saja, karena C menyenangkan dengan caranya sendiri, kamu juga bisa
melakukan sesuatu saat kondisinya false lewat klausa `else` pada `if`:

``` {.c}
int i = 99;

if (i == 10)
    printf("i is 10!\n");

else {
    printf("i is decidedly not 10.\n");
    printf("Which irritates me a little, frankly.\n");
}
```

Kamu bahkan bisa merangkainya secara bertingkat untuk menguji berbagai
kondisi, seperti ini:

``` {.c}
int i = 99;

if (i == 10)
    printf("i is 10!\n");

else if (i == 20)
    printf("i is 20!\n");

else if (i == 99) {
    printf("i is 99! My favorite\n");
    printf("I can't tell you how happy I am.\n");
    printf("Really.\n");
}
    
else
    printf("i is some crazy number I've never heard of.\n");
```

Kalau kamu sampai menempuh jalan itu, coba lihat pernyataan
[`switch`](#switch-statement) yang mungkin solusi lebih baik. Syaratnya,
`switch` hanya bisa membandingkan kesamaan dengan angka konstan. Rangkaian
`if`-`else` di atas bisa memeriksa ketidaksamaan, rentang, variabel, atau
apa pun yang bisa dibuat dalam ekspresi kondisional.
\index{if-else pernyataan@\texttt{if}-\texttt{else} pernyataan|)}

### Pernyataan `while` {#whilestat}

\index{while pernyataan@\texttt{while} pernyataan|(}
`while` adalah konstruksi loop biasa yang lazim. Lakukan sesuatu selama
ekspresi kondisinya bernilai true.

Yuk, coba!

``` {.c}
// Cetak output berikut:
//
//   i is now 0!
//   i is now 1!
//   [ sama saja untuk 2 sampai 7 ]
//   i is now 8!
//   i is now 9!

int i = 0;

while (i < 10) {
    printf("i is now %d!\n", i);
    i++;
}

printf("All done!\n");
```

Itulah loop dasar. C juga punya loop `for` yang akan membuat contoh tadi
lebih rapi.

Pemakaian `while` yang cukup sering adalah infinite loop, yaitu mengulang
selama true:

``` {.c}
while (1) {
    printf("1 is always true, so this repeats forever.\n");
}
```
\index{while pernyataan@\texttt{while} pernyataan|)}


### Pernyataan `do`-`while` {#dowhilestat}

\index{do-while pernyataan@\texttt{do}-\texttt{while} pernyataan|(}
Setelah menguasai pernyataan `while`, mari kita lihat saudara dekatnya,
`do`-`while`.

Keduanya pada dasarnya sama, kecuali kalau kondisi loop bernilai false pada
pass pertama: `do`-`while` tetap dieksekusi sekali, sedangkan `while` tidak
dieksekusi sama sekali. Dengan kata lain, pengujian apakah blok akan
dieksekusi terjadi di _akhir_ blok pada `do`-`while`, dan di _awal_ blok
pada `while`.

Kita lihat lewat contoh:

``` {.c}
// Memakai pernyataan while:

i = 10;

// ini tidak dieksekusi karena i tidak kurang dari 10:
while(i < 10) {
    printf("while: i is %d\n", i);
    i++;
}

// Memakai pernyataan do-while:

i = 10;

// ini dieksekusi sekali, karena kondisi loop baru diperiksa setelah
// badan loop dijalankan:

do {
    printf("do-while: i is %d\n", i);
    i++;
} while (i < 10);

printf("All done!\n");
```

Perhatikan bahwa pada kedua kasus, kondisi loop langsung bernilai false.
Pada `while`, kondisinya gagal, sehingga blok kodenya tidak pernah
dieksekusi. Tapi pada `do`-`while`, kondisi diperiksa _setelah_ blok kode
dieksekusi, jadi blok itu selalu berjalan setidaknya sekali. Di sini,
blok mencetak pesan, meng-increment `i`, lalu kondisinya gagal, dan program
lanjut ke output "All done!".

Intinya: kalau kamu ingin loop dieksekusi setidaknya sekali, apa pun
kondisinya, pakai `do`-`while`.

Semua contoh ini sebenarnya lebih cocok memakai loop `for`. Mari kita coba
sesuatu yang kurang deterministik---ulangi sampai muncul bilangan acak
tertentu!

``` {.c .numberLines}
#include <stdio.h>   // Untuk printf
#include <stdlib.h>  // Untuk rand

int main(void)
{
    int r;

    do {
        r = rand() % 100; // Ambil bilangan acak antara 0 dan 99
        printf("%d\n", r);
    } while (r != 37);    // Ulangi sampai 37 muncul
}
```

Catatan sampingan: pernahkah kamu menjalankannya lebih dari sekali? Kalau
ya, apakah kamu melihat urutan angka yang sama muncul lagi. Dan lagi. Dan
lagi? Itu karena `rand()` adalah pseudo-random number generator yang harus
di-_seed_ dengan angka berbeda supaya menghasilkan urutan yang berbeda.
Lihat fungsi
[`srand()`](https://beej.us/guide/bgclr/html/split/stdlib.html#man-srand)^[https://beej.us/guide/bgclr/html/split/stdlib.html#man-srand]
untuk detail lebih lanjut. \index{do-while pernyataan@\texttt{do}-\texttt{while} pernyataan|)}

### Pernyataan `for` {#forstat}

\index{for pernyataan@\texttt{for} pernyataan|(}
Selamat datang di salah satu loop paling populer di dunia: loop `for`!

Loop ini bagus kalau kamu sudah tahu jumlah iterasinya di awal.

Hal yang sama bisa dilakukan hanya dengan loop `while`, tapi loop `for`
membuat kode lebih rapi.

Berikut dua potongan kode yang setara---perhatikan bahwa loop `for` hanyalah
bentuk yang lebih ringkas:

``` {.c}
// Cetak angka antara 0 dan 9, inklusif...

// Memakai pernyataan while:

i = 0;
while (i < 10) {
    printf("i is %d\n", i);
    i++;
}

// Lakukan hal yang persis sama dengan for-loop:

for (i = 0; i < 10; i++) {
    printf("i is %d\n", i);
}
```

Betul, kawan---keduanya melakukan hal yang persis sama. Tapi kamu bisa
lihat sendiri bahwa pernyataan `for` sedikit lebih ringkas dan enak
dipandang. (Pengguna JavaScript pasti paham betul asal-usul C-nya di
titik ini.)

Pernyataan `for` terbagi menjadi tiga bagian yang dipisahkan titik koma.
Bagian pertama berisi inisialisasi, bagian kedua kondisi loop, dan bagian
ketiga apa yang dijalankan di akhir blok kalau kondisi loop bernilai
true. Ketiga bagian ini opsional.

``` {.c}
for (initialize things; loop if this is true; do this after each loop)
```

Perhatikan bahwa loop tidak akan dieksekusi sama sekali kalau kondisi loop
sejak awal bernilai false.

> **Fakta menarik tentang loop `for`!**
>
> Kamu bisa memakai operator koma untuk melakukan beberapa hal di setiap
> klausa loop `for`!
>
> ``` {.c}
> for (i = 0, j = 999; i < 10; i++, j--) {
>     printf("%d, %d\n", i, j);
> }
> ```

<!-- ` -->
`for` yang kosong berjalan selamanya:

``` {.c}
for(;;) {  // "selamanya"
    printf("I will print this again and again and again\n" );
    printf("for all eternity until the heat-death of the universe.\n");

    printf("Or until you hit CTRL-C.\n");
}
```
\index{for pernyataan@\texttt{for} pernyataan|)}

### Pernyataan `switch` {#switch-statement}

\index{switch pernyataan@\texttt{switch} pernyataan|(}
Tergantung bahasa asalmu, kamu mungkin sudah atau belum mengenal `switch`,
atau versi di C ini mungkin lebih terbatas daripada yang biasa kamu pakai.
Pernyataan ini memungkinkan kita mengambil tindakan yang berbeda tergantung
nilai sebuah ekspresi integer.

Pada dasarnya, ekspresi dievaluasi menjadi nilai integer, lalu eksekusi
melompat ke \index{case pernyataan@\texttt{case} pernyataan|(}`case` yang cocok dengan nilai itu, dan
berlanjut dari titik tersebut. Kalau pernyataan `break`\index{break pernyataan@\texttt{break} pernyataan|(} ditemukan, eksekusi melompat keluar dari `switch`.

Berikut contohnya: untuk jumlah kambing tertentu, kita cetak gambaran
seberapa banyak jumlah itu.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int goat_count = 2;

    switch (goat_count) {
        case 0:
            printf("You have no goats.\n");
            break;

        case 1:
            printf("You have a singular goat.\n");
            break;

        case 2:
            printf("You have a brace of goats.\n");
            break;

        default:
            printf("You have a bona fide plethora of goats!\n");
            break;
    }
}
```

Pada contoh itu, `switch` melompat ke `case 2` dan mengeksekusi dari sana.
Ketika (kalau) menemukan `break`, ia melompat keluar dari `switch`.
\index{break pernyataan@\texttt{break} pernyataan|)}

Kamu juga mungkin melihat label `default`\index{default label@\texttt{default} label} di bagian
bawah. Inilah yang dijalankan kalau tidak ada case yang cocok.

Setiap `case`, termasuk `default`, bersifat opsional. Urutannya bebas,
tapi sangat umum `default`, kalau ada, ditaruh paling akhir.\index{case pernyataan@\texttt{case} pernyataan|)}

Jadi, secara keseluruhan ia bekerja seperti rangkaian `if`-`else`:

``` {.c}
if (goat_count == 0)
    printf("You have no goats.\n");
else if (goat_count == 1)
    printf("You have a singular goat.\n");
else if (goat_count == 2)
    printf("You have a brace of goats.\n");
else
    printf("You have a bona fide plethora of goats!\n");
```

Dengan beberapa perbedaan utama:

* `switch` sering kali lebih cepat melompat ke kode yang benar (meski
  spesifikasi tidak menjamin hal itu).
* `if`-`else` bisa memakai kondisional relasional seperti `<` dan `>=`,
  serta floating point dan tipe lainnya, sedangkan `switch` tidak bisa.

Ada satu hal lagi tentang `switch` yang kadang kamu temui dan cukup
menarik: _fall through_.

\index{break pernyataan@\texttt{break} pernyataan|(}
Ingat bagaimana `break` membuat kita melompat keluar dari switch?

\index{Fall through|(}
Nah, apa yang terjadi kalau kita _tidak_ `break`?

Ternyata eksekusi terus masuk ke `case` berikutnya! Demo!

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        // Fall through!
    case 2:
        printf("2\n");
        break;
    case 3:
        printf("3\n");
        break;
}
```

Kalau `x == 1`, `switch` ini mula-mula mengenai `case 1`, mencetak `1`,
lalu langsung lanjut ke baris kode berikutnya... yang mencetak `2`!

Setelah itu, akhirnya kita bertemu `break` dan melompat keluar dari
`switch`.

Kalau `x == 2`, kita hanya mengenai `case 2`, mencetak `2`, dan `break`
seperti biasa.

Ketiadaan `break` inilah yang disebut _fall through_.

Kalau kamu memakai C sebelum C23, _SELALU_ taruh komentar di kode pada
tempat yang memang kamu maksudkan untuk fall through, seperti di atas.
Dengan begitu programmer lain tidak bertanya-tanya apakah kamu sengaja
melakukannya. GCC mengenali komentar ini dan menekan peringatan.

Atau kamu bisa memakai atribut GCC non-standar, yang kurang disukai
karena jelek dan non-standar, tapi dikenali oleh GCC maupun Clang:

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        __attribute__((fallthrough));
    case 2:
        ...
```

Tapi sekarang, akhirnya, di C23 kita punya cara standar lewat atribut:

``` {.c}
switch (x) {
    case 1:
        printf("1\n");
        [[fallthrough]];
    case 2:
        ...
```

Tapi ingat, ini salah satu tempat paling umum munculnya bug dalam program
C: lupa menaruh `break` di `case`. Kamu harus menaruhnya kalau tidak ingin
langsung jatuh ke case berikutnya[^19a1].

[^19a1]: Hal ini dianggap sebagai bahaya yang begitu besar, sampai-sampai
    para perancang bahasa pemrograman Go menjadikan `break` sebagai
    perilaku bawaan; kamu harus secara eksplisit memakai pernyataan
    `fallthrough` di Go kalau ingin jatuh ke case berikutnya.

\index{Fall through|)}
\index{break pernyataan@\texttt{break} pernyataan|)}

Tadi sudah disebut bahwa `switch` bekerja dengan tipe integer---patuhi
itu. Jangan pakai floating point atau tipe string di sana. Satu celahnya:
tipe karakter boleh dipakai karena diam-diam mereka juga integer. Jadi ini
sepenuhnya sah:

``` {.c}
char c = 'b';

switch (c) {
    case 'a':
        printf("It's 'a'!\n");
        break;

    case 'b':
        printf("It's 'b'!\n");
        break;

    case 'c':
        printf("It's 'c'!\n");
        break;
}
```

Terakhir, kamu bisa memakai \index{enum kata kunci@\texttt{enum} kata kunci}`enum` dalam `switch`
karena `enum` juga tipe integer. Soal itu dibahas lebih lanjut di bab
`enum`.

\index{switch pernyataan@\texttt{switch} pernyataan|)} \index{break pernyataan@\texttt{break} pernyataan|)} \index{Kendali Alur|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsi {#functions}

> _"Sir, not in an environment such as this. That's why I've also been
> programmed for over thirty secondary functions that---"_
>
> ---C3PO\index{C3PO}, sebelum diinterupsi dengan kasar, melaporkan jumlah
> fungsi tambahan yang sekarang sudah tidak mengesankan, _Star Wars_ script

\index{Fungsi|(}
Seperti bahasa lain yang sudah kamu pakai, C punya konsep _fungsi_.

Fungsi bisa menerima berbagai _argumen_\index{argumen Fungsi} dan
mengembalikan sebuah nilai. Satu hal penting: tipe argumen dan tipe nilai
kembalian harus dideklarasikan lebih dulu---begitulah cara C menyukainya!

Mari kita lihat sebuah fungsi. Fungsi ini menerima sebuah `int` sebagai
argumen, dan mengembalikan\index{return pernyataan@\texttt{return} pernyataan} sebuah `int`.

``` {.c .numberLines}
#include <stdio.h>

int plus_one(int n)  // "Definisi"-nya
{
    return n + 1;
}
 
```

`int` sebelum `plus_one` menunjukkan tipe kembalian.

`int n` menunjukkan bahwa fungsi ini menerima satu argumen `int`, yang
disimpan dalam _parameter_ `n`\index{parameter Fungsi}. Parameter adalah
variabel lokal khusus yang menjadi tempat argumen disalin.

Poin ini perlu ditegaskan: argumen disalin ke dalam parameter. Banyak hal
di C jadi lebih mudah dipahami kalau kamu ingat bahwa parameter adalah
_salinan_ argumen, bukan argumen itu sendiri. Soal ini sebentar lagi kita
bahas.

Lanjut ke `main()`, di sana ada pemanggilan fungsi, dan nilai
kembaliannya kita tugaskan ke variabel lokal `j`:

``` {.c .numberLines startFrom="8"}
int main(void)
{
    int i = 10, j;
    
    j = plus_one(i);  // "Pemanggilan"-nya

    printf("i + 1 is %d\n", j);
}
```

> Sebelum lupa: perhatikan bahwa fungsi didefinisikan sebelum dipakai.
> Kalau tidak, compiler belum mengenalnya ketika mengompilasi `main()`
> dan akan memberi error pemanggilan fungsi yang tidak dikenal. Ada cara
> yang lebih tepat untuk menulis kode di atas, yaitu dengan _function
> prototype_, tapi itu dibahas nanti.

Perhatikan juga bahwa `main()`\index{main() fungsi@\texttt{main()} fungsi} adalah sebuah fungsi!

Fungsi ini mengembalikan `int`.

Lalu, apa itu `void`\index{void tipe@\texttt{void} tipe}? Kata kunci ini menandakan bahwa
fungsi tidak menerima argumen.

Kamu juga bisa mengembalikan `void` untuk menandakan bahwa fungsi tidak
mengembalikan nilai:

``` {.c .numberLines}
#include <stdio.h>

// Fungsi ini tidak menerima argumen dan tidak mengembalikan nilai:

void hello(void)
{
    printf("Hello, world!\n");
}

int main(void)
{
    hello();  // Mencetak "Hello, world!"
}
```

## Passing by Value {#passvalue}

\index{Pass by value|}Tadi sudah disebutkan bahwa saat sebuah argumen
dioper ke fungsi, salinan argumen itu dibuat dan disimpan dalam parameter
yang sesuai.

Kalau argumennya berupa variabel, yang disalin ke parameter adalah nilai
variabel tersebut.

Lebih umum lagi, seluruh ekspresi argumen dievaluasi dulu untuk
mendapatkan nilainya. Nilai itulah yang disalin ke parameter.

Apa pun yang terjadi, nilai dalam parameter itu berdiri sendiri. Ia tidak
bergantung pada nilai atau variabel apa pun yang kamu pakai sebagai
argumen saat memanggil fungsi.

Mari kita lihat contohnya. Pelajari, dan coba tebak outputnya sebelum
dijalankan:

``` {.c .numberLines}
#include <stdio.h>

void increment(int a)
{
    a++;
}

int main(void)
{
    int i = 10;

    increment(i);

    printf("i == %d\n", i);  // Apa yang dicetak ini?
}
```

Sekilas, `i` bernilai `10`, lalu kita mengopernya ke fungsi `increment()`.
Di sana nilainya di-increment, jadi waktu dicetak pasti `11`, kan?

> _"Get used to disappointment."_
>
> ---Dread Pirate Roberts, _The Princess Bride_

Tapi hasilnya bukan `11`---yang tercetak `10`! Kenapa?

Semuanya berpangkal pada kenyataan bahwa ekspresi yang kamu oper ke fungsi
_disalin_ ke parameter yang sesuai. Parameter itu salinan, bukan yang
aslinya.

Jadi `i` bernilai `10` di dalam `main()`. Lalu kita mengopernya ke
`increment()`. Parameter yang sesuai di dalam fungsi itu bernama `a`.

Penyalinan pun terjadi, seolah-olah lewat penugasan. Kurang lebih,
`a = i`. Pada titik itu `a` bernilai `10`, dan di dalam `main()`, `i` juga
`10`.

Kemudian `a` di-increment menjadi `11`. Tapi `i` sama sekali tidak
tersentuh! Nilainya tetap `10`.

Akhirnya fungsi selesai. Semua variabel lokalnya dibuang (selamat tinggal,
`a`!) dan kita kembali ke `main()`, tempat `i` masih `10`.

Lalu `i` dicetak, hasilnya `10`, dan selesai.

Itulah sebabnya pada contoh `plus_one()` tadi, kita `return` nilai yang
sudah diubah secara lokal supaya bisa dilihat lagi di `main()`.

Kelihatannya cukup membatasi, ya? Sepertinya dari sebuah fungsi kamu hanya
bisa mendapat satu data kembali. Setidaknya itu yang ada di pikiranmu.
Tapi ada cara lain untuk mendapat data kembali; orang C menyebutnya
_passing by reference_, dan itu cerita untuk lain waktu.

Namun, sehebat apa pun namanya, itu tidak mengubah kenyataan bahwa
_SEMUA_ yang kamu oper ke fungsi _TANPA PENGECUALIAN_ disalin ke
parameter yang sesuai, dan fungsi bekerja pada salinan lokal itu, _APA
PUN YANG TERJADI_. Ingat itu, bahkan ketika nanti kita membahas passing
by reference yang konon hebat itu.
\index{Pass by value|)}

## Function Prototype {#prototypes}

\index{Function prototype|(}Kalau kamu ingat, beberapa bagian lalu (rasanya
sudah zaman es) disebutkan bahwa fungsi harus didefinisikan sebelum dipakai,
kalau tidak compiler belum mengenalnya dan akan meledak dengan error.

Itu tidak sepenuhnya benar. Kamu bisa memberi tahu compiler lebih dulu
bahwa kamu akan memakai fungsi bertipe tertentu dengan daftar parameter
tertentu. Dengan begitu, fungsinya bisa didefinisikan di mana saja (bahkan
di file lain), asalkan _function prototype_ sudah dideklarasikan sebelum
fungsinya dipanggil.

Untungnya, function prototype cukup mudah. Ia hanyalah salinan baris
pertama definisi fungsi, ditambah titik koma di akhir. Misalnya, kode
berikut memanggil fungsi yang didefinisikan belakangan, karena
prototype-nya sudah dideklarasikan lebih dulu:

``` {.c .numberLines}
#include <stdio.h>

int foo(void);  // Ini prototype-nya!

int main(void)
{
    int i;
    
    // Kita bisa memanggil foo() di sini sebelum definisinya karena
    // prototype-nya sudah dideklarasikan di atas!

    i = foo();
    
    printf("%d\n", i);  // 3490
}

int foo(void)  // Ini definisinya, persis seperti prototype-nya!
{
    return 3490;
}
```

Kalau fungsi tidak dideklarasikan sebelum dipakai (baik lewat prototype
maupun definisinya), yang terjadi disebut _implicit declaration_\index{Implicit declaration}. Ini dulu diizinkan di standar C pertama (C89), yang punya
aturan khusus untuknya, tapi sekarang tidak lagi diizinkan. Dan tidak ada
alasan sah untuk mengandalkannya di kode baru.

Mungkin kamu menyadari sesuatu pada kode contoh yang sudah kita pakai...
Kita memakai fungsi `printf()` yang setia itu tanpa mendefinisikannya atau
mendeklarasikan prototype-nya! Bagaimana kita bisa lolos dari
pelanggaran hukum ini? Sebenarnya tidak lolos. Prototype-nya ada, yaitu di
header file `stdio.h` yang kita `#include`, ingat? Jadi kita tetap sah,
Pak Polisi!\index{Function prototype|)}

## Daftar Parameter Kosong

\index{Empty parameter lists}
Di C23, daftar parameter kosong dalam definisi atau prototype fungsi
artinya persis sama dengan menulis `void`\index{void tipe@\texttt{void} tipe} di situ.

``` {.c}
void foo();
void foo(void); // Sama saja di C23
```

Meski begitu, kode C zaman sekarang masih cenderung memilih `void`\index{void tipe@\texttt{void} tipe} yang eksplisit di daftar parameter, terutama karena sebelum C23,
perilaku daftar parameter kosong sangat berbeda. Dan bukan perbedaan yang
menyenangkan.

Kita bahas sekarang, tapi singkatnya: kalau kodenya mungkin suatu saat
dibangun dengan compiler pra-C23, **selalu** pakai `void`\index{void tipe@\texttt{void} tipe} untuk menyatakan daftar parameter kosong di semua deklarasi
fungsi.

Ada dua konteks yang perlu dipertimbangkan.

* Menghilangkan semua parameter di tempat fungsi didefinisikan
* Menghilangkan semua parameter dalam sebuah prototype

Kita lihat dulu definisi fungsinya:

``` {.c}
void foo()  // Seharusnya ada `void` di sini
{
    printf("Hello, world!\n");
}
```

Spesifikasi memang menyebut bahwa ini berarti fungsi `foo()` tidak
menerima parameter, tapi compiler boleh saja tidak memperingatkanmu kalau
kamu keliru mengoper argumen. Dan itu undefined behavior. Untuk
menghindari semua itu, cukup tulis `void`\index{void tipe@\texttt{void} tipe} di sana.

Pada function prototype, ada perbedaan yang _signifikan_ antara memakai
`void`\index{void tipe@\texttt{void} tipe!in function prototypes} dan tidak:

``` {.c}
void foo();
void foo(void);  // Tidak sama!
```

Kalau `void` dihilangkan dari prototype, compiler diberi tahu bahwa tidak
ada informasi tambahan tentang parameter fungsi. Ini secara efektif
mematikan semua pengecekan tipe parameter, sehingga `void foo();` pada
dasarnya bukan "prototype" sama sekali. Ia hanyalah forward declaration.

Jadi, pada prototype **pasti** pakai `void` kalau daftar parameternya
kosong.

Dan, ya, itu praktik yang baik dalam semua kasus. Mungkin suatu hari,
ketika semua compiler pra-C23 sudah tidak dipakai lagi, kita bisa
meninggalkannya.

\index{Fungsi|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->
# Pointer---Gemetar Ketakutan! {#pointers}

> _"How do you get to Carnegie Hall?"_ \
> _"Practice!"_
>
> ---20th-century joke of unknown origin

\index{Pointers|(}Pointer adalah salah satu hal yang paling ditakuti di bahasa
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
array. Angka yang menandai setiap kotak disebut _alamat_\index{Memory address}-nya.

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

\index{\& operator address-of@\texttt{\&} operator address-of|(}Dengan kejutan yang pasti mengejutkan kamu,
pembaca yang baik, kita memakai operator `address-of` (yang ternyata
adalah ampersand: "`&`") untuk mencari alamat data. Ampersand.

Sebagai contoh cepat, kita perkenalkan _format specifier_ baru untuk
`printf()` agar bisa mencetak pointer. Kamu sudah tahu `%d` mencetak
integer desimal, kan? Nah, `%p`\index{printf() fungsi@\texttt{printf()} fungsi!with pointers}
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
\index{\& operator address-of@\texttt{\&} operator address-of|)}

Kalau penasaran, angka heksadesimal itu sama dengan 140.727.326.896.068
dalam desimal (basis 10, seperti yang biasa dipakai nenek kita). Itulah
indeks ke memori tempat data variabel `i` disimpan. Itulah alamat `i`.
Itulah lokasi `i`. Itulah pointer ke `i`.

> **Tunggu---RAM kamu 140 terabyte?** Ya! Kamu juga kan? Tapi itu
> bohong besar; tentu saja tidak (sekitar 2024). Komputer modern memakai
> teknologi ajaib bernama [virtual memory](https://en.wikipedia.org/wiki/Virtual_memory)^[https://en.wikipedia.org/wiki/Virtual_memory] yang membuat
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

\index{Pointer types|(}Jadi... semua ini bagus. Sekarang kamu sudah bisa
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
Bizarro.\index{Pointer types|)}

## Dereferencing {#deref}

\index{Dereferencing|(}Variabel pointer bisa dianggap _merujuk_ ke variabel
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

\index{* indirection operator@\texttt{*} indirection operator|(}Pengetahuanmu sudah hampir cukup untuk
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
`*p` menjadi semacam alias bagi `i`.\index{Dereferencing|)}\index{* indirection operator@\texttt{*} indirection operator|)}

Bagus, tapi _kenapa_? Kenapa repot-repot semua ini?

## Mengoper Pointer sebagai Argumen {#ptpass}

\index{Pointers!as arguments|(}Sekarang kamu mungkin merasa sudah punya
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

\index{NULL pointer@\texttt{NULL} pointer|(}Variabel pointer dari tipe pointer apa pun bisa
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

Walaupun penciptanya menyebutnya [kesalahan satu miliar dolar](https://en.wikipedia.org/wiki/Null_pointer#History)^[https://en.wikipedia.org/wiki/Null_pointer#History],
pointer `NULL` adalah [sentinel value](https://en.wikipedia.org/wiki/Sentinel_value)^[https://en.wikipedia.org/wiki/Sentinel_value] yang bagus dan
penanda umum bahwa pointer belum diinisialisasi.

(Tentu saja, seperti variabel lain, pointer akan menunjuk ke sampah kecuali
kamu secara eksplisit menugaskannya untuk menunjuk ke sebuah alamat atau
`NULL`.)
\index{NULL pointer@\texttt{NULL} pointer|)}

## Catatan tentang Mendeklarasikan Pointer

\index{Pointers!declarations|(}Sintaks deklarasi pointer bisa terlihat
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
dan `i`, karena hanya merekalah yang punya `*` di depannya.].\index{Pointers!declarations|)}

## `sizeof` dan Pointer

\index{Pointers!with sizeof@with \texttt{sizeof}|(}Ada sedikit sintaks di sini yang mungkin
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
ekspresi itu sendiri.\index{Pointers!with sizeof@with \texttt{sizeof}|)}\index{Pointers|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Arrays {#arrays}

> _“Should array indices start at 0 or 1?  My compromise of 0.5 was
> rejected without, I thought, proper consideration.”_
>
> ---Stan Kelly-Bootle, computer scientist

\index{Arrays|(}
Untungnya, C punya array. Memang, C dianggap bahasa low-level^[Setidaknya
zaman sekarang.], tapi konsep array sudah tertanam di dalamnya. Dan karena
banyak sekali bahasa yang terinspirasi dari sintaks C, kemungkinan besar
kamu sudah akrab dengan `[` dan `]` untuk mendeklarasikan dan memakai
array.

Tapi C hanya _nyaris_ punya array! Seperti yang akan kita lihat nanti,
array di C hanyalah syntactic sugar---jauh di dalamnya, semuanya adalah
pointer dan semacamnya. _Panik!_ Tapi untuk sekarang, kita pakai saja
sebagai array. _Fiuh_.

## Contoh Mudah

Langsung saja kita buat contohnya:

\index{Arrays!indexing|(}
``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    float f[4];  // Deklarasikan array berisi 4 float

    f[0] = 3.14159;  // Indeks dimulai dari 0, tentu saja.
    f[1] = 1.41421;
    f[2] = 1.61803;
    f[3] = 2.71828;

    // Cetak semuanya:

    for (i = 0; i < 4; i++) {
        printf("%f\n", f[i]);
    }
}
```

Saat mendeklarasikan array, kamu harus menentukan ukurannya. Dan ukuran itu
harus tetap^[Sebenarnya tidak juga, tapi variable-length array---yang
penulis sendiri kurang suka---adalah cerita untuk lain waktu.].

Pada contoh di atas, kita membuat array berisi 4 `float`. Angka di dalam
kurung siku pada deklarasi menunjukkan hal itu.

Di baris-baris berikutnya, kita mengakses nilai di dalam array, baik untuk
mengisi maupun membacanya, lagi-lagi dengan kurung siku.
\index{Arrays!indexing|)}

Semoga ini terlihat akrab dari bahasa-bahasa yang sudah kamu kenal!

## Mendapatkan Panjang Array

\index{Arrays!getting the length|(}
Tidak bisa...sebenarnya. C tidak mencatat informasi ini^[Karena di balik
layar array hanyalah pointer ke elemen pertama array, tidak ada informasi
tambahan yang mencatat panjangnya.]. Kamu harus mengelolanya sendiri di
variabel lain.

Kalau dibilang "tidak bisa", maksudnya ada beberapa keadaan ketika kamu
_bisa_. Ada trik untuk mendapatkan jumlah elemen array di scope tempat
array itu dideklarasikan. Tapi pada umumnya, trik ini tidak akan
berfungsi seperti yang kamu harapkan kalau array dioper ke
fungsi^[Karena ketika sebuah array dioper ke fungsi, yang sebenarnya dioper
hanyalah pointer ke elemen pertama array itu, bukan "seluruh" array.].

Mari kita lihat triknya. Idenya, kita ambil
\index{sizeof operator@\texttt{sizeof} operator!with arrays|(}`sizeof` array itu, lalu dibagi
ukuran tiap elemennya untuk mendapatkan panjangnya. Misalnya, kalau sebuah
`int` berukuran 4 byte dan array-nya berukuran 32 byte, berarti di
dalamnya ada tempat untuk $\frac{32}{4}$ atau $8$ `int`.

``` {.c}
int x[12];  // 12 int

printf("%zu\n", sizeof x);     // total 48 byte
printf("%zu\n", sizeof(int));  // 4 byte per int

printf("%zu\n", sizeof x / sizeof(int));  // 48/4 = 12 int!
```

Kalau array-nya berisi `char`, maka `sizeof` array itu _adalah_ jumlah
elemennya, karena `sizeof(char)` didefinisikan sebagai 1. Untuk tipe
lainnya, kamu harus membaginya dengan ukuran tiap elemen.

Tapi trik ini hanya berfungsi di scope tempat array didefinisikan. Kalau
array dioper ke fungsi, trik ini gagal. Bahkan kalau ukurannya ditulis
"besar" di signature fungsi:

``` {.c}
void foo(int x[12])
{
    printf("%zu\n", sizeof x);     // 8?! Ke mana perginya 48?
    printf("%zu\n", sizeof(int));  // 4 byte per int

    printf("%zu\n", sizeof x / sizeof(int));  // 8/4 = 2 int?? SALAH.
}
```

Ini terjadi karena ketika array "dioper" ke fungsi, yang dioper hanyalah
pointer ke elemen pertama, dan itulah yang diukur `sizeof`. Pembahasan
lebih lanjut ada di bagian [Mengoper Array Satu Dimensi ke
Fungsi](#passing1darrays) di bawah.

Satu hal lagi yang bisa dilakukan dengan `sizeof` dan array: mengetahui
ukuran array dengan jumlah elemen tertentu tanpa mendeklarasikan array-nya.
Ini seperti mengetahui ukuran `int` lewat `sizeof(int)`.

Misalnya, untuk mengetahui berapa byte yang dibutuhkan array berisi 48
`double`, tulis begini:

``` {.c}
sizeof(double [48]);
```
\index{sizeof operator@\texttt{sizeof} operator!with arrays|)}
\index{Arrays!getting the length|)}

## Array Initializer

\index{Array initializers|(}
Array bisa diinisialisasi lebih dulu dengan konstanta:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    int a[5] = {22, 37, 3490, 18, 95};  // Inisialisasi dengan nilai-nilai ini

    for (i = 0; i < 5; i++) {
        printf("%d\n", a[i]);
    }
}
```

Jangan pernah memasukkan item di initializer lebih banyak daripada tempat
yang tersedia di array, kalau tidak compiler akan ngambek:

``` {.zsh}
foo.c: In function ‘main’:
foo.c:6:39: warning: excess elements in array initializer
    6 |     int a[5] = {22, 37, 3490, 18, 95, 999};
      |                                       ^~~
foo.c:6:39: note: (near initialization for ‘a’)
```

Tapi (fakta menarik!) item di initializer boleh _lebih sedikit_ daripada
tempat yang tersedia di array. Elemen sisanya otomatis diinisialisasi
dengan nol. Ini berlaku umum untuk semua jenis array initializer: kalau
ada initializer, semua yang tidak diberi nilai secara eksplisit akan
bernilai nol.

``` {.c}
int a[5] = {22, 37, 3490};

// sama dengan:

int a[5] = {22, 37, 3490, 0, 0};
```

Bentuk pintas yang sering dipakai untuk mengisi seluruh array dengan nol:

``` {.c}
int a[100] = {0};
```

Artinya, "Buat elemen pertama bernilai nol, lalu secara otomatis buat sisanya
nol juga."

Kamu juga bisa mengisi elemen tertentu di initializer dengan menyebutkan
indeksnya! Kalau kamu melakukan ini, C dengan senang hati melanjutkan
pengisian nilai berikutnya sampai initializer habis, dan mengisi sisanya
dengan `0`.

Caranya, taruh indeks di dalam kurung siku diikuti `=`, lalu tulis
nilainya.

Berikut contoh pembuatan array:

``` {.c}
int a[10] = {0, 11, 22, [5]=55, 66, 77};
```

Karena kita menetapkan indeks 5 sebagai tempat awal `55`, data hasil di
dalam array adalah:

``` {.default}
0 11 22 0 0 55 66 77 0 0
```

Ekspresi konstan sederhana juga boleh dipakai di sana.

``` {.c}
#define COUNT 5

int a[COUNT] = {[COUNT-3]=3, 2, 1};
```

yang menghasilkan:

``` {.default}
0 0 3 2 1
```

Terakhir, kamu juga bisa menyuruh C menghitung ukuran array dari
initializer-nya, cukup dengan tidak menulis ukurannya:

``` {.c}
int a[3] = {22, 37, 3490};

// sama dengan:

int a[] = {22, 37, 3490};  // Ukurannya tidak ditulis!
```
\index{Array initializers|)}

## Di Luar Batas!

\index{Arrays!out of bounds|(}
C tidak melarang kamu mengakses array di luar batasnya. Bahkan mungkin tidak
ada peringatan sama sekali.

Kita pinjam contoh di atas dan terus mencetak melewati ujung array. Array
ini hanya punya 5 elemen, tapi mari coba cetak 10 dan lihat apa yang
terjadi:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    int a[5] = {22, 37, 3490, 18, 95};

    for (i = 0; i < 10; i++) {  // KABAR BURUK: mencetak terlalu banyak elemen!
        printf("%d\n", a[i]);
    }
}
```

Di komputer penulis, hasilnya:

``` {.default}
22
37
3490
18
95
32765
1847052032
1780534144
-56487472
21890
```

Waduh! Apa itu? Ternyata mencetak melewati ujung array menghasilkan apa
yang disebut developer C sebagai _undefined behavior_. Makhluk ini akan kita
bahas lebih lanjut nanti, tapi untuk sekarang artinya: "Kamu sudah
melakukan hal buruk, dan apa pun bisa terjadi selama programmu berjalan."

Dan "apa pun" di sini biasanya berarti menemukan angka nol, menemukan
angka sampah, atau crash. Tapi sebenarnya spesifikasi C menyatakan bahwa
dalam keadaan ini compiler boleh menghasilkan kode yang melakukan
_apa saja_^[Pada masa MS-DOS dulu, sebelum ada proteksi memori, penulis
pernah menulis kode C yang sangat kasar dan sengaja melakukan segala macam
undefined behavior. Tapi penulis tahu apa yang dilakukan, dan semuanya
berjalan cukup lancar. Sampai suatu langkah keliru menyebabkan komputer
hang dan, seperti yang ketahuan saat reboot, menghapus semua pengaturan
BIOS. Seru sekali. (Salam untuk @man atas masa-masa seru itu.)].

Singkatnya: jangan pernah melakukan apa pun yang menyebabkan undefined
behavior. Pernah^[Banyak hal yang menyebabkan undefined behavior, bukan
hanya akses array di luar batas. Inilah yang membuat bahasa C begitu
_menarik_.].
\index{Arrays!out of bounds|)}

## Array Multidimensi

\index{Arrays!multidimensional|(}
Kamu boleh menambahkan dimensi sebanyak apa pun pada array.

``` {.c}
int a[10];
int b[2][7];
int c[4][5][6];
```

Array ini disimpan di memori dengan [row-major
order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)^[https://en.wikipedia.org/wiki/Row-_and_column-major_order]. Artinya, pada array 2D, indeks pertama
menunjukkan baris, dan indeks kedua menunjukkan kolom.

Array multidimensi juga bisa diberi initializer dengan cara menyarangkannya:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int row, col;

    int a[2][5] = {      // Inisialisasi array 2D
        {0, 1, 2, 3, 4},
        {5, 6, 7, 8, 9}
    };

    for (row = 0; row < 2; row++) {
        for (col = 0; col < 5; col++) {
            printf("(%d,%d) = %d\n", row, col, a[row][col]);
        }
    }
}
```

Outputnya:

``` {.default}
(0,0) = 0
(0,1) = 1
(0,2) = 2
(0,3) = 3
(0,4) = 4
(1,0) = 5
(1,1) = 6
(1,2) = 7
(1,3) = 8
(1,4) = 9
```

Dan kamu juga bisa memakai indeks eksplisit:

``` {.c}
// Buat matriks identitas 3x3

int a[3][3] = {[0][0]=1, [1][1]=1, [2][2]=1};
```

yang membangun array 2D seperti ini:

``` {.default}
1 0 0
0 1 0
0 0 1
```
\index{Arrays!multidimensional|)}

## Array dan Pointer

\index{Arrays!as pointers|(}
[_Sambil lalu_] Nah... tadi di atas mungkin ada disinggung bahwa array itu
pointer, jauh di dalamnya? Sekarang kita menyelam sebentar di perairan
dangkal supaya semuanya tidak benar-benar membingungkan. Nanti kita
bahas hubungan sebenarnya antara array dan pointer, tapi untuk saat ini
kita hanya akan melihat cara mengoper array ke fungsi.

### Mendapatkan Pointer ke Array

Ada rahasia yang perlu disampaikan. Secara umum, ketika programmer C
berbicara tentang pointer ke array, yang mereka maksud adalah pointer
_ke elemen pertama_ array^[Ini secara teknis tidak tepat, karena pointer
ke array dan pointer ke elemen pertama array punya tipe yang berbeda. Tapi
soal itu kita urus nanti saat sudah sampai.].

Jadi mari kita ambil pointer ke elemen pertama sebuah array.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a[5] = {11, 22, 33, 44, 55};
    int *p;

    p = &a[0];  // p menunjuk ke array
                // Tepatnya, ke elemen pertama

    printf("%d\n", *p);  // Mencetak "11"
}
```

Karena begitu sering dipakai di C, bahasanya menyediakan jalan pintas:

``` {.c .numberLines}
p = &a[0];  // p menunjuk ke array

// sama dengan:

p = a;      // p menunjuk ke array, tapi tampilannya jauh lebih enak!
```

Menyebut nama array saja, tanpa embel-embel, sama dengan mengambil pointer
ke elemen pertama array! Ini akan sering kita pakai di contoh-contoh
berikutnya.

Tapi tunggu sebentar---bukankah `p` itu `int*`? Dan `*p` menghasilkan
`11`, sama seperti `a[0]`? Yessss. Kamu mulai melihat sekilas hubungan
antara array dan pointer di C. (Kita akan membahasnya jauh lebih dalam di
bab [Pointer II](#pointers2), di bagian [Kesetaraan
Array/Pointer](#arraypointerequiv).)
\index{Arrays!as pointers|)}

### Mengoper Array Satu Dimensi ke Fungsi {#passing1darrays}

\index{Arrays!passing to functions|(}
Kita coba contoh dengan array satu dimensi. Penulis akan menulis dua
fungsi yang bisa menerima array dan melakukan hal berbeda.

Bersiaplah untuk signature fungsi yang bikin takjub!

``` {.c .numberLines}
#include <stdio.h>

// Dioper sebagai pointer ke elemen pertama
void times2(int *a, int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 2);
}

// Sama saja, tapi memakai notasi array
void times3(int a[], int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 3);
}

// Sama saja, tapi memakai notasi array dengan ukuran
void times4(int a[5], int len)
{
    for (int i = 0; i < len; i++)
        printf("%d\n", a[i] * 4);
}

int main(void)
{
    int x[5] = {11, 22, 33, 44, 55};

    times2(x, 5);
    times3(x, 5);
    times4(x, 5);
}
```

Semua cara menuliskan array sebagai parameter fungsi tersebut identik.

``` {.c}
void times2(int *a, int len)
void times3(int a[], int len)
void times4(int a[5], int len)
```

Di kalangan pemakai C yang sudah berpengalaman, bentuk pertama jauh lebih
umum.

Bahkan pada bentuk terakhir, compiler tidak peduli angka berapa yang kamu
tulis (selain harus lebih besar dari nol^[C11 §6.7.6.2¶1 mensyaratkan
angkanya lebih besar dari nol. Tapi kamu mungkin menemukan kode yang
mendeklarasikan array berukuran nol di akhir `struct`, dan GCC cukup longgar
soal ini kecuali kamu mengompilasi dengan `-pedantic`. Array berukuran nol
ini dulu adalah mekanisme akal-akalan untuk membuat struktur dengan ukuran
variabel. Sayangnya, secara teknis mengakses array seperti itu adalah
undefined behavior, meskipun di mana-mana praktis berfungsi. C99
menetapkan pengganti yang terdefinisi dengan baik, yang disebut _flexible
array member_, dan akan kita bahas nanti.]). Compiler sama sekali tidak
memaksakan apa pun.

Setelah mengatakan itu, ukuran array di deklarasi fungsi sebenarnya
_penting_ ketika kita mengoper array multidimensi ke fungsi, tapi nanti
kita kembali ke sana.
\index{Arrays!passing to functions|)}

### Mengubah Array di Dalam Fungsi

\index{Arrays!modifying within functions|(}
Kita sudah bilang bahwa array hanyalah pointer yang menyamar. Artinya,
kalau kamu mengoper array ke fungsi, kemungkinan besar yang kamu oper
adalah pointer ke elemen pertama array.

Tapi kalau fungsi punya pointer ke data, fungsi itu bisa mengubah data
tersebut! Jadi perubahan yang dilakukan fungsi pada array akan terlihat
juga di caller.

Berikut contoh ketika kita mengoper pointer ke array ke sebuah fungsi,
fungsi itu mengubah nilai-nilai di array, dan perubahannya terlihat di
caller.

``` {.c .numberLines}
#include <stdio.h>

void double_array(int *a, int len)
{
    // Kalikan setiap elemen dengan 2
    //
    // Ini menggandakan nilai di x pada main() karena x dan a sama-sama menunjuk
    // ke array yang sama di memori!

    for (int i = 0; i < len; i++)
        a[i] *= 2;
}

int main(void)
{
    int x[5] = {1, 2, 3, 4, 5};

    double_array(x, 5);

    for (int i = 0; i < 5; i++)
        printf("%d\n", x[i]);  // 2, 4, 6, 8, 10!
}
```

Walaupun array dioper sebagai parameter `a` yang bertipe `int*`, lihat
bagaimana kita mengaksesnya dengan notasi array `a[i]`! Haaah. Ini sepenuhnya
diperbolehkan.

Nanti ketika kita membahas kesetaraan antara array dan pointer, hal ini
akan jauh lebih masuk akal. Untuk sekarang, cukup ketahui bahwa fungsi bisa
mengubah array dan perubahannya terlihat di caller.
\index{Arrays!modifying within functions|)}

### Mengoper Array Multidimensi ke Fungsi

\index{Arrays!passing to functions|(}
Ceritanya sedikit berubah ketika kita membicarakan array multidimensi. C
perlu tahu semua dimensi (kecuali yang pertama) supaya punya cukup
informasi untuk mengetahui di mana di memori nilai tertentu harus dicari.

Berikut contoh ketika kita eksplisit menyebutkan semua dimensi:

``` {.c .numberLines}
#include <stdio.h>

void print_2D_array(int a[2][3])
{
    for (int row = 0; row < 2; row++) {
        for (int col = 0; col < 3; col++)
            printf("%d ", a[row][col]);
        printf("\n");
    }
}

int main(void)
{
    int x[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };

    print_2D_array(x);
}
```

Tapi dalam kasus ini, kedua bentuk^[Ini juga setara: `void
print_2D_array(int (*a)[3])`, tapi itu sudah melebihi yang ingin dibahas
sekarang.] berikut setara:

``` {.c}
void print_2D_array(int a[2][3])
void print_2D_array(int a[][3])
```

Compiler sebenarnya hanya butuh dimensi kedua supaya tahu seberapa jauh
harus melompat di memori untuk setiap kenaikan dimensi pertama. Secara umum,
compiler harus tahu semua dimensi kecuali yang pertama.

Ingat juga bahwa compiler hanya melakukan pengecekan batas saat kompilasi
secara minimal (kalau beruntung), dan C sama sekali tidak mengecek batas
saat runtime. Tanpa sabuk pengaman! Jangan sampai crash karena mengakses
elemen array di luar batas!
\index{Arrays!passing to functions|)} \index{Arrays|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Strings

\index{Strings|(}Akhirnya! Strings! Apa yang bisa lebih sederhana dari ini?

Nah, ternyata string di C sebenarnya bukan string. Betul! Mereka itu
pointer! Tentu saja mereka pointer!

Seperti array, string di C _nyaris tidak ada_.

Tapi kita lihat saja---sebenarnya tidak seseram itu.

## String Literal

\index{String literals|(}Sebelum mulai, mari kita bahas dulu string literal di
C. String literal berupa deretan karakter di dalam tanda kutip _ganda_ (`"`). (Tanda
kutip tunggal mengapit karakter, dan itu makhluk yang sama sekali
berbeda.)

Contoh:

``` {.c}
"Hello, world!\n"
"This is a test."
"When asked if this string had quotes in it, she replied, \"It does.\""
```

Yang pertama punya newline di ujungnya---pemandangan yang sangat umum.

Yang terakhir punya tanda kutip di dalamnya, dan kamu bisa lihat masing-masing
didahului (kita bilang "di-escape oleh") sebuah backslash (`\`) yang
menandakan bahwa di titik itu ada tanda kutip literal yang menjadi bagian
string. Begitulah cara C compiler membedakan antara mencetak tanda kutip
ganda dan tanda kutip ganda penutup string.\index{String literals|)}

## Variabel String

\index{String variables|(}Setelah tahu cara membuat string literal, sekarang kita
tugaskan ke sebuah variabel supaya bisa kita pakai.

``` {.c}
char *s = "Hello, world!";
```

Perhatikan tipenya: pointer ke `char`. Variabel string `s` sebenarnya
adalah pointer ke karakter pertama string itu, yaitu `H`.

Kita bisa mencetaknya dengan format specifier `%s` (untuk "string"):

``` {.c}
char *s = "Hello, world!";

printf("%s\n", s);  // "Hello, world!"
```
\index{String variables|)}

## Variabel String sebagai Array

\index{String variables!as arrays|(}
Pilihan lainnya adalah ini, yang hampir setara dengan pemakaian `char*`
di atas:

``` {.c}
char s[14] = "Hello, world!";

// atau, kalau kita malas dengan benar dan menyuruh compiler
// menghitung panjangnya untuk kita:

char s[] = "Hello, world!";
```

Artinya, kamu bisa memakai notasi array untuk mengakses karakter dalam
string. Mari kita lakukan persis itu untuk mencetak semua karakter string
dalam satu baris:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[] = "Hello, world!";

    for (int i = 0; i < 13; i++)
        printf("%c", s[i]);
    printf("\n");
}
```

Perhatikan bahwa kita memakai format specifier `%c` untuk mencetak satu
karakter.

Lihat juga ini. Programnya tetap berjalan dengan baik kalau definisi `s`
kita ubah menjadi tipe `char*`:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char *s = "Hello, world!";   // char* di sini

    for (int i = 0; i < 13; i++)
        printf("%c", s[i]);    // Tapi masih memakai array di sini...?
    printf("\n");
}
```

Dan kita masih bisa memakai notasi array untuk mencetaknya! Ini mengejutkan,
tapi itu hanya karena kita belum membahas kesetaraan array/pointer. Meski
begitu, ini satu petunjuk lagi bahwa array dan pointer itu sama saja, jauh
di dalamnya.
\index{String variables!as arrays|)}

## Inisialisasi String

\index{Strings!initializers|(}
Kita sudah melihat beberapa contoh inisialisasi variabel string dengan
string literal:

``` {.c}
char *s = "Hello, world!";
char t[] = "Hello, again!";
```

Tapi kedua inisialisasi ini berbeda secara halus. Sebuah string literal,
seperti integer literal, memorinya dikelola otomatis oleh compiler untukmu!
Untuk integer, yaitu data berukuran tetap, compiler bisa mengelolanya dengan
mudah. Tapi string adalah makhluk berukuran byte yang bervariasi, yang
dijinakkan compiler dengan cara dilempar ke sepotong memori, lalu kamu
diberi pointer ke sana.

Bentuk ini menunjuk ke mana pun string itu ditaruh. Biasanya tempat itu
berada di negeri yang jauh dari memori program lainnya -- yaitu memori
read-only -- karena alasan performa dan keamanan.

``` {.c}
char *s = "Hello, world!";
```

Jadi, kalau kamu mencoba mengubah string itu dengan ini:

``` {.c}
char *s = "Hello, world!";

s[0] = 'z';  // KABAR BURUK: mencoba mengubah string literal!
```

Perilakunya undefined. Tergantung sistemmu, kemungkinan besar hasilnya
crash.

Tapi mendeklarasikannya sebagai array itu berbeda. Compiler tidak
menyembunyikan byte-byte itu di bagian kota yang lain; mereka ada di
jalan seberang rumahmu. Yang satu ini _salinan_ string yang bisa diubah
-- bisa kita ubah sesuka hati:

``` {.c}
char t[] = "Hello, again!";  // t adalah salinan array dari string itu
t[0] = 'z'; //  Tidak masalah

printf("%s\n", t);  // "zello, again!"
```

Jadi ingat: kalau kamu punya pointer ke string literal, jangan coba
mengubahnya! Dan kalau kamu memakai string di dalam tanda kutip ganda
untuk menginisialisasi array, itu sebenarnya bukan string literal.
\index{Strings!initializers|)}

## Mendapatkan Panjang String

\index{Strings!getting the length|(}
Tidak bisa, karena C tidak mencatatnya untukmu. Dan kalau dibilang "tidak
bisa", maksudnya sebenarnya "bisa"^[Walaupun memang benar bahwa C tidak
mencatat panjang string.]. Ada fungsi di `<string.h>` bernama `strlen()`
yang bisa dipakai untuk menghitung panjang string apa pun dalam
byte^[Kalau kamu memakai character set dasar atau character set 8-bit,
kamu terbiasa dengan satu karakter sama dengan satu byte. Tapi itu tidak
berlaku di semua character encoding.].

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char *s = "Hello, world!";

    printf("The string is %zu bytes long.\n", strlen(s));
}
```

Fungsi `strlen()` mengembalikan tipe `size_t`, yaitu tipe integer sehingga
bisa dipakai untuk perhitungan integer. Kita mencetak `size_t` dengan
`%zu`.

Program di atas mencetak:

``` {.default}
The string is 13 bytes long.
```

Bagus! Jadi ternyata _bisa_ mendapatkan panjang string!
\index{Strings!getting the length|)}

Tapi... kalau C tidak mencatat panjang string di mana pun, bagaimana ia
tahu seberapa panjang string itu?

## Terminasi String

\index{Strings!termination|(}
C memperlakukan string sedikit berbeda dari banyak bahasa pemrograman, dan
sebenarnya berbeda dari hampir semua bahasa pemrograman modern.

Saat membuat bahasa baru, pada dasarnya ada dua pilihan untuk menyimpan
string di memori:

1. Simpan byte-byte string beserta sebuah angka yang menyatakan panjang
   string.

2. Simpan byte-byte string, dan tandai akhir string dengan byte khusus
   yang disebut _terminator_.

Kalau kamu ingin string yang lebih panjang dari 255 karakter, pilihan 1
membutuhkan setidaknya dua byte untuk menyimpan panjangnya. Sedangkan
pilihan 2 hanya butuh satu byte untuk mengakhiri string. Jadi ada sedikit
penghematan di sana.

Tentu saja, zaman sekarang menghemat satu byte (atau 3---banyak bahasa
dengan senang hati membolehkan string sepanjang 4 gigabyte) terasa konyol.
Tapi dulu, itu urusan yang lebih besar.

Jadi C memilih pendekatan #2. Di C, sebuah "string" didefinisikan oleh dua
ciri dasar:

* Sebuah pointer ke karakter pertama string.
* Sebuah byte bernilai nol (atau karakter `NUL`^[Ini berbeda dari pointer
  `NULL`, dan penulis menyingkatnya `NUL` kalau membicarakan karakternya
  dan `NULL` untuk pointer-nya.]) di suatu tempat di memori setelah pointer
  itu, yang menandai akhir string.

Karakter `NUL` bisa ditulis di kode C sebagai `\0`, meski jarang kamu
perlu melakukannya.

Ketika kamu menulis string di dalam tanda kutip ganda di kodemu, karakter
`NUL` otomatis dan implisit ikut disertakan.

``` {.c}
char *s = "Hello!";  // Sebenarnya "Hello!\0" di balik layar
```

Dengan ini, mari kita tulis fungsi `strlen()` kita sendiri yang menghitung
`char` dalam string sampai menemukan `NUL`.

Caranya, telusuri string untuk mencari satu karakter `NUL`, sambil
menghitung^[Nanti kita akan belajar cara yang lebih rapi dengan pointer
arithmetic.]:

``` {.c}
int my_strlen(char *s)
{
    int count = 0;

    while (s[count] != '\0')  // Tanda kutip tunggal untuk satu karakter
        count++;

    return count;
}
```

Dan pada dasarnya begitulah cara `strlen()` bawaan menyelesaikan tugasnya.
\index{Strings!termination|)}

## Menyalin String

\index{Strings!copying|(}
String tidak bisa disalin lewat operator penugasan (`=`). Yang terjadi
hanyalah menyalin pointer ke karakter pertama... sehingga akhirnya kamu
punya dua pointer ke string yang sama:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[] = "Hello, world!";
    char *t;

    // Ini menyalin pointer-nya, bukan menyalin string-nya!
    t = s;

    // Kita ubah t
    t[0] = 'z';

    // Tapi mencetak s menampilkan perubahannya!
    // Karena t dan s menunjuk ke string yang sama!

    printf("%s\n", s);  // "zello, world!"
}
```

Kalau kamu ingin menyalin string, kamu harus menyalinnya byte demi
byte---artinya kamu mengambil byte-byte string dari satu tempat di memori
dan menduplikasinya ke tempat lain di memori. Ini dipermudah oleh fungsi
`strcpy()`.[^440b]

[^440b]: Ada fungsi lain bernama `strncpy()` yang membatasi jumlah byte yang
    disalin. Sebagian orang bilang kamu harus selalu memakai `strncpy()`
    karena ada perlindungan dari buffer overrun. Sebagian lain bilang
    jangan pernah memakai `strncpy()` karena ia belum tentu mengakhiri
    string-mu, footgun mengerikan lainnya. Kalau mau benar-benar aman,
    kamu bisa menulis versi `strncpy()` sendiri yang selalu mengakhiri
    string.

Sebelum menyalin string, pastikan ada ruang untuk menampung salinannya,
yaitu array tujuan yang akan menampung karakter-karakternya harus
setidaknya sepanjang string yang disalin.

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[] = "Hello, world!";
    char t[100];  // Setiap char satu byte, jadi ruangnya lebih dari cukup

    // Ini menyalin string-nya!
    strcpy(t, s);

    // Kita ubah t
    t[0] = 'z';

    // Dan s tidak terpengaruh karena itu string yang berbeda
    printf("%s\n", s);  // "Hello, world!"

    // Tapi t sudah berubah
    printf("%s\n", t);  // "zello, world!"
}
```

Perhatikan bahwa pada `strcpy()`, pointer tujuan adalah argumen pertama,
dan pointer sumber adalah argumen kedua. Trik untuk mengingatnya: ini
urutan yang akan kamu pakai untuk menulis `t` dan `s` seandainya
penugasan `=` bekerja untuk string, dengan sumber di kanan dan tujuan di
kiri.
\index{Strings!copying|)}\index{Strings|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Structs {#structs}

\index{struct keyword@\texttt{struct} keyword|(}
Di C ada yang disebut `struct`, yaitu tipe yang bisa didefinisikan sendiri
oleh pengguna dan menyimpan beberapa data sekaligus, yang bisa saja
bertipe berbeda-beda.

Ini cara praktis untuk membungkus beberapa variabel menjadi satu. Dengan
begitu, mengoper variabel ke fungsi jadi lebih ringkas (cukup satu yang
dioper, bukan banyak), data lebih rapi, dan kode lebih mudah dibaca.

Kalau kamu berasal dari bahasa lain, mungkin kamu sudah akrab dengan
konsep _class_ dan _object_. Keduanya tidak ada di C secara
bawaan^[Walaupun di C setiap item di memori seperti `int` disebut
"object", itu bukan object dalam arti pemrograman berorientasi objek.].
Kamu bisa membayangkan `struct` sebagai class yang hanya punya data member,
tanpa method.

## Mendeklarasikan Struct

\index{struct keyword@\texttt{struct} keyword!declaring|(}
Sebuah `struct` bisa dideklarasikan di kodemu seperti ini:

``` {.c}
struct car {
    char *name;
    float price;
    int speed;
};
```

Ini sering dilakukan di scope global, di luar semua fungsi, supaya
`struct` itu tersedia secara global.

Dengan begitu, kamu membuat sebuah _tipe_ baru. Nama lengkap tipenya
adalah `struct car`. (Bukan sekadar `car`---itu tidak akan berfungsi.)

Belum ada variabel bertipe itu, tapi kita bisa mendeklarasikannya:

``` {.c}
struct car saturn;  // Variabel "saturn" bertipe "struct car"
```

Sekarang kita punya variabel `saturn`^[Saturn dulu adalah merek mobil
ekonomi yang populer di Amerika Serikat, sampai akhirnya gulung tikar
akibat krisis 2008, sayangnya bagi kami para penggemarnya.] yang belum
diinisialisasi, bertipe `struct car`.

Sebaiknya kita inisialisasi! Tapi bagaimana cara mengisi nilai
masing-masing field-nya?

Seperti di banyak bahasa lain yang mencurinya dari C, kita memakai
operator titik (`.`) untuk mengakses tiap field.

``` {.c}
saturn.name = "Saturn SL/2";
saturn.price = 15999.99;
saturn.speed = 175;

printf("Name:           %s\n", saturn.name);
printf("Price (USD):    %f\n", saturn.price);
printf("Top Speed (km): %d\n", saturn.speed);
```

Di baris-baris awal, kita mengisi nilai-nilai di `struct car`, lalu di
bagian berikutnya kita mencetak nilai-nilai itu.
\index{struct keyword@\texttt{struct} keyword!declaring|)}

## Struct Initializer {#struct-initializers}

\index{struct keyword@\texttt{struct} keyword!initializers|(}
Contoh di bagian sebelumnya agak merepotkan. Pasti ada cara yang lebih baik
untuk menginisialisasi variabel `struct` itu!

Kamu bisa memakai initializer dengan mengisi nilai untuk field-field-nya
_sesuai urutan kemunculannya di `struct`_ saat mendefinisikan variabel.
(Ini tidak bisa dilakukan setelah variabelnya didefinisikan---harus
terjadi saat definisi.)

``` {.c}
struct car {
    char *name;
    float price;
    int speed;
};

// Sekarang dengan initializer! Urutan field sama seperti di deklarasi struct:
struct car saturn = {"Saturn SL/2", 16000.99, 175};

printf("Name:      %s\n", saturn.name);
printf("Price:     %f\n", saturn.price);
printf("Top Speed: %d km\n", saturn.speed);
```

Fakta bahwa field di initializer harus berurutan sama itu agak
menyeramkan. Kalau ada yang mengubah urutan di `struct car`, semua kode
lain bisa rusak!

Kita bisa lebih spesifik dengan initializer:

``` {.c}
struct car saturn = {.speed=175, .name="Saturn SL/2"};
```

Sekarang initializer tidak bergantung pada urutan di deklarasi `struct`.
Kode seperti ini jelas lebih aman.

Seperti pada array initializer, field designator yang tidak disebutkan akan
diinisialisasi dengan nol (di sini yaitu `.price`, yang sengaja dihilangkan).
\index{struct keyword@\texttt{struct} keyword!initializers|)}

## Mengoper Struct ke Fungsi

\index{struct keyword@\texttt{struct} keyword!passing and returning|(}
Ada dua cara untuk mengoper `struct` ke fungsi.

1. Oper `struct`-nya.
2. Oper pointer ke `struct`-nya.

Ingat bahwa ketika sesuatu dioper ke fungsi, sebuah _salinan_ dari sesuatu
itu dibuat untuk dikerjakan oleh fungsi, baik itu salinan pointer, `int`,
`struct`, atau apa pun.

Pada dasarnya ada dua keadaan ketika kamu ingin mengoper pointer ke
`struct`:

1. Fungsi perlu bisa mengubah `struct` yang dioper, dan perubahannya harus
   terlihat di caller.
2. `struct`-nya cukup besar sehingga menyalinnya ke stack lebih mahal
   daripada menyalin sebuah pointer^[Pointer kemungkinan berukuran 8 byte
   di sistem 64-bit.].

Karena dua alasan itu, jauh lebih umum mengoper pointer ke `struct` ke
fungsi, walaupun mengoper `struct`-nya langsung sama sekali tidak
dilarang.

Mari kita coba mengoper pointer, dengan membuat fungsi yang memungkinkan
kita mengisi field `.price` dari `struct car`:

``` {.c .numberLines}
#include <stdio.h>

struct car {
    char *name;
    float price;
    int speed;
};

int main(void)
{
    struct car saturn = {.speed=175, .name="Saturn SL/2"};

    // Oper pointer ke struct car ini, beserta harga baru yang
    // lebih realistis:
    set_price(&saturn, 799.99);

    printf("Price: %f\n", saturn.price);
}
```

Kamu seharusnya bisa menyusun signature `set_price()` hanya dengan
melihat tipe argumen yang kita punya di sana.

`saturn` adalah `struct car`, jadi `&saturn` pasti alamat dari `struct
car`, alias pointer ke `struct car`, yaitu `struct car*`.

Dan `799.99` adalah `float`.

Jadi deklarasi fungsinya pasti seperti ini:

``` {.c}
void set_price(struct car *c, float new_price)
```

Tinggal menulis badannya. Satu percobaan mungkin begini:

``` {.c}
void set_price(struct car *c, float new_price) {
    c.price = new_price;  // ERROR!!
}
```

Itu tidak akan berhasil karena operator titik hanya bekerja pada
`struct`... bukan pada _pointer_ ke `struct`.

Oke, jadi kita bisa men-dereference variabel `c` untuk "de-pointer"-nya
agar sampai ke `struct`-nya. Men-dereference `struct car*` menghasilkan
`struct car` yang ditunjuk pointer itu, yang seharusnya bisa kita beri
operator titik:

``` {.c}
void set_price(struct car *c, float new_price) {
    (*c).price = new_price;  // Berfungsi, tapi jelek dan tidak idiomatis :(
}
```

Dan itu berhasil! Tapi mengetik semua tanda kurung dan asterisk itu agak
merepotkan. C punya syntactic sugar bernama _arrow operator_ yang
membantu soal ini.
\index{struct keyword@\texttt{struct} keyword!passing and returning|)}

## Arrow Operator

\index{-> arrow operator@\texttt{->} arrow operator|(}
Arrow operator membantu merujuk ke field pada pointer ke `struct`.

``` {.c}
void set_price(struct car *c, float new_price) {
    // (*c).price = new_price;  // Berfungsi, tapi tidak idiomatis :(
    //
    // Baris di atas 100% setara dengan baris di bawah:

    c->price = new_price;  // Nah, yang ini!
}
```

Jadi saat mengakses field, kapan memakai titik dan kapan memakai panah?

* Kalau kamu punya `struct`, pakai titik (`.`).
* Kalau kamu punya pointer ke `struct`, pakai panah (`->`).
\index{-> arrow operator@\texttt{->} arrow operator|)}

## Menyalin dan Mengembalikan `struct`

\index{struct keyword@\texttt{struct} keyword!copying|(}
Yang ini gampang!

Tinggal tugaskan satu ke yang lain!

``` {.c}
struct car a, b;

b = a;  // Salin struct-nya
```

Mengembalikan `struct` (bukan pointer ke `struct`) dari sebuah fungsi juga
membuat salinan serupa ke variabel penerimanya.

Ini bukan "deep copy"^[_Deep copy_ mengikuti pointer di dalam `struct` dan
ikut menyalin data yang ditunjuknya. _Shallow copy_ hanya menyalin
pointer-nya, bukan hal yang ditunjuk. C tidak punya fungsionalitas deep
copy bawaan.]. Semua field disalin apa adanya, termasuk pointer ke
sesuatu.
\index{struct keyword@\texttt{struct} keyword!copying|)}

## Membandingkan `struct`

\index{struct keyword@\texttt{struct} keyword!comparing|(}
Hanya ada satu cara yang aman: bandingkan setiap field satu per satu.

Mungkin kamu mengira bisa memakai
[`memcmp()`](https://beej.us/guide/bgclr/html/split/stringref.html#man-strcmp)^[https://beej.us/guide/bgclr/html/split/stringref.html#man-strcmp],
tapi itu tidak menangani kemungkinan adanya [padding
byte](#struct-padding-bytes) di dalamnya.

Kalau `struct` dikosongkan dulu dengan nol memakai
[`memset()`](https://beej.us/guide/bgclr/html/split/stringref.html#man-memset)^[https://beej.us/guide/bgclr/html/split/stringref.html#man-memset],
hasilnya _mungkin_ berhasil, tapi masih ada elemen aneh yang
[mungkin tidak dibandingkan seperti yang
kamu harapkan](https://stackoverflow.com/questions/141720/how-do-you-compare-structs-for-equality-in-c)^[https://stackoverflow.com/questions/141720/how-do-you-compare-structs-for-equality-in-c].
\index{struct keyword@\texttt{struct} keyword!comparing|)} \index{struct keyword@\texttt{struct} keyword|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Input/Output File

\index{File I/O|(}
Kita sudah melihat beberapa contoh I/O dengan `printf()` untuk melakukan
I/O di konsol.

Tapi di bab ini kita akan mendorong konsep-konsep itu sedikit lebih jauh.

## Tipe Data `FILE*`

\index{FILE* type@\texttt{FILE*} type|(}
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

\index{stdin standard input@\texttt{stdin} standard input|(}
\index{stdout standard output@\texttt{stdout} standard output|(}
\index{stderr standard error@\texttt{stderr} standard error|(}

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
\index{stdin standard input@\texttt{stdin} standard input|)}
\index{stdout standard output@\texttt{stdout} standard output|)}
\index{stderr standard error@\texttt{stderr} standard error|)}

Cara melakukannya dibahas lebih lanjut nanti.
\index{FILE* type@\texttt{FILE*} type|(}

## Membaca File Teks

\index{File I/O!text files, reading|(}
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

\index{fopen() function@\texttt{fopen()} function|(}
\index{fgetc() function@\texttt{fgetc()} function|(}
\index{fclose() function@\texttt{fclose()} function|(}
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
\index{fopen() function@\texttt{fopen()} function|)}

Setelah itu, kita memakai fungsi `fgetc()` untuk mengambil satu karakter
dari stream. Mungkin kamu bertanya-tanya kenapa `c` dibuat `int` dan bukan
`char`---tahan dulu pikiran itu!
\index{fgetc() function@\texttt{fgetc()} function|)}

Terakhir, kita menutup stream saat selesai memakainya. Semua stream
otomatis ditutup saat program keluar, tapi menutup sendiri setiap file
yang sudah selesai dipakai itu sopan santun dan kebiasaan rumah tangga
yang baik.
\index{fclose() function@\texttt{fclose()} function|)}

\index{FILE* type@\texttt{FILE*} type}`FILE*` melacak posisi kita di dalam file. Jadi
pemanggilan `fgetc()` berikutnya akan mengambil karakter berikutnya dalam
file, lalu berikutnya lagi, sampai akhir.

Tapi itu terdengar merepotkan. Mari kita lihat apakah bisa dipermudah.
\index{File I/O!text files, reading|)}

## Akhir File: `EOF`

\index{EOF end of file@\texttt{EOF} end of file|(}
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

\index{fopen() function@\texttt{fopen()} function|(}
\index{fgetc() function@\texttt{fgetc()} function|(}
\index{fclose() function@\texttt{fclose()} function|(}
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
\index{fopen() function@\texttt{fopen()} function|)}
\index{fclose() function@\texttt{fclose()} function|)}

(Kalau baris 10 terlalu aneh, uraikan saja mulai dari tanda kurung yang
paling dalam. Hal pertama yang dilakukan adalah menugaskan hasil
`fgetc()` ke `c`, dan _baru kemudian_ membandingkan _nilai itu_ dengan
`EOF`. Kita hanya menjejalkannya dalam satu baris. Mungkin kelihatan
sulit dibaca, tapi pelajari---ini idiomatis di C.)
\index{fgetc() function@\texttt{fgetc()} function|)}

Dan saat dijalankan, kita lihat:

``` {.default}
Hello, world!
```

Tapi tetap saja kita bekerja satu karakter demi satu karakter, padahal
banyak file teks lebih masuk akal dibaca per baris. Mari beralih ke itu.
\index{EOF end of file@\texttt{EOF} end of file|)}

### Membaca Satu Baris Sekaligus

\index{File I/O!line by line|(}
Jadi bagaimana mendapat satu baris utuh sekaligus? \index{fgets() function@\texttt{fgets()} function|(}`fgets()` datang menyelamatkan! Sebagai argumen, ia menerima
pointer ke buffer `char` untuk menampung byte, jumlah byte maksimum yang
dibaca, dan `FILE*` tempat membaca. Ia mengembalikan `NULL` saat
end-of-file atau error. `fgets()` bahkan cukup baik hati untuk mengakhiri
string dengan NUL saat selesai^[Kalau buffer tidak cukup besar untuk
membaca satu baris penuh, ia akan berhenti membaca di tengah baris, dan
pemanggilan `fgets()` berikutnya akan melanjutkan sisa barisnya.].\index{fgets() function@\texttt{fgets()} function|)}

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

\index{fgets() function@\texttt{fgets()} function|(}
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
\index{fgets() function@\texttt{fgets()} function|)}

Yang memberi output:

``` {.default}
1: A wise man can learn more from
2: a foolish question than a fool
3: can learn from a wise answer.
4:                   --Bruce Lee
```
\index{File I/O!line by line|)}

## Input Terformat

\index{File I/O!formatted input|(}
Kamu tahu bagaimana kamu bisa mendapat output terformat dengan `printf()`
(dan, dengan demikian, `fprintf()` seperti yang akan kita lihat di
bawah)?

\index{fscanf() function@\texttt{fscanf()} function|(}
Hal yang sama bisa dilakukan dengan `fscanf()`.

> Sebelum mulai, perlu diingatkan bahwa memakai fungsi gaya `scanf()` bisa
> berbahaya untuk input yang tidak tepercaya. Kalau kamu tidak menentukan
> lebar field pada `%s`, buffer bisa meluap. Lebih buruk lagi, konversi
> numerik yang tidak valid menghasilkan undefined behavior. Cara aman untuk
> input yang tidak tepercaya adalah memakai `%s` dengan lebar field, lalu
> memakai fungsi seperti \index{strtol function@\texttt{strtol} function} `strtol()` atau
> \index{strtod function@\texttt{strtod} function} `strtod()` untuk melakukan konversinya.

Kita siapkan file berisi serangkaian record data. Dalam hal ini, paus,
dengan nama, panjang dalam meter, dan bobot dalam ton. `whales.txt`:

``` {.default}
blue 29.9 173
right 20.7 135
gray 14.9 41
humpback 16.0 30
```

Ya, kita bisa membacanya dengan \index{fgets() function@\texttt{fgets()} function}`fgets()` lalu
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
\index{fscanf() function@\texttt{fscanf()} function|)}

Yang memberi hasil:

``` {.default}
blue whale, 173 tonnes, 29.9 meters
right whale, 135 tonnes, 20.7 meters
gray whale, 41 tonnes, 14.9 meters
humpback whale, 30 tonnes, 16.0 meters
```
\index{File I/O!formatted input|)}

## Menulis File Teks

\index{File I/O!text files, writing|(}
\index{fputc() function@\texttt{fputc()} function|(}
\index{fputs() function@\texttt{fputs()} function|(}
\index{fprintf() function@\texttt{fprintf()} function|(}
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
\index{fputc() function@\texttt{fputc()} function|)}
\index{fputs() function@\texttt{fputs()} function|)}
\index{fprintf() function@\texttt{fprintf()} function|)}

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
\index{File I/O!text files, writing|)}

## I/O File Biner

\index{File I/O!binary files|(}
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

\index{fwrite() function@\texttt{fwrite()} function|(}
Sebagai gantinya, fungsi yang paling umum adalah \index{fread() function@\texttt{fread()} function}`fread()` dan `fwrite()`. Fungsi-fungsi ini membaca dan menulis
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
\index{fwrite() function@\texttt{fwrite()} function|)}

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

Kalau kita jalankan lewat program [hex dump](https://en.wikipedia.org/wiki/Hex_dump)^[https://en.wikipedia.org/wiki/Hex_dump], kita bisa
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

\index{fread() function@\texttt{fread()} function|(}
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
\index{fread() function@\texttt{fread()} function|)}

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
\index{File I/O!binary files|)}

### Peringatan tentang `struct` dan Angka

\index{File I/O!with structs@with \texttt{struct}s|(}
Seperti yang kita lihat di bagian `struct`, compiler bebas menambahkan
padding pada `struct` sesuai kehendaknya. Dan compiler yang berbeda
mungkin melakukannya secara berbeda. Dan compiler yang sama pada arsitektur
berbeda bisa melakukannya secara berbeda. Dan compiler yang sama pada
arsitektur yang sama pun bisa melakukannya secara berbeda.

Intinya: tidak portabel kalau langsung `fwrite()` seluruh `struct` ke
file ketika kamu tidak tahu di mana padding akan berakhir.
\index{File I/O!with structs@with \texttt{struct}s|)}

Bagaimana cara memperbaikinya? Tahan dulu pikiran itu---kita akan melihat
beberapa caranya setelah membahas masalah lain yang terkait.

\index{File I/O!with numeric values|(}
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
\index{Endianess}[_endianess_](https://en.wikipedia.org/wiki/Endianess)^[https://en.wikipedia.org/wiki/Endianess] arsitektur. Sebagian menulis
byte paling signifikan lebih dulu, dan sebagian menulis byte paling tidak
signifikan lebih dulu.

Artinya, kalau kamu menulis angka multibyte langsung dari memori, kamu
tidak bisa melakukannya secara portabel^[Dan inilah alasan penulis
dengan cerdik memakai byte individual pada contoh `fwrite()` dan
`fread()` di atas.].

Masalah serupa ada pada floating point. Kebanyakan sistem memakai format
yang sama untuk bilangan floating point, tapi sebagian tidak. Tidak ada
jaminan!

\index{File I/O!with structs@with \texttt{struct}s|(}
Jadi... bagaimana kita memperbaiki semua masalah angka dan `struct` ini
supaya data kita tertulis secara portabel?

Ringkasnya adalah dengan men-\index{Data serialization}_serialize_ data, yaitu
istilah umum yang berarti mengambil semua data dan menuliskannya dalam
format yang kamu kendalikan, yang terkenal, dan bisa diprogram agar
bekerja sama di semua platform.

Seperti yang mungkin kamu duga, ini masalah yang sudah terpecahkan. Ada
banyak library serialisasi yang bisa kamu manfaatkan, seperti
[_protocol buffers_](https://en.wikipedia.org/wiki/Protocol_buffers)^[https://en.wikipedia.org/wiki/Protocol_buffers] dari Google, yang sudah tersedia
dan siap dipakai. Mereka akan mengurus semua detail rumitnya, bahkan
memungkinkan data dari programmu di C berinteroperasi dengan bahasa lain
yang mendukung metode serialisasi yang sama.

Lakukan kebaikan untuk dirimu dan semua orang! Serialize data binermu saat
menulisnya ke stream! Ini akan menjaga semuanya tetap rapi dan portabel,
bahkan kalau kamu memindahkan file data dari satu arsitektur ke
arsitektur lain.
\index{File I/O!with structs@with \texttt{struct}s|)}
\index{File I/O!with numeric values|)}
\index{File I/O|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `typedef`: Membuat Tipe Baru

\index{typedef keyword@\texttt{typedef} keyword|(}
Sebenarnya bukan membuat tipe _baru_, melainkan memberi nama baru pada
tipe yang sudah ada. Sekilas kedengarannya sia-sia, tapi ini bisa kita
manfaatkan untuk membuat kode lebih bersih.

## `typedef` dalam Teori

Pada dasarnya, kamu mengambil tipe yang sudah ada lalu membuat alias untuk
tipe itu dengan `typedef`.

Seperti ini:

``` {.c}
typedef int antelope;  // Jadikan "antelope" alias untuk "int"

antelope x = 10;       // Tipe "antelope" sama dengan tipe "int"
```

Kamu bisa melakukannya pada tipe apa pun yang sudah ada. Kamu bahkan bisa
membuat beberapa tipe sekaligus dengan daftar yang dipisahkan koma:

``` {.c}
typedef int antelope, bagel, mushroom;  // Semuanya "int"
```

Itu sangat berguna, kan? Bisa mengetik `mushroom` alih-alih `int`? Kamu
pasti _sangat bersemangat_ dengan fitur ini!

Oke, Profesor Sarkasme---sebentar lagi kita lihat penerapan yang lebih
umum.

### Scoping

\index{typedef keyword@\texttt{typedef} keyword!scoping rules|(}
`typedef` mengikuti [aturan scope](#scope) biasa.

Karena itu, sangat umum menemukan `typedef` di file scope ("global")
supaya semua fungsi bisa memakai tipe baru itu sesuka hati.

## `typedef` dalam Praktik

Jadi mengganti nama `int` menjadi sesuatu yang lain memang tidak terlalu
menarik. Mari kita lihat di mana `typedef` biasanya muncul.

### `typedef` dan `struct` {#typedef-struct}

\index{typedef keyword@\texttt{typedef} keyword!with structs@with \texttt{struct}s|(}
Kadang sebuah `struct` di-`typedef` menjadi nama baru supaya kita tidak
perlu mengetik kata `struct` berulang-ulang.

``` {.c}
struct animal {
    char *name;
    int leg_count, speed;
};

//  nama asli          nama baru
//            |         |
//            v         v
//      |-----------| |----|
typedef struct animal animal;

struct animal y;  // Ini berfungsi
animal z;         // Ini juga berfungsi karena "animal" adalah alias
```

Secara pribadi, penulis tidak menyukai praktik ini. Penulis suka
kejelasan yang didapat kode ketika kata `struct` ditambahkan ke tipe;
programmer tahu apa yang mereka dapat. Tapi ini sangat umum, jadi tetap
dimasukkan di sini.

Sekarang kita jalankan contoh yang persis sama dengan cara yang mungkin
sering kamu lihat. Kita akan menaruh `struct animal` _di dalam_ `typedef`.
Semuanya bisa digabung seperti ini:

``` {.c}
//  nama asli
//            |
//            v
//      |-----------|
typedef struct animal {
    char *name;
    int leg_count, speed;
} animal;                         // <-- nama baru

struct animal y;  // Ini berfungsi
animal z;         // Ini juga berfungsi karena "animal" adalah alias
```

Itu persis sama dengan contoh sebelumnya, hanya lebih ringkas.

\index{typedef keyword@\texttt{typedef} keyword!with anonymous structs@with anonymous \texttt{struct}s|(}
Tapi itu belum semua! Ada jalan pintas umum lain yang mungkin kamu lihat di
kode yang memakai apa yang disebut _struktur anonim_^[Soal ini dibahas
lebih lanjut nanti.]. Ternyata di berbagai tempat kita tidak perlu memberi
nama pada struktur, dan `typedef` adalah salah satunya.

Kita buat contoh yang sama dengan struktur anonim:

``` {.c}
//  struct anonim! Tidak punya nama!
//         |
//         v
//      |----|
typedef struct {
    char *name;
    int leg_count, speed;
} animal;                         // <-- nama baru

//struct animal y;  // ERROR: ini tidak berfungsi lagi--tidak ada struct itu!
animal z;           // Ini berfungsi karena "animal" adalah alias
```

Sebagai contoh lain, kita mungkin menemukan sesuatu seperti ini:

``` {.c}
typedef struct {
    int x, y;
} point;

point p = {.x=20, .y=40};

printf("%d, %d\n", p.x, p.y);  // 20, 40
```
\index{typedef keyword@\texttt{typedef} keyword!with anonymous structs@with anonymous \texttt{struct}s|)}
\index{typedef keyword@\texttt{typedef} keyword!with structs@with \texttt{struct}s|)}
\index{typedef keyword@\texttt{typedef} keyword!scoping rules|)}

### `typedef` dan Tipe Lain

Bukan berarti memakai `typedef` pada tipe sederhana seperti `int` sama
sekali tidak berguna... ia membantu mengabstraksikan tipe agar lebih mudah
diubah belakangan.

Misalnya, kalau `float` tersebar di 100 zilyun tempat di kodemu, akan
menyakitkan mengubah semuanya menjadi `double` kalau belakangan kamu harus
melakukannya karena suatu alasan.

Tapi kalau kamu sudah bersiap sedikit dengan:

``` {.c}
typedef float app_float;

// dan

app_float f1, f2, f3;
```

Maka kalau belakangan kamu ingin beralih ke tipe lain, seperti `long
double`, kamu cukup mengubah `typedef`-nya:

``` {.c}
//        voila!
//      |---------|
typedef long double app_float;

// dan tidak perlu mengubah baris ini:

app_float f1, f2, f3;  // Sekarang semuanya long double
```

### `typedef` dan Pointer

\index{typedef keyword@\texttt{typedef} keyword!with pointers|(}
Kamu bisa membuat tipe yang berupa pointer.

``` {.c}
typedef int *intptr;

int a = 10;
intptr x = &a;  // "intptr" bertipe "int*"
```

Penulis benar-benar tidak menyukai praktik ini. Ia menyembunyikan fakta
bahwa `x` bertipe pointer karena tidak ada `*` di deklarasinya.

Menurut penulis, lebih baik secara eksplisit menunjukkan bahwa kamu
mendeklarasikan tipe pointer supaya developer lain bisa melihatnya dengan
jelas dan tidak keliru mengira `x` bukan tipe pointer.

Tapi menurut hitungan terakhir, katakanlah, 832.007 orang punya pendapat
berbeda.
\index{typedef keyword@\texttt{typedef} keyword!with pointers|)}

### `typedef` dan Kapitalisasi

Penulis pernah melihat segala macam kapitalisasi pada `typedef`.

``` {.c}
typedef struct {
    int x, y;
} my_point;          // snake case huruf kecil

typedef struct {
    int x, y;
} MyPoint;          // CamelCase

typedef struct {
    int x, y;
} Mypoint;          // Huruf besar di awal

typedef struct {
    int x, y;
} MY_POINT;          // SNAKE CASE HURUF BESAR
```

Spesifikasi C11 tidak menentukan salah satunya, dan menunjukkan contoh
dalam huruf besar semua maupun huruf kecil semua.

K&R2 terutama memakai huruf besar di awal, tapi menunjukkan beberapa
contoh dalam huruf besar dan snake case (dengan `_t`).

Kalau kamu punya style guide yang dipakai, ikuti saja. Kalau tidak punya,
pilih satu dan ikuti.

## Array dan `typedef`

\index{typedef keyword@\texttt{typedef} keyword!with arrays|(}
Sintaksnya agak aneh, dan menurut pengalaman penulis ini jarang terlihat,
tapi kamu bisa me-`typedef` array berisi sejumlah item.

``` {.c}
// Jadikan tipe five_ints sebagai array berisi 5 int
typedef int five_ints[5];

five_ints x = {11, 22, 33, 44, 55};
```

Penulis tidak menyukainya karena ia menyembunyikan sifat array dari
variabelnya, tapi itu mungkin dilakukan.
\index{typedef keyword@\texttt{typedef} keyword!with arrays|)}
\index{typedef keyword@\texttt{typedef} keyword|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Pointer II: Aritmetika {#pointers2}

\index{Pointers!arithmetic|(}

Saatnya menyelam lebih dalam dengan sejumlah topik pointer baru! Kalau
kamu belum paham betul soal pointer, [lihat dulu bagian pertama panduan
ini tentang topik tersebut](#pointers).

## Aritmetika Pointer

Ternyata pointer bisa dioperasikan secara matematis, terutama penjumlahan
dan pengurangan.

Tapi apa artinya kalau itu dilakukan?

Singkatnya, kalau kamu punya pointer ke suatu tipe, menambahkan satu pada
pointer itu akan memindahkannya ke item bertipe sama yang berada tepat
setelahnya di memori.

**Penting** untuk diingat bahwa saat kita menggeser pointer dan melihat
tempat-tempat berbeda di memori, kita harus selalu memastikan pointer itu
menunjuk ke tempat yang valid sebelum di-dereference. Kalau kita nyasar
entah ke mana lalu mencoba melihat isinya, perilakunya undefined, dan
crash adalah hasil yang umum.

Ini agak seperti masalah ayam-dan-telur dengan [Kesetaraan Array/Pointer,
di bawah](#arraypointerequiv), tapi kita coba saja.

### Menambah Pointer

Pertama, kita ambil sebuah array berisi angka.

``` {.c}
int a[5] = {11, 22, 33, 44, 55};
```

Lalu kita ambil pointer ke elemen pertama array itu:

``` {.c}
int a[5] = {11, 22, 33, 44, 55};

int *p = &a[0];  // Atau "int *p = a;" sama saja hasilnya
```

Lalu kita cetak nilai di sana dengan men-dereference pointer-nya:

``` {.c}
printf("%d\n", *p);  // Mencetak 11
```

Sekarang kita pakai aritmetika pointer untuk mencetak elemen berikutnya di
array, yaitu yang berindeks 1:

``` {.c}
printf("%d\n", *(p + 1));  // Mencetak 22!!
```

Apa yang terjadi di situ? C tahu bahwa `p` adalah pointer ke `int`. Jadi ia
tahu `sizeof` sebuah `int`^[Ingat, operator `sizeof` memberi tahu ukuran
sebuah object di memori dalam byte.] dan tahu harus melompati sebanyak itu
byte untuk sampai ke `int` setelah yang pertama!

Bahkan, contoh sebelumnya bisa ditulis dengan dua cara yang setara:

``` {.c}
printf("%d\n", *p);        // Mencetak 11
printf("%d\n", *(p + 0));  // Mencetak 11
```

karena menambahkan `0` pada pointer menghasilkan pointer yang sama.

Mari kita pikirkan manfaatnya. Kita bisa menelusuri elemen array dengan
cara ini, tanpa memakai array:

``` {.c}
int a[5] = {11, 22, 33, 44, 55};

int *p = &a[0];  // Atau "int *p = a;" sama saja hasilnya

for (int i = 0; i < 5; i++) {
    printf("%d\n", *(p + i));  // Sama dengan p[i]!
}
```

Dan hasilnya sama seperti kalau kita memakai notasi array! Oooo! Makin
dekat ke soal kesetaraan array/pointer itu! Nanti dibahas lebih lanjut di
bab ini.

Tapi sebenarnya apa yang terjadi di sini? Bagaimana cara kerjanya?

Ingat dari awal tadi bahwa memori itu seperti array besar, tempat sebuah
byte disimpan di setiap indeks array?

Dan indeks array ke dalam memori itu punya beberapa nama:

* Indeks ke dalam memori
* Lokasi
* Alamat
* _Pointer!_

Jadi pointer adalah indeks ke dalam memori, di suatu tempat.

Sebagai contoh acak, katakanlah angka 3490 disimpan di alamat ("indeks")
23.237.489.202. Kalau kita punya pointer `int` ke 3490 itu, nilai pointer
itu adalah 23.237.489.202... karena pointer adalah alamat memori. Kata
berbeda untuk hal yang sama.

Sekarang katakanlah ada angka lain, 4096, yang disimpan tepat setelah 3490
di alamat 23.237.489.210 (lebih tinggi 8 dari 3490 karena setiap `int`
dalam contoh ini berukuran 8 byte).

Kalau kita tambahkan `1` pada pointer itu, ia sebenarnya melompat maju
sejauh `sizeof(int)` byte ke `int` berikutnya. Ia tahu harus melompat
sejauh itu karena ia pointer `int`. Kalau ia pointer `float`, ia akan
melompat sejauh `sizeof(float)` byte untuk sampai ke float berikutnya!

Jadi kamu bisa melihat `int` berikutnya dengan menambahkan `1` pada
pointer, yang sesudahnya dengan menambahkan `2`, dan seterusnya.

### Mengubah Pointer

Tadi kita sudah melihat bahwa sebuah integer bisa ditambahkan ke pointer.
Sekarang, mari kita _ubah pointer itu sendiri_.

Nilai integer bisa langsung ditambahkan ke (atau dikurangkan dari) pointer
mana pun!

Kita ulangi contoh tadi dengan beberapa perubahan. Pertama, kita tambahkan
`999` di akhir deretan angka sebagai sentinel value. Ini memberi tahu kita
di mana data berakhir.

``` {.c}
int a[] = {11, 22, 33, 44, 55, 999};  // Tambahkan 999 di sini sebagai sentinel

int *p = &a[0];  // p menunjuk ke 11
```

Dan `p` menunjuk ke elemen berindeks `0` dari `a`, yaitu `11`, persis
seperti tadi.

Sekarang---kita mulai _meng-increment_ `p` supaya menunjuk ke
elemen-elemen array berikutnya. Kita lakukan ini sampai `p` menunjuk ke
`999`; artinya, sampai `*p == 999`:

``` {.c}
while (*p != 999) {       // Selama yang ditunjuk p bukan 999
    printf("%d\n", *p);   // Cetak
    p++;                  // Geser p supaya menunjuk ke int berikutnya!
}
```

Gila, kan?

Kalau dijalankan, mula-mula `p` menunjuk ke `11`. Lalu `p` di-increment,
dan ia menunjuk ke `22`, lalu sekali lagi, ia menunjuk ke `33`. Begitu
seterusnya, sampai ia menunjuk ke `999` dan kita berhenti.

### Mengurangi Pointer

\index{Pointers!subtracting|(}
Kamu bisa mengurangi pointer dengan sebuah nilai untuk sampai ke alamat
yang lebih awal, sama seperti tadi kita menambahkannya.

Tapi kita juga bisa mengurangkan dua pointer untuk mengetahui selisihnya,
misalnya untuk menghitung berapa banyak `int` yang ada di antara dua
`int*`. Syaratnya, ini hanya berlaku di dalam satu array^[Atau string, yang
sebenarnya array berisi `char`. Agak aneh, kamu juga boleh punya pointer
yang merujuk ke _satu elemen setelah_ ujung array tanpa masalah dan tetap
bisa menghitungnya. Hanya saja, pointer itu tidak boleh di-dereference
selagi berada di sana.]---kalau pointer-nya menunjuk ke hal lain, hasilnya
undefined behavior.

Ingat bahwa string di C adalah `char*`? Mari kita lihat apakah ini bisa
dipakai untuk menulis varian lain dari `strlen()` untuk menghitung panjang
string dengan pengurangan pointer.

Idenya: kalau kita punya pointer ke awal string, kita bisa mencari pointer
ke akhir string dengan menelusuri maju sampai menemukan karakter `NUL`.

Dan kalau kita punya pointer ke awal string, serta sudah menghitung
pointer ke akhir string, kita tinggal mengurangkan kedua pointer itu untuk
mendapatkan panjangnya!

``` {.c .numberLines}
#include <stdio.h>

int my_strlen(char *s)
{
    // Mulai menelusuri dari awal string
    char *p = s;

    // Telusuri sampai menemukan karakter NUL
    while (*p != '\0')
        p++;

    // Kembalikan selisih pointer
    return p - s;
}

int main(void)
{
    printf("%d\n", my_strlen("Hello, world!"));  // Mencetak "13"
}
```

Ingat, pengurangan pointer hanya boleh dilakukan antara dua pointer yang
menunjuk ke array yang sama!
\index{Pointers!subtracting|)}

## Kesetaraan Array/Pointer {#arraypointerequiv}

\index{Pointers!array equivalence|(}
Akhirnya kita siap membahas ini! Kita sudah melihat banyak contoh tempat
kita mencampur notasi array, tapi sekarang kita sampaikan _rumus dasar
kesetaraan array/pointer_:

``` {.c}
a[b] == *(a + b)
```

Pelajari itu! Keduanya setara dan bisa dipakai bergantian!

Ini sedikit disederhanakan, karena pada contoh di atas `a` dan `b`
sama-sama bisa berupa ekspresi, dan mungkin kita perlu beberapa tanda
kurung tambahan untuk memaksa urutan operasi kalau ekspresinya rumit.

Spesifikasi, seperti biasa, tegas soal ini, menyatakan (di C11
§6.5.2.1¶2):

> `E1[E2]` is identical to `(*((E1)+(E2)))`

tapi itu agak lebih sulit dicerna. Pokoknya pastikan kamu menambahkan
tanda kurung kalau ekspresinya rumit, supaya semua perhitunganmu terjadi
dalam urutan yang benar.

Artinya, kita bisa _memutuskan_ mau memakai notasi array atau notasi
pointer untuk array atau pointer apa pun (dengan asumsi pointer-nya
menunjuk ke elemen sebuah array).

Mari kita pakai sebuah array dan pointer dengan notasi array maupun
notasi pointer:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a[] = {11, 22, 33, 44, 55};

    int *p = a;  // p menunjuk ke elemen pertama a, yaitu 11

    // Cetak semua elemen array dengan berbagai cara:

    for (int i = 0; i < 5; i++)
        printf("%d\n", a[i]);      // Notasi array dengan a

    for (int i = 0; i < 5; i++)
        printf("%d\n", p[i]);      // Notasi array dengan p

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(a + i));  // Notasi pointer dengan a

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(p + i));  // Notasi pointer dengan p

    for (int i = 0; i < 5; i++)
        printf("%d\n", *(p++));    // Menggeser pointer p
        //printf("%d\n", *(a++));    // Menggeser variabel array a--ERROR!
}
```

Jadi kamu bisa lihat bahwa pada umumnya, kalau kamu punya variabel array,
kamu bisa memakai notasi pointer maupun notasi array untuk mengakses
elemennya. Begitu juga dengan variabel pointer.

Satu perbedaan besarnya: sebuah pointer bisa _diubah_ agar menunjuk ke
alamat lain, tapi variabel array tidak bisa. <!--
6.3.2.1p2 --> Dengan kata lain, kamu sama sekali tidak bisa menugaskan
nilai ke variabel array---hanya ke elemen-elemen individual array itu.

Kalau kamu benar-benar ingin menyalin satu array ke array lain, kamu harus
memakai fungsi seperti `memcpy()` (atau sebuah loop).

### Kesetaraan Array/Pointer pada Pemanggilan Fungsi

Di sinilah konsep ini paling sering kamu temui, pasti.

Kalau ada fungsi yang menerima argumen pointer, misalnya:

``` {.c}
int my_strlen(char *s)
```

artinya kamu bisa mengoper array maupun pointer ke fungsi ini, dan
keduanya berfungsi!

``` {.c}
char s[] = "Antelopes";
char *t = "Wombats";

printf("%d\n", my_strlen(s));  // Berfungsi!
printf("%d\n", my_strlen(t));  // Berfungsi juga!
```

Dan itu juga alasan kenapa dua signature fungsi ini setara:

``` {.c}
int my_strlen(char *s)    // Berfungsi!
int my_strlen(char s[])   // Berfungsi juga!
```
\index{Pointers!array equivalence|)}

## Pointer `void`

\index{void* void pointer@\texttt{void*} void pointer|(}

Kamu sudah melihat kata kunci `void` dipakai pada fungsi untuk menandakan
tidak ada parameter atau tidak ada nilai kembalian, tapi ini makhluk yang
sama sekali berbeda dan tidak berhubungan.

`void*` jelas adalah pointer ke sebuah *benda* yang ada. Tapi bagian
`void`-nya menandakan bahwa kita tidak tahu _tipe_ benda itu. Dan kadang,
percaya atau tidak, itu sangat berguna. Dengannya kita bisa menulis kode
yang sedikit lebih agnostik terhadap tipe, fleksibilitas yang menyenangkan
di bahasa bertipe seperti C.

Pada dasarnya ada dua kasus pemakaian---mari kita lihat dan coba
mengurangi misterinya.

\index{memcpy() function@\texttt{memcpy()} function|(}

1. Sebuah fungsi akan bekerja pada sesuatu byte demi byte. Misalnya,
   `memcpy()` menyalin byte memori dari satu pointer ke pointer lain,
   tapi pointer-pointer itu bisa menunjuk ke tipe apa pun. `memcpy()`
   memanfaatkan kenyataan bahwa kalau kita menelusuri `char*`, kita
   menelusuri byte-byte sebuah object apa pun tipenya. Pembahasan lebih
   lanjut ada di subbagian [Nilai Multibyte](#multibyte-values).

2. Fungsi lain memanggil fungsi yang kamu oper kepadanya (sebuah
   callback), dan ia mengoper data kepadamu. Kamu tahu tipe datanya,
   tapi fungsi yang memanggilmu tidak. Jadi ia mengoper `void*`---karena
   tidak tahu tipenya---dan kamu mengonversinya ke tipe yang kamu
   butuhkan. Fungsi bawaan
   [`qsort()`](https://beej.us/guide/bgclr/html/split/stdlib.html#man-qsort)^[https://beej.us/guide/bgclr/html/split/stdlib.html#man-qsort]
   dan
   [`bsearch()`](https://beej.us/guide/bgclr/html/split/stdlib.html#man-bsearch)^[https://beej.us/guide/bgclr/html/split/stdlib.html#man-bsearch]
   memakai teknik ini.

Mari kita lihat contohnya, fungsi bawaan `memcpy()`:

``` {.c}
void *memcpy(void *s1, void *s2, size_t n);
```

Fungsi ini menyalin `n` byte memori mulai dari alamat `s2` ke memori yang
dimulai di alamat `s1`.

Tapi lihat! `s1` dan `s2` adalah `void*`! Kenapa? Apa artinya? Mari kita
coba contoh-contoh lain.

Misalnya, kita bisa menyalin string dengan `memcpy()` (walaupun `strcpy()`
lebih pantas untuk string):

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[] = "Goats!";
    char t[100];

    memcpy(t, s, 7);  // Salin 7 byte--termasuk terminator NUL!

    printf("%s\n", t);  // "Goats!"
}
```

Atau kita bisa menyalin beberapa `int`:

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

int main(void)
{
    int a[] = {11, 22, 33};
    int b[3];

    memcpy(b, a, 3 * sizeof(int));  // Salin data 3 int

    printf("%d\n", b[1]);  // 22
}
```

Yang ini agak liar---kamu lihat apa yang kita lakukan dengan `memcpy()`
di situ? Kita menyalin data dari `a` ke `b`, tapi kita harus menentukan
berapa _byte_ yang disalin, dan sebuah `int` lebih dari satu byte.

Oke, kalau begitu---berapa byte yang dipakai sebuah `int`? Jawabannya:
tergantung sistem. Tapi kita bisa mengetahui berapa byte yang dipakai
tipe apa pun dengan operator `sizeof`.

Jadi inilah jawabannya: sebuah `int` memakai `sizeof(int)` byte memori
untuk menyimpannya.

Dan kalau ada 3 `int` di array kita, seperti pada contoh tadi, seluruh
ruang yang dipakai 3 `int` itu pasti `3 * sizeof(int)`.

(Pada contoh string tadi, yang lebih akurat secara teknis adalah menyalin
`7 * sizeof(char)` byte. Tapi `char` selalu berukuran satu byte menurut
definisi, jadi itu hanya menyusut menjadi `7 * 1`.)

Kita bahkan bisa menyalin sebuah `float` atau `struct` dengan `memcpy()`!
(Walaupun ini keterlaluan---untuk itu sebaiknya pakai `=` saja):

``` {.c}
struct antelope my_antelope;
struct antelope my_clone_antelope;

// ...

memcpy(&my_clone_antelope, &my_antelope, sizeof my_antelope);
```

Lihat betapa serbaguna `memcpy()`! Kalau kamu punya pointer ke sumber dan
pointer ke tujuan, serta jumlah byte yang ingin disalin, kamu bisa
menyalin _data bertipe apa pun_.

Bayangkan kalau tidak ada `void*`. Kita harus menulis fungsi `memcpy()`
khusus untuk setiap tipe:

\index{memcpy() function@\texttt{memcpy()} function|)}

``` {.c}
memcpy_int(int *a, int *b, int count);
memcpy_float(float *a, float *b, int count);
memcpy_double(double *a, double *b, int count);
memcpy_char(char *a, char *b, int count);
memcpy_unsigned_char(unsigned char *a, unsigned char *b, int count);

// dan seterusnya... iyuh!
```

Jauh lebih baik memakai `void*` dan punya satu fungsi yang bisa
mengerjakan semuanya.

Itulah kekuatan `void*`. Kamu bisa menulis fungsi yang tidak peduli tipe
variabelnya, tapi tetap bisa melakukan sesuatu dengannya.

Tapi dengan kekuatan besar datang tanggung jawab besar. Mungkin tidak
sebesar _itu_ dalam kasus ini, tapi memang ada beberapa batasan.

\index{void* void pointer@\texttt{void*} void pointer!caveats|(}

1. Kamu tidak bisa melakukan aritmetika pointer pada `void*`.
2. Kamu tidak bisa men-dereference `void*`.
3. Kamu tidak bisa memakai arrow operator pada `void*`, karena itu juga
   dereference.
4. Kamu tidak bisa memakai notasi array pada `void*`, karena itu juga
   dereference^[Karena ingat, notasi array hanyalah dereference ditambah
   sedikit aritmetika pointer, dan `void*` tidak bisa di-dereference!].

Dan kalau dipikir-pikir, aturan-aturan ini masuk akal. Semua operasi itu
bergantung pada pengetahuan tentang `sizeof` tipe data yang ditunjuk, dan
pada `void*`, kita tidak tahu ukuran data yang ditunjuk---bisa apa saja!

\index{void* void pointer@\texttt{void*} void pointer!caveats|)}

Tapi tunggu---kalau `void*` tidak bisa di-dereference, apa gunanya?

Seperti pada `memcpy()`, ia membantu menulis fungsi generik yang bisa
menangani berbagai tipe data. Tapi rahasianya: di dalamnya, _kamu
mengonversi `void*` ke tipe lain sebelum memakainya_!

Dan konversinya mudah: cukup tugaskan ke variabel bertipe yang
diinginkan^[Kamu juga bisa melakukan _cast_ pada `void*` ke tipe lain,
tapi kita belum sampai ke cast.].

``` {.c}
char a = 'X';  // Satu char

void *p = &a;  // p menunjuk ke 'X'
char *q = p;   // q juga menunjuk ke 'X'

printf("%c\n", *p);  // ERROR--tidak bisa men-dereference void*!
printf("%c\n", *q);  // Mencetak "X"
```

\index{memcpy() function@\texttt{memcpy()} function|(}
Mari kita tulis `memcpy()` sendiri untuk mencobanya. Kita bisa menyalin
byte (`char`), dan kita tahu jumlah byte-nya karena dioper sebagai
argumen.

``` {.c}
void *my_memcpy(void *dest, void *src, int byte_count)
{
    // Ubah void* menjadi char*
    char *s = src, *d = dest;

    // Setelah punya char*, kita bisa men-dereference dan menyalinnya
    while (byte_count--) {
        *d++ = *s++;
    }

    // Kebanyakan fungsi seperti ini mengembalikan tujuan, siapa tahu
    // berguna bagi caller.
    return dest;
}
```

Tepat di awal, kita menyalin `void*` ke `char*` supaya bisa dipakai sebagai
`char*`. Semudah itu.

Lalu ada keseruan di loop while, tempat kita men-decrement `byte_count`
sampai menjadi false (`0`). Ingat bahwa pada post-decrement, nilai
ekspresi dihitung lebih dulu (untuk dipakai `while`) dan _baru kemudian_
variabelnya di-decrement.

Dan ada keseruan di penyalinan, tempat kita menugaskan `*d = *s` untuk
menyalin byte, tapi memakai post-increment sehingga `d` dan `s`
sama-sama bergeser ke byte berikutnya setelah penugasan dilakukan.

Terakhir, kebanyakan fungsi memori dan string mengembalikan salinan
pointer ke tujuan, siapa tahu caller ingin memakainya.

Setelah semua itu, penulis ingin menunjukkan sekilas bahwa kita bisa
memakai teknik ini untuk menelusuri byte-byte dari object _apa pun_ di C,
`float`, `struct`, atau apa saja!
\index{memcpy() function@\texttt{memcpy()} function|)}

\index{qsort() function@\texttt{qsort()} function|(}
[Mari kita]{#qsort-example} coba satu contoh dunia nyata lagi dengan
rutin bawaan `qsort()` yang bisa mengurutkan _apa saja_ berkat keajaiban
`void*`.

(Pada contoh berikut, abaikan saja kata `const`, yang belum kita bahas.)

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

// Tipe struktur yang akan kita urutkan
struct animal {
    char *name;
    int leg_count;
};

// Ini fungsi pembanding yang dipanggil oleh qsort() untuk membantunya
// menentukan apa persisnya yang dijadikan dasar pengurutan. Kita pakai
// untuk mengurutkan array struct animal berdasarkan leg_count.
int compar(const void *elem1, const void *elem2)
{
    // Kita tahu yang diurutkan adalah struct animal, jadi mari kita
    // jadikan kedua argumen pointer ke struct animal
    const struct animal *animal1 = elem1;
    const struct animal *animal2 = elem2;

    // Kembalikan <0, =0, atau >0 tergantung dasar pengurutan yang kita mau.

    // Kita urutkan menaik berdasarkan leg_count, jadi kita kembalikan
    // selisih leg_count
    if (animal1->leg_count > animal2->leg_count)
        return 1;
    
    if (animal1->leg_count < animal2->leg_count)
        return -1;

    return 0;
}

int main(void)
{
    // Kita buat array berisi 4 struct animal dengan karakteristik
    // berbeda. Array ini tidak berurutan menurut leg_count, tapi
    // sebentar lagi akan kita urutkan.
    struct animal a[4] = {
        {.name="Dog", .leg_count=4},
        {.name="Monkey", .leg_count=2},
        {.name="Antelope", .leg_count=4},
        {.name="Snake", .leg_count=0}
    };

    // Panggil qsort() untuk mengurutkan array. qsort() perlu diberi tahu
    // persis berdasarkan apa data ini diurutkan, dan itu kita lakukan
    // di dalam fungsi compar().
    //
    // Pemanggilan ini berarti: qsort array a, yang punya 4 elemen, dan
    // setiap elemen berukuran sizeof(struct animal) byte, dan ini
    // fungsi yang akan membandingkan dua elemen mana pun.
    qsort(a, 4, sizeof(struct animal), compar);

    // Cetak semuanya
    for (int i = 0; i < 4; i++) {
        printf("%d: %s\n", a[i].leg_count, a[i].name);
    }
}
```

Selama kamu memberi `qsort()` fungsi yang bisa membandingkan dua item
dalam array yang akan diurutkan, ia bisa mengurutkan apa saja. Dan itu
dilakukan tanpa perlu menanamkan tipe item di mana pun. `qsort()` hanya
menata ulang blok-blok byte berdasarkan hasil fungsi `compar()` yang kamu
oper.
\index{qsort() function@\texttt{qsort()} function|)}
\index{void* void pointer@\texttt{void*} void pointer|)}
\index{Pointers!arithmetic|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Alokasi Memori Manual

\index{Manual memory management|(}
Inilah salah satu area besar tempat C kemungkinan berbeda dari bahasa yang
sudah kamu kenal: _manajemen memori manual_.

Bahasa lain memakai reference counting, garbage collection, atau cara
lain untuk menentukan kapan harus mengalokasikan memori baru untuk suatu
data---dan kapan harus mendealokasikannya ketika tidak ada lagi variabel
yang merujuknya.

Dan itu menyenangkan. Enak rasanya tidak perlu memikirkannya, cukup
melepas semua referensi ke suatu item dan percaya bahwa pada suatu saat
memori yang terkait dengannya akan dibebaskan.

Tapi C tidak sepenuhnya begitu.

\index{Automatic variables|(}
Tentu saja, di C ada variabel yang dialokasikan dan didealokasikan
otomatis ketika masuk dan keluar scope. Kita menyebutnya variabel
otomatis. Mereka adalah variabel "lokal" biasa dengan scope blok. Tidak
ada masalah.
\index{Automatic variables|)}

Tapi bagaimana kalau kamu ingin sesuatu bertahan lebih lama dari sebuah
blok tertentu? Di sinilah manajemen memori manual berperan.

Kamu bisa menyuruh C secara eksplisit untuk mengalokasikan sejumlah byte
yang bisa kamu pakai sesukamu. Dan byte-byte itu akan tetap teralokasi
sampai kamu membebaskan memori itu secara eksplisit^[Atau sampai program
berakhir, dan saat itu semua memori yang dialokasikannya dibebaskan. Tanda
bintang: beberapa sistem mengizinkan kamu mengalokasikan memori yang
bertahan setelah program berakhir, tapi itu bergantung pada sistem, di
luar cakupan panduan ini, dan kamu pasti tidak akan melakukannya secara
tidak sengaja.].

Penting untuk membebaskan memori yang sudah selesai kamu pakai! Kalau
tidak, itu disebut _memory leak_ dan prosesmu akan terus menahan memori
itu sampai berakhir.

_Kalau kamu mengalokasikannya secara manual, kamu harus membebaskannya
secara manual saat selesai memakainya._

Jadi bagaimana caranya? Kita akan belajar beberapa fungsi baru, dan
memakai operator `sizeof` untuk membantu mengetahui berapa byte yang harus
dialokasikan.

\index{The stack|(}
\index{The heap|(}
Dalam istilah umum di dunia C, para developer bilang variabel lokal
otomatis dialokasikan "di stack", dan memori yang dialokasikan manual ada
"di heap". Spesifikasi tidak membicarakan keduanya, tapi semua developer
C akan paham yang kamu maksud kalau kamu menyebutnya.
\index{The heap|)}
\index{The stack|)}

Semua fungsi yang kita pelajari di bab ini ada di `<stdlib.h>`.

## Mengalokasikan dan Mendealokasikan, `malloc()` dan `free()`

\index{malloc() function@\texttt{malloc()} function|(}
Fungsi `malloc()` menerima jumlah byte yang akan dialokasikan, dan
mengembalikan void pointer ke blok memori yang baru dialokasikan itu.

Karena berupa `void*`, hasilnya bisa ditugaskan ke tipe pointer apa pun
yang kamu mau... biasanya tipe ini berkaitan dengan jumlah byte yang
kamu alokasikan.

\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|(}
Jadi... berapa byte yang harus dialokasikan? `sizeof` bisa membantu.
Kalau kita ingin mengalokasikan ruang untuk satu `int`, kita bisa memakai
`sizeof(int)` dan mengopernya ke `malloc()`.
\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|)}

\index{free() function@\texttt{free()} function|(}
Setelah selesai memakai memori yang dialokasikan, kita bisa memanggil
`free()` untuk menandakan bahwa kita sudah selesai dan memori itu bisa
dipakai untuk hal lain. Sebagai argumen, oper pointer yang sama dengan
yang kamu dapat dari `malloc()` (atau salinannya). Memakai wilayah memori
setelah di-`free()` adalah undefined behavior.

Mari kita coba. Kita alokasikan memori yang cukup untuk satu `int`, lalu
simpan sesuatu di sana, lalu cetak.

``` {.c}
// Alokasikan ruang untuk satu int (sebesar sizeof(int) byte):

int *p = malloc(sizeof(int));

*p = 12;  // Simpan sesuatu di sana

printf("%d\n", *p);  // Cetak: 12

free(p);  // Selesai memakai memori itu

//*p = 3490;  // ERROR: undefined behavior! Dipakai setelah free()!
```
\index{free() function@\texttt{free()} function|)}

Nah, pada contoh yang dibuat-buat itu, sebenarnya tidak ada untungnya.
Kita bisa saja memakai `int` otomatis dan hasilnya sama. Tapi kita akan
lihat bahwa kemampuan mengalokasikan memori seperti ini punya
keunggulan, terutama untuk struktur data yang lebih rumit.

\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|(}
Satu hal lagi yang sering kamu lihat memanfaatkan fakta bahwa `sizeof`
bisa memberi ukuran tipe hasil dari ekspresi konstan apa pun. Jadi kamu
juga bisa menaruh nama variabel di sana dan memakainya. Berikut contohnya,
sama seperti sebelumnya:

``` {.c}
int *p = malloc(sizeof *p);  // *p adalah int, jadi sama dengan sizeof(int)
```
\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|)}
\index{malloc() function@\texttt{malloc()} function|)}

## Pengecekan Error

\index{malloc() function@\texttt{malloc()} function!error checking|(}
Semua fungsi alokasi mengembalikan pointer ke bentangan memori yang baru
dialokasikan, atau `NULL` kalau memori tidak bisa dialokasikan karena
suatu alasan.

Beberapa OS seperti Linux bisa dikonfigurasi sedemikian rupa sehingga
`malloc()` tidak pernah mengembalikan `NULL`, bahkan ketika memori habis.
Tapi meskipun begitu, kamu tetap harus menulis kodenya dengan
perlindungan.

``` {.c}
int *x;

x = malloc(sizeof(int) * 10);

if (x == NULL) {
    printf("Error allocating 10 ints\n");
    // lakukan sesuatu di sini untuk menanganinya
}
```

Berikut pola umum yang akan sering kamu lihat, ketika penugasan dan
kondisi dilakukan di baris yang sama:

``` {.c}
int *x;

if ((x = malloc(sizeof(int) * 10)) == NULL) {
    printf("Error allocating 10 ints\n");
    // lakukan sesuatu di sini untuk menanganinya
}
```
\index{malloc() function@\texttt{malloc()} function!error checking|)}

## Mengalokasikan Ruang untuk Array

\index{malloc() function@\texttt{malloc()} function!and arrays|(}
Kita sudah melihat cara mengalokasikan ruang untuk satu benda; bagaimana
kalau untuk banyak benda dalam sebuah array?

Di C, array adalah sekumpulan benda yang sama, berurutan dalam satu
bentangan memori yang kontigu.

Kita bisa mengalokasikan bentangan memori yang kontigu---kita sudah tahu
caranya. Kalau kita ingin 3490 byte memori, tinggal minta saja:

``` {.c}
char *p = malloc(3490);  // Voila
```

Dan---memang!---itu array berisi 3490 `char` (alias string!) karena
setiap `char` berukuran 1 byte. Dengan kata lain, `sizeof(char)` adalah
`1`.

Catatan: tidak ada inisialisasi pada memori yang baru dialokasikan---isinya
penuh sampah. Bersihkan dengan `memset()` kalau perlu, atau lihat
`calloc()` di bawah.

Tapi kita bisa mengalikan ukuran benda yang diinginkan dengan jumlah
elemen yang diinginkan, lalu mengaksesnya dengan notasi pointer maupun
array. Contoh!

\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|(}
``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // Alokasikan ruang untuk 10 int
    int *p = malloc(sizeof(int) * 10);

    // Isi dengan nilai 0-45:
    for (int i = 0; i < 10; i++)
        p[i] = i * 5;

    // Cetak semua nilai 0, 5, 10, 15, ..., 40, 45
    for (int i = 0; i < 10; i++)
        printf("%d\n", p[i]);

    // Bebaskan ruangnya
    free(p);
}
```

Kuncinya ada di baris `malloc()` itu. Kalau kita tahu setiap `int`
membutuhkan `sizeof(int)` byte, dan kita ingin 10 buah, kita tinggal
mengalokasikan persis sebanyak itu byte dengan:

``` {.c}
sizeof(int) * 10
```

Dan trik ini berlaku untuk semua tipe. Cukup oper ke `sizeof` dan kalikan
dengan ukuran array.
\index{sizeof operator@\texttt{sizeof} operator!with malloc()@with \texttt{malloc()}|)}
\index{malloc() function@\texttt{malloc()} function!and arrays|)}

## Alternatif: `calloc()`

\index{calloc() function@\texttt{calloc()} function|(}
Ini fungsi alokasi lain yang bekerja mirip `malloc()`, dengan dua
perbedaan utama:

* Alih-alih satu argumen, kamu mengoper ukuran satu elemen dan jumlah
  elemen yang ingin dialokasikan. Seolah-olah ia dibuat khusus untuk
  mengalokasikan array.
* Ia membersihkan memori menjadi nol.

Kamu tetap memakai `free()` untuk mendealokasikan memori yang didapat
lewat `calloc()`.

Berikut perbandingan `calloc()` dan `malloc()`.

``` {.c}
// Alokasikan ruang untuk 10 int dengan calloc(), diinisialisasi ke 0:
int *p = calloc(10, sizeof(int));

// Alokasikan ruang untuk 10 int dengan malloc(), diinisialisasi ke 0:
int *q = malloc(10 * sizeof(int));
memset(q, 0, 10 * sizeof(int));   // isi dengan 0
```

Sekali lagi, hasilnya sama untuk keduanya, kecuali `malloc()` tidak
mengosongkan memori secara bawaan.
\index{calloc() function@\texttt{calloc()} function|)}

## Mengubah Ukuran Alokasi dengan `realloc()`

\index{realloc() function@\texttt{realloc()} function|(}
Kalau kamu sudah mengalokasikan 10 `int`, tapi kemudian memutuskan butuh
20, apa yang bisa dilakukan?

Satu pilihan adalah mengalokasikan ruang baru, lalu `memcpy()` isi
memorinya... tapi ternyata kadang tidak ada yang perlu dipindahkan. Dan
ada satu fungsi yang cukup pintar untuk melakukan hal yang benar di semua
keadaan yang tepat: `realloc()`.

Ia menerima pointer ke memori yang sebelumnya dialokasikan (oleh
`malloc()` atau `calloc()`) dan ukuran baru untuk wilayah memori itu.

Ia lalu membesarkan atau mengecilkan memori itu, dan mengembalikan pointer
ke sana. Kadang ia mengembalikan pointer yang sama (kalau data tidak perlu
disalin ke tempat lain), atau pointer yang berbeda (kalau data memang
harus disalin).

Pastikan saat memanggil `realloc()`, kamu menentukan jumlah _byte_ yang
dialokasikan, bukan sekadar jumlah elemen array! Yaitu:

``` {.c}
num_floats *= 2;

np = realloc(p, num_floats);  // SALAH: butuh byte, bukan jumlah elemen!

np = realloc(p, num_floats * sizeof(float));  // Lebih baik!
```

Mari kita alokasikan array berisi 20 `float`, lalu berubah pikiran dan
menjadikannya array berisi 40.

Kita akan menugaskan nilai kembalian realloc() ke pointer lain supaya
bisa memeriksa apakah ia NULL. Kalau bukan, baru kita tugaskan ulang ke
pointer asli. (Kalau nilai kembalian langsung ditugaskan ke pointer asli,
kita akan kehilangan pointer itu ketika fungsi mengembalikan `NULL`, dan
tidak ada cara untuk mendapatkannya lagi.)

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // Alokasikan ruang untuk 20 float
    float *p = malloc(sizeof *p * 20);  // sizeof *p sama dengan sizeof(float)

    // Isi dengan nilai pecahan 0.0-1.0:
    for (int i = 0; i < 20; i++)
        p[i] = i / 20.0;

    // Tapi tunggu! Ternyata kita ingin array ini berisi 40 elemen
    float *new_p = realloc(p, sizeof *p * 40);

    // Periksa apakah realokasi berhasil
    if (new_p == NULL) {
        printf("Error reallocing\n");
        return 1;
    }

    // Kalau berhasil, kita tinggal menugaskan ulang p
    p = new_p;

    // Lalu isi elemen-elemen baru dengan nilai dalam rentang 1.0-2.0
    for (int i = 20; i < 40; i++)
        p[i] = 1.0 + (i - 20) / 20.0;

    // Cetak semua nilai 0.0-2.0 dari 40 elemen:
    for (int i = 0; i < 40; i++)
        printf("%f\n", p[i]);

    // Bebaskan ruangnya
    free(p);
}
```

Perhatikan bagaimana kita mengambil nilai kembalian `realloc()` dan
menugaskannya ulang ke variabel pointer `p` yang sama dengan yang kita
oper. Itu hal yang cukup umum dilakukan.

Kalau baris 7 tampak aneh dengan `sizeof *p` di dalamnya, ingat bahwa
`sizeof` bekerja pada ukuran tipe dari ekspresi. Dan tipe `*p` adalah
`float`, jadi baris itu setara dengan `sizeof(float)`.

Terakhir, mungkin terasa janggal bahwa tidak ada `free(new_p)` di mana
pun, padahal itu pointer yang dikembalikan `realloc()`. Alasannya,
`new_p` disalin ke `p` di baris 23, jadi keduanya bernilai sama; artinya
keduanya menunjuk ke gumpalan memori yang sama, dan gumpalannya hanya
satu. Jadi saat `free()`, kita bisa membebaskan yang mana saja dengan
efek yang sama.

\index{realloc() function@\texttt{realloc()} function|)}


### Membaca Baris dengan Panjang Sembarang

Dengan contoh lengkap ini, penulis ingin menunjukkan dua hal.

1. Pemakaian `realloc()` untuk membesarkan buffer saat kita membaca lebih
   banyak data.
2. Pemakaian `realloc()` untuk mengecilkan buffer ke ukuran yang pas
   setelah pembacaan selesai.

Di sini ada loop yang memanggil `fgetc()` berulang kali untuk menambah
isi buffer, sampai karakter terakhirnya newline.

Begitu newline ditemukan, buffer dikecilkan ke ukuran yang tepat lalu
dikembalikan.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

// Baca satu baris berukuran sembarang dari file
//
// Mengembalikan pointer ke baris tersebut.
// Mengembalikan NULL saat EOF atau error.
//
// Caller bertanggung jawab men-free() pointer ini setelah selesai.
//
// Perhatikan bahwa newline dibuang dari hasilnya. Kalau newline
// diperlukan, sebaiknya ubah ini menjadi do-while.

char *readline(FILE *fp)
{
    int offset = 0;   // Indeks tempat karakter berikutnya masuk ke buffer
    int bufsize = 4;  // Sebaiknya ukuran awal berupa pangkat 2
    char *buf;        // Buffer-nya
    int c;            // Karakter yang baru dibaca

    buf = malloc(bufsize);  // Alokasikan buffer awal

    if (buf == NULL)   // Cek error
        return NULL;

    // Loop utama--baca sampai newline atau EOF
    while (c = fgetc(fp), c != '\n' && c != EOF) {

        // Periksa apakah ruang di buffer habis, dengan memperhitungkan
        // satu byte ekstra untuk terminator NUL
        if (offset == bufsize - 1) {  // -1 untuk terminator NUL
            bufsize *= 2;  // Gandakan ruangnya

            char *new_buf = realloc(buf, bufsize);

            if (new_buf == NULL) {
                free(buf);   // Saat error, free lalu keluar
                return NULL;
            }

            buf = new_buf;  // Realokasi berhasil
        }

        buf[offset++] = c;  // Tambahkan byte ke buffer
    }

    // Kita menemui newline atau EOF...

    // Kalau di EOF dan tidak ada byte yang terbaca, free buffer dan
    // kembalikan NULL untuk menandakan kita di EOF:
    if (c == EOF && offset == 0) {
        free(buf);
        return NULL;
    }

    // Kecilkan sesuai ukuran
    if (offset < bufsize - 1) {  // Kalau belum sampai ujung
        char *new_buf = realloc(buf, offset + 1); // +1 untuk terminator NUL

        // Kalau berhasil, arahkan buf ke new_buf;
        // kalau tidak, biarkan buf di tempatnya
        if (new_buf != NULL)
            buf = new_buf;
    }

    // Tambahkan terminator NUL
    buf[offset] = '\0';

    return buf;
}

int main(void)
{
    FILE *fp = fopen("foo.txt", "r");

    char *line;

    while ((line = readline(fp)) != NULL) {
        printf("%s\n", line);
        free(line);
    }

    fclose(fp);
}
```

Saat membesarkan memori seperti ini, lazimnya (meski bukan aturan baku)
ruang yang dibutuhkan digandakan di setiap langkah, supaya jumlah
`realloc()` yang terjadi sesedikit mungkin.

Terakhir, kamu mungkin memperhatikan bahwa `readline()` mengembalikan
pointer ke buffer hasil `malloc()`. Karena itu, caller bertanggung jawab
untuk men-`free()` memori itu secara eksplisit saat selesai memakainya.

### `realloc()` dengan `NULL`

\index{realloc() function@\texttt{realloc()} function!with NULL argument@with \texttt{NULL} argument|(}
Waktunya trivia! Dua baris ini setara:

``` {.c}
char *p = malloc(3490);
char *p = realloc(NULL, 3490);
```

Ini bisa praktis kalau kamu punya semacam loop alokasi dan tidak ingin
memperlakukan `malloc()` pertama secara khusus.

``` {.c}
int *p = NULL;
int length = 0;

while (!done) {
    // Alokasikan 10 int lagi:
    length += 10;
    p = realloc(p, sizeof *p * length);

    // Lakukan hal-hal luar biasa
    // ...
}
```

Pada contoh itu, kita tidak butuh `malloc()` awal karena `p` memang
`NULL` sejak awal.
\index{realloc() function@\texttt{realloc()} function!with NULL argument@with \texttt{NULL} argument|)}

## Alokasi dengan Alignment

\index{Memory alignment|(}
Kemungkinan besar kamu tidak akan memerlukan ini.

Dan penulis tidak ingin terlalu melenceng membahasnya sekarang, tapi ada
yang disebut _memory alignment_, yang berkaitan dengan alamat memori
(nilai pointer) yang harus merupakan kelipatan dari angka tertentu.

Misalnya, sebuah sistem mungkin mewajibkan nilai 16-bit dimulai di alamat
memori kelipatan 2. Atau nilai 64-bit dimulai di alamat memori kelipatan
2, 4, atau 8, misalnya. Itu tergantung CPU.

Sebagian sistem membutuhkan alignment semacam ini agar akses memori cepat,
bahkan ada yang membutuhkannya agar akses memori bisa dilakukan sama
sekali.

Nah, kalau kamu memakai `malloc()`, `calloc()`, atau `realloc()`, C akan
memberimu gumpalan memori yang ter-align dengan baik untuk nilai apa pun,
bahkan `struct`. Berlaku di semua kasus.

Tapi mungkin ada saatnya kamu tahu bahwa data tertentu bisa di-align pada
batas yang lebih kecil, atau harus di-align pada batas yang lebih besar
karena suatu alasan. Penulis membayangkan ini lebih umum pada pemrograman
embedded system.

\index{aligned\_alloc() function@\texttt{aligned\_alloc()} function|(}
Dalam kasus itu, kamu bisa menentukan alignment dengan `aligned_alloc()`.

Alignment adalah integer pangkat dua yang lebih besar dari nol, jadi `2`,
`4`, `8`, `16`, dan seterusnya, dan itu diberikan ke `aligned_alloc()`
sebelum jumlah byte yang kamu inginkan.

Pembatas lainnya: jumlah byte yang dialokasikan harus merupakan kelipatan
alignment. Tapi ini mungkin akan berubah. Lihat [C Defect Report
460](http://www.open-std.org/jtc1/sc22/wg14/www/docs/summary.htm#dr_460)^[http://www.open-std.org/jtc1/sc22/wg14/www/docs/summary.htm#dr_460]

Mari kita coba contoh, mengalokasikan pada batas 64 byte:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    // Alokasikan 256 byte yang ter-align pada batas 64 byte
    char *p = aligned_alloc(64, 256);  // 256 == 64 * 4

    // Salin sebuah string ke sana dan cetak
    strcpy(p, "Hello, world!");
    printf("%s\n", p);

    // Bebaskan ruangnya
    free(p);
}
```

Ada catatan soal `realloc()` dan `aligned_alloc()`. `realloc()` tidak
punya jaminan alignment apa pun, jadi kalau kamu butuh ruang hasil
realokasi yang ter-align, kamu harus melakukannya dengan cara susah
memakai `memcpy()`.
\index{aligned\_alloc() function@\texttt{aligned\_alloc()} function|)}

Berikut fungsi `aligned_realloc()` non-standar, kalau kamu membutuhkannya:

``` {.c}
void *aligned_realloc(void *ptr, size_t old_size, size_t alignment, size_t size)
{
    char *new_ptr = aligned_alloc(alignment, size);

    if (new_ptr == NULL)
        return NULL;

    size_t copy_size = old_size < size? old_size: size;  // ambil yang terkecil

    if (ptr != NULL)
        memcpy(new_ptr, ptr, copy_size);

    free(ptr);

    return new_ptr;
}
```

Perhatikan bahwa fungsi ini _selalu_ menyalin data, yang memakan waktu,
sedangkan `realloc()` yang asli akan menghindarinya kalau bisa. Jadi ini
jauh dari efisien. Hindari kebutuhan untuk merealokasi data yang
alignment-nya kustom.
\index{Memory alignment|)}
\index{Manual memory management|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Scope {#scope}

\index{Scope|(}
Scope adalah soal variabel apa saja yang terlihat di konteks mana.

## Block Scope

\index{Scope!block|(}
Hampir semua variabel yang didefinisikan developer memakai scope ini,
termasuk yang di bahasa lain mungkin disebut "function scope", yaitu
variabel yang dideklarasikan di dalam fungsi.

Aturan dasarnya: kalau kamu mendeklarasikan variabel di dalam blok yang
dibatasi kurung kurawal, scope variabel itu adalah blok tersebut.

Kalau ada blok di dalam blok, variabel yang dideklarasikan di blok
_dalam_ bersifat lokal untuk blok itu, dan tidak terlihat di scope luar.

Begitu scope sebuah variabel berakhir, variabel itu tidak bisa lagi
dirujuk, dan nilainya boleh dianggap sudah lenyap ke [bit
bucket](https://en.wikipedia.org/wiki/Bit_bucket)^[https://en.wikipedia.org/wiki/Bit_bucket] raksasa di langit.

Contoh dengan scope bersarang:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int a = 12;         // Lokal untuk blok luar, tapi terlihat di blok dalam

    if  (a == 12) {
        int b = 99;     // Lokal untuk blok dalam, tidak terlihat di blok luar

        printf("%d %d\n", a, b);  // OK: "12 99"
    }

    printf("%d\n", a);  // OK, kita masih di scope a

    printf("%d\n", b);  // ILEGAL, di luar scope b
}
```

### Di Mana Variabel Didefinisikan

Fakta menarik lainnya: variabel bisa didefinisikan di mana saja dalam
blok, dalam batas kewajaran---scope-nya adalah blok itu, tapi tidak bisa
dipakai sebelum didefinisikan.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 0;

    printf("%d\n", i);     // OK: "0"

    //printf("%d\n", j);   // ILEGAL--j tidak bisa dipakai sebelum didefinisikan

    int j = 5;

    printf("%d %d\n", i, j);   // OK: "0 5"
}
```

Secara historis, C mewajibkan semua variabel didefinisikan sebelum kode
apa pun di dalam blok, tapi sejak standar C99 hal itu tidak berlaku lagi.

### Variable Hiding

\index{Variable hiding|(}
Kalau ada variabel bernama sama di scope dalam dan di scope luar, yang di
scope dalam diutamakan selama kode berjalan di scope dalam. Artinya, ia
_menyembunyikan_ variabel di scope luar selama masa hidupnya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i = 10;

    {
        int i = 20;

        printf("%d\n", i);  // i di scope dalam, 20 (i luar tersembunyi)
    }

    printf("%d\n", i);  // i di scope luar, 10
}
```

Mungkin kamu memperhatikan bahwa pada contoh itu, penulis cuma melempar
sebuah blok begitu saja di baris 7, tanpa `for` atau `if` yang
mengawalinya! Ini sepenuhnya legal. Kadang developer ingin mengelompokkan
beberapa variabel lokal untuk perhitungan singkat dan melakukan ini,
tapi jarang terlihat.
\index{Variable hiding|)}
\index{Scope!block|)}

## File Scope

\index{Scope!file|(}
Kalau kamu mendefinisikan variabel di luar blok, variabel itu punya _file
scope_. Ia terlihat di semua fungsi dalam file yang muncul setelahnya, dan
dipakai bersama oleh fungsi-fungsi itu. (Pengecualiannya: kalau sebuah
blok mendefinisikan variabel bernama sama, variabel itu akan menyembunyikan
yang ada di file scope.)

Ini yang paling mendekati apa yang kamu anggap sebagai scope "global" di
bahasa lain.

Contohnya:

``` {.c .numberLines}
#include <stdio.h>

int shared = 10;    // File scope! Terlihat di seluruh file setelah ini!

void func1(void)
{
    shared += 100;  // Sekarang shared bernilai 110
}

void func2(void)
{
    printf("%d\n", shared);  // Mencetak "110"
}

int main(void)
{
    func1();
    func2();
}
```

Perhatikan bahwa kalau `shared` dideklarasikan di bagian bawah file, kode
itu tidak akan dikompilasi. Variabel harus dideklarasikan _sebelum_
dipakai oleh fungsi mana pun.

Ada cara untuk memodifikasi item di file scope lebih jauh, yaitu dengan
[static](#static) dan [extern](#extern), tapi itu kita bahas nanti.
\index{Scope!file|)}

## Scope pada Loop `for`

\index{Scope!for loop@\texttt{for} loop|(}
Penulis sungguh tidak tahu harus menyebut ini apa, karena C11
§6.8.5.3¶1 tidak memberinya nama yang jelas. Kita juga sudah beberapa kali
melakukannya di panduan ini. Yaitu ketika kamu mendeklarasikan variabel di
klausa pertama sebuah loop `for`:

``` {.c}
for (int i = 0; i < 10; i++)
    printf("%d\n", i);

printf("%d\n", i);  // ILEGAL--i hanya ada di scope selama for-loop
```

Pada contoh itu, masa hidup `i` dimulai saat ia didefinisikan, dan
berlanjut selama loop berjalan.

Kalau badan loop dibungkus dalam sebuah blok, variabel yang didefinisikan
di `for`-loop terlihat dari scope dalam itu.

Kecuali, tentu saja, kalau scope dalam itu menyembunyikannya. Contoh gila
ini mencetak `999` sebanyak lima kali:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    for (int i = 0; i < 5; i++) {
        int i = 999;  // Menyembunyikan i di scope for-loop
        printf("%d\n", i);
    }
}
```
\index{Scope!for loop@\texttt{for} loop|)}

## Catatan tentang Function Scope

\index{Scope!function|(}
Spesifikasi C memang menyebut _function scope_, tapi istilah itu dipakai
khusus untuk _label_, yang belum kita bahas. Nanti di lain hari.
\index{Scope!function|)}
\index{Scope|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe II: Jauh Lebih Banyak Tipe!

Kita sudah terbiasa dengan tipe `char`, `int`, dan `float`, tapi sekarang
saatnya membawanya ke level berikutnya dan melihat apa lagi yang tersedia
di departemen tipe!

## Integer Signed dan Unsigned

\index{Types!signed and unsigned|(}
Sejauh ini kita memakai `int` sebagai tipe _signed_, yaitu nilai yang bisa
negatif maupun positif. Tapi C juga punya tipe integer _unsigned_ khusus
yang hanya bisa menyimpan angka positif.

Tipe-tipe ini diawali dengan kata kunci \index{unsigned type@\texttt{unsigned} type|(}`unsigned`.

``` {.c}
int a;           // signed
signed int a;    // signed
signed a;        // signed, "singkatan" dari "int" atau "signed int", jarang dipakai
unsigned int b;  // unsigned
unsigned c;      // unsigned, singkatan dari "unsigned int"
```

Kenapa? Kenapa kamu memutuskan hanya ingin menyimpan angka positif?

Jawabannya: variabel unsigned bisa menampung angka yang lebih besar
dibanding variabel signed.

Tapi kenapa begitu?

Bayangkan integer direpresentasikan oleh sejumlah _bit_^["Bit" kependekan
dari _binary digit_. Biner hanyalah cara lain merepresentasikan angka.
Alih-alih digit 0-9 yang biasa kita pakai, digitnya hanya 0-1.]. Di
komputerku, sebuah `int` direpresentasikan oleh 64 bit.

Dan setiap permutasi bit yang bernilai `1` atau `0` mewakili sebuah angka.
Kita yang memutuskan bagaimana membagi angka-angka ini.

Pada angka signed, kita memakai (kurang lebih) separuh permutasi untuk
angka negatif, dan separuh lainnya untuk angka positif.

Pada unsigned, kita memakai _semua_ permutasi untuk angka positif.

Di komputerku, dengan `int` 64-bit yang memakai [two's
complement](https://en.wikipedia.org/wiki/Two%27s_complement)^[https://en.wikipedia.org/wiki/Two%27s_complement] untuk merepresentasikan angka unsigned,
rentang integernya adalah sebagai berikut:

|Tipe|Minimum|Maksimum|
|:-|-:|-:|
|`int`|`-9,223,372,036,854,775,808`|`9,223,372,036,854,775,807`|
|`unsigned int`|`0`|`18,446,744,073,709,551,615`|

Perhatikan bahwa `unsigned int` positif terbesar kira-kira dua kali lebih
besar dari `int` positif terbesar. Jadi ada sedikit keleluasaan di sana.
\index{unsigned type@\texttt{unsigned} type|)}
\index{Types!signed and unsigned|)}

## Tipe Karakter

\index{Types!character|(}
\index{char type@\texttt{char} type|(}
Ingat `char`? Tipe yang bisa kita pakai untuk menyimpan satu karakter?

``` {.c}
char c = 'B';

printf("%c\n", c);  // "B"
```

Ada kejutan untukmu: sebenarnya ia adalah integer.

``` {.c}
char c = 'B';

// Ubah ini dari %c menjadi %d:
printf("%d\n", c);  // 66 (!!)
```

Jauh di dalamnya, `char` hanyalah `int` kecil, yaitu integer yang hanya
memakai satu byte ruang, jadi rentangnya terbatas pada...

Di sini spesifikasi C jadi sedikit aneh. Ia menjamin bahwa `char` adalah
satu byte, yaitu `sizeof(char) == 1`. Tapi kemudian di C11 §3.6¶3 ia
berusaha keras menyatakan:

> A byte is composed of a contiguous sequence of bits, _the number of
> which is implementation-defined._

Tunggu---apa? Mungkin sebagian dari kamu terbiasa dengan anggapan bahwa
byte itu 8 bit, kan? Maksudnya, memang begitu, kan? Dan jawabannya:
"Hampir pasti."^[Istilah industri untuk deretan yang tepat dan pasti 8 bit
adalah _oktet_.] Tapi C bahasa yang tua, dan mesin-mesin zaman dulu punya
pendapat yang, katakanlah, lebih _santai_ soal berapa bit dalam satu byte.
Dan selama bertahun-tahun, C mempertahankan fleksibilitas ini.

Tapi dengan asumsi byte di C-mu berukuran 8 bit, seperti hampir semua
mesin di dunia yang akan pernah kamu temui, rentang `char` adalah...

\index{unsigned char type@\texttt{unsigned char} type|(}
\index{signed char type@\texttt{signed char} type|(}
---Nah, sebelum aku bisa menyebutkannya, ternyata `char` bisa signed atau
unsigned tergantung compiler-mu. Kecuali kalau kamu menyebutkannya secara
eksplisit.

Dalam banyak kasus, cukup memakai `char` karena kamu tidak peduli soal
tanda data. Tapi kalau kamu butuh `char` signed atau unsigned, kamu
_harus_ eksplisit:

``` {.c}
char a;           // Bisa signed atau unsigned
signed char b;    // Pasti signed
unsigned char c;  // Pasti unsigned
```

Oke, sekarang akhirnya kita bisa menentukan rentang angkanya, dengan
asumsi `char` berukuran 8 bit dan sistemmu memakai representasi two's
complement yang hampir universal untuk signed dan unsigned^[Secara umum,
kalau ada bilangan two's complement $n$ bit, rentang signed-nya adalah
$-2^{n-1}$ sampai $2^{n-1}-1$. Dan rentang unsigned-nya adalah $0$ sampai
$2^n-1$.].

Jadi, dengan asumsi-asumsi itu, akhirnya kita bisa menghitung rentangnya:

|Tipe `char`|Minimum|Maksimum|
|:-|-:|-:|
|`signed char`|`-128`|`127`|
|`unsigned char`|`0`|`255`|

Dan rentang untuk `char` ditentukan oleh implementasi.
\index{unsigned char type@\texttt{unsigned char} type|)}
\index{signed char type@\texttt{signed char} type|)}

Biar kuluruskan dulu. `char` sebenarnya angka, berarti bisa dihitung secara
matematis?

Bisa! Cuma ingat untuk menjaga nilainya dalam rentang `char`!

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char a = 10, b = 20;

    printf("%d\n", a + b);  // 30!
}
```

\index{' single quote@\texttt{'} single quote|(}
Bagaimana dengan konstanta karakter dalam tanda kutip tunggal, seperti
`'B'`? Bagaimana ia punya nilai numerik?

Spesifikasi juga bersikap mengambang di sini, karena C tidak dirancang
untuk berjalan di satu jenis sistem dasar saja.

Tapi anggap saja untuk sementara character set-mu berbasis
[ASCII](https://en.wikipedia.org/wiki/ASCII)^[https://en.wikipedia.org/wiki/ASCII] setidaknya untuk 128 karakter pertama. Kalau begitu,
konstanta karakter akan dikonversi menjadi `char` yang nilainya sama
dengan nilai ASCII karakter tersebut.

Ribet juga, ya. Langsung saja contoh:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char a = 10;
    char b = 'B';  // Nilai ASCII 66

    printf("%d\n", a + b);  // 76!
}
```

Ini bergantung pada lingkungan eksekusimu dan [character set yang
dipakai](https://en.wikipedia.org/wiki/List_of_information_system_character_sets)^[https://en.wikipedia.org/wiki/List_of_information_system_character_sets]. Salah satu character
set paling populer saat ini adalah [Unicode](https://en.wikipedia.org/wiki/Unicode)^[https://en.wikipedia.org/wiki/Unicode] (yang merupakan
superset ASCII), jadi untuk 0-9, A-Z, a-z dan tanda baca dasar, hampir
pasti kamu mendapat nilai ASCII-nya.
\index{' single quote@\texttt{'} single quote|)}
\index{char type@\texttt{char} type|)}
\index{Types!character|)}

## Tipe Integer Lainnya: `short`, `long`, `long long`

Sejauh ini kita hanya memakai dua tipe integer secara umum:

* `char`
* `int`

dan baru-baru ini kita belajar varian unsigned dari tipe integer. Kita
juga tahu bahwa `char` diam-diam adalah `int` kecil yang menyamar. Jadi
kita tahu `int` bisa punya berbagai ukuran bit.

Tapi ada beberapa tipe integer lagi yang perlu kita lihat, beserta
rentang minimum yang bisa mereka tampung. (Implementasimu kemungkinan
punya rentang lebih lebar daripada yang disyaratkan spesifikasi, tapi
rentang di sini adalah yang **pasti** tersedia secara portabel.)

Header file `<limits.h>` mendefinisikan makro yang menyimpan rentang
berbagai tipe; andalkan itu untuk kepastian, dan _jangan pernah
meng-hardcode atau mengasumsikan nilai-nilai ini_.

\index{short type@\texttt{short} type|(}
\index{long type@\texttt{long} type|(}
\index{long long type@\texttt{long long} type|(}
Tipe tambahan ini adalah `short int`, `long int`, dan `long long int`.
Biasanya, saat memakai tipe-tipe ini, developer C menghilangkan bagian
`int` (misalnya `long long`), dan compiler tetap senang.

``` {.c}
// Dua baris ini setara:
long long int x;
long long x;

// Begitu juga dua baris ini:
short int x;
short x;
```

Mari kita lihat tipe data integer dan ukurannya secara menaik, dikelompokkan
menurut tanda. Sekali lagi, batas minimum dan maksimum ini adalah yang
dijamin secara portabel oleh spesifikasi; sistemmu kemungkinan punya
rentang lebih lebar.

|Tipe|Byte Minimum|Nilai Minimum|Nilai Maksimum|
|:-|-:|-:|-:|
|`char`|1|-128 atau 0|127 atau 255^[Tergantung apakah `char` secara bawaan berupa `signed char` atau `unsigned char`]|
|`signed char`|1|-128|127|
|`short`|2|-32768|32767|
|`int`|2|-32768|32767|
|`long`|4|-2147483648|2147483647|
|`long long`|8|-9223372036854775808|9223372036854775807|
|`unsigned char`|1|0|255|
|`unsigned short`|2|0|65535|
|`unsigned int`|2|0|65535|
|`unsigned long`|4|0|4294967295|
|`unsigned long long`|8|0|18446744073709551615|

Tidak ada tipe `long long long`. Kamu tidak bisa terus menambah `long`
seperti itu. Jangan konyol.

> Penggemar two's complement mungkin menyadari ada yang janggal pada
> angka-angka itu. Kenapa, misalnya, `signed char` berhenti di -127 dan
> bukan -128? Ingat: itu hanya minimum yang disyaratkan spesifikasi.
> Beberapa representasi angka (seperti [sign and
> magnitude](https://en.wikipedia.org/wiki/Signed_number_representations#Signed_magnitude_representation)^[https://en.wikipedia.org/wiki/Signed_number_representations#Signed_magnitude_representation])
> mentok di ±127.

Mari kita jalankan tabel yang sama pada sistem 64-bit two's complement
milikku dan lihat hasilnya:

|Tipe|Byte di Sistemku|Nilai Minimum|Nilai Maksimum|
|:-|-:|-:|-:|
|`char`|1|-128|127^[`char` di sistemku adalah signed.]|
|`signed char`|1|-128|127|
|`short`|2|-32768|32767|
|`int`|4|-2147483648|2147483647|
|`long`|8|-9223372036854775808|9223372036854775807|
|`long long`|8|-9223372036854775808|9223372036854775807|
|`unsigned char`|1|0|255|
|`unsigned short`|2|0|65535|
|`unsigned int`|4|0|4294967295|
|`unsigned long`|8|0|18446744073709551615|
|`unsigned long long`|8|0|18446744073709551615|

Ini sedikit lebih masuk akal, tapi kita bisa melihat bahwa sistemku
punya batas yang lebih besar daripada minimum di spesifikasi.

[Lalu apa saja makro di `<limits.h>`?]{#limits-macros}

|Tipe|Makro Min|Makro Maks|
|:-|:-|:-|
|`char`|`CHAR_MIN`|`CHAR_MAX`|
|`signed char`|`SCHAR_MIN`|`SCHAR_MAX`|
|`short`|`SHRT_MIN`|`SHRT_MAX`|
|`int`|`INT_MIN`|`INT_MAX`|
|`long`|`LONG_MIN`|`LONG_MAX`|
|`long long`|`LLONG_MIN`|`LLONG_MAX`|
|`unsigned char`|`0`|`UCHAR_MAX`|
|`unsigned short`|`0`|`USHRT_MAX`|
|`unsigned int`|`0`|`UINT_MAX`|
|`unsigned long`|`0`|`ULONG_MAX`|
|`unsigned long long`|`0`|`ULLONG_MAX`|

Perhatikan ada cara tersembunyi di sana untuk menentukan apakah sebuah
sistem memakai `char` signed atau unsigned. Kalau `CHAR_MAX == UCHAR_MAX`,
berarti ia unsigned.

Perhatikan juga bahwa tidak ada makro minimum untuk varian
`unsigned`---nilainya cuma `0`.
\index{short type@\texttt{short} type|)}
\index{long type@\texttt{long} type|)}
\index{long long type@\texttt{long long} type|)}

## Float Lainnya: `double` dan `long double`

Mari kita lihat apa kata spesifikasi tentang bilangan floating point di
§5.2.4.2.2¶1-2:

>The following parameters are used to define the model for each
>floating-point type:
>
> |Parameter|Definition|
> |:-|:-|
> |$s$|sign ($\pm1$)|
> |$b$|base or radix of exponent representation (an integer $> 1$)|
> |$e$|exponent (an integer between a minimum $e_{min}$ and a maximum $e_{max}$)|
> |$p$|precision (the number of base-$b$ digits in the significand)|
> |$f_k$|nonnegative integers less than $b$ (the significand digits)|
>
> A _floating-point number_ ($x$) is defined by the following model:
>
>> $x=sb^e\sum\limits_{k=1}^p f_kb^{-k},$\ \ \ \ $e_{min}\le e\le e_{max}$

Kuharap itu langsung bikin semuanya jelas.

Baiklah. Mundur selangkah dan lihat apa yang praktis.

Catatan: kita merujuk ke sejumlah makro di bagian ini. Makro-makro itu ada
di header `<float.h>`.

Bilangan floating point di-encode dalam urutan bit tertentu
([format IEEE-754](https://en.wikipedia.org/wiki/IEEE_754)^[https://en.wikipedia.org/wiki/IEEE_754] sangat populer) di dalam byte.

Lebih dalam lagi, bilangan itu pada dasarnya direpresentasikan sebagai
_significand_ (bagian angkanya---digit-digit signifikannya sendiri,
kadang juga disebut _mantissa_) dan _exponent_, yaitu pangkat yang
dipakai untuk menaikkan digit-digit itu. Ingat bahwa eksponen negatif
bisa membuat angka menjadi lebih kecil.

Bayangkan kita memakai $10$ sebagai bilangan yang dipangkatkan. Kita bisa
merepresentasikan angka-angka berikut dengan significand $12345$ dan
eksponen $-3$, $4$, dan $0$ untuk meng-encode nilai floating point
berikut:

$12345\times10^{-3}=12.345$

$12345\times10^4=123450000$

$12345\times10^0=12345$

Untuk semua angka itu, significand-nya tetap sama. Satu-satunya perbedaan
adalah eksponennya.

Di mesinmu, basis untuk eksponen kemungkinan adalah $2$, bukan $10$,
karena komputer suka biner. Kamu bisa mengeceknya dengan mencetak makro
`FLT_RADIX`.

Jadi kita punya sebuah angka yang direpresentasikan oleh sejumlah byte,
yang di-encode dengan cara tertentu. Karena pola bit terbatas, jumlah
bilangan floating point yang bisa direpresentasikan juga terbatas.

Lebih khusus lagi, hanya sejumlah tertentu digit desimal signifikan yang
bisa direpresentasikan secara akurat.

Bagaimana cara mendapat lebih banyak? Pakai tipe data yang lebih besar!

Dan kita punya beberapa. Kita sudah kenal `float`, tapi untuk presisi
lebih besar ada `double`. Dan untuk presisi yang lebih besar lagi, ada
`long double` (tidak berhubungan dengan `long int` kecuali namanya).

Spesifikasi tidak menentukan berapa byte penyimpanan yang dipakai setiap
tipe, tapi di sistemku, kita bisa melihat kenaikan ukuran relatifnya:

\index{double type@\texttt{double} type|(}
\index{long double type@\texttt{long double} type|(}

|Tipe|`sizeof`|
|:-|-:|
|`float`|4|
|`double`|8|
|`long double`|16|

Jadi setiap tipe (di sistemku) memakai bit tambahan itu untuk
presisi yang lebih tinggi.

Tapi _seberapa besar_ presisinya? Berapa banyak angka desimal yang bisa
direpresentasikan oleh nilai-nilai ini?

Nah, C menyediakan sejumlah makro di `<float.h>` untuk membantu kita
mengetahuinya.

Ini jadi agak aneh kalau kamu memakai sistem basis-2 (biner) untuk
menyimpan angka (yang dipakai hampir semua orang di planet ini, mungkin
termasuk kamu), tapi sabar saja sementara kita mengurainya.
\index{double type@\texttt{double} type|)}
\index{long double type@\texttt{long double} type|)}

### Berapa Banyak Digit Desimal?

\index{Significant digits|(}
Pertanyaan sejuta dolarnya adalah, "Berapa banyak digit desimal signifikan
yang bisa disimpan dalam tipe floating point tertentu sehingga ketika
dicetak, kita mendapat angka desimal yang sama?"

Jumlah digit desimal yang bisa disimpan dalam tipe floating point dan
pasti kembali sama saat dicetak diberikan oleh makro-makro ini:

\index{FLT\_DIG macro@\texttt{FLT\_DIG} macro|(}
\index{DBL\_DIG macro@\texttt{DBL\_DIG} macro|(}
\index{LDBL\_DIG macro@\texttt{LDBL\_DIG} macro|(}

|Tipe|Digit Desimal yang Bisa Disimpan|Minimum|Sistemku|
|:-|-:|-:|
|`float`|`FLT_DIG`|6|6|
|`double`|`DBL_DIG`|10|15|
|`long double`|`LDBL_DIG`|10|18|

\index{DBL\_DIG macro@\texttt{DBL\_DIG} macro|)}
\index{LDBL\_DIG macro@\texttt{LDBL\_DIG} macro|)}

Di sistemku, `FLT_DIG` adalah 6, jadi aku bisa yakin kalau sebuah
`float` 6 digit dicetak, hasilnya sama. (Bisa saja lebih dari 6
digit---sebagian angka tetap kembali dengan benar dengan lebih banyak
digit. Tapi 6 pasti kembali.)

Misalnya, kalau kita mencetak `float` dengan pola digit yang bertambah
seperti ini, kelihatannya kita sampai 8 digit sebelum ada yang salah,
tapi setelah itu kembali ke 7 digit yang benar.

``` {.default}
0.12345
0.123456
0.1234567
0.12345678
0.123456791  <-- Things start going wrong
0.1234567910
```

Kita coba demo lain. Pada kode ini ada dua `float` yang masing-masing
menyimpan angka dengan `FLT_DIG` digit desimal signifikan^[Program ini
berjalan sesuai komentarnya pada sistem dengan `FLT_DIG` bernilai `6` yang
memakai bilangan floating point IEEE-754 basis-2. Kalau tidak, hasilnya
mungkin berbeda.]. Lalu keduanya dijumlahkan, yang seharusnya menghasilkan
12 digit desimal signifikan. Tapi itu lebih dari yang bisa disimpan dalam
`float` dan dipulihkan dengan benar sebagai string---jadi ketika dicetak,
terlihat ada yang mulai salah setelah digit signifikan ke-7.


``` {.c .numberLines}
#include <stdio.h>
#include <float.h>

int main(void)
{
    // Kedua angka ini punya 6 digit signifikan, jadi bisa
    // disimpan secara akurat dalam float:

    float f = 3.14159f;
    float g = 0.00000265358f;

    printf("%.5f\n", f);   // 3.14159       -- benar!
    printf("%.11f\n", g);  // 0.00000265358 -- benar!

    // Sekarang jumlahkan
    f += g;                // 3.14159265358 adalah nilai f yang _seharusnya_

    printf("%.11f\n", f);  // 3.14159274101 -- salah!
}
```

(Kode di atas punya `f` setelah konstanta numeriknya---ini menandakan
bahwa konstanta itu bertipe `float`, bukan `double` yang menjadi
bawaannya. Soal ini dibahas nanti.)

Ingat bahwa `FLT_DIG` adalah jumlah digit aman yang bisa disimpan dalam
`float` dan diambil kembali dengan benar.

Kadang kamu bisa mendapat satu atau dua digit lebih. Tapi kadang kamu
hanya mendapat `FLT_DIG` digit. Yang pasti: kalau kamu menyimpan angka
dengan jumlah digit sampai dengan `FLT_DIG` dalam `float`, dijamin
kembali dengan benar.

Jadi begitulah ceritanya. `FLT_DIG`. Tamat.

\index{FLT\_DIG macro@\texttt{FLT\_DIG} macro|)}

...Atau belum?

### Mengonversi ke Desimal dan Kembali

Tapi menyimpan bilangan basis 10 dalam bilangan floating point lalu
mengambilnya kembali baru setengah cerita.

Ternyata bilangan floating point bisa meng-encode angka yang butuh lebih
banyak angka desimal untuk dicetak sepenuhnya. Hanya saja angka desimal
besarmu mungkin tidak cocok dengan salah satu angka itu.

Artinya, kalau kamu melihat bilangan floating point dari satu ke yang
berikutnya, ada celah di antaranya. Kalau kamu mencoba meng-encode angka
desimal di celah itu, yang dipakai adalah bilangan floating point
terdekat. Itulah kenapa kamu hanya bisa meng-encode `FLT_DIG` pada
`float`.

Tapi bagaimana dengan bilangan floating point yang _tidak_ ada di celah?
Berapa tempat desimal yang dibutuhkan untuk mencetaknya secara akurat?

Cara lain menyatakan pertanyaan ini: untuk bilangan floating point
tertentu, berapa digit desimal yang harus dipertahankan kalau kita ingin
mengonversi angka desimal itu kembali menjadi bilangan floating point yang
identik? Artinya, berapa digit yang harus dicetak di basis 10 untuk
memulihkan **semua** digit basis 2 pada angka aslinya?

Kadang hanya perlu beberapa. Tapi supaya yakin, kamu perlu mengonversi ke
desimal dengan jumlah tempat desimal aman tertentu. Angka itu di-encode
dalam makro-makro berikut:

\index{FLT\_DECMIAL\_DIG macro@\texttt{FLT\_DECMIAL\_DIG} macro|(}
\index{DBL\_DECMIAL\_DIG macro@\texttt{DBL\_DECMIAL\_DIG} macro|(}
\index{LDBL\_DECMIAL\_DIG macro@\texttt{LDBL\_DECMIAL\_DIG} macro|(}
\index{DECMIAL\_DIG macro@\texttt{DECMIAL\_DIG} macro|(}

|Makro|Deskripsi|
|-----------|---------------------------------------------------|
|`FLT_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `float`.|
|`DBL_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `double`.|
|`LDBL_DECIMAL_DIG`|Jumlah digit desimal yang di-encode dalam `long double`.|
|`DECIMAL_DIG`|Sama dengan encoding terlebar, `LDBL_DECIMAL_DIG`.|

\index{LDBL\_DECMIAL\_DIG macro@\texttt{LDBL\_DECMIAL\_DIG} macro|)}
\index{DECMIAL\_DIG macro@\texttt{DECMIAL\_DIG} macro|)}

Mari kita lihat contoh ketika `DBL_DIG` adalah 15 (jadi hanya segitu yang
bisa ada di konstanta), tapi `DBL_DECIMAL_DIG` adalah 17 (jadi kita harus
mengonversi ke 17 angka desimal untuk mempertahankan semua bit dari
`double` aslinya).

Kita tugaskan angka 15 digit signifikan `0.123456789012345` ke `x`, dan
angka 1 digit signifikan `0.0000000000000006` ke `y`.

``` {.default}
x is exact: 0.12345678901234500    Printed to 17 decimal places
y is exact: 0.00000000000000060
```

Tapi mari kita jumlahkan. Hasilnya seharusnya `0.1234567890123456`, tapi
itu lebih dari `DBL_DIG`, jadi mungkin terjadi hal-hal aneh... kita lihat:

``` {.default}
x + y not quite right: 0.12345678901234559    Should end in 4560!
```

Itulah yang kita dapat karena mencetak lebih dari `DBL_DIG`, kan? Tapi
lihat ini... angka di atas itu bisa direpresentasikan persis seperti
adanya!

Kalau kita tugaskan `0.12345678901234559` (17 digit) ke `z` dan
mencetaknya, hasilnya:

``` {.default}
z is exact: 0.12345678901234559   17 digits correct! More than DBL_DIG!
```

Kalau `z` dipotong menjadi 15 digit, angkanya tidak akan sama. Itulah
kenapa untuk mempertahankan semua bit sebuah `double`, kita butuh
`DBL_DECIMAL_DIG` dan bukan sekadar `DBL_DIG` yang lebih kecil.

\index{DBL\_DECMIAL\_DIG macro@\texttt{DBL\_DECMIAL\_DIG} macro|)}

Jadi, jelas bahwa saat bermain dengan angka desimal secara
umum, tidak aman mencetak lebih dari \index{FLT\_DIG macro@\texttt{FLT\_DIG} macro}`FLT_DIG`, \index{DBL\_DIG macro@\texttt{DBL\_DIG} macro}`DBL_DIG`, atau \index{LDBL\_DIG macro@\texttt{LDBL\_DIG} macro}`LDBL_DIG` digit agar tetap masuk akal terhadap angka basis 10
aslinya dan perhitungan matematika berikutnya.

Tapi saat mengonversi dari `float` ke representasi desimal dan _kembali_
ke `float`, pasti pakai `FLT_DECIMAL_DIG` agar semua bit terpertahankan
persis. \index{FLT\_DECMIAL\_DIG macro@\texttt{FLT\_DECMIAL\_DIG} macro|)}

\index{Significant digits|)}

## Tipe Konstanta Numerik

Saat kamu menulis konstanta angka, seperti `1234`, ia punya tipe. Tapi
tipe apa? Mari kita lihat bagaimana C menentukan tipe konstanta itu, dan
cara memaksanya memilih tipe tertentu.

### Heksadesimal dan Oktal

Selain desimal biasa seperti buatan nenek, C juga mendukung konstanta
dengan basis berbeda.

\index{0x hexadecimal@\texttt{0x} hexadecimal|(}

Kalau angka diawali `0x`, ia dibaca sebagai angka hex:

``` {.c}
int a = 0x1A2B;   // Heksadesimal
int b = 0x1a2b;   // Huruf besar/kecil tidak berpengaruh untuk digit hex

printf("%x", a);  // Cetak angka hex, "1a2b"
```

\index{0x hexadecimal@\texttt{0x} hexadecimal|)}


\index{0 octal@\texttt{0} octal|(}
Kalau angka diawali `0`, ia dibaca sebagai angka oktal:

``` {.c}
int a = 012;

printf("%o\n", a);  // Cetak angka oktal, "12"
```

Ini terutama jadi masalah bagi programmer pemula yang mencoba mengisi
angka desimal dengan `0` di kiri agar rapi dan sejajar, dan tanpa sadar
mengubah basis angkanya:

``` {.c}
int x = 11111;  // Desimal 11111
int y = 00111;  // Desimal 73 (Oktal 111)
int z = 01111;  // Desimal 585 (Oktal 1111)
```

\index{0 octal@\texttt{0} octal|)}

#### Catatan tentang Biner

\index{0b binary@\texttt{0b} binary|(}

Ekstensi tidak resmi^[Aku sungguh heran C belum memasukkan ini ke
spesifikasi. Di dokumen C99 Rationale, tertulis, "A proposal to add binary
constants was rejected due to lack of precedent and insufficient utility."
Padahal itu terdengar agak konyol, mengingat fitur-fitur lain yang
dijejalkan ke sana! Aku berani taruhan salah satu rilis berikutnya akan
memilikinya.] di banyak compiler C memungkinkan kita menulis bilangan
biner dengan awalan `0b`:

``` {.c}
int x = 0b101010;    // Biner 101010

printf("%d\n", x);   // Mencetak 42 desimal
```

Tidak ada format specifier `printf()` untuk mencetak bilangan biner. Kamu
harus melakukannya satu karakter demi satu karakter dengan operator
bitwise.

\index{0b binary@\texttt{0b} binary|)}

### Konstanta Integer

\index{Integer constants|(}

Kamu bisa memaksa konstanta integer menjadi tipe tertentu dengan
menambahkan sufiks yang menunjukkan tipenya.

Kita akan melakukan beberapa penugasan sebagai demo, tapi paling sering
developer tidak menulis sufiks kecuali perlu presisi. Compiler cukup
pandai memastikan tipe-tipenya kompatibel.

\index{U unsigned constant@\texttt{U} unsigned constant|(}
\index{L long constant@\texttt{L} long constant|(}
\index{LL long long constant@\texttt{LL} long long constant|(}
\index{UL unsigned long constant@\texttt{UL} unsigned long constant|(}
\index{ULL unsigned long long constant@\texttt{ULL} unsigned long long constant|(}

``` {.c}
int           x = 1234;
long int      x = 1234L;
long long int x = 1234LL

unsigned int           x = 1234U;
unsigned long int      x = 1234UL;
unsigned long long int x = 1234ULL;
```

Sufiks boleh huruf besar maupun kecil. Dan `U` serta `L` atau `LL` boleh
muncul dalam urutan mana pun.

|Tipe|Sufiks|
|:-|:-|
|`int`|Tidak ada|
|`long int`|`L`|
|`long long int`|`LL`|
|`unsigned int`|`U`|
|`unsigned long int`|`UL`|
|`unsigned long long int`|`ULL`|

Tadi di tabel kusebut bahwa "tanpa sufiks" berarti `int`... tapi
sebenarnya lebih rumit dari itu.

Jadi apa yang terjadi kalau ada angka tanpa sufiks seperti:

``` {.c}
int x = 1234;
```

Bertipe apa angka itu?

Umumnya C memilih tipe terkecil, mulai dari `int`, yang bisa menampung
nilainya.

Tapi lebih spesifik lagi, itu juga bergantung pada basis angkanya
(desimal, hex, atau oktal).

Spesifikasi punya tabel bagus yang menunjukkan tipe mana yang dipakai
untuk nilai tanpa sufiks. Bahkan, tabel itu akan kusalin utuh di sini.

C11 §6.4.4.1¶5 berbunyi, "The type of an integer constant is the first of
the first of the corresponding list in which its value can be
represented."

Lalu dilanjutkan dengan tabel ini:

+----------------+------------------------+-------------------------+
|Sufiks          |Konstanta Desimal       |Konstanta Oktal atau\    |
|                |                        |Heksadesimal             |
+:===============+:=======================+:========================+
|tidak ada       |`int`\                  |`int`\                   |
|                |`long int`              |`unsigned int`\          |
|                |                        |`long int`\              |
|                |                        |`unsigned long int`\     |
|                |                        |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u` atau `U`    |`unsigned int`\         |`unsigned int`\          |
|                |`unsigned long int`\    |`unsigned long int`\     |
|                |`unsigned long long int`|`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`l` atau `L`    |`long int`\             |`long int`\              |
|                |`long long int`         |`unsigned long int`\     |
|                |                        |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u`/`U` dan\    |`unsigned long int`\    |`unsigned long int`\     |
|`l`/`L`         |`unsigned long long int`|`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`ll` atau `LL`  |`long long int`         |`long long int`\         |
|                |                        |`unsigned long long int`\|
|                |                        |                         |
+----------------+------------------------+-------------------------+
|`u`/`U` dan\    |`unsigned long long int`|`unsigned long long int` |
|`ll`/`LL`       |                        |                         |
+----------------+------------------------+-------------------------+

\index{L long constant@\texttt{L} long constant|)}
\index{LL long long constant@\texttt{LL} long long constant|)}
\index{UL unsigned long constant@\texttt{UL} unsigned long constant|)}
\index{ULL unsigned long long constant@\texttt{ULL} unsigned long long constant|)}

Artinya, misalnya kalau kamu menulis angka seperti `123456789U`, mula-mula
C akan melihat apakah ia bisa menjadi `unsigned int`. Kalau tidak muat,
dicoba `unsigned long int`. Lalu `unsigned long long int`. Ia memakai tipe
terkecil yang bisa menampung angka itu.

\index{U unsigned constant@\texttt{U} unsigned constant|)}

\index{Integer constants|)}

### Konstanta Floating Point

\index{Floating point constants|(}

Kamu mungkin mengira konstanta floating point seperti `1.23` bertipe
bawaan `float`, kan?

Kejutan! Ternyata bilangan floating point tanpa sufiks bertipe `double`!
Selamat ulang tahun yang terlambat!

\index{F float constant@\texttt{F} float constant|(}
\index{L long double constant@\texttt{L} long double constant|(}

Kamu bisa memaksanya menjadi `float` dengan menambahkan `f` (atau
`F`---tidak membedakan huruf besar/kecil). Kamu bisa memaksanya menjadi
`long double` dengan menambahkan `l` (atau `L`).

|Tipe|Sufiks|
|:-|:-|
|`float`|`F`|
|`double`|Tidak ada|
|`long double`|`L`|

Misalnya:

``` {.c}
float x       = 3.14f;
double x      = 3.14;
long double x = 3.14L;
```

\index{F float constant@\texttt{F} float constant|)}
\index{L long double constant@\texttt{L} long double constant|)}

Tapi selama ini kita melakukan ini saja, kan?

``` {.c}
float x = 3.14;
```

Bukankah sisi kiri `float` dan sisi kanan `double`? Betul! Tapi C cukup
pandai soal konversi numerik otomatis, jadi lebih umum menulis konstanta
floating point tanpa sufiks daripada dengan sufiks. Nanti dibahas lagi.

\index{Floating point constants|)}

#### Notasi Ilmiah

\index{Scientific notation|(}

Ingat tadi saat kita membahas bahwa bilangan floating point bisa
direpresentasikan dengan significand, basis, dan eksponen?

Nah, ada cara umum menulis bilangan seperti itu, ditampilkan di sini
diikuti padanannya yang lebih mudah dikenali, yaitu hasil perhitungan
sebenarnya:

$1.2345\times10^3 = 1234.5$

Menulis angka dalam bentuk $s\times b^e$ disebut [_notasi
ilmiah_](https://en.wikipedia.org/wiki/Scientific_notation)^[https://en.wikipedia.org/wiki/Scientific_notation]. Di C, ini ditulis dengan "notasi E", jadi
dua bentuk ini setara:

|Notasi Ilmiah|Notasi E|
|:-|:-|
|$1.2345\times10^{-3}=0.0012345$|`1.2345e-3`|
|$1.2345\times10^8=123450000$|`1.2345e+8`|

Kamu bisa mencetak angka dalam notasi ini dengan `%e`:

``` {.c}
printf("%e\n", 123456.0);  // Mencetak 1.234560e+05
```

Beberapa fakta kecil yang menyenangkan tentang notasi ilmiah:

* Angka tidak harus ditulis dengan satu digit di depan titik desimal.
  Berapa pun jumlah angka boleh ditaruh di depan.

  ``` {.c}
  double x = 123.456e+3;  // 123456
  ```

  Tapi saat dicetak, eksponennya akan diubah sehingga hanya ada satu
  digit di depan titik desimal.

* Tanda plus pada eksponen boleh dihilangkan karena itu bawaannya, tapi
  dari yang pernah kulihat, ini jarang dipakai.

  ``` {.c}
  1.2345e10 == 1.2345e+10
  ```
* Kamu bisa menambahkan sufiks `F` atau `L` pada konstanta notasi E:

  ``` {.c}
  1.2345e10F
  1.2345e10L
  ```

\index{Scientific notation|)}

#### Konstanta Floating Point Heksadesimal

\index{Hex floating point constants|(}

Tapi tunggu, masih ada lagi yang mengambang!

Ternyata ada juga konstanta floating point heksadesimal!

Cara kerjanya mirip bilangan floating point desimal, tapi diawali `0x`
seperti angka integer.

Syaratnya, kamu _harus_ menyebutkan eksponen, dan eksponen ini
menghasilkan pangkat 2. Yaitu: $2^x$.

Lalu kamu memakai `p` alih-alih `e` saat menulis angkanya:

Jadi `0xa.1p3` adalah $10.0625\times2^3 == 80.5$.

Saat memakai konstanta hex floating point, 
Kita bisa mencetak notasi ilmiah hex dengan `%a`:

``` {.c}
double x = 0xa.1p3;

printf("%a\n", x);  // 0x1.42p+6
printf("%f\n", x);  // 80.500000
```

\index{Hex floating point constants|)}




<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe III: Konversi

\index{Type conversions|(}

Di bab ini kita akan membahas konversi dari satu tipe ke tipe lain. C
punya berbagai cara untuk melakukannya, dan sebagian mungkin sedikit
berbeda dari yang biasa kamu temui di bahasa lain.

Sebelum membahas cara membuat konversi terjadi, mari kita lihat dulu
bagaimana konversi bekerja saat ia _memang_ terjadi.

## Konversi String

\index{Type conversions!strings|(}

Tidak seperti banyak bahasa, C tidak melakukan konversi string ke angka
(dan sebaliknya) semulus konversi antartipe numerik.

Untuk itu, kita harus memanggil fungsi yang mengerjakan pekerjaan
kotornya.

### Nilai Numerik ke String

Ketika ingin mengonversi angka menjadi string, kita bisa memakai
`sprintf()` (dibaca _SPRINT-f_) atau `snprintf()`
(_s-n-print-f_)^[Keduanya sama, kecuali `snprintf()` memungkinkan kita
menentukan jumlah byte maksimum yang ditulis, sehingga mencegah
kelebihan menulis melewati ujung string. Jadi lebih aman.]

Keduanya pada dasarnya bekerja seperti `printf()`, hanya saja outputnya
ke string, dan string itu bisa dicetak nanti, atau diapakan saja.

Misalnya, mengubah sebagian nilai π menjadi string:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    char s[10];
    float f = 3.14159;

    // Konversi "f" menjadi string, simpan di "s", tulis paling banyak 10
    // karakter termasuk terminator NUL

    snprintf(s, 10, "%f", f);

    printf("String value: %s\n", s);  // String value: 3.141590
}
```

Jadi kamu bisa memakai `%d` atau `%u` seperti biasa untuk integer.

### String ke Nilai Numerik

Ada beberapa keluarga fungsi untuk ini di C. Kita sebut keluarga `atoi`
(dibaca _a-to-i_) dan keluarga `strtol` (_stir-to-long_).

Untuk konversi dasar dari string ke angka, coba fungsi `atoi` dari
`<stdlib.h>`. Fungsi-fungsi ini punya penanganan error yang buruk
(termasuk undefined behavior kalau kamu mengoper string yang salah), jadi
pakailah dengan hati-hati.

|Fungsi|Deskripsi|
|:-|:-|
|`atoi`|String ke `int`|
|`atof`|String ke `float`|
|`atol`|String ke `long int`|
|`atoll`|String ke `long long int`|

Walaupun spesifikasi tidak mengakuinya, huruf `a` di awal nama fungsi
singkatan dari [ASCII](https://en.wikipedia.org/wiki/ASCII)^[https://en.wikipedia.org/wiki/ASCII], jadi sebenarnya `atoi()` adalah
"ASCII-to-integer", tapi mengatakannya hari ini agak terlalu berpusat
pada ASCII.

Berikut contoh mengonversi string menjadi `float`:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *pi = "3.14159";
    float f;

    f = atof(pi);

    printf("%f\n", f);
}
```

Tapi, seperti kubilang tadi, hal-hal aneh seperti ini
menghasilkan undefined behavior:

``` {.c}
int x = atoi("what");  // "What" bukan angka yang pernah dikenal siapa pun
```

(Waktu kujalankan, hasilnya `0`, tapi jangan sekali-kali
mengandalkannya. Hasilnya bisa sama sekali berbeda.)

Untuk penanganan error yang lebih baik, mari kita lihat fungsi-fungsi
`strtol`, yang juga ada di `<stdlib.h>`. Bukan hanya itu, mereka juga
mengonversi ke lebih banyak tipe dan lebih banyak basis!

|Fungsi|Deskripsi|
|:-|:-|
|`strtol`|String ke `long int`|
|`strtoll`|String ke `long long int`|
|`strtoul`|String ke `unsigned long int`|
|`strtoull`|String ke `unsigned long long int`|
|`strtof`|String ke `float`|
|`strtod`|String ke `double`|
|`strtold`|String ke `long double`|

Fungsi-fungsi ini semuanya punya pola pemakaian yang mirip, dan bagi
banyak orang ini pengalaman pertama dengan pointer ke pointer! Tapi tenang
saja---tidak sesulit kelihatannya.

Mari kita coba contoh ketika string dikonversi menjadi `unsigned long`,
dengan membuang informasi error (yaitu informasi tentang karakter yang
salah di string input):

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "3490";

    // Konversi string s, angka basis 10, menjadi unsigned long int.
    // NULL berarti kita tidak peduli dengan informasi error apa pun.

    unsigned long int x = strtoul(s, NULL, 10);

    printf("%lu\n", x);  // 3490
}
```

Perhatikan beberapa hal di sana. Meskipun kita tidak sudi menangkap
informasi apa pun tentang karakter yang salah di string, `strtoul()` tidak
akan menghasilkan undefined behavior; ia hanya mengembalikan `0`.

Perhatikan juga bahwa kita menyebutkan bahwa ini angka desimal (basis 10).

Apakah ini berarti kita bisa mengonversi angka dengan basis berbeda?
Tentu! Kita coba biner!

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "101010";  // Apa makna angka ini?

    // Konversi string s, angka basis 2, menjadi unsigned long int.

    unsigned long int x = strtoul(s, NULL, 2);

    printf("%lu\n", x);  // 42
}
```

Oke, itu semua seru-seruan, tapi `NULL` di situ untuk apa?

Itu membantu kita mengetahui apakah terjadi error saat memproses string.
Ia adalah pointer ke pointer ke `char`, yang terdengar menakutkan, tapi
tidak lagi begitu setelah kamu paham.

Mari kita coba contoh dengan sengaja memberi angka yang salah, dan kita
lihat bagaimana `strtol()` memberi tahu di mana digit tidak valid yang
pertama.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "34x90";  // "x" bukan digit yang valid di basis 10!
    char *badchar;

    // Konversi string s, angka basis 10, menjadi unsigned long int.

    unsigned long int x = strtoul(s, &badchar, 10);

    // Ia mencoba mengonversi sebanyak mungkin, jadi sampai sini:

    printf("%lu\n", x);  // 34

    // Tapi kita bisa melihat karakter bermasalah itu karena badchar
    // menunjuk ke sana!

    printf("Invalid character: %c\n", *badchar);  // "x"
}
```

Jadi di sana `strtoul()` mengubah apa yang ditunjuk `badchar` untuk
menunjukkan di mana yang salah^[Kita harus mengoper pointer ke `badchar`
ke `strtoul()`, kalau tidak ia tidak akan bisa mengubahnya dengan cara yang
terlihat oleh kita, sama seperti alasan kenapa kamu harus mengoper pointer
ke `int` ke fungsi kalau ingin fungsi itu bisa mengubah nilai `int` itu.].

Tapi bagaimana kalau tidak ada yang salah? Dalam kasus itu, `badchar`
akan menunjuk ke terminator `NUL` di akhir string. Jadi kita bisa mengujinya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *s = "3490";  // "x" bukan digit yang valid di basis 10!
    char *badchar;

    // Konversi string s, angka basis 10, menjadi unsigned long int.

    unsigned long int x = strtoul(s, &badchar, 10);

    // Periksa apakah semuanya berjalan baik

    if (*badchar == '\0') {
        printf("Success! %lu\n", x);
    } else  {
        printf("Partial conversion: %lu\n", x);
        printf("Invalid character: %c\n", *badchar);
    }
}
```

Jadi begitulah. Fungsi gaya `atoi()` cukup baik untuk keadaan terkendali,
tapi fungsi gaya `strtol()` memberimu kendali jauh lebih besar atas
penanganan error dan basis input.

\index{Type conversions!strings|)}

## Konversi `char`

\index{Type conversions!char@\texttt{char}|(}

Bagaimana kalau ada satu karakter berisi digit, seperti `'5'`... Apakah
itu sama dengan nilai `5`?

Kita coba dan lihat.

``` {.c}
printf("%d %d\n", 5, '5');
```

Di sistemku yang memakai UTF-8, ini mencetak:

``` {.default}
5 53
```

Jadi... bukan. Dan 53? Apa itu? Itu code point UTF-8 (dan ASCII)
untuk simbol karakter `'5'`^[Setiap karakter punya nilai yang terkait
dengannya pada skema character encoding mana pun.]

Lalu bagaimana cara mengonversi karakter `'5'` (yang ternyata bernilai 53)
menjadi nilai `5`?

Dengan satu trik cerdik, begitulah caranya!

Standar C menjamin bahwa karakter-karakter ini punya code point yang
berurutan dan dalam urutan ini:

``` {.default}
0  1  2  3  4  5  6  7  8  9
```

Renungkan sebentar--bagaimana kita bisa memakainya? Awas, spoiler...

Mari kita lihat karakter-karakter itu beserta code point-nya di UTF-8:

``` {.default}
0  1  2  3  4  5  6  7  8  9
48 49 50 51 52 53 54 55 56 57
```

Kamu bisa lihat di sana bahwa `'5'` adalah `53`, persis seperti yang kita
dapat tadi. Dan `'0'` adalah `48`.

Jadi kita bisa mengurangkan `'0'` dari karakter digit mana pun untuk
mendapatkan nilai numeriknya:

``` {.c}
char c = '6';

int x = c;  // x bernilai 54, code point untuk '6'

int y = c - '0'; // y bernilai 6, persis seperti yang kita mau
```

Dan kita bisa mengonversi ke arah sebaliknya, cukup dengan menambahkan
nilainya.

``` {.c}
int x = 6;

char c = x + '0';  // c bernilai 54

printf("%d\n", c);  // mencetak 54
printf("%c\n", c);  // mencetak 6 dengan %c
```

Mungkin kamu mengira ini cara yang aneh untuk melakukan konversi, dan
menurut standar zaman sekarang, memang begitu. Tapi di zaman dahulu kala
ketika komputer dibuat dari kayu secara harfiah, inilah metode untuk
konversi itu. Dan karena tidak rusak, C tidak pernah memperbaikinya.

\index{Type conversions!char@\texttt{char}|)}

## Konversi Numerik

\index{Type conversions!numeric|(}

### Boolean

\index{Type conversions!Boolean}

Kalau nol dikonversi ke `bool`, hasilnya `0`. Selain itu hasilnya `1`.

### Konversi Integer ke Integer

\index{Type conversions!integer|(}

Kalau sebuah tipe integer dikonversi menjadi unsigned dan nilainya tidak
muat, hasil unsigned-nya berputar seperti odometer sampai muat di
unsigned itu^[Dalam praktiknya, implementasimu kemungkinan cuma membuang
bit-bit orde tinggi dari hasil, jadi angka
16-bit `0x1234` yang dikonversi menjadi angka 8-bit berakhir sebagai
`0x0034`, atau cukup `0x34`.].

Kalau sebuah tipe integer dikonversi menjadi angka signed dan tidak muat,
hasilnya ditentukan oleh implementasi! Sesuatu yang terdokumentasi akan
terjadi, tapi kamu harus mencarinya sendiri^[Lagi-lagi, dalam praktiknya,
di sistemmu kemungkinan pola bit angka aslinya
dipotong lalu dipakai begitu saja untuk merepresentasikan angka signed,
two's complement. Misalnya, sistemku mengambil `unsigned char`
`192` dan mengonversinya menjadi `signed char` `-64`. Dalam two's
complement, pola bit kedua angka ini adalah biner `11000000`.]

### Konversi Integer dan Floating Point

\index{Type conversions!floating point|(}

Kalau tipe floating point dikonversi menjadi tipe integer, bagian
pecahannya dibuang tanpa ampun^[Sebenarnya tidak---cuma dibuang seperti
biasa.].

Tapi---dan ini jebakannya---kalau angkanya terlalu besar untuk muat di
integer, hasilnya undefined behavior. Jadi jangan lakukan itu.

Dari integer atau floating point ke floating point, C berusaha sebaik
mungkin mencari bilangan floating point terdekat dengan integer itu.

Sekali lagi, kalau nilai aslinya tidak bisa direpresentasikan, hasilnya
undefined behavior.

\index{Type conversions!floating point|)}
\index{Type conversions!integer|)}

## Konversi Implisit

\index{Type conversions!implicit|(}

Konversi ini dilakukan compiler secara otomatis ketika kamu mencampur
berbagai tipe.

### Integer Promotion {#integer-promotions}

Di sejumlah tempat, kalau sebuah `int` bisa dipakai untuk merepresentasikan
nilai dari `char` atau `short` (signed maupun unsigned), nilai itu
_dipromosikan_ naik menjadi `int`. Kalau tidak muat di `int`, ia
dipromosikan menjadi `unsigned int`.

Begitulah cara kita bisa melakukan hal seperti ini:

``` {.c}
char x = 10, y = 20;
int i = x + y;
```

Dalam kasus itu, `x` dan `y` dipromosikan oleh C menjadi `int` sebelum
perhitungan terjadi.

Integer promotion terjadi saat Usual Arithmetic Conversion, pada fungsi
variadic^[Fungsi dengan jumlah argumen yang bervariasi.], operator unary
`+` dan `-`, atau saat mengoper nilai ke fungsi tanpa
prototype^[Ini jarang dilakukan karena compiler akan protes dan punya
prototype adalah _Hal yang Benar_. Kurasa ini masih berfungsi karena
alasan historis, sebelum prototype ada.].

### Usual Arithmetic Conversion {#usual-arithmetic-conversions}

Konversi otomatis ini dikerjakan C pada operasi numerik yang kamu
minta. (Memang itulah namanya, omong-omong, menurut C11 §6.3.1.8.)
Perhatikan bahwa di bagian ini kita hanya membicarakan tipe numerik---string
dibahas nanti.

Konversi ini menjawab pertanyaan tentang apa yang terjadi ketika tipe
dicampur, seperti ini:

``` {.c}
int x = 3 + 1.2;   // Mencampur int dan double
                   // 4.2 dikonversi menjadi int
                   // 4 disimpan di x

float y = 12 * 2;  // Mencampur float dan int
                   // 24 dikonversi menjadi float
                   // 24.0 disimpan di y
```

Apakah mereka menjadi `int`? Atau `float`? Bagaimana cara kerjanya?

Berikut langkah-langkahnya, diparafrasekan agar mudah dicerna.

1. Kalau salah satu bagian ekspresi bertipe floating, konversikan bagian
   lainnya ke tipe floating itu.

2. Kalau tidak, dan kedua tipe adalah tipe integer, lakukan integer
   promotion pada masing-masing, lalu jadikan tipe operand sebesar yang
   dibutuhkan untuk menampung nilai terbesar yang sama. Kadang ini
   melibatkan perubahan signed menjadi unsigned.

Kalau ingin tahu detail lengkapnya, lihat C11 §6.3.1.8. Tapi kemungkinan
besar kamu tidak perlu.

Pokoknya ingat bahwa tipe int menjadi tipe float kalau ada tipe floating
point di mana pun di dalamnya, dan compiler berusaha memastikan tipe
integer campuran tidak overflow.

Terakhir, kalau kamu mengonversi dari satu tipe floating point ke tipe
floating point lain, compiler akan berusaha membuat konversi yang tepat.
Kalau tidak bisa, ia akan membuat perkiraan terbaik yang ia bisa. Kalau
angkanya terlalu besar untuk muat di tipe tujuan, _duar_: undefined
behavior!

### `void*`

Tipe `void*` menarik karena bisa dikonversi dari atau ke tipe pointer
mana pun.

``` {.c}
int x = 10;

void *p = &x;  // &x bertipe int*, tapi kita simpan di void*

int *q = p;    // p adalah void*, tapi kita simpan di int*
```

\index{Type conversions!implicit|)}

## Konversi Eksplisit

\index{Type conversions!explicit|(}

Konversi jenis ini harus kamu minta sendiri; compiler tidak akan
melakukannya untukmu.

Kamu bisa mengonversi dari satu tipe ke tipe lain dengan menugaskan satu
tipe ke tipe lain memakai `=`.

Kamu juga bisa mengonversi secara eksplisit dengan _cast_.

\index{Type conversions!numeric|)}

### Casting

\index{Type conversions!casting|(}

Kamu bisa mengubah tipe sebuah ekspresi secara eksplisit dengan menaruh
tipe baru dalam tanda kurung di depannya. Sebagian developer C tidak
menyukai praktik ini kecuali benar-benar perlu, tapi kemungkinan besar
kamu akan menemui kode C yang memakainya.

Mari kita coba contoh ketika kita ingin mengonversi `int` menjadi `long`
supaya bisa disimpan di `long`.

Catatan: contoh ini dibuat-buat dan cast di sini sama sekali tidak perlu
karena ekspresi `x + 12` akan otomatis diubah menjadi `long int` agar
sesuai dengan tipe `y` yang lebih lebar.

``` {.c}
int x = 10;
long int y = (long int)x + 12;
```

Pada contoh itu, meskipun `x` tadinya bertipe `int`, ekspresi
`(long int)x` bertipe `long int`. Kita bilang, "Kita meng-cast `x`
menjadi `long int`."

Lebih sering, kamu mungkin melihat cast dipakai untuk mengonversi `void*`
menjadi tipe pointer tertentu agar bisa di-dereference.

Callback dari fungsi bawaan `qsort()` bisa menunjukkan perilaku ini
karena `void*` dioper kepadanya:

``` {.c}
int compar(const void *elem1, const void *elem2)
{
    if (*((const int*)elem2) > *((const int*)elem1)) return 1;
    if (*((const int*)elem2) < *((const int*)elem1)) return -1;
    return 0;
}
```

Tapi kamu juga bisa menulisnya dengan jelas lewat penugasan:

``` {.c}
int compar(const void *elem1, const void *elem2)
{
    const int *e1 = elem1;
    const int *e2 = elem2;

    return *e2 - *e1;
}
```

Salah satu tempat yang lebih sering memakai cast adalah untuk menghindari
peringatan saat mencetak nilai pointer dengan `%p` yang jarang dipakai,
yang rewel terhadap apa pun selain `void*`:

``` {.c}
int x = 3490;
int *p = &x;

printf("%p\n", p);
```

menghasilkan peringatan ini:

``` {.default}
warning: format ‘%p’ expects argument of type ‘void *’, but argument
         2 has type ‘int *’
```

Kamu bisa memperbaikinya dengan cast:

``` {.c}
printf("%p\n", (void *)p);
```

Tempat lainnya adalah pada perubahan pointer eksplisit, kalau kamu tidak
ingin memakai `void*` sebagai perantara, tapi ini juga cukup jarang:

``` {.c}
long x = 3490;
long *p = &x;
unsigned char *c = (unsigned char *)p;
```

Tempat ketiga yang sering mewajibkannya adalah pada fungsi konversi
karakter di
[`<ctype.h>`](https://beej.us/guide/bgclr/html/split/ctype.html)^[https://beej.us/guide/bgclr/html/split/ctype.html]
yang mengharuskan kamu meng-cast nilai yang tandanya meragukan menjadi
`unsigned char` untuk menghindari undefined behavior.

Sekali lagi, cast jarang sekali _dibutuhkan_ dalam praktik. Kalau kamu
mendapati dirimu sering meng-cast, mungkin ada cara lain untuk melakukan
hal yang sama, atau mungkin kamu meng-cast secara tidak perlu.

Atau mungkin memang perlu. Secara pribadi, aku berusaha
menghindarinya, tapi tidak takut memakainya kalau harus.

\index{Type conversions!explicit|)}
\index{Type conversions!casting|)}
\index{Type conversions|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe IV: Qualifier dan Specifier

Sekarang setelah kita punya lebih banyak tipe, ternyata tipe-tipe ini bisa
diberi atribut tambahan yang mengatur perilakunya. Inilah _type qualifier_
dan _storage-class specifier_.

## Type Qualifier

\index{Type qualifiers|(}

Dengan ini kamu bisa mendeklarasikan nilai konstan, dan juga memberi
petunjuk optimasi kepada compiler yang bisa dimanfaatkannya.

### `const`

\index{const type qualifier@\texttt{const} type qualifier|(}

Ini type qualifier yang paling umum kamu temui. Artinya variabel itu
konstan, dan setiap usaha mengubahnya akan membuat compiler sangat marah.

``` {.c}
const int x = 2;

x = 4;  // SUARA COMPILER MUNTAH, tidak bisa menugaskan ke konstanta
```

Nilai `const` tidak bisa diubah.

Sering kali kamu melihat `const` di daftar parameter fungsi:

``` {.c}
void foo(const int x)
{
    printf("%d\n", x + 30);  // OK, tidak mengubah "x"
}
```

#### `const` dan Pointer

\index{const type qualifier@\texttt{const} type qualifier!and pointers|(}

Ini agak membingungkan, karena ada dua pemakaian yang punya dua makna
ketika menyangkut pointer.

Pertama, kita bisa membuat agar nilai yang ditunjuk pointer tidak bisa
diubah. Caranya dengan menaruh `const` di depan nama tipe (sebelum
asterisk) pada deklarasi tipe.

``` {.c}
int x[] = {10, 20};
const int *p = x; 

p++;  // Kita bisa mengubah p, tidak masalah

*p = 30; // Error compiler! Tidak bisa mengubah yang ditunjuknya
```

Agak membingungkan, dua hal ini setara:

``` {.c}
const int *p;  // Tidak bisa mengubah yang ditunjuk p
int const *p;  // Tidak bisa mengubah yang ditunjuk p, sama seperti di atas
```

Bagus, jadi kita tidak bisa mengubah yang ditunjuk pointer, tapi kita bisa
mengubah pointer-nya sendiri. Bagaimana kalau kita ingin sebaliknya? Kita
ingin bisa mengubah yang ditunjuk pointer, tapi _tidak_ bisa mengubah
pointer-nya sendiri?

Cukup pindahkan `const` ke setelah asterisk pada deklarasi:

``` {.c}
int *const p;   // Kita tidak bisa mengubah "p" dengan aritmetika pointer

p++;  // Error compiler!
```

Tapi kita bisa mengubah yang ditunjuknya:

``` {.c}
int x = 10;
int *const p = &x;

*p = 20;   // Ubah "x" menjadi 20, tidak masalah
```

Kamu juga bisa membuat keduanya `const`:

``` {.c}
const int *const p;  // Tidak bisa mengubah p maupun *p!
```

Terakhir, kalau ada beberapa tingkat indirection, kamu harus memberi
`const` pada tingkat yang sesuai. Hanya karena sebuah pointer `const`,
bukan berarti pointer yang ditunjuknya juga harus `const`. Kamu bisa
mengaturnya secara eksplisit seperti pada contoh-contoh berikut:

``` {.c}
char **p;
p++;     // OK!
(*p)++;  // OK!

char **const p;
p++;     // Error!
(*p)++;  // OK!

char *const *p;
p++;     // OK!
(*p)++;  // Error!

char *const *const p;
p++;     // Error!
(*p)++;  // Error!
```

\index{const type qualifier@\texttt{const} type qualifier!and pointers|)}

#### `const` Correctness

\index{const type qualifier@\texttt{const} type qualifier!correctness|(}

Ada satu hal lagi yang harus kusebutkan: compiler akan memberi peringatan
untuk kode seperti ini:

``` {.c}
const int x = 20;
int *p = &x;
```

dengan pesan yang kira-kira berbunyi:

``` {.default}
initialization discards 'const' qualifier from pointer type target
```

Apa yang terjadi di sana?

Kita perlu melihat tipe di kedua sisi penugasan:

``` {.c}
    const int x = 20;
    int *p = &x;
//    ^       ^
//    |       |
//  int*    const int*
```

Compiler memperingatkan bahwa nilai di sisi kanan penugasan itu `const`,
tapi yang di sisi kiri tidak. Compiler memberi tahu bahwa ia membuang
sifat "const" dari ekspresi di sisi kanan.

Artinya, kita _masih bisa_ mencoba hal berikut, tapi itu salah. Compiler
akan memperingatkan, dan hasilnya undefined behavior:

``` {.c}
const int x = 20;
int *p = &x;

*p = 40;  // Undefined behavior--mungkin mengubah "x", mungkin tidak!

printf("%d\n", x);  // 40, kalau beruntung
```

\index{const type qualifier@\texttt{const} type qualifier!correctness|)}
\index{const type qualifier@\texttt{const} type qualifier|)}

### `restrict`

\index{restrict type qualifier@\texttt{restrict} type qualifier|(}

Singkatnya: kamu tidak pernah wajib memakai ini dan boleh mengabaikannya
setiap kali melihatnya. Kalau dipakai dengan benar, kemungkinan ada sedikit
peningkatan performa. Kalau dipakai dengan salah, kamu akan mendapat
undefined behavior.

`restrict` adalah petunjuk bagi compiler bahwa sepotong memori tertentu
hanya akan diakses oleh satu pointer dan tidak pernah oleh pointer lain.
(Artinya, tidak ada aliasing pada object yang ditunjuk pointer
`restrict` itu.) Kalau developer mendeklarasikan pointer sebagai
`restrict` lalu mengakses object yang ditunjuknya dengan cara lain (misalnya
lewat pointer lain), perilakunya undefined.

Pada dasarnya kamu berkata kepada C, "Hei---kujamin pointer tunggal ini
satu-satunya cara aku mengakses memori ini, dan kalau aku bohong, silakan
beri aku undefined behavior."

Dengan informasi itu, C bisa mengoptimasi hal-hal tertentu. Misalnya,
kalau kamu men-dereference pointer `restrict` berulang kali dalam loop, C
mungkin memutuskan menyimpan hasilnya di register dan hanya menyimpan
hasil akhirnya setelah loop selesai. Kalau ada pointer lain yang merujuk
memori yang sama dan mengaksesnya di dalam loop, hasilnya tidak akan
akurat.

(Perhatikan bahwa `restrict` tidak berpengaruh kalau object yang ditunjuk
tidak pernah ditulis. Semuanya tentang optimasi seputar penulisan ke
memori.)

Mari kita tulis fungsi untuk menukar dua variabel, dan kita pakai kata
kunci `restrict` untuk meyakinkan C bahwa kita tidak akan pernah mengoper
pointer ke hal yang sama. Lalu mari kita sengaja membuat kesalahan dan mencoba mengoper pointer
ke hal yang sama.

``` {.c .numberLines}
void swap(int *restrict a, int *restrict b)
{
    int t;

    t = *a;
    *a = *b;
    *b = t;
}

int main(void)
{
    int x = 10, y = 20;

    swap(&x, &y);  // OK! "a" dan "b" di atas menunjuk ke hal yang berbeda

    swap(&x, &x);  // Undefined behavior! "a" dan "b" sama!
}
```

Kalau kata kunci `restrict` di atas dihapus, kedua pemanggilan akan aman.
Tapi compiler mungkin tidak bisa mengoptimasi.

`restrict` punya block scope, artinya pembatasannya hanya berlaku selama
scope tempat ia dipakai. Kalau ada di daftar parameter fungsi, ia ada di
block scope fungsi itu.

Kalau pointer yang dibatasi menunjuk ke array, pembatasan itu hanya berlaku
pada object individual di array. Pointer lain boleh membaca dan menulis
array itu selama tidak membaca atau menulis elemen yang sama dengan yang
dibatasi.

Kalau `restrict` dipakai di file scope, di luar fungsi apa pun, pembatasannya
mencakup seluruh program.

Kamu kemungkinan besar akan melihatnya di fungsi library seperti `printf()`:

``` {.c}
int printf(const char * restrict format, ...);
```

Sekali lagi, itu hanya memberi tahu compiler bahwa di dalam fungsi
`printf()`, hanya akan ada satu pointer yang merujuk ke bagian mana pun
dari string `format` itu.

Satu catatan terakhir: kalau karena suatu alasan kamu memakai notasi array
pada parameter fungsi alih-alih notasi pointer, kamu bisa memakai
`restrict` seperti ini:

``` {.c}
void foo(int p[restrict])     // Tanpa ukuran

void foo(int p[restrict 10])  // Atau dengan ukuran
```

Tapi notasi pointer lebih umum.

\index{restrict type qualifier@\texttt{restrict} type qualifier|)}

### `volatile`

\index{volatile type qualifier@\texttt{volatile} type qualifier|(}

Kecil kemungkinan kamu melihat atau membutuhkan ini kecuali kamu
berurusan langsung dengan hardware.

`volatile` memberi tahu compiler bahwa sebuah nilai mungkin berubah di
luar pengetahuannya dan harus dibaca ulang setiap kali.

Contohnya ketika compiler membaca memori di sebuah alamat yang terus
diperbarui di balik layar, misalnya semacam timer hardware.

Kalau compiler memutuskan mengoptimasinya dan menyimpan nilai di register
dalam waktu lama, nilai di memori akan berubah dan tidak tercermin di
register.

Dengan mendeklarasikan sesuatu `volatile`, kamu berkata kepada compiler,
"Hei, yang ditunjuk ini mungkin berubah kapan saja karena alasan di luar
kode program ini."

``` {.c}
volatile int *p;
```

### `_Atomic`

Ini fitur C opsional yang akan kita bahas di [bab Atomics](#chapter-atomics).

\index{volatile type qualifier@\texttt{volatile} type qualifier|)}
\index{Type qualifiers|)}

## Storage-Class Specifier

\index{Storage-Class Specifiers|(}

Storage-class specifier mirip type qualifier. Mereka memberi compiler
informasi tambahan tentang tipe sebuah variabel.

### `auto`

\index{auto storage class@\texttt{auto} storage class|(}

Kata kunci ini hampir tidak pernah terlihat, karena `auto` adalah bawaan
untuk variabel block scope. Ia tersirat.

Keduanya sama:

``` {.c}
{
    int a;         // auto adalah bawaannya...
    auto int a;    // Jadi ini mubazir
}
```

Kata kunci `auto` menandakan bahwa object ini punya _automatic storage
duration_. Artinya, ia ada di scope tempat ia didefinisikan, dan otomatis
didealokasikan saat scope itu ditinggalkan.

Satu jebakan pada variabel otomatis: nilainya tak tentu sampai kamu
menginisialisasinya secara eksplisit. Kita bilang isinya "acak" atau
"sampah", walau aku sendiri kurang puas dengan keduanya. Pokoknya,
kamu tidak akan tahu isinya kecuali kamu menginisialisasinya.

\index{auto storage class@\texttt{auto} storage class|)}

Selalu inisialisasi semua variabel otomatis sebelum dipakai!

### `static` {#static}

\index{static storage class@\texttt{static} storage class|(}

Kata kunci ini punya dua makna, tergantung apakah variabelnya file scope
atau block scope.

Kita mulai dengan block scope.

#### `static` di Block Scope

\index{static storage class@\texttt{static} storage class!in block scope|(}

Dalam kasus ini, intinya kita berkata, "Aku cuma ingin satu instance
variabel ini ada, dipakai bersama antarpemanggilan."

Artinya, nilainya bertahan di antara pemanggilan.

`static` di block scope dengan initializer hanya diinisialisasi satu kali
saat program mulai, bukan setiap kali fungsi dipanggil.

Kita coba contoh:

``` {.c .numberLines}
#include <stdio.h>

void counter(void)
{
    static int count = 1;  // Ini diinisialisasi satu kali

    printf("This has been called %d time(s)\n", count);

    count++;
}

int main(void)
{
    counter();  // "This has been called 1 time(s)"
    counter();  // "This has been called 2 time(s)"
    counter();  // "This has been called 3 time(s)"
    counter();  // "This has been called 4 time(s)"
}
```

Lihat bagaimana nilai `count` bertahan di antara pemanggilan?

Satu hal yang perlu dicatat: variabel `static` block scope diinisialisasi
ke `0` secara bawaan.

``` {.c}
static int foo;      // Nilai awal bawaannya `0`...
static int foo = 0;  // Jadi penugasan `0` ini mubazir
```

Terakhir, perlu diingat bahwa kalau kamu menulis program multithread,
kamu harus memastikan beberapa thread tidak saling menginjak variabel yang
sama.

\index{static storage class@\texttt{static} storage class!in block scope|)}

#### `static` di File Scope

\index{static storage class@\texttt{static} storage class!in file scope|(}

Ketika kita sampai di file scope, di luar blok mana pun, maknanya agak
berubah.

Variabel di file scope sudah bertahan di antara pemanggilan fungsi, jadi
perilaku itu sudah ada.

Sebagai gantinya, dalam konteks ini `static` berarti variabel tersebut tidak
terlihat di luar file sumber ini. Mirip "global", tapi hanya di
file ini.

Lebih lanjut dibahas di bagian tentang membangun dengan banyak file
sumber.

\index{static storage class@\texttt{static} storage class!in file scope|)}
\index{static storage class@\texttt{static} storage class|)}

### `extern` {#extern}

\index{extern storage class@\texttt{extern} storage class|(}

Storage-class specifier `extern` memberi kita cara untuk merujuk ke object
di file sumber lain.

Katakanlah, misalnya, file `bar.c` isinya hanya ini:

``` {.c .numberLines}
// bar.c

int a = 37;
```

Hanya itu. Mendeklarasikan `int a` baru di file scope.

Tapi bagaimana kalau ada file sumber lain, `foo.c`, dan kita ingin merujuk
ke `a` yang ada di `bar.c`?

Mudah dengan kata kunci `extern`:

``` {.c .numberLines}
// foo.c

extern int a;

int main(void)
{
    printf("%d\n", a);  // 37, dari bar.c!

    a = 99;

    printf("%d\n", a);  // "a" yang sama dari bar.c, tapi sekarang 99
}
```

Kita juga bisa menaruh `extern int a` di block scope, dan ia tetap akan
merujuk ke `a` di `bar.c`:

``` {.c .numberLines}
// foo.c

int main(void)
{
    extern int a;

    printf("%d\n", a);  // 37, dari bar.c!

    a = 99;

    printf("%d\n", a);  // "a" yang sama dari bar.c, tapi sekarang 99
}
```

Nah, kalau `a` di `bar.c` ditandai `static`, ini tidak akan berfungsi.
Variabel `static` di file scope tidak terlihat di luar file itu.

Catatan terakhir tentang `extern` pada fungsi. Untuk fungsi, `extern`
adalah bawaan, jadi mubazir. Kamu bisa mendeklarasikan fungsi `static`
kalau hanya ingin fungsi itu terlihat di satu file sumber.

\index{extern storage class@\texttt{extern} storage class|)}

### `register`

\index{register storage class@\texttt{register} storage class|(}

Ini kata kunci untuk memberi petunjuk ke compiler bahwa variabel ini
sering dipakai, dan harus dibuat secepat mungkin diaksesnya. Compiler tidak
wajib menyetujuinya.

Sekarang, optimizer compiler C modern cukup pandai menentukan hal ini
sendiri, jadi jarang terlihat belakangan ini. Ada beberapa argumen untuk
memakainya, yang kucantumkan di bawah. Meski begitu, jujur saja, aku
belum pernah memakainya di kode C mana pun yang pernah kutulis.

Tapi kalau memang harus:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    register int a;   // Buat "a" secepat mungkin dipakai.

    for (a = 0; a < 10; a++)
        printf("%d\n", a);
}
```

Tapi ada harganya. Kamu tidak bisa mengambil alamat sebuah register:

``` {.c}
register int a;
int *p = &a;    // ERROR COMPILER! Tidak bisa mengambil alamat register
```

Hal yang sama berlaku untuk bagian mana pun dari array, karena
memperlakukan array `register` sebagai pointer adalah undefined behavior.
Kamu boleh mengambil `sizeof()`-nya. Tapi itu bukan keuntungan. Artinya
array `register` pada dasarnya tidak berguna.

GCC dan Clang sama-sama error pada semua yang berikut (kecuali GCC, anehnya,
tidak error pada `a[2]`; ia hanya memberi peringatan kalau kamu memakai
`-pedantic`):

``` {.c}
register int a[] = {11, 22, 33, 44, 55};
int *p = a;       // Undefined behavior!
int b = *(a + 2); // Undefined behavior!
int c = a[2];     // Undefined behavior!
```

Fakta bahwa alamat variabel register tidak bisa diambil membebaskan
compiler untuk melakukan optimasi berdasarkan asumsi itu kalau belum
menemukannya sendiri. Selain itu, menambahkan `register` pada variabel
`const` mencegah seseorang tanpa sengaja mengoper pointer-nya ke fungsi
lain yang dengan sengaja mengabaikan sifat
konstannya^[https://gustedt.wordpress.com/2010/08/17/a-common-misconsception-the-register-keyword/].

Sedikit latar belakang sejarah: jauh di dalam CPU ada "variabel" kecil
khusus bernama [_register_](https://en.wikipedia.org/wiki/Processor_register)^[https://en.wikipedia.org/wiki/Processor_register]. Aksesnya sangat cepat
dibanding RAM, jadi memakainya memberi dorongan kecepatan. Tapi mereka
tidak ada di RAM, jadi tidak punya alamat memori (itulah kenapa kamu tidak
bisa mengambil address-of atau pointer ke mereka).

Tapi, seperti kubilang tadi, compiler modern sangat pandai menghasilkan
kode optimal, memakai register kapan pun mungkin terlepas dari apakah kamu
menulis kata kunci `register` atau tidak. Bukan hanya itu, spesifikasi
membolehkan mereka memperlakukannya seolah-olah kamu menulis `auto`, kalau
mereka mau. Jadi tidak ada jaminan.

\index{register storage class@\texttt{register} storage class|)}

### `_Thread_local`

\index{\_Thread\_local storage class@\texttt{\_Thread\_local} storage class|(}

Saat memakai banyak thread dan ada variabel di global atau block scope
`static`, ini cara untuk memastikan setiap thread mendapat salinannya
sendiri dari variabel itu. Ini membantu menghindari race condition dan
thread yang saling menginjak kaki.

Kalau berada di block scope, kamu harus memakainya bersama `extern` atau
`static`.

Selain itu, kalau kamu menyertakan `<threads.h>`, kamu bisa memakai
`thread_local` yang jauh lebih enak dilihat sebagai alias dari
`_Thread_local` yang lebih jelek.

Informasi lebih lanjut ada di [bagian Threads](#thread-local).

\index{\_Thread\_local storage class@\texttt{\_Thread\_local} storage class|)}
\index{Storage-Class Specifiers|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Proyek Multifile

\index{Multifile projects|(}

Sejauh ini kita melihat program-program mainan yang sebagian besar muat di
satu file. Tapi program C yang kompleks terdiri dari banyak file yang
semuanya dikompilasi dan di-link menjadi satu executable.

Di bab ini kita akan melihat beberapa pola dan praktik umum untuk
menyusun proyek yang lebih besar.

## Include dan Function Prototype  {#includes-func-protos}

\index{Multifile projects!includes|(}
\index{Multifile projects!function prototypes|(}

Situasi yang sangat umum: sebagian fungsimu didefinisikan di satu file,
dan kamu ingin memanggilnya dari file lain.

Sebenarnya ini langsung berfungsi dengan sebuah peringatan... kita coba
dulu, lalu kita lihat cara yang benar untuk memperbaiki peringatannya.

Untuk mengompilasinya, kamu perlu menyebutkan semua file sumber di command
line:

``` {.zsh}
# file output   file sumber
#     v            v
#   |----| |---------|
gcc -o foo foo.c bar.c
```

Pada contoh itu, `foo.c` dan `bar.c` dibangun menjadi executable bernama
`foo`.

Pada contoh-contoh ini, nama file kita taruh sebagai komentar pertama di
sumbernya. Kita lihat file sumber `bar.c`:

``` {.c .numberLines}
// File bar.c

int add(int x, int y)
{
    return x + y;
}
```

Dan file `foo.c` yang berisi main:

``` {.c .numberLines}
// File foo.c

#include <stdio.h>

int main(void)
{
    printf("%d\n", add(2, 3));  // 5!
}
```

Lihat bagaimana dari `main()` kita memanggil `add()`---tapi `add()` ada di
file sumber yang sama sekali berbeda! Ia ada di `bar.c`, sedangkan
pemanggilannya ada di `foo.c`!

Kalau kita membangunnya dengan:

``` {.zsh}
gcc -o foo foo.c bar.c
```

kita mendapat error ini:

``` {.default}
error: implicit declaration of function 'add' is invalid in C99
```

(Atau mungkin kamu mendapat peringatan, yang tidak boleh kamu abaikan.
Jangan pernah mengabaikan peringatan di C; tangani semuanya.)

Kalau kamu ingat dari [bagian tentang prototype](#prototypes), implicit
declaration dilarang di C modern dan tidak ada alasan sah untuk
memasukkannya ke kode baru. Kita harus memperbaikinya.

Maksud `implicit declaration` adalah kita memakai sebuah fungsi, yaitu
`add()` dalam kasus ini, tanpa memberi tahu C apa pun tentangnya
sebelumnya. C ingin tahu apa yang dikembalikannya, tipe argumen apa yang
diterimanya, dan hal-hal semacam itu.

Kita sudah melihat cara memperbaikinya dengan _function prototype_.
Memang, kalau kita menambahkannya ke `foo.c` sebelum pemanggilan,
semuanya berjalan baik:

``` {.c .numberLines}
// File foo.c

#include <stdio.h>

int add(int, int);  // Tambahkan prototype-nya

int main(void)
{
    printf("%d\n", add(2, 3));  // 5!
}
```

Tidak ada error lagi!

Tapi itu merepotkan---harus mengetik prototype setiap kali ingin memakai
sebuah fungsi. Maksudnya, kita memakai `printf()` di situ dan tidak perlu
mengetik prototype; kok bisa?

_Sebenarnya kita memang sudah menyertakan prototype `printf()`_! Ada di
file `stdio.h`! Dan kita meng-include-nya dengan `#include`!

Bisakah kita melakukan hal yang sama untuk fungsi `add()`? Membuat
prototype-nya dan menaruhnya di header file?

Tentu bisa!

Secara konvensi, header file di C berekstensi `.h`. Dan sering, tapi tidak
selalu, namanya sama dengan file `.c` pasangannya. Jadi mari kita buat
file `bar.h` untuk file `bar.c`, dan kita taruh prototype-nya di situ:

``` {.c .numberLines}
// File bar.h

int add(int, int);
```

Sekarang kita ubah `foo.c` agar meng-include file itu. Dengan asumsi file
itu ada di direktori yang sama, kita meng-include-nya di dalam tanda
kutip ganda (bukan kurung siku):

``` {.c .numberLines}
// File foo.c

#include <stdio.h>

#include "bar.h"  // Include dari direktori saat ini

int main(void)
{
    printf("%d\n", add(2, 3));  // 5!
}
```

Perhatikan bahwa prototype sudah tidak ada di `foo.c`---kita
meng-include-nya dari `bar.h`. Sekarang file _mana pun_ yang butuh
fungsionalitas `add()` cukup `#include "bar.h"`, dan kamu tidak perlu
repot mengetik function prototype.

Seperti yang mungkin sudah kamu duga, `#include` secara harfiah menyisipkan
file yang disebut _tepat di situ_ di kode sumbermu, seolah-olah kamu
mengetiknya sendiri.

Lalu build dan jalankan:

``` {.zsh}
./foo
5
```

Memang, kita mendapat hasil $2+3$! Hore!

Tapi jangan buka minuman pilihanmu dulu. Kita hampir sampai! Tinggal satu
boilerplate lagi yang harus ditambahkan.

\index{Multifile projects!function prototypes|)}

## Menangani Include yang Berulang

Bukan hal aneh kalau sebuah header file itu sendiri meng-`#include` header
lain yang dibutuhkan untuk fungsionalitas file C pasangannya. Maksudnya,
kenapa tidak?

Dan bisa jadi sebuah header di-`#include` berkali-kali dari tempat yang
berbeda. Mungkin itu tidak masalah, tapi mungkin juga menyebabkan error
compiler. Dan kita tidak bisa mengendalikan berapa banyak tempat yang
meng-`#include`-nya!

Bahkan lebih buruk, kita bisa terjebak situasi gila ketika header `a.h`
meng-include header `b.h`, dan `b.h` meng-include `a.h`! Siklus `#include`
tanpa akhir!

Mencoba membangun yang seperti itu menghasilkan error:

``` {.default}
error: #include nested depth 200 exceeds maximum of 200
```

Mungkin siklusnya akan terselesaikan pada langkah ke-201...

Kita perlu mengaturnya supaya kalau sebuah file sudah di-include satu
kali, `#include` berikutnya untuk file itu diabaikan.

**Langkah yang akan kita ambil ini sangat umum, jadi lakukan saja secara
otomatis setiap kali membuat header file!**

Cara umumnya adalah dengan variabel preprocessor yang kita set saat
pertama kali file di-`#include`. Lalu pada `#include` berikutnya, kita
periksa dulu bahwa variabel itu belum didefinisikan.

Untuk nama variabel itu, sangat umum mengambil nama header file, misalnya
`bar.h`, mengubahnya menjadi huruf besar, dan mengganti titik dengan
underscore: `BAR_H`.

Jadi taruh pengecekan di bagian paling, paling atas file untuk melihat apakah file
sudah pernah di-include, dan secara efektif mengomentari seluruh isinya
kalau sudah.

(Jangan memakai underscore di awal (karena underscore di awal yang diikuti
huruf kapital itu dicadangkan) atau dua underscore di awal (karena itu juga
dicadangkan).)

``` {.c .numberLines}
#ifndef BAR_H   // Jika BAR_H belum didefinisikan...
#define BAR_H   // Definisikan (tanpa nilai tertentu)

// File bar.h

int add(int, int);

#endif          // Akhir dari #ifndef BAR_H
```

Ini secara efektif membuat header file hanya di-include satu kali, berapa
pun banyaknya tempat yang mencoba meng-`#include`-nya.

\index{Multifile projects!includes|)}

## `static` dan `extern`

\index{static storage class@\texttt{static} storage class|(}
\index{extern storage class@\texttt{extern} storage class|(}
\index{Multifile projects!static storage class@\texttt{static} storage class|(}
\index{Multifile projects!extern storage class@\texttt{extern} storage class|(}

Kamu bisa merujuk ke object di file lain dengan `extern`.

Kamu bisa memastikan variabel dan fungsi di file scope _tidak_ terlihat dari file sumber lain (bahkan kalau `extern` dipakai) dengan kata kunci `static`.

Untuk info lebih lanjut, lihat bagian di buku ini tentang storage-class
specifier [`static`](#static) dan [`extern`](#extern).

\index{static storage class@\texttt{static} storage class|)}
\index{extern storage class@\texttt{extern} storage class|)}
\index{Multifile projects!static storage class@\texttt{static} storage class|)}
\index{Multifile projects!extern storage class@\texttt{extern} storage class|)}

## Mengompilasi dengan Object File

\index{Object files|(}

Ini bukan bagian dari spesifikasi, tapi 99,999% umum di dunia C.

Kamu bisa mengompilasi file C menjadi representasi perantara yang disebut
_object file_. Isinya machine code (yaitu 1 dan 0 dari instruksi
sebenarnya) yang belum dibangun menjadi executable. 

Object file di Windows berekstensi `.OBJ`; di sistem mirip Unix, `.o`.

\index{gcc compiler@\texttt{gcc} compiler|(}

Di gcc, kita bisa membangunnya seperti ini, dengan flag `-c` (hanya
kompilasi!):

``` {.zsh}
gcc -c foo.c     # menghasilkan foo.o
gcc -c bar.c     # menghasilkan bar.o
```

Lalu kita bisa me-_link_ keduanya menjadi satu executable:

``` {.zsh}
gcc -o foo foo.o bar.o
```

_Voila_, kita sudah menghasilkan executable `foo` dari dua object file.

Tapi kamu mungkin berpikir, buat apa repot-repot? Bukankah kita bisa
langsung:

``` {.zsh}
gcc -o foo foo.c bar.c
```

dan sekali tepuk membunuh dua [boid](https://en.wikipedia.org/wiki/Boids)^[https://en.wikipedia.org/wiki/Boids]?

\index{gcc compiler@\texttt{gcc} compiler|)}

Untuk program kecil, itu tidak masalah. Aku sendiri sering
melakukannya.

Tapi untuk program yang lebih besar, kita bisa memanfaatkan fakta bahwa
kompilasi dari sumber ke object file relatif lambat, sedangkan me-link
banyak object file relatif cepat.

Ini benar-benar terlihat pada utilitas `make` yang hanya membangun ulang
sumber yang lebih baru daripada outputnya.

Misalkan kamu punya seribu file C. Kamu bisa mengompilasi semuanya menjadi
object file di awal (lambat) lalu menggabungkan semua object file itu
menjadi executable (cepat).

Sekarang katakanlah kamu hanya mengubah satu dari file sumber C itu---ini
keajaibannya: _kamu hanya perlu membangun ulang satu object file untuk
file sumber itu_! Lalu membangun ulang executable-nya (cepat). Semua file
C lainnya tidak perlu disentuh.

Dengan kata lain, dengan hanya membangun ulang object file yang perlu,
kita memangkas waktu kompilasi secara drastis. (Kecuali tentu saja kalau
kamu melakukan build "bersih", yang mengharuskan semua object file
dibuat.)

\index{Object files|)}
\index{Multifile projects|)}



<!-- BG_NEW_CHAPTER -->

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

\index{Command line arguments|(}

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

\index{argc parameter@\texttt{argc} parameter|(}
\index{argv parameter@\texttt{argv} parameter|(}
\index{main() function@\texttt{main()} function!command line options|(}

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

\index{main() function@\texttt{main()} function!command line options|)}
\index{argc parameter@\texttt{argc} parameter|)}

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

\index{argc parameter@\texttt{argc} parameter|(}

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

\index{argc parameter@\texttt{argc} parameter|)}
\index{argv parameter@\texttt{argv} parameter|)}
\index{Command line arguments|)}

## Exit Status {#exit-status}

\index{Exit status|(}

Pernahkah kamu perhatikan bahwa signature fungsi `main()` mengembalikan
tipe `int`? Untuk apa itu? Ini berkaitan dengan yang disebut _exit
status_, yaitu integer yang bisa dikembalikan ke program yang
menjalankan programmu untuk memberi tahu bagaimana hasilnya.

Ada beberapa cara program keluar di C, termasuk `return` dari `main()`,
atau memanggil salah satu varian `exit()`.

Semua cara itu menerima sebuah `int` sebagai argumen.

\index{main() function@\texttt{main()} function!returning from|(}
Catatan sampingan: sudah lihat belum bahwa di hampir semua contohku,
meskipun `main()` seharusnya mengembalikan `int`, aku tidak menulis
`return` apa pun? Di fungsi lain, ini ilegal, tapi di C ada kasus khusus:
kalau eksekusi mencapai akhir `main()` tanpa menemukan `return`, ia
otomatis melakukan `return 0`.
\index{main() function@\texttt{main()} function!returning from|)}

Tapi apa arti `0` itu? Angka apa lagi yang bisa kita taruh di sana? Dan
bagaimana angka-angka itu dipakai?

Spesifikasi jelas sekaligus samar soal ini, seperti biasa. Jelas karena ia
merinci apa yang bisa dilakukan, tapi samar karena tidak membatasinya
secara khusus.

Tidak ada pilihan lain selain _maju terus_ dan mencari tahu!

Mari kita ber-[Inception](https://en.wikipedia.org/wiki/Inception)^[https://en.wikipedia.org/wiki/Inception] sebentar: ternyata ketika
kamu menjalankan programmu, _kamu menjalankannya dari program lain_.

Biasanya program lain ini semacam [shell](https://en.wikipedia.org/wiki/Shell_(computing))^[https://en.wikipedia.org/wiki/Shell_(computing)] yang
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

\index{EXIT\_SUCCESS macro@\texttt{EXIT\_SUCCESS} macro|(}
\index{EXIT\_FAILURE macro@\texttt{EXIT\_FAILURE} macro|(}

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

\index{EXIT\_SUCCESS macro@\texttt{EXIT\_SUCCESS} macro|)}
\index{EXIT\_FAILURE macro@\texttt{EXIT\_FAILURE} macro|)}

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

\index{Exit status!obtaining from shell|(}

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

\index{Exit status!obtaining from shell|)}

Menarik! Kita lihat bahwa di sistemku, \index{EXIT\_FAILURE macro@\texttt{EXIT\_FAILURE} macro}`EXIT_FAILURE` adalah `1`. Spesifikasi tidak menyebutkannya, jadi
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

\index{Exit status|)}

## Environment Variable {#env-var}

\index{Environment variables|(}

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

\index{getenv() function@\texttt{getenv()} function|(}

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

\index{getenv() function@\texttt{getenv()} function|)}

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

Kalau di sistem mirip Unix, lihat dokumentasi \index{putenv() function@\texttt{putenv()} function}`putenv()`, \index{setenv()@\texttt{setenv()}}`setenv()`, dan \index{unsetenv() function@\texttt{unsetenv()} function}`unsetenv()`. Di Windows, lihat \index{\_putenv() function@\texttt{\_putenv()} function}`_putenv()`.

### Environment Variable Alternatif di Sistem Mirip Unix

Kalau kamu di sistem mirip Unix, kemungkinan kamu punya beberapa cara lain
untuk mengakses environment variable. Perhatikan bahwa walaupun spesifikasi
menunjukkannya sebagai ekstensi umum, ini bukan benar-benar bagian dari
standar C. Meski demikian, ini bagian dari standar POSIX.

\index{environ variable@\texttt{environ} variable|(}

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

\index{environ variable@\texttt{environ} variable|)}
\index{env parameter@\texttt{env} parameter|(}

Cara non-standar lain untuk mendapat environment variable adalah sebagai
parameter `main()`. Cara kerjanya hampir sama, tapi kamu tidak perlu
menambahkan variabel `extern` `environ`. [Bahkan spesifikasi POSIX pun
tidak mendukung
ini](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exec.html)^[https://pubs.opengroup.org/onlinepubs/9699919799/functions/exec.html]
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

\index{env parameter@\texttt{env} parameter|)}
\index{Environment variables|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# C Preprocessor

\index{Preprocessor|(}

Sebelum programmu dikompilasi, ia sebenarnya melewati tahap yang disebut
_preprocessing_. Hampir seperti ada bahasa _di atas_ bahasa C yang berjalan
lebih dulu. Dan ia menghasilkan kode C, yang kemudian dikompilasi.

Kita sudah melihatnya sedikit lewat `#include`! Itulah C Preprocessor! Di
tempat ia menemukan directive itu, ia menyertakan file yang disebut tepat
di situ, seolah-olah kamu mengetiknya sendiri. Dan _baru kemudian_
compiler membangun semuanya.

Tapi ternyata kemampuannya jauh lebih besar daripada sekadar menyertakan
sesuatu. Kamu bisa mendefinisikan _makro_ yang disubstitusi... bahkan
makro yang menerima argumen!

## `#include`

\index{\#include directive@\texttt{\#include} directive|(}

Kita mulai dari yang sudah sering kita lihat. Ini tentu saja cara untuk
menyertakan sumber lain ke dalam sumbermu. Sangat umum dipakai dengan
header file.

Walaupun spesifikasi mengizinkan berbagai perilaku untuk `#include`, kita
akan mengambil pendekatan yang lebih pragmatis dan membahas cara kerjanya
di setiap sistem yang pernah kulihat.

Header file bisa dibagi dua kategori: sistem dan lokal. Yang bawaan,
seperti `stdio.h`, `stdlib.h`, `math.h`, dan sebagainya, di-include dengan
kurung siku:

``` {.c}
#include <stdio.h>
#include <stdlib.h>
```

Kurung siku memberi tahu C, "Hei, jangan cari header file ini di direktori
saat ini---cari di direktori include level sistem."

\index{\#include directive@\texttt{\#include} directive!local files|(}

Itu tentu saja menyiratkan bahwa harus ada cara untuk meng-include file
lokal dari direktori saat ini. Dan memang ada: dengan tanda kutip ganda:

``` {.c}
#include "myheader.h"
```

Atau kamu juga hampir pasti bisa mencari di direktori relatif memakai garis
miring dan titik, seperti ini:

``` {.c}
#include "mydir/myheader.h"
#include "../someheader.py"
```

Jangan pakai backslash (`\`) sebagai pemisah path di `#include`-mu!
Itu undefined behavior! Pakai garis miring biasa (`/`) saja, bahkan di
Windows.

Ringkasnya, pakai kurung siku (`<` dan `>`) untuk include sistem, dan
pakai tanda kutip ganda (`"`) untuk include pribadimu.

\index{\#include directive@\texttt{\#include} directive!local files|)}
\index{\#include directive@\texttt{\#include} directive|)}

## Makro Sederhana

\index{Preprocessor!macros|(}

_Makro_ adalah identifier yang _diperluas_ menjadi potongan kode lain
sebelum compiler sempat melihatnya. Anggap seperti placeholder---ketika
preprocessor melihat salah satu identifier itu, ia menggantinya dengan
nilai lain yang sudah kamu definisikan.

\index{\#define directive@\texttt{\#define} directive|(}

Kita melakukannya dengan `#define` (sering dibaca "pound define", atau
mungkin "hash define", tapi jarang, kalaupun ada, "octothorpe define").
Berikut contohnya:

``` {.c .numberLines}
#include <stdio.h>

#define HELLO "Hello, world"
#define PI 3.14159

int main(void)
{
    printf("%s, %f\n", HELLO, PI);
}
```

Di baris 3 dan 4 kita mendefinisikan beberapa makro. Di mana pun makro itu
muncul di kode (baris 8), mereka akan disubstitusi dengan nilai yang
didefinisikan.

Dari sudut pandang C compiler, itu persis seperti kalau kita menulis ini:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    printf("%s, %f\n", "Hello, world", 3.14159);
}
```

Lihat bagaimana `HELLO` diganti dengan `"Hello, world"` dan `PI` diganti
dengan `3.14159`? Dari sudut pandang compiler, itu persis seperti
nilai-nilai itu muncul langsung di kode.

Perhatikan bahwa makro tidak punya tipe tertentu, _per se_. Yang terjadi
hanyalah mereka diganti utuh dengan apa pun yang di-`#define`-kan. Kalau
kode C hasilnya tidak valid, compiler akan muntah.

Kamu juga bisa mendefinisikan makro tanpa nilai:

``` {.c}
#define EXTRA_HAPPY
```

dalam kasus itu, makro-nya ada dan terdefinisi, tapi didefinisikan sebagai
kosong. Jadi di mana pun ia muncul di teks, akan diganti dengan kosong.
Kita akan melihat kegunaannya nanti.

Secara konvensi, nama makro ditulis `HURUF_BESAR_SEMUA` walaupun secara
teknis itu tidak wajib.

\index{\#define directive@\texttt{\#define} directive!versus const@versus \texttt{const}|(}

Secara keseluruhan, ini memberimu cara untuk mendefinisikan nilai konstan
yang efektif bersifat global dan bisa dipakai di tempat _mana saja_.
Bahkan di tempat yang tidak bisa memakai variabel `const`, misalnya pada
`case` di `switch` dan panjang array tetap.

Meski begitu, perdebatan masih ramai di internet soal apakah variabel
`const` bertipe lebih baik daripada makro `#define` secara umum.

Ia juga bisa dipakai untuk mengganti atau memodifikasi kata kunci, konsep
yang sama sekali asing bagi `const`, walaupun praktik ini sebaiknya
jarang dilakukan.

\index{\#define directive@\texttt{\#define} directive!versus const@versus \texttt{const}|)}
\index{\#define directive@\texttt{\#define} directive|)}
\index{Preprocessor!macros|)}

## Kompilasi Kondisional

\index{Conditional compilation|(}

Preprocessor bisa diminta menentukan apakah blok kode tertentu
ditampilkan ke compiler atau dibuang sepenuhnya sebelum kompilasi.

Caranya dengan membungkus kode dalam blok kondisional, mirip pernyataan
`if`-`else`.

### Jika Terdefinisi, `#ifdef` dan `#endif`

Pertama, mari kita coba mengompilasi kode tertentu bergantung pada apakah
sebuah makro terdefinisi atau tidak.

\index{\#ifdef directive@\texttt{\#ifdef} directive|(}
\index{\#endif directive@\texttt{\#endif} directive|(}

``` {.c .numberLines}
#include <stdio.h>

#define EXTRA_HAPPY

int main(void)
{

#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#endif

    printf("OK!\n");
}
```

Pada contoh itu, kita mendefinisikan `EXTRA_HAPPY` (sebagai kosong, tapi
ia _terdefinisi_), lalu di baris 8 kita memeriksa apakah ia terdefinisi
dengan directive `#ifdef`. Kalau terdefinisi, kode setelahnya disertakan
sampai `#endif`.

\index{\#ifdef directive@\texttt{\#ifdef} directive|)}
\index{\#endif directive@\texttt{\#endif} directive|)}

Jadi karena terdefinisi, kode itu disertakan untuk kompilasi dan
outputnya:

``` {.default}
I'm extra happy!
OK!
```

Kalau `#define`-nya kita jadikan komentar, seperti ini:

``` {.c}
//#define EXTRA_HAPPY
```

maka ia tidak terdefinisi, dan kode itu tidak disertakan dalam kompilasi.
Dan outputnya hanya:

``` {.default}
OK!
```

Penting diingat bahwa keputusan ini terjadi saat compile time! Kodenya
benar-benar dikompilasi atau dibuang tergantung kondisi. Ini berbeda
dengan pernyataan `if` biasa yang dievaluasi saat program berjalan.

### Jika Tidak Terdefinisi, `#ifndef`

Ada juga bentuk negatif dari "jika terdefinisi": "jika tidak
terdefinisi", atau `#ifndef`. Kita bisa mengubah contoh sebelumnya
supaya mengeluarkan hal berbeda tergantung apakah sesuatu terdefinisi:

\index{\#ifndef directive@\texttt{\#ifndef} directive|(}
\index{\#endif directive@\texttt{\#endif} directive|(}

``` {.c .numberLines startFrom="8"}
#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#endif

#ifndef EXTRA_HAPPY
    printf("I'm just regular\n");
#endif
```

Kita akan lihat cara yang lebih rapi untuk itu di bagian berikutnya.

Kalau dikaitkan kembali ke header file, kita sudah melihat bagaimana
header file bisa dibuat hanya di-include satu kali dengan
membungkusnya dalam directive preprocessor seperti ini:

``` {.c}
#ifndef MYHEADER_H  // Baris pertama myheader.h
#define MYHEADER_H

int x = 12;

#endif  // Baris terakhir myheader.h
```

\index{\#ifndef directive@\texttt{\#ifndef} directive|)}
\index{\#endif directive@\texttt{\#endif} directive|)}

Ini menunjukkan bagaimana sebuah makro bertahan lintas file dan beberapa
`#include`. Kalau belum terdefinisi, kita definisikan lalu kompilasi
seluruh header file.

Tapi saat di-include lagi, kita lihat bahwa `MYHEADER_H` _sudah_
terdefinisi, jadi kita tidak mengirim header file itu ke compiler---ia
secara efektif dibuang.


### `#else`

\index{\#else directive@\texttt{\#else} directive|(}

Tapi itu belum semua yang bisa kita lakukan! Ada juga `#else` yang bisa
kita campurkan.

\index{\#ifdef directive@\texttt{\#ifdef} directive|(}
\index{\#endif directive@\texttt{\#endif} directive|(}

Mari kita modifikasi contoh sebelumnya:

``` {.c .numberLines startFrom="8"}
#ifdef EXTRA_HAPPY
    printf("I'm extra happy!\n");
#else
    printf("I'm just regular\n");
#endif
```

\index{\#ifdef directive@\texttt{\#ifdef} directive|)}
\index{\#else directive@\texttt{\#else} directive|)}
\index{\#endif directive@\texttt{\#endif} directive|)}

Sekarang kalau `EXTRA_HAPPY` tidak terdefinisi, ia akan masuk ke klausa
`#else` dan mencetak:

``` {.default}
I'm just regular
```

### Else-If: `#elifdef`, `#elifndef`

\index{\#elifdef directive@\texttt{\#elifdef} directive|(}
\index{\#elifndef directive@\texttt{\#elifndef} directive|(}

Fitur ini baru di C23!

Bagaimana kalau kamu ingin sesuatu yang lebih kompleks? Mungkin kamu
butuh struktur rangkaian if-else supaya kodemu terbangun dengan benar?

Untungnya directive-directive ini tersedia. Kita bisa memakai `#elifdef`
untuk "else if defined":

``` {.c}
#ifdef MODE_1
    printf("This is mode 1\n");
#elifdef MODE_2
    printf("This is mode 2\n");
#elifdef MODE_3
    printf("This is mode 3\n");
#else
    printf("This is some other mode\n");
#endif
```

\index{\#elifdef directive@\texttt{\#elifdef} directive|)}

Sebaliknya, kamu bisa memakai `#elifndef` untuk "else if not defined".

\index{\#elifndef directive@\texttt{\#elifndef} directive|)}

### Kondisional Umum: `#if`, `#elif`

\index{\#if directive@\texttt{\#if} directive|(}
\index{\#elif directive@\texttt{\#elif} directive|(}

Ini bekerja sangat mirip directive `#ifdef` dan `#ifndef`, yaitu kamu juga
bisa punya `#else` dan semuanya ditutup dengan `#endif`.

Satu-satunya perbedaan: ekspresi konstan setelah `#if` harus bernilai true
(bukan nol) agar kode di dalam `#if` dikompilasi. Jadi alih-alih apakah
sesuatu terdefinisi atau tidak, kita ingin ekspresi yang bernilai true.

``` {.c .numberLines}
#include <stdio.h>

#define HAPPY_FACTOR 1

int main(void)
{

#if HAPPY_FACTOR == 0
    printf("I'm not happy!\n");
#elif HAPPY_FACTOR == 1
    printf("I'm just regular\n");
#else
    printf("I'm extra happy!\n");
#endif

    printf("OK!\n");
}
```

\index{\#elif directive@\texttt{\#elif} directive|)}

Sekali lagi, untuk klausa `#if` yang tidak cocok, compiler bahkan tidak
akan melihat baris-baris itu. Untuk kode di atas, setelah preprocessor
selesai, yang dilihat compiler hanyalah:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{

    printf("I'm just regular\n");

    printf("OK!\n");
}
```

Salah satu pemakaian akal-akalan dari hal ini adalah mengomentari banyak
baris dengan cepat^[Kamu tidak selalu bisa langsung membungkus kode
dengan komentar `/*` `*/` karena komentar itu tidak bisa bersarang.].

\index{\#if 0 directive@\texttt{\#if 0} directive|(}

Kalau kamu menaruh `#if 0` ("if false") di awal blok yang ingin
dikomentari dan `#endif` di akhir, kamu mendapat efek ini:

``` {.c}
#if 0
    printf("All this code"); /* is effectively */
    printf("commented out"); // by the #if 0
#endif
```

\index{\#if 0 directive@\texttt{\#if 0} directive|)}

Bagaimana kalau kamu memakai compiler pra-C23 dan tidak ada dukungan
directive `#elifdef` atau `#elifndef`? Bagaimana mendapat efek yang sama
dengan `#if`? Artinya, bagaimana kalau kita menginginkan ini:

``` {.c}
#ifdef FOO
    x = 2;
#elifdef BAR  // KEMUNGKINAN ERROR: Tidak didukung sebelum C23
    x = 3;
#endif
```

Bagaimana caranya?

Ternyata ada operator preprocessor bernama `defined` yang bisa dipakai
dengan pernyataan `#if`.

Ketiganya setara:

\index{\#if defined directive@\texttt{\#if defined} directive|(}
``` {.c}
#ifdef FOO
#if defined FOO
#if defined(FOO)   // Tanda kurung opsional
```

Begitu juga ketiga ini:

``` {.c}
#ifndef FOO
#if !defined FOO
#if !defined(FOO)   // Tanda kurung opsional
```

Perhatikan bahwa kita bisa memakai operator logika NOT standar (`!`) untuk
"tidak terdefinisi".

Jadi sekarang kita kembali ke dunia `#if` dan bisa memakai `#elif` sesuka
hati!

Kode yang rusak ini:

``` {.c}
#ifdef FOO
    x = 2;
#elifdef BAR  // KEMUNGKINAN ERROR: Tidak didukung sebelum C23
    x = 3;
#endif
```

bisa diganti dengan:

``` {.c}
#if defined FOO
    x = 2;
#elif defined BAR
    x = 3;
#endif
```

\index{\#if defined directive@\texttt{\#if defined} directive|)}
\index{\#if directive@\texttt{\#if} directive|)}
\index{Conditional compilation|)}

### Menghapus Makro: `#undef`

\index{\#undef directive@\texttt{\#undef} directive|(}

Kalau kamu sudah mendefinisikan sesuatu tapi tidak membutuhkannya lagi,
kamu bisa membatalkan definisinya dengan `#undef`.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
#define GOATS

#ifdef GOATS
    printf("Goats detected!\n");  // tercetak
#endif

#undef GOATS  // Buat GOATS tidak terdefinisi lagi

#ifdef GOATS
    printf("Goats detected, again!\n"); // tidak tercetak
#endif
}
```

\index{\#undef directive@\texttt{\#undef} directive|)}

## Makro Bawaan

\index{Preprocessor!predefined macros|(}

Standar mendefinisikan banyak makro bawaan yang bisa diuji dan dipakai
untuk kompilasi kondisional. Mari kita lihat di sini.


### Makro Wajib

Semua ini terdefinisi:

\index{\_\_DATE\_\_ macro@\texttt{\_\_DATE\_\_} macro|(}
\index{\_\_TIME\_\_ macro@\texttt{\_\_TIME\_\_} macro|(}
\index{\_\_FILE\_\_ macro@\texttt{\_\_FILE\_\_} macro|(}
\index{\_\_LINE\_\_ macro@\texttt{\_\_LINE\_\_} macro|(}
\index{\_\_func\_\_ identifier@\texttt{\_\_func\_\_} identifier|(}
\index{\_\_STDC\_VERSION\_\_ macro@\texttt{\_\_STDC\_VERSION\_\_} macro|(}

\index{\_\_STDC\_\_ macro@\texttt{\_\_STDC\_\_} macro}
\index{\_\_STDC\_HOSTED\_\_ macro@\texttt{\_\_STDC\_HOSTED\_\_} macro}

|Makro|Deskripsi|
|-----------|----------------------------------------------------|
|`__DATE__`|Tanggal kompilasi---yaitu saat file ini dikompilasi---dengan format `Mmm dd yyyy`|
|`__TIME__`|Waktu kompilasi dengan format `hh:mm:ss`|
|`__FILE__`|String berisi nama file ini|
|`__LINE__`|Nomor baris di file tempat makro ini muncul|
|`__func__`|Nama fungsi tempat ini muncul, sebagai string^[Ini sebenarnya bukan makro---secara teknis ia identifier. Tapi ia satu-satunya identifier bawaan dan terasa sangat seperti makro, jadi kumasukkan di sini. Seperti pemberontak.]|
|`__STDC__`|Bernilai `1` kalau ini C compiler standar|
|`__STDC_HOSTED__`|Bernilai `1` kalau compiler adalah _hosted implementation_^[Hosted implementation pada dasarnya berarti kamu menjalankan standar C yang lengkap, kemungkinan di atas semacam sistem operasi. Dan kemungkinan besar memang begitu keadaanmu. Kalau kamu berjalan di bare metal pada semacam embedded system, kemungkinan kamu memakai _standalone implementation_.], selain itu `0`|
|`__STDC_VERSION__`|Versi C ini, berupa konstanta `long int` dengan bentuk `yyyymmL`, mis. `201710L`|

Mari kita gabungkan semuanya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    printf("This function: %s\n", __func__);
    printf("This file: %s\n", __FILE__);
    printf("This line: %d\n", __LINE__);
    printf("Compiled on: %s %s\n", __DATE__, __TIME__);
    printf("C Version: %ld\n", __STDC_VERSION__);
}
```

\index{\_\_DATE\_\_ macro@\texttt{\_\_DATE\_\_} macro|)}
\index{\_\_TIME\_\_ macro@\texttt{\_\_TIME\_\_} macro|)}

Output di sistemku:

``` {.default}
This function: main
This file: foo.c
This line: 7
Compiled on: Nov 23 2020 17:16:27
C Version: 201710
```

`__FILE__`, `__func__` dan `__LINE__` sangat berguna untuk melaporkan
kondisi error dalam pesan kepada developer. Makro `assert()` di
`<assert.h>` memakai ketiganya untuk menunjukkan di bagian kode mana
assertion gagal.

\index{\_\_FILE\_\_ macro@\texttt{\_\_FILE\_\_} macro|)}
\index{\_\_LINE\_\_ macro@\texttt{\_\_LINE\_\_} macro|)}
\index{\_\_func\_\_ identifier@\texttt{\_\_func\_\_} identifier|)}

#### `__STDC_VERSION__`

\index{Language versions|(}

Kalau kamu penasaran, berikut nomor versi untuk rilis-rilis utama
Spesifikasi Bahasa C:

|Rilis|Versi ISO/IEC|`__STDC_VERSION__`|
|-|-|-|
|C89|ISO/IEC 9899:1990|tidak terdefinisi|
|**C89**|ISO/IEC 9899:1990/Amd.1:1995|`199409L`|
|**C99**|ISO/IEC 9899:1999|`199901L`|
|**C11**|ISO/IEC 9899:2011/Amd.1:2012|`201112L`|

Perhatikan bahwa makro ini awalnya tidak ada di C89.

Perhatikan juga bahwa rencananya nomor versi akan selalu naik, jadi kamu
selalu bisa memeriksa, misalnya, "setidaknya C99" dengan:

``` {.c}
#if __STDC_VERSION__ >= 1999901L
```

\index{Language versions|)}
\index{\_\_STDC\_VERSION\_\_ macro@\texttt{\_\_STDC\_VERSION\_\_} macro|)}

### Makro Opsional

Implementasimu mungkin mendefinisikan ini juga. Atau mungkin tidak.

\index{\_\_STDC\_ISO\_10646\_\_ macro@\texttt{\_\_STDC\_ISO\_10646\_\_} macro}
\index{\_\_STDC\_MB\_MIGHT\_NEQ\_WC\_\_ macro@\texttt{\_\_STDC\_MB\_MIGHT\_NEQ\_WC\_\_} macro}
\index{\_\_STDC\_UTF\_16\_\_ macro@\texttt{\_\_STDC\_UTF\_16\_\_} macro}
\index{\_\_STDC\_UTF\_32\_\_ macro@\texttt{\_\_STDC\_UTF\_32\_\_} macro}
\index{\_\_STDC\_ANALYZABLE\_\_ macro@\texttt{\_\_STDC\_ANALYZABLE\_\_} macro}
\index{\_\_STDC\_IEC\_559\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_\_} macro}
\index{\_\_STDC\_IEC\_559\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_COMPLEX\_\_} macro}
\index{\_\_STDC\_LIB\_EXT1\_\_ macro@\texttt{\_\_STDC\_LIB\_EXT1\_\_} macro}
\index{\_\_STDC\_NO\_ATOMICS\_\_ macro@\texttt{\_\_STDC\_NO\_ATOMICS\_\_} macro}
\index{\_\_STDC\_NO\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_NO\_COMPLEX\_\_} macro}
\index{\_\_STDC\_NO\_THREADS\_\_ macro@\texttt{\_\_STDC\_NO\_THREADS\_\_} macro}
\index{\_\_STDC\_NO\_VLA\_\_ macro@\texttt{\_\_STDC\_NO\_VLA\_\_} macro}

|Makro|Deskripsi|
|----------------|--------------------------------------------------|
|`__STDC_ISO_10646__`|Jika terdefinisi, `wchar_t` menyimpan nilai Unicode, selain itu sesuatu yang lain|
|`__STDC_MB_MIGHT_NEQ_WC__`|Nilai `1` menandakan bahwa nilai pada karakter multibyte mungkin tidak sama dengan nilai pada wide character|
|`__STDC_UTF_16__`|Nilai `1` menandakan bahwa sistem memakai encoding UTF-16 pada tipe `char16_t`|
|`__STDC_UTF_32__`|Nilai `1` menandakan bahwa sistem memakai encoding UTF-32 pada tipe `char32_t`|
|`__STDC_ANALYZABLE__`|Nilai `1` menandakan bahwa kode itu analyzable^[Oke, aku tahu itu jawaban yang mengelak. Pada dasarnya ada ekstensi opsional yang bisa diimplementasikan compiler, yaitu mereka setuju membatasi jenis undefined behavior tertentu supaya kode C lebih mudah dianalisis secara statis. Kecil kemungkinan kamu perlu memakainya.]|
|`__STDC_IEC_559__`|`1` jika floating point IEEE-754 (alias IEC 60559) didukung|
|`__STDC_IEC_559_COMPLEX__`|`1` jika floating point kompleks IEC 60559 didukung|
|`__STDC_LIB_EXT1__`|`1` jika implementasi ini mendukung berbagai fungsi standard library alternatif yang "aman" (nama mereka berakhiran `_s`)|
|`__STDC_NO_ATOMICS__`|`1` jika implementasi ini **tidak** mendukung `_Atomic` atau `<stdatomic.h>`|
|`__STDC_NO_COMPLEX__`|`1` jika implementasi ini **tidak** mendukung tipe kompleks atau `<complex.h>`|
|`__STDC_NO_THREADS__`|`1` jika implementasi ini **tidak** mendukung `<threads.h>`|
|`__STDC_NO_VLA__`|`1` jika implementasi ini **tidak** mendukung variable-length array|

\index{Preprocessor!predefined macros|)}

## Makro dengan Argumen

\index{Preprocessor!macros with arguments|(}

Makro lebih kuat daripada sekadar substitusi. Kamu bisa mengaturnya agar
menerima argumen yang ikut disubstitusi.

Pertanyaan yang sering muncul: kapan memakai makro berparameter versus
fungsi. Jawaban singkat: pakai fungsi. Tapi kamu akan melihat banyak makro
di alam liar dan di standard library. Orang cenderung memakainya untuk
hal-hal singkat yang bersifat matematis, dan juga untuk fitur yang mungkin
berubah dari satu platform ke platform lain. Kamu bisa mendefinisikan kata
kunci berbeda untuk satu platform atau lainnya.

### Makro dengan Satu Argumen

Kita mulai dengan yang sederhana, yang mengkuadratkan sebuah angka:

\index{\#define directive@\texttt{\#define} directive|(}

``` {.c .numberLines}
#include <stdio.h>

#define SQR(x) x * x  // Belum tepat, tapi ikuti saja dulu

int main(void)
{
    printf("%d\n", SQR(12));  // 144
}
```

Maksudnya: "di mana pun kamu melihat `SQR` dengan suatu nilai, ganti dengan
nilai itu dikali dirinya sendiri".

Jadi baris 7 akan diubah menjadi:

``` {.c .numberLines startFrom="7"}
    printf("%d\n", 12 * 12);  // 144
```

yang dengan nyaman dikonversi C menjadi 144.

Tapi kita membuat kesalahan elementer pada makro itu, kesalahan yang harus
dihindari.

Mari kita periksa. Bagaimana kalau kita ingin menghitung `SQR(3 + 4)`?
Nah, $3+4=7$, jadi tentu kita ingin menghitung $7^2=49$. Itu dia; `49`---
jawaban akhir.

Mari kita masukkan ke kode dan lihat bahwa kita mendapat... 19?

``` {.c .numberLines startFrom="7"}
    printf("%d\n", SQR(3 + 4));  // 19!!??
```

Apa yang terjadi?

Kalau kita ikuti perluasan makronya, kita mendapat 

``` {.c .numberLines startFrom="7"}
    printf("%d\n", 3 + 4 * 3 + 4);  // 19!
```

Waduh! Karena perkalian lebih diutamakan, kita menghitung $4\times3=12$
lebih dulu, dan mendapat $3+12+4=19$. Bukan yang kita inginkan.

Jadi kita harus memperbaikinya agar benar.

**Hal ini sangat umum, jadi lakukan saja secara otomatis setiap kali
membuat makro matematika berparameter!**

Perbaikannya mudah: tambahkan tanda kurung!

``` {.c .numberLines startFrom="3"}
#define SQR(x) (x) * (x)   // Lebih baik... tapi masih belum cukup bagus!
```

Dan sekarang makro kita diperluas menjadi:

``` {.c .numberLines startFrom="7"}
    printf("%d\n", (3 + 4) * (3 + 4));  // 49! Horeee!
```

Tapi sebenarnya kita masih punya masalah yang sama, yang bisa muncul kalau
ada operator berprioritas lebih tinggi daripada perkalian (`*`) di
dekatnya.

Jadi cara yang aman dan benar untuk menyusun makro itu adalah dengan
membungkus keseluruhannya dalam tanda kurung tambahan, seperti ini:

``` {.c .numberLines startFrom="3"}
#define SQR(x) ((x) * (x))   // Bagus!
```

Biasakan melakukan itu setiap kali membuat makro matematika dan kamu tidak
akan salah.

### Makro dengan Lebih dari Satu Argumen

Kamu bisa menumpuk ini sebanyak yang kamu mau:

``` {.c}
#define TRIANGLE_AREA(w, h) (0.5 * (w) * (h))
```

Mari kita buat beberapa makro yang menyelesaikan $x$ dengan rumus
kuadrat. Kalau-kalau kamu belum hafal, untuk persamaan
berbentuk:

$ax^2+bx+c=0$

kamu bisa mencari $x$ dengan rumus kuadrat:

$x=\displaystyle\frac{-b\pm\sqrt{b^2-4ac}}{2a}$

Rumus yang gila. Perhatikan juga plus-minus ($\pm$) di sana, yang menandakan
bahwa sebenarnya ada dua solusi.

Jadi mari kita buat makro untuk keduanya:

``` {.c}
#define QUADP(a, b, c) ((-(b) + sqrt((b)*(b) - 4 * (a) * (c))) / (2 * (a)))
#define QUADM(a, b, c) ((-(b) - sqrt((b)*(b) - 4 * (a) * (c))) / (2 * (a)))
```

Itu sudah memberi kita sedikit matematika. Tapi mari kita definisikan satu
lagi yang bisa dipakai sebagai argumen `printf()` untuk mencetak kedua
jawaban.

``` {.c}
//          makro              penggantian
//      |-----------| |----------------------------|
#define QUAD(a, b, c) QUADP(a, b, c), QUADM(a, b, c)
```

Itu hanya dua nilai yang dipisahkan koma---dan kita bisa memakainya sebagai
argumen "gabungan" untuk `printf()` seperti ini:

``` {.c}
printf("x = %f or x = %f\n", QUAD(2, 10, 5));
```

Mari kita satukan dalam kode:

``` {.c .numberLines}
#include <stdio.h>
#include <math.h>  // Untuk sqrt()

#define QUADP(a, b, c) ((-(b) + sqrt((b) * (b) - 4 * (a) * (c))) / (2 * (a)))
#define QUADM(a, b, c) ((-(b) - sqrt((b) * (b) - 4 * (a) * (c))) / (2 * (a)))
#define QUAD(a, b, c) QUADP(a, b, c), QUADM(a, b, c)

int main(void)
{
    printf("2*x^2 + 10*x + 5 = 0\n");
    printf("x = %f or x = %f\n", QUAD(2, 10, 5));
}
```

Dan ini memberi kita output:

``` {.default}
2*x^2 + 10*x + 5 = 0
x = -0.563508 or x = -4.436492
```

Memasukkan salah satu nilai itu memberi hasil kira-kira nol (agak meleset
karena angkanya tidak persis):

$2\times-0.563508^2+10\times-0.563508+5\approx0.000003$

### Makro dengan Argumen Variabel

\index{Preprocessor!macros with variable arguments|(}

Ada juga cara untuk mengoper jumlah argumen yang bervariasi ke makro,
dengan elipsis (`...`) setelah argumen bernama yang sudah diketahui. Saat
makro diperluas, semua argumen tambahan akan berada dalam daftar yang
dipisahkan koma di makro `__VA_ARGS__`, dan bisa diganti dari sana:

``` {.c .numberLines}
#include <stdio.h>

// Gabungkan dua argumen pertama menjadi satu angka,
// lalu jadikan sisanya daftar yang dipisahkan koma:

#define X(a, b, ...) (10*(a) + 20*(b)), __VA_ARGS__

int main(void)
{
    printf("%d %f %s %d\n", X(5, 4, 3.14, "Hi!", 12));
}
```

Substitusi yang terjadi di baris 10 adalah:

``` {.c .numberLines startFrom="10"}
    printf("%d %f %s %d\n", (10*(5) + 20*(4)), 3.14, "Hi!", 12);
```

dengan output:

``` {.default}
130 3.140000 Hi! 12
```

Kamu juga bisa men-"stringify" `__VA_ARGS__` dengan menaruh `#` di depannya:

``` {.c}
#define X(...) #__VA_ARGS__

printf("%s\n", X(1,2,3));  // Mencetak "1, 2, 3"
```

\index{\#define directive@\texttt{\#define} directive|)}
\index{Preprocessor!macros with variable arguments|)}
\index{Preprocessor!macros with arguments|)}

### Stringification

\index{\# stringification@\texttt{\#} stringification|(}

Sudah disebut di atas, kamu bisa mengubah argumen apa pun menjadi string
dengan menaruh `#` di depannya pada teks penggantian.

Misalnya, kita bisa mencetak apa pun sebagai string dengan makro ini dan
`printf()`:

``` {.c}
#define STR(x) #x

printf("%s\n", STR(3.14159));
```

Dalam kasus itu, substitusinya menghasilkan:

``` {.c}
printf("%s\n", "3.14159");
```

Mari kita lihat apakah ini bisa dipakai lebih efektif sehingga kita bisa
mengoper nama variabel `int` mana pun ke makro, dan makro itu mencetak
nama dan nilainya.

``` {.c .numberLines}
#include <stdio.h>

#define PRINT_INT_VAL(x) printf("%s = %d\n", #x, x)

int main(void)
{
    int a = 5;

    PRINT_INT_VAL(a);  // mencetak "a = 5"
}
```

Di baris 9, kita mendapat penggantian makro berikut:

``` {.c .numberLines startFrom="9"}
    printf("%s = %d\n", "a", a);
```

\index{\# stringification@\texttt{\#} stringification|)}

### Concatenation

\index{\#\# concatenation@\texttt{\#\#} concatenation|(}

Kita juga bisa menyambung dua argumen dengan `##`. Seru!

``` {.c}
#define CAT(a, b) a ## b

printf("%f\n", CAT(3.14, 1592));   // 3.141592
```

\index{\#\# concatenation@\texttt{\#\#} concatenation|)}

## Makro Multibaris

\index{Preprocessor!multiline macros|(}

Makro bisa dilanjutkan ke beberapa baris kalau newline di-escape dengan
backslash (`\`).

Mari kita tulis makro multibaris yang mencetak angka dari `0` sampai hasil
kali dua argumen yang dioper.

\index{do-while statement@\texttt{do}-\texttt{while} statement!in multiline macros|(}

``` {.c .numberLines}
#include <stdio.h>

#define PRINT_NUMS_TO_PRODUCT(a, b) do { \
    int product = (a) * (b); \
    for (int i = 0; i < product; i++) { \
        printf("%d\n", i); \
    } \
} while(0)

int main(void)
{
    PRINT_NUMS_TO_PRODUCT(2, 4);  // Mengeluarkan angka dari 0 sampai 7
}
```

Beberapa hal yang perlu dicatat:

* Escape di akhir setiap baris kecuali yang terakhir untuk menandakan
  bahwa makro berlanjut.
* Seluruhnya dibungkus dalam loop `do`-`while(0)` dengan kurung kurawal.

Poin kedua mungkin agak aneh, tapi semuanya soal menyerap `;` di
belakang yang dijatuhkan programmer setelah makro.

Awalnya aku mengira kurung kurawal saja sudah cukup, tapi ada kasus
yang gagal kalau programmer menaruh titik koma setelah makro. Inilah
kasusnya:

``` {.c .numberLines}
#include <stdio.h>

#define FOO(x) { (x)++; }

int main(void)
{
    int i = 0;

    if (i == 0)
        FOO(i);
    else
        printf(":-(\n");

    printf("%d\n", i);
}
```

Kelihatannya sederhana, tapi tidak akan terbangun tanpa syntax error:

``` {.default}
foo.c:11:5: error: ‘else’ without a previous ‘if’  
```

Kamu bisa lihat masalahnya?

Mari kita lihat perluasannya:

``` {.c}

    if (i == 0) {
        (i)++;
    };             // <-- Masalah dengan M huruf besar!

    else
        printf(":-(\n");
```

`;` itu mengakhiri pernyataan `if`, jadi `else` melayang-layang di sana
secara ilegal^[_Breakin' the law... breakin' the law..._].

Jadi bungkus makro multibaris itu dengan `do`-`while(0)`.

\index{do-while statement@\texttt{do}-\texttt{while} statement!in multiline macros|)}
\index{Preprocessor!multiline macros|)}

## Contoh: Makro Assert {#my-assert}

Menambahkan assert ke kodemu adalah cara bagus untuk menangkap kondisi
yang menurutmu seharusnya tidak terjadi. C menyediakan fungsionalitas
`assert()`. Ia memeriksa sebuah kondisi, dan kalau false, program
berhenti sambil memberi tahu file dan nomor baris tempat assertion gagal.

Tapi ini kurang memadai.

1. Pertama, kamu tidak bisa menambahkan pesan tambahan pada assert.
2. Kedua, tidak ada saklar on-off yang mudah untuk semua assert.

Yang pertama bisa kita atasi dengan makro.

Pada dasarnya, kalau aku punya kode ini:

``` {.c}
ASSERT(x < 20, "x must be under 20");
```

aku ingin sesuatu seperti ini terjadi (dengan asumsi `ASSERT()` ada di
baris 220 `foo.c`):

``` {.c}
if (!(x < 20)) {
    fprintf(stderr, "foo.c:220: assertion x < 20 failed: ");
    fprintf(stderr, "x must be under 20\n");
    exit(1);
}
```

Kita bisa mengambil nama file dari makro `__FILE__`, dan nomor baris dari
`__LINE__`. Pesannya sudah berupa string, tapi `x < 20` bukan, jadi kita
harus men-stringify-nya dengan `#`. Kita bisa membuat makro multibaris
dengan escape backslash di akhir baris.

``` {.c}
#define ASSERT(c, m) \
do { \
    if (!(c)) { \
        fprintf(stderr, __FILE__ ":%d: assertion %s failed: %s\n", \
                        __LINE__, #c, m); \
        exit(1); \
    } \
} while(0)
```

(Kelihatannya agak aneh dengan `__FILE__` di depan seperti itu, tapi ingat
ia adalah string literal, dan string literal yang bersebelahan otomatis
disambung. Sedangkan `__LINE__`, ia hanyalah `int`.)

Dan itu berfungsi! Kalau kujalankan ini:

``` {.c}
int x = 30;

ASSERT(x < 20, "x must be under 20");
```

aku mendapat output ini:

```
foo.c:23: assertion x < 20 failed: x must be under 20
```

Bagus sekali!

Tinggal satu hal lagi, yaitu cara menyalakan dan mematikannya, dan itu
bisa dilakukan dengan kompilasi kondisional.

Berikut contoh lengkapnya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

#define ASSERT_ENABLED 1

#if ASSERT_ENABLED
#define ASSERT(c, m) \
do { \
    if (!(c)) { \
        fprintf(stderr, __FILE__ ":%d: assertion %s failed: %s\n", \
                        __LINE__, #c, m); \
        exit(1); \
    } \
} while(0)
#else
#define ASSERT(c, m)  // Makro kosong kalau tidak diaktifkan
#endif

int main(void)
{
    int x = 30;

    ASSERT(x < 20, "x must be under 20");
}
```

Ini menghasilkan output:

``` {.default}
foo.c:23: assertion x < 20 failed: x must be under 20
```

## Directive `#error`

\index{\#error directive@\texttt{\#error} directive|(}

Directive ini membuat compiler langsung error begitu menemukannya.

Biasanya, ini dipakai di dalam kondisional untuk mencegah kompilasi kecuali
prasyarat tertentu terpenuhi:

``` {.c}
#ifndef __STDC_IEC_559__
    #error I really need IEEE-754 floating point to compile. Sorry!
#endif
```

\index{\#error directive@\texttt{\#error} directive|)}
\index{\#warning directive@\texttt{\#warning} directive|(}

Beberapa compiler punya directive `#warning` pelengkap yang non-standar,
yang mengeluarkan peringatan tapi tidak menghentikan kompilasi, namun ini
tidak ada di spesifikasi C11.

\index{\#warning directive@\texttt{\#warning} directive|)}

## Directive `#embed`

\index{\#embed directive@\texttt{\#embed} directive|(}

<!-- Godbolt demo: https://godbolt.org/z/Kb3ejE7q5 -->

Baru di C23!

Dan saat ini belum berfungsi di compiler mana pun milikku, jadi
jangan telan bagian ini bulat-bulat!

Intinya, kamu bisa menyertakan byte-byte dari sebuah file sebagai konstanta
integer seolah-olah kamu mengetiknya sendiri.

Misalnya, kalau kamu punya file biner bernama `foo.bin` yang berisi empat
byte bernilai desimal 11, 22, 33, dan 44, lalu kamu menulis ini:

``` {.c}
int a[] = {
#embed "foo.bin"
};
```

Hasilnya persis seperti kamu mengetik ini:

``` {.c}
int a[] = {11,22,33,44};
```

Ini cara yang sangat kuat untuk menginisialisasi array dengan data biner
tanpa perlu mengonversinya menjadi kode dulu---preprocessor yang
mengerjakannya untukmu!

Kasus pemakaian yang lebih khas mungkin file berisi gambar kecil yang
akan ditampilkan dan tidak ingin kamu muat saat runtime.

Berikut contoh lain:

``` {.c}
int a[] = {
#embed <foo.bin>
};
```

Kalau kamu memakai kurung siku, preprocessor mencari di serangkaian tempat
yang ditentukan implementasi untuk menemukan file, persis seperti
`#include`. Kalau kamu memakai tanda kutip ganda dan resource tidak
ditemukan, compiler akan mencobanya seolah-olah kamu memakai kurung siku
sebagai upaya putus asa terakhir untuk menemukan file itu.

`#embed` bekerja seperti `#include` dalam arti ia secara efektif
menempelkan nilai sebelum compiler melihatnya. Artinya kamu bisa
memakainya di berbagai tempat:

```
return
#embed "somevalue.dat"
;
```

atau

```
int x =
#embed "xvalue.dat"
;
```

Nah---apakah ini selalu byte? Maksudnya bernilai 0 sampai 255, inklusif?
Jawabannya, secara bawaan jelas "ya", kecuali saat jawabannya "tidak".

Secara teknis, elemennya selebar `CHAR_BIT` bit. Dan kemungkinan besar
itu 8 di sistemmu, jadi kamu mendapat rentang 0-255 pada nilainya.
(Nilainya selalu non-negatif.)

Mungkin juga implementasi membolehkan ini diubah dengan cara tertentu,
misalnya lewat command line atau parameter.

Ukuran file dalam bit harus kelipatan ukuran elemen. Artinya, kalau setiap
elemen 8 bit, ukuran file (dalam bit) harus kelipatan 8. Dalam pemakaian
sehari-hari, ini cara membingungkan untuk mengatakan bahwa setiap file
harus berupa jumlah byte yang bulat... yang tentu saja memang begitu.
Jujur saja, aku sendiri tidak yakin kenapa aku repot-repot menulis
paragraf ini. Baca spesifikasi kalau kamu benar-benar penasaran.

### Parameter `#embed`

Ada berbagai parameter yang bisa disebutkan pada directive `#embed`.
Berikut contoh dengan parameter `limit()` yang belum diperkenalkan:

``` {.c}
int a[] = {
#embed "/dev/random" limit(5)
};
```

Tapi bagaimana kalau `limit` sudah didefinisikan di tempat lain? Untungnya
kamu bisa menaruh `__` di sekeliling kata kuncinya dan ia bekerja dengan
cara yang sama:

``` {.c}
int a[] = {
#embed "/dev/random" __limit__(5)
};
```

Nah... apa itu `limit`?

### Parameter `limit()`

Kamu bisa menentukan batas jumlah elemen yang disematkan dengan parameter
ini.

Ini nilai maksimum, bukan nilai mutlak. Kalau file yang disematkan lebih
pendek dari batas yang ditentukan, hanya sebanyak itu byte yang diimpor.

Contoh `/dev/random` di atas adalah contoh motivasi untuk ini---di Unix,
itu _character device file_ yang mengembalikan aliran tak terbatas bilangan
yang cukup acak.

Menyematkan byte dalam jumlah tak terbatas memberatkan RAM, jadi parameter
`limit` memberi cara untuk berhenti setelah jumlah tertentu.

Terakhir, kamu boleh memakai makro `#define` di dalam `limit`, kalau kamu
penasaran.

### Parameter `if_empty`

\index{if\_empty() embed parameter@\texttt{if\_empty()} embed parameter|(}

Parameter ini menentukan apa hasil embed kalau file ada tapi tidak berisi
data. Misalkan file `foo.dat` berisi satu byte bernilai 123. Kalau kita
menulis ini:

``` {.c}
int x = 
#embed "foo.dat" if_empty(999)
;
```

kita mendapat:

``` {.c}
int x = 123;   // Ketika foo.dat berisi byte 123
```

Tapi bagaimana kalau file `foo.dat` berukuran nol byte (artinya tidak
berisi data dan kosong)? Kalau begitu, hasilnya diperluas menjadi:

``` {.c}
int x = 999;   // Ketika foo.dat kosong
```

Perlu dicatat, kalau `limit` diatur ke `0`, maka `if_empty` akan
selalu dipakai. Artinya, batas nol secara efektif berarti file itu kosong.

Ini akan selalu menghasilkan `x = 999` apa pun isi `foo.dat`:

``` {.c}
int x = 
#embed "foo.dat" limit(0) if_empty(999)
;
```

\index{if\_empty() embed parameter@\texttt{if\_empty()} embed parameter|)}

### Parameter `prefix()` dan `suffix()`

\index{prefix() embed parameter@\texttt{prefix()} embed parameter|(}
\index{suffix() embed parameter@\texttt{suffix()} embed parameter|(}

Ini cara untuk menambahkan sesuatu di depan embed.

Perhatikan bahwa ini hanya berpengaruh pada data yang tidak kosong! Kalau
file kosong, baik `prefix` maupun `suffix` tidak berpengaruh.

Berikut contoh ketika kita menyematkan tiga bilangan acak, tapi
mengawali angka-angka itu dengan `11,` dan mengakhirinya dengan `,99`:

``` {.c}
int x[] = {
#embed "/dev/urandom" limit(3) prefix(11,) suffix(,99)
};
```

Contoh hasil:

``` {.c}
int x[] = {11,135,116,220,99};
```

Tidak ada kewajiban memakai `prefix` dan `suffix` sekaligus. Kamu bisa
memakai keduanya, salah satu, atau tidak sama sekali.

Kita bisa memanfaatkan sifat bahwa keduanya hanya berlaku pada file yang
tidak kosong untuk efek yang rapi, seperti pada contoh berikut yang
dicuri tanpa malu dari spesifikasi.

Katakanlah kita punya file `foo.dat` yang berisi sejumlah data. Dan kita
ingin memakainya untuk menginisialisasi array, lalu kita ingin sufiks pada
array berupa elemen nol.

Tidak masalah, kan?

``` {.c}
int x[] = {
#embed "foo.dat" suffix(,0)
};
```

Kalau `foo.dat` berisi 11, 22, dan 33, kita mendapat:

``` {.c}
int x[] = {11,22,33,0};
```

Tapi tunggu! Bagaimana kalau `foo.dat` kosong? Kita mendapat:

``` {.c}
int x[] = {};
```

dan itu tidak bagus.

Tapi kita bisa memperbaikinya seperti ini:

``` {.c}
int x[] = {
#embed "foo.dat" suffix(,)
    0
};
```

Karena parameter `suffix` dihilangkan kalau file kosong, ini hanya akan
menjadi:

``` {.c}
int x[] = {0};
```

yang tidak masalah.

\index{prefix() embed parameter@\texttt{prefix()} embed parameter|)}
\index{suffix() embed parameter@\texttt{suffix()} embed parameter|)}

### Identifier `__has_embed()`

\index{\_\_has\_embed() identifier@\texttt{\_\_has\_embed()} identifier|(}

Ini cara yang bagus untuk menguji apakah file tertentu tersedia untuk
disematkan, dan juga apakah file itu kosong atau tidak.

Kamu memakainya dengan directive `#if`.

Berikut potongan kode yang mengambil 5 bilangan acak dari character device
generator bilangan acak. Kalau itu tidak ada, ia mencoba mengambilnya dari
file `myrandoms.dat`. Kalau itu juga tidak ada, ia memakai beberapa nilai
yang ditulis langsung:

``` {.c}
    int random_nums[] = {
#if __has_embed("/dev/urandom")
    #embed "/dev/urandom" limit(5)
#elif __has_embed("myrandoms.dat")
    #embed "myrandoms.dat" limit(5)
#else
    140,178,92,167,120
#endif
    };
```

Secara teknis, identifier `__has_embed()` bernilai salah satu dari tiga
nilai:

|Hasil `__has_embed()`|Deskripsi|
|-|-|
|`__STDC_EMBED_NOT_FOUND__`|Jika file tidak ditemukan.|
|`__STDC_EMBED_FOUND__`|Jika file ditemukan dan tidak kosong.|
|`__STDC_EMBED_EMPTY__`|Jika file ditemukan dan kosong.|

Aku punya alasan kuat untuk percaya bahwa `__STDC_EMBED_NOT_FOUND__`
adalah `0` dan yang lain bukan nol (karena itu tersirat dalam proposal dan
masuk akal secara logika), tapi aku kesulitan menemukannya di versi draf
spesifikasi ini.

\index{\_\_has\_embed() identifier@\texttt{\_\_has\_embed()} identifier|)}

TODO

### Parameter Lainnya

Implementasi compiler boleh mendefinisikan parameter embed lain sesukanya
---cari parameter non-standar ini di dokumentasi compiler-mu.

Misalnya:

``` {.c}
#embed "foo.bin" limit(12) frotz(lamp)
```

Parameter ini biasanya diberi awalan untuk membantu namespacing:

``` {.c}
#embed "foo.bin" limit(12) fmc::frotz(lamp)
```

Mungkin masuk akal mencoba mendeteksi apakah parameter ini tersedia
sebelum dipakai, dan untungnya kita bisa memakai `__has_embed` untuk itu.

Biasanya, `__has_embed()` hanya memberi tahu apakah file ada atau tidak.
Tapi---dan inilah bagian serunya---ia juga mengembalikan false kalau
parameter tambahan tidak didukung!

Jadi kalau kita memberinya file yang _kita tahu_ ada serta parameter yang
ingin kita uji keberadaannya, ia secara efektif memberi tahu apakah
parameter itu didukung.

Tapi file apa yang _selalu_ ada? Ternyata kita bisa memakai makro
`__FILE__`, yang diperluas menjadi nama file sumber yang merujuknya! File
itu _pasti_ ada, atau ada yang sangat salah di departemen ayam-dan-telur.

Mari kita uji parameter `frotz` itu untuk melihat apakah bisa dipakai:

``` {.c}
#if __has_embed(__FILE__ fmc::frotz(lamp))
    puts("fmc::frotz(lamp) is supported!");
#else
    puts("fmc::frotz(lamp) is NOT supported!");
#endif
```

### Menyematkan Nilai Multi-Byte

Bagaimana kalau yang dimasukkan `int`, bukan byte individual? Bagaimana
dengan nilai multi-byte di file yang disematkan?

Ini tidak didukung standar C23, tapi mungkin nanti ada ekstensi
implementasi yang mendefinisikannya.

\index{\#embed directive@\texttt{\#embed} directive|)}

## Directive `#pragma` {#pragma}

\index{\#pragma directive@\texttt{\#pragma} directive|(}

Ini directive yang aneh, kependekan dari "pragmatic". Kamu bisa memakainya
untuk... ya, apa pun yang didukung compiler-mu.

Pada dasarnya satu-satunya saat kamu menambahkan ini ke kodemu adalah kalau
ada dokumentasi yang menyuruhmu melakukannya.

### Pragma Non-Standar

\index{\#pragma directive@\texttt{\#pragma} directive!nonstandard pragmas|(}

Berikut satu contoh non-standar memakai `#pragma` untuk membuat compiler
menjalankan loop `for` secara paralel dengan beberapa thread (kalau
compiler mendukung ekstensi [OpenMP](https://www.openmp.org/)^[https://www.openmp.org/]):

``` {.c}
#pragma omp parallel for
for (int i = 0; i < 10; i++) { ... }
```

Ada berbagai macam directive `#pragma` yang terdokumentasi di seluruh
penjuru dunia.

Semua `#pragma` yang tidak dikenali diabaikan oleh compiler.

\index{\#pragma directive@\texttt{\#pragma} directive!nonstandard pragmas|)}

### Pragma Standar

Ada juga beberapa yang standar, dan ini diawali `STDC`, dengan bentuk
yang sama:

``` {.c}
#pragma STDC pragma_name on-off
```

Bagian `on-off` bisa berupa `ON`, `OFF`, atau `DEFAULT`.

Dan `pragma_name` bisa salah satu dari ini:

\index{FP\_CONTRACT pragma@\texttt{FP\_CONTRACT} pragma|(}
\index{CX\_LIMITED\_RANGE pragma@\texttt{CX\_LIMITED\_RANGE} pragma|(}

\index{FENV\_ACCESS pragma@\texttt{FENV\_ACCESS} pragma}

|Nama Pragma|Deskripsi|
|-------------|--------------------------------------------------|
|`FP_CONTRACT`|Mengizinkan ekspresi floating point diringkas menjadi satu operasi untuk menghindari galat pembulatan yang mungkin terjadi dari beberapa operasi.|
|`FENV_ACCESS`|Atur ke `ON` jika kamu berencana mengakses flag status floating point. Jika `OFF`, compiler mungkin melakukan optimasi yang membuat nilai di flag tidak konsisten atau tidak valid.|
|`CX_LIMITED_RANGE`|Atur ke `ON` untuk mengizinkan compiler melewatkan pengecekan overflow saat melakukan aritmetika kompleks. Bawaannya `OFF`.|

Misalnya:

``` {.c}
#pragma STDC FP_CONTRACT OFF
#pragma STDC CX_LIMITED_RANGE ON
```

\index{FP\_CONTRACT pragma@\texttt{FP\_CONTRACT} pragma|)}

Soal `CX_LIMITED_RANGE`, spesifikasi menunjukkan:

> The purpose of the pragma is to allow the implementation to use the
> formulas:
>
> $(x+iy)\times(u+iv) = (xu-yv)+i(yu+xv)$
>
> $(x+iy)/(u+iv) = [(xu+yv)+i(yu-xv)]/(u^2+v^2)$
>
> $|x+iy|=\sqrt{x^2+y^2}$
>
> where the programmer can determine they are safe.

\index{CX\_LIMITED\_RANGE pragma@\texttt{CX\_LIMITED\_RANGE} pragma|)}

### Operator `_Pragma`

\index{\_Pragma operator@\texttt{\_Pragma} operator|(}

Ini cara lain untuk mendeklarasikan pragma yang bisa dipakai di makro.

Keduanya setara:

``` {.c}
#pragma "Unnecessary" quotes
_Pragma("\"Unnecessary\" quotes")
```

\index{\_Pragma operator@\texttt{\_Pragma} operator!in a macro|(}

Ini bisa dipakai di makro, kalau perlu:

``` {.c}
#define PRAGMA(x) _Pragma(#x)
```

\index{\_Pragma operator@\texttt{\_Pragma} operator!in a macro|)}
\index{\_Pragma operator@\texttt{\_Pragma} operator|)}
\index{\#pragma directive@\texttt{\#pragma} directive|)}

## Directive `#line`

\index{\#line directive@\texttt{\#line} directive|(}
\index{\_\_LINE\_\_ macro@\texttt{\_\_LINE\_\_} macro|(}

Ini memungkinkan kamu menimpa nilai `__LINE__` dan `__FILE__`. Kalau mau.

Aku belum pernah ingin melakukannya, tapi di K&R2 mereka menulis:

> For the benefit of other preprocessors that generate C programs [...]

Jadi mungkin itulah alasannya.

Untuk menimpa nomor baris menjadi, misalnya, 300:

``` {.c}
#line 300
```

dan `__LINE__` akan terus menghitung naik dari sana.

\index{\_\_LINE\_\_ macro@\texttt{\_\_LINE\_\_} macro|)}

Untuk menimpa nomor baris dan nama file:

``` {.c}
#line 300 "newfilename"
```

\index{\#line directive@\texttt{\#line} directive|)}

## Null Directive

\index{\# null directive@\texttt{\#} null directive|(}

Tanda `#` sendirian di satu baris diabaikan oleh preprocessor. Nah, jujur
saja, aku tidak tahu apa kasus pemakaiannya.

Aku pernah melihat contoh seperti ini:

``` {.c}
#ifdef FOO
    #
#else
    printf("Something");
#endif
```

yang hanya kosmetik; baris dengan `#` sendirian itu bisa dihapus tanpa
efek buruk.

Atau mungkin demi konsistensi kosmetik, seperti ini:

``` {.c}
#
#ifdef FOO
    x = 2;
#endif
#
#if BAR == 17
    x = 12;
#endif
#
```

Tapi, soal kosmetik, itu jelek.

Satu postingan lain menyebut penghapusan komentar---bahwa di GCC, komentar
setelah `#` tidak akan dilihat compiler. Itu tidak kuragukan,
tapi spesifikasi tampaknya tidak menyebut ini sebagai perilaku standar.

Pencarianku akan alasannya tidak membuahkan banyak hasil. Jadi aku akan bilang
saja bahwa ini adalah esoterika C kuno yang sesungguhnya.

\index{\# null directive@\texttt{\#} null directive|)}
\index{Preprocessor|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `struct` II: Lebih Seru dengan `struct`

\index{struct keyword@\texttt{struct} keyword|(}

Ternyata ada jauh lebih banyak yang bisa dilakukan dengan `struct`
daripada yang sudah kita bahas, tapi isinya hanya tumpukan besar
hal-hal serba-serbi. Jadi kita kumpulkan saja di bab ini.

Kalau kamu sudah nyaman dengan dasar-dasar `struct`, kamu bisa
melengkapi pengetahuanmu di sini.

## Initializer untuk `struct` Bersarang dan Array

\index{struct keyword@\texttt{struct} keyword!initializers|(}

Ingat bagaimana kamu bisa [menginisialisasi anggota struktur dengan cara
seperti ini](#struct-initializers)?

``` {.c}
struct foo x = {.a=12, .b=3.14};
```

Ternyata initializer ini punya kemampuan lebih daripada yang awalnya
kita sampaikan. Seru!

Untuk satu hal, kalau ada substruktur bersarang seperti berikut, kamu
bisa menginisialisasi anggota substruktur itu dengan mengikuti nama
variabel berurutan ke bawah:

``` {.c}
struct foo x = {.a.b.c=12};
```

Kita lihat contohnya:

``` {.c .numberLines}
#include <stdio.h>

struct cabin_information {
    int window_count;
    int o2level;
};

struct spaceship {
    char *manufacturer;
    struct cabin_information ci;
};

int main(void)
{
    struct spaceship s = {
        .manufacturer="General Products",
        .ci.window_count = 8,   // <-- INITIALIZER BERSARANG!
        .ci.o2level = 21
    };

    printf("%s: %d seats, %d%% oxygen\n",
        s.manufacturer, s.ci.window_count, s.ci.o2level);
}
```

Lihat baris 16-17! Di situlah kita menginisialisasi anggota `struct
cabin_information` dalam definisi `s`, `struct spaceship` kita.

Dan ini opsi lain untuk initializer yang sama---kali ini kita buat yang
tampilannya lebih standar, tapi kedua cara berfungsi:

``` {.c .numberLines startFrom="15"}
    struct spaceship s = {
        .manufacturer="General Products",
        .ci={
            .window_count = 8,
            .o2level = 21
        }
    };
```

Nah, seolah-olah informasi di atas belum cukup spektakuler, kita juga bisa
mencampurkan array initializer di dalamnya.

Mari kita ubah contohnya supaya ada array informasi penumpang di dalamnya,
dan kita lihat bagaimana initializer bekerja di sana juga.

``` {.c .numberLines}
#include <stdio.h>

struct passenger {
    char *name;
    int covid_vaccinated; // Boolean
};

#define MAX_PASSENGERS 8

struct spaceship {
    char *manufacturer;
    struct passenger passenger[MAX_PASSENGERS];
};

int main(void)
{
    struct spaceship s = {
        .manufacturer="General Products",
        .passenger = {
            // Inisialisasi satu field per satu
            [0].name = "Gridley, Lewis",
            [0].covid_vaccinated = 0,

            // Atau sekaligus
            [7] = {.name="Brown, Teela", .covid_vaccinated=1},
        }
    };

    printf("Passengers for %s ship:\n", s.manufacturer);

    for (int i = 0; i < MAX_PASSENGERS; i++)
        if (s.passenger[i].name != NULL)
            printf("    %s (%svaccinated)\n",
                s.passenger[i].name,
                s.passenger[i].covid_vaccinated? "": "not ");
}
```

\index{struct keyword@\texttt{struct} keyword!initializers|)}

## `struct` Anonim

\index{struct keyword@\texttt{struct} keyword!anonymous|(}

Inilah "`struct` tanpa nama". Kita juga menyinggungnya di bagian
[`typedef`](#typedef-struct), tapi kita segarkan lagi di sini.

Ini `struct` biasa:

``` {.c}
struct animal {
    char *name;
    int leg_count, speed;
};
```

Dan ini padanannya yang anonim:

``` {.c}
struct {              // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
};
```

Okeeee. Jadi kita punya `struct`, tapi tanpa nama, sehingga tidak ada
cara memakainya nanti? Kelihatannya sia-sia.

Harus diakui, pada contoh itu memang begitu. Tapi kita masih bisa
memanfaatkannya dengan beberapa cara.

Satu cara yang jarang: karena `struct` anonim merepresentasikan sebuah
tipe, kita bisa menaruh nama variabel setelahnya dan memakainya.

``` {.c}
struct {                   // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
} a, b, c;                 // 3 variabel bertipe struct ini

a.name = "antelope";
c.leg_count = 4;           // misalnya
```

Tapi itu pun belum terlalu berguna.

Jauh lebih umum adalah memakai `struct` anonim bersama `typedef` supaya
bisa dipakai nanti (misalnya untuk mengoper variabel ke fungsi).

``` {.c}
typedef struct {                   // <-- Tanpa nama!
    char *name;
    int leg_count, speed;
} animal;                          // Tipe baru: animal

animal a, b, c;

a.name = "antelope";
c.leg_count = 4;           // misalnya
```

Secara pribadi, aku tidak banyak memakai `struct` anonim. Menurutku
lebih enak melihat seluruh `struct animal` sebelum nama variabel
pada deklarasi.

Tapi itu cuma, ya, pendapatku, kawan.

\index{struct keyword@\texttt{struct} keyword!anonymous|)}

## `struct` yang Merujuk Diri Sendiri

\index{struct keyword@\texttt{struct} keyword!self-referential|(}

Untuk struktur data apa pun yang mirip graf, berguna sekali punya pointer
ke node/vertex yang terhubung. Tapi ini berarti pada definisi sebuah
node, kamu perlu punya pointer ke node. Seperti ayam dan telur!

Tapi ternyata di C ini bisa dilakukan tanpa masalah sama sekali.

Misalnya, ini node linked list:

``` {.c}
struct node {
    int data;
    struct node *next;
};
```

Penting dicatat bahwa `next` adalah pointer. Inilah yang membuat semuanya
bisa dibangun. Walaupun compiler belum tahu seperti apa seluruh `struct
node`, semua pointer berukuran sama.

Berikut program linked list ala kadarnya untuk mengujinya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next;
};

int main(void)
{
    struct node *head;

    // Susun linked list secara asal-asalan (11)->(22)->(33)
    head = malloc(sizeof(struct node));
    head->data = 11;
    head->next = malloc(sizeof(struct node));
    head->next->data = 22;
    head->next->next = malloc(sizeof(struct node));
    head->next->next->data = 33;
    head->next->next->next = NULL;

    // Telusuri
    for (struct node *cur = head; cur != NULL; cur = cur->next) {
        printf("%d\n", cur->data);
    }
}
```

Menjalankannya mencetak:

``` {.default}
11
22
33
```

\index{struct keyword@\texttt{struct} keyword!self-referential|)}

## Flexible Array Member

\index{struct keyword@\texttt{struct} keyword!flexible array members|(}

Di zaman dahulu kala, ketika orang masih mengukir kode C dari kayu,
sebagian orang berpikir alangkah kerennya kalau bisa mengalokasikan
`struct` yang punya array dengan panjang bervariasi di ujungnya.

Aku ingin menegaskan bahwa paruh pertama bagian ini adalah cara lama,
dan setelah itu kita akan memakai cara baru.

Misalnya, mungkin kamu bisa mendefinisikan `struct` untuk menyimpan string
dan panjang string itu. Isinya sebuah panjang dan sebuah array untuk
menampung data. Mungkin seperti ini:

``` {.c}
struct len_string {
    int length;
    char data[8];
};
```

Tapi itu menetapkan `8` sebagai panjang maksimum string, dan itu tidak
banyak. Bagaimana kalau kita melakukan sesuatu yang _cerdik_ dan
`malloc()` sedikit ruang ekstra di ujung setelah struct, lalu membiarkan
data meluap ke ruang itu?

Mari kita lakukan, lalu alokasikan 40 byte lagi di atasnya:

``` {.c}
struct len_string *s = malloc(sizeof *s + 40);
```

Karena `data` adalah field terakhir dari `struct`, kalau kita meluapi field
itu, ia mengalir ke ruang yang sudah kita alokasikan! Karena alasan ini,
trik ini hanya bekerja kalau array pendek itu adalah field _terakhir_
dalam `struct`.

``` {.c}
// Salin lebih dari 8 byte!

strcpy(s->data, "Hello, world!");  // Tidak akan crash. Mungkin.
```

\index{Arrays!zero length|(}

Bahkan, dulu ada solusi umum dari compiler untuk ini, yaitu
mengalokasikan array berukuran nol di ujung:

``` {.c}
struct len_string {
    int length;
    char data[0];
};
```

Lalu setiap byte ekstra yang kamu alokasikan siap dipakai untuk string
itu.

Karena `data` adalah field terakhir dari `struct`, kalau kita meluapi field
itu, ia mengalir ke ruang yang sudah kita alokasikan!

``` {.c}
// Salin lebih dari 8 byte!

strcpy(s->data, "Hello, world!");  // Tidak akan crash. Mungkin.
```

Tapi, tentu saja, mengakses data di luar ujung array itu adalah undefined
behavior! Di zaman modern ini, kita tidak lagi sudi memakai kebiadaban
seperti itu.

\index{Arrays!zero length|)}

Untungnya, kita masih bisa mendapat efek yang sama dengan C99 dan
sesudahnya, tapi sekarang legal.

Kita cukup mengubah definisi di atas agar array tidak punya ukuran^[Secara
teknis kita bilang ia punya _incomplete type_.]:

``` {.c}
struct len_string {
    int length;
    char data[];
};
```

Sekali lagi, ini hanya bekerja kalau flexible array member adalah field
_terakhir_ dalam `struct`.

Lalu kita bisa mengalokasikan ruang sebanyak yang kita mau untuk
string-string itu dengan `malloc()` lebih besar dari `struct len_string`,
seperti pada contoh ini yang membuat `struct len_string` baru dari string
C:


``` {.c}
struct len_string *len_string_from_c_string(char *s)
{
    int len = strlen(s);

    // Alokasikan "len" byte lebih banyak dari yang biasanya dibutuhkan
    struct len_string *ls = malloc(sizeof *ls + len);

    ls->length = len;

    // Salin string ke byte-byte ekstra itu
    memcpy(ls->data, s, len);

    return ls;
}
```

\index{struct keyword@\texttt{struct} keyword!flexible array members|)}

## Padding Byte {#struct-padding-bytes}

\index{struct keyword@\texttt{struct} keyword!padding bytes|(}

Waspadai bahwa C boleh menambahkan padding byte di dalam atau setelah
`struct` sesuai kehendaknya. Kamu tidak bisa memastikan bahwa field-field
itu berdekatan langsung di memori^[Walaupun beberapa compiler punya opsi
untuk memaksa hal itu terjadi---cari `__attribute__((packed))` untuk
melihat caranya di GCC.].

Mari lihat program ini. Kita mengeluarkan dua angka. Satu adalah jumlah
`sizeof` tipe-tipe field individual. Satunya lagi `sizeof` seluruh
`struct`.

Orang akan mengira keduanya sama. Ukuran total adalah ukuran jumlah
bagian-bagiannya, kan?

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    int a;
    char b;
    int c;
    char d;
};

int main(void)
{
    printf("%zu\n", sizeof(int) + sizeof(char) + sizeof(int) + sizeof(char));
    printf("%zu\n", sizeof(struct foo));
}
```

Tapi di sistemku, ini mengeluarkan:

``` {.default}
10
16
```

Ternyata tidak sama! Compiler menambahkan 6 byte padding agar performanya
lebih baik. Mungkin di compiler-mu hasilnya berbeda, tapi kecuali kamu
memaksanya, kamu tidak bisa memastikan tidak ada padding.

\index{struct keyword@\texttt{struct} keyword!padding bytes|)}

## `offsetof`

\index{offsetof() macro@\texttt{offsetof()} macro|(}

Di bagian sebelumnya, kita melihat bahwa compiler bisa menyisipkan padding
byte sesukanya di dalam struktur.

Bagaimana kalau kita perlu tahu di mana letaknya? Kita bisa mengukurnya
dengan `offsetof`, yang didefinisikan di `<stddef.h>`.

Mari kita ubah kode di atas untuk mencetak offset field-field individual
dalam `struct`:

``` {.c .numberLines}
#include <stdio.h>
#include <stddef.h>

struct foo {
    int a;
    char b;
    int c;
    char d;
};

int main(void)
{
    printf("%zu\n", offsetof(struct foo, a));
    printf("%zu\n", offsetof(struct foo, b));
    printf("%zu\n", offsetof(struct foo, c));
    printf("%zu\n", offsetof(struct foo, d));
}
```

Di komputerku, ini mengeluarkan:

``` {.default}
0
4
8
12
```

yang menunjukkan bahwa kita memakai 4 byte untuk setiap field. Agak
aneh, karena `char` hanya 1 byte, kan? Compiler menaruh 3 padding byte
setelah setiap `char` agar semua field berukuran 4 byte. Mungkin ini akan
berjalan lebih cepat di CPU-ku.

\index{offsetof() macro@\texttt{offsetof()} macro|)}

<!--

6.7.2.1

15 Within a structure object, the non-bit-field members and the units in which bit-fields reside have addresses that increase in the order in which they are declared. A pointer to a structure object, suitably converted, points to its initial member (or if that member is a bit-field, then to the unit in which it resides), and vice versa. There may be unnamed padding within a structure object, but not at its beginning.

6.2.7 Compatible type and composite type

1 Two types have compatible type if their types are the same.

6.5

7 An object shall have its stored value accessed only by an lvalue expression that has one of the following types:

- a type compatible with the effective type of the object

-->

## Fake OOP {#fake-oop}

Ada hal yang agak kurang ajar dan mirip OOP yang bisa dilakukan dengan
`struct`.

Karena pointer ke `struct` sama dengan pointer ke elemen pertama
`struct`, kamu bebas meng-cast pointer ke `struct` menjadi pointer ke
elemen pertama.

Artinya kita bisa menyusun situasi seperti ini:

``` {.c}
struct parent {
    int a, b;
};

struct child {
    struct parent super;  // HARUS yang pertama
    int c, d;
};
```

Lalu kita bisa mengoper pointer ke `struct child` ke fungsi yang
mengharapkan itu _atau_ pointer ke `struct parent`!

Karena `struct parent super` adalah item pertama dalam `struct child`,
pointer ke `struct child` mana pun sama dengan pointer ke field
`super` itu^[Omong-omong, `super` bukan kata kunci. Aku cuma
meminjam sedikit terminologi OOP.].

Mari kita buat contohnya. Kita buat `struct` seperti di atas, lalu kita
oper pointer ke `struct child` ke fungsi yang butuh pointer ke `struct
parent`... dan itu tetap berfungsi.

``` {.c .numberLines}
#include <stdio.h>

struct parent {
    int a, b;
};

struct child {
    struct parent super;  // HARUS yang pertama
    int c, d;
};

// Menjadikan argumen `void*` supaya kita bisa mengoper tipe apa pun
// (yaitu struct parent atau struct child)
void print_parent(void *p)
{
    // Mengharapkan struct parent--tapi struct child juga berfungsi
    // karena pointer-nya menunjuk ke struct parent di field
    // pertama:
    struct parent *self = p;

    printf("Parent: %d, %d\n", self->a, self->b);
}

void print_child(struct child *self)
{
    printf("Child: %d, %d\n", self->c, self->d);
}

int main(void)
{
    struct child c = {.super.a=1, .super.b=2, .c=3, .d=4};

    print_child(&c);
    print_parent(&c);  // Juga berfungsi meskipun ini struct child!
}
```

Lihat apa yang kita lakukan di baris terakhir `main()`? Kita memanggil
`print_parent()` tapi mengoper `struct child*` sebagai argumen! Walaupun
`print_parent()` butuh argumen yang menunjuk ke `struct parent`, kita
_lolos_ karena field pertama dalam `struct child` adalah `struct parent`.

Sekali lagi, ini berfungsi karena pointer ke `struct` bernilai sama dengan
pointer ke field pertama dalam `struct` itu.

Semua ini bergantung pada bagian spesifikasi ini:

> **§6.7.2.1¶15** [...] A pointer to a structure object, suitably
> converted, points to its initial member [...], and vice versa.

dan

> **§6.5¶7** An object shall have its stored value accessed only by an
> lvalue expression that has one of the following types:
>
> * a type compatible with the effective type of the object
> * [...]

serta asumsiku bahwa "suitably converted" berarti "di-cast ke
effective type dari anggota pertama".

## Bit-Field

\index{struct keyword@\texttt{struct} keyword!bit fields|(}

Menurut pengalamanku, ini jarang dipakai, tapi kamu mungkin
melihatnya sesekali, terutama pada aplikasi level rendah yang
mengemas bit-bit bersama ke dalam ruang yang lebih besar.

Mari kita lihat kode untuk mendemonstrasikan satu kasus pemakaian:

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    unsigned int a;
    unsigned int b;
    unsigned int c;
    unsigned int d;
};

int main(void)
{
    printf("%zu\n", sizeof(struct foo));
}
```

Di sistemku, ini mencetak `16`. Itu masuk akal, karena `unsigned`
berukuran 4 byte di sistemku.

Tapi bagaimana kalau kita tahu bahwa semua nilai yang akan disimpan di
`a` dan `b` bisa disimpan dalam 5 bit, dan nilai di `c` dan `d` bisa
disimpan dalam 3 bit? Totalnya hanya 16 bit. Kenapa mencadangkan 128 bit
untuk mereka kalau kita hanya akan memakai 16?

Nah, kita bisa meminta C dengan sangat sopan untuk mencoba mengemas
nilai-nilai ini. Kita bisa menentukan jumlah bit maksimum yang bisa
dipakai nilai (dari 1 sampai ukuran tipe penampungnya).

Caranya dengan menaruh titik dua setelah nama field, diikuti lebar field
dalam bit.

``` {.c .numberLines startFrom="3"}
struct foo {
    unsigned int a:5;
    unsigned int b:5;
    unsigned int c:3;
    unsigned int d:3;
};
```

Sekarang kalau aku bertanya ke C seberapa besar `struct foo`-ku, jawabannya 4!
Tadinya 16 byte, sekarang hanya 4. Keempat nilai itu "dikemas" menjadi 4
byte, penghematan memori empat kali lipat.

Konsekuensinya, tentu saja, field 5-bit hanya bisa menampung nilai 0-31
dan field 3-bit hanya bisa menampung nilai 0-7. Tapi toh hidup memang soal
kompromi.

### Bit-Field yang Tidak Berdekatan

Satu jebakan: C hanya menggabungkan bit-field yang **berdekatan**. Kalau
mereka disela oleh non-bit-field, tidak ada penghematan:

``` {.c}
struct foo {            // sizeof(struct foo) == 16 (di sistemku)
    unsigned int a:1;   // karena a tidak berdekatan dengan c.
    unsigned int b;
    unsigned int c:1;
    unsigned int d;
};
```

Pada contoh itu, karena `a` tidak berdekatan dengan `c`, keduanya
"dikemas" dalam `int`-nya masing-masing.

Jadi kita punya satu `int` masing-masing untuk `a`, `b`, `c`, dan `d`.
Karena `int` di sistemku 4 byte, totalnya 16 byte.

Penataan ulang singkat menghemat ruang dari 16 byte menjadi 12 byte (di
sistemku):

``` {.c}
struct foo {            // sizeof(struct foo) == 12 (di sistemku)
    unsigned int a:1;
    unsigned int c:1;
    unsigned int b;
    unsigned int d;
};
```

Dan sekarang, karena `a` bersebelahan dengan `c`, compiler menaruh
keduanya dalam satu `int`.

Jadi kita punya satu `int` untuk gabungan `a` dan `c`, dan satu `int`
masing-masing untuk `b` dan `d`. Total 3 `int`, atau 12 byte.

Taruh semua bitfield-mu berdekatan supaya compiler menggabungkannya.

### `int` Signed atau Unsigned

Kalau kamu hanya mendeklarasikan bit-field sebagai `int`, compiler yang
berbeda akan memperlakukannya sebagai `signed` atau `unsigned`. Sama
seperti situasi pada `char`.

Jelaskan tanda (signedness)-nya secara spesifik saat memakai bit-field.

### Bit-Field Tanpa Nama

Pada keadaan tertentu, kamu mungkin perlu mencadangkan beberapa bit untuk
alasan hardware, tapi tidak perlu memakainya di kode.

Misalnya, katakanlah ada satu byte yang 2 bit teratasnya punya makna, 1
bit terbawahnya punya makna, tapi 5 bit di tengah tidak kamu
pakai^[Dengan asumsi `char` 8-bit, yaitu `CHAR_BIT == 8`.].

Kita _bisa_ melakukan sesuatu seperti ini:

``` {.c}
struct foo {
    unsigned char a:2;
    unsigned char dummy:5;
    unsigned char b:1;
};
```

Dan itu berfungsi---di kode kita memakai `a` dan `b`, tapi tidak pernah
`dummy`. Ia hanya ada untuk menghabiskan 5 bit supaya `a` dan `b` berada
di posisi "yang dibutuhkan" (oleh contoh yang dibuat-buat ini) di dalam
byte.

C menyediakan cara untuk merapikannya: _bit-field tanpa nama_. Kamu cukup
menghilangkan nama (`dummy`) dalam kasus ini, dan C tetap senang dengan
efek yang sama:

``` {.c}
struct foo {
    unsigned char a:2;
    unsigned char :5;   // <-- bit-field tanpa nama!
    unsigned char b:1;
};
```

### Bit-Field Tanpa Nama dengan Lebar Nol

Esoterika lagi di sini... Katakanlah kamu mengemas bit ke dalam
`unsigned int`, dan kamu butuh beberapa bit-field berdekatan untuk
dikemas ke `unsigned int` _berikutnya_.

Artinya, kalau kamu menulis ini:

``` {.c}
struct foo {
    unsigned int a:1;
    unsigned int b:2;
    unsigned int c:3;
    unsigned int d:4;
};
```

compiler mengemas semuanya ke dalam satu `unsigned int`. Tapi bagaimana
kalau kamu butuh `a` dan `b` dalam satu `int`, dan `c` dan `d` dalam `int`
yang lain?

Ada solusinya: taruh bit-field tanpa nama berlebar `0` di tempat kamu ingin
compiler mulai mengemas bit dari awal pada `int` yang berbeda:

``` {.c}
struct foo {
    unsigned int a:1;
    unsigned int b:2;
    unsigned int :0;   // <--Bit-field tanpa nama berlebar nol
    unsigned int c:3;
    unsigned int d:4;
};
```

Ini seperti page break eksplisit di word processor. Kamu memberi tahu
compiler, "Berhenti mengemas bit di `unsigned` ini, dan mulai mengemas di
yang berikutnya."

Dengan menambahkan bit-field tanpa nama berlebar nol di tempat itu,
compiler menaruh `a` dan `b` dalam satu `unsigned int`, dan `c` dan `d`
dalam `unsigned int` lain. Total dua, dengan ukuran 8 byte di sistemku (`unsigned int` masing-masing 4 byte).

\index{struct keyword@\texttt{struct} keyword!bit fields|)}

## Union

\index{union keyword@\texttt{union} keyword|(}

Pada dasarnya ini sama seperti `struct`, hanya saja field-nya saling
tumpang tindih di memori. `union` hanya akan sebesar field terbesarnya,
dan kamu hanya bisa memakai satu field dalam satu waktu.

Ini cara untuk memakai ulang ruang memori yang sama untuk berbagai jenis
data.

Deklarasinya sama seperti `struct`, hanya saja memakai `union`. Lihat ini:

``` {.c}
union foo {
    int a, b, c, d, e, f;
    float g, h;
    char i, j, k, l;
};
```

Nah, itu banyak sekali field. Kalau ini `struct`, sistemku akan
bilang dibutuhkan 36 byte untuk menampung semuanya.

Tapi ini `union`, jadi semua field itu saling tumpang tindih dalam
bentangan memori yang sama. Field terbesarnya `int` (atau `float`),
yang memakai 4 byte di sistemku. Dan memang, kalau kutanya `sizeof`
dari `union foo`, jawabannya 4!

Konsekuensinya, hanya satu field yang bisa dipakai secara portabel dalam
satu waktu. Namun...

### Union dan Type Punning {#union-type-punning}

\index{union keyword@\texttt{union} keyword!type punning|(}

Kamu bisa menulis ke satu field `union` dan membaca dari field lain
secara non-portabel!

Melakukannya disebut [type punning](https://en.wikipedia.org/wiki/Type_punning)^[https://en.wikipedia.org/wiki/Type_punning], dan kamu
memakainya kalau benar-benar tahu apa yang kamu lakukan, biasanya pada
pemrograman level rendah.

Karena anggota union berbagi memori yang sama, menulis ke satu anggota
pasti memengaruhi yang lain. Dan kalau kamu membaca dari satu anggota
sesuatu yang ditulis ke anggota lain, kamu mendapat efek yang aneh.

``` {.c .numberLines}
#include <stdio.h>

union foo {
    float b;
    short a;
};

int main(void)
{
    union foo x;

    x.b = 3.14159;

    printf("%f\n", x.b);  // 3.14159, wajar

    printf("%d\n", x.a);  // Tapi bagaimana dengan ini?
}
```

Di sistemku, ini mencetak:

```
3.141590
4048
```

karena di balik layar, representasi object dari float `3.14159` sama
dengan representasi object dari short `4048`. Di sistemku. Hasilmu
mungkin berbeda.

\index{union keyword@\texttt{union} keyword!type punning|)}

### Pointer ke `union`

\index{union keyword@\texttt{union} keyword!pointers to|(}

Kalau kamu punya pointer ke `union`, kamu bisa meng-cast pointer itu ke
tipe field mana pun dalam `union` itu dan mengambil nilainya dengan cara
itu.

Pada contoh ini, kita lihat `union` berisi `int` dan `float`. Kita ambil
pointer ke `union`, tapi kita cast ke tipe `int*` dan `float*` (cast
membungkam peringatan compiler). Lalu kalau kita men-dereference
pointer-pointer itu, kita lihat bahwa nilainya sama dengan yang kita
simpan langsung di `union`.

``` {.c .numberLines}
#include <stdio.h>

union foo {
    int a, b, c, d, e, f;
    float g, h;
    char i, j, k, l;
};

int main(void)
{
    union foo x;

    int *foo_int_p = (int *)&x;
    float *foo_float_p = (float *)&x;

    x.a = 12;
    printf("%d\n", x.a);           // 12
    printf("%d\n", *foo_int_p);    // 12, lagi

    x.g = 3.141592;
    printf("%f\n", x.g);           // 3.141592
    printf("%f\n", *foo_float_p);  // 3.141592, lagi
}
```

Kebalikannya juga benar. Kalau kita punya pointer ke tipe di dalam
`union`, kita bisa meng-cast-nya ke pointer ke `union` dan mengakses
anggotanya.

``` {.c}
union foo x;
int *foo_int_p = (int *)&x;             // Pointer ke field int
union foo *p = (union foo *)foo_int_p;  // Kembali ke pointer ke union

p->a = 12;  // Baris ini sama dengan...
x.a = 12;   // yang ini.
```

Semua ini hanya memberi tahu bahwa, di balik layar, semua nilai dalam
`union` dimulai di tempat yang sama di memori, dan itu sama dengan lokasi
seluruh `union`.

\index{union keyword@\texttt{union} keyword!pointers to|)}

### Common Initial Sequence pada Union

\index{union keyword@\texttt{union} keyword!common initial sequences|(}

Kalau kamu punya `union` berisi `struct`, dan semua `struct` itu diawali
dengan _common initial sequence_, boleh mengakses anggota urutan itu
dari anggota `union` mana pun.

Apa?

Berikut dua `struct` dengan common initial sequence:

``` {.c}
struct a {
    int x;     //
    float y;   // Common initial sequence

    char *p;
};

struct b {
    int x;     //
    float y;   // Common initial sequence

    double *p;
    short z;
};
```

Kamu lihat? Keduanya diawali `int` diikuti `float`---itulah common initial
sequence. Anggota-anggota dalam urutan pada `struct` harus bertipe
kompatibel. Dan kita lihat itu pada `x` dan `y`, yaitu `int` dan `float`
masing-masing.

Sekarang kita buat union dari keduanya:

``` {.c}
union foo {
    struct a sa;
    struct b sb;
};
```

Aturan ini menjamin bahwa anggota common initial sequence bisa saling
menggantikan di kode. Artinya:

* `f.sa.x` sama dengan `f.sb.x`.

dan

* `f.sa.y` sama dengan `f.sb.y`.

Karena field `x` dan `y` sama-sama ada di common initial sequence.

Selain itu, nama anggota dalam common initial sequence tidak penting---yang
penting hanyalah tipenya sama.

Secara keseluruhan, ini memberi kita cara untuk menambahkan informasi
bersama di antara `struct` dalam `union` secara aman. Contoh terbaiknya
mungkin memakai sebuah field untuk menentukan `struct` mana, dari semua
`struct` dalam `union`, yang sedang "dipakai".

Artinya, kalau ini tidak diizinkan dan kita mengoper `union` ke suatu
fungsi, bagaimana fungsi itu tahu anggota `union` mana yang harus
dilihatnya?

Lihat `struct`-`struct` ini. Perhatikan common initial sequence-nya:

``` {.c .numberLines}
#include <stdio.h>

struct common {
    int type;   // common initial sequence
};

struct antelope {
    int type;   // common initial sequence

    int loudness;
};

struct octopus {
    int type;   // common initial sequence

    int sea_creature;
    float intelligence;
};

```

Sekarang kita masukkan ke dalam `union`:

``` {.c .numberLines startFrom="20"}
union animal {
    struct common common;
    struct antelope antelope;
    struct octopus octopus;
};

```

Mohon dimaklumi dua `#define` untuk demo ini:

``` {.c .numberLines startFrom="26"}
#define ANTELOPE 1
#define OCTOPUS  2

```

Sejauh ini, belum ada yang istimewa. Tampaknya field `type` sama sekali
tidak berguna.

Tapi sekarang mari kita buat fungsi generik yang mencetak `union animal`.
Ia harus bisa membedakan apakah yang dilihatnya `struct antelope` atau
`struct octopus`.

Berkat keajaiban common initial sequence, ia bisa mencari tipe hewan di
salah satu dari tempat-tempat ini untuk `union animal x` tertentu:

``` {.c}
int type = x.common.type;    // atau...
int type = x.antelope.type;  // atau...
int type = x.octopus.type;
```

Semuanya merujuk ke nilai yang sama di memori.

Dan, seperti yang mungkin sudah kamu duga, `struct common` ada di sana
supaya kode bisa melihat tipe secara agnostik tanpa menyebut hewan
tertentu.

Mari kita lihat kode untuk mencetak `union animal`:

``` {.c .numberLines startFrom="29"}
void print_animal(union animal *x)
{
    switch (x->common.type) {
        case ANTELOPE:
            printf("Antelope: loudness=%d\n", x->antelope.loudness);
            break;

        case OCTOPUS:
            printf("Octopus : sea_creature=%d\n", x->octopus.sea_creature);
            printf("          intelligence=%f\n", x->octopus.intelligence);
            break;
        
        default:
            printf("Unknown animal type\n");
    }

}

int main(void)
{
    union animal a = {.antelope.type=ANTELOPE, .antelope.loudness=12};
    union animal b = {.octopus.type=OCTOPUS, .octopus.sea_creature=1,
                                       .octopus.intelligence=12.8};

    print_animal(&a);
    print_animal(&b);
}
```

Lihat bagaimana di baris 29 kita hanya mengoper `union`-nya---kita tidak
tahu `struct` hewan apa yang sedang dipakai di dalamnya.

Tapi tidak apa-apa! Karena di baris 31 kita memeriksa tipenya untuk melihat
apakah itu antelope atau octopus. Lalu kita bisa melihat `struct` yang
tepat untuk mengambil anggotanya.

Efek yang sama pasti bisa didapat hanya dengan `struct`, tapi kamu bisa
memakai cara ini kalau menginginkan efek hemat memori dari `union`.

\index{union keyword@\texttt{union} keyword!common initial sequences|)}

## Union dan Struct Tanpa Nama

\index{union keyword@\texttt{union} keyword!and unnamed structs@and unnamed \texttt{struct}s|(}

Kamu tahu bahwa kamu bisa punya struct tanpa nama, seperti ini:

``` {.c}
struct {
    int x, y;
} s;
```

Itu mendefinisikan variabel `s` bertipe `struct` anonim (karena `struct`
itu tidak punya name tag), dengan anggota `x` dan `y`.

Jadi hal seperti ini valid:

``` {.c}
s.x = 34;
s.y = 90;

printf("%d %d\n", s.x, s.y);
```

Ternyata `struct` tanpa nama itu bisa ditaruh dalam `union` seperti yang
mungkin kamu duga:

``` {.c}
union foo {
    struct {       // tanpa nama!
        int x, y;
    } a;

    struct {       // tanpa nama!
        int z, w;
    } b;
};
```

Lalu diakses seperti biasa:

``` {.c}
union foo f;

f.a.x = 1;
f.a.y = 2;
f.b.z = 3;
f.b.w = 4;
```

Tidak ada masalah!

\index{union keyword@\texttt{union} keyword!and unnamed structs@and unnamed \texttt{struct}s|)}

## Mengoper dan Mengembalikan `struct` dan `union`

\index{union keyword@\texttt{union} keyword!passing and returning|(}
\index{struct keyword@\texttt{struct} keyword!passing and returning|(}

Kamu bisa mengoper `struct` atau `union` ke fungsi secara by value
(bukan pointer ke objeknya)---salinan objek itu ke parameter dibuat
seperti pada penugasan biasa.

Kamu juga bisa mengembalikan `struct` atau `union` dari fungsi dan ia juga
dioper balik secara by value.

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    int x, y;
};

struct foo f(void)
{
    return (struct foo){.x=34, .y=90};
}

int main(void)
{
    struct foo a = f();  // Salinan dibuat

    printf("%d %d\n", a.x, a.y);
}
```

Fakta menarik: kalau kamu melakukan ini, kamu bisa memakai operator `.`
langsung pada pemanggilan fungsi:

``` {.c .numberLines startFrom="16"}
    printf("%d %d\n", f().x, f().y);
```

(Tentu saja contoh itu memanggil fungsi dua kali, secara tidak efisien.)

Dan hal yang sama berlaku untuk mengembalikan pointer ke `struct` dan
`union`---pastikan saja memakai arrow operator `->` dalam kasus itu.

\index{union keyword@\texttt{union} keyword!passing and returning|)}
\index{struct keyword@\texttt{struct} keyword!passing and returning|)}
\index{union keyword@\texttt{union} keyword|)}
\index{struct keyword@\texttt{struct} keyword|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Karakter dan String II

Kita sudah membahas bahwa tipe `char` sebenarnya hanyalah tipe integer
kecil... dan hal yang sama berlaku untuk karakter dalam tanda kutip
tunggal.

Tapi string dalam tanda kutip ganda bertipe `const char *`.

Ternyata ada beberapa jenis string dan karakter lagi, dan ini membawa kita
ke salah satu lubang kelinci paling terkenal di bahasa ini: urusan
multibyte/wide/Unicode/lokalisasi itu.

Kita akan mengintip ke lubang kelinci itu, tapi belum masuk. ...Belum!

## Escape Sequence

\index{Escape sequences|(}

Kita terbiasa dengan string dan karakter berisi huruf biasa, tanda baca,
dan angka:

``` {.c}
char *s = "Hello!";
char t = 'c';
```

Tapi bagaimana kalau kita ingin memasukkan karakter khusus yang tidak bisa
diketik di keyboard karena memang tidak ada (misalnya "€"), atau bahkan
kalau kita ingin karakter berupa tanda kutip tunggal? Jelas kita tidak
bisa menulis ini:

``` {.c}
char t = ''';
```

\index{{\backslash} backslash escape@\texttt{{\backslash}} backslash escape|(}

Untuk melakukan hal-hal ini, kita memakai yang disebut _escape sequence_.
Bentuknya karakter backslash (`\`) diikuti karakter lain. Kedua (atau
lebih) karakter itu bersama-sama punya makna khusus.

Untuk contoh karakter kutip tunggal kita, kita bisa menaruh escape (yaitu
`\`) di depan tanda kutip tunggal di tengah untuk mengatasinya:

\index{{\backslash}' single quote@\texttt{{\backslash}'} single quote|(}

``` {.c}
char t = '\'';
```

Sekarang C tahu bahwa `\'` hanya berarti tanda kutip biasa yang ingin
kita cetak, bukan akhir dari deretan karakter.

\index{{\backslash}' single quote@\texttt{{\backslash}'} single quote|)}

Dalam konteks ini kamu bisa mengatakan "backslash" atau "escape" ("escape
tanda kutip itu") dan developer C akan paham maksudmu. Selain itu,
"escape" dalam konteks ini berbeda dari tombol `Esc` di keyboardmu atau
kode ASCII `ESC`.

### Escape yang Sering Dipakai

Menurut pendapatku yang rendah hati, karakter escape ini mencakup
99,2%^[Angka itu kukarang sendiri, tapi kemungkinan tidak jauh meleset] dari
semua escape.

\index{{\backslash}n newline@\texttt{{\backslash}n} newline}
\index{{\backslash}' single quote@\texttt{{\backslash}'} single quote}
\index{{\backslash}" double quote@\texttt{{\backslash}"} double quote}
\index{{\backslash}{\backslash} backslash@\texttt{{\backslash}{\backslash}} backslash}

|Kode|Deskripsi|
|--|------------|
|`\n`|Karakter newline---saat mencetak, lanjutkan output berikutnya di baris selanjutnya|
|`\'`|Tanda kutip tunggal---dipakai untuk konstanta karakter kutip tunggal|
|`\"`|Tanda kutip ganda---dipakai untuk tanda kutip ganda dalam string literal|
|`\\`|Backslash---dipakai untuk `\` literal dalam string atau karakter|

Berikut beberapa contoh escape dan apa yang dikeluarkannya saat dicetak.

``` {.c}
printf("Use \\n for newline\n");  // Use \n for newline
printf("Say \"hello\"!\n");       // Say "hello"!
printf("%c\n", '\'');             // '
```

### Escape yang Jarang Dipakai

Tapi masih ada escape lain! Hanya saja jarang terlihat.

\index{{\backslash}a alert@\texttt{{\backslash}a} alert}
\index{{\backslash}b backspace@\texttt{{\backslash}b} backspace}
\index{{\backslash}f formfeed@\texttt{{\backslash}f} formfeed}
\index{{\backslash}r carriage return@\texttt{{\backslash}r} carriage return}
\index{{\backslash}t tab@\texttt{{\backslash}t} tab}
\index{{\backslash}v vertical tab@\texttt{{\backslash}v} vertical tab}
\index{{\backslash}? question mark@\texttt{{\backslash}?} question mark}

|Kode|Deskripsi|
|--|--------------------|
|`\a`|Alert. Membuat terminal mengeluarkan bunyi atau berkedip, atau keduanya!|
|`\b`|Backspace. Memundurkan kursor satu karakter. Tidak menghapus karakternya.|
|`\f`|Formfeed. Berpindah ke "halaman" berikutnya, tapi itu tidak punya banyak makna di zaman modern. Di sistemku, ini berperilaku seperti `\v`.|
|`\r`|Return. Pindah ke awal baris yang sama.|
|`\t`|Tab horizontal. Pindah ke tab stop horizontal berikutnya. Di mesinku, ini sejajar dengan kolom kelipatan 8, tapi hasilnya bisa berbeda di tempatmu.|
|`\v`|Tab vertikal. Pindah ke tab stop vertikal berikutnya. Di mesinku, ini pindah ke kolom yang sama di baris berikutnya.|
|`\?`|Tanda tanya literal. Kadang kamu butuh ini untuk menghindari trigraph, seperti ditunjukkan di bawah.|

#### Pembaruan Status Satu Baris

\index{{\backslash}b backspace@\texttt{{\backslash}b} backspace|(}
\index{{\backslash}r carriage return@\texttt{{\backslash}r} carriage return|(}

Kasus pemakaian `\b` atau `\r` adalah menampilkan pembaruan status yang
muncul di baris yang sama di layar dan tidak membuat tampilan bergulir.
Berikut contoh yang melakukan hitung mundur dari 10. (Kalau compiler-mu
tidak mendukung threading, kamu bisa memakai fungsi POSIX non-standar
`sleep()` dari `<unistd.h>`---kalau bukan di sistem mirip Unix, cari
platformmu dan `sleep` untuk padanannya.)

``` {.c .numberLines}
#include <stdio.h>
#include <threads.h>

int main(void)
{
    for (int i = 10; i >= 0; i--) {
        printf("\rT minus %d second%s... \b", i, i != 1? "s": "");

        fflush(stdout);  // Paksa output untuk diperbarui

        // Tidur selama 1 detik
        thrd_sleep(&(struct timespec){.tv_sec=1}, NULL);
    }

    printf("\rLiftoff!             \n");
}
```

Cukup banyak hal terjadi di baris 7. Pertama, kita mengawali dengan `\r`
untuk membawa kita ke awal baris saat ini, lalu menimpa apa pun yang ada di
sana dengan hitung mundur saat ini. (Ada operator ternary di situ untuk
memastikan kita mencetak `1 second` dan bukan `1 seconds`.)

Perhatikan juga ada spasi setelah `...` Itu supaya kita menimpa `.`
terakhir dengan benar ketika `i` turun dari `10` ke `9` dan kita jadi
satu kolom lebih sempit. Coba tanpa spasi itu untuk melihat maksudnya.

Dan kita tutup dengan `\b` untuk mundur melewati spasi itu supaya kursor
berada tepat di ujung baris dengan cara yang enak dipandang.

\index{{\backslash}b backspace@\texttt{{\backslash}b} backspace|)}

Perhatikan bahwa baris 15 juga punya banyak spasi di akhir untuk menimpa
karakter yang sudah ada dari hitung mundur.

Terakhir, ada `fflush(stdout)` yang aneh di situ, apa pun artinya.
Jawaban singkatnya, kebanyakan terminal secara bawaan _line buffered_,
artinya mereka tidak benar-benar menampilkan apa pun sampai ada karakter
newline. Karena kita tidak punya newline (kita hanya punya `\r`), tanpa
baris ini program akan diam saja sampai `Liftoff!` lalu mencetak semuanya
sekaligus dalam sekejap. `fflush()` menimpa perilaku ini dan memaksa
output terjadi _sekarang juga_.

\index{{\backslash}r carriage return@\texttt{{\backslash}r} carriage return|)}

#### Escape Tanda Tanya

\index{{\backslash}? question mark@\texttt{{\backslash}?} question mark|(}

Untuk apa repot-repot dengan ini? Toh ini berfungsi dengan baik:

``` {.c}
printf("Doesn't it?\n");
```

Dan berfungsi baik juga dengan escape:

``` {.c}
printf("Doesn't it\?\n");   // Perhatikan \?
```

Jadi apa gunanya??!

\index{Trigraphs|(}

Mari kita lebih tegas dengan satu tanda tanya lagi dan tanda seru:

``` {.c}
printf("Doesn't it??!\n");
```

Waktu kukompilasi, aku mendapat peringatan ini:

``` {.zsh}
foo.c: In function ‘main’:
foo.c:5:23: warning: trigraph ??! converted to | [-Wtrigraphs]
    5 |     printf("Doesn't it??!\n");
      |    
```

Dan menjalankannya memberi hasil yang tidak disangka:

``` {.default}
Doesn't it|
```

Jadi _trigraph_? Apa-apaan ini??!

Aku yakin kita akan mengunjungi lagi sudut berdebu bahasa ini nanti,
tapi singkatnya compiler mencari tiga serangkai karakter tertentu yang
diawali `??` dan menggantinya dengan karakter lain. Jadi kalau kamu di
terminal kuno tanpa simbol pipa (`|`) di keyboard, kamu bisa mengetik `??!`
sebagai gantinya.

Kamu bisa memperbaikinya dengan meng-escape tanda tanya kedua, seperti ini:

``` {.c}
printf("Doesn't it?\?!\n");
```

Lalu ia dikompilasi dan berfungsi seperti yang diharapkan.

Zaman sekarang, tentu saja, tidak ada yang memakai trigraph. Tapi `??!`
itu kadang muncul kalau kamu memutuskan memakainya dalam string untuk
penekanan.

\index{Trigraphs|)}
\index{{\backslash}? question mark@\texttt{{\backslash}?} question mark|)}

### Escape Numerik

Selain itu, ada cara untuk menentukan konstanta numerik atau nilai
karakter lain di dalam string atau konstanta karakter.

Kalau kamu tahu representasi oktal atau heksadesimal sebuah byte, kamu bisa
menyertakannya dalam string atau konstanta karakter.

Tabel berikut memakai angka contoh, tapi angka hex atau oktal apa pun bisa
dipakai. Beri nol di depan kalau perlu agar jumlah digitnya sesuai.

\index{{\backslash}123 octal value@\texttt{{\backslash}123} octal value}
\index{{\backslash}x12 hexadecimal value@\texttt{{\backslash}x12} hexadecimal value}
\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape}
\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape}

|Kode|Deskripsi|
|--|------------|
|`\123`|Sematkan byte bernilai oktal `123`, tepat 3 digit.|
|`\x4D`|Sematkan byte bernilai hex `4D`, 2 digit.|
|`\u2620`|Sematkan karakter Unicode pada code point bernilai hex `2620`, 4 digit.|
|`\U0001243F`|Sematkan karakter Unicode pada code point bernilai hex `1243F`, 8 digit.|

Berikut contoh notasi oktal yang jarang dipakai untuk merepresentasikan
huruf `B` di antara `A` dan `C`. Biasanya ini dipakai untuk semacam
karakter khusus yang tidak bisa dicetak, tapi kita punya cara lain untuk
itu, di bawah, dan ini hanya demo oktal:

\index{{\backslash}123 octal value@\texttt{{\backslash}123} octal value|(}

``` {.c}
printf("A\102C\n");  // 102 adalah `B` di ASCII/UTF-8
```

Perhatikan tidak ada nol di depan pada angka oktal saat disertakan dengan
cara ini. Tapi harus tiga karakter, jadi beri nol di depan kalau perlu.

\index{{\backslash}123 octal value@\texttt{{\backslash}123} octal value|)}

\index{{\backslash}x12 hexadecimal value@\texttt{{\backslash}x12} hexadecimal value|(}

Tapi zaman sekarang jauh lebih umum memakai konstanta hex. Berikut demo
yang sebaiknya tidak kamu pakai, tapi ia menunjukkan penyematan
byte UTF-8 0xE2, 0x80, dan 0xA2 dalam string, yang sesuai dengan karakter
"bullet" Unicode (•).

``` {.c}
printf("\xE2\x80\xA2 Bullet 1\n");
printf("\xE2\x80\xA2 Bullet 2\n");
printf("\xE2\x80\xA2 Bullet 3\n");
```

Menghasilkan output berikut kalau kamu di konsol UTF-8 (atau mungkin
sampah kalau bukan):

``` {.default}
• Bullet 1
• Bullet 2
• Bullet 3
```

\index{{\backslash}x12 hexadecimal value@\texttt{{\backslash}x12} hexadecimal value|)}


\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|(}
\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|(}

Tapi itu cara yang jelek untuk Unicode. Kamu bisa memakai escape `\u`
(16-bit) atau `\U` (32-bit) untuk merujuk ke Unicode lewat nomor code
point. Bullet adalah `2022` (hex) di Unicode, jadi kamu bisa melakukan ini
dan mendapat hasil yang lebih portabel:

``` {.c}
printf("\u2022 Bullet 1\n");
printf("\u2022 Bullet 2\n");
printf("\u2022 Bullet 3\n");
```

Pastikan memberi `\u` nol di depan secukupnya untuk mencapai empat
karakter, dan `\U` secukupnya untuk mencapai delapan.

\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|)}

Misalnya, bullet itu bisa ditulis dengan `\U` dan empat nol di depan:

``` {.c}
printf("\U00002022 Bullet 1\n");
```

\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|)}

Tapi siapa yang punya waktu untuk bertele-tele begitu?

\index{{\backslash} backslash escape@\texttt{{\backslash}} backslash escape|)}
\index{Escape sequences|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Enumerated Type: `enum`

\index{enum enumerated types@\texttt{enum} enumerated types|(}

C menawarkan cara lain untuk memiliki nilai integer konstan dengan nama:
`enum`.

Misalnya:

``` {.c}
enum {
  ONE=1,
  TWO=2
};

printf("%d %d", ONE, TWO);  // 1 2
```

Dalam beberapa hal, ini bisa lebih baik---atau berbeda---dibanding memakai
`#define`. Perbedaan utamanya:

* `enum` hanya bisa berupa tipe integer.
* `#define` bisa mendefinisikan apa saja.
* `enum` sering ditampilkan dengan nama identifier simbolisnya di debugger.
* Angka hasil `#define` hanya tampil sebagai angka mentah yang lebih sulit
  diketahui maknanya saat debugging.

Karena berupa tipe integer, mereka bisa dipakai di tempat mana pun
integer bisa dipakai, termasuk pada dimensi array dan pernyataan `case`.

Mari kita bedah lebih jauh.

## Perilaku `enum`

### Penomoran

\index{enum enumerated types@\texttt{enum} enumerated types!numbering order|(}

`enum` diberi nomor otomatis kecuali kamu menimpanya.

Penomoran dimulai dari `0`, dan bertambah otomatis dari sana, secara
bawaan:

``` {.c}
enum {
    SHEEP,  // Nilainya 0
    WHEAT,  // Nilainya 1
    WOOD,   // Nilainya 2
    BRICK,  // Nilainya 3
    ORE     // Nilainya 4
};

printf("%d %d\n", SHEEP, BRICK);  // 0 3
```

Kamu bisa memaksakan nilai integer tertentu, seperti yang sudah kita
lihat tadi:

``` {.c}
enum {
  X=2,
  Y=18,
  Z=-2
};
```

Duplikat tidak masalah:

``` {.c}
enum {
  X=2,
  Y=2,
  Z=2
};
```

kalau nilai dihilangkan, penomoran melanjutkan hitungan ke arah positif
dari nilai yang terakhir ditentukan. Misalnya:

``` {.c}
enum {
  A,    // 0, nilai awal bawaan
  B,    // 1
  C=4,  // 4, diatur manual
  D,    // 5
  E,    // 6
  F=3,  // 3, diatur manual
  G,    // 4
  H     // 5
}
```

\index{enum enumerated types@\texttt{enum} enumerated types!numbering order|)}

### Koma di Akhir

Ini sepenuhnya boleh, kalau itu gayamu:

``` {.c}
enum {
  X=2,
  Y=18,
  Z=-2,   // <-- Koma di akhir
};
```

Gaya ini makin populer di bahasa-bahasa dekade terakhir, jadi kamu mungkin
senang melihatnya.

### Scope

\index{enum enumerated types@\texttt{enum} enumerated types!scope|(}

`enum` mengikuti scope seperti yang kamu harapkan. Kalau di file scope,
seluruh file bisa melihatnya. Kalau di dalam blok, ia lokal untuk blok itu.

Sangat umum `enum` didefinisikan di header file sehingga bisa di-`#include`
di file scope.

\index{enum enumerated types@\texttt{enum} enumerated types!scope|)}

### Gaya Penulisan

Seperti yang kamu perhatikan, umumnya simbol `enum` dideklarasikan dengan
huruf besar (dengan underscore).

Ini bukan keharusan, tapi merupakan idiom yang sangat, sangat umum.

## `enum`-mu adalah Sebuah Tipe

Ini hal penting yang perlu diketahui tentang `enum`: ia adalah sebuah
tipe, seperti halnya `struct` adalah sebuah tipe.

Kamu bisa memberinya nama tag sehingga bisa merujuk ke tipe itu nanti dan
mendeklarasikan variabel bertipe itu.

Nah, karena `enum` adalah tipe integer, kenapa tidak pakai `int` saja?

Di C, alasan terbaiknya adalah kejelasan kode--ini cara bertipe yang rapi
untuk menjelaskan pemikiranmu dalam kode. C (tidak seperti C++) sebenarnya
tidak memaksa nilai apa pun berada dalam rentang untuk `enum` tertentu.

Mari kita coba contoh ketika kita mendeklarasikan variabel `r` bertipe
`enum resource` yang bisa menampung nilai-nilai itu:

``` {.c}
// enum bernama, tipenya "enum resource"

enum resource {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
};

// Deklarasikan variabel "r" bertipe "enum resource"

enum resource r = BRICK;

if (r == BRICK) {
    printf("I'll trade you a brick for two sheep.\n");
}
```

Kamu juga bisa me-`typedef`-nya, tentu saja, walaupun secara pribadi
aku tidak suka.


``` {.c}
typedef enum {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} RESOURCE;

RESOURCE r = BRICK;
```

Jalan pintas lain yang legal tapi jarang adalah mendeklarasikan variabel
saat mendeklarasikan `enum`:

``` {.c}
// Deklarasikan enum dan beberapa variabel terinisialisasi bertipe itu:

enum {
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} r = BRICK, s = WOOD;
```

Kamu juga bisa memberi nama pada `enum` agar bisa dipakai nanti, yang
mungkin ingin kamu lakukan pada kebanyakan kasus:


``` {.c}
// Deklarasikan enum dan beberapa variabel terinisialisasi bertipe itu:

enum resource {   // <-- tipenya sekarang "enum resource"
    SHEEP,
    WHEAT,
    WOOD,
    BRICK,
    ORE
} r = BRICK, s = WOOD;
```

Singkatnya, `enum` adalah cara yang bagus untuk menulis kode yang rapi,
ber-scope, dan bertipe.

\index{enum enumerated types@\texttt{enum} enumerated types|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Pointer III: Pointer ke Pointer dan Lainnya

Di sinilah kita membahas pemakaian pointer tingkat menengah dan lanjut.
Kalau kamu belum menguasai pointer, ulas dulu bab-bab sebelumnya tentang
[pointer](#pointers) dan [aritmetika pointer](#pointers2) sebelum mulai
dengan materi ini.

## Pointer ke Pointer

\index{Pointers!to pointers|(}

Kalau kamu bisa punya pointer ke sebuah variabel, dan variabel bisa
berupa pointer, bisakah kamu punya pointer ke variabel yang itu sendiri
adalah pointer?

Bisa! Itulah pointer ke pointer, yang disimpan dalam variabel
bertipe pointer-pointer. 

Sebelum membedahnya, aku ingin mencoba membangun _gut feel_ tentang cara
kerja pointer ke pointer.

Ingat bahwa pointer hanyalah sebuah angka. Angka yang mewakili indeks
dalam memori komputer, biasanya indeks yang menyimpan nilai yang
menarik minat kita karena suatu alasan.

Pointer itu, yang berupa angka, harus disimpan di suatu tempat. Dan
tempat itu adalah memori, sama seperti yang lainnya^[Ada setan di
detailnya untuk nilai yang hanya disimpan di register, tapi untuk
keperluan kita di sini, hal itu aman untuk diabaikan. Lagi pula
spesifikasi C tidak mengambil sikap soal "register" ini di luar kata
kunci `register`, yang deskripsinya tidak menyebut register.].

Tapi karena disimpan di memori, ia pasti punya indeks tempat ia disimpan,
kan? Pointer itu pasti punya indeks di memori tempat ia disimpan. Dan
indeks itu adalah sebuah angka. Itulah alamat pointer tersebut. Itulah
pointer ke pointer.

Kita mulai dengan pointer biasa ke sebuah `int`, dari bab-bab sebelumnya:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int x = 3490;  // Tipe: int
    int *p = &x;   // Tipe: pointer ke int

    printf("%d\n", *p);  // 3490
}
```

Cukup lugas, kan? Ada dua tipe yang terlibat: `int` dan `int*`, dan kita
mengatur `p` agar menunjuk ke `x`. Lalu kita bisa men-dereference `p` di
baris 8 dan mencetak nilai `3490`.

Tapi, seperti yang kita katakan, kita bisa punya pointer ke variabel
apa pun... jadi apakah itu berarti kita bisa punya pointer ke `p`?

Dengan kata lain, ekspresi ini bertipe apa?


``` {.c}
int x = 3490;  // Tipe: int
int *p = &x;   // Tipe: pointer ke int

&p  // <-- Apa tipe alamat p? Alias pointer ke p?
```

Kalau `x` adalah `int`, maka `&x` adalah pointer ke `int` yang kita simpan
di `p` yang bertipe `int*`. Ikut? (Ulangi paragraf ini sampai paham!)

Dan karena itu `&p` adalah pointer ke `int*`, alias "pointer ke pointer
ke `int`". Alias "`int`-pointer-pointer".

Paham? (Ulangi paragraf sebelumnya sampai paham!)

Kita menulis tipe ini dengan dua asterisk: `int **`. Kita lihat
pemakaiannya.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int x = 3490;  // Tipe: int
    int *p = &x;   // Tipe: pointer ke int
    int **q = &p;  // Tipe: pointer ke pointer ke int

    printf("%d %d\n", *p, **q);  // 3490 3490
}
```

Mari kita karang beberapa alamat pura-pura untuk nilai-nilai di atas
sebagai contoh dan lihat seperti apa ketiga variabel ini di memori. Nilai
alamat di bawah ini hanya karanganku untuk keperluan contoh:

|Variabel|Disimpan di Alamat|Nilai yang Tersimpan di Sana|
|-|-|-|
|`x`|`28350`|`3490`---nilai dari kode|
|`p`|`29122`|`28350`---alamat `x`!|
|`q`|`30840`|`29122`---alamat `p`!|

Sekarang mari kita coba sungguhan di komputerku^[Kemungkinan besar
angka di komputermu berbeda.] dan mencetak nilai pointer dengan `%p`, lalu
aku ulangi tabel yang sama dengan referensi sebenarnya (dicetak dalam hex).

|Variabel|Disimpan di Alamat|Nilai yang Tersimpan di Sana|
|-|-|-|
|`x`|`0x7ffd96a07b94`|`3490`---nilai dari kode|
|`p`|`0x7ffd96a07b98`|`0x7ffd96a07b94`---alamat `x`!|
|`q`|`0x7ffd96a07ba0`|`0x7ffd96a07b98`---alamat `p`!|

Kamu bisa lihat alamat-alamat itu sama kecuali byte terakhir, jadi fokus
saja ke sana.

Di sistemku, `int` berukuran 4 byte, itulah kenapa kita melihat
alamat naik 4 dari `x` ke `p`^[Sama sekali tidak ada di spesifikasi yang
mengatakan ini akan selalu begini, tapi kebetulan di sistemku
begitu.] lalu naik 8 dari `p` ke `q`. Di sistemku, semua pointer
berukuran 8 byte.

Apakah penting kalau itu `int*` atau `int**`? Apakah salah satunya lebih
banyak byte-nya? Tidak! Ingat bahwa semua pointer adalah alamat, yaitu
indeks ke dalam memori. Dan di mesinku, sebuah indeks bisa
direpresentasikan dengan 8 byte... tidak peduli apa yang disimpan di
indeks itu.

Sekarang lihat apa yang kita lakukan di baris 9 pada contoh sebelumnya:
kita _men-dereference ganda_ `q` untuk kembali ke `3490`.

Ini bagian penting tentang pointer dan pointer ke pointer:

* Kamu bisa mendapat pointer ke apa pun dengan `&` (termasuk ke pointer!)
* Kamu bisa mendapat yang ditunjuk sebuah pointer dengan `*` (termasuk
  pointer!)

Jadi kamu bisa menganggap `&` dipakai untuk membuat pointer, dan `*`
adalah kebalikannya---ia berjalan ke arah yang berlawanan dari `&`---untuk
sampai ke yang ditunjuk.

Dari sisi tipe, setiap kali kamu memakai `&`, itu menambah satu tingkat
pointer pada tipenya.

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int x`|`&x`|`int *`|
|`int *x`|`&x`|`int **`|
|`int **x`|`&x`|`int ***`|
|`int ***x`|`&x`|`int ****`|

Dan setiap kali kamu memakai dereference (`*`), ia melakukan kebalikannya:

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int ****x`|`*x`|`int ***`|
|`int ***x`|`*x`|`int **`|
|`int **x`|`*x`|`int *`|
|`int *x`|`*x`|`int`|

Perhatikan bahwa kamu bisa memakai beberapa `*` berturut-turut untuk
men-dereference dengan cepat, seperti yang kita lihat pada kode contoh
dengan `**q` di atas. Masing-masing mengupas satu tingkat indirection.

|Jika kamu punya|Lalu kamu jalankan|Tipe hasilnya|
|:-|:-:|:-|
|`int ****x`|`***x`|`int *`|
|`int ***x`|`**x`|`int *`|
|`int **x`|`**x`|`int`|

Secara umum, `&*E == E`^[Bahkan kalau `E` adalah `NULL`, ternyata,
anehnya.]. Dereference "membatalkan" address-of.

Tapi `&` tidak bekerja dengan cara yang sama---kamu hanya bisa
melakukannya satu per satu, dan harus menyimpan hasilnya di variabel
perantara:

``` {.c}
int x = 3490;     // Tipe: int
int *p = &x;      // Tipe: int *, pointer ke int
int **q = &p;     // Tipe: int **, pointer ke pointer ke int
int ***r = &q;    // Tipe: int ***, pointer ke pointer ke pointer ke int
int ****s = &r;   // Tipe: int ****, kamu paham maksudnya
int *****t = &s;  // Tipe: int *****
```

\index{Pointers!to pointers|)}

### Pointer Pointer dan `const`

\index{Pointers!to pointers, const@to pointers, \texttt{const}|(}

Kalau kamu ingat, mendeklarasikan pointer seperti ini:

``` {.c}
int *const p;
```

berarti kamu tidak bisa mengubah `p`. Mencoba `p++` akan memberi error saat
kompilasi.

Tapi bagaimana ini bekerja dengan `int **` atau `int ***`? Di mana
`const` ditaruh, dan apa artinya?

Kita mulai dari bagian sederhana. `const` yang tepat di sebelah nama
variabel merujuk ke variabel itu. Jadi kalau kamu ingin `int***` yang
tidak bisa diubah, kamu bisa menulis:

``` {.c}
int ***const p;

p++;  // Tidak diperbolehkan
```

Tapi di sinilah semuanya jadi agak aneh.

Bagaimana kalau kita punya situasi ini:

``` {.c .numberLines}
int main(void)
{
    int x = 3490;
    int *const p = &x;
    int **q = &p;
}
```

Waktu kubangun, aku mendapat peringatan ini:

``` {.default}
warning: initialization discards ‘const’ qualifier from pointer target type
    7 |     int **q = &p;
      |               ^
```

Apa yang terjadi? Compiler memberi tahu kita di sini bahwa ada variabel
yang `const`, dan kita menugaskan nilainya ke variabel lain yang tidak
`const` dengan cara yang sama. Sifat "`const`" dibuang, yang mungkin
bukan yang kita inginkan.

Tipe `p` adalah `int *const p`, jadi `&p` bertipe `int *const *`. Dan kita
mencoba menugaskannya ke `q`.

Tapi `q` adalah `int **`! Tipe dengan sifat `const` yang berbeda pada `*`
pertama! Jadi kita mendapat peringatan bahwa `const` pada `int *const *`
milik `p` diabaikan dan dibuang.

Kita bisa memperbaikinya dengan memastikan tipe `q` setidaknya sama
`const`-nya dengan `p`.

``` {.c}
int x = 3490;
int *const p = &x;
int *const *q = &p;
```

Dan sekarang kita senang.

Kita bahkan bisa membuat `q` lebih `const` lagi. Seperti di atas, kita
berkata, "`q` itu sendiri tidak `const`, tapi yang ditunjuknya `const`."
Tapi kita bisa membuat keduanya `const`:

``` {.c}
int x = 3490;
int *const p = &x;
int *const *const q = &p;  // Lebih const!
```

Dan itu juga berfungsi. Sekarang kita tidak bisa mengubah `q`, maupun
pointer yang ditunjuk `q`.

\index{Pointers!to pointers, const@to pointers, \texttt{const}|)}

## Nilai Multibyte {#multibyte-values}

\index{Pointers!to multibyte values|(}

Kita sudah menyiratkan ini di berbagai tempat sebelumnya, tapi jelas tidak
setiap nilai bisa disimpan dalam satu byte memori. Banyak hal memakai
beberapa byte memori (dengan asumsi bukan `char`). Kamu bisa mengetahui
jumlah byte dengan `sizeof`. Dan kamu bisa mengetahui alamat memori mana
yang merupakan byte _pertama_ dari object dengan operator `&` standar,
yang selalu mengembalikan alamat byte pertama.

Dan ini fakta menarik lainnya! Kalau kamu menelusuri byte-byte dari object
mana pun, kamu mendapat _object representation_-nya. Dua hal dengan object
representation yang sama di memori adalah sama.

Kalau kamu ingin menelusuri object representation, lakukan dengan
pointer ke `unsigned char`.

Mari kita buat versi kita sendiri dari
[`memcpy()`](https://beej.us/guide/bgclr/html/split/stringref.html#man-memcpy)^[https://beej.us/guide/bgclr/html/split/stringref.html#man-memcpy]
yang melakukan persis itu:

``` {.c}
void *my_memcpy(void *dest, const void *src, size_t n)
{
    // Buat variabel lokal untuk src dan dest, tapi bertipe unsigned char

    const unsigned char *s = src;
    unsigned char *d = dest;

    while (n-- > 0)   // Untuk jumlah byte yang diberikan
        *d++ = *s++;  // Salin byte sumber ke byte tujuan

    // Kebanyakan fungsi salin mengembalikan pointer ke dest demi
    // kemudahan caller

    return dest;
}
```

(Ada beberapa contoh bagus post-increment dan post-decrement di situ yang
bisa kamu pelajari juga.)

Penting dicatat bahwa versi di atas kemungkinan kurang efisien dibanding
yang disertakan di sistemmu.

Tapi kamu bisa mengoper pointer ke apa pun ke dalamnya, dan ia akan
menyalin object itu. Bisa `int*`, `struct animal*`, atau apa saja.

Mari kita coba contoh lain yang mencetak byte object representation dari
sebuah `struct` supaya kita bisa melihat apakah ada padding di dalamnya
dan nilainya apa^[C compiler-mu tidak wajib menambahkan padding byte, dan
nilai padding byte yang ditambahkan itu tak tentu.].

``` {.c .numberLines}
#include <stdio.h>

struct foo {
    char a;
    int b;
};

int main(void)
{
    struct foo x = {0x12, 0x12345678};
    unsigned char *p = (unsigned char *)&x;

    for (size_t i = 0; i < sizeof x; i++) {
        printf("%02X\n", p[i]);
    }
}
```

Di situ ada `struct foo` yang disusun sedemikian rupa agar compiler
terdorong menyisipkan padding byte (walaupun tidak wajib). Lalu kita
ambil `unsigned char *` ke byte pertama variabel
`struct foo` `x`.

Dari sana, yang kita perlu tahu hanya `sizeof x` dan kita bisa menelusuri
sebanyak itu byte, mencetak nilainya (dalam hex agar mudah).

Menjalankannya memberi banyak angka sebagai output. Aku sudah memberinya
anotasi di bawah untuk menunjukkan di mana nilai-nilai itu disimpan:

``` {.default}
12  | x.a == 0x12

AB  |
BF  | padding bytes with "random" value
26  |

78  | x.b == 0x12345678
56  |
34  |
12  |
```

Di semua sistem, `sizeof(char)` adalah 1, dan kita lihat byte pertama di
bagian atas output menyimpan nilai `0x12` yang kita simpan di sana.

Lalu ada beberapa padding byte---di sistemku, nilainya berubah dari
satu eksekusi ke eksekusi berikutnya.

Terakhir, di sistemku, `sizeof(int)` adalah 4, dan kita bisa
melihat 4 byte itu di bagian akhir. Perhatikan bahwa byte-nya sama dengan
nilai hex `0x12345678`, tapi anehnya dalam urutan terbalik^[Ini bervariasi
tergantung arsitektur, tapi sistemku _little endian_, artinya byte
paling tidak signifikan dari angka disimpan lebih dulu. Sistem _big
endian_ akan punya `12` di depan dan `78` di belakang. Tapi spesifikasi
tidak menentukan apa pun soal representasi ini.].

Jadi itu sedikit intipan ke bawah kap tentang byte-byte dari entitas yang
lebih kompleks di memori.

\index{Pointers!to multibyte values|)}

## Pointer `NULL` dan Nol

\index{NULL pointer@\texttt{NULL} pointer!zero equivalence|(}

Hal-hal berikut bisa dipakai secara bergantian:

* `NULL`
* `0`
* `'\0'`
* `(void *)0`

Secara pribadi, aku selalu memakai `NULL` kalau maksudnya `NULL`,
tapi kamu mungkin sesekali melihat varian lain. Walaupun `'\0'` (byte
dengan semua bit bernilai nol) juga akan dibandingkan sebagai sama,
membandingkannya dengan pointer itu _aneh_; kamu seharusnya
membandingkan `NULL` dengan pointer. (Tentu saja, sering kali dalam
pemrosesan string, kamu membandingkan _yang ditunjuk pointer_ dengan
`'\0'`, dan itu benar.)

`0` disebut _null pointer constant_, dan, ketika dibandingkan atau
ditugaskan ke pointer lain, ia dikonversi menjadi null pointer dengan
tipe yang sama.

\index{NULL pointer@\texttt{NULL} pointer!zero equivalence|)}

## Pointer sebagai Integer

\index{Pointers!as integers|(}

Kamu bisa meng-cast pointer ke integer dan sebaliknya (karena pointer
hanyalah indeks ke memori), tapi kemungkinan kamu hanya perlu melakukannya
kalau mengerjakan hal-hal hardware level rendah. Hasil dari akal-akalan
semacam itu ditentukan oleh implementasi, jadi tidak portabel. Dan _hal
aneh_ bisa terjadi.

Meski begitu, C memberi satu jaminan: kamu bisa mengonversi pointer ke
tipe `uintptr_t` dan kamu bisa mengonversinya kembali ke pointer tanpa
kehilangan data.

`uintptr_t` didefinisikan di `<stdint.h>`^[Ini fitur opsional, jadi
mungkin tidak ada---tapi kemungkinan besar ada.].

Selain itu, kalau kamu ingin yang signed, kamu bisa memakai `intptr_t`
dengan efek yang sama.

\index{Pointers!as integers|)}

## Meng-cast Pointer ke Pointer Lain

\index{Pointers!casting|(}

Hanya ada satu konversi pointer yang aman:

1. Mengonversi ke `intptr_t` atau `uintptr_t`.
2. Mengonversi ke dan dari `void*`.

DUA! Dua konversi pointer yang aman.

3. Mengonversi ke dan dari `char*` (atau `signed char*`/`unsigned char*`).

TIGA! Tiga konversi yang aman!

4. Mengonversi ke dan dari pointer ke sebuah `struct` dan pointer ke
   anggota pertamanya, dan sebaliknya.

EMPAT! Empat konversi yang aman!

Kalau kamu meng-cast ke pointer bertipe lain lalu mengakses object yang
ditunjuknya, perilakunya undefined karena sesuatu yang disebut _strict
aliasing_.

_Aliasing_ biasa merujuk ke kemampuan untuk punya lebih dari satu cara
mengakses object yang sama. Titik-titik akses itu adalah alias satu sama
lain.

_Strict aliasing_ menyatakan kamu hanya boleh mengakses sebuah object lewat
pointer ke _tipe yang kompatibel_ dengan object itu.

Misalnya, ini jelas diperbolehkan:

``` {.c}
int a = 1;
int *p = &a;
```

`p` adalah pointer ke `int`, dan ia menunjuk ke tipe yang
kompatibel---yaitu `int`---jadi aman.

Tapi yang berikut tidak bagus karena `int` dan `float` bukan tipe yang
kompatibel:

``` {.c}
int a = 1;
float *p = (float *)&a;
```

Berikut program demo yang melakukan aliasing. Ia mengambil variabel `v`
bertipe `int32_t` dan meng-alias-kannya ke pointer ke `struct words`.
`struct` itu berisi dua `int16_t`. Tipe-tipe ini tidak kompatibel, jadi
kita melanggar aturan strict aliasing. Compiler akan menganggap kedua
pointer ini tidak pernah menunjuk ke object yang sama... tapi kita
membuatnya menunjuk. Nakal sekali kita ini.

Mari kita lihat apakah kita bisa merusak sesuatu.

``` {.c .numberLines}
#include <stdio.h>
#include <stdint.h>

struct words {
    int16_t v[2];
};

void fun(int32_t *pv, struct words *pw)
{
    for (int i = 0; i < 5; i++) {
        (*pv)++;

        // Cetak nilai 32-bit dan nilai-nilai 16-bit:

        printf("%x, %x-%x\n", *pv, pw->v[1], pw->v[0]);
    }
}

int main(void)
{
    int32_t v = 0x12345678;

    struct words *pw = (struct words *)&v;  // Melanggar strict aliasing

    fun(&v, pw);
}
```

Lihat bagaimana aku mengoper kedua pointer yang tidak kompatibel ke `fun()`?
Satu bertipe `int32_t*` dan yang lain `struct words*`.

Tapi keduanya menunjuk ke object yang sama: nilai 32-bit yang
diinisialisasi dengan `0x12345678`.

Jadi kalau kita lihat field-field di `struct words`, kita seharusnya
melihat dua setengah bagian 16-bit dari angka itu. Benar?

Dan di loop `fun()`, kita meng-increment pointer ke `int32_t`. Itu saja.
Tapi karena `struct` menunjuk ke memori yang sama, ia juga seharusnya
diperbarui ke nilai yang sama.

Mari kita jalankan dan dapatkan ini, dengan nilai 32-bit di kiri dan dua
bagian 16-bit di kanan. Seharusnya cocok^[Aku mencetak nilai 16-bit
secara terbalik karena aku memakai mesin little-endian dan ini membuatnya
lebih mudah dibaca di sini.]:

``` {.default}
12345679, 1234-5679
1234567a, 1234-567a
1234567b, 1234-567b
1234567c, 1234-567c
1234567d, 1234-567d
```

dan memang cocok... _SAMPAI BESOK!_

Mari kita coba mengompilasinya dengan GCC memakai `-O3` dan `-fstrict-aliasing`:

``` {.default}
12345679, 1234-5678
1234567a, 1234-5679
1234567b, 1234-567a
1234567c, 1234-567b
1234567d, 1234-567c
```

Selisih satu! Tapi mereka menunjuk ke memori yang sama! Bagaimana bisa?
Jawabannya: melakukan aliasing memori seperti itu adalah undefined
behavior. _Apa pun mungkin terjadi_, hanya saja bukan dengan cara yang
baik.

Kalau kodemu melanggar aturan strict aliasing, apakah ia berfungsi atau
tidak bergantung pada bagaimana seseorang memutuskan mengompilasinya. Dan
itu menyebalkan karena di luar kendalimu. Kecuali kamu semacam dewa
mahakuasa.

Kemungkinan tidak, maaf.

GCC bisa dipaksa untuk tidak memakai aturan strict aliasing dengan
`-fno-strict-aliasing`. Mengompilasi program demo di atas dengan `-O3` dan
flag ini membuat outputnya sesuai harapan.

Terakhir, _type punning_ adalah memakai pointer bertipe berbeda untuk
melihat data yang sama. Sebelum ada strict aliasing, hal seperti ini cukup
umum:

``` {.c}
int a = 0x12345678;
short b = *((short *)&a);   // Melanggar strict aliasing
```

Kalau kamu ingin melakukan type punning dengan (relatif) aman, lihat
bagian tentang [Union dan Type Punning](#union-type-punning).

\index{Pointers!casting|)}

## Selisih Pointer {#ptr_differences}

\index{Pointers!subtracting|(}

Seperti yang kamu tahu dari bagian tentang aritmetika pointer, kamu bisa
mengurangkan satu pointer dari pointer lain^[Dengan asumsi keduanya
menunjuk ke object array yang sama.] untuk mendapatkan selisihnya dalam
jumlah elemen array.

Nah, _tipe selisih itu_ ditentukan oleh implementasi, jadi bisa berbeda
dari satu sistem ke sistem lain.

\index{ptrdiff\_t type@\texttt{ptrdiff\_t} type|(}

Agar lebih portabel, kamu bisa menyimpan hasilnya dalam variabel bertipe
`ptrdiff_t` yang didefinisikan di `<stddef.h>`.

``` {.c}
int cats[100];

int *f = cats + 20;
int *g = cats + 60;

ptrdiff_t d = g - f;  // selisihnya 40
```

\index{ptrdiff\_t type@\texttt{ptrdiff\_t} type!printing|(}
Dan kamu bisa mencetaknya dengan menambahkan awalan `t` pada format
specifier integer:

``` {.c}
printf("%td\n", d);  // Cetak desimal: 40
printf("%tX\n", d);  // Cetak hex:     28
```

\index{ptrdiff\_t type@\texttt{ptrdiff\_t} type!printing|)}
\index{ptrdiff\_t type@\texttt{ptrdiff\_t} type|)}
\index{Pointers!subtracting|)}

## Pointer ke Fungsi

\index{Pointers!to functions|(}

Fungsi hanyalah kumpulan instruksi mesin di memori, jadi tidak ada alasan
kita tidak bisa mengambil pointer ke instruksi pertama fungsi itu.

Lalu memanggilnya.

Ini bisa berguna untuk mengoper pointer ke sebuah fungsi ke fungsi lain
sebagai argumen. Lalu fungsi kedua itu bisa memanggil apa pun yang
dioper.

Bagian yang rumit, meski begitu, adalah C perlu tahu tipe variabel yang
berupa pointer ke fungsi itu.

Dan ia benar-benar ingin tahu semua detailnya.

Seperti "ini adalah pointer ke fungsi yang menerima dua argumen `int` dan
mengembalikan `void`".

Bagaimana menuliskan semua itu agar bisa mendeklarasikan variabel?

Ternyata bentuknya sangat mirip function prototype, hanya dengan beberapa
tanda kurung tambahan:

``` {.c}
// Deklarasikan p sebagai pointer ke fungsi.
// Fungsi ini mengembalikan float, dan menerima dua int sebagai argumen.

float (*p)(int, int);
```

Perhatikan juga bahwa kamu tidak harus memberi nama pada parameter. Tapi
boleh saja kalau mau; namanya hanya diabaikan.

``` {.c}
// Deklarasikan p sebagai pointer ke fungsi.
// Fungsi ini mengembalikan float, dan menerima dua int sebagai argumen.

float (*p)(int a, int b);
```

Jadi sekarang setelah kita tahu cara mendeklarasikan variabel, bagaimana
kita tahu apa yang ditugaskan ke dalamnya? Bagaimana mendapatkan alamat
sebuah fungsi?

Ternyata ada jalan pintas seperti saat mendapatkan pointer ke array: kamu
cukup menyebut nama fungsi tanpa kurung. (Kamu boleh menaruh `&` di
depannya kalau mau, tapi itu tidak perlu dan tidak idiomatis.)

Setelah punya pointer ke fungsi, kamu bisa memanggilnya hanya dengan
menambahkan tanda kurung dan daftar argumen.

Kita coba contoh sederhana yang secara efektif membuat alias untuk sebuah
fungsi dengan mengarahkan pointer ke sana. Lalu kita memanggilnya.

Kode ini mencetak `3490`:

``` {.c .numberLines}
#include <stdio.h>

void print_int(int n)
{
    printf("%d\n", n);
}

int main(void)
{
    // Tugaskan p agar menunjuk ke print_int:

    void (*p)(int) = print_int;

    p(3490);          // Panggil print_int lewat pointer
}
```

Perhatikan bagaimana tipe `p` merepresentasikan tipe nilai kembalian dan
parameter `print_int`. Harus begitu, kalau tidak C akan protes soal tipe
pointer yang tidak kompatibel.

Satu contoh lagi di sini menunjukkan bagaimana kita bisa mengoper pointer
ke fungsi sebagai argumen ke fungsi lain.

Kita akan menulis fungsi yang menerima beberapa argumen integer, ditambah
pointer ke fungsi yang beroperasi pada kedua argumen itu. Lalu ia
mencetak hasilnya.

``` {.c .numberLines}
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int mult(int a, int b)
{
    return a * b;
}

void print_math(int (*op)(int, int), int x, int y)
{
    int result = op(x, y);

    printf("%d\n", result);
}

int main(void)
{
    print_math(add, 5, 7);   // 12
    print_math(mult, 5, 7);  // 35
}
```

Luangkan waktu untuk mencernanya. Idenya, kita akan mengoper pointer ke
sebuah fungsi ke `print_math()`, dan ia akan memanggil fungsi itu untuk
melakukan perhitungan.

Dengan begitu kita bisa mengubah perilaku `print_math()` dengan mengoper
fungsi lain ke dalamnya. Kamu bisa lihat kita melakukannya di baris 22-23
saat mengoper pointer ke fungsi `add` dan `mult`.

Nah, di baris 13, kita semua pasti setuju signature fungsi `print_math()`
adalah pemandangan yang patut dikagumi. Dan, percaya atau tidak, yang ini
sebenarnya cukup lugas dibanding beberapa hal yang bisa kamu
susun^[Bahasa Pemrograman Go mengambil inspirasi sintaks deklarasi
tipenya dari kebalikan dari yang dilakukan C.].

Tapi mari kita cerna. Ternyata hanya ada tiga parameter, tapi agak sulit
dilihat:

``` {.c}
//                      op             x      y
//              |-----------------|  |---|  |---|
void print_math(int (*op)(int, int), int x, int y)
```

Parameter pertama, `op`, berupa pointer ke fungsi yang menerima dua `int`
sebagai argumen dan mengembalikan `int`. Ini cocok dengan signature
`add()` maupun `mult()`.

Yang kedua dan ketiga, `x` dan `y`, hanyalah parameter `int` biasa.

Pelan-pelan dan sengaja, biarkan matamu menelusuri signature itu sambil
mengenali bagian-bagian yang bekerja. Satu hal yang selalu menonjol bagiku
adalah urutan `(*op)(`, tanda kurung dan asterisk itu. Itulah
tanda bahwa ia pointer ke fungsi.

Terakhir, lompat kembali ke bab _Pointer II_ untuk melihat [contoh
pointer-ke-fungsi memakai `qsort()` bawaan](#qsort-example).

\index{Pointers!to functions|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Operasi Bitwise

\index{Bitwise operations|(}

Operasi numerik ini memungkinkan kamu memanipulasi bit individual dalam
variabel, cocok karena C adalah bahasa yang begitu low-level^[Bukan
berarti bahasa lain tidak melakukannya---mereka melakukannya. Menarik
betapa banyak bahasa modern memakai operator yang sama dengan C untuk
bitwise.].

Kalau kamu belum akrab dengan operasi bitwise, [Wikipedia punya
artikel bitwise yang bagus](https://en.wikipedia.org/wiki/Bitwise_operation)^[https://en.wikipedia.org/wiki/Bitwise_operation].

## Bitwise AND, OR, XOR, dan NOT

Untuk masing-masing, [usual arithmetic
conversion](#usual-arithmetic-conversions) terjadi pada operand (yang dalam
kasus ini harus bertipe integer), lalu operasi bitwise yang sesuai
dilakukan.

\index{\& bitwise AND@\texttt{\&} bitwise AND}
\index{\textbar{} bitwise OR@\texttt{\textbar{}} bitwise OR}
\index{\^{} bitwise XOR@\texttt{\^{}} bitwise XOR}
\index{~ bitwise NOT@\texttt{~} bitwise NOT}

|Operasi|Operator|Contoh|
|-|:-:|-|
|AND|`&`|`a = b & c`|
|OR|`|`|`a = b | c`|
|XOR|`^`|`a = b ^ c`|
|NOT|`~`|`a = ~c`|

Perhatikan betapa miripnya mereka dengan operator Boolean `&&` dan `||`.

Operator-operator ini punya varian singkat penugasan seperti `+=` dan
`-=`:

\index{\&= assignment@\texttt{\&=} assignment}
\index{\textbar{}= assignment@\texttt{\textbar{}=} assignment}
\index{\^{}= assignment@\texttt{\^{}=} assignment}

|Operator|Contoh|Bentuk panjang yang setara|
|-|-|-|
|`&=`|`a &= c`|`a = a & c`|
|`|=`|`a |= c`|`a = a | c`|
|`^=`|`a ^= c`|`a = a ^ c`|

## Bitwise Shift

Untuk ini, \index{Integer promotions} [integer
promotion](#integer-promotions) dilakukan pada setiap operand (yang harus
bertipe integer) lalu bitwise shift dieksekusi. Tipe hasilnya adalah tipe
operand kiri yang sudah dipromosikan.

Bit baru diisi dengan nol, dengan kemungkinan pengecualian yang disebut
pada perilaku yang ditentukan implementasi di bawah.

\index{<< shift left@\texttt{<<} shift left}
\index{>> shift right@\texttt{>>} shift right}

|Operasi|Operator|Contoh|
|-|:-:|-|
|Shift kiri|`<<`|`a = b << c`|
|Shift kanan|`>>`|`a = b >> c`|

Ada juga bentuk singkat yang serupa untuk shift:

\index{>>= assignment@\texttt{>>=} assignment}
\index{<<= assignment@\texttt{<<=} assignment}

|Operator|Contoh|Bentuk panjang yang setara|
|-|-|-|
|`>>=`|`a >>= c`|`a = a >> c`|
|`<<=`|`a <<= c`|`a = a << c`|

Waspadai undefined behavior: tidak boleh shift negatif, dan tidak boleh
shift yang lebih besar dari ukuran operand kiri yang sudah dipromosikan.

Waspadai juga perilaku yang ditentukan implementasi: kalau kamu melakukan
shift kanan pada angka negatif, hasilnya ditentukan implementasi. (Aman
saja melakukan shift kanan pada `int` signed, asalkan nilainya
positif.)

\index{Bitwise operations|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsi Variadic

\index{Variadic functions|(}

_Variadic_ adalah kata keren untuk fungsi yang menerima jumlah argumen
sembarang.

Fungsi biasa menerima jumlah argumen yang tertentu, misalnya:

``` {.c}
int add(int x, int y)
{
    return x + y;
}
```

Kamu hanya bisa memanggilnya dengan tepat dua argumen yang sesuai dengan
parameter `x` dan `y`.

``` {.c}
add(2, 3);
add(5, 12);
```

Tapi kalau kamu mencoba dengan lebih banyak, compiler tidak akan
mengizinkan:

``` {.c}
add(2, 3, 4);  // ERROR
add(5);        // ERROR
```

Fungsi variadic mengatasi batasan ini sampai tingkat tertentu.

Kita sudah melihat contoh terkenalnya di `printf()`! Kamu bisa mengoper
berbagai macam hal kepadanya.

``` {.c}
printf("Hello, world!\n");
printf("The number is %d\n", 2);
printf("The number is %d and pi is %f\n", 2, 3.14159);
```

Kelihatannya ia tidak peduli berapa banyak argumen yang diberikan!

Nah, itu tidak sepenuhnya benar. Nol argumen akan memberi error:

``` {.c}
printf();  // ERROR
```

Ini membawa kita ke salah satu batasan fungsi variadic di C: mereka harus
punya setidaknya satu argumen.

Tapi selain itu, mereka cukup fleksibel, bahkan membolehkan argumen
bertipe berbeda-beda seperti `printf()`.

Mari kita lihat cara kerjanya!

## Elipsis pada Signature Fungsi

Jadi bagaimana cara kerjanya, secara sintaksis?

\index{... variadic arguments@\texttt{...} variadic arguments|(}

Taruh semua argumen yang _harus_ dioper lebih dulu (dan ingat harus ada
setidaknya satu), lalu setelah itu taruh `...`. Seperti ini:

``` {.c}
void func(int a, ...)   // Secara harfiah 3 titik di sini
```

Berikut kode untuk mendemonstrasikannya:

``` {.c}
#include <stdio.h>

void func(int a, ...)
{
    printf("a is %d\n", a);  // Mencetak "a is 2"
}

int main(void)
{
    func(2, 3, 4, 5, 6);
}
```

\index{... variadic arguments@\texttt{...} variadic arguments|)}

Jadi, bagus, kita bisa mendapatkan argumen pertama yang ada di variabel
`a`, tapi bagaimana dengan argumen sisanya? Bagaimana cara mengaksesnya?

Di sinilah keseruan dimulai!

## Mengambil Argumen Tambahan

Kamu perlu meng-include \index{stdarg.h header file@\texttt{stdarg.h} header file} `<stdarg.h>` supaya
semua ini berfungsi.

\index{va\_list type@\texttt{va\_list} type|(}

Pertama-tama, kita akan memakai variabel khusus bertipe `va_list`
(variable argument list) untuk melacak variabel mana yang sedang kita
akses.

\index{va\_start() macro@\texttt{va\_start()} macro|(}
\index{va\_arg() macro@\texttt{va\_arg()} macro|(}
\index{va\_end() macro@\texttt{va\_end()} macro|(}

Idenya: kita mulai memproses argumen dengan memanggil `va_start()`,
memproses tiap argumen secara berurutan dengan `va_arg()`, lalu setelah
selesai, menutupnya dengan `va_end()`.

Saat memanggil `va_start()`, kamu perlu mengoper _parameter bernama
terakhir_ (yang tepat sebelum `...`) supaya ia tahu dari mana mulai
mencari argumen tambahan.

Dan saat memanggil `va_arg()` untuk mengambil argumen berikutnya, kamu
harus memberi tahu tipe argumen yang akan diambil.

Berikut demo yang menjumlahkan sejumlah integer sembarang. Argumen pertama
adalah jumlah integer yang akan dijumlahkan. Kita memakainya untuk
mengetahui berapa kali harus memanggil `va_arg()`.

``` {.c .numberLines}
#include <stdio.h>
#include <stdarg.h>

int add(int count, ...)
{
    int total = 0;
    va_list va;

    va_start(va, count);   // Mulai dengan argumen setelah "count"

    for (int i = 0; i < count; i++) {
        int n = va_arg(va, int);   // Ambil int berikutnya

        total += n;
    }

    va_end(va);  // Selesai

    return total;
}

int main(void)
{
    printf("%d\n", add(4, 6, 2, -4, 17));  // 6 + 2 - 4 + 17 = 21
    printf("%d\n", add(2, 22, 44));        // 22 + 44 = 66
}
```

\index{va\_start() macro@\texttt{va\_start()} macro|)}
\index{va\_end() macro@\texttt{va\_end()} macro|)}

(Perhatikan bahwa saat `printf()` dipanggil, ia memakai jumlah `%d` (atau
sejenisnya) di format string untuk mengetahui berapa banyak argumen
tambahan!)

Kalau sintaks `va_arg()` terlihat aneh bagimu (karena ada nama tipe yang
melayang bebas di dalamnya), kamu tidak sendirian. Ini diimplementasikan
dengan makro preprocessor supaya semua sihirnya bisa masuk.

## Fungsionalitas `va_list`

Apa itu variabel `va_list` yang kita pakai di atas? Ia adalah variabel
opaque^[Maksudnya, kita para developer rendahan tidak seharusnya tahu apa
isinya atau artinya. Spesifikasi tidak menentukan secara rinci apa itu.]
yang menyimpan informasi tentang argumen mana yang akan kita ambil
berikutnya dengan `va_arg()`. Lihat bagaimana kita memanggil `va_arg()`
berulang kali? Variabel `va_list` adalah placeholder yang melacak
kemajuan sejauh ini.

\index{va\_start() macro@\texttt{va\_start()} macro|(}

Tapi kita harus menginisialisasi variabel itu ke nilai yang masuk akal.
Di situlah `va_start()` berperan.

Saat memanggil `va_start(va, count)` di atas, kita berkata, "Inisialisasi
variabel `va` agar menunjuk ke argumen variabel _tepat setelah_ `count`."

\index{va\_end() macro@\texttt{va\_end()} macro|(}

Dan itulah _alasan_ kita perlu punya setidaknya satu variabel bernama di
daftar argumen^[Jujur saja, batasan itu sebenarnya bisa dihapus dari
bahasa, tapi idenya adalah makro `va_start()`, `va_arg()`, dan `va_end()`
harus bisa ditulis dalam C. Dan supaya itu terwujud, kita butuh cara untuk
menginisialisasi pointer ke lokasi parameter pertama. Dan untuk itu, kita
butuh _nama_ parameter pertama. Itu akan butuh ekstensi bahasa untuk
dimungkinkan, dan sejauh ini komite belum menemukan alasan untuk
melakukannya.].

Setelah punya pointer ke parameter awal, kamu bisa dengan mudah mengambil
nilai argumen berikutnya dengan memanggil `va_arg()` berulang kali. Saat
melakukannya, kamu harus mengoper variabel `va_list` (supaya ia bisa terus
melacak posisimu), serta tipe argumen yang akan kamu salin.

Kamu sebagai programmer yang harus menentukan tipe apa yang dioper ke
`va_arg()`. Pada contoh di atas, kita hanya memakai `int`. Tapi pada
`printf()`, ia memakai format specifier untuk menentukan tipe apa yang
diambil berikutnya.

Dan saat selesai, panggil `va_end()` untuk menutupnya. Kamu **harus**
(kata spesifikasi) memanggilnya pada variabel `va_list` tertentu sebelum
memutuskan memanggil `va_start()` atau `va_copy()` lagi padanya. Aku tahu
kita belum membahas `va_copy()`.

Jadi urutan standarnya adalah:

* `va_start()` untuk menginisialisasi variabel `va_list`
* `va_arg()` berulang kali untuk mengambil nilai-nilainya
* `va_end()` untuk men-deinisialisasi variabel `va_list`

\index{va\_start() macro@\texttt{va\_start()} macro|)}
\index{va\_end() macro@\texttt{va\_end()} macro|)}
\index{va\_arg() macro@\texttt{va\_arg()} macro|)}

\index{va\_copy() macro@\texttt{va\_copy()} macro|(}

Aku juga sudah menyebut `va_copy()` di atas; ia membuat salinan variabel
`va_list`-mu dalam keadaan yang persis sama. Artinya, kalau kamu belum
mulai dengan `va_arg()` pada variabel sumber, yang baru juga belum
dimulai. Kalau kamu sudah mengambil 5 variabel dengan `va_arg()` sejauh
ini, salinannya juga mencerminkan itu.

`va_copy()` bisa berguna kalau kamu perlu memindai argumen lebih ke depan
tapi juga perlu mengingat posisimu saat ini.

\index{va\_copy() macro@\texttt{va\_copy()} macro|)}

## Fungsi Library yang Memakai `va_list`

\index{va\_list type@\texttt{va\_list} type!passing to functions|(}

Salah satu kegunaan lain cukup keren: menulis varian `printf()` kustommu
sendiri. Repot sekali kalau harus menangani semua format specifier itu,
kan? Semiliar jumlahnya?

Untungnya, ada varian `printf()` yang menerima `va_list` yang sudah jalan
sebagai argumen. Kamu bisa memakainya untuk membungkus dan membuat
`printf()` kustommu sendiri!

\index{vprintf() function@\texttt{vprintf()} function|(}

Fungsi-fungsi ini diawali huruf `v`, seperti `vprintf()`, `vfprintf()`,
`vsprintf()`, dan `vsnprintf()`. Pada dasarnya semua `printf()` lawas
favoritmu, hanya dengan `v` di depan.

Mari kita buat fungsi `my_printf()` yang bekerja persis seperti `printf()`
kecuali ia menerima satu argumen ekstra di depan.

``` {.c .numberLines}
#include <stdio.h>
#include <stdarg.h>

int my_printf(int serial, const char *format, ...)
{
    va_list va;
    int rv;

    // Kerjakan bagian kustom
    printf("The serial number is: %d\n", serial);

    // Lalu serahkan sisanya ke vprintf()
    va_start(va, format);
    rv = vprintf(format, va);
    va_end(va);

    return rv;
}

int main(void)
{
    int x = 10;
    float y = 3.2;

    my_printf(3490, "x is %d, y is %f\n", x, y);
}
```

Lihat apa yang kita lakukan di situ? Di baris 12-14 kita memulai
variabel `va_list` baru, lalu langsung mengopernya ke `vprintf()`. Dan
`vprintf()` tahu persis apa yang harus dilakukan dengannya, karena ia
punya semua kecerdasan `printf()` bawaan.

\index{vprintf() function@\texttt{vprintf()} function|)}

Kita tetap harus memanggil `va_end()` saat selesai, jadi jangan lupa!

\index{va\_list type@\texttt{va\_list} type!passing to functions|)}
\index{va\_list type@\texttt{va\_list} type|)}

## Jebakan Makro Variadic

Seperti sudah kusebut, `va_start()` dan `va_end()` bisa berupa makro.
Salah satu konsekuensinya, mereka mungkin memperkenalkan scope lokal baru.
(Yaitu, kalau `va_start()` berisi `{` dan `va_end()` berisi `}`.)

\index{va\_start() macro@\texttt{va\_start()} macro!scoping issues|)}

Jadi kita perlu waspada terhadap kemungkinan masalah scope. Lihat contoh
berikut:

``` {.c}
va_start(va, format);          // mungkin berisi {
int rv = vprintf(format, va);
va_end(va);                    // mungkin berisi }

return rv;
```

Kalau `va_start()` membuka scope baru, `rv` akan lokal di scope itu dan
pernyataan `return` akan gagal. Tapi ini diam-diam hanya akan terjadi di
compiler yang kebetulan melakukan itu dengan makro `va`.

\index{Variadic functions|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Locale dan Internasionalisasi

\index{Locale|(}

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

Untuk pemanggilan ini, include \index{locale.h header file@\texttt{locale.h} header file} `<locale.h>`.

Pada dasarnya hanya ada satu hal yang bisa kamu lakukan secara portabel
dalam hal mendeklarasikan locale tertentu. Ini kemungkinan besar yang ingin kamu
lakukan kalau kamu akan mengerjakan apa pun terkait locale:

\index{setlocale() function@\texttt{setlocale()} function|(}

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

\index{setlocale() function@\texttt{setlocale()} function|)}

## Mengambil Pengaturan Locale Moneter

\index{Locale!money|(}

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

\index{localeconv() function@\texttt{localeconv()} function|(}

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

\index{localeconv() function@\texttt{localeconv()} function|)}

### Pengelompokan Digit Moneter {#monetary-digit-grouping}

\index{localeconv() function@\texttt{localeconv()} function!mon\_grouping@\texttt{mon\_grouping}|(}

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

\index{localeconv() function@\texttt{localeconv()} function!mon\_grouping@\texttt{mon\_grouping}|)}

### Pemisah dan Posisi Tanda

\index{localeconv() function@\texttt{localeconv()} function!sep\_by\_space@\texttt{sep\_by\_space}|(}

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

\index{localeconv() function@\texttt{localeconv()} function!sep\_by\_space@\texttt{sep\_by\_space}|)}
\index{Locale!money|)}

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

\index{setlocale() function@\texttt{setlocale()} function!LC\_ALL macro@\texttt{LC\_ALL} macro}
\index{setlocale() function@\texttt{setlocale()} function!LC\_COLLATE macro@\texttt{LC\_COLLATE} macro}
\index{setlocale() function@\texttt{setlocale()} function!LC\_CTYPE macro@\texttt{LC\_CTYPE} macro}
\index{setlocale() function@\texttt{setlocale()} function!LC\_MONETARY macro@\texttt{LC\_MONETARY} macro}
\index{setlocale() function@\texttt{setlocale()} function!LC\_NUMERIC macro@\texttt{LC\_NUMERIC} macro}
\index{setlocale() function@\texttt{setlocale()} function!LC\_TIME macro@\texttt{LC\_TIME} macro}

|Makro|Deskripsi|
|----|--------------|
|`LC_ALL`|Atur semua yang berikut ke locale yang diberikan.|
|`LC_COLLATE`|Mengatur perilaku fungsi `strcoll()` dan `strxfrm()`.|
|`LC_CTYPE`|Mengatur perilaku fungsi penanganan karakter^[Kecuali `isdigit()` dan `isxdigit()`.].|
|`LC_MONETARY`|Mengatur nilai yang dikembalikan `localeconv()`.|
|`LC_NUMERIC`|Mengatur titik desimal untuk keluarga fungsi `printf()`.|
|`LC_TIME`|Mengatur format waktu pada fungsi pencetak waktu dan tanggal `strftime()` dan `wcsftime()`.|

Cukup umum melihat \index{setlocale() function@\texttt{setlocale()} function!LC\_ALL macro@\texttt{LC\_ALL} macro}
`LC_ALL` yang diatur, tapi, setidaknya kamu punya pilihan.

Aku juga perlu menunjukkan bahwa \index{setlocale() function@\texttt{setlocale()} function!LC\_CTYPE macro@\texttt{LC\_CTYPE} macro} `LC_CTYPE` adalah salah satu yang besar karena terkait dengan wide
character, kaleng cacing yang signifikan yang akan kita bahas nanti.

\index{Locale|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Unicode, Wide Character, dan Semacamnya

\index{Unicode|(}

Sebelum mulai, perlu diketahui bahwa ini area yang masih aktif
berkembang di C, karena bahasa ini sedang berusaha melewati, ehm,
_sakit pertumbuhan_. Sekarang C23 sudah terbit, pembaruan di sini
kemungkinan akan ada.

Kebanyakan orang pada dasarnya hanya tertarik pada pertanyaan yang
tampak sederhana, "Bagaimana cara memakai character set ini-itu di C?"
Kita akan sampai ke sana. Tapi seperti yang akan kita lihat, mungkin itu
sudah berfungsi di sistemmu. Atau kamu mungkin harus menyerahkannya ke
library pihak ketiga.

Kita akan membahas banyak hal di bab ini---sebagian tidak bergantung
platform, dan sebagian khusus C.

Mari kita buat garis besarnya dulu:

* Latar belakang Unicode
* Latar belakang character encoding
* Source dan Execution Character Set
* Memakai Unicode dan UTF-8
* Memakai tipe karakter lain seperti `wchar_t`, `char16_t`, dan
  `char32_t`

Kita mulai menyelam!

## Apa Itu Unicode?

Dulu, di AS dan sebagian besar dunia, populer memakai encoding 7-bit atau
8-bit untuk karakter di memori. Artinya kita hanya bisa punya total 128
atau 256 karakter (termasuk karakter yang tidak bisa dicetak). Itu cukup
untuk dunia yang berpusat pada AS, tapi ternyata ada alfabet lain di luar
sana---siapa sangka? Bahasa Mandarin punya lebih dari 50.000 karakter, dan
itu tidak muat dalam satu byte.

Maka orang-orang menciptakan berbagai cara alternatif untuk
merepresentasikan character set kustom mereka sendiri. Dan itu tidak
masalah, tapi berubah menjadi mimpi buruk kompatibilitas.

Untuk lolos darinya, Unicode diciptakan. Satu character set untuk
menguasai semuanya. Ia memanjang sampai tak terhingga (secara efektif)
sehingga kita tidak akan pernah kehabisan ruang untuk karakter baru. Ia
punya huruf Mandarin, Latin, Yunani, aksara paku, simbol catur, emoji...
hampir semuanya, sungguh! Dan terus bertambah dari waktu ke waktu!

## Code Point

\index{Unicode!code points|(}

Aku ingin membahas dua konsep di sini. Membingungkan karena keduanya
berupa angka... angka berbeda untuk hal yang sama. Tapi sabar dulu ya.

Mari kita definisikan _code point_ secara longgar sebagai nilai numerik
yang mewakili sebuah karakter. (Code point juga bisa mewakili karakter
kontrol yang tidak bisa dicetak, tapi anggap saja yang kumaksud adalah
sesuatu seperti huruf "B" atau karakter "π".)

Setiap code point mewakili satu karakter unik. Dan setiap karakter punya
code point numerik unik yang terkait dengannya.

Misalnya, di Unicode, nilai numerik 66 mewakili "B", dan 960 mewakili
"π". Pemetaan karakter lain yang bukan Unicode mungkin memakai nilai
berbeda, tapi lupakan saja dan fokus pada Unicode, masa depan!

Jadi itu satu hal: ada angka yang mewakili setiap karakter. Di Unicode,
angka-angka ini berkisar dari 0 sampai lebih dari 1 juta.

\index{Unicode!code points|)}

Paham?

Karena kita akan sedikit membalik meja.

## Encoding

\index{Unicode!encoding|(}

Kalau kamu ingat, byte 8-bit bisa menampung nilai 0-255, inklusif. Itu
bagus untuk "B" yang bernilai 66---muat dalam satu byte. Tapi "π" bernilai
960, dan itu tidak muat dalam satu byte! Kita butuh byte lain. Bagaimana
menyimpan semua itu di memori? Atau bagaimana dengan angka yang lebih
besar, seperti 195.024? Itu butuh beberapa byte untuk menampungnya.

Pertanyaan Besarnya: bagaimana angka-angka ini direpresentasikan di
memori? Inilah yang kita sebut _encoding_ karakter.

Jadi kita punya dua hal: yang satu adalah code point yang secara efektif
memberi tahu nomor seri sebuah karakter. Dan satu lagi encoding yang
memberi tahu bagaimana kita merepresentasikan angka itu di memori.

Ada banyak encoding. Kamu bisa membuat sendiri sekarang juga, kalau
mau^[Misalnya, kita bisa menyimpan code point dalam integer 32-bit
big-endian. Mudah! Kita baru saja menciptakan sebuah encoding! Sebenarnya
tidak; itulah yang disebut encoding UTF-32BE. Ya sudahlah---kembali
bekerja!]. Tapi kita akan melihat beberapa encoding yang benar-benar umum
dan dipakai bersama Unicode.

\index{Unicode!UTF-8|(}
\index{Unicode!UTF-16|(}
\index{Unicode!UTF-32|(}

|Encoding|Deskripsi|
|:-----------------:|:--------------------------------------------------------------|
|UTF-8|Encoding berorientasi byte yang memakai jumlah byte bervariasi per karakter. Inilah yang sebaiknya dipakai.|
|UTF-16|Encoding 16-bit per karakter[^091d].|
|UTF-32|Encoding 32-bit per karakter.|

[^091d]: Kurang lebih. Secara teknis, lebarnya bervariasi---ada cara untuk
merepresentasikan code point yang lebih tinggi dari $2^{16}$ dengan
menggabungkan dua karakter UTF-16.

Pada UTF-16 dan UTF-32, urutan byte penting, jadi kamu mungkin melihat
UTF-16BE untuk big-endian dan UTF-16LE untuk little-endian. Sama untuk
UTF-32. Secara teknis, kalau tidak disebutkan, kamu harus mengasumsikan
big-endian. Tapi karena Windows memakai UTF-16 secara luas dan
little-endian, kadang itulah yang diasumsikan^[Ada karakter khusus bernama
_Byte Order Mark_ (BOM), code point 0xFEFF, yang boleh mendahului aliran
data dan menandakan endianess-nya. Tapi ini tidak wajib.].

Mari kita lihat beberapa contoh. Aku akan menulis nilainya dalam hex karena
itu tepat dua digit per byte 8-bit, dan memudahkan kita melihat bagaimana
susunannya di memori.

|Karakter|Code Point|UTF-16BE|UTF-32BE|UTF-16LE|UTF-32LE|UTF-8|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`A`|41|0041|00000041|4100|41000000|41|
|`B`|42|0042|00000042|4200|42000000|42|
|`~`|7E|007E|0000007E|7E00|7E000000|7E|
|`π`|3C0|03C0|000003C0|C003|C0030000|CF80|
|`€`|20AC|20AC|000020AC|AC20|AC200000|E282AC|

\index{Unicode!endianess|(}

Cari pola di dalamnya. Perhatikan bahwa UTF-16BE dan UTF-32BE hanyalah
code point yang direpresentasikan langsung sebagai nilai 16- dan
32-bit^[Sekali lagi, ini hanya benar di UTF-16 untuk karakter yang muat
dalam dua byte.].

\index{Unicode!UTF-16|)}
\index{Unicode!UTF-32|)}

Little-endian sama saja, hanya saja urutan byte-nya little-endian.

\index{Unicode!endianess|)}

Lalu ada UTF-8 di bagian akhir. Pertama, mungkin kamu memperhatikan bahwa
code point satu byte direpresentasikan sebagai satu byte. Itu bagus. Kamu
mungkin juga memperhatikan bahwa code point yang berbeda memakai jumlah
byte yang berbeda. Inilah encoding dengan lebar bervariasi.

Jadi begitu kita melewati nilai tertentu, UTF-8 mulai memakai byte
tambahan untuk menyimpan nilainya. Dan byte-byte itu tampaknya tidak
berkorelasi dengan nilai code point.

[Detail encoding UTF-8](https://en.wikipedia.org/wiki/UTF-8)^[https://en.wikipedia.org/wiki/UTF-8] di luar cakupan panduan ini, tapi cukup
diketahui bahwa ia memakai jumlah byte yang bervariasi per code point, dan
nilai byte itu tidak cocok dengan code point _kecuali untuk 128 code point
pertama_. Kalau kamu benar-benar ingin belajar lebih jauh, [Computerphile
punya video UTF-8 yang bagus bersama Tom
Scott](https://www.youtube.com/watch?v=MijmeoH9LT4)^[https://www.youtube.com/watch?v=MijmeoH9LT4].

Soal terakhir itu hal yang keren tentang Unicode dan UTF-8 dari sudut
pandang Amerika Utara: ia kompatibel mundur dengan encoding ASCII 7-bit!
Jadi kalau kamu terbiasa dengan ASCII, UTF-8 sama saja! Setiap dokumen
ber-encoding ASCII juga ber-encoding UTF-8! (Tapi tidak sebaliknya, tentu
saja.)

Kemungkinan poin terakhir itulah, lebih dari yang lain, yang mendorong
UTF-8 menguasai dunia.

\index{Unicode!UTF-8|)}
\index{Unicode!encoding|)}

## Source dan Execution Character Set {#src-exec-charset}

\index{Character sets|(}

Saat memprogram di C, ada (setidaknya) tiga character set yang berperan:

* Yang dipakai kodemu saat tersimpan di disk.
* \index{Character sets!source}Yang diterjemahkan compiler dari itu tepat
  saat kompilasi dimulai (_source character set_). Ini mungkin sama
  dengan yang di disk, mungkin juga tidak.
* \index{Character sets!execution}Yang menjadi tujuan terjemahan source
  character set oleh compiler untuk eksekusi (_execution character set_).
  Ini mungkin sama dengan source character set, mungkin juga tidak.

Compiler-mu kemungkinan punya opsi untuk memilih character set ini saat
build.

\index{Character sets!basic|(}

Basic character set untuk source maupun execution berisi karakter-karakter
berikut:

``` {.default}
A B C D E F G H I J K L M
N O P Q R S T U V W X Y Z
a b c d e f g h i j k l m
n o p q r s t u v w x y z
0 1 2 3 4 5 6 7 8 9
! " # % & ' ( ) * + , - . / :
; < = > ? [ \ ] ^ _ { | } ~
space tab vertical-tab
form-feed end-of-line
```

Itulah karakter yang bisa kamu pakai di source dan tetap 100% portabel.

Execution character set akan punya tambahan karakter untuk alert
(bell/flash), backspace, carriage return, dan newline.

Tapi kebanyakan orang tidak sampai seekstrem itu dan dengan bebas memakai
extended character set di source dan executable, terutama sekarang ketika
Unicode dan UTF-8 makin umum. Maksudnya, basic character set bahkan tidak
mengizinkan `@`, `$`, atau `` ` ``!

Perlu dicatat, menulis karakter Unicode hanya dengan basic character
set itu merepotkan (walaupun bisa dengan escape sequence).

\index{Character sets!basic|)}
\index{Character sets|)}

## Unicode di C {#unicode-in-c}

Sebelum membahas encoding di C, mari kita bahas Unicode dari sudut pandang
code point dulu. Ada cara di C untuk menentukan karakter Unicode dan
karakter-karakter ini akan diterjemahkan compiler ke execution character
set^[Mungkin compiler berusaha sebaik mungkin menerjemahkan code point ke
encoding output apa pun itu, tapi aku tidak menemukan jaminannya di
spesifikasi.].

Jadi bagaimana caranya?

Bagaimana dengan simbol euro, code point 0x20AC. (Ditulis dalam hex karena
kedua cara merepresentasikannya di C membutuhkan hex.) Bagaimana kita bisa
memasukkannya ke kode C kita?

\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|(}
Pakai escape `\u` untuk memasukkannya ke string, mis. `"\u20AC"` (huruf
besar/kecil pada hex tidak berpengaruh). Kamu harus menaruh **tepat empat**
digit hex setelah `\u`, dengan nol di depan kalau perlu.

Berikut contohnya:

``` {.c}
char *s = "\u20AC1.23";

printf("%s\n", s);  // €1.23
```

\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|(}

Jadi `\u` berfungsi untuk code point Unicode 16-bit, tapi bagaimana dengan
yang lebih besar dari 16 bit? Untuk itu kita butuh huruf kapital: `\U`.

Misalnya:

``` {.c}
char *s = "\U0001D4D1";

printf("%s\n", s);  // Mencetak huruf matematika "B"
```

Sama seperti `\u`, hanya dengan 32 bit alih-alih 16. Keduanya setara:

``` {.c}
\u03C0
\U000003C0
```

\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|)}
\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|)}

Sekali lagi, ini diterjemahkan ke \index{Character sets!execution}execution character set saat kompilasi. Mereka
merepresentasikan code point Unicode, bukan encoding tertentu. Selain itu,
kalau sebuah code point Unicode tidak bisa direpresentasikan di execution
character set, compiler boleh melakukan apa saja terhadapnya.

Nah, mungkin kamu bertanya kenapa tidak langsung begini:

``` {.c}
char *s = "€1.23";

printf("%s\n", s);  // €1.23
```

Dan kemungkinan besar bisa, dengan compiler modern. \index{Character sets!source}Source character set akan diterjemahkan oleh compiler ke
\index{Character sets!execution}execution character set untukmu. Tapi
compiler bebas muntah kalau menemukan karakter yang tidak termasuk dalam
extended character set-nya, dan simbol € jelas tidak ada di \index{Character sets!basic}basic character set.

\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|(}
\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|(}

Peringatan dari spesifikasi: kamu tidak boleh memakai `\u` atau `\U` untuk
meng-encode code point di bawah 0xA0 kecuali 0x24 (`$`), 0x40 (`@`), dan
0x60 (`` `
``)---ya, itulah tiga serangkai tanda baca umum yang tidak ada di basic
character set. Tampaknya pembatasan ini dilonggarkan di versi spesifikasi
yang akan datang.

\index{{\backslash}u Unicode escape@\texttt{{\backslash}u} Unicode escape|)}
\index{{\backslash}U Unicode escape@\texttt{{\backslash}U} Unicode escape|)}

Terakhir, kamu juga bisa memakai ini di identifier dalam kodemu, dengan
beberapa batasan. Tapi aku tidak ingin membahasnya di sini. Bab ini
fokus pada penanganan string.

Dan itu saja soal Unicode di C (kecuali encoding).

## Catatan Singkat tentang UTF-8 Sebelum Kita Terjun ke Semak-Semak {#utf8-quick}

\index{Unicode!UTF-8|(}

Bisa jadi file source di disk, extended source character, dan extended
execution character-mu semuanya berformat UTF-8. Dan library yang kamu
pakai mengharapkan UTF-8. Inilah masa depan gemilang dengan UTF-8 di
mana-mana.

Kalau begitu, dan kamu tidak keberatan tidak portabel ke sistem yang tidak
seperti itu, jalankan saja. Taruh karakter Unicode di source dan datamu
sesukamu. Pakai string C biasa dan berbahagialah.

Banyak hal akan langsung berfungsi (walaupun tidak portabel) karena string
UTF-8 aman diakhiri NUL seperti string C lainnya. Tapi mungkin kehilangan
portabilitas demi penanganan karakter yang lebih mudah adalah pertukaran
yang sepadan bagimu.

Meski begitu, ada beberapa peringatan:

* Fungsi seperti `strlen()` melaporkan jumlah byte dalam string, belum
  tentu jumlah karakternya. (Fungsi \index{mbstowcs() function@\texttt{mbstowcs()} function!with UTF-8}`mbstowcs()` mengembalikan jumlah karakter dalam
  string ketika kamu mengonversinya menjadi wide character. POSIX
  memperluas ini sehingga kamu bisa mengoper `NULL` sebagai argumen
  pertama kalau hanya ingin jumlah karakter.)

* Yang berikut tidak akan bekerja dengan benar pada karakter lebih dari
  satu byte: \index{strtok() function@\texttt{strtok()} function!with UTF-8}`strtok()`, \index{strchr() function@\texttt{strchr()} function!with UTF-8}`strchr()` (pakai \index{strstr() function@\texttt{strstr()} function!with UTF-8}`strstr()` sebagai gantinya), fungsi tipe `strspn()`,
  \index{toupper() function@\texttt{toupper()} function!with UTF-8}`toupper()`, \index{tolower() function@\texttt{tolower()} function!with UTF-8}`tolower()`, fungsi tipe \index{isalpha() function@\texttt{isalpha()} function!with UTF-8}`isalpha()`, dan mungkin lebih banyak lagi.
  Waspadai apa pun yang beroperasi pada byte.

* Varian \index{printf() function@\texttt{printf()} function!with UTF-8}`printf()` menyediakan cara
  untuk hanya mencetak sebagian byte dari sebuah string^[Dengan format
  specifier seperti `"%.12s"`, misalnya.]. Pastikan kamu mencetak jumlah
  byte yang benar agar berakhir di batas karakter.

* \index{malloc() function@\texttt{malloc()} function!with UTF-8}Kalau kamu ingin `malloc()` ruang
  untuk sebuah string, atau mendeklarasikan array `char` untuknya,
  ketahuilah bahwa ukuran maksimumnya bisa lebih besar dari yang kamu
  duga. Setiap karakter bisa memakai sampai \index{MB\_LEN\_MAX macro@\texttt{MB\_LEN\_MAX} macro}`MB_LEN_MAX` byte (dari `<limits.h>`)---kecuali karakter di
  basic character set yang dijamin satu byte.

Dan mungkin masih ada yang lain yang belum kutemukan. Kabari
aku kalau ada jebakan lain di luar sana...

\index{Unicode!UTF-8|)}

## Berbagai Tipe Karakter

Aku ingin memperkenalkan lebih banyak tipe karakter. Kita sudah terbiasa
dengan `char`, kan?

Tapi itu terlalu mudah. Mari kita buat semuanya jauh lebih sulit! Hore!

### Karakter Multibyte

\index{Multibyte characters|(}

Pertama-tama, aku ingin mengubah cara berpikirmu tentang apa itu
string (array `char`). Ini adalah _string multibyte_ yang terdiri dari
_karakter multibyte_.

Betul---string karakter biasa yang kamu kenal itu multibyte. Ketika
seseorang bilang "string C", maksudnya "string multibyte C".

Bahkan kalau sebuah karakter dalam string hanya satu byte, atau kalau
sebuah string hanya terdiri dari karakter tunggal, ia tetap dikenal
sebagai string multibyte.

Misalnya:

``` {.c}
char c[128] = "Hello, world!";  // String multibyte
```

Maksudnya, sebuah karakter yang tidak ada di basic character set bisa
tersusun dari beberapa byte. Sampai sebanyak \index{MB\_LEN\_MAX macro@\texttt{MB\_LEN\_MAX} macro}`MB_LEN_MAX` (dari `<limits.h>`). Memang di layar hanya tampak
seperti satu karakter, tapi ia bisa berupa beberapa byte.

Nilai Unicode juga bisa dimasukkan ke sana, seperti yang kita lihat tadi:

``` {.c}
char *s = "\u20AC1.23";

printf("%s\n", s);  // €1.23
```

Tapi di sini kita mulai masuk ke keanehan, karena lihat ini:

\index{strlen() function@\texttt{strlen()} function!with UTF-8|(}

``` {.c}
char *s = "\u20AC1.23";  // €1.23

printf("%zu\n", strlen(s));  // 7!
```

Panjang string `"€1.23"` adalah `7`?! Ya! Setidaknya di sistemku, ya!
Ingat bahwa `strlen()` mengembalikan jumlah byte dalam string, bukan
jumlah karakter. (Saat membahas "wide character" nanti, kita akan melihat
cara mendapat jumlah karakter dalam string.)

\index{strlen() function@\texttt{strlen()} function!with UTF-8|)}

Perhatikan bahwa walaupun C mengizinkan konstanta `char` multibyte
individual (berbeda dari `char*`), perilakunya bervariasi menurut
implementasi dan compiler-mu mungkin memperingatkannya.

GCC, misalnya, memperingatkan soal multi-character character constant
pada dua baris berikut (dan, di sistemku, mencetak encoding
UTF-8-nya):

``` {.c}
printf("%x\n", '€');
printf("%x\n", '\u20ac');
```

\index{Multibyte characters|)}

### Wide Character {#wide-characters}

\index{Wide characters|(}

Kalau bukan karakter multibyte, berarti kamu _wide character_.

Wide character adalah satu nilai yang bisa mewakili secara unik karakter
apa pun di locale saat ini. Analog dengan code point Unicode. Tapi bisa
jadi bukan. Atau bisa jadi iya.

Pada dasarnya, kalau string karakter multibyte adalah array byte, string
wide character adalah array _karakter_. Jadi kamu bisa mulai berpikir
per karakter, bukan per byte (yang terakhir jadi berantakan ketika
karakter mulai memakai jumlah byte yang bervariasi).

\index{wchar\_t type@\texttt{wchar\_t} type|(}

Wide character bisa direpresentasikan oleh sejumlah tipe, tapi yang paling
menonjol adalah `wchar_t`. Ia yang utama. Mirip `char`, hanya saja lebar.

Mungkin kamu bertanya-tanya, kalau kita tidak bisa tahu apakah itu Unicode
atau bukan, bagaimana itu memberi banyak fleksibilitas dalam menulis kode?
`wchar_t` membuka sebagian pintu itu, karena ada sekumpulan fungsi lengkap
yang bisa dipakai untuk menangani string `wchar_t` (seperti mengambil
panjang, dan sebagainya) tanpa peduli encoding-nya.

## Memakai Wide Character dan `wchar_t`

Saatnya tipe baru: `wchar_t`. Ini tipe wide character utama. Ingat bahwa
`char` hanya satu byte? Dan satu byte mungkin tidak cukup untuk
merepresentasikan semua karakter? Nah, yang ini cukup.

Untuk memakai `wchar_t`, include `<wchar.h>`.

Berapa byte ukurannya? Tidak sepenuhnya jelas. Bisa 16 bit. Bisa 32 bit.

Tapi tunggu, katamu---kalau hanya 16 bit, ia tidak cukup besar untuk
menampung semua code point Unicode, kan? Benar---memang tidak. Spesifikasi
tidak mewajibkannya. Ia hanya harus bisa merepresentasikan semua karakter
di locale saat ini.

Ini bisa menyebabkan masalah dengan Unicode pada platform yang `wchar_t`-nya
16-bit (ehem---Windows). Tapi itu di luar cakupan panduan ini.

\index{L wide character prefix@\texttt{L} wide character prefix|(}

Kamu bisa mendeklarasikan string atau karakter bertipe ini dengan awalan
`L`, dan mencetaknya dengan format specifier `%ls` ("ell ess"). Atau
mencetak satu `wchar_t` dengan `%lc`.

``` {.c}
wchar_t *s = L"Hello, world!";
wchar_t c = L'B';

printf("%ls %lc\n", s, c);
```

\index{L wide character prefix@\texttt{L} wide character prefix|)}

Nah---apakah karakter-karakter itu disimpan sebagai code point Unicode
atau tidak? Tergantung implementasi. Tapi kamu bisa mengujinya dengan makro
\index{\_\_STDC\_ISO\_10646\_\_ macro@\texttt{\_\_STDC\_ISO\_10646\_\_} macro} `__STDC_ISO_10646__`. Kalau makro ini
terdefinisi, jawabannya, "Itu Unicode!"

Lebih rinci, nilai di makro itu adalah integer berbentuk `yyyymm` yang
memberi tahu standar Unicode mana yang bisa kamu andalkan---yang berlaku
pada tanggal itu.

Tapi bagaimana cara memakainya?

### Konversi Multibyte ke `wchar_t`

Jadi bagaimana kita berpindah dari string standar berorientasi byte ke
string lebar berorientasi karakter dan sebaliknya?

Kita bisa memakai beberapa fungsi konversi string untuk mewujudkannya.

Pertama, beberapa konvensi penamaan yang akan kamu lihat pada fungsi-fungsi
ini:

* `mb`: multibyte
* `wc`: wide character
* `mbs`: string multibyte
* `wcs`: string wide character

Jadi kalau kita ingin mengonversi string multibyte menjadi string wide
character, kita bisa memanggil `mbstowcs()`. Dan sebaliknya: `wcstombs()`.

\index{mbtowc() function@\texttt{mbtowc()} function}
\index{wctomb() function@\texttt{wctomb()} function}
\index{mbstowcs() function@\texttt{mbstowcs()} function}
\index{wcstombs() function@\texttt{wcstombs()} function}

|Fungsi Konversi|Deskripsi|
|------------|---------------------------------------------------|
|`mbtowc()`|Mengonversi karakter multibyte menjadi wide character.|
|`wctomb()`|Mengonversi wide character menjadi karakter multibyte.|
|`mbstowcs()`|Mengonversi string multibyte menjadi wide string.|
|`wcstombs()`|Mengonversi wide string menjadi string multibyte.|

Mari kita buat demo singkat yang mengonversi string multibyte menjadi
string wide character, lalu membandingkan panjang kedua string itu dengan
fungsi masing-masing.

\index{mbstowcs() function@\texttt{mbstowcs()} function|(}

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>
#include <string.h>
#include <locale.h>

int main(void)
{
    // Keluar dari locale C ke locale yang kemungkinan punya simbol euro
    setlocale(LC_ALL, "");

    // String multibyte asli dengan simbol euro (code point Unicode 20ac)
    char *mb_string = "The cost is \u20ac1.23";  // €1.23
    size_t mb_len = strlen(mb_string);

    // Array wide character yang akan menampung string hasil konversi
    wchar_t wc_string[128];  // Menampung sampai 128 wide character

    // Konversi string MB ke WC; ini mengembalikan jumlah wide char
    size_t wc_len = mbstowcs(wc_string, mb_string, 128);

    // Cetak hasil--perhatikan %ls untuk string wide char
    printf("multibyte: \"%s\" (%zu bytes)\n", mb_string, mb_len);
    printf("wide char: \"%ls\" (%zu characters)\n", wc_string, wc_len);
}
```

\index{wchar\_t type@\texttt{wchar\_t} type|)}

Di sistemku, ini mengeluarkan:

``` {.default}
multibyte: "The cost is €1.23" (19 bytes)
wide char: "The cost is €1.23" (17 characters)
```

(Jumlah byte di sistemmu mungkin berbeda tergantung locale.)

Satu hal menarik: `mbstowcs()`, selain mengonversi string multibyte
menjadi wide, mengembalikan panjang (dalam karakter) string wide
character itu. Pada sistem yang patuh POSIX, kamu bisa memanfaatkan mode
khusus ketika ia _hanya_ mengembalikan panjang-dalam-karakter dari
string multibyte tertentu: cukup oper `NULL` untuk tujuan, dan `0` untuk
jumlah karakter maksimum yang dikonversi (nilai ini diabaikan).

(Pada kode di bawah, aku memakai extended source character
set milikku---kamu mungkin harus menggantinya dengan escape `\u`.)

``` {.c}
setlocale(LC_ALL, "");

// String berikut punya 7 karakter
size_t len_in_chars = mbstowcs(NULL, "§¶°±π€•", 0);

printf("%zu", len_in_chars);  // 7
```

\index{mbstowcs() function@\texttt{mbstowcs()} function|)}

Sekali lagi, itu ekstensi POSIX yang tidak portabel.

Dan, tentu saja, kalau ingin mengonversi ke arah sebaliknya, pakai
\index{wcstombs() function@\texttt{wcstombs()} function} `wcstombs()`.

## Fungsionalitas Wide Character

Setelah masuk ke wilayah wide character, kita punya berbagai fungsionalitas
yang bisa dipakai. Aku hanya akan meringkas sejumlah fungsi di sini,
tapi pada dasarnya yang ada adalah versi wide character dari fungsi string
multibyte yang sudah kita kenal. (Misalnya, kita kenal `strlen()` untuk
string multibyte; ada \index{wcslen() function@\texttt{wcslen()} function} `wcslen()` untuk string wide
character.)

### `wint_t`

\index{wint\_t type@\texttt{wint\_t} type|(}

Banyak fungsi ini memakai `wint_t` untuk menyimpan karakter tunggal, baik
yang dioper maupun yang dikembalikan.

Secara sifat ia terkait dengan `wchar_t`. `wint_t` adalah integer yang bisa
merepresentasikan semua nilai di extended character set, dan juga karakter
end-of-file khusus, `WEOF`.

\index{wint\_t type@\texttt{wint\_t} type|)}

Ini dipakai oleh sejumlah fungsi wide character yang berorientasi satu
karakter.

### Orientasi I/O Stream {#io-stream-orientation}

\index{I/O stream orientation|(}

Ringkasnya: jangan mencampur fungsi berorientasi byte (seperti
`fprintf()`) dengan fungsi berorientasi wide (seperti `fwprintf()`).
Putuskan apakah sebuah stream akan berorientasi byte atau wide dan
tetaplah memakai jenis fungsi I/O itu.

Lebih rinci: stream bisa berorientasi byte atau wide. Saat sebuah stream
pertama kali dibuat, ia belum punya orientasi, tapi pembacaan atau
penulisan pertama akan menetapkan orientasinya.

Kalau kamu pertama kali memakai operasi wide (seperti `fwprintf()`), stream
itu berorientasi wide.

Kalau kamu pertama kali memakai operasi byte (seperti `fprintf()`), stream
itu berorientasi byte.

Kamu bisa mengatur secara manual stream yang belum berorientasi ke satu
arah atau yang lain dengan memanggil \index{fwide() function@\texttt{fwide()} function} `fwide()`.
Fungsi yang sama bisa dipakai untuk mengetahui orientasi sebuah stream.

Kalau perlu mengubah orientasi di tengah jalan, bisa dilakukan dengan
`freopen()`.

\index{I/O stream orientation|)}

### Fungsi I/O

Biasanya include `<stdio.h>` dan `<wchar.h>` untuk ini.

\index{wprintf() function@\texttt{wprintf()} function}
\index{wscanf() function@\texttt{wscanf()} function}
\index{getwchar() function@\texttt{getwchar()} function}
\index{putwchar() function@\texttt{putwchar()} function}
\index{fwprintf() function@\texttt{fwprintf()} function}
\index{fwscanf() function@\texttt{fwscanf()} function}
\index{fgetwc() function@\texttt{fgetwc()} function}
\index{fputwc() function@\texttt{fputwc()} function}
\index{fgetws() function@\texttt{fgetws()} function}
\index{fputws() function@\texttt{fputws()} function}
\index{swprintf() function@\texttt{swprintf()} function}
\index{swscanf() function@\texttt{swscanf()} function}
\index{vfwprintf() function@\texttt{vfwprintf()} function}
\index{vfwscanf() function@\texttt{vfwscanf()} function}
\index{vswprintf() function@\texttt{vswprintf()} function}
\index{vswscanf() function@\texttt{vswscanf()} function}
\index{vwprintf() function@\texttt{vwprintf()} function}
\index{vwscanf() function@\texttt{vwscanf()} function}
\index{ungetwc() function@\texttt{ungetwc()} function}
\index{fwide() function@\texttt{fwide()} function}

|Fungsi I/O|Deskripsi|
|------------|---------------------------------------------------|
|`wprintf()`|Output terformat ke konsol.|
|`wscanf()`|Input terformat dari konsol.|
|`getwchar()`|Input konsol berbasis karakter.|
|`putwchar()`|Output konsol berbasis karakter.|
|`fwprintf()`|Output terformat ke file.|
|`fwscanf()`|Input terformat dari file.|
|`fgetwc()`|Input file berbasis karakter.|
|`fputwc()`|Output file berbasis karakter.|
|`fgetws()`|Input file berbasis string.|
|`fputws()`|Output file berbasis string.|
|`swprintf()`|Output terformat ke string.|
|`swscanf()`|Input terformat dari string.|
|`vfwprintf()`|Output terformat variadic ke file.|
|`vfwscanf()`|Input terformat variadic dari file.|
|`vswprintf()`|Output terformat variadic ke string.|
|`vswscanf()`|Input terformat variadic dari string.|
|`vwprintf()`|Output terformat variadic ke konsol.|
|`vwscanf()`|Input terformat variadic dari konsol.|
|`ungetwc()`|Mengembalikan sebuah wide character ke stream.|
|`fwide()`|Mengambil atau mengatur orientasi multibyte/wide sebuah stream.|

### Fungsi Konversi Tipe

Biasanya include `<wchar.h>` untuk ini.

\index{wcstod() function@\texttt{wcstod()} function}
\index{wcstof() function@\texttt{wcstof()} function}
\index{wcstold() function@\texttt{wcstold()} function}
\index{wcstol() function@\texttt{wcstol()} function}
\index{wcstoll() function@\texttt{wcstoll()} function}
\index{wcstoul() function@\texttt{wcstoul()} function}
\index{wcstoull() function@\texttt{wcstoull()} function}

|Fungsi Konversi|Deskripsi|
|------------|---------------------------------------------------|
|`wcstod()`|Mengonversi string menjadi `double`.|
|`wcstof()`|Mengonversi string menjadi `float`.|
|`wcstold()`|Mengonversi string menjadi `long double`.|
|`wcstol()`|Mengonversi string menjadi `long`.|
|`wcstoll()`|Mengonversi string menjadi `long long`.|
|`wcstoul()`|Mengonversi string menjadi `unsigned long`.|
|`wcstoull()`|Mengonversi string menjadi `unsigned long long`.|

### Fungsi Penyalinan String dan Memori

Biasanya include `<wchar.h>` untuk ini.

\index{wcscpy() function@\texttt{wcscpy()} function}
\index{wcsncpy() function@\texttt{wcsncpy()} function}
\index{wmemcpy() function@\texttt{wmemcpy()} function}
\index{wmemmove() function@\texttt{wmemmove()} function}
\index{wcscat() function@\texttt{wcscat()} function}
\index{wcsncat() function@\texttt{wcsncat()} function}

|Fungsi Penyalinan|Deskripsi|
|----|----------------------------------------------|
|`wcscpy()`|Menyalin string.|
|`wcsncpy()`|Menyalin string, dibatasi panjang.|
|`wmemcpy()`|Menyalin memori.|
|`wmemmove()`|Menyalin memori yang mungkin tumpang tindih.|
|`wcscat()`|Menyambung string.|
|`wcsncat()`|Menyambung string, dibatasi panjang.|

### Fungsi Perbandingan String dan Memori

Biasanya include `<wchar.h>` untuk ini.

\index{wcscmp() function@\texttt{wcscmp()} function}
\index{wcsncmp() function@\texttt{wcsncmp()} function}
\index{wcscoll() function@\texttt{wcscoll()} function}
\index{wmemcmp() function@\texttt{wmemcmp()} function}
\index{wcsxfrm() function@\texttt{wcsxfrm()} function}

|Fungsi Perbandingan|Deskripsi|
|-------------------|---------------------------------------------------------------|
|`wcscmp()`|Membandingkan string secara leksikografis.|
|`wcsncmp()`|Membandingkan string secara leksikografis, dibatasi panjang.|
|`wcscoll()`|Membandingkan string menurut urutan kamus berdasarkan locale.|
|`wmemcmp()`|Membandingkan memori secara leksikografis.|
|`wcsxfrm()`|Mengubah string menjadi versi yang membuat `wcscmp()` berperilaku seperti `wcscoll()`[^97d0].|

[^97d0]: `wcscoll()` sama dengan `wcsxfrm()` diikuti `wcscmp()`.

### Fungsi Pencarian String

Biasanya include `<wchar.h>` untuk ini.

\index{wcschr() function@\texttt{wcschr()} function}
\index{wcsrchr() function@\texttt{wcsrchr()} function}
\index{wmemchr() function@\texttt{wmemchr()} function}
\index{wcsstr() function@\texttt{wcsstr()} function}
\index{wcspbrk() function@\texttt{wcspbrk()} function}
\index{wcsspn() function@\texttt{wcsspn()} function}
\index{wcscspn() function@\texttt{wcscspn()} function}
\index{wcstok() function@\texttt{wcstok()} function}

|Fungsi Pencarian|Deskripsi|
|-|-|
|`wcschr()`|Mencari karakter dalam string.|
|`wcsrchr()`|Mencari karakter dalam string dari belakang.|
|`wmemchr()`|Mencari karakter di memori.|
|`wcsstr()`|Mencari substring dalam string.|
|`wcspbrk()`|Mencari salah satu dari sekumpulan karakter dalam string.|
|`wcsspn()`|Mencari panjang substring yang berisi salah satu dari sekumpulan karakter.|
|`wcscspn()`|Mencari panjang substring sebelum salah satu dari sekumpulan karakter.|
|`wcstok()`|Mencari token dalam string.|

### Fungsi Panjang/Lain-lain

Biasanya include `<wchar.h>` untuk ini.

\index{wcslen() function@\texttt{wcslen()} function}
\index{wmemset() function@\texttt{wmemset()} function}
\index{wcsftime() function@\texttt{wcsftime()} function}

|Fungsi Panjang/Lain-lain|Deskripsi|
|-|-|
|`wcslen()`|Mengembalikan panjang string.|
|`wmemset()`|Mengisi karakter di memori.|
|`wcsftime()`|Output tanggal dan waktu terformat.|

### Fungsi Klasifikasi Karakter

Include `<wctype.h>` untuk ini.

\index{iswalnum() function@\texttt{iswalnum()} function}
\index{iswalpha() function@\texttt{iswalpha()} function}
\index{iswblank() function@\texttt{iswblank()} function}
\index{iswcntrl() function@\texttt{iswcntrl()} function}
\index{iswdigit() function@\texttt{iswdigit()} function}
\index{iswgraph() function@\texttt{iswgraph()} function}
\index{iswlower() function@\texttt{iswlower()} function}
\index{iswprint() function@\texttt{iswprint()} function}
\index{iswpunct() function@\texttt{iswpunct()} function}
\index{iswspace() function@\texttt{iswspace()} function}
\index{iswupper() function@\texttt{iswupper()} function}
\index{iswxdigit() function@\texttt{iswxdigit()} function}
\index{towlower() function@\texttt{towlower()} function}
\index{towupper() function@\texttt{towupper()} function}

|Fungsi Panjang/Lain-lain|Deskripsi|
|------------|---------------------------------------------------|
|`iswalnum()`|True jika karakter alfanumerik.|
|`iswalpha()`|True jika karakter berupa huruf.|
|`iswblank()`|True jika karakter kosong (mirip spasi, tapi bukan newline).|
|`iswcntrl()`|True jika karakter adalah karakter kontrol.|
|`iswdigit()`|True jika karakter berupa digit.|
|`iswgraph()`|True jika karakter bisa dicetak (kecuali spasi).|
|`iswlower()`|True jika karakter huruf kecil.|
|`iswprint()`|True jika karakter bisa dicetak (termasuk spasi).|
|`iswpunct()`|True jika karakter berupa tanda baca.|
|`iswspace()`|True jika karakter berupa whitespace.|
|`iswupper()`|True jika karakter huruf besar.|
|`iswxdigit()`|True jika karakter berupa digit hex.|
|`towlower()`|Mengonversi karakter menjadi huruf kecil.|
|`towupper()`|Mengonversi karakter menjadi huruf besar.|

## Parse State, Fungsi Restartable

\index{Multibyte characters!parse state|(}

Kita akan masuk sedikit ke jeroan konversi multibyte, tapi ini hal yang
bagus untuk dipahami secara konseptual.

Bayangkan bagaimana programmu mengambil deretan karakter multibyte dan
mengubahnya menjadi wide character, atau sebaliknya. Pada suatu titik, ia
mungkin sedang di tengah mem-parse sebuah karakter, atau mungkin harus
menunggu byte tambahan sebelum menentukan nilai akhirnya.

Parse state ini disimpan dalam variabel opaque bertipe `mbstate_t` dan
dipakai setiap kali konversi dilakukan. Begitulah cara fungsi konversi
melacak posisinya di tengah pekerjaan.

Dan kalau kamu berpindah ke deretan karakter lain di tengah stream, atau
mencoba seek ke tempat lain di deretan inputmu, ia bisa bingung karenanya.

Mungkin kamu ingin menegurku soal ini: tadi kita sudah melakukan beberapa
konversi, dan aku tidak pernah menyebut `mbstate_t` di mana pun.

Itu karena fungsi konversi seperti `mbstowcs()`, `wctomb()`, dan
sebagainya masing-masing punya variabel `mbstate_t` sendiri yang mereka
pakai. Tapi hanya ada satu per fungsi, jadi kalau kamu menulis kode
multithread, fungsi-fungsi itu tidak aman dipakai.

Untungnya, C mendefinisikan versi _restartable_ dari fungsi-fungsi ini
yang memungkinkan kamu mengoper `mbstate_t`-mu sendiri per thread kalau perlu.
Kalau kamu bekerja dengan multithread, pakai ini!

Catatan singkat tentang menginisialisasi variabel `mbstate_t`: cukup
`memset()` ke nol. Tidak ada fungsi bawaan untuk memaksa inisialisasi.

``` {.c}
mbstate_t mbs;

// Atur state ke state awal
memset(&mbs, 0, sizeof mbs);
```

Berikut daftar fungsi konversi restartable---perhatikan konvensi
penamaan dengan menaruh "`r`" setelah tipe "asal":

* `mbrtowc()`---multibyte ke wide character
* `wcrtomb()`---wide character ke multibyte
* `mbsrtowcs()`---string multibyte ke string wide character
* `wcsrtombs()`---string wide character ke string multibyte

Fungsi-fungsi ini sangat mirip dengan padanannya yang non-restartable,
hanya saja mereka mewajibkan kamu mengoper pointer ke variabel `mbstate_t`
milikmu sendiri. Mereka juga mengubah pointer string sumber (untuk
membantu kalau ditemukan byte yang tidak valid), jadi mungkin berguna
menyimpan salinan aslinya.

Berikut contoh dari awal bab yang disusun ulang agar mengoper `mbstate_t`
kita sendiri.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
#include <wchar.h>
#include <string.h>
#include <locale.h>

int main(void)
{
    // Keluar dari locale C ke locale yang kemungkinan punya simbol euro
    setlocale(LC_ALL, "");

    // String multibyte asli dengan simbol euro (code point Unicode 20ac)
    char *mb_string = "The cost is \u20ac1.23";  // €1.23
    size_t mb_len = strlen(mb_string);

    // Array wide character yang akan menampung string hasil konversi
    wchar_t wc_string[128];  // Menampung sampai 128 wide character

    // Siapkan state konversi
    mbstate_t mbs;
    memset(&mbs, 0, sizeof mbs);  // State awal

    // mbsrtowcs() mengubah pointer input agar menunjuk ke karakter tidak
    // valid pertama, atau NULL jika berhasil. Kita buat salinan pointer
    // untuk diutak-atik mbsrtowcs() sehingga aslinya tidak berubah.
    //
    // Contoh ini kemungkinan berhasil, tapi kita periksa lebih ke bawah
    // untuk memastikan.
    const char *invalid = mb_string;

    // Konversi string MB ke WC; ini mengembalikan jumlah wide char
    size_t wc_len = mbsrtowcs(wc_string, &invalid, 128, &mbs);

    if (invalid == NULL) {
        printf("No invalid characters found\n");

        // Cetak hasil--perhatikan %ls untuk string wide char
        printf("multibyte: \"%s\" (%zu bytes)\n", mb_string, mb_len);
        printf("wide char: \"%ls\" (%zu characters)\n", wc_string, wc_len);
    } else {
        ptrdiff_t offset = invalid - mb_string;
        printf("Invalid character at offset %td\n", offset);
    }
}
```

Untuk fungsi konversi yang mengelola state-nya sendiri, kamu bisa mereset
state internalnya ke state awal dengan mengoper `NULL` untuk argumen
`char*`-nya, misalnya:

``` {.c}
mbstowcs(NULL, NULL, 0);   // Reset parse state untuk mbstowcs()
mbstowcs(dest, src, 100);  // Parse beberapa hal
```

Untuk I/O, setiap wide stream mengelola `mbstate_t`-nya sendiri dan
memakainya untuk konversi input dan output selama berjalan.

Dan beberapa fungsi I/O berorientasi byte seperti `printf()` dan
`scanf()` menyimpan state internalnya sendiri saat bekerja.

Terakhir, fungsi konversi restartable ini sebenarnya punya state internal
sendiri kalau kamu mengoper `NULL` sebagai parameter `mbstate_t`. Ini
membuat mereka berperilaku lebih mirip padanannya yang non-restartable.

\index{Multibyte characters!parse state|)}
\index{Wide characters|)}

## Encoding Unicode dan C

Di bagian ini, kita akan melihat apa yang bisa (dan tidak bisa) dilakukan
C pada tiga encoding Unicode tertentu: UTF-8, UTF-16, dan UTF-32.

### UTF-8

\index{Unicode!UTF-8|(}

Untuk menyegarkan ingatan sebelum bagian ini, baca [catatan singkat UTF-8,
di atas](#utf8-quick).

Selain itu, apa kemampuan UTF-8 di C?

Sayangnya, tidak banyak.

\index{u8 UTF-8 prefix@\texttt{u8} UTF-8 prefix|(}

Kamu bisa memberi tahu C bahwa kamu secara spesifik menginginkan string
literal ber-encoding UTF-8, dan ia akan melakukannya. Caranya dengan
awalan `u8` pada string:

``` {.c}
char *s = u8"Hello, world!";

printf("%s\n", s);   // Hello, world!--kalau kamu bisa mengeluarkan UTF-8
```

Nah, bisakah karakter Unicode dimasukkan ke sana?

``` {.c}
char *s = u8"€123";
```

\index{u8 UTF-8 prefix@\texttt{u8} UTF-8 prefix|)}

Tentu! Kalau extended source character set mendukungnya. (gcc mendukung.)

Bagaimana kalau tidak? Kamu bisa menentukan code point Unicode dengan
`\u` dan `\U` yang akrab di sekitar kita, [seperti dicatat di
atas](#unicode-in-c).

Tapi hanya itu. Tidak ada cara portabel di standard library untuk
mengambil input sembarang dan mengubahnya menjadi UTF-8 kecuali locale-mu
UTF-8. Atau untuk mem-parse UTF-8 kecuali locale-mu UTF-8.

Jadi kalau ingin melakukannya, berada di locale UTF-8 dan:

\index{setlocale() function@\texttt{setlocale()} function|(}

``` {.c}
setlocale(LC_ALL, "");
```

atau cari tahu nama locale UTF-8 di mesin lokalmu dan atur secara eksplisit
seperti ini:

``` {.c}
setlocale(LC_ALL, "en_US.UTF-8");  // Nama non-portabel
```

\index{setlocale() function@\texttt{setlocale()} function|)}

Atau pakai [library pihak ketiga](#utf-3rd-party).

\index{Unicode!UTF-8|)}

### UTF-16, UTF-32, `char16_t`, dan `char32_t`

\index{Unicode!UTF-16|(}
\index{Unicode!UTF-32|(}
\index{char16\_t type@\texttt{char16\_t} type|(}
\index{char32\_t type@\texttt{char32\_t} type|(}

`char16_t` dan `char32_t` adalah dua tipe karakter yang berpotensi lebar,
masing-masing berukuran 16 bit dan 32 bit. Belum tentu lebar, karena kalau
tidak bisa merepresentasikan setiap karakter di locale saat ini, mereka
kehilangan sifat wide character-nya. Tapi spesifikasi menyebut mereka tipe
"wide character" di mana-mana, jadi begitulah.

Tipe-tipe ini ada untuk membuat segalanya sedikit lebih ramah Unicode,
mudah-mudahan.

Untuk memakainya, include `<uchar.h>`. (Itu "u", bukan "w".)

Header file ini tidak ada di OS X---sayang sekali. Kalau kamu hanya ingin
tipenya, kamu bisa:

``` {.c}
#include <stdint.h>

typedef int_least16_t char16_t;
typedef int_least32_t char32_t;
```

Tapi kalau kamu juga ingin fungsinya, itu urusanmu sendiri.

\index{u Unicode prefix@\texttt{u} Unicode prefix|(}
\index{U Unicode prefix@\texttt{U} Unicode prefix|(}

Dengan asumsi kamu masih bisa lanjut, kamu bisa mendeklarasikan string atau
karakter bertipe ini dengan awalan `u` dan `U`:

``` {.c}
char16_t *s = u"Hello, world!";
char16_t c = u'B';

char32_t *t = U"Hello, world!";
char32_t d = U'B';
```

\index{char32\_t type@\texttt{char32\_t} type|)}
\index{u Unicode prefix@\texttt{u} Unicode prefix|)}
\index{U Unicode prefix@\texttt{U} Unicode prefix|)}

Nah---apakah nilai di dalamnya disimpan dalam UTF-16 atau UTF-32?
Tergantung implementasi.

Tapi kamu bisa mengujinya. Kalau makro \index{\_\_STDC\_UTF\_16\_\_ macro@\texttt{\_\_STDC\_UTF\_16\_\_} macro} `__STDC_UTF_16__` atau \index{\_\_STDC\_UTF\_32\_\_ macro@\texttt{\_\_STDC\_UTF\_32\_\_ macro}}
`__STDC_UTF_32__` terdefinisi (bernilai `1`), artinya tipe-tipe itu
masing-masing menyimpan UTF-16 atau UTF-32.

Kalau kamu penasaran, dan aku tahu kamu penasaran, nilainya, kalau
UTF-16 atau UTF-32, disimpan dalam endianess native. Artinya, kamu
seharusnya bisa langsung membandingkannya dengan nilai code point Unicode:

``` {.c}
char16_t pi = u"\u03C0";  // simbol pi

#if __STDC_UTF_16__
pi == 0x3C0;  // Selalu true
#else
pi == 0x3C0;  // Kemungkinan tidak true
#endif
```

\index{char16\_t type@\texttt{char16\_t} type|)}
\index{Unicode!UTF-16|)}
\index{Unicode!UTF-32|)}

### Konversi Multibyte

Kamu bisa mengonversi dari encoding multibyte-mu ke `char16_t` atau
`char32_t` dengan sejumlah fungsi bantu.

(Seperti kubilang tadi, hasilnya mungkin bukan UTF-16 atau UTF-32
kecuali makro yang sesuai bernilai `1`.)

Semua fungsi ini restartable (yaitu kamu mengoper `mbstate_t`-mu sendiri),
dan semuanya beroperasi karakter demi
karakter^[Kurang lebih---urusannya jadi aneh pada encoding UTF-16 dengan
beberapa `char16_t`.].

\index{mbrtoc16() function@\texttt{mbrtoc16()} function}
\index{mbrtoc32() function@\texttt{mbrtoc32()} function}
\index{c16rtomb() function@\texttt{c16rtomb()} function}
\index{c32rtomb() function@\texttt{c32rtomb()} function}

|Fungsi Konversi|Deskripsi|
|-|-|
|`mbrtoc16()`|Mengonversi karakter multibyte menjadi karakter `char16_t`.|
|`mbrtoc32()`|Mengonversi karakter multibyte menjadi karakter `char32_t`.|
|`c16rtomb()`|Mengonversi karakter `char16_t` menjadi karakter multibyte.|
|`c32rtomb()`|Mengonversi karakter `char32_t` menjadi karakter multibyte.|

### Library Pihak Ketiga {#utf-3rd-party}

Untuk konversi berat antara berbagai encoding spesifik, ada beberapa
library matang yang layak dicoba. Perhatikan bahwa aku belum pernah
memakai keduanya.

* [iconv](https://en.wikipedia.org/wiki/Iconv)^[https://en.wikipedia.org/wiki/Iconv]---Internationalization Conversion, API standar POSIX
  yang umum dan tersedia di platform-platform besar.
* [ICU](http://site.icu-project.org/)^[http://site.icu-project.org/]---International Components for
  Unicode. Setidaknya satu blogger menganggapnya mudah dipakai.

Kalau kamu punya library lain yang patut dicatat, kabari aku.

\index{Unicode|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Keluar dari Program

\index{Exiting|(}

Ternyata ada banyak cara untuk melakukannya, bahkan ada cara untuk
memasang "hook" supaya sebuah fungsi dijalankan ketika program keluar.

Di bab ini kita akan menyelam dan melihatnya.

Kita sudah membahas makna kode exit status di bagian [Exit
Status](#exit-status), jadi lompat kembali ke sana dan ulas kalau perlu.

Semua fungsi di bagian ini ada di `<stdlib.h>`.

## Keluar Normal

Kita mulai dengan cara-cara biasa keluar dari program, lalu melompat ke
beberapa yang lebih jarang dan esoterik.

Ketika kamu keluar dari program secara normal, semua I/O stream yang
terbuka di-flush dan file sementara dihapus. Pada dasarnya ini keluar yang
rapi ketika semuanya dibereskan dan ditangani. Inilah yang kamu inginkan
hampir sepanjang waktu kecuali kamu punya alasan lain.

### Return dari `main()`

\index{Exiting!return from main()@return from \texttt{main()}|(}

Kalau kamu perhatikan, `main()` punya tipe kembalian `int`... namun aku jarang
sekali, kalaupun ada, me-`return` sesuatu dari `main()`.

Ini karena hanya untuk `main()` (dan aku tidak bisa cukup menekankan bahwa
kasus khusus ini _hanya_ berlaku untuk `main()` dan tidak ada fungsi lain
di mana pun) ada `return 0` _implisit_ kalau eksekusi jatuh melewati
ujung.

Kamu bisa `return` secara eksplisit dari `main()` kapan saja, dan sebagian
programmer merasa lebih _Benar_ untuk selalu menaruh `return` di akhir
`main()`. Tapi kalau tidak ditulis, C akan menaruhnya untukmu.

Jadi... berikut aturan `return` untuk `main()`:

* Kamu bisa mengembalikan exit status dari `main()` dengan pernyataan
  `return`. `main()` adalah satu-satunya fungsi dengan perilaku khusus ini.
  Memakai `return` di fungsi lain hanya kembali dari fungsi itu ke
  caller-nya.
* Kalau kamu tidak `return` secara eksplisit dan hanya jatuh melewati
  ujung `main()`, itu sama saja seperti kamu mengembalikan `0` atau
  `EXIT_SUCCESS`.

\index{Exiting!return from main()@return from \texttt{main()}|)}

### `exit()`

\index{Exiting!return from main()@return from \texttt{main()}|)}

Yang ini juga sudah muncul beberapa kali. Kalau kamu memanggil `exit()`
dari mana pun di programmu, program akan keluar di titik itu.

Argumen yang kamu oper ke `exit()` adalah exit status.

### Memasang Exit Handler dengan `atexit()`

\index{atexit() function@\texttt{atexit()} function|(}

Kamu bisa mendaftarkan fungsi yang dipanggil saat program keluar, baik
lewat return dari `main()` maupun lewat pemanggilan fungsi `exit()`.

Pemanggilan `atexit()` dengan nama fungsi handler akan melakukannya. Kamu
bisa mendaftarkan beberapa exit handler, dan mereka akan dipanggil dalam
urutan terbalik dari pendaftaran.

Berikut contohnya:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

void on_exit_1(void)
{
    printf("Exit handler 1 called!\n");
}

void on_exit_2(void)
{
    printf("Exit handler 2 called!\n");
}

int main(void)
{
    atexit(on_exit_1);
    atexit(on_exit_2);
    
    printf("About to exit...\n");
}
```

Dan outputnya:

``` {.default}
About to exit...
Exit handler 2 called!
Exit handler 1 called!
```

\index{atexit() function@\texttt{atexit()} function|)}

## Keluar Lebih Cepat dengan `quick_exit()`

\index{quick\_exit() function@\texttt{quick\_exit()} function|(}

Ini mirip keluar normal, kecuali:

* File yang terbuka mungkin tidak di-flush.
* File sementara mungkin tidak dihapus.
* Handler `atexit()` tidak akan dipanggil.

Tapi ada cara untuk mendaftarkan exit handler: panggil `at_quick_exit()`
dengan cara yang analog dengan memanggil `atexit()`.

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>

void on_quick_exit_1(void)
{
    printf("Quick exit handler 1 called!\n");
}

void on_quick_exit_2(void)
{
    printf("Quick exit handler 2 called!\n");
}

void on_exit(void)
{
    printf("Normal exit--I won't be called!\n");
}

int main(void)
{
    at_quick_exit(on_quick_exit_1);
    at_quick_exit(on_quick_exit_2);

    atexit(on_exit);  // Ini tidak akan dipanggil

    printf("About to quick exit...\n");

    quick_exit(0);
}
```

Yang memberi output ini:

``` {.default}
About to quick exit...
Quick exit handler 2 called!
Quick exit handler 1 called!
```

Cara kerjanya persis seperti `exit()`/`atexit()`, kecuali bahwa
pem-flush-an file dan pembersihan mungkin tidak dilakukan.

\index{quick\_exit() function@\texttt{quick\_exit()} function|)}

## Nuke it from Orbit: `_Exit()`

\index{\_Exit() function@\texttt{\_Exit()} function|(}

Memanggil `_Exit()` langsung keluar, titik. Tidak ada fungsi callback
on-exit yang dijalankan. File tidak di-flush. File sementara tidak
dihapus.

Pakai ini kalau kamu harus keluar _detik ini juga, demi apa pun_.

## Keluar Kadang-kadang: `assert()`

Pernyataan `assert()` dipakai untuk menegaskan bahwa sesuatu harus benar,
kalau tidak program akan keluar.

Developer sering memakai assert untuk menangkap error jenis
Seharusnya-Tidak-Mungkin-Terjadi.

``` {.c}
#define PI 3.14159

assert(PI > 3);   // Benar saja, jadi lanjut
```

versus:

``` {.c}
goats -= 100;

assert(goats >= 0);  // Tidak boleh ada kambing negatif
```

Dalam kasus itu, kalau kujalankan dan `goats` turun di bawah `0`, ini yang
terjadi:

``` {.default}
goat_counter: goat_counter.c:8: main: Assertion `goats >= 0' failed.
Aborted
```

dan aku dilempar kembali ke command line.

Ini kurang ramah pengguna, jadi hanya dipakai untuk hal-hal yang tidak akan
pernah dilihat pengguna. Dan sering orang [menulis makro assert sendiri
yang lebih mudah dimatikan](#my-assert).

\index{\_Exit() function@\texttt{\_Exit()} function|)}

## Keluar Tidak Normal: `abort()`

\index{abort() function@\texttt{abort()} function|(}

Kamu bisa memakai ini kalau ada yang salah sekali dan ingin
menandakannya kepada lingkungan luar. Ini juga tidak selalu membersihkan
file yang terbuka, dan sebagainya.

Aku jarang melihat ini dipakai.

Sedikit bocoran tentang _signal_: ini sebenarnya bekerja dengan memunculkan
\index{SIGABRT signal@\texttt{SIGABRT} signal} `SIGABRT` yang akan mengakhiri proses. 

Apa yang terjadi setelah itu tergantung sistem, tapi di sistem mirip Unix,
umumnya [dump core](https://en.wikipedia.org/wiki/Core_dump)^[https://en.wikipedia.org/wiki/Core_dump] saat program berakhir.

\index{abort() function@\texttt{abort()} function|)}
\index{Exiting|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Penanganan Signal

\index{Signal handling|(}

Sebelum mulai, aku hanya ingin menyarankan agar kamu pada umumnya mengabaikan
seluruh bab ini dan memakai fungsi penanganan signal bawaan OS-mu yang
(sangat mungkin) lebih unggul. Sistem mirip Unix punya keluarga fungsi
\index{sigaction() function@\texttt{sigaction()} function} `sigaction()`, dan Windows punya... entah apa
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

\index{SIGABRT signal@\texttt{SIGABRT} signal}
\index{SIGFPE signal@\texttt{SIGFPE} signal}
\index{SIGILL signal@\texttt{SIGILL} signal}
\index{SIGINT signal@\texttt{SIGINT} signal}
\index{SIGSEGV signal@\texttt{SIGSEGV} signal}
\index{SIGTERM signal@\texttt{SIGTERM} signal}

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

\index{signal() function@\texttt{signal()} function|(}

Pemanggilan `signal()` menerima dua parameter: signal yang dimaksud, dan
aksi yang diambil ketika signal itu di-raise.

Aksinya bisa berupa salah satu dari tiga hal:

* Pointer ke fungsi handler.
* \index{SIG\_IGN macro@\texttt{SIG\_IGN} macro|(}`SIG_IGN` untuk mengabaikan signal.
* \index{SIG\_DFL macro@\texttt{SIG\_DFL} macro}`SIG_DFL` untuk mengembalikan handler bawaan signal
  itu.

Mari kita tulis program yang tidak bisa di-`CTRL-C`. (Tenang saja---pada
program berikut, kamu juga bisa menekan `RETURN` dan ia akan keluar.)

\index{SIGINT signal@\texttt{SIGINT} signal|(}

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

\index{SIGINT signal@\texttt{SIGINT} signal|)}
\index{SIG\_IGN macro@\texttt{SIG\_IGN} macro|)}

## Menulis Signal Handler

Tadi kusebut bahwa kamu juga bisa menulis fungsi handler yang
dipanggil ketika signal di-raise.

Ini cukup lugas, tapi kemampuannya juga sangat terbatas menurut
spesifikasi.

\index{signal() function@\texttt{signal()} function|(}

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

\index{signal() function@\texttt{signal()} function|)}

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

Ia juga bisa mengembalikan \index{SIG\_ERR macro@\texttt{SIG\_ERR} macro} `SIG_ERR` kalau terjadi
error.

Mari kita buat contoh yang membuatmu harus menekan `CTRL-C` dua kali untuk
keluar.

Aku ingin menegaskan bahwa program ini melibatkan undefined behavior dalam
beberapa hal. Tapi kemungkinan besar program ini berfungsi di sistemmu,
dan sulit membuat demo non-trivial yang portabel.

\index{signal() function@\texttt{signal()} function|(}
\index{SIG\_INT signal@\texttt{SIG\_INT} signal|(}

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

\index{SIG\_INT signal@\texttt{SIG\_INT} signal|)}

Salah satu hal yang akan kamu lihat: di baris 14 kita mereset signal
handler. Ini karena C punya opsi untuk mereset signal handler ke perilaku
\index{SIG\_DFL macro@\texttt{SIG\_DFL} macro} `SIG_DFL`-nya sebelum menjalankan handler kustommu.
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

\index{signal() function@\texttt{signal()} function|)}

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
sendiri (misalnya \index{sigaction() function@\texttt{sigaction()} function} `sigaction()` untuk sistem
mirip Unix).

Wikipedia bahkan menyebut satu-satunya hal yang benar-benar portabel
adalah memanggil `signal()` dengan `SIG_IGN` atau `SIG_DFL`, itu saja.

Berikut yang **tidak bisa** kita lakukan secara portabel:

\index{Signal handling-!limitations|(}

* Memanggil fungsi standard library apa pun.
  * Seperti `printf()`, misalnya.
  * Kurasa mungkin aman memanggil fungsi restartable/reentrant,
    tapi spesifikasi tidak memberikan kebebasan itu.
* Mengambil atau mengisi nilai dari variabel lokal `static`, file scope,
  atau thread-local.
  * Kecuali kalau itu atomic object yang lock-free atau...
  * Kamu menugaskan ke variabel bertipe `volatile sig_atomic_t`.

\index{sig\_atomic\_t type@\texttt{sig\_atomic\_t} type|(}

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

\index{Signal handling-!limitations|)}

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

\index{sig\_atomic\_t type@\texttt{sig\_atomic\_t} type|)}

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

\index{SIG\_DFL macro@\texttt{SIG\_DFL} macro|(}

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

\index{SIG\_DFL macro@\texttt{SIG\_DFL} macro|)}

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

\index{signal() function@\texttt{signal()} function|)}
\index{Signal handling|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Variable-Length Array (VLA)

\index{Variable-length array|(}

C menyediakan cara untuk mendeklarasikan array yang ukurannya ditentukan
saat runtime. Dengan begitu kamu mendapat manfaat ukuran dinamis saat
runtime seperti pada `malloc()`, tapi tanpa perlu repot men-`free()`
memorinya sesudahnya.

Nah, banyak orang tidak menyukai VLA. Misalnya, VLA dilarang di kernel
Linux. Alasannya akan dibahas lebih jauh [nanti](#vla-general-issues).

\index{\_\_STDC\_NO\_VLA\_\_ macro@\texttt{\_\_STDC\_NO\_VLA\_\_} macro|(}

Ini fitur bahasa yang opsional. Makro `__STDC_NO_VLA__` bernilai `1` kalau
VLA _tidak_ ada. (VLA wajib di C99, lalu menjadi opsional di C11.)

``` {.c}
#if __STDC_NO_VLA__ == 1
   #error Sorry, need VLAs for this program!
#endif
```

\index{\_\_STDC\_NO\_VLA\_\_ macro@\texttt{\_\_STDC\_NO\_VLA\_\_} macro|)}

Tapi karena GCC maupun Clang tidak repot-repot mendefinisikan makro ini,
manfaatnya mungkin terbatas.

Kita mulai dulu dengan contoh, lalu kita cari setan di detailnya.

## Dasar-Dasarnya

Array biasa dideklarasikan dengan ukuran konstan, seperti ini:

``` {.c}
int v[10];
```

\index{Variable-length array!defining|(}

Tapi dengan VLA, kita bisa memakai ukuran yang ditentukan saat runtime
untuk membuat array, seperti ini:

``` {.c}
int n = 10;
int v[n];
```

Kelihatannya sama, dan dalam banyak hal memang sama, tapi cara ini memberi
keleluasaan untuk menghitung ukuran yang dibutuhkan, lalu mendapatkan
array dengan ukuran persis itu.

Mari kita minta pengguna memasukkan ukuran array, lalu menyimpan
indeks-kali-10 di setiap elemen array itu:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int n;
    char buf[32];

    printf("Enter a number: "); fflush(stdout);
    fgets(buf, sizeof buf, stdin);
    n = strtoul(buf, NULL, 10);

    int v[n];

    for (int i = 0; i < n; i++)
        v[i] = i * 10;

    for (int i = 0; i < n; i++)
        printf("v[%d] = %d\n", i, v[i]);
}
```

(Di baris 8 aku menaruh `fflush()` yang seharusnya memaksa baris itu keluar
meskipun aku tidak menaruh newline di akhirnya.)

Baris 12 adalah tempat VLA dideklarasikan---begitu eksekusi melewati baris
itu, ukuran array ditetapkan sesuai nilai `n` pada saat itu. Panjang array
tidak bisa diubah belakangan.

Kamu juga bisa menaruh ekspresi di dalam kurung siku:

``` {.c}
int v[x * 100];
```

Beberapa batasan:

* VLA tidak bisa dideklarasikan di file scope, dan tidak bisa dibuat
  `static` di block scope^[Ini karena VLA biasanya dialokasikan di stack,
  sedangkan variabel `static` ada di heap. Dan seluruh gagasan VLA adalah
  mereka otomatis didealokasikan saat stack frame di-pop di akhir
  fungsi.].
* Kamu tidak bisa memakai initializer list untuk menginisialisasi array.

Selain itu, memasukkan nilai negatif untuk ukuran array memicu undefined
behavior---setidaknya di alam semesta ini.

\index{Variable-length array!defining|)}

## `sizeof` dan VLA

\index{Variable-length array!and sizeof()@and \texttt{sizeof()}|(}

Kita terbiasa dengan `sizeof` yang memberi ukuran dalam byte untuk object
tertentu, termasuk array. VLA pun tidak terkecuali.

Perbedaan utamanya: `sizeof` pada VLA dieksekusi saat _runtime_, sedangkan
pada variabel berukuran tetap ia dihitung saat _compile time_.

Tapi pemakaiannya sama.

Kamu bahkan bisa menghitung jumlah elemen dalam VLA dengan trik array
biasa:

``` {.c}
size_t num_elems = sizeof v / sizeof v[0];
```

Baris di atas menyiratkan hal yang halus dan benar: aritmetika pointer
bekerja persis seperti yang kamu harapkan pada array biasa. Jadi pakai
saja sepuasmu:

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int n = 5;
    int v[n];

    int *p = v;

    *(p+2) = 12;
    printf("%d\n", v[2]);  // 12

    p[3] = 34;
    printf("%d\n", v[3]);  // 34
}
```

Seperti pada array biasa, kamu bisa memakai tanda kurung dengan `sizeof()`
untuk mengetahui ukuran calon VLA tanpa benar-benar mendeklarasikannya:

``` {.c}
int x = 12;

printf("%zu\n", sizeof(int [x]));  // Mencetak 48 di sistemku
```

\index{Variable-length array!and sizeof()@and \texttt{sizeof()}|)}

## VLA Multidimensi

\index{Variable-length array!multidimensional|(}

Kamu boleh membuat berbagai macam VLA dengan satu dimensi atau lebih
berupa variabel

``` {.c}
int w = 10;
int h = 20;

int x[h][w];
int y[5][w];
int z[10][w][20];
```

Sekali lagi, kamu bisa menjelajahinya persis seperti array biasa.

\index{Variable-length array!multidimensional|)}

## Mengoper VLA Satu Dimensi ke Fungsi

\index{Variable-length array!passing to functions|(}

Mengoper VLA satu dimensi ke fungsi tidak berbeda dari mengoper array
biasa. Langsung saja.

``` {.c .numberLines}
#include <stdio.h>

int sum(int count, int *v)
{
    int total = 0;

    for (int i = 0; i < count; i++)
        total += v[i];

    return total;
}

int main(void)
{
    int x[5];   // Array standar

    int a = 5;
    int y[a];   // VLA

    for (int i = 0; i < a; i++)
        x[i] = y[i] = i + 1;

    printf("%d\n", sum(5, x));
    printf("%d\n", sum(a, y));
}
```

Tapi ada sedikit lebih banyak daripada itu. Kamu juga bisa memberi tahu C
bahwa array itu berukuran VLA tertentu dengan mengopernya lebih dulu lalu
menyebut dimensi itu di daftar parameter:

``` {.c}
int sum(int count, int v[count])
{
    // ...
}
```

\index{Variable-length array!in function prototypes|(}
\index{* for VLA function prototypes@\texttt{*} for VLA function prototypes|(}

Omong-omong, ada beberapa cara menulis prototype untuk fungsi di atas;
salah satunya memakai `*` kalau kamu tidak ingin menyebut nama nilai di
VLA secara spesifik. Ia hanya menandakan bahwa tipenya VLA, bukan pointer
biasa.

Prototype VLA:

``` {.c}
void do_something(int count, int v[count]);  // Dengan nama
void do_something(int, int v[*]);            // Tanpa nama
```

Sekali lagi, `*` itu hanya berfungsi di prototype---di fungsinya sendiri,
kamu harus menulis ukuran secara eksplisit.

\index{* for VLA function prototypes@\texttt{*} for VLA function prototypes|)}
\index{Variable-length array!in function prototypes|)}

Nah---_saatnya multidimensi_! Di sinilah keseruan dimulai.

## Mengoper VLA Multidimensi ke Fungsi

Sama seperti yang kita lakukan pada bentuk kedua VLA satu dimensi di
atas, tapi kali ini kita mengoper dua dimensi dan memakainya.

Pada contoh berikut, kita membuat matriks tabel perkalian dengan lebar dan
tinggi yang bervariasi, lalu mengopernya ke fungsi untuk dicetak.

``` {.c .numberLines}
#include <stdio.h>

void print_matrix(int h, int w, int m[h][w])
{
    for (int row = 0; row < h; row++) {
        for (int col = 0; col < w; col++)
            printf("%2d ", m[row][col]);
        printf("\n");
    }
}

int main(void)
{
    int rows = 4;
    int cols = 7;

    int matrix[rows][cols];

    for (int row = 0; row < rows; row++)
        for (int col = 0; col < cols; col++)
            matrix[row][col] = row * col;

    print_matrix(rows, cols, matrix);
}
```

### VLA Multidimensi Parsial

Sebagian dimensi bisa tetap dan sebagian variabel. Misalkan kita punya
panjang record yang tetap 5 elemen, tapi kita tidak tahu ada berapa record.

``` {.c .numberLines}
#include <stdio.h>

void print_records(int count, int record[count][5])
{
    for (int i = 0; i < count; i++) {
        for (int j = 0; j < 5; j++)
            printf("%2d ", record[i][j]);
        printf("\n");
    }
}

int main(void)
{
    int rec_count = 3;
    int records[rec_count][5];

    // Isi dengan beberapa data tiruan
    for (int i = 0; i < rec_count; i++)
        for (int j = 0; j < 5; j++)
            records[i][j] = (i+1)*(j+2);

    print_records(rec_count, records);
}
```

\index{Variable-length array!passing to functions|)}

## Kompatibilitas dengan Array Biasa

\index{Variable-length array!with regular arrays|(}

Karena VLA di memori sama saja dengan array biasa, boleh sekali mengoper
keduanya secara bergantian... selama dimensinya cocok.

Misalnya, kalau ada fungsi yang secara spesifik menginginkan array
$3\times5$, kita tetap bisa mengoper VLA ke dalamnya.

``` {.c}
int foo(int m[5][3]) {...}

\\ ...

int w = 3, h = 5;
int matrix[h][w];

foo(matrix);   // OK!
```

Begitu juga, kalau kamu punya fungsi VLA, kamu bisa mengoper array biasa
ke dalamnya:

``` {.c}
int foo(int h, int w, int m[h][w]) {...}

\\ ...

int matrix[3][5];

foo(3, 5, matrix);   // OK!
```

Tapi waspada: kalau dimensinya tidak cocok, kemungkinan besar kamu akan
mengalami undefined behavior.

\index{Variable-length array!with regular arrays|)}

## `typedef` dan VLA

\index{Variable-length array!with typedef@with \texttt{typedef}|(}

Kamu bisa me-`typedef` VLA, tapi perilakunya mungkin tidak seperti yang
kamu harapkan.

Pada dasarnya, `typedef` membuat tipe baru dengan nilai-nilai seperti yang
ada pada saat `typedef` dieksekusi.

Jadi ini bukan `typedef` dari VLA, melainkan tipe array berukuran tetap
baru dengan dimensi pada saat itu.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int w = 10;

    typedef int goat[w];

    // goat adalah array berisi 10 int
    goat x;

    // Inisialisasi dengan kuadrat angka
    for (int i = 0; i < w; i++)
        x[i] = i*i;

    // Cetak semuanya
    for (int i = 0; i < w; i++)
        printf("%d\n", x[i]);

    // Sekarang kita ubah w...

    w = 20;

    // Tapi goat TETAP array berisi 10 int, karena itulah nilai w
    // saat typedef dieksekusi.
}
```

Jadi ia bertingkah seperti array berukuran tetap.

Tapi kamu tetap tidak bisa memakai initializer list padanya.

\index{Variable-length array!with typedef@with \texttt{typedef}|)}

## Jebakan Lompatan

\index{Variable-length array!with goto@with \texttt{goto}|(}

Kamu harus berhati-hati saat memakai `goto` di dekat VLA karena banyak hal
yang tidak legal.

\index{Variable-length array!with goto@with \texttt{goto}|)}
\index{Variable-length array!with longjmp()@with \texttt{longjmp()}|(}

Dan saat memakai `longjmp()` ada kasus ketika kamu bisa membocorkan memori
dengan VLA.

\index{Variable-length array!with longjmp()@with \texttt{longjmp()}|)}

Tapi kedua hal ini akan kita bahas di bab masing-masing.


## Masalah Umum {#vla-general-issues}

\index{Variable-length array!controversy|(}

VLA dilarang di kernel Linux karena beberapa alasan:

* Banyak tempat yang memakainya seharusnya cukup dengan ukuran tetap.
* Kode di balik VLA lebih lambat (sampai tingkat yang tidak disadari
  kebanyakan orang, tapi berpengaruh pada sistem operasi).
* VLA tidak didukung sama baiknya oleh semua C compiler.
* Ukuran stack terbatas, dan VLA masuk ke stack. Kalau ada kode yang
  tidak sengaja (atau sengaja) mengoper nilai besar ke fungsi kernel yang
  mengalokasikan VLA, _Hal-Hal Buruk_™ bisa terjadi.

Orang lain di internet menunjukkan bahwa tidak ada cara mendeteksi
kegagalan alokasi VLA, dan program yang mengalami masalah seperti itu
kemungkinan besar langsung crash. Array berukuran tetap juga punya masalah
yang sama, tapi jauh lebih mungkin seseorang tanpa sengaja membuat _VLA
Berukuran Tidak Lazim_ daripada entah bagaimana tanpa sengaja
mendeklarasikan array berukuran tetap, katakanlah, 30 megabyte.

\index{Variable-length array!controversy|)}
\index{Variable-length array|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `goto`

\index{goto statement@\texttt{goto} statement|(}

Pernyataan `goto` dipuja secara universal dan bisa disajikan di sini tanpa
perdebatan.

Bercanda! Bertahun-tahun lamanya, orang bolak-balik berdebat apakah
`goto` [dianggap berbahaya](https://en.wikipedia.org/wiki/Goto#Criticism)^[https://en.wikipedia.org/wiki/Goto#Criticism] atau tidak (sering kali
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

\index{Labels|(}

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

\index{Labels|)}

## `continue` Berlabel

\index{goto statement@\texttt{goto} statement!as labeled continue@as labeled \texttt{continue}|(}

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

\index{goto statement@\texttt{goto} statement!as labeled continue@as labeled \texttt{continue}|)}

## Keluar Darurat

\index{goto statement@\texttt{goto} statement!for bailing out|(}

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

\index{goto statement@\texttt{goto} statement!for bailing out|)}

## `break` Berlabel

\index{goto statement@\texttt{goto} statement!as labeled break@as labeled \texttt{break}|(}

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

\index{goto statement@\texttt{goto} statement!as labeled break@as labeled \texttt{break}|)}

## Pembersihan Bertingkat

\index{goto statement@\texttt{goto} statement!multilevel cleanup|(}

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

\index{goto statement@\texttt{goto} statement!multilevel cleanup|)}

## Tail Call Optimization

\index{goto statement@\texttt{goto} statement!tail call optimzation|(}
\index{Tail call optimzation!with goto@with \texttt{goto}|(}

Kurang lebih. Hanya untuk fungsi rekursif.

Kalau kamu belum akrab, [Tail Call Optimization (TCO)](https://en.wikipedia.org/wiki/Tail_call)^[https://en.wikipedia.org/wiki/Tail_call]
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

\index{goto statement@\texttt{goto} statement!tail call optimzation|)}
\index{Tail call optimzation!with goto@with \texttt{goto}|)}

## Memulai Ulang System Call yang Terinterupsi

\index{goto statement@\texttt{goto} statement!restarting system calls|(}

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

\index{goto statement@\texttt{goto} statement!restarting system calls|)}

## `goto` dan Thread Preemption

\index{goto statement@\texttt{goto} statement!thread preemption|(}

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

\index{goto statement@\texttt{goto} statement!thread preemption|)}

## `goto` dan Scope Variabel 

\index{goto statement@\texttt{goto} statement!variable scope|(}

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

\index{goto statement@\texttt{goto} statement!variable scope|)}

## `goto` dan Variable-Length Array

\index{goto statement@\texttt{goto} statement!with variable-length arrays|(}

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

\index{goto statement@\texttt{goto} statement!with variable-length arrays|)}
\index{goto statement@\texttt{goto} statement|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe Bagian V: Compound Literal dan Generic Selection

Ini bab terakhir tentang tipe! Kita akan membahas dua hal:

* Cara memiliki object "anonim" tanpa nama dan apa gunanya.
* Cara menghasilkan kode yang bergantung pada tipe.

Keduanya tidak terlalu berkaitan, tapi masing-masing tidak cukup layak
dijadikan satu bab sendiri. Jadi aku menjejalkannya di sini seperti
pemberontak!

## Compound Literal

\index{Compound literals|(}

Ini fitur bahasa yang keren, yang memungkinkan kamu membuat object bertipe
tertentu secara langsung tanpa pernah menugaskannya ke variabel. Kamu bisa
membuat tipe sederhana, array, `struct`, apa saja.

Salah satu kegunaan utamanya adalah mengoper argumen kompleks ke fungsi
ketika kamu tidak ingin membuat variabel sementara untuk menampung
nilainya.

Cara membuat compound literal adalah menaruh nama tipe dalam tanda kurung,
lalu menaruh initializer list setelahnya. Misalnya, array `int` tanpa
nama mungkin tampak seperti ini:

``` {.c}
(int []){1,2,3,4}
```

Nah, baris kode itu sendiri tidak melakukan apa-apa. Ia membuat array
tanpa nama berisi 4 `int`, lalu membuangnya tanpa dipakai.

Kita bisa memakai pointer untuk menyimpan referensi ke array itu...

``` {.c}
int *p = (int []){1 ,2 ,3 ,4};

printf("%d\n", p[1]);  // 2
```

Tapi itu terasa seperti cara bertele-tele untuk punya array. Maksudnya, kita
bisa saja melakukan ini^[Yang tidak persis sama, karena ia array, bukan
pointer ke `int`.]:

``` {.c}
int p[] = {1, 2, 3, 4};

printf("%d\n", p[1]);  // 2
```

Jadi mari kita lihat contoh yang lebih berguna.

### Mengoper Object Tanpa Nama ke Fungsi

\index{Compound literals!passing to functions|(}

Katakanlah kita punya fungsi untuk menjumlahkan array `int`:

``` {.c}
int sum(int p[], int count)
{
    int total = 0;

    for (int i = 0; i < count; i++)
        total += p[i];

    return total;
}
```

Kalau ingin memanggilnya, biasanya kita harus melakukan sesuatu seperti
ini, mendeklarasikan array dan mengisinya dengan nilai untuk dioper ke
fungsi:

``` {.c}
int a[] = {1, 2, 3, 4};

int s = sum(a, 4);
```

Tapi object tanpa nama memberi kita cara untuk melewati variabel itu
dengan mengopernya langsung (nama parameter tercantum di atas). Lihat
ini---kita akan mengganti variabel `a` dengan array tanpa nama yang dioper
sebagai argumen pertama:

``` {.c}
//                   p[]         count
//           |-----------------|  |
int s = sum((int []){1, 2, 3, 4}, 4);
```

Keren sekali!

\index{Compound literals!passing to functions|)}

### `struct` Tanpa Nama

\index{Compound literals!with struct@with \texttt{struct}|(}
\index{struct keyword@\texttt{struct} keyword!compound literals|(}

Kita bisa melakukan hal serupa dengan `struct`.

Pertama, kita kerjakan tanpa object tanpa nama. Kita definisikan `struct`
untuk menyimpan koordinat `x`/`y`. Lalu kita definisikan satu, dengan
mengoper nilai ke initializer-nya. Terakhir, kita oper ke fungsi untuk
mencetak nilainya:

``` {.c .numberLines}
#include <stdio.h>

struct coord {
    int x, y;
};

void print_coord(struct coord c)
{
    printf("%d, %d\n", c.x, c.y);
}

int main(void)
{
    struct coord t = {.x=10, .y=20};

    print_coord(t);   // mencetak "10, 20"
}
```

Cukup lugas?

Mari kita ubah agar memakai object tanpa nama sebagai ganti variabel `t`
yang kita oper ke `print_coord()`.

Kita cukup mengambil `t` dari sana dan menggantinya dengan `struct` tanpa
nama:

``` {.c .numberLines startFrom="7"}
    //struct coord t = {.x=10, .y=20};

    print_coord((struct coord){.x=10, .y=20});   // mencetak "10, 20"
```

Tetap berfungsi!

\index{struct keyword@\texttt{struct} keyword!compound literals|)}
\index{Compound literals!with struct@with \texttt{struct}|)}

### Pointer ke Object Tanpa Nama

\index{Compound literals!pointers to|(}

Mungkin kamu memperhatikan pada contoh terakhir bahwa walaupun kita
memakai `struct`, yang dioper ke `print_coord()` adalah salinan `struct`,
bukan pointer ke `struct`.

Ternyata kita bisa mengambil alamat object tanpa nama dengan `&` seperti
biasa.

Ini karena, secara umum, kalau suatu operator bisa bekerja pada variabel
bertipe tertentu, kamu bisa memakai operator itu pada object tanpa nama
bertipe sama.

Mari kita ubah kode di atas agar mengoper pointer ke object tanpa nama

``` {.c .numberLines}
#include <stdio.h>

struct coord {
    int x, y;
};

void print_coord(struct coord *c)
{
    printf("%d, %d\n", c->x, c->y);
}

int main(void)
{
    //     Perhatikan &
    //          |
    print_coord(&(struct coord){.x=10, .y=20});   // mencetak "10, 20"
}
```

Selain itu, ini bisa menjadi cara yang bagus untuk mengoper pointer ke
object sederhana sekalipun:

``` {.c}
// Oper pointer ke int bernilai 3490
foo(&(int){3490});
```

Semudah itu.

\index{Compound literals!pointers to|)}

### Object Tanpa Nama dan Scope

\index{Compound literals!scope|(}

Masa hidup object tanpa nama berakhir di akhir scope-nya. Jebakan terbesarnya:
kamu membuat object tanpa nama baru, mengambil pointer
ke sana, lalu meninggalkan scope object itu. Dalam kasus itu, pointer
akan merujuk ke object yang sudah mati.

Jadi ini undefined behavior:

``` {.c}
int *p;

{
    p = &(int){10};
}

printf("%d\n", *p);  // TIDAK VALID: (int){10} sudah keluar dari scope
```

Begitu juga, kamu tidak bisa mengembalikan pointer ke object tanpa nama
dari sebuah fungsi. Object itu didealokasikan saat keluar dari scope:

``` {.c .numberLines}
#include <stdio.h>

int *get3490(void)
{
    // Jangan lakukan ini
    return &(int){3490};
}

int main(void)
{
    printf("%d\n", *get3490());  // TIDAK VALID: (int){3490} sudah keluar dari scope
}
```

Bayangkan saja scope-nya seperti scope variabel lokal biasa. Kamu juga
tidak bisa mengembalikan pointer ke variabel lokal.

\index{Compound literals!scope|)}

### Contoh Konyol Object Tanpa Nama

Tipe apa pun bisa ditaruh di sana untuk membuat object tanpa nama.

Misalnya, ketiga ini secara efektif setara:

``` {.c}
int x = 3490;

printf("%d\n", x);               // 3490 (variabel)
printf("%d\n", 3490);            // 3490 (konstanta)
printf("%d\n", (int){3490});     // 3490 (object tanpa nama)
```

Yang terakhir itu tanpa nama, tapi konyol. Lebih baik pakai yang sederhana
di baris sebelumnya.

Tapi semoga ini memberi sedikit kejelasan tentang sintaksnya.

\index{Compound literals|)}

## Generic Selection {#type-generics}

\index{Generic selections|(}

Ini ekspresi yang memungkinkan kamu memilih potongan kode berbeda
tergantung pada _tipe_ argumen pertama ekspresi itu.

Sebentar lagi kita lihat contohnya, tapi penting diketahui bahwa ini
diproses saat compile time, _bukan saat runtime_. Tidak ada analisis
runtime di sini.

\index{\_Generic keyword@\texttt{\_Generic} keyword|(}

Ekspresi ini diawali `_Generic`, bekerja mirip `switch`, dan menerima
setidaknya dua argumen.

Argumen pertama adalah ekspresi (atau variabel^[Variabel yang dipakai di
sini _adalah_ ekspresi.]) yang punya _tipe_. Semua ekspresi punya tipe.
Argumen sisanya untuk `_Generic` adalah kasus-kasus tentang apa yang
disubstitusikan sebagai hasil ekspresi kalau argumen pertama bertipe itu.

Wat?

Mari kita coba dan lihat.

``` {.c .numberLines}
#include <stdio.h>

int main(void)
{
    int i;
    float f;
    char c;

    char *s = _Generic(i,
                    int: "that variable is an int",
                    float: "that variable is a float",
                    default: "that variable is some type"
                );

    printf("%s\n", s);
}
```

Lihat ekspresi `_Generic` yang dimulai di baris 9.

Saat compiler melihatnya, ia melihat tipe argumen pertama. (Pada contoh
ini, tipe variabel `i`.) Lalu ia menelusuri kasus-kasus untuk mencari yang
bertipe itu. Setelah itu ia menyubstitusikan argumen tersebut menggantikan
seluruh ekspresi `_Generic`. 

Dalam kasus ini, `i` adalah `int`, jadi cocok dengan kasus itu. Lalu string
itu disubstitusikan ke ekspresi. Jadi baris itu berubah menjadi ini saat
dilihat compiler:

``` {.c}
    char *s = "that variable is an int";
```

Kalau compiler tidak menemukan kecocokan tipe di `_Generic`, ia mencari
kasus `default` yang opsional dan memakainya.

Kalau tidak ada kecocokan tipe dan tidak ada `default`, kamu akan mendapat
error kompilasi. Ekspresi pertama **harus** cocok dengan salah satu tipe
atau `default`.

Karena merepotkan menulis `_Generic` berulang-ulang, ia sering dipakai
untuk membuat badan makro yang bisa dengan mudah dipakai ulang.

Mari kita buat makro `TYPESTR(x)` yang menerima satu argumen dan
mengembalikan string berisi tipe argumen itu.

Jadi `TYPESTR(1)` akan mengembalikan string `"int"`, misalnya.

Begini caranya:

``` {.c}
#include <stdio.h>

#define TYPESTR(x) _Generic((x), \
                        int: "int", \
                        long: "long", \
                        float: "float", \
                        double: "double", \
                        default: "something else")

int main(void)
{
    int i;
    long l;
    float f;
    double d;
    char c;

    printf("i is type %s\n", TYPESTR(i));
    printf("l is type %s\n", TYPESTR(l));
    printf("f is type %s\n", TYPESTR(f));
    printf("d is type %s\n", TYPESTR(d));
    printf("c is type %s\n", TYPESTR(c));
}
```

Ini menghasilkan output:

``` {.default}
i is type int
l is type long
f is type float
d is type double
c is type something else
```

Yang seharusnya tidak mengejutkan, karena, seperti yang kita katakan, kode
di `main()` itu diganti dengan yang berikut saat dikompilasi:

``` {.c}
    printf("i is type %s\n", "int");
    printf("l is type %s\n", "long");
    printf("f is type %s\n", "float");
    printf("d is type %s\n", "double");
    printf("c is type %s\n", "something else");
```

Dan itulah persis output yang kita lihat.

Satu contoh lagi. Di sini aku menyertakan beberapa makro agar ketika
kamu menjalankan:

``` {.c}
int i = 10;
char *s = "Foo!";

PRINT_VAL(i);
PRINT_VAL(s);
```

kamu mendapat output:

``` {.default}
i = 10
s = Foo!
```

Kita perlu memakai sedikit sihir makro untuk itu.

``` {.c .numberLines}
#include <stdio.h>
#include <string.h>

// Makro yang mengembalikan format specifier untuk sebuah tipe
#define FMTSPEC(x) _Generic((x), \
                        int: "%d", \
                        long: "%ld", \
                        float: "%f", \
                        double: "%f", \
                        char *: "%s")
                        // TODO: tambahkan tipe lain
                        
// Makro yang mencetak variabel dalam bentuk "nama = nilai"
#define PRINT_VAL(x) do { \
    char fmt[512]; \
    snprintf(fmt, sizeof fmt, #x " = %s\n", FMTSPEC(x)); \
    printf(fmt, (x)); \
} while(0)

int main(void)
{
    int i = 10;
    float f = 3.14159;
    char *s = "Hello, world!";

    PRINT_VAL(i);
    PRINT_VAL(f);
    PRINT_VAL(s);
}
```

\index{\_Generic keyword@\texttt{\_Generic} keyword|)}

untuk output:

``` {.default}
i = 10
f = 3.141590
s = Hello, world!
```

Kita bisa saja menjejalkan semuanya dalam satu makro besar, tapi aku
memecahnya jadi dua supaya mata tidak berdarah.

\index{Generic selections|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Array Bagian II

Di bab ini kita akan membahas beberapa hal serba-serbi tambahan tentang
array.

* Type qualifier pada parameter array
* Kata kunci `static` pada parameter array
* Initializer array multidimensi parsial

Hal-hal ini tidak terlalu sering terlihat, tapi kita intip saja karena
termasuk bagian spesifikasi yang lebih baru.

## Type Qualifier untuk Array dalam Daftar Parameter

\index{Type qualifiers!arrays in parameter lists|(}
\index{Arrays!type qualifiers in parameter lists|(}

Kalau kamu ingat dari bab sebelumnya, dua hal ini setara dalam daftar
parameter fungsi:

``` {.c}
int func(int *p) {...}
int func(int p[]) {...}
```

Dan mungkin kamu juga ingat bahwa kamu bisa menambahkan type qualifier ke
variabel pointer seperti ini:

``` {.c}
int *const p;
int *volatile p;
int *const volatile p;
// dan seterusnya.
```

Tapi bagaimana melakukannya kalau kita memakai notasi array di daftar
parameter?

Ternyata qualifier itu ditaruh di dalam kurung siku. Dan kamu bisa
menaruh jumlah opsional setelahnya. Ketiga baris berikut setara:

``` {.c}
int func(int *const volatile p) {...}
int func(int p[const volatile]) {...}
int func(int p[const volatile 10]) {...}
```

Kalau kamu punya array multidimensi, type qualifier harus ditaruh di
pasangan kurung siku pertama.

\index{Type qualifiers!arrays in parameter lists|)}
\index{Arrays!type qualifiers in parameter lists|)}

## `static` untuk Array dalam Daftar Parameter

\index{Arrays!static in parameter lists@\texttt{static} in parameter lists|(}

Demikian pula, kata kunci static bisa dipakai pada array di daftar
parameter.

Aku belum pernah melihat ini di alam liar. Ia **selalu** diikuti
dimensi:

``` {.c}
int func(int p[static 4]) {...}
```

Artinya, pada contoh di atas, compiler akan menganggap bahwa array apa pun
yang dioper ke fungsi berisi _setidaknya_ 4 elemen.

Selain itu adalah undefined behavior.

``` {.c}
int func(int p[static 4]) {...}

int main(void)
{
    int a[] = {11, 22, 33, 44};
    int b[] = {11, 22, 33, 44, 55};
    int c[] = {11, 22};

    func(a);  // OK! a berisi 4 elemen, batas minimum
    func(b);  // OK! b berisi setidaknya 4 elemen
    func(c);  // Undefined behavior! c berisi kurang dari 4 elemen!
}
```

Ini pada dasarnya menetapkan ukuran minimum array yang boleh dioper.

Catatan penting: tidak ada apa pun di compiler yang melarangmu mengoper
array yang lebih kecil. Compiler kemungkinan tidak akan memperingatkan, dan
tidak akan mendeteksinya saat runtime.

Dengan menaruh `static` di sana, kamu berkata, "Aku JANJI dengan
sumpah rahasia ganda bahwa aku tidak akan pernah mengoper array yang lebih kecil
dari ini." Dan compiler menjawab, "Ya, baiklah," dan memercayaimu untuk
tidak melakukannya.

Lalu compiler bisa melakukan optimasi kode tertentu, dengan tenang karena
tahu bahwa kamu, sang programmer, akan selalu melakukan hal yang benar.

\index{Arrays!static in parameter lists@\texttt{static} in parameter lists|)}

## Initializer yang Setara

\index{Arrays!multidimensional initializers|(}

C itu, bolehlah dibilang, agak _fleksibel_ soal array initializer.

Kita sudah melihat sebagian, yaitu nilai yang tidak disebutkan diganti
dengan nol.

Misalnya, kita bisa menginisialisasi array 5 elemen menjadi `1,2,0,0,0`
dengan ini:

``` {.c}
int a[5] = {1, 2};
```

Atau mengisi seluruh array dengan nol memakai:

``` {.c}
int a[5] = {0};
```

Tapi segalanya jadi menarik saat menginisialisasi array multidimensi.

Kita buat array dengan 3 baris dan 2 kolom:

``` {.c}
int a[3][2];
```

Kita tulis kode untuk menginisialisasinya dan mencetak hasilnya:

``` {.c}
#include <stdio.h>

int main(void)
{
    int a[3][2] = {
        {1, 2},
        {3, 4},
        {5, 6}
    };

    for (int row = 0; row < 3; row++) {
        for (int col = 0; col < 2; col++)
            printf("%d ", a[row][col]);
        printf("\n");
    }
}
```

Dan saat dijalankan, kita mendapat hasil yang diharapkan:

``` {.default}
1 2
3 4
5 6
```

Mari kita hilangkan beberapa elemen initializer dan lihat bahwa mereka
diisi nol:


``` {.c}
    int a[3][2] = {
        {1, 2},
        {3},    // Angka 4 dihilangkan!
        {5, 6}
    };
```

yang menghasilkan:

``` {.default}
1 2
3 0
5 6
```

Sekarang kita hilangkan seluruh elemen tengah:

``` {.c}
    int a[3][2] = {
        {1, 2},
        // {3, 4},   // Potong saja seluruh bagian ini
        {5, 6}
    };
```

Dan sekarang kita mendapat ini, yang mungkin bukan yang kamu harapkan:

``` {.default}
1 2
5 6
0 0
```

Tapi kalau dipikir-pikir, kita hanya memberi initializer yang cukup untuk
dua baris, jadi initializer itu dipakai untuk dua baris pertama. Dan elemen
sisanya diinisialisasi dengan nol.

Sejauh ini bagus. Secara umum, kalau kita menghilangkan sebagian
initializer, compiler mengisi elemen yang bersesuaian dengan `0`.

Tapi sekarang mari kita menggila.

``` {.c}
    int a[3][2] = { 1, 2, 3, 4, 5, 6 };
```

Apa---? Itu array 2D, tapi initializer-nya hanya 1D!

Ternyata itu legal (walaupun GCC akan memperingatkannya kalau peringatan
yang tepat diaktifkan).

Pada dasarnya, yang terjadi: pengisian elemen dimulai di baris 0, lalu
baris 1, lalu baris 2, dari kiri ke kanan.

Jadi saat dicetak, urutannya:

``` {.default}
1 2
3 4
5 6
```

Kalau sebagian dihilangkan:

``` {.c}
    int a[3][2] = { 1, 2, 3 };
```

sisanya diisi `0`:

``` {.default}
1 2
3 0
0 0
```

Jadi kalau ingin mengisi seluruh array dengan `0`, silakan:

``` {.c}
    int a[3][2] = {0};
```

Tapi rekomendasiku: kalau punya array 2D, pakai initializer 2D. Kode jadi
lebih mudah dibaca. (Kecuali untuk menginisialisasi seluruh array dengan
`0`, yang idiomatis memakai `{0}` berapa pun dimensi arraynya.)

\index{Arrays!multidimensional initializers|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Long Jump dengan `setjmp`, `longjmp` {#setjmp-longjmp}

\index{Long jumps|(}

Kita sudah melihat `goto`, yang melompat dalam function scope. Tapi
`longjmp()` memungkinkanmu melompat kembali ke titik eksekusi yang lebih
awal, kembali ke fungsi yang memanggil fungsi ini.

Ada banyak batasan dan peringatan, tapi ini bisa menjadi fungsi yang
berguna untuk keluar dari kedalaman call stack kembali ke keadaan yang
lebih awal.

Menurut pengalamanku, fungsionalitas ini sangat jarang dipakai.

## Memakai `setjmp` dan `longjmp`

\index{setjmp() function@\texttt{setjmp()} function|(}
\index{longjmp() function@\texttt{longjmp()} function|(}

Tarian yang akan kita lakukan di sini pada dasarnya adalah menaruh
penanda buku dalam eksekusi dengan `setjmp()`. Nanti, kita memanggil
`longjmp()` dan ia akan melompat kembali ke titik eksekusi sebelumnya
tempat kita menaruh penanda itu dengan `setjmp()`.

Dan ini bisa dilakukan bahkan kalau kamu sudah memanggil subfungsi.

Berikut demo singkat ketika kita memanggil fungsi beberapa tingkat ke
dalam lalu keluar darurat.

Kita akan memakai variabel file scope `env` untuk menyimpan _state_ saat
kita memanggil `setjmp()` sehingga bisa memulihkannya ketika memanggil
`longjmp()` nanti. Inilah variabel tempat kita mengingat "posisi" kita.

Variabel `env` bertipe `jmp_buf`, tipe opaque yang dideklarasikan di
`<setjmp.h>`.

``` {.c .numberLines}
#include <stdio.h>
#include <setjmp.h>

jmp_buf env;

void depth2(void)
{
    printf("Entering depth 2\n");
    longjmp(env, 3490);           // Keluar darurat
    printf("Leaving depth 2\n");  // Ini tidak akan terjadi
}

void depth1(void)
{
    printf("Entering depth 1\n");
    depth2();
    printf("Leaving depth 1\n");  // Ini tidak akan terjadi
}

int main(void)
{
    switch (setjmp(env)) {
      case 0:
          printf("Calling into functions, setjmp() returned 0\n");
          depth1();
          printf("Returned from functions\n");  // Ini tidak akan terjadi
          break;

      case 3490:
          printf("Bailed back to main, setjmp() returned 3490\n");
          break;
    }
}
```

Saat dijalankan, ini mengeluarkan:

``` {.default}
Calling into functions, setjmp() returned 0
Entering depth 1
Entering depth 2
Bailed back to main, setjmp() returned 3490
```

Kalau kamu mencoba mencocokkan output itu dengan kodenya, jelas ada hal yang
benar-benar _aneh_ sedang terjadi.

Salah satu yang paling mencolok: `setjmp()` mengembalikan nilai _dua kali_.
Apa-apaan ini? Sihir apa ini?!

Begini ceritanya: kalau `setjmp()` mengembalikan `0`, artinya kamu
berhasil menaruh "penanda buku" di titik itu.

Kalau ia mengembalikan nilai bukan nol, artinya kamu baru saja kembali ke
"penanda buku" yang ditaruh sebelumnya. (Dan nilai yang dikembalikan adalah
nilai yang kamu oper ke `longjmp()`.)

Dengan cara ini kamu bisa membedakan antara menaruh penanda dan kembali ke
penanda itu nanti.

Jadi ketika kode di atas memanggil `setjmp()` untuk pertama kali,
`setjmp()` _menyimpan_ state di variabel `env` dan mengembalikan `0`.
Nanti saat kita memanggil `longjmp()` dengan `env` yang sama, ia
memulihkan state itu dan `setjmp()` mengembalikan nilai yang dioper ke
`longjmp()`.

\index{setjmp() function@\texttt{setjmp()} function|)}
\index{longjmp() function@\texttt{longjmp()} function|)}

## Jebakan

Di balik layar, ini cukup lugas. Biasanya _stack pointer_ melacak lokasi di
memori tempat variabel lokal disimpan, dan _program counter_ melacak
alamat instruksi yang sedang dieksekusi^[Baik "stack pointer" maupun
"program counter" terkait dengan arsitektur dan implementasi C di
bawahnya, dan bukan bagian dari spesifikasi.].

Jadi kalau kita ingin melompat kembali ke fungsi yang lebih awal, pada
dasarnya tinggal memulihkan stack pointer dan program counter ke nilai yang
disimpan di variabel \index{jmp\_buf type@\texttt{jmp\_buf} type} `jmp_buf`, dan memastikan nilai
kembaliannya diatur dengan benar. Lalu eksekusi akan dilanjutkan di sana.

Tapi berbagai faktor mengacaukan ini, menciptakan banyak jebakan
undefined behavior.

### Nilai Variabel Lokal

\index{setjmp() function@\texttt{setjmp()} function|(}

Kalau kamu ingin nilai variabel lokal otomatis (bukan `static` dan bukan
`extern`) tetap bertahan di fungsi yang memanggil `setjmp()` setelah
\index{longjmp()@\texttt{longjmp()}} `longjmp()` terjadi, kamu harus mendeklarasikan variabel
itu sebagai \index{volatile type qualifier@\texttt{volatile} type qualifier!with setjmp()@with \texttt{setjmp()}|(} `volatile`.

Secara teknis, mereka hanya perlu `volatile` kalau nilainya berubah
antara saat `setjmp()` dipanggil dan `longjmp()` dipanggil^[Alasannya,
program mungkin menyimpan nilai sementara di _register CPU_ saat
mengerjakannya. Pada rentang waktu itu, register menyimpan nilai yang
benar, dan nilai di stack mungkin sudah usang. Lalu nanti nilai register
akan tertimpa dan perubahan pada variabel itu hilang.].

Misalnya, kalau kita menjalankan kode ini:

``` {.c}
int x = 20;

if (setjmp(env) == 0) {
    x = 30;
}
```

dan nanti `longjmp()` kembali, nilai `x` akan tak tentu.

Kalau ingin memperbaikinya, `x` harus `volatile`:

``` {.c}
volatile int x = 20;

if (setjmp(env) == 0) {
    x = 30;
}
```

\index{setjmp() function@\texttt{setjmp()} function|)}

\index{volatile type qualifier@\texttt{volatile} type qualifier!with setjmp()@with \texttt{setjmp()}|)}

Sekarang nilainya akan benar, yaitu `30`, setelah \index{longjmp()@\texttt{longjmp()}}
`longjmp()` membawa kita kembali ke titik ini.

### Berapa Banyak State yang Disimpan?

\index{setjmp() function@\texttt{setjmp()} function|(}
\index{longjmp() function@\texttt{longjmp()} function|(}

Saat kamu `longjmp()`, eksekusi dilanjutkan di titik `setjmp()` yang
bersesuaian. Dan hanya itu.

\index{setjmp() function@\texttt{setjmp()} function|)}

Spesifikasi menunjukkan bahwa itu sama saja seolah-olah kamu melompat
kembali ke fungsi pada titik itu dengan variabel lokal bernilai apa pun
yang mereka miliki saat pemanggilan `longjmp()` dilakukan.

\index{longjmp() function@\texttt{longjmp()} function|)}

Hal-hal yang tidak dipulihkan, memparafrasekan spesifikasi, mencakup:

* Flag status floating point
* File yang terbuka
* Komponen lain apa pun dari abstract machine

### Kamu Tidak Boleh Menamai Apa Pun `setjmp`

Kamu tidak boleh punya identifier `extern` bernama `setjmp`. Atau, kalau
`setjmp` adalah makro, kamu tidak boleh meng-undefine-nya.

Keduanya undefined behavior.

### `setjmp()` Tidak Boleh Berada dalam Ekspresi yang Lebih Besar

\index{setjmp()@\texttt{setjmp()}!in an expression|(}

Artinya, kamu tidak bisa melakukan sesuatu seperti ini:

``` {.c}
if (x == 12 && setjmp(env) == 0) { ... }
```

Itu terlalu kompleks untuk diizinkan spesifikasi karena rekayasa yang harus
terjadi saat membongkar stack dan sebagainya. Kita tidak bisa
`longjmp()` kembali ke ekspresi kompleks yang baru dieksekusi sebagian.

Jadi ada batas kompleksitas pada ekspresi itu. 

* Ia boleh menjadi seluruh ekspresi pengendali dari kondisional.

  ``` {.c}
  if (setjmp(env)) {...}
  ```

  ``` {.c}
  switch (setjmp(env)) {...}
  ```

* Ia boleh menjadi bagian dari ekspresi relasional atau kesamaan, selama
  operand lainnya adalah konstanta integer. Dan keseluruhannya adalah
  ekspresi pengendali dari kondisional.

  ``` {.c}
  if (setjmp(env) == 0) {...}
  ```

* Operand dari operasi logical NOT (`!`), yang menjadi seluruh ekspresi
  pengendali.

  ``` {.c}
  if (!setjmp(env)) {...}
  ```

* Ekspresi mandiri, mungkin di-cast ke `void`.

  ``` {.c}
  setjmp(env);
  ```
  ``` {.c}
  (void)setjmp(env);
  ```

\index{setjmp()@\texttt{setjmp()}!in an expression|)}

### Kapan `longjmp()` Tidak Boleh Dilakukan?

\index{lonjmp()@\texttt{lonjmp()}|(}

Ini undefined behavior kalau:

* Kamu belum memanggil `setjmp()` sebelumnya
* Kamu memanggil `setjmp()` dari thread lain
* Kamu memanggil `setjmp()` di scope sebuah variable length array (VLA),
  dan eksekusi sudah meninggalkan scope VLA itu sebelum `longjmp()`
  dipanggil.
* Fungsi yang berisi `setjmp()` sudah keluar sebelum `longjmp()`
  dipanggil.

Untuk yang terakhir, "keluar" mencakup return normal dari fungsi itu,
serta kasus ketika `longjmp()` lain sudah melompat kembali ke titik yang
"lebih awal" di call stack daripada fungsi yang dimaksud.

### Kamu Tidak Boleh Mengoper `0` ke `longjmp()`

Kalau kamu mencoba mengoper nilai `0` ke `longjmp()`, nilai itu akan diam-diam
diubah menjadi `1`.

Karena `setjmp()` pada akhirnya mengembalikan nilai ini, dan
`setjmp()` yang mengembalikan `0` punya makna khusus, mengembalikan `0`
dilarang.

### `longjmp()` dan Variable Length Array

Kalau kamu berada di scope sebuah VLA dan `longjmp()` keluar dari sana,
memori yang dialokasikan untuk VLA itu bisa bocor^[Artinya, tetap
teralokasi sampai program berakhir tanpa cara untuk membebaskannya.].

Hal yang sama terjadi kalau kamu `longjmp()` melewati fungsi-fungsi
sebelumnya yang masih punya VLA dalam scope.

Ini salah satu hal yang benar-benar mengganggu aku soal VLA---kamu
bisa menulis kode C yang sepenuhnya sah tapi menyia-nyiakan memori. Tapi
ya, aku bukan pemegang spesifikasi.

\index{lonjmp()@\texttt{lonjmp()}|)}
\index{Long jumps|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Incomplete Type

\index{Incomplete types|(}

Mungkin mengejutkan bahwa kode ini dibangun tanpa error:

``` {.c}
extern int a[];

int main(void)
{
    struct foo *x;
    union bar *y;
    enum baz *z;
}
```

Kita tidak pernah memberi ukuran untuk `a`. Dan kita punya pointer ke
`struct` `foo`, `bar`, dan `baz` yang tampaknya tidak dideklarasikan di
mana pun.

Dan satu-satunya peringatan yang kudapat hanyalah bahwa `x`, `y`, dan `z`
tidak dipakai.

Inilah contoh _incomplete type_.

Incomplete type adalah tipe yang ukurannya (yaitu ukuran yang akan kamu
dapat dari `sizeof`) tidak diketahui. Cara lain memikirkannya: tipe yang
belum selesai kamu deklarasikan.

Kamu bisa punya pointer ke incomplete type, tapi tidak bisa
men-dereference-nya atau memakai aritmetika pointer padanya. Dan kamu
tidak bisa meng-`sizeof`-nya.

Jadi apa yang bisa dilakukan dengannya?

## Kasus Pemakaian: Struktur yang Merujuk Diri Sendiri

\index{Incomplete types!self-referential structs@self-referential \texttt{struct}s|(}

Aku hanya tahu satu kasus pemakaian yang nyata: forward reference ke
`struct` atau `union` dengan struktur yang merujuk diri sendiri atau saling
bergantung. (Untuk sisa contoh, aku akan memakai `struct`, tapi semuanya
berlaku sama untuk `union`.)

Kita kerjakan contoh klasiknya dulu.

Tapi sebelum aku mulai, ketahuilah ini! Saat kamu mendeklarasikan sebuah
`struct`, `struct` itu incomplete sampai kurung kurawal penutup tercapai!

``` {.c}
struct antelope {              // struct antelope incomplete di sini
    int leg_count;             // Masih incomplete
    float stomach_fullness;    // Masih incomplete
    float top_speed;           // Masih incomplete
    char *nickname;            // Masih incomplete
};                             // SEKARANG sudah complete.
```

Lalu kenapa? Kelihatannya cukup waras.

Tapi bagaimana kalau kita membuat linked list? Setiap node linked list
perlu punya referensi ke node lain. Tapi bagaimana kita membuat referensi
ke node lain kalau deklarasi node saja belum selesai?

Kelonggaran C untuk incomplete type memungkinkannya. Kita tidak bisa
mendeklarasikan sebuah node, tapi _bisa_ mendeklarasikan pointer ke node,
walaupun ia incomplete!

``` {.c}
struct node {
    int val;
    struct node *next;  // struct node incomplete, tapi tidak masalah!
};
```

Meskipun `struct node` incomplete di baris 3, kita tetap bisa
mendeklarasikan pointer ke sana^[Ini bisa karena di C, pointer berukuran
sama apa pun tipe data yang ditunjuknya. Jadi compiler tidak perlu tahu
ukuran `struct node` pada titik ini; ia hanya perlu tahu ukuran
pointer.].

Hal yang sama bisa dilakukan kalau kita punya dua `struct` berbeda yang
saling merujuk:

``` {.c}
struct a {
    struct b *x;  // Merujuk ke `struct b`
};

struct b {
    struct a *x;  // Merujuk ke `struct a`
};
```

Kita tidak akan pernah bisa membuat pasangan struktur itu tanpa aturan
yang dilonggarkan untuk incomplete type.

\index{Incomplete types!self-referential structs@self-referential \texttt{struct}s|)}

## Pesan Error Incomplete Type

Apakah kamu mendapat error seperti ini?

``` {.default}
invalid application of ‘sizeof’ to incomplete type

invalid use of undefined type

dereferencing pointer to incomplete type
```

Tersangka paling mungkin: kamu kemungkinan lupa meng-`#include` header file
yang mendeklarasikan tipe itu.

## Incomplete Type Lainnya

Mendeklarasikan `struct` atau `union` tanpa badan membuat incomplete type,
mis. `struct foo;`.

`enum` incomplete sampai kurung kurawal penutup.

`void` adalah incomplete type.

Array yang dideklarasikan `extern` tanpa ukuran adalah incomplete, mis.:

``` {.c}
extern int a[];
```

Kalau ia array non-`extern` tanpa ukuran yang diikuti initializer, ia
incomplete sampai kurung kurawal penutup initializer.

## Kasus Pemakaian: Array di Header File

Berguna mendeklarasikan tipe array incomplete di header file. Dalam kasus
itu, penyimpanan sebenarnya (tempat array lengkap dideklarasikan) harus ada
di satu file `.c`. Kalau ditaruh di file `.h`, ia akan diduplikasi setiap
kali header file di-include.

Jadi yang bisa dilakukan adalah membuat header file dengan incomplete type
yang merujuk ke array, seperti ini:

``` {.c .numberLines}
// File: bar.h

#ifndef BAR_H
#define BAR_H

extern int my_array[];  // Incomplete type

#endif
```

Lalu di file `.c`, definisikan array-nya yang sebenarnya:

``` {.c .numberLines}
// File: bar.c

int my_array[1024];     // Complete type!
```

Setelah itu kamu bisa meng-include header itu dari sebanyak mungkin tempat
yang kamu mau, dan setiap tempat itu akan merujuk ke `my_array` yang sama.

``` {.c .numberLines}
// File: foo.c

#include <stdio.h>
#include "bar.h"    // menyertakan incomplete type untuk my_array

int main(void)
{
    my_array[0] = 10;

    printf("%d\n", my_array[0]);
}
```

Saat mengompilasi banyak file, ingat untuk menyebutkan semua file `.c` ke
compiler, tapi bukan file `.h`, mis.:

``` {.zsh}
gcc -o foo foo.c bar.c
```

## Melengkapi Incomplete Type

Kalau kamu punya incomplete type, kamu bisa melengkapinya dengan
mendefinisikan `struct`, `union`, `enum`, atau array yang lengkap di scope
yang sama.

``` {.c}
struct foo;        // incomplete type

struct foo *p;     // pointer, tidak masalah

// struct foo f;   // Error: incomplete type!

struct foo {
    int x, y, z;
};                 // Sekarang struct foo sudah complete!

struct foo f;      // Berhasil!
```

Perhatikan bahwa walaupun `void` adalah incomplete type, tidak ada cara
untuk melengkapinya. Bukan berarti ada yang pernah terpikir melakukan hal
aneh itu. Tapi ini menjelaskan kenapa kamu bisa melakukan ini:

``` {.c}
void *p;             // OK: pointer ke incomplete type
```

dan tidak salah satu dari dua ini:

``` {.c}
void v;              // Error: mendeklarasikan variabel bertipe incomplete

printf("%d\n", *p);  // Error: men-dereference incomplete type
```

Makin tahu aja...

\index{Incomplete types|(}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Bilangan Kompleks

\index{Complex numbers|(}

Primer singkat tentang [bilangan kompleks](https://en.wikipedia.org/wiki/Complex_number)^[https://en.wikipedia.org/wiki/Complex_number] yang dicuri
langsung dari Wikipedia:

> A **complex number** is a number that can be expressed in the form
> $a+bi$, where $a$ and $b$ are real numbers [i.e. floating point types
> in C], and $i$ represents the imaginary unit, satisfying the equation
> $i^2=−1$. Because no real number satisfies this equation, $i$ is
> called an imaginary number. For the complex number $a+bi$, $a$ is
> called the **real part**, and $b$ is called the **imaginary part**.

Tapi hanya sampai di situ yang akan kujelaskan. Kita anggap kalau kamu membaca bab
ini, kamu sudah tahu apa itu bilangan kompleks dan apa yang ingin kamu
lakukan dengannya.

Yang perlu kita bahas hanyalah kemampuan C untuk itu.

Ternyata, dukungan bilangan kompleks di compiler adalah fitur
_opsional_. Tidak semua compiler yang patuh standar bisa melakukannya. Dan
yang bisa pun mungkin mendukungnya dengan tingkat kelengkapan yang
berbeda-beda.

Kamu bisa menguji apakah sistemmu mendukung bilangan kompleks dengan:

\index{\_\_STDC\_NO\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_NO\_COMPLEX\_\_} macro|(}

``` {.c}
#ifdef __STDC_NO_COMPLEX__
#error Complex numbers not supported!
#endif
```

\index{\_\_STDC\_NO\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_NO\_COMPLEX\_\_} macro|)}

\index{\_\_STDC\_IEC\_559\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_COMPLEX\_\_} macro|(}

Selain itu, ada makro yang menandakan kepatuhan pada standar ISO 60559
(IEEE 754) untuk matematika floating point dengan bilangan kompleks, serta
adanya tipe `_Imaginary`.

``` {.c}
#if __STDC_IEC_559_COMPLEX__ != 1
#error Need IEC 60559 complex support!
#endif
```

\index{\_\_STDC\_IEC\_559\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_COMPLEX\_\_} macro|)}

Detail lebih lanjut dijabarkan di Annex G spesifikasi C11.

## Tipe Kompleks

Untuk memakai bilangan kompleks, \index{complex.h header file@\texttt{complex.h} header file} `#include
<complex.h>`.

Dengan itu, kamu mendapat setidaknya dua tipe:

\index{\_Complex type@\texttt{\_Complex} type}
\index{complex type@\texttt{complex} type|(}

``` {.c}
_Complex
complex
```

Keduanya berarti sama, jadi sebaiknya pakai `complex` yang lebih cantik.

\index{complex type@\texttt{complex} type|)}

Kamu juga mendapat beberapa tipe untuk bilangan imajiner kalau
implementasimu patuh IEC 60559:

\index{\_Imaginary type@\texttt{\_Imaginary} type}
\index{imaginary type@\texttt{imaginary} type|(}

``` {.c}
_Imaginary
imaginary
```

Ini juga keduanya berarti sama, jadi sebaiknya pakai `imaginary` yang lebih
cantik.

\index{imaginary type@\texttt{imaginary} type|)}

Kamu juga mendapat nilai untuk bilangan imajiner $i$ itu sendiri:

\index{I macro@\texttt{I} macro|(}
\index{\_Complex\_I macro@\texttt{\_Complex\_I} macro|(}
\index{\_Imaginary\_I macro@\texttt{\_Imaginary\_I} macro|(}

``` {.c}
I
_Complex_I
_Imaginary_I
```

Makro `I` diatur menjadi `_Imaginary_I` (kalau tersedia), atau
`_Complex_I`. Jadi pakai saja `I` untuk bilangan imajiner.

\index{I macro@\texttt{I} macro|)}
\index{\_Complex\_I macro@\texttt{\_Complex\_I} macro|)}

\index{\_\_STDC\_IEC\_559\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_COMPLEX\_\_} macro|(}

Satu catatan sampingan: tadi kubilang bahwa kalau compiler punya
`__STDC_IEC_559_COMPLEX__` bernilai `1`, ia harus mendukung tipe
`_Imaginary` agar patuh. Itu bacaanku atas spesifikasi. Namun,
aku tidak tahu satu pun compiler yang benar-benar mendukung
`_Imaginary` meskipun `__STDC_IEC_559_COMPLEX__`-nya diatur. Jadi aku
akan menulis sebagian kode dengan tipe itu di sini tanpa bisa mengujinya.
Maaf!

\index{\_\_STDC\_IEC\_559\_COMPLEX\_\_ macro@\texttt{\_\_STDC\_IEC\_559\_COMPLEX\_\_} macro|)}
\index{\_Imaginary\_I macro@\texttt{\_Imaginary\_I} macro|)}

Oke, sekarang kita tahu ada tipe `complex`, bagaimana cara memakainya?

## Menugaskan Bilangan Kompleks

\index{Complex numbers!declaring|(}

Karena bilangan kompleks punya bagian real dan imajiner, dan keduanya
bergantung pada bilangan floating point untuk menyimpan nilai, kita juga
perlu memberi tahu C presisi apa yang dipakai untuk bagian-bagian bilangan
kompleks itu.

\index{complex float type@\texttt{complex float} type|(}
\index{complex double type@\texttt{complex double} type|(}
\index{complex long double type@\texttt{complex long double} type|(}

Caranya, tinggal menyematkan `float`, `double`, atau `long double` pada
`complex`, baik sebelum maupun sesudahnya.

\index{complex long double type@\texttt{complex long double} type|)}

Mari kita definisikan bilangan kompleks yang memakai `float` untuk
komponennya:

``` {.c}
float complex c;   // Spesifikasi lebih suka cara ini
complex float c;   // Sama saja--urutan tidak berpengaruh
```

Itu bagus untuk deklarasi, tapi bagaimana cara menginisialisasi atau
menugaskannya?

Ternyata kita bisa memakai notasi yang cukup alami. Contoh!

\index{I macro@\texttt{I} macro|(}

``` {.c}
double complex x = 5 + 2*I;
double complex y = 10 + 3*I;
```

\index{I macro@\texttt{I} macro|)}

Masing-masing untuk $5+2i$ dan $10+3i$.

\index{complex float type@\texttt{complex float} type|)}
\index{complex double type@\texttt{complex double} type|)}
\index{Complex numbers!declaring|)}

## Membentuk, Membongkar, dan Mencetak

Kita hampir sampai...

Kita sudah melihat satu cara menulis bilangan kompleks:

``` {.c}
double complex x = 5 + 2*I;
```

Tidak masalah juga memakai bilangan floating point lain untuk
membentuknya:

``` {.c}
double a = 5;
double b = 2;
double complex x = a + b*I;
```

\index{CMPLX() macro@\texttt{CMPLX()} macro|(}

Ada juga sekumpulan makro untuk membantu membentuknya. Kode di atas bisa
ditulis memakai makro `CMPLX()`, seperti ini:

``` {.c}
double complex x = CMPLX(5, 2);
```

Sejauh yang kutemukan dalam risetku, keduanya _hampir_ setara:

``` {.c}
double complex x = 5 + 2*I;
double complex x = CMPLX(5, 2);
```

Tapi makro `CMPLX()` akan selalu menangani nol negatif pada bagian imajiner
dengan benar, sedangkan cara yang lain mungkin mengonversinya menjadi nol
positif. Aku _kira_^[Yang satu ini lebih sulit diriset, dan aku
menerima informasi tambahan dari siapa pun. `I` bisa didefinisikan sebagai
`_Complex_I` atau `_Imaginary_I`, kalau yang terakhir ada. `_Imaginary_I`
akan menangani nol bertanda, tapi `_Complex_I` _mungkin_ tidak. Ini punya
implikasi pada branch cut dan hal-hal lain yang berbau
matematika-kompleks. Mungkin. Kelihatan kan aku mulai keluar dari zona
nyamanku? Bagaimanapun, makro `CMPLX()` berperilaku seolah-olah `I`
didefinisikan sebagai `_Imaginary_I`, dengan nol bertanda, bahkan kalau
`_Imaginary_I` tidak ada di sistem.] ini menyiratkan bahwa kalau ada
kemungkinan bagian imajinernya nol, sebaiknya pakai makro... tapi
seseorang tolong koreksi kalau aku keliru!

Makro `CMPLX()` bekerja pada tipe `double`. Ada dua makro lain untuk
`float` dan `long double`: \index{CMPLXF() macro@\texttt{CMPLXF()} macro} `CMPLXF()` dan
\index{CMPLXL() macro@\texttt{CMPLXL()} macro} `CMPLXL()`. (Sufiks "f" dan "l" ini muncul di
hampir semua fungsi terkait bilangan kompleks.)

\index{CMPLX() macro@\texttt{CMPLX()} macro|)}

Sekarang kita coba kebalikannya: kalau kita punya bilangan kompleks,
bagaimana memecahnya menjadi bagian real dan imajinernya?

\index{creal() function@\texttt{creal()} function|(}
\index{cimag() function@\texttt{cimag()} function|(}

Ada beberapa fungsi yang mengambil bagian real dan imajiner dari angka itu:
`creal()` dan `cimag()`:

``` {.c}
double complex x = 5 + 2*I;
double complex y = 10 + 3*I;

printf("x = %f + %fi\n", creal(x), cimag(x));
printf("y = %f + %fi\n", creal(y), cimag(y));
```

dengan output:

``` {.default}
x = 5.000000 + 2.000000i
y = 10.000000 + 3.000000i
```

Perhatikan bahwa `i` yang kupakai pada format string `printf()` adalah `i` literal yang
dicetak---ia bukan bagian dari format specifier. Kedua nilai kembalian
`creal()` dan `cimag()` bertipe `double`.

Dan seperti biasa, ada varian `float` dan `long double` dari fungsi-fungsi
ini: \index{crealf() function@\texttt{crealf()} function} `crealf()`, \index{cimagf() function@\texttt{cimagf()} function}
`cimagf()`, \index{creall() function@\texttt{creall()} function} `creall()`, dan \index{cimagl() function@\texttt{cimagl()} function} `cimagl()`.

\index{creal() function@\texttt{creal()} function|)}
\index{cimag() function@\texttt{cimag()} function|)}

## Aritmetika dan Perbandingan Kompleks

\index{Complex numbers!arithmetic|(}

Aritmetika bisa dilakukan pada bilangan kompleks, walaupun cara kerjanya
secara matematis di luar cakupan panduan ini.

``` {.c .numberLines}
#include <stdio.h>
#include <complex.h>

int main(void)
{
    double complex x = 1 + 2*I;
    double complex y = 3 + 4*I;
    double complex z;

    z = x + y;
    printf("x + y = %f + %fi\n", creal(z), cimag(z));

    z = x - y;
    printf("x - y = %f + %fi\n", creal(z), cimag(z));

    z = x * y;
    printf("x * y = %f + %fi\n", creal(z), cimag(z));

    z = x / y;
    printf("x / y = %f + %fi\n", creal(z), cimag(z));
}
```

dengan hasil:

``` {.default}
x + y = 4.000000 + 6.000000i
x - y = -2.000000 + -2.000000i
x * y = -5.000000 + 10.000000i
x / y = 0.440000 + 0.080000i
```

Kamu juga bisa membandingkan dua bilangan kompleks untuk kesamaan (atau
ketidaksamaan):

``` {.c .numberLines}
#include <stdio.h>
#include <complex.h>

int main(void)
{
    double complex x = 1 + 2*I;
    double complex y = 3 + 4*I;

    printf("x == y = %d\n", x == y);  // 0
    printf("x != y = %d\n", x != y);  // 1
}
```

dengan output:

``` {.default}
x == y = 0
x != y = 1
```

Keduanya sama kalau kedua komponennya sama. Perhatikan bahwa seperti pada
semua floating point, keduanya bisa dianggap sama kalau cukup dekat karena
galat pembulatan^[Kesederhanaan pernyataan ini tidak mencerminkan
besarnya upaya yang dibutuhkan hanya untuk memahami bagaimana floating
point sebenarnya bekerja.
https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/].

\index{Complex numbers!arithmetic|)}

## Matematika Kompleks

Tapi tunggu! Ada lebih dari sekadar aritmetika kompleks sederhana!

Berikut tabel ringkasan semua fungsi matematika yang tersedia untuk
bilangan kompleks.

Aku hanya mencantumkan versi `double` dari setiap fungsi, tapi untuk
semuanya ada versi `float` yang didapat dengan menambahkan `f` pada nama
fungsi, dan versi `long double` dengan menambahkan `l`.

Misalnya, fungsi `cabs()` untuk menghitung nilai mutlak bilangan kompleks
juga punya varian `cabsf()` dan `cabsl()`. Aku menghilangkannya demi
keringkasan.

### Fungsi Trigonometri

\index{ccos() function@\texttt{ccos()} function}
\index{csin() function@\texttt{csin()} function}
\index{ctan() function@\texttt{ctan()} function}
\index{cacos() function@\texttt{cacos()} function}
\index{casin() function@\texttt{casin()} function}
\index{catan() function@\texttt{catan()} function}
\index{ccosh() function@\texttt{ccosh()} function}
\index{csinh() function@\texttt{csinh()} function}
\index{ctanh() function@\texttt{ctanh()} function}
\index{cacosh() function@\texttt{cacosh()} function}
\index{casinh() function@\texttt{casinh()} function}
\index{catanh() function@\texttt{catanh()} function}

|Fungsi|Deskripsi|
|-|-|
|`ccos()`|Kosinus|
|`csin()`|Sinus|
|`ctan()`|Tangen|
|`cacos()`|Arc kosinus|
|`casin()`|Arc sinus|
|`catan()`|Bermain _Settlers of Catan_|
|`ccosh()`|Kosinus hiperbolik|
|`csinh()`|Sinus hiperbolik|
|`ctanh()`|Tangen hiperbolik|
|`cacosh()`|Arc kosinus hiperbolik|
|`casinh()`|Arc sinus hiperbolik|
|`catanh()`|Arc tangen hiperbolik|

### Fungsi Eksponensial dan Logaritmik

\index{cexp() function@\texttt{cexp()} function}
\index{clog() function@\texttt{clog()} function}

|Fungsi|Deskripsi|
|-|-|
|`cexp()`|Eksponensial basis-$e$|
|`clog()`|Logaritma natural (basis-$e$)|

### Fungsi Pangkat dan Nilai Mutlak

\index{cabs() function@\texttt{cabs()} function}
\index{cpow() function@\texttt{cpow()} function}
\index{csqrt() function@\texttt{csqrt()} function}

|Fungsi|Deskripsi|
|-|-|
|`cabs()`|Nilai mutlak|
|`cpow()`|Pangkat|
|`csqrt()`|Akar kuadrat|

### Fungsi Manipulasi

\index{creal() function@\texttt{creal()} function}
\index{cimag() function@\texttt{cimag()} function}
\index{CMPLX() macro@\texttt{CMPLX()} macro}
\index{carg() function@\texttt{carg()} function}
\index{conj() function@\texttt{conj()} function}
\index{cproj() function@\texttt{cproj()} function}

|Fungsi|Deskripsi|
|-|-|
|`creal()`|Mengembalikan bagian real|
|`cimag()`|Mengembalikan bagian imajiner|
|`CMPLX()`|Membentuk bilangan kompleks|
|`carg()`|Argumen/sudut fase|
|`conj()`|Konjugat[^4a34]|
|`cproj()`|Proyeksi pada bola Riemann|

[^4a34]: Ini satu-satunya yang tidak diawali `c` tambahan, anehnya.

\index{Complex numbers|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Tipe Integer Berlebar Tetap

\index{Fixed width integers|(}

C punya semua tipe integer kecil, lebih besar, dan terbesar seperti `int`
dan `long` dan sebagainya. Dan kamu bisa melihat [bagian tentang
batas](#limits-macros) untuk mengetahui `int` terbesar dengan `INT_MAX`
dan seterusnya.

Seberapa besar tipe-tipe itu? Artinya, berapa byte yang mereka pakai?
Kita bisa memakai `sizeof` untuk mendapat jawabannya.

Tapi bagaimana kalau aku ingin kebalikannya? Bagaimana kalau aku butuh
tipe yang persis 32 bit (4 byte) atau setidaknya 16 bit atau semacamnya?

Bagaimana kita mendeklarasikan tipe dengan ukuran tertentu?

Header \index{stdint.h header file@\texttt{stdint.h} header file} `<stdint.h>` memberi kita caranya.

## Tipe Berukuran Bit

Untuk integer signed maupun unsigned, kita bisa menentukan tipe yang
berukuran sejumlah bit tertentu, dengan beberapa syarat, tentu saja.

Dan ada tiga kelas utama tipe ini (pada contoh-contoh ini, `N` diganti
dengan jumlah bit tertentu):

* Integer dengan ukuran persis tertentu (`intN_t`)
* Integer dengan ukuran setidaknya tertentu (`int_leastN_t`)
* Integer dengan ukuran setidaknya tertentu dan secepat mungkin
  (`int_fastN_t`)[^4582]

[^4582]: Beberapa arsitektur punya data berukuran berbeda yang bisa
diolah CPU dan RAM dengan laju lebih cepat daripada yang lain. Dalam kasus
itu, kalau kamu butuh angka 8-bit tercepat, mungkin yang diberikan adalah
tipe 16- atau 32-bit karena itu memang lebih cepat. Jadi dengan ini, kamu
tidak akan tahu seberapa besar tipenya, tapi ukurannya setidaknya sebesar
yang kamu sebutkan.

Seberapa lebih cepat `fast` itu? Pasti mungkin agak lebih cepat. Kira-kira.
Spesifikasi tidak menyebut seberapa lebih cepat, hanya bahwa mereka akan
menjadi yang tercepat di arsitektur ini. Kebanyakan C compiler cukup
pandai, jadi kemungkinan ini hanya dipakai di tempat yang butuh jaminan
kecepatan setinggi mungkin (bukan sekadar berharap compiler menghasilkan
kode yang cepat banget, yang memang begitu).

Terakhir, tipe angka unsigned ini punya awalan `u` untuk membedakannya.

Misalnya, tipe-tipe ini punya makna yang tercantum:

\index{int\_leastN\_t types@\texttt{int\_leastN\_t} types|(}
\index{uint\_leastN\_t types@\texttt{uint\_leastN\_t} types|(}
\index{int\_fastN\_t types@\texttt{int\_fastN\_t} types|(}
\index{uint\_fastN\_t types@\texttt{uint\_fastN\_t} types|(}
\index{intN\_t types@\texttt{intN\_t} types|(}
\index{uintN\_t types@\texttt{uintN\_t} types|(}

``` {.c}
int32_t w;        // w persis 32 bit, signed
uint16_t x;       // x persis 16 bit, unsigned

int_least8_t y;   // y setidaknya 8 bit, signed

// z adalah representasi tercepat dengan setidaknya 64 bit, unsigned:
uint_fast64_t z;
```

Tipe-tipe berikut dijamin terdefinisi:

``` {.c}
int_least8_t      uint_least8_t
int_least16_t     uint_least16_t
int_least32_t     uint_least32_t
int_least64_t     uint_least64_t

int_fast8_t       uint_fast8_t
int_fast16_t      uint_fast16_t
int_fast32_t      uint_fast32_t
int_fast64_t      uint_fast64_t
```

Mungkin ada yang lain dengan lebar berbeda, tapi itu opsional.

Hei! Di mana tipe berlebar tetap seperti `int16_t`? Ternyata itu sepenuhnya
opsional...kecuali kondisi tertentu terpenuhi^[Yaitu, sistem punya integer
8, 16, 32, atau 64 bit tanpa padding yang memakai representasi two's
complement, dan dalam kasus itu varian `intN_t` untuk jumlah bit tersebut
_harus_ didefinisikan.]. Dan kalau kamu memakai sistem komputer modern
biasa, kondisi itu kemungkinan terpenuhi. Dan kalau terpenuhi, kamu akan
punya tipe-tipe ini:

``` {.c}
int8_t      uint8_t
int16_t     uint16_t
int32_t     uint32_t
int64_t     uint64_t
```

Varian lain dengan lebar berbeda mungkin didefinisikan, tapi itu opsional.

\index{int\_leastN\_t types@\texttt{int\_leastN\_t} types|)}
\index{uint\_leastN\_t types@\texttt{uint\_leastN\_t} types|)}
\index{int\_fastN\_t types@\texttt{int\_fastN\_t} types|)}
\index{uint\_fastN\_t types@\texttt{uint\_fastN\_t} types|)}
\index{intN\_t types@\texttt{intN\_t} types|)}
\index{uintN\_t types@\texttt{uintN\_t} types|)}

## Tipe Ukuran Integer Maksimum

Ada tipe yang bisa dipakai untuk menampung integer terbesar yang bisa
direpresentasikan di sistem, baik signed maupun unsigned:

\index{intmax\_t type@\texttt{intmax\_t} type|(}
\index{uintmax\_t type@\texttt{uintmax\_t} type|(}

``` {.c}
intmax_t
uintmax_t
```

\index{intmax\_t type@\texttt{intmax\_t} type|)}
\index{uintmax\_t type@\texttt{uintmax\_t} type|)}

Pakai tipe ini kalau kamu ingin sebesar mungkin.

Jelas nilai dari tipe integer lain dengan tanda yang sama pasti muat di
tipe ini.

## Memakai Konstanta Berukuran Tetap

Kalau kamu punya konstanta yang ingin dimuat dalam sejumlah bit tertentu,
kamu bisa memakai makro berikut untuk menambahkan sufiks yang tepat secara
otomatis pada angka itu (mis. `22L` atau `3490ULL`).

\index{INTn\_C() macros@\texttt{INTn\_C()} macros|(}
\index{UINTn\_C() macros@\texttt{UINTn\_C()} macros|(}
\index{INTMAX\_C() macro@\texttt{INTMAX\_C()} macro|(}
\index{UINTMAX\_C() macro@\texttt{UINTMAX\_C()} macro|(}

``` {.c}
INT8_C(x)     UINT8_C(x)
INT16_C(x)    UINT16_C(x)
INT32_C(x)    UINT32_C(x)
INT64_C(x)    UINT64_C(x)
INTMAX_C(x)   UINTMAX_C(x)
```

\index{INTn\_C() macros@\texttt{INTn\_C()} macros|)}
\index{UINTMAX\_C() macro@\texttt{UINTMAX\_C()} macro|)}

Sekali lagi, ini hanya bekerja dengan nilai integer konstan.

Misalnya, kita bisa memakai salah satunya untuk menugaskan nilai konstan
seperti ini:

``` {.c}
uint16_t x = UINT16_C(12);
intmax_t y = INTMAX_C(3490);
```

\index{UINTn\_C() macros@\texttt{UINTn\_C()} macros|)}
\index{INTMAX\_C() macro@\texttt{INTMAX\_C()} macro|)}

## Batas Integer Berukuran Tetap

Kita juga punya beberapa batas yang didefinisikan agar kamu bisa
mendapat nilai maksimum dan minimum untuk tipe-tipe ini:

\index{INTn\_MAX macros@\texttt{INTn\_MAX} macros|(}
\index{INTn\_MIN macros@\texttt{INTn\_MIN} macros|(}
\index{UINTn\_MAX macros@\texttt{UINTn\_MAX} macros|(}
\index{INT\_LEASTn\_MAX macros@\texttt{INT\_LEASTn\_MAX} macros|(}
\index{INT\_LEASTn\_MIN macros@\texttt{INT\_LEASTn\_MIN} macros|(}
\index{UINT\_LEASTn\_MAX macros@\texttt{UINT\_LEASTn\_MAX} macros|(}
\index{INT\_FASTn\_MAX macros@\texttt{INT\_FASTn\_MAX} macros|(}
\index{INT\_FASTn\_MIN macros@\texttt{INT\_FASTn\_MIN} macros|(}
\index{UINT\_FASTn\_MAX macros@\texttt{UINT\_FASTn\_MAX} macros|(}
\index{INTMAX\_MAX macro@\texttt{INTMAX\_MAX} macro|(}
\index{INTMAX\_MIN macro@\texttt{INTMAX\_MIN} macro|(}
\index{UINTMAX\_MAX macro@\texttt{UINTMAX\_MAX} macro|(}

``` {.c}
INT8_MAX           INT8_MIN           UINT8_MAX
INT16_MAX          INT16_MIN          UINT16_MAX
INT32_MAX          INT32_MIN          UINT32_MAX
INT64_MAX          INT64_MIN          UINT64_MAX

INT_LEAST8_MAX     INT_LEAST8_MIN     UINT_LEAST8_MAX
INT_LEAST16_MAX    INT_LEAST16_MIN    UINT_LEAST16_MAX
INT_LEAST32_MAX    INT_LEAST32_MIN    UINT_LEAST32_MAX
INT_LEAST64_MAX    INT_LEAST64_MIN    UINT_LEAST64_MAX

INT_FAST8_MAX      INT_FAST8_MIN      UINT_FAST8_MAX
INT_FAST16_MAX     INT_FAST16_MIN     UINT_FAST16_MAX
INT_FAST32_MAX     INT_FAST32_MIN     UINT_FAST32_MAX
INT_FAST64_MAX     INT_FAST64_MIN     UINT_FAST64_MAX

INTMAX_MAX         INTMAX_MIN         UINTMAX_MAX
```

\index{INTn\_MAX macros@\texttt{INTn\_MAX} macros|)}
\index{UINTn\_MAX macros@\texttt{UINTn\_MAX} macros|)}
\index{INT\_LEASTn\_MAX macros@\texttt{INT\_LEASTn\_MAX} macros|)}
\index{UINT\_LEASTn\_MAX macros@\texttt{UINT\_LEASTn\_MAX} macros|)}
\index{INT\_FASTn\_MAX macros@\texttt{INT\_FASTn\_MAX} macros|)}
\index{UINT\_FASTn\_MAX macros@\texttt{UINT\_FASTn\_MAX} macros|)}
\index{INTMAX\_MAX macro@\texttt{INTMAX\_MAX} macro|)}
\index{UINTMAX\_MAX macro@\texttt{UINTMAX\_MAX} macro|)}

Perhatikan bahwa `MIN` untuk semua tipe unsigned adalah `0`, sehingga tidak
ada makronya.

\index{INTn\_MIN macros@\texttt{INTn\_MIN} macros|)}
\index{INT\_LEASTn\_MIN macros@\texttt{INT\_LEASTn\_MIN} macros|)}
\index{INT\_FASTn\_MIN macros@\texttt{INT\_FASTn\_MIN} macros|)}
\index{INTMAX\_MIN macro@\texttt{INTMAX\_MIN} macro|)}

## Format Specifier

Untuk mencetak tipe-tipe ini, kamu perlu mengirim format specifier yang
tepat ke \index{printf() function@\texttt{printf()} function} `printf()`. (Dan masalah yang sama untuk
mengambil input dengan \index{scanf() function@\texttt{scanf()} function} `scanf()`.)

Tapi bagaimana kamu akan tahu ukuran tipe di baliknya? Untungnya, sekali
lagi, C menyediakan beberapa makro untuk membantu.

Semuanya ada di `<inttypes.h>`.

Nah, ada banyak sekali makro. Seperti ledakan kompleksitas makro. Jadi
aku akan berhenti mendaftar satu per satu dan cukup menaruh huruf
kecil `n` di tempat kamu seharusnya menaruh `8`, `16`, `32`, atau `64`
sesuai kebutuhan.

Mari kita lihat makro untuk mencetak integer signed:

\index{PRIdn macros@\texttt{PRIdn} macros|(}
\index{PRIin macros@\texttt{PRIin} macros|(}
\index{PRIdLEASTn macros@\texttt{PRIdLEASTn} macros|(}
\index{PRIiLEASTn macros@\texttt{PRIiLEASTn} macros|(}
\index{PRIdFASTn macros@\texttt{PRIdFASTn} macros|(}
\index{PRIiFASTn macros@\texttt{PRIiFASTn} macros|(}
\index{PRIdMAX macro@\texttt{PRIdMAX} macro|(}
\index{PRIiMAX macro@\texttt{PRIiMAX} macro|(}

``` {.c}
PRIdn    PRIdLEASTn    PRIdFASTn    PRIdMAX
PRIin    PRIiLEASTn    PRIiFASTn    PRIiMAX
```

Cari polanya. Kamu bisa melihat ada varian untuk tipe fixed, least, fast,
dan max.

Dan ada juga `d` huruf kecil dan `i` huruf kecil. Keduanya sesuai dengan
format specifier `printf()` `%d` dan `%i`.

Jadi kalau aku punya sesuatu bertipe:

``` {.c}
int_least16_t x = 3490;
```

Aku bisa mencetaknya dengan padanan format specifier `%d` dengan memakai
`PRIdLEAST16`.

Tapi bagaimana caranya? Bagaimana memakai makro itu?

Pertama, makro itu menentukan string berisi huruf atau huruf-huruf yang
dibutuhkan `printf()` untuk mencetak tipe itu. Misalnya, bisa berupa `"d"`
atau `"ld"`.

Jadi yang perlu kita lakukan hanyalah menyematkannya ke format string pada
pemanggilan `printf()`.

Untuk itu, kita bisa memanfaatkan fakta tentang C yang mungkin sudah kamu
lupakan: string literal yang bersebelahan otomatis digabung menjadi satu
string. Misalnya:

``` {.c}
printf("Hello, " "world!\n");   // Mencetak "Hello, world!"
```

Dan karena makro-makro ini adalah string literal, kita bisa memakainya
seperti ini:

``` {.c .numberLines}
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void)
{
    int_least16_t x = 3490;

    printf("The value is %" PRIdLEAST16 "!\n", x);
}
```

\index{PRIdn macros@\texttt{PRIdn} macros|)}
\index{PRIin macros@\texttt{PRIin} macros|)}
\index{PRIdLEASTn macros@\texttt{PRIdLEASTn} macros|)}
\index{PRIiLEASTn macros@\texttt{PRIiLEASTn} macros|)}
\index{PRIdFASTn macros@\texttt{PRIdFASTn} macros|)}
\index{PRIiFASTn macros@\texttt{PRIiFASTn} macros|)}
\index{PRIdMAX macro@\texttt{PRIdMAX} macro|)}
\index{PRIiMAX macro@\texttt{PRIiMAX} macro|)}

Kita juga punya setumpuk makro untuk mencetak tipe unsigned:

\index{PRIon macros@\texttt{PRIon} macros|(}
\index{PRIun macros@\texttt{PRIun} macros|(}
\index{PRIxn macros@\texttt{PRIxn} macros|(}
\index{PRIXn macros@\texttt{PRIXn} macros|(}
\index{PRIoLEASTn macros@\texttt{PRIoLEASTn} macros|(}
\index{PRIuLEASTn macros@\texttt{PRIuLEASTn} macros|(}
\index{PRIxLEASTn macros@\texttt{PRIxLEASTn} macros|(}
\index{PRIXLEASTn macros@\texttt{PRIXLEASTn} macros|(}
\index{PRIoFASTn macros@\texttt{PRIoFASTn} macros|(}
\index{PRIuFASTn macros@\texttt{PRIuFASTn} macros|(}
\index{PRIxFASTn macros@\texttt{PRIxFASTn} macros|(}
\index{PRIXFASTn macros@\texttt{PRIXFASTn} macros|(}
\index{PRIoMAX macros@\texttt{PRIoMAX} macros|(}
\index{PRIuMAX macros@\texttt{PRIuMAX} macros|(}
\index{PRIxMAX macros@\texttt{PRIxMAX} macros|(}
\index{PRIXMAX macros@\texttt{PRIXMAX} macros|(}

``` {.c}
PRIon    PRIoLEASTn    PRIoFASTn    PRIoMAX
PRIun    PRIuLEASTn    PRIuFASTn    PRIuMAX
PRIxn    PRIxLEASTn    PRIxFASTn    PRIxMAX
PRIXn    PRIXLEASTn    PRIXFASTn    PRIXMAX
```

Dalam kasus ini, `o`, `u`, `x`, dan `X` sesuai dengan format specifier
yang terdokumentasi di `printf()`.

Dan, seperti tadi, huruf kecil `n` harus diganti dengan `8`, `16`, `32`,
atau `64`.

\index{PRIon macros@\texttt{PRIon} macros|)}
\index{PRIun macros@\texttt{PRIun} macros|)}
\index{PRIxn macros@\texttt{PRIxn} macros|)}
\index{PRIXn macros@\texttt{PRIXn} macros|)}
\index{PRIoLEASTn macros@\texttt{PRIoLEASTn} macros|)}
\index{PRIuLEASTn macros@\texttt{PRIuLEASTn} macros|)}
\index{PRIxLEASTn macros@\texttt{PRIxLEASTn} macros|)}
\index{PRIXLEASTn macros@\texttt{PRIXLEASTn} macros|)}
\index{PRIoFASTn macros@\texttt{PRIoFASTn} macros|)}
\index{PRIuFASTn macros@\texttt{PRIuFASTn} macros|)}
\index{PRIxFASTn macros@\texttt{PRIxFASTn} macros|)}
\index{PRIXFASTn macros@\texttt{PRIXFASTn} macros|)}
\index{PRIoMAX macros@\texttt{PRIoMAX} macros|)}
\index{PRIuMAX macros@\texttt{PRIuMAX} macros|)}
\index{PRIxMAX macros@\texttt{PRIxMAX} macros|)}
\index{PRIXMAX macros@\texttt{PRIXMAX} macros|)}

Tapi tepat ketika kamu mengira sudah cukup dengan makro, ternyata kita
punya satu set pelengkap lengkap untuk \index{scanf() function@\texttt{scanf()} function} `scanf()`!

\index{SCNdn macros@\texttt{SCNdn} macros|(}
\index{SCNin macros@\texttt{SCNin} macros|(}
\index{SCNon macros@\texttt{SCNon} macros|(}
\index{SCNun macros@\texttt{SCNun} macros|(}
\index{SCNxn macros@\texttt{SCNxn} macros|(}
\index{SCNdLEASTn macros@\texttt{SCNdLEASTn} macros|(}
\index{SCNiLEASTn macros@\texttt{SCNiLEASTn} macros|(}
\index{SCNoLEASTn macros@\texttt{SCNoLEASTn} macros|(}
\index{SCNuLEASTn macros@\texttt{SCNuLEASTn} macros|(}
\index{SCNxLEASTn macros@\texttt{SCNxLEASTn} macros|(}
\index{SCNdFASTn macros@\texttt{SCNdFASTn} macros|(}
\index{SCNiFASTn macros@\texttt{SCNiFASTn} macros|(}
\index{SCNoFASTn macros@\texttt{SCNoFASTn} macros|(}
\index{SCNuFASTn macros@\texttt{SCNuFASTn} macros|(}
\index{SCNxFASTn macros@\texttt{SCNxFASTn} macros|(}
\index{SCNdMAX macros@\texttt{SCNdMAX} macros|(}
\index{SCNiMAX macros@\texttt{SCNiMAX} macros|(}
\index{SCNoMAX macros@\texttt{SCNoMAX} macros|(}
\index{SCNuMAX macros@\texttt{SCNuMAX} macros|(}
\index{SCNxMAX macros@\texttt{SCNxMAX} macros|(}

``` {.c}
SCNdn    SCNdLEASTn    SCNdFASTn    SCNdMAX
SCNin    SCNiLEASTn    SCNiFASTn    SCNiMAX
SCNon    SCNoLEASTn    SCNoFASTn    SCNoMAX
SCNun    SCNuLEASTn    SCNuFASTn    SCNuMAX
SCNxn    SCNxLEASTn    SCNxFASTn    SCNxMAX
```

\index{SCNdn macros@\texttt{SCNdn} macros|)}
\index{SCNin macros@\texttt{SCNin} macros|)}
\index{SCNon macros@\texttt{SCNon} macros|)}
\index{SCNun macros@\texttt{SCNun} macros|)}
\index{SCNxn macros@\texttt{SCNxn} macros|)}
\index{SCNdLEASTn macros@\texttt{SCNdLEASTn} macros|)}
\index{SCNiLEASTn macros@\texttt{SCNiLEASTn} macros|)}
\index{SCNoLEASTn macros@\texttt{SCNoLEASTn} macros|)}
\index{SCNuLEASTn macros@\texttt{SCNuLEASTn} macros|)}
\index{SCNxLEASTn macros@\texttt{SCNxLEASTn} macros|)}
\index{SCNdFASTn macros@\texttt{SCNdFASTn} macros|)}
\index{SCNiFASTn macros@\texttt{SCNiFASTn} macros|)}
\index{SCNoFASTn macros@\texttt{SCNoFASTn} macros|)}
\index{SCNuFASTn macros@\texttt{SCNuFASTn} macros|)}
\index{SCNxFASTn macros@\texttt{SCNxFASTn} macros|)}
\index{SCNdMAX macros@\texttt{SCNdMAX} macros|)}
\index{SCNiMAX macros@\texttt{SCNiMAX} macros|)}
\index{SCNoMAX macros@\texttt{SCNoMAX} macros|)}
\index{SCNuMAX macros@\texttt{SCNuMAX} macros|)}
\index{SCNxMAX macros@\texttt{SCNxMAX} macros|)}

Ingat: saat ingin mencetak tipe integer berukuran tetap dengan `printf()`
atau `scanf()`, ambil format specifier yang sesuai dari `<inttypes.h>`.

\index{Fixed width integers|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Fungsionalitas Tanggal dan Waktu

> "Time is an illusion. Lunchtime doubly so." \
> ---Ford Prefect, The Hitchhikers Guide to the Galaxy

\index{Date and time|(}

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

* \index{Universal Coordinated Time} **UTC**: Coordinated Universal Time
  adalah waktu absolut yang disepakati secara universal^[Di Bumi, setidaknya.
  Siapa tahu sistem gila apa yang dipakai _di luar sana_...]. Semua orang
  di planet ini menganggap sekarang waktunya sama di UTC... meskipun
  waktu lokal mereka berbeda.

* \index{Greenwich Mean time} **GMT**: Greenwich Mean Time, secara efektif
  sama dengan UTC^[Oke, jangan hajar aku! GMT secara teknis adalah zona
  waktu, sedangkan UTC adalah sistem waktu global. Selain itu beberapa
  negara mungkin menyesuaikan GMT untuk daylight saving time, sedangkan UTC
  tidak pernah disesuaikan untuk daylight saving time.]. Kemungkinan kamu
  ingin mengatakan UTC, atau "universal time". Kalau kamu membicarakan
  zona waktu GMT secara khusus, sebut GMT. Membingungkan, banyak fungsi UTC
  di C lebih tua daripada UTC dan masih merujuk ke Greenwich Mean Time.
  Saat melihat itu, ketahuilah bahwa maksud C adalah UTC.

* \index{Local time} **Waktu lokal**: jam berapa sekarang di tempat komputer
  yang menjalankan program berada. Ini dinyatakan sebagai selisih dari UTC.
  Walaupun ada banyak zona waktu di dunia, kebanyakan komputer bekerja
  dalam waktu lokal atau UTC.

\index{Universal Coordinated Time|(}

Sebagai aturan umum, kalau kamu menggambarkan kejadian yang terjadi satu
kali, seperti entri log, atau peluncuran roket, atau saat pointer akhirnya
klik bagimu, pakai UTC.

\index{Local time|(}

Sebaliknya, kalau itu sesuatu yang terjadi pada waktu yang sama _di setiap
zona waktu_, seperti malam Tahun Baru atau jam makan malam, pakai waktu
lokal.

Karena banyak bahasa hanya pandai mengonversi antara UTC dan waktu lokal,
kamu bisa menyakiti dirimu sendiri dengan memilih menyimpan tanggalmu dalam
bentuk yang salah. (Tanya aku bagaimana aku tahu.)

\index{Local time|)}
\index{Universal Coordinated Time|)}


## Tipe Tanggal

Ada dua^[Harus diakui, ada lebih dari dua.] tipe utama di C untuk tanggal:
`time_t` dan `struct tm`.

Spesifikasi sebenarnya tidak banyak bicara soal keduanya:

* \index{time\_t type@\texttt{time\_t} type} `time_t`: tipe real yang mampu menampung waktu. Jadi
  menurut spesifikasi, ini bisa berupa tipe floating atau integer. Di POSIX
  (sistem mirip Unix), ia integer. Ia menyimpan _calendar time_. Yang bisa
  dianggap sebagai waktu UTC.

* \index{struct tm type@\texttt{struct tm} type} `struct tm`: menyimpan komponen sebuah calendar
  time. Ini _broken-down time_, yaitu komponen-komponen waktu, seperti jam,
  menit, detik, hari, bulan, tahun, dan sebagainya.

\index{time\_t type@\texttt{time\_t} type|(}

Di banyak sistem, `time_t` mewakili jumlah detik sejak
[_Epoch_](https://en.wikipedia.org/wiki/Unix_time)^[https://en.wikipedia.org/wiki/Unix_time]. Epoch dalam beberapa hal adalah awal waktu dari
sudut pandang komputer, yang umumnya 1 Januari 1970 UTC.  `time_t` bisa
negatif untuk mewakili waktu sebelum Epoch. Windows berperilaku sama
dengan Unix sejauh yang kutahu.

\index{time\_t type@\texttt{time\_t} type|)}
\index{struct tm type@\texttt{struct tm} type|(}

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

\index{struct tm type@\texttt{struct tm} type|)}

Jadi pertanyaannya menjadi: "Bagaimana menginisialisasi data bertipe ini,
dan bagaimana mengonversi di antara mereka?"

## Inisialisasi dan Konversi Antartipe

\index{time() function@\texttt{time()} function|(}

Pertama, kamu bisa mengambil waktu saat ini dan menyimpannya dalam
`time_t` dengan fungsi `time()`.

``` {.c}
time_t now;  // Variabel untuk menampung waktu sekarang

now = time(NULL);  // Kamu bisa mengambilnya seperti ini...

time(&now);        // ...atau ini. Sama saja dengan baris sebelumnya.
```

Bagus! Kamu punya variabel yang menyimpan waktu sekarang.

\index{time() function@\texttt{time()} function|)}
\index{ctime() function@\texttt{ctime()} function|(}

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

\index{ctime() function@\texttt{ctime()} function|)}

Jadi itu agak kaku. Kalau ingin kendali lebih besar, kamu harus
mengonversi `time_t` itu menjadi `struct tm`.

### Mengonversi `time_t` ke `struct tm`

\index{time\_t type@\texttt{time\_t} type!conversion to struct tm@conversion to \texttt{struct tm}|(}

Ada dua cara luar biasa untuk melakukan konversi ini:

\index{localtime() function@\texttt{localtime()} function|(}

* `localtime()`: fungsi ini mengonversi `time_t` menjadi `struct tm`
  dalam waktu lokal.

\index{gmtime() function@\texttt{gmtime()} function|(}

* `gmtime()`: fungsi ini mengonversi `time_t` menjadi `struct tm` dalam
  UTC. (Lihat GMT kuno menyelinap ke nama fungsi ini?)

\index{asctime() function@\texttt{asctime()} function|(}

Mari kita lihat jam berapa sekarang dengan mencetak `struct tm` memakai
fungsi `asctime()`:

``` {.c}
printf("Local: %s", asctime(localtime(&now)));
printf("  UTC: %s", asctime(gmtime(&now)));
```

\index{asctime() function@\texttt{asctime()} function|)}
\index{localtime() function@\texttt{localtime()} function|)}
\index{gmtime() function@\texttt{gmtime()} function|)}

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

\index{time\_t type@\texttt{time\_t} type!conversion to struct tm@conversion to \texttt{struct tm}|)}

### Mengonversi `struct tm` ke `time_t`

\index{struct tm type@\texttt{struct tm} type!conversion to time\_t@conversion to \texttt{time\_t}|(}
\index{mktime() function@\texttt{mktime()} function|(}

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

\index{mktime() function@\texttt{mktime()} function|)}

Anehnya, standar tidak memberi kita cara untuk memuat `struct tm` dengan
waktu UTC lalu mengonversinya menjadi `time_t`. Kalau ingin melakukannya di
sistem mirip Unix, coba \index{timegm() Unix function@\texttt{timegm()} Unix function} `timegm()` yang
non-standar. Di Windows, \index{\_mkgmtime() Windows function@\texttt{\_mkgmtime()} Windows function}
`_mkgmtime()`.

\index{struct tm type@\texttt{struct tm} type!conversion to time\_t@conversion to \texttt{time\_t}|)}

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

\index{strftime() function@\texttt{strftime()} function|(}

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
`strftime()`, jadi pastikan melihatnya di [halaman referensi
`strftime()`](https://beej.us/guide/bgclr/html/split/time.html#man-strftime)^[https://beej.us/guide/bgclr/html/split/time.html#man-strftime].

\index{strftime() function@\texttt{strftime()} function|)}

## Resolusi Lebih Tinggi dengan `timespec_get()`

\index{timespec\_get() function@\texttt{timespec\_get()} function|(}

Kamu bisa mendapat jumlah detik dan nanodetik sejak Epoch dengan
`timespec_get()`.

Mungkin.

Implementasi mungkin tidak punya resolusi nanodetik (itu sepersemiliar
detik) jadi tidak ada yang tahu berapa banyak tempat signifikan yang kamu
dapat, tapi coba saja dan lihat.

\index{struct timespec type@\texttt{struct timespec} type|(}

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

\index{struct timespec type@\texttt{struct timespec} type|)}
\index{timespec\_get() function@\texttt{timespec\_get()} function|)}

## Selisih Antarwaktu

\index{Date and time!differences|(}

Satu catatan singkat soal mengambil selisih antara dua `time_t`: karena
spesifikasi tidak menentukan bagaimana tipe itu merepresentasikan waktu,
kamu mungkin tidak bisa begitu saja mengurangkan dua `time_t` dan
mendapat sesuatu yang masuk akal^[Di POSIX bisa, karena `time_t` jelas
integer. Sayangnya seluruh dunia bukan POSIX, jadi begitulah.].

\index{difftime() function@\texttt{difftime()} function|(}

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

\index{difftime() function@\texttt{difftime()} function|)}
\index{Date and time!differences|)}
\index{Date and time|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Multithreading

\index{Multithreading|(}

C11 secara resmi memperkenalkan multithreading ke bahasa C. Rasanya
sangat mirip dengan [POSIX threads](https://en.wikipedia.org/wiki/POSIX_Threads)^[https://en.wikipedia.org/wiki/POSIX_Threads], kalau kamu pernah
memakainya.

Dan kalau belum, tidak apa-apa. Kita akan membahasnya.

Namun perlu dicatat, aku tidak bermaksud membuat panduan multithreading
klasik yang lengkap^[Aku sendiri lebih suka shared-nothing, dan
kemampuanku dengan konstruksi multithreading klasik berkarat,
paling tidak begitu.]; untuk itu kamu harus mencari buku lain yang sangat
tebal. Maaf!

\index{\_\_STDC\_NO\_THREADS\_\_ macro@\texttt{\_\_STDC\_NO\_THREADS\_\_} macro|(}

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

\index{\_\_STDC\_NO\_THREADS\_\_ macro@\texttt{\_\_STDC\_NO\_THREADS\_\_} macro|)}

\index{gcc compiler@\texttt{gcc} compiler!with threads|(}

Selain itu, kamu mungkin perlu menentukan opsi linker tertentu saat
membangun. Di sistem mirip Unix, coba tambahkan `-lpthreads` di akhir
command line untuk me-link library `pthreads`^[Ya, `pthreads` dengan
"`p`". Itu singkatan POSIX threads, library yang banyak dipinjam C11 untuk
implementasi thread-nya.]:

``` {.zsh}
gcc -std=c11 -o foo foo.c -lpthreads
```

\index{gcc compiler@\texttt{gcc} compiler!with threads|)}

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

\index{Multithreading!and the standard library|(}

Sebagian fungsi di standard library (mis. `asctime()` dan `strtok()`)
mengembalikan atau memakai elemen data `static` yang tidak threadsafe.
Tapi secara umum, kecuali dinyatakan lain, standard library berusaha agar
threadsafe^[Menurut §7.1.4¶5.].

Tapi tetap waspada. Kalau sebuah fungsi standard library menyimpan state
antarpemanggilan di variabel yang bukan milikmu, atau kalau sebuah fungsi
mengembalikan pointer ke sesuatu yang tidak kamu oper, ia tidak
threadsafe.

\index{Multithreading!and the standard library|)}

## Membuat dan Menunggu Thread

Mari kita buat sesuatu!

Kita akan membuat beberapa thread (create) dan menunggu mereka selesai
(join).

Tapi ada sedikit hal yang perlu dipahami dulu.

Setiap thread diidentifikasi oleh variabel opaque bertipe
\index{thrd\_t type@\texttt{thrd\_t} type} `thrd_t`. Ini identifier unik per thread di programmu.
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
2. Dari thread utama, jalankan thread baru dengan \index{thrd\_create() function@\texttt{thrd\_create()} function}`thrd_create()`, dan oper pointer ke fungsi yang akan
   dijalankan.
3. Di fungsi itu, biarkan thread mengerjakan apa pun yang harus
   dikerjakannya.
4. Sementara itu, thread utama bisa melanjutkan apa pun yang harus
   dikerjakan _olehnya_.
5. Ketika thread utama memutuskan, ia bisa menunggu child thread selesai
   dengan memanggil \index{thrd\_join() function@\texttt{thrd\_join()} function} `thrd_join()`. Pada
   umumnya kamu **harus** me-`thrd_join()` thread itu untuk membersihkan
   sisanya, kalau tidak memori akan bocor^[Kecuali kamu
   `thrd_detach()`. Soal ini dibahas nanti.]

\index{thrd\_create() function@\texttt{thrd\_create()} function|(}
\index{thrd\_start\_t type@\texttt{thrd\_start\_t} type|(}

`thrd_create()` menerima pointer ke fungsi yang akan dijalankan, dan
bertipe `thrd_start_t`, yaitu `int (*)(void *)`. Itu bahasa Yunani untuk
"pointer ke fungsi yang menerima `void*` sebagai argumen, dan
mengembalikan `int`."

\index{thrd\_join() function@\texttt{thrd\_join()} function|(}

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

\index{thrd\_start\_t type@\texttt{thrd\_start\_t} type|)}

Lihat bagaimana kita memakai `thrd_create()` di situ untuk memanggil
fungsi `run()`? Lalu kita mengerjakan hal lain di `main()` dan kemudian
berhenti dan menunggu thread selesai dengan `thrd_join()`.

\index{thrd\_create() function@\texttt{thrd\_create()} function|)}
\index{thrd\_join() function@\texttt{thrd\_join()} function|)}

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

\index{thrd\_create() function@\texttt{thrd\_create()} function|(}
\index{thrd\_join() function@\texttt{thrd\_join()} function|(}

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

\index{thrd\_join() function@\texttt{thrd\_join()} function|)}

Saat aku menjalankan thread, aku menghitung `i` naik dari 0 sampai 4. Lalu aku
mengoper pointer ke `i` ke `thrd_create()`. Pointer ini berakhir di rutin `run()`
tempat kita membuat salinannya.

\index{thrd\_create() function@\texttt{thrd\_create()} function|)}

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

Ini sudah masuk ke wilayah seru \index{Multithreading!race conditions}
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

\index{thrd\_detach() function@\texttt{thrd\_detach()} function|(}

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

\index{thrd\_detach() function@\texttt{thrd\_detach()} function|)}

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

\index{Thread local data|(}

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

\index{\_Thread\_local storage class@\texttt{\_Thread\_local} storage class|(}

Pertama-tama, mari kita lihat cara mudah mengatasinya: storage-class
`_Thread_local`.

Pada dasarnya kita tinggal menempelkannya di depan variabel `static`
block scope kita dan semuanya berfungsi! Ia memberi tahu C bahwa setiap
thread harus punya versinya sendiri dari variabel ini, sehingga tidak ada
yang saling menginjak kaki.

\index{thread\_local storage class@\texttt{thread\_local} storage class|(}

Header \index{threads.h header file@\texttt{threads.h} header file} `<threads.h>` mendefinisikan
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

\index{thread\_local storage class@\texttt{thread\_local} storage class|)}
\index{\_Thread\_local storage class@\texttt{\_Thread\_local} storage class|)}
\index{Thread local data|)}

### Opsi Lain: Thread-Specific Storage

\index{Thread-specific storage|(}

Thread-specific storage (TSS) adalah cara lain untuk mendapatkan data
per-thread.

Satu fitur tambahan: fungsi-fungsi ini memungkinkanmu menentukan destructor
yang akan dipanggil pada data saat variabel TSS dihapus. Biasanya
destructor ini adalah `free()` untuk membersihkan data per-thread hasil
`malloc()` secara otomatis. Atau `NULL` kalau tidak ada yang perlu
dihancurkan.

Destructor bertipe \index{tss\_dtor\_t type@\texttt{tss\_dtor\_t} type} `tss_dtor_t`, yaitu pointer ke
fungsi yang mengembalikan `void` dan menerima `void*` sebagai argumen
(`void*` itu menunjuk ke data yang disimpan di variabel). Dengan kata
lain, ia `void (*)(void*)`, kalau itu membantu menjelaskan. Dan aku akui,
kemungkinan itu tidak membantu. Lihat contoh di bawah.

Secara umum, `thread_local` kemungkinan pilihan utamamu, tapi kalau kamu
menyukai gagasan destructor, kamu bisa memanfaatkannya.

Pemakaiannya agak aneh karena kita butuh variabel bertipe \index{tss\_t type@\texttt{tss\_t} type} `tss_t` yang tetap hidup untuk mewakili nilai per thread. Lalu kita
menginisialisasinya dengan \index{tss\_create() function@\texttt{tss\_create()} function} `tss_create()`.
Akhirnya kita membuangnya dengan \index{tss\_delete() function@\texttt{tss\_delete()} function|(}
`tss_delete()`. Perhatikan bahwa memanggil `tss_delete()` tidak menjalankan
semua destructor---yang melakukannya adalah `thrd_exit()` (atau return dari
fungsi run). `tss_delete()` hanya melepas memori yang dialokasikan oleh
`tss_create()`. \index{tss\_delete() function@\texttt{tss\_delete()} function|)}

Di tengah-tengahnya, thread bisa memanggil \index{tss\_set() function@\texttt{tss\_set()} function}
`tss_set()` dan \index{tss\_get() function@\texttt{tss\_get()} function} `tss_get()` untuk mengisi dan
mengambil nilai.

Pada kode berikut, kita menyiapkan variabel TSS sebelum membuat thread,
lalu membersihkan setelah thread selesai.

Pada fungsi `run()`, thread melakukan `malloc()` ruang untuk string dan
menyimpan pointer itu di variabel TSS.

Saat thread keluar, fungsi destructor (`free()` dalam kasus ini) dipanggil
untuk _semua_ thread.

\index{tss\_t type@\texttt{tss\_t} type|(}
\index{tss\_get() function@\texttt{tss\_get()} function|(}
\index{tss\_set() function@\texttt{tss\_set()} function|(}
\index{tss\_create() function@\texttt{tss\_create()} function|(}
\index{tss\_delete() function@\texttt{tss\_delete()} function|(}

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

\index{tss\_t type@\texttt{tss\_t} type|)}
\index{tss\_get() function@\texttt{tss\_get()} function|)}
\index{tss\_set() function@\texttt{tss\_set()} function|)}
\index{tss\_create() function@\texttt{tss\_create()} function|)}
\index{tss\_delete() function@\texttt{tss\_delete()} function|)}

Sekali lagi, ini cara yang agak menyakitkan dibanding `thread_local`, jadi
kecuali kamu benar-benar membutuhkan fungsionalitas destructor itu,
aku akan memakai `thread_local` saja.

\index{Thread-specific storage|)}

## Mutex {#mutex}

\index{Mutexes|(}

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
dipakai dengan \index{mtx\_init() function@\texttt{mtx\_init()} function} `mtx_init()`.

Lalu thread-thread berikutnya bisa memanggil \index{mtx\_lock() function@\texttt{mtx\_lock()} function}
`mtx_lock()` dan \index{mtx\_unlock function@\texttt{mtx\_unlock} function} `mtx_unlock()` untuk
memperoleh dan melepas mutex.

Saat kita benar-benar selesai dengan mutex, kita bisa menghancurkannya
dengan \index{mtx\_destroy() function@\texttt{mtx\_destroy()} function} `mtx_destroy()`, kebalikan logis dari
\index{mtx\_init() function@\texttt{mtx\_init()} function} `mtx_init()`.

\index{Multithreading!race conditions|(}

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

\index{Multithreading!race conditions|)}

Yang ingin kita lakukan adalah membungkus pengambilan variabel dan
pengisiannya ke dalam satu bentangan kode yang dilindungi mutex.

Kita tambahkan variabel baru untuk mewakili mutex bertipe \index{mtx\_t type@\texttt{mtx\_t} type} `mtx_t` di file scope, menginisialisasinya, lalu thread bisa
mengunci dan membukanya di fungsi `run()`.

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|(}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|(}
\index{mtx\_init() function@\texttt{mtx\_init()} function|(}
\index{mtx\_destroy() function@\texttt{mtx\_destroy()} function|(}

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

\index{mtx\_init() function@\texttt{mtx\_init()} function|)}
\index{mtx\_destroy() function@\texttt{mtx\_destroy()} function|)}

Tapi setiap thread memperoleh mutex di baris 15 dan melepasnya di baris
24.

Di antara `mtx_lock()` dan `mtx_unlock()` adalah _critical section_,
bagian kode yang tidak ingin kita biarkan dikacau beberapa thread
sekaligus.

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|)}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|)}

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

\index{Mutexes!types|(}

Seperti yang diisyaratkan tadi, ada beberapa tipe mutex yang bisa dibuat
dengan `mtx_init()`. (Sebagian tipe ini adalah hasil operasi bitwise-OR,
seperti tercatat di tabel.)

\index{Mutexes!timeouts|(}

\index{mtx\_plain macro@\texttt{mtx\_plain} macro}
\index{mtx\_timed macro@\texttt{mtx\_timed} macro}
\index{mtx\_plain macro@\texttt{mtx\_plain} macro}
\index{mtx\_timed macro@\texttt{mtx\_timed} macro}
\index{mtx\_recursive macro@\texttt{mtx\_recursive} macro}

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

\index{mtx\_timed macro@\texttt{mtx\_timed} macro|(}

Untuk mutex dengan timeout, pastikan membuatnya dengan `mtx_timed`:

``` {.c}
mtx_init(&serial_mtx, mtx_timed);
```

\index{mtx\_timed macro@\texttt{mtx\_timed} macro|)}

Lalu saat menunggunya, kamu harus menentukan waktu dalam UTC kapan ia akan
dibuka^[Mungkin kamu mengira itu "waktu dari sekarang", tapi kamu pasti
ingin berpikir begitu, kan!].

\index{timespec\_get() function@\texttt{timespec\_get()} function|(}

Fungsi `timespec_get()` dari `<time.h>` bisa membantu di sini. Ia
memberimu waktu saat ini dalam UTC di `struct timespec`, yang persis
kita butuhkan. Bahkan, ia tampaknya ada hanya untuk tujuan ini.

Ia punya dua field: `tv_sec` berisi waktu saat ini dalam detik sejak
epoch, dan `tv_nsec` berisi nanodetik (sepersemiliar detik) sebagai bagian
"pecahan".

Jadi kamu bisa mengisinya dengan waktu saat ini, lalu menambahkannya untuk
mendapat timeout tertentu.

\index{mtx\_timedlock() function@\texttt{mtx\_timedlock()} function|(}

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

\index{mtx\_timedlock() function@\texttt{mtx\_timedlock()} function|)}
\index{timespec\_get() function@\texttt{timespec\_get()} function|)}

Selain itu, timed lock sama saja dengan lock biasa.

\index{Mutexes!timeouts|)}
\index{Mutexes!types|)}
\index{Mutexes|)}

## Condition Variable

\index{Condition variables|(}

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

Semua ini terjadi seputar variabel tambahan bertipe \index{cnd\_t type@\texttt{cnd\_t} type}
`cnd_t` yang adalah _condition variable_. Kita membuat variabel ini dengan
fungsi \index{cnd\_init() function@\texttt{cnd\_init()} function} `cnd_init()` dan menghancurkannya saat
selesai dengan fungsi \index{cnd\_destroy() function@\texttt{cnd\_destroy()} function} `cnd_destroy()`.

Tapi bagaimana semuanya bekerja? Mari kita lihat garis besar apa yang akan
dilakukan child thread:

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|(}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|(}
\index{cnd\_wait() function@\texttt{cnd\_wait()} function|(}
\index{cnd\_signal() function@\texttt{cnd\_signal()} function|(}

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

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|)}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|)}

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

\index{cnd\_signal() function@\texttt{cnd\_signal()} function|)}

Satu hal lagi soal `cnd_wait()`. Kamu kemungkinan akan memanggil
`cnd_wait()` kalau suatu kondisi^[Dan itulah kenapa mereka disebut
_condition variable_!] belum terpenuhi (mis. dalam kasus ini, kalau belum
semua angka dimasukkan). Intinya: kondisi ini harus berada dalam loop
`while`, bukan pernyataan `if`. Kenapa?

Itu karena fenomena misterius bernama \index{Condition variables!spurious wakeup} _spurious wakeup_. Kadang, pada beberapa implementasi, sebuah
thread bisa terbangun dari tidur `cnd_wait()` tanpa _alasan_ yang jelas.
_[Musik X-Files]_^[Aku tidak bilang ini ulah alien... tapi ini ulah
alien. Oke, sebenarnya lebih mungkin thread lain sudah terbangun dan sampai
ke pekerjaannya lebih dulu.]. Karena itu kita harus memeriksa bahwa kondisi
yang kita butuhkan masih benar-benar terpenuhi saat bangun. Dan kalau
tidak, kembali tidur!

\index{cnd\_wait() function@\texttt{cnd\_wait()} function|)}

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

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|(}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|(}
\index{mtx\_init() function@\texttt{mtx\_init()} function|(}
\index{mtx\_destroy() function@\texttt{mtx\_destroy()} function|(}
\index{cnd\_init() function@\texttt{cnd\_init()} function|(}
\index{cnd\_destroy() function@\texttt{cnd\_destroy()} function|(}
\index{cnd\_wait() function@\texttt{cnd\_wait()} function|(}
\index{cnd\_signal() function@\texttt{cnd\_signal()} function|(}
\index{cnd\_t type@\texttt{cnd\_t} type|(}

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

\index{mtx\_lock() function@\texttt{mtx\_lock()} function|)}
\index{mtx\_unlock() function@\texttt{mtx\_unlock()} function|)}
\index{mtx\_init() function@\texttt{mtx\_init()} function|)}
\index{mtx\_destroy() function@\texttt{mtx\_destroy()} function|)}
\index{cnd\_init() function@\texttt{cnd\_init()} function|)}
\index{cnd\_destroy() function@\texttt{cnd\_destroy()} function|)}
\index{cnd\_wait() function@\texttt{cnd\_wait()} function|)}
\index{cnd\_signal() function@\texttt{cnd\_signal()} function|)}
\index{cnd\_t type@\texttt{cnd\_t} type|)}

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

\index{Condition variables!timeouts|(}

Ada varian `cnd_wait()` yang memungkinkanmu menentukan timeout agar bisa
berhenti menunggu.

Karena child thread harus mengunci ulang mutex, ini belum tentu berarti
kamu langsung hidup kembali tepat saat timeout terjadi; kamu masih harus
menunggu thread lain melepas mutex.

Tapi artinya kamu tidak akan menunggu sampai `cnd_signal()` terjadi.

Agar ini berfungsi, panggil \index{cnd\_timedwait() function@\texttt{cnd\_timedwait()} function}
`cnd_timedwait()` alih-alih `cnd_wait()`. Kalau ia mengembalikan nilai
\index{thrd\_timedout macro@\texttt{thrd\_timedout} macro} `thrd_timedout`, berarti timeout.

Timestamp-nya adalah waktu absolut dalam UTC, bukan waktu-dari-sekarang.
Untungnya fungsi \index{timespec\_get() function@\texttt{timespec\_get()} function} `timespec_get()` di
`<time.h>` tampaknya dibuat khusus untuk kasus ini.

\index{timespec\_get() function@\texttt{timespec\_get()} function|(}
\index{cnd\_timedwait() function@\texttt{cnd\_timedwait()} function|(}
\index{thrd\_timedout() macro@\texttt{thrd\_timedout()} macro|(}

``` {.c}
struct timespec timeout;

timespec_get(&timeout, TIME_UTC);  // Ambil waktu saat ini
timeout.tv_sec += 1;               // Timeout 1 detik setelah sekarang

int result = cnd_timedwait(&condition, &mutex, &timeout));

if (result == thrd_timedout) {
    printf("Condition variable timed out!\n");
}
```

\index{timespec\_get() function@\texttt{timespec\_get()} function|)}
\index{cnd\_timedwait() function@\texttt{cnd\_timedwait()} function|)}
\index{thrd\_timedout() macro@\texttt{thrd\_timedout()} macro|)}
\index{Condition variables!timeouts|)}

### Broadcast: Membangunkan Semua Thread yang Menunggu

\index{Condition variables!broadcasting|(}

\index{cnd\_signal() function@\texttt{cnd\_signal()} function|)} `cnd_signal()` hanya membangunkan satu
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

\index{cnd\_broadcast() function@\texttt{cnd\_broadcast()} function|(}

Cukup pakai `cnd_broadcast()` alih-alih `cnd_signal()`. Pemakaiannya
persis sama, hanya saja `cnd_broadcast()` membangunkan **semua** thread
yang tidur menunggu condition variable itu.

\index{cnd\_broadcast() function@\texttt{cnd\_broadcast()} function|)}
\index{Condition variables!broadcasting|)}
\index{Condition variables|)}

## Menjalankan Fungsi Satu Kali

\index{Multithreading!one-time functions|(}

Katakanlah kamu punya fungsi yang _bisa_ dijalankan banyak thread, tapi
kamu tidak tahu kapan, dan tidak sepadan repot menulis semua logikanya.

Ada jalan keluarnya: pakai \index{call\_once() function@\texttt{call\_once()} function} `call_once()`.
Banyak sekali thread bisa mencoba menjalankan fungsi itu, tapi hanya yang
pertama yang dihitung^[Yang kuat yang bertahan! Benar? Harus diakui ini
sebenarnya sama sekali tidak seperti itu.]

Untuk memakainya, kamu butuh variabel flag khusus yang dideklarasikan
untuk melacak apakah fungsi itu sudah dijalankan atau belum. Dan kamu
butuh fungsi yang akan dijalankan, yang tidak menerima parameter dan tidak
mengembalikan nilai.

\index{once\_flag type@\texttt{once\_flag} type|(}
\index{ONCE\_FLAG\_INIT macro@\texttt{ONCE\_FLAG\_INIT} macro|(}
\index{call\_once() function@\texttt{call\_once()} function|(}

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

\index{once\_flag type@\texttt{once\_flag} type|)}
\index{ONCE\_FLAG\_INIT macro@\texttt{ONCE\_FLAG\_INIT} macro|)}
\index{call\_once() function@\texttt{call\_once()} function|)}

Pada contoh ini, berapa pun banyaknya thread yang sampai ke fungsi
`run()`, `run_once_function()` hanya akan dipanggil satu kali.

\index{Multithreading!one-time functions|)}
\index{Multithreading|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Atomics {#chapter-atomics}

> _"They tried and failed, all of them?"_ \
> _"Oh, no." She shook her head. "They tried and died."_
>
> ---Paul Atreides and The Reverend Mother Gaius Helen Mohiam, _Dune_

\index{Atomic variables|(}

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

\index{\_\_STDC\_NO\_ATOMICS\_\_ macro@\texttt{\_\_STDC\_NO\_ATOMICS\_\_} macro|(}

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

\index{\_\_STDC\_NO\_ATOMICS\_\_ macro@\texttt{\_\_STDC\_NO\_ATOMICS\_\_} macro|)}

\index{Atomic variables!compiling with|(}

Kalau pengujian itu lolos, kamu aman meng-include `<stdatomic.h>`, header
yang menjadi dasar sisa bab ini. Tapi kalau tidak ada dukungan atomic,
header itu mungkin bahkan tidak ada.

Di beberapa sistem, kamu mungkin perlu menambahkan `-latomic` di akhir
command line kompilasi untuk memakai fungsi apa pun di header file itu.

\index{Atomic variables!compiling with|)}

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

Pertama, include \index{stdatomic.h header@\texttt{stdatomic.h} header} `<stdatomic.h>`.

\index{atomic\_int type@\texttt{atomic\_int} type|(}

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

\index{atomic\_int type@\texttt{atomic\_int} type|)}

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

\index{Atomic variables!synchronization|(}

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

\index{Atomic variables!synchronization|(}

## Acquire dan Release

\index{Atomic variables!acquire|(}
\index{Atomic variables!release|(}

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

\index{Atomic variables!acquire|)}
\index{Atomic variables!release|)}

## Sequential Consistency

\index{Atomic variables!sequential consistency|(}

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

\index{Atomic variables!sequential consistency|)}

## Penugasan dan Operator Atomic

\index{Atomic variables!assignments and operators|(}

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

\index{Atomic variables!assignments and operators|)}

## Fungsi Library yang Otomatis Menyinkronkan

\index{Atomic variables!synchronized library functions|(}

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

\index{Atomic variables!synchronized library functions|)}

## Type Specifier dan Qualifier Atomic

Mari kita turunkan sedikit tingkatnya dan lihat tipe apa yang tersedia,
dan bagaimana kita bahkan bisa membuat tipe atomic baru.

\index{\_Atomic type qualifier@\texttt{\_Atomic} type qualifier|(}

Pertama, mari kita lihat tipe atomic bawaan dan di-`typedef` ke apa
mereka. (Bocoran: `_Atomic` adalah type qualifier!)

\index{atomic\_bool type@\texttt{atomic\_bool} type}
\index{atomic\_char type@\texttt{atomic\_char} type}
\index{atomic\_schar type@\texttt{atomic\_schar} type}
\index{atomic\_uchar type@\texttt{atomic\_uchar} type}
\index{atomic\_short type@\texttt{atomic\_short} type}
\index{atomic\_ushort type@\texttt{atomic\_ushort} type}
\index{atomic\_int type@\texttt{atomic\_int} type}
\index{atomic\_uint type@\texttt{atomic\_uint} type}
\index{atomic\_long type@\texttt{atomic\_long} type}
\index{atomic\_ulong type@\texttt{atomic\_ulong} type}
\index{atomic\_llong type@\texttt{atomic\_llong} type}
\index{atomic\_ullong type@\texttt{atomic\_ullong} type}
\index{atomic\_char16\_t type@\texttt{atomic\_char16\_t} type}
\index{atomic\_char32\_t type@\texttt{atomic\_char32\_t} type}
\index{atomic\_wchar\_t type@\texttt{atomic\_wchar\_t} type}
\index{atomic\_int\_least8\_t type@\texttt{atomic\_int\_least8\_t} type}
\index{atomic\_uint\_least8\_t type@\texttt{atomic\_uint\_least8\_t} type}
\index{atomic\_int\_least16\_t type@\texttt{atomic\_int\_least16\_t} type}
\index{atomic\_uint\_least16\_t type@\texttt{atomic\_uint\_least16\_t} type}
\index{atomic\_int\_least32\_t type@\texttt{atomic\_int\_least32\_t} type}
\index{atomic\_uint\_least32\_t type@\texttt{atomic\_uint\_least32\_t} type}
\index{atomic\_int\_least64\_t type@\texttt{atomic\_int\_least64\_t} type}
\index{atomic\_uint\_least64\_t type@\texttt{atomic\_uint\_least64\_t} type}
\index{atomic\_int\_fast8\_t type@\texttt{atomic\_int\_fast8\_t} type}
\index{atomic\_uint\_fast8\_t type@\texttt{atomic\_uint\_fast8\_t} type}
\index{atomic\_int\_fast16\_t type@\texttt{atomic\_int\_fast16\_t} type}
\index{atomic\_uint\_fast16\_t type@\texttt{atomic\_uint\_fast16\_t} type}
\index{atomic\_int\_fast32\_t type@\texttt{atomic\_int\_fast32\_t} type}
\index{atomic\_uint\_fast32\_t type@\texttt{atomic\_uint\_fast32\_t} type}
\index{atomic\_int\_fast64\_t type@\texttt{atomic\_int\_fast64\_t} type}
\index{atomic\_uint\_fast64\_t type@\texttt{atomic\_uint\_fast64\_t} type}
\index{atomic\_intptr\_t type@\texttt{atomic\_intptr\_t} type}
\index{atomic\_uintptr\_t type@\texttt{atomic\_uintptr\_t} type}
\index{atomic\_size\_t type@\texttt{atomic\_size\_t} type}
\index{atomic\_ptrdiff\_t type@\texttt{atomic\_ptrdiff\_t} type}
\index{atomic\_intmax\_t type@\texttt{atomic\_intmax\_t} type}
\index{atomic\_uintmax\_t type@\texttt{atomic\_uintmax\_t} type}

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

\index{\_Atomic type qualifier@\texttt{\_Atomic} type qualifier|)}

Pakai saja sesukamu! Tipe-tipe ini konsisten dengan alias atomic di C++,
kalau itu membantu.

Tapi bagaimana kalau kamu ingin lebih?

Kamu bisa melakukannya dengan type qualifier atau type specifier.

\index{\_Atomic type specifier@\texttt{\_Atomic} type specifier|(}

Pertama, specifier! Ia adalah kata kunci `_Atomic` dengan tipe dalam
kurung setelahnya^[Tampaknya C++23 menambahkan ini sebagai makro.]---cocok
dipakai dengan `typedef`:

``` {.c}
typedef _Atomic(double) atomic_double;

atomic_double f;
```

Batasan pada specifier: tipe yang dijadikan atomic tidak boleh bertipe
array atau fungsi, dan tidak boleh atomic atau sudah ber-qualifier lain.

\index{\_Atomic type specifier@\texttt{\_Atomic} type specifier|)}
\index{\_Atomic type qualifier@\texttt{\_Atomic} type qualifier|(}

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

\index{\_Atomic type qualifier@\texttt{\_Atomic} type qualifier|)}

## Variabel Atomic Lock-Free {#lock-free-atomic}

\index{Atomic variables!lock-free|(}

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

\index{ATOMIC\_BOOL\_LOCK\_FREE macro@\texttt{ATOMIC\_BOOL\_LOCK\_FREE} macro}
\index{ATOMIC\_CHAR\_LOCK\_FREE macro@\texttt{ATOMIC\_CHAR\_LOCK\_FREE} macro}
\index{ATOMIC\_CHAR16\_T\_LOCK\_FREE macro@\texttt{ATOMIC\_CHAR16\_T\_LOCK\_FREE} macro}
\index{ATOMIC\_CHAR32\_T\_LOCK\_FREE macro@\texttt{ATOMIC\_CHAR32\_T\_LOCK\_FREE} macro}
\index{ATOMIC\_WCHAR\_T\_LOCK\_FREE macro@\texttt{ATOMIC\_WCHAR\_T\_LOCK\_FREE} macro}
\index{ATOMIC\_SHORT\_LOCK\_FREE macro@\texttt{ATOMIC\_SHORT\_LOCK\_FREE} macro}
\index{ATOMIC\_INT\_LOCK\_FREE macro@\texttt{ATOMIC\_INT\_LOCK\_FREE} macro}
\index{ATOMIC\_LONG\_LOCK\_FREE macro@\texttt{ATOMIC\_LONG\_LOCK\_FREE} macro}
\index{ATOMIC\_LLONG\_LOCK\_FREE macro@\texttt{ATOMIC\_LLONG\_LOCK\_FREE} macro}
\index{ATOMIC\_POINTER\_LOCK\_FREE macro@\texttt{ATOMIC\_POINTER\_LOCK\_FREE} macro}

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
\index{atomic\_is\_lock\_free() function@\texttt{atomic\_is\_lock\_free()} function} `atomic_is_lock_free()`. Fungsi ini
mengembalikan true atau false apakah tipe tertentu atomic saat ini.

Jadi kenapa kita peduli?

Lock-free lebih cepat, jadi mungkin ada kekhawatiran kecepatan yang akan
kamu atasi dengan cara lain. Atau mungkin kamu perlu memakai variabel
atomic di signal handler.

\index{Atomic variables!lock-free|)}

### Signal Handler dan Atomic Lock-Free

\index{Signal handlers!with lock-free atomics|(}
\index{Atomic variables!with signal handlers|(}

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
pemanggilan \index{raise() function@\texttt{raise()} function} `raise()`, ia disekuensikan _setelah_
`raise()`.]. Bagaimanapun, signal bisa di-raise kapan saja. Jadi aku
menganggap perilaku C serupa.

\index{Signal handlers!with lock-free atomics|)}
\index{Atomic variables!with signal handlers|)}

## Atomic Flag {#atomic-flags}

\index{Atomic variables!atomic flags|(}
\index{atomic\_flag type@\texttt{atomic\_flag} type|(}

Hanya ada satu tipe yang dijamin standar sebagai atomic lock-free:
`atomic_flag`. Ini tipe opaque untuk operasi
[test-and-set](https://en.wikipedia.org/wiki/Test-and-set)^[https://en.wikipedia.org/wiki/Test-and-set].

Ia bisa _set_ atau _clear_. Kamu bisa menginisialisasinya menjadi clear
dengan:

\index{ATOMIC\_FLAG\_INIT macro@\texttt{ATOMIC\_FLAG\_INIT} macro|(}

``` {.c}
atomic_flag f = ATOMIC_FLAG_INIT;
```

\index{ATOMIC\_FLAG\_INIT macro@\texttt{ATOMIC\_FLAG\_INIT} macro|)}

\index{atomic\_flag\_test\_and\_set() function@\texttt{atomic\_flag\_test\_and\_set()} function|(}

Kamu bisa men-set flag secara atomic dengan `atomic_flag_test_and_set()`,
yang akan men-set flag dan mengembalikan status sebelumnya sebagai
`_Bool` (true untuk set).

\index{atomic\_flag\_clear() function@\texttt{atomic\_flag\_clear()} function|(}

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

\index{atomic\_flag\_clear() function@\texttt{atomic\_flag\_clear()} function|)}
\index{atomic\_flag\_test\_and\_set() function@\texttt{atomic\_flag\_test\_and\_set()} function|)}
\index{Atomic variables!atomic flags|)}
\index{atomic\_flag type@\texttt{atomic\_flag} type|)}

## `struct` dan `union` Atomic

\index{Atomic variables!struct and union@\texttt{struct} and \texttt{union}|(}

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

\index{Atomic variables!struct and union@\texttt{struct} and \texttt{union}|)}

## Pointer Atomic

\index{Atomic variables!pointers|(}

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

\index{Atomic variables!pointers|)}

## Memory Order

\index{Atomic variables!memory order|(}
\index{Memory order|(}

Kita sudah membahas sequential consistency, yang paling masuk akal di
antara semuanya. Tapi ada beberapa yang lain:

\index{memory\_order\_seq\_cst enumerated type@\texttt{memory\_order\_seq\_cst} enumerated type}
\index{memory\_order\_acq\_rel enumerated type@\texttt{memory\_order\_acq\_rel} enumerated type}
\index{memory\_order\_release enumerated type@\texttt{memory\_order\_release} enumerated type}
\index{memory\_order\_acquire enumerated type@\texttt{memory\_order\_acquire} enumerated type}
\index{memory\_order\_consume enumerated type@\texttt{memory\_order\_consume} enumerated type}
\index{memory\_order\_relaxed enumerated type@\texttt{memory\_order\_relaxed} enumerated type}

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

\index{atomic\_fetch\_add() function@\texttt{atomic\_fetch\_add()} function|(}

``` {.c}
atomic_int x = 0;

atomic_fetch_add(&x, 5);  // Sequential consistency, yang bawaan
```
\index{atomic\_fetch\_add() function@\texttt{atomic\_fetch\_add()} function|)}

Atau kamu bisa melakukan hal yang sama dengan memory ordering eksplisit:

\index{atomic\_fetch\_add\_explicit() function@\texttt{atomic\_fetch\_add\_explicit()} function|(}

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

\index{atomic\_fetch\_add\_explicit() function@\texttt{atomic\_fetch\_add\_explicit()} function|)}

Kita akan membahas rincian memory order yang berbeda di bawah. Jangan
mengutak-atik apa pun selain sequential consistency kecuali kamu tahu apa
yang kamu lakukan. Sangat mudah membuat kesalahan yang menyebabkan
kegagalan langka yang sulit direproduksi.

### Sequential Consistency

\index{Atomic variables!sequential consistency|(}
\index{Memory order!sequential consistency|(}

* Operasi load melakukan acquire (lihat di bawah).
* Operasi store melakukan release (lihat di bawah).
* Operasi read-modify-write melakukan acquire lalu release.

Selain itu, untuk menjaga urutan total acquire dan release, tidak ada
acquire atau release yang akan ditata ulang terhadap satu sama lain.
(Aturan acquire/release tidak melarang penataan ulang release yang diikuti
acquire. Tapi aturan sequentially consistent melarangnya.)

\index{Memory order!sequential consistency|)}
\index{Atomic variables!sequential consistency|)}

### Acquire

\index{Atomic variables!acquire|(}
\index{Memory order!acquire|(}

Inilah yang terjadi pada operasi load/read variabel atomic.

* Kalau thread lain me-release variabel atomic ini, semua penulisan yang
  dilakukan thread itu kini terlihat di thread ini.

* Akses memori di thread ini yang terjadi setelah load ini tidak bisa
  ditata ulang ke sebelumnya.

\index{Memory order!acquire|)}
\index{Atomic variables!acquire|)}

### Release

\index{Atomic variables!release|(}
\index{Memory order!acquire|(}

Inilah yang terjadi pada store/write variabel atomic.

* Kalau thread lain nanti meng-acquire variabel atomic ini, semua
  penulisan memori di thread ini sebelum penulisan atomic-nya menjadi
  terlihat oleh thread lain itu.

* Akses memori di thread ini yang terjadi sebelum release tidak bisa
  ditata ulang ke setelahnya.

\index{Atomic variables!release|)}
\index{Memory order!release|)}

### Consume

\index{Atomic variables!consume|(}
\index{Memory order!consume|(}

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

\index{Atomic variables!consume|)}
\index{Memory order!consume|)}

### Acquire/Release

\index{Atomic variables!acquire/release|(}
\index{Memory order!acquire/release|(}

Ini hanya berlaku untuk operasi read-modify-write. Ia acquire dan release
yang digabung menjadi satu.

* Acquire terjadi untuk pembacaan.
* Release terjadi untuk penulisan.

\index{Atomic variables!acquire/release|)}
\index{Memory order!acquire/release|)}

### Relaxed

\index{Atomic variables!relaxed|(}
\index{Memory order!relaxed|(}

Tidak ada aturan; ini anarki! Semua orang boleh menata ulang semuanya di
mana saja! Anjing dan kucing hidup bersama---histeria massal!

Sebenarnya ada satu aturan. Pembacaan dan penulisan atomic tetap
semua-atau-tidak-sama-sekali. Tapi operasi bisa ditata ulang sesuka hati
dan sama sekali tidak ada sinkronisasi antar thread.

Ada beberapa kasus pemakaian untuk memory order ini, yang bisa kamu temukan
dengan sedikit pencarian, mis. counter sederhana.

Dan kamu bisa memakai fence untuk memaksa sinkronisasi setelah sekumpulan
penulisan relaxed.

\index{Atomic variables!relaxed|)}
\index{Memory order!relaxed|)}
\index{Memory order|)}
\index{Atomic variables!memory order|)}

## Fence

\index{Atomic variables!fences|(}

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

\index{atomic\_thread\_fence() function@\texttt{atomic\_thread\_fence()} function|(}

``` {.c}
atomic_thread_fence(memory_order_release);
```

\index{atomic\_thread\_fence() function@\texttt{atomic\_thread\_fence()} function|)}
\index{atomic\_signal\_fence() function@\texttt{atomic\_signal\_fence()} function|(}

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

\index{atomic\_signal\_fence() function@\texttt{atomic\_signal\_fence()} function|)}
\index{Atomic variables!fences|)}

## Referensi

Kalau kamu ingin mempelajari hal-hal ini lebih jauh, berikut beberapa
bahan yang membantuku menembusnya:

* Ceramah Herb Sutter _`atomic<>` Weapons_:
  * [Bagian 1](https://www.youtube.com/watch?v=A8eCGOqgvH4)^[https://www.youtube.com/watch?v=A8eCGOqgvH4]
  * [Bagian 2](https://www.youtube.com/watch?v=KeLBd2EJLOU)^[https://www.youtube.com/watch?v=KeLBd2EJLOU]

* [Materi Jeff Preshing](https://preshing.com/archives/)^[https://preshing.com/archives/], khususnya:
  * [An Introduction to Lock-Free Programming](https://preshing.com/20120612/an-introduction-to-lock-free-programming/)^[https://preshing.com/20120612/an-introduction-to-lock-free-programming/]
  * [Acquire and Release Semantics](https://preshing.com/20120913/acquire-and-release-semantics/)^[https://preshing.com/20120913/acquire-and-release-semantics/]
  * [The _Happens-Before_ Relation](https://preshing.com/20130702/the-happens-before-relation/)^[https://preshing.com/20130702/the-happens-before-relation/]
  * [The _Synchronizes-With_ Relation](https://preshing.com/20130823/the-synchronizes-with-relation/)^[https://preshing.com/20130823/the-synchronizes-with-relation/]
  * [The Purpose of `memory_order_consume` in C++11](https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/)^[https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/]
  * [You Can Do Any Kind of Atomic Read-Modify-Write Operation](https://preshing.com/20150402/you-can-do-any-kind-of-atomic-read-modify-write-operation/)^[https://preshing.com/20150402/you-can-do-any-kind-of-atomic-read-modify-write-operation/]

* CPPReference:
  * [Memory Order](https://en.cppreference.com/w/c/atomic/memory_order)^[https://en.cppreference.com/w/c/atomic/memory_order]
  * [Atomic Types](https://en.cppreference.com/w/c/language/atomic)^[https://en.cppreference.com/w/c/language/atomic]

* [Lockless Programming Considerations](https://docs.microsoft.com/en-us/windows/win32/dxtecharts/lockless-programming)^[https://docs.microsoft.com/en-us/windows/win32/dxtecharts/lockless-programming] oleh Bruce Dawson

* Orang-orang yang suka membantu dan berpengetahuan di
  [r/C_Programming](https://www.reddit.com/r/C_Programming/)^[https://www.reddit.com/r/C_Programming/]

\index{Atomic variables|)}



<!-- BG_NEW_CHAPTER -->

<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Function Specifier, Alignment Specifier dan Operator

Menurut pengalamanku, ini tidak terlalu sering dipakai, tapi kita
bahas di sini demi kelengkapan.

## Function Specifier

\index{Function specifiers|(}

Saat mendeklarasikan fungsi, kamu bisa memberi compiler beberapa petunjuk
tentang bagaimana fungsi itu mungkin atau akan dipakai. Ini memungkinkan
atau mendorong compiler melakukan optimasi tertentu.

### `inline` untuk Kecepatan---Mungkin

\index{inline function specifier@\texttt{inline} function specifier|(}

Kamu bisa mendeklarasikan fungsi sebagai inline seperti ini:

``` {.c}
static inline int add(int x, int y) {
    return x + y;
}
```

Ini dimaksudkan untuk mendorong compiler membuat pemanggilan fungsi ini
secepat mungkin. Dan, secara historis, salah satu caranya adalah
_inlining_, artinya badan fungsi disematkan seluruhnya di tempat
pemanggilan dilakukan. Ini menghindari semua overhead menyiapkan dan
membongkar pemanggilan fungsi, dengan konsekuensi ukuran kode lebih besar
karena fungsi disalin ke mana-mana, bukan dipakai ulang.

Hal-hal cepat dan kasar yang perlu diingat:

1. Kamu kemungkinan tidak perlu memakai `inline` untuk kecepatan. Compiler
   modern tahu mana yang terbaik.

2. Kalau kamu memakainya untuk kecepatan, pakai dengan file scope, yaitu
   `static inline`. Ini menghindari aturan rumit external linkage dan
   fungsi inline.

Berhenti membaca bagian ini sekarang.

Suka menyiksa diri, ya?

Mari kita coba menghilangkan `static`.

``` {.c .numberLines}
#include <stdio.h>

inline int add(int x, int y)
{
    return x + y;
}

int main(void)
{
    printf("%d\n", add(1, 2));
}
```

`gcc` memberi error linker pada `add()`^[Kecuali kamu mengompilasi dengan
optimasi aktif (kemungkinan)! Tapi kurasa, ketika itu terjadi,
perilakunya tidak sesuai spesifikasi.]. Spesifikasi mewajibkan bahwa kalau
kamu punya fungsi inline non-`extern`, kamu juga harus menyediakan versi
dengan external linkage.

Jadi kamu harus punya versi `extern` di tempat lain agar ini berfungsi.
Kalau compiler punya fungsi `inline` di file saat ini dan versi eksternal
dari fungsi yang sama di tempat lain, ia boleh memilih mana yang
dipanggil. Jadi aku sangat menyarankan keduanya sama.

Hal lain yang bisa dilakukan adalah mendeklarasikan fungsi sebagai
`extern inline`. Ini akan mencoba meng-inline di file yang sama (demi
kecepatan), tapi juga membuat versi dengan external linkage.

\index{inline function specifier@\texttt{inline} function specifier|)}

### `noreturn` dan `_Noreturn` {#noreturn}

\index{noreturn function specifier@\texttt{noreturn} function specifier|(}
\index{\_Noreturn function specifier@\texttt{\_Noreturn} function specifier|(}

Ini menandakan kepada compiler bahwa suatu fungsi tidak akan pernah
kembali ke pemanggilnya, yaitu program akan keluar lewat suatu mekanisme
sebelum fungsi itu kembali.

Ini memungkinkan compiler melakukan beberapa optimasi di sekitar
pemanggilan fungsi tersebut.

Ini juga memungkinkanmu memberi tahu developer lain bahwa sebagian logika
program bergantung pada fungsi yang _tidak_ kembali.

Kemungkinan kamu tidak akan pernah memakainya, tapi kamu akan melihatnya
pada beberapa pemanggilan library seperti
[`exit()`](https://beej.us/guide/bgclr/html/split/stdlib.html#man-exit)^[https://beej.us/guide/bgclr/html/split/stdlib.html#man-exit]
dan
[`abort()`](https://beej.us/guide/bgclr/html/split/stdlib.html#man-abort)^[https://beej.us/guide/bgclr/html/split/stdlib.html#man-abort].

Kata kunci bawaannya adalah `_Noreturn`, tapi kalau tidak merusak kode
yang sudah ada, semua orang akan menyarankan meng-include
`<stdnoreturn.h>` dan memakai `noreturn` yang lebih mudah dibaca.

Ini undefined behavior kalau fungsi yang dinyatakan `noreturn` ternyata
kembali. Itu tidak jujur secara komputasi, tahu.

Berikut contoh pemakaian `noreturn` dengan benar:

``` {.c .numberLines}
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

noreturn void foo(void) // Fungsi ini tidak boleh pernah kembali!
{
    printf("Happy days\n");

    exit(1);            // Dan ia tidak kembali--ia keluar di sini!
}

int main(void)
{
    foo();
}
```

Kalau compiler mendeteksi bahwa fungsi `noreturn` bisa kembali, ia
mungkin akan memperingatkanmu, dengan baik hati.

Mengganti fungsi `foo()` dengan ini:

``` {.c}
noreturn void foo(void)
{
    printf("Breakin' the law\n");
}
```

menghasilkan peringatan:

``` {.default}
foo.c:7:1: warning: function declared 'noreturn' should not return
```

\index{noreturn function specifier@\texttt{noreturn} function specifier|)}
\index{\_Noreturn function specifier@\texttt{\_Noreturn} function specifier|)}
\index{Function specifiers|)}

## Alignment Specifier dan Operator

\index{Alignment|(}

[_Alignment_](https://en.wikipedia.org/wiki/Data_structure_alignment)^[https://en.wikipedia.org/wiki/Data_structure_alignment] sepenuhnya tentang kelipatan
alamat tempat object bisa disimpan. Bisakah ini disimpan di alamat mana
pun? Atau harus alamat awal yang habis dibagi 2? Atau 8? Atau 16?

Kalau kamu membuat sesuatu yang low-level seperti memory allocator yang
berinteraksi dengan OS-mu, kamu mungkin perlu mempertimbangkan ini.
Kebanyakan developer melewati seluruh kariernya tanpa memakai
fungsionalitas ini di C.

### `alignas` dan `_Alignas`

\index{alignas alignment specifier@\texttt{alignas} alignment specifier|(}
\index{\_Alignas alignment specifier@\texttt{\_Alignas} alignment specifier|(}

Ini bukan fungsi. Melainkan _alignment specifier_ yang bisa dipakai pada
deklarasi variabel.

Specifier bawaannya adalah `_Alignas`, tapi header `<stdalign.h>`
mendefinisikannya sebagai `alignas` agar lebih sedap dipandang.

Kalau kamu butuh `char`-mu ter-align seperti `int`, kamu bisa
memaksanya seperti ini saat mendeklarasikannya:

``` {.c}
char alignas(int) c;
```

Kamu juga bisa mengoper nilai konstan atau ekspresi sebagai alignment. Ini
harus sesuatu yang didukung sistem, tapi spesifikasi tidak sampai
menentukan nilai apa yang boleh dimasukkan. Pangkat 2 kecil (1, 2, 4, 8,
dan 16) umumnya taruhan yang aman.

``` {.c}
char alignas(8) c;   // align pada batas 8 byte
```

Kalau ingin meng-align pada alignment maksimum yang dipakai sistemmu,
include `<stddef.h>` dan pakai tipe `max_align_t`, seperti ini:

``` {.c}
char alignas(max_align_t) c;
```

Kamu berpotensi melakukan _over-align_ dengan menentukan alignment lebih
besar dari `max_align_t`, tapi apakah hal seperti itu diizinkan tergantung
sistem.

\index{alignas alignment specifier@\texttt{alignas} alignment specifier|)}
\index{\_Alignas alignment specifier@\texttt{\_Alignas} alignment specifier|)}

### `alignof` dan `_Alignof`

\index{alignof operator@\texttt{alignof} operator|(}
\index{\_Alignof operator@\texttt{\_Alignof} operator|(}

Operator ini mengembalikan kelipatan alamat yang dipakai tipe tertentu
untuk alignment di sistem ini. Misalnya, mungkin `char` di-align setiap 1
alamat, dan `int` di-align setiap 4 alamat.

Operator bawaannya adalah `_Alignof`, tapi header `<stdalign.h>`
mendefinisikannya sebagai `alignof` kalau kamu ingin tampak lebih keren.

Berikut program yang mencetak alignment berbagai tipe. Sekali lagi, ini
akan bervariasi dari satu sistem ke sistem lain. Perhatikan bahwa tipe
`max_align_t` akan memberi alignment maksimum yang dipakai sistem.

``` {.c .numberLines}
#include <stdalign.h>
#include <stdio.h>     // untuk printf()
#include <stddef.h>    // untuk max_align_t

struct t {
    int a;
    char b;
    float c;
};

int main(void)
{
    printf("char       : %zu\n", alignof(char));
    printf("short      : %zu\n", alignof(short));
    printf("int        : %zu\n", alignof(int));
    printf("long       : %zu\n", alignof(long));
    printf("long long  : %zu\n", alignof(long long));
    printf("double     : %zu\n", alignof(double));
    printf("long double: %zu\n", alignof(long double));
    printf("struct t   : %zu\n", alignof(struct t));
    printf("max_align_t: %zu\n", alignof(max_align_t));
}
```

Output di sistemku:

``` {.default}
char       : 1
short      : 2
int        : 4
long       : 8
long long  : 8
double     : 8
long double: 16
struct t   : 16
max_align_t: 16
```

\index{alignof operator@\texttt{alignof} operator|)}
\index{\_Alignof operator@\texttt{\_Alignof} operator|)}

## Fungsi `memalignment()`

\index{memalignment() function@\texttt{memalignment()} function|(}

Baru di C23!

(Peringatan: tidak ada compilerku yang mendukung fungsi ini, jadi
kodenya sebagian besar belum teruji.)

`alignof` bagus kalau kamu tahu tipe datamu. Tapi bagaimana kalau kamu
_sangat tidak tahu_ tipenya, dan hanya punya pointer ke data itu?

Bagaimana itu bisa terjadi?

Tentu saja dengan teman baik kita `void*`. Kita tidak bisa mengopernya ke
`alignof`, tapi bagaimana kalau kita perlu tahu alignment sesuatu yang
ditunjuknya?

Mungkin kita ingin tahu ini kalau akan memakai memori itu untuk sesuatu
yang punya kebutuhan alignment yang signifikan. Misalnya, tipe atomic dan
floating sering berperilaku buruk kalau tidak ter-align.

Jadi dengan fungsi ini kita bisa memeriksa alignment sebuah data selama
kita punya pointer ke data itu, bahkan kalau berupa `void*`.

Mari kita buat tes cepat untuk melihat apakah void pointer ter-align
dengan baik untuk dipakai sebagai tipe atomic, dan, kalau ya, kita ambil
variabel untuk memakainya sebagai tipe itu:

``` {.c}
void foo(void *p)
{
    if (memalignment(p) >= alignof(atomic int)) {
        atomic int *i = p;
        do_things(i);
    } else
        puts("This pointer is no good as an atomic int\n");

...
```

Kurasa kamu akan jarang (sampai kemungkinan tidak pernah)
memerlukan fungsi ini kecuali kamu mengerjakan hal-hal low-level.

\index{memalignment() function@\texttt{memalignment()} function|)}

Dan begitulah. Alignment!

\index{Alignment|)}


