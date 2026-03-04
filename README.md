# AWS SAA-C03 Study Materials

Study materials for the **AWS Certified Solutions Architect - Associate (SAA-C03)** exam. Built over a 56-day study period, resulting in a **passing score of 789/1000** on March 2, 2026.

These materials are organized for reuse. Everything here is reference-quality — no filler, no fluff.

---

## Repository Structure

```
├── quick-reference/       # Per-service cheat sheets (the most useful folder)
├── practice/              # Exam-style scenario questions with full explanations
├── decision-trees/        # When-to-use-what decision trees for complex service choices
├── cheat-sheets/          # Specialized guides: storage comparison, exam strategy, etc.
├── flashcards/            # Printable flashcard sets by topic
├── study-plan/            # 56-day study schedule
└── archive/               # Daily drill notes, progress tracking, weakness log
```

---

## Where to Start

**If you have 4+ weeks:** Start with the [study plan](study-plan/Study-Schedule-Jan-5-Feb-10-2026.md) and use the quick-reference guides alongside it.

**If you have 1-2 weeks:** Go straight to [quick-reference/](quick-reference/) and [practice/](practice/). Do all three practice scenario files.

**If you have < 1 week:** [Exam-Strategy-Tips.md](cheat-sheets/Exam-Strategy-Tips.md) + [Master-Decision-Trees-All-Services.md](decision-trees/Master-Decision-Trees-All-Services.md) + [practice/](practice/).

---

## Quick Reference Guides

The most important files in this repo. Each covers a service category with tables, limits, and exam-pattern recognition sections.

| File | Covers |
|------|--------|
| [Quick-Reference-Compute.md](quick-reference/Quick-Reference-Compute.md) | EC2, Lambda, ECS, EKS, Elastic Beanstalk, Batch |
| [Quick-Reference-Storage.md](quick-reference/Quick-Reference-Storage.md) | S3, EBS, EFS, FSx, Storage Gateway, Snow Family |
| [Quick-Reference-Networking.md](quick-reference/Quick-Reference-Networking.md) | VPC, Route 53, CloudFront, Direct Connect, Transit Gateway |
| [Quick-Reference-Databases.md](quick-reference/Quick-Reference-Databases.md) | RDS, Aurora, DynamoDB, ElastiCache, Redshift |
| [Quick-Reference-Security-IAM.md](quick-reference/Quick-Reference-Security-IAM.md) | IAM, KMS, Secrets Manager, WAF, Shield, GuardDuty |
| [Quick-Reference-Monitoring-DR-Other.md](quick-reference/Quick-Reference-Monitoring-DR-Other.md) | CloudWatch, CloudTrail, Config, Backup, DR strategies |
| [Quick-Reference-Migration.md](quick-reference/Quick-Reference-Migration.md) | Snow Family, DataSync, Transfer Family, Migration Hub |
| [Quick-Reference-Analytics.md](quick-reference/Quick-Reference-Analytics.md) | Athena, QuickSight, Kinesis, EMR |

---

## Practice Scenarios

Exam-style multi-service scenario questions with detailed answer explanations — including why wrong answers are wrong.

| File | Description |
|------|-------------|
| [Practice-Scenarios.md](practice/Practice-Scenarios.md) | Core scenarios covering all domains |
| [Practice-Scenarios-Additional.md](practice/Practice-Scenarios-Additional.md) | Extended scenario set |
| [Advanced-Practice-Scenarios-Hard-Mode.md](practice/Advanced-Practice-Scenarios-Hard-Mode.md) | Harder questions, trickier distractors |

---

## Decision Trees

For services where the "which one do I use?" question is genuinely hard.

- [Master-Decision-Trees-All-Services.md](decision-trees/Master-Decision-Trees-All-Services.md) — Covers all major service categories
- [DynamoDB-Decision-Tree-Quick-Reference.md](decision-trees/DynamoDB-Decision-Tree-Quick-Reference.md) — DynamoDB access patterns, indexes, capacity modes
- [RDS-Aurora-Decision-Trees.md](decision-trees/RDS-Aurora-Decision-Trees.md) — RDS vs Aurora vs Aurora Serverless decision paths

---

## Key Things to Memorize

These come up constantly on the exam:

- **Lambda:** 15-min timeout, 10 GB memory, 1000 concurrent executions default
- **S3 storage classes:** Know retrieval times and minimum storage durations cold
- **DR strategies:** Backup/Restore → Pilot Light → Warm Standby → Multi-Site (RPO/RTO tradeoffs)
- **Load balancer cross-zone:** ALB = free, NLB/GWLB = costs money
- **VPC Endpoints:** Gateway (S3/DynamoDB, free), Interface (everything else, costs money)
- **RDS Multi-AZ:** Failover in 60-120 seconds. Read Replicas = async replication, not HA
- **Auto Scaling:** Scheduled (predictable) + Target Tracking (reactive spikes) = best combo
- **Route 53:** Geolocation = data residency by location, Latency = fastest region for user

---

## Exam Strategy

The SAA-C03 exam heavily uses keyword patterns. Read [Exam-Strategy-Tips.md](cheat-sheets/Exam-Strategy-Tips.md) before exam day.

Key patterns:
- "MOST cost-effective" → Spot Instances, S3 Glacier, Auto Scaling, reserved capacity
- "LEAST operational overhead" → Managed services (Lambda, Fargate, Elastic Beanstalk, Aurora Serverless)
- "MOST secure" → KMS encryption, MFA, least-privilege IAM, VPC endpoints
- "High availability" → Multi-AZ, Auto Scaling Groups, multi-region

---

## Archive

The [archive/](archive/) folder contains daily drill notes, the full progress tracker (Day 1 through exam day), and the weakness tracker showing 53+ identified weaknesses and how each was resolved. Useful if you want to see what a 56-day study journey looked like from a 56.9% low to a passing score.

---

## Exam Result

**Passed — 789/1000** (passing: 720)
Exam: AWS SAA-C03, March 2, 2026
