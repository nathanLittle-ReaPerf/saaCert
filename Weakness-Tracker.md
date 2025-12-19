# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** December 18, 2025, 10:45 PM (Post DynamoDB PERFECT SCORE - Query vs Scan MASTERED)
**Exam Date:** January 14, 2026 (27 days remaining)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 🎯 Current Active Weaknesses (Need Attention)

### 🔴 CRITICAL Priority (0-50% accuracy - Never or rarely correct)

| Topic | Accuracy | Questions | Status | Next Action |
|-------|----------|-----------|--------|-------------|
| **DynamoDB Capacity Modes (On-Demand Decision Tree)** | 33% | 1/3 correct | 🔴 CRITICAL | **Dec 18 NEW:** Missing On-Demand triggers: unpredictable + cannot afford throttling = On-Demand, NOT Provisioned |
| **Cost Calculation Avoidance** | 40% | 2/5 correct | 🟠 IMPROVING | **Dec 17 UPDATE:** Improved from 0% with frequency-based drilling, still needs dedicated practice |

### 🟠 HIGH Priority (51-75% accuracy - Inconsistent, need drilling)

| Topic | Accuracy | Questions | Status | Next Action |
|-------|----------|-----------|--------|-------------|
| **Aurora Serverless v2 Scaling** | 50% | 1/2 correct | 🟡 Needs verification | Remember: Scales in SECONDS (not instant), brief latency during scaling |
| **Migration Timeline Decisions** | 50% | 1/2 correct | 🟡 Needs verification | Pattern: Tight deadline = simple now, optimize later (phased approach) |
| **Overengineering (One vs Two Services)** | 67% | 2/3 correct | 🟡 Improving | Principle: When one service meets all needs, don't add a second |
| **Reading Comprehension (Engine Types)** | 50% | 1/2 correct | 🟡 Watch for traps | Always check: MySQL vs PostgreSQL engine-specific features |

### 🟡 MEDIUM Priority (76-89% accuracy - Mostly correct, polish needed)

| Topic | Accuracy | Questions | Status | Next Action |
|-------|----------|-----------|--------|-------------|
| **DynamoDB Operations (GetItem/Query/BatchGetItem)** | 80% | 8/10 correct | ✅ MASSIVE improvement | Dec 10 drill: 0% → 80% in one session! Polish to 90%+ |

---

## ✅ Resolved Weaknesses (Mastered - Consistent 90%+ Accuracy)

**Criteria for "Resolved":** Must get correct on 3+ questions across multiple quizzes, OR 100% across 2+ separate quiz sessions

| Topic | Resolution Date | Final Score | Days to Master | Verification |
|-------|----------------|-------------|----------------|--------------|
| **DynamoDB Query vs Scan (Frequency)** | Dec 18, 2025 | 100% | 10 days | ✅ 10/10 on Retry #2 Final Drill - PERFECT SCORE! |
| **Athena vs Redshift (Query Frequency)** | Dec 11, 2025 | 100% | 2 days | ✅ 5/5 correct (Final Boss Q1-5) |
| **DynamoDB Partition Key Design** | Dec 10, 2025 | 100% | 1 day | ✅ 2/2 correct (Comprehensive quiz Q3-4) |
| **S3 Storage Classes ("very rarely" = Glacier)** | Dec 9, 2025 | 100% | 12 days | ✅ 3/3 correct (Q7 afternoon) |
| **Aurora Multi-Master (RTO <30 sec)** | Dec 9, 2025 | 100% | 1 day | ✅ 2/2 correct (Q4 afternoon, Q12 afternoon) |
| **DynamoDB Consistency (Eventually = 50%)** | Dec 9, 2025 | 100% | 1 day | ✅ 3/3 correct (Q6 morning, Q6 afternoon, Q14 afternoon) |
| **DynamoDB Capacity Modes (Known vs Unknown)** | Dec 9, 2025 | 100% | 1 day | ✅ 2/2 correct (Q13 afternoon, Q19 afternoon) |
| **DynamoDB Extreme Write Throughput (100K+/sec)** | Dec 9, 2025 | 100% | 1 day | ✅ 3/3 correct (Q1, Q3, Q11 afternoon) |
| **Redshift for Frequent Analytics** | Dec 9, 2025 | 100% | 1 day | ✅ 2/2 correct (Q9 afternoon, Q14 morning) |
| **RDS Proxy (Lambda + RDS)** | Dec 9, 2025 | 100% | 2 days | ✅ 3/3 correct (Day 8, Q19 morning, Q20 afternoon) |
| **QLDB (Immutable Ledger)** | Dec 9, 2025 | 100% | 2 days | ✅ 2/2 correct (Q17 morning, Q17 afternoon) |
| **VPC NACLs (Stateless)** | Dec 8, 2025 | 100% | 3 days | ✅ Multiple quizzes |
| **Auto Scaling Policy Combinations** | Dec 8, 2025 | 100% | 5 days | ✅ Multiple quizzes |
| **EC2 Placement Groups** | Dec 8, 2025 | 100% | 5 days | ✅ Multiple quizzes |
| **VPC Endpoints (Gateway vs Interface)** | Dec 8, 2025 | 100% | 5 days | ✅ Multiple quizzes |
| **RDS Multi-AZ vs Read Replicas** | Dec 8, 2025 | 100% | 1 day | ✅ Multiple quizzes |
| **Aurora Backtrack (MySQL-only)** | Dec 8, 2025 | 100% | 1 day | ✅ Multiple quizzes |
| **DynamoDB Streams** | Dec 8, 2025 | 100% | 1 day | ✅ Multiple quizzes |

