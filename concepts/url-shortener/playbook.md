# URL Shortener - Playbook

## Design Decisions

| Requirement | Recommendation | Why |
|-------------|---------------|-----|
| Short IDs | Base62 | Compact and URL-safe |
| Distributed IDs | Snowflake | No central bottleneck |
| Fast redirects | Cache | Read-heavy workload |
| Billions of URLs | Database Sharding | Horizontal scaling |
| High availability | Replication | Fault tolerance |

---

## Tradeoffs

| Option | Pros | Cons |
|--------|------|------|
| Hash(Long URL) | Same URL → Same key | Collision handling |
| Random ID | Simple | Collision checks |
| Snowflake + Base62 | Unique, scalable | Slightly more complex |

---

## Common Interview Questions

### Why Base62?

It uses only URL-safe characters while minimizing URL length.

---

### Why not hash the long URL?

Hash collisions must still be handled, and identical URLs may not always be expected to produce the same short URL.

---

### Why cache redirects?

Redirects are read-heavy, so caching dramatically reduces database load and latency.

---

### Why 301 vs 302?

- **301**: Permanent redirect, cacheable by browsers and search engines.
- **302**: Temporary redirect, destination may change.

---

### Where would you shard?

Shard by the generated ID (or its hash) to distribute mappings evenly.

---

### How do you handle duplicate requests?

Either:
- Return the existing short URL (idempotent behavior), or
- Always generate a new one, depending on product requirements.

---

## Capacity Checklist

Ask before designing:

- URLs created per second?
- Redirects per second?
- Expected read/write ratio?
- Average URL length?
- Retention period?
- Analytics required?

---

## Rule of Thumb

| Situation | Recommendation |
|-----------|----------------|
| General-purpose URL shortener | Snowflake + Base62 + Cache |
| Read-heavy system | Cache first |
| Massive scale | Replication + Sharding |
| Custom aliases | Validate uniqueness before storing |
