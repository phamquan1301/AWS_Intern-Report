---
title: "Week 3 Worklog"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Week 3 Objectives

- Build a thorough understanding of Amazon EC2 and its companion storage services — EBS, EFS, and FSx.
- Learn how EC2 works under the hood, how to configure it, and how the different pricing models compare.
- Get hands-on with launching EC2 instances, managing EBS volumes, and working with snapshots.
- Apply Auto Scaling, explore pricing options, and get introduced to Amazon Lightsail.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | EC2 fundamentals: concepts, elastic scalability, comparison with physical servers. Instance types overview — CPU, memory, network, and storage specs. | 01/19/2026 | 01/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | AMI (Amazon Machine Image) and the EC2 provisioning process. Hypervisor types: KVM, HVM, PV — and how to choose between them. | 01/20/2026 | 01/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Key Pair mechanics and credential encryption for Linux and Windows. EBS deep dive: HDD vs. SSD types, snapshots, and data replication. | 01/21/2026 | 01/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Instance Store: characteristics, trade-offs, and when to use it. Practice: backing up EBS with snapshots and restoring from them. | 01/22/2026 | 01/22/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | User Data and Metadata in EC2 — automating setup at instance launch. EC2 Auto Scaling: policies, multi-AZ setup, and Load Balancer integration. | 01/23/2026 | 01/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | EC2 Pricing Options: On-Demand, Reserved Instances, Savings Plans, and Spot Instances. Introduction to Amazon Lightsail, EFS, FSx, and AWS MGN (Application Migration Service). | 01/24/2026 | 01/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 3 Achievements

##### Amazon EC2 — Architecture and How It Works

EC2 functions similarly to a traditional virtual machine but with far more flexibility and speed:

- Instances can be launched in minutes and scaled freely to match any type of workload — web servers, application backends, database hosts, and more.
- The underlying architecture is based on the **Nitro Hypervisor**, with support for **HVM** and **PV** virtualization modes.

##### Instance Types, AMI, and Key Pairs

- Each **Instance Type** is defined by its CPU, memory, network throughput, and storage profile — choosing the right one directly affects performance and cost.
- An **AMI** is a preconfigured image containing the OS, application stack, and settings — it defines what an instance looks like when it starts.
- **Key Pairs** use asymmetric encryption: the public key is stored by AWS, while the private key is kept locally for SSH (Linux) or RDP password decryption (Windows).

##### EBS — Elastic Block Store

- Block storage that attaches directly to an EC2 instance over a dedicated private network.
- Available in multiple volume types: **gp2/gp3** (general-purpose SSD), **io1/io2** (high-performance SSD), **st1/sc1** (HDD for throughput-intensive workloads).
- **Snapshots** are incremental backups stored in S3 — subsequent snapshots only save changed blocks.
- EBS volumes persist independently of the instance lifecycle.

##### Instance Store

- Temporary storage physically attached to the host machine — delivers extremely high IOPS.
- Data is lost when the instance is stopped or terminated. Suitable for caches, buffers, and scratch data.

##### EFS, FSx, and Storage Comparison

| Storage Type | Protocol | Use Case |
|---|---|---|
| EBS | Block | Single EC2, databases |
| Instance Store | Block | Ephemeral/temp data |
| EFS | NFS | Shared Linux storage (multi-EC2) |
| FSx | SMB / NTFS | Windows or high-performance Linux |

##### EC2 Auto Scaling

- Automatically adjusts the number of running instances based on demand or a defined schedule.
- Spans multiple **Availability Zones** for high availability.
- Integrates natively with **Elastic Load Balancer** to distribute traffic across healthy instances.
- Supports combining multiple pricing models in a single Auto Scaling group.

##### EC2 Pricing Options

- **On-Demand**: pay by the second, no commitment — most flexible, highest per-unit cost.
- **Reserved Instances & Savings Plans**: commit to 1 or 3 years for significant discounts (up to ~72%).
- **Spot Instances**: bid for unused AWS capacity at steep discounts — can be interrupted with a 2-minute warning.

##### Amazon Lightsail

A simplified compute service designed for smaller workloads, prototypes, and dev/test environments. Bundles compute, storage, and networking into a predictable monthly price.

##### AWS Application Migration Service (MGN)

Automates the replication of physical or virtual servers to EC2 — continuous block-level replication with minimal downtime on cutover.

##### Additional Practice

- Navigated the EC2 Console and CLI for resource inspection.
- Created and managed key pairs for instance access.
- Verified running service status using both Console and CLI.

By the end of the week, felt confident managing the full EC2 lifecycle — from launch to backup to scaling — across both the browser interface and the command line.
