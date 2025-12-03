# Day 3: VPC & Networking - Deep Dive

**Date:** Tuesday, December 2, 2025
**Topics:** VPC, Networking, Connectivity, Security
**Goal:** Master VPC architecture and networking patterns

---

## 🎯 Learning Objectives

By the end of this session, you will:
1. Understand VPC fundamentals (CIDR, subnets, routing)
2. Master Security Groups vs NACLs (stateful vs stateless - you know this!)
3. Know when to use NAT Gateway vs NAT Instance vs Internet Gateway
4. Understand VPC Endpoints (Gateway vs Interface - you learned this Day 1!)
5. Master VPC connectivity (Peering, Transit Gateway, PrivateLink)
6. Know Direct Connect vs VPN decision criteria
7. Understand Load Balancer types and when to use each
8. Master Route 53 routing policies

---

## Part 1: VPC Fundamentals

### What is a VPC?

**VPC = Virtual Private Cloud**
- Your own isolated network in AWS
- Complete control over IP addressing, subnets, routing, security
- Spans all AZs in a region (but subnets are tied to single AZ)

### CIDR Blocks

**CIDR (Classless Inter-Domain Routing)** defines IP address range.

**Format:** `10.0.0.0/16`
- **10.0.0.0**: Network address
- **/16**: Subnet mask (how many IPs)

**CIDR Math:**
- **/16**: 2^(32-16) = 65,536 IPs (VPC max size)
- **/24**: 2^(32-24) = 256 IPs (common subnet size)
- **/28**: 2^(32-28) = 16 IPs (VPC min size)

**Example:**
```
VPC: 10.0.0.0/16 (65,536 IPs)
├─ Public Subnet AZ1: 10.0.1.0/24 (256 IPs)
├─ Private Subnet AZ1: 10.0.2.0/24 (256 IPs)
├─ Public Subnet AZ2: 10.0.3.0/24 (256 IPs)
└─ Private Subnet AZ2: 10.0.4.0/24 (256 IPs)
```

### Reserved IPs in Every Subnet

**AWS reserves 5 IPs in each subnet:**

**Example subnet: 10.0.1.0/24**
- **10.0.1.0**: Network address (unusable)
- **10.0.1.1**: VPC router (unusable)
- **10.0.1.2**: DNS server (Route 53 Resolver) (unusable)
- **10.0.1.3**: Reserved for future use (unusable)
- **10.0.1.255**: Broadcast address (unusable)
- **Usable IPs**: 251 out of 256

**Exam trap:** "/24 has 256 IPs" → WRONG! Usable IPs = 251

---

### Public vs Private Subnets

| Feature | Public Subnet | Private Subnet |
|---------|---------------|----------------|
| **Internet access** | ✅ Via Internet Gateway | ✅ Via NAT Gateway (outbound only) |
| **Public IP** | ✅ Auto-assigned or Elastic IP | ❌ Private IP only |
| **Route table** | 0.0.0.0/0 → IGW | 0.0.0.0/0 → NAT Gateway |
| **Inbound from internet** | ✅ Yes (if Security Group allows) | ❌ No |
| **Use case** | Web servers, load balancers, bastion hosts | Databases, app servers, Lambda |

**What makes a subnet "public"?**
1. Route table has `0.0.0.0/0 → Internet Gateway`
2. Instance has public IP or Elastic IP
3. Security Group/NACL allows traffic

**What makes a subnet "private"?**
1. No direct route to Internet Gateway
2. No public IP
3. Internet access only via NAT Gateway (outbound)

---

### Internet Gateway (IGW)

**Purpose:** Allow VPC resources to access the internet

