# Advanced Practice Scenarios - Hard Mode

**Difficulty: Real SAA-C03 Exam Level**
**Questions: 5 complex, multi-service scenarios**
**Time: 10 minutes (2 minutes per question)**
**Passing: 4/5 correct (80%)**

These questions are designed to simulate the actual exam difficulty. They require you to:
- Consider multiple AWS services in a single solution
- Balance trade-offs (cost vs performance vs complexity)
- Identify exam keyword patterns
- Eliminate wrong answers that "almost" work

---

## Scenario 1: Multi-Service High Availability Architecture

A financial services company is migrating a mission-critical application to AWS with the following requirements:

**Requirements:**
- Application processes real-time financial transactions (latency-sensitive)
- Must handle 5 million TCP connections per second
- Requires static IP addresses for partner firewall whitelisting
- Application runs on EC2 instances across multiple Availability Zones
- Must achieve 99.99% availability
- Session data must persist across instance failures
- Application health must be monitored (not just instance health)

**Which architecture meets ALL requirements with the LEAST operational overhead?**

**A)** Application Load Balancer with sticky sessions, EC2 instances in Auto Scaling group across 3 AZs using EC2 status checks, session data stored on EBS volumes

**B)** Network Load Balancer with cross-zone load balancing enabled, EC2 instances in Auto Scaling group across 3 AZs using ELB health checks, session data stored in ElastiCache Redis with Multi-AZ

**C)** Gateway Load Balancer with third-party load balancing appliances, EC2 instances in Auto Scaling group across 2 AZs, session data stored in DynamoDB

**D)** Network Load Balancer in single AZ with EC2 instances in Auto Scaling group using target tracking scaling, session data stored in RDS Multi-AZ

---

## Scenario 2: Cost-Optimized Storage with Compliance Requirements

A healthcare company needs to store patient medical records with the following requirements:

**Requirements:**
- Records must be retained for 10 years (compliance requirement)
- Approximately 2 PB of data
- Frequently accessed for first 30 days after creation (doctors reviewing recent visits)
- Occasionally accessed for 30-90 days (insurance claims processing)
- Rarely accessed after 90 days (audit requests 2-3 times per year)
- When accessed after 90 days, retrieval within 5 minutes is acceptable
- Must maintain 99.999999999% durability
- HIPAA compliance required (encryption at rest)
- Must minimize storage costs

**Which solution meets ALL requirements at the LOWEST cost?**

**A)** Store all data in S3 Standard with lifecycle policy: transition to S3 Standard-IA after 30 days, then S3 Glacier Deep Archive after 90 days. Enable S3 default encryption with SSE-S3.

**B)** Store all data in S3 Standard with lifecycle policy: transition to S3 Standard-IA after 30 days, then S3 Glacier Flexible Retrieval after 90 days. Enable S3 default encryption with SSE-KMS.

**C)** Store all data in S3 Standard with lifecycle policy: transition to S3 One Zone-IA after 30 days, then S3 Glacier Flexible Retrieval after 90 days. Enable S3 default encryption with SSE-S3.

**D)** Use S3 Intelligent-Tiering for automatic cost optimization with lifecycle policy to transition to S3 Glacier Deep Archive after 90 days. Enable S3 default encryption with SSE-KMS.

---

## Scenario 3: Hybrid Scaling Strategy with Predictable and Unpredictable Load

A SaaS company's application has the following traffic patterns:

**Traffic Patterns:**
- Baseline: 20 instances needed 24/7
- Business hours (Monday-Friday 8 AM - 6 PM): 50 instances needed
- End-of-month processing (last 3 days of every month): Unpredictable spikes up to 100 instances
- Application startup time: 3 minutes from launch to ready
- Average request processing time: 200ms
- Must maintain average CPU utilization around 60%

**Which Auto Scaling strategy provides the MOST cost-effective solution while meeting performance requirements?**

**A)** Reserved Instances for 50 instances + Target Tracking Scaling (CPU 60%) with min=50, max=100, cooldown=300 seconds

**B)** Reserved Instances for 20 instances + Scheduled Scaling (50 instances at 7:55 AM Mon-Fri, 20 instances at 6 PM Mon-Fri) + Target Tracking Scaling (CPU 60%) with min=20, max=100

