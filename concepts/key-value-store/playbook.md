# Key-Value Store - Playbook

## Common Design Decisions

| Requirement | Recommendation | Why |
|-------------|---------------|-----|
| Horizontal scaling | Consistent Hashing | Minimizes data movement when nodes change |
| High availability | Replication | Data survives node failures |
| Tunable consistency | Quorum | Balance consistency and latency |
| Conflict resolution | Vector Clocks | Detect concurrent writes |
| Replica repair | Merkle Trees | Synchronize only changed data |
| Failure detection | Gossip Protocol | Decentralized and scalable |

---

## Interview Questions

### Why Consistent Hashing?

Avoids rehashing nearly all keys when servers are added or removed. Only keys belonging to the affected hash range move.

---

### Why Replication Factor = 3?

Provides fault tolerance while keeping storage overhead reasonable. It allows the system to tolerate one node failure when using majority quorums.

---

### Why Quorum?

Quorums provide tunable consistency.

If:

```
R + W > N
```

then reads and writes always overlap on at least one replica, reducing stale reads.

---

### Why Eventual Consistency?

It prioritizes availability and low latency over immediate consistency. Replicas converge over time through synchronization.

---

### Why Vector Clocks instead of Last Write Wins?

Vector clocks detect conflicting concurrent writes.

Last Write Wins is simpler but may silently discard valid updates.

---

### Why Merkle Trees?

Instead of comparing all data between replicas, compare hash trees to identify only the ranges that differ, reducing network traffic.

---

## Common Tradeoffs

| Option A | Option B | Choose When |
|----------|----------|-------------|
| Strong Consistency | Eventual Consistency | Financial systems vs Social apps |
| Leader-Based | Leaderless | Simplicity vs Availability |
| Last Write Wins | Vector Clocks | Simplicity vs Correctness |

---

## Rule of Thumb

| Requirement | Recommendation |
|-------------|----------------|
| General-purpose distributed KV store | Consistent Hashing + Replication + Quorum |
| Read-heavy workload | Lower write quorum, higher read quorum |
| Write-heavy workload | Lower read quorum, higher write quorum |
| Frequent node changes | Consistent Hashing with Virtual Nodes |

---

## What Interviewers Want to Hear

### "How would you partition the data?"

"Use consistent hashing with virtual nodes to evenly distribute keys and minimize data movement during scaling."

---

### "How do you prevent data loss?"

"Replicate each partition across multiple nodes using a replication factor of 3."

---

### "How do reads stay consistent?"

"Use quorum reads and writes where R + W > N to ensure read/write overlap."

---

### "How are conflicting writes resolved?"

"Use vector clocks to detect conflicts, then merge or resolve them according to application logic."
