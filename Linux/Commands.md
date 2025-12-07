

## Wget [Link](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/Wget.md)
- wget is a command-line tool used to download files from the internet.
- Below command used to download the Jenkins repository file and save it in the correct location
```
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

## YUM [Link](https://github.com/SeshadriRC/Devops-Learnings/blob/main/Linux/Yum.md)

```
yum upgrade
yum install jenkins
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
