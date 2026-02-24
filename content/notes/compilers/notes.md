+++
title = 'Compiler Notes'
date = 2024-02-11

+++

### Type erasure and reification

- Types at compile time, no types at run-time.
- Types matter to the compiler. Once everything is type-checked, however, the types are simply _erased_ and the code the compiler generates is oblivious to them.
- Reification is the other way to go - types are retained at run-time and used for performing various checks.
- In Java type erasure is used to implement generics entered in the compiler.
- In python types are a fully reified concept.

### Covariance and contravariance in subtyping

- _subtyping_ : a kind of polymorphism that lets us define hierarchical relations on types, with specific types being subtypes of more generic types. For example, a Cat could be a subtype of Mammal, which itself is a subtype of Vertebrate.
- **Liskov substitution principle** : Let phi(x) be a property provable about objects `x` of type `T`. Then phi(y) should be true for objects `y` of type `s` where `S` is a subtype of `T`
- A shorter way to say S is a subtype of T is `S <: T`.
- Variance refers to how subtyping between composite types (e.g list of Cats versus list of Mammals) relates to subtyping between their components (e.g. Cats and Mammals).
- Let `Composite<T>` to refer to some composite type with components of type `T`. Given types `S` and `T` with the relation `S <: T`, _variance_ is a way to describe the relation between the composite types:
  - _Covariant_ means the ordering of component types is preserved: `Composite<S> <: Composite<T>`
  - _Contravariant_ means the ordering is reversed: `Composite<T> <: Composite<S>`
  - _Bivariant_ means both covariant and contravariant
  - _Invariant_ means neither covariant and contravariant

### Garbage Collection

- Garbage collectors are getting pretty darned smart. They can determine object lifetimes, turn heap allocations into stack allocations, move objects around to manage the heap more effectively, pool immutable objects, and take advantage of hardware support for memory management.
- Things to worry about in a garbage collected system:
  - **Allocating too many objects** When you're considering an algorithm or data structure, be aware of its memory requirements,
  - **Keeping hard references to unused objects** if you have a live hard-reference to an object, then the garbage collector isn't going to free it
  - **Keeping unused resources open** another classic boo-boo in garbage-collected programs is using up all of your filehandles, database connections, or some other limited resource, because you chose to release them in finalizers that are never invoked. (as in `finally` blocks)
- Behaviour of GC:
  - serial vs parallel
  - concurrent vs stop the world
  - compacting vs non-compacting vs copying
- Performance metric:
  - **throughput** : % of time not spent on GC
  - **GC overhead** : amount of time spent on GC
  - **Pause time** : total time during which application halts during GC
  - **frequency of collection** : how frequent GC happens
  - **Promptness** : time b/w when object became garbage and GC happened for the object

### Weak vs Strong typing

- Choosing a language:
  1. _Above all, we need stability_. We have enormous scale and massive business complexity. To create order out of inevitable chaos, we need rigorous modeling for both our code and our data. If we don't get the model and architecture mostly correct in the beginning, it will hurt us later, so we'd better invest a lot of effort in up-front design. We need hardened interfaces — which means static typing by definition, or users won't be able to see how to use the interfaces. We need to maximize performance, and this requires static types and meticulous data models. Our most important business advantages are the stability, reliability, predictability and performance of our systems and interfaces. Viva SOAP (or CORBA), UML and rigorous ERDs, DTDs or schemas for all XML, and either of C++, Java, C#, OCaml, Haskell, Ada.
  2. _Above all, we need flexibility_. Our business requirements are constantly changing in unpredictable ways, and rigid data models rarely anticipate these changes adequately. Small teams need to be able to deliver quickly on their own goals, yet simultaneously keep up with rapid changes in the rest of the business. Hence we should use flexible, expressive languages and data models, even if it increases the cost of achieving the performance we need. We can achieve sufficient reliability through a combination of rigorous unit testing and agile development practices. Our most important business advantage is our ability to deliver on new initiatives quickly. Viva XML/RPC and HTTP, mandatory agile programming, loose name/value pair modeling for both XML and relational data, and either of Python, Ruby, Lisp, Smalltalk, Erlang.
- If we move to cellular automata, or in fact any other parallel computational model that's resilient to node failures, then we'll need a new language,
- Cell or grid (or whatever) parallel computing will have a radically different internal economy. It'll need new data structures, new algorithms, new instruction sets, new everything
