# DynamoDB Final Drill Quiz - December 18, 2025

## Quiz Metadata
- **Date:** December 18, 2025 (21:32 CST)
- **Topic:** DynamoDB (Weakness Elimination)
- **Questions:** 10
- **Format:** One question at a time
- **Target Score:** 90% (9/10) to eliminate weakness
- **Actual Score:** 100% (10/10)
- **Status:** WEAKNESS CONQUERED

---

## Performance Summary

### Overall Results
- **Score:** 10/10 (100%)
- **Target Met:** YES (exceeded 90% target)
- **Weakness Status:** ELIMINATED

### Question-by-Question Breakdown

| Question | Topic | Answer | Result |
|----------|-------|--------|--------|
| 1 | E-commerce shopping cart design | A | CORRECT |
| 2 | Social media feed access patterns | B | CORRECT |
| 3 | IoT sensor data storage | C | CORRECT |
| 4 | Financial audit logging | D | CORRECT |
| 5 | Gaming leaderboard (global) | A | CORRECT |
| 6 | Multi-tenant SaaS table design | B | CORRECT |
| 7 | Time-series metrics query | C | CORRECT |
| 8 | Sparse index optimization | A | CORRECT |
| 9 | Capacity mode selection | B | CORRECT |
| 10 | Trading platform design | A | CORRECT |

---

## Question 10: Trading Platform Table Design (FINAL QUESTION)

### Scenario
A trading platform stores 500 million trade records. The application has three query patterns:
- **Get trade by TradeId:** 50,000 requests/day (HIGHEST volume)
- **Get all trades for a specific user:** 5,000 requests/day
- **Get all trades for a specific stock:** 2,000 requests/day

What is the MOST efficient table design?

### Answer Options

**A)** PK: TradeId | GSI1: PK=UserId, SK=Timestamp | GSI2: PK=StockSymbol, SK=Timestamp

**B)** PK: UserId, SK: TradeId | GSI1: PK=StockSymbol, SK=Timestamp

**C)** PK: UserId, SK: Timestamp | LSI: SK=TradeId | GSI1: PK=StockSymbol, SK=Timestamp

**D)** PK: StockSymbol, SK: TradeId | GSI1: PK=UserId, SK=Timestamp

### User's Answer: A - CORRECT

### Explanation

**Why A is correct:**

1. **Highest volume pattern drives base table design**
   - TradeId lookups (50K/day) are the most frequent queries
   - TradeId as partition key enables direct GetItem operations
   - Most efficient and cheapest query type (no index overhead)

2. **GSI1 for user queries**
   - UserId as PK with Timestamp as SK
   - Allows efficient querying of user's trades in chronological order
   - Handles 5K requests/day efficiently

3. **GSI2 for stock queries**
   - StockSymbol as PK with Timestamp as SK
   - Allows efficient querying of stock's trades in chronological order
   - Handles 2K requests/day efficiently

4. **Cost and performance optimization**
   - Base table queries are cheapest (no index overhead)
   - Highest volume pattern uses base table = lowest cost
   - GSIs handle lower-volume patterns efficiently
   - Total cost optimized by putting expensive operations on cheapest tier

**Why other options are wrong:**

**Option B - WRONG**
- Makes the LOWEST volume pattern (user queries, 5K/day) the base table
- Forces the HIGHEST volume pattern (TradeId lookups, 50K/day) to use a GSI
- Inefficient: 50K/day queries hitting an index instead of base table = higher cost
- Poor cost optimization: Most expensive pattern gets most expensive query type

**Option C - WRONG**
- Same base table issue as Option B (wrong pattern prioritized)
- LSI limitation: Can only query within a partition (same UserId)
- To get a trade by TradeId, you'd need to know the UserId first
- Without UserId, requires Scan operation
- **SCAN with 500M records = exam answer red flag**
- LSI doesn't solve the problem, creates performance disaster

**Option D - WRONG**
- Makes the SECOND-LOWEST volume pattern (stock queries, 2K/day) the base table
- Forces highest volume pattern (50K/day) to use GSI again
- Poor design: Least frequent pattern gets best performance
- Inverted optimization: Optimizing for the wrong access pattern

### Key Concepts Demonstrated

