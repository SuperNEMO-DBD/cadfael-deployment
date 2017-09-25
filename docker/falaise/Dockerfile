FROM supernemo/falaise-ubuntu1604-base:latest
MAINTAINER Ben Morgan <Ben.Morgan@warwick.ac.uk>

# Install Falaise dependency layer
RUN brew install falaise \
    && brew cleanup -s \
    && rm -Rf $(brew --cache)


# Set ENTRYPOINT for interactive use
ENTRYPOINT ["/bin/bash"]
