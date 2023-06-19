### What does the statement "memory is managed in Java" mean?

- Memory is the key resource an application requires to run effectively and linke an resource, it is scarce. As such, its allocation and deallocation to an from applications or different parts o an application require a lot of care and consideration.
- However, in Java, a developer doesn't need to explicitly allocate and deallocate memory - the JVM and more specifically the GC - has the duty of handling memory allocation so that the developer doesn't have to.
- This is contrary to what happens in languages like C where a programmer has direct access to memory and literally references memory cells in his code, creating a lot of room for memory leaks.

### What is GC and what are its advantages?

- GC is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects.
- GC is the act of reclaiming memory by removing all the unreachable pointers (e.g. ref types that are no longer in scope)
- An in-use object, or a referenced object, means that some part of your program still maintains a pointer to that object. An unused object, or unreferenced object, is no longer referenced by any part of your program. So the memory used by an unreferenced object can be reclaimed.
- It does this using a **mark and sweep** approach. It first enumerates all the roots and then starts visiting the objects referenced by them recursively (essentially travelling the nodes in the memory graph). When it reaches an object it marks it with a special flag indicating that the object is reachable and hence not garbage. At the end of this mark phase it gets into the sweep phase. Any object in memory that is not marked by this time is garbage and the system disposes it.
- GC occurs when the system starts running out of memory (or some other such trigger)
- The biggest advantage of GC is that it removes the burden of manual memory allocation/deallocation from us so that we can focus on solving the problem at hand.

### Where is GC started (root set of references):

- currently executing method's arguments and local variables that are referencing objects, static reference variables

### Are there any disadvantages to GC?

- Yes. Whenever the garbage collector runs, it has an effect on the application's performance. This is because all other threads in the application have to be stopped to allow the garbage collector thread to effectively do its work.
- Depending on the requirements of application, this can be a real problem that is unacceptable by the client. However, this problem can be greatly reduced or even eliminated through skillful optimization and garbage collector tuning and using different GC algorithms.

### What is the meaning of the term "stop-the-world"?

- When the garbage collector thread is running, other threads are stopped, meaning the application is stopped momentarily. This is analogous to house cleaning or fumigation where occupants are denied access until the process is complete.
- Depending on the needs of an application, "stop the world" garbage collection can cause an unaccepatable freeze. This is why it is important to do garbage collector tuning and JVM optimization so that the freeze encountered is at least acceptable.

### What is generational garbage collection and what makes it a popular garbage collection approach?

- Generational garbage collection can be loosely defined as the strategy used by the garbage collector where the heap is divided into a number of sections called generations, each of which will hold objects according to their “age” on the heap.
- Whenever the garbage collector is running, the first step in the process is called marking. This is where the garbage collector identifies which pieces of memory are in use and which are not. This can be a very time-consuming process if all objects in a system must be scanned.
- As more and more objects are allocated, the list of objects grows and grows leading to longer and longer garbage collection time. However, empirical analysis of applications has shown that most objects are short-lived.
- With generational garbage collection, objects are grouped according to their “age” in terms of how many garbage collection cycles they have survived. This way, the bulk of the work spread across various minor and major collection cycles
- Today, almost all garbage collectors are generational. This strategy is so popular because, over time, it has proven to be the optimal solution.

### Describe in detail how generational garbage collection works

