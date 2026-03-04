# Day 45 -- Exam Eve Review
## March 1, 2026 | Exam Tomorrow at 9:00 AM EST

**You have passed every recent practice exam. This is a readiness scan, not a study session. Read it once, close the laptop, get some sleep.**

---

## 🚨 #1 WATCH PATTERN: SQS FIFO vs Kinesis Ordering

This is the only pattern that still bit you in the targeted drill. Know it cold.

### The Two-Question Framework

**Q: What is the SCOPE of the ordering requirement?**

| Ordering Requirement | Answer | Why |
|---|---|---|
| "Orders from the same customer must be in order" | **SQS FIFO** with customer ID as message group ID | Per-group ordering = FIFO's core strength |
| "All records must be globally ordered across ALL customers" | **Kinesis single shard** | Only single shard guarantees true global sequence |
| "Per-customer ordering AND >3,000 TPS" | **Kinesis multi-shard + consistent partition key** | Each customer routes to exactly one shard via partition key |

**Q: Does throughput exceed FIFO limits?**

| Condition | FIFO Status |
|---|---|
| Without batching | Hard cap: **300 TPS** — FIFO eliminated above this |
| With batching (up to 10 msgs/batch) | Hard cap: **3,000 TPS** — FIFO eliminated above this |
| Adding more consumers | Irrelevant — cap is on the **WRITE SIDE** of the queue, not the consumer side |

### The Trap the Exam Uses

The exam will present a question with TWO requirements and ask which solution handles BOTH. SQS FIFO will satisfy one (per-group ordering) but fail the other (global ordering OR throughput). **The distractor attacks FIFO's strength to bait you into selecting the wrong failure.**

**Burn this in:**
- Per-entity / per-customer / within a group = SQS FIFO ✅
- All records / globally / across all groups = Kinesis single shard ✅
- Per-entity ordering + high throughput = Kinesis multi-shard + consistent partition key ✅

---

### SQS FIFO Deduplication

**MessageDeduplicationId** — if a duplicate message is sent within the **5-minute deduplication window**, it is **silently discarded before it ever enters the queue.** No error. No DLQ. It never gets delivered.

| Mechanism | What Happens | DLQ Involved? |
|---|---|---|
| Duplicate within 5-min window | Silent discard — never enters queue | ❌ Never |
| Message fails processing (exceeds max receive count) | Sent to DLQ | ✅ Yes |

**Burn this in:** DLQ and deduplication are completely separate mechanisms that never interact. DLQ = processing failures only. Deduplication = silent discard before queue entry.

---

### SQS FIFO Head-of-Line Blocking

A failed message **blocks ALL subsequent messages in the same message group** — but **other groups are completely unaffected and continue processing normally.**

| Scenario | Effect |
|---|---|
| Message fails in Group A | Group A frozen until message is resolved or DLQ'd |
| Groups B, C, D | Unaffected — processing continues normally |
| No DLQ / no visibility timeout strategy | Group A lane frozen **indefinitely** — permanent business risk |

**Exam trap:** The question describes a business requirement that orders from the same customer must be processed in sequence. A failed order with no DLQ escape strategy will freeze that customer's lane permanently. The concern is **not** about UUID deduplication behavior — it's about the **operational consequence of the ordering requirement itself.**

**Burn this in:** Head-of-line blocking = same group only. DLQ is the escape hatch. Without it, one poisoned message owns the lane forever.

---

## DR Strategy -- Trigger Phrases

| Trigger Phrase | Answer | Trap to Avoid |
|---|---|---|
| "processes live production traffic continuously" | **Multi-Site Active/Active** | Aurora Global DB = still standby until promoted |
| "no idle standby capacity" | **Multi-Site Active/Active** | Warm Standby = scaled-down idle capacity |
| RTO of hours / RPO of hours | **Backup & Restore** | Cheapest, slowest |
| RTO of tens of minutes | **Pilot Light** | Core components always on, scale up on failover |
| RTO of minutes | **Warm Standby** | Scaled-down copy always running |
| RTO of seconds / near-zero | **Multi-Site Active/Active** | Most expensive |
| RPO of zero / zero data loss | **Synchronous replication** = Active/Active or Multi-AZ | Async replication has RPO > 0 |

**Aurora Global Database:** RPO < 1 second, RTO < 1 minute — but the secondary is **read-only standby**. It does NOT process live traffic. If the question says "live traffic in both regions" — Aurora Global is wrong.

---

## Lambda Concurrency -- The Cold Start Trap

