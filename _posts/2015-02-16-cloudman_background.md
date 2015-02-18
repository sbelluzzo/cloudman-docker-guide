---
layout: page
title: CloudMan Architecture
---
![CloudMan Architecture]({{ site.baseurl }}/images/cloudman-arch.jpg)
Schematic of CloudMan Architecture, from [Afgan *et al* 2010][afgan2010].

## Launcher
The [CloudMan Launcher](https://biocloudcentral.herokuapp.com) and variants (such as the [GVL Launcher](http://launch.genome.edu.au)) take on a number of roles; in particular providing a user-friendly interface to the options for starting a [CloudMan][cm_web] instance, provisioning a virtual machine from a [CloudMan][cm_web] image, and ensuring that the virtual machine is provided with the necessary metadata for finding public or private storage containing [CloudMan][cm_web] itself, in addition to more configuration data and details about storage to be mounted.

## Machine Image
The [CloudMan][cm_web] Machine Image provides a relatively known starting place for [CloudMan][cm_web], providing required software, virtualenvs, users, and most importantly, an Upstart job and `ec2autorun.py`, a Python script called by it. This script downloads `cm_boot.py` from object storage or default url and runs it, in turn downloading and bootstrapping the [CloudMan][cm_web] instance. The image requires appropriate metadata to be supplied on boot (usually by the Launcher), otherwise it behaves like an ordinary Ubuntu instance.

## Storage Snapshots
Public storage snapshots contain software components that can be managed by [CloudMan][cm_web] (eg [Galaxy][galaxy] and associated tools) or preloaded data (eg indices for [Galaxy][galaxy]). These are defined in the configuration bucket in the `snaps.yaml` file. They can take the form of archives to be unpacked, volume snapshots, or network-based mounts (Gluster, NFS and S3FS). Due to cloud limitations, volume snapshots must be in the same availablitiy zone as the instances they are to be used with.

Cluster data, resulting from analysis, is also stored in volume snapshots. These are defined in the `persistent_data.yaml` file in the private cluster bucket.

## Persistent data storage (Buckets)
[CloudMan][cm_web] buckets hold the configuration that brings all the components together, and are usually hosted on cloud object storage (S3, Swift or equivalent) There are two varieties: default/public (read-only), and cluster/private. Default buckets are used to bootstrap new clusters, which then create their own bucket to store configuration particular to them.

Typical components present in a default bucket are:
- `cm_boot.py`: a script to download and bootstrap [CloudMan][cm_web] from an accompanying `cm.tar.gz` tarball.
- `cm.tar.gz`: a tarball containing the [CloudMan][cm_web] distribution to use for clusters started using this bucket.
- `snaps.yaml`: a file containing details of snapshots that need to be mounted for types of clusters or different regions

However, a cluster bucket usually contains `cm_boot.py` and `cm.tar.gz` as before, but also contains `persistent_data.yaml`, a file describing the cluster; and an empty file with the cluster name, with the filename in the form `<cluster name>.clusterName` (eg `myCluster.clusterName`).

The cluster bucket name (in the form `cm-d52c2f263e5f47bb6cf7e709da9f0dea`) is related to the name of the cluster, allowing easy discovery for rebuilding a previously run cluster.


### References
- [CloudMan website][cm_web]
- [CloudMan source][cm_src]
- [Afgan, Enis *et al.* "Galaxy CloudMan: delivering cloud compute clusters." *BMC Bioinformatics* 11.Suppl 12 (2010): S4.][afgan2010]

[afgan2010]: http://doi.org/10.1186/1471-2105-11-S12-S4
[cm_src]: https://bitbucket.org/galaxy/cloudman
[cm_web]: https://wiki.galaxyproject.org/CloudMan
[galaxy]: http://galaxyproject.org/
