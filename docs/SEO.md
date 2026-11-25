# Panduan SEO

SEO situs Arto Printing dikelola langsung melalui HTML statis, metadata, tautan internal, sitemap, robots.txt, dan data terstruktur. Tidak ada plugin SEO atau CMS yang menjadi sumber metadata otomatis.

## 1. Metadata setiap halaman

Pastikan setiap halaman publik memiliki:

- `<title>` yang unik dan menggambarkan isi halaman.
- Meta description yang relevan dan tidak menyalin deskripsi halaman lain.
- Canonical absolut ke URL final dengan domain `https://www.artoprintingpbg.com`.
- Open Graph title, description, URL, dan image yang sesuai.
- Twitter metadata bila digunakan oleh pola halaman.
- Satu heading utama `<h1>` yang menjelaskan topik halaman.

Jangan mengisi metadata dengan klaim yang tidak didukung konten halaman. Pastikan URL canonical tidak mengarah ke URL redirect atau URL parameter yang bukan halaman kanonis.

## 2. URL dan internal linking

- Gunakan slug huruf kecil dan tanda hubung.
- Gunakan path final yang konsisten dan hindari membuat duplikat halaman untuk variasi huruf besar/kecil.
- Perbarui tautan navigasi, konfigurasi path, alias URL lama, canonical, dan sitemap saat mengubah URL.
- Pertahankan `produk.html` selama masih dibutuhkan untuk kompatibilitas tautan produk lama.
- Gunakan teks tautan yang jelas dan relevan dengan tujuan halaman.

## 3. Sitemap dan robots

File terkait:

- `sitemap.xml`: daftar URL publik yang ingin ditemukan mesin pencari.
- `robots.txt`: aturan crawl dan referensi sitemap.

Saat menambah halaman publik atau artikel:

1. Tambahkan URL final ke sitemap.
2. Gunakan nilai `lastmod` hanya bila tanggal perubahan konten diketahui dan benar.
3. Pastikan URL memakai HTTPS dan domain kanonis.
4. Pastikan URL dapat dibuka dan tidak mengarah ke 404/redirect berantai.

Jangan memasukkan halaman internal, file uji, atau URL yang tidak ingin diindeks tanpa pertimbangan.

## 4. Data terstruktur

Implementasi yang didokumentasikan saat ini:

- `LocalBusiness` pada Beranda dan Kontak.
- `Article` pada halaman artikel.
- `CollectionPage` pada indeks blog.

Isi structured data harus sesuai konten yang terlihat dan data bisnis yang terverifikasi. Jangan menambahkan rating, harga, jam buka, atau atribut bisnis lain jika datanya belum tersedia atau belum dapat dibuktikan.

Setelah perubahan, periksa JSON-LD dengan Google Rich Results Test:
https://search.google.com/test/rich-results

## 5. Gambar dan performa

- Gunakan alt text yang menjelaskan fungsi atau isi gambar; gambar dekoratif dapat memakai alt kosong.
- Pertahankan dimensi gambar bila diketahui untuk mengurangi layout shift.
- Gunakan `loading="lazy"` untuk gambar di luar viewport awal bila sesuai.
- Periksa URL gambar eksternal; gunakan validator opsional:

```bash
python tools/cek-proyek.py --cek-url-eksternal
```

Validator ini menguji URL gambar yang ditemukan dari sumber yang didukung script; bukan audit menyeluruh seluruh aset eksternal.

## 6. Pemeriksaan SEO sebelum rilis

- Title, description, canonical, Open Graph/Twitter sesuai.
- Heading dan konten halaman jelas serta tidak duplikat.
- Tautan internal tidak rusak.
- Sitemap memuat halaman publik yang relevan.
- Robots.txt mengarah ke sitemap yang benar.
- JSON-LD valid dan sesuai konten yang terlihat.
- Halaman mobile mudah digunakan dan tidak menyembunyikan konten penting.
- Setelah rilis, pantau pengindeksan dan masalah crawl melalui Google Search Console.

SEO tidak dijamin hanya dengan metadata. Kualitas konten, performa, aksesibilitas, tautan, dan pengalaman pengguna tetap berpengaruh.
