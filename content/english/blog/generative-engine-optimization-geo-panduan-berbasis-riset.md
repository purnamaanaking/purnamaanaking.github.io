---
title: "Generative Engine Optimization (GEO): Apa Kata Riset tentang Cara Konten Muncul di Jawaban AI"
meta_title: "Generative Engine Optimization (GEO): Panduan Berbasis Riset"
description: "Membedah GEO dari paper KDD 2024 (Princeton), studi 2025 tentang bias earned media di AI search, panduan resmi Google, dan data Pew: apa yang terbukti, apa yang belum."
date: 2026-10-24T00:00:00Z
image: "/images/blog/generative-engine-optimization-featured.jpg"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["geo", "seo", "ai", "llm", "search", "content", "research"]
draft: false
---

Semakin banyak orang bertanya ke ChatGPT, Perplexity, atau Google AI Overviews, lalu membaca jawabannya tanpa membuka satu pun situs. Bagi siapa pun yang menulis konten, ini mengubah pertanyaannya. Dulu: *bagaimana agar halaman saya di peringkat satu?* Sekarang: *bagaimana agar konten saya dikutip di dalam jawaban?*

Itulah wilayah **Generative Engine Optimization (GEO)**. Istilah ini banyak dipakai lembaga konsultan dan vendor tool, dan klaimnya sering berlebihan. Karena itu tulisan ini hanya bersandar pada riset yang bisa diperiksa: dua paper, satu studi survei perilaku, dan dokumentasi resmi Google. Daftar rujukannya ada di akhir.

> Catatan: saya hanya mengutip angka dari bagian paper yang sudah saya baca langsung. Beberapa angka di bawah adalah persentase relatif yang saya hitung sendiri dari tabel paper, dan saya tandai begitu.

## Apa itu GEO?

*Generative engine* adalah sistem pencarian yang memakai LLM untuk mengambil beberapa sumber, lalu meringkasnya menjadi satu jawaban yang menyertakan kutipan. Paper yang memperkenalkan istilah GEO mendefinisikannya sebagai kerangka *black-box optimization* untuk meningkatkan visibilitas konten di dalam jawaban semacam itu. Penulis konten hampir tidak punya kontrol atas kapan dan bagaimana kontennya muncul di sana.

Bedanya dengan SEO klasik terletak pada "hasil akhirnya":

| | SEO klasik | GEO |
|---|---|---|
| Hasil | Daftar link berperingkat | Satu jawaban sintetis dengan beberapa kutipan |
| Target | Peringkat teratas | Dikutip, dan seberapa menonjol kutipannya |
| Pembaca | Pengguna yang mengklik | Pengguna yang sering cukup membaca jawabannya |

## Seberapa penting? Data dari Pew Research

Sebelum membahas teknik, kita perlu tahu apakah ini layak diperhatikan. Pew Research Center melacak aktivitas browsing 900 orang dewasa di Amerika Serikat selama Maret 2025 dan membandingkan perilaku mereka pada halaman Google dengan dan tanpa ringkasan AI:

- Klik ke link hasil pencarian biasa: **8%** pada halaman dengan ringkasan AI, dibanding **15%** tanpa ringkasan AI.
- Klik pada link di dalam ringkasan AI itu sendiri: **1%** kunjungan.
- Sesi browsing yang berakhir setelah halaman itu: **26%** dengan ringkasan AI, dibanding **16%** tanpa.

Konteks yang perlu diingat: sampelnya pengguna Amerika, dan hanya sekitar 18% dari 68.879 pencarian dalam data itu yang memunculkan ringkasan AI. Angkanya bukan angka global, dan perilaku bisa sudah bergeser sejak Maret 2025. Arahnya cukup jelas: ketika jawaban AI muncul, klik berkurang. Maka dikutip di dalam jawaban menjadi hal yang layak diperjuangkan.

## Paper yang memulai: "GEO: Generative Engine Optimization"

Paper ini ditulis Pranjal Aggarwal, Vishvak Murahari, Tanmay Rajpurohit, Ashwin Kalyan, Karthik Narasimhan, dan Ameet Deshpande, diterima di **KDD 2024** (arXiv:2311.09735). Yang membuatnya berharga adalah metodenya: mereka mengukur, bukan berpendapat.

