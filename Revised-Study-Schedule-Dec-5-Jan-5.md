# Revised AWS SAA-C03 Study Schedule
**Exam Date: January 14, 2026** *(Updated from Jan 5 - +9 days)*
**Study Period: December 5, 2025 - January 13, 2026 (40 days)**
**Daily Commitment: 1.5-2 hours**
**Current Progress: Week 2 in progress - DynamoDB drilling (Dec 15)**

---

## 🎯 Current Situation Assessment

**Good news:** You've got 31 days - enough time to learn properly without panic cramming.

**Current Status:**
- ✅ Days 1-3: EC2, Auto Scaling, S3 basics covered
- ⚠️ Day 7 comprehensive quiz: 45% (failed) - indicates weak foundation
- ✅ Database deep dive (Dec 1)
- ✅ VPC deep dive (Dec 2)
- 🔄 Need to solidify Week 1 concepts before moving forward

**Adjusted approach:**
- Week 1 foundation needs reinforcement
- You have time for proper practice exams
- Can afford deeper study of weak areas
- More hands-on practice opportunities

---

## Study Strategy

### Phase 1: Foundation Solidification (Dec 5-11) - 7 days
**Goal:** Master core services with strong foundation

### Phase 2: Advanced Services & Security (Dec 12-20) - 9 days
**Goal:** Complete all major service coverage

### Phase 3: Architecture & Integration (Dec 21-27) - 7 days
**Goal:** Multi-service scenarios and patterns

### Phase 4: Final Review & Practice Exams (Dec 28 - Jan 4) - 8 days
**Goal:** Peak performance and confidence

---

## Phase 1: Foundation Solidification (Dec 5-11)

### Day 1 - TODAY (Thursday, Dec 5)
**Topic: Week 1 Review & Gap Filling**
**Time: 2 hours**

You scored 45% on your Week 1 comprehensive quiz. Let's fix that.

- [ ] **Review Day-7-Week-1-Deep-Dive-Review.md (60 min)**
  - Focus on the 5 critical weak areas identified:
    1. S3 Storage Classes (4 questions missed)
    2. VPC NACLs (3 questions missed)
    3. Encryption/KMS (3 questions missed)
    4. Auto Scaling (2 questions missed)
    5. EC2/VPC Concepts (2 questions missed)

- [ ] **S3 Storage Classes Deep Dive (30 min)**
  - Memorize the decision tree from your deep-dive doc
  - Focus on retrieval time requirements
  - Keywords: "immediately" vs "within minutes" vs "within hours"
  - Practice: 10 questions on S3 storage classes only
  - Target: 9/10 (90%)

- [ ] **VPC NACLs vs Security Groups (30 min)**
  - Drill the stateful vs stateless concept
  - NACL = Stateless (need both inbound + outbound rules)
  - Security Group = Stateful (return traffic automatic)
  - Practice: 10 questions on NACLs/SGs
  - Target: 9/10 (90%)

### Day 2 - Friday, Dec 6
**Topic: Encryption & Auto Scaling (Weak Area Focus)**
**Time: 2 hours**

- [ ] **Encryption Deep Dive (60 min)**
  - S3 encryption: SSE-S3, SSE-KMS, SSE-C, client-side
  - EBS encryption with KMS
  - RDS encryption at rest
  - When CloudHSM vs KMS
  - Focus: Recognize encryption requirements in scenarios

- [ ] **Auto Scaling Policies Review (30 min)**
  - Target Tracking (simplest, least overhead)
  - Scheduled (predictable patterns)
  - Step Scaling (complex thresholds)
  - When to combine policies vs use single policy

- [ ] **Practice: 20 questions on encryption + auto scaling**
  - Target: 85%+ (17/20)

### Day 3 - Saturday, Dec 7
**Topic: Week 1 Comprehensive Retake**
**Time: 2.5 hours**

- [ ] **Retake Week 1 Comprehensive Quiz (60 min)**
  - Same or similar quiz to your Day 7 quiz
  - Target: 80%+ (16/20) - MUST PASS to move on
  - If below 80%, spend tomorrow drilling weak areas again

