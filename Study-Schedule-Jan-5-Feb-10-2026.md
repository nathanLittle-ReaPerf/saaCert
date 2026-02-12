# AWS SAA-C03 Study Schedule - Fresh Start

**Exam Date: March 2, 2026 at 5:15 PM EST**
**Study Period: January 5 - March 1, 2026 (56 days)**
**Daily Commitment: 1.5-2 hours weekdays, 2-3 hours weekends**
**Target Score: 720/1000 (72% passing)**

---

## 🎯 Study Philosophy - Lessons from December

**What worked in December:**
- ✅ Targeted drilling on specific weaknesses (0% → 100% in days)
- ✅ Decision trees and pattern recognition
- ✅ Immediate quiz retakes after review
- ✅ You mastered 18+ topics! (DynamoDB, VPC, Auto Scaling, etc.)

**What didn't work:**
- ❌ Trying to push through while sick
- ❌ Not taking breaks during holidays
- ❌ Getting discouraged by bad quiz scores

**New approach:**
- 🎯 Build on your December victories (18 topics already mastered!)
- 🎯 Sustainable daily pace (no burnout)
- 🎯 Regular assessment without judgment
- 🎯 Focus on exam patterns, not just facts

---

## 📅 Study Structure (5 Weeks + Final Review)

### Week 1: Foundation Reset (Jan 5-11) - 7 days
**Goal:** Refresh December wins, assess current knowledge

### Week 2: Core Services Deep Dive (Jan 12-18) - 7 days
**Goal:** Master compute, storage, networking fundamentals

### Week 3: Advanced Services & Security (Jan 19-25) - 7 days
**Goal:** Databases, serverless, IAM, encryption

### Week 4: Integration & Architecture (Jan 26 - Feb 1) - 7 days
**Goal:** Multi-service scenarios, Well-Architected Framework

### Week 5: Practice Exams & Weak Areas (Feb 2-8) - 7 days
**Goal:** Full practice exams, targeted drilling

### Final Weekend: Light Review & Rest (Feb 9-10) - 2 days
**Goal:** Peak readiness, mental preparation

---

## Week 1: Foundation Reset (Jan 5-11)

### Day 1 - Sunday, Jan 5 (TODAY)
**Topic: Assessment & Game Plan**
**Time: 2 hours**

- [ ] **Morning: Read this entire study plan (30 min)**
  - Understand the strategy
  - Set realistic expectations
  - Get excited about the journey!

- [ ] **Baseline Assessment Quiz (60 min)**
  - Take 20-question quiz covering all major topics
  - Don't stress about the score - this is just to see where you are
  - Topics: EC2, S3, VPC, RDS, DynamoDB, Lambda, IAM
  - **Target: Whatever you get is fine - this is diagnostic only**

- [ ] **Review & Update Weakness-Tracker (30 min)**
  - Review quiz answers
  - Identify 3-5 weakest topics
  - Add to Weakness-Tracker.md
  - This becomes your focus for Week 2-3

**Evening: Relax! You've started the journey.**

---

### Day 2 - Monday, Jan 6
**Topic: EC2 Fundamentals Review**
**Time: 1.5 hours**

- [ ] **EC2 Core Concepts (60 min)**
  - Instance types (general purpose, compute, memory, storage)
  - Placement groups (Cluster, Partition, Spread) ← You mastered this in Dec!
  - Pricing models:
    - **On-Demand**: Highest cost, no commitment
    - **Reserved Instances**: 40-60% discount, 1-3 year commitment, PREDICTABLE workloads
    - **Spot Instances**: Up to 90% discount, interruptible
    - **Savings Plans**: Flexible commitment to compute spend
  - User Data scripts
  - Instance metadata service (IMDS)

- [ ] **EC2 Storage (30 min)**
  - EBS volume types (gp3, gp2, io2, io1, st1, sc1)
  - EBS snapshots (incremental, cross-region copy)
  - Instance Store (ephemeral, high IOPS, lost on stop)
  - EFS (shared file system, multi-AZ, NFSv4)

- [ ] **Quick Review Quiz: 10 EC2 questions**
  - Target: 80%+ (8/10)

---

### Day 3 - Tuesday, Jan 7
**Topic: S3 Deep Dive**
**Time: 1.5 hours**

- [ ] **S3 Core Features (45 min)**
  - S3 storage classes decision tree ← You mastered this in Dec!
    - Standard, Standard-IA, One Zone-IA
    - Glacier Instant Retrieval, Flexible Retrieval, Deep Archive
    - Intelligent-Tiering (automatic cost optimization)
  - S3 versioning
  - S3 replication (CRR, SRR)
  - S3 lifecycle policies
  - S3 Object Lock (WORM - Write Once Read Many)

- [ ] **S3 Security & Performance (30 min)**
  - Bucket policies vs IAM policies vs ACLs
  - S3 encryption (SSE-S3, SSE-KMS, SSE-C, client-side)
  - S3 Transfer Acceleration (uploads from far away)
  - Multipart upload (files >100 MB)
  - S3 Select and Glacier Select (query data in place)

- [ ] **Quick Review Quiz: 10 S3 questions**
  - Target: 80%+ (8/10)

---

### Day 4 - Wednesday, Jan 8
**Topic: VPC Fundamentals**
**Time: 1.5 hours**

- [ ] **VPC Core Concepts (60 min)**
  - VPC, Subnets (public vs private), Route Tables
  - Internet Gateway (IGW)
  - NAT Gateway vs NAT Instance
  - VPC Peering (non-transitive)
  - Security Groups vs NACLs ← You mastered this in Dec!
    - **Security Groups**: Stateful, allow rules only
    - **NACLs**: Stateless, allow + deny rules, need both inbound + outbound
  - VPC Endpoints ← You mastered this in Dec!
    - **Gateway endpoints**: S3, DynamoDB (FREE)
    - **Interface endpoints**: Other services (costs money)

