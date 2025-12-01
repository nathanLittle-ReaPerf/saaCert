# 7-Day Emergency Recovery Schedule - Week 1 Foundation Repair

**Start Date:** November 30, 2025 (Today)
**Completion Date:** December 6, 2025
**Exam Date:** December 17, 2025 (17 days away)
**Current Status:** Failed foundation quiz with 25% (5/20)
**Goal:** Achieve 80%+ on all Week 1 topics before advancing to Week 2

---

## 📅 Daily Schedule Overview

| Day | Focus Area | Time Required | Target Score | Status |
|-----|-----------|---------------|--------------|--------|
| **Day 1** (Nov 30) | EC2 Mastery | 2 hours | 8/10 (80%) | ⬜ Not started |
| **Day 2** (Dec 1) | Database Fundamentals | 2 hours | 8/10 (80%) | ⬜ Not started |
| **Day 3** (Dec 2) | VPC & Networking | 2 hours | 8/10 (80%) | ⬜ Not started |
| **Day 4** (Dec 3) | S3 Storage Classes | 1.5 hours | 9/10 (90%) | ⬜ Not started |
| **Day 5** (Dec 4) | Load Balancing & Auto Scaling | 1.5 hours | 8/10 (80%) | ⬜ Not started |
| **Day 6** (Dec 5) | Foundation Quiz Retake | 1 hour | 16/20 (80%) | ⬜ Not started |
| **Day 7** (Dec 6) | Week 1 Comprehensive Quiz | 1.5 hours | 24/30 (80%) | ⬜ Not started |

**Total Time Investment:** ~12 hours over 7 days
**Average:** 1.5-2 hours per day

---

## 📋 Detailed Daily Plans

### Day 1: EC2 Mastery (Saturday, November 30)

**Time:** 2 hours
**Goal:** Master EC2 instance types, placement groups, and Reserved Instances
**Target Score:** 8/10 (80%) on EC2-only quiz

#### Morning Session (1 hour 15 min)

**8:00 - 8:45 AM: Read Quick-Reference-Compute.md**
- [ ] Focus on EC2 instance type families (c, m, r, i, d, t)
- [ ] Understand when to use each family
- [ ] Highlight exam keywords for each type

**8:45 - 9:00 AM: Create Flashcards**
- [ ] Front: "CPU-intensive video encoding" → Back: "c5 (Compute optimized)"
- [ ] Front: "In-memory database Redis" → Back: "r5 (RAM optimized)"
- [ ] Front: "NoSQL database Cassandra" → Back: "i3 (I/O optimized SSD)"
- [ ] Front: "Data warehouse Hadoop" → Back: "d2 (Dense HDD storage)"
- [ ] Front: "General web server" → Back: "m5 (Memory balanced)"
- [ ] Front: "Burstable development" → Back: "t3 (Burstable)"

**9:00 - 9:15 AM: Memorize Placement Groups**
- [ ] Write from memory: Cluster vs Partition vs Spread
- [ ] Cluster = HPC, lowest latency, single AZ, same rack
- [ ] Partition = Hadoop/Kafka, fault isolation, 7 partitions per AZ
- [ ] Spread = Critical instances, max isolation, 7 instances per AZ

#### Afternoon Session (45 min)

