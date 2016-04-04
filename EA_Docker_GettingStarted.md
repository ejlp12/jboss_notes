Docker on Mac

```
brew update
brew install docker
brew install boot2docker
boot2docker init


boot2docker up
```

```
ejlp12-host:bin eariobow$ brew update
Initialized empty Git repository in /usr/local/.git/
remote: Counting objects: 212028, done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 212028 (delta 8), reused 10 (delta 7)
Receiving objects: 100% (212028/212028), 46.36 MiB | 92.00 KiB/s, done.
Resolving deltas: 100% (154522/154522), done.
From https://github.com/Homebrew/homebrew
 * [new branch]      gh-pages   -> origin/gh-pages
 * [new branch]      go         -> origin/go
 * [new branch]      master     -> origin/master
HEAD is now at 7ed0583 Exclude documentation from `brew list --unbrewed`
Already up-to-date.
```

```
ejlp12-host:bin eariobow$ brew install docker
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/docker-1.3.1.mavericks.bottle.tar.gz
######################################################################## 100.0%
==> Pouring docker-1.3.1.mavericks.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/docker/1.3.1: 9 files, 6.8M
```

```
ejlp12-host:bin eariobow$ brew install boot2docker
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/boot2docker-1.3.1.mavericks.bottle.3.tar.gz
######################################################################## 100.0%
==> Pouring boot2docker-1.3.1.mavericks.bottle.3.tar.gz
==> Caveats
To have launchd start boot2docker at login:
    ln -sfv /usr/local/opt/boot2docker/*.plist ~/Library/LaunchAgents
Then to load boot2docker now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.boot2docker.plist
==> Summary
🍺  /usr/local/Cellar/boot2docker/1.3.1: 3 files, 7.2M
```

```
ejlp12-host:bin eariobow$ boot2docker init
Latest release for boot2docker/boot2docker is v1.3.1
Downloading boot2docker ISO image...
Success: downloaded https://github.com/boot2docker/boot2docker/releases/download/v1.3.1/boot2docker.iso
	to /Users/eariobow/.boot2docker/boot2docker.iso
Generating public/private rsa key pair.
Your identification has been saved in /Users/eariobow/.ssh/id_boot2docker.
Your public key has been saved in /Users/eariobow/.ssh/id_boot2docker.pub.
The key fingerprint is:
79:3f:8e:e2:51:0d:31:6b:01:37:08:af:99:3e:72:31 eariobow@ejlp12-host.local
The key's randomart image is:
+--[ RSA 2048]----+
|      ...o*      |
|       ... *     |
|        . +      |
|       + o o     |
|      E S o .    |
|     . o o .     |
|    . + .   o    |
|     o ... o .   |
|       .... .    |
+-----------------+
ejlp12-host:bin eariobow$ boot2docker up
Waiting for VM and Docker daemon to start...
..........................oooooooooooooo
Started.
Writing /Users/eariobow/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/eariobow/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/eariobow/.boot2docker/certs/boot2docker-vm/key.pem

To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://192.168.59.103:2376
    export DOCKER_CERT_PATH=/Users/eariobow/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1
```

```
ejlp12-host:bin eariobow$ export DOCKER_HOST=tcp://192.168.59.103:2376
ejlp12-host:bin eariobow$ export DOCKER_CERT_PATH=/Users/eariobow/.boot2docker/certs/boot2docker-vm
```

```
ejlp12-host:bin eariobow$ docker info
Containers: 0
Images: 0
Storage Driver: aufs
 Root Dir: /mnt/sda1/var/lib/docker/aufs
 Dirs: 0
Execution Driver: native-0.2
Kernel Version: 3.16.4-tinycore64
Operating System: Boot2Docker 1.3.1 (TCL 5.4); master : 9a31a68 - Fri Oct 31 03:14:34 UTC 2014
Debug mode (server): true
Debug mode (client): false
Fds: 10
Goroutines: 11
EventsListeners: 0
Init Path: /usr/local/bin/docker
```

