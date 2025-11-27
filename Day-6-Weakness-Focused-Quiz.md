# Day 6 Weakness-Focused Quiz - Days 1-6 Topics
**Date: November 26, 2025**
**Topics Covered: Days 1-6 (EC2, Auto Scaling, Load Balancing, S3, VPC, Advanced Networking)**
**Focus: Your 5 Critical Weak Areas + Day 6 New Material**

---

## Instructions
- 10 questions testing Days 1-6 topics
- Each question targets one of your documented weak areas OR Day 6 new concepts
- You need 80% (8/10) to feel confident moving forward
- Take your time, read each question carefully
- Pay attention to exam keywords: "MOST cost-effective", "LEAST operational overhead", "MOST secure"

---

## Questions

### Question 1: S3 Lifecycle - Storage Class Transitions
A company stores application logs in S3 for compliance purposes. The logs are accessed frequently for the first 30 days for troubleshooting. After 30 days, logs are **rarely accessed** but must be available immediately when needed (typically once or twice per month). After 90 days, logs are archived for compliance and can tolerate 3-5 hour retrieval times. After 7 years, logs are deleted.

Which lifecycle policy is the MOST cost-effective solution?

**A.** Day 0-30: S3 Standard → Day 30: Transition to S3 Glacier Flexible Retrieval → Day 90: Transition to S3 Glacier Deep Archive → Day 2,555 (7 years): Expiration

**B.** Day 0-30: S3 Standard → Day 30: Transition to S3 Standard-IA → Day 90: Transition to S3 Glacier Flexible Retrieval → Day 2,555 (7 years): Expiration

**C.** Day 0-30: S3 Standard → Day 30: Transition to S3 Glacier Instant Retrieval → Day 90: Transition to S3 Glacier Deep Archive → Day 2,555 (7 years): Expiration

**D.** Day 0-30: S3 Standard → Day 30: Transition to S3 One Zone-IA → Day 90: Transition to S3 Glacier Deep Archive → Day 2,555 (7 years): Expiration

---

### Question 2: VPC Security - NACLs and Ephemeral Ports
A solutions architect is troubleshooting a connectivity issue. EC2 instances in a private subnet can SSH to each other successfully, but cannot make outbound HTTPS requests to an external API. The instances use a NAT Gateway for internet access. The Security Group allows all outbound traffic, and the NACL has the following rules:

**NACL Outbound Rules:**
- Rule 100: Allow TCP port 443 to 0.0.0.0/0
- Rule 110: Allow TCP port 22 to 10.0.0.0/16

**NACL Inbound Rules:**
- Rule 100: Allow TCP port 22 from 10.0.0.0/16
- Rule 110: Allow TCP port 443 from 0.0.0.0/0

The instances timeout when making HTTPS requests to the external API. What is the MOST likely cause?

**A.** The private subnet route table is missing a route to the NAT Gateway for 0.0.0.0/0 traffic

**B.** The NACL inbound rules do not allow return traffic on ephemeral ports (1024-65535) from 0.0.0.0/0

**C.** The Security Group is stateful and blocking the return HTTPS traffic

**D.** The NAT Gateway's Elastic IP is not associated correctly with the Internet Gateway

---

### Question 3: S3 Encryption - Client-Side vs SSE-KMS
A financial services company must store customer data in S3 with the following requirements:
- Data must be encrypted at rest using FIPS 140-2 Level 3 validated encryption modules
- **AWS should NOT have access to the encryption keys**
- The encryption solution must meet strict regulatory requirements for key isolation
- Audit trail of all encryption operations is required

Which solution meets these requirements?

**A.** Use SSE-KMS with customer-managed CMKs (Customer Master Keys) and enable CloudTrail logging for all KMS API calls

**B.** Use SSE-C (Server-Side Encryption with Customer-Provided Keys) and provide keys with each S3 API request

**C.** Use client-side encryption with AWS CloudHSM to generate and store encryption keys, then upload encrypted objects to S3

**D.** Use SSE-S3 with default encryption enabled on the S3 bucket and restrict access using IAM policies

---

### Question 4: Auto Scaling Policies - Combining Scheduled and Target Tracking
An e-commerce application experiences predictable traffic patterns with spikes at 9 AM when users start their workday and drops at 8 PM when users go home. The application also experiences unpredictable traffic spikes during flash sales and promotional campaigns. The company wants to optimize costs while ensuring performance during both predictable and unpredictable load patterns.

