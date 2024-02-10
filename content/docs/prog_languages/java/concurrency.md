### Notes on Java Concurrency

- In concurrent programming, there are two basic units of execution: _processes_ and _threads_.
- A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.
- Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process. Threads exist within a process — every process has at least one
- A _process_ runs independently and isolated of other processes. It cannot directly access shared data in other processes. The resources of the process, e.g. memory and CPU time, are allocated to it via the operating system.
- A _thread_ is a so called lightweight process. It has its own call stack, but can access shared data of other threads in the same process. Every thread has its own memory cache. If a thread reads shared data, it stores this data in its own memory cache.
- In Java we can create processes with the help of `ProcessBuilder` class.
- Each process has its own registers, PC, stack memory, heap memory assigned by the OS.
- A given process can contain multiple threads. And each thread shares the resources of process.
- Every thread has its own stack memory but all threads share the heap memory (shared memory space)
- Every object in Java has a so-called intrinsic lock (Monitor)
- A thread that needs exclusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them.
- The Problem is that every object has a single monitor lock.
- If we have 2 independent **synchronized** methods then the threads have to wait for each other to release the lock.
- A thread **cannot acquire a lock owned by another thread**. But a given thread **can acquire a lock that it already owns**. Allowing a thread to acquire the same lock more than once is called _re-entrant synchronization_. For instance, if a given thread calls a recursive and synchronized method several times then it is fine.
- Object level lock is a mechanism when we want to synchronize a non-static method or non-static code block such that only one thread will be able to execute the code block on given instance of the class. This should always be done to make instance level data thread safe.
- Class level lock prevents multiple threads to enter in synchronized block in any of the all available instances of the class on runtime. Class level locking should always be done to make static data thread safe.
- `wait` and `notify` must happen in a synchronized block on the monitor object whereas `sleep` does not.
- `wait` and `notify` can be interrupted where as `sleep` cannot
- Java doesn't notify the other thread immediately.
- `ReentrantLock`
  - It has the same behaviour as the "synchronized approach" with some extended features
  - `new ReentrantLock(boolean fairnessParameter)`
    - fairness parameter : if true, then the longest waiting thread will get the lock. If false, then there is no access order !!!
- We can make a lock fair : **prevent thread starvation**. synchronized blocks are **unfair** by default.
- We can check whether the given lock is held or not with reentrant locks.
- We can get the **list of threads waiting** for the given lock with reentrant locks.
- synchronized blocks are nicer : we don't need the try-catch-finally block
- Every read of a `volatile` variable will be read from RAM and not from cache.
- Deadlock occurs when two or more threads wait forever for a lock or resource held by another thread.
- Livelock threads are unable to make further progress. They are too busy responding to each other to resume work.
- How to handle deadlock and livelock
  - We should ensure that a thread doesn't block infinitely if it is unable to acquire a lock. This is why using `Lock` interface's `tryLock()` method is convenient
  - We should ensure that each thread acquires the locks in the same order to avoid any cyclic dependency in lock acquisition.
  - Livelock can be handled with the methods above and some randomness.
- Semaphore maintains a set of permits.
  - `acquire()` -> if a permit is available then takes it
  - `release()` -> adds a permit
  - Semaphore just keeps count of the number available. `new Semaphore(int permits, boolean fair)`
- Java provides its own multi-threading framework the so-called **Executor Framework**.
- With the help of this framework we can manage worker threads more efficiently because of **thread pools**.
- Thread pools can reuse threads by keeping the threads alive and reusing them (thread pools are usually **queues**)
- There are 4 types of executors
  - `SingleThreadExecutor` : This executor has a single thread so we can execute processes in a sequential manner. Every process is executed by a new thread.
  - `FixedThreadPool(n)` : This is how we can create a thread pool with **n** threads. Usually `n` is the number of cores in CPU. If there's more tasks than `n`, then these tasks are stored with a **LinkedBlockingQueue**
  - `CachedThreadPool` : The number of threads is not bounded - if all threads are busy and a new task comes the pool will create and add a new thread to the executor. If a thread remains idle for **60** secs then it is removed. It is used for short parallel tasks.
  - `ScheduledExecutor` : We can execute a given operation at regular intervals or we can use this executor if we wish to delay a certain task
- `Runnable` and `Callable` both run on different threads than the calling thread but `Callable` can return a value and `Runnable` can not.
- `Runnable` : a so-called _run-and-forget_ action. We execute a given operation in run() method without a return value
- `Callable<T>` : We use `call()` method if we want to return a given value from the given thread
  - `Callable` interface will not return the value : this is why we need `Future<T>`
  - Calling thread will be blocked till the `call()` method is executed and `Future<T>` returns the results.
  - `T` is the expected return type of `Callable`
- `executorService.submit()` can handle `Runnable` and `Callable` both.
- For parallel algorithms : we have to take the communication between threads into consideration.
- We have to make sure we split the work evenly amongst the processors.
- fork-join framework is the concrete implementation for parallel execution.
- A larger task -> it can be divided into smaller ones + the subsolutions can be combined.
- IMPORTANT subtasks have to be independent in order to be executed in parallel
- fork-join frameworks break the task into smaller subtasks until these subtasks are simple enough to solve without further breakups.
- `RecursiveTask<T>` will return a `T` type. All the tasks we want to execute in parallel is a subclass of this class. Override the `compute` method that will return the solution of the subproblem
- `RecursiveAction` : it is a task, but w/o any return value
- `ForkJoinPool` : it is a thread pool for executing fork-join tasks.
- MapReduce is a programming model : a way of structuring the computation that allows it easily to be run on lots of nodes (servers)
- The algorithm has three steps:
  - _map_: splits the original dataset
  - _shuffle & sort_ : all the data is rearranged to be run in parallel. It makes sure that items with same keys will get to the same reducer
  - _reduce_ : combines the final results

### IBM Docs

- `CopyOnWriteArrayList` is a "thread-safe" variant of `ArrayList` in which all mutative operations (add, set, and so on) are implemented by making a fresh copy of the array.
- `CopyOnWriteArrayList` is ideal for read-often, write-rarely collections such as `Listeners` for a JavaBean event
- `BlockingQueue` neatly solves the problem of how to "hand off" items gathered by one thread to another thread for processing, without explicit concern for synchronization issues.
- `BlockingQueue` also supports methods that take a time parameter, indicating how long the thread should block before returning to signal failure to insert or retrieve the item in question.
- `SynchronousQueue` is a blocking queue in which each insert operation must wait for a corresponding remove operation by another thread, and vice versa. It gives us an extremely lightweight way to exchange single elements from one thread to another.
- `SynchronousQueue` will allow an insert into the queue only if there is a thread waiting to consume it.
- `CountDownLatch` holds all threads at bay until a particular condition is met, at which point it releases them all at once.
- reading a volatile variable is synchronized and writing to a volatile variable is synchronized, but non-atomic operations are not. what this means is the following code is not thread-safe : `myVolatileVar++` _look deeper into it_
- The `synchronized` keyword is not considered to be part of a method's signature. So the synchronized modifier is not automatically inherited when subclasses override superclass methods, and methods in interfaces cannot be declared as `synchronized`.
- Also, constructors cannot be qualified as `synchronized` (although block synchronization can be used within constructors)

### Java Multithreading

- The theoretical possible performance gain can be calculated by _Amdahl's law_
- **Concurrency issues** : There are two basic problems, visibility and access problems.
- A _visibility_ problem occurs if thread A reads shared data which is later changed by thread B and thread A is unaware of this change.
- An _access_ problem can occur if several threads access and changes the same shared data at the same time.
- Visibility and access problem can lead to:
  - _Liveness failure_ : The program doesn't react anymore , e.g. deadlocks.
  - _Safety failure_ : The program creates incorrect data.
- If a variable is declared with _volatile_ keyword then it is guaranteed that any thread that reads the field will see the most recently written value.
- The _volatile_ keyword will not perform any mutual exclusive lock on the variable.
- An _atomic operation_ is an operation which is performed as a single unit of work without the possibility of interference from other operations.
- There are two basic strategies for using Thread objects to create a concurrent application.
  - To directly control thread creation and management, simply instantiate `Thread` each time the application needs to initiate an asynchronous task.
  - To abstract thread management from the rest of your application, pass the application's tasks to an _executor_
- An application that creates an instance of Thread must provide the code that will run in that thread. There are two ways to do this:
  - _Provide a `Runnable` object._ The `Runnable` interface defines a single method, `run`, meant to contain the code executed in the thread. The `Runnable` object is passed to the Thread constructor.
  - _Subclass `Thread`_ An application can subclass Thread, providing its own implementation of run.
- The idiom, which employs a `Runnable` object is more general because a `Runnable` object can subclass a class other than `Thread`.
- The `Runnable` is the task to perform and `Thread` is the worker doing this task.
- Thread pools manage a pool of worker threads. The thread pools contain a work queue which holds tasks waiting to be executed.
- A thread pool can be described as a collection of `Runnable` objects (work queue) and a connection of running threads.
- These threads are constantly running and are checking the work queue for new work. If there is new work to be done they execute this `Runnable`.
- The Executor framework provides example implementation of the `java.util.concurrent.Executor` interface, e.g. `Executors.newFixedThreadPool(int n)` which will create `n` worker threads.
- A compare-and-swap (CAS) operation checks if a variable has a certain value and if it has that value it will perform an operation.
- The fork-join framework allows you to distribute a certain task on several workers and then wait for the result.
- One can also define a thread by forming a subclass of the `Thread` class. Not recommended
- You should decouple the _task_ that is to be run in parallel from the _mechanism_ of running it
- Threads can be in one of six states:
  - New
  - Runnable
  - Blocked
  - Waiting
  - Timed Waiting
  - Terminated
- When a thread is created with the `new` operator - the thread is not yet running. It is in the _new_ state.
- Once `start` method is invoked the thread is in the _runnable_ state. A runnable thread may or may not actually be running at any given time.
- When a thread is blocked or waiting, it is temporarily inactive. It is upto the thread scheduler to reactivate it.
- When the thread tries to acquire an intrinsic object lock that is currently held by another thread, it becomes _blocked_.
- When the thread waits for another thread to notify the scheduler of a condition, it enters the _waiting_ state.
- Several methods have a timeout parameter. Calling them causes the thread to enter the _timed waiting_ state.
- A daemon is simply a thread that has no other role than to serve others. Examples are timer threads that send regular "timer ticks" or threads that clean up stale cache entries.
- To peek at the virtual machine bytecodes that execute each statement in our class. Run the command : `javap -c -v Bank` to decompile the Bank.class file.
- There are two mechanisms for protecting a code block from concurrent access. The Java language provides a `synchronized` keyword for this purpose and Java 5 introduced the `ReentrantLock` class.
- In general, you will want to protect blocks of code that update or inspect a shared object.
- A thread can only call `await`, `signalAll`, or `signal` on a condition if it owns the lock of the condition.
- A lock protects sections of code, allowing only one thread to execute the code at a time.
- A lock manages threads that are trying to enter a protected code segment.
- A lock can have one or more associated condition objects.
- Each condition object manages threads that have entered a protected code section but that cannot proceed.
- If a method is declared with the `synchronized` keyword, the object's lock protects the entire method.
- It is also legal to declare static methods as synchronized.
- A monitor has these properties:
  - A monitor is a class with only private fields.
  - Each object of that class has an associated lock.
  - All methods are locked by that lock. In other words, if a client calls `obj.method()`, then the lock for `obj` is automatically acquired at the beginning of the method call and relinquished when the method returns.
  - The lock can have any number of associated conditions
- `java.lang.Thread`

| Method                             | Use                                                                                                                        |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `static void yield()`              | causes the currently executing thread to yield to another thread.                                                          |
| `void join()`                      | waits for the specified thread to terminate.                                                                               |
| `void join(long millis)`           | waits for the specified thread to die or for the specified number of milliseconds to pass                                  |
| `Thread.State getState()`          | gets the state of this thread.                                                                                             |
| `void interrupt()`                 | sends an interrupt request to a thread. The interrupted status of the thread is set to `true`                              |
| `static boolean interrupted()`     | tests whether the _current_ thread has been interrupted. It resets the interrupted status of the current thread to `false` |
| `boolean isInterrupted()`          | tests whether a thread has been interrupted. No side effects.                                                              |
| `static Thread currentThread()`    |                                                                                                                            |
| `void setDaemon(boolean isDaemon)` |                                                                                                                            |

