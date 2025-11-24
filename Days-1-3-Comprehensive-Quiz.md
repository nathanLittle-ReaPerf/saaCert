# Days 1-3 Comprehensive Quiz: EC2, Auto Scaling/LB, S3

**Coverage:** EC2 Fundamentals, Auto Scaling, Load Balancing, S3 Storage Classes & Lifecycle
**Questions:** 15 exam-style scenarios
**Time:** 30 minutes (2 minutes per question)
**Passing Score:** 80% (12/15 correct)

---

## Question 1 - EC2 Instance Types
A company is migrating a high-performance computing (HPC) application that performs complex scientific simulations. The application requires the highest CPU performance and is CPU-bound. Which EC2 instance family should they choose?

**A)** T3 instances (burstable performance)
**B)** C6i instances (compute optimized)
**C)** R6i instances (memory optimized)
**D)** I4i instances (storage optimized)

---

## Question 2 - EC2 Placement Groups
A financial trading application requires the lowest possible network latency between EC2 instances to process high-frequency trading data. All instances need to communicate with each other with minimal latency. Which placement group strategy should be used?

**A)** Spread placement group
**B)** Partition placement group
**C)** Cluster placement group
**D)** No placement group (default)

---

## Question 3 - EC2 Pricing Models
A company runs a batch processing workload that can be interrupted and resumed without data loss. The workload is flexible on timing and can run during off-peak hours. The company wants to minimize costs. Which pricing model provides the MOST cost savings?

**A)** On-Demand Instances
**B)** Reserved Instances (1-year term)
**C)** Spot Instances
**D)** Savings Plans

---

## Question 4 - S3 Storage Classes
A company needs to store audit logs that must be retained for 7 years for compliance. The logs will rarely be accessed, but when needed, they can tolerate a retrieval time of 12 hours. Which S3 storage class provides the MOST cost-effective solution?

**A)** S3 Standard
**B)** S3 Standard-IA (Infrequent Access)
**C)** S3 Glacier Flexible Retrieval
**D)** S3 Glacier Deep Archive

---

## Question 5 - Auto Scaling Policy
A web application experiences traffic that doubles every Monday at 9 AM when the weekly sales report is published, then returns to normal by 10 AM. The company wants to ensure sufficient capacity is available before the spike occurs. Which scaling approach is MOST appropriate?

**A)** Target Tracking Scaling with CPU target of 50%
**B)** Step Scaling with multiple CloudWatch alarms
**C)** Scheduled Scaling to increase capacity at 8:55 AM Monday
**D)** Simple Scaling with cooldown period

---

## Question 6 - Load Balancer Selection
A company is building a real-time bidding platform that processes millions of TCP connections per second with ultra-low latency requirements (< 50ms). The platform also requires static IP addresses that clients can whitelist in their firewalls. Which load balancer should they use?

**A)** Application Load Balancer (ALB)
**B)** Network Load Balancer (NLB)
**C)** Gateway Load Balancer (GLB)
**D)** Classic Load Balancer (CLB)

---

## Question 7 - S3 Lifecycle Policies
A media company stores video files in S3. Videos are frequently accessed for 30 days after upload, occasionally accessed for the next 60 days, and rarely accessed after 90 days but must be retained for 1 year. Which lifecycle policy provides the MOST cost-effective solution?

**A)** Keep all objects in S3 Standard for 1 year
**B)** Transition to S3 Standard-IA after 30 days, then to S3 Glacier after 90 days
**C)** Transition to S3 One Zone-IA after 30 days, then to S3 Glacier Deep Archive after 90 days
**D)** Use S3 Intelligent-Tiering for automatic optimization

---

## Question 8 - EC2 Spot Instances
A company wants to use Spot Instances for their batch processing workload. They want to minimize interruptions while still benefiting from Spot pricing. Which strategy should they implement?

**A)** Use only the largest instance type to maximize processing power
**B)** Use a single Availability Zone to reduce network latency
**C)** Diversify across multiple instance types and Availability Zones
**D)** Only bid at the On-Demand price to guarantee availability

---

