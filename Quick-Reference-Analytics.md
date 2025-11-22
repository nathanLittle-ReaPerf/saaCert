# Analytics & Big Data Services Quick Reference

## Amazon Athena

### Overview
- **What**: Serverless interactive query service for S3 data using SQL
- **No infrastructure**: No servers to manage, pay per query
- **SQL Standard**: ANSI SQL (supports JOINs, window functions, arrays)
- **Data formats**: CSV, JSON, Parquet, ORC, Avro, Apache Logs

### Key Features
| Feature | Description |
|---------|-------------|
| **Pricing** | $5 per TB of data scanned (compressed data = lower cost) |
| **Performance** | Use Parquet/ORC (columnar) for 10x faster queries, 50-90% cost savings |
| **Integration** | Works with Glue Data Catalog for schema management |
| **Federated Query** | Query RDS, Redshift, DynamoDB via Lambda Data Source Connectors |
| **ACID** | Supports ACID transactions with Iceberg tables |

### Cost Optimization
- **Use columnar formats**: Parquet or ORC (scan less data)
- **Partition data**: Partition by date (S3 prefix), scan only relevant partitions
- **Compress data**: GZIP, Snappy (reduce data scanned)
- **Limit scans**: Use `LIMIT`, filter early in WHERE clause

**Example Cost:**
- 1 TB CSV data: $5 per query
- 1 TB Parquet (compressed): $0.50 per query (10x savings)

### Use Cases
- **Ad-hoc queries** on S3 data lakes
- **Log analysis** (CloudTrail, VPC Flow Logs, ALB logs)
- **BI reporting** (integrate with QuickSight)
- **Data exploration** before loading to Redshift

---

## Amazon Redshift

### Overview
- **What**: Petabyte-scale data warehouse for analytics (OLAP)
- **Architecture**: Massively parallel processing (MPP), columnar storage
- **SQL-based**: PostgreSQL-compatible
- **Pricing**: Pay per node per hour

### Cluster Architecture
| Node Type | Description | Use Case |
|-----------|-------------|----------|
| **Leader Node** | Coordinates queries, aggregates results | Free (1 per cluster) |
| **Compute Nodes** | Store data, execute queries | dc2 (SSD), ra3 (managed storage) |

### Redshift Node Types
| Type | Storage | Performance | Use Case |
|------|---------|-------------|----------|
| **dc2** (Dense Compute) | SSD, fixed storage | High performance | <10 TB, compute-intensive |
| **ra3** (Managed Storage) | S3-backed, scalable | Balanced | >10 TB, scale compute/storage independently |

### Key Features
- **Columnar storage**: Store data by column (fast analytics, compression)
- **Massively Parallel Processing (MPP)**: Distributes queries across nodes
- **Distribution styles**: KEY, EVEN, ALL (optimize query performance)
- **Sort keys**: Order data for faster queries
- **Compression**: Automatic compression (reduce storage, faster queries)

### Redshift Spectrum
- **What**: Query S3 data directly from Redshift (no loading required)
- **Use case**: Extend Redshift to exabytes of data in S3
- **Pricing**: $5 per TB scanned (same as Athena)

**Architecture:**
```
Redshift Cluster (hot data, frequently queried)
     ↓ (query extends to)
Redshift Spectrum → S3 Data Lake (cold data, infrequent queries)
```

### Redshift Features
| Feature | Purpose |
|---------|---------|
| **Automatic Snapshots** | Backup every 8 hours or 5 GB change, 1-35 day retention |
| **Manual Snapshots** | On-demand, indefinite retention |
| **Cross-Region Snapshot Copy** | DR, copy snapshots to another region |
| **Concurrency Scaling** | Auto-scale for concurrent queries (pay per second when scaling) |
| **Elastic Resize** | Change cluster size in minutes |
| **Enhanced VPC Routing** | Force all traffic through VPC (use VPC security) |

### Redshift vs Athena
| Feature | Redshift | Athena |
|---------|----------|--------|
| **Infrastructure** | Managed cluster (still servers) | Serverless |
| **Cost** | Per hour (cluster running) | Per query ($5/TB scanned) |
| **Performance** | Faster (dedicated resources, optimized) | Good (slower for complex joins) |
| **Use Case** | Regular, complex queries; BI dashboards | Ad-hoc queries, infrequent analysis |
| **Data Load** | Load data into Redshift | Query S3 directly (no load) |

