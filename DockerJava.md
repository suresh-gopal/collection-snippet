

# Java Docker Spring Boot Example

Minimal [Spring Boot](http://projects.spring.io/spring-boot/) sample app.

## Requirements

For building and running the application you need:

- [JDK 17](https://www.oracle.com/java/technologies/javase/jdk17-readme-downloads.html)
- [Maven 3](https://maven.apache.org)
- [Docker Desktop or Docker Cli](https://www.docker.com/products/docker-desktop/)

## Running the application locally

There are several ways to run a Spring Boot application on your local machine like mvn clean install and creates target folder with jar. 
Alternatively you can use the [Spring Boot Maven plugin](https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html) like so:

```shell
mvn spring-boot:run
```

## Deploying the application to Docker

Start the Docker Desktop and Open the in-built Terminal from Docker desktop. If Docker Desktop distro installation
failed message appears try unregistering docker-desktop using following command.

```shell
wsl --unregister docker-desktop
docker build -t my-image-1
docker run -p 8080:8080 my-image-1
```

This will create:

* A Docker image my-image-1 in Docker Desktop by copying jar from target folder.
* Docker Image can be run in a container(Optional to give name otherwise random name will be generated) with given port 8080
* Validate if application runs in http://localhost:8080/


