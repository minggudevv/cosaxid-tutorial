# Panduan Manajemen MariaDB: User & Database

Dokumen ini berisi instruksi lengkap untuk administrasi dasar MariaDB melalui Command Line Interface (CLI).

---

## 🚀 1. Manajemen User Admin (Root-Level)

### Membuat User Baru

```sql
-- Masuk ke MariaDB: sudo mysql -u root -p
CREATE USER 'admin_baru'@'localhost' IDENTIFIED BY 'password_anda';

-- Memberikan hak akses penuh
GRANT ALL PRIVILEGES ON *.* TO 'admin_baru'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

```

### Mengubah Password User

```sql
ALTER USER 'admin_baru'@'localhost' IDENTIFIED BY 'password_baru_anda';
FLUSH PRIVILEGES;

```

### Menghapus User

```sql
DROP USER 'admin_baru'@'localhost';

```

---

## 🗄️ 2. Manajemen Database

### Membuat Database Baru

```sql
CREATE DATABASE nama_database_anda;

```

### Melihat Daftar Database

Untuk memastikan database sudah dibuat atau ada:

```sql
SHOW DATABASES;

```

### Menghapus Database

> [!CAUTION]
> Menghapus database akan menghapus **seluruh tabel dan data** di dalamnya secara permanen.

```sql
DROP DATABASE nama_database_anda;

```

---

## 📋 Ringkasan Perintah Cepat

| Aksi | Perintah SQL |
| --- | --- |
| **Buat User** | `CREATE USER 'user'@'host' IDENTIFIED BY 'pass';` |
| **Hapus User** | `DROP USER 'user'@'host';` |
| **Ganti Pass** | `ALTER USER 'user'@'host' IDENTIFIED BY 'new_pass';` |
| **Buat DB** | `CREATE DATABASE nama_db;` |
| **Hapus DB** | `DROP DATABASE nama_db;` |
| **Lihat DB** | `SHOW DATABASES;` |
| **Lihat User** | `SELECT user, host FROM mysql.user;` |

---

## 💡 Tips Tambahan

* Sebelum menghapus database, sangat disarankan untuk melakukan **backup** menggunakan perintah:
`mysqldump -u root -p nama_database > backup_file.sql`
* Selalu akhiri setiap perintah SQL dengan titik koma (`;`).

---

Apakah Anda ingin saya buatkan panduan cara melakukan **backup dan restore** database juga untuk melengkapi file README ini?
