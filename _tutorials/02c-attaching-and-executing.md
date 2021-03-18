---
title: Attaching and updating a running containers
---
## Learning Objectives
In this session you will be able to learn:
- How to connect to a running container
- How to modify content inside a container

## Executing commands on running containers

This is primarily used for debugging purposes and perhaps updating some programs inside a running container. If you want to go into a container to execute some ad-hoc commands inside a running container you can use the following docker command:

```bash 
docker exec <options> <container> <command>
```
`exec`  command starts another process inside the container. This can for example be a shell or a script of some sort.

> NOTE:
> When you attach to an already started container, you can stop it by `Ctrl+d` or detach yourself by `Ctrl+p Ctrl+q`

First, start up our fastqc container and let it be running in the background:

```bash
docker run -d  biocontainers/fastqc:v0.11.9_cv7 sleep inf
661c6dd59d78b97f8142d67eff6b1d58fbbd42247900241e08f46abdbad19f06
```

You can check the list of containers running in your host machine and identify `container ID` corresonding to `fastqc` container

Step into `fastqc` container by executing a bash inside the container:

```bash
docker exec -it <container id> bash 

# In this case container id is starting with "661c6dd59...." 
docker exec -it <661c6dd59> bash
```
You are now inside of a running container and can run any available commands there.

Another way to grab a running container is to use `docker attach` command as below:

```bash
docker attach 661c6dd59
```
Think about the difference bewteen docker exec  and  attach command.

## Installing content inside the container.

As a good design principle, containers only have the bare minimum installations just to perform desired tasks. so if you want to explore the content inside the container, some tools may be missing. For example, *fastqc* container does not have vim edior, let's install vim editor here by going inside the container via. *docker exec* command:

```bash
docker exec -it <661c6dd59> bash # this will let you go insisde fastqc container as mentioned above
vi  # you will realise that vi editor is not installed in fastqc container
apt-get update && apt-get install vim # this installation now happens inside the container
```
Then detach from the container with `Ctrl+p and Ctrl+q`.  

You can go inside the container again to check whether *vi* editor is installed properly

```bash
docker container exec -it <container_id> bash
vi
```
This way you can add some modifications to an existing container. Please note that once containers is removed all modifications will disappear. 

## Conclusion
Understand Docker Run vs Docker Exec commands. `Docker run ...` is the command you use in creating a brand new container from an image, whereas docker exec lets you run commands on an already running container. You have also seen how to break out of a container either by terminating the process by `Ctrl+d`, or by detaching from the process by `Ctrl+p Ctrl+q`.
