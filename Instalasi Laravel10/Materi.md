# Instalasi Laravel 10

Untuk menginstall Laravel 10, kamu harus menginstall aplikasi in iterlebih dahulu:

Composer-Setup.exe

![alt text](<image1.jpg>)
![alt text](<image2.jpg>)
![alt text](<image3.jpg>)
![alt text](<image4.jpg>)

Setelah selesai instalasi, buka Command Prompt lalu ketikkan kode ini:

```
cd C:\xampp\htdocs
```
Lalu jalankan kode ini:
```
composer create-project laravel/laravel:^10.0 example-app
```
"example-app" pada kode diatas merupakan nama Folder yang akan dibuat, nama foldernya bisa apa saja selama kata nya tidak terpisah oleh space/spasi

![alt text](<WhatsApp Image 2024-04-04 at 23.35.47_5b455dee.jpg>)

Apabila terjadi error seperti dibawah ini:
![alt text](image.png)

Buka file Explorer, navigasi ke C: >> xampp >> php, lalu buka file php.ini.

![alt text](image-3.png)

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

Setelah kode di jalankan, maka tampilan Command Prompt akan menjadi seperti ini:

![alt text](image-4.png)

Lalu tunggu proses hingga mencapai akhir.

![alt text](image-5.png)

Setelah itu, masukkan kode ini di Command Prompt:

```
cd example-app
```
```
php artisan serve
```

Tunggu hingga tampilan menjadi seperti ini:

![alt text](image-6.png)

Lalu copy-paste "http://127.0.0.1:8000" dari Command Prompt ke browser.

![alt text](image-7.png)

![alt text](image-11.png)
