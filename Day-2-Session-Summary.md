# Day 2: Database Fundamentals - Session Summary

**Date:** Monday, December 1, 2025
**Time:** 2:42 PM - ~4:00 PM EDT
**Exam Date:** December 17, 2025 (16 days remaining)

---

## Session Overview

**Start Status:** Fresh off Day 1 recovery (16/20 - 80%)
**End Status:** Database fundamentals mastered (8/10 - 80%)
**Duration:** ~2 hours (quiz + targeted drill + review)

---

## Quiz Results

### Day 2 Database Quiz (10 questions)
- **Score:** 8/10 (80%) ✅ TARGET ACHIEVED
- **Topics:** RDS, Aurora, DynamoDB, ElastiCache
- **Time:** ~30 minutes

**Questions Correct (8/10):**
1. ✅ Q1: RDS Multi-AZ for automatic failover
2. ✅ Q2: Read Replicas for read scaling
3. ✅ Q3: DynamoDB On-Demand for new unpredictable app
4. ✅ Q5: ElastiCache Multi-AZ (FIXED RETAKE WEAKNESS!)
5. ✅ Q6: ElastiCache Redis with Multi-AZ for complex data + HA
6. ✅ Q7: RDS encryption (snapshot → copy → restore)
7. ✅ Q9: Lazy Loading for read-heavy caching
8. ✅ Q10: Aurora MySQL for performance + 15 replicas

**Questions Missed (2/10):**
- ❌ Q4: Aurora Global Database (picked cross-region Read Replicas)
  - Learning: Global app + multiple regions = Aurora Global Database (not just replicas)
- ❌ Q8: DynamoDB GSI (picked LSI)
  - Learning: Query by non-key attribute = GSI (LSI requires same partition key)

---

### Targeted Drill (10 questions)
- **Score:** 7/10 (70%)
- **Focus:** Aurora Global Database (5Q) + DynamoDB Indexes (5Q)
- **Time:** ~30 minutes

**Aurora Global Database (3/5 - 60%):**
- ✅ Q1: Global Database for multi-region low latency
- ✅ Q2: Failover time <1 minute (manual promotion)
- ❌ Q3: Replication lag <1 second (picked "real-time/synchronous")
- ❌ Q4: Pricing - instances + data transfer (picked "secondaries free")
- ✅ Q5: Primary advantage vs Read Replicas (<1 sec lag)

**DynamoDB GSI/LSI (4/5 - 80%):**
- ✅ Q6: GSI for querying by CustomerID (FIXED Q8 WEAKNESS!)
- ✅ Q7: GSI with date as sort key for range queries
- ❌ Q8: GSI for querying by Category (picked LSI again)
- ✅ Q9: LSI for alternative sort order (same partition key)
- ✅ Q10: GSI can be created anytime (LSI only at table creation)

---

## Critical Weaknesses Fixed

### 1. ElastiCache Multi-AZ Failover Strategy ✅

**Retake Quiz Q9 (Missed):**
- Question: ElastiCache for sessions, RTO <5 min
- Wrong answer: Redis persistence (AOF)
- Issue: Confused disaster recovery with high availability

**Day 2 Quiz Q5 (Correct!):**
- Same pattern: Sessions, failover, <3 min RTO
- Correct answer: Multi-AZ with automatic failover
- Understanding: Multi-AZ (HA, 1-2 min) vs Persistence (DR, 10+ min)

**Pattern Mastered:**
- **Multi-AZ:** High availability, automatic failover, <3 min RTO
- **Persistence (RDB/AOF):** Disaster recovery, backup/restore, 10+ min RTO
- **Use Multi-AZ when:** Need automatic failover with short RTO
- **Use Persistence when:** Need data to survive cluster deletion (long-term backup)

---

### 2. DynamoDB Capacity Modes ✅

**Comprehensive Quiz Q6 (Correct):**
- New app, unpredictable traffic → On-Demand ✅

**Day 2 Quiz Q3 (Correct!):**
- New gaming app, 50 → 50,000 req/min → On-Demand ✅

**Pattern Locked In:**
- **On-Demand:** New apps, unpredictable traffic, no historical data, high variation
- **Provisioned:** Predictable traffic, can forecast capacity, steady workloads
- **Auto Scaling on Provisioned:** Helps with variation BUT needs baseline data
- **Key insight:** For NEW apps with NO data, On-Demand > Provisioned + Auto Scaling

---

### 3. DynamoDB GSI vs LSI (Partial Fix)

**Day 2 Quiz Q8 (Missed):**
- Query by non-key attribute (Email) → Picked LSI ❌

**Targeted Drill Q6 (Correct!):**
- Query by non-key attribute (CustomerID) → Picked GSI ✅