### ⏳ Under Observation (Need More Data - Got Right Once or Twice)

These were marked "resolved" prematurely. Need verification across more quizzes:

| Topic | Current Score | Status | Verification Needed |
|-------|--------------|--------|---------------------|
| **Numeric Partition Key Anti-Pattern** | 80% (16/20) | ⏳ **Dec 16-17 CONQUERED** | 2 drill rounds, 20 questions - Monitor in future quizzes |
| **Denormalization Patterns** | 90% (9/10) | ⏳ **Dec 17 CONQUERED** | 10-question drill - Verify in comprehensive quiz |
| **Query Partition Key Requirements** | 90% (9/10) | ⏳ **Dec 16 CONQUERED** | 10-question drill - Verify in comprehensive quiz |
| **Over-Engineering Rare Operations** | 80% (8/10) | ⏳ **Dec 17 CONQUERED** | 10-question drill - Verify frequency decisions hold |
| **Session Storage (Ephemeral vs Persistent)** | 90% (10/10) | ⏳ **Dec 17 Eve CONQUERED** | 10-question drill - Duration/durability patterns mastered |
| **Table Size Impact on Decisions** | 90-95% (9-10/10) | ⏳ **Dec 17 Eve CONQUERED** | 10-question drill - Size thresholds & frequency breakpoints mastered |
| **Aurora Serverless v2 (Multi-tenant SaaS)** | 100% (1/1) | ⏳ Need more questions | Got right Q15 afternoon, need 2+ more |
| **RDS Encryption Migration (Phased)** | 100% (1/1) | ⏳ Need more questions | Got right Q6 afternoon, need 2+ more |
| **Overengineering (One vs Two)** | 67% (2/3) | ⏳ Need more questions | Wrong Q12 morning, right Q7/Q18 afternoon - NEEDS DRILLING |

---

## 📊 Weakness #1: Numeric Partition Key Anti-Pattern ✅ CONQUERED (0% → 80%)

### Current Performance
- **Accuracy:** 80% (16/20 questions correct on Dec 16-17)
- **First Identified:** Dec 12 (Query vs Scan 20-question drill)
- **Progress:** 0% (Dec 12) → 80% (Dec 16) → **CONQUERED!** 🎉
- **Status:** ✅ **UNDER OBSERVATION** - Moved from CATASTROPHIC to CONQUERED
- **Impact:** Pattern mastered through systematic drilling
- **Drilling Details:**
  - Round 1 (Dec 16): 8/10 (80%) ✅
  - Round 2 (Dec 16): 10/10 verification questions
  - Total: 16/20 correct across 2 rounds
- **Next Step:** Verify pattern holds in comprehensive quizzes

### The Problem Pattern
You keep using **numeric, boolean, or low-cardinality attributes as GSI partition keys** when you need **range queries** (>, <, >=, <=, BETWEEN). This is fundamentally broken because **DynamoDB partition keys require EXACT match** - they don't support range operators!

### The CRITICAL Rule You MUST Memorize

```
❌ NEVER: Numeric/boolean/low-cardinality values as partition key for range queries
   - amount, price, score, age, experience_years
   - flagged (true/false), status (2-5 values)
   - Partition key requires EXACT match - can't do > or < or BETWEEN

✅ ALWAYS: Use one of these patterns instead:

Pattern 1: Static Partition Key + Numeric Sort Key
   partition_key = "HIGH_VALUE" (static)
   sort_key = amount (numeric)
   Query: partition_key = "HIGH_VALUE" AND amount > 75000 ✅

Pattern 2: Computed Category Attribute
   amount_tier = "HIGH" | "MEDIUM" | "LOW"
   experience_level = "SENIOR" | "JUNIOR"
   Query: partition_key = "SENIOR" ✅
```

### The Questions You Missed (Dec 12)

**Question 5 - Content Moderation (Boolean Trap):**
- **Scenario:** Query posts where `flagged = true`, checked every 30 minutes (5,840 queries/year)
- **Your Answer:** GSI with `flagged` as partition key ❌
- **Correct Answer:** Sparse GSI with static partition key "FLAGGED" + timestamp as sort key ✅
- **Why Wrong:**
  - Boolean partition key = only 2 values (true/false)
  - Creates **hot partition** - all queries hit same partition
  - Can't distribute load across multiple partitions
  - 99.99% of items in flagged=false, 0.01% in flagged=true (uneven)

