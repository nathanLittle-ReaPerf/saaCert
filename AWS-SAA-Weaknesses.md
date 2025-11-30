# AWS SAA-C03 Weakness Tracking

## 2025-11-28 - Week 1 Comprehensive Quiz

### Question 3: S3 Data Processing with 2-4 Hour Jobs
**Scenario:** Files uploaded to S3, each job takes 2-4 hours, app handles interruptions, need 60%+ cost savings, 24-hour SLA

**User's Answer:** C - Migrate to AWS Lambda functions triggered by S3 uploads, with 15-minute timeout

**Correct Answer:** B - Use Spot Instances with EC2 Auto Scaling triggered by S3 uploads

**Knowledge Gap:**
- **CRITICAL MISS:** Did not recognize that Lambda's 15-minute maximum timeout makes it IMPOSSIBLE for jobs that take 2-4 hours (120-240 minutes)
- Failed to connect "can handle interruptions and resume from checkpoint" as the key phrase indicating Spot Instances are ideal
- Did not recognize that Spot Instances provide up to 90% savings (easily exceeding the 60% requirement)

**Review Action:**
- **MEMORIZE:** Lambda maximum timeout = 15 minutes
- **MEMORIZE:** Lambda maximum memory = 10 GB
- **MEMORIZE:** Lambda concurrent execution default = 1000
- Re-study Quick-Reference-Compute.md - Lambda Limits section
- Review when to use Lambda vs EC2 (Lambda = short-lived, <15 min; EC2 = long-running tasks)
- **Exam Pattern:** "Can handle interruptions" or "can resume from checkpoint" = SPOT INSTANCES indicator
- **Exam Pattern:** 60%+ cost reduction = likely Spot Instances (up to 90% savings)

**Mnemonic:** Lambda is a SPRINT runner (15 min max), not a MARATHON runner (hours). Use EC2 for marathons.
