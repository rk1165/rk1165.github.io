+++
title = 'System Design Questions Archetypes'
date = 2025-11-21

+++
System design interviews for backend engineers typically fall into specific "archetypes." Each archetype tests a different set of engineering muscles—some focus on data consistency, others on low latency or massive concurrency.

Here are the most common system design questions categorized by the primary challenge they present.

### 1. The "Heavy Read" Systems (Social & Content)

These questions test your ability to handle massive fan-out (one user's action affecting millions of followers) and caching strategies.

- **Design a News Feed (Twitter / Facebook / Instagram):**
  - _The Core Challenge:_ "Fan-out on write" vs. "Fan-out on read." How do you efficiently build a timeline for a user who follows 5,000 people?
  - _Key Concepts:_ Push vs. Pull models, Hybrid approaches for celebrities, Pagination, Caching (Redis).
- **Design a Video Streaming Service (Netflix / YouTube):**
  - _The Core Challenge:_ Storing and delivering massive blobs of data (video) with low latency globally.
  - _Key Concepts:_ CDNs (Content Delivery Networks), Adaptive Bitrate Streaming, Data partitioning (Sharding), Transcoding workers.
- **Design a Chat Application (WhatsApp / Slack):**
  - _The Core Challenge:_ Real-time bidirectional communication and message ordering.
  - _Key Concepts:_ WebSockets vs. Long Polling, Pub/Sub patterns, "Seen" status propagation, Persistent storage (Cassandra/HBase) for chat history.

---

### 2. The "Heavy Write" & Transactional Systems

These systems require strict data consistency. Losing a ride request or a payment is unacceptable.

- **Design a Ride-Sharing Service (Uber / Lyft):**
  - _The Core Challenge:_ Real-time geospatial matching. Matching a moving rider with a moving driver instantly.
  - _Key Concepts:_ QuadTrees / Geohashing (e.g., Google S2), High write throughput for location updates, ACID transactions for billing.
- **Design a Flash Sale System (Ticketmaster / Amazon Prime Day):**
  - _The Core Challenge:_ Massive concurrency on a single inventory item (e.g., 10,000 users trying to buy the last 5 tickets).
  - _Key Concepts:_ Distributed Locking, Optimistic Concurrency Control, Queues (Kafka) for load leveling, Rate Limiting.
- **Design a URL Shortener (TinyURL / Bitly):**
  - _The Core Challenge:_ Generating unique IDs at scale without collisions.
  - _Key Concepts:_ Base62 encoding, Key Generation Service (KGS), Bloom Filters for quick lookups.

---

### 3. The "Utility & Infrastructure" Systems

These questions test your understanding of backend tools. Sometimes you are asked to design the tool _itself_ rather than a consumer app.

- **Design a Web Crawler (Google Search Bot):**
  - _The Core Challenge:_ Politeness, handling cycles (infinite loops), and parsing massive scale.
  - _Key Concepts:_ Frontier management (URL queues), DNS resolution caching, Bloom Filters to avoid re-crawling.
- **Design a Rate Limiter:**
  - _The Core Challenge:_ accurately counting requests in a distributed environment without slowing down the actual requests.
  - _Key Concepts:_ Token Bucket / Leaky Bucket algorithms, Sliding Window logs, Redis (Lua scripts).
- **Design a Distributed Key-Value Store (DynamoDB / Cassandra):**
  - _The Core Challenge:_ Consistency vs. Availability (CAP Theorem).
  - _Key Concepts:_ Consistent Hashing, Gossip Protocols, Quorum reads/writes, Hinted Handoff.

---

### 4. The "Observability" Systems

- **Design a Metric/Logging System (Datadog / Prometheus):**
  - _The Core Challenge:_ Write-heavy ingestion where losing a few metrics is okay, but the system must never go down.
  - _Key Concepts:_ Time-series databases (TSDB), Data aggregation/rollups (downsampling), Pull vs. Push models.

---

### Summary of Key Concepts to Master

To ace these questions, you don't just need to know the _steps_; you need to know the _tools_.

| Category               | Standard Tool / Pattern                                       |
| :--------------------- | :------------------------------------------------------------ |
| **Load Balancing**     | Nginx, HAProxy, Round Robin, Consistent Hashing               |
| **Caching**            | Redis, Memcached, CDNs (Cloudflare/Cloudfront)                |
| **Queues / Streaming** | Kafka (high throughput), RabbitMQ (complex routing), SQS      |
| **Databases (SQL)**    | PostgreSQL (Sharding), MySQL (Replication)                    |
| **Databases (NoSQL)**  | Cassandra (Write heavy), MongoDB (Flexible schema), DynamoDB  |
| **Communication**      | gRPC (Internal services), REST/GraphQL (External), WebSockets |

### A Simple Mental Framework for the Interview

When you get asked any of the above, use this checklist:

1.  **Scope it:** "Do we need to handle text only, or images too? Real-time or delayed?"
2.  **Scale it:** "How many DAU (Daily Active Users)? What is the Read:Write ratio?"
3.  **High-Level Design:** Draw the big boxes (LB $\to$ Web Server $\to$ DB).
4.  **Bottlenecks:** "The DB will die with this write load. Let's add a Cache or Queue."
5.  **Deep Dive:** Focus on the component that is hardest to build (e.g., the "Matching Service" in Uber).

Would you like me to perform a mock interview round with you where I pose one of these questions (e.g., "Design a Rate Limiter") and you try to solve it?

[FAANG system design interview: Design YouTube (with FAANG Senior SWE)](https://www.youtube.com/watch?v=hqa2sfoGRlI)
