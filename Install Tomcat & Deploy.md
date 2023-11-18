### Setup Tomcat Server
Reffer this [Link](https://drive.google.com/file/d/1OojntSgG7hEOV9wPm4gcLuy0En4AaCYO/view?usp=sharing) to Setup Tomcat Server\
→ Setup a linux EC2 Instance\
→ Install Java
```bash
yum install java -y
java --version

openjdk 17.0.8.1 2023-08-22 LTS
OpenJDK Runtime Environment Corretto-17.0.8.8.1 (build 17.0.8.1+8-LTS)
OpenJDK 64-Bit Server VM Corretto-17.0.8.8.1 (build 17.0.8.1+8-LTS, mixed mode, sharing)
```
→ Install Tomcat
```bash
[root@ip-172-31-42-63 ~]# cd /opt

[root@ip-172-31-42-63 opt]# wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.81/bin/apache-tomcat-9.0.81.tar.gz

[root@ip-172-31-42-63 opt]# ls
apache-tomcat-9.0.81.tar.gz  aws  rh

[root@ip-172-31-42-63 opt]# tar -xvzf apache-tomcat-9.0.81.tar.gz 

[root@ip-172-31-42-63 opt]# ls
apache-tomcat-9.0.81  apache-tomcat-9.0.81.tar.gz  aws  rh

[root@ip-172-31-42-63 opt]# mv apache-tomcat-9.0.81 tomcat

[root@ip-172-31-42-63 opt]# ls
apache-tomcat-9.0.81.tar.gz  aws  rh  tomcat
```
→ Start Tomcat
```bash
[root@ip-172-31-42-63 opt]# cd tomcat/

[root@ip-172-31-42-63 tomcat]# cd bin/

[root@ip-172-31-42-63 bin]# ls
bootstrap.jar  catalina-tasks.xml  commons-daemon.jar            configtest.sh  digest.sh     setclasspath.bat  shutdown.sh  tomcat-juli.jar       tool-wrapper.sh
catalina.bat   ciphers.bat         commons-daemon-native.tar.gz  daemon.sh      makebase.bat  setclasspath.sh   startup.bat  tomcat-native.tar.gz  version.bat
catalina.sh    ciphers.sh          configtest.bat                digest.bat     makebase.sh   shutdown.bat      startup.sh   tool-wrapper.bat      version.sh

[root@ip-172-31-42-63 bin]# ./startup.sh 
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```
→ Open Port : 8080 \
→ http://3.109.5.133:8080 \
→ We can See Tomcat Home Page 

```bash
[root@ip-172-31-42-63 tomcat]# find / -name context.xml
/opt/tomcat/conf/context.xml
/opt/tomcat/webapps/docs/META-INF/context.xml
/opt/tomcat/webapps/examples/META-INF/context.xml
/opt/tomcat/webapps/host-manager/META-INF/context.xml
/opt/tomcat/webapps/manager/META-INF/context.xml
```
→ Comment Valve Tage in Both Files
```bash
vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
vi /opt/tomcat/webapps/manager/META-INF/context.xml
```
```bash
<?xml version="1.0" encoding="UTF-8"?>
<Context>
     "
  <!-- We are commenting value Tag bcs we are allowing every one to access manager-->
  <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
     "
</Context>           
```
→ Adding User's to Tomcat
```bash
[root@ip-172-31-42-63 tomcat]# find / -name tomcat-users.xml
/opt/tomcat/conf/tomcat-users.xml

[root@ip-172-31-42-63 tomcat]# vi /opt/tomcat/conf/tomcat-users.xml

# Add these user's in tomcat-users.xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
<user username="deployer" password="deployer" roles="manager-script"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
```
→ Restart Tomcat
```bash
[root@ip-172-31-42-63 bin]# cd /opt/tomcat/bin

[root@ip-172-31-42-63 bin]# ./shutdown.sh 
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED

[root@ip-172-31-42-63 bin]# ./startup.sh 
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```
→ We are able to see Tomcat Home Page & See Tomcat Manager Page

### Integrate Tomcat with Jenkins & Deploy Java(.war) Application to Tomcat
Reffer [Link](https://drive.google.com/file/d/1Ne39WNPa6j8xT4k-R13VlTgYi6ANOeny/view?usp=sharing) to Integrate Tomcat with Jenkins \
→ Install Deploy to Container Plugin\
→ Configure tomcat server with credentials\

### Automate build and deploy using Poll SCM
Reffer [Link](https://drive.google.com/file/d/1TgZWJkj7BlFBd5aj7SFUQrhEcw3gW0o5/view?usp=sharing) to Automate Build Using Tomcat \
→ Previously we manually build and deploy code on Tomcat \
→ We want Automated Process To execute Jenkins Job(build & deploy)\
→ Whenever there is a change in github it should automatically Identify and execute build job & deploy code \
→ In Job we use Build Triggers \
→ Poll SCM(if changes in code then job will execute) → schedule → ***** (min,hrs,day,dayofmonth,dayofweek)