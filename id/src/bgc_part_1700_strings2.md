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

[i[Escape sequences]<]

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

[i[`\` backslash escape]<]

Untuk melakukan hal-hal ini, kita memakai yang disebut _escape sequence_.
Bentuknya karakter backslash (`\`) diikuti karakter lain. Kedua (atau
lebih) karakter itu bersama-sama punya makna khusus.

Untuk contoh karakter kutip tunggal kita, kita bisa menaruh escape (yaitu
`\`) di depan tanda kutip tunggal di tengah untuk mengatasinya:

[i[`\'` single quote]<]

``` {.c}
char t = '\'';
```

Sekarang C tahu bahwa `\'` hanya berarti tanda kutip biasa yang ingin
kita cetak, bukan akhir dari deretan karakter.

[i[`\'` single quote]>]

Dalam konteks ini kamu bisa mengatakan "backslash" atau "escape" ("escape
tanda kutip itu") dan developer C akan paham maksudmu. Selain itu,
"escape" dalam konteks ini berbeda dari tombol `Esc` di keyboardmu atau
kode ASCII `ESC`.

### Escape yang Sering Dipakai

Menurut pendapatku yang rendah hati, karakter escape ini mencakup
99,2%^[Angka itu kukarang sendiri, tapi kemungkinan tidak jauh meleset] dari
semua escape.

[i[`\n` newline]]
[i[`\'` single quote]]
[i[`\"` double quote]]
[i[`\\` backslash]]

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

[i[`\a` alert]]
[i[`\b` backspace]]
[i[`\f` formfeed]]
[i[`\r` carriage return]]
[i[`\t` tab]]
[i[`\v` vertical tab]]
[i[`\?` question mark]]

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

[i[`\b` backspace]<]
[i[`\r` carriage return]<]

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

[i[`\b` backspace]>]

Perhatikan bahwa baris 15 juga punya banyak spasi di akhir untuk menimpa
karakter yang sudah ada dari hitung mundur.

Terakhir, ada `fflush(stdout)` yang aneh di situ, apa pun artinya.
Jawaban singkatnya, kebanyakan terminal secara bawaan _line buffered_,
artinya mereka tidak benar-benar menampilkan apa pun sampai ada karakter
newline. Karena kita tidak punya newline (kita hanya punya `\r`), tanpa
baris ini program akan diam saja sampai `Liftoff!` lalu mencetak semuanya
sekaligus dalam sekejap. `fflush()` menimpa perilaku ini dan memaksa
output terjadi _sekarang juga_.

[i[`\r` carriage return]>]

#### Escape Tanda Tanya

[i[`\?` question mark]<]

Untuk apa repot-repot dengan ini? Toh ini berfungsi dengan baik:

``` {.c}
printf("Doesn't it?\n");
```

Dan berfungsi baik juga dengan escape:

``` {.c}
printf("Doesn't it\?\n");   // Perhatikan \?
```

Jadi apa gunanya??!

[i[Trigraphs]<]

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

[i[Trigraphs]>]
[i[`\?` question mark]>]

### Escape Numerik

Selain itu, ada cara untuk menentukan konstanta numerik atau nilai
karakter lain di dalam string atau konstanta karakter.

Kalau kamu tahu representasi oktal atau heksadesimal sebuah byte, kamu bisa
menyertakannya dalam string atau konstanta karakter.

Tabel berikut memakai angka contoh, tapi angka hex atau oktal apa pun bisa
dipakai. Beri nol di depan kalau perlu agar jumlah digitnya sesuai.

[i[`\123` octal value]]
[i[`\x12` hexadecimal value]]
[i[`\u` Unicode escape]]
[i[`\U` Unicode escape]]

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

[i[`\123` octal value]<]

``` {.c}
printf("A\102C\n");  // 102 adalah `B` di ASCII/UTF-8
```

Perhatikan tidak ada nol di depan pada angka oktal saat disertakan dengan
cara ini. Tapi harus tiga karakter, jadi beri nol di depan kalau perlu.

[i[`\123` octal value]>]

[i[`\x12` hexadecimal value]<]

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

[i[`\x12` hexadecimal value]>]


[i[`\u` Unicode escape]<]
[i[`\U` Unicode escape]<]

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

[i[`\u` Unicode escape]>]

Misalnya, bullet itu bisa ditulis dengan `\U` dan empat nol di depan:

``` {.c}
printf("\U00002022 Bullet 1\n");
```

[i[`\U` Unicode escape]>]

Tapi siapa yang punya waktu untuk bertele-tele begitu?

[i[`\` backslash escape]>]
[i[Escape sequences]>]
