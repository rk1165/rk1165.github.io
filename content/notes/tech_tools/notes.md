+++
title = 'Tools and Techonologies'
date = 2024-02-11

+++

### How debuggers work

- **ptrace** : It's a versatile and rather complex tool that allows one process to control the execution of another and to peek and poke at its innards
- Peek and poke are well-known system programming jargon for directly reading and writing memory contents.
- ebp (base pointer, also known as frame pointer in other architectures) is used as a convenient anchor relative to which all function arguments and locals can be found. Arguments are above ebp in the stack (hence the positive offset when accessing them), while locals are below ebp in the stack.
- leaf functions (functions that call no other functions)
- An lvalue (locator value) represents an object that occupies some identifiable location in memory (i.e. has an address).
- rvalues are defined by exclusion, by saying that every expression is either an lvalue or an rvalue. Therefore, from the above definition of lvalue, an rvalue is an expression that does not represent an object occupying some identifiable location in memory.
- `strace` capture every single system call that gets called when executing a program.
- System calls are the interface between userspace programs and the kernel.
- To see the library calls instead of the system calls, use `ltrace`
- `ftrace` is a Linux kernel feature that lets you trace Linux kernel function calls.

### OTEL

- OpenTelemetry can be further be divided into other logical components that comprise the typical OTel solution. They are APIs, SDKs, and the Collector.
- APIs are in charge of gathering the telemetry and all the data that is part of it and the SDKs are what get this data out of the current observed process to another entity for analysis
- They allow us to decouple what is observed (API) to how it is handled (SDK)
- The collector’s entire job can be broken down into three different stages:
  - **Receive** telemetry data
  - **Process** telemetry data
  - **Export** telemetry data
- Telemetry and observability includes three distinct signals: **Traces**, **metrics**, and **logs**
- When we talk about "instrumentation", we are referring to the act of collecting trace data.
- A trace includes one or more spans, which are the instances of a particular operation.

### Random

- Both Kafka and SQS support concurrent message consumption but Kafka consumer groups are something that SQS doesn't support. They allow the same message to be consumed by multiple consumers (in different consumer groups), whereas you would need SQS and SNS to accomplish this in AWS. Kafka consumer groups also allow multiple consumers in the same consumer group to consume messages from the same topic (but different partition) concurrently.
- The scalability of a stable system is governed by Little's Law, which relates latency, concurrency, and throughput. If each request has a duration (or latency) of d, and we can perform N tasks concurrently, then throughput T is given by `T = N / d`
- when phone is turned on Sim card passes id number and authentication key to cell provider, provider then generates a random number and uses the key to spit out a response number and that random number is sent back to phone and the same calculation is done to generate a random number. If the two numbers match then connection is made
- Registry is a db on hard drives that stores important settings for windows and other programs.
- `regedit` in the run window
- Can proxy and reverse proxy be used at the same time : yes, in microservices this is called _Service Mesh_ or _Sidecar proxy_ like **linkerd**, **envoy**.
- Can I use proxy instead of VPN for anonymity: yeah but technically VPN is more secure than proxy. VPN can only see domain. Some proxies can see every thing especially if they are TLS terminating proxies.
- Every Windows OS has Active Directory which manages the authentication for you. This is built on a default authentication system called kerberos.
- **kerberos** : only symmetric encryption. authentication & authorization system
- Server memory used in TCP. Sent a package but doesn't reply (DoS attacks). Do these million of times your server will stack up memory and die eventually
- DNS uses UDP.
- HTTP/2
  - Compression :
  - Multiplexing : multiple tcp packets as a one request
  - Server Push :
  - SPDY :
  - secure by default
  - Protocol Negotiation during TLS (NPN/ALPN) : Application Layer Protocol Negotiation
- HTTP/2 over QUIC (HTTP/3)
  - Replaces TCP with QUIC (UDP with Congestion control)
  - All HTTP/2 features
- Bandwidth based DoS : server can process 50 mb/s and client upload 100mb/s. Another scenario is client has 10mb/s bandwidth and the server 1Gb/s. Then this attack can be carried in a "distributed" manner. That is DDoS.
- There are two types of cursor : client side and server side
- Client side cursors are when you query, essentially the client will allocate memory for the entire thing. All the results are delivered over the network to the client
- Server side cursors are created but left there, then it is asked give me one, give me one. The client fetches them incrementally
- Client side cursor require a lot of network bandwidth as we need to pull a lot of rows.
- `ngrok tcp 8080 --authtoken $token` : to expose web socket server
- `curl  -tcp-fastopen google.com`
- `sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination $IP_ADDR_OF_PI:8080`
- **Spatial Cache** : locality based cache. Let me pull nearby located data. e.g tools are downstairs and we are fitting the furniture upstairs so we would want to get all the tools once we go downstairs.
- **Temporal Cache** : LRU cache example. if you use a data lot keep it freshen in the case. L1, L2 and L3 caches are temporal cache.
- **Distributed Cache** : we need to keep cache in sync with datastore. Most popular is Redis.
- Two types of caching here :
  - **write-through** - when writing, first update the cache and synchronously block the client to write to the disk.
  - **write-back** - write to the cache and trust that cache wil asynchronously write to the datasource
- understanding of the different models of releasing software, much less of a complete system architecture, failure modes, consistency models, reliability estimates, performance limits, etc.
- Load Shedding is a technique where you drop low-priority requests to make sure that critical requests get through.
