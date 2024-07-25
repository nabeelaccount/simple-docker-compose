# Site visit

This application stores the number of visits to the web application in a Redis (memory database) server.

We first see how a web application is setup using node.js. We quickly run into a problem when we try to link this container to a redis container to store data, statefulset, previous counts on website access.

Docker-compose is the solution to this problem. With Docker-compose you can do networking between containers.

## How to run the application locally
prerequist:

| Plugin | README |
| ------ | ------ |
| Docker | [Install Docker](https://docs.docker.com/get-docker/) |
| Docker compise | [Install Docker Compose](https://docs.docker.com/compose/install/) |


Run docker compose:
`docker compose up`

Visit localhost:8081 on your web browser. You should see the following message:

**Number of visits 0**

The number of visits should increase as you increase your visits to the site.

## docker-compose file
```
version: '3'
services:
  redis-server:
    image: 'redis'
  node-app:
    # when to restart if container fails
    restart: always
    build: .
    ports:
      - '8081:8081'
```

Two serices are built, 
1) redis-server and 
2) node-app

The 1) redis-server is built on a base image pulled from dockerhub, redis.
The 2) node-app is built based on the local Dockerfile containing the required configurations for building the base node-app base image

The two services, made up of two images, are combined into one file called, docker-compose. Docker-compose ensures you can run a number of different containers with the networking between the services taken care of.


## Github action

1. We fist create a docker enviromment by setting up QEMU and Docker Buildx. 
2. We then login to Docker using a predefined credentials that are stored in as secret github action environment variable. 
3. We build a docker image for testing purposes. The testing image is tagged with "test"
4. We run the test image to ensure it runs as expected
5. We build and push the final tested image to dockerhub. 