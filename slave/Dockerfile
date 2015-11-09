FROM alpine:latest

ENV JENKINS_SWARM_VERSION 2.0
ENV JENKINS_HOME /var/lib/jenkins

RUN apk --update add \
  git \
  tar \
  make \
  vim \
  py-pip \
  openjdk7-jre-base \
  bash \
  openssh

### JOB BUILDER
RUN pip install --proxy $http_proxy jenkins-job-builder

### FLEETCTL
ADD https://github.com/coreos/fleet/releases/download/v0.11.5/fleet-v0.11.5-linux-amd64.tar.gz /tmp/fleet-v0.11.5-linux-amd64.tar.gz
RUN tar -zxvf /tmp/fleet-v0.11.5-linux-amd64.tar.gz -C /tmp/ \
    && mv /tmp/fleet-v0.11.5-linux-amd64/fleetctl /usr/local/bin/fleetctl

### DOCKER
ADD https://get.docker.com/builds/Linux/x86_64/docker-1.8.3 /usr/bin/docker
ADD https://github.com/docker/compose/releases/download/1.4.2/docker-compose-Linux-x86_64 /usr/bin/docker-compose
RUN chmod +rx /usr/bin/docker /usr/bin/docker-compose

### ANSIBLE
RUN apk --update add py-pip gcc alpine-sdk python-dev
RUN pip install --upgrade pip \
    && pip install ansible

### JENKINS
RUN curl --create-dirs -sSLo /usr/share/jenkins/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar \
       http://maven.jenkins-ci.org/content/repositories/releases/org/jenkins-ci/plugins/swarm-client/$JENKINS_SWARM_VERSION/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar \
       && chmod 755 /usr/share/jenkins
ADD https://raw.githubusercontent.com/carlossg/jenkins-swarm-slave-docker/master/jenkins-slave.sh
RUN chmod +rx /usr/local/bin/jenkins-slave.sh
RUN adduser -u 233 -h $JENKINS_HOME -G users -D jenkins
RUN chown -R jenkins:users $JENKINS_HOME

VOLUME $JENKINS_HOME
WORKDIR $JENKINS_HOME
USER jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-slave.sh"]
