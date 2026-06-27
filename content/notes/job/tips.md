+++
title = 'Tips & Pointers'
date = 2024-02-11

+++

Analyze my resume and give it a score for what percentage it was in

Scrape the jobs -> rss feeds -> extract key words -> 

https://www.reddit.com/r/golang/comments/1tn6idx/buffered_channel_size_must_be_1_uber_style_guide/
https://www.reddit.com/r/golang/comments/1tokxz5/generics_methods_are_now_implemented/

## Resume Tips

- Experience > Projects > Education > Skills
- The goal of your bullet points is to:
  - Highlight the technical work which you have done.
  - Highlight any domains you have had exposure to (building UI, building SDKs, building web services, interacting with cloud providers, working with databases, etc.)
  - Highlight technical challenges you faced and overcame.
  - Highlight the impact of your work.
- Developed a back end web service to handle user authentication utilizing JWT and interacting with existing user services to store sessions data in a Redis cache, leading to a 14% reduction in complaints about dropped sessions
- Developed a React-Native based mobile application by collaborating with product design teams, interacting with a GraphQL API allowing users to navigate and make orders to local restaurants. Led to better user engagement according to analytics funnels
- Led the development of a data pipelines platform utilizing Kafka streams, ingesting data from various data stores from across the application. Resulted in a more streamlined developer experience for data query teams and reducing congestion by 24% during peak hours
- **Good examples**: analyzed, architected, automated, built, created, decreased, designed, developed, implemented, improved, optimized, published, reduced, refactored
- **Bad examples**: aided, assisted, coded, collaborated, communicated, executed, exposed to, gained experience, helped, participated, programmed, ran, used, utilized, worked on
- **superfluous/awkward/unnecessarily complex verbs**: amplified, conceptualized, crafted, elevated, employed, engaged, engineered, enhanced, ensured, fostered, headed, honed, innovated, mastered, orchestrated, perfected, pioneered, revolutionized, spearheaded, transformed
- How did your software interface with the stepper motors to complete task X to a desired level of precision? That's the kind of stuff that stands out and shows the reader your technical abilities.
- S - Situation, T - Task, A - Action, R - Result


## System Design Interview Solving Template


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
