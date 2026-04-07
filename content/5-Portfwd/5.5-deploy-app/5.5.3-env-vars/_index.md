---
title: "Initialize Elastic BeanStalk"
weight: 3
chapter: false
pre: " <b> 4.5.3. </b> "
---

## Configure Elastic BeanStalk

First, create a file named **Dockerrun.aws.json** (a configuration file used by AWS Elastic Beanstalk to run your Docker container automatically), the file will be sibling with Dockerfile

![Deploy Session](/AWS_Intern-Report/images/dockerrun.png)

![Deploy Session](/AWS_Intern-Report/images/dockerrundetail.png)

Replace the image name with your **Image URI** that you just pushed, then zip the file  **.zip**

In AWS Management Console, search for **elastic beanstalk**

![Deploy Session](/AWS_Intern-Report/images/searchelb.png)

We can see AWS Elastic Beanstalk console, click create application

![Deploy Session](/AWS_Intern-Report/images/createapplication.png)

For configuration, choose as the images demonstrate

![Deploy Session](/AWS_Intern-Report/images/configelb1.png)

![Deploy Session](/AWS_Intern-Report/images/configelb2.png)

![Deploy Session](/AWS_Intern-Report/images/configelb3.png)

Choose **Upload your code**, -> local file, upload file **Dockerrun.aws.json.zip** that we have zipped before

![Deploy Session](/AWS_Intern-Report/images/configelb4.png)

![Deploy Session](/AWS_Intern-Report/images/configvpcelb.png)

Choose vpc that we has created before, for instance subnets, choose 2 public subnets form vpc

![Deploy Session](/AWS_Intern-Report/images/ec2sgelb.png)

Also, choose ec2 security group created 

![Deploy Session](/AWS_Intern-Report/images/ec2sgelb.png)

For environment type choose **Load balanced**, for elastic beanstalk using Application Load Balancer

![Deploy Session](/AWS_Intern-Report/images/loadbalancerelb.png)

Chooose 2 public subnets for load balancer subnets

![Deploy Session](/AWS_Intern-Report/images/loadbalancer1.png)

For instance types, choose only **t3.micro**

![Deploy Session](/AWS_Intern-Report/images/instanceec2elb.png)

Click next, review and check your application configuration. If OK, click **Create**. The system will take approximately 5-7 minutes to initialize.