**C)** Reserved Instances for 50 instances + Step Scaling with multiple CloudWatch alarms (CPU > 70% add 10, CPU > 85% add 20, CPU < 40% remove 5)

**D)** Spot Instances for 100 instances + Target Tracking Scaling (CPU 60%) with Spot Fleet diversification across multiple instance types

---

## Scenario 4: Complex Load Balancer Routing with Security Requirements

A company is building a customer-facing web application with the following architecture requirements:

**Requirements:**
- Route `/api/public/*` requests to public API servers (no authentication required)
- Route `/api/private/*` requests to private API servers (requires user authentication via Google/Facebook)
- Route `/admin/*` requests to admin servers (only from corporate IP range 203.0.113.0/24)
- Route `/ws/*` requests to WebSocket servers for real-time chat
- Invoke Lambda function for `/api/image-resize/*` requests (serverless image processing)
- Support HTTPS with SSL/TLS termination
- Must log all requests for security audit
- Minimize infrastructure management overhead

**Which solution meets ALL requirements?**

**A)** Network Load Balancer with Lambda targets for `/api/image-resize/*`, EC2 instances for other routes, security groups for IP filtering, CloudWatch Logs for request logging

**B)** Application Load Balancer with path-based routing rules, Cognito for `/api/private/*` authentication, source IP condition for `/admin/*`, Lambda targets for `/api/image-resize/*`, ALB access logs to S3

**C)** Multiple Application Load Balancers (one for each route), API Gateway for Lambda integration, WAF for IP filtering, CloudTrail for request logging

**D)** API Gateway with custom Lambda authorizers for authentication, Lambda functions for all routes including WebSocket connections, CloudWatch Logs for request logging

---

## Scenario 5: Disaster Recovery with RTO and RPO Requirements

A company runs a critical e-commerce application with the following disaster recovery requirements:

**Application Details:**
- Primary region: us-east-1
- Database: MySQL RDS database (2 TB, high transaction volume)
- Storage: 50 TB of product images in S3
- Compute: 30 EC2 instances behind Application Load Balancer

**DR Requirements:**
- RPO (Recovery Point Objective): 5 minutes (maximum data loss acceptable)
- RTO (Recovery Time Objective): 30 minutes (maximum downtime acceptable)
- Budget-conscious but DR is critical
- Must be able to failover to us-west-2

**Which disaster recovery strategy meets the requirements at the LOWEST cost?**

**A)** Backup and Restore: Daily automated RDS snapshots copied to us-west-2, S3 Cross-Region Replication, AMIs backed up weekly. Restore from backups during disaster.

**B)** Pilot Light: RDS Read Replica in us-west-2 with automated promotion, S3 Cross-Region Replication, minimal EC2 instances in us-west-2 (stopped) with Auto Scaling configured.

**C)** Warm Standby: RDS Multi-AZ in us-east-1 with Read Replica in us-west-2, S3 Cross-Region Replication, 30% of EC2 capacity running in us-west-2 with Auto Scaling to scale up during failover.

**D)** Multi-Site Active-Active: RDS Aurora Global Database with read/write in both regions, S3 Cross-Region Replication with bi-directional sync, full EC2 capacity in both regions with Route 53 weighted routing.

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

# Answers & Detailed Explanations

---

## Scenario 1: Answer - B

**Correct Answer: B) Network Load Balancer with cross-zone load balancing enabled, EC2 instances in Auto Scaling group across 3 AZs using ELB health checks, session data stored in ElastiCache Redis with Multi-AZ**

### Why B is Correct:

Let's check each requirement:

1. ✅ **5 million TCP connections per second**: NLB handles millions of requests per second
2. ✅ **Static IP addresses**: NLB provides one static IP per AZ (can assign Elastic IPs)
3. ✅ **Multiple AZs**: EC2 in Auto Scaling group across 3 AZs = 99.99% availability
4. ✅ **Application health monitoring**: ELB health checks test application endpoint
5. ✅ **Session persistence across failures**: ElastiCache Redis Multi-AZ with automatic failover
6. ✅ **Cross-zone load balancing**: Ensures even distribution across all instances