**Decision Tree:**
- **Frequent queries, complex joins, BI dashboards** → **Redshift**
- **Infrequent ad-hoc queries on S3** → **Athena**
- **Both**: Use Redshift for hot data, Redshift Spectrum for cold data in S3

---

## Amazon Kinesis

### Kinesis Family
| Service | Purpose | Use Case |
|---------|---------|----------|
| **Kinesis Data Streams** | Real-time data streaming, custom processing | Ingest clickstreams, IoT, logs; process with Lambda/applications |
| **Kinesis Data Firehose** | Load streams to destinations (S3, Redshift, OpenSearch) | ETL pipelines, simple data delivery (no custom processing) |
| **Kinesis Data Analytics** | SQL queries on streaming data | Real-time analytics, dashboards, alerts |
| **Kinesis Video Streams** | Ingest video streams | Video analytics, ML inference on video |

### Kinesis Data Streams

**Key Specs:**
- **Shard**: Base unit of capacity
  - **Write**: 1 MB/sec or 1,000 records/sec per shard
  - **Read**: 2 MB/sec per shard (5 reads/sec max with GetRecords API)
- **Retention**: 24 hours (default) to 365 days
- **Ordering**: Per shard (partition key determines shard)

**Producers:**
- Kinesis Producer Library (KPL), AWS SDK, Kinesis Agent

**Consumers:**
- Lambda, Kinesis Client Library (KCL), custom applications

**Scaling:**
- **Shard splitting**: Increase capacity (split 1 shard → 2 shards)
- **Shard merging**: Decrease capacity (merge 2 shards → 1 shard)
- **Enhanced fan-out**: Multiple consumers, each gets 2 MB/sec per shard

**Use Cases:**
- Real-time analytics (clickstream, app metrics)
- Log and event data collection
- IoT telemetry ingestion

### Kinesis Data Firehose

**Key Features:**
- **Fully managed**: No shards to manage (auto-scales)
- **Near real-time**: 60 seconds latency minimum (buffer)
- **Destinations**: S3, Redshift (via S3), OpenSearch, Splunk, HTTP endpoints
- **Transformations**: Lambda functions (transform data before delivery)
- **Compression**: GZIP, Snappy, Zip (for S3)
- **Format conversion**: Convert to Parquet/ORC (for S3)

**Buffering:**
- Delivers when buffer size (MB) or buffer interval (seconds) is reached
- Example: 5 MB or 300 seconds (whichever comes first)

**Use Cases:**
- Load streaming data to S3 (data lake)
- ETL pipelines (transform + load)
- Backup streaming data

**Data Streams vs Firehose:**
| Feature | Data Streams | Firehose |
|---------|--------------|----------|
| **Management** | Manage shards | Fully managed (auto-scale) |
| **Latency** | Real-time (<1 second) | Near real-time (60 sec min) |
| **Processing** | Custom (Lambda, KCL) | Limited (Lambda transformation only) |
| **Destinations** | Any (custom code) | Specific destinations (S3, Redshift, etc.) |
| **Use Case** | Custom processing, real-time | Simple delivery to destinations |

### Kinesis Data Analytics
- **What**: Run SQL queries on streaming data (Data Streams or Firehose)
- **Use case**: Real-time dashboards, metrics, anomaly detection
- **Output**: Send results to Lambda, Firehose, Data Streams

---

## AWS Glue

### Overview
- **What**: Serverless ETL (Extract, Transform, Load) service
- **Purpose**: Prepare data for analytics
- **Components**: Glue Crawler, Glue Data Catalog, Glue Jobs

### Glue Components
| Component | Purpose |
|-----------|---------|
| **Glue Crawler** | Scans data sources (S3, RDS), infers schema, populates Data Catalog |
| **Glue Data Catalog** | Centralized metadata repository (databases, tables, schemas) |
| **Glue Job** | ETL script (PySpark or Scala) to transform data |
| **Glue Trigger** | Schedule or event-based job execution |

