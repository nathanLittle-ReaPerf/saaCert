# Database Services Quick Reference

## RDS (Relational Database Service)

### Supported Engines
- **MySQL, PostgreSQL, MariaDB** (open-source)
- **Oracle, SQL Server** (commercial, BYOL or license included)
- **Amazon Aurora** (AWS-proprietary, MySQL/PostgreSQL compatible)

### Multi-AZ vs Read Replicas (CRITICAL TO KNOW!)
| Feature | Multi-AZ | Read Replicas |
|---------|----------|---------------|
| **Purpose** | High availability, disaster recovery | Scalability, read performance |
| **Replication** | Synchronous | Asynchronous |
| **Endpoint** | Single DNS (auto-failover) | Multiple endpoints (one per replica) |
| **Failover time** | 60-120 seconds (automatic) | Manual promotion (~few minutes) |
| **Use case** | Production HA | Read-heavy workloads, reporting, analytics |
| **Write traffic** | Primary only | Primary only (replicas read-only) |
| **Region** | Same region, different AZ | Same region or cross-region |
| **Cost** | Included (pay for instance) | Pay per replica + data transfer |
| **Backup** | Automated from standby | Not used for backups |

**Key Points:**
- **Multi-AZ**: For HA, not performance (standby is NOT accessible)
- **Read Replicas**: For performance, can have up to 5 per primary (15 for Aurora)
- **Promote Read Replica**: Can promote to standalone DB (for DR, breaks replication)

### Backup & Restore
| Feature | Automated Backup | Manual Snapshot |
|---------|------------------|-----------------|
| **Frequency** | Daily + transaction logs | On-demand |
| **Retention** | 0-35 days (default 7) | Indefinite |
| **Performance impact** | Minimal (from standby if Multi-AZ) | May cause I/O suspension (single-AZ) |
| **Deletion** | Deleted when DB deleted | Persists after DB deletion |

**Recovery:**
- **RPO**: 5 minutes (transaction logs backed up every 5 min)
- **Point-in-time restore**: Any time within retention period
- **Restore creates new DB instance** (not in-place)

### RDS Proxy
- **What**: Managed connection pooling for RDS/Aurora
- **Benefits**:
  - Reduces DB connections (Lambda can overwhelm DB)
  - Failover time reduced by 66% (preserves connections)
  - IAM authentication
- **Use case**: Lambda + RDS (many concurrent connections), connection pooling

### Encryption
- **At rest**: AES-256, KMS
- **In transit**: SSL/TLS
- **Note**: Cannot enable encryption on existing DB (must create encrypted snapshot → restore)

### Performance Insights
- **What**: Database performance monitoring and analysis
- **Free**: 7 days of history
- **Paid**: Longer retention
- **Use case**: Identify performance bottlenecks, slow queries

---

## Aurora

### Overview
- **What**: AWS-proprietary DB, MySQL/PostgreSQL compatible
- **Performance**: 5x faster than MySQL, 3x faster than PostgreSQL (AWS claims)
- **Availability**: 6 copies of data across 3 AZs
- **Storage**: Auto-scaling 10 GB → 128 TB
- **Cost**: ~20% more expensive than RDS

### Key Features
| Feature | Description | Benefit |
|---------|-------------|---------|
| **Auto-failover** | <30 seconds | High availability |
| **Read Replicas** | Up to 15 (Aurora Replicas) | Read scaling |
| **Aurora Endpoints** | Writer, Reader, Custom | Simplified connection management |
| **Backtrack** | Rewind DB to any point in time (without restore) | Quick recovery from mistakes |
| **Aurora Serverless** | Auto-scaling, pay per second | Unpredictable workloads, dev/test |
| **Aurora Global Database** | Primary region + up to 5 secondary regions | Global DR, low latency reads |
| **Aurora Multi-Master** | Multiple write nodes | High write availability |

### Aurora Serverless
- **What**: Auto-scaling Aurora, scales based on load
- **Pricing**: Pay per ACU (Aurora Capacity Unit) per second
- **Use case**: Infrequent/unpredictable workloads, dev/test, new apps
- **Limitations**: Cold start latency, limited features vs provisioned

