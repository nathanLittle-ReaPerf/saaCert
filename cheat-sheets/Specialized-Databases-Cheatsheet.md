# AWS Specialized Databases Cheatsheet

**Purpose:** Quick reference for AWS SAA-C03 exam - when to use specialized databases vs general-purpose databases

**Last Updated:** January 27, 2026

---

## 🚨 Critical Rule: Don't Default to DynamoDB + Lambda!

**Before choosing DynamoDB, ask:** "Is there a purpose-built service for this use case?"

---

## Database Selection Decision Tree

```
START: What is the primary use case?

├─ Time-series data (IoT, metrics, events with timestamps)?
│  └─ YES → Amazon Timestream ⏱️
│
├─ Graph relationships (social networks, fraud detection, connections)?
│  └─ YES → Amazon Neptune 🕸️
│
├─ Leaderboards, rankings, sorted data by score?
│  └─ YES → Redis Sorted Sets (ElastiCache) 🏆
│
├─ Immutable audit logs with cryptographic verification?
│  └─ YES → Amazon QLDB 📜
│
├─ MongoDB application migration with minimal code changes?
│  └─ YES → Amazon DocumentDB 📄
│
├─ Complex analytics with JOINs, aggregations, BI reports?
│  └─ YES → Amazon Redshift 📊
│
├─ Caching read-heavy database workloads?
│  └─ YES → ElastiCache (Redis or Memcached) 💾
│
└─ Key-value lookups, flexible schema, OLTP workload?
   └─ YES → DynamoDB (now you can use it!) 🗃️
```

---

## Amazon Timestream (Time-Series Database)

### When to Use ⏱️

**Exam Keywords:**
- "Time-series data"
- "IoT metrics" / "sensor data"
- "Event tracking with timestamps"
- "DevOps monitoring" / "application metrics"
- "Queries like 'average over last 7 days'"
- "High ingestion rate" (millions of events per hour)
- "Automatic data tiering" / "archive old data"

**Perfect For:**
- IoT device metrics (temperature, humidity, power consumption)
- Application monitoring (CPU, memory, request rates)
- Package/shipment tracking with event timestamps
- Financial market data (stock prices over time)
- Fleet management telemetry

**Key Features:**
- ✅ Built-in time-series analytics functions (AVG, INTERPOLATE, SMOOTH)
- ✅ Automatic data tiering (hot → warm → cold → S3)
- ✅ Microsecond timestamp precision
- ✅ Serverless, auto-scales
- ✅ 10x-100x more cost-effective than general databases for time-series
- ✅ Optimized for time-range queries

**When NOT to Use:**
- ❌ Key-value lookups by ID (use DynamoDB)
- ❌ Complex JOINs across multiple tables (use Redshift)
- ❌ Graph relationships (use Neptune)

**Example Scenarios:**
1. "500,000 IoT devices sending temperature readings every 30 seconds, query average temperature by device over last 7 days" → **Timestream**
2. "Package delivery events with microsecond precision, query 'packages delivered in last hour'" → **Timestream**
3. "Application metrics with 10 million events per hour, auto-archive after 90 days" → **Timestream**

---

## Amazon Neptune (Graph Database)

### When to Use 🕸️

**Exam Keywords:**
- "Graph database" / "graph relationships"
- "Social network" / "followers" / "friends"
- "Degrees of separation" / "shortest path"
- "Mutual connections" / "people you may know"
- "Fraud detection" / "relationship patterns"
- "Knowledge graphs" / "recommendations based on connections"
- "Network analysis"

**Perfect For:**
- Social networks (Facebook, LinkedIn style relationships)
- Fraud detection (detecting fraud rings through connected accounts)
- Recommendation engines based on connections
- Knowledge graphs (Wikipedia-style linked concepts)
- Network/IT topology mapping

**Key Features:**
- ✅ Purpose-built for graph traversal queries
- ✅ Gremlin or SPARQL query languages
- ✅ Sub-second queries on billions of relationships
- ✅ Relationships are first-class citizens (not JOINs)
- ✅ Optimized for "friends-of-friends-of-friends" queries
- ✅ Fully managed, automatic backups

