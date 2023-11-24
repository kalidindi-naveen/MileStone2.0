## Docker Lifecycle
```
→ Docker Image →→ Docker Container
→ Dockerfile →→ Docker Image →→ Docker Container
→ Docker Container →→ commit changes →→ Docker Image →→ Docker Container
  (modify files in container)           (create new Image)     
```
```
→ docker run -itd -p 80:80 --name nginx1 nginx
a93fb7a5a7f218c0f04eb141a788c65c782dd93744d39e5c01168991a91e671d

→docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                NAMES
a93fb7a5a7f2   nginx     "/docker-entrypoint.…"   11 seconds ago   Up 10 seconds   0.0.0.0:80->80/tcp   nginx1

→ docker exec -it nginx1 bash
→ root@a93fb7a5a7f2:/# pwd
/
→ root@a93fb7a5a7f2:/# cd /usr/share/nginx/html
→ root@a93fb7a5a7f2:/usr/share/nginx/html# ls
50x.html  index.html
→ root@a93fb7a5a7f2:/usr/share/nginx/html# echo "Hi From NK" > hi.html
→ root@a93fb7a5a7f2:/usr/share/nginx/html# ls
50x.html  hi.html  index.html
→ root@a93fb7a5a7f2:/usr/share/nginx/html# exit
exit

→ docker commit nginx1 nginx-hi
sha256:bbd19f18a9ab11b7a7692a732491d62219aee43d57e4c8e1f3ca3c498dbcf905

→docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
nginx-hi             latest    bbd19f18a9ab   5 seconds ago   187MB
nginx                latest    a6bd71f48f68   2 days ago      187MB

→ docker run -itd -p 81:80 --name nginx2 nginx-hi
33aa6284474b5d20449ad01f9557e83e7472f82f9d055d6f25180fa2c313e0ae
```

## Picking Docker Image
```
        → Linux →→ Apache →→ Deploy code
WebApp  
        → Linux + Apache →→ Deploy code  (preffered this Image)
```