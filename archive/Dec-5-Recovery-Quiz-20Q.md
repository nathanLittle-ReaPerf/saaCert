# December 5 Recovery Quiz - 20 Questions

**Purpose:** Get back on track after being sick. Assess current knowledge of Week 1 weak areas.
**Target Score:** 16/20 (80%)
**Time Limit:** 40 minutes (2 min per question)
**Topics:** S3 Storage Classes, VPC NACLs, Encryption/KMS, Auto Scaling, EC2/VPC Concepts

---

## Instructions

1. Answer all 20 questions below
2. Write down your answers (A, B, C, or D)
3. Time yourself (40 minutes max)
4. Check answers at the bottom
5. Review explanations for ANY question you missed OR guessed on

---

## Questions

### Question 1
A company stores customer data that is accessed frequently for the first 30 days after upload, then rarely accessed afterward. When accessed after 30 days, the data must be available **immediately**. The solution must be cost-effective.

Which S3 storage class combination should you recommend?

A) Store in S3 Standard, transition to S3 Glacier Flexible Retrieval after 30 days
B) Store in S3 Standard, transition to S3 Standard-IA after 30 days
C) Store in S3 Standard, transition to S3 Glacier Instant Retrieval after 30 days
D) Store in S3 Intelligent-Tiering for the entire lifecycle

---

### Question 2
A company's VPC has a Network ACL with the following rules:

**Inbound Rules:**
- Rule 100: Allow TCP port 443 from 0.0.0.0/0
- Rule \*: Deny all

**Outbound Rules:**
- Rule 100: Allow TCP port 80 to 0.0.0.0/0
- Rule \*: Deny all

Users report they cannot access the web application running on EC2 instances in this subnet via HTTPS. What is the problem?

A) The Security Group is blocking HTTPS traffic
B) The outbound NACL rule needs to allow ephemeral ports (1024-65535) for return traffic
C) The inbound NACL rule should allow port 80, not port 443
D) NACLs cannot be used with HTTPS traffic

---

### Question 3
A Solutions Architect must encrypt data at rest in an S3 bucket. The company's security team requires that the encryption keys be managed by AWS, but they need to maintain control over key rotation. Which encryption option should be used?

A) SSE-S3 (Server-Side Encryption with S3-managed keys)
B) SSE-KMS (Server-Side Encryption with AWS KMS-managed keys)
C) SSE-C (Server-Side Encryption with Customer-provided keys)
D) Client-Side Encryption

---

### Question 4
An application running on EC2 instances experiences predictable traffic spikes every Monday at 9 AM and unpredictable traffic spikes throughout the week. Which Auto Scaling policy combination provides the BEST solution?

A) Target Tracking Scaling only
B) Scheduled Scaling only
C) Scheduled Scaling for Monday 9 AM + Target Tracking Scaling for unpredictable spikes
D) Step Scaling with multiple thresholds

---

### Question 5
A company needs to launch EC2 instances for a high-performance computing (HPC) workload that requires extremely low latency between instances. The workload cannot tolerate performance variability. Which EC2 Placement Group should be used?

A) Spread Placement Group
B) Partition Placement Group
C) Cluster Placement Group
D) No placement group needed

---

### Question 6
A company has a file that is 100 GB and is accessed once every 6 months for auditing purposes. When accessed, the file must be retrieved **within 5 minutes**. What is the MOST cost-effective S3 storage class?

A) S3 Standard-IA
B) S3 Glacier Instant Retrieval
C) S3 Glacier Flexible Retrieval with Expedited retrieval
D) S3 Glacier Deep Archive with Standard retrieval

---

### Question 7
A Security Group has the following inbound rule:
- Type: HTTP, Protocol: TCP, Port: 80, Source: 0.0.0.0/0

An EC2 instance with this security group can receive HTTP requests. Which statement is TRUE about outbound rules?

A) You must add an outbound rule to allow response traffic on port 80
B) You must add an outbound rule to allow ephemeral ports (1024-65535)
C) No outbound rule is needed; Security Groups are stateful and automatically allow return traffic
D) You must add an outbound rule to allow all traffic (0.0.0.0/0)

---

