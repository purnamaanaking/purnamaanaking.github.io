---
title: "Cara Install Flutter Secara Manual di macOS (Tanpa Installer)"
meta_title: "Cara Install Flutter Secara Manual di macOS (Tanpa Installer)"
description: "Panduan lengkap instalasi Flutter SDK secara manual di macOS, mulai dari download, ekstrak, hingga setup PATH di Zsh dan verifikasi dengan flutter doctor."
date: 2026-07-20T06:00:00Z
image: "/images/blog/flutter-manual-install-macos.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["flutter", "dart", "macos", "mobile-development", "setup"]
draft: false
---

## Pendahuluan

Selain di Windows, instalasi manual Flutter juga bisa dilakukan di macOS bagi yang ingin kontrol penuh atas lokasi SDK atau tidak ingin bergantung pada installer. Prosesnya cukup sederhana karena macOS sudah menyediakan terminal dan tools dasar yang dibutuhkan.

Artikel ini merangkum langkah-langkah instalasi manual Flutter SDK di macOS, mengacu pada [dokumentasi resmi Flutter](https://docs.flutter.dev/install/manual).

---

## Prasyarat

### 1. Xcode Command-Line Tools

Flutter membutuhkan beberapa tools dari Xcode Command-Line Tools. Install lewat terminal:

```bash
xcode-select --install
```

Akan muncul dialog konfirmasi instalasi. Klik **Install**, tunggu sampai selesai, lalu klik **Done**.

### 2. Editor atau IDE

Siapkan editor dengan dukungan Flutter, misalnya Visual Studio Code atau Android Studio, lengkap dengan plugin Flutter dan Dart. Ini bukan syarat wajib untuk instalasi SDK, tapi akan sangat membantu proses development setelahnya.

---

## Langkah 1: Download Flutter SDK

Download Flutter SDK versi stable terbaru untuk macOS dari halaman [SDK archive](https://docs.flutter.dev/install/archive) di situs resmi Flutter. Perhatikan arsitektur CPU Mac yang digunakan:

- **Apple Silicon (M1/M2/M3/M4, ARM64)** — pilih bundle untuk ARM64.
- **Intel** — pilih bundle untuk Intel x64.

Jika tidak yakin arsitektur Mac yang dipakai, cek lewat menu Apple → **About This Mac**.

---

## Langkah 2: Siapkan Folder untuk SDK

Buat direktori khusus untuk menyimpan Flutter SDK, misalnya:

```
~/develop
```

Hindari meletakkan SDK di folder yang membutuhkan hak akses `sudo` untuk menulis file, karena Flutter perlu menulis ke dalam direktori SDK-nya sendiri saat digunakan.

---

## Langkah 3: Ekstrak SDK

Buka Terminal, lalu ekstrak file `.zip` yang sudah didownload menggunakan `unzip`:

```bash
unzip ~/Downloads/flutter_macos_3.29.3-stable.zip -d ~/develop/
```

Sesuaikan nama file dan versi dengan yang kamu download.

---

## Langkah 4: Tambahkan Flutter ke PATH

Agar perintah `flutter` dan `dart` bisa dijalankan dari terminal mana saja, folder `bin` di dalam SDK perlu ditambahkan ke environment variable `PATH`. Panduan ini menggunakan Zsh, shell default di macOS sejak Catalina.

### 1. Catat lokasi SDK

Salin path lengkap ke folder Flutter SDK yang sudah diekstrak, misalnya `~/develop/flutter`.

### 2. Buka atau buat file `~/.zprofile`

```bash
nano ~/.zprofile
```

Jika file belum ada, `nano` akan otomatis membuatnya.

### 3. Tambahkan Flutter ke PATH

Tambahkan baris berikut di akhir file:

```bash
export PATH="$HOME/develop/flutter/bin:$PATH"
```

Sesuaikan path dengan lokasi SDK yang kamu gunakan.

### 4. Simpan dan terapkan perubahan

Simpan file (di `nano`: tekan `Ctrl+O` lalu `Enter`, kemudian `Ctrl+X` untuk keluar). Setelah itu, tutup dan buka kembali semua sesi terminal agar PATH terbaca.

> **Catatan:** Jika menggunakan shell lain seperti Bash, langkahnya serupa tapi file yang diedit adalah `~/.bash_profile` atau `~/.bashrc`.

---

## Langkah 5: Verifikasi Instalasi

Buka terminal baru, lalu jalankan:

```bash
flutter --version
dart --version
```

Jika instalasi berhasil, kedua perintah akan menampilkan informasi versi Flutter dan Dart. Jika muncul error "command not found", cek kembali langkah setup PATH di atas, atau lihat [panduan troubleshooting instalasi Flutter](https://docs.flutter.dev/install/troubleshoot).

Sebagai langkah tambahan, jalankan juga:

```bash
flutter doctor
```

Perintah ini akan memeriksa kelengkapan environment — termasuk Xcode, CocoaPods, Android toolchain, dan editor yang terpasang — sekaligus memberi tahu apa saja yang masih perlu dilengkapi.

---

## Langkah Selanjutnya

Setelah Flutter SDK terpasang, langkah berikutnya adalah menyiapkan target platform untuk development, misalnya:

- **Web** (direkomendasikan untuk pemula, setup paling sederhana)
- Android
- iOS
- macOS desktop
- Linux

Untuk target iOS dan macOS desktop, pastikan Xcode sudah terpasang lengkap dari App Store, tidak cukup hanya Command-Line Tools. Pilih salah satu target sesuai kebutuhan project, lalu ikuti panduan setup platform yang bersangkutan di [dokumentasi resmi Flutter](https://docs.flutter.dev/platform-integration).

---

## Kesimpulan

Instalasi manual Flutter di macOS pada dasarnya hanya tiga langkah inti: download SDK sesuai arsitektur CPU, ekstrak ke lokasi yang tepat, lalu daftarkan folder `bin`-nya ke PATH lewat `~/.zprofile`. Setelah `flutter doctor` menunjukkan hasil bersih, kamu sudah siap mulai membangun aplikasi dengan Flutter di macOS.
