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
# Set Build And Environment variables
# ARG and Env usefull when setting overridable build parameters
#

#
# Set Environment variables
#  Env only usefull for Static build parameters
#


#
# Get server Jar file
#
COPY ../../../target/server-*.jar /tmp/server.jar


#
# Set executable rights server jar
#
RUN chmod +x /tmp/server.jar
    sync

# Set default WORKDIR
WORKDIR /tmp

CMD [ "/bin/bash", "-c", "/tmp/server.jar" ]
