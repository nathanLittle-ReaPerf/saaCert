# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** January 4, 2026, 1:30 PM CST (Post Day 1 Baseline Assessment - 75%)
**Exam Date:** February 11, 2026 at 5:15 PM EST (37 days remaining)
**Study Period:** January 5 - February 10, 2026 (37 days)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

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
