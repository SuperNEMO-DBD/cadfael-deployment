SuperNEMO Docker Containers
===========================

Dockerfiles and additional tools for building images of the SuperNEMO
software for testing and deployment. Four core platforms are
supported for Continuous Integration:

- CentOS 6, 7
- Ubuntu 14.04LTS, 16.04LTS

Ubuntu 16.04LTS is supported as the primary distribution for images
of the full software stack.


Installing and Using Docker
===========================

See the [comprehensive guides on the official docker site](https://docs.docker.com).


Getting Images and Running Containers
=====================================

If you simply wish to run the [Falaise](https://github.com/supernemo-dbd/falaise) software,
the `supernemo/falaise` image supplies an Ubuntu 16.04 LTS system with all the
needed runtime and development tools available. A standard interactive session
may be started via:

```
$ docker pull supernemo/falaise
$ docker run -ti supernemo/falaise
```

This will drop you into the running container in a `bash` shell with the environment
set as required to run `Falaise` programs. See the [Falaise documentation](https://supernemo-dbd.github.io/falaise)
for information on these programs. When finished, you can exit the
container by closing the shell session with the `exit` command.

The container may also be run in a "detached" mode that allows you
to run commands in the container from the host system:

```
$ docker run -itd --rm --name <ID> supernemo/falaise
$
```

where `<ID>` is a "hostname" with which to communicate with the container (e.g.
"MyContainer"). The `--rm` flag is used so that the container is removed when
it is stopped, helping to keep your docker environment clean and allowing the `<ID>`
name to be reused later.

After executing this command, you will still be running in a shell on your host
system. Commands may then be run in the container with the `docker exec` command:

```
$ docker exec <ID> which flsimulate
/home/linuxbrew/.linuxbrew/bin/flsimulate
$
```

The container retains its state between calls to `exec`. When finished, the container
can be stopped with

```
$ docker stop <ID>
```

this stops the container, and also removes it as we passed the `--rm` flag above.

By default, no data is shared between the host system and the container,
so you cannot, for example, save the output of an `flsimulate` run to your
host system for further use. [Docker Data Volumes](https://docs.docker.com/engine/tutorials/dockervolumes/)
allow you to persist data between container and host. In its simplest form,
we can share a directory between the host and container using the `-v hostdir:containerdir`
argument, e.g.

```
$ ls /my/host/dir
my_simulation.conf
$ docker run -itd --rm --name <ID> \
-v /my/host/dir:/shared_data
supernemo/falaise
...
$ docker exec <ID> ls /shared_data
my_simulation.conf
$ docker exec <ID> flsimulate -c /shared_data/my_simulation.conf -o /shared_data/my_simulation.brio
...
$ ls /my/host/dir
my_simulation.brio my_simulation.conf
$
```

See the [documentation on data volumes](https://docs.docker.com/engine/tutorials/dockervolumes/)
for more features and advanced data shared topics.

By default, pulling the `supernemo/falaise` image gets you the current production release.
Older versions may also be pulled and are hosted on the [SuperNEMO Docker Hub](https://hub.docker.com/r/supernemo/falaise/).
See the [Tags](https://hub.docker.com/r/supernemo/falaise/tags) page for a full list of versions.

Building Images
===============

[Dockerfiles](https://docs.docker.com/engine/reference/builder/) for each base image
are organised into a directory tree of `OSNAME/OSVERSION`.
The `falaise` directory holds the Dockerfile for the Ubuntu16.04 base image plus
an install of Falaise.

Each image may be built by going into the directory hosting the Dockerfile and
running

```
$ docker build -t <name> .
```

where `<name>` is an ID for the image. To tag and push the images to the
[SuperNEMO Docker Hub organisation](https://hub.docker.com/r/supernemo/), the
IDs and tags for _base_ images should be created and pushed as follows:

```
$ docker login
... entre your Docker Hub credentials
... NB: to push to the supernemo organisation requires push rights to it...
$ docker tag <name> supernemo/falaise-<OSNAME><OSVERSION>-base:latest
$ docker push supernemo/falaise-<OSNAME><OSVERSION>-base:latest
```

At present, only rolling release are supported, so the "latest" tag is used.
For the special `falaise` image, the naming is simpler:

```
...
$ docker tag <name> supernemo/falaise:latest
$ docker push supernemo/falaise:latest
```

As with base images, only rolling releases are supported at the moment,
with a formal tagging policy and method to be defined.

