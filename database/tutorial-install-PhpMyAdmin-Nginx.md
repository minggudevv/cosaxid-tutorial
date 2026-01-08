### 🛠️ Persiapan Awal

Pastikan server Anda sudah terpasang **NGINX**, **MySQL/MariaDB**, dan **PHP-FPM**.

---

### 1. 📥 Download phpMyAdmin

Pertama, kita pindah ke direktori `/var/www` dan unduh versinya yang terbaru.

```bash
cd /var/www
sudo wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz

```

* **Ekstrak file:** `sudo tar xvf phpMyAdmin-latest-all-languages.tar.gz`
* **Ubah nama folder:** `sudo mv phpMyAdmin-*-all-languages phpmyadmin`
* **Hapus file zip:** `sudo rm phpMyAdmin-latest-all-languages.tar.gz`

---

### 2. 🔑 Atur Izin (Permissions)

Agar NGINX bisa membaca file tersebut, kita perlu mengatur kepemilikan foldernya.

```bash
sudo chown -R www-data:www-data /var/www/phpmyadmin
sudo chmod -R 755 /var/www/phpmyadmin

```

---

### 3. 🌐 Konfigurasi NGINX Server Block

Buat file konfigurasi baru untuk phpMyAdmin agar bisa diakses melalui domain (misal: `db.domainkamu.com`).

```bash
sudo nano /etc/nginx/sites-available/phpmyadmin

```

Tempelkan kode berikut (sesuaikan `server_name` dan versi `php-fpm` kamu):

```nginx
server {
    listen 80;
    server_name db.domainkamu.com; # 👈 Ganti dengan domain/subdomainmu
    root /var/www/phpmyadmin;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock; # 👈 Cek versi PHP-mu
    }

    location ~ /\.ht {
        deny all;
    }
}

```

**Aktifkan konfigurasi:**

```bash
sudo ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

```

---

### 4. 🔒 Amankan dengan SSL (Certbot)

Sekarang kita enkripsi koneksinya agar data database kamu tidak disadap.

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y

```

**Jalankan Certbot:**

```bash
sudo certbot --nginx -d db.domainkamu.com

```

*Ikuti instruksi di layar, pilih **2** jika ditanya ingin mengalihkan (redirect) semua trafik HTTP ke HTTPS.*

---

### 5. 🧪 Selesai & Uji Coba

Buka browser dan akses domain kamu:
👉 `https://db.domainkamu.com`

> [!IMPORTANT]
> **Tips Keamanan:** Jangan gunakan user `root` untuk login jarak jauh. Buatlah user database khusus dengan akses terbatas, atau tambahkan proteksi password tambahan (.htpasswd) pada folder phpmyadmin.

---

©2026-COSAX
