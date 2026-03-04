# Query vs Scan Deep Dive - Emergency Recovery
## The Pattern You Keep Missing

**Your fundamental problem:** You're swinging between two extremes:
1. **Overengineering:** Building Streams + Lambda for 2-3 queries/week
2. **Infrastructure Misuse:** Creating GSI that costs $500-2,000/year for 4 quarterly queries

**The middle ground you're ignoring:** Scan is sometimes the RIGHT answer, not just acceptable.

---

## The Core Decision Tree

```
START: Need to search DynamoDB data on non-key attributes
│
├─ Do I know the partition key?
│  ├─ YES → Use Query (most efficient)
│  └─ NO → Continue below
│
├─ How frequently will this operation run?
│  │
│  ├─ FREQUENT (daily, multiple times per day, continuous)
│  │  └─ Is the attribute predictable/stable?
│  │     ├─ YES → Build GSI
│  │     └─ NO → Consider aggregating to separate table
│  │
│  ├─ MODERATE (few times per week, weekly)
│  │  └─ Calculate costs:
│  │     - GSI: ~$25-100/month (always running)
│  │     - Scan: ~$0.50-5/month (only when running)
│  │     └─ Unless data is HUGE, Scan is likely cheaper
│  │
│  └─ INFREQUENT (monthly, quarterly, one-time, ad-hoc)
│     └─ USE SCAN - Don't even think about GSI
│        Why? You'd pay 24/7 for infrastructure used 4-12 times/year
│
└─ Is this a real-time/latency-sensitive operation?
   ├─ YES → GSI (Scan takes seconds/minutes on large tables)
   └─ NO → Scan is acceptable
```

---

## Cost Analysis - Why Scan Beats GSI for Infrequent Queries

### Scenario: Quarterly compliance audit (4 queries/year)

**Option A: Build GSI**
- **Monthly cost:** $50 (provisioned capacity) × 12 months = **$600/year**
- **Storage cost:** $25/month × 12 = **$300/year**
- **Total annual cost:** **$900/year** for 4 queries

**Option B: Use Scan**
- **Cost per scan:** $0.25/GB scanned
- **4 scans/year on 10 GB table:** 4 × 10 × $0.25 = **$10/year**
- **Total annual cost:** **$10/year**

**Savings: $890/year by using Scan**

### Frequency Breakeven Analysis

| Frequency | Queries/Year | GSI Cost | Scan Cost (10GB) | Winner |
|-----------|-------------|----------|------------------|--------|
| Quarterly | 4 | $900 | $10 | **Scan** (90× cheaper) |
| Monthly | 12 | $900 | $30 | **Scan** (30× cheaper) |
| Weekly | 52 | $900 | $130 | **Scan** (7× cheaper) |
| 2-3×/week | 120 | $900 | $300 | **Scan** (3× cheaper) |
| Daily | 365 | $900 | $912 | **GSI** (breakeven) |
| Multiple/day | 1,000+ | $900 | $2,500+ | **GSI** (2-3× cheaper) |

**Key insight:** For anything less than DAILY queries, Scan is usually cheaper.

---

## When Scan Is The RIGHT Answer (Not Just Acceptable)

### ✅ Scan is OPTIMAL for:

1. **Infrequent operations:** Monthly, quarterly, yearly, one-time analyses
2. **Ad-hoc queries:** Business intelligence, auditing, compliance reports
3. **Low query frequency:** Less than once per day on average
4. **Numeric ranges on partition key:** Can't build GSI on partition key
5. **Frequently changing search attributes:** Would need multiple GSIs
6. **Small to medium tables:** < 50 GB where scan completes in reasonable time
7. **Non-latency-sensitive operations:** Background jobs, batch processing

### ❌ Scan is WRONG for:

1. **High frequency:** Multiple times per day, continuous queries
2. **Real-time/user-facing:** Need sub-second response times
3. **Huge tables + frequent use:** > 100 GB scanned multiple times daily
4. **Production bottlenecks:** Where scan duration impacts user experience

