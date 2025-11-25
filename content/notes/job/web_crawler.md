+++
title = 'Distributed Web Crawler'
date = 2025-11-21

+++

Designing a distributed web crawler is a classic system design interview question because it touches on almost every pillar of distributed computing: consistency (deduplication), availability (fault tolerance), partition tolerance (network issues), and scalability.

Here is a step-by-step breakdown of how to design one, moving from the high-level architecture to the nitty-gritty of distributed coordination.

---

### 1. High-Level Architecture

At a high level, the system forms a cycle. We need to pull a URL, download it, extract new URLs, and feed them back into the queue.

**The Core Components:**

1.  **Seed URLs:** The entry points (e.g., verified news sites, university domains).
2.  **URL Frontier:** The master "To-Do" list. It schedules _what_ to crawl and _when_.
3.  **DNS Resolver:** Maps URLs to IP addresses (needs heavy caching).
4.  **HTML Fetcher (Worker):** The component that actually connects to the web server and downloads pages.
5.  **Content Parser:** Validates HTML and extracts new links.
6.  **Deduplication Service:** Checks if we have already crawled a URL or content.
7.  **Storage:** Saves the raw data (S3/HDFS) and metadata (NoSQL).

---

### 2. The URL Frontier (The Brain)

The Frontier is not just a simple FIFO queue. If you treat it as one, you will inadvertently DDoS a single website (politeness violation) or get stuck in spider traps. You need a **Prioritization vs. Politeness** split.

#### The Architecture:

- **Front Queues (Prioritization):** Incoming URLs are assigned a priority (e.g., PageRank, update frequency). High-priority URLs go to the high-priority queue.
- **Back Queues (Politeness):** This is a set of queues mapped to specific domains (e.g., one queue for `wikipedia.org`, one for `cnn.com`).
- **The Mapping:** A router takes a URL from a Front Queue and routes it to the correct Back Queue based on the host.
- **The Selector:** A separate thread/process monitors the Back Queues. It enforces a time delay between requests to the same host (e.g., "Wait 2 seconds before pulling from the CNN queue again").

> **Key Concept:** This ensures that while your system is crawling thousands of pages per second, no single external server receives more than 1 request every few seconds.

---

### 3. The Fetcher Module (The Worker)

This is where the distributed nature shines. You will have hundreds of worker nodes running the Fetcher service.

- **Protocol Support:** Must handle HTTP/HTTPS, but potentially also FTP.
- **Robots.txt:** Before fetching `google.com/about`, the fetcher must check `google.com/robots.txt`. This needs to be cached locally to avoid fetching `robots.txt` for every single request.
- **Concurrency:** Do not use one thread per request. Use **asynchronous I/O** (like Node.js or Python `asyncio`) to handle thousands of open connections per worker node.

---

### 4. The Deduplication Service (The Filter)

The internet contains cycles. If you crawl A -> B -> A, you need to know A was already visited.

#### Option A: Database Lookup (Too Slow)

Checking a SQL database for every URL is a massive bottleneck.

#### Option B: Bloom Filters (The Right Way)

A Bloom Filter is a probabilistic data structure that tells you either "This definitely is NOT in the set" or "This MIGHT be in the set."

- It uses very little memory (bits instead of strings).
- **Workflow:**
  1.  Hash the URL.
  2.  Check the Bloom Filter.
  3.  If the filter says "Not in set," crawl it.
  4.  If the filter says "Might be in set," then (and only then) check the actual database/cache to confirm.

---

### 5. Data Storage

You are storing two different types of data:

| Data Type       | Recommended Storage                      | Why?                                                                                                                  |
| :-------------- | :--------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- |
| **Raw Content** | **Object Store (S3, HDFS)**              | You are storing massive blobs (HTML files, PDFs). Object stores are cheap and scalable for Write-Once-Read-Many data. |
| **Metadata**    | **Wide-Column NoSQL (Cassandra, HBase)** | You need to store URL, download time, content hash, and headers. NoSQL handles high write throughput better than SQL. |

---

### 6. Fault Tolerance

In a distributed system, workers _will_ crash.

- **Consistent Hashing:** Use consistent hashing to distribute URLs among downloader servers. If a server goes down, its keys are redistributed to neighbors without reshuffling the whole cluster.
- **Snapshots:** The URL Frontier must persist its state to disk periodically. If the Frontier service crashes, it should reload the queues from the last checkpoint rather than starting over from scratch.
- **Dead Letter Queues:** If a URL fails to download (4xx/5xx errors) multiple times, move it to a "Dead Letter Queue" for manual inspection or later retry, so it doesn't clog the active queues.

---

### 7. Step-by-Step Workflow Summary

1.  **Frontier** sends a batch of URLs to a **Fetcher**.
2.  **Fetcher** checks the **DNS Cache** (resolves IP if missing).
3.  **Fetcher** checks `robots.txt` cache.
4.  **Fetcher** downloads the HTML and sends it to **Storage**.
5.  **Parser** reads the HTML from storage, extracts links (Anchor tags).
6.  **Parser** passes new links to the **Deduplication Service**.
7.  **Deduplication Service** filters out seen URLs and passes unique ones back to the **Frontier**.
8.  **Frontier** calculates priority and assigns the URL to the correct Politeness Queue.

### A Next Step for You

The **Bloom Filter** is often the most confusing but powerful concept for developers new to this scale. Would you like me to show you how to implement a basic Bloom Filter in Python or Go to see how the bit-array logic actually works?
