# AWS SAA-C03 Master Decision Trees - All Services

**Purpose:** Consolidated decision trees for all AWS services to quickly identify the correct service for exam scenarios

**Created:** January 28, 2026
**Last Updated:** January 28, 2026
**Exam Date:** March 2, 2026 at 5:15 PM EST

---

## 📋 Table of Contents

1. [Database Selection Decision Trees](#database-selection-decision-trees)
2. [Neptune vs Other Databases](#neptune-vs-other-databases)
3. [DynamoDB Access Patterns](#dynamodb-access-patterns)
4. [RDS & Aurora Decision Trees](#rds--aurora-decision-trees)
5. [Specialized Databases](#specialized-databases)
6. [Caching Decision Trees](#caching-decision-trees)
7. [Analytics: OLAP vs OLTP](#analytics-olap-vs-oltp)
8. [Quick Reference Matrix](#quick-reference-matrix)

---

# Database Selection Decision Trees

## Master Database Selection Tree

```
START: What is the primary use case?

├─ Time-series data (IoT, metrics, events with timestamps)?
│  └─ YES → Amazon Timestream ⏱️
│      └─ Keywords: "time-series", "IoT sensors", "average over last 7 days", "high ingestion rate"
│
├─ Graph relationships (social networks, fraud detection, connections)?
│  └─ YES → Amazon Neptune 🕸️
│      └─ Keywords: "degrees of separation", "fraud rings", "mutual connections", "shortest path"
│
├─ Leaderboards, rankings, sorted data by score?
│  └─ YES → Redis Sorted Sets (ElastiCache) 🏆
│      └─ Keywords: "leaderboard", "top N", "ranking", "real-time scoring"
│
├─ Immutable audit logs with cryptographic verification?
│  └─ YES → Amazon QLDB 📜
│      └─ Keywords: "immutable", "tamper-proof", "audit log", "cryptographically verifiable"
│
├─ MongoDB application migration with minimal code changes?
│  └─ YES → Amazon DocumentDB 📄
│      └─ Keywords: "MongoDB compatibility", "minimal code changes", "document database"
│
├─ Complex analytics with JOINs, aggregations, BI reports?
│  └─ YES → Is this real-time or batch?
│      ├─ Real-time operational queries → RDS/Aurora 🗄️
│      └─ Batch analytics, historical analysis → Amazon Redshift 📊
│          └─ Keywords: "data warehouse", "OLAP", "nightly ETL", "BI reports"
│
├─ Caching read-heavy database workloads?
│  └─ YES → ElastiCache (Redis or Memcached) 💾
│      └─ Keywords: "reduce database load", "cache", "80% identical reads"
│
├─ Relational data with ACID transactions?
│  └─ YES → RDS or Aurora 🗄️
│      └─ Keywords: "SQL", "relational", "foreign keys", "transactions"
│
└─ Key-value lookups, flexible schema, NoSQL OLTP?
   └─ YES → DynamoDB 🗃️
       └─ Keywords: "key-value", "NoSQL", "flexible schema", "single-digit millisecond"
```

---

# Neptune vs Other Databases

## 🚨 Critical: When to Use Neptune vs When NOT to Use Neptune

### Decision Tree: Neptune or Something Else?

```
Is this a relationship/network problem?
│
├─ YES → Continue to Neptune evaluation ↓
│
└─ NO → NOT Neptune
    ├─ Time-series data? → Timestream
    ├─ Batch analytics? → Athena/Redshift
    ├─ Key-value lookups? → DynamoDB
    └─ Simple entity tracking? → DynamoDB

↓ (If YES to relationship problem)

What type of queries are needed?
│
├─ Real-time relationship traversal?
│  ├─ Multi-hop queries ("friends of friends")? → Neptune ✓
│  ├─ Shortest path algorithms? → Neptune ✓
│  ├─ Impact analysis ("what depends on this")? → Neptune ✓
│  └─ Social graph queries ("mutual connections")? → Neptune ✓
│
├─ Batch aggregation on relationships?
│  ├─ "How many times do products appear together?" → Athena (SQL aggregation) ✗
│  ├─ "Weekly correlation reports" → Athena/Redshift ✗
│  └─ "Historical pattern analysis" → Redshift ✗
│
└─ Scale considerations
   ├─ < 1M users + real-time recommendations? → Neptune ✓
   ├─ 10M+ users + sub-200ms SLA? → Pre-compute + DynamoDB ✗
   └─ 50M+ users + complex traversal? → Pre-compute + DynamoDB ✗
```

### Neptune Scale Limitations ⚠️

**Neptune works well when:**
- ✅ < 1M users for real-time recommendations
- ✅ Social graph queries (friends, connections)
- ✅ Relationship traversal is THE core feature
- ✅ Query latency can be 1-3 seconds
- ✅ Real-time impact analysis
- ✅ Fraud ring detection

**Neptune is TOO SLOW when:**
- ❌ 50M+ users with 200ms latency requirement
- ❌ Real-time collaborative filtering at scale
- ❌ Massive scale recommendations (use pre-computed + DynamoDB)

### Decision: Neptune vs DynamoDB for Recommendations

```
Recommendation Engine Requirements
│
├─ Is this social/relationship-based? ("Friends who liked X")
│  └─ YES → Consider Neptune
│      ├─ < 1M users + Complex relationships? → Neptune ✓
│      └─ > 10M users + <200ms SLA? → Pre-compute + DynamoDB ✓
│
└─ Is this item-based? ("Users who watched X also watched Y")
   └─ YES → Pre-compute similarities → DynamoDB ✓
       └─ Pattern: Batch calculate offline, serve fast from DynamoDB
```

### Decision: Graph Routing vs Geospatial Routing

```
"Shortest Route" Problem
│
├─ Physical/geographic routing?
│  ├─ Delivery routes (warehouse → customer) → Amazon Location Service + DynamoDB ✓
│  ├─ Flight paths (airport connections) → Routing API + DynamoDB ✓
│  └─ Road networks → Google Maps API + DynamoDB ✓
│      └─ Pattern: Geospatial routing uses GPS/maps, NOT graph database
│
└─ Data relationship routing?
   ├─ Social connections (user → friends) → Neptune ✓
   ├─ Org hierarchy (employee → manager → CEO) → Neptune ✓
   └─ Supply chain tiers (material → component → product) → Neptune ✓
       └─ Pattern: Relationship traversal through data connections
```

### Decision: Graph Traversal vs Batch Analytics

```
"Products Bought Together" Analysis
│
├─ Real-time recommendations for users?
│  └─ YES → Pre-compute + DynamoDB (or Neptune if <1M users)
│      └─ Pattern: Calculate offline, serve fast
│
└─ Batch reports on historical data?
   ├─ Data in S3? → Athena ✓
   ├─ Data warehouse needed? → Redshift ✓
   └─ Weekly/monthly reports? → Athena (most cost-effective) ✓
       └─ Pattern: This is COUNT/GROUP BY (SQL), not graph traversal
```

### Neptune Perfect Use Cases ✅

**1. Social Networks**
- "Find mutual friends between User A and User B"
- "People you may know within 3 degrees"
- "Show friend-of-friend connections"
- Keywords: degrees of separation, mutual connections, social graph

**2. Fraud Detection**
- "Identify fraud rings (connected accounts)"
- "Detect related suspicious transactions"
- "Find patterns in network of fraudsters"
- Keywords: fraud rings, relationship patterns, network analysis

**3. Threat Intelligence**
- "Trace malware propagation path"
- "Find all domains linked to this IP"
- "Visualize attack networks"
- Keywords: propagation path, network visualization, connected threats

**4. Family Trees / Genealogy**
- "Find all descendants within 5 generations"
- "Identify common ancestors between two people"
- "Show all marriage connections in family branch"
- Keywords: multi-generational, ancestors, descendants

**5. Infrastructure Dependencies**
- "If we shut down this database, what's affected?"
- "Show dependency chain for this service"
- "Impact analysis for infrastructure changes"
- Keywords: dependency tracking, impact analysis, upstream/downstream

**6. Knowledge Graphs**
- "Find concepts related to this topic"
- "Show connections between entities"
- "Navigate linked information"
- Keywords: knowledge graph, linked concepts, entity relationships

### Neptune WRONG Use Cases ❌

**1. Batch Analytics on Historical Data**
- ❌ "Weekly reports on product purchase patterns" → Use Athena
- ❌ "Correlation analysis on 10TB S3 data" → Use Athena/Redshift
- ❌ "Monthly aggregate statistics" → Use Redshift
- Pattern: Aggregation (COUNT, AVG) ≠ Graph traversal

**2. High-Scale Recommendations (50M+ users)**
- ❌ "50M users, 200ms SLA, collaborative filtering" → Use DynamoDB
- ❌ "Real-time recommendations at massive scale" → Pre-compute + DynamoDB
- Pattern: Neptune too slow for real-time traversal at this scale

**3. Geospatial Routing**
- ❌ "Shortest delivery route from warehouse to customer" → Location Service
- ❌ "Optimize vehicle routing" → Routing APIs + DynamoDB
- Pattern: Physical routing uses GPS/maps, not graph DB

**4. Simple Entity Tracking**
- ❌ "Track packages on vehicles" → DynamoDB
- ❌ "Current location of delivery" → DynamoDB
- Pattern: Simple lookups don't need graph database

**5. Time-Series Analytics**
- ❌ "IoT sensor metrics over time" → Timestream
- ❌ "Event tracking with timestamps" → Timestream
- Pattern: Timestamps + aggregations = Timestream, not Neptune

---

# DynamoDB Access Patterns

## 🚨 STOP! Before Answering ANY DynamoDB Question:

### Step 1: Can I Use Query?

```
Do I know the partition key value?
│
├─ YES → Query (if querying ONE partition) ✓
│   └─ Single-digit millisecond latency
│   └─ Most efficient DynamoDB operation
│
└─ NO → Cannot use Query!
   ├─ Need items across ALL partitions?
   │  ├─ Infrequent (<weekly) → Scan
   │  ├─ Frequent (daily+) → GSI with different partition key
   │  └─ Analytics (large %) → S3 Export
   └─ Continue to Step 2 ↓
```

**CRITICAL RULES:**
- ✅ Query = partition key REQUIRED
- ❌ Cannot Query on just sort key
- ❌ Cannot Query across all partitions
- ✅ Query can filter on sort key (with conditions like >, <, BETWEEN, begins_with)

### Step 2: Frequency + Selectivity Decision

```
What % of table do I need?
│
├─ <5% (Selective - small portion)
│  │
│  └─ How often?
│     ├─ Multiple times per day → GSI ($2-10/month) ✓
│     ├─ Daily → GSI ($2-10/month) ✓
│     ├─ Weekly → GSI ($5-20/month) ✓
│     ├─ Monthly → DO THE MATH! (probably GSI)
│     ├─ Quarterly → Scan ($5-15/quarter) ✓
│     ├─ Yearly → Scan ($5-15/year) ✓
│     └─ One-time/Ad-hoc → Scan (simplest) ✓
│
├─ 5-50% (Medium portion)
│  │
│  └─ How often?
│     ├─ Daily+ → GSI or S3 Export (do math)
│     ├─ Weekly → GSI or S3 Export (do math)
│     ├─ Monthly → S3 Export ($20-60/month) ✓
│     └─ Quarterly/Yearly → Scan (if <10B items) ✓
│
└─ >50% (Most/All of table)
   │
   └─ How often?
      ├─ Daily/Weekly → S3 Export + Glue/Athena ✓
      ├─ Monthly → S3 Export + Athena ✓
      ├─ Quarterly → S3 Export OR Scan (do math)
      └─ Yearly → Scan (if <10B items, cost amortizes) ✓
```

### DynamoDB Special Patterns

#### 1. Leaderboard / Global Ranking

```
Need: Top N items across entire table

Solution: GSI with synthetic partition key
- PK = Fixed value (e.g., "GLOBAL" or "DAILY")
- SK = Ranking attribute (score, timestamp)
- Query: PK="GLOBAL", ScanIndexForward=false, Limit=N

❌ DON'T: Use score/timestamp as partition key (hot partition!)
✅ DO: Synthetic partition + score as sort key
```

#### 2. Range Queries (>, <, >=, <=, BETWEEN)

```
Need: Query numeric ranges (price > 100, age >= 21)

Solution: Numeric value as SORT KEY, NOT partition key
- PK = Static value or category (e.g., "HIGH_VALUE")
- SK = Numeric attribute (amount, price, age)
- Query: PK="HIGH_VALUE" AND SK > 100

❌ DON'T: Numeric/boolean as partition key for ranges
✅ DO: Static partition key + numeric sort key
```

#### 3. Many-to-Many Relationships

```
Example: Posts ↔ Hashtags, Users ↔ Groups

Solution: Denormalize (one item per relationship)
- Base table: Normal item with Set attribute
- GSI: One item per value in the set
  - PK = hashtag (String)
  - SK = timestamp (Number)

❌ DON'T: Use Sets as partition/sort keys (not allowed!)
✅ DO: Create multiple GSI items per base table item
```

### DynamoDB Decision Mantras

```
1. "Query = partition key REQUIRED. No partition key? No Query."

2. "Monthly ≠ Export. Do the math: Frequency × Selectivity = Solution"

3. "Twice per year? Just Scan it. Don't overthink."

4. "Numeric ranges = SORT KEY, never partition key"

5. "Many-to-many = denormalize. One item per relationship."

6. "Sets are attributes, not keys."

7. "Production isolation? S3 Export (no RCUs consumed)."
```

---

# RDS & Aurora Decision Trees

## Aurora Serverless v1 vs v2

```
Does workload need to scale to ZERO (0 ACUs) for cost savings?
│
├─ YES → Aurora Serverless v1 ✓
│   └─ Use Cases:
│       • Infrequent workloads (dev/test, side projects)
│       • Unpredictable, sporadic traffic
│       • Non-critical apps that can tolerate startup delays
│       • Maximize cost savings (pay only when active)
│   └─ Limitations:
│       • Slower scaling (takes time to wake from zero)
│       • MySQL 5.6/5.7 and PostgreSQL 10.x only
│       • Cannot set max capacity
│
└─ NO → Aurora Serverless v2 ✓
    └─ Use Cases:
        • Production workloads with variable traffic
        • E-commerce with seasonal spikes
        • Apps requiring instant scaling
        • Multi-tenant SaaS applications
        • Does NOT scale to zero (always some capacity)
    └─ Benefits:
        • Instant scaling (sub-second)
        • MySQL 8.0 and PostgreSQL 13+ support
        • Set min/max capacity (e.g., 0.5-16 ACUs)
        • Lower latency, production-ready
```

## Cost Optimization Decision Tree

```
Question asks for "MOST cost-effective" solution?
│
├─ Step 1: Does it MEET requirements?
│   └─ Eliminate any option that fails requirements
│
├─ Step 2: Apply cost optimization hierarchy (cheapest first):
│   1. Use existing resources (Read Replicas, not new clusters)
│   2. Right-size (don't over-provision capacity)
│   3. Serverless (pay only for usage, not idle capacity)
│   4. Reserved Instances (if predictable workload)
│   5. On-Demand (last resort - most expensive)
│
└─ Step 3: Avoid over-engineering:
    ✅ GOOD: Aurora Read Replica for read-heavy analytics
    ❌ BAD: Separate Aurora Serverless v2 cluster for analytics

    ✅ GOOD: Aurora Serverless v1 for dev/test (scales to zero)
    ❌ BAD: Aurora Serverless v2 for dev/test (doesn't scale to zero)

    ✅ GOOD: ElastiCache for read caching only
    ❌ BAD: ElastiCache for write scaling (it doesn't help writes!)
```

## Operational Overhead Hierarchy

```
Question asks for "LEAST operational overhead"?
│
└─ Choose in this order (lowest overhead first):

    1️⃣ FULLY MANAGED AWS SERVICES (zero admin)
       • Aurora Serverless (automatic scaling)
       • DynamoDB (no server management)
       • RDS (automated backups, patching, failover)

    2️⃣ MANAGED CONTAINERS (some admin)
       • AWS Fargate (serverless containers)
       • ECS/EKS with Fargate (no EC2 management)

    3️⃣ SELF-MANAGED ON EC2 (high admin overhead)
       • Oracle on EC2 with Data Guard
       • Self-managed Oracle RAC on EC2
       • MySQL/PostgreSQL on EC2

    ❌ NEVER choose EC2 when managed service exists!
```

## Aurora Cross-Region Replication

```
Need cross-region disaster recovery or read scaling?
│
├─ Replication lag requirement < 1 SECOND?
│   └─ YES → Aurora Global Database ✓
│       • <1 second typical replication lag
│       • Up to 16 Read Replicas per secondary region
│       • Fast failover to secondary region (<1 min RPO)
│       • Best for: Global apps, DR with strict RTO/RPO
│
└─ NO (can tolerate seconds of lag) → Cross-Region Read Replica ✓
    └─ Use Cases:
        • Regional read scaling (not DR)
        • Lower cost than Global Database
        • Simpler setup (just create replica)
        • Async replication (seconds lag acceptable)
```

## Aurora Fast Recovery Decision Tree

```
Database corruption or data error occurred. Need to recover quickly (<10 min)?
│
├─ Know EXACT point in time (specific timestamp)?
│   └─ YES → Aurora Backtrack (MySQL only) ✓
│       • Rewind database IN PLACE (no new cluster)
│       • <10 minutes to any point in backtrack window
│       • No data loss, instant rollback
│       • Use Case: "Oops, I ran DELETE without WHERE clause!"
│
├─ Need to TEST recovery before promoting?
│   └─ YES → Aurora Clone ✓
│       • Copy-on-write clone (fast, cheap)
│       • Test recovery on clone without affecting prod
│       • Promote clone if recovery successful
│       • Only pay for changed data
│
└─ Need new production cluster from backup?
    └─ Point-In-Time Restore (PITR) ✓
        • Creates NEW cluster from backup
        • Slower than Backtrack (full restore)
        • Any RDS/Aurora database (not just MySQL)
```

---

# Specialized Databases

## Amazon Timestream (Time-Series Database)

### Decision Tree: Timestream vs DynamoDB

```
Does the scenario have time-series analytics needs?
│
├─ NO → NOT Timestream
│   └─ Use DynamoDB, RDS, or other appropriate service
│
└─ YES → Does it ALSO have operational queries requiring <100ms?
    │
    ├─ NO → Timestream alone ✓
    │   └─ If latency can be sub-second
    │
    └─ YES → DynamoDB + Timestream hybrid ✓
        └─ Pattern: DynamoDB Streams → Timestream
        └─ DynamoDB for real-time lookups, Timestream for analytics
```

### Timestream Indicators (Use Timestream When:)

- ✅ High-velocity ingestion (IoT sensors, tick data, logs)
- ✅ Time-series aggregations (avg, moving averages, trends)
- ✅ Time-range queries ("over past 30 days," "hourly averages")
- ✅ Automatic data tiering requirements
- ✅ Keywords: "analyze," "trends," "patterns," "aggregations over time"

### DynamoDB Indicators (NOT Timestream When:)

- ✅ Fast lookups by partition key (user_id, package_id)
- ✅ Timeline/feed queries (sorted by timestamp but accessed by ID)
- ✅ Sub-10ms latency requirements
- ✅ No aggregations needed (just retrieve raw data)
- ✅ Keywords: "retrieve," "fetch," "get," "show last N"

### Timestamp Trap ⚠️

**Having timestamps DOES NOT automatically mean Timestream!**

```
Data has timestamps → What's the access pattern?
│
├─ Time-range analytics? ("average over last 7 days")
│   └─ YES → Timestream ✓
│
└─ Lookup by ID, sorted by time? ("user's last 10 sessions")
    └─ YES → DynamoDB ✓
        └─ Pattern: PK=user_id, SK=timestamp
```

## Amazon QLDB (Quantum Ledger Database)

### When to Use

**Keywords:**
- "Immutable" / "cannot be altered or deleted"
- "Cryptographically verifiable" / "tamper-proof"
- "Audit log" / "audit trail"
- "Regulatory compliance" / "financial transactions"
- "Complete history of changes"
- "Verify data hasn't been tampered with"

**Perfect For:**
- Financial transaction logs
- Supply chain authenticity tracking
- Regulatory compliance records
- Insurance claims history
- Medical records with audit trails

## Amazon DocumentDB (MongoDB Compatibility)

### When to Use

**Keywords:**
- "MongoDB application"
- "MongoDB compatibility"
- "Document database" / "JSON documents"
- "Minimal code changes" / "minimal refactoring"

**Perfect For:**
- Migrating existing MongoDB applications
- Content management systems
- Product catalogs with flexible schemas
- User profiles with nested data

---

# Caching Decision Trees

## ElastiCache: Redis vs Memcached

### Default Choice: Redis

**Choose Redis when:**
- ✅ Need persistence (survive restarts)
- ✅ Need high availability (Multi-AZ)
- ✅ Need replication (read replicas)
- ✅ Complex data structures (sorted sets, lists, hashes)
- ✅ Pub/Sub messaging
- ✅ Leaderboards (sorted sets)
- ✅ Transactions
- ✅ **Default for 99% of use cases**

**Choose Memcached only when:**
- ✅ Pure ephemeral cache (don't care about data loss)
- ✅ Extremely simple key-value only
- ✅ Legacy app already using Memcached
- ✅ Multi-threaded performance critical (though Redis 6.0+ is also multi-threaded)

## ElastiCache Caching Patterns

### 1. Lazy Loading (Cache-Aside) - Most Common

```
When to Use:
├─ ✅ Read-heavy workload (80%+ reads)
├─ ✅ Infrequent writes
├─ ✅ Can tolerate cache misses
└─ ✅ Data doesn't change frequently

How It Works:
1. App checks cache first
2. Cache hit → return cached data
3. Cache miss → query DB, populate cache, return data

Example: "RDS with 80% identical reads, writes only at login/logout"
→ Lazy loading with TTL
```

### 2. Write-Through - Rare

```
When to Use:
├─ ✅ Write-heavy workload
├─ ✅ Reads must always have fresh data
└─ ✅ Can tolerate write latency increase

How It Works:
1. Every write: write to DB AND cache simultaneously
2. On read: check cache (always fresh)

Example: "Financial app where balance updates must be immediately visible"
→ Write-through (but usually lazy loading + cache invalidation is better)
```

### 3. Cache Invalidation - Common Pattern

```
When to Use:
├─ ✅ Data changes infrequently
├─ ✅ Updates must be immediately visible
└─ ✅ Read-heavy workload

How It Works:
1. On write: update database, DELETE cache entry
2. Next read: cache miss → fetch fresh from DB, populate cache

Example: "User balances cached, 99% reads, 1% writes, updates must be visible"
→ Lazy loading + cache invalidation
```

## Redis Architecture Patterns

### Redis Read Replicas - Default Choice

```
When to Use:
├─ ✅ Read-heavy workload (90%+ reads)
├─ ✅ Writes are infrequent
├─ ✅ Need high availability
└─ ✅ Primary at 90% CPU from reads

How It Works:
- Primary handles writes
- Replicas (up to 5) handle reads
- Async replication

Example: "Redis primary at 90% CPU from 100K concurrent reads"
→ Add read replicas (simple, least overhead)

❌ DON'T: Use Cluster mode for 99% read workloads (over-engineering!)
```

### Redis Cluster Mode - Only When Needed

```
When to Use:
├─ ✅ Need horizontal scaling of BOTH reads AND writes
├─ ✅ Very high throughput requirements
└─ ✅ Dataset larger than single node memory

How It Works:
- Data sharded across multiple nodes
- Each shard has primary + replicas
- More complex than read replicas

Example: "High-frequency trading with millions of writes per second"
→ Cluster mode

❌ DON'T use for: 99% read workloads, simple caching
```

---

# Analytics: OLAP vs OLTP

## 🚨 CRITICAL: Real-Time vs Batch Analytics

### Decision Tree: OLAP vs OLTP

```
What type of workload is this?
│
├─ Real-time operational queries (<5 sec SLA)?
│  │
│  ├─ Complex relationships? → Neptune ✓
│  ├─ Simple lookups? → DynamoDB ✓
│  ├─ Transactional (ACID)? → RDS/Aurora ✓
│  └─ Time-series? → Timestream ✓
│      └─ OLTP = Operational databases
│
└─ Batch analytics (reports, aggregates)?
   │
   ├─ Data in S3? → Athena ✓
   ├─ Large BI team, frequent queries? → Redshift ✓
   ├─ Weekly/monthly reports only? → Athena ✓
   └─ Complex ML pipelines? → EMR ✓
       └─ OLAP = Analytical databases
```

### OLAP vs OLTP Indicator Matrix

| Indicator | OLAP (Redshift/Athena) | OLTP (Neptune/DynamoDB/RDS) |
|-----------|------------------------|----------------------------|
| **Query timing** | "Weekly reports", "Daily batch", "Nightly ETL" | "Real-time", "2-second SLA", "<100ms" |
| **Data freshness** | "Hourly refresh", "Nightly load" | "Up-to-date", "As it happens", "Current" |
| **Query type** | Aggregates (SUM, AVG), trends, BI | Lookups, transactions, traversal |
| **Users** | Data analysts, BI team, executives | Application users, customers, researchers |
| **Keywords** | "Historical", "trend analysis", "BI dashboard" | "Patient matching", "customer lookup", "real-time" |

### Redshift RED FLAGS ⚠️

**DO NOT use Redshift when you see:**
- ❌ "Real-time queries"
- ❌ "2-second SLA"
- ❌ "Sub-second response time"
- ❌ "User-facing operational queries"
- ❌ "Researchers querying right now"
- ❌ "Patient matching in real-time"
- ❌ "Hourly refresh" + "real-time requirements" (contradiction!)

### Redshift GREEN FLAGS ✅

**Use Redshift when you see:**
- ✅ "Weekly BI reports"
- ✅ "Historical trend analysis"
- ✅ "Data warehouse"
- ✅ "Batch processing acceptable"
- ✅ "Nightly ETL jobs"
- ✅ "Complex JOINs across billions of rows"
- ✅ "Executive dashboards updated daily"

### Athena vs Redshift Decision

```
Analytics on S3 data - which to choose?
│
├─ Frequency of queries?
│  ├─ > 200 queries per day → Redshift ✓
│  ├─ < 50 queries per day → Athena ✓
│  └─ In between → Calculate cost ↓
│
├─ Query complexity?
│  ├─ Complex multi-table JOINs → Redshift ✓
│  └─ Simple SELECT, GROUP BY → Athena ✓
│
└─ Team size?
   ├─ Large BI team (10+ analysts) → Redshift ✓
   └─ Small team, infrequent queries → Athena ✓
```

---

# Quick Reference Matrix

## Database Selection by Scenario

| Scenario | Correct Database | NOT This | Why Not? |
|----------|-----------------|----------|----------|
| IoT sensor metrics with timestamp queries | **Timestream** | ❌ DynamoDB | Purpose-built for time-series analytics |
| Gaming leaderboard top 100 players | **Redis Sorted Sets** | ❌ DynamoDB + Lambda | Over-engineering - Redis ZSET solves natively |
| Social network friends-of-friends | **Neptune** | ❌ Redshift Spectrum | Real-time graph queries, not batch analytics |
| 50M users, recommendations, 200ms SLA | **DynamoDB** | ❌ Neptune | Neptune too slow for real-time at this scale |
| Financial audit log (immutable) | **QLDB** | ❌ RDS with backups | Need cryptographic verification |
| Migrate MongoDB app to AWS | **DocumentDB** | ❌ DynamoDB | MongoDB compatibility = minimal code changes |
| Nightly ETL with complex JOINs | **Redshift** | ❌ Athena | Frequent queries favor Redshift |
| Weekly reports on S3 data | **Athena** | ❌ Redshift | Infrequent queries = Athena more cost-effective |
| Cache read-heavy RDS workload | **ElastiCache Redis** | ❌ More read replicas | Cache cheaper and faster than multiple DBs |
| Real-time package event tracking | **Timestream** | ❌ DynamoDB | Time-series with automatic archiving |
| Fraud detection via relationships | **Neptune** | ❌ DynamoDB | Fraud rings = graph traversal |
| Clinical trial patient matching | **Neptune** | ❌ Redshift | Real-time operational queries, not OLAP |
| Product analytics on 10TB S3 historical data | **Athena** | ❌ Neptune | Batch aggregation, not graph traversal |
| Delivery tracking (packages on vehicles) | **DynamoDB** | ❌ Neptune | Simple lookups, geospatial routing |
| Infrastructure dependency mapping | **Neptune** | ❌ DynamoDB | Impact analysis = relationship traversal |

## Exam Keywords → Service Mapping

### Time-Series → Timestream
- "Time-series", "IoT", "metrics", "sensors", "events with timestamps"
- "Average over last 7 days", "hourly aggregations"
- "High ingestion rate (millions per hour)"
- "Automatic archiving/tiering"

### Graph → Neptune
- "Social network", "graph", "relationships", "connections"
- "Degrees of separation", "mutual friends", "shortest path"
- "Fraud rings", "network analysis", "propagation path"
- "Dependency tracking", "impact analysis"

### Leaderboard → Redis Sorted Sets
- "Leaderboard", "ranking", "top N"
- "Sorted by score", "ordered by points"
- "Real-time scoring", "get player rank"

### Immutable Ledger → QLDB
- "Immutable", "cannot be altered", "tamper-proof"
- "Cryptographically verifiable", "audit log"
- "Regulatory compliance", "complete history"

### MongoDB → DocumentDB
- "MongoDB application", "MongoDB compatibility"
- "Document database", "minimal code changes"

### Data Warehouse → Redshift
- "Data warehouse", "BI reports", "OLAP"
- "Complex JOINs", "aggregations (SUM, AVG, GROUP BY)"
- "Nightly ETL", "analytics workload", "historical analysis"

### Serverless Analytics → Athena
- "Query S3 directly", "serverless SQL"
- "Weekly reports", "ad-hoc queries"
- "Pay per query", "MOST cost-effective" + infrequent

### Caching → ElastiCache
- "Reduce database load", "cache frequently accessed data"
- "High read traffic", "read-heavy workload"
- "80% identical reads", "improve read performance"

### Operational OLTP → DynamoDB/RDS/Neptune
- "Real-time queries", "sub-second SLA"
- "User-facing", "operational workload"
- "Current data", "up-to-date"

---

# Common Exam Traps

## ❌ Trap #1: "Relationships" Always = Neptune

**Wrong thinking:** "I see 'products bought together' → Neptune!"

**Right thinking:**
- **Graph traversal** ("show me connections") → Neptune ✓
- **Aggregation** ("count how many times X and Y appear together") → Athena/Redshift ✓

**Pattern:** Analyze whether you need to TRAVERSE relationships or AGGREGATE statistics.

---

## ❌ Trap #2: Scale Ignorance with Neptune

**Wrong thinking:** "50M users need recommendations → Neptune for collaborative filtering!"

**Right thinking:**
- **< 1M users** + social recommendations → Neptune ✓
- **50M users** + 200ms SLA → Pre-compute + DynamoDB ✓

**Pattern:** Neptune has scale limits for real-time queries.

---

## ❌ Trap #3: Redshift for Real-Time Operational Queries

**Wrong thinking:** "Complex queries → Redshift!"

**Right thinking:**
- **Batch analytics** (weekly reports) → Redshift ✓
- **Real-time operational** (patient matching now) → Neptune/DynamoDB/RDS ✓

**Pattern:** OLAP ≠ OLTP. Check query timing requirements.

---

## ❌ Trap #4: Geospatial Routing = Graph Database

**Wrong thinking:** "Shortest route between warehouse and customer → Neptune!"

**Right thinking:**
- **Physical routing** (GPS, roads) → Location Service + DynamoDB ✓
- **Relationship routing** (org chart, supply chain) → Neptune ✓

**Pattern:** Physical distance ≠ relationship distance.

---

## ❌ Trap #5: ElastiCache for Write Scaling

**Wrong thinking:** "Database has high write load → Add ElastiCache!"

**Right thinking:**
- ElastiCache only helps **READS** (caching) ✓
- For write scaling → Use write-optimized DB (DynamoDB) or sharding ✓

**Pattern:** Cache = read optimization only.

---

## ❌ Trap #6: Timestamps = Timestream

**Wrong thinking:** "Data has timestamps → Timestream!"

**Right thinking:**
- **Time-range analytics** ("avg over last 7 days") → Timestream ✓
- **Lookup by ID, sorted by time** ("user's last 10 sessions") → DynamoDB ✓

**Pattern:** Having timestamps ≠ time-series analytics.

---

# Pre-Answer Checklist

Before answering ANY database question:

- [ ] **1. What's the primary use case?**
  - Time-series? Graph? Ledger? Cache? Analytics? OLTP?

- [ ] **2. Is this real-time or batch?**
  - Real-time (<5 sec) → OLTP (Neptune/DynamoDB/RDS)
  - Batch (reports, aggregates) → OLAP (Athena/Redshift)

- [ ] **3. What's the scale?**
  - < 1M → Most databases work
  - 50M+ → Check scale limits (Neptune, pre-compute, etc.)

- [ ] **4. What's the query pattern?**
  - Graph traversal → Neptune
  - Aggregation/analytics → Athena/Redshift
  - Key-value lookup → DynamoDB
  - Relationship-based → Check if real-time or batch

- [ ] **5. What are the keywords?**
  - Match keywords to service (degrees of separation → Neptune)
  - Watch for traps (timestamps ≠ always Timestream)

- [ ] **6. Is there a specialized service?**
  - Don't default to DynamoDB + Lambda
  - Use purpose-built services when they exist

- [ ] **7. What's the cost constraint?**
  - "MOST cost-effective" → Simple > Complex
  - Pre-compute vs real-time calculation
  - Serverless vs persistent infrastructure

---

# Study Strategy

## Memorization Flashcards

Create flashcards for:

1. **Service Trigger Words**
   - Front: "Degrees of separation, fraud rings, mutual connections"
   - Back: Neptune

2. **Scale Limits**
   - Front: "Neptune real-time recommendations scale limit?"
   - Back: < 1M users; 50M+ → pre-compute + DynamoDB

3. **OLAP vs OLTP Red Flags**
   - Front: "Redshift for real-time patient matching?"
   - Back: NO! Redshift = OLAP (batch). Use Neptune (OLTP)

4. **Geospatial vs Graph**
   - Front: "Shortest delivery route warehouse → customer?"
   - Back: Geospatial routing (Location Service), NOT Neptune

5. **Anti-Patterns**
   - Front: "Can ElastiCache solve write scaling?"
   - Back: NO! ElastiCache = reads only

## Pattern Recognition Drills

Practice identifying:
1. Graph traversal vs aggregation analytics
2. Real-time OLTP vs batch OLAP
3. Time-series analytics vs lookup by timestamp
4. Geospatial routing vs relationship routing
5. Scale considerations (Neptune, DynamoDB, pre-compute)

## Decision Tree Practice

For each question:
1. Identify primary use case (time-series? graph? analytics?)
2. Check scale (< 1M? 50M+?)
3. Check timing (real-time? batch?)
4. Match keywords to service
5. Verify no anti-patterns (Redshift for real-time? Neptune for 50M users?)

---

**Last Updated:** January 28, 2026
**Days Until Exam:** 14 days
**Status:** Master reference for all decision trees - review before every quiz

---

**Remember:**
- Pattern match keywords to services
- Check scale limits
- Verify OLAP vs OLTP
- Avoid anti-patterns
- Use specialized services when they exist
- Simple > Complex for cost optimization
