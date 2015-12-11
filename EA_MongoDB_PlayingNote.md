# MongoDB  

## Menjalankan MongoDB 

Buat direktori untuk menyimpan data, lalu jalankan MongoDB dengan perintah `mongod`

MongoDB akan menggunakan default direktori data yaitu di `/data/db` jika kita tidak memberikan opsi `--dbpath`

Default port yang digunakan MongoDB adalah 2717, gunakan opsi `--port <port>` untuk menggunakan port lain.

```
mkdir /data/db
mongod &
```

Berikut output
  	

```
2015-02-22T07:27:32.836+0700 I CONTROL  [initandlisten] MongoDB starting : pid=31404 port=27017 dbpath=/data/db 64-bit host=Eryans-MacBook-Pro.local
2015-02-22T07:27:32.836+0700 I CONTROL  [initandlisten] db version v3.0.0-rc9
2015-02-22T07:27:32.836+0700 I CONTROL  [initandlisten] git version: e6577bc37a2edba81b99146934cf7bad00c6e1b2
2015-02-22T07:27:32.836+0700 I CONTROL  [initandlisten] build info: Darwin bs-osx108-2 12.5.0 Darwin Kernel Version 12.5.0: Sun Sep 29 13:33:47 PDT 2013; root:xnu-2050.48.12~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2015-02-22T07:27:32.837+0700 I CONTROL  [initandlisten] allocator: system
2015-02-22T07:27:32.837+0700 I CONTROL  [initandlisten] options: {}
2015-02-22T07:27:32.893+0700 I JOURNAL  [initandlisten] journal dir=/data/db/journal
2015-02-22T07:27:32.893+0700 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-02-22T07:27:32.933+0700 I JOURNAL  [durability] Durability thread started
2015-02-22T07:27:32.935+0700 I JOURNAL  [journal writer] Journal writer thread started
2015-02-22T07:27:32.973+0700 I INDEX    [initandlisten] allocating new ns file /data/db/local.ns, filling with zeroes...
2015-02-22T07:27:33.233+0700 I STORAGE  [FileAllocator] allocating new datafile /data/db/local.0, filling with zeroes...
2015-02-22T07:27:33.233+0700 I STORAGE  [FileAllocator] creating directory /data/db/_tmp
2015-02-22T07:27:34.189+0700 I STORAGE  [FileAllocator] done allocating datafile /data/db/local.0, size: 64MB,  took 0.955 secs
2015-02-22T07:27:34.486+0700 I NETWORK  [initandlisten] waiting for connections on port 27017
```

## MongoDB Client

MongoDB client adalah program untuk kita bisa berinteraksi dengan MongoDB server.

Jalankan MongoDB client dengan perintah `mongo`, jika anda ingin keluar dari mongo prompt yang ditandai karakter `>`, gunakan perintah `exit`


```
mongo
```

Output perintah tersebut adalah sebagai berikut:

```
MongoDB shell version: 3.0.0-rc9
connecting to: test
2015-02-22T08:19:54.165+0700 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:61103 #1 (1 connection now open)
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
>
```

### Database

Melihat semua database yang ada `show dbs`

Membuat atau menggunakan database `use NAMA_DATABASE`

Melihat semua collection (tabel) yang ada di database yang digunakan `show collections`

Sekarang kita coba membuat sebuah database `books` dan kemudian coba masukan (insert) sebuah data buku. Data dibuat dalam bentuk format JSON seperti ini:

```
{
	title: "Begining Java",
	author: "Budi Budiman"
}
```

Tentu saja saat memasukan data tersebut kita tidak perlu dalam bentuk yang cantik seperti itu.

