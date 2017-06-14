Cadfael Containers
==================
Experimental repository for containerizing build and integration
testing SuperNEMO software.

Aim is not for distributing software as a container, rather as an
easy to setup multiplatform testing and validation.

Currently only using Docker, but other ideas should be explored (Rocket,
isolated glibc envs on Linux).

Installing Docker
=================
Linux
-----
TODO

Mac OS X
--------
- Install [Docker Toolbox](https://www.docker.com/docker-toolbox).
- Start Docker Quickstart Terminal
  - This should create a VM if none exists, then start it
  - Review Docker toolbox for guides on increasing disk quota/RAM/CPU

Supported Container Distros
===========================
The same Linuces as those supported for Cadfael

- RedHat Family 6/7 (inc. Scientific Linux, CentOS)
- Ubuntu 14.04 LTS

Other distros will be added as required based on use cases and availability of docker images

Testing on Mac OS X should use the native system or VMs from Parallels or other.

Build/Run Containers
====================
- To build the container from the Dockerfile in the cwd:
  ```
  $ docker build -t <containername> .
  ```
  It's recommends that `<containername>` be `cadfael-<distroid><distroversion>`, e.g. "`cadfael-ubuntu1404`".

- Check images for latest builds
  ```
  $ docker images
  ```

- Run the container
  ```
  $ docker run -t <containername>
  ```

- Run container interactively
  ```
  $ docker run -i -t <containername> /bin/bash
  ```
