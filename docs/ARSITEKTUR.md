# Arsitektur Website Arto Printing

Dokumen ini menjelaskan struktur aktual proyek. Situs utama adalah website statis; jangan mengasumsikan ada framework, backend, CMS, bundler, atau proses build untuk seluruh situs.

## 1. Gambaran teknologi

- HTML untuk struktur dan konten halaman.
- CSS untuk tema global, layout, komponen, dan stylesheet khusus halaman.
- JavaScript ES modules untuk perilaku dan fitur interaktif.
- JSON statis pada `assets/data/` sebagai sumber data katalog, pengaturan katalog, dan ulasan.
- GitHub Pages sebagai hosting; domain kanonis yang digunakan adalah `https://www.artoprintingpbg.com`.

## 2. Peta direktori

| Lokasi | Tanggung jawab |
|---|---|
| `index.html` | Beranda |
| `laman/*/index.html` | Halaman katalog, layanan, cara pesan, testimoni, struktur organisasi, kontak, dan detail produk |
| `blog/index.html` | Indeks artikel |
| `post/*.html` | Artikel statis |
| `tamu-undangan/` | Aplikasi pembuat link undangan terpisah |
| `master-theme.html` | Galeri/referensi internal komponen UI |
| `produk.html` | Jalur kompatibilitas untuk URL detail produk lama |
| `404.html` | Halaman 404 dan penanganan alias URL |
| `assets/css/` | Tema, layout, komponen, dan CSS halaman |
| `assets/js/config.js` | Konfigurasi yang dipakai modul JavaScript |
| `assets/js/core/` | Inisialisasi situs dan pengalihan URL |
| `assets/js/services/` | Pengambilan data JSON |
| `assets/js/components/` | Komponen perilaku yang dapat digunakan ulang |
| `assets/js/features/` | Fitur lintas halaman |
| `assets/js/pages/` | Entry point JavaScript halaman |
| `assets/data/` | Data JSON statis |
| `partials/` | Sumber HTML header dan footer bersama |
| `tools/` | Script sinkronisasi partial dan validator |
| `docs/` | Dokumentasi pemeliharaan proyek |

## 3. Halaman dan URL

Semua path internal memakai huruf kecil dan path absolut dari root domain.

| Kelompok | URL utama | File |
|---|---|---|
| Beranda | `/` | `index.html` |
| Katalog | `/laman/katalog/` | `laman/katalog/index.html` |
| Layanan | `/laman/layanan/` | `laman/layanan/index.html` |
| Cara pesan | `/laman/cara-pesan/` | `laman/cara-pesan/index.html` |
| Testimoni | `/laman/testimoni/` | `laman/testimoni/index.html` |
| Struktur organisasi | `/laman/struktur-organisasi/` | `laman/struktur-organisasi/index.html` |
| Kontak | `/laman/kontak/` | `laman/kontak/index.html` |
| Detail produk | `/laman/produk/?p=<slug>&id=<id>` | `laman/produk/index.html` |
| Blog | `/blog/` | `blog/index.html` |
| Artikel | `/post/<slug>.html` | `post/*.html` |
| Alat undangan | `/tamu-undangan/` dan `/tamu-undangan/generator.html` | `tamu-undangan/` |

`produk.html` dipertahankan sebagai pengalih kompatibilitas. Jangan menghapusnya tanpa memigrasikan seluruh tautan lama dan data produk.

## 4. CSS dan desain

Urutan umum stylesheet situs utama:

1. `assets/css/base.css`: token tema, reset, tipografi, tombol, dan utilitas.
2. `assets/css/layout.css`: header, navigasi, section, footer, dan layout global.
3. `assets/css/components.css`: komponen lintas halaman.
4. `assets/css/pages/<halaman>.css`: penyesuaian khusus halaman.

Gunakan token semantik dari `base.css`. Jangan menyalin komponen global ke stylesheet halaman.

`tamu-undangan/style.css` adalah CSS hasil kompilasi Tailwind. Sumber build dan lockfile yang dapat mereproduksinya belum tersedia di repository; jangan mengedit atau membangun ulang file tersebut dengan toolchain yang ditebak.

## 5. JavaScript dan alur data

- `assets/js/core/site.js` menginisialisasi menu situs dan tautan berbagi.
- Modul halaman dapat mengimpor `../core/site.js`; halaman tanpa entry point khusus dapat memuat `/assets/js/core/site.js` secara langsung.
- `services/data.js` mengambil JSON statis dengan mekanisme cache.
- Modul `components/` dan `features/` menyediakan perilaku yang digunakan halaman.
- `config.js` menyimpan konfigurasi bersama seperti nomor WhatsApp, path halaman, dan konfigurasi katalog.

Data utama:

| File | Isi |
|---|---|
| `assets/data/produk.json` | Produk berdasarkan kelompok/jenis produk |
| `assets/data/setting.json` | Pengaturan dan deskripsi kategori katalog |
| `assets/data/ulasan.json` | Data ulasan pelanggan |

Jaga nama file, struktur objek, dan nama field JSON karena modul halaman bergantung pada kontrak tersebut.

## 6. Header dan footer bersama

Sumber header/footer ada di `partials/header.html` dan `partials/footer.html`. Halaman yang dikelola sinkronisasi memiliki marker:

```html
<!-- partial:header -->
... isi header ...
<!-- /partial:header -->
```

Script `tools/sinkron-partial.py` mengganti isi di antara marker berdasarkan file pada `partials/`. Halaman dengan header dan footer standar terdaftar pada `HALAMAN` dan wajib memiliki kedua marker. Halaman dengan header utama tetapi footer khusus terdaftar pada `HALAMAN_HEADER_SAJA` dan hanya wajib memiliki marker header.

`master-theme.html`, `tamu-undangan/index.html`, dan `tamu-undangan/generator.html` menggunakan kategori header-saja: header utama tersinkron otomatis, sementara footer demo/aplikasi tetap dipertahankan sebagai markup khusus.

Halaman dengan tombol `.menu-btn` harus memuat `assets/js/core/site.js`, secara langsung atau melalui entry point halaman. Validator `tools/cek-proyek.py` memeriksa pemuatan langsung atau import `core/site.js` dari modul halaman. Halaman struktur organisasi kini memuat modul tersebut secara langsung.

## 7. Pengalihan URL

Pengalihan lama tersebar pada modul berikut:

- `core/redirect-www.js`: normalisasi host www dan HTTPS.
- `core/redirect-lama.js`: alias anchor lama dari beranda.
- `core/redirect-produk.js`: kompatibilitas URL produk lama.
- `core/alias-404.js`: alias URL pendek/huruf besar melalui halaman 404 pada hosting yang mendukung pola GitHub Pages.

Saat mengubah slug atau path, audit juga konfigurasi `config.js`, navigasi, alias 404, tautan internal, canonical, dan sitemap.

## 8. Validasi

Jalankan dari root repository:

```bash
python tools/sinkron-partial.py --cek
python tools/cek-proyek.py
```

Validator memeriksa JSON, referensi lokal HTML/CSS/JavaScript, ID HTML duplikat, link produk, konsistensi artikel blog, sitemap artikel, dan sinkronisasi partial. Pemeriksaan URL gambar eksternal bersifat opsional dan membutuhkan jaringan:

```bash
python tools/cek-proyek.py --cek-url-eksternal
```

Lihat [Panduan pengembangan](PANDUAN-PENGEMBANGAN.md) dan [Panduan deployment](DEPLOYMENT.md) untuk prosedur lengkap.
