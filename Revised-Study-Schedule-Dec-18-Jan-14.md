# AWS SAA-C03 Study Schedule - Final Push to Exam
**Created:** December 17, 2025
**Exam Date:** January 14, 2026 (Tuesday)
**Study Period:** December 18, 2025 - January 13, 2026
**Days Remaining:** 28 days
**Daily Commitment:** 1.5-2 hours (weekdays), 2-3 hours (weekends)

---

## 🎯 Current Status Assessment (Dec 17)

### ✅ Completed & Mastered (Strong Foundation)
- **Week 1 Topics (90% mastery):** EC2, S3, VPC, Auto Scaling, Load Balancers
- **Databases Part 1 (70%):** RDS, Aurora, Multi-AZ, Read Replicas
- **DynamoDB (80-90%):** Deep dive with 4 critical weaknesses conquered
  - Numeric Partition Key Anti-Pattern: 0% → 80%
  - Query Partition Key Requirements: 0% → 90%
  - Over-Engineering Rare Operations: 0% → 80%
  - Denormalization Patterns: 0% → 90%

### ⏳ Remaining Topics to Cover
- Lambda & Serverless
- Containers (ECS, EKS, Fargate, Elastic Beanstalk)
- Integration Services (SQS, SNS, EventBridge, Step Functions)
- Security & IAM (deep dive)
- Monitoring & Operations (CloudWatch, CloudTrail, Config, Systems Manager)
- Migration & Transfer Services
- Analytics (Athena, Kinesis, QuickSight, EMR)
- Architecture Patterns & Best Practices
- Practice Exams

---

## 📅 4-Week Study Plan

### **Week 1: Serverless & Integration (Dec 18-24) - 7 days**
Focus: Complete remaining compute and integration services

### **Week 2: Security & Operations (Dec 25-31) - 7 days**
Focus: Security, monitoring, migration, analytics

### **Week 3: Architecture & Practice (Jan 1-7) - 7 days**
Focus: Multi-service patterns and first practice exams

### **Week 4: Final Review & Exam (Jan 8-14) - 7 days**
Focus: Practice exams, weak area drilling, exam readiness

---

## Week 1: Serverless & Integration (Dec 18-24)

### Day 1 - Wednesday, Dec 18
**Topic: DynamoDB Verification + Lambda Intro**
**Time: 2 hours**

**Morning: DynamoDB Comprehensive Verification (60 min)**
- [ ] Take comprehensive DynamoDB quiz (25 questions)
- [ ] Cover: Partition keys, GSI/LSI, capacity modes, operations, denormalization
- [ ] **Target: 20/25 (80%)**
- [ ] If below 80%, identify which weaknesses need more work

**Afternoon: Lambda Fundamentals (60 min)**
- [ ] Read Quick-Reference-Compute.md Lambda section
- [ ] **Key limits to memorize:**
  - 15-minute max timeout
  - 10 GB max memory
  - 1000 concurrent executions (default)
  - 512 MB /tmp storage
  - 6 MB sync payload, 256 KB async
- [ ] Lambda pricing model (GB-seconds)
- [ ] Cold starts vs warm starts
- [ ] Lambda execution role (IAM)

**Evening: Quick quiz (optional)**
- [ ] 10 questions on Lambda basics
- [ ] Target: 7/10 (70%)

---

### Day 2 - Thursday, Dec 19
**Topic: Lambda Integrations & Patterns**
**Time: 2 hours**

**Lambda Event Sources (75 min)**
- [ ] S3 events (object created, deleted)
- [ ] DynamoDB Streams (change data capture)
- [ ] API Gateway (REST and HTTP APIs)
- [ ] EventBridge (CloudWatch Events)
- [ ] SQS, SNS, Kinesis
- [ ] Lambda@Edge vs CloudFront Functions (when to use each)

**Lambda Best Practices (30 min)**
- [ ] Error handling and retries
- [ ] Dead Letter Queues (DLQ)
- [ ] Environment variables and secrets
- [ ] VPC configuration (when needed, when to avoid)
- [ ] Concurrency and throttling

**Practice Quiz (15 min)**
- [ ] 15 questions on Lambda
- [ ] Target: 12/15 (80%)

---

### Day 3 - Friday, Dec 20
**Topic: Containers - ECS, EKS, Fargate**
**Time: 2 hours**

