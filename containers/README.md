Cadfael Containers
==================
Experimental repository for containerizing build and integration
testing of Cadfael and then SuperNEMO software.

Aim is not for distributing software as a container, rather as an
easy to setup multiplatform testing and validation.

Currently only using Docker, but other ideas should be explored (Rocket,
isolated glibc envs on Linux).

Installing Docker
=================
Mac OS X
--------
- Install [Docker Toolbox]([Docker Toolbox](https://www.docker.com/docker-toolbox).
- Start Docker Quickstart Terminal
  - This should create a VM if none exists, then start it

Working on Mac with `boot2docker`
=================================
NB: boot2docker is now deprecated. [Docker Toolbox](https://www.docker.com/docker-toolbox) should be used instead.

To increase boot2docker VM disk size, see [this guide](https://ryanfb.github.io/etc/2015/01/28/increasing_boot2docker_allocations_on_os_x.html)
on increasing it using a user profile file.

TODO: Add disk space logger in dockerfiles to log what a full Cadfael
install requires.

Build/Run Containers
====================
- To build the container from the Dockerfile in the cwd:
  ```
  $ docker build -t <containername> .
  ```

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
