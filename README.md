# Tutorial 6

# Commit 1
# Fungsi
```rust
fn handle_connection(mut stream: TcpStream) { 
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
    println!("Request: {:#?}", http_request);
}
```


# Inisialisasi Buffered Reader
- Membuat instance `BufReader` dengan menggunakan `BufReader::new(&mut stream);`. Ini mengambil mutable borrow dari `stream`, memungkinkan buffering data untuk pembacaan yang lebih efisien.

# Membaca Baris dari Stream
- `buf_reader.lines()`: Mengembalikan iterator yang berisi setiap baris dari stream sebagai `Result<String, Error>`.
- `.map(|result| result.unwrap())`: Iterasi dan unwrap setiap `Result` untuk mendapatkan `String`. Akan panic jika terjadi error.
- `.take_while(|line| !line.is_empty())`: Mengambil baris selama tidak kosong, berhenti pada baris kosong yang menandakan akhir header HTTP.
- `.collect()`: Mengumpulkan baris-baris yang diiterasi ke dalam `Vec<String>`, menyimpannya dalam `http_request`.

# Mencetak Permintaan HTTP
- `println!("Request: {:#?}", http_request);`: Mencetak `http_request`, yang berisi baris-baris dari permintaan HTTP, ke konsol dengan format "pretty print" untuk memudahkan debugging.

# Kesimpulan
Fungsi `handle_connection` adalah bagian penting dari server HTTP, di mana ia membaca stream data dari koneksi TCP, mengumpulkan permintaan HTTP dalam bentuk baris-baris teks, dan kemudian mencetaknya. Ini menunjukkan penggunaan borrowing di Rust untuk membaca data tanpa mengambil kepemilikan penuh, sehingga memfasilitasi penggunaan sumber daya yang efisien.

# Commit 2
![image](https://github.com/rhaken/advprog-modul6/assets/39646450/44646ea0-3f29-4a90-b6a4-8ecb09306160)
- Pada bagian modifikasi, ditambahkan variabel status_line yang menyatakan status line HTTP, yaitu "HTTP/1.1 200 OK". Ini memberikan informasi kepada klien bahwa permintaan berhasil (kode status 200).

- Selanjutnya, file HTML statis yang akan dikirim sebagai respons `(hello.html)`. Respons dibaca menggunakan `fs::read_to_string` dan dimasukkan ke dalam variabel contents.

- Panjang konten dari file HTML dihitung menggunakan `contents.len() `untuk menentukan ukuran respons. Respons disusun menggunakan `format!` untuk mencakup status line, panjang konten, dan isi konten.

# Commit 3
![image](https://github.com/rhaken/advprog-modul6/assets/39646450/044b0441-a643-4080-9130-ec50208003a9)
### Pemisahan Berdasarkan Permintaan
- Respons HTTP dibagi berdasarkan permintaan yang diterima dari klien. Jika permintaan adalah `GET / HTTP/1.1`, server akan mengirimkan respons `HTTP 200 (OK)` bersama dengan konten dari file `hello.html`. Namun, jika permintaan tidak dikenali, server akan mengirimkan respons `HTTP 404 (Not Found)` bersama dengan konten dari file `404.html`.

### Alasan Refakatorisasi
- **Ketepatan dan Kesesuaian Respons membagi respons** Berdasarkan permintaan, server memberikan respons yang lebih sesuai dengan situasi. Misalnya, jika klien meminta halaman yang tidak ada, respons `HTTP 404` memberi tahu klien bahwa halaman tidak ditemukan. Sementara respons `HTTP 200` menunjukkan bahwa halaman yang diminta berhasil ditemukan.

- **Penanganan Permintaan yang Tidak Dikenali** ini memungkinkan server untuk menangani permintaan yang tidak dikenali. Daripada memberikan respons default, server sekarang memberikan respons yang lebih informatif `(HTTP 404)` kepada klien, memberi tahu mereka bahwa permintaan tidak dapat dipenuhi. Selain itu informasi dapat dikostumisasi pada file `404.html`.


