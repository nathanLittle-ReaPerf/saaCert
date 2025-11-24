# Day 2 Catch-Up: Auto Scaling & Load Balancing

**Study Focus:** Essential concepts for AWS Auto Scaling Groups and Elastic Load Balancing (exam-critical topics)

---

## Auto Scaling Groups (ASG)

### Core Concepts

**What is an Auto Scaling Group?**
- Automatically adjusts the number of EC2 instances based on demand
- Ensures you have the right number of instances to handle application load
- Works with Load Balancers to distribute traffic across healthy instances

**Key Components:**
- **Launch Template/Configuration**: Defines what instances to launch (AMI, instance type, security groups, key pair)
- **Min/Max/Desired Capacity**: Controls how many instances run
  - Min: Minimum number of instances (always running)
  - Max: Maximum number of instances (cost ceiling)
  - Desired: Target number of instances (ASG tries to maintain this)
- **Health Checks**: EC2 status checks OR ELB health checks (ELB is more application-aware)
- **Scaling Policies**: Rules that trigger scale-in (terminate) or scale-out (launch) actions

### Scaling Policies (CRITICAL FOR EXAM)

#### 1. Target Tracking Scaling (EASIEST & MOST COMMON)
**How it works:** You specify a target metric value, ASG automatically adjusts to maintain it
- **Example**: "Keep average CPU utilization at 50%"
- **Common Metrics**:
  - `ASGAverageCPUUtilization` (CPU usage across all instances)
  - `ALBRequestCountPerTarget` (requests per instance from ALB)
  - `ASGAverageNetworkIn/Out` (network traffic)
- **Pros**: Simplest to configure, AWS handles the math
- **Cons**: Only works with metrics that change proportionally with capacity

**Exam Tip:** Questions asking for "LEAST operational overhead" for scaling → **Target Tracking**

#### 2. Step Scaling
**How it works:** Scale based on CloudWatch alarm thresholds with different scaling amounts per step
- **Example**:
  - CPU > 70% → Add 2 instances
  - CPU > 85% → Add 4 instances
  - CPU < 30% → Remove 1 instance
- **Pros**: More granular control, can scale aggressively when needed
- **Cons**: Requires manual CloudWatch alarm configuration

**Exam Tip:** Questions asking for "scale differently based on alarm severity" → **Step Scaling**

#### 3. Scheduled Scaling
**How it works:** Scale based on predictable time patterns
- **Example**:
  - Scale to 10 instances at 8 AM Monday-Friday (business hours)
  - Scale to 2 instances at 6 PM (after hours)
- **Use Case**: Predictable traffic patterns (business hours, weekend traffic, holiday sales)

**Exam Tip:** Questions mentioning "predictable load patterns", "business hours", "weekend traffic" → **Scheduled Scaling**

#### 4. Simple Scaling (Legacy - Know for Exam)
**How it works:** Single scaling action based on CloudWatch alarm
- **Difference from Step**: Waits for cooldown period after each scaling activity
- **Exam Tip**: If question mentions "cooldown period" → Simple Scaling (but AWS recommends Step Scaling instead)

### Scaling Policy Comparison Table

| Policy Type | Complexity | Use Case | Exam Keywords |
|------------|-----------|----------|---------------|
| **Target Tracking** | Low | Maintain specific metric value | "Least operational overhead", "simplest" |
| **Step Scaling** | Medium | Different scaling amounts per threshold | "Multiple thresholds", "aggressive scaling" |
| **Scheduled** | Low | Predictable time-based patterns | "Business hours", "predictable load" |
| **Simple** | Medium (legacy) | Basic alarm-based scaling with cooldown | "Cooldown period" |

### Important ASG Concepts

**Cooldown Period:**
- Default: 300 seconds (5 minutes)
- Prevents ASG from launching/terminating additional instances before previous scaling activity takes effect
- **Target Tracking** has built-in intelligent cooldown (better than Simple Scaling)

