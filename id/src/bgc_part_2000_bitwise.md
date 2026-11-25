<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# Operasi Bitwise

[i[Bitwise operations]<]

Operasi numerik ini memungkinkan kamu memanipulasi bit individual dalam
variabel, cocok karena C adalah bahasa yang begitu low-level^[Bukan
berarti bahasa lain tidak melakukannya---mereka melakukannya. Menarik
betapa banyak bahasa modern memakai operator yang sama dengan C untuk
bitwise.].

Kalau kamu belum akrab dengan operasi bitwise, [flw[Wikipedia punya
artikel bitwise yang bagus|Bitwise_operation]].

## Bitwise AND, OR, XOR, dan NOT

Untuk masing-masing, [usual arithmetic
conversion](#usual-arithmetic-conversions) terjadi pada operand (yang dalam
kasus ini harus bertipe integer), lalu operasi bitwise yang sesuai
dilakukan.

[i[`&` bitwise AND]]
[i[`|` bitwise OR]]
[i[`^` bitwise XOR]]
[i[`~` bitwise NOT]]

|Operasi|Operator|Contoh|
|-|:-:|-|
|AND|`&`|`a = b & c`|
|OR|`|`|`a = b | c`|
|XOR|`^`|`a = b ^ c`|
|NOT|`~`|`a = ~c`|

Perhatikan betapa miripnya mereka dengan operator Boolean `&&` dan `||`.

Operator-operator ini punya varian singkat penugasan seperti `+=` dan
`-=`:

[i[`&=` assignment]]
[i[`|=` assignment]]
[i[`^=` assignment]]

|Operator|Contoh|Bentuk panjang yang setara|
|-|-|-|
|`&=`|`a &= c`|`a = a & c`|
|`|=`|`a |= c`|`a = a | c`|
|`^=`|`a ^= c`|`a = a ^ c`|

## Bitwise Shift

Untuk ini, [i[Integer promotions]] [integer
promotion](#integer-promotions) dilakukan pada setiap operand (yang harus
bertipe integer) lalu bitwise shift dieksekusi. Tipe hasilnya adalah tipe
operand kiri yang sudah dipromosikan.

Bit baru diisi dengan nol, dengan kemungkinan pengecualian yang disebut
pada perilaku yang ditentukan implementasi di bawah.

[i[`<<` shift left]]
[i[`>>` shift right]]

|Operasi|Operator|Contoh|
|-|:-:|-|
|Shift kiri|`<<`|`a = b << c`|
|Shift kanan|`>>`|`a = b >> c`|

Ada juga bentuk singkat yang serupa untuk shift:

[i[`>>=` assignment]]
[i[`<<=` assignment]]

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

[i[Bitwise operations]>]
