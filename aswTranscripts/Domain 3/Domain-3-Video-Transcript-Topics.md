# Domain 3: Design High-Performing Architectures - Video Transcript Topics

**Exam Date: March 2, 2026 at 5:15 PM EST**
**Created: February 12, 2026**

This document contains concise explanations of all topics covered in the Domain 3 video transcripts that you need to know for the AWS SAA-C03 exam.

---

## **STORAGE FUNDAMENTALS**

### Storage Types in AWS

**Object Storage (S3):**
- Best for: Big data, backup/recovery, static content
- Globally resilient service (tolerates AZ failure)
- Data stored in specific Region, can replicate cross-region
- Understand S3 storage classes and multi-part uploads

**Block Storage (EBS):**
- Persistent storage for EC2 instances
- Keywords: DAS, SAN, persistent storage
- Two types of EC2 storage: Instance Store (ephemeral) + EBS (persistent)
- EBS snapshots stored in S3 = Region resilient
- Supports live configuration changes while in production

**File Storage:**
- **EFS**: Network file system, NFS protocol, Linux instances, hybrid access via VPN/Direct Connect
- **FSx for Windows**: SMB protocol, Windows NTFS, Active Directory integration, highly available
- **FSx for Lustre**: Linux high-performance computing

### Storage Scaling

**EBS Scaling:**
- Manual scaling required
- Modify volume type, size, and IOPS capacity
- NOT automatic but supports live changes

**EFS Scaling:**
- Automatically scales as you add/remove files
- No manual intervention needed
- Best for "least operational overhead"

---

## **STORAGE PERFORMANCE OPTIMIZATION**

### EBS Volume Types

**Know performance characteristics:**
- Different types have different IOPS and throughput limits
- Configurable performance impacts application latency
- Choose based on workload requirements

### S3 Performance Features

**Multi-part Uploads:**
- For large file uploads
- Improves transfer performance

**S3 Transfer Acceleration:**
- Faster uploads using CloudFront edge locations

**CloudFront Caching:**
- Improves data retrieval performance
- Caches content at edge locations globally

### Storage Service Upper Bounds

- Know capacity limits for each storage service
- Important for planning future growth (e.g., 3TB today → 100TB in 5 years)
- Consider data accumulation rate in architecture decisions

---

## **COMPUTE FUNDAMENTALS**

### Compute Types in AWS

**Instances (EC2):**
- Virtual servers with various families and sizes
- Understand virtualization fundamentals
- Know CPU, memory, local storage, network bandwidth per instance type
- Choose instance family based on workload (compute, memory, storage, GPU)

**Containers:**
- **ECS**: Elastic Container Service with instructions for running containers
- **ECS on EC2**: Full control of compute environment
- **ECS on Fargate**: Serverless containers (no server management)
- **EKS**: Kubernetes on AWS
- Integrate with Application Load Balancers for port mapping

**Functions (Lambda):**
- Serverless compute without EC2 instances
- Event-driven execution
- **Key Limits**: 15-minute timeout, 10 GB memory max, 1000 concurrent executions
- Billed for execution duration only
- Use Step Functions if execution > 15 minutes
- Can deploy to CloudFront edge locations (Lambda@Edge) for global performance

---

## **COMPUTE SCALING & ELASTICITY**

### EC2 Auto Scaling

**Scaling Policies:**
- **Target Tracking**: Maintains metric at target value (e.g., 70% CPU)
- **Scheduled Scaling**: Predictable traffic patterns (business hours)
- **Step/Simple Scaling**: Based on CloudWatch alarms
- **Custom Metrics**: Memory usage, custom application metrics

**Launch Configurations:**
- Define what to provision during scaling
- Set minimum, maximum, and desired capacity

### CloudWatch Integration

**Metrics for Scaling:**
- Default metrics: CPU, network, disk
- Custom metrics: Memory (not available by default), application-specific
- ELB metrics: HealthyHostCount, SurgeQueueLength
- Create alarms that invoke scaling actions when thresholds crossed

**Dashboards & Automation:**
- Visibility into performance
- Automated alerts and remediation
- Choose metrics based on application needs

### Inherently Scalable Services

**No scaling configuration needed:**
- Lambda (scales automatically with invocations)
- S3 (scales automatically)
- DynamoDB (with proper capacity mode)

---

## **DECOUPLING COMPUTE WORKLOADS**

### Decoupling Strategies

**SQS (Simple Queue Service):**
- Asynchronous decoupling
- Decouple microservices and distributed systems
- Durable message store
- Frontend can process faster than backend
- Horizontal scaling needed for high throughput

