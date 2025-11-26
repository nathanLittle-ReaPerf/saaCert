# Weak Areas Cheat Sheet - MEMORIZE THIS

**Your problem areas from quiz performance. Study this until you can recite it in your sleep.**

---

## 🚨 CRITICAL: ALB vs NLB - Static IP Question

**This is what keeps tripping you up:**

| Feature | ALB | NLB |
|---------|-----|-----|
| **Static IP Support** | ❌ **NO** (dynamic DNS only) | ✅ **YES** (one per AZ + Elastic IP) |
| **Protocol** | HTTP/HTTPS only | TCP/UDP/TLS |
| **Performance** | Thousands req/sec | **Millions** req/sec |
| **Latency** | Higher | **Ultra-low (< 100ms)** |
| **Path Routing** | ✅ YES | ❌ NO |
| **Lambda Targets** | ✅ YES (only ALB!) | ❌ NO |

### The Question Pattern That Gets You:

**If the question mentions:**
- ✅ "Static IP" → **NLB** (ALB is eliminated immediately)
- ✅ "Elastic IP" → **NLB** (ALB can't do this)
- ✅ "Whitelist IP addresses" → **NLB** (need static IPs)
- ✅ "Millions of requests per second" → **NLB**
- ✅ "Ultra-low latency" → **NLB**
- ✅ "UDP protocol" → **NLB**

**If the question mentions:**
- ✅ "Path-based routing" (`/api/*`, `/images/*`) → **ALB**
- ✅ "Lambda targets" → **ALB** (only ALB supports this)
- ✅ "Authenticate with Cognito" → **ALB**
- ✅ "HTTP/HTTPS only" → **ALB**

### Your Mistake Pattern:
- You see "high performance" or "TCP" and think it could be ALB
- **STOP DOING THIS**
- If question says **"static IP"** → ALB is **automatically eliminated**

**Memory trick:**
- **ALB = Application Layer = Smart** (sees URLs, routes by path, calls Lambda)
- **NLB = Network Layer = Fast & Fixed** (static IPs, extreme speed, TCP/UDP)

---

## 🚨 CRITICAL: S3 Storage Classes & Retrieval Times

**You keep choosing the wrong tier for retrieval requirements. MEMORIZE THIS TABLE:**

| Storage Class | Retrieval Time | Cost ($/GB/month) | Use Case |
|--------------|----------------|-------------------|----------|
| **S3 Standard** | Milliseconds | $0.023 | Frequent access |
| **S3 Standard-IA** | **Milliseconds** | $0.0125 | Infrequent but **immediate** access needed |
| **S3 Glacier Instant** | Milliseconds | $0.004 | Archive with instant retrieval |
| **S3 Glacier Flexible (Expedited)** | **1-5 minutes** | $0.0036 + retrieval fee | Rare access, can wait minutes |
| **S3 Glacier Flexible (Standard)** | **3-5 hours** | $0.0036 | Rare access, can wait hours |
| **S3 Glacier Deep Archive** | **12-48 hours** | $0.00099 (CHEAPEST) | Almost never accessed, compliance |

### Decision Tree for Your Quiz Mistakes:

**Question says: "Retrieve within minutes"**
- ✅ **Standard-IA** (milliseconds = well within minutes, no extra cost)
- ❌ NOT Glacier Flexible (needs Expedited retrieval = extra cost)

**Question says: "Retrieve within hours" or "12-hour retrieval acceptable"**
- ✅ **Glacier Flexible Retrieval** (Standard: 3-5 hours)
- ✅ **Glacier Deep Archive** if "cheapest" mentioned (12-48 hours)

**Question says: "Rarely accessed" + "7-year retention" + "compliance"**
- ✅ **Glacier Deep Archive** (cheapest, designed for compliance)

**Question says: "Occasionally accessed" + "quick retrieval needed"**
- ✅ **Standard-IA** (millisecond retrieval, 45% cheaper than Standard)

### Your Mistake Pattern:
- You jump to Glacier too quickly when you see "rarely accessed"
- Check the **retrieval time requirement FIRST**
- "Within minutes" = Standard-IA, NOT Glacier

**Memory trick:**
- **Standard/Standard-IA = Instant** (milliseconds)
- **Glacier Flexible = Minutes to Hours** (1-5 min expedited, 3-5 hr standard)
- **Glacier Deep Archive = Half a day to 2 days** (12-48 hours)

---

## 🚨 S3 Lifecycle Transitions - ONE WAY ONLY

**You got this right, but don't forget it:**

### Allowed Transitions (Waterfall - One Direction Only):
```
S3 Standard
    ↓ ✅
S3 Standard-IA or S3 One Zone-IA
    ↓ ✅
S3 Glacier Flexible Retrieval
    ↓ ✅
S3 Glacier Deep Archive
```

### NOT Allowed (Can't Go Backwards):
```
S3 Glacier → S3 Standard ❌
S3 Glacier → S3 Standard-IA ❌
Any colder tier → Any warmer tier ❌
```

**Note:** You CAN manually restore from Glacier using S3 Restore API, but you CANNOT automate it with lifecycle policy.

### Minimum Storage Durations:
- **Standard**: No minimum
- **Standard-IA**: 30 days
- **One Zone-IA**: 30 days
- **Glacier Flexible**: 90 days
- **Glacier Deep Archive**: 180 days

**If you delete before minimum duration, you pay for the full minimum period.**

---

## 🚨 Spot Instance Best Practices

**You chose single AZ (WRONG). Here's the rule:**

### To Minimize Spot Interruptions:

**✅ DO:**
- Diversify across **multiple instance types** (m5.large, m5.xlarge, m6i.large, c5.large)
- Diversify across **multiple Availability Zones** (us-east-1a, us-east-1b, us-east-1c)
- Use **Spot Fleet** or **Auto Scaling with mixed instances policy**

**❌ DON'T:**
- Use single instance type (fewer capacity pools = higher interruption risk)
- Use single AZ (if that AZ loses capacity, entire workload interrupted)
- Bid at On-Demand price (defeats the purpose, still not guaranteed)

### Why Diversification Works:
- AWS reclaims Spot capacity when needed for On-Demand
- If one instance type in one AZ is reclaimed, others continue running
- More capacity pools = lower overall interruption rate

**Your mistake:** You thought single AZ reduces latency (true) but increases interruption risk (bad for Spot)

**Exam keywords:** "Minimize interruptions" + "Spot Instances" → **Diversify across types and AZs**

**Memory trick:** Spot = Spread (diversify), NOT concentrate

---

## 🚨 Health Checks: EC2 vs ELB

**You got this right, but it's critical for the exam:**

| Health Check Type | What It Checks | Detects Application Crashes? |
|------------------|----------------|----------------------------|
| **EC2 Status Checks** | Instance running, network reachable, hardware OK | ❌ **NO** |
| **ELB Health Checks** | Application endpoint responding (`/health`, `/ping`) | ✅ **YES** |

### The Scenario That Shows Up:
- Application crashes but EC2 instance keeps running
- EC2 checks: PASS ✅ (instance is up)
- Application: CRASHED ❌ (returns 502/503/504 errors)
- ASG with EC2 checks: Doesn't terminate instance (thinks it's healthy)
- Result: Traffic routed to broken instances → intermittent errors

