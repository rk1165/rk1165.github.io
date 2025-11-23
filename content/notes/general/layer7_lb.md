+++
title = 'Layer 7 Load balancer'
date = 2025-11-21

+++

Designing a custom Layer 7 (Application Layer) load balancer is a significant systems engineering challenge that involves parsing HTTP protocols, managing concurrent connections, and intelligently routing traffic based on content.

Here is a comprehensive guide to designing one, starting from the architecture to the step-by-step implementation approach.

### 1\. High-Level Architecture

Unlike a Layer 4 load balancer (which routes based on IP/Port), a Layer 7 load balancer acts as a **Reverse Proxy**. It terminates the connection from the client, reads the request, and opens a _new_ connection to the backend server.

**Core Components:**

1.  **Listener:** Listens on a specific port (e.g., 80 or 443) for incoming TCP connections.
2.  **Parser:** Decodes the raw byte stream into HTTP Request objects (Headers, Body, Method, URL).
3.  **Router/Rule Engine:** Decides which backend pool to use based on the parsed data (e.g., `/api` goes to Pool A, `/static` goes to Pool B).
4.  **Load Balancing Strategy:** Selects a specific server from the chosen pool (e.g., Round Robin, Least Connections).
5.  **Backend Manager:** Manages connections to upstream servers (Health checks, connection pooling).

---

### 2\. Step-by-Step Implementation Approach

Do not try to build the full system at once. Follow this iterative path:

#### Phase 1: The Simple Proxy (1-to-1)

- **Goal:** Create a server that accepts a connection, reads the data, forwards it to _one_ hardcoded backend, and returns the response.
- **Action:**
  - Open a TCP socket binding to port 80.
  - On `accept()`, read the buffer.
  - Open a client socket to your backend server (e.g., `localhost:8080`).
  - Pipe bytes from Client $\to$ LB $\to$ Backend and vice versa.
- **Key Learning:** Understanding socket lifecycles and buffer management.

#### Phase 2: Protocol Parsing (The "Layer 7" Part)

- **Goal:** Make routing decisions based on content.
- **Action:**
  - Instead of blindly piping bytes, parse the incoming data.
  - Extract the **HTTP Method** (GET/POST) and **URI** (Path).
  - Extract **Headers** (Host, User-Agent, Cookies).
- **Critical Decision:** You must implement a parser that complies with HTTP/1.1 (handling `Content-Length` and `Transfer-Encoding: chunked`).
  - _If the request is malformed, return 400 Bad Request immediately._

#### Phase 3: The Load Balancer (1-to-Many)

- **Goal:** Distribute traffic across multiple backends.
- **Action:**
  - Create a configuration file (JSON/YAML) defining "Target Groups" or "Pools" of backend IPs.
  - Implement the **Round Robin** algorithm.
  - Update the main loop:
    1.  Parse Request.
    2.  Select Backend Pool based on URL path.
    3.  Select specific Backend IP using Round Robin.
    4.  Forward Request.

#### Phase 4: Concurrency Model

- **Goal:** Handle thousands of concurrent users.
- **Action:** Move away from "Thread per Connection" (which is slow and memory-intensive).
- **Solution:** Use **Non-blocking I/O (Async)**.
  - **In C/C++:** `epoll` (Linux) or `kqueue` (BSD/macOS).
  - **In Go:** Goroutines (lightweight threads).
  - **In Node:** Event Loop.
  - **In Java:** Java NIO (Netty).

#### Phase 5: Reliability & Health Checks

- **Goal:** Stop sending traffic to dead servers.
- **Action:**
  - **Passive Health Checks:** If a connection to a backend fails (TCP RST or Timeout), mark it as "down" for 60 seconds.
  - **Active Health Checks:** Run a background thread that pings `GET /health` on every backend every 5 seconds.

---

### 3\. Critical Problems & Solutions

When building a production-grade L7 LB, you will encounter these specific engineering hurdles:

#### Problem A: The "Slow Loris" & Concurrency

If a client connects but sends data very slowly (1 byte per second), a "thread-per-request" model will exhaust all server threads, blocking legitimate users.

- **Solution:** Use an **Event-Driven Architecture (Reactor Pattern)**. One thread manages thousands of connections. Use read/write timeouts aggressively. If a client doesn't send a full HTTP header within $X$ seconds, drop the connection.

#### Problem B: High Latency (TCP Handshake Overhead)

Opening a new TCP connection to the backend for every single request is expensive (3-way handshake).

- **Solution: Connection Pooling (Keep-Alive).**
  - Maintain a pool of _open_ TCP sockets to the backend servers.
  - When a request comes in, borrow an existing connection from the pool.
  - When the request finishes, do not close the socket; return it to the pool.

#### Problem C: Sticky Sessions (Session Persistence)

If a user logs in on Server A, but their next request goes to Server B, they might be logged out if session state isn't shared.

- **Solution:**
  - **Cookie Insertion:** The LB injects a cookie (e.g., `LB_NODE=ServerA`) into the response. On the next request, the LB reads this cookie and routes explicitly to Server A.
  - **Source IP Hashing:** Hash the client's IP address so they always map to the same backend (less reliable due to NAT).

#### Problem D: SSL/TLS Termination

Decryption is CPU intensive.

- **Solution:**
  - Offload SSL at the load balancer level using OpenSSL libraries.
  - Pass traffic to backends over unencrypted HTTP (if within a private VPC) to save backend CPU cycles.
  - Use hardware acceleration (AES-NI instructions) if available.

### 4\. Summary of Algorithms to Implement

1.  **Round Robin:** Loop through the list sequentially. (Simple, stateless).
2.  **Least Connections:** Send traffic to the server with the fewest active connections. (Best for long-lived connections like WebSockets).
3.  **Weighted Round Robin:** Assign "weights" to servers (e.g., a powerful server gets 3 requests for every 1 request a weak server gets).
4.  **IP Hash:** `hash(Client_IP) % Number_Of_Servers`. Ensures a user stays on the same server.

### 5\. Tech Stack Recommendations for a Custom Build

- **Performance (Hard Mode):** C, C++, Rust (using `epoll`/`io_uring`).
- **Balance (Medium Mode):** Go (Golang's `net/http` library is excellent for this).
- **Prototyping (Easy Mode):** Node.js or Python (Twisted/Asyncio).

### Suggested File Structure

```text
/src
  /core
    server.go       # Main TCP listener
    worker.go       # Event loop worker
  /http
    parser.go       # HTTP Header parsing logic
    writer.go       # Constructing responses
  /proxy
    router.go       # Logic matching /path to Pools
    balancer.go     # Algorithms (RR, LeastConn)
    health.go       # Background health checker
  /config
    config.yaml     # Port, Backend IPs, SSL cert paths
```