- [ ] **VPC Advanced (30 min)**
  - VPC Flow Logs
  - Transit Gateway (hub-and-spoke)
  - Direct Connect (dedicated network connection)
  - VPN (encrypted over internet)

- [ ] **Quick Review Quiz: 10 VPC questions**
  - Target: 80%+ (8/10)

---

### Day 5 - Thursday, Jan 9
**Topic: Load Balancing & Auto Scaling**
**Time: 1.5 hours**

- [ ] **Elastic Load Balancing (45 min)**
  - **ALB (Application Load Balancer)**:
    - Layer 7 (HTTP/HTTPS)
    - Path-based, host-based routing
    - WebSocket support
    - Cross-zone load balancing (FREE)
  - **NLB (Network Load Balancer)**:
    - Layer 4 (TCP/UDP)
    - Ultra-high performance, low latency
    - Static IP, Elastic IP
    - Cross-zone load balancing (COSTS MONEY)
  - **GWLB (Gateway Load Balancer)**:
    - Layer 3 (IP packets)
    - Third-party virtual appliances
  - Target groups, health checks, sticky sessions

- [ ] **Auto Scaling (45 min)**
  - Auto Scaling Groups (ASG)
  - Launch Templates vs Launch Configurations
  - Scaling policies ← You mastered this in Dec!
    - **Target Tracking**: Simplest, maintain metric
    - **Scheduled Scaling**: Predictable patterns
    - **Step Scaling**: Complex thresholds
    - **Combination**: Scheduled + Target Tracking for mixed patterns
  - Health check types (EC2, ELB)
  - Cooldown periods

- [ ] **Quick Review Quiz: 10 ELB/ASG questions**
  - Target: 80%+ (8/10)

---

### Day 6 - Friday, Jan 10
**Topic: Route 53 & CloudFront**
**Time: 1.5 hours**

- [ ] **Route 53 (45 min)**
  - DNS basics (A, AAAA, CNAME, Alias, MX, TXT)
  - Routing policies:
    - **Simple**: Single resource
    - **Weighted**: Split traffic by %
    - **Latency**: Route to lowest latency region
    - **Failover**: Active-passive DR
    - **Geolocation**: Route by user location (data residency)
    - **Geoproximity**: Route by geographic distance
    - **Multi-Value**: Simple with health checks
  - Health checks
  - Traffic Flow (visual editor)

- [ ] **CloudFront (45 min)**
  - CDN (Content Delivery Network)
  - Edge locations, regional edge caches
  - Origins (S3, custom HTTP, ALB, EC2)
  - CloudFront signed URLs/cookies
  - Origin Access Control (OAC) - prevent direct S3 access
  - Field-level encryption
  - Lambda@Edge vs CloudFront Functions
  - CloudFront vs S3 Transfer Acceleration

- [ ] **Quick Review Quiz: 10 Route 53/CloudFront questions**
  - Target: 75%+ (8/10)

---

### Day 7 - Saturday, Jan 11
**Topic: Week 1 Comprehensive Assessment**
**Time: 2.5 hours**

- [ ] **Week 1 Comprehensive Quiz (90 min)**
  - 30 questions covering all Week 1 topics
  - EC2, S3, VPC, ELB, ASG, Route 53, CloudFront
  - Timed: 60 minutes (2 min/question)
  - **Target: 70%+ (21/30)**
  - Don't panic if lower - this is early in your prep!

- [ ] **Deep Review (60 min)**
  - Review EVERY question (right and wrong)
  - Understand why each answer is correct/incorrect
  - Update Weakness-Tracker.md with topics <70%
  - Identify top 3 weak areas for Week 2 focus

**Decision Point:**
- **70%+**: Great foundation! Proceed to Week 2
- **60-69%**: Good progress, note weak areas for extra review
- **<60%**: Consider extending Week 1 by 2-3 days for foundation

**Evening: Rest and recover. Week 1 complete!**

---

## Week 2: Core Services Deep Dive (Jan 12-18)

### Day 8 - Sunday, Jan 12
**Topic: RDS & Relational Databases**
**Time: 2 hours**

- [ ] **RDS Fundamentals (75 min)**
  - RDS engines (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB)
  - RDS Multi-AZ vs Read Replicas ← You mastered this in Dec!
    - **Multi-AZ**: High availability, synchronous replication, automatic failover
    - **Read Replicas**: Scale reads, asynchronous replication, can be promoted
  - Automated backups (1-35 days retention)
  - Manual snapshots (kept until deleted)
  - RDS Proxy ← You mastered this in Dec!
    - Connection pooling for Lambda + RDS
    - Reduce failover time
  - Encryption at rest (KMS) and in transit (SSL)
  - Cannot SSH into RDS instances (managed service)

- [ ] **Aurora (45 min)**
  - Aurora architecture (6 copies across 3 AZs, automatic)
  - Aurora MySQL vs PostgreSQL
  - Aurora Replicas (up to 15, <10ms replica lag)
  - Aurora Multi-Master ← You mastered this in Dec!
    - Multiple write nodes, <10 second failover
  - Aurora Serverless v1 vs v2
    - v1: On-demand, scales to zero, infrequent workloads
    - v2: Always-on, scales in seconds, production workloads
  - Aurora Global Database (cross-region, <1 second RPO)
  - Aurora Backtrack ← You mastered this in Dec!
    - MySQL only, rewind DB to point in time
  - When Aurora vs RDS (cost vs features tradeoff)

- [ ] **Practice Quiz: 15 RDS/Aurora questions**
  - Target: 80%+ (12/15)

---

### Day 9 - Monday, Jan 13
**Topic: DynamoDB Deep Dive**
**Time: 2 hours**

