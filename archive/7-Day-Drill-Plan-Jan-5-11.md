# 7-Day Weakness Elimination Drill Plan

**Period:** January 5-11, 2026 (Week 1 of study period)
**Purpose:** Address the 5 critical/high-priority weaknesses from baseline assessment
**Baseline Score:** 15/20 (75%)
**Target After 7 Days:** 18/20 (90%) on retake

---

## 🎯 Week 1 Focus: Targeted Weakness Elimination

Based on your January 4 baseline assessment, you have **2 critical** and **3 high-priority** weaknesses that need immediate attention before proceeding with the full study schedule.

**Strategy:** Fix these gaps NOW rather than compounding them with new material.

---

## Day 1 - Sunday, January 5 (Lambda Service Limits)

### ⏰ Time: 1.5 hours

### 🎯 Objective
Memorize Lambda hard limits and recognize when Lambda is NOT the right choice.

### 📚 Study Tasks

**30 minutes - Lambda Limits Reference**
- [ ] Read Quick-Reference-Compute.md - Lambda section
- [ ] Create a flashcard for each Lambda limit:
  - Timeout: 15 minutes MAX
  - Memory: 10 GB max
  - Concurrent executions: 1,000 (soft limit)
  - Deployment package: 50 MB zipped, 250 MB unzipped
  - /tmp storage: 512 MB
  - Payload: 6 MB sync, 256 KB async

**30 minutes - Lambda FAQ Deep Dive**
- [ ] Read AWS Lambda FAQ: https://aws.amazon.com/lambda/faqs/
  - Focus on "Limits" section
  - Focus on "When NOT to use Lambda" patterns
- [ ] Write down 3 scenarios where Lambda is unsuitable

**30 minutes - Practice Quiz**
- [ ] Take 10-question quiz on Lambda limits and alternatives
- [ ] Target: 9/10 (90%)
- [ ] Topics: Lambda vs ECS Fargate vs Batch vs EC2

### ✅ Success Criteria
- Can recite all Lambda limits from memory
- Instantly recognize "task > 15 min" = Lambda is OUT

---

## Day 2 - Monday, January 6 (IAM Cross-Account Access)

### ⏰ Time: 2 hours

### 🎯 Objective
Master when to use IAM roles vs pre-signed URLs vs resource policies for cross-account access.

### 📚 Study Tasks

**45 minutes - IAM Cross-Account Patterns**
- [ ] Read Quick-Reference-Security-IAM.md - Cross-Account Access section
- [ ] Read AWS IAM FAQ on cross-account access
- [ ] Draw the decision tree (from Weakness-Tracker.md) 3 times on paper

**45 minutes - Scenario Practice**
- [ ] Create 5 scenarios and decide which method to use:
  1. Third-party vendor (has AWS account) needs S3 access for 2 hours
  2. Share specific S3 objects with customer (no AWS account)
  3. Lambda in Account A needs to read DynamoDB in Account B
  4. Vendor needs to browse/discover S3 objects
  5. CloudFront needs to access private S3 bucket
- [ ] Write out the solution for each

**30 minutes - Practice Quiz**
- [ ] Take 10-question quiz on IAM cross-account access
- [ ] Target: 9/10 (90%)
- [ ] Focus: IAM roles, AssumeRole, pre-signed URLs, resource policies

### ✅ Success Criteria
- Can explain the 3 methods and when to use each
- Recognize "vendor has AWS account" + "temporary access" = IAM role

---

## Day 3 - Tuesday, January 7 (Elastic Beanstalk & PaaS Keywords)

### ⏰ Time: 1.5 hours

### 🎯 Objective
Recognize exam keywords that signal "managed service" / "PaaS" answers.

### 📚 Study Tasks

**30 minutes - PaaS Services Review**
- [ ] Read Quick-Reference-Compute.md - Elastic Beanstalk section
- [ ] Review Exam-Strategy-Tips.md - "LEAST operational overhead" patterns
- [ ] Create keyword flashcards:
  - "limited expertise" → ?
  - "minimize operational overhead" → ?
  - "fastest deployment" → ?
  - "no infrastructure management" → ?

**45 minutes - Service Comparison**
- [ ] Create comparison table:
  - Elastic Beanstalk vs ECS vs Lambda vs EC2
  - When to use each
  - Operational overhead level (1-5 scale)
- [ ] Read Elastic Beanstalk FAQ: https://aws.amazon.com/elasticbeanstalk/faqs/

**15 minutes - Practice Quiz**
- [ ] Take 10-question quiz on compute service selection
- [ ] Target: 9/10 (90%)
- [ ] Focus: Recognizing keywords that signal managed services

