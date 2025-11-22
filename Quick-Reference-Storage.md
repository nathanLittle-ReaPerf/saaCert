# Storage Services Quick Reference

## S3 (Simple Storage Service)

### Storage Classes (CRITICAL TO MEMORIZE!)
| Class | Use Case | Availability | Retrieval Time | Cost |
|-------|----------|--------------|----------------|------|
| **S3 Standard** | Frequently accessed data | 99.99% | Milliseconds | $$$$ |
| **S3 Intelligent-Tiering** | Unknown/changing access patterns | 99.9% | Milliseconds | Auto-optimized |
| **S3 Standard-IA** | Infrequent access, rapid retrieval | 99.9% | Milliseconds | $$$ (+ retrieval fee) |
| **S3 One Zone-IA** | Infrequent, recreatable data | 99.5% (1 AZ) | Milliseconds | $$ (+ retrieval fee) |
| **S3 Glacier Instant Retrieval** | Archive, millisecond retrieval | 99.9% | Milliseconds | $$ (+ retrieval fee) |
| **S3 Glacier Flexible Retrieval** | Archive, 1-5 minute retrieval | 99.99% | 1-5 min (Expedited)<br>3-5 hrs (Standard)<br>5-12 hrs (Bulk) | $ |
| **S3 Glacier Deep Archive** | Long-term archive, lowest cost | 99.99% | 12 hrs (Standard)<br>48 hrs (Bulk) | $ (cheapest) |

### Storage Class Transitions (Lifecycle Policies)
```
S3 Standard
    ↓ (30 days minimum)
S3 Standard-IA / S3 Intelligent-Tiering
    ↓ (30 days minimum)
S3 Glacier Instant Retrieval
    ↓ (90 days minimum)
S3 Glacier Flexible Retrieval
    ↓ (180 days minimum)
S3 Glacier Deep Archive
```

**Rules:**
- Can transition from Standard to any class
- Can transition from Standard-IA to Glacier (any)
- **Cannot** transition from Glacier back to Standard or IA
- Must stay in Standard for 30 days before transitioning to IA
- One Zone-IA requires 30 days minimum in Standard

### S3 Encryption
| Type | Description | Use Case |
|------|-------------|----------|
| **SSE-S3** | AWS-managed keys | Default, simple, no key management |
| **SSE-KMS** | AWS KMS keys | Audit trail (CloudTrail), key rotation, access control |
| **SSE-C** | Customer-provided keys | Customer manages keys outside AWS |
| **Client-Side** | Encrypt before upload | Full control, encrypt before sending to AWS |

- **Encryption in transit**: HTTPS/TLS (use bucket policy to enforce)
- **Default encryption**: Can set default encryption for bucket (SSE-S3 or SSE-KMS)

### S3 Replication
| Type | Use Case | Prerequisites |
|------|----------|---------------|
| **CRR** (Cross-Region) | Compliance, lower latency, DR | Versioning enabled on both buckets |
| **SRR** (Same-Region) | Log aggregation, compliance | Versioning enabled on both buckets |

**Replication Features:**
- Replication is asynchronous (typically seconds to minutes)
- **Replication Time Control (RTC)**: 99.99% replicated within 15 minutes (SLA)
- Can replicate delete markers (optional)
- Existing objects NOT replicated automatically (use S3 Batch Replication)
- Can replicate to different storage classes

### S3 Versioning
- Stores all versions of object (including deletes)
- Once enabled, can only suspend (not disable)
- Delete marker = soft delete (can be removed to restore)
- **MFA Delete**: Require MFA for permanent deletion or suspending versioning
- Protects against accidental deletion

### S3 Object Lock
- **WORM** (Write Once Read Many) model
- **Governance Mode**: Users with special permissions can delete
- **Compliance Mode**: No one can delete (including root) until retention expires
- **Legal Hold**: Indefinite protection, no expiration date
- Use case: Regulatory compliance, immutable backups

### S3 Performance
- **3,500 PUT/COPY/POST/DELETE** per prefix per second
- **5,500 GET/HEAD** per prefix per second
- **Multipart Upload**: For files >100 MB (required for >5 GB)
- **S3 Transfer Acceleration**: Upload to edge location → AWS backbone network (faster)
- **Byte-Range Fetches**: Download parts of file in parallel (faster downloads)

