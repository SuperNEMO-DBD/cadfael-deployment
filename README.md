# cadfael-installer
Installer for CadfaelBrew

# Package Lists
Lists of prerequisite packages are derived from the minimal list
required by homebrew itself, *plus* any required from our own
packages.

## Mac OS X (Mavericks and higher)
Only what homebrew itself requires - OS X plus Xcode. At present, we
don't require Xquartz, preferring Cocoa and Qt (to be reviewed).

As homebrew supports OS X, support on other UNIX platforms is based
on trying to match the base system software supplied by Apple.

## RHEL/CentOS/SL 6/7
We'll try and use the `HEP_OSlibs" metapackage here as this is should always
be installed on Grid nodes. Plus it makes it easy for deployment on other
systems.

Basic list identified as

```
$ yum install -y expat-devel \
git \
HEP_OSlibs_SL6 \
ruby-irb \
redhat-lsb-core

$ yum groupinstall -y 'Development tools'
```

Note that `HEP_OSlibs_SL6` is provided by a CERN repo, so this needs to
be added (TODO Docs).

## Ubuntu 14.04 LTS
Required by homebrew:

```
$ apt-get install -y \
build-essential \
curl \
git \
m4 \
libbz2-dev \
libcurl4-openssl-dev \
libexpat-dev \
libncurses-dev \
ruby2.0 \
texinfo \
zlib1g-dev
```

Additional packages to fully support Cadfael:

- For Root5 visualization

```
$ apt-get install -y
libx11-dev \
libxpm-dev \
libxft-dev \
libxext-dev \
libpng-dev \
libjpeg-dev
```

