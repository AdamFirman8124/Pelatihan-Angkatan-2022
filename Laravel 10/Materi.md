# Instalasi Laravel 10

Untuk menginstall Laravel 10, kamu harus menginstall aplikasi in iterlebih dahulu:

Composer-Setup.exe

![alt text](<WhatsApp Image 2024-04-04 at 23.33.05_34312928.jpg>)
![alt text](<WhatsApp Image 2024-04-04 at 23.33.21_98587363.jpg>)
![alt text](<WhatsApp Image 2024-04-04 at 23.33.35_a8ada862.jpg>)
![alt text](<WhatsApp Image 2024-04-04 at 23.33.45_aab02019.jpg>)

Setelah selesai instalasi, buka Command Prompt lalu ketikkan kode ini:

```
cd C:\xampp\htdocs
```
Lalu jalankan kode ini:
```
composer create-project laravel/laravel:^10.0 example-app
```
atau
```
composer create-project laravel/laravel="10.2" example-app
```
atau juga bisa
```
laravel new example-app
```

"example-app" pada kode diatas merupakan nama Folder yang akan dibuat, nama foldernya bisa apa saja selama kata nya tidak terpisah oleh space/spasi

![alt text](<WhatsApp Image 2024-04-04 at 23.35.47_5b455dee.jpg>)

Apabila terjadi error seperti dibawah ini:
![alt text](image-3.png)

Buka file Explorer, navigasi ke C: >> xampp >> php, lalu buka file php.ini.

![alt text](image-8.png)

Ketik Ctrl + F untuk membuka Find Box.

![alt text](image-1.png)

Hapus titik koma (;) pada bagian kanannya "extension=zip", Sehingga jadi seperti ini:

![alt text](image-2.png)

Kemudian Save file lalu close.

Setelah itu, jalankan kembali kode ini di Command Prompt:

```
cd C:\xampp\htdocs
```
Lalu jalankan kode ini:
```
composer create-project laravel/laravel:^10.0 example-app
```
atau
```
composer create-project laravel/laravel="10.2" example-app
```
atau juga bisa
```
laravel new example-app
```

Setelah kode di jalankan, maka tampilan Command Prompt akan menjadi seperti ini:

![alt text](image-4.png)

Lalu tunggu proses hingga mencapai akhir.

![alt text](image-5.png)

Setelah itu, masukkan kode ini di Command Prompt:

```
cd example-app
```
Ingat untuk "example-app" itu hanya sebuah nama folder jadi diingat ingat sebelumnya nama folder kalian itu apa
```
php artisan serve
```

Tunggu hingga tampilan menjadi seperti ini:

![alt text](image-6.png)

Lalu copy-paste "http://127.0.0.1:8000" dari Command Prompt ke browser.

![alt text](image-7.png)

![alt text](image-11.png)

[LANJUT LOGIN DAN REGISTER](https://)
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