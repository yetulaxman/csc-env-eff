---
title: Extra excercises
author: CSC Training
---


#### Imagine that you have a data file and software binary (e.g., data.txt and softwareA_binary) on your Puhti home directory and  also have a shared project (e.g., project_1234) on Puhti and Mahti. How would you safely share your files to other project members on the same supercomputer (i.e., on Puhti) as well as on Mahti (i.e, another supercomputer at CSC)?

*Background*: This exercise is aimed at familiarising yourself with main disc areas in Puhti and Mahti supercomputers. Data files needed for computational analysis should be stored and shared in *scratch* directories and any software compilations and binaries should be shared in *proappl* directory. In order to find actual directories use commands such as `csc-workspaces` and `csc-projects`. Data transfer between two supercomputers can be done with many tools including `rsync`. In this example try to avoid using *allas* for data transfer between the supercomupters. 

***Solution:***
