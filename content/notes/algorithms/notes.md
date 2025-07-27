+++
title = 'Notes on Data Structures'
date = 2024-02-11

+++

### Stack

- Expression evaluation and syntax parsing: Many compilers use a stack for parsing the syntax of expressions, program blocks etc. before translating into low level code. Most programming languages are context-free languages, allowing them to be parsed with stack based machines.
- **Backtracking**: With the help of stacks, we remember the point where we have reached. This is done by pushing that point into the stack. In case we end up on the wrong path, we can pop the last point from the stack and thus return to the last point and continue our quest to find the right path. The prototypical example of a backtracking algorithm is depth-first search, which finds all vertices of a graph that can be reached from a starting vertex.
- Compile time memory management
- Efficient algorithms:
  - **Graham scan**, an algorithm for the convex hull of a 2-D system of points. A convex hull of a subset of they put is maintained in a stack, which is used to find and remove concavities in the boundary when a new point is added to the hull.
- All nearest smaller values, the problem of finding, for each number in an array, the closest preceding number that is smaller than it. One algorithm for this problem uses a stack to maintain a collection of candidates for the nearest smaller value. For each position in the array, the stack is popped until a smaller value is found on its top, and then the value in the new position is pushed onto the stack.

### Heap

- **Heapsort**: One of the best sorting methods being in-place and with no quadratic worst-case scenarios.
- **Selection algorithms**: A heap allows access to the min or max element in constant time, and other selections (such as median or kth-element) can be done in sub-linear time on data that is in a heap.
- **Graph Algorithms**: By using heaps as internal traversal data structures, run time will be reduced by polynomial order. Examples of such problems are Prim’s minimal-spanning-tree algorithm and Dijkstra’s shortest-path algorithm.
- **Order-statistics**: The Heap data structure can be used to efficiently find the k-th smallest (or largest) element in an array.
- The Java platform provides a binary heap implementation with the class `java.util.PriorityQueue` in the java `Collections` framework
- Python has a `heapq` module that implements a priority queue using a binary heap.

### Priority Queues

- A binary tree is _heap-ordered_ if the key in each node is larger than or equal to the keys in that node’s two children (if any)
- Moving up from any node, we get a nondecreasing sequence of keys; moving down from any node, we get a nonincreasing sequence of keys.
- The largest key in a heap-ordered binary tree is found at the root.
- A _binary heap_ is a collection of keys arranged in a complete heap-ordered binary tree, represented in level order in an array (not using the first entry.)
- In a heap, the parent of the node in position _k_ is in position ⎣*k/2*⎦.
- binary min-heap (where smaller values correspond to higher priorities). every node is smaller than its descendents
- binary max-heap (where larger values correspond to higher priorities). every node is larger than its descendents
- The two children of the node in position _k_ are in positions _2k_ and _2k+1_.
- To move _up_ the tree from a[k] we set k to k/2; to move _down_ the tree we set k to 2\*k or 2\*k+1
- The height of a complete binary tree of size N is ⎣lg N⎦
- **Bottom-up reheapify (swim)**. If the heap order is violated because a node's key become _larger_ than that node's parent's key, then we can progress toward fixing the violation by exchanging the node with its parent.
- **Top-down reheapify (sink)**. If the heap order is violated because a node’s key becomes _smaller_ than one or both of that node’s children’s keys, then we can make progress toward fixing the violation by exchanging the node with the _larger_ of its two children
- In an N-Key PQ, the heap algorithms require no more than 1 + lgN compares for _insert_ and no more than 2lgN compares for _remove the maximum_.
- When and Where is a PQ used?
  - Used in certain implementations of Dijkstra's Shortest Path algorithm.
  - Used in Huffman coding
  - Best First Search, algorithms such as A\* use PQs to continuously grab the next most promising node.
  - Used by MST algorithms.
- Turning MinPQ into Max PQ
  - Let x, y be numbers in the PQ. For a min PQ, if x <= y then x comes out of the PQ before y, so the negation of this is if x >= y then y comes out before x.
  - An alternative method for numbers is to negate the numbers as you insert them into the PQ and negate them again when they are take out. This has the same effect as negating the comparator.
  - For strings we can use **nlex** as a comparator which negates **lex** which provides natural ordering for strings.

### Hash Tables

