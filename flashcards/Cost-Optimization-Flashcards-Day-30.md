# Cost Optimization Flashcards - Day 30 Emergency Drill

**Created:** February 2, 2026
**Purpose:** Eliminate chronic weaknesses identified in Cost Optimization Drill (13/20 = 65%)
**Target:** Achieve 100% accuracy on these patterns before Practice Exam 2
**Focus Areas:** S3 Glacier pricing, Cost optimization hierarchy, EBS IOPS, Serverless economics

---

## 🔴 CRITICAL: S3 Glacier Instant Access Pricing (4 FAILURES - HIGHEST PRIORITY)

### Card 1: S3 Glacier Instant - Storage Cost

**Q:** What is the storage cost per GB-month for S3 Glacier Instant Access?

**A:** $0.004/GB-month

**Context:** This is CHEAPER storage than Standard-IA ($0.0125), BUT retrieval is MORE EXPENSIVE ($0.03 vs $0.01)

---

### Card 2: S3 Glacier Instant - Retrieval Cost

**Q:** What is the retrieval cost per GB for S3 Glacier Instant Access?

**A:** $0.03/GB

**Context:** This is 3× MORE EXPENSIVE than Standard-IA ($0.01) and Glacier Flexible ($0.01). This is why Glacier Instant is NOT for "occasional access"!

---

### Card 3: S3 Standard-IA Total Cost

**Q:** Calculate total cost for 100 GB with 2 retrievals per month using S3 Standard-IA.

**A:**
- Storage: 100 GB × $0.0125 = $1.25
- Retrieval: 2 × 100 GB × $0.01 = $2.00
- **Total: $3.25/month**

---

### Card 4: S3 Glacier Instant Total Cost

**Q:** Calculate total cost for 100 GB with 2 retrievals per month using S3 Glacier Instant.

**A:**
- Storage: 100 GB × $0.004 = $0.40
- Retrieval: 2 × 100 GB × $0.03 = $6.00
- **Total: $6.40/month (2× more expensive than Standard-IA!)**

---

### Card 5: S3 Glacier Flexible Total Cost

**Q:** Calculate total cost for 100 GB with 2 retrievals per year using S3 Glacier Flexible.

**A:**
- Storage: 100 GB × $0.0036 × 12 months = $4.32/year
- Retrieval: 2 × 100 GB × $0.01 = $2.00/year
- **Total: $6.32/year**

---

### Card 6: When to Use S3 Glacier Instant

**Q:** When should you use S3 Glacier Instant Access? (Give specific use case)

**A:**
- Archived data requiring **instant retrieval** (milliseconds)
- **Very rare** retrievals (< 1 per quarter)
- Examples: Medical images for compliance, legal documents for audits
- **NOT for "occasional access" or "queried occasionally"!**

---

### Card 7: S3 Storage Class for "Occasional Access"

**Q:** A scenario says data is "accessed occasionally" or "queried 1-2 times per month". Which S3 storage class is MOST cost-effective?

**A:** **S3 Standard-IA**

**Why NOT Glacier Instant?**
- Standard-IA: $0.0125 storage + $0.01 retrieval = $0.0225-0.0325/GB total
- Glacier Instant: $0.004 storage + $0.03 retrieval = $0.034-0.064/GB total
- Standard-IA is cheaper for any access frequency > 0.3×/month

---

### Card 8: S3 Storage Class for "Rarely Accessed"

**Q:** A scenario says data is "rarely accessed" or "accessed quarterly for audits". Which S3 storage class is MOST cost-effective?

**A:** **S3 Glacier Flexible Retrieval**

**Why?**
- Cheapest for infrequent access with acceptable retrieval time (3-5 hours)
- $0.0036/GB storage + $0.01/GB retrieval
- 90-day minimum storage duration

---

### Card 9: S3 Storage Class for "Never Accessed"

**Q:** A scenario says data is "never accessed after 1 year" but must be retained for compliance. Which S3 storage class is MOST cost-effective?

**A:** **S3 Glacier Deep Archive**

