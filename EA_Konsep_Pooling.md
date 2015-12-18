Suatu sistem atau aplikasi yang menangani request yang sangat banyak dalam waktu yang bersamaan (concurrent), misalnya aplikasi web,
biasanya memiliki keterbatasan kemampuan memproses dikarenakan adanya keterbatasan kemampuan pemrosesan oleh CPU 
(yang tergantung jenis dan banyaknya core), besarnya memori (RAM) dan disk I/O. 

Selain itu aplikasi yang menangani proses yang bergantung pada sistem lain --misalnya database atau aplikasi lain-- juga memiliki 
keterbatasan dikarenakan batasan kemampuan sistem lain tersebut. 

Misalnya sebuah aplikasi X memiliki proses perhitungan tertentu dimana data diambil dari sebuah database Y dan bergantung pada proses 
perhitungan lain misalnya dari aplikasi Z. Jika proses pemanggilan ke database Y dan aplikasi Z bersifat synchronous, maka proses 
perhitungan di aplikasi X akan terpengaruh oleh cepat atau lambatnya pemanggilan ke sistem Y dan Z.

Semakin lama sistem Y dan Z meresponse maka semakin lama juga sebuah proses perhitungan di sistem X dilakukan. Jika proses perhitungan 
tersebut sangat banyak dan dilakukan bersamaan maka dalam satu rentang waktu jumlah proses yang bisa diselesaikan menjadi lebih sedikit 
jika waktu penyelesaian sebuah proses semakin lama.

