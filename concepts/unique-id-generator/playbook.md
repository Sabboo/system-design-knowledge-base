# Unique ID Generator - Playbook

## Which solution should I choose?

| Requirement | Recommendation |
|-------------|----------------|
| Simplicity | UUID |
| Ordered IDs | Snowflake |
| Database primary key | Snowflake |
| No coordination | UUID |
| Highest scalability | Snowflake |

---

## Tradeoffs

| Solution | Biggest Advantage | Biggest Drawback |
|----------|-------------------|------------------|
| Auto Increment | Simple | Doesn't scale |
| UUID | Coordination-free | Large and unordered |
| Multi-Master | Ordered | Operational complexity |
| Snowflake | Fast + sortable | Depends on clock |

---

## Common Interview Questions

### Why not UUID?

Easy to generate but large, unordered, and inefficient for clustered database indexes.

---

### Why not Auto Increment?

Requires a central authority or coordinated database, creating a scalability bottleneck.

---

### Why Snowflake?

Generates compact, unique, sortable IDs locally without database coordination.

---

### What if two servers generate IDs simultaneously?

Each server has a unique machine ID, so their IDs remain unique even if the timestamp is identical.

---

### What if the clock moves backwards?

Generation should pause or otherwise prevent using an earlier timestamp to avoid duplicate IDs.

---

## Rule of Thumb

| Situation | Recommendation |
|-----------|----------------|
| Internal distributed system | Snowflake |
| Public API identifier | UUID or ULID |
| Relational database PK | Snowflake |
| Small monolith | Auto Increment |
