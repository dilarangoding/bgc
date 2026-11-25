# Handoff — Proyek Terjemahan Bahasa Indonesia Beej's Guide to C

Dokumen ini dibaca oleh sesi baru. **Fase terjemahan sudah selesai (41/41 bab). Tugas sesi baru adalah REVIEW KUALITAS**, bukan menerjemahkan dari nol.

---

## Konteks Proyek

- **Repo:** fork `dilarangoding/bgc` dari `beejjorgensen/bgc`
- **Lokasi lokal:** `/home/ry/Dev/personal/learn-c/bgc/`
- **Tujuan:** Beej's Guide to C Programming dalam Bahasa Indonesia
- **Build system:** `bgbspd` di `/home/ry/Dev/personal/learn-c/bgbspd/` (alat bersama; jangan diubah)
- **Semua terjemahan ada di:** `bgc/id/` (`id/src/*.md`, `id/website/`, `id/translation-guide/`)
- **JANGAN mengedit** sumber Inggris di `bgc/src/` maupun `bgc/website/`. Itu pembanding.

## Batasan Kerja (dari pemilik proyek, WAJIB)

1. Pekerjaan di sini **hanya menerjemahkan/memperbaiki teks**. Jangan menambah fitur, script, mengubah Makefile/build, atau membuat file bantu di repo.
2. Development lokal mengikuti cara pembuat aslinya: `make all` di `id/`.
3. Jangan mengerjakan hal di luar yang diminta. Kalau ragu, tanya dulu.
4. Judul tetap **"Beej's Guide to C Programming"** (tidak diterjemahkan). Nama seri "Beej's Guide" juga tidak.
5. Gaya bicara pemilik proyek di chat santai (`gue`/`lo`), tapi **isi terjemahan tidak boleh** memakai `gue`, `lo`, `Anda`.

## Cara Build

```bash
cd /home/ry/Dev/personal/learn-c/bgc/id
make all       # HTML (bgc.html, split/, split-wide/) + 8 PDF; makan beberapa menit
make clean
make pristine
```

Output di `id/src/`. Baca di browser: `id/src/split/index.html` atau `id/src/bgc.html`.
Landing page terjemahan: `id/website/index.html` (versi Inggris di `bgc/website/`).
`make all` saat ini bersih: tidak ada error dan tidak ada tautan internal yang menggantung.

---

## Status Chapter

