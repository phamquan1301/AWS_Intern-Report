---
title: "Package with Docker"
weight: 1
chapter: false
pre: " <b> 4.5.1. </b> "
---

### Package application with Docker
Before deploying to AWS Elastic Beanstalk, we need to package the Spring Boot application into a Docker image

### Create Dockerfile
In the root directory of application, create a file named Dockerfile

![Deploy Session](/AWS_Intern-Report/images/dockerfile.png)

![Deploy Session](/AWS_Intern-Report/images/dockerfiledetail.png)

Next, we have to build an jar file, using this command: 
- mvn clean package -DskipTests

You can see it in **target/**:
![Deploy Session](/AWS_Intern-Report/images/buildjarfile.png)


Then, we can test our app at local with commands:
- docker build -t my-app .
- docker run -p 8080:8080 my-app

Access **http://localhost:8080** to see your application

## Build & Push on Amazon ECR
First, we will create a repository to host and manage Docker container images
Get into the AWS Console, search for ECR:

![Deploy Session](/AWS_Intern-Report/images/searchecr.png)
