FROM centos:centos7
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base 
ADD RPM-GPG-KEY-wlcg /etc/pki/rpm-gpg/RPM-GPG-KEY-wlcg
ADD WLCG.repo /etc/yum.repos.d/WLCG.repo
RUN yum update -y \
    && yum install -y \
       HEP_OSlibs \
       git \
       openssl-devel \
       ruby-irb \
       redhat-lsb-core \
       # Need static glibc/stdc++ at least for building patchelf
       glibc-static \
       libstdc++-static \
       # which isn't a defualt installed package...
       which \
    && yum groupinstall -y "Development Tools" \
    && yum clean all

# Dirty hack to fix "ping: icmp open socket: Operation not permitted"
RUN chmod u+s /usr/bin/ping

# Create user
RUN useradd -m -s /bin/bash linuxbrew

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
    && brew cadfael-bootstrap \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

# Install Falaise dependency layer
RUN brew install falaise --only-dependencies \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

