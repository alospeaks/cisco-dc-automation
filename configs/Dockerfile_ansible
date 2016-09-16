#VERSION 1.0
FROM ubuntu:14.04
MAINTAINER Hemant Kumar, hemakuma@cisco.com
#RUN echo "deb http://archive.ubuntu.com/ubuntu trusty main universe" > /etc/apt/sources.list
RUN apt-get update && apt-get -y upgrade && apt-get install -y  vim git python curl openssh-server  python-pip python-dev build-essential libssl-dev libffi-dev
RUN pip install --upgrade pip
RUN pip install ansible==2.1.1
RUN pip install markupsafe
RUN pip install cryptography
##if this fails, use pip install cryptography --upgrade; failed in jenkins container
## upgrade to the latest VERSION
RUN pip install ansible --upgrade
WORKDIR myscript
