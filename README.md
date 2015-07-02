# cadfael-installer
Bootstrap installer for [CadfaelBrew](https://github.com/SuperNEMO-DBD/cadfaelbrew.git),
the SuperNEMO experiment's fork of the [Linuxbrew](http://brew.sh/linuxbrew)/[Homebrew](https://brew.sh) 
software package manager. This bootstrap script is provided
to help users get a base install of CadfaelBrew up and running on
Linux and OS X platforms. It handles checks for required system
packages, bootstrapping of any new versions of core packages like
git and Ruby, and the basic cloning and updating of brew itself.

Below we provide OS/software requirements for using CadfaelBrew, followed
by instructions on running the installer to get the base system up and running.
Also provided are the 'from scratch' steps underlying the installer script.
These are most useful if you're running on an unsupported system and
want to try testing CadfaelBrew to this.

# Systems Supported by CadfaelBrew
## Supported
As with upstream linuxbrew, only 64bit systems are supported.

- Mac OS X 10.9 (Mavericks), 10.10 (Yosemite)
- Linux Distributions:
  - RHEL/CentOS 6.X, 7.X
  - Scientific Linux 6.X
  - Ubuntu 14.04

## Deprecated
Supported on a 'best effort' basis, largely to allow us on older cluster
systems.

- RHEL/CentOS/Scientific Linux 5.X

## Work in Progress
These are known upcoming systems that will be supported but are currently
not supported by upstream Homebrew/Linuxbrew or are not at the release
stage.

- Mac OS X 10.11 (El Capitan)
  - Currently in Beta 
- Scientific Linux 7
  - Expected to be no different from RHEL/CentOS7 but requires testing

# System Package Requirements
Though CadfaelBrew provides a wide range of software packages, it
is not completely standalone and needs a base set of software
to be installed through the system package manager (yum/apt/App Store etc)
Lists of prerequisite packages are given below and are derived from the minimal 
list required by homebrew itself, *plus* any required from our own
packages.

## Mac OS X (Mavericks and higher)
Only what homebrew itself requires - OS X plus Xcode. The latter 
may be installed through the [App Store](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12),
though `cadfael-installer` will offer to install it for you if it is not present
when you run the installer script.

At present, we don't require Xquartz(X11), for graphical displays, preferring 
Cocoa and Qt as these are supported by ROOT and Geant4 respectively.

## Linux Distributions
As homebrew supports OS X, support on other UNIX platforms is based
on trying to match the base system software supplied by Apple.

## RHEL/CentOS/SL 6/7
We base the use the [`HEP_OSlibs`](https://twiki.cern.ch/twiki/bin/view/LCG/SL6DependencyRPM) 
metapackage here as this is should always be installed on Grid nodes. 
Plus it makes it easy for deployment on other systems. However, it is not
totally complete, so the basic list of rpms on the 6 series is:

```
$ yum install -y \
expat-devel \
git \
HEP_OSlibs_SL6 \
ruby-irb \
redhat-lsb-core

$ yum groupinstall -y 'Development tools'
```

and for 7 series:

```
$ TOBECHECKED
```

Note that `HEP_OSlibs*` rpms are provided by repos at CERN, so this needs to
be added in your `yum` configuration (TODO Docs).

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

Additional packages to fully support CadfaelBrew:

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

# Installing CadfaelBrew using `cadfael-installer`
You will require a working network connection in order to clone the CadfaelBrew repository and subsequently install any packages. Simply run the `cadfael-installer` script from the command line, optionally passing an installation prefix. For example, to install CadfaelBrew under the current working directory, simply do:

```
$ ./cadfael-installer
```

or to install to, say, a directory `$HOME/supernemo`, do

```
$ ./cadfael-installer -p $HOME/supernemo
```

In both cases, a subdirectory `Cadfael.git` will be created to hold the installation.

The script will check that your system is supported and provides the needed system packages used by Cadfael. 
Any missing packages will be reported together with instructions on installing them (this may need to be done by your sysadmin if you don't have sufficient priveleges on the system). For certain older systems that do not provide a sufficiently recent version of the Ruby language that CadfaelBrew is written in, this will be bootstrapped.
Once all requirements are met, the script clones CadfaelBrew's GitHub repository and performs a basic sanity check.

# Installing CadfaelBrew by Hand


# Containers
This directory holds files for building Linux containers for testing
installs of Cadfael on supported platforms. This is a work in progress
and at present only Docker containers are used.

