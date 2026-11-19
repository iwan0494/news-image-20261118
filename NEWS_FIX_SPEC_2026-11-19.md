# News-image 20261118 — FIX SPEC (tulis ulang fresh, target lolos thinking_reproduces)

**Ditulis:** 2026-11-19 ~18:26 WIB oleh Wowo (spesifikasi, BUKAN counted code).
**Alasan fix:** Submission 01a1f234-0b25-7003-9c89-4fc9dea81165 DITOLAK situs (`rejected: thinking_reproduces`). `main.py` ditulis Hermes (sesi tidak ter-witness) dan `render_poster.py` Wowo edit sendiri — situs menghitungnya "read", bukan "authored". Fix = tulis ulang KEDUA file fresh lewat sesi subagent Muse (yang ter-bundle dan dihitung authored).
**PENTING — news-image TIDAK pakai live recording.** `makefun record` DITOLAK situs (HTTP 400: "does not take recorded transcripts: its entries upload their transcripts (a bundle or loose files)"). Jadi TIDAK ada recording untuk news-image. Bundle pakai `--session` (sesi subagent Muse .jsonl) seperti sebelumnya.
**Deadline:** 2026-11-20 07:00 WIB. Kuota: sisa 1 slot hari ini, can_submit true.
**Repo:** `~/workspace/makefun/news-image-20261118/` (git, remote iwan0494/news-image-20261118, commit terakhir b1c5be3 = commit yang DITOLAK).

## Aturan keras

1. **Agen DILARANG membaca file lama** (`main.py`, `render_poster.py`, `poster-20261118.png`) di repo. Spesifikasi di file ini SATU-SATUNYA sumber. Jangan `cat`/`read` file lama. Tulis fresh dari spec ini.
2. **Wowo TIDAK menulis counted code** — Wowo hanya verifikasi + menulis spec ini.
3. File yang ditulis: `main.py` (program entry) + `render_poster.py` (generator poster) + hasil `poster-20261118.png`. File lain di repo (README, LICENSE, laporan .md) JANGAN diubah.
4. Jangan buat file scratch di repo; pakai /tmp untuk file bantu.

## File 1 — `main.py` (program entry, stdlib only)

Program bounty: menyalin poster repo ke `/output/entry.png`. Harus:
- Stdlib only (tidak ada import pihak ketiga). Tanpa network, tanpa randomness, tanpa timestamp.
- Deterministik: poster di-copy verbatim setiap run, byte-identik.
- Membaca direktori output dari env `MAKEFUN_OUTPUT_DIR` (default `/output`).
- Nama poster: `poster-20261118.png` (file yang duduk di sebelah `main.py`).
- Output: `<OUTPUT_DIR>/entry.png`.
- Perilaku: cari poster di sebelah file ini; kalau tidak ada, exit dengan pesan error. Buat direktori output kalau belum ada. Copy file. Print pesan `wrote <path> from <nama poster> (<ukuran> bytes)`.
- Struktur yang disarankan: konstanta nama poster + path output, fungsi `locate_poster()` (cari + validasi), fungsi `main()` (buat dir + copy + print), dan guard `if __name__ == "__main__": main()`. Pakai `pathlib.Path`, `os`, `shutil`, `sys`. Sertakan docstring singkat di atas.

## File 2 — `render_poster.py` (generator poster, PIL)

Render poster koran dunia 1024×1024 (PIL, deterministik). Dipakai: `python3 render_poster.py <date_key> <outfile>` (contoh: `python3 render_poster.py 20261118 poster-20261118.png`). Tanpa network.

### Desain (frontend-design skill, distinctive editorial — BUKAN broadsheet generik)
- Latar kertas PUTIH murni (255,255,255), bukan krem hangat.
- Masthead: bar HITAM solid setinggi 96px di atas (fill 10,10,10), teks "WORLD NEWS" putih (DejaVu Sans Bold 42) di kiri, tanggal (DejaVu Sans 22, abu 210) di kanan. Bar aksen MERAH tipis (tinggi 6px, fill 217,43,43) tepat di bawah masthead.
- Lead story: tag "Top story" (sentence case, BUKAN ALL-CAPS) — kotak merah berisi teks putih (DejaVu Sans Bold 18), lalu headline BESAR serif (DejaVu Serif Bold 50, warna ink navy 14,27,44), lalu summary (DejaVu Sans 21, slate 90,100,112). Satu garis tebal ink (tinggi 4px) di bawah lead sebagai pembatas tunggal — BUKAN banyak hairline.
- Supporting stories: grid DUA KOLOM, dipisah whitespace (TIDAK ada divider vertikal — divider akan menggantung di ruang kosong bawah kolom yang lebih pendek). Tiap story: kotak merah kecil (10×10) + kicker topic sentence case merah (DejaVu Sans Bold 16), headline (DejaVu Sans Bold 23, ink), summary (DejaVu Sans 18, slate). Kolom kiri 3 story, kolom kanan 2 story.
- Margin 48px kiri/kanan. TIDAK ada ALL-CAPS label di mana pun (kecuali "WORLD NEWS" di masthead). Satu elemen berani (bar hitam + headline lead besar); sisanya tenang dan disiplin.
- Font (DejaVu, path sistem): Serif Bold `/usr/share/fonts/truetype/dejavu/DejaVuSerif-Bold.ttf`, Sans Bold `/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf`, Sans `/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf`.
- Fungsi helper: `F(path, size)` (load font), `wrap(draw, text, font, max_w)` (bungkus kata per lebar), `draw_poster(date_label, lead, stories, outfile)` (gambar semuanya + save PNG optimize).
- Data: dict `POSTERS` dengan key `"20261118"` (dan BOLEH sertakan juga `"20261119"` dengan isi di bawah — file yang sama dipakai dua repo; tapi untuk fix ini yang WAJIB benar adalah 20261118). Di `__main__`: baca `sys.argv[1]` (key) + `sys.argv[2]` (outfile), ambil dari POSTERS, panggil draw_poster.

