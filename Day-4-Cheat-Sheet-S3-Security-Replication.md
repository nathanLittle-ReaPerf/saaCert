# Day 4 Cheat Sheet: S3 Security & Replication

**The "Stop Mixing Up Encryption Types" Guide**

---

## 🎯 The 30-Second Decision Tree

```
START: What kind of S3 security do you need?

├─ Encryption at rest?
│  ├─ AWS manages everything (keys + encryption)?
│  │  └─ ✅ SSE-S3 (AES-256, AWS-managed keys)
│  ├─ You control key policies, rotation, audit?
│  │  └─ ✅ SSE-KMS (AWS KMS, you control key)
│  ├─ You provide encryption keys to AWS?
│  │  └─ ✅ SSE-C (Customer-provided keys, AWS encrypts)
│  └─ You encrypt before uploading to S3?
│     └─ ✅ Client-Side Encryption (You encrypt, AWS stores ciphertext)
│
├─ Access control?
│  ├─ Bucket-level policies (IAM-like JSON)?
│  │  └─ ✅ BUCKET POLICIES
│  ├─ Object-level permissions (legacy)?
│  │  └─ ✅ ACLs (avoid unless legacy requirement)
│  └─ Separate access points for different use cases?
│     └─ ✅ S3 ACCESS POINTS
│
├─ Data protection?
│  ├─ Prevent accidental deletion?
│  │  └─ ✅ VERSIONING
│  ├─ Prevent deletion even by root user?
│  │  └─ ✅ OBJECT LOCK (Compliance/Governance)
│  └─ Require MFA to delete?
│     └─ ✅ MFA DELETE
│
└─ Replication?
   ├─ Copy to different AWS region?
   │  └─ ✅ CROSS-REGION REPLICATION (CRR)
   └─ Copy within same region?
      └─ ✅ SAME-REGION REPLICATION (SRR)
```

---

## 📋 S3 Encryption Comparison Table

| Feature | SSE-S3 | SSE-KMS | SSE-C | Client-Side |
|---------|--------|---------|-------|-------------|
| **Full Name** | Server-Side Encryption with S3-Managed Keys | Server-Side Encryption with KMS | Server-Side Encryption with Customer Keys | Client-Side Encryption |
| **Who Manages Keys?** | AWS (fully managed) | AWS KMS (you control) | You (provide each request) | You (encrypt before upload) |
| **Encryption Algorithm** | AES-256 | AES-256 | AES-256 | Your choice |
| **Key Rotation** | Automatic (AWS) | Automatic (KMS) | Manual (you) | Manual (you) |
| **Audit Trail** | ❌ No | ✅ Yes (CloudTrail KMS logs) | ❌ No | ❌ No |
| **Extra Cost** | ✅ Free | 💰 KMS API calls charged | ✅ Free | ✅ Free |
| **S3 Header** | `x-amz-server-side-encryption: AES256` | `x-amz-server-side-encryption: aws:kms` | `x-amz-server-side-encryption-customer-algorithm` | None (already encrypted) |
| **Use Case** | Default encryption, no audit required | Compliance, audit logs, key control | High security, external key management | Maximum control, encrypt before AWS |
| **HTTPS Required** | ❌ No | ❌ No | ✅ Yes (must use HTTPS) | ❌ No (already encrypted) |

---

## 🔑 Exam Keyword → Encryption Type Mapping

**When you see these keywords in a question:**

### SSE-S3 Keywords
- ✅ "Default encryption"
- ✅ "Simplest encryption solution"
- ✅ "No additional cost"
- ✅ "AWS manages everything"
- ✅ "Least operational overhead"
- ✅ "AES-256 encryption"

### SSE-KMS Keywords
- ✅ "Audit trail required" (CloudTrail logs KMS key usage)
- ✅ "Control key rotation"
- ✅ "Compliance requirements" (HIPAA, PCI-DSS)
- ✅ "Separate permissions for key and data"
- ✅ "Disable key access to revoke access to data"
- ✅ "CloudTrail logs for encryption activity"

