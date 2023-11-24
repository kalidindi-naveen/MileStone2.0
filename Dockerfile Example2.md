## VOLUME 
### No need to define size of volume
### we can use -v or --mount attach volume to container
## Volume Types
```
Anonymous volumes →→ Not Managed by Docker
Docker volummes →→ Managed by Docker [Preffer this way]
```
```
Storing files in container will lost once delete container because container uses host storage (Docker containers are Ephemeral)
→ Solution: stateful applications we use Docker volumes
→ attach volume to container (if container deleted still data remains in volume)

Host Storage
→ sudo su -
→ cd /var/lib/docker/overlay2  # this folder used to store ephemeral data 

Docker Volumes
→ cd var/lib/docker/volumes
```
```
create volume 
→ docker volume create my-vol

see volume
→ docker volume ls
→ docker volume inspect my-vol
[
    {
        "CreatedAt": "2023-11-24T02:46:32Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": null,
        "Scope": "local"
    }
]

Attach volume to container
→ docker run -it --mount source=my-vol,destination=/appdata ubuntu
→ root@09378492160f:/# ls
appdata  boot  etc   lib    lib64   media  opt   root  sbin  sys  usr
bin      dev   home  lib32  libx32  mnt    proc  run   srv   tmp  var
→ root@09378492160f:/# cd /appdata/
→ root@09378492160f:/appdata# ls
→ root@09378492160f:/appdata# echo "F1 From ubuntu1">hello.html
→ root@09378492160f:/appdata# ls
hello.html
→ root@09378492160f:/appdata# exit
exit

→ Note:: It will create app2 folder
→ docker run -it --mount source=my-vol,destination=/app2 almalinux
→ [root@20e643331101 /]# pwd
/
→ [root@20e643331101 /]# ls
afs  app2  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
→ [root@20e643331101 /]# cd app2
→ [root@20e643331101 app2]# ls
hello.html
→ [root@20e643331101 app2]# exit
exit

---OR---

Attach volume to container
→ docker run -d -v my-vol:/usr/share/nginx/html -p 80:80 nginx
-v volume 
-p host-port: container port
-v host-path: container-path

In EC2 Instance
→ sudo su -
→ cd var/lib/docker/volumes/my-vol/_data
→ ls -l
→ echo "Hello" > hello.html
→ xx.xx.xx.xx/hello.html
```
```
Anonymous volumes (not managed by docker)

→ mkdir test-data
→ docker run -d -v /home/ec2-user/test-data:/usr/share/nginx/html -p 8080:80 nginx
→ cd /home/ec2-user/test-data
→ echo hi>hi.html
→ xx.xx.xx.xx:8080/hi.html
```
### Dockerfile
```
FROM ubuntu
RUN mkdir /my-vol
RUN echo "hello world" > /my-vol/greeting
VOLUME /my-vol
```
```
→ docker build -t nk/volume:v1 .
→ docker run -it nk/volume:v1
→ root@37ad38536e4a:/# pwd
/
→ root@37ad38536e4a:/# ls
bin   dev  home  lib32  libx32  mnt     opt   root  sbin  sys  usr
boot  etc  lib   lib64  media   my-vol  proc  run   srv   tmp  var
→ root@37ad38536e4a:/# cd my-vol/
→ root@37ad38536e4a:/my-vol# ls
greeting
→ root@37ad38536e4a:/my-vol# ls -al
total 12
drwxr-xr-x 2 root root 4096 Nov 24 03:48 .
drwxr-xr-x 1 root root 4096 Nov 24 03:48 ..
-rw-r--r-- 1 root root   12 Nov 24 03:47 greeting
```

## MYSQL Image
```
→ create volume
docker volume create my-sql-data

→docker run -d -v my-sql-data:/var/lib/mysql mysql 
→ it will excited or won’t run bcs we need to provide password for mysql
→ docker logs <CID>

→ docker run -d -v my-sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123 --name mysql-c1 mysql 

→ docker ps # now its running
→ docker exec -it mysql-c1 bash
→ mysql -u root -p root123 
→ create database student;
→ show databases;

→ sudo su -
→ cd var/lib/docker/volumes/my-sql-data/_data # entire data present here
→ docker run -d -v my-sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123 --name mysql-c2 mysql 
→ docker exec -it mysql-c2 bash
→ mysql -u root -p root123 
→ show databases;
```
```
We can attach 1 volume to 2 containers
→ docker volume ls
→ docker run -itd -p 80:80 -v my-vol:/usr/share/nginx/html nginx
→ docker run -itd -p 8080:80 -v my-vol:/usr/share/nginx/html nginx
→ xx.xx.xx.xx:80/hi.html
→ xx.xx.xx.xx:8080/hi.html
```