FROM jenkins

ENV http_proxy http://192.168.10.145:8080
ENV https_proxy  http://192.168.10.145:8080
ENV HTTP_PROXY  http://192.168.10.145:8080
ENV HTTPS_PROXY  http://192.168.10.145:8080
ENV no_proxy localhost,127.0.0.1

COPY plugins.txt /var/jenkins_home/plugins.txt
RUN /usr/local/bin/plugins.sh /var/jenkins_home/plugins.txt

# Adding default Jenkins Jobs
COPY jobs/1-github-seed-job.xml /usr/share/jenkins/ref/jobs/1-github-seed-job/config.xml
COPY jobs/2-job-dsl-seed-job.xml /usr/share/jenkins/ref/jobs/2-job-dsl-seed-job/config.xml
COPY jobs/3-conference-app-seed-job.xml /usr/share/jenkins/ref/jobs/3-conference-app-seed-job/config.xml
COPY jobs/4-selenium2-maven-test.xml /usr/share/jenkins/ref/jobs/4-selenium2-maven-test/config.xml
COPY jobs/6-conference-app-ci.xml /usr/share/jenkins/ref/jobs/conference-app-1-ci/config.xml
COPY jobs/6-conference-app-sonar-analysis.xml /usr/share/jenkins/ref/jobs/conference-app-2-sonar-analysis/config.xml
COPY jobs/7-conference-app-monitoring-ci.xml /usr/share/jenkins/ref/jobs/conference-app-monitoring-1-ci/config.xml
COPY jobs/7-conference-app-monitoring-sonar-analysis.xml /usr/share/jenkins/ref/jobs/conference-app-monitoring-2-sonar-analysis/config.xml

############################################
# Configure Jenkins
############################################
# Jenkins settings
COPY config/config.xml /usr/share/jenkins/ref/config.xml

# Jenkins Settings, i.e. Maven, Groovy, ...
COPY config/hudson.tasks.Maven.xml /usr/share/jenkins/ref/hudson.tasks.Maven.xml
COPY config/hudson.plugins.groovy.Groovy.xml /usr/share/jenkins/ref/hudson.plugins.groovy.Groovy.xml
COPY config/maven-global-settings-files.xml /usr/share/jenkins/ref/maven-global-settings-files.xml

# SSH Keys & Credentials
COPY config/credentials.xml /usr/share/jenkins/ref/credentials.xml
COPY config/ssh-keys/cd-demo /usr/share/jenkins/ref/.ssh/id_rsa
COPY config/ssh-keys/cd-demo.pub /usr/share/jenkins/ref/.ssh/id_rsa.pub

#maven itself

RUN wget --no-verbose -O /tmp/apache-maven-3.3.3.tar.gz http://archive.apache.org/dist/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz
RUN tar xzf /tmp/apache-maven-3.3.3.tar.gz -C /opt
RUN ln -s /opt/apache-maven-3.3.3 /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven_3.3.3
RUN ln -s /opt/apache-maven-3.3.3 /opt/maven
RUN ln -s /opt/maven/bin/mvn /usr/local/bin
RUN rm -f /tmp/apache-maven-3.3.3.tar.gz
ENV MAVEN_HOME /opt/maven
