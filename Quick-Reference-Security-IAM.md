# Security & IAM Quick Reference

## IAM (Identity and Access Management)

### Core Components
| Component | Description | Key Facts |
|-----------|-------------|-----------|
| **User** | Individual person/service | Long-term credentials (access keys) |
| **Group** | Collection of users | Easier permission management |
| **Role** | Temporary credentials for services/users | NO permanent credentials, must be assumed |
| **Policy** | JSON document defining permissions | Attached to users, groups, roles |

### Policy Types
| Type | Scope | Use Case |
|------|-------|----------|
| **Identity-based** | Attached to users/groups/roles | What this identity can do |
| **Resource-based** | Attached to resources (S3 bucket, SQS queue) | Who can access this resource |
| **Permissions Boundary** | Max permissions for identity | Delegate admin, limit permissions |
| **SCP** (Service Control Policy) | AWS Organizations, limit account permissions | Organization-wide guardrails |
| **Session Policy** | Temporary, further restrict assumed role | Pass to AssumeRole for temporary restrictions |

### Policy Structure (JSON)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",  // or "Deny"
      "Action": [         // API actions
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*",  // Which resources
      "Condition": {      // Optional conditions
        "IpAddress": {"aws:SourceIp": "203.0.113.0/24"}
      }
    }
  ]
}
```

### Policy Evaluation Logic (CRITICAL!)
**Order of evaluation:**
1. **Explicit Deny** → Deny (always wins)
2. **SCP** (Organizations) → If deny, deny
3. **Permissions Boundary** → If not allowed, deny
4. **Identity-based or Resource-based** → If allowed, allow
5. **Default** → Deny (implicit deny)

**Key Rule**: Explicit Deny > Explicit Allow > Implicit Deny

**Example:**
- User has `s3:*` permission (Allow)
- Permission boundary allows only `s3:GetObject`
- Result: User can ONLY do `s3:GetObject` (boundary limits)

---

## IAM Roles

### Common Use Cases
| Use Case | How It Works |
|----------|--------------|
| **EC2 Instance Role** | EC2 assumes role to access AWS services (e.g., S3) |
| **Lambda Execution Role** | Lambda assumes role to access other services |
| **Cross-Account Access** | Account A assumes role in Account B |
| **Identity Federation** | External users (SAML, OIDC) assume role |

### Cross-Account Access
**Scenario**: Account A (dev) needs access to S3 bucket in Account B (prod)

**Steps:**
1. Account B creates IAM role with trust policy for Account A
2. Account B attaches permissions policy to role (e.g., `s3:GetObject`)
3. Account A user calls `AssumeRole` to get temporary credentials
4. Account A user uses temporary credentials to access Account B resources

**Trust Policy** (in Account B):
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::ACCOUNT-A-ID:root"},
    "Action": "sts:AssumeRole"
  }]
}
```

### IAM Roles vs Users
| Feature | User | Role |
|---------|------|------|
| **Credentials** | Long-term (access keys) | Temporary (assumed) |
| **Use case** | Specific person/service | Services, cross-account, federation |
| **Best practice** | Avoid for applications | Prefer for EC2, Lambda, services |

---

## AWS Organizations

### Overview
- **What**: Centrally manage multiple AWS accounts
- **Structure**: Root OU → OUs → Accounts
- **Consolidated billing**: One bill for all accounts
- **Volume discounts**: Aggregated usage across accounts

### Service Control Policies (SCPs)
- **What**: Max permissions for accounts (guardrails)
- **Scope**: Apply to OUs or accounts
- **Does NOT grant permissions**: Only limits (still need IAM policies)
- **Root account**: SCPs do NOT apply to management account root user

