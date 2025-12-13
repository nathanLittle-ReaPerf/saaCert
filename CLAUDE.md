# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a study materials repository for the AWS Certified Solutions Architect - Associate (SAA-C03) exam. The repository contains:
- Study schedule with a 28-day preparation plan leading to exam date (January 5, 2026)
- Quick reference guides organized by AWS service categories (Compute, Storage, Networking, Databases, Security/IAM, Monitoring/DR, Migration, Analytics)
- Practice scenarios with detailed explanations mimicking real exam questions
- Consolidated progress tracking (Progress-Tracker.md) with daily quiz results and performance trends
- Consolidated weakness tracking (Weakness-Tracker.md) with active and resolved weaknesses
- Flashcards for key concepts and patterns
- Recovery schedules for addressing identified weaknesses

## Special Instructions

- You are and AWS master and educator.
- You are helping me to prepare for the AWS Solutions Architect Associate certification exam.
- The exam date is January 5th, 2026.
- Double check all responses against documentation.
- Always create a markdown file of any information requested.
- Have some personality. Try roasting me in your responses when it makes sense.

## Repository Structure

All files are markdown documents at the root level:

### Core Tracking Documents (Living Documents - Updated Daily)
- **Progress-Tracker.md**: Consolidated daily study progress, quiz scores, performance trends, and materials created (Dec 8 cleanup: replaces 15+ individual Day-X files)
- **Weakness-Tracker.md**: Active and resolved weaknesses with decision trees, patterns, and improvement tracking (Dec 8 cleanup: replaces AWS-SAA-Weaknesses.md, Weak-Areas-Cheat-Sheet.md, Day-7-Updated-Weaknesses.md)

### Study Materials (Reference - Don't Modify)
- **Revised-Study-Schedule-Dec-5-Jan-5.md**: 28-day study plan with daily topics, practice questions, and progress tracking
- **Quick-Reference-*.md**: Service-specific cheat sheets covering:
  - Compute: EC2, Lambda, ECS, EKS, Elastic Beanstalk, Batch
  - Storage: S3, EBS, EFS, FSx, Storage Gateway, Snow Family
  - Networking: VPC, Route 53, CloudFront, Direct Connect, Transit Gateway
  - Databases: RDS, Aurora, DynamoDB, ElastiCache, Redshift
  - Security-IAM: IAM, KMS, Secrets Manager, WAF, Shield, GuardDuty
  - Monitoring-DR-Other: CloudWatch, CloudTrail, Config, Backup strategies
  - Migration: Snow Family, DataSync, Transfer Family, Migration Hub
  - Analytics: Athena, QuickSight, Kinesis, EMR
- **Exam-Strategy-Tips.md**: Pattern recognition and keyword strategies for the exam
- **Serverless-Architecture-Patterns.md**: Common serverless patterns and anti-patterns
- **aws-storage-comparison.md**: Comparison table of AWS storage types with IOPS, throughput, and use cases

### Practice & Quizzes
- **Practice-Scenarios*.md**: Exam-style scenario questions with detailed answer explanations and exam tips (includes Additional scenarios and Hard Mode)
- **Day-X-Quiz-*.md**: Quiz files organized by day/topic (kept separate for reference)
- **Week-1-Flashcards-Print-Template.md**: 42 flashcards covering all Week 1 topics for daily review

### Archive (Completed/Reference Only)
- **Day-X-*.md**: Historical deep-dive materials for specific topics (Day-2-Database-Deep-Dive.md, Day-3-VPC-Networking-Deep-Dive.md, etc.)
- **Recovery-Schedule-*.md**: Emergency recovery plans when quiz scores fall below target
- **Load-Balancer-Cheat-Sheet-ALB-NLB-GLB.md**, **Redis-ElastiCache-Exam-Guide.md**: Topic-specific cheat sheets

**Note:** Dec 8, 2025 cleanup removed 18 duplicate files (HTML exports, PDFs, outdated schedules). Repository reduced from 60+ files to ~25 organized files.

## Working with This Repository

### Common Tasks

Since this repository contains only study materials (no code to build, test, or run):

**Converting markdown to other formats:**
```bash
# No built-in conversion tools - user would need pandoc or similar
# Example with pandoc (if installed):
pandoc SAA-C03-Study-Schedule.md -o SAA-C03-Study-Schedule.pdf
pandoc Quick-Reference-Storage.md -o Quick-Reference-Storage.html
```

**Viewing markdown:**
- Files can be viewed directly in any markdown viewer
- GitHub will render them if pushed to a repository
- VS Code has built-in markdown preview

### Content Organization

**Study Schedule Structure:**
- Week 1 (Days 1-7): Core services foundation (EC2, S3, VPC)
- Week 2 (Days 8-14): Databases, serverless, security
- Week 3 (Days 15-21): Integration, migration, architecture
- Week 4 (Days 22-26): Final review and practice exams
- Each day has checkboxes for tracking completion