- We need a different hash function for each key type that we use.
- **Positive integers** The most commonly used method for hashing integers is called _modular hashing_: we choose the array size _M_ to be prime and, for any positive integer key _k_, compute the remainder when dividing _k_ by _M_.
- We need to use a table size that is a prime (in particular, _not_ a power of 2), if we want to use modular hashing to disperse them
- **Floating-point numbers** If the keys are real numbers between 0 and 1, we might just multiply by M and round off to the nearest integer to get an index between 0 and M-1. This approach is defective because it gives more weight to the most significant bits of the keys. One way to address this situation is to use modular hashing on the the binary representation of the key.
- **Strings** Modular hashing works for long keys such as strings, too: we simply treat them as huge integers. A classic algorithm _Horner's method_ gets the job done with N multiplications, additions and mod ops.
- If computing the hash code is expensive, it may be worthwhile to _cache the hash_ for each key.
- Poor perfomance due to hash function not taking all of the bits of keys into account.

#### Hashing with linear probing

- Another approach to implementing hashing is to store N key-value pairs in a hash table of size M > N, relying on empty entries in the table to help with collision resolution.
- The simplest open-addressing method is called _linear probing_: where there is a collision, then we just check the next entry in the table.
- a hash function that maps each item into a unique slot is referred to as a **perfect hash function**
- One way to always have a perfect hash function is to increase the size of the hash table so that each possible value in the item range can be accommodated. It is not feasible when possible items are large.
- Our goal is to create a hash function that minimizes the number of collisions, is easy to compute, and evenly distributes the items in the hash table
- The **folding method** for constructing hash functions begins by dividing the item into equal-size pieces (the last piece may not be of equal size). These pieces are then added together to give the resulting hash value. Some folding methods go one step further and reverse every other piece before the addition
- Another numerical technique for constructing a hash function is called the **mid-square method**. We first square the item, and then extract some portion of the resulting digits

#### Collision Resolution

- **open addressing** it tries to find the next open slot or address in the hash table. we may need to go back to the first slot (circularly) to cover the entire hash table.
- By systematically visiting each slot one at a time, we are performing an open addressing technique called **linear probing**
- A disadvantage to linear probing is the tendency for **clustering**; items become clustered in the table.
- One way to deal with clustering is to extend the linear probing technique so that instead of looking sequentially for the next open slot, we skip slots, thereby more evenly distributing the items that have caused collisions.
- The general name for this process of looking for another slot after a collision is **rehashing**
- It is often suggested that the table size be a prime number
- A variation of the linear probing idea is called **quadratic probing**. Instead of using a constant “skip” value, we use a rehash function that increments the hash value by 1, 3, 5, 7, 9, and so on. This means that if the first hash value is h, the successive values are h+1, h+4, h+9, h+16, and so on. In other words, quadratic probing uses a skip consisting of successive perfect squares
- An alternative method for handling the collision problem is to allow each slot to hold a reference to a collection (or chain) of items. **Chaining** allows many items to exist at the same location in the hash table.
- The **shell sort**, sometimes called the “diminishing increment sort,” improves on the insertion sort by breaking the original list into a number of smaller sublists, each of which is sorted using an insertion sort.
- . Instead of breaking the list into sublists of contiguous items, the shell sort uses an increment i, sometimes called the **gap**, to create a sublist by choosing all items that are i items apart. Shell sort is **unstable** - it may change the relative order of elements with equal values.

### 2-3 search trees

- A 2-3 _search tree_ is a tree that is either empty or
  - A _2-node_, with one key (and associated value) and two links, a left link to a 2-3 search tree with smaller keys, ans a right link to a 2-3 search tree with larger keys.
  - A _3-node_, with two keys (and associated values) and _three_ links, a left link to a 2-3 search tree with smaller keys, a middle link to a 2-3 search tree with keys between the node's keys, and right link to a 2-3 search tree with larger keys.
- A _perfectly balanced_ 2-3 search tree is one whose null links are all the same distance from the root.
- Search and insert operations in a 2-3 tree with N keys are guaranteed to visit at most lg N nodes.

### Trie

- a trie, also called _digital tree_ or _prefix tree_, is a kind of search tree — an ordered tree data structure used to store a dynamic set or associative array where the keys are usually strings.
- The trie is a tree of nodes which supports Find and Insert operations.
- `Find` returns the value for a key string, and `Insert` inserts a string (the key) and a value into the trie. Both insert and find run in O(n) time, where n is the length of the key.
- Ways to implement a trie:
  - Use an array of references
    - Each array element represent one letter of the alphabet.
    - Each array reference will point to a sub-trie that corresponds to strings that starts with the corresponding letter.
  - Use a binary tree.
    - This implementation is called a _bitwise_ trie.
