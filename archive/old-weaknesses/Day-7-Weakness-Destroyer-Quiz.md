# Day 7 Weakness Destroyer Quiz - 20 Questions
**Date: November 27, 2025**
**Target Score: 16/20 (80%+) to prove you've fixed your weak areas**
**Time Limit: 40 minutes (2 minutes per question)**

---

## Instructions

- Answer each question BEFORE looking at explanations
- Write down your answers (A, B, C, or D)
- No peeking at the answer key until you're done with all 20
- These questions specifically target YOUR documented weak areas
- If you score below 16/20 (80%), you need to re-read Day-7-Updated-Weaknesses.md

---

## Questions

### Question 1: S3 Storage Classes - Retrieval Times
**Difficulty: Medium** | **Your Weakness: Rarely Accessed Trap**

A company stores application logs in S3. Logs are accessed frequently for the first 30 days for debugging. After 30 days, logs are **rarely accessed** (1-2 times per month) but when accessed, developers need them **immediately** for troubleshooting production issues. After 180 days, logs can be archived with retrieval times up to 12 hours acceptable. What is the MOST cost-effective lifecycle policy?

**A.** Day 30 → S3 Glacier Flexible Retrieval with Expedited retrieval | Day 180 → S3 Glacier Deep Archive

**B.** Day 30 → S3 Standard-IA | Day 180 → S3 Glacier Deep Archive

**C.** Day 30 → S3 Glacier Instant Retrieval | Day 180 → S3 Glacier Deep Archive

**D.** Day 30 → S3 One Zone-IA | Day 180 → S3 Glacier Flexible Retrieval

---

### Question 2: VPC NACLs - Ephemeral Ports
**Difficulty: Hard** | **Your Weakness: Stateless Return Traffic**

An application running on EC2 instances in a private subnet needs to download software updates from the internet via HTTPS. The instances use a NAT Gateway in the public subnet for internet access. The Security Group allows all outbound traffic. The NACL has the following rules:

**Outbound Rules:**
- Rule 100: Allow TCP 443 to 0.0.0.0/0
- Rule 110: Allow TCP 80 to 0.0.0.0/0

**Inbound Rules:**
- Rule 100: Allow TCP 22 from 10.0.0.0/16
- Rule 110: Allow TCP 443 from 10.0.0.0/16

Instances can SSH to each other but experience **connection timeouts** when downloading updates. What is the problem?

**A.** The NAT Gateway is in the wrong subnet and should be moved to the private subnet

**B.** The Security Group is blocking return traffic because it's stateful

**C.** The NACL inbound rules don't allow return traffic on ephemeral ports (1024-65535) from 0.0.0.0/0

**D.** The route table is missing a route to the Internet Gateway

---

### Question 3: Auto Scaling Policies - Combining Scheduled and Target Tracking
**Difficulty: Medium** | **Your Weakness: Defaulting to Target Tracking Only**

An e-commerce website experiences the following traffic patterns:
- Predictable spike every day at 12 PM (lunch time) and 6 PM (after work)
- Random traffic spikes during flash sales (timing unpredictable)
- Gradual decline from 10 PM to 6 AM
- Application takes 3 minutes to fully warm up after launch

Which Auto Scaling configuration provides the BEST performance and cost optimization?

**A.** Target Tracking policy targeting 70% CPU utilization

**B.** Scheduled Actions at 11:45 AM, 5:45 PM, 10 PM combined with Target Tracking policy

**C.** Step Scaling policy with thresholds at 60%, 70%, 80%, 90% CPU

**D.** Predictive Scaling using ML to forecast traffic patterns

---

### Question 4: S3 Encryption - AWS Access to Keys
**Difficulty: Hard** | **Your Weakness: Picking SSE-KMS When AWS Should NOT Have Access**

A financial services company must encrypt sensitive customer data in S3 with the following requirements:
- **AWS must NOT have access to encryption keys or unencrypted data**
- Keys must be stored in FIPS 140-2 Level 3 validated HSMs
- Comprehensive audit logging of all cryptographic operations required
- Automatic key rotation every 90 days

Which solution meets ALL requirements?

**A.** SSE-KMS with customer-managed CMKs, enable automatic key rotation, enable CloudTrail logging

**B.** SSE-C (Customer-Provided Keys) with keys stored in AWS Secrets Manager

**C.** Client-side encryption using AWS CloudHSM for key generation and storage, encrypt before uploading to S3

**D.** SSE-S3 with default encryption enabled and S3 bucket key enabled for cost optimization

---

### Question 5: S3 Access Control - Pre-signed URLs vs Access Points
**Difficulty: Medium** | **Your Weakness: Choosing Access Points for Temporary Access**

A media company needs to share large video files (5-10 GB each) stored in S3 with external contractors for editing. The contractors do not have AWS accounts. Access should be valid for exactly 72 hours, after which the links should stop working. The solution should require the LEAST operational overhead.

Which solution meets these requirements?

**A.** Create an S3 Access Point for each contractor with time-based IAM policies that expire after 72 hours

**B.** Generate pre-signed URLs for the video files with a 72-hour expiration time using the AWS CLI

**C.** Create temporary IAM users for each contractor, provide access keys, and manually delete users after 72 hours

**D.** Enable S3 Block Public Access, then make objects public for 72 hours, then revert to private

---

### Question 6: Load Balancer - Cross-Zone Costs
**Difficulty: Easy** | **Your Weakness: Thinking ALB Cross-Zone Costs Money**

A company is using an Application Load Balancer (ALB) with targets in three Availability Zones. The ALB is experiencing uneven traffic distribution, with some instances receiving more traffic than others. The solutions architect recommends enabling cross-zone load balancing.

What is the cost impact of enabling cross-zone load balancing on an ALB?

**A.** $0.01 per GB of data transferred across Availability Zones

**B.** $0.05 per hour per Availability Zone with cross-zone enabled

**C.** No additional cost - cross-zone load balancing is enabled by default on ALB and is free

**D.** $0.01 per hour per cross-zone connection

---

### Question 7: S3 Storage Classes - "Almost Never Accessed"
**Difficulty: Medium** | **Your Weakness: Rarely vs Almost Never**

A healthcare company stores patient medical records in S3 for 10 years for regulatory compliance. Records are:
- Accessed frequently in the first 90 days (diagnostic period)
- **Almost never accessed** from day 91-365 (accessed maybe once per year for audits)
- Never accessed after year 1, but must be retained for 10 years
- When accessed after day 90, 5-hour retrieval time is acceptable

What is the MOST cost-effective storage strategy?

**A.** Day 0-90: S3 Standard | Day 90: S3 Standard-IA | Day 365: S3 Glacier Deep Archive | Day 3,650: Delete

