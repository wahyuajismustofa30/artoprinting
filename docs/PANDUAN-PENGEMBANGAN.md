# Panduan Pengembangan

Panduan ini berlaku untuk perubahan pada repository website Arto Printing. Tujuannya menjaga perubahan tetap kecil, kompatibel, dan dapat diperiksa.

## 1. Sebelum mengubah kode

1. Pastikan repository yang dibuka adalah `artoprinting` dan branch kerja bukan branch produksi.
2. Periksa struktur folder serta file terkait.
3. Baca implementasi dan pola halaman yang sudah ada sebelum menambahkan pola baru.
4. Telusuri pemakaian ID HTML, selector CSS, import JavaScript, path data, dan tautan yang terdampak.
5. Hindari mengubah fitur di luar ruang lingkup pekerjaan.

Jangan mengasumsikan ada framework atau proses build. Situs utama menggunakan HTML, CSS, dan ES modules tanpa framework.

## 2. Konvensi umum

- Nama file, folder, dan URL memakai huruf kecil; pisahkan kata dengan tanda hubung.
- Gunakan path absolut dari root, misalnya `/assets/css/base.css`.
- Gunakan elemen HTML semantik dan heading secara berurutan.
- Setiap input/form harus memiliki label yang jelas.
- Gunakan token tema dari `assets/css/base.css`; hindari duplikasi warna dan komponen global.
- Fitur lintas halaman ditempatkan pada modul `components/` atau `features/` bila memang reusable.
- Jangan mengganti ID yang dipakai JavaScript tanpa memperbarui semua pemakainya.
- Jangan menaruh credential, token, atau private key di repository publik.
- Jangan menambahkan dependency baru jika fitur dapat dibuat dengan kemampuan platform yang sudah tersedia.

## 3. Menambah atau mengubah halaman

Gunakan halaman yang paling mirip sebagai acuan. Checklist:

1. Buat/ubah HTML pada lokasi yang sesuai dengan struktur URL.
2. Perbarui title, meta description, canonical, Open Graph, Twitter metadata, dan heading utama.
3. Muat stylesheet dengan urutan tema → layout → komponen → CSS halaman.
4. Pastikan header/footer mengikuti mekanisme partial yang berlaku.
5. Pastikan halaman dengan header interaktif memuat `core/site.js` langsung atau melalui modul halaman.
6. Bila ada JavaScript khusus, buat entry point di `assets/js/pages/`; letakkan logika reusable di `components/` atau `features/`.
7. Perbarui navigasi, `config.js`, alias URL di `404.html`/modul terkait, dan `sitemap.xml` bila URL baru perlu diindeks.
8. Tambahkan halaman ke `HALAMAN` di `tools/sinkron-partial.py` hanya jika halaman memakai marker header/footer yang dikelola script.
9. Jalankan validator dan tinjau tampilan desktop serta mobile.

Catatan: tiga halaman saat ini memakai header utama tetapi belum masuk sinkronisasi marker: `master-theme.html`, `tamu-undangan/index.html`, dan `tamu-undangan/generator.html`. Konsistensi markup dan mekanisme sinkronisasi untuk halaman tersebut masih merupakan pekerjaan teknis terpisah.

## 4. Mengubah header/footer

1. Ubah sumber di `partials/header.html` atau `partials/footer.html`.
2. Jalankan `python tools/sinkron-partial.py`.
3. Jalankan `python tools/sinkron-partial.py --cek`.
4. Periksa diff untuk memastikan hanya blok yang dikelola yang berubah.
5. Periksa halaman yang belum memakai marker secara terpisah.

Jangan mengedit blok di antara marker langsung pada halaman karena perubahan akan tertimpa saat sinkronisasi berikutnya.

## 5. Menambah artikel blog

1. Salin artikel yang ada di `post/` sebagai pola.
2. Buat file `post/<slug>.html`; isi artikel, judul, deskripsi, canonical, Open Graph/Twitter, tanggal, gambar, dan JSON-LD Article.
3. Tambahkan kartu artikel ke `blog/index.html`.
4. Perbarui `CollectionPage` JSON-LD dan `numberOfItems`.
5. Tambahkan URL ke `sitemap.xml`.
6. Tambahkan file artikel ke `HALAMAN` di `tools/sinkron-partial.py`.
7. Jalankan validator. Validator membandingkan file artikel, kartu indeks, JSON-LD, canonical, dan sitemap.

Blog adalah HTML statis; tidak ada CMS atau database artikel.

## 6. Mengubah data katalog

- Pertahankan struktur dan nama field pada `assets/data/produk.json`, `setting.json`, dan `ulasan.json`.
- Periksa relasi `link_produk` dan ID produk.
- Jangan memindahkan file data karena path tersebut digunakan oleh modul layanan.
- Jalankan `python tools/cek-proyek.py` setelah perubahan.

## 7. Pengelolaan aset dan dependensi eksternal

- Pertahankan URL aset eksternal yang sudah dipakai kecuali ada alasan dan verifikasi pengganti.
- Periksa status gambar eksternal dengan `python tools/cek-proyek.py --cek-url-eksternal` bila diperlukan.
- Generator undangan memakai dependensi jsDelivr yang dipatok ke commit tertentu dan SRI. Jangan mengganti URL atau hash tanpa memverifikasi file upstream.
- CSS generator undangan merupakan hasil kompilasi; sumber build belum tersedia di repository.

## 8. Pemeriksaan sebelum menyelesaikan pekerjaan

Jalankan:

```bash
python tools/sinkron-partial.py --cek
python tools/cek-proyek.py
```

Jika perubahan menyentuh URL gambar eksternal dan jaringan tersedia:

```bash
python tools/cek-proyek.py --cek-url-eksternal
```

Setelah itu:

- Baca ulang semua file yang diubah.
- Periksa diff dan pastikan tidak ada perubahan yang tidak terkait.
- Uji halaman yang terdampak di browser pada ukuran mobile dan desktop.
- Periksa console browser, navigasi, tautan, dan alur utama halaman.
- Catat pemeriksaan yang tidak dapat dijalankan; jangan mengklaim lulus bila belum diuji.

## 9. Pull request dan review

- Kerjakan pada branch fitur.
- Buat commit dengan pesan yang menjelaskan perubahan.
- Buat atau perbarui Pull Request menuju `main`.
- Sertakan ringkasan, file yang berubah, hasil validasi, risiko, dan screenshot bila perubahan UI.
- Jangan merge atau deploy tanpa otorisasi yang sesuai.
