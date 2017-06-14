Deployment of SuperNEMO Software on CVMFS
=========================================

The [CVMFS](https://cernvm.cern.ch/portal/filesystem) system provides a remote file
system implementation to allow efficient distribution and use of large software stacks.

It requires deploying a software build(s) to a server, and clients to connect to this
service via a `cvmfs` client (Linux and Mac, but requires `root` permissions to
install and configure). As the standard system used to deploy LHC software to the
LHC Grid, it is a common and supported tool on a wide range of cluster and cloud resources.
It can also potentially be used inside docker containers to add software rather than through
package managers such as yum/apt/homebrew/spack.

SuperNEMO _could_ use UK [GridPP](https://www.gridpp.ac.uk) to provide

- SuperNEMO Grid Virtual Organization
  - TODO: Reactivate existing one
- CVMFS service for Cadfael/Falaise software deployment to Grid sites
  - TODO: Needs VO so that devs can get accounts on GridPP CVMFS deployment server

- More on Grid at the [GridPP site](https://www.gridpp.ac.uk)
  - See the [User Guide](https://www.gridpp.ac.uk/userguide/) for the basic UI setup,
    but note that
    - It requires a VM which is not acceptable for SuperNEMO requirements (it requires `root` permissions)
    - The Ganga/Dirac installs are overly complex
    - TODO: Use new Ganga `pip` install method, test and feedback ([Ganga on GitHub](https://github.com/ganga-devs/ganga))
    - TODO: Push for/develop Dirac `pip` install method ([Dirac on GitHub](https://github.com/DIRACGrid/DIRAC), and see [PR 2775](https://github.com/DIRACGrid/DIRAC/pull/2775) starting to address issue)

Note that any software deployed via CVMFS must neccessarily be relocatable
(i.e. it does not hard code any build or install time paths), or it must
be built assuming `/cvmfs/<site>/<project>/...` as the expected install prefix.

