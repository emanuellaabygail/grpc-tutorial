# Tutorial 8 - Advanced Programming - High Level Networking
**Emanuella Abygail - 2206081534**

## Refleksi

### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

#### Jawaban:
- Unary RPC: Dalam metode ini, client mengirimkan  satu request dan server memberikan satu response. Cocok untuk request sederhana seperti mengambil data atau menjalankan satu perintah.
  
- Server Streaming RPC: Client mengirimkan satu request dan menerima banyak response dari server secara bertahap. Ini cocok untuk skenario di mana server mengirimkan data dalam potongan-potongan, seperti streaming video atau data sensor yang terus diperbarui.
  
- Bi-directional Streaming RPC: Baik client maupun server dapat mengirimdata secara terus-menerus. Cocok untuk komunikasi dua arah yang aktif, seperti aplikasi chat atau game real-time.

#### Kapan Digunakan:
- Unary: Saat hanya butuh satu permintaan dan satu respons.
- Server Streaming: Saat server perlu mengirim data berkelanjutan ke klien.
- Bi-directional Streaming: Saat komunikasi dua arah yang interaktif dibutuhkan.

---

### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

#### Jawaban:
- Autentikasi: Memastikan identitas klien dan server, biasanya lewat sertifikat TLS/SSL. Di Rust, bisa menggunakan crate seperti `tonic`.
  
- Otorisasi: Memeriksa apakah pengguna punya izin untuk mengakses layanan tertentu. Bisa dibuat menggunakan middleware.

- Enkripsi Data: gRPC sudah menggunakan SSL/TLS untuk mengenkripsi komunikasi antara client dan server, yang menghindari potensi pencurian data. Rust menyediakan library seperti `openssl` atau `rustls` untuk implementasi enkripsi ini.

#### Tantangan:
Memastikan sertifikat tetap valid dan aman, serta menjaga pengelolaan kunci dengan baik agar komunikasi tetap terlindungi.

---

### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

#### Jawaban:
- Dalam aplikasi streaming dua arah seperti chat, memastikan bahwa koneksi tetap stabil dan tidak terputus adalah tantangan. Di Rust, ini bisa melibatkan pengelolaan waktu tunggu (timeout) atau deteksi kesalahan.
  
- Menjaga aliran pesan yang konsisten antara banyak client bisa menjadi masalah terutama jika ada banyak pengguna yang terhubung sekaligus. Ini bisa menyebabkan kesulitan dalam hal sinkronisasi dan urutan pesan.

- Ketika lebih banyak client yang terhubung, server harus bisa menangani banyak aliran data secara bersamaan tanpa menurunkan kinerja. Ini membutuhkan arsitektur yang scalable dan penggunaan library asynchronous seperti `tokio`.

---

### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

#### Jawaban:
- **Keuntungan**:
  - ReceiverStream memberikan fleksibilitas karena memungkinkan konversi dari tokio::sync::mpsc::Receiver menjadi stream yang bisa langsung digunakan dalam gRPC.
  - Penggunaannya cukup sederhana dan cukup mudah.
  - ReceiverStream juga mudah diintegrasikan dengan ekosistem dan tools lain yang disediakan oleh Tokio sehingga seringkali menjadi pilihan.
  
- **Kekurangan**:
  - Penggunaan ReceiverStream dengan operasi yang bersifat blocking bisa menurunkan performa aplikasi secara keseluruhan.
  - Tidak ideal untuk skenario dengan throughput tinggi, karena ReceiverStream tidak dapat menangani volume data yang besar secara efektif.

---

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

#### Jawaban:
- Struktur kode dapat memisahkan logika aplikasi dari kode infrastruktur (seperti server gRPC, autentikasi, dan database). Dengan cara ini, logika bisnis lebih mudah diuji dan dikembangkan secara terpisah.
  
- Membuat modul-modul kecil yang dapat digunakan lagi dan crates untuk fungsi-fungsi umum, seperti autentikasi atau pengelolaan aliran data. Hal ini meningkatkan modularity dan membuat kode lebih mudah untuk di-maintain dan diperbarui.

- Menerapkan prinsip SOLID (misalnya SRP dan OCP) dapat meningkatkan skalability dan maintainability kode di masa depan.

---

### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

#### Jawaban:
- Validasi data untuk memastikan data pembayaran yang diterima valid dan lengkap, seperti memeriksa nomor kartu kredit atau saldo rekening.
  
- Integrasi dengan Penyedia Pembayaran**: Menghubungkan dengan API eksternal untuk memproses pembayaran, seperti sistem kartu kredit atau gateway pembayaran.
  
- Menambahkan keamanan tambahan, seperti enkripsi data sensitif dan autentikasi dua faktor, untuk melindungi transaksi.

---

### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

#### Jawaban:
- Kecepatan dan efisiensi: gRPC menggunakan HTTP/2 yang memungkinkan pengiriman data lebih cepat dengan multiplexing, kompresi header, dan pengelolaan koneksi yang lebih efisien.
  
- Language agnostic: gRPC mendukung berbagai bahasa pemrograman (seperti Rust, Go, Java) sehingga memungkinkan integrasi mudah antara sistem yang berbeda.

- Skalabilitas: gRPC berbasis pada protokol biner dan mendukung streaming sehingga lebih cocok untuk aplikasi yang memerlukan skalabilitas tinggi seperti microservices.

---

### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

#### Jawaban:
- **Keuntungan HTTP/2**:
  - Multiplexing: Beberapa request dapat dikirimkan dalam satu koneksi sehingga mengurangi keterlambatan.
  - Kompresi header: Mengurangi ukuran header HTTP, meningkatkan efisiensi bandwidth.
  - Push Server: Server dapat mendorong data ke client.

- **Kekurangan HTTP/2**:
  - Kompleksitas Implementasi: Dibandingkan dengan HTTP/1.1, HTTP/2 lebih kompleks untuk diimplementasikan dan dikonfigurasi.
  - Kompatibilitas: Beberapa aplikasi atau layanan yang lebih lama mungkin tidak mendukung HTTP/2.

---

### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

#### Jawaban:
- **Request-Response (REST)**: REST API menggunakan model request-response satu kali, yang berarti client harus menunggu response setelah mengirimkan request. Ini tidak cocok untuk komunikasi waktu nyata yang memerlukan pembaruan atau response yang cepat.
  
- **Bidirectional Streaming (gRPC)**: gRPC memungkinkan komunikasi dua arah yang lebih cepat dan lebih efisien dengan mengalirkan data secara terus-menerus antara client dan server. Ini sangat cocok untuk aplikasi real-time, seperti chat atau sistem pemantauan.

---

### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

#### Jawaban:
- **gRPC (Protocol Buffers)**:
  - Keuntungan: Lebih efisien dalam hal ukuran dan kecepatan karena menggunakan format biner yang terstruktur dan terkompresi.
  - Kekurangan: Kurang fleksibel dibandingkan JSON, karena perubahan skema memerlukan pembaruan di seluruh sistem.

- **REST (JSON)**:
  - Keuntungan: JSON lebih fleksibel karena dapat dengan mudah diperbarui dan dipahami tanpa memerlukan alat khusus.
  - Kekurangan: JSON cenderung lebih besar dan lebih lambat dibandingkan dengan format biner seperti Protocol Buffers.