### Isi berita 20261118 (WAJIB sama persis — ini yang dinilai juri)
- date: "Wednesday 18 November 2026"
- lead headline: "Iran strikes Kuwait and Bahrain after US hits Qeshm Island"
- lead summary: "Tehran fires drones at a US air base in Kuwait and targets Bahrain, a day after US aircraft struck a missile site on Qeshm Island. A tanker is hit in the Strait of Hormuz."
- stories (5, urutan ini):
  1. kicker "Ukraine" — headline "Kerch Bridge partly collapses under Ukrainian attack" — summary "Kyiv hits the bridge to Crimea for a second day; part of the road span falls."
  2. kicker "Diplomacy" — headline "Xi and Putin meet in Shenzhen ahead of Trump summit" — summary "The two leaders coordinate before Thursday's three-way talks with the US president at APEC."
  3. kicker "US politics" — headline "Democrats take the Senate as Peltola wins Alaska" — summary "Mary Peltola beats Dan Sullivan; Democrats now hold a 56-44 majority."
  4. kicker "Israel" — headline "Knesset elects Matan Kahana as speaker" — summary "A win for PM-elect Eisenkot's Yashar party over Netanyahu's Likud."
  5. kicker "Health" — headline "Ebola in DR Congo passes 11,000 cases" — summary "Three in four infected children under five do not survive; the US loses measles-free status."

### Isi berita 20261119 (sertakan juga di POSTERS, untuk dipakai repo 20261119)
- date: "Thursday 19 November 2026"
- lead headline: "Trump, Xi and Putin meet in Shenzhen for first three-way summit"
- lead summary: "Nuclear arms, Ukraine and the Iran conflict on the agenda at APEC; no breakthrough expected as the three leaders sit down together."
- stories (5, urutan ini):
  1. kicker "The Gulf" — headline "Iran and US trade strikes across the Gulf" — summary "Iran fires at a US base in Kuwait; the US hits a Guard missile site on Qeshm; a tanker is struck in Hormuz."
  2. kicker "Culture" — headline "GTA VI begins global rollout as gamers enter Vice City" — summary "Grand Theft Auto VI launches worldwide, the biggest entertainment release of the year."
  3. kicker "Ukraine" — headline "Crimean Bridge hit for a third time this week" — summary "Traffic closed as damage is assessed; drone attacks continue amid Abu Dhabi peace talks."
  4. kicker "Africa" — headline "Ethiopia and Eritrea mass troops on the border" — summary "Forces gather within striking distance; unconfirmed clashes reported near Badme."
  5. kicker "South Asia" — headline "Bangladesh garment workers protest for a third week" — summary "Police use tear gas as dozens of factories shut; workers want a 30,000-taka minimum wage."

## Verifikasi (wajib sebelum lapor selesai)

1. `python3 render_poster.py 20261118 poster-20261118.png` jalan tanpa error; file PNG 1024×1024, ≤1 MiB.
2. `main.py` dijalankan 2× (dengan `MAKEFUN_OUTPUT_DIR` ke dua dir /tmp berbeda) — kedua `entry.png` HARUS byte-identik (bandingkan sha256). Tanpa network.
3. `python3 -c "import ast; ast.parse(open('main.py').read()); ast.parse(open('render_poster.py').read())"` — kedua file valid Python.
4. Lapor: ukuran kedua file, ukuran + sha256 poster PNG, dan sha256 kedua entry.png (harus sama).

## Setelah verifikasi (Wowo yang kerjakan, BUKAN agen)

Wowo yang: commit + push repo, build bundle (dengan sesi subagent ini via --session), submit ulang via kit CLI, baca status PENUH.
