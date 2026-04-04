---
title: "Week 8 Worklog"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives

- Understand the fundamental concepts of data analytics on AWS and how they differ from traditional data processing approaches.
- Learn what a Data Lake is, how it differs from a Data Warehouse, and when to use each.
- Master core AWS analytics services: Amazon Athena, AWS Glue, Amazon Kinesis, Amazon EMR, and Amazon QuickSight.
- Understand the role of the AWS Glue Data Catalog as a unified metadata repository.
- Learn how to build end-to-end analytics pipelines on AWS — from ingestion to visualization.
- Get familiar with real-time streaming data processing patterns using Amazon Kinesis.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Studied Data Lake concepts: definition, architecture layers (raw, curated, consumption), and how it differs from a Data Warehouse in terms of schema, flexibility, and cost. | 02/23/2026 | 02/23/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Explored AWS Glue: crawler-based schema discovery, ETL job authoring, the Glue Data Catalog, Glue Studio visual editor, and Glue DataBrew for no-code data preparation. | 02/24/2026 | 02/24/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Learned about Amazon Athena: serverless interactive query service, Presto-based SQL on S3, partitioning and columnar format optimization (Parquet, ORC), and cost management via data scanning control. | 02/25/2026 | 02/25/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Investigated Amazon Kinesis: Data Streams (real-time ingest), Data Firehose (delivery to S3/Redshift/OpenSearch), Data Analytics (SQL over streams), and Video Streams for media workloads. | 02/26/2026 | 02/26/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Examined Amazon EMR: managed Hadoop/Spark clusters, instance fleet configuration, EMR Serverless and EMR on EKS, and common big data processing patterns at scale. | 02/27/2026 | 02/27/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Explored Amazon QuickSight: SPICE in-memory engine, dataset connections, visual types, ML Insights (anomaly detection, forecasting), and embedding dashboards into applications. | 02/28/2026 | 02/28/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 8 Achievements

##### Data Lake vs. Data Warehouse — Foundational Concepts

A **Data Lake** is a centralized repository that stores raw data in its native format at scale — structured, semi-structured, and unstructured — at significantly lower cost than a Data Warehouse:

| Dimension | Data Lake | Data Warehouse |
|---|---|---|
| **Schema** | Schema-on-read (applied at query time) | Schema-on-write (enforced at ingestion) |
| **Data types** | Any format (JSON, CSV, Parquet, images, logs) | Structured, relational only |
| **Cost** | Low (S3-based storage) | Higher (compute + storage co-located) |
| **Flexibility** | High — store first, define structure later | Low — schema must be defined upfront |
| **Query latency** | Higher for ad hoc queries | Lower for pre-aggregated reporting |
| **AWS service** | S3 + Glue + Athena | Amazon Redshift |

**The Lake House Architecture** — the modern AWS recommendation — combines both: a Data Lake on S3 as the central store, with Redshift as a Data Warehouse for structured analytics, connected by Redshift Spectrum and Athena for cross-service queries.

##### AWS Glue — Serverless ETL and Data Catalog

AWS Glue is a fully managed, serverless ETL (Extract, Transform, Load) service that forms the backbone of most AWS data pipelines:

- **Glue Crawlers**: automatically scan data sources (S3, RDS, Redshift, DynamoDB), infer schema, and populate the **Glue Data Catalog** — a central metadata repository compatible with Apache Hive Metastore.
- **Glue ETL Jobs**: Python or Scala scripts running on Apache Spark under the hood, auto-scaling compute (DPU — Data Processing Units) without cluster management.
- **Glue Studio**: a visual drag-and-drop interface for building ETL pipelines without writing code — generates PySpark scripts automatically.
- **Glue DataBrew**: a no-code data preparation tool for data analysts — apply 250+ built-in transformations, profile datasets, and detect quality issues interactively.
- **Glue Data Catalog**: acts as a unified schema registry for Athena, EMR, Redshift Spectrum, and Lake Formation — providing a single source of truth for what data exists and its structure.
- **Glue Workflows**: orchestrate multi-step ETL pipelines with triggers (on-demand, scheduled, or event-based).

##### Amazon Athena — Serverless Interactive Queries

Athena is a serverless, interactive query service that allows running standard SQL directly against data stored in Amazon S3 with no infrastructure to manage:

- **Presto-based engine**: executes ANSI SQL with support for complex joins, window functions, and subqueries across petabytes of data in S3.
- **Pay-per-scan billing**: charges $5 per TB of data scanned — making query optimization directly tied to cost reduction.
- **Cost optimization strategies:**
  - **Partitioning**: organize S3 data into prefixes (e.g., `year=/month=/day=`) so Athena scans only relevant partitions.
  - **Columnar formats**: convert CSV/JSON to **Parquet** or **ORC** to reduce scan volume by 60–90%.
  - **Compression**: Snappy or Gzip further reduces data read from S3.
