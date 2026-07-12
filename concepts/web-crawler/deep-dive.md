# Web Crawler

## Problem

Need to crawl billions of web pages while avoiding duplicates, respecting website limits, and scaling across many workers.

---

## High-Level Flow

```
Seed URLs
    ↓
URL Frontier
    ↓
Scheduler
    ↓
Downloader
    ↓
HTML Parser
    ↓
Extract Links
    ↓
Deduplicate
    ↓
URL Frontier
```

The process repeats continuously.

---

## Step 1 - Seed URLs

Start with an initial list of trusted URLs.

Example:

```
https://example.com
https://wikipedia.org
```

---

## Step 2 - URL Frontier

Acts as the crawl queue.

Responsibilities:

- Store pending URLs
- Prioritize URLs
- Prevent duplicate scheduling
- Retry failed requests

The frontier is the heart of the crawler.

---

## Step 3 - Scheduler

Decides:

- Which URL to crawl next
- Which worker should crawl it
- When a site can be crawled again

Must avoid sending many requests to the same host simultaneously.

---

## Step 4 - Downloader

Fetch the page.

Possible outcomes:

- Success
- Redirect
- Timeout
- Server Error

Retry only transient failures.

---

## Step 5 - Parse HTML

Extract:

- Page title
- Metadata
- Outgoing links
- Content

Store useful information.

---

## Step 6 - Extract URLs

Find hyperlinks:

```
<a href="...">
```

Normalize URLs before storing.

Example:

```
https://site.com/

↓

https://site.com
```

---

## Step 7 - Deduplicate

Before adding a URL:

```
Already seen?

↓

Yes → Ignore

No → Add to Frontier
```

Large crawlers often use Bloom Filters to reduce memory usage.

---

## Step 8 - Storage

Typically store:

- Raw HTML
- Parsed content
- Metadata
- Crawl history

---

## Scaling

Scale horizontally by adding:

- More download workers
- More parsing workers
- Distributed frontier
- Distributed storage

The pipeline naturally parallelizes.

---

## Common Challenges

- Infinite crawl loops
- Duplicate URLs
- Dynamic pages
- Rate limiting
- Very large websites
