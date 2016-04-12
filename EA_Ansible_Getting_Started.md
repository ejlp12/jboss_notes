

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

