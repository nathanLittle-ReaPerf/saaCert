# Load Balancer Cheat Sheet: ALB vs NLB vs GLB

**The "Stop Mixing These Up" Guide**

---

## 🎯 The 30-Second Decision Tree

```
START: What kind of traffic are you load balancing?

├─ Third-party security appliances (firewalls, IDS/IPS)?
│  └─ ✅ GATEWAY LOAD BALANCER (GLB)
│
├─ HTTP/HTTPS web traffic?
│  ├─ Need path routing (/api/* vs /images/*)?
│  │  └─ ✅ APPLICATION LOAD BALANCER (ALB)
│  ├─ Need Lambda targets?
│  │  └─ ✅ APPLICATION LOAD BALANCER (ALB)
│  ├─ Need authentication (Cognito, social login)?
│  │  └─ ✅ APPLICATION LOAD BALANCER (ALB)
│  └─ Need WebSocket?
│     └─ ✅ APPLICATION LOAD BALANCER (ALB)
│
└─ TCP/UDP or non-HTTP traffic?
   ├─ Need static IP addresses?
   │  └─ ✅ NETWORK LOAD BALANCER (NLB)
   ├─ Need extreme performance (millions req/sec)?
   │  └─ ✅ NETWORK LOAD BALANCER (NLB)
   ├─ Need ultra-low latency (< 100ms)?
   │  └─ ✅ NETWORK LOAD BALANCER (NLB)
   └─ Gaming, IoT, or UDP traffic?
      └─ ✅ NETWORK LOAD BALANCER (NLB)
```

---

## 📋 The One-Page Comparison Table

| Feature | ALB | NLB | GLB |
|---------|-----|-----|-----|
| **OSI Layer** | Layer 7 (Application) | Layer 4 (Transport) | Layer 3 (Network) |
| **Protocol** | HTTP, HTTPS, gRPC | TCP, UDP, TLS | IP packets (GENEVE) |
| **Best For** | Web apps, microservices | Extreme performance, TCP/UDP | Third-party appliances |
| **Static IP** | ❌ No | ✅ Yes (Elastic IP support) | ✅ Yes |
| **Performance** | ~Thousands req/sec | ~Millions req/sec | High |
| **Latency** | Higher (~ms) | Ultra-low (< 100ms) | Low |
| **Path Routing** | ✅ Yes (`/api/*` → Target A) | ❌ No | ❌ No |
| **Host Routing** | ✅ Yes (`api.example.com` → Target A) | ❌ No | ❌ No |
| **Header Routing** | ✅ Yes (HTTP headers) | ❌ No | ❌ No |
| **Lambda Targets** | ✅ Yes (ONLY ALB!) | ❌ No | ❌ No |
| **WebSocket** | ✅ Yes | ✅ Yes (as TCP) | ❌ No |
| **Preserve Source IP** | Via `X-Forwarded-For` header | ✅ Native | ✅ Yes |
| **Authentication** | ✅ Cognito, OIDC | ❌ No | ❌ No |
| **TLS Termination** | ✅ Yes | ✅ Yes | ❌ No |
| **Cross-Zone LB Default** | ✅ Enabled (FREE) | ❌ Disabled (COSTS $$ if enabled) | ❌ Disabled |
| **Target Types** | EC2, IP, Lambda, ALB | EC2, IP, ALB | EC2, IP |
| **Use Cases** | Web apps, APIs, containers | Gaming, IoT, FinTech, static IP | Firewall fleets, IDS/IPS |

---

## 🔑 Exam Keyword → Load Balancer Mapping

**When you see these keywords in a question, the answer is obvious:**

### ALB Keywords (Application Load Balancer)
- ✅ "HTTP" or "HTTPS" traffic
- ✅ "Path-based routing" (`/api/*`, `/images/*`)
- ✅ "Host-based routing" (subdomains)
- ✅ "Microservices" or "containers"
- ✅ "Lambda" as target
- ✅ "Serverless" architecture
- ✅ "WebSocket" connections
- ✅ "Authenticate users" (Cognito)
- ✅ "Social login" (Google, Facebook)
- ✅ "Route based on HTTP headers"
- ✅ "Content-based routing"

