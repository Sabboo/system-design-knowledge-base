# Rate Limiting - Decision Guide

> Choose the most appropriate rate limiting algorithm based on your system requirements.

## Quick Decision Tree

```text
Need exact request counting?
│
├── Yes
│   └── Sliding Window Log
│
└── No
    │
    ├── Need to allow bursts?
    │   ├── Yes → Token Bucket
    │   └── No
    │
    ├── Need constant output rate?
    │   ├── Yes → Leaky Bucket
    │   └── No
    │
    └── Need lowest memory usage?
        ├── Yes → Fixed Window Counter
        └── No → Sliding Window Counter
```

## Comparison Matrix

| Algorithm | Accuracy | Memory | Burst Support | Performance | Production |
|-----------|----------|--------|---------------|-------------|------------|
| Fixed Window | Low | Excellent | Yes | Excellent | Rare |
| Sliding Window Log | Exact | Poor | Yes | Fair | Sometimes |
| Sliding Window Counter | High | Excellent | Partial | Excellent | Common |
| Token Bucket | High | Excellent | Excellent | Excellent | Very Common |
| Leaky Bucket | High | Good | No | Excellent | Common |

## Choose By Requirement

| If you need... | Choose... | Why |
|----------------|-----------|-----|
| Lowest memory usage | Fixed Window Counter | Single counter |
| Exact request counting | Sliding Window Log | Stores every request |
| Best overall balance | Sliding Window Counter | Accurate + efficient |
| Burst handling | Token Bucket | Tokens accumulate over time |
| Smooth traffic | Leaky Bucket | Constant processing rate |

## Typical Use Cases

| System | Recommended Algorithm | Reason |
|---------|-----------------------|--------|
| Public REST API | Token Bucket | Allows short bursts |
| API Gateway | Sliding Window Counter | Accurate and scalable |
| Login Endpoint | Sliding Window Counter | Prevent brute-force attacks |
| Payment API | Sliding Window Log | Maximum accuracy |
| Network Router | Leaky Bucket | Smooth outgoing traffic |
| Internal Admin API | Fixed Window Counter | Simplicity |

## Tradeoffs

| Algorithm | Biggest Advantage | Biggest Limitation |
|-----------|-------------------|--------------------|
| Fixed Window | Simple | Boundary problem |
| Sliding Window Log | Exact | High memory usage |
| Sliding Window Counter | Excellent balance | Approximation |
| Token Bucket | Supports bursts | More complex |
| Leaky Bucket | Smooth output | Rejects bursts |

## Interview Answers

### Why not Fixed Window?

- Boundary problem allows traffic spikes.

---

### Why not Sliding Window Log?

- Memory grows with request volume.

---

### Why is Sliding Window Counter common?

- Near-exact accuracy with O(1) memory.

---

### Why Token Bucket for APIs?

- Allows legitimate burst traffic while enforcing an average rate.

---

### Why Leaky Bucket?

- Produces a predictable output rate and protects downstream systems.

## Rule of Thumb

| Situation | Recommendation |
|-----------|----------------|
| Don't know what to choose | Sliding Window Counter |
| Need burst support | Token Bucket |
| Need perfect accuracy | Sliding Window Log |
| Need smooth traffic | Leaky Bucket |
| Need the simplest implementation | Fixed Window Counter |

## Key Takeaways

- There is no universally best algorithm.
- Start with system requirements, then choose the algorithm.
- Most production APIs use **Token Bucket** or **Sliding Window Counter**.
- Every algorithm is a tradeoff between accuracy, memory, and performance.
