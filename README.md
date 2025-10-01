# Project KDJK Kelompok 7 Paralel 3

# Aplikasi Web "Chevereto"

## Sekilas Tentang

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
Kemudian kunjungi domain/IP web 

## Cara Pemakaian

- Tampilan aplikasi web
- Fungsi-fungsi utama
- Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot
1. Landing Page
   ![Halaman utama](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20140835.png)
   Tampilan utama aplikasi web memberikan pengguna akses langsung ke fitur-fitur utama aplikasi. Pada halaman ini, pengguna dapat melihat menu navigasi, dan dari sana pengguna bisa mengakses berbagai bagian aplikasi seperti koleksi data dan lainnya.
   
2. Login
   ![Halaman user](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20142736.png)
   Untuk dapat menggunakan fitur secara penuh, pengguna perlu melakukan login. Di halaman user settings ini, pengguna akan melakukan autentikasi, biasanya dengan username dan password yang telah ditentukan.
   
3. Collections
   ![Halaman Collection](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20191618.png)
   Bagian Collections berisi berbagai data yang sudah dimasukkan oleh pengguna. Data ini bisa berupa daftar address, agents, atau entitas lain yang dikelola dalam sistem. Pada contoh ini, beberapa koleksi sudah diisi dengan data dummy 
   
4. Create New Data
   ![membuat data 1](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20192004.png)
   ![membuat data 2](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20192102.png)
   Untuk menambah data baru, pengguna dapat mengakses bagian untuk membuat data. Pengguna akan diminta untuk mengisi beberapa informasi terkait entitas yang ingin ditambahkan.
   
5. Customize
   ![Halaman kustomisasi](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20191645.png)
   Aplikasi ini memungkinkan pengguna untuk mengkustomisasi tampilan web sesuai dengan preferensi mereka. Pengguna dapat mengubah tema, tata letak, atau warna antarmuka aplikasi.
   
