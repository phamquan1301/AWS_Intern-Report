---
title: "Week 1 Worklog"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Week 1 Objectives

- Meet and get to know the team at **First Cloud Journey (FCJ)**.
- Build a working knowledge of essential AWS services, the **AWS Management Console**, and the **AWS CLI**.

---
### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Introduction to FCJ team. Review internship rules and workplace regulations. | 01/05/2026 | 01/05/2026 | — |
| 2 | Initial setup: access AWS via the Console and configure the CLI. | 01/06/2026 | 01/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Register an AWS Free Tier account. Explore the Console and CLI interfaces. Practice: account creation, CLI installation & configuration, and running basic CLI commands. | 01/07/2026 | 01/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | EC2 fundamentals: instance types, AMIs, EBS volumes. SSH access methods. Elastic IP overview. | 01/08/2026 | 01/08/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Hands-on practice: spin up an EC2 instance, SSH in, and attach an EBS volume. | 01/09/2026 | 01/09/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---
### Week 1 Achievements

##### AWS Architecture Diagrams

Picked up the basics of drawing cloud architecture diagrams using **draw.io** with the official **AWS Architecture Icons** library.


##### AWS Free Tier Account Setup

Set up a working AWS Free Tier account from scratch:

- Clarified the distinction between a **Root Account** and an **IAM User** and why it matters.
- Created an **IAM Group** with admin privileges, then added a new **IAM User** to it.
- Learned when to use **Access Key / Secret Key** (for CLI/API access) versus a **Console Password** (for browser-based login).

##### AWS Management Console

Spent time navigating the web-based Console to get comfortable with the interface:

- Logged in and moved between different service dashboards.
- Created and modified **User Groups**, **Users**, and their **Permission** policies in IAM.
- Recognized why securing IAM user passwords is an important baseline practice.

##### AWS CLI v2 Setup

Installed AWS CLI v2 and connected it to the AWS account:

- Provided **Access Key**, **Secret Key**, and selected a **default Region** during configuration.
- Ran the following command to confirm the CLI was wired up correctly:

```bash
aws sts get-caller-identity
```

##### AWS CLI Basic Operations

Ran a range of commands to get familiar with the CLI workflow:

```bash
# Confirm account identity
aws sts get-caller-identity

# Pull a list of available AWS regions
aws ec2 describe-regions

# Retrieve current EC2 instance details
aws ec2 describe-instances

# Generate a new key pair
aws ec2 create-key-pair --key-name MyKeyPair

# Push a local file up to an S3 bucket
aws s3 cp <local-file> s3://<bucket-name>/
```

By week's end, felt comfortable switching between the **Console** and **CLI** depending on the task at hand.

##### Additional Topics Covered

- Registering a custom domain through **Route 53**.
- High-level overview of **CloudFront CDN**, **AWS WAF**, and other edge security tools.
- Reviewing cloud costs and usage via the **AWS Billing Dashboard**.
- Comparing the different **AWS Support Plans** and what each tier covers.
