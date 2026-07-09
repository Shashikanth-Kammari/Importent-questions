# Docker and K8

https://docs.google.com/document/d/1jy7mejuXFYnFfHiCI7kqFWZCbpfc2Yot3dqajQdyXeI/edit?usp=sharing

## Docker Interview Questions

- What is a Dockerfile?
A Dockerfile is a text file containing instructions to build a Docker image automatically.
FROM ubuntu:22.04
RUN apt update
RUN apt install nginx -y
COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx","-g","daemon off;"]

- What is multi-stage in Docker?

Multi-stage build allows multiple FROM statements in one Dockerfile.

Purpose:

Reduce image size
Separate build environment from runtime
Improve security

Example:

# Build Stage
```
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package

# Runtime Stage

FROM eclipse-temurin:17-jre
COPY --from=builder /app/target/app.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]```

Benefits:

Small images
Faster deployment
Less vulnerabilities
Production-ready


- What is a Docker volume and how do you use it?

Volumes store data outside the container lifecycle.

Without volume:
Container deleted → Data lost
With volume:
Container deleted → Data remains
Create volume

```docker volume create mysql-data```

Run container

```docker run -d \
-v mysql-data:/var/lib/mysql \
mysql```

Types
-------
Named Volume
Bind Mount
Anonymous Volume



- How can we pass an argument to Dockerfile?
Use ARG.

Dockerfile
```ARG VERSION=17

FROM openjdk:${VERSION}```
Build

```docker build \
--build-arg VERSION=21 \
-t myapp .```

Difference:

ARG

Available during build only

ENV

Available during runtime



- Dockerfile runs as which user?
By default
```root```
To improve security
```RUN useradd appuser
USER appuser```
Production best practice:
Never run containers as root.


- How do you push the image to Docker Hub?

```
docker login
docker tag myapp shashi123/myapp:v1
docker push shashi123/myapp:v1
```

- Write a Dockerfile for a Java-based application.
```
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```
- What is Docker Compose?
Docker Compose is used to manage multiple containers using one YAML file.
```version: "3"

services:

  app:
    image: myapp
    ports:
      - "8080:8080"

  mysql:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD=password
      ```
```
docker compose up

docker compose down

docker compose ps

docker compose logs
```


- How is Docker integrated in Jenkins?
```
Git

↓

Jenkins

↓

Build

↓

Docker Build

↓

Docker Push

↓

Deploy Kubernetes
```

```
stage('Docker Build') {
    steps {
        sh 'docker build -t myapp .'
    }
}

stage('Push') {
    steps {
        sh 'docker push myrepo/myapp'
    }
}```

- Please explain Docker networking.


- Can you use multiple FROM statements in a Dockerfile?
- What is the difference between RUN and CMD?
- Write a Dockerfile for an Nginx application.
- What is the difference between Docker images and Docker containers?
- What are the benefits and use cases of using multi-stage builds in Docker?
- How does Docker handle resource limitations and isolation, such as CPU, memory, and I/O?
- Docker command: COPY vs ADD.
- ENTRYPOINT vs CMD in Dockerfile.
- Tell us something about Docker Compose.

## Kubernetes Interview Questions

- If I want to run a specific container in a pod in Kubernetes, how do I run it?
- ENTRYPOINT vs CMD.
- How are the pods communicating with each other?
- What will you do to make your application more secure?
- What is a namespace in Kubernetes?
- What is a pod security policy?
- How will you implement security for a running container if you find a vulnerability?
- What are the various things that can be done to increase Kubernetes security?
- What problems have you faced?
- How often do you release the product?
- What Kubernetes issues have you encountered in production?
- What is the architecture of Kubernetes?
- How is the pod health check done in Kubernetes?
- How do you use Helm charts?
- What is the Blue/Green Deployment Pattern?
- Can you explain the concept of auto-healing in Kubernetes and give examples of how it works?
- What is a label and selector in Kubernetes?
- What is the CrashLoopBackOff state in a pod?
- What is Kubernetes Ingress?
- What are Kubernetes ConfigMaps and Secrets?
- How do microservices work?
- How do you debug your existing container?
- What is a DaemonSet?
- How do you upgrade the database and cluster in Kubernetes?
- How do you design your pods to be highly available?
- What is the Kubernetes version you are using?
- What is the difference between StatefulSets and Deployments in Kubernetes?
- Have you done the backup of etcd?