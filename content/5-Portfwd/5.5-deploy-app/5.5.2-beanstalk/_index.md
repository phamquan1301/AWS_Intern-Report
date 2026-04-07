---
title: "Build & Push Docker Image on Amazon ECR"
weight: 2
chapter: false
pre: " <b> 4.5.2. </b> "
---

## Build & Push on Amazon ECR
First, we will create a repository to host and manage Docker container images
Get into the AWS Console, search for ECR:

![Deploy Session](/AWS_Intern-Report/images/searchecr.png)

You can see the console of ECR, click create

![Deploy Session](/AWS_Intern-Report/images/createecr.png)

Fill out your repository name, at **Image tag mutability** choose **Mutable**

![Deploy Session](/AWS_Intern-Report/images/configecr1.png)

For Encrytion configuration, choose **AES-256**, then click create repository

![Deploy Session](/AWS_Intern-Report/images/configecr2.png)

After created, we can see new private repository on console, with an **URI**

# Login Docker into AWS
In order to login docker into aws, we have to an iam user with attach policy **AmazonEC2ContainerRegistryFullAccess**

![Deploy Session](/AWS_Intern-Report/images/searchiam.png)

![Deploy Session](/AWS_Intern-Report/images/createiamuser.png)

![Deploy Session](/AWS_Intern-Report/images/attachpolicy.png)

Then, access to **Security Credentials**, create access key, secret key for AWS CLI

Back to ECR repositoy just created, click repository, we can see tab images, click view push commands

![Deploy Session](/AWS_Intern-Report/images/imagepushcommand.png)

After accessing the AWS CLI, execute each command in the “View push commands” section in sequence to build the Docker image and then push it to the ECR repository.

Then, access to ECR console, in the image tab, we can see new image has been available


