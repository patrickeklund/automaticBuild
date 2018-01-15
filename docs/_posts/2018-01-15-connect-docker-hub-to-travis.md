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
COPY target/server-*.jar /tmp/server.jar

#
# Set executable rights server jar
#
RUN chmod +x /tmp/server.jar

# Set default WORKDIR
WORKDIR /tmp

CMD [ "java", "-jar", "/tmp/server.jar" ]
```


## Update travis.yml with docker items
```yml
dist: trusty
sudo: required
services:
  - docker
script:
  - echo
  - echo "Creating Docker tag latest"
  - docker tag patrickeklund/automaticbuild:build-$TRAVIS_BRANCH-$(date +'%Y-%m-%d')-$TRAVIS_BUILD_NUMBER latest
  - echo
  - echo "login to DockerHub..."
  - docker login --username=${DOCKER_HUB_USER} --password=${DOCKER_HUB_USER_PASSWORD}
  - echo
  - echo "Deploy docker image to DockerHub..."
  - docker push patrickeklund/automaticbuild:build-$TRAVIS_BRANCH-$(date +'%Y-%m-%d')-$TRAVIS_BUILD_NUMBER
  - docker push latest
```

## Update travis.yml with docker your docker login credentials
```
travis encrypt --add
```
Answere yes on all items and then add:
```
DOCKER_HUB_USER=<docker hub user>
DOCKER_HUB_USER_PASSWORD=<docker hub user password>
```
Where you’d replace **<docker hub user>** and **<docker hub user password>** with your dockerhub credentials.
Exit with ctrl+d. <br>
This command stores the encrypted github token in the env: section inside the **.travis.yml** file exmple below:
```yml
env:
 global:
 secure: BlB88SGMw2SyXJG0RqQExnAeVbMt9RPpMD8Tw+AmvPQVylqUTIzkzYzSiEXiOCts6L2l/a2IGJxh9qtDZGZi1nJAOYGoZQRITHBVfDWqAPVL6z+gSiQfWDUBJKZ1pMWnKN2a+8Zm/v2Owc0EmQUE1CXg12YcC34/LbECZNP9sLZOkFAy5tD9XsXno4F1E2x3kySyseKzjvY+t6cCIYIOLVvRZt3tMUI9F6Lb0azb3cxg+4hlTEQB/X47F80XLSk6R2QKK5uGLKwwJmFuvooRGvj8XI0/na5UyrPkwEr/RrYzAF8foDuudc/utEA8ffzJFP1JgGhwdBu7CDV8o+vaJWU7B5Kcnz2+e6mHQOJJv1OUeTtToAL3O0hooOjNtw3pigdXTGNlAhOrSRfBKZnUobSohppOI/WtiCEugQiAKoaWZ7JC1aCHtDPrXIT+ubbPBI7/pN0K8IsTWJ508W2ukdIzhDQ5kJ0uGJCEPuFJso/DlVwmDPjzMf7BOM6iT01oksBk4dJMCahIKEnktle/1Gv0PGK4wLbFkOCREb1HZXvw+LUdEDUSv3P46pCfELdhTELfhaV7FegOE17KXQ35jygzZ8ZbR7omR244umyZn0tUqd5tAquO6Zma0K2VVLyCakYbkjuglJoXdXaf2babWfKMfx2zBPZQDXirnHyBO8E=
```


[back]({{ site.baseurl }}/)
