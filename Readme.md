# jenkins
This repo enables you to create a jenkins master and slave with a simple command. with the help of some great tools magic can happen on your laptop or in production without much fuss.

## usage

#### master & slave
```
docker-compose up
```

#### scale jenkins slaves

```
docker-compose scale jenkins-slave=3
```

## references
* [docker-compose](https://docs.docker.com/compose/)
* [jenkins-ci](https://hub.docker.com/_/jenkins/)
* [jenkins-swarm](https://hub.docker.com/r/csanchez/jenkins-swarm-slave/)
* [jenkins swarm plugin](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin)