- [ ] **Review ALL answers (60 min)**
  - Understand why you got each one right or wrong
  - Update your weakness tracker
  - Create new flashcards for concepts you still struggle with

- [ ] **Flashcard Review (30 min)**
  - Review Week-1-Flashcards-Print-Template.md
  - Add any new concepts from your retake

**Decision point:**
- If 80%+: Proceed to Phase 2 (databases)
- If 70-79%: Do one more day of weak area drilling before Phase 2
- If <70%: Extend Phase 1 by 2-3 days

### Day 4 - Sunday, Dec 8
**Topic: Databases - RDS & Aurora**
**Time: 2 hours**

- [ ] **RDS Fundamentals (60 min)**
  - RDS engines (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB)
  - Multi-AZ vs Read Replicas (CRITICAL exam topic)
  - Automated backups vs manual snapshots
  - RDS Proxy (reduce connections, improve failover)
  - Encryption at rest and in transit

- [ ] **Aurora Deep Dive (45 min)**
  - Aurora architecture (6 copies across 3 AZs)
  - Aurora Serverless v1 vs v2
  - Aurora Global Database (cross-region)
  - Aurora Replicas (up to 15)
  - When to use Aurora vs RDS

- [ ] **Practice: 15 questions on RDS & Aurora**
  - Target: 80%+ (12/15)

### Day 5 - Monday, Dec 9
**Topic: DynamoDB & NoSQL**
**Time: 2 hours**

- [ ] **DynamoDB Core Concepts (75 min)**
  - Partition key + sort key design
  - GSI (Global Secondary Index) vs LSI (Local Secondary Index)
  - Provisioned vs On-Demand capacity modes
  - DynamoDB Streams (change data capture)
  - DAX (DynamoDB Accelerator) for caching
  - Global Tables (multi-region, multi-active)

- [ ] **When DynamoDB vs RDS (30 min)**
  - DynamoDB: Key-value, document, sub-millisecond latency, massive scale
  - RDS: Relational, complex queries, ACID transactions
  - Decision tree practice

- [ ] **Practice: 15 questions on DynamoDB**
  - Target: 80%+ (12/15)

### Day 6 - Tuesday, Dec 10
**Topic: Caching & Other Databases**
**Time: 2 hours**

- [ ] **ElastiCache (45 min)**
  - Redis vs Memcached (know the differences)
  - Redis: Advanced data structures, persistence, pub/sub, multi-AZ
  - Memcached: Simple, multi-threaded, no persistence
  - Common caching patterns
  - ElastiCache for RDS vs DAX for DynamoDB

- [ ] **Other Databases (45 min)**
  - Redshift (data warehouse, columnar, OLAP)
  - Neptune (graph database)
  - DocumentDB (MongoDB compatible)
  - Timestream (time series)
  - QLDB (ledger, immutable)
  - When to use each

- [ ] **Practice: 20 questions on caching + specialty databases**
  - Target: 75%+ (15/20)

### Day 7 - Wednesday, Dec 11
**Topic: Database Review & Assessment**
**Time: 2 hours**

- [ ] **Review all database notes (45 min)**
  - RDS, Aurora, DynamoDB, ElastiCache, specialty databases
  - Create comparison tables
  - Update flashcards

- [ ] **Database Comprehensive Quiz (45 min)**
  - 25 questions covering all database topics
  - Target: 80%+ (20/25)

- [ ] **Hands-on practice (30 min)** (if time)
  - Create RDS instance with Multi-AZ
  - Create DynamoDB table with GSI
  - Create ElastiCache cluster

---

## Phase 2: Advanced Services & Security (Dec 12-20)

### Day 8 - Thursday, Dec 12
**Topic: Lambda & Serverless Compute**
**Time: 2 hours**

- [ ] **Lambda Fundamentals (60 min)**
  - Lambda limits (memorize these):
    - 15-minute max timeout
    - 10 GB max memory
    - 1000 concurrent executions (default)
    - 512 MB /tmp storage
  - Cold starts and optimization
  - Lambda pricing model
  - Lambda versions and aliases
  - Lambda layers

