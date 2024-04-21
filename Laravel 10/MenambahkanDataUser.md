# Menambahkan Menu Data User

1. Buka file layout **app.blade.php** tambahkan menu Data User yang tampil jika pengguna sudah login. Blok program di antara @auth ... @endauth akan ditampilkan jika pengguna sudah login. Blok program di antara @guest ... @endguest akan ditampilkan jika pengguna belum login.

```
<!-- Left Side Of Navbar -->
<ul class="navbar-nav me-auto">
@guest
    <li class="nav-item">
        <a class="nav-link" href="{{url('/about')}}">About</a>
    </li>
@endguest

@auth
    <li class="nav-item">
        <a class="nav-link" href="{{url('/user')}}">Data User</a>
    </li>
@endauth
</ul>
```

2. Pada bagian lain file app.blade.php terdapat bagian menampilkan nama pengguna jika sudah login. Pada bagian lain yang berisi @yield('content') tentunya dipahami sebagai tempat menampilkan @section('content') dari file lain ke dalam template ini.

3. Akses alamat URL /login dan lihat hasil bagian atas, Ada menu About. 

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-13.png)

4. Jika sudah login, akan ada menu Data User.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-14.png)

Selain itu, jika sudah Login juga ditampilkan Nama pengguna, dan menu Logout.

[LANJUTKAN MENAMPILKAN DATA USER](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/MenampilkanDataUser.md)
