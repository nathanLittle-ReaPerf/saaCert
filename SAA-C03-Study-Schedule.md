# AWS SAA-C03 Study Schedule
**Exam Date: January 5, 2026** ⚠️ UPDATED
**Study Period: December 5, 2025 - January 4, 2026 (31 days)** ⚠️ REVISED
**Daily Commitment: 1.5-2 hours**
**Current Progress: Week 1 foundation completed (with gaps) | See Revised-Study-Schedule-Dec-5-Jan-5.md**

⚠️ **NOTE:** This is the ORIGINAL schedule from November. See **Revised-Study-Schedule-Dec-5-Jan-5.md** for your current 31-day plan starting December 5th.

---

## Week 1: Core Services Foundation (Nov 21-27)

### Day 1 - Thursday, Nov 21 ✅ COMPLETED
**Topic: EC2 Fundamentals & Instance Types**
- [x] Review EC2 instance types and use cases (compute optimized, memory optimized, storage optimized)
- [x] Study placement groups (cluster, spread, partition)
- [x] Understand EC2 pricing models (On-Demand, Reserved, Spot, Savings Plans)
- [x] Practice: Launch different instance types, test placement groups
- [x] Practice Questions: 20 questions on EC2

### Day 2 - Friday, Nov 22 ✅ COMPLETED (Catch-up on Nov 24)
**Topic: Auto Scaling & Load Balancing**
- [x] Auto Scaling groups, scaling policies (target tracking, step, scheduled)
- [x] Load balancer types: ALB vs NLB vs GLB (use cases and differences)
- [x] Connection draining, health checks, sticky sessions
- [x] Created study materials: Day-2-Catchup-Auto-Scaling-Load-Balancing.md
- [x] Created cheat sheet: Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.md
- [x] Completed practice quiz (achieved 100% on load balancer quiz)

### Day 3 - Saturday, Nov 23 ✅ COMPLETED
**Topic: S3 Storage Classes & Lifecycle**
- [x] S3 storage classes (Standard, IA, One Zone-IA, Glacier, Glacier Deep Archive, Intelligent-Tiering)
- [x] Lifecycle policies and transitions
- [x] S3 cost optimization strategies
- [x] Practice: Create buckets with lifecycle policies
- [x] Practice Questions: 25 questions on S3

### Day 4 - Monday, Nov 24 🔄 IN PROGRESS (TODAY)
**Topic: S3 Security & Replication**
- [ ] S3 encryption (SSE-S3, SSE-KMS, SSE-C, client-side)
- [ ] Bucket policies, ACLs, access points
- [ ] S3 replication (CRR, SRR), versioning
- [ ] S3 Object Lock, MFA Delete
- [ ] Practice: Configure encryption and replication
- [ ] Practice Questions: 25 questions on S3 security

### Day 5 - Tuesday, Nov 25
**Topic: VPC Fundamentals**
- [ ] VPC components: subnets, route tables, internet gateway, NAT gateway
- [ ] CIDR blocks and subnet design
- [ ] Security Groups vs NACLs (stateful vs stateless)
- [ ] Practice: Build a VPC from scratch with public/private subnets
- [ ] Practice Questions: 25 questions on VPC basics

### Day 6 - Wednesday, Nov 26
**Topic: Advanced VPC & Hybrid Connectivity**
- [ ] VPC endpoints (Gateway vs Interface)
- [ ] VPC peering, Transit Gateway
- [ ] Direct Connect, Site-to-Site VPN, Client VPN
- [ ] AWS PrivateLink
- [ ] Practice: Set up VPC endpoints and peering
- [ ] Practice Questions: 25 questions on advanced networking

### Day 7 - Thursday, Nov 27 (Thanksgiving)
**Topic: Week 1 Review & Assessment**
- [ ] Review all Week 1 notes and flagged topics
- [ ] Full practice test: 75 questions (90 minutes)
- [ ] Analyze mistakes and create a list of weak areas
- [ ] Update study focus based on results

---

## Week 2: Databases, Serverless & Security (Nov 28 - Dec 4)

### Day 8 - Friday, Nov 28
**Topic: RDS & Aurora**
- [ ] RDS engines (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB)
- [ ] Multi-AZ vs Read Replicas (differences and use cases)
- [ ] Aurora architecture, Aurora Serverless, Aurora Global Database
- [ ] RDS Proxy, automated backups, snapshots
- [ ] Practice: Create RDS with Multi-AZ and Read Replicas
- [ ] Practice Questions: 25 questions on RDS