### Question 8
A company must encrypt an RDS database at rest. The security team requires the ability to audit all encryption key usage. Which solution meets these requirements?

A) Use RDS default encryption (AWS-managed keys)
B) Use AWS KMS with a Customer Managed Key (CMK)
C) Use AWS CloudHSM
D) Enable SSL/TLS for data in transit

---

### Question 9
An Auto Scaling group has a desired capacity of 4 instances. A scale-out alarm triggers and adds 2 instances (desired capacity = 6). Before the new instances finish launching and become healthy, another scale-out alarm triggers. What happens?

A) Auto Scaling waits for the previous scaling activity to complete before starting the new one
B) Auto Scaling adds more instances immediately, potentially causing over-scaling
C) Auto Scaling ignores the second alarm
D) Auto Scaling terminates the unhealthy instances and starts over

---

### Question 10
A company is running a three-tier web application across multiple Availability Zones. The application tier is in private subnets and needs to download software updates from the internet. The solution must be highly available and NOT expose the instances to inbound internet traffic. What should be implemented?

A) Internet Gateway
B) NAT Gateway in each Availability Zone
C) NAT Instance in one Availability Zone
D) VPC Peering

---

### Question 11
A company stores archived documents in S3. The documents are almost never accessed but must be retained for 7 years for compliance. On rare occasions (once per year), documents must be retrieved **within 12 hours**. What is the MOST cost-effective storage class?

A) S3 Glacier Instant Retrieval
B) S3 Glacier Flexible Retrieval
C) S3 Glacier Deep Archive with Standard retrieval (12 hours)
D) S3 Standard-IA

---

### Question 12
A Network ACL is configured with default rules (all traffic allowed). A Solutions Architect adds a new rule:
- Rule 50: Deny TCP port 22 from 0.0.0.0/0

There is also an existing rule:
- Rule 100: Allow all traffic from 0.0.0.0/0

What is the result?

A) SSH traffic is denied because rule 50 has a lower number and is evaluated first
B) SSH traffic is allowed because rule 100 explicitly allows all traffic
C) The rules conflict and the NACL will not work
D) SSH traffic is denied only for new connections, existing connections remain

---

### Question 13
A company requires that all data stored in S3 be encrypted. The encryption keys must be stored in a dedicated hardware security module (HSM) that meets FIPS 140-2 Level 3 requirements. Which solution should be used?

A) SSE-S3
B) SSE-KMS with AWS-managed CMK
C) SSE-KMS with Customer-managed CMK
D) SSE-KMS with CloudHSM custom key store

---

### Question 14
An application running on EC2 instances experiences steady traffic from 6 AM to 10 PM daily, and minimal traffic overnight. The application must maintain at least 2 instances at all times. Which Auto Scaling configuration is MOST cost-effective?

A) Scheduled Scaling: Min=2, Max=10, Desired=2 (overnight), Desired=8 (daytime)
B) Target Tracking Scaling: Min=2, Max=10, Target CPU=50%
C) Scheduled Scaling for daytime ramp-up + Target Tracking for dynamic adjustment
D) Step Scaling with CloudWatch alarms

---

### Question 15
A company needs to deploy a distributed NoSQL database (Apache Cassandra) across multiple EC2 instances. The application requires instances to be spread across underlying hardware to reduce the risk of correlated failures. The company needs more than 7 instances per Availability Zone. Which Placement Group should be used?

A) Spread Placement Group (supports up to 7 instances per AZ)
B) Partition Placement Group (supports multiple partitions with many instances)
C) Cluster Placement Group
D) Multiple Spread Placement Groups

---

### Question 16
A company has data in S3 with **unpredictable access patterns**. Sometimes the data is accessed frequently, other times it's not accessed for months. The company wants to optimize costs automatically without manual intervention. Which storage class should be used?

A) S3 Standard-IA
B) S3 Glacier Flexible Retrieval
C) S3 Intelligent-Tiering
D) S3 One Zone-IA

---

### Question 17
An EC2 instance in a private subnet needs to communicate with the internet. A NAT Gateway is configured in a public subnet. The route table for the private subnet has the following routes:

- 10.0.0.0/16 → local
- 0.0.0.0/0 → igw-12345 (Internet Gateway)

The instance cannot reach the internet. What is the problem?

