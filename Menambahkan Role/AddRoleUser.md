# Menambahkan Role Pada User

1. Modifikasi Database

    1.1. Buka phpMyAdmin melalui xampp:

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-71.png)
        
    Pilih bagian Admin pada MySQL nya. maka akan keluar tampilan ini:

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-81.png)

    1.2. Setelah masuk, pilih nama database kalian masing - masing (disini namanya laravel) -> users -> Structure

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image1.png)

    1.3. Pilih bagian Add = 1, colomn(s) = after email, kemudian pencet Go

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-111.png)

    1.4. kemudian masukkan Name = role, Type = VARCHAR, Length/Values = 50, lalu kelik Save

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-211.png)

    1.5. Masuk pada bagian Browse tab, lalu pilih edit users pertama

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-31.png)

    1.6. Pilih attribute role dan masukkan Value nya = manajer, scroll kebawah dan klik Go

    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-51.png)

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
3. Modefikasi Register Controller

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
    ![alt text](https://github.com/AdamFirman8124/Pemateri/blob/main/Laravel%2010/assets/image-61.png)

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
        // Pembatasan hak akses atau policy
        $this->authorize('delete',$produk);
        
        // Lanjutan blok program tetap

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
        // Pembatasan hak akses atau policy
        $this->authorize('create',Produk::class);
        
        // Lanjutan blok program tetap

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
    Kode diatas berarti setiap user uang memiliki role sebagai manajer atau staff dapat melakukan fungsi create pada database.
    
    Kemudian pada file create.blade.php, bisa dimasukkan kode ini untuk pengatran tampilan hak akses yag dimiliki untuk create produk:
    ```
    @can('create', App\Models\Produk::class)
    <p>Bagian ini hanya bisa dilihat yang memiliki hak akses create produk</p>
    @endcan
    ```
    Kode diatas diawali `@can` dan diakihri dengan `@endcan` yang berarti bagian tersebut merupakan bagian yang dapat menampilkan bagian yang bisa di akses oleh pemilik akses create produk. sebaliknya jika diawali `@cannot` dan diakhiri `@endcannot` digunakan untuk yang tidak memiliki hak akses create produk.

6. Pembatasan Menggunakan Route

    Misalnya user dengan role sebagai manajer dan staff dapat melihat data, maka pastikan kode ini ada pada function view di file Produk.Policy.php:
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