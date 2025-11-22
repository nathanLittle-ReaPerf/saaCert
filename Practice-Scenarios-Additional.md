# SAA-C03 Additional Practice Scenarios

These 10 additional scenarios cover gaps in your original practice materials: hybrid architectures, multi-account strategies, serverless patterns, analytics pipelines, and container orchestration.

---

## Scenario 9: Hybrid Cloud Active Directory Integration
**Difficulty: Hard**

A financial services company has an on-premises Microsoft Active Directory with 5,000 users. They're migrating applications to AWS and need employees to use the same credentials for both on-premises and AWS resources. The solution must support:

**Requirements:**
- Single sign-on (SSO) to AWS Management Console and applications
- On-premises AD remains the source of truth
- Support for EC2 instances to join the domain
- Minimal latency for authentication
- Support for both on-premises and AWS applications

**Question:** Which solution meets these requirements with the LEAST operational overhead?

**A)** Deploy AWS Managed Microsoft AD in AWS. Set up a two-way forest trust between on-prem AD and AWS Managed Microsoft AD. Use AWS IAM Identity Center (AWS SSO) for SSO to AWS accounts and applications.

**B)** Deploy AD Connector in AWS pointing to on-prem AD. Use AWS IAM Identity Center with AD Connector for SSO. Enable seamless domain join for EC2 instances.

**C)** Replicate on-prem AD to EC2 using native AD replication. Set up SAML 2.0 federation between AD and AWS IAM. Use IAM roles for application access.

**D)** Migrate on-prem AD to AWS Managed Microsoft AD. Use AWS IAM Identity Center for SSO. Configure VPN for on-prem resources to access AWS AD.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **AD Connector**: Proxy service that forwards authentication requests to on-prem AD (AD remains source of truth)
- **No replication**: No need to sync users/passwords to AWS (minimal overhead)
- **AWS IAM Identity Center**: Provides SSO to AWS Console, accounts, and SAML applications
- **Seamless domain join**: EC2 instances can join on-prem domain via AD Connector
- **Low latency**: AD Connector deployed in AWS VPC, fast authentication to on-prem via Direct Connect/VPN
- **LEAST operational overhead**: No AD infrastructure to manage in AWS, just proxy

**Why others are wrong:**
- **A:**
  - Two-way forest trust requires managing TWO AD forests (on-prem + AWS)
  - More operational overhead (replication, trust management, dual infrastructure)
  - Violates "on-prem AD remains source of truth" if users can be created in either forest
  - Use this when you need AWS apps to work even if on-prem AD is down (HA requirement not mentioned)
- **C:**
  - Running AD on EC2 = self-managed (high operational overhead: patches, backups, HA)
  - SAML 2.0 federation with IAM is older approach, more complex than IAM Identity Center
  - Violates "LEAST operational overhead"
- **D:**
  - Migrating AD to AWS makes AWS the source of truth, not on-prem (violates requirement)
  - On-prem resources would need to authenticate to AWS (latency, connectivity dependency)
  - Backwards from the requirement

**AD Service Comparison:**
| Service | Use Case | On-Prem AD Required? | Operational Overhead |
|---------|----------|---------------------|---------------------|
| **AD Connector** | Proxy to on-prem AD, SSO, domain join | Yes | Minimal (AWS managed) |
| **AWS Managed Microsoft AD** | Standalone AD in AWS or trust with on-prem | No (can standalone) | Low (AWS managed) |
| **Simple AD** | Small, standalone AD (Samba-based) | No | Low (AWS managed) |
| **AD on EC2** | Full control, custom configs | No | High (self-managed) |

**Key Exam Tips:**
- **"On-prem AD is source of truth"** → **AD Connector** (proxy, no replication)
- **"Need AD in AWS even if on-prem fails"** → **AWS Managed Microsoft AD with trust**
- **"Small workloads, no on-prem AD"** → **Simple AD**
- **AWS IAM Identity Center (AWS SSO)** = modern way for SSO (replaces SAML 2.0 federation)
</details>

---

## Scenario 10: Multi-Account Cost Allocation and Governance
**Difficulty: Medium**

A company uses AWS Organizations with 50 AWS accounts across Dev, Test, and Prod environments. They need to:

**Requirements:**
- Prevent developers from launching expensive instance types (>$1/hour)
- Block all accounts from deploying resources outside us-east-1 and eu-west-1
- Allocate costs to business units (Engineering, Sales, Marketing)
- Require all EC2 instances to be tagged with "Environment" and "Owner"
- Centralized billing with volume discounts

**Question:** Which combination of solutions meets these requirements?

**A)** Use IAM policies in each account to restrict instance types and regions. Create Cost Allocation Tags for Environment and Owner. Use AWS Budgets to track costs by business unit. Enable consolidated billing in Organizations.

**B)** Create Service Control Policies (SCPs) to deny expensive instance types and restrict regions to us-east-1/eu-west-1. Use Cost Allocation Tags for Environment and Owner. Create AWS Cost Categories for business units. Enable consolidated billing.

**C)** Use AWS Config rules to detect expensive instances and non-compliant regions. Create Cost Allocation Tags. Use Cost Explorer to group costs by tags. Enable consolidated billing. Use EventBridge to alert on violations.

**D)** Deploy CloudFormation StackSets to enforce instance type restrictions across all accounts. Use SCPs to restrict regions. Create Cost Allocation Tags. Use Cost Explorer with tag filters. Enable consolidated billing.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **Service Control Policies (SCPs)**:
  - Apply to ALL accounts in an OU (enforced guardrails)
  - Can deny specific instance types: `"Condition": {"StringEquals": {"ec2:InstanceType": ["t3.2xlarge", "m5.4xlarge"...]}}`
  - Can restrict regions: `"Condition": {"StringNotEquals": {"aws:RequestedRegion": ["us-east-1", "eu-west-1"]}}`
  - Cannot be bypassed (even by account admin)
- **Cost Allocation Tags**:
  - Tags applied to resources (Environment, Owner)
  - Activate in Billing console to track costs by tag
- **AWS Cost Categories**:
  - Organize costs into business units based on tags, accounts, or services
  - Example: "Engineering" category = all accounts in Engineering OU + resources tagged "Team:Engineering"
- **Consolidated billing**: Single bill, volume discounts aggregated across all accounts

**Why others are wrong:**
- **A:**
  - **IAM policies** apply per account, not organization-wide (need to deploy to 50 accounts)
  - Developers with admin can modify IAM policies in their account (not enforced)
  - AWS Budgets tracks costs but doesn't organize by business unit as cleanly as Cost Categories
  - Doesn't prevent violations, only detects after spending happens
