# URL Shortener

> Convert long URLs into short, unique links with low-latency redirection.

## Functional Requirements

- Create short URLs
- Redirect short → long URL
- Optional custom aliases
- Optional expiration
- Analytics (optional)

## Non-Functional Requirements

- High availability
- Low redirect latency
- High read throughput
- Horizontally scalable

## Core Components

- API Service
- ID Generator
- Base62 Encoder
- Metadata Store
- Cache

## Design Decisions

- ID generation strategy
- Encoding scheme
- Storage engine
- Cache strategy
- Analytics collection

## Interview Recognition

Use when designing:

- TinyURL
- Bitly
- Firebase Dynamic Links
- Internal redirect services

## Key Takeaways

- Redirects are read-heavy.
- Cache hot URLs aggressively.
- Base62 produces compact URLs.
- ID generation must be globally unique.
