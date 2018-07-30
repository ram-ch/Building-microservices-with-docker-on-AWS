# pull ubuntu image
FROM ubuntu:16.04

# Pull a layer of python
FROM python:2.7

# update the current version
RUN apt-get update -y

# Install pip and others build tools
RUN apt-get install -y python-pip python-dev build-essential libssl-dev libffi-dev 

# Make directory sp
RUN mkdir /sp

# Copy all files from sp
COPY . /sp

# make Working directory sp
WORKDIR /sp

# upgrade pip
RUN pip install --upgrade pip

# Install required libraries
RUN pip install -r requirements.txt

# Expose port
EXPOSE 5000














