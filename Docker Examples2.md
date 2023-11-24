### Almalinux
```
Image → static single file
Container is running version of image

ex: Ubuntu OS is a image →  It can run into any number of laptops

AWS Linux2 = CentOS = Almalinux = Redhat
Docker Image is a Static file →  It can run into any number of Containers
Image = base OS → bare minimum OS (we need to install packages + Softwares)
```
```
→ Base OS we install packages
ex: ps, clear … we need to install 
install ps → yum install procps -y 
install clear → yum install ncurses -y

→ show information about stopped containers with cutting container id
docker ps -a

→ show information about stopped containers with full container id
docker ps -a --no-trunc

→ run & exited bcs its executing “/bin/bash”
docker run almalinux 

→ sleep for 20 seconds & excited bcs its executing “sleep 20”
docker run almalinux sleep 20 

→ ping 5 packets to google.com CMD "ping -c 5 google.com"
docker run almalinux ping -c 5 google.com

→ show stopped container id only
docker ps -a -q

Remove all Stopped containers
docker rm -f `docker ps -a -q` 
```