| Type | What It Does | Cold Starts? | Hard Cap? |
|---|---|---|---|
| **Reserved Concurrency** | Allocates portion of account pool for this function | ❌ Does NOT eliminate | ✅ Yes — throttles above limit |
| **Provisioned Concurrency** | Pre-warms execution environments | ✅ **ELIMINATES cold starts** | ❌ Not a cap |

**Trigger:** "eliminate cold starts" / "consistent low latency" = **Provisioned Concurrency**
**Trigger:** "prevent function from consuming too many executions" / "protect downstream" = **Reserved Concurrency**

---

## Kinesis Shard Math

**1 shard =**
- **Write:** 1,000 records/sec OR 1 MB/sec (whichever is hit first = bottleneck)
- **Read:** 2 MB/sec

**Two-check method:** Check BOTH constraints. Take the higher shard count.
- Example: 5,000 records/sec at 500 bytes each = 2.5 MB/sec
  - Records constraint: 5,000 / 1,000 = **5 shards**
  - Throughput constraint: 2.5 MB / 1 = **3 shards** (rounds up)
  - Answer: **5 shards** (records constraint wins)

---

## Aurora Endpoints

| Endpoint | Routes To | Use For |
|---|---|---|
| **Cluster endpoint** | Writer instance ONLY | Writes |
| **Reader endpoint** | Load balances ALL readers | Reads (use this, not cluster) |
| **Instance endpoint** | Specific instance | Debugging, maintenance |

**Classic trap:** Reporting service connected to cluster endpoint. Writer CPU = 85%. Added readers = 0% CPU. Fix = **switch to reader endpoint**.

RDS Proxy + Auto Scaling delay: New readers take time to enter reader endpoint rotation + RDS Proxy holds existing connections to old readers until recycled.

---

## Fargate Hard Limits (GPU / OS Access)

**Fargate = zero OS access. Any of these = ECS with EC2 launch type:**
- GPU workloads (NVIDIA, ML inference, video transcoding with GPU)
- Custom Linux kernel modules
- Elevated Linux capabilities (CAP_NET_ADMIN, CAP_SYS_MODULE)
- Any workload requiring direct hardware access

**Lambda also cannot do GPU.** For GPU: ECS EC2 launch type with p3/g4dn instances, full stop.

---

## AWS Batch + Spot vs Lambda

| Factor | Lambda | AWS Batch + Spot |
|---|---|---|
| Max duration | **15 minutes** | Hours (no limit) |
| Max memory | **10 GB** | Instance-dependent (TB scale) |
| Pricing model | Per GB-second | Per EC2 Spot hour (~90% savings) |
| Best for | Event-driven, short, stateless | Batch, long-running, compute-heavy |

**Trigger:** Jobs > 15 min OR > 10 GB RAM OR cost-sensitive long-running = **AWS Batch + Spot**
**Step Functions chaining of Lambdas** does NOT fix jobs that cannot be checkpointed.

---

## DynamoDB Capacity Modes

| Situation | Mode | Why |
|---|---|---|
| New app, no traffic history, unpredictable spikes | **On-Demand** | No capacity planning needed |
| Stable traffic pattern (2+ weeks consistent) | **Provisioned + Auto Scaling** | ~6x cheaper per RCU/WCU |
| Known spike at known time (e.g., Black Friday midnight) | **Scheduled Scaling** | Pre-provision BEFORE spike — Auto Scaling ramps too slowly |

**Auto Scaling with high max ≠ instant scaling.** It scales incrementally in response to throttling. For a known spike start time, pre-provision 30 minutes early with Scheduled Scaling.

---

## Preventive vs Reactive Controls

| Control Type | Examples | When to Use |
|---|---|---|
| **Preventive** (block before it happens) | S3 Block Public Access, SCPs, bucket policy Deny, IAM permission boundaries | "Ensure buckets are NEVER public" |
| **Reactive** (detect after it happens) | AWS Config, GuardDuty, EventBridge, Security Hub | "Alert when buckets become public" |

**"Ensure X never happens"** = preventive = Block Public Access, SCP, Deny policy
**"Detect and remediate"** = reactive = Config rules + auto-remediation

---

## Key Numbers Table

| Service | Number | Context |
|---|---|---|
| Lambda | 15 min | Max execution timeout |
| Lambda | 10 GB | Max memory |
| Lambda | 1,000 | Default concurrent executions per region |
| SQS FIFO | 300 TPS | Without batching (hard write cap) |
| SQS FIFO | 3,000 TPS | With batching (hard write cap) |
| Kinesis | 1,000 rec/sec | Per shard write limit |
| Kinesis | 1 MB/sec | Per shard write throughput limit |
| Kinesis | 2 MB/sec | Per shard read throughput |
| RDS Multi-AZ | 60-120 sec | Automatic failover time |
| Aurora Global | <1 sec | RPO (replication lag) |
| Aurora Global | <1 min | RTO (failover time) |
| S3 Standard-IA | 30 days | Minimum storage duration |
| S3 One Zone-IA | 30 days | Minimum storage duration |
| S3 Glacier Flexible | 90 days | Minimum storage duration |
| S3 Glacier Deep Archive | 180 days | Minimum storage duration |
| Cross-AZ transfer | $0.01/GB | Each direction (private IP) |
| Same-AZ transfer | FREE | Private IP |
| Internet egress | $0.09/GB | First 10 TB/month |
| Spot savings | up to 90% | vs On-Demand |

