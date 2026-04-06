---
title : "Proposal"
date: 2026-01-10
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

## 1. Executive Summary
**AnTiScaQ** is an online scam risk-checking system that helps users early detect signs of scams through various input data types such as **phone numbers, domains, and email contents** by verifying credibility and alerting based on risk levels. The system does not provide legal conclusions but serves as a warning, explanation, and prevention guide. The system collects data, receives community reports, and scores risks.

## 2. Problem Statement

#### Current Issues
* Online scams are rapidly increasing and becoming more sophisticated, causing significant damage to users and businesses.
* Lack of a reliable system, making it easy to make wrong decisions.
* Users often have to manually search via browsers (Google, Microsoft Edge, ...) or use fragmented databases to check suspicious phone numbers/links, causing time waste and risks.
* Current tools often lack **contextual content analysis**.
  - Lack of data **from the user community**.
  - Do not provide **clear explanations of risk levels**.
* Extremely high costs to purchase APIs from enterprise Threat Intel platforms (like Recorded Future, VirusTotal).
* Incomplete reporting, **not real-time** when new phishing campaigns break out.

#### Proposed Solution

To solve the growing online scam issues and the lack of a reliable verification system, AnTiScaQ proposes a platform based on a **Hybrid Cloud-Native** architecture with the following main directions:

- **Automated risk detection:** Multi-source analysis (phone, domain, URL, email content) combining rule-based and AI to detect early scam signs.

- **AI Contextual Analysis:** Using Amazon Bedrock to understand content and provide clear explanations of risk levels, helping users make decisions easily.

- **Leveraging community power:** Building a reporting and reputation scoring system based on user contributions, keeping data updated and reflecting reality.

- **Cost & Scalability Optimization:** Applying a Hybrid architecture:
  - Elastic Beanstalk (Docker) for the main backend 
  - Lambda for AI & flexible processing 
  → Reduces costs compared to traditional enterprise systems.

- **AWS Ecosystem Integration:** Using Cognito, RDS, DynamoDB, S3, CloudFront, WAF… to ensure security, performance, and scalability.

- **Simple User Experience:** Providing a single platform replacing manual searches, enabling fast and accurate checks.

**Benefits**
- Early detection of scam signs from multiple data sources (phone, domain, content).
- Provide clear, easy-to-understand risk alerts.
- Save time through automated checking.
- Leverage community data to increase accuracy.
- Protect users while browsing the web.
- Raise awareness and scam prevention skills.
- Improve security: Through continuous monitoring and real-time alerts.

## 3. Solution Architecture

![ConnectPrivate](/AWS_Intern-Report/images/aws_architecture.drawio.png?width=60rem) 

**AWS Services Used**

| AWS Service | Main Function |
| :--- | :--- |
| **AWS Elastic Beanstalk** | Deploy and manage the backend application (Spring Boot) as Docker containers |
| **Amazon RDS for MySQL** | Relational database, storing structured data (user, report, domain, history, ...) |
| **AWS Lambda** | Process serverless tasks (AI chatbot, content analysis, asynchronous processing) |
| **Amazon API Gateway** | Manage and expose APIs, handle request routing, validation, and endpoint security |
| **Amazon DynamoDB** | NoSQL database, storing temporary/real-time data (OTP) |
| **AWS Cognito** | User authentication and authorization (Authentication & Authorization, supports Google SSO) |
| **Amazon S3** | File storage (images, documents, static assets) |
| **Amazon CloudFront** | CDN for content delivery (frontend, files from S3), reducing latency and speeding up load times |
| **Amazon Route 53** | DNS management, mapping domains to the system |
| **AWS WAF** | Protect APIs from attacks (rate limiting, anti-bot, anti-abuse) |
| **Amazon CloudWatch** | System monitoring, logs, metrics, and alerts |
| **Amazon SNS** | Send notifications (email, SMS) to the system and users |
| **Amazon Bedrock** | Provide AI/LLM for chatbot and scam content analysis |
| **AWS Secrets Manager** | Manage sensitive security information (API keys, credentials) |
| **AWS CodePipeline** | Automate CI/CD pipeline (build → test → deploy) |
| **AWS CodeBuild** | Build and test service in the CI/CD pipeline |

#### Component Design

**1. Data Collection & Detection Layer**
* **Data Sources:** Collect data from multiple sources including user input (phone, domain, email content), community data (reports, feedback), and external APIs (WHOIS domain).
* **Ingestion:** The backend (Spring Boot on Elastic Beanstalk) receives and standardizes data, automatically triggering analysis tasks when new data arrives.

