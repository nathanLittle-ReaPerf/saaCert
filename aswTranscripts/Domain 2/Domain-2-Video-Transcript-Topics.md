# Domain 2: Design Resilient Architectures - Video Transcript Topics

**Exam Date: March 2, 2026 at 5:15 PM EST**
**Created: February 10, 2026**

This document contains concise explanations of all topics covered in the Domain 2 video transcripts that you need to know for the AWS SAA-C03 exam.

---

## **SCALING FUNDAMENTALS**

### Vertical vs Horizontal Scaling

**Vertical Scaling:**
- Increase instance size (t2.micro → t2.large)
- Limited by instance maximums
- More expensive
- Requires downtime

**Horizontal Scaling:**
- Add more instances
- Uses Auto Scaling
- More cost-effective
- Elastic (matches demand)

### Elasticity

- Automation + horizontal scaling = capacity matches demand automatically
- Uses Launch Configurations/Templates + Auto Scaling
- Optimizes performance, operations, and cost

### EC2 Auto Scaling Policies

- **Target Tracking**: Maintains metric at target (e.g., 70% CPU)
- **Scheduled Scaling**: Predictable traffic patterns (business hours)
- **Step/Simple Scaling**: Based on CloudWatch alarms
- **BEST PRACTICE**: Scheduled + Target Tracking combined for predictable + unpredictable loads

---

## **HIGH AVAILABILITY vs FAULT TOLERANCE vs DISASTER RECOVERY**

### High Availability (HA)

- System stays operational as much as possible
- **SOME downtime acceptable** during failures
- Fast recovery but not instant
- Example: Active-Standby servers (users may need to log back in)
- **Less expensive** than fault tolerance

### Fault Tolerance (FT)

- System **continues operating** during component failure
- **ZERO downtime** - failures are transparent to users
- Example: Two active servers (one fails, other continues serving)
- **More expensive** than HA

### Disaster Recovery (DR)

- **Pre-planning** for disaster scenarios
- Requires offsite backups (NOT same building/region)
- **Practice DR exercises** before real disasters

### DR Strategies (Know RTO/RPO impact)

- **Backup & Restore**: Cheapest, highest RTO/RPO
- **Pilot Light**: Core services always running, scale up during disaster
- **Warm Standby**: Scaled-down version running, scale up quickly
- **Multi-Site Active/Active**: Full production in multiple regions, lowest RTO/RPO, most expensive

### RTO vs RPO

- **RPO** (Recovery Point Objective): Max acceptable **data loss** time (how often to back up)
- **RTO** (Recovery Time Objective): Max acceptable **downtime** (how fast to restore service)

---

## **DATABASE RESILIENCE**

### RDS Multi-AZ

- **High availability**, NOT performance
- Automatic failover in **60-120 seconds**
- Standby **cannot be accessed directly**
- Synchronous replication to standby

### Read Replicas

- **Performance** benefits (scale reads)
- **Availability** benefits (can promote to primary)
- Asynchronous replication
- **NOT a substitute for caching** (still has DB overhead)

### RDS Proxy

- Fully managed, HA database proxy
- Connection pooling (reduces DB load)
- **66% faster failover** times
- Integrates with Secrets Manager + IAM
- **Critical for serverless apps** (Lambda) with high connection rates

### Aurora Global Database

- Cross-region replication
- Fast cross-region failover
- Know failover time

### DynamoDB Global Tables

- Multi-region, multi-master
- Active-active replication

---

## **CACHING STRATEGIES**

**Know when to use each:**

- **CloudFront**: Edge caching for static/dynamic content globally
- **ElastiCache**: In-memory caching (Redis/Memcached) for DB queries, session data
- **DynamoDB Accelerator (DAX)**: Microsecond latency for DynamoDB reads
- **Read Replicas**: NOT caching (still DB overhead - use actual cache instead)

---

## **DECOUPLING ARCHITECTURES**

**Decoupling = components autonomous and unaware of each other**

### Synchronous Decoupling

- Both components must be available simultaneously
- Example: Load Balancer + EC2 instances

### Asynchronous Decoupling

- Components communicate through message stores
- Frontend can process faster than backend
- **SQS** for message queuing
- **EventBridge** for event-driven architectures

### SQS

- Decouple microservices, distributed systems
- High throughput requires **horizontal scaling** of producers/consumers
- Durable message store

---

## **SERVERLESS TECHNOLOGIES**

### Serverless Definition (AWS)

1. No infrastructure to provision/manage
2. Automatically scales by consumption unit
3. Pay-for-value billing
4. Built-in availability and fault tolerance

### Lambda

- Event-driven compute
- **Key Limits**: 15-min timeout, 10 GB memory, 1000 concurrent executions
- Understand **concurrency** and how it scales
- Use with RDS Proxy for database connections

### API Gateway