```
> use books
switched to db ejlp
> db.books.insert({title:"Begining Java",author:"Budi Budiman"})
015-02-22T12:16:39.382+0700 I INDEX    [conn2] allocating new ns file /data/db/books.ns, filling with zeroes...
2015-02-22T12:16:40.033+0700 I STORAGE  [FileAllocator] allocating new datafile /data/db/books.0, filling with zeroes...
2015-02-22T12:16:40.960+0700 I STORAGE  [FileAllocator] done allocating datafile /data/db/books.0, size: 64MB,  took 0.918 secs
2015-02-22T12:16:41.317+0700 I WRITE    [conn2] insert books.users query: { _id: ObjectId('54e966379d836e67fa51813d'), title: "Begining Java", author: "Budi" } ninserted:1 keyUpdates:0 writeConflicts:0 numYields:0 locks:{ Global: { acquireCount: { w: 2 } }, MMAPV1Journal: { acquireCount: { w: 8 }, acquireWaitCount: { w: 2 }, timeAcquiringMicros: { w: 6268 } }, Database: { acquireCount: { w: 1, W: 1 } }, Collection: { acquireCount: { W: 1 } }, Metadata: { acquireCount: { W: 4 } } } 2031ms
2015-02-22T12:16:41.317+0700 I COMMAND  [conn2] command books.$cmd command: insert { insert: "users", documents: [ { _id: ObjectId('54e966379d836e67fa51813d'), title: "Begining Java", author: "Budi" } ], ordered: true } keyUpdates:0 writeConflicts:0 numYields:0 reslen:40 locks:{} 2040ms
WriteResult({ "nInserted" : 1 })
```

Saat anda memberikan perintah `use books`, database `books` belum ada dan belum dibuat.
Saat anda mencoba memasukan satu data dengan perintah `db.<nama-database>.insert({data})` barulah database dibuat dan data disimpan dalam disk.
	
Direktori `/data/db` sekarang berisi file & direktori seperti ini:

```
_tmp/
books.0
books.ns
journal/
local.0
local.ns
mongod.lock*
storage.bson
```

Untuk menghapus database yang sudah kita buat, gunakan perintah `db.dropDatabase()` setelah kita memilih database dengan perintah `use NAMA_DATABASE`

```
> use ejlp
switched to db ejlp
> db.dropDatabase()
2015-02-22T15:36:43.714+0700 I COMMAND  [conn3] dropDatabase ejlp starting
2015-02-22T15:36:43.729+0700 I JOURNAL  [conn3] journalCleanup...
2015-02-22T15:36:43.729+0700 I JOURNAL  [conn3] removeJournalFiles
2015-02-22T15:36:43.783+0700 I JOURNAL  [conn3] journalCleanup...
2015-02-22T15:36:43.783+0700 I JOURNAL  [conn3] removeJournalFiles
2015-02-22T15:36:43.817+0700 I COMMAND  [conn3] dropDatabase ejlp finished
2015-02-22T15:36:43.817+0700 I COMMAND  [conn3] command ejlp.$cmd command: dropDatabase { dropDatabase: 1.0 } keyUpdates:0 writeConflicts:0 numYields:0 reslen:55 locks:{} 139ms
{ "dropped" : "ejlp", "ok" : 1 }
```

### Insert data

Sekarang kita coba lihat data yang ada di database `books` dengan perintah `db.NAMA_DATABASE.find()` seperti dibawah ini:

```
> db.books.find()
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi Budiman" }
```

Sekarang kita coba insert data dengan format data yang sedikit berbeda yaitu menambahkan field `publisher` yang di data sebelumnya tidak ada. Apakah akan error saat insert? 


```
> db.books.insert({title:"Begining MongoDB",author:"Wati Irawati",publisher:"Gramereka"})
WriteResult({ "nInserted" : 1 })
> db.books.find()
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi Budiman" }
{ "_id" : ObjectId("54e96d309d836e67fa51813f"), "title" : "Begining MongoDB", "author" : "Wati Irawati", "publisher" : "Gramereka" }
```

Sekarang kita coba meng-insert dengan dokumen (data) yang memiliki format sangat berbeda

