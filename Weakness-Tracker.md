# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** January 13, 2026, 7:05 PM CST (Post Lambda + External Data Sources Drill - 44% FAILED)
**Exam Date:** February 11, 2026 at 5:15 PM EST (29 days remaining)
**Study Period:** January 5 - February 10, 2026 (37 days)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 📊 Day 7 Lambda + External Data Sources Recovery Drill (January 13, 2026)

### Lambda + External Data Sources Targeted Drill
**Topic:** Lambda memory limits, ElastiCache vs EFS vs S3, RDS Proxy, performance requirements
**Score:** 4/9 (44%)
**Status:** 🚨 CATASTROPHIC FAILURE - Critical pattern recognition breakdown

**Context:** This drill targeted the Week 1 Comprehensive weakness (Q14 - Lambda + 12 GB dataset, scored 67% on this topic). The user regressed significantly, demonstrating fundamental misunderstanding of service selection criteria.

**Questions Correct: 4/9**
- ✅ Q1: Lambda + 12 GB dataset + 10ms latency → ElastiCache
- ✅ Q2: Lambda + 6 GB ML model → EFS
- ✅ Q3: Lambda + RDS connection exhaustion → RDS Proxy
- ✅ Q9: Lambda memory exceeded → Increase memory to 2 GB

**Questions Incorrect: 5/9 (Critical Failures)**
- ❌ Q4: Chose EFS for 80 MB log files (should use /tmp ephemeral storage)
- ❌ Q5: Chose EFS for 15 GB + 200ms latency (should use ElastiCache)
- ❌ Q6: Chose "connection limit" for DynamoDB throttling (should recognize hot partition)
- ❌ Q7: Chose ElastiCache for 500 MB static lookup table (should use /tmp)
- ❌ Q8: Chose ElastiCache+EFS split for 5 GB ML inference (should use /tmp+memory)
- ❌ Q10: Chose /tmp for 50ms SLA authentication (should use ElastiCache) **CRITICAL: Question explicitly stated /tmp was failing!**

---

### 🚨 CRITICAL NEW WEAKNESSES IDENTIFIED

#### 🔴 WEAKNESS #13: Reading Comprehension - Ignoring Explicit Failure Modes (CATASTROPHIC)

**The Disaster:**
Q10 explicitly stated: "Currently, the Lambda function downloads the revocation list from S3 on each cold start (taking 5-8 seconds), but authentication is failing SLA during traffic spikes when new Lambda containers are created."

**What you did:** Chose option B - /tmp caching (THE EXACT FAILING SOLUTION DESCRIBED IN THE QUESTION)

**The Problem:**
```
You chose the solution that the question EXPLICITLY stated was failing.
This is not a knowledge gap - this is a reading comprehension failure.
```

**Root Cause Analysis:**
- Not reading the entire scenario before selecting answer
- Seeing "300 MB dataset" and immediately defaulting to "/tmp" without considering constraints
- Ignoring "50ms SLA" requirement
- Ignoring "10,000 req/min" high-frequency access pattern
- Ignoring "updated every 5 minutes" (frequent updates)
- Ignoring explicit statement that current approach (cold start downloads) is failing

**The Rule You MUST Learn:**
```
When a question describes a FAILING solution:
1. Read the ENTIRE scenario
2. Identify what's currently being used
3. Identify WHY it's failing
4. NEVER choose a solution that matches the failing approach

Q10 Explicitly Said:
"authentication is failing SLA during traffic spikes when new Lambda containers are created"

Translation: Cold start downloads are TOO SLOW for the SLA
Solution: Need EXTERNAL CACHE with sub-millisecond lookups = ElastiCache
```

**Exam Impact:** CRITICAL - If you don't read full scenarios, you'll miss 20-30% of questions

---

#### 🔴 WEAKNESS #14: Lambda Storage Decision Framework - Overcorrection Swings (CRITICAL)

**The Pattern of Failure:**
```
Questions 1-3: You correctly identified when to use ElastiCache, EFS, RDS Proxy ✅

Questions 4-8: You started swinging wildly between wrong extremes:
├─ Q4: Over-complicated (EFS for 80 MB) when /tmp was simple
├─ Q5: Under-estimated (EFS for 15 GB + 200ms) when ElastiCache needed
├─ Q7: Over-complicated (ElastiCache for static 500 MB) when /tmp was simple
└─ Q8: Architectural disaster (split 5 GB across two services) when single solution worked

Question 10: Chose failing solution explicitly described as failing ❌
```