**ECS (Elastic Container Service) (60 min)**
- [ ] ECS on EC2 vs ECS on Fargate (when to use each)
- [ ] Task definitions (CPU, memory, networking)
- [ ] Services (desired count, auto scaling)
- [ ] Clusters and capacity providers
- [ ] ALB/NLB integration for load balancing
- [ ] ECS auto scaling (target tracking, step scaling)
- [ ] **Key decision: Fargate = least overhead, EC2 = more control/cost optimization**

**EKS & Elastic Beanstalk (45 min)**
- [ ] **EKS:** Managed Kubernetes, when to use vs ECS
- [ ] EKS on Fargate
- [ ] **Elastic Beanstalk:** PaaS, least operational overhead
- [ ] Beanstalk platforms (Java, .NET, Node.js, Python, Docker, etc.)
- [ ] **When to use what:**
  - Beanstalk: Simple web apps, least overhead
  - ECS: Docker containers, AWS-native
  - EKS: Kubernetes required, multi-cloud
  - Lambda: Event-driven, serverless

**Practice Quiz (15 min)**
- [ ] 15 questions on containers
- [ ] Target: 11/15 (75%)

---

### Day 4 - Saturday, Dec 21
**Topic: Integration Services Part 1 - SQS, SNS**
**Time: 2.5 hours**

**SQS (Simple Queue Service) (60 min)**
- [ ] Standard vs FIFO queues (key differences)
- [ ] Visibility timeout (default 30 sec, max 12 hours)
- [ ] Message retention (default 4 days, max 14 days)
- [ ] Long polling vs short polling
- [ ] Dead Letter Queues (DLQ) for failed messages
- [ ] **Decoupling pattern:** Producer → SQS → Consumer
- [ ] **Auto Scaling with SQS:** ApproximateNumberOfMessages metric

**SNS (Simple Notification Service) (45 min)**
- [ ] Pub/Sub pattern (one message, multiple subscribers)
- [ ] Subscribers: SQS, Lambda, HTTP/HTTPS, email, SMS
- [ ] SNS + SQS Fan-out pattern (critical exam pattern)
- [ ] Message filtering
- [ ] FIFO topics (with FIFO SQS subscriptions)

**SQS + SNS Patterns (30 min)**
- [ ] Fan-out: SNS → multiple SQS queues → multiple consumers
- [ ] Decoupling: Direct vs Queue vs Pub/Sub
- [ ] **When SQS vs SNS vs EventBridge**

**Practice Quiz (15 min)**
- [ ] 15 questions on SQS + SNS
- [ ] Target: 12/15 (80%)

---

### Day 5 - Sunday, Dec 22
**Topic: Integration Services Part 2 - EventBridge, Step Functions**
**Time: 2.5 hours**

**EventBridge (CloudWatch Events) (60 min)**
- [ ] Event bus (default, custom, partner)
- [ ] Event patterns and rules
- [ ] Targets: Lambda, SQS, SNS, Step Functions, etc.
- [ ] Scheduled events (cron, rate expressions)
- [ ] **EventBridge vs CloudWatch Events vs SNS:**
  - EventBridge: Advanced routing, filtering, schema registry
  - SNS: Simple pub/sub, multiple protocols
  - CloudWatch Events: Legacy, now EventBridge

**Step Functions (60 min)**
- [ ] State machines (standard vs express)
- [ ] States: Task, Choice, Parallel, Wait, Pass, Fail, Succeed
- [ ] Orchestrating Lambda functions (complex workflows)
- [ ] Error handling and retries
- [ ] **When Step Functions vs Lambda alone vs SQS**
- [ ] Integration with other services

**API Gateway (30 min)**
- [ ] REST API vs HTTP API vs WebSocket API
- [ ] Integration types: Lambda, HTTP, AWS services
- [ ] Throttling, caching, authorization
- [ ] API Gateway + Lambda pattern (serverless API)

**Practice Quiz**
- [ ] 20 questions on EventBridge, Step Functions, API Gateway
- [ ] Target: 16/20 (80%)

---

### Day 6 - Monday, Dec 23
**Topic: Specialty Databases & Caching**
**Time: 2 hours**

