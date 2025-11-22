# Serverless Architecture Patterns

Common serverless patterns that appear on the SAA-C03 exam. Master these and you'll ace serverless questions.

---

## Pattern 1: API Backend (REST API)

### Architecture
```
User/App
   ↓
API Gateway (REST or HTTP API)
   ↓
Lambda (business logic)
   ↓
DynamoDB (data storage)
```

### Components
- **API Gateway**: HTTP endpoint, throttling, caching, authentication
- **Lambda**: Serverless compute (process requests)
- **DynamoDB**: NoSQL database (scales automatically)

### When to Use
- Mobile/web app backend
- Microservices API
- Serverless applications

### Enhancements
**Authentication:**
- Cognito User Pools (user authentication)
- Lambda Authorizer (custom auth logic)
- IAM roles (service-to-service)

**Caching:**
- API Gateway caching (reduce Lambda invocations)
- DynamoDB DAX (microsecond latency for reads)

**Example Use Case:**
Mobile app for todo list:
- `POST /todos` → Lambda creates todo → DynamoDB
- `GET /todos` → Lambda retrieves todos → DynamoDB
- API Gateway throttles requests, Cognito authenticates users

### Cost Optimization
- Use HTTP API instead of REST API (70% cheaper)
- Enable API Gateway caching (reduce Lambda invocations)
- Use DynamoDB On-Demand for unpredictable traffic

---

## Pattern 2: Event-Driven Processing (S3 Upload)

### Architecture
```
User uploads file to S3
   ↓
S3 Event Notification
   ↓
Lambda (process file)
   ↓
Store results (DynamoDB, S3, etc.)
```

### Components
- **S3**: Object storage (trigger on PUT)
- **Lambda**: Process file (resize image, parse CSV, etc.)
- **DynamoDB/S3**: Store processed data

### When to Use
- Image/video processing (thumbnail generation)
- File transformation (CSV to JSON)
- Data validation on upload

### Enhancements
**Large Files (>15 min processing):**
- S3 → Lambda → Submit to AWS Batch (long-running jobs)
- S3 → EventBridge → Step Functions → ECS Fargate

**Fan-out (multiple processors):**
- S3 → SNS → Multiple Lambda functions (parallel processing)
- Example: Upload image → SNS → [Thumbnail Lambda, Metadata Lambda, AI Analysis Lambda]

**Error Handling:**
- Lambda Dead Letter Queue (DLQ) → SQS or SNS (for failed invocations)
- S3 versioning (recover from processing errors)

### Example Use Case
User uploads profile photo:
1. Upload to S3
2. S3 triggers Lambda
3. Lambda resizes image (thumbnail, medium, large)
4. Lambda stores resized images in S3
5. Lambda updates DynamoDB (user profile with image URLs)

---

## Pattern 3: Asynchronous Processing (Queue)

### Architecture
```
Producer (app, service)
   ↓
SQS Queue (buffer)
   ↓
Lambda (consumer, processes messages)
   ↓
Store results
```

### Components
- **SQS**: Message queue (decouple producer and consumer)
- **Lambda**: Event source mapping (polls SQS)
- **DynamoDB/S3**: Store results

