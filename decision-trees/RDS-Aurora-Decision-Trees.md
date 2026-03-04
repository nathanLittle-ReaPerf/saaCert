# RDS & Aurora Decision Trees - Day 20 Recovery

**Created:** January 25, 2026
**Purpose:** Fix catastrophic 47% RDS & Aurora quiz performance
**Target:** 90%+ on recovery drill before advancing

---

## 🚨 Critical Weakness #1: Aurora Serverless v1 vs v2

### Decision Tree: Which Aurora Serverless?

```
Does workload need to scale to ZERO (0 ACUs) for cost savings?
│
├─ YES → Aurora Serverless v1
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
└─ NO → Aurora Serverless v2
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

### 🎯 Exam Flashcard

**Front:** "Serverless database that scales to ZERO for development/testing workloads?"
**Back:** Aurora Serverless v1 (v2 does NOT scale to zero - it's for production workloads)

**Front:** "Production e-commerce app with variable traffic needs instant scaling?"
**Back:** Aurora Serverless v2 (v1 too slow to wake from zero, v2 instant sub-second scaling)

---

## 🚨 Critical Weakness #2: Cost Optimization Patterns

### Decision Tree: "MOST cost-effective" Questions

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

### 🎯 Exam Flashcard

**Front:** "Analytics team needs to run queries on production RDS without impacting OLTP performance. MOST cost-effective?"
**Back:** Create Read Replica (cheaper than separate cluster, isolates workload, async replication)

**Front:** "Development team needs database for testing new features. MOST cost-effective?"
**Back:** Aurora Serverless v1 (scales to ZERO when not in use - maximum cost savings)

**Front:** "Can ElastiCache solve database WRITE scaling issues?"
**Back:** NO! ElastiCache only helps READ operations (caching). For write scaling, use sharding or different database.

---

## 🚨 Critical Weakness #3: "LEAST Operational Overhead" Hierarchy

### Decision Tree: Managed Services Hierarchy

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

### 🎯 Exam Flashcard

**Front:** "Company wants to migrate Oracle database to AWS. LEAST operational overhead?"
**Back:** Amazon RDS for Oracle (managed service - automated backups, patching, Multi-AZ failover)

**Front:** "When to use Oracle on EC2 instead of RDS for Oracle?"
**Back:** Only when you need features RDS doesn't support (Oracle RAC, specific plugins, direct OS access)

**Front:** "Operational overhead ranking (low to high)?"
**Back:**
1. Aurora Serverless (lowest)
2. RDS Multi-AZ
3. EC2 with managed backups
4. Self-managed EC2 (highest)

---

## 🚨 Critical Weakness #4: Aurora Global Database vs Cross-Region Read Replicas

### Decision Tree: Cross-Region Replication

```
Need cross-region disaster recovery or read scaling?
│
├─ Replication lag requirement < 1 SECOND?
│   └─ YES → Aurora Global Database
│       • <1 second typical replication lag
│       • Up to 16 Read Replicas per secondary region
│       • Fast failover to secondary region (<1 min RPO)
│       • Best for: Global apps, DR with strict RTO/RPO
│
└─ NO (can tolerate seconds of lag) → Cross-Region Read Replica
    └─ Use Cases:
        • Regional read scaling (not DR)
        • Lower cost than Global Database
        • Simpler setup (just create replica)
        • Async replication (seconds lag acceptable)
