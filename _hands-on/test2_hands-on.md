---
title: Training Environment for Course
---


This course provides a VirtualBox image with pre-installed docker and singularity clients to facilitate your hands-on tutorials. You may use [Play-with-Docker](https://labs.play-with-docker.com/) (PWD) which is an in-browser Linux terminal for docker environment if you have problems in setting-up VirtualBox on your computer. In the later case, you need to sign in PWD with your docker ID.

## Getting Started with Course Environment

1. Start your VirtualBox set-up for course if you have not already done so.

   Download VirtualBox image for course from CSC's [Allas objects storage](https://a3s.fi/Biocontainer/BioContainer.ova) and [set-up VirtualBox](https://raw.githubusercontent.com/amsaren/course_materials/main/Biocontainers_2020/BC2020_Working_with_VirtualBox.pdf)

2. What is the version of Docker installed in VirtualBox environment(=host machine for docker) for course?
```
docker --version
```
3. What is the version of Singularity installed in this VirtualBox environment?
```
singularity --version
````

> **_NOTE:_** 
> Some useful commands inside VirtualBox:
> ```bash
> Copying text inside linux terminal: shift+control+c
> Pasting text inside linux terminal: shift+control+v
> Copying/pasting text inside the browser should work normally (i.e., control +c and control +v)
> ```

