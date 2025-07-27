+++
title = 'Scalability Notes'
date = 2024-02-11

+++

### Web scalability for Startup Engineers

- The key thing to observe is that any data you put into the session should be stored outside of the web server itself to be available from any web server.
- There are three common ways to solve this problem:
  - Store session state in cookies
  - Delegate the session storage to an external data store
  - Use a load balancer that supports sticky sessions
- Using cookies for session data storage works very well as long as you can keep your data minimal. If all you need to keep in session scope is user ID or some security token, you will benefit from the simplicity and speed of this solution
- The second alternative approach is to store session data in a dedicated data store. In this case, your web application would take the session identifier from the web request and then load session data from an external data store. The only requirement here is to have very low latency on get-by-key and put-by-key operations
- Finally, you can handle session state by doing nothing in the application layer and pushing the responsibility onto the load balancer.
- If your startup grows much larger, you can also use **latency-based routing** of Route 53 to direct your clients to the “closest” data center. If you were hosting your servers in multiple Amazon regions, your clients would actually benefit from establishing a connection to a region that is closer to their location. Route 53 allows you to do that easily using latency-based routing
- ELB needs some time to “warm up” and scale out. If you get sudden spikes in traffic that require doubling capacity in a matter of seconds or minutes, ELB may be too slow for you.
- HAProxy, on the other hand, is simpler in design than Nginx, as it is just a load balancer. It can be configured as either a layer 4 or layer 7 load balancer. When HAProxy is set up to be a layer 4 proxy, it does not inspect higher-level protocols and it depends solely on TCP/IP headers to distribute the traffic. This, in turn, allows HAProxy to be a load balancer for any protocol, not just HTTP/HTTPS. You can use HAProxy to distribute traffic for services like cache servers, message queues, or databases.
- HAProxy can also be configured as a layer 7 proxy, in which case it supports sticky sessions and SSL termination, but needs more resources to be able to inspect and track HTTP-specific information.
- The second alternative to distributed transactions is to provide a mechanism of compensating transaction. A compensating transaction can be used to revert the result of an operation that was issued as part of a larger logical transaction that has failed.
- Cassandra, allow clients to fine-tune the guarantees and tradeoffs made by specifying the consistency level of each query independently. Rather than having a global tradeoff affecting all of your queries, you can choose which queries require more consistency and which ones can deal with stale data, gaining more availability and reducing latency of your responses.
- All you need to do to replace a broken server is add a new (blank) one and tell Cassandra which IP address this new node is replacing.
- As a result, some use cases that add and delete a lot of data can become inefficient because deletes increase the data size rather than reducing it (until the compaction process cleans them up).
- You can use different slaves for different types of queries. For example, you could use one slave for regular application queries and another slave for slow, long-running reports.
- To prioritize what needs to be cached, use a simple metric of aggregated time spent generating a particular type of response. You can calculate the aggregated time spent in the following way: `aggregated time spent = time spent per request * number of requests`
- In the case of cache-aside caches, the application needs to be aware of the existence of the object cache, and it actively uses it to store and retrieve objects rather than the cache being transparently positioned between the application and its data sources.
- Always consider ways to reduce the number of possible cache keys. The fewer cache keys possible, the better for your cache efficency.
- The first header you need to become familiar with is **Cache-Control**. `Cache-Control : no-cache, no-store, max-age=0, must-revalidate`
- **private** option in Cache-Control indicates that the result is specific to the user who request it and the response cannot be served to any other user.
- Another important header is **Vary**. The purpose of that header is to tell caches that you may need to generate multiple variations of response based on some HTTP request header. `Vary: Accept-Encoding`
- There are four main types of HTTP Caches: browser cache, caching proxies, reverse proxies, and CDN.
- The availability of different routing methods may depend on which message broker you decide to use, but they usually support the following routing methods: direct worker queue, publish/subscribe, and custom routing rules
- Direct worker queue routing model is well suited for the distribution of time-consuming tasks across multiple worker machines. It is best if consumers are stateless and uniform; then replacement of failed nodes becomes as easy as adding a new worker node.
- A good example of pub/sub routing model is to publish a message for every purchase. Your e-commerce application could publish a message to a topic each time a purchase is confirmed. Then you could create multiple consumers performing different actions whenever a purchase message is published.
- Logging and alerting are good examples of custom routing based on pattern matching.
- Some of the common difficulties and pitfalls you may encounter when working with message queues and asynchronous processing includes no message ordering, message requeuing, race conditions, and increased complexity.
- The first rule of thumb when creating indexes on a data set is that the higher the cardinality, the better the index performance.
- The second rule of thumb when creating indexes is the equal distribution leads to better index performance.
- Data should be stored in a way that is optimized for specific access patterns and that indexes are the primary tool for making search scalable.
- When managing a project, you have three “levers” allowing you to balance the project: scope, cost, and time.
- Anytime you increase or decrease the scope, cost, or deadline, the remaining two variables need to be adjusted to reach a balance

### Performance vs Scalability

- A service is **scalable** if it results in increased **performance** in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow
- Another way to look at performance vs scalability
  - If you have a **performance** problem, your system is slow for a single user.
  - If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.