### Cara pengujiannya

- **GEO-bench**: benchmark berisi **10.000 query** dari 25 domain dan 9 sumber dataset (di antaranya MS MARCO, Natural Questions, dan ELI5). Sekitar 80% query bersifat informasional.
- Tiap query dilengkapi teks bersih dari 5 hasil teratas Google. Sebuah generative engine menjawab dengan sumber-sumber itu, lalu peneliti mengubah isi **satu** sumber dengan salah satu dari 9 metode dan melihat apakah sumber tersebut jadi lebih menonjol di jawaban.
- **Metrik visibilitas**: *Position-Adjusted Word Count* (PAWC), yakni jumlah kata dari kalimat yang mengutip sumber, diberi bobot lebih besar bila kutipannya muncul lebih awal. Ada juga *Subjective Impression*, gabungan tujuh penilaian (relevansi, pengaruh, keunikan, dan lain-lain) yang dinilai dengan LLM.

### 9 metode yang diuji

Authoritative, Statistics Addition, Keyword Stuffing, Cite Sources, Quotation Addition, Easy-to-Understand, Fluency Optimization, Unique Words, dan Technical Terms.

### Hasilnya

Metode terbaik meningkatkan visibilitas hingga sekitar **41%** pada PAWC dan **28%** pada Subjective Impression (abstraknya menulis "up to 40%"). Berikut sebagian isi Tabel 1. Persentase relatif dihitung dari angka tabel terhadap baseline PAWC 19,5:

| Metode | PAWC | Relatif terhadap baseline |
|---|---|---|
| Quotation Addition | 27,8 | sekitar +43% |
| Statistics Addition | 25,9 | sekitar +33% |
| Fluency Optimization | 25,1 | sekitar +29% |
| Cite Sources | 24,9 | sekitar +28% |
| **Keyword Stuffing** | 17,8 | **sekitar −9% (di bawah baseline)** |

Tiga temuan yang menurut saya paling penting:

1. **Menambahkan kutipan, statistik, dan sumber** adalah metode terkuat. Konten yang *menunjukkan rujukannya* lebih mudah dipilih mesin untuk dikutip.
2. **Keyword stuffing, trik utama SEO lama, tidak membantu.** Paper menyimpulkan metode itu memberi sedikit atau tidak ada peningkatan, dan pada PAWC malah di bawah baseline.
3. **Situs berperingkat rendah paling diuntungkan.** Dengan Cite Sources, visibilitas situs di peringkat 5 naik sekitar **115%**, sedangkan situs peringkat 1 turun sekitar **30%**. Penulisnya menyimpulkan situs yang lebih kecil mendapat manfaat jauh lebih besar dari GEO.

Efektivitas juga **bergantung domain**. Misalnya Cite Sources paling membantu untuk topik fakta dan hukum/pemerintahan, Quotation Addition untuk topik sejarah dan masyarakat, sedangkan Fluency Optimization untuk bisnis, sains, dan kesehatan. Menggabungkan Fluency Optimization dengan Statistics Addition mengalahkan metode tunggal mana pun lebih dari 5,5% (pada subset 200 contoh).

Penulis juga mencoba pada Perplexity.ai. Quotation Addition menaikkan PAWC sekitar 22%, Statistics Addition menaikkan skor subjektif hingga 37%, dan Keyword Stuffing lagi-lagi menurunkan PAWC sekitar 10%.

### Batasan yang diakui penulis

- Pengujian hanya pada dua generative engine, dan hanya satu yang tersedia untuk publik. Mesin berubah cepat, sehingga hasilnya bisa bergeser.
- Metrik subjektif dinilai oleh LLM, dan label domain dibuat dengan GPT-4, yang bisa mengandung noise.
- Mereka **tidak** menguji dampak teknik ini terhadap peringkat di mesin pencari biasa.
- Konteks yang dipakai adalah 5 hasil teratas Google. Itu simulasi, bukan pengukuran di ChatGPT atau AI Overviews yang sebenarnya.

Satu catatan dari saya: "naik 40%" adalah kenaikan relatif pada metrik visibilitas di dalam jawaban, bukan kenaikan traffic.

## Studi kedua: AI search lebih menyukai *earned media*