- [ ] **DynamoDB Core (90 min)**
  - NoSQL key-value and document database
  - Partition key + optional sort key
  - Partition key design best practices ← You mastered this in Dec!
  - DynamoDB operations ← You mastered this in Dec!
    - GetItem: Single item by primary key
    - Query: Items in partition, can filter sort key
    - Scan: All items (expensive, avoid if possible)
    - BatchGetItem: Multiple items (up to 100)
    - BatchWriteItem: Multiple items (up to 25)
  - GSI vs LSI ← Review from December
    - **GSI**: Different partition key, created anytime
    - **LSI**: Same partition key, created at table creation only
  - DynamoDB Streams ← You mastered this in Dec!
    - Change data capture, 24-hour retention
    - Trigger Lambda on changes
  - TTL (Time To Live) - automatic item expiration

- [ ] **DynamoDB Advanced (30 min)**
  - Capacity modes ← You mastered this in Dec!
    - **On-Demand**: Unpredictable traffic, no throttling
    - **Provisioned**: Predictable traffic, cheaper at scale
  - Read consistency ← You mastered this in Dec!
    - Eventually consistent: 50% cheaper, 1-2 second lag
    - Strongly consistent: More expensive, immediate
  - DAX (DynamoDB Accelerator): Microsecond latency cache
  - Global Tables: Multi-region, multi-active
  - DynamoDB Transactions: ACID across items

- [ ] **Practice Quiz: 15 DynamoDB questions**
  - Target: 80%+ (12/15)

---

### Day 10 - Tuesday, Jan 14
**Topic: ElastiCache & Other Databases**
**Time: 1.5 hours**

- [ ] **ElastiCache (60 min)**
  - In-memory caching service
  - **Redis vs Memcached**:
    - **Redis**: Advanced data structures, persistence, pub/sub, Multi-AZ, backup/restore, sorted sets
    - **Memcached**: Simple key-value, multi-threaded, no persistence, horizontal scaling
  - Caching strategies:
    - Lazy loading (cache-aside)
    - Write-through
    - Session store
  - Redis use cases: Leaderboards, rate limiting, pub/sub
  - ElastiCache for RDS vs DAX for DynamoDB

- [ ] **Other Databases (30 min)**
  - **Redshift** ← You mastered this in Dec!
    - Data warehouse, columnar storage, OLAP
    - For frequent analytics queries
  - **Athena** ← You mastered this in Dec!
    - Serverless SQL on S3
    - For infrequent analytics, ad-hoc queries
  - **Neptune**: Graph database
  - **DocumentDB**: MongoDB-compatible
  - **Timestream**: Time-series data
  - **QLDB** ← You mastered this in Dec!
    - Quantum Ledger Database, immutable, cryptographically verifiable
  - **Keyspaces**: Apache Cassandra-compatible

- [ ] **Practice Quiz: 15 caching/database questions**
  - Target: 75%+ (12/15)

---

### Day 11 - Wednesday, Jan 15
**Topic: Lambda & Serverless**
**Time: 1.5 hours**

- [ ] **Lambda Fundamentals (75 min)**
  - Event-driven compute service
  - **Lambda limits (MEMORIZE THESE)**:
    - 15-minute max timeout
    - 10 GB max memory (CPU scales with memory)
    - 1000 concurrent executions (soft limit, can increase)
    - 512 MB /tmp storage (ephemeral)
    - 50 MB deployment package (zipped), 250 MB unzipped
    - 6 MB sync invocation payload, 256 KB async
  - Lambda pricing: Pay per request + duration
  - Cold starts vs warm starts
  - Lambda layers (share code/libraries)
  - Lambda versions and aliases (blue/green deployments)
  - Environment variables
  - Lambda execution role (IAM)

- [ ] **Lambda Integrations (15 min)**
  - S3 events
  - DynamoDB Streams
  - API Gateway
  - EventBridge (CloudWatch Events)
  - SQS, SNS, Kinesis
  - Lambda@Edge, CloudFront Functions

- [ ] **Practice Quiz: 10 Lambda questions**
  - Target: 80%+ (8/10)

---

### Day 12 - Thursday, Jan 16
**Topic: Containers (ECS, EKS, Fargate)**
**Time: 1.5 hours**

- [ ] **ECS (Elastic Container Service) (60 min)**
  - Container orchestration service
  - **ECS on EC2 vs ECS on Fargate**:
    - **EC2**: Manage instances, cheaper at scale, more control
    - **Fargate**: Serverless, no instance management, pay per task
  - Task definitions (container specs)
  - Services (maintain desired count)
  - Clusters (logical grouping)
  - ECS task placement strategies
  - ALB integration for load balancing
  - ECS Service Auto Scaling vs EC2 Auto Scaling (different!)

- [ ] **EKS & Elastic Beanstalk (30 min)**
  - **EKS (Elastic Kubernetes Service)**:
    - Managed Kubernetes
    - When to use: Existing K8s expertise, portability
  - **Elastic Beanstalk**:
    - Platform as a Service (PaaS)
    - **LEAST operational overhead** keyword!
    - Supports: Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker
    - Automatically handles deployment, capacity, load balancing, scaling

- [ ] **Practice Quiz: 10 container questions**
  - Target: 75%+ (8/10)

---

### Day 13 - Friday, Jan 17
**Topic: Integration Services (SQS, SNS, EventBridge)**
**Time: 1.5 hours**

- [ ] **SQS (Simple Queue Service) (45 min)**
  - Fully managed message queue
  - **Standard vs FIFO queues**:
    - **Standard**: Best-effort ordering, at-least-once delivery, unlimited throughput
    - **FIFO**: Guaranteed order, exactly-once delivery, 300 msg/sec (3000 with batching)
  - Visibility timeout (default 30 sec, max 12 hours)
  - Message retention (1 min to 14 days, default 4 days)
  - Long polling (1-20 seconds) vs short polling
  - Dead-letter queues (DLQ) for failed messages
  - Delay queues (0-15 minutes)

