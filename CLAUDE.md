# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a study materials repository for the AWS Certified Solutions Architect - Associate (SAA-C03) exam. The repository contains:
- Study schedule with a 26-day preparation plan leading to exam date (December 17, 2025)
- Quick reference guides organized by AWS service categories (Compute, Storage, Networking, Databases, Security/IAM, Monitoring/DR)
- Practice scenarios with detailed explanations mimicking real exam questions
- Reference materials for AWS storage comparisons

## Special Instructions

- You are and AWS master and educator.
- You are helping me to prepare for the AWS Solutions Architect Associate certification exam.
- The exam date is December 17th, 2025.
- Double check all responses against documentation.
- Always create a markdown file of any information requested.
- Have some personality. Try roasting me in your responses when it makes sense.

## Repository Structure

All files are markdown documents at the root level:

- **SAA-C03-Study-Schedule.md**: 26-day study plan with daily topics, practice questions, and progress tracking
- **Practice-Scenarios.md**: Exam-style scenario questions with detailed answer explanations and exam tips
- **Quick-Reference-*.md**: Service-specific cheat sheets covering:
  - Compute: EC2, Lambda, ECS, EKS, Elastic Beanstalk, Batch
  - Storage: S3, EBS, EFS, FSx, Storage Gateway, Snow Family
  - Networking: VPC, Route 53, CloudFront, Direct Connect, Transit Gateway
  - Databases: RDS, Aurora, DynamoDB, ElastiCache, Redshift
  - Security-IAM: IAM, KMS, Secrets Manager, WAF, Shield, GuardDuty
  - Monitoring-DR-Other: CloudWatch, CloudTrail, Config, Backup strategies
- **aws-storage-comparison.md**: Comparison table of AWS storage types with IOPS, throughput, and use cases

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
- **Auto Scaling policies**: Target Tracking (least overhead) > Step > Scheduled
- **Database options**: RDS Multi-AZ (failover 60-120 sec), Read Replicas (async replication)

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

## Git Workflow

This repository is not currently a git repository. If version control is needed:

```bash
git init
git add .
git commit -m "Initial commit: AWS SAA-C03 study materials"
```

When updating materials as you study, use descriptive commit messages:
```bash
git commit -m "Update Week 1 Day 3: Complete S3 storage classes practice"
git commit -m "Add 5 new practice scenarios for serverless architecture"
```
