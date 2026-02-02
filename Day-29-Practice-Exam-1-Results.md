# Day 29 (Feb 1): First Full 65-Question Practice Exam - 55.4% FAIL

**Date:** February 1, 2026, 6:14 PM CST
**Exam:** SAA-C03 Practice Exam 1 (Full 65 questions)
**Score:** 36/65 (55.4%) ❌ **CATASTROPHIC FAILURE**
**Passing Score:** 47/65 (72%)
**Gap:** -11 questions (17 percentage points)
**Days Until Exam:** 10 days (Feb 11, 2026 at 5:15 PM EST)

---

## 🚨 EXAM STATUS: HIGH RISK OF FAILURE

**Current Pass Probability:** ~5% (would need exceptional luck)

**Result:** If the exam were today, you would FAIL by a significant margin.

---

## 📊 Performance by Domain

### Domain 1: Design Secure Architectures (30% of exam)
- **Score:** 4/5 (80%) ✅ **STRENGTH**
- **Questions:** 20, 35, 46, 53, 60
- **Strengths:** Cognito Identity Pools, KMS customer managed keys, S3 Object Lock Compliance, multi-tenant S3 architecture
- **Missed:** Q35 (Oracle → PostgreSQL requires refactoring, can't swap engines)

### Domain 2: Design Resilient Architectures (26% of exam)
- **Score:** 5/5 (100%) ✅✅ **MASTERY**
- **Questions:** 5, 45, 52, 54, 61
- **Strengths:** DR strategies (Pilot Light), Auto Scaling scheduled scaling, App Mesh circuit breakers, CloudFront CDN, sticky sessions
- **This is your strongest domain - perfect score!**

### Domain 3: Design High-Performing Architectures (24% of exam)
- **Score:** 6/10 (60%) ⚠️ **BELOW TARGET**
- **Questions:** 4, 40, 43, 44, 47, 50, 51, 55, 56, 63
- **Strengths:** AWS Batch Spot, MediaConvert, Kinesis Flink, CloudFront CDN
- **Critical Failures:**
  - Q4: Didn't know io2 Block Express for 500K IOPS (chose Aurora)
  - Q44: **SAME MISTAKE** - io2 Block Express for 500K IOPS (chose Aurora again!)
  - Q47: DynamoDB On-Demand for 100x instant spike (chose provisioned auto-scaling)
  - Q50: DAX caching vs RCU calculation (chose over-provisioned RCUs)
  - Q51: FSx for Lustre sub-millisecond latency (chose EFS Max I/O)
  - Q56: Elastic Beanstalk for "least overhead" (chose ECS Fargate over-engineering)

### Domain 4: Design Cost-Optimized Architectures (20% of exam)
- **Score:** 1/5 (20%) ❌❌ **CATASTROPHIC DISASTER**
- **Questions:** 41, 48, 59, 62, 65
- **Only got Q52 correct (Pilot Light DR, also counted in Domain 2)**
- **Epic Failures:**
  - Q41: S3 lifecycle tiers - confused Glacier Instant (expensive) vs Glacier Flexible (cheap)
  - Q48: Direct Connect vs Snowball for "no public internet" requirement
  - Q59: S3 lifecycle minimum storage durations - can't skip tiers directly to Deep Archive
  - Q62: AWS Glue serverless for sporadic Spark jobs (chose always-on EMR cluster)
  - Q65: Rightsizing + RI combo (chose Instance Scheduler, missed rightsizing)

### Domain 5: Multi-Service Integration & Patterns
- **Score:** 2/5 (40%) ⚠️ **WEAKNESS**
- **Questions:** 42, 49, 57, 58, 64
- **Strengths:** App Mesh retry logic, S3 Object Lock Compliance
- **Failures:**
  - Q42: Oracle compatibility - tried to swap to PostgreSQL (requires refactoring)
  - Q57: Lambda + Aurora Serverless v2 for flash sales (chose scheduled scaling)
  - Q58: DynamoDB single-table multi-tenant (chose over-engineered composite key sharding)

---

## 🔥 Top 10 Critical Weaknesses

### 1. S3 Lifecycle Minimum Storage Durations (URGENT)
- **Failed:** Q41, Q59
- **Gap:** Don't understand that you can't skip storage tiers
- **Rule:** Standard-IA (30d min) → Glacier Flexible (90d min) → Deep Archive (180d min)
- **Tried to:** Jump straight to Glacier Deep Archive (violates minimums)
- **Fix:** Memorize tier progression rules, create decision tree

### 2. io2 Block Express for Extreme IOPS (CRITICAL - FAILED TWICE!)
- **Failed:** Q4, Q44 (same mistake repeated!)
- **Gap:** Don't know io2 Block Express exists or its 256K IOPS capability
- **Pattern:** Default to Aurora/RDS when extreme IOPS (>64K) mentioned
- **Reality:** io2 Block Express up to 256K IOPS, io2 maxes at 64K
- **Fix:** Memorize IOPS hierarchy: gp3 (16K) → io2 (64K) → io2 Block Express (256K)

### 3. Cost Optimization Hierarchy: Rightsize > Commit > Schedule
- **Failed:** Q65
- **Gap:** Chose Instance Scheduler over rightsizing + Reserved Instance
- **Rule:** ALWAYS rightsize first (biggest savings), THEN commit (RI/Savings Plan), THEN schedule
- **Mistake:** Focused on scheduling (save 50%) instead of rightsize + RI (save 70%)
- **Fix:** Create cost optimization decision tree

### 4. Serverless for Sporadic Workloads
- **Failed:** Q62
- **Gap:** Chose always-on EMR cluster for 20% utilization Spark jobs
- **Pattern:** Don't recognize AWS Glue as serverless Spark option
- **Reality:** Glue = pay-per-job, EMR 24/7 = wasted idle time
- **Fix:** Study serverless compute patterns (Glue, Lambda, Fargate)

### 5. DynamoDB On-Demand vs Provisioned Auto-Scaling
- **Failed:** Q47
- **Gap:** Chose provisioned auto-scaling for "100x spike in seconds"
- **Reality:** Auto-scaling is reactive (takes minutes), On-Demand is instant
- **Pattern:** "Within seconds" = serverless, "gradual ramp" = auto-scaling
- **Fix:** Create DynamoDB capacity mode decision tree

### 6. Glacier Instant vs Glacier Flexible Retrieval
- **Failed:** Q41
- **Gap:** Thought Glacier Instant was cheaper than Standard-IA
- **Reality:** Glacier Instant is MORE expensive than Glacier Flexible
- **Cost Hierarchy:** Standard > Standard-IA > Glacier Instant > Glacier Flexible > Deep Archive
- **Fix:** Memorize cost hierarchy table

### 7. FSx for Lustre vs EFS Latency
- **Failed:** Q51
- **Gap:** Chose EFS Max I/O for "sub-millisecond latency" requirement
- **Reality:** EFS = single-digit ms, FSx Lustre = sub-millisecond (hundreds of μs)
- **Pattern:** "Sub-millisecond" + "shared file system" = FSx Lustre
- **Fix:** Memorize storage latency hierarchy

### 8. Database Engine Compatibility
- **Failed:** Q42
- **Gap:** Tried to migrate Oracle → Aurora PostgreSQL for "least overhead"
- **Reality:** Stored procedures aren't compatible, requires massive refactoring
- **Pattern:** "Migrate" ≠ "Modernize" - keep database engine unless told to refactor
- **Fix:** Study database migration patterns

### 9. Over-Engineering vs Managed Service Simplicity
- **Failed:** Q56
- **Gap:** Chose ECS Fargate containerization for WordPress "least overhead"
- **Reality:** Elastic Beanstalk = upload ZIP, done (Fargate = Dockerfile, task definitions, networking)
- **Pattern:** "LEAST operational overhead" = simplest managed service (Beanstalk, not ECS)
- **Fix:** Create "operational overhead" ranking table

### 10. DynamoDB Single-Table Multi-Tenant Pattern
- **Failed:** Q58
- **Gap:** Chose composite partition key with manual sharding
- **Reality:** Simple customer ID partition key is sufficient, DynamoDB auto-partitions
- **Pattern:** Don't over-engineer with manual sharding
- **Fix:** Study DynamoDB single-table design patterns

---

## 📉 Score Trajectory Analysis

**Questions 40-50:** 5/11 (45%) - Early struggles with cost and performance
**Questions 51-65:** 4/15 (27%) - **COLLAPSE** in final stretch

**Collapse Reasons:**
1. Fatigue affecting decision-making
2. Cost optimization questions clustered at end (weakest domain)
3. Complex multi-service integration questions

**Pattern:** Performance degraded significantly as exam progressed. Need stamina training and focused drilling on weak domains.

---

## 🎯 Emergency Recovery Plan (10 Days)

### Phase 1: URGENT - Cost Optimization (Days 1-3)
**Target:** Raise Domain 4 from 20% → 90%+

**Day 1 (Feb 2):**
- [ ] Study S3 storage classes with minimum durations (MEMORIZE!)
- [ ] Create S3 lifecycle decision tree
- [ ] Study cost optimization hierarchy: Rightsize > Commit > Schedule
- [ ] Drill: 20 cost optimization questions (target: 18/20 = 90%)

**Day 2 (Feb 3):**
- [ ] Study serverless cost models (Lambda, Glue, Fargate On-Demand)
- [ ] Study Reserved Instance vs Savings Plan vs On-Demand
- [ ] Drill: 20 cost optimization questions (target: 19/20 = 95%)

**Day 3 (Feb 4):**
- [ ] Retake all 5 missed cost questions from Practice Exam 1
- [ ] Final cost optimization drill: 20 questions (target: 20/20 = 100%)
- [ ] Create cost optimization flashcards

### Phase 2: Service Limits & Extreme Performance (Days 4-5)
**Target:** Raise Domain 3 from 60% → 85%+

**Day 4 (Feb 5):**
- [ ] Memorize IOPS table: gp3 (16K), io2 (64K), io2 Block Express (256K)
- [ ] Study Aurora IOPS limits (~100K-200K max)
- [ ] Study FSx for Lustre vs EFS latency differences
- [ ] Drill: 20 high-performance questions (target: 17/20 = 85%)

**Day 5 (Feb 6):**
- [ ] Study DynamoDB capacity modes (On-Demand vs Provisioned)
- [ ] Study Aurora Serverless v2 scaling characteristics
- [ ] Study Lambda vs Fargate vs EC2 scaling speeds
- [ ] Drill: 20 high-performance questions (target: 18/20 = 90%)

### Phase 3: Pattern Recognition & Anti-Patterns (Days 6-7)
**Target:** Eliminate over-engineering mistakes

**Day 6 (Feb 7):**
- [ ] Study "LEAST operational overhead" patterns (Beanstalk > ECS > EC2)
- [ ] Study database migration vs modernization patterns
- [ ] Study DynamoDB single-table multi-tenant design
- [ ] Drill: 20 mixed questions (target: 17/20 = 85%)

**Day 7 (Feb 8):**
- [ ] Practice Exam 2 (full 65 questions, target: 50/65 = 77%)
- [ ] Analyze results, identify remaining gaps

### Phase 4: Final Prep & Review (Days 8-10)
**Day 8 (Feb 9):**
- [ ] Review all weaknesses from Practice Exams 1 & 2
- [ ] Drill weak areas until 90%+ achieved
- [ ] Create final cheat sheet (1 page)

**Day 9 (Feb 10):**
- [ ] Light review of flashcards
- [ ] Practice Exam 3 (full 65 questions, target: 52/65 = 80%)
- [ ] Evening: REST (no studying after 6 PM)

**Day 10 (Feb 11 - EXAM DAY):**
- [ ] Morning: Review 1-page cheat sheet only
- [ ] Arrive 30 minutes early
- [ ] **EXAM at 5:15 PM EST**

---

## ⚠️ Exam Day Risk Level

**Current Risk:** 🔴 **CRITICAL - HIGH PROBABILITY OF FAILURE**

**Reasons:**
1. Cost optimization domain at 20% (needs to be 70%+)
2. Repeated mistakes (io2 Block Express twice)
3. Score collapse in final questions (fatigue/complexity)
4. 17-point gap to passing score

**Mitigation Strategy:**
- **URGENT drilling** on cost optimization (Days 1-3)
- **Memorize** service limits and IOPS hierarchy (Days 4-5)
- **Practice exams** to build stamina (Days 7, 9)
- **Rest** before exam day (no cramming night before)

---

## 💪 Strengths to Leverage

1. **Resilient Architectures:** 100% (5/5) - DR strategies, Auto Scaling, circuit breakers
2. **Secure Architectures:** 80% (4/5) - Cognito, KMS, S3 Object Lock, IAM
3. **Time Management:** Completed 65 questions without rushing

**Strategy:** These domains should be "easy points" on exam day. Focus recovery effort on cost optimization (your disaster zone).

---

## 📝 Lessons Learned

1. **Read requirements carefully:** "Sub-millisecond" vs "millisecond" = different services
2. **Identify hard requirements first:** "No public internet" = eliminates options
3. **Don't over-engineer:** "Least overhead" = simplest managed service
4. **Rightsize before committing:** Biggest cost savings from rightsizing
5. **Understand service limits:** io2 Block Express, Aurora IOPS, API Gateway RPS
6. **Serverless for sporadic:** <50% utilization = serverless usually cheaper
7. **Storage tier progression:** Must respect minimum storage durations

---

**Status:** 🚨 **EMERGENCY RECOVERY MODE ACTIVATED**

**Next Action:** Commit results to git, then start Cost Optimization drill (20 questions, target 90%+)

**Mantra for next 10 days:** "Rightsize first. Know your limits. Don't over-engineer."
