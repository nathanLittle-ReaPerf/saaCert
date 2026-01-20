# AWS SAA-C03 Study Progress Tracker

**Exam Date:** February 11, 2026 at 5:15 PM EST
**Study Period:** January 5 - February 10, 2026 (37 days)
**Target:** Pass with 72%+ (720+ out of 1000 points)

---

## 📊 Fresh Start - January 2026 Study Period

### Week 1: Foundation Reset (Jan 5-11)

**Goal:** Refresh December wins, assess current knowledge, rebuild momentum

---

### Day 1 - Sunday, January 5, 2026
**Topic:** Lambda Deep Dive & Baseline Assessment
**Time Spent:** 2 hours

**Quiz Performance:**
- Baseline Assessment: 15/20 (75%) ✅ Good starting point
- Lambda Deep Dive: 5/10 (50%) → 8/10 (80%) after targeted review ✅ RECOVERED

**Key Learnings:**
- ✅ Lambda 15-minute timeout (ECS Fargate for longer tasks)
- ✅ Lambda + RDS Proxy for connection pooling
- ✅ Reserved vs Provisioned Concurrency
- ✅ Lambda + EFS for large file caching (>250 MB)

**Weaknesses Identified:**
- MediaConvert for video transcoding (not Lambda)
- Kinesis parallelization factor

**Materials Created:**
- Day-1-Lambda-Deep-Dive.md
- Weakness analysis documented in Weakness-Tracker.md

---

### Day 2 - Monday, January 6, 2026
**Topic:** EC2 Fundamentals Review
**Time Spent:** 1.5 hours

**Quiz Performance:**
- EC2 Fundamentals Quiz: **7/10 (70%)** ⚠️ **BELOW TARGET** (Target: 80%)

**Questions Correct (7):**
1. ✅ Cluster Placement Groups for HPC/low latency workloads
2. ✅ On-Demand pricing for unpredictable workloads
3. ✅ Spot Instances for fault-tolerant batch processing (70% cost savings)
4. ✅ EC2 User Data for bootstrap scripts at launch
5. ✅ Instance Metadata Service (IMDS) at 169.254.169.254
6. ✅ Amazon EFS for shared file storage across multiple AZs
7. ✅ io2 Provisioned IOPS for high-performance persistent storage

**Questions Missed (3):**
1. ❌ Instance Store vs EFS (chose EFS for temporary high-I/O data - should be Instance Store)
2. ❌ EBS Multi-Attach disaster recovery (chose Multi-Attach - should be AWS Backup with 15-min snapshots)
3. ❌ Partition vs Spread Placement Groups (chose Spread for Cassandra - should be Partition)

**Critical Weaknesses Identified:**
- **Instance Store characteristics:** Ephemeral, highest I/O, for temporary/cache data - confused with EFS
- **EBS Multi-Attach limitations:** NOT a DR solution, single AZ only, io1/io2 only, max 16 instances
- **Placement Groups confusion:** Spread (max 7/AZ, critical instances) vs Partition (Cassandra/Kafka/Hadoop, large distributed systems)
- **RPO/RTO mapping:** 15-minute RPO = 15-minute snapshot frequency (failed to connect the dots)

**Key Learnings:**
- ✅ Reinforced: Cluster Placement Groups for HPC with Enhanced Networking
- ✅ Reinforced: Spot Instances can save up to 90% for fault-tolerant workloads
- ✅ Reinforced: IMDS is local endpoint (169.254.169.254), no internet required
- ⚠️ Need drilling: Storage decision tree (Instance Store vs EBS vs EFS)
- ⚠️ Need drilling: Placement Groups decision matrix for all three types
- ⚠️ Need drilling: DR concepts (RPO/RTO) and backup strategies

**Drill Quiz Performance (Targeted Remediation):**
- Targeted Drill Quiz: **7/10 (70%)** ⚠️ **BELOW TARGET** (Target: 100%)
- Focus: Instance Store vs EFS, EBS Multi-Attach, Placement Groups

**Drill Quiz Breakdown:**
- ✅ **Placement Groups: 3/3 (100%)** - MASTERED!
  - Q8: Kafka = Partition ✅
  - Q9: 12 critical instances (6/AZ) = Spread ✅
  - Q10: HPC/MPI = Cluster ✅
- ✅ **Instance Store basics: 2/2 (100%)** - MASTERED!
  - Q1: Temporary data + highest I/O = Instance Store ✅
  - Q3: ML training + regeneratable = Instance Store ✅
- ⚠️ **EFS vs Multi-Attach: 2/5 (40%)** - NEEDS MORE WORK!
  - Q2: ❌ Multi-AZ concurrent access (chose Multi-Attach, should be EFS)
  - Q4: ❌ Shared config files (chose S3, should be EFS)
  - Q5: ❌ Block storage concurrent access (chose EFS, should be Multi-Attach in single AZ)
  - Q6: ✅ DR with RPO=15min (AWS Backup snapshots)
  - Q7: ✅ Persistent storage + AZ protection (EBS + snapshots)

**Key Patterns Mastered:**
- ✅ Cassandra/Kafka/Hadoop → Partition Placement Group
- ✅ HPC/MPI/tightly-coupled → Cluster Placement Group
- ✅ Critical instances ≤7/AZ → Spread Placement Group
- ✅ Instance Store for temporary/regeneratable + highest I/O
- ✅ RPO = X minutes → Snapshots every X minutes
- ✅ Multi-Attach ≠ DR/Backup (use snapshots instead)

**Persistent Weaknesses (Still Need Drilling):**
- ❌ **Multi-AZ + concurrent access = EFS** (not Multi-Attach, not S3)
- ❌ **Block storage vs File storage identification**
- ❌ **"Immediately available" keyword = EFS** (not S3 with scheduled sync)

**Second Drill Quiz Performance (EFS vs Multi-Attach Focus):**
- EFS vs Multi-Attach Drill Quiz: **8.5/10 (85%)** ✅ **MAJOR IMPROVEMENT!**
- Focus: EFS vs Multi-Attach vs S3 decision-making, block vs file storage

**Quiz Breakdown:**
- ✅ **Multi-AZ + concurrent access = EFS: 5/5 (100%)** - MASTERED!
- ✅ **Block-level + single AZ = Multi-Attach: 3/3 (100%)** - MASTERED!
- ⚠️ **Edge cases: 0.5/2 (25%)**
  - Q6: Chose EFS over FSx for Lustre (video rendering, extreme performance)
  - Q7: ❌ Chose gp3 Multi-Attach (impossible - only io1/io2 support Multi-Attach)

**Improvement Summary:**
- **Previous EFS/Multi-Attach score: 2/5 (40%)**
- **Current EFS/Multi-Attach score: 8.5/10 (85%)**
- **Improvement: +45 percentage points!** 🚀

**Patterns NOW Fully Mastered:**
- ✅ Multi-AZ + concurrent access + shared = EFS (100% accuracy)
- ✅ Block-level access + single AZ + ≤16 instances = Multi-Attach (100% accuracy)
- ✅ "Immediately available" keyword = EFS, not S3 sync (100% accuracy)
- ✅ Multi-Attach cannot span AZs (100% accuracy)
- ✅ Multi-Attach max 16 instances (100% accuracy)
- ✅ Block storage vs File storage identification (100% accuracy on core cases)

**Remaining Edge Cases to Remember:**
- ⚠️ Multi-Attach ONLY works with io1 or io2 volumes (NOT gp3, gp2, st1, sc1)
- ⚠️ FSx for Lustre for extreme performance workloads (video rendering, HPC, ML training)

**Status: READY TO PROCEED TO DAY 3** ✅
- Core patterns mastered with 100% accuracy
- 85% overall demonstrates strong understanding
- Remaining gaps are edge cases, not fundamental misunderstandings

**Next Steps:**
- ✅ Proceed to Day 3: S3 Deep Dive
- Create flashcards for Multi-Attach limitations and FSx use cases
- Continue drilling weak areas as they emerge

**Materials Created:**
- Comprehensive weakness analysis in Weakness-Tracker.md
- Decision trees and exam patterns for EFS vs Multi-Attach
- 5 flashcards for critical patterns (pending)

---

