# Day 2 Quiz: Auto Scaling & Load Balancing

**Instructions:** Answer all 10 questions. Answers and explanations are at the bottom (don't cheat!).

---

## Question 1
A company's web application experiences variable traffic throughout the day. The solutions architect wants to maintain an average CPU utilization of 50% across all instances with minimal configuration and management overhead. Which Auto Scaling policy should be implemented?

**A)** Simple Scaling with a CloudWatch alarm for CPU > 50%
**B)** Target Tracking Scaling with ASGAverageCPUUtilization set to 50%
**C)** Step Scaling with multiple CloudWatch alarms
**D)** Scheduled Scaling with hourly capacity adjustments

---

## Question 2
A financial services company needs to deploy a fleet of third-party firewall appliances to inspect all traffic before it reaches their application servers. The solution must distribute traffic across multiple firewall instances in different Availability Zones. Which AWS service should they use?

**A)** Application Load Balancer with path-based routing
**B)** Network Load Balancer with TCP listeners
**C)** Gateway Load Balancer with GENEVE protocol
**D)** AWS WAF with AWS Shield

---

## Question 3
A gaming company is deploying multiplayer game servers that use UDP protocol. They need a load balancing solution that provides:
- Ultra-low latency (< 100 ms)
- Static IP addresses for players to connect to
- Support for millions of requests per second

Which load balancer type meets these requirements?

**A)** Application Load Balancer (ALB)
**B)** Network Load Balancer (NLB)
**C)** Classic Load Balancer (CLB)
**D)** Gateway Load Balancer (GLB)

---

## Question 4
A company runs a microservices application with the following routing requirements:
- Route `/api/users/*` to the User Service
- Route `/api/orders/*` to the Order Service
- Route `/images/*` to the Image Service
- Authenticate users using Google and Facebook social login

Which solution provides these capabilities with the LEAST operational overhead?

**A)** Network Load Balancer with Lambda functions for routing logic
**B)** Application Load Balancer with path-based routing and Cognito integration
**C)** Three separate Network Load Balancers for each service
**D)** API Gateway with custom Lambda authorizers

---

## Question 5
An Auto Scaling group is configured with EC2 status checks. Instances occasionally pass these checks even when the application has crashed, causing users to receive connection errors. What should the solutions architect do to resolve this issue?

**A)** Increase the health check grace period
**B)** Change the health check type to ELB health checks targeting the application endpoint
**C)** Enable detailed CloudWatch monitoring
**D)** Reduce the health check interval to 5 seconds

---

## Question 6
A company runs a data processing application that experiences predictable load patterns:
- High traffic Monday-Friday from 8 AM to 6 PM (10 instances needed)
- Low traffic evenings and weekends (2 instances needed)
- Occasional spikes during business hours

Which combination of scaling policies provides the MOST cost-effective solution?

**A)** Target Tracking Scaling only
**B)** Step Scaling with multiple thresholds
**C)** Scheduled Scaling for predictable patterns + Target Tracking for unexpected spikes
**D)** Simple Scaling with cooldown periods

---

## Question 7
A solutions architect is configuring a Network Load Balancer. They notice that instances in AZ-A (2 instances) are receiving significantly more traffic per instance than AZ-B (8 instances). What is the MOST likely cause?

**A)** The security groups are misconfigured
**B)** Cross-Zone Load Balancing is disabled
**C)** Connection draining is not enabled
**D)** Health checks are failing in AZ-B

---

## Question 8
A company is migrating an application that stores user session data locally on EC2 instances. After deploying an Application Load Balancer, users report being logged out randomly. Which solution fixes this issue?

**A)** Enable sticky sessions (session affinity) on the ALB
**B)** Increase the ALB idle timeout
**C)** Enable cross-zone load balancing
**D)** Store session data in ElastiCache instead of local storage

---

## Question 9
During a deployment, an Auto Scaling group is updating instances with a new version of the application. Some instances are processing long-running requests that take 2-3 minutes to complete. What should be configured to ensure these requests complete successfully before instance termination?

**A)** Increase the cooldown period to 180 seconds
**B)** Configure connection draining / deregistration delay to 300 seconds
**C)** Enable sticky sessions
**D)** Increase the health check interval

---

## Question 10
A company needs to load balance HTTP traffic with the following requirements:
- Distribute traffic to Lambda functions for serverless processing
- Support WebSocket connections for real-time features
- Route traffic based on HTTP headers
- Minimize costs

Which load balancer type should they choose?

**A)** Network Load Balancer (NLB) - cheapest option with Lambda support
**B)** Application Load Balancer (ALB) - supports Lambda, WebSocket, and header-based routing
**C)** Gateway Load Balancer (GLB) - supports advanced routing
**D)** Classic Load Balancer (CLB) - cheapest option for simple routing

---
---

# STOP HERE - Don't scroll down if you haven't answered yet!

---
---
---
---
---
---
---
---
---
---

