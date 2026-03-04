# Redis & AWS ElastiCache - Complete Exam Guide

## What is Redis?

**Redis** = **RE**mote **DI**ctionary **S**erver

Redis is an open-source, in-memory data structure store used as:
- **Database** (in-memory)
- **Cache** (most common use case)
- **Message broker**
- **Session store**

### Why "In-Memory"?

- Data is stored in RAM (not disk) → **EXTREMELY FAST** (sub-millisecond latency)
- Typical response times: **< 1 millisecond**
- Can handle **millions of requests per second**
- Trade-off: RAM is expensive and volatile (data lost if server crashes, unless persistence is enabled)

### What Makes Redis Special?

Unlike simple key-value stores, Redis supports rich data structures:
- **Strings** (simple values)
- **Lists** (ordered collections)
- **Sets** (unordered unique values)
- **Sorted Sets** (sets with scores for ranking)
- **Hashes** (maps/objects)
- **Bitmaps**
- **HyperLogLogs** (probabilistic data structures)
- **Geospatial indexes**

## AWS ElastiCache for Redis

AWS ElastiCache is a fully managed caching service supporting two engines:
1. **Redis** ← The one you'll see most on the exam
2. **Memcached** ← Simpler, older, less features

### ElastiCache for Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Structures** | Complex (lists, sets, sorted sets, hashes) | Simple (strings only) |
| **Persistence** | Yes (snapshots, AOF) | No (purely in-memory) |
| **Replication** | Yes (Multi-AZ with auto-failover) | No |
| **High Availability** | Yes (Cluster Mode, Multi-AZ) | No |
| **Backup & Restore** | Yes | No |
| **Pub/Sub** | Yes | No |
| **Transactions** | Yes | No |
| **Lua Scripting** | Yes | No |
| **Multi-threaded** | No (single-threaded) | Yes |
| **Horizontal Scaling** | Yes (Cluster Mode with sharding) | Yes (simple, add nodes) |
| **Use Case** | Advanced caching, session store, leaderboards, real-time analytics | Simple object caching, horizontal scaling |

**EXAM TIP**: If the question mentions **"high availability"**, **"backup"**, **"persistence"**, **"complex data types"**, or **"pub/sub"** → Choose **Redis**. If it just says "simple distributed caching" → either works, but Redis is usually safer.

---

## ElastiCache for Redis Architecture

### 1. Cluster Mode Disabled (Classic)

```
[Primary Node]  ←---  Read/Write
      ↓ replication
[Replica 1]     ←---  Read-only
[Replica 2]     ←---  Read-only
[Replica 3]     ←---  Read-only
[Replica 4]     ←---  Read-only
```

**Characteristics:**
- **1 primary node** (handles all writes)
- **Up to 5 read replicas** (async replication)
- **Multi-AZ with automatic failover** (if enabled)
- **Vertical scaling**: Can scale up/down node types
- **Horizontal read scaling**: Add replicas for more read capacity
- **Single shard**: All data on one primary + replicas

**Use Case**: Most common setup for general caching, session stores

---

### 2. Cluster Mode Enabled (Clustered)

```
Shard 1: [Primary] → [Replica 1]
Shard 2: [Primary] → [Replica 2]
Shard 3: [Primary] → [Replica 3]
...
Up to 500 shards total
```

**Characteristics:**
- **Multiple shards** (1-500 shards, called "node groups")
- **Data partitioned across shards** (horizontal scaling)
- **Each shard has 1 primary + up to 5 replicas**
- **Multi-AZ for each shard**
- **Scales writes AND reads**
- **Max capacity**: 500 shards × 6 nodes = 3000 nodes

**Use Case**: Massive datasets, high write throughput, need to scale beyond single node limits

**EXAM TIP**: If question says **"dataset larger than single node"** or **"scale write capacity"** → **Cluster Mode Enabled**

---

## Common AWS Exam Scenarios for Redis

### Scenario 1: Database Caching (DAX Alternative)

**Problem**: DynamoDB queries are slow and expensive
**Solution**: Add ElastiCache for Redis in front

```
[Application] → Check Redis Cache → (cache miss) → Query DynamoDB → Store in Redis → Return
              ↓ (cache hit)
              Return from Redis (sub-ms latency)
```

**Why Redis?**: Reduces database load, improves response time, saves money on DynamoDB RCUs

---

### Scenario 2: Session Store for Stateless Applications

**Problem**: Web app running on multiple EC2 instances needs to share session data
**Solution**: Store sessions in ElastiCache for Redis

```
[User] → [ALB] → [EC2-1]
               → [EC2-2]  → All read/write sessions to Redis
               → [EC2-3]
```

**Why Redis?**:
- **Persistence**: Sessions survive server crashes
- **Multi-AZ**: High availability
- **Fast access**: Sub-millisecond latency
- **Data structures**: Can store complex session objects as hashes

