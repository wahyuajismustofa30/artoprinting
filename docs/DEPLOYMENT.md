# Panduan Deployment

Website Arto Printing dihosting melalui GitHub Pages. Dokumen ini menjelaskan persiapan dan verifikasi rilis tanpa mengasumsikan konfigurasi deployment yang tidak terlihat di source code.

## 1. Batasan operasional

- Repository sumber: `artoprinting`.
- Domain publik: `www.artoprintingpbg.com`.
- Hosting: GitHub Pages.
- Jangan mengubah konfigurasi domain, DNS, branch sumber Pages, atau pengaturan repository tanpa otorisasi.
- Jangan menganggap perubahan di branch fitur otomatis tampil di website produksi.
- Merge ke branch produksi dan deployment adalah langkah terpisah yang harus mengikuti persetujuan pemilik proyek.

## 2. Checklist sebelum rilis

### A. Tinjau perubahan

- Pastikan perubahan berada pada repository dan branch yang benar.
- Tinjau diff; pastikan tidak ada file rahasia, data yang tidak perlu, atau perubahan di luar ruang lingkup.
- Pastikan link internal, path absolut, canonical, dan navigasi sesuai URL yang berlaku.
- Pastikan file penting GitHub Pages seperti `CNAME` dan `.nojekyll` (jika digunakan) tidak terhapus.

### B. Sinkronisasi partial

```bash
python tools/sinkron-partial.py --cek
```

Jika belum sinkron, periksa sumber dan marker terlebih dahulu, lalu jalankan:

```bash
python tools/sinkron-partial.py
python tools/sinkron-partial.py --cek
```

Script hanya memeriksa halaman yang terdaftar di `HALAMAN`. Saat ini `master-theme.html`, `tamu-undangan/index.html`, dan `tamu-undangan/generator.html` belum memakai marker partial; perubahan header pada halaman tersebut perlu diperiksa manual.

### C. Validasi proyek

```bash
python tools/cek-proyek.py
```

Opsional, untuk pemeriksaan URL gambar eksternal:

```bash
python tools/cek-proyek.py --cek-url-eksternal
```

Pemeriksaan eksternal membutuhkan jaringan. Kegagalan jaringan sementara tidak sama dengan URL gambar yang terkonfirmasi hilang.

### D. Uji manual

Buka halaman yang berubah melalui server lokal:

```bash
python -m http.server 8000
```

Periksa sekurang-kurangnya:

- Beranda dan halaman yang diubah.
- Navigasi desktop dan menu mobile.
- Katalog, detail produk, serta tautan WhatsApp jika alur tersebut terdampak.
- Artikel dan tautan internal bila konten blog berubah.
- Aplikasi tamu undangan bila file di folder tersebut berubah.
- Tampilan mobile, tablet, dan desktop.
- Console browser dan Network untuk error file, modul, atau data.

Server Python lokal tidak mereplikasi seluruh perilaku GitHub Pages. Khususnya, routing 404 dan alias URL perlu diuji pada hosting setelah rilis.

## 3. Proses rilis

1. Selesaikan perubahan pada branch fitur.
2. Jalankan validasi dan pemeriksaan manual.
3. Buat Pull Request ke `main` dengan ringkasan perubahan dan hasil tes.
4. Lakukan review atas diff dan hasil pemeriksaan.
5. Merge hanya setelah mendapat persetujuan.
6. Ikuti sumber deployment GitHub Pages yang saat ini dikonfigurasi di pengaturan repository. Jangan mengubahnya sebagai bagian dari rilis rutin.
7. Tunggu proses publikasi GitHub Pages selesai sebelum melakukan pemeriksaan pascadeploy.

Repository tidak mendokumentasikan workflow build/deploy otomatis khusus; jangan mengasumsikan ada perintah `npm run build`, bundler, atau pipeline tertentu.

## 4. Pemeriksaan setelah rilis

- Buka `https://www.artoprintingpbg.com/` dan halaman yang terdampak.
- Pastikan HTTPS dan domain kanonis bekerja.
- Pastikan CSS, JavaScript module, JSON, gambar, dan font dimuat tanpa error.
- Uji navigasi, tombol, formulir, katalog, dan tautan WhatsApp yang terdampak.
- Periksa URL baru pada `sitemap.xml` dan `robots.txt`.
- Bila perubahan SEO, gunakan Google Search Console dan Rich Results Test sesuai kebutuhan.
- Catat waktu rilis, commit, halaman yang diperiksa, dan temuan.

## 5. Pemulihan bila terjadi masalah

1. Identifikasi commit dan perubahan yang menyebabkan regresi.
2. Jika memungkinkan, lakukan revert terhadap commit terkait melalui Pull Request.
3. Jangan menghapus data atau mengubah konfigurasi hosting untuk mengatasi masalah konten biasa.
4. Setelah pemulihan, jalankan ulang validator dan uji halaman yang terdampak.
5. Catat penyebab dan langkah pencegahan agar masalah tidak terulang.

Jangan melakukan force-push, mengubah DNS, atau mengubah konfigurasi produksi sebagai tindakan pemulihan tanpa persetujuan eksplisit.