Which Auto Scaling configuration provides the MOST cost-effective solution with the LEAST operational overhead?

**A.** Use Target Tracking scaling policy only, targeting 70% average CPU utilization

**B.** Use Scheduled Actions to scale up at 8:30 AM and scale down at 8:00 PM, combined with Target Tracking policy targeting 70% CPU utilization

**C.** Use Step Scaling with multiple CloudWatch alarms for different CPU thresholds (60%, 70%, 80%, 90%)

**D.** Use Predictive Scaling only, allowing AWS ML models to forecast and schedule capacity

---

### Question 5: S3 Access Control - Pre-signed URLs vs Access Points
A company needs to share large video files stored in S3 with external video editors who do not have AWS accounts. The access should be temporary (valid for 48 hours only) and should not require creating or managing IAM users. The solution should provide the LEAST operational overhead.

Which solution meets these requirements?

**A.** Create an S3 Access Point with a time-based IAM policy that expires after 48 hours

**B.** Generate pre-signed URLs for the video files with a 48-hour expiration time and share them with the external editors

**C.** Create temporary IAM users with access keys, grant them S3 read permissions, and manually delete the users after 48 hours

**D.** Make the S3 bucket objects public for 48 hours, then revert them to private after the editors complete their work

---

### Question 6: VPC Endpoints - Gateway vs Interface
A company has a large application running on EC2 instances in a private subnet. The application frequently reads and writes data to S3 buckets and queries DynamoDB tables. The company wants to optimize costs and improve security by ensuring traffic to S3 and DynamoDB does not traverse the internet or NAT Gateway.

Which solution provides the MOST cost-effective access to both S3 and DynamoDB?

**A.** Create VPC Gateway Endpoints for both S3 and DynamoDB, and update the route table to direct traffic through the endpoints

**B.** Create VPC Interface Endpoints for both S3 and DynamoDB with PrivateDNS enabled

**C.** Configure AWS PrivateLink to establish private connectivity to S3 and DynamoDB services

**D.** Use a NAT Gateway in the private subnet and create Security Group rules to restrict traffic to S3 and DynamoDB IP ranges only

---

### Question 7: VPC Connectivity - Transit Gateway vs VPC Peering
A company has 15 VPCs across three AWS regions (us-east-1, us-west-2, eu-west-1) and an on-premises data center connected via AWS Direct Connect. The company needs full mesh connectivity where:
- All VPCs can communicate with each other across regions
- All VPCs can communicate with the on-premises data center
- The solution should scale easily as new VPCs are added
- Routing should be transitive (if VPC-A connects to VPC-B, and VPC-B connects to VPC-C, then VPC-A should reach VPC-C)

Which solution provides the MOST scalable architecture with the LEAST operational overhead?

**A.** Create VPC Peering connections between all VPCs (105 peering connections for full mesh) and attach each VPC individually to the Direct Connect Virtual Private Gateway

**B.** Deploy an AWS Transit Gateway in each region, connect all VPCs in each region to their regional Transit Gateway, establish Transit Gateway peering between regions, and attach the Direct Connect to one Transit Gateway

**C.** Create a hub-and-spoke architecture using one central VPC with VPC Peering connections to all other VPCs, and route all traffic through the hub VPC

**D.** Use AWS PrivateLink to expose services from each VPC, and create Interface Endpoints in all other VPCs that need to access those services

---

### Question 8: Hybrid Connectivity - VPN vs Direct Connect
A startup company needs to establish connectivity between their on-premises data center and their new AWS VPC. They have the following requirements:
- Connectivity must be established within 2 days (urgent migration)
- Bandwidth requirement: 500 Mbps initially, may grow to 2 Gbps in 6 months
- Data transfer is sensitive but not subject to strict compliance requirements
- Budget-conscious in the initial phase

Which solution should the solutions architect recommend for the INITIAL setup, with a plan to upgrade later?

**A.** Provision a 1 Gbps AWS Direct Connect connection immediately, even though setup takes 2-4 weeks, because it's the most secure option

**B.** Set up AWS Site-to-Site VPN immediately to meet the 2-day deadline, then provision Direct Connect over the next 2 months and transition to it

**C.** Set up multiple Site-to-Site VPN connections (4 tunnels total) to achieve 5 Gbps aggregate bandwidth and avoid Direct Connect costs

**D.** Wait for Direct Connect to be provisioned (2-4 weeks) while temporarily using VPN as a backup, then switch to Direct Connect as primary

---