**B.** Day 0-90: S3 Standard | Day 90: S3 Glacier Flexible Retrieval | Day 365: S3 Glacier Deep Archive | Day 3,650: Delete

**C.** Day 0-90: S3 Standard | Day 90: S3 Glacier Instant Retrieval | Day 365: S3 Glacier Deep Archive | Day 3,650: Delete

**D.** Day 0-90: S3 Standard | Day 90: S3 Intelligent-Tiering | Day 365: S3 Glacier Deep Archive | Day 3,650: Delete

---

### Question 8: EC2 Placement Groups - Hadoop/Kafka
**Difficulty: Medium** | **Your Weakness: Choosing Cluster for Distributed Systems**

A company is deploying a large-scale Apache Kafka cluster on EC2 for real-time data streaming. The deployment will have 20 broker nodes across 3 Availability Zones. The architecture must:
- Protect against correlated hardware failures (if one rack fails, other nodes continue)
- Support distributed system fault tolerance
- Balance between performance and availability

Which placement group strategy is MOST appropriate?

**A.** Cluster placement group for maximum network throughput between brokers

**B.** Partition placement group with 7 partitions per Availability Zone

**C.** Spread placement group to ensure each instance is on separate hardware

**D.** No placement group - rely on Auto Scaling for fault tolerance

---

### Question 9: VPC Security - NACLs vs Security Groups
**Difficulty: Medium** | **Your Weakness: Confusing Stateful and Stateless**

A solutions architect is designing security for a three-tier application. The application tier in a private subnet makes API calls to external third-party services over HTTPS. The application uses a NAT Gateway for outbound connectivity.

Which statement is correct about configuring NACLs for this scenario?

**A.** NACLs are stateful, so allowing outbound HTTPS (port 443) automatically allows return traffic

**B.** NACLs are stateless, so both outbound rule for port 443 and inbound rule for ephemeral ports (1024-65535) are required

**C.** NACLs only need outbound rules; Security Groups handle inbound return traffic

**D.** NACLs should allow all traffic (0.0.0.0/0) because Security Groups provide sufficient security

---

### Question 10: S3 Encryption - CloudHSM vs KMS
**Difficulty: Hard** | **Your Weakness: FIPS 140-2 Level 3 Requirements**

A government agency requires:
- FIPS 140-2 Level 3 validated hardware for encryption keys
- Keys must be generated and stored in hardware security modules under exclusive customer control
- AWS should have no technical means to access encryption keys
- Need to encrypt data in S3, EBS volumes, and RDS databases

Which solution meets ALL requirements?

**A.** AWS KMS with customer-managed CMKs in the customer's AWS account

**B.** AWS CloudHSM cluster in the customer's VPC, with client-side encryption for S3 and CloudHSM integration for EBS/RDS

**C.** SSE-KMS for S3, EBS encryption with KMS, RDS encryption with KMS - all using the same customer-managed CMK

**D.** AWS Secrets Manager with encryption using AWS-managed keys

---

### Question 11: Load Balancer - Static IP Requirements
**Difficulty: Easy** | **Your Weakness: (Previously Fixed, Testing Retention)**

A gaming company needs to deploy a load balancer for their UDP-based multiplayer game servers. Enterprise customers require static IP addresses for firewall whitelisting. The game requires extremely low latency (<100ms).

Which load balancer type should they use?

**A.** Application Load Balancer (ALB) with AWS Global Accelerator for static IPs

**B.** Network Load Balancer (NLB) with Elastic IP addresses

**C.** Gateway Load Balancer (GLB) for transparent traffic inspection

**D.** Classic Load Balancer (CLB) with Elastic IPs

---

### Question 12: Auto Scaling - Health Check Grace Period
**Difficulty: Medium** | **Your Weakness: Understanding Application Warm-up**

An application deployed in an Auto Scaling group takes 90 seconds to start up after instance launch (initialization scripts, database connections, cache warming). The ALB health check interval is 30 seconds with an unhealthy threshold of 2 consecutive failures.

Instances are being terminated shortly after launch. What is the MOST likely cause?

**A.** The health check interval (30 seconds) is too long

**B.** The health check grace period is too short (less than 90 seconds + 60 seconds buffer)

**C.** The unhealthy threshold (2) is too low and should be increased to 5

**D.** The ALB is using EC2 health checks instead of ELB health checks

---

### Question 13: S3 Storage Classes - Minimum Storage Duration
**Difficulty: Medium** | **Your Weakness: Cost Optimization with Early Deletion**

A company stores temporary data in S3 Standard-IA. Some objects are deleted after 15 days because they're no longer needed. The storage cost is higher than expected.

What is the cause of the unexpected cost?

**A.** S3 Standard-IA has a minimum storage duration of 30 days; deleting objects before 30 days still incurs charges for the full 30 days

**B.** S3 Standard-IA charges retrieval fees for any access, including deletion

**C.** S3 Standard-IA requires a minimum object size of 128 KB; smaller objects are charged as 128 KB

**D.** Deletion triggers a data transfer charge equivalent to the object size

---

### Question 14: VPC Endpoints - Gateway vs Interface
**Difficulty: Medium** | **Your Weakness: Day 6 New Material - Cost Optimization**

An application running in a private subnet frequently accesses both S3 buckets and DynamoDB tables. The company wants to eliminate NAT Gateway costs and keep traffic within the AWS network.

Which VPC endpoint configuration is MOST cost-effective?

**A.** Create VPC Interface Endpoints for both S3 and DynamoDB with PrivateDNS enabled

**B.** Create VPC Gateway Endpoints for both S3 and DynamoDB (free of charge)

**C.** Use AWS PrivateLink to connect to S3 and DynamoDB services

**D.** Create a single VPC Interface Endpoint that supports both S3 and DynamoDB

---

### Question 15: EC2 Instance Types - Storage Optimized
**Difficulty: Medium** | **Your Weakness: i3 vs d2 Selection**

A data analytics company needs to process large datasets (50+ TB) with sequential read/write patterns for data warehousing. The workload requires high throughput but does not require ultra-low latency random I/O.

Which EC2 instance family is MOST cost-effective?

**A.** i3 instances with NVMe SSD local storage for high IOPS

**B.** d2 instances with HDD-based local storage for high sequential throughput

**C.** m5 instances with EBS gp3 volumes for balanced performance

**D.** r6i instances with instance store for memory-optimized processing

---

### Question 16: S3 Encryption - Audit Trail Requirements
**Difficulty: Hard** | **Your Weakness: SSE-KMS vs Client-Side Use Cases**

A company needs to encrypt data in S3 with the following requirements:
- **Audit trail of all encryption and decryption operations via CloudTrail**
- Encryption keys managed by the company (not AWS default keys)
- Ability to disable or delete encryption keys to make data unreadable
- Data must be encrypted at rest

