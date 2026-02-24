+++
title = 'Garbage Collection'
date = 2024-02-11

+++

- GC is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects.
- It does this by using a **mark and sweep** approach. It first enumerates all the roots and then starts visiting the objects referenced by them recursively (essentially travelling the nodes in the memory graph). When it reaches an object it marks it with a special flag indicating that the object is reachable and hence not garbage. At the end of this mark phase it gets into the sweep phase. Any object in memory that is not marked by this time is garbage and the system disposes it.
- Whenever the GC runs, it has an effect on the application's performance. This is because all other threads in the application have to be stopped to allow the GC thread to effectively do its work.

### What is generational GCn and what makes it a popular GCn approach?

- Generational GCn can be loosely defined as the strategy used by the GC where the heap is divided into a number of sections called generations, each of which will hold objects according to their **age** on the heap.
- While marking GC can take a lot of time if all objects in a system must be scanned.
- As more and more objects are allocated, the list of objects grows leading to longer GCn time. However, empirical analysis of applications has shown that most objects are short-lived.
- With generational GCn, objects are grouped based on GCn cycles they have survived. This way, the bulk of the work is spread across various minor and major collection cycles.
- Today, almost all GCs are generational.

### Describe in detail how generational GCn works

- The heap is divided up into smaller spaces or generations. These spaces are **Young Generation**, **Old or Tenured Generation**, and **Permanent Generation**.
- The **young generation hosts most of the newly created objects**. New objects start their journey here and are only _promoted_ to the old generation space after they have attained a certain _age_.
- The term **age** in generational GCn refers to the number of collection cycles the object has survived.
- The young generation space is further divided into three spaces: an **Eden space** and two survivor spaces such as **S0** and **S1**.
- Objects that survived GCn from the young generation are promoted to old generation. It is generally larger than the young generation. As it is bigger in size, the GCn is more expensive and occurs less frequently than in the young generation.
- The permanent generation or more commonly called, _PermGen_, contains metadata required by the JVM to describe the classes and methods used in the application. It also contains the string pool for storing interned strings. It is populated by the JVM at runtime based on classes in use by the application. In addition, platform library classes and methods may be stored here.
- Any new objects are allocated to the Eden space. _S0_ and _S1_ are empty. When the Eden space fills up, a minor GCn is triggered. Referenced objects are moved to **S0**. Unreferenced objects are deleted.
- During the next minor GC, the same thing happens to the Eden space. Unreferenced objects are deleted and referenced objects are moved to **S1**.
- In addition, objects from the last minor GC in the first survivor space **S0** have their age incremented and are moved to **S1**.
- Once all surviving objects have been moved to **S1**, both **S0** and **Eden** space are cleared. At this point, Si contains objects with different ages.
- At the next minor GC, the same process is repeated. However this time the survivor spaces switch. Referenced objects are moved to S0 from both Eden and S1. Surviving objects are aged. Eden and S1 are cleared.
- After every minor GCn cycle, the age of each object is checked. Those that have reached a certain arbitrary age, for example, 8, are promoted from the young generation to the old or tenured generation. For all subsequent minor GC cycles, objects will continue to be promoted to the old generation space.
- This pretty much exhausts the process of GCn in the young generation. Eventually, a major GCn will be performed on the old generation which cleans up and compacts that space. For each major GC, there are several minor GCs

### When does an object become eligible for GCn? Describe how the GC collects an eligible object?

- An object becomes eligible for GCn if it is not reachable from any live threads or by any static references.
- The most straightforward case of an object becoming eligible for GCn is if all its references are null. Cyclic dependencies without any live external reference are also eligible for GC. So if object A references object B and object B references Object A and they don't have any other live reference then both Objects A and B will be eligible for GCn.
- Another obvious case is when a parent object is set to null. When a kitchen object internally references a fridge object and a sink object, and the kitchen object is set to null, both fridge and sink will become eligible for GCn alongside their parent, kitchen.

### How do you trigger GCn from Java code?

- **One cannot force GCn in Java**; it will only trigger if JVM thinks it needs a GCn based on Java heap size.
- Before removing an object from memory GCn thread invokes `finalize()` method of that object and gives an opportunity to perform any sort of cleanup required. You can also invoke this method of an object code, however, there is no guarantee that GCn will occur when you call this method
- Additionally, there are methods like `System.gc()` and `Runtime.gc()` which is used to send request of GCn to JVM but it's not guaranteed that GCn will happen.

### Is it possible to resurrect an object that became eligible for GCn?

- When an object becomes eligible for GCn, the GC has to run the `finalize()` method on it. The `finalize()` method is guaranteed to run only once, thus the GC flags the object as finalized and gives it a rest until the next cycle.
- In the `finalize()` method you can technically _resurrect_ an object by assigning it to a `static` field. The object would become alive again and non-eligible for GCn, so the GC would not collect it during the next cycle.
- The object, however, would be marked as finalized, so when it would become eligible again, the `finalize` method would not be called. In essence, you can turn this _resurrection_ trick only once for the lifetime of the object. Beware that this should be used only if you really know what you're doing.