### Question 9: Load Balancer Selection - Static IP and Protocol Requirements
A gaming company is deploying a multiplayer game server architecture on AWS. The game uses UDP protocol for real-time player communication and requires extremely low latency (<10ms). Third-party firewall appliances used by enterprise customers need to whitelist specific static IP addresses to allow game traffic.

Which load balancer type should the company use?

**A.** Application Load Balancer (ALB) with AWS Global Accelerator to provide static IP addresses

**B.** Network Load Balancer (NLB) with Elastic IP addresses assigned to each Availability Zone

**C.** Gateway Load Balancer (GLB) to handle UDP traffic and integrate with third-party firewall appliances

**D.** Classic Load Balancer (CLB) configured for TCP/UDP load balancing with Elastic IPs

---

### Question 10: EC2 Auto Scaling - Health Check Configuration
A web application runs on EC2 instances behind an Application Load Balancer in an Auto Scaling group. The application occasionally crashes due to memory leaks, but the EC2 instances remain running and pass EC2 status checks. When crashes occur, users receive HTTP 502 Bad Gateway errors intermittently until engineers manually restart the affected instances.

Which configuration change will AUTOMATICALLY detect and replace the unhealthy instances?

**A.** Change the Auto Scaling group health check type from EC2 to ELB health checks

**B.** Configure CloudWatch to monitor the StatusCheckFailed_Instance metric and trigger instance replacement via SNS

**C.** Enable detailed CloudWatch monitoring to collect application-level metrics every 1 minute instead of 5 minutes

**D.** Increase the health check grace period from 300 seconds to 600 seconds to allow more time for the application to stabilize

---

## Answer Key (Do NOT look until you've answered all questions!)

<details>
<summary><strong>Click here to reveal answers after completing the quiz</strong></summary>

### Question 1: CORRECT ANSWER - B

**Explanation:**
- **Days 0-30** (frequent access): S3 Standard
- **Days 31-90** ("rarely accessed" but need immediate access): **S3 Standard-IA**
  - Key insight: "Rarely accessed" ≠ "archive" - it means occasional access (1-2x per month)
  - Standard-IA provides millisecond retrieval with no retrieval delay fees for expedited retrieval
  - Cost: $0.0125/GB/month vs $0.023/GB/month for Standard (45% savings)
- **Days 91+** (can tolerate 3-5 hour retrieval): S3 Glacier Flexible Retrieval
  - 3-5 hours = Standard retrieval tier for Glacier Flexible
  - Meets the "can tolerate hours" requirement
  - Cost: $0.0036/GB/month
- **After 7 years**: Automatic deletion via lifecycle expiration

**Why others are wrong:**
- **A**: Day 30 → Glacier Flexible is wrong because "rarely accessed but must be available immediately" contradicts the 1-5 minute (expedited) or 3-5 hour (standard) retrieval time. This is the trap answer that catches people who see "rarely accessed" and immediately think Glacier.
- **C**: Glacier Instant Retrieval costs more than Standard-IA ($0.004/GB/month) but is meant for data accessed quarterly. For monthly access patterns, Standard-IA is more cost-effective.
- **D**: One Zone-IA lacks durability guarantees for compliance logs (99.5% availability vs 99.9% for Standard-IA). Compliance data should not use single-AZ storage.

**Exam Keywords:**
- "Rarely accessed" + "immediately when needed" = **Standard-IA** (NOT Glacier!)
- "Can tolerate 3-5 hours" = Glacier Flexible Retrieval
- "MOST cost-effective" = Lowest cost tier that meets performance requirements

**Your Weak Area:** You've been jumping to Glacier too quickly when you see "rarely accessed". Standard-IA is for data that's infrequently accessed BUT needs instant retrieval.

---

### Question 2: CORRECT ANSWER - B

**Explanation:**
The NACL is **stateless**, meaning return traffic must be explicitly allowed. When instances make outbound HTTPS requests:
1. Outbound request: Instance → NAT Gateway → Internet (port 443)
2. Return response: Internet → NAT Gateway → Instance (ephemeral port 1024-65535)

The NACL outbound rules correctly allow port 443, but the **inbound rules are missing ephemeral port ranges** (1024-65535) for return traffic.

**Current NACL inbound rules:**
- Rule 100: Allow TCP port 22 from 10.0.0.0/16 ✅ (for SSH between instances)
- Rule 110: Allow TCP port 443 from 0.0.0.0/0 ❌ (wrong - this allows NEW inbound HTTPS connections, not return traffic)

