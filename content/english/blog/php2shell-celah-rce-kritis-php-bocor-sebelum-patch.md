---
title: "PHP2Shell: Dua Celah RCE Kritis PHP Bocor Sebelum Patch Rilis, Ini yang Perlu Dilakukan"
meta_title: "PHP2Shell: Celah RCE Kritis PHP, Solusi dan Mitigasi"
description: "Rangkuman PHP2Shell (CVE-2026-109618 dan CVE-2026-109619), dua celah remote code execution di inti PHP yang bocor sebelum jadwal disclosure, lengkap dengan versi patch dan langkah mitigasi."
date: 2026-10-22T05:00:00Z
image: "/images/blog/php2shell-featured.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["php", "security", "cve", "rce", "php2shell", "devops", "web-security"]
draft: false
---

Kalau kemarin timeline keamanan siber Anda mendadak ramai, kemungkinan besar penyebabnya **PHP2Shell**. Dua celah *remote code execution* (RCE) di inti PHP bocor sebelum jadwal disclosure resmi, dan patch-nya akhirnya dirilis dipercepat satu hari.

Karena PHP masih menjalankan sebagian besar web di internet, ini bukan berita yang boleh dilewatkan, termasuk oleh yang merasa "saya kan cuma pakai WordPress". Tulisan ini merangkum apa yang terjadi, siapa yang terdampak, dan apa yang perlu dikerjakan hari ini. Semua fakta saya ambil dari advisory resmi PHP dan laporan media serta vendor keamanan. Daftar rujukannya ada di bagian akhir.

> Catatan: situasinya masih bergerak cepat. Jika ada detail yang berbeda dengan advisory resmi PHP, ikuti advisory resmi.

## Apa itu PHP2Shell?

PHP2Shell adalah nama yang dipakai peneliti dan media untuk dua kerentanan kritis di **interpreter inti PHP**. Nama dan nomor CVE-nya berasal dari peneliti dan unggahan yang bocor, bukan dari penamaan resmi PHP.

| CVE | Jenis | Ringkasan |
|---|---|---|
| CVE-2026-109618 | Korupsi akibat *out-of-memory* (OOM) | PHP bisa terhenti saat data internal *request* baru separuh terinisialisasi. Proses pembersihan lalu memperlakukan data yang tidak valid itu seolah valid. |
| CVE-2026-109619 | *Stale global state* | State internal milik *request* bisa bertahan setelah memorinya dibebaskan, lalu terpakai oleh *request* berikutnya di *worker* yang sama. |

Keduanya dinilai **CVSS v4.0 9.2 (Critical)** dan memungkinkan RCE **tanpa autentikasi**. Unggahan yang bocor sempat mengklaim skor 10.0, tetapi PHP menetapkan 9.2.

Kata kuncinya ada di bagian "inti PHP". Celah ini bukan di WordPress, Laravel, atau Magento, melainkan di mesin PHP-nya. Dalam eksploit yang didemonstrasikan, serangan berjalan **sebelum kode aplikasi dieksekusi**. Jadi kode aplikasi yang rapi tidak melindungi Anda, dan memperbarui CMS saja tidak cukup.

Menurut laporan SecurityWeek, Wiz menyebut kedua CVE ini mewakili empat masalah keamanan memori, dan masih mungkin ada varian lain yang belum terlacak.

## Kronologi singkat

- **Agustus 2026:** Peneliti dari Wiz, OpenAI, dan Calif menemukan kerentanan ini dan merahasiakannya sambil mengoordinasikan perbaikan. Calif dan OpenAI melaporkan masalah OOM-ke-RCE secara terpisah pada 11 Agustus.
- **20 Oktober:** Sebuah unggahan di forum Hacker News menyebut dua celah RCE kritis PHP akan diumumkan 22 Oktober. Unggahan itu dihapus, tetapi tangkapan layarnya sudah menyebar. Peneliti watchTowr mengonfirmasi informasinya nyata dan masih dalam *embargo*. Pada hari yang sama tim keamanan PHP mengumumkan rilis keamanan di luar jadwal.
- **21 Oktober:** PHP merilis patch, lebih cepat dari rencana disclosure. Advisory resmi terbit pada hari yang sama.
- **21 Oktober, hampir bersamaan:** Beberapa vendor keamanan melaporkan percobaan eksploitasi, sebagian bahkan sebelum patch tersedia.

