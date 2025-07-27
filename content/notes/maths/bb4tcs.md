+++
title = 'Building Blocks for Theoretical Computer Science'
date = 2024-02-11

+++


#### Note 1

- **proposition** is simply a statement which is either true or false.
- **Conjunction**: P ^ Q ("P and Q").
- **Disjunction**: P ∨ Q ("P or Q").
- **Negation**: ¬P ("not P")
- A propositional form that is always true regardless of the truth values of its variables is called a _tautology_ such as P ∨ ¬P
- A statement such as P ∧ ¬P, which is always false, is called a _contradiction_.
- **Implication**: P => Q ("P implies Q"). This is the same as "If P, then Q."
- Here P is called the _hypothesis_ and Q is the _conclusion_.
- An implication P => Q is false only when P is true and Q is false.
- Note that P => Q is always true when P is false.
- When an implication is stupidly true because the hypothesis is false, we say that it is _vacuously true_.
- Note also that P => Q is **logically equivalent** to ¬P ∨ Q
- Note that P <=> Q is true only when P and Q have the same truth values (both true or both false).
- Given an implication P => Q, we can also define its

1.  **Contrapositive**: ¬Q => ¬P
2.  **Converse**: Q => P

- Note that in a _finite_ universe, we can express existentially and universally quantified propositions without quantifiers, using disjunctions and conjunctions respectively.
- How to negate conjunctions and disjunctions:
  - ¬(P∧Q) ≡ (¬P∨¬Q)
  - ¬(P∨Q) ≡ (¬P∧¬Q)
  - ¬(∀xP(x)) ≡ ∃x¬P(x)
  - ¬(∃xP(x)) ≡ ∀x¬P(x)

### Proof Techniques

#### Direct Proof

- _Goal_: To prove P => Q <br> _Approach_: Assume P .... Therefore Q
- Whenever you wish to prove an equivalence P <=> Q, always proceed by showing P => Q and Q => P separately.

#### Proof by Contraposition

- _Goal_: To prove P => Q <br> _Approach_: Assume ¬Q ... Therefore ¬P <br> _Conclusion_: ¬Q => ¬P, which is equivalent to P => Q.
- (Pigeonhole Principle). Let n and k be positive integers. Place n objects into k boxes. If n > k, then at least one box must contain more than one object.

#### Proof by Contradiction

- _Goal_: To prove P <br> _Approach_: Assume ¬P .... R .... ¬R <br> _Conclusion_: ¬P => ¬R∧R, which is a contradiction. Therefore, P holds.
- Lemma: Every natural number greater than one is either prime or has a prime divisor.
- proving that something _doesn't_ exist seems difficult. But this is actually one setting in which proof by contradiction shines.
- Lemma: if a<sup>2</sup> is even, then a is even

#### Proof by Cases

- Sometimes when we wish to prove a claim, we don’t know which of a set of possible cases is true, but we know that _at least one_ of the cases is true. What we can do then is to prove the result in _both_ cases; then, clearly the general statement must hold.

#### Common errors when writing proofs:

1. When writing proofs, do not assume the claim you aim to prove!
2. Never forget to consider the case where your variables take on the value 0.
3. Be careful when mixing negative numbers and inequalities.
4. A subsidiary result that is useful in a more complex proof is called a _lemma_.

#### Note 3

- Letting P(n) denote the statement, our goals is to prove that ∀n∈N,P(n). The _principle of induction_ asserts that to prove this requires three simple steps:

1.  **Base Case**: Prove that P(0) is true.
2.  **Inductive Hypothesis**: For arbitrary k >= 0, assume that _P(k)_ is true.
3.  **Inductive Step**: With the assumption of the Inductive Hypothesis in hand, show that _P(k + 1)_ is true.

### Number Theory