**2. Event Processing Layer**
* **Routing & Logic:** API Gateway receives client requests and routes them to the backend, which handles validation, business logic, and request classification.
* **Async & Events:** Asynchronous tasks (AI analysis, content processing) are pushed to AWS Lambda, with event-driven processing handled via SNS (notifications) or internal services.

**3. Orchestration & Business Logic Layer**
* **Core Orchestration:** The backend acts as the main orchestrator, seamlessly managing the processing flow from input → analysis → result delivery.
* **Processing Tasks:** It aggregates data from multiple sources (RDS, DynamoDB, external APIs), calls Lambda to analyze content/violations via AI, calculates risk scores, and classifies risk levels.

**4. Data Processing & Storage Layer**
* **Data Storage:** Utilize Amazon RDS (MySQL) for primary data (user, report, domain, history), Amazon DynamoDB for fast data (OTP), and Amazon S3 for file storage (images, documents).
* **Processing Support:** AWS Lambda supports supplementary data processing, including light ETL and preprocessing for AI.

**5. AI & Analysis Layer**
* **AI Capabilities:** Amazon Bedrock provides robust AI models to analyze email content, phone numbers, and domain names, explain risk levels, and power a chatbot for scam prevention consulting.
* **Integration:** AWS Lambda acts as the secure intermediary to call Bedrock and process its analytical results.

**6. Presentation & User Interaction Layer**
* **User Interface:** The Frontend (React) is deployed via S3 + CloudFront, communicating securely with the backend via API Gateway.
* **Access Control:** User authentication and sessions are managed via AWS Cognito (JWT).

**7. Security & Monitoring Layer**
* **Security & Access:** Implement Authentication via AWS Cognito (SSO, JWT) and manage overall security using AWS WAF, IAM, ACM (SSL), and Route 53 (DNS).
* **Operations:** System monitoring is handled by Amazon CloudWatch (logs, metrics) and SNS (alerts), while credentials are encrypted and managed by AWS Secrets Manager.

## 4. Technical Deployment Roadmap

**Step 1: Infrastructure & Platform Setup**
* **Network & Security:** Setup VPC (Public Subnet for LB/Internet, Private Subnet for RDS), configure Internet Gateway, and set up Security Groups (Beanstalk: HTTP/HTTPS, RDS: Backend access only).
* **Core Deployment:** Deploy Spring Boot Backend as Docker containers on AWS Elastic Beanstalk. Provision Amazon RDS (MySQL) for main data, DynamoDB for OTP, and S3 for file/image storage.
* **Base Services:** Setup AWS Cognito (Authentication + Google SSO), build basic APIs (Auth, Threat CRUD, Report), and build Admin Dashboard APIs.

**Step 2: APIs & Web Portal MVP**
* **Public Services:** Build Public Lookup APIs (Check phone, domain, content).
* **Frontend:** Deploy React.js application on Amazon S3 + CloudFront.
* **Features & Routing:** Build scam reporting workflow, basic statistical dashboard, and configure Route 53 for domain mapping.

**Step 3: Data Processing & Core Logic**
* **Logic Systems:** Integrate WHOIS API for domain checking. Build systems for Risk scoring (rule-based + community) and Risk level classification.
* **AI & Async:** Integrate AWS Lambda + Amazon Bedrock to handle Email content analysis, Consulting Chatbot, and Asynchronous processing.

**Step 4: System Finalization & Advanced Frontend**
* **UI/UX Refinement:** Refine the Detailed Dashboard and Check history interfaces.
* **Integrations:** Develop and integrate a Browser extension for real-time website warnings.
* **Enhancements:** Optimize API responses and overall user experience.

**Step 5: Testing, Security & Optimization**
* **Testing:** Execute Unit tests, Integration tests, and Load testing.
* **Security:** Implement AWS WAF to prevent API abuse, configure IAM Roles for strict authorization, and provision AWS ACM for SSL/TLS certificates.
* **Optimization:** Enable Auto Scaling for Elastic Beanstalk, configure CloudFront caching, and perform Query optimization on RDS.

#### Technical Requirements

