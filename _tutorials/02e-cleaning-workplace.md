---
title: Cleaning Docker images and containers
---

# Cleaning your workspace
 As you work with containers, many containers are created, tested, and abandoned during their lifecycle. In due course of time, you may be piling up containers and images in your workspace in host system. Therefore it is important to identify unnecessary containers and images to clean them up every now and then from your work environment. Otherwise, docker can gradually eat up the disk space of host machine.

## Learning objectives
 You will learn how to clean up your unwanted containers and images that you no longer need them.

## Cleaning up containers you do not use anymore

Containers are still persisted, even though they are stopped.
If you want to delete them from your server you need to use the `docker container rm` command.
`docker container rm` can take either the `CONTAINER ID` or `NAME` as seen above. Try to remove the `hello-world` container:

```bash
$ docker container ls -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS                      PORTS                                                          NAMES
6a9246ff53cb        hello-world               "/hello"                 18 seconds ago      Exited (0) 16 seconds ago                                                                  ecstatic_cray

$ docker container rm 6a9246ff53cb    # container with STATUS marked as exited

```

The container is now gone when you execute  `ls -a` command to check the current situation.

what if there are several such containers to be deleted? Well, you can list those container IDs with the following command:

```bash
docker rm <container-id1> <container-id2> ...
```
or you can use `filter` flag to remove exited containers:

docker rm $(docker ps -qa --filter status=exited)

more useful commands:
`docker stop $(docker ps -a -q)`  # stops all running containers
`docker rm $(docker ps -a -q)`.  #  removes all running containers

### Deleting docker images

You deleted the container instance above, but not the image of hello-world itself. Real-world docker images comprise multiple layers to run code within a container. Old and outdated images can clutter your system, taking up storage space and making searches more cumbersome. In our case, you do not need e.g., hello-world image anymore so let us delete it.

First of all, list all the images you have downloaded to your computer:

```bash
$ docker image ls
REPOSITORY                              TAG                   IMAGE ID            CREATED             SIZE
alpine                                  latest                053cde6e8953        9 days ago          3.97MB
hello-world                             latest                48b5124b2768        10 months ago       1.84kB
```

Here you can see the images downloaded as well as their sizes.
To remove the hello-world image use the `docker image rm` command together with the id of the docker image.

```bash
sofus@praq-sal:~$ docker image rm 48b5124b2768
Untagged: hello-world:latest
Untagged: hello-world@sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Deleted: sha256:48b5124b2768d2b917edcb640435044a97967015485e812545546cbed5cf0233
Deleted: sha256:98c944e98de8d35097100ff70a31083ec57704be0991a92c51700465e4544d08
```
and obviously, you can remove multiple images as below:

docker rmi <your-image-id> <your-image-id> ...

## Remove Dangling Images

Normally these images are unused and only use up disk space. We can find them by using the -f filter option with the string dangling=true. They can be purged afterwards.

To view dangling images use the below command:

```bash
$ docker images –f dangling=true  

```

to delete or remove dangling images use
```
$ docker images purge

```

### In general

In the process of pulling, building and running images for our daily needs, we often end-up downloading several of images. These images will not be deleted as docker takes a very conservative approach to clean up. Docker provides a `prune` command, taking all dangling containers/images/networks/volumes.

* `docker container prune`
* `docker image prune`
* `docker network prune`
* `docker volume prune`

The docker image prune command allows you to clean up unused images. By default, docker image prune only cleans up dangling images. A dangling image is one that is not tagged and is not referenced by any container. To remove all _unused_ resources, resources that are not directly used by any existing containers, use the `-a` switch as well.

If you want a general cleanup, then `docker system prune` is your friend.

## Summary

We have seen how to identify the unused docker containers/images and clean them up from working space.
