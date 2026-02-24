+++
title = 'Distributed Rate Limiter'
date = 2025-11-21

+++

Designing a distributed rate limiter is a delicate balancing act between **precision** (accurately counting requests) and **latency** (not slowing down the user's request).

Here is a step-by-step breakdown of how to design a production-grade distributed rate limiter.

---

### 1\. High-Level Placement

Where does the limiter live?
If you place it inside the application code, you couple your business logic with infrastructure logic.

- **The Best Spot:** The **API Gateway** (or a Sidebar Proxy like Envoy).
- **Why?** It acts as a shield. It blocks excess traffic _before_ it hits your expensive backend servers.

---

### 2\. The Algorithm Selection

Before designing the infrastructure, you must choose how you count.

#### Option A: Fixed Window (The Simple Trap)

You simply count requests per minute (e.g., 12:00 to 12:01).

- **The Problem:** If a user sends 10 requests at 12:00:59 and 10 requests at 12:01:01, they effectively sent 20 requests in 2 seconds, potentially overwhelming the system, even though the per-minute rate was respected.

#### Option B: Token Bucket (The Standard)

- **Concept:** Imagine a bucket that gets filled with tokens at a constant rate (e.g., 10 tokens/second). Every time a request comes in, it takes a token. If the bucket is empty, the request is dropped.
- **Pros:** Handles "bursts" of traffic well (you can accumulate tokens) but enforces a long-term average.

#### Option C: Sliding Window Counter (The Accurate Choice)

This combines the memory efficiency of Fixed Windows with the accuracy of Sliding Logs.

- **Logic:**
  - Limit: 100 req/min.
  - Request comes at 1:00:30 (30% into the current minute).
  - We calculate: `(Requests in Current Min) + (Requests in Previous Min * 0.7)`.
  - If the sum \> 100, block.
  - **Why:** This approximates a rolling window without storing every single timestamp.

---

### 3\. Data Storage (The "Distributed" Challenge)

Since this is distributed, multiple API Gateways need to share the counter.

- **Memory (Local HashMap):** Fails. Gateway A doesn't know Gateway B received 50 requests.
- **Database (Postgres):** Fails. Too slow. Disk I/O will add 10-50ms to every request.
- **Distributed Cache (Redis):** **The Winner.**
  - It uses RAM (extremely fast).
  - It supports atomic increments.
  - It supports Time-To-Live (TTL) to automatically expire old counters.

---

### 4\. Handling Race Conditions (The Critical Step)

This is the most common interview failure point.

**The Scenario:**

1.  **Gateway A** reads the counter for `User_1` (Value: 9).
2.  **Gateway B** reads the counter for `User_1` (Value: 9).
3.  **Gateway A** increments to 10 and allows the request.
4.  **Gateway B** increments to 10 (overwriting A's work) and allows the request.
    _Result:_ The user sent 11 requests, but the system thinks they sent 10.

**The Solution: Lua Scripts (Redis)**
You cannot use a "Read-Modify-Write" pattern in your application code. You must push the logic _into_ Redis.
Redis allows you to upload a small Lua script. Redis guarantees that a Lua script runs **atomically** (no other command runs while the script is executing).

**Pseudocode for the Lua Script:**

```lua
-- Arguments: Key (user_id), Limit, Window_Time
local current_count = redis.call('GET', KEYS[1])
if current_count and tonumber(current_count) >= tonumber(ARGV[1]) then
    return 0 -- Rejected
else
    redis.call('INCR', KEYS[1])
    if tonumber(current_count) == 0 then
        redis.call('EXPIRE', KEYS[1], ARGV[2]) -- Set expiry on first write
    end
    return 1 -- Allowed
end
```

---

### 5\. Optimization: The "Thundering Herd" & Latency

Even with Redis, calling a remote cache for _every_ request adds network latency (e.g., 5ms). If you have millions of requests, this burns network bandwidth.

**Strategy: Local Buffer (Synchronization)**
Instead of talking to Redis for every request, use a hybrid approach:

1.  **Local Memory:** Each Gateway node counts locally.
2.  **Sync:** Every few seconds (or every 10 requests), the Gateway pushes its local count to Redis and pulls the global total.

<!-- end list -->

- **Trade-off:** You gain massive performance, but your rate limiter becomes "Eventual Consistent." You might allow a user to exceed their limit by a small margin (e.g., 105 requests instead of 100) before the nodes sync up. This is usually acceptable.

---

### 6\. Step-by-Step Workflow Summary

1.  **Client** sends a request (`GET /api/resource`).
2.  **API Gateway** intercepts the request.
3.  **Gateway** constructs a key: `ratelimit:{user_id}:{minute_timestamp}`.
4.  **Gateway** executes the **Lua Script** against the Redis Cluster.
    - _Inside Redis:_ Check count, Increment, Check Limit.
5.  **Redis** returns result: `ALLOWED` or `BLOCKED`.
6.  **Gateway** Decision:
    - _If Allowed:_ Forward request to Backend Service.
    - _If Blocked:_ Immediately return **HTTP 429 Too Many Requests** with a header `Retry-After: 30`.

### 7\. Fault Tolerance (Fail-Open vs. Fail-Closed)

What happens if Redis goes down?

- **Fail-Closed:** Block all traffic. (Bad for business, good for strict security).
- **Fail-Open:** Allow all traffic. (Good for user experience, risky for backend load).
- **Recommendation:** **Fail-Open.** If your rate limiter breaks, you shouldn't take down your entire website. Alert the team, but let the traffic flow.

### A Next Step for You

The specific implementation of the **Lua Script** is powerful because it works for rate limiting, inventory management, and distributed locking.