| Component | Description |
| :--- | :--- |
| **Frontend & Dashboard** | The interface uses the Next.js framework, hosted statically on Amazon S3 and distributed globally at high speed via the CloudFront CDN network. |
| **Backend & Logic Processing** | Core logic is programmed in Python 3.12 running on the AWS Lambda platform and routed through Amazon API Gateway. |
| **Data & Storage** | System data uses Amazon DynamoDB to ensure lightning-fast query speeds, combined with Amazon S3 to securely store evidence files. |
| **Infrastructure (IaC)** | All AWS infrastructure resources are defined and managed entirely as code via the AWS Cloud Development Kit (CDK). |
| **Security & Monitoring** | The system applies AWS Cognito multi-factor authentication, strictly manages permissions via IAM, and is continuously monitored and logged by Amazon CloudWatch. |
| **CI/CD** | AWS CodePipeline (automated pipeline), AWS CodeBuild (build & test) |

## 5. Roadmap & Milestones

| Week | Phase | Key Activities | Deliverables |
| :--- | :--- | :--- | :--- |
| **1-2** | Platform (MVP Core) | AWS setup (VPC, Subnet, Security Group), deploy Beanstalk + RDS, configure Cognito, build basic APIs | Auth, Threat API, Report completed, backend successfully deployed |
| **3-4** | APIs & Automation | Build lookup APIs (phone, domain, URL), integrate WHOIS, configure notifications (SNS), setup CI/CD | Lookup APIs operational, Web portal MVP, CI/CD pipeline |
| **5-6** | AI & Advanced | Integrate Lambda + Bedrock, content analysis, AI risk scoring, asynchronous processing | AI Chatbot, content analysis, advanced risk scoring |
| **7-8** | Dashboard & Integration | Build admin dashboard, statistics API, refine frontend, extension integration | Admin dashboard, completed interface |
| **9-10** | System Optimization | Performance optimization (Auto Scaling, CloudFront), database query optimization, API improvements | Stable system, improved performance |
| **11** | Security | Configure WAF, IAM, SSL (ACM), logging with CloudWatch, security testing | Fully secured system |
| **12** | Testing | Unit test, integration test, load test, end-to-end testing, bug fixing | Test reports, stable system |
| **13** | Handover | Write documentation (APIs, architecture), system demo, GitHub standardization | Complete documentation, demo, project handover |

## 6. Budget Estimation

**Monthly AWS Costs**
*Serverless Architecture - Cost Optimization*

| Service | Configuration | Cost/Month |
| :--- | :--- | :--- |
| **AWS Lambda** | 15K invocations, 512MB, 4000ms avg | $0 *(Free tier)* |
↳ Free tier: 1M requests + 400K GB-seconds/month
| **Amazon API Gateway** | 15K REST API requests | $0 |
| **Amazon DynamoDB** | On-demand, 5GB storage, 1M reads, 0.5M write | $0.5 |
| **Amazon S3 Vectors** | 2GB data, PUT/GET | $0.60 |
| **Amazon Bedrock** | Model: Claude Haiku 3, Input Token: 6GB, Output Token: 4GB | $6.5 |
| **Amazon CloudFront** | 10GB transfer, 200K requests | $1.00 |
| **Amazon Route 53** | 1 hosted zone | $0.90 |
| **Amazon CloudWatch**| Basic logs and Auth | $0 *(Free tier)* |
| **AWS Secrets Manager**| Proxy key management | $0.4 |
| **AWS Elastic Beanstalk (EC2)** | t3.micro | $11.68 |
| **Amazon Cognito** | 1000 MAU | $0 |
↳ Free tier: < 50K MAU
| **Amazon RDS for MySQL** | instance db.t4g.micro, gp3 | $21.01 |
| | **TOTAL AWS/MONTH** | **~$42.59** |


## 7. Risk Assessment & Mitigation

| Risk | Impact | Mitigation |
| :--- | :--- | :--- |
| **Backend & RDS Overload (Spike traffic)** | High | Configure Auto Scaling for Elastic Beanstalk based on CPU/Network. Use Connection Pooling (like HikariCP) for Spring Boot to avoid crashing RDS. |
| **Sudden Spike in Amazon Bedrock (AI) Costs** | High | Set strict Rate Limiting at API Gateway/WAF. Limit `max_tokens` for the model. Cache duplicate content analysis results to avoid calling the AI API repeatedly. |
| **False Positives (Incorrect community reports)** | High | Apply a reputation scoring system (Trust score) for users. Require cross-verification from multiple sources (WHOIS, Rule-based) before AI issues official warnings. |
| **Scraper / WHOIS API IP Block or Rate Limit** | Medium | Use rotating proxy pools (proxy info securely stored in AWS Secrets Manager) and establish a Retry mechanism (Exponential Backoff) when calling external APIs. |
| **Lambda Timeout during AI/Async processing** | Low | Decouple heavy Bedrock calls from the synchronous API flow. Communicate via Amazon SNS for Lambda to process in the background (async) and return results later. Optimize Lambda bundle size. |


