- [copy](#copy)
- [exec](#exec)
- [run](#run)


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
