---
title: "Modul #07 Version Control System (Git dan Github)"
meta_title: "Modul #07 Version Control System (Git dan Github)"
description: "Modul #07 Version Control System (Git dan Github)"
date: 2023-12-17T05:00:00Z
image: "/images/blog/version-control-system-git-github.png"
categories: ["Laravel"]
author: "Purnama Anaking"
tags:
  ["modul", "praktikum", "pemrograman", "framework", "laravel", "git", "github"]
draft: false
---

Pada modul kali ini kita akan melanjutkan belajar tentang penggunaan GIT dan Github di dalam mengelola sebuah project pemrograman. Materi akan diilustrasikan dalam bentuk studi kasus. Kegiatan ini dilakukan agar kita dapat menerapkan dan mengelola kode program yang dibangun bersama tim menggunakan version control system GIT.

#### 1. Instalasi dan Konfirgurasi GIT

- Download installer GIT dari https://git-scm.com/
- Jalankan installer dan ikuti langkah-langkah nya seperti di bawah ini.

  {{< image src="images/blog/git-setup-1.png" caption="" alt="Git Setup" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}
  {{< image src="images/blog/git-setup-2.png" caption="" alt="Git Setup" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}
  {{< image src="images/blog/git-setup-3.png" caption="" alt="Git Setup" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}
  {{< image src="images/blog/git-setup-4.png" caption="" alt="Git Setup" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}
  {{< image src="images/blog/git-setup-5.png" caption="" alt="Git Setup" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}
  {{< image src="images/blog/git-setup-6.png" caption="" alt="Git Setup" height="" width="300" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Untuk memastikan GIT telah ter-install dengan baik, buka terminal dan jalankan perintah **git –version**, maka akan muncul seperti di bawah ini.
  {{< image src="images/blog/git-version.png" caption="" alt="Git Version" height="" width="600" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Buka GIT Bash / cmd / terminal, konfigurasi name dan email pada GIT dengan command:

  ```
  git config --global user.name "Nama Anda"
  ```

  ```
  git config --global user.email email@anda.com

  ```

- Jalankan perintah di bawah ini untuk memeriksa hasil konfigurasi yang baru saja dilakukan.

  ```
  git config --list
  ```

#### 2. Membuat Repository Baru pada Github

- Buka dan login pada platform https://github.com/
- Pilih menu **“New repository”**

  {{< image src="images/blog/github-new-repo.png" caption="" alt="Github New Repo" height="" width="300" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Isi **“Repository Name”** dengan **“bootstrap-clone”**
- Klik **“Create repository”**

  {{< image src="images/blog/github-create-repo.png" caption="" alt="Github Create Repo" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Akan muncul halaman awal repository project **“bootstrap-clone”** yang baru saja di buat pada Github anda.

#### 3. Inisiasi Awal Direktori Project pada Local Computer

- Buat folder di local computer anda dengan nama **“bootstrap-clone”**
- Tambahkan sebuah file dengan nama **index.html** pada folder yang baru saja di buat. Isikan file **index.html** dengan kode program seperti di bawah ini.

  ```html
  <!doctype html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Bootstrap Clone</title>
    </head>
    <body>
      <h1>Bootstrap Clone</h1>
    </body>
  </html>
  ```

- Tambahkan sebuah file dengan nama **README.md** pada folder yang baru saja di buat. Isikan file **README.md** dengan kode program seperti di bawah ini.

  ```bash
  # Bootstrap Clone

  Clone Bootstrap Homepage with Bootstrap CSS Framework
  ```

#### 4. Arahkan Direktori Project pada Local Computer ke Github Repository

- Buat **GIT bash / cmd / terminal**, arahkan pada direktori project **“bootstrap-clone”** yang telah dibuat sebelumnya. Atau bisa juga buka direktori project **“bootstrap-clone”** pada **vsCode**, kemudian buka terminal baru pada vsCode tersebut.

  {{< image src="images/blog/vscode-terminal-bs.png" caption="" alt="VsCode Terminal Bootstrap" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Ketikkan perintah di bawah ini untuk melakukan inisialisasi GIT pada project **“bootstrat-clone”** yang baru saja dibuat.

  ```
  git init
  ```

- Ketikkan perintah di bawah ini untuk melihat status direktori project **“bootstrap-clone”** yang baru saja dibuat dan ditambahkan file **index.html** dan **README.md**.

  ```
  git status
  ```

- Ketikkan perintah di bawah ini agar GIT **“men-track” semua perubahan** yang terjadi pada direktori project.

  ```
  git add .
  ```

- Ketikkan perintah di bawah ini agar GIT **_“men-commit”_ semua perubahan** yang terjadi pada direktori project. Opsi **-m** kita gunakan untuk menuliskan pesan mengenai aksi apa yang telah dilakukan terkait perubahan kode program yang terjadi.

  ```
  git commit -m "Inisialisasi awal"
  ```

- Ketikkan perintah di bawah ini untuk memeriksa Git anda memiliki branch apa saja dan sedang aktif pada branch apa. Pada tahapan ini akan terihat bahwa anda baru memiliki satu branch dengan nama master dan Git anda sedang aktif pada branch master tersebut.

  ```
  git branch
  ```

- Ketikkan perintah di bawah ini untuk mengarahkan direktori project yang ada di local computer kita ke repository server Github yang telah dibuat sebelumnya. **Sesuaikan URL Repository dengan milik anda sendiri**.

  ```
  git remote add origin https://github.com/purnamaanaking/bootstrap-clone.git
  ```

- Setting Personal Access Token (PAT) pada Github. Klik menu **Settings > Developer Settings > Personal Access Tokens > Tokens (classic)**.
  - Klik menu **“Generate new token (classic)”**
  - Isi **Notes** dengan misal: **“Personal Access”**.
  - Pilih **Expiration** dengan misal: **“30 days”**.
  - Centang semua pada bagian **Select Scopes**.
  - Klik **“Generate Token”**.
  - Simpan **Token** yang sudah ter-generate yang nanti akan digunakan jika diminta ketika melakukan **git push**.
- Ketikkan perintah di bawah ini untuk membawa semua perubahan yang terjadi pada direktori project pada local computer kita ke repository server pada Github dan kita bawa semua perubahan itu ke branch **master** yang ada di repository server Github.

  ```
  git push origin master
  ```

- Buka kembali halaman repository project **“bootstrap-clone”** pada Github anda. Maka pada tahapan ini repo Github anda telah berisi satu file **index.html** dan satu file **README.md** sesuai dengan perubahan yang anda lakukan pada direktori project di local computer anda.

  {{< image src="images/blog/github-bootstrap-clone.png" caption="" alt="Github Bootstrap Clone" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 5. Menyalin Project yang Sudah Ada pada Repository Github

- Hapus direktori project **“bootstrap-clone”** yang ada di local computer anda.
- Kemudian **“clone”** kembali project tersebut ke local computer anda dari project yang sudah ada di Repository Github.
- Buka halaman repository **“bootstrap-clone”** pada Github. Masuk ke halaman **Settings**. Atur pada bagian **Default Branch**, arahkan ke branch **master**. Jika **Default Branch** kita arahkan ke branch **master**, maka ketika kita melakukan **“clone”** kode program, yang disalin dari remote repository Github nya adalah kode program yang ada pada branch **master**.

  {{< image src="images/blog/github-bootstrap-clone-setting.png" caption="" alt="Bootstrap Clone Setting" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Buka halaman repository **“bootstrap-clone”** pada Github. Dapatkan URL Remote Repository anda dengan klik tombol **“Code”** lihat pada bagian **“HTTPS”**.

  {{< image src="images/blog/github-bootstrap-clone-code.png" caption="" alt="Bootstrap Clone Code" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Buka **Git Bash / cmd / terminal**, arahkan pada folder tertentu yang anda inginkan, kemudian ketikkan perintah di bawah ini untuk melakukan **“clone”** ke local computer kita. **Sesuaikan URL remote repository dengan milik anda sendiri**.

  ```
  git clone https://github.com/purnamaanaking/bootstrap-clone.git
  ```

- Jika berhasil maka akan muncul kembali direktori **“bootstrap-clone”** pada local computer anda yang telah terhubung dengan remote repository pada Github.

#### 6. Buat Percabangan (Branch) Baru untuk Kegiatan Develop Kode Program

- Buka vsCode dan arahkan pada direktori **“bootstrap-clone”** yang ada di local computer anda dan buka **terminal vsCode** pada direktori tersebut.
- Ketikkan perintah di bawah ini untuk membuat branch baru dengan nama **“develop”** yang nantinya akan digunakan sebagai branch aktif untuk men-develop kode program selanjutnya.

  ```
  git branch develop
  ```

- Ketikkan perintah di bawah ini untuk memeriksa branch apa saja yang ada pada GIT di local computer kita. Pada tahapan ini akan tampak bahwa ada 2 branch yaitu branch **master** dan branch **develop**.

  ```
  git branch
  ```

- Ketikkan perintah di bawah ini untuk memerintahkan GIT untuk aktif pada branch **“develop”** yang baru saja dibuat.

  ```
  git checkout develop
  ```

- Lakukan kegiatan develop / perubahan kode program sehingga menghasikan struktur direktori project **“bootstrap-clone”** seperti di bawah ini.

  {{< image src="images/blog/bootstrap-clone-dir.png" caption="" alt="Bootstrap Clone Directory" height="" width="200" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Tuliskan kode program di bawah ini pada file **style.css**.

  ```css
  .bg-purple {
    background-color: #7533f9 !important;
  }

  #main a {
    color: gray;
  }

  #main a:hover {
    color: rgb(73, 73, 73);
  }

  #footer a {
    text-decoration: none;
    color: black;
  }

  #footer a:hover {
    text-decoration: underline;
    color: var(--bs-primary) !important;
  }
  ```

- Tuliskan kode program di bawah ini pada file **index.html**.

  ```html
  <!doctype html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Bootstrap Clone</title>
      <link
        rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha2/dist/css/bootstrap.min.css"
      />
      <link
        rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.6.0/font/bootstrap-icons.css"
      />
      <link rel="stylesheet" href="assets/css/style.css" />
    </head>
    <body>
      <!-- Navbar -->
      <nav
        class="navbar fixed-top navbar-expand-lg bg-purple"
        data-bs-theme="dark"
      >
        <!-- Container -->
        <div class="container py-2 px-4">
          <!-- Navbar Brand -->
          <a href="#" class="navbar-brand mb-0 h1">
            <img
              class="img-fluid"
              src="assets/images/logo-white.svg"
              alt="logo"
              style="width: 40px;"
            />
          </a>

          <!-- Navbar Toggler -->
          <button
            type="button"
            class="navbar-toggler"
            data-bs-toggle="offcanvas"
            data-bs-target="#offcanvasNavbar"
          >
            <span class="navbar-toggler-icon"></span>
          </button>

          <!-- Offcanvas -->
          <div class="offcanvas offcanvas-end bg-purple" id="offcanvasNavbar">
            <!-- Offcanvas Header -->
            <div class="offcanvas-header pb-0 px-4">
              <h5 class="offcanvas-title text-white" id="offcanvasNavbarLabel">
                Bootstrap
              </h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="offcanvas"
                aria-label="Close"
              ></button>
            </div>

            <!-- Offcanvas Body -->
            <div class="offcanvas-body pt-0 px-4">
              <hr class="d-lg-none text-white-50" />
              <!-- Horizontal Line -->

              <!-- Top Menu -->
              <ul class="navbar-nav flex-row flex-wrap">
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link active">Docs</a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link">Examples</a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link">Icons</a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link">Themes</a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link">Blog</a>
                </li>
              </ul>

              <hr class="d-lg-none text-white-50" />
              <!-- Horizontal Line -->

              <!-- Sosial Media  -->
              <ul class="navbar-nav flex-row flex-wrap ms-md-auto">
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link active">
                    <i class="bi-github"></i
                    ><small class="ms-2 d-lg-none">Github</small>
                  </a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link active">
                    <i class="bi-twitter"></i
                    ><small class="ms-2 d-lg-none">Twitter</small>
                  </a>
                </li>
                <li class="nav-item col-6 col-lg-auto">
                  <a href="#" class="nav-link active">
                    <i class="bi-slack"></i
                    ><small class="ms-2 d-lg-none">Slack</small>
                  </a>
                </li>
                <li class="nav-item col-12 col-lg-auto">
                  <div
                    class="vr d-none d-lg-flex h-100 mx-lg-2 text-white"
                  ></div>
                  <hr class="d-lg-none my-2 text-white-50" />
                </li>
                <li class="nav-item">
                  <!-- Dropdown -->
                  <div class="dropdown" data-bs-theme="light">
                    <button
                      type="button"
                      class="btn nav-link text-white dropdown-toggle"
                      data-bs-toggle="dropdown"
                      aria-expanded="false"
                    >
                      <span class="d-lg-none">Bootstrap</span> v5.3
                    </button>
                    <ul class="dropdown-menu dropdown-menu-end">
                      <li><h6 class="dropdown-header">v5 releases</h6></li>
                      <li>
                        <button
                          class="dropdown-item active bg-purple"
                          type="button"
                        >
                          <small>Latest (5.3.x) <i class="bi-check"></i></small>
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v5.2.3</small>
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v5.1.3</small>
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v5.0.2</small>
                        </button>
                      </li>
                      <li><hr class="dropdown-divider" /></li>
                      <li>
                        <h6 class="dropdown-header">Previous releases</h6>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v4.6.x</small>
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v3.4.1</small>
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>v2.3.2</small>
                        </button>
                      </li>
                      <li><hr class="dropdown-divider" /></li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small>All versions</small>
                        </button>
                      </li>
                    </ul>
                  </div>
                </li>
                <li class="nav-item col-12 col-lg-auto">
                  <div
                    class="vr d-none d-lg-flex h-100 mx-lg-2 text-white"
                  ></div>
                  <hr class="d-lg-none my-2 text-white-50" />
                </li>
                <li class="nav-item">
                  <!-- Dropdown -->
                  <div class="dropdown" data-bs-theme="light">
                    <button
                      type="button"
                      class="btn nav-link text-white dropdown-toggle"
                      data-bs-toggle="dropdown"
                      aria-expanded="false"
                    >
                      <i class="bi-brightness-high-fill"></i
                      ><span class="d-lg-none">Toggle theme</span>
                    </button>
                    <ul class="dropdown-menu dropdown-menu-end">
                      <li>
                        <button
                          class="dropdown-item active bg-purple"
                          type="button"
                        >
                          <small
                            ><i class="bi-brightness-high-fill me-2"></i
                            >Light</small
                          >
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small
                            ><i class="bi-moon-stars-fill me-2"></i>Dark</small
                          >
                        </button>
                      </li>
                      <li>
                        <button class="dropdown-item" type="button">
                          <small><i class="bi-circle-half me-2"></i>Auto</small>
                        </button>
                      </li>
                    </ul>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </nav>

      <!-- Main -->
      <div class="bg-light mt-5" id="main">
        <!-- Container -->
        <div class="container py-5 px-4">
          <div class="row">
            <div class="col-md-5 order-md-2">
              <img
                class="img-fluid"
                src="assets/images/main.svg"
                alt="main logo"
              />
            </div>
            <div class="col-md-7 order-md-1">
              <h1 class="mt-4 display-3">
                Build fast, responsive sites with Bootstrap
              </h1>
              <p class="fs-5 mt-3">
                Quickly design and customize responsive mobile-first sites with
                Bootstrap, the world's most popular front-end open source
                toolkit, featuring Sass variables and mixins, responsive grid
                system, extensive prebuilt components, and powerful JavaScript
                plugins.
              </p>
              <div class="row">
                <div class="d-flex flex-column flex-md-row">
                  <button
                    type="button"
                    class="btn bg-purple text-white btn-lg mb-3 me-md-3 px-4 py-3"
                  >
                    Get Started
                  </button>
                  <button
                    type="button"
                    class="btn btn-outline-dark btn-lg mb-3 px-4 py-3"
                  >
                    Download
                  </button>
                </div>
              </div>
              <div class="text-muted">
                Currently <strong>v5.3.0-alpha2.</strong> ·
                <a href="#">v4.6.x docs</a> · <a href="#">All releases</a>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Bootstrap Icons -->
      <div id="bs-icons">
        <!-- Container -->
        <div class="container py-5 px-4">
          <div class="row">
            <div class="col-lg-6">
              <div class="mb-3">
                <i
                  class="bi bi-subtract fs-2 bg-warning py-2 px-3 rounded-3 text-white"
                ></i>
              </div>
              <h2 class="display-5 mb-3">
                Personalize it with <br />Bootstrap Icons
              </h2>
              <p class="fs-5">
                <a href="#">Bootstrap Icons</a> is an open source SVG icon
                library featuring over 1,800 glyphs, with more added every
                release. They're designed to work in any project, whether you
                use Bootstrap itself or not. Use them as SVGs or icon
                fonts--both options give you vector scaling and easy
                customization via CSS.
              </p>
              <a
                class="icon-link icon-link-hover lead fw-semibold mb-5"
                href="#"
              >
                Get Bootstrap Icons
                <i class="bi bi-arrow-right mb-2"></i>
              </a>
            </div>
            <div class="col-lg-6">
              <img
                class="img-fluid"
                src="assets/images/bs-icons.png"
                alt="Bootstrap Icons"
              />
            </div>
          </div>
        </div>
      </div>

      <!-- Bootstrap Official Themes -->
      <div id="bs-themes">
        <!-- Container -->
        <div class="container py-5 px-4">
          <div class="row">
            <div class="col-lg-6">
              <div class="mb-3 mt-5">
                <i
                  class="bi bi-droplet-fill fs-2 bg-info py-2 px-3 rounded-3 text-white"
                ></i>
              </div>
              <h2 class="display-5">
                Make it yours with official Bootstrap Themes
              </h2>
              <p class="fs-5">
                Take Bootstrap to the next level with premium themes from the
                <a href="#">official Bootstrap Themes marketplace</a>. Themes
                are built on Bootstrap as their own extended frameworks, rich
                with new components and plugins, documentation, and powerful
                build tools.
              </p>
              <a
                class="icon-link icon-link-hover lead fw-semibold mb-5"
                href="#"
              >
                Browse Bootstrap Themes
                <i class="bi bi-arrow-right mb-2"></i>
              </a>
            </div>
            <div class="col-lg-6">
              <img
                class="img-fluid"
                src="assets/images/bs-themes.png"
                alt="Bootstrap Themes"
              />
            </div>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div id="footer" class="bg-light py-5">
        <!-- Container -->
        <div class="container py-5 px-4">
          <div class="row">
            <div class="col-lg-3 mb-5">
              <a href="#" class="logo text-decoration-none">
                <div class="d-flex">
                  <img
                    class="img-fluid"
                    src="assets/images/logo-black.svg"
                    alt="Bootstrap Logo"
                    style="width: 40px;"
                  />
                  <div class="fs-5 ms-2 text-black">Bootstrap</div>
                </div>
              </a>
              <div class="mt-2 text-muted">
                <small
                  >Designed and built with all the love in the world by the
                  <a href="#" class="text-black">Bootstrap team</a> with the
                  help of
                  <a href="#" class="text-black">our contributors</a>.</small
                >
              </div>
              <div class="mt-2 text-muted">
                <small
                  >Code licensed <a href="#" class="text-black">MIT</a>, docs
                  <a href="#" class="text-black">CC BY 3.0</a>.</small
                >
              </div>
              <div class="mt-2 text-muted">
                <small>Currently v5.3.0-alpha2.</small>
              </div>
            </div>
            <div class="col-6 col-lg-2 offset-lg-1 mb-5">
              <h5>Links</h5>
              <ul class="list-unstyled">
                <li class="mb-2"><a href="#">Home</a></li>
                <li class="mb-2"><a href="#">Docs</a></li>
                <li class="mb-2"><a href="#">Examples</a></li>
                <li class="mb-2"><a href="#">Icons</a></li>
                <li class="mb-2"><a href="#">Themes</a></li>
                <li class="mb-2"><a href="#">Blog</a></li>
                <li class="mb-2"><a href="#">Swag Store</a></li>
              </ul>
            </div>
            <div class="col-6 col-lg-2 mb-5">
              <h5>Guides</h5>
              <ul class="list-unstyled">
                <li class="mb-2"><a href="#">Getting started</a></li>
                <li class="mb-2"><a href="#">Starter template</a></li>
                <li class="mb-2"><a href="#">Webpack</a></li>
                <li class="mb-2"><a href="#">Parcel</a></li>
              </ul>
            </div>
            <div class="col-6 col-lg-2 mb-5">
              <h5>Projects</h5>
              <ul class="list-unstyled">
                <li class="mb-2"><a href="#">Bootstrap 5</a></li>
                <li class="mb-2"><a href="#">Bootstrap 4</a></li>
                <li class="mb-2"><a href="#">Icons</a></li>
                <li class="mb-2"><a href="#">RFS</a></li>
                <li class="mb-2"><a href="#">npm starter</a></li>
              </ul>
            </div>
            <div class="col-6 col-lg-2 mb-5">
              <h5>Community</h5>
              <ul class="list-unstyled">
                <li class="mb-2"><a href="#">Issues</a></li>
                <li class="mb-2"><a href="#">Discussions</a></li>
                <li class="mb-2"><a href="#">Corporate sponsors</a></li>
                <li class="mb-2"><a href="#">Open Collective</a></li>
                <li class="mb-2"><a href="#">Slack</a></li>
                <li class="mb-2"><a href="#">Stack overflow</a></li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha2/dist/js/bootstrap.bundle.min.js"></script>
    </body>
  </html>
  ```

#### 7. Bawa Perubahan Kode Program ke Remote Repository Github

- Ketikkan perintah di bawah ini untuk melihat status direktori project **“bootstrap-clone”** anda dan perubahan apa saja yang terjadi pada direktori tersebut.

  ```
  git status
  ```

- Ketikkan perintah di bawah ini agar GIT **“men-track” semua perubahan** yang terjadi pada direktori project.

  ```
  git add .
  ```

- Ketikkan perintah di bawah ini agar GIT **“men-commit” semua perubahan** yang terjadi pada direktori project. Opsi **-m** kita gunakan untuk menuliskan pesan mengenai aksi apa yang telah dilakukan terkait perubahan kode program yang terjadi.

  ```
  git commit -m "Finalisasi clone halaman utama website bootstrap"
  ```

- Ketikkan perintah di bawah ini untuk membawa semua perubahan yang terjadi pada direktori project pada local computer kita ke repository server pada Github dan kita bawa semua perubahan itu ke branch **develop** pada repository server Github.

  ```
  git push origin develop
  ```

- Pada tahapan ini, jika kita buka halaman respository project **“bootstrap-clone”** pada Github kita, akan terlihat ada 2 branch yaitu **master** dan **develop**. Dua branch tersebut memiliki 2 versi kode program yang berbeda.

  - Branch **master**: berisi file **index.html** dan **README.md** saja
  - Branch **develop**: berisi direktori dan file-file terbaru yang telah ditambahkan pada branch develop di local computer kita.

    {{< image src="images/blog/github-bootstrap-clone-branch.png" caption="" alt="Bootstrap Clone Branch" height="" width="500" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

#### 8. Buat Pull Request untuk Kegiatan Code Review dan Merge ke Branch Utama

- Pada tahapan ini kita ingin versi kode program yang ada pada percabangan (branch) **develop** digabungkan (merge) ke branch **master**, melalui kegiatan **“code review”** dengan membuat sebuah **“pull request”** pada github.
- Buka halaman repository **“bootstrap-clone”** pada Github anda. Masuk ke menu **Pull Request > New pull request**. Arahkan bahwa kita ingin melakukan pull request dari branch **develop** ke branch **master** seperti di bawah ini.

  {{< image src="images/blog/github-pull-request.png" caption="" alt="Github Pull Request" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Klik **“Create pull request”**, maka akan muncul form **“Open a pull request”** seperti di bawah ini. Anda bisa memberikan comment yang nanti dapat dibaca oleh code reviewer nantinya.

  {{< image src="images/blog/github-open-pull-request.png" caption="" alt="Github Open Pull Request" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Klik **“Create pull request”** kembali untuk men-submit sebuah Pull Request.
- Code Reviewer dapat melakukan review dari Pull Request yang diajukan. Jika dianggap telah sesuai, Klik **“Merge pull request”** lalu **“Merge”** untuk menggabungkan atau mensinkronkan kode program dari branch **develop** ke branch **master**.

  {{< image src="images/blog/github-merge-pull-request.png" caption="" alt="Github Merge Pull Request" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

  {{< image src="images/blog/github-pull-request-success.png" caption="" alt="Github Pull Request Success" height="" width="800" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="false" >}}

- Pada tahapan ini di remote repository Github, maka branch **master** telah memiliki kode program yang sama dengan branch **develop**.

#### 9. Mengambil Kode Program Terbaru dari Remote Repository Github

- Ketikkan perintah di bawah ini untuk memeriksa Git anda memiliki branch apa saja dan sedang aktif pada branch apa.

  ```
  git branch
  ```

- Pada tahapan ini di local computer kita ada 2 branch yaitu branch **master** dan **develop**. Dengan kondisi sebagai berikut:
  - Branch **master**: berisi file **index.html** dan **README.md** saja
  - Branch **develop**: berisi direktori dan file-file terbaru yang telah ditambahkan pada branch develop di local computer kita.
- Kita bisa memeriksanya dengan berpindah-pindah branch dengan perintah git checkout.
- Ketikkan perintah di bawah ini untuk memindahkan GIT di local computer kita agar aktif pada branch **master**.

  ```
  git checkout master
  ```

- Kita akan temukan bahwa branch **master** di local computer kita belum berada pada kondisi update / terbaru / tidak sama / tidak sesuai dengan kondisi branch **master** yang ada pada remote repository Github.
- Ketikkan perintah di bawah ini untuk menarik kode program dari branch **master** pada remote repository Github ke branch **master** yang ada di local computer kita.

  ```
  git pull origin master
  ```

- Pada tahapan ini kondisi branch **master** dan branch **develop** pada local computer kita telah sama dengan branch **master** dan branch **develop** pada remote repository Github kita.
- Maka perintah **git pull** juga bisa kita gunakan untuk menarik kode program ketika ada anggota tim lain yang melakukan perubahan kode program dan telah membawa perubahan tersebut ke remote repository Github.

#### 10. Latihan

- Bentuk kelompok yang terdiri dari 4 orang (kelompok yang sama untuk final project UAS nanti).
- Terapkan kolaborasi antar anggota tim menggunakan GIT dan Github untuk membuat project laravel yang kita kerjakan pada **modul 06** kemarin.
- **Orang A** membuat repository baru pada github dan melakukan inisialisasi awal direktori project laravel pada local computer sampai mengarahkan direktori project ke github repository (seperti bab 2-4 pada modul ini).
- **Orang B, C,** dan **D** menyalin project yang sudah dibuat **Orang A** pada Repository Github (seperti bab 5 pada modul ini).
- **Orang A, B, C,** dan **D** membagi tugas masing-masing untuk membuat kode program pada project yang telah ter-setup dengan GIT dan Github tersebut (seperti bab 6 - 7 pada modul ini).
  - **Orang A** bekerja pada branch **“nama-orang-a”**, men-develop kode program bagian tertentu.
  - **Orang B** bekerja pada branch **“nama-orang-b”**, men-develop kode program bagian tertentu lainnya.
  - **Orang C** bekerja pada branch **“nama-orang-c”**, men-develop kode program bagian tertentu lainnya.
  - **Orang D** bekerja pada branch **“nama-orang-d”**, men-develop kode program bagian tertentu lainnya.
- Jika telah selesai men-develop kode program, setiap orang melakukan **Pull Request** dari masing-masing branch ke branch **master** via Github (bab 8 pada modul ini).
- Lakukan kegiatan code review lalu **merge** kode program jika telah sesuai.
- Setiap orang memindahkan branch aktif nya ke branch **master** pada **local computer** masing-masing, kemudian **tarik (pull)** kode program terbaru yang ada di branch **master** pada remote repository **github**.
- Setiap orang memiliki kode program versi terbaru yang dikerjakan bersama-sama dengan GIT dan dapat menjalankan project laravel tersebut pada local computer masing-masing.

#### 11. Tugas

- **Praktekkan** seluruh **poin (bab 1 - 9)** yang ada di atas secara **INDIVIDU**.
- **Praktekkan** poin latihan **(bab 10)** yang ada di atas secara **BERKELOMPOK**.

<hr>

{{< notice "note" >}}

##### Artikel ini merupakan bagian dari:

{{< button label="Kumpulan Modul Belajar Pemrograman Framework Laravel (Tahap Dasar)" link="/blog/kumpulan-modul-praktikum-pemrograman-framework-tahap-dasar/" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}