Ada juga dua kejadian yang memicu kontroversi, menurut SecurityWeek. Sebuah video yang menampilkan PoC yang berfungsi sempat terpublikasi lebih awal dari jadwal. Lalu Chainguard merilis *image* berisi perbaikan sekitar enam jam sebelum rilis terkoordinasi. PHP Foundation menilai langkah itu memberi penyerang kesempatan mempelajari perbaikan dalam bentuk biner sebelum pengguna lain sempat menambal.

## Siapa yang terdampak?

Hampir semua rilis PHP, **mundur sampai PHP 5.4**, dan semua cara menjalankan PHP yang melayani *request* web:

- PHP-FPM
- Apache dengan `mod_php` (dinilai PHP sebagai yang paling berisiko)
- `php-cgi`
- FrankenPHP
- LiteSpeed

Aplikasi seperti WordPress, Drupal, Joomla, dan Magento ikut terdampak karena berjalan di atas PHP yang rentan. Wiz memperkirakan celah ini menyentuh sekitar 70% situs web, apa pun CMS-nya. Kedua celah juga bisa dipakai untuk membaca memori proses yang sensitif, yang mungkin berisi kredensial dan API key.

Yang sering terlewat:

- PHP yang ikut terpasang di dalam aplikasi vendor atau *appliance*
- Panel hosting seperti cPanel dan Plesk
- Binary Windows `php.exe` dan `php-cgi.exe`
- *Base image* container yang sudah lama tidak di-*rebuild*

## Status eksploitasi

Menurut SecurityWeek, watchTowr mulai melihat percobaan eksploitasi di internet sekitar waktu patch rilis, tetapi belum mengamati eksploitasi yang berhasil. Hadrian melaporkan eksploitasi dalam skala besar beberapa jam sebelum patch rilis, sebagian besar lewat fitur *session upload progress* PHP. Patchstack dan Cloudflare juga melaporkan percobaan sejak awal 21 Oktober. Arctic Wolf menambahkan bahwa mereka melihat aktivitas pemindaian dan percobaan eksploitasi yang mengunduh binary ELF dan Golang lewat `wget`, yang terkait malware komoditas.

Dengan PoC publik yang beredar, anggap saja **masa tenggang sudah habis**.

## Solusi utama: update PHP

PHP menyatakan hanya satu cara yang benar-benar aman, yaitu memperbarui. Rilis perbaikannya:

| Cabang | Versi perbaikan | Catatan |
|---|---|---|
| PHP 8.5 | **8.5.12** | Didukung penuh |
| PHP 8.4 | **8.4.27** | Didukung penuh |
| PHP 8.3 | **8.3.36** | Hanya perbaikan keamanan |
| PHP 8.2 | **8.2.35** | Hanya perbaikan keamanan, segera rencanakan upgrade |
| PHP 8.1, 8.0, 7.4 | Tidak ada rilis resmi | PHP menyediakan *minimal source backport* untuk 7.4, 8.0, dan 8.1; bisa juga lewat vendor/distro |
| PHP 7.3 ke bawah, 5.x | Tidak ada | Sudah EOL, migrasi atau isolasi |

Sebelum patch rilis, beberapa laporan sempat menyebut cabang 8.1 ke bawah tidak mendapat perbaikan sama sekali. Advisory resmi kemudian menyebut adanya *minimal source backport* untuk 7.4, 8.0, dan 8.1. Distro dan vendor LTS juga sudah menerima patch lebih dulu.

### Langkah praktis

**1. Inventarisasi dulu.** Cari semua tempat PHP berjalan, termasuk yang tidak terpikir.

