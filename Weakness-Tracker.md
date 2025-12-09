# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** December 8, 2025
**Exam Date:** January 5, 2026 (28 days remaining)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 🎯 Current Active Weaknesses (Need Attention)

| Topic | Current Status | Last Quiz Score | Target | Priority | Next Action |
|-------|---------------|----------------|--------|----------|-------------|
| **S3 Storage Classes** | 🟡 Improving | 75% | 90% | HIGH | Drill "rarely" vs "very rarely" patterns |
| **Aurora Multi-Master** | 🔴 Need Review | 0% | 80% | HIGH | Study fast failover scenarios (<30 sec RTO) |
| **RDS Encryption Migration** | 🟡 Improving | 67% | 80% | MEDIUM | Review snapshot/restore vs DMS decision tree |
| **DynamoDB Consistency Models** | 🟡 Improving | 67% | 80% | MEDIUM | Study eventually consistent = 50% cost savings |

---

## ✅ Resolved Weaknesses (Mastered - 90%+ Accuracy)

| Topic | Resolution Date | Final Score | Days to Master |
|-------|----------------|-------------|----------------|
| **VPC NACLs (Stateless)** | Dec 8, 2025 | 100% | 3 days |
| **Auto Scaling Policy Combinations** | Dec 8, 2025 | 100% | 5 days |
| **EC2 Placement Groups** | Dec 8, 2025 | 100% | 5 days |
| **VPC Endpoints (Gateway vs Interface)** | Dec 8, 2025 | 100% | 5 days |
| **RDS Multi-AZ vs Read Replicas** | Dec 8, 2025 | 100% | 1 day |
| **Aurora Serverless Use Cases** | Dec 8, 2025 | 100% | 1 day |
| **RDS Proxy (Lambda + RDS)** | Dec 8, 2025 | 100% | 1 day |
| **Aurora Backtrack** | Dec 8, 2025 | 100% | 1 day |
| **DynamoDB Streams** | Dec 8, 2025 | 100% | 1 day |
| **QLDB (Immutable Ledger)** | Dec 8, 2025 | 100% | 1 day |

---

## 📊 Weakness #1: S3 Storage Classes (ACTIVE - HIGH PRIORITY)

### Current Performance
- **Accuracy:** 75% (3/4 questions correct on recent quizzes)
- **First Identified:** Nov 27 (Day 7 comprehensive quiz - 4 questions missed)
- **Progress:** 40% → 75% (improved but not mastered)
- **Status:** 🟡 Improving, still need polish

### The Problem Pattern
You keep defaulting to **Glacier** when you see "rarely accessed" WITHOUT checking the retrieval time requirement.

### The Decision Tree You MUST Memorize

```
Question about S3 storage?
│
├─ Step 1: How often accessed?
│  ├─ FREQUENTLY (daily/weekly) → S3 Standard or Intelligent-Tiering
│  ├─ INFREQUENTLY (monthly) → Check retrieval time ↓
│  └─ RARELY (1-2 times/year) → Archive pattern ↓
│
├─ Step 2: What's the retrieval time requirement?
│  ├─ IMMEDIATE (milliseconds, "within seconds") → Standard-IA or Glacier Instant
│  ├─ MINUTES (1-5 min acceptable) → Glacier Flexible (Expedited)
│  ├─ HOURS (3-12 hours acceptable) → Glacier Flexible or Deep Archive
│  └─ NO TIME MENTIONED → Default to Standard-IA (safe choice)
│
└─ Step 3: Cost requirement?
   ├─ "MOST cost-effective" → Pick cheapest that meets above
   └─ Check minimum storage duration (IA=30d, Glacier=90d, Deep=180d)
```

### Critical Keywords to Watch For

| Keyword in Question | What It Actually Means | Correct Answer |
|-------------------|----------------------|----------------|
| "**immediately**" or "**within seconds**" | 0-second retrieval (milliseconds) | Standard-IA, NOT Glacier |
| "**rarely accessed**" (alone) | Monthly access | Standard-IA (if immediate needed) |
| "**very rarely**" or "**1-2 times/year**" | Archive pattern | Glacier family |
| "**can wait 5 minutes**" | Glacier Expedited works | Glacier Flexible |
| "**can wait 12 hours**" | Slowest archive acceptable | Glacier Deep Archive |
| "**unknown access patterns**" | Can't predict when/how often | Intelligent-Tiering |

### Recent Mistakes

**Day 8, Question 10 (Dec 8):**
- **Scenario:** Media files accessed frequently for 30 days, then "very rarely" (1-2 times/year), can wait 5 minutes
- **Your Answer:** Standard-IA ❌
- **Correct Answer:** Glacier Flexible Retrieval
- **Why Wrong:** You saw "rarely" and went to Standard-IA without noticing "VERY rarely (1-2 times/year)" = archive pattern

**The Fix:** Check THREE things before answering:
1. Access frequency (infrequent vs rarely)
2. Retrieval time requirement
3. Cost optimization need

### Next Steps
- [ ] Re-read S3 storage class decision tree (5 min)
- [ ] Take 10-question drill on ONLY S3 storage classes
- [ ] Target: 9/10 (90%)

---

## 📊 Weakness #2: Aurora Multi-Master (ACTIVE - HIGH PRIORITY)

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

**Last Updated:** December 8, 2025, 9:45 AM
**Next Review:** December 9, 2025 (after DynamoDB quiz)
