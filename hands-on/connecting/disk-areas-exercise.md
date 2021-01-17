---
title: Disk areas in CSC supercomputing environment
---

### Imagine that you have a data file and software binary (e.g., data.txt and softwareA_binary) on your Puhti home directory and  also have a shared project (e.g., project_1234) on Puhti and Mahti. How would you safely share your files to other project members on the same supercomputer (i.e., on Puhti) as well as on Mahti (i.e, another supercomputer at CSC)?

1. First login to Puhti supecomputer.

```bash
ssh <username>@puhti.csc.fi
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

### What would be ideal disk area to perform a task that require high I/O operations (e.g, handling big tar file containing 52000 small files)?

The “normal” Lustre based project specific directories, *scratch* and *projappl*, can store large amounts of data and make it accessible to all the nodes of Puhti. However these directories are not good for managing a large number of files.  If you anyhow need to work with a huge number of files, you should consider using the NVME based local temporary scratch directories, either through normal or interactive batch jobs.

To launch an interactive session in Puhti, execute command:
```text
sinteractive -i
```
To demonstrate the effectivity of local scratch area let’s study a sample directory called *big_data*. The directory contains about 100 GiB of data in 120 000 files. In the beginning the data is packed in one tar-archive file in the scratch directory of project 2001234 (/scratch/project_2001234/big_data.tar)

First we launch an interactive batch job with 2 cores, 4 GB of memory and 250 GB of fast temporary scratch disk.
```text
interactive -c 2 -m 4G -d 250
```
The analysis is then done in three steps:

**Step 1**. Move to the local scratch area using environment variable $LOCAL_SCRATCH and open the tar package to the fast local disk.
```
cd $LOCAL_SCRATCH
tar xvf /scratch/project_2001234/big_data.tar ./
```

**Step 2**. Run the analysis. This time we run a for loop that uses command *transeq* to translate all the fasta files, found in the big_data directory, 
into new protein sequence files:
```text
for ffile in $(find ./ | grep fasta$ )
do
     transeq $ffile ${ffile}.pep
done 
```
In this example about 52000 fasta files were found, so after the processing the big_data directory contains 52000 more small files. 

The actual translation is a simple task so relatively much time is consumed to just open and close files.


**Step 3**. When the processing is finished we store the results back to scratch directory into a new tar file.

```text
tar cvf /scratch/project_2001234/big_data.pep.tar ./
```
Now the results are safe in one file in scratch directory and we can exit from the interactive session.

We could do the same analysis procedure in the scratch directory too.  Below is execution time comparison for running the three steps above in LOCAL_SCRATCH and in normal scratch.  The response times of LOCAL_SCRATCH are rather stable, but in the scratch directory the execution times will vary much, due to changes in the total load of the Lustre file system.

|                               | LOCAL_SCRATCH |         scratch|
|-------------------------------|---------------|----------------|    
|Step 1. Opening tar file       | 2m 8s         |   4m 12s       |
|Step 2. Analysis               | 9m 42s        |   21m 58s      |
|Step 3. Creating new tar file  | 2m 25s        |   42m 21s      | 
|Total                          | 14m 15s       |   1h 8m 31s    |
              
 
### How do you make use of local scratch drive on compute node for faster computational tasks? Convert the following normal batch job into the one that uses local scratch drive?

Below is a normal batch job that pulls docker image from DockerHub and converts into a singularity one  that is compatible for working in HPC environment such as CSC Puhti and Mahti supercomputers. During the conversion process, several layers are retrieved, cached and then converted into singularity file (.sif format)

```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxx

export SINGULARITY_TMPDIR=/scratch/project_xxx/$USER
export SINGULARITY_CACHEDIR=/scratch/project_xxx/$USER
singularity pull --name trinity.simg  docker://trinityrnaseq/trinityrnaseq
```

***Hints***:
- Request local storage using the --gres flag  in sbatch directive as below:

```
#SBATCH --gres=nvme:<local_storage_space_per_node>  # e.g., to claim 200 GB of storage, use option --gres=nvme:200. 

```
- Use the environment variable $LOCAL_SCRATCH to access the local storage on each node.

- Please move any data to shared area once  the job is finished


***Solution:***

```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxx
#SBATCH  --gres=nvme:100

export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR

cd $LOCAL_SCRATCH
#pwd
#df -lh
singularity pull --name trinity.simg docker://trinityrnaseq/trinityrnaseq
mv trinity.simg /scratch/project_xxx/$USER/                                                            
```