- [ ] **Lambda Integrations (45 min)**
  - S3 events, DynamoDB Streams, API Gateway
  - EventBridge (CloudWatch Events)
  - SQS, SNS, Kinesis
  - Lambda@Edge, CloudFront Functions
  - Step Functions (orchestration)

- [ ] **Practice: 15 questions on Lambda**
  - Target: 80%+ (12/15)

### Day 9 - Friday, Dec 13
**Topic: Containers & Fargate**
**Time: 2 hours**

- [ ] **ECS (Elastic Container Service) (60 min)**
  - ECS on EC2 vs ECS on Fargate
  - Task definitions, services, clusters
  - ALB integration
  - ECS auto scaling vs EC2 auto scaling
  - When ECS vs EKS vs Lambda

- [ ] **EKS (Elastic Kubernetes Service) (30 min)**
  - Managed Kubernetes
  - When to use EKS vs ECS
  - EKS on Fargate

- [ ] **Elastic Beanstalk (30 min)**
  - Platform as a Service (PaaS)
  - Least operational overhead
  - Supports multiple languages/platforms
  - When Beanstalk vs ECS vs Lambda

- [ ] **Practice: 15 questions on containers**
  - Target: 75%+ (12/15)

### Day 10 - Saturday, Dec 14
**Topic: Integration Services - Part 1**
**Time: 2 hours**

- [ ] **SQS (Simple Queue Service) (45 min)**
  - Standard vs FIFO queues
  - Visibility timeout
  - Dead-letter queues (DLQ)
  - Long polling vs short polling
  - Delay queues
  - Message retention (up to 14 days)

- [ ] **SNS (Simple Notification Service) (30 min)**
  - Pub/Sub pattern
  - Topics and subscriptions
  - SNS + SQS fanout pattern
  - Message filtering

- [ ] **SQS + SNS Patterns (30 min)**
  - Decoupling architectures
  - Fanout pattern
  - When to use SQS vs SNS vs both

- [ ] **Practice: 15 questions on SQS & SNS**
  - Target: 80%+ (12/15)

### Day 11 - Sunday, Dec 15
**Topic: Integration Services - Part 2**
**Time: 2 hours**

- [ ] **EventBridge (60 min)**
  - Event-driven architectures
  - Rules and targets
  - Custom event buses
  - EventBridge vs CloudWatch Events vs SNS

- [ ] **Kinesis (60 min)**
  - Kinesis Data Streams (real-time)
  - Kinesis Data Firehose (near real-time, delivery)
  - Kinesis Data Analytics
  - Kinesis Video Streams
  - When Kinesis vs SQS

- [ ] **Practice: 15 questions on EventBridge & Kinesis**
  - Target: 75%+ (12/15)

### Day 12 - Monday, Dec 16
**Topic: IAM - Part 1 (CRITICAL)**
**Time: 2 hours**

- [ ] **IAM Fundamentals (75 min)**
  - Users, groups, roles, policies
  - Inline policies vs managed policies
  - Policy evaluation logic (explicit deny > explicit allow > implicit deny)
  - Permission boundaries
  - IAM best practices (least privilege, MFA, rotate credentials)

- [ ] **Cross-Account Access (30 min)**
  - AssumeRole
  - Trust policies vs permissions policies
  - Resource-based policies

- [ ] **Practice: 15 questions on IAM basics**
  - Target: 80%+ (12/15)

### Day 13 - Tuesday, Dec 17
**Topic: IAM - Part 2 & Organizations**
**Time: 2 hours**

- [ ] **AWS Organizations (60 min)**
  - Organizational Units (OUs)
  - Service Control Policies (SCPs)
  - SCP vs IAM policy (SCP sets boundaries)
  - Consolidated billing
  - Control Tower (automated setup)