Paper kedua, "Generative Engine Optimization: How to Dominate AI Search" (Mahe Chen, Xiaoxuan Wang, Kaiwen Chen, Nick Koudas; arXiv:2509.08919, September 2025), meneliti dari sisi lain: **sumber seperti apa yang dikutip mesin AI?** Perlu dicatat, ini *preprint* di arXiv dan tidak mencantumkan venue publikasi, jadi statusnya belum melewati peer review seperti paper KDD di atas.

Mereka membandingkan Google dengan empat layanan AI search (Perplexity, Claude, Gemini, dan GPT dengan fitur web search) pada sekitar 1.000 prompt untuk produk konsumen (otomotif, elektronik, dan software), di Amerika Serikat dan Kanada. Sumber dikelompokkan menjadi tiga:

- **Brand**: situs resmi pemilik produk atau layanan.
- **Earned**: media independen, situs ulasan, dan perbandingan.
- **Social**: platform sosial, forum, dan konten buatan pengguna.

Hasilnya untuk pencarian di AS:

| Vertikal | Earned di Google | Earned di AI search |
|---|---|---|
| Otomotif | 45,1% | 81,9% |
| Software | 45,4% | 72,7% |
| Elektronik konsumen | - | 92,1% |

Konten Social nyaris hilang dari sitasi AI (di beberapa vertikal 0%), sementara Google masih menampilkan 10-23% konten Social. Tumpang tindih domain dengan Google pun sedang saja, misalnya sekitar sepertiga pada 5 teratas untuk mobil listrik. Artinya, **peringkat bagus di Google tidak otomatis berarti dikutip AI.**

Temuan lain: hasil AI search lebih stabil terhadap perubahan kata dalam pertanyaan dibanding perubahan bahasa. Antar-bahasa, Claude paling konsisten, sedangkan Google dan GPT hampir tidak punya tumpang tindih domain lintas bahasa. Paper ini juga membahas *freshness* dan bias terhadap merek besar, tetapi karena saya tidak membaca bagian itu sampai tuntas, angkanya tidak saya kutip di sini.

Batasannya: model yang diuji adalah versi pertengahan 2025 (misalnya `claude-3.5-sonnet`, `gpt-4o-search-preview`, `gemini-2.5-flash`), objeknya produk konsumen, dan klasifikasi sumbernya dibantu GPT-4o. Mengambil kesimpulan untuk blog teknis atau konten akademik butuh kehati-hatian.

## Sudut pandang Google: tidak ada trik khusus

Untuk AI Overviews dan AI Mode, dokumentasi resmi Google menyatakan: *"There are no additional requirements to appear in AI Overviews or AI Mode, nor other special optimizations necessary."* Mereka juga menyebut:

- Tidak ada schema.org khusus yang perlu ditambahkan.
- Tidak perlu membuat file machine-readable baru, file teks khusus AI, atau markup khusus.
- Fondasi SEO tetap berlaku: halaman bisa di-crawl, ter-index dan layak tampil dengan snippet, ada link internal, konten penting tersedia dalam bentuk teks, dan structured data sesuai dengan teks yang terlihat.
- Fokus pada pengunjung dan konten yang unik serta memuaskan.

Sekilas ini tampak bertentangan dengan paper GEO, padahal keduanya menjawab hal yang berbeda. Google bicara soal **syarat teknis**: tidak ada tombol rahasia. Paper GEO bicara soal **isi tulisan**: mana yang lebih sering dipilih ketika mesin merangkai jawaban. Tapi perlu jujur juga: paper Aggarwal dkk. tidak menguji AI Overviews, jadi tidak ada bukti langsung bahwa teknik mereka bekerja di sana.

## Yang perlu diwaspadai: sisi manipulasi

Karena ada insentif untuk dikutip, ada pula yang mencoba memanipulasinya. Nestaas, Debenedetti, dan Tramèr (ETH Zürich, ICLR 2025) menunjukkan *preference manipulation attacks*: konten yang disusun khusus sehingga LLM condong merekomendasikan produk penyerang dan merendahkan pesaingnya. Mereka mengujinya pada Bing dan Perplexity. Penulisnya memperingatkan bahwa kalau semua pihak ikut menyerang, kualitas jawaban turun untuk semua orang, semacam *prisoner's dilemma*.

