### CSS
Bayangkan kamu punya selembar kertas besar di depanmu, dan kamu ingin membuat halaman login di dalamnya seperti yang sering kamu lihat di situs web. Jika halaman tersebut biasa saja terlihat membosankan bukan? nah css ini buat mengatur file html.

### 1. Kita buat judul besar "login" di tengah kertas:
```css
h1 {
    text-align: center;
    font-size: 24px; /* Ini membuat tulisan lebih besar */
    color: navy; /* Ini membuat tulisan berwarna biru tua */
}
```
### 2. Kita tambahkan kotak untuk nama pengguna dan kata sandi:
```css
input[type="text"],
input[type="password"] {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border: 2px solid #ccc; /* Ini memberikan garis pinggir kotak */
    border-radius: 5px; /* Ini membuat kotak menjadi berbentuk bulat di sudutnya */
    box-sizing: border-box;
}
```
### 3. Kemudian, tambahkan tombol "Login":
```css
input[type="submit"] {
    width: 100%;
    background-color: #007bff; /* Ini memberi warna latar belakang tombol */
    color: white; /* Ini memberi warna tulisan pada tombol */
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer; /* Ini membuat kursor berubah saat diarahkan ke tombol */
}
```
### 4. Terakhir, kita akan membuat kertas ini tampak seperti kartu dengan bayangan dan sedikit ruang kosong di sekitarnya:
```css
.card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Ini memberikan efek bayangan */
    max-width: 400px; /* Ini mengatur lebar maksimal kartu */
    margin: 0 auto; /* Ini membuat kartu berada di tengah layar */
}
```
Jadi, ketika kamu menambahkan semua ini ke halaman HTML-nya, kamu akan mendapatkan halaman login yang tampak seperti kartu dan berada di tengah layar! Semoga penjelasan ini membantu kamu memahaminya dengan lebih baik!