- [ ] **IAM Advanced Topics (45 min)**
  - IAM Identity Center (SSO)
  - IAM Conditions
  - IAM Access Analyzer
  - Resource-based policies vs identity-based policies

- [ ] **Practice: 20 questions on IAM + Organizations**
  - Target: 80%+ (16/20)

### Day 14 - Wednesday, Dec 18
**Topic: Security Services - Part 1**
**Time: 2 hours**

- [ ] **KMS (Key Management Service) (60 min)**
  - Customer Master Keys (CMK) - AWS managed vs customer managed
  - Data keys
  - Key policies vs IAM policies
  - Automatic key rotation
  - KMS vs CloudHSM (FIPS 140-2 Level 3, custom key store)
  - Envelope encryption

- [ ] **Secrets Manager vs Parameter Store (30 min)**
  - Secrets Manager: Automatic rotation, RDS integration, costs money
  - Parameter Store: Free tier, hierarchical, no auto-rotation
  - When to use which

- [ ] **Certificate Manager (ACM) (15 min)**
  - Free SSL/TLS certificates
  - Automatic renewal
  - Integration with ALB, CloudFront, API Gateway

- [ ] **Practice: 15 questions on KMS & secrets**
  - Target: 80%+ (12/15)

### Day 15 - Thursday, Dec 19
**Topic: Security Services - Part 2**
**Time: 2 hours**

- [ ] **WAF (Web Application Firewall) (30 min)**
  - Protect against common web exploits
  - Rate limiting, geo-blocking, IP filtering
  - WAF rules and rule groups
  - Integration with ALB, CloudFront, API Gateway

- [ ] **Shield (30 min)**
  - Shield Standard (free, automatic DDoS protection)
  - Shield Advanced (costs money, enhanced protection)

- [ ] **GuardDuty, Security Hub, Inspector, Macie (60 min)**
  - GuardDuty: Threat detection using ML
  - Security Hub: Centralized security findings
  - Inspector: Vulnerability scanning (EC2, ECR, Lambda)
  - Macie: Discover and protect sensitive data in S3

- [ ] **Practice: 20 questions on security services**
  - Target: 75%+ (15/20)

### Day 16 - Friday, Dec 20
**Topic: Security Review & First Practice Exam**
**Time: 3 hours**

- [ ] **Security Review (60 min)**
  - Review all IAM + security notes
  - Create security decision trees
  - Update flashcards

- [ ] **FIRST FULL PRACTICE EXAM (90 min)**
  - Take AWS Skill Builder Official Practice Exam (20 questions) OR
  - Take full 65-question practice exam (130 minutes, timed)
  - Simulate real conditions
  - Target: 65%+ (don't panic if lower, you're still learning)

- [ ] **Quick scoring and gap identification (30 min)**
  - Note weak domains for Phase 3 focus

---

## Phase 3: Architecture & Integration (Dec 21-27)

### Day 17 - Saturday, Dec 21
**Topic: Practice Exam Review & Gap Analysis**
**Time: 2.5 hours**

- [ ] **Deep Review of Practice Exam (120 min)**
  - Review EVERY question (correct and incorrect)
  - Understand why each answer is right/wrong
  - Document weak topics by domain:
    - Design Resilient Architectures
    - Design High-Performing Architectures
    - Design Secure Applications
    - Design Cost-Optimized Architectures

- [ ] **Update weakness tracker (30 min)**
  - Identify top 5 weak topics
  - Plan focused study for next few days

### Day 18 - Sunday, Dec 22
**Topic: Monitoring & Logging**
**Time: 2 hours**

- [ ] **CloudWatch (60 min)**
  - Metrics, alarms, dashboards
  - CloudWatch Logs, Log Insights, Log subscriptions
  - CloudWatch custom metrics
  - CloudWatch Events → EventBridge
  - CloudWatch Synthetics (canaries)

- [ ] **CloudTrail (30 min)**
  - Management events vs data events
  - Multi-region trail
  - CloudTrail Insights
  - Integration with CloudWatch Logs

- [ ] **AWS Config (30 min)**
  - Configuration compliance tracking
  - Config rules (managed and custom)
  - Remediation actions