**Why?**
- Cheapest long-term storage: $0.00099/GB-month
- 180-day minimum storage duration
- 12-hour retrieval time acceptable for "never accessed" data

---

### Card 10: S3 Cost Hierarchy (Memorize!)

**Q:** List S3 storage classes from MOST to LEAST expensive (storage cost only).

**A:**
1. S3 Standard: $0.023/GB-month
2. S3 Intelligent-Tiering: $0.023/GB-month* (*plus monitoring)
3. S3 Standard-IA: $0.0125/GB-month
4. S3 One Zone-IA: $0.01/GB-month
5. **S3 Glacier Instant: $0.004/GB-month** ← NOT cheapest overall!
6. **S3 Glacier Flexible: $0.0036/GB-month** ← Cheaper storage than Instant!
7. S3 Glacier Deep Archive: $0.00099/GB-month (cheapest)

---

### Card 11: S3 Retrieval Cost Ranking

**Q:** Rank S3 storage classes by retrieval cost (highest to lowest).

**A:**
1. **S3 Glacier Instant: $0.03/GB** (MOST EXPENSIVE!)
2. S3 Glacier Deep Archive: $0.02/GB
3. S3 Standard-IA: $0.01/GB
4. S3 Glacier Flexible: $0.01/GB
5. S3 Standard: FREE

**Key insight:** Glacier Instant has HIGHEST retrieval cost!

---

### Card 12: S3 Exam Keyword Mapping

**Q:** Match the exam keywords to the correct S3 storage class:
- "Occasional access"
- "Rarely accessed"
- "Never accessed"
- "Unpredictable patterns"
- "Instant retrieval required for archived data"

**A:**
- "Occasional access" → **S3 Standard-IA**
- "Rarely accessed" → **S3 Glacier Flexible**
- "Never accessed" → **S3 Glacier Deep Archive**
- "Unpredictable patterns" → **S3 Intelligent-Tiering**
- "Instant retrieval for archived data" → **S3 Glacier Instant**

---

## 🔴 CRITICAL: Cost Optimization Hierarchy (3+ FAILURES)

### Card 13: The Golden Rule

**Q:** What is the correct order for cost optimization actions?

**A:**
1. **RIGHTSIZE FIRST** (eliminate waste)
2. **COMMIT SECOND** (Reserved Instances / Savings Plans)
3. **SCHEDULE THIRD** (Auto Scaling / turn off when not needed)

**NEVER commit to unoptimized infrastructure!**

---

### Card 14: Rightsizing Math

**Q:** You have 20 × m5.2xlarge instances running at 25% CPU. What should you do FIRST?

**A:** **Rightsize to 10 × m5.xlarge instances** (50% immediate savings)

**THEN commit with Reserved Instances** (additional 40-60% on remaining)

**NOT:** Buy RIs for oversized instances (locks in waste for 1-3 years)

---

### Card 15: Lambda Cost Optimization

**Q:** Lambda function runs for 2 minutes with 8 GB memory. You can optimize code to run in 1 minute. What should you do FIRST?

**A:** **Optimize code to 1 minute** (50% savings in GB-seconds)

**THEN consider Compute Savings Plan** (17% additional on optimized cost)

**NOT:** Buy Savings Plan first (17% savings on wasteful code)

**Math:**
- Optimize first then commit: 50% + (17% of 50%) = 58.5% total savings
- Commit first: 17% savings only

---

### Card 16: Low Utilization Signal

**Q:** CloudWatch shows EC2 instances at 25% CPU utilization for 30 days. What's the FIRST action?

**A:** **Rightsize the instances** (reduce instance type/count)

