# Project KDJK Kelompok 7 Paralel 3

# Aplikasi Web "Chevereto"

## Deskripsi Aplikasi

Chevereto adalah aplikasi self-hosted untuk membangun layanan image hosting atau galeri foto online. Dengan Chevereto, pengguna dapat mengunggah, menyimpan, dan membagikan gambar secara mudah melalui antarmuka web yang modern dan responsif.

Platform ini pada dasarnya memungkinkan siapa pun membuat versi pribadi dari layanan seperti Imgur, tetapi dengan kontrol penuh atas data dan kustomisasi. Chevereto mendukung multi-user, sehingga bisa digunakan baik untuk keperluan pribadi, komunitas, maupun bisnis.
## Instalasi

Instal docker beserta docker-compose
```
apt-get update
apt-get install docker-compose
apt-get install docker
```

Buat file docker-compose.yml
```
nano docker-compose.yml
```
```
services:
  chevereto:
    image: chevereto/chevereto:latest
    container_name: chevereto
    restart: unless-stopped
    environment:
      CHEVERETO_DB_HOST: db
      CHEVERETO_DB_USER: chevereto
      CHEVERETO_DB_PASS: chevereto_pass
      CHEVERETO_DB_NAME: chevereto
    ports:
      - "127.0.0.1:8080:80"
    volumes:
      - ./images:/var/www/html/images
    depends_on:
      - db

  db:
    image: mariadb:10.11
    container_name: chevereto-db
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: chevereto
      MYSQL_USER: chevereto
      MYSQL_PASSWORD: chevereto_pass
    volumes:
      - ./db:/var/lib/mysql
```

Jalankan docker compose pada directory yang sama dengan file

```
docker-compose up -d
```

Coba curl untuk mencecek
```
curl -I http://127.0.0.1:8080/

HTTP/1.1 200 OK
Date: Tue, 30 Sep 2025 16:20:16 GMT
Server: Apache/2.4.62 (Debian)
Set-Cookie: PHPSESSID=aeb088c0e880b4cda898250b4e21edef; path=/; domain=127.0.0.1; HttpOnly
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Type: text/html; charset=UTF-8
```
Karena saat di curl sudah 200 OK maka kita akan membuat server nginx nya

Buat server configurasi server nginx 
```
server {
    listen 80;
    server_name chevereto.kdjkp3.my.id;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host              $host;
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Lalu jalankan
```
sudo ln -s /etc/nginx/sites-available/chevereto.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

```

Jalankan certbot untuk https
```
sudo certbot --nginx -d chevereto.kdjkp3.my.id
```

Coba curl ke https
```
curl -I https://chevereto.kdjkp3.my.id

