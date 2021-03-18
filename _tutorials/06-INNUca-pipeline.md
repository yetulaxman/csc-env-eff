---
title: Running a Containerised Pipeline - INNUca Pipeline
---
# Running a Containerised Pipeline

We have gained some skills so far in handling docker containers.  Let's run a bioinformatics pipeline which has some real-world application.

INNUca is a standardized, fully automated, flexible, portable and pathogen-independent pipeline for bacterial genome assembly and quality control starting from short reads. The new version of INNUca is available [here](https://github.com/INNUENDOCON/INNUca)

## Learning Objectives

You will learn how to launch a real-world container and explore several subtasks that are involved as part of INNuca pipeline. This is also a good place to use most of the skills you have used in this course.

## Assembly of high throughput sequencing data using INNUca pipeline

1. It is a good idea to clean all of your workspace before start launching INNUca pipeline.

   Docker image size of INNUca is rather large (~ 4GB) and may take up some considerable disk space on your VirtualBox. So clean up unwanted/unused docker containers and images.

2. Start pulling INNUca docker image from DockerHub

   Navigate to INNUca repository on [Dockerhub](https://hub.docker.com/r/ummidock/innuca). Explore some information such as author information, docker image and its tags, among others.

   start pulling the image as below:
   ```bash
   docker pull ummidock/innuca:4.2.2-02

   ```
3. Prepare data on your local machine

   For the convenience of this tutorial, we have provided some small datasets on CSC'S Allas object storage. You
   can download as below:

   ```bash
   wget https://a3s.fi/Biocontainer/INNUca_data.tar.gz

   ```
4. Run INNUca pipeline

   ```bash
   # INNUca basic command
   # You should specify where the output goes whenever there is an option to do that
   # Whenever possible use the option to specify the number of CPUs/threads to be used
   # make sure you have downloaded read data inside `/home/biouser/innuca/read` folder and then mount `innuca` folder(e.g,/home/biouser/innuca) inside container

   docker run --rm -u $(id -u):$(id -g) -it -v /home/biouser/innuca:/data ummidock/innuca:4.2.2-02 \
    INNUca.py --inputDirectory /data/reads/ \
              --speciesExpected "Streptococcus agalactiae" \
              --genomeSizeExpectedMb 2.1 \
              --outdir /data/results/ \
              --threads 2
              --fastQCproceed \
              --fastQCkeepFiles \
              --trimKeepFiles \
              --saveExcludedContigs
    ```

5. Explore the different modules present in INNUca pipeline.

As it takes some time to run the pipeline, explore different modules available as part of this pipeline by visiting the latest version of INNUca as available [here](https://github.com/INNUENDOCON/INNUca)


6. Remove docker image from you workspace once you are done with your pipeline

   ```bash
    # List Docker images
    docker images
    # Remove INNuca image
    # Find the INNUca image line starting with ummidock/innuca
    # Get the Image ID, something like 1f467865b7f3
    docker rmi <INNUca_Image_ID>
   ```
## Conclusion
In this session, you have learned how to use a real-world example of a bioinformatics container. This tutorial required using the basic skills you have learned so far. In reality, running actual pipelines may be difficult as pipelines usually require many more subtasks.
