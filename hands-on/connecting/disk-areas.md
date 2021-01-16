---
title: Disk areas in CSC supercomputing environment
---

### Learning Objectives
Users at CSC supercomputers have been granted with personal and project-specific disk areas. It is important to understand different disk areas that belong to you in order to manage your and other project memebrs data.

Upon completion of this tutorial, you will get familiar with:
- Main disk areas and their quotas in CSC supercomputing environment
- Navigating between different project-specific disk areas

## How do you identify your disk areas in Puhti and Mahti supercomputers?

CSC has main different disk areas (or directories), each one with specific purpose. Let's get familiar with them.

```bash
csc-workspaces 
```
Resulting output from the above command shows lot of information about different directories and their current quota. Briefly, these directories are as below:

- user-specific directory which is your home directory ($HOME) which can contain data up to 10 GB. It is default directory when you login to Puhti/Mahti. Its purpose is to store configuration files and other minor personal data. 

- Project-specific directories which are scratch  and projappl directories. Each project has by default 1 TB of scratch disk space. It is a temporary storage space in supercomputers and the files that have not been used for 90 days will be automatically removed. ProjAppl directory on the other hand can contain up to 50 GB of data and is mainly for storing and sharing compiled applications and libraries etc. with other members of the project. 


## Mac
