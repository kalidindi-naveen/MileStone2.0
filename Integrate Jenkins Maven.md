### Integrate Maven With Jenkins
Reffer this [Link](https://drive.google.com/file/d/1sT87tUoTtd2VD0Vt7B33HzVGZYsn2sDc/view?usp=sharing) to Integrate Jenkins with Maven

→ Setup maven on jenkins Server (if we want we can build separate build server)
```bash
[root@ip-172-31-40-37 ~]# cd /opt

[root@ip-172-31-40-37 opt]# wget https://archive.apache.org/dist/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.tar.gz

[root@ip-172-31-40-37 opt]# ls
apache-maven-3.8.5-bin.tar.gz  aws  rh

[root@ip-172-31-40-37 opt]# tar -xvzf apache-maven-3.8.5-bin.tar.gz 

[root@ip-172-31-40-37 opt]# ls
apache-maven-3.8.5  apache-maven-3.8.5-bin.tar.gz  aws  rh

[root@ip-172-31-40-37 opt]# mv apache-maven-3.8.5 /usr/share/maven

[root@ip-172-31-40-37 opt]# vi /etc/profile

# Add These 3 Lines in /etc/profile
export M2_HOME=/usr/share/maven
export MAVEN_HOME=/usr/share/maven
export PATH=${M2_HOME}/bin:${PATH}

mvn -v

output:
Apache Maven 3.8.5 (3599d3414f046de2324203b78ddcf9b5e4388aa0)
Maven home: /usr/share/maven
Java version: 17.0.8.1, vendor: Amazon.com Inc., runtime: /usr/lib/jvm/java-17-amazon-corretto.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.196-185.743.amzn2.x86_64", arch: "amd64", family: "unix"
```
→ Install Maven Plugin in Jenkins\
→ Configure Maven and Java 

### Jenkins to Build Project
Reffer this [Link](https://drive.google.com/file/d/1XPf3JHQLbsvwexSKPjLBc9XfIGqd_QCd/view?usp=sharing) to Clone Code From Public GitHub Repository & Build .jar or .war file