### Solution:
- Configure ASG to use **ELB health checks** instead of EC2 status checks
- ELB tests application health, detects crashes
- ASG terminates and replaces unhealthy instances

**Exam pattern:** "Application crashes but instance running" + "intermittent 502 errors" → **Use ELB health checks**

**Memory trick:** EC2 checks = "Is the server on?" / ELB checks = "Is the app working?"

---

## 🚨 Cross-Zone Load Balancing Costs

**You got this right, but know it cold:**

| Load Balancer | Cross-Zone Default | Charges for Cross-Zone |
|--------------|-------------------|----------------------|
| **ALB** | ✅ Enabled | **FREE** ✅ |
| **NLB** | ❌ Disabled | **COSTS MONEY** 💰 |
| **GLB** | ❌ Disabled | **COSTS MONEY** 💰 |

### What Cross-Zone Load Balancing Does:
- **Without**: Traffic distributed evenly to **AZs**, then to instances in that AZ
  - Problem: Uneven distribution if AZs have different instance counts
- **With**: Traffic distributed evenly to **ALL instances** regardless of AZ
  - Benefit: Perfect distribution across all instances

### Exam Pattern:
- "Uneven traffic distribution across AZs" + "NLB" → **Enable cross-zone (charges apply)**
- Cost question mentioning NLB data transfer → **Cross-zone load balancing costs**

