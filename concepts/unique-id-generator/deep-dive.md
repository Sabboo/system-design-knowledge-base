# Unique ID Generator

## Problem

Need IDs that are:

- Unique across all machines
- Fast to generate
- Scalable
- Available without a single bottleneck

---

## Naive Solution: Auto Increment

```
1
2
3
4
```

Works for a single database.

Fails in distributed systems because multiple nodes can generate the same ID.

---

## UUID

Generate a random 128-bit identifier.

Example:

```
550e8400-e29b-41d4-a716-446655440000
```

Pros:

- No coordination
- Extremely low collision probability

Cons:

- Large (128 bits)
- Not sortable
- Poor database index locality

---

## Multi-Master Auto Increment

Assign each server a different increment.

```
Server A

1 3 5 7 ...

Server B

2 4 6 8 ...
```

Pros:

- Ordered

Cons:

- Difficult to scale
- Adding/removing servers is complex

---

## Snowflake

Split the ID into multiple fields.

```
+-----------+-----------+---------+----------+
| Timestamp | Datacenter| Machine | Sequence |
+-----------+-----------+---------+----------+
```

Generation process:

1. Get current timestamp.
2. Append machine ID.
3. Increment sequence.
4. Return 64-bit ID.

No database lookup required.

---

## Sequence Number

Multiple IDs may be generated within the same millisecond.

Use a sequence counter:

```
Timestamp

↓

Machine ID

↓

Sequence

0
1
2
3
...
```

When the sequence is exhausted:

Wait for the next millisecond.

---

## Clock Drift

Problem:

A machine's clock moves backwards.

```
10:00:01

↓

09:59:59
```

Risk:

Duplicate IDs.

Common solutions:

- Reject generation
- Wait until clock catches up
- Use a logical clock

---

## Result

Snowflake generates IDs that are:

- Globally unique
- Mostly time ordered
- Compact (64 bits)
- Highly scalable