- To properly understand how generational garbage collection works, it is important to first remember how Java heap is structured to facilitate generational garbage collection.
- The heap is divided up into smaller spaces or generations. These spaces are Young Generation, Old or Tenured Generation, and Permanent Generation.
- The **young generation hosts most of the newly created objects**. An empirical study of most applications shows that majority of objects are quickly short lived and therefore, soon become eligible for collection. Therefore, new objects start their journey here and are only “promoted” to the old generation space after they have attained a certain “age”.
- The term **age** in generational garbage collection refers to the number of collection cycles the object has survived.
- The young generation space is further divided into three spaces: an Eden space and two survivor spaces such as Survivor 1 (s1) and Survivor 2 (s2).
- The old generation hosts objects that have lived in memory longer than a certain “age”. The objects that survived garbage collection from the young generation are promoted to this space. It is generally larger than the young generation. As it is bigger in size, the garbage collection is more expensive and occurs less frequently than in the young generation.
- The permanent generation or more commonly called, _PermGen_, contains metadata required by the JVM to describe the classes and methods used in the application. It also contains the string pool for storing interned strings. It is populated by the JVM at runtime based on classes in use by the application. In addition, platform library classes and methods may be stored here.
- First, any new objects are allocated to the Eden space. Both survivor spaces start out empty. When the Eden space fills up, a minor garbage collection is triggered. Referenced objects are moved to the first survivor space. Unreferenced objects are deleted.
- During the next minor GC, the same thing happens to the Eden space. Unreferenced objects are deleted and referenced objects are moved to a survivor space. However, in this case, they are moved to the second survivor space (S1).
- In addition, objects from the last minor GC in the first survivor space (S0) have their age incremented and are moved to S1. Once all surviving objects have been moved to S1, both S0 and Eden space are cleared. At this point, Si contains objects with different ages.
- At the next minor GC, the same process is repeated. However this time the survivor spaces switch. Referenced objects are moved to S0 from both Eden and S1. Surviving objects are aged. Eden and S1 are cleared.
- After every minor garbage collection cycle, the age of each object is checked. Those that have reached a certain arbitrary age, for example, 8, are promoted from the young generation to the old or tenured generation. For all subsequent minor GC cycles, objects will continue to be promoted to the old generation space.
- This pretty much exhausts the process of garbage collection in the young generation. Eventually, a major garbage collection will be performed on the old generation which cleans up and compacts that space. For each major GC, there are several minor GCs

### When does an object become eligible for garbage collection? Describe how the GC collects an eligible object?

- An object becomes eligible for Garbage collection or GC if it is not reachable from any live threads or by any static references.
- The most straightforward case of an object becoming eligible for garbage collection is if all its references are null. Cyclic dependencies without any live external reference are also eligible for GC. So if object A references object B and object B references Object A and they don't have any other live reference then both Objects A and B will be eligible for Garbage collection.
- Another obvious case is when a parent object is set to null. When a kitchen object internally references a fridge object and a sink object, and the kitchen object is set to null, both fridge and sink will become eligible for garbage collection alongside their parent, kitchen.

### How do you trigger garbage collection from Java code?

- **One can not force garbage collection in Java**; it will only trigger if JVM thinks it needs a garbage collection based on Java heap size.
- Before removing an object from memory garbage collection thread invokes `finalize()` method of that object and gives an opportunity to perform any sort of cleanup required. You can also invoke this method of an object code, however, there is no guarantee that garbage collection will occur when you call this method
- Additionally, there are methods like `System.gc()` and `Runtime.gc()` which is used to send request of garbage collection to JVM but it's not guaranteed that garbage collection will happen.

### What happens when there is not enough heap space to accommodate storage of new objects?

- If there is no memory space for creating a new object in Heap, Java Virtual Machine throws _OutOfMemoryError_.

### Is it possible to resurrect an object that became eligible for garbage collection?

- When an object becomes eligible for garbage collection, the GC has to run the `finalize()` method on it. The `finalize()` method is guaranteed to run only once, thus the GC flags the object as finalized and gives it a rest until the next cycle.
- In the `finalize()` method you can technically “resurrect” an object, for example, by assigning it to a `static` field. The object would become alive again and non-eligible for garbage collection, so the GC would not collect it during the next cycle.
- The object, however, would be marked as finalized, so when it would become eligible again, the `finalize` method would not be called. In essence, you can turn this “resurrection” trick only once for the lifetime of the object. Beware that this ugly hack should be used only if you really know what you're doing — however, understanding this trick gives some insight into how the GC works.

### Suppose we have a circular reference (two objects that reference each other). Could such pair of objects become eligible for garbage collection and why?

- Yes, a pair of objects with circular reference can become eligible for garbage collection. This is because of how Java’s garbage collector handles circular references. It considers objects live not when they have any reference to them, but when they are reachable by navigating the object graph starting from some garbage collection root (a local variable of a live thread or a static field). If a pair of objects with a circular reference is not reachable from any root, it is considered eligible for garbage collection.

### The difference between Serial and Parallel Garbage Collector?