**Key facts:**
- ✅ **One per VPC** (can't attach multiple IGWs)
- ✅ **Highly available** (horizontally scaled, redundant)
- ✅ **No bandwidth constraints** (AWS managed)
- ✅ **Free** (no charges for IGW itself, only data transfer)

**Use case:** Public subnets need internet access (web servers, load balancers)

---

### NAT Gateway vs NAT Instance

**Purpose:** Allow private subnet instances to access internet (outbound only) without exposing them to inbound connections.

| Feature | NAT Gateway | NAT Instance |
|---------|-------------|--------------|
| **Management** | AWS managed (HA, auto-scaling) | You manage (EC2 instance) |
| **Availability** | **HA within AZ** (need one per AZ for HA) | Manual failover scripts required |
| **Bandwidth** | Up to 45 Gbps | Depends on instance type |
| **Cost** | $$$ ($/hour + $/GB processed) | $ (EC2 instance + data transfer) |
| **Security Group** | ❌ Cannot attach | ✅ Can attach |
| **Bastion host** | ❌ No (not an EC2 instance) | ✅ Yes (can use as bastion) |
| **Elastic IP** | ✅ Required (one per NAT Gateway) | ✅ Required |
| **Use case** | Production, HA required | Cost-sensitive, need bastion, low traffic |

#### Decision Tree

```
Need private instances to access internet?
├─ Production environment?
│  └─ YES → NAT Gateway (managed, HA, reliable)
│
├─ Low traffic, cost-sensitive?
│  └─ YES → NAT Instance (cheaper)
│
└─ Need bastion host functionality?
   └─ YES → NAT Instance (can SSH into it)
```

**Exam pattern:** Unless question says "cost-sensitive" or "bastion host," default answer is **NAT Gateway**.

---

## Part 2: Security - NACLs vs Security Groups

### YOU ALREADY KNOW THIS FROM DAY 1!

Remember your comprehensive quiz Q3? You nailed the NACL stateless behavior!

Let's reinforce and expand:

### The Critical Difference: STATEFUL vs STATELESS

| Feature | Security Group | NACL |
|---------|----------------|------|
| **Scope** | **Instance level** (attached to ENI) | **Subnet level** (all instances in subnet) |
| **State** | **STATEFUL** ✅ | **STATELESS** ❌ |
| **Return traffic** | **Automatic** ✅ | **Must explicitly allow** ❌ |
| **Rules** | ALLOW only | ALLOW + DENY |
| **Rule processing** | All rules evaluated | Processed in order (lowest # first) |
| **Default** | Deny inbound, allow outbound | Allow all |
| **Association** | Multiple per instance | One per subnet |

### What Does "Stateful" Mean?

**Security Groups (STATEFUL):**
```
You allow inbound port 80 HTTP
→ Outbound response is AUTOMATICALLY allowed
→ No need to configure outbound rule!
```

**Example:**
- Inbound rule: Allow port 80 from 0.0.0.0/0
- User requests web page on port 80
- Response automatically goes back (no outbound rule needed!)

### What Does "Stateless" Mean?

**NACLs (STATELESS):**
```
You allow inbound port 80 HTTP
→ Outbound response is NOT automatic
→ MUST add outbound rule for ephemeral ports!
```

**Example (you learned this Day 1!):**
- Inbound rule: Allow port 80 from 0.0.0.0/0
- User requests web page on port 80
- **MUST ALSO allow:** Outbound 1024-65535 to 0.0.0.0/0 (ephemeral ports for response!)

### Ephemeral Ports (Critical for NACLs)

**What are ephemeral ports?**
- Temporary ports used by clients for return traffic
- Range: **1024-65535**
- Example: Your browser uses port 51234 to receive the response

**NACL rule for web server (Day 1 pattern!):**
```
Inbound Rules:
- Rule 100: Allow TCP 80 from 0.0.0.0/0 (HTTP requests)
- Rule 110: Allow TCP 443 from 0.0.0.0/0 (HTTPS requests)
- Rule 120: Allow TCP 1024-65535 from 0.0.0.0/0 (return traffic!)

Outbound Rules:
- Rule 100: Allow TCP 80 to 0.0.0.0/0 (if instance makes HTTP requests)
- Rule 110: Allow TCP 443 to 0.0.0.0/0 (if instance makes HTTPS requests)
- Rule 120: Allow TCP 1024-65535 to 0.0.0.0/0 (responses to clients!)
```

### When to Use Each

**Security Groups (default):**
- ✅ Primary security control
- ✅ Instance-specific rules
- ✅ Most use cases

**NACLs (additional layer):**
- ✅ Subnet-level control
- ✅ **DENY specific IPs** (Security Groups can't deny!)
- ✅ Compliance requirements (defense in depth)
- ✅ DDoS protection (block malicious IPs)

**Exam pattern:** "Block specific IP address" → **NACL** (only NACLs have deny rules!)

---

## Part 3: VPC Endpoints (YOU LEARNED THIS DAY 1!)

Remember your Day 2 Quiz Q13? You got it right! Let's expand.

### Gateway Endpoint vs Interface Endpoint

| Feature | Gateway Endpoint | Interface Endpoint |
|---------|------------------|-------------------|
| **Services** | **S3, DynamoDB ONLY** | Most other AWS services |
| **How it works** | Route table entry | ENI (Elastic Network Interface) in subnet |
| **IP address** | ❌ No | ✅ Yes (private IP) |
| **Security Group** | ❌ No | ✅ Yes |
| **Cost** | **FREE** ✅ | $$$ ($/hour + $/GB) |
| **DNS** | Uses service DNS (s3.amazonaws.com) | Private DNS name |
| **Subnet** | VPC-wide (route table) | Per subnet (ENI) |

### Gateway Endpoint (FREE!)

**Services:** S3 and DynamoDB

**How it works:**
1. Create Gateway Endpoint
2. AWS updates route table: `s3-prefix-list → vpce-id`
3. Traffic to S3/DynamoDB goes through endpoint (private)
4. No internet, no NAT Gateway needed

**Use case:** EC2 → S3 without internet (FREE!)

**Example:**
```
Before Gateway Endpoint:
EC2 (private subnet) → NAT Gateway → Internet Gateway → S3
Cost: NAT Gateway charges

After Gateway Endpoint:
EC2 (private subnet) → Gateway Endpoint → S3
Cost: FREE!
```

### Interface Endpoint (Powered by PrivateLink)

**Services:** EC2, SNS, SQS, CloudWatch, Kinesis, and 100+ others

**How it works:**
1. Create Interface Endpoint in subnet
2. AWS creates ENI with private IP
3. Attach Security Group to ENI
4. Access service via private IP

**Use case:** Private access to AWS services (not S3/DynamoDB)

**Cost:** $/hour per endpoint + $/GB data processed

### Decision Tree

```
Need private access to AWS service from VPC?
├─ Service is S3 or DynamoDB?
│  └─ YES → Gateway Endpoint (FREE!)
│
└─ Other service (SNS, SQS, etc.)?
   └─ YES → Interface Endpoint (costs money)
```

**Exam pattern (you know this!):**
- "Private access to S3" → **Gateway Endpoint (FREE)**
- "Private access to SNS" → **Interface Endpoint (costs money)**

---

## Part 4: VPC Connectivity

### VPC Peering

**What:** Connect two VPCs privately (like a virtual network cable)

**Key facts:**
- ✅ **Non-transitive:** A↔B, B↔C does NOT mean A↔C
- ✅ **CIDR cannot overlap:** 10.0.0.0/16 cannot peer with 10.0.0.0/24
- ✅ **Same or cross-region**
- ✅ **Same or different accounts**
- ❌ **No transitive routing!**

**Use case:** Simple 1:1 VPC connection

**Limitations:**
- Must update route tables on both sides
- Cannot route through peered VPC (not transitive)
- Maximum: 125 peering connections per VPC

**Exam trap:**
```
VPC A ↔ VPC B (peered)
VPC B ↔ VPC C (peered)
Can VPC A reach VPC C? NO! (not transitive)
```

---

### Transit Gateway

**What:** Central hub connecting multiple VPCs and on-premises networks

**Key facts:**
- ✅ **Transitive routing:** A→TGW→B→TGW→C (A can reach C!)
- ✅ **Scales:** Up to 5,000 VPCs per Transit Gateway
- ✅ **Centralized management:** One routing table
- ✅ **Cross-region:** Transit Gateway peering
- ✅ **Hub-and-spoke topology**

**Use case:** Complex multi-VPC architectures

**Example:**
```
        On-Premises
             |
        Transit Gateway
        /    |    |    \
     VPC1  VPC2  VPC3  VPC4

All VPCs can reach each other AND on-premises!
```

**VPC Peering vs Transit Gateway:**

| Scenario | VPC Peering | Transit Gateway |
|----------|-------------|----------------|
| **2-3 VPCs** | ✅ Simple, cheap | Overkill |
| **10+ VPCs** | ❌ Complex (n*(n-1)/2 connections!) | ✅ Scalable |
| **Transitive routing needed** | ❌ Not supported | ✅ Supported |
| **On-premises integration** | ❌ Each VPC needs VPN | ✅ Single connection |

**Exam pattern:** "Many VPCs" + "transitive routing" → **Transit Gateway**

---

### AWS PrivateLink

**What:** Expose services to thousands of VPCs without peering

**How it works:**
1. Service provider: Creates NLB in their VPC
2. Service provider: Creates VPC Endpoint Service
3. Consumer: Creates Interface Endpoint
4. Consumer accesses service via private IP (no peering!)

**Use case:**
- ✅ SaaS provider exposing service to customers
- ✅ Partner integration
- ✅ Thousands of VPCs need access to one service

**PrivateLink vs VPC Peering:**
- **PrivateLink:** Service exposure (consumers can't access full VPC)
- **VPC Peering:** Full network access (more permissive)

**Exam pattern:** "Expose service to 1000s of VPCs securely" → **PrivateLink**

---

## Part 5: Hybrid Connectivity

### VPN (Site-to-Site VPN)

**Components:**
- **Virtual Private Gateway (VGW):** VPN endpoint on AWS side (attached to VPC)
- **Customer Gateway (CGW):** VPN endpoint on customer side
- **Site-to-Site VPN:** Encrypted IPSec tunnel over internet

**Key specs:**
- **Bandwidth:** Up to 1.25 Gbps per tunnel
- **Redundancy:** 2 tunnels per connection (HA)
- **Encryption:** IPSec (encrypted over internet)
- **Setup time:** **Minutes** (fast!)
- **Cost:** $0.05/hour per connection + data transfer

**Use case:**
- ✅ Quick setup needed
- ✅ Temporary connection
- ✅ Cost-sensitive
- ✅ Bandwidth < 1.25 Gbps acceptable

---

### AWS Direct Connect

**What:** Dedicated private connection from on-premises to AWS (NOT over internet)

**Key specs:**
- **Bandwidth:** 1 Gbps, 10 Gbps, or 100 Gbps (dedicated) / 50 Mbps - 10 Gbps (hosted)
- **Latency:** **Consistent, predictable** (private connection)
- **Setup time:** **Weeks to months** (requires AWS + carrier coordination)
- **Cost:** $$$ (port hours + data transfer OUT)
- **HA:** Use VPN as backup, or 2 Direct Connect connections

**Virtual Interfaces (VIFs):**
- **Private VIF:** Access VPC resources (via VGW)
- **Public VIF:** Access AWS public services (S3, DynamoDB) without internet

**Use case:**
- ✅ High bandwidth required (>1.25 Gbps)
- ✅ Consistent latency needed
- ✅ Reduce bandwidth costs (cheaper per GB than internet)
- ✅ Compliance (private connection, not internet)

---

### VPN vs Direct Connect Decision Tree

```
Need connectivity to AWS from on-premises?
├─ Need it immediately / temporary?
│  └─ YES → VPN (setup in minutes)
│
├─ High bandwidth (>1 Gbps) or consistent latency?
│  └─ YES → Direct Connect (weeks to setup)
│
├─ Cost-sensitive, <1 Gbps acceptable?
│  └─ YES → VPN (cheaper)
│
└─ Production workload, long-term?
   └─ YES → Direct Connect + VPN backup (HA)
```

**Exam keywords:**
- "Immediate" / "quick setup" → **VPN**
- "High bandwidth" / "consistent latency" / "reduce costs" → **Direct Connect**

---

## Part 6: Load Balancers

### ALB vs NLB vs GLB

You learned cross-zone load balancing costs in Day 1! Let's expand.

| Feature | ALB | NLB | GLB |
|---------|-----|-----|-----|
| **Layer** | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP) | Layer 3+4 (IP) |
| **Protocol** | HTTP, HTTPS, WebSocket | TCP, UDP, TLS | All IP packets |
| **Performance** | Good | **Ultra-high** (millions req/sec) | High |
| **Latency** | Moderate | **Ultra-low** (<100 µs) | Low |
| **Static IP** | ❌ No | ✅ Yes (one per AZ) | ✅ Yes |
| **Routing** | Path, host, header, query string | Port-based only | N/A |
| **SSL termination** | ✅ Yes | ✅ Yes | ❌ No |
| **Use case** | Web apps, microservices, containers | High performance, non-HTTP, static IP | 3rd-party security appliances |
| **Cross-zone cost** | **FREE** ✅ | **COSTS MONEY** 💰 | **COSTS MONEY** 💰 |

### Application Load Balancer (ALB)

**Best for:** HTTP/HTTPS applications, microservices, containers

**Routing capabilities:**
- **Path-based:** `/api` → Target Group 1, `/admin` → Target Group 2
- **Host-based:** `api.example.com` → TG1, `www.example.com` → TG2
- **Query string:** `?platform=mobile` → Mobile target group
- **Header-based:** `User-Agent: iOS` → iOS target group

**Targets:**
- EC2 instances
- ECS containers
- Lambda functions
- Private IP addresses

**Features:**
- ✅ Sticky sessions (cookie-based)
- ✅ SSL termination (offload SSL from backend)
- ✅ Authentication (Cognito, OIDC)
- ✅ Fixed hostname: `xxx.region.elb.amazonaws.com`

**Use case:** Modern web apps, microservices, path-based routing

---

### Network Load Balancer (NLB)

**Best for:** Extreme performance, non-HTTP protocols, static IP

**Performance:**
- **Millions of requests per second**
- **Ultra-low latency** (<100 microseconds)

**Features:**
- ✅ **Static IP:** One per AZ (can assign Elastic IP)
- ✅ **Preserve source IP:** Client IP preserved (not ALB behavior)
- ✅ **Protocols:** TCP, UDP, TLS
- ❌ No path-based routing (Layer 4 only)

**Use case:**
- ✅ Non-HTTP protocols (TCP, UDP)
- ✅ Static IP required (whitelisting)
- ✅ Extreme performance (gaming, IoT)

---

### Gateway Load Balancer (GLB)

**Best for:** Routing traffic through 3rd-party security appliances

**How it works:**
```
User → GLB → 3rd-party appliance (firewall/IDS) → GLB → Application
```

**Use case:**
- ✅ Inspect all traffic with 3rd-party tools
- ✅ Firewalls, intrusion detection, deep packet inspection

---

### Load Balancer Decision Tree

```
What protocol?
├─ HTTP/HTTPS?
│  ├─ Need path/host routing?
│  │  └─ YES → ALB
│  └─ Need extreme performance + HTTP?
│     └─ YES → NLB with HTTP listener
│
├─ TCP/UDP (non-HTTP)?
│  └─ YES → NLB
│
└─ Need to route through security appliances?
   └─ YES → GLB (Gateway Load Balancer)
```

**Exam patterns:**
- "Path-based routing" / "microservices" → **ALB**
- "Static IP" / "millions of req/sec" / "TCP application" → **NLB**
- "3rd-party firewall" / "inspect all traffic" → **GLB**

---

## Part 7: Route 53 Routing Policies

### The 7 Routing Policies

| Policy | Use Case | Key Behavior |
|--------|----------|--------------|
| **Simple** | Single resource | Returns all IPs, client chooses |
| **Weighted** | A/B testing, gradual rollout | Distribute by weight (%) |
| **Latency** | **Performance optimization** | Route to **lowest latency** region |
| **Failover** | Active-passive DR | Primary + secondary (health check) |
| **Geolocation** | **Data residency, localization** | Route by **user location** |
| **Geoproximity** | Bias traffic to regions | Location + bias value |
| **Multi-Value** | Simple load balancing | Multiple IPs (up to 8), health checks |

### Latency vs Geolocation (YOU LEARNED THIS DAY 1!)

Remember your retake quiz Q17? This was a weak spot!

**Critical distinction:**

| Routing Policy | Routes Based On | Use Case |
|---------------|----------------|----------|
| **Latency** | **Fastest/lowest latency** region | Performance, speed |
| **Geolocation** | **User's geographic location** | Data residency, compliance |

**Example scenario:**
- User in Germany
- Servers in: US (50ms), EU (100ms), Asia (200ms)

**Latency routing:** Routes to US (fastest)
**Geolocation routing:** Routes to EU (user's location)

**Exam pattern (Day 1 retake Q17!):**
- "Data residency" / "EU data in EU" → **Geolocation**
- "Lowest latency" / "best performance" → **Latency-based**

---

### Weighted Routing

**Use case:** A/B testing, gradual migration, canary deployments

**Example:**
- 90% traffic → Old version
- 10% traffic → New version

**How:** Assign weights (90 and 10), Route 53 distributes accordingly

---

### Failover Routing

**Use case:** Active-passive disaster recovery

**How:**
- Primary record (with health check)
- Secondary record (backup)
- If primary fails health check → Route to secondary

**Example:**
- Primary: us-east-1 (active)
- Secondary: us-west-2 (passive backup)

---

## Part 8: CloudFront

### What is CloudFront?

**CloudFront = Content Delivery Network (CDN)**
- Caches content at edge locations worldwide (~400 locations)
- Reduces latency (content closer to users)
- Reduces load on origin
- DDoS protection (AWS Shield)

### Origins

**What can be an origin?**
- S3 bucket
- Application Load Balancer
- EC2 instance
- Custom HTTP server

### Key Features

**Caching:**
- Content cached at edge based on TTL (Time To Live)
- Cache hit = fast response from edge
- Cache miss = fetch from origin, cache at edge

**Security:**
- **Signed URLs/Cookies:** Restrict access to premium content
- **Origin Access Identity (OAI):** Restrict S3 bucket to only CloudFront
- **Field-Level Encryption:** Encrypt specific fields at edge

**Edge Computing:**
- **Lambda@Edge:** Run code at edge (viewer request/response, origin request/response)
- **CloudFront Functions:** Lightweight functions (faster, cheaper)

### CloudFront vs Global Accelerator

| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Purpose** | **Cache content** at edge | Improve performance (**no caching**) |
| **Best for** | Static/dynamic HTTP content | TCP/UDP apps (gaming, VoIP, IoT) |
| **Caching** | ✅ Yes | ❌ No |
| **Protocols** | HTTP/HTTPS | TCP, UDP |
| **Static IP** | ❌ No | ✅ Yes (2 Anycast IPs) |
| **Use case** | Websites, APIs, video streaming | Real-time apps, non-HTTP protocols |

**Exam pattern:**
- "Cache website content globally" → **CloudFront**
- "Gaming application, global users, UDP" → **Global Accelerator**

---

## Part 9: Exam Patterns & Decision Trees

### Networking Decision Trees

#### 1. Internet Connectivity for Private Subnet

```
Private subnet needs to access internet?
├─ Outbound only (instances initiate)?
│  ├─ Production, HA required?
│  │  └─ NAT Gateway (per AZ for HA)
│  └─ Cost-sensitive, low traffic?
│     └─ NAT Instance
│
└─ Inbound + outbound (public-facing)?
   └─ Move to public subnet + Internet Gateway
```

#### 2. VPC Connectivity

```
Need to connect VPCs?
├─ 2-3 VPCs, simple connection?
│  └─ VPC Peering
│
├─ 10+ VPCs, need transitive routing?
│  └─ Transit Gateway
│
└─ Expose service to 1000s of VPCs?
   └─ PrivateLink
```

#### 3. Hybrid Connectivity

```
Connect on-premises to AWS?
├─ Need it immediately?
│  └─ VPN (setup in minutes)
│
├─ High bandwidth (>1 Gbps)?
│  └─ Direct Connect (weeks to setup)
│
├─ Production + HA?
│  └─ Direct Connect + VPN backup
│
└─ Temporary / cost-sensitive?
   └─ VPN
```

#### 4. Load Balancer Selection

```
What type of traffic?
├─ HTTP/HTTPS?
│  ├─ Need path/host routing?
│  │  └─ ALB
│  └─ Just load balancing?
│     └─ ALB or NLB
│
├─ TCP/UDP (non-HTTP)?
│  └─ NLB
│
├─ Need static IP?
│  └─ NLB
│
└─ Need to inspect with security appliance?
   └─ GLB
```

#### 5. Private AWS Service Access

```
Need private access to AWS service?
├─ Service is S3 or DynamoDB?
│  └─ Gateway Endpoint (FREE!)
│
└─ Other service (SNS, SQS, etc.)?
   └─ Interface Endpoint (costs money)
```

---

## Part 10: Exam Keyword Recognition

### Keywords → Solutions

| Keyword/Phrase | Solution |
|---------------|----------|
| "Block specific IP address" | NACL (deny rules) |
| "Stateless firewall" | NACL |
| "Stateful firewall" | Security Group |
| "Private access to S3" | S3 Gateway Endpoint |
| "Many VPCs, transitive routing" | Transit Gateway |
| "Expose service to 1000s of VPCs" | PrivateLink |
| "Immediate connectivity to AWS" | VPN |
| "High bandwidth, consistent latency" | Direct Connect |
| "Path-based routing" | ALB |
| "Static IP, millions req/sec" | NLB |
| "3rd-party security appliance" | GLB |
| "Data residency, EU users in EU" | Route 53 Geolocation |
| "Lowest latency routing" | Route 53 Latency-based |
| "A/B testing" | Route 53 Weighted |
| "Cache content globally" | CloudFront |
| "Gaming, global, UDP" | Global Accelerator |

---

## Part 11: Common Exam Traps

### Trap 1: VPC Peering Transitivity
**WRONG:** "VPC A peers with B, B peers with C, so A can reach C"
**RIGHT:** VPC Peering is NOT transitive. A cannot reach C!

### Trap 2: NAT Gateway HA
**WRONG:** "One NAT Gateway provides HA"
**RIGHT:** Need one NAT Gateway **per AZ** for HA

### Trap 3: Security Groups for Blocking IPs
**WRONG:** "Use Security Group to block malicious IP"
**RIGHT:** Security Groups have no deny rules! Use **NACL** to block IPs

### Trap 4: NACL Stateless Behavior
**WRONG:** "Allow inbound 80, outbound automatic"
**RIGHT:** NACLs are stateless! Must allow ephemeral ports 1024-65535 for return traffic

### Trap 5: /24 Subnet Has 256 IPs
**WRONG:** "/24 = 256 usable IPs"
**RIGHT:** AWS reserves 5 IPs, so 251 usable

### Trap 6: Direct Connect Setup Time
**WRONG:** "Use Direct Connect for immediate connectivity"
**RIGHT:** Direct Connect takes weeks/months. Use VPN for immediate!

### Trap 7: ALB Static IP
**WRONG:** "Use ALB for static IP requirement"
**RIGHT:** ALB has dynamic DNS. Use **NLB** for static IP!

### Trap 8: Geolocation vs Latency
**WRONG:** "Use Latency routing for data residency"
**RIGHT:** Use **Geolocation** for data residency (ensures EU users → EU region)

---

## Part 12: Quick Reference Checklist

### Before Day 3 Quiz, Ensure You Can Answer:

**VPC Fundamentals:**
- [ ] What makes a subnet public vs private?
- [ ] How many IPs are usable in a /24 subnet? (251, not 256!)
- [ ] What is 0.0.0.0/0 in a route table? (default route = all internet)

**Security:**
- [ ] Security Groups: Stateful or stateless? (STATEFUL)
- [ ] NACLs: Stateful or stateless? (STATELESS)
- [ ] Which can deny IPs? (NACL)
- [ ] What ports for NACL return traffic? (1024-65535 ephemeral)

**Connectivity:**
- [ ] NAT Gateway vs NAT Instance? (Gateway = managed/HA, Instance = cheaper)
- [ ] VPC Peering: Transitive? (NO!)
- [ ] Transit Gateway: Transitive? (YES!)
- [ ] VPN vs Direct Connect? (VPN = quick/cheap, DC = high bandwidth/consistent)

**Endpoints:**
- [ ] Gateway Endpoint: Which services? (S3, DynamoDB - FREE!)
- [ ] Interface Endpoint: Which services? (Others - costs money)

**Load Balancers:**
- [ ] ALB: Which layer? (Layer 7 - HTTP/HTTPS)
- [ ] NLB: Which layer? (Layer 4 - TCP/UDP)
- [ ] Which LB has static IP? (NLB)
- [ ] Which LB has FREE cross-zone? (ALB)

**Route 53:**
- [ ] Geolocation vs Latency? (Geo = user location, Latency = fastest)
- [ ] A/B testing? (Weighted)
- [ ] DR failover? (Failover policy)

---

## Summary: Critical Patterns

### The Big 5 Networking Patterns

1. **Private AWS Service Access:**
   - S3/DynamoDB → Gateway Endpoint (FREE)
   - Other services → Interface Endpoint (costs money)

2. **Hybrid Connectivity:**
   - Immediate/temporary → VPN
   - High bandwidth/long-term → Direct Connect

3. **VPC Connectivity:**
   - 2-3 VPCs → VPC Peering
   - Many VPCs + transitive → Transit Gateway
   - Service exposure → PrivateLink

4. **Load Balancer Selection:**
   - HTTP + routing → ALB
   - TCP/UDP or static IP → NLB
   - Security appliances → GLB

5. **Route 53 Routing:**
   - Data residency → Geolocation
   - Performance → Latency-based
   - A/B testing → Weighted

---

## You're Ready!

**What you already know from Days 1-2:**
- ✅ NACL stateless behavior (ephemeral ports)
- ✅ VPC Endpoint costs (Gateway = free, Interface = $$)
- ✅ Load balancer cross-zone costs (ALB = free, NLB = $$)
- ✅ Route 53 Geolocation vs Latency

**This deep dive added:**
- ✅ VPC fundamentals (CIDR, subnets, reserved IPs)
- ✅ NAT Gateway vs NAT Instance decision criteria
- ✅ VPC connectivity options (Peering, Transit Gateway, PrivateLink)
- ✅ Hybrid connectivity (VPN vs Direct Connect)
- ✅ Load balancer selection (ALB vs NLB vs GLB)

**You're ready for the Day 3 quiz!** 🎯

---

**Next:** Take the 10-question Day 3 VPC & Networking quiz (target 8/10 - 80%)