- [ ] **Practice: 15 questions on monitoring**
  - Target: 80%+ (12/15)

### Day 19 - Monday, Dec 23
**Topic: Cost Optimization**
**Time: 2 hours**

- [ ] **Cost Management Tools (45 min)**
  - Cost Explorer
  - AWS Budgets
  - Cost Allocation Tags
  - Billing alarms

- [ ] **Cost Optimization Strategies (60 min)**
  - EC2: Reserved Instances, Savings Plans, Spot Instances
  - S3: Storage class optimization, lifecycle policies, Intelligent-Tiering
  - RDS: Reserved Instances, Aurora Serverless
  - Right-sizing (Compute Optimizer, Trusted Advisor)
  - Data transfer costs

- [ ] **Practice: 15 questions on cost optimization**
  - Target: 75%+ (12/15)

### Day 20 - Tuesday, Dec 24 (Christmas Eve)
**Topic: Migration Services (Light Study Day)**
**Time: 1-1.5 hours**

- [ ] **Migration Overview (45 min)**
  - AWS Migration Hub
  - Application Discovery Service
  - Server Migration Service (SMS) → Application Migration Service
  - Database Migration Service (DMS)
  - Schema Conversion Tool (SCT)
  - CloudEndure (disaster recovery)

- [ ] **Snow Family (30 min)**
  - Snowcone (8-14 TB)
  - Snowball Edge (80 TB)
  - Snowmobile (100 PB)
  - When to use Snow vs online transfer (>10 TB + slow internet = Snow)

- [ ] **Transfer Family (15 min)**
  - SFTP, FTPS, FTP over S3

### Day 21 - Wednesday, Dec 25 (Christmas)
**BREAK DAY - No studying!**
Merry Christmas! Rest your brain.

### Day 22 - Thursday, Dec 26
**Topic: Hybrid & Storage Services**
**Time: 2 hours**

- [ ] **Storage Gateway (60 min)**
  - File Gateway (S3)
  - Volume Gateway (EBS snapshots) - Cached vs Stored modes
  - Tape Gateway (Glacier)
  - When to use each

- [ ] **FSx Family (45 min)**
  - FSx for Windows File Server (native Windows, SMB, AD integration)
  - FSx for Lustre (HPC, ML, sub-millisecond latency)
  - FSx for NetApp ONTAP (multi-protocol)
  - FSx for OpenZFS (NFS, snapshots)
  - When to use each

- [ ] **DataSync (15 min)**
  - Automated data transfer (on-prem to AWS)
  - Schedule-based sync

- [ ] **Practice: 15 questions on hybrid storage**
  - Target: 75%+ (12/15)

### Day 23 - Friday, Dec 27
**Topic: Well-Architected Framework & DR**
**Time: 2 hours**

- [ ] **Well-Architected Framework (75 min)**
  - Operational Excellence (IaC, small frequent changes, automation)
  - Security (defense in depth, least privilege, traceability)
  - Reliability (recover from failures, scale horizontally, automated recovery)
  - Performance Efficiency (serverless, multi-region, monitoring)
  - Cost Optimization (pay for what you use, managed services)
  - Sustainability (minimize resources, maximize utilization)

- [ ] **Disaster Recovery Strategies (45 min)**
  - Backup & Restore (cheapest, highest RTO/RPO)
  - Pilot Light (core services always running)
  - Warm Standby (scaled-down version running)
  - Multi-Site / Hot Site (most expensive, lowest RTO/RPO)
  - Map RTO/RPO requirements to DR strategy

- [ ] **Practice: 15 questions on architecture + DR**
  - Target: 80%+ (12/15)

---

## Phase 4: Final Review & Practice Exams (Dec 28 - Jan 4)

### Day 24 - Saturday, Dec 28
**Topic: Focused Weak Area Study - Day 1**
**Time: 3 hours**

Based on your first practice exam (Day 16) and ongoing quizzes, focus on your weakest 3 topics:

