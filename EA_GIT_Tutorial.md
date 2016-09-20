# GIT Tutorial

Mau belajar GIT? berikut video tutorial yang cukup bagus (tidak bertele-tele) untuk memulai belajar praktek menggunakan GIT dan GITHUB:
* https://www.youtube.com/playlist?list=PLfdtiltiRHWFEbt9V04NrbmksLV4Pdf3j

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

2. Enable git untuk direktori project

   Di local directory project lakukan perintah berikut:
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
- Update repository lokal: `git pull origin master` atau `git pull`
- Resolve konflik perbedaan file: `git mergetool`
- Menambahkan semua file (rekursif) ke repository: `git add -A `
- Melihat status perubahan yang akan dilakukan: `git status`
- Meng-commit perubahan: `git commit -m "Komentar..."`
- Push ke master: `git push origin master`
- Mengganti file di lokal dengan file orisinal dari repository: `git checkout -- <filename>`

- Membuat tag: `git tag <tag-name>`
- Melihat daftar tag yang ada `git tag`
- Pindah ke suatu tag: `git checkout <tag-name>`
- Push tag ke git repository: `git push --tag` atau `git push origin <tag-name>`

- Membuat branch: `git branch <branch-name>`
- Pindah ke branch working directory: `git checkout <branch-name>`
- Melihat branch name dari working directory: `git branch` atau `git status`
- Push branch ke git repository: `git push origin <branch-name>`
- Menghapus branch di lokal: `git branch -d <branch-name>`
- Menghapus branch di git repository: `git push origin --delete <branch-name>`

### One time setup
```
git config --global user.name "ejlp12"
git config --global user.email "ejlp12@gmail.com"
```
