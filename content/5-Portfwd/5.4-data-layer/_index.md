---
title: "Deploy Data Layer"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

### Overview
In this phase, we will establish the storage and knowledge base for the HRM application using Generative AI. This process includes configuring a secure object storage (S3), importing structured data into a high-speed NoSQL database (DynamoDB), and integrating the power of AWS Bedrock to handle RAG (Retrieval-Augmented Generation) data.

The storage and AI architecture to be established includes:
* **Amazon S3 (Simple Storage Service):** Initialize 3 separate buckets with strict security policies (blocking public access, automatic encryption, versioning enabled) to store AI data, reports, and images.
* **Amazon DynamoDB:** Create structured data tables automatically by directly importing existing data sources from S3 buckets, saving time on manual schema configuration.
* **AWS Bedrock Knowledge Base:** Build the AI ​​brain by directly connecting PDF documents from S3, configuring the Embedding model, and automatically creating a Vector Store so the chatbot can retrieve semantic information.

### Implementation Content
* [Setting up Amazon S3 Bucket](5.4.1-security-groups//)
* [Importing Data from S3 into DynamoDB](5.4.2-rds//)
* [Setting up AWS Bedrock Knowledge Base](5.4.3-elasticache//)