```bash
# Ubuntu / Debian
dpkg -l | grep php

# RHEL / Rocky / AlmaLinux / CentOS
rpm -qa | grep php

# Alpine (container)
apk info | grep php
```

**2. Update dari sumber yang benar.** Pakai paket distro atau vendor Anda, atau rilis dari php.net. Prioritaskan server yang menghadap internet.

Distro sering menambal tanpa mengubah nomor versi, jadi nomor yang tampak lama belum tentu rentan. Pastikan dengan memeriksa *changelog* paket:

```bash
# Ubuntu / Debian
apt changelog php8.3-cli | grep -E "CVE-2026-109618|CVE-2026-109619"

# RHEL / Rocky / AlmaLinux
rpm -q php-cli --changelog | grep -E "CVE-2026-109618|CVE-2026-109619"
```

**3. Restart semuanya.** Menginstal paket saja tidak cukup, karena *worker* lama masih memakai kode lama di memori. Restart PHP-FPM, Apache, dan *worker* aplikasi.

**4. Rebuild container.** Menambal container yang sedang berjalan membuat *image*-nya tetap rentan. Bangun ulang *image* dari base yang sudah dipatch, lalu deploy ulang.

**5. Catat buktinya.** Simpan versi terpasang, referensi advisory, waktu restart, dan hasil verifikasi untuk tiap host atau *image*. Ini berguna untuk audit dan insiden berikutnya.

## Mitigasi sementara jika belum bisa update

Bagian ini penting dibaca dengan benar: ini **bukan** pengganti patch. Catatan rilis PHP sendiri menyatakan tidak ada mitigasi yang lengkap. Langkah di bawah hanya mengurangi sebagian jalur serangan, dan saya ambil dari panduan Arctic Wolf yang merujuk advisory PHP.

**Untuk CVE-2026-109618 (OOM):**

- Setel `memory_limit=-1` dengan cara yang tidak bisa ditimpa aplikasi, misalnya `php_admin_value` di *pool* FPM atau konfigurasi Apache.
- Turunkan `post_max_size`, misalnya 8M jika tidak ada fitur unggah dan 32M jika ada. Jaga `upload_max_filesize` tetap di bawahnya.
- Terapkan batas ukuran *request*, jumlah *field*, dan *header* di web server atau *reverse proxy*.
- Jangan memakai `RLIMIT_AS` atau `RLIMIT_DATA`, karena justru bisa memicu kegagalan alokasi memori yang rentan itu.

**Untuk CVE-2026-109619 (stale state):**

- Setel `session.upload_progress.enabled=0` di `php.ini`, atau di konfigurasi *pool* FPM atau server yang berlaku sebelum *request* diparsing, lalu restart semua *worker*. Ini hanya menutup jalur sesi yang sudah terkonfirmasi, bukan varian lain.
- Di PHP 8.5, setel `fatal_error_backtraces=0` lalu restart semua *worker*.
- Perbaiki *fatal error* dan *execution timeout* yang bisa dipicu dari luar.

**Lapisan tambahan di jaringan:**

- Aktifkan proteksi IPS atau WAF dengan signature terbaru dalam mode *prevent*. Ini mengurangi risiko, tetapi bukan pengganti update PHP.
- Batasi *egress* dari web server hanya ke tujuan yang memang diperlukan, supaya shell yang berhasil masuk sulit mengunduh payload.
- Jika sebuah aplikasi PHP yang menghadap internet benar-benar tidak bisa ditambal, CERT-EU menyarankan mengisolasinya, misalnya di belakang VPN atau kontrol akses, atau menurunkannya sementara.

## Cek apakah sudah telanjur disusupi

Karena percobaan eksploitasi sudah terjadi sebelum patch rilis, jangan berhenti di update. Periksa juga:

