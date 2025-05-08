# Refleksi Module 8 - HLN
## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
- Unary adalah protokol komunikasi dimana client mengirim satu request dan server mengirim kembali satu request. Penggunaannya cocok untuk autentikasi (client mengirim kredensial dan server mengirim kembali bearer token).
- Server streaming adalah protokol komunikasi dimana client atau server mengirim stream of responses (tidak hanya satu). Penggunaannya cocok untuk mengirim file besar dari sisi client, atau mengirim data streaming video dari dari sisi server.
- Bidirectional streaming adalah protokol komunikasi dimana client dan server saling mengirim data dalam continuous stream. Penggunaannya cocok untuk komunikasi real-time antara client dan server, seperti untuk instant messenger (chat applications seperti WA, Discord, dan LINE).

## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
Dalam mengirim data seperti authentication token, dan kredensial, komunikasi yang digunakan pada gRPC harus di encrypt. Dalam Rust, kita bisa menggunakan  `tonic::transport::ServerTlsConfig`.
Untuk meningkatkan security, arsitektur yang diterapkan sebaiknya layered dengan adanya intermediary seperti proxy dan gateway antara client dan server. gRPC sudah memiliki support untuk modul autentikasi dan load balancing yang bisa diterapkan juga.

## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Dalam handling bidirectional streaming pada Rust gRPC, masalah yang bisa dihadapi antara lain mengkoordinasi urutan pesan (khususnya bila beberapa orang mengirim pesan secara sekaligus), mengatasi masalah concurrency, dan handling connection state beserta timeout.

## 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
- Keuntungan: abstraksi yang rapih untuk stream asynchronous, backpressure handling secara otomatis, dan kompabilitas dengan asynchronous model dari Tokio.
- Kekurangan: lebih berat dibandingkan implementasi direct stream, kemungkinan lebih kompleks saat error handling

## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
Struktur yang bisa diterapkan yaitu membuat module terpisah untuk logika bisnis di dalam service dan gRPC handler. Kita bisa menggunakan traits untuk membuat interface layanan, dan middleware atau interceptor sebagai reusable module untuk fungsionalitas umum.

## 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Untuk payment processing yang lebih kompleks, dibutuhkan integrity check, input validation, dan race condition prevention dengan menerapkan locks. Selain itu, perlu juga pengelolaan database yang baik, dan adanya placeholder untuk terhubung ke third party seperti payment gateway.

## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
gRPC lebih lightweight dibandingkan protokol komunikasi lainnya, dan mendukung microservices dengan berbagai programming languages. Meskipun demikian, gRPC kurang fleksibel karena infrastruktur harus mendukung HTTP/2, sehingga tidak bisa terhubung ke semua jenis platform dan teknologi.

## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
HTTP/1.1 lebih fleksibel dan universal dibandingkan HTTP/2, meskipun lebih lambat dan tidak memiliki fitur efisiensi. HTTP/1.1 kompatibel dengan lebih banyak services dibandingkan HTTP/2. HTTP/2 lebih lightweight, memiliki compression untuk header, dan lebih efisien secara keseluruhan. Meskipun demikian, HTTP/2 belum didukung oleh banyak platform, serta lebih kompleks untuk diterapkan dan di-debug.

## 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
Model REST bersifat request-driven dan pull-based, sehingga memerlukan polling untuk menerima update. gRPC mendukung komunikasi push-based melalui streaming, memungkinkan update real time dengan latency lebih rendah. Ini memungkinkan notifikasi langsung dari server ke client tanpa polling berulang, menghasilkan pemanfaatan sumber daya server yang lebih efisien dan pengalaman pengguna yang lebih responsif.

## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
Protocol Buffers menawarkan ukuran payload yang lebih kecil dan validasi skema yang ketat. Serialization dan deserialization lebih cepat dengan Protocol Buffers, dan system type yang kuat mencegah banyak kesalahan. Namun, JSON lebih mudah dibaca manusia dan menawarkan fleksibilitas skema yang lebih besar. Perubahan skema dalam Protocol Buffers memerlukan versioning yang lebih hati-hati, sementara JSON lebih fleksibel tetapi kurang aman dari segi validasi.