- If the trie is dense and/or alphabet Sigma is small, use an array to store the labels.
- The binary tree implementation uses the alphabet {0, 1}. The keys are read as a sequence of bits.
- Height of a trie is the length of the longest string.
- Looking up data in trie is in the worst case = O(m), where m = length of the key.
- Unlike a hash table, a trie can provide alphabetical ordering of the entries by key

### Disjoint Set Union / Union Find

- Union Find is a data structure that keeps track of elements which are split into one or more disjoint sets. It has two primary operations: _find_ and _union_
- Use cases :
  - Kruskal's minimum spanning tree algorithm
  - Grid percolation
  - Network connectivity
  - LCA in trees
  - Image processing
- To find which component a particular element belongs to find the root of that component by following the parent nodes until a self loop is reached (a node who's parent is itself)
- To unify two elements find which are the root nodes of each component and if the root nodes are different make one of the root nodes be the parent of the other.
- **Connected components in a graph** : We have to add vertices and undirected edges, and answer queries of the form `(a, b)` - "are the vertices a and b in the same connected component of the graph"
- Here we can directly apply the data structure, and get a solution that handles an addition of a vertex or an edge and a query in nearly constant time on average.
- **Search for connected components in an image** One of the applications of DSU is the following task: there is an image of n×m pixels. Originally all are white, but then a few black pixels are drawn. You want to determine the size of each white connected component in the final image.
- For the solution we simply iterate over all white pixels in the image, for each cell iterate over its four neighbors, and if the neighbor is white call union_sets. Thus we will have a DSU with nm nodes corresponding to image pixels. The resulting trees in the DSU are the desired connected components

### Suffix Arrays

- A suffix array is an array of **sorted** indices which contains all the sorted suffixes of a string.
- The actual 'suffix array' is the array of sorted indices. This provides a compressed representation of the sorted suffixes w/o actually needing to store the suffixes.
- suffix arrays can do everything suffix trees can, with some additional information such as a LCP array

### Fenwick Tree

- Motivation : Given an array of integer values compute the range sum between index [i, j]
- Fenwick trees are 1 based.
- Unlike a regular array, in a Fenwick tree a specific cell is responsible for other cells as well.
- The position of the LSB determines the range of responsibility that cell has to the cells below itself.
- 10 -> 1010 : LSB is at position 2, so this index is responsible for 2^(2-1) = 2 cells below itself.
- In a Fenwick tree we may compute the prefix sum up to a certain index, which ultimately lets us perform range sum query.
- find lowest one bit : i & -i

### Graphs

#### Graph-Processing Problems

- **s-t Path.** Is there a path between vertices s and t?
- **Shortest s-t Path.** What is the shortest path between vertices s and t?
- **Cycle.** Does the graph contain any cycles?
- **Euler Tour.** Is there a cycle that uses every edge excactly once?
- **Hamilton Tour.** Is there a cycle that uses every vertex exactly once?
- **Connectivity.** Is the graph connected, i.e. is there a path between all vertex pairs?
- **Biconnectivity.** Is there a vertex whose removal disconnects the graph?
- **Planarity.** Can you draw the graph on a piece of paper with no crossing edges?
- **Isomorphism.** Are two graphs isomorphic (the same graph in disguise)?

#### Directed Graphs

- **Single-source reachability** Given a digraph and a source vertex `s`, support queries of the form _Is there a path from s to a given target vertex `v`_ ?
- **Multi-source reachability** Given a digraph and a _set_ of source vertices, support queries of the form _Is there a directed path from any vertex in the set to a given target vertex v_?
- This problem arises in the solution of a classic string-processing problem.
- DFS marks all the vertices in a digraph reachable from a given set of sources in time proportional to the sum of the outdegrees of the vertices marked.
- **Mark-and-sweep garbage collection** - An important application of multiple-source reachability is found in typical memory-management systems.
- A mark-and-sweep garbage collection strategy reserves one bit per object for the purpose of garbage collection, then periodically _marks_ the set of potentially accessible objects by running a digraph reachability algorithm like `DirectedDFS` and _sweeps_ through all objects, collecting the unmarked ones for use for new objects.
- **Single-source directed paths** Given a digraph and a source vertex s, Is there a directed path from s to a given target vertex v? If so, find such a path.
- **Single-source shortest directed paths** Given a digraph and a source vertex s, Is there a path from s to a given target vertex v ? If so, find a shortest such path (one with a minimal number of edges).

#### Depth First Search

- To search a graph invoke a recursive method that visits vertices. To visit a vertex :
  - Mark it as having been visited.
  - Visit (recursively) all the vertices that are adjacent to it and that have not yet been marked.
- A DFS marks all the vertices connected to a given source in time proportional to the sum of their degrees.
- **Connectivity** : Are two given vertices connected? and How many connected components does the graph have?
- **Single-source paths** Given a graph and a source vertex s, Is there a path from s to a given target vertex v? If so, find such a path.
- DFS allows us to provide clients with a path from a given source to any marked vertex in time proportional to its length.
- The recursive call in DFS symbolizes a stack (LIFO). We choose, of the passages yet to be explored, the one that was most recently encountered.
- Properties of DFS
  - Guaranteed to reach every node.
  - Runs in O(v + E) time. Basic idea is that every edge is used at most once, and total number of vertex considerations is equal to number of edges.
- The depth first search is well geared towards problems where we want to visit all of the nodes in the graph
- It is a key property of the Depth First search that we not visit the same node more than once

#### Breadth First Search

- **Single-source shortest paths** Given a graph and a source vertex s, Is there a path from s to a given target vertex v ? If so, find a shortest such path (one with a minimal number of edges).
- The classical method for accomplishing this task, is called Breadth-First Search (BFS)
- In BFS we use a queue (FIFO). We choose, of the passages yet
  to be explored, the one that was least recently encountered.
- For any vertex v reacheable from s, BFS computes a shortest path from s to v (no path from s to v has fewer edges).
- BFS takes time proportional to V+E in the worst case
- It has the extremely useful property that if all of the edges in a graph are unweighted (or the same weight) then the first time a node is visited is the shortest path to that node from the source node

#### Applications of BFS

- Copying garbage collection
- Find the shortest path between two nodes _u_ and _v_, with path length measured by number of edges
- (Reverse) Cuthill-Mckee mesh numbering
- Ford-Fulkerson method for computing the maximum flow in a flow network
- Serialization/Deserialization of a binary tree vs serialization in sorted order, allows the tree to be re-constructed in an efficient manner.
- Testing bipartiteness of a graph

#### Connected Components

- DFS uses preprocessing time and space proportional to V+E to support constant-time connectivity queries in a graph.
- we prefer union-find when determining connectivity is our only task or when we have a large number of queries intermixed with edge insertions

#### Cycle detection

- Is a given graph acyclic?

#### Two-colorability

- Can the vertices of a given graph be assigned one of two colors in such a way that no edge connects vertices of the same color? In other words: Is the graph bipartite?

#### Scheduling Problems

- _precedence constraints_, which specify that certan tasks must be performed before certain others.
- **Precedence-constrained scheduling** Given a set of jobs to be completed, with precedence constraints that specify that certain jobs have to be completed before certain other jobs are begun, how can we schedule the jobs such that they are all completed while still respecting the constraints?
- **Topological sort** Given a digraph, put the vertices in order such that all its directed edges point from a vertex earlier in the order to a vertex later in the order (or report that doing so is not possible)
- If a precedence-constrained scheduling problem has a directed cycle, then there is no feasible solution.
- **Directed cycle detection** Does a given digraph have a directed cycle? If so, find the vertices on some such cycle, in order from some vertex back to itself.

#### Eulerian Cycle

- A directed graph has a Eulerian cycle if the following conditions are true
  - All vertices with nonzero degress belong to a single strongly connected component
  - In degree and out degree of every vertex is the same.

#### Minimum Height Trees

- Number of minimum height trees is 1 or 2. First find the tree diameter. If the diameter is even 2 MHT if it's odd 1 MHT.
- The middle node between the two ends of the diameters correspond to the root nodes for which the MHT exists

### Fast Algorithms

| Application                   | Better Performance Choices                                          |
| ----------------------------- | ------------------------------------------------------------------- |
| Trusted data hashing          | [xxhash](http://cyan4973.github.io/xxHash/)                         |
| Untrusted data hashing        | [blake3](https://github.com/BLAKE3-team/BLAKE3)                     |
| Fast compression              | [lz4](https://github.com/lz4/lz4)                                   |
| Good compression              | [zstd](https://facebook.github.io/zstd/)                            |
| Best compression              | [zstd - 10+](https://facebook.github.io/zstd/)                      |
| Java crypto (md5, aes-gcm...) | [ACCP](https://github.com/corretto/amazon-corretto-crypto-provider) |
