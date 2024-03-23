# Tutorial 6

## Commit 1
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
