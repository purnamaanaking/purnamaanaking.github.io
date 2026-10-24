---
title: "Celah Kritis Token Guard Laravel: Login Sebagai User Lain Tanpa API Token, Ini Solusi dan Mitigasinya"
meta_title: "Laravel TokenGuard Auth Bypass (CVE-2026-114509): Solusi dan Mitigasi"
description: "Rangkuman celah authentication bypass di TokenGuard Laravel (GHSA-f9x3-7vq9-cph6 / CVE-2026-114509): siapa yang terdampak, cara kerjanya, versi patch, dan langkah mitigasi."
date: 2026-10-24T00:00:00Z
image: "/images/blog/laravel-token-guard-featured.jpg"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["laravel", "php", "security", "cve", "authentication", "web-security", "devops"]
draft: false
---

Laravel baru saja mempublikasikan advisory untuk celah **authentication bypass** berskor kritis di `TokenGuard`, guard bawaan untuk autentikasi API berbasis token. Dampaknya serius: penyerang yang belum login bisa masuk sebagai *user* lain **tanpa mengetahui API token-nya**.

Tulisan ini merangkum apa yang terjadi, siapa yang terdampak, cara kerja celahnya, dan apa yang perlu dikerjakan. Fakta saya ambil dari advisory resmi Laravel di GitHub dan laporan media keamanan. Daftar rujukannya ada di bagian akhir.

> Catatan: beberapa detail masih berbeda antar sumber (saya tandai di bawah). Jika ada perbedaan, ikuti advisory resmi Laravel.

## Ringkasan cepat

| Item | Detail |
|---|---|
| Advisory | GHSA-f9x3-7vq9-cph6 |
| CVE | CVE-2026-114509 (tercantum di halaman advisory GitHub; sebagian media belum menyebutnya) |
| Severity | **Critical, CVSS v4 9.2** |
| Komponen | `Illuminate\Auth\TokenGuard` (guard dengan driver `token`) di `laravel/framework` |
| Terdampak | Laravel 12.x di bawah **12.68.0** dan 13.x di bawah **13.29.0** |
| Versi perbaikan | **12.68.0** dan **13.29.0** (atau lebih baru) |
| Syarat | Memakai guard `token` **dan** database MySQL/MariaDB |
| Autentikasi | Tidak diperlukan (*unauthenticated*) |
| Workaround resmi | Tidak ada. Satu-satunya solusi yang dicantumkan: update |
| Peneliti | Hristijan Manasijev |

## Apa yang terjadi?

Menurut advisory, `TokenGuard` bawaan Laravel bisa dilewati. Pada MySQL atau MariaDB, penyerang dapat mengirim kredensial berupa **nilai non-string** yang diproses keliru saat pencarian *user*. Query akhirnya cocok dengan *record user* lain, bukan gagal seperti seharusnya. Hasilnya, penyerang diperlakukan sebagai user tersebut di semua *route* yang dilindungi guard itu.

Penyebabnya sederhana. Sebelum perbaikan, guard hanya memeriksa apakah token **tidak kosong**, tanpa memeriksa **tipe**-nya. Nilai apa pun yang lolos pemeriksaan itu diteruskan langsung ke *user provider*.

### Kenapa array bisa cocok dengan user lain?

Advisory tidak menjelaskan mekanismenya secara rinci, jadi bagian ini adalah **analisis saya** berdasarkan commit perbaikan dan keterangan SecurityOnline.

Test yang ditambahkan pada commit perbaikan mengirim `api_token` berupa array `[0]`. Di Laravel, `EloquentUserProvider` mengubah nilai berupa array menjadi `WHERE ... IN (...)`, sehingga query kira-kira menjadi:

```sql
SELECT * FROM users WHERE api_token IN (0) LIMIT 1;
```

Kolom `api_token` bertipe teks, sedangkan pembandingnya angka. MySQL dan MariaDB akan mengonversi teks ke angka untuk membandingkannya. Token acak yang diawali huruf umumnya terkonversi menjadi `0`, sehingga baris itu ikut cocok. SecurityOnline menyebut hal serupa: stored token dibandingkan sebagai angka, dan sebagian besar token terkonversi ke nilai yang sama.

Kalau analisis ini benar, wajar kalau celahnya hanya muncul di MySQL/MariaDB. Database lain tidak melakukan konversi implisit seperti itu.

## Siapa yang terdampak?

Anda terdampak jika **semua** syarat ini terpenuhi:

1. Aplikasi memakai Laravel 12.x di bawah 12.68.0 atau 13.x di bawah 13.29.0.
2. Ada guard di `config/auth.php` yang memakai `'driver' => 'token'`.
3. Database-nya MySQL atau MariaDB.
4. Ada *route* yang dilindungi guard tersebut, misalnya `auth:api`.

Menurut Laravel News dan Panda Core, **Sanctum dan Passport tidak terdampak** karena keduanya mendaftarkan driver guard sendiri dan tidak tercantum di advisory. Jadi tidak semua aplikasi Laravel yang punya API otomatis rentan.

Cek cepat guard yang dipakai:

