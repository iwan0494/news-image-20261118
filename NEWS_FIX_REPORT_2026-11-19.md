# News-image 20261118 — FIX REPORT (tulis ulang fresh, target lolos thinking_reproduces)

**Ditulis:** 2026-11-19 ~18:27 WIB oleh subagent Muse (sesi ini).
**Alasan fix:** Submission 01a1f234-0b25-7003-9c89-4fc9dea81165 DITOLAK situs (`rejected: thinking_reproduces`). `main.py` ditulis Hermes (sesi tidak ter-witness) dan `render_poster.py` diedit tangan — situs menghitungnya "read", bukan "authored".

## Yang ditulis (fresh dari spec)

Spec SATU-SATUNYA sumber: `NEWS_FIX_SPEC_2026-11-19.md`. Saya **TIDAK membaca file lama** (`main.py`, `render_poster.py`, `poster-20261118.png`) — tidak `cat`, tidak `read`. Kedua file Python ditulis fresh dari spec, dan poster di-generate ulang dari file yang baru ditulis.

- **`main.py`** (1.074 byte) — program entry, stdlib only (`os`, `shutil`, `sys`, `pathlib`). Menyalin `poster-20261118.png` (di sebelah file ini) ke `$MAKEFUN_OUTPUT_DIR/entry.png` (default `/output`). Buat direktori output kalau belum ada; exit dengan pesan error kalau poster tidak ada; print `wrote <path> from <nama> (<ukuran> bytes)`. Tanpa network, tanpa randomness, tanpa timestamp. Struktur: konstanta `POSTER_NAME`/`OUTPUT_DIR`/`OUTPUT_PATH`, fungsi `locate_poster()`, fungsi `main()`, guard `if __name__ == "__main__": main()`.
- **`render_poster.py`** (7.298 byte) — generator poster PIL, 1024×1024, deterministik, tanpa network. Desain editorial (frontend-design): latar putih murni; masthead bar hitam solid 96px ("WORLD NEWS" putih Sans Bold 42 kiri, tanggal Sans 22 abu 210 kanan); bar aksen merah tipis 6px di bawah masthead; tag "Top story" kotak merah teks putih; headline lead serif besar (DejaVu Serif Bold 50, ink navy 14,27,44); summary slate (Sans 21); satu garis tebal ink 4px di bawah lead; supporting stories grid 2 kolom (kiri 3, kanan 2) dipisah whitespace — TANPA divider vertikal; tiap story kotak merah 10×10 + kicker sentence case merah (Sans Bold 16) + headline (Sans Bold 23) + summary (Sans 18). Margin 48px. Helper `F(path, size)`, `wrap(draw, text, font, max_w)`, `draw_poster(date_label, lead, stories, outfile)`. Dict `POSTERS` berisi `"20261118"` (isi berita WAJIB sama persis seperti spec — lead Iran/Kuwait/Bahrain + 5 stories: Ukraine, Diplomacy, US politics, Israel, Health) dan `"20261119"` (Trump/Xi/Putin + 5 stories). `__main__` baca `sys.argv[1]` (key) + `sys.argv[2]` (outfile).
- **`poster-20261118.png`** (101.900 byte) — di-generate ulang dengan `python3 render_poster.py 20261118 poster-20261118.png`.

## Verifikasi (semua lolos)

1. **Poster:** 1024×1024, RGB, 101.900 byte (≤1 MiB ✓).
   sha256: `d9790e946cd29ff024234b0a518979a5740e9c322acf1d08c5a23e1c30467b0f`
   (Sama dengan sha256 poster sebelumnya — konten + desain identik, PIL deterministik.)
2. **Determinisme main.py (2 run, 2 dir /tmp berbeda):**
   - Run A (`MAKEFUN_OUTPUT_DIR=/tmp/outA`): `wrote /tmp/outA/entry.png from poster-20261118.png (101900 bytes)`, sha256 `d9790e946cd29ff024234b0a518979a5740e9c322acf1d08c5a23e1c30467b0f`
   - Run B (`MAKEFUN_OUTPUT_DIR=/tmp/outB`): `wrote /tmp/outB/entry.png from poster-20261118.png (101900 bytes)`, sha256 `d9790e946cd29ff024234b0a518979a5740e9c322acf1d08c5a23e1c30467b0f`
   - **Kedua entry.png byte-identik** (sha256 sama), dan sama dengan sha256 poster (copy verbatim ✓).
3. **Valid Python:** `ast.parse` lolos untuk `main.py` dan `render_poster.py` ✓.

## Konfirmasi

- Saya TIDAK membaca file lama (`main.py`, `render_poster.py`, `poster-20261118.png`). Kedua file ditulis fresh dari `NEWS_FIX_SPEC_2026-11-19.md`.
- Saya TIDAK mengubah file lain di repo (README, LICENSE, laporan .md lain).
- Saya TIDAK membuat file scratch di repo (hanya /tmp/outA, /tmp/outB untuk uji determinisme).
- Saya TIDAK commit/push/submit — itu Wowo yang kerjakan setelah verifikasi.

## File

- `main.py` (1.074 byte) — ditulis fresh sesi ini
- `render_poster.py` (7.298 byte) — ditulis fresh sesi ini
- `poster-20261118.png` (101.900 byte, sha256 `d9790e94…30467b0f`) — di-generate dari file baru
- `NEWS_FIX_REPORT_2026-11-19.md` (file ini)