A) The NAT Gateway is not associated with an Elastic IP
B) The route table should point 0.0.0.0/0 to the NAT Gateway, not the Internet Gateway
C) The Security Group is blocking outbound traffic
D) Private subnets cannot access the internet

---

### Question 18
A company uses AWS KMS Customer Managed Keys (CMK) to encrypt EBS volumes. The security team needs to know which IAM users and roles have accessed the encryption keys. Where can this information be found?

A) AWS CloudWatch Logs
B) AWS CloudTrail logs
C) AWS Config
D) KMS key policy

---

### Question 19
An Auto Scaling group uses a Launch Template that specifies an instance type of t3.medium. The application requires consistent performance with no variability. However, during scale-out events, some instances experience performance throttling. What is the MOST likely cause?

A) The instance type is too small
B) T3 instances are burstable and have exhausted their CPU credits
C) The Auto Scaling group is scaling too quickly
D) The Launch Template is misconfigured

---

### Question 20
A company needs to allow EC2 instances in a VPC to access S3 buckets without traversing the internet. The solution must be cost-effective and secure. What should be implemented?

A) NAT Gateway
B) Internet Gateway
C) VPC Endpoint (Gateway Endpoint for S3)
D) VPN Connection

---

## Answer Key

1. **B** - S3 Standard → S3 Standard-IA
2. **B** - Outbound NACL needs ephemeral ports
3. **B** - SSE-KMS with AWS KMS
4. **C** - Scheduled + Target Tracking
5. **C** - Cluster Placement Group
6. **C** - Glacier Flexible with Expedited
7. **C** - Security Groups are stateful
8. **B** - KMS with CMK for audit trail
9. **B** - Auto Scaling adds immediately (can over-scale)
10. **B** - NAT Gateway in each AZ
11. **C** - Glacier Deep Archive (12-hour retrieval)
12. **A** - Rule 50 evaluated first (lower number)
13. **D** - CloudHSM custom key store (FIPS 140-2 Level 3)
14. **C** - Scheduled + Target Tracking
15. **B** - Partition Placement Group
16. **C** - S3 Intelligent-Tiering
17. **B** - Route should point to NAT Gateway
18. **B** - AWS CloudTrail
19. **B** - T3 burstable instances exhausted credits
20. **C** - VPC Gateway Endpoint (free for S3!)

---

## Detailed Explanations

<details>
<summary><strong>Question 1 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- First 30 days: S3 Standard (frequent access)
- After 30 days: S3 Standard-IA (infrequent access)
- **KEY POINT:** "Must be available **immediately**" = millisecond retrieval
- Standard-IA provides immediate (millisecond) retrieval at lower cost than Standard

**Why others are wrong:**
- **A:** Glacier Flexible Retrieval requires 3-5 hours (Standard) or 1-5 min (Expedited) - NOT immediate
- **C:** Glacier Instant Retrieval works, but more expensive than Standard-IA for this use case
- **D:** Intelligent-Tiering costs more than Standard → Standard-IA transition for known access pattern

**Exam Tip:** "Immediately" or "instant access" = Standard or Standard-IA (NOT Glacier unless it says "Instant Retrieval")
</details>

<details>
<summary><strong>Question 2 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- **NACLs are STATELESS** - must explicitly allow both inbound AND outbound traffic
- HTTPS request comes in on port 443 (allowed by inbound rule)
- Response goes out on **ephemeral ports (1024-65535)** - NOT on port 443!
- Current outbound rule only allows port 80, blocking the HTTPS response

**Why others are wrong:**
- **A:** We don't know the Security Group configuration (and SGs are stateful anyway)
- **C:** Port 443 is correct for HTTPS
- **D:** NACLs work fine with HTTPS

**Fix:** Add outbound rule: Allow TCP 1024-65535 to 0.0.0.0/0

**Exam Tip:** NACL = Stateless. Always think about return traffic and ephemeral ports!
</details>

<details>
<summary><strong>Question 3 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- SSE-KMS with AWS KMS-managed keys
- AWS manages the encryption keys
- You can control key rotation policies
- Provides CloudTrail audit logs of key usage