- Is achieving good scalability possible? Absolutely, but only if we architect and engineer our systems to take scalability into account. For the systems we build we must carefully inspect along which axis we expect the system to grow, where redundancy is required, and how one should handle heterogeneity in this system, and make sure that architects are aware of which tools they can use for under which conditions, and what the common pitfalls are.
- **Latency** is the time to perform some action or to produce some result. Latency is measured in units of time -- hours, minutes, seconds, nanoseconds or clock periods.
- **Throughput** is the number of such actions or results per unit of time. This is measured in units of whatever is being produced (cars, motorcycles, I/O samples, memory words, iterations) per unit of time
- An assembly line is manufacturing cars. It takes eight hours to manufacture a car and that the factory produces one hundred and twenty cars per day.
- The latency is: 8 hours.
- The throughput is: 120 cars / day or 5 cars / hour
- Generally you should aim for **maximal throughput** with **acceptable latency**.

### CAP Theorem

- CAP Theorem states that, in a distributed system, you can only have two out of the following three guarantees across a write/read pair: Consistency, Availability, and Partition Tolerance - one of them must be sacrificed.
- More informally, the CAP theorem tells us that we can't build a database that both responds to every request and returns the results that you would expect every time
- **Consistency**:
  - Every read receives the most recent write or an error.
  - A read is guaranteed to return the most recent write for a given client.
  - will all executions of reads and writes seen by all nodes be _atomic_ or _linearizably_ consistent?
- **Availability**:
  - Every request receives a response, without guarantee that it contains the most recent version of the information.
  - A non-failing node will return a reasonable response within a reasonable amount of time (no error or timeout)
  - will a request made to the data store always eventually complete?
- **Partition Tolerance**:
  - The system continues to operate despite arbitrary partitioning due to network failures.
  - the network is allowed to drop any messages.
- Given that networks aren’t completely reliable, you must tolerate partitions in a distributed system, period. According to the CAP theorem, this means we are left with two options: Consistency and Availability.
- **CP** - Wait for a response from the partitioned node which could result in a timeout error. The system can also choose to return an error, depending on the scenario you desire. Choose Consistency over Availability when your business requirements dictate atomic reads and writes.
- **AP** - Return the most recent version of the data you have, which could be stale. This system state will also accept writes that can be processed later when the partition is resolved. Choose Availability over Consistency when your business requirements allow for some flexibility around when the data in the system synchronizes. Availability is also a compelling option when the system needs to continue to function in spite of external errors (shopping carts, etc.)

### Remembrance Inc, analogy

- **Consistency**: Your customers, once they have updated information with you, will always get the most updated information when they call subsequently. No matter how quickly they call back
- **Availability**: Remembrance Inc will always be available for calls until any one of you (you or your wife) report to work.
- **Partition Tolerance**: Remembrance Inc will work even if there is a communication loss between you and your wife!
- You can have a run around clerk, who will update other’s notebook when one of your’s or your wife’s notebook is updated. This is how many NoSQL systems work, one node updates itself locally and a background process synchronizes all other nodes accordingly. The only problem is that you will lose consistency of some time. For eg., a customer’s call reaches your wife first and before the clerk has a chance to update your notebook, the customer calls back and it reaches you.

## Introduction to Architecting Systems for Scale

- Conventions
  - _green_ is an external request from an external client (an HTTP request from a browser)
  - _blue_ is your code running in some container.
  - _red_ is a piece of infrastructure (MySQL, Redis, RabbitMQ, etc)
- Horizontal scalability and redundancy are usually achieved via load balancing

![arch](/images/arch.png)

- Load balancing is the process of spreading requests across multiple resources according to some metric (random, round-robin, random with weighing for machine capacity, etc) and their current status (available for requests, not responding, elevated error rate, etc).
- Caching consists of: precalculating results (e.g. the number of visits from each referring domain for the previous day), pre-generating expensive indexes (e.g. suggested stories based on a user's click history), and storing copies of frequently accessed data in a faster backend e.g. Memcached.
- There are two primary approaches to caching: **application** caching and **database** caching (most systems rely heavily on both)

![caching](/images/caching.png)

- Application caching requires explicit integration in the application code itself
- The other side of the coin is database caching.

![dbcaching](/images/dbcaching.png)

- **Least Recently used** LRU works by evicting less commonly used data in preference of more frequently used data, and is almost always an appropriate caching strategy.
- CDNs take the burden of serving static media off of your application servers.

![cdn](/images/cdn.png)

- Message queues allow your web applications to quickly publish messages to the queue, and have other consumers processes perform the processing outside the scope and timeline of the client request.

![queues](/images/queues.png)

- Adding a map-reduce layer makes it possible to perform data and/or processing intensive operations in a reasonable amount of time.

![map-reduce](/images/map-reduce.png)

- Platform

![platform](/images/platform.png)

### Types of databases

- **Document-oriented** : generally semi-structured & stored in a JSON-like format.
  - Working with semi-structured data
  - Need a flexible schema
  - Examples are real-time feeds, live sport apps, web-based multiplayer games
- **Graph** : stored data in nodes/vertices and edges in the form of relationships
  - Maps
  - Social graphs
  - Recommendation engines
  - Storing genetic data
- **Key-value** : use a simple key-value method to store and quickly fetch the data
  - Caching
  - Implementing queue
  - Managing real-time data
- **Time series** : optimized for tracking & persisting time series data
  - Managing data in real-time & continually over a long period of time.
  - Managing data for running analytics and monitoring
- **Wide column** : primarily used to handle massive amounts of data
  - Use cases : Managing big data