| File | Judul Indonesia | Status |
|---|---|---|
| `bgc_part_0100_boilerplate.md` | Kata Pengantar | ✅ Selesai |
| `bgc_part_0150_intro.md` | Hello, World! | ✅ Selesai |
| `bgc_part_0200_var_stat.md` | Variabel dan Pernyataan | ✅ Selesai |
| `bgc_part_0300_functions.md` | Fungsi | ✅ Selesai |
| `bgc_part_0400_pointers.md` | Pointer---Gemetar Ketakutan! | ✅ Selesai |
| `bgc_part_0500_arrays.md` | Arrays | ✅ Selesai |
| `bgc_part_0600_strings.md` | Strings | ✅ Selesai |
| `bgc_part_0700_structs.md` | Structs | ✅ Selesai |
| `bgc_part_0725_io.md` | Input/Output File | ✅ Selesai |
| `bgc_part_0750_typedef.md` | typedef: Membuat Tipe Baru | ✅ Selesai |
| `bgc_part_0800_pointers_2.md` | Pointer II: Aritmetika | ✅ Selesai |
| `bgc_part_0850_malloc.md` | Alokasi Memori Manual | ✅ Selesai |
| `bgc_part_0900_scope.md` | Scope | ✅ Selesai |
| `bgc_part_1000_types_2.md` | Tipe II | ✅ Selesai |
| `bgc_part_1100_types_3.md` | Tipe III: Konversi | ✅ Selesai |
| `bgc_part_1200_types_4.md` | Tipe IV: Qualifier dan Specifier | ✅ Selesai |
| `bgc_part_1300_multifile.md` | Proyek Multifile | ✅ Selesai |
| `bgc_part_1400_env.md` | Lingkungan di Luar Program | ✅ Selesai |
| `bgc_part_1500_cpp.md` | C Preprocessor | ✅ Selesai |
| `bgc_part_1600_structs2.md` | struct II | ✅ Selesai |
| `bgc_part_1700_strings2.md` | Karakter dan String II | ✅ Selesai |
| `bgc_part_1800_enum.md` | Enumerated Type: enum | ✅ Selesai |
| `bgc_part_1900_ptrptr.md` | Pointer III | ✅ Selesai (heading "Nilai Multibyte" cocok dengan 0800) |
| `bgc_part_2000_bitwise.md` | Operasi Bitwise | ✅ Selesai |
| `bgc_part_2100_variadic.md` | Fungsi Variadic | ✅ Selesai |
| `bgc_part_2150_locale.md` | Locale dan Internasionalisasi | ✅ Selesai |
| `bgc_part_2300_exit.md` | Keluar dari Program | ✅ Selesai (dikerjakan mendahului 2200) |
| `bgc_part_2200_unicode.md` | Unicode, Wide Character, dan Semacamnya | ✅ Selesai |
| `bgc_part_2400_signal.md` | Penanganan Signal | ✅ Selesai |
| `bgc_part_2500_vla.md` | Variable-Length Array (VLA) | ✅ Selesai |
| `bgc_part_2600_goto.md` | goto | ✅ Selesai |
| `bgc_part_2700_typesv.md` | Tipe Bagian V | ✅ Selesai |
| `bgc_part_2800_arrays_2.md` | Array Bagian II | ✅ Selesai |
| `bgc_part_2900_jmp.md` | Long Jump dengan setjmp, longjmp | ✅ Selesai |
| `bgc_part_3000_inctype.md` | Incomplete Type | ✅ Selesai |
| `bgc_part_3100_complex.md` | Bilangan Kompleks | ✅ Selesai |
| `bgc_part_3200_fixedint.md` | Tipe Integer Berlebar Tetap | ✅ Selesai |
| `bgc_part_3300_time.md` | Fungsionalitas Tanggal dan Waktu | ✅ Selesai |
| `bgc_part_3400_threads.md` | Multithreading | ✅ Selesai (anchor `#thread-local`, `#mutex`) |
| `bgc_part_3490_atomics.md` | Atomics | ✅ Selesai |
| `bgc_part_3500_noreturn_align.md` | Function Specifier, Alignment Specifier dan Operator | ✅ Selesai |

---


Catatan urutan: nomor file tidak selalu kelipatan 100 (0725, 0750, 0850, 2150, 3490). Bandingkan `ls bgc/src/bgc_part_*.md` dengan `ls bgc/id/src/bgc_part_*.md` (harus sama, 41 file).

---

## TUGAS SESI BARU: Review Kualitas Tiap Bab

### Masalah yang dilaporkan pemilik proyek
1. **Struktur kalimat tidak sesuai SPOK Indonesia**: masih mengikuti urutan klausa Inggris.
2. **Konteks/nuansa hilang di beberapa tempat**: terutama sapaan `kamu`, suara Beej yang bercakap-cakap, dan humor.
3. Ada **aturan yang kemungkinan membuat hasil jadi ngaco**. Periksa dan usulkan perbaikan aturan, jangan hanya memperbaiki kalimat.

### Aturan yang dicurigai menimbulkan masalah (tinjau serius)
- **[SUDAH DIPUTUSKAN, lihat Catatan Review]** ~~"Kata ganti orang pertama Beej (I): hilangkan atau restructure, bukan `gue`"~~ → hasilnya kata **"penulis"** dipakai sangat banyak (mis. "penulis tidak yakin", "di sistem penulis"). Ini sering terdengar kaku/repetitif. Evaluasi alternatif yang natural: hilangkan subjek, kalimat pasif, "kita", atau bentuk lain, sesuai konteks. Putuskan aturan yang konsisten bersama pemilik proyek, lalu terapkan.
- **"Hilangkan subjek ketika natural"** → pernah membuat `kamu` hilang pada kalimat yang menirukan/menyapa pembaca ("You say…" jadi tanpa subjek). Sudah ada pengecualian, tapi periksa seluruh bab untuk kasus serupa.
- **"Pertahankan istilah Inggris" (pointer, array, string, buffer, dll.)** → campuran Inggris-Indonesia kadang janggal. Periksa konsistensi, jangan menambah istilah baru tanpa persetujuan.
- **"Komentar di blok kode wajib diterjemahkan"** → periksa agar terjemahan komentar tidak mengubah makna teknis dan output yang dikutip tetap sama dengan kode.

