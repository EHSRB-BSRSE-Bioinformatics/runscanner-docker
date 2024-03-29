#######################################################
## Build Run Scanner from GitHub Repository
#######################################################

FROM tomcat:9-jdk11-openjdk-slim

# Install a few dependencies
RUN apt-get update && apt-get -y install build-essential wget curl git libjsoncpp-dev jq zip procps pkg-config cmake autoconf libtool clang

# Downloading and installing Maven
# 1- Define a constant with the version of maven you want to install
ARG MAVEN_VERSION=3.8.4         

# 2- Define a constant with the working directory
ARG USER_HOME_DIR="/root"

# 3- Define the SHA key to validate the maven download
# ARG SHA=

# 4- Define the URL where maven can be downloaded from
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

# 5- Create the directories, download maven, validate the download, install it, remove downloaded file and set links
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && echo "Downloading maven" \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  \
  && echo "Checking download hash" \
#  && echo "${SHA}  /tmp/apache-maven.tar.gz" | sha512sum -c - \
  \
  && echo "Unziping maven" \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  \
  && echo "Cleaning and setting links" \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# 6- Define environmental variables required by Maven, like Maven_Home directory and where the maven repo is located
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV CONTEXT "ROOT"

# This is how it needs to be compiled. Better approach than zip releases, because Maven requires the git repository it seems.
RUN git clone https://github.com/miso-lims/runscanner.git

WORKDIR ./runscanner
RUN mvn clean package
RUN cp ${CATALINA_HOME}/runscanner/scanner/target/scanner*war ${CATALINA_HOME}/webapps/${CONTEXT}.war

RUN git submodule init && git submodule update && \
  cd ./runscanner-illumina && \
  ./build-illumina-interop && autoreconf -i && ./configure && make && make install

CMD ["catalina.sh", "run"]