---

## Exam Keyword Triggers

| Question Keyword | Likely Answer Direction |
|---|---|
| "MOST cost-effective" | Spot, Glacier, Reserved Instances, Auto Scaling, Gateway Endpoints (free) |
| "LEAST operational overhead" | Managed services: Lambda, Fargate, RDS, DynamoDB, Elastic Beanstalk |
| "eliminate cold starts" | Provisioned Concurrency (NOT Reserved) |
| "static IP" / "whitelist IP" | NLB (ALB has no static IP) |
| "millions of requests/sec" | NLB |
| "path-based routing" / "Lambda target" | ALB |
| "processes live traffic continuously" | Active/Active (multi-site) |
| "no data loss" / "RPO = zero" | Synchronous replication / Active/Active |
| "GPU workload" | ECS EC2 launch type (p3/g4dn) |
| "jobs > 15 minutes" | AWS Batch |
| "per-customer ordering" | SQS FIFO with group ID |
| "global ordering across all records" | Kinesis single shard |
| "throughput > 3,000 TPS" | Kinesis (FIFO is eliminated) |
| "known spike at known time" | Scheduled Scaling |
| "unpredictable spikes at launch" | DynamoDB On-Demand |
| "prevent public access" | S3 Block Public Access (preventive) |
| "detect public access" | AWS Config rule (reactive) |
| "new instance endpoint" after PITR | Yes — PITR always creates NEW instance |
| "CloudFront + ALB lockdown" | Prefix list on SG + custom header |
| "HSM / bring your own key" | KMS custom key store (NOT SSE-C) |

---

## Quick Sanity Check: Confirmed Strong Areas

**Read this list. If any one makes you uncertain, re-read that section of the cheat sheet.**

- [ ] VPC Gateway Endpoints (S3/DynamoDB = FREE, no NAT needed)
- [ ] NLB vs ALB (static IP, TCP/UDP, millions/sec = NLB)
- [ ] CloudTrail data events vs management events
- [ ] Config = compliance history; CloudTrail = API audit trail
- [ ] GuardDuty = threat detection (NOT compliance)
- [ ] WAF = Layer 7 protection (SQL injection, XSS)
- [ ] Shield Standard (free) vs Shield Advanced (DDoS with WAF + response team)
- [ ] KMS customer managed key vs AWS managed key vs customer key store
- [ ] Secrets Manager = auto-rotation; Parameter Store = no auto-rotation
- [ ] RDS PITR = creates NEW instance + new endpoint (update connection strings)
- [ ] Aurora Fast Clone = separate copy, changes don't affect original
- [ ] S3 OAC (new) vs OAI (legacy) for CloudFront origin
- [ ] Transit Gateway = hub-and-spoke, replaces VPC peering mesh
- [ ] Direct Connect + VPN as failover = no code changes (BGP routing handles it)
- [ ] EC2 Placement Groups: Cluster = HPC/low latency, Partition = Kafka/Hadoop, Spread = max 7/AZ
- [ ] Snowball = large data (>10 TB) + slow internet + tight deadline
- [ ] DMS + SCT = database migration (Schema Conversion Tool for heterogeneous)
- [ ] Aurora Serverless v2 = does NOT scale to zero (minimum ACU always running)
- [ ] Scheduled RIs = DISCONTINUED (always a trap answer)
- [ ] Cross-AZ load balancing: ALB = FREE, NLB/GWLB = costs money

---

## Exam Day Protocol

**March 2, 2026 -- 9:00 AM EST**

1. Arrive 30 minutes early
2. Read each question twice before looking at answers
3. For every question: identify the **decision trigger keyword** first
4. Eliminate obviously wrong answers before evaluating remaining two
5. "MOST cost-effective" = eliminate any answer with unnecessary services
6. "LEAST operational overhead" = managed service wins
7. If two answers look identical, find the **one differentiator** — that's the question
8. Trust your prep. You have been drilling these patterns for 45 days.

**You passed 76.9% on Day 40. You've been above 75% consistently. The exam is 72% to pass. You have buffer.**

Go get it.
