---
title: "Monitor with CloudWatch"
weight: 1
chapter: false
pre: " <b> 4.7.1. </b> "
---

## Monitor with SNS & CloudWatch

## Create SNS Topic:

In AWS Management console, search for sns:

![Monitor Session](/AWS_Intern-Report/images/searchsns.png)

Go to tab **Topic**, click **Create topic**

Choose type **Standard**, type name **Application-alerts**, click **Create topic**

![Monitor Session](/AWS_Intern-Report/images/configsns1.png)

## Create Subcription

Click **Create subscription**, select **Protocol** is Email, type in your email to receive notification from SNS

![Monitor Session](/AWS_Intern-Report/images/createsubscription.png)

After created, please go to your mail to confirm your email.

![Monitor Session](/AWS_Intern-Report/images/confirmsns.png)

## Create CPU Alarm

Go to **CloudWatch** > **Alarms** > **Create alarm**

![Monitor Session](/AWS_Intern-Report/images/createalarm.png)

Select metric > EC2 > Per-Instance Metrics > Select the InstanceID of Beanstalk > **CPUUtilization**

![Monitor Session](/AWS_Intern-Report/images/selectmetric.png)

![Monitor Session](/AWS_Intern-Report/images/selectec2.png)

![Monitor Session](/AWS_Intern-Report/images/selectpermetric.png)

![Monitor Session](/AWS_Intern-Report/images/selectelbid.png)

**Condition**: CPUUtilization: Greater than 70%

![Monitor Session](/AWS_Intern-Report/images/condition.png)

**Notification**: Select Topic **Application-alerts**

![Monitor Session](/AWS_Intern-Report/images/confignotifi.png)

Review configuration of the alarm. Then, click **Create alarm**

![Monitor Session](/AWS_Intern-Report/images/reviewalarm.png)









