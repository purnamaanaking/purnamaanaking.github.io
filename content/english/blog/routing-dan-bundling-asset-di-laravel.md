---
title: "Modul 03 Belajar Pemrograman Framework Laravel Tahap Dasar: Routing dan Bundling Asset di Laravel"
meta_title: "Modul 03 Belajar Pemrograman Framework Laravel Tahap Dasar: Routing dan Bundling Asset di Laravel"
description: "Modul 03 Belajar Pemrograman Framework Laravel Tahap Dasar: Routing dan Bundling Asset di Laravel"
date: 2023-08-28T05:00:00Z
image: "/images/blog/laravel-routing.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["modul", "praktikum", "pemrograman", "framework", "laravel", "bootstrap", "routing", "vite"]
draft: false
---

Pada modul ini kita akan mulai memasuki materi awal pada pembelajaran framework Laravel. Kita akan mulai belajar tentang Routing di Laravel dengan berbagai macam jenisnya. Kemudian kita akan belajar tentang bundling asset pada Laravel menggunakan Vite. Kegiatan ini dilakukan agar kita dapat menerapkan konsep routing dan manajemen asset pada framework Laravel.

{{< image src="images/blog/belajar-laravel-routing.png" caption="" alt="Belajar Laravel Routing" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 1. Generate Laravel Project dan Laravel UI

- Buat project laravel baru via composer
  ```
  composer create-project laravel/laravel your-project-name
  ```
- Masuk ke dalam folder project laravel yang baru saja dibuat, kemudian install package Laravel UI.
  ```
  composer require laravel/ui
  ```
- Generate scaffolding untuk project Laravel berbasis CSS framework Bootstrap.
  ```
  php artisan ui bootstrap
  ```
- Jalankan script di bawah ini untuk compile scafolding Bootstrap yang barusan di-setup.
  ```
  npm install
  ```
- Jalankan script di bawah ini untuk mengaktifkan local development server Laravel sehingga aplikasi web dapat diakses melalui browser.
  ```
  php artisan serve
  ```

#### 2. Bunding Asset dengan Vite

- Download dan install NodeJS terbaru via https://nodejs.org/en/download/
- Periksa hasil instalasi Anda. Ketik script ini di prompt perintah Anda:
  ```
  node -v
  ```
  ```
  npm -v
  ```
- Install semua dependencies yang dibutuhkan untuk Bundling Asset dengan Vite, dengan mengetikkan perintah:
  ```
  npm install
  ```
- Terapkan fitur **“Refreshing on Save”** dengan memeriksa file konfigurasi file vite pada **vite.config.js** dan sesuaikan seperti gambar di bawah ini.
  ```javascript
  import { defineConfig } from 'vite';
  import laravel from 'laravel-vite-plugin';

  export default defineConfig({
      plugins: [
          laravel({
              input: [
                  'resources/sass/app.scss',
                  'resources/js/app.js',
              ],
              refresh: true,
          }),
      ],
  });
  ```
- Terapkan fitur **“Processing Static Assets With Vite”** dengan buka file **/resources/js/app.js** lalu sesuaikan kode program seperti di bawah ini. Vite akan merujuk pada path direktori yang kita definiskan untuk mengambil aset gambar/image yang kita butuhkan nantinya.
  ```javascript
  import "./bootstrap";
  import.meta.glob(["../images/**"]);
  ```
- Buat folder pada **/resources** dengan nama **images**. Letakkan aset gambar/image yang akan kita gunakan pada website kita pada folder tersebut.
- Buka file View bernama **welcome.blade.php**. Hapus seluruh kode program yang ada. Kemudian, letakkan dan arahkan file **css** dan **javascript** yang telah didefinisikan di atas, pada file view (blade) menggunakan Blade Directive **@vite()**. Panggil asset gambar atau image dengan pendekatan Vite seperti di bawah ini.
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Welcome</title>
      @vite('resources/sass/app.scss')
  </head>
  <body>
      <div class="container m-5">
          {{-- Contoh cara mereferensikan gambar di dalam file blade dengan menggunakan pendekatan Vite --}}
          <img class="img-thumbnail" src="{{ Vite::asset('resources/images/main.svg') }}" alt="image">
      </div>
      @vite('resources/js/app.js')
  </body>
  </html>
  ```
- Jalankan Vite.
  - Untuk mode **development** (jalankan ini saja jika sedang development):
    ```
    npm run dev
    ```
  - Atau untuk mode **production** (ketika akan deploy ke server):
    ```
    npm run build
    ```
- Pastikan Vite Connected melalui Tab Console seperti di bawah ini.
  {{< image src="images/blog/vite-connected.png" caption="" alt="Belajar Laravel Routing" height="" width="400" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Jika sudah berhasil maka ketika kita mengubah kode program pada file css, blade, ataupun javascript, maka browser akan melakukan **reload secara otomatis**.

#### 3. Install Bootstrap dan Bootstrap Icons Terbaru pada Project Laravel

- Jalankan perintah berikut pada project laravel anda untuk mengintal bootstrap, popper dan bootstrap icons terbaru.
  ```
  npm install bootstrap@5.3.0-alpha3 bootstrap-icons @popperjs/core
  ```
- Di dalam project laravel anda, buka file **resources\sass\app.scss** dan tambahkan:
  ```
  @import "bootstrap-icons/font/bootstrap-icons.css";
  ```
- Hapus atau Comment kode import Font & import Variables pada file **resources\sass\app.scss**. Kode program akan terlihat seperti di bawah ini:
  ```css
  // Bootstrap
  @import "bootstrap/scss/bootstrap";
  @import "bootstrap-icons/font/bootstrap-icons.css";
  ```

#### 4. Praktik Laravel Routing

- Buka file **routes/web.php**, buat sebuah Route dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/routing', function() {
      return view('routing');
  });
  ```
- Buat file View baru pada direktori **/resources/views/** dengan nama **routing.blade.php**.
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Belajar Laravel Routing</title>
      @vite('resources/sass/app.scss')
  </head>
  <body>
      <div class="container m-5">
          <h1>Belajar Laravel Routing</h1>

          <div class="list-group list-group-numbered mt-4">
              {{-- Kode anda selanjutnya letakkan di sini --}}
          </div>

          {{-- Khusus kode program untuk Route Groups di sini --}}
      </div>
      @vite('resources/js/app.js')
  </body>
  </html>
  ```
- Akses halaman ini dengan mengetikkan **localhost:8000/routing** pada browser.

##### 4.1. Praktik Basic Routing

- Buka file **routes/web.php**, praktikkan **Basic Routing (No View, No Controller)** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/basic_routing', function() {
      return 'Hello World';
  });
  ```
- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/basic_routing') }}" class="list-group-item list-group-item-action">
      Basic Routing (No View, No Controller)
  </a>
  ```

##### 4.2. Praktik View Route

- Buka file **routes/web.php**, praktikkan View Route dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::view('/view_route', 'view_route');
  Route::view('/view_route', 'view_route', ['name' => 'Purnama']);
  ```
- Buat file View dengan nama **view_route.blade.php**, kemudian isikan dengan kode program seperti di bawah ini.
  ```php
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>View Route</title>
      @vite('resources/sass/app.scss')
  </head>
  <body>
      <div class="container m-5">
          <h1>This is from View Route</h1>
          <p>Hello, My name is {{ $name }}</p>
      </div>
      @vite('resources/js/app.js')
  </body>
  </html>
  ```
- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/view_route') }}" class="list-group-item list-group-item-action">
    View Route
  </a>
  ```

##### 4.3. Praktik Controller Route

- Generate file controller dengan nama RouteController menggunakan bantuan script artisan sebagai berikut:
  ```
  php artisan make:controller RouteController
  ```
- Tambahkan function bernama **“index”** pada class **RouteController** tersebut.
  ```php
  public function index() {
      return "This is from Controller";
  }
  ```
- Buka file **routes/web.php**, praktikkan **Controller Route** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/controller_route', [RouteController::class, 'index']);
  ```
- Pada file **routes/web.php**, lihat bagian atas file tersebut, pastikan **RouteController** ter-import seperti kode program di bawah ini.
  ```php
  <?php

  use App\Http\Controllers\RouteController;
  use Illuminate\Support\Facades\Route;
  ```
- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/controller_route') }}" class="list-group-item list-group-item-action">
    Controller Route
  </a>
  ```

##### 4.4. Praktik Redirect Route

- Buka file **routes/web.php**, praktikkan Redirect Route dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::redirect('/', '/routing');
  ```
- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/') }}" class="list-group-item list-group-item-action">
      Redirect Route
  </a>
  ```

##### 4.5. Praktik Route Parameter (Required Parameter)

- Buka file **routes/web.php**, praktikkan **Route Parameter (Required Parameter)** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/user/{id}', function($id) {
      return "User Id: ".$id;
  });
  Route::get('/posts/{post}/comments/{comment}', function($postId, $commentId) {
      return "Post Id: ".$postId.", Comment Id: ".$commentId;
  });
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/user/12345') }}" class="list-group-item list-group-item-action">
      Route Parameter (Required Parameter) - 1
  </a>
  <a href="{{ url('/posts/01/comments/20') }}" class="list-group-item list-group-item-action">
      Route Parameter (Required Parameter) - 2
  </a>
  ```

##### 4.6. Praktik Route Parameter (Optional Parameter)

- Buka file **routes/web.php**, praktikkan **Route Parameter (Optional Parameter)** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('username/{name?}', function($name = null) {
      return 'Username: '.$name;
  });
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/username') }}" class="list-group-item list-group-item-action">
      Route Parameter (Optional Parameter)
  </a>
  ```

##### 4.7. Praktik Route With Regular Expression Constraints

- Buka file **routes/web.php**, praktikkan **Route With Regular Expression Constraints** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/title/{title}', function($title) {
      return "Title: ".$title;
  })->where('title', '[A-Za-z]+');
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/title/this-is-my-title') }}" class="list-group-item list-group-item-action">
      Route With Regular Expression Constraints
  </a>
  ```
- Jika anda test di browser akan muncul halaman warning **404 Not Found**. Analisa dan jelaskan kenapa terjadi demikian!

##### 4.8. Praktik Named Route

- Buka file **routes/web.php**, praktikkan **Named Route** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/profile/{profileId}', [RouteController::class, 'profile'])->name('profileRouteName');
  ```
- Tambahkan function bernama **“profile”** pada class **RouteController**.
  ```php
  public function profile($profileId) {
      return "This is Profile from Controller, profile id: ".$profileId;
  }
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ route('profileRouteName', ['profileId' => '123']) }}" class="list-group-item list-group-item-action">
      Named Route
  </a>
  ```

##### 4.9. Praktik Route Priority

- Buka file **routes/web.php**, praktikkan **Route Priority** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::get('/route_priority/{rpId}', function($rpId) {
      return "This is Route One";
  });
  Route::get('/route_priority/user', function() {
      return "This is Route 1";
  });
  Route::get('/route_priority/user', function() {
      return "This is Route 2";
  });
  ```
- Comment Route yang pertama di atas, seperti kode program di bawah ini
  ```php
  // Route::get('/route_priority/{rpId}', function($rpId) {
  //     return "This is Route One";
  // });
  Route::get('/route_priority/user', function() {
      return "This is Route 1";
  });
  Route::get('/route_priority/user', function() {
      return "This is Route 2";
  });
  ```
- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/route_priority/user') }}" class="list-group-item list-group-item-action">
      Route Priority
  </a>
  ```
- Test pada browser dengan mengetikkan **localhost:8000/route_priority/user**. Analisa prioritas route yang terjadi sebelum dan sesudah comment Route yang pertama di atas!

##### 4.10. Praktik Fallback Routes

- Buka file **routes/web.php**, praktikkan **Fallback Routes** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::fallback(function() {
      return 'This is Fallback Route';
  });
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```php
  <a href="{{ url('/asdqwezxc') }}" class="list-group-item list-group-item-action">
      Fallback Routes
  </a>
  ```

##### 4.11. Praktik Route Groups (Route Prefixes & Route Name Prefixes)

- Buka file **routes/web.php**, praktikkan **Route Groups (Route Prefixes & Route Name Prefixes)** dengan menuliskan kode program seperti di bawah ini.
  ```php
  Route::prefix('admin')->name('admin.')->group(function() {
      Route::get('/dashboard', function() {
          return "This is admin dashboard";
      })->name('dashboard');
      Route::get('/users', function() {
          return "This is user data on admin page";
      })->name('users');
      Route::get('/items', function() {
          return "This is item data on admin page";
      })->name('items');
  });
  ```

- Buka file view **routing.blade.php** lalu tambahkan kode program sebagai berikut:
  ```html
  <h6 class="mt-4">Route Groups (Route Prefixes & Route Name Prefixes)</h6>
  <div class="list-group list-group-numbered mt-4">
      <a href="{{ route('admin.dashboard') }}" class="list-group-item list-group-item-action">
          Admin Dashboard
      </a>
      <a href="{{ route('admin.users') }}" class="list-group-item list-group-item-action">
          Admin Users
      </a>
      <a href="{{ route('admin.items') }}" class="list-group-item list-group-item-action">
          Admin Items
      </a>
  </div>
  ```

##### 4.12. Praktik View Route List

- Ketikkan pada cmd / terminal script artisan berikut ini: 
  ```
  php artisan route:list
  ```

##### 4.13. Praktik Route Caching

- Ketikkan pada cmd / terminal script artisan berikut ini untuk menerapkan **Route Caching**: 
  ```
  php artisan route:cache
  ```
- Ketikkan pada cmd / terminal script artisan berikut ini untuk menghapus **Route Cache**: 
  ```
  php artisan route:clear
  ```

<hr>

{{< notice "note" >}}
  ### Artikel ini Merupakan Bagian Dari Kumpulan Modul Belajar Pemrograman Framework Laravel (Tahap Dasar)
  <hr>
  {{< button label="Modul 01: Pengenalan Framework CSS (Bootstrap)" link="/blog/cloning-bootstrap-dengan-bootstrap" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 02: Pengenalan Laravel dan Konfigurasi Awal" link="/blog/pengenalan-laravel-dan-konfigurasi-awal" style="outline" class="btn-sm w-full" >}}
    Modul 03: Routing dan Bundling Asset di Laravel
  {{< button label="Modul 04: Pengenalan Controller dan View" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 05: Laravel Database Tahap Dasar" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 06: Laravel Database Tahap Lanjut (Eloquent ORM & Blade Templates)" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 07: Version Control System (GIT dan Github)" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 08: Laravel Authentication" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 09: Eloquent Factories & File Storage" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 10: Laravel Third Party Package" link="" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}
