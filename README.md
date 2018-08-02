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


## 1. Introduction: ##
This project aims to build and run a python flask application with gunicorn[Web Server Gateway Interface] and Nginx[front end web server for reverse proxy] as Docker containers on AWS ubuntu 16.04 server. The project is a complete ground up from creation of an AWS instance to deploying containers and managing them on Rancher.

## 2. Creating an AWS ubuntu 16.04 server: ##
* Login in to your AWS console home page
* choose a free tier ubuntu 16.04 server
* Use Putty SSH to connect with your AWS server

## 3. Installing required applications: ##
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

## 4. Creating directories and files ##
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

#make Working directory sp
WORKDIR /sp

#upgrade pip
RUN pip install --upgrade pip

#Install required libraries
RUN pip install -r requirements.txt

#Expose port
EXPOSE 5000
````
Create requirements.txt inside directory sp
`$ sudo vim /home/microservices/sp/requirements.txt`

**requirements.txt** include the required libraries for the python application
**gunicorn** is an interface between the flask app and the **nginx** front end web server

````
# for flask app
Flask==1.0.2
# for api calls
requests==2.18.4
# for gunicorn wsgi[web server gate way interface]
gunicorn==19.9.0
````
Create sp.py inside directory sp     
`$ sudo vim /home/microservices/sp/sp.py`     

**sp.py** is a sample python flask application   

````
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
        return "SP app"		
        
if __name__ == "__main__":
    app.run(host="0.0.0.0")
````

Create directory nginx inside directory microservices
`$ sudo mkdir /microservices/nginx`

**nginx** is a front end web server used for reverse proxy, load balancing and cache. But in this project we use nginx only for 

Create Dockerfile inside nginx directory            
`$ sudo mkdir /microservices/nginx/Dockerfile`     

````
# pull ubuntu image
FROM ubuntu:16.04

# update the current version
RUN apt-get update -y

# Install pip and others build tools
RUN apt-get install -y python-pip python-dev build-essential libssl-dev libffi-dev

# pull nginx image
FROM nginx:1.10.3

# create web.conf
ADD web.conf /etc/nginx/conf.d/web.conf

# check the configurations file format
RUN nginx -t
````
Create web.conf inside nginx directory            
`$ sudo mkdir /microservices/nginx/web.conf`

**web.conf** file consists of the reverse proxy configuration 
This web.conf file makes configurations of nginx to listen on the default port 80 for aws.server.ip.here/sp and redirect to http://aws.server.ip.here:5000/ 

````
server {
	listen    80;
	server_name aws.server.ip.here;
	access_log off; 
 
	location /sp {	
	proxy_pass   http://aws.server.ip.here:5000/;
 	proxy_set_header   Host   $host;
	proxy_set_header   X-Real-IP  $remote_addr;
	proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
	
	client_max_body_size    10m;
	client_body_buffer_size 128k;
 	proxy_send_timeout   90;
	proxy_read_timeout   90;
	proxy_buffer_size    128k;
	proxy_buffers     4 256k;
	proxy_busy_buffers_size 256k;
	proxy_temp_file_write_size 256k;
	proxy_connect_timeout 30s;
	}
}

````
Create docker-compose.yml inside microservices directory            
`$ sudo vim /microservices/docker-compose.yml`

**docker-compose.yml** is the master instruction manual followed by the docker to build all the containers, link them and map ports 
````
version: '2'
services:
  sp:
    restart: always
    build: ./sp
    command: gunicorn -w 4 --bind :5000 sp:app
    expose:
     - "5000"
    ports:
     - "5000:5000"
  nginx:
    restart: always
    build: ./nginx
    links:
     - "sp"
    expose:
     - "80" 
    ports:
     - "80:80" 
 
````

## 5. The `$ sudo docker-compose up -d` :##
`$ sudo docker-compose up -d` initiates the docker-compose.yml file which then step by step executes all the instructions given in it.

-d is the tag which runs the containers in detached mode    

Finally run the command    
`$ sudo docker-compose up -d`    

## 6. Managing with Ranger console
**Rancher** is Open source container management platform for deploying and managing containers