6. Shortcut
    ![Shortcut](https://github.com/bunyamin88/Projek_KDJK_kel5_P1/blob/main/SS%20Web/Screenshot%202024-10-10%20191656.png)
   Pengguna juga diberikan kemudahan dalam mengakses berbagai bagian aplikasi dengan menggunakan shortcut. Fitur ini mempercepat navigasi, sehingga pengguna dapat langsung menuju halaman yang sering digunakan tanpa harus melalui beberapa langkah.
   
**Tips Penggunaaan**: 
Untuk memaksimalkan penggunaan aplikasi web ini, pastikan untuk login terlebih dahulu agar dapat mengakses semua fitur, serta gunakan shortcut untuk navigasi yang lebih cepat. Di bagian *Collections*, kelola data dengan cermat dan selalu simpan perubahan setelah mengedit. Jika baru mencoba fitur baru, gunakan data dummy terlebih dahulu agar data real tetap aman. Manfaatkan fitur kustomisasi tampilan, seperti *dark mode* atau pengaturan font, untuk kenyamanan penggunaan, terutama jika digunakan dalam waktu lama. Terakhir, pantau log aktivitas secara berkala untuk memantau perubahan atau aktivitas yang terjadi di aplikasi.


## Pembahasan
## Kelebihan Atomic Data

1. **Skema Fleksibel**: Atomic Data sangat cocok untuk data yang bervariasi, seperti wiki terstruktur atau data semantik, karena memungkinkan setiap objek memiliki atribut yang berbeda. Setiap sumber daya dapat memiliki properti yang berbeda sesuai kebutuhan.

2. **Data Terbuka**: Meskipun lebih sulit dibuat dibandingkan dengan JSON biasa, Atomic Data lebih mudah digunakan kembali dan dipahami. Penggunaan URL untuk setiap properti menjadikan data terdokumentasi sendiri, sehingga lebih mudah diakses dan dipahami oleh berbagai pihak.

3. **Interoperabilitas Tinggi**: Atomic Data sangat bermanfaat jika banyak kelompok harus berbagi skema yang sama. Dengan Atomic Data, skema dapat dibatasi, divalidasi, dan tipe data dijaga keamanannya dengan lebih mudah.

4. **Data Terhubung dan Terdesentralisasi**: Atomic Data memungkinkan penggunaan URL untuk menghubungkan data dari berbagai komputer tanpa membuat salinan. Ini sangat berguna untuk jaringan sosial terdesentralisasi atau data yang tersebar di berbagai lokasi.

5. **Auditabilitas & Versi**: Dengan Atomic Commits, setiap perubahan data disimpan sebagai transaksi yang dapat diputar ulang. Ini memungkinkan adanya riwayat perubahan lengkap dan log audit yang terperinci.

## Kekurangan Atomic Data

1. **Penggunaan Internal Saja**: Jika data hanya digunakan secara internal dan tidak perlu dibagikan kepada pihak luar, menggunakan Atomic Data justru dapat mempersulit proses karena sifatnya yang lebih kompleks dibandingkan format lain seperti JSON.

2. **Big Data**: Untuk data yang sangat besar (dalam ukuran TeraBytes), Atomic Data bukan pilihan ideal. Biaya tambahan untuk validasi skema serta kurangnya alat penyimpanan berskala besar membuatnya kurang efisien untuk skala ini.

3. **Data Video, Audio, atau 3D**: Data multimedia seperti video, audio, atau 3D sebaiknya menggunakan format biner yang dioptimalkan dan skema statis. Kelebihan Atomic Data untuk data yang terhubung dan terdesentralisasi tidak terlalu relevan dalam konteks ini, kecuali hanya untuk penyimpanan metadata.

## Perbandingan Atomic Data dengan Aplikasi Web Sejenis

### 1. **Atomic Data vs JSON-LD**

- **Kelebihan Atomic Data:**
  - **Skema Fleksibel**: Atomic Data memungkinkan penambahan properti apapun pada sumber daya, yang membuatnya lebih dinamis dalam menangani berbagai jenis data.
  - **Validasi Tipe Data**: Atomic Data menggunakan tipe data yang terstruktur dengan baik, sehingga setiap nilai harus divalidasi berdasarkan tipe datanya, memberikan keamanan yang lebih tinggi.
  - **Auditabilitas dan Versi**: Atomic Data mendukung Atomic Commits, yang memungkinkan penyimpanan setiap perubahan sebagai transaksi, memberikan kemampuan audit dan riwayat perubahan yang terperinci.

- **Kelebihan JSON-LD:**
  - **Kompatibilitas**: JSON-LD lebih populer dan didukung oleh berbagai platform seperti Google dan aplikasi web yang membutuhkan data terstruktur untuk SEO.
  - **Kemudahan Penggunaan**: JSON-LD lebih mudah digunakan dan diterapkan karena strukturnya yang lebih sederhana dan langsung dapat diintegrasikan dengan JSON biasa.
  - **Kinerja**: JSON-LD lebih ringan karena tidak memerlukan validasi tipe yang ketat seperti Atomic Data, sehingga lebih cepat dalam memproses data yang sederhana.

### 2. **Atomic Data vs RDF (Resource Description Framework)**

- **Kelebihan Atomic Data:**
  - **Kemudahan Implementasi**: Atomic Data lebih mudah diimplementasikan dibandingkan RDF, karena strukturnya lebih modular dan sederhana.
  - **Penggunaan URL untuk Dokumentasi**: Atomic Data menggunakan URL untuk mendokumentasikan properti, menjadikan data lebih mudah dimengerti tanpa memerlukan dokumentasi eksternal tambahan.
  - **Fleksibilitas**: Atomic Data memberikan fleksibilitas yang lebih tinggi dalam menentukan skema dan properti yang dapat digunakan oleh berbagai sumber data.

- **Kelebihan RDF:**
  - **Standar Terbuka**: RDF telah menjadi standar untuk data terstruktur yang terhubung di web, dengan dukungan dari berbagai organisasi dan platform.
  - **Skalabilitas untuk Dataset Besar**: RDF lebih cocok untuk skala besar, seperti Big Data dan dataset terdistribusi yang besar, karena memiliki alat-alat yang lebih matang untuk pengelolaan skala besar.
  - **Integrasi Semantik**: RDF mendukung integrasi semantik yang lebih baik, memungkinkan data dari berbagai sumber dapat dihubungkan dan dianalisis secara lebih mendalam.

### 3. **Atomic Data vs GraphQL**

- **Kelebihan Atomic Data:**
  - **Auditabilitas & Versi**: Atomic Data menyediakan fitur versioning yang kuat dengan Atomic Commits, memungkinkan setiap perubahan disimpan sebagai transaksi yang dapat diaudit.
  - **Data Terhubung dan Terdesentralisasi**: Dengan menggunakan URL, Atomic Data memudahkan untuk menghubungkan dataset yang tersebar di berbagai sumber tanpa membuat duplikasi data.

- **Kelebihan GraphQL:**
  - **Kontrol Pengguna atas Data**: GraphQL memungkinkan klien menentukan secara spesifik data apa yang mereka inginkan dari server, mengurangi pengambilan data yang berlebihan dan meningkatkan efisiensi.
  - **Komunitas dan Ekosistem yang Lebih Besar**: GraphQL memiliki komunitas pengembang yang lebih luas dan berbagai alat bantu untuk mempermudah pengembangan API.
  - **Kinerja**: GraphQL lebih cepat dalam menangani query data yang kompleks karena hanya memerlukan pengambilan data yang diminta, tanpa memvalidasi tipe atau mengikuti skema ketat seperti Atomic Data.

## Kesimpulan

Atomic Data menawarkan fleksibilitas tinggi dalam pengelolaan data terstruktur dengan skema yang fleksibel, validasi tipe yang ketat, serta kemampuan audit dan versioning yang baik. Namun, untuk kasus penggunaan tertentu seperti skala besar, data internal, atau data multimedia, aplikasi lain seperti RDF, JSON-LD, dan GraphQL mungkin lebih tepat digunakan. Atomic Data unggul dalam kasus di mana interoperabilitas, auditabilitas, dan konektivitas data terdesentralisasi sangat dibutuhkan.


## Referensi

(https://docs.atomicdata.dev/)
