# February 14, 2026 PM - Mixed Domain Assessment (48% - CRITICAL FAILURE)

## Mixed Domain Coverage Assessment Results
**Topic:** Networking, Security/IAM, Databases, Monitoring/DR, Compute (domains NOT drilled yet)
**Score:** 9.5/20 (48%) 🔴 **CRITICAL FAILURE** (Target: 16/20 = 80%)
**Status:** 🔴 **MULTIPLE CRITICAL GAPS IN CORE DOMAINS**

**Context:** February 14, 2026, 2:27 PM CST (17 days to exam). After 52 cost optimization questions, assessed unexplored domains. Revealed MASSIVE gaps in Networking (40%), Security (50%), Monitoring (33%). Projected exam score: 64% (FAILING).

---

## Performance Breakdown by Domain

### NETWORKING: 2/5 (40%) 🔴 **WEAKEST DOMAIN**

**Questions Correct (2/5):**
- Q1: NAT Gateway multi-AZ deployment for private subnets ✅
- Q6: AWS Global Accelerator for latency-sensitive gaming ✅

**Questions Incorrect (3/5):**

#### WEAKNESS #44: Gateway vs Interface VPC Endpoints
- Q10: Chose Gateway Endpoint for Secrets Manager ❌
  - **User's answer:** A (Create VPC Gateway Endpoint for Secrets Manager)
  - **Correct answer:** B (Create VPC Interface Endpoint for Secrets Manager)
  - **Misconception:** Thought Gateway Endpoints work for any AWS service
  - **CRITICAL FACT:** Gateway Endpoints ONLY support S3 and DynamoDB (FREE)
  - **Pattern:** All other services require Interface Endpoints (COSTS MONEY)
  - **Exam Impact:** HIGH - Appears 3-5 times on SAA-C03

#### WEAKNESS #45: NAT Gateway Provides Internet Access
- Q14: Chose NAT Gateway for "no internet connectivity" requirement ❌
  - **User's answer:** A (NAT Gateway + Security Groups to block internet)
  - **Correct answer:** B (VPC Interface Endpoints for CloudWatch - truly private)
  - **Misconception:** Thought NAT Gateway + Security Groups = no internet access
  - **CRITICAL FACT:** NAT Gateway routes traffic to PUBLIC INTERNET via IGW
  - **Pattern:** "No internet connectivity" = VPC Interface Endpoints (truly private), NOT NAT Gateway
  - **Exam Impact:** MEDIUM - Appears 2-3 times on SAA-C03

#### WEAKNESS #46: CloudFront vs Global Accelerator
- Q17: Chose Global Accelerator for video streaming ❌
  - **User's answer:** A (AWS Global Accelerator for global video viewers)
  - **Correct answer:** B (CloudFront with ALB origin, caching enabled)
  - **Misconception:** Thought Global Accelerator was for "global" content delivery
  - **CRITICAL FACTS:**
    - CloudFront = CDN, caches content at edge (400+ locations), reduces origin bandwidth
    - Global Accelerator = Layer 4 network accelerator, NO caching, all traffic hits origin
  - **Pattern:** "Video streaming" + "global viewers" + "reduce bandwidth" = CloudFront (NOT Global Accelerator)
  - **Exam Impact:** HIGH - Appears 4-6 times on SAA-C03 (most commonly confused services)

---

### SECURITY/IAM: 2.5/5 (50%) 🔴 **SECOND WEAKEST**