Pelajarannya: teknik GEO yang saya bahas di sini (kutipan, data, kejelasan) adalah memperbaiki mutu konten. Itu berbeda dengan menyuntikkan instruksi tersembunyi atau teks yang ditulis untuk mengakali model. Yang kedua berisiko dan bisa merusak kepercayaan pada situs Anda.

## Jadi, apa yang bisa dikerjakan?

Berdasarkan semua sumber di atas, ini rangkuman yang menurut saya masuk akal. Tidak semuanya terbukti sama kuat, jadi saya beri label.

**Didukung pengukuran langsung (paper KDD 2024):**

1. **Rujuk sumber yang kredibel** di dalam tulisan, dengan tautan dan nama yang jelas.
2. **Pakai data konkret** menggantikan klaim kualitatif, misalnya "naik 43%" ketimbang "naik cukup banyak", lengkap dengan asalnya.
3. **Sertakan kutipan langsung** dari sumber yang relevan.
4. **Tulis dengan jelas dan lancar.** Fluency Optimization masuk empat besar.
5. **Jangan keyword stuffing.** Tidak membantu, bahkan bisa merugikan.

**Didukung studi observasional (preprint 2025, produk konsumen):**

6. **Bangun jejak di luar situs sendiri**: ulasan, perbandingan, dan liputan pihak ketiga. Untuk kategori yang diteliti, AI search jauh lebih sering mengutip earned media daripada situs brand.
7. **Susun konten agar mudah dipindai dan dibenarkan oleh mesin**: tabel perbandingan, daftar pro/kontra, klaim yang jelas.

**Didukung dokumentasi Google (bukan eksperimen GEO):**

8. **Pastikan fondasi SEO beres**: bisa di-crawl, ter-index, konten utama berupa teks HTML, dan structured data sesuai teks.
9. **Tidak perlu `llms.txt` atau markup khusus AI untuk muncul di Google AI features.** Dokumentasi Google menyatakan itu tidak diperlukan. Untuk mesin lain saya belum menemukan bukti ilmiah yang mendukung atau menolak.

Satu hal yang tidak boleh dilupakan: banyak teknik di atas hanyalah praktik menulis yang baik. Itu kabar baik, karena mengoptimalkan untuk mesin dan untuk pembaca ternyata sebagian besar sejalan.

## Penutup

GEO adalah bidang yang sangat muda. Bukti yang kokoh baru segelintir paper, diuji pada mesin yang terus berubah, dan sebagian klaim di internet (termasuk angka-angka dari blog vendor) tidak bisa ditelusuri ke paper aslinya. Pegangan yang aman: perlakukan GEO sebagai kelanjutan dari menulis konten yang akurat, punya rujukan, dan jelas, lalu ukur hasilnya sendiri alih-alih percaya pada resep instan.

Semoga bermanfaat. Silakan bagikan ke teman atau tim yang mengelola konten dan website.

---

## Sumber

- Aggarwal, P., Murahari, V., Rajpurohit, T., Kalyan, A., Narasimhan, K., Deshpande, A. [GEO: Generative Engine Optimization](https://arxiv.org/abs/2311.09735) (KDD 2024, arXiv:2311.09735)
- Chen, M., Wang, X., Chen, K., Koudas, N. [Generative Engine Optimization: How to Dominate AI Search](https://arxiv.org/abs/2509.08919) (arXiv:2509.08919, preprint, September 2025)
- Nestaas, F., Debenedetti, E., Tramèr, F. [Adversarial Search Engine Optimization for Large Language Models](https://arxiv.org/abs/2406.18382) (ICLR 2025, arXiv:2406.18382)
- Pew Research Center. [Google users are less likely to click on links when an AI summary appears in the results](https://www.pewresearch.org/short-reads/2025/07/22/google-users-are-less-likely-to-click-on-links-when-an-ai-summary-appears-in-the-results/) (22 Juli 2025)
- Google Search Central. [AI features and your website](https://developers.google.com/search/docs/appearance/ai-features)
- Google Search Central Blog. [Top ways to ensure your content performs well in Google's AI experiences on Search](https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search) (21 Mei 2025)
