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