Which solution meets these requirements?

**A.** SSE-S3 with AWS-managed keys

**B.** SSE-KMS with customer-managed CMKs (Customer Master Keys)

**C.** SSE-C with customer-provided keys sent with each request

**D.** Client-side encryption with keys stored in AWS Secrets Manager

---

### Question 17: Auto Scaling - Combining Multiple Policies
**Difficulty: Hard** | **Your Weakness: When to Combine vs Single Policy**

An application experiences the following patterns:
- Predictable morning spike (8-9 AM) as users log in
- Steady baseline load throughout the day
- Unpredictable spikes during marketing campaigns
- Predictable evening drop (8-9 PM) as users log off

The company wants to minimize costs while ensuring performance. What is the BEST Auto Scaling strategy?

**A.** Single Target Tracking policy targeting 60% CPU to handle all scenarios

**B.** Scheduled Actions at 7:30 AM (scale up) and 8:30 PM (scale down) only

**C.** Scheduled Actions at 7:30 AM (scale up) and 8:30 PM (scale down), PLUS Target Tracking policy targeting 70% CPU

**D.** Step Scaling policy with aggressive scaling up at 70% CPU and scale down at 30% CPU

---

### Question 18: VPC NACLs - Troubleshooting Connectivity
**Difficulty: Hard** | **Your Weakness: Identifying NACL Issues**

A web application in a public subnet serves HTTPS traffic. The application can receive inbound HTTPS requests from the internet successfully, but when the application tries to make outbound API calls to external services (HTTPS), the connections time out.

Security Group allows all outbound traffic. The NACL has:
- **Inbound**: Allow TCP 443 from 0.0.0.0/0
- **Outbound**: Allow TCP 443 to 0.0.0.0/0

What is the problem?

**A.** The NACL is missing an outbound rule for ephemeral ports (1024-65535)

**B.** The NACL is missing an inbound rule for ephemeral ports (1024-65535)

**C.** The Security Group should explicitly allow outbound HTTPS instead of all traffic

**D.** The route table needs a route to the NAT Gateway for outbound traffic

---

### Question 19: Load Balancer - NLB Cross-Zone Costs
**Difficulty: Easy** | **Your Weakness: NLB vs ALB Cross-Zone Differences**

A company is using a Network Load Balancer (NLB) with targets distributed across 3 Availability Zones. They notice uneven traffic distribution and want to enable cross-zone load balancing.

What is the cost impact?

**A.** No cost - cross-zone load balancing is free for NLB, same as ALB

**B.** Cross-zone load balancing incurs data transfer charges ($0.01/GB) for traffic between AZs

**C.** Cross-zone load balancing is enabled by default on NLB and cannot be disabled

**D.** $0.05 per hour per NLB regardless of cross-zone settings

---

### Question 20: S3 Access Control - Pre-signed URL Security
**Difficulty: Medium** | **Your Weakness: Pre-signed URL Use Cases**

A company uses pre-signed URLs to grant temporary access to private S3 objects. Objects are encrypted using SSE-KMS with a customer-managed CMK.

What permissions does the IAM principal (user/role) generating the pre-signed URL need?

**A.** Only s3:GetObject permission on the S3 bucket

**B.** s3:GetObject on the bucket AND kms:Decrypt permission on the CMK

**C.** s3:PutObject permission because pre-signed URLs require upload capability

**D.** No permissions needed - pre-signed URLs work independently of IAM permissions

---

## Answer Key

<details>
<summary><strong>🚨 DO NOT EXPAND UNTIL YOU'VE ANSWERED ALL 20 QUESTIONS 🚨</strong></summary>

### Question 1: CORRECT ANSWER - B

**Explanation:**
- **Days 0-30**: S3 Standard (frequent access)
- **Days 31-180**: "Rarely accessed" + "need immediately" = **S3 Standard-IA**
  - Key insight: "Rarely accessed" does NOT mean archive it to Glacier
  - Standard-IA provides millisecond retrieval (immediate access)
  - Accessed 1-2x per month = infrequent but still needs instant access
  - Cost: $0.0125/GB/month (45% cheaper than Standard)
- **Days 181+**: "12 hours acceptable" = **S3 Glacier Deep Archive**
  - Cheapest: $0.00099/GB/month
  - 12-hour retrieval is within acceptable range

**Why others are wrong:**
- **A**: Glacier Flexible with Expedited retrieval costs extra ($0.03/GB retrieval fee) and is unnecessary when Standard-IA provides free instant access
- **C**: Glacier Instant Retrieval ($0.004/GB/month) is more expensive than Standard-IA for data accessed 1-2x per month
- **D**: One Zone-IA has lower durability/availability (99.5% vs 99.9%); Glacier Flexible at day 180 is more expensive than Deep Archive when 12-hour retrieval is acceptable

**Your Pattern:** You see "rarely accessed" and jump to Glacier. Read the retrieval requirement first!

---

### Question 2: CORRECT ANSWER - C

**Explanation:**
The issue is **NACL stateless behavior**:

1. **Outbound Request**: Instance → NAT Gateway → Internet (port 443) ✅
   - NACL outbound rule 100 allows this
2. **Return Response**: Internet → NAT Gateway → Instance (ephemeral port 32768-65535) ❌
   - NACL inbound rules only allow:
     - Port 22 from 10.0.0.0/16 (internal SSH)
     - Port 443 from 10.0.0.0/16 (internal HTTPS)
   - **Missing**: Inbound ephemeral ports from 0.0.0.0/0

**The fix:**
```
NACL Inbound Rule 120: Allow TCP 1024-65535 from 0.0.0.0/0
```

**Why others are wrong:**
- **A**: NAT Gateway must be in public subnet (not private) - this is correct as-is
- **B**: Security Groups are STATEFUL - they automatically allow return traffic (this is not the issue)
- **D**: Private subnets route to NAT Gateway (not Internet Gateway) - this is correct as-is

**Key Insight:** "Connection timeout" + "Security Group allows outbound" = **Check NACL inbound for ephemeral ports**

---

### Question 3: CORRECT ANSWER - B

**Explanation:**
This question has **BOTH** predictable and unpredictable patterns:

**Predictable Patterns:**
- 12 PM spike (lunch)
- 6 PM spike (after work)
- 10 PM decline

**Unpredictable Patterns:**
- Random flash sales

**Solution: COMBINE policies**

**Scheduled Actions** (proactive):
- 11:45 AM: Scale up to baseline capacity (15 min before lunch spike)
- 5:45 PM: Scale up to evening capacity (15 min before after-work spike)
- 10 PM: Scale down to night capacity
- **Why 15 minutes early?** App takes 3 min to warm up + instance launch takes 2-5 min = instances ready BEFORE spike