**NOT:**
- Buy Reserved Instances (locks in waste)
- Implement scheduling (doesn't fix fundamental oversizing)
- Enable Auto Scaling (still oversized when running)

---

### Card 17: Cost Optimization for "MOST Cost-Effective"

**Q:** Exam question asks for "MOST cost-effective solution" with low resource utilization. What should you look for FIRST?

**A:** **Rightsizing or serverless options**

**Order of evaluation:**
1. Can we eliminate the resource? (serverless, consolidation)
2. Can we rightsize it? (smaller instance, less capacity)
3. Can we commit to it? (RIs/Savings Plans after optimization)
4. Can we schedule it? (turn off during idle periods)

---

### Card 18: Commitment Trap

**Q:** Why is buying Reserved Instances for oversized infrastructure a mistake?

**A:**
- You're **locking in waste for 1-3 years**
- Example: 25% CPU utilization = 75% waste
- RI gives 40% discount on 100% of infrastructure
- Rightsizing gives 75% savings immediately
- **Math:** 40% discount on waste < 75% elimination of waste

---

## 🔴 CRITICAL: Serverless Economics (1 FAILURE)

### Card 19: Utilization Threshold for Serverless

**Q:** What utilization percentage is the threshold for choosing serverless over persistent infrastructure?

**A:** **< 30% utilization → Serverless** (Lambda, Glue, Fargate Spot)

**Calculation:**
- Runtime hours / Total hours = Utilization %
- Example: 4 hours/week ÷ 168 hours/week = 2.4% → Serverless!

---

### Card 20: Serverless vs Persistent Cost

**Q:** Spark job runs 4 hours per week. Compare costs:
- Persistent EMR cluster: $14,000/month
- AWS Glue serverless: ???

**A:** **AWS Glue: ~$1,500/month** (90% savings!)

**Why?**
- Glue charges only for 4 hours/week runtime
- EMR charges for 168 hours/week (24/7)
- 2.4% utilization = serverless is 10× cheaper

---

### Card 21: Serverless Service Selection

**Q:** Match the use case to the serverless service:
- Batch ETL jobs (4 hours/week)
- API functions (< 15 min runtime)
- Container workloads (sporadic)
- Long-running batch (fault-tolerant)

**A:**
- Batch ETL jobs → **AWS Glue**
- API functions → **Lambda**
- Container workloads → **Fargate** (or Fargate Spot)
- Long-running batch → **AWS Batch** with Spot

---

### Card 22: When NOT to Use Serverless

**Q:** When should you use persistent infrastructure instead of serverless?

**A:**
- **Utilization > 70%** → Persistent + Reserved Instances is cheaper
- **Constant 24/7 load** → Always-on infrastructure more cost-effective
- **Large state requirements** → Lambda has memory/storage limits

**Example:** Database running 24/7 → RDS Reserved Instance, not serverless

---

## 🔴 MODERATE: EBS Volume IOPS Limits (3 FAILURES)

### Card 23: EBS gp3 IOPS Limit

**Q:** What is the maximum IOPS for an EBS gp3 volume?

**A:** **16,000 IOPS**

**Cost:** $0.005 per provisioned IOPS-month

---

### Card 24: EBS io2 IOPS Limits

**Q:** What are the IOPS limits for EBS io2 volumes?

**A:**
- **Small volumes:** Up to 64,000 IOPS
- **Large volumes (≥16 TiB):** Up to 256,000 IOPS

**Cost:** $0.065 per provisioned IOPS-month

**KEY INSIGHT:** io2 can reach 256K IOPS! Don't jump to Block Express!

---

### Card 25: EBS io2 Block Express IOPS

**Q:** What is the maximum IOPS for io2 Block Express?

**A:** **256,000 IOPS** (same as large io2 volumes)

**Cost:** $0.119 per provisioned IOPS-month (83% MORE expensive than io2)

**When to use:** Only if io2 cannot provide required IOPS (which is rare)

---

### Card 26: EBS IOPS Cost Comparison

**Q:** Calculate monthly cost for 200,000 IOPS:
- io2 (regular)
- io2 Block Express

**A:**
- **io2:** 200,000 × $0.065 = $13,000/month ✅ Correct answer
- **io2 Block Express:** 200,000 × $0.119 = $23,800/month ❌ Over-provisioned

**Savings:** $10,800/month = $129,600/year by choosing io2!

---

### Card 27: EBS Over-Provisioning Trap

**Q:** Scenario requires 200,000 IOPS. Which volume type is MOST cost-effective?

**A:** **io2 with 200,000 IOPS provisioned**

**NOT io2 Block Express!**
- io2 can provide 256K IOPS for volumes ≥16 TiB
- Cheaper per IOPS than Block Express ($0.065 vs $0.119)
- "MOST cost-effective" = provision EXACTLY what's needed

---

### Card 28: EBS Volume Decision Tree

**Q:** Use this decision tree for EBS volume selection:

**A:**
```
Required IOPS?
├─ ≤ 16,000 IOPS → gp3 (cost-effective baseline)
├─ 16,001 - 256,000 IOPS → io2 (high performance)
└─ > 256,000 IOPS → io2 Block Express (extreme performance)

NEVER over-provision!
"MOST cost-effective" = exact match, not "more than needed"
```

---

## 🟡 MODERATE: S3 Lifecycle Transitions

### Card 29: Lifecycle Transition Cost

**Q:** What does it cost to transition objects between S3 storage classes using lifecycle policies?

**A:** **$0 (FREE!)**

**No retrieval fees, no data transfer charges**

Lifecycle transitions are direct tier-to-tier movements without retrieval.

---

### Card 30: Early Deletion Penalty Truth

**Q:** You have data in Glacier Flexible at day 45 (of 90-day minimum). If you transition to Deep Archive now vs day 90, which is cheaper?

**A:** **Same cost either way!**

**Why?**
- Early deletion penalty = you pay 90-day minimum REGARDLESS
- Transitioning at day 45: Pay for 45 remaining days
- Waiting until day 90: Pay for 45 remaining days in Glacier Flexible storage

**Benefit of transitioning NOW:** Start Deep Archive savings 45 days earlier!

---

### Card 31: Lifecycle vs Manual Delete/Re-upload

**Q:** Which costs more for 200 TB transition from Glacier to Deep Archive?
- Lifecycle policy
- Delete and re-upload

**A:**
- **Lifecycle policy:** $0 transition cost (only early deletion penalty if before minimum)
- **Delete and re-upload:**
  - Retrieval: 200 TB × $0.01/GB = $2,048
  - Early deletion penalty: Same as lifecycle
  - **MUCH more expensive!**

**ALWAYS use lifecycle policies for transitions!**

---

### Card 32: S3 Minimum Storage Durations

**Q:** What are the minimum storage durations for these S3 classes?
- Standard-IA
- Glacier Flexible
- Glacier Deep Archive

**A:**
- **Standard-IA:** 30 days
- **Glacier Flexible:** 90 days
- **Glacier Deep Archive:** 180 days

**If deleted early:** Charged for remaining days of minimum duration

---

### Card 33: S3 Early Deletion Buffer Strategy

**Q:** Reports are stored for 30 days, but occasionally deleted before 90 days (causing Glacier early deletion fees). How do you eliminate the fees?

**A:** **Use S3 Standard-IA as a buffer:**

```
Days 0-30: S3 Standard (frequent access)
Days 30-90: S3 Standard-IA (30-day minimum already met)
Days 90+: Glacier Flexible (90-day minimum met by time of transition)
```

**If deleted at day 60:**
- Still in Standard-IA (no Glacier penalty)
- Already past 30-day minimum (no Standard-IA penalty)
- **Zero early deletion fees!**

---

## 🔴 CRITICAL: Additional Cost Patterns

### Card 34: ALB vs NLB Cross-Zone Cost

**Q:** What is the cost for cross-zone load balancing?
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)

