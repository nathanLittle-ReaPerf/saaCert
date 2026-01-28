# Day 8 Lambda Storage Flashcards

**Created:** January 14, 2026
**Purpose:** Drill Lambda + External Data Sources patterns until 100% mastery

---

## Set 1: /tmp Storage - When It Works vs Fails

### Card 1
**Q:** A Lambda function needs to cache a 400 MB lookup table that's updated once per day. The function receives 50 requests/minute with no strict SLA. What storage should you use?

**A:** **/tmp ephemeral storage**

**Why:**
- ✅ 400 MB < 10 GB (fits in Lambda)
- ✅ Updated daily = infrequent
- ✅ 50 req/min = low-moderate traffic
- ✅ No strict SLA = cold starts acceptable
- ✅ Most cost-effective (~$0/month vs $30-50/month ElastiCache)

---

### Card 2
**Q:** A Lambda function authenticates requests using a 300 MB revocation list. The list is updated every 5 minutes. Traffic is 10,000 requests/minute with a 50ms SLA. What storage should you use?

**A:** **ElastiCache for Redis**

**Why /tmp FAILS:**
- ❌ 50ms SLA violated by 5-8 second cold starts
- ❌ 10,000 req/min = constant new containers = constant cold starts
- ❌ Updated every 5 minutes = frequent updates
- ❌ Question explicitly states cold start approach is FAILING

**Why ElastiCache works:**
- ✅ Sub-millisecond lookups (<1ms)
- ✅ No cold start impact (external to Lambda)
- ✅ All containers share same cache
- ✅ Update once every 5 min, all see instantly

---

### Card 3
**Q:** What are the FOUR scenarios where /tmp caching FAILS?

**A:**
1. **Strict SLA (<100ms)** that cold starts violate
2. **High request rate (1000s/sec)** causing constant new containers
3. **Frequent updates (every few minutes)** requiring complex cache invalidation
4. **Shared across multiple Lambda functions** (each downloads own copy)

---

### Card 4
**Q:** A Lambda processes 80 MB log files temporarily during execution. Files are not needed after processing. Current /tmp is 512 MB. What should you do?

**A:** **Configure /tmp storage to 1024 MB (1 GB)**

**Why NOT EFS:**
- ❌ Over-engineering simple temporary storage
- ❌ EFS costs money + adds VPC complexity
- ❌ Network latency for temporary processing
- ✅ /tmp is free, local, fast, perfect for < 10 GB temporary data

---

## Set 2: Lambda Storage Limits

### Card 5
**Q:** What is the Lambda /tmp storage limit?

**A:** **512 MB to 10 GB (configurable)**

**Key facts:**
- Default: 512 MB
- Maximum: 10 GB
- Ephemeral (lost when container terminates)
- Survives across warm invocations
- Billed at $0.0000000309/GB-second

---

### Card 6
**Q:** A Lambda function needs to cache a 2 GB ML model + 3 GB feature dataset for inference. Latency requirement is 100ms. What storage approach?

**A:** **Lambda memory (6 GB) + /tmp caching**

**Solution:**
1. Configure Lambda memory: 6 GB (5 GB data + 1 GB overhead)
2. On cold start: Download model + features from S3 to /tmp
3. Load into Lambda memory
4. Reuse across warm invocations

**Why NOT ElastiCache + EFS split:**
- ❌ ML inference requires ALL data in-memory
- ❌ Can't fetch features from Redis during tensor operations
- ❌ 4 GB insufficient for 5 GB total data

---

### Card 7
**Q:** What is Lambda's maximum memory limit?

**A:** **10 GB**

**Critical limits to memorize:**
- Memory: 128 MB to 10 GB
- Timeout: 15 minutes maximum
- /tmp storage: 512 MB to 10 GB
- Deployment package: 50 MB zipped, 250 MB unzipped
- Lambda layers: 250 MB total (all layers combined)

---

## Set 3: Data Size Decision Tree

### Card 8
**Q:** Dataset is 15 GB with 200ms latency requirement and sorted set lookups. Lambda or external storage?

**A:** **ElastiCache for Redis (external storage)**

**Why:**
- ❌ 15 GB > 10 GB Lambda limit
- ✅ ElastiCache Redis supports sorted sets
- ✅ Sub-millisecond latency (meets 200ms easily)
- ❌ NOT EFS: Network filesystem too slow for 200ms sorted lookups