**Target Tracking** (reactive):
- Handles unpredictable flash sales
- Scales beyond scheduled capacity when needed
- Least operational overhead for unpredictable load

**Why others are wrong:**
- **A**: Target Tracking alone is reactive - instances won't be ready when 12 PM spike hits (users experience slow performance during 2-5 min instance launch + 3 min warm-up)
- **C**: Step Scaling requires more operational overhead (manual threshold management) and doesn't proactively scale for predictable patterns
- **D**: Predictive Scaling learns patterns but doesn't handle unpredictable flash sales as well as the combination approach

**Your Pattern:** You default to Target Tracking only. When question has BOTH predictable AND unpredictable → **COMBINE**.

---

### Question 4: CORRECT ANSWER - C

**Explanation:**
The critical requirement: **"AWS must NOT have access to encryption keys or unencrypted data"**

Let's evaluate each option:

| Option | AWS Has Access to Keys? | FIPS 140-2 Level 3? | Audit Logging? |
|--------|------------------------|-------------------|----------------|
| **SSE-KMS** | ✅ YES (AWS performs operations) | ❌ NO (Level 2/3) | ✅ YES (CloudTrail) |
| **SSE-C** | ⚠️ Partial (keys sent with requests) | ❌ NO | ❌ NO |
| **Client-side + CloudHSM** | ❌ NO (encrypt before upload) | ✅ YES (Level 3) | ✅ YES (CloudHSM logs) |
| **SSE-S3** | ✅ YES (AWS manages everything) | ❌ NO | ❌ NO |

**Why C is correct:**
- **Client-side encryption**: Encrypt data BEFORE uploading to S3 (AWS receives already-encrypted data)
- **CloudHSM**: FIPS 140-2 Level 3 validated HSMs in customer VPC
- **Exclusive control**: Keys generated and stored in CloudHSM cluster under customer control
- **AWS has NO access**: Keys never leave CloudHSM, AWS cannot decrypt
- **Audit logging**: CloudHSM provides detailed logs of all cryptographic operations
- **Automatic rotation**: Implement rotation logic using CloudHSM SDK

**Why others are wrong:**
- **A (SSE-KMS)**: AWS KMS performs encryption/decryption operations on behalf of customer. Even with customer-managed CMKs, AWS has operational access to perform cryptographic operations. KMS is FIPS 140-2 Level 2/3 (not pure Level 3).
- **B (SSE-C)**: Keys sent with each API request (operational complexity), no HSM storage, no automatic rotation, limited audit trail
- **D (SSE-S3)**: AWS fully manages keys, no customer control, no Level 3 compliance

**Your Pattern:** You pick SSE-KMS when you see "encryption" + "audit trail". But if question says "AWS should NOT have access", SSE-KMS is WRONG.

---

### Question 5: CORRECT ANSWER - B

**Explanation:**
Requirements:
- Share large video files (5-10 GB)
- External contractors (no AWS accounts)
- Temporary access (exactly 72 hours)
- LEAST operational overhead

**Pre-signed URLs are perfect:**
```bash
# Generate pre-signed URL valid for 72 hours (259,200 seconds)
aws s3 presign s3://bucket/video.mp4 --expires-in 259200
```

**Benefits:**
- ✅ No AWS credentials required for contractors
- ✅ Automatic expiration after 72 hours (no manual cleanup)
- ✅ Works with large files (5-10 GB)
- ✅ Can be generated in seconds
- ✅ Minimal operational overhead

**Why others are wrong:**
- **A (S3 Access Points)**: Access Points are for long-term multi-tenant access with different permissions per team. They do NOT natively support time-based expiration. Still require IAM credentials. Operationally complex for simple "72-hour share" requirement.
- **C (Temporary IAM users)**: High operational overhead - create users, generate access keys, distribute credentials securely, configure AWS CLI for contractors, manually delete users after 72 hours. Violates "LEAST operational overhead".
- **D (Public bucket)**: Security nightmare - makes files accessible to EVERYONE on the internet. No way to enforce 72-hour limit. Violates security best practices.

**Decision Tree:**
```
Temporary access + No AWS credentials + Time limit?
└─> Pre-signed URLs ✅
```

**Your Pattern:** You chose Access Points. Remember: Pre-signed URLs = temporary sharing, Access Points = long-term multi-tenant.

---

### Question 6: CORRECT ANSWER - C

**Explanation:**
For **Application Load Balancer (ALB)**:
- Cross-zone load balancing is **enabled by default**
- **NO additional cost** (completely free)
- Traffic distributed evenly across all registered targets in all enabled AZs

**Cross-Zone Load Balancing Comparison:**

| Load Balancer | Default Setting | Cost |
|--------------|----------------|------|
| **ALB** | ✅ Enabled | **FREE** ✅ |
| **NLB** | ❌ Disabled | **$0.01/GB** 💰 |
| **GLB** | ❌ Disabled | **$0.01/GB** 💰 |

**Why others are wrong:**
- **A**: $0.01/GB is the charge for **NLB** cross-zone data transfer (not ALB)
- **B**: No hourly charge for cross-zone on ALB
- **D**: No per-connection charge for cross-zone on ALB

**Memory Trick:**
- **ALB = Always free** (cross-zone enabled by default, NO COST)
- **NLB/GLB = Not free** (cross-zone disabled by default, costs money if enabled)

**Your Pattern:** You keep thinking ALB cross-zone costs money. It doesn't. This is a gimme question - don't miss it.

---

### Question 7: CORRECT ANSWER - B

**Explanation:**
Let's parse the access patterns:
- **Days 0-90**: Frequent access → **S3 Standard**
- **Days 91-365**: **"Almost never accessed"** + "5-hour retrieval acceptable" → **S3 Glacier Flexible Retrieval**
  - Key word: **"Almost never"** (NOT "rarely")
  - "Accessed maybe once per year" = true archive scenario
  - 5-hour retrieval = Glacier Flexible Standard retrieval (3-5 hours)
  - Cost: $0.0036/GB/month
- **Days 366-3650**: Never accessed → **S3 Glacier Deep Archive**
  - Cheapest: $0.00099/GB/month
  - 12-hour retrieval sufficient for "never accessed"

**Why others are wrong:**
- **A**: Day 90 → Standard-IA is more expensive ($0.0125/GB/month vs $0.0036/GB/month for Glacier Flexible) and unnecessary when 5-hour retrieval is acceptable
- **C**: Glacier Instant Retrieval ($0.004/GB/month) costs more than Glacier Flexible and provides instant retrieval that's not needed
- **D**: Intelligent-Tiering has monitoring fees ($0.0025 per 1000 objects) and is designed for unknown access patterns, not "almost never accessed"