**ElastiCache (60 min)**
- [ ] **Redis vs Memcached (critical comparison)**
  - Redis: Persistence, replication, pub/sub, data structures, backup/restore
  - Memcached: Simple, multi-threaded, no persistence
- [ ] Cache strategies: Lazy loading, Write-through
- [ ] Session storage pattern (Redis for sessions)
- [ ] **When ElastiCache vs DAX vs CloudFront**

**Specialty Databases (45 min)**
- [ ] **Redshift:** Data warehouse, OLAP, columnar, analytics
- [ ] **DocumentDB:** MongoDB-compatible, document database
- [ ] **Neptune:** Graph database (social networks, fraud detection)
- [ ] **QLDB:** Immutable ledger, cryptographically verifiable
- [ ] **Timestream:** Time-series database (IoT, metrics)
- [ ] **Keyspaces:** Cassandra-compatible, wide-column

**When to Use What Database (15 min)**
- [ ] Create decision tree: Relational vs NoSQL vs Graph vs Ledger vs Time-series
- [ ] Memorize keywords: "audit trail" = QLDB, "graph" = Neptune, "warehouse" = Redshift

**Practice Quiz**
- [ ] 15 questions on caching + specialty databases
- [ ] Target: 11/15 (75%)

---

### Day 7 - Tuesday, Dec 24
**Topic: Week 1 Review & Serverless Architecture Patterns**
**Time: 2 hours**

**Review All Week 1 Topics (60 min)**
- [ ] Lambda (limits, integrations, patterns)
- [ ] Containers (ECS, EKS, Fargate, Beanstalk)
- [ ] Integration (SQS, SNS, EventBridge, Step Functions)
- [ ] Databases & Caching (ElastiCache, specialty databases)

**Serverless Architecture Patterns (45 min)**
- [ ] Review Serverless-Architecture-Patterns.md
- [ ] API Gateway + Lambda + DynamoDB (serverless API)
- [ ] S3 + Lambda + EventBridge (event-driven processing)
- [ ] SNS + SQS + Lambda (fan-out processing)
- [ ] Step Functions + Lambda (workflow orchestration)

**Comprehensive Quiz**
- [ ] 30 questions covering all Week 1 topics (Lambda, containers, integration)
- [ ] Target: 24/30 (80%)
- [ ] If below 80%, identify weak areas for tomorrow

---

## Week 2: Security & Operations (Dec 25-31)

### Day 8 - Wednesday, Dec 25
**Topic: IAM Deep Dive**
**Time: 2 hours (Christmas - lighter day)**

**IAM Fundamentals (60 min)**
- [ ] Users, Groups, Roles, Policies
- [ ] Identity-based vs Resource-based policies
- [ ] Inline vs Managed policies (AWS managed vs Customer managed)
- [ ] Policy evaluation logic (Explicit Deny > Allow > Implicit Deny)
- [ ] **Least privilege principle**
- [ ] IAM roles for EC2, Lambda, etc.
- [ ] Cross-account access (assume role)

**IAM Best Practices (30 min)**
- [ ] MFA for root and privileged users
- [ ] Password policies
- [ ] Access keys rotation
- [ ] IAM Access Analyzer (identify overly permissive policies)
- [ ] Service Control Policies (SCPs) in AWS Organizations
- [ ] **Exam pattern: "Most secure" = MFA + least privilege + deny overrides**

**Practice Quiz (30 min)**
- [ ] 15 questions on IAM
- [ ] Target: 12/15 (80%)

---

### Day 9 - Thursday, Dec 26
**Topic: Security Services Part 1**
**Time: 2 hours**

**KMS (Key Management Service) (60 min)**
- [ ] Customer Managed Keys (CMK) vs AWS Managed Keys
- [ ] Symmetric vs Asymmetric keys
- [ ] Key policies and grants
- [ ] Automatic key rotation (365 days for CMK)
- [ ] Envelope encryption
- [ ] **KMS vs CloudHSM:**
  - KMS: FIPS 140-2 Level 2, shared tenancy, AWS managed
  - CloudHSM: FIPS 140-2 Level 3, dedicated hardware, customer managed
- [ ] Integration with S3, EBS, RDS, DynamoDB

**Secrets Manager vs Systems Manager Parameter Store (45 min)**
- [ ] **Secrets Manager:**
  - Automatic rotation (Lambda-based)
  - RDS/Redshift/DocumentDB integration
  - Costs per secret + API calls