**Root Cause:** You don't have a systematic decision framework - you're pattern-matching superficially instead of analyzing requirements.

**The Framework You MUST Internalize:**

```
Lambda + External Data Storage Decision Tree:

STEP 1: Can it fit in Lambda? (< 10 GB total)
├─ NO (> 10 GB) → MUST use external storage → Go to STEP 2A
└─ YES (< 10 GB) → CAN use Lambda memory/tmp → Go to STEP 2B

STEP 2A: External Storage Required (Dataset > 10 GB)
├─ What's the latency requirement?
│  ├─ Sub-second (<1 sec) → ElastiCache Redis (sub-ms lookups)
│  └─ Seconds acceptable → EFS (if file operations needed)
│
└─ What's the access pattern?
   ├─ Key-value lookups → ElastiCache
   └─ File operations → EFS

STEP 2B: Lambda Can Hold It (Dataset < 10 GB)
├─ What's the SLA / latency requirement?
│  ├─ Strict SLA that cold starts violate (<100ms)?
│  │  └─ ElastiCache (external, no cold start impact)
│  │
│  └─ Cold starts acceptable (seconds)?
│     └─ Continue to STEP 3
│
STEP 3: Update Frequency
├─ Updated frequently (minutes) or high request rate (1000s/sec)?
│  └─ ElastiCache (in-memory, fast updates)
│
└─ Updated infrequently (hours/days)?
   └─ /tmp caching (download once per container)

STEP 4: ML Inference Special Case
├─ Is this ML inference?
│  ├─ Model + data < 10 GB → Lambda memory + /tmp
│  ├─ Model + data > 10 GB → NOT Lambda (use SageMaker)
│  └─ NEVER split model across services
│
└─ Cost consideration
   └─ "MOST cost-effective" mentioned?
      └─ Prefer /tmp over ElastiCache when both work
```

**Application to Failed Questions:**

| Question | Data Size | Latency | Access Pattern | Update Freq | Correct Answer | Your Wrong Answer |
|----------|-----------|---------|----------------|-------------|----------------|-------------------|
| Q4 | 80 MB | Seconds OK | Single function | N/A | **/tmp (512 MB → 1 GB)** | EFS |
| Q5 | 15 GB | 200ms | Sorted lookups | Every 6 hrs | **ElastiCache** | EFS |
| Q7 | 500 MB | No strict SLA | Load once | Hourly | **/tmp** | ElastiCache |
| Q8 | 5 GB | 100ms | ML inference | N/A | **/tmp + memory** | ElastiCache+EFS |
| Q10 | 300 MB | 50ms SLA | 10K req/min | Every 5 min | **ElastiCache** | /tmp |

**Target:** 100% accuracy on storage selection by applying framework systematically

---

#### 🔴 WEAKNESS #15: DynamoDB vs RDS Architecture (Connection Models) (HIGH)

**The Problem:** Q6 - You said DynamoDB has "connection limits" that Lambda exceeds. DynamoDB is connectionless.

**The Mistake:**
```
Question: DynamoDB throttling with ProvisionedThroughputExceededException, sufficient RCU/WCU provisioned
Your Answer: "Lambda concurrent executions exceed DynamoDB connection limit" ❌
Correct Answer: Hot partition causing partition-level throttling ✅

Why you were catastrophically wrong:
├─ DynamoDB has NO connection limits (it's HTTP/REST API, not connection-based)
├─ You confused DynamoDB (connectionless) with RDS (connection-based)
├─ You applied RDS Proxy pattern (Q3) to the wrong service
└─ You missed "sufficient provisioned capacity" = not a capacity problem, it's a HOT PARTITION problem
```

**The Rule:**
```
AWS Database Connection Models:

RDS/Aurora (MySQL/PostgreSQL):
├─ Connection-based protocol
├─ Has connection limits (150-5,000 depending on instance size)
├─ Lambda problem: Each instance creates connections → exhaustion
├─ Solution: RDS Proxy (connection pooling)
└─ Keywords: "Too many connections", "connection exhaustion"

DynamoDB:
├─ Connectionless (HTTP REST API)
├─ NO connection limits
├─ Lambda problem: Hot partitions (uneven key distribution)
├─ Solution: Better partition key design, caching hot items, auto-scaling
└─ Keywords: "ProvisionedThroughputExceededException", "sufficient capacity but throttling"

ElastiCache (Redis/Memcached):
├─ Connection-based
├─ Has connection limits (depends on node type)
├─ Lambda problem: Each instance creates connections
├─ Solution: Connection pooling in Lambda code
└─ But: Connection limits are much higher than RDS
```

