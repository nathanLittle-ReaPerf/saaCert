# Monitoring, DR, & Other Services Quick Reference

## CloudWatch

### Overview
- **What**: Monitoring and observability service
- **Metrics**: Collect and track metrics from AWS services
- **Logs**: Aggregate, monitor, and store logs
- **Alarms**: Trigger actions based on metrics
- **Dashboards**: Visualize metrics

### Key Concepts
| Component | Description | Use Case |
|-----------|-------------|----------|
| **Metrics** | Time-ordered data points (CPU, NetworkIn, etc.) | Monitor resource performance |
| **Namespace** | Container for metrics (AWS/EC2, AWS/RDS) | Organize metrics by service |
| **Dimension** | Name/value pair to identify metric (InstanceId=i-123) | Filter metrics |
| **Statistic** | Aggregation (Sum, Average, Min, Max) | Analyze metric data |

### CloudWatch Metrics
**Default EC2 Metrics** (every 5 minutes, free):
- **CPU**: CPUUtilization
- **Network**: NetworkIn, NetworkOut
- **Disk**: DiskReadOps, DiskWriteOps
- **Status**: StatusCheckFailed

**NOT included by default** (need CloudWatch Agent):
- **Memory utilization**
- **Disk space utilization**
- **Swap usage**
- **Process counts**

**Detailed Monitoring**: 1-minute intervals (costs extra)

### CloudWatch Alarms
- **States**: OK, ALARM, INSUFFICIENT_DATA
- **Actions**:
  - **EC2**: Stop, terminate, reboot, recover
  - **Auto Scaling**: Trigger scaling policies
  - **SNS**: Send notifications
- **Composite Alarms**: Combine multiple alarms (AND, OR logic)

### CloudWatch Logs
**Features:**
- **Log Groups**: Collection of log streams (e.g., /aws/lambda/my-function)
- **Log Streams**: Sequence of log events from same source
- **Retention**: 1 day to 10 years (or never expire)
- **Encryption**: KMS encryption at rest

**Log Insights:**
- **What**: Query and analyze log data
- **Query language**: SQL-like syntax
- **Use case**: Troubleshoot, find patterns, aggregate data

**Log Subscriptions:**
- **What**: Real-time feed of log events
- **Targets**: Kinesis Data Streams, Kinesis Firehose, Lambda
- **Use case**: Real-time processing, centralized logging

**Log Export:**
- **S3**: Batch export (up to 12 hours delay)
- **Use case**: Long-term archival, compliance

### CloudWatch Agent
- **What**: Collect metrics and logs from EC2/on-premises servers
- **Collects**: Memory, disk, process metrics, custom logs
- **Configuration**: SSM Parameter Store or config file

### EventBridge (formerly CloudWatch Events)
- **What**: Event bus for AWS services and custom applications
- **Sources**: AWS services (EC2, S3, etc.), custom apps, SaaS apps
- **Targets**: Lambda, SNS, SQS, Step Functions, etc.
- **Rules**: Match events and route to targets
- **Scheduled events**: Cron expressions (e.g., every 5 minutes)

**Example Use Cases:**
- S3 object created → EventBridge → Lambda (process file)
- EC2 instance state change → EventBridge → SNS (alert)
- Schedule Lambda every day at 6 AM

---

## AWS X-Ray

### Overview
- **What**: Distributed tracing for microservices
- **Traces**: End-to-end request path through application
- **Service Map**: Visual representation of services and dependencies
- **Use case**: Debug performance issues, find bottlenecks, identify errors

### Key Concepts
- **Segment**: Data about work done by single component (EC2, Lambda)
- **Subsegment**: Granular detail within segment (DB query, HTTP call)
- **Trace**: Collection of segments (complete request path)
- **Sampling**: Control how many requests to trace (reduce cost)

### Integration
- **Lambda**: Enable X-Ray in console
- **Elastic Beanstalk**: Enable in configuration
- **ECS/EKS**: Run X-Ray daemon as sidecar container
- **EC2**: Install X-Ray daemon

---