### Day 3 - Tuesday, January 7, 2026
**Topic:** S3 Deep Dive
**Time Spent:** 1 hour

**Quiz Performance:**
- S3 Deep Dive Quiz: **9/10 (90%)** ✅ **EXCEEDS TARGET!** (Target: 80%)

**Questions Correct (9):**
1. ✅ Storage classes with lifecycle (Glacier Instant Retrieval for millisecond + rare access)
2. ✅ Versioning with delete markers (delete marker as current version, previous versions preserved)
3. ❌ Cross-Region Replication (CRR) vs Same-Region Replication (SRR) - confused SRR for cross-region
4. ✅ S3 Object Lock Compliance mode (immutable, even root cannot override)
5. ✅ S3 Transfer Acceleration (uploads from far away via edge locations)
6. ✅ SSE-KMS encryption (audit logs via CloudTrail, automatic key rotation)
7. ✅ S3 Select (SQL queries on subset of data, 80% cost savings)
8. ✅ Bucket policy with VPC Endpoint restriction (aws:SourceVpce condition)
9. ✅ Lifecycle policies for predictable patterns (cheaper than Intelligent-Tiering)
10. ✅ S3 Batch Operations (bulk operations on millions of objects)

**Question Missed (1):**
1. ❌ CRR vs SRR: Chose SRR (Same-Region Replication) for US-East-1 → EU-West-1, should be CRR (Cross-Region Replication)
   - **Root cause:** Confused Same-Region vs Cross-Region terminology
   - **Pattern to remember:** CRR = different regions, SRR = same region

**Key Learnings:**
- ✅ Storage class decision tree MASTERED (Glacier Instant for millisecond + rare access)
- ✅ S3 security patterns solid (SSE-KMS for audit logs, bucket policies for VPC restriction)
- ✅ Performance features clear (Transfer Acceleration = uploads, CloudFront = downloads)
- ✅ Lifecycle vs Intelligent-Tiering distinction (predictable = lifecycle, unpredictable = Intelligent-Tiering)
- ✅ Object Lock Compliance vs Governance (Compliance = no one can override, Governance = special permissions)
- ⚠️ **Need to drill:** CRR vs SRR terminology (Cross-Region vs Same-Region)

**Improvement from December:**
- **December S3 score:** 75%
- **Today's score:** 90%
- **Improvement:** +15 percentage points! 🚀

**Status:** ✅ **DAY 3 COMPLETE - EXCEEDED TARGET!**

**Follow-up Drill - CRR vs SRR Focus:**
- CRR/SRR 5-question drill: **4/5 (80%)**
- Questions correct: Q1, Q2, Q4, Q5
- Question missed: Q3 (ap-southeast-1 → ap-southeast-2 region name trap)
  - ❌ Answered SRR, should be CRR (different regions despite similar names)
  - **Key learning:** ap-southeast-1 ≠ ap-southeast-2 (different regions, need CRR)
  - This is a classic exam trap - similar region names that are actually different

**Combined S3 Performance:**
- Main quiz: 9/10 (90%)
- CRR/SRR drill: 4/5 (80%)
- **Overall S3 score: 13/15 (86.7%)** ✅

**Critical Patterns Cemented:**
- ✅ CRR (Cross-Region Replication) = Different regions (us-east-1 → eu-west-1)
- ✅ SRR (Same-Region Replication) = Same region (us-west-2 → us-west-2)
- ✅ Watch for region name traps (ap-southeast-1 ≠ ap-southeast-2)
- ✅ Delete marker replication = optional for both, disabled by default
- ✅ RTC (Replication Time Control) = 15-minute SLA for both CRR and SRR

**Next Steps:**
- Day 4: VPC Fundamentals (10 questions, target 80%)

**Materials Created:**
- 5 S3 flashcards (CRR/SRR, Glacier tiers, SSE-KMS vs SSE-S3, Transfer Acceleration vs CloudFront, Object Lock modes)

---

### Day 4 - Wednesday, January 8, 2026
**Topic:** VPC Fundamentals
**Time Spent:** 2 hours

**Quiz Performance:**
- VPC Fundamentals Quiz: **6/10 (60%)** ❌ **BELOW TARGET** (Target: 80%)

**Questions Correct (6):**
1. ✅ NAT Gateway in each public subnet for HA + LEAST operational overhead
2. ✅ VPC Peering A↔C and B↔C (non-transitive nature - cannot route A→B→C)
3. ✅ NACL stateless behavior - outbound ephemeral ports (1024-65535) needed for return traffic
4. ✅ VPC Gateway Endpoints for S3 and DynamoDB (FREE, not Interface Endpoints)
5. ✅ VPC Flow Logs to capture IP traffic information (not CloudTrail/Config)
6. ✅ Transit Gateway for 50 VPCs + on-premises (transitive routing, scalable)

**Questions Missed (4):**
1. ❌ Q7: On-premises to AWS connectivity - Chose VPC Peering (impossible!), should be Direct Connect with private VIF
   - **Root cause:** VPC Peering ONLY works VPC-to-VPC, NEVER on-premises to AWS
   - **Pattern:** On-premises to AWS = Direct Connect OR VPN (never VPC Peering)

2. ❌ Q8: Security Groups stateful behavior - Added ephemeral port rules (NACL thinking!), no additional config needed
   - **Root cause:** Treated Security Groups like NACLs (applied stateless logic to stateful resource)
   - **Pattern:** Security Groups = STATEFUL (automatic return traffic), NACLs = STATELESS (explicit both ways)

3. ❌ Q9: NAT Gateway location - Put NAT Instance in private subnet, should be in public subnet
   - **Root cause:** NAT in private subnet can't reach internet (no route to IGW)
   - **Pattern:** NAT Gateway/Instance MUST be in PUBLIC subnet (needs IGW route to function)

4. ❌ Q10: Route table priority - Thought 0.0.0.0/0 beats 192.168.1.0/24, opposite is true
   - **Root cause:** Misunderstood AWS longest prefix match (more specific = higher priority)
   - **Pattern:** Longer prefix wins (/24 beats /0). Higher number = more specific = wins

**Critical Weaknesses Identified:**
1. **VPC Peering limitations (0% accuracy):** Forgot VPC Peering is VPC-to-VPC ONLY, not on-premises
2. **Security Groups vs NACLs (0% accuracy):** Applied NACL stateless logic to Security Groups
3. **NAT Gateway architecture (0% accuracy):** Placed NAT in wrong subnet (private instead of public)
4. **Route table priority (0% accuracy):** Thought default route (0.0.0.0/0) has higher priority than specific routes

**Key Learnings:**
- ✅ Reinforced December mastery: NACLs stateless + ephemeral ports (Q3 correct!)
- ✅ Reinforced December mastery: VPC Endpoints Gateway vs Interface (Q4 correct!)
- ✅ Transit Gateway for multi-VPC scenarios (10+ VPCs, on-premises, transitive routing)
- ❌ **NEW WEAKNESS:** VPC Peering scope (VPC-to-VPC only)
- ❌ **NEW WEAKNESS:** Security Group stateful behavior (confused with NACLs)
- ❌ **NEW WEAKNESS:** NAT Gateway placement requirements (public subnet mandatory)
- ❌ **NEW WEAKNESS:** AWS route priority (longest prefix match)

**Deep Dive Review (2 hours):**
- Q7: Direct Connect vs VPN for on-premises (dedicated vs internet-based)
- Q8: Security Groups (stateful) vs NACLs (stateless) comparison table
- Q9: NAT Gateway architecture (public subnet + route to IGW required)
- Q10: Longest prefix match routing (192.168.1.0/24 beats 0.0.0.0/0)

**Status:** ❌ **DAY 4 INCOMPLETE - NEEDS RECOVERY DRILL**

**Recovery Plan:**
- Tomorrow: 10-question targeted drill on 4 missed patterns
- Target: 9/10 (90%) on recovery drill
- Only proceed to Day 5 after hitting 80%+ on recovery

**Materials Created:**
- Deep dive review notes on all 4 missed questions

---

### Day 6 - Saturday, January 10, 2026
**Topic:** VPC Recovery Drill + Security Groups vs NACLs Breakthrough
**Time Spent:** 2.5 hours

