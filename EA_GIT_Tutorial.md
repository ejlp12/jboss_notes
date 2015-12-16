# GIT Tutorial

```
$ mkdir newrepo.git
$ cd newrepo.git
$ git init --bare --shared=all
```

### Membuat repository baru

Buat satu direktori, masuk ke direktori tersebut dan jalankan perintah `git init`

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

### Memasukan existing project ke GitHub:

1. Buat repository di GitHub

Di local directory project lakukan perintah berikut:

2. Enable git untuk direktori project:
```
$ cd new_project
$ git init
```
3. Buat file `.gitignore` yang isinya bisa digenerate dari [gitignore.io](https://www.gitignore.io/)

4. Masukan semua file ke git
```
$ git add .
$ git commit -m "Initial commit"
```
5. Set remote git repository
```
$ git remote add origin https://github.com/ejlp12/nama_project.git
```
6. Push semua file ke remote git repository
```
$ git push origin master
```



### Beberapa perintah yang sering digunakan

- Mengecek remote git repository: `git remote -v`
- Update repository lokal: `git pull origin master`
- Resolve konflik perbedaan file: `git mergetool`
- Menambahkan semua file (rekursif) ke repository: `git add -A `
- Melihat status perubahan yang akan dilakukan: `git status`
- Meng-commit perubahan: `git commit -m "Komentar..."`
- Push ke master: `git push origin master`
- Mengganti file di lokal dengan file orisinal dari repository: `git checkout -- <filename>`

