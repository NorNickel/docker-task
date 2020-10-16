# docker-task: Standart and Maximum Solution

## Create *.env files

##### client-service.env
```properties
SPRING_PROFILES_ACTIVE=prod
SPRING_BOOT_ADMIN_CLIENT_URL=http://admin-server:9003

INFO_APPLICATION_NAME=client-service
INFO_APPLICATION_DESCRIPTION=spring boot client-service application
INFO_APPLICATION_VERSION=0.0.1-SNAPSHOT

SPRING_DATA_MONGODB_DATABASE=test_db
SPRING_DATA_MONGODB_PORT=27017
SPRING_DATA_MONGODB_HOST=mongo

SPRING_DATA_MONGODB_AUTHENTICATION-DATABASE=admin
SPRING_DATA_MONGODB_USERNAME=admin
SPRING_DATA_MONGODB_PASSWORD=admin1!
```

##### mongo.env
```properties
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=admin1!
```

##### mongo-express.env
```properties
ME_CONFIG_MONGODB_ADMINUSERNAME=admin
ME_CONFIG_MONGODB_ADMINPASSWORD=admin1!
```

## Change \*.properties files using env variables

##### for example: src/main/resources/application.properties:
```properties
spring.profiles.active=${SPRING_PROFILES_ACTIVE}

management.endpoints.web.exposure.include=*
spring.boot.admin.client.url=${SPRING_BOOT_ADMIN_CLIENT_URL}

info.application.name=${INFO_APPLICATION_NAME}
info.application.description=${INFO_APPLICATION_DESCRIPTION}
info.application.version=${INFO_APPLICATION_VERSION}
```

## Rebuild image nornickel/client-service and push to DockerHub

```bash
$gradlew clean build
$docker build -t nornickel/client-service-image .

$docker login
$docker push nornickel/client-service-image
```

## Run docker-compose.yml

```bash
$docker-compose up
```