### Pola kekakuan yang sudah teridentifikasi
Lihat tabel di `translation-guide/01-voice-and-tone.md` (bagian "Struktur Kalimat (SPOK)"). Ringkas:
- "Yang X adalah Y" (pseudo-cleft), "Ini adalah … yang …"
- "melakukan + kata benda" (melakukan konversi/optimasi), "Kamu bisa …" di hampir tiap kalimat
- "sehingga" berantai, "dengan cara …", "Hal ini …"
- calque seperti "kanonik"
Audit yang sudah dilakukan **hanya berbasis pola (grep)** pada bab 0100–2100. Itu tidak menangkap semuanya.

### Bab yang paling berisiko (dikerjakan cepat, sedikit self-review)
Prioritaskan review di sini:
- **Tinggi:** 1000, 1100, 1200, 1300, 1400 (lima bab dikerjakan sekaligus dalam satu giliran), 1500, 1600, 1700, 1800, 1900
- **Tinggi:** 2000, 2100, 2150, 2200, 2300, 2400, 2500
- **Tinggi:** 2600, 2700, 2800, 2900, 3000, 3100, 3200, 3300, 3400, 3490, 3500
- **Sedang:** 0600, 0700, 0725, 0750, 0800, 0850, 0900
- **Lebih rendah (sudah direvisi sekali):** 0100–0500. Tetap periksa konteks/humor yang hilang.

### Cara mereview satu bab
1. Buka `bgc/src/bgc_part_XXXX_*.md` (Inggris) dan `bgc/id/src/bgc_part_XXXX_*.md` berdampingan, **paragraf demi paragraf**.
2. Per paragraf, periksa:
   - **Makna:** ada yang salah, hilang, atau ditambah? (Dilarang menambah lelucon/penjelasan yang tidak ada di sumber.)
   - **SPOK/keterbacaan:** baca keras-keras. Kalau terdengar seperti terjemahan mesin, susun ulang menurut pola Subjek-Predikat-Objek-Keterangan Indonesia.
   - **Suara Beej:** sapaan `kamu`/`kita`, retorika, humor, kalimat pendek untuk efek.
   - **Konsistensi istilah** dengan glossary di bawah dan dengan bab lain.
   - **Pemakaian "penulis"** yang berlebihan (lihat di atas).
3. Perbaiki seperlunya. **Jangan menyentuh** marker, kode, anchor, URL (lihat invarian).
4. Jalankan QA invarian (di bawah) setelah mengedit, lalu `make all`.
5. Catat temuan aturan (bukan hanya kalimat) di bagian "Catatan Review" di bawah.

### QA invarian (harus tetap sama dengan sumber Inggris setelah edit)
Jalankan dari `/home/ry/Dev/personal/learn-c/bgc`, ganti `XXXX` dan `xxx`:

```bash
EN=src/bgc_part_XXXX_xxx.md
ID=id/src/bgc_part_XXXX_xxx.md

# Jumlah harus sama (EN vs ID):
grep -c '^##' $EN; grep -c '^##' $ID                       # heading
grep -oF '[i[' $EN | wc -l; grep -oF '[i[' $ID | wc -l      # marker indeks
grep -cF '```' $EN; grep -cF '```' $ID                      # code fence
diff <(grep -o '{#[^}]*}' $EN) <(grep -o '{#[^}]*}' $ID)    # anchor identik
diff <(grep -o '|[^]]*\]\]' $EN) <(grep -o '|[^]]*\]\]' $ID)  # URL/ID di [fl[ ]] [flw[ ]]
diff <(grep -o '(#[^)]*)' $EN) <(grep -o '(#[^)]*)' $ID)    # tautan internal

# Kode TANPA komentar harus identik:
strip(){ awk '/^```/{f=!f;next} f' $1 | sed -E 's://.*$::; s:/\*.*\*/::' | sed -E 's/[ ]+$//' | grep -v '^[[:space:]]*$'; }
diff <(strip $EN) <(strip $ID)

# Harus nol (huruf besar/kecil tak berpengaruh):
grep -ci '\bgue\b\|\bAnda\b' $ID
```

Catatan: pada blok `{.zsh}` komentar `#` ikut diterjemahkan, jadi `diff` kode bisa menunjukkan selisih hanya pada baris komentar shell. Itu wajar.

