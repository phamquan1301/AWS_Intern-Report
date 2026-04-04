---
title: "Week 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Week 10 Objectives

- Deepen understanding of serverless analytics patterns on AWS and how to build cost-effective, scalable BI solutions.
- Master Amazon QuickSight from dataset connection through to published dashboards and embedded analytics.
- Understand how SPICE works and when to use Direct Query vs. SPICE import mode.
- Learn how to build calculated fields, parameters, and dynamic controls in QuickSight for interactive dashboards.
- Explore QuickSight ML Insights: anomaly detection, forecasting, and narrative summaries.
- Understand how to implement multi-tenant BI with row-level security and column-level security in QuickSight.
- Get familiar with embedding QuickSight dashboards into external web applications using the Embedding SDK.

---

### Weekly Task Plan

| Day | Tasks | Start Date | Completion Date | Reference |
|-----|-------|------------|-----------------|-----------|
| 1 | Connected QuickSight to Athena datasets over the Data Lake built in Week 9; configured SPICE import schedules and compared query performance between Direct Query and SPICE modes. | 03/09/2026 | 03/09/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 2 | Built calculated fields, custom date hierarchies, and conditional formatting rules; designed a multi-sheet executive dashboard with KPI cards, bar charts, line charts, and heat maps. | 03/10/2026 | 03/10/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | Implemented interactive controls: parameters, filter actions, URL actions, and custom tooltips; enabled cross-sheet navigation and drill-down hierarchies for self-service analytics. | 03/11/2026 | 03/11/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | Explored QuickSight ML Insights: configured anomaly detection on revenue metrics, built time-series forecasts with confidence intervals, and added Auto-Narrative summaries to executive sheets. | 03/12/2026 | 03/12/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | Implemented Row-Level Security (RLS) using a dataset rules file and Column-Level Security (CLS) to enforce per-user data access; tested access isolation across different QuickSight user identities. | 03/13/2026 | 03/13/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | **Event:** Cloud Mastery 2026 - #1 AI From Scratch.<br>Embedded a QuickSight dashboard into a Next.js web application using the QuickSight Embedding SDK; implemented anonymous and authenticated embedding with per-session parameter overrides. | 03/14/2026 | 03/14/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |

---

### Week 10 Achievements

##### Amazon QuickSight — Architecture and Data Connectivity

QuickSight connects to a wide variety of data sources and supports two distinct query execution modes that trade off between performance and freshness:

**Data Source Connectivity:**

| Source Type | Examples |
|---|---|
| AWS Services | Athena, Redshift, RDS, Aurora, S3, OpenSearch |
| SaaS | Salesforce, ServiceNow, Adobe Analytics, Jira |
| Databases | MySQL, PostgreSQL, SQL Server, Oracle (via VPC) |
| Files | CSV, Excel, JSON (uploaded to SPICE directly) |

**Direct Query vs. SPICE:**

| Aspect | Direct Query | SPICE Import |
|---|---|---|
| **Data freshness** | Always live — queries hit the source | Snapshot at import time |
| **Performance** | Depends on source query speed | Sub-second (in-memory) |
| **Cost** | Source query costs apply (e.g., Athena scan) | SPICE capacity cost |
| **Scale** | Limited by source concurrency | Scales to millions of readers |
| **Best for** | Real-time operational metrics, small datasets | Analytical dashboards, large reader counts |

**SPICE** (Super-fast, Parallel, In-memory Calculation Engine) ingests data from the source once and stores it in a columnar, in-memory format managed by QuickSight. SPICE refresh can be scheduled hourly, daily, or triggered via API — enabling near-real-time dashboards without continuously querying the underlying source.

##### Building Dashboards — Visual Design and Calculations

**Sheet and Visual Types:**

QuickSight sheets organize visuals into pages. Each sheet corresponds to a focus area (e.g., "Executive Summary", "Regional Breakdown", "Trend Analysis"). Key visual types used:

| Visual | Best For |
|---|---|
| KPI Card | Single metric with period comparison and conditional color |
| Bar / Stacked Bar | Comparing categories or time-based breakdowns |
| Line Chart | Trends over time; supports dual-axis for secondary metrics |
| Heat Map | Correlation between two dimensions across a value |
| Pivot Table | Cross-tabulation with totals and sub-totals |
| Scatter Plot | Outlier detection across two numeric dimensions |
| Funnel Chart | Conversion rates across ordered stages |

**Calculated Fields:**

QuickSight's expression language supports a broad set of functions:

```
# Revenue growth rate vs. prior period
(sum({revenue}) - sum({revenue_prior})) / sum({revenue_prior}) * 100

# Days since last order (customer segmentation)
dateDiff(parseDate({last_order_date}, 'yyyy-MM-dd'), now(), 'DD')

# Conditional classification
ifelse({margin_pct} >= 0.3, 'High', {margin_pct} >= 0.1, 'Medium', 'Low')
```

**Conditional Formatting**: applied to KPI cards and tables — text color, background color, and icon indicators driven by calculated field thresholds, making anomalies immediately visible without requiring the reader to interpret raw values.

##### Interactive Controls — Parameters and Actions

Static dashboards have limited value for self-service analytics. QuickSight's interactivity features allow users to explore data dynamically:

**Parameters:**
- Named variables that can be bound to filter controls (dropdowns, date pickers, text inputs).
- Drive dynamic titles (e.g., "Sales Report for {selected_region}"), URL actions, and cascading filters.
- Can be passed via URL at embed time for pre-filtered dashboards.

**Filter Actions:**
- Click-to-filter: clicking a bar in a bar chart filters all other visuals on the same sheet by that dimension value.
- Cross-sheet actions: a click event on Sheet 1 navigates to Sheet 2 with pre-applied filters (drill-through pattern).

**URL Actions:**
- On visual element click, open a custom URL with field values interpolated — used to link from the dashboard to source records in an external CRM or ticketing system.

**Drill-Down Hierarchies:**
- Define date hierarchies (Year → Quarter → Month → Day) or geographic hierarchies (Country → Region → City) so users can progressively explore from macro to micro views within a single visual.

##### QuickSight ML Insights

QuickSight embeds machine learning directly into the analytics layer, accessible without any data science expertise:

**Anomaly Detection:**
- Powered by the **Random Cut Forest (RCF)** algorithm running continuously against SPICE data.
- Identifies statistically anomalous data points in time-series metrics (e.g., unusual revenue drop on a Tuesday).
- Configurable sensitivity (1–100) and contribution analysis to show *which dimensions* drove the anomaly.
- Can trigger **QuickSight Alerts** via email when anomalies exceed a configured threshold.

**Forecasting:**
- Applies AWS's proprietary **time-series forecasting model** (similar to DeepAR) to project metrics forward.
- Configurable forecast period (days, weeks, months) and prediction interval (80%, 90%, 95% confidence bands).
- Forecast lines rendered directly on the line chart visual — no export or external tool required.

**Auto-Narratives (Narrative Insights):**
- Automatically generates natural language descriptions of what changed in a dataset.
- Supports customizable narrative templates with dynamic field references.
- Particularly useful for executive summary sheets where non-technical stakeholders need plain-language highlights rather than chart interpretation.

##### Row-Level Security and Column-Level Security

QuickSight provides fine-grained access control at the dataset level, enabling a single dashboard to serve multiple user groups with appropriately restricted data views:

**Row-Level Security (RLS):**

RLS is implemented via a *permission dataset* — a mapping table that associates each QuickSight user or group with the dimension values they are allowed to see:

```
| UserName         | region   | department  |
|------------------|----------|-------------|
| alice@company.com| APAC     | Sales       |
| bob@company.com  | EMEA     | Finance     |
| carol@company.com|          |             |  ← (empty = see all)
```

When alice logs in and opens a dashboard, her view is automatically filtered to `region = 'APAC' AND department = 'Sales'` — no application code required.