## Question 9 - Load Balancer Routing
A company has a microservices architecture with the following requirements:
- Route requests to `/api/users/*` to the User Service
- Route requests to `/api/products/*` to the Product Service
- Route requests to `/admin/*` to Admin Service (only from specific IP ranges)
- Support HTTPS with SSL/TLS termination

Which solution meets these requirements with the LEAST operational overhead?

**A)** Multiple Network Load Balancers with IP-based routing
**B)** Application Load Balancer with path-based routing and security group rules
**C)** Gateway Load Balancer with custom routing rules
**D)** EC2 instances with NGINX reverse proxy

---

## Question 10 - S3 Storage Class Transitions
Which of the following S3 lifecycle transitions is NOT allowed?

**A)** S3 Standard → S3 Standard-IA
**B)** S3 Standard-IA → S3 Glacier Flexible Retrieval
**C)** S3 Glacier Flexible Retrieval → S3 Standard
**D)** S3 One Zone-IA → S3 Glacier Deep Archive

---

## Question 11 - Auto Scaling Health Checks
An Auto Scaling group runs web servers behind an Application Load Balancer. The application occasionally crashes but the EC2 instances remain running. Users report intermittent 502 errors. What is the MOST likely cause?

**A)** The Auto Scaling group is using ELB health checks and terminating instances too quickly
**B)** The Auto Scaling group is using EC2 status checks which don't detect application failures
**C)** The health check grace period is too short
**D)** Cross-zone load balancing is disabled

---

## Question 12 - EC2 Instance Types - Memory
A company is migrating an in-memory database (Redis) that requires 256 GB of RAM with consistent high memory bandwidth. Which EC2 instance family is MOST appropriate?

**A)** C6i (Compute Optimized)
**B)** T3 (Burstable)
**C)** R6i (Memory Optimized)
**D)** M6i (General Purpose)

---

## Question 13 - S3 Cost Optimization
A company has 500 TB of data in S3 Standard that hasn't been accessed in over 90 days. They want to reduce storage costs but maintain the ability to retrieve data within minutes if needed. Which solution provides the BEST cost savings while meeting the retrieval requirement?

**A)** Transition to S3 Standard-IA (Infrequent Access)
**B)** Transition to S3 Glacier Flexible Retrieval
**C)** Transition to S3 Glacier Deep Archive
**D)** Enable S3 Intelligent-Tiering

---

## Question 14 - Load Balancer Cross-Zone
A Network Load Balancer distributes traffic across two Availability Zones. AZ-A has 4 instances and AZ-B has 4 instances. The architect wants to ensure even distribution of traffic across all 8 instances regardless of which AZ receives the traffic. What should they configure?

**A)** Enable sticky sessions
**B)** Enable cross-zone load balancing (additional charges apply)
**C)** Increase health check frequency
**D)** Add more instances to each AZ

---

## Question 15 - EC2 Reserved Instances
A company has a steady-state workload running 24/7 on m5.xlarge instances in us-east-1. They want to reduce costs with Reserved Instances. The application may need to change instance types in the future. Which Reserved Instance type provides the MOST flexibility?

**A)** Standard Reserved Instance (1-year, no upfront)
**B)** Standard Reserved Instance (3-year, all upfront)
**C)** Convertible Reserved Instance (1-year, partial upfront)
**D)** Scheduled Reserved Instances

---
---

# STOP HERE - Don't scroll down if you haven't answered yet!

---
---
---
---
---
---
---
---

# Answers & Explanations

---

## Question 1: Answer - B
**Correct Answer: B) C6i instances (compute optimized)**

**Explanation:**
- **Compute Optimized (C-family)** instances provide the highest CPU performance for compute-bound workloads
- **C6i** uses Intel Ice Lake processors optimized for high-performance computing (HPC), scientific modeling, batch processing
- HPC applications are CPU-intensive and require maximum compute performance
- **Why not A?** T3 burstable instances are for variable workloads, not sustained high CPU
- **Why not C?** R6i is for memory-intensive workloads (databases, in-memory caches)
- **Why not D?** I4i is for high IOPS storage workloads

