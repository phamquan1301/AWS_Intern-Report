---
title: "Optimization & Security"
weight: 7
chapter: false
pre: " <b> 4.6. </b> "
---

## Overview

For a system like **AnTiScaQ**, ensuring **high availability** and **robust security** is the top priority to protect users from online threats. In this section, we will optimize the infrastructure to accelerate image data processing and establish resilient defense layers against sophisticated cyberattacks.

### Implementation items:

* **Offloading Static Assets:**
    * Transfer all images and static data from the Web Server to **Amazon S3**.
    * Utilize **Amazon CloudFront (CDN)** for content delivery to reduce latency and alleviate pressure on the central processing system.

* **Security Hardening:**
    * Deploy **AWS WAF (Web Application Firewall)** as a protective barrier in front of CloudFront.
    * Prevent attack attempts exploiting vulnerabilities such as **SQL Injection**, **XSS**, and other unauthorized intrusion methods.
    
### Implementation Content
* [Configure S3 & CloudFront & WAF](5.7.1-cloudfront//)
