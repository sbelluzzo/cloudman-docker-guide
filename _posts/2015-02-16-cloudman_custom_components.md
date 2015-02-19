---
layout: page
title: Building custom CloudMan components
---

##CloudMan Default/Public Bucket

Creating a new bucket is the fastest way to customize [CloudMan][cm], in fact cluster buckets are effectively this.

The key files inside a CloudMan default bucket, as detailed in CloudMan Architecture, are:

- `cm_boot.py`
- `cm.tar.gz`
- `snaps.yaml`

The bucket often will also contain tarball archives of storage snapshots referred to by `snaps.yaml`, as well as configuration files to be used by tools (such as Galaxy, suffixed by ".cloud").

`cm_boot.py` and `cm.tar.gz` can be sourced from the [CloudMan repository][cm] (`cm_boot.py` just copied from the repository, `cm.tar.gz` produced by making a tarball of the repository), or if setting up a bucket on a cloud that already has one, by copying from an existing bucket. This is probably best if not working on an Amazon cloud, as sometimes modifications are made to allow CloudMan to function.

The `snaps.yaml` file has a relatively self-explanatory structure, with a basic example using only volume-snapshots in the [CloudMan][cm] repository. A slightly more complex example is the default [NeCTAR][nectar] [CloudMan bucket][nectar-snap], which has examples of using volume-snapshots, tarball archives and GlusterFS network mounts. NFS and S3FS are also supported.

---

##CloudMan Machine Image & GalaxyFS

The [CloudMan][cm] Machine Image and GalaxyFS are unlikely to need to be built, unless there is a major change required, as was the case here, due to compatibility issues and an unpcoming new release of the [GVL][gvl] machine image we wanted to be compatible with. The process is documented in the containing [Ansible playbook repo][playbook], as well as on the [Galaxy Wiki][wiki-details].

Some modifications were made to the playbooks, these can be found in [this forked repository][local-cm], but also are in the process of being submitted upstream.

[cm]: https://bitbucket.org/galaxy/cloudman
[gvl]: https://genome.edu.au/wiki/Get
[local-cm]: https://github.com/simonalpha/galaxy-cloudman-playbook
[nectar]: http://nectar.org.au/
[nectar-snap]: https://swift.rc.nectar.org.au:8888/v1/AUTH_377/cloudman-os/snaps.yaml
[playbook]: https://github.com/galaxyproject/galaxy-cloudman-playbook
[wiki-details]: https://wiki.galaxyproject.org/CloudMan/Building

