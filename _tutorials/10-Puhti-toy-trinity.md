---
title: Running Singularity Container using Trinity Example 
---

## Accessing and running singularity containers ##

Log into Puhti using your CSC credentials.

Move to the course project scatch directory and make a subdirectory for yourself
```
cd /scratch/project_xxxx
mkdir $USER
cd $USER
mkdir trinity
cd  trinity
```
In this exercise we use ready provided Singularity image and data to keep things simple.

Make and save the following batch job script as e.g. trinity.sh
```
#!/bin/bash
#SBATCH --time=00:30:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=5
#SBATCH --mem=4000

export TMPDIR=$PWD
singularity exec --bind $PWD:$PWD --bind /scratch/project_xxx/Trinity:/data /scratch/project_xxx/Trinity/trinityrnaseq_latest.sif \
 Trinity --seqType fq \
 --max_memory 1G \
 --CPU 5 \
 --output $PWD/TrinityOut \
 --left /data/reads.left.fq.gz \
 --right /data/reads.right.fq.gz \
                                   
```
Submit the job:
```
sbatch trinity.sh
```



