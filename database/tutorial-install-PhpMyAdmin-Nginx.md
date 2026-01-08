### 🛠️ Langkah 1: Persiapan & Download phpMyAdmin

Kita akan mengunduh file resmi dan menempatkannya di direktori web.

1. **Masuk ke direktori `/var/www`:**
```bash
cd /var/www

```


2. **Unduh versi terbaru:**
```bash
sudo wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz

```


3. **Ekstrak dan Rapikan:**
```bash
sudo tar xvf phpMyAdmin-latest-all-languages.tar.gz
sudo mv phpMyAdmin-*-all-languages phpmyadmin
sudo rm phpMyAdmin-latest-all-languages.tar.gz

```



---

### 🔑 Langkah 2: Keamanan Folder & Izin

Kita berikan hak akses kepada user NGINX (`www-data`).

```bash
sudo chown -R www-data:www-data /var/www/phpmyadmin
sudo chmod -R 755 /var/www/phpmyadmin

```

---

### 🛡️ Langkah 3: Konfigurasi Keamanan (Blowfish Secret)

phpMyAdmin membutuhkan 32 karakter acak untuk enkripsi cookie.

1. **Generate kode acak dengan OpenSSL:**
```bash
openssl rand -base64 32

```


*(Salin hasilnya yang muncul di layar)* 📋
2. **Buat file konfigurasi:**
```bash
cd /var/www/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
sudo nano config.inc.php

```


3. **Cari baris `blowfish_secret**` dan tempelkan kode tadi:
```php
$cfg['blowfish_secret'] = 'HASIL_OPENSSL_KAMU_DISINI';

```


*Simpan: `Ctrl+O`, `Enter`, `Ctrl+X`.*

---

### 🌐 Langkah 4: Konfigurasi NGINX (HTTP & SSL Manual)

Karena Certbot terkadang gagal otomatis, kita langsung buat konfigurasi yang mendukung HTTPS.

1. **Buat file konfigurasi baru:**
```bash
sudo nano /etc/nginx/sites-available/phpmyadmin

```


2. **Tempelkan kode di bawah ini** (Sesuaikan domain):
```nginx
server {
    listen 80;
    server_name pma.alfikz.my.id;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name pma.alfikz.my.id;

    root /var/www/phpmyadmin;
    index index.php index.html;

    # Sertifikat SSL dari Certbot
    ssl_certificate /etc/letsencrypt/live/pma.alfikz.my.id/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pma.alfikz.my.id/privkey.pem;

    # Pengaturan PHP
    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}

```


3. **Aktifkan Konfigurasi:**
```bash
sudo ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

```



---

### 🔒 Langkah 5: Aktivasi SSL (Certbot)

Sekarang kita panggil Certbot untuk menerbitkan sertifikatnya.

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d pma.alfikz.my.id

```

> [!NOTE]
> Jika Certbot bertanya "Redirection?", pilih **2 (Redirect)**. Jika muncul error "Could not install", tidak masalah karena kita sudah memasukkan path sertifikat secara manual di Langkah 4.

---

### 🧪 Langkah 6: Uji Coba Akhir

1. **Restart PHP & NGINX:**
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl restart nginx

```


2. **Buka di Browser:**
Akses `https://pma.alfikz.my.id`

---

### 👮 Tips Tambahan (Sangat Disarankan)

Agar lebih aman, jangan login pakai `root`. Buat user admin baru di terminal MySQL:

```sql
CREATE USER 'admin_pma'@'localhost' IDENTIFIED BY 'PasswordKuatKamu';
GRANT ALL PRIVILEGES ON *.* TO 'admin_pma'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

```
