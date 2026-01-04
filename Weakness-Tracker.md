# AWS SAA-C03 Weakness Tracker - Living Document

**Last Updated:** January 4, 2026 (Fresh start after holiday break)
**Exam Date:** February 11, 2026 at 5:15 PM EST
**Study Period:** January 5 - February 10, 2026 (37 days)
**Purpose:** Track weaknesses, monitor improvement, ensure no weak spots remain for exam

---

## 🎯 Current Active Weaknesses (Need Attention)

### 🔴 CRITICAL Priority (0-50% accuracy - Never or rarely correct)

_No critical weaknesses identified yet - starting fresh!_

### 🟠 HIGH Priority (51-75% accuracy - Inconsistent, need drilling)

_Will be populated as you take quizzes during the study period._

### 🟡 MEDIUM Priority (76-89% accuracy - Mostly correct, polish needed)

_Will be populated as you take quizzes during the study period._

---

## ✅ Previously Resolved Weaknesses (From Dec 2025 Study Period)

**These topics were mastered during your previous study period. Keep them sharp!**

| Topic | Resolution Date | Final Score | Verification |
|-------|----------------|-------------|--------------|
| **DynamoDB Query vs Scan (Frequency)** | Dec 18, 2025 | 100% | ✅ 10/10 on Retry #2 Final Drill - PERFECT SCORE! |
| **Athena vs Redshift (Query Frequency)** | Dec 11, 2025 | 100% | ✅ 5/5 correct (Final Boss Q1-5) |
| **DynamoDB Partition Key Design** | Dec 10, 2025 | 100% | ✅ 2/2 correct (Comprehensive quiz Q3-4) |
| **S3 Storage Classes ("very rarely" = Glacier)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **Aurora Multi-Master (RTO <30 sec)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **DynamoDB Consistency (Eventually = 50%)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **DynamoDB Capacity Modes (Known vs Unknown)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **DynamoDB Extreme Write Throughput (100K+/sec)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **Redshift for Frequent Analytics** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **RDS Proxy (Lambda + RDS)** | Dec 9, 2025 | 100% | ✅ 3/3 correct |
| **QLDB (Immutable Ledger)** | Dec 9, 2025 | 100% | ✅ 2/2 correct |
| **VPC NACLs (Stateless)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **Auto Scaling Policy Combinations** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **EC2 Placement Groups** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **VPC Endpoints (Gateway vs Interface)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **RDS Multi-AZ vs Read Replicas** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **Aurora Backtrack (MySQL-only)** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |
| **DynamoDB Streams** | Dec 8, 2025 | 100% | ✅ Multiple quizzes |

---

## 📊 Key Patterns & Decision Trees (Reference)

### DynamoDB: Numeric Partition Key Anti-Pattern

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

### DynamoDB: Query vs Scan Decision Tree

```
Need to query by different attribute than partition key?
│
├─ YES (e.g., query by "category" when partition key is "product_id")
│  └─ Use GSI (Global Secondary Index)
│     - GSI can have DIFFERENT partition key
│     - Enables cross-partition queries
│
└─ NO (just need alternative sort key on SAME partition)
   └─ Use LSI (Local Secondary Index)
      - LSI shares SAME partition key as base table
      - Only changes sort key
```

### EC2 Cost Optimization

```
Is the workload PREDICTABLE?
│
├─ YES (e.g., 9-5 weekdays, always 50 instances)
│  └─ Reserved Instances (1-year or 3-year)
│     - Discount: 40% (1-year) or 60% (3-year)
│
└─ NO (unpredictable traffic)
   └─ Auto Scaling with:
      ├─ On-Demand (for flexibility)
      └─ Spot (for cost savings if interruptible)
```

### S3 Storage Classes Decision Tree

```
Access frequency?
│
├─ Frequently (daily/weekly) → S3 Standard
├─ Infrequently (monthly) → S3 Standard-IA
├─ Rarely (quarterly) → S3 One Zone-IA or Glacier Instant Retrieval
├─ Very rarely (yearly) + mins retrieval → Glacier Flexible Retrieval
└─ Archive (almost never) + hours retrieval → Glacier Deep Archive
```

---

## 📝 How to Use This Tracker

1. **After each quiz:** Update active weaknesses immediately
2. **Track patterns:** Not just topics, but WHY you got it wrong
3. **Move to resolved:** Only when 90%+ accuracy on 3+ questions
4. **Review weekly:** Check if weaknesses are improving or stuck
5. **Fresh start mindset:** Don't be discouraged by December's struggles - this is a new study period!

---

## 🚨 Warning Signs

**A weakness needs immediate attention when:**
- ⚠️ Scoring <70% on same topic across 2+ quizzes
- ⚠️ Making the SAME mistake on different quiz dates
- ⚠️ Defaulting to wrong pattern without thinking
- ⚠️ Uncertainty when answering (guessing, not confident)

**A weakness is resolving when:**
- ✅ Scoring 80%+ consistently
- ✅ Answering confidently without hesitation
- ✅ Able to explain WHY wrong answers are wrong
- ✅ No repeat mistakes on retakes

---

**Last Updated:** January 4, 2026
**Next Review:** After first quiz of new study period (Week 1)
