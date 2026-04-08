---
title: "Monitoring & Operations"
weight: 8
chapter: false
pre: " <b> 4.7. </b> "
---

### Overview
To maintain the stability of **AnTiScaQ** system, implementing automated **Monitoring** and **Alerting** mechanisms is mandatory. System administration should not rely on manual checks; instead, it requires an immediate response process to ensure that scam data analysis workflows remain uninterrupted.

In this module, we will build a centralized operations management system for **AnTiScaQ** based on AWS solutions:

* **Amazon CloudWatch:** A tool for tracking and analyzing detailed operational **Metrics** from the infrastructure (EC2, RDS, ELB), enabling real-time system health assessments.
* **Amazon SNS (Simple Notification Service):** A reliable communication channel connecting the monitoring system with the administrative team. When metrics exceed safe thresholds or a service failure occurs, the system automatically triggers **Email** notifications for timely intervention.

### Implementation Content
* [Monitor with CloudWatch](5.8.1-cloudwatch//)
