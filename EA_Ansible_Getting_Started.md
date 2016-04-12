## Instalasi Ansible di Mac OS-X

Ansible adalah program untuk melakukan otomasi pekerjaan di beberapa mesin atau host yang dibuat menggunakan bahasa pemrograman Python. 

Untuk menginstal Ansible paling mudah di Mac OS-X adalah menggunakan Homebrew, caranya cukup satu baris perintah:

```
brew install ansible
```

Cara lain adalah dengan menginstall menggunakan *pip*

*pip* adalah sistem management pemaketan (package management system) yang digunakan untuk menginstal dan mengelola paket software (library atau module) yang dibuat menggunakan Python.

Biasanya di Mac OS-X terbaru, Python sudah terinstal (jika belum maka anda belum beruntung, silakan install Python terlebih dahulu) sehingga kita bisa langsung menginstall ansible dengan perintah berikut:

Upgrade pip:
```
sudo pip install --upgrade pip
```

Install ansible, jika ansible sudah ada maka perintah ini akan mengupgrade ansible ke versi terbaru:
```
sudo pip install ansible --upgrade
```

Setelah selesai, coba test ansible dengan perintah `ansible --version`

```
$ ansible --version
ansible 2.0.1.0
  config file =
  configured module search path = Default w/o overrides
```


## Memulai Menjalankan Ansible

Test ansible dengan menjalankan perintah satu baris berikut:

```
ansible all -i "localhost," -c local -m shell -a 'echo hello world'
```

Perintah tersebut akan menjalankan shell script `echo hello world` di localhost. Biasanya di lingkungan "Production" kita melakukan perintah ansible untuk mengeksekusi perintah dibeberapa remote hosts sehingga mempermudah pekerjaan manual berulang-ulang.

Sekarang kita coba menjalankan ansible dengan menggunakan *playbook*. Playbook adalah file 

Buat file dengan nama `helloworld.yml` di direktori

```
---
- hosts: all
  tasks:
  - shell: echo "hello world"
```

Jalankan ansible dengan playbook yang sudah anda buat:

```
ansible-playbook -i "localhost," -c local helloworld.yml 
```

Kita masih menyuruh ansible untuk menjalankan playbook di mesin localhost dengan opsi `-i`.

Buat file `hosts` yang mendefinisikan nama-nama host dan pengelompokan (grouping) hostname, di direktori `~/.ansible`

```
mkdir ~/.ansible
export ANSIBLE_HOSTS=~/.ansible/hosts
```

Kemudian isi file hosts tersebut dengan teks berikut:


```
localhost ansible_connection=local
```

Lalu sekarang jalankan ansible dengan perintah yang lebih sederhana:

```
ansible-playbook helloworld.yml
```

Perintah tersebut akan membaca file `~/.ansible/hosts` karena opsi `-i` tidak diberikan dan memparsing serta meneksekusi file `helloworld.yml` 

Ouput dari perintah tersebut adalah seperti ini:

```
PLAY [all] ********************************************************************

GATHERING FACTS ***************************************************************
ok: [localhost]

TASK: [shell echo "hello world"] **********************************************
changed: [localhost]

PLAY RECAP ********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