### Why A is Wrong:
- ❌ **ALB doesn't support static IP** (dynamic DNS only) - fails requirement #2
- ❌ EC2 status checks don't monitor application health - fails requirement #6
- ❌ Session data on EBS volumes doesn't persist across instance failures (EBS is attached to single instance)

### Why C is Wrong:
- ❌ **Gateway Load Balancer is for third-party security appliances**, not application load balancing
- ❌ Doesn't provide the performance or static IP features needed

### Why D is Wrong:
- ❌ **Single AZ** = cannot achieve 99.99% availability (needs multi-AZ)
- ❌ RDS for session data is overkill and slower than ElastiCache

### Key Learning Points:
- **5 million connections + static IP** = NLB (not ALB)
- **Session persistence** = ElastiCache or DynamoDB (not EBS, not local storage)
- **Application health monitoring** = ELB health checks (not EC2 status checks)
- **High availability** = Multi-AZ deployment (3 AZs for 99.99%)

---

## Scenario 2: Answer - B

**Correct Answer: B) Store all data in S3 Standard with lifecycle policy: transition to S3 Standard-IA after 30 days, then S3 Glacier Flexible Retrieval after 90 days. Enable S3 default encryption with SSE-KMS.**

### Why B is Correct:

Let's check each requirement:

1. ✅ **10-year retention**: All options support this
2. ✅ **Frequent access 0-30 days**: S3 Standard (millisecond retrieval)
3. ✅ **Occasional access 30-90 days**: S3 Standard-IA (millisecond retrieval)
4. ✅ **Rare access after 90 days**: S3 Glacier Flexible Retrieval
5. ✅ **5-minute retrieval acceptable**: Glacier Flexible Expedited = 1-5 minutes
6. ✅ **99.999999999% durability**: Standard-IA and Glacier Flexible both provide this
7. ✅ **HIPAA compliance encryption**: SSE-KMS provides key management audit trail (better for compliance)
8. ✅ **Minimize costs**: Optimal transition path for access patterns

### Why A is Wrong:
- ❌ **Glacier Deep Archive retrieval time = 12-48 hours** (doesn't meet "5 minutes acceptable")
- ❌ SSE-S3 works but SSE-KMS is better for HIPAA (key audit trail via CloudTrail)

### Why C is Wrong:
- ❌ **One Zone-IA durability = 99.5%** (not 99.999999999%) - stores in single AZ
- ❌ **Risk of data loss** if that AZ fails (unacceptable for medical records)
- ❌ SSE-S3 less ideal for compliance (no key audit trail)

### Why D is Wrong:
- ❌ **Intelligent-Tiering has monitoring fees** ($0.0025 per 1000 objects)
- ❌ For 2 PB with known access patterns, Intelligent-Tiering adds unnecessary cost
- ❌ Can't lifecycle from Intelligent-Tiering to Glacier Deep Archive automatically
- ❌ More expensive than manual lifecycle policy with known patterns

### Storage Class Breakdown:
- **0-30 days**: S3 Standard (~$0.023/GB/month) - frequent access
- **30-90 days**: S3 Standard-IA (~$0.0125/GB/month) - occasional access, immediate retrieval
- **90+ days**: S3 Glacier Flexible (~$0.0036/GB/month) - rare access, 1-5 min retrieval (Expedited)

**Cost for 2 PB:**
- First 30 days: 2000 TB × $0.023 = $46,000/month
- Next 60 days: 2000 TB × $0.0125 = $25,000/month
- Remaining 9+ years: 2000 TB × $0.0036 = $7,200/month
- Massive savings after lifecycle transitions

### Key Learning Points:
- **"5-minute retrieval acceptable"** = Glacier Flexible (NOT Deep Archive)
- **HIPAA compliance** = prefer SSE-KMS (audit trail via CloudTrail)
- **One Zone-IA** = lower durability (risky for critical medical data)
- **Intelligent-Tiering** = only worth it if access patterns are unpredictable

---

## Scenario 3: Answer - B

**Correct Answer: B) Reserved Instances for 20 instances + Scheduled Scaling (50 instances at 7:55 AM Mon-Fri, 20 instances at 6 PM Mon-Fri) + Target Tracking Scaling (CPU 60%) with min=20, max=100**

