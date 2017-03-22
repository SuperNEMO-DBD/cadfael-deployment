CentOS Docker Containers
========================
Dockerfiles and files for RHEL/CentOS/SL family of distros. These are
for testing Cadfael on cluster systems typically in use in HEP and on
the Grid. The Docker builds therefore aim to match these systems as
close as possible to sites where LHC software may run. It's expected
that differences between RHEL/CentOS/SL are minimal as affects Cadfael.
All versions use the `HEP_OSlibs` metapackage(s) as defined by the WLCG
which (largely) provides the system RPMs needed on top of the base
system for running LHC experiment software. This is done to provide
maximum ease of use and compatibility.

The sections below describe the setup of each version/variant of RHEL
used in Docker. You can also use these on your own systems to match the container builds.

6 Series
========
- Docker base image: [centos:centos6](https://registry.hub.docker.com/_/centos/)

7 Series
========
- Docker base image: [centos:centos7](https://registry.hub.docker.com/_/centos/)


