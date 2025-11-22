# Networking & Content Delivery Quick Reference

## VPC (Virtual Private Cloud)

### Core Components
| Component | Purpose | Key Facts |
|-----------|---------|-----------|
| **VPC** | Isolated virtual network | Max /16 (65,536 IPs), Min /28 (16 IPs) |
| **Subnet** | Range of IPs in VPC | Tied to single AZ, 5 IPs reserved per subnet |
| **Internet Gateway (IGW)** | Connect VPC to internet | One per VPC, horizontally scaled, HA |
| **NAT Gateway** | Private subnet → internet (outbound) | Managed, 45 Gbps, per AZ, $$$ |
| **NAT Instance** | EC2 as NAT | Self-managed, cheaper, single point of failure |
| **Route Table** | Routing rules for subnets | Each subnet has one route table |

### Reserved IPs in Subnet (Example: 10.0.0.0/24)
- **10.0.0.0**: Network address
- **10.0.0.1**: VPC router
- **10.0.0.2**: DNS server (Route 53 Resolver)
- **10.0.0.3**: Reserved for future use
- **10.0.0.255**: Broadcast (not used in VPC but reserved)
- **Usable IPs**: 251 out of 256

### Public vs Private Subnet
| Feature | Public Subnet | Private Subnet |
|---------|---------------|----------------|
| **Internet access** | Via Internet Gateway | Via NAT Gateway/Instance |
| **Public IP** | Auto-assign or Elastic IP | No |
| **Route table** | 0.0.0.0/0 → IGW | 0.0.0.0/0 → NAT Gateway |
| **Use case** | Web servers, load balancers | Databases, app servers |

---

## Security Groups vs NACLs

### Comparison Table (CRITICAL TO KNOW!)
| Feature | Security Group | NACL (Network ACL) |
|---------|----------------|---------------------|
| **Scope** | Instance level | Subnet level |
| **State** | **Stateful** (return traffic auto-allowed) | **Stateless** (must allow both inbound/outbound) |
| **Rules** | Allow rules only | Allow AND Deny rules |
| **Rule processing** | All rules evaluated | Rules processed in order (lowest number first) |
| **Default** | Deny all inbound, allow all outbound | Allow all inbound/outbound |
| **Association** | Multiple SGs per instance | One NACL per subnet |
| **Use case** | Instance-specific security | Subnet-level security, explicit deny |

### Best Practices
- **Security Groups**: First line of defense, most granular control
- **NACLs**: Additional layer, block specific IPs or ranges (e.g., DDoS protection)
- **Defense in depth**: Use both SGs and NACLs together

---

## VPC Connectivity

### VPC Peering
- **What**: Connect two VPCs (private IPv4/IPv6)
- **Routing**: Non-transitive (A↔B, B↔C does NOT mean A↔C)
- **Region**: Same or cross-region
- **Account**: Same or different accounts
- **CIDR**: Cannot overlap
- **Use case**: Connect VPCs within AWS

### Transit Gateway
- **What**: Central hub connecting VPCs, on-premises networks
- **Routing**: Transitive (A→TGW→B→TGW→C = A can reach C)
- **Scale**: Up to 5,000 VPCs per Transit Gateway
- **Use case**: Complex multi-VPC architectures, hub-and-spoke topology
- **vs VPC Peering**: Transit Gateway for many VPCs, Peering for simple 1:1 connections

### VPC Endpoints
| Type | Protocol | Use Case | Example Services |
|------|----------|----------|------------------|
| **Gateway Endpoint** | Free, route table entry | S3, DynamoDB | S3, DynamoDB |
| **Interface Endpoint** | Costs $, ENI in subnet | Most AWS services | EC2, SNS, SQS, etc. |

**Key Points:**
- **Gateway Endpoint**: No IP address, update route table, free
- **Interface Endpoint**: Private IP (ENI), Security Group, costs per hour + data
- **Use case**: Access AWS services without internet/NAT gateway (secure, lower cost)

### AWS PrivateLink
- **What**: Private connectivity to services in other VPCs
- **How**: Service provider creates NLB → consumers create Interface Endpoint
- **Use case**: Expose services to 1000s of VPCs (SaaS), secure service access
- **vs VPC Peering**: PrivateLink = scalable service exposure, Peering = full network access

