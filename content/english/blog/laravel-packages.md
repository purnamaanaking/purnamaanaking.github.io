---
title: "Modul #10 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Third Party Packages"
meta_title: "Modul #10 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Third Party Packages"
description: "Modul #10 Belajar Pemrograman Framework Laravel Tahap Dasar: Laravel Third Party Packages"
date: 2023-12-25T05:00:00Z
image: "/images/blog/laravel-3rd-packages.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags:
  [
    "modul",
    "pemrograman",
    "framework",
    "laravel",
    "package",
    "datatables",
    "sweetalert",
    "excel",
    "dompdf",
  ]
draft: false
---

{{< notice "note" >}}

##### Artikel ini merupakan bagian dari:

{{< button label="Kumpulan Modul Belajar Dasar Pemrograman Framework Laravel" link="/blog/kumpulan-modul-pemrograman-framework-tahap-dasar/" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}

Pada modul kali ini kita akan melanjutkan belajar tentang bagaimana menerapkan package-package tambahan pada project laravel kita. Package tambahan yang diberikan pada modul ini adalah **Laravel DataTables**, **Laravel Sweet Alert**, **Laravel Excel** dan **Laravel DomPdf**. Seluruh package akan diterapkan pada project laravel yang telah dibuat pada pembelajaran [**Modul #09**](/blog/laravel-factory-file-storage/).

#### 1. Penerapan Laravel DataTables

- Edit file **EmployeeSeeder** untuk dapat menambahkan data dummy seperti di bawah ini. Data dummy ditambahkan menjadi sebanyak 200.

  ```php
  <?php

  namespace Database\Seeders;

  use App\Models\Employee;
  use Illuminate\Database\Seeder;

  class EmployeeSeeder extends Seeder
  {
      /**
       * Run the database seeds.
       */
      public function run(): void
      {
          Employee::factory()->count(200)->create();
      }
  }
  ```

- Jalankan file seeder tersebut via **Artisan** seperti di bawah ini.

  ```
  php artisan db:seed --class=EmployeeSeeder
  ```

##### 1.1 Basic DataTables

- Install packages **Laravel DataTables** via **composer**

  ```
  composer require yajra/laravel-datatables-oracle
  ```

- Install aset-aset **DataTables** yang dibutuhkan via **NPM**

  ```
  npm install datatables.net-bs5 datatables.net-buttons-bs5
  ```

- Registrasikan aset-aset **DataTables** pada file **/resources/js/app.js** seperti yang terlihat di bawah ini.

  ```js
  import "./bootstrap";
  import "datatables.net-bs5";
  import "datatables.net-buttons-bs5";
  import.meta.glob(["../images/**"]);
  ```

- Registrasikan aset-aset **DataTables** pada file **/resources/sass/app.scss** seperti yang terlihat di bawah ini.

  ```php
  // Bootstrap
  @import 'bootstrap/scss/bootstrap';
  @import "bootstrap-icons/font/bootstrap-icons.css";

  // DataTables
  @import "datatables.net-bs5/css/dataTables.bootstrap5.css";
  @import "datatables.net-buttons-bs5/css/buttons.bootstrap5.css";
  ```

- Import **jQuery** pada file **/resources/js/bootstrap.js** seperti yang terlihat di bawah ini.

  ```js
  import "bootstrap";

  import axios from "axios";
  window.axios = axios;
  window.axios.defaults.headers.common["X-Requested-With"] = "XMLHttpRequest";

  import $ from "jquery";
  window.$ = $;
  ```

- Buka file **base layout** pada **/resources/views/layouts/app.blade.php** kemudian tambahkan blade directive **stack()** dengan nama **“scripts”** seperti di bawah ini.

  ```php
  <!doctype html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>{{ $pageTitle }}</title>
      @vite('resources/sass/app.scss')
    </head>
    <body>
      @include('layouts.nav')
      @yield('content')
      @vite('resources/js/app.js')
      @stack('scripts')
    </body>
  </html>
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian gunakan block blade directive **push()** untuk menambahkan kode program seperti di bawah ini.

  ```php
  @push('scripts')
      <script type="module">
          $(document).ready(function() {
              $('#employeeTable').DataTable();
          });
      </script>
  @endpush
  ```

- Berikan atribut id dengan nilai **“employeeTable”** pada elemen tag `<table>` seperti kode program di bawah ini.

  ```php
  <table class="table table-bordered table-hover table-striped mb-0 bg-white" id="employeeTable">
  ```

- Akses halaman **Employee List** via browser, jika berhasil akan muncul tampilan **DataTables** untuk data employee.

  {{< image src="images/blog/employee-datatables.png" caption="" alt="Employee DataTables" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

##### 1.2 Server-side Processing DataTables

- Buat Route baru pada **/routes/web.php** dengan URI **/getEmployees**, mengarahkan ke method **getData()** pada **EmployeeController** seperti kode program di bawah ini.

  ```php
  Route::get('getEmployees', [EmployeeController::class, 'getData'])->name('employees.getData');
  ```

- Buat method **getData()** pada **EmployeeController** seperti di bawah ini.

  ```php
  public function getData(Request $request)
  {
      $employees = Employee::with('position');

      if ($request->ajax()) {
          return datatables()->of($employees)
              ->addIndexColumn()
              ->addColumn('actions', function($employee) {
                  return view('employee.actions', compact('employee'));
              })
              ->toJson();
      }
  }
  ```

- Sesuaikan method **index()** pada **EmployeeController** seperti kode program di bawah ini. Query eloquent untuk mengambil data employee sudah tidak diperlukan lagi karna sudah digantikan proses nya pada method **getData()**.

  ```php
  public function index()
  {
      $pageTitle = 'Employee List';

      return view('employee.index', compact('pageTitle'));
  }
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian sesuaikan kode program pada bagian tag `<table>` nya seperti di bawah ini. Perulangan yang ada pada tag `<tbody>` dihapus karna data akan diload secara **asynchronous** dan ditangani oleh **DataTables**.

  ```html
  <table
    class="table table-bordered table-hover table-striped mb-0 bg-white"
    id="employeeTable"
  >
    <thead>
      <tr>
        <th>ID</th>
        <th>No.</th>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Email</th>
        <th>Age</th>
        <th>Position</th>
        <th></th>
      </tr>
    </thead>
  </table>
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian sesuaikan script **DataTables** nya seperti di bawah ini.

  ```php
  @push('scripts')
      <script type="module">
          $(document).ready(function() {
              $("#employeeTable").DataTable({
                  serverSide: true,
                  processing: true,
                  ajax: "/getEmployees",
                  columns: [
                      { data: "id", name: "id", visible: false },
                      { data: "DT_RowIndex", name: "DT_RowIndex", orderable: false, searchable: false },
                      { data: "firstname", name: "firstname" },
                      { data: "lastname", name: "lastname" },
                      { data: "email", name: "email" },
                      { data: "age", name: "age" },
                      { data: "position.name", name: "position.name" },
                      { data: "actions", name: "actions", orderable: false, searchable: false },
                  ],
                  order: [[0, "desc"]],
                  lengthMenu: [
                      [10, 25, 50, 100, -1],
                      [10, 25, 50, 100, "All"],
                  ],
              });
          });
      </script>
  @endpush
  ```

- Akses halaman Employee List via browser, jika berhasil akan muncul tampilan **DataTables** untuk data employee dengan teknik **server-side processing**.

  {{< image src="images/blog/employee-datatables.png" caption="" alt="Employee DataTables" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

#### 2. Penerapan Laravel Sweet Alert

- Install package **SweetAlert** via **composer**.

  ```
  composer require realrashid/sweet-alert
  ```

- Buka file base layout pada **/resources/views/layouts/app.blade.php** kemudian tambahkan blade directive **include()** untuk menerapkan package **SweetAlert** seperti di bawah ini. Letakkan setelah **vite()** dan sebelum **stack()**.

  ```php
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>{{ $pageTitle }}</title>
      @vite('resources/sass/app.scss')
  </head>
  <body>
      @include('layouts.nav')
      @yield('content')
      @vite('resources/js/app.js')
      @include('sweetalert::alert')
      @stack('scripts')
  </body>
  </html>
  ```

- **Publish** aset dari **SweetAlert** via **Artisan**.

  ```
  php artisan sweetalert:publish
  ```

- Buka **EmployeeController** dan letakkan **SweetAlert** pada method **store()**, **update()** dan **destroy()**.

  ```php
  use RealRashid\SweetAlert\Facades\Alert;

  public function store(Request $request)
  {
      ...

      Alert::success('Added Successfully', 'Employee Data Added Successfully.');

      return redirect()->route('employees.index');
  }
  ```

  ```php
  use RealRashid\SweetAlert\Facades\Alert;

  public function update(Request $request, string $id)
  {
      ...

      Alert::success('Changed Successfully', 'Employee Data Changed Successfully.');

      return redirect()->route('employees.index');
  }
  ```

  ```php
  use RealRashid\SweetAlert\Facades\Alert;

  public function destroy(string $id)
  {
      ...

      Alert::success('Deleted Successfully', 'Employee Data Deleted Successfully.');

      return redirect()->route('employees.index');
  }
  ```

- Buka file view pada **/resources/views/employee/actions.blade.php** kemudian sesuaikan kode program pada bagian tag `<button>`. Tambahkan class **“btn-delete”** dan atribut **“data-name”** seperti di bawah ini.

  ```html
  <form
    action="{{ route('employees.destroy', ['employee' => $employee->id]) }}"
    method="POST"
  >
    @csrf @method('delete')
    <button
      type="submit"
      class="btn btn-outline-dark btn-sm me-2 btn-delete"
      data-name="{{ $employee->firstname.' '.$employee->lastname }}"
    >
      <i class="bi-trash"></i>
    </button>
  </form>
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian sesuaikan kode program pada bagian tag `<table>`. Tambahkan class **“datatable”** seperti di bawah ini.

  ```php
  <table class="table table-bordered table-hover table-striped mb-0 bg-white datatable" id="employeeTable">
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian tambahkan kode program di bawah ini pada block blade directive **push()** seperti di bawah ini.

  ```php
  @push('scripts')
      <script type="module">
          $(document).ready(function() {

              ...

              $(".datatable").on("click", ".btn-delete", function (e) {
                  e.preventDefault();

                  var form = $(this).closest("form");
                  var name = $(this).data("name");

                  Swal.fire({
                      title: "Are you sure want to delete\n" + name + "?",
                      text: "You won't be able to revert this!",
                      icon: "warning",
                      showCancelButton: true,
                      confirmButtonClass: "bg-primary",
                      confirmButtonText: "Yes, delete it!",
                  }).then((result) => {
                      if (result.isConfirmed) {
                          form.submit();
                      }
                  });
              });
          });
      </script>
  @endpush
  ```

- Sesuaikan method **index()** pada **EmployeeController** seperti kode program di bawah ini. Panggil method **confirmDelete()** milik SweetAlert.

  ```php
  public function index()
  {
      $pageTitle = 'Employee List';

      confirmDelete();

      return view('employee.index', compact('pageTitle'));
  }
  ```

- Akses Fitur **Create**, **Edit** dan **Delete** Employee via browser, jika berhasil akan muncul tampilan popup window dari **SweetAlert** sebagai feedback sistem untuk proses yang sedang dilakukan oleh user. Contoh tampilannya seperti gambar di bawah ini.

  {{< image src="images/blog/employee-sweet-alert.png" caption="" alt="Employee Sweet Alert" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

#### 3. Penerapan Laravel Excel

- Install **Laravel Excel** via **Composer**.

  ```
  composer require maatwebsite/excel
  ```

- **Publish** file konfigurasi dengan menjalankan command script seperti di bawah ini.

  ```
  php artisan vendor:publish --provider="Maatwebsite\Excel\ExcelServiceProvider" --tag=config
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian buat button untuk fitur export data employee menjadi Excel. Sesuaikan bagian kode program seperti di bawah ini.

  ```php
  <div class="row mb-0">
      <div class="col-lg-9 col-xl-6">
          <h4 class="mb-3">{{ $pageTitle }}</h4>
      </div>
      <div class="col-lg-3 col-xl-6">
          <ul class="list-inline mb-0 float-end">
              <li class="list-inline-item">
                  <a href="{{ route('employees.exportExcel') }}" class="btn btn-outline-success">
                      <i class="bi bi-download me-1"></i> to Excel
                  </a>
              </li>
              <li class="list-inline-item">|</li>
              <li class="list-inline-item">
                  <a href="{{ route('employees.create') }}" class="btn btn-primary">
                      <i class="bi bi-plus-circle me-1"></i> Create Employee
                  </a>
              </li>
          </ul>
      </div>
  </div>
  ```

- Buat Route baru pada **/routes/web.php** dengan URI **/exportExcel**, mengarahkan ke method **exportExcel()** pada **EmployeeController** seperti kode program di bawah ini.

  ```php
  Route::get('exportExcel', [EmployeeController::class, 'exportExcel'])->name('employees.exportExcel');
  ```

- Buat file **Export** dengan nama **EmployeesExport** via Artisan seperti di bawah ini. Diarahkan kepada model **Employee**.

  ```
  php artisan make:export EmployeesExport --model=Employee
  ```

- Edit atau ubah file **EmployeesExport**, sesuaikan seperti di bawah ini.

  ```php
  <?php

  namespace App\Exports;

  use App\Models\Employee;
  use Illuminate\Contracts\View\View;
  use Maatwebsite\Excel\Concerns\FromView;
  use Maatwebsite\Excel\Concerns\ShouldAutoSize;
  use Maatwebsite\Excel\Concerns\WithStyles;
  use PhpOffice\PhpSpreadsheet\Worksheet\Worksheet;

  class EmployeesExport implements FromView, WithStyles, ShouldAutoSize
  {
      public function styles(Worksheet $sheet)
      {
          return [
              1 => ['font' => ['bold' => true]],
          ];
      }

      public function view(): View
      {
          return view('employee.export_excel', [
              'employees' => Employee::all()
          ]);
      }
  }
  ```

- Buat file view baru dengan nama **export_excel.blade.php** pada direktori **/resources/views/employee**. Isi dengan kode program seperti di bawah ini.

  ```php
  <table>
      <thead>
          <tr>
              <th>No.</th>
              <th>First Name</th>
              <th>Last Name</th>
              <th>Email</th>
              <th>Age</th>
              <th>Position</th>
          </tr>
      </thead>
      <tbody>
          @foreach ($employees as $index => $employee)
              <tr>
                  <td>{{ $index + 1 }}</td>
                  <td>{{ $employee->firstname }}</td>
                  <td>{{ $employee->lastname }}</td>
                  <td>{{ $employee->email }}</td>
                  <td>{{ $employee->age }}</td>
                  <td>{{ $employee->position->name }}</td>
              </tr>
          @endforeach
      </tbody>
  </table>
  ```

- Buat method **exportExcel()** pada EmployeeController seperti kode program di bawah ini.

  ```php
  use Maatwebsite\Excel\Facades\Excel;
  use App\Exports\EmployeesExport;

  public function exportExcel()
  {
      return Excel::download(new EmployeesExport, 'employees.xlsx');
  }
  ```

- Akses halaman Employee List via browser, kemudian klik button **“to Excel”** untuk meng-export data employee ke dalam bentuk file Excel.

  {{< image src="images/blog/employee-export.png" caption="" alt="Employee Export " height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

  {{< image src="images/blog/employee-export-excel.png" caption="" alt="Employee Export Excel" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

#### 4. Penerapan Laravel DomPdf

- Install Laravel **DomPdf** via Composer.

  ```
  composer require barryvdh/laravel-dompdf
  ```

- **Publish** file konfigurasi dengan menjalankan command script seperti di bawah ini.

  ```
  php artisan vendor:publish --provider="Barryvdh\DomPDF\ServiceProvider"
  ```

- Buka file view pada **/resources/views/employee/index.blade.php** kemudian buat button untuk fitur export data employee menjadi PDF. Sesuaikan bagian kode program seperti di bawah ini.

  ```php
  <div class="row mb-0">
      <div class="col-lg-9 col-xl-6">
          <h4 class="mb-3">{{ $pageTitle }}</h4>
      </div>
      <div class="col-lg-3 col-xl-6">
          <ul class="list-inline mb-0 float-end">
              <li class="list-inline-item">
                  <a href="{{ route('employees.exportExcel') }}" class="btn btn-outline-success">
                      <i class="bi bi-download me-1"></i> to Excel
                  </a>
              </li>
              <li class="list-inline-item">
                  <a href="{{ route('employees.exportPdf') }}" class="btn btn-outline-danger">
                      <i class="bi bi-download me-1"></i> to PDF
                  </a>
              </li>
              <li class="list-inline-item">|</li>
              <li class="list-inline-item">
                  <a href="{{ route('employees.create') }}" class="btn btn-primary">
                      <i class="bi bi-plus-circle me-1"></i> Create Employee
                  </a>
              </li>
          </ul>
      </div>
  </div>
  ```

- Buat Route baru pada **/routes/web.php** dengan URI **/exportPdf**, mengarahkan ke method **exportPdf()** pada **EmployeeController** seperti kode program di bawah ini.

  ```php
  Route::get('exportPdf', [EmployeeController::class, 'exportPdf'])->name('employees.exportPdf');
  ```

- Buat file view baru dengan nama **export_pdf.blade.php** pada direktori **/resources/views/employee**. Isi dengan kode program seperti di bawah ini.

  ```php
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Employee List</title>
      <style>
          html {
              font-size: 12px;
          }

          .table {
              border-collapse: collapse !important;
              width: 100%;
          }

          .table-bordered th,
          .table-bordered td {
              padding: 0.5rem;
              border: 1px solid black !important;
          }
      </style>
  </head>
  <body>
      <h1>Employee List</h1>
      <table class="table table-bordered">
          <thead>
              <tr>
                  <th>No.</th>
                  <th>First Name</th>
                  <th>Last Name</th>
                  <th>Email</th>
                  <th>Age</th>
                  <th>Position</th>
              </tr>
          </thead>
          <tbody>
              @foreach ($employees as $index => $employee)
                  <tr>
                      <td align="center">{{ $index + 1 }}</td>
                      <td>{{ $employee->firstname }}</td>
                      <td>{{ $employee->lastname }}</td>
                      <td>{{ $employee->email }}</td>
                      <td align="center">{{ $employee->age }}</td>
                      <td>{{ $employee->position->name }}</td>
                  </tr>
              @endforeach
          </tbody>
      </table>
  </body>
  </html>
  ```

- Buat method **exportPdf()** pada **EmployeeController** seperti kode program di bawah ini.

  ```php
  use PDF;
  use App\Models\Employee;

  public function exportPdf()
  {
      $employees = Employee::all();

      $pdf = PDF::loadView('employee.export_pdf', compact('employees'));

      return $pdf->download('employees.pdf');
  }
  ```

- Akses halaman Employee List via browser, kemudian klik button **“to PDF”** untuk meng-export data employee ke dalam bentuk file PDF.

  {{< image src="images/blog/employee-export.png" caption="" alt="Employee Export " height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

  {{< image src="images/blog/employee-export-pdf.png" caption="" alt="Employee Export PDF" height="" width="900" position="left" command="fit" option="q100" class="img-fluid" title=""  webp="true" >}}

#### 5. Memindahkan dan Memisahkan Kode Javascript

- Kita dapat memindahkan kode javascript yang ada pada file view kita dan kita kelompokkan dengan file-file javascript pada project laravel kita.
- Buat file **employee.js** pada direktori **/resources/js/**. Hapus blok kode yang ada pada blade directive **push()** pada file **/resources/views/index.blade.php** dan pindahkan kode javascript yang ada ke file **employee.js**. Kode program pada file **index.blade.php** akan menjadi seperti di bawah ini.

  ```php
  @extends('layouts.app')

  @section('content')
      <div class="container mt-4">
          <div class="row mb-0">
              <div class="col-lg-9 col-xl-6">
                  <h4 class="mb-3">{{ $pageTitle }}</h4>
              </div>
              <div class="col-lg-3 col-xl-6">
                  <ul class="list-inline mb-0 float-end">
                      <li class="list-inline-item">
                          <a href="{{ route('employees.exportExcel') }}" class="btn btn-outline-success">
                              <i class="bi bi-download me-1"></i> to Excel
                          </a>
                      </li>
                      <li class="list-inline-item">
                          <a href="{{ route('employees.exportPdf') }}" class="btn btn-outline-danger">
                              <i class="bi bi-download me-1"></i> to PDF
                          </a>
                      </li>
                      <li class="list-inline-item">|</li>
                      <li class="list-inline-item">
                          <a href="{{ route('employees.create') }}" class="btn btn-primary">
                              <i class="bi bi-plus-circle me-1"></i> Create Employee
                          </a>
                      </li>
                  </ul>
              </div>
          </div>
          <hr>
          <div class="table-responsive border p-3 rounded-3 mb-5">
              <table class="table table-bordered table-hover table-striped mb-0 bg-white datatable" id="employeeTable">
                  <thead>
                      <tr>
                          <th>ID</th>
                          <th>No.</th>
                          <th>First Name</th>
                          <th>Last Name</th>
                          <th>Email</th>
                          <th>Age</th>
                          <th>Position</th>
                          <th></th>
                      </tr>
                  </thead>
              </table>
          </div>
      </div>
  @endsection
  ```

- Kode program pada file **employee.js** akan menjadi seperti di bawah ini.

  ```js
  $(function () {
    $("#employeeTable").DataTable({
      serverSide: true,
      processing: true,
      ajax: "/getEmployees",
      columns: [
        { data: "id", name: "id", visible: false },
        {
          data: "DT_RowIndex",
          name: "DT_RowIndex",
          orderable: false,
          searchable: false,
        },
        { data: "firstname", name: "firstname" },
        { data: "lastname", name: "lastname" },
        { data: "email", name: "email" },
        { data: "age", name: "age" },
        { data: "position.name", name: "position.name" },
        {
          data: "actions",
          name: "actions",
          orderable: false,
          searchable: false,
        },
      ],
      order: [[0, "desc"]],
      lengthMenu: [
        [10, 25, 50, 100, -1],
        [10, 25, 50, 100, "All"],
      ],
    });

    $(".datatable").on("click", ".btn-delete", function (e) {
      e.preventDefault();

      var form = $(this).closest("form");
      var name = $(this).data("name");

      Swal.fire({
        title: "Are you sure want to delete\n" + name + "?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonClass: "bg-primary",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        if (result.isConfirmed) {
          form.submit();
        }
      });
    });
  });
  ```

- Buka file **/resources/js/app.js** kemudian import file **employee.js**.

  ```js
  import "./bootstrap";
  import "datatables.net-bs5";
  import "datatables.net-buttons-bs5";
  import "./employee";
  import.meta.glob(["../images/**"]);
  ```

- Akses halaman Employee List via browser, jika berhasil seluruh fitur tetap berjalan dengan baik.

#### 6. Tugas

- **Praktekkan** dan **analisis** seluruh poin dan latihan yang ada di atas secara **INDIVIDU**.

<hr>

{{< notice "note" >}}

##### Artikel ini merupakan bagian dari:

{{< button label="Kumpulan Modul Belajar Dasar Pemrograman Framework Laravel" link="/blog/kumpulan-modul-pemrograman-framework-tahap-dasar/" style="outline" class="btn-sm w-full mb-3" >}}
{{< /notice >}}