**What's needed:**
- Rule 120: Allow TCP ports 1024-65535 from 0.0.0.0/0 (for return traffic)

**Why others are wrong:**
- **A**: If route table was missing NAT Gateway route, requests wouldn't leave the subnet at all (immediate failure, not timeout). Connection timeout indicates requests ARE leaving but responses AREN'T returning.
- **C**: Security Groups are **stateful** - they automatically allow return traffic. This is not the issue.
- **D**: If NAT Gateway's EIP wasn't associated, no traffic would flow at all (not just HTTPS).

**Exam Keywords:**
- "Connection timeout" + "Security Group allows all outbound" = **NACL blocking return traffic**
- "Stateless" = NACLs (must configure both directions)
- "Stateful" = Security Groups (automatic return traffic)

**Your Weak Area:** Remember - Security Groups are your friends (stateful), NACLs are not (stateless). When troubleshooting connectivity with "timeout" + "SG allows outbound", immediately think NACL ephemeral ports.

---

### Question 3: CORRECT ANSWER - C

**Explanation:**
The key requirement is **"AWS should NOT have access to the encryption keys"** + **FIPS 140-2 Level 3**.

Let's evaluate each option:

| Encryption Type | AWS Has Key Access? | FIPS 140-2 Level | Audit Trail? |
|----------------|-------------------|------------------|--------------|
| **SSE-S3** | ✅ YES (AWS manages) | Level 2/3 | ❌ NO |
| **SSE-KMS** | ✅ YES (AWS performs operations) | Level 2/3 | ✅ YES (CloudTrail) |
| **SSE-C** | ⚠️ Partially (keys sent with requests) | Level 2/3 | ❌ NO |
| **Client-side + CloudHSM** | ❌ **NO** (AWS never sees keys) | **Level 3** ✅ | ✅ YES (CloudHSM logs) |

**Why C is correct:**
- **Client-side encryption**: You encrypt data BEFORE uploading to S3 (AWS receives already-encrypted data)
- **CloudHSM**: FIPS 140-2 Level 3 validated hardware security module (KMS is Level 2/3)
- **Key isolation**: Keys never leave CloudHSM, AWS has no access
- **Audit trail**: CloudHSM provides detailed audit logs of all cryptographic operations

**Why others are wrong:**
- **A (SSE-KMS)**: Even with customer-managed CMKs, AWS KMS performs the encryption/decryption operations. AWS has operational access to perform cryptographic operations on your behalf. This violates "AWS should NOT have access to keys".
- **B (SSE-C)**: You provide keys with each request, but no FIPS 140-2 Level 3 guarantee, and no comprehensive audit trail.
- **D (SSE-S3)**: AWS fully manages keys and encryption. No audit trail. Violates requirements.

**Exam Keywords:**
- "AWS should NOT have access to keys" = **Client-side encryption** (NOT SSE-KMS!)
- "FIPS 140-2 Level 3" = **CloudHSM** (NOT KMS)
- "Audit trail via CloudTrail" = **SSE-KMS** (different requirement!)

**Your Weak Area:** You keep choosing SSE-KMS when you see "audit trail" or "encryption", but if the question says "AWS should NOT have access to keys", SSE-KMS is ELIMINATED. Even customer-managed KMS keys give AWS operational access.

---

### Question 4: CORRECT ANSWER - B

**Explanation:**
The scenario has TWO distinct traffic patterns:
1. **Predictable**: 9 AM spike (users start work), 8 PM drop (users go home)
2. **Unpredictable**: Flash sales, promotional campaigns

**Why B is correct - Combine both policies:**

**Scheduled Actions** (for predictable patterns):
- 8:30 AM: Scale up to baseline capacity BEFORE 9 AM spike
- Proactive scaling prevents cold start lag (2-5 minutes for new instances)
- 8:00 PM: Scale down to save costs when traffic drops
- No waiting for CPU to spike → better user experience

**Target Tracking** (for unpredictable patterns):
- Target 70% CPU utilization
- Automatically scales up during flash sales (beyond scheduled capacity)
- Automatically scales down when flash sale ends
- LEAST operational overhead (simple configuration, no manual intervention)

**When multiple policies are configured:**
- AWS Auto Scaling evaluates all policies
- **Chooses the one that provides MORE capacity** (safer approach)
- If scheduled action sets 10 instances, but CPU spikes and Target Tracking wants 15 → scales to 15

