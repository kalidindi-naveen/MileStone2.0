### Integrate Docker With Jenkins
Reffer this [Link](https://drive.google.com/file/d/1Zjwr9yTLxpDxY-Jsk6-d6usmiQYtsosP/view?usp=sharing) to Integrate Docker with Jenkins

```bash
cat /etc/passwd #check users
cat /etc/group #check groups
```

## Step1: Create a dockeradmin user & add dockeradmin to docker group
→ We need a dedicated user (ec2-user not recommended) & dockeradmin user added to docker group.
```bash
[root@ip-172-31-39-179 ~]# useradd dockeradmin
[root@ip-172-31-39-179 ~]# passwd dockeradmin
Changing password for user dockeradmin.
New password: 
BAD PASSWORD: The password contains the user name in some form
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@ip-172-31-39-179 ~]# id dockeradmin
uid=1001(dockeradmin) gid=1001(dockeradmin) groups=1001(dockeradmin)
[root@ip-172-31-39-179 ~]# usermod -aG docker dockeradmin
[root@ip-172-31-39-179 ~]# id dockeradmin
uid=1001(dockeradmin) gid=1001(dockeradmin) groups=1001(dockeradmin),992(docker)
```

## Enable password based authentication for EC2

## Step2: Install “publish over ssh” plugin
→ we need to copy artifacts from jenkins to docker host.

## Step3: Add Dockerhost to Jenkins "Configure Systems"
→ we can operate jenkins from dockerhost

## Jenkins job to build and copy artifacts on to dockerhost
Reffer [Link](https://drive.google.com/file/d/1T6ebz8jpXFM1snM9SEoM9x51xp21DUM9/view?usp=sharing) to copy artifacts to DockerHost as dockeradmin in /home/dockeradmin location.

```bash
[dockeradmin@ip-172-31-39-179 ~]$ ls
home

[dockeradmin@ip-172-31-39-179 ~]$ cd home/

[dockeradmin@ip-172-31-39-179 home]$ ls
dockeradmin

[dockeradmin@ip-172-31-39-179 home]$ cd dockeradmin/

[dockeradmin@ip-172-31-39-179 dockeradmin]$ ls
webapp.war
```
## Tomcat Dockerfile to Manually Deployment Process
Reffer this [Link](https://drive.google.com/file/d/1U-9pYgaJRp-KS1LELYLmYHlokBNwkJQD/view?usp=sharing) for Tomcat Deployment Process in Docker Container

```bash
[ec2-user@ip-172-31-39-179 ~]$ sudo -i

[root@ip-172-31-39-179 ~]# cd /opt

[root@ip-172-31-39-179 opt]# ls
aws  containerd  rh

[root@ip-172-31-39-179 opt]# mkdir docker

[root@ip-172-31-39-179 opt]# ll
total 0
drwxr-xr-x 4 root root 33 Sep 26 16:19 aws
drwx--x--x 4 root root 28 Oct 11 14:43 containerd
drwxr-xr-x 2 root root  6 Oct 12 07:14 docker
drwxr-xr-x 2 root root  6 Aug 16  2018 rh

[root@ip-172-31-39-179 opt]# chown dockeradmin:dockeradmin -R docker

[root@ip-172-31-39-179 opt]# ll
total 0
drwxr-xr-x 4 root        root        33 Sep 26 16:19 aws
drwx--x--x 4 root        root        28 Oct 11 14:43 containerd
drwxr-xr-x 2 dockeradmin dockeradmin  6 Oct 12 07:14 docker
drwxr-xr-x 2 root        root         6 Aug 16  2018 rh

[root@ip-172-31-39-179 opt]# cd docker/

[root@ip-172-31-39-179 docker]# vi Dockerfile

FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps

[root@ip-172-31-39-179 docker]# ll
total 4
-rw-r--r-- 1 root root 88 Oct 12 07:18 Dockerfile

[root@ip-172-31-39-179 docker]# chown -R dockeradmin:dockeradmin Dockerfile 

[root@ip-172-31-39-179 docker]# ll
total 4
-rw-r--r-- 1 dockeradmin dockeradmin 88 Oct 12 07:18 Dockerfile
```
→ After Executing Pipeline we are able to see webapp.war
```bash
[root@ip-172-31-39-179 docker]# ls
Dockerfile  webapp.war

[root@ip-172-31-39-179 docker]# ll
total 8
-rw-r--r-- 1 dockeradmin dockeradmin   88 Oct 12 07:18 Dockerfile
-rw-rw-r-- 1 dockeradmin dockeradmin 2362 Oct 12 07:20 webapp.war

[root@ip-172-31-39-179 docker]# whoami
root
```
→ Build Image & Run Image
```bash
[root@ip-172-31-39-179 docker]# docker build -t tomcat:v1 .

[root@ip-172-31-39-179 docker]# docker run -d --name tomcatv1 -p 8080:8080 tomcat:v1
4e37aaa96504fe2d429aca8176bb38ea68c828747f6d93f240a3144caadd9ff3
```
→ Open Browser and hit URL http://13.235.104.34:8080/webapp \
→ we can see Application homepage

## Tomcat Dockerfile to Automatically Deployment Process
```bash
[root@ip-172-31-39-179 docker]# docker ps
CONTAINER ID   IMAGE       COMMAND             CREATED         STATUS         PORTS                                       NAMES
a692924cdc34   tomcat:v1   "catalina.sh run"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   tomcatv1
```
→ After Modifying Exec Command
```bash
[root@ip-172-31-39-179 docker]# docker ps
CONTAINER ID   IMAGE       COMMAND             CREATED          STATUS          PORTS                                       NAMES
b98323938a62   tomcat:v1   "catalina.sh run"   48 seconds ago   Up 47 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   tomcatv1
```
→ Open Browser and hit URL http://13.235.104.34:8080/webapp \
→ we can see Application homepage