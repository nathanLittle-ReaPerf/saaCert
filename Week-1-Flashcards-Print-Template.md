# AWS SAA-C03 Week 1 Flashcards - Print Template

**Instructions:** Print this document and use it as a reference to hand-write flashcards. Each card has a FRONT (question/prompt) and BACK (answer/explanation).

---

## EC2 PLACEMENT GROUPS (Critical!)

### Card 1
**FRONT:** What are the 3 types of EC2 Placement Groups?

**BACK:**
- CLUSTER: Low latency, high throughput (HPC, ML training)
- PARTITION: Large distributed systems (Kafka, Hadoop, Cassandra, Spark, Elasticsearch)
- SPREAD: Small critical instances (max 7 per AZ)

---

### Card 2
**FRONT:** 50-node Kafka cluster across 3 AZs. Which placement group?

**BACK:** PARTITION
- Large distributed system (50 nodes)
- Spread won't work (7 per AZ limit)
- Provides rack-level isolation

---

### Card 3
**FRONT:** 4 mission-critical database servers need hardware isolation. Which placement group?

**BACK:** SPREAD
- Small number (under 7 limit)
- Each instance on separate hardware
- Minimizes correlated failures

---

### Card 4
**FRONT:** ML training cluster needs lowest latency. Which placement group?

**BACK:** CLUSTER
- Lowest latency (10-25 Gbps)
- Highest throughput
- Single AZ only
- For tightly-coupled workloads

---

### Card 5
**FRONT:** When do you use PARTITION placement group?

**BACK:** Large distributed/partitioned systems:
- Kafka, Hadoop, Cassandra, Spark, Elasticsearch
- 40+ nodes typically
- Multi-AZ support
- Rack-level fault isolation

---

## S3 STORAGE CLASSES

### Card 6
**FRONT:** What are the S3 storage class retrieval times?

**BACK:**
- Standard/Standard-IA: Milliseconds
- Glacier Instant: Milliseconds
- Glacier Flexible: 3-5 hours (Standard), 1-5 min (Expedited)
- Glacier Deep Archive: 12+ hours

---

### Card 7
**FRONT:** "Rarely accessed + must be available immediately" = Which S3 class?

**BACK:** S3 Standard-IA
- Infrequent access
- IMMEDIATE retrieval (milliseconds)
- Cheaper than Standard

---

### Card 8
**FRONT:** "Archival + 3-5 hour retrieval acceptable" = Which S3 class?

**BACK:** S3 Glacier Flexible Retrieval
- Cheapest for rarely accessed data (not Deep Archive)
- 3-5 hour retrieval with Standard retrieval
- Good for compliance archives

---

### Card 9
**FRONT:** When do you use S3 Lifecycle policies vs Intelligent-Tiering?

**BACK:**
- LIFECYCLE: Known access patterns (e.g., "frequent for 30 days then rarely")
- INTELLIGENT-TIERING: Unknown/changing patterns (but has monitoring cost)

---

## DYNAMODB

### Card 10
**FRONT:** New app with unpredictable traffic (100-10,000 requests). Which DynamoDB capacity mode?

**BACK:** ON-DEMAND
- No historical data
- Highly unpredictable (100x variation)
- Scales instantly
- Pay per request

---

### Card 11
**FRONT:** When do you use Provisioned vs On-Demand capacity in DynamoDB?

**BACK:**
- PROVISIONED: Predictable traffic, can forecast capacity
- ON-DEMAND: Unpredictable traffic, new apps, unknown patterns

---

## AUTO SCALING

### Card 12
**FRONT:** Batch job at 2 AM daily + unpredictable daytime spikes. Which Auto Scaling policies?

**BACK:** SCHEDULED + TARGET TRACKING
- Scheduled: Proactively add capacity before 2 AM (predictable)
- Target Tracking: React to daytime spikes (unpredictable)
- COMBINE both!

---

### Card 13
**FRONT:** When do you combine Scheduled + Target Tracking Auto Scaling?

**BACK:** When you have BOTH:
- Predictable patterns (batch jobs, business hours)
- Unpredictable patterns (random spikes)
Use Scheduled for known, Target Tracking for reactive

---

## VPC NETWORKING

### Card 14
**FRONT:** What's the difference between Security Groups and NACLs?

**BACK:**
- Security Groups: STATEFUL (return traffic automatic)
- NACLs: STATELESS (must explicitly allow return traffic)
- NACLs need ephemeral ports 1024-65535 for return traffic

---

### Card 15
**FRONT:** EC2 downloads from internet (HTTPS). What NACL rules needed?

