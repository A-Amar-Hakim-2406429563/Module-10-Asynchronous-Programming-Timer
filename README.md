## Experiment 1.2 - Understanding How It Works

![alt text](Commit2.png)

Di experiment ini aku nambahin satu `println!()` setelah `spawner.spawn(...)`.

```rust
println!("Amar's Computer: hey hey");
```
Pas program dijalankan, output yang muncul malah:
- Amar's Computer: hey hey
- Amar's Computer: howdy!
- Amar's Computer: done!

Awalnya aku kira howdy! bakal muncul duluan, tapi ternyata hey hey yang muncul dulu. Hal ini terjadi karena spawner.spawn(...) sebenarnya belum langsung menjalankan async task-nya. Spawn cuma memasukkan task ke dalam queue milik executor.

Task async baru benar-benar dijalankan saat executor.run() dipanggil. Karena itu, kode biasa setelah spawn() masih dijalankan dulu secara normal sebelum executor mulai memproses future yang ada. Setelah executor berjalan, task async mulai dipoll, lalu muncul howdy!, delay 2 detik, dan akhirnya selesai deh.