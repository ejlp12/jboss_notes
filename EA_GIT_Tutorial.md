# GIT Tutorial

```
$ mkdir newrepo.git
$ cd newrepo.git
$ git init --bare --shared=all
```

Membuat repository baru: Buat satu direktori, masuk ke direktori tersebut dan jalankan perintah `git init`

```
$ mkdir newrepo
$ cd newrepo
$ git init
$ touch README
$ git add README
$ git commit -m "initial commit"
$ git remote add origin https://github.com/ejlp12/repository.git
$ git push origin master
```



https://www.gitignore.io/

+ Create repository di github.com
+ Copy ULR, misalnya 

+ Buat file .gitignore yang isinya bisa digenerate dari [gitignore.io](https://www.gitignore.io/)



Mengecek remote git repository: `git remote -v`

Menambahkan semua file (rekursif) ke repository: `git add -A `
Melihat status perubahan yang akan dilakukan: `git status`
Meng-commit perubahan: `git commit -m "Komentar..."`
Push ke master: `git push origin master`
Mengganti file di lokal dengan file orisinal dari repository: `git checkout -- <filename>`
