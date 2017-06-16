FROM ubuntu:16.04
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base packages for CadfaelBrew - will be added to
# as we identify base system requirements
RUN apt-get update \ 
    && apt-get install -y --no-install-recommends \
       build-essential \
       curl \
       file \
       git \
       iputils-ping \
       libbz2-dev \
       libcurl4-openssl-dev \
       libexpat1-dev \
       libncurses5-dev \
       libx11-dev \
       libxpm-dev \
       libxft-dev \
       libxext-dev \
       libpng12-dev \
       libjpeg-dev \
       libegl1-mesa-dev \
       libgl1-mesa-dev \
       libglu1-mesa-dev \
       libgles2-mesa-dev \
       locales \
       lsb-release \
       m4 \
       python-setuptools \
       ruby \
       texinfo \
       zlib1g-dev

# Setup basic system/user
RUN localedef -i en_US -f UTF-8 en_US.UTF-8 \
    && useradd -m -s /bin/bash linuxbrew

# Linuxbrew layer
RUN git clone https://github.com/SuperNEMO-DBD/brew.git /home/linuxbrew/.linuxbrew \
    && chown -R linuxbrew: /home/linuxbrew/.linuxbrew

# Switch to user
USER linuxbrew
WORKDIR /home/linuxbrew
ENV PATH=/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:$PATH \
	  SHELL=/bin/bash

# Install bootstrap layer
RUN brew update \
    && brew cadfael-bootstrap \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

# Install Falaise dependency layer
RUN brew install falaise --only-dependencies \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