**When NOT to Use:**
- ❌ Time-series data (use Timestream)
- ❌ Key-value lookups (use DynamoDB)
- ❌ Analytics with aggregations (use Redshift)

**Example Scenarios:**
1. "Social media app needs to find mutual friends and 'people you may know' within 3 degrees of separation" → **Neptune**
2. "Fraud detection system analyzing relationships between accounts to find fraud rings" → **Neptune**
3. "Find shortest path between two users in a network of 50 million users" → **Neptune**

---

## ElastiCache Redis Sorted Sets (Leaderboards)

### When to Use 🏆

**Exam Keywords:**
- "Leaderboard" / "ranking" / "top N"
- "Real-time scoring" / "gaming scores"
- "Sorted by score" / "ordered by points"
- "Get player rank by username"
- "Range queries by score" (e.g., "scores between 500-1000")

**Perfect For:**
- Gaming leaderboards (top 100 players)
- Real-time voting/polling results
- Rate limiting (sorted by timestamp)
- Priority queues

**Key Commands:**
- `ZADD` - Add/update score for a member
- `ZRANGE` - Get members by rank (e.g., top 100)
- `ZREVRANGE` - Get members by rank in reverse order
- `ZRANGEBYSCORE` - Get members with scores in range
- `ZRANK` - Get rank of a specific member
- `ZSCORE` - Get score of a specific member

**Key Features:**
- ✅ O(log N) performance for all operations
- ✅ Atomic operations (no race conditions)
- ✅ Sub-millisecond latency
- ✅ Native support for sorted data
- ✅ Can handle millions of members