### Hal yang SENGAJA tidak diterjemahkan (jangan "diperbaiki")
- Judul buku; epigraf (kutipan di awal bab); nama produk dan istilah baku.
- Kode, nama identifier, **string literal termasuk argumen `printf`**, dan blok output `{.default}` (output harus cocok).
- Komentar yang sebenarnya menampilkan output program (mis. `// Sunday, February 28`).
- Diagram ASCII di 2400 (prototype `signal()`) karena kolomnya terikat ke kode.
- Baris `\\ ...` di 2500 adalah salah ketik di sumber Inggris; dipertahankan.
- Marker `[nh[...]]`, `[is[...]]`; subentri `[i[...]]` dibiarkan identik dengan Inggris.
- URL, form PayPal, dan script di `id/website/index.html`.
- Kutipan lagu/literatur di footnote (mis. *Breakin' the law*, Hitchhiker's Guide) dibiarkan Inggris.

### Keputusan gaya yang sudah dibuat selama terjemahan (boleh ditinjau ulang)
- Register `kamu` (pembaca), `kita` (penulis + pembaca). Tidak ada `Anda`.
- **[REVISI, disepakati]** Suara pertama Beej ("I", "my") → `aku`/`-ku` untuk opini/humor/ucapan ke pembaca; "on my system" → "di sistemku"; "I" yang hanya membuka klausa teknis → `kita` atau subjek dihilangkan; "penulis" hanya jika sumber menyebut "the author". Tabel lengkap: `translation-guide/01-voice-and-tone.md` bagian "Kata Ganti Beej".
- Istilah: *pass* → "mengoper"; *macro* → "makro"; *scope*, *loop*, *buffer*, *thread*, *mutex* dll. tetap Inggris.
- Heading diterjemahkan, **kecuali** istilah baku (mis. "Block Scope", "Variable Hiding", "Stringification"). Teks tautan internal harus cocok dengan heading targetnya.
- Footnote `[^...]` dan isi tabel ikut diterjemahkan.

## Aturan Terjemahan (dari awal proyek)

### Register
- `kamu` → menyapa pembaca langsung
- `kita` → penulis + pembaca bersama
- Hilangkan subjek ketika natural, **KECUALI** pada kalimat yang menirukan/menyapa pembaca atau berisi dialog/humor
  (mis. *"You say something's still not clear"* → *"Katamu masih ada yang belum jelas"*, bukan *"Masih ada yang belum jelas"*).
  Jika subjek ikut membawa nada atau lelucon, pertahankan `kamu`.
- Hindari calque (terjemahan kata per kata yang tidak lazim), mis. *canonical* → ❌ "kanonik", ✅ "klasik"/"baku"/"yang paling lazim".
  Tanyakan: "Apakah orang Indonesia akan mengucapkan ini?" Jika tidak, cari padanan.
- Dilarang menambah lelucon/ekspresi yang tidak ada di sumber (mis. "Eh, bukan").
- **Perhatikan SPOK:** hindari calque struktur Inggris ("Yang X adalah Y", "Ini adalah ... yang ...", "melakukan + kata benda", "sehingga" berantai). Lihat tabel di `translation-guide/01-voice-and-tone.md`.
- **Judul tetap "Beej's Guide to C Programming"** (jangan diterjemahkan) di Makefile, README, dan teks. Nama seri "Beej's Guide" juga tidak diterjemahkan.
- Terjemahkan juga footnote `[^...]` di bawah halaman dan isi tabel.
- **Komentar di dalam blok kode (`//` dan `/* */`) WAJIB diterjemahkan.** Kode, nama identifier, dan string literal
  (termasuk argumen `printf`) JANGAN diubah, karena output di buku harus tetap cocok. Blok output (`{.default}`) tidak disentuh.
  Pada blok shell (`{.zsh}`), perintahnya tidak diubah tapi komentar `#` ikut diterjemahkan. "on my system" → "di sistemku" (lihat aturan "I" Beej).
- ❌ Jangan gunakan `Anda`, `lo`, `gue`
- Kata ganti orang pertama Beej (I/my): `aku`/`-ku` sesuai tabel di `01-voice-and-tone.md`; BUKAN `gue`, dan BUKAN "penulis" (kecuali sumber menyebut "the author")

### Hierarki Prioritas
1. Akurasi teknis dan semantik
2. Suara dan kepribadian Beej (humor, self-deprecation, dll.)
3. Keterbacaan alami Bahasa Indonesia
4. Struktur kalimat asli

### Glossary (Wajib Konsisten)