**2:00 - 2:15 PM: Write from Memory**
- [ ] When to use each instance family (don't look at notes)
- [ ] When to use each placement group type
- [ ] Compare your answers to Quick-Reference-Compute.md
- [ ] Review anything you got wrong

**2:15 - 2:45 PM: Take EC2-Only Quiz (10 questions)**
- [ ] Target: 8/10 (80%)
- [ ] If you score 8+: ✅ Move to Day 2
- [ ] If you score 7 or below: ❌ Review Quick-Reference-Compute.md again, retake quiz

#### Success Criteria:
- ✅ Can recite instance families from memory
- ✅ Can match use cases to correct instance types
- ✅ Can identify placement group type for HPC vs Hadoop vs critical instances
- ✅ Scored 8/10 or higher on quiz

---

### Day 2: Database Fundamentals (Sunday, December 1)

**Time:** 2 hours
**Goal:** Master RDS Multi-AZ, Read Replicas, Aurora, and DynamoDB capacity modes
**Target Score:** 8/10 (80%) on Database-only quiz

#### Morning Session (1 hour 15 min)

**9:00 - 9:45 AM: Read Quick-Reference-Databases.md**
- [ ] RDS section: Multi-AZ vs Read Replicas
- [ ] Aurora section: Advantages over RDS
- [ ] DynamoDB section: Provisioned vs On-Demand capacity

**9:45 - 10:05 AM: Memorize Decision Trees**

**Multi-AZ vs Read Replicas:**
```
High Availability + Automatic Failover → Multi-AZ
- Synchronous replication
- Automatic failover (60-120 sec RDS, ~30 sec Aurora)
- Same connection string
- 2x cost

Read Scaling + Reporting → Read Replicas
- Asynchronous replication
- Different connection strings
- Can be cross-region
- Offload reporting workloads
```

**Aurora Advantages:**
```
Why choose Aurora over RDS:
- Faster failover (~30 sec vs 60-120 sec)
- Auto-scaling read replicas
- Shared storage (not copied to each replica)
- 5x faster than MySQL, 3x faster than PostgreSQL
- Aurora Global Database (<1 sec cross-region replication)
```

**DynamoDB Capacity:**
```
On-Demand:
- Unpredictable traffic
- New applications
- 10x traffic variation
- Pay per request

Provisioned:
- Predictable traffic
- Steady workloads
- Cost optimization
- Set RCU/WCU
```

**10:05 - 10:15 AM: Write from Memory**
- [ ] When to use Multi-AZ vs Read Replicas
- [ ] 5 advantages of Aurora over RDS
- [ ] When to use DynamoDB On-Demand vs Provisioned

#### Afternoon Session (45 min)

**3:00 - 3:10 PM: Review CloudWatch Metrics**
- [ ] ReplicaLag is published on SOURCE database (not replica)
- [ ] DatabaseConnections, CPUUtilization locations
- [ ] Write from memory: where to find ReplicaLag metric

**3:10 - 3:45 PM: Take Database-Only Quiz (10 questions)**
- [ ] Target: 8/10 (80%)
- [ ] If you score 8+: ✅ Move to Day 3
- [ ] If you score 7 or below: ❌ Review Quick-Reference-Databases.md again, retake quiz

#### Success Criteria:
- ✅ Can explain Multi-AZ vs Read Replicas from memory
- ✅ Can list 5 Aurora advantages
- ✅ Can choose correct DynamoDB capacity mode based on traffic pattern
- ✅ Know where ReplicaLag metric is published
- ✅ Scored 8/10 or higher on quiz

---

### Day 3: VPC & Networking (Monday, December 2)

**Time:** 2 hours
**Goal:** Master VPC components, NACLs, Security Groups, and ephemeral ports
**Target Score:** 8/10 (80%) on Networking-only quiz

#### Morning Session (1 hour 15 min)

**7:00 - 7:45 AM: Read Quick-Reference-Networking.md**
- [ ] VPC components: subnets, route tables, IGW, NAT Gateway
- [ ] Security Groups vs NACLs
- [ ] VPC Endpoints: Gateway vs Interface

**7:45 - 7:55 AM: Write from Memory - CRITICAL**
- [ ] "NACLs are _____ (stateful or stateless)?"
- [ ] "Security Groups are _____ (stateful or stateless)?"
- [ ] "For outbound internet access, NACL must allow inbound ports _____"
- [ ] Check your answers:
  - NACLs are STATELESS
  - Security Groups are STATEFUL
  - NACL must allow inbound ports 1024-65535 (ephemeral)

**7:55 - 8:10 AM: Draw VPC Architecture**
- [ ] Draw from memory: VPC with public and private subnets
- [ ] Include: IGW, NAT Gateway, Route Tables, NACLs, Security Groups
- [ ] Verify against Quick-Reference-Networking.md diagrams

**8:10 - 8:15 AM: Write 10 Times**
"NACLs are STATELESS. Return traffic needs explicit inbound ephemeral ports 1024-65535."

#### Afternoon Session (45 min)

**6:00 - 6:10 PM: Review VPC Endpoints**
- [ ] Gateway Endpoints: S3, DynamoDB only, FREE
- [ ] Interface Endpoints: All other services, costs money
- [ ] When to use each type

**6:10 - 6:45 PM: Take Networking-Only Quiz (10 questions)**
- [ ] Target: 8/10 (80%)
- [ ] If you score 8+: ✅ Move to Day 4
- [ ] If you score 7 or below: ❌ Review NACL section again, retake quiz

#### Success Criteria:
- ✅ Can recite: "NACLs are stateless, need ephemeral ports 1024-65535"
- ✅ Can draw VPC architecture from memory
- ✅ Can identify connection timeout as NACL issue (not routing)
- ✅ Know Gateway endpoints are FREE, Interface endpoints cost money
- ✅ Scored 8/10 or higher on quiz

---

### Day 4: S3 Storage Classes Deep Dive (Tuesday, December 3)

**Time:** 1.5 hours
**Goal:** MASTER S3 storage class selection - eliminate this #1 weakness
**Target Score:** 9/10 (90%) on S3-only quiz

#### Session 1 (30 min) - 7:00 - 7:30 AM

**Read Day-7-Week-1-Deep-Dive-Review.md Section 1 (S3) THREE times**
- [ ] First read: Understand the decision tree
- [ ] Second read: Highlight key patterns
- [ ] Third read: Write summary in your own words

#### Session 2 (20 min) - 7:30 - 7:50 AM

**Memorize Storage Class Retrieval Times:**
- [ ] S3 Standard: Milliseconds (immediate)
- [ ] S3 Standard-IA: Milliseconds (immediate)
- [ ] S3 Intelligent-Tiering: Milliseconds (immediate)
- [ ] Glacier Instant Retrieval: Milliseconds (immediate)
- [ ] Glacier Flexible (Expedited): 1-5 minutes
- [ ] Glacier Flexible (Standard): 3-5 hours
- [ ] Glacier Deep Archive (Standard): 12 hours

**Create Sticky Note:**
```
"RARELY ACCESSED" + "IMMEDIATELY" = STANDARD-IA
"RARELY ACCESSED" + "HOURS WAIT OK" = GLACIER FLEXIBLE
"ALMOST NEVER" + "12+ HOURS OK" = GLACIER DEEP ARCHIVE
```

**Put sticky note on your monitor - DON'T REMOVE until you get 5 S3 questions right in a row**

#### Session 3 (15 min) - 7:50 - 8:05 AM

**Memorize Valid Lifecycle Transitions:**
```
Standard → Standard-IA → Glacier Instant → Glacier Flexible → Deep Archive

Standard → Intelligent-Tiering (allowed)
Intelligent-Tiering → Glacier (NOT ALLOWED) ❌

Cannot reverse transitions (one-way only)
```

#### Session 4 (25 min) - 6:00 - 6:25 PM

**Take S3-Only Quiz (10 questions)**
- [ ] Target: 9/10 (90%)
- [ ] If you score 9+: ✅ Move to Day 5
- [ ] If you score 8 or below: ❌ Re-read Day-7 review, check sticky note, retake quiz

#### Success Criteria:
- ✅ Can match retrieval times to storage classes from memory
- ✅ "Rarely + Immediately" immediately triggers "Standard-IA" in your brain
- ✅ Know you can't transition from Intelligent-Tiering to Glacier
- ✅ Sticky note is on monitor and you've committed it to memory
- ✅ Scored 9/10 or higher on quiz

---

### Day 5: Load Balancing & Auto Scaling (Wednesday, December 4)

**Time:** 1.5 hours
**Goal:** Perfect load balancer selection and Auto Scaling policy combinations
**Target Score:** 8/10 (80%) on Load Balancing quiz

#### Morning Session (45 min) - 7:00 - 7:45 AM

**7:00 - 7:30 AM: Review ALB vs NLB vs GWLB**
- [ ] Read Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.md
- [ ] Memorize decision criteria for each type

**7:30 - 7:40 AM: Memorize Cross-Zone Load Balancing Costs**
```
ALB Cross-Zone: FREE (enabled by default)
NLB Cross-Zone: COSTS MONEY (disabled by default)
GWLB Cross-Zone: COSTS MONEY (disabled by default)

Memory trick: ALB = Always free
```

**7:40 - 7:45 AM: Write from Memory**
- [ ] "ALB cross-zone load balancing costs: _____" (FREE)
- [ ] "NLB cross-zone load balancing costs: _____" (MONEY)

#### Afternoon Session (45 min) - 6:00 - 6:45 PM

**6:00 - 6:20 PM: Review Auto Scaling Policies**

**Combined Policy Pattern:**
```
Predictable pattern + Unpredictable spikes = Scheduled + Target Tracking

Example:
- Predictable: 9 AM spike every weekday
- Unpredictable: Flash sales, random traffic
- Solution: Scheduled Action (9 AM) + Target Tracking (CPU 70%)
```

**Session Management:**
```
Sticky Sessions + Centralized Session Store = WRONG (mutually exclusive)

If using Redis/DynamoDB for sessions:
- DON'T use sticky sessions
- Prevents cost-effective scale-in
- Any instance can serve any request
```

**6:20 - 6:45 PM: Take Load Balancing Quiz (10 questions)**
- [ ] Target: 8/10 (80%)
- [ ] If you score 8+: ✅ Move to Day 6
- [ ] If you score 7 or below: ❌ Review cheat sheet again, retake quiz

#### Success Criteria:
- ✅ Know ALB cross-zone is FREE, NLB/GWLB cross-zone costs money
- ✅ Can identify when to combine Scheduled + Target Tracking
- ✅ Understand sticky sessions contradict centralized session storage
- ✅ Scored 8/10 or higher on quiz

---

### Day 6: Foundation Quiz Retake (Thursday, December 5)

**Time:** 1 hour
**Goal:** Retake the original Day 8 Foundation Quiz and score 80%+
**Target Score:** 16/20 (80%)

#### Session (1 hour) - 7:00 - 8:00 AM

**7:00 - 7:40 AM: Retake Foundation Quiz (20 questions)**
- [ ] Same 20 questions from Day 8
- [ ] Don't look at previous answers
- [ ] Read each question carefully
- [ ] Underline key requirements before answering

**7:40 - 8:00 AM: Review Results**
- [ ] Compare to first attempt
- [ ] Identify patterns in missed questions
- [ ] Review those specific topics immediately

#### Success Criteria:
- ✅ Scored 16/20 (80%) or higher
- ✅ If 16+: Ready for Day 7
- ✅ If 14-15: Review weak topics today, retake tomorrow
- ✅ If <14: REPEAT Days 1-5, you're not ready

---

### Day 7: Week 1 Comprehensive Quiz (Friday, December 6)

**Time:** 1.5 hours
**Goal:** Take NEW comprehensive Week 1 quiz covering all topics
**Target Score:** 24/30 (80%)

#### Session (1.5 hours) - 7:00 - 8:30 AM

**7:00 - 8:00 AM: Comprehensive Quiz (30 questions)**
- [ ] Mix of EC2, RDS, S3, VPC, Lambda, Auto Scaling, Load Balancing
- [ ] Read questions carefully
- [ ] Apply all patterns learned in Days 1-6

**8:00 - 8:30 AM: Final Review**
- [ ] Score the quiz
- [ ] Review each missed question
- [ ] Document any NEW weaknesses discovered
- [ ] Determine readiness for Week 2

#### Success Criteria:
- ✅ Scored 24/30 (80%) or higher
- ✅ If 24+: CLEARED for Week 2
- ✅ If 20-23: Conditional - review weak topics over weekend, start Week 2 Monday
- ✅ If <20: NOT READY - must repeat recovery plan

---

## ✅ Week 2 Entry Checklist

**Complete this checklist before advancing to Week 2:**

### Quiz Scores:
- [ ] Day 1: EC2-only quiz ≥ 8/10 (80%)
- [ ] Day 2: Database-only quiz ≥ 8/10 (80%)
- [ ] Day 3: Networking-only quiz ≥ 8/10 (80%)
- [ ] Day 4: S3-only quiz ≥ 9/10 (90%)
- [ ] Day 5: Load Balancing quiz ≥ 8/10 (80%)
- [ ] Day 6: Foundation retake ≥ 16/20 (80%)
- [ ] Day 7: Week 1 comprehensive ≥ 24/30 (80%)

### Knowledge Verification:
- [ ] Can recite EC2 instance families (c, m, r, i, d, t) from memory
- [ ] Can explain Multi-AZ vs Read Replicas without notes
- [ ] Can recite: "NACLs are stateless, need ephemeral ports 1024-65535"
- [ ] Can match S3 retrieval requirements to correct storage class
- [ ] Know Lambda timeout is 15 minutes maximum
- [ ] Know ALB cross-zone is FREE, NLB/GWLB cross-zone costs money

### If ALL checkboxes are ✅:
**You are CLEARED for Week 2. Well done! 🎉**

### If ANY checkbox is ❌:
**You are NOT READY for Week 2. Review that topic and retake the quiz.**

---

## 📊 Progress Tracking

**Update this table daily:**

| Day | Topic | Study Complete? | Quiz Score | Pass? |
|-----|-------|----------------|-----------|-------|
| Day 1 | EC2 | ⬜ | __/10 | ⬜ |
| Day 2 | Databases | ⬜ | __/10 | ⬜ |
| Day 3 | VPC/Networking | ⬜ | __/10 | ⬜ |
| Day 4 | S3 Storage | ⬜ | __/10 | ⬜ |
| Day 5 | Load Balancing | ⬜ | __/10 | ⬜ |
| Day 6 | Foundation Retake | ⬜ | __/20 | ⬜ |
| Day 7 | Week 1 Comprehensive | ⬜ | __/30 | ⬜ |

---

## 🎯 Final Notes

**This is your recovery plan. Stick to it.**

- **No shortcuts** - If you skip a day, you're setting yourself up to fail
- **Hit the targets** - 80% is the minimum, aim for 90%+
- **Review immediately** - When you miss a question, study that topic for 15 minutes right away
- **Don't advance early** - Even if you feel confident, complete all 7 days

**You have 17 days until the exam. Use them wisely.**

**Now go start Day 1. Good luck! 💪**
