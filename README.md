# Genshin Arena - Admin Panel Manager

Proyek ini merupakan Aplikasi Panel Admin berbasis desktop yang dibuat untuk memenuhi tugas akhir (UAS) mata kuliah Pemrograman Berbasis Desktop. Aplikasi ini dirancang menggunakan **Java Swing (GUI Manual dengan GroupLayout)** dan dibangun di atas sistem **Java with Ant** agar memiliki kompatibilitas penuh dengan komponen desktop bawaan.

Aplikasi ini berfungsi sebagai sistem manajemen data (Content Management System) terpusat untuk mengelola ekosistem permainan Genshin Arena, yang mencakup data akun pengguna, inventaris senjata, hingga karakteristik karakter yang saling berelasi secara dinamis dengan database MySQL.

---

## 🚀 Fitur Utama

Aplikasi ini mengimplementasikan sistem manajemen data penuh (Full CRUD) yang dilengkapi arsitektur keamanan dan kenyamanan pengguna tingkat lanjut:

*   **Sistem Gerbang Keamanan (Login):** Membatasi akses panel admin melalui pencocokan enkripsi data dinamis pada tabel `users`.
*   **Manajemen 3 Tabel Berelasi (Full CRUD):** Fitur Tampil (JTable), Tambah, Edit, dan Hapus data yang terintegrasi penuh untuk modul **Users**, **Senjata**, and **Karakter**.
*   **Pencarian Data Instan (Live Search):** Mempermudah penyaringan ratusan informasi data secara *real-time* menggunakan parameter SQL dinamis pada baris pencarian.
*   **Validasi Input & Sistem Anti-Crash:** Proteksi otomatis terhadap kolom kosong, kesalahan tipe data (seperti memaksa memasukkan huruf pada kolom angka), serta pencegahan duplikasi nama akun pengguna (*username*).
*   **Dialog Konfirmasi Pengamanan:** Integrasi pop-up konfirmasi interaktif untuk menghindari terhapusnya data penting secara tidak sengaja.
*   **Implementasi SQL JOIN:** Menampilkan relasi antar-tabel secara informatif di mana ID Senjata pada tabel karakter secara otomatis diterjemahkan menjadi nama asli senjata pada antarmuka tabel pengguna.

---

## 🛠️ Spesifikasi Teknis & Prasyarat

Untuk menjalankan atau mengembangkan proyek ini kembali, pastikan lingkungan perangkat lunak memenuhi spesifikasi berikut:

*   **IDE:** Apache NetBeans (Versi 29 atau yang kompatibel)
*   **JDK:** Java Development Kit versi 25.0.2
*   **Build System:** Java with Ant (Bukan Maven/Gradle)
*   **Database:** MySQL Server (Direkomendasikan via XAMPP Control Panel)
*   **Konektor Database:** `mysql-connector-j-9.x.jar` (Ditambahkan ke dalam pustaka proyek lokal / `Libraries`)
*   **Driver Class:** `com.mysql.cj.jdbc.Driver`
*   **URL Koneksi:** `jdbc:mysql://localhost:3306/genshin_arena?useSSL=false&serverTimezone=Asia/Jakarta`

---

## 🗄️ Konfigurasi & Struktur Database

Sebelum menjalankan aplikasi, pastikan layanan Apache dan MySQL pada XAMPP sudah aktif. Buat database baru bernama `genshin_arena` melalui `phpMyAdmin`, lalu eksekusi struktur skrip SQL berikut:

```sql
CREATE TABLE users (
  id_user INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(50) NOT NULL UNIQUE,
  password VARCHAR(50) NOT NULL,
  level VARCHAR(20) NOT NULL DEFAULT 'user'
);

CREATE TABLE senjata (
  id_senjata INT PRIMARY KEY AUTO_INCREMENT,
  nama_senjata VARCHAR(50) NOT NULL,
  tipe_senjata VARCHAR(20) NOT NULL,
  rarity INT NOT NULL,
  base_atk INT NOT NULL
);

CREATE TABLE karakter (
  id_karakter INT PRIMARY KEY AUTO_INCREMENT,
  nama_karakter VARCHAR(50) NOT NULL,
  elemen VARCHAR(20) NOT NULL,
  rarity INT NOT NULL,
  id_senjata INT,
  deskripsi VARCHAR(255),
  FOREIGN KEY (id_senjata) REFERENCES senjata(id_senjata)
);

-- Akun Default untuk Login Sistem
INSERT INTO users (username, password, level) VALUES ('admin', 'admin123', 'admin');