### When to Use
- Decouple components (producer doesn't wait for processing)
- Handle traffic spikes (queue buffers messages)
- Retry logic (SQS retries failed messages)

### Enhancements
**FIFO Queue (ordering):**
- Use SQS FIFO for strict ordering
- Example: Process bank transactions in order

**Dead Letter Queue:**
- Messages that fail after max retries → DLQ
- Investigate failed messages separately

**Batch Processing:**
- Lambda batch size (process 1-10 messages per invocation)
- Reduces Lambda invocations, lowers cost

### Example Use Case
E-commerce order processing:
1. Web app places order → SQS
2. SQS buffers orders (handles Black Friday spike)
3. Lambda processes orders at its own pace
4. Lambda updates inventory (DynamoDB), sends confirmation email (SNS)

**Benefits:**
- Order placement is fast (doesn't wait for processing)
- No order loss (SQS retains messages for 4-14 days)
- Auto-scales (Lambda scales to queue depth)

---

## Pattern 4: Fan-Out (Pub/Sub)

### Architecture
```
Publisher
   ↓
SNS Topic
   ↓ (fan-out to multiple subscribers)
   ├→ SQS Queue 1 → Lambda 1 (email service)
   ├→ SQS Queue 2 → Lambda 2 (analytics)
   └→ Lambda 3 (logging)
```

### Components
- **SNS**: Publish message once, deliver to multiple subscribers
- **SQS**: Buffer for each subscriber (reliable delivery)
- **Lambda**: Process messages independently

### When to Use
- One event, multiple actions
- Decouple microservices
- Parallel processing

### Example Use Case
User creates account:
1. App publishes "UserCreated" event → SNS
2. SNS fans out to:
   - SQS → Lambda (send welcome email)
   - SQS → Lambda (create CRM record)
   - SQS → Lambda (log to analytics)
3. Each Lambda processes independently

**Benefits:**
- Each subscriber processes at its own pace
- Add new subscribers without changing publisher
- Failures in one subscriber don't affect others

### SNS Filtering
- Subscribers can filter messages by attributes
- Example: Email service only receives messages where `event_type = "user_signup"`

---

## Pattern 5: Scheduled Tasks (Cron Jobs)

### Architecture
```
EventBridge (CloudWatch Events)
   ↓ (scheduled rule: cron or rate)
Lambda (execute task)
   ↓
Perform action (cleanup, reports, etc.)
```

### Components
- **EventBridge**: Scheduled events (cron expressions)
- **Lambda**: Execute task

### When to Use
- Daily reports
- Cleanup old data
- Scheduled backups
- Polling external APIs

### Examples

**Daily Report (every day at 9 AM):**
```
EventBridge rule: cron(0 9 * * ? *)
   → Lambda (query DynamoDB, generate report, send via SES)
```

**Cleanup old DynamoDB items (every hour):**
```
EventBridge rule: rate(1 hour)
   → Lambda (scan DynamoDB, delete items older than 30 days)
```

**Note:** DynamoDB TTL is better for automatic deletion (use Lambda for custom logic)

### EventBridge vs CloudWatch Events
- **EventBridge**: Successor to CloudWatch Events (more features)
- **Same cron syntax**: `cron(minutes hours day month day-of-week year)`
- **Rate syntax**: `rate(1 hour)`, `rate(5 minutes)`

---

## Pattern 6: Real-Time Stream Processing

### Architecture
```
Data Source (app, IoT)
   ↓
Kinesis Data Streams
   ↓
Lambda (process events)
   ↓
Store aggregated data (DynamoDB, S3)
```

### Components
- **Kinesis Data Streams**: Real-time data ingestion
- **Lambda**: Process events (aggregate, filter, transform)
- **DynamoDB/S3**: Store processed data

### When to Use
- Real-time analytics (clickstream, metrics)
- IoT data processing
- Log aggregation

### Example Use Case
Real-time website analytics:
1. Web app sends clickstream events → Kinesis
2. Lambda processes events (count page views, sessions)
3. Lambda updates DynamoDB (real-time dashboard)

### Enhancements

**Store Raw + Processed Data:**
```
Kinesis Data Streams
   ↓ (fan-out to 2 consumers)
   ├→ Kinesis Firehose → S3 (raw data, compliance)
   └→ Lambda → DynamoDB (processed data, real-time queries)
```

**Windowed Aggregation:**
- Lambda processes events in batches (e.g., every 10 seconds)
- Aggregate metrics (count events, sum values)
- Write to DynamoDB (time-series data)

---

## Pattern 7: Step Functions (Workflow Orchestration)

### Architecture
```
Trigger (API Gateway, EventBridge, S3)
   ↓
Step Functions State Machine
   ↓ (orchestrates workflow)
   ├→ Lambda 1 (validate input)
   ├→ Lambda 2 (process data)
   ├→ Lambda 3 (send notification)
   └→ (parallel, retry, error handling)
```

### Components
- **Step Functions**: Orchestrate multi-step workflows
- **Lambda**: Individual tasks
- **State Machine**: Define workflow (JSON)

### When to Use
- Multi-step workflows (order processing, approval workflows)
- Long-running processes (hours/days)
- Complex error handling, retries
- Parallel execution (run multiple tasks simultaneously)

### Step Functions States
| State | Purpose |
|-------|---------|
| **Task** | Execute Lambda, ECS, Batch, etc. |
| **Choice** | Conditional logic (if/else) |
| **Parallel** | Execute multiple branches simultaneously |
| **Wait** | Delay for specified time |
| **Succeed** | End workflow successfully |
| **Fail** | End workflow with error |

### Example Use Case
E-commerce order fulfillment:
1. API Gateway → Step Functions (new order)
2. Step Functions orchestrates:
   - Validate payment (Lambda)
   - Check inventory (Lambda)
   - If in stock:
     - Reserve inventory (Lambda)
     - Notify warehouse (SNS)
     - Send confirmation email (SES)
   - If out of stock:
     - Notify customer (SNS)
     - End workflow

**Benefits:**
- Visual workflow (see progress in console)
- Automatic retries (configure retry logic per task)
- Error handling (catch failures, run compensation logic)
- Audit trail (CloudWatch Logs)

### Step Functions vs SQS
| Feature | Step Functions | SQS |
|---------|---------------|-----|
| **Use Case** | Complex workflows, orchestration | Simple queuing, decoupling |
| **Visibility** | Visual workflow, state tracking | Message queue (no workflow) |
| **Error Handling** | Built-in retries, catch errors | Manual (DLQ) |
| **Cost** | Per state transition | Per message |

---

## Pattern 8: API Gateway + Lambda Authorizer

### Architecture
```
User
   ↓ (includes auth token)
API Gateway
   ↓ (before invoking backend)
Lambda Authorizer (validate token)
   ↓ (returns IAM policy)
API Gateway
   ↓ (if authorized)
Lambda (backend logic)
```

### Components
- **API Gateway**: HTTP endpoint
- **Lambda Authorizer**: Custom authentication logic
- **Backend Lambda**: Business logic

### When to Use
- Custom authentication (OAuth, SAML, proprietary)
- Validate JWT tokens
- Check user permissions in external system

### Lambda Authorizer Types
| Type | Input | Output |
|------|-------|--------|
| **Token-based** | Bearer token (Authorization header) | IAM policy (allow/deny) |
| **Request-based** | Headers, query params, context | IAM policy |

### Example Use Case
API with OAuth tokens:
1. User calls API with OAuth token in header
2. API Gateway invokes Lambda Authorizer
3. Lambda Authorizer validates token with OAuth provider
4. Lambda Authorizer returns IAM policy (allow or deny)
5. API Gateway caches policy (1 hour, reduces authorizer invocations)
6. If allowed, invoke backend Lambda

**Benefits:**
- Custom auth logic (not limited to Cognito or IAM)
- Caching (reduce authorizer invocations, lower cost)
- Flexibility (integrate with any auth system)

---

## Pattern 9: Cognito for User Authentication

### Architecture
```
Mobile/Web App
   ↓ (sign up, sign in)
Cognito User Pool (user directory)
   ↓ (returns JWT token)
App includes token in API request
   ↓
API Gateway (validates JWT with Cognito)
   ↓
Lambda (business logic)
   ↓
DynamoDB
```

### Components
- **Cognito User Pool**: User directory (sign-up, sign-in, MFA)
- **API Gateway**: Validates JWT tokens (integrated with Cognito)
- **Lambda + DynamoDB**: Backend

### When to Use
- Mobile/web app user authentication
- Social login (Google, Facebook, Apple)
- MFA, password reset, email verification

### Cognito Features
- **User Pools**: User directory, authentication
- **Identity Pools**: Provide AWS credentials (access S3, DynamoDB directly)
- **Hosted UI**: Pre-built sign-in page
- **Triggers**: Lambda triggers for custom logic (pre-signup, post-authentication)

### Example Flow
1. User signs up → Cognito User Pool
2. Cognito sends verification email
3. User signs in → Cognito returns JWT token
4. App calls API Gateway with JWT token in `Authorization` header
5. API Gateway validates JWT with Cognito (no Lambda Authorizer needed)
6. Lambda processes request

**Benefits:**
- Fully managed authentication (no custom auth code)
- MFA, password policies, account recovery
- Social login (federated identity)

---

## Pattern 10: Direct S3 Upload (Presigned URLs)

### Architecture
```
Client (web app)
   ↓ (request presigned URL)
Lambda (generates presigned URL)
   ↓ (returns URL)
Client uploads file directly to S3 (bypasses API Gateway)
   ↓
S3 Event Notification → Lambda (process file)
```

### When to Use
- Large file uploads (videos, images)
- Reduce API Gateway/Lambda invocations (cheaper)
- Client uploads directly to S3 (faster)

### How It Works
1. Client requests presigned URL from Lambda
2. Lambda generates presigned URL (using AWS SDK, S3 presigned URL API)
   - URL is time-limited (e.g., valid for 15 minutes)
   - Grants temporary permission to upload to specific S3 key
3. Client uploads file directly to S3 using presigned URL
4. S3 triggers Lambda to process file

**Benefits:**
- No file size limits (API Gateway has 10 MB limit)
- Faster uploads (direct to S3, no API Gateway hop)
- Cheaper (no data transfer through API Gateway/Lambda)

### Example Code (Lambda generates presigned URL)
```python
import boto3
s3_client = boto3.client('s3')

url = s3_client.generate_presigned_url(
    'put_object',
    Params={'Bucket': 'my-bucket', 'Key': 'uploads/file.jpg'},
    ExpiresIn=900  # 15 minutes
)
return {'upload_url': url}
```

---

## Pattern 11: Lambda Layers (Code Reuse)

### Architecture
```
Lambda Function 1 ─┐
Lambda Function 2 ─┼→ Lambda Layer (shared code, libraries)
Lambda Function 3 ─┘
```

### When to Use
- Share code across multiple Lambda functions
- Reduce deployment package size
- Separate dependencies from function code

### Lambda Layers
- **What**: ZIP archive with libraries, custom code, or data
- **Limit**: Max 5 layers per function, 250 MB total (unzipped)
- **Versions**: Layers are versioned (immutable)

### Example Use Cases
**Shared Utility Functions:**
- Create layer with common utility functions (logging, validation)
- Attach to all Lambda functions

**Large Dependencies:**
- Python: NumPy, Pandas, Matplotlib (large libraries)
- Node.js: AWS SDK, Axios
- Upload as layer (reduces deployment package size per function)

**Custom Runtime:**
- Use layers to support custom runtimes (e.g., PHP, Rust)

**Benefits:**
- Smaller deployment packages (faster deploys)
- Code reuse (update layer, all functions get update)
- Organize code (separate business logic from dependencies)

---

## Pattern 12: Lambda Destinations (Success/Failure Routing)

### Architecture
```
Lambda (async invocation)
   ↓
Success? ─┬→ Yes → SQS (success queue)
          └→ No  → SNS (failure topic) → Email alert
```

### When to Use
- Route results based on success/failure (asynchronous invocations)
- Avoid polling for Lambda results
- Better than Dead Letter Queues (DLQ)

### Lambda Destinations
- **OnSuccess**: SQS, SNS, Lambda, EventBridge (where to send result on success)
- **OnFailure**: SQS, SNS, Lambda, EventBridge (where to send result on failure)

### Example Use Case
Image processing pipeline:
1. S3 upload triggers Lambda (resize image)
2. Lambda success → SQS (queue for next step: AI analysis)
3. Lambda failure → SNS (alert operations team)

**Destinations vs DLQ:**
- **Destinations**: Route success AND failure, more flexible
- **DLQ**: Only route failures (SQS or SNS)

---

## Serverless Cost Optimization Tips

### Lambda
- **Right-size memory**: Test different memory settings (higher memory = faster CPU, but more cost)
- **Reduce invocations**: Use API Gateway caching, SQS batching
- **Provisioned Concurrency**: Only use for latency-sensitive apps (costs more)

### API Gateway
- **Use HTTP API** instead of REST API (70% cheaper)
- **Enable caching**: Reduce backend invocations
- **Throttling**: Prevent abuse, control costs

### DynamoDB
- **On-Demand vs Provisioned**: On-Demand for unpredictable, Provisioned + Auto Scaling for predictable
- **DAX caching**: Reduce read costs (for hot data)
- **TTL**: Auto-delete old items (no cost)

### S3
- **Lifecycle policies**: Transition to cheaper storage classes
- **Intelligent-Tiering**: Auto-optimize costs for unknown access patterns

---

## Common Exam Scenarios

### "Mobile app needs user authentication" → **Cognito User Pools**
- Sign-up, sign-in, MFA
- Integrate with API Gateway

### "Process S3 uploads" → **S3 Event → Lambda**
- Serverless, event-driven

### "REST API with database" → **API Gateway + Lambda + DynamoDB**
- Fully serverless backend

### "Decouple components" → **SQS + Lambda**
- Buffer messages, handle spikes

### "One event, multiple actions" → **SNS (fan-out) + SQS + Lambda**
- Parallel processing

### "Multi-step workflow" → **Step Functions**
- Orchestrate complex workflows

### "Large file upload" → **Presigned URLs (direct S3 upload)**
- Bypass API Gateway 10 MB limit

### "Real-time stream processing" → **Kinesis Data Streams + Lambda**
- Process events in real-time

### "Scheduled tasks" → **EventBridge + Lambda**
- Cron jobs, periodic tasks

### "Custom authentication" → **Lambda Authorizer**
- OAuth, custom logic

---

## Key Exam Tips

### Lambda Limits (MEMORIZE!)
- **Timeout**: 15 minutes max
- **Memory**: 128 MB - 10 GB
- **Deployment package**: 50 MB (zipped), 250 MB (unzipped)
- **Concurrent executions**: 1,000 (account limit, can increase)

### When NOT to Use Lambda
- **Long-running tasks** (>15 minutes) → Use ECS, Batch, EC2
- **Stateful applications** → Use ECS, EC2
- **Large deployment packages** (>250 MB) → Use containers (ECS, EKS)

### API Gateway Limits
- **Payload size**: 10 MB max
- **Timeout**: 29 seconds max
- **Throttle**: 10,000 req/sec (default, can increase)

### DynamoDB Best Practices
- **Use partition key wisely**: Distribute data evenly (avoid hot partitions)
- **GSI for queries**: Query on non-key attributes
- **DynamoDB Streams**: Trigger Lambda on item changes

### Cost Hierarchy (Cheapest to Most Expensive)
1. **S3** (storage)
2. **DynamoDB On-Demand** (low traffic)
3. **Lambda** (pay per invocation)
4. **API Gateway HTTP API**
5. **API Gateway REST API**
6. **DynamoDB Provisioned** (high traffic)
7. **Fargate** (containers)
8. **EC2** (always running)

---

Now you have solid serverless patterns for the exam. Practice building these in your AWS account (Free Tier)! 🚀