```

### 🎯 Exam Flashcard

**Front:** "Global application needs <1 second replication lag to multiple regions?"
**Back:** Aurora Global Database (sub-second replication, up to 5 secondary regions)

**Front:** "Difference between Aurora Global Database and Cross-Region Read Replica?"
**Back:**
- **Global DB:** <1 sec lag, fast DR failover, up to 5 regions
- **Cross-Region RR:** Seconds lag, regional read scaling, simpler/cheaper

---

## 🚨 Critical Weakness #5: Aurora Fast Recovery Patterns

### Decision Tree: Fast Database Recovery

```
Database corruption or data error occurred. Need to recover quickly (<10 min)?
│
├─ Know EXACT point in time (specific timestamp)?
│   └─ YES → Aurora Backtrack (MySQL only)
│       • Rewind database IN PLACE (no new cluster)
│       • <10 minutes to any point in backtrack window
│       • No data loss, instant rollback
│       • Use Case: "Oops, I ran DELETE without WHERE clause!"
│
├─ Need to TEST recovery before promoting?
│   └─ YES → Aurora Clone
│       • Copy-on-write clone (fast, cheap)
│       • Test recovery on clone without affecting prod
│       • Promote clone if recovery successful
│       • Only pay for changed data
│
└─ Need new production cluster from backup?
    └─ Point-In-Time Restore (PITR)
        • Creates NEW cluster from backup
        • Slower than Backtrack (full restore)
        • Any RDS/Aurora database (not just MySQL)
```

### 🎯 Exam Flashcard

**Front:** "Accidental DELETE query on Aurora MySQL. Need to recover in <10 minutes?"
**Back:** Aurora Backtrack (rewinds database in place to point before DELETE)

**Front:** "Want to test database recovery strategy before promoting to production?"
**Back:** Aurora Clone (copy-on-write, test on clone, promote if successful)

**Front:** "Aurora Backtrack limitations?"
**Back:**
- MySQL ONLY (not PostgreSQL)
- Must be enabled at cluster creation
- Limited backtrack window (default 24-72 hours)

---

## 🚨 Critical Weakness #6: Compliance & Data Isolation

### Decision Tree: Separate Databases vs Schemas

```
Compliance requires "separate database" for each tenant/department?
│
├─ What does "separate database" mean in context?
│
│   ├─ Option A: Separate SCHEMAS in single RDS instance
│   │   • Lower cost (shared infrastructure)
│   │   • Logical separation only
│   │   • ❌ May not meet strict compliance (shared OS, same instance)
│   │
│   ├─ Option B: Separate RDS INSTANCES
│   │   • Higher cost (multiple instances)
│   │   • Strong isolation (separate OS, compute, storage)
│   │   • ✅ Meets most compliance requirements
│   │
│   └─ Option C: Aurora with separate clusters
│       • Each tenant = separate Aurora cluster
│       • Maximum isolation + compliance
│       • Highest cost, most operational overhead
│
└─ 🎯 EXAM TIP: "Separate database" usually means separate RDS instance or cluster
    (NOT just schemas - exam wants physical/logical isolation)
