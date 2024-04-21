# Menambahkan Role Pada User

1. Modifikasi Database

    1.1. Buka phpMyAdmin melalui xampp:

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-71.png)
        
    Pilih bagian Admin pada MySQL nya. maka akan keluar tampilan ini:

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-81.png)

    1.2. Setelah masuk, pilih nama database kalian masing - masing (disini namanya laravel) -> users -> Structure

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image1.png)

    1.3. Pilih bagian Add = 1, colomn(s) = after email, kemudian pencet Go

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-111.png)

    1.4. kemudian masukkan Name = role, Type = VARCHAR, Length/Values = 50, lalu klik Save

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-211.png)

    1.5. Masuk pada bagian Browse tab, lalu pilih edit users pertama

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-31.png)

    1.6. Pilih attribute role dan masukkan Value nya = manajer, scroll kebawah dan klik Go

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-51.png)

2. Modifikasi Kode User

    Buka file user.php, lalu ganti menjadi kode ini:
    ```
    protected $fillable = [
        'name',
        'email',
        'role',
        'password',
    ];
    ```
    sehingga kode seluruh kode user.php menjadi seperti ini:
    ```
    <?php

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
            'role',
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
3. Modifikasi Register Controller

    Buka file RegisterController.php, kemudian tambahkan kode ini di bagian paling bawah class RegisterCOntroller:
    ```
    protected function create(array $data)
    {
        return User::create([
            'name' => $data['name'],
            'email' => $data['email'],
            'role' => 'staff',
            'password' => Hash::make($data['password']),
        ]);
    }
    ```
    sehingga kode RegisterController.php menjadi seperti ini:
    ```
    <?php

    namespace App\Http\Controllers\Auth;

    use App\Http\Controllers\Controller;
    use App\Models\User;
    use Illuminate\Foundation\Auth\RegistersUsers;
    use Illuminate\Support\Facades\Hash;
    use Illuminate\Support\Facades\Validator;

    class RegisterController extends Controller
    {
        /*
        |--------------------------------------------------------------------------
        | Register Controller
        |--------------------------------------------------------------------------
        |
        | This controller handles the registration of new users as well as their
        | validation and creation. By default this controller uses a trait to
        | provide this functionality without requiring any additional code.
        |
        */

        use RegistersUsers;

        /**
        * Where to redirect users after registration.
        *
        * @var string
        */
        protected $redirectTo = '/home';

        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
            $this->middleware('guest');
        }

        /**
        * Get a validator for an incoming registration request.
        *
        * @param  array  $data
        * @return \Illuminate\Contracts\Validation\Validator
        */
        protected function validator(array $data)
        {
            return Validator::make($data, [
                'name' => ['required', 'string', 'max:255'],
                'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
                'password' => ['required', 'string', 'min:8', 'confirmed'],
            ]);
        }

        /**
        * Create a new user instance after a valid registration.
        *
        * @param  array  $data
        * @return \App\Models\User
        */
        protected function create(array $data)
        {
            return User::create([
                'name' => $data['name'],
                'email' => $data['email'],
                'role' => 'staff',
                'password' => Hash::make($data['password']),
            ]);
        }
    }
    ```

4. Membuat Policy Terhadap Produk Menggunakan Migration

    4.1. Membuat file policy

    Masukkan perintah dibawah ini kedalam terminal:
    ```
    php artisan make:policy ProdukPolicy -m Produk
    ```
    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Menambahkan%20Role/assets/image-61.png)

    4.2. Set fungsi pada Produk Policy menjadi 'true'

    Buka file ProdukPolicy.php, kemudian ganti function view menjadi seperti ini:
    ```
    public function view(User $user, Produk $produk): bool
    {
        return true;
    }
    ```

    4.3. Atur fungsi delete pada Produk Policy

    Pada file ProdukPolicy.php, ubah function delete menjadi seperti ini:
    ```
    public function delete(User $user, Produk $produk): bool
    {
        // Khusus delete, hanya pengguna yang tertulis yang dibolehkan
        return $user->role === 'manajer';
        // return $user->email === 'priandari@gmail.com';
    }
    ```
    Kemudian buka file ProdukController.php, lalu ubah function destroy menjadi seperti ini:
    ```
    public function destroy(Produk $produk)
    {
        $this->authorize('delete',$produk);
        
        // Hapus file
        if($produk->image){ //Cek dahulu apakah field ada isinyagant
            $destination = 'img/'.$produk->image;
            if(File::exists($destination)) // Cek apakah file ada di folder
            {
                File::delete($destination);
            }
        }

        $produk->delete();
        return redirect('/produks')->with('pesan',"Produk $produk->nama_produk berhasil dihapus");
    }
    ```

    4.4. Atur fungsi create pada Produk Policy

    Pada file ProdukPolicy.php, ubah function create menjadi seperti ini:
    ```
    public function create(User $user): bool
    {
        // Gunakan in_array jika lebih dari satu peran yang boleh melakukan hak akses
        // return in_array($user->email,['user@gmail.com']);
        return in_array($user->role,['manajer', 'staff']);
    }
    ```

    Kemudian pada file ProdukController.php, ubah function store menjadi seperti ini:
    ```
    public function store(Request $request)
    {
        $this->authorize('create',Produk::class);
        
        $validateData = $request->validate(
            [
                'nama_produk'   => 'required',
                'deskripsi' => 'required',
                'image' => 'image|mimes:jpg,png,jpeg,gif,svg|max:2048'
                // 'jumlah' => 'required|min:10|integer'
            ]
            );

        if($request->file('image')){
            $file= $request->file('image');
            $filename= date('YmdHi').$file->getClientOriginalName();
            $file-> move(public_path('img'), $filename);
            $validateData['image']=$filename;
        }
        // Tambah atribut yang tidak memakai validasi. 
        $validateData['aturan_sewa']=$request->aturan_sewa;
        /*
        Cara Lain tidak memakai mass assignment
        $data= new Produk();

        if($request->file('image')){
            $file= $request->file('image');
            $filename= date('YmdHi').$file->getClientOriginalName();
            $file-> move(public_path('public/img'), $filename);
            $data['image']= $filename;
        }
        $data->save();
        */

        // kalau pakai mass assignment di model harus ada
        // protected $guarded = [];
        Produk::create($validateData);
        return redirect('/produks')->with('pesan',"Produk $request->nama_produk berhasil ditambahkan");
    }
    ```
5. Pemberian Hak Akses

    Pada file ProdukPolicy.php, terdapat kode yang tadi dimasukkan seperti ini:
    ```
    public function create(User $user): bool
    {
        // Gunakan in_array jika lebih dari satu peran yang boleh melakukan hak akses
        // return in_array($user->email,['user@gmail.com']);
        return in_array($user->role,['manajer', 'staff']);
    }
    ```
    Kode diatas berarti setiap user yang memiliki role sebagai manajer atau staff dapat melakukan fungsi create pada database.
    
    Kemudian pada file create.blade.php, masukkan kode ini antara `<hr>` dengan `<form action...>`:
    ```
    @can('create', App\Models\Produk::class)
    <p>Bagian ini hanya bisa dilihat yang memiliki hak akses create produk</p>
    @endcan
    ```
    Kode diatas diawali `@can` dan diakihri dengan `@endcan` yang berarti bagian tersebut merupakan bagian yang dapat menampilkan bagian yang bisa di akses oleh pemilik akses create produk. Sebaliknya jika diawali `@cannot` dan diakhiri `@endcannot` digunakan untuk yang tidak memiliki hak akses create produk.

6. Pembatasan Menggunakan Route

    Misalnya user dengan role sebagai manajer dan staff dapat melihat data, maka pastikan kode ini ada pada function view di file ProdukPolicy.php:
    ```
    public function view(User $user, Produk $produk): bool
    {
        // return in_array($user->email,['user@gmail.com']);
        return in_array($user->role,['manajer','staff']);
        // Jika semua pengguna boleh, maka set return true seperti semula
    }
    ```
    Kemudian ubah route dengan memasukkan kode ini kedalam web.php:
    ```
    Route::get('produks/{produk}',[ProdukController::class,'show'])
    ->name('produks.show')
    ->middleware('can:view,produk');
    ```
    Kode diatas digunakan untuk memberikan rute pada web.php untuk menampilkan detail produk sesuai dengan hak akses tiap user berdasarkan role nya.

7. Final Code

    Berikut adalah final code dari file create.blade.php :
    ```
    @extends('layouts.app')

    @section('content')
    <div class="container mt-3">
        <div class="row">
            <div class="col-sm-8 col-md-6">
                <h1 class="h2 pt-4">Penambahan Produk</h1>
                <hr>

                @can('create', App\Models\Produk::class)
                <p>Bagian ini hanya bisa dilihat yang memiliki hak akses create produk</p>
                @endcan

                <form action="{{url('/produks')}}" method="POST" enctype="multipart/form-data">
                @csrf
                <div class="mb-3">
                    <label class="form-label" for="nama_produk">Nama Produk</label>
                    <input type="text" class="form-control @error('nama_produk') is-invalid @enderror" id="nama_produk" name="nama_produk" value="{{ old('nama_produk') }}">
                    @error('nama_produk')
                        <div class="text-danger">{{ $message }}</div>
                    @enderror
                </div>

                <div class="mb-3">
                    <label class="form-label" for="deskripsi">Deskripsi</label>
                    <input type="text" class="form-control @error('deskripsi') is-invalid @enderror" id="deskripsi" name="deskripsi" value="{{ old('deskripsi') }}">
                    @error('deskripsi')
                        <div class="text-danger">{{ $message }}</div>
                    @enderror
                </div>

                <div class="mb-3">
                    <label class="form-label" for="aturan_sewa">Aturan Sewa</label>
                    <input type="text" class="form-control @error('aturan_sewa') is-invalid @enderror" id="aturan_sewa" name="aturan_sewa" value="{{ old('aturan_sewa') }}">
                    @error('aturan_sewa')
                        <div class="text-danger">{{ $message }}</div>
                    @enderror
                </div>

                <div class="mb-3">
                    <label class="form-label" for="image">Image</label>
                    <input type="file" placeholder="Choose image" class="form-control @error('image') is-invalid @enderror" id="image" name="image" value="{{ old('image') }}">
                    @error('image')
                        <div class="text-danger">{{ $message }}</div>
                    @enderror
                </div>

                <button type="submit" class="btn btn-primary my-2">Simpan</button>
            </form>

            </div>
        </div>
    </div>
    @endsection
    ```

    Kemudian berikut ini adalah final code dari file web.php :
    ```
    <?php

    use Illuminate\Support\Facades\Route;
    use App\Http\Controllers\ProdukController;

    /*
    |--------------------------------------------------------------------------
    | Web Routes
    |--------------------------------------------------------------------------
    |
    | Here is where you can register web routes for your application. These
    | routes are loaded by the RouteServiceProvider and all of them will
    | be assigned to the "web" middleware group. Make something great!
    |
    */

    Route::get('/', function () {
        return view('welcome');
    });

    Auth::routes();

    Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');
    Route::get('/user', [App\Http\Controllers\UserController::class, 'dataUser'])->name('user');
    Route::resource('produks',ProdukController::class)->middleware('auth');
    Route::get('produks/{produk}',[ProdukController::class,'show'])
    ->name('produks.show')
    ->middleware('can:view,produk');
    ```

    Lalu berikut adalah final code dari file ProdukController.php :
    ```
    <?php

    namespace App\Http\Controllers;

    use App\Models\Produk;
    use Illuminate\Http\Request;
    // Dua baris tambahan
    use Illuminate\Http\Response;
    use Illuminate\Http\RedirectResponse;
    use Illuminate\Support\Facades\File;
    use Illuminate\Database\Eloquent\Builder; 

    class ProdukController extends Controller
    {
        public function index(Request $request) //Tambahkan Request untuk search
        {
            //Baris ini mengarahkan ke folder produk, file index.blade.php
            // return response()->view('produk.index',['produks'=>Produk::all()]);
            // Pagination tanpa searching
            // return response()->view('produk.index',['produks'=>Produk::paginate(2)]);
    
            // Pagination dan searching
            // Jika menambah search
    
            $produks = Produk::query()
            ->when(
                $request->q,
                function (Builder $builder) use ($request) {
                    $builder->where('nama_produk', 'like', "%{$request->q}%")
                        ->orWhere('deskripsi', 'like', "%{$request->q}%");
                }
            )
            ->paginate(2)->withQueryString();
        return view('produk.index', compact('produks'));
    
        }

        public function create()
        {
            //Baris ini mengarahkan ke folder produk, file create.blade.php
            return response()->view('produk.create');
        }

        public function store(Request $request)
        {
            $this->authorize('create',Produk::class);
            
            $validateData = $request->validate(
                [
                    'nama_produk'   => 'required',
                    'deskripsi' => 'required',
                    'image' => 'image|mimes:jpg,png,jpeg,gif,svg|max:2048'
                    // 'jumlah' => 'required|min:10|integer'
                ]
                );

            if($request->file('image')){
                $file= $request->file('image');
                $filename= date('YmdHi').$file->getClientOriginalName();
                $file-> move(public_path('img'), $filename);
                $validateData['image']=$filename;
            }
            // Tambah atribut yang tidak memakai validasi. 
            $validateData['aturan_sewa']=$request->aturan_sewa;
            /*
            Cara Lain tidak memakai mass assignment
            $data= new Produk();

            if($request->file('image')){
                $file= $request->file('image');
                $filename= date('YmdHi').$file->getClientOriginalName();
                $file-> move(public_path('public/img'), $filename);
                $data['image']= $filename;
            }
            $data->save();
            */

            // kalau pakai mass assignment di model harus ada
            // protected $guarded = [];
            Produk::create($validateData);
            return redirect('/produks')->with('pesan',"Produk $request->nama_produk berhasil ditambahkan");
        }

        public function show(Produk $produk)
        {
            //Baris ini mengarahkan ke folder produk, file show.blade.php
            return response()->view('produk.show', compact('produk'));
        }

        public function edit(Produk $produk)
        {
            //Baris ini mengarahkan ke folder produk, file edit.blade.php
            return response()->view('produk.edit',compact('produk'));
        }


        public function update(Request $request, Produk $produk)
        {
            $validateData = $request->validate([
                    'nama_produk'   => 'required',
                    'deskripsi' => 'required',
                    // 'aturan_sewa' => 'text',
                    'image' => 'image|mimes:jpg,png,jpeg,gif,svg|max:2048'
                    // 'jumlah' => 'required|min:10|integer'
            ]);
            $validateData['aturan_sewa']=$request->aturan_sewa;
            //Jika akan menghapus file lama.
            // $produk = Produk::find($produk->id);
            // Sebenarnya gak perlu cari karena sudah didefinisi di parameter

            if($request->hasfile('image')){
                $destination = 'img/'.$produk->image;
                if(File::exists($destination))
                {
                    File::delete($destination);
                }
                $file= $request->file('image');
                $filename= date('YmdHi').$file->getClientOriginalName();
                $file-> move(public_path('img'), $filename);
                $validateData['image']=$filename;
            }
            $produk->update($validateData);
            return redirect('/produks/'.$produk->id)->with('pesan',"Produk $produk->nama_produk berhasil diupdate");
        }

        public function destroy(Produk $produk)
        {
            $this->authorize('delete',$produk);
            
            // Hapus file
            if($produk->image){ //Cek dahulu apakah field ada isinyagant
                $destination = 'img/'.$produk->image;
                if(File::exists($destination)) // Cek apakah file ada di folder
                {
                    File::delete($destination);
                }
            }

            $produk->delete();
            return redirect('/produks')->with('pesan',"Produk $produk->nama_produk berhasil dihapus");
        }
    }
    ```
    Terakhir ini adalah final code dari file ProdukPolicy.php :
    ```
    <?php

    namespace App\Policies;

    use App\Models\Produk;
    use App\Models\User;
    use Illuminate\Auth\Access\Response;

    class ProdukPolicy
    {
        /**
        * Determine whether the user can view any models.
        */
        public function viewAny(User $user): bool
        {
            //
        }

        /**
        * Determine whether the user can view the model.
        */
        public function view(User $user, Produk $produk): bool
        {
            // return in_array($user->email,['user@gmail.com']);
            return in_array($user->role,['manajer','staff']);
            // Jika semua pengguna boleh, maka set return true seperti semula
        }

        /**
        * Determine whether the user can create models.
        */
        public function create(User $user): bool
        {
            // Gunakan in_array jika lebih dari satu peran yang boleh melakukan hak akses
            // return in_array($user->email,['user@gmail.com']);
            return in_array($user->role,['manajer', 'staff']);
        }
        /**
        * Determine whether the user can update the model.
        */
        public function update(User $user, Produk $produk): bool
        {
            //
        }

        /**
        * Determine whether the user can delete the model.
        */
        public function delete(User $user, Produk $produk): bool
        {
            // Khusus delete, hanya pengguna yang tertulis yang dibolehkan
            return $user->role === 'manajer';
            // return $user->email === 'priandari@gmail.com';
        }

        /**
        * Determine whether the user can restore the model.
        */
        public function restore(User $user, Produk $produk): bool
        {
            //
        }

        /**
        * Determine whether the user can permanently delete the model.
        */
        public function forceDelete(User $user, Produk $produk): bool
        {
            //
        }
    }
    ```