- `java.util.concurrent.locks.Condition`
  - `void await()` puts this thread on the wait set for this condition
  - `void signalAll()` unblocks all threads in the wait set for this condition.
  - `void signal()` unblocks one randomly selected thread in the wait set for this condition.
- `java.util.concurrent.locks.Lock`
  - `Condition newCondition()` returns a condition object associated with this lock.
- `java.lang.Object`
  - `void notifyAll()` unblocks the threads that called `wait` on this object.
  - `void notify()` unblocks one randomly selected thread among the threads that called `wait` on this object.
  - `void wait()` causes a thread to wait until it is notified.
  - `void wait(long millis)`
  - `void wait(long millis, int nanos)`
- The above methods can only be called from within a _synchronized_ method or block.
- By extending the thread class, the derived class itself is a thread object and it gains full control over the thread life cycle.
- Implementing the Runnable interface does not give developers any control over the thread itself, as it simply defines the unit of work that will be executed in a thread.
- All the thread instances the developer created have the same priority, which the process will schedule fairly without worrying about the order.
- If one thread tries to read the data and another thread tries to update the same data, it is known as read/write problem, and it leads to inconsistent state for the shared data. This can be prevented by synchronizing access to the data via Java `synchronized` keyword.
- The producer-consumer problem (also known as the bounded-buffer problem ) is another classical example of a multithread synchronization problem.
- The problem describes two threads, the producer and the consumer, who share a common, fixed-size buffer. The producer’s job is to generate a piece of data and put it into the buffer. The consumer is consuming the data from the same buffer simultaneously. The problem is to make sure that the producer will not try to add data into the buffer if it is full and that the consumer will not try to remove data from an empty buffer.
- `Thread.sleep` causes the current thread to suspend execution for a specified period. One cannot assume that invoking sleep will suspend the thread for precisely the time period specified.
- An _interrupt_ is an indication to a thread that it should stop what it is doing and do something else.
- For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption.
- How does a thread support its own interruption? This depends on what it's currently doing. If the thread is frequently invoking methods that throw `InterruptedException`, it simply returns from the `run` method after it catches that exception.
- If a thread goes a long time w/o invoking method that throws `InterruptedException`, then it must periodically invoke `Thread.interrupted` which returns true if an interrupt has been received. e.g.

```java
for (int i = 0; i < inputs.length; i++) {
  heavyCrunch(inputs[i]);
  if (Thread.interrupted()) {
    // We've been interrupted : no more crunching
    return;
  }
}
```

- In more complex applications it makes more sense to throw an `InterruptedException` when a thread is interrupted.
- The interrupt mechanism is implemented using an internal flag known as the _interrupt status_. Invoking `Thread.interrupt` sets this flag. When a thread checks for an interrupt by invoking the static method `Thread.interrupted`, interrupt status is cleared. The non-static `isInterrupted` method, which is used by one thread to query the interrupt status of another, does not change the interrupt status flag.
- By convention, any method that exits by throwing an `InterruptedException` clears interrupt status when it does so.
- The `join` method allows one thread to wait for the completion of another.
- If `t` is a `Thread` object whose thread is currently executing `t.join()` causes the current thread to pause execution until `t`'s thread terminates.
- Interference happens when two operations, running in different threads, but acting on the same data, _interleave_.
- _Memory consistency errors_ occur when different threads have inconsistent views of what should be the same data
- The key to avoiding memory consistency errors is understanding the _happens-before_ relationship
- A concurrent application's ability to execute in a timely manner is known as its _liveness_.
- _Starvation_ describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads.
- A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then _livelock_ may result
- The biggest advantage of `Lock` objects over implicit locks is their ability to back out of an attempt to acquire a lock
- The fork/join framework is distinct because it uses a _work-stealing_ algorithm. Worker threads that run out of things to do can steal tasks from other threads that are still busy.
- `int r = ThreadLocalRandom.current().nextInt(4, 77);`
- **Futures** They're basically placeholders for a result of an operation that hasn't finished yet. Once the operation finishes, the `Future` will contain that result. For example, an operation can be a `Runnable` or `Callable` instance that is submitted to an `ExecutorService`. The submitter of the operation can use the `Future` object to check whether the operation `isDone()`, or wait for it to finish using the blocking `get()` method.
- **CompletableFuture** : They are in fact an evolution of regular Futures, inspired by Google's Listenable Futures, part of the Guava library. They are Futures that also allow you to string tasks together in a chain. You can use them to tell some worker thread to "go do some task X, and when you're done, go do this other thing using the result of X". Using CompletableFutures, you can do something with the result of the operation without actually blocking a thread to wait for the result.
- _CompletableFuture_ is at the same time a building block and a framework, with **about 50 different methods for composing, combining, and executing asynchronous computation steps and handling errors**.
- `CompletableFuture` supports asynchronous calls. It implements the `CompletionStage` interface.
- `CompletionStage` offers methods, that lets you attach call backs that will be executed on completion.
- `CompletableFuture.supplyAsync` runs the task asynchronously on the default thread pool of Java.
- The `thenApply` method can be used to define a callback which is executed once `supplyAsync` finishes.

| Task Type     | Ideal pool size | Considerations                                                                                                                          |
| ------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| CPU intensive | CPU Core count  | How many other applications (or other executors/threads) are running on the same CPU                                                    |
| IO intensive  | High            | Exact number will depend on rate of task submissions and average task wait time. Too many threads will increase memory consumption too. |

![thread_pool_queue](/images/thread_pool_queues.png)
![rejection_policies](/images/rejection_policies.png)

### What do we understand by the term concurrency?

- Concurrency is the ability of a program to execute several computations simultaneously.
  This can be achieved by distributing the computations over the available CPU cores of a machine or even over different machines within the same network.

### What is the difference between process and thread?

- A process is an execution environment provided by the operating system that has its own set of private resources (e.g. memory, open files, etc.). Threads, in contrast to processes, live within a process and share their resources (memory, open files, etc.) with the other threads of the process. The ability to share resources between different threads makes thread more suitable for tasks where performance is a significant requirement
- Thread is a subset of Process, in other words, one process can contain multiple threads. Two processes runs in different memory space, but all threads share same memory space. Don't confuse this with stack memory, which is different for different thread and is used to store local data for that thread.

### What is Thread in Java with example.

- Java Thread is an independent path of execution within a program which can run in parallel with other existing Threads.
- In Java, when a program requires more than one task to execute in parallel,
  1. Reading a data from a local file.
  2. Reading a data from remote connection.
- When both of above task need to be executed in parallel at that time Threading will come in picture.
- So Java Threads help create multiple independent path of execution within a program which can run in parallel.
- Thread is basically a lightweight sub-process, a smallest unit of processing.
- **In Java every thread maintains its own separate stack. It is called Runtime Stack but they share the same memory**.

### What is a scheduler?

- A scheduler is the implementation of a scheduling algorithm that manages access of processes and threads to some limited resource like the processor or some I/O channel. The goal of most scheduling algorithms is to provide some kind of load balancing for the available processes/threads that guarantees that each process/thread gets an appropriate time frame to access the requested resource exclusively.

### How many threads does a Java program have at least?

- Each Java program is executed within the main thread; hence each Java application has at least one thread.

### How can a Java application access the current thread?

- The current thread can be accessed by calling the static method `currentThread()`.

### What properties does each Java thread have?

- Each Java thread has the following properties:
  - an identifier of type `long` that is unique within the JVM
  - a name of type `String`
  - a priority of type `int`
  - a state of type `java.lang.Thread.State`
  - a thread group the thread belongs to

### What is the purpose of thread groups?

- Each thread belongs to a group of threads. The JDK class `java.lang.ThreadGroup` provides some methods to handle a whole group of Threads. With these methods we can, for example, interrupt all threads of a group or set their maximum priority.
- `ThreadGroup` is a class which was intended to provide information about a thread group. ThreadGroup API is weak and it doesn’t have any functionality that is not provided by Thread. Two of the major feature it had are to get the list of active threads in a thread group and to set the uncaught exception handler for the thread. But Java 1.5 has added `setUncaughtExceptionHandler(UncaughtExceptionHandler eh)` method using which we can add uncaught exception handler to the thread. So ThreadGroup is obsolete and hence not advised to use anymore.

### Explaing Life cycle of a thread

![life_cycle](/images/life_cycle.png)

- **New** – A newly created thread object instance on which the start() method has not yet been invoked is in the new state.
- **Runnable** – A thread in new state enters the runnable state when the `Thread.start()` method is invoked on it. There are 2 important points to note regarding the runnable state –
  1. Although the thread enters the runnable state immediately on invoking the start() method, but it is not necessary that the thread immediately starts executing. A thread runs when the logic it holds in its run() method can be executed by the processor. In case the thread logic needs any resource which is not available then the thread waits for the resource to become available.
  2. Secondly, a thread in runnable state may run for some time and then get blocked for a monitor lock, or enter the waiting/timed_waiting states as it waits for the opportunity/time to enter runnable state again.
- **Blocked** – A running thread may enter the blocked state as it waits for a monitor lock to be freed. It may also be blocked as it waits to reenter a monitor lock after being asked to wait using the Thread.wait() method.
- **Waiting** – A thread enters the waiting state when it is made to wait for a go-ahead signal to proceed. The go-ahead in this case is given by another thread and can be given in the following 3 scenarios –
  1. Thread waiting due to `Thread.wait()` method being called on it: The other thread can use `Thread.notify()` or `Thread.notifyAll()` to give the go-ahead to the waiting thread.
  2. Thread waiting as it itself has asked for joining another thread using `Thread.join()`: The waiting thread gets a go-ahead when the thread it's waiting for ends.
  3. Thread waiting due to `LockSupport.park()` method being invoked on it: The waiting thread resumes when `LockSupport.unPark()` is called with the parked thread object as the parameter.
- **Timed_Waiting** – A thread which is waiting as it has been specifically ‘instructed’ to wait for a specified waiting time is in a timed_waiting state. A thread can be made to wait for a pre-determined amount of time in the following ways –
  1. Thread made to wait using `Thread.sleep()` method.
  2. Threads being asked to wait for a permit for a specified amount of time using `LockSuport.parkNanos()` and `LockSupport.parkUntil()` methods.
  3. Threads being made to wait for a fixed amount of time using `Thread.wait(long millis)` or `Thread.join(long millis, int nanos)`.
- **Terminated** – A thread enters its ‘final resting’ state or terminated state when it has finished executing the logic specified in its run() method.

### How do we set the priority of a thread?

- The priority of a thread is set by using the method setPriority(int). To set the priority to the maximum value, we use the constant `Thread.MAX_PRIORITY` and to set it to the minimum value we use the constant `Thread.MIN_PRIORITY` because these values can differ between different JVM implementations.

### Why should a thread not be stopped by calling its method `stop()`?

- A thread should not be stopped by using the deprecated methods `stop()` of `java.lang.Thread`, as a call to this method causes the thread to unlock all monitors it has acquired.
- If any object protected by one of the released locks was in an inconsistent state, this state gets visible to all other threads. This can cause arbitrary behavior when other threads work with this inconsistent object.

### Is it possible to start a thread twice?

- No, after having started a thread by invoking its `start()` method, a second invocation of `start()` will throw an `IllegalThreadStateException`

### How it throws IllegalThreadStateException?

- If you see the code of start() method, you will observe that Thread maintains `threadStatus` whose value initialy is 0 and once the thread is completed its value is 2.

```java
public synchronized void start() {
    ...
    if (threadStatus != 0)
        throw new IllegalThreadStateException();
    }
    ...
```

- So when thread.start() is called again, threadStatus value is 2 and not 0 that is why it throws IllegalThreadStateException

### What is difference between user Thread and daemon Thread?

- When we create a Thread in Java program, it’s known as **user** thread. A daemon thread runs in background and doesn’t prevent JVM from terminating. When there are no user threads running, JVM shutdown the program and quits. A child thread created from daemon thread is also a daemon thread.
- **Daemon** thread in Java is a service provider thread that provides services to the user thread. Its life depend on the mercy of user threads i.e. when all the user threads dies, JVM terminates this thread automatically.
- There are many Java daemon threads running automatically e.g. gc, finalizer etc.
- You can see all the detail by typing the jconsole in the command prompt. The jconsole tool provides information about the loaded classes, memory usage, running threads etc.