#### Best practices for Cost & Performance Optimization

* **Elastic Beanstalk & RDS:** With a `t3.micro` instance, you need to optimize MySQL queries (proper indexing, avoid N+1 issues). Limit the number of connections from Spring Boot to RDS to optimize memory.
* **Lambda & Bedrock:** Keep the deployment file (bundle size) small. Initialize connections (HTTP clients, DB connections) outside the *handler function* to leverage reuse mechanisms, avoiding *cold starts*. Closely monitor Bedrock costs via AWS Budgets.
* **DynamoDB:** Since you use DynamoDB primarily to store OTPs, configure the **TTL (Time to Live)** feature so the system automatically deletes expired OTP codes, saving maximum storage capacity. Using On-demand billing is reasonable for the MVP.
* **S3 & CloudFront:** Set up *Lifecycle policies* on S3 to automatically transition older evidence images or log files to cheaper storage classes (Glacier). Enable caching on CloudFront to reduce the number of direct requests to S3.
* **API Gateway & WAF:** Enable Response caching (30-60s) for public lookup APIs (Phone, Domain check) to reduce the backend load. It is mandatory to set up Throttling and WAF Rate-based rules to block bot spam or DDoS attacks from exhausting resources.

## 8. Expected Outcomes

#### Technical Improvements
* **Automation & High Speed:** Completely replaces the cumbersome manual lookup process. The system detects, analyzes, and responds to results almost instantly (real-time).
* **AI Breakthroughs:** Successfully applies Amazon Bedrock to "understand" the complex context of phishing content, vastly outperforming traditional rule-based keyword filters.
* **Flexible & Resilient Infrastructure:** The Hybrid Cloud-Native architecture (combining Beanstalk and Lambda) allows the system to seamlessly Auto Scale during traffic spikes without crashing or creating bottlenecks.
* **High Accuracy:** Minimizes False Positives thanks to multi-source cross-verification mechanisms and a community reputation scoring system.

#### Business Value
* **Breakthrough Cost Optimization:** Operates the entire system at an ultra-low cost (under $30/month), creating an absolute competitive advantage over purchasing expensive APIs from enterprise Threat Intel platforms.
* **Practical User Protection:** Promptly prevents financial damage and personal data leaks through clear, easy-to-understand warnings.
* **Time-Saving:** Saves thousands of lookup hours for the community thanks to an "All-in-one" checking platform.
* **Self-Sustaining Ecosystem:** Attracts and builds an active community of users reporting threats, helping data to be continuously enriched automatically with zero collection costs.

#### Long-term Vision 
* **Comprehensive Security Ecosystem:** Expands coverage from the Web Portal to Browser Extensions and AI Chatbots, protecting users at every touchpoint in cyberspace.
* **Exclusive Threat Intelligence Database:** Owns a highly specific and fastest-updating scam identification database, opening up B2B commercialization opportunities by providing APIs back to financial institutions and banks.
* **Predictive Artificial Intelligence:** Upgrades the AI model from "Detection" to "Prediction", capable of identifying sophisticated phishing campaigns from the moment they begin to form.

## 9. Conclusion

The **AnTiScaQ** system with a **Hybrid Cloud-Native** architecture provides:

* **Ultra-Optimized Costs:** Extremely economical operation, only about **~$27.43/month** for the entire system.
* **No upfront cost:** Maximizes AWS Free Tier and in-house team resources, requiring absolutely zero initial hardware investment.
* **Massive ROI:** Completely eliminates the dependence and high costs of purchasing APIs from enterprise Threat Intel platforms.
* **Scalable:** Flexible Auto Scaling capabilities (Beanstalk) combined with Serverless (Lambda), seamlessly handling traffic explosions caused by phishing campaigns.
* **Operational Optimization:** Leverages the power of Managed Services and Serverless from the AWS ecosystem, minimizing the server administration burden.
* **Fast development:** Rapid deployment roadmap, taking only about **13 weeks (~3 months)** from building to finalizing, testing, and handover.

This is an **ideal, groundbreaking, and practical** solution to build an automated cybersecurity alert platform, harnessing community power and AI without requiring a massive infrastructure budget.