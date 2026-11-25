# Beej's Guide to C Programming

Ini adalah sumber (diterjemahkan ke Bahasa Indonesia) dari Beej's Guide to C Programming.

Jika hanya ingin membaca panduan ini, kunjungi situs web [Beej's Guide to C](https://beej.us/guide/bgc/).

Ini ada di sini agar terjemahan Indonesia dapat dikelola bersama dalam satu repositori.

## Instruksi Build

### Dependensi

- [GNU make](https://www.gnu.org/software/make/)
- [Python 3+](https://www.python.org/)
- [Pandoc 2.7.3+](https://pandoc.org/)
- XeLaTeX (tersedia di [TeX Live](https://www.tug.org/texlive/))
- Font [Libertinus](https://github.com/alerque/libertinus) (`otf-libertinus`)
- Font DejaVu Sans Mono

Repositori [bgbspd](https://github.com/beejjorgensen/bgbspd) harus di-clone sebagai sibling dari repositori ini:

```
learn-c/
  bgc/        ← repositori ini
  bgbspd/     ← build system
```

### Build

Dari direktori `id/`:

```
make all
```

Untuk membersihkan build sementara:

```
make clean
```

Untuk membersihkan semua hasil build:

```
make pristine
```

### Output

Hasil build tersimpan di `id/src/`:

- `bgc.html` — HTML satu halaman
- `bgc-wide.html` — HTML satu halaman, layar lebar
- `bgc_*.pdf` — berbagai format PDF
- `split/` — HTML per bab
- `split-wide/` — HTML per bab, layar lebar

## Panduan Terjemahan

Lihat direktori `translation-guide/` untuk panduan editorial dan glosarium istilah.

## Penerjemah

Terjemahan ini dimaintain oleh [dilarangoding](https://github.com/dilarangoding).
