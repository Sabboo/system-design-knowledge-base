# Consistent Hashing

> Distribute data across nodes while minimizing data movement when nodes are added or removed.

## Problem

Modulo hashing (`hash(key) % N`) requires rehashing nearly all keys whenever the number of servers changes.

## Core Idea

Hash both **servers** and **keys** onto the same hash ring.

Each key is assigned to the **first server encountered clockwise** on the ring.

## Why It Works

When a server is:

- **Added:** Only keys between the new server and its predecessor move.
- **Removed:** Only keys assigned to that server are redistributed.

This minimizes data movement compared to modulo hashing.

## Virtual Nodes

A physical server is represented by multiple positions on the ring.

**Benefits**

- More even key distribution
- Better load balancing
- Easier scaling

## Comparison

| Modulo Hashing | Consistent Hashing |
|----------------|--------------------|
| Massive key movement | Minimal key movement |
| Poor scalability | Excellent scalability |
| Uneven rebalancing | Localized rebalancing |

## Complexity

| Operation | Complexity |
|-----------|------------|
| Lookup | O(log N)* |
| Add Server | O(V log N) |
| Remove Server | O(V log N) |

\* Assuming servers are stored in a sorted structure (e.g., balanced tree). With arrays and binary search, lookup is also O(log N).

## Use Cases

- Distributed caches
- Database sharding
- Distributed key-value stores
- Load balancing

## Interview Recognition

Use consistent hashing when the interviewer mentions:

- Adding or removing servers
- Cache clusters
- Data sharding
- Rebalancing
- Horizontal scaling

## Key Takeaways

- Solves the rehashing problem caused by modulo hashing.
- Keys and servers share the same hash space.
- Keys map to the next server clockwise.
- Virtual nodes improve load balancing.
- Only a small subset of keys move when topology changes.
