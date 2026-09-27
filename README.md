# Miku Prompt — Koleksi & Builder Prompt

Website galeri prompt AI-image yang mengambil datanya langsung dari folder `/prompts` di repo GitHub ini lewat GitHub API. Upload file `.txt` baru ke folder kategori manapun di `/prompts`, dan otomatis muncul di situs — tidak perlu edit `index.html`.

## Isi paket ini

```
index.html          → halaman utama, hosting-nya di GitHub Pages (atau host statis apa pun)
prompts/             → semua koleksi prompt, dikelompokkan per kategori
  ├─ iphone-filter/
  ├─ candy-style/
  ├─ poster-event/
  ├─ product-ads/
  ├─ kartu-konten/
  ├─ vtuber-logo/
  ├─ gaming-banner/
  ├─ typografi-doodle/
  ├─ kolase-mix/
  ├─ gcam-filter/
  └─ hd-color-boost/
```

222 file `.txt` di atas adalah hasil migrasi penuh dari *Prompt Builder* (`prompt-builder-poster-5-2-1-6-1.html`) — setiap kombinasi gaya × jenis prompt (Otomatis / + Referensi / Full Gabungan) dipecah jadi satu file sendiri, supaya semuanya tetap bisa dipilih satu-satu di galeri, persis seperti prompt lain yang sudah ada sebelumnya. Di setiap folder kategori juga ada satu file tambahan `_info.txt` — bukan prompt, hanya metadata tampilan (lihat bagian di bawah).

## Cara pasang

1. Buka repo `siapagatau/Miku-prompt` di GitHub.
2. Timpa `index.html` di root repo dengan `index.html` dari paket ini (ada perbaikan kecil pada mesin template — lihat bagian *Perubahan pada `index.html`* di bawah).
3. Upload seluruh isi folder `prompts/` ke folder `/prompts` di repo (pertahankan struktur sub-foldernya).
4. Selesai — buka situsnya, kategori & prompt baru langsung muncul otomatis (fetch dari GitHub API, tidak ada proses build).

## Format file prompt (`.txt`)

Setiap file boleh diawali blok "field" opsional untuk bikin form kustomisasi, diakhiri baris `---`, lalu isi prompt di bawahnya:

```
judul = text | Judul utama, cth: Diskon Akhir Tahun
deskripsi = textarea | Deskripsi produk
ukuran = select | 1:1 Square ; 4:5 Portrait ; 9:16 Vertikal
elemen = multiselect | Polaroid ; Musik ; Chat Bubble
wm = checkbox | Sertakan watermark kecil
---
Buat desain untuk {{judul}} ... {{deskripsi}} ... ukuran {{ukuran}} ...
```

- Tipe field yang didukung: `text`, `textarea`, `select`, `multiselect`, `checkbox`.
- Opsi untuk `select` / `multiselect` dipisah pakai **titik koma ( `;` )**, bukan koma — supaya teks tiap opsi boleh mengandung koma sendiri (mis. "9:16 vertical (1080×1920px) — TikTok, IG Story, Reels").
- Placeholder `{{key}}` diganti nilai isian user. Tambahkan default kalau kosong: `{{key|teks default}}`.
- File tanpa blok field / tanpa `---` tetap valid — dianggap prompt statis apa adanya.
- Nama file bebas (boleh pakai emoji + spasi) — nama file itulah yang jadi judul kartu di galeri, dan emoji pertama di judul otomatis dipakai sebagai badge ikon.

## Mempercantik tampilan kategori (`_info.txt`)

Setiap folder kategori boleh diisi satu file `_info.txt` supaya tab & headernya di galeri tidak cuma nama folder polos, tapi ada emoji + judul rapi + deskripsi singkat. File ini **tidak ikut tampil** sebagai kartu prompt — otomatis disembunyikan.

Format:

```
emoji = 🕌
label = Poster Event
deskripsi = Poster acara profesional untuk berbagai tema — religi, wedding, ulang tahun, promo, seminar, kuliner, hingga konser.
```

Kalau sebuah folder tidak punya `_info.txt`, situs tetap jalan normal — label dibuat otomatis dari nama folder (mis. `hd-color-boost` → "Hd Color Boost") dan emoji fallback jadi 📁. Ke-11 kategori di paket ini sudah dilengkapi `_info.txt` masing-masing.

## Gambar banner per kategori (`_thumbnail.*`)

Tiap folder kategori juga boleh diisi **satu file gambar** bernama persis `_thumbnail` dengan ekstensi `.jpg`, `.jpeg`, `.png`, `.webp`, atau `.gif` — misalnya:

```
prompts/vtuber-logo/_thumbnail.jpg
prompts/poster-event/_thumbnail.png
```

Tinggal upload file gambarnya ke folder kategori yang sesuai di GitHub (drag & drop biasa, sama seperti upload file `.txt`) — **tidak perlu tulis URL apa pun**. Situs otomatis mendeteksi file ini lewat GitHub API dan menampilkannya sebagai banner gambar di atas judul kategori saat kategori itu dibuka.

