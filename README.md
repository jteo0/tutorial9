# Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
- Unary RPC adalah RPC dimana client mengirim satu request ke server dan mendapat kembali satu response. Sebaiknya digunakan dalam scenario dimana client mengirim request dan menunggu untuk respon.
- Server streaming RPC adalah RPC dimana client mengirim suatu request ke server dan mendapat kembali stream. Stream tersebut berisi beberapa *message* yang kemudian dibaca client. Sebaiknya digunakan dalam scenario dimana suatu client mendapatkan stream data dari server, seperti update real-time.
- Bidirectional streaming RPC adalah RPC dimana server dan client saling mengirim stream yang berisi *message* dan menggunakan read-write stream. Sebaiknya digunakan dalam scenario dimana server dan client perlu untuk saling mengirim stream data (kadang secara sekaligus).

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
- Authentication: Agar hanya client yang authorized dapat mengakses service, OAuth, JWT, atau skema autentikasi yang lain dapat diimplementasi dengan service gRPC di Rust.
- Authorization: Client yang sudah di autentikasi bisa diberi akses ke bagian spesifik dari operasi dengan menggunakan RBAC (Role-Based Access Control), ABAC (Attribute-Based Access Control), atau mekanisme autorisasi lain.
- Data Encryption: Agar data yang di transmit tetap bersifat konfidensial dan aman, kita dapat mengimplementasikan enkripsi TLS (Transport Layer Security) untuk gRPC communication. Ini bisa dilakukan dengan menkonfigurasi server dan client gRPC untuk menggunkan sertifikat TLS.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Beberapa masalah adalah dalam menghandle error dan exception yang mungkin muncul dan menjaga <i>thread safety</i>. Secara khusus untuk aplikasi chat, masalah dapat muncul dari terjadinya hal seperti disconnect dan timeout.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
- Keunggulannya: tokio_stream::wrappers:ReceiverStream dapat menkonvert tokio::sync::mpsc::Receiver menjadi suatu stream.
- Kekurangan: Untuk melakukan itu (yang diatas) Receiver harus bersifat valid selama seluruh lifetime stream. Ini dapat mempersusah managemen lifetime.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
- Mengimplementasi builder pattern untuk object dan konfigurasi yang kompleks.
- Mengimplementasi strategy pattern untuk variasi *behavior* pada runtime.
- Mendefinisikan fungsionalitas yang sama di modul atau library yang bisa dibagi (*share*).

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Mengecek apakah jumlah uang cukup untuk membayar, melakukan validasi informasi payment, memproses pembayaran, memberi notifikasi konfirmasi sukses atau gagal pembayaran, dan handling error dan exception.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
Adopsi gRPC dapat menyebabkan peningkatan efisiensi komunikasi karena menggunakan HTTP/2 dan protocol buffer. *Interoperability* juga meningkat, karena gRPC men-support banyak bahasa pemrograman.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
- Keuntungannya adalah HTTP/2 memiliki fungsionalisasi untuk melakukan <i>multiplexing, header compression,</i> dan <i>server push</i>. Ini dapat meningkatkan performance dan efisiensi dari kode.
- Kekurangannya adalah belum sepenuhnya di support oleh semua client dan server.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
REST API menggunakan model request-response yang kurang ideal untuk komunikasi *real-time* karena adanya overhead request. gRPC dengan bidirectional streaming lebih baik untuk komunikasi *real-time* dan *full-duplex*, namun secara umum lebih kompleks untuk diimplementasi.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
- *Strong Typing*: Protocol Buffer menggunakan skema yang lebih *strict*, sehingga data dan tipe data sudah *pre-defined*. Ini menolong untuk menangkap error pada compile-time, membuat data lebih konsisten, dan mengurangi runtime error. JSON memperbolehkan developer untuk lebih fleksibel dalam mendefinisikan struktur dan tipe data, namun ini memiliki risiko lebih tinggi untuk menyebabkan inkonsistensi dan error pada runtime.
- Efisiensi: Protocol Buffer menggunakan format *binary serialization*, sedangkan JSON menggunakan format berbasis teks. *Binary serialization* menggunakan ukuran message yang lebih kecil dan serialisasi yang lebih cepat, sehingga lebih efisien dalam konteks kecepatan pemrosesan dan network bandwidth.
- *Code Generation*: Protocol Buffer memerlukan langkah kompilasi yang terpisah untuk men-*generate* kode untuk interface service dan *message types*. Proses ini menyediakan API *strongly typed* di berbagai bahasa programming, sehingga lebih mudah untuk digunakan dengan gRPC. Sebaliknya, JSON tidak menggunakan *code generation* dan bisa langsung di parse dan manupulasi dengan fitur dan library in-language.
- *Versioning and Evolution*: Protocol Buffer mendukung evolusi schema dan versioning dengan penggunaan message dan opsi *field-level.* Ini membuatnya bisa ada backward dan forward compatibility diantara versi beda dari suatu service. Sebaliknya, JSON tidak memiliki pendukungan *built-in*.