- **Serial Garbage Collector**

  - Serial garbage collector works by holding all the application threads.
  - It is designed for the single-threaded environments.
  - It uses just a single thread for garbage collection.
  - The way it works by freezing all the application threads while doing garbage collection may not be suitable for a server environment.
  - It is best suited for simple command-line programs.
  - Turn on the `-XX:+UseSerialGC` JVM argument to use the serial garbage collector.

- **Parallel Garbage Collector**
  - Parallel garbage collector is also called as throughput collector.
  - It is the default garbage collector of the JVM.
  - Unlike serial garbage collector, this uses multiple threads for garbage collection.
  - Similar to serial garbage collector this also freezes all the application threads while performing garbage collection.

### Ways to make an object eligible for GC

- Even though programmer is not responsible to destroy useless objects but it is highly recommended to make an object unreachable (thus eligible for GC) if it is no longer required.
- There are generally five different ways to make an object eligible for garbage collection.
  1. **Nullifying the reference variable** : When all the reference variables of an object are changed to NULL, it becomes unreachable and thus becomes eligible for garbage collection.
  2. **Re-assigning the reference variable** : When reference id of one object is referenced to reference id of some other object then the previous object has no any longer reference to it and becomes unreachable and thus becomes eligible for garbage collection.
  3. **Object created inside method** : When a method is called it goes inside the stack frame. When the method is popped from the stack, all its members dies and if some objects were created inside it then these objects becomes unreachable or anonymous after method execution and thus becomes eligible for garbage collection
  4. **Island of Isolation** : Object 1 references Object 2 and Object 2 references Object 1. Neither Object 1 nor Object 2 is referenced by any other object. So, this is a group of objects that reference each other but they are not referenced by any active object in the application. That’s an island of isolation. They are in an isolation to active world and hence ready to be garbage collected.
  5. **Anonymous object** : The reference id of an anonymous object is not stored anywhere. Hence, it becomes unreachable.

### Explain JMM and Java Garbage Collection in that?

![JMM](assets/jmm.png)
![JMM2](assets/jmm2.png)

- Once we launch the JVM, the OS allocates memory for the process. Here, the JVM itself is a process, and the memory allocated to that process includes the Heap, Meta Space, JIT code cache, thread stacks, and shared libraries. We call this native memory.
- **Native Memory** is the memory provided to the process by the OS. How much memory the OS allocates to the Java process depends on the OS, processor, and the JRE. Different memory blocks available for the JVM.
  - **Heap Memory** : JVM uses this memory to store objects. This memory is in turn split into two different areas called the “**Young Generation Space**” and “**Tenured Space**“.
    - **Young Generation** : Young generation is the place where all the new objects are created. When young generation is filled, garbage collection is performed. This garbage collection is called Minor GC. The Young Generation or the New Space is divided into two portions called **Eden Space** and **Survivor Space**.
      - **Eden Space** : When we create an object, the memory will be allocated from the Eden Space.
      - **Survivor Space** : This contains the objects that have survived from the Young garbage collection or Minor garbage collection. We have two equally divided survivor spaces called S0 and S1.
    - **Tenured Space** : The objects which reach to max tenured threshold during the minor GC or young GC, will be moved to “**Tenured Space**” or “**Old Generation Space**“. Old Generation memory contains the objects that are long lived and survived after many rounds of Minor GC. Usually garbage collection is performed in Old Generation memory when it’s full. Old Generation Garbage Collection is called Major GC and usually takes longer time.
  - **Meta Space** : This memory is out of heap memory and part of the native memory. As per the document by default the meta space doesn’t have an upper limit. In earlier versions of Java we called this “**Perm Gen Space**". This space is used to store the class definitions loaded by class loaders. This is designed to grow in order to avoid out of memory errors. However, if it grows more than the available physical memory, then the OS will use virtual memory. This will have an adverse effect on application performance, as swapping the data from virtual memory to physical memory and vice versa is a costly operation. We have JVM options to limit the Meta Space used by the JVM. In that case, we may get out of memory errors. Two new flags can be used to set the size of the metaspace, they are: **-XX:MetaspaceSize**” and “**-XX:MaxMetaspaceSize**”. The theme behind the Metaspace is that the lifetime of classes and their metadata matches the lifetime of the classloaders. That is, as long as the classloader is alive, the metadata remains alive in the Metaspace and can’t be freed.
