# Day 6 Catch-Up Quiz: Days 1-5 Comprehensive Review

**Date: November 26, 2025**
**Topics Covered:** EC2, Auto Scaling, Load Balancing, S3 Storage Classes, S3 Security, VPC Fundamentals
**Questions: 20**
**Passing Score: 15/20 (75%)**

---

## Instructions
- Answer each question before looking at the explanation
- Mark your answer (A, B, C, or D)
- Check the detailed explanation after answering
- Track topics that need more review

---

## Question 1 (EC2 - Instance Types)
**Difficulty: Easy**

A company is running a high-performance computing (HPC) application that requires extremely low latency and high network throughput between instances for molecular simulations. Which combination provides the BEST performance?

A) Memory-optimized instances (r6i) in a Partition placement group
B) Compute-optimized instances (c7g) in a Spread placement group
C) Compute-optimized instances (c6i) in a Cluster placement group
D) General-purpose instances (m6i) in a Partition placement group

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Explanation:**
- **Cluster placement groups** pack instances close together in a single AZ for maximum network performance (up to 25 Gbps between instances). This is ideal for HPC workloads requiring low latency.
- **Compute-optimized instances (c6i)** provide the best price/performance for compute-intensive workloads like molecular simulations.

**Why other answers are wrong:**
- **A**: Partition placement groups spread instances across logical partitions (racks), reducing correlated failures but NOT optimizing for low latency
- **B**: Spread placement groups place instances on distinct hardware, minimizing correlated failures but providing the WORST inter-instance latency
- **D**: General-purpose instances lack the compute optimization needed for HPC, and Partition groups don't optimize for latency

**Key Exam Tip:** Cluster placement group = lowest latency + highest throughput | Spread = highest availability | Partition = balanced (big data workloads)

</details>

---

## Question 2 (EC2 - Pricing)
**Difficulty: Medium**

A startup is launching a web application with unpredictable traffic patterns for the first 6 months, after which they expect steady 24/7 usage. The application can tolerate interruptions during low-priority batch processing jobs. What is the MOST cost-effective strategy?

A) Use On-Demand instances for 6 months, then switch to 1-year Reserved Instances
B) Use On-Demand instances for the web tier, and Spot Instances for batch processing throughout
C) Use Savings Plans immediately with a 3-year commitment
D) Use Reserved Instances from day one with Convertible options

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **On-Demand for web tier** provides flexibility during the unpredictable first 6 months without commitment
- **Spot Instances for batch processing** can save up to 90% compared to On-Demand, and the workload explicitly tolerates interruptions
- This hybrid approach optimizes cost throughout the entire period

**Why other answers are wrong:**
- **A**: Doesn't leverage Spot for batch processing (missing huge savings), and 6 months is too early to predict capacity needs
- **C**: 3-year commitment is risky when traffic patterns are unpredictable (you don't know what instance types/sizes you'll need)
- **D**: Reserved Instances from day one wastes commitment on unpredictable usage; Convertible RIs have less discount than Standard

**Key Exam Tip:** Spot = interrupt-tolerant workloads (batch, data analysis) | On-Demand = unpredictable, short-term | Reserved/Savings Plans = steady, predictable usage (1+ year)

</details>

---

## Question 3 (Auto Scaling)
**Difficulty: Medium**

An e-commerce application experiences predictable traffic spikes every day at 9 AM and 6 PM, and also needs to handle unpredictable promotional campaigns. Which Auto Scaling configuration is MOST appropriate?

A) Target Tracking scaling policy based on CPU utilization
B) Scheduled scaling actions at 9 AM and 6 PM only
C) Scheduled scaling actions at 9 AM and 6 PM, PLUS Target Tracking scaling policy
D) Step scaling policy with CloudWatch alarms

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Explanation:**
- **Scheduled scaling** handles the predictable daily spikes (9 AM, 6 PM) by proactively adding capacity BEFORE traffic arrives
- **Target Tracking** handles unpredictable promotional campaigns by reacting to actual load (e.g., CPU utilization, ALB request count)
- You can combine multiple scaling policies on a single Auto Scaling group

**Why other answers are wrong:**
- **A**: Reactive-only approach; instances won't be ready when 9 AM spike hits (cold start problem)
- **B**: Doesn't handle unpredictable promotional campaigns
- **D**: Step scaling requires more operational overhead than Target Tracking (you must define thresholds and scaling amounts manually)