- [ ] **Parameter Store:**
  - Free tier (Standard parameters)
  - Advanced parameters (higher throughput, larger size, parameter policies)
  - No automatic rotation (manual via Lambda)
- [ ] **When to use which:** Rotation needed = Secrets Manager, otherwise Parameter Store

**Practice Quiz (15 min)**
- [ ] 15 questions on KMS, Secrets Manager, Parameter Store
- [ ] Target: 11/15 (75%)

---

### Day 10 - Friday, Dec 27
**Topic: Security Services Part 2**
**Time: 2 hours**

**WAF (Web Application Firewall) & Shield (60 min)**
- [ ] **WAF:** Layer 7 (HTTP/HTTPS) protection
  - Web ACLs (Access Control Lists)
  - Rules: IP sets, geo-matching, rate limiting, SQL injection, XSS
  - Integration: CloudFront, ALB, API Gateway, AppSync
- [ ] **Shield Standard:** Free, automatic DDoS protection
- [ ] **Shield Advanced:** $3000/month, enhanced DDoS, DDoS Response Team, cost protection
- [ ] **Firewall Manager:** Centrally manage WAF rules across accounts

**Security Monitoring & Compliance (45 min)**
- [ ] **GuardDuty:** Threat detection (ML-based, VPC Flow Logs, DNS logs, CloudTrail)
- [ ] **Inspector:** Vulnerability assessment for EC2, ECR, Lambda
- [ ] **Macie:** Data security for S3 (PII detection, data classification)
- [ ] **Detective:** Security investigation (visualize, analyze, investigate)
- [ ] **Security Hub:** Central security findings across accounts

**Network Security (15 min)**
- [ ] Security Groups (stateful, allow rules only)
- [ ] NACLs (stateless, allow + deny rules, numbered rules)
- [ ] VPC Flow Logs (capture IP traffic)
- [ ] VPN (Site-to-Site, Client VPN)
- [ ] Direct Connect (dedicated connection, NOT encrypted by default)
- [ ] PrivateLink (private connectivity to services)

**Practice Quiz**
- [ ] 20 questions on security services
- [ ] Target: 16/20 (80%)

---

### Day 11 - Saturday, Dec 28
**Topic: Monitoring & Operations Part 1**
**Time: 2.5 hours**

**CloudWatch (90 min)**
- [ ] **CloudWatch Metrics:**
  - Standard vs Detailed monitoring (1 min vs 5 min)
  - Custom metrics (PutMetricData API)
  - Metric math and anomaly detection
- [ ] **CloudWatch Alarms:**
  - Metric alarms, composite alarms
  - Actions: SNS, Auto Scaling, EC2 actions, Systems Manager
- [ ] **CloudWatch Logs:**
  - Log groups, log streams, retention
  - Metric filters (extract metrics from logs)
  - Log Insights (query logs)
  - Export to S3, stream to Lambda/Kinesis
- [ ] **CloudWatch Events → EventBridge**
- [ ] **CloudWatch Dashboards**

**CloudTrail (45 min)**
- [ ] API activity logging (who did what, when)
- [ ] Management events vs Data events vs Insights events
- [ ] Multi-region trail vs single-region
- [ ] Log file integrity validation
- [ ] Integration with CloudWatch Logs for real-time analysis
- [ ] **Exam pattern: "Audit API calls" = CloudTrail**

**Practice Quiz (15 min)**
- [ ] 15 questions on CloudWatch + CloudTrail
- [ ] Target: 12/15 (80%)

---

### Day 12 - Sunday, Dec 29
**Topic: Monitoring & Operations Part 2**
**Time: 2.5 hours**

**AWS Config (60 min)**
- [ ] Resource inventory and configuration history
- [ ] Config Rules (AWS managed vs custom Lambda)
- [ ] Compliance dashboard
- [ ] Remediation actions (SSM Automation)
- [ ] **Config vs CloudTrail:**
  - CloudTrail: WHO did WHAT and WHEN (API calls)
  - Config: WHAT changed and IS it compliant (resource configuration)