**Example SCP** (deny access to specific region):
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {"aws:RequestedRegion": ["us-east-1", "eu-west-1"]}
    }
  }]
}
```

### Best Practices
- **Separate accounts**: Dev, test, prod (blast radius, billing)
- **SCPs**: Enforce compliance (e.g., deny leaving regions, deny root user)
- **Control Tower**: Automate multi-account setup with best practices

---

## Encryption

### Encryption at Rest
| Service | Options | Key Management |
|---------|---------|----------------|
| **S3** | SSE-S3, SSE-KMS, SSE-C | AWS, KMS, Customer |
| **EBS** | AES-256 via KMS | KMS |
| **RDS** | AES-256 via KMS | KMS |
| **DynamoDB** | AWS owned, AWS managed, Customer managed KMS | AWS or KMS |
| **EFS** | KMS | KMS |

### Encryption in Transit
- **HTTPS/TLS**: API calls, S3, RDS connections
- **VPN**: IPSec for Site-to-Site VPN
- **Direct Connect**: Not encrypted by default (use VPN over Direct Connect for encryption)

---

## KMS (Key Management Service)

### Overview
- **What**: Managed service to create and control encryption keys
- **Key types**:
  - **Symmetric (AES-256)**: Single key for encrypt/decrypt (most common)
  - **Asymmetric (RSA, ECC)**: Public/private key pair

### Key Types
| Type | Description | Use Case |
|------|-------------|----------|
| **AWS managed CMK** | Created by AWS for service (e.g., aws/s3) | Default, no control |
| **Customer managed CMK** | You create and manage | Full control, key rotation, policies |
| **AWS owned CMK** | AWS owns, used internally | No visibility, no control |
| **CloudHSM** | Dedicated hardware, FIPS 140-2 Level 3 | Regulatory compliance, single-tenant |

### Key Policies
- **What**: Resource-based policy for CMK (who can use key)
- **Default**: Root user has full access
- **Grant**: Temporary, programmatic access to keys

**Example Key Policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::123456789012:role/MyRole"},
    "Action": ["kms:Encrypt", "kms:Decrypt"],
    "Resource": "*"
  }]
}
```

### Key Rotation
- **Customer managed CMK**: Automatic rotation every 1 year (optional)
- **AWS managed CMK**: Automatic rotation every 3 years
- **Manual rotation**: Create new key, update aliases