**Memory trick:**
- **ALB = Always on, Always free** (cross-zone enabled & free)
- **NLB = Not on, Not free** (cross-zone disabled & costs money)

---

## 🚨 EC2 Reserved Instances: Standard vs Convertible

**You got this right, but memorize the difference:**

| Type | Discount | Can Change Instance Family? | Use Case |
|------|----------|---------------------------|----------|
| **Standard RI** | Up to 72% | ❌ NO (only size within family) | Know exact requirements, max savings |
| **Convertible RI** | Up to 66% | ✅ YES (full flexibility) | May need to change types, more flexibility |

### What You CAN Change:

**Standard RI:**
- ✅ Instance size within same family (m5.large → m5.xlarge)
- ✅ Availability Zone
- ❌ Instance family (m5 → c6i) ❌

**Convertible RI:**
- ✅ Instance family (m5 → c6i → r6i)
- ✅ Instance type
- ✅ OS (Linux → Windows)
- ✅ Tenancy (shared → dedicated)

**Exam pattern:** "May need to change instance types in future" → **Convertible RI**

---

## 📝 Quick Self-Test (Answer Without Looking!)

1. Does ALB support static IP addresses? **Answer: NO**
2. Which load balancer can target Lambda functions? **Answer: ALB (only ALB)**
3. What's the retrieval time for S3 Standard-IA? **Answer: Milliseconds**
4. What's the retrieval time for S3 Glacier Deep Archive? **Answer: 12-48 hours**
5. Can you create a lifecycle rule to transition from Glacier to Standard? **Answer: NO (one-way only)**
6. To minimize Spot interruptions, should you use single AZ or multiple AZs? **Answer: Multiple AZs (diversify)**
7. Which health check detects application crashes: EC2 or ELB? **Answer: ELB**
8. Is cross-zone load balancing free for NLB? **Answer: NO (costs money)**
9. Is cross-zone load balancing free for ALB? **Answer: YES (free)**
10. Which RI type allows changing instance family? **Answer: Convertible RI**

**If you got less than 9/10 correct, read this sheet again.**

---

## 🎯 Exam Day Strategy for Your Weak Areas

### When You See a Load Balancer Question:

**Step 1:** Look for "static IP" or "Elastic IP"
- If YES → NLB (eliminate ALB immediately)
- If NO → Check protocol

**Step 2:** Check protocol
- HTTP/HTTPS → probably ALB
- TCP/UDP → probably NLB
- Third-party appliances → GLB

**Step 3:** Check routing requirements
- Path routing (`/api/*`) → ALB only
- Lambda targets → ALB only
- No routing needed → NLB for performance

### When You See an S3 Storage Class Question:

**Step 1:** Check retrieval time requirement FIRST
- "Within minutes" or "immediate" → Standard-IA
- "Within hours" → Glacier Flexible
- "12+ hours acceptable" → Glacier Deep Archive

**Step 2:** Check access pattern
- Frequent → Standard
- Infrequent but immediate → Standard-IA
- Rare + hours OK → Glacier Flexible
- Compliance + rarely accessed → Glacier Deep Archive

