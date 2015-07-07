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
    redhat-lsb-core && \
  yum groupinstall -y "Development Tools"

# Create user
RUN useradd -m -U cadfael

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