**Terjemahkan:**
| English | Indonesian |
|---|---|
| variable | variabel |
| function | fungsi |
| memory | memori |
| compilation | kompilasi |
| type | tipe |
| value | nilai |
| address | alamat |
| statement | pernyataan |
| expression | ekspresi |
| keyword | kata kunci |
| macro | makro |
| assignment | penugasan |

**Pertahankan dalam Inggris:**
`pointer`, `array`, `struct`, `stack`, `heap`, `scope`, `loop`, `compiler`, `linker`, `header file`, `string`, `integer`, `float`, `byte`, `buffer`, `flag`, `bit`, `runtime`, `output`, `input`, `executable`, `preprocessor`, `directive`, `identifier`, `operator`, `storage`, `parameter`

---

## Aturan Special Markers

- `[nh[...]]` → **byte-for-byte identical**, tidak disentuh
- `[is[...]]` → **byte-for-byte identical**, tidak disentuh
- `[i[entri]flag]` → terjemahkan descriptor sesuai glossary, pertahankan flags dan backtick identifiers
- `[fl[teks|URL]]` → terjemahkan teks jika frasa deskriptif, URL tidak disentuh
- `[flw[teks|artikel]]` → terjemahkan teks jika frasa deskriptif, article ID tidak disentuh

Contoh `[i[...]]`:
- `[i[\`printf()\` function]]` → `[i[\`printf()\` fungsi]]`
- `[i[Variables]<]` → `[i[variabel]<]`
- `[i[Pointers]<]` → `[i[Pointers]<]` (pointer tidak diterjemahkan)

---

## Panduan Terjemahan Lengkap

Ada di `bgc/id/translation-guide/`:
- `00-overview.md` — filosofi proyek
- `01-voice-and-tone.md` — suara Beej, kapan `kamu` tidak boleh hilang, calque, dan tabel SPOK
- `02-terminology.md` — glossary lengkap
- `03-boundaries.md` — batasan kebebasan terjemahan (termasuk komentar kode)
- `04-special-markers.md` — aturan detail per marker
- `05-qa-checklist.md` — checklist QA tiga dimensi

Kalau review menemukan aturan yang keliru, **perbarui file panduan ini**, bukan hanya mengoreksi kalimat satu per satu.

---

## Catatan Review (isi oleh sesi review)

Keputusan yang disepakati dengan pemilik proyek (sesi review 1):
- "I"/"my" Beej: `aku`/`-ku` (opini, humor, ke pembaca), "di sistemku" (keterangan sistem), `kita`/tanpa subjek (klausa teknis netral), "penulis" hanya jika sumber menyebut "the author". Header tabel: "Sistemku".
- Tambahan SPOK/idiom: tidak ada kalimat dibuka fragmen "Yang ..."; idiom dicek maknanya (mis. *a mouthful*); sarkasme dipertahankan.
- Sisa "penulis" di bab lain (238 kemunculan saat sesi dimulai, terbanyak di 3490: 44) harus ditinjau satu per satu saat review bab tersebut; cek dengan `grep -ni penulis`.

