# Kata Pengantar
<!-- Beej's guide to C
# vim: ts=4:sw=4:nosi:et:tw=72
-->

<!-- No hyphenation -->
[nh[scalbn]]
[nh[scalbnf]]
[nh[scalbnl]]
[nh[scalbln]]
[nh[scalblnf]]
[nh[scalblnl]]
<!-- Can't do things that aren't letters
[nh[atan2]]
[nh[atan2f]]
[nh[atan2l]]
-->
[nh[lrint]]
[nh[lrintf]]
[nh[lrintl]]
[nh[llrint]]
[nh[llrintf]]
[nh[llrintl]]
[nh[lround]]
[nh[lroundf]]
[nh[lroundl]]
[nh[llround]]
[nh[llroundf]]
[nh[llroundl]]

<!-- Index see alsos -->
[is[String==>see `char *`]]
[is[New line==>see `\n` newline]]
[is[Ternary operator==>see `?:` ternary operator]]
[is[Addition operator==>see `+` addition operator]]
[is[Subtraction operator==>see `-` subtraction operator]]
[is[Multiplication operator==>see `*` multiplication operator]]
[is[Division operator==>see `/` division operator]]
[is[Modulus operator==>see `%` modulus operator]]
[is[Boolean NOT==>see `!` operator]]
[is[Boolean AND==>see `&&` operator]]
[is[Boolean OR==>see `||` operator]]
[is[Bell==>see `\a` operator]]
[is[Tab (is better)==>see `\t` operator]]
[is[Carriage return==>see `\r` operator]]
[is[Hexadecimal==>see `0x` hexadecimal]]

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
di atas. Itu potongan dari salah satu peserta [fl[International
Obfuscated C Code Contest|https://www.ioccc.org/]][i[International
Obfuscated C Code Contest]] tahun 2001, sebuah kompetisi luar biasa
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
[flw[Python|Python_(programming_language)]],
[flw[JavaScript|JavaScript]], [flw[Java|Java_(programming_language)]],
[flw[Rust|Rust_(programming_language)]],
[flw[Go|Go_(programming_language)]],
[flw[Swift|Swift_(programming_language)]], dan lainnya.
(Developer [flw[Objective-C|Objective-C]] akan merasa sangat mudah!)

Diasumsikan bahwa kamu sudah tahu apa itu variabel, apa yang dilakukan
loop, bagaimana cara kerja fungsi, dan seterusnya.

Kalau ternyata kamu belum punya bekal itu, yang bisa ditawarkan hanyalah
bacaan yang menghibur. Satu-satunya janji yang masuk akal: panduan ini
tidak akan berakhir dengan cliffhanger... atau justru _akan_?

## Cara Membaca Buku Ini

Panduan ini terdiri dari dua volume, dan ini yang pertama: volume
tutorial!

Volume kedua adalah [fl[referensi
library|https://beej.us/guide/bgclr/]], dan isinya jauh lebih banyak
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

Panduan ini akan berusaha untuk tetap pada [flw[ISO-standard C|ANSI_C]]
yang Klasik dan Sederhana. Ya, sebagian besar. Di sana-sini mungkin ada
bahasan tentang [flw[POSIX|POSIX]] atau semacamnya, tapi kita lihat nanti.

Pengguna **Unix** (misalnya Linux, BSD, dll.) coba jalankan `cc` atau `gcc`
dari command line---mungkin compiler-nya sudah terpasang. Kalau belum, cari
tahu cara memasang `gcc` atau `clang` di distribusimu.

Pengguna **Windows** sebaiknya cek [fl[Visual Studio
Community|https://visualstudio.microsoft.com/vs/community/]]. Atau, jika
menginginkan pengalaman yang lebih mirip Unix (direkomendasikan!), instal
[fl[WSL|https://docs.microsoft.com/en-us/windows/wsl/install-win10]] dan
`gcc`.

Pengguna **Mac** perlu memasang
[fl[XCode|https://developer.apple.com/xcode/]], khususnya command line
tools-nya.

Ada banyak compiler di luar sana, dan hampir semuanya bisa dipakai untuk
buku ini. Compiler C++ juga bisa mengompilasi banyak (tapi tidak semua!)
kode C. Sebisa mungkin, pakailah compiler C yang sebenarnya.

## Halaman Utama Resmi

Lokasi resmi dokumen ini ada di
[fl[https://beej.us/guide/bgc/|https://beej.us/guide/bgc/]]. Alamat ini
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
* Para sukarelawan di [fl[cppreference.com|https://en.cppreference.com/]]
  yang membangun jembatan dari spesifikasi menuju dunia nyata.
* Orang-orang yang suka membantu dan berpengetahuan luas di
  [fl[comp.lang.c|https://groups.google.com/g/comp.lang.c]] dan
  [fl[r/C_Programming|https://www.reddit.com/r/C_Programming/]], yang
  menemani penulis melewati bagian-bagian bahasa yang paling sulit.
* Semua orang yang mengirim koreksi dan pull-request, mulai dari
  instruksi yang menyesatkan sampai typo.

Terima kasih! ♥
