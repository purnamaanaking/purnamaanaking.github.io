---
title: "Modul 02 Belajar Pemrograman Framework Laravel Tahap Dasar: Pengenalan Laravel dan Konfigurasi Awal"
meta_title: "Modul 02 Belajar Pemrograman Framework Laravel Tahap Dasar: Pengenalan Laravel dan Konfigurasi Awal"
description: "Modul 02 Belajar Pemrograman Framework Laravel Tahap Dasar: Pengenalan Laravel dan Konfigurasi Awal"
date: 2023-08-27T05:00:00Z
image: "/images/blog/konfigurasi-laravel.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["modul", "praktikum", "pemrograman", "framework", "laravel", "bootstrap"]
draft: false
---

Ada banyak cara dan pendekatan untuk melakukan instalasi sebuah project Laravel. Namun pada modul ini kita akan fokus melakukan instalasi dan setup seluruh kebutuhan untuk memulai belajar pemrograman php dengan framework Laravel dengan pendekatan yang paling dasar dan mudah. Mulai dari install XAMPP, Composer, sampai install sebuah project Laravel. Kegiatan ini dilakukan agar kita siap untuk belajar Laravel dengan komputer atau laptop masing-masing.

<!-- <hr>

#### Slide Presentasi

<div class="container-frame">
<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTRGNtbjcqM4UYmhhTaqIvl0QUz4cGOwnxG_682Mymq-D2TNDraEieSKEU3u4kqbg/embed?start=false&loop=false&delayms=3000" class="responsive-iframe" frameborder="0" width="" height="" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

<hr> -->

#### 1. Install dan Jalankan XAMPP

- Download **XAMPP** pada link berikut: https://www.apachefriends.org/download.html
- Jalankan installer, ikuti prosesnya hingga selesai.
- Jalankan **XAMPP Control Panel**:

  {{< image src="images/blog/xampp-control-panel.png" caption="" alt="XAMPP Control Panel" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Jalankan **PHPMyAdmin (MySQL Client)** dengan mengakses URL pada browser: _**http://localhost/phpmyadmin**_.

  {{< image src="images/blog/phpmyadmin.png" caption="" alt="PHPMyadmin" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 2. Install dan Jalankan Visual Studio Code

- Download **Visual Studio Code** pada link berikut: https://code.visualstudio.com/download
- Jalankan Visual Studio Code

  {{< image src="images/blog/vscode.png" caption="" alt="VsCode" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Install **Laravel Extension Pack**:

  {{< image src="images/blog/laravel-ext-pack.png" caption="" alt="Laravel Extension Pack" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 3. Install dan Jalankan Composer

- Download **Composer** pada link berikut: https://getcomposer.org/download/ (bagian Windows Installer)
- Jalankan installer, ikuti prosesnya hingga selesai.
- Buka **Command Prompt** kemudian jalankan perintah _**composer**_ untuk memastikan proses instalasi telah berhasil.

  ```terminal
  composer
  ```

  {{< image src="images/blog/composer-terminal.png" caption="" alt="Composer Terminal" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 4. Install dan Jalankan Project Laravel (Via Composer)

- Jika komputer Anda sudah terinstal **PHP** dan **Composer**, Anda dapat membuat proyek Laravel baru dengan menggunakan Composer secara langsung.
  ```
  composer create-project laravel/laravel example-app
  ```
- Masuk ke folder project Laravel yang baru saja dibuat
  ```
  cd example-app
  ```
- Setelah project aplikasi dibuat, anda dapat memulai **Local Development Server** Laravel menggunakan perintah **_serve_** dari Artisan CLI:
  ```
  php artisan serve
  ```
- Akses web laravel dengan URL **_http://localhost:8000_** via browser. Hasilnya adalah sebagai berikut:

  {{< image src="images/blog/laravel-home.png" caption="" alt="Laravel Home" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 5. Install dan Jalankan Project Laravel (Via Laravel Installer)

- Anda dapat menginstal **Laravel Installer** sebagai _Global Composer Dependency_.
  ```
  composer global require laravel/installer
  ```
- Pastikan untuk menempatkan direktori **_/bin_** Composer di seluruh sistem di **$PATH** anda agar eksekusi command **_laravel_** dapat ditemukan oleh sistem Anda. Direktori ini ada di lokasi yang berbeda berdasarkan sistem operasi Anda.

  {{< image src="images/blog/bin-composer.png" caption="" alt="Bin Composer" height="" width="500" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Buat project laravel baru dengan command **laravel new**.
  ```
  laravel new example-app
  ```
- Masuk ke folder project Laravel yang baru saja dibuat
  ```
  cd example-app
  ```
- Setelah project aplikasi dibuat, anda dapat memulai **Local Development Server** Laravel menggunakan perintah **_serve_** dari Artisan CLI:
  ```
  php artisan serve
  ```

<hr>

{{< notice "note" >}}

### Artikel ini Merupakan Bagian Dari Kumpulan Modul Belajar Pemrograman Framework Laravel (Tahap Dasar)

  <hr>
  {{< button label="Modul 01: Pengenalan Framework CSS (Bootstrap)" link="/blog/cloning-bootstrap-dengan-bootstrap" style="outline" class="btn-sm w-full" >}}
    Modul 02: Pengenalan Laravel dan Konfigurasi Awal
  {{< button label="Modul 03: Routing dan Bundling Asset di Laravel" link="/blog/routing-dan-bundling-asset-di-laravel" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 04: Pengenalan Controller dan View" link="/blog/pengenalan-controller-dan-view-pada-laravel/" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 05: Laravel Database Tahap Dasar" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 06: Laravel Database Tahap Lanjut (Eloquent ORM & Blade Templates)" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 07: Version Control System (GIT dan Github)" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 08: Laravel Authentication" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 09: Eloquent Factories & File Storage" link="" style="outline" class="btn-sm w-full mb-3" >}}
  {{< button label="Modul 10: Laravel Third Party Package" link="" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}