**A:**
- **ALB:** FREE (no cross-AZ data transfer charges)
- **NLB:** $0.01/GB cross-AZ data transfer

**For 5 TB/month:** ALB saves $51.20/month vs NLB

---

### Card 35: NAT Gateway vs NAT Instance

**Q:** When should you use NAT Instance instead of NAT Gateway?

**A:** **When cost is the priority over managed service:**

- NAT Gateway: $45/month + $0.045/GB data processing
- NAT Instance (t3.micro): ~$7/month + no data processing fees

**Trade-off:** NAT Instance requires management (patching, HA, scaling)

**Exam keyword:** "MOST cost savings" → NAT Instance

---

### Card 36: VPC Gateway Endpoints

**Q:** What is the cost for VPC Gateway Endpoints (S3, DynamoDB)?

**A:** **FREE!**

- No hourly charges
- No data processing charges
- Direct route from VPC to S3/DynamoDB without internet gateway

**Interface Endpoints cost money** ($7.20/AZ-month + data processing)

---

### Card 37: Aurora Serverless v2 Use Case

**Q:** When should you use Aurora Serverless v2 instead of provisioned Aurora?

**A:** **Variable traffic with >50% idle time:**

- Scales to 0.5 ACU during low traffic (vs paying for full capacity 24/7)
- Scales up in ~15 seconds for predictable patterns
- **Example:** Business hours only (8 AM-6 PM weekdays) = 70% idle time

