# News Feed — Interview Playbook

## 1. Clarify Requirements

Ask:
- Is the feed chronological or ranked?
- Are posts text-only or do they include images and videos?
- Is the feed limited to followed users?
- How many users and posts are expected?
- Are celebrity accounts with millions of followers in scope?
- What are the feed latency and freshness requirements?

Start with a chronological feed unless ranking is explicitly required.

## 2. Identify the Two Critical Flows

**Publishing:** Store post → find followers → apply privacy rules → queue fanout → update feed caches.

**Retrieval:** Read feed IDs → hydrate post/user data → return JSON → load media from CDN.

Explain why these flows are separated: publishing needs scalable distribution, while retrieval needs low latency.

## 3. Choose a Fanout Strategy

**Question: Should we use push or pull?**

- Push: Best for most users because feed reads are fast.
- Pull: Best for celebrity accounts or users with huge follower counts.
- Hybrid: Best default for a large-scale news feed.

**Question: What is the hotkey problem?**

A celebrity post may need to be delivered to millions of followers. Processing all recipients at publish time creates a large workload concentrated around one account or event.

**Solution:** Use pull for high-follower accounts, asynchronous fanout, scalable workers, and load distribution techniques.

## 4. Explain the Cache Design

**Question: Why store only IDs in the feed cache?**

A post can appear in millions of users' feeds. Storing the full post object for each recipient would waste memory. Store feed references and keep post content in a separate cache.

**Question: How is a feed assembled?**

Read the user's post IDs, fetch post and author data from their caches, and hydrate the feed response.

**Question: Why limit the feed cache?**

Most users primarily browse recent posts. A bounded cache controls memory consumption while keeping the most frequently accessed content available.

## 5. Discuss Reliability and Consistency

Be ready to explain:
- Asynchronous queues absorb bursts and decouple publishing from fanout.
- Workers can retry failed jobs; processing should be idempotent.
- Queue lag affects how quickly posts appear.
- Feed updates can be eventually consistent.
- Privacy and mute settings must be respected during distribution and, where necessary, checked again during retrieval.
- Cache misses require fetching from the underlying data store.

## 6. Common Follow-up Questions

**What happens when a user follows someone new?**

Update the social graph. Depending on product requirements, their existing posts may be fetched on demand or selectively backfilled into the feed.

**What happens when a user unfollows or mutes someone?**

Update the relationship or preference data and ensure future feed generation excludes their posts. Existing cached entries may need filtering or cleanup.

**How do you prevent duplicate feed entries?**

Use idempotent fanout processing and a suitable uniqueness key, such as the user ID and post ID, where the data model requires deduplication.

**How do you handle very large feeds?**

Return a bounded page of recent entries and paginate older content rather than loading the entire history at once.

**How do you serve images and videos efficiently?**

Store media separately and deliver it through a CDN.

**How would you scale fanout?**

Partition queue work, scale workers horizontally, monitor queue lag, and route high-follower accounts through the pull path.

## 7. Design Checklist

- [ ] Functional and non-functional requirements clarified.
- [ ] Publishing and retrieval flows explained.
- [ ] Post storage separated from feed references.
- [ ] Push, pull, and hybrid fanout compared.
- [ ] Celebrity fanout and hotkey problem addressed.
- [ ] Queue, worker retries, and idempotency discussed.
- [ ] Cache layers and hydration explained.
- [ ] Feed-cache bounds and pagination considered.
- [ ] Privacy, eventual consistency, and CDN delivery covered.

## Rule of Thumb

**Optimize feed reads with push for ordinary users; avoid massive write amplification with pull for high-follower accounts. Keep feed references lightweight and hydrate them from separate caches.**
