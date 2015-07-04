---
layout: page
title: Dockerizing your own tools for Galaxy
---
This process involves two stages; altering the tool wrapper for Galaxy, and preparing the Docker image containing the tools.

## Tool Wrapper
Altering the tool wrapper in most cases is as simple as adding a single line in the requirements tag:

`<container type="docker">USERNAME/IMAGE_NAME[:tag]</container>`

where `USERNAME` and `IMAGE_NAME` are your username and image name on Docker Hub. Use of tags is recommended, allowing the availability of multiple versions and matching wrappers against Docker images.

## Docker image
Building a Docker image can be simple or very complicated, depending on your tools. The overall goal is the have the required binaries/scripts on the path of the image, with the required dependencies installed. However, size is an important factor to keep in mind. An understanding of the Docker image/container graph, and how snapshots work is likely to be useful for larger tools. See the [Docker docs](https://docs.docker.com) for more detail.

In most cases, we suggest using a Dockerfile, something akin to a modified shell script. This has the advantages of easy use of Docker Hub for building and distribution, and being transparent in the steps involved. Further detail can be found on the [Protk image](http://127.0.0.1:4000/cloudman-docker-guide/2015/02/16/protk_docker_image.html) page, or for Dockerfiles, the [Docker docs](https://docs.docker.com).

[NCBI-BLAST](http://blast.ncbi.nlm.nih.gov) is an example of a simple tool to produce a Dockerfile for, as its a set of precompiled binaries. An example Dockerfile can be found [here](https://github.com/simonalpha/ncbi-blast-docker/blob/master/ncbi-blast-2.2.30%2B/Dockerfile); and has the basic steps of installing the requirements for the Galaxy BLAST wrapper (Python) and curl to download BLAST, downloading the BLAST tarball and unarchiving it, and adding the resulting directory to the path. This particular [repository](https://github.com/simonalpha/ncbi-blast-docker) also has other Dockerfiles to provide other BLAST versions.

A more complicated Dockerfile is the [ProtK Dockerfile](https://github.com/iracooke/protk-dockerfile/blob/master/protk-1.4.1/Dockerfile), where the majority of work is done in a single RUN command, to avoid the increase in size that would otherwise be present due to the layering/caching methods of Docker. This was necessary to have the build tools available to build the ProtK dependencies, but not present in the final production image.

Both of these repositories are hooked to [Docker Hub](https://hub.docker.com), so that when the repositories change, the images will automatically be rebuilt, and made available for download. Details are on the [Docker docs](https://docs.docker.com).