**NOT for steady 24/7 traffic** (provisioned + Reserved Instances is cheaper)

---

### Card 38: DynamoDB On-Demand vs Provisioned

**Q:** When should you use DynamoDB On-Demand capacity mode?

**A:**
- **Unpredictable traffic** (can't forecast capacity needs)
- **Spiky workloads** that change "within seconds" (Auto Scaling takes minutes)
- **New applications** (don't know traffic patterns yet)

**Example:** "Traffic could spike 100× within seconds" → On-Demand

**NOT for predictable steady traffic** (Provisioned is cheaper)

---

### Card 39: CloudFront Cost Optimization

**Q:** How do you reduce CloudFront costs when you have high data transfer charges?

**A:** **Increase cache TTL to improve cache hit rate**

**Why?**
- Higher cache hit rate = fewer origin fetches from S3
- S3 → CloudFront costs $0.02/GB (origin fetch)
- CloudFront cache hit = no origin fetch needed
- **Example:** 85% → 95% cache hit rate saves 10% of origin fetch costs

---

### Card 40: Cost Optimization Final Check

**Q:** Before answering any cost optimization question, what three things should you check?

**A:**
1. **Calculate utilization** - Is it <30% (serverless), 30-70% (Auto Scale), or >70% (persistent)?
2. **Check the hierarchy** - Are they rightsizing BEFORE committing?
3. **Verify "MOST cost-effective"** - Are they provisioning EXACTLY what's needed, no more?

**RED FLAGS:**
- "Low CPU/memory utilization" + Reserved Instances = WRONG (rightsize first!)
- "Occasional access" + Glacier Instant = WRONG (Standard-IA cheaper!)
- "< 30% utilization" + persistent infrastructure = WRONG (serverless!)

---

## Drill Instructions

**How to Use These Flashcards:**

1. **First Pass (Tonight):** Read all 40 cards front and back, focus on understanding
2. **Second Pass (Tomorrow AM):** Quiz yourself on fronts, check backs only when stuck
3. **Third Pass (Tomorrow PM):** Speed drill - aim for <5 seconds per card
4. **Verification:** Retake Cost Optimization quiz (target 18/20 = 90%+)

**Focus Priority:**
- **HIGHEST:** Cards 1-12 (S3 Glacier pricing) - 4 failures on this pattern
- **HIGH:** Cards 13-18 (Cost hierarchy) - 3+ failures
- **MEDIUM:** Cards 19-22 (Serverless) - 1 failure but high-impact
- **MEDIUM:** Cards 23-28 (EBS IOPS) - 3 failures, easy to fix with memorization

**Success Criteria:**
- 100% accuracy on Cards 1-12 (S3 Glacier)
- 100% accuracy on Cards 13-18 (Cost hierarchy)
- 100% accuracy on cost calculations (Cards 3-5, 15, 26)

**Next Milestone:** Practice Exam 2 (Feb 7-8) - Target 50/65 (77%)

---

**Last Updated:** February 2, 2026, 9:47 PM CST
**Days Until Exam:** 9