**Key Exam Tip:** Scheduled = predictable time-based patterns | Target Tracking = least overhead, reactive | You CAN combine multiple scaling policy types

</details>

---

## Question 4 (Load Balancing)
**Difficulty: Hard**

A financial services company needs to deploy a real-time trading application with the following requirements:
- Handle millions of TCP connections
- Source IP addresses must be preserved for compliance logging
- Must support static IP addresses for firewall whitelisting by partners
- Requires ultra-low latency (sub-millisecond)

Which load balancer configuration meets ALL requirements?

A) Network Load Balancer with Proxy Protocol v2 enabled
B) Network Load Balancer with preserve source IP enabled (default)
C) Application Load Balancer with X-Forwarded-For headers
D) Gateway Load Balancer with IP preservation

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **Network Load Balancer (NLB)** provides static IP addresses (via Elastic IPs), ultra-low latency, and handles millions of connections
- **NLB preserves source IP by default** when using TCP target type, meeting the compliance requirement
- NLB operates at Layer 4 (TCP/UDP) with minimal processing overhead (sub-millisecond latency)

**Why other answers are wrong:**
- **A**: Proxy Protocol v2 is only needed when source IP isn't preserved by default (e.g., with TLS termination), but it adds complexity
- **C**: ALB operates at Layer 7 (HTTP/HTTPS) with higher latency; X-Forwarded-For is an HTTP header (not suitable for raw TCP); no static IPs
- **D**: Gateway Load Balancer is for third-party virtual appliances (firewalls, IDS/IPS), not application load balancing

**Key Exam Tip:** NLB = TCP/UDP + static IPs + extreme performance + preserves source IP | ALB = HTTP/HTTPS + path routing | GLB = third-party appliances ONLY

</details>

---

## Question 5 (Load Balancing)
**Difficulty: Easy**

An application requires routing HTTP requests to different target groups based on the URL path. Requests to `/api/*` should go to API servers, and requests to `/images/*` should go to image processing servers. Which load balancer should be used?

A) Network Load Balancer
B) Application Load Balancer
C) Classic Load Balancer
D) Gateway Load Balancer

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **Application Load Balancer (ALB)** operates at Layer 7 (HTTP/HTTPS) and supports path-based routing rules
- You can configure listener rules to route `/api/*` to one target group and `/images/*` to another target group

**Why other answers are wrong:**
- **A**: NLB operates at Layer 4 (TCP/UDP) and cannot inspect HTTP paths
- **C**: Classic Load Balancer is deprecated and lacks advanced routing features
- **D**: Gateway Load Balancer is for third-party virtual appliances, not HTTP routing

**Key Exam Tip:** Path-based routing or Host-based routing = ALB | Content-based routing requires Layer 7 inspection (ALB only)

</details>

---

## Question 6 (S3 - Storage Classes)
**Difficulty: Medium**

A company stores financial audit logs that must be retained for 7 years for compliance. The logs are accessed once per quarter for audits and must be retrievable within 12 hours. Which S3 storage class provides the MOST cost-effective solution?

A) S3 Standard-IA (Infrequent Access)
B) S3 One Zone-IA
C) S3 Glacier Flexible Retrieval
D) S3 Glacier Deep Archive

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: D**

**Explanation:**
- **S3 Glacier Deep Archive** is the cheapest storage class (~$1/TB/month), perfect for rarely accessed archive data
- **Standard retrieval** time is 12 hours, which exactly meets the 12-hour requirement
- Accessed only once per quarter (4 times per year) = minimal retrieval costs

**Why other answers are wrong:**
- **A**: Standard-IA costs ~$0.0125/GB/month vs Deep Archive's ~$0.00099/GB/month (12x more expensive)
- **B**: One Zone-IA is cheaper than Standard-IA but still 8x more expensive than Deep Archive; doesn't meet cost-effectiveness requirement
- **C**: Glacier Flexible Retrieval is cheaper than IA but more expensive than Deep Archive (~$0.0036/GB/month); you're paying for faster retrieval you don't need