### S3 Security
| Feature | Purpose |
|---------|---------|
| **Bucket Policies** | Resource-based policy (JSON), control access to bucket |
| **IAM Policies** | User/role-based permissions |
| **ACLs** | Legacy, grant basic permissions (avoid if possible) |
| **Access Points** | Simplify access for large datasets, different permissions per access point |
| **Block Public Access** | Account/bucket level setting to prevent public access |
| **Pre-signed URLs** | Temporary access to objects (GET or PUT) |

### S3 Event Notifications
- Trigger on object creation, deletion, restore, replication
- Targets: **SNS, SQS, Lambda, EventBridge**
- Use EventBridge for advanced filtering and more targets

---

## EBS (Elastic Block Store)

### Volume Types (Know the IOPS and throughput!)
| Type | Use Case | Max IOPS | Max Throughput | Size |
|------|----------|----------|----------------|------|
| **gp3** (SSD) | General purpose, boot volumes | 16,000 | 1,000 MB/s | 1 GB - 16 TB |
| **gp2** (SSD) | General purpose, older gen | 16,000 (baseline 3 IOPS/GB) | 250 MB/s | 1 GB - 16 TB |
| **io2 Block Express** (SSD) | Highest performance, sub-ms latency | 256,000 | 4,000 MB/s | 4 GB - 64 TB |
| **io2** (SSD) | High-performance, mission-critical | 64,000 | 1,000 MB/s | 4 GB - 16 TB |
| **io1** (SSD) | High-performance, older gen | 64,000 | 1,000 MB/s | 4 GB - 16 TB |
| **st1** (HDD) | Throughput-optimized, big data | 500 | 500 MB/s | 125 GB - 16 TB |
| **sc1** (HDD) | Cold storage, infrequent access | 250 | 250 MB/s | 125 GB - 16 TB |

**Key Points:**
- **gp3**: New default, better price/performance than gp2 (provision IOPS and throughput independently)
- **gp2**: Baseline 3 IOPS per GB (100 GB = 300 IOPS), burst to 3,000 IOPS with credits
- **io1/io2**: For databases requiring sustained high IOPS
- **HDD (st1/sc1)**: Cannot be boot volumes, good for sequential workloads

### EBS Features
- **Snapshots**: Incremental backups to S3, can copy across regions
- **Encryption**: AES-256, encrypted at rest and in transit (minimal performance impact)
- **Multi-Attach** (io1/io2 only): Attach to up to 16 instances (same AZ, cluster-aware filesystem required)
- **EBS Optimized**: Dedicated bandwidth for EBS (most instance types by default)
- **Fast Snapshot Restore (FSR)**: No latency when restoring from snapshot (costs extra)

---

## EFS (Elastic File System)

### Overview
- **What**: Managed NFS (Network File System), shared across multiple EC2 instances
- **Supports**: NFSv4.1 protocol
- **Availability**: Multi-AZ, highly available
- **Scaling**: Petabyte-scale, elastic (grows/shrinks automatically)
- **Pricing**: Pay per GB stored

### Storage Classes
| Class | Use Case | Cost |
|-------|----------|------|
| **EFS Standard** | Frequently accessed | $$$$ |
| **EFS Infrequent Access (IA)** | Files not accessed for 30-90 days | $ (70% cheaper) |

**Lifecycle Management**: Auto-transition to IA after N days

### Performance Modes
| Mode | Use Case | Latency | Throughput |
|------|----------|---------|------------|
| **General Purpose** | Default, web servers, CMS | Lowest latency | Up to 7,000 file ops/sec |
| **Max I/O** | Big data, media processing | Higher latency | Higher parallelism, 500,000+ ops/sec |

### Throughput Modes
| Mode | Description | Use Case |
|------|-------------|----------|
| **Bursting** | Scales with size, burst credits | Variable workloads |
| **Provisioned** | Set throughput regardless of size | Predictable, high throughput needs |
| **Elastic** | Auto-scales throughput | Unpredictable workloads |

### EFS vs EBS vs Instance Store
| Feature | EBS | EFS | Instance Store |
|---------|-----|-----|----------------|
| **Attachment** | One instance (except io1/io2 Multi-Attach) | Multiple instances | Instance only (ephemeral) |
| **Availability** | Single AZ | Multi-AZ | Single host |
| **Use Case** | Boot volumes, databases | Shared file storage, content management | Temporary storage, cache, buffers |
| **Persistence** | Yes (snapshots to S3) | Yes | No (data lost on stop/terminate) |
| **Performance** | High IOPS (io1/io2) | Good for shared access | Highest IOPS (local disk) |

---

## FSx (File System Options)

