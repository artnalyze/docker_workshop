# Docker Workshop Ebook

> Source : https://github.com/PacktWorkshops/The-Docker-Workshop

# Workshop

## Set up Environment

```sh
# updating package lists
$ sudo apt update
$ sudo apt upgrade

# install git 
$ sudo apt install git-all

# docker 
$ sudo apt install docker.io -y

$ sudo systemctl start docker 
$ sudo systemctl enable docker

$ docker --version

$ sudo usermod -aG docker ${USER}
$ sudo su ${USER}
$ docker ps
```

## Add user

```sh
$ sudo useradd -d /home/artnalyze -m artnalyze
$ ls -l /home
$ sudo passwd artnalyze

$ cat /etc/passwd
$ sudo cat /etc/shadow
$ sudo cat /etc/shadow | grep root
## show infomation password
$ sudo passwd -S <username>

$ ls -la /etc/skel

# swithcing users
$ sudo passwd
$ sudo su -
$ sudo su - <username>

# Mananging Group
$ cat /etc/group
$ sudo grouppadd admins

# remove group
$ sudo groupdel admins
$ sudo usermod -aG admins myuser
$ sudo usermod -g <group-name> <username>
$ sudo gpasswd -d <username> <grouptoremove>

# managing passwords and password policies
## locking and unlocking user account
$ sudo passwd -l <username>
$ sudo passwd -u <username>

## setting password expiration information
$ sudo chage -l <username>
$ sudo chage -d 0 <username>
```

## Mananging Packages

```sh
$ sudo apt install <package1> <package2> <package3>
$ sudo apt update
$ sudo apt remove <package>
$ sudo apt remove --purge <package>
```

## Command Other

```sh
# search
$ apt search apache php

# check ssh
$ cat /etc/ssh/sshd_config | grep Port

# check log network
$ tail -n 100 /var/log/syslog
$ tail -f /var/log/syslog

# check disk
$ sudo apt install ncdu
```

## Running My First Docker Container

```sh
# Running the hello-world container
$ docker run hello-world

# check process
$ docker ps

# check all process
$ docker ps -a

# check images local
$ docker images

# run hell-world again
$ docker run hello-world
```

### Manage Docker Container

```sh
$ docker pull
$ docker stop
$ docker start
$ docker restart
$ docker attach
$ docker exec
$ docker rm 
$ docker rmi
$ docker inspect
```

### Managing Container Life Cycles

```sh
$ docker pull ubuntu:18.04
$ docker pull ubuntu:19.04
$ docker images
$ docker inspect [18.04_id]
$ docker inspect [19.04_id]

# run
$ docker run -d ubuntu:18.04
$ docker ps -a
$ docker run -i -t -d --name ubuntu1 ubuntu:18.04
$ docker ps -a
$ docker exec -it ubuntu1 /bin/bash
# echo "Hello world from ubuntu" > hello-world.txt
# exit
$ docker run -it -d --name ubuntu2 ubuntu:19.04
$ docker exec -it ubuntu2 /bin/bash
# echo "Hello World from ubuntu2!" > hello-world.txt

$ docker exec -it ubuntu1 cat hello-world.txt
# Hello world from ubuntu

$ docker exec -it ubuntu2 cat hello-world.txt
# Hello World from ubuntu2!

$ docker stop ubuntu2
$ docker ps
$ docker ps -a
$ docker start ubuntu2
$ docker ps

# stop container
$ docker stop ubuntu1
$ docker stop ubuntu2

# remove container
$ docker rm ubuntu1
$ docker rm ubuntu2
$ docker rm [container_id]

# Docker Images list
$ docker images

# remove old container 
$ docker system prune -fa

# attaching to an ubuntu container
$ docker run -itd --name attach-example1 ubuntu:latest
$ docker ps
$ docker attach attach-example1
#/# ctrl+p ctrl+q
#/# read escape sequence

# connect again
$ docker attach attach-example1
#/# exit

$ docker ps
# ........................
```

