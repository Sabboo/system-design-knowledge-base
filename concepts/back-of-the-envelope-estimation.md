# Back-of-the-Envelope Estimation

> A quick approximation technique used to estimate system scale before making architectural decisions.

## Purpose

Estimate whether a proposed design can handle the expected scale.

## Why It Matters

Accurate estimates help determine:

- Server count
- Storage requirements
- Bandwidth
- Memory usage
- Database capacity
- Cache size

## Typical Estimations

- Daily Active Users (DAU)
- Concurrent Users
- Requests Per Second (RPS)
- Peak RPS
- Storage
- Bandwidth
- Memory
- Cache Size

## Common Formulas

### Requests Per Second

```
RPS = Daily Requests / 86,400
```

### Peak RPS

```
Peak RPS ≈ Average RPS × Peak Factor
```

(Peak factor is commonly 2–5× depending on the system.)

### Storage

```
Total Storage = Number of Objects × Object Size
```

### Bandwidth

```
Bandwidth = RPS × Response Size
```

## Example

Given:

- 10M Daily Active Users
- 20 requests/user/day

```
Daily Requests

10M × 20
= 200M requests/day

Average RPS

200M / 86,400
≈ 2,315 RPS
```

## Interview Recognition

Estimate when the interviewer asks:

- "How many users?"
- "How much storage?"
- "How many servers?"
- "How much traffic?"
- "Can one database handle this?"

## Common Mistakes

- Skipping estimation completely.
- Using unrealistic assumptions.
- Forgetting peak traffic.
- Confusing average load with peak load.
- Over-optimizing before estimating scale.

## Key Takeaways

- Estimate before designing.
- Use approximations; exact numbers are unnecessary.
- State your assumptions clearly.
- Round numbers to simplify calculations.
- The goal is reasoning, not mathematical precision.
