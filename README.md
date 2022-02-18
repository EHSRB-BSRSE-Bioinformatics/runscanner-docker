# Docker container for Run Scanner

A containerized version of Run Scanner for MISO-LIMS

# Building the container

To get this running, you will need to clone the repository (`https://github.com/EHSRB-BSRSE-Bioinformatics/runscanner-docker.git`).

Then, build the docker container (first entering the proper directory with `cd runscanner-docker`) using `docker build ./build`. This might take a few minutes since it has to download and compile some software.

# Run the container

Finally, you can bring the container up with `docker-compose up -d`. Follow the [Run Scanner](https://miso-lims.readthedocs.io/projects/runscanner/en/latest/installation/) documentation to configure things for your environment.

It should be accessible at port localhost:9111 by default (or, the IP of your server on port 9111), but you can change this if desired. The port in the container that is exposed is port 8080, the default for Tomcat.
