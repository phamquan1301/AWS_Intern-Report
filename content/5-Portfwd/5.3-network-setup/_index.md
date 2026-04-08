---
title: "Network Infrastructure Setup"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

### Overview
During this phase, we will build the Virtual Private Cloud (VPC) infrastructure and establish security firewall layers, creating a solid foundation for deploying applications and databases on AWS. Proper network and security configuration is crucial for the system to operate securely and be effectively isolated in the Cloud.

The network infrastructure architecture to be established includes:
* **VPC (Virtual Private Cloud):** Creates a virtual private cloud on AWS through the "VPC and more" wizard, enabling quick setup of the core network infrastructure.

* **Security Group:** Acts as a resource-level virtual firewall, tightly controlling inbound network rules for EC2 systems.

* **DB Subnet Group:** Groups subnets within a VPC together to allocate separate, secure network space for Amazon RDS or Aurora database clusters.

### Implementation Content
* [Create VPC](5.3.1-vpc-subnets/)
* [Create Security Group](5.3.2-nat-gateway//)
* [Create RDS Subnet Group](5.3.3-route-table//)
