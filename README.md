# Sistem Informasi Laundry

Aplikasi web sederhana untuk mengelola pelanggan, transaksi laundry, status cucian, harga layanan, dan laporan.

## Fitur

- Login dan logout admin.
- Dashboard ringkasan pelanggan dan transaksi.
- Tambah, lihat, ubah, dan hapus data pelanggan.
- Pencatatan transaksi laundry beserta rincian jenis dan jumlah pakaian.
- Perubahan status transaksi dan cetak invoice.
- Pengaturan harga laundry per kilogram.
- Ganti password admin.
- Filter laporan berdasarkan rentang tanggal, cetak laporan, dan ekspor PDF.

## Teknologi

- PHP 8.1 atau lebih baru dengan ekstensi `mysqli`.
- MySQL.
- Bootstrap 3 (berkas CSS dan JavaScript tersedia di `assets`).
- Dompdf (tersedia di `assets/dompdf`).

## Menjalankan dengan Laragon

1. Salin atau clone proyek ke folder:

   ```text
   C:\laragon\www\laundry
   ```

2. Jalankan Apache dan MySQL melalui Laragon.
3. Buat database `laundry` melalui phpMyAdmin, lalu jalankan SQL berikut untuk membuat tabel:

   ```sql
   CREATE DATABASE IF NOT EXISTS laundry
     CHARACTER SET utf8mb4
     COLLATE utf8mb4_unicode_ci;
   USE laundry;

   CREATE TABLE admin (
     id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
     username VARCHAR(225) NOT NULL,
     password VARCHAR(225) NOT NULL
   ) ENGINE=InnoDB;

   CREATE TABLE harga (
     harga_per_kilo INT NOT NULL
   ) ENGINE=InnoDB;

   CREATE TABLE pelanggan (
     pelanggan_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
     pelanggan_nama VARCHAR(225) NOT NULL,
     pelanggan_hp VARCHAR(20) NOT NULL,
     pelanggan_alamat TEXT NOT NULL
   ) ENGINE=InnoDB;

   CREATE TABLE transaksi (
     transaksi_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
     transaksi_tgl DATE NOT NULL,
     transaksi_pelanggan INT NOT NULL,
     transaksi_harga INT NOT NULL,
     transaksi_berat INT NOT NULL,
     transaksi_tgl_selesai DATE NOT NULL,
     transaksi_status INT NOT NULL
   ) ENGINE=InnoDB;

   CREATE TABLE pakaian (
     pakaian_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
     pakaian_transaksi INT NOT NULL,
     pakaian_jenis VARCHAR(255) NOT NULL,
     pakaian_jumlah INT NOT NULL
   ) ENGINE=InnoDB;

   INSERT INTO harga (harga_per_kilo) VALUES (6000);
   ```

4. Buat hash password admin menggunakan PHP:

   ```bash
   php -r "echo password_hash('GantiDenganPasswordKuat', PASSWORD_DEFAULT), PHP_EOL;"
   ```

   Masukkan username dan hasil hash tersebut ke tabel `admin`:

   ```sql
   INSERT INTO admin (username, password)
   VALUES ('admin', 'TEMPEL_HASH_PASSWORD_DI_SINI');
   ```

   Ganti `GantiDenganPasswordKuat` dengan password pilihan Anda dan simpan password tersebut dengan aman.

5. Periksa konfigurasi koneksi database di [`koneksi.php`](./koneksi.php). Nilai bawaan proyek menggunakan host `localhost`, user `root`, password kosong, dan database `laundry`. Sesuaikan dengan konfigurasi MySQL lokal Anda.
6. Buka [http://localhost/laundry/](http://localhost/laundry/) di browser dan masuk dengan akun admin yang dibuat di atas.

## Struktur direktori

```text
laundry/
├── admin/       Halaman dashboard dan modul pengelolaan
├── assets/      Bootstrap, jQuery, dan pustaka Dompdf
├── index.php    Halaman login dan dashboard
├── koneksi.php  Konfigurasi koneksi MySQL
├── login.php    Proses autentikasi admin
└── README.md
```

## Catatan

- Folder `assets/dompdf` harus tersedia agar fitur PDF dapat digunakan.
- Password admin baru disimpan menggunakan `password_hash`. Jika database lama masih menggunakan hash MD5, aplikasi akan memverifikasi lalu meningkatkan hash tersebut saat login berhasil.
- Sebelum digunakan di server publik, ubah kredensial database bawaan dan gunakan konfigurasi yang sesuai dengan lingkungan hosting.