- [ ] **SNS (Simple Notification Service) (30 min)**
  - Pub/Sub messaging service
  - Topics and subscriptions
  - Supported protocols: HTTP/S, Email, SMS, SQS, Lambda, Kinesis Firehose
  - SNS + SQS fanout pattern (one message to multiple queues)
  - Message filtering

- [ ] **EventBridge (15 min)**
  - Serverless event bus
  - Rules and targets
  - Event patterns (filter events)
  - Scheduled events (cron)
  - Custom event buses
  - EventBridge vs SNS (structured events vs simple pub/sub)

- [ ] **Practice Quiz: 10 integration questions**
  - Target: 80%+ (8/10)

---

### Day 14 - Saturday, Jan 18
**Topic: Week 2 Comprehensive Assessment**
**Time: 2.5 hours**

- [ ] **Week 2 Comprehensive Quiz (90 min)**
  - 30 questions covering Week 2 topics
  - RDS, Aurora, DynamoDB, ElastiCache, Lambda, ECS, SQS, SNS
  - Timed: 60 minutes
  - **Target: 75%+ (23/30)**

- [ ] **Deep Review & Planning (60 min)**
  - Review all answers
  - Update Weakness-Tracker.md
  - Identify persistent weak areas
  - Plan Week 3 focus topics

**Milestone check:**
- **75%+**: Excellent progress! Ready for advanced topics
- **65-74%**: Good progress, note weak areas
- **<65%**: Review weak topics before Week 3

---

## Week 3: Advanced Services & Security (Jan 19-25)

### Day 15 - Sunday, Jan 19
**Topic: IAM Fundamentals**
**Time: 2 hours**

- [ ] **IAM Core (90 min)**
  - Users, groups, roles, policies
  - Inline policies vs managed policies (AWS managed vs customer managed)
  - Policy structure (Effect, Principal, Action, Resource, Condition)
  - **Policy evaluation logic** (CRITICAL):
    1. Explicit Deny (always wins)
    2. Explicit Allow
    3. Implicit Deny (default)
  - Permission boundaries (set max permissions)
  - IAM best practices:
    - Least privilege principle
    - MFA for privileged users
    - Rotate credentials
    - Use roles, not access keys
    - Use groups, not individual user policies

- [ ] **Cross-Account & Roles (30 min)**
  - AssumeRole
  - Trust policies vs permissions policies
  - Resource-based policies (S3 bucket policies, etc.)
  - Service-linked roles
  - EC2 instance profiles

- [ ] **Practice Quiz: 15 IAM questions**
  - Target: 80%+ (12/15)

---

### Day 16 - Monday, Jan 20 (MLK Day - Extra Study Time!)
**Topic: Organizations & Advanced IAM**
**Time: 2 hours**

- [ ] **AWS Organizations (75 min)**
  - Organizational Units (OUs)
  - **Service Control Policies (SCPs)**:
    - Set permission boundaries for entire accounts/OUs
    - SCPs do NOT grant permissions, only limit them
    - Override account permissions (even root!)
  - Consolidated billing
  - Reserved Instance sharing
  - Control Tower (automated best-practice setup)

- [ ] **Advanced IAM (45 min)**
  - IAM Identity Center (AWS SSO)
  - IAM Access Analyzer (find public resources)
  - IAM Conditions (IP address, time, MFA, etc.)
  - Resource-based policies vs identity-based policies
  - Cross-account access patterns

- [ ] **Practice Quiz: 15 Organizations/IAM questions**
  - Target: 80%+ (12/15)

---

### Day 17 - Tuesday, Jan 21
**Topic: Encryption & Key Management**
**Time: 1.5 hours**

- [ ] **KMS (Key Management Service) (75 min)**
  - Customer Master Keys (CMKs):
    - **AWS managed**: Automatic rotation every 3 years
    - **Customer managed**: Optional rotation every year
    - **Customer managed with imported key material**: No auto rotation
  - KMS key policies vs IAM policies (both needed!)
  - Envelope encryption (data key encrypted by master key)
  - **KMS vs CloudHSM**:
    - **KMS**: AWS managed, shared hardware, FIPS 140-2 Level 2
    - **CloudHSM**: Dedicated hardware, FIPS 140-2 Level 3, custom encryption
  - KMS key types (symmetric vs asymmetric)
  - Multi-region keys

- [ ] **Secrets Management (15 min)**
  - **Secrets Manager**: Automatic rotation, RDS integration, costs money
  - **Parameter Store**: Hierarchical, free tier, no auto-rotation
  - When to use which

- [ ] **Practice Quiz: 10 encryption questions**
  - Target: 80%+ (8/10)

---

### Day 18 - Wednesday, Jan 22
**Topic: Security Services**
**Time: 1.5 hours**

- [ ] **Application Security (45 min)**
  - **WAF (Web Application Firewall)**:
    - Protect against OWASP top 10
    - Rate limiting, geo-blocking, IP filtering
    - WAF rules and managed rule groups
    - Integration: ALB, CloudFront, API Gateway
  - **Shield**:
    - **Shield Standard**: Free, automatic DDoS protection
    - **Shield Advanced**: Costs money, enhanced protection, cost protection
  - **Certificate Manager (ACM)**:
    - Free SSL/TLS certificates
    - Automatic renewal
    - Integration: ALB, CloudFront, API Gateway

- [ ] **Detection & Compliance (45 min)**
  - **GuardDuty**: Threat detection using ML (VPC Flow Logs, CloudTrail, DNS)
  - **Security Hub**: Centralized security findings
  - **Inspector**: Vulnerability scanning (EC2, ECR, Lambda)
  - **Macie**: Discover and protect sensitive data in S3 (PII, PHI)
  - **Detective**: Investigate security issues (uses GuardDuty findings)

- [ ] **Practice Quiz: 10 security questions**
  - Target: 75%+ (8/10)

---

### Day 19 - Thursday, Jan 23
**Topic: Monitoring & Logging**
**Time: 1.5 hours**