**Elastic Load Balancing:**
- Synchronous decoupling
- Distributes traffic across instances
- Integrates with Auto Scaling for health checks
- Application-aware routing

### Load Balancer Types

**Know OSI Layer for each:**
- **ALB** (Application Load Balancer): Layer 7
- **NLB** (Network Load Balancer): Layer 4
- **GLB** (Gateway Load Balancer): Layer 3

---

## **NETWORKING FUNDAMENTALS**

### Network Performance Factors

**Key Concepts:**
- Bandwidth, latency, jitter, throughput
- Network is between ALL components of architecture
- Understanding networking fundamentals is critical

**AWS Networking Features:**
- Enhanced networking
- EBS-optimized instances
- S3 Transfer Acceleration
- CloudFront edge caching

### VPC Architecture

**Build Order:**
1. VPC (Regional resilience)
2. Subnets
3. Route tables
4. Internet Gateway
5. Network ACLs
6. Security Groups
7. Customization (NAT Gateway, peering, endpoints)

**VPC is private by default** - requires explicit configuration for public access

---

## **HYBRID CONNECTIVITY**

### Connection Types

**VPN (Virtual Private Network):**
- Encrypted connection over internet
- Lower cost
- Variable performance (internet-dependent)

**Direct Connect:**
- Dedicated private connection
- Consistent performance
- Higher cost
- Better for large data volumes and compliance

**Transit Gateway:**
- Hub-and-spoke model for multiple VPCs
- Works with VPN or Direct Connect
- Simplifies network peering

**AWS Cloud Hub:**
- Hub-and-spoke for connecting networks
- Multi-site connectivity

### VPC Connectivity

**VPC Peering:**
- Connect two VPCs
- Direct network route

**PrivateLink:**
- Private connection between VPCs
- No VPC peering overhead
- No internet exposure

**VPC Endpoints:**
- **Gateway Endpoints** (S3, DynamoDB): FREE
- **Interface Endpoints** (other services): COSTS MONEY
- Access AWS services without internet/NAT gateway

---

## **GLOBAL PERFORMANCE OPTIMIZATION**

### Route 53 Routing Policies

**Geoproximity Routing:**
- Route to geographically closer Region
- Based on user location

**Latency-based Routing:**
- Route to fastest Region for user
- Performance optimization

**Failover Routing:**
- Primary/secondary for disaster recovery
- Health check-based

### CloudFront

**Use Cases:**
- Cache static and dynamic content at edge locations
- Global content delivery
- Reduces latency for end users
- In-depth knowledge required for exam

### Global Accelerator

**Features:**
- Improves global application performance
- Uses AWS global network (not public internet)
- S3 Multi-Region Access Points support
- Improves security, reliability, performance

---

## **DATA TRANSFER SERVICES**

### Migration Service Selection

**Know when to use each:**

**AWS DataSync:**
- Automated data transfer
- NFS, SMB, S3, EFS, FSx
- Scheduled transfers

**Snow Family:**
- Massive data transfers (>10TB)
- Physical device shipped to you
- Snowball, Snowball Edge, Snowmobile
- Use when large data + tight deadline + slow internet

**AWS Transfer Family:**
- Managed SFTP/FTPS/FTP
- No infrastructure to manage
- Integrates with S3, EFS

**Database Migration Service (DMS):**
- Database-to-database migrations
- Minimal downtime

**Selection criteria:**
- Amount of data
- Type of data
- Source and destination
- Network bandwidth
- Timeline

---

## **DATA INGESTION & TRANSFORMATION**

### Data Ingestion Patterns

**Homogenous Ingestion:**
- Same format/engine from source to destination
- Focus: Speed, data integrity, automation
- Use: Athena, EMR for cloud-based ETL

**Heterogeneous Ingestion:**
- Transform data during ingestion
- Change data type/format
- Apply machine learning for new attributes

### Kinesis Family

**Kinesis Data Streams:**
- Real-time data streaming
- High scalability and durability
- Captures gigabytes/second from multiple sources
- Producers push data into streams (records)

**Kinesis Data Firehose:**
- Load data streams into AWS data stores
- Simplest approach for capturing, transforming, loading
- Provides basic data transformation

**Kinesis Data Analytics:**
- Includes data transformation options
- Process and analyze streaming data

**Kinesis Video Streams:**
- Ingest streaming video data

### Data Transformation Services

**Amazon EMR (Elastic MapReduce):**
- Large-scale data processing
- Process data at massive scale
- Transform to Parquet format for data lakes
- Adjust concurrent S3 requests, retry strategies

**AWS Glue:**
- Data integration service
- Discover, prepare, move, integrate data
- ETL jobs for analytics, ML, application development
- Works with S3 data lakes

