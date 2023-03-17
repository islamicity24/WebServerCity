# Bangun dan kembangkan webserver code dan mengonfigurasi NGINX sebagai Proxy Server serta menerapkan Limit Access di NGINX

Untuk membangun dan mengonfigurasi webserver code dengan NGINX sebagai proxy server serta menerapkan limit access, langkah-langkah yang dapat dilakukan adalah sebagai berikut:

1. Install NGINX
Jalankan perintah sudo apt update untuk melakukan update sistem
Setelah itu, jalankan perintah sudo apt install nginx untuk menginstal NGINX
Verifikasi status NGINX dengan menjalankan perintah systemctl status nginx
2. Konfigurasi NGINX sebagai Proxy Server
Buka file konfigurasi NGINX dengan perintah sudo nano /etc/nginx/sites-available/default

Hapus konfigurasi default dan gantikan dengan konfigurasi berikut ini:

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Konfigurasi di atas akan mengarahkan permintaan HTTP ke port 3000 pada localhost.

3. Menerapkan Limit Access di NGINX
Buka file konfigurasi NGINX dengan perintah sudo nano /etc/nginx/sites-available/default

Tambahkan konfigurasi berikut ini pada blok server:

python
```
location /admin {
    allow 192.168.0.0/16;
    deny all;
}
```
Konfigurasi di atas akan membatasi akses ke direktori /admin hanya pada jaringan 192.168.0.0/16.

4. Restart NGINX
Setelah melakukan perubahan konfigurasi, restart NGINX dengan menjalankan perintah sudo systemctl restart nginx
Setelah langkah-langkah di atas dilakukan, NGINX sudah diatur sebagai proxy server dan limit access telah diterapkan untuk direktori /admin pada jaringan 192.168.0.0/16.


Setelah melakukan langkah-langkah di atas, Anda dapat mengakses aplikasi web Anda melalui alamat IP server atau nama domain yang sudah diatur. Selain itu, untuk memastikan bahwa konfigurasi NGINX sudah benar, Anda dapat melakukan uji coba dengan membuka aplikasi web di browser dan mengakses direktori /admin dari jaringan yang diizinkan dan jaringan yang tidak diizinkan.

Misalnya, jika Anda ingin mengakses direktori /admin dari jaringan 192.168.1.0/24 yang diizinkan, Anda dapat membuka browser dan mengakses http://<alamat_ip_server>/admin. Jika akses berhasil, maka konfigurasi NGINX sudah benar. Namun, jika akses ditolak, maka konfigurasi NGINX perlu diperiksa kembali.

Selain itu, jika Anda ingin mengatur limit access pada direktori atau file lainnya, Anda dapat menambahkan konfigurasi pada blok location di file konfigurasi NGINX. Anda juga dapat mengatur limit access dengan menggunakan auth_basic dan auth_basic_user_file untuk memerlukan autentikasi pengguna.
