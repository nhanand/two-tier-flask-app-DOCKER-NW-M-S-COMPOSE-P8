# Dockerfile

# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# install required packages for system
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Copy the requirements file into the container
COPY requirements.txt .

# Install app dependencies
RUN pip install mysqlclient
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Specify the command to run your application
CMD ["python", "app.py"]

=============================================================================================

# two tier application - networking

create a network

docker network create two-tier-network -d bridge

build docker images for python application and push to docker hub. pull docker image for mysql application.

cmd: docker pull mysql
     docker build -t dockeruser534/two-tier-backend .

# create a network

docker network create two-tier-network -d bridge

# build docker container for both application in same network

cmd:   

docker run -d --name mysql --network two-tier-network -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql

docker run -d -p 5000:5000 --network two-tier-network -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two-tier-backend:latest

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/app.png

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/data.png

# inspect docker network

  docker network inspect two-tier-network                              # you will find both container are running in same network

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/network.png

on browser
localhot:5000                   # access your application

on ec2

ip_address:5000    #
