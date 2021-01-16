---
title: Disk areas in CSC supercomputing environment
---

## Learning Objectives
Users at CSC supercomputers have been granted with personal and project-specific disk areas. It is important to understand different disk areas that belong to you in order to manage your and other project memebrs data.

Upon completion of this tutorial, you will get familiar with:
- Personal and project-specific disk areas and their quotas in CSC supercomputing environment
- Ideal disk areas for large IO operations
- Navigating between different project-specific disk areas

### How do you identify your personal and project-specific directories in Puhti and Mahti supercomputers?

CSC has main different disk areas (or directories), each one with specific purpose. Let's get familiar with them.

```bash
csc-workspaces 
```
Resulting output from the above command shows lot of information about different directories and their current quota. Briefly, these directories are as below:

- user-specific directory which is your home directory ($HOME) which can contain data up to 10 GB. It is default directory when you login to Puhti/Mahti. Its purpose is to store configuration files and other minor personal data. 

- Project-specific directories which are scratch  and projappl directories. Each project has by default 1 TB of scratch disk space. It is a temporary storage space in supercomputers and the files that have not been used for 90 days will be automatically removed. ProjAppl directory on the other hand can contain up to 50 GB of data and is mainly for storing and sharing compiled applications and libraries etc. with other members of the project. 


### What would be the ideal disk space for large scale IO operations in CSC computing environment?

Once in a while we come across the cases where we have to handle uncommonly large number of smaller files which cause heavy IO load on supercomputing environment. In order to facilitate such operations, CSC has provided fast local disk arease in login and compute nodes.

In order to identify such directories in login nodes in Puhti/Maht, use the following command:

```bash
echo $TMPDIR
```
This space for example be useful for pre- and post-processing of large files. 

However, if you are going to do some computing tasks on those larger number of smaller files, we will use local storage areas in compute nodes which are accessed either interactively or using batch jobs.

In the [interactive jobs](https://docs.csc.fi/computing/running/interactive-usage/), isssue the following command to find out local storage area in that compute node:

```bash
echo $TMPDIR
```
When using batch job, use the environment variable $LOCAL_SCRATCH in your [batch job scripts](https://docs.csc.fi/computing/running/creating-job-scripts-puhti/#local-storage) to access the local storage on each node.

### How do you navigate between different project-specific directories?

A CSC user can have multiple projects and often needs to switch between different projects. Currently, all directories on scratch drive are project-based and one should be aware of a project number to find out actual path of a given project. While we can actually find scratch directories corresponding to all your projects numbers using `csc-workspace`, it may not be immediately obvious to map those project numbers to meta data of projects. You can instead use the following command
to find more descriptions related to your project.

```bash
csc-projects
```
Good thing about this command is that you will also get billing units information associated with each of your project.

