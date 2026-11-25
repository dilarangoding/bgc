# 01 — Suara dan Gaya Beej

## Register

Gunakan register informal tapi natural — seperti programmer berpengalaman yang menjelaskan ke programmer lain.

| Situasi | Pilihan |
|---|---|
| Beej menyapa pembaca langsung | `kamu` |
| Beej bicara tentang dirinya sendiri ("I", "my") | `aku` / `-ku` (lihat bagian "Kata ganti Beej") |
| Beej menyertakan dirinya & pembaca ("we") | `kita` |
| Narasi biasa yang secara alami tidak butuh subjek | Hilangkan |
| Kalimat yang menirukan/menyapa pembaca, dialog, atau subjek yang membawa humor | **Pertahankan `kamu`** |
| Situasi apapun | ❌ `Anda`, `lo`, `gue` |

### Kapan subjek TIDAK boleh dihilangkan

> *"What's this? **You say** something's still not clear about this whole C programming language thing?"*

- ❌ "Apa nih? Masih ada yang belum jelas tentang bahasa pemrograman C ini?" (suara Beej menirukan pembaca hilang)
- ✅ "Apa nih? **Katamu** masih ada yang belum jelas soal bahasa pemrograman C ini?"

Beej sering berbicara *kepada* pembaca atau menirukan reaksinya. Hilangkan subjek hanya pada narasi biasa, bukan pada kalimat bernada percakapan.

### Hindari calque

Terjemahan kata per kata yang tidak lazim di Bahasa Indonesia harus diganti padanan yang natural.

| English | ❌ Calque | ✅ Alternatif |
|---|---|---|
| canonical | kanonik | klasik / baku / yang paling lazim |

Tambahkan entri di sini setiap menemukan calque baru. Ujinya: "Apakah orang Indonesia akan mengucapkan ini?"


### Struktur Kalimat (SPOK) --- hindari pola Inggris

Bahasa Indonesia tidak harus mengikuti urutan klausa Inggris. Pola yang sering muncul sebagai calque dan harus dihindari:

| Pola Inggris | ❌ Kaku | ✅ Natural |
|---|---|---|
| *What we have there is X* | "Yang kita punya di situ adalah X" | "Di situ ada X" |
| *What C generally does is pick...* | "Yang umumnya dilakukan C adalah memilih..." | "Umumnya C memilih..." |
| *This is a conversion that the compiler does...* | "Ini adalah konversi yang dilakukan compiler..." | "Compiler melakukan konversi ini..." / "Konversi ini dilakukan compiler..." |
| *This is so common that you should...* | "Ini begitu umum sehingga kamu sebaiknya..." | "Hal ini sangat umum, jadi lakukan saja..." |
| *perform/do + noun* | "melakukan optimasi", "melakukan konversi" | "mengoptimasi", "mengonversi" (jika tidak canggung) |
| *so that ...* berulang | "..., sehingga ..., sehingga ..." | pecah kalimat; pakai "jadi", "agar", "supaya" |

Aturan praktis:
1. Hindari pseudo-cleft "Yang ... adalah ..."; jadikan kalimat biasa bersubjek jelas.
2. Mulai kalimat dengan "Ini adalah ..." hanya jika memang memperkenalkan sesuatu yang baru; lebih sering cukup "Ia ..." atau ulangi nama bendanya.
3. Jangan menambah "Kamu bisa ..." di hampir tiap kalimat; imperatif ("Taruh ...", "Pakai ...") sering lebih natural.
4. Baca keras-keras: jika terdengar seperti Google Translate, susun ulang dari Subjek-Predikat-Objek-Keterangan Indonesia.

## Karakteristik Gaya Beej (dari sumber asli)

### Drama yang dibangun lalu dipatahkan sendiri

> *"Pointers---Cower In Fear! [...] Really? Well, not really. I'm just trying to set you up for success."*

Judul dramatis dan pematahannya harus dipertahankan.

### Kalimat pendek untuk efek

> *"There sure can be lotsa stuff in a C program. Yup."*

`Yup.` adalah pilihan stilistik, bukan kesalahan. Jangan digabung.

### Self-deprecation

> *"And that's my excuse for writing such a hilariously large book for such a small, concise language."*

Jangan diperhalus menjadi kerendahan hati yang sopan.

### Pertanyaan retoris sebagai teknik pedagogi

> *"Now, what is this `#include`? GROSS!"*

Beej mendahului kebingungan pembaca. Pertahankan pola ini.

### Footnote sebagai dua hal

Footnote Beej berfungsi sebagai (1) klarifikasi teknis dan (2) humor sampingan. Keduanya harus dipertahankan. Footnote humor tidak boleh diterjemahkan seperti catatan akademis.

## Yang Tidak Boleh Ditambahkan

- Lelucon atau ekspresi yang tidak ada di sumber
- Antusiasme tambahan ("Keren banget!", "Wow!")
- Penjelasan tambahan yang tidak diminta
- Opini atau kesimpulan yang tidak ada di sumber

## Kata Ganti Beej ("I", "my") --- ATURAN FINAL

Disepakati dengan pemilik proyek. **Kata "penulis" tidak dipakai sebagai pengganti "I".**

| Konteks "I" di sumber | Contoh | Terjemahan |
|---|---|---|
| Opini, humor, ucapan langsung ke pembaca | "I have a shocker for you", "I'll bet one of the next releases has it" | `aku` / `-ku`: "Ada kejutan untukmu", "Aku berani taruhan…" |
| Keterangan sistem Beej | "On my system", "My bytes", "My `char` is signed" | "di sistemku", "di komputerku", header tabel "Sistemku" / "Byte di Sistemku" |
| "I" hanya membuka klausa teknis tanpa nada pribadi | "so I can be sure that…" | `kita`, atau subjek dihilangkan |
| Sumber memang menyebut "the author" | (jarang) | "penulis" |

Catatan:
- Ini menggantikan keputusan lama bahwa `aku` hanya untuk ucapan kutipan programmer. Itu tetap `aku`.
- `gue`, `lo`, `Anda` tetap dilarang.
- Jangan membuang "I" yang membawa nada (mis. *Let me get this straight*, *I hope that cleared it right up*, *I mentioned in the table*). Terjemahkan dengan `aku`/`-ku` ("Biar kuluruskan", "Kuharap itu langsung bikin semuanya jelas", "Tadi kusebut di tabel").
- Kata "penulis" dalam teks terjemahan hanya boleh muncul jika sumbernya menyebut penulis.

## Aturan Tambahan SPOK dan Idiom (hasil review bab 1000)

1. Jangan membuka kalimat dengan fragmen "Yang ..." (calque dari *Which ...*). Jadikan kalimat bersubjek: "Padahal itu terdengar agak konyol, mengingat ...".
2. Idiom jangan diterjemahkan harfiah. Periksa maknanya dulu (mis. *That was a mouthful* = kalimat yang ribet diucapkan, bukan "panjang penjelasannya").
3. Sarkasme (*I hope that cleared it right up*) harus tetap terdengar sarkastik.
4. "Hilangkan subjek ketika natural" tidak berlaku untuk "I" yang membawa nada, dan tidak berlaku untuk `kamu` yang menirukan pembaca.

## Pola yang Masih Dipantau

- "Hilangkan subjek" dan `kamu` pada kalimat yang menirukan pembaca (periksa tiap bab).
- Calque dari pola Inggris (tabel SPOK di atas): "Yang X adalah Y", "Ini adalah … yang …", "melakukan + kata benda", "sehingga" berantai.
