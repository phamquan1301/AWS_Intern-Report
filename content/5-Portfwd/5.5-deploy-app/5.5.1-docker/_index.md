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

You can see the jar file in **target/**:
![Deploy Session](/AWS_Intern-Report/images/buildjarfile.png)


Then, we can test the app at local before deploy with commands:
- docker build -t my-app .
- docker run -p 8080:8080 my-app

Access **http://localhost:8080** to view your application













