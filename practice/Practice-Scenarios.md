# SAA-C03 Practice Scenarios

These scenarios mirror real exam questions - multi-service solutions with specific requirements. Focus on keywords like "MOST cost-effective", "LEAST operational overhead", "MOST secure", etc.

---

## Scenario 1: E-Commerce Website High Availability
**Difficulty: Medium**

A company runs an e-commerce website on AWS. The application uses a three-tier architecture with web servers, application servers, and a MySQL database. The company wants to ensure high availability and automatic scaling based on traffic patterns while minimizing costs.

**Current Architecture:**
- Web tier: 4 EC2 instances (t3.medium)
- App tier: 6 EC2 instances (c5.large)
- Database: Single RDS MySQL instance (db.m5.xlarge)

**Requirements:**
- Must handle traffic spikes during sales events (5x normal traffic)
- Database downtime must be less than 2 minutes for maintenance
- Must be resilient to AZ failures
- Cost optimization during low-traffic periods

**Question:** Which combination of solutions meets these requirements with the LEAST operational overhead?

**A)** Deploy web and app tiers in an Auto Scaling group across multiple AZs with ALB. Enable RDS Multi-AZ. Use scheduled scaling for known sales events.

**B)** Deploy web and app tiers in an Auto Scaling group with target tracking policy. Enable RDS Multi-AZ with read replicas in 2 AZs. Use CloudFront for static content.

**C)** Use EC2 Auto Scaling with step scaling policies. Enable RDS Multi-AZ. Use ElastiCache for read-heavy workloads. Deploy ALB with SSL termination.

**D)** Migrate to ECS Fargate with Auto Scaling. Enable Aurora Multi-AZ with Aurora Replicas. Use CloudFront with ALB origin.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- Auto Scaling with target tracking is the LEAST operational overhead (automatically adjusts without scheduled rules)
- RDS Multi-AZ provides <2 min failover for maintenance
- Read replicas improve database performance during traffic spikes
- CloudFront reduces load on origin servers and is cost-effective
- Meets all requirements with minimal management

**Why others are wrong:**
- **A:** Scheduled scaling requires predicting traffic patterns (more operational overhead)
- **C:** Step scaling requires defining multiple policies (more operational overhead than target tracking)
- **D:** While technically sound, migrating to ECS Fargate and Aurora is significant effort, not "least overhead" for current setup

**Key Exam Tips:**
- "LEAST operational overhead" = managed services + auto-scaling
- Multi-AZ failover time is typically 60-120 seconds
- Target tracking > Step scaling > Scheduled scaling (for operational simplicity)
</details>

---

## Scenario 2: Secure File Upload System
**Difficulty: Hard**

A healthcare company needs to build a system where patients can upload medical documents securely. Documents must be encrypted at rest and in transit, accessible only by authorized doctors, and automatically deleted after 7 years due to compliance requirements.

**Requirements:**
- Encryption at rest with company-managed keys
- Encryption in transit (end-to-end)
- Access logged for compliance auditing
- Automatic deletion after 7 years
- Doctors must access via presigned URLs valid for 1 hour
- MOST secure solution

**Question:** Which solution meets ALL security and compliance requirements?

**A)** Use S3 with SSE-S3 encryption. Enable S3 lifecycle policy to delete after 7 years. Generate presigned URLs via Lambda. Use CloudTrail for logging.

**B)** Use S3 with SSE-KMS using customer-managed CMK. Enable S3 Object Lock with retention period of 7 years then delete. Generate presigned URLs with IAM roles. Enable S3 server access logging and CloudTrail.

**C)** Use S3 with SSE-KMS using customer-managed CMK. Create S3 lifecycle policy to expire objects after 7 years. Generate presigned URLs via API Gateway + Lambda with IAM roles. Enable CloudTrail S3 data events and S3 server access logging.

**D)** Use S3 with SSE-C (customer-provided keys). Use lifecycle policy for 7-year deletion. Generate presigned URLs via Lambda. Enable CloudTrail and VPC Flow Logs.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Why C is correct:**
- **SSE-KMS with CMK**: Company-managed keys (required by scenario)
- **Lifecycle policy**: Automatic deletion after 7 years (compliance requirement)
- **API Gateway + Lambda + IAM roles**: Proper access control and presigned URL generation
- **CloudTrail S3 data events**: Logs all S3 object-level API calls (PutObject, GetObject) for compliance
- **S3 server access logging**: Additional access logging
- **Encryption in transit**: HTTPS enforced by API Gateway