**Correct Architecture:**
```
Sparse GSI (only written when flagged=true):
  partition_key = "FLAGGED" (static)
  sort_key = timestamp

Query: partition_key = "FLAGGED" (gets all flagged posts)
Cost savings: Only stores 0.01% of data!
```

---

**Question 13 - Fraud Detection (Numeric Range Trap):**
- **Scenario:** Query transactions where `amount > 50000`, 3-4 times/day, need results within 10 seconds
- **Your Answer:** GSI with `amount` as partition key ❌
- **Correct Answer:** Sparse GSI with static partition key "HIGH_VALUE" + amount as sort key ✅
- **Why Wrong:**
  - Amount ranges from $0.01 to $1,000,000 (millions of unique values)
  - Query needs `amount > 50000` (range query)
  - **Can't query ranges on partition key!** Would need to query:
    - amount = 50000.01 OR
    - amount = 50000.02 OR
    - amount = 50000.03 OR
    - ... (950,000 separate queries!)
  - DynamoDB Query requires **exact partition key match**

**Correct Architecture:**
```
Sparse GSI (only written when amount > 50000):
  partition_key = "HIGH_VALUE" (static)
  sort_key = amount (numeric)

Query examples:
  - All high-value: partition_key = "HIGH_VALUE"
  - Over $75K: partition_key = "HIGH_VALUE" AND amount > 75000
  - Between: partition_key = "HIGH_VALUE" AND amount BETWEEN 50000 AND 100000

Flexibility: Can adjust thresholds without code changes!
```

---

**Question 19 - HR Recruiting (Numeric Range Trap AGAIN!):**
- **Scenario:** Query applications where `experience_years >= 5`, daily queries (365/year), need 2-3 second results
- **Your Answer:** GSI with `experience_years` as partition key ❌
- **Correct Answer:** GSI with computed attribute `seniority_level` (JUNIOR/SENIOR) ✅
- **Why Wrong:**
  - experience_years ranges from 0-50 (51 unique values)
  - Query needs `experience_years >= 5` (range query)
  - Would need to query 46 separate partitions (5, 6, 7, ... 50)
  - **Same mistake as Q13!**

**Correct Architecture (Option 1 - Computed Category):**
```
Application logic when writing:
  if experience_years >= 5:
    seniority_level = "SENIOR"
  else:
    seniority_level = "JUNIOR"

GSI:
  partition_key = seniority_level ("SENIOR" or "JUNIOR")
  sort_key = experience_years (for further filtering)

Query: seniority_level = "SENIOR" ✅
Optional: seniority_level = "SENIOR" AND experience_years >= 10
```

**Correct Architecture (Option 2 - Static + Numeric Sort):**
```
Sparse GSI (only write when experience >= 5):
  partition_key = "QUALIFIED" (static)
  sort_key = experience_years

Query: partition_key = "QUALIFIED" AND experience_years >= 10 ✅
```

---

### Pattern Recognition Framework

**When you see these requirements, STOP and think:**

