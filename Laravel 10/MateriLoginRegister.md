# Sekarang kita mencoba untuk membuat Login dan Register di Laravel 10

Step 1: Buka Terminal baru lalu masuk ke bagian dimana file laravel kalian berada

Contoh:
```
cd C:\xampp\htdocs\example-app
```

Setelah itu masukkan kode dibawah ini untuk Instalasi Modul Laravel UI:
```
composer require laravel/ui
```

Setelah itu lanjutkan modul authentikasi, kodenya dibawah ini:
```
php artisan ui bootstrap --auth
```

Jika berhasil kode akan seperti dibawah ini:
![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-9.png)

Jika gagal, coba kalian cek di terminal dengan scroll ke atas apakah ada error atau tidak, biasanya terdapat error seperti dibawah ini.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-17.png)

Cara mengatasinya dengan membuka **package.json** di file laravel kalian melalui vscode lalu tambahkan kode ini dibawah "private":
```
"type": "module",
```

Sehingga kode keseluruhan menjadi seperti berikut:

```
{
    "private": true,
    "type": "module",
    "scripts": {
        "dev": "vite",
        "build": "vite build"
    },
    "devDependencies": {
        "@tailwindcss/forms": "^0.5.2",
        "alpinejs": "^3.4.2",
        "autoprefixer": "^10.4.2",
        "axios": "^1.1.2",
        "laravel-vite-plugin": "^0.7.2",
        "postcss": "^8.4.31",
        "tailwindcss": "^3.1.0",
        "vite": "^4.0.0"
    }
}
```

ketika sudah dipastikan kode seperti itu maka jalankan kembali kode ini dan tunggu sampai berhasil.

```
php artisan ui bootstrap --auth
```

Setelah itu buka terminal lalu jalankan perintah.

```
npm install
```

Setelah itu:

```
npm run dev
```

Nanti akan muncul seperti ini di terminal

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-11.png)

Buka lagi sebuah terminal baru, dan jalankan server laravel. 
```
php artisan serve
```
(npm run dev harus tetap berjalan di terminal lain) setelah itu hasil pada browser, tampilan awal laravel saat ini sudah dilengkapi dengan **login** dan **register**.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-12.png)

Sampai sesi ini, antarmuka sudah jalan. Pada windows, kedua perintah harus dijalankan. (npm run dev) dan (php artisan serve).

# Jika terdapat error dibagian SQL nya contoh seperti ini:

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-18.png)

Maka solusi yang bisa kita gunakan adalah menggunakan kode dibawah ini:

Step 1: Cari file bernama **AppServiceProvider.php**

Step 2: Tambahkan kode ini di dalam file 
```
use Illuminate\Support\Facades\Schema;
```
Step 3: Tambahkan juga kode ini didalam file
```
public function boot()
{
    Schema::defaultStringLength(191);
}
```

Sehingga kode keseluruhan menjadi:
```
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Schema;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        Schema::defaultStringLength(191);
    }
}
```

Step 4: cari file bernama **.env**. Dan kosongkan password jika koneksi ke database tidak menggunakan password, atau isikan sesuai password yang telah di atur pada saat instalasi XAMPP.



Step 5: buka aplikasi xampp lalu start apache dan MySQL seperti ini:
![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-19.png)

Pastikan **DB_PORT** sesuai dengan yang ada di aplikasi xampp

Step 6: Buka website lalu ketik **localhost** lalu klik bagian **phpMyAdmin**

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-20.png)

Step 7: Setelah muncul seperti ini.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-21.png)

Website akan berubah seperti ini

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-22.png)

Lalu kalian bikin **Database name** sesuai dengan yang ada di file **.env** kalian bisa cek di **DB_DATABASE**

Step 8: Ketika sudah dipastikan nama database nya benar dan portnya benar kalian bisa mengulang kode yang ditaruh di terminal tadi.

```
php artisan migrate:fresh
```
atau
```
php artisan migrate
```

Proses ini melakukan migrasi tabel users dan beberapa table lain yang terdapat pada folder.

Ketika berhasil akan muncul **done** seperti ini
![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-23.png)

Setelah itu kembali ke terminal dan mengaktifkan (php artisan serve) dan (npm run dev). Ketika sudah muncul link nya yang berada di (php artisan serve). kalian bisa klik link tersebut dan menuju kesebuah website. Ketika sudah muncul website nya, kalian bisa klik register disebelah kanan atas dan silahkan memasukkan username, password, dan email aktif kalian. Jika sudah berhasil maka akan muncul tulisan seperti ini:

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-24.png)

# Meninjau Model

1.Buka file app\Models\User.php
```
namespace App\Models;
// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

    /**
    * The attributes that are mass assignable.
    * @var array<int, string>
    */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
    * The attributes that should be hidden for serialization.
    * @var array<int, string>
    */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
    * The attributes that should be cast.
    * @var array<string, string>
    */
    protected $casts = [
        'email_verified_at' => 'datetime',
        'password' => 'hashed',
    ];
}
```
2. Ada tiga assigment properti pada model User. ketiganya adalah

- protected $fillable. Mendifinisikan kolom yang boleh diisi hanya name, email dan password pada saat melakukan Insert data. Fields atau atribut lain akan dibuat otomatis oleh sistem.

- protected $hidden. Mendefinisikan field atau kolom yang tidak tertampil seandainya diakukan pemanggilan data untuk semua kolom menggunakan fungsi User::all().

- protected $casts. Mengkonversi nilai tipe data. Dalam hal ini data dari field email_verified_at diubah menjadi datetime, dan password diubah menjadi terenkripsi. 

# Meninjau View 

1. Buka folder resources\views

2. Route dengan alamat url / mengarahkan respon ke view welcome.blade.php.

3. Kemudian url /home mengatur menuju controller HomeController.php. Hanya saja, fungsi-fungsi pada HomeController.php harus melewati proses authentikasi middleware('auth') yang didefinisikan pada fungsi __construct() atau harus login. Itulah mengapa ketika kita mencoba akses langsung ke /home selalui dilempar ke halaman view(login).

4. Pada views juga telah dibuatkan layout app.blade.php sebagai template utama, yang di extend oleh view('welcome') dan view('home').

[TIP] Buka kembali tutorial tentang layout.

5. Pada folder auth, terdapat file view login.blade.php, register.blade.php dan verify.blade.php. Anda tidak perlu melakukan modifikasi pada bagian ini.

6. Di dalam folder auth ada lagi folder passwords yang berisi beberapa file view.

[LANJUT MENAMBAHKAN DATA USER](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/AddDataUser.md)