### Is it possible to convert a normal user thread into a daemon thread after it has been started?

- A user thread **cannot** be converted into a daemon thread once it has been started. Invoking the method `thread.setDaemon(true)` on an already running thread instance causes a `IllegalThreadstateException`.

### What do we understand by busy waiting?

- Busy waiting means implementations that wait for an event by performing some active computations that let the thread/process occupy the processor although it could be removed from it by the scheduler.
- An example for busy waiting would be to spend the waiting time within a loop that determines the current time again and again until a certain point in time is reached:

```java
Thread thread = new Thread(new Runnable() {
  @Override
  public void run() {
    long millisToStop = System.currentTimeMillis() + 5000;
    long currentTimeMillis = System.currentTimeMillis();
    while (millisToStop > currentTimeMillis) {
      currentTimeMillis = System.currentTimeMillis();
    }
  }
});
```

### How can we prevent busy waiting?

- One way to prevent busy waiting is to put the current thread to sleep for a given amount of time. This can be done by calling the method `java.lang.Thread.sleep(long)` by passing the number of milliseconds to sleep as an argument.

### Can we use `Thread.sleep()` for real-time processing?

- The number of milliseconds passed to an invocation of `Thread.sleep(long)` is only an indication for the scheduler how long the current thread does not need to be executed. It may happen that the scheduler lets the thread execute again a few milliseconds earlier or later depending on the actual implementation. Hence an invocation of `Thread.sleep()` should not be used for real-time processing.

### How can a thread be woken up that has been put to sleep before using `Thread.sleep()`?

- The method `interrupt()` interrupts a sleeping thread. The interrupted thread that has been put to sleep by calling `Thread.sleep()` is woken up by an `InterruptedException`:

### How can a thread query if it has been interrupted?

- If the thread is not within a method like `Thread.sleep()` that would throw an `InterruptedException`, the thread can query if it has been interrupted by calling either the static method `Thread.interrupted()` or the method `isInterrupted()` that it has inherited from `java.lang.Thread`

### How should an `InterruptedException` be handled?

- Methods like `sleep()` and `join()` throw an `InterruptedException` to tell the caller that another thread has interrupted this thread. In most cases this is done in order to tell the current thread to stop its current computations and to finish them unexpectedly. Hence ignoring the exception by catching it and only logging it to the console or some log file is often not the appropriate way to handle this kind of exception. The problem with this exception is, that the method `run()` of the Runnable interface does not allow that `run()` throw any exception. So just rethrowing it does not help. This means the implementation of `run()` has to handle this checked exception itself and this often leads to the fact that it is caught and ignored.

### After having started a child thread, how do we wait in the parent thread for the termination of the child thread?

- Waiting for a thread's termination is done by invoking the method `join()` on the thread's instance variable.

### What happens when an uncaught exception leaves the `run()` method?

- It can happen that an unchecked exception escapes from the `run()` method. In this case the thread is stopped by the JVM. It is possible to catch this exception by registering an instance that implements the interface `UncaughtExceptionHandler` as an exception handler.
- This is either done by invoking the static method `Thread.setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler)`, which tells the JVM to use the provided handler in case there was no specific handler registered on the thread itself, or by invoking `setUncaughtExceptionHandler(Thread.UncaughtExceptionHandler)` on the thread instance itself.

### What is a shutdown hook?

- A shutdown hook is a thread that gets executed when the JVM shuts down. It can be registered by invoking `addShutdownHook(Runnable)` on the Runtime instance.

### For what purposes is the keyword `synchronized` used?

- When you have to implement exclusive access to a resource, like some static value or some file reference, the code that works with the exclusive resource can be embraced with a synchronized block:

### What intrinsic lock does a synchronized method acquire?

- A synchronized method acquires the intrinsic lock for that method's object and releases it when the method returns. Even if the method throws an exception, the intrinsic lock is released.

### Can a constructor be synchronized?

- **No**, a constructor can't be synchronized. The reason why this leads to a syntax error is the fact that only the constructing thread should have access to the object being constructed.

### Can primitive values be used for intrinsic locks?

- **No**, primitive values can't be used for intrinsic locks.

### Are intrinsic locks reentrant?

- Yes, intrinsic locks can be accessed by the same thread again and again. Otherwise code that acquires a lock would have to pay attention that it does not accidently tries to acquire a lock it has already acquired.

### What do we understand by an atomic operation?

- An atomic operation is an operation that is either executed completely or not at all.

### Is the statement `i++` atomic?

- No, the incrementation of an integer variable consist of more than one operation. First we have to load the current value of i, increment it and then finally store the new value back. The current thread performing this incrementation may be interrupted in-between any of these three steps, hence this operation is not atomic.

### What operations are atomic in Java?

- The Java language provides some basic operations that are atomic and that therefore can be used to make sure that concurrent threads always see the same value:
  - Read and write operations to reference variables and primitive variables (except long and double)
  - Read and write operations for all variables declared as volatile

### Is the following implementation thread-safe?

```java
public class DoubleCheckedSingleton {

  private DoubleCheckedSingleton instance = null;

  public DoubleCheckedSingleton getInstance() {
    if (instance == null) {
      synchronized(DoubleCheckedSingleton.class) {
        if (instance == null) {
          instance = new DoubleCheckedSingleton();
        }
      }
    }
    return instance;
  }
}
```

- The code above is **not** thread-safe. Although it checks the value of instance once again within the synchronized block (for performance reasons), the JIT compiler can rearrange the bytecode in a way that the reference to instance is set before the constructor has finished its execution. This means the method `getInstance()` returns an object that may not have been initialized completely. To make the code thread-safe, the keyword volatile can be used since Java 5 for the instance variable. Variables that are marked as volatile get only visible to other threads once the constructor of the object has finished its execution completely.

### What do we understand by a deadlock?

- A deadlock is a situation in which two (or more) threads are each waiting on the other thread to free a resource that it has locked, while the thread itself has locked a resource the other thread is waiting on:
  - Thread 1: locks resource A, waits for resource B
  - Thread 2: locks resource B, waits for resource A

### What are the requirements for a deadlock situation?

- In general the following requirements for a deadlock can be identified:
  - **Mutual exclusion**: There is a resource which can be accessed only by one thread at any point in time.
  - **Resource holding**: While having locked one resource, the thread tries to acquire another lock on some other exclusive resource.
  - **No pre-emption**: There is no mechanism, which frees the resource if one thread holds the lock for a specific period of time.
  - **Circular wait**: During runtime a constellation occurs in which two (or more) threads are each waiting on the other thread to free a resource that it has locked

### Is it possible to prevent deadlocks at all?

- In order to prevent deadlocks one (or more) of the requirements for a deadlock has to be eliminated:
  - **Mutual exclusion**: In some situation it is possible to prevent mutual exclusion by using optimistic locking.
  - **Resource holding**: A thread may release all its exclusive locks, when it does not succeed in obtaining all exclusive locks.
  - **No pre-emption**: Using a timeout for an exclusive lock frees the lock after a given amount of time.
  - **Circular wait**: When all exclusive locks are obtained by all threads in the same sequence, no circular wait occurs.

### Is it possible to implement a deadlock detection?

- When all exclusive locks are monitored and modelled as a directed graph, a deadlock detection system can search for two threads that are each waiting on the other thread to free a resource that it has locked. The waiting threads can then be forced by some kind of exception to release the lock the other thread is waiting on.

### What is a livelock?

- A livelock is a situation in which two or more threads block each other by responding to an action that is caused by another thread. In contrast to a deadlock situation, where two or more threads wait in one specific state, the threads that participate in a livelock change their state in a way that prevents progress on their regular work.
- An example would be a situation in which two threads try to acquire two locks, but release a lock they have acquired when they cannot acquire the second lock. It may now happen that both threads concurrently try to acquire the first lock. As only one thread succeeds, the second thread may succeed in acquiring the second lock. Now both threads hold two different locks, but as both want to have both locks, they release their lock and try again from the beginning. This situation may happen again and again.

### What do we understand by thread starvation?

- Threads with lower priority get less time for execution than threads with higher priority. When the threads with lower priority performs a long enduring computations, it may happen that these threads do not get enough time to finish their computations just in time. They seem to "starve" away as threads with higher priority steal them their computation time.

### Can a synchronized block cause thread starvation?

- The order in which threads can enter a synchronized block is not defined. So in theory it may happen that in case many threads are waiting for the entrance to a synchronized block, some threads have to wait longer than other threads. Hence they do not get enough computation time to finish their work in time.

### What do we understand by the term race condition?

- A race condition describes constellations in which the outcome of some multi-threaded implementation depends on the exact timing behavior of the participating threads. In most cases it is not desirable to have such a kind of behavior, hence the term race condition also means that a bug due to missing thread synchronization leads to the differing outcome.
- Race conditions occurs when two thread operate on same object without proper synchronization and there operation interleave with each other. Classical example of Race condition is incrementing a counter since increment is not an atomic operation and can be further divided into three steps like read, update and write. If two threads tries to increment count at same time and if they read same value because of interleaving of read operation of one thread to update operation of another thread, one count will be lost when one thread overwrite increment done by other thread. atomic operations are not subject to race conditions because those operation cannot be interleaved.

### How to find Race Conditions in Java

- Finding race conditions by unit testing is not reliable due to random nature of race conditions. Since race conditions surface only some time, your unit test may pass without facing any race condition. Only sure shot way to find race condition is reviewing code manually or using code review tools which can alert you on potential race conditions based on code pattern and use of synchronization in Java.
- Two code patterns "**check and act**" and "**read modify write**" can suffer race condition if not synchronized properly. both cases rely on natural assumption that a single line of code will be atomic and executed in one shot which is wrong e.g. ++ is not atomic.
- **"Check and Act" race condition pattern** In concurrent programming a Race Condition occurs when a second thread modifies the state of one (or more objects), making any assumptions, checks, made by the first threads invalid. This is sometimes referred to as “_check and act_“.
- classic example of "check and act" race condition in Java is getInstance() method of Singleton Class. getInstace() method first checks for whether instance is null and then initializes the instance and return to caller. Whole purpose of Singleton is that getInstance should always return same instance of Singleton. if you call getInstance() method from two thread simultaneously it's possible that while one thread is initializing singleton after null check, another thread sees value of \_instance reference variable as null (quite possible in Java) especially if your object takes longer time to initialize and enters into critical section which eventually results in getInstance() returning two separate instance of Singleton. This may not happen always because a fraction of delay may result in value of \_instance updated in main memory. here is a code example

  ```java
  public Singleton getInstance(){
  //race condition if two threads sees _instance= null
  if(_instance == null){
  	_instance = new Singleton();
  	}
  }
  ```

- An easy way to fix "check and act" race conditions is to use synchronized keyword and enforce locking which will make this operation atomic and guarantees that block or method will only be executed by one thread and result of operation will be visible to all threads once synchronized blocks completed or thread exited form synchronized block.
- **read-modify-update race conditions** read-modify-update pattern also comes due to improper synchronization of **non-atomic operations** or combination of two individual atomic operations which is not atomic together e.g. put if absent scenario. consider below code

  ```java
  if(!hashtable.contains(key)){
  	hashtable.put(key,value);
  }
  ```

- Here we only insert object into hashtable if its not already there. point is both contains() and put() are atomic but still this code can result in race condition since both operation together is not atomic. consider thread T1 checks for conditions and goes inside if block now CPU is switched from T1 to thread T2 which also checks condition and goes inside if block. now we have two thread inside if block which result in either T1 overwriting T2 value or vice-versa based on which thread has CPU for execution. In order to fix this race condition in Java you need to wrap this code inside synchronized block which makes them atomic together because no thread can go inside synchronized block if one thread is already there.
- **Real example of Race condition** One example of Race Condition is shopping online. Say that you found an item you want to buy in an online store. The online store indicates that they only have one left. By the time you press buy, another user beats you to it and buys it after you browse the page (which page indicates one item left) but before you pressed buy. This is another example of Race Condition as the state changed while you were shopping without you knowing.
- **Example of Non Thread Safe Code in Java**
  ```java
  public class Counter {
      private int count;
       //This method is not thread-safe because ++ is not an atomic operation
      public int getCount(){
          return count++;
      }
  }
  ```
