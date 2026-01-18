- [cd](#cd)
- [chmod](#chmod)
- [ls](#ls)
- [vi-editor](#vi-editor)
- [touch](#touch)
- [mkdir](#mkdir)
- [cat](#cat)
- [free](#free)
- [nproc](#nproc)
- [top](#top)
- [set](#set)
- [ps](#ps)
- [awk](#awk)
- [curl](#curl)
- [wget](#wget)
****
- [get-ent](#get-ent)
- [nslookup](#nslookup)
- [openssl](#openssl)
- [SSH](#ssh)
- [User-management](#user-management)
- [Wget](#wget)
- [Yum](#yum)


## wget

[wget](https://github.com/SeshadriRC/Devops-Concepts/blob/main/DevOps-Fundamentals-Core-Tooling/Shell-scripting-part2.md#wget)

```
wget https://google.com
```

## curl

[curl](https://github.com/SeshadriRC/Devops-Concepts/blob/main/DevOps-Fundamentals-Core-Tooling/Shell-scripting-part2.md#curl)

```
curl -X GET googl.com
```

## awk

[awk](https://github.com/SeshadriRC/Devops-Concepts/blob/main/DevOps-Fundamentals-Core-Tooling/Shell-scripting-part2.md#awk)

## ps

```
ps -ef
ps -ef | grep "amazon"
```

## set

```
set -x     -> debug mode
```

[link](https://github.com/SeshadriRC/Devops-Concepts/blob/main/DevOps-Fundamentals-Core-Tooling/Shell-scripting-part2.md#set--e)
```
set -e
```

[link](https://github.com/SeshadriRC/Devops-Concepts/blob/main/DevOps-Fundamentals-Core-Tooling/Shell-scripting-part2.md#set--o-pipefail)

```
set -o pipefail
```


## chmod

[chmod](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/Concepts/chmod.md)

```
chmod 755 <file.txt>
```

## top

```
top                    --> it will show all memory, cpu and disk
```

## nproc

```
nproc
```

## free

```
free -h
free -m
```

## cat

```
cat <file-name>
```

## mkdir

```
mkdir <dir-name>
```

## touch

```
touch <file-name>
touch <file-name1> <file-name2>
```

## cd

```
cd <dir-name>     
cd ..           -> one step back
cd ../../       -> two step back
```

## ls

```
ls
ls -lrth
```

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

## SSH

[SSH](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Linux/Concepts/ssh-key-gen.md)
```
ssh-keygen
ssh-copy-id natasha@ststor01
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

- To check the installed packages

```
rpm -qa | grep git
yum list --installed | grep git
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

- undo the changes

```
in escape mode , type u

u
```

- redo

```
ctrl+r
```