**AWS Lake Formation:**
- Centralized data lake management
- Quick ingestion, eliminate duplication
- Centralized governance

**Lambda:**
- Transform data assets in S3 data lakes
- Event-driven transformations

### Data Analytics & Visualization

**Amazon Athena:**
- Serverless query service
- Query data in S3 using SQL
- No infrastructure to manage

**Amazon QuickSight:**
- Business intelligence and visualization
- Interactive dashboards

**AWS Lake Formation:**
- Build and manage data lakes
- Security and governance

---

## **DATA LAKE ARCHITECTURE**

### Data Lake Benefits

- Manage multiple data types from various sources
- Store structured and unstructured data
- Centralized repository
- Quick ingestion
- Eliminate data duplication and sprawl
- Centralized governance and management

### Data Lake Security

**Access Control:**
- S3 bucket policies for centralized access
- IAM user policies for data processing tools
- Role-based permissions
- S3 cross-region replication
- S3 Object Lock and versioning

**Encryption:**
- S3 encryption options
- KMS for encryption key management
- API Gateway + Cognito for additional protection
- Cloud HSM for PII compliance requirements

### Data Lake Ingestion Methods

**Select based on:**
- Frequency of data streaming
- Data change frequency
- Source location (on-premises vs cloud)

**Services for ingestion:**
- Kinesis Data Firehose (streaming)
- Snow Family (massive on-prem transfers)
- AWS Glue (ETL)
- AWS DataSync (automated transfers)
- AWS Transfer Family (SFTP/FTP)
- Storage Gateway (hybrid)
- Direct Connect (dedicated connection)
- DMS (database migrations)

---

## **STREAMING DATA SOLUTIONS**

### Amazon MSK (Managed Streaming for Apache Kafka)

- Alternative to Kinesis
- Apache Kafka managed service
- Real-time data streaming

### Streaming Architecture Components

**Data Producers → Data Streams → Data Processing → Data Storage**

Example flow:
1. Application produces data
2. Kinesis Data Stream captures data
3. Kinesis Data Analytics processes data
4. Kinesis Data Firehose loads into S3/Redshift

---

## **PERFORMANCE OPTIMIZATION STRATEGIES**

### S3 Optimization

**For EMR and Glue accessing S3:**
- Adjust concurrent S3 requests
- Modify retry strategy
- Adjust number of S3 objects processed
- S3 scales horizontally - leverage distributed processing

### Caching Strategies

**CloudFront:**
- Edge caching for static/dynamic content
- Global distribution
- Reduces origin load

**ElastiCache:**
- In-memory caching (Redis/Memcached)
- Database query caching
- Session data storage

**DynamoDB Accelerator (DAX):**
- Microsecond latency for DynamoDB reads
- In-memory cache

---

## **SCALABILITY CONSIDERATIONS**

### High Availability at Scale

**Design principles:**
- No single point of failure
- Automated monitoring and failure detection
- Failover mechanisms for stateless and stateful components
- Multi-AZ deployments
- Multi-Region for critical workloads

**Service combinations:**
- Auto Scaling + Lifecycle hooks + CloudWatch Events
- Route 53 health checks + DNS failover
- Lambda functions for automated remediation

### Global Scalability

**Serverless global architecture example:**
- S3 website hosting for static content
- Route 53 latency + failover routing
- DynamoDB Global Tables for cross-region replication
- CloudFront for improved performance, reliability, security
- Global Accelerator for S3 Multi-Region Access Points

### Regional Architecture

**Considerations:**
- Customer base location (3+ regions)
- Latency requirements
- Real-time communication needs (99.9% to 99.999% availability)
- Cost optimization

---

## **COMPUTE SERVICE SELECTION**

### When to Use Each

**EC2:**
- Need full control of OS and configuration
- Stateful applications
- Custom software requirements
- Long-running processes

**Lambda:**
- Event-driven workloads
- Short executions (<15 minutes)
- Unpredictable/variable load
- Serverless requirements

**Containers (ECS/EKS):**
- Microservices architecture
- Portable applications
- Need orchestration
- Mixed workload types

**Fargate:**
- Serverless containers
- No infrastructure management
- Focus on application, not servers

---

## **OPTIMIZATION TECHNIQUES**

### Network Optimization

**Placement strategies:**
- Place resources close to users
- Use edge locations
- Cross-region deployment for global users

**Placement Groups (EC2):**
- Know when to use each type for performance
- Consider network throughput requirements

**Infrastructure as Code:**
- CloudFormation for network definition
- Quick rebuild and modification
- Version control for network architecture
- Evolve network over time