**Key Exam Tip:**
- Deep Archive = lowest cost, 12-48 hour retrieval, minimum 180-day storage
- Glacier Flexible = 3-5 hour retrieval (Standard), minimum 90-day storage
- "Within hours" (12+ hours acceptable) = Deep Archive | "Within minutes/hours" (3-5 hours) = Glacier Flexible

</details>

---

## Question 7 (S3 - Lifecycle Policies)
**Difficulty: Medium**

A media company uploads 4K video files to S3 daily. Videos are frequently accessed during the first 30 days, rarely accessed from day 31-90, and almost never accessed after 90 days but must be kept for 1 year. After 1 year, videos should be permanently deleted. What lifecycle policy is MOST cost-effective?

A) Day 30 → Glacier Flexible, Day 90 → Glacier Deep Archive, Day 365 → Delete
B) Day 30 → Standard-IA, Day 90 → Glacier Deep Archive, Day 365 → Delete
C) Day 30 → Standard-IA, Day 90 → Glacier Flexible, Day 365 → Delete
D) Day 30 → One Zone-IA, Day 90 → Glacier Deep Archive, Day 365 → Delete

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **First 30 days**: Keep in S3 Standard (frequent access, no transition needed)
- **Day 30 → Standard-IA**: Rarely accessed, but might need occasional quick access (millisecond retrieval)
- **Day 90 → Glacier Deep Archive**: Almost never accessed, lowest cost, 12-hour retrieval acceptable
- **Day 365 → Delete**: Automatic cleanup

**Why other answers are wrong:**
- **A**: Transitioning to Glacier Flexible at day 30 is premature; Standard-IA provides same quick access as Standard for rare access patterns
- **C**: Glacier Flexible at day 90 is more expensive than Deep Archive, and you don't need 3-5 hour retrieval for "almost never accessed" data
- **D**: One Zone-IA lacks multi-AZ resilience; for video archives that must be kept for compliance, durability is important

**Key Exam Tip:**
- 0-30 days frequent access = Standard
- 30+ days infrequent access = Standard-IA (if need quick access) or Glacier (if can wait)
- 90+ days archive = Glacier Deep Archive for max savings
- Minimum storage durations: Standard-IA (30 days), Glacier Flexible (90 days), Deep Archive (180 days)

</details>

---

## Question 8 (S3 - Security)
**Difficulty: Hard**

A healthcare company must encrypt patient data in S3 with the following requirements:
- Encryption keys must be managed by the company (not AWS)
- AWS should NOT have access to the encryption keys
- Need automatic key rotation every 90 days
- Keys must be stored in a FIPS 140-2 Level 3 validated hardware security module

Which solution meets ALL requirements?

A) SSE-KMS with customer managed CMK and automatic key rotation
B) SSE-C (Customer-Provided Keys) with client-side key rotation
C) SSE-S3 with AWS-managed keys
D) Client-side encryption with AWS CloudHSM for key storage

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: D**

**Explanation:**
- **Client-side encryption** means the company encrypts data BEFORE uploading to S3, so AWS never sees unencrypted data
- **AWS CloudHSM** provides FIPS 140-2 Level 3 validated HSMs (KMS is Level 2/3 hybrid)
- Company fully manages keys within CloudHSM; AWS has NO access to key material
- Implement custom key rotation logic (CloudHSM doesn't auto-rotate, but allows programmatic rotation)

**Why other answers are wrong:**
- **A**: SSE-KMS means AWS manages the encryption/decryption process using KMS keys; even with customer managed CMKs, AWS KMS performs the operations; KMS is FIPS 140-2 Level 2 (not Level 3)
- **B**: SSE-C requires providing the encryption key with EVERY request; keys are not stored in AWS at all (no HSM storage); lacks automatic rotation; operationally complex
- **C**: SSE-S3 uses AWS-managed keys; AWS has full access to keys (violates "AWS should NOT have access" requirement)

**Key Exam Tip:**
- Full control over keys + AWS has NO access = Client-side encryption
- CloudHSM = FIPS 140-2 Level 3 | KMS = Level 2/3 hybrid
- SSE-KMS/SSE-S3/SSE-C = AWS handles encryption (AWS has some level of access)

</details>

---

## Question 9 (S3 - Replication)
**Difficulty: Medium**

A company needs to replicate S3 objects from us-east-1 to eu-west-1 for disaster recovery. The following requirements must be met:
- Existing objects must be replicated (not just new objects)
- Replicate delete markers for cleanup operations
- Objects in the source bucket use SSE-KMS encryption

What configuration is required?

A) Enable Cross-Region Replication (CRR) with default settings
B) Enable CRR with Replica Modification Sync, Existing Object Replication, and grant KMS permissions
C) Enable CRR with Delete Marker Replication, Existing Object Replication, and grant KMS permissions
D) Enable Same-Region Replication (SRR) with versioning

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Explanation:**
- **Cross-Region Replication (CRR)** is needed for replicating across regions (us-east-1 → eu-west-1)
- **Delete Marker Replication** must be explicitly enabled to replicate delete markers
- **Existing Object Replication** must be explicitly enabled (by default, only new objects are replicated)
- **KMS permissions** must be granted in the replication configuration to decrypt source objects and re-encrypt destination objects