---

### Card 9
**Q:** What's the decision threshold for "must use external storage"?

**A:** **Dataset > 10 GB**

**Decision tree:**
```
Dataset size?
├─ < 10 GB → CAN use Lambda memory/tmp (check other factors)
└─ > 10 GB → MUST use external storage
   ├─ Sub-second latency + key-value → ElastiCache
   └─ File operations → EFS
```

---

### Card 10
**Q:** Dataset is 6 GB ML model. Lambda function needs to load it for inference. What approach?

**A:** **Lambda + EFS mount**

**Why:**
- ✅ 6 GB fits in Lambda (< 10 GB)
- ✅ EFS allows one-time download, persistent across invocations
- ✅ No cold start download delay after first load
- ❌ NOT /tmp alone: Would download 6 GB on every cold start (slow)
- ✅ EFS = download once, reuse forever

---

## Set 4: DynamoDB vs RDS Architecture

### Card 11
**Q:** Does DynamoDB have connection limits?

**A:** **NO - DynamoDB is connectionless (HTTP REST API)**

**Critical distinction:**
- **DynamoDB:** Connectionless HTTP → No connection limits
- **RDS/Aurora:** Connection-based → Has limits (150-5,000 depending on instance)
- **ElastiCache:** Connection-based → Has limits (high, but exists)

---

### Card 12
**Q:** Lambda + DynamoDB is experiencing `ProvisionedThroughputExceededException` but table has sufficient RCU/WCU provisioned. What's the cause?

**A:** **Hot partition issue (uneven key distribution)**