### Why B is Correct:

This is a **hybrid scaling strategy** that optimizes cost:

1. ✅ **Reserved Instances for 20**: Covers baseline 24/7 need at ~72% discount
2. ✅ **Scheduled Scaling**: Proactively scales to 50 instances before business hours (7:55 AM = 5 minutes before 8 AM spike)
3. ✅ **Target Tracking**: Handles unpredictable end-of-month spikes (scales from 50 to 100 as needed)
4. ✅ **Cost-effective**: Only pays for On-Demand instances during business hours and spikes

**Cost Breakdown:**
- **20 Reserved Instances**: ~$0.35/hr × 20 × 24 × 30 = $5,040/month (always running)
- **30 On-Demand instances**: ~$1.00/hr × 30 × 10 hrs × 22 days = $6,600/month (business hours)
- **Spike instances (0-50)**: Only used 3 days/month during end-of-month processing
- **Total estimated**: ~$12,000-15,000/month depending on spikes

### Why A is Wrong:
- ❌ **Reserved Instances for 50** = paying for 50 instances 24/7 even though baseline is only 20
- ❌ **Wastes money** during off-hours (50 instances running when only 20 needed)
- ❌ **Higher cost**: 50 RIs × $0.35/hr × 24 × 30 = $12,600/month just for RIs (before scaling)

### Why C is Wrong:
- ❌ **Reserved Instances for 50** = same problem as A (over-provisioned)
- ❌ **Step Scaling requires manual CloudWatch alarm configuration** (more operational overhead than Target Tracking)
- ❌ More expensive and more complex

### Why D is Wrong:
- ❌ **Spot Instances for mission-critical application** = risky (can be interrupted)
- ❌ No baseline capacity guarantee (Spot can be reclaimed)
- ❌ Doesn't optimize for predictable business hours pattern

### Scaling Strategy Comparison:

| Time Period | Capacity Needed | How B Handles It | Cost Type |
|------------|-----------------|------------------|-----------|
| **Off-hours** (6 PM - 8 AM, weekends) | 20 instances | 20 Reserved Instances | Reserved (cheap) |
| **Business hours** (8 AM - 6 PM Mon-Fri) | 50 instances | 20 RIs + 30 On-Demand via Scheduled | Reserved + On-Demand |
| **End-of-month spikes** | 50-100 instances | Target Tracking scales from 50 to 100 | On-Demand (only when needed) |

### Key Learning Points:
- **Predictable baseline + predictable spikes + unpredictable spikes** = RI (baseline) + Scheduled (predictable) + Target Tracking (unpredictable)
- **Don't over-provision Reserved Instances** (only buy for guaranteed minimum usage)
- **Scheduled Scaling starts 5 minutes early** to ensure capacity is ready before spike
- **Target Tracking handles unexpected spikes** (least operational overhead)

---

## Scenario 4: Answer - B

**Correct Answer: B) Application Load Balancer with path-based routing rules, Cognito for `/api/private/*` authentication, source IP condition for `/admin/*`, Lambda targets for `/api/image-resize/*`, ALB access logs to S3**

### Why B is Correct:

**ALB provides ALL features in a single load balancer:**

1. ✅ **Path-based routing**:
   - `/api/public/*` → Public API Target Group
   - `/api/private/*` → Private API Target Group (with Cognito auth)
   - `/admin/*` → Admin Target Group (with source IP condition)
   - `/ws/*` → WebSocket Target Group
   - `/api/image-resize/*` → Lambda function

2. ✅ **Authentication**: ALB native Cognito integration for social login (Google/Facebook)

3. ✅ **IP filtering**: ALB listener rules support source IP conditions (restrict `/admin/*` to 203.0.113.0/24)

4. ✅ **WebSocket support**: ALB has native WebSocket support

5. ✅ **Lambda targets**: ALB is the ONLY load balancer that can directly invoke Lambda functions

6. ✅ **HTTPS/TLS termination**: ALB supports SSL/TLS certificates

7. ✅ **Request logging**: ALB access logs to S3 for security audit