HTTP/2 200
date: Wed, 01 Oct 2025 02:41:50 GMT
content-type: text/html; charset=UTF-8
server: cloudflare
expires: Thu, 19 Nov 1981 08:52:00 GMT
cache-control: no-store, no-cache, must-revalidate
pragma: no-cache
permissions-policy: interest-cohort=()
content-security-policy: frame-ancestors 'none'
x-powered-by: Chevereto 4
nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
cf-cache-status: DYNAMIC
report-to: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=5Xym%2F3qqmQ2ax32qInHL2Cr4J4HIAcnlsrM4rvmKE0diHlcth3AdORj5WWvXU36wcYrXFPrYD3%2FRHVbr6cOSYnn2x%2FsTf43PU9SbWM%2FYIeOhZzfUn%2B4%3D"}]}
set-cookie: PHPSESSID=06ee2d349cb9a7cdb3552faac4d47e10; HttpOnly; Secure; Path=/; Domain=chevereto.kdjkp3.my.id
cf-ray: 98789a68485f2aca-LHR
alt-svc: h3=":443"; ma=86400
```
Maka web bisa diakses di (https://chevereto.kdjkp3.my.id)
## Cara Pemakaian
1. Akses Halaman Web
  Setelah instalasi dan konfigurasi selesai, buka browser dan akses:
  https://chevereto.kdjkp3.my.id
  Anda akan diarahkan ke halaman utama Chevereto.

2. Proses Setup Awal
  Pertama kali masuk, Chevereto akan meminta konfigurasi dasar.
  Masukkan nama situs, alamat email admin, dan buat akun admin.
  Setelah itu, klik Finish Installation.

3. Login Sebagai Admin
  Masuk ke dashboard admin menggunakan akun yang sudah dibuat.
  Dari sini Anda bisa mengatur pengaturan global, tema, registrasi user, serta batasan upload.

4. Mengunggah Gambar
  Klik tombol Upload di menu utama.
  Seret dan jatuhkan gambar atau pilih file dari perangkat.
  Setelah selesai, Chevereto akan memberikan link langsung (direct link), HTML embed, atau BBCode untuk berbagi.

5. Manajemen Galeri
  Buat album untuk mengelompokkan foto.
  Atur privasi album: publik, tersembunyi, atau pribadi.
  Gunakan fitur pencarian untuk menemukan gambar dengan cepat.

**Tips Penggunaaan**: 

1. Gunakan Album untuk Organisasi
  Buat album khusus (misalnya: event, produk, meme, fotografi pribadi) agar gambar mudah dicari dan tidak bercampur.

2. Atur Privasi dengan Bijak
  Gunakan opsi di bawah ini sesuai kebutuhan berbagi atau menjaga kerahasiaan:
  Publik: semua orang bisa lihat.
  Tersembunyi: hanya bisa diakses dengan link langsung.
  Pribadi: hanya pemilik yang bisa lihat.
  
3. Manfaatkan Link Otomatis
  Setelah upload, Chevereto memberikan berbagai format link (direct URL, HTML <img>, BBCode [img]). Pilih sesuai platform tempat gambar akan dibagikan (misalnya forum,   blog, atau media sosial).

4. Kelola Kapasitas Penyimpanan
  Monitor ukuran folder ./images di server. Jika server hampir penuh, pertimbangkan menambahkan storage eksternal (misalnya S3 compatible storage).

5. Batasi Ukuran & Jenis File
  Admin bisa mengatur batas ukuran upload per gambar dan tipe file yang diperbolehkan agar server tidak terbebani.

6. Rutin Backup Database & Folder
  Backup folder ./images dan database MariaDB secara berkala untuk menghindari kehilangan data jika server bermasalah.


## Pembahasan
## Kelebihan Chevereto

1. Kontrol penuh atas data & privasi
   Karena self-hosted, pengguna punya kontrol atas server, data, backup, konfigurasi keamanan. Tidak tergantung pada layanan pihak ketiga yang mungkin tutup atau ada kebijakan yang berubah. 

2. Fleksibilitas dalam kustomisasi
  Bisa ubah tema / tampilan, layout, konfigurasi album publik / privat, upload drag-and-drop, dan konfigurasi pengelolaan gambar seperti watermark, penghapusan metadata EXIF, dsb. 

3. Fitur modern dan lengkap
  - Mendukung upload massal / bulk uploads (banyak file sekaligus). 
  - API untuk integrasi & pengembangan custom. 
  - Dukungan versi untuk images & video. 
  - Fitur manajemen seperti album, tags, kategori, privasi, user permissions. 

4. Skalabilitas & performa yang cukup baik
  Chevereto bisa dijalankan di berbagai jenis server / lingkungan (VPS, Docker, panel hosting, dll). Ada dukungan caching, optimasi gambar, dan dukungan untuk storage eksternal seperti S3 (tergantung edisi). 

5. Tersedia edisi gratis open source
  Untuk penggunaan pribadi yang tidak butuh semua fitur canggih, versi Free sudah cukup banyak fiturnya. 

## Kekurangan Chevereto

1. Fitur terbatas di versi gratis
  Banyak fitur “premium” yang hanya tersedia di versi Lite / Pro/Dibayar. Contohnya: dukungan lebih lanjut untuk storage eksternal, multi-user dengan kontrol lengkap, integrasi tertentu, mungkin limit upload atau fitur moderasi.

2. Butuh kemampuan teknis / pemeliharaan server
  Karena self-hosted, pengguna harus siapkan server (hosting, domain, keamanan, backup), melakukan update software, menangani scaling, dan memantau performa & keamanan. Kalau tidak terbiasa, bisa jadi kendala.

3. Biaya lisensi / fitur tambahan
  Untuk versi dengan fitur lengkap, perlu membeli lisensi atau berlangganan. Jika ingin memakai storage besar atau trafik besar, bakal ada biaya tambahan terkait hosting dan bandwidth.

4. Pengaturan penyimpanan eksternal kadang rumit
  Beberapa pengguna melaporkan bahwa mengkonfigurasi storage seperti S3 / MinIO atau integrasi remote storage kadang tidak langsung mulus, perlu konfigurasi ekstra

5. Skala besar & trafik tinggi bisa jadi tantangan
  Jika jumlah gambar/video sangat banyak dan trafik pengakses tinggi, maka server harus cukup kuat, bandwidth memadai, optimasi caching, mungkin perlu CDN; kalau tidak, loading bisa lambat. Biaya operasional bisa meningkat.

6. Kurva belajar (learning curve)
  Untuk yang belum pernah setup server / administrasi web, mungkin perlu belajar dulu tentang PHP, MySQL, konfigurasi webserver, keamanan HTTPS, backup, dan sebagainya.

7. Kompatibilitas & pembaruan
  Jika tidak mengikuti update reguler, ada risiko bug, celah keamanan, atau ketidakcocokan dengan versi PHP / dependency server yang baru. Juga, modul tambahan / plugin pihak ketiga bisa saja tidak selalu up to date.

8. Fitur tertentu belum ada atau masih berkembang
  Misalnya, dukungan untuk federation (saling terhubung antar komunitas), kalau pengguna ingin sistem seperti Pixelfed, mungkin Chevereto belum mendukung secara penuh. Atau komentar publik/komunitas mungkin masih kurang fleksibel dibanding platform sosial media khusus.

## Perbandingan Chevereto dengan Aplikasi Web Sejenis


## Kesimpulan



## Referensi
https://github.com/chevereto/chevereto
