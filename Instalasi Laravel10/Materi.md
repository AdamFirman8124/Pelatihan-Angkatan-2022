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
"example-app" pada kode diatas merupakan nama Folder yang akan dibuat, nama foldernya bisa apa saja selama kata nya tidak terpisah oleh space/spasi

![alt text](<WhatsApp Image 2024-04-04 at 23.35.47_5b455dee.jpg>)

Apabila terjadi error seperti dibawah ini:
![alt text](image.png)

maka buka 
ketik
![alt text](image-1.png)
Hapus titik koma (;) pada bagian kanannya "extension=zip", sehingga jadi seperti ini:
![alt text](image-2.png)

### 2 Tambahkan dua kotak untuk mereka menulis nama pengguna dan kata sandi
```html
<label for="username">Username:</label>
<input type="text" id="username" name="username"><br><br>

<label for="password">Password:</label>
<input type="password" id="password" name="password"><br><br>
```

### 3 Tambahkan tombol "Login" yang ketika ditekan akan mengirimkan informasi tersebut
```html
<input type="submit" value="Login">
```

Jadi ketika menggabungkan semuanya, hasil lengkapnya seperti ini:
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Login Page</title>
</head>
<body>

<h1>Login</h1>

<form action="submit.php" method="POST">
    <label for="username">Username:</label><br>
    <input type="text" id="username" name="username"><br><br>

    <label for="password">Password:</label><br>
    <input type="password" id="password" name="password"><br><br>

    <input type="submit" value="Login">
</form>

</body>
</html>
```
Dan hasil akhirnya akan menjadi seperti ini
![Gambar Login Page](image.png)

Dalam contoh di atas, saat tombol "Login" ditekan, informasi yang dimasukkan oleh pengguna (nama pengguna dan kata sandi) akan dikirimkan ke submit.php untuk diproses lebih lanjut. submit.php adalah file PHP yang harus kamu buat untuk menangani data yang dikirimkan dari formulir login ini.

Nah jadi gimana? udah bisa bikin halaman loginnya? tapi bosenin banget kan ya? sekarang kita coba tambahkan cssnya. [LANJUT CSS :D](../CSS/materi.md)