- **C:**
  - **AWS Config** is detective (detects violations after they happen), not preventive
  - Doesn't PREVENT launching expensive instances, only alerts after launched
  - Violates "prevent" requirement (question wants to block, not just detect)
  - More complex and slower than SCPs
- **D:**
  - CloudFormation StackSets can deploy resources but not enforce runtime restrictions
  - Can't prevent users from launching instances outside CloudFormation
  - SCPs are correct for region restriction
  - Over-complicated solution

**Service Control Policies (SCPs) - Key Facts:**
- **Scope**: Organization-wide guardrails (applied to OUs or accounts)
- **Effect**: Maximum permissions (can only restrict, not grant)
- **Cannot bypass**: Even root user in member account can't override
- **Does NOT apply to**: Management account root user
- **Use cases**: Deny regions, deny expensive services, enforce tagging, compliance

**Example SCP (Deny Expensive Instances):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "ec2:RunInstances",
    "Resource": "arn:aws:ec2:*:*:instance/*",
    "Condition": {
      "StringEquals": {
        "ec2:InstanceType": [
          "m5.8xlarge", "m5.16xlarge", "m5.24xlarge",
          "c5.9xlarge", "c5.18xlarge", "p3.8xlarge"
        ]
      }
    }
  }]
}
```

**Key Exam Tips:**
- **Organization-wide enforcement** → **SCPs** (not IAM policies)
- **Prevent actions** → **SCPs** (preventive) vs **Config** (detective)
- **Cost allocation by business unit** → **Cost Categories** or **Cost Allocation Tags**
- **Consolidated billing** = volume discounts across all accounts in organization
</details>

---

## Scenario 11: Serverless Real-Time Analytics Pipeline
**Difficulty: Hard**

A social media company needs to process clickstream data from their mobile app in real-time. They receive 10,000 events per second with spikes to 50,000 events/sec during peak hours.

**Requirements:**
- Ingest events from mobile app (JSON format)
- Real-time processing: Detect trending topics within 10 seconds
- Store processed data for ad-hoc SQL queries (data analysts)
- Store raw events for compliance (7-year retention)
- Auto-scale to handle traffic spikes
- MOST cost-effective solution

**Question:** Which architecture meets these requirements?

**A)** Mobile app → API Gateway → Lambda → DynamoDB Streams → Lambda (processing) → DynamoDB (processed data). Use Athena to query DynamoDB. Store raw events in S3 Glacier Deep Archive.

**B)** Mobile app → Kinesis Data Streams → Lambda (processing) → S3 (processed data, Parquet format) → Athena (queries). Use Kinesis Firehose to store raw events in S3 with lifecycle policy to Glacier Deep Archive after 30 days.

**C)** Mobile app → API Gateway → SQS → Lambda (processing) → RDS (processed data) → QuickSight (queries). Use SQS to write raw events to S3 Standard, lifecycle to Glacier Deep Archive.

**D)** Mobile app → Application Load Balancer → EC2 Auto Scaling (processing) → Redshift (processed data) → Athena (queries). Write raw events to S3 Standard-IA, lifecycle to Glacier Deep Archive.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **Kinesis Data Streams**:
  - Designed for real-time streaming (perfect for 10K-50K events/sec)
  - Auto-scales with shards (1 shard = 1,000 records/sec write, 2 MB/sec)
  - Multiple consumers (Lambda + Firehose can both read from same stream)
- **Lambda processing**:
  - Serverless, auto-scales to match Kinesis throughput
  - Processes events in real-time (detect trending topics within seconds)
- **S3 + Parquet**:
  - Parquet = columnar format, optimized for analytics (10x faster queries, lower cost)
  - Serverless storage, unlimited scale
- **Athena**:
  - Query S3 data using SQL (perfect for ad-hoc analyst queries)
  - Serverless, pay per query
- **Kinesis Firehose**:
  - Automatically reads from Kinesis Data Streams
  - Loads raw events to S3 (batch delivery every 1-5 min or when buffer fills)
  - Applies compression and format conversion
  - Lifecycle policy: S3 Standard → Glacier Deep Archive (compliance, cheapest storage)
- **Cost-effective**: All serverless, pay for what you use

**Architecture Flow:**
```
Mobile App
    ↓
Kinesis Data Streams (ingest 10K-50K events/sec)
    ↓ (fan-out to 2 consumers)
    ├─→ Lambda → Process events → S3 (Parquet) → Athena (analyst queries)
    └─→ Kinesis Firehose → S3 (raw events) → Lifecycle → Glacier Deep Archive
