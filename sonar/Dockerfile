FROM java:openjdk-8u45-jdk

MAINTAINER David Gageot <david.gageot@sonarsource.com>

ENV SONARQUBE_HOME /opt/sonarqube

#Database configuration
# Defaults to using H2
ENV SONARQUBE_JDBC_USERNAME sonar
ENV SONARQUBE_JDBC_PASSWORD sonar
ENV SONARQUBE_JDBC_URL jdbc:h2:tcp://localhost:9092/sonar
ENV http_proxy http://192.168.10.145:8080
ENV https_proxy  http://192.168.10.145:8080
ENV HTTP_PROXY  http://192.168.10.145:8080
ENV HTTPS_PROXY  http://192.168.10.145:8080
ENV no_proxy localhost,127.0.0.1

ENV SONAR_VERSION 5.2

# pub   2048R/D26468DE 2015-05-25
#       Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
# uid                  sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
# sub   2048R/06855C1D 2015-05-25

RUN set -x \
	&& cd /opt \
	&& curl -o sonarqube.zip -fSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
	&& unzip sonarqube.zip \
	&& mv sonarqube-$SONAR_VERSION sonarqube \
	&& rm sonarqube.zip* \
	&& rm -rf $SONARQUBE_HOME/bin/*

VOLUME ["$SONARQUBE_HOME/data", "$SONARQUBE_HOME/extensions"]
EXPOSE 9000
EXPOSE 9092
WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
ENTRYPOINT ["./bin/run.sh"]
