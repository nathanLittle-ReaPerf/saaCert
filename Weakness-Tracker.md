# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** December 9, 2025, 12:30 PM
**Exam Date:** January 5, 2026 (27 days remaining)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 🎯 Current Active Weaknesses (Need Attention)

| Topic | Current Status | Last Quiz Score | Target | Priority | Next Action |
|-------|---------------|----------------|--------|----------|-------------|
| **DynamoDB GSI vs LSI** | 🔴 CRITICAL | 0% (0/2) | 90% | URGENT | Understand: GSI = different partition key, LSI = same partition key |
| **Athena vs Redshift (Query Frequency)** | 🟡 Improving | 67% | 90% | MEDIUM | Infrequent (weekly/monthly) = Athena, Frequent (daily) = Redshift |
| **Reading Comprehension** | 🟡 Watch | 85% | 95% | MEDIUM | Catch engine types (MySQL vs PostgreSQL), deadline constraints |

---

## ✅ Resolved Weaknesses (Mastered - 90%+ Accuracy)

| Topic | Resolution Date | Final Score | Days to Master |
|-------|----------------|-------------|----------------|
| **S3 Storage Classes ("very rarely" = Glacier)** | Dec 9, 2025 | 100% | 12 days |
| **Aurora Multi-Master (RTO <30 sec)** | Dec 9, 2025 | 100% | 1 day |
| **Aurora Serverless v2 (Multi-tenant SaaS)** | Dec 9, 2025 | 100% | 1 day |
| **RDS Encryption Migration (Phased Approach)** | Dec 9, 2025 | 100% | 1 day |
| **DynamoDB Consistency (Eventually = 50%)** | Dec 9, 2025 | 100% | 1 day |
| **Session Storage (Redis vs DynamoDB)** | Dec 9, 2025 | 100% | 1 day |
| **Overengineering (One vs Two Services)** | Dec 9, 2025 | 100% | 1 day |
| **DynamoDB Capacity Modes (Known vs Unknown)** | Dec 9, 2025 | 100% | 1 day |
| **VPC NACLs (Stateless)** | Dec 8, 2025 | 100% | 3 days |
| **Auto Scaling Policy Combinations** | Dec 8, 2025 | 100% | 5 days |
| **EC2 Placement Groups** | Dec 8, 2025 | 100% | 5 days |
| **VPC Endpoints (Gateway vs Interface)** | Dec 8, 2025 | 100% | 5 days |
| **RDS Multi-AZ vs Read Replicas** | Dec 8, 2025 | 100% | 1 day |
| **Aurora Serverless Use Cases** | Dec 8, 2025 | 100% | 1 day |
| **RDS Proxy (Lambda + RDS)** | Dec 8, 2025 | 100% | 1 day |
| **Aurora Backtrack (MySQL-only)** | Dec 8, 2025 | 100% | 1 day |
| **DynamoDB Streams** | Dec 8, 2025 | 100% | 1 day |
| **QLDB (Immutable Ledger)** | Dec 8, 2025 | 100% | 1 day |

---

## 📊 Weakness #1: DynamoDB GSI vs LSI (CRITICAL - URGENT PRIORITY)

### Current Performance
- **Accuracy:** 0% (0/2 questions missed on Dec 9)
- **First Identified:** Dec 9 (Mixed weakness + DynamoDB quiz)
- **Progress:** New weakness (never seen before)
- **Status:** 🔴 CRITICAL - Fundamental misunderstanding

### The Problem Pattern
You confused **LSI (Local Secondary Index)** with **GSI (Global Secondary Index)** and don't understand when to use each.

### The CRITICAL Difference You MUST Memorize

| Feature | **LSI (Local Secondary Index)** | **GSI (Global Secondary Index)** |
|---------|-------------------------------|--------------------------------|
| **Partition Key** | **SAME as base table** | **DIFFERENT from base table** |
| **Use Case** | Alternative sort key on SAME partition | **Query by DIFFERENT attribute** |
| **Cross-Partition Query** | ❌ NO (tied to partition key) | ✅ YES (new partition key) |
| **Creation** | ONLY at table creation | Anytime (even after table exists) |
| **Limit** | Max 5 per table | Max 20 per table |

