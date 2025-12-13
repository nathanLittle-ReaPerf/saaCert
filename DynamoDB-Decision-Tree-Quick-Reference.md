# DynamoDB Decision Tree - Quick Reference
**Created:** December 13, 2025
**Purpose:** Stop over-engineering, start calculating

---

## 🚨 STOP! Before Answering ANY DynamoDB Question:

### Step 1: Can I use Query?
```
Do I know the partition key value?
├─ YES → Query (if querying ONE partition)
└─ NO → Cannot use Query!
   ├─ Need items across ALL partitions?
   │  ├─ Infrequent (<weekly) → Scan
   │  ├─ Frequent (daily+) → GSI with different partition key
   │  └─ Analytics (large %) → S3 Export
   └─ Continue to Step 2
```

**CRITICAL RULE:**
- ✅ Query = partition key REQUIRED
- ❌ Cannot Query on just sort key
- ❌ Cannot Query across all partitions

---

## Step 2: Frequency + Selectivity Decision

```
┌─ What % of table do I need?
│
├─ <5% (Selective - small portion)
│  │
│  ├─ How often?
│  │  ├─ Multiple times per day → GSI ($2-10/month)
│  │  ├─ Daily → GSI ($2-10/month)
│  │  ├─ Weekly → GSI ($5-20/month)
│  │  ├─ Monthly → DO THE MATH!
│  │  │  ├─ <1M items → GSI (~$2-5/month)
│  │  │  └─ >1M items → Calculate GSI vs Export
│  │  ├─ Quarterly → Scan ($5-15/quarter)
│  │  ├─ Yearly → Scan ($5-15/year)
│  │  └─ One-time/Ad-hoc → Scan (simplest)
│
├─ 5-50% (Medium portion)
│  │
│  ├─ How often?
│  │  ├─ Daily+ → GSI or S3 Export (do math)
│  │  ├─ Weekly → GSI or S3 Export (do math)
│  │  ├─ Monthly → S3 Export ($20-60/month)
│  │  └─ Quarterly/Yearly → Scan (if <10B items)
│
└─ >50% (Most/All of table)
   │
   ├─ How often?
   │  ├─ Daily/Weekly → S3 Export + Glue/Athena
   │  ├─ Monthly → S3 Export + Athena
   │  ├─ Quarterly → S3 Export OR Scan (do math)
   │  └─ Yearly → Scan (if <10B items, cost amortizes)
   │
   └─ Table size consideration:
      ├─ <500 GB → Scan acceptable for rare queries
      └─ >500 GB → S3 Export for infrequent analytics
```

---

## Step 3: Special Patterns

### Leaderboard / Global Ranking
```
Need: Top N items across entire table

Solution: GSI with synthetic partition key
- PK = Fixed value (e.g., "GLOBAL" or "DAILY")
- SK = Ranking attribute (score, timestamp)
- Query: PK="GLOBAL", ScanIndexForward=false, Limit=N

❌ DON'T: Use score/timestamp as partition key (hot partition!)
✅ DO: Synthetic partition + score as sort key
```

### Many-to-Many Relationships
```
Example: Posts ↔ Hashtags, Users ↔ Groups

Solution: Denormalize (one item per relationship)
- Base table: Normal item with Set attribute
- GSI: One item per value in the set
  - PK = hashtag (String)
  - SK = timestamp (Number)

❌ DON'T: Use Sets as partition/sort keys (not allowed!)
✅ DO: Create multiple GSI items per base table item
```

### Range Queries (>, <, >=, <=, BETWEEN)
```
Need: Query numeric ranges (price > 100, age >= 21)

Solution: Numeric value as SORT KEY, NOT partition key
- PK = Static value or category (e.g., "HIGH_VALUE")
- SK = Numeric attribute (amount, price, age)
- Query: PK="HIGH_VALUE" AND SK > 100

❌ DON'T: Numeric/boolean as partition key for ranges
✅ DO: Static partition key + numeric sort key
```

### Production Isolation (No RCU Impact)
```
Need: Process large dataset without affecting prod traffic

Solution: S3 Export (does NOT consume RCUs)
- DynamoDB Export to S3
- Process with EMR, Glue, or Athena
- Zero impact on production table capacity

When to use:
✅ Quarterly/annual compliance audits
✅ Large table (5B+ records) analytics
✅ Provisioned capacity table (protect production)
❌ Don't use for frequent queries (monthly+)
```

---

## 💰 Cost Calculation Quick Reference

### DynamoDB Costs
```
Provisioned RCU: $0.00013/RCU-hour = ~$0.09 per million requests
On-Demand RCU:   $0.25 per million read request units
GSI Storage:     $0.25/GB-month
GSI Writes:      Write amplification (every base write → GSI write)
```

### S3 Export + Athena Costs
```
S3 Export:  $0.10/GB exported (one-time, no RCU consumption)
Athena:     $5/TB scanned
```

### Quick Mental Math
```
Monthly queries on small % of table:
- <1M items   → GSI ~$1-5/month
- 1-10M items → GSI ~$5-50/month
- >10M items  → S3 Export ~$20-100/month

Rare queries (quarterly/yearly):
- Just Scan it (cost amortizes)
- Scan 1B items 4x/year ≈ $10-20/year
- GSI for same = $50-100/year (wasteful!)
```

---

## 🎯 Common Mistakes to Avoid

