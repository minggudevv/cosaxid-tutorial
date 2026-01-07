# Panduan Membuat User Admin Baru (Root-Level) di MariaDB

Dokumen ini berisi langkah-langkah teknis untuk membuat user baru di MariaDB dengan hak akses administratif penuh yang setara dengan user root.

---

## Prasyarat

* Memiliki akses ke terminal/SSH.
* Memiliki akses ke akun root MariaDB yang ada saat ini.

---

## Langkah-langkah

### 1. Masuk ke Konsol MariaDB

Langkah pertama adalah masuk ke sistem MariaDB menggunakan akun administratif:

```bash
sudo mysql -u root -p

```

### 2. Membuat User Baru

Gunakan perintah berikut untuk membuat user. Ganti admin_baru dan password_anda sesuai keinginan Anda.

```sql
CREATE USER 'admin_baru'@'localhost' IDENTIFIED BY 'password_anda';

```

* **'localhost'**: User hanya bisa login dari server lokal.
* **'%'**: Ganti localhost dengan % jika ingin user bisa login secara remote.

### 3. Memberikan Hak Akses Penuh

Berikan semua izin (privileges) agar user memiliki kemampuan yang sama dengan root:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'admin_baru'@'localhost' WITH GRANT OPTION;

```

### 4. Menyimpan Perubahan

Pastikan MariaDB memuat ulang tabel hak akses agar perubahan segera berlaku:

```sql
FLUSH PRIVILEGES;

```

### 5. Keluar dari Sistem

```sql
EXIT;

```

---

## Verifikasi Akun Baru

Untuk memastikan user baru berfungsi dengan benar, coba login kembali menggunakan kredensial yang baru dibuat:

```bash
mysql -u admin_baru -p

```

Setelah login, Anda bisa mengecek hak akses Anda dengan perintah:

```sql
SHOW GRANTS FOR 'admin_baru'@'localhost';

```

---

## Catatan Keamanan

1. **Password Kuat**: Gunakan kombinasi huruf besar, kecil, angka, dan simbol.
2. **Akses Remote**: Jika Anda menggunakan '%', pastikan Firewall server Anda dikonfigurasi dengan benar.
