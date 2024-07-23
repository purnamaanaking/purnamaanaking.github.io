---
title: "Langkah-langkah Deploy Project Laravel ke Vercel"
meta_title: "Langkah-langkah Deploy Project Laravel ke Vercel"
description: "Langkah-langkah Deploy Project Laravel ke Vercel"
date: 2024-06-20T05:00:00Z
image: "/images/blog/laravel-vercel-github.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["framework", "laravel", "deploy", "vercel", "github"]
draft: false
---

Untuk kamu yang mau buat aplikasi berbasis web menggunakan laravel dan ingin di-online-kan, salah satu alternatif yang bisa dilakukan adalah men-deploy-nya ke platform **Vercel** https://vercel.com. Berikut ini adalah langkah-langkah yang pernah saya lakukan, check it out!

### 1. Setup dan Konfigurasi Project Laravel

#### 1.1. Buat project Laravel baru

- Buka terminal, saya buat project baru Laravel via **composer** dengan nama **laravel-on**, dengan command berikut. Engine php yang saya pakai masih php7, jadi project laravel yang terinstal adalah laravel 8. Nama project laravel silakan bisa disesuaikan dengan nama yang lain.

```
composer create-project laravel/laravel laravel-on
```

- Masuk pada project laravel yang baru saja dibuat. Cek versi project laravel yang terinstal, dengan command berikut:

```
cd laravel-on
```

```
php artisan --version
```

#### 1.2. Generate application key untuk project laravel

- Generate application key dengan menjalankan command berikut:

```
php artisan key:generate
```

- Buka file **.env**.

```
nano .env
```

- Jika berhasil pada file **.env** maka akan tergenerate string key pada **APP_KEY**. Contohnya seperti gambar di bawah ini.

  {{< image src="images/blog/laravel-vercel-env.png" caption="" alt="File .env" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title="File .env"  webp="true" >}}

#### 1.3. Buat entry point untuk vercel

- Buat folder **/api**.

```
mkdir api

```

- Buat file **index.php** pada folder **/api**.

```
touch api/index.php

```

- Buka file **/api/index.php**

```
nano api/index.php

```

- Forward entry-point vercel ke **public/index.php** (entry-point default project laravel) dengan menuliskan kode di bawah ini pada file **/api/index.php**:

```php
<?php

require __DIR__ . "/../public/index.php";
```

#### 1.4. Buat file .vercelignore

- Buat dan buka file **.vercelignore**

```
touch .vercelignore
```

```
nano .vercelignore
```

- Isikan dengan kode di bawah ini:

```bash
/vendor
```

#### 1.5. Setup file untuk konfigurasi Vercel

- Buat file **vercel.json**. Isi dengan kode di bawah ini:

```
touch vercel.json
```

- Buka file **vercel.json** yang baru saja dibuat.

```
nano vercel.json
```

- Setup konfigurasi vercel dengan menulis kode di bawah ini pada file **vercel.json**. Pada bagian **APP_KEY** sesuaikan value nya dengan value **APP_KEY** pada file **.env**.

- Hati-hati pada pemilihan versi **vercel-php** pada bagian **functions runtime**. Pemilihan versi **vercel-php** saya merujuk pada stackoverflow berikut: https://stackoverflow.com/questions/78242231/error-while-deploying-laravel-app-on-vercel-project. Saya menggunakan vercel-php versi 0.3.5 karna php yang saya pakai versi 7.4.x.

  {{< image src="images/blog/laravel-vercel-php.png" caption="" alt="vercel-php version" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title="vercel-php version"  webp="true" >}}

```bash
{
    "version": 2,
      "framework": null,
    "functions": {
        "api/index.php": { "runtime": "vercel-php@0.3.5" }
    },
    "routes": [{
        "src": "/(.*)",
        "dest": "/api/index.php"
    }],
    "env": {
        "APP_ENV": "production",
        "APP_DEBUG": "true",
        "APP_URL": "https://yourprojectdomain.com",
        "APP_KEY": "your_application_key",
        "APP_CONFIG_CACHE": "/tmp/config.php",
        "APP_EVENTS_CACHE": "/tmp/events.php",
        "APP_PACKAGES_CACHE": "/tmp/packages.php",
        "APP_ROUTES_CACHE": "/tmp/routes.php",
        "APP_SERVICES_CACHE": "/tmp/services.php",
        "VIEW_COMPILED_PATH": "/tmp",

        "CACHE_DRIVER": "array",
        "LOG_CHANNEL": "stderr",
        "SESSION_DRIVER": "cookie"
    }
}
```

#### 1.6. Setup versi Node.js yang digunakan

- By default vercel menggunakan Node.js versi 20.x. Karna saya menggunakan Node.js versi 18, maka kita bisa override konfigurasi tersebut dengan membuka file **package.json** kemudian tambahkan kode di bawah ini:

```bash
"engines": {
    "node": "18.x"
}
```

{{< image src="images/blog/laravel-vercel-package-json.png" caption="" alt="File package.json" height="" width="500" position="left" command="fit" option="q100" class="img-fluid" title="File package.json"  webp="true" >}}

---

### 2. Setup Vercel dan Deploy

#### 2.1. Setup Vercel via CLI

- install vercel via terminal

```
npm install -g vercel
```

- login Vercel via terminal. Saya memilih akun github untuk login.

```
vercel login
```

