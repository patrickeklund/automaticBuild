---
layout: default
title: Automatic Build with travis.ci and hub.docker.com
---

## General
Create a docker hub account

## Create Docker hub Repository
Go to https://hub.docker.com/add/repository/ and create a repository **automaticBuild** Visibility **public**. Once created it is accessed at:
```
docker pull patrickeklund/automaticbuild
```

## Create a Dockerfile
Create a simple Dockerfile that exposes needed ports and adds the compiled jar file and starts it on startup, see below:

```
#############################################################################
#
# automaticBuild
#  - creates an image with the automaticBuild server
#
#############################################################################
# Base image to start from
FROM centos:centos7

# Package maintainer
MAINTAINER Patrick Eklund

#
# expose server and admin ports
#
EXPOSE 8080
EXPOSE 9999

#
# Get server Jar file
#
COPY ../../../target/server-*.jar /tmp/server.jar

#
# Set executable rights server jar
#
RUN chmod +x /tmp/server.jar

# Set default WORKDIR
WORKDIR /tmp

CMD [ "/bin/bash", "-c", "server.jar" ]
```


## Update travis.yml with docker items
```yml
dist: trusty
sudo: required
services:
  - docker
script:
  - docker build --pull --file src/resoures/docker/Dockerfile --tag patrickeklund/automaticbuild:"build-$TRAVIS_BRANCH-$(date +'%Y-%m-%d')-$TRAVIS_BUILD_NUMBER"
  - docker push patrickeklund/automaticbuild:"build-$TRAVIS_BRANCH-$(date +'%Y-%m-%d')-$TRAVIS_BUILD_NUMBER"
```

[back]({{ site.baseurl }}/)
