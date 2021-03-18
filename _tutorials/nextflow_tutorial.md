---
title: Nextflow(101) for Puhti users
author: CSC Training
---
# Nextflow (101) tutorial for  Puhti users 
Running and managing workflows for bioinformatics applications can be challenging as the workflows usually are fragile eco-systems of several software tools and their dependencies. We therefore need a workflow manager like Nextflow to manage our scientific pipelines. [Nextflow](https://www.nextflow.io/docs/latest/index.html) is a [groovy-based](https://en.wikipedia.org/wiki/Apache_Groovy) language for expressing the entire workflow in a single script and also facilitates the ease of working with workflows by rendering several useful features as mentioned below: 
 - Workflow management
 - Reproducibility
 - Portability
 - Scalability
 - Parallelization
 - Easy prototyping 
 - Partial resumption

## Learning Objectives
Upon completion of this tutorial, you will be able to learn: 
- The basic understanding of how Nextflow works
- The deployment of Nextflow scripts on Puhti supercomputer
- How to use Singularity containers with Nextflow for your analysis

## Content Summary
- [Set up your work environment for tutorials on Puhti](#set-up-your-work-environment-for-tutorials-on-puhti)
- [Tutorial 1: Hello-world example](#tutorial-1-hello-world-example)
- [Tutorial 2: A real-world example with `fastqc` software](#tutorial-2-a-real-world-example-with-fastqc-software)
- [Tutorial 3: Tutorial 3: Nextflow pipeline with containers and other useful features](#tutorial-3-nextflow-pipeline-with-containers-and-other-useful-features)
- [(Bonus) Tutorial 4: Running a nf-core pipeline](#bonus-tutorial-4-running-a-nf-core-pipeline)
- [(Bonus) Converting a bioconda package into singularity image](#bonus-converting-a-bioconda-package-into-singularity-image)

## Set up your work environment for tutorials on Puhti
This hands-on tutorial uses Puhti supercomputer for executing Nextflow scripts for interactive and batch jobs. One therefore needs to have either a training or user account at CSC to access Puhti.

Login to Puhti using `ssh` command followed by making a work directory named `nextflow_tutorial` on *scratch* drive as shown below: 

```nextflow
ssh <your_csc_username>@puhti.csc.fi
mkdir -p  /scratch/project_xxxx/$USER/nextflow_tutorial && cd /scratch/project_xxxx/$USER/nextflow_tutorial 
```

### Start an interactive session on Puhti

Lanuch an [interactive session](https://docs.csc.fi/computing/running/interactive-usage/) on Puhti as below:
```
sinteractive -c 2 -m 4G -d 250 -A project_xxxx  # replace actual project number here
module load bioconda
source activate nextflow
```

## Tutorial 1: Hello-world example 
In this Hello-world tutorial, you will learn how to run a Nextflow script as well as understand the default location of resulting output files.

Download course material from CSC's [`allas`](https://docs.csc.fi/data/Allas/) object storage as shown below:

```bash
wget https://a3s.fi/nextflow/tutorial_demo.tar.gz
tar -xavf tutorial_demo.tar.gz && rm tutorial_demo.tar.gz
```

After unpacking the `tutorial_demo.tar.gz` file, you can see `hello_demo` folder which has hello-world script (ending with `.nf`) for running this demo. Execute the script by entering the following command on your interactive Puhti terminal: 

```nextflow
cd hello_demo
nextflow run hello-world.nf
```
This script defines one process named `sayHello`. This process takes a set of greetings from different languages and then writes each one to a separate file.

The resulting terminal output would look similar to the text shown below:

```nextflow
N E X T F L O W  ~  version 20.07.1
Launching `hello-world.nf` [cheeky_shaw] - revision: 3ffdbdd5c7
executor >  local (5)
[e2/9aa8c8] process > sayHello (5) [100%] 5 of 5 ✔
```
#### Where are the output files created from the above hello-world example?

The hexadecimal number, like e2/9aa8c8, identifies the unique process execution and the number is also the prefix of the directory where `sayHello` process is executed. You can inspect the files produced by above script by changing to directory $PWD/work.

Execute the following command on your terminal:
```
ls -l work/**/*
```
You can see that there is a separate output file created under each directory. 

#### What kind of hidden files exist inside $PWD/work directory?

Hidden files are present in each process directory, and the files are very handy when you want to debug a failed process.

you can find the hidden files as shown below:

```
ls -l work/**/*/.command*
```

## Tutorial 2: A real-world example with `fastqc` software
In this example,  let's use some real-world example that involves working with samples from sequencing experiments. We specifically learn:
- Declaring (and overriding default) pipeline parameters
- Moving results from analysis to a convenient folder
- Basic Nextflow channels and operations (at a theoretical level)

`fastqc_demo` folder has the necessary files for running this tutorial. You can run Nextflow script for *fastqc* analysis on your interactive terminal by issuing the following command:

```nextflow
cd fastqc_demo
nextflow run fastqc.nf 
```
### Identify resulting files from *fastqc* analysis ?

*tip*: check $PWD/work directory as shown in previous example

### Passing parameters to nextflow pipeline
Nextflow parameters inside a script are declared by prepending to a variable name with the prefix *params*, separated by dot character (e.g., params.reads). Parameters thus specified in script are used by default. The parameter can also be specified on the commandline by prefixing the parameter name with a double dash character (e.g., --reads). 
 
Here is an example to declare parameters (here, input files) to *fastqc* software inside Nextflow script (NOT for running the command on terminal):

```nextflow
params.reads = "$baseDir/data/*_{1,2}.fq.gz"
input_ch = Channel.fromPath(params.reads)
```
One can also override parameter values (here files inside `$baseDir/data/` directory) in nextflow script by passing the parameters in commandline when executing script as shown below:

```bash
nextflow run fastqc.nf --reads data2/*_{1,2}_subset.fq.gz
```
Please note that *data2* folder has different samples (i.e., lymphnode4a samples) than the ones (i.e.,lung3e samples) in *data* folder which was used by default. You can see that *fastqc* analysis was performed on a new set of samples now as shown below:  

```
ls -l $PWD/work/**/*
```
> **_NB_**: single dash (`-`): The single dash parameters are from a small, nextflow-defined subset. double dash (`--`): The double-dash parameters are user-defined and completely extensible -- they are used to populate `params`.

### Moving results to a convenient directory

Checking resulting files from a workflow analysis as shown above is quite tedious especially when there are several processes inside a workflow. Nextflow provides an easy way to collect resulting files to a convenient place using a special directive, *publishDir*.

Open *fastqc.nf* script in any text editor and uncomment (= remove double slashes) the following line:

```nextflow
// publishDir 'results' 
```
and then run pipeline again. But this time, let's use '-resume' flag as we don't need to perform quality control analysis again so that actual analysis is skipped due to the capability of nextflow to track cached results from the previous analysis.  

```nextflow
nextflow run fastqc.nf -resume
```
Once the script is run successfully, you can check the files:

```
ls -l results/
````
By using `-resume` flag, the resulting files from previous analysis are simply copied to folder 'results' directory.

### Understanding nextflow channels and operators 
We don't spend time here learning different ways of creating channels and operators. But it is worth noting that there are different chanels and operators as core features if nextflow. [Channels](https://www.nextflow.io/docs/latest/channel.html) are like streams of files (or parameter values) you put through your pipeline and also support different data types like `file`, `val` annd `set`

Here are few examples on how one can create channels in nextflow script:
```nextflow
Channel.create(); Channel.empty; Channel.fromPath()
 ```
This default semantics can be changed using the channel operators that Nexflow provides, some of which are shown below:

```nextflow
split         merge         close
filter        map/reduce    group
join, merge   fork          view
```

## Tutorial 3: Nextflow pipeline with containers and other useful features

In this tutorial, you will learn nextflow script that uses 
 - singularity container
 - user-defined profiles
 - visualisation capabilities
 - batch job script to deploy a pipeline on Puhti

Containerised applications are highly portable and reproducible for scientific applications. Fortunately, Nextflow smoothly supports integration with popular containers ( e.g., [Docker](https://www.nextflow.io/docs/latest/docker.html) and [Singularity](https://www.nextflow.io/docs/latest/singularity.html)) to provide a light-weight virtualisation layer for running software applications. You can either create your own Docker/Singularity image or download pre-existing one from a container registry. Please note that you can only work with *Singularity* containers on Puhti as *docker* containers require prevelized access which CSC users **don't** have on Puhti.

When working with Nextflow scripts using containers, pay attention to the following things:
- Nextflow usually takes care of mounting work directory for that process on container image. Other files/directories need to be mounted through run time options if needed.
- Required input files are staged in and out automatically via Nextflow mechanisms.
- Nextflow process gets executed in a container environment

### Launch an example nextflow application 

Let's download material needed for this tutorial from github as shown below:

```nextflow
cd nextflow_tutorial 
git clone https://github.com/yetulaxman/nf_coverage_demo.git
cd nf_coverage_demo
git clone https://github.com/iarcbioinfo/data_test

```

### Using containers with nextflow *via* commandline option
Here is a simple example syntax (for an alternative approach, see *profiles* section below) to use docker/singularity containers: 

```bash
## For Docker
nextflow run <nextflow_script>  -with-docker <image_path> # e.g.,image_path = docker://biocontainers/fastqc:v0.11.9_cv7

## For Singularity
nextflow run <nextflow_script>  -with-singularity <image_path>    
```

Because of the way how nextflow works with containers, you **don't** need to have software (e.g., `fastqc`) installed on your machine. It will download container image and  uses *fastqc* from the image.

### Using containers with nextflow *via* profiles

We often need to add some other attributes besides a container flag as mentioned above. This is accomplished using *profiles*. A profile is a set of configuration attributes that can be activated/chosen when launching a pipeline execution.  When a workflow script is launched, Nextflow first looks for a file named `nextflow.config` in the current directory and in the script base directory (if it is not the same as the current directory). Finally, it checks for the file $HOME/.nextflow/config. Configuration files can contain the definition of one or more profiles. 

Example profiles are shown below:

```nextflow
profiles {


 docker {
    docker.enabled = true 
    process.container = 'iarcbioinfo/nf_coverage_demo:v2.3'
    pullTimeout = "200 min"
  }
  
  singularity {
    singularity.enabled = true 
    singularity.autoMounts = true
    process.container = 'shub://IARCbioinfo/nf_coverage_demo:v2.3'
    pullTimeout = "200 min"
  }
}
```

open `nextflow.config` file in the current directory and copy and paste above script inside the file.

You can then launch nf_coverage workflow with defined profiles as shown below:

```bash
nextflow run plot_coverage.nf  \
          -profile singularity \
          --bam_folder data_test/BAM/BAM_multiple/ \
          --bed data_test/BED/TP53_exon2_11.bed
```

### Visualising nextflow pipeline
Nextflow supports several visualisation tools:
```bash
-with-dag
-with-timeline
-with-report
```
You can either use the flags in commandline or create profile for each visualisation feature as discussed below:

####  `dag`
Either use the following flag (-with-dag) when launching script as below:

```
nextflow run  <nextflow_script>  -with-dag <file-name>.dot
```
or add the following script to `nextflow.config` file at the end.

```
dag {
  enabled = true
}
```
####  `timeline`

Either use the following flag (-with-timeline) when launching script as below:
```
nextflow run <nextflow_script> -with-timeline <file-name>.html
````
or add the following script to `nextflow.config` file at the end.

```
timeline {
  enabled = true
}
```
####  `report`
Either use the following flag (-with-report) when launching script as below:

```
nextflow run <nextflow_script> -with-report <file-name>.html
```
or add the following script to `nextflow.config` file at the end.
```
report {
  enabled = true
}
```

### `trace`

Either use the following flag (-with-trace) when launching script as below:

```
nextflow run <nextflow_script> -with-trace <file-name>.txt
```
or add the following script to `nextflow.config` file at the end.

 ```
trace {
  enabled = true
}
```

For the convenience of this tutorial, configure all visualisation features (i.e., dag/timeline/reports/trace) into `nextflow.config` file.

### Launch nextflow application as a batch job

Once you have configured profiles for singularity and enabled visualisation features in *nextflow.config* file, you can use the following batch script to submit on Puhti:

```
#!/bin/bash
#SBATCH --time=00:10:00
#SBATCH --partition=test
#SBATCH --account=project_xxx


export TMPDIR=$PWD
export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR


# Activate  Nextflow on Puhti
module load bioconda
source activate nextflow

# Nextflow command here
nextflow run plot_coverage.nf  \
       -profile singularity  \
       --bam_folder data_test/BAM/BAM_multiple/  \
       --bed data_test/BED/TP53_exon2_11.bed 
```
copy and paste above script to a file (nf_coverage.sh), replace with course project number in slurm directives and submit sbatch script to Puhti cluster:

```
rm -fr work/    # remove previous analysis results
rm *.html *.dot *.txt # remove these visualisation files if any
sbatch nf_coverage.sh # start a fresh job
```
###  Nextflow with 'slurm' executor on Puhti (Currently NOT recommended)

One of the advantages of nextflow is that the actual pipeline functional logic is separated from the execution environment. The same script can be executed in different environment by changing the execution environment. Nextflow uses the `executor` information to decide where the job should run. Once executor is configured, Nextflow submits each process to the specified job scheduler on your behalf.

Default executor is `local` where the  process is run in your computer/localhost where Nextflow is launched.  Other executors include:

- PBS/Torque
- SLURM
- Amazon (AWS Batch)
- SGE (Sun Grid Engine)

To enable the SLURM executor on Puhti, simply set  `process.executor` property to slurm value in the `nextflow.config` file as shown below:

```
profiles {


 standard {
     process.executor = 'local'
   }

 puhti {
     process.clusterOptions = '--account=project_xxxx --ntasks-per-node=1 --cpus-per-task=4 --ntasks=1 --time=00:00:05'
     process.executor = 'slurm'
     process.queue = 'small'
     process.memory = '10GB'
    }
}
```

In this case, you can run a nextflow script as below: 

```
nextflow run <nextflow_script> -profile puhti
```
This will submit each process of your job to Puhti cluster.

### Viewing the results on Puhti

Copy all nextflow visualisation files (i.e., .html, .dot and .txt files) to home directory to view them in your local browser.

Every participant should have a *unique port number* opened on Puhti login node to view files locally on your browser. Open a new terminal on your local machine and replace *$port* value with some random number (e.g., a number between 5000 and 9000) before executing the following command:

```
ssh -L $port:localhost:$port <your_csc_username>puhti.csc.fi  # e.g., with port number: 7077 
                                                              # ssh -L 7077:localhost:7077 <username>puhti.csc.fi 
```
and then run the following command (also use the same port value that you have slected before) on the login node:
```
python3 -m http.server $port # with port number: 7077 -> python3 -m http.server 7077
```

Point your browser to http://localhost:$port (remember to replace your port number with *$port*) on your local machine. You can now view all files available on your Puhti home directory. 


## (Bonus) Tutorial 4: Running a nf-core pipeline

*nf-core* is a community effort to collect a curated set of analysis pipelines built using Nextflow. Here we use [nfcore/atacseq](https://github.com/nf-core/atacseq) as an example pipeline for ATAC-seq data.

Here is an example batch script to run the pipeline on Puhti:

```
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000

export TMPDIR=$PWD
export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR

# Activate  Nextflow on Puhti
module load bioconda
source activate nextflow

# Nextflow command here
nextflow run nf-core/atacseq -r 1.2.1 -profile test,singularity -resume
````
copy and paste the above script to a file named `atacseq.sh` and replace your project number with `project_xxxx` in slurm directives. 

Finally, submit your job 

```
sbatch atacseq.sh
```

## (Bonus) Converting a bioconda package into singularity image

We recommend using singularity containers over conda environment to work with nextflow pipelines on Puhti. You can convert the most of bioconda packges into singularity images. You can for example take a look at *tastx_toolkit* package as available on [bioconda page](https://bioconda.github.io/recipes/fastx_toolkit/README.html). In order to convert it into a singularity image, you need to know the *url* of image from the docker pull command which in this case appears as below:

```
docker pull quay.io/biocontainers/fastx_toolkit:<tag>   # url: docker://quay.io/biocontainers/fastx_toolkit:<tag>
```

In addition to the url of a docker image, it is good practice to use a specific tag for reproducibility of workflows.  Here, you can pick a tag (e.g., "0.0.14--he1b5a44_8") from [list of tags](https://quay.io/repository/biocontainers/fastx_toolkit?tab=tags) associated with fastx_toolkit sofwtare.

 you can then use the the following script on Puhti interactive terminal to prepare *fastx_toolkit* singularity image:

```nextflow
export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR
singularity build fastx_toolkit.sif docker://quay.io/biocontainers/fastx_toolkit:0.0.14--he1b5a44_8
```

Once above script is successfully executed, there should be  a singularity image named `fastx_toolkit.sif` in current folder
