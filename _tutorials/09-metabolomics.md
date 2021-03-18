---
title: Containerised Metabolomics Application
---
## Learning objectives
On completion of this session, you will learn:
- To explore a containersied metabolomics application and perform some data pre-processing steps.

# Tutorial with dimspy 

In general, when we look for a docker container for our needs, we may end-up finding several relevant tools for our needs. It may take some time to  get used to new tools and often difficult to judge the quality of tools if it is not a popular one. Getting an unfamiliar tool to work for our needs is a good learning process. We will practice it using dimsy software.

More information about the software can be found [here](https://github.com/computational-metabolomics/dimspy).

### Pull dimpsy container from dockerhub

```bash
docker pull quay.io/biocontainers/dimspy:2.0.0--py_0
Make a note about the *tag* we used here. What are all other tags present in this software? ÷

### Get documentation help from Dimpsy tool

```bash
docker run -it quay.io/biocontainers/dimspy:2.0.0--py_0 bash
dimspy --help
```
what are the different processing steps that dimspy can perform on metabolomics data?

Once you have got some idea about different processing steps.

### Download zip file containing data

You can now download some tutorial data from here:

```bash 
wget https://github.com/computational-metabolomics/dimspy-galaxy/raw/master/tools/dimspy/test-data/MTBLS79_mzml_triplicates.zip

```

and  use the following files list:

```bash
filename	replicate	batch	injectionOrder	classLabel
batch04_B02_rep01_301.mzML	1	1	1	blank
batch04_B02_rep02_302.mzML	2	1	2	blank
batch04_B02_rep03_303.mzML	3	1	3	blank
batch04_QC17_rep01_262.mzML	1	1	4	QC
batch04_QC17_rep02_263.mzML	2	1	5	QC
batch04_QC17_rep03_264.mzML	3	1	6	QC
batch04_S01_rep01_247.mzML	1	1	7	sample
batch04_S01_rep02_248.mzML	2	1	8	sample
batch04_S01_rep03_249.mzML	3	1	9	sample

```

save both zip file and a file containing files list (name it as `filelist_csl_MTBLS79_mzml_triplicates.txt`) in a directory (e.g., /home/biouser/dimspy).


You can now unzip the mzxml files using the following command from dimspy software:

```
docker run -it -v /home/biouser/dimspy:/data quay.io/biocontainers/dimspy:2.0.0--py_0 bash  # go inside the container

# run the following command inside the container
dimspy unzip \
--input /data/MTBLS79_mzml_triplicates.zip \
--output results/
```

Check how many files are there?

Now you can  run the *process-scans* step using the following command:

### Perform couple of metabolomics processing steps

```
dimspy process-scans \
--input /data/results/ \
--output /data/results/peaklists.hdf5 \
--filelist /data/filelist_csl_MTBLS79_mzml_triplicates.txt \
--function-noise median \
--snr-threshold 3.0 \
--ppm 2.0 \
--min_scans 1 \
--min-fraction 0.5 \
--block-size 5000 \
--ncpus 1

```

Report the output files you have got from this step?

Run the following steps one by one :

```
dimspy replicate-filter \
--input results/peaklists.hdf5 \
--output results/peaklists_rf.hdf5 \
--ppm 2.0 \
--replicates 3 \
--min-peak-present 2

dimspy replicate-filter \
--input results/peaklists.hdf5 \
--output results/peaklists_rf.hdf5 \
--ppm 2.0 \
--replicates 3 \
--min-peak-present 2

dimspy align-samples \
--input results/peaklists.hdf5 \
--output results/pm_a.hdf5 \
--ppm 2.0

dimspy blank-filter \
--input results/pm_a.hdf5 \
--output results/pm_a_bf.hdf5 \
--blank-label blank \
--remove

dimspy sample-filter \
--input results/pm_a_bf.hdf5 \
--output results/pm_a_bf_sf.hdf5 \
--min-fraction 0.8

dimspy hdf5-pls-to-txt \
--input results/peaklists_rf.hdf5 \
--output results \
--delimiter tab

dimspy hdf5-pm-to-txt \
--input results/pm_a_bf_sf.hdf5 \
--output results/pm_a_bf_sf.txt \
--delimiter tab

dimspy merge-peaklists \
--input results/peaklists_rf.hdf5 \
--input results/peaklists.hdf5 \
--output results/peaklists_merged.hdf5

```
 Please note that once you exit (using control+ p and then contrl +q) from the container after analysis, all the resulting files from analysis should be in directory: /home/biouser/dimspy