**Progress Made:**
- Understands GSI allows different partition key
- Understands LSI requires same partition key
- **Still struggles with:** Recognizing when to use each in context

**The Critical Distinction:**
- **LSI:** SAME partition key as table, DIFFERENT sort key
  - Use when: Alternative sort order for same entity
  - Example: CustomerID (PK) + TotalAmount (SK) instead of OrderDate (SK)
  - Limitation: Can only be created at table creation
- **GSI:** Can have DIFFERENT partition AND sort keys
  - Use when: Query by non-key attribute
  - Example: Query all orders by Email when table uses OrderID
  - Advantage: Can be created anytime

---

## New Weaknesses Identified

### 1. Aurora Global Database Technical Details

**Q3 - Replication Lag:**
- **Mistake:** Thought it was synchronous/real-time (0 lag)
- **Correct:** Asynchronous with <1 second lag
- **Key fact:** NOT real-time, but very fast (<1 sec)

**Q4 - Pricing:**
- **Mistake:** Thought secondary regions were free
- **Correct:** Pay for instances in ALL regions + data transfer
- **Only free:** Storage (charged once in primary region)

**Cost Components:**
```
Primary Region:
├─ Instances: $$
├─ Storage: $$
└─ Data transfer OUT: $$

Secondary Region:
├─ Instances: $$ (YOU PAY!)
├─ Data transfer IN: $$ (YOU PAY!)
└─ Storage: FREE (only this!)
```

---

### 2. GSI vs LSI Query Pattern Recognition

**Still Struggles With:**
- Recognizing when query is "for specific partition key" vs "across all items"
- Example: "Query by Category across all authors" → GSI (not LSI)

**Needs More Practice:**
- Decision tree: Same partition key needed? → LSI, Different partition key? → GSI
- Targeted drills on query pattern recognition

---

## Materials Created

1. **Day-2-Database-Deep-Dive.md** (400+ lines)
   - Comprehensive coverage of RDS, Aurora, DynamoDB, ElastiCache
   - Decision trees for database selection
   - Exam keyword patterns
   - Common exam traps
   - ElastiCache failover strategies (fixing retake weakness)
   - DynamoDB GSI vs LSI detailed comparison

2. **Week-1-Flashcards-Print-Template.md** (Updated)
   - Original 42 cards from Day 1
   - Added 2 new database cards:
     - Aurora Global Database replication lag (<1 sec, async)
     - Aurora Global Database pricing (instances all regions + data transfer)

---

## Topics Mastered