```bash
grep -n "'driver' => 'token'" config/auth.php
grep -rn "auth:api\|auth:" routes/ app/ | head -50
```

### Bagaimana dengan Laravel 11 dan versi lama?

Di sini sumber berbeda pendapat:

- Advisory resmi dan Laravel News hanya mencantumkan cabang **12.x dan 13.x**.
- SecurityOnline menulis bahwa pembuat PoC menyebut Laravel 11.x di bawah 11.56.1 juga terdampak. Saya belum bisa memverifikasi ini dari advisory resmi.

Jadi jika Anda masih di Laravel 11 atau yang lebih lama, jangan menyimpulkan "aman karena tidak tercantum". Pastikan lewat advisory resmi, rencanakan upgrade, dan pasang mitigasi sementara di bawah.

## Status eksploitasi

- Advisory dipublikasikan **19 Oktober 2026**.
- Hakai Security merilis PoC publik di GitHub beberapa hari kemudian, lengkap dengan Docker lab untuk build rentan dan build yang sudah dipatch (menurut SecurityOnline).
- Sampai sumber-sumber yang saya baca terbit, **belum ada laporan eksploitasi aktif** dan CVE ini belum masuk katalog KEV milik CISA. EPSS 30 hari tercatat 0,4%.

Dengan PoC publik yang sudah ada, anggap saja risikonya akan naik. Tidak perlu menunggu laporan eksploitasi untuk mulai menambal.

Satu hal menarik soal waktu: Laravel News dan halaman rilis GitHub menunjukkan 12.68.0 dirilis pada **25 Agustus**, sedangkan advisory baru terbit **19 Oktober**. Perbaikannya sudah ada sekitar dua bulan sebelum disclosure resmi. Jadi kalau Anda rutin memperbarui dependency, kemungkinan besar Anda sudah aman tanpa sadar. Catatan rilis 12.68.0 sendiri tidak menyebut perbaikan token guard, sehingga yang tidak memantau advisory bisa melewatkannya.

## Solusi utama: update Laravel

Perbaikannya ada di commit `9b21ce0` ("fix token retrieval"). Intinya, guard sekarang menolak token yang bukan string sebelum menyentuh database:

```php
// TokenGuard::user()
if (is_string($token) && ! empty($token)) {
    // ...
}

// TokenGuard::validate()
if (! is_string($credentials[$this->inputKey] ?? null) || empty($credentials[$this->inputKey])) {
    return false;
}
```

Langkah praktisnya:

**1. Cek versi terpasang.**

```bash
composer show laravel/framework | grep versions
php artisan --version
```

**2. Update ke versi yang sudah dipatch.**

```bash
# Laravel 12
composer require laravel/framework:"^12.68" -W

# Laravel 13
composer require laravel/framework:"^13.29" -W
```

Atau cukup `composer update laravel/framework --with-dependencies` jika *constraint* di `composer.json` sudah mengizinkan.

**3. Jalankan test**, terutama alur login dan *route* yang dilindungi.

**4. Deploy dengan `composer.lock` yang sudah diperbarui.** Di production, pakai `composer install --no-dev --optimize-autoloader`, bukan `composer update`.

**5. Muat ulang kode di memori.** Jalankan `php artisan queue:restart` agar *worker* memakai kode baru, dan restart PHP-FPM jika OPcache tidak melakukan revalidasi. Untuk Octane, restart *worker*-nya.

**6. Rebuild container.** Menambal container yang sedang berjalan membuat *image*-nya tetap rentan. Bangun ulang *image* dan deploy ulang.

**7. Verifikasi dengan `composer audit`.**

```bash
composer audit
```

Perintah ini membandingkan dependency terpasang dengan advisory yang dipublikasikan.

## Mitigasi sementara jika belum bisa update

Penting: advisory **tidak menyediakan workaround resmi**. Yang di bawah ini adalah langkah *defense in depth* usulan saya. Anggap sebagai penahan sementara, bukan pengganti update.

### 1. Tolak token non-string sebelum guard bekerja