### Envelope Encryption
- **Concept**: Encrypt data with Data Key, encrypt Data Key with CMK
- **Why**: Efficient (don't send all data to KMS), only encrypt small data key
- **Services**: S3, EBS, etc. use envelope encryption with KMS

---

## Secrets Manager vs Parameter Store

| Feature | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| **Purpose** | Store secrets (passwords, API keys) | Store config, secrets |
| **Rotation** | Automatic rotation (Lambda) | No automatic rotation |
| **Cost** | $0.40 per secret per month | Free (standard), $0.05 per advanced |
| **Versioning** | Yes | Yes |
| **Encryption** | KMS (automatic) | Optional (KMS) |
| **Cross-account** | Yes | No (same account only) |
| **Integration** | RDS auto-rotation | AppConfig, Systems Manager |
| **Use case** | Database credentials, API keys | App configs, non-sensitive data |

**Exam Tip**: "Automatic secret rotation" → **Secrets Manager**. "Cost-effective config storage" → **Parameter Store**.

---

## AWS Certificate Manager (ACM)

### Overview
- **What**: Provision, manage, deploy SSL/TLS certificates
- **Cost**: Free for public certificates (used with AWS services)
- **Integration**: ALB, NLB, CloudFront, API Gateway, Elastic Beanstalk
- **Renewal**: Automatic for ACM-issued certs

### Private Certificate Authority (PCA)
- **What**: Create private CA for internal apps
- **Cost**: $400/month per CA + issued certificates
- **Use case**: Internal services, private network

---

## Security Services

### AWS WAF (Web Application Firewall)
- **What**: Protect web apps from common exploits (SQL injection, XSS)
- **Deploy on**: ALB, API Gateway, CloudFront, AppSync
- **Rules**: IP sets, rate limiting, geo-blocking, SQL injection/XSS protection
- **Web ACL**: Collection of rules
- **Pricing**: Per Web ACL + per rule + per request

### AWS Shield
- **What**: DDoS protection
- **Shield Standard**: Free, automatic (Layer 3/4 DDoS protection)
- **Shield Advanced**: $3,000/month, 24/7 DRT (DDoS Response Team), cost protection
- **Use case**: Protect against large-scale DDoS attacks

### AWS Firewall Manager
- **What**: Centrally manage WAF rules, Shield Advanced, Security Groups across accounts
- **Requires**: AWS Organizations
- **Use case**: Enforce WAF rules across multiple accounts/resources

### GuardDuty
- **What**: Threat detection using ML (analyzes logs)
- **Data sources**: CloudTrail, VPC Flow Logs, DNS logs
- **Findings**: Compromised instances, reconnaissance, account compromise
- **Actions**: EventBridge → SNS/Lambda for automated response
- **Pricing**: Pay per GB analyzed

### Inspector
- **What**: Automated security assessment
- **Scope**: EC2 instances, container images, Lambda functions
- **Checks**: Vulnerabilities, network exposure, CVEs
- **Output**: Risk scores, remediation recommendations

### Macie
- **What**: Discover and protect sensitive data in S3 (PII, secrets)
- **Uses**: ML to identify sensitive data
- **Alerts**: Findings to EventBridge, Security Hub
- **Use case**: Data privacy compliance (GDPR, HIPAA)

### Security Hub
- **What**: Centralized security dashboard (aggregates findings)
- **Integrations**: GuardDuty, Inspector, Macie, IAM Access Analyzer, Systems Manager
- **Standards**: CIS AWS Foundations, PCI DSS
- **Output**: Findings → EventBridge for automated remediation

### CloudTrail
- **What**: Audit log (API calls across AWS)
- **Events**:
  - **Management events**: Control plane (CreateBucket, TerminateInstance) - enabled by default
  - **Data events**: Data plane (GetObject, PutObject) - must enable, extra cost
- **Storage**: S3 bucket (encrypted)
- **Insights**: Detect unusual activity (ML-based)
- **Use case**: Compliance, forensics, troubleshooting

### Config
- **What**: Track resource configuration changes over time
- **Rules**: Evaluate compliance (e.g., "all S3 buckets must have versioning")
- **Remediation**: Automatic (SSM Automation) or manual
- **Use case**: Compliance auditing, resource inventory, change tracking

### AWS Artifact
- **What**: Portal for compliance reports (SOC, PCI, ISO)
- **Use case**: Download audit reports for compliance evidence

### Detective
- **What**: Investigate security findings (root cause analysis)
- **Data**: CloudTrail, VPC Flow Logs, GuardDuty findings
- **Uses**: ML and graph analysis to visualize activity
- **Use case**: After GuardDuty alert, use Detective to investigate

---

## Identity Federation

### SAML 2.0
- **What**: Federate corporate identities (Active Directory) to AWS
- **Flow**: User → IdP (ADFS) → SAML assertion → AWS STS → temporary credentials
- **Use case**: Enterprise SSO to AWS console/CLI

### AWS SSO (Identity Center)
- **What**: Centralized SSO for AWS accounts and business apps
- **Integrations**: Microsoft AD, Okta, Azure AD, Google Workspace
- **Use case**: Simplified multi-account access, SSO to 3rd-party apps

### Cognito
- **What**: User authentication for web/mobile apps
- **User Pools**: User directory (sign-up, sign-in, MFA)
- **Identity Pools**: Provide temporary AWS credentials to users (access AWS services)
- **Federation**: Google, Facebook, Amazon, SAML, OIDC
- **Use case**: Mobile/web app authentication, social login

---

## Best Practices

### IAM Best Practices
1. **Enable MFA** on root account and privileged users
2. **Never use root account** for daily tasks
3. **Use roles instead of access keys** for EC2, Lambda
4. **Principle of least privilege**: Grant only necessary permissions
5. **Rotate credentials regularly** (90 days)
6. **Use groups** to assign permissions (not individual users)
7. **Enable CloudTrail** for audit logging

### Encryption Best Practices
1. **Encrypt at rest**: Use KMS for EBS, S3, RDS
2. **Encrypt in transit**: Use HTTPS, TLS, VPN
3. **Rotate keys**: Enable automatic key rotation for CMKs
4. **Separate keys**: Different keys for dev, test, prod

### Network Security Best Practices
1. **Security Groups**: Deny all by default, allow specific ports
2. **NACLs**: Additional layer, block IPs if needed
3. **Private subnets**: Place databases, app servers in private subnets
4. **VPC Flow Logs**: Monitor traffic, detect anomalies
5. **Least privilege**: Limit outbound traffic from instances

---

## Exam Pattern Recognition

### "Temporary credentials for EC2/Lambda" → **IAM Role**

### "Cross-account access" → **IAM Role with trust policy** (AssumeRole)

### "Automatic secret rotation for database" → **Secrets Manager**

### "Cost-effective config storage" → **Systems Manager Parameter Store**

### "DDoS protection" → **Shield Standard** (free) or **Shield Advanced** (paid)

### "Web app protection (SQL injection, XSS)" → **AWS WAF**

### "Threat detection, compromised instances" → **GuardDuty**

### "Discover PII in S3" → **Macie**

### "Audit API calls" → **CloudTrail**

### "Track resource configuration changes" → **Config**

### "Centralized security findings" → **Security Hub**

### "Company-managed encryption keys" → **KMS Customer Managed CMK**

### "Regulatory compliance, FIPS 140-2 Level 3" → **CloudHSM**

### "Federate corporate AD to AWS" → **SAML 2.0** or **AWS SSO**

### "User authentication for mobile app" → **Cognito User Pools**

### "Mobile app access to S3" → **Cognito Identity Pools**