- **Above example is not thread-safe because** `++` (increment operator) is not an **atomic operation** and can be broken down into read, update and write operation. if multiple thread call `getCount()` approximately same time, each of these three operation may coincide or overlap with each other for example while thread 1 is updating value, thread 2 reads and still gets old value, which eventually lets thread 2 override thread 1 increment and **one count is lost** because multiple thread called it concurrently.
- **How to make code Thread-Safe in Java**

  1. Use synchronized keyword in Java and lock the getCount() method so that only one thread can execute it at a time which removes possibility of coinciding or interleaving.
  2. use Atomic Integer, which makes this ++ operation atomic and since atomic operations are thread-safe and saves cost of external synchronization.

  ```java
  public class Counter {

      private int count;
      AtomicInteger atomicCount = new AtomicInteger(0);

  	  // This method thread-safe now because of locking and synchornization
      public synchronized int getCount(){
       		return count++;
      }

      // This method is thread-safe because count is incremented atomically
      public int getCountAtomically(){
       	 return atomicCount.incrementAndGet();
      }
  }
  ```

### How to write thread safe code in Java

1. Immutable objects are by default thread-safe because there state can not be modified once created. Since String is immutable in Java, its inherently thread-safe.
2. Read only or final variables in Java are also thread-safe in Java.
3. Locking is one way of achieving thread-safety in Java.
4. Static variables if not synchronized properly becomes major cause of thread-safety issues.
5. Example of thread-safe class in Java: Vector, Hashtable, ConcurrentHashMap, String etc.
6. Atomic operations in Java are thread-safe e.g. reading a 32 bit int from memory because its an atomic operation it can't interleave with other thread.
7. local variables are also thread-safe because each thread has there own copy and using local variables is good way to writing thread-safe code in Java.
8. In order to avoid thread-safety issue minimize sharing of objects between multiple thread.
9. volatile keyword in Java can also be used to instruct thread not to cache variables and read from main memory and can also instruct JVM not to reorder or optimize code from threading perspective.

### What is Lock API? Java Lock vs synchronized?

- The traditional way to achieve thread synchronization in Java is by the use of synchronized keyword. While it provides a certain basic synchronization, the synchronized keyword is quite rigid in its use. For example, a thread can take a lock only once. Synchronized blocks don’t offer any mechanism of a waiting queue and after the exit of one thread, any thread can take the lock. This could lead to starvation of resources for some other thread for a very long period of time.
- Reentrant Locks are provided in Java to provide synchronization with greater flexibility.
  1. **Lock**: This is the base interface for Lock API. It provides all the features of synchronized keyword with additional ways to create different Conditions for locking, providing timeout for thread to wait for lock. Some of the important methods are lock() to acquire the lock, unlock() to release the lock, tryLock() to wait for lock for a certain period of time, newCondition() to create the Condition etc.
  2. **Condition**: Condition objects are similar to Object wait-notify model with additional feature to create different sets of wait. A Condition object is always created by Lock object. Some of the important methods are await() that is similar to wait() and signal(), signalAll() that is similar to notify() and notifyAll() methods.
  3. **ReadWriteLock**: It contains a pair of associated locks, one for read-only operations and another one for writing. The read lock may be held simultaneously by multiple reader threads as long as there are no writer threads. The write lock is exclusive.
  4. **ReentrantLock**: This is the most widely used implementation class of Lock interface. This class implements the Lock interface in similar way as synchronized keyword. The class ReentrantLock is a mutual exclusion lock with the same basic behavior as the implicit monitors accessed via the synchronized keyword but with extended capabilities.
- **Apart from Lock interface implementation, ReentrantLock contains some utility methods to get the thread holding the lock, threads waiting to acquire the lock etc.** As the name says, ReentrantLock allow threads to enter into lock on a resource more than once. When the thread first enters into lock, a hold count is set to one. Before unlocking, the thread can re-enter into lock again and every time hold count is incremented by one. For every unlock request, hold count is decremented by one and when hold count is 0, the resource is unlocked.
- A lock is acquired via lock() and released via unlock(). It's important to wrap your code into a try/finally block to ensure unlocking in case of exceptions. This method is thread-safe just like the synchronized counterpart. If another thread has already acquired the lock subsequent calls to lock() pause the current thread until the lock has been unlocked. Only one thread can hold the lock at any given time.
- **Java Lock vs synchronized**
  1. Java Lock API provides more visibility and options for locking, unlike synchronized where a thread might end up waiting indefinitely for the lock, we can use tryLock() to make sure thread waits for specific time only.
  2. Synchronization code is much cleaner and easy to maintain whereas with Lock we are forced to have try-finally block to make sure Lock is released even if some exception is thrown between lock() and unlock() method calls.
  3. synchronization blocks or methods can cover only one method whereas we can acquire the lock in one method and release it in another method with Lock API.
  4. synchronized keyword doesn’t provide fairness whereas we can set fairness to true while creating ReentrantLock object so that longest waiting thread gets the lock first.
  5. We can create different conditions for Lock and different thread can await() for different conditions.

### What do we understand by fair locks?

- A fair lock takes the waiting time of the threads into account when choosing the next thread that passes the barrier to some exclusive resource. An example implementation of a fair lock is provided by the Java SDK: `java.util.concurrent.locks.ReentrantLock`. If the constructor with the boolean flag set to true is used, the `ReentrantLock` grants access to the longest-waiting thread.

### Which two methods that each object inherits from java.lang.object can be used to implement a simple producer/consumer scenario?

- When a worker thread has finished its current task and the queue for new tasks is empty, it can free the processor by acquiring an intrinsic lock on the queue object and by calling the method `wait()`. The thread will be woken up by some producer thread that has put a new task into the queue and that again acquires the same intrinsic lock on the queue object and calls `notify()` on it.

### What is the difference between `notify()` and `notifyAll()`?

- Both methods are used to wake up one or more threads that have put themselves to sleep by calling `wait()`. While `notify()` only wakes up one of the waiting threads, `notifyAll()` wakes up all waiting threads.

### How is it determined which thread wakes up by calling `notify()` ?

- It is not specified which threads will be woken up by calling `notify()` if more than one thread is waiting. Hence code should not rely on any concrete JVM implementation.

### Is the following code that retrieves an integer value from some queue implementation correct?

```java
public Integer getNextInt() {

  Integer retVal = null;

  synchronized (queue) {
    try {
      while (queue.isEmpty()) {
        queue.wait();
      }
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  synchronized (queue) {
    retVal = queue.poll();
    if (retVal == null) {
      System.err.println("retVal is null");
      throw new IllegalStateException();
    }
  }
  return retVal;
}
```

- Although the code above uses the queue as object monitor, it does not behave correctly in a multi-threaded environment. The reason for this is that it has two separate synchronized blocks. When two threads are woken up in line 6 by another thread that calls `notifyA1l()`, both threads enter one after the other the second synchronized block. In this second block the queue has now only one new value, hence the second thread will poll
  on an empty queue and get null as return value.

### Is it possible to check whether a thread holds a monitor lock on some given object?

- The class `java.lang.Thread` provides the static method `Thread.holdsLock(Object)` that returns true iff the current thread holds the lock on the object given as argument to the method invocation.

### What does the method `Thread.yield()` do?

- An invocation of the static method `Thread.yield()` gives the scheduler a hint that the
  current thread is willing to free the processor. The scheduler is free to ignore this hint.
  As it is not defined which thread will get the processor after the invocation of `Thread/yield()`, it may even happen that the current thread becomes the "next" thread to be executed.

### What do you have to consider when passing object instances from one thread to another?

- When passing objects between threads, you will have to pay attention that these objects
  are not manipulated by two threads at the same time. An example would be a `Map` implementation whose key/value pairs are modified by two concurrent threads. In order to
  avoid problems with concurrent modifications you can design an object to be immutable.

### Which rules do you have to follow in order to implement an immutable class?

- All fields should be final and private.
- There should be not setter methods.
- The class itself should be declared final in order to prevent subclasses to violate the principle of immutability
- If fields are not of a primitive type but a reference to another object:
  - There should not be a getter method that exposes the reference directly to the caller.
  - Don't change the referenced objects (or at least changing these references is not visisble to clients of the object).

### What is the purpose of the class `java.lang.ThreadLocal` ?

- As memory is shared between different threads, `ThreadLocal` provides a way to store and
  retrieve values for each thread separately. Implementations of `ThreadLocal` store and retrieve the values for each thread independently such that when thread A stores the
  value A1 and thread B stores the value B1 in the same instance of `ThreadLocal`, thread A
  later on retrieves value A1 from this `ThreadLocal` instance and thread B retrieves value
  B1.

### What are possible use cases for `java.lang.ThreadLocal` ?

- Instances of `ThreadLocal` can be used to transport information throughout the application without the need to pass this from method to method. Examples would be the
  transportation of security/login information within an instance of `ThreadLocal` such that it is accessible by each method. Another use case would be to transport transaction information or in general objects that should be accessible in all methods without passing
  them from method to method.

### What is ThreadLocal?

- The Java ThreadLocal class enables you to create variables that can only be read and written by the same thread. Thus, even if two threads are executing the same code, and the code has a reference to the same ThreadLocal variable, the two threads cannot see each other's ThreadLocal variables. Thus, the Java ThreadLocal class provides a simple way to make code thread safe that would not otherwise be so.

```java
private ThreadLocal threadLocal = new ThreadLocal(); // Creating a ThreadLocal
threadLocal.set("A thread local value"); // Set ThreadLocal Value
String threadLocalValue = (String) threadLocal.get(); // Get ThreadLocal Value
threadLocal.remove(); // Remove ThreadLocal Value

// Java code illustrating get() and set() method
public class ThreadLocalExample {

    public static void main(String[] args) {

        ThreadLocal<Number> tlObj = new ThreadLocal<Number>();

        // setting the value
        tlObj.set(100);
        System.out.println("value = " + tlObj.get());
    }
}
```

### Is it possible to improve the performance of an application by the usage of multi-threading? Give some examples.

- If we have more than one CPU core available, the performance of an application can be improved by multi-threading if it is possible to parallelize the computations over the available CPU cores. An example would be an application that should scale all images that are stored within a local directory structure. Instead of iterating over all images one after the other, a producer/consumer implementation can use a single thread to scan the directory structure and a bunch of worker threads that perform the actual scaling operation. Another example would be an application that mirrors some web page. Instead of loading one HTML page after the other, a producer thread can parse the first HTML page and issue the links it found into a queue. The worker threads monitor the queue and load the web pages found by the parser. While the worker threads wait for the page to get loaded completely, other threads can use the CPU to parse the already loaded pages and issue new requests.

### Is it possible to compute the theoretical maximum speed up for an application by using multiple processors?

- Amdahl's law provides a formula to compute the theoretical maximum speed up by providing multiple processors to an application. The theoretical speedup is computed by `S(n) = 1 / (B + (1-B)/n)` where n denotes the number of processors and `B` the fraction of the program that cannot be executed in parallel. When n converges against infinity, the term `(1-B)/n` converges to zero. Hence the formula can be reduced in this special case to `1/B`. As we can see, the theoretical maximum speedup behaves reciprocal to the fraction that has to be executed serially. This means the lower this fraction is, the more theoretical speedup can be achieved.

### What do we understand by lock contention?

- Lock contention occurs, when two or more threads are competing in the acquisition of a
  lock. The scheduler has to decide whether it lets the thread, which has to wait sleeping
  and performs a context switch to let another thread occupy the CPU, or if letting the
  waiting thread busy-waiting is more efficient. Both ways introduce idle time to the inferior thread.

### Which techniques help to reduce lock contention?

- In some cases lock contention can be reduced by applying one of the following techniques:
  - The scope of the lock is reduced.
  - The number of times a certain lock is acquired is reduced (lock splitting).
  - Using hardware supported optimistic locking operations instead of synchronization.
  - Avoid synchronization where possible.
  - Avoid object pooling.

### Which technique to reduce lock contention can be applied to the following code?

```java
synchronized (map) {
  UUID randomUUID = UUID.randomUUID();
  Integer value = Integer.valueOf(42);
  String key = randomUUID.toString();
  map.put(key, value);
}
```

- The code above performs the computation of the random UUID and the conversion of the literal 42 into an Integer object within the synchronized block, although these two lines of code are local to the current thread and do not affect other threads. Hence they can be moved out of the synchronized block:

