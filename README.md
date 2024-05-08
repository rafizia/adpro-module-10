Nama: Muhammad Rafi Zia Ulhaq<br>
NPM: 2206814551<br>
Kelas: Pemrograman Lanjut B<br>

# Module 10 - Timer

### Understanding how it works

![alt text](https://github.com/rafizia/adpro-module-10-timer/blob/master/src/image/Experiment%201.2.png?raw=true)

Pesan "Rafi's Komputer: hey hey" dicetak terlebih dahulu daripada baris kode `spawner.spawn(async { ... })` padahal baris kode `spawner.spawn(async { ... })` dijalankan sebelum kode `println!("Rafi's Komputer: hey hey")`. Hal ini terjadi karena pesan tersebut dicetak secara langsung oleh fungsi `println!()` di dalam fungsi `main()`. Saat program dimulai, *task* `async` dipasang oleh `spawner.spawn(async { ... })`. Namun, *task* tersebut masih dalam *queue* untuk dieksekusi oleh `Executor`. Setelah *task* tersebut dipasang, pesan "Rafi's Komputer: hey hey" dicetak. Kemudian, `Executor` menjalankan *task* `async` tersebut setelah pesan "hey hey" dicetak.

### Multiple Spawn

![alt text](https://github.com/rafizia/adpro-module-10-timer/blob/master/src/image/Experiment%201.3%20-%20drop(spawner).png?raw=true)

Pada gambar di atas, terlihat bahwa pesan "done3" muncul lebih dulu daripada "done2". Hal ini terjadi karena proses eksekusi terjadi secara konkuren, sehingga ketiga *tasks* tadi mungkin dieksekusi dalam urutan yang berbeda-beda. Oleh karena itu, meskipun *task* "done3" dipasang terakhir, hal tersebut tidak menjamin bahwa *task* tersebut akan dieksekusi terakhir.

### Removing drop

![alt text](https://github.com/rafizia/adpro-module-10-timer/blob/master/src/image/Experiment%201.3%20-%20no%20drop(spawner).png?raw=true)

Jika kita menghilangkan baris `drop(spawner)`, hal ini berarti `Spawner` tidak akan di-*drop* secara eksplisit, sehingga komunikasi antara `Spawner` dan `Executor` tidak akan pernah mengalami kondisi *disconnect* secara otomatis karena mungkin masih terdapat komunikasi yang mungkin terjadi. Tidak adanya kondisi *disconnect* ini mengakibatkan `Executor` terus menunggu task baru datang tanpa akhir.

