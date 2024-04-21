# Menampilkan Data User

Step 1: Tambahkan sebuah baris routes pada file **web.php** sebagai berikut.

```
Route::get('/user', [App\Http\Controllers\UserController::class, 'index'])->name('user');
```

Alamat URL tersebut akan memanggil UserController.php.

Step 2: Buat controller menggunakan terminal dengan perintah **php artisan make:controller UserController**

Step 3: Buat fungsi index() pada **UserController.php**, dan tambahan penggunaan Facades Auth.

```
use Illuminate\Support\Facades\Auth; //Auth ada fungsi menarik data
```

```
public function index()
{
    echo Auth::user()->id."<br>";
    echo Auth::user()->name."<br>";
    echo Auth::user()->email."<br>";
    echo Auth::user()->password."<br>";
    dump(Auth::user());
}
```

Step 4: Kemudian login, klik menu Data User, Lihat hasilnya.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-15.png)

Step 5: Selain menggunakan Auth, user yang sedang login juga dapat diketahui menggunakan facades Request. Pada UserController.php buat fungsi datauser().

```
public function dataUser(Request $request)
{
echo $request->user()->id."<br>";
echo $request->user()->name."<br>";
echo $request->user()->email."<br>";
echo $request->user()->password."<br>";
}
```

Step 6: Tambahkan sebuah route.

```
Route::get('/user-data', [App\Http\Controllers\UserController::class, 'dataUser'])->name('user_-_data');
```

Step 7: Lihat pada url /user-data dan lihat hasilnya.

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-16.png)

Step 8: Lanjutkan membuat file user.blade.php, isinya sebagai berikut.

```
@extends('layouts.app')
@section('content')
<div class="container">
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header">{{$judul}}</div>
                    <div class="card-body">
                        <p>Tempat Data User</p>
                        Nama : {{$datauser['name']}}
					    <br>
					    Email : {{$datauser['email']}}
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

Step 9: Modifikasi fungsi dataUser pada UserController.

```
public function dataUser(Request $request)
{
    if(Auth::check())
    {
        return view('user',['judul'=>'Data User', 'datauser'=>$request->user()]);
    }
    else
    {
        return view('user',['judul'=>'Silahkan login terlebih dahulu']);
    }
    
}
```

Step 10: Modifikasi bagian **web.php** 

Rubah kode didalam file tersebut dari yang semula
```
Route::get('/user', [App\Http\Controllers\UserController::class, 'index'])->name('user');
```
Menjadi
```
Route::get('/user', [App\Http\Controllers\UserController::class, 'dataUser'])->name('user');
```

Jadi ketika kalian menekan bagian **Data User** maka akan seperti ini

![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-25.png)

[LANJUT KE PEMBUATAN TABEL](https://github.com/AdamFirman8124/Pemateri/blob/main/Sistem%20Informasi/Materi.md)