### SSE-C Keywords
- ✅ "Customer-provided encryption keys"
- ✅ "You manage keys externally"
- ✅ "AWS doesn't store your keys"
- ✅ "HTTPS required" (must send keys securely)

### Client-Side Encryption Keywords
- ✅ "Encrypt before uploading"
- ✅ "Maximum control over encryption"
- ✅ "AWS stores ciphertext only"
- ✅ "You handle encryption/decryption"

---

## 🧠 Memory Trick: Encryption Decision

**SSE-S3**: "**S**imple **S**erver-**S**ide" - AWS does everything, you do nothing
**SSE-KMS**: "**K**ey **M**anagement **S**ervice" - You control keys, AWS encrypts
**SSE-C**: "**C**ustomer provides keys" - You send keys with each request (HTTPS required)
**Client-Side**: "Client does it" - You encrypt, AWS just stores it

---

## 🔐 Access Control Comparison

| Method | Scope | Best For | Example Use Case |
|--------|-------|----------|------------------|
| **Bucket Policies** | Bucket-level (JSON IAM policy) | Public access, cross-account, IP restrictions | "Allow public read on `mybucket/images/*`" |
| **ACLs** | Object-level (legacy XML) | Legacy apps, simple grants | "Grant read to specific AWS account" (avoid if possible) |
| **IAM Policies** | User/Role-level | AWS user/service access | "Allow EC2 role to write to `mybucket/logs/`" |
| **Access Points** | Bucket + prefix-level | Multiple access patterns to same bucket | "Finance team access to `/finance/*`, HR to `/hr/*`" |
| **S3 Block Public Access** | Account/Bucket level | Prevent accidental public exposure | "Block all public access to sensitive buckets" |

### When to Use What?

**Bucket Policy**:
- Cross-account access ("Account B can read mybucket")
- Public access ("Everyone can read mybucket/public/*")
- IP-based restrictions ("Only allow from 203.0.113.0/24")

**IAM Policy**:
- AWS user/service access ("This EC2 role can write to mybucket")
- Always evaluated with bucket policy (must allow in BOTH)

**Access Points**:
- Multiple teams/apps accessing same bucket with different permissions
- Simplifies complex bucket policies
- Each access point has own policy

**ACLs**:
- Legacy compatibility ONLY (AWS recommends bucket policies instead)

---

## 🔁 S3 Versioning & Deletion

### Versioning States
- **Unversioned** (default): No version tracking
- **Versioning-Enabled**: Every PUT creates new version ID
- **Versioning-Suspended**: Stops creating versions (existing versions kept)

### How Deletion Works with Versioning

| Action | Versioning OFF | Versioning ON |
|--------|----------------|---------------|
| **DELETE object** | Permanently deleted | Delete marker added (object hidden, recoverable) |
| **DELETE with version ID** | N/A | Permanently deletes that version |
| **DELETE delete marker** | N/A | Removes marker, object becomes visible again |

### Exam Pattern:
- "Accidentally deleted object" + "recover" → **Versioning enabled, delete the delete marker**
- "Prevent permanent deletion" → **Versioning + MFA Delete**
- "Reduce storage costs" + "versioning" → **Lifecycle policy to expire old versions**

**Memory Trick**: Versioning = **Safety net** (nothing truly deleted unless you specify version ID)

---

## 🔒 S3 Object Lock & MFA Delete

### Object Lock (Write Once Read Many - WORM)

| Mode | Can Delete? | Can Overwrite? | Can Shorten Retention? | Use Case |
|------|-------------|----------------|----------------------|----------|
| **Governance Mode** | ✅ Yes (with `s3:BypassGovernanceRetention` permission) | ❌ No | ✅ Yes (with permission) | Internal compliance, testing |
| **Compliance Mode** | ❌ **NO** (not even root user!) | ❌ No | ❌ **NO** (immutable) | Regulatory compliance (SEC, FINRA) |