```

### 🎯 Exam Flashcard

**Front:** "Compliance requires 'data stored in separate database for each client.' What does this mean?"
**Back:** Separate RDS instances or Aurora clusters (NOT just schemas in single instance)

**Front:** "Aurora Multi-Master - how many regions?"
**Back:** SINGLE REGION ONLY (all write nodes must be in same region for consistency)

---

## 📊 Quick Reference: Aurora Feature Matrix

| Feature | Aurora Serverless v1 | Aurora Serverless v2 | Aurora Provisioned | Aurora Global DB |
|---------|---------------------|---------------------|-------------------|-----------------|
| **Scales to Zero** | ✅ YES | ❌ NO | ❌ NO | ❌ NO |
| **Instant Scaling** | ❌ NO (slow wake) | ✅ YES (sub-second) | ❌ NO (manual) | ✅ YES |
| **Production Ready** | ⚠️ Dev/Test only | ✅ YES | ✅ YES | ✅ YES |
| **MySQL 8.0 Support** | ❌ NO (5.6/5.7) | ✅ YES | ✅ YES | ✅ YES |
| **Cross-Region** | ❌ NO | ❌ NO | ⚠️ Read Replicas | ✅ <1 sec lag |
| **Best For** | Infrequent workloads | Variable production | Predictable load | Global apps |

---

## 🎯 Pattern Recognition for Exam

### Cost Optimization Keywords → Answers

- **"MOST cost-effective"** → Use existing resources (Read Replicas, not new clusters)
- **"Minimize costs"** → Right-size, avoid over-provisioning
- **"Development environment"** → Aurora Serverless v1 (scales to zero)
- **"Pay only for usage"** → Serverless (v1 or v2 depending on requirements)

### Operational Overhead Keywords → Answers

- **"LEAST operational overhead"** → Managed services (RDS, Aurora, DynamoDB)
- **"Minimal management"** → Avoid EC2, choose Fargate or serverless
- **"Automated backups/patching"** → RDS/Aurora (not self-managed EC2)
- **"No server management"** → Aurora Serverless, DynamoDB, Lambda

### Performance Keywords → Answers

- **"<1 second replication lag"** → Aurora Global Database
- **"Instant scaling"** → Aurora Serverless v2 (NOT v1)
- **"Sub-10 minute recovery"** → Aurora Backtrack or Aurora Clone
- **"Isolate read workload"** → Read Replicas (OLAP on replica, OLTP on primary)

---

## 🔥 Common Exam Traps (DON'T FALL FOR THESE!)

### ❌ Trap #1: ElastiCache for Write Scaling
**Question:** "Database experiencing high write load. How to scale?"
**Wrong Answer:** Add ElastiCache cluster
**Why Wrong:** ElastiCache only helps READS (caching). Doesn't solve write scaling.
**Right Answer:** Use write-optimized database (DynamoDB) or shard RDS

### ❌ Trap #2: Aurora Serverless v1 for Production
**Question:** "Production e-commerce app with variable traffic needs serverless database?"
**Wrong Answer:** Aurora Serverless v1
**Why Wrong:** v1 too slow to wake from zero (bad for production)
**Right Answer:** Aurora Serverless v2 (instant scaling, production-ready)

### ❌ Trap #3: Over-Engineering Cost Solutions
**Question:** "Analytics team needs to query production database. MOST cost-effective?"
**Wrong Answer:** Create separate Aurora Serverless v2 cluster for analytics
**Why Wrong:** Unnecessary cost - two clusters instead of one
**Right Answer:** Create Read Replica from existing RDS (cheaper, isolates workload)

### ❌ Trap #4: EC2 When Managed Service Exists
**Question:** "Migrate Oracle database to AWS. LEAST operational overhead?"
**Wrong Answer:** Oracle on EC2 with Data Guard
**Why Wrong:** High operational overhead (OS patching, backups, failover setup)
**Right Answer:** Amazon RDS for Oracle (managed service, automated everything)

---

## 🎯 Recovery Drill Preview

Your next quiz will focus on these exact patterns:

1. **Aurora Serverless v1 vs v2 scenarios** (3 questions)
2. **Cost optimization ranking** (2 questions)
3. **Operational overhead hierarchy** (2 questions)
4. **Aurora Global Database vs Read Replicas** (1 question)
5. **Aurora Clone/Backtrack recovery** (1 question)
6. **ElastiCache limitations** (1 question)

**Target:** 9/10 (90%) minimum before advancing to Day 21 content.

---

## 📝 Memorization Checklist

Before taking recovery drill, you should be able to answer these instantly:

- [ ] Aurora Serverless v1 scales to zero? (YES)
- [ ] Aurora Serverless v2 scales to zero? (NO)
- [ ] Can ElastiCache help write scaling? (NO - reads only)
- [ ] Aurora Global Database replication lag? (<1 second)
- [ ] Aurora Backtrack database engines? (MySQL only, not PostgreSQL)
- [ ] Aurora Clone use case? (Test recovery before promoting)
- [ ] RDS vs EC2 for Oracle - which has LEAST operational overhead? (RDS)
- [ ] Most cost-effective analytics solution for RDS? (Read Replica, not new cluster)
- [ ] Aurora Multi-Master supports multiple regions? (NO - single region only)
- [ ] "Separate database" for compliance means? (Separate RDS instances/clusters)

**When you can answer all 10 instantly, you're ready for the recovery drill.**