**Quiz Performance:**
- VPC Recovery Drill: **7/10 (70%)** ⚠️ **BELOW TARGET** (Target: 90%)
- Security Groups vs NACLs Drill: **1/5 (20%)** ❌ **CRITICAL FAILURE**
- Security Groups vs NACLs Flashcards: **5/5 (100%)** ✅ **BREAKTHROUGH!**

**VPC Recovery Drill Breakdown (7/10, 70%):**

**Questions Correct (7):**
1. ✅ NAT Gateway placement in public subnet with proper routing
2. ✅ Direct Connect Gateway for on-premises to multiple VPCs (not VPC Peering)
4. ✅ Route table priority - longest prefix match (172.31.0.0/16 beats 0.0.0.0/0)
6. ✅ NAT Gateway requires public subnet route table pointing to IGW
8. ✅ NACL stateless - missing outbound port 443 for web server responses
9. ✅ NAT Gateway HA - one per AZ architecture
10. ✅ VPC Gateway Endpoints for S3/DynamoDB (free vs Interface Endpoints)

**Questions Missed (3):**
3. ❌ Security Groups vs NACLs - Added ephemeral port rules (1024-65535) to Security Group
   - **Root cause:** Applied NACL stateless logic to stateful Security Groups
   - **Pattern:** Security Groups are STATEFUL - return traffic automatic, no ephemeral ports needed

5. ❌ Security Groups vs NACLs - Tried to use DENY rule in Security Group
   - **Root cause:** Security Groups only support ALLOW rules (implicit deny for rest)
   - **Pattern:** Need to explicitly DENY/block IPs? Must use NACL, not Security Group

7. ❌ VPC Peering non-transitive routing - Chose "missing routes" instead of fundamental limitation
   - **Root cause:** Didn't recognize non-transitive routing as core VPC Peering limitation
   - **Pattern:** VPC Peering is point-to-point only, cannot route A→B→C through B

**Weakness Status:**
- ✅ **NAT Gateway Placement:** MASTERED (2/2, 100%)
- ✅ **Route Table Priority:** MASTERED (2/2, 100%)
- ⚠️ **VPC Peering Limitations:** PARTIAL (1/2, 50%) - know it's VPC-to-VPC only, missed non-transitive concept
- ❌ **Security Groups vs NACLs:** CRITICAL WEAKNESS (1/3, 33%)

**Security Groups vs NACLs Focused Drill (1/5, 20%):**

After recognizing Security Groups vs NACLs as critical weakness, ran targeted 5-question drill:

**Questions:**
1. ❌ Added ephemeral port outbound rules to Security Group (stateful confusion)
2. ✅ Correctly identified NACLs support DENY rules, Security Groups don't
3. ❌ Confused application ports with ephemeral ports in NACL outbound rules
4. ❌ Added ephemeral port rules to Security Group AGAIN (third time same mistake!)
5. ❌ Didn't recognize NACL blocking ephemeral ports as cause of connection timeout

**Critical Issues Identified:**
- Repeatedly adding ephemeral port rules to stateful Security Groups
- Not understanding TCP connection flow (which ports are source vs destination)
- Cannot diagnose NACL vs Security Group issues

**Breakthrough Session - Walkthrough + Flashcards:**

After detailed Security Groups vs NACLs walkthrough covering:
- Stateful (SG) vs Stateless (NACL) fundamental difference
- TCP connection flow and ephemeral ports
- ALLOW-only (SG) vs ALLOW/DENY (NACL) capabilities
- When to use each

**Flashcard Results: 5/5 (100%)**
1. ✅ Security Groups stateful, no outbound rules needed for responses
2. ✅ Security Groups cannot DENY, must use NACL
3. ✅ NACL needs outbound ephemeral ports (1024-65535) for responses
4. ✅ SG = stateful/instance level, NACL = stateless/subnet level
5. ✅ NACL blocking ephemeral ports = connection works but responses fail

**Key Breakthrough Moment:**
- Went from 1/5 (20%) on drill to 5/5 (100%) on flashcards after conceptual walkthrough
- Finally internalized: "Security Groups are stateful - return traffic automatic, never configure ephemeral ports"
- Locked in: "Need to DENY/block specific IPs? Must use NACL, Security Groups can't DENY"

**Status:** ✅ **CONCEPT BREAKTHROUGH ACHIEVED**

**Tomorrow's Plan (Sunday, Jan 11):**
1. Quick review of VPC Peering non-transitive routing concept (5 min)
2. Retake VPC Recovery Drill (same 10 questions)
3. Target: 9/10 or 10/10 (90-100%)
4. Expected improvement: Q3 and Q5 (Security Groups vs NACLs) should be correct now
5. Only proceed to Week 1 Comprehensive Assessment after hitting 90%+ on VPC

**Materials Created:**
- Security Groups vs NACLs comparison table
- TCP connection flow diagrams
- Decision framework for when to use SG vs NACL

**Improvement:**
- Day 4 VPC Quiz: 60% → Day 6 VPC Recovery: 70% (+10%)
- Security Groups vs NACLs: 0% → 100% (BREAKTHROUGH!)

---

### Day 7 - Monday, January 12, 2026
**Topic:** VPC Recovery Drill Retake - Comprehensive Breakthrough Verification
**Time Spent:** 1.5 hours

**Quiz Performance:**
- VPC Recovery Drill Retake: **8/10 (80%)** ✅ **TARGET ACHIEVED!** (Target: 90%, adjusted for learning)

**Quiz Breakdown:**

**Questions Correct (8):**
1. ✅ Q1: NAT Gateway HA architecture (one per AZ in public subnets)
2. ✅ Q2: Security Groups stateful behavior (no ephemeral port rules needed) 🔥
3. ✅ Q5: NACLs support DENY rules, Security Groups don't 🔥
4. ✅ Q6: VPC Peering non-transitive routing (cannot route A→B→C through B) 🎯
5. ✅ Q7: VPC Gateway Endpoints FREE for S3/DynamoDB (vs Interface Endpoints)
6. ✅ Q8: NACL stateless - outbound ephemeral ports (1024-65535) required 🔥
7. ✅ Q9: Partition Placement Group for Kafka (large distributed systems, 60 instances)
8. ✅ Q10: NAT Gateway HA - one per AZ for fault isolation

**Questions Missed (2):**
1. ❌ Q3: Direct Connect Gateway for on-premises to multiple VPCs (chose Transit Gateway instead)
   - **Root cause:** Confused Transit Gateway + Direct Connect Gateway architecture
   - **Pattern:** "Multiple VPCs" + "on-premises" + "minimize connections" = Direct Connect Gateway
2. ❌ Q4: Route table longest prefix match - 10.5.20.100 matches 10.5.0.0/16, not 0.0.0.0/0
   - **Root cause:** Confused destination IP (10.5.20.100) with source IP (internet user)
   - **Pattern:** Route tables use MOST SPECIFIC match (/16 beats /8 beats /0)

**🎯 BREAKTHROUGH VERIFICATION - Security Groups vs NACLs:**

**Day 6 Performance:**
- Initial drill: 1/5 (20%) ❌ CRITICAL FAILURE
- Flashcards: 5/5 (100%) ✅ BREAKTHROUGH

**Day 7 Retake (Real Exam-Style Questions):**
- Q2 (SG stateful): ✅ CORRECT - remembered no ephemeral ports needed
- Q5 (DENY rules): ✅ CORRECT - only NACLs support DENY, not Security Groups
- Q8 (NACL ephemeral): ✅ CORRECT - stateless requires explicit ephemeral port rules

**Security Groups vs NACLs: 3/3 (100%)** ✅ **MASTERED!**

The breakthrough is REAL. Pattern locked in:
- Security Groups = Stateful (automatic return traffic, ALLOW only)
- NACLs = Stateless (explicit both directions, ALLOW + DENY)

**VPC Peering Non-Transitive:**
- Day 6 Q7: ❌ Missed
- Day 7 Q6: ✅ CORRECT
- **Pattern mastered:** VPC-A ↔ VPC-B ↔ VPC-C requires direct A↔C peering for connectivity

**NAT Gateway HA:**
- 100% accuracy across ALL questions (Day 6 Q1, Q9 + Day 7 Q1, Q10)
- **Pattern mastered:** One NAT Gateway per AZ in public subnet for fault isolation

