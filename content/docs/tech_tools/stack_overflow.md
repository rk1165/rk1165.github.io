### Joins in SQL

- **Inner join** produces only the set of records that match in both Table A and Table B
- **Full outer join** produces the set of all records in Table A and Table B, with matching records from both sides where available. If there is no match, the missing side will contain null
- To produce the set of records unique to Table A and Table B, we perform the same full outer join, then **exclude the records we don't want from both sides via a where clause**
- **Left outer join** produces a complete set of records from Table A, with the matching records (where available) in Table B. If there is no match, the right side will contain null
- To produce the set of records only in Table A, but not in Table B, we perform the same left outer join, then **exclude the records we don't want from the right side via a where clause**
- There's also a cartesian product or **cross join**. This joins "everything to everything".
- for returning just the duplicate rows : `SELECT fruit_name, color, count(\*) FROM fruits GROUP_BY fruit_name, color HAVING COUNT(\*) > 1;`

### Lazy Evaluation

```python
def someFunction(x, y):
  return x + 1

someFunction(3, 3 + 2)
```

- In the example above there is no point in evaluating `3 + 2`, because it's not used.
- The style of only evaluating what's needed is called **lazy evaluation**, while the opposite (evaluating immediately) is called **strict evaluation**
- For lazy evaluation to be efficient, it needs to use _memoization_
- With lazy evaluation, we get short circuiting for free.
- With lazy compilers, `if`s are not **primitives** (built into the compiler) but instead a part of the standard library of the language.
- Another cool side effect of lazy evaluation is the ability to work with infinite lists.
- Haskell is lazy by default. OCaml and Scheme, let you opt into lazy behaviour.
- Lazy evaluation is performant (and correct) only when correctly mixed with [referential transparency](https://programmingwords.com/home/referential-transparency).

### Rollback Journaling

- When we want to write to database we write it to a rollback journal called database-journal. The journal is then flushed to the disk space used for rollback. Finally we write the data to Disk using OS Cache as an intermediary.
- os cache locks???
- Write-Ahead Log

### Public key Cryptography

- Asymmetric Cryptography: public key and private key, public key other can encrypt the message but only the one with the private key can read the message.
- We are looking for these attributes:
- It is computationally easy to:
  - Generate a set of keys
  - Encrypt/Decrypt using these keys
- But it is also computationally infeasible to:
  - Determine the private key from the public key
  - Bruteforce the private key from the public key and bruteforce the ciphertext
- We want to turn messages into numbers.
- **RSA algorithm:**
  1. Choose 2 large prime numbers, p & q.
  2. Compute n = pq, z = (p-1)(q-1)
  3. Choose e (with e < z) such that e has no common factors with z.
  4. Choose d such that ed−1 is exactly divisible by z.
  5. The public key is (n, e). The private key is (n, d)
- To send an encrypted message, Bob computes _C = m<sup>e</sup> mod n_ for message m and key e. To decrypt the message, Alice computes _m = c<sup>d</sup> mod n_.

### Diffie-Hellman key exchange

- **Idea** Alice places the message in a box, puts a good lock on it, keeps the key, and sends the package to Bob. Bob then put _his own_ lock on the box and sends the package _back_ to Alice, locked twice. Alice removes her own lock using her key and then sends the box, still safe because it bears Bob's lock, back to Bob. Now Bob uses his key, opens the box, and gets the message!.
- **Implementation** Suppose the message to be sent from Alice to Bob is a secret number, call it _n_. Alice's "key" is an exponent, _a_, which she chooses, and then uses it to raise _n_ to. So the "locked box with the message" that Alice sends Bob is n<sup>a</sup>. Bob has his own "key", which is a number of his own choosing, _b_, that he uses as an exponent. He sends Alice the "box with two locks": n<sup>ab</sup>. Alice takes the ath root of n<sup>ab</sup>, which gives her n<sup>b</sup>, which she now sends back to Bob. Bob then takes bth root of n<sup>b</sup>, and he thus obtains the secret number n.

### Why is Diffie-Hellman presented as the solution for exchanging a key over an unsecure channel in security classes?

- Diffie Hellman does solve the problem of exchanging a key over an unsecured channel, but it only works if you know you are communicating with a trusted person.
- In most used communication protocols, we have three parts:
  - **Authentication** (public key): Am I talking to who I think I'm talking to? (Prevents MITM)
  - **Key Exchange** (DH): Can we share a secret session key that no one else can figure out?
  - **Message Encryption** (symmetric key): How can we quickly and securely encrypt a message with that secret key that will be repeatedly used throughout the session?
- Applied Cryptography by Schneier as a general reference, Practical Cryptography as a design reference, and Introduction to Modern Cryptography by J. Katz and Y. Lindell
- Every person has a button. For somebody there's a financial issue, for somebody it's a very appealing date, for somebody it's a family thing," Sarts says. "It's varied, but everybody has a button. The point is, what's openly available online is sufficient to know what that is."

### RSA

- a _pair_ of keys: a _public key_ and a _private key_, are jointly used for communicating secret messages. The public key can be published and known to all. Its use saves time. The private key is a secret that Bob keeps, allowing him to decipher coded messages from Alice (or from anyone who knows his public key). Bob publishes his public key, which is a large number. This number is obtained when he multiplies together two very large prime numbers, known only to him (they constitute his private key). When Alice wants to send Bob a secret message, she encrypts it using his known public key. But in order to decrypt the message, one would need to know Bob's private key, which is the two prime numbers he had used to create his publicly-known key. Supposedly, only Bob can do this.
- Encrypting and decrypting messages using the RSA algorithm is [a complicated mathematical procedure](http://www.nku.edu/~christensen/section%2026%20RSA.pdf) that relies on modular arithmetic and prime numbers.
- There are two good reasons for using modular arithmetic.
- It acts as a many-to-one function, in the sense that many numbers, when divided by a prime, will give the same remainder.
- It puts a cap on the size of numbers involved when using exponentials.

### Building Own Public Key Infrastructure

- PKI provides a mechanism for trusting identities online.
- The tools that enable this are digital certificates and public key cryptography.
- A certificate lets a website or service prove its identity.
- A certificate is a file with some identity information about the owner, a public key, and a signature from a certificate authority (CA).
- Each public key has an associated private key, which is kept securely under the certificate owner's control.
- The private key can be used to create digital signatures that can be verified by the associated public key.

### How to build good software

- **Operating principles for Good Software development**
  - Start as simple.
    - Building good software requires focus: starting with the simplest solution that could solve the problem.
  - Seek out problems and iterate;
    - The aim is to refine the output, sanding down rough edges until a good product emerges.
    - Small feedback loops allow for quick and easy correction but miss out on broader issues. Large feedback loops catch broader issues but are slow and expensive
  - Hire the best engineers you can.
- **Reusing Software Lets you Build Good Things quickly**
  - The point is to understand what the right systems to reuse are, how to customise them to fit your unique requirements, and fixing novel problems discovered along the way.
  - Software engineering is about building automated systems, and one of the first things that gets automated away is routine software engineering work.
- **Software is Limited by Complexity**
  - How useful a piece of software can be is usually limited by its complexity rather than the amount of resources invested in building it.
  - The bigger an application’s codebase, the more bugs are introduced whenever a new feature is built.
  - Eventually, the rate of work created from new bugs cancels out the rate of work done from feature development.
  - This is known as “technical debt” and is the main challenge in professional software development
  - Building good software involves alternating cycles of expanding and reducing complexity.
- **Software is about Developing Knowledge More than Writing Code**

### How does Turing Machine captures the essence of computing?

- In order to understand what things you can or can't do with an algorithm, you need to say what an algorithm is in the language of math.
- A **Turing Machine** is what we call Alan Turing's way of saying what an algorithm is in the language of math.
- Alonzo Church came up with a different way of saying what an algorithm is in the language of math. We call Alonzo Church's way the **lambda calculus**, and it turns out it says the same thing as Alan Turing's way, just phrased a little differently.
- Turing machines "capture the essence of computing" because they give us a way of carefully describing what it means to be an algorithm.

### How are incoming instant messages and notifications handled by app clients?

- So messaging clients and stuff like games make best use of web sockets. A normal socket in most basic form is just a directory in a file system that data can be written and read from. A web socket is this but over the internet.
- A server keeps track of all open connections (sockets) and receives input from those sockets by use of what are basically listeners. Then, the server can query through the open connections it use and send raw data (binary data, in Node a Buffer) back through the socket.
- Sockets are like a normal network request except they work from both directions. This is SUPER COOL when working with because you instantly have data transmutation and you ALWAYS no if there is connection error.
- Another way to do this is long pulling. It is definitely not as efficient or frankly easy to implement but basically the client makes a request and the server basically doesn’t fulfill the request until it wants to (ie a message is sent in a thread). Long pulling is a bad way of doing things.

### How do I know what data belongs to whom and make sure data can only be accessed by its owner in cloud storage technology?

- [CloudKit: Structured Storage for Mobile Applications](http://www.vldb.org/pvldb/vol11/p540-shraer.pdf)

### Does iteratively hashing a string make it harder to determine the original string?

- If we apply hashing `n` times we simply increase the brute force time by `n` times on the adversary side but also increased the time spent performing hashes `n` times on our side.
- In cryptographic systems we want an asymmetry in the amount of effort being spent. We'd like a small increase in our processing time to lead to a tremendous increase in the adversary's processing time. You can achieve this by using a hash function with a larger output.

### Can someone explain the relationship between hyper-threading, virtual cores, vCPUs, and physical cores, and sockets (sockets in the context of CPU, not TCP/UDP communication)?

- vCPUs and virtual cores are implementation specific. So there’s no single definition - you’ll need to refer to the documentation of whatever virtualization system you’re using.
- Talking about x86 servers here for the rest:
  - Sockets are the physical connectors in the motherboard that you plug a physical processor into. A “single socket” system will have one processor that you can take out and hold in your hand. A "two socket" system will have two. And so on.
  - Each physical socket/processor contains multiple isolated processing units (cores/cpus) that generally have their own operation units and L2 cache, and share L3 and high speed buses with other cores
  - Each of these cores, the actual cpus inside of the physical processor, can consist of (depending on configuration) multiple “logical” cores. Each runs a hyper-threaded context, but shares the same physical instruction hardware. One can run while the other is waiting on expensive fetch instructions, for example.
  - When hyperthreading is enabled, each lcore will look to the OS like an independent processing unit. It’s up to the programmer to understand that certain lcores are running in a hyperthreaded manner, and to modify their programs accordingly (if necessary - it usually isn’t) or just disable hyperthreading entirely
  - So socket -> physical cpu -> physical cores -> logical cores (hyperthreading). Virtualization software runs on top of this and exposes vCPUS that really just represent some timeslice of processing over one or more logical cores.

### How data storage works in "standard" technologies?

- **Static RAM (SRAM)** is the fastest memory technology. SRAM is used in CPU caches and other speed-critical storage needs. The fastest SRAMs run at full CPU frequency ... for a high-end CPU, that can be as high as 5GHz, meaning that SRAMs can read or write cache lines at a rate of five billion times per second. Standard cache line width is 64 bytes, for a grand total bandwidth of about 2.5 terabits per second.
- **Dynamic RAM** is the next fastest memory technology. Read and write frequencies easily run into the hundreds of megahertz or even gigahertz range in the latest technologies. The limiting factor of DRAM versus SRAM is that DRAM cells utilize capacitance, which causes the cells to require more time to read or write (since capacitors require time to charge or discharge). The benefit is that you can pack DRAM way more densely than you can pack SRAM, so it allows much more memory to be stored per unit area.
- **Non-volatile RAM**, like the _flash memory_ used in SD cards or thumb-drives, is even slower than DRAM because it utilizes an electrical effect inside each transistor that is capacitive but the charges must actually be moved through an insulating layer. Whereas filling a standard capacitor is like pouring water into a jug, filling the gates in flash memory is like trying to soak water up into a gigantic sponge that only has a tiny number of holes on its outer layer, making it mostly non-absorbent. So, it's just slow.
- **Hard Drives** The magnetic medium used in hard-drives is actually very fast to read and write but the fact that the hard-drive platters are physically spinning causes there to be unavoidable latency in reads/writes since the disk must physically rotate into position and the drive heads must also physically move into position before reading or writing can begin. The absence of latency is one of the factors that makes SSDs more responsive.

### What is a REST API?

- An API is just a set of rules to interface with a system.
- The main thing about REST is that it's _stateless_. That means that it doesn't maintain some sort of session or anything like that. So you shouldn't need to do a handshake or anything. If you send a request with the right headers then you will get a response (though those headers might include some sort of authentication)
- It returns only data
- SOAP is an actual protocol where you pass serialized objects in XML that have a specific format. REST is an architectural style of sorts, think of it as a guideline or a big idea.
- In SOAP, you’ll have an envelope object that contains a body with the data you want to transmit. This protocol allows multiple state changes to occur in a single transaction
- In HTTP REST: GET, PUT, PATCH, DELETE should be Idempotent only POST should not be Idempotent. By Idempotent it means "If you do the action 1, or 100, or any number of times (more than 0), the outcome will be the same"

### Restful API Design best practices

- PUT : Commonly used for updating (full updates). Always include the whole payload in the request. PUT is not meant to be used for partial updates.
- POST : `POST /employees` creates a new employee. The new URL is delivered back to the client in the `Location` Header.
- PATCH : Used for partial updates. `PATCH /employees/1` updates employee 1 with the fields contained in the payload, other fields are not changed.
- `GET /employees` returns a list of objects in the `data` field.
- The payload of PUT, POST, and PATCH requests should also contain the `data` field with the actual object.
- Move complexity or optional parameters to the query string.
- **2xx : Success** 200 - OK, 201 - Created
- **3xx : Redirect** 301 - Moved Permanently, 304 - Not modified
- **4xx : Client Error** 400 - Bad Request, 401 - Unauthorized, 403 - Forbidden, 404 - Not Found, 410 - Gone
- **5xx : Server Error** 500 - Internal Server Error
- It's better to provide links in your response which the client can follow.
- Design relationships appropriately : There are basically three common options to design relationships within an API: Links, Sideloading and Embedding.
- Use camelCase for attribute names.
- Two popular approaches to pagination mechanism:
  - Offset-based Pagination
  - Keyset-based Pagination aka Continuation Token aka Cursor (recommended)
- You can provide links for getting the next or previous page. `GET /employees?offset=20&limit=10`
- Consider API versioning.

### How can we recreate the Internet for performance and security?

- For security, you need to start by enumerating your security objectives.
- That said, one obvious main thing would be to prevent tampering with the intended routing of messages... BGPSec, DNSSec, maybe secure ARP, and secure Wi-Fi so everyone is assured they’re talking to the real base station.
- Presumably you’d build end-to-end encryption into the Transport Layer from the start and make it required. (Although this would mean you need better PKI, or else accept a lot more loss of availability in lieu of loss of security.) But what about message signing; that too? Do we also need to ensure authenticity? What about anonymity - Is it more “secure” to ensure anonymity or to ensure accountability through authenticity?
- At a certain point we have questions of policy and perhaps even philosophy. Those need to be answered before we can design a system that meets the requirements.
- Okay, now the performance question... first, a lot of the above necessarily sacrifices at least a little performance. Such is the nature of security...
- That aside, much of the Internet is already designed about as performant as we can expect under its other design criteria. (Decentralized architecture, packet switching, etc.)
- We could perhaps do better if the entire Internet was owned & operated by a single organization. But such an Internet would be virtually unrecognizable... (and also not technically an “inter”-net)
- We also would get different performance characteristics if everything was circuit switched instead of packet switched... but it’s not objectively better, just trade offs.
- Maybe more active forms of congestion control would help... but those are hard to do in a decentralized inter-net.
- Some improvements could perhaps be made at the link layer, but that is relatively easy to do, so I’m sure those will roll out as we develop them.
- The other layer that’s easy to fix is the application layer. HTTP is used a lot and has some inefficiencies... but HTTP 2 is fixing a lot of that. It’s pretty cool - check it out! :)
- Well, for modern web apps, your friend has a point. Since most modern web apps are... well, applications. That is, they’re basically full programs delivered through your browser.
- Using a technology designed to remotely deliver applications, rather than web pages, could indeed do that faster! And HTTP 2 doesn’t address this particular issue...
- However, there are serious security implications to simply running whatever code anyone delivers to you over the Internet! (Remember Flash??)
- That said... see Web Assembly.

### How do you choose what programming language to use?

- My approach nowadays is to build a project using Python first and optimize the bottlenecks via C, C++ or Fortran libraries, or writing extensions from the scratch using Cython/Pythran/Numba/Cupy etc. which lets you easily leverage OpenMP, GPU, SIMD vectorization etc. if needed - which can take a lot of effort to get it right in conventional languages. Python is the best glue language that exists today.

### How Netflix Works

- Store the original digital copy of the show or movie on to their AWS server
- Netflix works on thousands of devices, and each of them play a different format of video and sound files. Another set of AWS servers take this original film file, and convert it into hundreds of files, each meant to play the entire show or film on a particular type of device and a particular screen size or video quality. Even more of these files can be made with varying video qualities so that they are easier to load on a poor network connection. This is a process known as _transcoding_. A special piece of code is also added to these files to lock them with what is called digital rights management or DRM
- The Netflix app or website determines what particular device you are using to watch, and fetches the exact file for that show meant to specially play on your particular device, with a particular video quality based on how fast your internet is at that moment
- Servers across the world make a copy of it and store it so that the closest one to you delivers it at max quality and speed
- When you select a show, your Netflix app cherry picks which of these servers will it load the video from.

### Web Architecture 101

- horizontal scaling means that you scale by adding more machines into your pool of resources whereas “vertical” scaling means that you scale by adding more power (e.g., CPU, RAM) to an existing machine.
- Secondly, horizontal scaling allows you to minimally couple different parts of your application backend (web server, database, service X, etc.) by having each of them run on different servers
- **load balancers**. They’re the magic sauce that makes scaling horizontally possible. They route incoming requests to one of many application servers that are typically clones / mirror images of each other and send the response from the app server back to the client.
- Applications typically leverage caching services to save the results of expensive computations so that it’s possible to retrieve the results from the cache instead of recomputing them the next time they’re needed
- An application might cache results from a database query, calls to external services, HTML for a given URL, and many more
- The two most widespread caching server technologies are Redis and Memcache.
- While there are different architectures that enable asynchronous work to be done, the most ubiquitous is what I’ll call the “job queue” architecture. It consists of two components: a queue of “jobs” that need to be run and one or more job servers (often called “workers”) that run the jobs in the queue.
- Many if not most web apps support some sort of search feature where a user provides a text input (often called a “query”) and the app returns the most “relevant” results. The technology powering this functionality is typically referred to as “full-text search”, which leverages an inverted index to quickly look up documents that contain the query keywords.
- The most popular full-text search platform today is Elasticsearch though there are other options such as Sphinx or Apache Solr.
- Almost every app these days, once it reaches a certain scale, leverages a data pipeline to ensure that data can be collected, stored, and analyzed. A typical pipeline has three main stages:
  1. The app sends data, typically events about user interactions, to the data “firehose” which provides a streaming interface to ingest and process the data. AWS Kinesis and Kafka are the two most common technologies for this purpose
  2. The raw data as well as the final transformed/augmented data are saved to cloud storage
  3. The transformed/augmented data is often loaded into a data warehouse for analysis

### ZooKeeper

- It is commonly used in distributed systems to manage configuration information, naming services, distributed synchronization, quorum, and state.
- Distributed systems rely on ZooKeeper to implement consensus, leader election, and group management.
- It allows distributed processes to coordinate with each other through a shared hierarchical name space of data registers - called znodes.
- ZooKeeper in a cluster is known as ensemble.

### One-liners

- A connection pool is, at the most basic level, a database connection cache implementation.
- To begin with, python -m pip executes pip using the Python interpreter you specified as python. So /usr/bin/python3.7 -m pip means you are executing pip for your interpreter located at /usr/bin/python3.7
- FLUSHDB : clears currently active Database
- FLUSHALL : clears all the existing databases.
- dbsize returns the number of keys.
- How to use sadd with multiple elements in Redis using Python API? : r.sadd('a', 1, 2, 3) or r.sadd('a', \*set([3, 4]))
- r.sadd('a', \*[3, 4])
- Wireshark has a [plugin](https://www.wireshark.org/docs/wsdg_html_chunked/ChapterDissection.html) interface for adding support for arbitrary network protocols. Henry’s project this summer was to write a plugin for Async-RPC, which is an internally developed protocol that we use all over the place

### What do the terms "CPU bound" and "I/O bound" mean?

- A program is _CPU bound_ if it would go faster if the CPU were faster, i.e. it spends the majority of its time simply using the CPU (doing calculations). e.g. computing new digits of pi.
- A program is _I/O bound_ if it would go faster if the I/O subsystem was faster.
- matrix multiplication is CPU-bound on RAM and GPUs.
- Netowrking is the prototypical IO-bound example.
  - Even when we send a single byte of data, it still takes a large time to reach it's destination. Parallelizing small network requests like HTTP requests can offer a huge performance gains.

### Why are NoSQL database more scalable than SQL

- It's not about NoSQL vs SQL, it's about BASE vs ACID. Scalable has to be broken down into its constituents:
  - Read scaling = handle higher volumes of read operations
  - Write scaling = handle higher volumes of write operations
- ACID-compliant databases (like traditional RDBMS's) can scale reads. They are not inherently less efficient than NoSQL databases because the (possible) performance bottlenecks are introduced by things NoSQL (sometimes) lacks (like joins and where restrictions) which you can opt not to use. Clustered SQL RDBMS's can scale reads by introducing additional nodes in the cluster. There are constraints to how far read operations can be scaled, but these are imposed by the difficulty of scaling up writes as you introduce more nodes into the cluster.
- Write scaling is where things get hairy. There are various constraints imposed by the ACID principle which you do not see in eventually-consistent (BASE) architectures:
- Atomicity means that transactions must complete or fail as a whole, so a lot of bookkeeping must be done behind the scenes to guarantee this.
- Consistency constraints mean that all nodes in the cluster must be identical. If you write to one node, this write must be copied to all other nodes before returning a response to the client. This makes a traditional RDBMS cluster hard to scale.
- Durability constraints mean that in order to never lose a write you must ensure that before a response is returned to the client, the write has been flushed to disk.
- To scale up write operations or the number of nodes in a cluster beyond a certain point you have to be able to relax some of the ACID requirements:
  - Dropping Atomicity lets you shorten the duration for which tables (sets of data) are locked. Example: MongoDB, CouchDB.
  - Dropping Consistency lets you scale up writes across cluster nodes. Examples: riak, cassandra.
  - Dropping Durability lets you respond to write commands without flushing to disk. Examples: memcache, redis.
- NoSQL databases typically follow the BASE model instead of the ACID model. They give up the A, C and/or D requirements, and in return they improve scalability. Some, like Cassandra, let you opt into ACID's guarantees when you need them. However, not all NoSQL databases are more scalable all the time.
- The SQL API lacks a mechanism to describe queries where ACID's requirements are relaxed. This is why the BASE databases are all NoSQL.
- Personal note: one final point I'd like to make is that most cases where NoSQL is currently being used to improve performance, a solution would be possible on a proper RDBMS by using a correctly normalized schema with proper indexes. As proven by this very site (powered by MS SQL Server) RDBMS's can scale to high workloads, if you use them appropriately. People who don't understand how to optimize RDBMS's should stay away from NoSQL, because they don't understand what risks they are taking with their data.
- Update (2019-09-17):
  - The landscape of databases has evolved since posting this answer. While there is still the dichotomy between the RDBMS ACID world and the NoSQL BASE world, the line has become fuzzier. The NoSQL databases have been adding features from the RDBMS world like SQL API's and transaction support. There are now even databases which promise SQL, ACID and write scaling, like Google Cloud Spanner, YugabyteDB or CockroachDB. Typically the devil is in the details, but for most purposes these are "ACID enough". For a deeper dive into database technology and how it has evolved you can take a look at [this slide deck](https://docs.google.com/presentation/d/1ovHHGT-H-wfGlRqYB1V-1aydSz3O4W9xR7kMQEIJ7CI/edit?usp=sharing) (the slide notes have the accompanying explanation).