# Answers & Explanations

---

## Question 1: Answer - B
**Correct Answer: B) Target Tracking Scaling with ASGAverageCPUUtilization set to 50%**

**Explanation:**
- **Target Tracking** is the simplest scaling policy with **least operational overhead**
- You specify target metric (50% CPU), AWS handles all calculations and scaling decisions automatically
- No need to create CloudWatch alarms manually
- **Why not A?** Simple Scaling is legacy and has cooldown limitations
- **Why not C?** Step Scaling requires manual CloudWatch alarm configuration (more overhead)
- **Why not D?** Scheduled Scaling is for predictable time-based patterns, not variable traffic

**Exam Keyword:** "Minimal configuration and management overhead" → **Target Tracking**

---

## Question 2: Answer - C
**Correct Answer: C) Gateway Load Balancer with GENEVE protocol**

**Explanation:**
- **Gateway Load Balancer (GLB)** is specifically designed for **third-party virtual appliances**
- Use cases: firewalls, intrusion detection/prevention systems (IDS/IPS), deep packet inspection
- Operates at Layer 3 (network layer) using GENEVE protocol on port 6081
- Provides transparent network gateway for traffic inspection
- **Why not A?** ALB is Layer 7 for HTTP/HTTPS applications, not network appliances
- **Why not B?** NLB is Layer 4 for high-performance TCP/UDP load balancing, not appliances
- **Why not D?** WAF/Shield are AWS-managed security services, not for third-party appliances

**Exam Keywords:** "Third-party firewall appliances", "fleet of security appliances" → **Gateway Load Balancer**

---

## Question 3: Answer - B
**Correct Answer: B) Network Load Balancer (NLB)**

**Explanation:**
- **NLB** is the ONLY load balancer that meets ALL requirements:
  - **Layer 4**: Supports TCP/UDP protocols (UDP required for gaming)
  - **Ultra-low latency**: < 100 ms latency
  - **Static IP**: One static IP per AZ (can assign Elastic IPs)
  - **Extreme performance**: Millions of requests per second
- **Why not A?** ALB is Layer 7 (HTTP/HTTPS only), doesn't support UDP, no static IP
- **Why not C?** CLB is legacy (shouldn't use for new deployments)
- **Why not D?** GLB is for third-party appliances, not general load balancing

**Exam Keywords:** "UDP", "ultra-low latency", "static IP", "millions of requests" → **Network Load Balancer**

---

## Question 4: Answer - B
**Correct Answer: B) Application Load Balancer with path-based routing and Cognito integration**

**Explanation:**
- **ALB** provides ALL required features in a single service:
  - **Path-based routing**: Route different URL paths (`/api/users/*`, `/api/orders/*`, `/images/*`) to different target groups
  - **Cognito integration**: Authenticate users with social identity providers (Google, Facebook, Amazon, Apple)
  - **Layer 7**: HTTP/HTTPS traffic inspection
- Single ALB = **least operational overhead**
- **Why not A?** NLB doesn't support path-based routing or authentication
- **Why not C?** Three separate NLBs = more complexity, no path routing
- **Why not D?** API Gateway works but ALB is simpler for basic routing + auth

**Exam Keywords:** "Path-based routing", "social login", "least operational overhead" → **ALB with Cognito**

---

## Question 5: Answer - B
**Correct Answer: B) Change the health check type to ELB health checks targeting the application endpoint**

**Explanation:**
- **EC2 status checks** only verify the instance is running (hardware/network OK)
- **ELB health checks** test the actual application endpoint (e.g., `/health`, `/ping`)
- If application crashes but instance is running, EC2 checks pass but ELB checks fail
- ASG should use **ELB health checks** to detect application failures and replace unhealthy instances
- **Why not A?** Grace period delays health checks after launch, doesn't fix detection issue
- **Why not C?** CloudWatch monitoring doesn't improve health check accuracy
- **Why not D?** Faster checks don't help if checking wrong thing (EC2 vs application)

**Exam Keywords:** "Instance running but application crashed" → **Use ELB health checks**

---

## Question 6: Answer - C
**Correct Answer: C) Scheduled Scaling for predictable patterns + Target Tracking for unexpected spikes**

**Explanation:**
- **Scheduled Scaling**: Handle predictable load (business hours vs off-hours)
  - Scale to 10 instances at 8 AM Monday-Friday
  - Scale to 2 instances at 6 PM
- **Target Tracking**: Handle unexpected spikes during business hours
  - Automatically add capacity if CPU > target threshold
- **Cost-effective**: Right-size baseline capacity, only pay for spikes when needed
- **Why not A?** Target Tracking alone would keep high capacity during low-traffic periods (wasteful)
- **Why not B?** Step Scaling requires more management (CloudWatch alarms), no scheduled baseline
- **Why not D?** Simple Scaling is legacy with cooldown limitations

**Exam Keywords:** "Predictable patterns" + "occasional spikes" → **Scheduled + Target Tracking**