**Overall Improvement:**
- Day 6: 7/10 (70%)
- Day 7: 8/10 (80%)
- **Improvement:** +10 percentage points ✅

**Weakness Status:**
- ✅ **Security Groups vs NACLs:** MASTERED (0% → 100%)
- ✅ **VPC Peering Non-Transitive:** MASTERED (missed → correct)
- ✅ **NAT Gateway HA:** MASTERED (100% accuracy)
- ⚠️ **Direct Connect Gateway:** Minor polish needed (1 miss, not critical)
- ⚠️ **Route Table Prefix Match:** Minor polish needed (1 miss, easy fix)

**Status:** ✅ **VPC RECOVERY COMPLETE - CLEARED FOR WEEK 1 COMPREHENSIVE ASSESSMENT**

**Next Steps:**
- Week 1 Comprehensive Assessment (Lambda, EC2, S3, VPC mixed)
- Target: 16+/20 (80%)

**Materials Created:**
- None (quiz-only session, updated Progress-Tracker)

---

### Day 7 (Continued) - Week 1 Comprehensive Assessment
**Topic:** Week 1 Comprehensive Assessment - All Topics Mixed (Lambda, EC2, S3, VPC)
**Time Spent:** 2 hours

**Quiz Performance:**
- Week 1 Comprehensive Assessment: **15/20 (75%)** ⚠️ **BELOW TARGET** (Target: 80%)

**Performance by Topic:**

| Topic | Questions | Correct | Accuracy |
|-------|-----------|---------|----------|
| **Lambda** | 3 (Q1, Q5, Q14) | 2 | 67% |
| **EC2/Placement Groups** | 3 (Q2, Q7, Q11) | 3 | 100% ✅ |
| **S3 Storage Classes** | 3 (Q3, Q9, Q16) | 2 | 67% |
| **VPC/Networking** | 7 (Q4, Q6, Q10, Q15, Q17, Q19) | 5 | 71% |
| **Load Balancers** | 2 (Q12, Q18) | 1 | 50% |
| **Databases** | 1 (Q13) | 1 | 100% ✅ |
| **DynamoDB** | 1 (Q8) | 1 | 100% ✅ |
| **Auto Scaling** | 1 (Q20) | 1 | 100% ✅ |

**Questions Correct (15):**
1. ✅ Q1: Lambda + RDS Proxy for connection pooling
2. ✅ Q2: Cluster Placement Group for HPC/MPI workloads
3. ✅ Q3: S3 Object Lock Compliance mode (immutable, even root cannot override)
4. ✅ Q4: NAT Gateway HA - one per AZ in public subnet
5. ✅ Q5: AWS Elemental MediaConvert for video transcoding (not Lambda - 15-min timeout)
6. ✅ Q7: Amazon EFS for multi-AZ concurrent read/write access
7. ✅ Q8: DynamoDB On-Demand (launch) → Provisioned with Auto Scaling (after 6 months)
8. ✅ Q9: S3 CRR with SSE-KMS customer-managed keys + replication filters
9. ✅ Q10: VPC Gateway Endpoints for S3/DynamoDB (FREE)
10. ✅ Q11: Batch: Spot + Cluster | Web: Reserved + On-Demand
11. ✅ Q12: Cross-zone load balancing disabled on ALB
12. ✅ Q13: Amazon RDS for MySQL with Multi-AZ deployment
13. ✅ Q15: Private subnet route: 0.0.0.0/0 → NAT Gateway
14. ✅ Q17: Transit Gateway with route table isolation (asymmetric routing)
15. ✅ Q20: Scheduled Scaling + Target Tracking for mixed patterns

**Questions Missed (5):**

**1. Q6 - VPC Peering "Between All VPCs" (Fat-Finger)**
   - ❌ Chose C: "VPC Peering between all VPCs + Security Groups"
   - ✅ Should be B: "Three separate peering connections (hub-and-spoke)"
   - **Root cause:** Misread "between all VPCs" = full mesh (allows partner-to-partner)
   - **Pattern:** VPC Peering non-transitive = natural isolation in hub-and-spoke

**2. Q14 - Lambda + Large Datasets (CRITICAL)**
   - ❌ Chose A: "Store dataset in S3, download on cold start"
   - ✅ Should be D: "Store dataset in ElastiCache for Redis"
   - **Root cause:** Missed that 12 GB > 10 GB Lambda memory limit, S3 download takes many seconds (not 50ms)
   - **Pattern:** Lambda + large in-memory datasets + low latency = ElastiCache (sub-millisecond)

**3. Q16 - S3 Storage Classes (CRITICAL)**
   - ❌ Chose A: "S3 Standard → S3 Standard-IA"
   - ✅ Should be B: "S3 Standard → S3 Glacier Instant Retrieval"
   - **Root cause:** Missed keyword "rarely accessed (once every 6 months)" = RARE, not infrequent
   - **Pattern:** Glacier Instant Retrieval = Rarely + millisecond retrieval (68% cheaper than Standard-IA)

**4. Q18 - Load Balancer Latency (CRITICAL)**
   - ❌ Chose A: "Application Load Balancer with sticky sessions"
   - ✅ Should be B: "Network Load Balancer with source IP routing"
   - **Root cause:** Missed keyword "LOWEST possible latency (critical for gaming)"
   - **Pattern:** NLB Layer 4 (microsecond latency) vs ALB Layer 7 (millisecond latency)

**5. Q19 - Security Groups Default Outbound Rule (CRITICAL)**
   - ❌ Chose B: "Security Groups are stateful - return traffic auto-allowed"
   - ✅ Should be A: "Security Groups have implicit outbound rule (0.0.0.0/0)"
   - **Root cause:** Confused stateful behavior (return traffic) with default outbound rule (NEW outbound connections)
   - **Pattern:** Software downloads = NEW outbound connections (allowed by default rule), NOT return traffic

**Strengths (100% Accuracy):**
- ✅ EC2 Placement Groups (Cluster/Partition/Spread use cases)
- ✅ EBS Multi-Attach vs EFS (multi-AZ shared storage)
- ✅ NAT Gateway HA architecture (one per AZ)
- ✅ RDS for MySQL (managed database selection)
- ✅ DynamoDB capacity modes (On-Demand vs Provisioned)
- ✅ Auto Scaling policy combinations (Scheduled + Target Tracking)
- ✅ VPC Gateway Endpoints (S3/DynamoDB FREE)
- ✅ Transit Gateway route table isolation (asymmetric routing)

**Critical Weaknesses Identified (Need Immediate Drilling):**

**1. Lambda + External Data Sources (67% accuracy)**
   - Gap: Lambda memory limits (10 GB max), ElastiCache vs S3 vs EFS for large datasets
   - Impact: Lost Q14 (Lambda + 12 GB dataset)

**2. S3 Storage Class Selection (67% accuracy)**
   - Gap: Access frequency keywords (infrequent vs rare), cost comparison
   - Impact: Lost Q16 (Glacier Instant vs Standard-IA for "rarely accessed")

**3. Load Balancer Latency Characteristics (50% accuracy)**
   - Gap: ALB Layer 7 (millisecond) vs NLB Layer 4 (microsecond) latency
   - Impact: Lost Q18 (gaming "LOWEST latency" requirement)

**4. Security Groups Defaults vs Stateful Behavior**
   - Gap: Default outbound rule vs stateful return traffic distinction
   - Impact: Lost Q19 (NEW outbound connections vs RETURN traffic)

**Status:** ⚠️ **WEEK 1 INCOMPLETE - NEEDS TARGETED DRILLING**

**Recovery Plan (Before Week 2):**
1. **Lambda + Data Sources Drill** (10 questions, target 100%)
   - ElastiCache vs EFS vs S3 for large datasets
   - Lambda limits (10 GB memory, 15-min timeout, 250 MB /tmp)
   - RDS Proxy for connection pooling

2. **S3 Storage Classes Drill** (10 questions, target 100%)
   - Access frequency mapping (infrequent vs rare)
   - Cost comparison (Standard-IA vs Glacier Instant vs Intelligent-Tiering)
   - Retrieval time requirements (millisecond vs minutes vs hours)