**Exam Keywords:** "High-performance computing", "CPU-bound", "scientific simulations" → **Compute Optimized (C-family)**

---

## Question 2: Answer - C
**Correct Answer: C) Cluster placement group**

**Explanation:**
- **Cluster placement group** packs instances close together in a single AZ for lowest latency and highest network throughput
- Provides up to **10 Gbps bandwidth between instances**
- **Critical for HPC, financial trading, and applications requiring instance-to-instance communication**
- All instances in the same cluster placement group can communicate with each other
- **Why not A?** Spread placement group maximizes availability by separating instances (increases latency)
- **Why not B?** Partition placement group is for distributed workloads (Hadoop, Cassandra), not lowest latency
- **Why not D?** Default placement has higher latency

**Exam Keywords:** "Lowest latency", "high-frequency trading", "HPC", "instances communicate with each other" → **Cluster placement group**

**Trade-off:** Single AZ = lower availability, but lowest latency

---

## Question 3: Answer - C
**Correct Answer: C) Spot Instances**

**Explanation:**
- **Spot Instances** provide up to **90% discount** compared to On-Demand pricing
- Perfect for **fault-tolerant, flexible, interruptible workloads** (batch processing, data analysis, image processing)
- Can be interrupted by AWS with 2-minute warning when capacity is needed
- Since workload can be interrupted and resumed, Spot is ideal
- **Why not A?** On-Demand has no discount (most expensive)
- **Why not B?** Reserved Instances require commitment (1-3 years), good for steady workloads but less savings than Spot
- **Why not D?** Savings Plans offer discounts (up to 72%) but still not as cheap as Spot for interruptible workloads

**Exam Keywords:** "Can be interrupted", "flexible timing", "minimize costs", "batch processing" → **Spot Instances**

---

## Question 4: Answer - D
**Correct Answer: D) S3 Glacier Deep Archive**

**Explanation:**
- **S3 Glacier Deep Archive** is the **lowest-cost storage class** (cheapest in AWS)
- Designed for data accessed once or twice per year
- Retrieval time: **12-48 hours** (question says 12 hours is acceptable)
- Perfect for **long-term archival, compliance, regulatory requirements** (7-year retention)
- **Why not A?** S3 Standard is for frequently accessed data (most expensive)
- **Why not B?** S3 Standard-IA is for infrequent access but costs more than Glacier Deep Archive
- **Why not C?** S3 Glacier Flexible Retrieval is cheaper than IA but more expensive than Deep Archive (retrieval: minutes to hours)

**Cost Comparison (per GB/month):**
- S3 Standard: ~$0.023
- S3 Standard-IA: ~$0.0125
- S3 Glacier Flexible Retrieval: ~$0.0036
- **S3 Glacier Deep Archive: ~$0.00099** ✅ (cheapest)

**Exam Keywords:** "Rarely accessed", "7-year retention", "compliance", "12-hour retrieval acceptable" → **S3 Glacier Deep Archive**

---

## Question 5: Answer - C
**Correct Answer: C) Scheduled Scaling to increase capacity at 8:55 AM Monday**

**Explanation:**
- **Scheduled Scaling** is perfect for **predictable, time-based load patterns**
- Question states the spike happens "every Monday at 9 AM" (predictable!)
- Scaling at 8:55 AM ensures capacity is ready BEFORE the spike (proactive)
- Scales back down at 10 AM when traffic returns to normal
- **Why not A?** Target Tracking is reactive (waits for CPU to increase, then scales) - would lag behind the spike
- **Why not B?** Step Scaling is also reactive and requires manual alarm configuration
- **Why not D?** Simple Scaling is legacy with cooldown issues

**Exam Keywords:** "Predictable pattern", "every Monday", "ensure capacity before spike" → **Scheduled Scaling**

**Pro Tip:** Combine Scheduled (baseline) + Target Tracking (unexpected spikes) for best results

---

## Question 6: Answer - B
**Correct Answer: B) Network Load Balancer (NLB)**

