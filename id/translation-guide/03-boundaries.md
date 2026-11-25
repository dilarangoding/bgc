# 03 — Batasan Kebebasan Terjemahan

## Level 1 — Faithful Translation ✅

Terjemahan langsung makna per kalimat. Digunakan untuk prosa teknis lugas.

## Level 2 — Natural Sentence Restructuring ✅

Mengubah urutan klausa atau memecah kalimat panjang. Makna identik, alasan keterbacaan.

## Level 3 — Idiom and Humor Adaptation ✅ (batasan ketat)

Hanya ketika terjemahan literal gagal mempertahankan efek. **Pertahankan imagery dan metafora asli semaksimal mungkin.**

> *"But up until that moment, they're slippery eels."*
> ✅ *"Tapi sampai saat itu tiba, mereka masih seperti belut licin yang susah dipegang."*
> ❌ *"Tapi sebelum sampai ke sana, pointer terasa sulit dipahami."* ← metafora dihilangkan

## Level 4 — Cultural Localization ⚠️ (tidak digunakan sebagai default)

Referensi budaya asli dipertahankan. Tidak ada referensi asing yang diganti dengan padanan Indonesia tanpa persetujuan eksplisit.

## Level 5 — Unauthorized Rewriting ❌

Tidak pernah diizinkan:
- Menambah penjelasan yang tidak ada di sumber
- Menambah humor atau opini yang tidak didukung original
- Menghilangkan bagian apapun tanpa otorisasi
- Memperhalus sarkasme atau self-deprecation
- Menghilangkan kalimat pendek untuk efek

## Komentar dalam Kode

Komentar `//` dan `/* */` di dalam blok kode **diterjemahkan** ke Bahasa Indonesia.
Yang **tidak** diubah: kode, nama identifier, string literal (termasuk argumen `printf`), blok output, dan perintah shell.