- [ ] **CloudWatch (75 min)**
  - **CloudWatch Metrics**:
    - Built-in metrics (CPU, network, disk)
    - Custom metrics (memory, disk usage inside instance)
    - Standard resolution (5 min) vs high resolution (1 sec)
  - **CloudWatch Alarms**:
    - Metric alarms, composite alarms
    - Actions: SNS, Auto Scaling, EC2 actions
  - **CloudWatch Logs**:
    - Log groups, log streams, retention
    - CloudWatch Logs Insights (query logs)
    - Log subscriptions (send to Lambda, Kinesis)
  - CloudWatch Dashboards
  - CloudWatch Synthetics (canaries for monitoring)

- [ ] **CloudTrail & Config (15 min)**
  - **CloudTrail**:
    - API call logging (who did what when)
    - Management events vs data events
    - Multi-region trail
    - CloudTrail Insights (detect unusual activity)
  - **AWS Config**:
    - Configuration compliance tracking
    - Config rules (managed and custom)
    - Remediation actions (SSM Automation)

- [ ] **Practice Quiz: 10 monitoring questions**
  - Target: 80%+ (8/10)

---

### Day 20 - Friday, Jan 24
**Topic: Disaster Recovery & Backup**
**Time: 1.5 hours**

- [ ] **DR Strategies (60 min)**
  - **RTO (Recovery Time Objective)**: How long to recover
  - **RPO (Recovery Point Objective)**: How much data loss acceptable
  - **DR Strategy Comparison**:
    1. **Backup & Restore**: Cheapest, hours RTO, variable RPO
       - S3 backups, restore when needed
    2. **Pilot Light**: Core systems always on, minutes-hours RTO
       - RDS replica, minimal EC2, scale up on disaster
    3. **Warm Standby**: Scaled-down version running, minutes RTO
       - Smaller instances, scale up on disaster
    4. **Multi-Site / Hot Site**: Full capacity running, seconds RTO
       - Active-active, most expensive

- [ ] **Backup Services (30 min)**
  - **AWS Backup**:
    - Centralized backup across services
    - Backup plans, vaults, compliance
    - Cross-region and cross-account backups
  - S3 versioning + lifecycle policies
  - RDS automated backups + snapshots
  - EBS snapshots
  - DynamoDB on-demand backups + PITR (point-in-time recovery)

- [ ] **Practice Quiz: 10 DR/backup questions**
  - Target: 75%+ (8/10)

---

### Day 21 - Saturday, Jan 25
**Topic: Week 3 Comprehensive Assessment**
**Time: 2.5 hours**

- [ ] **Week 3 Comprehensive Quiz (90 min)**
  - 30 questions covering Week 3 topics
  - IAM, Organizations, KMS, security services, monitoring, DR
  - Timed: 60 minutes
  - **Target: 75%+ (23/30)**

- [ ] **Deep Review (60 min)**
  - Review all answers
  - Update Weakness-Tracker.md
  - Note security topics that need more review

**Milestone check - 3 weeks done!**
- **75%+**: On track for passing!
- **65-74%**: Review weak areas during Week 4
- **<65%**: Schedule extra drilling sessions

---

## Week 4: Integration & Architecture (Jan 26 - Feb 1)

### Day 22 - Sunday, Jan 26
**Topic: API Gateway & App Integration**
**Time: 2 hours**

- [ ] **API Gateway (75 min)**
  - RESTful APIs and WebSocket APIs
  - Integration types:
    - Lambda (most common)
    - HTTP endpoints
    - AWS services (direct integration)
  - Stages and deployments
  - API keys and usage plans
  - Request/response transformations
  - Caching (reduce backend calls)
  - Throttling and quotas
  - CORS (Cross-Origin Resource Sharing)
  - API Gateway vs ALB for Lambda

- [ ] **Step Functions (30 min)**
  - Workflow orchestration for Lambda
  - State machines (choice, parallel, wait, etc.)
  - Standard vs Express workflows
  - Error handling and retries

- [ ] **AppSync (15 min)**
  - Managed GraphQL service
  - Real-time data sync
  - Offline support

- [ ] **Practice Quiz: 10 API/integration questions**
  - Target: 75%+ (8/10)

---

### Day 23 - Monday, Jan 27
**Topic: Data Processing & Analytics**
**Time: 1.5 hours**

- [ ] **Kinesis (60 min)**
  - **Kinesis Data Streams**:
    - Real-time data streaming
    - Shards (read 2 MB/s, write 1 MB/s per shard)
    - Retention: 1-365 days
    - Consumers: Lambda, Kinesis Data Analytics, EC2
  - **Kinesis Data Firehose**:
    - Near real-time (60 sec buffer)
    - Delivery to S3, Redshift, Elasticsearch, Splunk
    - No data retention, automatic scaling
  - **Kinesis Data Analytics**:
    - SQL queries on streaming data
  - **Kinesis Video Streams**:
    - Video streaming and storage
  - When Kinesis vs SQS

- [ ] **Other Analytics (30 min)**
  - **EMR (Elastic MapReduce)**: Hadoop, Spark, big data
  - **Glue**: ETL service, data catalog, crawlers
  - **QuickSight**: BI dashboards
  - **Data Pipeline**: Orchestrate data movement (legacy, use Glue instead)

- [ ] **Practice Quiz: 10 analytics questions**
  - Target: 70%+ (7/10)

---

### Day 24 - Tuesday, Jan 28
**Topic: Migration & Hybrid Cloud**
**Time: 1.5 hours**

- [ ] **Migration Services (60 min)**
  - **Application Migration Service (MGN)**: Lift-and-shift migrations
  - **Database Migration Service (DMS)**:
    - Homogeneous (MySQL → RDS MySQL)
    - Heterogeneous (Oracle → Aurora PostgreSQL with SCT)
    - Continuous replication (CDC)
  - **Schema Conversion Tool (SCT)**: Convert database schemas
  - **Migration Hub**: Track migrations
  - **Application Discovery Service**: Discover on-prem servers

