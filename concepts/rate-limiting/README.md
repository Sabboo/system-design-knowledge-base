# Rate Limiting

> Control how many requests a client can make within a given period.

## Problem

Protect systems from abuse, ensure fair usage, and prevent overload.

## Common Use Cases

- Public APIs
- Login endpoints
- Payment APIs
- File uploads
- Authentication services

## Algorithms

| Algorithm | Memory | Accuracy | Burst Support | Complexity |
|-----------|--------|----------|---------------|------------|
| Token Bucket | Low | High | Yes | Medium |
| Leaky Bucket | Low | High | No | Medium |
| Fixed Window Counter | Very Low | Low | Yes | Low |
| Sliding Window Log | High | Very High | Yes | High |
| Sliding Window Counter | Low | High | Partial | Medium |

## Design Decisions

| Decision | Options | Considerations |
|----------|---------|----------------|
| Scope | Global, Per User, Per IP, Per API Key, Per Endpoint | Defines fairness and isolation |
| Storage | In-Memory, Redis, Database | Scalability, persistence, latency |
| Deployment | Local, Centralized, Distributed | Consistency vs simplicity |
| Fail Strategy | Fail Open, Fail Closed | Availability vs protection |
| Time Window | Fixed, Sliding | Simplicity vs accuracy |
| Algorithm | Token Bucket, Sliding Window, etc. | Memory, accuracy, burst handling |
| Reset Policy | Automatic, Manual | Quotas and operational requirements |

## Interview Recognition

Use rate limiting when you need to:

- Protect backend services
- Prevent abuse
- Enforce quotas
- Handle traffic spikes
- Ensure fairness

## Key Takeaways

- Rate limiting is a tradeoff between accuracy, memory, and performance.
- No single algorithm is best for every use case.
- Sliding Window Counter is a common production choice.
- Redis is commonly used for distributed rate limiting.
- Always choose the algorithm based on system requirements.