**Why others are wrong:**
- **A**: Target Tracking alone is reactive - waits for CPU to spike, then scales (2-5 minute lag). Users experience degraded performance during the spike while instances launch. Misses the opportunity to proactively scale for known 9 AM pattern.
- **C**: Step Scaling requires managing multiple CloudWatch alarms and thresholds. More operational overhead than Target Tracking. No proactive scaling for predictable patterns.
- **D**: Predictive Scaling alone learns patterns but doesn't handle unpredictable flash sales well. Also, "predictable patterns" in the question refer to time-of-day patterns, which are best handled by Scheduled Actions (more direct control).

**Exam Keywords:**
- "Predictable patterns" + "unpredictable patterns" in same question = **Combine policies!**
- "LEAST operational overhead" for unpredictable = **Target Tracking**
- "Proactive scaling" for predictable = **Scheduled Actions**

**Your Weak Area:** You defaulted to Target Tracking only (reactive approach). When a question mentions BOTH predictable time-based patterns AND unpredictable load, the answer is to COMBINE Scheduled Actions + Target Tracking.

---

### Question 5: CORRECT ANSWER - B

**Explanation:**
The requirements are:
- Temporary access (48 hours)
- External users without AWS accounts
- No IAM user management
- LEAST operational overhead

**Pre-signed URLs are perfect for this:**
- Generate a signed URL with 48-hour expiration: `aws s3 presign s3://bucket/video.mp4 --expires-in 172800`
- Share URL with external editors (via email, Slack, etc.)
- URL automatically expires after 48 hours (no manual cleanup)
- No AWS credentials required for recipients
- Works with SSE-KMS encrypted objects (if generator has permissions)
- Can be generated in seconds

**Why others are wrong:**
- **A (S3 Access Points)**: Access Points simplify access for multi-tenant scenarios with different permission requirements per team/application. They do NOT provide native time-based expiration. Access Points still require IAM credentials (access keys or roles). Operationally complex for simple "48-hour access" requirement.
- **C (Temporary IAM users)**: High operational overhead - must create users, generate access keys, distribute credentials securely, manually delete users after 48 hours. Also requires recipients to configure AWS CLI/SDK with credentials. Violates "LEAST operational overhead".
- **D (Public bucket)**: Major security risk. Makes objects accessible to EVERYONE on the internet, not just the intended editors. No way to enforce 48-hour time limit. Violates security best practices.

**When to use each:**
- **Pre-signed URLs**: Temporary access (hours to days), no AWS credentials needed, specific objects
- **S3 Access Points**: Multi-tenant, long-term structured access, different permissions per team
- **IAM Users**: Long-term programmatic access for known users
- **Public bucket**: Never (exam will always mark this as wrong unless explicitly required)

**Exam Keywords:**
- "Temporary access" + "limited time" + "no AWS credentials" = **Pre-signed URLs**
- "Third-party" + "48 hours" = **Pre-signed URLs**
- "Multi-tenant" + "separate permissions" = **S3 Access Points**

**Your Weak Area:** You chose Access Points when the question needed temporary, time-limited access. Pre-signed URLs are the go-to for temporary external sharing.

---

### Question 6: CORRECT ANSWER - A

**Explanation:**
Both S3 and DynamoDB support **VPC Gateway Endpoints**, which are:
- **Free** (no hourly charge, no data transfer charge)
- High bandwidth
- No ENI (Elastic Network Interface) required
- Configured via route table entries (traffic to S3/DynamoDB → Gateway Endpoint)

**Configuration:**
1. Create VPC Gateway Endpoint for S3
2. Create VPC Gateway Endpoint for DynamoDB
3. Update private subnet route table to point S3 prefix list → S3 Gateway Endpoint
4. Update private subnet route table to point DynamoDB prefix list → DynamoDB Gateway Endpoint
5. Traffic now flows: EC2 → Gateway Endpoint → S3/DynamoDB (never leaves AWS network)

**Benefits:**
- No NAT Gateway costs (no hourly charge, no per-GB data transfer charge)
- No internet exposure
- Better performance (stays on AWS backbone network)
- Completely free