## Disaster Recovery (DR) Strategies

### RPO vs RTO (CRITICAL!)
- **RPO** (Recovery Point Objective): How much data loss is acceptable (time between backups)
- **RTO** (Recovery Time Objective): How quickly must you recover (downtime tolerance)

**Example:**
- RPO = 1 hour: Can lose up to 1 hour of data (backup every hour)
- RTO = 4 hours: System must be back online within 4 hours

### DR Strategies (MUST KNOW!)

| Strategy | RPO | RTO | Cost | Description |
|----------|-----|-----|------|-------------|
| **Backup & Restore** | Hours | 24 hours | $ | Backup to S3, restore when needed |
| **Pilot Light** | Minutes | Hours (2-4) | $$ | Minimal resources running (DB replica), scale up in DR |
| **Warm Standby** | Minutes | Minutes (10-60) | $$$ | Scaled-down version running, scale up in DR |
| **Multi-Site Active-Active** | Near-zero | Seconds | $$$$ | Full production in multiple regions |

### Strategy Details

**1. Backup & Restore**
- **How**: Regular backups to S3/Glacier, restore in DR event
- **AWS Services**: AWS Backup, EBS snapshots, RDS snapshots, AMIs
- **Pros**: Cheapest
- **Cons**: Slowest recovery (hours to restore data, launch resources)

**2. Pilot Light**
- **How**: Core services running at minimum (e.g., DB replica), quickly scale up in DR
- **Example**: RDS read replica in DR region, scale up EC2 when needed
- **Pros**: Faster than backup/restore, cost-effective
- **Cons**: Still requires scaling time (2-4 hours)

**3. Warm Standby**
- **How**: Scaled-down but functional environment in DR region, scale to full capacity in DR
- **Example**: Reduced EC2 fleet, RDS read replica, scale up Auto Scaling in DR
- **Pros**: Fast recovery (minutes), test DR environment
- **Cons**: More expensive than Pilot Light

**4. Multi-Site Active-Active**
- **How**: Full production environment in multiple regions, active traffic routing
- **Example**: Aurora Global Database, Route 53 failover, full EC2 fleet in both regions
- **Pros**: Near-zero RPO/RTO, highest availability
- **Cons**: Most expensive (double infrastructure cost)

### DR Components
- **Multi-AZ**: HA within region (NOT DR, same region)
- **Cross-Region**: True DR (survive region failure)
- **Route 53 Health Checks + Failover**: Automatic DNS failover
- **Aurora Global Database**: <1 sec replication, <1 min failover
- **S3 Cross-Region Replication**: Replicate data to DR region
- **CloudFormation**: Infrastructure as Code (quickly rebuild in DR region)

---

## High Availability Patterns

### Design Principles
1. **Eliminate single points of failure**: Use Multi-AZ, redundancy
2. **Automate recovery**: Health checks, Auto Scaling, failover
3. **Scale horizontally**: Add more instances (not bigger instances)
4. **Stop guessing capacity**: Use Auto Scaling, elastic services
5. **Design for failure**: Assume everything will fail

### Multi-AZ Best Practices
- **Distribute resources**: Spread across multiple AZs
- **Load balancers**: ALB/NLB across multiple AZs
- **Databases**: RDS Multi-AZ, Aurora (multi-AZ by default)
- **Stateless applications**: Store state externally (DynamoDB, ElastiCache)

---

## Well-Architected Framework

### 6 Pillars (MEMORIZE!)

**1. Operational Excellence**
- **Focus**: Run and monitor systems, continuous improvement
- **Key**: Automation, infrastructure as code (CloudFormation), small frequent changes
- **Services**: CloudFormation, Systems Manager, CloudWatch, X-Ray

**2. Security**
- **Focus**: Protect information, systems, assets
- **Key**: IAM, encryption, detective controls, incident response
- **Services**: IAM, KMS, GuardDuty, Security Hub, CloudTrail

**3. Reliability**
- **Focus**: Recover from failures, meet demand
- **Key**: Foundations, change management, failure management
- **Services**: Auto Scaling, Multi-AZ, backups, Route 53