**Study approach:**
- [ ] Read AWS FAQs for weak services (90 min)
- [ ] Create decision trees for complex choices (30 min)
- [ ] Practice 40 questions on weak areas only (60 min)
- [ ] Target: 85%+ on weak area questions

### Day 25 - Sunday, Dec 29
**Topic: Focused Weak Area Study - Day 2**
**Time: 3 hours**

Continue weak area focus:

- [ ] Deep dive into remaining weak topics (90 min)
- [ ] Hands-on practice for weak services (if applicable) (30 min)
- [ ] Practice 40 more questions on weak areas (60 min)
- [ ] Target: 90%+ on weak area questions

### Day 26 - Monday, Dec 30
**Topic: SECOND FULL PRACTICE EXAM**
**Time: 3 hours**

- [ ] **Full Practice Exam #2 (130 min)**
  - Take 65-question practice exam (different source than first)
  - Timed, simulated conditions
  - Target: 72%+ (47/65 correct) - passing score

- [ ] **Review all answers (60 min)**
  - Understand every question
  - Note any remaining gaps
  - Update weakness list

### Day 27 - Tuesday, Dec 31 (New Year's Eve)
**Topic: Scenario-Based Architecture Questions**
**Time: 2 hours**

- [ ] **Multi-Service Scenarios (90 min)**
  - Complex scenarios requiring 3+ AWS services
  - Common patterns:
    - Web apps: ALB + EC2 + RDS + S3 + CloudFront
    - Serverless: API Gateway + Lambda + DynamoDB + S3
    - Data processing: S3 + Lambda + Kinesis + Athena
    - Hybrid: Direct Connect + Storage Gateway + VPN

- [ ] **Exam Keywords Drill (30 min)**
  - "MOST cost-effective" → Reserved Instances, Spot, S3 Glacier, Auto Scaling
  - "LEAST operational overhead" → Managed services, serverless, Elastic Beanstalk
  - "MOST secure" → KMS, MFA, least privilege, encryption at rest + transit
  - "High availability" → Multi-AZ, Auto Scaling, multi-region

- [ ] **Practice: 20 complex scenario questions**
  - Target: 75%+ (15/20)

### Day 28 - Wednesday, Jan 1 (New Year's Day)
**LIGHT STUDY DAY - Review only**
**Time: 1 hour**

- [ ] Review your cheat sheets (30 min)
- [ ] Review flashcards (30 min)
- [ ] Relax and enjoy New Year's Day!

### Day 29 - Thursday, Jan 2
**Topic: THIRD FULL PRACTICE EXAM**
**Time: 3 hours**

- [ ] **Full Practice Exam #3 (130 min)**
  - Final 65-question practice exam (different source)
  - Timed, simulated conditions
  - Target: 75%+ (49/65 correct)

- [ ] **Quick review of mistakes only (60 min)**
  - Focus on concepts you missed
  - Don't deep-dive, just refresh
  - Update your "always forget" list

### Day 30 - Friday, Jan 3
**Topic: Final Review & Confidence Building**
**Time: 2 hours**

- [ ] **Review all cheat sheets (60 min)**
  - Quick-Reference-*.md files
  - Your weakness trackers
  - Flashcards

- [ ] **Review exam strategy (30 min)**
  - Time management (2 min per question)
  - Elimination strategy
  - Flag and return approach
  - Common traps and distractors

- [ ] **Light practice (30 min)**
  - 20 questions, untimed, relaxed
  - Just to stay sharp
  - Target: 85%+

- [ ] **Logistics prep**
  - Confirm exam appointment
  - Check ID requirements (2 forms of ID)
  - Plan your route/timing
  - Prepare materials

### Day 31 - Saturday, Jan 4 (Day Before Exam)
**Topic: REST & MENTAL PREP**
**Time: 30 minutes MAX**

- [ ] **Morning only: Light review (30 min)**
  - Skim your personal notes
  - Look at your "always forget" list
  - Review your cheat sheets one last time

- [ ] **NO STUDYING AFTER LUNCH**
  - Seriously. Your brain needs rest.