**DynamoDB Throttling Decision Tree:**
```
ProvisionedThroughputExceededException occurred, what's the cause?

Does table have sufficient RCU/WCU for the total workload?
├─ NO → Increase provisioned capacity or switch to On-Demand mode
└─ YES (sufficient capacity) → It's a HOT PARTITION problem
   │
   Hot Partition Causes:
   ├─ Uneven distribution of requests across partition keys
   ├─ Many requests accessing same partition key (hot key)
   ├─ Burst traffic concentrated on specific items
   └─ Adaptive capacity hasn't kicked in yet (takes 5-30 min)
   │
   Solutions:
   ├─ Design better partition key (higher cardinality)
   ├─ Cache frequently accessed items in ElastiCache/DAX
   ├─ Use DynamoDB auto-scaling
   └─ Pre-warm table with dummy requests before traffic spike
```

**Exam Keywords:**
- "Sufficient provisioned capacity" + throttling = Hot partition, NOT capacity problem
- "Too many connections" = RDS (NOT DynamoDB)
- DynamoDB errors are about THROUGHPUT (RCU/WCU), NOT connections

**Target:** Never confuse connection-based (RDS) with connectionless (DynamoDB) services

---

#### 🔴 WEAKNESS #16: /tmp Storage Use Cases - When It FAILS (CRITICAL)

**The Problem:** You don't understand when /tmp caching breaks down under real-world constraints.

**Failed Scenarios:**
```
Q4: Chose EFS for 80 MB files when /tmp was perfect ❌
Q7: Chose ElastiCache for 500 MB static data when /tmp was perfect ❌
Q10: Chose /tmp for 50ms SLA when cold starts violated SLA ❌
```

**When /tmp Works:**
```
✅ Data size: < 10 GB (Lambda ephemeral storage limit)
✅ Update frequency: Infrequent (hourly, daily)
✅ Access pattern: Load once per container, reuse many times
✅ Latency: Cold start delays acceptable (no strict SLA)
✅ Request rate: Low to moderate (cold starts don't happen frequently)
✅ Cost: "MOST cost-effective" is mentioned
```

**When /tmp FAILS:**
```
❌ Strict SLA (<100ms) that cold starts violate
   └─ Cold starts take 5-15 seconds to download data
   └─ New containers created during traffic spikes
   └─ Some requests experience 5-15 second delays
   └─ Solution: ElastiCache (external, no cold start impact)

❌ High request rate (1000s/second)
   └─ Lambda creates many new containers to scale
   └─ Each new container = cold start = download delay
   └─ Constant cold starts = constant SLA violations
   └─ Solution: ElastiCache (always warm, sub-ms access)

❌ Data updated frequently (every few minutes)
   └─ Cached data in /tmp becomes stale
   └─ Need to invalidate/refresh cache
   └─ Complex lifecycle management
   └─ Solution: ElastiCache (centralized updates)

❌ Data shared across multiple Lambda functions
   └─ Each function downloads its own copy
   └─ Wasteful, inconsistent
   └─ Solution: EFS (shared filesystem) or ElastiCache (shared cache)
```

**Q10 Breakdown (Why /tmp Failed):**
```
Scenario: JWT revocation list authentication
├─ Data size: 300 MB (fits in /tmp ✓)
├─ Update frequency: Every 5 minutes (FREQUENT ✗)
├─ Request rate: 10,000 req/min = 166/sec (HIGH ✗)
├─ SLA: 50ms per request (STRICT ✗)
├─ Current problem: Cold starts taking 5-8 seconds (SLA VIOLATION ✗)
└─ Question explicitly states: /tmp caching is FAILING

Why /tmp doesn't work:
├─ 166 req/sec → Lambda scales to 50-100+ containers during peaks
├─ Each new container = 5-8 second cold start
├─ 50ms SLA × 8000ms cold start = 160x SLA violation
├─ Updates every 5 minutes = constant cache invalidation complexity
└─ High availability authentication system can't tolerate cold start delays

Why ElastiCache works:
├─ 300 MB revocation list loaded once into Redis
├─ Sub-millisecond lookups (<1ms)
├─ No cold start impact (external to Lambda)
├─ All Lambda containers query same Redis instance
├─ Update once every 5 minutes in Redis, all Lambdas see updated data instantly
└─ 50ms SLA easily met (<1ms Redis + processing time)
```

