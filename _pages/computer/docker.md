---
layout: page
title: Docker Notes
date: 2021-08-20
tags: computer
---

## Definitions

- **Docker**: An open source platform to develop, ship, and run applications.
- **Docker Daemon (or Engine)**: The process that runs on the host which manages images and containers.
- **Dockerfile**: A text document with instructions, e.g. all the commands you would normally execute manually for a task, to build a docker image.
- **Image**: An ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.
- **Container**: A runtime instance of a docker image which consists of: a docker image, an execution environment, and a standard set of instructions.
- **Registry**: A hosted service containing repositories of images.
- **Repository**: A set of docker images.

## Links

- [Docker](https://www.docker.com/)
- [Docker Docs](https://docs.docker.com/)
- [Docker Glossery](https://docs.docker.com/glossary/)
- [Docker Cheat Sheet Git Repo](https://github.com/wsargent/docker-cheat-sheet)
- [NetworkChuck Tutorial](https://www.youtube.com/watch?v=eGz9DS-aIeY)
- [Intro to Containers Article](https://www.freecodecamp.org/news/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b/)

## Windows - Install Docker

Download and Install Docker Desktop on Windows (use Google since the download link seems to change).  Once installed restart computer.

Right click the Docker icon in the Windows tray (lower right hand side) and click Settings:

If proxy is required then go to "Resources" > "PROXIES" and activate "Manual proxy configuration" and enter:

- HTTP: <proxyaddress>:<port>
- HTTPS: <proxyaddress>:<port>
- Bypass proxy settings...: *.<organization_domain>
- Select Apply & Reset

# Ubuntu - Install Docker

Open a terminal and type:

```
$ sudo apt install docker.io
```

## Testing Docker

Open a terminal and type:

```
$ docker --version
$ docker run hello-world
```

Note: the "docker run" command should automatically pull the "hello-world" image from the Docker Hub registry.

## Using Docker Containers

Pull the latest ubuntu (or any other OS, my favorite is archlinux) from DockerHub, a registry of docker images:

```
$ docker pull ubuntu
```

Run the container, check the running docker containers to see if it is running, and check resource usage:

```
$ docker run -d -t --name ubuntucontainer ubuntu
$ docker ps
$ docker stats
```

Connect to the newly created container, view the files and folders, <do other things>, and finally exit when finished:

```
$ docker exec -it ubuntucontainer bash
$ ls
$ <other commands>
$ exit
```

Finally, you can stop the container, or start it up again if needed:

```
$ docker stop ubuntucontainer
$ docker start ubuntucontainer
```

## Creatng a Dockerfile

Create a directory DockerTest/ (I recommend putting it in your user or home directory) and navigate into it:

```
$ mkdir DockerTest
$ cd DockerTest
```

**Create a Dockerfile**

Create a file named "Dockerfile":

```
(Linux)$ touch Dockerfile
(Windows)$ type nul > Dockerfile
```

Open the Dockerfile in your favorite editor, copy this text, and save:

```
# get base image from registry
FROM ubuntu

# runs get executed when you build the image
RUN apt-get update

# cmds gets executed when you create a container from the image
CMD ["echo", "Hello World!"]
```

**Create an image**

In your terminal build the Dockerfile:

```
$ docker build -t testimage . -f Dockerfile
```

If the image was successfully built then check if exists as an image:

```
$ docker images
```

**Run image to create a container**

If the image exists then run it:

```
$ docker run testimage
```

The terminal should return "Hello World!"

## Useful Notes

Other commands include:

- Type `docker --version` to check if docker is installed
- Type `docker image ls -a` to list images
- Type `docker container ls -a` to list containers
- Type `docker ps` to see running containers
- Type `docker build -t <imagename> <path> -f <filename>` to build a Dockerfile into an image
- Type `docker system prune` to remove all stopped containers, dangling images, and unused networks
- Type `docker image prune -a` to remove all docker images
- Type `docker stop <container>` to stop a particular container
- Type `docker rm <container>` to remove a particular container
