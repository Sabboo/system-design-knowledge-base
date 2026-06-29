# Key-Value Store

## Problem

A single database eventually hits limits:

- Storage
- CPU
- Memory
- Availability

We need to distribute data across multiple machines.

---

## Step 1 - Partitioning

Split keys across servers.

Simple hashing works...

until servers are added or removed.

↓

Use Consistent Hashing.

---

## Step 2 - Replication

One copy is risky.

Store multiple replicas.

```
Replication Factor = 3

A → B → C
```

Now a server can fail without data loss.

---

## Step 3 - Writes

Client

↓

Coordinator

↓

Replica 1

Replica 2

Replica 3

↓

Wait for W acknowledgements.

---

## Step 4 - Reads

Coordinator requests data from replicas.

Wait for R responses.

Return newest version.

---

## Step 5 - Quorum

Replication Factor = N

Need:

```
R + W > N
```

Guarantees overlap between reads and writes.

---

## Step 6 - Failure Detection

Servers exchange heartbeats.

Missing heartbeat?

↓

Mark node unhealthy.

---

## Step 7 - Temporary Failures

A replica goes offline.

Store missed writes elsewhere.

↓

Hinted Handoff.

When the node returns...

Replay missed writes.

---

## Step 8 - Permanent Divergence

Replicas eventually become inconsistent.

Repair them using:

- Merkle Trees
- Anti-Entropy

---

## Step 9 - Concurrent Writes

Two users update the same key simultaneously.

Need conflict resolution.

Common approaches:

- Last Write Wins
- Vector Clocks

---

## Result

A distributed key-value store that is:

- Horizontally scalable
- Highly available
- Fault tolerant
