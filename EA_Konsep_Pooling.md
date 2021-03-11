Suatu sistem atau aplikasi yang menangani request yang sangat banyak dalam waktu yang bersamaan (concurrent), misalnya aplikasi web,
biasanya memiliki keterbatasan kemampuan memproses dikarenakan adanya **keterbatasan kemampuan pemrosesan dari sumberdaya perangkat 
keras (hardware resources)** seperti CPU (yang tergantung jenis dan banyaknya core), besarnya memori (RAM), disk I/O dan network I/O/

![image](https://cloud.githubusercontent.com/assets/3068071/15131944/8a135842-167e-11e6-8b35-3ab0fb5441fb.png)

Selain itu aplikasi yang menangani proses yang bergantung pada sistem lain --misalnya database atau aplikasi lain-- juga memiliki 
keterbatasan dikarenakan **batasan kemampuan eksternal sistem lain** tersebut. 

Misalnya sebuah aplikasi X memiliki proses perhitungan tertentu dimana data diambil dari sebuah database Y dan bergantung pada proses 
perhitungan lain misalnya dari aplikasi Z. Jika proses pemanggilan ke database Y dan aplikasi Z bersifat synchronous, maka proses 
perhitungan di aplikasi X akan terpengaruh oleh cepat atau lambatnya pemanggilan ke sistem Y dan Z.

![image](https://cloud.githubusercontent.com/assets/3068071/15131797/56bc2574-167d-11e6-8d7b-da90b9e73089.png)

Semakin lama sistem Y dan Z merespon maka semakin lama juga sebuah proses perhitungan di sistem X dilakukan. Jika proses perhitungan 
tersebut sangat banyak dan dilakukan bersamaan maka **dalam satu rentang waktu tententu jumlah proses yang bisa diselesaikan menjadi lebih sedikit jika waktu penyelesaian sebuah proses semakin lama**.


Masalah keterbatasan sumber daya (_resource_) ini adalah masalah yang umum pada aplikasi. Kita harus menjaga aplikasi yang kita buat dan
kita jalankan di lingkungan produksi agar tetap berjalan stabil dan tidak melebihi beban (_overload_). Lebih baik untuk sebuah aplikasi untuk selalu bisa 
menangani _request_ dan menolak _request_ lainnya, dibandingkan aplikasi tidak bisa menangani _request_ sama sekali.

Untuk mengoptimalkan penggunaan sumber daya baik internal maupun eksternal maka biasanya sebuah aplikasi menggunakan teknik **_pooling_**. 
Teknik _pooling_ memiliki beberapa konsep penting yaitu:

* Membatasi jumlah concurrent process (pekerja atau _worker_) atau jumlah obyek yang hidup di dalam sistem.
* _Reusabaility_ (penggunaan kembali) obyek tersebut, artinya obyek yang sudah selesai mengerjakan tugas tidak segera dibuang, dan penanganan _request_ baru tidak perlu membuat obyek baru, tetapi dapat menggunakan kembali obyek yang sudah tidak dipakai tersebut (_idle_).
* Antrian (_queuing_) dari _request_ yang akan dikerjakan oleh _worker_ atau yang akan menggunakan obyek tersebut.

Dalam sebuah sistem komputasi, _poolin_g dapat diimplementasikan dalam banyak hal, oleh karena itu mungkin anda sering mendengar istilah **object pooling**, **thread pooling**, **connection pooling**.

Untuk lebih mengerti konsep _pooling_, saya akan jelaskan dengan contoh yang paling umum yaitu _connection pooling_ ke database.