**BACK:**
- OUTBOUND: Allow port 443 to 0.0.0.0/0
- INBOUND: Allow ephemeral ports 1024-65535 from 0.0.0.0/0 (return traffic!)

---

### Card 16
**FRONT:** EC2 needs private access to S3. What do you use?

**BACK:** VPC GATEWAY ENDPOINT
- Keeps traffic private (no internet)
- FREE (no data transfer charges)
- S3 and DynamoDB use Gateway Endpoints

---

### Card 17
**FRONT:** Which VPC Endpoints are FREE?

**BACK:**
- Gateway Endpoints: S3 and DynamoDB (FREE)
- Interface Endpoints: All other services (COSTS MONEY - $/hour + $/GB)

---

## LAMBDA

### Card 18
**FRONT:** What are the Lambda limits?

**BACK:**
- Max timeout: 15 MINUTES
- Max memory: 10 GB
- Default concurrent executions: 1,000

---

### Card 19
**FRONT:** Job takes 1-3 hours. Can Lambda handle it?

**BACK:** NO
- Lambda max timeout is 15 minutes
- Use AWS Batch, ECS, or Step Functions instead

---

## RDS & DATABASES

### Card 20
**FRONT:** What's the difference between RDS Multi-AZ and Read Replicas?

**BACK:**
- MULTI-AZ: High availability, automatic failover (60-120 sec), synchronous replication
- READ REPLICAS: Read scaling, async replication, manual promotion

---

### Card 21
**FRONT:** "Automatic failover" + "RDS" = What solution?

**BACK:** RDS MULTI-AZ
- Automatic failover (no manual intervention)
- 60-120 second failover time
- Synchronous replication to standby

---

### Card 22
**FRONT:** What's better for failover: RDS Multi-AZ or Aurora Multi-AZ?

**BACK:** AURORA
- Aurora failover: ~30 seconds
- RDS failover: 60-120 seconds
- Aurora also has shared storage, auto-scaling read replicas

---

## LOAD BALANCERS

### Card 23
**FRONT:** Which load balancers have FREE cross-zone load balancing?

**BACK:**
- ALB: Cross-zone = FREE
- NLB: Cross-zone = COSTS MONEY
- GWLB: Cross-zone = COSTS MONEY

---

### Card 24
**FRONT:** How do you force HTTPS on Application Load Balancer?

**BACK:** Create listener rule to REDIRECT
- Port 80 listener with redirect action to port 443
- Returns 301/302 redirect to HTTPS
- Built-in ALB feature

---

## AWS BATCH & COMPUTE

### Card 25
**FRONT:** Thousands of batch jobs, 1-3 hours each, tolerates interruptions. Most cost-effective?

**BACK:** AWS BATCH with SPOT INSTANCES
- Purpose-built for batch computing
- Handles queuing, scheduling, provisioning
- Spot = up to 90% cost savings

---

### Card 26
**FRONT:** When do you use AWS Batch vs ECS Fargate?

**BACK:**
- AWS BATCH: Batch computing workloads (especially with Spot for cost savings)
- ECS FARGATE: General containerized applications, long-running services

---

## DATA MIGRATION

### Card 27
**FRONT:** How do you calculate if internet can handle data migration?

**BACK:**
1. Convert TB to Gigabits (TB × 8,000)
2. Divide by connection speed in Gbps
3. Result = seconds (divide by 86,400 for days)
4. If > deadline, use Snowball!

---

### Card 28
**FRONT:** 100 TB migration, 1 Gbps connection, 10-day deadline. What solution?

