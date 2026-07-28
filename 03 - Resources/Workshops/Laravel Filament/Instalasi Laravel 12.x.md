---
tags:
  - code/laravel
time: 2025-11-22T11:22:00
---
## Why Laravel ? 

**[Laravel](https://laravel.com/)** adalah sebuah framework PHP open source yang bertujuan untuk memudahkan pengembangan website. Laravel menggunakan arsitektur MVC (Model-View-Controller) dan  menyediakan berbagai fitur serta alat yang berguna untuk mempercepat proses pengembangan web.

Laravel sangat populer di kalangan pengembang karena sifatnya yang open source dan relatif mudah untuk dipelajari karena dokumentasinya yang lengkap. Laravel juga menyediakan berbagai macam fitur seperti [Eloquent ORM](https://laravel.com/docs/12.x/eloquent), [Artisan CLI](https://laravel.com/docs/12.x/artisan), [Database Seeder](https://laravel.com/docs/12.x/seeding), dan lain lain.


### Manfaat Framework

- Mempercepat pengembangan sebuah aplikasi, baik berbasis website, desktop maupun mobile.
- Membantu developer dalam perencanaan, pembuatan dan pemeliharaan sebuah aplikasi.
- Memastikan seluruh tim menggunakan struktur dan gaya kode yang sama sehingga meningkatkan konsistensi sebuah kode.
- Kode yang lebih bersih dan efisien membuat proses debugging lebih mudah dan cepat.

### Model View Controller

1. **Model:** Model adalah bagian yang bertanggung jawab mengelola data, logika bisnis, dan aturan yang menentukan bagaimana data itu dapat diakses atau dimanipulasi. Model berinterksi langsung dengan database untuk melakukan operasi CRUD. Selain itu, Model juga memastikan bahwa setiap perubahan pada data mengikuti aturan bisnis yang telah ditentukan. Dalam arsitektur MVC, Model tidak mengetahui bagaimana data akan ditampilkan — tugasnya hanya fokus pada pengelolaan dan validasi data.

2. **View:** View adalah bagian yang bertugas menampilkan data kepada pengguna dalam bentuk antarmuka yang mudah dipahami. View menerima data dari Controller dan merendernya menjadi halaman web, tampilan aplikasi, atau komponen UI lainnya. View hanya fokus pada bagaimana informasi harus ditampilkan, seperti layout, warna, daftar item, tabel, atau elemen visual lainnya. Dengan memisahkan tampilan dari logika, View menjadi lebih fleksibel untuk diubah tanpa memengaruhi proses bisnis di belakangnya.

3.  **Controller:** Berperan sebagai penghubung antara Model dan View, menerima input dari pengguna, memproses data tersebut, dan menentukan response yang tepat untuk dikirimkan kembali kepada View. Ketika pengguna menekan tombol, mengirim form, atau mengakses URL, Controller akan menangani request tersebut, mengolahnya menggunakan logika tertentu, memanggil Model jika perlu memanipulasi data, den mengirimnya kembali ke View.

### Tahap instalasi Laravel

#### 1. Installing Laravel installer

Buka terminal lalu masukkan perintah ini

```bash
composer global require laravel/installer
```

#### 2. Creating an Application

Setelah installer Laravel berhasil di install, Selanjutnya kita menjalankan perintah

```bash
laravel new example-app
```

Perintah tersebut bertujuan untuk membuat folder Laravel bernama `example-app`. Di perintah itu, kita dapat mengkonfigurasi starter kit, testing framework, hingga database.

Setelah proses tersebut selesai, kita bisa memulai Laravel's local development server, queue worker, dan Vite development server menggunakan `dev` Composer script:

```bash
cd example-app // change directory to example-app
npm install && npm run build // install package 
composer run dev // menjalankan development server
```

Ketika kita sudah berhasil menjalankan development server, kita dapat melihat tampilan aplikasi kita di web browser [http://localhost:8000](http://localhost:8000/). 

#### Setting Database Environment

Secara default, Laravel menyimpan konfigurasi aplikasi pada `.env` configuration file specifies that Laravel will be interacting with an SQLite database.

During the creation of the application, Laravel created a `database/database.sqlite` file for you, and ran the necessary migrations to create the application's database tables.

If you prefer to use another database driver such as MySQL or PostgreSQL, you can update your `.env` configuration file to use the appropriate database. For example, if you wish to use MySQL, update your `.env` configuration file's `DB_*` variables like so: