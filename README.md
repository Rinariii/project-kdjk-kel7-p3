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

   
**Tips Penggunaaan**: 

## Pembahasan
## Kelebihan Chevereto


## Kekurangan Chevereto



## Perbandingan Chevereto dengan Aplikasi Web Sejenis


## Kesimpulan



## Referensi
