### Multithreading

- Each program (browser, media player) is known as a **process** that is being executed.
- Each process is able to run simultaneous sub-tasks within itself, called **threads**
- You can think of a thread as a slice of the process itself.
- Every process triggers at least one thread on startup, which is called the **main thread**.
- Multithreading is about running multiple threads within a single process.
- Each process has its own chunk of memory assigned by the operating system.
- By default that memory cannot be shared with other processes.
- By default, two or more processes have no way to share data, unless they perform advanced tricks - the so-called inter-process communication.
- Unlike processes, threads share the same chunk of memory assigned to their parent process by the OS, so it is easier for two threads to talk to each other.
- **Green threads**, also known as **fibers** are a kind of emulation that makes multithreaded programs work in environments that don't provide that capability.
- **Concurrency** as the _perception_ of having tasks that run at the same time, while **true parallelism** as tasks that literally run at the same time.
- Things run smoothly as long as two or more threads _read_ from the same memory location. The troubles kick in when at least one of them _writes_ to the shared memory, while others are reading from it. Two problems can occur at this point:
- **data race** - while a writer thread modifies, a reader thread might be reading from it. If the writer has not finished its work yet, the reader will get corrupted data.
- **race condition** - a reader thread is suppose to read only after a writer has written. What if the opposite happens? More subtle than a data race, a race condition is about two or more threads doing their job in an unpredictable order, when in fact the operations should be performed in the proper sequence to be done correctly. Your program can trigger a race condition even if it has been protected against data races.
- A piece of code is said to be **thread-safe** if it works correctly, that is without data races or race conditions, even if many threads are executing simultaneously.
- The art of accommodating two or more concurrent threads is called **concurrency control**: os and programming languages offer several solutions to take care of it. The most important ones are :
  - **synchronization** - a way to ensure that resources will be usd by only one thread at a time. It is about marking specific parts of your code as "protected"
  - **atomic operations** - a bunch of non-atomic operations can be turned into atomic ones thanks to special instructions provided by the os.
  - **immutable data** - shared data is marked as immutable.
- OS maintains a dynamic priority for each thread. _Dynamic Priority = Static priority + Bonus_
- Static priority can be set by developers.
- When can we interrupt a thread:
  - If the thread is executing a method that throws an `InterruptedException`
  - If the thread's code is handling the interrupt signal explicitly.
- Daemon threads : Background threads the do not prevent the application from exiting if the main thread terminates. Some scenarios when we want to make a thread daemon
  - Background tasks, that should not block our application from terminating. e.g File saving thread in a text editor.
  - code in a worker thread is not under our control, and we don't want it to block our application from terminating. e.g worker thread that uses an external library
- Unfortunately `System.in.read()` does not respond to `Thread.interrupt()`;
- We should always pass the value about how long we are willing to wait for the worker thread to thread.join() method
- latency : IOW the faster the transaction is the more performant the application is considered to be. (high speed trading system)
- Latency : The time to completion of a task. Measured in time units.
- Throughput : The amount of tasks completed in a given period. Measured in tasks/time unit.
- Think of synchronized as a door to a room, if one of the door is locked all other room's door gets locked.
- All reference assignment are atomic
- All getters and setters are atomic
- all assignments to primitive types are safe except long and double.
- Assignments to `long` and `double` if declared `volatile` are atomic
- Volatile makes assignments to long atom, however **incrementing a volatile variable still involves multiple operations**.
- Data Race - Problem
  - Compiler and CPU may execute the instructions Out of Order to optimize performance and utilization. One example could be

```java
x++;
y++;
```

- Since both the instructions are independent they can be executed out of order by the CPU.
- Vectorization - parallel instruction execution (SIMD)
- `tryLock` is useful in Real Time Applications where suspending a thread on a `lock()` method is unacceptable. for e.g.
  - Video/Image processing
  - High Speed/Low latency trading systems
  - User Interface applications
- We absolutely need to check the return value of the `tryLock()` to determine if we can use the shared resource or not. In addition, if we call `lock.unlock()` on a lock that another thread has acquired, we will get an exception and our thread will crash
- `ReentrantReadWriteLock` - When to use?
  - When read operations are predominant
  - Or when the read operations are not as fast : read from many variables, read from a complex data structure
  - Mutual exclusion of reading threads negatively impacts the performance
- Multiple threads can acquire the readLock. Only a single thread is allowed to lock a writeLock.
- Semaphore doesn't have a notion of owner thread.
- Deadlock are nastiest problem unless we have deadlock detection and resolution code in place.
- The more locks we use the more chances of deadlocks.
- Priority inversion : Two threads with different priority set.
- Thread not releasing a lock (Kill Tolerance)