```java
UUID randomUUID = UUID.randomUUID();
Integer value = Integer.valueOf(42);
String key = randomUUID.toString();
synchronized (map) {
  map.put(key, value);
}
```

### Explain by an example the technique lock splitting.

- Lock splitting may be a way to reduce lock contention when one lock is used to synchronize access to different aspects of the same application. Suppose we have a class that implements the computation of some statistical data of our application. A first version of this class uses the keyword synchronized in each method signature in order to guard the internal state before corruption by multiple concurrent threads. This also means that each method invocation may cause lock contention as other threads may try to acquire the same lock simultaneously. But it may be possible to split the lock on the object instance into a few smaller locks for each type of statistical data within each method. Hence thread T1 that tries to increment the statistical data D1 does not have to wait for the lock while thread T2 simultaneously updates the data D2.

### What kind of technique for reducing lock contention is used by the SDK class ReadWriteLock?

- The SDK class `ReadwriteLock` uses the fact that concurrent threads do not have to acquire a lock when they want to read a value when no other thread tries to update the
  value. This is implemented by a pair of locks, one for read-only operations and one for writing operations. While the read-only lock may be obtained by more than one thread, the implementation guarantees that all read operation see an updated value once the write lock is released.

### What do we understand by lock striping?

- In contrast to lock splitting, where we introduce different locks for different aspects of the application, lock striping uses multiple locks to guard different parts of the same data structure. An example for this technique is the class `ConcurrentHashMap` from JDK's `java.util.concurrent` package. The `Map` implementation uses internally different buckets to store its values. The bucket is chosen by the value's key. `ConcurrentHashMap` now uses different locks to guard different hash buckets. Hence one thread that tries to access the first hash bucket can acquire the lock for this bucket, while another thread can simultaneously access a second bucket. In contrast to a synchronized version of `Hashmap` this technique can increase the performance when different threads work on different buckets.

### What do we understand by a CAS operation?

- CAS stands for compare-and-swap and means that the processor provides a separate instruction that updates the value of a register only if the provided value is equal to the
  current value. CAS operations can be used to avoid synchronization as the thread can try to update a value by providing its current value and the new value to the CAS operation. If another thread has meanwhile updated the value, the thread's value is not equal to the current value and the update operation fails. The thread then reads the new value and tries again. That way the necessary synchronization is interchanged by an optimistic spin waiting.

### Which Java classes uses thee CAS operation?

- The SDK classes in the package `java.util.concurrent.atomic` like `AtomicInteger` or `AtomicBoolean` use internally the CAS operation to implement concurrent incrementation.

```java
public class CounterAtomic {
  private AtomicLong counter = new AtomicLong();

  public void increment() {
    counter.incrementAndGet();
  }

  public long get() {
    return counter.get();
  }
}
```

### Provide an example why performance improvements for single-threaded applications can cause performance degradation for multi-threaded applications.

- A prominent example for such optimizations is a `List` implementation that holds the number of elements as a separate variable. This improves the performance for single- threaded applications as the `size()` operation does not have to iterate over all elements but can return the current number of elements directly. Within a multi-threaded application the additional counter has to be guarded by a lock as multiple concurrent threads may insert elements into the list. This additional lock can cost performance when there are more updates to the list than invocations of the `size()` operation.

### Is object pooling always a performance improvement for multi-threaded applications?

- Object pools that try to avoid the construction of new objects by pooling them can improve the performance of single-threaded applications as the cost for object creation is interchanged by requesting a new object from the pool. In multi-threaded applications such an object pool has to have synchronized access to the pool and the additional costs of lock contention may outweigh the saved costs of the additional construction and garbage collection of the new objects. Hence object pooling may not always improve the overall performance of a multi-threaded application.

### What is the relation between the two interfaces `Executor` and `ExecutorService`?

- The interface `Executor` only defines one method: `execute(Runnable)`. Implementations of
  this interface will have to execute the given Runnable instance at some time in the future.
- The `ExecutorService` interface is an extension of the `Executor` interface and provides additional methods to shut down the underlying implementation, to await the termination of all submitted tasks and it allows submitting instances of `Callable`.

### What happens when you `submit() a new task to an `ExecutorService` instance whose queue is already full?

- As the method signature of `submit()` indicates, the `ExecutorService` implementation is
  supposed to throw a `RejectedExecutionException`.

### What is a `ScheduledExecutorService`?

- The interface `ScheduledExecutorService` extends the interface `Executorservice` and adds
  method that allow to submit new tasks to the underlying implementation that should be
  executed a given point in time. There are two methods to schedule one-shot tasks and
  two methods to create and execute periodic tasks.

### Do you know an easy way to construct a thread pool with 5 threads that executes some tasks that return a value?

- The SDK provides a factory and utility class `Executors` whose static method `newFixedThreadPool(int nThreads)` allows the creation of a thread pool with a fixed number of threads (the implementation of `MyCallable` is omitted):

### What is the difference between the two interfaces Runnable and Callable

- The interface Runnable defines the method run() without any return value whereas the
  interface Callable allows the method call() to return a value and to throw an exception.

### Which are use cases for the class `java.util.concurrent.Future` ?

- Instances of the class `java.util.concurrent.Future` are used to represent results of asynchronous computations whose result are not immediately available. Hence the class provides methods to check if the asynchronous computation has finished, canceling the task
  and to the retrieve the actual result. The latter can be done with the two get methods
  provided. The first `get()` methods takes no parameter and blocks until the result is available whereas the second `get()` method takes a timeout parameter that lets the method
  invocation return if the result does not get available within the given timeframe.

### What is a `CountDownLatch` ?

- The SDK class `CountDownLatch` provides a synchronization aid that can be used to implement scenarios in which threads have to wait until some other threads have reached the
  same state such that all thread can start. This is done by providing a synchronized counter that is decremented until it reaches the value zero. Having reached zero the `CountDownLatch` instance lets all threads proceed. This can be either used to let all threads start at a given point in time by using the value 1 for the counter or to wait until a number of threads has finished. In the latter case the counter is initialized with the number of threads and each thread that has finished its work counts the latch down by one.

### What is the difference between a `CountDownLatch` and a `CyclicBarrier` ?

- Both SDK classes maintain internally a counter that is decremented by different threads. The threads wait until the internal counter reaches the value zero and proceed from there on. But in contrast to the `CountDownLatch` the class `CyclicBarrier` resets the internal value back to the initial value once the value reaches zero. As the name indicates instances of `CyclicBarrier` can therefore be used to implement use cases where threads have to wait on each other again and again.

### Similarity and difference between `CyclicBarrier` and `CountDownLatch`?

- `CountDownLatch(int count)` is initialized with given `count`. `count` specifies the number of events that must occur before latch is released.
- `CyclicBarrier(int parties)` is created with given `parties`. `parties` specifies the number of threads waiting for each other to reach commob barrier point. When all threads have reached common barrier point, `parties` number of waiting threads are released.
- `CyclicBarrier` and `CountDownLatch` are similar because they wait for specified number of threads to reach certain point and make count/parties equal to 0. But for completing wait in `CountDownLatch`, specified number of threads must call `countDown()` method and for completing wait in `CyclicBarrier` specified number of threads must call `await()` method.
- `CyclicBarrier` can be awaited repeatedly, but CountDownLatch can’t be awaited repeatedly. i.e. once count has become 0 `CyclicBarrier` can be used again but `CountDownLatch` cannot be used again.
- `CyclicBarrier` can be used to trigger event, but `CountDownLatch` can’t be used to launch event. i.e. once count has become 0 `CyclicBarrier` can trigger event but `CountDownLatch` can’t.

### What kind of tasks can be solved by using the Fork/Join framework?

- The base class of the Fork/Join Framework `java.util.concurrent.ForkJoinPool` is basically a thread pool that executes instances of `java.util.concurrent.ForkjoinTask`. The class `ForkJoinTask` provides the two methods `fork()` and `join()`. While `fork()` is used to start the asynchronous execution of the task, the method `join()` is used to await the result of the computation. Hence the Fork/Join framework can be used to implement divide-and-conquer algorithms where a more complex problem is divided into a number of smaller and easier to solve problems.

### Is it possible to find the smallest number within an array of numbers using the Fork/Join-Framework?

- The problem of finding the smallest number within an array of numbers can be solved by
  using a divide-and-conquer algorithm. The smallest problem that can be solved very easily is an array of two numbers as we can determine the smaller of the two numbers directly by one comparison. Using a divide-and-conquer approach the initial array is divided into two parts of equal length and both parts are provided to two instances of `RecursiveTask` that extend the class `ForkJoinTask`. By forking the two tasks they get executed and either solve the problem directly, if their slice of the array has the length two, or they again recursively divide the array into two parts and fork two new RecursiveTasks. Finally each task instance returns its result (either by having it computed directly or by waiting for the two subtasks). The root tasks then returns the smallest number in the array.

### What is the difference between the two classes `RecursiveTask` and `RecursiveAction`?

- In contrast to `RecursiveTask` the method `compute()` of `RecursiveAction` does not have to return a value. Hence `RecursiveAction` can be used when the action works directly on
  some data structure without having to return the computed value.

### Is it possible to perform stream operations in Java 8 with a thread pool?

- Collections provide the method `parallelstream()` to create a stream that is processed by
  a thread pool. Alternatively you can call the intermediate method `parallel()` on a given
  stream to convert a sequential stream to a parallel counterpart.

### How can we access the thread pool that is used by parallel stream operations?

- The thread pool used for parallel stream operations can be accessed
  by `ForkjoinPool.commonPool()`. This way we can query its level of parallelism with `commonpool.getParallelism()`. The level cannot be changed at runtime but it can be configured by providing the following JVM parameter: `-Djava.util.concurrent.ForkjoinPool.common. parallelism=5`

### How to implement thread-safe code without using the synchronized keyword?

- **Atomic updates**: A technique in which you call atomic instructions like compare and set provided by the CPU
- **java.util.concurrent.locks.ReentrantLock**: A lock implementation that provides more flexibility than synchronized blocks
- **java.util.concurrent.locks.ReentrantReadWriteLock**: A lock implementation in which reads do not block reads
- **java.util.concurrent.locks.StampedLock** a nonreeantrant Read-Write lock with the possibility of optimistically reading values.
- **java.lang.ThreadLocal**: No need for synchronization if the mutable state is confined to a single thread. This can be done by using local variables or `java.lang.ThreadLocal`.

### What is the difference between calling `wait()` and `sleep()` method ?

- Though both `wait()` and `sleep()` introduce some form of pause in Java application, they are tools for different needs
- `wait()` method is used for inter thread communication, it relinquishes lock if waiting for a condition is true and waits for notification when, due to an action of another thread waiting condition becomes false.
- `sleep()` method is used just to relinquish CPU or stop execution of current thread for specified time duration. Calling sleep method doesn't release the lock held by current thread
- Also, wait for method in Java should be called from synchronized method or block while there is no such requirement for sleep() method.
- Another difference is Thread.sleep() method is a static method and applies on current thread, while wait() is an instance specific method and only got wake up if some other thread calls notify method on same object. Also, in the case of sleep, sleeping thread immediately goes to Runnable state after waking up while in the case of wait, waiting for a thread first acquires the lock and then goes into Runnable state. So based upon your need, if you want to pause a thread for specified duration then use sleep() method and if you want to implement inter-thread communication use wait method.
- Waiting thread can be awaken by calling notify and notifyAll while sleeping thread can not be awakened by calling notify method.
- wait is normally done on condition, Thread wait until a condition is true while sleep is just to put your thread on sleep.
- The wait() method is called on an Object on which the synchronized block is locked, while sleep is called on the Thread.
- CPU State: wait(): Thread is still in running mode and uses CPU cycles, sleep(): Thread does not consume any CPU cycles

### What is the difference between `wait` and `yield` methods

- wait() is declared in Java.lang.Object class while yield() is declared in Java.lang.Thread class.
- wait is overloaded method and has two versions - wait(), normal and timed wait() while yield() is not overloaded.
- wait is an instance method while yield() is a static method and work on current thread.
- When a Thread calls wait() it releases the monitor.
- wait() method must be called from either synchronized block or synchronized method, There is no such requirement for yield() method.
- it's advised to call wait() method inside loop but yield() is better to be called outside of loop.

### What is difference between `yield` and `sleep` method in Java?

