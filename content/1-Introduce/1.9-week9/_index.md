---
title: "Week 9 Worklog"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives

- Move from theoretical knowledge to hands-on implementation of data engineering concepts on AWS.
- Design and build a complete Data Lake architecture on Amazon S3 with properly layered storage zones.
- Implement an end-to-end ETL pipeline using AWS Glue to ingest, transform, and publish data.
- Configure and use Amazon Athena to query transformed data efficiently with partitioning and columnar formats.
- Build real-time data ingestion pipelines using Amazon Kinesis Data Firehose.
- Monitor, secure, and govern the Data Lake using AWS Lake Formation, IAM, and CloudWatch.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Designed a 3-zone S3 Data Lake (raw, curated, consumption zones), configured bucket policies, lifecycle rules, S3 Intelligent-Tiering, and enabled server-side encryption with KMS. | 03/02/2026 | 03/02/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Built and ran AWS Glue Crawlers against raw zone data, validated schema inference in the Glue Data Catalog, and resolved partition projection issues for large date-partitioned datasets. | 03/03/2026 | 03/03/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Authored a multi-step Glue ETL job: read CSV from raw zone, apply column renaming, type casting, deduplication, and date-based partitioning, then write Parquet to the curated zone. | 03/04/2026 | 03/04/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Set up a real-time ingestion pipeline using Kinesis Data Firehose: configured a Python producer script, transformed records with an inline Lambda function, and delivered compressed Parquet files to S3. | 03/05/2026 | 03/05/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Implemented AWS Lake Formation: registered the S3 Data Lake, granted column-level and row-level permissions to IAM roles, and verified access control from Athena across different user identities. | 03/06/2026 | 03/06/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | Tuned Athena query performance: applied partition pruning, rewrote queries to use columnar projections, analyzed execution plans with EXPLAIN, and set up workgroup cost controls and query result reuse. | 03/07/2026 | 03/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 9 Achievements

##### Designing the S3 Data Lake — Three-Zone Architecture

A well-structured Data Lake on S3 is organized into three logical storage zones, each serving a distinct purpose in the data lifecycle:

| Zone | Purpose | Data State | Typical Format |
|---|---|---|---|
| **Raw Zone** | Landing area for all incoming data as-is | Unprocessed, immutable | CSV, JSON, logs, binary |
| **Curated Zone** | Cleaned, validated, and transformed data ready for analytics | Processed, partitioned | Parquet, ORC |
| **Consumption Zone** | Aggregated or domain-specific datasets optimized for specific use cases | Query-optimized | Parquet (partitioned by business key) |

Key S3 configurations applied to the Data Lake:

- **Bucket policies**: restrict direct write access to the raw zone to only authorized ingestion roles; prevent consumers from bypassing the curated layer.
- **S3 Lifecycle Rules**: automatically transition raw zone objects to S3-IA after 30 days and Glacier after 90 days to minimize long-term storage costs.
- **S3 Intelligent-Tiering**: applied to the curated zone where access patterns are less predictable, automatically moving objects between frequent and infrequent access tiers.
- **Encryption**: all zones encrypted at rest using **SSE-KMS** with a Customer Managed Key, enabling per-zone key rotation policies.
- **Versioning**: enabled on the raw zone to provide a recovery mechanism for accidental overwrites or deletions.

##### AWS Glue — Building the ETL Pipeline

**Crawler Configuration:**

Running Glue Crawlers on the raw zone allowed automatic detection of file structure changes without manual schema updates:

- Crawlers infer Hive-compatible partition keys from S3 prefixes (e.g., `s3://bucket/raw/sales/year=2026/month=03/`).
- **Partition projection**: configured directly in the Glue Data Catalog to avoid crawler re-runs when new partitions are added — significantly reducing catalog refresh latency for high-frequency data arrivals.
- **Exclusion patterns**: configured to skip hidden files, `_SUCCESS` markers, and partial uploads.

**ETL Job — CSV to Parquet Transformation:**

The Glue ETL job executed the following transformation steps using the DynamicFrame API:

1. **Read**: load raw CSV from S3 using `create_dynamic_frame.from_catalog()`.
2. **Apply mappings**: rename columns to snake_case, cast string timestamps to `timestamp` type, and convert string numerics to `double`.
3. **Filter**: drop records with null primary keys or malformed dates using `Filter.apply()`.
4. **Deduplicate**: use `toDF().dropDuplicates(['id', 'event_date'])` to remove exact duplicates.
5. **Repartition**: repartition by date to align output file granularity with downstream query patterns.
6. **Write**: output Parquet with Snappy compression to the curated zone, partitioned by `year/month/day`.

**Job Bookmarks**: enabled to track previously processed S3 objects, ensuring incremental runs process only new data instead of reprocessing the entire dataset.

