---
title: "Cara Install Flutter Secara Manual di Windows (Tanpa Installer)"
meta_title: "Cara Install Flutter Secara Manual di Windows (Tanpa Installer)"
description: "Panduan lengkap instalasi Flutter SDK secara manual di Windows, mulai dari download, ekstrak, hingga setup PATH dan verifikasi dengan flutter doctor."
date: 2026-07-20T05:00:00Z
image: "/images/blog/flutter-manual-install-windows.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["flutter", "dart", "windows", "mobile-development", "setup"]
draft: false
---

## Pendahuluan

Cara paling umum untuk mulai menggunakan Flutter adalah lewat installer resmi atau `flutter_console.bat`. Namun ada kalanya kita ingin, atau perlu, melakukan instalasi secara manual — misalnya di lingkungan kerja yang membatasi installer, ingin kontrol penuh atas lokasi SDK, atau sekadar ingin memahami apa yang sebenarnya terjadi di balik proses setup Flutter.

Artikel ini merangkum langkah-langkah instalasi manual Flutter SDK di Windows, mengacu pada [dokumentasi resmi Flutter](https://docs.flutter.dev/install/manual).

---

## Prasyarat

Sebelum mulai, pastikan dua hal berikut sudah tersedia.

### 1. Git for Windows

Flutter menggunakan Git untuk mengelola SDK-nya. Download dan install dari [git-scm.com/downloads/win](https://git-scm.com/downloads/win). Jika butuh panduan instalasi lebih detail, bisa merujuk ke [dokumentasi resmi Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

### 2. Editor atau IDE

Siapkan editor dengan dukungan Flutter, misalnya Visual Studio Code atau Android Studio, lengkap dengan plugin Flutter dan Dart. Ini bukan syarat wajib untuk instalasi SDK, tapi akan sangat membantu proses development setelahnya.

---

## Langkah 1: Download Flutter SDK

Download Flutter SDK versi stable terbaru untuk Windows dari halaman [SDK archive](https://docs.flutter.dev/install/archive) di situs resmi Flutter. File yang didownload berbentuk arsip `.zip`, misalnya `flutter_windows_3.29.3-stable.zip`.

---

## Langkah 2: Siapkan Folder untuk SDK

Buat direktori khusus untuk menyimpan Flutter SDK. Lokasi yang direkomendasikan adalah:

```
%USERPROFILE%\develop
```

Contohnya `C:\Users\{username}\develop`.

Dua hal yang wajib diperhatikan saat memilih lokasi folder:

- **Jangan gunakan path yang mengandung karakter khusus atau spasi** — ini bisa menyebabkan error saat build.
- **Jangan letakkan di folder yang butuh hak akses administrator** (misalnya `C:\Program Files`), karena Flutter perlu menulis file di dalam direktori SDK-nya sendiri.

---

## Langkah 3: Ekstrak SDK

Buka PowerShell, lalu ekstrak file `.zip` yang sudah didownload menggunakan `Expand-Archive`:

```powershell
Expand-Archive `
  -Path $env:USERPROFILE\Downloads\flutter_windows_3.29.3-stable.zip `
  -Destination $env:USERPROFILE\develop\
```

Sesuaikan nama file dan versi dengan yang kamu download.

> **Catatan:** Jika setelah ekstrak file `flutter.bat` tidak ditemukan di folder `bin`, kemungkinan besar antivirus sudah mengkarantina file tersebut. Tambahkan folder Flutter SDK ke daftar pengecualian (trusted) di antivirus, lalu ekstrak ulang.

---

## Langkah 4: Tambahkan Flutter ke PATH

Agar perintah `flutter` dan `dart` bisa dijalankan dari terminal mana saja, folder `bin` di dalam SDK perlu ditambahkan ke environment variable `Path`.

### 1. Catat lokasi SDK

Salin path lengkap ke folder Flutter SDK yang sudah diekstrak.

### 2. Buka pengaturan Environment Variables

1. Tekan **Windows + Pause** (atau **Windows + Fn + B** jika keyboard tidak punya tombol Pause).
2. Pilih **Advanced System Settings** → tab **Advanced** → **Environment Variables...**

### 3. Tambahkan folder `bin` Flutter

**Jika entri `Path` sudah ada:**

1. Klik dua kali entri **Path** di bagian **User variables**.
2. Klik dua kali baris kosong.
3. Masukkan path ke folder `bin` Flutter, misalnya:
   ```
   %USERPROFILE%\develop\flutter\bin
   ```
4. Pilih entri Flutter tadi, lalu klik **Move Up** sampai berada di posisi paling atas.
5. Klik **OK** tiga kali untuk menyimpan.

**Jika entri `Path` belum ada:**

1. Klik **New...**
2. Isi **Variable name** dengan `Path`.
3. Isi **Variable value** dengan:
   ```
   %USERPROFILE%\develop\flutter\bin
   ```
4. Klik **OK** tiga kali.

### 4. Terapkan perubahan

Tutup semua Command Prompt, PowerShell, terminal, dan IDE yang sedang terbuka, lalu buka kembali agar perubahan PATH terbaca.

---

## Langkah 5: Verifikasi Instalasi

Buka Command Prompt atau PowerShell baru, lalu jalankan:

```bash
flutter --version
dart --version
```

Jika instalasi berhasil, kedua perintah akan menampilkan informasi versi Flutter dan Dart. Jika muncul error "command not found" atau semacamnya, cek kembali langkah setup PATH di atas, atau lihat [panduan troubleshooting instalasi Flutter](https://docs.flutter.dev/install/troubleshoot).

Sebagai langkah tambahan, jalankan juga:

```bash
flutter doctor
```

Perintah ini akan memeriksa kelengkapan environment — termasuk Git, Android toolchain, dan editor yang terpasang — sekaligus memberi tahu apa saja yang masih perlu dilengkapi.

---

## Langkah Selanjutnya

Setelah Flutter SDK terpasang, langkah berikutnya adalah menyiapkan target platform untuk development, misalnya:

- **Web** (direkomendasikan untuk pemula, setup paling sederhana)
- Android
- iOS
- Windows desktop
- Linux

Pilih salah satu sesuai kebutuhan project, lalu ikuti panduan setup platform yang bersangkutan di [dokumentasi resmi Flutter](https://docs.flutter.dev/platform-integration).

---

## Kesimpulan

Instalasi manual memang butuh beberapa langkah lebih dibanding menggunakan installer, tapi cara ini memberi kontrol penuh atas lokasi SDK dan cocok untuk environment yang punya batasan tertentu. Intinya ada tiga: download SDK, ekstrak ke lokasi yang tepat, lalu daftarkan folder `bin`-nya ke PATH. Setelah `flutter doctor` menunjukkan hasil bersih, kamu sudah siap mulai membangun aplikasi dengan Flutter.
