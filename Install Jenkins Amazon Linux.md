### Create Security Group in AWS

Reffer this [Link](https://drive.google.com/file/d/1BubO_hgcT0mT3vHxatDJD3asZcefoOYs/view?usp=sharing) to create Security Group.


### Create Key-Pair (Login into EC2 Instance’s)

Reffer this [Link](https://drive.google.com/file/d/1p14dQgCuxhahfqZCd9ZJDFVt0pnbC5dH/view?usp=sharing) to create Key-Pair.

### Create EC2 Instance with AMI (Amazon Linux 2)

Reffer this [Link](https://drive.google.com/file/d/13QRPgY6WY30POy9diSlpymoTWmZPWFUK/view?usp=sharing) to Create EC2 Instance.

### Install Jenkins on Amazon Linux 2

Setup Jenkins Server ?? \
——————————\
1.setup linux ec2 instance\
2.install java\
3.install jenkins\
4.start jenkins\
5.Access Jenkins on port 8080

```bash
yum install java -y
java --version
output:
openjdk 17.0.8.1 2023-08-22 LTS
OpenJDK Runtime Environment Corretto-17.0.8.8.1 (build 17.0.8.1+8-LTS)
OpenJDK 64-Bit Server VM Corretto-17.0.8.8.1 (build 17.0.8.1+8-LTS, mixed mode, sharing)
```
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade -y
sudo yum install jenkins -y
sudo systemctl daemon-reload
```

```bash 
systemctl enable jenkins
systemctl start jenkins
systemctl status jenkins
```

Edit Inbound Rules in Security Group → Open Port: 8080\
Login to Browser with External IP Address

→ 3.109.1.91:8080

We Will See Jenkins Home Page

To get AdminPassword
```bash
[root@ip-172-31-3-167 ~]# cat /var/lib/jenkins/secrets/initialAdminPassword
output:
be3a0c7b2633431fbbe0a66fbc2b034c
```