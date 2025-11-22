# Migration Services Quick Reference

## The 7 R's of Migration (CRITICAL!)

AWS migration strategies - know when to use each:

| Strategy | Description | Use Case | Effort | Cost Savings |
|----------|-------------|----------|--------|--------------|
| **Rehost** | "Lift and shift" - move as-is to AWS | Quick migration, no code changes | Low | 30% |
| **Replatform** | "Lift, tinker, and shift" - minor optimizations | Move to RDS instead of EC2 database | Medium | 40% |
| **Repurchase** | "Drop and shop" - move to SaaS | Replace self-hosted with AWS service | Low | Variable |
| **Refactor** | "Re-architect" - redesign for cloud-native | Serverless, microservices, containers | High | 50%+ |
| **Retire** | Shut down unused applications | Decommission redundant apps | Minimal | 100% |
| **Retain** | Keep on-premises | Legacy systems, compliance reasons | None | 0% |
| **Relocate** | Move to AWS without changes (VMware) | VMware Cloud on AWS | Low | 20% |

### Exam Tips for 7 R's
- **"Fastest migration"** → Rehost (lift and shift)
- **"Optimize during migration"** → Replatform
- **"Minimize ongoing management"** → Repurchase (use SaaS/managed services)
- **"Long-term cost savings"** → Refactor (cloud-native)
- **"VMware environment"** → Relocate (VMware Cloud on AWS)

---

## AWS Application Discovery Service

### Overview
- **What**: Discovers on-premises servers, applications, and dependencies
- **Purpose**: Plan migrations by understanding current infrastructure
- **Deployment**: Agent-based or agentless

### Discovery Methods
| Type | How It Works | Data Collected | Use Case |
|------|--------------|----------------|----------|
| **Agentless** | VMware vCenter connector | CPU, memory, disk, network utilization | VMware environments, quick discovery |
| **Agent-based** | Install AWS Discovery Agent on servers | System performance, running processes, network connections | Detailed dependency mapping, physical servers |

### Integration
- **AWS Migration Hub**: Central dashboard for discovered applications
- **Export**: CSV files for analysis
- **Application Discovery**: Group servers into applications

---

## AWS Migration Hub

### Overview
- **What**: Central dashboard to track application migrations
- **Integrations**: DMS, SMS, CloudEndure, third-party tools
- **Purpose**: Single pane of glass for migration status

### Features
- **Track progress**: Monitor migrations across multiple tools
- **Application grouping**: Organize servers into applications
- **Migration status**: See which apps are in-progress, completed, or not started
- **Metrics**: Time to migrate, resources migrated

