---
title: "Modul #08 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Authentication"
meta_title: "Modul #08 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Authentication"
description: "Modul #08 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Authentication"
date: 2023-12-19T05:00:00Z
image: "/images/blog/laravel-authentication.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags:
  [
    "modul",
    "praktikum",
    "pemrograman",
    "framework",
    "laravel",
    "authentication",
  ]
draft: false
---

Pada modul kali ini kita akan melanjutkan belajar tentang Laravel Authentication. Kita akan mencoba menerapkan fitur login dan lain-lain pada sebuah project laravel. Kegiatan ini dilakukan agar kita dapat menerapkan fitur authentication untuk mendukung sebuah pengembangan aplikasi web berbasis laravel.

#### 1. Authentication via Laravel UI

- Buat project Laravel baru dengan command di bawah ini. Perhatikan kondisi project laravel tersebut pada bagian controllers, views, dan routes nya.

  ```
  composer create-project laravel/laravel laravel-project-name
  ```

- Kemudian masuk ke project laravel yang baru dibuat.

  ```
  cd laravel-project-name
  ```

- Install package **Laravel UI** via composer.

  ```
  composer require laravel/ui
  ```

- Generate Scafolding Bootstrap dengan fitur Authentication pada project laravel kita. Perhatikan kondisi project laravel tersebut pada bagian controllers, views, dan routes nya.

  ```
  php artisan ui bootstrap --auth
  ```

- Analisis kondisi project laravel antara sebelum dan sesudah menjalankan tahapan di atas!
- Jalankan project laravel tersebut via browser, akan muncul menu & fitur **login** dan **register** pada halaman welcome.

  {{< image src="images/blog/laravel-login-register.png" caption="" alt="Laravel Login Register" height="" width="200" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 2. Setup Database

- Jalankan service **MySQL** dan **Apache** via control panel **XAMPP**.
- Buat database dengan name **“laravel_auth”** via **PhpMyAdmin**.
- Buka file **.env** dan atur dan sesuaikan value variabel **DB_DATABASE**, **DB_USERNAME**, **DB_PASSWORD**.
- Lakukan laravel migration.

  ```
  php artisan migrate
  ```

#### 3. Menjalankan Fitur Register dan Login

- Jalankan project laravel via browser dan coba fitur **Register** untuk membuat akun user baru pada sistem.

  {{< image src="images/blog/laravel-register.png" caption="" alt="Laravel Register" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Coba fitur **Login** menggunakan akun user yang baru saja dibuat.

  {{< image src="images/blog/laravel-login.png" caption="" alt="Laravel Login" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Analisis alur program ketika menjalankan fitur login dan register. Bagaimana hubungan antara file-file **views**, **controllers** dan juga **routes** yang ada!

#### 4. Proses Authentication Secara Manual

- Buka file **LoginController**, tambahkan function dengan nama **authenticate()** menjadi seperti di bawah ini.

  ```php
  <?php

  namespace App\Http\Controllers\Auth;

  use App\Http\Controllers\Controller;
  use App\Providers\RouteServiceProvider;
  use Illuminate\Foundation\Auth\AuthenticatesUsers;
  use Illuminate\Http\Request;
  use Illuminate\Support\Facades\Auth;

  class LoginController extends Controller
  {
      /*
      |--------------------------------------------------------------------------
      | Login Controller
      |--------------------------------------------------------------------------
      |
      | This controller handles authenticating users for the application and
      | redirecting them to your home screen. The controller uses a trait
      | to conveniently provide its functionality to your applications.
      |
      */

      use AuthenticatesUsers;

      /**
       * Where to redirect users after login.
       *
       * @var string
       */
      protected $redirectTo = RouteServiceProvider::HOME;

      /**
       * Create a new controller instance.
       *
       * @return void
       */
      public function __construct()
      {
          $this->middleware('guest')->except('logout');
      }

      public function authenticate(Request $request)
      {
          $credentials = $request->validate([
              'email' => ['required', 'email'],
              'password' => ['required'],
          ]);

          if (Auth::attempt($credentials)) {
              $request->session()->regenerate();

              return redirect()->intended('home');
          }

          return back()->withErrors([
              'email' => 'The provided credentials do not match our records.',
          ])->onlyInput('email');
      }
  }
  ```

- Buka file **routes/web.php** kemudian buat route baru dan arahkan ke function **authenticate()** pada **LoginController** seperti di bawah ini.

  ```php
  Route::post('/login', [LoginController::class, 'authenticate']);
  ```

- Jalankan kembali fitur **Login** via Browser.

#### 5. Latihan

- **Lanjutkan (bukan buat dari awal lagi)** Project Laravel yang kita kerjakan pada **Modul 06 (aplikasi sederhana data master employee)** dengan menerapkan Laravel Authentication yang sudah dipelajari pada project laravel tersebut. Manfaatkan command **Laravel UI** yang ada pada php artisan di bawah ini.

  {{< image src="images/blog/laravel-ui-cli.png" caption="" alt="Laravel UI" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Hasil akhir yang diinginkan adalah sebuah aplikasi sederhana data master employee yang telah memiliki:

  - Halaman **Login** dan fitur **Authentication** (tidak ada halaman & fitur Register)
  - Halaman Login langsung muncul atau tampil ketika user mengakses http://localhost:8000 via browser.
  - Menu **Logout** pada bagian **Navbar**
  - Ketika berhasil login, user diarahkan ke halaman **home**.
  - Semua halaman aplikasi hanya bisa diakses **setelah login**.
  - Buat satu data **user awal**, via **Seeder** dengan:

    - **name**: Administrator
    - **email/username**: admin@admin
    - **password**: adminadmin
    - Password yang disimpan di database adalah password yang terenkripsi
    - Gunakan method atau function **bcrypt()** untuk mengenkripsi text string password anda ketika akan melakukan insert data user ke database.

      ```php
      bcrypt('adminadmin')
      ```

  - Sesuaikan tampilan UI dengan tampilan aplikasi data master employee yang dimiliki sebelumnya.

    {{< image src="images/blog/employee-master-login.png" caption="" alt="Employee Master Login" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

    {{< image src="images/blog/employee-master.png" caption="" alt="Employee Master" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Simulasikan fitur **Login** dengan **user awal** tersebut dan jelaskan proses apa saja yang dilakukan.

#### 6. Tugas

- **Praktekkan** seluruh **poin** dan **latihan (bab 1 - 5)** yang ada di atas secara **INDIVIDU**.

<hr>

{{< notice "note" >}}

##### Artikel ini merupakan bagian dari:

{{< button label="Kumpulan Modul Belajar Dasar Pemrograman Framework Laravel" link="/blog/kumpulan-modul-pemrograman-framework-tahap-dasar/" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}
