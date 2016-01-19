Ubuntu Docker Containers
========================
Aim to support LTS releases for stability

Current: 14.04 LTS
------------------

Next: 16.04 LTS
---------------
Still in Alpha, Dockerfile gets to working brew system, but packages built after binutils will fail. Known
that 16.04 using very new binutils from the 2.26 development branch, so likely a backward compatibility issue.
Also, system GCC is 5.3 with new CXX ABI. This will need reviewing - for example whether GCC should be built on
this platform.

