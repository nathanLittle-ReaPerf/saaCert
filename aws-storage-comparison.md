# AWS Storage Types Comparison

## AWS EBS Volume Types

| Type | Max IOPS | Max Throughput | Latency | Size Range | Use Case | Cost |
|------|----------|----------------|---------|------------|----------|------|
| **gp3** | 16,000 | 1,000 MB/s | Single-digit ms | 1 GB - 16 TB | General purpose, cost-effective | Base: 3,000 IOPS, 125 MB/s included |
| **gp2** | 16,000 | 250 MB/s | Single-digit ms | 1 GB - 16 TB | General purpose (legacy) | IOPS scales with size (3 IOPS/GB) |
| **io2 Block Express** | 256,000 | 4,000 MB/s | Sub-millisecond | 4 GB - 64 TB | Mission-critical, highest performance | Premium, pay per provisioned IOPS |
| **io2** | 64,000 | 1,000 MB/s | Single-digit ms | 4 GB - 16 TB | I/O intensive databases | Premium, pay per provisioned IOPS |
| **io1** | 64,000 | 1,000 MB/s | Single-digit ms | 4 GB - 16 TB | I/O intensive (legacy) | Premium, pay per provisioned IOPS |
| **st1** | 500 | 500 MB/s | N/A (throughput-optimized) | 125 GB - 16 TB | Big data, log processing | Low cost, throughput focused |
| **sc1** | 250 | 250 MB/s | N/A (throughput-optimized) | 125 GB - 16 TB | Infrequent access | Lowest cost EBS |

## Other AWS Storage Types

| Type | IOPS | Throughput | Latency | Use Case | Notes |
|------|------|------------|---------|----------|-------|
| **EFS Standard** | Up to 500,000+ | 10+ GB/s (read) | Low single-digit ms | Shared file storage | Scales automatically, pay per use |
| **EFS One Zone** | Up to 500,000+ | 10+ GB/s (read) | Low single-digit ms | Shared file storage (single AZ) | 47% cheaper than Standard |
| **S3 Standard** | 5,500 (read) / 3,500 (write) per prefix | N/A | ~100-200 ms | Object storage, backups | Unlimited scalability, pay per GB |
| **Instance Store** | Varies (NVMe: millions) | Up to 60 GB/s+ | Microseconds | Temporary, high-performance | Ephemeral, lost on stop/terminate |
| **FSx for Lustre** | Millions | Up to 1+ TB/s | Sub-millisecond | HPC, ML training | High-performance parallel file system |
| **FSx for Windows** | Up to 2 million | Up to 12.5 GB/s | Sub-millisecond | Windows applications | Fully managed Windows file server |

## Key Notes:

- **gp3** is generally recommended over gp2 (more flexible, better price/performance)
- **io2** has 100x better durability than io1 (99.999% vs 99.9%)
- **IOPS limits** also depend on EC2 instance type capabilities
- **EBS Multi-Attach** is available for io1/io2 only
- **Burst credits** apply to gp2, st1, sc1 volumes
