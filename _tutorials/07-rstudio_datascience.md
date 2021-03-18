---
title: Serving Containerised Web Application - Rstudio Demonstration
---
 RStudio is a graphical interface to the software R and is heavily used in statistical data analysis. Main advantages of using Rstudio as a docker container include:
  -  Reproducibility of analysis results
  -  Collaboration with colleagues
  
Learning Objectives:
- Launching RStudio as a docker container
- Performing a simple data analysis task using R


## How to run (recap from earlier tutorial)

Run rstudio with default password as below:
```
docker run -d -p 8080:8787 -i -t rocker/rstudio
```

Launch rstudio with new password as below:

```
docker run -d -p 8080:8787 -e USERPASS=secretpassword  -i -t rocker/rstudio
```

## How to access rstudio server

To access the app, point your web browser at  http://localhost:8080/

## Perform PCA analysis

Principal component analysis (PCA) is a method of extracting important variables (in form of components) from a large set of variables available in a data set. It extracts low dimensional set of features from a high dimensional data set with a motive to capture as much information as possible.

we will use prcomp function from `stats` package. 

```bash
data(iris)
log.ir <- log(iris[, 1:4])

ir.pca <- prcomp(log.ir,
                 center = TRUE,
                 scale. = TRUE) 
# plot method
plot(ir.pca, type = "l")
```

## Install an R package called "ggplot2" in rstudio

More often we need to install some R packages in rstudio to perform our analysis. Let's try to install ggplot2 package here as an example.
```
# On Rstudio GUI use the following command
install.packages("ggplot2", type = "source")
or 
# commandline approach
docker exec -it <CID> bash 
R -e  'install.packages("ggplot2", type = "source")'
```
