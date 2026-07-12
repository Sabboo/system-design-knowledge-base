# Web Crawler - Playbook

## Design Decisions

| Requirement | Recommendation | Why |
|-------------|---------------|-----|
| Prevent duplicates | Bloom Filter | Memory efficient |
| Scale downloads | Worker Pool | Parallel fetching |
| Queue URLs | URL Frontier | Central scheduling |
| Respect websites | robots.txt + crawl delay | Politeness |
| Retry failures | Exponential Backoff | Avoid overload |

---

## Common Interview Questions

### Why use a URL Frontier?

It centralizes scheduling, prioritization, retries, and duplicate prevention.

---

### Why not crawl URLs immediately after discovering them?

A scheduler is needed to control crawl order, avoid overwhelming websites, and distribute work across workers.

---

### Why use a Bloom Filter?

Checking billions of URLs with a hash set consumes significant memory. A Bloom Filter greatly reduces memory usage with a small false-positive rate.

---

### How do you avoid crawling the same page twice?

Normalize URLs and check whether they've already been seen before adding them to the frontier.

---

### How do you prevent overloading websites?

Limit concurrent requests per host, respect `robots.txt`, and apply crawl delays or rate limits.

---

### How would you scale to billions of pages?

Distribute the frontier, downloader, parser, and storage across many machines. Workers operate independently while sharing crawl state.

---

## Capacity Checklist

Before designing:

- Pages per second?
- Average page size?
- Number of workers?
- Crawl frequency?
- Storage retention?
- Expected number of unique URLs?

---

## Rule of Thumb

| Situation | Recommendation |
|-----------|----------------|
| Large-scale crawling | Distributed worker pipeline |
| Duplicate detection | Bloom Filter + URL normalization |
| Read-heavy parsing | Worker pool |
| Host fairness | Per-domain scheduling |
| Fault tolerance | Persistent frontier + retries |
