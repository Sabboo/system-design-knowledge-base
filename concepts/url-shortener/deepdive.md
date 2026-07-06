# URL Shortener

## Problem

Need to convert long URLs into short, unique links while supporting billions of redirects with low latency.

---

## High-Level Flow

### Create

```
Client
    │
POST /shorten
    │
Generate ID
    │
Base62 Encode
    │
Store Mapping
    │
Return Short URL
```

### Redirect

```
Client
    │
GET /abc123
    │
Cache
    │
Database (miss)
    │
301 / 302 Redirect
```

---

## Step 1 - Generate an ID

Requirements:

- Globally unique
- Fast
- Distributed

Possible strategies:

- Auto Increment
- Snowflake
- Hash(Long URL)

**See:** `patterns/unique-id-generator`

---

## Step 2 - Encode

Convert numeric ID to Base62.

```
125 → cb
```

Why Base62?

- URL safe
- Short
- Human friendly

---

## Step 3 - Store Mapping

```
Short URL
↓

Long URL
```

Example:

| Short | Long |
|-------|------|
| abc123 | https://example.com/very/long/url |

---

## Step 4 - Redirect

Lookup flow:

```
Short URL

↓

Cache

↓

Database

↓

Return Long URL

↓

301 / 302
```

Reads dominate writes, making caching essential.

---

## Step 5 - Scale

As traffic grows:

- Load Balancer
- Stateless API Servers
- Distributed Cache
- Database Replication
- Database Sharding

---

## Bottlenecks

| Component | Mitigation |
|-----------|------------|
| Database | Cache + Replicas |
| ID Generator | Distributed generation |
| Hot URLs | Cache |
| Storage | Sharding |

---

## Extensions

- Custom aliases
- Link expiration
- Analytics
- QR codes
- Spam detection
