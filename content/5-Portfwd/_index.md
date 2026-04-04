---
title : "Workshop"
date: 2026-01-10
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

# Deploy AnTiScaQ (Anti-Scam Intelligence Platform) on AWS 

## Overview

This workshop provides detailed instructions on deploying **AnTiScaQ**, an **anti-phishing intelligence** platform, on the **AWS** cloud using a **Serverless** architecture. The system utilizes Python to **Collect open-source data** and provide **real-time risk scores** for phone numbers, emails, and domains, while strictly adhering to the pillars of the **AWS Well-Architected Framework** to **optimize costs**, ensure **data integrity**, and provide **flexible scalability** in the event of outbreak. At the computing layer, the platform uses **AWS Lambda** and an **API Gateway** to smoothly handle high-speed queries and asynchronous data collection tasks **without the need for server management**. Meanwhile, **Amazon DynamoDB** is used as the primary database with extremely fast response times, combined with **Amazon S3** to store fraud evidence and host the static Next.js web interface. The system's security is robustly reinforced by **Amazon Cognito** for access control and **AWS Secrets Manager** to **automatically protect sensitive data**. Furthermore, the entire source code packaging and application deployment process is **automated** through **GitHub Actions**'s CI/CD system, and the platform also leverages the power of **CloudFront CDN** in conjunction with **Amazon S3** to **maximize** the delivery of interface content to users. All system activities, DDoS protection to abnormal cost alerts, are **closely monitored** by **AWS WAF**, **Amazon CloudWatch**. Ultimately, through this workshop, you will fully master building and operating a secure backend infrastructure, as well as understand the **resource cleanup process** to **avoid unexpected costs**.

### Content

**4.1.** [Introduction](5.1-introduction/)

**4.2.** [Prerequisites](5.2-prerequisites/)

**4.3.** [Network Infrastructure Setup](5.3-network-setup/)

**4.4.** [Deploy Data Layer](5.4-data-layer/)

**4.5.** [Deploy Application](5.5-deploy-app/)

**4.6.** [Automate CI/CD](5.6-cicd/)

**4.7.** [Optimization & Security](5.7-security/)

**4.8.** [Monitoring & Operations](5.8-monitoring/)

**4.9.** [Clean up resources](5.9-cleanup/)