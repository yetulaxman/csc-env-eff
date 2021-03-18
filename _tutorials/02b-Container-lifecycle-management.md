---
title: Management of Docker Container Life-cycle
---
## Learning Objectives
One of the most useful and often used docker commands is `docker run ...` . We have briefly seen it in `hello-world` example. Here, let's go beyond a simple `hello-world` example and explore few more related docker commands.

In this episode, you will learn: 
- Different ways of launching a container
- How to run containers interactively
- How to run docker containers in the background
- How to list images and containers in host system

## Different ways of launching a container

In our previous `hello-world` example, `docker run` command not only implicitly fetched its image from DockerHub and but also launched a container on your host machine. What if you just want to download a docker image but not yet ready for running a container. No worries! we've `docker pull ...` command for the same purpose. This time, we'll however  use a well-known bioinformatics container named, [fastqc](https://hub.docker.com/r/biocontainers/fastqc) which is a  quality control tool for raw sequence data coming from high throughput sequencing pipelines.

To get started, let's pull *fastqc* image in the following way:

```bash
docker pull biocontainers/fastqc:v0.11.9_cv7   
```
*Note*: if you don't provide tag (i.e., v0.11.9_cv6), docker daemon looks for `fastqc` image with tag "latest" which may or may not present in DockerHub. While image is being downloaded, look for all available tags for *fastqc* image of *biocontainers* repository in [DockerHub](https://hub.docker.com/).

Here, the `docker pull ...` command fetches the *fastqc* image from the **Docker registry** and saves it locally in your system. It will not run (i.e., does not create any container from image) image yet. We will use *docker create and docker start* commands instead.

Great! Now let's start a Docker **container** based on fastqc image.

```bash
docker create biocontainers/fastqc:v0.11.9_cv7 sleep 300
docker start 410bcd45613a  # 410bcd45613a is a container ID which can be found by using *docker ps* command 
```

We have now learned a harder way to launch a container and can be much useful if you want to prepare a container before actually running it directly.

Instead of running all the above steps, we can just run  `docker run ...` command 

```bash
docker run biocontainers/fastqc:v0.11.9_cv7 sleep 300
```

When you run `docker container run ...`, with a command (`sleep`), please note that command was actually run inside the container and the container is exited after running `sleep` command.

So please note that **docker create** adds a writeable container on top of your image and sets it up for running whatever command you specified in your CMD. The container ID is reported back but it’s not started. **Start** will start any stopped containers. This includes freshly created containers. **Run** is a combination of create and start i.e., it creates a container and starts it.

## Docker run interactively

In previous examples, `docker run ...` command exited once a user-defined (or default) command is executed inside the container. You're probably thinking if there is a way to run more than just one command in a container. It is possible to get command prompt back by running docker containers interactively. Running images interactively is useful for testing or development. So let's try the following command:

```bash

docker container run -it biocontainers/fastqc:v0.11.9_cv7 /bin/sh

```
*Note*: the flags `-it` are short for `-i -t` which again are the short forms of `--interactive` (Keep STDIN open) and  `--tty` (allocate a terminal).

Here you ran the  `docker run ...` command with special flags `-it` so that it attaches you to an interactive tty in the container. You can run any command inside a container if it is provisioned by author of image! Take some time to run a useful command (e.g., get some documentation help from a software). Remember, you can write `exit` when you want to quit.

Here is an example on how one can get help on fastqc usage:

```bash
docker container run -it biocontainers/fastqc:v0.11.9_cv7 fastqc --help
```

## Run docker containers in the background

The main disadvantage of running a container in the foreground (the default mode for dockers) is that you can not access the command prompt anymore. That means you can't run any other commands while the `fastqc` container is running. In order to run a `fastqc` container in the background mode, you can use flag `-d` (--detach).

Let's launch `fastqc` in the background (=detached) mode as below:

```bash
docker container run -it -d biocontainers/fastqc:v0.11.9_cv7  sleep inf
docker ps -a
````
## Stopping a running container

`Docker stop` can be used to gracefully stop a running container process i.e., it may take a while to shut down the container completely. 

```bash 
docker stop 410bcd45613a  # container id of a running container which you can check by issuing `docker ps` command 
```

In case you want to kill a container without taking some time, you can  use `docker kill` command instead. 

## Listing images and containers

if you start working with multiple containers, it is possible that multiple images and containers are hanging around in the host system. It is time to manage them now.

List running containers using the `ps` command as below:

```bash
docker ps #command shows you all containers that are currently running.
```
All containers have an **ID** and a **name**. Both the ID and name are generated randomly every time a new container is created. If you want to assign a specific name to a container then you can use the `--name` option. That can make it easier for you to reference the container later.

List running all containers using `ps -a` command as below:

```bash

docker ps -a  

```
In the above steps, we saw how to get the shorter container IDs of all our containers using `docker ps -a`

Try adding the `--no-trunc` flag to see the entire container ID:

```bash
 docker container ps -a --no-trunc
```
This long ID is the same as the string that is returned after starting a container with `docker run ...` command.

In order to list only container IDs, we can use `-q` flag as below:

```bash
docker ps -a -q
```
What you see  a list of all containers that you ran. Notice that the `STATUS` column shows that these containers exited some time ago.


You can also check whether the fetched image is available on the host system using the following command:

use the `docker image ls` command to see a list of all images on your system.

```bash
docker image ls

```
How many images are stored locally on your workstation?

##  Summary

We explored part of container lifecycle in this short tour of docker commands. Among all the commands, the `docker  run ...` command is more often used command in our daily life. It makes sense to spend some time getting comfortable with it. To find out more options about `docker run ...`, use `docker container run --help` to see a list of all flags it supports.