### Activity 1.01

pulling and running the PostgreSQL container image from Docker

```sh
$ docker pull postgres:12
$ docker run -itd -e "POSTGRES_USER=panoramic" -e "POSTGRES_PASSWORD=trekking" postgres:12

$ docker exec -it <container_id> psql --username panoramic --password
# Password:
#/# \l  ## list database
#/# \q  ## quite
$ docker stop <container_id>
$ docker rm <container_id>
$ docker rmi <container_id>
```

## Getting Started with Dockerfiles

dockerfile

```dockerfile
# This is a comment
DIRECTIVE argument
```

Common Directive in Dockerfiles

1. FROM
2. LABEL
3. RUN
4. CMD
5. ENTRYPOINT

### The FROM Directive

> specify the parent image

```sh
FROM <image>:<tag>
```

```sh
FROM ubuntu:20.04

# use custom
FROM scratch
```

### The LABEL Directive

> key-value pair metadata

```sh
LABEL <key>=<value>
```

```sh
# miltiple line
LABEL maintainer=sss@mydomain.com
LABEL version=1.0
LABEL environment=dev

# single line
LABEL maintainer=sss@mydomain.com version=1.0 environment=dev
```

### The RUN Directive

> used to execute commands during the image build time

```sh
RUN <command>
```

```sh
# multiple line
RUN apt-get update
RUN apt-get install nginx -y

# single line
RUN apt-get update && apt-get install nginx -y
```

### The CMD Directive

> executed when a container is created
> command parameter

```sh
CMD ["executable","param1","param2","param3",...]

CMD ["echo","Hello World"]
# docker container run <image>
# Hello World

# docker container run <image> echo "Hello Docker !!!"
```

### The ENTRYPOINT Directive

> used to provide this default initialization command

```sh
ENTRYPOINT ["executable","param1","param2","param3",...]
```

```sh
ENTRYPOINT ["echo", "Hello"]
CMD ["World"]

# docker container run <image>
# Hello World

# docker container run <image> "Docker"
# Hello Docker
```

```sh
# create dockerfile
$ mkdir custom-docker-image
$ cd custom-docker-image
$ touch Dockerfile
$ vim Dockerfile

# -------------------------------------
# This is my first Docker image
FROM ubuntu
LABEL maintainer=niidvp@gmail.com
RUN apt-get update
CMD ["The Docker Workshop"]
ENTRYPOINT ["echo", "You are reading"]
# -------------------------------------
```

### Building Docker Images

