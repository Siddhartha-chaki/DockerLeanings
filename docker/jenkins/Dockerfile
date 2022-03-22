 
FROM jenkins/jenkins:lts-jdk11
# if we want to install via apt
USER root
RUN apt-get update
RUN apt-get -y install python3 python3-nacl python3-pip libffi-dev maven
RUN pip3 install ansible
# drop back to the regular jenkins user - good practice
USER jenkins
