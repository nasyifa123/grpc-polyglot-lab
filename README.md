# gRPC Polyglot Lab

Proyek praktikum komunikasi inter-service dengan gRPC: server Go, client Python, dan client Node.js yang memakai satu kontrak `user.proto`.

## Analisis Pendekatan Kompilasi

Pada pendekatan *static code generation* (Go dan Python), file `user.proto` dikompilasi lebih dulu oleh `protoc` menjadi kode sumber (`user.pb.go`, `user_grpc.pb.go`, `user_pb2.py`, `user_pb2_grpc.py`) yang disimpan di dalam proyek dan langsung dipakai saat build atau import. Tipe data sudah tervalidasi lebih awal dan performanya baik, tetapi setiap perubahan pada `.proto` (misalnya menambah field `phone`) mengharuskan proses generate ulang. Sebaliknya, pada *dynamic runtime generation* (Node.js), pustaka `@grpc/proto-loader` membaca file `.proto` langsung saat program dijalankan dan membentuk definisi service di memori, sehingga tidak ada langkah kompilasi dan perubahan skema langsung terbaca setelah program dijalankan ulang. Konsekuensinya, pengecekan tipe tidak dilakukan saat build dan ada sedikit overhead saat startup.