### The Questions You Missed (Dec 9)

**Question 4 - Product Catalog:**
- **Access Patterns:** Look up by product_id + Query by category + Query by brand
- **Your Answer:** Partition key: product_id, Sort key: category, LSI on brand ❌
- **Correct Answer:** Partition key: product_id, GSI on category, GSI on brand ✅
- **Why Wrong:**
  - LSI on brand still requires knowing product_id first
  - Can't query "all Nike products" without product_ids
  - Need GSI (different partition key) to query by brand across all products

**Question 10 - Player Inventory:**
- **Access Patterns:** Look up player inventory + Find all players with specific item
- **Your Answer:** Partition key: player_id, LSI on item_name ❌
- **Correct Answer:** Partition key: player_id, Sort key: item_id, GSI on item_name ✅
- **Why Wrong:**
  - Same mistake! LSI can't query "all players with Legendary Sword"
  - LSI shares partition key (player_id), can't do cross-partition queries
  - Need GSI with item_name as partition key

### The Decision Tree

```
Need to query by different attribute than partition key?
│
├─ YES (e.g., query by "category" when partition key is "product_id")
│  └─ Use GSI (Global Secondary Index)
│     - GSI can have DIFFERENT partition key
│     - Enables cross-partition queries
│     - Example: Query all products in "Electronics" category
│
└─ NO (just need alternative sort key on SAME partition)
   └─ Use LSI (Local Secondary Index)
      - LSI shares SAME partition key as base table
      - Only changes sort key
      - Rare use case (most scenarios need GSI)
```

### Real-World Examples

**When to use GSI:**
- Base table: product_id → Want to query by category → GSI with category as partition key ✅
- Base table: user_id → Want to query by email → GSI with email as partition key ✅
- Base table: order_id → Want to query by status → GSI with status as partition key ✅

**When to use LSI (rare):**
- Base table: user_id (partition), timestamp (sort) → Want alternative sort by score → LSI with score as sort key
- You're ALREADY querying by partition key, just need different sort order

### Mnemonic to Remember