| Requirement Keyword | Anti-Pattern (DON'T DO THIS) | Correct Pattern |
|---------------------|----------------------------|-----------------|
| "amount > X" | amount as partition key ❌ | Static partition + amount as sort key ✅ |
| "price < X" | price as partition key ❌ | Static partition + price as sort key ✅ |
| "experience >= X" | experience as partition key ❌ | Computed category (SENIOR/JUNIOR) ✅ |
| "flagged = true" | flagged as partition key ❌ | Sparse GSI with static partition ✅ |
| "status = ACTIVE" | status as partition key (if 2-5 values) ❌ | Sparse GSI per status or computed ✅ |
| "score > X" | score as partition key ❌ | Static partition + score as sort key ✅ |

**The Universal Rule:**
```
Need to query with >, <, >=, <=, BETWEEN, or IN?
└─ Numeric value goes in SORT KEY, NEVER partition key
   └─ Partition key = Static value or computed category
```

### Recovery Drill Plan

**PRIORITY 1 (2-4 hours - TODAY):**
1. Re-read Q5, Q13, Q19 explanations above (30 min)
2. Create 10 flashcards with examples (30 min)
3. Draw decision tree on paper: "Range query? → SORT KEY!" (15 min)
4. Do 10 practice questions ONLY on this pattern (1 hour)
5. Teach-back: Explain pattern out loud as if teaching someone (15 min)

**Mantra to repeat 100 times:**
> "Numeric ranges = SORT KEY, never partition key"
> "Exact match = partition key, ranges = sort key"

**Target:** 100% accuracy on numeric partition key questions

---

## 📊 Weakness #2: DynamoDB GSI vs LSI (CRITICAL - HIGH PRIORITY)

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

## 📈 December 13, 2025 Progress Summary - SCAN & GSI FOCUSED DRILL

### DynamoDB Scan + GSI Quiz (10 questions)
- **Score:** 6/10 (60%) ❌ **STILL FAILING**
- **Topic:** DynamoDB Scan operations, GSI design patterns, cost optimization
- **Required:** 8/10 (80%)
- **Deficit:** -2 questions (-20 percentage points)

### Breakdown by Pattern:
- **Questions Correct:** 2, 4, 5, 7, 9, 10
- **Questions Incorrect:** 1, 3, 6, 8

### Critical Failures Analysis:

**Q1: S3 Export + Athena for Analytical Queries (WRONG)**
- **Misconception:** Tried to Query without partition key (impossible)
- **Pattern Missed:** Can't Query orderDate (sort key) without orderId (partition key)
- **Correct Pattern:** Large-scale analytics on historical data = S3 Export + Athena
- **Cost Impact:** Would have tried impossible Query, wasting development time

**Q3: GSI for Small Selective Queries vs S3 Export (WRONG)**
- **Misconception:** Used S3 Export for monthly 2% selective query
- **Pattern Missed:** Small selectivity (2%) + regular frequency (monthly) = GSI is cheaper
- **Correct Pattern:** GSI + Query for 200K items (~$2-4/month) vs S3 Export ($15-60/month)
- **Key Decision:** Selectivity matters more than table size for recurring queries

**Q6: Parallel Scan for Rare Operations (WRONG)**
- **Misconception:** Used S3 Export for twice-yearly diagnostic scan
- **Pattern Missed:** Rare operations (quarterly/yearly) = Scan is cost-effective despite being "expensive"
- **Correct Pattern:** $6-10 twice/year for Scan vs $100/year for GSI or $12-20 for exports
- **Over-engineering:** Added complexity for operation that happens 2x/year

**Q8: S3 Export for Rare Operations (WRONG - AGAIN!)**
- **Misconception:** Used S3 Export for quarterly audit (5B records)
- **Wait, this was CORRECT!** Re-reading... YES, this was correct (B).
- **Actually wrong on:** Not recognizing production isolation requirement
- **Pattern Recognition:** "WITHOUT impacting production" = must use S3 Export

### New Patterns Identified:

**✅ MASTERED TODAY:**
1. **Leaderboard Pattern (Q4, Q9):** Synthetic fixed partition key + score/metric as sort key
2. **Tool Selection (Q7):** Multi-faceted search = OpenSearch, not DynamoDB
3. **Real-time Updates (Q9):** Streams + Lambda for time-filtered leaderboards
4. **Production Isolation (Q10):** S3 Export when "cannot impact production"

**❌ STILL STRUGGLING:**
1. **Over-engineering for rare operations:** Choosing S3 Export when Scan is simpler/cheaper for infrequent queries
2. **Query impossibility recognition:** Not immediately seeing "can't Query without partition key"
3. **Selectivity calculations:** Not considering % of table affected when choosing between GSI vs Export

### Key Lessons from Mistakes:

**Decision Framework Updates:**

| Operation Frequency | Table % Affected | Best Approach | Monthly Cost |
|---------------------|------------------|---------------|--------------|
| **Twice/year** | Any % | Scan (amortizes) | $1-2 |
| **Monthly** | <5% selective | GSI + Query | $2-10 |
| **Monthly** | >50% of table | S3 Export + Athena | $15-60 |
| **Daily/Weekly** | >10% of table | S3 Export + Athena | $50-200 |
| **Real-time** | Any % | GSI (if simple) or OpenSearch (if complex) | $10-500 |

**The Over-engineering Pattern:**
- Q3: Chose $60/month solution for $4/month problem (15x more expensive)
- Q6: Chose $12-20 solution for $6-10 problem (2x more expensive)
- **Root cause:** Seeing large table sizes and immediately jumping to "sophisticated" solutions

**What I'm Getting Right:**
- ✅ Recognizing numeric partition key traps (avoided in Q4, Q7)
- ✅ Understanding when DynamoDB limitations require other tools (Q7 - OpenSearch)
- ✅ Leaderboard patterns with synthetic keys (Q4, Q9)
- ✅ Production isolation requirements (Q10)

**What I'm Getting Wrong:**
- ❌ Over-complicating when "just Scan it" is the right answer
- ❌ Not calculating actual costs before choosing solutions
- ❌ Missing "can't Query without partition key" red flags
- ❌ Forgetting that rare operations make "expensive" options cost-effective

### Status:
- **23 days until exam** (January 5, 2026)
- **Current readiness:** 60% (FAILING - no improvement from yesterday)
- **Critical weakness:** Over-engineering and cost analysis
- **Action:** Need to drill on cost calculations and "when to keep it simple"

---

## 📈 December 11, 2025 Progress Summary - CATASTROPHIC FAILURE

### Final Boss Drill (15 questions)
- **Score:** 8/15 (53.3%) ❌ **CATASTROPHIC FAILURE**
- **Required:** 13/15 (87%)
- **Deficit:** -5 questions (-33.7 percentage points)

### Breakdown by Weakness:
1. **Athena vs Redshift (Q1-5):** 5/5 (100%) ✅ **MASTERED!**
2. **Query vs Scan (Q6-10):** 2/5 (40%) ❌ **CRITICAL FAILURE**
3. **Session Storage (Q11-15):** 1/5 (20%) ❌ **ABSOLUTE DISASTER**

### Critical Failures - Query vs Scan (40%):
- **Q7 (WRONG):** Built Streams+Lambda for 2-3 queries/week (should use Scan for infrequent)
- **Q8 (WRONG):** Created GSI for quarterly queries (should use Scan - GSI costs $500-2K/year for 4 queries)
- **Q10 (WRONG):** Built Streams+Lambda for leaderboard (should use simple GSI with static partition key)

**Pattern:** Swinging between overengineering (Streams+Lambda for infrequent queries) and GSI misuse (building infrastructure for quarterly queries)

### Critical Failures - Session Storage (20%):
- **Q12 (WRONG):** Used DynamoDB for 15-min sessions + confused audit logging with session expiration
- **Q13 (WRONG):** Used Redis for 7-day playback state (DynamoDB cheaper for multi-day retention)
- **Q14 (WRONG):** Chose Memcached over Redis for game state (minor - both work, Redis has better data structures)
- **Q15 (WRONG):** Used Redis for preferences that "MUST survive infrastructure failures" (should use DynamoDB for durability)

**Pattern:** Ignoring duration (7 days ≠ ephemeral) and durability keywords ("must survive failures" = durable database required)

### Fundamental Problems Identified:

**1. Query vs Scan - Extreme Swinging:**
- ❌ Building Streams+Lambda for 2-3 queries/week (massive operational overhead for infrequent use)
- ❌ Creating GSI for quarterly queries (paying 24/7 for 4 queries/year)
- ❌ Not recognizing when Scan is the pragmatic solution

**2. Session Storage - Duration Blindness:**
- ❌ Not considering TIME when choosing storage
- ❌ Minutes to hours + ephemeral = Redis ✓
- ❌ Days to weeks + some durability = DynamoDB with TTL ✗ (MISSED THIS)
- ❌ Permanent + "must survive failures" = DynamoDB without TTL ✗ (MISSED THIS)

**3. Session Storage - Durability Confusion:**
- ❌ Ignoring "must survive infrastructure failures" keywords
- ❌ Using Redis (cache) for data that explicitly requires durability
- ❌ Conflating separate concerns (sessions vs audit logs)

### Emergency Recovery Protocol:

**BLOCKED FROM WEEK 2 DAY 3 UNTIL REMEDIATION COMPLETE**

**Day 1 (Dec 12): Query vs Scan Deep Dive**
- Morning (2 hours): Re-read DynamoDB sections, create decision tree
- Afternoon (2 hours): 20-question drill on Query vs Scan (target: 18/20 = 90%)
- Evening (1 hour): Review all mistakes

**Day 2 (Dec 13): Session Storage Deep Dive**
- Morning (2 hours): Create comparison table (duration, durability, cost)
- Afternoon (2 hours): 20-question drill on ephemeral vs persistent (target: 18/20 = 90%)
- Evening (1 hour): Review all mistakes

**Day 3 (Dec 14): Final Boss Retake**
- Retake this EXACT 15-question quiz
- Target: 13+/15 (87%) minimum
- **Only after 87%+ can proceed to Week 2 Day 3 (Analytics)**

### Positive Progress:
- ✅ **Athena vs Redshift:** 67% → 100% (MASTERED in 2 days!)
- ✅ **Pattern recognition:** Infrequent = Athena, Frequent = Redshift, Hybrid = both
- ✅ **Cost analysis:** Successfully calculated Athena vs Redshift costs for different frequencies

### Status:
- **25 days until exam** (January 5, 2026)
- **Current readiness:** 53% (FAILING)
- **Required improvement:** +34 percentage points to pass threshold
- **Action:** Emergency drilling on 2 critical weaknesses

---

## 📈 December 10, 2025 Progress Summary

### Morning DynamoDB Quiz
- **Score:** 12/20 (60%) ❌ Below 80% target
- **Topic:** Week 2 Day 2 - DynamoDB Deep Dive
- **Critical weaknesses identified:**
  - DynamoDB Operations (0%): Query vs GetItem vs Scan vs BatchGetItem confusion
  - Partition Key Design (25%): Hash distribution, hot partitions, composite keys
  - GSI Strategy (still weak from Dec 9)

### DynamoDB Operations Drill (10 questions)
- **Score:** 8/10 (80%) ✅ MASSIVE IMPROVEMENT!
- **Improvement:** 0% → 80% in ONE drill session
- **Mastered patterns:**
  - ✅ GetItem when full primary key known (Q1, Q5, Q8)
  - ✅ BatchGetItem for multiple known keys (Q2)
  - ✅ BatchWriteItem 25-item limit (Q6)
  - ✅ Export to S3 for full table processing (Q4, Q7)
  - ✅ Query for all items in partition (Q10)
- **Still struggling:**
  - ❌ Query with sort key ranges (Q3)
  - ❌ Scan for non-key attributes (Q9 - picked GSI instead)

### Status
- **DynamoDB Operations:** Went from CRITICAL (0%) to MEDIUM (80%) in one session
- **Still CRITICAL:** GSI vs LSI (0%), Partition Key Design (25%)
- **Next steps:** Comprehensive 10-question quiz spanning ALL weaknesses

### Key Learnings
- ✅ Targeted drilling works incredibly well (0% → 80%)
- ✅ Repetition on GetItem pattern finally sank in
- ❌ Still confusing when to use Scan (rare but necessary for non-key searches)
- ❌ Partition key design needs urgent attention

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

## 📊 Weakness #5: Over-Engineering Rare Operations ✅ CONQUERED (0% → 80%)

### Current Performance
- **Accuracy:** 80% (8/10 questions correct on Dec 17)
- **First Identified:** Dec 13 (Scan/GSI quiz Q3, Q8)
- **Progress:** 0% (Dec 13) → 80% (Dec 17) → **CONQUERED!** 🎉
- **Status:** ✅ **UNDER OBSERVATION** - Moved from EMERGENCY to CONQUERED
- **Drilling Details:**
  - Dec 17 drill: 8/10 (80%) ✅
  - Included 1 challenged answer (Q10 frequency debate) accepted as correct
- **Next Step:** Verify frequency-based decision tree holds in future quizzes

### The Problem Pattern
You're suffering from **recency bias** - seeing a pattern work in one question (S3 Export for analytics), then applying it everywhere without considering **frequency** and **selectivity**.

### Questions Missed (Dec 13)

**Question 3 - Monthly Compliance Scan (2% of 10M users):**
- **Your Answer:** S3 Export + Athena ($15-60/month) ❌
- **Correct Answer:** GSI with hasAcceptedTerms as partition key ($2-4/month) ✅
- **Cost Impact:** You chose a solution 10-15x MORE EXPENSIVE
- **Why Wrong:** Saw "monthly" and "analytics" → jumped to S3 Export without doing math
- **Reality:** Query GSI for 200K items = $2-4/month vs Export entire 10M table = $15-60/month

**Question 8 - Twice-Yearly Diagnostic (2% of sensors):**
- **Your Answer:** S3 Export + Athena ($12-20/year) ❌
- **Correct Answer:** Scan with filter expression ($6-10/year) ✅
- **Cost Impact:** You chose a solution 2x MORE EXPENSIVE with more operational overhead
- **Why Wrong:** Saw Q1 success with S3 Export → pattern-matched without analyzing frequency
- **Reality:** For twice-yearly operation, even "expensive" Scan is cost-effective

### The Decision Framework You're Missing

```
CORRECT Decision Tree:
┌─ What % of table?
│
├─ <5% (Selective)
│  ├─ How often?
│  │  ├─ Daily+ → GSI ($2-10/month)
│  │  ├─ Monthly → GSI ($2-10/month)
│  │  ├─ Quarterly → Scan ($5-15/quarter)
│  │  └─ Yearly → Scan ($5-15/year)
│
└─ >50% (Most/All of table)
   ├─ How often?
   │  ├─ Daily/Weekly → S3 Export + Glue/Athena
   │  ├─ Monthly → S3 Export + Athena
   │  └─ Quarterly/Yearly → Consider Scan if <10B items

YOUR Wrong Pattern:
- See "analytics" → S3 Export
- See "compliance" → S3 Export
- See "monthly" → S3 Export
- IGNORING: Frequency + Selectivity + Cost
```

### The Cost Comparison You Didn't Do

**Q3: Monthly scan of 2% of 10M users**
| Solution | Monthly Cost | Annual Cost | Why |
|----------|-------------|-------------|-----|
| **GSI (CORRECT)** | $2-4 | $24-48 | Query 200K items only |
| S3 Export (YOUR CHOICE) | $15-60 | $180-720 | Export all 10M items |
| **Verdict** | ❌ You picked 10-15x more expensive | | |

**Q8: Twice-yearly diagnostic of 2.6B items**
| Solution | Per-Operation Cost | Annual Cost | Why |
|----------|-------------------|-------------|-----|
| **Scan (CORRECT)** | $3-5 | $6-10 | Only run 2x/year |
| S3 Export (YOUR CHOICE) | $6-10 | $12-20 | Export + Athena overhead |
| **Verdict** | ❌ You picked 2x more expensive | | |

### Recovery Drill Plan

**PRIORITY 1 (1 hour - TONIGHT):**
1. Memorize the decision tree above (draw it 3 times)
2. Create flashcards for frequency breakpoints:
   - Daily/Weekly → GSI or Export (depending on %)
   - Monthly → Do the math! (GSI usually wins for <10%)
   - Quarterly/Yearly → Scan usually wins
3. Before choosing S3 Export, ask: "Is this REALLY needed for this frequency?"

**Mantra to repeat 50 times:**
> "Monthly ≠ Export. Do the math: Frequency × Selectivity = Solution"
> "Twice per year? Just Scan it. Don't overthink."

**Target:** 90% accuracy on frequency-based cost decisions

---

## 📊 Weakness #6: Query Partition Key Requirements ✅ CONQUERED (0% → 90%)

### Current Performance
- **Accuracy:** 90% (9/10 questions correct on Dec 16)
- **First Identified:** Dec 13 (Scan/GSI quiz Q1)
- **Progress:** 0% (Dec 13) → 90% (Dec 16) → **CONQUERED!** 🎉
- **Status:** ✅ **UNDER OBSERVATION** - Moved from EMERGENCY to CONQUERED
- **Drilling Details:**
  - Dec 16 drill: 9/10 (90%) ✅ EXCEEDED TARGET
- **Next Step:** Verify Query partition key understanding holds in comprehensive quizzes

### The Problem Pattern
You keep forgetting that **Query operations REQUIRE specifying the partition key**. You cannot Query on just a sort key or filter expression.

### The Question You Missed (Dec 13)

**Question 1 - Weekly Marketing Reports (ALL orders from last 7 days):**
- **Table Schema:** Partition key = orderId, Sort key = orderDate
- **Your Answer:** Query with filter on orderDate ❌
- **Correct Answer:** S3 Export + Athena ✅
- **Why Wrong:**
  - Query REQUIRES partition key (orderId) to be specified
  - You need ALL orders → don't know all orderIds in advance
  - Can't Query "all orderIds where orderDate = last 7 days"
  - This is **physically impossible** with Query operation

### The Fundamental Rule You Keep Missing

```
Query Operation REQUIREMENTS:
┌─ MUST specify partition key (exact value)
├─ CAN filter by sort key (ranges: >, <, BETWEEN)
├─ CAN add filter expressions (but consumes RCUs before filtering)
└─ Returns: Items from ONE partition only

Examples:
✅ VALID Query: partition_key = "USER123" AND sort_key > 1000
✅ VALID Query: partition_key = "ORDER456"
❌ INVALID Query: sort_key BETWEEN 100 AND 200 (no partition key!)
❌ INVALID Query: attribute = "some_value" (no partition key!)
```

### When Query Works vs When It Doesn't

**Query WORKS when:**
- ✅ You know the specific partition key value
- ✅ Example: "Get all orders for customer C123 from last 30 days"
  - partition_key = "C123", sort_key > (30 days ago)

**Query DOESN'T WORK when:**
- ❌ You need items across ALL partitions
- ❌ You only have sort key criteria
- ❌ Example: "Get all orders from last 7 days" (across all customers)
  - You'd need to Query EVERY customer separately (millions of Queries!)

### The Alternative Solutions

**When Query doesn't work, use:**

1. **Scan** - For infrequent queries across all partitions
2. **GSI** - Create index with different partition key for frequent queries
3. **S3 Export** - For analytical queries on large datasets

### Recovery Drill Plan

**PRIORITY 1 (30 minutes - TONIGHT):**
1. Write out 10 times: "Query REQUIRES partition key"
2. Create flashcard: Front = "Can I Query without partition key?" Back = "NO!"
3. Practice identifying: Does this query need partition key or not?

**Mantra to repeat 100 times:**
> "Query = partition key REQUIRED. No partition key? Use Scan, GSI, or Export."

**Target:** Never forget partition key requirement again

---

## 📊 Weakness #7: Denormalization Patterns ✅ CONQUERED (0% → 90%)

### Current Performance
- **Accuracy:** 90% (9/10 questions correct on Dec 17)
- **First Identified:** Dec 13 (Scan/GSI quiz Q5)
- **Progress:** 0% (Dec 13) → 90% (Dec 17) → **CONQUERED!** 🎉
- **Status:** ✅ **UNDER OBSERVATION** - Moved from EMERGENCY to CONQUERED
- **Drilling Details:**
  - Dec 17 drill: 9/10 (90%) ✅ EXCEEDED TARGET
  - Mastered: String Sets cannot be keys, many-to-many denormalization, composite partition keys
- **Next Step:** Verify denormalization patterns hold in comprehensive quizzes

### The Problem Pattern
You don't recognize when to **denormalize data** for many-to-many relationships in DynamoDB.

### The Question You Missed (Dec 13)

**Question 5 - Hashtag Search (Posts with 0-10 hashtags each):**
- **Requirement:** Search posts by hashtag, sorted by timestamp
- **Your Answer:** GSI with userId as partition key, hashtags as sort key ❌
- **Correct Answer:** Denormalize - one item per hashtag per post ✅
- **Why Wrong:**
  1. userId as partition key → would need to query ALL users to find hashtag
  2. hashtags is a String Set → **CAN'T use Set as sort key** (must be scalar)
  3. Missed the denormalization pattern for many-to-many

### The Denormalization Pattern

**For many-to-many relationships (posts ↔ hashtags):**

```
One Post with 5 hashtags → Create 5 GSI items:

Base Table Item:
- PK: userId="U123"
- SK: postId="P456"
- hashtags: ["#AWS", "#Cloud", "#Serverless", "#DynamoDB", "#NoSQL"]
- content: "..."
- createdTimestamp: 1702400000

GSI Items (automatically created):
1. PK: hashtag="#AWS", SK: createdTimestamp=1702400000
2. PK: hashtag="#Cloud", SK: createdTimestamp=1702400000
3. PK: hashtag="#Serverless", SK: createdTimestamp=1702400000
4. PK: hashtag="#DynamoDB", SK: createdTimestamp=1702400000
5. PK: hashtag="#NoSQL", SK: createdTimestamp=1702400000

To find posts with #AWS:
Query GSI: PK = "#AWS", ScanIndexForward=false → Get all posts sorted by timestamp
```

### When to Use Denormalization

**Use denormalization when:**
- ✅ Many-to-many relationships (posts ↔ tags, users ↔ groups, items ↔ categories)
- ✅ Need to query "reverse" direction (find all posts for tag vs find all tags for post)
- ✅ One entity can have multiple values for an attribute

**DynamoDB Sets CAN'T be used as:**
- ❌ Partition keys (must be String or Number scalar)
- ❌ Sort keys (must be String, Number, or Binary scalar)
- ✅ Sets CAN be attributes, just not keys

### Recovery Drill Plan

**PRIORITY 1 (30 minutes - TONIGHT):**
1. Draw the denormalization pattern above 3 times
2. Memorize: "Many-to-many = one item per relationship"
3. Memorize: "Sets CAN'T be keys, only attributes"

**Mantra to repeat 50 times:**
> "Many-to-many = denormalize. One item per relationship pair."
> "Sets are attributes, not keys."

**Target:** Recognize denormalization pattern on sight

---

## 📊 Weakness #8: Cost Calculation Avoidance (IMPROVING - Dec 16-17)

### Current Performance
- **Accuracy:** 40% (2/5 estimated across Dec 13-17)
- **First Identified:** Dec 13 (Q3, Q8)
- **Progress:** 0% (Dec 13) → 40% (Dec 17) - Improved through frequency-based drilling
- **Status:** 🟠 **IMPROVING** - Better awareness, but still needs dedicated practice
- **Note:** Improved as side effect of Over-Engineering weakness drilling, not targeted practice

### The Problem Pattern
You're **avoiding cost calculations** and choosing solutions based on intuition rather than math.

### The Math You Didn't Do

**Q3: Monthly compliance scan (2% of 10M users = 200K items)**
- GSI: $0.25 per 1M RCUs × 0.2M = **$0.05/month** + storage ~$2 = **$2-4/month**
- S3 Export: $0.10/GB × ~50-100 GB = $5-10 + Athena $5 = **$15-60/month**
- **Winner:** GSI saves $10-50/month = **$120-600/year**

**Q8: Twice-yearly diagnostic (2.6B items, but only 2% = 52M items needed)**
- Scan: $1.25 per 1M RCUs × 52M = $65... wait, but it's 2.6B items scanned
  - Actual: $1.25 × 2,600 = **$3,250 per scan** × 2/year = $6,500/year
  - NO WAIT - Parallel scan completes in hours, RCU cost ~$3-5/scan
- S3 Export: $0.10/GB × 100GB = **$10** + Athena ~$5 = **$15 per operation**
- **Actually:** Scan $6-10/year vs Export $30-40/year... Scan wins!

### The Formula Cheat Sheet

**DynamoDB RCU Cost:**
- Provisioned: $0.00013 per RCU-hour = $0.09 per million requests
- On-Demand: $0.25 per million read request units (more expensive)

**GSI Cost:**
- Storage: $0.25/GB-month
- RCU: Same as base table
- WCU: Write amplification (every write to base table writes to GSI)

**S3 Export Cost:**
- $0.10 per GB exported (one-time, no RCU consumption)

**Athena Cost:**
- $5 per TB scanned

**Quick Mental Math:**
```
For monthly queries on small % of table:
- <1M items queried → GSI ~$1-5/month
- 1-10M items queried → GSI ~$5-50/month
- >10M items queried → Consider S3 Export ~$20-100/month

For rare queries (quarterly/yearly):
- Just Scan it (cost amortizes over time)
```

### Recovery Drill Plan

**PRIORITY 1 (1 hour - TONIGHT):**
1. Memorize the formula cheat sheet above
2. Practice 10 cost calculations with calculator
3. Create rule of thumb: "Monthly + <10% = probably GSI"

**Mantra to repeat 25 times:**
> "Do the math BEFORE choosing. 2 minutes of arithmetic saves hours of wrong solutions."

**Target:** Calculate costs on every question before answering

---

**Last Updated:** December 13, 2025, 10:00 AM (Post Scan/GSI Quiz)
**Next Review:** December 13, 2025 PM (Cost calculation & Query limitation drills)

---

## 🎯 Tonight's EMERGENCY Recovery Focus (Dec 13)

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
