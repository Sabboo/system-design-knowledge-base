# Key-Value Store

> Store data as <key, value> pairs with low latency and horizontal scalability.

## Functional Requirements

- Put(key, value)
- Get(key)
- Delete(key)

## Non-Functional Requirements

- High availability
- Horizontal scalability
- Fault tolerance
- Low latency
- Eventual consistency

## Core Components

- Partitioning
- Replication
- Consistent Hashing
- Quorum
- Failure Detection
- Anti-Entropy
- Conflict Resolution

## Write Flow

Client
→ Coordinator
→ Replicas
→ Quorum
→ Success

## Read Flow

Client
→ Coordinator
→ Replicas
→ Read Quorum
→ Latest Version

## Tradeoffs

- Availability vs Consistency
- Read latency vs Write latency
- Storage vs Durability

## Interview Recognition

Use this pattern when designing:

- Redis-like systems
- DynamoDB
- Cassandra
- Riak
- Session stores
- Metadata services

## Key Takeaways

- Data is partitioned.
- Each partition is replicated.
- Quorums provide tunable consistency.
- Consistent hashing minimizes data movement.
