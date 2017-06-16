FROM centos:centos7
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base 
ADD RPM-GPG-KEY-wlcg /etc/pki/rpm-gpg/RPM-GPG-KEY-wlcg
ADD WLCG.repo /etc/yum.repos.d/WLCG.repo
RUN yum update -y && \
  yum install -y \
    git \
    HEP_OSlibs \
    openssl-devel \
    ruby-irb \
    redhat-lsb-core \
    # Need static glibc/stdc++ at least for building patchelf
    glibc-static \
    libstdc++-static \
    # which isn't a defualt installed package...
    which && \
  yum groupinstall -y "Development Tools"

# Create user
RUN useradd -m -U cadfael

# Dirty hack to fix "ping: icmp open socket: Operation not permitted"
RUN chmod u+s /usr/bin/ping

# Now our own user and basic "at container entry" setup
USER cadfael
WORKDIR /home/cadfael
#ENV PATH /home/cadfael/cadfael-installer/Cadfael.git/bin:$PATH

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
CMD git clone https://github.com/SuperNEMO-DBD/cadfaelbrew.git && \
  cd cadfaelbrew && \
  ./bin/brew cadfael-bootstrap && \
  ./bin/brew install falaise && \
  ./bin/brew ls --versions && \
  ./bin/brew info