---

## Question 7: Answer - B
**Correct Answer: B) Cross-Zone Load Balancing is disabled**

**Explanation:**
- **Without Cross-Zone Load Balancing**:
  - Traffic distributed evenly to AZs (50% to AZ-A, 50% to AZ-B)
  - AZ-A: 2 instances get 50% traffic = 25% each
  - AZ-B: 8 instances get 50% traffic = 6.25% each
  - AZ-A instances get 4x more traffic per instance!
- **With Cross-Zone Load Balancing**:
  - Traffic distributed evenly across ALL instances (10% each)
- **NLB**: Cross-zone disabled by default (charges apply if enabled)
- **ALB**: Cross-zone enabled by default (no charges)
- **Why not A?** Security groups would block ALL traffic, not cause uneven distribution
- **Why not C?** Connection draining doesn't affect traffic distribution
- **Why not D?** Failed health checks would cause no traffic, not uneven traffic

**Exam Keywords:** "Uneven traffic across AZs", "NLB" → **Cross-Zone Load Balancing disabled**

---

## Question 8: Answer - D (but A also works as temporary fix)
**Correct Answer: D) Store session data in ElastiCache instead of local storage**

**Explanation:**
- **Best practice**: Store session data in **ElastiCache** (Redis/Memcached) or **DynamoDB**
  - Sessions accessible from any instance
  - Survives instance termination
  - Scalable and highly available
- **Why D is better than A**:
  - **Sticky sessions (A)** is a band-aid solution that works but has limitations:
    - If instance fails, all sessions on that instance are lost
    - Uneven load distribution (some instances get more sessions)
    - Doesn't work well with Auto Scaling (instances terminate)
  - **ElastiCache (D)** is the proper architectural solution
- **Exam perspective**: Both A and D would be accepted, but D is the "best practice" answer
- If question says "MOST scalable" or "best practice" → ElastiCache
- If question says "quick fix" or "least changes" → Sticky sessions

**Exam Keywords:** "Session data lost", "random logouts" → **ElastiCache** (best) or **Sticky Sessions** (quick fix)

---

## Question 9: Answer - B
**Correct Answer: B) Configure connection draining / deregistration delay to 300 seconds**

**Explanation:**
- **Connection draining / deregistration delay**: Time for in-flight requests to complete before instance termination
- Long-running requests (2-3 minutes = 120-180 seconds) need sufficient time to finish
- Set to **300 seconds (5 minutes)** to safely handle these requests
- When instance becomes unhealthy or ASG terminates it:
  - Load balancer stops sending NEW requests
  - Existing requests allowed to complete within deregistration delay
  - After delay, instance terminates
- **Why not A?** Cooldown period is for preventing scaling thrashing, not request completion
- **Why not C?** Sticky sessions route requests to same instance, doesn't help with termination
- **Why not D?** Health check interval is for detecting unhealthy instances, not request completion

**Exam Keywords:** "In-flight requests", "long-running requests", "complete before termination" → **Connection draining / deregistration delay**

---

## Question 10: Answer - B
**Correct Answer: B) Application Load Balancer (ALB) - supports Lambda, WebSocket, and header-based routing**

**Explanation:**
- **ALB** is the ONLY load balancer that supports ALL requirements:
  - **Lambda targets**: ALB can directly invoke Lambda functions (unique feature)
  - **WebSocket support**: Full support for WebSocket connections
  - **Header-based routing**: Route based on HTTP headers, path, host, query strings
  - **Cost**: More expensive than NLB but cheaper than managing API Gateway
- **Why not A?** NLB doesn't support Lambda as target (can only target EC2, IP, ALB)
- **Why not C?** GLB is for third-party appliances, doesn't support Lambda or WebSocket
- **Why not D?** CLB is legacy, doesn't support Lambda targets or advanced routing

**Exam Keywords:** "Lambda targets", "WebSocket", "header-based routing" → **Application Load Balancer**

---

## Quiz Results

**Scoring:**
- **9-10 correct**: You crushed it! Day 2 material locked in. ✅
- **7-8 correct**: Solid understanding, review the questions you missed
- **5-6 correct**: You need another pass through the study guide
- **Below 5**: Re-read the Day 2 Catch-Up guide carefully

**Common Mistakes to Watch For:**
1. Confusing **Target Tracking** (simplest) with **Step Scaling** (more control)
2. Forgetting **NLB is the only LB with static IP support**
3. Not knowing **ALB is the only LB that can target Lambda functions**
4. Missing that **GLB is specifically for third-party appliances**
5. Confusing **EC2 health checks** (instance running) with **ELB health checks** (application working)
6. Forgetting **NLB cross-zone is disabled by default and costs extra**

---

**Next Steps:**
1. Review any questions you got wrong
2. Go back to the Day 2 Catch-Up guide for topics you're unclear on
3. When ready, tackle Day 4: S3 Security & Replication

Good luck! 🚀
