FROM ubuntu:16.04
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base packages for CadfaelBrew - will be added to
# as we identify base system requirements
RUN apt-get update && apt-get install -y \
  # Not in base system or newly needed
  lsb-release \
  iputils-ping \
  # Known from Ubuntu 14
  build-essential \
  curl \
  git \
  m4 \
  libbz2-dev \
  libcurl4-openssl-dev \
  libexpat-dev \
  libncurses-dev \
  ruby \
  texinfo \
  zlib1g-dev \
  libx11-dev \
  libxpm-dev \
  libxft-dev \
  libxext-dev \
  libpng12-dev \
  libjpeg-dev

# Create user
RUN useradd -m -U cadfael

# Now our own user and basic "at container entry" setup
USER cadfael
WORKDIR /home/cadfael
#ENV PATH /home/cadfael/cadfael-installer/Cadfael.git/bin:$PATH

# ---------------------------------------------------------------------
# What actually happens when we RUN the container under "docker run"
# Also look at ENTRYPOINT. However, note that we're using the container
# as a one off builder! May want interactivity and/or daemon style
# containers for continuous builds The sections above could be used
# as a base image here (probably)
# Also, can pass args so could checkout tags, build tarballs
CMD git clone https://github.com/SuperNEMO-DBD/cadfael-installer.git && \
  cd cadfael-installer && \
  ./cadfael-installer && \
  cd Cadfael.git/bin && \
  ./brew test falaise && \
  ./brew ls --versions && \
  ./brew info
