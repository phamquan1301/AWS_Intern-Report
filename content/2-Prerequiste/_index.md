---
title : "Proposal"
date: 2026-01-10
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

## 1. Executive Summary

**AnTiScaQ** is an integrated threat intelligence platform designed to protect users by verifying the reputation and risk level of phone numbers, emails, and domains. The system automates the entire OSINT (Open Source Intelligence) workflow from data scraping, crowdsourced reporting, threat scoring. This is an **in-house project** developed by the team, focusing on an **MVP with optimized costs under $50/month** in the initial phase, utilizing Lambda, API Gateway, and DynamoDB to ensure high performance during traffic spikes and keep operating costs minimal.

## 2. Problem Statement

**Current Issues**
* Users rely on manual Google searches or fragmented databases to check suspicious numbers or links, causing time waste and high exposure to fraud.
* Threat intelligence data gathering is **not integrated** and highly manual.
* No **automated verification workflows** to cross-check reported scams.
* High costs for enterprise threat intelligence feeds (e.g., Recorded Future, VirusTotal).
* Weak reporting mechanisms, **not real-time** when new phishing campaigns launch.

**Proposed Solution**
The system uses **AWS Serverless Architecture** to optimize costs and handle variable traffic:
* **Compute:** AWS Lambda (pay-per-use, utilizing Python for scraping and Node.js for APIs).
* **API:** API Gateway REST API.
* **Database:** DynamoDB (on-demand billing for lightning-fast lookups).
* **Cache:** ElastiCache Redis (cache.t3.micro) - optional for phase 2 to cache popular scam domains.
* **Authentication:** AWS Cognito (free tier <50K MAU) for API key management and admin access.
* **Storage:** S3 for scraping logs and evidence, CloudFront CDN.
* **CI/CD:** GitHub Actions for automated deployment.
* **Monitoring:** CloudWatch (free tier).
* **Security:** Route 53, WAF (cost-optimized rules to prevent API abuse), IAM Roles.

**Key Features**
* **Single Sign-On** (Google, Microsoft 365) for admins and contributors.
* **Detailed RBAC** (Admin, Moderator, API Consumer, Public User).
* **Real-time lookup API** for domains, emails, and phone numbers.
* **Approval workflows** for user-submitted scam reports to prevent false positives.
* **Web portal** (Next.js) for public search and crowdsourced reporting.

**Benefits**
* Save **70%** of manual checking and data aggregation time.
* Reduce **90%** of false positive threat classifications.
* Cost only **below $50/month** for initial scale (90% cheaper than commercial Threat Intel feeds).
* **In-house development** - no external data licensing or outsourcing costs.

## 3. Solution Architecture

**AWS Services Used**
* **AWS Lambda:** Backend API logic (Node.js 20.x) & automated web scrapers (Python).
* **API Gateway:** REST API endpoints, request validation, rate limiting.
* **Amazon DynamoDB:** NoSQL database (on-demand billing) for threat records.
* **AWS Cognito:** Authentication, SSO, API Keys, JWT tokens.
* **Amazon S3:** Raw data storage (scraping logs, screenshot evidence).
* **CloudFront:** CDN for static Next.js assets and S3.
* **Route 53:** DNS management.
* **AWS WAF** (optional Phase 2): API protection against botnets.
* **CloudWatch:** Logs, monitoring (free tier).
* **Secrets Manager:** API keys for third-party integrations and proxy credentials.

**Component Design**
* **Authentication Layer:** Cognito User Pools with JWT (RS256); Lambda authorizer for API Gateway to validate API keys.
* **API Layer:** AWS Lambda functions deployed via GitHub Actions; API Gateway REST API with rate limiting (e.g., 10 requests/second per free tier user).
* **Business Logic (Lambda Functions):** * Threat lookups (Read from DynamoDB/Cache).
    * Data Aggregation (Python scrapers pulling from public blacklists).
    * Report management (CRUD for user-submitted scams).
    * Email notifications (SES free tier for threat alerts).
* **Data Layer - DynamoDB Tables:**
    * `Users` - GSI on email
    * `ThreatIntel` - GSI on `indicator_type` (phone/email/domain) + `risk_score`
    * `ScamReports` - GSI on `target_indicator` + `date`
    * `ScrapingLogs` - GSI on `source` + `status`
* **Storage Layer:** S3 Standard for evidence (<30 days); S3 Lifecycle → Glacier Deep Archive (>90 days) for historical logs.
* **Frontend:** Next.js 14 (React 18) + TypeScript - Static export; Material-UI components; Hosted on CloudFront + S3.
* **CI/CD Pipeline:** GitHub Actions workflow to build Lambda packages, deploy via AWS CLI, and sync frontend to S3.

## 4. Technical Implementation

**Phase 1: MVP Core (Month 1-2)**
* **Month 1:** AWS setup (Cognito, DynamoDB, S3, Lambda). Authentication UI. Threat CRUD APIs and admin dashboard.
* **Month 2:** Public lookup APIs. Next.js web portal MVP. Manual report submission workflow. Basic trending dashboard.

**Phase 2: Automation & Data Aggregation (Month 3-4)**
* **Month 3:** Python web scraping engine (Lambda) to automate blacklist data collection. Reputation scoring logic.
* **Month 4:** Email notifications (SES) for subscribed alerts. Audit logging. Evidence upload to S3.