Pasang *middleware* global yang menolak request jika `api_token` bukan string. Letakkan sebelum middleware `auth`.

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class RejectNonStringApiToken
{
    public function handle(Request $request, Closure $next)
    {
        // Sesuaikan dengan 'input_key' yang dipakai guard Anda (default: api_token)
        $token = $request->input('api_token');

        if ($token !== null && ! is_string($token)) {
            abort(401, 'Unauthenticated.');
        }

        return $next($request);
    }
}
```

Jika Anda mengubah `input_key` pada konfigurasi guard, ganti nama field-nya. Satu sumber menyebut serangan berjalan lewat *JSON body*, tetapi saya sarankan tidak berasumsi dan memeriksa semua sumber input.

### 2. Filter di WAF atau reverse proxy

Jika Anda punya WAF, buat aturan yang menolak request ke endpoint API ketika `api_token` berupa array atau objek (misalnya `api_token[]=` pada query/form, atau `"api_token": [` / `"api_token": {` pada JSON body). Ini hanya lapisan tambahan dan bisa terlewat, jadi jangan dijadikan satu-satunya pertahanan.

### 3. Pertimbangkan migrasi ke Sanctum

Sanctum dan Passport tidak terdampak, dan keduanya jauh lebih kaya fitur (hashing token, *abilities*, kedaluwarsa, revoke). Jika API Anda masih memakai driver `token` yang sederhana, ini momen yang baik untuk merencanakan migrasi. Tentu bukan sesuatu yang dikerjakan semalam.

### 4. Batasi dampak jika ada yang lolos

- Terapkan *rate limiting* dan peringatan pada login atau akses mencurigakan di *route* API.
- Batasi hak akses akun yang bisa dijangkau lewat token guard. Prinsip *least privilege* berlaku di sini.
- Jika punya custom guard, pastikan input kredensial divalidasi **tipe**-nya, bukan hanya keberadaannya.

## Cek apakah sudah telanjur disalahgunakan

Tidak ada *indicator of compromise* yang dipublikasikan, jadi pemeriksaannya harus bersifat umum:

- Tinjau log akses untuk request ke *route* terproteksi yang berhasil (200) tanpa token valid, atau request dengan `api_token` berbentuk array/objek.
- Cari login atau aktivitas yang tidak biasa pada akun dengan hak akses tertinggi (admin, billing).
- Periksa perubahan data yang tidak dikenal oleh akun-akun tersebut.
- Jika ada kecurigaan, **rotasi API token** untuk akun sensitif dan investigasi lebih lanjut.

Karena celah ini membuat penyerang tampil sebagai user lain, jejaknya di log aplikasi bisa terlihat seperti aktivitas normal. Log tingkat web server (body request, bila disimpan) biasanya lebih berguna daripada log aplikasi.

## Satu advisory lain yang terbit bersamaan

Pada hari yang sama Laravel juga mempublikasikan **GHSA-5wcm-3v2w-8362** (CVSS v3.1 6.5, medium) pada fitur **JSON:API resources**. Request ke *endpoint* baca-saja bisa dibuat sehingga memicu operasi database destruktif pada tabel terkait, dan user dengan akses baca saja bisa mengosongkan tabel tersebut.

- Terdampak: Laravel 12.45.0 hingga sebelum 12.68.0, dan 13.x sebelum 13.27.0.
- Diperbaiki di 12.68.0 dan 13.27.0.

Kalau Anda update ke 12.68.0 atau 13.29.0 untuk celah token guard, yang ini ikut tertutup.

## Pelajaran yang bisa dibawa pulang

1. **Validasi tipe, bukan hanya keberadaan.** `! empty($token)` terasa aman, padahal array `[0]` juga "tidak kosong". Input dari luar harus dicek tipenya sebelum masuk ke query.
2. **Loose typing di database bisa jadi celah.** Konversi implisit string ke angka di MySQL punya sejarah panjang sebagai sumber bug keamanan. Perilaku seperti ini mudah luput dari mata.
3. **Patch bisa sudah ada sebelum advisory.** Perbaikan di 12.68.0 terbit hampir dua bulan sebelum disclosure. Update dependency secara rutin lebih murah daripada menambal darurat.
4. **Pantau advisory, bukan hanya changelog.** Catatan rilis tidak menyebut perbaikan keamanan ini. `composer audit` di CI adalah cara murah untuk mendapat peringatan.
5. **Tahu guard apa yang Anda pakai.** Banyak tim tidak sadar mereka memakai driver `token` bawaan, bukan Sanctum. Inventaris autentikasi ikut menentukan seberapa cepat Anda bisa menjawab saat advisory seperti ini muncul.

Semoga bermanfaat. Silakan bagikan ke teman atau tim yang menjalankan Laravel di production.

---

## Sumber

- [Token-based API guard Auth Bypass, GHSA-f9x3-7vq9-cph6](https://github.com/laravel/framework/security/advisories/GHSA-f9x3-7vq9-cph6) (GitHub Security Advisory, laravel/framework, 19 Oktober 2026)
- [Commit 9b21ce0 "fix token retrieval"](https://github.com/laravel/framework/commit/9b21ce0a9b) (laravel/framework)
- [Critical Token Guard Auth Bypass Fixed in Laravel 13.29 and 12.68](https://laravel-news.com/laravel-token-guard-auth-bypass) (Laravel News)
- [Laravel v12.68.0 release](https://github.com/laravel/framework/releases/tag/v12.68.0) (GitHub)
- [Laravel Authentication Bypass PoC Released for Critical TokenGuard Flaw CVE-2026-114509](https://securityonline.info/laravel-authentication-bypass-cve-2026-114509-poc/) (SecurityOnline)
- [Laravel Fixes a Critical TokenGuard Authentication Vulnerability](https://pandacore.net/blog/laravel-tokenguard-critical-vulnerability-ghsa-f9x3-7vq9-cph6) (Panda Core)
- [Laravel 12.68.0 and 13.29.0 fix token guard auth bypass](https://pulse.adyog.com/insights/laravel-token-guard-auth-bypass) (Pulse)
