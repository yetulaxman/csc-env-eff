---
title: Proteomics Web Application
---

# Proteome Network analysis using containerised webapplication #

Phosphoproteomic experiments typically identify sites within a protein that are differentially phosphorylated between two or more cell states. However, the interpretation of these data is hampered by the lack of methods that can translate site-specific information into global maps of active proteins and signaling networks, especially as the phosphoproteome is often undersampled. Here, we use  PHOTON, a method for interpreting phosphorylation data within their signaling context, as captured by protein-protein interaction networks, to identify active proteins and pathways and pinpoint functional phosphosites. 

## Learning Objectives
- Start a containerised webb app for analysing phosphoproteomics dataset
- Perform analysis using a test dataset



## Basic usage of docker
- [Explore more about photon here](https://hub.docker.com/r/jdrudolph/photon)
- Pull docker image from dockerhub

```
docker pull jdrudolph/photon:dev

```
- Launch web-application as below:

```bash
 docker run -d -p 5000:5000 jdrudolph/photon:dev
```

Now you can access PHOTON from your browser at: http://localhost:5000

1. Construct a phosphoproteomics signaling networks and identify the positively regulated neighbours of EGFR  protein?
2. Explain your experience of using a containerised web-application like Photon. How easy or hard it would be if you were to install all dependencies of photon to get it work?