8. ✅ **Least operational overhead**: Single ALB handles everything

**ALB Listener Rules (evaluated in priority order):**
```
Priority 1: Path = /admin/* AND Source IP = 203.0.113.0/24 → Admin Target Group
Priority 2: Path = /api/private/* → Private Target Group (Cognito auth)
Priority 3: Path = /api/image-resize/* → Lambda function
Priority 4: Path = /ws/* → WebSocket Target Group
Priority 5: Path = /api/public/* → Public API Target Group
Default: → Default Target Group (error page)
```

### Why A is Wrong:
- ❌ **NLB doesn't support path-based routing** (Layer 4, can't see URLs)
- ❌ **NLB doesn't support Lambda targets** (can only target EC2, IP, or ALB)
- ❌ **NLB doesn't support authentication** (no Cognito integration)
- ❌ Automatic elimination due to routing requirements

### Why C is Wrong:
- ❌ **Multiple ALBs** = more operational overhead (need to manage multiple load balancers)
- ❌ **API Gateway for Lambda** = unnecessary complexity when ALB can invoke Lambda directly
- ❌ **WAF for IP filtering** = overkill when ALB listener rules can do this natively
- ❌ **CloudTrail logs API calls**, not request logs (need ALB access logs for HTTP requests)
- ❌ More expensive and complex than single ALB

### Why D is Wrong:
- ❌ **API Gateway with Lambda** = works but higher cost for high-traffic workloads
- ❌ **Lambda for ALL routes** = expensive and unnecessary (EC2 instances more cost-effective for steady traffic)
- ❌ **Custom Lambda authorizers** = more operational overhead than ALB Cognito integration
- ❌ More complex architecture than needed

### Key Learning Points:
- **Multiple routing requirements + Lambda + authentication** = ALB (it does everything)
- **ALB is the ONLY load balancer that supports Lambda targets**
- **ALB native Cognito integration** = social login without custom code
- **ALB listener rules support source IP conditions** = no need for WAF for simple IP filtering
- **Single ALB handles complex routing** = least operational overhead

---

## Scenario 5: Answer - B

**Correct Answer: B) Pilot Light: RDS Read Replica in us-west-2 with automated promotion, S3 Cross-Region Replication, minimal EC2 instances in us-west-2 (stopped) with Auto Scaling configured.**

### Why B is Correct:

**Disaster Recovery Strategy Analysis:**

**Requirements:**
- **RPO: 5 minutes** (max 5 minutes of data loss acceptable)
- **RTO: 30 minutes** (must be operational within 30 minutes)
- **Budget-conscious**

**Pilot Light DR Strategy:**

1. ✅ **RPO: 5 minutes**:
   - RDS Read Replica in us-west-2 with **asynchronous replication** (lag typically seconds to minutes)
   - S3 Cross-Region Replication (near real-time)
   - Meets 5-minute RPO requirement

2. ✅ **RTO: 30 minutes**:
   - Read Replica can be **promoted to standalone DB in ~5 minutes**
   - EC2 instances (stopped) can be started and Auto Scaling launches additional capacity in ~5-10 minutes
   - DNS failover via Route 53 (TTL-dependent, ~5 minutes)
   - **Total: ~15-20 minutes** (meets 30-minute RTO)

3. ✅ **Budget-conscious**:
   - Read Replica cost: ~$X/month (running DB in DR region)
   - S3 CRR: Storage cost + replication cost
   - Stopped EC2 instances: Only pay for EBS storage (not compute)
   - **Total DR cost: ~20-30% of production cost**

### Why A is Wrong:
- ❌ **RPO: 24 hours** (daily snapshots = up to 24 hours of data loss)
- ❌ **Doesn't meet 5-minute RPO requirement**
- ❌ **RTO: Hours** (restoring 2 TB RDS from snapshot takes 1-2 hours)
- ❌ **Doesn't meet 30-minute RTO requirement**
- ❌ Cheapest option but doesn't meet requirements

### Why C is Wrong:
- ✅ **Meets RPO and RTO** (would work)
- ❌ **NOT budget-conscious**: Running 30% of EC2 capacity 24/7 in DR region is expensive
- ❌ **Warm Standby cost**: ~50-70% of production cost
- ❌ **Over-engineered** for the requirements (Pilot Light is cheaper and meets requirements)

