# Daftar Prioritas Perbaikan Teknis — hamzahquran.com

Disusun berdasarkan besarnya dampak terhadap performa, Core Web Vitals, dan pengalaman pengguna (*User Experience*).

---

## 🚨 Prioritas 1: Perbaikan Kritis (Dampak Terbesar pada LCP & Beban Jaringan)

Perbaikan di kategori ini menargetkan penurunan nilai **Largest Contentful Paint (LCP)** dari **33.5 detik (Mobile)** / **4.5 detik (Desktop)** menjadi di bawah 2.5 detik, serta mengurangi total muatan jaringan yang mencapai **17.7 MB**.

### 1. Optimasi & Kompresi Berkas Video Testimoni
- **Masalah**: Berkas `testi-hamzah1vid.mp4` memiliki ukuran transfer sebesar **12.4 MB** pada seluler dan **11.7 MB** pada desktop.
- **Aksi Teknis**:
  - Jangan memuat (*autoload*) berkas video secara penuh saat halaman pertama kali dibuka.
  - Gunakan teknik *lazy-loading* pada elemen `<video>` atau ganti pemutaran langsung dengan gambar *thumbnail* berukuran kecil yang baru memuat video ketika diklik pengguna.
  - Tambahkan trek teks/keterangan `<track kind="captions">` pada elemen video testimoni untuk mendukung aksesibilitas.

### 2. Resize & Resample Gambar Katalog Utama
- **Masalah**: Banyak gambar dimuat dalam resolusi asli yang jauh lebih besar dari ukuran tampilannya di layar, menyebabkan pemborosan data hingga **1.6 MB** di seluler dan **4.0 MB** di desktop.
- **Aksi Teknis**:
  - **Katalog Produk**: Berkas seperti `8qpp-a5.jpg` (3.8 MB / 1.2 MB) dan `8qpp-a4.jpg` (1.2 MB) yang berukuran asli 1080x1080 piksel hanya ditampilkan seluas 256x256 piksel. Ubah ukuran fisik gambar sesuai dimensi tampilannya.
  - **Hero Banner**: Ubah `hero-mobile.png` (1.1 MB, dimensi asli 935x1212) dan `hero.png` (371 KiB) ke format modern seperti **WebP** atau **AVIF** dengan tingkat kompresi yang lebih efisien.
  - **Logo & Icon**: Ubah gambar logo seperti `sss.png` (diubah dari 3104x1633 ke 58x30) dan `kamilpustaka.png` (diubah dari 377x152 ke 57x23).

### 3. Optimasi Elemen LCP (`img.hero-watermark`)
- **Masalah**: Gambar utama di bagian hero `body > main > section.hero-section > img.hero-watermark` mengalami penundaan rendisi.
- **Aksi Teknis**:
  - Hapus atribut `loading="lazy"` pada gambar hero ini.
  - Tambahkan atribut `fetchpriority="high"` agar peramban mengunduhnya sebagai sumber daya prioritas tertinggi.

---

## ⚡ Prioritas 2: Optimasi Kecepatan Tampil (*Render-Blocking & Script*)

Perbaikan ini bertujuan untuk menghilangkan penundaan pemuatan awal (*First Contentful Paint*) sebesar **1,830 ms** pada seluler.

### 4. Eliminasi Sumber Daya Menghambat Rendisi (*Render-Blocking Resources*)
- **Masalah**: Berkas stylesheet Tailwind CDN (`cdn.tailwindcss.com/3.4.17`, 124.1 KiB), `style.css` (9.3 KiB), serta Google Fonts memblokir pembentukan tampilan pertama halaman.
- **Aksi Teknis**:
  - Ganti penggunaan CDN Tailwind penguji (`cdn.tailwindcss.com`) dengan kompilasi Tailwind CSS lokal terkompresi (*built/purged CSS*) agar hanya memuat class yang digunakan.
  - Pasang kode CSS kritis secara *inline* di dalam tag `<head>` dan tunda (*defer*) pemuatan berkas CSS non-kritis.
  - Gunakan properti `font-display: swap` pada aturan Google Fonts agar teks tetap langsung terlihat menggunakan font bawaan sistem sebelum font kustom selesai diunduh.

### 5. Kurangi & Tunda JavaScript Pihak Ketiga (Unused JS)
- **Masalah**: Terdapat pemborosan eksekusi skrip hingga **131 KiB**, terutama dari skrip pelacak Facebook Pixel (`fbevents.js`, 203.8 KiB).
- **Aksi Teknis**:
  - Tunda pemuatan Facebook Pixel (`fbevents.js`) hingga ada interaksi pengguna (*user interaction*) atau setelah peristiwa `DOMContentLoaded`.
  - Lakukan minifikasi berkas JavaScript lokal `script.js` (potensi hemat 9.6 KiB) dan CSS `style.css` (potensi hemat 2.6 KiB).

---

## ♿ Prioritas 3: Aksesibilitas & Kerapihan Kode (Skor 93 ke 100)

### 6. Perbaiki Kontras Warna Teks di Bagian Footer
- **Masalah**: Rasio kontras warna latar belakang dan teks di bagian bawah halaman terlalu rendah sehingga sulit dibaca.
- **Aksi Teknis**: Pertajam warna teks atau ubah warna latar belakang pada tautan/teks footer seperti **Shopee**, **Tokopedia**, **WhatsApp**, **Email**, **Layanan Pelanggan**, teks hak cipta `© 2026 Hamzah Quran`, dan `Developed by`.

### 7. Perbaiki Hirarki Elemen Judul (*Heading Order*)
- **Masalah**: Urutan tag elemen judul (`<h1>`, `<h2>`, `<h3>`) melompati tingkatan (misalnya pada bagian testimoni `Aisha Rahma`).
- **Aksi Teknis**: Struktur ulang tag HTML agar urutan heading bersifat hirarkis dan sekuensial (contoh: `<h1>` diikuti `<h2>`, lalu `<h3>`) untuk memudahkan navigasi pembaca layar (*screen reader*).
