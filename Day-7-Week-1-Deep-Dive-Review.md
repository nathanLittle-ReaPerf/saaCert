# Week 1 Deep Dive Review - Weak Areas Bootcamp

**Purpose:** Fix the critical gaps identified in your Day 7 comprehensive quiz (scored 45%)
**Target:** Master these concepts to score 80%+ on retake
**Time Required:** 2-3 hours of focused study

---

## Critical Weak Area #1: S3 Storage Classes (4 Questions Missed)

### Your Current Problem
You're defaulting to Glacier/Deep Archive whenever you see "infrequent" or "rarely" accessed, WITHOUT checking retrieval time requirements.

### The Decision Tree You Need to Memorize

```
START: Need to store data in S3
│
├─ How often accessed?
│  │
│  ├─ FREQUENTLY (daily/weekly)
│  │  ├─ Known access patterns? → S3 Standard
│  │  └─ Unknown/changing patterns? → S3 Intelligent-Tiering
│  │
│  ├─ INFREQUENTLY (monthly) - "rarely accessed"
│  │  ├─ Need IMMEDIATE retrieval?
│  │  │  ├─ Can tolerate 30-day minimum? → S3 Standard-IA
│  │  │  └─ Less than 30 days storage? → S3 Standard (IA has 30-day minimum)
│  │  │
│  │  └─ Don't need immediate retrieval? → Consider Glacier
│  │
│  └─ ALMOST NEVER (yearly or less) - "archive"
│     ├─ Need within minutes-hours?
│     │  ├─ Within 1-5 minutes? → Glacier Instant Retrieval
│     │  ├─ Within 3-5 hours? → Glacier Flexible (Expedited: 1-5 min, Standard: 3-5 hrs)
│     │  └─ Within 12 hours? → Glacier Deep Archive (Standard: 12 hrs)
│     │
│     └─ Can wait 12+ hours? → Glacier Deep Archive (cheapest)
```

### Exam Pattern Recognition - CRITICAL KEYWORDS

| Keyword in Question | What It Means | Correct Answer |
|-------------------|---------------|----------------|
| "need them **immediately**" | 0 second retrieval time | Standard-IA or Standard (NOT Glacier) |
| "rarely accessed" | Monthly access pattern | Standard-IA if immediate needed |
| "almost never accessed" | Yearly or less | Glacier family |
| "**unknown** access patterns" | Can't predict when/how often | Intelligent-Tiering |
| "accessed **unpredictably**" | Random access times | Intelligent-Tiering |
| "accessed a **few times per year**" | Archive pattern | Glacier Instant/Flexible |
| "must retrieve within **5 minutes**" | Fast archive retrieval | Glacier Flexible (Expedited) |
| "acceptable to wait **12 hours**" | Slow archive retrieval | Glacier Deep Archive |

### The Traps You Keep Falling Into

#### ❌ TRAP #1: "Rarely = Glacier"
**Wrong Thinking:** Question says "rarely accessed" → Pick Glacier
**Correct Thinking:** Check retrieval requirement FIRST

**Example from Quiz:**
- "Compliance documents accessed only a few times per year, **but need them immediately** when requested"
- You picked: Glacier Flexible with Expedited Retrieval
- Correct answer: Standard-IA (immediate = 0 seconds, Glacier Expedited = 1-5 minutes)

#### ❌ TRAP #2: "Expedited Retrieval = Immediate"
**Wrong:** Glacier Expedited (1-5 minutes) is NOT "immediate"
**Right:** Only Standard and Standard-IA provide true immediate (0 second) retrieval

#### ❌ TRAP #3: Ignoring Minimum Storage Duration
| Storage Class | Minimum Duration | What This Means |
|--------------|------------------|-----------------|
| Standard | None | Any duration works |
| Standard-IA | **30 days** | Charged for 30 days even if deleted sooner |
| Intelligent-Tiering | **30 days** | Same as Standard-IA |
| Glacier Instant | **90 days** | Charged for 90 days minimum |
| Glacier Flexible | **90 days** | Charged for 90 days minimum |
| Glacier Deep Archive | **180 days** | Charged for 180 days minimum |

