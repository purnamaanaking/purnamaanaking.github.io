---
title: "Cara Install Android Studio di Windows"
meta_title: "Cara Install Android Studio di Windows"
description: "Panduan lengkap instalasi Android Studio di Windows, mulai dari system requirements, download, instalasi lewat EXE atau ZIP, hingga setup wizard pertama kali."
date: 2026-07-20T00:00:00Z
image: "/images/blog/android-studio-install-windows.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["android-studio", "android", "windows", "mobile-development", "setup"]
draft: false
---

## Pendahuluan

Android Studio adalah IDE resmi untuk pengembangan aplikasi Android, dan juga menjadi salah satu tools yang dibutuhkan saat setup environment untuk development Flutter target Android. Di Windows, proses instalasinya bisa dilakukan lewat installer EXE atau file ZIP, tergantung preferensi masing-masing.

Artikel ini merangkum langkah-langkah instalasi Android Studio di Windows, mengacu pada [dokumentasi resmi Android Developers](https://developer.android.com/studio/install).

---

## System Requirements

Sebelum install, pastikan PC atau laptop Windows yang digunakan memenuhi spesifikasi berikut.

| Kebutuhan | Minimum | Rekomendasi |
|---|---|---|
| **OS** | Windows 10 64-bit | Versi Windows 64-bit terbaru |
| **RAM** | 8 GB (Studio saja) / 16 GB (Studio & Emulator) | 32 GB |
| **CPU** | Mendukung virtualisasi (Intel VT-x atau AMD-V, aktif di BIOS); mikroarsitektur setelah 2017 (misal Intel Core i5 Gen 8 atau AMD Ryzen Zen) | Mikroarsitektur terbaru (Intel Core i5/i7/i9 atau AMD Ryzen 5/6/7/9) |
| **Disk Space** | 8 GB free (Studio saja) / 16 GB free (Studio & Emulator) | SSD dengan 32 GB atau lebih |
| **Resolusi Layar** | 1280 x 800 | 1920 x 1080 |
| **GPU** | Tidak wajib (Studio saja) / VRAM 4GB (Studio & Emulator) | VRAM 8GB (Nvidia GeForce seri 20 ke atas, atau AMD Radeon RX6600 ke atas) |

> **Catatan:** Windows dengan CPU berbasis ARM saat ini belum didukung.

---

## Langkah 1: Download Android Studio

Download installer Android Studio untuk Windows dari halaman resmi [developer.android.com/studio](https://developer.android.com/studio).

---

## Langkah 2: Install Android Studio

Ada dua cara instalasi yang bisa dipilih.

### Opsi A: Installer EXE (Direkomendasikan)

Cara paling praktis:

1. Klik dua kali file `.exe` yang sudah didownload untuk menjalankan installer.
2. Ikuti instruksi di setiap langkah installer — pilih lokasi instalasi, komponen yang ingin dipasang (Android Studio dan Android Virtual Device), lalu klik **Next** sampai proses instalasi selesai.

### Opsi B: File ZIP

Jika lebih suka instalasi manual tanpa installer:

1. Ekstrak file `.zip` yang sudah didownload.
2. Salin folder **android-studio** ke dalam folder **Program Files**.
3. Buka folder **android-studio > bin**.
4. Jalankan `studio64.exe` untuk sistem 64-bit, atau `studio.exe` untuk sistem 32-bit.

---

## Langkah 3: Setup Wizard Saat Pertama Kali Dibuka

Setelah Android Studio terbuka untuk pertama kali, ikuti **Setup Wizard** yang akan memandu instalasi komponen yang dibutuhkan, termasuk:

- Android SDK
- Android SDK Platform-Tools
- Android Emulator (opsional, jika ingin menjalankan emulator)

Pilih paket SDK yang direkomendasikan (recommended), lalu tunggu proses download dan instalasi selesai. Proses ini butuh koneksi internet dan bisa memakan waktu beberapa menit tergantung kecepatan download.

---

## Langkah 4: Verifikasi Instalasi

Setelah setup wizard selesai, Android Studio akan menampilkan halaman **Welcome Screen**. Ini menandakan instalasi sudah berhasil dan siap digunakan untuk membuat atau membuka project.

Untuk memastikan Android Studio selalu menggunakan versi terbaru, cek update secara berkala lewat menu:

```
Help > Check for Update
```

Android Studio juga akan otomatis menampilkan notifikasi saat ada tools atau API baru yang tersedia.

---

## Langkah Selanjutnya

Setelah Android Studio terpasang, beberapa hal yang biasanya dilakukan selanjutnya:

- Install Android SDK versi tertentu lewat **SDK Manager** sesuai target minimum aplikasi.
- Setup **Android Virtual Device (AVD)** lewat **Device Manager** jika ingin menjalankan emulator. Pastikan fitur virtualisasi (Intel VT-x/AMD-V) sudah aktif di BIOS agar emulator berjalan lancar.
- Jika digunakan untuk development Flutter, jalankan `flutter doctor` untuk memastikan Android toolchain sudah terdeteksi dengan benar — lihat juga panduan [instalasi manual Flutter di Windows](/blog/cara-install-flutter-manual-di-windows).

---

## Kesimpulan

Instalasi Android Studio di Windows bisa dilakukan lewat installer EXE untuk kemudahan, atau lewat file ZIP jika ingin kontrol lebih atas lokasi instalasi. Setelah setup wizard mengunduh komponen SDK yang dibutuhkan dan welcome screen muncul, environment sudah siap digunakan untuk mulai development aplikasi Android.
