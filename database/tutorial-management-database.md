---

# 🗄️ Panduan Administrator MariaDB

Panduan ini mencakup instalasi user, manajemen keamanan, dan pengelolaan database melalui Command Line.

---

## 🛠 1. Cara Akses Masuk

Langkah pertama, buka **Terminal** atau **CMD** Anda, lalu jalankan perintah berikut untuk masuk ke sistem MariaDB:

```bash
# Masuk sebagai root (User tertinggi)
sudo mysql -u root -p

```

*Setelah menekan Enter, masukkan password sistem/database Anda.*

---

## 👤 2. Manajemen User

Jalankan perintah ini **setelah** Anda berhasil masuk ke dalam MariaDB (ditandai dengan prompt `MariaDB [(none)]>`).

### A. Membuat User Admin Baru (Setara Root)

```sql
-- Membuat user baru
CREATE USER 'admin_baru'@'localhost' IDENTIFIED BY 'password_kuat_anda';

-- Memberikan hak akses penuh (Superuser)
GRANT ALL PRIVILEGES ON *.* TO 'admin_baru'@'localhost' WITH GRANT OPTION;

-- Menyimpan perubahan
FLUSH PRIVILEGES;

```

### B. Mengubah Password User

```sql
ALTER USER 'admin_baru'@'localhost' IDENTIFIED BY 'password_baru_anda';
FLUSH PRIVILEGES;

```

### C. Menghapus User

```sql
DROP USER 'admin_baru'@'localhost';

```

---

## 🗃️ 3. Manajemen Database

Gunakan perintah ini untuk mengelola data Anda.

### A. Membuat Database

```sql
CREATE DATABASE nama_database_anda;

```

### B. Menghapus Database

> [!WARNING]
> Hati-hati! Perintah ini akan menghapus seluruh data secara permanen dan tidak bisa dikembalikan.

```sql
DROP DATABASE nama_database_anda;

```

### C. Cek Daftar Database & User

```sql
-- Melihat semua database
SHOW DATABASES;

-- Melihat semua user yang ada di sistem
SELECT user, host FROM mysql.user;

```

---

## 🚪 4. Cara Keluar

Untuk kembali ke terminal biasa, ketik:

```sql
EXIT;

```

---

## 💡 Ringkasan Cheat Sheet

| Kategori | Perintah | Fungsi |
| --- | --- | --- |
| **Akses** | `mysql -u user -p` | Masuk ke MariaDB |
| **User** | `CREATE USER` | Membuat akun baru |
| **Izin** | `GRANT ALL` | Memberi akses admin |
| **Password** | `ALTER USER` | Mengganti password |
| **Database** | `CREATE DATABASE` | Membuat database baru |
| **Hapus** | `DROP` | Menghapus User/Database |

---

**Saran Tambahan:** Jika Anda ingin mencoba akses remote (bisa login dari aplikasi seperti DBeaver atau Navicat), Anda perlu mengganti `'localhost'` menjadi `'%'`.