**Explanation:**
- **NLB** is the ONLY load balancer that meets ALL requirements:
  - **Millions of TCP connections per second** (extreme performance)
  - **Ultra-low latency < 50ms** (NLB provides < 100ms, can achieve < 50ms)
  - **Static IP addresses** (one static IP per AZ, Elastic IP support)
  - **Layer 4** (TCP protocol)
- Perfect for real-time bidding, gaming, financial trading
- **Why not A?** ALB doesn't support static IPs (dynamic DNS only) and has higher latency
- **Why not C?** GLB is for third-party security appliances, not application load balancing
- **Why not D?** CLB is legacy (don't use for new deployments)

**Exam Keywords:** "Millions of connections", "ultra-low latency", "static IP", "TCP" → **Network Load Balancer**

---

## Question 7: Answer - B
**Correct Answer: B) Transition to S3 Standard-IA after 30 days, then to S3 Glacier after 90 days**

**Explanation:**
- **Lifecycle Policy Breakdown:**
  - **0-30 days**: S3 Standard (frequent access)
  - **30-90 days**: S3 Standard-IA (occasional access, retrieval in ms)
  - **90-365 days**: S3 Glacier Flexible Retrieval (rare access, retrieval in minutes-hours)
- Balances cost savings with access patterns
- **Why not A?** Keeping everything in Standard wastes money (most expensive)
- **Why not C?** One Zone-IA has lower durability (99.5% vs 99.999999999%), risky for media files. Deep Archive has 12-hour retrieval (too slow for "rarely accessed")
- **Why not D?** Intelligent-Tiering has monitoring fees and auto-transitions don't include Glacier (requires manual lifecycle policy)

**S3 Storage Class Minimum Storage Duration:**
- Standard: None
- Standard-IA: 30 days minimum
- Glacier Flexible: 90 days minimum
- Glacier Deep Archive: 180 days minimum

**Exam Tip:** Match storage class to access pattern and retrieval time requirements

---

## Question 8: Answer - C
**Correct Answer: C) Diversify across multiple instance types and Availability Zones**

**Explanation:**
- **Spot Instance Best Practice: Diversification reduces interruption risk**
- AWS reclaims Spot capacity when needed - spreading across multiple instance types and AZs means:
  - If one instance type is reclaimed in one AZ, others continue running
  - Increases overall availability and reduces interruption rate
- **Spot Fleet** or **Auto Scaling with mixed instance types** implement this strategy
- **Why not A?** Single large instance type = higher interruption risk (less capacity pools)
- **Why not B?** Single AZ = if that AZ has capacity reclaimed, entire workload interrupted
- **Why not D?** Bidding at On-Demand price defeats the purpose (no cost savings)

**Exam Keywords:** "Minimize interruptions", "Spot Instances" → **Diversify instance types and AZs**

**Spot Instance Interruption:** AWS gives 2-minute warning (EC2 instance interruption notice)

---

## Question 9: Answer - B
**Correct Answer: B) Application Load Balancer with path-based routing and security group rules**

**Explanation:**
- **ALB** provides ALL requirements in a single load balancer:
  - **Path-based routing**: `/api/users/*` → User Target Group, `/api/products/*` → Product Target Group, `/admin/*` → Admin Target Group
  - **IP-based rules**: ALB listener rules can route based on source IP (restrict `/admin/*` to specific IPs)
  - **HTTPS/TLS termination**: ALB natively supports SSL/TLS certificates
- **Least operational overhead**: Single ALB with routing rules (no custom infrastructure)
- **Why not A?** NLB doesn't support path-based routing (Layer 4, doesn't see URLs)
- **Why not C?** GLB is for third-party security appliances, not application routing
- **Why not D?** NGINX on EC2 requires managing instances, updates, scaling (more overhead)

**Exam Keywords:** "Path-based routing", "HTTPS", "least operational overhead" → **Application Load Balancer**

---

## Question 10: Answer - C
**Correct Answer: C) S3 Glacier Flexible Retrieval → S3 Standard**