```
> db.books.insert({title:"Big Data Explained",author:"Wati Irawati",publisher:"Tempedia",edition: [{"1" : "1996"},{"2" : "2015"}] })
WriteResult({ "nInserted" : 1 })
```

Kita bisa lihat suatu collection, dalam hal ini `books` dapat diisi oleh dokumen format JSON apa saja yang formatnya berbeda-beda.

```
> db.books.find()
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi" }
{ "_id" : ObjectId("54e96d309d836e67fa51813f"), "title" : "Begining MongoDB", "author" : "Wati Irawati", "publisher" : "Gramereka" }
{ "_id" : ObjectId("54e9747f9d836e67fa518140"), "title" : "Big Data Explained", "author" : "Wati Irawati", "publisher" : "Tempedia", "edition" : [ { "1" : "1996" }, { "2" : "2015" } ] }
```

### Mencari record (search) dengan kriteria

Query dengan kriteria pencarian tertentu sangatlah umum dilakukan aplikasi. Sekarang kita coba cari data dengan tertentu dari data yang ada di database `books` dengan perintah `db.nama-database.find({kriteria})` 


```
> db.books.find({author: "Wati Irawati"})
{ "_id" : ObjectId("54e96d309d836e67fa51813f"), "title" : "Begining MongoDB", "author" : "Wati Irawati", "publisher" : "Gramereka" }
{ "_id" : ObjectId("54e9747f9d836e67fa518140"), "title" : "Big Data Explained", "author" : "Wati Irawati", "publisher" : "Tempedia", "edition" : [ { "1" : "1996" }, { "2" : "2015" } ] }
```

Hasilnya sesuai harapan, yaitu didaptkan semua buku yang memiliki nilai `author` sama dengan "Wati Irawati".

Sekarang, coba kira cari semua buku yang publisher-nya bukan (not equal) "Gramereka"

```
> db.books.find({publisher: { $ne : "Gramereka"}})
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi" }
{ "_id" : ObjectId("54e96d309d836e67fa51813f"), "title" : "Begining MongoDB", "author" : "Wati Irawati", "publisher" : "Tahumedia", "edition" : [ { "1" : "1996" }, { "2" : "2015" } ] }
```

Atau coba cari semua buku yang tidak memiliki data publihser

```
> db.books.find({publisher: { $exists : false}})
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi" }
```