**EXAM TIP**: If question says **"web app session store"** or **"stateless architecture"** → ElastiCache for Redis or DynamoDB (Redis is faster, DynamoDB is more durable)

---

### Scenario 3: Real-Time Leaderboards / Gaming

**Problem**: Gaming app needs real-time leaderboard with millions of players
**Solution**: Use Redis **Sorted Sets**

```python
# Add player score
ZADD leaderboard 95000 "player123"

# Get top 10 players
ZREVRANGE leaderboard 0 9 WITHSCORES

# Get player rank
ZREVRANK leaderboard "player123"
```

**Why Redis?**: Sorted Sets provide O(log N) insertion and range queries, perfect for leaderboards

---

### Scenario 4: Pub/Sub for Real-Time Analytics

**Problem**: Multiple services need real-time notifications
**Solution**: Redis Pub/Sub

**Why Redis?**: Built-in pub/sub messaging, low latency

---

## ElastiCache for Redis Security

### Encryption

- **Encryption at rest**: AES-256 encryption for disk snapshots
- **Encryption in-transit**: TLS encryption for data flowing between nodes and clients
- **AUTH token**: Password-based authentication
- **Redis AUTH**: Set password in Redis configuration

**EXAM TIP**: If question asks for **"encrypted cache"** → Enable both at-rest and in-transit encryption

### Network Security

- **VPC only**: ElastiCache runs inside your VPC (not publicly accessible)
- **Security Groups**: Control inbound/outbound traffic
- **No IAM authentication**: Use Redis AUTH tokens instead

---

## ElastiCache for Redis Backup & Recovery

### Backup Options

1. **Automatic Backups** (snapshots)
   - Daily snapshots during backup window
   - Retention: 1-35 days
   - **RDS-like backup behavior**

2. **Manual Snapshots**
   - Taken on-demand
   - Persist until explicitly deleted
   - Can copy to another region

### Restore

- **Create new cluster from snapshot**
- Cannot restore to existing cluster
- Can change node type during restore

**EXAM TIP**: If question asks **"disaster recovery for cache"** → Enable automatic backups with Multi-AZ

---

## Redis Persistence (Important Concept)

Redis supports two persistence mechanisms:

### 1. RDB (Redis Database Backup)
- **Point-in-time snapshots**
- Saves entire dataset to disk at intervals
- **Faster restart** (loads binary file)
- **Risk**: Can lose data between snapshots

### 2. AOF (Append-Only File)
- **Logs every write operation**
- Better durability (can replay writes)
- **Slower restart** (must replay all operations)
- **Larger file size**

**ElastiCache default**: Uses RDB for backups
**EXAM TIP**: You don't control persistence mechanism in ElastiCache (AWS manages it), but understand the concepts

---

## When to Use Redis vs Other AWS Services

| Requirement | Best Service |
|-------------|--------------|
| Database query caching | ElastiCache for Redis or DAX (DynamoDB) |
| Session store (web app) | ElastiCache for Redis, DynamoDB, EFS |
| Simple distributed cache | ElastiCache for Memcached or Redis |
| Real-time leaderboards | ElastiCache for Redis (Sorted Sets) |
| Pub/Sub messaging | ElastiCache for Redis, SNS, SQS |
| Complex data structures | ElastiCache for Redis |
| Sub-millisecond latency | ElastiCache for Redis, DAX, DynamoDB |
| High availability required | ElastiCache for Redis (not Memcached) |
| Durable long-term storage | DynamoDB, RDS (NOT ElastiCache) |
| Shared file storage | EFS, FSx |

---

## Critical Exam Facts to Memorize

### Performance
- **Latency**: Sub-millisecond (< 1 ms)
- **Throughput**: Millions of requests per second

### Scaling
- **Cluster Mode Disabled**: Up to 5 read replicas
- **Cluster Mode Enabled**: Up to 500 shards, 3000 nodes total
- **Node types**: r6g, r5, r4, m6g, m5, m4, t3, t2 (memory-optimized for caching)

### High Availability
- **Multi-AZ with automatic failover**
- **Async replication to replicas**
- **Failover time**: Typically 1-2 minutes

### Backup
- **Automatic daily backups** with 1-35 day retention
- **Manual snapshots** (persist indefinitely)
- **Cross-region snapshot copy** supported

### Security
- **VPC only** (not publicly accessible)
- **Encryption at rest and in-transit**
- **Redis AUTH token** for authentication
- **No IAM authentication** (use AUTH instead)

---

## Exam Pattern Recognition

### Keywords → ElastiCache for Redis

| Question Contains | Answer = ElastiCache for Redis |
|-------------------|-------------------------------|
| "Cache database queries" | ✅ (with lazy loading pattern) |
| "Sub-millisecond latency" | ✅ (or DAX for DynamoDB) |
| "Session store for web app" | ✅ (or DynamoDB) |
| "Reduce database read load" | ✅ (read-through cache) |
| "Real-time leaderboard" | ✅ (Sorted Sets) |
| "In-memory data store" | ✅ |
| "Pub/Sub messaging" | ✅ (or SNS/SQS) |
| "High availability cache" | ✅ Redis (NOT Memcached) |
| "Backup and restore cache" | ✅ Redis (NOT Memcached) |