```

**Why others are wrong:**
- **A:**
  - **API Gateway → Lambda → DynamoDB Streams**:
    - API Gateway has 10K req/sec limit (need to request increase for 50K)
    - Throttling during spikes (not ideal for real-time ingestion)
    - DynamoDB Streams is for reacting to DynamoDB changes, not for ingestion
  - **Athena can't query DynamoDB** directly (need to export to S3 or use DynamoDB connector for Athena)
  - Doesn't meet "ad-hoc SQL queries" cleanly
- **C:**
  - **SQS** is for decoupling, not streaming analytics
    - Standard SQS has no ordering, not ideal for real-time processing
    - FIFO SQS limited to 3,000 messages/sec (can't handle 50K spike)
  - **RDS** is relational database, overkill for analytics (Redshift or S3+Athena better)
  - **QuickSight** is visualization, not query engine (Athena is query engine, QuickSight visualizes results)
  - SQS doesn't "write raw events to S3" - would need Lambda to do that (more complex)
- **D:**
  - **ALB + EC2 Auto Scaling**:
    - Not serverless, need to manage EC2 instances (operational overhead)
    - Auto Scaling has lag time (not as fast as Lambda scaling)
  - **Redshift** is expensive for this use case (data warehouse, pay per hour for cluster)
    - Overkill for analysts doing ad-hoc queries (Athena cheaper and simpler)
  - **Athena can't query Redshift** directly (wrong architecture)

**Kinesis Family Decision Tree:**
| Service | Use Case | When to Use |
|---------|----------|-------------|
| **Kinesis Data Streams** | Real-time streaming, custom processing | Need to process events in real-time, multiple consumers, order matters |
| **Kinesis Firehose** | Load streams into S3/Redshift/OpenSearch | Simple ETL, just need to load data to destination, don't need custom processing |
| **Kinesis Data Analytics** | SQL queries on streams | Real-time analytics with SQL (not in this scenario) |

**Key Exam Tips:**
- **Real-time ingestion + processing** → **Kinesis Data Streams + Lambda**
- **Load streaming data to S3** → **Kinesis Firehose**
- **Ad-hoc SQL on S3** → **Athena**
- **7-year retention, cheapest** → **S3 Glacier Deep Archive**
- **Parquet/ORC** = columnar formats (faster queries, lower cost for analytics)
- **Multiple consumers from stream** → Kinesis Data Streams (fan-out)
</details>

---

## Scenario 12: Container Orchestration Decision
**Difficulty: Medium**

A startup is building a new microservices application with 20 services. The team has:
- No Kubernetes experience
- Limited DevOps resources (2 people)
- Services written in Node.js, Python, and Go
- Need auto-scaling based on CPU and custom metrics
- Tight AWS integration (S3, DynamoDB, RDS)

**Requirements:**
- Minimal operational overhead for container orchestration
- Auto-scaling for each service independently
- Integration with ALB for HTTP routing
- Secrets management for database credentials
- Cost-effective for startup (variable traffic)

**Question:** Which solution meets these requirements with the LEAST operational overhead?

**A)** Amazon ECS with Fargate launch type. Use ECS Service Auto Scaling with target tracking. Integrate with ALB for routing. Store secrets in AWS Secrets Manager, reference in task definitions.

**B)** Amazon EKS with Fargate launch type. Use Kubernetes Horizontal Pod Autoscaler (HPA) with metrics server. Deploy ALB Ingress Controller. Store secrets in AWS Secrets Manager with Secrets Store CSI driver.

**C)** Amazon ECS with EC2 launch type. Use ECS Service Auto Scaling and EC2 Auto Scaling. Integrate with ALB. Store secrets in Systems Manager Parameter Store, reference in task definitions.

**D)** Deploy Kubernetes on EC2 instances using kops. Use Horizontal Pod Autoscaler. Deploy nginx ingress controller. Store secrets in Kubernetes Secrets backed by etcd encryption.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Why A is correct:**
- **ECS with Fargate**:
  - **Serverless containers** (no servers to manage, AWS handles infrastructure)
  - **LEAST operational overhead** (no patching, scaling, or cluster management)
  - **No Kubernetes knowledge required** (simple compared to K8s)
- **ECS Service Auto Scaling**:
  - Auto-scale each service independently based on CPU, memory, or custom CloudWatch metrics
  - Target tracking policy (simplest, least overhead)
- **ALB integration**:
  - Native integration with ECS (path-based routing, host-based routing)
  - Each ECS service can be a separate ALB target group
- **Secrets Manager**:
  - Native integration with ECS task definitions (inject secrets as environment variables)
  - Automatic rotation for database credentials (reduce operational overhead)
- **Cost-effective**:
  - Fargate pay per vCPU/memory/second (no idle capacity)
  - Perfect for variable traffic (startup use case)

**Why others are wrong:**
- **B:**
  - **EKS (Kubernetes)** has steep learning curve (team has no K8s experience)
  - More operational overhead than ECS (even with Fargate):
    - Need to manage Kubernetes concepts (pods, deployments, services, ingress)
    - Need to deploy and manage ALB Ingress Controller (complex)
    - Need to configure Secrets Store CSI driver (more complex than ECS native integration)
  - Violates "LEAST operational overhead" and "no K8s experience"
  - **Only use EKS if**:
    - Team already knows Kubernetes
    - Need multi-cloud portability
    - Need advanced K8s features (operators, custom schedulers)
- **C:**
  - **ECS with EC2 launch type**:
    - Need to manage EC2 instances (patching, OS updates, AMI management)
    - Need to manage **two** auto-scaling groups (ECS tasks + EC2 instances)
    - More expensive (pay for EC2 instances even if containers not running)
    - More operational overhead than Fargate
- **D:**
  - **Self-managed Kubernetes on EC2**:
    - HIGHEST operational overhead (manage K8s control plane + worker nodes)
    - No K8s experience = disaster waiting to happen
    - Need to manage upgrades, etcd backups, control plane HA
    - Violates every requirement (LEAST overhead, no K8s experience)

**ECS vs EKS Decision Matrix:**
| Factor | Choose ECS | Choose EKS |
|--------|-----------|-----------|
| **Kubernetes experience** | None or basic | Team knows K8s |
| **Portability** | AWS only | Multi-cloud, on-prem |
| **Operational overhead** | Lower | Higher |
| **AWS integration** | Native, tight | Good (via controllers) |
| **Complexity** | Lower | Higher |
| **Use case** | AWS-native apps, simpler deployments | K8s requirement, complex orchestration |

**ECS Launch Types:**
| Launch Type | Description | Use Case | Operational Overhead |
|-------------|-------------|----------|---------------------|
| **Fargate** | Serverless, AWS manages infrastructure | Variable workloads, minimal overhead | Lowest |
| **EC2** | You manage EC2 instances | Reserved capacity, cost optimization | Medium |

**Key Exam Tips:**
- **"No Kubernetes experience"** or **"LEAST operational overhead"** → **ECS** (not EKS)
- **"Serverless containers"** or **"variable workload"** → **Fargate** (not EC2 launch type)
- **"Team already uses Kubernetes"** or **"multi-cloud"** → **EKS**
- **"Cost-effective for startup"** → Fargate (pay per use, no idle capacity)
- ECS native integration with Secrets Manager (simpler than K8s CSI drivers)
</details>

---

## Scenario 13: Data Lake Architecture with Compliance
**Difficulty: Hard**

A healthcare company needs to build a data lake for patient medical records and research data. They have:
- 500 TB of existing data in on-prem NAS (NFS)
- Daily ingestion of 5 TB new data
- Data analysts need to run SQL queries on data
- Compliance requires immutable audit logs of all data access
- Data must be encrypted at rest with company-managed keys
- PII (Personally Identifiable Information) must be automatically discovered and classified

**Question:** Which architecture meets all requirements?

**A)** Use AWS DataSync to migrate existing data to S3. Set up S3 Transfer Acceleration for daily ingestion. Enable S3 Versioning and Object Lock (Compliance Mode). Use S3 SSE-KMS with Customer Managed CMK. Use AWS Glue Crawler to catalog data. Query with Athena. Enable S3 Server Access Logging. Use Amazon Macie to discover PII.

**B)** Use Snowball Edge to migrate existing data to S3. Set up Storage Gateway (File Gateway) for daily ingestion. Enable S3 Versioning and MFA Delete. Use S3 SSE-S3 encryption. Use AWS Glue to catalog data and ETL. Query with Amazon Redshift Spectrum. Enable CloudTrail S3 data events. Use Amazon Comprehend Medical to detect PII.

**C)** Use AWS DataSync to migrate existing data to S3. Use DataSync for daily ingestion (scheduled). Enable S3 Versioning and Object Lock (Governance Mode). Use S3 SSE-KMS with Customer Managed CMK. Use AWS Lake Formation to catalog and manage permissions. Query with Athena. Enable CloudTrail S3 data events and S3 Server Access Logging. Use Amazon Macie to discover PII.

**D)** Use AWS Transfer Family (SFTP) to migrate data to S3. Enable S3 Versioning. Use S3 SSE-C (customer-provided keys). Use AWS Glue Crawler to catalog data. Query with EMR (Hive). Enable VPC Flow Logs and CloudWatch Logs. Manually tag PII data.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Why C is correct:**
- **DataSync**:
  - Migrate 500 TB from NFS to S3 (up to 10 Gbps per agent, automated)
  - Schedule daily ingestion (hourly, daily, weekly) - perfect for 5 TB/day
  - Data verification and encryption in transit
- **S3 Versioning + Object Lock (Governance Mode)**:
  - Versioning: Keep all versions of objects (audit trail, compliance)
  - Object Lock Governance: Immutability (can't delete until retention expires)
    - Governance Mode allows users with special permissions to override (vs Compliance Mode = nobody can delete)
    - Healthcare compliance often needs Governance (admins can remove if court-ordered)
- **SSE-KMS with Customer Managed CMK**:
  - Meets "company-managed keys" requirement
  - CloudTrail logs all key usage (audit trail for encryption operations)
- **AWS Lake Formation**:
  - Centralized data lake management (catalog, access control, auditing)
  - Fine-grained permissions (column-level, row-level security for PII)
  - Integrates with Glue Data Catalog
  - Audit logs of all data access (meets compliance)
- **Athena**:
  - Serverless SQL queries on S3 (perfect for analysts)
  - Pay per query, no infrastructure
- **CloudTrail S3 data events**:
  - Logs all S3 API calls (GetObject, PutObject, DeleteObject)
  - Immutable audit trail (compliance requirement)
- **S3 Server Access Logging**:
  - Additional access logs (who accessed what, when)
- **Amazon Macie**:
  - ML-based PII discovery (automatically scans S3 for SSN, credit cards, patient IDs)
  - Compliance reports for HIPAA, GDPR

**Why others are wrong:**
- **A:**
  - **S3 Transfer Acceleration**: For faster uploads over internet (not needed for DataSync)
  - **Object Lock Compliance Mode**: Cannot be overridden even with permissions
    - Too restrictive for healthcare (may need to delete for legal/patient requests)
    - Governance Mode is better (special permissions can override)
  - **S3 Server Access Logging** alone is insufficient (need CloudTrail data events for compliance)
  - Missing **Lake Formation** for centralized access control and auditing
- **B:**
  - **Snowball Edge**: Good for initial 500 TB migration (offline transfer)
  - **Storage Gateway**: Not ideal for daily 5 TB ingestion (DataSync better)
  - **SSE-S3**: Uses AWS-managed keys, NOT company-managed (violates requirement)
  - **Redshift Spectrum**: Queries S3 from Redshift (need Redshift cluster = expensive)
  - **Amazon Comprehend Medical**: Extracts medical entities (diagnoses, medications), NOT for PII classification (use Macie)
- **D:**
  - **Transfer Family (SFTP)**: For file transfers, not for NAS migration or bulk data ingestion
  - **SSE-C**: Customer provides encryption key with every request (operational burden, hard to manage)
  - **EMR (Hive)**: Big data processing, overkill for SQL queries (Athena simpler)
  - **VPC Flow Logs**: For network traffic, not data access auditing
  - **Manually tag PII**: Not scalable for 500 TB, violates "automatically discover"

**S3 Object Lock Modes:**
| Mode | Description | Can Delete? |
|------|-------------|-------------|
| **Governance** | Users with special permissions can override | Yes (with permission) |
| **Compliance** | Nobody can delete (not even root) until retention expires | No |

**Lake Formation Benefits:**
- Centralized data catalog (Glue Data Catalog)
- Fine-grained access control (column-level, row-level)
- Audit logging (who accessed what data, when)
- Data ingestion workflows (blueprint templates)
- Security (encrypted data lake, permission management)

**Key Exam Tips:**
- **"Company-managed encryption keys"** → **SSE-KMS with Customer Managed CMK**
- **"Immutable audit logs"** → **CloudTrail data events + S3 Versioning**
- **"Discover PII automatically"** → **Amazon Macie**
- **"Data lake, SQL queries"** → **S3 + Glue Catalog + Athena** (or Lake Formation)
- **"Healthcare/financial compliance"** → Often needs Lake Formation for fine-grained access control
- **"Migrate NFS to S3"** → **DataSync** (not Storage Gateway for migration)
</details>

---

## Scenario 14: Global Application with Low Latency
**Difficulty: Medium**

A gaming company serves users worldwide with latency-sensitive real-time gameplay. They need:

**Requirements:**
- Sub-100ms latency for users globally
- Static IP addresses (for whitelisting in enterprise firewalls)
- Automatic failover if region becomes unhealthy
- TCP/UDP support (game protocol)
- DDoS protection

**Question:** Which solution meets these requirements MOST cost-effectively?

**A)** Deploy game servers in 10 AWS regions. Use Amazon CloudFront with custom origin (game servers). Use Route 53 latency-based routing with health checks. Use AWS Shield Standard for DDoS protection.

**B)** Deploy game servers in 10 AWS regions. Use AWS Global Accelerator with endpoints in each region. Use health checks for automatic failover. Use AWS Shield Standard for DDoS protection. Optionally use Shield Advanced for enhanced protection.

**C)** Deploy game servers in 10 AWS regions behind Network Load Balancers. Use Route 53 latency-based routing with health checks. Assign Elastic IPs to NLBs. Use AWS WAF for DDoS protection.

**D)** Deploy game servers in edge locations using Lambda@Edge. Use CloudFront with Route 53 failover routing. Use Shield Advanced for DDoS protection.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **AWS Global Accelerator**:
  - **2 static Anycast IP addresses** (global, remain same even if endpoints change)
  - Routes traffic over **AWS global network** (not public internet)
    - Reduces latency by 60% vs internet routing
    - Consistent performance (avoids congested public internet paths)
  - **TCP/UDP support** (CloudFront is HTTP/HTTPS only)
  - **Automatic failover**: Health checks on endpoints, routes to healthy regions within 30 seconds
  - **DDoS protection**: Shield Standard included (Layer 3/4 DDoS mitigation)
- **Shield Standard**:
  - Free, automatic DDoS protection (Layer 3/4)
  - Sufficient for most use cases
- **Shield Advanced** (optional):
  - $3,000/month, 24/7 DDoS Response Team, cost protection
  - Only if company has budget and needs advanced protection

**Why others are wrong:**
- **A:**
  - **CloudFront**: HTTP/HTTPS only (game uses TCP/UDP)
  - CloudFront is CDN for caching static content, not for TCP/UDP gaming protocols
  - Violates TCP/UDP requirement
- **C:**
  - **Route 53 latency routing**:
    - Routes based on DNS queries (works, but less optimal than Global Accelerator)
    - DNS-based routing has limitations:
      - DNS caching (clients may cache old IPs, slower failover)
      - Public internet routing (higher latency than AWS backbone)
  - **Elastic IPs on NLB**: Static IPs ✓, but limited to one region per IP
    - Would need 10 different IPs (one per region), not 2 global static IPs
    - Clients would need to track multiple IPs or rely on DNS (defeats "static IP" benefit)
  - **AWS WAF**: Only works with ALB, API Gateway, CloudFront (NOT NLB)
    - WAF doesn't protect NLB
  - More expensive and complex than Global Accelerator
- **D:**
  - **Lambda@Edge**: Max 5 second timeout, not for long-running game sessions
  - Can't run game servers in Lambda (game servers are stateful, long-running)
  - Shield Advanced is expensive ($3,000/month), not "MOST cost-effective"

**CloudFront vs Global Accelerator:**
| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Purpose** | CDN (cache content at edge) | Improve global performance (no caching) |
| **Protocols** | HTTP/HTTPS | TCP, UDP |
| **Use Case** | Static/dynamic web content, video streaming | Gaming, IoT, VoIP, non-HTTP apps |
| **Static IP** | No | Yes (2 Anycast IPs) |
| **Caching** | Yes | No |
| **Routing** | Edge locations (cache) → origin | AWS edge → optimal AWS region |

**AWS Global Accelerator Benefits:**
1. **Static IPs**: 2 Anycast IPs (globally reachable, never change)
2. **Performance**: AWS global network (reduces latency, jitter)
3. **Availability**: Instant regional failover (30 seconds)
4. **DDoS protection**: Shield Standard included

**Key Exam Tips:**
- **"TCP/UDP, low latency, global"** → **Global Accelerator** (not CloudFront)
- **"Static IP addresses"** → **Global Accelerator** or **NLB with Elastic IP** (Global Accelerator better for global)
- **"HTTP/HTTPS, cache content"** → **CloudFront**
- **"Gaming, IoT, VoIP"** → **Global Accelerator** (non-HTTP protocols)
- **DDoS protection**: Shield Standard (free, automatic), Shield Advanced ($3K/mo, advanced)
</details>

---

## Scenario 15: Auto Scaling with Predictive and Reactive Policies
**Difficulty: Medium**

An e-commerce website experiences:
- Steady traffic: 100 requests/sec (requires 10 EC2 instances)
- Daily traffic spike at 6 PM: 500 requests/sec for 2 hours (requires 50 instances)
- Black Friday: 2,000 requests/sec for 12 hours (requires 200 instances)

**Requirements:**
- Handle traffic spikes without lag (instances ready before spike)
- Scale down after spikes to minimize costs
- Handle unexpected traffic bursts
- Minimize operational overhead

**Question:** Which Auto Scaling configuration meets these requirements MOST effectively?

**A)** Use Scheduled Scaling to add 40 instances at 5:45 PM daily and remove at 8 PM. Use Step Scaling for unexpected bursts. Set desired capacity to 10.

**B)** Use Predictive Scaling (ML-based) for daily pattern. Use Target Tracking Scaling (70% CPU) for reactive scaling. Set minimum 10, maximum 200 instances.

**C)** Use Scheduled Scaling for daily 6 PM spike and Black Friday. Use Simple Scaling based on CPU alarms. Set desired capacity to 50 (always ready).

**D)** Use only Target Tracking Scaling with 50% CPU target. Set minimum 10, maximum 200. Use CloudWatch alarms to notify when scaling occurs.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **Predictive Scaling (ML-based)**:
  - Analyzes historical traffic patterns (learns daily 6 PM spike)
  - **Proactively** scales up BEFORE spike (instances ready when traffic arrives)
  - No lag time (solves "instances ready before spike" requirement)
  - Learns and adapts over time
- **Target Tracking Scaling (reactive)**:
  - Maintains target CPU (e.g., 70%)
  - Handles unexpected bursts (Black Friday traffic that doesn't match daily pattern)
  - Auto-adjusts capacity to maintain performance
  - **LEAST operational overhead** (set target, AWS handles the rest)
- **Min 10, Max 200**:
  - Min 10: Baseline capacity for steady traffic
  - Max 200: Safety cap for Black Friday traffic
  - Scales down automatically after spike (cost optimization)

**Scaling Flow:**
1. **Normal traffic**: 10 instances (minimum)
2. **Approaching 6 PM**: Predictive Scaling adds instances proactively (reaches 50 by 6 PM)
3. **6 PM spike**: Instances already running, no lag
4. **Unexpected burst**: Target Tracking scales up reactively (adds more if CPU exceeds 70%)
5. **After spike**: Scales down automatically to minimum

**Why others are wrong:**
- **A:**
  - **Scheduled Scaling**:
    - Works for predictable spikes (6 PM daily) ✓
    - But requires manual configuration for EVERY event
    - Black Friday needs separate schedule
    - Future traffic changes need manual updates (high operational overhead)
  - **Step Scaling**: More complex than Target Tracking (need to define steps)
  - No learning/adaptation (Predictive Scaling learns patterns)
- **C:**
  - **Desired capacity 50**: Wastes money (running 50 instances 24/7, even during 100 req/sec traffic)
  - **Simple Scaling**: Deprecated, use Step or Target Tracking instead
    - Simple Scaling has cooldown period (slow to react to bursts)
  - Violates "minimize costs" (always running excess capacity)
- **D:**
  - **Only Target Tracking**:
    - Reactive only (scales after CPU increases)
    - Will have lag during daily 6 PM spike (wait for CPU to rise → scale up → wait for instances to launch)
    - Doesn't meet "instances ready before spike" requirement
  - No proactive scaling for predictable patterns

**Auto Scaling Policies Comparison:**
| Policy | Type | Use Case | Operational Overhead |
|--------|------|----------|---------------------|
| **Target Tracking** | Reactive | Maintain target metric (CPU, requests) | Lowest (set target, done) |
| **Step Scaling** | Reactive | Multiple thresholds, fine-grained control | Medium (define steps) |
| **Scheduled Scaling** | Proactive | Known traffic patterns, specific times | Medium (update schedules) |
| **Predictive Scaling** | Proactive (ML) | Recurring patterns, learns over time | Lowest (auto-learns) |

**Best Practice: Combine Policies**
- **Predictive Scaling**: Handle recurring patterns (daily, weekly spikes)
- **Target Tracking**: Handle unexpected bursts, maintain performance
- **Scheduled Scaling**: One-time events (Super Bowl, product launch)

**Key Exam Tips:**
- **"Recurring traffic pattern"** → **Predictive Scaling** (learns and adapts)
- **"Instances ready BEFORE spike"** → **Predictive** or **Scheduled Scaling** (proactive)
- **"Unexpected bursts"** → **Target Tracking** or **Step Scaling** (reactive)
- **"LEAST operational overhead"** → **Target Tracking** (simplest) or **Predictive** (auto-learns)
- **Combine policies**: Predictive + Target Tracking = proactive + reactive (best of both)
</details>

---

## Scenario 16: Cross-Region Failover for Mission-Critical App
**Difficulty: Hard**

A financial trading platform must be available 24/7 with:

**Requirements:**
- Primary region: us-east-1
- DR region: us-west-2
- RTO (Recovery Time Objective): 5 minutes
- RPO (Recovery Point Objective): 1 minute
- Automatic failover without manual intervention
- Database size: 2 TB (PostgreSQL)
- Application: Stateless containers

**Question:** Which architecture meets the RTO and RPO requirements with automatic failover?

**A)** Primary: Aurora PostgreSQL Multi-AZ in us-east-1, ECS Fargate with ALB. DR: Aurora Global Database (us-west-2 read replica), ECS Fargate with ALB. Route 53 health checks on primary ALB, failover routing to DR region.

**B)** Primary: RDS PostgreSQL Multi-AZ in us-east-1, ECS on EC2 with ALB. DR: RDS Read Replica in us-west-2, ECS on EC2 with ALB. Route 53 health checks, failover routing. In DR event, manually promote read replica.

**C)** Primary: Aurora PostgreSQL in us-east-1, ECS Fargate with ALB. DR: Nightly RDS snapshots copied to us-west-2. In DR event, restore from snapshot, launch ECS tasks. Use Route 53 failover routing.

**D)** Primary: Aurora PostgreSQL Multi-AZ in us-east-1, ECS Fargate with ALB. DR: Aurora Global Database (us-west-2 read replica), ECS Fargate scaled to 0 (launch on DR). Route 53 health checks, failover routing. Use Lambda to automatically promote Aurora replica and scale ECS on DR event.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: A**

**Why A is correct:**
- **Aurora Global Database**:
  - **Cross-region read replica** in us-west-2 (automatic, continuous replication)
  - **RPO < 1 second** (sub-second replication lag) - meets 1-minute requirement ✓
  - **Promotion time < 1 minute** (can be automated with RDS events + Lambda)
- **ECS Fargate running in DR region**:
  - Containers already running in us-west-2 (scaled down to minimal capacity, e.g., 2 tasks)
  - **Quick scale-up** (ECS Service Auto Scaling can increase to full capacity in 2-3 minutes)
- **Route 53 health checks + failover**:
  - Monitors primary ALB health (every 30 seconds)
  - **Automatic DNS failover** to DR region if primary fails (within 1-2 minutes)
  - No manual intervention required ✓
- **RTO ~5 minutes**:
  - Health check detects failure: 1 minute
  - DNS failover: 1 minute (TTL dependent)
  - Aurora promotion (automated): 1 minute
  - ECS scale-up: 2-3 minutes
  - Total: ~4-6 minutes ✓

**Architecture:**
```
Primary (us-east-1):
  Route 53 → ALB → ECS Fargate (full capacity) → Aurora Global DB (primary)

