---
title: "Week 7 Worklog"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives

- Gain a deep understanding of advanced relational database capabilities available on AWS.
- Understand how Amazon Aurora differs architecturally from standard RDS and why it excels at scale.
- Explore Aurora Serverless, Aurora Global Database, and Aurora Multi-Master for globally distributed workloads.
- Learn advanced RDS features: Multi-AZ deployments, cross-region Read Replicas, and Performance Insights.
- Understand how to tune, monitor, and optimize relational databases running on AWS for production workloads.
- Get familiar with database proxy services (RDS Proxy) and how they improve connection management and resilience.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------| 
| 1 | Reviewed Aurora's distributed storage architecture: 6-way replication across 3 AZs, self-healing storage, and how it eliminates the replication lag typical of standard RDS. | 02/16/2026 | 02/16/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Studied Aurora Serverless v1 and v2: on-demand capacity scaling, ACU (Aurora Capacity Units), use cases for intermittent and variable workloads, and how billing works per second. | 02/17/2026 | 02/17/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Examined Aurora Global Database: cross-region replication with sub-second latency, managed failover, and disaster recovery patterns for globally distributed applications. | 02/18/2026 | 02/18/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Explored RDS Multi-AZ deployments in depth: synchronous vs. asynchronous replication, automatic failover mechanics, maintenance windows, and applying patches with minimal downtime. | 02/19/2026 | 02/19/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Learned about RDS Proxy: connection pooling, IAM authentication integration, improved failover handling, and reducing database load from serverless and Lambda-intensive workloads. | 02/20/2026 | 02/20/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Investigated performance tuning tools: RDS Performance Insights, Enhanced Monitoring, CloudWatch metrics for databases, slow query logs, and parameter group optimization strategies. | 02/21/2026 | 02/21/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 7 Achievements

##### Amazon Aurora — Architecture Deep Dive

Aurora reimagines relational databases for the cloud with a fundamentally different storage layer compared to traditional databases:

- **Distributed storage**: Aurora separates compute from storage. The storage layer automatically replicates data across **6 storage nodes** distributed across **3 Availability Zones**, providing 6-way redundancy.
- **Self-healing**: If any storage node fails, Aurora automatically repairs it using the remaining copies — no DBA intervention required.
- **Replication lag eliminated**: Because all Aurora instances read from the same shared distributed storage volume, read replicas reflect writes with near-zero lag (typically under 10ms), unlike standard RDS where replicas apply binlog events asynchronously.
- **Crash recovery**: Aurora replays the redo log on startup rather than full buffer pool reconstruction, dramatically reducing recovery time after a crash.

| Feature | Standard RDS | Amazon Aurora |
|---|---|---|
| Storage replication | 2 copies (1 AZ per engine) | 6 copies across 3 AZs |
| Read replica lag | Seconds (async) | Sub-10ms (shared storage) |
| Storage auto-scaling | Manual resize | Automatic in 10 GB increments |
| Crash recovery time | Minutes | Seconds |

##### Aurora Serverless

Aurora Serverless removes the need to provision or manage database capacity — compute scales automatically based on actual usage:

- **Aurora Capacity Units (ACUs)**: each ACU represents approximately 2 GiB of memory and proportional CPU and networking. You define a minimum and maximum ACU range, and Aurora scales within that range.
- **Serverless v2** (recommended): scales in fine-grained increments of 0.5 ACU, responds to demand within fraction of a second, and supports Multi-AZ for high availability.
- **Serverless v1**: suitable for infrequent or unpredictable workloads; can scale to zero and pause completely when idle, resuming on the next connection (with a brief cold-start delay).
- **Billing**: charged per ACU-second consumed, making it highly cost-efficient for dev/test, seasonal, or sporadic production workloads.
- **Ideal use cases**: new applications with uncertain traffic, development and staging environments, infrequent batch processing, and SaaS applications with variable per-tenant loads.

##### Aurora Global Database

Aurora Global Database enables a single Aurora cluster to span multiple AWS regions, providing low-latency global reads and fast disaster recovery:

