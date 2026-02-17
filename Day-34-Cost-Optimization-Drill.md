# Day 34 - Cost Optimization Targeted Drill
## 10 Questions - Brutal Edition

**Date:** February 16, 2026 (14 days to exam)
**Focus:** Cost calculation accuracy after 53% quiz failure
**Target:** 9/10 (90%+) to prove worksheet mastery
**Time Limit:** 30 minutes (3 min per question max)

---

## INSTRUCTIONS

This drill targets your EXACT failures from today's quiz:
- RI utilization break-even calculations
- S3 storage + retrieval cost totals
- Savings Plan commitment sizing
- Commitment flexibility (regional restrictions)

**Rules:**
1. Show your work for all calculations
2. No calculator initially - mental math first
3. Then verify with calculator
4. If you get <9/10, you MUST redo the entire Cost Calculation Worksheet

---

## Question 1: RI Utilization Break-Even (Your Q11 Disaster)

A university runs research simulations:
- 80 c5.4xlarge instances per simulation
- Simulations run 4 times/month, each lasting 20 hours
- Instance: c5.4xlarge @ $0.68/hour On-Demand
- 1-year Standard RI (no upfront): 40% discount = $0.408/hour

**Calculate:**

A) Monthly instance-hours of usage:
```
Hours/month = __________
```

B) Monthly cost if you buy 80 Standard RIs:
```
RI cost = __________
```

C) Monthly cost with current On-Demand:
```
OD cost = __________
```

D) Utilization percentage for the RIs:
```
Utilization = __________%
Break-even at 40% discount = 60%
Conclusion: RIs would __________ (save/waste) money
```

E) What's the correct cost optimization strategy?
- **A)** Buy 80 Standard RIs (lock in discount)
- **B)** Buy Compute Savings Plan sized for baseline usage
- **C)** Use 100% Spot Instances (simulations can checkpoint)
- **D)** Use On-Demand (no commitments for low utilization)

**Your Answer:** _______

---

## Question 2: S3 Retrieval Cost Trap (Your Q12 Disaster)

A media company stores 300 TB of video content (archived shows, 1+ year old):
- Access pattern: Accessed rarely (3-5 times per month total, not per file)
- Average file size: 10 GB
- Must retrieve within 10 minutes for customer requests
- Currently in S3 Standard: 300 TB × $0.023/GB = $6,900/month

**Calculate total monthly cost for each option:**

**Option A: S3 Standard-IA**
```
Storage: 300,000 GB × $0.0125 = $__________
Retrievals: 4 requests/month × 10 GB × $0.01/GB = $__________
Total: $__________
```

**Option B: S3 Glacier Instant Retrieval**
```
Storage: 300,000 GB × $0.004 = $__________
Retrievals: 4 requests/month × 10 GB × $0.03/GB = $__________
Total: $__________
```

**Option C: S3 Intelligent-Tiering**
```
Storage (assume optimizes to Archive Instant): 300,000 GB × $0.004 = $__________
Monitoring (300 TB / 10 GB = 30,000 files): 30,000 / 1,000 × $0.0025 = $__________
Retrieval: $__________
Total: $__________
```

**Which option is MOST cost-effective while meeting 10-minute retrieval requirement?**
- **A)** Standard-IA
- **B)** Glacier Instant Retrieval
- **C)** Intelligent-Tiering
- **D)** Keep in S3 Standard

**Your Answer:** _______

---

## Question 3: Seasonal Workload Utilization (Your Q11 Again)

A retail company has a holiday shopping spike:
- Nov 15 - Jan 15 (60 days): Need 400 m5.xlarge instances 24/7
- Jan 16 - Nov 14 (305 days): Need 80 m5.xlarge instances 24/7
- Instance: m5.xlarge @ $0.192/hour On-Demand
- 1-year Standard RI: 40% discount = $0.115/hour

**Calculate annual cost for each option:**

