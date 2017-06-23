FROM centos:centos6
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Update and install base 
ADD RPM-GPG-KEY-cern /etc/pki/rpm-gpg/RPM-GPG-KEY-cern
ADD HEP_OSlibs.repo /etc/yum.repos.d/HEP_OSlibs.repo
RUN yum update -y \
    && yum install -y \
       HEP_OSlibs_SL6 \
       expat-devel \
       git \
       openssl-devel \
       ruby-irb \
       redhat-lsb-core \
     && yum groupinstall -y "Development Tools" \
     && yum clean all

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
RUN brew update \
    && brew cadfael-bootstrap \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

# Install Falaise dependency layer
RUN brew install falaise --only-dependencies \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)