**Key Facts**:
- Object Lock requires **versioning** (automatic)
- Two settings: **Retention Period** (days/years) and **Legal Hold** (indefinite, no expiration)
- Legal Hold: ON/OFF toggle, independent of retention period

### MFA Delete
- Requires **versioning enabled**
- Requires **MFA device** to:
  - Permanently delete object version
  - Suspend versioning on bucket
- **Cannot be enabled via AWS Console** (must use AWS CLI with root account credentials)

### Exam Pattern:
- "Regulatory compliance" + "prevent deletion" → **Object Lock Compliance Mode**
- "Prevent deletion but allow exceptions" → **Object Lock Governance Mode**
- "Require MFA to delete" → **MFA Delete** (also needs versioning)
- "Not even root can delete" → **Object Lock Compliance Mode**

**Memory Trick**:
- **Governance** = "**G**ovt can override" (with permission)
- **Compliance** = "**C**an't touch this" (immutable, even root)

---

## 🌍 S3 Replication: CRR vs SRR

| Feature | CRR (Cross-Region) | SRR (Same-Region) |
|---------|-------------------|-------------------|
| **Full Name** | Cross-Region Replication | Same-Region Replication |
| **Use Case** | DR, compliance, low-latency access | Log aggregation, prod/test sync, compliance |
| **Versioning Required** | ✅ Yes (both buckets) | ✅ Yes (both buckets) |
| **What Replicates** | New objects after replication enabled | New objects after replication enabled |
| **Replicate Existing?** | ❌ No (use S3 Batch Replication) | ❌ No (use S3 Batch Replication) |
| **Delete Markers** | ❌ Not replicated by default (optional) | ❌ Not replicated by default (optional) |
| **Permanent Deletes** | ❌ Not replicated | ❌ Not replicated |
| **Encryption** | ✅ Supports SSE-S3, SSE-KMS, SSE-C | ✅ Supports SSE-S3, SSE-KMS, SSE-C |
| **Cross-Account** | ✅ Yes | ✅ Yes |

### Replication Requirements Checklist:
- ✅ Versioning enabled on BOTH source and destination buckets
- ✅ IAM role with permissions to replicate
- ✅ If SSE-KMS: KMS key policy allows replication role

### What Does NOT Replicate (CRITICAL):
- ❌ Objects created **before** replication was enabled (use S3 Batch Replication)
- ❌ Delete markers (unless explicitly enabled in replication config)
- ❌ Permanent deletions (deletions with version ID)
- ❌ Lifecycle actions
- ❌ Objects encrypted with SSE-C (customer-provided keys)

### Exam Pattern:
- "Disaster recovery in different region" → **CRR**
- "Compliance requires copy in different region" → **CRR**
- "Aggregate logs from multiple buckets" → **SRR**
- "Sync production and test buckets in same region" → **SRR**
- "Replicate existing objects" → **S3 Batch Replication** (not automatic CRR/SRR)

**Memory Trick**:
- **CRR** = "**C**ross-**R**egion for **DR**" (disaster recovery)
- **SRR** = "**S**ame-**R**egion for **L**ogs/**T**esting" (aggregation/sync)

---

## ❌ Common Exam Traps (Don't Fall For These!)

### Trap 1: "SSE-KMS provides encryption"
**Wrong thinking**: SSE-S3 and SSE-KMS both encrypt, so they're the same
**Right answer**: Pick **SSE-KMS** when question mentions:
- Audit trail required (CloudTrail logs KMS API calls)
- Compliance (HIPAA, PCI-DSS)
- Separate key permissions
- Ability to disable key to revoke access

### Trap 2: "Versioning prevents deletion"
**Wrong thinking**: Versioning-enabled means objects can't be deleted
**Right answer**: Versioning creates **delete markers** (object hidden but recoverable)
- To permanently delete: specify version ID
- To recover: delete the delete marker
- To prevent deletion: **Object Lock Compliance Mode** (not just versioning)