1. **Base table design follows highest volume access pattern**
   - Put most frequent queries on base table for cost optimization
   - GSIs cost more per query than base table operations
   - 50K/day on base table vs GSI = significant cost savings

2. **GSI selection for secondary patterns**
   - Lower volume patterns (5K, 2K per day) can afford GSI costs
   - GSIs enable flexible query patterns without Scan
   - PK + SK in GSI enables range queries (by timestamp)

3. **LSI limitations**
   - Can only query within same partition key
   - Requires knowing partition key value before query
   - Not suitable when you need to query by different attributes
   - Would force Scan for TradeId lookups = performance disaster

4. **Cost hierarchy understanding**
   - GetItem on base table (cheapest)
   - Query on base table
   - Query on GSI (more expensive)
   - Scan (most expensive, avoid in exam answers)

### Exam Patterns Recognized

- "MOST efficient" = Optimize for highest volume pattern
- Multiple access patterns = Consider GSIs for secondary patterns
- Large dataset (500M records) + unknown partition key = Never use LSI
- Cost optimization = Base table for frequent, GSI for infrequent

---

## Skills Mastered in This Quiz

### 1. Table Design Optimization
- Analyzing access pattern frequency
- Selecting appropriate base table schema
- Determining when to use GSIs vs LSIs
- Cost-optimizing based on query volume

### 2. Access Pattern Analysis
- Identifying primary vs secondary access patterns
- Mapping query requirements to key structures
- Understanding PK/SK combinations for range queries
- Recognizing when LSIs are inappropriate

### 3. Index Selection
- GSI use cases: Different partition key than base table
- LSI limitations: Same partition key, limited to 10 per table
- Understanding index projection types (ALL, KEYS_ONLY, INCLUDE)
- Cost implications of index queries

### 4. Performance Optimization
- GetItem vs Query vs Scan performance
- Avoiding Scans on large datasets
- Using sort keys for range queries
- Sparse indexes for optional attributes

### 5. Capacity Planning
- On-Demand for unpredictable workloads
- Provisioned with Auto Scaling for predictable patterns
- Understanding WCU/RCU calculations
- Burst capacity and throttling

### 6. Cost Optimization
- Base table cheaper than GSI queries
- GSI storage costs (duplicates data)
- On-Demand vs Provisioned cost breakeven
- Free tier understanding (25 GB storage, 25 WCU/RCU)

---

## DynamoDB Weakness Timeline

### December 8, 2025 - Initial Assessment
- Day 6 Catchup Quiz: 14/20 (70%) - DynamoDB questions failed
- Identified fundamental gaps in table design and index selection
- Couldn't differentiate GSI vs LSI use cases
- Confused Query vs Scan operations

### December 11, 2025 - Deep Dive Study
- Created "Query vs Scan Deep Dive" document
- Studied decision trees for GetItem vs Query vs Scan
- Reviewed DynamoDB pricing and capacity modes
- Score improvement minimal - still missing fundamentals

### December 15, 2025 - Nuclear Reset
- Stuck at 60% for 3 days on DynamoDB questions
- Created "DynamoDB Decision Tree Quick Reference"
- Memorized GSI/LSI structure and limitations
- Focused on cost optimization patterns

### December 16-17, 2025 - Breakthrough
- DynamoDB Weakness Elimination Marathon
- 4 weaknesses conquered in targeted drills
- Achieved 8/10 (80%) on comprehensive quiz
- 2 remaining weaknesses: Table design and capacity modes

### December 18, 2025 - Victory
- Final Drill Quiz: 10/10 (100%)
- All DynamoDB weaknesses eliminated
- Ready for exam-level questions
- **DynamoDB is now a STRENGTH**

---

## Study Materials Created

1. **DynamoDB Decision Tree Quick Reference** (Dec 15)
   - GetItem vs Query vs Scan decision flow
   - GSI vs LSI selection criteria
   - Capacity mode selection (On-Demand vs Provisioned)

2. **Query vs Scan Deep Dive** (Dec 11)
   - Performance comparison with examples
   - Cost analysis for different operation types
   - When to use each operation type

3. **Multiple Targeted Drill Quizzes**
   - Dec 16: Access pattern quizzes (80% score)
   - Dec 17: Index selection quizzes (90% score)
   - Dec 18: Final comprehensive quiz (100% score)

