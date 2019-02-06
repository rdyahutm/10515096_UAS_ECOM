# Aplikasi Pendataan Siswa SMKN 1 San Francisco
Aplikasi Pendataan Siswa SMKN 1 San Francisco merupakan aplikasi yang berisi data siswa dan data kelas dan terdiri dari beberapa field dan dibuat dengan menggunakan framework Laravel. Aplikasi Pendataan Siswa SMKN 1 San Francisco dibuat untuk memenuhi tugas Ujian Akhir Semester pada mata kuliah E-Commerce Lanjut.

# Running Project Aplikasi Pendataan Siswa SMKN 1 San Francisco
Cara running project ini dengan mengaktifkan Apache dan MySql pada xampp control panel dan mengcopy project ke folder htdocs yang berada di folder xampp, kemudian buka browser dengan mengakses http://localhost/10515096_UAS_ECOM/public

# Menggunakan Template AdminLTE 2.3.11 di Framework Laravel
Untuk mengintegrasikan Laravel dengan Template AdminLTE 2.3.11, download terlebih dahulu Template AdminLTE 2.3.11 dengan mengakses link https://codeload.github.com/almasaeed2010/AdminLTE/zip/v2.3.11. Buat folder assets di folder public, copy folder dist, bootstrap dan plugin dari template AdminLTE 2.3.11 ke folder assets. Selanjutnya, buat folder templates pada folder resources/views dan buat folder kelas di folder resources/views. Di folder kelas, buat file index.blade.php dan open file blank.html di folder AdminLTE-2.3.11. Kemudian, copy Line 1 s.d 391, paste di file templates/header.blade.php, copy line 392 s.d 432, paste di file index.blade.php, dan copy line 433 s.d 654, paste di file templates/footer.blade.php Pada file templates/header.blade.php baris 21, tambahkan: @stack digunakan untuk menyimpan potongan script dari hasil @push Masih pada file templates/header.blade.php baris terakhir, tambahkan 2 baris sintaks berikut:
1. @yield digunakan untuk menyimpan potongan script dari hasil @section
2. @include digunakan untuk menyisipkan file lain Buka file kelas/index.blade.php, tambahkan pada baris pertama kodingan ini : @extends('templates/header') @section('content')' Dan pada baris terakhir tambahkan : @endsection Pada file templates/header.blade.php dan templates/footer.blade.php lakukan Find and Replace untuk mengganti semua link asset ke folder asset yang sudah kita copy kan tadi. Find : ../.. | Replace : {{asset('assets')}} Buka Command Prompt (dengan cara tekan Shift + Klik Kanan di sembarang tempat, pilih Open windows command windows here) di folder belajarLaravel, buat Controller baru menggunakan artisan dengan syntax berikut: php artisan make:controller Kelas Controller Buka file app/Http/Controllers/KelasController.php, tambahkan sintaks berikut ke dalam kodingan class KelasController : public function index() { return view('kelas/index'); } Selanjutnya buka file routes/web.php, tambahkan kodingan routes yang sudah ada menjadi : Route::get('/', 'KelasController@index); Akses web kalian dengan alamat url sesuai dengan yang sudah di install di awal maka Template AdminLTE 2.3.11 telah terintegrasi ke Laravel

# Membuat CRUD
Pertama, akan dijelaskan bagaimana read data dari database. Read data pada framework Laravel dan menggunakan fitur Eloquent, Eloquent  adalah fitur dari Laravel yang digunakan untuk memanggil data dari database dalam bentuk Entity Object, tanpa syntax MySQL sama sekali. Dalam membuat database MySQL nya pun akan digunakan fitur migration dari Laravel. Tutorial Make Simple CRUD (Create, Read, Updata, dan Delete) dapat di akses di link berikut : https://drive.google.com/file/d/1AmexPu9OEQEz1cHfvVOHHIx3-47ml-Jm/view

# Eloquent Relationship
Selanjutnya bagaimana membuat relasi dua tabel menggunakan fitur dari Laravel, yaitu Relationship Eloquent. Pertama, buatlah satu tabel lagi seperti langkah CRUD di atas dengan nama tabel yaitu table_siswa dengan field nis, nama_lengkap, jenis_kelamin, alamat, no_telp, dan id_kelas sebagai foreign key dari tabel kelas ke tabel siswa ini.

Pada file resources/views/siswa/index.blade.php ada sedikit kodingan yang harus di tambahkan : @$row->kelas->nama_kelas digunakan untuk memanggil data relasi eloquent. Pada Model Siswa kita sudah membuat fungsi kelas, nah disini @$row->kelas adalah memanggil fungsi tersebut (Fungsi Relasi Eloquent). Setelah @$row->kelas kita memiliki seluruh atribut Data Kelas, tinggal panggil lagi saja atribut Data Kelas nya, misal @$row->kelas->nama_kelas.