**Why others are wrong:**
- **A:** SSE-S3 uses AWS-managed keys, not company-managed (requirement violated)
- **B:** S3 Object Lock is for WORM (Write Once Read Many) protection against deletion, but question requires deletion after 7 years. Lifecycle policy is correct approach.
- **D:** SSE-C requires client to provide encryption key with every request (operational burden). VPC Flow Logs are for network traffic, not S3 access auditing.

**Key Exam Tips:**
- Company-managed keys = KMS CMK (Customer Managed Key)
- AWS-managed keys = SSE-S3 or SSE-KMS with AWS managed key
- Object Lock = prevent deletion/modification (compliance hold)
- Lifecycle policy = automatic transitions/deletions
- CloudTrail data events = S3 object-level logging for compliance
</details>

---

## Scenario 3: Hybrid Cloud Storage Solution
**Difficulty: Medium**

A manufacturing company has an on-premises application that generates 500 GB of data daily. The data must be backed up to AWS for disaster recovery. The company needs low-latency access to frequently accessed files (last 30 days) while older files can have retrieval times of up to 12 hours. The solution must minimize on-premises storage costs.

**Requirements:**
- Low-latency access to recent data (last 30 days)
- Older data can take up to 12 hours to retrieve
- Minimize on-premises storage
- Seamless integration with existing NFS-based applications
- Cost-effective long-term storage

**Question:** Which solution meets these requirements MOST cost-effectively?

**A)** Deploy AWS Storage Gateway (File Gateway) with S3 Intelligent-Tiering as the backend. Use lifecycle policy to move data older than 30 days to S3 Glacier Deep Archive.

**B)** Use AWS DataSync to sync data to S3 Standard. Configure lifecycle policy to transition to S3 Standard-IA after 30 days and S3 Glacier after 90 days.

**C)** Deploy AWS Storage Gateway (Volume Gateway in cached mode) with S3 Standard backend. Configure lifecycle policy to move to S3 Glacier Deep Archive after 30 days.

**D)** Deploy AWS Storage Gateway (File Gateway) with S3 Standard backend. Configure lifecycle policy to transition to S3 Standard-IA after 30 days and S3 Glacier Deep Archive after 90 days.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: D**

**Why D is correct:**
- **File Gateway**: Provides NFS interface for existing applications (seamless integration)
- **S3 Standard**: Low-latency access for recent data (last 30 days)
- **S3 Standard-IA**: Cost-effective for data 30-90 days old (can still access if needed)
- **S3 Glacier Deep Archive**: Cheapest storage for data >90 days old (retrieval up to 12 hours acceptable)
- **Local cache**: File Gateway caches frequently accessed files on-premises (low latency)
- **Lifecycle transitions**: Automatic cost optimization

**Why others are wrong:**
- **A:** S3 Intelligent-Tiering is more expensive than Standard when you have predictable access patterns. Direct transition from S3 to Glacier Deep Archive (skipping IA tier) wastes cost savings opportunity for 30-90 day old data that might occasionally be accessed.
- **B:** DataSync is for one-time or scheduled migrations, not continuous low-latency access. No local caching for frequently accessed files.
- **C:** Volume Gateway cached mode is for block storage (iSCSI), not NFS-based file applications.

**Key Exam Tips:**
- File Gateway = NFS/SMB file shares → S3
- Volume Gateway = iSCSI block storage → S3 (EBS snapshots)
- Tape Gateway = Virtual tape library → S3/Glacier
- Glacier Deep Archive = cheapest, 12-48 hour retrieval
- Lifecycle: Standard → Standard-IA → Glacier → Glacier Deep Archive (optimize costs)
</details>

---

## Scenario 4: Serverless Data Processing Pipeline
**Difficulty: Hard**

A media company receives video files uploaded by users throughout the day. Each video must be:
1. Scanned for inappropriate content
2. Transcoded to multiple formats (1080p, 720p, 480p)
3. Thumbnail generated
4. Metadata stored in a database
5. Users notified when processing completes

The solution must be fully serverless, cost-effective, and handle variable workloads (10-1000 videos/hour).

**Question:** Which architecture provides the MOST scalable and cost-effective solution?

