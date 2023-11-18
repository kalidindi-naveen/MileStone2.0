## IAC
### AWS → AWS Cloud Formation
### AZURE → Azure Resource Manager (ARM) Template's
### GOOGLE → Google Cloud Deployment Manager
## Terraform → Cloud Neutral (AWS,GCP,AZURE,.....etc)

### Terraform use's API as Code Internally
### → HCL code convert into AWS/Azure/GCP API's
### Tools Installation
```bash
ubuntu@ip-172-31-3-80:~$ sudo -i
root@ip-172-31-3-80:~# pwd
/root
root@ip-172-31-3-80:~# aws --version
Command 'aws' not found, but can be installed with:
snap install aws-cli  # version 1.15.58, or
apt  install awscli   # version 1.22.34-1
See 'snap info aws-cli' for additional versions.
root@ip-172-31-3-80:~# terraform --version
Command 'terraform' not found, but can be installed with:
snap install terraform
```
## Install Terraform
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \

sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt-get install terraform

root@ip-172-31-3-80:~# terraform --version
Terraform v1.6.1 on linux_amd64
```

## Install AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
```bash
apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
You can now run: /usr/local/bin/aws --version

root@ip-172-31-3-80:~# aws --version
aws-cli/2.13.25 Python/3.11.5 Linux/5.19.0-1025-aws exe/x86_64.ubuntu.22 prompt/off
```

## Configure AWS IAM Credentials on EC2
```bash
root@ip-172-31-3-80:~# aws configure
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: ap-south-1
Default output format [None]: json

root@ip-172-31-3-80:~# aws s3 ls
```