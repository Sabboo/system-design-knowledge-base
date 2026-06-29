# Consistent Hashing - Playbook

## Should I Use Consistent Hashing?

| Situation | Recommendation |
|-----------|----------------|
| Static cluster | No |
| Servers frequently added/removed | Yes |
| Distributed cache | Yes |
| Single database | No |
| Database sharding | Yes |
| Load balancing | Sometimes |

## Decision Guide

Need to distribute data?

├── Single server
│ └── Don't use consistent hashing
└── Multiple servers

├── Cluster size changes?

│ ├── No → Simple hashing may be enough

│ └── Yes → Consistent Hashing

## When Virtual Nodes Are Needed

| Servers | Virtual Nodes |
|----------|---------------|
| <10 | Strongly recommended |
| 10–100 | Recommended |
| >100 | Usually beneficial |

## Common Interview Questions

### Why not `hash(key) % N`?

Changing **N** reassigns nearly every key.

---

### Why virtual nodes?

Improve key distribution and balance load.

---

### Why clockwise lookup?

Guarantees every key maps to exactly one server.

---

### What happens when a server dies?

Its keys move to the next server clockwise.

---

### How much data moves?

Only the keys owned by that server.

## Production Examples

| System | Uses Consistent Hashing |
|---------|-------------------------|
| Dynamo | Yes |
| Cassandra | Yes |
| Riak | Yes |
| Redis Cluster | Similar concept (hash slots) |
| Memcached Client | Yes |

## Rule of Thumb

Use Consistent Hashing when:

- Nodes change over time.
- Data redistribution should be minimized.
- Horizontal scaling is required.

Don't use it when:

- Cluster size is fixed.
- Simplicity is more important.
- Data lives on a single server.