### ❌ Mistake 1: "Monthly = S3 Export"
**Wrong thinking:** "I see 'monthly' → must use S3 Export"

**Right thinking:** "Monthly + what % of table?"
- Monthly + 2% → GSI ($4/mo)
- Monthly + 50% → S3 Export ($60/mo)
- Monthly + 100% → S3 Export ($100/mo)

**Do the math!**

---

### ❌ Mistake 2: "Twice per year = Need infrastructure"
**Wrong thinking:** "This is important → must build S3 Export pipeline"

**Right thinking:** "Twice per year → cost amortizes"
- Scan cost: $10/operation × 2/year = $20/year
- GSI cost: $50-100/year for infrastructure used 2 days
- S3 Export: $20-30/year + operational overhead

**For rare operations: Keep it simple → Scan**

---

### ❌ Mistake 3: "Numeric values as partition keys"
**Wrong thinking:** "I need to query amount > 1000 → amount as partition key"

**Right thinking:** "Range queries need SORT KEY"
- ❌ PK=amount → Can't query > or <, need exact match
- ✅ PK="HIGH_VALUE", SK=amount → Can query amount > 1000

**Numeric ranges = SORT KEY, never partition key**

---

### ❌ Mistake 4: "Query without partition key"
**Wrong thinking:** "I'll Query for all items where date > X"

**Right thinking:** "Query REQUIRES partition key"
- ❌ Cannot Query on just sort key
- ❌ Cannot Query across all partitions
- ✅ Need partition key + optional sort key filter

**No partition key? Use Scan, GSI, or Export**

---

### ❌ Mistake 5: "Sets can be keys"
**Wrong thinking:** "I'll use the hashtags Set as partition key"

**Right thinking:** "Sets CAN'T be keys, only attributes"
- ❌ Partition/Sort keys must be scalar (String, Number, Binary)
- ❌ Cannot use Sets, Lists, or Maps as keys
- ✅ Denormalize: One item per value in the set

**Many-to-many = denormalize, not Set keys**

---

## 🔥 Decision Mantras (Repeat Until Muscle Memory)

```
1. "Query = partition key REQUIRED. No partition key? No Query."

2. "Monthly ≠ Export. Do the math: Frequency × Selectivity = Solution"

3. "Twice per year? Just Scan it. Don't overthink."

4. "Numeric ranges = SORT KEY, never partition key"

5. "Many-to-many = denormalize. One item per relationship."

6. "Sets are attributes, not keys."

7. "Before choosing S3 Export, calculate GSI cost. 2 minutes of math saves hours."

8. "Rare operations = simple solutions. Don't build infrastructure for 2 queries/year."

9. "Production isolation? S3 Export (no RCUs). Otherwise? Probably GSI or Scan."

10. "When in doubt, calculate. Arithmetic beats intuition."
```

---

## 📋 Pre-Answer Checklist

Before answering ANY DynamoDB question:

- [ ] **1. Do I know the partition key value?**
  - YES → Can use Query
  - NO → Cannot use Query (Scan/GSI/Export)

- [ ] **2. What % of the table do I need?**
  - <5% → Selective
  - 5-50% → Medium
  - >50% → Most/All

- [ ] **3. How often will this run?**
  - Daily+ → Consider GSI or Export
  - Weekly/Monthly → Do the math!
  - Quarterly/Yearly → Probably Scan

- [ ] **4. Is this a range query? (>, <, >=, <=)**
  - YES → Numeric must be SORT KEY, not partition key
  - NO → Continue

- [ ] **5. Is this many-to-many? (Tags, groups, categories)**
  - YES → Denormalize (one item per relationship)
  - NO → Continue

- [ ] **6. Production isolation required?**
  - YES + Large table → S3 Export
  - NO → GSI or Scan fine

- [ ] **7. Did I calculate the cost?**
  - YES → Choose cheapest solution
  - NO → **GO BACK AND CALCULATE!**

---

## 🎓 Summary Decision Flow

```
START
  ↓
Can I Query? (Know partition key?)
  ├─ YES → Use Query
  └─ NO → Continue ↓
       ↓
What % of table + How often?
  ├─ Small % + Daily/Weekly → GSI
  ├─ Small % + Monthly → DO THE MATH! (probably GSI)
  ├─ Small % + Quarterly/Yearly → Scan
  ├─ Large % + Daily/Weekly → S3 Export
  ├─ Large % + Monthly → S3 Export
  └─ Large % + Quarterly/Yearly → Scan OR Export (calculate)
       ↓
Special pattern? (Leaderboard/Many-to-many/Range)
  ├─ Leaderboard → Synthetic partition key + score sort key
  ├─ Many-to-many → Denormalize
  └─ Range query → Numeric as SORT KEY
       ↓
Calculate costs before finalizing answer!
  ↓
DONE
```

---

**Remember:** The exam wants you to choose the MOST cost-effective solution with the LEAST operational overhead. That often means:
- **Simple > Complex** (Scan > S3 Export for rare queries)
- **Calculate > Assume** (2 minutes of math > wrong answer)
- **Frequency matters** (Yearly ≠ Daily, Monthly ≠ Weekly)

**Stop over-engineering. Start calculating. Pass the exam.**

---

**Created:** December 13, 2025
**Last Updated:** December 13, 2025
**Next Review:** Before every DynamoDB question