**Cost Comparison:**
```
/tmp approach:
├─ Storage cost: ~$0.01/month (300 MB ephemeral)
├─ But: SLA violations = business cost of failed authentications
├─ But: Operational cost of managing cache invalidation
└─ Total: "Cheap" but DOESN'T WORK

ElastiCache approach:
├─ Redis node: ~$30-50/month
├─ Zero SLA violations
├─ Simple architecture (no cache invalidation logic)
└─ Total: $50/month for working solution vs $0 for broken solution
```

**The Exam Trap:**
The exam will show you a "cheap" solution (/tmp) that technically could work in ideal conditions, but will fail under real-world constraints (SLA, high traffic, frequent updates). You MUST read the constraints carefully.

**Target:** Recognize when /tmp cold start delays violate SLA or operational requirements

---

#### 🔴 WEAKNESS #17: ML Inference Architecture - Data Locality Requirements (HIGH)

**The Problem:** Q8 - You split a 5 GB ML model between ElastiCache (features) and EFS (model), not understanding ML inference requires in-memory data.

**The Mistake:**
```
Question: 5 GB ML inference (2 GB features + 3 GB model), 100ms latency requirement
Your Answer: Store features in ElastiCache + model in EFS + 4 GB Lambda ❌
Correct Answer: 6 GB Lambda memory + cache both in /tmp ✅

Why this is architecturally broken:
├─ ML inference performs tensor operations on data IN MEMORY
├─ Can't fetch features from Redis during inference (adds 1-5ms per lookup × hundreds of features)
├─ Can't load model from EFS during request (takes seconds, not milliseconds)
├─ 4 GB Lambda memory insufficient (3 GB model + overhead + inference = need 6 GB+)
└─ You created a Rube Goldberg machine that can't possibly meet 100ms SLA
```

**ML Inference Requirements:**
```
Machine Learning Inference Must Have:
1. Model loaded IN MEMORY (can't run inference from external storage)
2. Features IN MEMORY (tensor operations require local data)
3. One-time load cost acceptable (cold start loads once, serves thousands of requests)
4. Sufficient memory for model + features + inference overhead

Lambda ML Inference Decision Tree:
Model + Features + Overhead < 10 GB?
├─ YES → Lambda with enough memory
│  ├─ Configure Lambda memory: data size + 20-30% overhead
│  ├─ Download from S3 to /tmp on cold start
│  ├─ Load into memory for inference
│  └─ Reuse across warm invocations
│
└─ NO (> 10 GB) → Lambda NOT suitable
   └─ Use SageMaker Endpoint (purpose-built for ML inference)
```

**Q8 Correct Solution:**
```
Requirements: 5 GB total (2 GB features + 3 GB model), 100ms latency

Correct Architecture:
├─ Lambda memory: 6 GB (5 GB data + 1 GB overhead)
├─ On cold start:
│  ├─ Download features (2 GB) from S3 to /tmp
│  ├─ Download model (3 GB) from S3 to /tmp
│  └─ Load both into Lambda memory (takes 10-15 seconds)
├─ On each request (warm invocation):
│  ├─ Features already in memory: 0ms load time
│  ├─ Model already in memory: 0ms load time
│  ├─ Perform inference: 50-80ms
│  └─ Total: Well under 100ms ✅
└─ Cost: Lambda memory charges only (most cost-effective)

Why this works:
├─ 5 GB < 10 GB Lambda limit ✅
├─ In-memory inference meets 100ms requirement ✅
├─ Cold start acceptable (happens rarely) ✅
└─ Simple architecture (no external dependencies) ✅
```

**Why Your Split Architecture Failed:**
```
ElastiCache (features) + EFS (model) + 4 GB Lambda:

Problems:
├─ Feature lookup latency:
│  └─ Fetching 100-500 features from Redis: 1-5ms × 500 features = 500ms - 2500ms
│  └─ Blows past 100ms budget before inference even starts ❌
│
├─ Model loading:
│  └─ Loading 3 GB model from EFS into memory: 5-10 seconds
│  └─ Can't do this per request (violates 100ms SLA) ❌
│  └─ Would need to load once on cold start anyway (so why use EFS?)
│
├─ Memory insufficient:
│  └─ 4 GB Lambda - 3 GB model = 1 GB for features + inference
│  └─ 2 GB features + inference overhead won't fit ❌
│
├─ Complexity:
│  ├─ Need to manage Redis connection pooling
│  ├─ Need to serialize/deserialize features
│  ├─ Need to mount EFS in VPC
│  └─ High operational overhead ❌
│
└─ Cost:
   ├─ Redis: ~$30-50/month
   ├─ EFS: ~$0.30/GB = ~$1/month
   ├─ Lambda: Same cost as simple solution
   └─ Total: More expensive AND doesn't work ❌
```

