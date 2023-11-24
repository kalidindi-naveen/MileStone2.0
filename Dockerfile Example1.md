## Dockerfile Commands
→ Remove all images
```
docker system prune -a
```
## Create Custom Images
```
→ Dockerfile
Dockerfile is a Declarative way to create own images
Docker will give us some syntax to create our own images.
File name should be Dockerfile. 
Docker command should run where your Dockerfile exists.
```
## FROM
```
From or ARG should be first instruction on Dockerfile
```
#### Dockerfile
```
FROM almalinux
```
```
→ docker build -t docker.io/naveen2809/from:v1 .
(or) by default docker.io not necessary 
→ docker build -t naveen2809/from:v1 .
```
```
→ docker login
Authenticating with existing credentials...
Login Succeeded

→ docker push naveen2809/from:v1
The push refers to repository [docker.io/naveen2809/from]
96dbe1e77d15: Mounted from library/almalinux
v1: digest: sha256:8a2d66c1dc3d36c0700cb82981862c71a96e751bb5f4874abc7144b45986cc2b size: 528
```

## RUN
```
Run instructions we use to install software, packages and other tasks. It runs at the time of image building.
```
#### Dockerfile
```
FROM naveen2809/from:v1
RUN yum update -y
RUN yum install nginx -y
```
```
→ docker build -t naveen2809/run:v1 . 
→ docker run -itd -p 80:80 naveen2809/run:v1
```

## CMD
```
CMD is the instruction that runs the container. It should run in the foreground and it should run for infinite time.
```
## RUN vs CMD
```
RUN command will execute at the time of image creation.
CMD command will execute at the time of running the container.
```
#### Dockerfile
```
FROM naveen2809/from:v1
RUN yum install nginx -y
CMD ["nginx", "-g", "daemon off;"]
CMD ["sleep","20"]  // this will only execute bcs only 1 CMD instruction should be in Dockerfile
```
```
→ docker build -t naveen2809/cmd:v1 .
→ docker run -itd -p 80:80 naveen2809/cmd:v1 
→ docker exec -it <CID> bash
→ cd /usr/share/nginx/html
→ echo "Hello World" > hello.html
→ xx.xx.xx.xx/hello.html
```

## LABEL
```
LABEL instruction is useful to give some metadata information
```
#### Dockerfile
```
FROM naveen2809/from:v1
LABEL OWNER="NK" \
	   AGE="25"
```
```
→ docker build -t naveen2809/label:v1 .
→ docker inspect naveen2809/label:v1
```
```
 "OnBuild": null,
            "Labels": {
                "AGE": "25",
                "OWNER": "NK"
            }
```
```
→ docker images --filter "label=AGE=25"
REPOSITORY         TAG       IMAGE ID       CREATED       SIZE
naveen2809/label   v1        e5dece99e179   3 hours ago   184MB

→ docker images --filter "label=OWNER=NK"
REPOSITORY         TAG       IMAGE ID       CREATED       SIZE
naveen2809/label   v1        e5dece99e179   3 hours ago   184MB
```

## EXPOSE
```
EXPOSE tell which protocol/container port is opened for container or image opening
```
```
FROM naveen2809/from:v1
EXPOSE 8080/tcp
```
```
→ docker build -t naveen2809/expose:v1 .
→ docker inspect naveen2809/expose:v1
```
```
 "ExposedPorts": {
                "8080/tcp": {}
            },
```
```
→ docker run naveen2809/expose:v1 sleep 20
→ docker ps
CONTAINER ID   IMAGE                  COMMAND      CREATED         STATUS                  PORTS      NAMES
aba008f576da   naveen2809/expose:v1   "sleep 20"   2 seconds ago   Up Less than a second   8080/tcp   trusting_saha
```

## ENV
```
ENV is the instruction to provide environment variables to image and container. 
You can override env variables at runtime
```
```
FROM naveen2809/from:v1
ENV OWNER="NK"\
	   AGE="25"
```
```
→ docker build -t naveen2809/env:v1 .
docker inspect naveen2809/env:v1
```
```
"Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "OWNER=NK",
                "AGE=25"
            ],
```
```
→ docker run naveen2809/env:v1 env  // Run env cmd inside container
OWNER=NK
AGE=25
```
```
→ docker run -e AGE="22" naveen2809/env:v1 env // Ower riding ENV variables
OWNER=NK
AGE=22
```
```
→ docker ps -a
CONTAINER ID   IMAGE                  COMMAND      CREATED         STATUS                     PORTS     NAMES
4552869ee9a1   naveen2809/env:v1      "env"        2 minutes ago   Exited (0) 2 minutes ago             nice_jones
b4ee6b3d5664   naveen2809/env:v1      "env"        2 minutes ago   Exited (0) 2 minutes ago             exciting_cannon
```

## COPY
```
COPY instruction is useful to copy the files from local to image
```
```
FROM naveen2809/from:v1
RUN yum install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
COPY ./website /usr/share/nginx/html/
CMD ["nginx", "-g", "daemon off;"]
```
```
→ docker build -t naveen2809/copy:v1 .
→ docker run -itd -p 80:80 naveen2809/copy:v1
```

