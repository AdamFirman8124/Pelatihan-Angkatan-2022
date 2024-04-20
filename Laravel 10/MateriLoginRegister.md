
# Sekarang kita mencoba untuk membuat Login dan Register di Laravel 10

Step 1: Buka kembali website Laravel lalu cari Starter Kits lalu Breeze and Blade

![alt text](image-9.png)

Setelah itu kita liat di sebelah terdapat tulisan Breeze and Blade lalu kita klick

![alt text](image-10.png)

Nanti akan langsung mengarahkan ke sini

![alt text](image-12.png)

Sekarang kalian buka visual studio code kalian lalu buka file Laravel yang tadi kalian buat lalu buka terminal dengan shortcut (ctrl + `) atau kalian bisa mencari di pojok atas kiri tulisan **Terminal**.

Setelah itu kalian copy ini dan paste ke dalam terminal terlebih dahulu:

```
composer require laravel/breeze --dev
```

Setelah proses selesai, kalian copy dan paste lagi kode yang berada di tag Breeze dan Blade. **Harap memasukkan kode nya 1 1 tidak semua**

```
php artisan breeze:install
```

Ketika muncul kata-kata seperti ini pilih **0**.
![alt text](image-13.png)

Jika kalian muncul seperti ini pilih **blade**.
![alt text](image-14.png)

Silahkan tunggu sampai proses selesai, ketika muncul seperti ini tekan **Enter** saja.
![alt text](image-15.png)

Ketika sudah selesai dan muncul seperti ini pilih **0** dan tunggu sampai selesai.
![alt text](image-16.png)

Coba kalian cek di terminal dengan scroll ke atas apakah ada error atau tidak, biasanya terdapat error seperti dibawah ini.
![alt text](image-17.png)

Cara mengatasinya dengan membuka **package.json** lalu tambahkan kode ini dibawah "private":

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
php artisan breeze:install
```

Ketika sudah memastikan kode tidak ada error di terminal maka bisa langsung lanjut kode yang dibawah ini:

```
php artisan migrate
```

Biasanya terdapat error dibagian SQL nya contoh seperti ini:
![alt text](image-18.png)

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
    Schema::defaultStringLength(191)
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

Step 4: cari file bernama **.env**

Step 5: buka aplikasi xampp lalu start apache dan MySQL seperti ini:
![alt text](image-19.png)

Pastikan **DB_PORT** sesuai dengan yang ada di aplikasi xampp

Step 6: Buka website lalu ketik **localhost** lalu klik bagian **phpMyAdmin**
![alt text](image-20.png)

Step 7: Setelah muncul seperti ini.
![alt text](image-21.png)

Website akan berubah seperti ini
![alt text](image-22.png)

Lalu kalian bikin **Database name** sesuai dengan yang ada di file **.env** kalian bisa cek di **DB_DATABASE**

Step 8: Ketika sudah dipastikan nama database nya benar dan portnya benar kalian bisa mengulang kode yang ditaruh di terminal tadi.

```
php artisan migrate
```

Ketika berhasil akan muncul **done** seperti ini
![alt text](image-23.png)

Pastikan kode sebelumnya bisa berjalan dengan benar, ketika sudah kita lanjut 2 kode dibawah ini.

```
npm install
```

```
npm run dev
```

Ketika sudah muncul link yang tersedia dari terminal kita buat terminal baru dengan dengan klik logo **+** di sebelah kanan lalu gunakan kode dibawah ini.

```
php artisan serve
```

Ketika sudah muncul link nya kalian bisa klik link tersebut dan menuju kesebuah website. Ketika sudah muncul website nya, kalian bisa klik register disebelah kanan atas dan silahkan memasukkan username, password, dan email aktif kalian. Jika sudah berhasil maka akan muncul tulisan seperti ini:

![alt text](image-24.png)

Tunggu update selanjutnya ya :D

[def]: Pemateri/