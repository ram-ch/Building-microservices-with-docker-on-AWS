# Building microservices with docker on AWS

**Table of Contents:**
1. Introduction
1. Creating an AWS ubuntu 16.04 server
1. Installing required applications
1. Directory structure
1. Creating directories and files
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
`sudo apt-get update`

STEP2: install docker 
`sudo apt install -y docker.io`

STEP3: start docker
`sudo service docker start`

STEP4: install pip
`sudo apt install python-pip`

STEP5: install docker-compose
`sudo pip install docker-compose`







