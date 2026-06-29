# Rate Limiting - Deep Dive

## Overview

Rate limiting controls how many requests a client can make within a given period.

**Goals**

- Protect backend services
- Prevent abuse
- Ensure fair usage
- Improve system stability

## Design Requirements

A good rate limiter should be:

- Accurate
- Low latency
- Memory efficient
- Horizontally scalable
- Fault tolerant

## Algorithms

### Fixed Window Counter

**Idea:** Count requests within fixed time windows.

**Example**

```text
Limit: 100 req/min

12:00 ───────────── 12:01
      100 requests
Reset → Counter = 0
```

| Pros | Cons |
|------|------|
| Simple | Boundary problem |
| O(1) memory | Traffic spikes |
| Fast | Low accuracy near boundaries |

| Time | Memory |
|------|--------|
| O(1) | O(1) |

**Use When**

- Simple systems
- Memory is critical
- Occasional bursts are acceptable

---

### Sliding Window Log

**Idea:** Store every request timestamp and remove expired entries before counting.

**Example**

```text
Window = 60s

Current Time = 100

Stored:
41 52 70 81 95

Expired (<40): none

Count = 5
```

| Pros | Cons |
|------|------|
| Exact | High memory usage |
| Fair | Cleanup overhead |
| Easy to understand | Doesn't scale well |

| Time | Memory |
|------|--------|
| O(n) | O(n) |

**Use When**

- Accuracy is critical
- Moderate traffic
- Memory isn't a concern

---

### Sliding Window Counter

**Idea:** Estimate the rolling window using the previous and current counters.

**Formula**

```
Estimated Requests =
Current Counter +
(Previous Counter × Remaining Percentage)
```

**Example**

```text
Window = 60s

Previous Window = 80
Current Window  = 20

15 seconds elapsed

Estimated

20 + (80 × 0.75)

= 80
```

| Pros | Cons |
|------|------|
| Near-exact | Approximation |
| O(1) memory | Slightly more complex |
| Production friendly | Not perfectly accurate |

| Time | Memory |
|------|--------|
| O(1) | O(1) |

**Use When**

- High-throughput APIs
- Distributed systems
- General production workloads

---

### Token Bucket

**Idea:** Requests consume tokens. Tokens are replenished at a fixed rate.

**Example**

```text
Capacity : 10 tokens
Refill   : 5 tokens/sec

Request

Token Available? → Allow

No Token? → Reject
```

| Pros | Cons |
|------|------|
| Supports bursts | Slightly harder to implement |
| Constant memory | Requires refill calculation |
| Very common in production | |

| Time | Memory |
|------|--------|
| O(1) | O(1) |

**Use When**

- Public APIs
- Mobile applications
- Traffic with occasional bursts

---

### Leaky Bucket

**Idea:** Requests enter a queue and leave at a constant processing rate.

**Example**

```text
Incoming Requests → Queue → Fixed Processing Rate
```

| Pros | Cons |
|------|------|
| Smooth traffic | Doesn't support bursts |
| Predictable output | Queue may overflow |
| Protects downstream systems | Higher latency during bursts |

| Time | Memory |
|------|--------|
| O(1) | O(queue size) |

**Use When**

- Network traffic shaping
- Constant processing rate is required
- Downstream services can't handle bursts

## Distributed Rate Limiting

### Challenge

Multiple servers must enforce the same limit.

```text
Server A → 80 requests
Server B → 80 requests

Actual = 160 requests
```

Without coordination, each server applies the limit independently.

### Common Solutions

| Solution | Purpose |
|----------|---------|
| Redis | Shared counters |
| Lua Scripts | Atomic updates |
| Distributed Cache | Shared state |
| Dedicated Rate Limiter | Centralized enforcement |

## Counter Storage

| Storage | Advantages | Disadvantages |
|----------|------------|---------------|
| In-Memory | Fastest | Doesn't work across multiple servers |
| Redis | Distributed, low latency | Additional infrastructure |
| Database | Durable | High latency |

## Failure Strategies

| Strategy | Behavior | Typical Use |
|----------|----------|-------------|
| Fail Open | Allow requests if limiter fails | Prioritize availability |
| Fail Closed | Reject requests if limiter fails | Prioritize protection |

## Evolution

| Algorithm | Solves | Main Limitation |
|-----------|--------|-----------------|
| Fixed Window | Basic rate limiting | Boundary problem |
| Sliding Window Log | Boundary problem | High memory usage |
| Sliding Window Counter | Memory usage | Approximation |
| Token Bucket | Burst handling | More complex implementation |
| Leaky Bucket | Smooth output rate | Doesn't allow bursts |

## Common Interview Questions

- Why not use Fixed Window Counter?
- Why is Redis commonly used?
- How do you avoid race conditions?
- Which algorithm supports bursts?
- Which algorithm is most memory efficient?
- Which algorithm is best for production APIs?
- How would you implement distributed rate limiting?
