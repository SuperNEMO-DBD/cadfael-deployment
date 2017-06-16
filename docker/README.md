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


Build/Tag Containers
====================

Have an organisation on [Docker Hub/Cloud](https://hub.docker.com/r/supernemo/) where
images will be hosted. Need a setup/naming convention for repositories and image
tags. Ideally separate by OS as primary id, with or without version. Also want
to separate out "base" images from higher level ones so that Dockerfiles don't get
cluttered, and can reuse layers as best we can.

Want to check how layers in the `brew` system will work. As the primary goal is
to get containers working for CI, can split into a few layers:

1. Base system, so all system packages/setup
2. Base brew checkout and update
3. Bootstrap packages (may include compiler, so potentially heavyweight)
4. Package set up to Falaise (the testbot part)

This should lead to a similar image size provided we're careful to ensure
proper "install/cleanup" steps at each `RUN` command (i.e. remove everything
temporary created in the step before going on to the next one).


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