**4. Performance Efficiency**
- **Focus**: Use resources efficiently, maintain efficiency as demand changes
- **Key**: Select right resource types, monitor performance, evolve architecture
- **Services**: Auto Scaling, Lambda, CloudFront, ElastiCache

**5. Cost Optimization**
- **Focus**: Avoid unnecessary costs
- **Key**: Right-sizing, Reserved Instances, Spot, monitoring spending
- **Services**: Cost Explorer, Budgets, Trusted Advisor, S3 Intelligent-Tiering

**6. Sustainability**
- **Focus**: Minimize environmental impact
- **Key**: Use managed services, reduce over-provisioning, efficient architectures
- **Services**: Lambda, Graviton instances, S3 Intelligent-Tiering

---

## Cost Management

### AWS Cost Explorer
- **What**: Visualize and analyze costs
- **Features**: Filter by service, tag, region, time period
- **Forecasting**: Predict future costs (12 months)
- **Savings Plans recommendations**: Identify cost savings

### AWS Budgets
- **What**: Set custom budgets, alert when exceeded
- **Types**: Cost, usage, Reserved Instance utilization
- **Alerts**: Email, SNS
- **Use case**: Cost control, prevent overspending

### AWS Trusted Advisor
- **What**: Recommendations for cost, performance, security, fault tolerance
- **Free**: 7 core checks (S3 bucket permissions, Security Groups, IAM use, MFA on root)
- **Business/Enterprise Support**: All checks (40+ checks)
- **Checks**:
  - **Cost**: Idle RDS, unattached EBS volumes, underutilized instances
  - **Security**: S3 public buckets, IAM password policy, MFA
  - **Fault Tolerance**: RDS Multi-AZ, ELB health checks
  - **Performance**: High utilization instances, CloudFront

