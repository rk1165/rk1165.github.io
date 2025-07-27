+++
title = 'What every CS Major should know'
date = 2024-02-11

+++

### Portfolio vs Resume

- Every CS major should build a _portfolio_
  - A portfolio could be as simple as a personal blog, with a post for each project or accomplishment. A better portfolio would include per-project pages, and publicly browsable code.
  - Contributions to open source should be linked and documented.
- [Github is my resume](http://pydanny.blogspot.com/2011/08/github-is-my-resume.html)

### The Unix philosophy

- compose processes with pipes
- comfortably edit a file with emacs and vim
- create, modify and execute a Makefile for a software project
- write simple shell scripts.
- Tasks to complete
  - Find the five folders in a given directory consuming the most space.
  - Report duplicate MP3s (by file contents, not file name) on a computer.
  - Take a list of names whose first and last names have been lower-cased, and properly recapitalize them.
  - Find all words in English that have x as their second letter, and n as their second-to-last.
  - Directly route your microphone input over the network to another computer's speaker.
  - Replace all spaces in a filename with underscore for a given directory.
  - Report the last ten errant accesses to the web server coming from a specific IP address.

#### Recommended reading

- The Unix Programming Environment
- The Linux Programming Interface
- Unix Power Tools
- Linux Server Hacks
- [The Single Unix specification](http://www.unix.org/online.html)
- [commandlinefu](http://www.commandlinefu.com/)

### Systems administration

- Install and administer a Linux distribution
- Configure and compile the Linux kernel
- Troubleshoot a connection with `dig`, `ping` and `traceroute`
- Compile and configure a web server like apache.
- Compile and configure a DNS daemon like bind
- Maintain a web site with a text editor.
- **Reading** UNIX and Linux System Administration Handbook by Nemeth, Synder et al.

### Programming languages

- The following languages provide a reasonable mixture of paradigms and practical applications:
  - Racket
  - C
  - JavaScript
  - Squeak
  - Java
  - Standard ML
  - Prolog
  - Scala
  - Haskell
  - C++
  - Assembly

#### Racket

- [How to Design Programs](https://htdp.org/2018-01-06/Book/)
- [The Racket Docs](http://docs.racket-lang.org/)

#### C

- The C Programming Language

#### JavaScript

- JavaScript: The Definitive Guide
- JavaScript: The Good Parts
- Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript

#### Squeak

- [Introduction to Squeak](http://wiki.squeak.org/squeak/377)

#### Java

- Effective Java

#### Standard ML

- ML for the Working Programmer
- The Definition of Standard ML

#### Prolog

- [Learn Prolog Now!](http://www.learnprolognow.org/)
- [Another tutorial](http://kti.ms.mff.cuni.cz/~bartak/prolog/contents.html)
- [miniKanren](http://minikanren.org/)

#### Scala

- Programming in Scala by Odersky, Spoon and Venners
- Programming Scala by Wampler and Payne

#### Haskell

- [Learn You a Haskell](http://learnyouahaskell.com/)
- [Real World Haskell](http://book.realworldhaskell.org/read/)

#### C++

- The C++ Programming Language
- C++ Templates: The Complete Guide
- Programming Pearls

#### Assembly

- Learning compilers is the best way to learn assembly
- should understand generative programming (macros); lexical (and dynamic) scope; closures; continuations; higher-order functions; dynamic dispatch; subtyping; modules and functors; and monads as semantic concepts distinct from any specific syntax.

#### Lisp

- SICP
- Lisp in Small Pieces

### Discrete Mathematics

- It's important to cover reasoning about:
- trees
- graphs
- formal languages
- automata
- **Reads**
  - How to Prove it: A Structured Approach
  - How to Solve It

### Data Structures and Algorithms

### Theory

- theory should cover at least models of computation and computational complexity.
- Models of computation should cover finite-state automata, regular languages (and regular expressions), pushdown automata, context-free languages, formal grammars, Turing machines, the lambda calculus, and undecidability.
- should learn at least enough complexity to understand the difference between P, NP, NP-Hard and NP-Complete.
- **Read** Introduction to Theory of Computation

### Architecture

- nand2tetris
- [What every programmer should know about memory](http://lwn.net/Articles/250967/)

### Operating Systems

- should be aware of how kernels handle system calls, paging, scheduling, context-switching, filesystems and internal resource management.
- A good understanding of operating systems is secondary only to an understanding of compilers and architecture for achieving performance.
- To get a better understanding of the kernel, students could:
  - print "hello world" during the boot process;
  - design their own scheduler;
  - modify the page-handling policy; and
  - create their own filesystem.
- **Read** Linux Kernel Development

### Networking

- should have a firm understanding of the network stack and routing protocols within a network.
- it's helpful to know the protocols for existing standards, such as:
- 802.3 and 802.11
- IPv4 and IPv6
- DNS, SMTP and HTTP
- should understand exponential back off in packet collision resolution and the additive-increase multiplicative-decrease mechanism involved in congestion control.
- Should implement the following:
  - an HTTP client and daemon
  - a DNS resolver and Server
  - a command-line SMTP mailer
- Wireshark
- too far to require all students to implement a reliable transmission protocol from scratch atop IP
- **Read** Unix Network Programming

### Security

- develop a sense of defensive programming--a mind for thinking about how their own code might be attacked.
- At a minimum:
  - social engineering
  - buffer overflows
  - integer overflow
  - code injection vulnerabilities
  - race conditions
  - privilege confusion
- **Reads**
  - Metasploit: The Penetration Tester's Guide
  - Security Engineering

### Cryptography

- should understand and be able to implement the following concepts, as well as the common pitfalls in doing so:
  - symmetric-key cryptosystems
  - public-key cryptosystems
  - secure hash functions
  - challenge-response authentication
  - digital signature algorithms
  - threshold cryptosystems
- should know how to acquire a _sufficiently_ random number for the task at hand.
- need to know how to salt and hash passwords for storage.
- RSA is easy enough to implement that everyone should do it
- Every student should create their own digital certificate and set up https in apache.
- Student should also write a console web client that connects over SSL.
- should know how to use GPG; how to use public-key authentication for ssh; and how to encrypt a directory or a hard disk.
- **Read** Cryptography Engineering

### Software testing

### User experience design

- [The Absolute Minimum](http://www.joelonsoftware.com/articles/Unicode.html)

### Visualization

- The Visual Display of Quantitative Information

### Parallelism

- harnessing parallelism requires deep knowledge of architecture: multicore, caches, buses, GPUs, etc.
- students should learn CUDA and OpenCL.
- pthreads is a reasonably portable threads library to learn.
- For anyone interested in large-scale parallelism, MPI is a prerequisite.

### Software Engineering

- A working knowlege of debugging tools like gdb and valgrind goes a long way when they finally become necessary.
- [Version Control by Example](http://www.ericsink.com/vcbe/)

### Formal Methods

- should be at least moderately comfortable using one theorem prover
- [Software Foundations](http://www.cis.upenn.edu/~bcpierce/sf/)

### Graphics and simulation

- Simple ray tracers can be constructed in under 100 lines of code.
- It's good mental hygiene to work out the transformations necessary to perform a perspective 3D projection in a wireframe 3D engine
- Data structures like BSP trees and algorithms like z-buffer rendering are great examples of clever design.
- Mathematics for 3D Game Programming and Computer Graphics by Lengyel.

### Robotics

### Artifical Intelligence

- **Read** Artifical Intelligence Rusell and Norvig

### Machine Learning

### Databases

- Fundamental data structures and algorithms that power a database engine
- Relational algebra and relational calculus stand out as exceptional success stories in sub-Turing models of computation.
- ER modeling seems to be a reasonable mechanism for visualing encoding the design of and constraints upon a software artifact.
- that can set up and operate a LAMP stack is one good idea and a lot of hard work away from running their own company.
- **Read** SQL and Relational Theory by Data

### Minimum

- To answer the title question, here is what an undergraduate CS student (the foundation for an adept programmer) should know upon graduation:

### Data Structures

- Machine Data Representation
  - Ones, Two's Complement, and Related Arithmetic
  - Words, Pointers, Floating Point
  - Bit Access, Shifting, and Manipulation
- Linked Lists
- Hash Tables (maps or dictionaries)
- Arrays
- Trees
- Stacks
- Queues
- Graphs
- Databases

### Algorithms

- Sorting:
  - Insertion Sort
  - Radix style sorts, Counting Sort and Bucket Sort
  - Bogo and Quantum Sort
- Searching:
  - String Manipulation
  - Iteration
  - Tree Traversal
  - List Traversal
  - Hashing Functions
  - Concrete implementation of a Hash Table, Tree, List, Stack, Queue, Array, and Set or Collection
  - Scheduling Algorithms
  - File System Traversal and Manipulation (on the inode or equivalent level).

### Design Patterns

- Modularization
- Factory
- Builder
- Singleton
- Adapter
- Decorator
- Flyweight
- Observer
- Iterator
- State Machine
- Model View Controller
- Threading and Parallel Programming Patterns

### Paradigms

- Imperative
- Object Oriented
- Functional
- Declarative
- Static and Dynamic Programming
- Data Markup

### Complexity Theory

- Complexity Spaces
- Computability
- Regular, Context Free, and Universal Turing Machine complete Languages
- Regular Expressions
- Counting and Basic Combinatorics

### Beyond

- Here are some tips for algorithms and data structures:
- Radix style sorts are awesome, but only when you have finite classes of things being sorted.
- Trees are good for almost anything as are Hash Tables. The functionality of a Hash Table can be extrapolated and used to solve many problems at the cost of efficiency.
- Arrays can be used to back most higher level data structures. Sometimes a "data structure" is no more than some clever math for accessing locations in an array.

### Algorithms to Know

### Numerical:

- [Euclid's algorithms](http://en.wikipedia.org/wiki/Euclidean_algorithm)
- [Gaussian elimination](http://en.wikipedia.org/wiki/Gaussian_elimination)
- [Fourier-Motzkin elimination](http://en.wikipedia.org/wiki/Fourier-Motzkin_elimination)
- [Fast Fourier Transform](http://en.wikipedia.org/wiki/Fast_Fourier_transform)
- [Karatsuba algorithm](https://en.wikipedia.org/wiki/Karatsuba_algorithm)

### Data structures:

- [Binary trees](http://en.wikipedia.org/wiki/Binary_tree)
- [Hash tables](http://en.wikipedia.org/wiki/Hash_table)
- [Binary decision diagrams](http://en.wikipedia.org/wiki/Binary_decision_diagram)
- [Disjoint sets](http://en.wikipedia.org/wiki/Disjoint-set_data_structure)
- [Trie](http://en.wikipedia.org/wiki/Trie)
- [Priority queue](http://en.wikipedia.org/wiki/Priority_queue)
- [Suffix tree](https://en.wikipedia.org/wiki/Suffix_tree)

### Sorting & searching arrays:

- [Bubblesort](http://en.wikipedia.org/wiki/Bubblesort)
- [Quicksort](http://en.wikipedia.org/wiki/Quicksort) - in place sort, uses less memory and is [empirically faster](http://cs.stackexchange.com/questions/3/why-is-quicksort-better-than-other-sorting-algorithms-in-practice) for most applications
- [Heapsort](http://en.wikipedia.org/wiki/Heapsort) - mostly useful because building a heap is a really handy way of making a priority queue
- [Binary search](http://en.wikipedia.org/wiki/Binary_search)
- [Mergesort](https://en.wikipedia.org/wiki/Merge_sort)
- [Radixsort](https://en.wikipedia.org/wiki/Radix_sort)

### Tree search:

- [Breadth first search](http://en.wikipedia.org/wiki/Breadth_first_search)
- [Depth first search](http://en.wikipedia.org/wiki/Depth_first_search)

### Graphs:

- [Dijkstra's algorithm](http://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)
- [Prim's algorithm](http://en.wikipedia.org/wiki/Prim%27s_algorithm)
- [Ford-Fulkerson](http://en.wikipedia.org/wiki/Ford-Fulkerson_algorithm)
- [A\*](http://en.wikipedia.org/wiki/A*_search_algorithm)

### Automata and parsing:

- [Finite automata](http://en.wikipedia.org/wiki/Finite_automaton)
- [Thompson's construction for creating a nondeterministic finite automaton from a regular expression](http://en.wikipedia.org/wiki/Thompson's_construction_algorithm)
- [The powerset construction for determinising a finite automaton](http://en.wikipedia.org/wiki/Powerset_construction)
- [Pushdown automata](http://en.wikipedia.org/wiki/Pushdown_automaton) and [context-free languages](http://en.wikipedia.org/wiki/Context_free_language)
- [Recursive descent parsing for LL(k) languages LR parsing for LR(k) languages](http://en.wikipedia.org/wiki/Recursive_descent_parser)

### Numerical optimization:

- [Simplex](http://en.wikipedia.org/wiki/Simplex_algorithm)
- [Hill climbing algorithms](http://en.wikipedia.org/wiki/Hill_climbing)
- [Newton's method](http://en.wikipedia.org/wiki/Newton%27s_method_in_optimization)

### Combinatorial optimization:

- [The Hungarian algorithm for solving the assignment problem](http://en.wikipedia.org/wiki/Hungarian_algorithm)
- [Boolean satisfiability -- an NP-complete problem](http://en.wikipedia.org/wiki/Boolean_satisfiability_problem)
- [DPLL -- used to solve SAT](http://en.wikipedia.org/wiki/DPLL_algorithm)
- [QBF -- a PSPACE complete problem](http://en.wikipedia.org/wiki/QBF)

### Graphics:

- [Rasterisation](http://en.wikipedia.org/wiki/Rasterisation)
- [Ray tracing](http://en.wikipedia.org/wiki/Ray_tracing)

### Compilers:

- [Hindley-Milner type inference](http://en.wikipedia.org/wiki/Hindley%E2%80%93Milner)
- [Register colouring](http://en.wikipedia.org/wiki/Register_allocation)

### Machine learning:

- [Back propagation for neural networks](http://en.wikipedia.org/wiki/Back-propagation)
- [Expectation maximization algorithm](http://en.wikipedia.org/wiki/Expectation-maximization_algorithm)
- [Naive Bayes](http://en.wikipedia.org/wiki/Naive_Bayes_classifier)
- [Support vector machines](http://en.wikipedia.org/wiki/Support_vector_machine)
- [Random forests](http://en.wikipedia.org/wiki/Random_forests)

### Cryptography:

- [Diffie-Hellman key exchange](http://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
- [DES](http://en.wikipedia.org/wiki/Data_Encryption_Standard)
- [Block cipher modes of operation](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation) -- most important is CBC
- [RSA](<http://en.wikipedia.org/wiki/RSA_(algorithm)>)

### Miscellaneous:

- [Graham scan for finding convex hulls](http://en.wikipedia.org/wiki/Graham_scan)
- [Quine-McCluskey algorithm for logic minimization](http://en.wikipedia.org/wiki/Quine-McCluskey)

### Unconstrained optimization

- [BFGS and conjugate gradient](http://www.alglib.net/optimization/lbfgsandcg.php)

### Five essential phone screen questions

#### Coding

- Write a function to reverse a string Runtime O(n)
- Write a function to compute Nth fibonacci number. Runtime O(n)
- Print out the grade school multiplication table upto 12x12
- Write a function that sums up integers from a text file, one int per line.
- Write function to print the odd numbers from 1 to 99.
- Format an RGB value (three 1-byte numbers) as a 6-digit hexadecimal string.

#### Object Oriented Programming

- Should be able to give satisfactory definitions for a random selection of the following terms:
  1. class, object (and the difference between the two)
  2. instantiation
  3. method (as opposted to, say, a C function)
  4. virtual method, pure virtual method
  5. class/static method
  6. static/class initializer
  7. constructor
  8. destructor/finalizers
  9. superclass or base class
  10. subclass or derived class
  11. inheritance
  12. encapsulation
  13. multiple inheritance (and give an example)
  14. delegation/ forwarding
  15. compositon/aggregation
  16. abstract class
  17. interface/protocol (and difference from abstract class)
  18. method overriding
  19. method overloading (and difference from overriding)
  20. polymorphism (without resorting to examples)
  21. is-a versus has-a relationships (with examples)
  22. method signatures (what's included in one)
  23. method visibility (e.g. public/private/other)
- understand when to use a subclass versus an attribute or property
- understand the difference between a static member and an instance member
- understand that objects are supposed to know how to take care of themselves
- understand the difference between a char\*, an object, and an enum.
- For OO-design describe:
  1. What classes they would define.
  2. What methods go in each class (including signatures)
  3. What the class constructors are responsible for.
  4. What data structures the class will have to maintain.
  5. Whether any Design Patterns are applicable to this problem
- Some examples of OO-Design
  1. Design a deck of cards that can be used for different card game applications.
  2. Model the Animal kingdom as a class system, for use in a Virtual Zoo program.
  3. Create a class design to represent a filesystem
  4. Design an OO representation to model HTML.

#### Scripting and Regular Expressions

- 50,000 HTML files in a Unix directory tree, under a directory called "/website". We have 2 days to get a list of file paths to the editorial staff. You need to give me a list of the .html files in this directory tree that appear to contain phone numbers in the following two formats: (xxx) xxx-xxxx and xxx-xxx-xxxx.

#### Data Structures

- For the standard data structures in java.util, STL, or those built into a higher-level language, they need to know the big-O complexity for the operations on those data structures
- The (concrete) data structures they absolutely must understand
  1. arrays
  2. vectors
  3. linked lists
  4. hashtables
  5. trees
  6. graphs
- Should be able to describe
  - what you use them for (real-life examples)
  - why you prefer them for those examples
  - the operations they typically provide (e.g insert, delete, find)
  - the big-O performance of those operations
  - how you traverse them to visit all their elements, and what order they're visited in.
- difference between an abstract data type such as a Stack, Map, List or Set, and a concrete data structure such as a singly-linked list or a hash table
- For a given abstract data type (e.g. a Queue), they should be able to suggest at least two possible concrete implementations, and explain the performance trade-offs between the two implementations.
- Some questions
  1. What are some really common data structures, e.g. in java.util?
  2. When would you use a linked list vs. a vector?
  3. Can you implement a Map with a tree? What about with a list?
  4. How do you print out the nodes of a tree in level-order (i.e. first level, then 2nd level, then 3rd level, etc.)
  5. What's the worst-case insertion performance of a hashtable? Of a binary tree?
  6. What are some options for implementing a priority queue

#### Bits and Bytes

- should know the bitwise and logical operators for their language, and should be able to use them for simple things like setting or testing a specific bit, or set of bits.
- should know about the bit-shift operators in their language, and should know why you would want to use them.
- Some questions:
  - Tell me how to test whether the high-order bit is set in a byte.
  - Write a function to count all the bits in an int value; e.g. the function with the signature int countBits(int x)
  - Describe a function that takes an int value, and returns true if the bit pattern of that int value is the same if you reverse it (i.e. it's a palindrome); i.e. boolean isPalindrome(int x)
- Areas where bit-and-byte manipulation is useful:
  - stuff that involves a lot of bit- and byte-manipulation; examples include network protocols, writing binary serialization/marshalling code, and reading or reverse-engineering file formats.
  - Another is when you're doing any sort of memory or pointer coding
  - Another broad class of problems involves data whose byte representation is especially significant. UTF-8 and Unicode are good examples