---

## Next Steps

### Immediate (Dec 19-20)
1. Move to next weakness: RDS/Aurora
2. Create similar decision trees for relational databases
3. Practice differentiation: RDS vs Aurora vs DynamoDB

### Short-term (Dec 21-24)
1. S3 storage classes deep dive
2. VPC networking patterns
3. Lambda timeout and use case selection

### Final Prep (Dec 25 - Jan 5)
1. Full 65-question practice exams daily
2. Target 80%+ on all practice exams
3. Review all conquered weaknesses
4. Focus on remaining gaps

---

## Waldorf and Statler Final Review

**Waldorf:** Well, well, well... I don't believe it. A perfect score. On DynamoDB. From the same person who couldn't tell a partition key from a parking lot just a few days ago.

**Statler:** Must be a glitch in the system. Check the logs. Nobody goes from "what's a GSI?" to 100% on table design in one week.

**Waldorf:** I did check the logs! Question 10 was a beauty too - trading platform with three query patterns, 500 million trades, and they actually put the HIGHEST volume pattern on the base table instead of burning money with a GSI!

**Statler:** *grudgingly* That's... actually correct. The 50K requests per day should hit the base table, not an index. Basic cost optimization.

**Waldorf:** They even understood WHY the other options were wrong! Option C with the LSI would require scanning 500 million records just to find one trade. In my day, that would've melted the servers!

**Statler:** And they didn't fall for Option B either - putting the 5K/day pattern on the base table and forcing 50K/day through a GSI. That's like building a highway for bicycles and a bike path for trucks.

**Waldorf:** Remember when they thought you could use LSIs across different partition keys? Or when they wanted to add a GSI after creating 500 million records?

**Statler:** *chuckles darkly* Or yesterday when they tried to solve EVERYTHING with Query when half the problems screamed GetItem? That was performance malpractice!

**Waldorf:** But this... this is different. Ten questions, ten correct answers. Access patterns, capacity modes, index selection, cost optimization - they nailed every single one.

**Statler:** I suppose even a broken clock is right twice a day. Though this clock seems to have figured out how to actually tell time.

**Waldorf:** You know what the worst part is? Now we have nothing to mock them about. What are we supposed to do with a perfect score?

**Statler:** *sighs heavily* I suppose we could... acknowledge it. *winces* They've actually learned DynamoDB.

**Waldorf:** Fine. FINE. *dramatically* You've conquered the weakness. Happy now? Your GSI-PK-SK confusion is gone. Your Scan vs Query disasters are over. You can actually design a table without bankrupting the company.

**Statler:** On January 5th, 2026, when you're sitting in that exam and they ask about DynamoDB, you'll know the answer. And that's... *struggling* ...that's actually kind of impressive.

**Waldorf:** But don't get cocky! This was DynamoDB! You still have RDS, Aurora, ElastiCache, Redshift, QLDB, Neptune, DocumentDB, Timestream—

**Statler:** —and you'll probably mess all of those up spectacularly!

**Waldorf:** Exactly! So enjoy your little victory lap while it lasts, because tomorrow we're coming for your relational database knowledge with the fury of a thousand failed transactions!

**Statler:** And when you confuse Read Replicas with Multi-AZ deployments, we'll be right here in our balcony, watching you crash and burn!

**Waldorf:** But today... *grudgingly tips hat* ...today you earned it. 10/10. Weakness eliminated. Now get out of here before we change our minds.

**Statler:** Dohohohoho!

**Both:** DOHOHOHOHOHO!

---

## Final Statistics

**Quiz Duration:** Approximately 40 minutes (one question at a time format)
**Study Time Invested:** 4 days (Dec 15-18)
**Previous Attempts:** 3 quizzes (60%, 80%, 100%)
**Improvement Rate:** 40 percentage points in 4 days
**Weakness Status:** ELIMINATED
**Confidence Level:** HIGH
**Exam Readiness:** DynamoDB questions = STRENGTH

**Days Until Exam:** 18 days (January 5, 2026)

---

**Quiz completed at:** December 18, 2025, 21:32 CST
**Documented by:** AWS Quiz Master
**Status:** WEAKNESS CONQUERED - READY FOR EXAM
