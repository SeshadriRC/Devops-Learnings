- [get-ent](#get-ent)
- [nslookup](#nslookup)
- [openssl](#openssl)
- [User-management](#user-management)
- [vi-editor](#vi-editor)
- [Wget](#wget)
- [Yum](#yum)


## Get-ent

```
getent group nautilus_developers
```


## nslookup

- [nslookup](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/nslookup.md)

```
[tony@stapp01 ~]$ nslookup stapp03
Server:         127.0.0.11
Address:        127.0.0.11#53

Non-authoritative answer:
Name:   stapp03.stratos.xfusioncorp.com
Address: 172.17.0.6
```

## OpenSSL

[openssl](#https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/openssl.md)

- Generate a Private Key
```
openssl genrsa -out server.key 2048
```

- Create a Certificate Signing Request (CSR)

```
openssl req -new -key server.key -out server.csr
```

- Generate a Self-Signed Certificate

```
openssl req -x509 -new -nodes \
  -key server.key \
  -sha256 \
  -days 365 \
  -out server.crt
```

- Check Certificate Details

```
openssl x509 -in server.crt -text -noout

```

## Wget

[Wget](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/Wget.md)
- wget is a command-line tool used to download files from the internet.
- Below command used to download the Jenkins repository file and save it in the correct location
```
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```
## Yum

[YUM](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/Yum.md)

```
yum upgrade
yum install jenkins
yum provides */nslookup       -> it will check whether nslookup is in which package 
```

## User-management

- User creation

```
useradd sesha
useradd -u 1013 -d /var/www/siva -m siva
id <username>

cat /etc/password -> Check the user information
cat /etc/group -> Check the group information

```

- User creation without home directory

```
useradd -M john
```

- Check the group details

```
cat /etc/group

[banner@stapp03 ~]$ groups jarod
jarod : jarod nautilus_developers
```
- Create a group

```
groupadd developers

```

- Add user to a group

```

Primary group
usermod -g <primary-group> <user-name>

Secondary group
usermod -aG <group-name> <user-name>
```

## vi-editor

esc - Normal mode

i - insert mode

- delete the entire line

```
dd 
```
- copy and paste entire line

```
yy
p
```

- move to beginning of line

```
0 - move to beginnig of line
```

- move to end of line

```
shift+$ - move to end of the line
```

