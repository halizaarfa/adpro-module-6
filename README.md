# Adpro - Tutorial 6
Haliza N. S. Arfa | 2306211401 | Adpro A

---

## Milestone 1: Single-Threaded Web Server
Dalam membuat single-threaded web server, terdapat dua protokol utama yang terlibat, yaitu
HTTP (Hypertext Transfer Protocol) dan TCP (Transmission Control Protocol).
Prinsip dari kedua protokol ini adalah request-response, artinya server menerima dan merespons request dari client.

TCP adalah protokol tingkat rendah yang menjelaskan bagaimana informasi berpindah dari suatu server ke server lainnya,
tetapi tidak menentukan apa informasi tersebut. HTTP berguna untuk menentukan konten request dan response.
Data dari HTTP ini dikirim melalui TCP.

Pada awalnya, program ini hanya mampu menerima koneksi browser request dengan menggunakan `TcpListener` ke alamat `127.0.0.1:7878`. Program akan mencetak "Connection established!" setiap kali koneksi tersebut diterima.
- `bind()`: Menghubungkan `TcpListener` dengan alamat dan port tertentu pada mesin lokal untuk didengarkan koneksinya.
- `unwrap()`: Mengekstrak nilai dari sebuah `Result` atau `Option`. Jika hasilnya adalah `Ok` atau `Some`, maka nilai yang terkandung di dalamnya akan dikembalikan. Namun, jika hasilnya adalah `Err` atau `None`, maka `unwrap` akan menyebabkan program mengalami panic dan berhenti, menunjukkan bahwa terjadi masalah yang serius.
- `incoming()`: Menerima koneksi yang masuk pada `TcpListener`. Ini mengembalikan sebuah iterator yang menghasilkan `Result<TcpStream, Error>`, di mana setiap elemen iterator tersebut adalah _stream_ yang mewakili koneksi terbuka antara client dan server. Kita dapat menerima koneksi secara berurutan dan menanganinya satu per satu.
 
Untuk menangani bagaimana server akan berperilaku ketika menerima request dari browser, dibuat method `handle_connection()`.
Console akan memberikan pesan HTTP (`http request`) di akhir method.
- `BufReader` membaca stream atau baris-baris dari `TcpStream` dengan metode `lines()`.
- `http_request` mengumpulkan baris-baris permintaan HTTP yang diterima dari browser.
- `println!("Request: {:#?}", http_request);` mencetak seluruh permintaan HTTP dalam format debug.

## Milestone 2: Returning HTML
Screenshot:
<img src='img/commit2.png'>

Fungsi `handle_connection` menggunakan `fs::read_to_string` untuk membaca konten dari file `hello.html` ke dalam sebuah string.
Hal tersebut berguna untuk mengirimkan konten HTML sebagai bagian dari respons yang dikirimkan kembali kepada user.

Setelah itu, buat respons HTTP dengan status line `"HTTP/1.1 200 OK"` dan header `Content-Length` yang menunjukkan panjang dari
konten HTML yang akan dikirimkan, dan menambahkan konten HTML sebagai badan (body) dari respons.

Selanjutnya, kita mengirimkan respons tersebut melalui stream TCP menggunakan metode `write_all`.