**BACK:** AWS SNOWBALL
- 100 TB = 800,000 Gb
- 1 Gbps = ~9.3 days minimum (won't make it with overhead)
- Snowball: delivery + copy + ship = ~10 days

---

### Card 29
**FRONT:** When do you use Snowball vs internet transfer?

**BACK:**
- SNOWBALL: Large data (>10 TB), tight deadline, slow internet
- INTERNET: Small data, fast connection, flexible timeline
- Rule: If internet transfer > days available, use Snowball

---

## ELASTICACHE

### Card 30
**FRONT:** RDS caching + cache miss auto-fetches from DB. What pattern?

**BACK:** ELASTICACHE with LAZY LOADING (cache-aside)
- Check cache first
- Miss → fetch from DB → populate cache
- Minimal code changes

---

### Card 31
**FRONT:** ElastiCache needs automatic failover within 5 minutes. What solution?

**BACK:** MULTI-AZ with AUTOMATIC FAILOVER
- Failover in 1-2 minutes (meets 5-min RTO)
- Automatic (no manual intervention)
- NOT just Redis persistence (takes 10+ minutes)

---

## SECURITY & ENCRYPTION

### Card 32
**FRONT:** What are the S3 encryption options and AWS access levels?

**BACK:**
- SSE-S3: AWS manages keys (AWS has access)
- SSE-KMS: AWS KMS manages (AWS has access)
- SSE-C: Customer provides keys (AWS uses but doesn't store)
- CLIENT-SIDE: Customer encrypts (AWS has ZERO access)

---

### Card 33
**FRONT:** "AWS must have NO access to encryption keys" = Which encryption?

**BACK:** CLIENT-SIDE ENCRYPTION
- Data encrypted before upload
- AWS never sees keys or unencrypted data
- Strongest compliance (or CloudHSM)

---

### Card 34
**FRONT:** What does CloudTrail log?

**BACK:** AWS API CALLS
- WHO: User/role identity
- WHEN: Timestamp
- WHAT: Resource affected, action taken
- For audit and compliance

---

### Card 35
**FRONT:** CloudTrail vs CloudWatch Logs vs Config vs VPC Flow Logs?

**BACK:**
- CLOUDTRAIL: API calls / audit trail
- CLOUDWATCH LOGS: Application/system logs
- CONFIG: Configuration changes / compliance
- VPC FLOW LOGS: Network traffic (IP/ports)

---

## ROUTE 53 ROUTING POLICIES

### Card 36
**FRONT:** "Data residency requirements" (EU data in EU, US in US) = Which Route 53 routing?

**BACK:** GEOLOCATION ROUTING
- Routes based on USER'S GEOGRAPHIC LOCATION
- Ensures EU users → EU region, US users → US region
- For compliance and data residency

---

### Card 37
**FRONT:** What's the difference between Geolocation and Latency-based routing?

**BACK:**
- GEOLOCATION: Routes based on USER LOCATION (for data residency)
- LATENCY: Routes to FASTEST region (for performance)
- Latency might route EU user to US if faster!

---

## MONITORING & SECURITY SERVICES

### Card 38
**FRONT:** Automated security vulnerability scans on EC2 (open ports, missing patches). Which service?

**BACK:** AMAZON INSPECTOR
- Purpose-built for security assessments
- Network exposure, software vulnerabilities
- Scheduled scans

---

### Card 39
**FRONT:** Inspector vs Config vs Security Hub?

**BACK:**
- INSPECTOR: Vulnerability scanning (does the scanning)
- CONFIG: Configuration compliance tracking
- SECURITY HUB: Aggregates findings (dashboard from Inspector, Config, etc.)

---

## EXAM KEYWORDS & PATTERNS

### Card 40
**FRONT:** What do these exam keywords indicate?
- "MOST cost-effective"
- "LEAST operational overhead"
- "MOST secure"

**BACK:**
- COST-EFFECTIVE: Spot, Glacier, Auto Scaling, Batch
- LEAST OVERHEAD: Managed services (Lambda, Fargate, Elastic Beanstalk)
- MOST SECURE: KMS, MFA, least privilege IAM, encryption

---

### Card 41
**FRONT:** How do you recognize when to use Partition placement group?

**BACK:** Look for:
- Kafka, Hadoop, Cassandra, Spark, Elasticsearch
- Large node count (40-50+)
- "Distributed system"
- "Rack failure isolation"

---

### Card 42
**FRONT:** RTO vs RPO - what's the difference?

**BACK:**
- RTO (Recovery Time Objective): How long to RECOVER (time to restore service)
- RPO (Recovery Point Objective): How much DATA LOSS acceptable (time between backups)

---

---

## PRINT INSTRUCTIONS

**Recommended Flashcard Format:**
- Print on cardstock or heavy paper
- Cut along horizontal lines between cards
- Fold each card in half (FRONT on one side, BACK on other)
- Or hand-write on blank index cards using this as reference

**Study Method:**
1. Quiz yourself on FRONT
2. Check BACK for answer
3. Cards you miss → separate pile for focused review
4. Repeat until 100% accuracy

**Focus Areas (based on your weaknesses):**
- Cards 1-5: Placement Groups (your biggest turnaround!)
- Cards 10-11: DynamoDB capacity modes
- Cards 25-26: AWS Batch
- Cards 27-29: Data migration math
- Card 36-37: Routing policies (new weak spot)

---

**Total Cards: 42**
**Estimated Hand-Writing Time: 60-90 minutes**
**Daily Review Time: 15-20 minutes**
