## Enable password based authentication for EC2
→ we need to use password based authentication but ec2 instances won't allow it so we need to uncomment this (“PasswordAuthentication yes”)
```bash
vi /etc/ssh/sshd_config

# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes
#PermitEmptyPasswords no
PasswordAuthentication no
```
```bash
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no
```
```bash
[root@ip-172-31-39-179 ~]# service sshd reload
Redirecting to /bin/systemctl reload sshd.service
```