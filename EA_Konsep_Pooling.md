Suatu sistem atau aplikasi yang menangani request yang sangat banyak dalam waktu yang bersamaan (concurrent), misalnya aplikasi web,
biasanya memiliki keterbatasan kemampuan memproses dikarenakan adanya **keterbatasan kemampuan pemrosesan dari sumberdaya perangkat keras (hardware resources)** seperti CPU (yang tergantung jenis dan banyaknya core), besarnya memori (RAM), disk I/O dan network I/O/

![image](https://cloud.githubusercontent.com/assets/3068071/11893212/4e9a11fa-a5a1-11e5-948e-8449c86cb142.png)



Selain itu aplikasi yang menangani proses yang bergantung pada sistem lain --misalnya database atau aplikasi lain-- juga memiliki 
keterbatasan dikarenakan **batasan kemampuan eksternal sistem lain** tersebut. 

Misalnya sebuah aplikasi X memiliki proses perhitungan tertentu dimana data diambil dari sebuah database Y dan bergantung pada proses 
perhitungan lain misalnya dari aplikasi Z. Jika proses pemanggilan ke database Y dan aplikasi Z bersifat synchronous, maka proses 
perhitungan di aplikasi X akan terpengaruh oleh cepat atau lambatnya pemanggilan ke sistem Y dan Z.

![image](https://cloud.githubusercontent.com/assets/3068071/15131797/56bc2574-167d-11e6-8d7b-da90b9e73089.png)

Semakin lama sistem Y dan Z meresponse maka semakin lama juga sebuah proses perhitungan di sistem X dilakukan. Jika proses perhitungan 
tersebut sangat banyak dan dilakukan bersamaan maka dalam satu rentang waktu jumlah proses yang bisa diselesaikan menjadi lebih sedikit
jika waktu penyelesaian sebuah proses semakin lama.



Masalah keterbatasan sumber daya (resource) ini adalah masalah yang umum pada aplikasi. Kita harus menjaga aplikasi yang kita buat dan
jalankan di lingkungan produksi agar tetap berjalan stabil (tidak overload). Lebih baik untuk sebuah aplikasi untuk selalu bisa menangani
request dan menolak request lainnya dibandingkan aplikasi mati dan tidak bisa menangani request sama sekali.

Untuk mengoptimalkan penggunaan resource baik internal maupun eksternal maka biasanya sebuah aplikasi menggunakan teknik **pooling**. 
Teknik pooling memiliki beberapa konsep penting yaitu:

* Membatasi jumlah concurrent process (pekerja atau worker) atau jumlah obyek yang hidup di dalam sistem.
* Reusabaility (penggunaan kembali) obyek tersebut artinya obyek yang sudah selesai mengerjakan tugas tidak segera dibuang, dan request baru tidak perlu membuat obyek baru tapi menggunakan kembali obyek yang sudah tidak dipakai tersebut.
* Antrian (queuing) dari request yang akan dikerjakan oleh worker atau yang akan menggunakan obyek tersebut.

Dalam sebuah sistem komputasi pooling dapat diimplementasikan dalam banyak hal, oleh karena itu mungkin anda sering mendengar istilah **object pooling**, **thread pooling**, **connection pooling**.

Untuk lebih mengerti konsep pooling, saya akan jelaskan dengan contoh yang paling umum yaitu connection pooling ke database.








