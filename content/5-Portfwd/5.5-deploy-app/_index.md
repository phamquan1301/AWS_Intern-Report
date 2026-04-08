---
title: "Deploy Application"
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---

### Overview
During this phase, we will modernize and automate the application deployment process. Specifically, the Spring Boot application will be packaged into a Docker container, securely stored in AWS storage, and finally deployed to a scalable and load-balanced environment.

The deployment architecture will include:
* **Docker:** Packaging the application along with all the environment and dependencies into a single image, ensuring the application runs stably anywhere (from local to production).
* **Amazon ECR (Elastic Container Registry):** A private, secure, and scalable repository on AWS used to store and manage your Docker images.
* **AWS Elastic Beanstalk:** A Platform as a Service (PaaS) that simplifies deployment. It will automatically handle complex tasks such as resource allocation (EC2), application load balancing configuration, and auto-scaling based on the `Dockerrun.aws.json` configuration file.

### Implementation Contents
* [Packaging the application with Docker](5.5.1-docker//)
* [Building & Pushing Docker Image to Amazon ECR](5.5.2-beanstalk//)
* [Initializing Elastic Beanstalk](5.5.3-env-vars//)
