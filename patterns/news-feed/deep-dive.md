# News Feed — Engineering Notes

## 1. High-Level Architecture

The system has two primary flows:

- **Feed publishing:** A new post is stored and distributed to eligible followers.
- **Feed retrieval:** A user's feed IDs are retrieved and expanded into complete post objects.

Publishing is write-heavy because one post may need to reach many followers. Retrieval is latency-sensitive because users expect their feeds to load quickly.

## 2. Feed Publishing

### Web Servers
- Validate authentication tokens.
- Apply rate limits to prevent spam and abuse.
- Accept and route post requests.

### Fanout Service

Fanout distributes a new post to the author's eligible followers.

1. Fetch follower IDs from the graph database.
2. Retrieve user information and apply privacy, mute, and sharing rules.
3. Send recipient IDs and the new post ID to a message queue.
4. Fanout workers consume jobs.
5. Workers add the post ID to each recipient's news feed cache.

The queue decouples publishing from distribution, allowing workers to process fanout asynchronously and scale independently.

### Feed Cache Data Model

The cache can be represented as a mapping between user IDs and post IDs. A conceptual table might contain:

| user_id | post_id |
|---|---|
| U1 | P100 |
| U2 | P100 |
| U3 | P100 |

The same post ID may appear in many users' feeds, but the post's full content is stored separately. This avoids duplicating large post and media objects in every feed entry.

Feed caches store a configurable number of recent entries rather than an unlimited history.

## 3. Fanout Strategies

### Fanout on Write (Push)
Precompute feeds when a post is published by inserting its ID into followers' feed caches.

**Advantages**
- Fast feed retrieval.
- Posts can appear quickly without computing the feed on every read.

**Disadvantages**
- High-follower accounts can generate huge amounts of work.
- Work is wasted for inactive followers.

The hotkey problem occurs when a popular account or data partition creates a disproportionate amount of traffic or processing work.

### Fanout on Read (Pull)
Build the feed when a user requests it by fetching recent posts from followed accounts.

**Advantages**
- No write-time fanout to every follower.
- Avoids wasted work for inactive users.
- Better suited to accounts with extremely large follower counts.

**Disadvantages**
- More work during feed retrieval.
- Reads may be slower because the feed must be assembled on demand.

### Hybrid Approach
Use push for most users and pull for celebrities or accounts with massive follower counts. This keeps normal feed reads fast while avoiding expensive fanout operations for high-follower accounts.

Consistent hashing can help distribute requests or data more evenly, but it does not eliminate the underlying cost of delivering a post to a very large audience.

## 4. Feed Retrieval

1. Client requests `GET /v1/me/feed`.
2. Load balancer routes the request to a web server.
3. Web server calls the News Feed service.
4. Service retrieves post IDs from the user's feed cache.
5. Service fetches the corresponding post and user objects from their caches.
6. Service assembles a hydrated feed and returns JSON.
7. Client renders the feed; images and videos are retrieved through the CDN.

**Hydration** means resolving feed IDs into the complete data needed by the client, such as author information, post text, profile pictures, and media URLs.

## 5. Cache Architecture

Keep these cache responsibilities separate:

- **News Feed cache:** User-to-post feed entries.
- **Content cache:** Post content and metadata; popular posts can be kept in a hot cache.
- **Social Graph cache:** Relationship data used during distribution and retrieval.
- **Action cache:** Likes, replies, and other user interactions.
- **Counters cache:** Like counts, reply counts, follower counts, and following counts.

Media files are served through a CDN rather than repeatedly transferred from the application servers.

## 6. Scaling and Reliability

- Add queue consumers to increase fanout processing capacity.
- Use backpressure and monitor queue lag when publishing spikes occur.
- Make asynchronous processing idempotent so retries do not create unintended duplicate feed entries.
- Bound feed-cache size to control memory usage.
- Monitor hot accounts, fanout volume, cache hit rate, and feed latency.
- Expect eventual consistency: a newly published post may take some time to appear in every follower's feed.
- Ensure privacy and sharing rules are applied before a post is delivered to a recipient.

## 7. Core Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Push vs. pull | Fast reads vs. cheaper publishing | Fanout work vs. read-time computation |
| IDs vs. full objects | Smaller feed cache | Additional cache lookups for hydration |
| Bounded feed cache | Predictable memory usage | Older entries may require another retrieval path |
| Asynchronous fanout | Decoupled, scalable publishing | Eventual consistency and retry handling |
| Hybrid strategy | Balances read latency and fanout load | More complex system logic |