**Why other answers are wrong:**
- **A**: Default CRR settings only replicate NEW objects, don't replicate delete markers, and don't configure KMS permissions
- **B**: "Replica Modification Sync" replicates metadata changes (object metadata, ACLs, tags), not delete markers
- **D**: Same-Region Replication (SRR) is for replication within the same region (not us-east-1 → eu-west-1)

**Key Exam Tip:**
- CRR = Cross-Region | SRR = Same-Region
- By default, CRR/SRR only replicates NEW objects (must enable "Existing Object Replication" for existing objects)
- Delete markers, lifecycle actions, and SSE-KMS require explicit configuration

</details>

---

## Question 10 (S3 - Security)
**Difficulty: Easy**

A company wants to prevent any accidental deletion of critical S3 objects for 5 years. Even the root account should not be able to delete or modify these objects during this period. Which S3 feature should be used?

A) S3 Versioning with MFA Delete enabled
B) S3 Object Lock in Compliance mode with 5-year retention
C) S3 Object Lock in Governance mode with 5-year retention
D) S3 Bucket Policy with explicit Deny on DeleteObject

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **S3 Object Lock in Compliance mode** prevents object deletion or modification for the retention period
- **Compliance mode** means NO user (not even root account) can override retention, delete objects, or shorten the retention period
- This is the ONLY feature that meets "even the root account should not be able to delete"

**Why other answers are wrong:**
- **A**: MFA Delete requires MFA for deleting object versions, but root account with MFA can still delete (doesn't meet "even root account" requirement)
- **C**: Governance mode allows users with special permissions (s3:BypassGovernanceRetention) to delete objects; doesn't prevent root account deletion
- **D**: Bucket policies can be modified by accounts with sufficient permissions (including root); doesn't guarantee immutability

**Key Exam Tip:**
- Object Lock Compliance mode = IMMUTABLE (even root can't delete)
- Object Lock Governance mode = Can be overridden with special permission
- MFA Delete = Requires MFA for deletion, but doesn't prevent deletion

</details>

---

## Question 11 (VPC - Fundamentals)
**Difficulty: Easy**

A company is building a VPC with public and private subnets. Web servers in the public subnet must be accessible from the internet, and database servers in the private subnet must be able to download security patches from the internet but should NOT be directly accessible from the internet. What components are required?

A) Internet Gateway for public subnet, NAT Gateway in public subnet for private subnet
B) Internet Gateway for public subnet, NAT Instance in private subnet
C) NAT Gateway in public subnet for both public and private subnets
D) Internet Gateway in both public and private subnets

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Explanation:**
- **Internet Gateway (IGW)** provides internet access for public subnet (attached to VPC, routes from public subnet to 0.0.0.0/0 point to IGW)
- **NAT Gateway** must be deployed in the PUBLIC subnet (needs IGW for outbound internet access)
- Private subnet route table points to NAT Gateway for outbound internet traffic (0.0.0.0/0 → NAT Gateway)
- NAT Gateway allows private subnet resources to initiate outbound connections but prevents inbound connections from the internet

**Why other answers are wrong:**
- **B**: NAT Instance can work but requires manual management (patching, scaling, HA); NAT Gateway is managed service (less operational overhead). Also, NAT Instance must be in public subnet (not private).
- **C**: Public subnet still needs route to IGW (not NAT Gateway) for internet access
- **D**: You can only attach ONE Internet Gateway per VPC; IGW is VPC-level (not subnet-level); private subnet should NOT route to IGW directly