**The Distinction:**
- **"Rarely accessed" + "immediate"** → **Standard-IA** (might need it quickly)
- **"Almost never accessed" + "hours OK"** → **Glacier Flexible** (true archive)

**Your Pattern:** You're getting better at this! "Almost never" + "5 hours acceptable" = Glacier Flexible.

---

### Question 8: CORRECT ANSWER - B

**Explanation:**
For **Apache Kafka** (distributed streaming system):

**Requirements:**
- 20 broker nodes across 3 AZs
- Protection against correlated hardware failures
- Support distributed system fault tolerance

**Partition Placement Group is correct:**
- **7 partitions per AZ** (AWS supports max 7 partitions per AZ)
- Each partition is on a **separate rack** (fault isolation)
- If one rack/partition fails, other partitions continue operating
- Kafka's built-in replication works across partitions
- **Designed for distributed systems** like Hadoop, Kafka, Cassandra

**Why others are wrong:**
- **A (Cluster)**: Cluster placement groups pack instances close together in **single AZ on same rack** for maximum network performance. This creates a single point of failure (entire rack fails → all nodes down). Not suitable for fault-tolerant distributed systems.
- **C (Spread)**: Spread placement groups limit to **max 7 instances per AZ**. With 20 broker nodes across 3 AZs, you'd need 7+7+6 distribution. Spread is for critical individual instances, not large distributed clusters. Too restrictive.
- **D (No placement group)**: Misses opportunity to control rack placement for fault isolation.

**Placement Group Decision Tree:**
```
Distributed system (Hadoop, Kafka, Cassandra)?
└─> Partition ✅

HPC with low latency requirement?
└─> Cluster ✅

Critical instance isolation (max 7 instances)?
└─> Spread ✅
```

**Your Pattern:** You were picking Cluster for distributed systems. Remember: Cluster = single rack (HPC), Partition = multiple racks (Kafka/Hadoop).

---

### Question 9: CORRECT ANSWER - B

**Explanation:**
**NACLs are STATELESS** - this is the core concept you must remember.

For outbound HTTPS requests from private subnet:

**Traffic Flow:**
1. **Outbound Request**: Instance → NAT Gateway → Internet (port 443)
   - NACL outbound rule needed: Allow TCP 443 to 0.0.0.0/0 ✅
2. **Return Response**: Internet → NAT Gateway → Instance (ephemeral port 32768-65535)
   - NACL inbound rule needed: Allow TCP 1024-65535 from 0.0.0.0/0 ✅

**Required NACL Rules:**
```
OUTBOUND:
- Allow TCP 443 to 0.0.0.0/0 (HTTPS requests)

INBOUND:
- Allow TCP 1024-65535 from 0.0.0.0/0 (return traffic on ephemeral ports)
```

**Why others are wrong:**
- **A**: NACLs are STATELESS (not stateful) - return traffic is NOT automatic
- **C**: Security Groups and NACLs are independent; NACLs must have both directions configured
- **D**: While technically true that Security Groups provide security, NACLs still need correct configuration to allow traffic flow

**Stateful vs Stateless:**
| Security Control | Behavior | Return Traffic |
|-----------------|----------|----------------|
| **Security Groups** | Stateful | Automatic ✅ |
| **NACLs** | Stateless | Manual (ephemeral ports) ⚠️ |

**Your Pattern:** You're confusing Security Groups (stateful) with NACLs (stateless). Drill this in.

---

### Question 10: CORRECT ANSWER - B

**Explanation:**
Requirements:
- FIPS 140-2 Level 3 validated hardware
- **Exclusive customer control** (AWS has no access)
- Encrypt S3, EBS, RDS

**CloudHSM is the only solution:**

**Architecture:**
1. **AWS CloudHSM cluster** in customer VPC (dedicated HSM hardware)
2. **S3**: Client-side encryption using keys from CloudHSM, upload encrypted objects
3. **EBS**: EBS encryption with CloudHSM custom key store integration
4. **RDS**: RDS encryption with CloudHSM custom key store

**Why CloudHSM:**
- **FIPS 140-2 Level 3** validated (KMS is Level 2/3)
- **Customer exclusive control**: Keys generated and stored in CloudHSM cluster
- **AWS cannot access**: AWS has no technical means to access keys in CloudHSM
- **Multi-service support**: Can integrate with S3, EBS, RDS via custom key stores

**Why others are wrong:**
- **A (KMS)**: KMS is FIPS 140-2 Level 2/3 (NOT pure Level 3). AWS has operational access to perform cryptographic operations on customer's behalf.
- **C (SSE-KMS)**: Same issue - KMS is not Level 3, and AWS performs encryption operations
- **D (Secrets Manager)**: Secrets Manager is for storing secrets (API keys, passwords), not for encryption key management. Uses KMS under the hood (not Level 3).

**KMS vs CloudHSM:**
| Feature | KMS | CloudHSM |
|---------|-----|----------|
| **FIPS 140-2 Level** | 2/3 | **3** ✅ |
| **AWS Access** | Operational access | **NO access** ✅ |
| **Use Case** | Most workloads, audit trail | Regulatory compliance, exclusive control |

**Your Pattern:** You default to KMS for encryption. But when question says "Level 3" or "AWS should NOT have access", only CloudHSM qualifies.

---

### Question 11: CORRECT ANSWER - B

**Explanation:**
Requirements:
- **UDP protocol** (not HTTP/HTTPS)
- **Static IP addresses** (for firewall whitelisting)
- **Extremely low latency** (<100ms)

**Network Load Balancer (NLB) is correct:**
- **Supports UDP** (also TCP, TLS) - Layer 4 load balancer
- **Static IP via Elastic IPs**: One Elastic IP per Availability Zone
- **Ultra-low latency**: <100 microseconds (0.1 milliseconds)
- **Millions of requests per second**

**Why others are wrong:**
- **A (ALB + Global Accelerator)**: ALB is **Layer 7** (HTTP/HTTPS only), does NOT support UDP. Even with Global Accelerator providing static IPs, ALB cannot handle UDP traffic. Eliminated immediately when you see "UDP".
- **C (GLB)**: Gateway Load Balancer is for **third-party security appliances** (firewalls, IDS/IPS), not general application load balancing. Wrong use case.
- **D (CLB)**: Classic Load Balancer is **legacy/deprecated** on current exam. CLB doesn't support Elastic IP assignment. Modern exams favor ALB/NLB/GLB.

