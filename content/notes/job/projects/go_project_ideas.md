+++
title = 'Go Project Ideas'
date = 2025-11-21

+++

Go is arguably the best language for learning distributed systems because it is the "language of the cloud" (Kubernetes, Docker, Prometheus, and Terraform are all written in Go). Its concurrency model (Goroutines and Channels) is specifically designed to handle the complexity of networked systems.

Here are four Intermediate to Hard Golang projects that will force you to master System Design concepts.

---

### **1. Distributed Key-Value Store with Raft (The "Etcd" Clone)**

- **Difficulty:** Very Hard
- **The Goal:** Build a distributed Key-Value store (like Redis or Etcd) that remains consistent even if nodes in the cluster crash. To do this, you must implement the **Raft Consensus Algorithm**.
- **System Design Concepts:**
  - **Consensus:** How do multiple machines agree on the value of `x`?
  - **Leader Election:** A cluster needs a leader to handle writes. If the leader dies, the followers must elect a new one automatically.
  - **Log Replication:** The leader must replicate every "write" command to the followers' logs before confirming success to the user.
- **Go Specifics:**
  - Using **Channels** to handle the complex state machine of Raft (Follower $\to$ Candidate $\to$ Leader).
  - Using `time.Ticker` for heartbeats (to detect node failures).
  -
- **Why this is the "Holy Grail":** If you can build Raft from scratch, you essentially master the core of distributed consistency.

### **2. High-Throughput Pub/Sub Message Broker (The "NATS" Clone)**

- **Difficulty:** Hard
- **The Goal:** Build a message broker where "Publishers" send messages to a topic, and "Subscribers" receive them. It needs to handle thousands of messages per second.
- **System Design Concepts:**
  - **Decoupling:** Publishers shouldn't know who Subscribers are.
  - **Delivery Guarantees:** Implement "At-most-once" (fire and forget) vs "At-least-once" (requires acknowledgments/retries).
  - **Persistence:** If the broker crashes, messages in the queue should be saved to disk (Write-Ahead Log) and recovered on startup.
- **Go Specifics:**
  - **Goroutines:** Spawn a lightweight goroutine for every incoming connection (handling 10k+ concurrent connections).
  - **Buffered Channels:** Use them as the internal queues for topics.
  - **Sync.Pool:** Reduce Garbage Collection pressure by reusing message objects.
  -

### **3. Distributed Web Crawler (The "Google" Prototype)**

- **Difficulty:** Intermediate
- **The Goal:** A system that takes a seed URL (e.g., `wikipedia.org`), scrapes it, finds links, and distributes those links to worker nodes to scrape recursively.
- **System Design Concepts:**
  - **Work Distribution:** A "Master" node assigns URLs to "Worker" nodes.
  - **Deduplication:** How do you ensure two workers don't scrape the same page twice? (Bloom Filters or Shared Set).
  - **Politeness:** Enforce rate limiting per domain (don't DDOS the target site).
- **Go Specifics:**
  - **Worker Pattern:** Create a pool of workers using Goroutines that pull jobs off a shared channel.
  - **Select Statement:** Use `select` to handle timeouts (e.g., if a website takes too long to respond, drop it).

### **4. Service Registry & Health Checker (The "Consul" Clone)**

- **Difficulty:** Intermediate
- **The Goal:** In a microservices architecture, Service A needs to find Service B. Build a central registry where services "register" themselves (IP + Port) and the registry constantly checks if they are alive.
- **System Design Concepts:**
  - **Service Discovery:** Client-side discovery vs Server-side discovery.
  - **Health Checks:** Implement active checking (pinging the service every 5s) and passive checking (removing service after $N$ failed requests).
  - **TTL (Time to Live):** Services must send a "heartbeat" or they get evicted from the registry.
- **Go Specifics:**
  - **HTTP/GRPC:** Expose the registry API via HTTP, but use gRPC for internal heartbeats between services for performance.
  - **Mutexes:** Protecting the central map of services (`map[string]ServiceInfo`) is critical to prevent race conditions during concurrent reads/writes.

---

### **Summary of Skills You Will Gain**

| Project              | System Design Concept    | Go Concept                        |
| :------------------- | :----------------------- | :-------------------------------- |
| **Raft KV Store**    | CAP Theorem, Consensus   | Channels, State Machines          |
| **Pub/Sub Broker**   | Asynchronous Processing  | `sync.Pool`, High-perf Networking |
| **Web Crawler**      | MapReduce, Queueing      | Worker Pools, `select`            |
| **Service Registry** | Availability, Heartbeats | Mutex locks, gRPC                 |

### **Recommended Next Step**

I highly recommend starting with the **Distributed Web Crawler**. It is the most visually rewarding (you see data coming in) and it creates a perfect excuse to use the **Worker Pool** pattern, which is the bread-and-butter of Go concurrency.
