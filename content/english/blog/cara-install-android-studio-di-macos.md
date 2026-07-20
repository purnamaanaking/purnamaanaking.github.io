---
title: "Cara Install Android Studio di macOS"
meta_title: "Cara Install Android Studio di macOS"
description: "Panduan lengkap instalasi Android Studio di macOS, mulai dari system requirements, download, instalasi lewat DMG, hingga setup wizard pertama kali."
date: 2026-07-20T07:00:00Z
image: "/images/blog/android-studio-install-macos.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["android-studio", "android", "macos", "mobile-development", "setup"]
draft: false
---

## Pendahuluan

Android Studio adalah IDE resmi untuk pengembangan aplikasi Android, dan juga menjadi salah satu tools yang dibutuhkan saat setup environment untuk development Flutter target Android. Proses instalasinya di macOS cukup sederhana, tapi ada beberapa hal yang perlu diperhatikan, terutama terkait system requirements dan setup wizard di awal.

Artikel ini merangkum langkah-langkah instalasi Android Studio di macOS, mengacu pada [dokumentasi resmi Android Developers](https://developer.android.com/studio/install).

---

## System Requirements

Sebelum install, pastikan Mac yang digunakan memenuhi spesifikasi berikut.

| Kebutuhan | Minimum | Rekomendasi |
|---|---|---|
| **OS** | macOS 12 | Versi macOS 64-bit terbaru |
| **RAM** | 8 GB (Studio saja) / 16 GB (Studio & Emulator) | 32 GB |
| **CPU** | Apple M1, atau Intel Core generasi ke-6 ke atas (misal MacBook Pro 2016 dengan i7-4770HQ atau lebih tinggi) | Apple Silicon terbaru |
| **Disk Space** | 8 GB free (Studio saja) / 16 GB free (Studio & Emulator) | SSD dengan 32 GB atau lebih |
| **Resolusi Layar** | 1280 x 800 | 1920 x 1080 |
| **GPU** | Integrated | Integrated |

> **Catatan:** Apple sudah mulai mengurangi dukungan untuk Mac berbasis chip Intel, jadi jika menggunakan perangkat baru, disarankan yang berbasis Apple Silicon (M1 ke atas).

---

## Langkah 1: Download Android Studio

Download installer Android Studio untuk macOS dari halaman resmi [developer.android.com/studio](https://developer.android.com/studio). Pastikan memilih versi DMG yang sesuai dengan arsitektur Mac — Apple Silicon atau Intel.

Jika tidak yakin arsitektur Mac yang dipakai, cek lewat menu Apple → **About This Mac**.

---

## Langkah 2: Install Lewat DMG

### 1. Buka file DMG

Cari file `.dmg` yang sudah didownload, lalu klik dua kali untuk mount.

### 2. Pindahkan ke folder Applications

Setelah file DMG terbuka, akan muncul jendela dengan ikon Android Studio dan shortcut ke folder **Applications**. Drag ikon Android Studio ke folder **Applications** tersebut.

### 3. Buka Android Studio

Buka folder **Applications**, lalu klik Android Studio untuk menjalankannya. Karena aplikasi didownload dari luar App Store, macOS mungkin menampilkan konfirmasi keamanan — klik **Open** untuk melanjutkan.

---

## Langkah 3: Setup Wizard Saat Pertama Kali Dibuka

### 1. Import Settings

Saat pertama kali dibuka, Android Studio akan menanyakan apakah ingin mengimpor pengaturan dari instalasi sebelumnya. Jika ini instalasi baru, pilih opsi default (tidak mengimpor), lalu klik **OK**.

### 2. Ikuti Setup Wizard

Android Studio akan menjalankan **Setup Wizard** yang akan mendownload komponen-komponen yang dibutuhkan untuk development, termasuk:

- Android SDK
- Android SDK Platform-Tools
- Android Emulator (opsional, jika ingin menjalankan emulator)

Proses ini butuh koneksi internet dan bisa memakan waktu beberapa menit tergantung kecepatan download. Ikuti saja instruksi di layar sampai selesai.

---

## Langkah 4: Verifikasi Instalasi

Setelah setup wizard selesai, Android Studio akan menampilkan halaman **Welcome Screen**. Ini menandakan instalasi sudah berhasil dan siap digunakan untuk membuat atau membuka project.

Untuk memastikan Android Studio selalu menggunakan versi terbaru, cek update secara berkala lewat menu:

```
Android Studio > Check for Updates
```

Android Studio juga akan otomatis menampilkan notifikasi saat ada tools atau API baru yang tersedia.

---

## Langkah Selanjutnya

Setelah Android Studio terpasang, beberapa hal yang biasanya dilakukan selanjutnya:

- Install Android SDK versi tertentu lewat **SDK Manager** sesuai target minimum aplikasi.
- Setup **Android Virtual Device (AVD)** lewat **Device Manager** jika ingin menjalankan emulator.
- Jika digunakan untuk development Flutter, jalankan `flutter doctor` untuk memastikan Android toolchain sudah terdeteksi dengan benar — lihat juga panduan [instalasi manual Flutter di macOS](/blog/cara-install-flutter-manual-di-macos).

---

## Kesimpulan

Instalasi Android Studio di macOS pada dasarnya hanya tiga langkah: download DMG sesuai arsitektur Mac, drag ke folder Applications, lalu ikuti setup wizard untuk mendownload komponen SDK yang dibutuhkan. Setelah welcome screen muncul, environment sudah siap digunakan untuk mulai development aplikasi Android.
