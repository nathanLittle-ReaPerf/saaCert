# AWS SAA-C03 Study Progress Tracker

**Exam Date:** March 2, 2026 at 5:15 PM EST
**Study Period:** January 5 - March 1, 2026 (56 days)
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

### Day 20 - Monday, January 20, 2026 (MLK Day)
**Topic:** Load Balancer Deep Dive Drill (ALB vs NLB vs GWLB)
**Time Spent:** 1.5 hours

**Quiz Performance:**
- Load Balancer Deep Dive Drill: **8/10 (80%)** ✅ **TARGET ACHIEVED!** (Target: 80%)

**Quiz Breakdown:**

**Questions Correct (8):**
1. ✅ Q1: Gaming UDP + Static IP → NLB with Elastic IPs (avoided cross-zone cost trap)
2. ✅ Q2: Microservices path routing → ALB with Lambda targets + Cognito
3. ✅ Q3: Financial trading cross-zone → Disabled (traffic matched instances)
4. ✅ Q4: WebSocket long-lived → Keep ALB (resisted premature optimization)
5. ✅ Q5: Palo Alto security appliances → GWLB with endpoints
6. ✅ Q6: API Gateway vs ALB → Both work, API Gateway has more features
7. ✅ Q8: Hybrid EC2 + Lambda → Single ALB (Route 53 can't do path routing)
8. ✅ Q10: Fortinet HIPAA compliance → GWLB with multi-VPC endpoints

**Questions Missed (2):**
1. ❌ Q7: RTMP streaming → Enabled cross-zone when should disable (no imbalance, latency-sensitive)
2. ❌ Q9: IoT UDP telemetry → **CATASTROPHIC: Chose ALB for UDP traffic (ALB doesn't support UDP!)**

**Strengths Demonstrated:**
- ✅ **GWLB use cases: 100%** (Q5, Q10 both correct - third-party security appliances)
- ✅ **ALB Layer 7 features: 100%** (Path routing, Cognito, Lambda targets, minimize overhead)
- ✅ **Static IP requirements: 100%** (NLB with Elastic IPs for whitelisting)
- ✅ **Avoiding over-engineering: 100%** (Single ALB vs multiple, resist unnecessary migration)
- ✅ **Mixed target types: 100%** (ALB supports both EC2 and Lambda in same load balancer)

**Critical Weaknesses Identified:**

**1. Protocol Knowledge (CATASTROPHIC - Q9 failure):**
- ❌ Chose ALB for UDP traffic (architecturally impossible)
- ❌ Confused "real-time" with "WebSocket" and "UDP"
- ❌ Didn't recognize WebSocket = TCP-based (NOT UDP)
- **Pattern:** ALB = HTTP/HTTPS/gRPC/WebSocket ONLY (all TCP-based), NLB = TCP/UDP/TLS
- **Exam rule:** "UDP mentioned" → NLB is ONLY option (eliminate ALB immediately)

**2. Cross-Zone Load Balancing Decisions (INCONSISTENT - 67% accuracy):**
- Q1: Disabled ✅ CORRECT (gaming, no imbalance mentioned)
- Q3: Disabled ✅ CORRECT (traffic 60%/25%/15% matched instances)
- Q7: Enabled ❌ WRONG (RTMP streaming, no imbalance, latency-sensitive)
- **Pattern inconsistency:** Sometimes correct, sometimes enables unnecessarily
- **Rule:** NLB/GWLB cross-zone DEFAULT = DISABLED (only enable if question shows imbalance)

**Key Patterns Mastered:**
- ✅ Third-party security appliances (Palo Alto, Fortinet) → GWLB (100% accuracy)
- ✅ Path-based routing + HTTP → ALB
- ✅ Cognito authentication → ALB ONLY
- ✅ "Minimize operational overhead" → Single load balancer with routing rules
- ✅ Route 53 cannot inspect URL paths (DNS level, not HTTP level)
- ✅ ALB supports Lambda targets (many people don't know this)
- ✅ Static IP whitelisting → NLB with Elastic IPs

**Patterns Still Need Work:**
- ⚠️ Cross-zone enablement decisions (2/3 correct, 67%)
- ❌ Protocol identification (UDP vs TCP vs HTTP) - CRITICAL GAP

**Improvement Analysis:**
- **Day 9 Load Balancer drill:** 70% (7/10)
- **Day 20 Load Balancer drill:** 80% (8/10)
- **Improvement:** +10 percentage points ✅

**Recovery Actions Taken:**
- Created protocol flashcards (ALB: HTTP/HTTPS/gRPC/WebSocket ONLY, NLB: TCP/UDP/TLS)
- Documented weaknesses in Weakness-Tracker.md

**Status:** ✅ **LOAD BALANCER TARGET ACHIEVED (80%)**

Minor gaps remain (cross-zone decisions, protocol memorization), but core patterns are solid. Load Balancers now a strength.

**Materials Created:**
- Protocol support flashcards (ALB vs NLB vs GWLB)

**Next Steps:**
- Continue Week 1 recovery (retake Week 1 Comprehensive to hit 80%?)
- OR proceed to Week 2/3 topics per schedule (Organizations & IAM, RDS/Aurora, Security)
- Practice protocol flashcards until 100% recall

**Exam Readiness:**
- **Days until exam:** 22 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **STRONG** - Load Balancers now at 80%
- **Week 1 Status:** Lambda (90%), S3 (80%), Load Balancers (80%), DR Strategies (85%)
- Load Balancers will account for ~15% of exam (10-12 questions) - positioned to score 80%+ on these

---

### Day 25-26 - January 25-26, 2026 (Weekend Recovery)
**Topic:** RDS & Aurora Recovery Drills (Post-Day 20 Catastrophic Failure)
**Time Spent:** 3 hours over 2 days

**Context:**
- Day 20 (Jan 20): RDS & Aurora Deep Dive Quiz: **7/15 (47%)** ❌ **CATASTROPHIC FAILURE**
- 5 days away from studying (Jan 20-25)
- Returned Jan 25 to fix critical gaps

**Recovery Materials Created:**
- **RDS-Aurora-Decision-Trees.md** (Jan 25) - Comprehensive decision trees covering:
  - Aurora Serverless v1 vs v2 (scales to zero distinction)
  - Cost optimization patterns (avoid over-engineering)
  - Operational overhead hierarchy (RDS vs EC2)
  - Aurora Global Database vs Cross-Region Read Replicas
  - Aurora Clone/Backtrack recovery patterns
  - ElastiCache limitations (reads only)
  - Compliance & data isolation requirements

**Quiz Performance:**

**Recovery Drill 1 (Jan 25):**
- Score: **6/10 (60%)** ❌ **FAILED** (Target: 90%)
- Improvement from Day 20: +13 percentage points (47% → 60%)

**Questions Missed (4):**
1. ❌ Q1: Aurora Serverless v1 vs v2 for production SaaS (chose v1, should be v2)
2. ❌ Q5: Cost optimization for dev/test databases (chose v2, should be v1 scales to zero)
3. ❌ Q6: ElastiCache for write performance (chose ElastiCache, doesn't help writes!)
4. ❌ Q10: Workload isolation (chose Auto Scaling, should be Custom Endpoints)

**Recovery Drill 2 (Jan 26):**
- Score: **8/10 (80%)** ✅ **TARGET MET** (Standard 80% threshold)
- Improvement from Day 20: +33 percentage points (47% → 80%)
- Missed 90% recovery target by 10 points

**Questions Correct (8):**
1. ✅ Q1: Aurora Serverless v1 for beta testing (scales to zero)
2. ✅ Q2: Aurora Serverless v2 for production e-commerce (instant scaling)
3. ✅ Q3: Aurora Serverless v1 for dev databases (maximum cost savings)
4. ✅ Q4: Aurora Serverless v2 for game launch (avoided v1 production trap)
5. ✅ Q5: Upgrade RDS instance for write performance (not ElastiCache)
6. ✅ Q6: ElastiCache helps reads only, not writes
7. ✅ Q7: Custom Endpoints for workload isolation
8. ✅ Q9: Read Replica for analytics (cost-effective, not separate cluster)

**Questions Missed (2):**
1. ❌ Q8: Chose Serverless v2 migration over Auto Scaling (over-engineering existing Provisioned cluster)
2. ❌ Q10: Chose EC2 Data Guard over RDS Multi-AZ (operational overhead hierarchy - good critical thinking on OS access requirement, but RDS is still correct)

**Weaknesses RESOLVED (100% Accuracy Achieved):**

**1. Aurora Serverless v1 vs v2 (4/4 correct - 100%):**
- ✅ v1 = scales to ZERO = dev/test/infrequent workloads/maximum cost savings
- ✅ v2 = does NOT scale to zero = production/instant scaling/variable traffic
- **Pattern mastered:** "Development databases not used at night/weekends" → v1
- **Pattern mastered:** "Production e-commerce with variable traffic" → v2

**2. ElastiCache Limitations (2/2 correct - 100%):**
- ✅ ElastiCache helps READS ONLY (caching query results)
- ✅ ElastiCache does NOT help write performance
- ✅ Write performance issues → Upgrade instance class (more CPU/IOPS)
- **Pattern mastered:** "Reads improved, writes still slow" → ElastiCache doesn't help writes

**3. Custom Endpoints vs Auto Scaling (1/1 correct - 100%):**
- ✅ Workload isolation = Custom Endpoints (dedicated replicas per workload)
- ✅ More capacity = Auto Scaling (adds replicas dynamically)
- **Pattern mastered:** "Ensure Workload A is NEVER impacted by Workload B" → Custom Endpoints

**4. Cost Optimization Patterns (Mostly Resolved - 75%):**
- ✅ Analytics on production RDS → Read Replica (NOT separate cluster)
- ⚠️ Still over-engineering occasionally (chose migration over simple feature enablement)

**Weakness STILL ACTIVE (50% Accuracy):**

**Operational Overhead Hierarchy (1/2 correct - 50%):**
- ❌ Q10: Still choosing EC2 self-managed when RDS managed service exists
- **Gap:** Defaulting to EC2 solutions instead of managed services
- **Pattern to reinforce:** "LEAST operational overhead" = RDS/Aurora > EC2
- **When to choose EC2:** ONLY when explicit requirement RDS can't meet (Oracle RAC, specific plugins, features not in RDS)

**Key Learnings:**

**Aurora Serverless Decision Tree:**
```
Does workload need to scale to ZERO (0 ACUs)?
├─ YES → Aurora Serverless v1
│   └─ Dev/test, infrequent, maximum cost savings
└─ NO → Aurora Serverless v2
    └─ Production, instant scaling, variable traffic
```

**ElastiCache Reality:**
- What it does: ✅ Cache READ query results, reduce read load
- What it does NOT: ❌ Help writes, reduce write load, improve write performance

**Custom Endpoints vs Auto Scaling:**
- Workload isolation → Custom Endpoints
- More capacity → Auto Scaling

**Cost Optimization Hierarchy:**
1. Use existing resources (Read Replica)
2. Right-size (don't over-provision)
3. Managed services (RDS > EC2)

**Progress Analysis:**
- **Day 20:** 47% (7/15) - CATASTROPHIC
- **Recovery Drill 1:** 60% (6/10) - Still failing
- **Recovery Drill 2:** 80% (8/10) - **TARGET MET!**
- **Improvement:** +33 percentage points in 6 days ✅

**Strengths Demonstrated:**
- ✅ Aurora Serverless v1 vs v2 distinction: 100% accuracy (was 0%)
- ✅ ElastiCache limitations: 100% accuracy (was 0%)
- ✅ Custom Endpoints for workload isolation: 100% accuracy (was 0%)
- ✅ Read Replicas for cost-effective analytics: 100%
- ✅ Aurora Global Database for <1 sec replication: 100%

**Materials Created:**
- RDS-Aurora-Decision-Trees.md (comprehensive decision trees and flashcards)
- Updated Weakness-Tracker.md with resolved weaknesses

**Next Steps:**
- Operational overhead hierarchy needs reinforcement (watch for RDS vs EC2 in future quizzes)
- Ready to proceed to Week 2 topics (RDS covered, move to DynamoDB/ElastiCache deep dive)
- Or continue Week 3 schedule (Organizations, IAM, Security)

**Exam Readiness:**
- **Days until exam:** 16 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **STRONG RECOVERY** - RDS & Aurora now at 80% (was 47%)
- **RDS & Aurora Readiness:** Core patterns mastered, ready for exam-level questions
- RDS/Aurora will account for ~15% of exam (10-12 questions) - positioned to score 80%+ on these

---

---

### Day 26 (Continued) - January 26, 2026 (Evening)
**Topic:** DynamoDB Final Remediation Drill (Second Attempt After 70% First Drill)
**Time Spent:** 1.5 hours

**Context:**
- Day 20 (Jan 20): DynamoDB Deep Dive Quiz: **6/15 (40%)** ❌ **CATASTROPHIC FAILURE**
- Day 25 (Jan 25): First DynamoDB Remediation Drill: **7/10 (70%)** ⚠️ **BELOW TARGET**
- Today: Second remediation drill after reviewing first drill mistakes

**Quiz Performance:**
- DynamoDB Final Remediation Drill: **7/10 (70%)** ❌ **FAILED TARGET** (Target: 90%)

**CRITICAL FINDING: PLATEAUED AT 70% - NO IMPROVEMENT FROM FIRST DRILL**

**Quiz Breakdown:**

**Questions Correct (7):**
1. ✅ Q1: Hash key + range key for sorted query results (composite key pattern)
2. ✅ Q3: Write sharding with random suffix for hot partition (100 WCUs per hash)
3. ✅ Q4: Partition distribution sharding for hot key
4. ✅ Q5: DynamoDB Transactions for ACID guarantees across tables
5. ✅ Q8: Redis Sorted Sets for real-time leaderboards (not DynamoDB GSI)
6. ✅ Q9: Athena with Parquet + partitioning (80-90% cost reduction)
7. ✅ Q10: Redshift for 200+ daily queries (not Athena - frequency threshold)

**Questions Missed (3):**

**1. Q2 - Multiple GSI Cost Calculation (CRITICAL):**
   - ❌ Chose B: DynamoDB optimizes multiple GSI writes
   - ✅ Should be A: 5 WCUs total (1 base + 2 GSI #1 + 2 GSI #2)
   - **Root cause:** Thought DynamoDB shares WCU capacity across GSIs or provides bulk discounts
   - **Pattern:** Each GSI is charged INDEPENDENTLY - no shared capacity, no optimization

**2. Q6 - Database Selection for Key-Value Workload (CATASTROPHIC - THIRD TIME):**
   - ❌ Chose C: Aurora Serverless v2 for session management
   - ✅ Should be B: DynamoDB with TTL
   - **Root cause:** THIRD TIME choosing Aurora/RDS for key-value + flexible schema workload
   - **Pattern:** Key-value primary access + flexible schema + TTL needed = DynamoDB (NOT Aurora)
   - **THIS IS THE EXACT SAME MISTAKE FROM PREVIOUS DRILLS**

**3. Q7 - Complex Analytics Architecture (CRITICAL):**
   - ❌ Chose A: DynamoDB + Streams + Lambda for aggregations
   - ✅ Should be D: DynamoDB → S3 → Redshift for complex analytics
   - **Root cause:** Tried to force DynamoDB + Lambda to handle complex analytics with JOINs
   - **Pattern:** DynamoDB can't do JOINs/GROUP BY/AVG - complex analytics needs data warehouse (Redshift)

**Performance Trajectory:**
- Day 20 Main Quiz: 6/15 (40%) - CATASTROPHIC
- Day 25 First Remediation: 7/10 (70%) - Improvement
- Day 26 Second Remediation: 7/10 (70%) - **PLATEAUED - NO IMPROVEMENT**

**Strengths (100% Accuracy):**
- ✅ Hot partition write sharding (Q3, Q4)
- ✅ DynamoDB Transactions for ACID (Q5)
- ✅ Redis Sorted Sets for leaderboards (Q8)
- ✅ Athena Parquet + partitioning optimization (Q9)
- ✅ Redshift vs Athena frequency threshold (Q10)

**Critical Blind Spots (Persistent Across All Drills):**

**1. Database Selection Pattern (0% Accuracy - THIRD FAILURE):**
   - Keeps choosing Aurora/RDS for key-value workloads with flexible schemas
   - Pattern missed: Key-value primary access + flexible schema = DynamoDB
   - **This is a FUNDAMENTAL gap that won't improve with more drills**

**2. GSI Cost Calculation (0% Accuracy):**
   - Doesn't understand that multiple GSIs charge independently
   - Each GSI = separate WCU cost (no shared capacity or bulk discounts)

**3. Complex Analytics Architecture (0% Accuracy):**
   - Tries to use DynamoDB + Lambda for complex analytics (JOINs, GROUP BY, AVG)
   - Pattern missed: DynamoDB → S3 export → Redshift for data warehouse needs

**Quiz Master Assessment:**
- "You have a fundamental blind spot in database selection"
- "This is your THIRD time missing the key-value + flexible schema → DynamoDB pattern"
- "You're plateaued at 70% - need different learning approach, not more drills"

**Status:** ❌ **DYNAMODB RECOVERY INCOMPLETE - PLATEAUED AT 70%**

**Analysis:**
User has made significant progress on tactical patterns (hot partitions, transactions, leaderboards) but has fundamental pattern recognition gaps that aren't improving:
1. Database selection for access patterns (repeated 3 times)
2. GSI cost calculation with multiple indexes
3. Complex analytics architecture patterns

**Three Options Presented:**
1. One hour focused study on database selection patterns, then move on
2. Accept 70% and move to next topic (ElastiCache & Other Databases - Day 10)
3. Move on without additional drilling (revisit during practice exams)

**Materials Created:**
- None (quiz-only session, all decision trees already exist in RDS-Aurora-Decision-Trees.md and Weakness-Tracker.md)

**Next Steps:**
- **User decision required:** Choose Option 1, 2, or 3
- 16 days until exam (February 11, 2026 at 5:15 PM EST)
- Multiple Week 2/3 topics still need coverage

**Exam Readiness:**
- **Days until exam:** 16 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **AT RISK** - Stuck at 70% on DynamoDB with persistent blind spots
- **DynamoDB Readiness:** Tactical patterns strong (70%), but fundamental database selection gaps
- DynamoDB will account for ~10% of exam (6-10 questions) - currently positioned to score 70% on these

---

---

### Day 27 - January 27, 2026
**Topic:** ElastiCache & Other Databases Deep Dive (Timestream, Neptune, QLDB, DocumentDB, Redis/Memcached)
**Time Spent:** 1 hour

**Quiz Performance:**
- ElastiCache & Other Databases Quiz: **4/10 (40%)** ❌ **CATASTROPHIC FAILURE** (Target: 80%)

**CRITICAL FINDING: OVER-RELIANCE ON DYNAMODB + LAMBDA FOR SPECIALIZED DATABASE USE CASES**

**Quiz Breakdown:**

**Questions Correct (4):**
1. ✅ Q3: QLDB for immutable ledger with cryptographic verification (regulatory compliance)
2. ✅ Q6: Redis read replicas for read-heavy workload (90% CPU on primary)
3. ✅ Q7: Redshift data warehouse for complex JOINs and aggregations (nightly ETL)
4. ✅ Q9: DocumentDB for MongoDB migration with minimal code changes

**Questions Missed (6):**

**1. Q1 - Gaming Leaderboard (CRITICAL - DynamoDB Over-Engineering):**
   - ❌ Chose A: DynamoDB + GSI + Streams + Lambda + S3 for leaderboard
   - ✅ Should be B: ElastiCache Redis with Sorted Sets (ZSET)
   - **Root cause:** Over-engineered with 4 services when Redis ZSET does this natively
   - **Pattern:** Leaderboards = Redis Sorted Sets (ZADD, ZRANGE, ZRANK)

**2. Q2 - RDS Caching for Read-Heavy Workload (COST MISUNDERSTANDING):**
   - ❌ Chose C: Multiple RDS read replicas with Route 53 routing
   - ✅ Should be A: ElastiCache Redis with lazy loading
   - **Root cause:** Chose most expensive option (multiple databases) when question asked for "MOST cost-effective"
   - **Pattern:** 80% identical reads + "reduce database load" + "cost-effective" = Add cache, not more databases

**3. Q4 - IoT Time-Series Data (CRITICAL - TIMESTREAM BLINDSPOT #1):**
   - ❌ Chose A: DynamoDB with composite key (deviceId#timestamp) + Streams for aggregation
   - ✅ Should be B: Amazon Timestream with automatic tiering
   - **Root cause:** Defaulted to DynamoDB when Timestream is purpose-built for time-series
   - **Pattern:** IoT metrics + time-series + aggregations + auto-archive = Timestream

**4. Q5 - Social Network Graph Traversal (CATASTROPHIC - NEPTUNE BLINDSPOT):**
   - ❌ Chose C: Redshift Spectrum querying S3 Parquet files
   - ✅ Should be B: Amazon Neptune graph database with Gremlin queries
   - **Root cause:** Chose batch analytics tool for real-time graph queries
   - **Pattern:** Social network + degrees of separation + mutual friends = Neptune

**5. Q8 - Redis vs Memcached Caching (OVER-ENGINEERING):**
   - ❌ Chose C: Redis Cluster mode with write-through caching
   - ✅ Should be B: Redis with lazy loading + TTL for sessions
   - **Root cause:** Over-engineered with cluster mode for 99% read workload (only 1% writes)
   - **Pattern:** Read-heavy + infrequent writes = Lazy loading, not write-through

**6. Q10 - Package Event Tracking (CRITICAL - TIMESTREAM BLINDSPOT #2):**
   - ❌ Chose A: DynamoDB with packageId partition key + timestamp sort key
   - ✅ Should be B: Amazon Timestream with automatic data tiering
   - **Root cause:** IDENTICAL MISTAKE to Q4 - chose DynamoDB for time-series workload
   - **Pattern:** Event tracking + time-range queries + 10M events/hour + auto-archive = Timestream

**Performance Analysis:**

**Database Selection by Category:**
- ✅ **Ledger/Audit:** 100% (1/1) - QLDB correct
- ✅ **Data Warehouse:** 100% (1/1) - Redshift correct
- ✅ **MongoDB Migration:** 100% (1/1) - DocumentDB correct
- ⚠️ **Redis Scaling:** 50% (1/2) - Got read replicas, missed caching pattern
- ❌ **Time-Series:** 0% (0/2) - Missed Timestream BOTH times
- ❌ **Graph Database:** 0% (0/1) - Missed Neptune
- ❌ **Leaderboards:** 0% (0/1) - Missed Redis Sorted Sets
- ❌ **Caching Patterns:** 0% (0/1) - Over-engineered with cluster mode

**Critical Weaknesses Identified:**

**1. DynamoDB Over-Reliance (0% accuracy on 3 questions):**
   - Q1, Q4, Q10: Defaulted to DynamoDB + Lambda when specialized databases exist
   - Tried to force DynamoDB for: leaderboards, time-series, event tracking
   - **Root cause:** Using familiar service instead of pattern-matching to specialized database

**2. Timestream Blindspot (0% accuracy on 2 identical questions):**
   - Q4 and Q10 were IDENTICAL patterns - both time-series with high ingestion
   - Made SAME mistake twice in single quiz
   - **Root cause:** Don't recognize time-series keywords (events, metrics, timestamps, aggregations over time)

**3. Neptune Blindspot (0% accuracy):**
   - Q5: Chose Redshift Spectrum (batch analytics) for real-time graph queries
   - **Root cause:** Don't recognize graph database keywords (social network, degrees of separation, relationships)

**4. Cost Optimization Misunderstanding (0% accuracy):**
   - Q2: Chose multiple read replicas when question asked for "MOST cost-effective"
   - **Pattern missed:** Single cache cheaper than multiple databases

**5. Over-Engineering Complex Solutions (0% accuracy):**
   - Q1: Used DynamoDB + Streams + Lambda + S3 when Redis ZSET solves natively
   - Q8: Used Cluster mode + write-through for 99% read workload
   - **Root cause:** Not recognizing when simple solutions are sufficient

**Comparison to DynamoDB Quiz (Day 26):**
- DynamoDB Quiz: 70% (plateaued after 2 attempts)
- ElastiCache & Databases Quiz: 40% (worse performance)
- **Regression:** -30 percentage points

**Common Thread Across Both Quizzes:**
- Defaulting to DynamoDB + Lambda for specialized use cases
- Not pattern-matching keywords to purpose-built services
- Over-engineering solutions with unnecessary complexity

**Status:** ❌ **CATASTROPHIC FAILURE - CRITICAL PATTERN RECOGNITION GAPS**

**Exam Impact:**
- Specialized databases account for ~20% of SAA-C03 exam (13-16 questions)
- Current trajectory: 40% accuracy = 5-6 correct out of 13-16 questions
- **Need 80% accuracy = 10-13 correct questions**
- **Gap: 5-7 more questions need mastery**

**Recovery Required:**
1. **Specialized Database Decision Tree** - When to use each database service
2. **Keyword Pattern Matching** - Map exam keywords to correct services:
   - Time-series, IoT metrics, events with timestamps → Timestream
   - Social network, graph, degrees of separation, relationships → Neptune
   - Leaderboard, ranking, top N, range queries by score → Redis Sorted Sets
   - Immutable, audit log, cryptographic verification → QLDB
   - MongoDB migration, document database → DocumentDB
3. **Stop Over-Engineering** - Use simple solutions when they exist
4. **Cost Optimization Logic** - Single cache > multiple databases

**Materials Created:**
- None (quiz-only session, patterns documented here)

**Next Steps:**
- **DO NOT PROCEED TO NEXT TOPIC** until specialized database patterns mastered
- Drill #1: Timestream vs DynamoDB decision tree (10 questions, target 100%)
- Drill #2: Neptune vs other databases (10 questions, target 100%)
- Drill #3: ElastiCache patterns (lazy loading, write-through, read replicas) (10 questions, target 100%)
- Only proceed after hitting 80%+ on comprehensive specialized database quiz

**Exam Readiness:**
- **Days until exam:** 15 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **CRITICAL RISK** - Two consecutive failures (DynamoDB 70%, Specialized Databases 40%)
- **WARNING:** Pattern recognition gaps affecting 20-30% of exam content
- **Immediate action required** to avoid exam failure

---

**Last Updated:** January 27, 2026, 1:15 AM CST
**Next Session:** Specialized Database Recovery Drills (Timestream, Neptune, ElastiCache patterns)

---

### Day 27 (Continued) - January 27, 2026 (Evening Session)
**Topic:** Timestream vs DynamoDB Recovery Drill
**Time Spent:** 1.5 hours

**Context:**
- Day 27 Morning: ElastiCache & Specialized Databases Quiz: **4/10 (40%)** ❌ CATASTROPHIC FAILURE
- **Timestream Blind Spot Identified:** 0/2 (0%) on Timestream questions in morning quiz
- Evening: Targeted drill to fix Timestream pattern recognition

**Quiz Performance:**
- Timestream vs DynamoDB Drill: **9/10 (90%)** ✅ **TARGET EXCEEDED** (Target: 80%)

**BREAKTHROUGH: 0% → 90% IN ONE DRILL SESSION**

**Quiz Breakdown:**

**Questions Correct (9):**
1. ✅ Q1: Industrial sensors (50K sensors, 10M writes/min, hourly averages, 90-day tiering) → Timestream
2. ✅ Q2: User clickstream (95% queries by specific user_id, sub-10ms latency) → DynamoDB
3. ✅ Q3: Stock market tick data (10K stocks, 100ms updates, moving averages, 7-year retention) → Timestream
4. ✅ Q4: Social media posts (user timeline, sorted by recent, 10x spikes) → DynamoDB
5. ✅ Q5: Smart building sensors (5K sensors, average temp per floor, monthly trends) → Timestream
6. ✅ Q6: Gaming sessions (player's last 10 sessions, sub-10ms latency) → DynamoDB
8. ✅ Q8: Weather stations (10K stations, current conditions API + 30-day analytics, sub-second OK) → Timestream only
9. ✅ Q9: Network flow logs (500K events/sec, time-windowed alerts, 90-day patterns) → Timestream
10. ✅ Q10: Ride-sharing trips (<100ms for driver/rider lookups + nightly analytics) → DynamoDB + Timestream hybrid

**Question Missed (1):**

**Q7 - Package Tracking Hybrid Architecture:**
   - ❌ Chose B: Timestream only
   - ✅ Should be A: DynamoDB + Timestream hybrid
   - **Root cause:** Saw "analytics" and defaulted to single Timestream, missed dual-workload requirement
   - **Pattern missed:** Real-time customer portal (fast lookups) + analytics dashboards = hybrid architecture
   - **Learning:** When BOTH operational (<100ms) AND analytical workloads exist at scale → hybrid

**Performance Analysis:**

**Single-Database Decisions: 8/8 (100% accuracy):**
- ✅ Timestream-only: 5/5 (Q1, Q3, Q5, Q8, Q9)
- ✅ DynamoDB-only: 3/3 (Q2, Q4, Q6)

**Hybrid Architecture Decisions: 1/2 (50% accuracy):**
- ❌ Q7: Package tracking (missed)
- ✅ Q10: Ride-sharing (correct)

**Pattern Recognition Mastered:**

**1. Timestream Indicators (100% accuracy on pure Timestream questions):**
- ✅ High-velocity ingestion (IoT sensors, tick data, logs)
- ✅ Time-series aggregations (avg, moving averages, trends)
- ✅ Time-range queries ("over past 30 days," "hourly averages")
- ✅ Automatic data tiering requirements
- ✅ Keywords: "analyze," "trends," "patterns," "aggregations over time"

**2. DynamoDB Indicators (100% accuracy on pure DynamoDB questions):**
- ✅ Fast lookups by partition key (user_id, player_id, package_id)
- ✅ Timeline/feed queries (sorted by timestamp)
- ✅ Sub-10ms latency requirements
- ✅ No aggregations needed (just retrieve raw data)
- ✅ Keywords: "retrieve," "fetch," "get," "show last N," "display timeline"

**3. Latency-Based Decision Making (100% accuracy):**
- ✅ Sub-10ms → DynamoDB required (Q2, Q6, Q10)
- ✅ Sub-second OK → Timestream memory store sufficient (Q8)
- ✅ Seconds OK → Pure analytics workload (Q1, Q3, Q5, Q9)

**4. Timestamp Trap Mastery (100% accuracy):**
- ✅ Q2, Q4, Q6: Data has timestamps but access pattern is key-value → DynamoDB
- ✅ Correctly identified: Timestamps ≠ automatic Timestream (depends on query pattern!)

**Critical Learnings:**

**Timestream vs DynamoDB Decision Tree:**
```
Does the scenario have time-series analytics needs?
├─ NO → DynamoDB (or other service, not Timestream)
└─ YES → Does it ALSO have operational queries requiring <100ms?
    ├─ NO → Timestream alone (if latency can be sub-second)
    └─ YES → DynamoDB + Timestream hybrid
```

**Hybrid Architecture Pattern (Emerging - 50% accuracy):**
- When to use hybrid: BOTH operational AND analytical workloads at scale
- DynamoDB Streams bridges them automatically (zero operational overhead)
- Red flags: "Customer portal" + "executive dashboards"
- Red flags: "Real-time API" + "trend analysis"
- Red flags: Dual latency requirements (fast + slow)

**Comparison to Morning Quiz:**
- **Morning (Day 27):** Timestream accuracy: 0/2 (0%) - Complete blind spot
- **Evening (Drill #1):** Timestream accuracy: 5/5 (100%) - Pattern mastered
- **Overall improvement:** 0% → 90% in single session

**Status:** ✅ **TIMESTREAM PATTERN MASTERED - DRILL #1 COMPLETE**

**Materials Created:**
- None (all patterns documented in Progress-Tracker and captured during quiz)

**Next Steps:**
- ✅ **Drill #1 Complete:** Timestream vs DynamoDB (90% achieved)
- ⏭️ **Drill #2 (Tomorrow AM):** Neptune vs Other Databases (10 questions, target 100%)
  - Focus: Graph database keywords (social networks, degrees of separation, relationships)
  - Address Day 27 Q5 failure (chose Redshift for social network queries)
- ⏭️ **Drill #3 (Tomorrow):** ElastiCache Patterns (10 questions, target 100%)
  - Focus: Redis Sorted Sets for leaderboards, lazy loading vs write-through, read replicas
  - Address Day 27 Q1, Q2, Q8 failures

**Exam Readiness:**
- **Days until exam:** 15 days (February 11, 2026 at 5:15 PM EST)
- **Current trajectory:** **STRONG RECOVERY IN PROGRESS** ✅
- **Timestream vs DynamoDB:** EXAM READY (90% accuracy)
- **Specialized databases:** 2 more drills needed (Neptune, ElastiCache)
- **Confidence:** Rising - targeted drilling proving highly effective

---

**Last Updated:** January 27, 2026, 9:22 PM CST
**Next Session (Tomorrow AM):** Neptune vs Other Databases Drill (graph database pattern recognition)

---

## Day 28 - January 28, 2026 (Evening)

**Topic:** Neptune vs Other Databases Recovery Drill
**Time:** 1 hour
**Status:** ❌ **BELOW TARGET - ADDITIONAL DRILLING REQUIRED**

### Quiz Results

**Neptune vs Other Databases Drill: 60% (6/10)** ❌ **BELOW TARGET**
- **Target:** 80% (8/10)
- **Result:** 60% (6/10)
- **Status:** 🚨 **CRITICAL WEAKNESS PERSISTS**

**Performance Breakdown:**
- ✅ Correct Neptune Identification: 5/5 (100%)
  - Social networks, fraud detection, threat intelligence, family trees, infrastructure dependencies
- ❌ Failed "When NOT to Use Neptune": 4/5 (80% failure rate)
  - Over-applying Neptune to scale problems (50M user recommendations)
  - Confusing aggregation with traversal (product analytics)
  - Missing geospatial vs graph routing distinction (delivery tracking)
  - REPEAT: Choosing Redshift for real-time queries (clinical trials)

### New Critical Weaknesses Identified

**🔴 WEAKNESS #29: Neptune Scale Limitations - Real-Time Traversal vs Pre-Computed Results**
- Chose Neptune for 50M user recommendation engine with 200ms SLA
- Correct: DynamoDB with pre-computed similarity scores
- Pattern: Neptune works <1M users; 10M+ users with <200ms = pre-compute + DynamoDB

**🔴 WEAKNESS #30: Confusing Graph Traversal with Batch Analytics**
- Chose Neptune for product analytics on 10TB historical data in S3
- Correct: Athena to query S3 directly with SQL
- Pattern: "Products bought together" = COUNT/GROUP BY aggregation, NOT graph traversal

**🔴 WEAKNESS #31: Geospatial Routing vs Graph Database Routing**
- Chose Neptune for logistics delivery tracking ("shortest delivery route")
- Correct: DynamoDB for entity tracking + Amazon Location Service for routing
- Pattern: Physical routing (GPS/maps) ≠ relationship routing (graph DB)

**🔴 WEAKNESS #32: Redshift for Real-Time Operational Queries (REPEAT MISTAKE!)**
- Chose Redshift for clinical trial patient matching with 2-second SLA
- Correct: Neptune for complex relationship pattern matching
- Pattern: Redshift = OLAP (batch analytics), Neptune = OLTP (real-time operational)

### Pattern Analysis

**Core Issue:** User correctly identifies WHEN Neptune is right (100% accuracy), but struggles with WHEN Neptune is WRONG (80% failure rate).

**Failed Distinctions:**
1. Scale limits (Neptune vs pre-computed DynamoDB)
2. Aggregation vs traversal (Athena vs Neptune)
3. Geospatial vs graph routing (Location Service vs Neptune)
4. OLAP vs OLTP (Redshift vs Neptune) - REPEAT from Day 27

### Materials Created

- ✅ **Day-28-Neptune-Weaknesses.md**: Detailed failure analysis with decision trees
- ✅ **Master-Decision-Trees-All-Services.md**: Consolidated decision trees for all AWS services
  - Database selection master tree
  - Neptune vs other databases (with scale limits, routing types, traversal vs analytics)
  - DynamoDB access patterns
  - RDS & Aurora decision trees
  - Specialized databases (Timestream, QLDB, DocumentDB)
  - Caching patterns (ElastiCache)
  - OLAP vs OLTP critical distinctions
  - Quick reference matrix with exam keywords

### Recovery Actions Required

**Immediate (Before Next Quiz):**
1. Create Neptune Decision Tree flashcard (graph traversal vs aggregation vs geospatial)
2. Memorize scale limits: Neptune works <1M users for real-time recommendations, DynamoDB for 10M+
3. Create "Redshift RED FLAGS" flashcard (real-time, operational, <5sec SLA = NOT Redshift)
4. Review Cost-Analysis-Reference-Tables.md for Neptune vs Athena vs DynamoDB cost models

**Drilling Required:**
- "When NOT to Use Neptune" quiz (10 questions, target 90%+)
- "OLAP vs OLTP" quiz (10 questions, target 100%)
- "Recommendation Engine Architecture" quiz (focus on scale + latency requirements)

**Status:** 🚨 **ACTIVE WEAKNESS - REQUIRES IMMEDIATE REMEDIATION**

---

**Last Updated:** January 28, 2026, 5:39 PM CST
**Next Session:** Day 28 (Feb 1) - FIRST FULL PRACTICE EXAM (65 questions, 130 minutes)

---

## Day 29 - February 1, 2026 (Evening)

**Topic:** First Full SAA-C03 Practice Exam (65 questions)
**Time:** 2.5 hours
**Status:** ❌ **CATASTROPHIC FAILURE - EMERGENCY RECOVERY REQUIRED**

### Practice Exam 1 Results

**Score: 36/65 (55.4%)** ❌ **FAIL**
- **Passing Score:** 47/65 (72%)
- **Gap:** -11 questions (17 percentage points)
- **Days Until Exam:** 10 days (February 11, 2026 at 5:15 PM EST)
- **Current Pass Probability:** ~5% (would fail if exam were today)

### Performance by Domain

| Domain | Score | % | Status |
|--------|-------|---|--------|
| **Design Secure Architectures** | 4/5 | 80% | ✅ Strength |
| **Design Resilient Architectures** | 5/5 | 100% | ✅✅ Mastery |
| **Design High-Performing Architectures** | 6/10 | 60% | ⚠️ Below Target |
| **Design Cost-Optimized Architectures** | 1/5 | 20% | ❌❌ Catastrophic |
| **Multi-Service Integration** | 2/5 | 40% | ⚠️ Weakness |

### Critical Findings

**Disaster Zone:** Cost Optimization (20%)
- Failed 4 out of 5 cost optimization questions
- Fundamental gaps in S3 lifecycle policies, rightsizing, serverless cost models

**Repeated Mistakes:**
- **io2 Block Express:** Missed TWICE (Q4 and Q44) - doesn't know 256K IOPS capability
- **Redshift OLAP confusion:** Still choosing Redshift for real-time queries (from Day 27-28 weakness)

**Score Collapse:**
- Questions 40-50: 45%
- Questions 51-65: 27% (final stretch collapse)

### Top 10 Critical Weaknesses (New)

1. **S3 Lifecycle Minimum Storage Durations** (Q41, Q59)
   - Can't skip tiers: Standard-IA (30d) → Glacier Flexible (90d) → Deep Archive (180d)
   
2. **io2 Block Express for Extreme IOPS** (Q4, Q44 - FAILED TWICE!)
   - Doesn't know io2 Block Express supports 256K IOPS (vs io2 64K max)
   
3. **Cost Optimization Hierarchy** (Q65)
   - Chose scheduling over rightsizing + Reserved Instance
   - Rule: Rightsize > Commit > Schedule
   
4. **Serverless for Sporadic Workloads** (Q62)
   - Chose always-on EMR for 20% utilization Spark jobs
   - Correct: AWS Glue (serverless, pay-per-job)
   
5. **DynamoDB On-Demand vs Provisioned** (Q47)
   - Chose provisioned auto-scaling for "100x spike in seconds"
   - Auto-scaling takes minutes; On-Demand is instant
   
6. **Glacier Instant vs Flexible Retrieval** (Q41)
   - Thought Glacier Instant was cheaper (it's more expensive!)
   - Cost: Standard > Standard-IA > Glacier Instant > Glacier Flexible > Deep Archive
   
7. **FSx for Lustre Sub-Millisecond Latency** (Q51)
   - Chose EFS Max I/O (millisecond) for "sub-millisecond" requirement
   - FSx Lustre provides sub-millisecond, EFS doesn't
   
8. **Database Engine Compatibility** (Q42)
   - Tried to migrate Oracle → Aurora PostgreSQL for "least overhead"
   - Stored procedures aren't compatible; requires massive refactoring
   
9. **Over-Engineering** (Q56)
   - Chose ECS Fargate containerization for WordPress
   - Elastic Beanstalk = "least operational overhead" (upload ZIP, done)
   
10. **DynamoDB Single-Table Multi-Tenant** (Q58)
    - Chose composite partition key with manual sharding
    - Simple customer ID partition key is sufficient

### Materials Created

- ✅ **Day-29-Practice-Exam-1-Results.md**: Complete exam analysis with recovery plan

### Emergency Recovery Plan (10 Days)

**Phase 1: Cost Optimization (Days 1-3 - Feb 2-4)**
- Target: Raise from 20% → 90%+
- S3 lifecycle policies with minimum durations
- Cost optimization hierarchy (rightsize > commit > schedule)
- Serverless cost models
- Daily drills: 20 questions, target 90%+

**Phase 2: Service Limits & Performance (Days 4-5 - Feb 5-6)**
- Target: Raise from 60% → 85%+
- IOPS hierarchy: gp3 (16K) → io2 (64K) → io2 Block Express (256K)
- FSx Lustre vs EFS latency
- DynamoDB capacity modes
- Aurora Serverless v2 scaling

**Phase 3: Pattern Recognition (Days 6-7 - Feb 7-8)**
- "Least operational overhead" patterns
- Database migration vs modernization
- DynamoDB single-table design
- Practice Exam 2 (target: 50/65 = 77%)

**Phase 4: Final Prep (Days 8-10 - Feb 9-11)**
- Review all weaknesses
- Practice Exam 3 (target: 52/65 = 80%)
- Rest before exam day

### Risk Assessment

**Current Status:** 🔴 **CRITICAL - HIGH PROBABILITY OF FAILURE**

**Reasons:**
1. Cost optimization at 20% (needs 70%+)
2. Repeated mistakes (io2 Block Express twice)
3. Final stretch collapse (27% in Q51-65)
4. 17-point gap to passing

**Mitigation:** URGENT drilling on cost optimization starting tomorrow

---

**Last Updated:** February 1, 2026, 6:14 PM CST
**Next Session (Tomorrow):** Cost Optimization Drill (20 questions, target 90%+)
**Exam Countdown:** 10 DAYS REMAINING

---

## Day 30 - February 2, 2026 (Evening)

**Topic:** Cost Optimization Recovery Drill (Emergency Recovery - Phase 1)
**Time:** 2.5 hours
**Status:** ⚠️ **IMPROVEMENT BUT BELOW TARGET**

### Quiz Results

**Cost Optimization Drill: 65% (13/20)** ⚠️ **BELOW 90% TARGET**
- **Target:** 90% (18/20)
- **Result:** 65% (13/20)
- **Improvement from Practice Exam 1:** +45 percentage points (20% → 65%)
- **Status:** 🟡 **SIGNIFICANT IMPROVEMENT BUT CRITICAL GAPS REMAIN**

**Performance Breakdown by Topic:**

✅ **Strengths (100% accuracy):**
- S3 Intelligent-Tiering use cases (Q6)
- Elastic Beanstalk vs over-engineering (Q5, Q12)
- DynamoDB capacity modes (Q3)
- Load balancer cross-zone pricing (Q11)
- CloudFront cache optimization (Q15)
- Aurora Serverless v2 for variable traffic (Q16)
- DynamoDB Global Tables over-engineering (Q17)
- EKS rightsizing + Savings Plans (Q19)
- S3 early deletion penalty avoidance (Q20)

❌ **Critical Failures:**
- Cost optimization hierarchy violations (Q2, Q14): Still choosing Commit before Rightsize
- S3 Glacier Instant vs Flexible confusion (Q10, Q18): **CHRONIC WEAKNESS** - failed 4 times total (Practice Exam Q41, Q59 + Drill Q10, Q18)
- Serverless for sporadic workloads (Q7): Chose Auto Scaling over AWS Glue (2.4% utilization)
- EBS volume IOPS over-provisioning (Q4): **REPEAT from Practice Exam Q4, Q44**
- S3 lifecycle transition mechanics (Q8): Chose manual delete/re-upload over lifecycle
- NAT Gateway vs alternatives (Q9): Partial credit (correct but incomplete reasoning)
- S3 lifecycle policies (Q1): Correct

### New Critical Weaknesses Identified

**🔴 CATASTROPHIC WEAKNESS #33: S3 Glacier Instant Access Pricing Confusion (REPEAT x4)**
- **Pattern:** Continuously choosing Glacier Instant for "occasional access" scenarios
- **Misconception:** Believes Glacier Instant is cheaper than Standard-IA for infrequent access
- **Reality:**
  - Glacier Instant: $0.004/GB-month storage + $0.03/GB retrieval = $0.034-0.064/GB total
  - Standard-IA: $0.0125/GB-month storage + $0.01/GB retrieval = $0.0225-0.0325/GB total
  - Glacier Flexible: $0.0036/GB-month storage + $0.01/GB retrieval (cheapest for rare access)
- **Failed Questions:** Practice Exam 1 Q41, Q59; Cost Drill Q10, Q18 (4 failures total)
- **Impact:** Would waste thousands monthly by using wrong storage class

**🔴 CRITICAL WEAKNESS #34: Cost Optimization Hierarchy Violations (REPEAT)**
- **Pattern:** Committing to Reserved Instances/Savings Plans BEFORE rightsizing workloads
- **Rule:** Rightsize > Commit > Schedule (ALWAYS in this order)
- **Failed Questions:** Cost Drill Q2 (chose scheduling over rightsizing), Q14 (chose Savings Plan before optimizing Lambda code)
- **Examples:**
  - Q2: Should rightsize 25% CPU instances BEFORE buying RIs (chose scheduling instead)
  - Q14: Should optimize Lambda from 2min → 1min (50% savings) BEFORE Savings Plan (17% savings)
- **Impact:** Locking in commitment to wasteful infrastructure

**🔴 CRITICAL WEAKNESS #35: Serverless Economics for Low-Utilization Workloads**
- **Pattern:** Choosing persistent infrastructure for <30% utilization workloads
- **Failed Question:** Q7 - Spark jobs running 4 hours/week (2.4% utilization)
- **Wrong Answer:** Auto Scaling EMR (still paying for infrastructure 24/7)
- **Correct Answer:** AWS Glue serverless (pay only for 4 hours/week runtime)
- **Rule:** <30% utilization → Serverless (Glue, Lambda, Fargate Spot)
- **Impact:** $14,000/month vs $1,500/month (90% cost waste)

**🔴 WEAKNESS #36: EBS Volume IOPS Over-Provisioning (REPEAT from Practice Exam)**
- **Pattern:** Choosing most expensive volume type when cheaper options meet requirements
- **Failed Question:** Q4 - 200K IOPS requirement
- **Wrong Answer:** io2 Block Express with 256K IOPS (over-provisioned by 28%)
- **Correct Answer:** Regular io2 with 200K IOPS (cheaper, exact requirement)
- **Misconception:** Believes io2 maxes at 64K IOPS (it can reach 256K for volumes ≥16 TiB)
- **Impact:** $3,640/month waste on unnecessary over-provisioning
- **Note:** Same mistake on Practice Exam 1 Q4 and Q44 (3 failures total)

**🔴 WEAKNESS #37: S3 Lifecycle Transition Mechanics**
- **Failed Question:** Q8 - Glacier Flexible to Deep Archive transition with early deletion penalties
- **Wrong Answer:** Delete and re-upload to avoid penalties (costs $2,048 in retrieval fees)
- **Correct Answer:** Lifecycle transition (costs $0, penalties same either way)
- **Misconception:** Believes lifecycle transitions trigger retrieval costs (they don't)
- **Impact:** Manual operations cost 100x more than automated lifecycle policies

### Pattern Analysis

**Strengths Emerging:**
- Architectural over-engineering detection (Elastic Beanstalk, DynamoDB Global Tables)
- Advanced cost optimization (CloudFront caching, S3 early deletion buffers, Aurora Serverless v2)
- Service selection for managed vs DIY (NAT Gateway vs Instance, VPC Endpoints)

**Chronic Weaknesses (4+ Failures):**
1. **S3 Glacier Instant Access confusion** - 4 failures across 2 exams
2. **EBS IOPS over-provisioning** - 3 failures (Practice Exam Q4, Q44, Drill Q4)
3. **Cost optimization hierarchy** - 2+ failures (Practice Exam Q65, Drill Q2, Q14)

**Root Cause:** Strong architectural thinking but weak on pricing details and optimization sequences. Can identify complex waste but struggles with basic cost hierarchies.

### Materials Created

- ✅ Cost Optimization Drill complete (13/20 questions documented)
- 📋 Weakness patterns identified for flashcard creation
- 📋 Ready for Weakness-Tracker.md update

### Recovery Actions Required

**IMMEDIATE (Tonight - Feb 2):**
1. ✅ Update Weakness-Tracker.md with Weaknesses #33-37
2. 📋 Update Cost-Analysis-Reference-Tables.md with S3 Glacier pricing comparison
3. 📋 Create S3 Glacier pricing flashcards (Instant vs Flexible vs Standard-IA)
4. 📋 Create cost optimization hierarchy flashcard ("Rightsize → Commit → Schedule")

**TOMORROW (Feb 3):**
1. 📋 Drill S3 storage class pricing (20 questions, target 90%+)
2. 📋 Drill cost optimization hierarchy (20 questions, target 100%)
3. 📋 Retake Cost Optimization quiz (target 18/20 = 90%+)

**THIS WEEK (Feb 3-4):**
1. 📋 EBS volume types and IOPS limits drill
2. 📋 Serverless economics drill (Lambda, Glue, Fargate cost models)
3. 📋 Complete Phase 1 of Emergency Recovery Plan

### Risk Assessment Update

**Previous Status (Feb 1):** 🔴 CRITICAL - Cost optimization at 20%
**Current Status (Feb 2):** 🟡 **MODERATE RISK - Improved to 65% but gaps remain**

**Positive Developments:**
- ✅ 45-point improvement in cost optimization (20% → 65%)
- ✅ Correctly identified architectural over-engineering
- ✅ Strong performance on advanced patterns (Aurora Serverless, early deletion penalties)
- ✅ No failures on "least operational overhead" patterns

**Remaining Risks:**
- ❌ Still 25 points below 90% target
- ❌ Chronic S3 Glacier pricing confusion (4 failures)
- ❌ Cost optimization hierarchy not internalized
- ❌ EBS IOPS over-provisioning repeating (3 failures)
- ❌ 9 days to exam - limited time for mastery

**Probability of Passing (72%+ on Feb 11):**
- **Previous:** ~5% (based on 55% Practice Exam 1)
- **Current:** ~40-50% (based on improvement trajectory)
- **Target:** 80%+ probability requires hitting 80-85% on Practice Exam 2

**Path to Success:**
1. Eliminate Glacier pricing confusion with flashcard drilling (2-3 days)
2. Internalize cost optimization hierarchy through repetition (1-2 days)
3. Memorize EBS IOPS limits to stop over-provisioning (1 day)
4. Practice Exam 2 on Feb 7-8 (target: 50/65 = 77%)
5. Final review and Practice Exam 3 on Feb 9-10 (target: 52/65 = 80%)

---

**Last Updated:** February 2, 2026, 9:47 PM CST
**Next Session (Tomorrow):** S3 Storage Class Pricing Drill + Cost Hierarchy Drill (40 questions total, target 90%+)
**Exam Countdown:** 9 DAYS REMAINING


### Day 31 - Wednesday, February 12, 2026
**Topic:** 20-Question Weakness Assessment (Claude Code Session)
**Time Spent:** 2 hours

**Quiz Performance:**
- **Overall Score: 10/20 (50%)** ⚠️ **CRITICAL - Exposed Major Gaps**

**Performance by Category:**
1. **S3 Storage Classes & Pricing: 4/5 (80%)** ✅ Strong
   - ✅ Q1: Glacier Instant for millisecond + infrequent access
   - ✅ Q2: Minimum object size trap (128 KB for Standard-IA)
   - ✅ Q3: Early deletion penalties (Standard-IA 30-day minimum)
   - ❌ Q4: Chose Glacier Instant migration over Standard retrieval (over-engineered)
   - ✅ Q5: Double-billing during storage class transitions

2. **Cost Optimization Patterns: 2/5 (40%)** 🔴 **CRITICAL WEAKNESS**
   - ✅ Q6: Rightsize before committing to RIs ✅ **BREAKTHROUGH!**
   - ❌ Q7: Tried to sell RIs instead of using auto-apply feature
   - ❌ Q8: Instance Scheduler on production web app (would cause outages)
   - ❌ Q9: Pure Spot without On-Demand fallback (reliability risk)
   - ✅ Q10: Rightsize + RDS RI + VPC endpoints (correct prioritization)

3. **EBS Volume Types & IOPS: 2/5 (40%)** 🔴 **CRITICAL WEAKNESS**
   - ❌ Q11: Dismissed io2 Block Express (thought 64K IOPS max, actually 256K)
   - ❌ Q12: Chose gp3 when peak IOPS (18K) exceeded gp3's 16K limit
   - ✅ Q13: Instance store for temporary video processing ✅ **CORRECT!**
   - ❌ Q14: Oversized gp2 storage instead of gp3 with provisioned IOPS
   - ❌ Q15: Thought stopping instances saves EBS costs (it doesn't - EBS bills 24/7)

4. **DR Strategies & Backup: 3/5 (60%)** 🟡 Acceptable
   - ✅ Q16: Pilot Light for 1-hour RTO / 5-minute RPO
   - ❌ Q17: Object Lock for "accidental deletion" (versioning alone sufficient)
   - ✅ Q18: AWS Backup for centralized management + audit trail
   - ❌ Q19: RDS export to S3 Glacier for "backups" (it's for analytics, not backup)
   - ✅ Q20: Aurora Global Database managed failover + Route 53

**Critical Weaknesses Identified (MUST FIX - 18 Days to Exam):**

🚨 **WEAKNESS #38: io2 Block Express IOPS Limits**
- Misconception: io2 maxes at 64,000 IOPS
- Reality: io2 = 64K max, io2 Block Express = 256K max (4x higher!)
- Impact: Failed Q11 (dismissed io2 BE for 100K IOPS requirement)

🚨 **WEAKNESS #39: EBS Billing for Stopped Instances**
- Misconception: Stopping instances saves storage costs
- Reality: EBS bills 24/7 whether instance is running or stopped
- Impact: Failed Q15 (thought Instance Scheduler saves storage costs)

🚨 **WEAKNESS #40: Cost Optimization Hierarchy**
- The Rule: RIGHTSIZE → COMMIT → SCHEDULE → OPTIMIZE
- Failures: Q7 (sell RIs), Q8 (Instance Scheduler on prod), Q9 (pure Spot)
- Impact: 3/5 failures in cost optimization category

🚨 **WEAKNESS #41: gp3 16,000 IOPS Limit**
- Mistake: Chose gp3 for 18K IOPS peak
- Reality: gp3 maxes at 16,000 IOPS (hard limit)
- Impact: Failed Q12 (would cause production performance issues)

🚨 **WEAKNESS #42: Object Lock vs Versioning**
- Mistake: Object Lock for "accidental deletion"
- Reality: Versioning = deletion protection, Object Lock = WORM/immutable
- Impact: Failed Q17 (over-engineered compliance solution)

🚨 **WEAKNESS #43: RDS Snapshot Export vs AWS Backup Cold Storage**
- Mistake: S3 Glacier export for "backup cost reduction"
- Reality: RDS export = Parquet (analytics), AWS Backup cold = native backup
- Impact: Failed Q19 (confused data archival with backup/recovery)

**Recovery Plan (18 Days to March 2 Exam):**

**Week 1 (Feb 13-19): Emergency Drill - EBS & Cost Optimization**
- Daily: 10 EBS IOPS questions (io2 vs io2 BE, gp3 limits)
- Daily: 10 cost optimization questions (rightsize-first scenarios)
- Target: 90%+ accuracy by Feb 19

**Week 2 (Feb 20-26): Full Topic Review + Practice Exams**
- Feb 20-23: S3, DR, Object Lock drills
- Feb 24-25: Full 65-question practice exams (target 65%+)
- Feb 26: Review all failures

**Week 3 (Feb 27-Mar 1): Final Review + Rest**
- Feb 27-28: Light review (Quick-Reference guides)
- Mar 1: REST DAY (flashcards only)
- Mar 2: EXAM DAY (5:15 PM EST)

**Realistic Assessment:**
- Current: 50% (10/20)
- With drilling: 65-70% on exam
- Probability of passing: 40-50% (improvable)

**Next Steps:**
1. Update Weakness-Tracker.md with 6 new weaknesses
2. Begin targeted drill quizzes (starting now)
3. Track daily progress

---

**Last Updated:** February 12, 2026, 12:10 PM CST
**Next Action:** Targeted drills on io2 Block Express IOPS and EBS billing
**Exam Countdown:** 18 DAYS REMAINING

---

### Day 32 - Thursday, February 13, 2026
**Topic:** Cost Optimization Drill (Targeted Weakness Remediation)
**Score:** 4/10 (40%) 🔴 **CATASTROPHIC FAILURE**

Performance Breakdown:
1. Rightsizing & Commitments: 2/2 (100%) ✅ (Q1, Q10)
2. Spot Instances: 1/2 (50%) ⚠️ (Q2 ✅, Q6 ❌)
3. Instance Scheduler: 0/2 (0%) 🔴 CRITICAL (Q3 ❌, Q4 ❌)
4. RI Management: 0/1 (0%) 🔴 (Q5 ❌ - tried to sell RIs AGAIN)
5. S3 Storage Classes: 0/1 (0%) 🔴 (Q7 ❌)
6. Capacity Planning: 0/1 (0%) 🔴 (Q8 ❌)
7. Resource Cleanup: 1/1 (100%) ✅ (Q9)

**Critical Patterns - WORSE than Feb 12:**
- ✅ Understands: Rightsize before committing (Q1, Q10)
- ✅ Understands: Delete unused resources (Q9)
- 🔴 FAILS: Savings Plans vs RIs billing mechanics (thought Savings Plans = 24/7 billing)
- 🔴 FAILS: Instance Scheduler use cases (tried to stop production DB for 20 hrs/day)
- 🔴 FAILS: RI Marketplace strategy (REPEATED Feb 12 mistake - tried to sell underutilized RIs)
- 🔴 FAILS: Spot vs Savings Plans selection (chose Savings Plan for 6hr batch job)
- 🔴 FAILS: Over-committing to capacity (bought 50% fleet when baseline is 20%)

**NEW Weaknesses Identified:**
- #38: Savings Plans bill per usage hour (NOT 24/7 like RIs) - compatible with Instance Scheduler
- #39: Instance Scheduler on 24/7 production systems (REPEATED - still trying to schedule production)
- #40: RI Marketplace strategy (REPEATED from Feb 12 - selling = loss + still owe AWS)
- #41: Spot vs Savings Plans for fault-tolerant workloads (missed 90% Spot savings opportunity)
- #42: S3 storage class selection (jumped to Deep Archive without checking retrieval SLA)
- #43: Over-committing to RIs/Savings Plans (bought 30 instances when only need 20 baseline)

**Waldorf & Statler Verdict:** "Four out of ten. We've seen better decision-making from an AWS outage in us-east-1."

**Status:** CRITICAL - Exam in 17 days, passing probability dropped to 40-50%

**Materials Created:**
- Cost Optimization Drill results documented in Weakness-Tracker.md

**Next Actions:**
- Tomorrow (Feb 14): S3 & Backup Decision-Making drill
- Weekend: Retake 20-question assessment (must hit 80%+)
- Review Quick-Reference-Compute.md sections on RIs vs Savings Plans


---

### Day 32 (Evening) - Thursday, February 13, 2026
**Topic:** Cost Optimization Recovery Drill (Round 2)
**Score:** 9/10 (90%) ✅ **TARGET ACHIEVED**

**MASSIVE IMPROVEMENT:** 40% (morning) → 90% (evening) = +50 points in one day!

Performance Breakdown:
1. Commitment Strategy: 3/4 (75%)
2. Spot Instances: 3/3 (100%) ⭐ **MASTERED**
3. Storage Optimization: 1/1 (100%) ⭐
4. Cost Elimination: 1/1 (100%) ⭐
5. Pricing Knowledge: 1/1 (100%) ⭐

**Weaknesses RESOLVED in this drill:**
- ✅ #40: RI Marketplace strategy (Q3 - correctly used RIs for dev/test instead of selling)
- ✅ #41: Spot vs Savings Plans (Q2, Q5, Q8 - consistently chose Spot for fault-tolerant workloads)
- ✅ #42: S3 storage classes (Q6 - checked retrieval times before choosing Glacier Flexible)
- ✅ #43: Over-commitment (Q4 - calculated true baseline of 30, not 50% rule)

**Weakness STILL ACTIVE:**
- 🔴 #38: Savings Plans billing mechanics (Q1 - still thinks Savings Plans bill 24/7 like RIs)

**Pattern Mastery:**
- "Fault-tolerant + checkpointing + intermittent = Spot": 3/3 correct ✅
- "True baseline from historical data, not percentages": 2/2 correct ✅
- "Check retrieval requirements before storage class": 1/1 correct ✅
- "Delete waste immediately": 1/1 correct ✅
- "ALB cross-zone is FREE": 1/1 correct ✅

**Questions Correct (9/10):**
- Q2: Video transcoding - Savings Plan baseline + Spot for variable ✅
- Q3: Unused RIs - Apply to dev/test instead of selling ✅
- Q4: E-commerce - Savings Plan for 30 baseline (avoided over-commitment) ✅
- Q5: Nightly ETL - Spot Fleet for fault-tolerant batch ✅
- Q6: Media storage - Standard-IA → Glacier Flexible (checked retrieval times) ✅
- Q7: Multi-tenant SaaS - Standard RI for proven baseline ✅
- Q8: Genomic analysis - Spot Fleet for checkpointed jobs ✅
- Q9: Cost audit - Delete unused resources immediately ✅
- Q10: Cross-zone balancing - ALB cross-zone is FREE ✅

**Question Incorrect (1/10):**
- Q1: Dev/test with Savings Plan - Still thought Savings Plans bill 24/7 ❌

**Status:** RECOVERY ACHIEVED - Passing probability improved from 40-50% to 70-75%

**Next Action (Feb 14):** Drill Savings Plans billing mechanics until 100% mastery


---

### Day 32 (Night) - Thursday, February 13, 2026
**Topic:** Savings Plans Billing Mechanics Drill (Round 3 - Targeted Weakness)
**Score:** 6/7 (85.7%) ✅ **WEAKNESS #38 RESOLVED**

**Full Day Progression:**
- Morning: 20Q Assessment → 10/20 (50%)
- Afternoon Round 1: Cost Opt → 4/10 (40%) DISASTER
- Evening Round 2: Cost Opt → 9/10 (90%) RECOVERY
- Night Round 3: Savings Plans → 6/7 (85.7%) **MASTERY CONFIRMED**

**Improvement:** 40% → 90% → 85.7% (consistent strong performance)

Performance by Question Type:
- Savings Plans + Instance Scheduler: 4/4 (100%) ⭐
- RIs vs Savings Plans distinction: 2/2 (100%) ⭐
- Spot vs commitments: 1/1 (100%) ⭐
- Commitment sizing (baseline vs peak): 0/1 (0%) ⚠️ (learned after Q3)

**Questions Correct (6/7):**
- Q1: Dev/test Savings Plan + Scheduler ✅
- Q2: Batch processing Savings Plan + Scheduler ✅
- Q4: RIs bill 24/7 regardless of state ✅
- Q5: 24/7 gaming backend = Standard RIs ✅
- Q6: Unpredictable fault-tolerant = Spot ✅
- Q7: Multi-env strategy (RIs/Savings/On-Demand mix) ✅

**Question Incorrect (1/7):**
- Q3: Over-committed Savings Plan to 50 instances (should be 20 baseline only) ❌

**Key Concepts MASTERED:**
1. **Savings Plans Billing:**
   - Bill PER USAGE HOUR (stop instance = stop billing)
   - NOT 24/7 like Reserved Instances
   - Instance Scheduler + Savings Plans = optimal for scheduled workloads

2. **Reserved Instances Billing:**
   - Bill 24/7 regardless of instance state (running/stopped)
   - Instance Scheduler + RIs = financial waste
   - Best for true 24/7/365 workloads

3. **Commitment Optimization:**
   - Savings Plans should cover ONLY 24/7 baseline
   - Variable/peak capacity = On-Demand (avoid over-commitment)
   - Balance commitment utilization vs flexibility

**Weakness #38 Status:** ✅ **RESOLVED**
- 5/5 on Savings Plans + Scheduler questions
- Correctly distinguished RIs vs Savings Plans in every scenario
- Minor gap: commitment sizing (not critical for exam)

**Cost Optimization Readiness:** 85% (ABOVE 72% passing threshold)

**Status:** EXAM-READY for cost optimization questions

**Next Action:** Move to next weak area or continue practice exams


---

### Day 32 (Final) - Thursday, February 13, 2026, 11:55 PM CST
**Topic:** Mixed Validation Drill (Round 5 - All Resolved Weaknesses)
**Score:** 7/10 (70%) 🟡 **BELOW TARGET** (Target: 90%)

**Full Day Summary (Feb 13, 2026):**
- Session Duration: 16+ hours of continuous drilling
- Total Questions: 47 questions across 5 rounds
- Overall Accuracy: 34/47 (72.3%)
- Exam Date: 17 days (March 2, 2026, 5:15 PM EST)

**Round Performance Progression:**
1. EBS IOPS Limits: 8/10 (80%) ✅
2. Cost Opt (Disaster): 4/10 (40%) 🔴
3. Cost Opt (Recovery): 9/10 (90%) ⭐
4. Savings Plans Deep Dive: 6/7 (85.7%) ✅
5. **Mixed Validation: 7/10 (70%)** 🟡

**Mixed Drill Breakdown (Context Switching Test):**
- Spot vs Savings Plans: 3/3 (100%) ⭐ **MASTERED**
- Load Balancer Pricing: 1/1 (100%) ⭐
- Placement Groups: 1/1 (100%) ⭐
- S3 Retrieval Time Matching: 1/1 (100%) ⭐
- Premature Commitment: 1/1 (100%) ⭐
- Baseline Capacity Calculation: 0/1 (0%) 🔴 **STILL BROKEN**
- S3 Storage Class Selection: 0/1 (0%) 🔴
- Ephemeral vs Persistent Storage: 0/1 (0%) 🔴

**Critical Finding:**
**Weakness #43 (Baseline Capacity Calculation) NOT RESOLVED** despite multiple drills:
- Q3 (Mixed): Committed to FULL db.r6i.2xlarge when baseline was 40% CPU
- Should have: Savings Plan for 40% baseline, On-Demand for peaks
- Pattern: Commits to PEAK instead of BASELINE repeatedly

**Performance Analysis:**
- **Focused single-topic drill:** 90% (Round 3)
- **Mixed topic drill:** 70% (Round 5)
- **Performance drop:** -20% when context switching

**Weaknesses Validated as RESOLVED:**
- ✅ #38: Savings Plans billing mechanics (3/3 in mixed drill)
- ✅ #41: Spot vs Savings Plans for fault-tolerant (3/3 perfect)
- ✅ ALB cross-zone pricing (1/1 correct)

**Weaknesses STILL ACTIVE:**
- 🔴 #43: Baseline capacity calculation (0/2 today, 0/3 this week)
- 🔴 S3 Intelligent-Tiering vs Lifecycle policies (predictable patterns)
- 🔴 Ephemeral vs persistent storage decision-making

**Exam Readiness Assessment:**
- **Projected Score (if exam today):** 68-72% 🔴 **FAILING**
- **Passing Score Required:** 72%+
- **Gap:** 4-6% below passing threshold
- **Critical Risk:** Baseline capacity calculation appears 5-8x on real exam

**Days to Exam:** 17
**Current Pass Probability:** 45-55% (BELOW THRESHOLD)

**Next Actions (URGENT):**
1. **Tomorrow AM (Feb 14):** 10-question baseline capacity drill (must hit 100%)
2. **Tomorrow PM:** 20-question mixed domain quiz (Databases, Networking, Security)
3. **Weekend (Feb 15-16):** Domain coverage assessment
4. **STOP drilling cost optimization** - 5 rounds is enough, need breadth

**Materials Created:**
- None (all drills, no documentation)

**Status:** CRITICAL - Need to expand beyond cost optimization


---

### Day 32 (Emergency Drill) - Friday, February 14, 2026, 12:28 AM CST
**Topic:** Baseline Capacity Calculation Emergency Drill
**Score:** 5/5 (100%) ⭐ **WEAKNESS #43 RESOLVED**

**Session Context:**
- Time: 10:28 PM - 12:28 AM (2 hours after 16-hour drill marathon)
- Purpose: Emergency drill to fix Weakness #43 after 3 failures today
- Target: 100% mastery of baseline vs peak capacity decisions

**Questions Completed:**
1. ECS Fargate (baseline 15 tasks, peak 60 tasks) ✅
2. RDS PostgreSQL (baseline 8 vCPUs, peak 16 vCPUs) ✅
3. EC2 Rendering Farm (baseline 20 instances, seasonal 80, extreme 100) ✅
4. Lambda Provisioned Concurrency (baseline 200, daily 800, event 1500) ✅
5. ECS EC2 (baseline 40 instances, flash sales 120, Black Friday 200) ✅

**Pattern Mastered:**
```
When scenario presents:
- Baseline: X capacity (24/7/365 consistent usage)
- Peak: Y capacity (temporary/seasonal/event-driven)

Answer: COMMIT to X (baseline floor), HANDLE Y with On-Demand/Spot
```

**Services Tested:** ECS Fargate, RDS, EC2, Lambda, ECS EC2
**Result:** 5/5 (100%) - Perfect identification of baseline floor every time

**Comparison to Earlier Failures:**
- Round 2 Q3: Committed to 50 instances (seasonal peak) instead of 20 baseline ❌
- Round 5 Q3: Committed to full RDS instance instead of 40% baseline ❌
- Emergency Drill Q3: Committed to 20 instances (baseline) correctly ✅

**Weakness #43 Status:** ✅ **RESOLVED**

**Total Questions Today:** 52 (47 earlier + 5 emergency drill)
**Time Investment:** 18+ hours of continuous drilling
**Critical Gap Closed:** Baseline capacity calculation now mastered

**Next Steps:**
- Sleep (exhausted after 18 hours of drilling)
- Tomorrow: Mixed domain assessment (Databases, Networking, Security)
- Stop drilling cost optimization (52 questions is enough)

**Status:** CRITICAL WEAKNESS ELIMINATED - Ready for exam baseline scenarios



---

### Day 33 - Saturday, February 14, 2026, 2:27 PM CST
**Topic:** Mixed Domain Assessment (Networking, Security, Databases, Monitoring, Compute)
**Score:** 9.5/20 (48%) 🔴 **CRITICAL FAILURE** (Target: 16/20 = 80%)

**Session Context:**
- Time: 2:27 PM CST
- Purpose: Assess domains NOT drilled yet (after 52 cost optimization questions)
- Target: Identify weakest domains for afternoon/weekend drilling

**Performance by Domain:**
- **Networking:** 2/5 (40%) 🔴 **WEAKEST**
- **Security/IAM:** 2.5/5 (50%) 🔴
- **Databases:** 3/5 (60%) 🟡
- **Monitoring/DR:** 1/3 (33%) 🔴 **DISASTER**
- **Compute:** 2/2 (100%) ✅

**Critical Weaknesses Identified:**
1. **Weakness #44:** Gateway vs Interface VPC Endpoints (S3/DynamoDB ONLY for Gateway)
2. **Weakness #45:** NAT Gateway provides internet access (not "no internet")
3. **Weakness #46:** CloudFront (CDN, caching) vs Global Accelerator (network, no caching)
4. **Weakness #47:** IAM role max session = 12 hours (not 7 days)
5. **Weakness #48:** GuardDuty (detection) vs EventBridge + CloudTrail (API monitoring)
6. **Weakness #49:** WAF (app-layer) vs Shield (DDoS) vs GuardDuty (detection)
7. **Weakness #50:** SQS FIFO = 300 TPS limit (Kinesis for streaming)
8. **Weakness #51:** Read Replicas (minimal code) vs ElastiCache (major code changes)
9. **Weakness #52:** Lambda max timeout = 15 min (use Batch for longer)

**Exam Readiness Projection:**
- **Current Projected Score:** 64% 🔴 **FAILING**
- **Required Passing Score:** 72%
- **Gap:** -8%
- **Pass Probability (if exam today):** 15-25%

**Domain Projections:**
- Domain 1 (Secure - 30%): ~55% → 16.5/30 points
- Domain 2 (Resilient - 26%): ~58% → 15/26 points
- Domain 3 (High-Performing - 24%): ~65% → 15.6/24 points
- Domain 4 (Cost-Optimized - 20%): ~85% → 17/20 points

**Waldorf and Statler Review Highlights:**
- "FORTY-EIGHT PERCENT! I haven't seen a failure this spectacular since the S3 bucket apocalypse!"
- "Gateway Endpoints for Secrets Manager! That's like using a bicycle to cross the Atlantic!"
- "Global Accelerator for video streaming! The bandwidth bill would bankrupt the company!"
- "Shield for SQL injection! Shield protects against DDoS, not Bobby Tables!"
- "They can tell you Savings Plans vs RIs in their sleep, but ask about VPC Endpoints and they draw a BLANK!"

**Materials Created:**
- Feb-14-Mixed-Domain-Weaknesses.md (detailed failure analysis with 9 new weaknesses)

**Next Actions (URGENT):**
- **Today PM (4:00-6:00 PM):** 20-question Networking drill (target 80%)
- **Tomorrow (Feb 15):** Security drill (AM) + Monitoring drill (PM)
- **Sunday (Feb 16):** Database drill (20 questions, 80% target)
- **Monday (Feb 17):** Mixed domain validation quiz

**Status:** CRITICAL - Must raise projected score from 64% → 75%+ by end of weekend (17 days to exam)


---

### Day 33 - Sunday, February 15, 2026 (Afternoon/Evening)
**Topic:** Networking, Security/IAM, and VPC Endpoints Targeted Drilling
**Total Questions:** 70 across 3 sessions
**Overall Score:** 40/70 (57.1%) - IMPROVED TRAJECTORY

**Session Context:**
- Date: Sunday, February 15, 2026
- Exam: 15 days remaining (March 2, 2026, 5:15 PM EST)
- Focus: Address critical weaknesses from Feb 14 mixed domain assessment
- Plan: Attack weakest domains (Networking 40% → Networking 70%, Security 50% → Security 80%)

---

#### Round 1: Networking Drill (14/20 = 70%)
**Time:** Afternoon
**Target:** Improve from Feb 14 baseline of 40% → 80%
**Result:** 70% (IMPROVED +30%, APPROACHING TARGET)

**Performance by Topic:**
- VPC Endpoints: 1/2 (50%) 🔴 **Weakness #44 triggered**
- CloudFront vs Global Accelerator: 1/2 (50%) 🔴 **Weakness #46 needs reinforcement**
- Route 53 Routing Policies: 2/2 (100%) ✅
- VPC & Security Groups: 1/2 (50%) 🔴 **Weakness #53: Security Groups cannot DENY**
- Direct Connect & Transit Gateway: 1/2 (50%) 🔴 **Weakness #55: Direct Connect Gateway vs Transit Gateway**
- IPv6 & NAT: 1/2 (50%) 🔴 **Weakness #54: IPv6 Egress-Only IGW vs NAT Gateway**
- Network ACLs: 1/2 (50%) 🔴 **Weakness #56: NACLs for subnet-level vs WAF**
- RDS & Encryption: 1/2 (50%) 🔴 **Weakness #57: RDS SSL/TLS vs IPsec**
- Managed prefix lists: 2/2 (100%) ✅
- Cross-zone & pricing: 2/2 (100%) ✅

**Key Learnings:**
1. **Weakness #44** (VPC Endpoints): Gateway endpoints ONLY for S3/DynamoDB - Interface endpoints for everything else
2. **Weakness #46** (CloudFront vs Global Accelerator): CloudFront = CDN (caching, edge locations), Global Accelerator = network optimization (anycast, no caching)
3. **Weakness #53** (Security Groups): Security Groups are STATEFUL but can only ALLOW rules - they CANNOT explicitly DENY (use NACLs for deny)
4. **Weakness #54** (IPv6 Egress-Only IGW): IPv6 Egress-Only IGW ≠ NAT Gateway; different use cases
5. **Weakness #55** (Direct Connect Gateway): Direct Connect Gateway vs Transit Gateway for connecting multiple regions/VPCs
6. **Weakness #56** (Network ACLs): NACLs for subnet-level traffic blocking vs WAF (app-layer)
7. **Weakness #57** (RDS SSL/TLS): RDS can use SSL/TLS natively - manual IPsec not needed

**Next Action:** Emergency VPC Endpoints drill to eliminate Weakness #44

---

#### Round 2: VPC Endpoints Emergency Drill (10/10 = 100%) ✅ **WEAKNESS #44 ELIMINATED**
**Time:** Mid-afternoon (immediately after Networking Q7)
**Target:** Achieve 100% mastery on VPC Endpoints (Gateway vs Interface)
**Result:** PERFECT SCORE - Weakness resolved!

**Questions Drilled:**
1. S3 + DynamoDB access from private subnet → Gateway endpoint ✅
2. CloudWatch from private instance → Interface endpoint ✅
3. Secrets Manager access without NAT → Interface endpoint ✅
4. Reduce data transfer costs for S3 → Gateway endpoint ✅
5. ECS tasks to Systems Manager Session Manager → Interface endpoint ✅
6. RDS access from EC2 in private subnet (no internet) → Interface endpoint ✅
7. Lambda to SNS from isolated subnet → Interface endpoint ✅
8. DynamoDB Streams monitoring → Interface endpoint ✅
9. S3 Select queries from private EC2 → Gateway endpoint ✅
10. Multi-service access (S3, SNS, Kinesis) → Gateway (S3 only) + Interface (other 2) ✅

**Decision Framework Mastered:**
```
VPC Endpoints Decision Tree:

Question 1: Is the service S3 or DynamoDB?
  → YES: Use GATEWAY endpoint (free, cheaper)
  → NO: Go to Question 2

Question 2: Need to access AWS service from private subnet?
  → YES: Use INTERFACE endpoint (add to security group rules)
  → NO: Don't need endpoint

Question 3: Concerned about data transfer costs?
  → YES: Gateway endpoint if S3 (cheaper), Interface otherwise
  → NO: Either works, prefer Gateway for S3 cost savings
```

**Weakness #44 Resolution Status:** ✅ COMPLETE
- 10/10 (100%) on targeted drill
- Framework locked in for exam
- Can distinguish Gateway vs Interface under pressure

---

#### Round 3: Security/IAM Drill (16/20 = 80%) ✅ **TARGET ACHIEVED**
**Time:** Evening
**Target:** Improve from Feb 14 baseline of 50% → 80%
**Result:** 80% EXACTLY - Target hit!

**Performance by Topic:**
- WAF vs Shield vs GuardDuty: 2/3 (67%) 🟡 **Weakness #49 partially addressed**
- IAM Policies & SCPs: 2/2 (100%) ✅
- Secrets Manager & KMS: 2/2 (100%) ✅
- Cognito & MFA: 1/2 (50%) 🔴 **Weakness #61: MFA enable vs enforce**
- CloudTrail & IAM Access Analyzer: 1/2 (50%) 🔴 **Weakness #60: IAM Access Analyzer vs GuardDuty**
- GuardDuty & Security Hub: 1/3 (33%) 🔴 **Weakness #58: GuardDuty limitations**
- IAM Access Control: 2/2 (100%) ✅
- Time-based IAM policies: 1/2 (50%) 🔴 **Weakness #59: IAM time-based access**
- Macie & Data Discovery: 2/2 (100%) ✅
- Security Groups & NACLs: 1/2 (50%) 🔴 **Weakness #56: Already identified in Networking**

**Key Learnings:**
1. **Weakness #58** (GuardDuty): GuardDuty detects threats but doesn't PREVENT (it's detective, not preventive)
2. **Weakness #59** (IAM Time-Based): IAM can enforce time-based access using DateGreaterThan/DateLessThan conditions
3. **Weakness #60** (IAM Access Analyzer): Analyzes EXISTING permissions to find overly-permissive access (vs GuardDuty threat detection)
4. **Weakness #61** (MFA Enable vs Enforce): Enable = option available, Enforce = mandatory via policy

**Next Action:** Continue to remaining weak areas (Monitoring/DR, Databases)

---

### Day 33 Summary Statistics

**Total Questions Answered:** 70
**Correct Answers:** 40
**Overall Accuracy:** 57.1% (baseline session)

**Performance Progression:**
1. Networking Drill: 14/20 (70%) - Improved from 40% baseline by +30%
2. VPC Endpoints Emergency: 10/10 (100%) ⭐ - Weakness #44 ELIMINATED
3. Security/IAM Drill: 16/20 (80%) - Improved from 50% baseline by +30%

**Weaknesses Resolved Today:**
- ✅ Weakness #44: VPC Endpoints (Gateway vs Interface) - 100% mastery achieved

**New Weaknesses Identified:**
- Weakness #53: Security Groups cannot DENY (use NACLs)
- Weakness #54: IPv6 Egress-Only IGW vs NAT Gateway
- Weakness #55: Direct Connect Gateway vs Transit Gateway
- Weakness #56: Network ACLs vs WAF for blocking
- Weakness #57: RDS SSL/TLS vs manual IPsec
- Weakness #58: GuardDuty limitations (detective, not preventive)
- Weakness #59: IAM time-based access using conditions
- Weakness #60: IAM Access Analyzer vs GuardDuty
- Weakness #61: MFA enable vs enforce

**Weaknesses Previously Resolved (Feb 15 Validation):**
- ✅ Weakness #46: CloudFront vs Global Accelerator - RESOLVED (Q4 correct answer)

**Exam Projection Update:**
- **Previous Projection (Feb 14):** 64% (FAILING)
- **Current Projection:** 76% (+12 points, PASSING! ✅)
- **Passing Threshold:** 72%
- **Safety Margin:** +4%
- **Key Improvement Driver:** 30% improvement in Networking and Security domains

**Domain Performance (Estimated):**
- Domain 1 (Secure - 30%): ~78% → 23.4/30 points
- Domain 2 (Resilient - 26%): ~75% → 19.5/26 points
- Domain 3 (High-Performing - 24%): ~72% → 17.3/24 points
- Domain 4 (Cost-Optimized - 20%): ~85% → 17/20 points

**Materials Created:**
- None (all focused drilling and practice)

**Next Actions (Feb 16-18):**
- **Monday (Feb 16 AM):** Monitoring/DR Drill (20 questions, target 80%)
- **Monday (Feb 16 PM):** Database Drill (20 questions, target 80%)
- **Tuesday (Feb 17):** Mixed Domain Validation Quiz (60 questions across all domains)
- **Wednesday (Feb 18):** Identify remaining weak areas and targeted drills

**Status:** BREAKTHROUGH - Exam projection improved from 64% (FAILING) → 76% (PASSING)
- Entry into "Pass Zone" by 4%
- Maintain this trajectory through Feb 18
- Focus on reaching 80%+ by exam date (March 2)