**Explanation:**
- **S3 Lifecycle transitions are ONE-WAY** (you can't transition back to warmer tiers via lifecycle policy)
- **Allowed transitions** (waterfall model):
  - Standard → Standard-IA → Glacier Flexible → Glacier Deep Archive ✅
  - One Zone-IA → Glacier Flexible → Glacier Deep Archive ✅
- **NOT allowed in lifecycle policy:**
  - Glacier → Standard ❌
  - Glacier → Standard-IA ❌
  - Any "backwards" transition to warmer tiers ❌
- **To restore from Glacier to Standard:** Use S3 Restore API (manual operation, not lifecycle)
- **Why A, B, D are allowed?** All are forward transitions to colder storage

**Exam Trap:** You CAN manually restore objects from Glacier to Standard, but you CANNOT create a lifecycle rule to do it automatically

**Exam Keywords:** "Lifecycle transition NOT allowed" → **Glacier to Standard/IA**

---

## Question 11: Answer - B
**Correct Answer: B) The Auto Scaling group is using EC2 status checks which don't detect application failures**

**Explanation:**
- **Problem**: Application crashes but EC2 instance still runs
- **EC2 status checks** only verify:
  - Instance is running (VM is up)
  - Network is reachable
  - Hardware is OK
- **EC2 checks DON'T test the application** (can't detect crashed applications)
- **ELB health checks** test application endpoint (`/health`, `/ping`)
- **Result**: ASG keeps routing traffic to instances with crashed apps → 502 errors
- **Solution**: Change ASG health check type from EC2 to ELB
- **Why not A?** If using ELB checks correctly, terminating unhealthy instances is correct behavior
- **Why not C?** Grace period is for startup time, doesn't affect detection of app crashes
- **Why not D?** Cross-zone balancing affects traffic distribution, not health detection

**Exam Keywords:** "Application crashes but instance running", "intermittent errors" → **Use ELB health checks instead of EC2**

---

## Question 12: Answer - C
**Correct Answer: C) R6i (Memory Optimized)**

**Explanation:**
- **R-family (Memory Optimized)** instances are designed for memory-intensive workloads
- **R6i** provides:
  - High memory-to-CPU ratio
  - High memory bandwidth
  - Optimized for in-memory databases (Redis, Memcached), SAP HANA, real-time big data analytics
- **256 GB RAM requirement** needs memory-optimized instance
- **Why not A?** C6i is compute-optimized (high CPU, less memory)
- **Why not B?** T3 burstable has low memory capacity and variable performance
- **Why not D?** M6i general purpose has balanced CPU/memory but less memory than R6i

**Instance Family Cheat Sheet:**
- **C** = Compute (CPU-bound: HPC, scientific modeling)
- **R** = RAM/Memory (memory-bound: databases, caches)
- **I** = IOPS/Storage (storage-bound: NoSQL, data warehouses)
- **M** = Medium/General purpose (balanced workloads)
- **T** = Throttled/Burstable (variable workloads)

**Exam Keywords:** "In-memory database", "256 GB RAM", "high memory bandwidth" → **Memory Optimized (R-family)**

---

## Question 13: Answer - A
**Correct Answer: A) Transition to S3 Standard-IA (Infrequent Access)**

**Explanation:**
- **Requirement**: Reduce costs + retrieve within minutes if needed
- **S3 Standard-IA**:
  - **Cost**: ~$0.0125/GB/month (45% cheaper than Standard ~$0.023/GB/month)
  - **Retrieval time**: Milliseconds (same as Standard)
  - **Use case**: Infrequently accessed data with immediate access requirements
- **500 TB** = significant savings (~$5,500/month saved)
- **Why not B?** Glacier Flexible Retrieval time: minutes to hours (Expedited: 1-5 min, Standard: 3-5 hours)
- **Why not C?** Glacier Deep Archive retrieval: 12-48 hours (too slow)
- **Why not D?** Intelligent-Tiering costs extra for monitoring + doesn't save as much for data not accessed in 90 days

**Retrieval Time Comparison:**
- Standard/Standard-IA: **Milliseconds** ✅
- Glacier Instant Retrieval: **Milliseconds**
- Glacier Flexible (Expedited): **1-5 minutes**
- Glacier Flexible (Standard): **3-5 hours**
- Glacier Deep Archive: **12-48 hours**