**Option A: Buy 400 RIs year-round**
```
Annual cost: 400 × 8,760 hrs × $0.115 = $__________
Utilization:
  - Holiday: 400 instances × 1,440 hrs = __________ hrs
  - Off-season: 80 instances × 7,320 hrs = __________ hrs
  - Total usage: __________ hrs
  - Total RI capacity: 400 × 8,760 = __________ hrs
  - Utilization: __________%
Is this above 60% break-even? __________
```

**Option B: Buy 80 RIs baseline + 320 On-Demand during holidays**
```
80 RIs: 80 × 8,760 × $0.115 = $__________
320 OD (holidays): 320 × 1,440 × $0.192 = $__________
Total: $__________
```

**Current On-Demand annual cost:**
```
Holiday: 400 × 1,440 × $0.192 = $__________
Off-season: 80 × 7,320 × $0.192 = $__________
Total: $__________
```

**Which option provides MOST cost savings?**
- **A)** Option A (400 RIs year-round)
- **B)** Option B (80 RIs + 320 OD seasonal)
- **C)** Current On-Demand
- **D)** Options A and B cost the same

**Your Answer:** _______

---

## Question 4: Savings Plan vs RI Regional Flexibility (Your Q14 Disaster)

A SaaS company operates in 4 regions with identical architecture per region:
- us-east-1: 50 m5.2xlarge + 25 c5.4xlarge
- eu-west-1: 50 m5.2xlarge + 25 c5.4xlarge
- ap-southeast-1: 50 m5.2xlarge + 25 c5.4xlarge
- ap-northeast-1: 50 m5.2xlarge + 25 c5.4xlarge

Total: 200 m5.2xlarge + 100 c5.4xlarge across 4 regions

**Requirement:** Must be able to shift capacity between regions based on time-of-day traffic (scale up APAC during Asian hours, scale down US at night, etc.)

**Evaluate each option:**

**Option A: Purchase region-specific Convertible RIs (50 m5 + 25 c5 per region)**
```
Can Convertible RIs change instance family? __________ (Yes/No)
Can Convertible RIs change instance size? __________ (Yes/No)
Can Convertible RIs move between regions? __________ (Yes/No)
Conclusion: Convertible RIs __________ (do/don't) support cross-region traffic shifting
```

**Option B: Purchase Compute Savings Plan**
```
Does Compute SP apply across all regions? __________ (Yes/No)
Does Compute SP apply across instance families? __________ (Yes/No)
Can you freely shift capacity between regions? __________ (Yes/No)
Conclusion: Compute SP __________ (does/doesn't) support cross-region traffic shifting
```

**Option C: Purchase EC2 Instance Savings Plan (sized for m5 family)**
```
Does EC2 Instance SP apply across regions? __________ (Yes/No)
Does EC2 Instance SP apply to c5 instances? __________ (Yes/No)
Conclusion: EC2 Instance SP __________ (does/doesn't) meet all requirements
```

**Which option provides cross-region flexibility AND covers both instance families?**
- **A)** Convertible RIs
- **B)** Compute Savings Plan
- **C)** EC2 Instance Savings Plan
- **D)** Standard RIs

**Your Answer:** _______

---

## Question 5: Standard-IA Access Pattern Break-Even

You have 200 TB of data stored in S3 Standard-IA.

**Calculate:** At what access frequency does Standard-IA stop making sense compared to S3 Standard?

**Given:**
- S3 Standard storage: $0.023/GB/month
- S3 Standard-IA storage: $0.0125/GB/month
- S3 Standard-IA retrieval: $0.01/GB

**Break-even calculation:**
```
Storage savings per GB/month: $0.023 - $0.0125 = $__________/GB/month

Break-even retrieval frequency:
  Storage savings = Retrieval cost
  $__________ = $0.01 × (retrievals/month)
  Retrievals/month = __________

Conclusion: Standard-IA makes sense only when accessing <__________ times per month
```