- **Federated queries**: Athena Data Source Connectors allow querying data in RDS, DynamoDB, Redshift, and custom sources alongside S3.
- **Athena for Apache Spark**: run interactive Spark applications without cluster management.
- **Integration**: results can be written back to S3, visualized in QuickSight, or consumed by downstream pipelines.

##### Amazon Kinesis — Real-Time Data Streaming

Kinesis is a family of services for collecting, processing, and analyzing real-time streaming data at scale:

**Kinesis Data Streams (KDS):**
- Ingests data in real time with sub-second latency.
- Data is partitioned across **shards** (1 MB/s in, 2 MB/s out per shard); shard count drives throughput and cost.
- Data is retained for 24 hours (extendable to 365 days).
- Consumers include Lambda, Kinesis Data Analytics, KCL (Kinesis Client Library) applications, and Apache Flink.

**Kinesis Data Firehose:**
- Fully managed delivery pipeline — no consumer application needed.
- Buffers, optionally transforms (via Lambda), and delivers to S3, Redshift, Amazon OpenSearch, Splunk, or any HTTP endpoint.
- Near-real-time delivery (buffer window: 60 seconds or 1–128 MB).

**Kinesis Data Analytics:**
- Run Apache Flink applications or SQL queries on live data streams in real time.
- Detect patterns, sessionize events, compute running aggregations, and trigger alerts.

**Kinesis Video Streams:**
- Ingests, stores, and processes video streams from devices (cameras, IoT sensors) for ML or playback.

##### Amazon EMR — Managed Big Data Processing

Amazon EMR is a fully managed cloud service for running large-scale distributed data processing jobs using open-source frameworks:

- **Supported frameworks**: Apache Spark, Hadoop MapReduce, Hive, HBase, Presto, Flink, and more.
- **Cluster modes**:
  - **Long-running clusters**: persistent clusters for ongoing workloads.
  - **Transient clusters**: spin up, run a job, terminate — pay only for job execution time.
- **EMR Serverless**: submit Spark or Hive jobs without managing clusters; EMR allocates resources automatically and scales to zero when idle.
- **EMR on EKS**: run Spark jobs on Amazon EKS, sharing Kubernetes infrastructure with other containerized workloads.
- **Instance Fleet**: mix of On-Demand and Spot Instances optimized by EMR to balance cost and availability.
- **Storage**: jobs process data directly from S3 (decoupled compute/storage), optionally using HDFS on instance storage for intermediate data.

##### Amazon QuickSight — Cloud-Native Business Intelligence

QuickSight is AWS's fully managed, cloud-native BI and data visualization service, accessible via browser or mobile without any server deployment:

- **SPICE (Super-fast, Parallel, In-memory Calculation Engine)**: imports and caches datasets in memory for sub-second query responses at scale, even when the underlying source is unavailable.
- **Data connections**: natively connects to Athena, Redshift, RDS, S3, Salesforce, Jira, and many third-party sources.
- **ML Insights:**
  - **Anomaly Detection**: automatically surfaces unusual spikes or drops in metrics.
  - **Forecasting**: ML-powered time-series prediction using historical data.
  - **Auto-Narratives**: generates natural language summaries of chart data.
- **Embedded analytics**: dashboards can be embedded into web applications using 1-click embedding, enabling product teams to build analytics within their own products.
- **Pricing model**: pay-per-session (readers) or per-author — no infrastructure costs.

##### End-to-End Analytics Pipeline on AWS

A typical AWS analytics pipeline follows this flow:

```
Raw Data Sources
    │
    ▼
Amazon Kinesis / S3 (Ingestion & Storage)
    │
    ▼
AWS Glue (Crawl, Catalog, ETL Transform)
    │
    ▼
S3 Data Lake (Curated / Parquet / Partitioned)
    │
    ▼
Amazon Athena / Redshift (Query & Analyze)
    │
    ▼
Amazon QuickSight (Visualize & Share)
```

##### Hands-On Activities

- Created an S3-based Data Lake with a layered prefix structure (raw/, curated/, consumption/).
- Ran an AWS Glue Crawler against S3 to discover schema and populate the Data Catalog.
- Authored a Glue ETL job to convert CSV data to Parquet with partitioning by date.
- Queried the curated Parquet dataset using Amazon Athena and compared scan cost vs. the original CSV.
- Set up a Kinesis Data Firehose delivery stream ingesting test events from a Lambda producer into S3.
- Built a QuickSight dashboard connected to Athena, visualizing aggregated metrics with forecasting enabled.