Pada form select diatas, terdapat @foreach, digunakan untuk me-looping data. Data yang diambil adalah \App\Kelas::all() yang artinya mengambil semua data pada tabel t_kelas. Kemudian pada sintaks berikutnya memasukan id_kelas sebagai value, dan nama_kelas sebagai display teks nya. {{ @$result->id_kelas == $kelas->id_kelas ? ‘selected’ : ‘’ }} Sintaks tersebut digunakan pada saat form mode edit, untuk menandakan status selected pada combo box sesuai dengan data yang akan diedit.

Running Aplikasi Pendanaan SMKN 1 San Francisco, kemudian coba lakukan input data maka relasi tabel antara tabel kelas dan tabel siswa sudah berjalan.

# Login
Selanjutnya akan dijelaskan mengenai Authentikasi User. Kita akan menggunakan fitur bawaan Laravel dan sedikit modifikasi sistemnya. Langkah pertama kita buat dulu tampilan login nya. Buka AdminLTE-2.3.11/pages/examples/login.html. Buat file baru di resources/views/login.blade.php Copy isi dari login.html ke login.blade.php Sama seperti file template pada pembahasan awal, Find & Replace. Find : ../.. | Replace : {{asset('assets')}} Modifikasi tambah feedback, form action, field name dan tambahkan token (Line 36, 38, 39, 42, 46 dan 53) 36. @include('tempaltes.feedback') 38.

39. {{csrf_field() }} 42. 46. 53. <input type="checkbox name="remember"> Remember Me
Kemudian edit routes/web.php

di atas kodingan route::get tambahkan : Route::auth(); dan Route::group(['middleware'=>'auth'], function() { });

Route::auth() digunakan untuk menciptakan routes yang berhubungan dengan authentikasi (Bawaan Laravel)
Route::group digunakan untuk membuat grup route dan mengimplemntasikan atribut/rule khusus kepada grup tsb. Pada contoh diatas mengimplementasikan middleware auth pada seluruh route yang ada pada grup tsb. Jadi untuk mengakses route yang ada di dalam grup tersebut harus login terlebih dahulu. Contohnya mengakses alamat laravel yang sudah di install tidak akan terbuka jika user belum login.
Selanjutnya membuat migration table login yaitu buka cmd kemudian ketikan perintah berikut ini : php artisan make:migration create_table_user Edit file migration tersebut tambahkan kodingan : Schema::create('t_login', function (Blueprint $table) { $table->increments('id_login'); $table->string('nama_user', 100); $table->string('username', 100); $table->string('password', 150); $table->rememberToken(); $table->timestamps(); });

Kemudian lakukan migrate dengan ketik perintah berikut di cmd : php artisan migrate Table t_login akan tercipta otomatis. Lihat hasilnya di phpMyAdmin Untuk Login, model nya cukup berbeda. Laravel sudah menyediakan Model nya dengan nama User pada folder app, tinggal kita modifikasi saja. use Notifiable; protected $table = 't_login'; public $primaryKey = 'id_login'; protected $fillable = [ 'nama_user', 'username', 'password' ]; Untuk membuat user adminnya, kita coba lewat database seeder ya. Langkah-langkahnya adalah sebagai berikut:

Buka cmd dan ketikan perintah : php artisan make:seed LoginSeeder
Edit file Seeder tersebut seperti ini : public function run() { $user = new \App\User; $user->nama_user = 'Admin'; $user->username = 'admin'; $user->password = bcrypt('admin'); $user->save(); }
Kemudian jangan lupa edit juga database/seeds/DatabaseSeeder.php yaitu mematikan fungsi yang lain kemudian menambahkan fungsi : $this->call(LoginSeeder::class);
Kemudian ketikan ini pada cmd : php artisan db:seed
Selanjutnya modifikasi Login Controller yaitu Modifikasi file app/Http/Controllers/Auth/LoginController.php (Line 28, Line 40-48) 28. protected $redirectTo = '/'; 40 - 46 : public function showLoginForm(){ return view('login'); } public function username(){ return 'username'; }

Coba buka web browser kamu dan akses alamat installan laravel maka akan otomatis muncul halaman login. Masukkan username admin dan password admin, maka akan langsung ke halaman utama. Jika username/password salah akan muncul feedback pesan error.

Untuk membuat tombol logout maka Ikuti langkah berikut:

Buka file resources/views/templates/header.blade.php
Edit line 173, menjadi :
{{ csrf_field() }} Sign Out
