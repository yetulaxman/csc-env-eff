---
title: Disk areas in CSC supercomputing environment
---

### Imagine that you have a data file and software binary (e.g., data.txt and softwareA_binary) on your Puhti home directory and  also have a shared project (e.g., project_1234) on Puhti and Mahti. How would you safely share your files to other project
members on the same supercomputer (i.e., on Puhti) as well as on Mahti (i.e, another supercomputer at CSC)?

1. First login to Puhti supecomputer.

```bash
ssh username@puhti.csc.fi
```
You end up in home directory once you login to Puhti. Let's assume that file data.txt is intended for computational use and softwareA_binary is as software needed for analysis
As you know the project name, you can share file data.txt in scratch folder and softwareA_binary in Projapple directory.

2. Share your softwareA_binary in Projapple directory

```bash
cp softwareA_binary  /projapple/project_1234
````

3. Share data file in scratch directory
```bash
cp data.txt /scratch/project_1234
```
All new files and directories are also fully accessible for other group members (including read, write and execution permissions). If you want to restrict access from your group members, you can reset the permissions with the chmod command.

Setting read-only permissions for your group members for the directory my_directory:

```bash
chmod -R g-w data.txt
```
4. sharing files on Mahti computer
you can copy data.txt file on puhti to scratch drive on Mahti as below:

```bash
rsync -azP data.txt <username>@mahti.csc.fi:/scratch/project_1234
```
you can copy sofwtareA_binary file on puhti to projapple directory on Mahti as below:

```bash
rsync -azP sofwtareA_binary <username>@mahti.csc.fi:/scratch/project_1234
```

Note: you can also you CSC object storage environment (i.e., Allas) to share files between supercomputers.

###