### Day 9 - Saturday, Nov 29
**Topic: DynamoDB & Other Databases**
- [ ] DynamoDB core concepts (partition keys, sort keys, GSI, LSI)
- [ ] DynamoDB capacity modes (provisioned vs on-demand)
- [ ] DynamoDB Streams, DAX, Global Tables
- [ ] ElastiCache (Redis vs Memcached), Redshift, Neptune, DocumentDB
- [ ] Practice: Create DynamoDB table with GSI
- [ ] Practice Questions: 25 questions on NoSQL & caching

### Day 10 - Sunday, Nov 30
**Topic: Lambda & Serverless Compute**
- [ ] Lambda fundamentals, execution models, limits
- [ ] Lambda pricing, cold starts, optimization
- [ ] Lambda integrations (S3, DynamoDB, API Gateway, EventBridge)
- [ ] Lambda@Edge, Step Functions
- [ ] Practice: Create Lambda functions with triggers
- [ ] Practice Questions: 25 questions on serverless

### Day 11 - Monday, Dec 1
**Topic: Application Integration Services**
- [ ] SQS (Standard vs FIFO), visibility timeout, dead-letter queues
- [ ] SNS (topics, subscriptions, fanout patterns)
- [ ] EventBridge (formerly CloudWatch Events)
- [ ] Kinesis (Data Streams, Firehose, Data Analytics)
- [ ] Practice: Build SQS-SNS integration, set up EventBridge rules
- [ ] Practice Questions: 25 questions on integration

### Day 12 - Tuesday, Dec 2
**Topic: IAM & Access Management**
- [ ] IAM users, groups, roles, policies
- [ ] Policy evaluation logic, permission boundaries
- [ ] Cross-account access (AssumeRole)
- [ ] AWS Organizations, SCPs, Control Tower
- [ ] Practice: Create complex IAM policies and cross-account roles
- [ ] Practice Questions: 25 questions on IAM

### Day 13 - Wednesday, Dec 3
**Topic: Security Services**
- [ ] KMS (CMK, data keys, grants, key policies)
- [ ] Secrets Manager vs Systems Manager Parameter Store
- [ ] AWS Certificate Manager, WAF, Shield, GuardDuty
- [ ] Security Hub, Inspector, Macie
- [ ] Practice: Encrypt resources with KMS, store secrets
- [ ] Practice Questions: 25 questions on security

### Day 14 - Thursday, Dec 4
**Topic: Week 2 Review & Assessment**
- [ ] Review all Week 2 notes and flagged topics
- [ ] Full practice test: 100 questions (130 minutes)
- [ ] Deep dive into mistakes (understand WHY each answer is correct)
- [ ] Update weak areas list

---

## Week 3: Integration, Migration & Architecture (Dec 5-11)

### Day 15 - Friday, Dec 5
**Topic: Monitoring & Logging**
- [ ] CloudWatch metrics, alarms, dashboards
- [ ] CloudWatch Logs, Log Insights, Log subscriptions
- [ ] CloudTrail (management vs data events)
- [ ] AWS Config (rules, compliance tracking)
- [ ] Practice: Set up CloudWatch alarms and Config rules
- [ ] Practice Questions: 25 questions on monitoring

### Day 16 - Saturday, Dec 6
**Topic: Cost Optimization**
- [ ] Cost Explorer, Budgets, Cost Allocation Tags
- [ ] Trusted Advisor recommendations
- [ ] Right-sizing, Reserved Instances, Savings Plans
- [ ] S3 Intelligent-Tiering, EBS snapshot lifecycle
- [ ] Practice: Analyze costs in Cost Explorer
- [ ] Practice Questions: 20 questions on cost optimization

### Day 17 - Sunday, Dec 7
**Topic: Migration Services**
- [ ] AWS Migration Hub, Application Discovery Service
- [ ] Database Migration Service (DMS), Schema Conversion Tool
- [ ] Server Migration Service, CloudEndure
- [ ] Transfer Family (SFTP, FTPS, FTP)
- [ ] Practice Questions: 25 questions on migration