**If your data is accessed 5 times per month, which is cheaper?**
```
Standard-IA: $0.0125 storage + (5 × $0.01) retrieval = $__________/GB/month
S3 Standard: $0.023/GB/month
Difference: Standard-IA is $__________ (cheaper/more expensive)
```

**Answer:** At 5 accesses/month, use __________ (Standard/Standard-IA)

---

## Question 6: Spot vs RI for Low-Utilization Workload

A biotech company runs protein folding simulations:
- 200 c5.18xlarge instances per simulation
- Simulations run unpredictably: 5-8 times per month
- Each simulation: 6-10 hours (average 8 hours)
- **Simulations checkpoint every 30 minutes** (can restart from checkpoint if interrupted)
- Instance: c5.18xlarge @ $3.06/hour On-Demand
- 1-year Standard RI: 40% discount = $1.836/hour

**Calculate:**

A) Monthly instance-hours and cost:
```
Simulations: 6.5 avg × 8 hrs = __________ hours average
Instance-hours: 200 × __________ = __________ instance-hours/month
On-Demand cost: __________ × $3.06 = $__________/month
```

B) Cost if you buy 200 Standard RIs:
```
RI monthly cost: 200 × 730 × $1.836 = $__________/month
Utilization: __________ hrs usage / 146,000 hrs RI = __________%
Break-even: 60%
Conclusion: RIs would __________ (save/waste) $__________/month
```

C) Cost with 100% Spot Instances (70% discount):
```
Spot cost: $__________ × 0.30 = $__________/month
Savings vs On-Demand: $__________/month
Savings vs RI option: $__________/month
```

**What's the BEST cost optimization strategy?**
- **A)** Buy 200 Standard RIs (maximum discount)
- **B)** Buy Compute Savings Plan (flexibility)
- **C)** Use 100% Spot Instances (fault-tolerant with checkpointing)
- **D)** Stay On-Demand (no commitments)

**Your Answer:** _______

**Why is this the best option?**
_________________________________________________________________

---

## Question 7: Commitment Flexibility Matrix

**Fill in the table:**

| Commitment Type | Cross-Region? | Cross-Instance Family? | Can Change Size? | Use Case |
|-----------------|---------------|------------------------|------------------|----------|
| Standard RI | __________ | __________ | __________ | _________________________ |
| Convertible RI | __________ | __________ | __________ | _________________________ |
| EC2 Instance SP | __________ | __________ | __________ | _________________________ |
| Compute SP | __________ | __________ | __________ | _________________________ |

**Ranking by flexibility (most to least):**
1. __________
2. __________
3. __________
4. __________

---

## Question 8: Multi-Tier Savings Plan Sizing

An application has these components:
- Tier 1 (24/7): 20 m5.xlarge @ $0.192/hr = $2,810/month
- Tier 2 (24/7): 10 r5.large @ $0.126/hr = $919/month
- Tier 3 (12 hrs/day): 30 c5.2xlarge @ $0.34/hr × 360 hrs = $3,672/month
- **Total On-Demand: $7,401/month**

Tier 3 is a batch processing system (runs 6 AM - 6 PM daily, fault-tolerant with checkpointing).

**Calculate optimal strategy:**

**Option A: Compute Savings Plan for Tiers 1+2, Spot for Tier 3**
```
24/7 baseline: $2,810 + $919 = $__________/month On-Demand
Savings Plan commitment (40% discount): $__________ / 0.60 = $__________ covers $__________ usage
SP cost: $__________
Tier 3 Spot (70% discount): $3,672 × 0.30 = $__________
Total: $__________ + $__________ = $__________/month
Savings: $__________/month (_________%)
```

**Option B: Savings Plan for 50% of total usage**
```
50% commitment: $7,401 × 0.50 = $__________/month
This covers $__________ / 0.60 = $__________ usage
Remaining OD: $7,401 - $__________ = $__________
Total: $__________ + $__________ = $__________/month
Savings: $__________/month (_________%)
```

