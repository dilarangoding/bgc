# 04 — Aturan Special Markers

## Marker yang Aktif Digunakan di bgc

| Marker | Jumlah | Fungsi |
|---|---|---|
| `[i[...]]` | 1.638 | Index entry dalam teks |
| `[flw[...]]` | 50 | Footnote link ke Wikipedia |
| `[fl[...]]` | 39 | Footnote link ke URL umum |
| `[nh[...]]` | 21 | No-hyphenation (PDF only) |
| `[is[...]]` | 15 | Index "see also" |

---

## `[nh[nama_fungsi]]`

**Preservasi penuh byte-for-byte.** Konten selalu berupa nama fungsi C, tidak pernah diterjemahkan.

```
✅ [nh[scalbn]]  →  [nh[scalbn]]
❌ [nh[scalbn]]  →  dihilangkan
```

---

## `[is[Kunci==>see Referensi]]`

**Preservasi penuh.** Semua key adalah istilah teknis/operator yang termasuk kategori "pertahankan" di glossary. Tidak ada cross-dependency dengan prose `[i[...]]` entries (diverifikasi).

```
✅ [is[String==>see `char *`]]  →  [is[String==>see `char *`]]
❌ [is[String==>see `char *`]]  →  [is[String==>lihat `char *`]]
```

---

## `[i[entri]flag]`

Flags (`<`, `>`, `()`, `T`, `-->`) selalu dipertahankan. Isi diterjemahkan atau tidak sesuai jenis konten:

| Jenis konten | Perlakuan |
|---|---|
| Identifier dalam backtick | Tidak diterjemahkan |
| Descriptor words (sesuai glossary "translate") | Terjemahkan |
| Descriptor words (sesuai glossary "pertahankan") | Tidak diterjemahkan |
| Concept entries | Terjemahkan sesuai glossary |

```
✅ [i[`printf()` function]]  →  [i[`printf()` fungsi]]
✅ [i[Variables]<]           →  [i[variabel]<]
✅ [i[Pointers]<]            →  [i[Pointers]<]       ← pointer tidak diterjemahkan
❌ [i[variabel]<] ... [i[variable]>]  ← pair tidak konsisten
```

Range markers harus selalu berpasangan dengan terjemahan yang identik.

---

## `[fl[teks|URL]]`

Teks tampilan diterjemahkan jika frasa deskriptif. URL tidak pernah dimodifikasi.

```
✅ [fl[library reference|URL]]        →  [fl[referensi library|URL]]
✅ [fl[Visual Studio Community|URL]]  →  [fl[Visual Studio Community|URL]]
❌ [fl[library reference|URL_BARU]]   ← URL diubah
```

---

## `[flw[teks|artikel_wikipedia]]`

Teks tampilan diterjemahkan jika frasa deskriptif. Article ID tidak pernah dimodifikasi.

```
✅ [flw[assembly language|Assembly_language]]  →  [flw[bahasa assembly|Assembly_language]]
✅ [flw[Python|Python_(programming_language)]] →  [flw[Python|Python_(programming_language)]]
❌ [flw[assembly language|Bahasa_Assembly]]    ← article ID diubah
```

---

## Ringkasan: Structural vs Translatable

| Komponen | Status |
|---|---|
| Nama marker, brackets, separators, flags | ❌ Structural — tidak disentuh |
| Backtick-delimited identifiers di `[i[...]]` | ❌ Technical — tidak disentuh |
| Isi `[nh[...]]` dan `[is[...]]` | ❌ Tidak disentuh |
| URL di `[fl[...]]`, Article ID di `[flw[...]]` | ❌ Tidak disentuh |
| Descriptor words di `[i[...]]` (sesuai glossary) | ✅ Translatable |
| Display text di `[fl[...]]` dan `[flw[...]]` (frasa deskriptif) | ✅ Translatable |