### Anti-Patterns (NOT Redis)

| Question Contains | Answer ≠ ElastiCache |
|-------------------|---------------------|
| "Long-term durable storage" | Use DynamoDB/RDS/S3 |
| "Relational database" | Use RDS |
| "File system" | Use EFS/FSx |
| "Directly query S3 data" | Use Athena |
| "Serverless cache" | No serverless cache (DAX for DynamoDB only) |

---

## Common Caching Strategies (Important!)

### 1. Lazy Loading (Cache-Aside)
```python
def get_user(user_id):
    # Check cache first
    user = redis.get(f"user:{user_id}")
    if user:
        return user  # Cache hit

    # Cache miss - query database
    user = db.query("SELECT * FROM users WHERE id = ?", user_id)

    # Store in cache for next time
    redis.setex(f"user:{user_id}", 3600, user)  # 1 hour TTL
    return user
```

**Pros**: Only caches requested data, cache stays small
**Cons**: Cache miss penalty (3 round trips: check cache → query DB → write cache)

---

### 2. Write-Through
```python
def update_user(user_id, data):
    # Update database
    db.update("UPDATE users SET ... WHERE id = ?", user_id, data)

    # Update cache immediately
    redis.setex(f"user:{user_id}", 3600, data)
```

**Pros**: Cache is always up-to-date, no stale data
**Cons**: Write penalty (2 writes), caches data that may never be read

---

### 3. Hybrid (Lazy Loading + Write-Through)
Most real-world applications use **both**:
- **Lazy loading** for reads
- **Write-through** for writes

**EXAM TIP**: If question asks **"how to implement cache?"** → Describe lazy loading pattern

---

## Practice Scenario

**Scenario**: A social media company runs a web application on Auto Scaling EC2 instances behind an Application Load Balancer. Users frequently access their profile data, which is stored in an RDS MySQL database. The database is experiencing high read load during peak hours, causing slow response times. The company wants to improve performance while minimizing changes to the application code.

**What should the solutions architect recommend?**

A) Implement ElastiCache for Memcached and use lazy loading to cache profile data
B) Implement ElastiCache for Redis with Multi-AZ and use lazy loading to cache profile data
C) Create RDS read replicas and update the application to query replicas for profile reads
D) Migrate the database to DynamoDB and implement DAX for caching

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- **ElastiCache for Redis**: Provides sub-millisecond latency for cached reads
- **Multi-AZ**: High availability (HA) for production workload
- **Lazy loading**: Only caches frequently accessed data (profile data)
- **Minimal code changes**: Application just adds cache check before database query

**Why A is wrong:**
- Memcached works but lacks Multi-AZ high availability
- No backup/restore capability
- Redis is generally preferred for production caching

**Why C is wrong:**
- Read replicas still query the database (not as fast as cache)
- **Requires application code changes** to route reads to replicas
- More expensive than caching

**Why D is wrong:**
- **Major migration effort** (RDS → DynamoDB is massive change)
- Violates "minimizing changes to application code"
- Overkill for this scenario

**Key Exam Tips:**
- **"High read load on database"** → Add caching layer (ElastiCache or DAX)
- **"Minimize code changes"** → ElastiCache with lazy loading (simple pattern)
- **"Production workload"** → Choose Multi-AZ for HA

</details>

---

## Summary: What You MUST Know for the Exam

1. **Redis = in-memory cache** with sub-millisecond latency
2. **ElastiCache for Redis vs Memcached**: Redis has HA, backups, persistence, complex data types
3. **Cluster Mode Disabled**: 1 primary + up to 5 replicas (most common)
4. **Cluster Mode Enabled**: Up to 500 shards for massive scale
5. **Use cases**: Database caching, session store, leaderboards, pub/sub
6. **Security**: VPC only, AUTH token, encryption at rest/in-transit
7. **Backup**: Automatic daily snapshots (1-35 days), manual snapshots
8. **Caching pattern**: Lazy loading (check cache → miss → query DB → store in cache)
9. **HA**: Multi-AZ with automatic failover (1-2 min failover time)
10. **NOT for**: Long-term storage, file systems, relational queries

---

## Your Action Items

1. ✅ Read this entire guide
2. ⬜ Review **Quick-Reference-Databases.md** for ElastiCache details
3. ⬜ Create 10 practice questions on Redis vs Memcached, caching patterns, and cluster modes
4. ⬜ Memorize the comparison table (Redis vs Memcached)
5. ⬜ Understand lazy loading pattern (will be on exam!)

Now you know what Redis is. Go dominate that exam! 💪