**Why NOT connection limits:**
- ❌ DynamoDB has NO connections (it's HTTP/REST)
- ✅ "Sufficient capacity but throttling" = hot partition
- ✅ Adaptive capacity hasn't kicked in yet (5-30 min delay)

**Solutions:**
- Design better partition key (higher cardinality)
- Cache hot items in ElastiCache/DAX
- DynamoDB auto-scaling
- Pre-warm with dummy requests

---

### Card 13
**Q:** Lambda + RDS MySQL experiencing "Too many connections" errors. What's the solution?

**A:** **RDS Proxy (connection pooling)**

**Why connection pooling in Lambda code FAILS:**
- 500 Lambda instances × 5 connections per pool = 2,500 total connections
- Each Lambda instance is isolated (doesn't share pools)
- Makes problem WORSE, not better

**RDS Proxy benefits:**
- Multiplexes connections (500 Lambda → 50 RDS connections)
- Connection pooling across ALL Lambda instances
- Reduces connection overhead

---

## Set 5: ML Inference Architecture

### Card 14
**Q:** Can you split an ML model between ElastiCache (features) and EFS (model weights) for Lambda inference?

**A:** **NO - ML inference requires ALL data in-memory**

**Why this is architecturally broken:**
- ❌ Tensor operations require local in-memory data
- ❌ Fetching features from Redis: 1-5ms × 500 features = 500-2500ms
- ❌ Loading model from EFS during request: 5-10 seconds
- ❌ Can't meet sub-second latency requirements

**Correct approach:**
- Load EVERYTHING into Lambda memory
- Download from S3 to /tmp on cold start
- Keep in memory for warm invocations

---

### Card 15
**Q:** ML model + features = 12 GB total. Can Lambda handle this?

**A:** **NO - Lambda maximum is 10 GB memory**

**Alternatives:**
- SageMaker Endpoint (purpose-built for ML inference)
- ECS Fargate with larger memory
- EC2 with large instance types

**If < 10 GB total:**
- ✅ Lambda with sufficient memory
- Download to /tmp on cold start
- Load to memory for inference

---

## Set 6: Reading Comprehension

### Card 16
**Q:** A scenario describes "currently using /tmp caching, but authentication is failing SLA during traffic spikes." What should you NOT choose?

**A:** **/tmp caching (the explicitly failing solution)**

**Critical rule:**
1. Read ENTIRE scenario
2. Identify what's currently being used
3. Identify WHY it's failing
4. NEVER choose solution that matches described failure

**This question literally told you /tmp was broken!**

---

### Card 17
**Q:** Scenario states: "Cold starts take 5-8 seconds, violating 50ms SLA." Is /tmp caching appropriate?

**A:** **NO - cold starts violate the SLA**

**Why /tmp fails here:**
- Cold start downloads take 5-8 seconds
- 50ms SLA × 8000ms cold start = 160x violation
- High traffic creates constant new containers = constant violations

**Solution:** ElastiCache (no cold start impact)

---

## Set 7: Cost vs Performance Tradeoffs

### Card 18
**Q:** 500 MB static lookup table, updated hourly, no strict SLA, keyword "MOST cost-effective." ElastiCache or /tmp?

**A:** **/tmp ephemeral storage**

**Cost comparison:**
- /tmp: ~$0.01/month (negligible)
- ElastiCache: ~$30-50/month (cache.t3.micro)

**Why /tmp works:**
- ✅ 500 MB < 10 GB
- ✅ Hourly updates = infrequent
- ✅ No strict SLA = cold starts OK
- ✅ "MOST cost-effective" = prefer free over $30/month

**When to use ElastiCache instead:**
- SLA requires sub-second response
- High request rate (1000s/sec)
- Updated frequently (every few minutes)

---

### Card 19
**Q:** Authentication system with 50ms SLA, 10K requests/min, 300 MB dataset updated every 5 minutes. /tmp costs $0, ElastiCache costs $30/month. Which should you choose?

**A:** **ElastiCache ($30/month for working solution)**

**Why cost isn't the deciding factor:**
- /tmp is "cheaper" but DOESN'T WORK (violates SLA)
- SLA violations = business cost of failed authentications
- $30/month for working solution > $0 for broken solution

**The exam trap:**
- Cheap solution that fails requirements
- Must read constraints carefully (SLA, traffic, update frequency)

---

## Set 8: Lambda Limits Quick Fire

### Card 20
**Q:** Lambda timeout limit?

**A:** **15 minutes maximum (900 seconds)**

If task > 15 minutes → Use ECS Fargate, EC2, or AWS Batch (NOT Lambda)

---

### Card 21
**Q:** Lambda deployment package limits?

**A:**
- **Zipped:** 50 MB max
- **Unzipped:** 250 MB max
- **Lambda layers:** 250 MB total (all layers + package combined)

For files > 250 MB → Use /tmp download from S3 or EFS mount

---

### Card 22
**Q:** Lambda concurrent execution default limit?

**A:** **1,000 concurrent executions** (soft limit, can request increase)

**Burst limit:** +500 executions per minute across all functions

---

## Practice Scenario

### Card 23
**Q:** Lambda function processes user uploads. Files are 50-200 MB each. Processing takes 30 seconds. Files are deleted after processing. Current /tmp is 512 MB. Users report intermittent "No space left on device" errors. What should you do?

**A:** **Configure /tmp storage to 1024 MB (1 GB) or 2048 MB (2 GB)**

**Analysis:**
- Files: 50-200 MB (fits in Lambda < 10 GB)
- Temporary processing: Perfect /tmp use case
- Deleted after: Ephemeral storage appropriate
- Error: 512 MB too small for 200 MB files + processing overhead
- Solution: Increase /tmp to 1-2 GB

**Why NOT EFS:**
- Over-engineering temporary storage
- Adds cost and complexity
- /tmp is perfect for < 10 GB temporary files

---

## Drilling Instructions

**For maximum retention:**

1. **Shuffle and drill:** Don't go in order - randomize to avoid memorizing position
2. **Answer out loud:** Forces you to articulate the full reasoning
3. **Cover the answer:** Test yourself before revealing
4. **Mark misses:** Any card you miss gets drilled again immediately
5. **Daily review:** Run through ALL cards once per day until exam
6. **Speed rounds:** Once you know them, answer within 5 seconds
7. **Teach someone:** Explain each concept to another person (or the wall)

**Mastery criteria:**
- ✅ 100% correct 3 days in a row
- ✅ Can answer within 5 seconds
- ✅ Can explain WHY wrong answers are wrong
- ✅ No hesitation or uncertainty

---

**Total Cards:** 23 flashcards covering 8 critical patterns
**Estimated drill time:** 15-20 minutes per complete pass
**Target:** 100% mastery before retaking Lambda + Data Sources quiz

---

**Last Updated:** January 14, 2026
**Next Action:** Drill these cards, then retry the 10-question quiz targeting 90%+