---

## Your Specific Mistakes - Let's Fix Them

### Mistake 1: Building Streams + Lambda for 2-3 queries/week

**What you did (Q7):**
```
Requirement: Search userData by department, 2-3 times/week
Your answer: DynamoDB Streams → Lambda → Maintain aggregate table
```

**Why this is insane:**
- **Operational overhead:** Maintaining Lambda function, monitoring streams, debugging failures
- **Cost:** Lambda invocations on EVERY write (even if department doesn't change)
- **Complexity:** 100× more complex than needed
- **Annual cost:** ~$500-1,000 (Lambda + additional table)

**Correct answer:** Scan
- **Cost:** 120 scans/year on 5 GB = 120 × 5 × $0.25 = **$150/year**
- **Operational overhead:** ZERO - just run the scan when needed
- **Complexity:** Single API call

**Pattern to remember:** If frequency is measured in "times per week," Scan is probably fine.

---

### Mistake 2: Building GSI for quarterly queries

**What you did (Q8):**
```
Requirement: Quarterly compliance audit (4 times/year)
Your answer: Create GSI on complianceStatus
```

**Why this fails the smell test:**
- You're paying 24/7 for infrastructure used **4 times per year**
- **Math:** 8,760 hours/year paying for capacity ÷ 4 queries = $219/hour per query
- That's like buying a Ferrari to drive to the grocery store once per quarter

**Correct answer:** Scan
- **Cost:** 4 × $2.50 = **$10/year** vs GSI's **$900/year**
- **Savings:** You could run this scan 360 times and still be cheaper than GSI

**Pattern to remember:** If frequency is "quarterly" or "annually," always Scan.

---

### Mistake 3: Not recognizing when GSI IS appropriate

**What you did (Q10):**
```
Requirement: Gaming leaderboard, display top 100 scores
Your answer: Streams + Lambda updating aggregate table
```

**Why this is overengineering:**
- Leaderboard = **frequent reads** (every user visiting leaderboard page)
- You need a GSI on score to efficiently query top 100
- Streams+Lambda adds unnecessary complexity

**Correct answer:** GSI on score (descending)
- **Why:** Frequent queries (100s-1000s per day) justify infrastructure cost
- **Pattern:** `Query` with `ScanIndexForward=false` and `Limit=100`
- **Simple:** Single API call, no Lambda maintenance

**Pattern to remember:** Frequent + predictable = GSI is worth it.

---

## Decision Framework - Use This Every Time

### Step 1: Identify Query Frequency
- **Ask yourself:** "How many times will this run per day/week/month/year?"
- **Count it:** 2-3 times/week = 120/year, quarterly = 4/year, etc.

### Step 2: Calculate GSI Annual Cost
- **Provisioned capacity:** ~$25-100/month = $300-1,200/year
- **Storage:** ~$25/month = $300/year
- **Total:** ~$600-1,500/year for a basic GSI

### Step 3: Calculate Scan Annual Cost
- **Formula:** Queries/year × Table size (GB) × $0.25/GB
- **Example:** 120 queries × 5 GB × $0.25 = $150/year

### Step 4: Apply The 10× Rule
- **If Scan is 10× cheaper, use Scan** (unless latency matters)
- **If GSI is 2-3× cheaper, use GSI** (frequent queries justify it)

### Step 5: Check Latency Requirements
- **User-facing/real-time?** Probably need GSI regardless of cost
- **Background/batch/reports?** Scan is fine even if it takes minutes

---

## Exam Keywords That Scream "SCAN"

When you see these phrases, think Scan first:

| Keyword | Why It Means Scan |
|---------|-------------------|
| "quarterly audit" | 4×/year - don't build infrastructure |
| "monthly compliance report" | 12×/year - Scan is 30× cheaper |
| "one-time migration" | Literally once - GSI would be insane |
| "ad-hoc analysis" | Unpredictable - can't optimize for it |
| "2-3 times per week" | 120×/year - Scan costs $150, GSI costs $900 |
| "annual review" | 1×/year - $2.50 scan vs $900 GSI |
| "occasionally need to find" | Not frequent = Scan |

---

## Exam Keywords That Scream "GSI"

When you see these phrases, think GSI:

| Keyword | Why It Means GSI |
|---------|------------------|
| "real-time leaderboard" | Frequent + latency-sensitive |
| "display top 100" | Frequent reads, needs sorting |
| "users frequently search by" | High frequency justifies infrastructure |
| "application requires fast lookups" | Latency requirement |
| "hundreds of queries per day" | 365+ queries justifies GSI cost |
| "primary use case" | Core functionality = frequent |

---

## The Trap You Keep Falling Into

**Scan Sounds Dirty:** You think "Scan = bad practice" because you've heard it's inefficient.

**Reality Check:**
- **Scan is inefficient** for high-frequency operations
- **Scan is OPTIMAL** for low-frequency operations
- **The exam tests whether you know the difference**

**Analogies to help you remember:**

1. **GSI = Maintaining a highway:** Costs money 24/7, great for frequent traffic
2. **Scan = Taking the side roads:** Free to maintain, fine for occasional trips
3. **Don't build a highway for a quarterly trip to IKEA**

---

## Practice Scenarios - Apply The Pattern

### Scenario 1
**Requirement:** Search 50,000 orders by orderStatus, 5-6 times per week for operations team.

**Your thought process:**
- Frequency: 5-6×/week = ~280 queries/year
- GSI cost: ~$900/year
- Scan cost: 280 × 2 GB × $0.25 = $140/year
- Ratio: GSI is 6× more expensive
- Latency: Operations team (not user-facing) = not critical
- **Answer: Scan**

---

### Scenario 2
**Requirement:** Gaming app needs to display leaderboard ranking updated every 30 seconds for 10,000 concurrent users.

**Your thought process:**
- Frequency: 2 queries/minute × 1,440 minutes/day = 2,880 queries/day
- Annual: 2,880 × 365 = 1,051,200 queries/year
- Scan cost: 1M × 5 GB × $0.25 = **$1,250,000/year** (insane)
- GSI cost: ~$900/year
- Latency: Real-time user experience = critical
- **Answer: GSI (with score as sort key)**

---

### Scenario 3
**Requirement:** Quarterly compliance audit needs to identify all users with incomplete KYC verification.

**Your thought process:**
- Frequency: Quarterly = 4 queries/year
- GSI cost: ~$900/year for 4 queries = $225/query
- Scan cost: 4 × 10 GB × $0.25 = $10/year = $2.50/query
- Ratio: GSI is 90× more expensive
- Latency: Compliance report (not user-facing) = not critical
- **Answer: Scan**

---

## Your Action Items

1. **Memorize the breakeven point:** GSI becomes cost-effective at ~daily frequency
2. **Stop thinking Scan = always bad:** It's optimal for infrequent operations
3. **Read the frequency keywords:** "quarterly" → 4×/year → Scan
4. **Calculate costs mentally:** 120 queries × 5 GB × $0.25 = $150 (cheaper than $900 GSI)
5. **Stop overengineering:** Streams+Lambda is almost never the answer for simple queries

---

## The Bottom Line

**You need to internalize this:**

- **Infrequent queries (< daily) = Scan is OPTIMAL, not just acceptable**
- **Frequent queries (multiple/day) = GSI justifies the cost**
- **Real-time/latency-sensitive = GSI regardless of frequency**
- **Stop building infrastructure for operations that run 4-120 times/year**

**Next step:** 20-question drill to prove you've learned this. You need 18/20 (90%) to proceed.

Ready to get drilled?
