+++
title = 'Questions from Stack Overflow'
date = 2024-02-11

+++

### Joins in SQL

- **Inner join** produces only the set of records that match in both Table A and Table B
- **Full outer join** produces the set of all records in Table A and Table B, with matching records from both sides where available. If there is no match, the missing side will contain null
- To produce the set of records unique to Table A and Table B, we perform the same full outer join, then **exclude the records we don't want from both sides via a where clause**
- **Left outer join** produces a complete set of records from Table A, with the matching records (where available) in Table B. If there is no match, the right side will contain null
- To produce the set of records only in Table A, but not in Table B, we perform the same left outer join, then **exclude the records we don't want from the right side via a where clause**
- There's also a cartesian product or **cross join**. This joins "everything to everything".
- For returning just the duplicate rows : `SELECT fruit_name, color, count(*) FROM fruits GROUP_BY fruit_name, color HAVING COUNT(*) > 1;`

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
- Lazy evaluation is performant (and correct) only when correctly mixed with referential transparency - calling function multiple times giving the same result.

### Diffie-Hellman key exchange

- **Idea** Alice places the message in a box, puts a good lock on it, keeps the key, and sends the package to Bob. Bob then put _his own_ lock on the box and sends the package _back_ to Alice, locked twice. Alice removes her own lock using her key and then sends the box, still safe because it bears Bob's lock, back to Bob. Now Bob uses his key, opens the box, and gets the message!.
- **Implementation** Suppose the message to be sent from Alice to Bob is a secret number, call it _n_. Alice's "key" is an exponent, _a_, which she chooses, and then uses it to raise _n_ to. So the "locked box with the message" that Alice sends Bob is n<sup>a</sup>. Bob has his own "key", which is a number of his own choosing, _b_, that he uses as an exponent. He sends Alice the "box with two locks": n<sup>ab</sup>. Alice takes the ath root of n<sup>ab</sup>, which gives her n<sup>b</sup>, which she now sends back to Bob. Bob then takes bth root of n<sup>b</sup>, and he thus obtains the secret number n.

### Why is Diffie-Hellman presented as the solution for exchanging a key over an unsecure channel in security classes?

- Diffie Hellman does solve the problem of exchanging a key over an unsecured channel, but it only works if you know you are communicating with a trusted person.
- In most used communication protocols, we have three parts:
  - **Authentication** (public key): Am I talking to who I think I'm talking to? (Prevents MITM)
  - **Key Exchange** (DH): Can we share a secret session key that no one else can figure out?
  - **Message Encryption** (symmetric key): How can we quickly and securely encrypt a message with that secret key that will be repeatedly used throughout the session?

### RSA

- a _pair_ of keys: a _public key_ and a _private key_, are jointly used for communicating secret messages. The public key can be published and known to all. Its use saves time. The private key is a secret that Bob keeps, allowing him to decipher coded messages from Alice (or from anyone who knows his public key). Bob publishes his public key, which is a large number. This number is obtained when he multiplies together two very large prime numbers, known only to him (they constitute his private key). When Alice wants to send Bob a secret message, she encrypts it using his known public key. But in order to decrypt the message, one would need to know Bob's private key, which is the two prime numbers he had used to create his publicly-known key. Supposedly, only Bob can do this.
- Encrypting and decrypting messages using the RSA algorithm is a complicated mathematical procedure that relies on modular arithmetic and prime numbers.
- There are two good reasons for using modular arithmetic.
- It acts as a many-to-one function, in the sense that many numbers, when divided by a prime, will give the same remainder.
- It puts a cap on the size of numbers involved when using exponentials.

### How does Turing Machine captures the essence of computing?

- In order to understand what things you can or can't do with an algorithm, you need to say what an algorithm is in the language of math.
- A **Turing Machine** is what we call Alan Turing's way of saying what an algorithm is in the language of math.
- Alonzo Church came up with a different way of saying what an algorithm is in the language of math. We call Alonzo Church's way the **lambda calculus**, and it turns out it says the same thing as Alan Turing's way, just phrased a little differently.
- Turing machines "capture the essence of computing" because they give us a way of carefully describing what it means to be an algorithm.

### How data storage works in "standard" technologies?

- **Static RAM (SRAM)** is the fastest memory technology. SRAM is used in CPU caches and other speed-critical storage needs. The fastest SRAMs run at full CPU frequency ... for a high-end CPU, that can be as high as 5GHz, meaning that SRAMs can read or write cache lines at a rate of five billion times per second. Standard cache line width is 64 bytes, for a grand total bandwidth of about 2.5 terabits per second.
- **Dynamic RAM** is the next fastest memory technology. Read and write frequencies easily run into the hundreds of megahertz or even gigahertz range in the latest technologies. The limiting factor of DRAM versus SRAM is that DRAM cells utilize capacitance, which causes the cells to require more time to read or write (since capacitors require time to charge or discharge). The benefit is that you can pack DRAM way more densely than you can pack SRAM, so it allows much more memory to be stored per unit area.
- **Non-volatile RAM**, like the _flash memory_ used in SD cards or thumb-drives, is even slower than DRAM because it utilizes an electrical effect inside each transistor that is capacitive but the charges must actually be moved through an insulating layer. Whereas filling a standard capacitor is like pouring water into a jug, filling the gates in flash memory is like trying to soak water up into a gigantic sponge that only has a tiny number of holes on its outer layer, making it mostly non-absorbent. So, it's just slow.
- **Hard Drives** The magnetic medium used in hard-drives is actually very fast to read and write but the fact that the hard-drive platters are physically spinning causes there to be unavoidable latency in reads/writes since the disk must physically rotate into position and the drive heads must also physically move into position before reading or writing can begin. The absence of latency is one of the factors that makes SSDs more responsive.

### Restful API Design best practices

- PUT : Commonly used for updating (full updates). Always include the whole payload in the request. PUT is not meant to be used for partial updates.
- POST : `POST /employees` creates a new employee. The new URL is delivered back to the client in the `Location` Header.
- PATCH : Used for partial updates. `PATCH /employees/1` updates employee 1 with the fields contained in the payload, other fields are not changed.
- `GET /employees` returns a list of objects in the `data` field.
- The payload of PUT, POST, and PATCH requests should also contain the `data` field with the actual object.
- Move complexity or optional parameters to the query string.
- It's better to provide links in your response which the client can follow.
- Design relationships appropriately : There are basically three common options to design relationships within an API: Links, Sideloading and Embedding.
- Two popular approaches to pagination mechanism:
  - Offset-based Pagination
  - Keyset-based Pagination aka Continuation Token aka Cursor (recommended)
- You can provide links for getting the next or previous page. `GET /employees?offset=20&limit=10`

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
  - The landscape of databases has evolved since posting this answer. While there is still the dichotomy between the RDBMS ACID world and the NoSQL BASE world, the line has become fuzzier. The NoSQL databases have been adding features from the RDBMS world like SQL API's and transaction support. There are now even databases which promise SQL, ACID and write scaling, like Google Cloud Spanner, YugabyteDB or CockroachDB. Typically the devil is in the details, but for most purposes these are "ACID enough". For a deeper dive into database technology and how it has evolved you can take a look at [this slide deck](https://docs.google.com/presentation/d/1ovHHGT-H-wfGlRqYB1V-1aydSz3O4W9xR7kMQEIJ7CI/edit?usp=sharing).

- KeySpaceNotifications. This feature allows you to listen to key-related events such as when keys are set, deleted, or expired. These notifications enable applications to trigger real-time business logic based on Redis events