### The difference between Serial and Parallel GC?

- **Serial GC**

  - Serial GC works by holding all the application threads.
  - It is designed for the single-threaded environments.
  - It uses just a single thread for GCn.
  - The way it works by freezing all the application threads while doing GCn may not be suitable for a server environment.
  - It is best suited for simple command-line programs.
  - Turn on the `-XX:+UseSerialGC` JVM argument to use the serial GC.

- **Parallel GC**
  - Parallel GC is also called as throughput collector.
  - It is the default GC of the JVM.
  - Unlike serial GC, this uses multiple threads for GCn.
  - Similar to serial GC this also freezes all the application threads while performing GCn.

### Ways to make an object eligible for GC

- Even though programmer is not responsible to destroy useless objects but it is highly recommended to make an object unreachable (thus eligible for GC) if it is no longer required.
- There are generally five different ways to make an object eligible for GCn.
  1. **Nullifying the reference variable** : When all the reference variables of an object are changed to `null`, it becomes unreachable and thus becomes eligible for GCn.
  2. **Re-assigning the reference variable** : When reference id of one object is referenced to reference id of some other object then the previous object has no any longer reference to it and becomes unreachable and thus becomes eligible for GCn.
  3. **Object created inside method** : When a method is called it goes inside the stack frame. When the method is popped from the stack, all its members die and if some objects were created inside it then these objects become unreachable or anonymous after method execution and thus eligible for GCn
  4. **Island of Isolation** : Object 1 references Object 2 and Object 2 references Object 1. Neither Object 1 nor Object 2 is referenced by any other object. So, this is a group of objects that reference each other but they are not referenced by any active object in the application. That’s an island of isolation. They are in an isolation to active world and hence ready to be garbage collected.
  5. **Anonymous object** : The reference id of an anonymous object is not stored anywhere. Hence, it becomes unreachable.

### Explain JMM and Java GCn in that?

![JMM](/images/jmm.png)
![JMM2](/images/jmm2.png)

- Once we launch the JVM, the OS allocates memory for the process. Here, the JVM itself is a process, and the memory allocated to that process includes the Heap, Meta Space, JIT code cache, thread stacks, and shared libraries. We call this native memory.
- **Native Memory** is the memory provided to the process by the OS. How much memory the OS allocates to the Java process depends on the OS, processor, and the JRE.
  - **Heap Memory** : JVM uses this memory to store objects. This memory is in turn split into two different areas called the **Young Generation Space** and “**Tenured Space**“.
    - **Young Generation** : Young generation is the place where all the new objects are created. When young generation is filled, GCn is performed. This GCn is called Minor GC. The Young Generation or the New Space is divided into two portions called **Eden Space** and **Survivor Space**.
      - **Eden Space** : When we create an object, the memory will be allocated from the Eden Space.
      - **Survivor Space** : This contains the objects that have survived from the Young GCn or Minor GCn. We have two equally divided survivor spaces called S0 and S1.
    - **Tenured Space** : The objects which reach to max tenured threshold during the minor GC or young GC, will be moved to **Tenured Space** or **Old Generation Space**“ Old Generation memory contains the objects that are long lived and survived after many rounds of Minor GC. Usually GCn is performed in Old Generation memory when it’s full. Old Generation GCn is called Major GC and usually takes longer time.
  - **Meta Space** : This memory is out of heap memory and part of the native memory. By default the meta space doesn’t have an upper limit and the space is used to store the class definitions loaded by class loaders. It is designed to grow in order to avoid out of memory errors. However, if it grows more than the available physical memory, then the OS will use virtual memory. This will have an adverse effect on application performance, as swapping the data from virtual memory to physical memory and vice versa is a costly operation. We have JVM options to limit the Meta Space used by the JVM. In that case, we may get out of memory errors.
    - Two new flags can be used to set the size of the metaspace, they are: **-XX:MetaspaceSize** and **-XX:MaxMetaspaceSize**.
- **Code Cache**: JVM has an interpreter to interpret the byte code and convert it into hardware dependent machine code. The frequently accessed code blocks will be compiled to native code by the JIT and stored in code cache. The JIT compiled code will not be interpreted.
- **Method Area** is part of space in **Metaspace** and used to store class structure (runtime constants and static variables) and code for methods and constructors.
- **Memory Pools** are created by JVM memory managers to create a pool of immutable objects, if implementation supports it. String Pool is a good example of this kind of memory pool. Memory Pool can belong to Heap or Metaspace, depending on the JVM memory manager implementation.
- **Runtime constant pool** is per-class runtime representation of constant pool in a class. It contains class runtime constants and static methods. Runtime constant pool is the part of method area.
- **Java Stack memory** is used for execution of a thread. They contain method specific values that are short-lived and references to other objects in the heap that are getting referred from the method. Stack memory is always referenced in LIFO order. Whenever a method is invoked, a new block is created in the stack memory for the method to hold local primitive values and reference to other objects in the method. As soon as method ends, the block becomes unused and available for next method.