**Phase 3: Advanced Features (Month 5-6)**
* AI/ML Risk Scoring module (predictive analysis based on patterns).
* Advanced threat analytics dashboard.
* Security hardening (WAF integration).
* Load testing for high-volume lookup queries.
* Public API documentation launch.

**Tech Stack**
* **Backend:** Node.js 20.x, Python 3.11 (Scraping), AWS Lambda.
* **Database:** DynamoDB (single-table design pattern).
* **Frontend:** Next.js 14, React 18, TypeScript, Material-UI v5.
* **Infrastructure as Code:** AWS SAM / Serverless Framework.
* **CI/CD:** GitHub Actions.

## 5. Roadmap & Milestones

| Month | Phase | Key Deliverables |
| :--- | :--- | :--- |
| 1-2 | MVP Core | Auth, Threat Database, Public Search Portal, Manual Reporting |
| 3-4 | Automation | Python Scraping Engine, API Keys, Alert Notifications |
| 5-6 | Advanced & Launch | AI Scoring, Advanced Analytics, Security Hardening, API Go-Live |

## 6. Budget Estimation

**Monthly AWS Costs (Phase 1: ~5,000 API lookups/day)**
*Serverless Architecture - Cost Optimized*

* **AWS Lambda:** 150K invocations -> $0 (Within free tier)
* **API Gateway:** 150K requests -> $0.15
* **DynamoDB:** On-demand, 5GB storage, 1M reads, 500K writes -> $3.50
* **S3 Storage & Requests:** 20GB data -> $0.60
* **S3 Glacier (archive):** 10GB logs -> $0.10
* **CloudFront:** 10GB transfer, 200K requests -> $1.00
* **Route 53:** 1 hosted zone -> $0.90
* **CloudWatch & Cognito:** -> $0 (Within free tier)
* **Secrets Manager & SES:** -> $0.85
* **Data Transfer & Contingency:** -> $1.20
* **TOTAL AWS/MONTH (Phase 1): ~$8.30**

**Costs When Scaling to Phase 2 (Automation + Caching)**
* **Lambda + API Gateway:** 300K requests -> $0.30
* **DynamoDB:** 10GB, 2M reads -> $9.50
* **S3 + CloudFront:** 40GB storage -> $2.50
* **ElastiCache Redis (optional):** cache.t3.micro -> $12.50
* **AWS WAF (optional):** Basic protection -> $7.00
* **Misc + Contingency:** -> $5.60
* **TOTAL (Phase 2, with cache/WAF): ~$37.40** *(~$17.90 without cache/WAF)*

**Costs When Scaling to Phase 3 (High Volume APIs)**
* **TOTAL (Phase 3 - 750k+ lookups, larger cache, WAF rules): ~$87.50 / month**

**ROI Analysis (In-house project)**
* **Initial Investment:** Setup & Dev tools (~$42) + AWS Dev environment (~$30) = **~$72**
* **First Year Operating Costs:** Phase 1 (6 months) + Phase 2 (6 months) = **~$210**
* **Total Year 1 Cost:** **~$282**
* **Savings:** Commercial Threat Intel APIs easily cost $10,000 - $30,000/year. Building this aggregator in-house saves thousands of dollars annually while providing a proprietary dataset.

## 7. Risk Assessment & Mitigation

| Risk | Impact | Probability | Mitigation |
| :--- | :--- | :--- | :--- |
| **Scraper IP Blocking** | High | High | Use rotating proxy pools (Secrets Manager) and randomize scraping intervals. |
| **False Positives** | High | Medium | Implement multi-source validation and manual approval workflows for high-profile domains. |
| **DynamoDB costs spike** | Medium | Low | On-demand billing, CloudWatch alarms at $30 threshold. Batch writes via SQS. |
| **Lambda time limits** | Low | Medium | Break heavy Python scraping jobs into smaller chunks; optimize bundle sizes. |

## 8. Expected Outcomes

**Technical Improvements**
* **90%** of OSINT data collection automated via Python scrapers.
* Real-time threat dashboard with data < 5 seconds old.
* **< 1s** API lookup response time (P95) with DynamoDB/Redis.
* Infinite scalability during widespread phishing campaigns (traffic spikes).

**Business Value**
* Security team reduces **60%** manual investigation workload.
* Community protection increases with easy self-service lookups.
* **100%** audit trail for reported indicators.
* Cost savings of **$10,000+/year** vs commercial alternatives.
* Operating cost only **$8-12/month** for initial scale.

**Long-term Vision**
* Scale to handle millions of queries per month.
* Integrate AI/ML (AWS Bedrock) for predictive domain generation algorithm (DGA) detection.
* Potential SaaS product offering API keys to B2B clients.

## 9. Conclusion

The **AnTiScaQ** Platform with **Serverless Architecture** provides:
* ✅ **Ultra-low cost:** Only $8-12/month for Phase 1.
* ✅ **No upfront cost:** ~$72 setup, leveraging existing in-house team.
* ✅ **Massive ROI:** Eliminates the need for expensive third-party Threat Intel feeds.
* ✅ **Scalable:** Pay-as-you-go, automatically handles massive traffic spikes when new scams go viral.
* ✅ **Zero maintenance:** Serverless = no server management.
* ✅ **Fast development:** 6 months MVP → production.

This is an **ideal solution** for building a robust, high-performance cybersecurity tool without massive upfront infrastructure investments.