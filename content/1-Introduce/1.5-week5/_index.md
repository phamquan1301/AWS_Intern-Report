---
title: "Week 5 Worklog"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives

- Build a clear understanding of the Shared Responsibility Model and the division of security duties between AWS and its customers.
- Gain proficiency in Identity and Access Management (IAM) — covering users, groups, roles, and policies.
- Explore AWS Organizations, AWS IAM Identity Center, and AWS KMS as tools for enterprise-scale security management.
- Learn how to configure permissions, apply encryption strategies, and enforce security auditing within an AWS environment.
- Get acquainted with Amazon Cognito for application-level authentication and AWS Security Hub for continuous security posture evaluation.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------| 
| 1 | Explored the Shared Responsibility Model: how security duties are divided between AWS and customers, and how responsibility levels vary across service categories — Infrastructure, Hybrid, and Fully Managed. | 02/02/2026 | 02/02/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Studied the AWS Root Account and best practices for securing it: provisioning a dedicated IAM administrator account, safeguarding root credentials, and maintaining domain and email renewal hygiene. | 02/03/2026 | 02/03/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Examined IAM Users, Groups, Roles, and Policies in depth — including how permissions are evaluated, how trust policies work, and why explicit Deny always takes precedence. | 02/04/2026 | 02/04/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Hands-on practice: created IAM Roles, performed role assumption, and used AWS STS (Security Token Service) to issue short-lived, scoped temporary credentials. | 02/05/2026 | 02/05/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Investigated Amazon Cognito (User Pools & Identity Pools) — including how user registration and login flows work, token-based authentication, and how authenticated users gain access to AWS resources. | 02/06/2026 | 02/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Surveyed AWS Organizations, IAM Identity Center, AWS KMS, and AWS Security Hub — covering multi-account governance, centralized access management, encryption key lifecycle, and automated compliance auditing. | 02/07/2026 | 02/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 5 Achievements

##### Shared Responsibility Model

AWS and its customers each carry a distinct set of security obligations, drawing a clear boundary between infrastructure-level protection and workload-level configuration:

- **AWS** is accountable for security *of* the cloud — encompassing physical hardware, global network infrastructure, hypervisor layers, and managed service internals.
- **Customers** are accountable for security *in* the cloud — covering service configuration, data classification, encryption choices, and identity management.
- The customer's share of responsibility scales with the degree of infrastructure control:
  - **Infrastructure services** (e.g., EC2, VPC, EBS) → customers bear the heaviest responsibility.
  - **Managed services** (e.g., RDS, EKS) → responsibility is distributed between both parties.
  - **Fully managed services** (e.g., S3, Lambda) → AWS absorbs the majority of the burden.

##### AWS Root Account

The root account holds unrestricted access to every AWS service and resource and must be treated as a break-glass credential:

- Root credentials should be stored securely and used only when the situation absolutely requires it.
- **Best practices:**
  - Provision a dedicated IAM administrator account and use it for all day-to-day operations.
  - Distribute root account access responsibilities across two separate administrators.
  - Proactively track and renew the domain and email address tied to the root account.

##### AWS Identity and Access Management (IAM)

IAM is the foundational access control service for AWS resources, defining *who* can do *what* on *which* resources:

- **Principals** supported by IAM include: AWS Root user, IAM users, Federated users (via SAML or OAuth), IAM roles, AWS services, and anonymous users.
- **IAM User:**
  - Starts with zero permissions and must be explicitly granted access.
  - Can authenticate via the AWS Management Console or programmatically using Access Key / Secret Key pairs.
  - Not suitable for managing applications, services, or operating systems directly.
- **IAM Group:**
  - A collection of IAM users that simplifies bulk permission management.
  - Groups cannot be nested within other groups.
- **IAM Policy (JSON):**
  - *Identity-based policies* are attached directly to principals (users, groups, roles).
  - *Resource-based policies* are attached to specific AWS resources (e.g., S3 buckets).
  - Evaluation order: **explicit Deny always overrides any Allow**, regardless of other policies.
- **IAM Role:**
  - Carries no long-term credentials — access is only granted upon assumption.
  - A *trust policy* defines which principals are authorized to assume the role.
  - Commonly used for cross-account delegation or granting AWS services (such as EC2) permission to call other AWS APIs.
  - Integrated with AWS STS to deliver time-limited, automatically expiring credentials.

##### Amazon Cognito

Cognito provides a fully managed identity layer for web and mobile applications, removing the complexity of building custom authentication systems:

- **User Pool** — handles user registration, login, multi-factor authentication, and session management.
- **Identity Pool** — maps authenticated (or guest) identities to temporary IAM credentials for accessing AWS services directly.
- Supports native username/password flows as well as federated sign-in via third-party providers such as Google, Facebook, and Amazon.
- A common pattern combines both pools: users authenticate through the User Pool, then exchange tokens for AWS credentials via the Identity Pool.

##### AWS Organizations

AWS Organizations enables governance of an entire portfolio of AWS accounts from a single management account:

- Accounts can be grouped into **Organizational Units (OUs)** to reflect business units, environments, or compliance domains.
- **Consolidated Billing** aggregates usage across all accounts, unlocking volume-based pricing benefits.
- **Service Control Policies (SCPs)** define the maximum possible permissions for OUs or individual accounts — they act as guardrails, not grants: an SCP alone cannot grant permissions, but an explicit SCP deny can block even an administrator.

##### AWS IAM Identity Center (formerly AWS SSO)

IAM Identity Center streamlines access management across all AWS accounts in an organization and external business applications:

- Supports multiple identity sources: AWS Managed Microsoft AD, on-premises Active Directory (via trust relationship or AD Connector), or an external identity provider.
- **Permission Sets** define a collection of policies; when assigned to a user or group for a given account, they automatically provision the corresponding IAM role.

##### AWS Key Management Service (KMS)

KMS is a managed service for creating, storing, and controlling the lifecycle of cryptographic keys:

- Provides **Encryption at Rest** that meets FIPS 140-2 Level 2 compliance requirements.
- **Customer Managed Keys (CMKs)** are the primary resource; they are used to generate **Data Keys** that perform the actual encryption of data in other services.
- KMS never exposes a CMK in plaintext outside of its secure hardware boundary — AWS itself cannot export or access the key material.

##### AWS Security Hub

Security Hub delivers a continuous, automated security posture assessment across an AWS account or an entire organization:

- Evaluates resource configurations against **AWS Foundational Security Best Practices** and industry benchmark standards (e.g., CIS, PCI DSS).
- Produces a normalized **security score** that surfaces the highest-risk findings first.
- Acts as a central aggregation point for security findings from services including Amazon GuardDuty, AWS Config, Amazon Inspector, and others.

##### Hands-On Activities

- Navigated the EC2 console to review running instances and understand service state visibility.
- Created and managed EC2 key pairs for secure SSH-based access.
- Inspected active service configurations across multiple AWS services.
- Developed the ability to coordinate between the AWS Management Console and the AWS CLI to manage resources in parallel, reinforcing both graphical and programmatic workflows.
