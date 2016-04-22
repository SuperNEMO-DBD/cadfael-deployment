# cadfael-deployment
Tools for deploying [CadfaelBrew](https://github.com/SuperNEMO-DBD/cadfaelbrew.git)
in containers or to the Grid.

These are focused on system testing and cluster/grid deployment, *not* as end user
interfaces to the SuperNEMO software because they require `root` permissions to
use.

# Containers
At present only Docker Linux containers are built. See the [subproject](containers/README.md)
for further details on building and deploying.

# Deployment to the Grid with CVMFS
Use UK [GridPP](https://www.gridpp.ac.uk) to provide

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
- [CVMFS](https://cernvm.cern.ch/portal/filesystem)