- **1. Currently executing thread state**: sleep() method causes the currently executing thread to sleep for the number of milliseconds specified in the argument. yield() method temporarily pauses the currently executing thread to give a chance to the remaining waiting threads of the same priority to execute. If there is no waiting thread or all the waiting threads of low priority then the current thread will continue its execution.
- **2. Interrupted Exception**: Sleep method throws the Interrupted exception if another thread interrupts the sleeping thread. yield method does not throw Interrupted Exception.
- **3. Give up monitors**: Thread.sleep() method does not cause cause currently executing thread to give up any monitors while yield() method give up the monitors.

### What do you understand about Thread Priority?

- Every thread has a priority, usually higher priority thread gets precedence in execution but it depends on Thread Scheduler implementation that is OS dependent. We can specify the priority of thread but it doesn’t guarantee that higher priority thread will get executed before lower priority thread. Thread priority is an int whose value varies from 1 to 10 where 1 is the lowest priority thread and 10 is the highest priority thread.
- By **default**, a **thread** inherits the **priority** of its **parent** thread. You can increase or decrease the priority of any thread with the setPriority method. You can set the priority to any value between MIN_PRIORITY (defined as 1 in the Thread class) and MAX_PRIORITY (defined as 10).
- In a Multi threading environment, thread scheduler assigns processor to a thread based on priority of thread. Whenever we create a thread in Java, it always has some priority assigned to it. Priority can either be given by JVM while creating the thread or it can be given by programmer explicitly.
- **public static int MIN_PRIORITY**: This is minimum priority that a thread can have. Value for this is 1.
- **public static int NORM_PRIORITY**: This is default priority of a thread if do not explicitly define it. Value for this is 5.
- **public static int MAX_PRIORITY**: This is maximum priority of a thread. Value for this is 10.
- Get and Set Thread Priority:
  1. `public final int getPriority()`: Java.lang.Thread.getPriority() method returns priority of given thread.
  2. `public final void setPriority(int newPriority)` :Java.lang.Thread.setPriority() method changes the priority of thread to the value newPriority. This method throws IllegalArgumentException if value of parameter newPriority goes beyond minimum(1) and maximum(10) limit.

### What is Thread Scheduler and Time Slicing?

- Thread Scheduler is the Operating System service that allocates the CPU time to the available runnable threads. Once we create and start a thread, it’s execution depends on the implementation of Thread Scheduler. Time Slicing is the process to divide the available CPU time to the available runnable threads. Allocation of CPU time to threads can be based on thread priority or the thread waiting for longer time will get more priority in getting CPU time. Thread scheduling can’t be controlled by Java, so it’s always better to control it from application itself.
- Context Switching is the process of storing and restoring of CPU state so that Thread execution can be resumed from the same point at a later point of time. Context Switching is the essential feature for multitasking operating system and support for multi-threaded environment.

### Why Thread sleep() and yield() methods are static and join is non static?

- Thread sleep() and yield() methods work on the currently executing thread. So there is no point in invoking these methods on some other threads that are in wait state. That’s why these methods are made static so that when this method is called statically, it works on the current executing thread and avoid confusion to the programmers who might think that they can invoke these methods on some non-running threads.
- Join method is not static because it halts current thread execution till the joined thread completes it's execution so to know which thread needs to be joined it requires instance of that thread.

### What is the difference between `synchronized` and `volatile` ?

- The `volatile` keyword in Java is a field modifier whereas `synchronized` modifies code blocks and methods.
- `synchronized` obtains and releases the lock on monitor while `volatile` doesn't require that.
- Threads in Java can be blocked or waiting for any monitor in case of `synchronized`, that is not the case with the `volatile` keyword in Java.
- `synchronized` method affects performance more than a `volatile` keyword in Java.
- Since `volatile` keyword in Java only synchronizes the value of one variable between Thread memory and main memory while `synchronized` synchronizes the value of all variable between thread memory and main memory and locks and releases a monitor to boot. Due to this reason `synchronized` keyword in Java is likely to have more overhead than volatile.
- You can not `synchronize` on the `null` object but your `volatile` variable in Java could be `null`.
- From Java 5 writing into a `volatile` field has the same memory effect as a monitor release, and reading from a `volatile` field has the same memory effect as a monitor acquire

### What is Executor framework in Java?

- Executor and ExecutorService are used for following purposes :
  - creating thread in java,
  - starting threads in java,
  - managing whole life cycle of Threads in java.
- `Executor` creates pool of threads and manages life cycle of all threads in it. In `Executor` framework, `Executor` interface and `ExecutorService` class are most prominently used in java. `Executor` interface defines very important `execute()` method which executes command in Java.
- `ExecutorService` interface extends Executor interface.
- An `Executor` interface provides following type of methods:
  - methods for managing termination
  - methods that can produce a Future for tracking progress of tasks in java.

### What are differences between execute() and submit() method of Executor framework?

| execute()                                  | submit()                                                                                                                                    |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| It is defined in Executor interface.       | It is defined in ExecutorService interface                                                                                                  |
| It can be used for executing runnable task | It can be used for executing runnable or callable task.                                                                                     |
| void execute(Runnable task)                | It has 3 forms `<T> Future<T> submit(Callable<T> task)`, `<T> Future<T> submit(Runnable task, T result)`, `Future<?> submit Runnable(task)` |

### What is a Semaphore?

- Conceptually, a semaphore maintains a set of permits. Each `acquire()` blocks if necessary until a permit is available, and then takes it. Each `release()` adds a permit, potentially releasing a blocking acquirer. However, no actual permit objects are used; the Semaphore just keeps a count of the number available and acts accordingly. Semaphores are often used to restrict the number of threads that can access some (physical or logical) resource.

### What is CountDownLatch? Can you design a Custom CountDownLatch ?

- A CountDownLatch is initialized with a given **count**. **count** specifies the number of events that must occur before latch is released. Every time a event happens **count** is reduced by 1. Once **count** reaches 0 latch is released.

### What is Java Thread Dump, How can we get Java Thread dump of a Program?

- A Java thread dump is a way of finding out what every thread in the JVM is doing at a particular point in time. This is especially useful if your Java application sometimes seems to hang when running under load, as an analysis of the dump will show where the threads are stuck.
- You can generate a thread dump under Unix/Linux by running `kill -QUIT <pid>`, and under Windows by hitting `Ctl + Break`.
- Thread dump is the list of all the threads, every entry shows information about thread which includes following in the order of appearance.
  - **Thread Name**: Name of the Thread
  - **Thread Priority**: Priority of the thread
  - **Thread ID**: Represents the unique ID of the Thread
  - **Thread Status**: Provides the current thread state, for example RUNNABLE, WAITING, BLOCKED. While analyzing deadlock look for the blocked threads and resources on which they are trying to acquire lock.
  - **Thread callstack**: Provides the vital stack information for the thread. This is the place where we can see the locks obtained by Thread and if it’s waiting for any lock.

### What will happen if we don’t override Thread class run() method?

- If we don't override run() method in our defined thread then Thread class run() method will be executed and we will not get any output because Thread class run() is with an empty implementation.
- It is highly recommended to override run() method because it improves the performance of the system. If we override run() method in the user-defined thread then in run() method we will define a job and Our created thread is responsible to execute run() method.

```java
abstract class NotOverridableRunMethod extends Thread {
	abstract public void run();
}

class ParentMain {
	public static void main(String[] args) {
		OverrideRunMethod orn = new OverrideRunMethod();
		orn.start();
		System.out.println("Thread class run() method will be executed with empty implementation");
	}
}
```

Output

```
cmd> java ParentMain
Thread class run() method will be executed with empty implementation
I am in run() method
```

### What is difference between the Thread class and Runnable interface for creating a Thread?

| THREAD CLASS                                                                                                                        | RUNNABLE INTERFACE                                                                                                   |
| ----------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Each thread creates a unique object and gets associated with it.                                                                    | Multiple threads share the same objects.                                                                             |
| As each thread create a unique object, more memory required.                                                                        | As multiple threads share the same object less memory is used.                                                       |
| In Java, multiple inheritance not allowed hence, after a class extends Thread class, it can not extend any other class.             | If a class define thread implementing the Runnable interface it has a chance of extending one class.                 |
| A user must extend thread class only if it wants to override the other methods in Thread class.                                     | If you only want to specialize run method then implementing Runnable is a better option.                             |
| Extending Thread class introduces tight coupling as the class contains code of Thread class and also the job assigned to the thread | Implementing Runnable interface introduces loose coupling as the code of Thread is separate form the job of Threads. |

### What is Lock interface in Java Concurrency API? What is the Difference between ReentrantLock and Synchronized?

- A `java.util.concurrent.locks.Lock` is a thread synchronization mechanism just like synchronized blocks. A Lock is, however, more flexible and more sophisticated than a synchronized block. Since Lock is an interface, you need to use one of its implementations to use a Lock in your applications. `ReentrantLock` is one such implementation of Lock interface.

```java
Lock lock = new ReentrantLock();

lock.lock();

//critical section
lock.unlock();
```

- **Difference between Lock Interface and synchronized keyword**
  - Having a timeout trying to get access to a `synchronized` block is not possible. Using `Lock.tryLock(long timeout, TimeUnit timeUnit)`, it is possible.
  - The `synchronized` block must be fully contained within a single method. A Lock can have it’s calls to `lock()` and `unlock()` in separate methods.

### What is Java Memory Model (JMM)? Describe its purpose and basic ideas.

- The Java memory model used internally in the JVM divides memory between thread stacks and the heap. Each thread running in the Java virtual machine has its own thread stack. The thread stack contains information about what methods the thread has called to reach the current point of execution.
- The thread stack also contains all local variables for each method being executed (all methods on the call stack). A thread can only access it's own thread stack. Local variables created by a thread are invisible to all other threads than the thread who created it. Even if two threads are executing the exact same code, the two threads will still create the local variables of that code in each their own thread stack. Thus, each thread has its own version of each local variable.
- All local variables of primitive types ( boolean, byte, short, char, int, long, float, double) are fully stored on the thread stack and are thus not visible to other threads. One thread may pass a copy of a pritimive variable to another thread, but it cannot share the primitive local variable itself.
- The heap contains all objects created in your Java application, regardless of what thread created the object. This includes the object versions of the primitive types (e.g. Byte, Integer, Long etc.). It does not matter if an object was created and assigned to a local variable, or created as a member variable of another object, the object is still stored on the heap.

### Describe the conditions of livelock and starvation?

- **Livelock** occurs when two or more processes continually repeat the same interaction in response to changes in the other processes without doing any useful work. These processes are not in the waiting state, and they are running concurrently. This is different from a deadlock because in a deadlock all processes are in the waiting state.
- **Starvation** describes a situation where a greedy thread holds a resource for a long time so other threads are blocked forever. The blocked threads are waiting to acquire the resource but they never get a chance. Thus they starve to death.
- Starvation can occur due to the following reasons:
  - Threads are blocked infinitely because a thread takes long time to execute some synchronized code (e.g. heavy I/O operations or infinite loop).
  - A thread doesn’t get CPU’s time for execution because it has low priority as compared to other threads which have higher priority.
  - Threads are waiting on a resource forever but they remain waiting forever because other threads are constantly notified instead of the hungry ones.

### How do I share a variable between 2 Java threads?

- We should declare such variables as static and volatile.
- **Volatile variables** are shared across multiple threads. This means that individual threads won’t cache its copy in the thread local. But every object would have its own copy of the variable so threads may cache value locally.
- We know that static fields are shared across all the objects of the class, and it belongs to the class and not the individual objects. But, for static and non-volatile variable also, threads may cache the variable locally.

### What is atomic variable in Java?