### Day 18 - Monday, Dec 8
**Topic: Hybrid & Storage Gateway**
- [ ] AWS Storage Gateway (File, Volume, Tape Gateway)
- [ ] DataSync, Snow Family (Snowcone, Snowball, Snowmobile)
- [ ] FSx (Windows File Server, Lustre, NetApp ONTAP, OpenZFS)
- [ ] EFS vs FSx comparison
- [ ] Practice Questions: 25 questions on hybrid storage

### Day 19 - Tuesday, Dec 9
**Topic: Well-Architected Framework - Part 1**
- [ ] Operational Excellence pillar
- [ ] Security pillar
- [ ] Reliability pillar
- [ ] Read: AWS Well-Architected Framework whitepaper (sections 1-3)
- [ ] Practice Questions: 20 questions on architecture principles

### Day 20 - Wednesday, Dec 10
**Topic: Well-Architected Framework - Part 2**
- [ ] Performance Efficiency pillar
- [ ] Cost Optimization pillar
- [ ] Sustainability pillar
- [ ] Disaster Recovery strategies (Backup/Restore, Pilot Light, Warm Standby, Multi-Site)
- [ ] RPO vs RTO concepts
- [ ] Practice Questions: 20 questions on DR and optimization

### Day 21 - Thursday, Dec 11
**Topic: Full Practice Exam #1**
- [ ] Take AWS Skill Builder Official Practice Exam (20 questions, 40 minutes)
- [ ] Then take AWS Enhanced Practice Exam (65 questions, 130 minutes, timed)
- [ ] Simulate real exam conditions (no distractions, timed)
- [ ] Score and review EVERY question (even correct ones)
- [ ] Document all weak topics for final review
- [ ] Target score: 75%+ (Official AWS practice exams are harder, 70%+ is good)

---

## Week 4: Final Review & Exam Prep (Dec 12-16)

### Day 22 - Friday, Dec 12
**Topic: Weak Areas Deep Dive - Part 1**
- [ ] Review all mistakes from practice exams
- [ ] Focus on your top 5 weakest topics
- [ ] Read AWS FAQs for those services
- [ ] Hands-on practice for weak services
- [ ] Practice Questions: 30 questions on weak areas

### Day 23 - Saturday, Dec 13
**Topic: Weak Areas Deep Dive - Part 2**
- [ ] Continue reviewing flagged topics
- [ ] Review scenario-based questions (multi-service solutions)
- [ ] Study common exam patterns and distractors
- [ ] Practice Questions: 30 questions on weak areas

### Day 24 - Sunday, Dec 14
**Topic: Full Practice Exam #2**
- [ ] Take full 65-question practice exam from Tutorials Dojo or Skill Builder (130 minutes, timed)
- [ ] Simulate real exam conditions
- [ ] Score and review all questions
- [ ] Target score: 80%+ (52/65 correct)
- [ ] Note any remaining gaps

### Day 25 - Monday, Dec 15
**Topic: Full Practice Exam #3 & Final Review**
- [ ] Take final 65-question practice exam from alternate source (130 minutes)
- [ ] Score and quick review of mistakes
- [ ] Target score: 85%+ (55/65 correct)
- [ ] Review Skill Builder exam readiness videos
- [ ] Review cheat sheets for all major services
- [ ] Read exam tips and strategies

### Day 26 - Tuesday, Dec 16
**Topic: Light Review & Mental Prep**
- [ ] NO heavy studying - avoid burnout
- [ ] Skim through your notes and cheat sheets (30 min)
- [ ] Review common exam traps and keywords
- [ ] Prepare exam day logistics (ID, confirmation, location)
- [ ] Get good sleep tonight
- [ ] Relax and trust your preparation

---

## Day 27 - Wednesday, Dec 17
### EXAM DAY
- [ ] Arrive 30 minutes early
- [ ] Bring two forms of ID
- [ ] Stay calm and read questions carefully
- [ ] Flag difficult questions and come back to them
- [ ] Trust your first instinct on unclear questions
- [ ] YOU'VE GOT THIS!

---

## Study Resources

### Primary Resources (AWS Skill Builder)
- **Course**: "Exam Prep Standard Course: AWS Certified Solutions Architect - Associate" (10 hours)
- **Course**: "AWS Technical Essentials" (refresh if needed)
- **Practice Exams**: Official AWS Practice Exam (20 questions) + Enhanced Practice Exam (65 questions)
- **Labs**: AWS Skill Builder hands-on labs for services you're less familiar with
- **Exam Prep**: Official AWS Exam Prep question sets by domain

