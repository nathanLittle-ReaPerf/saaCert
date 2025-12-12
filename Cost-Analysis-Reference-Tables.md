# AWS SAA-C03 Cost Analysis Reference Tables

**Last Updated:** December 12, 2025
**Purpose:** Quick reference for cost-based decision making on exam questions
**Exam Date:** January 5, 2026 (24 days remaining)

---

## 📋 Table of Contents

1. [DynamoDB: Query vs Scan vs GSI vs S3 Export](#dynamodb-query-vs-scan-vs-gsi-vs-s3-export)
2. [Future Topics](#future-topics)

---

## DynamoDB: Query vs Scan vs GSI vs S3 Export

### Table 1: Annual Costs by Table Size & Query Frequency

| Table Size | Queries/Year | **Scan Cost** | **GSI Storage Cost** | **S3 Export + Athena** | **Winner** |
|------------|--------------|---------------|---------------------|------------------------|------------|
| **10 GB** | 4 (quarterly) | $10 | $30 | $50 | **Scan** ✅ |
| **10 GB** | 12 (monthly) | $30 | $30 | $60 | **Tie** (Scan or GSI) |
| **10 GB** | 52 (weekly) | $130 | $30 | $250 | **GSI** ✅ |
| **10 GB** | 365 (daily) | $912 | $30 | $1,850 | **GSI** ✅ |
| | | | | | |
| **100 GB** | 4 (quarterly) | $100 | $300 | $500 | **Scan** ✅ |
| **100 GB** | 12 (monthly) | $300 | $300 | $600 | **Tie** (Scan or GSI) |
| **100 GB** | 52 (weekly) | $1,300 | $300 | $2,500 | **GSI** ✅ |
| **100 GB** | 365 (daily) | $9,125 | $300 | $18,500 | **GSI** ✅ |
| | | | | | |
| **500 GB** | 4 (quarterly) | $500 | $1,500 | $2,500 | **Scan** ✅ |
| **500 GB** | 12 (monthly) | $1,500 | $1,500 | $3,000 | **Tie** (Scan or GSI) |
| **500 GB** | 52 (weekly) | $6,500 | $1,500 | $12,500 | **GSI** ✅ |
| **500 GB** | 365 (daily) | $45,625 | $1,500 | $91,250 | **GSI** ✅ |
| | | | | | |
| **1 TB (1,000 GB)** | 4 (quarterly) | $1,000 | $3,000 | $5,000 | **Scan** ✅ |
| **1 TB** | 12 (monthly) | $3,000 | $3,000 | $6,000 | **Tie** (Scan or GSI) |
| **1 TB** | 52 (weekly) | $13,000 | $3,000 | $25,000 | **GSI** ✅ |
| **1 TB** | 365 (daily) | $91,250 | $3,000 | $182,500 | **GSI** ✅ |
| | | | | | |
| **2 TB (2,000 GB)** | 4 (quarterly) | $2,000 | $6,000 | **$3,200** | **S3 Export** ✅ |
| **2 TB** | 12 (monthly) | $6,000 | $6,000 | **$3,600** | **S3 Export** ✅ |
| **2 TB** | 52 (weekly) | $26,000 | $6,000 | **$10,000** | **GSI** ✅ |
| **2 TB** | 365 (daily) | $182,500 | $6,000 | $73,000 | **GSI** ✅ |
| | | | | | |
| **10 TB (10,000 GB)** | 4 (quarterly) | $10,000 | $30,000 | **$16,000** | **S3 Export** ✅ |
| **10 TB** | 12 (monthly) | $30,000 | $30,000 | **$18,000** | **S3 Export** ✅ |
| **10 TB** | 52 (weekly) | $130,000 | $30,000 | **$50,000** | **GSI** ✅ |
| **10 TB** | 365 (daily) | $912,500 | $30,000 | $365,000 | **GSI** ✅ |

**Cost Formulas:**
- **Scan:** `Table_Size_GB × $0.25 × Queries_Per_Year`
- **GSI Storage:** `Table_Size_GB × $0.25/month × 12 months` (provisioned capacity additional)
- **S3 Export:** `(Table_Size_GB × $0.10 × Exports/Year) + (Table_Size_GB × $0.023 × 12) + (Queries × $0.005)`

---

### Table 2: Breakeven Points (When GSI Becomes Cheaper Than Scan)

| Table Size | Breakeven Queries/Year | Breakeven Frequency | Example |
|------------|------------------------|---------------------|---------|
| **10 GB** | **120** | **2-3 times/week** | Marketing reports 3×/week |
| **50 GB** | **24** | **Twice monthly** | Mid-month + end-month reports |
| **100 GB** | **12** | **Monthly** | Monthly compliance audit |
| **200 GB** | **6** | **Every 2 months** | Bi-monthly analytics |
| **500 GB** | **2-3** | **Quarterly-ish** | Quarterly + mid-quarter checks |
| **1 TB** | **1-2** | **Annual-ish** | Annual + mid-year review |
| **2 TB+** | **N/A** | **Consider S3 Export first** | Large data analytics |

**Pattern:** As table size increases, breakeven point decreases (GSI justifies sooner)

---

### Table 3: Decision Matrix by Table Size & Frequency

| | **Annual (1-4×)** | **Quarterly (4-12×)** | **Monthly (12-52×)** | **Weekly (52-200×)** | **Daily (365+×)** |
|---|---|---|---|---|---|
| **< 50 GB** | Scan | Scan | Scan/GSI | GSI | GSI |
| **50-100 GB** | Scan | Scan | GSI | GSI | GSI |
| **100-500 GB** | Scan | Scan | GSI | GSI | GSI |
| **500 GB - 1 TB** | Scan | GSI | GSI | GSI | GSI |
| **1-2 TB** | S3 Export | S3 Export | GSI | GSI | GSI |
| **2-5 TB** | S3 Export | S3 Export | S3 Export | GSI | GSI |
| **5+ TB** | S3 Export | S3 Export | S3 Export | GSI | GSI |

**Key Insight:** For multi-TB tables, S3 Export dominates for infrequent queries!

---

### Table 4: Sparse GSI Economics (When You Only Need X% of Data)

**Scenario:** You only need 5% of the data in the GSI

| Full Table Size | Sparse GSI Size (5%) | Full GSI Annual Cost | Sparse GSI Annual Cost | Savings |
|-----------------|----------------------|---------------------|------------------------|---------|
| 10 GB | 0.5 GB | $30 | **$2** | 94% |
| 100 GB | 5 GB | $300 | **$15** | 95% |
| 500 GB | 25 GB | $1,500 | **$75** | 95% |
| 1 TB | 50 GB | $3,000 | **$150** | 95% |
| 2 TB | 100 GB | $6,000 | **$300** | 95% |
| 8 TB | 800 MB (0.01%) | $24,000 | **$2.40** | 99.99% |

**Real Example (Q16 from Dec 12 drill):**
- 8 TB table, need 0.01% of data (large trades only)
- Sparse GSI: **$2.40/year**
- Quarterly Scan alternative: **$8,000/year**
- **Savings: $7,997.60/year (99.97%!)**

**Rule:** Sparse GSI can justify even infrequent queries if it saves 95%+ storage costs

---

### Table 5: Frequency Thresholds (When to Switch Strategies)

| Table Size | Switch from Scan to GSI | Switch from GSI to S3 Export |
|------------|-------------------------|------------------------------|
| **10 GB** | > 120 queries/year (weekly+) | Never (GSI always cheaper) |
| **100 GB** | > 12 queries/year (monthly+) | Never (GSI always cheaper) |
| **500 GB** | > 6 queries/year (bi-monthly+) | < 4 queries/year (quarterly-) |
| **1 TB** | > 3 queries/year (quarterly+) | < 12 queries/year (monthly-) |
| **2 TB** | > 2 queries/year (semi-annual+) | **< 52 queries/year (weekly-)** |
| **5 TB** | Always use GSI or Export | < 100 queries/year |
| **10 TB** | Always use GSI or Export | < 200 queries/year |

**Critical Rule:** **2 TB+ with < weekly queries = S3 Export wins!**

---

### Table 6: Cost Per Query Comparison

| Table Size | Queries/Year | Scan $/Query | GSI $/Query | S3 Export $/Query | Winner |
|------------|--------------|--------------|-------------|-------------------|--------|
| **100 GB** | 4 | $25 | $75 | $125 | Scan |
| **100 GB** | 365 | $25 | **$0.82** ✅ | $51 | **GSI** |
| **1 TB** | 4 | $250 | $750 | $1,250 | Scan |
| **1 TB** | 365 | $250 | **$8.22** ✅ | $500 | **GSI** |
| **2 TB** | 4 | $500 | $1,500 | **$800** ✅ | **S3 Export** |
| **2 TB** | 12 | $500 | $500 | **$300** ✅ | **S3 Export** |
| **2 TB** | 365 | $500 | **$16.44** ✅ | $200 | **GSI** |
| **10 TB** | 4 | $2,500 | $7,500 | **$4,000** ✅ | **S3 Export** |
| **10 TB** | 12 | $2,500 | $2,500 | **$1,500** ✅ | **S3 Export** |
| **10 TB** | 52 | $2,500 | **$577** ✅ | $962 | **GSI** |

**Pattern Recognition:**
- Cost per query DECREASES with GSI as frequency increases
- S3 Export has relatively flat cost per query
- Scan has constant high cost per query

---

### Table 7: The "10× Rule" Quick Reference

**If Scan is 10× more expensive than GSI, always use GSI (unless latency doesn't matter)**

| Table Size | Daily Queries Cost | Weekly Queries Cost | When 10× Rule Triggers |
|------------|-------------------|---------------------|------------------------|
| **10 GB** | Scan: $912, GSI: $30 | Scan: $130, GSI: $30 | **Weekly+** (30× cheaper) |
| **100 GB** | Scan: $9,125, GSI: $300 | Scan: $1,300, GSI: $300 | **Weekly+** (30× cheaper) |
| **500 GB** | Scan: $45,625, GSI: $1,500 | Scan: $6,500, GSI: $1,500 | **Weekly+** (30× cheaper) |
| **1 TB** | Scan: $91,250, GSI: $3,000 | Scan: $13,000, GSI: $3,000 | **Weekly+** (30× cheaper) |
| **2 TB** | Scan: $182,500, GSI: $6,000 | Scan: $26,000, GSI: $6,000 | **Weekly+** (30× cheaper) |

**Conclusion:** For weekly+ queries on ANY table size, GSI is 10-30× cheaper than Scan!

---

### Table 8: Real-World Decision Examples

| Scenario | Size | Frequency | First Thought | Correct Answer | Annual Cost | Why |
|----------|------|-----------|---------------|----------------|-------------|-----|
| Compliance audit | 200 GB | Quarterly (4×) | Scan | **Scan** ✅ | $200 | $200 vs $600 GSI |
| Marketing reports | 8 GB | 3×/week (150×) | Scan | **GSI** ✅ | $24 | $300 Scan vs $24 GSI |
| Fraud detection | 2 TB | 3×/day (1,200×) | GSI | **GSI** ✅ | $6K | $600K Scan vs $6K GSI |
| Annual review | 2 TB | Annual (1×) | Scan | **S3 Export** ✅ | $800 | $2K Scan vs $800 Export |
| Ad-hoc analytics | 2 TB | 2-3×/week (varies) | GSI | **S3 + Athena** ✅ | $11K | Flexibility > fixed GSI |
| Daily digest | 20 GB | Daily (365×) | Scan | **GSI** ✅ | $60 | $1,825 Scan vs $60 GSI |
| Stock trades audit | 8 TB | Quarterly (4×) | Scan | **Sparse GSI** ✅ | $2.40 | $8K Scan vs $2.40 sparse |

---

### 🎯 DynamoDB Cost Decision Rules (Memorize These)

1. **Small tables (< 100 GB):** Scan is cheap, GSI only for weekly+
2. **Medium tables (100 GB - 1 TB):** GSI breakeven at monthly+
3. **Large tables (1-2 TB):** S3 Export wins for < monthly, GSI for weekly+
4. **Huge tables (2+ TB):** S3 Export for < weekly, GSI for weekly+
5. **Sparse GSI:** Can justify GSI even for infrequent if saves 95%+ storage
6. **The 10× Rule:** If GSI is 10× cheaper, use it (unless latency doesn't matter)
7. **Multi-TB + Infrequent:** Always check S3 Export costs first!

---

## Future Topics

### S3 Storage Classes (Coming Soon)
- Cost comparison: Standard vs Intelligent-Tiering vs Glacier
- Retrieval time vs cost tradeoffs
- Lifecycle policy cost analysis

### RDS vs Aurora (Coming Soon)
- Cost breakeven analysis by instance size
- Multi-AZ vs Read Replica cost comparison
- Aurora Serverless v2 vs provisioned RDS

### Data Transfer Costs (Coming Soon)
- Inter-AZ transfer costs
- Internet egress pricing
- VPC endpoints savings analysis

### Lambda vs ECS/Fargate (Coming Soon)
- Request-based cost comparison
- Long-running workload breakeven points
- Cold start cost impact

### EBS Volume Types (Coming Soon)
- IOPS cost comparison (gp3 vs io2)
- Throughput pricing analysis
- Right-sizing recommendations

---

## 📝 How to Use This Document

1. **During quiz questions:** Quick lookup for cost-based decisions
2. **Before answering:** Check table size + frequency → find winner
3. **Pattern recognition:** Memorize the threshold rules
4. **After mistakes:** Update with new patterns learned

**Keep this file open during practice quizzes!**

---

**Last Updated:** December 12, 2025 - Added comprehensive DynamoDB cost tables after Query vs Scan drill failure
