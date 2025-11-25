+++
title = 'Tiny Url'
date = 2025-11-21

+++

Designing a URL shortening service (like TinyURL or Bitly) is a classic system design interview question because it seems simple on the surface but involves handling massive scale, concurrency, and database optimization.

Here is a step-by-step guide to designing this system, from the basic math to the advanced architecture.

---

### 1. Requirements & Capacity Planning

Before designing, we must agree on the scale. Let’s assume a high-scale system:

- **Traffic:** 100 million new URLs generated per month.
- **Read/Write Ratio:** 100:1 (For every 1 URL shortened, it is clicked 100 times).
- **Lifespan:** We store URLs for 5 years.

**The Math (Back-of-the-envelope):**

- **Writes:** 100M requests/month $\approx$ 40 requests/second.
- **Reads:** 100M $\times$ 100 = 10B requests/month $\approx$ 4,000 requests/second.
- **Storage:** If one record is 500 bytes:
  $$100\text{M} \times 12\text{ months} \times 5\text{ years} \times 500\text{ bytes} \approx 3\text{ TB of data}$$

---

### 2. The Algorithm: Generating the Short Key

The core problem is converting a Long URL into a unique Short Key (e.g., `bit.ly/3xYz21`).

#### Approach A: Hashing (MD5/SHA)

You could hash the Long URL (e.g., MD5) and take the first 6-7 characters.

- **Problem:** Hashing produces collisions. Different Long URLs might result in the same first 6 characters. Handling these collisions requires expensive database lookups.

#### Approach B: Base62 Conversion (Recommended)

We assign a unique integer ID to every database entry (1, 2, 3...) and convert that integer to **Base62**.

- **Why Base62?** It uses `[a-z]`, `[A-Z]`, and `[0-9]`. (26 + 26 + 10 = 62 characters).
- **Capacity:** With a length of 7 characters, we have $62^7$ combinations.
  $$62^7 \approx 3.5 \text{ Trillion combinations}$$
  This is more than enough for 5 years of data.

**The Workflow:**

1.  Database auto-increments ID to `100,000,000`.
2.  System converts `100,000,000` to Base62 $\rightarrow$ `8m0K`.
3.  Short URL becomes `tinyurl.com/8m0K`.

---

### 3. High-Level Architecture

**Step-by-Step Flow:**

1.  **Client** sends a `POST` request with the Long URL.
2.  **Web Server** talks to a unique ID generator (see "KGS" below).
3.  **Worker** converts the ID to Base62.
4.  **DB** stores the mapping: `{ id: 123, short: "abc", long: "http://..." }`.
5.  **Client** receives the short URL.
6.  **On Redirect:** Client hits the short URL $\rightarrow$ Load Balancer $\rightarrow$ Cache (Redis) $\rightarrow$ Database $\rightarrow$ Returns HTTP 301/302 to Client.

---

### 4. API Design

You need two primary endpoints. REST is suitable here.

**1. Shorten URL**

- **Endpoint:** `POST /api/v1/data/shorten`
- **Payload:** `{ "long_url": "https://www.google.com/..." }`
- **Response:** `{ "short_url": "https://tiny.url/xyz123" }`

**2. Redirect URL**

- **Endpoint:** `GET /{short_url}`
- **Response:** HTTP Redirect (Status 301 or 302) to the Long URL.

---

### 5. Critical Problems & Solutions

Designing for scale introduces several specific problems.

#### Problem A: The "Distributed ID" Problem

If you have multiple web servers, they cannot simply use a local counter (e.g., `count++`) to generate IDs, or two servers might generate ID `100` at the same time, causing a collision.

- **Solution 1: Database Auto-Increment:** Use a central database to generate IDs.
  - _Risk:_ The database becomes the bottleneck (Single Point of Failure).
- **Solution 2: Key Generation Service (KGS):** Create a standalone service that pre-generates keys.
  - It generates keys offline and stores them in a "Unused Key" table.
  - Web servers request a batch of keys (e.g., 1,000 keys) and keep them in memory.
  - **Benefit:** Reads/writes are instant; no collision checks needed.

#### Problem B: 301 vs. 302 Redirects

Which HTTP status code should you return when a user clicks the short link?

- **301 (Permanent Redirect):** The browser caches the redirection. Next time the user clicks the link, the browser goes _directly_ to the long URL without hitting your server.
  - _Pro:_ Reduces load on your server.
  - _Con:_ **You lose analytics.** You cannot track how many times the link was clicked after the first time.
- **302 (Temporary Redirect):** The browser hits your server every single time.
  - _Pro:_ Accurate analytics.
  - _Con:_ Higher server load.
- **Verdict:** Use 302 if analytics are part of your business model. Use 301 if you want to save money on infrastructure.

#### Problem C: Database Lookups are Slow

Hitting the database for every click (10 billion/month) will crash a standard RDBMS.

- **Solution: Caching (Redis/Memcached).**
  - Since URLs don't change often, they are perfect for caching.
  - **Flow:** Check Cache $\rightarrow$ If Miss, Check DB $\rightarrow$ Update Cache.
  - **Eviction Policy:** Use **LRU (Least Recently Used)**. If the cache is full, remove URLs that haven't been clicked in a while.

#### Problem D: malicious Users

Users might spam the service or create loops.

- **Solution:**
  - **Rate Limiting:** Limit users to $X$ requests per minute via their API Key or IP address.
  - **Sanitization:** Verify the input URL is valid and accessible before shortening it.

---

### Summary Data Model

If using a NoSQL database (like DynamoDB or Cassandra) for easy scaling:

| PK (ShortKey) | LongURL               | CreationDate | UserID   | ExpirationDate |
| :------------ | :-------------------- | :----------- | :------- | :------------- |
| `3hK8`        | `http://google.com`   | `2023-10-27` | `User_1` | `2028-10-27`   |
| `3hK9`        | `http://facebook.com` | `2023-10-27` | `User_2` | `2028-10-27`   |

### Would you like me to...

1.  Write the Python/Go code for the **Base62** conversion logic?
2.  Deep dive into how to design the **Key Generation Service (KGS)** specifically?
3.  Create a schema for the database in SQL vs NoSQL?