**Key Exam Tip:**
- Public subnet = IGW in route table (0.0.0.0/0 → IGW)
- Private subnet = NAT Gateway in route table (0.0.0.0/0 → NAT Gateway)
- NAT Gateway itself must live in public subnet

</details>

---

## Question 12 (VPC - Security)
**Difficulty: Medium**

An application has a three-tier architecture: web servers in public subnet, application servers in private subnet, and database servers in private subnet. Which security configuration follows AWS best practices?

A) Web SG: Allow HTTP/HTTPS from 0.0.0.0/0 | App SG: Allow all traffic from Web SG | DB SG: Allow MySQL from App SG
B) Public NACL: Allow HTTP/HTTPS inbound | Private NACL: Deny all inbound | Use Security Groups only
C) Web SG: Allow HTTP/HTTPS from 0.0.0.0/0 | App SG: Allow required ports from Web SG | DB SG: Allow required ports from App SG
D) Single Security Group: Allow HTTP/HTTPS from 0.0.0.0/0, allow MySQL from 10.0.0.0/16

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Explanation:**
- **Web tier**: Allow HTTP (80) and HTTPS (443) from the internet (0.0.0.0/0)
- **App tier**: Allow only REQUIRED ports from Web tier security group (reference Web SG as source, not CIDR blocks)
- **DB tier**: Allow only REQUIRED database ports (e.g., MySQL 3306) from App tier security group
- This implements **principle of least privilege** - each tier only allows traffic it needs from the previous tier

**Why other answers are wrong:**
- **A**: "Allow all traffic from Web SG" to App tier is overly permissive (violates least privilege); should only allow required ports
- **B**: NACLs are stateless and more complex to manage; Security Groups are stateful and preferred for instance-level security; "Deny all inbound" on private NACL would break return traffic
- **D**: Single security group violates defense-in-depth; allowing MySQL from entire VPC CIDR (10.0.0.0/16) is overly permissive

**Key Exam Tip:**
- Security Groups = stateful, allow rules only (default deny), reference other SGs
- NACLs = stateless (must allow return traffic), allow AND deny rules, subnet-level
- Best practice: Use Security Groups for instance security, reference other SGs (not CIDR blocks)

</details>

---

## Question 13 (VPC - Security Groups vs NACLs)
**Difficulty: Hard**

An application in a private subnet is experiencing connection timeouts when making outbound HTTPS requests to an external API. The Security Group allows all outbound traffic, and the NACL allows all traffic. What is the MOST likely cause?

A) Security Group is blocking return traffic on ephemeral ports
B) NACL is blocking return traffic on ephemeral ports
C) Internet Gateway is not attached to the VPC
D) Private subnet route table doesn't have a route to NAT Gateway

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **NACLs are stateless**, meaning return traffic must be explicitly allowed
- HTTPS requests to external API use port 443 outbound, but the API responds on **ephemeral ports** (typically 1024-65535)
- If NACL inbound rules don't allow ephemeral ports, return traffic is blocked (causing connection timeout)
- Security Groups are stateful, so if outbound is allowed, return traffic is automatically allowed

**Why other answers are wrong:**
- **A**: Security Groups are STATEFUL; if outbound is allowed, return traffic is automatically allowed (doesn't need explicit inbound rule)
- **C**: If IGW wasn't attached, there would be no outbound connectivity at all (not just timeouts); also, private subnet uses NAT Gateway (not IGW directly)
- **D**: If NAT Gateway route was missing, outbound requests wouldn't even leave the subnet (different error than "connection timeout")

**Key Exam Tip:**
- Security Group = STATEFUL (return traffic automatically allowed)
- NACL = STATELESS (must explicitly allow return traffic on ephemeral ports 1024-65535)
- Connection timeout with "Security Group allows all outbound" = check NACL inbound for ephemeral ports

</details>

---

## Question 14 (VPC - CIDR Design)
**Difficulty: Medium**

A company is designing a VPC with the following requirements:
- Support 2,000 hosts across multiple subnets
- Allow for future expansion to 5,000 hosts
- Need public and private subnets across 3 Availability Zones

Which VPC CIDR block provides adequate capacity with minimal waste?

A) 10.0.0.0/16 (65,536 IPs)
B) 10.0.0.0/19 (8,192 IPs)
C) 10.0.0.0/21 (2,048 IPs)
D) 10.0.0.0/20 (4,096 IPs)

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- Need 5,000 IPs for future expansion
- AWS reserves 5 IPs per subnet (network address, VPC router, DNS, future use, broadcast)
- With 6 subnets (public + private × 3 AZs), need buffer for subnet overhead and growth
- **10.0.0.0/19 = 8,192 IPs** provides comfortable room for 5,000 hosts + subnet overhead + future growth