- Atomic variables allow us to perform atomic operations on the variables. The most commonly used atomic variable classes in Java are `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, and `AtomicReference`. These classes represent an int, long, boolean and object reference respectively which can be atomically updated. The main methods exposed by these classes are:
  - `get()`: gets the value from the memory, so that changes made by other threads are visible; equivalent to reading a volatile variable
  - `set()`: writes the value to memory, so that the change is visible to other threads; equivalent to writing a volatile variable
  - `lazySet()`: eventually writes the value to memory, may be reordered with subsequent relevant memory operations. One use case is nullifying references, for the sake of garbage collection, which is never going to be accessed again. In this case, better performance is achieved by delaying the null volatile write
  - `compareAndSet()`: same as described in section 3, returns true when it succeeds, else false
  - `weakCompareAndSet()`: same as described in section 3, but weaker in the sense, that it does not create happens-before orderings. This means that it may not necessarily see updates made to other variables

### What is Phaser in Java concurrency?

- The Phaser allows us to build logic in which **threads need to wait on the barrier before going to the next step of execution**. Phaser is similar to other synchronization barrier utils like CountDownLatch and CyclicBarrier.
- **CountDownLatch vs CyclicBarrier vs Phaser**
  1. **CountDownLatch**:
     - Created with a fixed number of threads
     - Cannot be reset
     - Allows threads to wait(CountDownLatch#await()) or continue with its execution(CountDownLatch#countDown()).
  2. **CyclicBarrier**:
     - Can be reset.
     - Does not a provide a method for the threads to advance. The threads have to wait till all the threads arrive.
     - Created with fixed number of threads.
  3. **Phaser**:
     - Number of threads need not be known at Phaser creation time. They can be added dynamically.
     - Can be reset and hence is, reusable.
     - Allows threads to wait(Phaser#arriveAndAwaitAdvance()) or continue with its execution(Phaser#arrive()).
     - Supports multiple Phases(hence the name phaser).

### What is Synchronization in Java?

- Synchronization in Java is the capability to control the access of multiple threads to any shared resource.
- Java Synchronization is better option where we want to allow only one thread to access the shared resource.

### Why do we need Synchronization in Java?

- If your code is executing in a multi-threaded environment, you need synchronization for objects, which are shared among multiple threads, to avoid any corruption of state or any kind of unexpected behavior. Synchronization in Java will only be needed if shared object is mutable. if your shared object is either read-only or immutable object, then you don't need synchronization, despite running multiple threads. Same is true with what threads are doing with an object if all the threads are only reading value then you don't require synchronization in Java. JVM guarantees that Java synchronized code will only be executed by one thread at a time.
- **Java synchronized Keyword provides following functionality essential for concurrent programming**:
  1. The `synchronized` keyword in Java provides locking, which ensures mutually exclusive access to the shared resource and prevents data race.
  2. `synchronized` keyword also prevent reordering of code statement by the compiler which can cause a subtle concurrent issue if we don't use synchronized or volatile keyword.
  3. `synchronized` keyword involve locking and unlocking. before entering into synchronized method or block, thread needs to acquire the lock, at this point it reads data from main memory and caches it when release the lock, it flushes write operation into main memory which eliminates memory inconsistency errors.

### Concept of Lock in Java

- Synchronization is built around an internal entity known as the lock or monitor. Every object has a lock associated with it. By convention, a thread that needs consistent access to an object's fields has to acquire the object's lock before accessing them, and then release the lock when it's done with them.

### Java synchronized method

- If you declare any method as synchronized, it is known as synchronized method. Synchronized method is used to lock an object for any shared resource. When a thread invokes a synchronized method, it automatically acquires the lock for that object and releases it when the thread completes its task.

### For starting a Thread we call thread.start() which internally invokes run() method. What if we call run() method directly without using start() method ?

- When `start()` method is invoked, it internally invokes `start0`, which is a native method call : `private native void start0();`
- The basic purpose of `start()` method is to instruct running Operating System to create a new Thread which can be executed independently of Thread created it.
- **when start() method is invoked, Thread will be created and it executes run() method of Task submitted.**
- By calling **thread.run()** directly, will not create a new Thread instead it will call run method of the task submitted on the same caller thread. So there will be no separate execution for newly created Thread.

![start_vs_run](/images/start_vs_run.png)

### What is volatile variable in Java?

- `volatile` is a special modifier, which can only be used with instance variables. In concurrent Java programs, changes made by multiple threads on instance variables is not visible to other in absence of any synchronizers e.g. synchronized keyword or locks. Volatile variable guarantees that a write will happen before any subsequent read
- Most of the time while writing game we use a variable bExit to check whether user has pressed exit button or not, value of this variable is updated in event thread and checked in game thread, So if we don't use volatile keyword with this variable, Game Thread might miss update from event handler thread if it's not synchronized in Java already. volatile keyword in Java guarantees that value of the volatile variable will always be read from main memory and "happens-before" relationship in Java Memory model will ensure that content of memory will be communicated to different threads.

```java
private boolean bExit;
 while(!bExit) {
    checkUserPosition();
    updateUserPosition();
 }
```

- In this code example, One Thread (Game Thread) can cache the value of "bExit" instead of getting it from main memory every time and if in between any other thread (Event handler Thread) changes the value; it would not be visible to this thread. Making boolean variable "bExit" as volatile in Java ensures this will not happen.

### What is the use of join method in case of threading in Java?

- join() method is used for waiting the thread in execution until the thread on which join is called is not completed.

![join_method](/images/join_method.png)

- Remember, the thread which will wait is the thread in execution and it will wait until the thread on which join method called is not completed.
- Lets take a scenario, we have Main thread, Thread 1, Thread 2 and Thread 3 and we want our thread to execute in particular scenario like,
  - Main thread to start first and ends only after all 3 threads is completed.
  - Thread 1 to start and complete.
  - Thread 2 to start only after Thread 1 is completed.
  - Thread 3 to start only after Thread 2 is completed.

### How join method works internally.

- join() method is used for waiting the thread in execution until the thread on which join is called is not completed. Remember, the thread which will wait is the thread in execution and it will wait until the thread on which join method called is not completed.

### Who calls notify/notifyAll in case of thread waiting on join method?

- After run() method of thread is completed, it doesn't mean thread task is completed, It has to do many other tasks like
  1. Destroying the associated stack,
  2. Setting the necessary threadStatus etc.
- One of the task is notifying the waiting threads, So that Thread waiting on join() method will be notified that thread has completed its task and joined threads can resume.
  Above task are executed inside native thread call, so it wont be visible in Java thread API.

### When join method is invoked, does thread release its resources and goes in waiting state or it keep resources and goes in waiting state?

- This topic is about the behavior of the join method and its comparison to the wait method. - **I am making this comparison because the join method calls the wait method internally**.
- **wait()** : If a thread calls wait method on an object (obviously that thread must already have acquired the lock of the object it is calling the wait method on, otherwise a runtime exception is thrown), that thread releases the object and goes to the waiting state. So in all the wait call forces the thread to release the object it is calling wait on.
- **join()** : If a thread (name it **T1**) calls the join method on another Thread (remember the wait method is in the Object class and the join method is specific to the Thread class and is present in the Thread class) named **T2**, then T1 waits for T2 to complete its execution before it continues from that point. Now think what will happen.
- **Will thread T1 leave the locks on the object it has acquired lock on or not ?**
  If we consider the fact that **the join method internally calls the wait method** the answer that pops up immediately is that “**obviously it should leave the lock**“, which is WRONG !!!!
- For a moment lets assume this answer is correct. Now imagine the scenario, where the thread T1 has called this wait inside a synchronized lock where it has acquired a lock on a String object “abc” and has called the T2.join() inside this block. Now as this call has been made by T1 inside the synchronized block, it obviously means that it needs the lock on the “abc” object to continue the execution further. If T1 has released the lock on “abc” object it will be rescheduled by the thread scheduler to acquire lock on “abc”. But it is not guaranteed that it will acquire the lock as soon as the thread T2 completes. So in this case some other thread T3 might acquire the lock on “abc” and enter the same synchronized block which the thread T1 already half way !!!!
- **So if the join method makes the calling thread leave the locks, then whole use of the synchronized block is ruined.**
- **wait() inside join() mystery** : The wait method call inside the join method implementation is being called on “this” i.e the thread on which join was called (T2). That wait call does not have to do anything with the calling thread (T1). That means the join call does not causes the calling thread to release a lock on any object and it simply makes it wait till the thread it called join on completes its execution keeping all the locks it already acquired.

### Can Thread be created without any ThreadGroup, I mean can Thread exist independently without associated to any ThreadGroup?

- **No**. Thread cannot be created independently, it will be part of atleast one of the Thread Group. Generally, while creating Thread we do not associate it to any Thread Group, but internally it will be part of "**main**" Thread Group.

### Say Thread "t1" is spawned from "main" thread, what happen when RuntimeException is thrown from "t1", Will "main" thread continue to run?

- **Yes**. "main" thread will continue to run if an exception is thrown from threads that are created within main thread. Let's see example and understand. In general, Exception thrown by one thread will not affect another thread, as all threads are independent and have different stack.

### How exceptions are handled in case of Multithreading scenario? who will handle Exceptions if there is no handler?

- Exceptions that are thrown from Thread can be handled in 3 different ways,
  1. **At Thread Level** : Each threads have there own Exception handling mechanism
  2. **At ThreadGroup Level** : Each ThreadGroup have there own Exception handling mechanism which will be applicable to all thread within group.
  3. **At Global Thread Level** : Default Exception Handler can be configured at Global thread level which will be applicable to all the threads.
- When an uncaught exception occurs from a particular Thread, the JVM looks for handler in way shown below,
  1. First JVM will look whether **UncaughtExceptionHandler** (setUncaughtExceptionHandler) for the current **Thread** is set or not. If set than exception will be catch by Thread handler. If not set than exception will be propagated up the call stack.
  2. Second JVM will check whether uncaughtException of ThreadGroup is overriden or not, JVM will not only check uncaughtException handler of direct ThreadGroup of which Thread is part of, but JVM will also look at all the parent ThreadGroups as well. If uncaughtException is overriden by any one of ThreadGroup handler than exception will be caught by that ThreadGroup handler. If not set than exception will be propagated up the call stack.
  3. Third JVM will check whether DefaultUncaughtExceptionHandler (setDefaultUncaughtExceptionHandler) at JVM level(Global Thread level) is configured or not, It will act as handler for all the Threads in JVM. If set than exception will be catch by Global Thread handler. If not set than exception will be propagated up the call stack.
  4. When there is no handler configured, Threadgroup class ("main" threadgroup to which main thread is part of) provide default implementation of uncaughtException() method that is called which prints Exception as shown below and JVM shutdowns.

### In case of Exception, What happen to lock which is acquired by Thread, will that be released?

- When an Exception is thrown from the Thread which hold lock on some resource say "obj", Thread will release a lock on "obj", so that Thread in which Exception occured can be terminated but other threads can still progress.
- If say thread doesn't release lock on Exception, if this is the case then it may result in deadlock.
- Say Thread "thread1" is waiting for lock on "obj" to enter in synchronized block.
- Say Thread "thread2" is holding lock on "obj" and doing some operation and now if thread2 throws exception then "thread1" will be blocked and cannot progress.
- If execution of the synchronized block completes normally, then the lock is unlocked and the synchronized statement completes normally.
- If execution of the synchronized block completes abruptly for any reason, then the lock is unlocked and the exception is thrown until it finds exception handler up the call stack.

### Is it possible to take a lock on null reference? What is the output of the Program?

- **Lock cannot be acquired on null reference.** calling a synchronized method on object and intrinsically acquiring a lock on that object is similar to acquiring extrinsic lock by using synchronized() block. `public void synchronized method1(){}` calling `obj1.method1()` will hold a lock on obj1 (obj1 cannot be null otherwise NPE will be thrown)
- Similarly, `public void method1(){ synchronized(obj1){}}` obj1 at this point should also be not null for holding a lock on obj1.

### When we cannot take a lock on null reference, what will happen if we make a reference null after acquiring a lock on object it is referering to? What is the output of the Program?

```java
class SynchronizationExample {
   private static SynchronizationExample synchronizationExample =
       new SynchronizationExample();
   public static void main(String ar[]) {
       hello();
 }

 private static void hello(){
  synchronized (synchronizationExample) {
       System.out.println("Before making reference null");
       synchronizationExample = null;
       System.out.println("After making reference null");
      }
  }
}

