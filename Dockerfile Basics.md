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
Download tomcat packages → RUN\
Extract tar.gz file → RUN\
Rename to tomcat directory → RUN\
Tell to docker that it runs on port 8080 → EXPOSE\
Start tomcat service → CMD
```bash
# Use Amazon Linux 2 as the base image
FROM amazonlinux:2

# Maintainer information
MAINTAINER "Naveen"

# Install OpenJDK 11 using Amazon Linux Extras
RUN amazon-linux-extras install java-openjdk11 -y

# Create a directory for Apache Tomcat in /opt
RUN mkdir /opt/tomcat

# Set the working directory to /opt/tomcat
WORKDIR /opt/tomcat

# Download Apache Tomcat 9.0.85 from the specified URL
RUN curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz

# Install tar utility
RUN yum install tar -y

# Extract the contents of the downloaded Apache Tomcat archive
RUN tar zxvf apache-tomcat-9.0.85.tar.gz

# Move the contents of the extracted directory to /opt/tomcat
RUN mv apache-tomcat-9.0.85/* /opt/tomcat/.

# Expose port 8080 for incoming HTTP traffic
EXPOSE 8080

# Start Tomcat using the specified command
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
