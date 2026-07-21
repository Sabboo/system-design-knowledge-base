# Notification System - Playbook

## Design Decisions

| Requirement | Recommendation | Why |
|-------------|---------------|-----|
| Decouple services | Message Queue | Absorb spikes and isolate failures |
| Scale delivery | Worker Pool | Parallel processing |
| Reliable delivery | Retries + DLQ | Recover from transient failures |
| Prevent duplicates | Idempotency Key | Safe retries |
| Multiple channels | Channel abstraction | Easier to add providers |

---

## Tradeoffs

| Option | Pros | Cons |
|--------|------|------|
| Synchronous | Simple | High latency, tightly coupled |
| Asynchronous | Scalable | More components |
| Single Queue | Easy | Less isolation |
| Separate Queues | Better isolation | More operational complexity |

---

## Common Interview Questions

### Why use a queue?

It decouples producers from consumers, absorbs traffic spikes, and allows workers to process notifications asynchronously.

---

### Why use workers?

Workers process notifications independently and can scale horizontally without affecting the application.

---

### Why retries?

Provider APIs and networks occasionally fail. Retries improve delivery reliability for transient failures.

---

### Why a Dead Letter Queue?

Messages that repeatedly fail are isolated for later inspection instead of blocking normal processing.

---

### How do you avoid duplicate notifications?

Assign an idempotency key to each notification request so retries don't produce multiple deliveries.

---

### How would you support multiple channels?

Define a common notification interface, then implement separate adapters for Push, Email, SMS, and In-App delivery.

---

## Capacity Checklist

Before designing:

- Notifications per second?
- Peak traffic?
- Channel distribution?
- Retry policy?
- Delivery latency target?
- Retention period?

---

## Rule of Thumb

| Situation | Recommendation |
|-----------|----------------|
| High throughput | Queue + Worker Pool |
| External providers | Retry + DLQ |
| Multiple channels | Channel abstraction |
| Burst traffic | Queue buffering |
| Duplicate risk | Idempotency |