**Why others are wrong:**
- **B (Interface Endpoints for S3 and DynamoDB)**: Interface Endpoints cost **$0.01/hour per AZ** + **$0.01/GB data processed**. For high-volume S3/DynamoDB access, this is expensive. Interface Endpoints are for services that DON'T support Gateway Endpoints (EC2, SNS, SQS, etc.). Since S3 and DynamoDB support Gateway Endpoints, use those instead.
- **C (AWS PrivateLink)**: PrivateLink is for accessing services in OTHER VPCs or third-party SaaS services. It's not needed for accessing AWS services like S3/DynamoDB. PrivateLink uses Interface Endpoints under the hood (costs money).
- **D (NAT Gateway + Security Groups)**: NAT Gateway costs **$0.045/hour** + **$0.045/GB data processed**. For high-volume S3/DynamoDB traffic, this is expensive and unnecessary. Security Groups cannot restrict traffic to S3/DynamoDB IP ranges (IPs change dynamically, managed by AWS).

**Key Facts:**
| VPC Endpoint Type | Services | Cost | Implementation |
|------------------|----------|------|----------------|
| **Gateway Endpoint** | **S3, DynamoDB** | **FREE** ✅ | Route table entry |
| **Interface Endpoint** | Most other AWS services | **$0.01/hr/AZ + $0.01/GB** | ENI in subnet, Security Group |

**Exam Keywords:**
- "MOST cost-effective" + "S3 and/or DynamoDB" = **Gateway Endpoints** (free!)
- "Private access" + "S3/DynamoDB" = **Gateway Endpoints**
- "Other AWS services" (EC2, SNS, SQS) = **Interface Endpoints**

**Day 6 Topic:** VPC Endpoints - Gateway vs Interface is a Day 6 concept!

---

### Question 7: CORRECT ANSWER - B

**Explanation:**
This is a complex multi-VPC, multi-region, hybrid connectivity scenario. Let's evaluate the requirements:
- 15 VPCs across 3 regions
- Full mesh connectivity (all VPCs talk to each other)
- On-premises connectivity via Direct Connect
- **Transitive routing** (A→B→C means A can reach C)
- Scalable (easy to add new VPCs)

**Why Transit Gateway is the winner:**

**Architecture:**
1. Deploy AWS Transit Gateway in each region:
   - us-east-1: Transit Gateway 1 (connect all us-east-1 VPCs)
   - us-west-2: Transit Gateway 2 (connect all us-west-2 VPCs)
   - eu-west-1: Transit Gateway 3 (connect all eu-west-1 VPCs)

2. Establish Transit Gateway Peering between regions:
   - TGW1 ↔ TGW2 (us-east-1 ↔ us-west-2)
   - TGW2 ↔ TGW3 (us-west-2 ↔ eu-west-1)
   - TGW1 ↔ TGW3 (us-east-1 ↔ eu-west-1)

3. Attach Direct Connect to one Transit Gateway (e.g., TGW1 in us-east-1)

**Benefits:**
- **Transitive routing**: All VPCs can reach each other through Transit Gateways
- **Hub-and-spoke topology**: Each TGW is the hub for its region
- **Scalable**: Add new VPC → attach to regional TGW (one connection, not N connections)
- **Centralized management**: Route tables managed at TGW level
- **Supports up to 5,000 VPCs per Transit Gateway**

**Why others are wrong:**
- **A (VPC Peering)**: For 15 VPCs, full mesh requires **N × (N-1) / 2 = 15 × 14 / 2 = 105 peering connections**. This is operationally complex and doesn't scale. Peering is **non-transitive** (A→B→C does NOT mean A can reach C). Each VPC needs individual Direct Connect attachment (complex).
- **C (Hub-and-spoke with VPC Peering)**: Reduces peering connections (14 instead of 105), but creates a single point of failure and bottleneck (all traffic flows through hub VPC). Hub VPC needs massive bandwidth. Still doesn't provide true transitive routing (requires routing configuration in hub).
- **D (AWS PrivateLink)**: PrivateLink is for **service exposure**, not full network connectivity. It allows VPCs to access specific services (via NLB + Interface Endpoints) but not full IP connectivity between VPCs. Doesn't support transitive routing. Operationally complex for this use case.

**VPC Peering vs Transit Gateway:**
| Feature | VPC Peering | Transit Gateway |
|---------|-------------|----------------|
| **Connections for N VPCs** | N × (N-1) / 2 (full mesh) | N (one per VPC) |
| **Transitive routing** | ❌ NO | ✅ YES |
| **Scalability** | Poor (105 connections for 15 VPCs) | Excellent (up to 5,000 VPCs) |
| **Use case** | Simple 1:1 or small number of VPCs | Complex multi-VPC, hub-and-spoke |

**Exam Keywords:**
- "Multiple VPCs" + "transitive routing" = **Transit Gateway**
- "Full mesh" + "scalable" = **Transit Gateway**
- "Simple 1:1 VPC connection" = **VPC Peering**