- [ ] **Data Transfer (30 min)**
  - **Snow Family**:
    - **Snowcone**: 8-14 TB, IoT edge computing
    - **Snowball Edge**: 80 TB, edge computing
    - **Snowmobile**: 100 PB, exabyte-scale
    - **Decision**: >10 TB + slow internet = Snow
  - **DataSync**: Automated data transfer (on-prem ↔ AWS)
  - **Transfer Family**: SFTP/FTPS/FTP over S3
  - **Direct Connect**: Dedicated network connection (1-100 Gbps)
  - **VPN**: Encrypted over internet (cheaper, slower than DX)

- [ ] **Practice Quiz: 10 migration questions**
  - Target: 70%+ (7/10)

---

### Day 25 - Wednesday, Jan 29
**Topic: Hybrid Storage & File Systems**
**Time: 1.5 hours**

- [ ] **Storage Gateway (60 min)**
  - Hybrid cloud storage bridge
  - **File Gateway**: S3 (NFS/SMB protocol)
  - **Volume Gateway**: EBS snapshots
    - Cached volumes: Frequent data on-prem, full data in S3
    - Stored volumes: Full data on-prem, async backup to S3
  - **Tape Gateway**: Glacier (virtual tape library)
  - When to use each type

- [ ] **FSx Family (30 min)**
  - **FSx for Windows File Server**:
    - Native Windows, SMB protocol
    - Active Directory integration
    - DFS (Distributed File System)
  - **FSx for Lustre**:
    - High-performance computing (HPC)
    - Machine learning workloads
    - Sub-millisecond latency
    - Integration with S3
  - **FSx for NetApp ONTAP**: Multi-protocol (NFS, SMB, iSCSI)
  - **FSx for OpenZFS**: Linux workloads, snapshots

- [ ] **Practice Quiz: 10 storage questions**
  - Target: 70%+ (7/10)

---

### Day 26 - Thursday, Jan 30
**Topic: Well-Architected Framework**
**Time: 1.5 hours**

- [ ] **6 Pillars Deep Dive (90 min)**
  - **Operational Excellence**:
    - Infrastructure as Code (CloudFormation, CDK)
    - Small, frequent, reversible changes
    - Automation, monitoring, learning from failures
  - **Security**:
    - Defense in depth
    - Least privilege (IAM)
    - Traceability (CloudTrail, Config)
    - Encryption at rest and in transit
  - **Reliability**:
    - Automatic recovery from failure
    - Scale horizontally
    - Multi-AZ, multi-region
    - Test recovery procedures
    - Manage change through automation
  - **Performance Efficiency**:
    - Serverless architectures
    - Multi-region deployment
    - Experiment with different technologies
    - Monitoring and metrics
  - **Cost Optimization**:
    - Pay only for what you use
    - Managed services (reduce TCO)
    - Right-sizing
    - Reserved capacity for predictable workloads
  - **Sustainability** (newest pillar):
    - Minimize resources
    - Maximize utilization
    - Use managed services
    - Reduce data movement

- [ ] **Practice Quiz: 10 Well-Architected questions**
  - Target: 75%+ (8/10)

---

### Day 27 - Friday, Jan 31
**Topic: Multi-Service Architecture Scenarios**
**Time: 1.5 hours**

- [ ] **Common Architecture Patterns (60 min)**
  - **Classic 3-tier web app**:
    - Route 53 → CloudFront → ALB → EC2 ASG → RDS Multi-AZ
    - S3 for static assets
  - **Serverless web app**:
    - Route 53 → CloudFront → S3 (static site) → API Gateway → Lambda → DynamoDB
  - **Event-driven processing**:
    - S3 → Lambda → DynamoDB
    - S3 → SNS → multiple SQS → Lambda
  - **Data analytics pipeline**:
    - Kinesis Data Streams → Lambda → Kinesis Firehose → S3 → Athena/QuickSight
  - **Hybrid cloud**:
    - On-prem → Direct Connect/VPN → VPC → Storage Gateway → S3

- [ ] **Exam Keyword Drills (30 min)**
  - **"MOST cost-effective"**: Reserved Instances, Spot, S3 Glacier, Auto Scaling, serverless
  - **"LEAST operational overhead"**: Managed services, Elastic Beanstalk, Lambda, Fargate
  - **"MOST secure"**: KMS encryption, MFA, least privilege IAM, Shield/WAF
  - **"High availability"**: Multi-AZ, Auto Scaling, Route 53 health checks, multi-region
  - **"Lowest latency"**: CloudFront, DynamoDB DAX, ElastiCache, Placement Groups (Cluster)
  - **"Decoupling"**: SQS, SNS, EventBridge

- [ ] **Practice Quiz: 15 complex scenario questions**
  - Target: 75%+ (12/15)

---

### Day 28 - Saturday, Feb 1
**Topic: Week 4 Comprehensive + FIRST Practice Exam**
**Time: 3 hours**

- [ ] **FIRST FULL PRACTICE EXAM (130 min)**
  - 65 questions (full SAA-C03 exam length)
  - Timed: 130 minutes (2 hours 10 minutes)
  - Simulate real exam conditions:
    - No distractions
    - No looking up answers
    - Flag difficult questions, return later
  - **Target: 60-65%+ (39-42/65)**
  - First practice exam - don't panic about the score!

