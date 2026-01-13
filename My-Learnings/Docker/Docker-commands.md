- [copy](#copy)
- [exec](#exec)
- [run](#run)
- [pull](#pull)
- [tag](#tag)


# Copy

```
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/
```


# Exec

```
docker exec -it <container-name> /bin/bash
-----------
docker exec -it ubuntu_latest ls -l /usr/src/
```

# Run

```
docker run -d --name <container-name> -p <hostport>:<container-port> -v <hostpath>:<docker-mountpath> <image-name>

docker run -d --name nautilus -p 8080:80 -v /var/www/html:/usr/local/apache2/htdocs httpd
Eg: curl http://localhost:8080/

-----------

```

# Pull

```
docker pull busybox:musl

[tony@stapp01 ~]$ docker pull busybox:musl
musl: Pulling from library/busybox
5bfa213ad291: Pull complete 
Digest: sha256:03db190ed4c1ceb1c55d179a0940e2d71d42130636a780272629735893292223
Status: Downloaded newer image for busybox:musl
docker.io/library/busybox:musl
```

# tag

```
docker tag <oldimage-name:tag>  <image-name:tag>
[tony@stapp01 ~]$ docker tag busybox:musl busybox:local

- below we can notice that imageid will be same

[tony@stapp01 ~]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      local     ff7d91a4de4f   15 months ago   1.51MB
busybox      musl      ff7d91a4de4f   15 months ago   1.51MB
```