### Monitoring & Improvement

**CloudWatch:**
- Track metrics continuously
- Set alarms for performance thresholds
- Use metrics to guide scaling decisions

**Evolution:**
- Stay current with architecture
- Modify as workload evolves
- Use data from benchmarking and load testing
- Data-driven optimization decisions

---

## **DATABASE PERFORMANCE** (Brief Overview)

### Read Replicas

**Use cases:**
- Scale read-heavy workloads
- RDS/Aurora support
- NOT a replacement for caching

**Caching alternatives:**
- ElastiCache for frequently accessed data
- DAX for DynamoDB reads

### DynamoDB Capacity Modes

**On-Demand:**
- Unpredictable traffic
- New applications with unknown patterns
- Pay per request

**Provisioned:**
- Predictable traffic patterns
- Cost optimization for steady workloads
- Auto Scaling available

---

## **API & APPLICATION SERVICES**

### API Gateway

**Performance features:**
- Scales automatically
- Configure throttling and quotas
- Prevent overwhelming backend
- Exposes Lambda as HTTP APIs
- Can call AWS services publicly

### AWS AppSync

**Features:**
- Scalable GraphQL interface
- Combine data from multiple sources
- DynamoDB, Lambda, HTTP APIs integration
- Real-time data synchronization

### Step Functions

**Use when:**
- Workflow orchestration needed
- Multi-step processes
- Longer than 15-minute execution
- Complex state management

### EventBridge

**Use cases:**
- Event-driven architectures
- Decouple microservices
- Near real-time event routing

---

## **BATCH PROCESSING**

### AWS Batch

**Features:**
- Fully managed batch computing
- Scales compute resources automatically
- Schedule batch jobs
- No infrastructure management

**Use cases:**
- Large-scale data processing
- Scientific simulations
- Financial modeling

---

## **HYBRID ARCHITECTURE PATTERNS**

### Typical Hybrid Model

**Components:**
- Amazon VPC in AWS
- On-premises data center
- Private communication for data transfer

**Design considerations:**
- Volume of data
- Compliance standards
- Performance requirements
- Cost constraints

### Connectivity Selection

**Choose based on:**
- Data volume
- Required throughput
- Latency sensitivity
- Security/compliance needs
- Budget

---

## **EXAM KEYWORDS (CRITICAL!)**

- **"High-performing"** → Enhanced networking, EBS-optimized, placement groups, caching
- **"Scalable"** → Auto Scaling, horizontal scaling, managed services
- **"LEAST operational overhead"** → Lambda, Fargate, managed services (EFS vs EBS)
- **"Low latency"** → ElastiCache, DAX, CloudFront, placement groups, direct connections
- **"Real-time"** → Kinesis, Lambda, EventBridge, AppSync
- **"Large data transfer"** → Snow Family (>10TB), Direct Connect, DataSync
- **"Hybrid"** → Storage Gateway, VPN, Direct Connect, FSx, Transfer Family

---

## **KEY TAKEAWAYS FOR EXAM DAY**

1. **Storage scaling**: EBS = manual, EFS = automatic
2. **Storage types**: Object (S3), Block (EBS), File (EFS/FSx)
3. **Compute selection**: EC2 (control), Lambda (serverless), Containers (portable)
4. **Lambda limits**: 15-min timeout, 10 GB memory, 1000 concurrent executions
5. **VPC Endpoints**: Gateway (S3/DynamoDB) = FREE, Interface = COSTS MONEY
6. **Data transfer**: Large data + tight deadline + slow internet = Snowball
7. **Kinesis family**: Data Streams (real-time capture), Firehose (load to stores), Analytics (transform)
8. **Hybrid connectivity**: VPN (encrypted over internet), Direct Connect (dedicated private)
9. **Global performance**: CloudFront (caching), Global Accelerator (AWS network), Route 53 (routing)
10. **Decoupling**: SQS (async), ELB (sync), EventBridge (event-driven)
11. **Auto Scaling policies**: Combine scheduled + target tracking for best results
12. **Data lakes**: Use Glue for ETL, Lake Formation for governance, Athena for queries
13. **Network fundamentals**: Know VPC build order, security groups, NACLs, route tables
14. **Caching layers**: CloudFront (edge), ElastiCache (in-memory), DAX (DynamoDB)
15. **High availability**: Multi-AZ, Auto Scaling, no single point of failure

---

**Note**: Domain 3 focuses on **performance and scalability** across all architectural layers. Always consider:
- How will it scale?
- What are the performance characteristics?
- Is there operational overhead?
- Can it handle global distribution?
- What are the limits and quotas?

**Good luck on your exam! 🚀**
