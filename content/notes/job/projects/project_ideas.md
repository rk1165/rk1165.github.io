+++
title = 'Project Ideas'
date = 2025-10-21

+++

### Project ideas to work on

- Make a REST API for generating random social media usernames, add a database, user authentication, Stripe payments, and analytics.
- Build your own api gateway.
- password manager which will interact with bitwarden for creating/updating/deleting/reading from terminal?
- Make a bitcask-style DB.
- Make a git clone.
- Make a load balancer that handles OAuth2/jwt auth for the services it proxies.
- Make a Docker clone.
- Parallel torrent / file downloader should be fun. With progress bars and shit
- Booking.com scrape
- Sync between cross browsers - Apple notes and Google keep
- Travel plan together with friends
- Summarise job listing using gpt and try to find startup opportunities
- To get a better understanding of the kernel, students could:
  - print "hello world" during the boot process;
  - design their own scheduler;
  - modify the page-handling policy; and
  - create their own filesystem.
- Should implement the following:
  - an HTTP client and daemon
  - a DNS resolver and Server
  - a command-line SMTP mailer
- RSA is easy enough to implement that everyone should do it

### Portfolio vs Resume

- Every CS major should build a _portfolio_
  - A portfolio could be as simple as a personal blog, with a post for each project or accomplishment. A better portfolio would include per-project pages, and publicly browsable code.
  - Contributions to open source should be linked and documented.
- [Github is my resume](http://pydanny.blogspot.com/2011/08/github-is-my-resume.html)

### Chat APP

- Chat app. For the purposes of practicing concurrency, it's fine to just set up a stupid simple TCP connection that you can actually just use "netcat" to connect to on the command line; don't waste a lot of time here if you don't want to go down a large rabbit hole on making this slick.
- The chat server that is managing multiple connections and sending the received messages out to all the other users will have a nice amount of concurrency it in. Run the whole thing under the race detector, because you may not have an easy time triggering race conditions on your own, but the detector will do a good job of pointing out the problems.
- The server can just open a socket, and for all incoming connections wrap a bufio around it and have a goroutine reading lines. Another goroutine can be responsible for writing the lines. You don't need auth and you don't even need "names", or formatting, or "chat rooms", or anything else; just very raw messaging. (But those are all fine followups if you feel inclined!)
- To give you an idea what I'm thinking of here, it's probably on the order of ~500 lines when its done.
- I would recommend sending it to all the other users; if A says "hello", B and C get it but you don't send it back to A.
- Sending it to everyone in the first pass is easier, but sending it only to the one who didn't send it will be a good exercise.


## Go Project Ideas

### **1. High-Throughput Pub/Sub Message Broker (The "NATS" Clone)**

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

### **2. Service Registry & Health Checker (The "Consul" Clone)**

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
| **Pub/Sub Broker**   | Asynchronous Processing  | `sync.Pool`, High-perf Networking |
| **Service Registry** | Availability, Heartbeats | Mutex locks, gRPC                 |

## Java Project ideas

#### **1. Regular Expression Engine**

- **Difficulty:** Hard
- **The Goal:** Don't use `java.util.regex`. Write a class that takes a pattern `(a|b)*c` and matches it against a string.
- **Key Features:**
  - Build a **Finite Automaton** (NFA/DFA) from the input string.
  - Support operators like `*` (zero or more), `+` (one or more), and `|` (OR).
- **Core Concepts:** Theory of Computation, Graph Traversal, Recursion.

#### **2. Custom Java Virtual Machine (Mini-JVM)**

- **Difficulty:** Very Hard
- **The Goal:** Write a program in Java that reads a simple `.class` file (bytecodes) and executes it.
- **Key Features:**
  - Implement a **Stack Machine**: Push/Pop values as you read instructions like `iadd`, `iconst`, `imul`.
  - Read the Constant Pool to understand method names and variable types.
- **Core Concepts:** Bytecode, Stack Data Structures, Interpreters.

#### **3. Mini-Kafka (Distributed Log/Message Queue)**

- **Difficulty:** Very Hard
- **The Goal:** Build a persistent message queue where producers write messages to a "Topic" and consumers read them.
- **System Design Concepts You Will Learn:**
  - **Sequential Disk I/O:** Learn why appending to a file is faster than random database writes.
  - **Partitioning/Sharding:** Split a "Topic" into multiple partitions so different consumers can read in parallel.
  - **Offset Management:** Tracking which consumer has read which message.
- **The "Hard" Implementation Details:**
  - **Memory Mapped Files (mmap):** Use `java.nio.MappedByteBuffer` to map the log file directly into memory for lightning-fast writes (this is how the real Kafka works).
  - **Replication:** (Optional extension) Ensure messages are written to a "Leader" node and copied to a "Follower" node.
- **Java Stack:** Java NIO, FileChannel, Sockets.
