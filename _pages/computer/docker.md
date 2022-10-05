---
layout: page
title: Docker Notes
date: 2021-08-20
tags: computer
---

## Instructions

- show version: `docker --version`
- build image: `docker build -t <imagename> . -f Dockerfile`
- list images: `docker image ls -a`
- run container: `docker run -d --name <containername> <imagename>`
- list containers: `docker container ls -a`
- remove container: `docker container rm <containername>`
- remove image: `docker image rm <iamgename>`

## Links

- [Docker](https://www.docker.com/)
- [Docker Docs](https://docs.docker.com/)
- [Docker Glossery](https://docs.docker.com/glossary/)
- [Docker Cheat Sheet Git Repo](https://github.com/wsargent/docker-cheat-sheet)
- [NetworkChuck Tutorial](https://www.youtube.com/watch?v=eGz9DS-aIeY)
- [Intro to Containers Article](https://www.freecodecamp.org/news/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b/)