**Why other answers are wrong:**
- **A**: /16 (65,536 IPs) is excessive for 5,000 hosts (wastes 60,000+ IP addresses)
- **C**: /21 (2,048 IPs) barely fits current 2,000 hosts requirement; no room for expansion to 5,000 hosts
- **D**: /20 (4,096 IPs) might seem sufficient, but with 6 subnets + AWS reserved IPs + growth, it's too tight

**Key Exam Tip:**
- AWS reserves 5 IPs per subnet (.0, .1, .2, .3, .255)
- VPC CIDR between /16 (largest) and /28 (smallest)
- Common sizes: /16 (65K), /20 (4K), /24 (256)
- Plan for future growth + subnet overhead

</details>

---

## Question 15 (EC2 - Advanced)
**Difficulty: Hard**

A financial application requires persistent storage that survives instance termination, supports 100,000 IOPS, and provides single-digit millisecond latency. The storage must be encrypted and support point-in-time snapshots. Which solution meets these requirements MOST cost-effectively?

A) EBS io2 Block Express volume with EBS encryption
B) EBS gp3 volume with EBS encryption
C) Instance Store with RAID 0 configuration
D) EBS io2 volume with EBS encryption

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: D**

**Explanation:**
- **EBS io2 volume** provides up to 64,000 IOPS per volume (256,000 IOPS with io2 Block Express), sub-millisecond latency
- **EBS encryption** provides encryption at rest and in transit
- **Snapshots** supported for point-in-time backups
- **Survives instance termination** (persistent storage)
- io2 is more cost-effective than io2 Block Express for 100,000 IOPS requirement (Block Express is for >64,000 IOPS)

**Why other answers are wrong:**
- **A**: io2 Block Express is overkill for 100,000 IOPS (supports up to 256,000 IOPS); it's more expensive than regular io2 and only needed for >64,000 IOPS per volume
- **B**: gp3 maxes out at 16,000 IOPS per volume (insufficient for 100,000 IOPS requirement)
- **C**: Instance Store does NOT survive instance termination (ephemeral storage); no native encryption; no snapshots; violates persistence requirement

**Key Exam Tip:**
- gp3: Up to 16,000 IOPS (cost-effective for most workloads)
- io2: Up to 64,000 IOPS, 99.999% durability
- io2 Block Express: Up to 256,000 IOPS (premium pricing)
- Instance Store: Ephemeral (does NOT survive termination)

</details>

---

## Question 16 (S3 - Performance)
**Difficulty: Medium**

An application uploads millions of small objects (1-10 KB each) to S3 every hour. The upload performance is poor despite using multi-part uploads. What is the MOST likely cause and solution?

A) Small objects don't benefit from multi-part upload; use S3 Transfer Acceleration instead
B) S3 request rate limit is being hit; implement random key prefixes or use S3 Express One Zone
C) Need to use CloudFront in front of S3 for better upload performance
D) Switch to S3 Intelligent-Tiering for better performance

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Explanation:**
- **Multi-part upload** is designed for large objects (>100 MB recommended); it provides NO benefit for small objects (1-10 KB)
- For small objects, the overhead of initiating multi-part uploads actually DEGRADES performance
- **S3 Transfer Acceleration** uses CloudFront edge locations to optimize uploads (especially useful for geographically distributed uploads)
- Normal S3 PUT requests perform better for small objects than multi-part uploads

