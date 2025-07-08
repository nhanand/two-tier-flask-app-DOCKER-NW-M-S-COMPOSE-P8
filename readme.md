# two tier application - networking

create a network

docker network create two-tier-network -d bridge

build docker images for python application and push to docker hub. pull docker image for mysql application.

cmd: docker pull mysql
     docker build -t dockeruser534/two-tier-backend .

# create a network
                                                                                                                          docker network create two-tier-network -d bridge

# build docker container for both application in same network

cmd:   docker run -d --name mysql --network two-tier-network -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
        docker run -d -p 5000:5000 --network two-tier-network -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two-tier-backend:latest

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/app.png

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/data.png

# inspect docker network

  docker network inspect two-tier-network  # you will find both container are running in same network

https://github.com/nhanand/two-tier-flask-app-docker-networking-P7/blob/master/images/network.png

on browser
localhot:5000                   # access your application

on ec2

ip_address:5000    #