**A)** S3 → SNS → Lambda (content scan) → SQS → Lambda (transcode) → Lambda (thumbnail) → DynamoDB → SNS (notify users)

**B)** S3 event → Lambda (orchestration) → Step Functions (workflow) → Lambda (content scan) + AWS Rekognition → MediaConvert (transcode) → Lambda (thumbnail) → DynamoDB → SNS (notify users)

**C)** S3 event → EventBridge → Lambda (content scan) → EventBridge → MediaConvert → EventBridge → Lambda (thumbnail + metadata) → SES (notify users)

**D)** S3 → SQS → Lambda (content scan) → Step Functions → ECS Fargate (transcode) → Lambda (thumbnail) → Aurora Serverless → SES (notify users)

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **S3 event to Lambda**: Immediate trigger when video uploaded
- **Step Functions**: Orchestrates complex multi-step workflow with proper error handling and retries
- **AWS Rekognition**: Managed service for content moderation (purpose-built for video analysis)
- **MediaConvert**: Managed transcoding service (handles multiple formats, fully serverless)
- **Lambda**: Lightweight tasks (thumbnail generation)
- **DynamoDB**: Serverless database for metadata
- **SNS**: Fanout notifications to multiple users
- **Fully serverless**: All components scale automatically from 10 to 1000 videos/hour
- **Cost-effective**: Pay only for what you use

**Why others are wrong:**
- **A:** Lambda for video transcoding is not recommended (15-minute timeout, not designed for heavy compute). No purpose-built services for content moderation.
- **C:** Lacks orchestration for complex workflow. EventBridge alone doesn't provide workflow state management, error handling, or retries like Step Functions. No mention of content scanning service.
- **D:** ECS Fargate is not fully serverless in the same way (requires container management). Aurora Serverless is overkill for simple metadata storage compared to DynamoDB.

**Key Exam Tips:**
- Step Functions = serverless workflow orchestration
- MediaConvert = video transcoding (not Lambda)
- Rekognition = image/video analysis, content moderation
- Lambda 15-min timeout (not for long-running tasks)
- EventBridge = event routing, Step Functions = workflow state management
</details>

---

## Scenario 5: Multi-Region Disaster Recovery
**Difficulty: Hard**

A financial services company runs a critical trading application on AWS. The application uses:
- Application servers on EC2
- RDS PostgreSQL database (500 GB)
- ElastiCache Redis cluster
- S3 for transaction logs

**Requirements:**
- RPO (Recovery Point Objective): 5 minutes
- RTO (Recovery Time Objective): 1 hour
- Primary region: us-east-1
- DR region: us-west-2
- Cost-effective DR solution

**Question:** Which DR strategy meets the RPO and RTO requirements MOST cost-effectively?

**A)** **Backup and Restore**: Schedule RDS snapshots every 5 minutes to us-west-2. Copy S3 data using cross-region replication. Store AMIs in us-west-2. In DR event, restore from backups.

**B)** **Pilot Light**: Maintain RDS read replica in us-west-2. Use S3 cross-region replication. Keep minimal EC2 instances (1 instance) in us-west-2. In DR event, scale up EC2 and promote read replica.

**C)** **Warm Standby**: Run scaled-down EC2 fleet in us-west-2 behind ALB (DNS weighted routing 0%). Maintain RDS read replica in us-west-2. Use S3 cross-region replication. Replicate ElastiCache data using Global Datastore. In DR event, scale up and change DNS weights.

**D)** **Multi-Site Active-Active**: Run full production environment in both regions with Route 53 failover routing. Use Aurora Global Database. Replicate ElastiCache with Global Datastore. Real-time synchronization.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C (Warm Standby)**

**Why C is correct:**
- **RPO 5 minutes**:
  - RDS read replica has near real-time replication (typically <1 minute lag)
  - S3 CRR replicates objects in minutes
  - ElastiCache Global Datastore provides sub-second replication
- **RTO 1 hour**:
  - EC2 instances already running (just need to scale up ~10-30 min)
  - RDS read replica promotion (~5-10 min)
  - DNS TTL update (5-60 min depending on TTL)
  - Total: ~30-60 minutes (meets requirement)
- **Cost-effective**: More expensive than Pilot Light but cheaper than Multi-Site. Only pays for scaled-down resources in DR region.