// output  : This is perfectly fine and ouputs
// Before making reference null
// After making reference null
```

### What is Polling and what are problems with it?

- The process of testing a condition repeatedly till it becomes true is known as polling. Polling is usually implemented with the help of loops to check whether a particular condition is true or not. If it is true, certain action is taken. This waste many CPU cycles and makes the implementation inefficient.
- For example, in a classic queuing problem where one thread is producing data and other is consuming it.

### How Java multi threading tackles this problem?

- To avoid polling, Java uses three methods, namely, wait(), notify() and notifyAll().
  All these methods belong to object class as final so that all classes have them. They must be used within a synchronized block only.

### Why wait, notify and notifyAll is defined in Object Class and not on Thread class in Java?

- First of all, let us know what purpose do these mehods fulfil
  **wait()** - Tells the current thread to release the lock and go to sleep until some other thread enters the same monitor and calls notify().
  **notify()** - Wakes up the single thread that is waiting on this object's monitor.
  **notifyAll()** - It wakes up all the threads that called wait() on the same object.
- Following above definition, we can conclude that wait() and notify() work at the monitor level and monitor is assigned to an object not to a particular thread. Hence, wait() and notify() methods are defined in Object class rather than Thread class. If wait() and notify() were on the Thread instead then each thread would have to know the status of every other thread and there is no way to know thread1 that thread2 was waiting for any resource to access.Hence, notify, wait, notifyAll methods are defined in object class in Java.
- Suppose there is a joint bank account and hence multiple users can use the same account for transactions through multiple channels.Currently, the account having a balance of 1500/- and minimum amount balance to remain in the account is 1000/-.Now, the first user is trying to withdraw an amount of 500/- through ATM and another user is trying to purchase any goods worth 500/- through swipe machine.Here whichever channel first access the account to perform the transaction acquires the lock on the account at first and the other channel will wait untill the transaction is completed and the lock on the account is released as there is no way to know which channel has already acquired the lock and which channel is waiting to acquire a lock. Hence lock is always applied on the account itself rather than a channel.Here, we can treat account as an object and channel as a thread.

### Why wait, notify and notifyAll is called from synchronized block only?

- If we don't call wait() or notify() method from synchronized method/block, it will throw the "Java.lang.IllegalMonitorStateException: current thread not owner". **Why?**
- There is possibility of race condition between wait() and notify() in Java which could exist if we don't call them inside synchronized method or block.
- For example, one thread read data from a buffer and one thread write data into the buffer. The reading data thread needs to wait until the writing data thread completely writes a block data into the buffer. The writing data thread needs to wait until the reading data thread completely read the data from the buffer.
- If wait(), notify(), and notifyAll() methods can be called by an ordinary method, the reading thread calls wait() and the thread is being added to waiting queue. At the same moment, the writing thread calls notify() to signal the condition changes. The reading thread misses the change and waits forever. Hence, they must be called inside a synchronized method or block which is **mutually exclusive.**
- There are few important points we should be aware of before going into the reasons for why wait(), notify() and notifyAll() must be called inside a synchronized method or block.
  1. Every object created in Java has one associated monitor (mutually exclusive lock). Only one thread can own a monitor at any given time.
  2. For achieving synchronization in Java this monitor is used. When any thread enters a synchronized method/block it acquires the lock on the specified object.
  3. When any thread acquires a lock it is said to have entered the monitor. All other threads which need to execute the same shared piece of code (locked monitor) will be suspended until the thread which initially acquired the lock releases it.
  4. wait() method tells the current thread (thread which is executing code inside a synchronized method or block) to give up monitor and go to waiting state.
- **Another example**
- Calling notify() or notifyAll() methods issues a notification to a single or multiple thread that a condition has changed and once notification thread leaves synchronized block, all the threads which are waiting fight for object lock on which they are waiting and lucky thread returns from wait() method after re-acquiring the lock and proceed further.
- Let’s divide this whole operation into steps to see a possibility of race condition between wait() and notify() method in Java, we will use Produce Consumer thread example to understand the scenario better:
  1. The Producer thread tests the condition (buffer is full or not) and confirms that it must wait (after finding buffer is full).
  2. The Consumer thread sets the condition after consuming an element from a buffer.
  3. The Consumer thread calls the notify () method; this goes unheard since the Producer thread is not yet waiting.
  4. The Producer thread calls the wait () method and goes into waiting state.
- So due to race condition here we potentially lost a notification and if we use buffer or just one element, Producer thread will be waiting forever and your program will hang.

![wait/notify](/images/wait_notify.png)

- This race condition is resolved by using synchronized keyword and locking provided by Java. In order to call the wait(), notify() or notifyAll() methods in Java, we must have obtained the lock for the object on which we're calling the method.
- Since the wait() method in Java also releases the lock prior to waiting and reacquires the lock prior to returning from the wait() method, we must use this lock to ensure that checking the condition (buffer is full or not) and setting the condition (taking element from buffer) is atomic which can be achieved by using synchronized method or block in Java.

### Why wait(), notify() and notifyAll() must be called from inside loop?

- You need not only to loop it but check your condition in the loop. Java does not guarantee that your thread will be woken up only by a notify()/notifyAll() call or the right notify()/notifyAll() call at all.
- A thread can also wake up without being notified, interrupted, or timing out, a so-called spurious wakeup. While this will rarely occur in practice, applications must guard against it by testing for the condition that should have caused the thread to be awakened, and continuing to wait if the condition is not satisfied. In other words, waits should always occur in loops, like this one:

```java
synchronized (obj) {
while (<condition does not hold>)
    obj.wait(timeout);
}
```

- Waking up from a wait() doesn't mean the condition you were waiting for has happened! So you generally need to wait in a loop until the condition holds (or until you decide to give up).
- The timed wait waits forever if you pass in a zero wait time! So be careful to check for zero if you are calculating the wait time...!
- Choosing notify() when notifyAll() is required can make threads stall.
- Conversely, calling notifyAll() when only notify() is required is generally benign, but may reduce program throughput.
- Calling wait() automatically releases the lock of the object you are waiting on, but does not release locks on other objects! So in the following case, the caller will still hold the lock to object1 during the wait:

```java
	synchronized (object1) {
		synchronized (object2) {
			object2.wait();
 		}}
```

- Calling notify() does not "transfer control" to notified threads immediately: it merely marks them as "runnable". A notified thread will not be able to run until the following things happen: (1) the notifying thread releases its lock on the object being notified; (2) the thread scheduler next schedules the notified thread. (On some systems such as Windows, this will happen fairly quickly, because threads are given a temporary priority boost when woken from a wait state, but it generally won't happen until the next interrupt.)

### Producer Consumer Problem in Java

- First confusion arise from the fact, how to call wait() method? Since wait method is not defined in Thread class, you cannot simply call Thread.wait(), that won't work but since many Java developers are used to calling Thread.sleep() they try the same thing with wait() method and stuck.
- You need to call **wait()** method on the object which is shared between two threads, in producer-consumer problem its the queue which is shared between producer and consumer threads.
- The second confusion comes from the fact that wait needs to be a call from synchronized block or method? So if you use synchronized block, which object should be put to go inside the block? This should be the same object, whose lock you want to acquire i.e. the shared object between multiple threads. In our case it's queue.

### Explain yield, sleep, isAlive, interrupt methods.

- **yield()**: Suppose there are three threads t1, t2, and t3. Thread t1 gets the processor and starts its execution and thread t2 and t3 are in Ready/Runnable state. Completion time for thread t1 is 5 hour and completion time for t2 is 5 minutes. Since t1 will complete its execution after 5 hours, t2 has to wait for 5 hours to just finish 5 minutes job. In such scenarios where one thread is taking too much time to complete its execution, we need a way to prevent execution of a thread in between if something important is pending. yield() helps us in doing so. yield() basically means that the thread is not doing anything particularly
  important and if any other threads or processes need to run, they should run. Otherwise, the current thread will continue to run.
- **Use of yield method:**
  - Whenever a thread calls Java.lang.Thread.yield method, it gives hint to the thread scheduler that it is ready to pause its execution. Thread scheduler is free to ignore this hint.
  - If any thread executes yield method , thread scheduler checks if there is any thread with same or high priority than this thread. If processor finds any thread with higher or same priority then it will move the current thread to Ready/Runnable state and give processor to other thread and if not – current thread will keep executing.
- **Note**:
  - Once a thread has executed yield method and there are many threads with same priority is waiting for processor, then we can't specify which thread will get execution chance first.
  - The thread which executes the yield method will enter in the Runnable state from Running state.
  - Once a thread pauses its execution, we can't specify when it will get chance again it depends on thread scheduler.
  - Underlying platform must provide support for preemptive scheduling if we are using yield method.
- **yield does** not have any synchronization semantic. If thread holds **lock**, it will continue to hold it. Only wait methods of the Object class **release** the intrinsic **lock** of the current instance (the thread may have other **locks** acquired, they don't get **released**). **Yield**, sleep, join **do** not bother about **locks**.
- **sleep()** : This method causes the currently executing thread to sleep for the specified number of milliseconds, subject to the precision and accuracy of system timers and schedulers.
- **Java Thread Sleep important points**
  1. It always pause the current thread execution.
  2. The actual time thread sleeps before waking up and start execution depends on system timers and schedulers. For a quiet system, the actual time for sleep is near to the specified sleep time but for a busy system it will be little bit more.
  3. Thread sleep doesn’t lose any monitors or locks current thread has acquired.
  4. Any other thread can interrupt the current thread in sleep, in that case InterruptedException is thrown.
  5. With sleep() in Java it's not guaranteed that when sleeping thread woke up it will definitely get CPU, instead it will go to Runnable state and fight for CPU with other thread.
  6. There is a **misconception** about sleep method in Java that calling t.sleep() will put Thread "t" into sleeping state, that's not true because Thread.sleep method is a static method it always put the current thread into Sleeping state and not thread "t".
- **How Thread Sleep Works**
  - Thread.sleep() interacts with the thread scheduler to put the current thread in wait state for specified period of time. Once the wait time is over, thread state is changed to runnable state and wait for the CPU for further execution. So the actual time that current thread sleep depends on the thread scheduler that is part of operating system.The thread however does not lose ownership of any monitors.
  - So, when you call the sleep() method, Thread leaves the CPU and stops its execution for a period of time. During this time, it's not consuming CPU time, so the CPU can be executing other tasks. When Thread is sleeping and is interrupted, the method throws an InterruptedException exception immediately and doesn't wait until the sleeping time finishes.
- **isAlive()** : It tests if this thread is alive. A thread is alive if it has been started and has not yet died. There is a transitional period from when a thread is running to when a thread is not running. After the run() method returns, there is a short period of time before the thread stops. If we want to know if the start method of the thread has been called or if thread has been terminated, we must use isAlive() method. This method is used to find out if a thread has actually been started and has yet not terminated.
- Return Value: returns **true** if the thread upon which it is called is still running. It returns **false** otherwise.
- **interrupt()** If any thread is in sleeping or waiting state (i.e. sleep() or wait() is invoked), calling the interrupt() method on the thread, breaks out the sleeping or waiting state throwing InterruptedException. If the thread is not in the sleeping or waiting state, calling the interrupt() method performs normal behaviour and doesn't interrupt the thread but sets the interrupt flag to true.
- The `isInterrupted()` method returns the interrupted flag either true or false. The static interrupted() method returns the interrupted flag after that it sets the flag to false if it is true.

### When InvalidMonitorStateException is thrown? Why?

- This exception is thrown when you try to call wait()/notify()/notifyAll() any of these methods for an Object from a point in your program where u are NOT having a lock on that object.(i.e. u r not executing any synchronized block/method of that object and still trying to call wait()/notify()/notifyAll())
- wait(), notify() and notifyAll() all throw IllegalMonitorStateException. since This exception is a subclass of RuntimeException so we r not bound to catch it (although u may if u want to). and being a RuntimeException this exception is not mentioned in the signature of wait(), notify(), notifyAll() methods.

### Can we initialize thread with Thread.currentThread()?

- **No**. The Thread.currentThread() method returns a reference to the Thread instance executing currentThread() . This way you can get access to the Java Thread object representing the thread executing a given block of code. Once you have a reference to the Thread object, you can call methods on it. For instance, you can get the name of the thread currently executing the code like this: `String threadName = Thread.currentThread().getName();`

### Static synchronize method vs non-static synchronize method

### How locking works in synchronization?

### Does it require to synchronize volatile getter and setter methods

### What is thread call-stack

### Re-entrant lock implementation?

### What is difference between ArrayBlockingQueue & LinkedBlockingQueue in Java Concurrency?

### What is PriorityBlockingQueue in Java Concurrency?

### What is DelayQueue in Java Concurrency?

### What is SynchronousQueue in Java?

### What is Exchanger in Java concurrency?

### What is Busy Spinning? Why will you use Busy Spinning as wait strategy?
