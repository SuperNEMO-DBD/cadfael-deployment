FROM ubuntu:14.04
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base packages for CadfaelBrew - will be added to
# as we identify base system requirements
RUN apt-get update && apt-get install -y \
  build-essential \
  curl \
  file \
  git \
  python-setuptools \
  ruby2.0 \
  m4 \
  libbz2-dev \
  libcurl4-openssl-dev \
  libexpat-dev \
  libncurses-dev \
  texinfo \
  zlib1g-dev \
  libx11-dev \
  libxpm-dev \
  libxft-dev \
  libxext-dev \
  libpng12-dev \
  libjpeg-dev \
  libegl1-mesa-dev \
  libgl1-mesa-dev \
  libglu1-mesa-dev \
  libgles2-mesa-dev

# Need locales
RUN locale-gen "en_US.UTF-8" && dpkg-reconfigure locales

# Create user
RUN useradd -m -U cadfael

# Now our own user and basic "at container entry" setup
USER cadfael
WORKDIR /home/cadfael

RUN git config --global user.name "Cadfael" && \
    git config --global user.email " "

#Basic tool layer
RUN git clone --depth 1 https://github.com/SuperNEMO-DBD/brew.git && \
    cd brew && \
    ./bin/brew update && \
    ./bin/brew pull --no-publish https://github.com/SuperNEMO-DBD/homebrew-cadfael/pull/11 && \
    ./bin/brew cadfael-bootstrap && \
    ./bin/brew install --only-dependencies bayeux bayeux@2 && \
    ./bin/brew cleanup -s && \
    rm -Rf $(./bin/brew --cache)


# ---------------------------------------------------------------------
# What actually happens when we RUN the container under "docker run"
# Also look at ENTRYPOINT. However, note that we're using the container
# as a one off builder! May want interactivity and/or daemon style
# containers for continuous builds The sections above could be used
# as a base image here (probably)
# Also, can pass args so could checkout tags, build tarballs
CMD git clone https://github.com/SuperNEMO-DBD/brew.git && \
  cd cadfaelbrew && \
  ./bin/brew cadfael-bootstrap && \
  ./bin/brew install falaise && \
  ./bin/brew ls --versions && \
  ./bin/brew info