**Why others are wrong:**
- **A (Backup and Restore):**
  - RDS automated snapshots run hourly (can't meet 5-min RPO)
  - Even with manual snapshots every 5 min, restore time from snapshot takes 30+ min
  - Launching EC2 from AMIs, configuring, testing = 2-4 hours (violates RTO)
- **B (Pilot Light):**
  - RDS read replica meets RPO ✓
  - BUT: Starting from 1 minimal instance and scaling to full production, running application config, testing = 1.5-2 hours (violates 1-hour RTO)
  - Pilot Light typically has RTO of 2-4 hours
- **D (Multi-Site Active-Active):**
  - Meets RPO and RTO (best DR strategy) ✓
  - BUT: Not "MOST cost-effective" - running full production in both regions doubles costs
  - Only needed for RPO near-zero and RTO of minutes

**DR Strategy Comparison:**
| Strategy | RPO | RTO | Cost |
|----------|-----|-----|------|
| Backup/Restore | Hours | 24 hours | $ |
| Pilot Light | Minutes | 2-4 hours | $$ |
| Warm Standby | Minutes | 1 hour | $$$ |
| Multi-Site | Near-zero | Minutes | $$$$ |

**Key Exam Tips:**
- RPO = data loss tolerance (how old can recovered data be?)
- RTO = downtime tolerance (how fast must you recover?)
- Read Replica promotion = 5-10 minutes
- Snapshot restore = 30+ minutes (depending on size)
- Match DR strategy to RPO/RTO requirements, then choose most cost-effective
</details>

---

## Scenario 6: Decoupled Application Architecture
**Difficulty: Medium**

An application processes customer orders from a web application. During peak hours (Black Friday), the order processing system gets overwhelmed and drops orders, resulting in lost revenue. The company wants to ensure no orders are lost and that processing can catch up during off-peak hours.

**Current Architecture:**
- Web application → API → Order processing application (3 EC2 instances)
- Orders are processed synchronously in real-time

**Requirements:**
- Zero order loss (critical)
- Handle 10x traffic during peak hours
- Cost-effective during normal operations
- Processing can be delayed up to 1 hour during peaks

**Question:** Which solution ensures zero order loss MOST cost-effectively?

**A)** Implement an Application Load Balancer with connection draining. Scale EC2 processing instances to 30 during peak hours using Auto Scaling. Enable CloudWatch alarms for queue depth.

**B)** Place an SQS Standard queue between web application and order processors. Configure Auto Scaling to scale EC2 processors based on ApproximateNumberOfMessages metric. Set SQS message retention to 4 days.

**C)** Use Kinesis Data Streams to buffer orders. Scale EC2 consumers based on GetRecords.IteratorAgeMilliseconds metric. Store orders in DynamoDB before processing.

**D)** Implement API Gateway with throttling. Use SQS FIFO queue for order processing. Configure Lambda to process orders with reserved concurrency of 1000. Set dead-letter queue for failed messages.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **SQS Standard queue**: Acts as buffer, prevents order loss (messages stored for up to 14 days)
- **Decoupling**: Web app writes to queue (fast), processors read at their own pace
- **Auto Scaling based on queue depth**: Automatically scales processors when queue backs up during peaks
- **Cost-effective**: Only scales when needed, queue storage is cheap
- **Zero order loss**: SQS guarantees message durability with at-least-once delivery
- **4-day retention**: More than enough time to process backlog (requirement is 1-hour delay tolerance)

**Why others are wrong:**
- **A:** ALB and scaling help with traffic but don't prevent order loss during overload. Connection draining is for graceful shutdowns, not message buffering. Orders can still be dropped if all instances are maxed out before Auto Scaling kicks in.
- **C:**
  - Kinesis is overkill for this use case (designed for streaming analytics, more expensive)
  - Requires managing shards (more operational overhead)
  - "Store in DynamoDB before processing" adds unnecessary complexity and cost
  - Kinesis default retention is 24 hours (can extend to 365 days but adds cost)
