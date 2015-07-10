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

# Systems Requirements for CadfaelBrew
## Supported
As with upstream linuxbrew, only 64bit systems are supported.

- [Mac OS X](https://www.apple.com/osx/) 10.8 (Mountain Lion), 10.9 (Mavericks), 10.10 (Yosemite)
- Linux Distributions:
  - [RHEL](http://www.redhat.com/en/technologies/linux-platforms/enterprise-linux)/[CentOS](https://www.centos.org) 6.X, 7.X
  - [Scientific Linux](https://www.scientificlinux.org) 6.X
  - [Ubuntu](http://www.ubuntu.com) 14.04LTS

All require of order 2GB disk space for the final install. A similar
amount is needed for downloads of source/binary packages plus
temporary space when building packages from source.

If your system is not listed above, this does not neccessarily mean that CadfaelBrew
will not work on it. Please review the sections on [deprecated](#deprecated)
and [work in progress](#work-in-progress) to see if your system is listed. If it
is not listed and you'd like support for it, please see the section on [manually installing CadfaelBrew](#installing-cadfaelbrew-by-hand) for further instructions.

## Deprecated
Supported on a 'best effort' basis, largely to allow use on older cluster
systems.

- RHEL/CentOS/Scientific Linux 5.X

## Work in Progress
These are known upcoming systems that will be supported but are currently
not supported by upstream Homebrew/Linuxbrew or are not at the release
stage.

- Mac OS X 10.11 (El Capitan)
  - Currently in Beta, release expected Autumn 2015
- Scientific Linux 7
  - Expected to be no different from RHEL/CentOS7 but requires testing
- Ubuntu 16.04LTS
  - Next LTS (Long Term Support) version. Expected Spring 2016

# System Package Requirements
Though CadfaelBrew provides a wide range of software packages, it
is not completely standalone and needs a base set of software
to be installed through the system package manager (yum/apt/App Store etc)
Lists of prerequisite packages are given below and are derived from the minimal
list required by homebrew itself, *plus* any required from our own
packages.

## Homebrew Requirements
Homebrew is written in the [Ruby language](https://www.ruby-lang.org/en/) and
uses the [git](https://git-scm.com) version control system for distributing
updates. The minimum requirements to run the `brew` command are thus:

- Ruby 1.9 or higher (Run `ruby --version` to check)
- git 1.7 or higher (Run `git --version` to check)

Homebrew are gradually removing support for Ruby versions less than 2.0,
though these patches have not been merged into CadfaelBrew yet.
The installer script will bootstrap a temporary local Ruby install if the
system package manager cannot supply
a sufficient version. It will then use `brew` itself to install the
Homebrew Ruby for permanent use. So far no issues have been encountered
with Git versions, though this can also be bootstrapped in future if required. Package lists below include any packages needed to bootstrap
Ruby/Git.

## Mac OS X
Support for OS X follows Homebrew itself, and this generally means the last three production versions.
At the time of writing this means OS X 10.8 (Mountain Lion), 10.9 (Mavericks) and 10.10 (Yosemite).
System requirements are only what homebrew itself requires - OS X plus Xcode. The latter
may be installed through the [App Store](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12),
though `cadfael-installer` will offer to install it for you if it is not present
when you run the installer script.

At present, we don't require Xquartz(X11), for graphical displays, preferring
Cocoa and Qt as these are supported by ROOT and Geant4 respectively.

## Linux Distributions
As homebrew supports OS X, support on other UNIX platforms is based
on trying to match the base system software supplied by Apple.

## RHEL/CentOS/SL 6/7
On this family, the [`HEP_OSlibs`](https://twiki.cern.ch/twiki/bin/view/LCG/SL6DependencyRPM)
metapackage is used to provide most of the functionality on top of the base system as this is should always be installed on any system running LHC software. To get and install this rpm, follow the instructions as
provided for the [6 Series](https://twiki.cern.ch/twiki/bin/view/LCG/SL6DependencyRPM) or [7 Series](https://twiki.cern.ch/twiki/bin/view/LCG/CentOS7DependencyRPM) as appropriate. A few extra packages are needed
to support Cadfael's bootstrapping proceedure, so the basic list of
rpms for the 6 series is:

```
$ yum install -y \
expat-devel \
git \
HEP_OSlibs_SL6 \
openssl-devel \
ruby-irb \
redhat-lsb-core

$ yum groupinstall -y 'Development tools'
```

and for 7 series:

```
$ yum install -y \
git \
HEP_OSlibs \
openssl-devel \
ruby-irb \
redhat-lsb-core

$ yum groupinstall -y 'Development tools'
```

Note that these are not the *complete* list
of packages required, simply the additional set needed to be installed on top
of a base install. Yum will pull in all needed dependencies.

## Ubuntu 14.04 LTS
The list is longer here as there is no metapackage like `HEP_OSlibs`
for Ubuntu.

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
zlib1g-dev \
libx11-dev \
libxpm-dev \
libxft-dev \
libxext-dev \
libpng12-dev \
libjpeg-dev
```

## RHEL/CentOS/SL 5
The 5 series of RHEL is considered deprecated due to LHC production sites
moving to the 6 and 7 series. However, 5 series support is maintained on
"best effort" basis to provide a transitional on older cluster systems.
As with the 6/7 series, the [`HEP_OSlibs` metapackage](https://twiki.cern.ch/twiki/bin/view/LCG/SL5DependencyRPM) is used to provide a basic
package set, and this should be installed following the [instructions provided on the LCG wiki)(https://twiki.cern.ch/twiki/bin/view/LCG/SL5DependencyRPM). This RPM is not as fully featured as those for the 6/7 series, so a larger number of additional packages are required:

```
$ yum install -y \
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
libXpm-devel

$ yum groupinstall -y "Development Tools"
```


# Installing CadfaelBrew using `cadfael-installer`
Ensure your system is supported and that the requisite software is installed. You will also require a working network connection in order to clone the CadfaelBrew repository and subsequently install any packages.

Begin by cloning this repository to a location of your choice:

```
$ git clone https://github.com/SuperNEMO-DBD/cadfael-installer.git cadfael-installer.git
```

Change into the root directory of the cloned repository and run the `cadfael-installer` script from the command line, optionally passing an installation prefix. For example, to install CadfaelBrew under the current working directory, simply do:

```
$ cd cadfael-installer.git
$ ./cadfael-installer
```

or to install to, say, a directory `$HOME/supernemo`, do

```
$ ./cadfael-installer -p $HOME/supernemo
```

In both cases, a subdirectory `Cadfael.git` will be created under the prefix to hold the installation. For all options available, run

```
$ ./cadfael-installer -h
```

The script will check that your system is supported and provides the needed system packages used by Cadfael.
Any missing packages will be reported together with instructions on installing them (this may need to be done by your sysadmin if you don't have sufficient privileges on the system).For certain older systems that do not provide a sufficiently recent version of the Ruby language that CadfaelBrew is written in, this will be bootstrapped.

Once all requirements are met, the script clones CadfaelBrew's GitHub repository and performs a basic sanity check.
It then runs the `brew` command to install the `cadfael` Formula that installs the required software packages. A
complete list of packages can be found in [the documentation for the `homebrew-cadfael` tap](https://github.com/SuperNEMO-DBD/homebrew-cadfael)
Once complete, instructions should be printed on using the installed software.


# Installing CadfaelBrew by Hand
Should your system not be supported by the installer, or you cannot run the installer for other reasons, CadfaelBrew
can be installed by hand. The installer script simply wraps these steps in a convenient manner together with checks for required system packages.

To get CadfaelBrew itself installed, first ensure you have the needed
versions of Ruby and Git as [listed above](#system-requirements-for-cadfaelbrew).
Should the system Ruby and Git versions not be sufficient, temporary
local installs should be performed following the instructions on the
[Ruby](http://www.ruby-lang.org) and [Git](https://git-scm.com) homepages.
Ensure that Ruby is built with OpenSSL support. If you have installed
temporary Ruby and Git versions, ensure that the directories holding their
`ruby` and `git` executables are prepended to the UNIX PATH environment
variable.

With the requisite Ruby and Git in place, clone the core CadfaelBrew
repository to a location of your choice:

```
$ git clone https://github.com/SuperNEMO-DBD/cadfaelbrew.git Cadfael.git
```

Once checked out, prepend the `bin` subdirectory of the repository (in the
example above this would be `Cadfael.git/bin`) to the PATH. Though not
100% required, it makes running the following command easier, and gives
immediate access to the installed software later.

Before installing anything, run brew's self-check `doctor` command to
see if there are any internal/setup issues:

```
$ brew doctor
...
```

As the checks run are very stringent, it's likely that several warnings
will be issued. Known ones that can be ignored include those about
brew being installed in a non-default location, and duplicate programs
in the path. If in doubt, raise an issue on the [CadfaelBrew issue tracker](https://github.com/SuperNEMO-DBD/cadfaelbrew).

With CadfaelBrew ready to brew, the first thing to do if you needed
to is install Ruby and Git by hand is to install the brewed versions:

```
$ brew install ruby git
```

You can then remove the temporary local installs you performed.

To install Cadfael itself, begin by tapping its repository of Homebrew
formulae:

```
$ brew tap SuperNEMO-DBD/cadfael
```

Now install cadfael:

```
$ brew install cadfael
```

If installation completes successfully, you can review what was installed
by running

```
$ brew ls --versions
```

Should any errors occur, you can run again with

```
$ brew install -v cadfael
```

which will print out more information. If you're happy for more manual intervention, do

```
$ brew install -vd cadfael
```

This will offer to drop you into the build of the failing package (choose option '5') from which you can run its configure/build system to try
and diagnose the issue in more depth.
If you are running on an unsupported system, the most likely cause
of errors is missing system packages. Some investigation will be required
through the system package manager to track these down. Though package
names will not be the same across different systems, the [Ubuntu](ubuntu-14.04-lts) list can provide a good starting list for matching up to
your system.

## Adding Support for New Systems
If your system is not supported by the installer script, please submit a feature request on the issue tracker.
To help in triaging whether support can be added, you should still clone this repo and then run

```
$ ./cadfael-installer -s
```

This will print out some information on the characteristics of your system.
Paste this output into your feature request. Support cannot be guaranteed for all systems.

If you prefer to add support by yourself, the steps needed to add support
in the `cadfael-installer` script are documented within that script.
Should you add support for a new system, please submit a pull request!


# Containers
This directory holds files for building Linux containers for testing
installs of Cadfael on supported platforms. This is a work in progress
and at present only Docker containers are used.

