FROM cern/slc5-base
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base 
ADD HEP_OSlibs.repo /etc/yum.repos.d/HEP_OSlibs.repo
RUN yum update -y && \
  yum clean all && \
  yum install -y \
    bzip2-devel \
    HEP_OSlibs_SL5 \
    expat-devel \
    gcc44 \
    gcc44-c++ \
    gcc44-gfortran \
    glibc-devel \
    git \
    openssl-devel \
    ruby-irb \
    redhat-lsb \
    mesa-libGL-devel \
    mesa-libGLU-devel \
    ncurses-devel \
    libX11-devel \
    libXau-devel \
    libXdamage-devel \
    libXdmcp-devel \
    libXext-devel \
    libXfixes-devel \
    libXft-devel \
    libXpm-devel && \
  yum groupinstall -y "Development Tools"

# Create user
RUN useradd -m cadfael

# Now our own user and basic "at container entry" setup
USER cadfael
WORKDIR /home/cadfael
ENV PATH /home/cadfael/cadfael-installer/Cadfael.git/bin:$PATH

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
  brew install cadfael && \
  brew ls --versions

