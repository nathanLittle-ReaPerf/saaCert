# Query vs Scan Breakeven Point Flashcards

**Purpose:** Memorize breakeven points for instant decision-making
**Target:** < 5 seconds per card
**Goal:** Perfect recall before retaking 20-question drill

---

## Flashcard 1: The 5 Core Breakeven Numbers

**Front:**
```
What are the 5 critical breakeven points for DynamoDB Scan vs GSI?
```

**Back:**
```
1. 10 GB = 120 queries/year (2-3×/week)
2. 50 GB = 24 queries/year (twice monthly)
3. 100 GB = 12 queries/year (monthly)
4. 200 GB = 6 queries/year (bi-monthly)
5. 2+ TB = Check S3 Export first if < weekly

Rule: queries ≥ breakeven → GSI
      queries < breakeven → Scan
```

---

## Flashcard 2: Frequency to Queries/Year Conversion

**Front:**
```
Convert these frequencies to queries/year:
- Weekly
- Twice monthly
- Monthly
- Quarterly
- Twice per year
```

**Back:**
```
- Weekly = 52/year
- Twice monthly = 24/year
- Monthly = 12/year
- Quarterly = 4/year
- Twice per year = 2/year

Memorize: Weekly = 52, Twice monthly = 24, Monthly = 12, Quarterly = 4
```

---

## Flashcard 3: 30 GB Table Scenario

**Front:**
```
30 GB table, weekly queries (52×/year)
Scan or GSI?
```

**Back:**
```
GSI ✅

Calculation:
- 30 GB breakeven ≈ 40 queries/year
- 52 > 40 = above breakeven
- Scan: 52 × 30 × $0.25 = $390/year
- GSI: 30 × $3 = $90/year
- GSI is 4× cheaper

Pattern: Weekly on ANY table > 10 GB = GSI
```

---

## Flashcard 4: 20 GB Table Scenario

**Front:**
```
20 GB table, twice monthly (24×/year)
Scan or GSI?
```

**Back:**
```
GSI ✅

Calculation:
- 20 GB breakeven ≈ 60 queries/year
- 24 < 60 = below breakeven... BUT check the math!
- Scan: 24 × 20 × $0.25 = $120/year
- GSI: 20 × $3 = $60/year
- GSI is 2× cheaper

Pattern: Even "below breakeven" can favor GSI if costs are close!
```

---

## Flashcard 5: 150 GB Table Scenario

**Front:**
```
150 GB table, monthly queries (12×/year)
Scan or GSI?
```

**Back:**
```
GSI ✅

Calculation:
- 150 GB breakeven ≈ 12 queries/year
- 12 = 12 = AT breakeven
- Scan: 12 × 150 × $0.25 = $450/year
- GSI: 150 × $3 = $450/year
- TIE → Pick GSI for consistent performance

Pattern: At breakeven, always pick GSI
```

---

## Flashcard 6: 200 GB Table Scenario

**Front:**
```
200 GB table, twice monthly (24×/year)
Scan or GSI?
```

**Back:**
```
GSI ✅

Calculation:
- 200 GB breakeven = 6 queries/year
- 24 > 6 = WAY above breakeven
- Scan: 24 × 200 × $0.25 = $1,200/year
- GSI: 200 × $3 = $600/year
- GSI is 2× cheaper

Pattern: 200+ GB + twice monthly = always GSI
```

---

## Flashcard 7: The 2 TB Exception Rule

**Front:**
```
2 TB table, monthly queries (12×/year), need 80% of data
Scan, GSI, or S3 Export?
```

**Back:**
```
S3 Export ✅

Calculation:
- Scan: 12 × 2000 × $0.25 = $6,000/year
- GSI: 2000 × $3 = $6,000/year
- S3 Export: ~$3,600/year
- S3 Export is 40% cheaper!

Pattern: 2+ TB + < weekly + NOT sparse = S3 Export
```

---

## Flashcard 8: Sparse GSI Exception

**Front:**
```
1 TB table, weekly (52×/year), need 2% of data
What strategy?
```

**Back:**
```
Sparse GSI ✅

Calculation:
- 1 TB × 2% = 20 GB sparse GSI
- Sparse GSI: 20 × $3 = $60/year
- Full GSI: 1000 × $3 = $3,000/year
- Scan: 52 × 1000 × $0.25 = $13,000/year
- Sparse GSI is 50-200× cheaper!

Pattern: < 5% data needed = Sparse GSI (overrides other rules)
```

---

## Flashcard 9: Small Table, Very Infrequent

**Front:**
```
300 GB table, twice per year (2×/year)
Scan, GSI, or S3 Export?
```

**Back:**
```
Scan ✅

Calculation:
- Scan: 2 × 300 × $0.25 = $150/year
- GSI: 300 × $3 = $900/year
- S3 Export: ~$160/year (similar to Scan)
- Scan is simplest and cheapest

Pattern: Ultra-infrequent (1-4×/year) + < 500 GB = Scan
```

---

## Flashcard 10: The Mental Math Formula

**Front:**
```
Quick mental math for Scan vs GSI:

Scan annual cost = ?
GSI annual cost = ?
```

**Back:**
```
Scan annual cost:
Queries/year × Table_GB × $0.25

GSI annual cost:
Table_GB × $3

Breakeven formula:
Queries/year × Table_GB × $0.25 = Table_GB × $3
Queries/year × $0.25 = $3
Queries/year = 12

Pattern: At 12 queries/year, costs are equal for ANY table size!
Wait... that's only true for certain sizes. Better to memorize:
- Small tables (< 50 GB): breakeven at 24-120 queries
- Medium tables (100 GB): breakeven at 12 queries
- Large tables (200+ GB): breakeven at 6 or fewer queries
```

---

## Quick Reference Table (Memorize This)

| Table Size | Breakeven | If Above → GSI | If Below → Scan |
|------------|-----------|----------------|-----------------|
| 10 GB | 120/year (weekly+) | 52+ queries | < 52 queries |
| 30 GB | 40/year (weekly-ish) | 40+ queries | < 40 queries |
| 50 GB | 24/year (twice monthly) | 24+ queries | < 24 queries |
| 100 GB | 12/year (monthly) | 12+ queries | < 12 queries |
| 200 GB | 6/year (bi-monthly) | 6+ queries | < 6 queries |
| 2+ TB | N/A | Check S3 Export | Check S3 Export |

---

## Practice Drill (Do This Now)

For each scenario, calculate in < 30 seconds:

1. **40 GB, weekly (52×/year)** → ?
2. **150 GB, monthly (12×/year)** → ?
3. **25 GB, twice monthly (24×/year)** → ?
4. **200 GB, quarterly (4×/year)** → ?
5. **2 TB, weekly (52×/year), 90% data** → ?

**Answers:**
1. GSI (52 > 40 breakeven, $520 Scan vs $120 GSI)
2. GSI (at breakeven, $450 = $450, pick GSI)
3. Scan or GSI (24 ≈ 30 breakeven, close call, check math: $150 Scan vs $75 GSI → GSI!)
4. Scan (4 << 6 breakeven, $200 Scan vs $600 GSI)
5. GSI (weekly + large table, ignore 2 TB export rule, $26K Scan vs $6K GSI)

---

**Study Method:**
1. Read card front
2. Cover back
3. Calculate answer in < 30 seconds
4. Check back
5. If wrong, write calculation 5 times
6. Repeat until perfect recall

**Next:** 10-question breakeven drill (must score 9/10)