DR (us-west-2):
  Route 53 → ALB → ECS Fargate (minimal, 2 tasks) → Aurora Global DB (replica)

On Failover (automated):
  1. Route 53 health check fails → DNS switches to us-west-2
  2. EventBridge detects failure → Lambda promotes Aurora replica
  3. ECS Service Auto Scaling increases tasks to full capacity
```

**Why others are wrong:**
- **B:**
  - **RDS Read Replica**:
    - Replication lag typically 30 seconds - 5 minutes (acceptable for RPO)
    - BUT: **Promotion is MANUAL** ("manually promote read replica")
    - Violates "automatic failover without manual intervention"
  - **RTO**: Manual promotion could take 10-30 minutes (human reaction time + promotion time)
    - Violates 5-minute RTO
- **C:**
  - **Nightly snapshots**:
    - **RPO = 24 hours** (last snapshot could be up to 24 hours old)
    - Violates 1-minute RPO ❌
  - **Restore from snapshot**:
    - Takes 30+ minutes for 2 TB database
    - Violates 5-minute RTO ❌
  - This is **Backup & Restore** strategy (RPO hours, RTO hours/days) - wrong for mission-critical
- **D:**
  - **ECS Fargate scaled to 0** in DR:
    - Need to launch all containers from zero during DR event
    - Launching + pulling images + starting = 5-10 minutes (might exceed RTO)
    - Risky (what if container images fail to pull?)
  - **Lambda for automation**: Correct approach, but cold-starting all containers is risky
  - Option A is safer (containers already running, just scale up)

**DR Strategy Review:**
| Strategy | RPO | RTO | Matches Requirement? |
|----------|-----|-----|---------------------|
| **Backup & Restore** (C) | Hours | Hours/Days | ❌ (need RPO 1 min, RTO 5 min) |
| **Pilot Light** (B with manual) | Minutes | 10-30 min | ❌ (RTO too long, manual) |
| **Warm Standby** (A) | Seconds | 5-10 min | ✅ |
| **Multi-Site Active-Active** | Near-zero | Seconds | ✅ but expensive (not needed) |

**Aurora Global Database:**
- **Replication lag**: Typically <1 second
- **Promotion time**: <1 minute (can be automated)
- **Use case**: Cross-region DR, low RPO/RTO requirements
- **Cost**: Pay for replica instance + cross-region data transfer

**Key Exam Tips:**
- **RPO in seconds/minutes + RTO in minutes** → **Warm Standby** (Aurora Global DB + running resources in DR)
- **"Automatic failover"** → **Route 53 health checks + failover routing** + **automation (Lambda/EventBridge)**
- **Manual promotion** = longer RTO (humans take time)
- **Aurora Global Database** = best for low RPO/RTO cross-region DR
- **Containers in DR**: Keep minimal capacity running (faster scale-up than cold start)
</details>

---

## Scenario 17: Compliance and Auditing with Multi-Tier App
**Difficulty: Medium**

A SaaS company must comply with SOC 2 and PCI DSS. They have a 3-tier web application:
- Web tier: EC2 instances behind ALB
- App tier: Lambda functions
- Data tier: DynamoDB

**Requirements:**
- All API calls must be logged for audit (who did what, when)
- Detect configuration changes to resources (non-compliant changes)
- Alert when security groups are modified to allow 0.0.0.0/0 on port 22
- Centralized log storage for 7 years
- Detect if S3 buckets become publicly accessible
- Monthly compliance reports

**Question:** Which combination of services meets ALL compliance requirements?

**A)** Enable CloudTrail for API logging (store in S3). Use AWS Config to track resource changes. Create Config rule to detect open security groups. Use EventBridge to trigger SNS for alerts. Enable S3 Server Access Logging. Use Trusted Advisor for compliance reports.

**B)** Enable CloudTrail (store in S3 with lifecycle to Glacier). Use AWS Config to track resource changes. Create Config rules for security group violations and public S3 buckets. Use Config for automatic remediation (Lambda). Use Security Hub for centralized compliance reporting. Use EventBridge + SNS for alerts.

**C)** Enable VPC Flow Logs and CloudWatch Logs. Use CloudWatch Logs Insights to query API calls. Create CloudWatch alarms for security group changes. Use GuardDuty to detect public S3 buckets. Store logs in S3 Standard-IA with 7-year retention.

**D)** Enable CloudTrail (store in S3). Use AWS Config for change tracking. Use AWS Systems Manager OpsCenter to track compliance. Create EventBridge rules to detect security group changes. Use Macie to detect public S3 buckets. Use QuickSight for compliance reporting.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **CloudTrail**:
  - Logs ALL API calls (who did what, when) - meets audit requirement ✓
  - Store in S3 with lifecycle to Glacier (7-year retention, cost-effective) ✓
  - Immutable audit trail (required for SOC 2, PCI DSS)
- **AWS Config**:
  - Tracks resource configuration changes (security groups, S3 buckets, EC2, etc.) ✓
  - **Config Rules**: Evaluate compliance
    - Managed rule: `restricted-ssh` (detect 0.0.0.0/0 on port 22) ✓
    - Managed rule: `s3-bucket-public-read-prohibited` (detect public S3 buckets) ✓
  - **Config Timeline**: See configuration history for each resource
  - **Automatic remediation**: Lambda functions to fix non-compliant resources (e.g., remove public access)
- **Security Hub**:
  - **Centralized compliance dashboard** (aggregates findings from Config, GuardDuty, Inspector, etc.)
  - **Compliance standards**: Built-in support for PCI DSS, CIS AWS Foundations, etc.
  - **Monthly reports**: Export compliance status, findings ✓
- **EventBridge + SNS**:
  - Config sends findings to EventBridge
  - EventBridge triggers SNS for real-time alerts (e.g., email/SMS when SG modified) ✓

**Compliance Flow:**
```
API Call → CloudTrail → S3 (audit log)
Resource Change → Config → Evaluate Rules → Non-compliant?
                                               ├─ Yes → EventBridge → SNS (alert)
                                               │     ├→ Lambda (auto-remediate)
                                               │     └→ Security Hub (compliance dashboard)
                                               └─ No → Compliant (Config records in timeline)