### Why D is Wrong:
- ✅ **Exceeds RPO and RTO** (best DR, near-zero RPO/RTO)
- ❌ **VERY EXPENSIVE**: Running full capacity in both regions = ~200% cost
- ❌ **Not budget-conscious at all**
- ❌ **Massive over-engineering** for 5-minute RPO / 30-minute RTO requirements

### DR Strategy Comparison:

| Strategy | RPO | RTO | Cost (% of prod) | Meets Requirements? |
|----------|-----|-----|------------------|---------------------|
| **Backup & Restore** | Hours-Days | Hours | 10-20% | ❌ NO (RPO/RTO too slow) |
| **Pilot Light** | Minutes | 10-30 min | 20-40% | ✅ YES (meets all) |
| **Warm Standby** | Seconds-Minutes | Minutes | 50-70% | ✅ YES (but expensive) |
| **Multi-Site Active-Active** | Near-zero | Near-zero | 150-200% | ✅ YES (way overkill) |

### Pilot Light DR Components:

**Always Running in DR Region:**
- ✅ RDS Read Replica (asynchronous replication from primary)
- ✅ S3 bucket with Cross-Region Replication enabled
- ✅ AMIs and Launch Templates configured
- ✅ Auto Scaling groups configured (but scaled to 0 or minimal)

**Started During Disaster:**
- Start stopped EC2 instances
- Promote Read Replica to standalone RDS instance
- Auto Scaling launches additional EC2 capacity
- Update Route 53 DNS to point to us-west-2

**Timeline:**
- **T+0**: Disaster detected
- **T+2**: Initiate Read Replica promotion
- **T+5**: Start EC2 instances, trigger Auto Scaling
- **T+7**: Read Replica promoted (now standalone DB)
- **T+10**: EC2 instances running, application starting
- **T+15**: Update Route 53 DNS (users start routing to DR)
- **T+20-25**: Application fully operational in us-west-2

### Key Learning Points:
- **RPO 5 min** = need active replication (Read Replica, CRR)
- **RTO 30 min** = Pilot Light sufficient (Warm Standby overkill)
- **Budget-conscious** = choose cheapest solution that meets requirements
- **Pilot Light** = minimal resources running in DR (DB, storage), compute started on-demand
- **Don't over-engineer DR** = match strategy to RPO/RTO requirements

---

## Quiz Scoring

**Your Score: ___/5 (___%)**

**Grading Scale:**
- **5/5 (100%)**: Outstanding! You're thinking like an AWS Solutions Architect
- **4/5 (80%)**: Strong! You understand complex multi-service solutions
- **3/5 (60%)**: Needs work - review the explanations for questions you missed
- **Below 3 (< 60%)**: Review Days 1-3 materials before attempting this again

---

## Key Patterns from These Questions

### Pattern 1: Multi-Service Integration
- Real exam questions combine multiple services (EC2 + ELB + ASG + ElastiCache)
- Must consider ALL requirements, not just one feature

### Pattern 2: Trade-off Analysis
- Cost vs Performance vs Complexity
- "Budget-conscious" vs "MOST available" vs "LEAST overhead"
- Exam tests if you can choose appropriate solution (not always most expensive/fancy)

### Pattern 3: Requirement Matching
- **Each requirement eliminates options**
- "Static IP" → eliminates ALB
- "5-minute retrieval" → eliminates Glacier Deep Archive
- "Application health monitoring" → eliminates EC2 status checks

### Pattern 4: Cost Optimization
- Reserved Instances for baseline capacity
- On-Demand or Spot for variable capacity
- Right-sized DR strategy (don't over-engineer)

### Pattern 5: Exam Keywords
- "LEAST operational overhead" → managed services, simple solutions
- "LOWEST cost" → cheapest option that meets requirements
- "MOST secure" → encryption, least privilege, Multi-AZ
- "Budget-conscious" → cost-aware but still functional

---

**These are harder than the previous quiz. If you score 80%+, you're ready for Day 4.**

Good luck! 🚀