**The Pattern You Missed:**
```
ML Inference with Lambda:

IF model + features < 10 GB:
└─ Load EVERYTHING into Lambda memory
   └─ Download from S3 on cold start
   └─ Cache in /tmp and load to memory
   └─ NEVER split across external services

IF model + features > 10 GB:
└─ Lambda is NOT the answer
   └─ Use SageMaker Endpoint
   └─ Or use EC2/ECS with larger instance types
```

**Exam Keywords for ML Inference:**
- "ML model inference" + "< 10 GB" → Lambda with sufficient memory
- "TensorFlow model" + "features" → Load ALL data into Lambda memory
- "Real-time inference" + "< 1 second latency" → In-memory processing required
- NEVER see: "Split ML model across services" (this doesn't work)

**Target:** Understand ML inference requires ALL data in-memory, can't be split across external services

---

### 📊 Performance Analysis Summary

**Accuracy by Category:**
```
Lambda Memory/Limits: 100% (1/1) ✅
├─ Q9: Correctly increased memory for "memory exceeded" error

RDS Integration: 100% (1/1) ✅
├─ Q3: Correctly identified RDS Proxy for connection pooling

ElastiCache Use Cases: 33% (1/3) ⚠️
├─ Q1: Correctly used for 12 GB + 10ms latency ✅
├─ Q7: Incorrectly used for 500 MB static data ❌ (should be /tmp)
└─ Q10: Missed entirely, chose failing /tmp solution ❌

EFS Use Cases: 50% (1/2) ⚠️
├─ Q2: Correctly used for 6 GB ML model ✅
└─ Q4: Incorrectly used for 80 MB files ❌ (should be /tmp)
   Q5: Incorrectly used for 15 GB + 200ms ❌ (should be ElastiCache)
   Q8: Incorrectly mixed with ElastiCache ❌ (should be /tmp only)

/tmp Storage: 0% (0/4) 🚨 CRITICAL FAILURE
├─ Q4: Chose EFS instead ❌
├─ Q7: Chose ElastiCache instead ❌
├─ Q8: Chose ElastiCache+EFS split instead ❌
└─ Q10: Chose /tmp (finally!) but it was WRONG for this scenario ❌

DynamoDB Architecture: 0% (0/1) 🚨
└─ Q6: Confused connectionless DynamoDB with connection-based RDS ❌
```

**Critical Patterns Missed:**
1. **Reading comprehension** - Chose solution explicitly described as failing (Q10)
2. **/tmp use cases** - Don't understand when it's appropriate vs when it fails
3. **SLA analysis** - Ignored that cold starts violate strict SLA requirements
4. **Service connection models** - Confused DynamoDB (connectionless) with RDS (connection-based)
5. **ML inference architecture** - Tried to split data across services when it must be in-memory
6. **Cost vs performance trade-offs** - Chose expensive ElastiCache when free /tmp worked (Q7)
7. **Overcorrection** - Swung between extremes instead of systematic analysis

---

### 🎯 Immediate Action Required

**Before attempting ANY more quizzes:**

1. **Read and memorize the Lambda Storage Decision Framework** (in Weakness #14 above)
2. **Create flashcards for:**
   - When /tmp works vs when it fails
   - DynamoDB (connectionless) vs RDS (connection-based)
   - ML inference data locality requirements
   - ElastiCache cost ($30-50/month) vs /tmp cost (~$0)

3. **Practice reading comprehension:**
   - Read ENTIRE scenario before looking at options
   - Identify current state (what's being used now)
   - Identify failure mode (WHY it's failing)
   - Never choose solution that matches described failure

4. **Re-take Quick-Reference-Compute.md section on Lambda**
5. **Re-take Quick-Reference-Storage.md section on ephemeral storage**

**Do NOT attempt next drill until you can answer these without hesitation:**
- When does /tmp caching FAIL? (Answer: Strict SLA + cold starts violate SLA, OR high update frequency, OR high request rate)
- Does DynamoDB have connection limits? (Answer: NO - it's HTTP REST API, connectionless)
- Can you split an ML model between ElastiCache and EFS? (Answer: NO - inference requires all data in-memory)
- A 300 MB dataset with 50ms SLA and 10K req/min - ElastiCache or /tmp? (Answer: ElastiCache - /tmp cold starts violate SLA)

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