### ✅ Success Criteria
- Instantly recognize "limited expertise" → Elastic Beanstalk (for web apps)
- Can rank services by operational overhead: Beanstalk < ECS Fargate < ECS on EC2 < Manual EC2

---

## Day 4 - Wednesday, January 8 (RDS Multi-AZ vs Multi-Region)

### ⏰ Time: 1.5 hours

### 🎯 Objective
Understand what RDS HA/DR options actually exist and when to use each.

### 📚 Study Tasks

**45 minutes - RDS HA/DR Deep Dive**
- [ ] Read Quick-Reference-Databases.md - RDS section
- [ ] Read RDS FAQ on Multi-AZ: https://aws.amazon.com/rds/faqs/
- [ ] Create comparison table:
  - RDS Multi-AZ vs Read Replicas vs Aurora Global Database
  - Failover time for each
  - Replication type (sync vs async)
  - When to use each

**30 minutes - Common Traps**
- [ ] Memorize: "RDS Multi-Region Deployment" DOES NOT EXIST
- [ ] Learn the correct terminology:
  - Multi-AZ = same region HA
  - Read Replicas = can be cross-region for DR (manual failover)
  - Aurora Global Database = true multi-region with fast replication
- [ ] Write down 3 scenarios for each option

**15 minutes - Practice Quiz**
- [ ] Take 10-question quiz on RDS HA/DR
- [ ] Target: 9/10 (90%)
- [ ] Focus: Multi-AZ, Read Replicas, RTO/RPO requirements

### ✅ Success Criteria
- Never confuse Multi-AZ with Multi-Region again
- Know failover times: Multi-AZ (60-120 sec), Read Replica (manual), Aurora Global (manual)

---

## Day 5 - Thursday, January 9 (RDS Read Replica Routing)

### ⏰ Time: 1 hour

### 🎯 Objective
Understand how to route traffic to read replicas considering application constraints.

### 📚 Study Tasks

**30 minutes - Read Replica Patterns**
- [ ] Review Weakness-Tracker.md - RDS Read Replica Routing section
- [ ] Understand the difference:
  - Direct endpoint provision (analytics team gets replica URL)
  - Application-level routing (app decides which endpoint)
  - Aurora reader endpoint (automatic load balancing)
- [ ] Create decision tree for read replica routing

**15 minutes - Constraint Recognition**
- [ ] Practice identifying constraints in questions:
  - "Cannot modify application code" = ?
  - "Least operational overhead" = ?
  - "Automatic load balancing" = ?
- [ ] Map constraints to solutions

**15 minutes - Practice Scenarios**
- [ ] Work through 5 scenarios:
  1. Analytics team needs read-only access, can't modify app code
  2. Need to load balance reads across 3 replicas automatically
  3. OLTP writes + analytical reads, can modify app code
  4. Separate reporting application from main app
  5. Need read scaling with automatic failover

### ✅ Success Criteria
- Recognize "cannot modify code" → provide separate endpoint
- Recognize "automatic" → Aurora reader endpoint

---

## Day 6 - Friday, January 10 (Comprehensive Review)

### ⏰ Time: 2 hours

### 🎯 Objective
Review all 5 weaknesses and ensure they're now strengths.

### 📚 Study Tasks

**60 minutes - Flash Review**
- [ ] Review all decision trees from Weakness-Tracker.md (15 min each):
  - Lambda limits and alternatives
  - IAM cross-account access patterns
  - PaaS keyword recognition
  - RDS Multi-AZ vs Read Replicas
  - Read replica routing strategies

**60 minutes - Mixed Practice Quiz**
- [ ] Take 20-question quiz covering ALL 5 weak areas:
  - 4 questions on Lambda limits
  - 4 questions on IAM cross-account
  - 4 questions on Elastic Beanstalk/PaaS
  - 4 questions on RDS Multi-AZ
  - 4 questions on Read Replica routing
- [ ] Target: 18/20 (90%)
- [ ] Review ANY questions you miss immediately

### ✅ Success Criteria
- Score 90%+ on mixed weakness quiz
- Can explain each wrong answer and why correct answer is right

---

## Day 7 - Saturday, January 11 (Full Baseline Retake)

### ⏰ Time: 2 hours

### 🎯 Objective
Validate improvement by retaking the baseline assessment.

### 📚 Study Tasks