**Why other answers are wrong:**
- **B**: S3 automatically scales to handle high request rates (3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD per prefix per second); prefixes help but aren't the primary issue here; S3 Express One Zone is for ultra-low latency single-digit millisecond access (not upload performance)
- **C**: CloudFront is primarily for content DELIVERY (reads), not uploads; Transfer Acceleration (which uses CloudFront edges) is better for uploads
- **D**: Intelligent-Tiering is a storage class for cost optimization (automatic tiering), not performance optimization

**Key Exam Tip:**
- Multi-part upload: Objects >100 MB (5 GB+ required for objects >5 GB)
- Transfer Acceleration: Global uploads, far from region
- S3 scales automatically to high request rates

</details>

---

## Question 17 (Auto Scaling - Policies)
**Difficulty: Hard**

An application experiences varying load throughout the day. Traffic gradually increases from 8 AM to 12 PM, spikes suddenly during flash sales (unpredictable timing), and drops sharply after 8 PM. Which Auto Scaling policy combination provides the BEST balance of performance and cost?

A) Target Tracking policy for CPU utilization only
B) Scheduled actions at 8 AM, 12 PM, 8 PM + Target Tracking policy for CPU utilization
C) Step Scaling with multiple CloudWatch alarms
D) Scheduled actions at 8 AM and 8 PM only

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Explanation:**
- **Scheduled actions** handle predictable patterns (8 AM ramp-up, 8 PM scale-down), adding capacity BEFORE traffic arrives (avoiding cold starts)
- **Target Tracking** handles unpredictable flash sales by reacting to actual load (e.g., CPU, ALB requests)
- This hybrid approach optimizes both performance (proactive scheduled scaling) and cost (reactive scaling for unpredictable spikes)

**Why other answers are wrong:**
- **A**: Purely reactive; slow to respond during 8 AM ramp-up (new instances take minutes to launch and warm up)
- **C**: Step Scaling requires more operational overhead than Target Tracking (manual threshold definition); no proactive scaling for predictable patterns
- **D**: No scaling for flash sales (unpredictable spikes); also missing 12 PM action for gradual increase handling

**Key Exam Tip:**
- Predictable patterns = Scheduled actions (proactive)
- Unpredictable load = Target Tracking (reactive, least overhead)
- You CAN and SHOULD combine multiple scaling policy types
- Step Scaling = more control, more overhead

</details>

---

## Question 18 (Load Balancing - Health Checks)
**Difficulty: Medium**

An Application Load Balancer is routing traffic to EC2 instances in an Auto Scaling group. Instances are marked unhealthy and terminated, but investigation shows the application is running correctly. The application takes 60 seconds to start accepting requests after launch. What is the MOST likely cause?

A) Health check interval is too short
B) Health check timeout is too short
C) Unhealthy threshold is too low
D) Health check grace period is too short

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: D**

**Explanation:**
- **Health check grace period** is the time Auto Scaling waits BEFORE checking instance health after launch
- With a 60-second startup time, if the grace period is <60 seconds, Auto Scaling will terminate instances before they're fully ready
- Default grace period is 300 seconds (5 minutes); if shortened or if health checks start too soon, instances are prematurely terminated

**Why other answers are wrong:**
- **A**: Health check interval determines how often checks are performed (default 30 seconds); too short would increase check frequency but wouldn't cause immediate termination
- **B**: Health check timeout is how long to wait for a response (default 5 seconds); too short would cause false negatives but not immediate termination
- **C**: Unhealthy threshold is the number of consecutive failed checks before marking unhealthy (default 2); low threshold would speed up detection but isn't the root cause

**Key Exam Tip:**
- Health check grace period = time before Auto Scaling starts checking instance health
- Set grace period > application startup time + health check interval × unhealthy threshold
- Common mistake: forgetting to account for application startup time

</details>

---

## Question 19 (S3 - Access Control)
**Difficulty: Hard**

A company needs to grant third-party vendors read access to specific objects in an S3 bucket for a limited time (24 hours), without sharing AWS credentials or creating IAM users for each vendor. Objects are encrypted with SSE-KMS. Which solution is MOST secure and operationally efficient?

A) Generate pre-signed URLs with 24-hour expiration
B) Create temporary IAM users with access keys, delete after 24 hours
C) Make objects public for 24 hours, then revert to private
D) Use S3 Access Points with time-based policies

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Explanation:**
- **Pre-signed URLs** grant temporary access to specific S3 objects without AWS credentials
- URLs automatically expire after the specified duration (24 hours)
- Works with SSE-KMS encrypted objects (the IAM entity generating the URL must have KMS decrypt permissions)
- No IAM user creation or credential sharing required

