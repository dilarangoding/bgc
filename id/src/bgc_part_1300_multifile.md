<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Proyek Multifile

[i[Multifile projects]<]

Sejauh ini kita melihat program-program mainan yang sebagian besar muat di
satu file. Tapi program C yang kompleks terdiri dari banyak file yang
semuanya dikompilasi dan di-link menjadi satu executable.

Di bab ini kita akan melihat beberapa pola dan praktik umum untuk
menyusun proyek yang lebih besar.

## Include dan Function Prototype  {#includes-func-protos}

[i[Multifile projects-->includes]<]
[i[Multifile projects-->function prototypes]<]

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

[i[Multifile projects-->function prototypes]>]

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

[i[Multifile projects-->includes]>]

## `static` dan `extern`

[i[`static` storage class]<]
[i[`extern` storage class]<]
[i[Multifile projects-->`static` storage class]<]
[i[Multifile projects-->`extern` storage class]<]

Kamu bisa merujuk ke object di file lain dengan `extern`.

Kamu bisa memastikan variabel dan fungsi di file scope _tidak_ terlihat dari file sumber lain (bahkan kalau `extern` dipakai) dengan kata kunci `static`.

Untuk info lebih lanjut, lihat bagian di buku ini tentang storage-class
specifier [`static`](#static) dan [`extern`](#extern).

[i[`static` storage class]>]
[i[`extern` storage class]>]
[i[Multifile projects-->`static` storage class]>]
[i[Multifile projects-->`extern` storage class]>]

## Mengompilasi dengan Object File

[i[Object files]<]

Ini bukan bagian dari spesifikasi, tapi 99,999% umum di dunia C.

Kamu bisa mengompilasi file C menjadi representasi perantara yang disebut
_object file_. Isinya machine code (yaitu 1 dan 0 dari instruksi
sebenarnya) yang belum dibangun menjadi executable. 

Object file di Windows berekstensi `.OBJ`; di sistem mirip Unix, `.o`.

[i[`gcc` compiler]<]

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

dan sekali tepuk membunuh dua [flw[boid|Boids]]?

[i[`gcc` compiler]>]

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

[i[Object files]>]
[i[Multifile projects]>]