- **D:**
  - FIFO queue is unnecessary (question doesn't require strict ordering)
  - FIFO has lower throughput (300 TPS) vs Standard (unlimited TPS with batching)
  - Lambda reserved concurrency costs more than EC2 with Auto Scaling for sustained processing
  - API throttling could cause order loss (defeats the requirement)

**Key Exam Tips:**
- SQS = decouple components, buffer messages, prevent data loss
- SQS Standard = at-least-once delivery, no ordering, unlimited throughput
- SQS FIFO = exactly-once processing, strict ordering, 300 TPS (or 3000 with batching)
- Auto Scaling with SQS metric = scale based on queue depth (perfect for variable workloads)
- Kinesis = real-time streaming analytics (overkill for simple queuing)
</details>

---

## Scenario 7: Cost Optimization for Predictable Workloads
**Difficulty: Medium**

A company runs a data analytics platform that processes batch jobs overnight (10 PM - 6 AM) every day. The workload is consistent and predictable. Currently using:
- 50 c5.4xlarge On-Demand instances running 24/7
- Annual cost: $350,000

**Requirements:**
- Reduce costs by at least 50%
- Maintain same processing performance
- Batch jobs must complete by 6 AM (8-hour window)

**Question:** Which approach provides the GREATEST cost savings while meeting requirements?

**A)** Purchase 50 c5.4xlarge Standard Reserved Instances (1-year, All Upfront). Maintain 24/7 operations.

**B)** Purchase 25 c5.4xlarge Convertible Reserved Instances (3-year, No Upfront). Use Auto Scaling to launch 25 additional Spot Instances during batch processing window (10 PM - 6 AM).

**C)** Use Auto Scaling to launch 50 c5.4xlarge Spot Instances only during batch window (10 PM - 6 AM). Implement Spot Instance interruption handling with checkpointing.

**D)** Use EC2 Instance Scheduler to start 50 c5.4xlarge On-Demand instances at 10 PM and stop at 6 AM daily. Purchase Savings Plan based on 8-hour daily usage.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: C**

**Why C is correct:**
- **Spot Instances**: Up to 90% discount vs On-Demand (massive savings)
- **Only running 8 hours/day**: 33% of time vs 24/7
- **Combined savings**: ~90% discount × 33% usage = ~97% cost reduction (easily exceeds 50% requirement)
- **Estimated cost**: ~$10,500/year (from $350,000)
- **Batch workload tolerance**: Batch jobs are fault-tolerant; if Spot interrupted, can resume from checkpoint
- **Risk mitigation**: Diversify Spot requests across multiple instance types and AZs
- **c5.4xlarge typically has low interruption rates** for batch windows during off-peak hours

**Why others are wrong:**
- **A:**
  - Reserved Instances save ~40-60% (doesn't meet "greatest savings")
  - Still running 24/7 when only need 8 hours/day (wasting 16 hours daily = 67% waste)
  - Savings: ~50% (good but not greatest)
- **B:**
  - Convertible RIs save less than Standard RIs (~30-45%)
  - No Upfront saves less than All Upfront
  - Only 25 RIs + 25 Spot = partial optimization
  - Still running 25 instances 24/7 (unnecessary)
  - Savings: ~60-70% (better but not greatest)
- **D:**
  - Instance Scheduler + Savings Plan = good approach
  - Savings Plan saves ~40-60% on 8-hour usage
  - Combined savings: ~55-65% (meets requirement but not greatest)
  - Still using On-Demand pricing (more expensive than Spot)

**Cost Breakdown Comparison:**
- Current: $350,000/year (50 On-Demand 24/7)
- Option A: ~$175,000/year (50 RIs 24/7) - 50% savings
- Option B: ~$120,000/year (25 RIs 24/7 + 25 Spot 8h/day) - 66% savings
- Option C: ~$10,500/year (50 Spot 8h/day only) - 97% savings ✓
- Option D: ~$140,000/year (50 On-Demand 8h/day + Savings Plan) - 60% savings

**Key Exam Tips:**
- Spot Instances = up to 90% discount (best for fault-tolerant, flexible workloads)
- Reserved Instances = 40-60% discount (predictable, steady-state workloads)
- Savings Plans = 40-60% discount (flexible across instance types/regions)
- "GREATEST cost savings" = look for Spot Instances if workload can tolerate interruptions
- Batch processing = typically fault-tolerant = great for Spot
- Only run resources when needed (8 hours vs 24 hours = 3x cost savings)
</details>

---

## Scenario 8: Database Migration Strategy
**Difficulty: Hard**

A company wants to migrate an on-premises Oracle database (2 TB) to AWS. The database supports a critical application that must remain online during migration. The company wants to move to a managed database service and minimize licensing costs.

**Requirements:**
- Zero downtime during migration
- Minimize Oracle licensing costs
- Managed service (no server management)
- Maintain compatibility with existing application (minimal code changes)

**Question:** Which migration path meets these requirements?

**A)** Migrate to RDS for Oracle using AWS Database Migration Service (DMS) with change data capture (CDC). After migration, optimize costs by right-sizing the instance.

