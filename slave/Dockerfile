FROM alpine:latest

ENV JENKINS_SWARM_VERSION 2.0
ENV JENKINS_HOME /var/lib/jenkins

RUN apk --update add \
  git \
  tar \
  make \
  vim \
  openjdk7-jre-base \
  bash \
  openssh

### DOCKER
ADD https://get.docker.com/builds/Linux/x86_64/docker-1.8.3 /usr/bin/docker
ADD https://github.com/docker/compose/releases/download/1.4.2/docker-compose-Linux-x86_64 /usr/bin/docker-compose
RUN chmod +rx /usr/bin/docker /usr/bin/docker-compose

### KUBECTL
ADD https://storage.googleapis.com/kubernetes-release/release/v1.0.1/bin/linux/amd64/kubectl /usr/local/bin/kubectl
RUN chmod +rx /usr/local/bin/kubectl

### JENKINS
RUN curl --create-dirs -sSLo /usr/share/jenkins/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar \
       http://maven.jenkins-ci.org/content/repositories/releases/org/jenkins-ci/plugins/swarm-client/$JENKINS_SWARM_VERSION/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar \
       && chmod 755 /usr/share/jenkins
ADD https://raw.githubusercontent.com/carlossg/jenkins-swarm-slave-docker/master/jenkins-slave.sh /usr/local/bin/jenkins-slave.sh
RUN chmod +rx /usr/local/bin/jenkins-slave.sh
RUN adduser -u 233 -h $JENKINS_HOME -G users -D jenkins
RUN chown -R jenkins:users $JENKINS_HOME

VOLUME $JENKINS_HOME
WORKDIR $JENKINS_HOME
USER jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-slave.sh"]