**When NOT to Use:**
- ❌ Need persistence across restarts (Redis has persistence, but ensure it's enabled)
- ❌ Complex analytics (use Redshift)
- ❌ Time-series with automatic archiving (use Timestream)

**Example Scenarios:**
1. "Gaming leaderboard showing top 100 players, instant updates, query player rank by username" → **Redis Sorted Sets**
2. "Real-time voting system displaying rankings by vote count" → **Redis Sorted Sets**

**DON'T DO THIS:** DynamoDB + GSI + Streams + Lambda + S3 for leaderboards (over-engineering disaster!)

---

## Amazon QLDB (Quantum Ledger Database)

### When to Use 📜

**Exam Keywords:**
- "Immutable" / "cannot be altered or deleted"
- "Cryptographically verifiable" / "tamper-proof"
- "Audit log" / "audit trail"
- "Regulatory compliance" / "financial transactions"
- "Complete history of changes"
- "Ledger" / "journal"
- "Verify data hasn't been tampered with"

**Perfect For:**
- Financial transaction logs (banking, cryptocurrency)
- Supply chain tracking (prove authenticity)
- Regulatory compliance records
- Insurance claims history
- Medical records requiring audit trails

**Key Features:**
- ✅ Immutable by design (append-only journal)
- ✅ Cryptographic verification (SHA-256 hashing)
- ✅ Complete sequenced history of all changes
- ✅ PartiQL query language (SQL-compatible)
- ✅ ACID transactions
- ✅ Serverless, fully managed

**When NOT to Use:**
- ❌ Need to delete or update records (use RDS/DynamoDB)
- ❌ High-frequency writes (QLDB has lower throughput)
- ❌ Time-series analytics (use Timestream)

**Example Scenarios:**
1. "Financial services company must maintain immutable audit log of all transactions, auditors need to verify records haven't been altered" → **QLDB**
2. "Supply chain needs cryptographically verifiable tracking of product authenticity" → **QLDB**

---

## Amazon DocumentDB (MongoDB Compatibility)

### When to Use 📄

**Exam Keywords:**
- "MongoDB application"
- "MongoDB compatibility"
- "Document database" / "JSON documents"
- "Minimal code changes" / "minimal refactoring"
- "Fully managed MongoDB"

**Perfect For:**
- Migrating existing MongoDB applications to AWS
- Content management systems
- Catalogs (product catalogs with flexible schemas)
- User profiles with nested/complex data

**Key Features:**
- ✅ MongoDB 3.6, 4.0, 5.0 API compatibility
- ✅ Fully managed (AWS handles backups, patching, scaling)
- ✅ ACID transactions
- ✅ Up to 15 read replicas
- ✅ Storage auto-scales to 128 TB
- ✅ Just change connection string, app code mostly unchanged

**When NOT to Use:**
- ❌ Need 100% MongoDB feature parity (use self-managed on EC2)
- ❌ Simple key-value workload (use DynamoDB)
- ❌ Relational data with complex JOINs (use RDS)

**Example Scenarios:**
1. "Migrate MongoDB application to AWS with minimal code changes and fully managed service" → **DocumentDB**
2. "Need document database with flexible schema and ACID transactions" → **DocumentDB**

---

## Amazon Redshift (Data Warehouse)

### When to Use 📊

**Exam Keywords:**
- "Data warehouse" / "OLAP"
- "Business intelligence" / "BI reports"
- "Complex JOINs" / "aggregations" (SUM, AVG, GROUP BY)
- "Analytics workload" / "analytical queries"
- "Nightly ETL jobs"
- "Queries across billions of rows"
- "Join data from multiple sources"

**Perfect For:**
- Business intelligence dashboards
- Financial reporting
- Sales analytics
- Historical data analysis
- ETL/ELT pipelines

**Key Features:**
- ✅ Columnar storage (optimized for analytics)
- ✅ Massively parallel processing (MPP)
- ✅ Efficient COPY from S3
- ✅ Complex JOINs and aggregations
- ✅ Can query S3 directly (Redshift Spectrum)
- ✅ Can pause cluster when not in use (cost savings)
- ✅ Redshift Serverless option

**When NOT to Use:**
- ❌ Real-time OLTP workload (use RDS/Aurora)
- ❌ Key-value lookups (use DynamoDB)
- ❌ Time-series with high ingestion (use Timestream)
- ❌ Graph relationships (use Neptune)

**Redshift vs Athena Decision:**
- **Frequent queries** (>200 per day) → Redshift (persistent compute)
- **Infrequent queries** (<50 per day) → Athena (pay per query)
- **Complex multi-table JOINs** → Redshift (faster)
- **Simple S3 queries** → Athena (serverless)

**Example Scenarios:**
1. "Nightly ETL jobs with complex JOINs across billions of rows, generate BI reports" → **Redshift**
2. "Business intelligence dashboard querying data from S3, RDS, and other sources" → **Redshift**
3. "Analytics workload with 200+ queries per day on large datasets" → **Redshift**

---

## ElastiCache: Redis vs Memcached

### When to Use ElastiCache 💾

**General Use Case:** Caching frequently accessed data to reduce database load

**Exam Keywords:**
- "Reduce database load"
- "Cache frequently accessed data"
- "Improve read performance"
- "High read traffic" / "read-heavy workload"
- "80% identical reads"

### Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Persistence** | ✅ Yes (AOF, RDB) | ❌ No |
| **Replication** | ✅ Yes | ❌ No |
| **Multi-AZ Failover** | ✅ Yes | ❌ No |
| **Data Structures** | ✅ Complex (sorted sets, lists, sets, hashes) | ❌ Simple key-value only |
| **Pub/Sub** | ✅ Yes | ❌ No |
| **Transactions** | ✅ Yes | ❌ No |
| **Sorted Sets (Leaderboards)** | ✅ Yes | ❌ No |
| **TTL (Automatic Expiration)** | ✅ Yes | ✅ Yes |
| **Multi-threaded** | ✅ Yes (Redis 6.0+) | ✅ Yes |

**Default Choice:** **Redis** (more features, persistence, high availability)

**Choose Memcached only if:**
- Pure ephemeral cache (don't care if data is lost)
- Extremely simple key-value needs
- Legacy application already using Memcached

**In 2026:** Redis is almost always the right choice.

---

## ElastiCache Caching Patterns

### 1. Lazy Loading (Cache-Aside)

**When to Use:**
- ✅ Read-heavy workload with infrequent writes
- ✅ Can tolerate cache misses (slightly slower on first read)
- ✅ Data doesn't change frequently

**How It Works:**
1. Application checks cache first
2. If **cache hit** → return cached data
3. If **cache miss** → query database, populate cache, return data

**Pros:**
- Only cache data that's actually accessed
- Simple to implement
- Works for most scenarios

**Cons:**
- Cache misses add latency (have to query database)
- Stale data possible (cache not immediately updated on writes)

**Example:** "RDS PostgreSQL with 80% identical reads, writes only during login/logout" → **Lazy loading**

---

### 2. Write-Through

**When to Use:**
- ✅ Write-heavy workload
- ✅ Reads must always have fresh data
- ✅ Can tolerate write latency increase

**How It Works:**
1. On every write: write to database AND cache simultaneously
2. On read: check cache (always fresh)

**Pros:**
- Cache always has fresh data
- No stale data issues

**Cons:**
- Adds latency to writes (writing to two places)
- Caches data that may never be read
- More complex

**Example:** "Financial application where balance updates must be immediately visible to all users" → **Write-through** (but see note below)

**Note:** For 99% read + 1% write workloads, lazy loading with cache invalidation is usually better than write-through.

---

### 3. Cache Invalidation

**When to Use:**
- ✅ Data changes infrequently
- ✅ Updates must be immediately visible
- ✅ Read-heavy workload

**How It Works:**
1. On write: update database, then **delete cache entry** (or update it)
2. Next read: cache miss → fetch fresh data from database, populate cache

**Pros:**
- Simple
- Ensures reads get fresh data after updates
- Works well for infrequent writes

**Example:** "User account balances cached, 99% reads, 1% writes, updates must be immediately visible" → **Lazy loading + cache invalidation**

---

## Redis Architecture Patterns

### Redis Read Replicas

**When to Use:**
- ✅ Read-heavy workload (90%+ CPU on primary from reads)
- ✅ Writes are infrequent
- ✅ Need high availability

**How It Works:**
- Primary node handles writes
- Replicas (up to 5) handle reads
- Asynchronous replication from primary to replicas

**Pros:**
- Horizontal scaling of read capacity
- High availability (replica promoted on primary failure)
- Simple, least operational overhead

**Example:** "Redis primary at 90% CPU from 100K concurrent reads, writes only during game start/end" → **Read replicas**

---

### Redis Cluster Mode

**When to Use:**
- ✅ Need horizontal scaling of BOTH reads AND writes
- ✅ Very high throughput requirements
- ✅ Dataset larger than single node memory

**How It Works:**
- Data sharded across multiple nodes
- Each shard has primary + replicas
- More complex than read replicas

**Pros:**
- Scales both reads and writes
- Can handle massive datasets

**Cons:**
- More operational complexity
- Data sharding adds complexity

**Example:** "High-frequency trading platform with millions of writes per second" → **Cluster mode**

**DON'T DO THIS:** Use cluster mode for 99% read workloads (over-engineering!)

---

## Common Anti-Patterns to Avoid

### ❌ Anti-Pattern 1: DynamoDB + Lambda for Leaderboards

**Wrong Approach:**
- DynamoDB with GSI on score
- DynamoDB Streams → Lambda to maintain rankings
- Store rankings in S3

**Why It's Wrong:**
- Over-engineered (4 services vs 1)
- Can't efficiently get player rank by username
- High operational overhead
- Expensive

**Correct Solution:** Redis Sorted Sets (ZADD, ZRANK, ZRANGE)

---

### ❌ Anti-Pattern 2: DynamoDB for Time-Series Analytics

**Wrong Approach:**
- DynamoDB with composite key (deviceId#timestamp)
- Lambda functions for aggregations
- Query device-by-device

**Why It's Wrong:**
- Can't efficiently query "all devices' average over time"
- No built-in time-series functions
- Expensive for high ingestion rates
- Have to build what Timestream already does

**Correct Solution:** Amazon Timestream

---

### ❌ Anti-Pattern 3: Multiple Read Replicas Instead of Cache

**Wrong Approach:**
- "Database is read-heavy, let's add 5 read replicas"
- Route 53 weighted routing to distribute traffic

**Why It's Wrong:**
- Very expensive (5 RDS instances running 24/7)
- Still executing full SQL queries (slower than cache)
- Doesn't actually reduce computational load

**Correct Solution:** Add ElastiCache (single cache node cheaper and faster than 5 RDS replicas)

---

### ❌ Anti-Pattern 4: Redshift Spectrum for Real-Time Queries

**Wrong Approach:**
- Store data in S3 Parquet
- Query with Redshift Spectrum for real-time app queries

**Why It's Wrong:**
- Redshift/Athena are for batch analytics, not real-time
- S3 is immutable (hard to update frequently)
- Slow for real-time use cases

**Correct Solution:** Use appropriate real-time database (DynamoDB, RDS, Neptune, etc.)

---

### ❌ Anti-Pattern 5: Write-Through Caching for Read-Heavy Workload

**Wrong Approach:**
- 99% reads, 1% writes
- Implement write-through caching (write to DB + cache on every write)
- Use Redis Cluster mode for "high availability"

**Why It's Wrong:**
- Write-through is for write-heavy workloads
- Cluster mode is overkill for 1% writes
- Over-engineered solution

**Correct Solution:** Lazy loading with cache invalidation + simple read replicas

---

## Quick Decision Matrix

| Scenario | Database | Not This |
|----------|----------|----------|
| IoT sensor metrics with timestamp queries | **Timestream** | ❌ DynamoDB |
| Gaming leaderboard top 100 players | **Redis Sorted Sets** | ❌ DynamoDB + Lambda |
| Social network friends-of-friends | **Neptune** | ❌ Redshift Spectrum |
| Financial audit log (immutable) | **QLDB** | ❌ RDS with backups |
| Migrate MongoDB app to AWS | **DocumentDB** | ❌ DynamoDB |
| Nightly ETL with complex JOINs | **Redshift** | ❌ Athena (if frequent) |
| Cache read-heavy RDS workload | **ElastiCache Redis** | ❌ More read replicas |
| Real-time package event tracking | **Timestream** | ❌ DynamoDB |
| Fraud detection via relationships | **Neptune** | ❌ DynamoDB |

---

## Exam Keywords → Database Mapping

**Time-Series Keywords → Timestream:**
- Time-series, IoT, metrics, sensors, events with timestamps
- "Average over last 7 days"
- High ingestion rate (millions per hour)
- Automatic archiving/tiering

**Graph Keywords → Neptune:**
- Social network, graph, relationships, connections
- Degrees of separation, mutual friends
- Shortest path, network analysis
- Fraud detection via relationships

**Leaderboard Keywords → Redis Sorted Sets:**
- Leaderboard, ranking, top N
- Sorted by score, ordered by points
- Real-time scoring

**Immutable Ledger Keywords → QLDB:**
- Immutable, cannot be altered
- Cryptographically verifiable, tamper-proof
- Audit log, regulatory compliance
- Complete history of changes

**MongoDB Keywords → DocumentDB:**
- MongoDB application, MongoDB compatibility
- Document database, JSON documents
- Minimal code changes

**Analytics Keywords → Redshift:**
- Data warehouse, BI reports, OLAP
- Complex JOINs, aggregations (SUM, AVG, GROUP BY)
- Nightly ETL, analytics workload

**Caching Keywords → ElastiCache:**
- Reduce database load, cache frequently accessed data
- High read traffic, read-heavy workload
- 80% identical reads

---

## Study Tactics for Exam

1. **Memorize Keywords:** When you see "time-series" → immediately think Timestream
2. **Avoid DynamoDB Default:** Ask "Is there a specialized service?" before choosing DynamoDB + Lambda
3. **Pattern Match:** Graph = Neptune, Leaderboard = Redis ZSET, Audit = QLDB
4. **Don't Over-Engineer:** If Redis Sorted Sets solves it, don't use DynamoDB + Streams + Lambda + S3
5. **Cost = Simple:** "MOST cost-effective" usually means simpler solution (cache vs multiple databases)

---

## Summary: The Golden Rule

**Before answering ANY database question:**

1. Read the keywords carefully (time-series? graph? leaderboard? audit log?)
2. Match keywords to specialized database
3. If no specialized service fits → then use DynamoDB or RDS
4. Don't over-engineer with Lambda + Streams if native solution exists

**Remember:** AWS built specialized databases for a REASON. Use them!

---

**Last Updated:** January 27, 2026
**Created By:** Claude (after you failed the specialized databases quiz 40%)
**Purpose:** Stop defaulting to DynamoDB + Lambda for everything!
