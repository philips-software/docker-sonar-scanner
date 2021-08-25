FROM philipssoftware/node:16-java

LABEL maintainer="Jeroen Knoops <jeroen.knoops@philips.com>"

ENV SONAR_SCANNER_VERSION 4.6.2.2472-linux

USER root

RUN apt-get update && \
    apt-get install -y \
      python3-pip \
      python-pip \
      ca-certificates \
      grep \
      sed \
      shellcheck \
      unzip && \
    rm -rf /var/lib/apt/lists/*

RUN pip install --upgrade pip
RUN pip install -U setuptools
RUN pip install -U pylint
RUN npm install -g typescript

WORKDIR /root

RUN curl -o ./sonarscanner.zip -L https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION.zip

RUN unzip sonarscanner.zip
RUN rm sonarscanner.zip

ENV SONAR_RUNNER_HOME=/root/sonar-scanner-$SONAR_SCANNER_VERSION
ENV PATH $PATH:/root/sonar-scanner-$SONAR_SCANNER_VERSION/bin
ENV NODE_PATH=/usr/local/lib/node_modules

# The config should be set in the project. See sonar-project.properties as example.
# COPY sonar-project.properties ./sonar-scanner-$SONAR_SCANNER_VERSION/conf/sonar-scanner.properties

#   ensure Sonar uses the provided Java for musl instead of a borked glibc one
RUN sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /root/sonar-scanner-$SONAR_SCANNER_VERSION/bin/sonar-scanner

ADD REPO TAGS /root/

RUN echo "\
----------------------------------------------------------------------------------------------\n\
TAGS: $(cat /TAGS)\n\
REPO: $(cat /REPO)\n\
----------------------------------------------------------------------------------------------\n\
SONARSCANNER: $(sonar-scanner --version)\n\
----------------------------------------------------------------------------------------------\n\
PYTHON: $(python --version 2>&1)\n\
PYTHON3: $(python3 --version)\n\
NODE: $(node --version)\n\
JAVA: $(java -version 2>&1 | head -n 1 | awk -F '"' '{print $2}')\n\
----------------------------------------------------------------------------------------------"\
    > /etc/motd

# Use bash if you want to run the environment from inside the shell, otherwise use the command that actually runs the underlying stuff
#CMD /bin/bash
CMD sonar-scanner -Dsonar.projectBaseDir=./src