**Systems Manager (60 min)**
- [ ] **Session Manager:** Secure shell access without SSH/bastion
- [ ] **Parameter Store:** Configuration and secrets
- [ ] **Patch Manager:** Automated patching
- [ ] **Run Command:** Execute commands on instances
- [ ] **Automation:** Runbooks for common tasks
- [ ] **State Manager:** Maintain instance configuration
- [ ] **Inventory:** Collect metadata about instances

**Trusted Advisor & Other Tools (20 min)**
- [ ] Trusted Advisor: Best practice recommendations (cost, performance, security, fault tolerance, service limits)
- [ ] Free tier: 7 core checks, Full tier: All checks + API access
- [ ] **Service Quotas:** View and request quota increases
- [ ] **Health Dashboard:** Service and personal health events

**Practice Quiz (10 min)**
- [ ] 20 questions on Config, Systems Manager, Trusted Advisor
- [ ] Target: 15/20 (75%)

---

### Day 13 - Monday, Dec 30
**Topic: Migration & Transfer Services**
**Time: 2 hours**

**Snow Family (45 min)**
- [ ] **Snowcone:** 8-14 TB, edge computing, portable
- [ ] **Snowball Edge:**
  - Storage Optimized: 80 TB, 40 vCPUs
  - Compute Optimized: 42 TB, 104 vCPUs, GPU
- [ ] **Snowmobile:** Exabyte-scale (100 PB)
- [ ] **When to use:** Large data (>10 TB) + slow internet + tight deadline
- [ ] **Edge computing use cases**

**Migration Services (60 min)**
- [ ] **DataSync:** Automated data transfer (on-premises to AWS)
  - NFS, SMB to S3, EFS, FSx
  - Scheduling, bandwidth throttling, data validation
- [ ] **Transfer Family:** Managed SFTP/FTPS/FTP to S3 or EFS
- [ ] **Database Migration Service (DMS):**
  - Homogeneous (Oracle to Oracle) vs Heterogeneous (Oracle to Aurora)
  - Schema Conversion Tool (SCT) for heterogeneous
  - Continuous replication (CDC)
  - Snowball Edge integration for large databases
- [ ] **Application Discovery Service:** Discover on-premises servers
- [ ] **Application Migration Service (MGN):** Lift-and-shift rehosting
- [ ] **Migration Hub:** Central tracking of migrations

**Storage Gateway (15 min)**
- [ ] File Gateway (S3), Volume Gateway (EBS snapshots), Tape Gateway (Glacier)
- [ ] Hybrid cloud storage

**Practice Quiz**
- [ ] 15 questions on migration and transfer
- [ ] Target: 11/15 (75%)

---

### Day 14 - Tuesday, Dec 31
**Topic: Analytics & Data Processing**
**Time: 2 hours**

**Athena (30 min)**
- [ ] Serverless SQL queries on S3
- [ ] Pay per query (per TB scanned)
- [ ] Partitioning and columnar formats (Parquet, ORC) to reduce costs
- [ ] Federated queries (DynamoDB, RDS, Redshift, on-premises)
- [ ] **When Athena vs Redshift:** Ad-hoc = Athena, frequent = Redshift

**Kinesis (60 min)**
- [ ] **Kinesis Data Streams:**
  - Real-time streaming (shards, retention 1-365 days)
  - Producers: SDK, Kinesis Agent, Kinesis Producer Library (KPL)
  - Consumers: Kinesis Client Library (KCL), Lambda, Kinesis Data Firehose, Kinesis Data Analytics
- [ ] **Kinesis Data Firehose:**
  - Near real-time (60 sec buffer or 1 MB)
  - Load to S3, Redshift, Elasticsearch, Splunk
  - Automatic scaling, data transformation (Lambda)
- [ ] **Kinesis Data Analytics:**
  - SQL queries on streaming data
  - Real-time dashboards and anomaly detection
- [ ] **Kinesis Video Streams:** Video ingestion

**Other Analytics (30 min)**
- [ ] **QuickSight:** BI dashboards (serverless, ML insights)
- [ ] **EMR (Elastic MapReduce):** Big data (Hadoop, Spark, Hive, Presto)
- [ ] **Glue:** ETL service, Data Catalog, Crawlers, Job Bookmarks
- [ ] **Data Pipeline:** Orchestrate data workflows (legacy, use Step Functions or Glue)
- [ ] **MSK (Managed Streaming for Kafka):** Fully managed Kafka

