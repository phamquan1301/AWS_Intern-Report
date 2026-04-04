---
title: "Week 4 Worklog"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives

- Develop a strong understanding of Amazon S3 and related storage services.
- Understand object-based storage and how it differs from block storage.
- Get comfortable with storage classes, lifecycle policies, CORS, and versioning.
- Explore Amazon Glacier, the Snow Family, AWS Storage Gateway, and AWS Backup.
- Grasp RTO/RPO concepts and the different Disaster Recovery strategies available on AWS.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Introduction to Amazon S3: object storage fundamentals, data replication across Availability Zones, 99.999999999% durability, 99.99% availability. | 01/26/2026 | 01/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | S3 core concepts: buckets, objects, keys, and access points. Storage classes: Standard, IA, Intelligent-Tiering, One Zone, Glacier, Deep Archive. | 01/27/2026 | 01/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Configure S3 lifecycle policies for automatic storage class transitions. Multipart upload and event triggers on object creation/deletion. Static website hosting on S3, CORS policy, ACL, Bucket Policy, and IAM Policy. | 01/28/2026 | 01/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | **Event**: AWS re:Invent Recap HCMC - Track 2: Analytics/ML/AI & Innovation | 01/29/2026 | 01/29/2026 | [AWS re:Invent](https://reinvent-recap-hcmc.splashthat.com/) |
| 5 | S3 Versioning, S3 Endpoints, partition and prefix performance tuning. Amazon Glacier deep dive: retrieval tiers — Expedited, Standard, Bulk. Hands-on access permission configuration. | 01/30/2026 | 01/30/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Snow Family (Snowball, Snowball Edge, Snowmobile). AWS Storage Gateway (File, Volume, Tape). AWS Backup, RTO/RPO, and Disaster Recovery strategies. | 01/31/2026 | 01/31/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 4 Achievements

##### Amazon S3 — Core Concepts

S3 is a fully managed object storage service with unique characteristics compared to block storage:

- Objects cannot be partially edited — any change requires re-uploading the full object.
- Data is automatically replicated across **3 Availability Zones** within the same region.
- Supports event triggers, multipart uploads for large files, versioning, and CORS configuration.

##### S3 Storage Classes

| Class | Best For |
|---|---|
| S3 Standard | Frequently accessed data |
| S3 Standard-IA / One Zone-IA | Infrequent access, cost-optimized |
| S3 Intelligent-Tiering | Data with unpredictable access patterns |
| S3 Glacier / Deep Archive | Long-term archival, retrieval latency acceptable |

##### Lifecycle Policies

- Automate object transitions between storage classes after a defined number of days.
- Schedule automatic deletion of objects that have exceeded their retention window.

##### CORS and Permission Mechanisms

- **CORS** (Cross-Origin Resource Sharing) enables browser-based apps to fetch S3 resources from a different domain.
- **S3 ACL**: coarse-grained access control at the bucket or object level.
- **Bucket Policy / IAM Policy**: fine-grained JSON-based policies for detailed access control.

##### Versioning

- When enabled, every overwrite or deletion creates a new version rather than permanently removing data.
- Makes it straightforward to recover from accidental deletions or unintended overwrites.

##### S3 Performance Optimization

- Using randomized key prefixes distributes requests across partitions and avoids hot-spot throttling.
- Understanding S3's underlying hash-based partition map helps design efficient key naming strategies.

##### Amazon Glacier

Designed for long-term archival storage at very low cost. Three retrieval speed tiers:

- **Expedited**: 1–5 minutes
- **Standard**: 3–5 hours
- **Bulk**: 5–12 hours

##### Snow Family

Purpose-built hardware devices for migrating massive datasets (TB to EB) from on-premises to AWS:

- **Snowball / Snowball Edge**: ruggedized data transfer devices with optional compute capability.
- **Snowmobile**: a truck-scale solution for exabyte-level migrations.

Data is encrypted, compressed, and imported directly into S3 or Glacier after transfer.

##### AWS Storage Gateway

Bridges on-premises infrastructure with cloud storage. Three gateway types:

- **File Gateway** (NFS/SMB) — stores files directly in S3.
- **Volume Gateway** (iSCSI) — block storage with EBS snapshot support.
- **Tape Gateway** (VTL) — replaces physical tape libraries using S3 and Glacier as the backend.

##### AWS Backup and Disaster Recovery

- **RTO** (Recovery Time Objective): the maximum acceptable time to restore operations after a failure.
- **RPO** (Recovery Point Objective): the maximum acceptable data loss measured in time.

Four DR strategies, in order of increasing complexity and cost:

1. **Backup & Restore** — lowest cost, longest recovery time.
2. **Pilot Light** — minimal footprint kept warm in AWS.
3. **Warm Standby** — scaled-down full environment running at all times.
4. **Multi-Site Active-Active** — full capacity in multiple regions, near-zero RTO/RPO.

AWS Backup provides centralized scheduling and management for EBS, EC2, RDS, DynamoDB, EFS, and Storage Gateway.


##### Event: AWS re:Invent Recap HCMC — Track 2: Analytics/ML/AI & Innovation

Attended a full-day track covering AI/ML innovation on AWS, with sessions spanning Vector Databases on S3, Natural Query Language in OpenSearch, and the latest Nova & Bedrock capabilities for GenAI apps. A standout session was hosted by Prapti Gupta — her energy and presentation style made the content immediately engaging and easy to absorb. The afternoon shifted toward applied topics: using MCP to give AI agents live AWS resource access, multimodal retrieval for Bedrock Knowledge Bases, and MLOps best practices with SageMaker AI. Overall, the track offered a well-rounded view of how AWS is pushing the boundary between data infrastructure and intelligent applications.