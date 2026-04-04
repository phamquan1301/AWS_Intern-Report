---
title: "Week 2 Worklog"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Week 2 Objectives

- Develop a solid understanding of Amazon VPC architecture and how its core network components interact within AWS.
- Get hands-on with Subnet, Route Table, Security Group, NACL, VPC Endpoint, VPC Peering, and Elastic Load Balancer configurations.
- Identify the right connectivity options for linking on-premises infrastructure to AWS Cloud.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Amazon VPC overview: structure, account limits, IPv4/IPv6 CIDR. Configure a basic VPC via Console. | 01/12/2026 | 01/12/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Subnets and Availability Zones: IP addressing, create public/private subnets, assign route tables. | 01/13/2026 | 01/13/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Route Table, Internet Gateway, NAT Gateway, VPC Endpoint. Distinguish Interface vs. Gateway endpoints. | 01/14/2026 | 01/14/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Security Group, Network ACL, VPC Flow Logs. Practice access control and logging between subnets. | 01/15/2026 | 01/15/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | VPC Peering, Transit Gateway, VPN, Direct Connect, and Elastic Load Balancer types (ALB, NLB, CLB, GLB). | 01/16/2026 | 01/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 2 Achievements

##### VPC — Virtual Private Cloud

A VPC is essentially a private, isolated network environment in AWS — used to separate and manage resources by environment (Production / Dev / Test / Staging).

- Default quota: **5 VPCs** per AWS account.
- Every VPC must have an **IPv4 CIDR block**; IPv6 is optional.

##### Subnets

- Each subnet lives in a single **Availability Zone**.
- The subnet CIDR must fall within the parent VPC's CIDR range.
- AWS automatically reserves the **first 5 IP addresses** in every subnet for internal purposes.

##### Route Table

- Controls how traffic is routed within and out of the VPC.
- A **default route table** is auto-created with each VPC (cannot be deleted) and handles internal VPC communication.

##### Elastic Network Interface (ENI) & Elastic IP

- **ENI** is a virtual NIC that can be attached to EC2 instances and migrated between them.
- **Elastic IP** is a static public IPv4 address that binds to an ENI.

##### VPC Endpoint

Lets VPC resources reach AWS services without a public internet connection. Two types:

- **Interface Endpoint** — uses an ENI with a private IP.
- **Gateway Endpoint** — routes traffic via the route table; supports S3 and DynamoDB only.

##### Internet Gateway & NAT Gateway

- **Internet Gateway**: enables EC2 instances in public subnets to communicate with the internet. Fully managed by AWS — no scaling configuration needed.
- **NAT Gateway**: gives private subnet instances outbound-only internet access.

##### Security Group vs. Network ACL

- **Security Group (SG)**: stateful firewall attached to an ENI; supports allow-only rules.
- **Network ACL (NACL)**: stateless firewall applied at the subnet level; has both allow and deny rules, evaluated top to bottom.
- Default behavior: SG blocks all inbound and allows all outbound; NACL allows everything.

##### VPC Flow Logs

- Captures metadata on IP traffic flowing to/from a VPC, subnet, or ENI.
- Logs are stored in **CloudWatch Logs** or **S3**. Packet content is not recorded.

##### VPC Peering & Transit Gateway

- **VPC Peering**: direct one-to-one connection between two VPCs. Transitive routing is not supported; CIDRs cannot overlap.
- **Transit Gateway**: a centralized hub that connects multiple VPCs and on-premises networks together.

##### VPN & AWS Direct Connect

- **Site-to-Site VPN**: links an on-premises data center to AWS using a Virtual Private Gateway and a Customer Gateway.
- **Client-to-Site VPN**: allows a single device to securely connect to resources inside a VPC.
- **AWS Direct Connect**: a dedicated physical link from a data center to AWS (via Direct Connect Partners in Vietnam). Delivers low latency (~20–30 ms) with configurable bandwidth.

##### Elastic Load Balancing (ELB)

Distributes incoming traffic across multiple EC2 instances or containers. Four types are available:

- **ALB (Application Load Balancer)** — Layer 7, supports path-based and host-based routing.
- **NLB (Network Load Balancer)** — Layer 4, designed for high-throughput workloads; supports static IPs.
- **CLB (Classic Load Balancer)** — Layer 4 & 7, legacy option, largely replaced.
- **GLB (Gateway Load Balancer)** — Layer 3, uses the GENEVE protocol (port 6081) for third-party virtual appliances.

Supports **Sticky Sessions** and **Access Logs** (stored in S3).

##### Additional CLI Practice

During the week, continued building CLI fluency:

- Browsed EC2 resources and checked running service status.
- Created and managed key pairs.
- Queried information on active services.

By the end of the week, felt comfortable moving between the Console and CLI depending on the task.