### Aurora Global Database
- **What**: One primary region (read/write) + up to 5 secondary regions (read-only)
- **Replication lag**: <1 second
- **Failover**: Promote secondary to primary in <1 minute (manual)
- **Use case**: Disaster recovery, global applications, low-latency reads worldwide

---

## DynamoDB

### Overview
- **What**: Fully managed NoSQL database (key-value and document)
- **Performance**: Single-digit millisecond latency at any scale
- **Scaling**: Automatic, limitless (scales to millions of requests/sec)
- **Availability**: Multi-AZ, highly available

### Key Concepts
| Concept | Description |
|---------|-------------|
| **Table** | Collection of items (like SQL table) |
| **Item** | Row (max 400 KB per item) |
| **Attribute** | Column (key-value pair) |
| **Partition Key** | Primary key (unique identifier) |
| **Sort Key** | Optional, creates composite primary key (partition + sort) |
| **GSI** (Global Secondary Index) | Query on non-primary key attributes (different partition/sort key) |
| **LSI** (Local Secondary Index) | Alternative sort key (same partition key) |

### Capacity Modes
| Mode | Description | Use Case | Pricing |
|------|-------------|----------|---------|
| **Provisioned** | Set RCU/WCU, auto-scaling available | Predictable traffic | Pay per RCU/WCU |
| **On-Demand** | Pay per request, no planning | Unpredictable traffic, spiky | Pay per request (more expensive) |

**Capacity Units:**
- **RCU** (Read Capacity Unit): 1 strongly consistent read/sec (4 KB) or 2 eventually consistent reads/sec (4 KB)
- **WCU** (Write Capacity Unit): 1 write/sec (1 KB)

### Consistency Models
- **Eventually Consistent**: Default, faster, cheaper (may read stale data)
- **Strongly Consistent**: Always latest data (costs 2x RCUs)
- **Transactional**: ACID transactions (costs 2x RCUs/WCUs)

### DynamoDB Features
| Feature | Purpose |
|---------|---------|
| **DynamoDB Streams** | Capture changes (inserts, updates, deletes), trigger Lambda |
| **DynamoDB Accelerator (DAX)** | In-memory cache, microsecond latency (10x performance) |
| **Global Tables** | Multi-region, multi-active replication (read/write in any region) |
| **Point-in-Time Recovery (PITR)** | Restore to any point in last 35 days |
| **TTL** | Auto-delete expired items (no cost) |
| **Backup & Restore** | On-demand or continuous (PITR) |

### DynamoDB vs RDS
| Feature | DynamoDB | RDS |
|---------|----------|-----|
| **Data model** | NoSQL (key-value, document) | Relational (SQL) |
| **Schema** | Flexible (schemaless) | Fixed schema |
| **Scaling** | Automatic, horizontal | Vertical (bigger instance) + read replicas |
| **Joins** | No | Yes |
| **Queries** | Key-based or scan (inefficient) | Complex SQL queries |
| **Use case** | High-scale, simple queries, low latency | Complex queries, ACID transactions, joins |

---

## ElastiCache

### Overview
- **What**: Fully managed in-memory caching (Redis or Memcached)
- **Use case**: Reduce database load, low-latency access, session storage

### Redis vs Memcached
| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data structures** | Strings, lists, sets, sorted sets, hashes | Strings only |
| **Persistence** | Yes (snapshots, AOF) | No (ephemeral) |
| **Replication** | Multi-AZ, read replicas | No |
| **Backup/Restore** | Yes | No |
| **Pub/Sub** | Yes | No |
| **Lua scripting** | Yes | No |
| **Multi-threading** | No (single-threaded per shard) | Yes |
| **Use case** | Complex data, HA, persistence | Simple cache, multi-core performance |

### Caching Strategies
| Strategy | Description | Use Case |
|----------|-------------|----------|
| **Lazy Loading** | Load data into cache on cache miss | Read-heavy, tolerate stale data |
| **Write-Through** | Update cache when DB updated | Always fresh data, more writes |
| **TTL** | Expire data after time | Prevent stale data (use with Lazy Loading) |