### Cost Optimization Strategies
1. **Right-sizing**: Use appropriate instance types (don't over-provision)
2. **Reserved Instances/Savings Plans**: 40-60% discount for committed usage
3. **Spot Instances**: Up to 90% discount for fault-tolerant workloads
4. **Auto Scaling**: Scale down during low-traffic periods
5. **S3 Lifecycle Policies**: Transition to cheaper storage classes
6. **Shut down non-production**: Stop dev/test resources after hours
7. **Use managed services**: Reduce operational overhead (Lambda, Fargate)

---

## CloudFormation

### Overview
- **What**: Infrastructure as Code (IaC)
- **Templates**: JSON or YAML describing resources
- **Stacks**: Collection of resources created from template
- **Change Sets**: Preview changes before applying

### Key Concepts
| Concept | Description |
|---------|-------------|
| **Template** | Define AWS resources (EC2, VPC, RDS, etc.) |
| **Stack** | Deployed resources from template |
| **Parameters** | Input values at runtime (instance type, key pair) |
| **Mappings** | Static key-value pairs (AMI IDs per region) |
| **Resources** | AWS resources to create (required section) |
| **Outputs** | Values to export (VPC ID, ALB DNS) |

### Features
- **Version control**: Track template changes in Git
- **Rollback**: Automatic rollback on failure
- **Drift detection**: Detect manual changes outside CloudFormation
- **StackSets**: Deploy stacks across multiple accounts/regions
- **Nested stacks**: Reuse templates (modular architecture)

### CloudFormation vs Terraform
| Feature | CloudFormation | Terraform |
|---------|----------------|-----------|
| **Provider** | AWS only | Multi-cloud |
| **Language** | JSON/YAML | HCL |
| **State** | Managed by AWS | Manual (S3 + DynamoDB) |
| **Cost** | Free | Open-source (free) |

---

## Other Important Services

### SQS (Simple Queue Service)
- **What**: Managed message queue
- **Types**: Standard (at-least-once, no ordering) | FIFO (exactly-once, strict ordering)
- **Use case**: Decouple components, buffer messages, async processing
- **Retention**: 1 min to 14 days (default 4 days)
- **Visibility timeout**: Message hidden after read (0 sec to 12 hours)
- **Long polling**: Wait for messages (reduce empty responses, save cost)

### SNS (Simple Notification Service)
- **What**: Pub/sub messaging (fanout)
- **Subscribers**: Email, SMS, HTTP, Lambda, SQS, mobile push
- **Use case**: Send notifications to multiple subscribers
- **Message filtering**: Subscribers filter messages by attributes

### Step Functions
- **What**: Orchestrate workflows (state machines)
- **States**: Task, Choice, Parallel, Wait, Succeed, Fail
- **Use case**: Coordinate multiple Lambda functions, long-running workflows
- **Pricing**: Per state transition

### SWF (Simple Workflow Service)
- **What**: Coordinate work across distributed components (legacy)
- **vs Step Functions**: SWF is older, use Step Functions for new projects

### AppSync
- **What**: Managed GraphQL service
- **Data sources**: DynamoDB, Lambda, RDS, HTTP endpoints
- **Use case**: Real-time apps, mobile/web APIs with GraphQL

### Kinesis
| Service | Purpose | Use Case |
|---------|---------|----------|
| **Data Streams** | Real-time data streaming | Ingest, process, analyze streams |
| **Firehose** | Load streams into S3, Redshift, OpenSearch | ETL, data lake |
| **Data Analytics** | SQL queries on streams | Real-time analytics |
| **Video Streams** | Ingest video streams | Video analytics, ML |

### Glue
- **What**: ETL service (Extract, Transform, Load)
- **Glue Crawler**: Discover schema, populate Data Catalog
- **Glue Job**: Run ETL scripts (PySpark, Scala)
- **Glue Data Catalog**: Metadata repository (used by Athena, Redshift Spectrum)

### Athena
- **What**: Query S3 data using SQL (serverless)
- **File formats**: CSV, JSON, Parquet, ORC
- **Use case**: Ad-hoc queries, analyze logs, BI reporting
- **Pricing**: $5 per TB scanned (use Parquet/ORC for compression, save cost)

---

## Exam Pattern Recognition

### "Decouple components, buffer messages" → **SQS**
- Strict ordering: **SQS FIFO**
- No ordering: **SQS Standard**

### "Send notification to multiple subscribers" → **SNS**

### "Fanout (one message to many queues)" → **SNS → SQS**

### "Orchestrate multi-step workflow" → **Step Functions**

### "Real-time data streaming" → **Kinesis Data Streams**

### "Load streaming data to S3" → **Kinesis Firehose**

### "Query S3 data with SQL" → **Athena**

### "ETL, data transformation" → **Glue**

### "Infrastructure as Code" → **CloudFormation**

### "Monitor metrics, set alarms" → **CloudWatch**

### "Distributed tracing, debug microservices" → **X-Ray**

### "Cost recommendations, security checks" → **Trusted Advisor**

### "Audit API calls" → **CloudTrail**

### "Track config changes, compliance" → **Config**

### "RPO in minutes, RTO in hours" → **Pilot Light**

### "RPO in minutes, RTO in minutes" → **Warm Standby**

### "Near-zero RPO/RTO" → **Multi-Site Active-Active**

---

## Exam Keywords to Recognize

**Cost:**
- "MOST cost-effective" → Spot, Savings Plans, Lifecycle policies, right-sizing
- "LEAST operational overhead" → Managed services (Lambda, Fargate, RDS)

**Performance:**
- "Low latency" → CloudFront, ElastiCache, Aurora, DynamoDB
- "High IOPS" → io1/io2 EBS, instance store
- "High throughput" → st1 EBS, S3, Kinesis

**Availability:**
- "High availability" → Multi-AZ, Auto Scaling, ELB
- "Disaster recovery" → Cross-region, backups, failover

**Security:**
- "Encrypt" → KMS, SSL/TLS
- "Audit" → CloudTrail, Config
- "Threat detection" → GuardDuty
- "Web protection" → WAF

**Scalability:**
- "Scale automatically" → Auto Scaling, Lambda, DynamoDB
- "Handle millions of requests" → NLB, DynamoDB, CloudFront
