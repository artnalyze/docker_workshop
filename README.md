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
RUN
```