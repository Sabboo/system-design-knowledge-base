# Fundamentals

> Core principles that appear in almost every system design interview.

## Goal

Design systems that remain scalable, reliable, available, and maintainable as users and data grow.

## Core Metrics

| Metric | Definition |
|---------|------------|
| Latency | Time to serve one request |
| Throughput | Requests processed per unit time |
| Availability | Percentage of uptime |
| Reliability | System behaves correctly |
| Scalability | Ability to handle increasing load |

## Scaling

| Vertical (Scale Up) | Horizontal (Scale Out) |
|---------------------|------------------------|
| Upgrade one machine | Add more machines |
| Simple architecture | Distributed architecture |
| Limited by hardware | Scales almost indefinitely |
| Single point of failure | Better fault tolerance |

## Common Building Blocks

| Purpose | Component |
|---------|-----------|
| Route traffic | Load Balancer |
| Execute business logic | Application Servers |
| Reduce latency | Cache / CDN |
| Store persistent data | Database |
| Process asynchronously | Message Queue |
| Store large files | Object Storage |

## Capacity Planning

Estimate before designing:

- Number of users
- Concurrent users
- Requests per second (RPS)
- Storage growth
- Bandwidth
- Expected future growth

## Interview Recognition

| If the interviewer mentions... | Consider... |
|--------------------------------|-------------|
| Millions of users | Horizontal Scaling |
| Slow responses | Cache |
| Database bottleneck | Replication / Sharding |
| Traffic spikes | Rate Limiting |
| High availability | Replication & Redundancy |

## Never Forget

- Estimate scale before selecting technologies.
- Horizontal scaling is the default approach for large systems.
- Latency and throughput measure different aspects of performance.
- Availability and reliability are different goals.
- Every architectural decision is a tradeoff.
