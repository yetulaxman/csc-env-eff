---
title: Port-forwarding in Docker Services
---

## Learning objectives

In this short session you will learn how to:
- Perform port mapping (or port forwarding) using rstudio app
- Get familiar with running rstudio

## Port forwarding using rstudio webserver

Containers are not accessible to outside world by default. if we want to access the application inside a container *via* port number, you need to map the port number of container to the port number of the Docker host.

Start a new container from the ``rstudio`` image that exposes port 8787 from the container to port 8080 on your host. You will need to use the `-p` flag with `docker container run` command.

```bash
 docker run --rm -p 8080:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
 ```
> NB: Mapping ports between your host machine and your containers can get confusing. The trick is to remember that **the host port always goes to the left**,
> and **the container port always goes to the right.**. Remember it as traffic coming _from_ the host, _to_ the container.

Open a web browser and go to port 8080 on your host as below:
* **Native Linux** - [http://localhost:8080]
* **Cloud server** - Make sure firewall rules are configured to allow traffic on port 8080. Open browser and use the hostname (or IP) for your server.

Use username as "rstudio". If you see a webpage saying "Welcome to rstudio!" then you're done!


## Running rstudio container in the background mode

When running a webserver like rstudio, it's pretty useful to run it in the background, freeing up our terminal for other things. Docker enables this with the `-d` flag in `docker run` command

> ```bash
> docker run -d --rm -p 8080:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
> ```

**Exercise questions**:
1. Can you update few R packages of your choice to a running rstudio container?