**Day 6 Topic:** VPC Peering vs Transit Gateway is a Day 6 concept!

---

### Question 8: CORRECT ANSWER - B

**Explanation:**
This is a classic "VPN now, Direct Connect later" scenario:

**Requirements analysis:**
- **Urgent**: Must establish within 2 days
- **Initial bandwidth**: 500 Mbps (VPN can handle: up to 1.25 Gbps per tunnel)
- **Future bandwidth**: 2 Gbps in 6 months (requires Direct Connect)
- **Budget-conscious** initially

**Why B is correct:**

**Phase 1 - Immediate (Day 0-2):**
- Set up AWS Site-to-Site VPN (can be configured in minutes to hours)
- Provides encrypted connectivity over internet
- Cost: ~$0.05/hour per connection ($36/month) + data transfer
- Bandwidth: 1.25 Gbps per tunnel (2 tunnels per connection = HA)
- Meets 500 Mbps requirement

**Phase 2 - Long-term (Month 2-3):**
- Order Direct Connect (1 Gbps or 10 Gbps)
- Provisioning takes 2-4 weeks (AWS) + carrier coordination
- When Direct Connect is ready, transition traffic from VPN to Direct Connect
- Can keep VPN as backup (HA)

**Why others are wrong:**
- **A**: Waiting for Direct Connect (2-4 weeks) violates the "within 2 days" requirement. Business cannot wait.
- **C**: Multiple Site-to-Site VPN connections don't aggregate bandwidth (each connection is independent, 1.25 Gbps max per tunnel). You can't achieve 5 Gbps aggregate bandwidth with VPN. Also, VPN becomes expensive at high bandwidth (data transfer costs).
- **D**: Similar to A - waiting for Direct Connect violates "urgent" requirement. VPN is not a "temporary backup" in this scenario; it's the primary solution until Direct Connect is ready.

**VPN vs Direct Connect Decision Matrix:**
| Requirement | VPN | Direct Connect |
|-------------|-----|----------------|
| **Setup time** | Minutes to hours | 2-4 weeks+ |
| **Bandwidth** | Up to 1.25 Gbps | 1-100 Gbps |
| **Cost** | $ (low) | $$$ (high) |
| **Latency** | Variable (internet) | Consistent (dedicated) |
| **Use case** | Urgent, temporary, budget-conscious | High bandwidth, consistent latency, long-term |

**Exam Keywords:**
- "Urgent" or "within days" = **Start with VPN**
- "High bandwidth" or "consistent latency" = **Migrate to Direct Connect later**
- "Budget-conscious initially" = **VPN first, Direct Connect later**

**Day 6 Topic:** VPN vs Direct Connect is a Day 6 concept!

---

### Question 9: CORRECT ANSWER - B

**Explanation:**
Let's evaluate the requirements:
- **UDP protocol** (not HTTP/HTTPS)
- **Extremely low latency** (<10ms)
- **Static IP addresses** (for whitelisting by third-party firewalls)

**Why Network Load Balancer (NLB) is correct:**
- **Supports UDP** (also TCP, TLS) → Layer 4 load balancer
- **Ultra-low latency**: <100 microseconds (µs) = 0.1 milliseconds
- **Static IP**: One Elastic IP per Availability Zone (can assign your own EIPs)
- **Performance**: Millions of requests per second
- **Use case**: Gaming, IoT, real-time applications

**Configuration:**
1. Create NLB in multiple AZs (high availability)
2. Assign Elastic IPs to NLB (one per AZ)
3. Share Elastic IPs with enterprise customers for whitelisting
4. Configure UDP listeners to target EC2 game servers

**Why others are wrong:**
- **A (ALB + Global Accelerator)**: ALB is **Layer 7** (HTTP/HTTPS only) and does NOT support UDP. Even with Global Accelerator (which provides static IPs), ALB cannot handle UDP traffic. ALB is eliminated immediately when you see "UDP".
- **C (Gateway Load Balancer)**: GLB is for **3rd-party security appliances** (firewalls, IDS/IPS) in a transparent inspection mode. It's not designed for general application load balancing. GLB uses GENEVE protocol on port 6081 for encapsulation. While it technically supports IP protocols, it's architecturally the wrong tool for this use case.
- **D (Classic Load Balancer)**: CLB is **legacy** (deprecated on exam unless explicitly mentioned as existing infrastructure). CLB doesn't support Elastic IP assignment. Modern exams favor ALB/NLB/GLB.