```
ejlp12-host:bin eariobow$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

```
ejlp12-host:bin eariobow$ docker run -d -P --name web nginx
Unable to find image 'nginx' locally
nginx:latest: The image you are pulling has been verified
511136ea3c5a: Pull complete 
f10807909bc5: Pull complete 
f6fab3b798be: Pull complete 
d21beea329f5: Pull complete 
04499cf33a0e: Pull complete 
34806d38e48d: Pull complete 
4cae2a7ca6bb: Pull complete 
23f7e46a4bbc: Pull complete 
9dfd3384699f: Pull complete 
475220486d0e: Pull complete 
30bb1926e17f: Pull complete 
ef45dc12127b: Pull complete 
e426f6ef897e: Pull complete 
Status: Downloaded newer image for nginx:latest
458288e1586b57dce773b52577682781ba66fbadcfce4e6a7f856c2ef33fba66
```

```
ejlp12-host:bin eariobow$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                                           NAMES
458288e1586b        nginx:latest        "nginx -g 'daemon of   38 minutes ago      Up 9 minutes        0.0.0.0:49153->443/tcp, 0.0.0.0:49154->80/tcp   web
```

```
ejlp12-host:bin eariobow$ echo $(boot2docker ip 2>/dev/null) dockerhost | sudo tee -a /etc/hosts
Password:
192.168.59.103 dockerhost
```

####################################################################

boot2docker up  
boot2docker down

Setelah boot2docker jalan dengan perintah `boot2docker up`, setiap kali anda akan berintaksi dengan docker pada shell/terminal yang lain. Jalankan

`boot2docker shellinit`

Untuk melihat images yang tersedia di repository lokal:
`docker images`

```
REPOSITORY              TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
jpetazzo/nsenter        latest              6ed3da1d7fa6        5 weeks ago         367.7 MB
nginx                   latest              e426f6ef897e        8 weeks ago         100.2 MB
centos                  centos6             70441cac1ed5        8 weeks ago         215.8 MB
jboss/base-jdk          7                   90832e1f0bb9        11 weeks ago        813.7 MB
jboss/base-jdk          8                   4be6902cb6bc        11 weeks ago        754.4 MB
```

Untuk melihat container yang sedang berjalan
`docker ps`

Untuk melihat semua container yang pernah dijalankan
`docker ps -a`


Untuk menjalankan image:
`docker run -i -t <repository_name:tag> <program>`

		docker run - Menjalankan sebuah container
		-t - Mengalokasikan sebuah (pseudo) tty
		-i - Membuat stdin tetap terbuka (sehingga kita bisa berinteraksi dengan container tsb)


Contoh:
`docker run -i -t -v /Users/eariobow:/data:rw jboss/base-jdk:7 /bin/bash`


docker run -d <container-id>

Commit suatu container menjadi image agar bisa dijalankan lagi
`docker commit <Container ID> <Name>:<Tag>`

`docker commit bd11af020339 jboss-eap-6.3:1.0`




TIPS: Tidak perlu menjalankan SSHD di container

Install docker-enter (utilities untuk mengakses container dan mendapatkan shell sehingga kita tidak
perlu menginstal SSHD di container)
`docker run --rm -v /usr/local/bin:/target jpetazzo/nsenter`

Jalankan container, misalnya `docker run -d -P nginx` lalu 

SSH ke VM
`boot2docker ssh`


```
ejlp12@ejlp12-host:~$ boot2docker ssh
                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.4.1, build master : 86f7ec8 - Tue Dec 16 23:11:29 UTC 2014
Docker version 1.4.1, build 5bc2ff8
docker@boot2docker:~$ sudo docker-enter e1216929ac7f
```



Di environment production container harus jalan selalu secara otomatis, tidak dijalankan manual dengan 
perintah, untuk itu perlu dibuat

https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/



To stop all currently running docker files, one can call:
`sudo docker stop $(sudo docker ps -a -q) `

To remove all created docker container files, one can call:
`sudo docker rm $(sudo docker ps -a -q)`

To get all IP address of running containers
`for i in $(sudo docker ps -q); do sudo docker inspect $i| grep IPAddr; done`


iptables -t nat -A  DOCKER -p tcp --dport 8002 -j DNAT --to-destination 172.17.0.19:8000