Lebih lanjut mengenai operator yang dapat digunakan untuk query ini bisa dilihat di dokumentasi:
[Query and Projection Operators](http://docs.mongodb.org/manual/reference/operator/query/)

### Menggubah (update) record

Untuk mengupdate record yang ada, kita bisa menggunakan perintah `db.NAMA_COLLECTION.update({KRITERIA},{$set: {NILAI_BARU}})` dimana KRITERIA adalah statement pemilihan data, seperti kausul "WHERE" di SQL pada RDBMS.

Sekarang kita coba ubah data buku dengan athor "Wati Irawati" agar memiliki nilai publisher = 
"Tahumedia"

```
> db.books.update({author:"Wati Irawati"},{$set:{publisher:"Tahumedia"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
Kita lihat hasilnya:

```
> db.books.find()
{ "_id" : ObjectId("54e966d29d836e67fa51813e"), "title" : "Begining Java", "author" : "Budi Budiman" }
{ "_id" : ObjectId("54e96d309d836e67fa51813f"), "title" : "Begining MongoDB", "author" : "Wati Irawati", "publisher" : "Tahumedia" }
{ "_id" : ObjectId("54e9747f9d836e67fa518140"), "title" : "Big Data Explained", "author" : "Wati Irawati", "publisher" : "Tempedia", , "edition" : [ { "1" : "1996" }, { "2" : "2015" } ] }
```
	
Hasilnya adalah 1 record yang _match_ dan di-update, padahal kita tahu ada dua record yang memiliki field author dengan nama "Wati Irawati" Lihat di dokumentasi ini: [Collection Methods > db.collection.update()](http://docs.mongodb.org/manual/reference/method/db.collection.update/), anda bisa lihat ada option yang bisa ditambahkan di perintah update tersebut. Option apakah itu?


Ya, menggunakan option `multi`. Jadi perintah untuk mengubah semua record yang _match_ adalah seperti ini:

```
> db.books.update({author:"Wati Irawati"},{$set:{publisher:"Tahumedia"}},{multi: true})
```


### Menghapus record

Untuk menghapus record, kita bisa menggunakan sintak `db.NAMA_COLLECTION.remove({KRITERIA})`.
Sintak tersebut akan **menghapus semua record yang match dengan KRITERIA**. Hati-hati juga jika kita tidak menuliskan KRITERIA, berarti semua record dalam satu collection akan dihapus.

```
> db.books.remove({author:"Budi"})
WriteResult({ "nRemoved" : 1 })
```

Berikut cara menghapus semua record:

```
> db.books.remove({})
WriteResult({ "nRemoved" : 2 })
```

### Mengambil sebagian field dari dokumen dengan query find()

Dalam contoh diatas, anda sudah melihat bahwa setiap kali sintak `db.NAMA_COLLECTION.find()` dijalankan, maka satu dokumen (semua fields) akan ditampilkan. Bagaimana jika kita hanya ingin mengambil sebagian data dari dokumen saja? 

Menggambil data sebagian disebut *projection*. Hal ini bisa dilakukan dengan perintah seperti ini
`db.COLLECTION_NAME.find({KRITERIA},{KEY:OPSI, KEY:OPSI, ...}) ` dimana KEY adalah key yang ingin ditampilkan atau dihilangkan sedanakan OPSI adalah nilai 1 atau 0. 1 untuk menampilkan dan 0 untuk menhilangkan.

Sekarang kita coba, insert dulu beberapa data seperti berikut: 

```
> db.books.insert({title:"Big Data Explained",author:"Wati Irawati",publisher:"Tempedia",edition: [{"1" : "1996"},{"2" : "2015"}] })
> db.books.insert({title:"Belajar MongoDB",author:"Kinoy Suganteng",publisher:"Tempedia",edition: [{"1" : "2014"},{"2" : "2015"}] })
> db.books.insert({title:"Belajar NoSQL",author:"Melani Cicantik",publisher:"Tahumedia",edition: [{"1" : "2014"}] })
```

Sekarang kita coba find() dengan hanya menampilkan field title saja:

```
> db.books.find({},{title:1})
{ "_id" : ObjectId("54e9bf7a2c29b50e91e5e2a4"), "title" : "Big Data Explained" }
{ "_id" : ObjectId("54e9bfa22c29b50e91e5e2a5"), "title" : "Belajar MongoDB" }
{ "_id" : ObjectId("54e9bfe72c29b50e91e5e2a6"), "title" : "Belajar NoSQL" }
```

Kita lihat field `_id` secara default akan ditampilkan. Jika kita tidak ingin menampilkan field `_id` maka secara eksplisit harus kita tulis dengan opsi 0.

Sekarang kita coba find() semua record tanpa menampilkan field `_id` dan field `edition`

```
> db.books.find({},{_id:0, edition:0})
{ "title" : "Big Data Explained", "author" : "Wati Irawati", "publisher" : "Tempedia" }
{ "title" : "Belajar MongoDB", "author" : "Kinoy Suganteng", "publisher" : "Tempedia" }
{ "title" : "Belajar NoSQL", "author" : "Melani Cicantik", "publisher" : "Tahumedia" }
```


### Insert banyak dokumen untuk test

Sebelum lanjut latihan berikutnya, sebaiknya kita coba untuk insert banyak data ke MongoDB untuk test. Kita akan insert beberapa juta dokumen yang akan memerlukan disk sebesar kira-kira 1.9GB dengan format seperti ini:

```
{ 
  "created_on" : ISODate("2012-01-01T10:48:14.071Z"), 
  "value" : 0.16771210124716163 
}
```

Caranya adalah sebagai berikut. 
Kita dapat membuat script dengan menggunakan JavaScript untuk dijalankan oleh `mongo`. Script tersebut akan melakukan iterasi, membuat dokumen seperti format diatas dan dimasukan ke sebuah array.
Array tersebut akan di-insert untuk secara batch meng-insert tiap-tiap dokumen ke database MongoDB pada collection `randomData`.


Mulailah dengan membuatlah file `create_random.js` dengan isi seperti ini: 


```
var minDate = new Date(2012, 0, 1, 0, 0, 0, 0);
var maxDate = new Date(2013, 0, 1, 0, 0, 0, 0);
var delta = maxDate.getTime() - minDate.getTime();
 
var job_id = arg2;
 
var documentNumber = arg1;
var batchNumber = 5 * 1000;
 
var job_name = 'Job#' + job_id
var start = new Date();
 
var batchDocuments = new Array();
var index = 0;
 
while(index < documentNumber) {
    var date = new Date(minDate.getTime() + Math.random() * delta);
    var value = Math.random();
    var document = {       
        created_on : date,
        value : value
    };
    batchDocuments[index % batchNumber] = document;
    if((index + 1) % batchNumber == 0) {
        db.randomData.insert(batchDocuments);
    }
    index++;
    if(index % 100000 == 0) {  
        print(job_name + ' inserted ' + index + ' documents.');
    }
}
print(job_name + ' inserted ' + documentNumber + ' in ' + (new Date() - start)/1000.0 + 's');
```

> Source code file tersebut diambil dari: [https://github.com/vladmihalcea/vladmihalcea.wordpress.com/blob/master/mongodb-facts/data-generator/timeseries/create_random.js]()

Setelah itu jalankan scipt tersebut untuk meng-insert  sebaganyak 5juta records dengan menset arg1=5000000.

```
mongo random --eval "var arg1=5000000;arg2=1" create_random.js
```

Setelah selesai insert, kita cek 


```
> show dbs
books   0.078GB
local   0.078GB
random  1.953GB
> show collections
randomData
system.indexes
```

Kita cek datanya, secara default `mongo` hanya menampilkan 20 records dan untuk melihat 20 records berikutnya kita bisa memberikan perintah `it` (iterate):

```
> db.randomData.find()
{ "_id" : ObjectId("54e9ed5f7239ec12c95bcca1"), "created_on" : ISODate("2012-09-04T01:08:59.201Z"), "value" : 0.3521699025295675 }
...<DIHAPUS KARENA DATA BANYAK SEKALI>
{ "_id" : ObjectId("54e9ed5f7239ec12c95bcca2"), "created_on" : ISODate("2012-02-23T15:26:00.446Z"), "value" : 0.35164571506902575 }
Type "it" for more
> it
```

Sekarang kita sudah mempunyai banyak records dan bisa lanjut dengan latihan berikutnya menggunakan records tersebut.
	
### Membatasi jumlah record saat query

Untuk membatasi jumlah record, kita bisa menggunakan sintak berikut `db.NAMA_COLLECTION.find().limit(JUMLAH_RECORDS)`

Cobalah untuk menjalankan perintah ini:

```
> db.randomData.find().limit(5)
```

### Skip record saat query 

Hasil dari suatu query dapat kita hilangkan beberapa record dengan method `skip(NOMOR_RECORD)`
Berikut beberapa contoh:

Mengambil 5 records dan menghilangkan record ke-2

```
> db.randomData.find().limit(5).skip(2)
```

Mengambil semua records dan menghilangkan record ke-1

```
> db.randomData.find().skip(1)
```

### Pengurutan (sort)

```
db.COLLECTION_NAME.find().sort({KEY:1})
```