### Supplementary Resources
- **Practice Exams**: Tutorials Dojo (Jon Bonso) - 6 practice tests (optional, for extra practice)
- **Cheat Sheets**: Tutorials Dojo or Digital Cloud Training
- **AWS Documentation**: Service FAQs (S3, EC2, RDS, VPC, Route 53)
- **Hands-On**: AWS Free Tier account for practice

### Key AWS Skill Builder Assets
- **Domain-specific practice**: Use Skill Builder's domain-focused question sets
- **Video courses**: Watch 1.5x-2x speed given your advanced experience
- **Labs**: Focus on services you haven't used in production

### Secondary Resources
- **Whitepapers** (available in Skill Builder):
  - AWS Well-Architected Framework
  - Architecting for the Cloud: AWS Best Practices
  - AWS Security Best Practices
- **AWS Service FAQs**: Essential reading for high-weight services

### Exam Tips
1. **Read questions carefully** - look for keywords like "MOST cost-effective", "LEAST operational overhead", "MOST secure"
2. **Eliminate wrong answers** first, then choose from remaining options
3. **Time management** - 2 minutes per question average, flag and move on if stuck
4. **Multi-AZ vs Read Replicas** - know the differences cold
5. **S3 storage classes** - memorize use cases and retrieval times
6. **Security** - always choose most secure option if cost isn't mentioned
7. **Cost optimization** - Reserved Instances/Savings Plans for predictable, Spot for fault-tolerant workloads

---

## Progress Tracking

### Week 1 Progress: 3/7 days completed (Days 1-3 ✅, Day 4 in progress 🔄)
### Week 2 Progress: ___/7 days completed
### Week 3 Progress: ___/7 days completed
### Week 4 Progress: ___/5 days completed

### Practice Exam Scores
- Comprehensive Quiz (Nov 24): 13/15 (87%) ✅
- Day 7: ___/75 (___%)
- Day 14: ___/100 (___%)
- Day 21: ___/65 (___%)
- Day 24: ___/65 (___%)
- Day 25: ___/65 (___%)

**Passing Score: 720/1000 (approximately 72%)**

### Study Materials Created
- ✅ Day-2-Catchup-Auto-Scaling-Load-Balancing.md
- ✅ Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.md
- ✅ Weak-Areas-Cheat-Sheet.md
- ✅ Day-2-Quiz-Auto-Scaling-Load-Balancing.md
- ✅ Days-1-3-Comprehensive-Quiz.md
- ✅ Advanced-Practice-Scenarios-Hard-Mode.md (prepared for later)

---

## Notes & Weak Areas

Use this section to track topics that need extra attention:

**Services to Review:**
- S3 Storage Classes: Retrieval time requirements (Standard-IA vs Glacier for "within minutes")
- S3 Encryption options (SSE-S3, SSE-KMS, SSE-C) - Day 4 focus
- S3 Replication (CRR, SRR) - Day 4 focus

**Common Mistakes (Fixed ✅):**
- ✅ Load Balancer selection (ALB vs NLB vs GLB) - FIXED (100% on retake)
- ✅ Static IP requirement → NLB (not ALB, not GLB) - FIXED
- ✅ Spot Instance diversification → Multiple types + Multiple AZs - FIXED
- ⚠️ S3 retrieval times: "Within minutes" = Standard-IA (not Glacier Expedited) - NEEDS MORE PRACTICE

**Key Concepts to Remember:**
- **Load Balancers:**
  - GLB = Third-party appliances ONLY (firewalls, IDS/IPS)
  - NLB = Static IP, UDP, TCP, extreme performance
  - ALB = HTTP/HTTPS, path routing, Lambda targets, Cognito auth

- **S3 Retrieval Times:**
  - Standard/Standard-IA: Milliseconds
  - Glacier Flexible (Expedited): 1-5 minutes (costs extra)
  - Glacier Flexible (Standard): 3-5 hours
  - Glacier Deep Archive: 12-48 hours

- **Auto Scaling Policies:**
  - Target Tracking = least operational overhead
  - Scheduled = predictable time-based patterns
  - Step = multiple thresholds with different scaling amounts

---

Good luck with your preparation! Stay consistent, practice daily, and you'll be ready by December 17th.
