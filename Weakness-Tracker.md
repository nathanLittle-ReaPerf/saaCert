# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** January 6, 2026, 7:40 AM CST (Post Day 2 EC2 Fundamentals - 70%)
**Exam Date:** February 11, 2026 at 5:15 PM EST (36 days remaining)
**Study Period:** January 5 - February 10, 2026 (37 days)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 📊 Day 2 Quiz Results (January 6, 2026)

### Initial EC2 Fundamentals Quiz
**Topic:** EC2 Fundamentals (Instance Types, Placement Groups, Pricing, Storage)
**Score:** 7/10 (70%)
**Status:** ⚠️ BELOW TARGET (Target: 80%) - Critical storage and placement group gaps identified

**Strengths Demonstrated:**
- ✅ Cluster Placement Groups for HPC/low latency (Enhanced Networking)
- ✅ On-Demand pricing for unpredictable workloads
- ✅ Spot Instances for fault-tolerant batch processing (up to 90% discount)
- ✅ EC2 User Data for bootstrap scripts
- ✅ Instance Metadata Service (IMDS) at 169.254.169.254
- ✅ Amazon EFS for shared file storage across multiple AZs
- ✅ io2 Provisioned IOPS for high-performance persistent storage

**Critical Weaknesses Identified:**
- ❌ **Instance Store vs EFS/EBS** - Chose EFS Max I/O for temporary, high-performance storage when Instance Store was correct
- ❌ **EBS Multi-Attach misconception** - Thought Multi-Attach protects against AZ failures (it's single AZ only, NOT a DR solution)
- ❌ **Partition vs Spread Placement Groups** - Chose Spread (max 7/AZ) for Cassandra when Partition (large distributed systems) was correct
- ❌ **RPO/RTO mapping** - Failed to map 15-minute RPO requirement to 15-minute snapshot frequency

---

### Targeted Drill Quiz (Remediation Attempt)
**Score:** 7/10 (70%)
**Status:** ⚠️ BELOW TARGET (Target: 100%) - Partial improvement, but EFS vs Multi-Attach still problematic

**Performance by Weak Area:**
1. **Placement Groups: 3/3 (100%)** ✅ **WEAKNESS ELIMINATED!**
   - Correctly identified Kafka → Partition
   - Correctly identified 12 critical instances (6/AZ) → Spread
   - Correctly identified HPC/MPI → Cluster
   - **Status: FULLY MASTERED** - No further drilling needed

2. **Instance Store (basics): 2/2 (100%)** ✅ **WEAKNESS ELIMINATED!**
   - Correctly identified temporary + highest I/O → Instance Store
   - Correctly identified ML training + regeneratable → Instance Store
   - **Status: FULLY MASTERED** - No further drilling needed

3. **EFS vs Multi-Attach vs S3: 2/5 (40%)** 🚨 **CRITICAL - STILL STRUGGLING!**
   - ❌ Q2: Chose Multi-Attach for multi-AZ concurrent access (should be EFS)
   - ❌ Q4: Chose S3 sync for shared config files (should be EFS for "immediately available")
   - ❌ Q5: Chose EFS for "block storage" requirement (should be Multi-Attach in single AZ)
   - ✅ Q6: Correctly used snapshots for DR (RPO=15min)
   - ✅ Q7: Correctly avoided Multi-Attach for AZ-level protection
   - **Status: NEEDS INTENSIVE DRILLING** - Must achieve 90%+ before proceeding

**Root Cause Analysis:**
- **Pattern #1 not internalized:** "Multi-AZ + concurrent access = EFS (ALWAYS)"
- **Pattern #2 confusion:** Mixing up "block storage" vs "file storage" requirements
- **Pattern #3 missed:** "Immediately available" = real-time access = EFS (not S3 scheduled sync)

**Next Action Required:**
- 🚨 Take another 10-question drill quiz focusing EXCLUSIVELY on EFS vs Multi-Attach scenarios
- 🚨 Create decision tree flashcard
- 🚨 Target: 9/10 or 10/10 (90%+) before moving to Day 3

---

### Second Drill Quiz - EFS vs Multi-Attach Focus
**Score:** 8.5/10 (85%)
**Status:** ✅ **MAJOR IMPROVEMENT - Core patterns MASTERED!**

**Performance by Pattern:**
1. **Multi-AZ + concurrent access = EFS: 5/5 (100%)** ✅ **MASTERED!**
   - Q1: Multi-AZ video editing → EFS ✅
   - Q3: 80 instances, multi-AZ, NFS → EFS ✅
   - Q4: Shared config, immediately available → EFS ✅
   - Q8: 300 instances, multi-AZ logs → EFS ✅
   - Q10: 500 instances, multi-AZ dataset → EFS ✅
   - **This weakness is ELIMINATED** - 100% accuracy achieved

2. **Block-level + single AZ = Multi-Attach: 3/3 (100%)** ✅ **MASTERED!**
   - Q2: Oracle RAC, block-level, single AZ → Multi-Attach ✅
   - Q5: 14 instances, block-level, single AZ → Multi-Attach ✅
   - Q9: SQL Server, block-level, single AZ → Multi-Attach ✅
   - **This weakness is ELIMINATED** - 100% accuracy achieved

3. **Edge Cases: 0.5/2 (25%)** ⚠️ **Minor gaps on specialty topics**
   - Q6: ⚠️ Video rendering → FSx for Lustre (chose EFS - partial credit)
   - Q7: ❌ gp3 Multi-Attach trap (impossible - only io1/io2)

**Improvement Analysis:**
```
Drill Quiz #1 (EFS/Multi-Attach section): 2/5 (40%)
Drill Quiz #2 (EFS/Multi-Attach focus):   8.5/10 (85%)

Improvement: +45 percentage points! 🚀

Critical Pattern Accuracy:
- Multi-AZ concurrent access: 40% → 100% ✅
- Block-level single AZ: 67% → 100% ✅
- Overall EFS decision-making: MASTERED ✅
```

**Status Update:**
- ✅ **Primary weakness RESOLVED** - EFS vs Multi-Attach core patterns at 100%
- ⚠️ **New minor gaps identified** - Multi-Attach volume types, FSx for Lustre
- ✅ **Ready to proceed to Day 3** - Core understanding is solid

**Updated Weakness Priority:**
1. 🟢 **RESOLVED:** Multi-AZ + concurrent access = EFS
2. 🟢 **RESOLVED:** Block-level + single AZ = Multi-Attach
3. 🟢 **RESOLVED:** "Immediately available" = EFS
4. 🟡 **MINOR:** Multi-Attach volume type limitation (io1/io2 only)
5. 🟡 **MINOR:** FSx for Lustre for extreme performance workloads

---

## 📊 Day 1 Quiz Results (January 5, 2026)

**Topic:** Lambda Service Limits & Optimization Patterns
**Score:** 5/10 (50%)
**Status:** New weakness patterns identified - need targeted drilling

**Strengths Demonstrated:**
- ✅ Lambda 15-minute timeout recognition (ECS Fargate for long tasks)
- ✅ Lambda memory = CPU scaling
- ✅ Timeout troubleshooting

**New Weaknesses Identified:**
- ❌ MediaConvert for video transcoding (not Lambda)
- ❌ Kinesis parallelization factor (concurrency per shard)
- ❌ Reserved vs Provisioned Concurrency (throttling solutions)
- ❌ Lambda + EFS for large file caching (> 250 MB)
- ❌ RDS Proxy for Lambda + RDS connection pooling

---

## 📊 Baseline Assessment Results (January 4, 2026)

**Score:** 15/20 (75%)
**Status:** Good starting point after holiday break - 5% below target, but fixable gaps

**Strong Areas (100%):** S3 Storage, VPC Networking, DynamoDB, Cost Optimization
**Weak Areas (<70%):** Lambda Limits (50%), IAM Cross-Account (50%), EC2 Compute (67%), RDS (67%)

---

## 🎯 Current Active Weaknesses (Need Attention)

### 🔴 CRITICAL Priority (0-50% accuracy - Never or rarely correct)

| Topic | Accuracy | Questions | Status | Next Action |
|-------|----------|-----------|--------|-------------|
| **Lambda Service Limits (15-min timeout)** | 50% | 1/2 correct | 🔴 CRITICAL | **URGENT:** Memorize Lambda hard limits - 15-min timeout means ECS/Fargate for longer tasks |
| **IAM Cross-Account Access Patterns** | 50% | 1/2 correct | 🔴 CRITICAL | **URGENT:** Learn when to use IAM roles vs pre-signed URLs vs resource policies |

### 🟠 HIGH Priority (51-75% accuracy - Inconsistent, need drilling)

| Topic | Accuracy | Questions | Status | Next Action |
|-------|----------|-----------|--------|-------------|
| **Elastic Beanstalk (PaaS) vs Manual Infrastructure** | 67% | 2/3 correct | 🟠 NEEDS WORK | Keyword recognition: "limited expertise" → Beanstalk, not EC2/ASG/ALB |
| **RDS Multi-AZ vs Multi-Region Concepts** | 67% | 2/3 correct | 🟠 NEEDS WORK | Multi-Region is NOT a native RDS feature; Multi-AZ = automatic failover |
| **RDS Read Replica Routing Strategies** | 67% | 2/3 correct | 🟠 NEEDS WORK | Direct endpoint provision vs load balancer logic - consider constraints |

### 🟡 MEDIUM Priority (76-89% accuracy - Mostly correct, polish needed)

_None identified yet - will populate as more quizzes are taken._

---

## 🆕 Day 1 New Weaknesses (January 5, 2026)

### 🔴 NEW WEAKNESS #6: MediaConvert vs Lambda for Video Processing

**The Problem:** You chose Lambda multi-part upload for 4K video transcoding, missing Lambda's timeout and storage limits.

**The Rule:**
```
Video/Media Processing Keywords → Purpose-Built Service

"video transcoding" → AWS Elemental MediaConvert ✅
"live streaming" → AWS Elemental MediaLive ✅
"image resize/watermark" → Lambda + S3 ✅
"4K video processing" → MediaConvert (NOT Lambda) ✅

Why Lambda fails:
├─ 15-minute timeout (transcoding takes 30+ min)
├─ 10 GB /tmp limit (4K videos are 10-50+ GB)
└─ Not optimized for video codecs
```

**Target:** Recognize "video transcoding" → MediaConvert instantly

---

### 🔴 NEW WEAKNESS #7: Kinesis Parallelization Factor

**The Problem:** You chose shard iterator type (WHERE to read) instead of parallelization factor (HOW FAST to process).

**The Rule:**
```
Kinesis + Lambda Performance Formula:

Total Concurrent Invocations = Shards × Parallelization Factor

Examples:
├─ 5 shards × 1 parallelization = 5 concurrent Lambda invocations
├─ 5 shards × 10 parallelization = 50 concurrent Lambda invocations
└─ 10 shards × 10 parallelization = 100 concurrent Lambda invocations

Parallelization Factor:
├─ Range: 1-10
├─ Default: 1 (sequential processing per shard)
├─ Set to 10: Each shard processed by 10 Lambda instances in parallel
└─ Use when: "Kinesis processing too slow" or "records backing up"

NOT Shard Iterator (that's for WHERE to start reading: LATEST, TRIM_HORIZON)
```

**Target:** Remember the formula: Shards × Parallelization = Total Concurrency

---

### 🔴 NEW WEAKNESS #8: Lambda Throttling vs Performance (Concurrency Types)

**The Problem:** You chose "increase memory" for throttling (concurrency problem), not performance (speed problem).

**The Rule:**
```
Lambda Problems & Solutions:

Problem: "Throttled" / "Rate Exceeded" / "No capacity"
└─ This is a CONCURRENCY problem (not enough instances)
   ├─ Solution 1: Reserved Concurrency (guarantee capacity for this function)
   ├─ Solution 2: Provisioned Concurrency (pre-warm instances, best for spikes)
   └─ NOT: Increase memory (that's for speed, not capacity!)

Problem: "Slow" / "Timing out" / "Takes too long"
└─ This is a PERFORMANCE problem (execution speed)
   ├─ Solution: Increase memory (more memory = more CPU)
   └─ NOT: Concurrency (that's for capacity, not speed!)

Problem: "Cold starts" / "First request slow"
└─ This is a LATENCY problem (initialization time)
   └─ Solution: Provisioned Concurrency (keep instances warm)

Three Concurrency Types:
1. On-Demand (default): Scales 0→1,000, burst limit +500/min
2. Reserved: Guarantees X slots for this function (prevents other functions from stealing)
3. Provisioned: Pre-warms X instances (no cold starts, instant capacity)
```

**Target:** Throttling = concurrency problem, NOT memory problem

---

### 🔴 NEW WEAKNESS #9: Lambda Storage Options (Deployment Package vs Layers vs EFS)

**The Problem:** You chose Lambda layers for 2 GB ML model, but layers have 250 MB total limit.

**The Rule:**
```
Lambda Storage Decision Tree:

File < 250 MB + Single function:
└─ Deployment package ✅

File < 250 MB + Multiple functions need same file:
└─ Lambda Layer ✅ (shared across functions)

File 250 MB - 10 GB + Download each cold start is OK:
└─ /tmp directory (configure size) ✅

File > 250 MB + Need to CACHE across invocations:
└─ Lambda + EFS (Elastic File System) ✅
   ├─ Persistent storage (survives cold starts)
   ├─ Download ONCE, use forever
   └─ Shared across all Lambda instances

File > 10 GB:
└─ Lambda NOT appropriate ❌
   └─ Use ECS Fargate or EC2

Lambda Limits to Memorize:
├─ Deployment package: 250 MB max (unzipped)
├─ Lambda layers: 250 MB total (all layers + package)
├─ /tmp directory: 512 MB - 10 GB (configurable, ephemeral)
└─ EFS: Unlimited (persistent)
```

**Target:** Know when to use EFS (> 250 MB + need caching)

---

## 🆕 Day 2 New Weaknesses (January 6, 2026)

### 🔴 NEW WEAKNESS #10: Instance Store vs EBS vs EFS (Storage Performance Hierarchy)

**The Problem:** You chose EFS Max I/O for temporary data needing "sub-millisecond latency" and "HIGHEST I/O performance", missing that Instance Store is faster than ANY network storage.

**The Mistake:**
```
Question: 5 TB temp data, millions of IOPS, sub-millisecond latency, can be regenerated
Your Answer: EFS Max I/O ❌
Correct Answer: Instance Store ✅

Why you were wrong:
├─ EFS is a NETWORK file system (higher latency, network overhead)
├─ EFS Max I/O mode has HIGHER latency than General Purpose
├─ EFS is for SHARED storage across instances, not single-instance performance
└─ Missed keywords: "temporary", "can be regenerated", "HIGHEST I/O"
```

**The Rule:**
```
EC2 Storage Performance Hierarchy (Fastest → Slowest):

1. Instance Store (FASTEST)
   ├─ Millions of IOPS, sub-millisecond latency
   ├─ Ephemeral (lost on stop/terminate/hardware failure)
   ├─ NO cost beyond instance price
   ├─ Use for: Cache, buffers, scratch data, temporary files
   └─ Keywords: "temporary", "can be regenerated", "HIGHEST I/O"

2. EBS Provisioned IOPS (io2 Block Express)
   ├─ Up to 64,000 IOPS, 4,000 MB/s throughput
   ├─ Persistent, survives stop/terminate
   ├─ Costs money (GB-month + IOPS)
   ├─ Use for: Databases, high-performance apps needing persistence
   └─ Keywords: "high IOPS", "persistent", "cost secondary"

3. EBS General Purpose (gp3)
   ├─ Up to 16,000 IOPS, 1,000 MB/s throughput
   ├─ Balanced price/performance
   └─ Use for: Most workloads

4. EFS (SLOWEST for single-instance I/O)
   ├─ Network latency (milliseconds, not sub-millisecond)
   ├─ Designed for SHARED access across many instances
   ├─ Max I/O mode = HIGHER latency (bad for performance)
   └─ Use for: Shared file storage, multi-AZ, concurrent access
```

**Decision Tree:**
```
Does data need to PERSIST between reboots?
├─ NO (ephemeral OK) ───→ Instance Store ✅ (if HIGHEST performance needed)
└─ YES (must persist) ───→ EBS (io2 for extreme, gp3 for balanced)

Does data need to be SHARED across multiple instances?
├─ YES ───→ EFS ✅
└─ NO ───→ Instance Store or EBS

What's the performance requirement?
├─ "HIGHEST I/O" + "sub-millisecond" + "temporary" ───→ Instance Store ✅
├─ "High IOPS" + "persistent" ───→ EBS io2 ✅
└─ "Shared" + "multi-AZ" ───→ EFS ✅
```

**Exam Keywords:**
- **"Temporary data" + "can be regenerated" + "HIGHEST I/O"** = Instance Store
- **"Sub-millisecond latency" + "millions of IOPS"** = Instance Store (only storage that can do this)
- **"Network file system"** = EFS (inherently slower than local storage)
- **"Max I/O mode"** = Higher latency (opposite of what you want for speed)

**Target:** Memorize Instance Store as FASTEST storage, ephemeral, for temporary high-performance data

---

### 🔴 NEW WEAKNESS #11: EBS Multi-Attach Limitations (NOT a DR Solution!)

**The Problem:** You chose EBS Multi-Attach for disaster recovery from AZ failures with 15-minute RPO, completely misunderstanding what Multi-Attach does.

**The Mistake:**
```
Question: DR strategy for RTO=1 hour, RPO=15 minutes, protect against instance + AZ failures
Your Answer: EBS Multi-Attach across multiple AZs ❌
Correct Answer: AWS Backup with 15-minute snapshots ✅

Why you were CATASTROPHICALLY wrong:
├─ EBS Multi-Attach only works in SINGLE AZ (not multi-AZ!)
├─ Multi-Attach is for CONCURRENT ACCESS, not backup/DR
├─ Multi-Attach doesn't protect against AZ failure (same AZ = same failure domain)
├─ Multi-Attach doesn't create backups (no RPO protection)
└─ If someone deletes a file, ALL attached instances see it deleted!
```

**The Rule:**
```
EBS Multi-Attach - What It Actually Does:

Purpose: Attach ONE EBS volume to MULTIPLE EC2 instances simultaneously
├─ Max: 16 instances in the SAME AVAILABILITY ZONE
├─ Volume types: io1 or io2 Provisioned IOPS ONLY
├─ Requires: Cluster-aware file system (not standard ext4/xfs!)
└─ Use case: Clustered databases, shared storage for cluster nodes

What Multi-Attach IS:
✅ Concurrent read/write access to same volume
✅ High-availability within a cluster (if one node fails, others still attached)

What Multi-Attach is NOT:
❌ NOT a backup solution (no snapshots, no point-in-time recovery)
❌ NOT disaster recovery (single AZ = single failure domain)
❌ NOT multi-AZ (all instances must be in SAME AZ)
❌ NOT data protection (data corruption/deletion affects all instances)
❌ NOT automatic failover (you handle failover logic)
```

**DR Solution for This Question:**
```
Requirements:
├─ RTO = 1 hour (how fast to recover)
├─ RPO = 15 minutes (max data loss acceptable)
├─ Protect against instance AND AZ failures
└─ Minimal data loss

Correct Solution: AWS Backup with 15-minute snapshots
├─ Snapshots every 15 minutes = 15-minute RPO ✅
├─ Snapshots stored across AZs automatically = AZ failure protection ✅
├─ Restore from snapshot in ~30-60 minutes = RTO < 1 hour ✅
└─ AWS Backup automates scheduling and lifecycle

Why this meets requirements:
RPO = 15 minutes ───→ Take snapshots every 15 minutes
RTO = 1 hour ───→ Can restore EBS volume from snapshot within 1 hour
AZ failure ───→ Snapshots replicated across AZs automatically
```

**RPO/RTO Mapping (Memorize This!):**
```
RPO (Recovery Point Objective) = How much data loss is acceptable?
└─ Determines BACKUP FREQUENCY
   ├─ 15-minute RPO = Snapshots every 15 minutes
   ├─ 1-hour RPO = Snapshots every hour
   └─ 24-hour RPO = Daily snapshots

RTO (Recovery Time Objective) = How fast must you recover?
└─ Determines RECOVERY METHOD
   ├─ Seconds = Active-Active (Multi-Site)
   ├─ Minutes = Warm Standby (running but scaled down)
   ├─ Hours = Pilot Light (minimal always-on, scale up on disaster)
   └─ Days = Backup & Restore (cheapest, slowest)
```

**Exam Keywords:**
- **"RPO = X minutes"** → Backup frequency must match: snapshots every X minutes
- **"Multi-AZ protection"** → EBS Multi-Attach is WRONG (single AZ only)
- **"Disaster recovery"** → Think backups/snapshots, NOT Multi-Attach
- **"Cluster-aware file system"** → This is the ONLY valid use case for Multi-Attach

**Target:** Understand EBS Multi-Attach is for concurrent access in SINGLE AZ, NOT for DR/backups

---

### 🔴 NEW WEAKNESS #12: Placement Groups - Partition vs Spread (Cassandra/Kafka Pattern)

**The Problem:** You chose Spread Placement Group for Apache Cassandra distributed database, missing that Spread has a 7-instance-per-AZ limit and Partition is designed for large distributed systems.

**The Mistake:**
```
Question: Cassandra cluster across multiple AZs, protection against hardware failures, partition-aware client
Your Answer: Spread Placement Group ❌
Correct Answer: Partition Placement Group ✅

Why you were wrong:
├─ Spread has MAX 7 INSTANCES PER AZ (too small for Cassandra cluster!)
├─ Cassandra typically needs DOZENS to HUNDREDS of nodes
├─ The question said "partition-aware client" (huge hint!)
└─ Partition groups are DESIGNED for Cassandra/Kafka/Hadoop
```

**The Rule:**
```
EC2 Placement Groups - Complete Breakdown:

1. CLUSTER Placement Group
   ├─ Purpose: LOWEST network latency, HIGHEST throughput
   ├─ Location: Single Availability Zone (all instances close together)
   ├─ Use cases: HPC, MPI, machine learning training, tightly-coupled apps
   ├─ Performance: Up to 100 Gbps bandwidth between instances
   └─ Keywords: "low latency", "HPC", "MPI", "tightly-coupled"

2. SPREAD Placement Group
   ├─ Purpose: Maximum isolation for CRITICAL instances
   ├─ Limit: MAX 7 INSTANCES PER AVAILABILITY ZONE ⚠️
   ├─ Each instance on separate hardware rack
   ├─ Use cases: Small number of critical instances (domain controllers, critical app servers)
   ├─ Can span multiple AZs
   └─ Keywords: "critical instances", "maximum isolation", "small scale"

3. PARTITION Placement Group
   ├─ Purpose: Large DISTRIBUTED and REPLICATED workloads
   ├─ Structure: Up to 7 partitions per AZ, HUNDREDS of instances per partition
   ├─ Each partition on separate hardware racks
   ├─ Use cases: Cassandra, Kafka, Hadoop, HDFS, large distributed databases
   ├─ Can span multiple AZs
   ├─ Partition-aware applications can control which partition instances go to
   └─ Keywords: "Cassandra", "Kafka", "Hadoop", "distributed database", "partition-aware"
```

**Decision Matrix:**
```
What's the workload?
├─ HPC / MPI / Machine Learning / Low Latency ───→ CLUSTER ✅
├─ Cassandra / Kafka / Hadoop / Distributed DB ───→ PARTITION ✅
└─ Critical instances that must be isolated ───→ SPREAD ✅

How many instances?
├─ < 7 per AZ ───→ SPREAD possible ✅
├─ > 7 per AZ ───→ SPREAD impossible ❌, use PARTITION instead ✅
└─ Hundreds ───→ PARTITION only ✅

Need multi-AZ?
├─ Yes + Low latency ───→ NOT CLUSTER (single AZ only)
├─ Yes + Distributed system ───→ PARTITION ✅
└─ Yes + Critical instances ───→ SPREAD ✅
```

**Exam Pattern Recognition:**
```
Keyword Detection:

"Cassandra" or "Kafka" or "Hadoop" or "HDFS"
└─ PARTITION Placement Group ✅ (100% of the time)

"partition-aware client" or "partition-aware application"
└─ PARTITION Placement Group ✅ (it's literally in the name!)

"HPC" or "MPI" or "LOWEST latency" or "tightly-coupled"
└─ CLUSTER Placement Group ✅

"7 or fewer instances" + "critical" + "isolated"
└─ SPREAD Placement Group ✅

"Large distributed system" + "hardware isolation"
└─ PARTITION Placement Group ✅
```

**Common Exam Traps:**
```
Trap: "Cassandra needs hardware isolation, so use Spread!"
Reality: Cassandra clusters have 50+ nodes → Exceeds Spread's 7-per-AZ limit
Solution: Partition gives hardware isolation AND scales to hundreds of instances

Trap: "Need multi-AZ high availability, use Cluster!"
Reality: Cluster is SINGLE AZ only
Solution: Partition or Spread for multi-AZ

Trap: "Protection against hardware failures means Spread!"
Reality: Spread is for SMALL SCALE critical instances
Solution: For large distributed systems, use Partition
```

**Target:** Memorize: Cassandra/Kafka/Hadoop = PARTITION (always), Spread = max 7 per AZ (small scale only)

---

### 🔴 NEW WEAKNESS #10: Lambda + RDS Connection Pooling (RDS Proxy)

**The Problem:** You chose connection pooling in Lambda code, missing that each Lambda instance has its own pool.

**The Rule:**
```
Lambda + Database Patterns:

Lambda + RDS (MySQL/PostgreSQL):
└─ Use RDS Proxy ✅
   ├─ Multiplexes connections (500 Lambda → 50 RDS connections)
   ├─ Each Lambda instance = separate connection without proxy
   ├─ Connection pooling in code DOESN'T work (each instance has own pool)
   └─ RDS Proxy pools across ALL Lambda instances

Lambda + DynamoDB:
└─ No proxy needed ✅
   └─ Serverless, no connection limits

Lambda + Aurora:
└─ Can use RDS Proxy ✅
   └─ Benefits from connection pooling and automatic failover

Why connection pooling in code fails:
├─ 500 Lambda instances × 5 connections per pool = 2,500 total connections
├─ Each instance is isolated (doesn't share pools)
└─ Makes problem WORSE, not better!

RDS Connection Limits:
├─ db.t3.small: ~150 max connections
├─ db.t3.medium: ~300 max connections
└─ db.r5.large: ~1,000 max connections
```

**Target:** Lambda + RDS = Always consider RDS Proxy

---

## ✅ Previously Resolved Weaknesses (From Dec 2025 Study Period)

**These topics were mastered during your previous study period. Keep them sharp!**

| Topic | Resolution Date | Final Score | Verification |
|-------|----------------|-------------|--------------|
| **DynamoDB Query vs Scan (Frequency)** | Dec 18, 2025 | 100% | ✅ 10/10 on Retry #2 Final Drill - PERFECT SCORE! |
| **Athena vs Redshift (Query Frequency)** | Dec 11, 2025 | 100% | ✅ 5/5 correct (Final Boss Q1-5) |
| **DynamoDB Partition Key Design** | Dec 10, 2025 | 100% | ✅ 2/2 correct (Comprehensive quiz Q3-4) |
| **S3 Storage Classes ("very rarely" = Glacier)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **Aurora Multi-Master (RTO <30 sec)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **DynamoDB Consistency (Eventually = 50%)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **DynamoDB Capacity Modes (Known vs Unknown)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **DynamoDB Extreme Write Throughput (100K+/sec)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **Redshift for Frequent Analytics** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **RDS Proxy (Lambda + RDS)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **QLDB (Immutable Ledger)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **VPC NACLs (Stateless)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **Auto Scaling Policy Combinations** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **EC2 Placement Groups** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **VPC Endpoints (Gateway vs Interface)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **RDS Multi-AZ vs Read Replicas** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **Aurora Backtrack (MySQL-only)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **DynamoDB Streams** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |

---

---

## 📊 Weakness Deep Dives (January 4, 2026 - Active Weaknesses)

### 🔴 WEAKNESS #1: Lambda Service Limits (CRITICAL)

**The Problem:** You chose Step Functions chunking instead of recognizing Lambda's 15-minute hard limit makes it unsuitable for 12-18 minute tasks.

**The Rule You Must Memorize:**

```
Lambda Hard Limits (CANNOT be exceeded):
┌─ Timeout: 15 minutes MAXIMUM (900 seconds)
├─ Memory: 10 GB maximum
├─ Concurrent executions: 1,000 (soft limit, can request increase)
├─ Deployment package: 50 MB zipped, 250 MB unzipped
├─ /tmp storage: 512 MB (ephemeral)
└─ Payload: 6 MB sync, 256 KB async

When task exceeds 15 minutes:
└─ Use ECS Fargate (no timeout), EC2, or AWS Batch
   └─ Lambda is OUT - no exceptions, no workarounds
```

**Decision Tree for Long-Running Tasks:**

```
Task duration > 15 minutes?
│
├─ YES → Lambda CANNOT be used
│  └─ Choose:
│     ├─ ECS Fargate (serverless containers, no timeout)
│     ├─ AWS Batch (managed batch processing)
│     └─ EC2 (full control, manual management)
│
└─ NO (< 15 minutes) → Lambda is viable
   └─ But also consider:
      - Memory needs (max 10 GB)
      - Payload size (max 6 MB sync)
      - Stateful requirements (Lambda is stateless)
```

**Target:** 100% accuracy on Lambda limits questions

---

### 🔴 WEAKNESS #2: IAM Cross-Account Access Patterns (CRITICAL)

**The Problem:** You chose S3 pre-signed URLs when cross-account IAM role assumption was the AWS best practice.

**The Three Methods Compared:**

| Method | Use Case | Pros | Cons |
|--------|----------|------|------|
| **Cross-Account IAM Roles** | Third-party needs dynamic access to browse/discover resources | ✅ AWS best practice<br>✅ Temporary credentials<br>✅ No sharing your credentials<br>✅ Flexible access | Requires vendor has AWS account |
| **S3 Pre-Signed URLs** | Share specific objects known in advance | ✅ Simple<br>✅ Time-limited<br>✅ No AWS account needed | ❌ Must generate URL per object<br>❌ Can't browse/discover<br>❌ Not scalable for many objects |
| **Resource-Based Policies** | Grant access to AWS services or specific AWS accounts | ✅ No role assumption needed<br>✅ Direct access | ❌ Broad permissions<br>❌ Less auditable |

**Decision Tree:**

```
Third-party needs temporary access to your AWS resources?
│
├─ Do they have their own AWS account?
│  │
│  ├─ YES → Cross-Account IAM Role (BEST PRACTICE)
│  │  └─ They use AssumeRole with their credentials
│  │  └─ Temporary session tokens (2-12 hours)
│  │  └─ Can browse/discover resources dynamically
│  │
│  └─ NO (external vendor without AWS account)
│     └─ Do they need specific objects known in advance?
│        ├─ YES → S3 Pre-Signed URLs (per object)
│        └─ NO → Create temporary IAM user (delete after)
│
└─ Is this AWS service accessing your resources?
   └─ Resource-Based Policy (S3 bucket policy, etc.)
```

**Key Exam Patterns:**
- "Third-party vendor" + "temporary access" + "vendor has AWS account" = **IAM Role Assumption**
- "Share specific files" + "time-limited" + "no AWS account" = **Pre-Signed URLs**
- "Lambda accessing S3" or "CloudFront accessing S3" = **Resource-Based Policy**

**Target:** 100% accuracy on cross-account access questions

---

### 🟠 WEAKNESS #3: Elastic Beanstalk vs Manual Infrastructure

**The Problem:** You chose manual EC2/ASG/ALB setup for a scenario with "limited AWS expertise."

**The Exam Keyword Pattern:**

```
Question contains these keywords?
│
├─ "limited expertise"
├─ "minimize operational overhead"
├─ "least operational complexity"
├─ "fastest time to deploy"
└─ "no infrastructure management experience"
   │
   └─ Answer = Platform-as-a-Service (PaaS)
      └─ Elastic Beanstalk (for web apps)
      └─ AWS Amplify (for frontend/mobile)
      └─ AWS AppRunner (for containerized web apps)
```

**When to Choose What:**

| Scenario | Service | Why |
|----------|---------|-----|
| Limited expertise, web app (PHP, Node, Python, Java) | **Elastic Beanstalk** | Automatic infrastructure provisioning |
| Experienced team, need full control of infrastructure | EC2 + ASG + ALB | Manual setup, more control |
| Serverless, event-driven functions | Lambda | No servers to manage |
| Containers, no server management | ECS Fargate | Serverless containers |
| Containers, need control over instances | ECS on EC2 | More control, lower cost at scale |

**Target:** Recognize PaaS keywords instantly

---

### 🟠 WEAKNESS #4: RDS Multi-AZ vs Multi-Region

**The Problem:** You chose "RDS Multi-Region deployment" which doesn't exist as a native RDS feature.

**What Actually Exists:**

```
RDS High Availability & Disaster Recovery Options:

1. RDS Multi-AZ (Same Region HA)
   ├─ Automatic failover: 60-120 seconds
   ├─ Synchronous replication to standby
   ├─ No read scaling (standby cannot serve reads)
   └─ Use case: Protect against AZ failure

2. RDS Read Replicas (Read Scaling)
   ├─ Asynchronous replication
   ├─ Can be in same region or cross-region
   ├─ CAN be manually promoted to standalone DB
   └─ Use case: Scale reads, cross-region DR

3. Aurora Global Database (Multi-Region DR)
   ├─ Cross-region replication: <1 second lag
   ├─ Manual failover to secondary region
   ├─ Up to 5 secondary regions
   └─ Use case: Global applications, DR across regions

❌ "RDS Multi-Region Deployment" is NOT a thing!
```

**Decision Tree:**

```
What's the requirement?
│
├─ High availability WITHIN a region (RTO < 2 min, automatic failover)
│  └─ RDS Multi-AZ ✅
│
├─ Scale READ traffic (read-heavy workload)
│  └─ RDS Read Replicas (up to 15) ✅
│
├─ Disaster recovery ACROSS regions
│  └─ RDS Read Replica in another region ✅
│     └─ Manual promotion during disaster
│
└─ Ultra-fast cross-region replication (<1 sec lag)
   └─ Aurora Global Database ✅
      └─ Aurora only, not RDS MySQL/PostgreSQL
```

**Target:** Never confuse Multi-AZ with Multi-Region again

---

### 🟠 WEAKNESS #5: RDS Read Replica Routing

**The Problem:** You chose load balancer routing when the constraint was "cannot modify application code."

**The Key Insight:**

```
Constraint: "Cannot modify application code"
│
├─ Option A: Configure load balancer to route analytical queries to replica
│  └─ Requires: Application logic to identify query types
│  └─ Result: ❌ This IS modifying application code/logic
│
└─ Option B: Give analytics team the read replica endpoint URL
   └─ Requires: Analytics team connects to different endpoint
   └─ Result: ✅ No application code changes
```

**The Pattern:**

When separating read workloads (OLTP vs analytics):
1. **Application can route:** Use application logic + read replica endpoint
2. **Application cannot route:** Give different teams different endpoints
3. **Need automatic routing:** Use Aurora with reader endpoint (auto load balances across replicas)

**Target:** Recognize "cannot modify" constraints

---

## 📊 Key Patterns & Decision Trees (Reference)

### DynamoDB: Numeric Partition Key Anti-Pattern

```
❌ NEVER: Numeric/boolean/low-cardinality values as partition key for range queries
   - amount, price, score, age, experience_years
   - flagged (true/false), status (2-5 values)
   - Partition key requires EXACT match - can't do > or < or BETWEEN

✅ ALWAYS: Use one of these patterns instead:

Pattern 1: Static Partition Key + Numeric Sort Key
   partition_key = "HIGH_VALUE" (static)
   sort_key = amount (numeric)
   Query: partition_key = "HIGH_VALUE" AND amount > 75000 ✅

Pattern 2: Computed Category Attribute
   amount_tier = "HIGH" | "MEDIUM" | "LOW"
   experience_level = "SENIOR" | "JUNIOR"
   Query: partition_key = "SENIOR" ✅
```

### DynamoDB: Query vs Scan Decision Tree

```
Need to query by different attribute than partition key?
│
├─ YES (e.g., query by "category" when partition key is "product_id")
│  └─ Use GSI (Global Secondary Index)
│     - GSI can have DIFFERENT partition key
│     - Enables cross-partition queries
│
└─ NO (just need alternative sort key on SAME partition)
   └─ Use LSI (Local Secondary Index)
      - LSI shares SAME partition key as base table
      - Only changes sort key
```

### EC2 Cost Optimization

```
Is the workload PREDICTABLE?
│
├─ YES (e.g., 9-5 weekdays, always 50 instances)
│  └─ Reserved Instances (1-year or 3-year)
│     - Discount: 40% (1-year) or 60% (3-year)
│
└─ NO (unpredictable traffic)
   └─ Auto Scaling with:
      ├─ On-Demand (for flexibility)
      └─ Spot (for cost savings if interruptible)
```

### S3 Storage Classes Decision Tree

```
Access frequency?
│
├─ Frequently (daily/weekly) → S3 Standard
├─ Infrequently (monthly) → S3 Standard-IA
├─ Rarely (quarterly) → S3 One Zone-IA or Glacier Instant Retrieval
├─ Very rarely (yearly) + mins retrieval → Glacier Flexible Retrieval
└─ Archive (almost never) + hours retrieval → Glacier Deep Archive
```

---

## 📝 How to Use This Tracker

1. **After each quiz:** Update active weaknesses immediately
2. **Track patterns:** Not just topics, but WHY you got it wrong
3. **Move to resolved:** Only when 90%+ accuracy on 3+ questions
4. **Review weekly:** Check if weaknesses are improving or stuck
5. **Fresh start mindset:** Don't be discouraged by December's struggles - this is a new study period!

---

## 🚨 Warning Signs

**A weakness needs immediate attention when:**
- ⚠️ Scoring <70% on same topic across 2+ quizzes
- ⚠️ Making the SAME mistake on different quiz dates
- ⚠️ Defaulting to wrong pattern without thinking
- ⚠️ Uncertainty when answering (guessing, not confident)

**A weakness is resolving when:**
- ✅ Scoring 80%+ consistently
- ✅ Answering confidently without hesitation
- ✅ Able to explain WHY wrong answers are wrong
- ✅ No repeat mistakes on retakes

---

**Last Updated:** January 4, 2026
**Next Review:** After first quiz of new study period (Week 1)