**Column-Level Security (CLS):**
- Restrict specific columns (e.g., `salary`, `social_security_number`) from appearing in a dataset for designated user groups.
- Restricted columns are invisible in visuals, filter panels, and export — preventing any indirect access.

**Tag-Based Row Security** (QuickSight + Lake Formation integration): Lake Formation tag-based policies enforced in Athena are respected when QuickSight queries via Athena — providing a consistent, centrally managed access control layer.

##### Embedding QuickSight Dashboards

Registered Embedded dashboards allow QuickSight to be surfaced inside external web or mobile applications, enabling product teams to offer analytics directly within their own user experience:

**Embedding SDK (JavaScript):**

```javascript
import { createEmbeddingContext } from 'amazon-quicksight-embedding-sdk';

const embeddingContext = await createEmbeddingContext();

const frameOptions = {
  url: embeddedDashboardUrl,   // obtained from GetDashboardEmbedUrl API
  container: '#dashboard-container',
  height: '700px',
  width: '100%',
};

const contentOptions = {
  parameters: [
    { Name: 'region', Values: ['APAC'] },  // per-session override
  ],
  locale: 'en-US',
  toolbarOptions: { export: true, undoRedo: false, reset: true },
};

const dashboard = await embeddingContext.embedDashboard(frameOptions, contentOptions);
```

**Embedding modes:**
- **Authenticated (per-user)**: reader logs in via identity provider → QuickSight maps them to a registered user → RLS applies automatically.
- **Anonymous (1-click)**: anonymous URL generated per session by the backend; parameters drive data scoping; no QuickSight user account needed.

**GenerateEmbedUrlForAnonymousUser API**: backend (Lambda or server) calls this API with session tags and allowed parameter values, receives a time-limited embed URL, and returns it to the frontend — keeping AWS credentials entirely server-side.

##### Serverless Analytics Cost Model

Understanding the QuickSight cost model is critical for architecting cost-efficient BI solutions:

| Component | Pricing Model |
|---|---|
| **Authors** | $18/month (standard) or $54/month (enterprise) per author |
| **Readers** | $0.30/session (max $5/month/reader cap) |
| **SPICE** | $0.25/GB/month — first 1 GB per author free |
| **Athena queries** | $5/TB scanned (separate from QuickSight) |
| **Embedding** | Reader sessions billed per 30-min session |

For high-reader-count dashboards, the **Reader capped pricing** ($5/month max per reader) makes QuickSight significantly cheaper than traditional BI tools that charge per named user seat.

##### Hands-On Activities

- Connected QuickSight datasets to the Athena-backed Data Lake from Week 9; configured hourly SPICE refresh and benchmarked Direct Query vs. SPICE response times on 50 M-row datasets.
- Built a 4-sheet executive dashboard: "Overview", "Sales by Region", "Product Performance", and "Trend Analysis" — using 12 visual types with conditional formatting throughout.
- Implemented parameterized filters for Region, Date Range, and Product Category with cascading filter dependencies.
- Enabled Anomaly Detection on the weekly revenue metric and triggered a test alert; validated the ML model identified a seeded anomaly with 94% confidence.
- Set up RLS permission dataset assigning 5 test users to different region/department combinations; verified dashboard renders different row counts per user login.
- Embedded the dashboard into a Next.js application using the Embedding SDK with anonymous session embedding and per-session parameter overrides for department scoping.

##### Event: Cloud Mastery 2026 — #1 AI From Scratch

Attended the first edition of Cloud Mastery 2026 themed around building AI from the ground up, covering three focused sessions: *Building AI Agent with Strands*, *Automated Prompt Engineering: Enhancing LLM Output Quality*, and *AI-Powered Projects*. The sessions provided a practical end-to-end perspective — from architecting agentic workflows with Strands to systematically improving LLM responses through prompt optimization techniques. The final session on AI-Powered Projects demonstrated how these capabilities come together in real-world applications, bridging the gap between prototype and production-ready AI systems.