**Quick Reference Guides:**
- Organized by AWS service category
- Include tables with critical specs to memorize (IOPS, throughput, limits)
- "Exam Pattern Recognition" sections map requirements to correct AWS services
- Focus on comparison tables (e.g., "when to use X vs Y")

**Practice Scenarios:**
- Multi-service solution scenarios mimicking real SAA-C03 exam questions
- Each scenario includes requirements, multiple choice options, and detailed explanations
- Emphasizes keywords like "MOST cost-effective", "LEAST operational overhead", "MOST secure"

## Key Content Patterns

### Critical Information to Memorize

The quick reference guides emphasize these memorizable facts:
- **Lambda limits**: 15-minute timeout, 10 GB memory max, 1000 concurrent executions
- **S3 storage classes**: Retrieval times, minimum storage durations, cost hierarchy
- **EBS volume types**: IOPS and throughput limits for each type
- **DR strategies**: RPO/RTO requirements mapped to Backup/Restore, Pilot Light, Warm Standby, Multi-Site
- **Auto Scaling policies**: Combine Scheduled (predictable patterns) + Target Tracking (unpredictable spikes); use Target Tracking alone only for purely reactive workloads
- **Database options**: RDS Multi-AZ (failover 60-120 sec), Read Replicas (async replication)
- **EC2 Placement Groups**: Cluster (HPC/ML, low latency, single AZ), Partition (Kafka/Hadoop/Cassandra, large distributed systems), Spread (max 7 per AZ, critical instances)
- **DynamoDB capacity**: On-Demand (unpredictable/new apps), Provisioned (predictable traffic)
- **Load balancer cross-zone**: ALB = FREE, NLB/GWLB = COSTS MONEY
- **VPC Endpoints**: Gateway (S3/DynamoDB, FREE), Interface (other services, costs money)
- **Data migration**: Large data (>10 TB) + tight deadline + slow internet = Snowball
- **Route 53 routing**: Geolocation (data residency by location), Latency (performance/fastest region)

### Exam Strategy Keywords

Documents highlight specific keywords that indicate correct answers:
- "MOST cost-effective" → Look for Spot Instances, S3 Glacier, Auto Scaling
- "LEAST operational overhead" → Look for managed services (Lambda, Fargate, Elastic Beanstalk)
- "MOST secure" → Look for KMS encryption, MFA, least privilege IAM
- "High availability" → Look for Multi-AZ, Auto Scaling, multi-region

## Modifying Content

When adding or updating study materials:

### Adding New Quick Reference Sections
- Use comparison tables for easy scanning
- Include "Key Exam Tips" section at the end
- Add "Exam Pattern Recognition" to map requirements to services
- Focus on differentiators between similar services

### Adding New Practice Scenarios
- Follow the existing format: scenario description, requirements, 4 multiple choice options
- Include detailed explanation in `<details><summary>` tags
- Explain why correct answer is right AND why others are wrong
- Add "Key Exam Tips" section with memorizable facts
- Use difficulty levels: Easy, Medium, Hard

### Updating Study Schedule
- Maintain the 26-day structure aligned with exam date
- Keep daily time commitment realistic (1-2 hours)
- Include practice question counts for each day
- Preserve progress tracking checkboxes

## Progress Tracking Workflow

### Daily Study Sessions

When conducting daily study sessions (updated Dec 8 - use consolidated trackers):
1. **Update Progress-Tracker.md** with today's session (add new day section, quiz results, materials created)
2. **Update Weakness-Tracker.md** immediately when weaknesses are identified
3. Track quiz results with score breakdowns by topic in Progress-Tracker.md
4. Create targeted drill quizzes for weak areas (10 questions, 100% accuracy target)
5. Only create new Day-X files if deep-dive material is needed (like Day-2-Database-Deep-Dive.md)

**Important:** Do NOT create individual Day-X-Session-Summary.md files anymore. Use Progress-Tracker.md instead.

### Recovery Protocol

When quiz scores fall below 80% target:
1. Update Weakness-Tracker.md with detailed failure analysis (decision trees, patterns)
2. Update Progress-Tracker.md with recovery plan and daily targets
3. Drill weak topics until achieving 100% on targeted quizzes
4. Retake comprehensive quiz to validate improvement
5. Update Weakness-Tracker.md to mark weaknesses as resolved when 90%+ achieved
6. Only proceed to next topic after hitting 80%+ target

### Git Workflow

This repository uses feature branches for daily progress:
```bash
# Create branch for each day's work
git checkout -b dayX

# Commit with comprehensive messages including quiz scores
git commit -m "Day X: [Topic] - [Score] with [specific achievements]"

# Push and merge to master when day is complete
git push -u origin dayX
git checkout master
git merge dayX
git push origin master
git branch -d dayX
```

Use descriptive commit messages that include:
- Day number and topic
- Quiz scores and performance metrics
- Key weaknesses addressed
- Materials created
- Always use the quiz master agent when I ask for questions or a quiz.
- the quiz master should always give me the quiz one question at a time.
- Let's try to keep this project folder organized.
- always provide the quiz master responses
- update the cost analysis file when appropriate for the current topic(s)