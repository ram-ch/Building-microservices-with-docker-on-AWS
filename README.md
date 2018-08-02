# Building microservices with docker on AWS

**Table of Contents:**
1. Introduction
1. Creating an AWS ubuntu 16.04 server
1. Installing required applications
1. Creating directories and files
   * /home/microservices
   * /home/microservices/sp
   * /home/microservices/sp/dockerfile
   * /home/microservices/sp/requirements.txt
   * /home/microservices/sp/sp.py
   * /home/microservices/nginx
   * /home/microservices/nginx/dockerfile
   * /home/microservices/nginx/web.conf
   * /home/microservices/docker-compose.yml
1. The `sudo docker-compose up -d`
1. Managing with Ranger console


## Introduction: ##
This project aims to build and run a python flask application with gunicorn[Web Server Gateway Interface] and Nginx[front end web server for reverse proxy] as Docker containers on AWS ubuntu 16.04 server. The project is a complete ground up from creation of an AWS instance to deploying containers and managing them on Rancher.

## Creating an AWS ubuntu 16.04 server: ##
* Login in to your AWS console home page
* choose a free tier ubuntu 16.04 server
* Use Putty SSH to connect with your AWS server

## Installing required applications: ##
Note: If you have logged in with username ubuntu then you are not a root user. Hence you need to provide sudo for most of the commands

STEP1: Update the OS
`$ sudo apt-get update`

STEP2: install docker 
`$ sudo apt install -y docker.io`

STEP3: start docker
`$ sudo service docker start`

STEP4: install pip
`$ sudo apt install python-pip`

STEP5: install docker-compose
`$ sudo pip install docker-compose`

## Creating directories and files ##
Note: 
1. configure winSCP to drag and drop files into your AWS server. Else follow these steps
1. Make sure vim is installed

Create directory microservices 
`$ sudo mkdir /home/microservices`

Create directory sp inside directory microservices
`$ sudo mkdir /microservices/sp`

Create dockerfile inside directory sp
`$ sudo vim /home/microservices/sp/Dockerfile`

**Dockerfile** consists of a set of instructions to the docker for building the container. This means the docker is installing an ubuntu 16.04 OS, installing python, creating the required directory structure inside the container and finally exposing the required ports of the container. Later these ports will be mapped to the ports of the server

````
#pull ubuntu image
FROM ubuntu:16.04
#Pull python image
FROM python:2.7
#update the current version
RUN apt-get update -y
#Install pip and others build tools
RUN apt-get install -y python-pip python-dev build-essential libssl-dev libffi-dev 
#Make directory sp
RUN mkdir /sp
#Copy all files from sp
COPY . /sp
make Working directory sp
WORKDIR /sp
upgrade pip
RUN pip install --upgrade pip
Install required libraries
RUN pip install -r requirements.txt
Expose port
EXPOSE 5000
````





