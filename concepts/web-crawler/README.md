# Web Crawler

> Discover, download, and process web pages efficiently while respecting website policies.

## Functional Requirements

- Discover new URLs
- Download web pages
- Parse content
- Extract new URLs
- Avoid duplicate crawling
- Respect robots.txt

## Non-Functional Requirements

- Highly scalable
- Fault tolerant
- Polite crawling
- Efficient storage
- Incremental crawling

## Core Components

- URL Frontier
- Scheduler
- Downloader
- HTML Parser
- URL Extractor
- Deduplicator
- Storage

## Design Decisions

- URL scheduling strategy
- Duplicate detection
- Crawl frequency
- Storage format
- Failure handling

## Interview Recognition

Use when designing:

- Search engines
- SEO crawlers
- Web archiving
- Site monitoring
- Content aggregation

## Key Takeaways

- The crawler is a distributed pipeline.
- URL Frontier controls crawl order.
- Deduplication prevents revisits.
- Politeness is as important as performance.