### Redis Features
- **Cluster Mode**: Horizontal scaling (shard data across multiple nodes)
- **Multi-AZ**: Automatic failover
- **Backup & Restore**: RDB snapshots
- **Auth Token**: Password protection
- **Encryption**: At rest and in transit

---

## Other Database Services

### Redshift (Data Warehouse)
- **What**: Petabyte-scale data warehouse for analytics (OLAP)
- **Architecture**: Columnar storage, massively parallel processing (MPP)
- **Use case**: Business intelligence, large-scale analytics, SQL queries on big data
- **Pricing**: Pay per node per hour
- **Redshift Spectrum**: Query S3 data directly (no loading required)
- **vs RDS**: Redshift for analytics (OLAP), RDS for transactions (OLTP)

### DocumentDB
- **What**: Managed MongoDB-compatible document database
- **Use case**: MongoDB workloads, document storage
- **Why not MongoDB on EC2**: Fully managed, scalable, HA

### Neptune
- **What**: Managed graph database
- **Engines**: Gremlin, SPARQL
- **Use case**: Social networks, recommendation engines, fraud detection, knowledge graphs

### QLDB (Quantum Ledger Database)
- **What**: Immutable, cryptographically verifiable ledger
- **Use case**: Audit logs, financial transactions, supply chain (need proof of data integrity)
- **vs Blockchain**: Centralized, no decentralization, simpler

### Timestream
- **What**: Time-series database
- **Use case**: IoT, operational metrics, application monitoring
- **Features**: Auto-scaling, fast queries, data tiering (memory → SSD → S3)

### Keyspaces
- **What**: Managed Apache Cassandra-compatible database
- **Use case**: Cassandra workloads, high write throughput, eventual consistency

### MemoryDB for Redis
- **What**: Redis-compatible, durable in-memory database
- **vs ElastiCache Redis**: MemoryDB is durable (data persists), ElastiCache is cache (may lose data)
- **Use case**: Primary database with microsecond latency

---

## Database Migration

### AWS Database Migration Service (DMS)
- **What**: Migrate databases to AWS (on-prem → AWS, AWS → AWS)
- **Source**: On-prem, EC2, RDS, S3, Azure SQL, etc.
- **Target**: RDS, Aurora, Redshift, DynamoDB, S3, etc.
- **Continuous Replication**: CDC (Change Data Capture) for zero-downtime migration
- **Homogeneous**: Same engine (MySQL → RDS MySQL) - simple migration
- **Heterogeneous**: Different engines (Oracle → Aurora PostgreSQL) - use SCT + DMS

### Schema Conversion Tool (SCT)
- **What**: Convert database schema from one engine to another
- **Use case**: Oracle → PostgreSQL, SQL Server → MySQL, etc.
- **Output**: Converted schema + assessment report (identifies manual changes needed)

---

## Exam Pattern Recognition

### "High availability, automatic failover" → **RDS Multi-AZ** or **Aurora**

### "Read-heavy workload, scale reads" → **Read Replicas** or **ElastiCache**

### "Disaster recovery, cross-region" → **Aurora Global Database** or **RDS Read Replica** (promote)

### "Infrequent/unpredictable workload, minimize cost" → **Aurora Serverless** or **DynamoDB On-Demand**

### "Millions of requests/sec, single-digit ms latency" → **DynamoDB** or **ElastiCache**

### "Complex SQL queries, joins, transactions" → **RDS** or **Aurora**

### "Simple key-value, massive scale" → **DynamoDB**

### "Analytics, data warehouse, petabyte scale" → **Redshift**

### "Cache to reduce DB load" → **ElastiCache** or **DAX** (for DynamoDB)
- **ElastiCache Redis**: General caching, HA, persistence
- **ElastiCache Memcached**: Simple cache, multi-threaded
- **DAX**: DynamoDB-specific, microsecond latency

### "Graph database, social network" → **Neptune**

### "Time-series data, IoT metrics" → **Timestream**

### "Immutable audit log, data integrity" → **QLDB**

### "MongoDB workload" → **DocumentDB**

### "Migrate database to AWS, zero downtime" → **DMS** (with CDC)

### "Convert Oracle to PostgreSQL" → **SCT** + **DMS**