- *Web shell* atau file `.php` baru di *webroot*, terutama folder `upload`, `cache`, dan `tmp`
- Proses web server (`php-fpm`, `httpd`, `www-data`) yang memanggil `sh`, `bash`, `curl`, `wget`, `python`, atau `nc`
- *Crash* atau restart mendadak pada *worker* PHP (segfault di log)
- `POST` `multipart/form-data` yang besar atau tidak wajar, terutama ke endpoint yang tidak menerima unggahan
- Koneksi keluar baru dari web server ke host asing sesaat setelah *request* masuk
- *Cron job*, *scheduled task*, atau akun baru yang tidak dikenal

Jika ada tanda mencurigakan, isolasi server, amankan log web server dan PHP, lalu telusuri persistensi dan pergerakan lateral. Karena celah ini juga bisa membocorkan isi memori, **rotasi kredensial dan API key** di server yang terpapar sebelum patch.

## Pelajaran yang bisa dibawa pulang

Beberapa hal yang menurut saya layak dicatat dari kejadian ini:

1. **Inventaris aset adalah fondasi.** Pertanyaan pertama saat insiden seperti ini selalu "di mana saja kita pakai PHP?". Tim yang sudah punya jawabannya akan jauh lebih cepat.
2. **Embargo itu rapuh.** Satu unggahan forum dan satu rilis *image* yang terlalu awal cukup untuk mempersempit jendela patch. Proses patching harus siap kapan saja, tidak hanya menunggu jadwal.
3. **Software EOL adalah utang keamanan.** Cabang 8.1 ke bawah tidak lagi mendapat rilis resmi dan hanya bergantung pada backport. Menunda migrasi punya harga yang akhirnya ditagih.
4. **Defense in depth tetap penting.** Pembatasan egress, deteksi proses mencurigakan, dan WAF tidak menggantikan patch, tetapi memberi waktu dan visibilitas saat patch belum bisa diterapkan.

PHP Foundation juga mencatat ada 15 CVE PHP baru sejak 1 Juli, dan menyebut AI ikut membantu menemukan lebih banyak kerentanan. Jadi ritme patch semacam ini kemungkinan akan makin sering. Sebaiknya proses update PHP di tim Anda dibuat rutin dan otomatis, bukan hanya jadi respons darurat.

Semoga bermanfaat. Silakan bagikan ke teman atau tim yang masih menjalankan PHP di production.

---

## Sumber

- [Security advisory PHP: Critical remote code execution vulnerabilities, update now](https://www.php.net/archive/2026.php#2026-10-21-5) (php.net, 21 Oktober 2026)
- [Advance notice PHP security release](https://www.php.net/archive/2026.php#2026-10-20-1) (php.net, 20 Oktober 2026)
- [GHSA-w8hw-w9fx-j79w (CVE-2026-109618)](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w) dan [GHSA-6wmf-4rv5-p3rp (CVE-2026-109619)](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)
- [PHP2Shell Patches Land as First Exploitation Attempts Hit Sensors](https://www.securityweek.com/php2shell-patches-land-as-first-exploitation-attempts-hit-sensors/) (SecurityWeek)
- [PHP2Shell: Mass Exploitation Looms for Leaked Critical PHP Flaws](https://www.securityweek.com/php2shell-mass-exploitation-looms-for-leaked-critical-php-flaws/) (SecurityWeek)
- [Critical PHP Vulnerabilities Leaked Ahead of Scheduled Security Patch](https://www.infosecurity-magazine.com/news/critical-php-vulnerabilities-leaked/) (Infosecurity Magazine)
- [Patch Now: php2shell, What We Know About the Leaked Critical PHP Vulnerabilities](https://blog.checkpoint.com/security/php2shell-what-we-know-about-the-leaked-critical-php-vulnerabilities-cve-2026-109618-cve-2026-109619/) (Check Point)
- [Finding, Prioritizing, and Patching PHP2Shell Exposure](https://arcticwolf.com/resources/blog/arctic-wolf-labs-finding-prioritizing-and-patching-php2shell-exposure/) (Arctic Wolf Labs)