### NLB Keywords (Network Load Balancer)
- ✅ "TCP" or "UDP" protocol
- ✅ "Static IP address" or "Elastic IP"
- ✅ "Millions of requests per second"
- ✅ "Ultra-low latency" or "< 100ms latency"
- ✅ "Extreme performance"
- ✅ "Preserve source IP"
- ✅ "Gaming" or "multiplayer"
- ✅ "IoT" devices
- ✅ "Non-HTTP protocol"
- ✅ "Client needs to whitelist IP addresses"
- ✅ "FinTech" or "high-frequency trading"

### GLB Keywords (Gateway Load Balancer)
- ✅ "Third-party appliances"
- ✅ "Firewall fleet" or "deploy firewalls"
- ✅ "Intrusion detection" (IDS) or "intrusion prevention" (IPS)
- ✅ "Deep packet inspection"
- ✅ "Security appliances"
- ✅ "Virtual appliances"
- ✅ "Transparent network gateway"
- ✅ "Palo Alto", "Fortinet", "Check Point" (firewall vendors)

---

## 🧠 Memory Tricks

### ALB = "Application" = Smart
- **Layer 7** = Can see application data (URLs, headers, cookies)
- **Smart routing** = Path, host, header-based decisions
- **Smart authentication** = Cognito integration
- **Smart targets** = Can invoke Lambda (serverless)
- **Think**: "If it needs to be smart about HTTP, use ALB"

### NLB = "Network" = Fast & Strong
- **Layer 4** = Blind to application data (just forwards packets)
- **Static IP** = Network-level addressing
- **Ultra-performance** = Millions of requests, < 100ms latency
- **TCP/UDP** = Network protocols
- **Think**: "If it needs to be fast, handle TCP/UDP, or have static IP, use NLB"

### GLB = "Gateway" = Security Gatekeeper
- **Layer 3** = Lowest level (IP packets)
- **Gateway** = All traffic passes through (inspection point)
- **Security appliances** = Firewalls, IDS/IPS
- **Think**: "If third-party security is involved, use GLB"

---

## ❌ Common Exam Traps (Don't Fall For These!)

### Trap 1: "Static IP" + "HTTP routing"
**Question**: Need static IP AND path-based routing

**Wrong Answer**: Can't be done, choose one
**Right Answer**: **NLB → ALB** (put ALB behind NLB!)
- NLB provides static IP
- NLB forwards to ALB as target
- ALB provides path-based routing
- Yes, you can chain them!