**Which option saves more money?**
- **A)** Option A (SP for baseline + Spot for batch)
- **B)** Option B (50% SP commitment)
- **C)** Both save the same amount
- **D)** Neither saves 30%+

**Your Answer:** _______

---

## Question 9: Deep Archive vs Glacier Flexible Retrieval

You have 500 TB of compliance data that must be retained for 7 years:
- Access frequency: 2-3 times per year (regulatory audits)
- Retrieval requirement: Must be available within 8 hours
- Retention: Cannot delete for 7 years

**Calculate monthly cost:**

**Option A: Glacier Deep Archive (Standard retrieval = 12 hours)**
```
Storage: 500,000 GB × $0.00099 = $__________/month
Retrieval (2.5/year = 0.21/month): 500,000 × 0.21 × $0.02 = $__________/month
Total: $__________/month
Meets 8-hour requirement? __________ (Yes/No - Standard retrieval takes 12 hrs)
```

**Option B: Glacier Flexible Retrieval (Expedited = 1-5 min, Standard = 3-5 hrs)**
```
Storage: 500,000 GB × $0.0036 = $__________/month
Retrieval (Standard, 3-5 hrs): 500,000 × 0.21 × $0.03 = $__________/month
Total: $__________/month
Meets 8-hour requirement? __________ (Yes/No)
```

**Which option is MOST cost-effective while meeting the 8-hour retrieval requirement?**
- **A)** Deep Archive (Standard retrieval)
- **B)** Glacier Flexible (Expedited retrieval)
- **C)** Glacier Flexible (Standard retrieval)
- **D)** Glacier Instant Retrieval

**Your Answer:** _______

---

## Question 10: RI Utilization Tipping Point

You're evaluating whether to purchase RIs for a variable workload.

**Given:**
- Workload runs X hours per month (variable)
- On-Demand rate: $1.00/hour
- 1-year RI (40% discount): $0.60/hour
- Monthly RI cost if purchased: 730 hours × $0.60 = $438/month

**Calculate:** At what monthly usage hours do RIs become cheaper than On-Demand?

```
Break-even: RI cost = OD cost
$438 = X hours × $1.00
X = __________ hours/month

Utilization percentage: __________ / 730 = __________%

Conclusion: RIs save money when workload runs >__________ hours/month (_________% utilization)
```

**If your workload runs 500 hours/month:**
```
OD cost: 500 × $1.00 = $__________
RI cost: $438 (fixed)
Best choice: __________ (RI/On-Demand)
Savings with best choice: $__________/month
```

**If your workload runs 300 hours/month:**
```
OD cost: 300 × $1.00 = $__________
RI cost: $438 (fixed)
Best choice: __________ (RI/On-Demand)
Waste with wrong choice: $__________/month
```

---

## ANSWER KEY

### Question 1:
A) 80 × 4 × 20 = **6,400 hours/month**
B) 80 × 730 × $0.408 = **$23,827/month**
C) 6,400 × $0.68 = **$4,352/month**
D) 6,400 / 58,400 = **11% utilization** (way below 60% break-even)
E) **Answer: C** - Use 100% Spot (fault-tolerant + massive savings)

**If you chose A or B: You failed. RIs waste $19,475/month at 11% utilization.**

---

### Question 2:
**Option A:** $3,750 + $4 = **$3,754/month**
**Option B:** $1,200 + $1.20 = **$1,201/month** ✅ **BEST**
**Option C:** $1,200 + $75 + $0 = **$1,275/month**

**Answer: B** - Glacier Instant is cheapest and meets 10-min requirement (milliseconds)

---

### Question 3:
**Option A:**
- Annual: **$403,440**
- Holiday usage: 576,000 hrs
- Off-season usage: 585,600 hrs
- Total: 1,161,600 hrs / 3,504,000 hrs = **33% utilization**
- Below 60% break-even = **WASTE MONEY**

**Option B:**
- 80 RIs: $80,582
- 320 OD: $88,474
- Total: **$169,056/year** ✅