### Glue Data Catalog
- **What**: Metadata store for data lake (table definitions, schemas)
- **Integrations**: Used by Athena, Redshift Spectrum, EMR, Glue Jobs
- **Benefit**: Single source of truth for schemas

**Example:**
- Crawler scans S3 bucket with CSV files
- Crawler creates table in Data Catalog (infers schema from CSV)
- Athena queries table using Data Catalog metadata

### Glue Jobs (ETL)
- **Languages**: PySpark (Python + Spark), Scala
- **Execution**: Serverless (auto-provisions Spark environment)
- **DPU (Data Processing Unit)**: Compute capacity (1 DPU = 4 vCPU, 16 GB RAM)
- **Pricing**: Per second (billed per DPU-hour)

**Use Cases:**
- Convert CSV to Parquet (columnar format)
- Clean and deduplicate data
- Join data from multiple sources
- Partition data for Athena queries

### Glue Features
- **Job bookmarks**: Track processed data (avoid reprocessing)
- **Schema evolution**: Handle schema changes over time
- **Encryption**: KMS encryption for Data Catalog and ETL jobs

---

## Amazon EMR (Elastic MapReduce)

### Overview
- **What**: Managed Hadoop framework (Spark, Hive, HBase, Presto, Flink)
- **Use case**: Big data processing, machine learning, log analysis

### EMR Cluster Components
| Component | Description |
|-----------|-------------|
| **Master Node** | Coordinates cluster, manages jobs (NameNode, ResourceManager) |
| **Core Nodes** | Run tasks, store data in HDFS (DataNode, NodeManager) |
| **Task Nodes** | Run tasks only (no HDFS storage), can use Spot Instances |

### EMR Use Cases
- **Big data processing**: Process petabytes of data with Spark
- **Machine learning**: Train models with Spark MLlib
- **Log analysis**: Process log files with Hive or Spark
- **ETL at scale**: Transform massive datasets

### EMR vs Glue
| Feature | EMR | Glue |
|---------|-----|------|
| **Management** | Managed, but you configure cluster | Fully serverless |
| **Flexibility** | Full Hadoop ecosystem (Spark, Hive, HBase, etc.) | Limited to Spark (PySpark) |
| **Cost** | Per hour (cluster running) | Per second (DPU-hour) |
| **Use Case** | Complex big data processing, custom Hadoop apps | Simple ETL, serverless preference |

**Decision:**
- **Complex Hadoop workloads, need full control** → **EMR**
- **Simple ETL, serverless** → **Glue**

### EMR Features
- **EMR Notebooks**: Jupyter notebooks for interactive analysis
- **EMR Managed Scaling**: Auto-scale core/task nodes based on workload
- **Spot Instances**: Use Spot for task nodes (cost savings)
- **EMRFS**: Access S3 as Hadoop file system

---

## Amazon QuickSight

### Overview
- **What**: Serverless BI (Business Intelligence) tool for data visualization
- **Pricing**: Pay per user per month
- **Data sources**: RDS, Aurora, Redshift, Athena, S3, SaaS apps

### Key Features
- **SPICE** (Super-fast, Parallel, In-memory Calculation Engine): In-memory caching for fast queries
- **ML Insights**: Auto-detect anomalies, forecasting
- **Embedded dashboards**: Embed dashboards in web apps
- **Row-level security**: Filter data based on user permissions

### Use Cases
- **Business dashboards**: Sales, marketing, finance dashboards
- **Ad-hoc analysis**: Self-service BI for analysts
- **Embedded analytics**: Embed dashboards in SaaS applications

---

## AWS Lake Formation

### Overview
- **What**: Simplify building, securing, and managing data lakes
- **Purpose**: Centralized governance, fine-grained access control
- **Components**: Data ingestion, catalog, security, access control

### Key Features
| Feature | Purpose |
|---------|---------|
| **Centralized Permissions** | Column-level, row-level security (instead of S3 bucket policies) |
| **Data Catalog** | Uses Glue Data Catalog |
| **Blueprints** | Pre-built templates for data ingestion (S3, RDS, on-prem databases) |
| **Audit Logs** | Track all data access (CloudTrail integration) |