Catatan:
- Nama file harus persis `_thumbnail` (bukan `_thumbnail1`, `thumbnail`, dst.) — hanya ekstensinya yang boleh beda.
- Kalau ada lebih dari satu file yang cocok, yang dipakai cuma satu (yang pertama ditemukan).
- File ini **tidak ikut tampil** sebagai kartu prompt — otomatis disembunyikan, sama seperti `_info.txt`.
- Kalau folder tidak punya `_thumbnail.*`, header kategori tetap tampil normal tanpa banner — fitur ini opsional.
- Ukuran gambar yang disarankan: rasio lebar **16:6** (mis. 1600×600px), format `.webp` atau `.jpg` supaya ringan.

## Perubahan pada `index.html`

- **Fitur baru — `_info.txt` per kategori**: tab & header kategori sekarang bisa punya emoji, judul rapi, dan deskripsi singkat (lihat bagian di atas), bukan cuma nama folder mentah.
- **Bugfix delimiter**: sebelumnya opsi `select`/`multiselect` dipisah koma, sehingga opsi yang mengandung koma di dalam teksnya (contoh ukuran/rasio) ke-split salah jadi beberapa opsi. Sekarang dipisah pakai `;`. Kalau ada prompt lama buatan sendiri yang masih pakai format koma, update dulu ke `;` biar tidak pecah.
- **Fitur baru — banner gambar per kategori (`_thumbnail.*`)**: lihat bagian di atas. Gambar diambil langsung dari file di repo (via GitHub API), bukan dari URL eksternal seperti Unsplash — jadi tinggal upload file ke folder, tidak perlu edit kode atau tulis link manapun.
- **Fitur baru — kartu digabung per gaya, bukan per varian**: sebelumnya tiap kombinasi gaya × jenis prompt (Otomatis / + Referensi / Full Gabungan) muncul sebagai kartu terpisah di grid, jadi satu gaya bisa muncul 3× dan bikin grid penuh. Sekarang file-file itu otomatis dikelompokkan jadi **satu kartu per gaya** (dideteksi dari nama file berformat `Nama Gaya — Jenis Prompt.txt`), dan jenis prompt-nya dipilih lewat tombol "🔀 Jenis Prompt" di dalam modal. Tidak perlu ubah struktur file apa pun — pengelompokan ini murni otomatis dari nama file yang sudah ada.
- **Bugfix chip pilihan (select/multiselect)**: sebelumnya kalau teks sebuah opsi mengandung tanda kutip ganda (`"`), tombolnya jadi rusak dan tidak bisa diklik (atribut HTML-nya ke-cut sama tanda kutip itu). Sekarang tiap opsi direferensikan lewat nomor index, bukan menyisipkan teksnya langsung ke atribut HTML — jadi aman dari karakter apa pun.
- **Bugfix tampilan chip**: sebelumnya label tombol pilihan menampilkan seluruh kalimat deskripsi teknis (bisa 100–200 karakter), bikin chip jadi kotak teks panjang yang berantakan. Sekarang tiap opsi otomatis diberi **emoji + label singkat** (diambil dari bagian yang lebih pendek di antara sebelum/sesudah tanda " — " pada teks opsi), sementara deskripsi lengkapnya tetap dipakai utuh sebagai isi prompt akhir — cuma tampilannya yang dirapikan. Teks lengkapnya juga tetap bisa dilihat lewat tooltip (hover/tekan lama) di setiap chip.

## Catatan penyesuaian saat migrasi dari Prompt Builder

Mesin template `index.html` sengaja dibuat sederhana (satu nilai → satu placeholder), sedangkan Prompt Builder lama punya logika kondisional yang lebih rumit (baris tertentu hilang total kalau field dikosongkan, warna custom bisa override dropdown preset, dst). Supaya tidak ada informasi yang hilang, disesuaikan seperti ini:

- **Warna**: dropdown preset + kolom warna custom digabung jadi satu field bebas teks `warna`, dengan nilai default = palet asli dari gaya yang dipilih. Kosongkan untuk otomatis, atau isi bebas untuk override.
- **Elemen kolase**: dijadikan field `multiselect` sungguhan (12 widget), bukan daftar tetap — jadi tetap bisa diubah dari galeri, bukan cuma dari Prompt Builder.
- **Field opsional lain** (deskripsi tambahan, brand, kredit, dll.): kalau dikosongkan, baris terkait tidak hilang total (keterbatasan mesin template ini), tapi diisi teks instruksi seperti *"not provided by the user, the AI may skip this detail entirely"* — supaya AI generator tetap tahu untuk mengabaikannya.
- Setiap **gaya visual** (mis. 10 gaya VTuber, 9 tema poster, dst.) dan setiap **jenis prompt** (Otomatis / + Referensi / Full Gabungan) dipisah jadi file sendiri-sendiri — tidak ada gaya atau varian yang dikurangi dari Prompt Builder asli.

## Menambah prompt baru

Tinggal buat file `.txt` baru (dengan format di atas atau tanpa field sama sekali untuk prompt statis) dan upload ke folder kategori yang sesuai di `/prompts` — atau bikin folder kategori baru kalau perlu. Situs akan otomatis mendeteksinya lewat GitHub API, tidak perlu deploy ulang.