**Example Trap:**
- Question: "Temporary files stored for 2 weeks, accessed once, then deleted"
- Wrong: Standard-IA (you'd pay for 30 days even though you only store 14 days)
- Right: S3 Standard (no minimum duration)

### Retrieval Time Comparison - MEMORIZE THIS

| Storage Class | Retrieval Time | Cost |
|--------------|---------------|------|
| **S3 Standard** | Milliseconds (immediate) | $0.023/GB |
| **S3 Standard-IA** | Milliseconds (immediate) | $0.0125/GB + $0.01/GB retrieval |
| **S3 Intelligent-Tiering** | Milliseconds (immediate) | $0.023/GB frequent, $0.0125/GB infrequent |
| **Glacier Instant Retrieval** | Milliseconds (immediate) | $0.004/GB + $0.03/GB retrieval |
| **Glacier Flexible - Expedited** | 1-5 minutes | $0.004/GB + $0.03/GB retrieval |
| **Glacier Flexible - Standard** | 3-5 hours | $0.004/GB + $0.01/GB retrieval |
| **Glacier Deep Archive - Standard** | 12 hours | $0.00099/GB + $0.02/GB retrieval |

### Practice Decision Making

**Scenario 1:** Financial records accessed maybe twice a year for audits, but auditors need them within seconds when they ask.
**Your Answer:** ________________
**Correct:** Glacier Instant Retrieval (archive pattern + immediate retrieval)

**Scenario 2:** Log files that might be analyzed or might never be touched - completely unpredictable.
**Your Answer:** ________________
**Correct:** Intelligent-Tiering (unknown/unpredictable pattern)

**Scenario 3:** Backup files accessed monthly for testing, need them immediately when disaster recovery drill happens.
**Your Answer:** ________________
**Correct:** Standard-IA (monthly = infrequent, immediate = no Glacier)

**Scenario 4:** Archived videos from 5 years ago, legal requirement to keep for 7 years, acceptable to wait 12 hours if ever needed.
**Your Answer:** ________________
**Correct:** Glacier Deep Archive (almost never + can wait = cheapest)

---

## Critical Weak Area #2: VPC NACLs - Stateless Behavior (3 Questions Missed)

### Your Current Problem
You keep treating NACLs like Security Groups. **THEY ARE FUNDAMENTALLY DIFFERENT.**

### Security Groups vs NACLs - THE CRITICAL DIFFERENCE

| Feature | Security Groups | NACLs |
|---------|----------------|-------|
| **Stateful/Stateless** | **STATEFUL** | **STATELESS** |
| **What that means** | Return traffic automatically allowed | **Return traffic must be explicitly allowed** |
| **Example** | Allow inbound 443 → Outbound return traffic auto-allowed | Allow inbound 443 → Must ALSO allow outbound 1024-65535 |
| **Rules** | Allow rules only | **Allow AND Deny rules** |
| **Rule Processing** | All rules evaluated | **Rules processed in order** (lowest number first) |
| **Applies to** | Instance level | **Subnet level** |

### The Ephemeral Ports Problem - THIS IS WHY YOU KEEP FAILING

#### What Are Ephemeral Ports?
When a client connects to a server:
1. **Client** uses a random port (1024-65535) as source port
2. **Server** uses well-known port (80, 443, 22, etc.) as destination port
3. **Return traffic reverses**: Server responds from port 80/443 to client's random port

#### Why This Matters for NACLs

**Scenario:** Web server in subnet, client on internet

**Inbound Request:**
- Source: Internet (random port 35000)
- Destination: Web server (port 443)
- **NACL Rule Needed:** Allow inbound 443

**Outbound Response (RETURN TRAFFIC):**
- Source: Web server (port 443)
- Destination: Internet (port 35000)
- **NACL Rule Needed:** Allow outbound 1024-65535 (ephemeral ports)

### Common NACL Troubleshooting Patterns - EXAM FAVORITES

| Symptom | Root Cause | Fix |
|---------|-----------|-----|
| **Connection timeout** | NACL blocking return traffic | Add outbound rule for ephemeral ports (1024-65535) |
| **Connection refused** | Application not running OR Security Group blocking | Check app first, then Security Group |
| **Intermittent connectivity** | NACL rule ordering issue | Check rule numbers, lower = higher priority |
| **Can't reach specific IP** | NACL has explicit DENY rule | DENY rules processed first, remove or reorder |

### Example Scenarios You Got Wrong

#### Question 2 (Your Quiz):
**Scenario:** EC2 instances can't connect to internet despite:
- Internet Gateway attached
- Route table has 0.0.0.0/0 → IGW
- Security Group allows all outbound
- Instances have public IPs

**Your Thinking:** "Routing issue or Security Group"
**Actual Problem:** NACL missing outbound ephemeral ports (1024-65535)

**Why:** Instances initiate connection to internet (outbound on random port 40000), internet responds back (inbound to port 40000), but NACL blocks inbound ephemeral ports.

**Correct NACL Rules:**
```
Outbound Rules:
100 - Allow - 0.0.0.0/0 - 80 (HTTP)
110 - Allow - 0.0.0.0/0 - 443 (HTTPS)
120 - Allow - 0.0.0.0/0 - 1024-65535 (ephemeral for responses)

Inbound Rules:
100 - Allow - 0.0.0.0/0 - 1024-65535 (ephemeral for return traffic)
```

#### Question 18 (Your Quiz):
**Scenario:** Application Load Balancer can't reach backend instances
- ALB in public subnet
- Instances in private subnet
- Different NACLs for each subnet

**Your Answer:** Security Group issue
**Actual Problem:** Private subnet NACL not allowing ephemeral ports for ALB health checks

**Why:** ALB sends health check from random port (45000) to instance port 80, instance responds from port 80 to ALB's port 45000. Private subnet NACL must allow:
- Inbound from ALB subnet on port 80
- Outbound to ALB subnet on 1024-65535 (ephemeral)

### The NACL Decision Tree

```
Problem: Can't connect to resource in VPC
│
├─ Check Layer 1: Route Table
│  └─ Does route exist to destination? NO → Add route
│
├─ Check Layer 2: NACL (subnet level)
│  ├─ Inbound: Does rule allow destination port? NO → Add inbound rule
│  ├─ Outbound: Does rule allow ephemeral ports (1024-65535)? NO → Add outbound rule
│  └─ Rule ordering: Is DENY rule before ALLOW? YES → Reorder or remove DENY
│
└─ Check Layer 3: Security Group (instance level)
   ├─ Inbound: Does rule allow source + port? NO → Add inbound rule
   └─ Outbound: Auto-allowed for return traffic (stateful)
```

### NACL Rule Best Practices - Avoid These Mistakes

#### ❌ MISTAKE #1: Forgetting Ephemeral Ports
**Wrong NACL:**
```
Inbound: Allow 443
Outbound: Allow 443
```

**Right NACL:**
```
Inbound: Allow 443 (for incoming requests)
Inbound: Allow 1024-65535 (for return traffic from outbound connections)
Outbound: Allow 443 (for outbound requests)
Outbound: Allow 1024-65535 (for return traffic from inbound connections)
```

#### ❌ MISTAKE #2: Treating NACL Like Security Group
**Wrong:** "Security Group allows all outbound, so NACL doesn't matter"
**Right:** NACLs are evaluated BEFORE Security Groups and are stateless

#### ❌ MISTAKE #3: Explicit DENY Without Understanding Order
**Rule 100:** DENY 10.0.1.0/24 on port 22
**Rule 200:** ALLOW 0.0.0.0/0 on port 22

Result: 10.0.1.0/24 blocked (rule 100 processed first)

---

## Critical Weak Area #3: Encryption & Key Management (3 Questions Missed)

### Your Current Problem
You pick SSE-KMS when the question explicitly says "AWS should NOT have access to encryption keys."

### Encryption Options Decision Tree

```
Question: Where should the data be encrypted?
│
├─ AWS should NOT have access to encryption keys
│  ├─ Need FIPS 140-2 Level 3 compliance?
│  │  └─ YES → CloudHSM (dedicated hardware, you control keys)
│  │
│  └─ NO FIPS requirement?
│     ├─ Encrypt before upload? → Client-Side Encryption (you manage keys)
│     └─ Encrypt during upload? → SSE-C (you provide key per request)
│
└─ AWS can manage encryption (most common)
   ├─ Need separate keys per object/region?
   │  └─ YES → SSE-KMS with Customer Managed Keys (CMK)
   │
   ├─ Need automatic key rotation?
   │  └─ YES → SSE-KMS (auto-rotates annually)
   │
   ├─ Need audit trail of key usage?
   │  └─ YES → SSE-KMS (integrates CloudTrail)
   │
   └─ Just need encryption, simplest option?
      └─ SSE-S3 (AWS manages everything)
```

### Encryption Options Comparison - MEMORIZE THIS

| Encryption Type | Who Manages Keys? | AWS Access to Keys? | Use Case | FIPS 140-2 Level |
|----------------|-------------------|---------------------|----------|------------------|
| **SSE-S3** | AWS | YES | Default encryption, least overhead | Level 2 |
| **SSE-KMS** | AWS (you control via IAM) | YES | Audit trail, key rotation, granular control | Level 2 |
| **SSE-C** | You (provide per request) | NO | You control keys, AWS encrypts/decrypts | Level 2 |
| **Client-Side** | You (encrypt before upload) | NO | Complete control, encrypt before AWS | Depends on your library |
| **CloudHSM** | You (dedicated hardware) | NO | FIPS 140-2 Level 3, regulatory compliance | **Level 3** |

### Exam Keywords - Pattern Recognition

| Question Says... | Correct Answer | Why |
|-----------------|----------------|-----|
| "AWS should NOT have access to keys" | CloudHSM or Client-Side | SSE-KMS/SSE-S3 give AWS access |
| "FIPS 140-2 **Level 3** compliance" | CloudHSM | Only CloudHSM provides Level 3 |
| "audit trail of who used encryption keys" | SSE-KMS + CloudTrail | KMS integrates with CloudTrail |
| "automatic key rotation" | SSE-KMS | Auto-rotates CMKs annually |
| "separate encryption keys per region" | SSE-KMS with CMKs | Can create CMK per region |
| "least operational overhead" | SSE-S3 | AWS manages everything |
| "need to control when keys are deleted" | SSE-KMS with CMK | You set deletion window (7-30 days) |

### The Traps You Fell Into

#### Question 4: "AWS must NOT have access to encryption keys"
**Your Answer:** SSE-KMS
**Why Wrong:** KMS keys are managed by AWS, AWS performs encryption operations
**Correct Answer:** CloudHSM (dedicated hardware, AWS has no access to keys)

#### Question 10: "FIPS 140-2 Level 3 compliance"
**Your Answer:** SSE-KMS
**Why Wrong:** KMS is only FIPS 140-2 Level 2
**Correct Answer:** CloudHSM (only service providing Level 3)

#### Question 20: "Pre-signed URLs with SSE-KMS encrypted objects"
**Your Answer:** Only s3:GetObject permission
**Why Wrong:** Encrypted objects require decryption
**Correct Answer:** s3:GetObject AND kms:Decrypt (need both to retrieve and decrypt)

### Pre-Signed URL Permissions - Critical Concept

Pre-signed URLs delegate the **URL generator's permissions** to the URL user.

**Scenario:** Generate pre-signed URL for SSE-KMS encrypted object

**Required Permissions (URL generator needs):**
1. `s3:GetObject` - to retrieve the object from S3
2. `kms:Decrypt` - to decrypt the object using KMS CMK

**When URL is used:**
- S3 retrieves encrypted object (checks s3:GetObject)
- S3 decrypts using KMS (checks kms:Decrypt)
- S3 returns decrypted object to user

**If URL generator lacks kms:Decrypt → Pre-signed URL fails with Access Denied**

### CloudHSM vs KMS - When to Use What

| Requirement | Use KMS | Use CloudHSM |
|------------|---------|--------------|
| Automatic key rotation | ✅ YES | ❌ Manual rotation |
| Shared multi-tenant service | ✅ YES (cost-effective) | ❌ Dedicated hardware |
| FIPS 140-2 Level 2 | ✅ YES | ✅ YES (and Level 3) |
| FIPS 140-2 Level 3 | ❌ NO | ✅ YES |
| AWS manages infrastructure | ✅ YES | ❌ You manage HSM cluster |
| Integrated with AWS services | ✅ YES | ⚠️ Limited integration |
| Price | $ | $$$ |
| AWS has key access | ✅ YES | ❌ NO |

---

## Critical Weak Area #4: Auto Scaling Policy Combinations (2 Questions Missed)

### Your Current Problem
You default to single policies (usually Target Tracking) instead of recognizing when you need to COMBINE policies.

### Auto Scaling Policy Types

| Policy Type | When to Use | Example | Operational Overhead |
|------------|-------------|---------|---------------------|
| **Target Tracking** | Unpredictable loads that follow a metric | Keep CPU at 70% | Low (set & forget) |
| **Step Scaling** | Need precise control at different thresholds | +2 at 60% CPU, +5 at 80% CPU | Medium (define steps) |
| **Scheduled Scaling** | Predictable load patterns | +10 instances at 8 AM weekdays | Low (set schedule) |
| **Simple Scaling** | Basic threshold-based scaling | +1 instance when CPU > 80% | Medium (one action per alarm) |

### The Pattern You Keep Missing: COMBINING POLICIES

#### Scenario Pattern:
"Application has **predictable** morning traffic spike AND **unpredictable** afternoon traffic based on external events"

**Your Answer:** Target Tracking (single policy)
**Correct Answer:** Scheduled Scaling (morning) + Target Tracking (afternoon)

### When to Combine Policies - Decision Tree

```
Analyze the workload pattern:
│
├─ Is there a PREDICTABLE component?
│  └─ YES → Use Scheduled Scaling for predictable part
│     │
│     ├─ Is there ALSO an unpredictable component?
│     │  └─ YES → COMBINE Scheduled + Target Tracking
│     │
│     └─ Entirely predictable?
│        └─ Scheduled Scaling only
│
└─ Entirely unpredictable?
   └─ Target Tracking only
```

### Exam Keywords for Policy Selection

| Question Says... | Policy to Use |
|-----------------|---------------|
| "**predictable** daily pattern" | Scheduled Scaling |
| "**unknown** load patterns" | Target Tracking |
| "traffic spikes **every morning at 9 AM**" | Scheduled Scaling |
| "load **varies based on user behavior**" | Target Tracking |
| "**both** regular business hours **and** unpredictable peaks" | **COMBINE** Scheduled + Target |
| "**least operational overhead**" | Target Tracking (single metric) |
| "need **precise control** at different thresholds" | Step Scaling |

### Examples from Your Quiz

#### Question 3/17 Pattern:
**Scenario:** "Predictable traffic spike every weekday 8 AM - 5 PM, PLUS unpredictable spikes based on marketing campaigns"

**Your Answer:** Target Tracking only
**Why Wrong:** Ignores predictable component
**Correct Answer:**
1. Scheduled Scaling → +20 instances at 7:55 AM weekdays
2. Target Tracking → Maintain 70% CPU during business hours
3. Scheduled Scaling → -20 instances at 5:05 PM weekdays

**Why Combine:**
- Scheduled handles predictable morning ramp-up (faster than waiting for CPU to climb)
- Target Tracking handles unpredictable campaign spikes
- Scheduled handles predictable evening scale-down

### Target Tracking vs Scheduled - When to Use What

#### Use Target Tracking When:
- ✅ Load is unpredictable
- ✅ Want to maintain specific metric (CPU, request count, custom metric)
- ✅ Need least operational overhead
- ✅ Pattern not time-based

#### Use Scheduled Scaling When:
- ✅ Load follows predictable time-based pattern
- ✅ Know exact times to scale up/down
- ✅ Want proactive scaling (before load hits)
- ✅ Business hours pattern

#### Use BOTH When:
- ✅ Predictable base load + unpredictable spikes
- ✅ Regular business hours + variable demand within those hours
- ✅ Need proactive scaling for known pattern + reactive for unknown

### Practice Scenarios

**Scenario 1:** E-commerce site with heavy traffic during business hours (9 AM - 9 PM), but unpredictable spikes during flash sales.
**Your Answer:** ________________
**Correct:** Scheduled (+instances at 8:55 AM, -instances at 9:05 PM) + Target Tracking (CPU 70%)

**Scenario 2:** Video processing service with completely random job submissions throughout the day.
**Your Answer:** ________________
**Correct:** Target Tracking on queue depth or CPU

**Scenario 3:** Batch processing job that runs every night at 2 AM, takes 3 hours, then idle rest of day.
**Your Answer:** ________________
**Correct:** Scheduled (+instances at 1:55 AM, -instances at 5:05 AM)

**Scenario 4:** Customer service portal with morning rush (8-10 AM), lunch rush (12-1 PM), and afternoon rush (3-5 PM).
**Your Answer:** ________________
**Correct:** Multiple Scheduled policies for each rush + Target Tracking for variations

---

## Critical Weak Area #5: EC2 Placement Groups & VPC Endpoints (2 Questions Missed)

### EC2 Placement Groups - Pick the Right Type

| Placement Group Type | Use Case | Exam Keywords |
|---------------------|----------|---------------|
| **Cluster** | HPC, lowest latency, single AZ | "**lowest latency**", "**high throughput**", "**tightly coupled**" |
| **Spread** | Critical instances isolated from each other | "**reduce correlated failures**", "**separate hardware**", "max **7 instances per AZ**" |
| **Partition** | Distributed systems (Hadoop, Kafka, Cassandra) | "**distributed workload**", "**large scale**", "**fault isolation**" |

### The Trap You Fell Into

#### Question 8: Kafka Cluster Deployment
**Scenario:** Deploy Kafka cluster across multiple nodes

**Your Answer:** Cluster placement group
**Why Wrong:** Kafka is a DISTRIBUTED system that needs fault isolation, not lowest latency
**Correct Answer:** Partition placement group

**The Pattern:**
- **Cluster** = "Make these as close as possible" (HPC, databases with tight coupling)
- **Partition** = "Separate these into groups on different hardware" (Kafka, Hadoop, Cassandra)
- **Spread** = "Keep these completely separate" (critical single instances)

### Placement Group Decision Tree

```
What type of application?
│
├─ High-Performance Computing (HPC), ML training, tightly-coupled nodes
│  └─ Cluster Placement Group (single AZ, same rack, lowest latency)
│
├─ Distributed Big Data (Hadoop, Kafka, Cassandra)
│  └─ Partition Placement Group (multiple partitions, fault isolation)
│
└─ Critical instances that must be isolated (DNS, AD servers)
   └─ Spread Placement Group (max 7 per AZ, separate hardware)
```

### VPC Endpoints - Gateway vs Interface

| Feature | Gateway Endpoint | Interface Endpoint |
|---------|-----------------|-------------------|
| **Services** | S3, DynamoDB ONLY | All other services |
| **Cost** | **FREE** | $$$ (per hour + data) |
| **How it works** | Route table entry | ENI in subnet |
| **DNS** | No | Yes (private DNS) |
| **Exam keyword** | "**most cost-effective**" | "**PrivateLink**" |

### The Trap You Fell Into

#### Question 14: VPC Endpoint Cost
**Question:** "VPC Gateway Endpoints for S3 and DynamoDB cost..."

**Your Answer:** "$0.01 per GB processed"
**Why Wrong:** Gateway Endpoints are FREE
**Correct Answer:** No charge (free)

**The Memory Hook:**
- **Gateway** = **FREE** (think: Gateway to savings)
- **Interface** = **$$** (think: ENI costs money)

### When to Use Which Endpoint

| Scenario | Use Gateway Endpoint | Use Interface Endpoint |
|----------|---------------------|----------------------|
| S3 or DynamoDB | ✅ YES (free) | ❌ Don't pay for Interface |
| Other AWS services (EC2, SNS, SQS, etc.) | ❌ Not supported | ✅ YES (only option) |
| Need private DNS | ❌ No | ✅ YES |
| Most cost-effective for S3 | ✅ YES | ❌ NO |
| On-premises access via Direct Connect | ❌ NO | ✅ YES |

---

## Study Strategy for Tomorrow's Review

### Morning Session (2 hours):

**Hour 1: S3 + NACLs**
1. Read this document's S3 section (30 min)
2. Draw the S3 decision tree from memory (15 min)
3. Read this document's NACL section (30 min)
4. Write out NACL rules for common scenario from memory (15 min)

**Hour 2: Encryption + Auto Scaling**
1. Read encryption section (30 min)
2. Create flashcards: "FIPS Level 3" → CloudHSM, "AWS no access" → CloudHSM/Client-side, etc. (15 min)
3. Read Auto Scaling section (30 min)
4. Practice identifying when to combine policies (15 min)

### Afternoon Session (1 hour):
**Retake Day-7-Weakness-Destroyer-Quiz**
- Target: 16+/20 (80%)
- Write your answer BEFORE looking at explanations
- Note which concepts you still struggle with

### Evening (30 min):
**Review mistakes from retake**
- If same mistakes → Re-read that section in this document
- If new mistakes → Those are your next weak areas

---

## Key Takeaways - MEMORIZE THESE

### S3 Storage Classes
- "Immediately" = Standard or Standard-IA (NOT Glacier Expedited)
- "Unknown patterns" = Intelligent-Tiering
- "Rarely" = Check retrieval time first, then choose
- Minimum durations: IA = 30 days, Glacier = 90 days, Deep Archive = 180 days

### VPC NACLs
- Stateless = Return traffic needs explicit rules
- Always allow ephemeral ports (1024-65535) for return traffic
- "Connection timeout" = NACL blocking return traffic
- DENY rules processed before ALLOW (lowest rule number wins)

### Encryption
- "AWS should NOT have access" = CloudHSM or Client-Side (NOT KMS)
- "FIPS 140-2 Level 3" = CloudHSM ONLY
- SSE-KMS objects need s3:GetObject AND kms:Decrypt permissions
- Gateway Endpoints (S3/DynamoDB) = FREE

### Auto Scaling
- Predictable pattern = Scheduled Scaling
- Unpredictable pattern = Target Tracking
- Both patterns = COMBINE Scheduled + Target Tracking
- Least overhead = Target Tracking (single metric)

### Placement Groups
- Cluster = HPC, lowest latency
- Partition = Distributed systems (Kafka, Hadoop)
- Spread = Isolated critical instances

---

## Success Criteria for Retake

You should score **16+/20 (80%)** on the retake quiz if you:
1. Can draw the S3 decision tree from memory
2. Can explain why NACLs need ephemeral ports
3. Know when CloudHSM is required (FIPS Level 3, AWS no access)
4. Recognize patterns requiring combined Auto Scaling policies
5. Can differentiate Cluster vs Partition placement groups

**If you score 16+**: You're ready for Week 2
**If you score 14-15**: Review weak areas, retake Sunday
**If you score <14**: Full Week 1 review needed - don't start Week 2

---

## Final Thoughts

You scored 45% because you were **pattern matching incorrectly**:
- Seeing "rarely" → defaulting to Glacier
- Seeing "connectivity issue" → defaulting to Security Groups
- Seeing "encryption" → defaulting to KMS
- Seeing "scaling" → defaulting to single Target Tracking policy

The exam tests whether you can **read carefully** and **apply the right pattern**.

Tomorrow, focus on understanding **WHY** each answer is correct, not just memorizing answers.

**You got this. Now go fix it.**