**10 minutes - Mental Prep**
- [ ] Quick skim of all decision trees (don't study, just refresh)
- [ ] Review your "always forget" notes
- [ ] Get in the zone

**60 minutes - Baseline Assessment Retake**
- [ ] Retake the SAME 20 questions from January 4 baseline
- [ ] Timed: 40 minutes (2 min per question)
- [ ] **Target: 18-20/20 (90-100%)**
- [ ] Focus especially on the 5 questions you missed before

**50 minutes - Deep Review**
- [ ] Review all answers (right and wrong)
- [ ] Update Weakness-Tracker.md:
  - Mark resolved weaknesses (90%+ accuracy)
  - Note any remaining weak spots
  - Celebrate your progress!

### ✅ Success Criteria
- **Primary Goal:** Score 90%+ (18/20 or better)
- **Stretch Goal:** Get all 5 previously-missed questions correct
- **Validation:** No new weaknesses introduced

---

## Week 1 Success Metrics

### Baseline to Target Progression

| Metric | Baseline (Jan 4) | Target (Jan 11) | Improvement |
|--------|------------------|-----------------|-------------|
| **Overall Score** | 15/20 (75%) | 18/20 (90%) | +15% |
| **Lambda Limits** | 50% (1/2) | 100% (2/2) | +50% |
| **IAM Cross-Account** | 50% (1/2) | 100% (2/2) | +50% |
| **Elastic Beanstalk** | 67% (2/3) | 100% (3/3) | +33% |
| **RDS Multi-AZ** | 67% (2/3) | 100% (3/3) | +33% |
| **Read Replica Routing** | 67% (2/3) | 100% (3/3) | +33% |

### What Success Looks Like

**If you hit 90%+ on the retake:**
- ✅ Week 1 weaknesses eliminated
- ✅ Ready to proceed to Week 2 of study schedule
- ✅ Solid foundation for building on

**If you score 80-89%:**
- ⚠️ Good progress, but 1-2 weaknesses remain
- 🔄 Spend an extra day drilling the remaining gaps
- 📅 Delay Week 2 start by 1 day

**If you score <80%:**
- 🚨 Weaknesses not fully resolved
- 🔄 Extend Week 1 by 2-3 days
- 📊 Identify which of the 5 topics is still problematic
- 💪 Drill that specific topic until 90%+

---

## Daily Time Commitment

**Weekdays (Mon-Fri):** 1-2 hours
**Weekends (Sat-Sun):** 1.5-2 hours
**Total for Week 1:** ~11 hours

This is sustainable while working full-time and ensures you build a strong foundation.

---

## Study Tips for This Week

### Active Learning
- ✅ **DO:** Create flashcards, draw decision trees, write scenarios
- ✅ **DO:** Teach concepts out loud (if you can explain it, you know it)
- ✅ **DO:** Take practice quizzes daily
- ❌ **DON'T:** Just read passively
- ❌ **DON'T:** Move on if you score <90% on daily quiz

### Memorization Techniques
- **Spaced repetition:** Review Lambda limits every day this week
- **Mnemonics:** Create memorable phrases for hard-to-remember facts
- **Visual aids:** Draw the IAM cross-account decision tree from memory 3x
- **Real scenarios:** Imagine yourself as the solutions architect making the decision

### Tracking Progress
- Update Weakness-Tracker.md after every quiz
- Note patterns in mistakes (not just topics)
- Celebrate small wins (90% on a daily quiz is a win!)

---

## Resources for This Week

### AWS Documentation
- Lambda FAQ: https://aws.amazon.com/lambda/faqs/
- IAM FAQ: https://aws.amazon.com/iam/faqs/
- RDS FAQ: https://aws.amazon.com/rds/faqs/
- Elastic Beanstalk FAQ: https://aws.amazon.com/elasticbeanstalk/faqs/

### Repository Files
- Quick-Reference-Compute.md (Lambda, Beanstalk)
- Quick-Reference-Security-IAM.md (Cross-account access)
- Quick-Reference-Databases.md (RDS Multi-AZ, Read Replicas)
- Weakness-Tracker.md (Your decision trees and patterns)
- Exam-Strategy-Tips.md (Keyword patterns)

### Practice Quizzes
- Use the aws-quiz-master agent for daily quizzes
- Request specific topics (e.g., "10 questions on Lambda limits")
- Always review wrong answers immediately

---

## Motivation

You went from **complete beginner to mastering 18 topics in December**. You proved you can do this.

**December victories:**
- DynamoDB Query vs Scan: 0% → 100% in 10 days
- S3 Storage Classes: 40% → 100% in 12 days
- VPC NACLs: 0% → 100% in 3 days

**This week's challenge:**
- 5 weaknesses → 0 weaknesses in 7 days

You've done harder things. You can do this. Let's make Week 1 your foundation for success.

---

**Last Updated:** January 4, 2026
**Next Review:** January 11, 2026 (after baseline retake)