### Trap 3: "Replication is automatic for all objects"
**Wrong thinking**: Enable replication and all objects replicate
**Right answer**: Replication only applies to **NEW objects after it's enabled**
- Existing objects: Use **S3 Batch Replication**
- Delete markers: NOT replicated by default

### Trap 4: "CRR and SRR are the same except for region"
**Mostly correct, but watch for**:
- CRR = Disaster recovery, compliance, latency
- SRR = Log aggregation, prod/test sync
- Exam will hint at use case to pick the right one

### Trap 5: "Object Lock and MFA Delete are the same"
**Wrong thinking**: Both prevent deletion, so they're the same
**Right answer**:
- **Object Lock**: WORM model, retention period, Governance/Compliance modes
- **MFA Delete**: Requires MFA device to delete or suspend versioning
- Can use BOTH together for maximum protection

### Trap 6: "Block Public Access only blocks bucket policies"
**Wrong thinking**: If I set Block Public Access, bucket policies are disabled
**Right answer**: Block Public Access **overrides** policies that grant public access
- Bucket policies still work for non-public access
- Best practice: Enable by default, explicitly allow public only when needed

---

## 🎓 The "If You Remember Nothing Else" Summary

### S3 Encryption
**SSE-S3**: Default, simple, AWS manages everything (free)
**SSE-KMS**: Audit trail, key control, compliance (costs extra)
**SSE-C**: You provide keys, HTTPS required (advanced)
**Client-Side**: You encrypt before upload (maximum control)

**Exam pattern**: "Audit trail" = SSE-KMS, "Simplest" = SSE-S3

---

### S3 Access Control
**Bucket Policies**: Cross-account, public access, IP restrictions (JSON)
**IAM Policies**: AWS user/service access (works with bucket policies)
**Access Points**: Multiple teams/apps, different permissions (simplifies management)
**ACLs**: Legacy only (avoid unless required)

**Exam pattern**: "Multiple teams" = Access Points, "Public access" = Bucket Policy

---

### S3 Versioning & Deletion
**Versioning ON**: Delete creates marker (recoverable)
**Versioning OFF**: Delete is permanent
**Delete marker**: Remove to recover object
**Permanent delete**: Specify version ID

**Exam pattern**: "Recover deleted object" = Delete the delete marker (versioning required)

---

### S3 Object Lock
**Governance Mode**: Deletable with special permission (testing, internal compliance)
**Compliance Mode**: NOT deletable even by root (regulatory compliance)
**Legal Hold**: ON/OFF toggle, no expiration (independent of retention)

**Exam pattern**: "Not even root can delete" = Compliance Mode, "Allow exceptions" = Governance Mode

---

### S3 MFA Delete
**Requires**: Versioning enabled + MFA device
**Protects**: Permanent deletions, versioning suspension
**Setup**: AWS CLI only (not Console), root account credentials

**Exam pattern**: "Require MFA to delete" = MFA Delete (also needs versioning)

---

### S3 Replication
**CRR**: Cross-Region, disaster recovery, compliance, latency
**SRR**: Same-Region, log aggregation, prod/test sync
**Both require**: Versioning on source AND destination
**Does NOT replicate**: Existing objects (use Batch Replication), delete markers, permanent deletions

**Exam pattern**: "DR different region" = CRR, "Aggregate logs same region" = SRR

---

## 📝 Quick Quiz (Answer in 5 Seconds Each)

1. Need audit trail of encryption key usage → **?**
2. Prevent deletion even by root user → **?**
3. Customer provides encryption keys with each request → **?**
4. Replicate objects to different region for disaster recovery → **?**
5. Multiple teams accessing same bucket with different permissions → **?**
6. Recover accidentally deleted object → **?**
7. Simplest encryption with no extra cost → **?**
8. Require MFA to permanently delete object version → **?**
9. Aggregate logs from multiple buckets in same region → **?**
10. Compliance mode that allows exceptions with permission → **?**