**Load Balancer Decision Tree:**
```
Protocol: UDP?
├─> ALB? ❌ NO (HTTP/HTTPS only)
└─> NLB? ✅ YES (TCP/UDP/TLS)

Need static IP?
├─> ALB? ❌ NO (dynamic DNS only)
└─> NLB? ✅ YES (Elastic IP per AZ)
```

**Your Pattern:** You previously struggled with this (ALB static IP trap). This is a retention test - you should nail this.

---

### Question 12: CORRECT ANSWER - B

**Explanation:**
The problem: Instances are **terminated shortly after launch**

**Root Cause: Health Check Grace Period Too Short**

**Timeline:**
1. Instance launches at T=0
2. Application takes 90 seconds to start (T=0 to T=90)
3. ALB health check interval: 30 seconds
4. Unhealthy threshold: 2 consecutive failures
5. **Health check timeline:**
   - T=30s: First health check (app not ready) → FAIL
   - T=60s: Second health check (app not ready) → FAIL
   - T=60s: Instance marked unhealthy (2 consecutive failures)
   - ASG terminates instance before app finishes starting at T=90s

**Solution: Set health check grace period to 90s + 60s buffer = 150-180 seconds**

The grace period is how long ASG waits **before** starting health checks after instance launch.

**Recommended grace period formula:**
```
Grace Period = Application Startup Time + (Health Check Interval × Unhealthy Threshold)
Grace Period = 90s + (30s × 2) = 150 seconds minimum
Recommended: 180 seconds (3 minutes) for buffer
```