---

## Hybrid Connectivity

### VPN (Virtual Private Network)
| Component | Description | Specs |
|-----------|-------------|-------|
| **Virtual Private Gateway (VGW)** | VPN concentrator on AWS side | Attached to VPC |
| **Customer Gateway (CGW)** | VPN endpoint on customer side | Physical or software |
| **Site-to-Site VPN** | Encrypted connection over internet | Up to 1.25 Gbps per tunnel |

**Features:**
- **Redundancy**: 2 tunnels per connection (HA)
- **Encryption**: IPSec
- **Cost**: $0.05/hr per VPN connection + data transfer
- **Setup time**: Minutes (fast)

### Direct Connect
- **What**: Dedicated private connection (1 Gbps or 10 Gbps)
- **Benefits**:
  - Lower latency (predictable network)
  - Reduced bandwidth costs
  - Private connection (not over internet)
- **Setup time**: Weeks to months (AWS + carrier coordination)
- **HA**: Use VPN as backup, or 2 Direct Connect connections
- **Virtual Interfaces**:
  - **Private VIF**: Access VPC via VGW
  - **Public VIF**: Access AWS public services (S3, DynamoDB) without internet

### VPN vs Direct Connect
| Feature | VPN | Direct Connect |
|---------|-----|----------------|
| **Cost** | $ | $$$ |
| **Bandwidth** | Up to 1.25 Gbps | 1-100 Gbps |
| **Latency** | Variable (internet) | Consistent (private) |
| **Setup time** | Minutes | Weeks/months |
| **Use case** | Quick setup, cost-sensitive | High bandwidth, consistent latency |

**Exam Tip**: For "immediate" or "temporary" connectivity → **VPN**. For "high bandwidth" or "consistent latency" → **Direct Connect**.

---

## Load Balancers

### Types (MUST KNOW DIFFERENCES!)
| Type | Layer | Protocol | Use Case |
|------|-------|----------|----------|
| **ALB** (Application) | Layer 7 (HTTP/HTTPS) | HTTP, HTTPS, WebSocket | Web apps, microservices, path/host routing |
| **NLB** (Network) | Layer 4 (TCP/UDP) | TCP, UDP, TLS | Ultra-low latency, static IP, millions of requests/sec |
| **GLB** (Gateway) | Layer 3+4 | All IP packets | 3rd-party security appliances (firewall, IDS/IPS) |
| **CLB** (Classic) | Layer 4/7 | HTTP, HTTPS, TCP, SSL | Legacy (avoid on exam unless explicitly mentioned) |

### Application Load Balancer (ALB)
**Features:**
- **Routing**: Path-based (/api, /admin), host-based (api.example.com)
- **Targets**: EC2, ECS, Lambda, IP addresses
- **Sticky sessions**: Cookie-based
- **SSL termination**: Offload SSL from backend
- **Authentication**: Cognito, OIDC
- **Fixed hostname**: xxx.region.elb.amazonaws.com
- **Use case**: HTTP/HTTPS apps, microservices, containers

### Network Load Balancer (NLB)
**Features:**
- **Performance**: Millions of requests/sec, ultra-low latency (<100 µs)
- **Static IP**: One per AZ (good for whitelisting)
- **Elastic IP**: Can assign EIP
- **Protocols**: TCP, UDP, TLS
- **Use case**: Extreme performance, non-HTTP protocols, static IP requirement

### Gateway Load Balancer (GLB)
**Features:**
- **Function**: Routes traffic to 3rd-party appliances (firewalls, IDS/IPS)
- **Protocol**: GENEVE on port 6081
- **Use case**: Inspect all traffic with 3rd-party security appliances

### Key Features (All LBs)
- **Health checks**: Monitor target health
- **Cross-Zone Load Balancing**:
  - ALB: Always enabled (no extra charge)
  - NLB: Disabled by default (extra charge if enabled)
  - CLB: Disabled by default (no extra charge if enabled)
- **Connection Draining** (CLB) / **Deregistration Delay** (ALB/NLB): Wait time before terminating connections (0-3600 seconds)

---

## Route 53