| Bab | Status review | Catatan |
|---|---|---|
| 1000 | Direview (kalimat per paragraf vs sumber) | 12 "penulis" → `aku`/`-ku`/"sistemku"; 5 perbaikan idiom/SPOK ("a mouthful", "In that case", fragmen "Yang ...", sarkasme, "All that being said"). Invarian lulus. `kamu` dan humor sudah baik, tidak diubah. |
| 1100 | Direview | 4 "penulis" → `aku`/`-ku`/"sistemku"; "seperti yang sudah dikatakan" → "seperti kubilang tadi"; 2 footnote pseudo-cleft ("yang mungkin terjadi … adalah") disusun ulang; "melakukan konversi" → pasif; "Kurasa" dipulihkan. Invarian lulus. Terjemahan selebihnya sudah bagus (humor, `kamu` utuh). |
| 1200 | Direview | 2 "penulis" → `aku`; "I" yang hilang dipulihkan ("kusebutkan", "kucantumkan", "aku sendiri kurang puas"); heading "Kebenaran `const`" (calque) → "`const` Correctness" (istilah baku); typo sumber "type quantifier" dibetulkan jadi "qualifier"; kalimat ambigu diperjelas ("Kalau berada di luar fungsi apa pun…", "Kemungkinan kamu melihatnya…"); "melakukan optimasi" → "mengoptimasi". Invarian lulus. |
| 1300 | Direview | 1 "penulis" → "Aku sendiri sering melakukannya"; fragmen "Yang tidak boleh kamu abaikan" digabung; "very, very top" dipulihkan. Terjemahan selebihnya mudah dicerna, tidak diubah. Invarian lulus. |
| 1400 | Direview | Bab dengan "I" paling banyak hilang: 7 "penulis" + beberapa "I" yang disamarkan jadi pasif/"kita" ("Di bash, kita bisa mengisinya", "kalau program ini dijalankan") dipulihkan jadi `aku`/`-ku`/"sistemku"; pseudo-cleft "Yang kita lakukan … adalah" dibuang; "pay the bills" dipulihkan ("membayar tagihan"); "hardening … left as an exercise" diperjelas; "we'll require that you" → "Kamu wajib". Invarian lulus. `make all` bersih setelah 1000–1300. |
| 1500 | Direview | 12 "penulis" → `aku`/`-ku`/"sistemku" (termasuk footnote "Like a rebel", "cop-out"); "I want"/"I get" dipulihkan di bagian ASSERT; 4 fragmen "Yang ..." dibuang ("Yang tentu saja menyiratkan", "Yang gila", dll.); dangling modifier "Mengaitkan semuanya kembali…" diperbaiki; rumus kuadrat disusun ulang agar terbaca; "sebutir garam" (calque idiom *grain of salt*) → "jangan telan bulat-bulat". Invarian lulus. |
| 1600 | Direview | 19 "penulis" → `aku`/`-ku`/"sistemku"/"CPU-ku"; "when I ask C how big my struct foo is" dipulihkan ("kalau aku bertanya ke C…"); "my opinion, man" ("pendapatku, kawan"); "cheesy" → "ala kadarnya". Invarian lulus. |
| 1700 | Direview | 5 baris "penulis" → `aku`/`-ku`/"sistemku"/"mesinku" (termasuk footnote "I just made up that number" → "kukarang sendiri"); "Saat penulis mengompilasi" → "Waktu kukompilasi, aku mendapat peringatan ini". Sisanya sudah natural. Invarian lulus. |
| 1800 | Direview | Bab pendek dan sudah baik. 1 "penulis" → `aku` ("secara pribadi aku tidak suka"). Invarian lulus. |
| 1900 | Direview | 12 baris "penulis" → `aku`/`-ku`/"sistemku"/"mesinku"/"komputerku"; "I want to try for a gut feel" dan "See how I pass in" dipulihkan ke `aku`; fragmen "Yang nakal dari kita" → "Nakal sekali kita ini"; "compiling GCC with -O3" (kalimat sumber ambigu) dijelaskan jadi "mengompilasinya dengan GCC memakai -O3" tanpa mengubah makna. Invarian lulus. |
| 2000 | Direview | Bab pendek; tidak ada "penulis". 1 perbaikan tata bahasa ("asalkan pastikan nilainya" → "asalkan nilainya"). Invarian lulus. |
| 2100 | Direview | Terjemahan sudah natural, `kamu`/humor utuh. 3 "I" yang hilang dipulihkan ("Aku tahu kita belum membahas…", "Aku juga sudah menyebut…", "Seperti sudah kusebut"). Invarian lulus. |
| 2150 | Direview | 3 "penulis" → `aku`/`-ku` ("Waktu kuambil nilai-nilainya di sistemku", "Di sistemku, aku bisa…"); "I should point out" dipulihkan; "ke kebangsaan lain" (calque) → "ke negara lain dengan mulus". Invarian lulus. |
| 2200 | Direview | 13 "penulis" → `aku`/`-ku`/"sistemku" (sisa "penulisan" = *write*, bukan temuan); "I want to talk about two concepts", "I'll write the values in hex", "I never mentioned any mbstate_t", "Like I said" dipulihkan; kalimat salah ("ketika kamu bisa mengoper" → "yang memungkinkan kamu mengoper"); fragmen "Yang patut dicatat" dibuang. Invarian lulus. |
| 2300 | Direview | Terjemahan sudah baik. "Penulis jarang melihat ini dipakai" (huruf besar, terlewat oleh `grep -c penulis` yang case-sensitive) → "Aku jarang…"; "I've rarely been returning", "I can't stress enough", "if I try to run it" dipulihkan. Invarian lulus. **Catatan: pakai `grep -ci penulis`**. |
| 2400 | Direview | 8 "penulis" → `aku`/`-ku`/"kurasa"/"menurutku"; "I mentioned", "I want to be clear", "I've marked" dipulihkan. Diagram ASCII prototype dibiarkan (sesuai aturan). Invarian lulus. |
| 2500 | Direview | 1 "penulis" → "sistemku"; "On line 8, I have an fflush()" dipulihkan ke "aku menaruh". Baris `\ ...` sumber dipertahankan. Invarian lulus. |
| 2600 | Direview | 6 "penulis" → `aku`/`-ku`; "In this programmer's opinion" (bukan "the author") → "programmer yang satu ini"; "I could have done this", "if I carelessly reverse", "gives me a warning", "when I run it" dipulihkan; kalimat "begitulah bukan cara kerjanya" diperbaiki. "penulis" tersisa 1 = "like-minded authors" (sumber memang "authors"). Invarian lulus. |
| 2700 | Direview | 3 "penulis" → `aku` ("aku menjejalkannya… seperti pemberontak", "Di sini aku menyertakan beberapa makro", "aku memecahnya jadi dua"); "The biggest way this could bite you" diperjelas jadi "Jebakan terbesarnya:". Invarian lulus. |
| 2800 | Direview | 2 "penulis" → `aku`; "I double secret PROMISE" ("Aku JANJI dengan sumpah rahasia ganda"). Invarian lulus. |
| 2900 | Direview | 3 "penulis" → `aku`/`-ku`. Invarian lulus. |
| 3000 | Direview | "I" dipulihkan: "yang kudapat", "Aku hanya tahu satu kasus pemakaian", "sebelum aku mulai". Invarian lulus. |
| 3100 | Direview | 8 "penulis" → `aku`/`-ku`/"kurasa" (termasuk footnote panjang "I _think_"); "But that's as far as I'm going to go", "I've said", "I'm omitting them" dipulihkan; humor tabel ("Bermain _Settlers of Catan_") utuh. Invarian lulus. |
| 3200 | Direview | "what if I wanted… I needed…" dan "if I have something of type" dipulihkan ke `aku`; 1 "penulis" → `aku`; salah arti "setinggi mungkin" → "sebesar mungkin" (*as big as possible*). Invarian lulus. |
| 3300 | Direview | 4 "penulis" → `aku`/`-ku`/"kutahu" ("Oke, jangan hajar aku!", "Tanya aku bagaimana aku tahu"); "dear reader" dan "More on that soon!" dipulihkan; "_the_ waktu" (campuran Inggris) → "_satu-satunya_ waktu". Invarian lulus. |
| 3400 | Direview | 11 "penulis" → `aku`/`-ku`/"kurasa" (sisa "penulisan" = *writing*); banyak "I" dipulihkan ("Aku menamai fungsi utama thread ini", "Saat aku menjalankan thread, aku menghitung `i`…", "Waktu kujalankan…", "aku akan memakai `thread_local` saja", "Aku tidak bilang ini ulah alien... tapi ini ulah alien"); "Which I admit it probably doesn't" → "Dan aku akui, kemungkinan itu tidak membantu". Anchor `#thread-local`, `#mutex` utuh. Invarian lulus. |
| 3500 | Direview | 6 "penulis" → `aku`/`-ku`/"kurasa". Humor ("Itu tidak jujur secara komputasi, tahu", "Suka menyiksa diri, ya?") utuh. Invarian lulus. |
| 3490 | Direview | Bab dengan "penulis" terbanyak. Awalnya 44 baris cocok `grep -ci penulis`, tapi hampir semuanya "penulisan" (= *write*, benar); kata "penulis" sebagai pengganti "I" hanya ±12 (intro "Pada dasarnya, aku akan membahas…", "aku nyaris tidak punya kualifikasi", "di sistemku", "compilerku", "Aku _kira_ idenya", "Sejauh yang kutahu", "bahan yang membantuku menembusnya", dst.). **Pakai `grep -ciw penulis` (kata utuh) agar tidak terkecoh "penulisan".** Invarian lulus. |
| website/index.html | Dicek vs `bgc/website/index.html` | `index.css` identik byte-per-byte; struktur tag HTML identik (beda hanya `lang="id"`, teks, `alt`, `placeholder`, `aria-label`). 8 "penulis" (suara Beej di halaman) → "aku". Tidak ada perubahan styling. |
