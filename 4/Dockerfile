FROM philipssoftware/node:12-java

LABEL maintainer="Jeroen Knoops <jeroen.knoops@philips.com>"

ENV SONAR_SCANNER_VERSION 4.3.0.2102-linux

USER root

RUN apk add --no-cache ca-certificates curl grep sed unzip

RUN pip install --upgrade pip
RUN pip install -U setuptools
RUN pip install -U pylint

WORKDIR /root

RUN curl -o ./sonarscanner.zip -L https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION.zip

RUN unzip sonarscanner.zip
RUN rm sonarscanner.zip

ENV SONAR_RUNNER_HOME=/root/sonar-scanner-$SONAR_SCANNER_VERSION
ENV PATH $PATH:/root/sonar-scanner-$SONAR_SCANNER_VERSION/bin

# The config should be set in the project. See sonar-project.properties as example.
# COPY sonar-project.properties ./sonar-scanner-$SONAR_SCANNER_VERSION/conf/sonar-scanner.properties

#   ensure Sonar uses the provided Java for musl instead of a borked glibc one
RUN sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /root/sonar-scanner-$SONAR_SCANNER_VERSION/bin/sonar-scanner

# Use bash if you want to run the environment from inside the shell, otherwise use the command that actually runs the underlying stuff
#CMD /bin/bash
CMD sonar-scanner -Dsonar.projectBaseDir=./src