{{< image src="images/blog/laravel-vercel-login-1.png" caption="" alt="Vercel Login" height="" width="400" position="left" command="fit" option="q100" class="img-fluid" title="Vercel Login"  webp="true" >}}

{{< image src="images/blog/laravel-vercel-login-2.png" caption="" alt="Vercel Login" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Vercel Login"  webp="true" >}}

#### 2.2. Deploy project Laravel ke Vercel

- Untuk deploy project laravel ke Vercel, jalankan command di bawah ini:

```
vercel --prod
```

{{< image src="images/blog/laravel-vercel-prod.png" caption="" alt="Deploy vercel production" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Deploy vercel production"  webp="true" >}}

- Project laravel hasil deployment bisa diakses melalui URL production yang tampak pada gambar di atas. Atau bisa kita ubah via website vercel. Masuk ke project vercel yang sudah terbuat di dashboard website vercel, masuk pada bagian **Setting > Domains**. Klik tombol **Edit**, maka akan muncul tampilan seperti gambar di bawah ini.

{{< image src="images/blog/laravel-vercel-project.png" caption="" alt="Vercel project" height="" width="500" position="left" command="fit" option="q100" class="img-fluid" title="Vercel project"  webp="true" >}}

{{< image src="images/blog/laravel-vercel-domain.png" caption="" alt="Vercel domain" height="" width="1000" position="left" command="fit" option="q100" class="img-fluid" title="Vercel domain"  webp="true" >}}

- By default nama domain mengikuti sama dengan nama project vercel yang kita definisikan ketika deploy. Maka domain yang saya miliki adalah laravel-on.vercel.app. Jika kita akses url https://laravel-on.vercel.app maka akan muncul welcome page project laravel seperti di bawah ini.

{{< image src="images/blog/laravel-vercel-on.png" caption="" alt="Laravel on Vercel" height="" width="1000" position="left" command="fit" option="q100" class="img-fluid" title="Laravel on Vercel"  webp="true" >}}

---

Kita sudah berhasil deploy project laravel ke vercel sehingga project laravel kita bisa diakses secara online. Selanjutnya kita bisa koneksikan project laravel kita di vercel ke Github repository untuk memudahkan kita melakukan re-deployment setelah proses development atau setelah melakukan perubahan kode program di local computer.

### 3. Konek ke Github

#### 3.1 Buat Repository Baru Github

- Masuk ke akun Github yang dimiliki. https://github.com/
- Klik menu **New Repository** untuk membuat repository baru pada akun Github.

  {{< image src="images/blog/laravel-vercel-new-repo.png" caption="" alt="Create New Repository" height="" width="200" position="left" command="fit" option="q100" class="img-fluid" title="Create New Repository"  webp="true" >}}

- Masukkan nama repository yang akan dibuat. Saya buat dengan nama **laravel-on**. Silakan sesuaikan sendiri ya..

  {{< image src="images/blog/laravel-vercel-create-repo.png" caption="" alt="Set Repository Name" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Set Repository Name"  webp="true" >}}

#### 3.2 Inisiasi Git pada project Laravel

- Buka terminal dan masuk ke root direktori project laravel yang sudah kita setup di atas.
- Kemudian inisiasi, commit, buat branch **main** sebagai branch utama, setup remote ke repository yang barusan dibuat, dan push project laravel ke repository, dengan menjalankan command di bawah ini pada terminal:

```
git init
```

```
git add .
```

```
git commit -m "first commit"
```

```
git branch -M main
```

```
git remote add origin https://github.com/purnamaanaking/laravel-on.git
```

```
git push -u origin main
```

#### 3.1 Konek Github Repository ke Vercel

- Masuk ke akun vercel yang kita miliki. https://vercel.com/
- Saya pilih project **laravel-on** yang telah dibuat sebelumnya dan klik **Connect Git Repository**

  {{< image src="images/blog/laravel-vercel-connect-git.png" caption="" alt="Connect Git Repository" height="" width="400" position="left" command="fit" option="q100" class="img-fluid" title="Connect Git Repository"  webp="true" >}}

- Pilih button **Github**.

  {{< image src="images/blog/laravel-vercel-connect-github.png" caption="" alt="Select Github" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Select Github"  webp="true" >}}

- Saya cari & pilih repository **larave-on**. Klik **Connect**.

  {{< image src="images/blog/laravel-vercel-set-repo.png" caption="" alt="Select Github Repository" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Select Github Repository"  webp="true" >}}

  {{< image src="images/blog/laravel-vercel-connected-git.png" caption="" alt="Connected Git" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Connected Git"  webp="true" >}}

- Dengan itu repository **laravel-on** pada Github dengan tersambung dengan Vercel, sehingga ketika kita melakukan perubahan kode program pada local computer kemudian melakukan **push** atau **merge** ke branch main pada repo Github, maka vercel akan melakukan re-deployment di sisi server secara otomatis. Kita dapat melihat hasil perubahan dari aplikasi web yang kita deploy di vercel secara langsung.

  {{< image src="images/blog/laravel-vercel-deploy.png" caption="" alt="Laravel Vercel Deploy" height="" width="700" position="left" command="fit" option="q100" class="img-fluid" title="Laravel Vercel Deploy"  webp="true" >}}

---

Kita sudah berhasil menyambungkan Github repository yang berisi project laravel dengan Vercel. Selanjutnya, untuk urusan database bagaimana? Untuk database bisa kita sambungkan dengan pilihan cloud database yang ada. Kita lanjutkan tulis pada artikel yang berbeda in syaa Allah..