### Routing Policies
| Policy | Use Case | How It Works |
|--------|----------|--------------|
| **Simple** | Single resource, no health checks | Returns all IPs, client picks one |
| **Weighted** | A/B testing, gradual migration | Distribute traffic by weight (%) |
| **Latency** | Performance optimization | Route to lowest latency region |
| **Failover** | Active-passive DR | Primary + secondary (health check) |
| **Geolocation** | Content localization, restrictions | Route based on user location |
| **Geoproximity** | Bias traffic to specific regions | Route based on location + bias |
| **Multi-Value** | Simple load balancing | Returns multiple IPs (up to 8), health checks |

### Health Checks
- Monitor endpoints (HTTP, HTTPS, TCP)
- Can monitor CloudWatch alarms or other health checks (calculated health checks)
- 30-second interval (can set to 10 seconds for faster failover, costs more)
- **Failover**: If health check fails, Route 53 routes to secondary

### Key Concepts
- **Alias Record**: Route traffic to AWS resources (ELB, CloudFront, S3, etc.) - No charge for queries
- **CNAME**: Route to another domain (can't use for zone apex, e.g., example.com)
- **A Record**: Map domain to IPv4
- **AAAA Record**: Map domain to IPv6
- **Zone Apex**: example.com (not www.example.com) - can only use A or Alias records

---

## CloudFront (CDN)

### Overview
- **What**: Content Delivery Network (edge locations worldwide)
- **Origin**: S3, ALB, EC2, custom HTTP server
- **Benefits**: Low latency, DDoS protection (Shield), reduced load on origin
- **Cache**: Cached at edge locations based on TTL

### Key Features
| Feature | Purpose |
|---------|---------|
| **Edge Locations** | Cache content close to users (~400 globally) |
| **Regional Edge Cache** | Intermediate cache between edge and origin |
| **Origin Shield** | Additional caching layer (reduce load on origin) |
| **Signed URLs/Cookies** | Restrict access (e.g., premium content) |
| **Field-Level Encryption** | Encrypt specific fields at edge |
| **Lambda@Edge** | Run code at edge locations |
| **CloudFront Functions** | Lightweight functions at edge (faster, cheaper than Lambda@Edge) |

### CloudFront vs Global Accelerator
| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Purpose** | Cache content at edge | Improve global performance (no caching) |
| **Use case** | Static/dynamic content delivery | TCP/UDP apps (gaming, IoT, VoIP) |
| **Caching** | Yes | No |
| **Protocols** | HTTP/HTTPS | TCP, UDP |
| **Static IP** | No | Yes (2 Anycast IPs) |

---

## API Gateway

### Overview
- **What**: Fully managed service to create, publish, maintain APIs
- **Types**: REST API, HTTP API, WebSocket API
- **Integration**: Lambda, HTTP endpoints, AWS services

### Features
- **Throttling**: Prevent overload (10,000 req/sec, burst 5,000)
- **Caching**: Cache responses (reduce backend calls)
- **Authentication**: IAM, Cognito, Lambda authorizers
- **Usage Plans**: API keys, throttling quotas for customers
- **Stages**: Dev, test, prod environments
- **CORS**: Enable for cross-origin requests

### REST API vs HTTP API
| Feature | REST API | HTTP API |
|---------|----------|----------|
| **Price** | $$$ | $ (70% cheaper) |
| **Features** | Full features (usage plans, API keys, caching) | Core features only |
| **Use case** | Complex APIs, caching, usage plans | Simple, cost-effective APIs |

---

## Exam Pattern Recognition

### "Low latency, global application" → **CloudFront** (caching) or **Global Accelerator** (no caching)
- Static content: **CloudFront**
- TCP/UDP apps: **Global Accelerator**

### "Route based on URL path or hostname" → **Application Load Balancer**

### "Extreme performance, millions req/sec, static IP" → **Network Load Balancer**

### "Private connection to AWS, no internet" → **Direct Connect** or **VPN**
- High bandwidth, consistent latency: **Direct Connect**
- Quick setup, lower cost: **VPN**

### "Multiple VPCs, transitive routing" → **Transit Gateway**

### "Private access to S3 from VPC" → **S3 Gateway Endpoint** (free)

### "Allow specific IP, deny others" → **NACL** (can create deny rules, Security Groups cannot)

### "Stateful vs stateless firewall" → **Security Group** (stateful) vs **NACL** (stateless)