- It has very important applications in cryptography and in the design of randomized algorithms.
- Randomization has become an increasingly important technique for creating very fast algorithms for storing and retrieving objects (e.g. hash tables), testing whether two objects are the same (e.g. MP3's).
- The shorthand for a divides b is a | b. The divisor is on the left.
- _For any integers a, b, and c, if a | b and a | c then a | (b + c)_
- Fundamental Theorem of Arithmetic: Every integer ≥ 2 can be written as the product of one or more prime factors. Except for the order in which you write the factors, this prime factorization is unique.
- There are quite fast algorithms for testing whether a large integer is prime.
- **(Division Algorithm)** _For any integers a and b, where b is positive, there are unique integers q (the quotient) and r (the remainder) such that_ `a = bq + r` _and_ `0 ≤ r < b`
- Many applications of number theory, use modular arithmetic.
- In modular arithmetic, there are only a finite set of numbers and addition "wraps around" from highest number to the lowest one.
- Two integers are "congruent mod `k`" if they differ by a multiple of `k`. Formally:
- If `k` is any positive integer, two integers `a` and `b` are congruent mod k (written `a ≡ b (mod k)`) if `k | (a - b)`
- _For any integers a, b, c, d, and k, k positive, if a ≡ b (mod k) and c ≡ d (mod k), then_ `a + c ≡ b + d (mod k)`
- _For any integers a, b, c, d, and k, k positive, if a ≡ b (mod k) and c ≡ d (mod k), then_ `ac ≡ bd (mod k)`
- When we gather up a group of congruent integers and treat them all as a unit, such a group is known as a **congruence class** or an **equivalence class**.
- Specifically, suppose that we fix a particular value for `k`. Then, if `x` is an integer, the equivalence class of x (written `[x]`) is the set of all integers congruent to `x mod k`. Or, equivalently, the set of integers that have remainder `x` when divided by `k`
- For the `k` equivalence classes of integers mod `k`, mathematicians tend to prefer the names `[0], [1],...,[k-1]`
- We define addition and multiplication on equivalence classes by:
- `[x] + [y] = [x + y]`
- `[x] * [y] = [x * y]`
- Modular arithmetic is often useful in describing periodic phenomena.
- Low-precision integer storage on computers frequently uses modular arithmetic. values in digitized pictures are often stored as 8-bit unsigned numbers.

### Sets

- Proofs of sets typically involve reasoning about subset relations.
- Both ⊆ and ≤ are examples of a general type of object called a _partial order_, for which transitivity is a key defining property.

### Relations

- A _relation R on a set A_ is a subset of A x A, i.e R is a set of ordered pairs of elements from A.
- We can draw pictures of relations using directed graphs.
- We draw a graph node for each element of A and we draw an arrow joining each pair of elements that are related
- Relations are classified by several key properties. The first is whether an element of the set is related to itself or not. There are three cases:
- Reflexive: every element is related to itself
- Irreflexive: no element is related to itself
- Neither reflexive nor irreflexive: some elements are related to themselves but some aren't
- The familiar relations ≤ and = on the real numbers are reflexive, but < is irreflexive.
- If R is a relation on set A then
- R is reflexive if xRx for all x ∈ A.
- R is irreflexive if xR/x for all x ∈ A
- Irreflexive is not the negation of reflexive. The negation of reflexive would be:
- not reflexive: there is an x ∈ A, xR/x
- Another important property of a relation is whether the order matters within each pair. That is, if xRy is in R, is it always the case the yRx? If this is true, then the relation is called _symmetric_.
- In a graph picture of a symmetric relation, a pair of elements is either joined by a pair of arrows going in opposite directions, or no arrows.
- Relations that put elements into an order, like ≤ or divides, have a different property called _antisymmetry_.
- A relation is _antisymmetric_ is two distinct elements are never related in both directions.
- There are mixed relations that have neither property.
- If R is a relation on a set A then:
- symmetric : for all x, y ∈ A, xRy implies yRx
- antisymmetric: for all x and y in A with x &#8800; y, if xRy, then y R/ x
- antisymmetric: for all x and y in A, if xRy and yRx, then x = y
- A relation R on a set is _transitive_ if:
- for all a, b, c ∈ A, if aRb and bRc, then aRc.
- transitivity holds for a broad range of familiar numerical relations such as <, =, divides, and set inclusion
- If we look at graph pictures, transitivity means that whenever there is an indirect path from x to y then there must also be a direct arrow from x to y.
- not transitive: there a, b, c ∈ A, aRb and bRc and aR/c
- It's ok if some sets of elements just aren't connected at all.
- A counter-intuitive special case is the relation in which absolutely no elements are related.
- Some important types of relations
- A **partial order** is a relation that is reflexive, antisymmetric, and transitive.
- A **linear order** (also called a total order) is a partial order R in which every pair of elements are **comparable**. That is, for any two elements x and y, either xRy or yRx
- A **strict partial order** is a relation that is irreflexive, antisymmetric and transitive.
- An **equivalence relation** is a relation that is reflexive, symmetric, and transitive.

### Functions and onto

- For each input value, a function must provide one and only one output value. i.e every element in A must have only one image in B
- A function f: A → B is _onto_ if its image is its whole co-domain. Or, equivalently, ∀y ∈ B, ∃x ∈ A, f (x) = y
- A function f is not onto if ∃y ∈ B, ∀x ∈ A, f (x) =/ y

#### Functions and one-to-one

- As with onto, whether a function is one-to-one frequently depends on its type signature.
- **Pigeonhole principle** Suppose you have `n` objects and assign `k` labels to these objects. If `n` > `k`, then two objects must have the same label.
- The phrase "without loss of generality" means that we are adding an additional assumption to our proof but we claim it isn't really adding any more information.

#### Graphs

- Graphs consists of a set of nodes `V` and a set of edges `E`.
- Complete graph on `n` nodes (K<sub>n</sub>) is a graph with `n` nodes in which every node is connected to every other node.
- We have n (n-1) / 2 connections
- Suppose G<sub>1</sub> = (V<sub>1</sub>, E<sub>1</sub>) and G<sub>2</sub> = (V<sub>2</sub>, E<sub>2</sub>) are graphs. An **isomorphism** from G<sub>1</sub> to G<sub>2</sub> is a bijection f : V<sub>1</sub> -> V<sub>2</sub> such that nodes `a` and `b` are joined by an edge if f(a) and f(b) are joined by an edge.
- Invariant graph properties for checking isomorphism :
- The two graphs must have the same number of nodes and the same number of edges.
- For any node degree `k`, the two graphs must have the same number of nodes of degree `k`.
- If G and G' are graphs then G' is a **subgraph** of G iff the nodes of G' are a subset of the nodes of G and the edges of G' are a subset of the edges of G. If two graph G and F are isomorphic, then any subgraph of G must have a matching subgraph somewhere in F.
- A **walk** of length `k` from node `a` to node `b` is a finite sequence of nodes a = v<sub>1</sub>, v<sub>2</sub>, ... v<sub>n</sub> = b and a finite sequence of edges e<sub>1</sub>, e<sub>2</sub>, ... e<sub>n-1</sub> in which e<sub>i</sub> connects v<sub>i</sub> to v<sub>i+1</sub> for all i.
- A walk is **closed** if its starting and ending nodes are the same, otherwise it is **open**.
- A **path** is a walk in which no node is used more than once.
- A **cycle** is a closed walk with at least three nodes in which no node is used more than once except that the starting and ending nodes are the same.
- Unlike cycles, closed walks can reuse nodes.
- A Graph is **connected** if there is a walk between every pair of nodes in G. If we have a graph G that might or might not be connected, we can divide G into connected components
- An **Euler Circuit** of a Graph G is a closed walk that uses each edge of the Graph exactly once. Only possible when graph is connected and each node has _even_ degree.
- A Graph G = (V, E) is **bipartite** if we can split V into two non-overlapping subsets V<sub>1</sub> and V<sub>2</sub> such that every edge in G connects an element of V<sub>1</sub> with an element of V<sub>2</sub>. That is, no edge connects two nodes from the same part of the division.
- Bipartite graphs often appear in matching problems, where the two subsets represent different types of objects.

#### 2-way Bounding

- Because we need to establish a lower bound and also an upper bound, a 2-way bounding proof contains two sub-proofs.
- The two sub-proofs can use entirely different techniques.
- Method is widely used in upper-level CS and maths courses (real analysis, algorithms)
- Marker making algorithms - like cutting clothing parts out of rectangles of cloth.
- _Suppose that T is an equilateral triangle with sides of length 2
  units We can place a maximum of four points in the triangle such that every pair of points are more than 1 unit apart._ Proof uses 2-way bounding together with Pigeonhole principle. Classic example of bounding a quantity from above and below.
- A _coloring_ of graph G assigns a color to each node of G, with the restriction that two adjacent nodes never have the same color.
- If G can be colored with `k` colors, we say that G is k-colorable.
- The _chromatic number_ of G, written χ(G), is the smallest number of colors needed to color G.
- The complete graph K<sub>n</sub> requires `n` colors, because each node is adjacent to all the other nodes.
- A bipartite graph never requires more than two colors.
- Graph coloring is required for solving a wide range of practical problems. For example, there is a coloring algorithm embedded in most compilers
- We can model a sudoku puzzle by setting up one node for each square. The colors are the 9 numbers, and some are pre-assigned to certain nodes. Two nodes are connected if their squares are in the same block or row or column. The puzzle is solvable if we can 9-color this graph, respecting the pre-assigned colors.
- We can model exam scheduling as a coloring problem
- A particularly important use of coloring in CS is register allocation.

#### Induction

- For any positive integer D, if all nodes in a graph G have degree
  ≤ D, then G can be colored with D + 1 colors.
- To apply induction to objects like graphs, we organize our objects by their size.
- We can use this idea to design an algorithm (called the “greedy” algo-
  rithm) for coloring a graph. This algorithm walks through the nodes one-by-one, giving each node a color without revising any of the previously-assigned colors. When we get to each node, we see what colors have been assigned to its neighbors. If there is a previously used color not assigned to a neighbor, we re-use that color. Otherwise, we deploy a new color. The above theorem shows that the greedy algorithm will never use more than D + 1 colors.
- The performance of the greedy algorithm is very sensitive to the order
  in which the nodes are considered
- So a useful heuristic is to order nodes by their degrees and color higher-degree nodes earlier in the process.
- Our inductive step assumed that _P(n)_ was true for all values of `n` from the base up through `k-1`, a so called **strong** inductive hypothesis.
- **weak** inductive hypothesis , in which we just assumed that _P(k-1)_ was true.

#### Recursive Definition

- The simplest technique for finding closed forms is called **unrolling**.
- _T(1) = 1_ , _T(n) = 2 \* T(n-1) + 3, ∀ n ≥ 2_
- The idea behind unrolling is to substitute a recursive definition into itself, so as to re-express _T(n)_ in in terms of _T(n-2)_ rather than _T(n-1)_.
- Finally we check when we hit the base case and do a substitution
- Many important algorithms in computer science involve dividing a big problem of (integer) size n into a sub-problems, each of size n/b.
- Analyzing such algorithms involves definitions that look like
- _S(1) = c_ , _S(n) = a\*S(⌈n/b⌉) + f(n), ∀ n ≥ 2_
- The base case takes some constant amount of work c. The term _f(n)_ is the work involved in dividing up the big problem and/or merging together the solutions for the smaller problems.
- Recursive definitions are ideally suited to inductive proofs.

### Trees

- Decision trees often used for engineering classification problems for which precise numerical models do not exist, such as transcribing speech waveforms into the basic sounds of a natural language.
- Formally, a tree is an undirected graph with a special node called the _root_, in which every node is connected to the root by exactly one path.
- Trees with "fat" nodes with a large bound on the number of children (e.g. 16) occur in some storage applications.
- In a **full** m-ary tree, each node has either zero or m children.
- In a **complete** m-ary tree, all leaves are at the same height.
- A full m-ary tree with i internal nodes has mi + 1 nodes total.
- Applications involving languages, both human languages and computer languages, frequently involve **parse trees** that show the structure of a sequence of **terminal symbols**.
- A **context-free grammar** is a set of rules which specify what sorts of children are possible for a parent node with each type of label.
- The lefthand side of each rule gives the symbol on the parent node and the righthand side shows one possible pattern for the symbols on its children.
- If all the nodes of a tree T have children matching the rules of some grammar G, we say that the tree T and the **terminal sequence** stored in its leaves are **generated** by G.
- The numbers obey the _heap property_ if , for every node X in the tree, the value in X is at least as big as the value in each of X's children.
- Trees with the heap property are convenient for applications where you have to maintain a list of people or tasks with associated priorities.

### NP

- The group of problems requiring exponential time is called **EXP**.
- There is another large class of tasks where the best known algorithm is exponential, but no one has proved that it is impossible to construct a polynomial-time algorithm. This group of problems is known as **NP**. (Non-deterministic Polynomial).
- A computational problem is in the set **NP** when an algorithm can provide justifications of "yes" answers, where each justification can be checked in polynomial time.
- Problems for which we can give polynomial-time checkable justifications for negative answers are in the set **co-NP**

### Collections of Sets

- Formally, a partition of a set A is a set of non-empty subsets of A which cover all the elements of A and which don't overlap. So, if the subsets in the partition are A<sub>1</sub> , A<sub>2</sub> , . . . A<sub>n</sub> , then they must satisfy three conditions:

1.  covers all of A:
2.  non-empty: A<sub>i</sub> &#8800; ∅ for all i
3.  no overlap: A<sub>i</sub> ∩ A<sub>j</sub> = ∅ for all i &#8800; j

- A subset of size _k_ is clled a _k_-combination.
- Suppose we are picking a group of _k_ objects (with possible duplicates) from a list of n types. Then our picture will contain _k_ stars and _n_-1 #'s. So we have _k_+_n_-1 positions in the picture and need to choose _n_-1 positions to contain the #'s. So the number of possible pictures is _k_+_n_-1 "chose" _n_-1

#### State Diagrams

- State diagrams are a type of directed graph, in which the graph nodse represent states and labels on the graph edges represent actions.
- State diagrams are often used to model puzzles or games. e.g. wolf-goat-cabbage puzzle
- Another standard application for state diagrams is in modelling pronunciations of words for speech recognition.
- In these diagrams, known as _phone lattices_, each edge represents the action of reading a single sound (a _phone_) from the input speech stream.
- Formally, we can define a state diagram to consist of a set of states _S_, a set of actions _A_, and a _transition function δ_ that maps out the edges of the graph, i.e. shows how actions result in state changes
- when a state diagram is viewed as an active device, i.e.
  as a type of machine or computer, it is often called a _state transition_ or an _automaton_

#### Countability

- Two sets _A_ and _B_ have the same cardinality (|A| = |B|) iff there is bijection from _A_ to _B_.
- An infinite set _A_ is _countably infinite_ if there is a bijection from N (or equivalently Z) onto _A_.
- |A| ≤ |B| iff there is a one-to-one function from A to B

#### Planar graphs

- A planar graph is a graph which can be drawn in the plane without any edges crossing.
- Planar graphs can be colored with only 4 colors.
- When a planar graph is drawn with no crossing edges, it divides the plane into a set of regions, called _faces_.
- The _boundary_ of a faces is the subgraph containing all the edges adjacent to that face and a _boundary walk_ is a closed walk containing all of those edges
- The _degree_ of the face is the minimum length of a boundary walk
- Euler's formula - `v - e + f = 2`
- **Corollary 1** : Suppose G is a connected planar graph, with v nodes, e edges, and f faces, where `v ≥ 3`. Then `e ≤ 3v - 6`.
- **Corollary 2** : If G is a connected planar graph, G has a node of degree less than six.
- **Corollary 3** : If G is a planar graph, G has a node of degree less than six.
- **Corollary 4** : Suppose G is a connected planar graph, with v nodes, e edges, and f faces, where v ≥ 3, and if all cycles in G have length ≥ 4, then `e ≤ 2v - 4`
- **Kuratowski's Theorem** : A graph is nonplanar iff it contains a subgraph that is a subdivision of K<sub>3,3</sub> or K<sub>5</sub>
- There are only five _Platonic solids_: cube, dodecahedron, tetrahedron, icosahedron, octahedron.
- These are convex polyhedra whose faces all have the same number of sides (k) and whose nodes all have the same number of edges going into them (d)
