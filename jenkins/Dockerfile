FROM jenkins

ENV http_proxy http://192.168.10.145:8080
ENV https_proxy  http://192.168.10.145:8080
ENV HTTP_PROXY  http://192.168.10.145:8080
ENV HTTPS_PROXY  http://192.168.10.145:8080
ENV no_proxy localhost,127.0.0.1

COPY plugins.txt /var/jenkins_home/plugins.txt
RUN /usr/local/bin/plugins.sh /var/jenkins_home/plugins.txt

# Adding default Jenkins Jobs
#COPY jobs/1-github-seed-job.xml /usr/share/jenkins/ref/jobs/1-github-seed-job/config.xml
#COPY jobs/2-job-dsl-seed-job.xml /usr/share/jenkins/ref/jobs/2-job-dsl-seed-job/config.xml
#COPY jobs/3-conference-app-seed-job.xml /usr/share/jenkins/ref/jobs/3-conference-app-seed-job/config.xml
#COPY jobs/4-selenium2-maven-test.xml /usr/share/jenkins/ref/jobs/4-selenium2-maven-test/config.xml
#COPY jobs/6-conference-app-ci.xml /usr/share/jenkins/ref/jobs/conference-app-1-ci/config.xml
#COPY jobs/6-conference-app-sonar-analysis.xml /usr/share/jenkins/ref/jobs/conference-app-2-sonar-analysis/config.xml
#COPY jobs/7-conference-app-monitoring-ci.xml /usr/share/jenkins/ref/jobs/conference-app-monitoring-1-ci/config.xml
#COPY jobs/7-conference-app-monitoring-sonar-analysis.xml /usr/share/jenkins/ref/jobs/conference-app-monitoring-2-sonar-analysis/config.xml

############################################
# Configure Jenkins
############################################
# Jenkins settings
COPY config/config.xml /usr/share/jenkins/ref/config.xml

# Jenkins Settings, i.e. Maven, Groovy, ...
COPY config/hudson.tasks.Maven.xml /usr/share/jenkins/ref/hudson.tasks.Maven.xml
COPY config/hudson.plugins.groovy.Groovy.xml /usr/share/jenkins/ref/hudson.plugins.groovy.Groovy.xml
COPY config/maven-global-settings-files.xml /usr/share/jenkins/ref/maven-global-settings-files.xml
COPY config/hudson.plugins.sonar.SonarPublisher.xml  /usr/share/jenkins/ref/hudson.plugins.sonar.SonarPublisher.xml

COPY config/hudson.plugins.sonar.SonarRunnerInstallation.xml  /usr/share/jenkins/ref/hudson.plugins.sonar.SonarRunnerInstallation.xml

# SSH Keys & Credentials
COPY config/credentials.xml /usr/share/jenkins/ref/credentials.xml
COPY config/ssh-keys/cd-demo /usr/share/jenkins/ref/.ssh/id_rsa
COPY config/ssh-keys/cd-demo.pub /usr/share/jenkins/ref/.ssh/id_rsa.pub

#maven itself
USER root
RUN wget --no-verbose -O /tmp/apache-maven-3.3.3-bin.zip http://archive.apache.org/dist/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.zip
RUN unzip /tmp/apache-maven-3.3.3-bin.zip -d /usr/share 
RUN rm -f /tmp/apache-maven-3.3.3-bin.zip
RUN ln -s  /usr/share/apache-maven-3.3.3/bin/mvn  /usr/local/bin
RUN chmod -R 755  /usr/share/apache-maven-3.3.3 
ENV MAVEN_HOME /usr/share/apache-maven-3.3.3
COPY config/settings.xml /usr/share/apache-maven-3.3.3/conf

#SonarRunner

RUN wget --no-verbose -O /tmp/sonar-runner-dist-2.4.zip http://repo1.maven.org/maven2/org/codehaus/sonar/runner/sonar-runner-dist/2.4/sonar-runner-dist-2.4.zip
RUN unzip  /tmp/sonar-runner-dist-2.4.zip -d /usr/share
RUN rm -f /tmp/sonar-runner-dist-2.4.zip
RUN chmod -R 755  /usr/share/sonar-runner-2.4
COPY config/sonar-runner.properties /usr/share/sonar-runner-2.4/conf

#grails

RUN wget --no-verbose -O /tmp/grails-2.4.4.zip https://github.com/grails/grails-core/releases/download/v2.4.4/grails-2.4.4.zip
RUN unzip  /tmp/grails-2.4.4.zip -d /usr/share
RUN rm -f /tmp/grails-2.4.4.zip
RUN chmod -R 755  /usr/share/grails-2.4.4
COPY config/com.g2one.hudson.grails.GrailsInstallation.xml /usr/share/jenkins/ref/com.g2one.hudson.grails.GrailsInstallation.xml 


