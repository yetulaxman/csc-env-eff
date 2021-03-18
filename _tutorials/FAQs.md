
## (WIP) This section is in progress

### Where does docker store all of its files on host machine?
use `docker info`  command to spot docker root directory 
### How to change the default folder of docker workspace?
```
docker info # to get the current location of docker root dir
sudo systemctl stop docker
mkdir /mnt/docker-base
chown -R root:root /mnt/docker-base
vi /etc/docker/daemon.json
add the following:

{
    "graph": "/mnt/docker-base"
}

sudo systemctl daemon-reload
sudo systemctl start docker
docker info # check again new root dir of docker
docker run hello-world # perform some test
ls -l /mnt/docker-base
```

## how do we ensure the uniqueness of image while pulling from a registry?
Tags are more frequent way to ensure the unqiuess of images. While  most application software are updated usually with new tag, it is possible to have fewer digests under one tag. Digest is a content-addressable identifier. Image reference with digest value provides better uniqueness in production setting.

## how can you convert a docker image (e.g.,fastx software) on bioconda environment to singularity container?

Visit the fastx software tool available on the bioconda page (https://bioconda.github.io/recipes/fastx_toolkit/README.html) and take path of the image from  docker pull command from above page as below:

```

docker pull quay.io/biocontainers/fastx_toolkit:<tag>
```

Path for above image is here: docker://quay.io/biocontainers/fastx_toolkit:<tag>

And pick your tag from here: (https://quay.io/repository/biocontainers/fastx_toolkit?tab=tags)

And now, you can use any linux machine that has singularity installed 

Use the following command to convert a docker image to singularity using tag (here i picked 0.0.14--he1b5a44_8) :

```

singularity build fastx_toolkit.sif docker://quay.io/biocontainers/fastx_toolkit:0.0.14--he1b5a44_8

```

Alternatively,  on Puhti interactive terminal, you can use the the following commands:

```

export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR
singularity build fastx_toolkit.sif docker://quay.io/biocontainers/fastx_toolkit:0.0.14--he1b5a44_8

```
