# 05 — QA Checklist per Chapter

Setiap chapter harus melewati tiga dimensi QA secara independen sebelum dianggap selesai.

---

## Dimensi A — Structural Completeness

**Cara verifikasi:** Bandingkan source dan terjemahan element by element.

**Headings:**
- [ ] Jumlah heading per level sama
- [ ] Semua `{#anchor-id}` dipertahankan persis

**Content blocks:**
- [ ] Jumlah paragraf sama
- [ ] Jumlah list items sama (termasuk nested)
- [ ] Jumlah table rows dan columns sama
- [ ] Jumlah code blocks sama
- [ ] Jumlah footnotes sama (inline dan reference)
- [ ] Jumlah blockquotes sama

**Markers:**
- [ ] Jumlah `[i[]]` markers sama
- [ ] Jumlah `[fl[]]` markers sama
- [ ] Jumlah `[flw[]]` markers sama
- [ ] Jumlah `[nh[]]` markers sama dan identik dengan sumber
- [ ] Jumlah `[is[]]` markers sama dan identik dengan sumber
- [ ] Semua range markers `[i[...]<]` memiliki pasangan `[i[...]>]` yang konsisten

---

## Dimensi B — Technical and Semantic Accuracy

**Cara verifikasi:** Baca terjemahan sambil membandingkan dengan sumber kalimat per kalimat.

- [ ] Tidak ada fakta teknis yang berubah
- [ ] Tidak ada instruksi yang diubah artinya
- [ ] Semua kode, output, dan commands identik dengan sumber
- [ ] Semua URL dipertahankan
- [ ] Semua article IDs `[flw[...]]` dipertahankan
- [ ] Semua math expressions dipertahankan
- [ ] Terminology konsisten dengan `02-terminology.md`
- [ ] Descriptor words dalam `[i[...]]` entries konsisten dengan glossary
- [ ] Tidak ada konten yang dihilangkan, disingkat, atau diparafrase

---

## Dimensi C — Editorial Quality and Author's Voice

**Cara verifikasi:** Baca terjemahan sebagai dokumen mandiri.

- [ ] Register konsisten sepanjang chapter (tidak ada pergeseran ke formal)
- [ ] Humor dan drama Beej terasa natural dalam Bahasa Indonesia
- [ ] Kalimat pendek untuk efek dipertahankan sebagai kalimat terpisah
- [ ] Footnote humor terasa seperti bisikan, bukan catatan akademis
- [ ] Tidak ada antusiasme, penjelasan, atau opini yang ditambahkan
- [ ] Pronoun `kamu`/`kita` digunakan secara konsisten dan tepat
- [ ] Subjek `kamu` tidak hilang pada kalimat yang menirukan/menyapa pembaca (cek "You say", "you're", dialog)
- [ ] Tidak ada calque (mis. "kanonik"); setiap frasa terdengar seperti yang akan diucapkan orang Indonesia
- [ ] Tidak ada lelucon/ekspresi tambahan yang tidak ada di sumber
- [ ] Footnote `[^...]` ikut diterjemahkan
- [ ] Komentar di blok kode (`//`, `/* */`) sudah diterjemahkan; kode & string literal tidak berubah
  (uji: hapus semua komentar dari blok kode EN dan ID, hasilnya harus identik)

---

## Tambahan untuk Review Pasca-Terjemahan

- [ ] Dibaca berdampingan dengan sumber Inggris **per paragraf**, bukan hanya per heading
- [ ] Tidak ada makna yang hilang, salah, atau ditambah
- [ ] Kalimat mengikuti SPOK Indonesia (tidak ada "Yang X adalah Y", "Ini adalah … yang …" yang kaku)
- [ ] Kata "penulis" tidak dipakai sebagai pengganti "I"/"my" Beej (pakai `aku`/`-ku`, `kita`, atau hilangkan subjek sesuai tabel di `01-voice-and-tone.md`); `grep -ni penulis` hanya boleh sisa jika sumbernya menyebut "the author"
- [ ] Sapaan `kamu`/`kita` dan nada percakapan Beej masih terasa
- [ ] Lelucon dan retorika Beej masih sampai, tanpa lelucon baru
- [ ] Istilah konsisten dengan glossary dan dengan bab lain
- [ ] Invarian lulus: heading, marker `[i[`, fence, anchor, URL, tautan internal, kode tanpa komentar (perintahnya ada di `HANDOFF.md`)
- [ ] `make all` di `id/` bersih