![](https://imgur.com/OQhXmAV)

```sh
$ docker image build <context>
$ docker image build .

$ docker image list

# add tag
$ docker image tag 80de46e65f34 my-tagged-image:v1.0
$ docker image build -t my-tagged-image:v2.0 .
```

> exercise : creating our first docker image

```sh
# Dockerfile
# -------------------------------------
# This is my first Docker image
FROM ubuntu
LABEL maintainer=niidvp@gmail.com
RUN apt-get update
CMD ["The Docker Workshop"]
ENTRYPOINT ["echo", "You are reading"]
# -------------------------------------
$ docker image build -t welcome:1.0 .
$ docker image build -t welcome:2.0 .
$ docker image list
$ docker container run welcome:1.0
# You are reading The Docker Workshop
$ docker container run welcome:1.0 "Docker Beginner's Guide"
```

### Other Dockerfile Directives

1. ENV
2. ARG
3. WORKDIR
4. COPY
5. ADD
6. USER
7. VOLUME
8. EXPOSE
9. HEALTHCHECK
10. ONBUILD

#### The ENV Directive

> used to set environment variables 

```sh
ENV <key> <value>
```

```sh
$PATH:/usr/local/myapp/bin/
```

```sh
# set one line
ENV PATH $PATH:/usr/local/myapp/bin/
```

```sh
# set multiple environment
ENV <key>=<value> <key>=<value> ...

ENV PATH=$PATH:/usr/local/myapp/bin/ VERSION=1.0.0
```

#### The ARG Directive

> used to define variables that the user can pass at build time.

```sh
$ docker image build -t <imag>:<tag> --build-arg <variable>=<value> .
```

```sh
ARG <varname>

ARG USER
ARG VERSION

ARG USER=TestUser
ARG VERSION=1.0.0
```

### Using ENV and ARG Directive in a Dockerfile

```sh
$ mkdir env-arg-exercise
$ touch Dockerfile
$ vim Dockerfile

#-------------------------------------
# ENV and ARG example
ARG TAG=latest
FROM ubuntu:${TAG}
LABEL maintainer=artnalyze@gmail.com
ENV PUBLISH=packt APP_DIR=/usr/local/app/bin
CMD ["env"]
#-------------------------------------

$ docker image build -t env-arg --build-arg TAG=19.04 .
$ docker container run env-arg
```

## The WORKDIR Directive

> used to specify the current working directory of the Docker container

```sh
WORKDIR /path/to/workdir
```

```sh
WORKDIR /one
WORKDIR two
WORKDIR three
RUN pwd
```

## The COPY Directive

> used to copy files from our local filesystem to the Docker

```sh
COPY <source> <destination>
COPY index.html /var/www/html/index.html
COPY *.html /var/www/html
COPY --chown=myuser:mygroup *.html /var/www/html
```

## The ADD Directive

> used similar to the COPY

```
ADD <source> <destination>
ADD http://sample.com/test.txt /tmp/test.txt

ADD html.tar.gz /var/www/html
```

## Exercise : using the workdir, copy, and add directive in the Dockerfile

```sh
$ mkdir workdir-copy-add-exercise
$ cd workdir-copy-add-exercise
$ touch index.html
$ vim index.html
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker</title>
</head>
<body>
    <h1>Welcome to The Docker Workshop</h1>
    <img src="logo.png" alt="" height="350">
</body>
</html>
```

```
$ touch Dockerfile
$ vim Dockerfile
```

```dockerfile
# WORKDIR, COPY and ADD example
FROM ubuntu:latest
RUN apt-get update && apt-get install apache2 -y
WORKDIR /var/www/html/
COPY index.html .
ADD https://www.docker.com/site/default/files/d8/2019-07/Moby-log.png ./logo.png
CMD ["ls"]
```

```
$ docker container run workdir-copy-add
```

```sh
$ docker image build -t workdir-copy-add .
```

## The USER Directive

> USER to change this default behavior

```
USER <user><group>
```

Exercise 2.05: Using USER Directive in the Dockerfile

```sh
$ mkdir user-exercise
$ cd user-exercise
$ touch Dockerfile
$ vim Dockerfile
```

```dockerfile
# USER example
FROM ubuntu
RUN apt-get update && apt-get install apache2 -y 
USER www-data
CMD ["whoami"]
```

```sh
$ docker image build -t user .
$ docker container run user
```

## The VOLUME Directive

> the VOLUME directive within the Dockerfile to create Docker volumes.

```
VOLUME ["/path/to/volume"]
VOLUME /path/to/volume1 /path/to/volume2
```

inspect container

```sh
$  docker container inspect <container>
```

Exercise 2.06: Using VOLUME Directive in the Dockerfile

```sh
$ mkdir volume-exercise
$ cd volume-exercise
$ touch Dockerfile
$ vim Dockerfile
```

Dockerfile

```dockerfile
# VOLUME example
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install apache2 -y
VOLUME ["/var/log/apache2"]
```

```sh
$ docker image build -t volume .
$ docker container run --interactive --tty --name volume-container volume /bin/bash

root@bc61d46de960: /#
# cd /var/log/apache2/
# ls -l
exit

$ docker container inspect volume-container
```

>  docker volume inspect <volume_name>

```sh
$  sudo ls -l /var/lib/docker/volumes/354d188e0761d82e1e7d9f3d5c6ee644782b7150f51cead8f140556e5d334bd5/_data
```

## The EXPOSE Directive