- **Code Cache**: JVM has an interpreter to interpret the byte code and convert it into hardware dependent machine code. As part of JVM optimization, JIT compiler has been introduced. The frequently accessed code blocks will be compiled to native code by the JIT and stored in code cache. The JIT compiled code will not be interpreted.

- **Method Area** is part of space in the Perm Gen and used to store class structure (runtime constants and static variables) and code for methods and constructors.
- **Memory Pools** are created by JVM memory managers to create a pool of immutable objects, if implementation supports it. String Pool is a good example of this kind of memory pool. Memory Pool can belong to Heap or Perm Gen, depending on the JVM memory manager implementation.
- **Runtime constant pool** is per-class runtime representation of constant pool in a class. It contains class runtime constants and static methods. Runtime constant pool is the part of method area.
- **Java Stack memory** is used for execution of a thread. They contain method specific values that are short-lived and references to other objects in the heap that are getting referred from the method. Stack memory is always referenced in LIFO (Last-In-First-Out) order. Whenever a method is invoked, a new block is created in the stack memory for the method to hold local primitive values and reference to other objects in the method. As soon as method ends, the block becomes unused and become available for next method.

### Stop the world event

- All the Garbage Collections are “Stop the World” events because all application threads are stopped until the operation completes.
- Since Young generation keeps short-lived objects, Minor GC is very fast and the application doesn’t get affected by this.
- However Major GC takes longer time because it checks all the live objects. Major GC should be minimized because it will make your application unresponsive for the garbage collection duration. So if you have a responsive application and there are a lot of Major Garbage Collection happening, you will notice timeout errors.
- The duration taken by garbage collector depends on the strategy used for garbage collection. That’s why it’s necessary to monitor and tune the garbage collector to avoid timeouts in the highly responsive applications.
- If minor GC triggers several times, eventually the **Tenured Space** will be filled up and will require more garbage collection. During this time the JVM triggers a “**major GC**” event. Some times we call this full GC. But, as part of full GC, the JVM reclaims the memory from “Meta Space”. If there are no objects in the heap, then the loaded classes will be removed from the meta space.
- One of the basic way of garbage collection involves three steps:
  - **Marking** : This is the first step where garbage collector identifies which objects are in use and which ones are not in use
  - **Normal Deletion** : Garbage collector removes the unused objects and reclaims the free space to be allocated to other objects
  - **Deletion with compacting** : For better performance, after deleting unused objects, all the survived objects can be moved to be together. This will increase the performance of allocation of memory to newer objects

### Mark and Sweep Model of Garbage collection

- JVM uses the mark and sweep garbage collection model for performing garbage collection of the whole heap. A mark and sweep garbage collection consists of two phases, the mark phase and the sweep phase.
- During the mark phase, all the objects that are reachable from Java threads, native handlers and other root sources are marked as alive, as well as the objects that are reachable from these objects and so forth. This process identifies and marks all objects that are still used, and the rest can be considered garbage.
- During the sweep phase, the heap is traversed to find the gaps between the live objects. These gaps are recorded in a free list and are made available for new object allocation.

### Java Garbage Collection Types

- There are five types of garbage collection types that we can use in our applications. We just need to use JVM switch to enable the garbage collection strategy for the application.
  1. **Serial GC (-XX:+UseSerialGC)**: Serial GC uses the simple mark-sweep-compact approach for young and old generations garbage collection that is, Minor and Major GC
  2. **Parallel GC (-XX:+UseParallelGC)**: Parallel GC is same as Serial GC except that, it spawns N threads for young generation garbage collection where N is the number of CPU cores in the system. We can control the number of threads using **–XX:ParallelGCThreads=n** JVM option
  3. **Parallel Old GC (-XX:+UseParallelOldGC)**: This is same as Parallel GC except that it uses multiple threads for both young generation and old generation garbage collection
  4. **Concurrent Mark Sweep (CMS) Collector (-XX:+UseConcMarkSweepGC)**: CMS is also referred as concurrent low pause collector. It does the garbage collection for old generation. CMS collector tries to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently within the application threads. CMS collector on young generation uses the same algorithm as that of the parallel collector. This garbage collector is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using **–XX:ParallelCMSThreads=n** JVM option
  5. **G1 Garbage Collector (-XX:+UseG1GC)**: The garbage first or G1 Garbage Collector is available from Java 7 and its long term goal is to replace the CMS collector. The G1 collector is a parallel, concurrent and incrementally compact low-pause garbage collector. Garbage first collector doesn’t work like other collectors and there is no concept of young and old generation space. It divides the heap space into multiple equal-sized heap regions. When a garbage collector is invoked, it first collects the region with lesser live data, hence “Garbage First”