### Answers:
1. SSE-KMS (CloudTrail logs KMS API calls)
2. Object Lock Compliance Mode (immutable)
3. SSE-C (customer-provided keys)
4. CRR (Cross-Region Replication)
5. S3 Access Points (separate policies per team)
6. Delete the delete marker (versioning required)
7. SSE-S3 (default, AWS-managed, free)
8. MFA Delete (requires versioning)
9. SRR (Same-Region Replication)
10. Object Lock Governance Mode (can bypass with permission)

**If you got less than 9/10, read this sheet again.**

---

## 🎯 Exam Day Strategy for S3 Security & Replication

### When You See an Encryption Question:

**Step 1**: Look for "audit trail" or "CloudTrail logs"
- If YES → SSE-KMS (logs key usage)
- If NO → Check next step

**Step 2**: Look for "simplest" or "least overhead"
- If YES → SSE-S3 (default, free, AWS manages all)

**Step 3**: Look for "customer-provided keys"
- If YES → SSE-C (you provide keys, HTTPS required)

**Step 4**: Look for "encrypt before upload"
- If YES → Client-Side Encryption

### When You See an Access Control Question:

**Step 1**: Look for "multiple teams" or "different access patterns"
- If YES → S3 Access Points

**Step 2**: Look for "public access" or "cross-account"
- If YES → Bucket Policy (JSON)

**Step 3**: Look for AWS service or user access
- If YES → IAM Policy (+ Bucket Policy must both allow)

### When You See a Deletion/Protection Question:

**Step 1**: Look for "recover deleted object"
- If YES → Versioning enabled, delete the delete marker

**Step 2**: Look for "not even root can delete"
- If YES → Object Lock Compliance Mode

**Step 3**: Look for "require MFA to delete"
- If YES → MFA Delete (requires versioning)

**Step 4**: Look for "allow exceptions with permission"
- If YES → Object Lock Governance Mode

### When You See a Replication Question:

**Step 1**: Check region requirement
- Different region → CRR (disaster recovery, compliance)
- Same region → SRR (log aggregation, prod/test sync)

**Step 2**: Check for "existing objects"
- If YES → S3 Batch Replication (not automatic CRR/SRR)

**Step 3**: Verify versioning mentioned
- Both source AND destination need versioning

---

## ⚡ The "Can't Get This Wrong" List

**MEMORIZE THESE FACTS:**

1. **SSE-KMS provides audit trail** (CloudTrail logs) - SSE-S3 does not
2. **Object Lock Compliance Mode prevents deletion by anyone** - Even root user
3. **Versioning creates delete markers** - Objects recoverable unless version ID specified
4. **MFA Delete requires versioning AND MFA device** - Can't enable via Console (CLI only)
5. **Replication only applies to NEW objects** - Use Batch Replication for existing
6. **CRR = Cross-Region for DR** - SRR = Same-Region for logs/testing
7. **Access Points simplify multi-team access** - Each team gets own policy
8. **SSE-C requires HTTPS** - Must securely send customer keys
9. **Object Lock Governance allows bypass** - Compliance does NOT (immutable)
10. **Delete markers are NOT replicated by default** - Optional setting in replication config

---

## 🚀 Your Action Plan

**Study Session:**
1. Read this sheet 2 times (20 minutes)
2. Do the quick self-test without looking (5 minutes)
3. Review any wrong answers

**Quiz Time:**
1. Take Day 4 practice questions
2. Target: 85%+ correct
3. Focus on encryption decision tree

**If you score below 85%:**
- Review the comparison tables again
- Take the quiz again
- Don't move to Day 5 until you hit 85%+

---

**Now go memorize this. S3 security is 10-15% of the exam. Don't throw away easy points.** 💪
