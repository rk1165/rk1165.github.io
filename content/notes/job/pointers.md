+++
title = 'System Design Interview Solving Template'
date = 2024-02-11

+++


### FEATURE EXPECTATIONS (5 min)

1. Use cases
2. Scenarios that will not be covered
3. Who will use
4. How many will use
5. Usage patterns

### ESTIMATIONS (5 min)

1. Throughput (QPS for read and write queries)
2. Latency expected from the system (for read and write queries)
3. Read/Write ratio
4. Traffic estimates
   - Write (QPS, Volume of data)
   - Read (QPS, Volume of data)
5. Storage estimates
6. Memory estimates
   - If we are using a cache, what is the kind of data we want to store in cache
   - How much RAM and how many machines do we need for us to achieve this?
   - Amount of data you want to store in disk/ssd

### DESIGN GOALS (5 min)

1. Latency and Throughput requirements
2. Consistency and Availability (Weak/Strong/Eventual => consistency | Failover/replication => availability)

### HIGH LEVEL DESIGN (5-10 min)

1. APIs for Read/Write scenarios for crucial components
2. Database schema
3. Basic algorithm
4. High level design for Read/Write scenario

### DEEP DIVE (15-20 min)

1. Scaling the algorithm
2. Scaling individual components:
   - Availability, Consistency and Scale store for each component
   - Consistency and availabilty patterns
3. Think about the following components, how they would fit in and how it would help
   - DNS
   - CDN (Push vs Pull)
   - Load Balancer (Active-Passive, Active-Active, Layer 4, Layer 7)
   - Reverse Proxy
   - Application layer scaling (Microservices, Service Discovery)
   - DB (RRDBS, NoSQL)
     - RDBMS
       - Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning
     - NoSQL
       - Key-value, Wide-Column, Graph, Document
       - Fast-lookups:
         - RAM (Bounded Size) : Redis, Memcached
         - AP (Unbounded size) : Cassandra, RIAK, Voldemort
         - CP (Unbounded size) : HBase, MongoDB, Couchbase, DynamoDB
   - Caches
     - Client caching, CDN caching, Webserver caching, Database caching, Application Caching, Cache `@Query` level, Cache `@Object` level
     - Eviction Policies
       - LRU
       - LFU
       - FIFO
     - Caching Patterns
       - Cache aside
       - Write through
       - Write behind
       - Refresh ahead
   - Asynchronism
     - Message queues
     - Task queues
     - Backpressure
   - Communication
     - TCP
     - UDP
     - REST
     - RPC

### JUSTIFY (5 min)

1. Throughput of each layer
2. Latency caused between each layer
3. Overall latency justification

### What to put in Resume

- I've read a lot of resumes in my time, and here is my advice:
  - No one reads resumes. At best they skim them. They look at the shortest lines, which are usually where you worked and the job title. Name recognition makes a difference here, no matter how many people tell you otherwise. This applies to your college as well. But don't let it discourage you, it's more of a "if you have a big name there you get a boost but it's not a negative if you don't".
  - They skim for key technologies and then read the text around them.
  - They most likely will read the full text of the most recent job, so make sure that is the most detailed and interesting.
- Your resume should talk about results. Don't say "Created a new CMS for the company". Say "Created a new CMS for the company in Python that resulted in an ~70% reduction in website update times, from 1 hour to 18 minutes."
- If you do put a "skills cloud" on it, be ready to talk about them all. If someone had a skills cloud I would immediately look for the most esoteric skills and dive deep on it in the interview (after Googling it myself) to see if you were BSing or not. If you really know that skill, you should know more than what I learned in five minutes of searching.
- All this applies to your LinkedIn as well, which you should keep up to date, so that if there is a job you want but don't want to spend the time applying, you can just send them your LinkedIn.
- Forgot to mention, as pointed out below, that your resume also drives the interview, so while some of what you write won't be read to get the interview, you still want it there as a place to jump off during an interview. Always better if you can drive the conversation towards your most positive qualities, which is easier if they are in your resume and the interviewer asks about them.

### SQL Params, their purpose and value

| Parameter               | Purpose                          | Recommended Value                     |
|-------------------------|----------------------------------|---------------------------------------|
| useSSL                  | Enable SSL encryption            | true in production                    |
| requireSSL              | Fail if SSL not available        | true in production                    |
| serverTimezone          | Set server timezone              | UTC                                   |
| allowPublicKeyRetrieval | Allow RSA key retrieval for auth | true (if using caching_sha2_password) |
| useUnicode              | Enable Unicode support           | true                                  |
| characterEncoding       | Character set                    | UTF-8                                 |
| connectionTimeZone      | Force connection timezone        | UTC                                   |
| autoReconnect           | Auto-reconnect on failure        | false (let pool handle it)            |
| failOverReadOnly        | Read-only mode on failover       | false                                 |
| connectTimeout          | Connection timeout (ms)          | 10000                                 |
| socketTimeout           | Socket read timeout (ms)         | 60000                                 |