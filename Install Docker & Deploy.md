### Setup Docker Server
Reffer [Link](https://drive.google.com/file/d/15WUD3yAjFCKbn0wvAmmiGebrY8dlcu-i/view?usp=sharing) to setup Docker Server\
→ Setup a Linux EC2 Instance\
→ Install Docker
```bash
[root@ip-172-31-39-179 ~]# yum install docker -y

[root@ip-172-31-39-179 ~]# docker --version
Docker version 20.10.23, build 7155243
```
→ Start Docker Services
```bash
systemctl enable docker
systemctl start docker
systemctl status docker
```
### How To Create a Docker Container
→ To Create Docker Container We need Docker Image\
→ With help of docker image only we are able to create docker container\
→ We can get docker images in 2 ways\
1.Docker Hub \
2.Creating own docker file

###  Step1: Pull Image From Docker Hub
```bash
docker run -d --name tomcat-container -p 8081:8080 tomcat
```

→ http://3.109.182.69:8081/ \
→ IPv4:8081 we will get error(HTTP status 404 - Not Found) \
→ This issues was common for tomcat version > 9 in docker image \
→ solve copy/move files from webapp.dist to webapp

### Solve Tomcat Container Issue
```bash
[root@ip-172-31-39-179 ~]# docker exec -it tomcat-container /bin/bash

root@76d6bfca79a2:/usr/local/tomcat# ls
bin  BUILDING.txt  conf  CONTRIBUTING.md  lib  LICENSE  logs  native-jni-lib  NOTICE  README.md  RELEASE-NOTES  RUNNING.txt  temp  webapps  webapps.dist  work

root@76d6bfca79a2:/usr/local/tomcat# cd webapps

root@76d6bfca79a2:/usr/local/tomcat/webapps# ls

root@76d6bfca79a2:/usr/local/tomcat/webapps# cd ../webapps.dist/

root@76d6bfca79a2:/usr/local/tomcat/webapps.dist# ls
docs  examples  host-manager  manager  ROOT

root@76d6bfca79a2:/usr/local/tomcat/webapps.dist# cp -R * ../webapps

root@76d6bfca79a2:/usr/local/tomcat/webapps.dist# cd ../webapps

root@76d6bfca79a2:/usr/local/tomcat/webapps# ls
docs  examples  host-manager  manager  ROOT

root@76d6bfca79a2:/usr/local/tomcat/webapps# exit
```
→ we are able to see Tomcat Home Page in Browser

### Step2: Create Own Dockerfile

→ Dockerfile 
```bash
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
```
→ Build Image & Run it
```bash
docker build -t demo-tomcat .
docker run -d --name demo-tomcat-container -p 8081:8080 demo-tomcat
```

### Check wheather container running or not
```bash
docker ps -a
```
→ Check the Tomcat Home Page on browser (http://3.109.182.69:8081/)