# Design a News Feed

## Overview

A news feed system allows users to publish posts and view recent posts from people they follow. The main challenge is delivering posts to many users while keeping feed retrieval fast and handling users with millions of followers.

## Requirements

**Functional**
- Publish posts containing text and media.
- Follow users and retrieve their posts in a personalized feed.
- Support likes, replies, and feed interactions.
- Respect privacy settings, mutes, and selective sharing.

**Non-functional**
- Low-latency feed retrieval.
- High availability and scalability.
- Eventual consistency is acceptable for feed updates.
- Efficient handling of celebrity accounts and high-fanout posts.

## Core Components

- **Web servers:** Authentication, rate limiting, and request handling.
- **Post service / Post cache:** Stores post content and metadata.
- **Graph database:** Stores follower/following relationships.
- **Fanout service:** Distributes new post IDs to eligible followers.
- **Message queue:** Buffers fanout work and decouples publishing from delivery.
- **Fanout workers:** Process queued jobs and update feed caches.
- **News Feed service:** Retrieves feed IDs and hydrates them with user and post data.
- **Caches:** News feed, content, social graph, actions, and counters.
- **CDN:** Delivers images and videos efficiently.

## Publishing Flow

1. Authenticate the user and validate the post request.
2. Store the post and its content.
3. Retrieve followers from the social graph.
4. Filter recipients using privacy and user settings.
5. Queue the follower IDs and new post ID.
6. Fanout workers add the post ID to each eligible user's feed cache.

## Feed Retrieval Flow

1. Client requests `GET /v1/me/feed`.
2. Web server forwards the request to the News Feed service.
3. Retrieve the user's feed post IDs from the feed cache.
4. Fetch post and user objects from their caches.
5. Return the hydrated feed to the client.
6. Client renders the feed; media is served through the CDN.

## Fanout Strategies

| Strategy | Advantage | Limitation |
|---|---|---|
| Fanout on write (push) | Fast feed reads | Expensive for users with many followers |
| Fanout on read (pull) | Avoids work for inactive users; no write-time fanout spike | Slower feed reads |
| Hybrid | Balances read latency and fanout cost | More complex retrieval logic |

Use push for most users and pull for celebrities or accounts with huge follower counts.

## Key Design Decisions

- Store IDs in the feed cache rather than complete post and user objects.
- Limit the number of recent feed entries cached per user.
- Use asynchronous queues and workers for fanout.
- Use a hybrid push/pull model to mitigate hot keys and excessive fanout.
- Use consistent hashing where appropriate to distribute load.
- Separate feed IDs from post content, user data, actions, and counters.

## Main Trade-off

**Precompute feeds for fast reads, but avoid precomputing every feed for high-follower accounts and inactive users.**