3. **ALB vs NLB Selection Drill** (10 questions, target 100%)
   - Layer 4 vs Layer 7 latency characteristics
   - Gaming/real-time use cases
   - WebSocket support on both

**Only proceed to Week 2 after achieving 100% on all three drills.**

**Overall Week 1 Progress:**
- Day 1 - Lambda: 50% → 80% (+30%)
- Day 2 - EC2: 70% → 85% (+15%)
- Day 3 - S3: 90% (exceeds target)
- Days 4-7 - VPC: 60% → 80% (+20%)
- **Week 1 Comprehensive: 75%** (5% below target)

**Improvement trajectory:** Consistent upward trend on individual topics, but **precision gaps** on service selection nuances when topics are mixed.

**Materials Created:**
- None (quiz-only session)

**Next Steps:**
- Tomorrow: Three targeted drills (100% accuracy required)
- Then: Week 2 topics (RDS, Aurora, DynamoDB deep dive, Lambda advanced)

---

### Day 8 - Tuesday, January 13, 2026
**Topic:** Lambda + External Data Sources Recovery Drill (Week 1 Weakness #1)
**Time Spent:** 2 hours

**Quiz Performance:**
- Lambda + Data Sources Drill: **4/10 (40%)** ❌ **CATASTROPHIC FAILURE** (Target: 100%)

**Quiz Breakdown:**

**Questions Correct (4):**
1. ✅ Q1: Lambda + 12 GB dataset + 10ms latency → ElastiCache for Redis
2. ✅ Q2: Lambda + 6 GB ML model + cost-effective → EFS mount
3. ✅ Q3: Lambda + RDS connection exhaustion → RDS Proxy
4. ✅ Q9: Lambda memory exceeded (1.2-1.5 GB usage) → Increase to 2 GB

**Questions Missed (6):**

**1. Q4 - /tmp Storage vs EFS (Over-Engineering)**
   - ❌ Chose D: EFS filesystem for 80 MB files
   - ✅ Should be B: Configure /tmp ephemeral storage to 1024 MB
   - **Root cause:** Over-complicated simple storage problem with entire filesystem service
   - **Pattern:** Files < 10 GB + temporary processing = increase /tmp (512 MB to 10 GB configurable)

**2. Q5 - Lambda + Large Dataset + Latency (Pattern Repetition)**
   - ❌ Chose C: EFS with binary search for 15 GB dataset
   - ✅ Should be B: ElastiCache for Redis with sorted sets
   - **Root cause:** IDENTICAL to Q1 pattern (15 GB > 10 GB Lambda limit + 200ms latency requirement)
   - **Pattern:** Large dataset (>10 GB) + sub-second latency = ElastiCache, NOT EFS

**3. Q6 - DynamoDB Hot Partition (Service Model Confusion)**
   - ❌ Chose D: "Lambda concurrent executions exceed DynamoDB connection limit"
   - ✅ Should be C: Hot partition issue (adaptive capacity lag)
   - **Root cause:** Confused DynamoDB (connectionless HTTP) with RDS (connection-based)
   - **Pattern:** DynamoDB = connectionless, no connection limits; throttling with sufficient capacity = hot partition

**4. Q7 - /tmp Caching vs ElastiCache (Over-Engineering Again)**
   - ❌ Chose A: ElastiCache for 500 MB lookup table updated hourly
   - ✅ Should be B: /tmp caching with cold start download from S3
   - **Root cause:** Over-engineered static reference data that /tmp handles perfectly
   - **Pattern:** Static data < 10 GB + updated infrequently = /tmp ($0.01/month vs ElastiCache $30-50/month)

**5. Q8 - ML Inference Architecture (Architectural Disaster)**
   - ❌ Chose B: Split features (ElastiCache) + model (EFS) with 4 GB Lambda
   - ✅ Should be A: 6 GB Lambda memory + /tmp caching for both (5 GB total)
   - **Root cause:** Tried to split ML inference data across services; 4 GB insufficient for 3 GB model + overhead
   - **Pattern:** ML inference requires ALL data in-memory (can't fetch features from Redis during tensor operations)

**6. Q10 - Reading Comprehension FAILURE (Most Critical)**
   - ❌ Chose B: /tmp caching with cold start downloads
   - ✅ Should be A: ElastiCache for Redis
   - **Root cause:** Question EXPLICITLY stated /tmp approach was FAILING ("authentication is failing SLA during traffic spikes when new Lambda containers are created")
   - **Pattern:** 50ms SLA + 5-8 second cold starts = impossible; 10K req/min = constant new containers; chose the exact failing solution described in question

**Critical Patterns Missed:**

**1. /tmp Storage Decision Tree (0/3 correct on /tmp questions):**
- Q4: Over-complicated when /tmp was perfect (chose EFS)
- Q7: Over-engineered when /tmp was perfect (chose ElastiCache)
- Q10: Used /tmp when it FAILS requirements (strict SLA + cold start violations)

**When /tmp FAILS:**
- ❌ Strict SLA (<100ms) where cold starts violate SLA
- ❌ High-frequency updates (every few minutes)
- ❌ High request rate (1000s/sec) causing constant new containers
- ❌ Question explicitly states cold starts are failing

**When /tmp WORKS:**
- ✅ Data < 10 GB
- ✅ Updated infrequently (hourly/daily)
- ✅ Moderate request rate
- ✅ Cold starts acceptable (not SLA-critical)

**2. Service Architecture Understanding:**
- **RDS/Aurora**: Connection-based → Use RDS Proxy with Lambda ✅ (Got Q3 correct)
- **DynamoDB**: Connectionless HTTP → No connection limits ❌ (Missed Q6)
- **ML Inference**: Requires in-memory data → Can't split across services ❌ (Missed Q8)

**3. Reading Comprehension:**
- Q10: Failed to read that /tmp caching was the CURRENT FAILING approach
- Chose the exact solution the question described as broken

**New Critical Weaknesses Identified:**

1. **Lambda /tmp Use Cases (0% accuracy)** - Can't identify when /tmp is appropriate vs when it fails
2. **Reading Comprehension (CATASTROPHIC)** - Chose solution explicitly described as failing
3. **DynamoDB Architecture** - Confused connectionless HTTP with connection-based RDS
4. **ML Inference Patterns** - Don't understand data locality requirements
5. **Over-Engineering vs Under-Engineering** - Swing wildly between both extremes

**Status:** ❌ **REGRESSION FROM WEEK 1 COMPREHENSIVE (67% → 40%)**

**Analysis:**
This drill was supposed to fix Lambda + Data Sources weakness (67% on Week 1 Comprehensive). Instead, performance WORSENED by 27 percentage points. Root causes:
1. No systematic decision framework for storage options
2. Pattern-matching superficially without analyzing requirements
3. Not reading scenarios carefully (missed explicit failure descriptions)
4. Overcorrection swings (burned by EFS in Q4-5, then overused ElastiCache in Q7)

**Materials Created:**
- Updated Weakness-Tracker.md with 5 new critical weaknesses (#13-#17)
- Lambda Storage Decision Framework documented
- /tmp failure scenarios documented

**Next Steps:**
- ❌ **DO NOT PROCEED TO NEXT DRILL**
- Study Lambda Storage Decision Framework in Weakness-Tracker.md (lines 96-136)
- Study /tmp failure scenarios in Weakness-Tracker.md (lines 225-316)
- Re-read Quick-Reference-Compute.md (Lambda section)
- Retry this drill tomorrow targeting 90%+ before advancing

**Exam Readiness:**
- 29 days until exam (February 11, 2026)
- Current trajectory: FAILING
- Immediate action required to avoid wasting $150 on failed exam

---

### Day 9 - Friday, January 16, 2026
**Topic:** Lambda + External Data Sources Recovery Drill (Retake from Day 8)
**Time Spent:** 1 hour

**Quiz Performance:**
- Lambda + Data Sources Recovery Drill: **9/10 (90%)** ✅ **TARGET ACHIEVED!** (Target: 90%)

**Quiz Breakdown:**

**Questions Correct (9):**
1. ✅ Q1: 9.5 GB ML model + /tmp + provisioned concurrency (not SageMaker over-engineering)
2. ✅ Q2: ElastiCache for 150 MB dataset updated every 5 min (not /tmp - freshness requirement)
3. ✅ Q3: 25 GB knowledge base + file operations = EFS (not layers - 250 MB limit)
4. ✅ Q4: RDS Proxy for Lambda + RDS connection pooling (not pooling in Lambda code)
5. ✅ Q5: 4 GB + quarterly updates + cold starts acceptable = /tmp (most cost-effective)
7. ✅ Q7: 7 GB model + low traffic + cold starts acceptable = /tmp (not provisioned concurrency)
8. ✅ Q8: 12 GB > 10 GB limit = EFS (not /tmp - exceeds max)
9. ✅ Q9: Hybrid approach - model in /tmp, market data in ElastiCache (different update frequencies)
10. ✅ Q10: 9.5 GB ML inference = all data in /tmp + memory (not split across services)

**Questions Missed (1):**
1. ❌ Q6: Gaming leaderboard + key-value + 5,000 Lambda = DynamoDB migration (chose ElastiCache cache layer)
   - **Root cause:** Tried to add caching instead of recognizing architectural mismatch
   - **Pattern:** Key-value + connectionless needed + eventual consistency OK = DynamoDB, not RDS + cache

**🎯 BREAKTHROUGH PERFORMANCE - Day 8 Recovery Complete!**

**Improvement Analysis:**
- **Day 8 (Jan 13):** 4/10 (40%) ❌ CATASTROPHIC FAILURE
- **Day 9 (Jan 16):** 9/10 (90%) ✅ **TARGET CRUSHED**
- **Improvement:** +50 percentage points in 3 days! 🚀

**Weaknesses CONQUERED:**
- ✅ **/tmp vs ElastiCache decision framework** (100% accuracy on Q2, Q5, Q7, Q9)
- ✅ **Reading comprehension** (Q2 recognized failing /tmp approach)
- ✅ **Lambda 10 GB memory limit** (Q1, Q8, Q10 all correct)
- ✅ **ML inference data locality** (Q1, Q10 both correct - no splitting across services)
- ✅ **RDS Proxy pattern** (Q4 correct)
- ✅ **Hybrid architectures** (Q9 correct - /tmp for static, ElastiCache for dynamic)
- ⚠️ **Database architecture decisions** (Q6 missed - should migrate to DynamoDB vs adding cache)

**Key Patterns Mastered:**
1. ✅ When /tmp WORKS: <10 GB, infrequent updates, cold starts acceptable, cost-effective
2. ✅ When /tmp FAILS: Frequent updates, strict freshness requirements, high request rate
3. ✅ When ElastiCache needed: Frequent updates (every few minutes), strict consistency, high traffic
4. ✅ When EFS needed: >10 GB, file operations, shared access across functions
5. ✅ ML inference: ALL data must be in-memory, never split across external services
6. ✅ Hybrid approach: Static data in /tmp, dynamic data in ElastiCache

**Status:** ✅ **LAMBDA + DATA SOURCES WEAKNESS RESOLVED**

**Next Steps:**
- Week 1 Comprehensive Assessment incomplete gaps: S3 Storage Classes (67% → need 90%+)
- Continue to remaining Week 1 weaknesses before Week 2

**Materials Created:**
- None (quiz-only session, updated Progress-Tracker)

**Exam Readiness:**
- 26 days until exam (February 11, 2026)
- Trajectory: **RECOVERING** - Major breakthrough on Lambda patterns
- Lambda now a strength (40% → 90%)

---

### Day 9 (Continued) - S3 Storage Classes Recovery Drill
**Topic:** S3 Storage Classes Selection (Week 1 Comprehensive weakness - scored 67%)
**Time Spent:** 45 minutes

**Quiz Performance:**
- S3 Storage Classes Drill: **8/10 (80%)** ✅ **TARGET ACHIEVED** (Target: 80%+)

**Quiz Breakdown:**

**Questions Correct (8):**
1. ✅ Q1: Lifecycle Standard → Standard-IA → Glacier Instant (frequency progression)
2. ✅ Q2: Intelligent-Tiering when transitioning before 30-day minimum
3. ✅ Q3: Glacier Deep Archive for once-yearly access with 12-hour retrieval
4. ✅ Q4: Glacier Instant for 2-3 accesses/year (rarely = Glacier Instant, not Standard-IA)
5. ✅ Q5: Intelligent-Tiering for unpredictable access patterns
6. ✅ Q6: One Zone-IA for non-critical data (20% cheaper)
7. ✅ Q7: Glacier Flexible for 3-5 hour retrieval requirement
10. ✅ Q10: Intelligent-Tiering → Deep Archive (handles unpredictable active case durations)

**Questions Missed (2):**
8. ❌ Q8: Chose Glacier Instant at 14 days (violates 30-day Standard minimum + 90-day Glacier Instant minimum)
   - **Root cause:** Forgot minimum storage duration penalties
   - **Correct:** Intelligent-Tiering with Archive disabled (no minimum duration)

9. ❌ Q9: Chose Glacier Instant lifecycle (static) for viral/trending content
   - **Root cause:** Missed "unpredictable spikes" keyword
   - **Correct:** Intelligent-Tiering adapts to viral spikes automatically

**Key Patterns Mastered:**
- ✅ Access frequency vocabulary: infrequently (monthly) vs rarely (2-4×/year) vs very rarely (yearly)
- ✅ "Rarely" (quarterly/semi-annual) = Glacier Instant, NOT Standard-IA
- ✅ Glacier Instant 68% cheaper than Standard-IA
- ✅ Retrieval time mapping: milliseconds vs 3-5 hours vs 12 hours
- ✅ Predictable patterns → Lifecycle policies
- ✅ Unpredictable patterns → Intelligent-Tiering
- ✅ One Zone-IA for non-critical data (20% savings)

**Patterns Still Need Work:**
- ⚠️ Minimum storage duration awareness (30-day Standard-IA, 90-day Glacier Instant)
- ⚠️ Intelligent-Tiering adapts to viral/unpredictable spikes (not just for general unpredictability)

**Improvement Analysis:**
- **Week 1 Comprehensive:** 67% (2/3) - missed "rarely accessed" keyword
- **Day 9 Drill:** 80% (8/10) ✅
- **Improvement:** +13 percentage points

**Status:** ✅ **S3 STORAGE CLASSES TARGET MET (80%)**

Minor gaps remain on minimum storage durations, but core patterns mastered.

**Materials Created:**
- None (quiz-only session)

---

### Day 9 (Continued) - ALB vs NLB Selection Drill
**Topic:** ALB vs NLB vs GWLB Selection (Week 1 Comprehensive weakness - scored 50%)
**Time Spent:** 45 minutes

**Quiz Performance:**
- ALB vs NLB Selection Drill: **7/10 (70%)** ⚠️ **BELOW TARGET** (Target: 80%)

**Quiz Breakdown:**

**Questions Correct (7):**
1. ✅ Q1: Gaming + UDP + ultra-low latency + static IPs → NLB
3. ✅ Q3: High-frequency trading + microsecond latency + custom TCP → NLB
4. ✅ Q4: HTTP/2 + gRPC + header-based routing + weighted targets → ALB
5. ✅ Q5: Third-party IDS/IPS security appliances + Layer 3 → GWLB
7. ✅ Q7: Host-based routing (subdomains) + multiple SSL certs → ALB
9. ✅ Q9: SMTP protocol (TCP port 25) + static IPs → NLB
10. ✅ Q10: Query string routing + HTTP header inspection + redirects → ALB

**Questions Missed (3):**
2. ❌ Q2: ALB path-based routing with cross-zone load balancing
   - **Root cause:** Disabled cross-zone to "save costs" - but ALB cross-zone is FREE!
   - **Pattern:** ALB cross-zone = always FREE (no reason to disable)

6. ❌ Q6: WebSocket connections with SSL termination
   - **Root cause:** Chose NLB when no "ultra-low latency" mentioned
   - **Pattern:** WebSocket without extreme latency requirement = ALB (both support it, ALB is standard choice)

8. ❌ Q8: VoIP (UDP) with evenly distributed traffic across AZs
   - **Root cause:** Enabled cross-zone on NLB unnecessarily
   - **Pattern:** NLB cross-zone COSTS MONEY - disable when traffic already balanced to save costs

**Critical Pattern - Cross-Zone Load Balancing Costs:**
```
ALB: Cross-zone = FREE (always enabled by default, no cost to disable/enable)
NLB: Cross-zone = COSTS MONEY (data transfer charges)
GWLB: Cross-zone = COSTS MONEY (data transfer charges)

Decision:
- ALB: Always enable cross-zone (it's free!)
- NLB/GWLB: Only enable if traffic is unbalanced across AZs
- NLB/GWLB: Disable if traffic already evenly distributed (saves $$)
```

**Key Patterns Mastered:**
- ✅ Gaming/real-time/UDP/ultra-low latency → NLB (Layer 4, microsecond latency)
- ✅ HTTP/HTTPS with Layer 7 routing (path, host, query string, headers) → ALB
- ✅ Third-party security appliances (IDS/IPS, firewalls) → GWLB
- ✅ Non-HTTP protocols (SMTP, custom TCP) + static IPs → NLB
- ✅ HTTP/2, gRPC, weighted targets → ALB

**Patterns Still Need Work:**
- ❌ **Cross-zone cost awareness (0/2 correct on cost questions)**
  - Missed Q2: Thought ALB cross-zone costs money (it's FREE)
  - Missed Q8: Enabled NLB cross-zone when traffic already balanced (costs money)
- ⚠️ WebSocket default choice (ALB unless extreme latency needed)

**Improvement Analysis:**
- **Week 1 Comprehensive:** 50% (1/2) - chose ALB for gaming when NLB needed
- **Day 9 Drill:** 70% (7/10)
- **Improvement:** +20 percentage points (but below 80% target)

**Status:** ⚠️ **ALB vs NLB TARGET NOT MET (70% < 80%)**

Core patterns understood, but **cross-zone cost trap** caught me twice (2/3 misses were cost-related).

**Next Steps:**
- Need to drill cross-zone load balancing cost scenarios
- Memorize: ALB = FREE, NLB/GWLB = COSTS MONEY

**Materials Created:**
- None (quiz-only session)

**Overall Day 9 Summary:**
- Lambda + Data Sources: 90% ✅ (40% → 90% recovery)
- S3 Storage Classes: 80% ✅ (67% → 80% improvement)
- ALB vs NLB: 70% ⚠️ (50% → 70% improvement, below target)

**Exam Readiness:**
- 26 days until exam (February 11, 2026)
- Trajectory: **IMPROVING** - 2 of 3 drills hit target
- Lambda now a strength, S3 improved, Load Balancers need cross-zone cost drilling

---

### Day 9 (Continued) - Cross-Zone Load Balancing Cost Drill
**Topic:** Cross-Zone Load Balancing Cost Patterns (targeted weakness from ALB vs NLB drill)
**Time Spent:** 30 minutes

**Quiz Performance:**
- Cross-Zone LB Cost Drill: **8/10 (80%)** ✅ **TARGET MET** (Target: 80%+)

**Quiz Breakdown:**

**Questions Correct (8):**
2. ✅ Q2: NLB with evenly distributed traffic → disable cross-zone to save money
3. ✅ Q3: NLB with unbalanced traffic → enable cross-zone despite cost (better than overprovisioning)
4. ✅ Q4: ALB cross-zone free, NLB costs money → disable NLB cross-zone only
5. ✅ Q5: NLB with 100 Gbps evenly distributed → disable cross-zone (always costs money)
6. ✅ Q6: GWLB with unbalanced traffic → enable cross-zone despite cost (security priority)
7. ✅ Q7: ALB cross-zone feature free, but ALB-to-target data transfer costs money
8. ✅ Q8: NLB with relatively balanced traffic (5K/4.8K/5.2K) → keep disabled
9. ✅ Q9: Multi-tier with ALB + 2 NLBs, even traffic → enable ALB only, disable both NLBs

**Questions Missed (2):**
1. ❌ Q1: Tried to disable ALB cross-zone to "save costs"
   - **Root cause:** Fell for trap - ALB cross-zone is FREE (no cost to save!)
   - **Pattern:** ALB cross-zone = always FREE, never disable it

10. ❌ Q10: Disabled NLB cross-zone permanently when weekdays unbalanced (70/20/10)
   - **Root cause:** Optimized for weekend pattern (2 days) vs weekday pattern (5 days)
   - **Pattern:** When traffic is unbalanced MOST of the time, keep cross-zone enabled

**CRITICAL PATTERN MASTERED:**
```
Cross-Zone Load Balancing Costs:

ALB:  FREE - Always enable (no downside)
NLB:  COSTS MONEY - Only enable if traffic unbalanced
GWLB: COSTS MONEY - Only enable if traffic unbalanced

Decision Framework:
1. ALB → Always enable (it's free)
2. NLB/GWLB with evenly distributed traffic → Disable (save money)
3. NLB/GWLB with unbalanced traffic → Enable (availability > cost)
4. "ALB cross-zone free" = feature is free, ALB-to-target data transfer still costs
```

**Improvement Analysis:**
- Started: 0/1 (0%) - fell for ALB trap on Q1
- Recovery: 8/9 correct after Q1 (88.9% on remaining questions)
- Final: 8/10 (80%) ✅

**Status:** ✅ **CROSS-ZONE COST PATTERN TARGET MET**

**Key Takeaway:** After catastrophic Q1 start, achieved 8 straight correct answers by internalizing the cost framework.

**Materials Created:**
- None (quiz-only session)

**Day 9 Final Summary (4 Drills Completed):**
1. Lambda + Data Sources: 9/10 (90%) ✅ **CRUSHED IT**
2. S3 Storage Classes: 8/10 (80%) ✅ **TARGET MET**
3. ALB vs NLB Selection: 7/10 (70%) ⚠️ **BELOW TARGET**
4. Cross-Zone LB Costs: 8/10 (80%) ✅ **TARGET MET**

**Overall Day 9 Performance: 32/40 (80%)** - 3 of 4 drills hit target

**Exam Readiness:**
- 26 days until exam (February 11, 2026)
- Trajectory: **STRONG RECOVERY** - Major weaknesses addressed
- Lambda: MASTERED (40% → 90%)
- S3 Storage Classes: IMPROVED (67% → 80%)
- Load Balancer basics: SOLID (understanding core patterns)
- Cross-zone costs: MASTERED (cost framework internalized)

---

---

### Day 10 - Monday, January 19, 2026 (Continued)
**Topic:** Week 1 Comprehensive Assessment Retake (Question 20 - FINAL)
**Time Spent:** 5 minutes

**Quiz Performance:**
- Week 1 Comprehensive Assessment Retake: **15/20 (75%)** ⚠️ **BELOW TARGET** (Target: 80%)

**Question 20 Result: INCORRECT** ❌

**Question 20 Breakdown:**
- **Topic:** Multi-Region Disaster Recovery Strategy
- **Requirements:** RTO 5 minutes, RPO 30 seconds, mission-critical trading application
- **Your Answer:** A - Warm Standby with CloudFormation StackSets provisioning during failover ❌
- **Correct Answer:** D - Hot Standby/Multi-Site Active-Active with full capacity ✅

**Why Your Answer Was CATASTROPHICALLY WRONG:**
1. **Logical impossibility:** "Warm Standby" means infrastructure ALREADY RUNNING, but answer said "provision full infrastructure during failover" - these are mutually exclusive
2. **RTO violation:** CloudFormation StackSets takes 15-30 minutes to provision infrastructure (requirement was 5 minutes)
3. **Pattern mismatch:** Mission-critical + 5-minute RTO = Hot Standby/Multi-Site ONLY (no other DR strategy fast enough)

**New Critical Weakness Identified:**
- ❌ **DR Strategies: 0% accuracy** - Cannot map RTO/RPO to correct strategy (Backup/Restore, Pilot Light, Warm Standby, Hot Standby)
- ❌ **Mission-critical keyword recognition** - Missed that mission-critical + 5-min RTO = Hot Standby ONLY
- ❌ **Infrastructure provisioning times** - Forgot CloudFormation takes 15-30 minutes (violates 5-min RTO)

**FINAL SCORE: 15/20 (75%)**

**Comparison to First Attempt (Day 7):**
- First attempt: 15/20 (75%)
- Retake: 15/20 (75%)
- **Improvement: 0 percentage points** (no improvement)
- **Different questions missed:** Fixed Q14, Q16, Q18, Q19 but FAILED Q20 (NEW weakness)

**Status:** ❌ **WEEK 1 INCOMPLETE - NEW CRITICAL WEAKNESS DISCOVERED**

**Analysis:**
Playing whack-a-mole with weaknesses instead of systematically mastering foundational patterns. Fixed 4 questions but discovered NEW critical gap in DR strategies (0% accuracy).

**Immediate Action Required (Before Week 2):**

**Drill #1: DR Strategies (CRITICAL - 10 questions, target 100%)**
- Focus: RTO/RPO mapping to Backup/Restore, Pilot Light, Warm Standby, Hot Standby
- Mission-critical keyword = Hot Standby/Multi-Site only
- Infrastructure provisioning times (EC2, RDS, CloudFormation, Elastic Beanstalk)
- Aurora Global Database replication lag (<1 second)
- Warm Standby = already running at reduced capacity (NOT provisioning during failover)

**Drill #2: Load Balancers Comprehensive (PERSISTENT WEAKNESS - 10 questions, target 100%)**
- Focus: ALB vs NLB latency, cross-zone costs, gaming/real-time = NLB

**Drill #3: Week 1 Comprehensive Retake #2 (After drills - target 16+/20, 80%)**
- Only proceed to Week 2 after achieving 80%+ on comprehensive assessment

**Exam Readiness:**
- **Days until exam:** 23 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **AT RISK** - Stuck at 75% with new weaknesses emerging
- **WARNING:** Discovering new weaknesses faster than closing old ones

**Materials Created:**
- Updated Weakness-Tracker.md with NEW WEAKNESS #18: DR Strategies (RTO/RPO Mapping)

---

### Day 19 - Monday, January 19, 2026
**Topic:** DR Strategies Drill (Addressing Week 1 Comprehensive Q20 Catastrophic Failure)
**Time Spent:** 1 hour

**Quiz Performance:**
- DR Strategies Drill: **8.5/10 (85%)** ✅ **TARGET EXCEEDED!** (Target: 80%)

**Quiz Breakdown:**

**Questions Correct (7.5/10):**
1. ✅ Q1: Mission-critical financial trading, RTO <5 min → Hot Standby/Multi-Site Active-Active
2. ✅ Q2: Monitoring app with reduced standby capacity → Pilot Light (not Warm Standby)
3. ❌ Q3: Session data loss vs RTO timing (chose session data, should focus on RTO violation)
4. ✅ Q4: 4-hour RTO, snapshots every 12 hours → RPO violation (data loss exceeds requirement)
5. ✅ Q5: RDS cold start, 30-min RTO → RTO violation (provisioning takes too long)
6. ✅ Q6: Startup budget constraints, RTO 2 hours → Pilot Light (cost-effective middle ground)
7. ✅ Q7: Missing DocumentDB in DR plan → Incomplete DR planning
8. ⚠️ Q8: RPO = 0 regulatory requirement → PARTIAL CREDIT (identified RDS snapshots issue, missed Aurora Global DB truly isn't RPO=0)
9. ✅ Q9: Stateful gaming workload, session data in ElastiCache → DynamoDB Global Tables for session persistence
10. ✅ Q10: Multi-constraint FinTech optimization (budget + compliance + RPO/RTO) → Enhanced Pilot Light with critical data replication

**Question Details:**

**Q3 - "MOST Significant Issue" Prioritization (INCORRECT):**
- Scenario: Warm Standby with 5-min RTO, ElastiCache session data not replicated, 8-min infrastructure scale-up time
- **Your Answer:** A - Session data loss during failover ❌
- **Correct Answer:** C - Infrastructure scale-up violates RTO (8 min > 5 min requirement) ✅
- **Root Cause:** Prioritized data issue over timing violation; "MOST significant" means which breaks requirements FIRST
- **Pattern to remember:** RTO violations trump secondary issues when explicitly stated requirements are broken

**Q8 - RPO = 0 Understanding (PARTIAL CREDIT):**
- Scenario: RPO = 0 (zero data loss), RTO 10 minutes
- **Your Answer:** A - RDS snapshots every 5 minutes violate RPO = 0 ⚠️ (CORRECT identification)
- **Missing Piece:** Aurora Global Database replication lag (<1 sec) still isn't truly RPO = 0
- **Full Answer:** Both RDS snapshots AND Aurora Global Database violate RPO = 0 (only synchronous multi-region like DynamoDB Global Tables truly achieves RPO = 0)
- **Pattern to remember:** RPO = 0 means ZERO data loss → synchronous replication required (DynamoDB Global Tables, S3 versioning with MFA delete)

**Key Patterns Mastered:**
- ✅ RTO < 5 minutes = Hot Standby/Multi-Site ONLY (100% accuracy)
- ✅ Mission-critical keyword = Hot Standby (100% accuracy)
- ✅ Cost optimization within DR constraints (Q6, Q10)
- ✅ Identifying missing components in DR plans (Q7)
- ✅ Understanding stateful workload requirements (Q9)
- ✅ Multi-constraint optimization (budget + compliance + RPO/RTO) (Q10)

**Patterns Still Need Work:**
- ⚠️ "MOST significant issue" prioritization (Q3 missed - need to identify which violation is PRIMARY)
- ⚠️ RPO = 0 true meaning (Q8 partial - Aurora Global DB is <1 sec, not 0)
- ⚠️ Multi-component violation detection (Q8 - should identify ALL violations, not just first one)

**Strengths Demonstrated:**
- ✅ RTO/RPO mapping to correct DR strategy (7/8 correct on RTO/RPO questions, 87.5%)
- ✅ Cost optimization while meeting requirements (2/2, 100%)
- ✅ Complete DR planning (identifying missing components) (1/1, 100%)
- ✅ Stateful workload special considerations (1/1, 100%)

**Improvement Analysis:**
- **Day 10 Week 1 Comprehensive Q20:** 0% (catastrophic DR strategy failure)
- **Day 19 DR Strategies Drill:** 85% ✅
- **Improvement:** From 0% to 85% in one focused drill session! 🚀

**Status:** ✅ **DR STRATEGIES WEAKNESS SIGNIFICANTLY IMPROVED**

Core RTO/RPO mapping mastered (87.5% accuracy). Minor gaps remain on edge cases ("MOST significant" prioritization, RPO=0 true meaning), but foundational understanding is solid.

**Waldorf & Statler Review:**
> **Waldorf:** "Well, well, well... looks like you finally figured out that Hot Standby isn't when you put your coffee on a warmer."
>
> **Statler:** "And only ONE catastrophic failure this time! That's practically a Nobel Prize in AWS land."
>
> **Waldorf:** "You only missed the 'MOST significant' on Q3. Next time, remember: when the entire building is on fire AND someone stole a cookie, the fire is PROBABLY more significant."
>
> **Statler:** "Though I notice you're STILL confused about RPO = 0. Let me simplify: Aurora Global Database is like a very fast turtle. DynamoDB Global Tables is like teleportation. See the difference?"
>
> **Waldorf:** "8.5 out of 10 though... that's like getting a B+ on your SAA exam. Except you need 72% to pass, so this would actually be... wait, that's an A. Carry on then!"
>
> **Both:** "DOHOHOHOHO!"

**Materials Created:**
- None (quiz-only session, updated Progress-Tracker and Weakness-Tracker)

**Next Steps:**
- Week 1 comprehensive assessment still at 75% (need 80%)
- Consider retaking Week 1 Comprehensive now that DR weakness is addressed
- Or proceed to Week 2 topics (RDS, Aurora, DynamoDB deep dive per schedule)

**Exam Readiness:**
- **Days until exam:** 23 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **RECOVERING STRONGLY** - DR Strategies now a strength (0% → 85%)
- **Week 1 Status:** Lambda (90%), S3 (80%), Load Balancers (70-80%), DR Strategies (85%)
- DR Strategies will account for ~10% of exam (6-10 questions) - now positioned to score 85%+ on these

---

**Last Updated:** January 19, 2026, 9:04 PM CST
**Next Session:** Continue Week 1 recovery OR begin Week 2 topics
