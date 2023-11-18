### Dockerfile Basics

## Write 1 st Dockerfile
FROM: pull image from base\
RUN: execute commands(shell commands like install packages)\
CMD: to provide defaults for executing container (these commands can be overridden)\
ENTRYPOINT : to configure a container that will run as an executable(same as CMD but these commands can’t overridden)\
WORKDIR : to sets the working directory\
COPY : copy directory from local to docker container\
ADD : copy files and folder from local to docker container\
EXPOSE : informs Docker that the container listens on the specific network ports at runtime\
ENV : to set environment variables\
## Ex :: Install Tomcat on Centos
Pull centos from dockerhub  → FROM\
Install java → RUN\
create/opt/tomcat directory → RUN\
Change work directory to /opt/tomcat → WORKDIR\
Download tomcat packages → ADD / RUN\
Extract tar.gz file → RUN\
Rename to tomcat directory → RUN\
Tell to docker that it runs on port 8080 → EXPOSE\
Start tomcat service → CMD\
```bash
FROM centos:latest
RUN yum install java -y
RUN mkdir /opt/tomcat
WORKDIR /opt/tomcat
ADD wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.82/bin/apache-tomcat-8.5.82.tar.gz
RUN tar zxvf apache-tomcat-8.5.82.tar.gz
RUN mv apache-tomcat-8.5.82 /opt/tomcat
EXPOSE 8080
CMD ["/opt/tomcat/bin/catalina.sh","run"]
```
### ---OR---
## Building Docker Image From Scratch is not preffered → use official Docker Image
```bash
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
```
→ Build Image & Run it
```bash
docker build -t demo-tomcat .
docker run -d --name demo-tomcat-container -p 8081:8080 demo-tomcat
```