**Termination Policies (Which Instance to Terminate First?):**
1. **Default**: Select AZ with most instances → select instance with oldest launch configuration/template
2. **OldestInstance**: Terminate the oldest instance (useful for upgrading to new instance types)
3. **NewestInstance**: Terminate newest instance
4. **OldestLaunchConfiguration**: Helpful when updating launch configurations
5. **ClosestToNextInstanceHour**: Minimize costs

**Lifecycle Hooks:**
- Pause instance launch/termination to perform custom actions
- **Use Cases**:
  - Install software before instance goes in service
  - Extract logs before instance terminates
  - Register instance with external system

---

## Elastic Load Balancing (ELB)

### Three Types of Load Balancers

#### 1. Application Load Balancer (ALB) - Layer 7 (HTTP/HTTPS)

**Best For:** Web applications, microservices, containers

**Key Features:**
- **Layer 7** (Application layer) - can inspect HTTP/HTTPS traffic
- **Path-based routing**: Route /api/* to one target group, /images/* to another
- **Host-based routing**: Route api.example.com vs www.example.com to different targets
- **Query string/header routing**: Route based on HTTP headers or query parameters
- **WebSocket and HTTP/2 support**
- **Native integration with ECS, EKS** (container services)
- **Authentication**: Integrated with Cognito, OIDC (Google, Facebook login)
- **Fixed hostname**: xxx.region.elb.amazonaws.com
- **Target Types**:
  - EC2 instances
  - IP addresses (useful for on-premises servers, containers)
  - Lambda functions (serverless)
  - Another ALB (for advanced routing)

**ALB Routing Rules:**
```
Example Rules (evaluated in priority order):
1. Priority 1: /api/* → API Target Group
2. Priority 2: /images/* → Image Server Target Group
3. Priority 3: Host: mobile.example.com → Mobile Target Group
4. Default: /* → Default Target Group
```

**Sticky Sessions (Session Affinity):**
- Routes requests from same client to same target
- **Cookie Types**:
  - **Application-based cookie**: Custom cookie name (generated by application or ALB)
  - **Duration-based cookie** (AWSALB): Generated by ALB, 1 second to 7 days
- **Use Case**: User session data stored on specific instance (though better to use ElastiCache/DynamoDB for sessions)

**Cross-Zone Load Balancing:**
- **Enabled by default** for ALB (no additional charges)
- Distributes traffic evenly across all registered targets in all enabled AZs

**Exam Tips for ALB:**
- Keywords: "HTTP/HTTPS", "path-based routing", "microservices", "containers", "WebSocket", "Lambda targets" → **ALB**
- "Authenticate users with social identity providers" → **ALB with Cognito**
- "Route based on URL path" → **ALB**

---

#### 2. Network Load Balancer (NLB) - Layer 4 (TCP/UDP/TLS)

**Best For:** Extreme performance, millions of requests per second, low latency, TCP/UDP traffic

**Key Features:**
- **Layer 4** (Transport layer) - forwards TCP/UDP traffic (no inspection of application data)
- **Ultra-high performance**: Millions of requests per second, < 100 ms latency
- **Static IP support**: One static IP per AZ (can also use Elastic IP)
  - **Critical for whitelisting**: Clients can whitelist specific IPs
- **Preserve source IP**: Targets see actual client IP (not load balancer IP)
- **Protocol support**: TCP, UDP, TLS
- **Target Types**:
  - EC2 instances
  - IP addresses
  - ALB (yes, you can put ALB behind NLB for static IP + Layer 7 features)
- **Health Checks**: TCP, HTTP, HTTPS

**Cross-Zone Load Balancing:**
- **Disabled by default** for NLB
- **Charges apply** if you enable it

**Exam Tips for NLB:**
- Keywords: "extreme performance", "millions of requests/sec", "low latency", "static IP", "Elastic IP", "TCP/UDP", "source IP preservation", "non-HTTP protocols" → **NLB**
- "Client needs to whitelist load balancer IPs" → **NLB with Elastic IP**
- "Gaming traffic", "IoT", "TCP traffic" → **NLB**

---

#### 3. Gateway Load Balancer (GLB) - Layer 3 (IP Packets)

**Best For:** Third-party virtual appliances (firewalls, IDS/IPS, deep packet inspection)

**Key Features:**
- **Layer 3** (Network layer) - operates at IP protocol level
- **Transparent network gateway**: Single entry/exit point for traffic
- **Distributes traffic to fleet of virtual appliances**: Firewalls, IDS/IPS, deep packet inspection tools
- **GENEVE protocol** on port 6081
- **Target Types**: EC2 instances (virtual appliances), IP addresses
- **Use Case**: Deploy/scale third-party security appliances

**How it Works:**
```
Internet → GLB → Target Group (Security Appliances) → GLB → Application
```

**Exam Tips for GLB:**
- Keywords: "third-party security appliances", "firewall fleet", "intrusion detection", "deep packet inspection", "transparent network gateway" → **GLB**
- "Deploy and scale Palo Alto firewalls" → **GLB**

---

### Load Balancer Comparison Table

| Feature | ALB | NLB | GLB |
|---------|-----|-----|-----|
| **OSI Layer** | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP/TLS) | Layer 3 (IP) |
| **Performance** | Moderate (thousands of requests/sec) | Extreme (millions of requests/sec) | High |
| **Latency** | Higher (~ms) | Ultra-low (< 100 ms) | Low |
| **Static IP** | No (dynamic hostname) | Yes (one per AZ) | Yes |
| **Elastic IP** | No | Yes | Yes |
| **Preserve Source IP** | Via X-Forwarded-For header | Yes (native) | Yes |
| **Path-based Routing** | Yes | No | No |
| **Host-based Routing** | Yes | No | No |
| **WebSocket** | Yes | Yes (as TCP) | No |
| **TLS Termination** | Yes | Yes | No |
| **Target Types** | EC2, IP, Lambda, ALB | EC2, IP, ALB | EC2, IP |
| **Authentication (Cognito)** | Yes | No | No |
| **Cross-Zone LB Default** | Enabled (free) | Disabled (charges if enabled) | No |
| **Use Cases** | Web apps, microservices, containers | TCP/UDP apps, extreme performance, gaming, IoT | Third-party appliances, firewalls, IDS/IPS |

---

### Important Load Balancer Concepts

#### Connection Draining / Deregistration Delay

**What it is:**
- Time to complete "in-flight requests" before deregistering an instance
- When instance is unhealthy or you manually deregister it, ELB stops sending NEW requests but allows existing requests to complete

**Settings:**
- **Default**: 300 seconds (5 minutes)
- **Range**: 1 - 3600 seconds (1 second to 1 hour)
- **Best Practice**: Set based on typical request completion time
  - Short requests (APIs): 30-60 seconds
  - Long requests (video uploads): 300+ seconds

**Exam Tip:** "Ensure in-flight requests complete before terminating instances" → **Connection Draining / Deregistration Delay**

---

#### Health Checks

**How it works:**
- Load balancer sends periodic requests to targets to check health
- **Healthy**: Instance responds with HTTP 200 within timeout period
- **Unhealthy**: Instance fails health check → Load balancer stops routing traffic to it

**Key Settings:**
- **Protocol**: HTTP, HTTPS, TCP, or TLS (depends on LB type)
- **Path**: For HTTP/HTTPS (e.g., `/health` or `/ping`)
- **Interval**: How often to check (default 30 seconds, minimum 5 seconds)
- **Timeout**: How long to wait for response (default 5 seconds)
- **Healthy Threshold**: Consecutive successful checks needed to mark healthy (default 5)
- **Unhealthy Threshold**: Consecutive failed checks needed to mark unhealthy (default 2)

**For ASG Integration:**
- ASG can use **EC2 health checks** (is instance running?) OR **ELB health checks** (is application responding?)
- **ELB health checks are more reliable** for application failures
- If ELB marks instance unhealthy, ASG will terminate and replace it

**Exam Tip:**
- "Instance running but application crashed" → **Use ELB health checks with ASG**
- "Health check on specific application endpoint" → **Configure custom health check path**

---

#### Sticky Sessions (Session Affinity)

**What it is:**
- Ensures requests from the same client always go to the same target instance

**Cookie Types:**
1. **Application-based cookies:**
   - **Custom cookie**: Generated by your application (any name except AWSALB, AWSALBAPP, AWSALBTG)
   - **Application cookie (AWSALBAPP)**: Generated by ALB, includes custom attributes from target
2. **Duration-based cookie (AWSALB):**
   - Generated by load balancer
   - Duration: 1 second to 7 days

**Use Case:**
- User session data stored on specific EC2 instance (not ideal - should use ElastiCache/DynamoDB for distributed sessions)

**Exam Tip:** "Users losing session data after being routed to different instance" → **Enable Sticky Sessions** (but better solution: use ElastiCache for session storage)

---

#### Cross-Zone Load Balancing

**What it is:**
- Distributes traffic evenly across ALL registered targets in ALL enabled Availability Zones
- **Without Cross-Zone**: Traffic distributed evenly to AZs, then to instances in that AZ
  - Problem: If AZ-A has 2 instances and AZ-B has 8 instances, each AZ-A instance gets 25% traffic but AZ-B instances get 6.25% each
- **With Cross-Zone**: Traffic distributed evenly to ALL instances regardless of AZ
  - Each instance gets 10% traffic (100% / 10 instances)

**Cross-Zone Load Balancing by Load Balancer Type:**

| Load Balancer | Default | Charges |
|--------------|---------|---------|
| **ALB** | Enabled | No charges (free) |
| **NLB** | Disabled | Charges apply if enabled |
| **GLB** | Disabled | Charges apply if enabled |

**Exam Tip:**
- "Uneven traffic distribution across instances in different AZs" → **Enable Cross-Zone Load Balancing**
- NLB questions about additional costs → **Cross-Zone Load Balancing charges**

---

## Exam Pattern Recognition: When to Use What?

### Scenario Keywords → Solution Mapping

| Scenario Keywords | Solution |
|------------------|----------|
| "HTTP/HTTPS traffic", "path-based routing", "microservices" | ALB |
| "Millions of requests per second", "ultra-low latency", "static IP" | NLB |
| "Third-party firewall", "intrusion detection", "security appliance" | GLB |
| "Least operational overhead" for scaling | Target Tracking Policy |
| "Business hours traffic pattern", "predictable load" | Scheduled Scaling |
| "Different scaling amounts based on alarm severity" | Step Scaling |
| "Users losing session data" | Sticky Sessions OR ElastiCache for sessions |
| "In-flight requests must complete before termination" | Connection Draining / Deregistration Delay |
| "Application health checks" | ELB health checks (not EC2 status checks) |
| "Uneven traffic across AZs" | Cross-Zone Load Balancing |

---

## Key Exam Tips - Memorize These

### Auto Scaling Golden Rules:
1. **Target Tracking** = simplest, least operational overhead
2. **Step Scaling** = multiple thresholds with different scaling amounts
3. **Scheduled Scaling** = predictable time-based patterns
4. **Always use ELB health checks** (not EC2 checks) for application-aware health monitoring
5. **Cooldown period** prevents scaling thrashing (default 300 seconds)

### Load Balancer Golden Rules:
1. **ALB** = Layer 7, HTTP/HTTPS, path/host routing, Lambda targets, Cognito auth
2. **NLB** = Layer 4, ultra-performance, static IP, TCP/UDP, preserve source IP
3. **GLB** = Layer 3, third-party security appliances
4. **Connection draining** = complete in-flight requests (default 300 sec, range 1-3600)
5. **ALB cross-zone is FREE**, **NLB cross-zone has CHARGES**
6. **Sticky sessions** solve session data problems (but ElastiCache is better)

### Common Exam Traps:
- ❌ Don't use Simple Scaling (it's legacy) → Use Target Tracking or Step Scaling
- ❌ Don't use EC2 status checks if application can crash → Use ELB health checks
- ❌ Don't use sticky sessions for session storage → Use ElastiCache/DynamoDB
- ✅ ALB can have Lambda as target (unique feature)
- ✅ NLB can have ALB as target (for static IP + Layer 7 routing)
- ✅ NLB is the ONLY load balancer that supports static IP and Elastic IP

---

## Practice Scenarios

### Scenario 1: Scaling Policy Selection
**Question:** A company runs a web application that experiences variable traffic throughout the day. They want to maintain an average CPU utilization of 60% across all instances with minimal configuration effort. What should they implement?

**Answer:** Target Tracking Scaling Policy
- Set target value to 60% for ASGAverageCPUUtilization
- Least operational overhead (no CloudWatch alarms needed)
- AWS automatically calculates scaling actions

---

### Scenario 2: Load Balancer Selection
**Question:** A gaming company needs to load balance UDP traffic for their multiplayer game servers. They require ultra-low latency (< 100 ms) and need to provide customers with static IPs for firewall whitelisting. Which load balancer should they use?

**Answer:** Network Load Balancer (NLB)
- Supports UDP protocol (Layer 4)
- Ultra-low latency (< 100 ms)
- Provides static IP per AZ (can also assign Elastic IPs)
- ALB doesn't support UDP, GLB is for security appliances

---

### Scenario 3: Routing Requirements
**Question:** A company has a web application with the following requirements:
- Route `/api/*` requests to API servers
- Route `/images/*` requests to image processing servers
- Authenticate users using Google and Facebook login
Which solution meets these requirements with LEAST operational overhead?

**Answer:** Application Load Balancer (ALB) with Cognito
- Path-based routing for `/api/*` and `/images/*` to different target groups
- Cognito integration for social identity authentication (Google, Facebook)
- Single ALB handles both routing and authentication
- NLB doesn't support path-based routing or authentication

---

### Scenario 4: Health Check Strategy
**Question:** An Auto Scaling group uses EC2 status checks, but instances sometimes pass status checks while the application has crashed. Users report intermittent connection failures. What should the solutions architect do?

**Answer:** Configure ASG to use ELB health checks instead of EC2 status checks
- EC2 status checks only verify instance is running (not application health)
- ELB health checks test application endpoint (e.g., `/health`)
- ASG will terminate and replace instances that fail ELB health checks
- More reliable for application failures

---

### Scenario 5: Traffic Distribution
**Question:** A company has an Application Load Balancer distributing traffic to instances in two Availability Zones. AZ-A has 2 instances and AZ-B has 10 instances. They notice that instances in AZ-A are receiving 5x more traffic per instance than AZ-B. What's the most likely cause?

**Answer:** Cross-Zone Load Balancing is disabled (but wait, ALB has it enabled by default!)
- Actually, this scenario shouldn't happen with ALB since cross-zone is enabled by default
- If this were an NLB, cross-zone would be disabled by default causing this issue
- **For exam**: Know that ALB cross-zone is enabled (free), NLB is disabled (charges apply)

---

## Next Steps

Now that you've reviewed Day 2 material:

1. ✅ **Self-Quiz**: Can you explain the difference between ALB, NLB, and GLB from memory?
2. ✅ **Self-Quiz**: When would you use Target Tracking vs Step Scaling vs Scheduled Scaling?
3. ✅ **Practice**: Try 10-20 practice questions on Auto Scaling and Load Balancing
4. ✅ **Hands-On (if time)**: Launch an ASG with ALB in AWS console (Free Tier eligible)
5. ✅ **Review**: Come back to this guide before your Week 1 practice exam (Day 7)

---

**You're now caught up on Day 2! Let's move on to Day 4 (S3 Security & Replication) when you're ready.**
