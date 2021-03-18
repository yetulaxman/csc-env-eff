---
title: Docker Terminology and Architecture
---


### Learning Objectives
Understanding docker fundamentals can sometimes be challenging especially in the beginning stage of learning. Part of the challenge comes from the terminology of docker eco-system. It is useful to understand some essential jargon first to get started with course.

After this session, you will get familiar with:
- The basic terminology of docker system
- Core components of docker architecture

### 1. Understand the following basic terminology used in Docker world

Although understanding the extensive list of [Docker-specific jargon](https://docs.docker.com/glossary/) is quite useful, below is the basic terminology we often use in this course:

- **Images**: A Docker image is a read-only file system, comprised of multiple layers, that is used to execute code in a Docker container. An image is created usually from a Dockerfile, which comprises a set of instructions for a complete and executable version of an application. It is a Docker container at rest, i.e., not running.

- **Containers**: Running instances of Docker images. Containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers and runs as an isolated process in user space on the host OS. 

- **Docker daemon**: The background service running on the host that manages building, running and distributing Docker containers.

- **Docker client**: The commandline tool that allows the user to interact with the Docker daemon.

- **Docker Registry**: A remote repository where Docker Images can be stored and made public. Constitutes the point of contact between application developers and their users and allows the distribution of Docker containers.

- **DockerHub**: A [registry](https://hub.docker.com/explore/) of Docker images. You can think of the registry as a directory of all available Docker images. You'll be using this later in this tutorial.


### 2. Name different components of docker architecture

- Docker Daemon
- Docker Clients
- Docker Host 
- Docker Registry 
- Docker Images and containers