**Step 3:** Verify cost optimization
- Cheapest always = Glacier Deep Archive
- Balance cost + retrieval = Standard-IA or Glacier Flexible

---

## ⚡ The "Can't Get This Wrong" List

**MEMORIZE THESE FACTS:**

1. **ALB has NO static IP** (dynamic DNS only) - If question needs static IP, ALB is eliminated
2. **Only ALB can target Lambda** - No other load balancer can do this
3. **S3 Standard-IA retrieval = milliseconds** - NOT minutes, NOT hours
4. **S3 Glacier Deep Archive = cheapest** - Always the answer for "MOST cost-effective" long-term archive
5. **Lifecycle transitions are one-way** - Can't automate Glacier → Standard
6. **Spot diversification = multiple types + multiple AZs** - NOT single AZ
7. **ELB health checks detect app crashes, EC2 checks don't** - "App crashed but instance running" = use ELB checks
8. **NLB cross-zone costs money, ALB cross-zone is free** - Critical for cost questions
9. **Convertible RI can change instance family, Standard RI cannot** - "May need to change types" = Convertible
10. **Target Tracking = least overhead** - Simplest scaling policy

---

## 🚀 Your Action Plan

**Tonight:**
1. Read this sheet 3 times (15 minutes)
2. Do the quick self-test without looking (5 minutes)
3. Sleep on it

**Tomorrow morning:**
1. Read this sheet again (10 minutes)
2. Retake the 15-question quiz
3. Target: 13/15 or better (87%+)

**If you score below 13/15:**
- Read this sheet again
- Take the quiz again
- Don't move to Day 4 until you hit 87%+

---

## 🆕 NEW WEAK AREAS (Nov 25 Quiz - 14/20, 70%)

### 🚨 CRITICAL: ALB Cross-Zone Costs (STILL GETTING THIS WRONG!)

**You got this wrong AGAIN. This is now on the "never miss again" list.**

| Load Balancer | Cross-Zone Default | Cost for Cross-Zone |
|--------------|-------------------|-------------------|
| **ALB** | ✅ Enabled by default | **FREE** ✅ (NO CHARGES) |
| **NLB** | ❌ Disabled by default | **COSTS MONEY** 💰 |
| **GLB** | ❌ Disabled by default | **COSTS MONEY** 💰 |

