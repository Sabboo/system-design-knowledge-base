# Unique ID Generator

> Generate globally unique, scalable, and sortable IDs in a distributed system.

## Requirements

- Globally unique
- High throughput
- Low latency
- Highly available
- Horizontally scalable
- (Optional) Time sortable

## Candidate Solutions

| Solution | Distributed | Sortable | Scalable | Notes |
|----------|-------------|----------|----------|------|
| Auto Increment | ❌ | ✅ | ❌ | Single DB bottleneck |
| UUID | ✅ | ❌ | ✅ | Large, random |
| Multi-Master Auto Increment | ⚠️ | ✅ | Limited | Coordination required |
| Snowflake | ✅ | ✅ | ✅ | Common production choice |

## Snowflake ID Layout

| Timestamp | Datacenter | Machine | Sequence |
|-----------|------------|---------|----------|

## Design Decisions

- ID format (numeric vs string)
- Ordering required?
- Coordination acceptable?
- Expected throughput?
- Clock synchronization strategy?

## Interview Recognition

Use when designing:

- Orders
- Payments
- Messages
- URLs
- Posts
- Events
- Database primary keys

## Key Takeaways

- UUID maximizes simplicity.
- Auto Increment doesn't scale.
- Snowflake balances uniqueness, ordering, and scalability.