**Current OD:** $110,592 + $112,538 = **$223,130/year**

**Answer: B** - Option B saves $54,074/year (24%). Option A LOSES $180,310 vs current!

---

### Question 4:
**Option A:** Family: Yes, Size: Yes, **Region: NO** ❌
**Option B:** Cross-region: Yes, Cross-family: Yes, **Shift capacity: YES** ✅
**Option C:** Cross-region: Yes, **c5 covered: NO** ❌

**Answer: B** - Only Compute Savings Plan provides cross-region + cross-family flexibility

---

### Question 5:
- Storage savings: **$0.0105/GB/month**
- Break-even: **1.05 retrievals/month**
- At 5 accesses: Standard-IA = $0.0125 + $0.05 = **$0.0625/GB** vs Standard **$0.023/GB**
- **Standard-IA is $0.0395 MORE EXPENSIVE**

**Answer: Use S3 Standard** at 5 accesses/month

---

### Question 6:
A) 6.5 × 8 = 52 hrs, 200 × 52 = **10,400 hrs/month**, **$31,824/month**
B) RI: **$268,056/month**, 10,400/146,000 = **7.1% utilization**, **WASTE $236,232/month** 💸
C) Spot: **$9,547/month**, saves **$22,277 vs OD**, **saves $258,509 vs RI**

**Answer: C** - 100% Spot with checkpointing saves 70%

**Why:** Fault-tolerant (checkpointing) + low utilization (7%) = Spot is perfect

---

### Question 7:

| Type | Cross-Region | Cross-Family | Change Size | Use Case |
|------|--------------|--------------|-------------|----------|
| Standard RI | **NO** | **NO** | **NO** | Max savings, fixed architecture |
| Convertible RI | **NO** | **YES** | **YES** | Regional flexibility, changeable |
| EC2 Instance SP | **YES** | **NO** | **YES** | Cross-region, family-locked |
| Compute SP | **YES** | **YES** | **YES** | Maximum flexibility |

**Ranking:** 1. Compute SP, 2. EC2 Instance SP, 3. Convertible RI, 4. Standard RI

---

### Question 8:
**Option A:**
- Baseline: $3,729 OD
- SP commitment: $3,729 / 0.60 = **$6,215** covers this usage, so commitment = **$2,237**
- Spot: **$1,102**
- Total: **$3,339/month**
- Savings: **$4,062 (55%)**

**Option B:**
- Commitment: **$3,701**
- Covers: $6,168 usage
- Remaining: $1,233 OD
- Total: **$4,934/month**
- Savings: **$2,467 (33%)**

**Answer: A** - Saves $1,595/month more than Option B

---

### Question 9:
**Option A:** $495 + $2,100 = **$2,595/month**, **Meets 8-hr? NO** (12 hrs) ❌
**Option B:** $1,800 + $3,150 = **$4,950/month**, **Meets 8-hr? YES** ✅

**Answer: C** - Glacier Flexible Standard retrieval (3-5 hrs meets 8-hr requirement, cheaper than Expedited)

---

### Question 10:
- Break-even: **438 hours/month** (60% utilization)
- At 500 hrs: OD = $500, RI = $438, **RI saves $62** ✅
- At 300 hrs: OD = $300, RI = $438, **RI wastes $138** ❌

**Conclusion:** RIs only make sense at **>438 hrs/month (60% utilization)**

---

## SCORING

**9-10 correct:** ✅ You're ready for more advanced drills
**7-8 correct:** ⚠️ Redo Cost Calculation Worksheet sections you missed
**<7 correct:** 🔴 Start over with Cost Calculation Worksheet - do all 8 drills

**Questions you MUST get right:**
- Q1, Q3, Q6: RI utilization (your biggest weakness)
- Q2, Q5, Q9: S3 total cost including retrieval
- Q4, Q7: Commitment flexibility

If you missed ANY of these, you're not ready for the exam.