### FSx for Windows File Server
- **What**: Fully managed Windows native file system (SMB protocol)
- **Features**: AD integration, DFS, Windows ACLs, shadow copies
- **Use Case**: Windows applications, Active Directory, SMB shares
- **Deployment**: Single-AZ or Multi-AZ
- **Storage**: SSD or HDD

### FSx for Lustre
- **What**: High-performance file system for HPC, ML, video processing
- **Integration**: S3 (can read/write directly to S3)
- **Performance**: Sub-millisecond latency, up to 1+ TB/s throughput
- **Use Case**: HPC, machine learning, video rendering, financial modeling
- **Deployment**: Scratch (temporary, no replication) or Persistent (HA, replicated)

### FSx for NetApp ONTAP
- **What**: Managed NetApp ONTAP (enterprise NAS)
- **Protocols**: NFS, SMB, iSCSI
- **Features**: Snapshots, cloning, compression, deduplication
- **Use Case**: Enterprise apps requiring NetApp features
- **Storage**: Multi-AZ, high availability

### FSx for OpenZFS
- **What**: Managed OpenZFS file system
- **Performance**: Up to 1 million IOPS
- **Use Case**: Linux workloads, ZFS features (snapshots, compression)

---

## Storage Gateway

### Gateway Types
| Type | Protocol | Use Case | Cache |
|------|----------|----------|-------|
| **File Gateway** | NFS, SMB | File shares backed by S3 | Local cache |
| **Volume Gateway (Cached)** | iSCSI | Block storage, frequently accessed data cached | Primary data in S3 |
| **Volume Gateway (Stored)** | iSCSI | Low-latency access, async backup to S3 | Primary data on-premises |
| **Tape Gateway** | iSCSI VTL | Virtual tape library, backup apps | Tapes in S3/Glacier |

**Key Concepts:**
- **File Gateway**: On-prem access to S3 as NFS/SMB shares
- **Volume Gateway Cached**: Primary data in S3, cache on-prem (minimize on-prem storage)
- **Volume Gateway Stored**: Primary data on-prem, backup to S3 (low latency)
- **Tape Gateway**: Replace physical tape backup with S3/Glacier

---

## AWS Backup
- **What**: Centralized backup service across AWS services
- **Supports**: EC2, EBS, RDS, Aurora, DynamoDB, EFS, FSx, Storage Gateway
- **Features**: Backup plans, retention policies, cross-region backup, lifecycle to cold storage
- **Use Case**: Unified backup strategy, compliance

---

## DataSync
- **What**: Data transfer service for on-premises ↔ AWS
- **Protocols**: NFS, SMB
- **Bandwidth**: Up to 10 Gbps per agent
- **Use Case**: One-time or scheduled data migrations to S3, EFS, FSx
- **vs Storage Gateway**: DataSync = migration/sync, Storage Gateway = hybrid access

---

## Snow Family

### Snowcone
- **Size**: 8 TB usable (HDD) or 14 TB usable (SSD)
- **Use Case**: Edge computing, small data migration (<10 TB)
- **Connectivity**: Online (DataSync) or offline (ship back to AWS)

### Snowball Edge
- **Size**: 80 TB or 210 TB
- **Types**: Storage Optimized (80 TB) | Compute Optimized (42 TB + GPU)
- **Use Case**: Large data migration (10-100 TB), edge computing
- **Features**: Run EC2 instances, Lambda functions locally

### Snowmobile
- **Size**: 100 PB per truck
- **Use Case**: Exabyte-scale data transfer (>10 PB)
- **Security**: GPS tracking, 24/7 video surveillance, escort vehicle

**Decision Tree:**
- <10 TB: Network transfer or Snowcone
- 10 TB - 10 PB: Snowball Edge
- >10 PB: Snowmobile

---

## Exam Pattern Recognition

### "Shared file storage across multiple EC2 instances" → **EFS** or **FSx**
- Linux/cross-platform: **EFS**
- Windows apps: **FSx for Windows**
- HPC/ML: **FSx for Lustre**

### "Block storage for single EC2 instance" → **EBS**
- High IOPS: **io1/io2**
- General purpose: **gp3**
- Throughput-optimized: **st1**

### "Archive data, infrequent access, lowest cost" → **S3 Glacier Deep Archive**

### "On-premises backup to AWS" → **Storage Gateway** or **AWS Backup**
- File access: **File Gateway**
- Block storage: **Volume Gateway**
- Tape backup: **Tape Gateway**

### "Large data transfer to AWS (TB-PB scale)" → **Snow Family**
- Internet too slow: **Snowball** or **Snowmobile**
- Continuous sync: **DataSync**