### Lake Formation Benefits
- **Fine-grained access**: Column-level, row-level permissions (better than S3 bucket policies)
- **Single place for permissions**: Manage access for Athena, Redshift Spectrum, EMR, Glue
- **Data lake creation**: Automated ingestion, cataloging, cleaning

**Without Lake Formation:**
- Manage S3 bucket policies, IAM policies separately for each service
- No row-level/column-level security

**With Lake Formation:**
- Centralized permissions (table, column, row-level)
- One place to manage access for all analytics services

---

## Amazon Managed Streaming for Apache Kafka (MSK)

### Overview
- **What**: Managed Apache Kafka service
- **Use case**: Real-time streaming, event-driven architectures

### MSK vs Kinesis Data Streams
| Feature | MSK | Kinesis Data Streams |
|---------|-----|---------------------|
| **Ecosystem** | Apache Kafka (open-source) | AWS proprietary |
| **Portability** | Multi-cloud, on-prem (Kafka-compatible) | AWS only |
| **Management** | Managed (still configure brokers) | Fully managed (no broker management) |
| **Use Case** | Existing Kafka apps, need Kafka ecosystem | AWS-native, simpler streaming |

**When to Use MSK:**
- Migrating from on-prem Kafka
- Need Kafka ecosystem (Kafka Connect, Kafka Streams)
- Multi-cloud strategy

**When to Use Kinesis:**
- AWS-native applications
- Simpler streaming (no Kafka expertise needed)

---

## Amazon OpenSearch Service (formerly Elasticsearch)

### Overview
- **What**: Managed search and analytics engine
- **Use cases**: Log analytics, full-text search, application monitoring

### Key Features
- **Kibana**: Visualization and dashboards (included)
- **OpenSearch Dashboards**: Open-source alternative to Kibana
- **Indexing**: Fast search on structured/unstructured data
- **Aggregations**: Real-time analytics on ingested data

### Common Architecture
```
Application Logs → Kinesis Firehose → OpenSearch → Kibana Dashboards
CloudWatch Logs → Lambda → OpenSearch → Kibana
```

### Use Cases
- **Log analytics**: Centralized logging (application logs, CloudTrail, VPC Flow Logs)
- **Full-text search**: Product catalogs, document search
- **Security analytics**: Analyze security logs for threats

---

## Exam Pattern Recognition

### "Ad-hoc SQL queries on S3 data" → **Athena**
- Cheapest for infrequent queries
- No infrastructure

### "Data warehouse, complex queries, BI dashboards" → **Redshift**
- Better performance for frequent queries
- Dedicated resources

### "Real-time streaming, custom processing" → **Kinesis Data Streams**
- Process events with Lambda or KCL
- Real-time (<1 second latency)

### "Load streaming data to S3/Redshift" → **Kinesis Firehose**
- Fully managed, no custom processing needed
- Near real-time (60 sec latency)

### "ETL, transform data" → **Glue** (serverless) or **EMR** (Hadoop ecosystem)
- Glue for simple ETL
- EMR for complex big data processing

### "Centralized data lake with fine-grained permissions" → **Lake Formation**
- Column-level, row-level security
- Centralized governance

### "BI dashboards, visualizations" → **QuickSight**
- Serverless BI tool
- Pay per user

### "Log analytics, full-text search" → **OpenSearch** (Elasticsearch)
- Search and analyze logs
- Kibana dashboards

### "Kafka workloads" → **MSK** (Managed Kafka)
- Migrate from on-prem Kafka
- Kafka ecosystem needed

---

## Key Exam Tips

### Cost Optimization
- **Athena**: Use Parquet/ORC (10x cheaper than CSV)
- **Redshift**: Reserved Instances for predictable workloads
- **EMR**: Spot Instances for task nodes
- **Kinesis**: Right-size shards, use enhanced fan-out only when needed

### Performance
- **Athena**: Partition data, use columnar formats
- **Redshift**: Distribution keys, sort keys, columnar storage
- **Kinesis**: Increase shards for higher throughput

### Decision Criteria
- **Serverless preference** → Athena, Glue, Firehose
- **Control over infrastructure** → Redshift, EMR, Kinesis Data Streams
- **Open-source ecosystem** → MSK (Kafka), EMR (Hadoop)
- **AWS-native** → Kinesis, Glue, Athena
