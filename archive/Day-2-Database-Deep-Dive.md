# Day 2: Database Fundamentals - Deep Dive

**Date:** Monday, December 1, 2025, 4:10 PM
**Topics:** RDS, DynamoDB, Aurora, ElastiCache
**Goal:** Master database selection patterns and architectural decisions

---

## 🎯 Learning Objectives

By the end of this session, you will:
1. Know EXACTLY when to choose RDS vs DynamoDB vs Aurora
2. Understand Multi-AZ vs Read Replicas inside and out
3. Master DynamoDB capacity modes (reinforcing today's learning!)
4. Clarify ElastiCache failover strategies (fixing retake Q9 confusion)
5. Recognize database patterns from exam keywords

---

## Part 1: RDS - The Foundation

### Multi-AZ vs Read Replicas (CRITICAL EXAM PATTERN)

This is THE most tested RDS concept. Let's nail it.

#### Decision Tree

```
Need high availability / automatic failover?
└─ YES → **RDS MULTI-AZ**
   - Synchronous replication to standby (different AZ, same region)
   - Automatic failover in 60-120 seconds
   - Single DNS endpoint (automatic routing)
   - Standby is NOT accessible (no read traffic)
   - Use case: Production databases that can't go down

Need to scale read traffic?
└─ YES → **READ REPLICAS**
   - Asynchronous replication
   - Up to 5 replicas per primary (15 for Aurora)
   - Each replica has own endpoint
   - Manual promotion to standalone DB
   - Can be cross-region
   - Use case: Read-heavy workloads, reporting, analytics
```

#### Exam Keywords

| **Keyword in Question** | **Answer** |
|------------------------|-----------|
| "automatic failover" | Multi-AZ |
| "read-heavy workload" | Read Replicas |
| "scale read performance" | Read Replicas |
| "disaster recovery, same region" | Multi-AZ |
| "disaster recovery, cross-region" | Read Replica (promote) or Aurora Global |
| "high availability" | Multi-AZ |
| "reporting database, don't impact production" | Read Replica |

#### Critical Misunderstanding to Avoid

**WRONG:** "Multi-AZ gives you 2x read capacity"
**RIGHT:** Multi-AZ standby is NOT accessible. It's only for failover.

**WRONG:** "Read Replicas provide automatic failover"
**RIGHT:** Read Replicas require MANUAL promotion. They're for reads, not HA.

---

### RDS Backups: Automated vs Manual Snapshots

#### Comparison Table

| Feature | Automated Backup | Manual Snapshot |
|---------|------------------|-----------------|
| **Trigger** | Daily + transaction logs every 5 min | On-demand (you create it) |
| **Retention** | 0-35 days (0 = disabled) | Indefinite (until you delete) |
| **When DB deleted** | Deleted automatically | **Persists!** |
| **Point-in-time restore** | Yes (to any second within retention) | No (only to snapshot time) |
| **Performance impact** | Minimal (uses standby if Multi-AZ) | May cause I/O pause (single-AZ) |
| **RPO** | 5 minutes (transaction log frequency) | Time since last snapshot |

#### Exam Scenario Recognition

**Scenario:** "Restore database to 2:37 PM yesterday"
**Answer:** Point-in-time restore from automated backups (transaction logs)

**Scenario:** "Database deleted accidentally, need to restore"
**Answer:** Manual snapshot (automated backups deleted with DB!)

**Scenario:** "Need backup that survives DB deletion"
**Answer:** Create manual snapshot before deletion

---

### RDS Encryption

**At rest:** AES-256 with KMS
**In transit:** SSL/TLS
**Critical limitation:** Cannot encrypt existing unencrypted DB!

**To encrypt an unencrypted DB:**
1. Create snapshot of unencrypted DB
2. Copy snapshot with encryption enabled
3. Restore from encrypted snapshot
4. Result: New encrypted DB

**Exam trap:** "Enable encryption on existing RDS" → NOT POSSIBLE directly!

---

## Part 2: Aurora - The Upgrade

### Why Aurora Beats RDS

| Feature | Aurora | RDS MySQL/PostgreSQL |
|---------|--------|---------------------|
| **Performance** | 5x MySQL, 3x PostgreSQL | 1x baseline |
| **Storage** | Auto-scaling 10 GB → 128 TB | Manual scaling, max 64 TB |
| **Replication** | 6 copies across 3 AZs (shared storage) | Multi-AZ = 2 copies |
| **Failover time** | <30 seconds | 60-120 seconds |
| **Read replicas** | Up to 15 Aurora Replicas | Up to 5 |
| **Reader endpoint** | Yes (automatic load balancing) | No (app must route) |
| **Backtrack** | Yes (rewind without restore) | No |
| **Cost** | ~20% more expensive | Baseline |

### Aurora Endpoints (Critical for Exam!)

Aurora provides 3 endpoint types:

1. **Writer Endpoint** (Cluster Endpoint)
   - Points to primary instance
   - Use for: INSERT, UPDATE, DELETE
   - Automatically updated on failover

2. **Reader Endpoint**
   - Load balances across all read replicas
   - Use for: SELECT queries
   - Connection-level load balancing

3. **Custom Endpoint**
   - User-defined subset of instances
   - Use for: Route specific queries to specific instance types

**Exam pattern:** "Distribute read traffic across replicas" → Reader Endpoint

---

### Aurora Serverless

**What it is:** Auto-scaling Aurora, scales compute based on load

**When to use:**
- ✅ Infrequent workloads (dev/test)
- ✅ Unpredictable traffic
- ✅ New applications (unknown load)
- ✅ Want to minimize cost (pay per second)

**When NOT to use:**
- ❌ Predictable steady traffic (provisioned is cheaper)
- ❌ Need millisecond response times (cold start latency)
- ❌ Need all Aurora features (some limitations)

**Pricing:** ACU (Aurora Capacity Units) per second

**Exam keyword:** "Infrequent workload, minimize cost" → Aurora Serverless

---

### Aurora Global Database

**Architecture:** 1 primary region (read/write) + up to 5 secondary regions (read-only)

**Key specs:**
- Replication lag: <1 second
- Failover to secondary: <1 minute (MANUAL promotion)
- Use case: Global apps, DR, low-latency reads worldwide

**Exam scenario:** "Application users in Asia, US, Europe. Need low read latency in all regions."
**Answer:** Aurora Global Database with secondary regions in Asia, Europe

---

## Part 3: DynamoDB - NoSQL Mastery

### Capacity Modes (You Just Learned This!)

Let's reinforce what you learned from today's comprehensive quiz.

#### Decision Matrix

| **Situation** | **Capacity Mode** | **Why** |
|--------------|------------------|---------|
| New app, no historical data | **On-Demand** | Can't predict capacity |
| Traffic varies 100x (100 → 10,000 req/min) | **On-Demand** | Huge variation, hard to provision |
| Steady traffic, can forecast | **Provisioned** | Cheaper for predictable loads |
| Cost optimization, known pattern | **Provisioned** | Pay for capacity, not per request |
| Spiky, unpredictable | **On-Demand** | Scales instantly |

#### Cost Comparison

**On-Demand:**
- Pay per request (per read/write)
- NO planning needed
- More expensive per request
- No minimum cost

**Provisioned:**
- Pay per RCU/WCU (capacity units)
- Need to forecast capacity
- Cheaper per request (if used efficiently)
- Auto Scaling available (but takes time to scale)

**Exam trap:** "Provisioned with Auto Scaling for unpredictable traffic"
**Why wrong:** Auto Scaling reacts to metrics, takes time. For HIGHLY unpredictable or NEW apps, On-Demand is better!

**Today's learning:** You picked "Provisioned with Auto Scaling" for a NEW app with unpredictable traffic. The correct answer was On-Demand because:
- No historical data to configure Auto Scaling
- Traffic varies 100x (too unpredictable)
- On-Demand scales INSTANTLY (Auto Scaling takes minutes)

---

### Primary Keys

DynamoDB has two key types:

#### 1. Partition Key Only (Simple Primary Key)
- **Example:** UserID
- Each item must have unique partition key
- Use when: Simple unique identifier

#### 2. Partition Key + Sort Key (Composite Primary Key)
- **Example:** UserID (partition) + Timestamp (sort)
- Items can have same partition key if sort key differs
- Use when: Multiple items per entity (orders per user, posts per author)

**Query patterns:**
- Partition key only: `UserID = "123"` → Gets ONE item
- Partition + sort key: `UserID = "123" AND Timestamp > yesterday` → Gets MULTIPLE items

---

### Indexes: GSI vs LSI

| Feature | Global Secondary Index (GSI) | Local Secondary Index (LSI) |
|---------|----------------------------|---------------------------|
| **Partition key** | Can be DIFFERENT from table | SAME as table |
| **Sort key** | Can be DIFFERENT from table | DIFFERENT from table |
| **When created** | Anytime (after table creation) | Only at table creation |
| **Capacity** | Own RCU/WCU (separate from table) | Shares table's capacity |
| **Projection** | Choose attributes to include | Choose attributes to include |
| **Limit** | 20 per table | 5 per table |
| **Use case** | Query on completely different attributes | Alternative sort key for same partition |

**Exam keyword:** "Query by attribute that's not the primary key" → **GSI**

**Example:**
- Table: `UserID` (partition key)
- Need to query by `Email` → Create GSI with `Email` as partition key

---

### DynamoDB Features

#### DynamoDB Streams
- **What:** Ordered log of item changes (insert, update, delete)
- **Retention:** 24 hours
- **Use case:** Trigger Lambda on changes, replicate to other table, analytics

**Exam scenario:** "When item added to DynamoDB, send email notification"
**Answer:** DynamoDB Streams + Lambda trigger

#### DAX (DynamoDB Accelerator)
- **What:** In-memory cache for DynamoDB
- **Latency:** Microseconds (vs milliseconds for DynamoDB)
- **Throughput:** 10x improvement
- **Use case:** Read-heavy workloads needing ultra-low latency

**Critical:** DAX is ONLY for DynamoDB! Don't use ElastiCache for DynamoDB (use DAX instead).

**Exam keyword:** "DynamoDB" + "microsecond latency" → **DAX**

---

## Part 4: ElastiCache - Caching Mastery

### Redis vs Memcached

This is highly testable!

| Feature | **Redis** | **Memcached** |
|---------|----------|---------------|
| **Data structures** | Strings, lists, sets, hashes, sorted sets | Strings only |
| **Persistence** | YES (RDB snapshots, AOF) | NO (ephemeral) |
| **Replication** | YES (Multi-AZ, replicas) | NO |
| **Backup** | YES | NO |
| **High availability** | YES (automatic failover) | NO |
| **Pub/Sub** | YES | NO |
| **Sorting/ranking** | YES (sorted sets) | NO |
| **Multi-threaded** | NO (single-threaded per shard) | YES |
| **Use case** | Complex data, HA required, persistence | Simple cache, horizontal scaling, multi-core |

#### Decision Tree

```
Need high availability / persistence?
└─ YES → **Redis**
   - Multi-AZ with automatic failover
   - Backup & restore
   - Data survives node failure

Need complex data structures (lists, sets)?
└─ YES → **Redis**
   - Supports 5+ data types
   - Sorted sets for leaderboards

Just need simple string cache?
└─ Multi-threaded performance important?
   └─ YES → **Memcached**
      - Simpler, faster for simple operations
      - Multi-threaded utilization

Default answer for most exam questions → **Redis** (more features)
```

---

### ElastiCache Failover Strategies (Fixing Your Retake Q9!)

Let's clarify the confusion from your retake quiz Q9:

**Question:** ElastiCache for sessions, need to persist if primary fails, RTO = 5 minutes.
**You answered:** Redis persistence (AOF)
**Correct answer:** Multi-AZ with automatic failover

#### Why Multi-AZ Is Correct

**Multi-AZ with automatic failover:**
- Replicates data to standby replica continuously
- Automatic failover in 1-2 minutes (well within 5-min RTO!)
- No manual intervention
- Data loss = minimal (seconds of recent writes)
- This is for **high availability**

**Redis persistence (RDB/AOF):**
- Saves snapshots to disk
- Requires: Detect failure → Launch new node → Restore from backup
- Takes 10+ minutes typically (exceeds 5-min RTO!)
- Manual process (not automatic)
- This is for **disaster recovery**, not HA

#### The Pattern

| **Requirement** | **Solution** | **RTO** |
|----------------|-------------|---------|
| "Automatic failover" | Multi-AZ | 1-2 minutes |
| "Sessions persist if node fails" + <5 min RTO | Multi-AZ | 1-2 minutes |
| "Disaster recovery, backup" | Redis persistence | 10+ minutes |
| "Data must survive cluster deletion" | Redis persistence | N/A |

**Key insight:** "Persist" in the question means "don't lose sessions," NOT "save to disk." Multi-AZ provides persistence through replication, not backups!

---

### Caching Strategies

#### 1. Lazy Loading (Cache-Aside)

**How it works:**
1. App checks cache
2. Cache hit? Return cached data
3. Cache miss? Fetch from DB → Update cache → Return data

**Pros:**
- Only cache what's needed
- Minimal memory waste

**Cons:**
- Cache misses cause delays (2 round trips: cache + DB)
- Stale data (cache might be outdated)

**Use case:** Read-heavy workloads, tolerate slight staleness

**Exam keyword:** "Cache miss fetches from database" → Lazy Loading

---

#### 2. Write-Through

**How it works:**
1. App writes to DB
2. Simultaneously writes to cache
3. Cache always has latest data

**Pros:**
- Cache is always fresh
- No stale data

**Cons:**
- Write penalty (every write hits cache + DB)
- Cache might store unused data

**Use case:** Write-heavy, need fresh data

---

#### 3. TTL (Time-To-Live)

**How it works:**
- Set expiration time on cached data
- After TTL expires, data is removed
- Next read triggers cache miss → fetch from DB

**Use case:** Combine with Lazy Loading to prevent indefinite staleness

**Best practice:** Lazy Loading + TTL

---

## Part 5: Database Selection Decision Trees

### When to Use Each Database

#### Decision Tree 1: Relational vs NoSQL

```
Need complex SQL queries / JOINs / transactions?
└─ YES → **Relational (RDS/Aurora)**
   │
   ├─ Need 5x MySQL performance / Global replication?
   │  └─ YES → **Aurora**
   │
   └─ Standard relational workload?
      └─ YES → **RDS**

Need millions of requests/sec, simple key-value?
└─ YES → **NoSQL (DynamoDB)**
```

#### Decision Tree 2: Caching

```
Need to reduce database load?
└─ YES → **Caching**
   │
   ├─ Backend is DynamoDB?
   │  └─ YES → **DAX**
   │
   └─ Backend is RDS/Aurora/other?
      └─ YES → **ElastiCache**
         │
         ├─ Need HA / persistence / complex data?
         │  └─ YES → **Redis**
         │
         └─ Simple cache / multi-threaded?
            └─ YES → **Memcached**
```

#### Decision Tree 3: Aurora Features

```
Need database for production?
└─ Using MySQL/PostgreSQL?
   └─ YES → Consider **Aurora**
      │
      ├─ Unpredictable/infrequent workload?
      │  └─ YES → **Aurora Serverless**
      │
      ├─ Global application, need low latency worldwide?
      │  └─ YES → **Aurora Global Database**
      │
      └─ Standard high-performance relational?
         └─ YES → **Aurora (standard)**
```

---

## Part 6: Exam Keyword Patterns

### Pattern Recognition Table

| **Keywords in Question** | **Answer** |
|------------------------|-----------|
| "automatic failover" + "RDS" | RDS Multi-AZ |
| "read-heavy" + "scale reads" | Read Replicas |
| "DynamoDB" + "microsecond latency" | DAX |
| "cache" + "HA" + "complex data" | ElastiCache Redis |
| "cache" + "simple strings" + "multi-threaded" | ElastiCache Memcached |
| "millions of requests/sec" | DynamoDB |
| "complex SQL" + "joins" | RDS or Aurora |
| "global app" + "low latency reads" | Aurora Global Database |
| "unpredictable workload" + "new app" | DynamoDB On-Demand or Aurora Serverless |
| "infrequent workload" + "minimize cost" | Aurora Serverless or DynamoDB On-Demand |
| "5x MySQL performance" | Aurora |
| "session storage" + "<5 min failover" | ElastiCache Redis Multi-AZ |
| "analytics" + "petabyte scale" | Redshift (not covered in depth today) |

---

## Part 7: Common Exam Traps

### Trap 1: "Multi-AZ for read scaling"
**WRONG!** Multi-AZ standby is not accessible. Use Read Replicas for read scaling.

### Trap 2: "Read Replicas for automatic failover"
**WRONG!** Read Replicas require manual promotion. Use Multi-AZ for automatic failover.

### Trap 3: "Enable encryption on existing RDS"
**WRONG!** Must snapshot → copy with encryption → restore.

### Trap 4: "ElastiCache for DynamoDB caching"
**WRONG!** Use DAX (DynamoDB Accelerator), not ElastiCache.

### Trap 5: "Provisioned capacity for unpredictable NEW app"
**WRONG!** Use On-Demand for new apps with no historical data.

### Trap 6: "Redis persistence for <5 min RTO"
**WRONG!** Use Multi-AZ for quick failover. Persistence is for disaster recovery (longer RTO).

### Trap 7: "Aurora is always cheaper than RDS"
**WRONG!** Aurora is ~20% more expensive but offers better performance and features.

---

## Part 8: Quick Review Questions

Test your understanding:

### Q1: What's the failover time for RDS Multi-AZ?
**Answer:** 60-120 seconds (automatic)

### Q2: What's the failover time for Aurora Multi-AZ?
**Answer:** <30 seconds (automatic)

### Q3: How many read replicas can RDS have?
**Answer:** Up to 5

### Q4: How many read replicas can Aurora have?
**Answer:** Up to 15 (Aurora Replicas)

### Q5: What's the maximum DynamoDB item size?
**Answer:** 400 KB

### Q6: What's the difference between RCU and WCU?
**Answer:**
- RCU: 1 strongly consistent 4 KB read/sec (or 2 eventually consistent)
- WCU: 1 write of 1 KB/sec

### Q7: When do you use DynamoDB On-Demand?
**Answer:** Unpredictable traffic, new apps, no historical data, highly variable loads

### Q8: When do you use DAX?
**Answer:** DynamoDB workloads needing microsecond latency (10x faster than DynamoDB's milliseconds)

### Q9: What's the difference between Redis persistence and Multi-AZ?
**Answer:**
- Multi-AZ: High availability, automatic failover, 1-2 min RTO
- Persistence: Disaster recovery, backup to disk, 10+ min RTO

### Q10: What endpoint do you use for Aurora read queries?
**Answer:** Reader Endpoint (automatically load balances across read replicas)

---

## Summary: Critical Takeaways

### Must-Know Patterns

1. **RDS Multi-AZ** = Automatic failover (60-120 sec), HA, same region
2. **Read Replicas** = Read scaling, manual promotion, can be cross-region
3. **Aurora** = 5x MySQL performance, <30 sec failover, auto-scaling storage
4. **DynamoDB On-Demand** = Unpredictable/new apps, pay per request
5. **DynamoDB Provisioned** = Predictable traffic, cheaper per request
6. **DAX** = DynamoDB caching only, microsecond latency
7. **ElastiCache Redis** = HA, persistence, complex data, Multi-AZ
8. **ElastiCache Memcached** = Simple cache, multi-threaded, no HA
9. **Redis Multi-AZ** = <5 min RTO, automatic failover
10. **Redis Persistence** = DR/backup, 10+ min RTO, manual restore

### Fixed Your Weaknesses

✅ **DynamoDB Capacity Modes** - You now understand On-Demand vs Provisioned
✅ **ElastiCache Failover** - You now understand Multi-AZ (HA) vs Persistence (DR)

---

## Next Steps

You just absorbed a TON of database knowledge. Time to validate with a 10-question quiz!

**Ready for the Day 2 Database Quiz?**
- 10 questions
- Target: 8/10 (80%)
- Topics: RDS, Aurora, DynamoDB, ElastiCache
- Focus: Patterns and decision trees you just learned

Let me know when you're ready!
