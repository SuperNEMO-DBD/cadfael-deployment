Cadfael Containers
==================
Experimental repository for containerizing build and integration
testing of Cadfael and then SuperNEMO software.

Aim is not for distributing software as a container, rather as an
easy to setup multiplatform testing and validation.

Currently only using Docker, but other ideas should be explored (Rocket,
isolated glibc envs on Linux).

Working on Mac with `boot2docker`
=================================
To increase boot2docker VM disk size, see [this guide](https://ryanfb.github.io/etc/2015/01/28/increasing_boot2docker_allocations_on_os_x.html)
on increasing it using a user profile file.

TODO: Add disk space loggin in dockerfiles to log what a full Cadfael
install requires.