**Load Balancer Decision Tree for UDP:**
```
Protocol: UDP
└─> ALB? ❌ NO (HTTP/HTTPS only)
└─> NLB? ✅ YES (TCP/UDP/TLS)
└─> GLB? ⚠️ Wrong use case (3rd-party appliances)
└─> CLB? ❌ NO (legacy, no Elastic IP)
```

**Static IP Requirement:**
```
Need static IP?
└─> ALB? ❌ NO (dynamic DNS only)
└─> NLB? ✅ YES (Elastic IP per AZ)
└─> GLB? ✅ YES (but wrong use case for this question)
```

**Exam Keywords:**
- "UDP protocol" = **NLB** (ALB is eliminated)
- "Static IP" or "Elastic IP" = **NLB** (ALB is eliminated)
- "Extremely low latency" = **NLB**
- "Third-party appliances" (firewalls, IDS/IPS) = **GLB**

**Your Pattern:** You've struggled with "static IP" questions before. Remember: **ALB has NO static IP** (dynamic DNS only).

---

### Question 10: CORRECT ANSWER - A

**Explanation:**
The problem:
- Application crashes (memory leaks)
- EC2 instances remain running (OS is fine)
- EC2 status checks PASS ✅ (instance is running, network is reachable)
- Application crashes → HTTP 502 Bad Gateway errors
- Manual restarts fix the issue (engineers must intervene)

**Why A is correct - Change to ELB health checks:**

**EC2 Status Checks** (current configuration):
- Check if instance is running (StatusCheckFailed_Instance)
- Check if network is reachable (StatusCheckFailed_System)
- **Do NOT check application health**
- Result: Crashed application still passes health check → ASG thinks instance is healthy → doesn't replace instance

**ELB Health Checks** (correct configuration):
- ALB sends HTTP requests to application endpoint (e.g., `/health`, `/ping`)
- If application responds with HTTP 200: Healthy ✅
- If application crashes: No response or HTTP 502/503/504: Unhealthy ❌
- ASG receives health check status from ELB
- ASG automatically terminates unhealthy instances and launches replacements
- **Detects application crashes, not just instance failures**

**Configuration:**
```bash
aws autoscaling update-auto-scaling-group \
    --auto-scaling-group-name my-asg \
    --health-check-type ELB \
    --health-check-grace-period 300
```

**Why others are wrong:**
- **B (CloudWatch metric + SNS)**: This notifies engineers but doesn't automatically replace instances. Manual intervention still required. Doesn't solve the "automatic" requirement.
- **C (Detailed monitoring)**: Changing from 5-minute to 1-minute metric collection frequency doesn't change what's being monitored (still EC2 status checks, not application health). Doesn't detect application crashes.
- **D (Increase grace period)**: Grace period is the time ASG waits after instance launch before checking health (allows application startup time). Increasing this from 300s to 600s means ASG waits LONGER before checking health, making the problem worse.

**Health Check Comparison:**
| Health Check Type | What It Checks | Detects Application Crashes? |
|------------------|----------------|----------------------------|
| **EC2 Status Checks** | Instance running, network reachable | ❌ NO |
| **ELB Health Checks** | Application endpoint responding | ✅ YES |

**Exam Keywords:**
- "Application crashes but instance running" = **Use ELB health checks**
- "Intermittent 502 errors" = Application unhealthy (not instance failure)
- "Automatically detect and replace" = **ELB health checks in ASG**

**Your Pattern:** This was covered in your weak areas before. EC2 checks = "Is server on?", ELB checks = "Is app working?"

---

</details>

---

## Scoring

**Total Correct: _____ / 10**

**Percentage: _____%**

**Performance Evaluation:**
- **9-10 correct (90-100%)**: Excellent! You've addressed your weak areas. Ready for Day 7 review.
- **8 correct (80%)**: Good, but review the questions you missed carefully.
- **7 correct (70%)**: Still at passing threshold. Re-study your weak areas before moving forward.
- **6 or below (<60%)**: Stop. Re-read the Weak-Areas-Cheat-Sheet.md and retake this quiz.

---

## Next Steps

1. **Record your answers** before looking at the answer key
2. **Check your answers** against the detailed explanations
3. **If you scored below 80%**: I will create an AWS-SAA-Weaknesses.md file documenting your specific gaps
4. **If you scored 80%+**: You're ready for Waldorf and Statler's brutally honest review

---

**Remember:** The exam is in 21 days. Every mistake now is a lesson learned before it costs you a passing score.