### Describe strong, weak, soft and phantom references and their role in garbage collection.

- Much as memory is managed in Java, an engineer may need to perform as much optimization as possible to minimize latency and maximize throughput, in critical applications. Much as it is impossible to explicitly control when garbage collection is triggered in the JVM, it is possible to influence how it occurs as regards the objects we have created.
- Java provides us with reference objects to control the relationship between the objects we create and the garbage collector.
- By default, every object we create in a Java program is strongly referenced by a variable: `StringBuilder sb = new StringBuilder();`
- In the above snippet, the _new_ keyword creates a new `StringBuilder` object and stores it on the heap. The variable `sb` then stores a **strong reference** to this object. What this means for the garbage collector is that the particular `StringBuilder` object is not eligible for collection at all due to a strong reference held to it by `sb`. The story only changes when we nullify sb like this: `sb = null;`
- After calling the above line, the object will then be eligible for collection.
- We can change this relationship between the object and the garbage collector by explicitly wrapping it inside another reference object which is present in `java.lang.ref` package.

- **Strong References**: This is the default type/class of Reference Object. Any object which has an active strong reference are not eligible for garbage collection. The object is garbage collected only when the variable which was strongly referenced points to null.

```java
MyClass obj = new MyClass();
```

- A **soft reference** can be created to the above object like this:

```java
//Java Code to illustrate Soft reference
import java.lang.ref.SoftReference;
class MainClass
{
    public void message() {
        System.out.println("Weak References Example");
    }
}

public class Example
{
    public static void main(String[] args) {
        // Strong Reference
        MainClass g = new MainClass();
        g.message();

        // Creating Soft Reference to MainClass-type object to which 'g'  is also pointing.
        SoftReference<MainClass> softref = new SoftReference<MainClass>(g);
        g = null;
        g = softref.get();
        g.message();
    }
}
```

- The story will only change when memory becomes tight and the JVM is on the brink of throwing an _OutOfMemory_ error. In other words, **objects with only soft references are collected as a last resort** to recover memory.

- A **weak reference** can be created in a similar manner using _WeakReference_ class. When `sb` is set to null and the `StringBuilder` object only has a weak reference, the JVM's garbage collector will have absolutely no compromise and immediately collect the object at the very next cycle.

```java
//Java Code to illustrate Weak reference
import java.lang.ref.WeakReference;
class MainClass
{
    public void message() {
        System.out.println("Weak References Example");
    }
}

public class Example
{
    public static void main(String[] args) {
        // Strong Reference
        MainClass g = new MainClass();
        g.message();

        // Creating Weak Reference to MainClass-type object to which 'g'
        // is also pointing.
        WeakReference<MainClass> weakref = new WeakReference<MainClass>(g);
        g = null;
        g = weakref.get();
        g.message();
    }
}
```

- A **phantom reference** is similar to a weak reference and an object with only phantom references will be collected without waiting. However, phantom references are enqueued as soon as their objects are collected. We can poll the reference queue to know exactly when the object was collected. The objects which are being referenced by phantom references are eligible for garbage collection. But, before removing them from the memory, JVM puts them in a queue called ‘reference queue’ . They are put in a reference queue after calling finalize() method on them. To create such references java.lang.ref.PhantomReference class is used.

```java
//Java Code to illustrate Phantom reference
import java.lang.ref.*;
class MainClass
{
    public void message() {
        System.out.println("Phantom References Example");
    }
}

public class Example
{
    public static void main(String[] args) {
        // Strong Reference
        MainClass g = new MainClass();
        g.message();

        // Creating Phantom Reference to MainClass-type object to which 'g' is also pointing.
        PhantomReference<MainClass> phantomRef = null;
        phantomRef = new PhantomReference<MainClass>(g,refQueue);
        g = null;
        g = phantomRef.get();
        g.message();
    }
}
```