**Why others are wrong:**
- **A:** SSE-S3 - AWS manages everything, no control over rotation
- **C:** SSE-C - YOU provide and manage the keys (more work)
- **D:** Client-side encryption - you manage keys before uploading

**Exam Tip:** KMS = auditing + key rotation control. SSE-S3 = fully managed, no control.
</details>

<details>
<summary><strong>Question 4 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **Scheduled Scaling** handles the predictable Monday 9 AM spike
- **Target Tracking Scaling** handles unpredictable spikes throughout the week
- Combining both policies is the best practice for mixed patterns

**Why others are wrong:**
- **A:** Target Tracking alone is reactive - will be slow to respond to Monday spike
- **B:** Scheduled Scaling alone won't handle unpredictable spikes
- **D:** Step Scaling works but Target Tracking is simpler (less operational overhead)

**Exam Tip:** Predictable + Unpredictable patterns = Scheduled + Target Tracking
</details>

<details>
<summary><strong>Question 5 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **Cluster Placement Group** = lowest latency, highest throughput
- Instances placed in same rack/AZ
- Perfect for HPC, ML, low-latency applications
- Single AZ only (trade-off for performance)

**Why others are wrong:**
- **A:** Spread = max 7 instances per AZ, spread across hardware (for fault tolerance, not performance)
- **B:** Partition = for distributed systems like Kafka/Hadoop (fault tolerance within groups)
- **D:** No placement group = random placement, higher latency

**Exam Tip:**
- HPC/ML/Low Latency = **Cluster**
- Critical instances (max 7/AZ) = **Spread**
- Large distributed systems (Kafka/Cassandra) = **Partition**
</details>

<details>
<summary><strong>Question 6 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- Accessed once every 6 months = archive storage
- Must retrieve **within 5 minutes** = Glacier Flexible with Expedited (1-5 min)
- Most cost-effective for this rare access pattern

**Why others are wrong:**
- **A:** Standard-IA more expensive, designed for monthly access
- **B:** Glacier Instant Retrieval more expensive than Flexible (designed for quarterly access with instant retrieval)
- **D:** Deep Archive takes 12 hours minimum (too slow)

**Exam Tip:**
- Within minutes + archive = Glacier Flexible Expedited
- Within hours + archive = Glacier Flexible Standard
- 12+ hours + archive = Glacier Deep Archive
</details>

<details>
<summary><strong>Question 7 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **Security Groups are STATEFUL**
- If inbound traffic is allowed, return traffic is automatically allowed
- No outbound rule needed for response traffic