**Questions Correct (2.5/5):**
- Q2: SSE-KMS with customer-managed keys + CloudTrail ✅
- Q5: IAM time-based policies (partial credit - didn't know 12hr role limit) ⚠️

**Questions Incorrect (2.5/5):**

#### WEAKNESS #47: IAM Role Session Duration Limits
- Q5: Thought IAM roles support 7-day sessions ❌
  - **User's answer:** C (IAM role with 7-day maximum session duration)
  - **Correct answer:** D (IAM user with time-based Condition using aws:CurrentTime)
  - **Misconception:** Thought IAM roles can have multi-day session durations
  - **CRITICAL FACT:** IAM role maximum session duration = 12 HOURS (cannot be exceeded)
  - **Pattern:** Temporary access > 12 hours = IAM user with time-based Condition policy
  - **Exam Impact:** MEDIUM - Appears 2-3 times on SAA-C03

#### WEAKNESS #48: GuardDuty vs EventBridge + CloudTrail
- Q7: Chose GuardDuty to detect CloudTrail API calls ❌
  - **User's answer:** D (GuardDuty with S3 Protection for CloudTrail changes)
  - **Correct answer:** B (Organization trail + EventBridge rule for StopLogging API)
  - **Misconception:** Thought GuardDuty detects specific API calls
  - **CRITICAL FACTS:**
    - GuardDuty = Threat detection (unusual behavior, compromised credentials, malware)
    - EventBridge + CloudTrail = Detect SPECIFIC API calls in near real-time
  - **Pattern:** "Detect API calls" (StopLogging, DeleteBucket, etc.) = EventBridge + CloudTrail
  - **Exam Impact:** MEDIUM - Appears 2-4 times on SAA-C03

#### WEAKNESS #49: AWS WAF vs Shield vs GuardDuty
- Q19: Chose Shield for SQL injection/XSS protection ❌
  - **User's answer:** C and D (Shield Advanced + WAF rate-based rules)
  - **Correct answer:** A and D (WAF managed rules for SQL/XSS + WAF rate-based rules)
  - **Misconception:** Thought Shield protects against application-layer exploits
  - **CRITICAL FACTS:**
    - AWS WAF = Web Application Firewall, blocks SQL injection, XSS, bad bots (Layer 7)
    - AWS Shield = DDoS protection, SYN floods, UDP reflection (Layer 3/4)
    - GuardDuty = Threat detection (monitors logs, DOES NOT block traffic)
  - **Pattern:** "SQL injection + XSS" = WAF, "DDoS" = Shield, "Detect threats" = GuardDuty
  - **Exam Impact:** CRITICAL - Appears 5-8 times on SAA-C03 (most commonly confused)

---

### DATABASES: 3/5 (60%) 🟡 **BELOW TARGET**

**Questions Correct (3/5):**
- Q4: RDS Multi-AZ for Oracle with Read Replicas ✅
- Q9: DynamoDB → S3 + Athena for analytics (correct pattern) ✅
- Q15: Multi-tenant isolation (separate databases in same instance) ✅

**Questions Incorrect (2/5):**

#### WEAKNESS #50: SQS FIFO Throughput Limits
- Q3: Chose SQS FIFO for 500K user clickstream ❌
  - **User's answer:** A (SQS FIFO queue → Lambda → DynamoDB)
  - **Correct answer:** B (Kinesis Data Streams → Lambda → DynamoDB)
  - **Misconception:** Thought SQS FIFO could handle high-volume streaming data
  - **CRITICAL FACT:** SQS FIFO limit = 300 TPS (3,000 TPS with batching) - NOT for streaming
  - **Pattern:** "Real-time analytics" + "clickstream" + "ordering per user" = Kinesis
  - **Exam Impact:** HIGH - Appears 3-5 times on SAA-C03

#### WEAKNESS #51: Read Replicas vs ElastiCache Complexity
- Q12: Chose ElastiCache over Read Replicas ❌
  - **User's answer:** C (ElastiCache Redis with 5-sec TTL)
  - **Correct answer:** A (RDS Read Replicas in same region)
  - **Misconception:** Thought ElastiCache was "minimal code changes"
  - **CRITICAL FACTS:**
    - Read Replicas = Change connection string ONLY (1 line of code)
    - ElastiCache = Implement cache-aside pattern, invalidation, miss handling (major code changes)
  - **Pattern:** "Minimize code changes" + "offload reads" = Read Replicas
  - **Exam Impact:** MEDIUM - Appears 2-3 times on SAA-C03

---

### MONITORING/DR: 1/3 (33%) 🔴 **DISASTER**

**Questions Correct (1/3):**
- Q18: Aurora Zero-Downtime Patching ✅

**Questions Incorrect (2/3):**

**WEAKNESS #48 (DUPLICATE):** GuardDuty vs EventBridge + CloudTrail (see Q7 above)

#### WEAKNESS #52: Lambda Timeout Limits
- Q16: Thought Lambda timeout could handle 50-100 GB files ❌
  - **User's answer:** A (Increase Lambda memory to 10GB and timeout to 15 min)
  - **Correct answer:** C (AWS Batch job triggered by EventBridge)
  - **Misconception:** Thought increasing Lambda timeout solves long-running jobs
  - **CRITICAL FACT:** Lambda maximum timeout = 15 MINUTES (cannot be exceeded)
  - **Pattern:** "Process files of any size" + "long-running" = AWS Batch (NOT Lambda)
  - **Exam Impact:** MEDIUM - Appears 2-3 times on SAA-C03

---

### COMPUTE: 2/2 (100%) ✅

**Questions Correct (2/2):**
- Q13: ECS Fargate for serverless containers ✅
- Q20: Cluster placement groups for HPC ✅

---

## ACTIVE WEAKNESSES SUMMARY (Post Feb 14 Assessment)

### CRITICAL PRIORITY (Must Fix This Weekend)

**Weakness #44:** Gateway vs Interface VPC Endpoints
- **Scope:** S3 + DynamoDB = Gateway (FREE), All other services = Interface (COSTS MONEY)
- **Impact:** HIGH - Appears 3-5 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #46:** CloudFront vs Global Accelerator
- **Scope:** CloudFront = CDN with caching, GA = network accelerator (no caching)
- **Impact:** HIGH - Appears 4-6 times on exam (most confused services)
- **Status:** 🔴 ACTIVE

**Weakness #49:** AWS WAF vs Shield vs GuardDuty
- **Scope:** WAF = web exploits (Layer 7), Shield = DDoS (Layer 3/4), GuardDuty = detection only
- **Impact:** CRITICAL - Appears 5-8 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #50:** SQS FIFO Throughput Limits
- **Scope:** FIFO = 300 TPS (3K with batching), Kinesis = high-volume streaming
- **Impact:** HIGH - Appears 3-5 times on exam
- **Status:** 🔴 ACTIVE

### HIGH PRIORITY (Drill Next Week)

**Weakness #45:** NAT Gateway Provides Internet Access
- **Impact:** MEDIUM - Appears 2-3 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #47:** IAM Role Session Duration = 12 Hours Max
- **Impact:** MEDIUM - Appears 2-3 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #48:** GuardDuty vs EventBridge + CloudTrail
- **Impact:** MEDIUM - Appears 2-4 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #51:** Read Replicas vs ElastiCache Complexity
- **Impact:** MEDIUM - Appears 2-3 times on exam
- **Status:** 🔴 ACTIVE

**Weakness #52:** Lambda 15-Min Maximum Timeout
- **Impact:** MEDIUM - Appears 2-3 times on exam
- **Status:** 🔴 ACTIVE

---

## RECOVERY PLAN (Feb 14-20 - URGENT)

**Current Projected Score:** 64% (FAILING - Need 72% to pass)
**Gap:** -8%
**Days to Exam:** 17

### Immediate Actions (Today - Feb 14 PM):

**Afternoon Session (4:00-6:00 PM):**
- 20-question Networking drill (VPC Endpoints, Route 53, CloudFront/GA, Direct Connect)
- Target: 16/20 (80%)
- Focus: Weaknesses #44, #45, #46

### Weekend Drilling (Feb 15-16):

**Saturday AM (Feb 15):**
- 20-question Security/IAM drill (WAF/Shield/GuardDuty, KMS, IAM policies/roles, Secrets Manager)
- Target: 16/20 (80%)
- Focus: Weaknesses #47, #48, #49

**Saturday PM (Feb 15):**
- 20-question Monitoring/DR drill (CloudTrail, EventBridge, Config, DR strategies, CloudWatch)
- Target: 16/20 (80%)
- Focus: Weaknesses #48, #52

**Sunday (Feb 16):**
- 20-question Database drill (RDS, Aurora, DynamoDB, ElastiCache, read scaling patterns)
- Target: 16/20 (80%)
- Focus: Weaknesses #50, #51

### Validation (Monday Feb 17):

- 20-question mixed domain quiz (context switching test)
- Must achieve 16/20 (80%) across all domains
- If passing: proceed to Week 5 practice exams
- If failing: extend drilling 2-3 more days

**Goal:** Raise projected exam score from 64% → 75%+ by end of weekend
