---
title: "Docker Wiki"
tags:
- software-engineering
- docker
- containers
disableToc: false
---

## Writing a Dockerfile for Spring
Use docker multi stage build
```Dockerfile
FROM maven:3.6.3-openjdk-11-slim as builder

WORKDIR /app
COPY pom.xml .
# Use this optimization to cache the local dependencies. Works as long as the POM doesn't change
RUN mvn dependency:go-offline

COPY src/ /app/src/
RUN mvn package

# Use AdoptOpenJDK for base image.
FROM adoptopenjdk/openjdk11:jre-11.0.8_10-alpine

# Copy the jar to the production image from the builder stage.
COPY --from=builder /app/target/*.jar /app.jar

# Run the web service on container startup.
CMD ["java", "-jar", "/app.jar"]
```

## Useful Commands

- `docker container ls` lists all running containers
- `docker container ls -a` lists all containers
- `docker images` lists all images that are present locally
- `docker image rm IMAGE_ID` to remove an image

You can run an interactive shell container using that image and explore whatever content that image has. For instance:
```
docker run -it image_name sh
```

Or the following for images with an `entrypoint`:
```
docker run -it --entrypoint sh image_name
```

Or, if you want to see how the image was build, meaning the steps in its `Dockerfile`, you can:
```
docker image history --no-trunc image_name > image_history
```

The steps will be logged into the `image_history` file.

- Load Docker Image from tar file
```
docker load -i application-image.tar 
```
- Inspect image
```
docker inspect application
```