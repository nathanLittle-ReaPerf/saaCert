# Day 2 Critical Flashcards - EC2 Storage & Placement Groups

**Created:** January 6, 2026
**Purpose:** 5 essential patterns mastered today - review daily until exam

---

## Flashcard #1: Multi-AZ Concurrent Access Pattern

**FRONT:**
```
Scenario: Multiple EC2 instances across MULTIPLE AVAILABILITY ZONES
need concurrent read/write access to shared files.

What storage solution?
```

**BACK:**
```
ANSWER: Amazon EFS ✅

ALWAYS EFS when you see:
├─ Multiple AZs
├─ Concurrent access
├─ Shared files
├─ "Immediately available"
└─ File system needed

WHY NOT Multi-Attach?
❌ Multi-Attach is SINGLE AZ ONLY (cannot span AZs)

WHY NOT S3 sync?
❌ Not "immediately available" (sync delays)

PATTERN TO MEMORIZE:
Multi-AZ + Concurrent + Shared = EFS (100% of the time)
```

---

## Flashcard #2: Block Storage Concurrent Access Pattern

**FRONT:**
```
Scenario: Multiple EC2 instances in a SINGLE AVAILABILITY ZONE
need concurrent BLOCK-LEVEL access to the same storage volume.

What storage solution?
```

**BACK:**
```
ANSWER: Amazon EBS Multi-Attach ✅

Requirements ALL met:
├─ Single AZ deployment ✅
├─ Block-level/block storage ✅
├─ Concurrent access ✅
└─ ≤16 instances ✅

EBS Multi-Attach Limitations (MEMORIZE):
1. SINGLE AZ ONLY (cannot span AZs)
2. io1 or io2 volumes ONLY (NOT gp3, gp2, st1, sc1)
3. Max 16 instances
4. Requires cluster-aware file system

WHY NOT EFS?
❌ EFS is FILE storage, not BLOCK storage

KEYWORD TRIGGERS:
"block-level access" = Must be EBS (not EFS)
"block storage volume" = Must be EBS (not EFS)
```

---

## Flashcard #3: EBS Multi-Attach Complete Limitations

**FRONT:**
```
What are the 4 critical limitations of EBS Multi-Attach?
```

**BACK:**
```
EBS Multi-Attach Limitations:

1. ❌ SINGLE AZ ONLY
   └─ Cannot attach to instances in different AZs
   └─ All instances must be in SAME Availability Zone

2. ❌ io1 or io2 ONLY
   └─ Does NOT work with: gp3, gp2, st1, sc1
   └─ Only Provisioned IOPS SSD volumes

3. ❌ Max 16 instances
   └─ Cannot attach to more than 16 instances
   └─ For larger clusters, use EFS

4. ❌ Requires cluster-aware file system
   └─ Cannot use standard ext4/xfs
   └─ Examples: Oracle ASM, Windows WSFC, GFS2

WHAT MULTI-ATTACH IS NOT:
❌ NOT a disaster recovery solution
❌ NOT a backup solution
❌ NOT multi-AZ protection
❌ NOT for file sharing (use EFS)

WHAT IT IS FOR:
✅ Concurrent block-level access
✅ Clustered databases (Oracle RAC, SQL Server)
✅ Single AZ high-availability clusters
```

---

## Flashcard #4: Instance Store vs EBS vs EFS Decision Tree

**FRONT:**
```
How do you decide between Instance Store, EBS, and EFS?
```

**BACK:**
```
DECISION TREE:

Step 1: Does data need to persist?
├─ NO (ephemeral/temporary OK)
│  └─ Need HIGHEST I/O performance?
│     ├─ YES → Instance Store ✅
│     └─ NO → Consider EBS or EFS
│
└─ YES (must survive stop/terminate)
   └─ Go to Step 2

Step 2: Multiple instances need access?
├─ YES (shared/concurrent access)
│  └─ Multiple AZs?
│     ├─ YES → EFS ✅
│     └─ NO (single AZ) → Block or file storage?
│        ├─ Block → Multi-Attach (if ≤16 instances)
│        └─ File → EFS
│
└─ NO (single instance only)
   └─ EBS (gp3 for general, io2 for extreme)

STORAGE PERFORMANCE RANKING (fastest → slowest):
1. Instance Store (millions IOPS, sub-millisecond)
2. EBS io2 (up to 256,000 IOPS)
3. EBS gp3 (up to 16,000 IOPS)
4. EFS (network latency, good for shared access)

KEYWORD TRIGGERS:
"temporary" + "can be regenerated" → Instance Store
"Multi-AZ" + "concurrent" → EFS
"block-level" + "single AZ" → Multi-Attach
"HIGHEST I/O" + "temporary" → Instance Store
```

---

## Flashcard #5: Placement Groups Complete Pattern

**FRONT:**
```
How do you choose between Cluster, Spread, and Partition
Placement Groups?
```

**BACK:**
```
DECISION MATRIX:

CLUSTER Placement Group:
├─ Use case: HPC, ML training, MPI workloads
├─ Keywords: "lowest latency", "tightly-coupled", "HPC", "MPI"
├─ Performance: Up to 100 Gbps between instances
├─ Limitation: SINGLE AZ ONLY
└─ When: Need absolute lowest network latency

SPREAD Placement Group:
├─ Use case: Small number of CRITICAL instances
├─ Keywords: "critical", "isolated", "maximum separation"
├─ HARD LIMIT: MAX 7 INSTANCES PER AZ ⚠️
├─ Isolation: Each instance on separate hardware rack
└─ When: Critical instances that must never fail together

PARTITION Placement Group:
├─ Use case: LARGE distributed systems
├─ Keywords: "Cassandra", "Kafka", "Hadoop", "partition-aware"
├─ Scale: HUNDREDS/THOUSANDS of instances
├─ Structure: Up to 7 partitions/AZ, hundreds of instances per partition
└─ When: Large distributed databases or streaming platforms

GUARANTEED EXAM ANSWERS:
"Cassandra" → PARTITION ✅ (100% of the time)
"Kafka" → PARTITION ✅ (100% of the time)
"Hadoop" → PARTITION ✅ (100% of the time)
"HPC" or "MPI" → CLUSTER ✅ (100% of the time)
"Critical" + "≤7 per AZ" → SPREAD ✅

COMMON TRAPS:
❌ Cassandra + Spread → WRONG (exceeds 7/AZ limit)
❌ Multi-AZ + Cluster → WRONG (Cluster is single AZ only)
❌ 50+ instances + Spread → WRONG (exceeds limit)
```

---

## Daily Review Checklist

Review these 5 flashcards every day until exam:

- [ ] Day 2 (Jan 6) - Created
- [ ] Day 3
- [ ] Day 4
- [ ] Day 5
- [ ] Day 6
- [ ] Day 7
- [ ] Day 8
- [ ] Day 9
- [ ] Day 10

Continue reviewing until you can answer all 5 from memory without hesitation!

---

## Quick Reference - Key Facts to Memorize

**Multi-Attach:**
- Single AZ only
- io1/io2 only (NOT gp3!)
- Max 16 instances
- Cluster-aware FS required

**Storage Performance:**
1. Instance Store (fastest, ephemeral)
2. EBS io2 (persistent, expensive)
3. EBS gp3 (balanced)
4. EFS (shared, network latency)

**Placement Groups:**
- Cassandra/Kafka/Hadoop = PARTITION
- HPC/MPI = CLUSTER
- ≤7 critical instances = SPREAD

**EFS vs Multi-Attach:**
- Multi-AZ + concurrent = EFS
- Single AZ + block + concurrent = Multi-Attach
- "Immediately available" = EFS