- Scales automatically
- Exposes Lambda functions as HTTP APIs
- Can call AWS services publicly

---

## **COMPUTE OPTIONS & PLACEMENT**

### EC2 Placement Groups

- **Cluster**: Low latency, high throughput, **single AZ**, HPC/ML workloads
- **Partition**: Large distributed systems (Kafka, Hadoop, Cassandra), up to 7 partitions per AZ
- **Spread**: Max 7 instances per AZ, critical instances, isolated failure domains

### Containers vs Serverless vs EC2

- **Lambda/Fargate**: No server management, serverless
- **ECS/EKS**: Container orchestration, managed control plane
- **EC2**: Full control, stateful apps

### Stateful Apps

- Use **EFS** or **FSx** for shared file storage across instances
- **Sticky sessions** (Application Load Balancer) keep users on same instance for in-memory state

---

## **LOAD BALANCING & TRAFFIC MANAGEMENT**

### Elastic Load Balancing + Auto Scaling

- Self-healing environment
- Tolerates single EC2 failure or entire AZ loss
- Use **across multiple AZs**

### Cross-Zone Load Balancing

- **ALB**: FREE
- **NLB/GWLB**: COSTS MONEY

### Route 53 Routing Policies

- **Failover**: Primary/secondary for DR
- **Latency-based**: Route to fastest region
- **Geolocation**: Data residency by user location

### Global Accelerator

- Improves global application availability and performance
- Uses AWS global network (not internet)

---

## **NETWORKING SERVICES**

### Must Know

- **VPC peering**: Connect VPCs
- **Transit Gateway**: Hub-and-spoke for multiple VPCs
- **Site-to-Site VPN**: Encrypted connection over internet
- **Direct Connect**: Dedicated private connection to AWS
- **Route 53 Resolver**: DNS queries between on-prem and AWS

### VPC Endpoints

- **Gateway Endpoints** (S3, DynamoDB): FREE
- **Interface Endpoints** (other services): COSTS MONEY

---

## **MONITORING & OBSERVABILITY**

### CloudWatch

- Track application/infrastructure metrics
- **CloudWatch Alarms**: Automated actions based on metrics
- **EventBridge**: Near real-time responses to changes

### X-Ray

- Distributed tracing
- Analyze and debug applications

---

## **AUTOMATION & DEPLOYMENT**

### Infrastructure as Code

- **CloudFormation**: AWS-native IaC
- **Elastic Beanstalk**: PaaS, least operational overhead
- **OpsWorks**: Chef/Puppet-based

### Security Scanning

- **Inspector**: Vulnerability scanning for infrastructure
- **CodeGuru**: Code analysis and recommendations

---

## **MANAGED SERVICES WITH BUILT-IN RESILIENCE**

These have **HA built-in** (know their capabilities):
- SQS
- Lambda
- Fargate
- DynamoDB
- S3

---

## **ADDITIONAL SERVICES TO KNOW**

### AWS Transfer Family

- Managed SFTP/FTPS/FTP
- No infrastructure to manage
- Auto-scales across 3 AZs

### Elastic Disaster Recovery

- For on-prem and cloud-based apps
- Know use cases

### Amazon Polly

- Text-to-speech
- Use case: IT service request systems (with Comprehend for classification)
- Self-service in Amazon Connect contact centers

### Comprehend

- NLP/ML service
- Classify documents (e.g., IT tickets)

---

## **ARCHITECTURE PATTERNS**

### Service-Oriented Architecture (SOA)

- Reusable software components via service interfaces

### Microservices

- Smaller, simpler components than SOA
- Communicate via APIs
- Patterns: API-driven, event-driven, data streaming

### Multi-Tier Architectures

- Purpose-built databases for different needs (RDS, DynamoDB, Redshift, Aurora)

---

## **EXAM KEYWORDS (CRITICAL!)**

- **"MOST cost-effective"** → Spot, Glacier, Auto Scaling, managed services
- **"LEAST operational overhead"** → Lambda, Fargate, Elastic Beanstalk, managed services
- **"MOST secure"** → KMS, MFA, least privilege IAM
- **"High availability"** → Multi-AZ, Auto Scaling, multi-region

---

## **KEY TAKEAWAYS FOR EXAM DAY**

1. **Know the difference**: HA vs FT vs DR (and RTO/RPO)
2. **RDS Multi-AZ** = HA (NOT performance), **Read Replicas** = performance
3. **Scaling policies**: Combine scheduled + target tracking
4. **Decoupling**: SQS for async, ELB for sync
5. **Placement Groups**: Cluster (HPC), Partition (big data), Spread (critical apps)
6. **RDS Proxy**: Faster failover, connection pooling, serverless-friendly
7. **Global services**: Route 53 failover/latency routing + Global Accelerator
8. **Built-in HA services**: SQS, Lambda, Fargate, S3, DynamoDB

---

**Good luck on your exam! 🚀**