## ADD
```
ADD is the same as COPY
It is useful to copy files from local to container. 
But it has 2 extra capabilities.
ADD can download the file directly from the internet to the container.
ADD can untar/unzip the file directly into the container.
```
Note: we need to have apache-tomcat-9.0.83.tar.gz in current directory
``` 
FROM naveen2809/from:v1
ADD https://github.com/files-community/Files/blob/main/README.md /tmp/
ADD apache-tomcat-9.0.83.tar.gz  /tmp/
```
```
→ docker build -t naveen2809/add:v1 .
→ docker run -it naveen2809/add:v1
→ [root@6e58bda7de2e /]# cd /tmp
→ [root@6e58bda7de2e tmp]# ls -al
total 156
drwxrwxrwt 1 root root   4096 Nov 23 13:37 .
drwxr-xr-x 1 root root   4096 Nov 23 13:39 ..
-rw------- 1 root root 145759 Jan  1  1970 README.md
drwxr-xr-x 9 root root   4096 Nov 23 13:37 apache-tomcat-9.0.83
```

## ENTRYPOINT
```
ENTRYPOINT is also used to run the container just like CMD. But there are few differences.
We can't override ENTRYPOINT, but we can override CMD.
We can't override ENTRYPOINT, if you try to do so it will go and append to the ENTRYPOINT command.

If you use CMD and ENTRYPOINT and don't give any command from the terminal, CMD acts as an argument provider to ENTRYPOINT.
CMD will supply default arguments to ENTRYPOINT.
You can always override CMD arguments from runtime.
You can stop missusing your image with other commands.
```
```
FROM naveen2809/from:v1
CMD ["ping", "-c5", "google.com"]
```
```
→ docker build -t naveen2809/entry:v1 .

→ docker run naveen2809/entry:v1
→ ping google.com 5 times

# Overridding CMD
→ docker run naveen2809/entry:v1 ping -c 5 facebook.com
→ ping facebook.com 5 times
```
```
FROM naveen2809/from:v1
ENTRYPOINT ["ping", "-c5",”google.com”]
```
```
→ docker build -t naveen2809/entry:v1 .

→ docker run naveen2809/entry:v1
→ ping google.com 5 times

# Can't Override ENTRYPOINT (append at end)
→ docker run naveen2809/entry:v1 ping -c 5 facebook.com
→ Not ping facebook.com 5 times (show error)
o/p CMD: ping -c5 google.com ping -c2 facebook.com
```
```
FROM naveen2809/from:v1
CMD ["google.com"]
ENTRYPOINT ["ping", "-c5"]
```
```
→ docker build -t naveen2809/entry:v1 .

→ docker run naveen2809/entry:v1
→ ping google.com 5 times

→ docker run naveen2809/entry:v1 facebook.com
→ ping facebook.com 5 times
```

## USER
```
USER is used to run the commands as a particular user.
```
```
FROM naveen2809/from:v1
RUN adduser nginx-nk
USER nginx-nk
RUN touch /tmp/hello.txt
```
```
→ docker build -t naveen2809/user:v1 .

→ docker run -it naveen2809/user:v1
→ [nginx-nk@c0c0ffdbf47b /]$ whoami
nginx-nk
→ [nginx-nk@c0c0ffdbf47b /]$ cd /tmp
→ [nginx-nk@c0c0ffdbf47b tmp]$ ls -l
total 0
-rw-r--r-- 1 nginx-nk nginx-nk 0 Nov 23 13:56 hello.txt
```

## WORKDIR 
```
WORKDIR is used to set the path to the image while creating.
```
```
FROM naveen2809/from:v1
WORKDIR /tmp
RUN touch hello.txt
```
```
→ docker build -t naveen2809/work:v1 .
→ docker run -it naveen2809/work:v1
→  [root@602b2b6a556a tmp]# pwd
/tmp
```

## ARG
```
ARG is used to supply a few variables at the image creation.

ARG is the only instruction you can use before FROM. ARG declared before cant be accessed after FROM.

Using ENV and ARG for best results.
Create one env variable and assign the value of ARG to that.
Then we can access ARG values through ENV both in image and container.
```
```
ARG VERSION
FROM naveen2809/from:${VERSION:-v1}
ARG GREETING="HI GM" # default value
ENV GREETING=${GREETING}
RUN echo "$GREETING" # print HI GM
RUN echo "$VERSION" # we are not getting this value
```
```
→ default v1 if we want we change version as v2
→ docker build -t naveen2809/arg:v1 --build-arg VERSION=v2 .
or 
→ docker build -t naveen2809/arg:v1 --build-arg VERSION=v1  --build-arg GREET=Hello .
docker run naveen2809/arg:v1
```

## ONBUILD
```
ONBUILD is used to set some hard guidelines to the image. 
We can control how others can use our image as their base image
```
```
FROM naveen2809/from:v1
RUN yum install nginx -y
# When the image creator is running this below command will not run. when someone else tries to use your image then it will run
# On building Image from this Image(as Base Image) we need to have simple.txt file
ONBUILD ADD simple.txt /tmp/
```
```
→ docker build -t naveen2809/onbuild:v1 .
```
```
From naveen2809/onbuild:v1 
```
```
→ docker build -t naveen2809/onuse:v1 .  # fail because we need to have simple.txt file

 => ERROR [2/1] ADD simple.txt /tmp/                                                                               0.0s
------
 > [2/1] ADD simple.txt /tmp/:
------
Dockerfile:1
--------------------
   1 | >>> From naveen2809/onbuild:v1
--------------------
ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref 9a190181-7fff-41f7-957d-21aefd820f8e::ggswiqvn3m2m1spxgk5wovx48: "/simple.txt": not found
```

## STOPSIGNAL
```
STOPSIGNAL is used to know how to exit the container.
```
```
By default docker requests for exit and wait for some time.
If it is not exiting it can force kill. →→ Not Right way

When your container received STOPSIGNAL your application can perform
→ close DB connections.
→ do some back up.
```