- [ ] **Evening: Prepare & Relax**
  - Get exam materials ready (ID, confirmation)
  - Set multiple alarms
  - Relaxing activity (NOT studying)
  - Early bedtime (8+ hours sleep)

---

## Day 32 - Sunday, January 5, 2026
### 🎯 EXAM DAY

**Morning:**
- [ ] Eat a protein-rich breakfast
- [ ] Arrive 30 minutes early
- [ ] Bring TWO forms of ID
- [ ] Bathroom break before starting

**During exam:**
- [ ] Read questions carefully (keywords!)
- [ ] Eliminate wrong answers first
- [ ] Flag difficult questions, move on
- [ ] Trust your first instinct
- [ ] Manage time: ~2 min per question

**YOU'VE GOT THIS!** 🚀

---

## Study Resources

### Primary Resources
1. **AWS Skill Builder** - Exam Prep Standard Course
2. **AWS Official Practice Exams** (20 questions + 65 questions)
3. **Tutorials Dojo** - Jon Bonso practice exams (6 practice tests)
4. **Your Quick-Reference-*.md files** in this repo
5. **AWS Service FAQs** (high-yield!)

### AWS FAQs to Read (Priority Order)
1. S3 - https://aws.amazon.com/s3/faqs/
2. EC2 - https://aws.amazon.com/ec2/faqs/
3. VPC - https://aws.amazon.com/vpc/faqs/
4. IAM - https://aws.amazon.com/iam/faqs/
5. RDS - https://aws.amazon.com/rds/faqs/
6. Lambda - https://aws.amazon.com/lambda/faqs/
7. DynamoDB - https://aws.amazon.com/dynamodb/faqs/
8. Route 53 - https://aws.amazon.com/route53/faqs/

### Whitepapers (Optional)
- AWS Well-Architected Framework
- Architecting for the Cloud: AWS Best Practices

---

## Success Metrics

**Week 1 (Dec 5-11):** Week 1 retake 80%+
**Week 2 (Dec 12-20):** First practice exam 65%+
**Week 3 (Dec 21-27):** Second practice exam 72%+
**Week 4 (Dec 28-Jan 4):** Third practice exam 75%+
**Jan 5:** PASS REAL EXAM! (720/1000 = 72%)

---

## Daily Time Commitment

**Mon-Fri:** 1.5-2 hours/day
**Sat-Sun:** 2-3 hours/day
**Total:** ~50-60 hours over 31 days

This is sustainable! You can do this while working full-time.

---

## Critical Exam Tips

✅ **Keywords matter:**
- "MOST cost-effective" → look for cheaper options
- "LEAST operational overhead" → look for managed services
- "MOST secure" → look for encryption, least privilege
- "High availability" → look for Multi-AZ, Auto Scaling

✅ **Elimination strategy:**
- Cross out obviously wrong answers first
- Choose from remaining 2 options

✅ **Time management:**
- 2 minutes per question average
- Flag and return if stuck

✅ **Common traps:**
- Multi-AZ vs Read Replicas (Multi-AZ = HA, Read Replica = scale reads)
- S3 storage classes (check retrieval time!)
- NACLs are stateless (need both inbound + outbound rules)
- Auto Scaling: combine Scheduled + Target Tracking for mixed patterns

---

## If Practice Exam Scores Are Low...

**If Day 16 practice exam < 60%:**
- Extend Phase 2 by 2-3 days
- Do more focused study on weak domains

**If Day 26 practice exam < 70%:**
- Consider rescheduling exam to Jan 8-10
- Focus heavily on weak areas in final days

**Be honest with yourself. Better to delay slightly than fail and pay again.**

---

## Notes

You have 31 solid days. This is a MUCH better timeline than cramming in 12 days. Use the time wisely:
- Don't rush through topics
- Actually do the practice questions
- Review mistakes thoroughly
- Get hands-on experience where possible
- Read those FAQs (they're gold)

**Now let's get to work. Day 1 starts today!** 💪