- **Architecture**: one **primary region** accepts all writes; up to **5 secondary regions** each maintain read-only replicas.
- **Replication**: uses dedicated storage-level replication (not database-level binlog), achieving typical replication latency of **under 1 second**.
- **Managed failover**: in the event of a regional outage, a secondary region can be promoted to primary within approximately **1 minute** (RTO < 1 min, RPO < 1 second).
- **Global reads**: applications in secondary regions can query local replicas without crossing region boundaries, significantly reducing read latency for geographically distributed user bases.
- **Write forwarding (Serverless v2)**: secondary regions can optionally forward write requests to the primary region transparently, simplifying application logic.

##### RDS Multi-AZ — In-Depth

Multi-AZ is AWS's primary mechanism for synchronous high-availability within a single region:

- **Synchronous replication**: every write committed on the primary is synchronously replicated to the standby before acknowledgment — zero data loss on failover.
- **Automatic failover**: if the primary becomes unhealthy (instance failure, storage failure, OS crash, or AZ outage), RDS automatically promotes the standby and updates the CNAME DNS endpoint, typically completing in **60–120 seconds**.
- **Standby is not readable**: the Multi-AZ standby is a pure failover target — it cannot serve read traffic (use Read Replicas for that).
- **Patching with reduced downtime**: RDS applies patches to the standby first, performs a failover, then patches the original primary—minimizing the visible maintenance window.

**Multi-AZ vs. Read Replica — Key Distinctions:**

| Aspect | Multi-AZ | Read Replica |
|---|---|---|
| Purpose | High availability / failover | Read scaling / reporting |
| Replication | Synchronous | Asynchronous |
| Readable | No | Yes |
| Scope | Same region (different AZ) | Same or different region |
| Failover | Automatic | Manual promotion |

##### Amazon RDS Proxy

RDS Proxy is a fully managed, highly available database proxy that sits between applications and RDS or Aurora databases:

- **Connection pooling**: maintains a pool of established database connections and multiplexes them across application connections — critical for Lambda functions and container workloads that open and close connections rapidly.
- **Reduced database load**: by reusing connections, RDS Proxy can support thousands of simultaneous application connections while maintaining only a fraction of that count at the database level, staying within engine max-connection limits.
- **IAM authentication**: RDS Proxy supports IAM-based credentials (no hardcoded passwords) for database authentication, improving security posture.
- **Failover acceleration**: during Multi-AZ failover events, RDS Proxy queues incoming connections and re-routes them to the new primary transparently, reducing the effective failover time visible to applications.
- **Supports**: MySQL, PostgreSQL, Aurora MySQL, Aurora PostgreSQL, MariaDB, and SQL Server.

##### Performance Tuning & Monitoring

AWS provides a rich observability stack for relational databases:

**RDS Performance Insights:**
- Visualizes **DB Load** — the average number of active sessions at any point in time, broken down by wait event, SQL statement, host, or user.
- Helps pinpoint the exact query or resource bottleneck (CPU, I/O, lock waits) causing performance degradation.
- Retains 7 days of history free; extends to 2 years with paid retention.

**Enhanced Monitoring:**
- Streams OS-level metrics (CPU steal, memory pressure, file I/O) to CloudWatch Logs at 1-second granularity.
- Essential for diagnosing issues invisible at the database layer (e.g., noisy-neighbor effects on shared hosts).

**Parameter Groups:**
- Control engine-level configuration (buffer pool size, max connections, query cache, etc.).
- Changes to static parameters require a DB instance reboot; dynamic parameters apply immediately.
- Best practice: use custom parameter groups rather than the default to retain control over settings during upgrades.

**Slow Query Log:**
- Captures queries exceeding a configurable execution time threshold.
- Analyze with tools like **pt-query-digest** (Percona Toolkit) or directly via CloudWatch Logs Insights to identify candidates for index optimization.

##### Hands-On Activities

- Launched an Aurora MySQL cluster and examined the cluster endpoint vs. reader endpoint behavior under load.
- Configured a Read Replica in a secondary region and validated replication lag metrics via CloudWatch.
- Enabled RDS Performance Insights and identified top wait events on a test workload.
- Created and applied a custom DB parameter group, adjusting `max_connections` and `innodb_buffer_pool_size` for a MySQL-compatible instance.
- Tested RDS Proxy connection pooling behavior with a Lambda function making burst database calls.