**Practice Quiz**
- [ ] 20 questions on analytics
- [ ] Target: 14/20 (70%)

---

## Week 3: Architecture & Practice (Jan 1-7)

### Day 15 - Wednesday, Jan 1
**Topic: Disaster Recovery & High Availability**
**Time: 2 hours (New Year's Day - lighter)**

**DR Strategies (60 min)**
- [ ] **Backup and Restore:** Low cost, high RTO/RPO (hours/days)
- [ ] **Pilot Light:** Minimal replication, medium RTO (10 min - hours)
- [ ] **Warm Standby:** Scaled-down version running, medium RTO (minutes)
- [ ] **Multi-Site Active-Active:** Full capacity, lowest RTO (real-time)
- [ ] **RTO (Recovery Time Objective) vs RPO (Recovery Point Objective)**
- [ ] Map requirements to strategies:
  - RTO hours + RPO hours = Backup/Restore
  - RTO <1 hour + RPO minutes = Pilot Light or Warm Standby
  - RTO <1 minute + RPO seconds = Multi-Site

**High Availability Patterns (45 min)**
- [ ] Multi-AZ deployments (RDS, ElastiCache, EFS)
- [ ] Auto Scaling Groups across AZs
- [ ] Load balancers (ALB, NLB cross-zone)
- [ ] Route 53 health checks and failover
- [ ] CloudFront + S3 for static content
- [ ] Read Replicas for read scaling

**Practice Quiz (15 min)**
- [ ] 15 questions on DR and HA
- [ ] Target: 12/15 (80%)

---

### Day 16 - Thursday, Jan 2
**Topic: Cost Optimization & Performance**
**Time: 2 hours**

**Cost Optimization (60 min)**
- [ ] EC2 pricing: On-Demand, Reserved, Savings Plans, Spot
- [ ] S3 storage classes and lifecycle policies
- [ ] EBS volume types (gp3 cheaper than gp2)
- [ ] Compute Optimizer recommendations
- [ ] Cost Explorer and Cost and Usage Reports
- [ ] Budgets and billing alarms
- [ ] **Exam keywords:** "MOST cost-effective"
  - Look for: Spot, S3 Glacier, Auto Scaling, Lambda, Fargate

**Performance Optimization (45 min)**
- [ ] CloudFront (edge caching, reduce latency)
- [ ] ElastiCache and DAX (in-memory caching)
- [ ] Read Replicas (read scaling)
- [ ] EBS volume types (io2 for max IOPS, gp3 for balanced)
- [ ] EC2 instance types (compute, memory, storage optimized)
- [ ] Auto Scaling for elasticity

**Review Key Metrics to Memorize (15 min)**
- [ ] Lambda: 15 min timeout, 10 GB memory
- [ ] S3: 5 GB single PUT, use multipart >100 MB
- [ ] EBS: gp3 (3000-16000 IOPS), io2 Block Express (256,000 IOPS)
- [ ] RDS: 5 Read Replicas (Aurora: 15)

**Practice Quiz**
- [ ] 20 questions on cost and performance optimization
- [ ] Target: 16/20 (80%)

---

### Day 17 - Friday, Jan 3
**Topic: Architecture Patterns & Best Practices**
**Time: 2 hours**

**Well-Architected Framework (60 min)**
- [ ] **6 Pillars:**
  1. Operational Excellence
  2. Security
  3. Reliability
  4. Performance Efficiency
  5. Cost Optimization
  6. Sustainability
- [ ] Review Exam-Strategy-Tips.md
- [ ] Pattern recognition for exam questions:
  - "LEAST operational overhead" → Managed services
  - "MOST cost-effective" → Cheapest that meets requirements
  - "MOST secure" → KMS, MFA, least privilege

**Common Architecture Patterns (45 min)**
- [ ] 3-Tier Web Application (ALB + EC2/ECS + RDS)
- [ ] Serverless Web Application (CloudFront + S3 + API Gateway + Lambda + DynamoDB)
- [ ] Event-Driven Architecture (S3/DynamoDB + Lambda + SQS/SNS)
- [ ] Microservices (ECS/EKS + ALB + RDS/DynamoDB)
- [ ] Data Lake (S3 + Glue + Athena + QuickSight)
- [ ] Hybrid Cloud (Direct Connect + Storage Gateway + DataSync)

**Practice Scenarios (15 min)**
- [ ] Review Practice-Scenarios.md (select 5 scenarios)
- [ ] Focus on multi-service solutions

---

### Day 18 - Saturday, Jan 4
**Topic: First Practice Exam**
**Time: 3 hours**

**Practice Exam 1 (130 min = 2h 10min)**
- [ ] Take full practice exam (65 questions, 130 minutes)
- [ ] Simulate exam conditions (no distractions, timed)
- [ ] **Target: 45/65 (70%)** - First benchmark

**Review & Analysis (50 min)**
- [ ] Review ALL answers (correct and incorrect)
- [ ] Identify patterns in mistakes
- [ ] Create list of weak areas
- [ ] Update Weakness-Tracker.md with new gaps

**Action Plan**
- [ ] If >70%: Proceed to more practice exams
- [ ] If 60-70%: Identify 3-5 weak topics, drill tomorrow
- [ ] If <60%: Need focused review, adjust schedule

---

### Day 19 - Sunday, Jan 5
**Topic: Weak Area Drilling (Based on Practice Exam 1)**
**Time: 3 hours**

**Targeted Review (2 hours)**
- [ ] Review the 3-5 weakest topics from Practice Exam 1
- [ ] Deep dive into those areas using Quick Reference guides
- [ ] Create flashcards for concepts you missed

**Targeted Quizzes (60 min)**
- [ ] 20 questions on each weak topic
- [ ] Target: 80%+ on each

**Track Progress**
- [ ] Update Weakness-Tracker.md
- [ ] Confirm weak areas are improving

---

## Week 4: Final Review & Exam (Jan 8-14)

### Day 20 - Wednesday, Jan 8
**Topic: Practice Exam 2**
**Time: 3 hours**

**Practice Exam 2 (130 min)**
- [ ] New practice exam (65 questions)
- [ ] Timed, full exam simulation
- [ ] **Target: 52/65 (80%)** - Exam passing threshold

**Review & Analysis (50 min)**
- [ ] Review all answers
- [ ] Compare performance to Exam 1
- [ ] Identify remaining weak areas

**Action Plan**
- [ ] If >80%: You're exam-ready, keep reviewing
- [ ] If 70-80%: Good progress, drill weak areas
- [ ] If <70%: Need intensive review next 3 days

---

### Day 21 - Thursday, Jan 9
**Topic: Weak Area Drilling Round 2**
**Time: 2 hours**

**Focus on Remaining Gaps**
- [ ] Review topics still below 80% accuracy
- [ ] Targeted drilling with 50+ questions on weak areas
- [ ] Use Practice-Scenarios-Hard-Mode.md for difficult scenarios

**Flashcard Review**
- [ ] Review all flashcards created throughout study period
- [ ] Focus on topics you keep getting wrong

---

### Day 22 - Friday, Jan 10
**Topic: Practice Exam 3**
**Time: 3 hours**

**Practice Exam 3 (130 min)**
- [ ] Third full practice exam
- [ ] **Target: 55/65 (85%)** - Above passing, exam confidence

**Review & Final Adjustments (50 min)**
- [ ] Identify any last remaining gaps
- [ ] Create final review list for next 2 days

---

### Day 23 - Saturday, Jan 11
**Topic: Comprehensive Review Day 1**
**Time: 3 hours**

**Review All Quick Reference Guides (3 hours)**
- [ ] Quick-Reference-Compute.md (30 min)
- [ ] Quick-Reference-Storage.md (30 min)
- [ ] Quick-Reference-Networking.md (30 min)
- [ ] Quick-Reference-Databases.md (30 min)
- [ ] Quick-Reference-Security-IAM.md (20 min)
- [ ] Quick-Reference-Monitoring-DR-Other.md (20 min)
- [ ] Quick-Reference-Analytics.md (20 min)
- [ ] Quick-Reference-Migration.md (20 min)

**Focus on:**
- [ ] Comparison tables (when to use X vs Y)
- [ ] Limits and specifications to memorize
- [ ] Exam pattern keywords

---

### Day 24 - Sunday, Jan 12
**Topic: Comprehensive Review Day 2 + Practice Exam 4**
**Time: 4 hours**

**Morning: Flashcard Blitz (90 min)**
- [ ] Review ALL flashcards (Week 1, DynamoDB, new topics)
- [ ] Focus on concepts you keep forgetting
- [ ] Memorize critical limits and specifications

**Afternoon: Practice Exam 4 (130 min)**
- [ ] Final practice exam before real exam
- [ ] **Target: 55/65 (85%+)**
- [ ] Build confidence

**Evening: Light Review (60 min)**
- [ ] Review any mistakes from Exam 4
- [ ] Don't cram new material
- [ ] Relax and build confidence

---

### Day 25 - Monday, Jan 13
**Topic: Final Review & Relaxation**
**Time: 1.5 hours (LIGHT day before exam)**

**Morning: Light Review (60 min)**
- [ ] Review Exam-Strategy-Tips.md
- [ ] Review key decision trees and comparison tables
- [ ] Review common exam keywords and patterns

**Afternoon: Exam Preparation (30 min)**
- [ ] Confirm exam time and location/online setup
- [ ] Prepare ID and required materials
- [ ] Test computer and internet (if online exam)
- [ ] Get 8+ hours of sleep tonight

**DO NOT:**
- [ ] ❌ Cram new material
- [ ] ❌ Take full practice exams
- [ ] ❌ Study late into the night

**DO:**
- [ ] ✅ Light review of key concepts
- [ ] ✅ Relax and stay confident
- [ ] ✅ Get proper sleep and nutrition
- [ ] ✅ Trust your preparation

---

### Day 26 - Tuesday, Jan 14
**EXAM DAY** 🎯

**Pre-Exam (1-2 hours before)**
- [ ] Light breakfast (avoid heavy food)
- [ ] Arrive 30 min early (test center) or log in 15 min early (online)
- [ ] Quick mental review of key patterns (no heavy studying)
- [ ] Deep breaths, stay calm

**During Exam (130 minutes)**
- [ ] Read each question carefully (keywords matter!)
- [ ] Eliminate obviously wrong answers first
- [ ] Flag difficult questions, come back later
- [ ] Manage your time (65 questions in 130 min = 2 min per question)
- [ ] Trust your preparation

**Exam Keywords to Remember:**
- "MOST cost-effective" → Look for cheapest option
- "LEAST operational overhead" → Managed services, serverless
- "MOST secure" → KMS, MFA, least privilege
- "High availability" → Multi-AZ, Auto Scaling
- "Lowest latency" → CloudFront, ElastiCache, edge locations

**Post-Exam:**
- [ ] Celebrate! You prepared thoroughly
- [ ] Provisional results immediately (pass/fail)
- [ ] Official score report within 5 business days

---

## 📊 Study Progress Tracking

### Weekly Targets
- **Week 1 (Dec 18-24):** Complete Lambda, Containers, Integration, Databases
- **Week 2 (Dec 25-31):** Complete Security, Monitoring, Migration, Analytics
- **Week 3 (Jan 1-7):** Architecture patterns + 2 practice exams (>70%)
- **Week 4 (Jan 8-14):** 2 more practice exams (>80%) + final review + EXAM

### Success Metrics
- Practice Exam 1 (Jan 4): >70% (baseline)
- Practice Exam 2 (Jan 8): >80% (exam passing)
- Practice Exam 3 (Jan 10): >85% (confidence building)
- Practice Exam 4 (Jan 12): >85% (final validation)
- **Real Exam (Jan 14): >720/1000 (PASS!)**

---

## 🎯 Key Principles for Next 28 Days

1. **Quality over speed:** Don't rush through topics just to check boxes
2. **Drill weaknesses:** When you score <80%, drill that topic until >90%
3. **Practice exams matter:** They reveal gaps and build exam stamina
4. **Trust your deep work:** Your DynamoDB deep dive proves your method works
5. **Stay consistent:** 1.5-2 hours daily is better than cramming
6. **Sleep matters:** Don't sacrifice sleep for studying (especially exam week)
7. **You've got this:** 28 days is enough time with focused effort

---

**Created:** December 17, 2025
**Next Update:** After Practice Exam 1 (Jan 4) to adjust final week if needed
**Maintained by:** Claude Code + Your dedication

Good luck! You're going to crush this exam. 💪