**Your mistake:** You keep thinking ALB cross-zone costs money (it does NOT - it's FREE)

**Memory trick:**
- **ALB = Always free** (cross-zone enabled by default, NO COST)
- **NLB/GLB = Not free** (cross-zone disabled, costs extra if enabled)

**Exam pattern:** "Additional charges for cross-zone load balancing" → **NLB or GLB** (NOT ALB!)

---

### 🚨 S3 Encryption: SSE-KMS vs SSE-C Audit Trail

**You picked SSE-C when the question asked about audit trail. WRONG.**

| Encryption Type | Audit Trail (CloudTrail Logs)? | Who Manages Keys? |
|----------------|-------------------------------|-------------------|
| **SSE-S3** | ❌ NO | AWS (fully managed) |
| **SSE-KMS** | ✅ **YES** (CloudTrail logs KMS API calls) | AWS KMS (you control) |
| **SSE-C** | ❌ NO | Customer (you provide with each request) |
| **Client-Side** | ❌ NO | Customer (encrypt before upload) |

**Your mistake:** SSE-C = Customer-provided keys, but NO audit trail
- SSE-C: You provide keys, AWS encrypts, no CloudTrail logs
- SSE-KMS: AWS KMS manages keys, CloudTrail logs all key usage

**Memory trick:**
- **SSE-KMS = Audit trail** (CloudTrail logs KMS API calls)
- **SSE-C = Customer keys only** (no audit trail)

**Exam keywords:** "Audit trail" or "CloudTrail logs for encryption" → **SSE-KMS** (NOT SSE-C)

---

### 🚨 S3 Bucket Policies vs Security Groups (Cross-Account Access)

**You picked Security Groups for cross-account S3 access. WRONG.**

**What Works for S3 Access Control:**

| Method | Use Case | Can Do Cross-Account? |
|--------|----------|---------------------|
| **Bucket Policies** | S3 access control (JSON, IAM-like) | ✅ YES |
| **IAM Policies** | User/Role permissions | ✅ YES (with bucket policy) |
| **Access Points** | Multi-team access patterns | ✅ YES |
| **Security Groups** | **EC2/VPC network traffic ONLY** | ❌ N/A (not for S3!) |

**Your mistake:** Security Groups are for EC2/VPC network-level access control, NOT S3
- Security Groups = Layer 4 network firewall (EC2, RDS, ELB)
- Bucket Policies = S3 access control (JSON policy document)

**Memory trick:**
- **Security Groups = Servers** (EC2, RDS, ELB network access)
- **Bucket Policies = Buckets** (S3 object/bucket access)

**Exam keywords:** "Cross-account S3 access" → **Bucket Policy** (NOT Security Groups!)

---

### 🚨 Storage Optimized Instance Types: i3 vs d2

**You picked i3 when the question asked for HDD-based storage. WRONG.**

**Storage Optimized Instance Types:**

| Instance Family | Storage Type | IOPS | Use Case |
|----------------|--------------|------|----------|
| **i3/i3en** | **NVMe SSD** | Very High (millions) | NoSQL databases, real-time analytics, caching |
| **d2** | **HDD** (spinning disks) | Moderate | Large sequential reads/writes (data warehouses, Hadoop, MapReduce) |
| **h1** | **HDD** | Moderate | MapReduce, HDFS, distributed file systems |

**Your mistake:** i3 = SSD (fast random access), d2 = HDD (large sequential datasets)
- i3: High IOPS, low latency, expensive (SSDs)
- d2: High throughput, lower cost, large capacity (HDDs)

**Memory trick:**
- **i3 = I/O intensive** (SSDs for databases, analytics)
- **d2 = Data warehouses** (HDDs for large sequential data)

**Exam keywords:** "Large datasets", "HDD-based", "sequential reads" → **d2** (NOT i3)

---

### 🚨 Partition Placement Groups (Hadoop/Kafka)

**You picked Cluster when the question mentioned Hadoop/Kafka. WRONG.**

**Placement Group Types:**

| Type | Use Case | Max Instances per AZ | Benefit |
|------|----------|---------------------|---------|
| **Cluster** | HPC, low-latency, single AZ | Limited | Lowest latency (10 Gbps network) |
| **Partition** | **Distributed apps (Hadoop, Kafka, Cassandra)** | 7 partitions per AZ | Fault isolation (each partition on separate rack) |
| **Spread** | Critical instances, max 7 per AZ | 7 per AZ | Strict isolation (each instance on separate hardware) |

**Your mistake:** Cluster = single rack (high performance), Partition = separate racks (fault tolerance)
- Cluster: All instances close together, low latency, but single point of failure
- Partition: Each partition on different rack, fault isolation for distributed systems

**Memory trick:**
- **Cluster = Close together** (HPC, low latency, single rack)
- **Partition = Partitioned across racks** (Hadoop/Kafka, fault tolerance)
- **Spread = Spread apart** (max 7, critical instances)

**Exam keywords:** "Hadoop", "Kafka", "HDFS", "distributed system" → **Partition** (NOT Cluster!)

---

### 🚨 Sticky Sessions: Duration-Based vs Application Cookie

**You picked session cookie when ALB uses duration-based. WRONG.**

**ALB Sticky Session Types:**

| Cookie Type | Who Creates? | Duration | Use Case |
|-------------|--------------|----------|----------|
| **Duration-Based (AWSALB)** | **Load balancer** | 1 second - 7 days | Default, simple sticky sessions |
| **Application-Based** | Your application | Custom (app-defined) | App needs custom session data in cookie |

**Your mistake:** ALB uses duration-based cookies by default (load balancer-generated)
- Duration-based: ALB creates cookie (`AWSALB`), simple duration setting
- Application-based: Your app creates cookie, ALB reads it for routing

**Memory trick:**
- **Duration-based = Default** (ALB creates AWSALB cookie, set duration)
- **Application-based = App creates** (custom app cookie)

**Exam keywords:** "Sticky sessions" + "ALB" → **Duration-based (AWSALB)** by default

---

## 📝 Updated Quick Self-Test (Answer Without Looking!)

**Original 10 questions (from before):**
1. Does ALB support static IP addresses? **Answer: NO**
2. Which load balancer can target Lambda functions? **Answer: ALB (only ALB)**
3. What's the retrieval time for S3 Standard-IA? **Answer: Milliseconds**
4. What's the retrieval time for S3 Glacier Deep Archive? **Answer: 12-48 hours**
5. Can you create a lifecycle rule to transition from Glacier to Standard? **Answer: NO (one-way only)**
6. To minimize Spot interruptions, should you use single AZ or multiple AZs? **Answer: Multiple AZs (diversify)**
7. Which health check detects application crashes: EC2 or ELB? **Answer: ELB**
8. Is cross-zone load balancing free for NLB? **Answer: NO (costs money)**
9. Is cross-zone load balancing free for ALB? **Answer: YES (FREE - never costs money!)**
10. Which RI type allows changing instance family? **Answer: Convertible RI**

**NEW questions (from latest quiz mistakes):**
11. Which encryption type provides audit trail via CloudTrail? **Answer: SSE-KMS**
12. For cross-account S3 access, use Security Groups or Bucket Policy? **Answer: Bucket Policy**
13. For HDD-based large datasets, use i3 or d2? **Answer: d2**
14. For Hadoop/Kafka distributed systems, use Cluster or Partition placement? **Answer: Partition**
15. ALB sticky sessions use which cookie type by default? **Answer: Duration-based (AWSALB)**

**If you got less than 13/15 correct, read the NEW sections again.**

---

## ⚡ Updated "Can't Get This Wrong" List

**MEMORIZE THESE FACTS (including new ones):**

1. **ALB has NO static IP** (dynamic DNS only) - If question needs static IP, ALB is eliminated
2. **Only ALB can target Lambda** - No other load balancer can do this
3. **S3 Standard-IA retrieval = milliseconds** - NOT minutes, NOT hours
4. **S3 Glacier Deep Archive = cheapest** - Always the answer for "MOST cost-effective" long-term archive
5. **Lifecycle transitions are one-way** - Can't automate Glacier → Standard
6. **Spot diversification = multiple types + multiple AZs** - NOT single AZ
7. **ELB health checks detect app crashes, EC2 checks don't** - "App crashed but instance running" = use ELB checks
8. **NLB cross-zone costs money, ALB cross-zone is FREE** - ⚠️ **YOU KEEP GETTING THIS WRONG!**
9. **Convertible RI can change instance family, Standard RI cannot** - "May need to change types" = Convertible
10. **Target Tracking = least overhead** - Simplest scaling policy
11. **SSE-KMS provides audit trail (CloudTrail logs), SSE-C does NOT** - ⚠️ **NEW: "Audit trail" = SSE-KMS**
12. **Bucket Policies for S3 cross-account, NOT Security Groups** - ⚠️ **NEW: Security Groups are for EC2/VPC only**
13. **d2 instances = HDD-based (data warehouses), i3 = SSD-based (databases)** - ⚠️ **NEW**
14. **Partition placement = Hadoop/Kafka, Cluster = HPC low latency** - ⚠️ **NEW**
15. **ALB sticky sessions = Duration-based (AWSALB) by default** - ⚠️ **NEW**

---

**You've got this. Now go memorize the NEW sections.** 💪