```

**Why others are wrong:**
- **A:**
  - **S3 Server Access Logging**: Logs S3 bucket access, NOT API calls across all AWS services
    - Missing API logs for EC2, Lambda, DynamoDB, etc.
  - **Trusted Advisor**: Provides recommendations, NOT detailed compliance reports
    - Doesn't aggregate findings from Config, GuardDuty, etc.
    - Not designed for SOC 2 / PCI DSS compliance reporting
  - No lifecycle policy mentioned for CloudTrail logs (7-year retention unclear)
- **C:**
  - **VPC Flow Logs**: Network traffic logs (IP, port, protocol), NOT API calls
    - Doesn't log "who created S3 bucket" or "who modified security group"
  - **CloudWatch Logs Insights**: Query logs, but VPC Flow Logs don't contain API calls
  - **GuardDuty**: Threat detection, not configuration compliance
    - GuardDuty detects threats (compromised instances, crypto mining), not "public S3 bucket"
  - Missing Config for resource change tracking
  - Doesn't meet "API call logging" requirement
- **D:**
  - **Systems Manager OpsCenter**: Operational issue management, NOT compliance tracking
    - OpsCenter aggregates operational issues (alarms, events), not compliance findings
  - **Macie**: PII/sensitive data discovery in S3, NOT for detecting public S3 buckets
    - Macie finds data (SSN, credit cards), not misconfigurations
    - Config is correct for detecting public buckets
  - **QuickSight**: BI/visualization tool, not a compliance reporting platform
    - Would need to manually create dashboards, not built-in compliance reports like Security Hub

**AWS Config Managed Rules (Exam Favorites):**
| Rule | Purpose |
|------|---------|
| `restricted-ssh` | Detect security groups allowing 0.0.0.0/0 on port 22 |
| `restricted-common-ports` | Detect security groups allowing 0.0.0.0/0 on common ports (3389, 3306, etc.) |
| `s3-bucket-public-read-prohibited` | Detect publicly readable S3 buckets |
| `s3-bucket-public-write-prohibited` | Detect publicly writable S3 buckets |
| `encrypted-volumes` | Ensure EBS volumes are encrypted |
| `rds-encryption-enabled` | Ensure RDS instances are encrypted |
| `iam-password-policy` | Check IAM password policy |

**Security Hub Standards:**
- **CIS AWS Foundations Benchmark**: Security best practices
- **PCI DSS**: Payment Card Industry compliance
- **AWS Foundational Security Best Practices**: General security controls

**Key Exam Tips:**
- **"Audit all API calls"** → **CloudTrail**
- **"Track resource configuration changes"** → **Config**
- **"Detect non-compliant resources"** → **Config Rules**
- **"Compliance reporting (SOC 2, PCI DSS)"** → **Security Hub**
- **"Auto-remediate"** → **Config + Lambda** (via SSM Automation or custom Lambda)
- **"Alert on changes"** → **Config + EventBridge + SNS**
- **7-year log retention** → **S3 with lifecycle to Glacier/Deep Archive** (cost-effective)
</details>

---

## Scenario 18: Serverless Batch Processing with Cost Optimization
**Difficulty: Medium**

A media company processes video files uploaded by users:
- Users upload 1,000 videos/day (average 500 MB each)
- Processing takes 10 minutes per video (transcode, thumbnail, metadata extraction)
- Processing can happen within 24 hours (not real-time)
- Traffic varies: 100 uploads/day on weekdays, 3,000 uploads/day on weekends

**Requirements:**
- Serverless solution (no server management)
- Handle variable workloads (100-3,000 videos/day)
- MOST cost-effective solution
- Durability (no video loss)

**Question:** Which architecture is MOST cost-effective?

**A)** S3 (upload) → S3 event notification → Lambda (transcode) → S3 (processed). Use Lambda with 10 GB memory, 15-minute timeout. Pay per invocation.

**B)** S3 (upload) → S3 event notification → SQS → Lambda (read SQS, submit to AWS Batch) → AWS Batch (Docker containers on Spot Instances) → S3 (processed).

**C)** S3 (upload) → S3 event notification → Step Functions → ECS Fargate task (transcode) → S3 (processed). Use Fargate Spot for cost savings.

**D)** S3 (upload) → EventBridge → SQS → ECS on EC2 Auto Scaling (process queue) → S3 (processed). Use Spot Instances with Auto Scaling.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **S3 event notification → SQS**:
  - Decouples upload from processing (upload doesn't wait for processing)
  - SQS durability: No video loss (messages retained 4-14 days) ✓
  - Buffer: Handles variable load (100-3,000 videos)
- **Lambda reads SQS, submits to AWS Batch**:
  - Lambda lightweight (just submit job to Batch, <1 second)
  - Cheap (minimal Lambda invocations)
- **AWS Batch**:
  - Managed batch compute (Docker containers)
  - **Spot Instances**: Up to 90% discount vs On-Demand ✓
  - Automatically retries if Spot interrupted (fault-tolerant)
  - Scales from 0 (no jobs) to 100s of instances (weekend spike)
  - **Pay only when processing** (no idle capacity)
- **Cost breakdown**:
  - SQS: ~$0.50/month (1M requests free tier)
  - Lambda: <$1/month (minimal, just submit jobs)
  - Batch on Spot: ~$10-50/month (10 min processing × 1,000 videos × Spot discount)
  - S3 storage: Variable
  - **Total: ~$15-60/month** (cheapest option)

**Why others are wrong:**
- **A:**
  - **Lambda for transcoding**:
    - 10 minutes per video = 600 seconds per invocation
    - Lambda pricing: $0.0000166667 per GB-second
    - Cost: 10 GB × 600 sec × $0.0000166667 = **$0.10 per video**
    - For 1,000 videos/day: **$100/day = $3,000/month** 💸
  - **Lambda 15-min timeout**: Works, but EXPENSIVE for long-running CPU-intensive tasks
  - Violates "MOST cost-effective"
- **C:**
  - **Step Functions**:
    - Charges per state transition ($0.000025 per transition)
    - For 1,000 videos/day with multi-step workflow: adds cost
  - **Fargate Spot**:
    - More expensive than Batch on EC2 Spot
    - Fargate charges per vCPU/memory/second (minimum 1 minute billing)
    - Batch on Spot is cheaper for batch processing
  - Not as cost-effective as Batch on Spot
- **D:**
  - **ECS on EC2**:
    - Need to manage Auto Scaling (scale based on queue depth)
    - More operational overhead than Batch (AWS Batch manages scheduling automatically)
  - **Violates "serverless"**: EC2 instances are not serverless (need to manage AMIs, patching)
  - Batch is simpler and more serverless-like

**Lambda vs AWS Batch:**
| Factor | Lambda | AWS Batch |
|--------|--------|-----------|
| **Duration** | <15 minutes | Hours/days (no limit) |
| **Use Case** | Event-driven, short tasks | Long-running batch jobs |
| **Cost for long tasks** | Expensive (pay per GB-sec) | Cheap (Spot Instances) |
| **Scaling** | Automatic (1000 concurrent) | Automatic (scales to demand) |
| **Containers** | No (just code + layers) | Yes (Docker containers) |

**AWS Batch Benefits:**
- Managed batch compute (no server management)
- Automatically provisions optimal compute (CPU-optimized, memory-optimized)
- Spot Instance support (up to 90% savings)
- Job queues, priorities, dependencies
- Retry logic for failed jobs

**Key Exam Tips:**
- **"Long-running task (>15 min)"** → **AWS Batch** or **ECS/Fargate** (NOT Lambda)
- **"Batch processing, cost-effective"** → **AWS Batch on Spot Instances**
- **"Serverless, short task (<15 min)"** → **Lambda**
- **"Decouple upload from processing"** → **SQS** (buffer)
- **"Variable workload, don't lose data"** → **SQS** (durable queue)
- **Lambda cost scales with duration** (expensive for 10-min tasks)
</details>

---

## Summary: Key Patterns from Additional Scenarios

### Multi-Account & Governance (Scenario 10)
- **Organization-wide enforcement**: SCPs (not IAM policies)
- **Cost allocation**: Cost Categories, Cost Allocation Tags
- **Prevent vs Detect**: SCPs (prevent), Config (detect)

### Hybrid Cloud (Scenario 9)
- **On-prem AD integration**: AD Connector (proxy), AWS Managed AD (standalone or trust)
- **SSO to AWS**: IAM Identity Center (AWS SSO)

### Real-Time Analytics (Scenario 11)
- **Streaming ingestion**: Kinesis Data Streams
- **Load to S3**: Kinesis Firehose
- **Query S3**: Athena (ad-hoc), Redshift (data warehouse)

### Containers (Scenario 12)
- **No K8s experience**: ECS (not EKS)
- **Serverless containers**: Fargate
- **Least overhead**: ECS Fargate > ECS EC2 > EKS

### Data Lake (Scenario 13)
- **Centralized data lake**: Lake Formation
- **Discover PII**: Macie
- **Immutable logs**: CloudTrail data events, S3 Object Lock

### Global Low Latency (Scenario 14)
- **TCP/UDP, global**: Global Accelerator (not CloudFront)
- **Static IPs**: Global Accelerator (2 Anycast IPs)

### Auto Scaling (Scenario 15)
- **Recurring patterns**: Predictive Scaling (ML-based)
- **Unexpected bursts**: Target Tracking
- **Combine**: Predictive + Target Tracking

### Cross-Region DR (Scenario 16)
- **Low RPO/RTO**: Aurora Global Database, Warm Standby
- **Automatic failover**: Route 53 health checks + failover routing

### Compliance (Scenario 17)
- **API call logs**: CloudTrail
- **Resource changes**: Config
- **Compliance reporting**: Security Hub

### Batch Processing Cost (Scenario 18)
- **Long tasks, cost-effective**: AWS Batch on Spot
- **Short tasks**: Lambda
- **Decouple**: SQS

---

You now have 18 total practice scenarios. Keep grinding! 💪