**Why others are wrong:**
- **A, B, D:** All incorrect because they assume Security Groups are stateless (they're not!)

**Exam Tip:**
- **Security Groups = STATEFUL** (return traffic automatic)
- **NACLs = STATELESS** (must explicitly allow return traffic)
</details>

<details>
<summary><strong>Question 8 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- KMS with Customer Managed Key (CMK) provides:
  - Encryption at rest
  - CloudTrail audit logs of all key usage
  - Key rotation control
  - Key policies for access control

**Why others are wrong:**
- **A:** Default encryption uses AWS-managed keys (limited audit visibility)
- **C:** CloudHSM is overkill and expensive for this requirement
- **D:** SSL/TLS encrypts data in transit, not at rest

**Exam Tip:** "Audit key usage" = KMS with CloudTrail integration
</details>

<details>
<summary><strong>Question 9 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- Auto Scaling does NOT wait for previous scaling activity to complete
- It adds instances immediately when a new alarm triggers
- This can cause over-scaling (launching too many instances)
- This is why cooldown periods are important

**Why others are wrong:**
- **A:** Auto Scaling doesn't wait (no built-in throttling without cooldown)
- **C:** Auto Scaling doesn't ignore alarms
- **D:** Instances aren't terminated during scale-out

**Exam Tip:** Be aware of Auto Scaling cooldown periods to prevent over-scaling
</details>

<details>
<summary><strong>Question 10 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- Private subnets need NAT Gateway for outbound internet access
- **One NAT Gateway per AZ** = high availability
- NAT Gateway in public subnet allows private instances to reach internet
- NAT Gateway does NOT allow inbound internet traffic

**Why others are wrong:**
- **A:** Internet Gateway alone won't work for private subnets
- **C:** NAT Instance in one AZ = single point of failure (not HA)
- **D:** VPC Peering doesn't provide internet access

**Exam Tip:** Private subnet + internet access = NAT Gateway (one per AZ for HA)
</details>

<details>
<summary><strong>Question 11 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- Almost never accessed (once per year) = deep archive storage
- 7-year retention = long-term archive
- **12-hour retrieval** acceptable = Glacier Deep Archive Standard retrieval (12 hours)
- **MOST cost-effective** option for this scenario

**Why others are wrong:**
- **A:** Glacier Instant Retrieval = more expensive, designed for quarterly access with instant retrieval
- **B:** Glacier Flexible = more expensive than Deep Archive
- **D:** Standard-IA = way too expensive for once-per-year access

**Exam Tip:** Almost never accessed + can wait 12 hours = Glacier Deep Archive (cheapest!)
</details>

<details>
<summary><strong>Question 12 Explanation</strong></summary>

**Correct Answer: A**

**Why A is correct:**
- NACL rules are evaluated in order from lowest number to highest
- Rule 50 (Deny SSH) is evaluated before Rule 100 (Allow all)
- Once a rule matches, evaluation stops
- SSH traffic is **DENIED**

**Why others are wrong:**
- **B:** Rule 100 is never reached because Rule 50 matches first
- **C:** Rules don't conflict; they're evaluated in order
- **D:** NACLs are stateless and apply to all connections

**Exam Tip:** NACL rule numbers matter! Lower numbers evaluated first.
</details>

<details>
<summary><strong>Question 13 Explanation</strong></summary>

**Correct Answer: D**

**Why D is correct:**
- **FIPS 140-2 Level 3** requirement = must use CloudHSM
- KMS is FIPS 140-2 Level 2 (not Level 3)
- CloudHSM custom key store integrates CloudHSM with KMS
- Allows SSE-KMS encryption using keys stored in CloudHSM

**Why others are wrong:**
- **A, B, C:** All use KMS which is FIPS 140-2 Level 2 (doesn't meet Level 3 requirement)

**Exam Tip:**
- FIPS 140-2 Level 2 = KMS
- FIPS 140-2 Level 3 = CloudHSM
- "Dedicated HSM" or "Level 3" = CloudHSM
</details>

<details>
<summary><strong>Question 14 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **Scheduled Scaling** pre-scales for known 6 AM ramp-up (proactive)
- **Target Tracking** handles variations during the day (reactive)
- Combining both = most cost-effective with best performance

**Why others are wrong:**
- **A:** Scheduled only = doesn't adapt to traffic variations
- **B:** Target Tracking only = slow to respond to 6 AM spike
- **D:** Step Scaling works but more complex than Target Tracking

**Exam Tip:** Predictable daily pattern + variations = Scheduled + Target Tracking
</details>

<details>
<summary><strong>Question 15 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- **Partition Placement Group** designed for large distributed systems
- Supports multiple partitions, each on separate hardware
- Perfect for Cassandra, Kafka, Hadoop, HDFS
- No 7-instance limit (unlike Spread)

**Why others are wrong:**
- **A:** Spread limited to 7 instances per AZ (not enough)
- **C:** Cluster doesn't provide fault isolation across hardware
- **D:** Multiple Spread groups = unnecessary complexity

**Exam Tip:**
- Cassandra/Kafka/Hadoop = **Partition Placement Group**
- Critical instances (<7 per AZ) = Spread
- HPC/low latency = Cluster
</details>

<details>
<summary><strong>Question 16 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **Unpredictable access patterns** = key phrase for Intelligent-Tiering
- Automatically moves objects between tiers based on access patterns
- No retrieval fees (unlike other IA classes)
- Ideal for unknown or changing access patterns

**Why others are wrong:**
- **A:** Standard-IA requires known infrequent access pattern
- **B:** Glacier requires known archive pattern
- **D:** One Zone-IA requires known infrequent access pattern

**Exam Tip:** "Unpredictable" or "unknown access patterns" = S3 Intelligent-Tiering
</details>

<details>
<summary><strong>Question 17 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- Private subnet route table should point 0.0.0.0/0 to **NAT Gateway** (not IGW)
- Internet Gateway is for public subnets
- NAT Gateway forwards traffic from private subnet to internet via IGW

**Route table should be:**
- 10.0.0.0/16 → local
- 0.0.0.0/0 → nat-gateway-id

**Why others are wrong:**
- **A:** NAT Gateway requires EIP, but that's not the issue here
- **C:** Security Groups allow all outbound by default
- **D:** Private subnets CAN access internet via NAT Gateway

**Exam Tip:**
- Public subnet: 0.0.0.0/0 → IGW
- Private subnet: 0.0.0.0/0 → NAT Gateway
</details>

<details>
<summary><strong>Question 18 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- **CloudTrail logs all KMS API calls**
- Shows which IAM principals used which keys
- Provides audit trail of key usage
- Can be integrated with CloudWatch Logs for analysis

**Why others are wrong:**
- **A:** CloudWatch Logs doesn't automatically log KMS usage (need CloudTrail)
- **C:** AWS Config tracks configuration changes, not usage
- **D:** KMS key policy controls access, doesn't log usage

**Exam Tip:** KMS audit trail = CloudTrail
</details>

<details>
<summary><strong>Question 19 Explanation</strong></summary>

**Correct Answer: B**

**Why B is correct:**
- **T3 instances are burstable** (use CPU credits)
- When CPU credits exhausted, performance drops to baseline
- "Consistent performance with no variability" = should NOT use burstable instances
- Should use M5, C5, or R5 instance types instead

**Why others are wrong:**
- **A:** Instance might be too small, but the real issue is the burstable nature
- **C:** Scaling speed doesn't cause throttling
- **D:** Launch Template itself isn't misconfigured

**Exam Tip:** T2/T3 = burstable (variable performance). M5/C5/R5 = consistent performance.
</details>

<details>
<summary><strong>Question 20 Explanation</strong></summary>

**Correct Answer: C**

**Why C is correct:**
- **VPC Gateway Endpoint for S3** = private connection to S3
- Traffic stays within AWS network (doesn't traverse internet)
- **FREE** (no data transfer or hourly charges for Gateway Endpoints)
- Secure (no internet exposure)

**Why others are wrong:**
- **A:** NAT Gateway costs money and uses internet
- **B:** Internet Gateway exposes traffic to internet
- **D:** VPN Connection unnecessary and costs money

**Exam Tip:**
- S3/DynamoDB from VPC = Gateway Endpoint (FREE!)
- Other AWS services = Interface Endpoint (costs money)
</details>

---

## Scoring Guide

**18-20 correct (90-100%):** Excellent! You've mastered Week 1 concepts. Move to databases.
**16-17 correct (80-85%):** Good! Review missed topics and proceed.
**14-15 correct (70-75%):** Need more review. Drill weak areas before moving on.
**Below 14 (< 70%):** Spend another day on Week 1 fundamentals.

---

## What to Do After This Quiz

### If you scored 80%+ (16/20):
✅ Great job! Move to **Day 2** of revised schedule (Encryption + Auto Scaling deep dive)
✅ Then take Week 1 comprehensive retake on Day 3

### If you scored 70-79% (14-15/20):
⚠️ Review the detailed explanations for ALL missed questions
⚠️ Spend 1 more hour drilling the specific weak topics
⚠️ Retake this quiz tomorrow before moving to Day 2

### If you scored below 70% (< 14/20):
❌ Extend Phase 1 by 1-2 days
❌ Review Day-7-Week-1-Deep-Dive-Review.md thoroughly
❌ Create flashcards for concepts you're struggling with
❌ Retake this quiz in 2 days

---

## Topic Breakdown (For Gap Analysis)

Track which questions you missed by topic:

**S3 Storage Classes:** Q1, Q6, Q11, Q16 (4 questions)
**VPC NACLs/Security Groups:** Q2, Q7, Q12, Q17 (4 questions)
**Encryption/KMS:** Q3, Q8, Q13, Q18 (4 questions)
**Auto Scaling:** Q4, Q9, Q14, Q19 (4 questions)
**EC2/VPC Concepts:** Q5, Q10, Q15, Q20 (4 questions)

---

**Good luck! Take your time and read each question carefully. Remember: elimination strategy works!** 🎯