- [ ] **Quick scoring only (20 min)**
  - Calculate your score
  - Note weak domains (don't deep review yet)
  - Celebrate taking your first full practice exam!

**Evening: REST. Week 4 complete - you're in the final stretch!**

---

## Week 5: Practice Exams & Targeted Drilling (Feb 2-8)

### Day 29 - Sunday, Feb 2
**Topic: Practice Exam #1 Deep Review**
**Time: 3 hours**

- [ ] **Deep Review Every Question (150 min)**
  - Review ALL 65 questions (right and wrong)
  - For each question:
    - Why is the correct answer right?
    - Why are the wrong answers wrong?
    - What pattern/keyword should trigger the right answer?
  - Document by domain:
    - Design Resilient Architectures (30-32%)
    - Design High-Performing Architectures (28-30%)
    - Design Secure Applications (24-26%)
    - Design Cost-Optimized Architectures (20-22%)

- [ ] **Update Weakness-Tracker (30 min)**
  - Identify top 5 weakest topics
  - Create drilling plan for Day 30-32
  - Set specific improvement targets

---

### Day 30 - Monday, Feb 3
**Topic: Weakness Drilling - Day 1**
**Time: 2 hours**

- [ ] **Focus on weakest topic #1 (60 min)**
  - Re-read AWS FAQ for that service
  - Create decision tree or comparison table
  - Understand the "why" behind patterns

- [ ] **Focus on weakest topic #2 (60 min)**
  - Same approach as topic #1

- [ ] **Targeted Quiz: 20 questions on weak topics**
  - Target: 85%+ (17/20)

---

### Day 31 - Tuesday, Feb 4
**Topic: Weakness Drilling - Day 2**
**Time: 2 hours**

- [ ] **Focus on weakest topic #3 (60 min)**

- [ ] **Focus on weakest topic #4 (60 min)**

- [ ] **Targeted Quiz: 20 questions on weak topics**
  - Target: 85%+ (17/20)

---

### Day 32 - Wednesday, Feb 5
**Topic: Weakness Drilling - Day 3**
**Time: 2 hours**

- [ ] **Focus on weakest topic #5 (60 min)**

- [ ] **Review all weak area notes (30 min)**

- [ ] **Final Weak Area Quiz: 25 questions**
  - Covering ALL 5 weak topics from Practice Exam #1
  - Target: 90%+ (23/25)
  - If below 85%, continue drilling tomorrow

---

### Day 33 - Thursday, Feb 6
**Topic: SECOND FULL PRACTICE EXAM**
**Time: 3 hours**

- [ ] **Practice Exam #2 (130 min)**
  - 65 questions, timed
  - Different source/question bank than Exam #1
  - **Target: 70%+ (46/65)** - aiming for improvement!

- [ ] **Quick Review (50 min)**
  - Review incorrect answers only
  - Note any new weak topics
  - Update Weakness-Tracker

**Goal: Confirm improvement from Practice Exam #1**
- 5-10% improvement = on track
- <5% improvement = identify what's not clicking

---

### Day 34 - Friday, Feb 7
**Topic: Final Topic Review & Flashcards**
**Time: 2 hours**

- [ ] **Review all Quick-Reference cheat sheets (90 min)**
  - Quick-Reference-Compute.md
  - Quick-Reference-Storage.md
  - Quick-Reference-Networking.md
  - Quick-Reference-Databases.md
  - Quick-Reference-Security-IAM.md
  - Quick-Reference-Monitoring-DR-Other.md
  - Scan for facts you always forget

- [ ] **Flashcard review (30 min)**
  - Go through all flashcards
  - Make note of any you still miss

---

### Day 35 - Saturday, Feb 8
**Topic: THIRD FULL PRACTICE EXAM (Final Assessment)**
**Time: 3 hours**

- [ ] **Practice Exam #3 (130 min)**
  - 65 questions, timed
  - Different source than Exams #1-2
  - **Target: 72-75%+ (47-49/65)** - passing score!

- [ ] **Quick Review (50 min)**
  - Review incorrect answers
  - Make note of remaining weak spots
  - Update "always forget" list

**This is your final assessment before exam day**
- **72%+**: You're ready! 🎉
- **68-71%**: Close, focus on weak spots tomorrow
- **<68%**: Consider light review of fundamentals

---

## Final Weekend: Rest & Mental Prep (Feb 9-10)

### Day 36 - Sunday, Feb 9
**Topic: Light Review & Confidence Building**
**Time: 2 hours MAX**

- [ ] **Morning: Review "always forget" list (60 min)**
  - Your personal list of facts you keep missing
  - Key limits/numbers to memorize
  - Decision trees for tricky choices

- [ ] **Relaxed practice (30 min)**
  - 15-20 untimed questions
  - Just to stay sharp, not to learn new things
  - Target: 85%+

- [ ] **Exam strategy review (30 min)**
  - Time management (2 min per question = 130 min for 65 questions)
  - Elimination strategy (cross out wrong answers first)
  - Flag and return approach
  - Common trap patterns
  - Read questions for keywords (MOST, LEAST, HIGHEST, LOWEST)

**AFTERNOON: NO STUDYING**
- Do something fun and relaxing
- Exercise, movie, time with friends/family
- Early dinner

**EVENING: Exam logistics**
- [ ] Confirm exam appointment (time, location)
- [ ] Prepare 2 forms of ID
- [ ] Plan route/transportation (arrive 30 min early)
- [ ] Set multiple alarms
- [ ] Lay out clothes for tomorrow

**BEDTIME: Early! (Aim for 8+ hours sleep)**

---

### Day 37 - Monday, Feb 10 (Day Before Exam)
**Topic: REST DAY**
**Time: 30 minutes MAX**

- [ ] **Morning ONLY: Lightest possible review (30 min)**
  - Skim your cheat sheets ONE last time
  - Look at your "always forget" list
  - Remind yourself of key patterns

**NO STUDYING AFTER 12 PM**
- Seriously. Your brain needs rest more than more facts.
- You've done 36 days of studying. You're ready.

- [ ] **Afternoon/Evening: Relax**
  - Light exercise (walk, yoga)
  - Healthy meals
  - Hydrate well
  - Avoid alcohol and caffeine after 2 PM
  - Early bedtime (8+ hours sleep)

---

## Day 38 - Tuesday, February 11, 2026
### 🎯 EXAM DAY - 5:15 PM EST

**Morning/Afternoon:**
- [ ] Normal routine (don't cram!)
- [ ] Eat protein-rich meals
- [ ] Stay hydrated
- [ ] Light exercise/walk to reduce anxiety
- [ ] Arrive at test center 30 min early (4:45 PM)

**At Test Center:**
- [ ] Bring TWO forms of ID
- [ ] Use bathroom before exam
- [ ] Take deep breaths
- [ ] Remember: You've prepared for 37 days!

**During Exam:**
- [ ] Read questions carefully (keywords!)
- [ ] Eliminate obviously wrong answers
- [ ] Flag difficult questions, move on
- [ ] Trust your preparation
- [ ] Manage time (~2 min per question)
- [ ] Don't second-guess yourself too much

**YOU'VE GOT THIS!** 🚀

---

## Success Metrics & Milestones

**Week 1 (Jan 5-11):** 70%+ on comprehensive quiz
**Week 2 (Jan 12-18):** 75%+ on comprehensive quiz
**Week 3 (Jan 19-25):** 75%+ on comprehensive quiz
**Week 4 (Jan 26-Feb 1):** 60-65% on first practice exam
**Week 5 (Feb 2-8):**
- Practice Exam #2: 70%+
- Practice Exam #3: 72-75%+ (passing!)

**Passing score on real exam: 720/1000 (72%)**

---

## Study Resources

### Primary Resources
1. **This repository's Quick-Reference guides** (your best friend!)
2. **AWS Skill Builder** - Official Exam Prep course
3. **AWS Official Practice Exams**:
   - 20-question Official Practice Question Set
   - 65-question Official Practice Exam
4. **Tutorials Dojo** - Jon Bonso practice exams (highly recommended - 6 practice tests)
5. **AWS Service FAQs** (high-yield reading!)

### AWS FAQs (Priority Order - Read These!)
1. EC2 - https://aws.amazon.com/ec2/faqs/
2. S3 - https://aws.amazon.com/s3/faqs/
3. VPC - https://aws.amazon.com/vpc/faqs/
4. RDS - https://aws.amazon.com/rds/faqs/
5. IAM - https://aws.amazon.com/iam/faqs/
6. Lambda - https://aws.amazon.com/lambda/faqs/
7. DynamoDB - https://aws.amazon.com/dynamodb/faqs/
8. CloudFront - https://aws.amazon.com/cloudfront/faqs/

### Optional (If Time Permits)
- AWS Well-Architected Framework whitepaper
- Architecting for the Cloud: AWS Best Practices

---

## Daily Study Tips

**Consistency is key:**
- 1.5-2 hours weekdays (sustainable while working)
- 2-3 hours weekends (deeper dives)
- Total: ~60-70 hours over 37 days

**Active learning:**
- Don't just read - take quizzes daily
- Review wrong answers immediately
- Teach concepts out loud (if you can explain it, you know it)
- Draw decision trees and diagrams

**Track everything:**
- Update Weakness-Tracker.md after every quiz
- Note patterns in mistakes (not just topics)
- Celebrate wins (every mastered topic counts!)

**Take care of yourself:**
- Consistent sleep schedule
- Regular exercise
- Healthy meals
- Breaks when needed (this is a marathon, not a sprint)

---

## Exam Strategy Reminders

### Keywords Matter
- **"MOST cost-effective"** → Cheapest option (Reserved Instances, Spot, Glacier, serverless)
- **"LEAST operational overhead"** → Managed services (Elastic Beanstalk, Lambda, Fargate, RDS vs EC2 database)
- **"MOST secure"** → Encryption, MFA, least privilege, Shield/WAF
- **"Highest performance"** → Caching, CloudFront, Placement Groups, provisioned IOPS
- **"Highest availability"** → Multi-AZ, Auto Scaling, multi-region

### Elimination Strategy
1. Cross out obviously wrong answers first
2. Choose from remaining 2 options
3. Look for keywords that match one option
4. Trust your gut on 50/50 choices

### Time Management
- 130 minutes for 65 questions = 2 min per question
- Flag difficult questions, return later
- Don't get stuck on one question for 10 minutes
- Pace yourself: check time at question 20, 40, 60

### Common Traps
- Multi-AZ vs Read Replicas (Multi-AZ = HA, Read Replica = scale reads)
- S3 storage classes (check retrieval time requirements!)
- NACLs are stateless (need inbound AND outbound rules)
- Auto Scaling policies (combine Scheduled + Target Tracking for mixed patterns)
- CloudFront vs S3 Transfer Acceleration (CloudFront = downloads, Transfer Acceleration = uploads)

---

## If Things Don't Go As Planned...

**If quiz scores are consistently <70%:**
- Slow down, don't rush through topics
- Spend extra day on weak topics before moving on
- Consider extending study period by 1 week (exam can be rescheduled)

**If practice exams are <65%:**
- Don't panic! Practice exams are often harder than real exam
- Focus on weak domains (check your breakdown)
- Read more AWS FAQs for weak services
- Consider scheduling 1-on-1 tutoring for specific topics

**If you need to reschedule exam:**
- Better to delay and pass than rush and fail
- Exam costs $150 - worth taking when ready
- You can reschedule up to 24 hours before exam

---

## Encouragement for the Journey

You've got 37 solid days - plenty of time if you're consistent!

**Remember:**
- You already mastered 18 topics in December! You CAN do this.
- Getting sick during holidays was not a failure - it was life happening
- Starting fresh with a clean slate is a strength, not a weakness
- This exam is passable - 72% is the bar, and you can clear it

**Your December wins prove you can learn:**
- DynamoDB Query vs Scan: 0% → 100% in 10 days
- S3 Storage Classes: 40% → 100% in 12 days
- VPC NACLs: 0% → 100% in 3 days

You have the ability. Now you have the time. Let's make February 11 your victory day! 💪

---

**Last Updated:** January 4, 2026
**Next Update:** After Day 1 baseline assessment (January 5)
