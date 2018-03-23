FROM ubuntu:14.04
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base packages for CadfaelBrew - will be added to
# as we identify base system requirements
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
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

# Set up basic system and user
RUN locale-gen "en_US.UTF-8" \
    && dpkg-reconfigure locales \
    && useradd -m -s /bin/bash linuxbrew

# Linuxbrew layer
RUN git clone https://github.com/SuperNEMO-DBD/brew.git /home/linuxbrew/.linuxbrew \
    && chown -R linuxbrew: /home/linuxbrew/.linuxbrew

# Switch to user
USER linuxbrew
WORKDIR /home/linuxbrew
ENV PATH=/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:$PATH \
    SHELL=/bin/bash \
    USER=linuxbrew

# Install bootstrap layer
RUN brew vendor-install ruby \
    && brew update \
    && brew install patchelf ninja gcc49 cmake \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

# Install Falaise dependency layer
RUN brew install falaise --only-dependencies \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