### Stop the world event

- All the GCns are Stop the World events because all application threads are stopped until the operation completes.
- However Major GC takes longer time because it checks all the live objects. Major GC should be minimized because it will make your application unresponsive for the GCn duration.
- The duration taken by GC depends on the strategy used for GCn. That’s why it’s necessary to monitor and tune the GC to avoid timeouts in the highly responsive applications.
- If minor GC triggers several times, eventually the **Tenured Space** will be filled up and will require more GCn. During this time the JVM triggers a **major GC**. Sometimes we call this full GC. But, as part of full GC, the JVM reclaims the memory from Meta Space. If there are no objects in the heap, then the loaded classes will be removed from the meta space.
- One of the basic way of GCn involves three steps:
  - **Marking** : This is the first step where GC identifies which objects are in use and which ones are not in use
  - **Normal Deletion** : GC removes the unused objects and reclaims the free space to be allocated to other objects
  - **Deletion with compacting** : For better performance, after deleting unused objects, all the survived objects can be moved to be together. This will increase the performance of allocation of memory to newer objects

### Java GCn Types

- There are five types of GCn types that we can use in our applications. We just need to use JVM switch to enable the GCn strategy for the application.
  1. **Serial GC (-XX:+UseSerialGC)**: Serial GC uses the simple mark-sweep-compact approach for young and old generations GCn that is, Minor and Major GC
  2. **Parallel GC (-XX:+UseParallelGC)**: Parallel GC is same as Serial GC except that, it spawns N threads for young generation GCn where N is the number of CPU cores in the system. We can control the number of threads using **–XX:ParallelGCThreads=n** JVM option
  3. **Parallel Old GC (-XX:+UseParallelOldGC)**: This is same as Parallel GC except that it uses multiple threads for both young generation and old generation GCn
  4. **Concurrent Mark Sweep (CMS) Collector (-XX:+UseConcMarkSweepGC)**: CMS is also referred as concurrent low pause collector. It does the GCn for old generation. CMS collector tries to minimize the pauses due to GCn by doing most of the GCn work concurrently within the application threads. CMS collector on young generation uses the same algorithm as that of the parallel collector. This GC is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using **–XX:ParallelCMSThreads=n**
  5. **G1 GC (-XX:+UseG1GC)**: The garbage first or G1 GC is available from Java 7 and its long term goal is to replace the CMS collector. The G1 collector is a parallel, concurrent and incrementally compact low-pause GC. Garbage first collector doesn’t work like other collectors and there is no concept of young and old generation space. It divides the heap space into multiple equal-sized heap regions. When a GC is invoked, it first collects the region with lesser live data, hence Garbage First

### Describe strong, weak, soft and phantom references and their role in GCn.

- It is possible to influence how GC occurs as regards to the objects we have created.
- Java provides us with reference objects to control the relationship between the objects we create and the GC.
- After calling the above line, the object will then be eligible for collection.
- We can change this relationship between the object and the GC by explicitly wrapping it inside another reference object which is present in `java.lang.ref` package.
- **Strong References**: is the default type/class of Reference Object. Any object which has an active strong reference are not eligible for GCn. The object is garbage collected only when the variable which was strongly referenced points to null.
  - `StringBuilder sb = new StringBuilder()`
  - The variable `sb` stores a **strong reference** to `StringBuilder`. What this means for the GC is that the particular `StringBuilder` object is not eligible for collection at all due to a strong reference held to it by `sb`. The story only changes when we nullify `sb` like this: `sb = null;`
- When the JVM is on the brink of throwing an _OutOfMemory_ error, **objects with only soft references are collected as a last resort** to recover memory.
- A **weak reference** can be created in a similar manner using _WeakReference_ class. When `sb` is set to null and the `StringBuilder` object only has a weak reference, the JVM's GC will have absolutely no compromise and immediately collect the object at the very next cycle.
- A **phantom reference** is similar to a weak reference and an object with only phantom references will be collected without waiting. However, phantom references are enqueued as soon as their objects are collected.
- We can poll the reference queue to know exactly when the object was collected. The objects which are being referenced by phantom references are eligible for GCn. But, before removing them from the memory, JVM puts them in a queue called _reference queue_. They are put in a reference queue after calling finalize() method on them.

- The only times `finally` won't be called are:
  - If you invoke System.exit()
  - If you invoke Runtime.getRuntime().halt(exitStatus)
  - If the JVM crashes first
  - If the JVM reaches an infinite loop (or some other non-interruptable, non-terminating statement) in the try or catch block
  - If the OS forcibly terminates the JVM process; e.g., kill -9 <pid> on UNIX
  - If the host system dies; e.g., power failure, hardware error, OS panic, et cetera
  - If the finally block is going to be executed by a daemon thread and all other non-daemon threads exit before finally is called