### Key Concepts
- **Migration Hub Strategy Recommendations**: AI-powered recommendations for migration strategy (7 R's)
- **Migration Hub Refactor Spaces**: Incremental refactoring of applications

---

## AWS Database Migration Service (DMS)

### Overview
- **What**: Migrate databases to AWS with minimal downtime
- **Source**: On-premises, EC2, RDS, Azure SQL, Oracle, etc.
- **Target**: RDS, Aurora, Redshift, DynamoDB, S3, etc.
- **Key Feature**: Continuous data replication during migration (CDC - Change Data Capture)

### Migration Types
| Type | Description | Use Case |
|------|-------------|----------|
| **Homogeneous** | Same DB engine (MySQL → RDS MySQL) | Simple, minimal schema changes |
| **Heterogeneous** | Different DB engines (Oracle → PostgreSQL) | Requires Schema Conversion Tool (SCT) |

### DMS Components
| Component | Purpose |
|-----------|---------|
| **Replication Instance** | EC2 instance running DMS (performs migration) |
| **Source Endpoint** | Connection to source database |
| **Target Endpoint** | Connection to target database |
| **Replication Task** | Defines what to migrate and how |

### DMS Replication Tasks
| Task Type | Description | Use Case |
|-----------|-------------|----------|
| **Full Load** | Migrate all existing data | Initial migration |
| **Full Load + CDC** | Migrate existing data, then continuous replication | Zero-downtime migration |
| **CDC Only** | Replicate changes only (assumes data already migrated) | Ongoing replication after initial load |

### DMS Use Cases
- **Migrate databases to AWS** (on-prem → RDS)
- **Continuous replication** for DR (primary on-prem, DR in AWS)
- **Consolidate databases** (multiple sources → single target)
- **Database engine conversion** (Oracle → Aurora PostgreSQL)
- **Development/test environments** (production → dev environment in AWS)

### DMS Best Practices
- Use **Multi-AZ** replication instance for production migrations (HA)
- Use **large enough replication instance** (bottleneck = slow migration)
- Enable **CloudWatch logs** for monitoring
- Use **validation** to ensure data integrity
- For large migrations, do **full load first, then CDC**

---

## AWS Schema Conversion Tool (SCT)

### Overview
- **What**: Convert database schemas from one engine to another
- **Use Case**: Heterogeneous migrations (Oracle → PostgreSQL, SQL Server → MySQL)
- **Output**: Converted schema + assessment report

### How It Works
1. **Install SCT** on local machine
2. **Connect to source database** (Oracle, SQL Server, etc.)
3. **Connect to target database** (RDS, Aurora, etc.)
4. **Run assessment**: SCT analyzes schema, identifies conversion issues
5. **Convert schema**: Automated conversion of tables, views, stored procedures
6. **Manual fixes**: Review and fix items SCT can't convert automatically
7. **Apply to target**: Deploy converted schema to target database

### Assessment Report
- **Conversion complexity**: % of schema that can be auto-converted
- **Action items**: Manual changes needed (stored procedures, triggers)
- **Recommendations**: Best practices for target database

### Common Conversion Challenges
- **Stored procedures**: Often require manual rewrite (PL/SQL → PL/pgSQL)
- **Proprietary features**: Oracle-specific features may not exist in PostgreSQL
- **Data types**: Some data types don't have direct equivalents
- **Functions**: Built-in functions differ between engines

### SCT Use Cases
- **Oracle → Aurora PostgreSQL** (most common, cost reduction)
- **SQL Server → Aurora MySQL**
- **Oracle → Redshift** (data warehouse migration)
- **Teradata → Redshift**

---

## AWS Application Migration Service (MGN)

### Overview
- **What**: Automated lift-and-shift for servers (formerly CloudEndure Migration)
- **Replaces**: AWS Server Migration Service (SMS) - MGN is newer, recommended
- **Use Case**: Rehost (lift and shift) physical, virtual, or cloud servers to AWS

### How It Works
1. **Install replication agent** on source servers
2. **Continuous replication** to AWS (low-impact on source)
3. **Create staging area** in AWS (replicated servers in lightweight staging)
4. **Test** (non-disruptive testing in AWS)
5. **Cutover** (switch production to AWS)

### Key Features
- **Continuous replication**: Real-time, block-level replication (RPO: seconds)
- **Minimal downtime**: Cutover takes minutes (RTO: minutes)
- **Automated conversion**: Converts source servers to boot and run on AWS
- **Any source**: Physical servers, VMware, Hyper-V, Azure, other clouds
- **Large-scale**: Migrate hundreds of servers simultaneously

### MGN vs DMS
| Feature | MGN (Application Migration Service) | DMS (Database Migration Service) |
|---------|-------------------------------------|----------------------------------|
| **Purpose** | Server migration (OS + apps) | Database migration only |
| **Source** | Physical/virtual servers | Databases |
| **Target** | EC2 instances | RDS, Aurora, Redshift, DynamoDB |
| **Use Case** | Lift-and-shift entire applications | Migrate/replicate databases |

---

## AWS DataSync

### Overview
- **What**: Automated data transfer between on-premises and AWS
- **Protocols**: NFS, SMB
- **Speed**: Up to 10 Gbps per agent
- **Use Case**: One-time migration or scheduled transfers (hourly, daily, weekly)

### DataSync Components
| Component | Purpose |
|-----------|---------|
| **DataSync Agent** | VM running on-premises (connects to NFS/SMB) |
| **Source Location** | On-prem NFS/SMB share or AWS storage |
| **Destination Location** | S3, EFS, FSx for Windows |
| **Task** | Defines source, destination, and schedule |

### DataSync vs Storage Gateway
| Feature | DataSync | Storage Gateway |
|---------|----------|-----------------|
| **Purpose** | One-time or scheduled data transfer | Continuous hybrid access to data |
| **Use Case** | Migrate to AWS, periodic backup | Ongoing on-prem access to AWS storage |
| **Data Flow** | Push data to AWS (scheduled) | Continuous sync (real-time access) |
| **Speed** | Up to 10 Gbps | Network-dependent |

**Example:**
- **DataSync**: Migrate 100 TB of files from on-prem NAS to S3 (one-time or nightly)
- **Storage Gateway**: Access S3 files via on-prem NFS share (continuous)

### DataSync Features
- **Bandwidth throttling**: Limit network usage
- **Data verification**: Checksums to ensure integrity
- **Filtering**: Include/exclude files by pattern
- **Scheduling**: Hourly, daily, weekly transfers
- **Encryption**: TLS for in-transit, KMS for at-rest

---

## AWS Transfer Family

### Overview
- **What**: Managed SFTP/FTPS/FTP server backed by S3 or EFS
- **Use Case**: Replace on-prem FTP servers, B2B file transfers

### Protocols
| Protocol | Security | Use Case |
|----------|----------|----------|
| **SFTP** (SSH File Transfer) | Encrypted | Secure file transfers (recommended) |
| **FTPS** (FTP over TLS) | Encrypted | Legacy systems requiring FTP with TLS |
| **FTP** | Unencrypted | Legacy systems (avoid if possible) |

### Authentication
- **Service-managed**: Users stored in Transfer Family
- **Custom**: Lambda function (integrate with Active Directory, LDAP, etc.)
- **AWS Managed Microsoft AD**: Integrate with existing AD

### Storage Backend
- **Amazon S3**: Files uploaded via SFTP → stored in S3 bucket
- **Amazon EFS**: Files stored in EFS (shared file system)

### Use Cases
- **Replace on-prem SFTP servers** (lift-and-shift to managed service)
- **B2B file transfers** (partners upload files via SFTP to your S3)
- **Compliance**: HIPAA, PCI DSS (encrypted file transfers)

---

## VM Import/Export

### Overview
- **What**: Import/export virtual machine images to/from AWS
- **Supported Formats**: VMDK, VHD, OVA
- **Use Case**: Migrate VMs from on-prem VMware/Hyper-V to EC2

### VM Import (On-Prem → AWS)
1. Export VM from VMware/Hyper-V (as VMDK/VHD)
2. Upload VM image to S3
3. Use `aws ec2 import-image` to convert to AMI
4. Launch EC2 instance from AMI

### VM Export (AWS → On-Prem)
- Export EC2 instance to VMDK/VHD
- Download and import to on-prem hypervisor

### Limitations
- Not all VM configurations supported
- Check AWS documentation for supported OS and configurations
- Consider using **Application Migration Service (MGN)** instead (automated, easier)

---

## AWS Snow Family (Physical Data Transfer)

### Overview
- **What**: Physical devices to transfer large amounts of data to/from AWS
- **Use Case**: Limited internet bandwidth, petabyte-scale migrations

### Snow Devices
| Device | Storage | Use Case | Compute |
|--------|---------|----------|---------|
| **Snowcone** | 8 TB (HDD) or 14 TB (SSD) | Edge computing, small migrations (<10 TB) | 2 vCPUs, 4 GB RAM |
| **Snowball Edge Storage Optimized** | 80 TB | Large data migration (10-100 TB) | 40 vCPUs, 80 GB RAM |
| **Snowball Edge Compute Optimized** | 42 TB + optional GPU | Edge computing, ML inference | 52 vCPUs, 208 GB RAM |
| **Snowmobile** | 100 PB | Exabyte-scale transfer (>10 PB) | N/A (truck-based) |

### Decision Tree
- **<10 TB**: Internet transfer or Snowcone
- **10 TB - 10 PB**: Snowball Edge
- **>10 PB**: Snowmobile

### Snow Family Features
- **Edge computing**: Run EC2 instances, Lambda functions on device
- **DataSync support**: Use DataSync with Snowcone for automated transfer
- **Encryption**: 256-bit encryption, tamper-resistant
- **Clustering**: Multiple Snowballs can work together for durability

### Snowball Workflow
1. **Order** Snow device from AWS Console
2. **Receive** device at your location (ships in 4-6 days)
3. **Connect** to your network
4. **Copy** data to device (use Snowball client or S3 Adapter)
5. **Ship** back to AWS (AWS wipes device after data ingestion)
6. **Import** data appears in S3 bucket

---

## AWS Migration Evaluator

### Overview
- **What**: Creates data-driven business case for AWS migration
- **Output**: Cost estimates, projected savings, migration recommendations
- **Use Case**: Justify migration to leadership (ROI analysis)

### How It Works
1. **Collect data**: Agentless collector or spreadsheet upload
2. **Analyze**: CPU, memory, storage, network utilization
3. **Generate report**: TCO comparison (on-prem vs AWS), projected savings

---

## CloudEndure Disaster Recovery

### Overview
- **What**: Continuous replication for DR (now part of DRS - Elastic Disaster Recovery)
- **RPO**: Seconds (continuous replication)
- **RTO**: Minutes (fast recovery)
- **Use Case**: Disaster recovery from on-prem to AWS or AWS to AWS

### How It Works
- **Continuous replication**: Block-level replication to AWS staging area
- **Low-cost staging**: Replicated data stored in low-cost staging (not full EC2)
- **Failover**: Launch full production instances in minutes
- **Failback**: Replicate back to on-prem after DR event

---

## AWS Elastic Disaster Recovery (DRS)

### Overview
- **What**: AWS-managed disaster recovery service (successor to CloudEndure DR)
- **RPO**: Seconds (continuous replication)
- **RTO**: Minutes (fast failover)
- **Use Case**: DR from on-prem to AWS or AWS region to AWS region

### Features
- **Continuous replication**: Block-level, near-real-time
- **Point-in-time recovery**: Recover to any point in time
- **Automated failover**: Runbooks for orchestrated recovery
- **Non-disruptive testing**: Test DR without affecting production

---

## Migration Patterns & Best Practices

### Pattern 1: Database Migration (Zero Downtime)
**Scenario**: Migrate Oracle database to Aurora PostgreSQL, no downtime

**Solution:**
1. **SCT**: Convert schema (Oracle → PostgreSQL)
2. **DMS**: Full load (migrate existing data)
3. **DMS CDC**: Continuous replication (sync changes)
4. **Test**: Validate data in Aurora
5. **Cutover**: Switch application to Aurora (minimal downtime)

### Pattern 2: Server Migration (Lift and Shift)
**Scenario**: Migrate 100 VMs from VMware to AWS

**Solution:**
1. **Application Discovery Service**: Discover VMs and dependencies
2. **MGN (Application Migration Service)**: Install agents, replicate to AWS
3. **Test**: Launch test instances in AWS
4. **Cutover**: Switch production to AWS (minutes of downtime)

### Pattern 3: Large Data Transfer
**Scenario**: Migrate 500 TB from on-prem NAS to S3

**Solution:**
- **If good network (<1 week)**: DataSync (10 Gbps)
- **If limited network (>1 week)**: Order Snowball Edge devices

**Calculation:**
- 500 TB ÷ 10 Gbps = ~4.6 days (with DataSync)
- 500 TB ÷ 100 Mbps = ~462 days (over internet - use Snowball!)

### Pattern 4: Hybrid Cloud Storage
**Scenario**: On-prem application needs access to S3 data

**Solution:**
- **Storage Gateway (File Gateway)**: Access S3 as NFS/SMB share
- **Caching**: Frequently accessed data cached on-premises (low latency)
- **Async upload**: Changes uploaded to S3 asynchronously

---

## Exam Pattern Recognition

### "Migrate database with zero downtime" → **DMS with CDC** (Full Load + Change Data Capture)

### "Convert Oracle to PostgreSQL" → **SCT** (Schema Conversion Tool) + **DMS**

### "Migrate hundreds of servers to AWS" → **Application Migration Service (MGN)**

### "Transfer large dataset (TB-PB scale)" →
- Good network: **DataSync**
- Limited network: **Snowball Edge**
- Exabyte-scale: **Snowmobile**

### "Continuous disaster recovery, RPO in seconds" → **Elastic Disaster Recovery (DRS)**

### "On-prem NFS access to S3" → **Storage Gateway (File Gateway)**

### "Replace SFTP server" → **AWS Transfer Family (SFTP)**

### "Discover on-prem servers for migration planning" → **Application Discovery Service**

### "Track migration progress across multiple tools" → **Migration Hub**

### "Migrate VMware VMs" → **Application Migration Service (MGN)** or **VMware Cloud on AWS**

---

## Key Exam Tips

### Service Selection
- **Database migration**: DMS (may need SCT for heterogeneous)
- **Server migration**: MGN (Application Migration Service)
- **Data transfer**: DataSync (network) or Snow Family (physical)
- **DR**: Elastic Disaster Recovery (DRS)
- **SFTP replacement**: Transfer Family

### Common Mistakes
- Using **SMS** instead of **MGN** (MGN is newer, always prefer)
- Forgetting **SCT** for heterogeneous database migrations
- Using **DataSync** when network is too slow (use Snowball)
- Confusing **DMS** (database) with **MGN** (servers)

### Decision Criteria
- **Downtime tolerance**:
  - Zero downtime: DMS with CDC, MGN
  - Some downtime OK: VM Import, manual migration
- **Data size**:
  - <10 TB: DataSync or internet
  - 10 TB - 10 PB: Snowball
  - >10 PB: Snowmobile
- **Database type**:
  - Same engine: DMS only
  - Different engine: SCT + DMS