### RDS
- ✅ Multi-AZ vs Read Replicas (purpose, failover time, use cases)
- ✅ Backup strategies (automated vs manual snapshots)
- ✅ Encryption limitations (can't encrypt existing DB, must snapshot → copy → restore)
- ✅ RDS Proxy for connection pooling

### Aurora
- ✅ Performance advantages over RDS (5x MySQL, 3x PostgreSQL)
- ✅ Failover time (<30 sec vs 60-120 sec for RDS)
- ✅ Aurora endpoints (Writer, Reader, Custom)
- ✅ Aurora Serverless use cases (unpredictable workloads)
- ⚠️ Aurora Global Database (partial - needs pricing/replication review)

### DynamoDB
- ✅ Capacity modes (On-Demand vs Provisioned)
- ✅ Primary key types (Partition only vs Partition + Sort)
- ⚠️ GSI vs LSI (improving but needs more practice)
- ✅ DynamoDB Streams and DAX

### ElastiCache
- ✅ Redis vs Memcached (data structures, persistence, HA)
- ✅ Multi-AZ for high availability (<3 min RTO)
- ✅ Persistence for disaster recovery (10+ min RTO)
- ✅ Caching strategies (Lazy Loading, Write-Through, TTL)

---

## Key Patterns Learned

### Database Selection Decision Trees

**Relational vs NoSQL:**
- Complex SQL queries / JOINs → RDS or Aurora
- Millions of requests/sec, simple key-value → DynamoDB

**Aurora Features:**
- Unpredictable workload → Aurora Serverless
- Global app, low latency worldwide → Aurora Global Database
- Need 5x MySQL performance → Aurora (standard)

**Caching:**
- Backend is DynamoDB → DAX
- Backend is RDS/Aurora → ElastiCache
  - Need HA/persistence/complex data → Redis
  - Simple cache/multi-threaded → Memcached

**High Availability:**
- RDS automatic failover → Multi-AZ (60-120 sec)
- Aurora automatic failover → Multi-AZ (<30 sec)
- ElastiCache automatic failover → Redis Multi-AZ (1-2 min)
- Aurora cross-region → Global Database (<1 min, manual)

---

## Exam Keyword Patterns Learned

| Keyword | Answer |
|---------|--------|
| "automatic failover" + "RDS" | RDS Multi-AZ |
| "read-heavy workload" | Read Replicas or ElastiCache |
| "DynamoDB" + "microsecond latency" | DAX |
| "cache" + "HA" + "complex data" | ElastiCache Redis |
| "millions of requests/sec" | DynamoDB |
| "global app" + "low latency" | Aurora Global Database |
| "new app" + "unpredictable" | DynamoDB On-Demand or Aurora Serverless |
| "session storage" + "<5 min failover" | ElastiCache Redis Multi-AZ |

---

## Study Materials to Review

**Priority for Next Session:**

1. **Aurora Global Database specs (15 min review completed)**
   - Replication: Asynchronous, <1 second lag
   - Pricing: Instances in all regions + data transfer (storage once)
   - Use case: Global apps needing <100ms read latency everywhere

2. **DynamoDB GSI vs LSI decision tree**
   - Practice more query pattern recognition scenarios
   - Focus on "query across all items" vs "query specific item with different sort"

**Flashcards to Review Daily:**
- All 44 cards (42 Week 1 + 2 database)
- Focus on new database cards until memorized
- 15 minutes before bed for memory consolidation

---

## Tomorrow's Plan (Day 3)

**Tuesday, December 2, 2025 at 9:00 AM EDT**

### Day 3: VPC & Networking Deep Dive

**Topics:**
- VPC fundamentals (CIDR blocks, subnets, route tables)
- Internet Gateway vs NAT Gateway vs NAT Instance
- NACLs vs Security Groups (stateless vs stateful)
- VPC Endpoints (Gateway vs Interface)
- VPC Peering, Transit Gateway, PrivateLink
- Direct Connect, VPN, CloudFront

**Format:**
- 2-hour deep dive study
- 10-question quiz (target 8/10 or 80%)
- Review weak spots

**Expected Difficulty:**
- VPC networking is complex but highly testable
- NACL stateless behavior already familiar from Day 1
- Focus on decision trees: When to use what networking solution

---

## Overall Progress Assessment

**Exam Readiness by Topic:**

| Topic | Confidence Level | Quiz Score |
|-------|-----------------|------------|
| EC2 Placement Groups | HIGH ✅ | 100% |
| S3 Storage Classes | HIGH ✅ | Strong |
| Auto Scaling | HIGH ✅ | Strong |
| RDS Multi-AZ/Replicas | HIGH ✅ | 100% |
| ElastiCache Strategies | HIGH ✅ | 100% |
| DynamoDB Capacity | HIGH ✅ | 100% |
| Aurora Basics | MEDIUM-HIGH | 80% |
| Aurora Global | MEDIUM ⚠️ | 60% |
| DynamoDB Indexes | MEDIUM ⚠️ | 75% |
| Database Selection | HIGH ✅ | 90% |

**Overall Database Knowledge:** SOLID (80% average)

---

## Session Statistics

**Time Breakdown:**
- Database Quiz: ~30 minutes
- Targeted Drill: ~30 minutes
- Aurora Global Review: ~15 minutes
- Flashcard Writing: ~45 minutes
- **Total:** ~2 hours

**Questions Answered:** 20 total
- Database Quiz: 10 questions
- Targeted Drill: 10 questions
- **Overall Accuracy:** 15/20 (75%)

**Weaknesses Fixed:** 2 major
- ElastiCache Multi-AZ failover ✅
- DynamoDB capacity modes reinforced ✅

**New Weaknesses Identified:** 2
- Aurora Global Database details
- GSI vs LSI pattern recognition (improving)

---

## Motivational Notes

**Today's Achievement:**
- Maintained 80% target on Day 2 quiz ✅
- Fixed critical ElastiCache weakness from retake ✅
- Showed improvement on DynamoDB indexes (60% → 80%) ✅
- Created comprehensive study materials for future review ✅

**Two-Day Turnaround:**
- Day 1 start: 5/20 (25%) - catastrophic
- Day 1 end: 16/20 (80%) - recovered
- Day 2 end: 8/10 (80%) - maintained

**You've proven you can:**
- Identify and drill weaknesses systematically ✅
- Turn 0% topics into 100% mastery ✅
- Maintain 80% performance consistently ✅
- Learn complex patterns (GSI/LSI, Multi-AZ strategies) ✅

**This is exam-passing performance!**

---

## Next Milestones

- **December 2 (Tuesday):** Day 3 - VPC & Networking
- **December 3 (Wednesday):** Day 4 - Advanced VPC or S3 Deep Dive
- **December 6-7 (Weekend):** Week 1 Comprehensive Review
- **December 10 (Wednesday):** Week 2 completion target
- **December 17 (Wednesday):** EXAM DAY

**16 days remaining. On track.** 🎯