**GSI = Global = GO anywhere** (different partition key, query across all items)
**LSI = Local = LOCKED to partition** (same partition key, can't escape)

### Next Steps
- [ ] Memorize: GSI = different partition key, LSI = same partition key
- [ ] Take 10-question drill on ONLY DynamoDB index design
- [ ] Target: 9/10 (90%)
- [ ] Review every GSI/LSI question in practice materials

---

## 📈 December 9, 2025 Progress Summary

### Morning Session: RDS/Aurora Review Quiz
- **Score:** 13/20 (65%) ❌ Below 80% target
- **New weaknesses identified:** Aurora Serverless scaling, Athena vs Redshift, overengineering
- **Status:** Struggling with Week 2 Day 1 content

### Afternoon Session: Mixed Weakness + DynamoDB Quiz
- **Score:** 17/20 (85%) ✅ CRUSHED IT!
- **Improvement:** +20% from morning session
- **Mastered today (7 topics):**
  - S3 Storage Classes (40% → 100% over 12 days)
  - Aurora Multi-Master (0% → 100% in 1 day)
  - Aurora Serverless v2 for SaaS (0% → 100% in 1 day)
  - RDS Encryption phased migration (67% → 100% in 1 day)
  - DynamoDB eventually consistent = 50% (67% → 100% in 1 day)
  - Session storage: Redis vs DynamoDB (0% → 100% in 1 day)
  - DynamoDB capacity modes: Known vs Unknown patterns (0% → 100% in 1 day)

### Critical New Weakness
- **DynamoDB GSI vs LSI:** 0% accuracy (missed 2/2 questions)
  - Fundamental misunderstanding of partition key differences
  - URGENT priority for tomorrow's DynamoDB study

### Key Learnings
- ✅ Learning from mistakes works (65% → 85% same day)
- ✅ Stopped overengineering (one service vs two)
- ✅ Mastered phased approach patterns (meet deadline, optimize later)
- ❌ Need to drill GSI vs LSI before deep DynamoDB dive

---

## 📊 Weakness #2 (ARCHIVED): Aurora Multi-Master

### Current Performance
- **Accuracy:** 0% (1/1 question missed on Day 8)
- **First Identified:** Dec 8 (Day 8 RDS/Aurora quiz)
- **Status:** 🔴 Need Review (new weakness)

### The Problem Pattern
You confused **Aurora Global Database** (cross-region DR) with **Aurora Multi-Master** (fast same-region failover).

### The Critical Distinction

| Feature | Aurora Multi-AZ (Standard) | Aurora Multi-Master | Aurora Global Database |
|---------|---------------------------|-------------------|----------------------|
| **Purpose** | Same-region HA | **Sub-30-sec failover** | Cross-region DR |
| **Failover Time** | <30 seconds (auto) | **<10 seconds** (instant) | <1 minute (manual) |
| **Failover Type** | Automatic | **Automatic** | Manual promotion |
| **Write Nodes** | 1 (single writer) | **Multiple writers** | 1 per region |
| **Use Case** | Standard production | **RTO <30 seconds** | Regional disaster |

### The Question You Missed (Day 8, Q9)

**Scenario:** Trading app needs RTO = 60 seconds, RPO = 5 minutes, automatic failover

**Your Answer:** Aurora Global Database ❌
- Why Wrong: Global Database failover is MANUAL (requires promoting secondary region)
- Doesn't meet "automatic failover without manual intervention" requirement

**Correct Answer:** Aurora Multi-Master ✅
- Multiple write-capable nodes in same region
- If one writer fails, app connects to another writer immediately
- Failover time: <10 seconds (meets 60-second RTO)
- Automatic (no manual promotion needed)

### Key Pattern to Remember

| Requirement | Solution |
|------------|----------|
| **RTO < 60 seconds** + **same region** + **automatic** | Aurora Multi-Master |
| **RTO < 2 minutes** + **cross-region** + **manual OK** | Aurora Global Database |
| **RTO < 2 minutes** + **same region** + **automatic** | Aurora Multi-AZ (standard) |

### Next Steps
- [ ] Read Aurora Multi-Master documentation
- [ ] Understand <10-second failover mechanism
- [ ] Practice RTO/RPO scenario questions

---

## 📊 Weakness #3: RDS Encryption Migration (ACTIVE - MEDIUM PRIORITY)

### Current Performance
- **Accuracy:** 67% (missed on Day 8, Q20)
- **First Identified:** Dec 8
- **Status:** 🟡 Improving

### The Problem Pattern
You chose **DMS (complex)** when **snapshot/restore (simple)** was better within a maintenance window.

### The Decision Tree

```
Need to encrypt existing unencrypted RDS database?
│
├─ CRITICAL: Can you enable encryption on existing instance?
│  └─ NO! ❌ This is IMPOSSIBLE (trap answer)
│
├─ Do you have a maintenance window (downtime acceptable)?
│  │
│  ├─ YES (e.g., 4-hour window)
│  │  └─ Use Snapshot → Encrypted Copy → Restore
│  │     - Simple, well-documented
│  │     - Takes 3-4 hours for 2 TB database
│  │     - FASTEST to implement
│  │
│  └─ NO (must stay online 24/7)
│     └─ Use AWS DMS with CDC
│        - Complex setup (replication instance, endpoints, tasks)
│        - Near-zero downtime
│        - Slower to implement, more moving parts
```

### The Question You Missed (Day 8, Q20)

**Scenario:** Need to encrypt 2 TB RDS PostgreSQL, have 4-hour maintenance window

**Your Answer:** DMS with CDC ❌
- Why Wrong: DMS is for when you CAN'T afford downtime
- Question said "FASTEST way WITHIN maintenance window" (downtime acceptable!)

**Correct Answer:** Encrypted snapshot → restore ✅
- Takes 3-4 hours (fits in window)
- Simplest, most straightforward approach
- Standard AWS-recommended method

### Key Pattern

| Scenario | Method | Why |
|----------|--------|-----|
| **Have maintenance window** | Snapshot/restore | Simple, fast to implement |
| **No downtime allowed** | DMS with CDC | Complex but near-zero downtime |
| **"Fastest within window"** | Snapshot/restore | Fewest steps |

### Next Steps
- [x] Understand: DMS = for no-downtime scenarios
- [x] Remember: Snapshot/restore = standard method when downtime OK
- [ ] Practice encryption migration scenarios

---

## 📊 Weakness #4: DynamoDB Cost Optimization (ACTIVE - MEDIUM PRIORITY)

### Current Performance
- **Accuracy:** 67% (missed on Day 8, Q18)
- **First Identified:** Dec 8
- **Status:** 🟡 Improving

### The Problem Pattern
You chose **DAX (complex + costs money)** when **eventually consistent reads (50% cheaper)** was the answer.

### The Critical Fact You Missed

**DynamoDB Read Consistency Pricing:**
- **Strongly consistent read:** 1 RCU per 4 KB read
- **Eventually consistent read:** **0.5 RCU per 4 KB read** (EXACTLY 50% CHEAPER!)

### The Decision Tree

```
Question about reducing DynamoDB read costs?
│
├─ Option A: Switch to eventually consistent reads
│  - Cost reduction: Exactly 50%
│  - Complexity: Zero (just change read consistency)
│  - Tradeoff: Data might be 1-2 seconds stale
│  - When to use: Data can tolerate slight staleness (leaderboards, catalogs)
│
├─ Option B: Add DAX (cache)
│  - Cost reduction: Variable (depends on cache hit rate)
│  - Adds cost: DAX cluster nodes ($200-700/month)
│  - Net savings: Maybe not 50% (DynamoDB savings - DAX costs)
│  - When to use: Need microsecond latency, performance priority
│
└─ Question asks for "50% cost reduction"?
   └─ Answer = Eventually consistent reads ✅
```

### The Question You Missed (Day 8, Q18)

**Scenario:** Leaderboard with millions of reads, want 50% cost reduction

**Your Answer:** DAX ❌
- Why Wrong: DAX adds infrastructure costs (cluster nodes 24/7)
- Net savings probably NOT 50%

**Correct Answer:** Eventually consistent reads ✅
- Literally 50% cheaper (0.5 RCU vs 1 RCU)
- Simple configuration change (no new infrastructure)
- Leaderboards can tolerate 1-2 second staleness

### Key Pattern

| Goal | Solution | Why |
|------|----------|-----|
| **50% cost reduction on reads** | Eventually consistent | Literal 50% cheaper |
| **Microsecond latency** | DAX | 10x faster but costs money |
| **Simple solution** | Eventually consistent | No new infrastructure |

### Next Steps
- [x] Memorize: Eventually consistent = 50% cheaper (exact)
- [x] Remember: DAX = for performance, not cost reduction
- [ ] Practice DynamoDB cost optimization questions

---

## 📈 Weakness Resolution Timeline

### Week 1 (Nov 21-27): Discovery Phase
- **Identified:** 8 critical weaknesses on Day 7 comprehensive quiz (45%)
- S3 storage classes, VPC NACLs, Encryption/KMS, Auto Scaling, EC2/VPC

### Week 1 Recovery (Dec 5-8): Mastery Phase
- **Resolved:** 6 weaknesses (VPC NACLs, Auto Scaling, Placement Groups, etc.)
- **Improved:** 2 weaknesses (S3: 40%→75%, Encryption: 30%→67%)
- **Result:** 90% on weakness recovery quiz ✅

### Week 2 Day 1 (Dec 8): New Material
- **Identified:** 4 new weaknesses from RDS/Aurora quiz (70%)
- Aurora Multi-Master, RDS encryption migration, DynamoDB cost optimization
- **Pattern:** Week 2 introducing new weak areas as expected

### Expected Pattern
- **Day 1-2 of new topic:** 70-75% (discovery phase)
- **Day 3-5:** Targeted drilling, 80%+ (mastery phase)
- **Day 6:** 90%+ (retention confirmed)

---

## 🎯 Action Plan for Active Weaknesses

### This Week (Dec 8-13)
1. **S3 Storage Classes** (15 min/day drilling)
   - Monday: Re-read decision tree
   - Tuesday: 10-question drill targeting "rarely" vs "very rarely"
   - Wednesday: Retake missed questions from Week 1

2. **Aurora Multi-Master** (part of ongoing Week 2 study)
   - Study when covering advanced Aurora features
   - Practice RTO/RPO scenario questions
   - Understand <10-second failover mechanism

3. **RDS/DynamoDB Patterns** (integrated into Week 2)
   - Will be reinforced during Week 2 Day 2 (DynamoDB deep dive)
   - Practice cost optimization questions

### Success Criteria
- [ ] S3 Storage Classes: 90%+ on 10-question drill
- [ ] Aurora Multi-Master: 100% on RTO/RPO scenarios
- [ ] Cost optimization: Instantly recognize "50% = eventually consistent"

---

## 📝 Lessons Learned from Weakness Resolution

### What Works
1. ✅ **Targeted drilling:** 10-question quizzes on single weakness
2. ✅ **Decision trees:** Visual flowcharts for complex choices
3. ✅ **Pattern recognition:** "Keyword X = Service Y" memorization
4. ✅ **Immediate retake:** Quiz → Review → Retake same day
5. ✅ **100% target:** Don't stop at 80%, push to 100% on drills

### What Doesn't Work
1. ❌ **Passive reading:** Just re-reading without testing
2. ❌ **Moving on at 70%:** Weaknesses compound if not fully resolved
3. ❌ **Batch studying:** Mixing multiple weak topics causes confusion
4. ❌ **Delayed review:** Waiting days before addressing weakness

### Average Time to Resolve Weakness
- **Simple concept** (VPC endpoints): 1 day of drilling → 100%
- **Moderate concept** (VPC NACLs): 3 days of drilling → 100%
- **Complex concept** (S3 storage classes): 8+ days, ongoing → 75%

**Takeaway:** Most weaknesses resolve within 3-5 days with focused attention.

---

## 🚨 Red Flags to Watch For

### Signs a Weakness Needs Immediate Attention
- ⚠️ Scoring <70% on same topic across 2+ quizzes
- ⚠️ Making the SAME mistake on different quiz dates
- ⚠️ Defaulting to wrong pattern (e.g., "rarely" → Glacier without checking retrieval)
- ⚠️ Uncertainty when answering (guessing, not confident)

### Signs a Weakness is Resolving
- ✅ Scoring 80%+ consistently
- ✅ Answering confidently without hesitation
- ✅ Able to explain WHY wrong answers are wrong
- ✅ No repeat mistakes on retakes

---

**Last Updated:** December 9, 2025, 12:30 PM
**Next Review:** December 10, 2025 (after DynamoDB deep dive)

---

## 🎯 Tomorrow's Focus (Dec 10)

1. **URGENT:** 30-min drill on DynamoDB GSI vs LSI
   - 10-question targeted quiz
   - Memorize: GSI = different partition key, LSI = same partition key
   - Target: 9/10 (90%)

2. **DynamoDB Deep Dive** (Week 2 Day 2 content)
   - Partition keys, sort keys, design patterns
   - Capacity modes, pricing, optimization
   - Build on today's strong foundation (85%)

3. **Maintain Momentum**
   - You went 65% → 85% in one day
   - Keep this energy for DynamoDB
   - Target: 16+/20 (80%+) on DynamoDB quiz
