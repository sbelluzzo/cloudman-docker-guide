---
layout: page
title: Galaxy & Docker Integration
---
Running in a job runner with Docker enabled: will automatically pull down image and run.
Running in a job runner without Docker enabled: ignores container tag, uses installed dependencies.

As long as the runtime for any wrapper scripts is available in the image, and required binaries are on the image's path, any execution using Docker works.

Dev-mapper backend for Docker occasionally fails to run Docker containers, leading to failed jobs in Galaxy. Rerunning the job in most cases will work. Updates to the kernel and Docker should resolve this. Alternative solution is to use another backend, for instance AUFS (out-of-tree kernel module).

Container numbers can build up, filling the root partition, as the default Docker graph directory is /var/lib/docker. More recent versions of Galaxy have an option in `job_conf.xml` to autoremove after running; while this is not default, it is highly recommended. Older versions don't have this, so care is necessary: monitoring of space or change the graph directory location.

By default, Galaxy runs all containers with no networking. However, some tools require network access. Network access can be activated in `job_conf.xml` with a `<param id="docker_net">true</param>` tag in each job destination that network access is required in. Unfortunately, this is an all or nothing option currently. Options are to allow all, allow none and adapt wrappers to use locally installed tools for these instances, or set up dynamic dispatch of jobs based on tool requirements.
