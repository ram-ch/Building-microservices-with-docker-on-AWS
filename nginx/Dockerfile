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