**Exam Keywords:** "Reduce costs", "retrieve within minutes", "hasn't been accessed" → **S3 Standard-IA** (meets retrieval requirement)

---

## Question 14: Answer - B
**Correct Answer: B) Enable cross-zone load balancing (additional charges apply)**

**Explanation:**
- **Without Cross-Zone Load Balancing** (NLB default):
  - Traffic split evenly to AZs (50% AZ-A, 50% AZ-B)
  - Then split among instances in each AZ
  - Each instance gets same traffic IF instance counts are equal
- **With Cross-Zone Load Balancing**:
  - Traffic distributed evenly across ALL 8 instances regardless of AZ
  - Ensures perfect load distribution
- **NLB Cross-Zone**: Disabled by default, **charges apply** for data transfer between AZs
- **Why not A?** Sticky sessions route same client to same instance, doesn't affect even distribution
- **Why not C?** Health check frequency doesn't affect traffic distribution
- **Why not D?** Adding instances doesn't solve uneven distribution if instances are already equal

**Critical Fact:**
- **ALB**: Cross-zone enabled by default, **FREE** ✅
- **NLB**: Cross-zone disabled by default, **COSTS MONEY** 💰

**Exam Keywords:** "Even distribution across all instances", "NLB" → **Enable cross-zone load balancing (charges apply)**

---

## Question 15: Answer - C
**Correct Answer: C) Convertible Reserved Instance (1-year, partial upfront)**

**Explanation:**
- **Convertible Reserved Instances** allow you to:
  - **Change instance type** (m5.xlarge → m6i.xlarge → c6i.xlarge)
  - Change instance family (M5 → C6i → R6i)
  - Change OS, tenancy, and payment options
- **Standard Reserved Instances**: CANNOT change instance family (less flexible)
- **1-year term** provides flexibility to reassess after 1 year
- **Partial upfront** balances cost savings with flexibility
- **Why not A or B?** Standard RIs don't allow instance family changes
- **Why not D?** Scheduled RIs are for predictable time windows (deprecated, not recommended)

**Reserved Instance Comparison:**
| Type | Discount | Flexibility |
|------|----------|------------|
| **Standard RI** | Up to 72% | Can modify AZ, instance size within family |
| **Convertible RI** | Up to 66% | Can change instance family, type, OS, tenancy ✅ |

**Payment Options:**
- **All Upfront**: Maximum discount (pay everything upfront)
- **Partial Upfront**: Balanced (pay some upfront, rest monthly)
- **No Upfront**: Least discount (pay monthly)

**Exam Keywords:** "Steady-state workload", "may change instance types", "flexibility" → **Convertible Reserved Instance**

---

## Quiz Scoring

**Your Score: ___/15 (___%)**

**Grading Scale:**
- **13-15 correct (87-100%)**: Excellent! You're ready to move forward ✅
- **12 correct (80%)**: Passing, but review missed questions carefully
- **10-11 correct (67-73%)**: Needs work - review study materials for weak areas
- **Below 10 (< 67%)**: Not ready - re-study Days 1-3 before continuing

---

## Common Mistakes to Watch For:

**EC2:**
- Confusing instance families (C=Compute, R=Memory, I=Storage, M=General)
- Not knowing placement group types (Cluster=low latency, Spread=HA, Partition=distributed)
- Forgetting Spot discount (up to 90%) vs Reserved (up to 72%)

**Auto Scaling & Load Balancing:**
- EC2 health checks vs ELB health checks (big difference!)
- Scheduled scaling for predictable patterns
- NLB = static IP, ALB = path routing

**S3:**
- Storage class retrieval times (Standard/IA=ms, Glacier=min-hours, Deep Archive=12-48hrs)
- Lifecycle transitions are ONE-WAY (can't go back to warmer tiers)
- Glacier Deep Archive is CHEAPEST (~$0.001/GB/month)

---

**Next Steps:**
1. Review all questions you got wrong
2. Understand WHY each wrong answer is wrong
3. If you scored below 80%, revisit the study materials
4. Ready to move to Day 4 when you hit 80%+

Good luck! 🚀
