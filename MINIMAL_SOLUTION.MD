# docker-task: Minimal Solution

## Change url to SpringBootAdminURL

##### client-service/src/main/resources/application.properties:
```groovy
spring.boot.admin.client.url=http://admin-server:9003
```

## Create 'admin-server' image

##### Dockerfile:
```groovy
FROM openjdk:11
ARG JAR_FILE="build/libs/admin-server-0.0.1-SNAPSHOT.jar"
COPY ${JAR_FILE} admin-server.jar
EXPOSE 9003
ENTRYPOINT ["java", "-jar", "/admin-server.jar"]
```
```bash
$docker build -t nornickel/admin-service-image .
```
## Create 'client-service' image

##### Dockerfile:
```groovy
FROM openjdk:11
ARG JAR_FILE="build/libs/client-service-0.0.1-SNAPSHOT.jar"
COPY ${JAR_FILE} client-service.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/client-service.jar"]
```
```bash
$docker build -t nornickel/client-service-image .
```

---
## Start without docker-compose

```bash
$docker network create docker-network
$docker run -d -p 9003:9003 --rm --network=docker_network --name running-admin-server nornickel/admin-server
$docker run -d -p 8080:8080 --rm --network=docker_network --name running-client-service nornickel/client-service
```
---

## Push to DockerHub
```bash
$docker login
$docker push nornickel/admin-server-image
$docker push nornickel/client-service-image
```

## Create docker-compose.yml

##### docker-compose.yml:
```groovy
version: "3.8"
services:

  admin-server:
    image: nornickel/admin-server-image:latest
    ports:
    - "9003:9003"

  client-service:
    image: nornickel/client-service-image:latest
    ports:
    - "8080:8080"
    depends_on:
      - admin-server
```

## Start docker-compose.yml

```bash
$docker-compose up -d
```
