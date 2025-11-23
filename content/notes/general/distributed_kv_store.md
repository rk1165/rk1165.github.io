+++
title = 'Distributed Key-Value Store'
date = 2025-11-21

+++

Designing a distributed key-value store (modeled after Amazon's Dynamo or Apache Cassandra) is one of the most comprehensive system design challenges. It requires you to move away from a "Master-Slave" architecture to a "Peer-to-Peer" architecture to achieve infinite horizontal scale.

Here is the step-by-step design, focusing on how to solve the problems of data distribution, consistency, and failure detection.

---

### 1. High-Level Requirements & Architecture

We want a system that is **Always Writeable** (highly available) and **Partition Tolerant**. We accept that data might briefly be inconsistent across nodes (Eventual Consistency).

- **API:** `get(key)`, `put(key, value)`
- **Architecture:** A ring of identical nodes (Peer-to-Peer). There is no single "Master" node to bottleneck the system. Any node can handle a request.

---

### 2. Data Partitioning (Consistent Hashing)

How do we know which node holds Key "A"?
If we use simple modular hashing (`hash(key) % NumNodes`), adding or removing a single server would require reshuffling almost ALL keys. This causes massive downtime.

**The Solution: Consistent Hashing Ring**

1.  **The Ring:** Imagine a circle representing the hash space (e.g., 0 to $2^{64} - 1$).
2.  **Server Placement:** Hash the server IP addresses to place them on the ring.
3.  **Key Placement:** Hash the Key to place it on the ring.
4.  **The Rule:** To find where a key lives, go clockwise on the ring from the Key's position until you find the first Server.

**The Refinement: Virtual Nodes (VNodes)**
If one server is more powerful than others, or if servers are unevenly spaced, the load is unbalanced.

- **Strategy:** Instead of placing a server on the ring once, place it 100 times (Virtual Nodes). This ensures a statistically even distribution of data across the physical hardware.

---

### 3. Data Replication (Durability)

If the node holding "Key A" crashes, the data is lost. We need redundancy.

**The Strategy:**
When a key lands on a node (the "Coordinator"), that node replicates the data to the next $N-1$ nodes on the ring (skipping VNodes that belong to the same physical machine).

- **$N$ (Replication Factor):** Usually 3. This means data lives on 3 distinct physical machines.

---

### 4. Consistency (Tunable Quorums)

Since we have 3 copies, what happens if we update one, but the other two are lagging? We use **Tunable Consistency** ($N, R, W$).

- **$N$:** Number of replicas (e.g., 3).
- **$W$:** Write Quorum. How many nodes must acknowledge the write for it to be successful?
- **$R$:** Read Quorum. How many nodes must we read from to be sure we have the latest data?

**The Formula:** $R + W > N$
If this equation holds, you are guaranteed strong consistency (the read set and write set overlap).

- **Latency vs. Accuracy:** A common configuration is $N=3, W=1, R=1$ for extreme speed (but risk of stale data), or $N=3, W=2, R=2$ for strong consistency.

---

### 5. Conflict Resolution (Vector Clocks)

If two users update the same key at the same time on different nodes (network partition), you get a conflict. "Last Write Wins" (using timestamps) is dangerous because clocks drift.

**The Solution: Vector Clocks**
We attach metadata to every piece of data: `[NodeA: 1, NodeB: 2]`.

1.  **Causality:** If Version Y contains all counter values of Version X, Y is newer. Overwrite X.
2.  **Conflict:** If Version Y has `NodeA: 2` and Version Z has `NodeB: 2` (neither implies the other), we have a "Concurrent Write."
3.  **Resolution:** The database keeps _both_ versions and returns them to the client on the next read. The **Client** must resolve the conflict (merge the data) and write it back.

---

### 6. Handling Temporary Failures (Hinted Handoff)

What if Node A is supposed to take a write, but it is temporarily down?

**The Strategy:**

1.  The request is sent to Node B (the neighbor).
2.  Node B saves the data in a separate buffer with a "hint": _"This belongs to Node A."_
3.  Node B repeatedly pings Node A.
4.  When Node A comes back online, Node B pushes the data to A and deletes its local hint.
    This ensures writes never fail even if the target node is down.

---

### 7. Handling Permanent Inconsistencies (Merkle Trees)

[Image of merkle tree diagram]

If a node is down for days, "Hinted Handoff" buffers might overflow. The node comes back with very stale data. We need to sync it with the others efficiently without sending the entire database over the network.

**The Solution: Merkle Trees (Hash Trees)**

1.  Each node maintains a Merkle Tree of its data (a tree of hashes).
2.  Nodes compare the **Root Hash** of their trees.
3.  If they match, the data is identical. Stop.
4.  If they differ, compare the children.
5.  Recurse down until you find the specific "buckets" (leaves) that differ. Sync only those buckets.

---

### 8. Step-by-Step Request Flow (Summary)

**The Write Path (`PUT`):**

1.  Client sends request to any node (Load Balancer).
2.  That node acts as **Coordinator**.
3.  Coordinator hashes the key, finds the "Preference List" (the $N$ replicas).
4.  Coordinator sends data to all $N$ replicas.
5.  Waits for $W$ acknowledgments.
6.  Returns "Success" to client.

**The Read Path (`GET`):**

1.  Client connects to Coordinator.
2.  Coordinator requests data from all $N$ replicas.
3.  Waits for $R$ responses.
4.  **Read Repair:** If the replicas return different versions of the data, the Coordinator uses Vector Clocks to determine the winner, returns the latest version to the client, and silently updates the outdated replicas in the background.

### A Next Step for You

We have covered how data moves _between_ nodes. But how the data is stored _on the disk_ of a single node is equally critical for performance.

Standard B-Trees (used in SQL) are often too slow for the massive write throughput of distributed KV stores. Most use **LSM Trees (Log-Structured Merge-Trees)**.

Would you like me to explain how an **LSM Tree** works and why it makes writes incredibly fast compared to traditional databases?