**Why others are wrong:**
- **A**: Health check interval (30s) is fine - the issue is grace period, not interval
- **C**: Increasing unhealthy threshold just delays the problem; doesn't fix root cause
- **D**: ASG is already using ELB health checks (that's how it knows instances are "unhealthy")

**Key Insight:** If instances are terminated shortly after launch, think: **health check grace period < application startup time**

---

### Question 13: CORRECT ANSWER - A

**Explanation:**
**S3 Standard-IA has a minimum storage duration of 30 days.**

**What happens when you delete before 30 days:**
- Object stored for 15 days, then deleted
- **You're charged for the full 30 days** (minimum storage duration)
- You pay for 15 days of unused storage

**Minimum Storage Durations:**
| Storage Class | Minimum Duration | Early Deletion Charge |
|--------------|------------------|---------------------|
| **S3 Standard** | None | No |
| **S3 Standard-IA** | **30 days** | Yes ✅ |
| **S3 One Zone-IA** | **30 days** | Yes |
| **S3 Glacier Flexible** | **90 days** | Yes |
| **S3 Glacier Deep Archive** | **180 days** | Yes |

**Solution:**
For data that might be deleted before 30 days, use **S3 Standard** (no minimum duration).

**Why others are wrong:**
- **B**: Retrieval fees apply to Glacier tiers, not Standard-IA. Also, deletion is not a retrieval operation.
- **C**: True, but not the cause of "higher than expected" cost in this scenario. Minimum duration is the main issue.
- **D**: S3 doesn't charge data transfer for deletion operations within the same region.

**Exam Tip:** Always check minimum storage duration when lifecycle policies involve early transitions or deletions.

---

### Question 14: CORRECT ANSWER - B

**Explanation:**
Both **S3 and DynamoDB** support **VPC Gateway Endpoints**.

**VPC Gateway Endpoints:**
- **FREE** (no hourly charge, no data processing charge)
- Configured via route table entries
- High bandwidth, no throttling
- Traffic never leaves AWS network (no NAT Gateway needed)

**Configuration:**
1. Create VPC Gateway Endpoint for S3
2. Create VPC Gateway Endpoint for DynamoDB
3. Update private subnet route table:
   - S3 prefix list → S3 Gateway Endpoint
   - DynamoDB prefix list → DynamoDB Gateway Endpoint
4. Remove NAT Gateway (save $0.045/hour + $0.045/GB)

**Cost Savings:**
- **NAT Gateway costs**: $0.045/hour + $0.045/GB data processed
- **Gateway Endpoint costs**: $0 (FREE)
- For high S3/DynamoDB traffic, this saves hundreds to thousands per month

**Why others are wrong:**
- **A (Interface Endpoints)**: Interface Endpoints cost **$0.01/hour per AZ** + **$0.01/GB** data processed. Since S3 and DynamoDB support Gateway Endpoints (which are free), use those instead. Interface Endpoints are for services that DON'T support Gateway Endpoints (EC2, SNS, SQS, etc.).
- **C (AWS PrivateLink)**: PrivateLink is for accessing services in OTHER VPCs or third-party SaaS. Not needed for AWS services. PrivateLink uses Interface Endpoints (costs money).
- **D**: You cannot create a single endpoint for both - you need separate Gateway Endpoints for S3 and DynamoDB.

**VPC Endpoint Decision:**
| Service | Endpoint Type | Cost |
|---------|--------------|------|
| **S3** | Gateway | **FREE** ✅ |
| **DynamoDB** | Gateway | **FREE** ✅ |
| **EC2, SNS, SQS, etc.** | Interface | **$0.01/hr/AZ + $0.01/GB** |

**Exam Keyword:** "MOST cost-effective" + "S3 and/or DynamoDB" = **Gateway Endpoints**

---

### Question 15: CORRECT ANSWER - B

**Explanation:**
Requirements:
- Large datasets (50+ TB)
- **Sequential read/write patterns** (data warehousing)
- High throughput needed
- Does **NOT** require ultra-low latency random I/O
- Cost-effective

**d2 instances are designed for this:**
- **HDD-based local storage** (dense HDD storage)
- **High sequential throughput** (optimized for large sequential I/O)
- **Large capacity**: Up to 48 TB of HDD storage per instance
- **Cost-effective**: HDDs are cheaper than SSDs for large capacity
- **Use case**: Data warehouses, Hadoop, MapReduce, distributed file systems

**Why others are wrong:**
- **A (i3 instances)**: i3 uses **NVMe SSD** local storage for ultra-high IOPS and low-latency random I/O (perfect for NoSQL databases, caching). SSDs are more expensive than HDDs for large capacity. Overkill for sequential data warehousing.
- **C (m5 + gp3)**: General-purpose instances with EBS gp3 are flexible but not as cost-effective as d2 with local HDD storage for large sequential workloads. Also, EBS costs more than local instance store for 50+ TB.
- **D (r6i)**: Memory-optimized instances are for in-memory workloads (SAP HANA, Redis), not large-capacity sequential storage.

**Storage Optimized Instance Comparison:**
| Instance | Storage Type | IOPS | Use Case |
|----------|--------------|------|----------|
| **i3/i3en** | NVMe SSD | Very High | NoSQL databases, caching, real-time analytics |
| **d2** | **HDD** | Moderate | **Data warehouses, Hadoop, MapReduce** ✅ |
| **h1** | HDD | Moderate | MapReduce, HDFS |

**Memory Trick:**
- **i3 = I/O intensive** (SSDs for databases)
- **d2 = Data warehouses** (HDDs for large sequential data)

**Your Pattern:** You've been picking i3 when you see "storage optimized". Check if it's SSD (i3) or HDD (d2) workload.

---

### Question 16: CORRECT ANSWER - B

**Explanation:**
Requirements:
- **Audit trail via CloudTrail** ✅ This is the key
- Company manages keys (not AWS default)
- Ability to disable keys
- Encryption at rest

**SSE-KMS with customer-managed CMKs is correct:**

**Why SSE-KMS:**
- **CloudTrail logs all KMS API calls**: Encrypt, Decrypt, GenerateDataKey, etc.
- **Customer-managed CMK**: Company creates and manages the Customer Master Key
- **Key control**: Can disable, schedule deletion, rotate keys
- **Encryption at rest**: Data encrypted in S3
- **Audit trail**: Every encryption/decryption operation logged in CloudTrail

**Example CloudTrail log:**
```json
{
  "eventName": "Decrypt",
  "userIdentity": {"arn": "arn:aws:iam::123456789012:role/app-role"},
  "requestParameters": {"keyId": "arn:aws:kms:us-east-1:123456789012:key/abc123"},
  "responseElements": null,
  "resources": [{"ARN": "arn:aws:kms:us-east-1:123456789012:key/abc123"}]
}
```

**Why others are wrong:**
- **A (SSE-S3)**: Uses AWS-managed keys (not company-managed). No audit trail of encryption operations.
- **C (SSE-C)**: Customer provides keys with each request. No CloudTrail audit trail of encryption operations (S3 doesn't log SSE-C key usage).
- **D (Client-side + Secrets Manager)**: Encryption happens client-side (before upload), so S3/CloudTrail doesn't see encryption operations. Secrets Manager is for secrets storage, not encryption auditing.

**SSE-KMS vs Client-side Encryption:**
| Requirement | SSE-KMS | Client-side + CloudHSM |
|-------------|---------|----------------------|
| **Audit trail via CloudTrail** | ✅ YES | ❌ NO (uses CloudHSM logs) |
| **AWS should NOT have access to keys** | ❌ NO (AWS performs ops) | ✅ YES |
| **FIPS 140-2 Level 3** | ❌ NO (Level 2/3) | ✅ YES |

**Your Pattern:** This is the OPPOSITE scenario from Question 4:
- Question 4: "AWS should NOT have access" → Client-side + CloudHSM
- Question 16: "Audit trail via CloudTrail" → SSE-KMS

**Key Distinction:** CloudTrail logs KMS operations (SSE-KMS), but CloudHSM has its own separate logging (not in CloudTrail).

---

### Question 17: CORRECT ANSWER - C

**Explanation:**
Traffic patterns analysis:
- **Predictable morning spike** (8-9 AM)
- Steady baseline during day
- **Unpredictable marketing campaigns**
- **Predictable evening drop** (8-9 PM)

**Solution: COMBINE Scheduled Actions + Target Tracking**

**Scheduled Actions** (for predictable patterns):
- **7:30 AM**: Scale up to morning baseline (30 min before 8 AM rush)
  - Instances have time to launch (2-5 min) and warm up before traffic
- **8:30 PM**: Scale down to evening baseline (after 8 PM drop)
  - Save costs during low-traffic evening/night hours
- **Proactive scaling**: Capacity ready BEFORE traffic arrives

**Target Tracking** (for unpredictable patterns):
- **Target 70% CPU utilization**
- Handles unpredictable marketing campaigns
- Scales up beyond scheduled capacity when needed
- Scales down when campaign ends
- **Reactive scaling**: Responds to actual load

**Why this works:**
- When both policies are active, ASG chooses the one that provides **more capacity**
- If scheduled action sets 10 instances, but marketing campaign drives CPU to 80% and Target Tracking wants 15 instances → ASG scales to 15
- Best of both worlds: proactive for known patterns, reactive for unknown spikes

**Why others are wrong:**
- **A**: Target Tracking alone is purely reactive. During 8 AM spike, it waits for CPU to hit threshold, then launches instances (2-5 min lag). Users experience degraded performance while waiting. Misses opportunity to proactively prepare for known 8 AM pattern.
- **B**: Scheduled Actions alone don't handle unpredictable marketing campaigns. If campaign drives traffic beyond scheduled capacity, no auto-scaling occurs (degraded performance or failures).
- **D**: Step Scaling requires more operational overhead (define thresholds at 70%, 80%, 90% and scaling amounts for each). Less optimal than Target Tracking for unpredictable load. Doesn't proactively scale for predictable patterns.

**Your Pattern:** This is your weakness - you keep choosing single policy when question has BOTH predictable and unpredictable patterns.

**Rule:** Predictable + Unpredictable in same question = **COMBINE Scheduled + Target Tracking**

---

### Question 18: CORRECT ANSWER - B

**Explanation:**
**This is a reverse scenario** from Question 2:

**Traffic flow:**
1. **Inbound HTTPS** (Internet → Application): Works fine ✅
   - Internet → Instance (port 443)
   - NACL inbound rule allows TCP 443 from 0.0.0.0/0 ✅
2. **Outbound API calls** (Application → Internet): Times out ❌
   - Outbound request: Instance → Internet (port 443)
   - NACL outbound rule allows TCP 443 to 0.0.0.0/0 ✅
   - Return response: Internet → Instance (ephemeral port 32768-65535)
   - **NACL inbound rule missing for ephemeral ports** ❌

**The problem:**
When the application makes outbound API calls, the external API responds on **ephemeral ports**. The NACL only allows inbound port 443, not ephemeral ports.

**Required NACL rules:**
```
INBOUND:
- Rule 100: Allow TCP 443 from 0.0.0.0/0 (for receiving HTTPS requests)
- Rule 110: Allow TCP 1024-65535 from 0.0.0.0/0 (for return traffic from outbound API calls) ⚠️

OUTBOUND:
- Rule 100: Allow TCP 443 to 0.0.0.0/0 (for making API calls)
- Rule 110: Allow TCP 1024-65535 to 0.0.0.0/0 (for sending responses to inbound requests)
```

**Why others are wrong:**
- **A**: Outbound ephemeral ports are for sending RESPONSES to inbound HTTPS requests (that works fine). The issue is RETURN traffic from outbound API calls needs INBOUND ephemeral ports.
- **C**: Security Group allowing all outbound is correct (Security Groups are stateful, no issue here)
- **D**: Public subnet uses Internet Gateway (not NAT Gateway) - route table is fine

**Key Insight:**
- NACL inbound 443 → Allows receiving HTTPS requests from clients
- NACL inbound 1024-65535 → Allows receiving return traffic from outbound API calls ⚠️ (this is what's missing)

**Your Pattern:** You're getting better at NACL questions. This is a tricky reverse scenario.

---

### Question 19: CORRECT ANSWER - B

**Explanation:**
For **Network Load Balancer (NLB)**:
- Cross-zone load balancing is **disabled by default**
- When enabled, **incurs data transfer charges** ($0.01/GB for traffic between AZs)

**This is DIFFERENT from ALB:**

| Load Balancer | Cross-Zone Default | Cost When Enabled |
|--------------|-------------------|------------------|
| **ALB** | ✅ Enabled | **FREE** ✅ |
| **NLB** | ❌ Disabled | **$0.01/GB** 💰 |
| **GLB** | ❌ Disabled | **$0.01/GB** 💰 |

**Why it costs money for NLB:**
- Cross-zone traffic flows between AZs
- Data transfer between AZs incurs charges
- ALB is an exception (AWS doesn't charge for ALB cross-zone)

**Why others are wrong:**
- **A**: NLB cross-zone is NOT free (unlike ALB). This is a trap answer for people who confuse ALB and NLB.
- **C**: NLB cross-zone is **disabled by default** (you must manually enable it). ALB cross-zone is enabled by default.
- **D**: The hourly charge ($0.05/hour) is the base NLB charge (not related to cross-zone). Cross-zone adds $0.01/GB data transfer charge.

**Memory Trick:**
- **ALB = Always free** (cross-zone enabled by default, FREE)
- **NLB = Not free** (cross-zone disabled by default, costs $0.01/GB if enabled)

**Exam Trap:** Questions about "cross-zone load balancing costs" are testing if you know ALB is free but NLB/GLB cost money.

---

### Question 20: CORRECT ANSWER - B

**Explanation:**
When generating pre-signed URLs for **SSE-KMS encrypted objects**:

**Required permissions for the IAM principal (user/role) generating the URL:**
1. **s3:GetObject** on the S3 bucket/object
2. **kms:Decrypt** on the Customer Master Key (CMK)

**Why both permissions:**
- Pre-signed URL grants temporary access using the **credentials of the principal who generated the URL**
- When recipient uses the URL to download the object:
  1. S3 retrieves the encrypted object (needs s3:GetObject permission)
  2. S3 decrypts the object using KMS (needs kms:Decrypt permission)
  3. S3 returns decrypted object to recipient
- **All operations happen using the URL generator's permissions**

**Why others are wrong:**
- **A**: s3:GetObject alone is insufficient for SSE-KMS objects. KMS decryption permission is required.
- **C**: s3:PutObject is for uploading (not needed for generating download URLs)
- **D**: Pre-signed URLs work by temporarily delegating the URL generator's permissions. If the generator doesn't have permissions, the URL won't work.

**For different encryption types:**
| Encryption Type | Permissions Required |
|----------------|---------------------|
| **No encryption** | s3:GetObject |
| **SSE-S3** | s3:GetObject |
| **SSE-KMS** | s3:GetObject + kms:Decrypt ✅ |
| **SSE-C** | s3:GetObject + customer provides key with URL |

**Exam Tip:** Pre-signed URLs for SSE-KMS objects require both S3 and KMS permissions.

---

</details>

---

## Scoring & Next Steps

**Your Score: _____ / 20**

### Performance Evaluation

- **18-20 correct (90-100%)**: 🎉 Outstanding! You've crushed your weak areas. Ready for Week 2.
- **16-17 correct (80-85%)**: ✅ Good! Review missed questions, then move forward.
- **14-15 correct (70-75%)**: ⚠️ Still on the edge. Re-read Day-7-Updated-Weaknesses.md tonight.
- **12-13 correct (60-65%)**: 🚨 Not ready. Re-study weak areas for 2 more hours before Week 2.
- **<12 correct (<60%)**: 🛑 STOP. You're not retaining the material. Spend tomorrow reviewing Days 1-6.

### What to Do Based on Your Score

**If you scored 16+ (80%+):**
1. ✅ Review the questions you missed
2. ✅ Update your cheat sheet with any new patterns
3. ✅ Start Week 2 (RDS & Aurora) tomorrow
4. ✅ You're on track for 85%+ on the actual exam

**If you scored 14-15 (70-75%):**
1. ⚠️ Re-read Day-7-Updated-Weaknesses.md tonight (30 min)
2. ⚠️ Retake this quiz tomorrow morning
3. ⚠️ Target: 18+ on retake
4. ⚠️ Don't start Week 2 until you hit 80%

**If you scored below 14 (<70%):**
1. 🚨 You're not ready for new material
2. 🚨 Spend tomorrow reviewing Days 1-6 materials
3. 🚨 Focus on the questions you got wrong
4. 🚨 Retake this quiz in 48 hours
5. 🚨 Target: 16+ before moving to Week 2

---

## Top Mistakes to Watch For

As you grade yourself, check if you made these common errors:

### Mistake Pattern 1: "Rarely Accessed" → Glacier
- ❌ Jumping to Glacier when you see "rarely accessed"
- ✅ Check retrieval time requirement FIRST

### Mistake Pattern 2: Forgetting NACL Ephemeral Ports
- ❌ Assuming NACLs are stateful like Security Groups
- ✅ Remember: NACLs are stateless, must allow 1024-65535 for return traffic

### Mistake Pattern 3: SSE-KMS for "AWS Should NOT Have Access"
- ❌ Picking SSE-KMS when question says "AWS should NOT have access to keys"
- ✅ Only client-side encryption + CloudHSM ensures AWS has NO access

### Mistake Pattern 4: Single Policy When Question Has Both Patterns
- ❌ Defaulting to Target Tracking only
- ✅ COMBINE Scheduled + Target Tracking when question has predictable AND unpredictable patterns

### Mistake Pattern 5: Access Points for Temporary Access
- ❌ Choosing S3 Access Points for temporary time-limited sharing
- ✅ Pre-signed URLs for temporary access, Access Points for long-term multi-tenant

---

## Weekly Target Progress

You should be hitting these scores on each week's assessment:

- **Day 7 (Week 1 Review)**: 80%+ (60/75 questions) ← You need 16/20 on this quiz to be on track
- **Day 14 (Week 2 Review)**: 85%+ (85/100 questions)
- **Day 21 (Official Practice)**: 80%+ (52/65 questions)
- **Actual Exam (Dec 17)**: 85%+ (target 850/1000 score)

---

**Now take the quiz. No peeking at answers until you're done with all 20 questions.**

**You've got this. Time to prove you've learned from your mistakes.** 💪