**B)** Migrate to Aurora PostgreSQL using AWS DMS with Schema Conversion Tool (SCT). Use DMS CDC for continuous replication during migration. Cutover during maintenance window.

**C)** Create Oracle database on EC2. Use Oracle Data Pump to migrate. After migration, migrate to RDS for Oracle. Then convert to Aurora PostgreSQL using DMS and SCT.

**D)** Use AWS DMS to migrate directly to Aurora MySQL. Modify application to support MySQL syntax and stored procedures. Use blue/green deployment for cutover.

<details>
<summary>Answer & Explanation</summary>

**Correct Answer: B**

**Why B is correct:**
- **Aurora PostgreSQL**:
  - Managed service (no server management) ✓
  - No Oracle licensing fees (significant cost savings) ✓
  - PostgreSQL is most compatible with Oracle (compared to MySQL)
- **AWS DMS with SCT**:
  - SCT converts Oracle schema to PostgreSQL
  - DMS migrates data with CDC (continuous replication)
  - Zero downtime migration (application keeps running) ✓
- **Minimal code changes**:
  - PostgreSQL has similar features to Oracle (stored procedures, triggers, etc.)
  - SCT identifies incompatibilities before migration
  - More compatible than MySQL for Oracle migrations
- **Managed service**: Aurora is fully managed ✓

**Why others are wrong:**
- **A:**
  - RDS for Oracle still requires Oracle licensing (BYOL or License Included)
  - Doesn't "minimize Oracle licensing costs" - still paying Oracle
  - Right-sizing reduces compute costs but not licensing costs
  - Doesn't meet the cost optimization requirement
- **C:**
  - Multi-step migration adds complexity and risk
  - Oracle on EC2 = not managed service (you manage OS, patches, backups)
  - Oracle Data Pump requires downtime for cutover (violates zero downtime requirement)
  - Still incurs Oracle licensing costs for intermediate steps
  - Unnecessary complexity (can migrate directly with DMS)
- **D:**
  - MySQL is less compatible with Oracle than PostgreSQL
  - Oracle → MySQL migration requires significant code changes (stored procedures, PL/SQL → MySQL syntax)
  - Violates "minimal code changes" requirement
  - Higher risk and effort

**Key Exam Tips:**
- Oracle migration to reduce licensing costs → PostgreSQL (not MySQL, not RDS Oracle)
- Aurora PostgreSQL = most compatible open-source alternative to Oracle
- AWS DMS = online migration with CDC (zero downtime)
- AWS SCT = schema conversion (Oracle → PostgreSQL, SQL Server → MySQL, etc.)
- RDS for Oracle = still pay Oracle licensing (doesn't solve cost problem)
</details>

---

## Practice Tips for These Scenarios:

1. **Identify keywords**: "MOST cost-effective", "LEAST operational overhead", "MOST secure", "GREATEST savings"
2. **Eliminate wrong answers**: Often 1-2 options have obvious flaws
3. **Map requirements to AWS services**: Each requirement typically maps to a specific service feature
4. **Watch for over-engineering**: Simpler solutions often win if they meet requirements
5. **Know service limits**: Lambda timeout, RDS snapshot frequency, S3 retrieval times, etc.
6. **Cost hierarchy**: Spot > Savings Plans > Reserved Instances > On-Demand
7. **DR strategies**: Match RPO/RTO requirements to correct strategy, then choose most cost-effective

---

## Additional Practice Questions to Try:

Create your own scenarios based on:
- **VPC and Networking**: Multi-tier architectures, hybrid connectivity, security groups vs NACLs
- **Storage**: When to use S3 vs EFS vs EBS vs FSx
- **Compute**: When to use EC2 vs Lambda vs ECS vs EKS
- **Databases**: RDS vs DynamoDB vs Aurora vs Redshift use cases
- **Security**: IAM policies, encryption, compliance scenarios

Would you like me to create more scenarios in specific domains?