### Trap 2: "Lowest cost load balancer"
**Wrong Answer**: NLB (it's simpler, must be cheaper)
**Right Answer**: **ALB** (generally cheaper for HTTP workloads)
- ALB pricing is usually lower for web applications
- NLB charges for cross-zone load balancing
- Unless question specifies TCP/UDP, ALB is typically more cost-effective for HTTP

### Trap 3: "WebSocket support"
**Trap**: You might think NLB since it's "faster"
**Right Answer**: **ALB** (native WebSocket support)
- ALB has built-in WebSocket support
- NLB can handle WebSocket as TCP traffic, but ALB is purpose-built for it
- If question says "WebSocket" and HTTP traffic → ALB

### Trap 4: "Preserve client source IP"
**Trick**: All three can do it, but differently
- **ALB**: Uses `X-Forwarded-For` HTTP header (application must read it)
- **NLB**: Native preservation (target sees real client IP directly)
- **GLB**: Native preservation
- If question emphasizes "native" or "direct" → NLB or GLB

### Trap 5: "Cross-Zone Load Balancing costs"
**Critical for cost questions**:
- **ALB**: Enabled by default, **FREE** ✅
- **NLB**: Disabled by default, **COSTS MONEY if enabled** 💰
- **GLB**: Disabled by default, **COSTS MONEY if enabled** 💰
- If question asks about "additional charges for cross-zone" → talking about NLB or GLB

### Trap 6: "Lambda as a target"
**ONLY ALB supports Lambda targets**
- NLB cannot target Lambda (only EC2, IP, ALB)
- GLB cannot target Lambda (only EC2, IP)
- If you see Lambda + load balancer → 100% ALB

---

## 🎓 The "If You Remember Nothing Else" Summary

### Application Load Balancer (ALB) - Layer 7
**One sentence**: HTTP/HTTPS smart routing with path/host/header rules, Lambda targets, and Cognito auth

**Exam pattern**: If it's web traffic with complex routing or Lambda → ALB

---

### Network Load Balancer (NLB) - Layer 4
**One sentence**: TCP/UDP extreme performance with static IPs, ultra-low latency, and millions of req/sec

**Exam pattern**: If it needs static IP, UDP, or extreme performance → NLB

---

### Gateway Load Balancer (GLB) - Layer 3
**One sentence**: Third-party security appliance fleet (firewalls, IDS/IPS) transparent gateway

**Exam pattern**: If it mentions third-party security appliances → GLB

---

## 📝 Quick Quiz (Answer in 5 Seconds Each)

1. Need to route `/api/*` to one service, `/web/*` to another → **?**
2. Gaming servers using UDP protocol → **?**
3. Deploy a fleet of Palo Alto firewalls → **?**
4. Load balance traffic to Lambda functions → **?**
5. Client needs to whitelist load balancer IP addresses → **?**
6. Authenticate users with Google/Facebook login → **?**
7. Millions of requests per second, ultra-low latency → **?**
8. HTTP traffic, WebSocket support, least operational overhead → **?**
9. Deep packet inspection with third-party tools → **?**
10. TCP traffic with static IP requirement → **?**

### Answers:
1. ALB (path routing)
2. NLB (UDP support)
3. GLB (third-party appliances)
4. ALB (only LB that targets Lambda)
5. NLB (static IP/Elastic IP)
6. ALB (Cognito integration)
7. NLB (performance beast)
8. ALB (HTTP + WebSocket)
9. GLB (third-party security)
10. NLB (TCP + static IP)

**If you got less than 9/10, read this sheet again. Then quiz yourself tomorrow.**

---

## 🎯 Final Exam Strategy

When you see a load balancer question:

**Step 1**: What protocol?
- HTTP/HTTPS → Probably ALB
- TCP/UDP → Probably NLB
- IP packets / third-party appliances → GLB

**Step 2**: What special features?
- Lambda targets → ALB (no other choice)
- Static IP → NLB (ALB can't do this)
- Third-party security → GLB (that's its only job)
- Path/host routing → ALB (NLB/GLB can't do this)
- Authentication → ALB (NLB/GLB can't do this)

**Step 3**: Performance requirements?
- "Ultra-low latency" or "millions of req/sec" → NLB
- "Least operational overhead" for HTTP → ALB

**Step 4**: Cross-check
- Does your answer support ALL requirements in the question?
- If using NLB, did you check if cross-zone load balancing costs apply?
- If using ALB, did you verify it's HTTP/HTTPS traffic?

---

## 💪 Your Mission

**Before Day 4:**
1. ✅ Read this cheat sheet 3 times
2. ✅ Close your eyes and recite: "ALB = HTTP/Lambda/routing, NLB = TCP/UDP/static IP, GLB = third-party security"
3. ✅ Retake the Day 2 quiz and get 9/10 or better
4. ✅ Do 10 more load balancer practice questions from AWS Skill Builder

**You need to be able to choose the right load balancer in 10 seconds or less on exam day.**

This is non-negotiable. Load balancers are 5-10 questions on the exam. Don't throw away easy points.

---

**Now go memorize this. I'll quiz you on it later. 😏**
