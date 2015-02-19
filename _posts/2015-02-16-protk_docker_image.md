---
layout: page
title: ProtK Docker Image
---

##TL;DR
Assuming [Docker][docker] is installed, to use an 'Automated Build' image from [Docker Hub][docker-hub]:

- Run `docker pull simonalpha/protk` to fetch latest version of image.
- Run `docker run -it simonalpha/protk` to start a container and reach a bash prompt. The [ProtK][protk] tools are on the path, and can be accessed without further effort.
- For actual use, read the next section.

To build your own, again assuming [Docker][docker] is installed:

- Fetch the [protk-dockerfile][protk-dockerfile] repository.
- Run `docker build -t <name> .` inside the directory of the version you wish to build.
- Run `docker run -it <name>` to start a container and reach a bash prompt. The [ProtK][protk] tools are on the path, and can be accessed without further effort.

##Using the ProtK image for real analysis

###TL;DR

- Use `-v <host_dir>:<container_mount_point>` to mount directories from host to a container.
- Use `-w <container_dir>` to set a location in the container as the default working directory.
- Use `-u $(id -u)` to allow the user inside the container to access mounted data.

###Getting Data In & Out
We feel the best approach is to mount directories from the host and work inside them, thus allowing the container to remain ephemeral and prevent your data being trapped inside a multitude of stopped containers.

Directories can be mounted using the `-v <host_dir>:<container_mount_point>` flag, where `<host_dir>` is the directory on the host to be mounted and `<container_mount_point>` is where it is to be mounted in the launched container. Any files altered or written in this location will be persisted on the host. The `-v` flag can be used multiple times to mount multiple locations.
A mount point (or any other location in the container) can be designated as the default working directory with `-w <container_mount_point>`. **Important:** in each of these cases, the path must be absolute (ie be from /).

Further information is available in the [Docker docs](https://docs.docker.com/userguide/dockervolumes/).

###File permissions
File permissions are tricky, especially with host-mounted volumes, given different user and group IDs between the host and containers possibly preventing reading or writing. The current method we use to work around this is to have `root` as the default user inside the ProtK container, which is not fantastic security practice. However, we intend to alter this soon, as [Galaxy][galaxy] now supports setting the user to run in a container. For security and future-proofing, we recommend running as detailed below.

To allow access to files mounted from host, we want the user id to match inside and outside the host. This can be accomplished with the `-u` flag, in conjunction with the `id` tool. To run with the same uid as the user starting the container (typical use case), use the flag `-u $(id -u)`.



##Image Construction

The Dockerfile for building this image is available in the [protk-dockerfile][protk-dockerfile] repository.

###Design Choices
- Based off Debian stable for small size/stability
- Current Dockerfile approach chosen for use of Automated Builds:
  - Provides transparency for end-users
  - Alleviates build environment requirements locally
  - Easily maintained via GitHub repository pushes and hooks
- However:
  - Non-separate build and deploy images - not as clean/small as possible
  - Majority of logic in one step to reduce build artifacts in layers - reduces ability to cache
  - May be able to refactor when Dockerfile v2 or nested builds land
- Alternatives:
  - Local build: Dockerfile easily converted into script
  - Use CI to build, pull into Docker image by hand or Automated Build
  - Packer (requires more tooling, not as transparent)
  - Use Ansible to control Docker build process (take artifacts and copy into deploy image)

Build and deploy envs would work well if same env used for both, Debian recommended base as small and full-featured.

Initial steps - get build tools
Install ruby using ruby-install and delete when complete
Install ProtK from RubyGems, use to run install process for tools

Improvements:

- ProtK fix to allow multiple, or even meta args
- Reduction of TPP use, may be possible to get rid of most of cgi-bin and html
- sharing of BLAST or other tools with other tools/ use smaller containers (requires changes in Galaxy)

[docker]: http://www.docker.com/
[docker-hub]: https://registry.hub.docker.com/
[protk]: https://github.com/iracooke/protk
[protk-dockerfile]: https://github.com/iracooke/protk-dockerfile
