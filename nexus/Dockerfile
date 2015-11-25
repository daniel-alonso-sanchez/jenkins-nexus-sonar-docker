#
# Creates a docker container with Nexus Artifact Repository
#

FROM centos:6

MAINTAINER Marcel Birkner <marcel.birkner@codecentric.de>

USER root
# Update the system

ENV http_proxy http://192.168.10.145:8080
ENV https_proxy  http://192.168.10.145:8080
ENV HTTP_PROXY  http://192.168.10.145:8080
ENV HTTPS_PROXY  http://192.168.10.145:8080
ENV no_proxy localhost,127.0.0.1

RUN yum -y update; \
    yum clean all

##########################################################
# Install Java JDK, SSH and other useful cmdline utilities
##########################################################
RUN yum -y install java-1.7.0-openjdk-devel \
    which \
    telnet \
    unzip \
    openssh-server \
    sudo \
    openssh-clients \
    iputils \
    iproute \
    httpd-tools \
    wget \
    tar; \
    yum clean all
ENV JAVA_HOME /usr/lib/jvm/jre

##########################################################
# Install Nexus
##########################################################
RUN mkdir -p /opt/sonatype-nexus /opt/sonatype-work
RUN wget -O /tmp/nexus-latest-bundle.tar.gz http://www.sonatype.org/downloads/nexus-latest-bundle.tar.gz
RUN tar xzvf /tmp/nexus-latest-bundle.tar.gz -C /opt/sonatype-nexus --strip-components=1
RUN useradd --user-group --system --home-dir /opt/sonatype-nexus nexus

ADD nexus.xml /opt/sonatype-work/nexus/conf/nexus.xml

RUN chown -R nexus:nexus /opt/sonatype-work /opt/sonatype-nexus

ENV NEXUS_WEBAPP_CONTEXT_PATH /nexus
RUN echo "#!/bin/bash" > /opt/start-nexus.sh
RUN echo "su -c \"/opt/sonatype-nexus/bin/nexus console\" - nexus" >> /opt/start-nexus.sh
RUN chmod +x /opt/start-nexus.sh

CMD ["/opt/start-nexus.sh"]
EXPOSE 8081