**Why other answers are wrong:**
- **B**: Creating temporary IAM users is operationally inefficient (manual user lifecycle management), requires distributing access keys (security risk), and needs cleanup after 24 hours
- **C**: Making objects public exposes them to EVERYONE (not just vendors), violates security requirements, and requires manual reversion
- **D**: S3 Access Points simplify multi-tenant access but don't natively support time-based expiration; still need IAM credentials for vendors

**Key Exam Tip:**
- Pre-signed URLs = temporary access without credentials
- Works with encrypted objects (SSE-KMS, SSE-S3, SSE-C)
- Generated using SDK/CLI with user's credentials and expiration time

</details>

---

## Question 20 (VPC - Advanced)
**Difficulty: Hard**

An application in VPC A (CIDR 10.0.0.0/16) needs to access resources in VPC B (CIDR 10.1.0.0/16) without using the internet. Both VPCs are in the same region and AWS account. Which solution provides the MOST secure and cost-effective connectivity?

A) VPC Peering connection between VPC A and VPC B
B) Transit Gateway with VPC A and VPC B attachments
C) VPN connection between VPC A and VPC B
D) Internet Gateway in both VPCs with Elastic IPs

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Explanation:**
- **VPC Peering** creates a direct network route between two VPCs using private IP addresses
- Traffic stays within AWS network (doesn't traverse internet)
- No single point of failure, no bandwidth bottleneck
- Most cost-effective solution for connecting two VPCs ($0.01/GB for data transfer)

**Why other answers are wrong:**
- **B**: Transit Gateway is designed for connecting MULTIPLE VPCs (3+) or hybrid networks; for two VPCs, it's more expensive ($0.05/hour per attachment + $0.02/GB) and adds unnecessary complexity
- **C**: VPN connection is for connecting on-premises networks to AWS (not VPC-to-VPC); requires VPN gateways (additional cost and complexity)
- **D**: Using Internet Gateways routes traffic over the public internet (violates "without using the internet" requirement), exposes data to internet risks, and requires additional security measures

**Key Exam Tip:**
- 2 VPCs = VPC Peering (simplest, cheapest)
- 3+ VPCs or hub-and-spoke = Transit Gateway
- On-premises to AWS = Direct Connect (dedicated) or Site-to-Site VPN (encrypted over internet)
- VPC Peering: Non-overlapping CIDR blocks required

</details>

---

## Answer Key

1. C
2. B
3. C
4. B
5. B
6. D
7. B
8. D
9. C
10. B
11. A
12. C
13. B
14. B
15. D
16. A
17. B
18. D
19. A
20. A

---

## Scoring

- **18-20 correct (90-100%)**: Outstanding! You're ready for Day 6 content.
- **15-17 correct (75-85%)**: Solid performance. Review missed topics before Day 6.
- **12-14 correct (60-70%)**: Need more review. Go back through Days 1-5 materials.
- **<12 correct (<60%)**: Significant gaps. Deep dive into weak areas before proceeding.

---

## Topics to Review Based on Mistakes

**If you missed Questions 1, 2, 15:** Review EC2 instance types, pricing models, placement groups, and EBS volume types

**If you missed Questions 3, 17, 18:** Review Auto Scaling policies (Target Tracking vs Scheduled vs Step), health check grace periods

**If you missed Questions 4, 5:** Review Load Balancer types (ALB vs NLB vs GLB), use cases, and Layer 4 vs Layer 7

**If you missed Questions 6, 7, 16:** Review S3 storage classes, lifecycle policies, retrieval times, and performance optimization

**If you missed Questions 8, 9, 10, 19:** Review S3 security (SSE-KMS vs SSE-C vs client-side encryption), replication, Object Lock, pre-signed URLs

**If you missed Questions 11, 12, 13, 14, 20:** Review VPC fundamentals (IGW vs NAT Gateway, Security Groups vs NACLs, CIDR design, VPC Peering)

---

Good luck! Remember: The SAA-C03 exam loves scenario-based questions, so understanding WHY an answer is correct (and why others are wrong) is more important than memorization.