##### Real-Time Ingestion with Kinesis Data Firehose

A real-time event ingestion pipeline was built to handle continuous data streams:

**Architecture:**

```
Python Producer (Boto3)
    │  PutRecord API calls
    ▼
Kinesis Data Firehose Delivery Stream
    │  Buffer: 60s / 5MB
    ├─► Lambda Transform (JSON → Parquet conversion, field enrichment)
    │
    ▼
S3 Raw Zone (partitioned by Firehose delivery time: yyyy/MM/dd/HH)
    │
    ▼
Glue Crawler (triggered on schedule) → Data Catalog update
    │
    ▼
Athena (queryable within minutes of ingestion)
```

**Key configurations:**

- **Buffer hints**: set to 60-second buffer interval with 5 MB size threshold — whichever is reached first triggers a S3 PUT.
- **Lambda transformation**: inline function enriched records with a `processing_timestamp` and converted JSON arrays to newline-delimited JSON (NDJSON) compatible with Glue.
- **Error logging**: failed transformation records redirected to a separate S3 prefix for dead-letter inspection.
- **Compression**: Firehose configured to GZIP-compress deliveries to reduce S3 storage volume by ~70%.

##### AWS Lake Formation — Fine-Grained Access Control

AWS Lake Formation sits on top of Glue Data Catalog and IAM to provide database-like access control over S3-based data:

- **Data Lake registration**: S3 locations registered with Lake Formation to delegate access decisions away from raw S3 bucket policies.
- **Table-level permissions**: granted `SELECT` on specific Glue tables to analyst IAM roles, while restricting access to raw zone tables entirely.
- **Column-level security**: masked PII columns (e.g., `email`, `phone_number`) from the curated zone for non-privileged analyst roles — the columns simply do not appear in Athena query results.
- **Row-level filtering (RLS)**: applied data filter expressions (e.g., `region = 'VN'`) per IAM principal, ensuring analysts only see rows relevant to their business domain.
- **Tag-based access control (TBAC)**: classified tables with Lake Formation tags (e.g., `sensitivity=PII`, `domain=finance`) and granted permissions at the tag level rather than per table — enabling scalable policy management as new tables are added.

##### Amazon Athena — Query Optimization in Practice

Moving beyond basic queries, several optimization techniques were applied and benchmarked:

**Partition Pruning:**
```sql
-- Before: full table scan (scans all years)
SELECT * FROM sales WHERE CAST(event_date AS DATE) = DATE '2026-03-01';

-- After: partition-aware (only scans year=2026/month=03/day=01)
SELECT * FROM sales WHERE year='2026' AND month='03' AND day='01';
```
Result: scan reduced from **18 GB → 420 MB** (98% reduction).

**Columnar Projection:**
```sql
-- Select only needed columns — Parquet reads only those column chunks
SELECT order_id, total_amount, customer_id
FROM sales
WHERE year='2026' AND status='completed';
```

**Query Result Reuse**: enabled in Athena Workgroup settings — identical queries within a 60-minute window return cached results instantly with zero additional scan cost.

**Workgroup cost controls**: configured per-query data scan limit of 1 GB and per-workgroup monthly scan budget with SNS alert, preventing runaway queries from incurring unexpected costs.

##### Data Quality and Monitoring

**AWS Glue Data Quality (DQDL rules):**
- Defined rules such as `ColumnValues "order_id" matches "[A-Z]{3}-[0-9]{6}"` and `Completeness "email" >= 0.95` to flag records failing quality checks.
- Failed records quarantined to a separate S3 prefix for manual review.

**CloudWatch Monitoring:**
- Glue Job metrics (execution duration, DPU hours, records written) tracked via CloudWatch dashboards.
- Firehose delivery success rate and `DeliveryToS3.Success` metric monitored with alarms at < 99% threshold.
- Athena query execution time and data scanned per workgroup tracked for cost attribution by team.

##### Hands-On Activities

- Built the full 3-zone S3 Data Lake structure with Terraform-style naming conventions and applied lifecycle and encryption policies.
- Ran Glue Crawlers and debugged schema inference issues for nested JSON files using custom classifiers.
- Authored and ran a Glue ETL job end-to-end: 4.2 million raw CSV records processed to 890 MB Parquet output in under 6 minutes using 5 G.1X DPUs.
- Deployed a Kinesis Firehose stream with Lambda transformation; verified end-to-end latency from producer event to Athena-queryable record was under 90 seconds.
- Configured Lake Formation column-level masking and validated with a test IAM role that restricted columns were absent from Athena result sets.
- Benchmarked Athena query performance before and after partitioning and Parquet conversion; achieved 97% cost reduction on the same analytical query.
