+++
title = 'Java 8'
date = 2024-02-11

+++

### What are the important features of Java 8 release?

- Interface methods by default
- Lambda expressions
- Functional interfaces
- References to methods and constructors
- Repeatable annotations
- Annotations on data types
- Reflection for method parameters
- Stream API for working with collections
- Parallel sorting of arrays
- New API for working with dates and times
- New JavaScript Nashorn Engine
- Added several new classes for thread safe operation
- Added a new API for `Calendar`and `Locale`
- Added support for Unicode 6.2.0
- Added a standard class for working with Base64
- Added support for unsigned arithmetic
- Improved constructor `java.lang.String(byte[], *)` and method performance `java.lang.String.getBytes()`
- A new implementation `AccessController.doPrivileged` that allows you to set a subset of privileges without having to check all other access levels
- Password-based algorithms have become more robust
- Added support for SSL / TLS Server Name Indication (NSI) in JSSE Server
- Improved keystore (KeyStore)
- Added SHA-224 algorithm
- Removed JDBC Bridge - ODBC
- PermGen is removed, the method for storing metadata of classes changed
- Ability to create profiles for the Java SE platform, which include not the entire platform, but some part of it
- Java 8 interface changes include `static` methods and `default` methods in interfaces.
- A **functional interface** is an interface that defines only one abstract method.
- To accurately determine the interface as functional, an annotation has been added `@FunctionalInterface` that works on the principle of `@Override`. It will designate a plan and will not allow to define the second abstract method in the interface.
- An interface can include as many `default` methods as you like while remaining functional, because `default` methods are not abstract.
- If a class implements an interface, it can, but does not have to, implement the `default` methods already implemented in the interface. The class inherits the default implementation.
- If a class implements several interfaces that have the same `default` method, then the class must implement the method with the same signature on its own. The situation is similar if one interface has a `default` method, and in the other the same method is abstract - no class default implementation is inherited.
- The `default` method cannot override the class method `java.lang.Object`.
- `static` interface methods are similar to `default` methods, except that there is no way to override them in classes that implement the interface.
- `static` methods in the interface are part of the interface without the ability to use them for objects of the implementation class
- Class methods `java.lang.Object` cannot be overridden as static
- `static` methods in the interface are used to provide helper methods, for example, checking for null, sorting collections, etc.
- `Base64` - a thread-safe class that implements a data encoder and decoder using a base64 encoding scheme.
- Base64 contains 6 basic methods:
  - `getEncoder()`/`getDecoder()` - returns a base64 encoder/decoder;
  - `getUrlEncoder()`/`getUrlDecoder()`- returns URL-safe base64 encoder/decoder
  - `getMimeEncoder()`/`getMimeDecoder()`- returns a MIME encoder/decoder

### What is the method `collect()` for streams for?

- A method `collect()`is the final operation that is used to represent the result as a collection or some other data structure.
- `collect()`accepts an input that contains four stages:
  - **supplier** — initialization of the battery,
  - **accumulator** — processing of each element,
  - **combiner** — connection of two accumulators in parallel execution,
  - **[finisher]** — a non-mandatory method of the last processing of the accumulator.
- In Java 8, the class `Collectors` implements several common collectors:
  - `toList()`, `toCollection()`, `toSet()`- present stream in the form of a list, collection or set
  - `toConcurrentMap()`, `toMap()`- allow you to convert the stream to `Map`
  - `averaging[Int/Double/Long]()` - returns the average value
  - `summing[Int/Double/Long]()` - returns the sum
  - `summarizing[Int/Double/Long]()` - returns SummaryStatistics with different values of the aggregate
  - `partitioningBy()`- divides the collection into two parts according to the condition and returns them as `Map<Boolean, List>`
  - `groupingBy()`- divides the collection into several parts and returns `Map<N, List<T>>`
  - `mapping()`- Additional value conversions for complex Collectors.

### Intermediate methods of working with streams

| Methods                                     | Use                                                                            |
| ------------------------------------------- | ------------------------------------------------------------------------------ |
| `filter()`                                  | filters records, returning only records matching the condition                 |
| `skip()`                                    | allows you to skip a certain number of elements at the beginning               |
| `distinct()`                                | returns a stream without duplicates (for a method `equals()`)                  |
| `map()`                                     | converts each element                                                          |
| `peek()`                                    | returns the same stream, applying a function to each element                   |
| `limit()`                                   | allows you to limit the selection to a certain number of first elements        |
| `sorted()`                                  | allows you to sort values ​​either in natural order or by setting `Comparator`   |
| `mapTo[Int/Double/Long]()`                  | analog to `map()` return stream numeric primitives                             |
| `flatMap()`, `flatMapTo[Int/Double/Long]()` | similar to `map()` but can create a single element                             |

### final methods of working with streams

| Methods            | Use                                                                               |
| ------------------ | --------------------------------------------------------------------------------- |
| `findFirst()`      | returns the first element                                                         |
| `findAny()`        | returns any suitable item                                                         |
| `collect()`        | presentation of results in the form of collections and other data structures      |
| `count()`          | returns the number of elements                                                    | 
| `anyMatch()`       | returns true if the condition is satisfied for at least one element               |
| `noneMatch()`      | returns true if the condition is not satisfied for any element                    |
| `allMatch()`       | returns true if the condition is satisfied for all elements                       |
| `min()`            | returns the minimum element, using as a condition Comparator                      |
| `max()`            | returns the maximum element, using as a condition Comparator                      |
| `forEach()`        | applies a function to each object (order is not guaranteed in parallel execution) |
| `forEachOrdered()` | applies a function to each object while preserving the order of elements          |
| `toArray()`        | returns an array of values                                                        |
| `reduce()`         | allows you to perform aggregate functions and return a single result.             |
| `sum()`            | returns the sum of all numbers                                                    |
| `average()`        | returns the arithmetic mean of all numbers.                                       |
