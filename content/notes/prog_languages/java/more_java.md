+++
title = 'More on Java'
date = 2024-02-11

+++

### Java Gotchas

- Constructor in Java can't be `abstract`, `static`, `final` or `synchronized`
- We may request JVM for garbage collection by calling `system.gc()` method. But this doesn't guarantee that JVM will perform the garbage collection
- Abstract methods can never be `final` and `static` as they can't be overridden
- Abstract classes can have constructors - which will get executed during the child object's instantiation, member variables and concrete methods
- We can have instance and static initialization blocks in an abstract class, whereas we can never have them in the interface
- When an instance variable is declared as `transient`, then its value doesn't persist when an object is serialized
- String objects are stored in a special memory area known as string constant pool inside the heap memory
- `StringBuffer` is thread safe i.e multiple threads can't access it simultaneously. Stored in heap
- `StringBuilder` is not thread safe. Stored in stack
- An object that implements `autocloseable` or `closeable` can be passed as a parameter to try statement
- `Error` class represents the errors which are mainly caused by the environment in which application is running
- `Exception` class represents the exceptions which are mainly caused by the application itself. `Npe`, `ClasscastException`
- Diamond problem of inheritance: We have two classes B and C inheriting from A. Assume that B and C are overriding an inherited method and they provide their own implementation. Now D inherits from both B and C doing multiple inheritance. D should inherit that overidden method, which overridden method will be used?
- An `InputStreamReader` is a bridge from byte streams to character streams: It reads bytes and decodes them into characters using a specified charset. The charset that it uses may be specified by name or may be given explicitly, or the platform's default charset may be accepted.
- `InputStreamReader(InputStream in)` OR `InputStreamReader(InputStream in, Charset cs)`
- `BufferedReader` class is used to read the text from a character-based input stream. It can be used to read data line by line using `readLine()` method. It makes the performance fast
- `BufferedReader(Reader rd)` OR `BufferedReader(Reader rd, int size)`
- `read()` for reading a single character
- `Files.readAllBytes` : Reads all the bytes from a file. The method ensures that the file is closed when all bytes have been read or an I/O error, or other runtime exception, is thrown. Intended for reading small files.
- `Paths.get(path)` Converts a path string, or a sequence of strings that when joined form a path string, to a Path
- `Long.parseLong("AA0F245C", 16)` to parse a hex to long
- System-level classes such as `Thread`, `OutputStream` and its subclasses, and `Socket` are not serializable. If your serializable class contains such objects, it must mark them as "transient".
- We can use the `Object.hashCode()` method to retrieve the hashcode of an object.
- `Objects.hashCode()` is a null-safe method we can use to get the hashcode of an object. It only takes a single object. If we provide `null` we will get 0 back.
- `Objects.hash()` can take one or more objects and provides a hashcode for them.
- `System.out.println(1 + 2 + 3 + "Out" + 1 + 2 + 3);` prints `6Out123`
- The `invokeAny()` method takes a collection of `Callable` objects, or subinterfaces of `Callable`. Invoking this method does not return a `Future`, but returns the result of one of the `Callable` objects. You have no guarantee about which of the `Callable`'s results you get. Just one of the ones that finish.
- If one of the tasks complete (or throws an exception), the rest of the `Callable`'s are cancelled.
- The `invokeAll()` method invokes all of the Callable objects you pass to it in the collection passed as parameter. The `invokeAll()` returns a list of `Future` objects via which you can obtain the results of the executions of each `Callable`.
- Keep in mind that a task might finish due to an exception, so it may not have "succeeded". There is no way on a `Future` to tell the difference.
- Handling Exceptions for `ThreadPoolExecutor`
  - When you submit a task to the executor, it returns you a `FutureTask` instance.
  - `FutureTask.get()` will re-throw any exception thrown by the task as an `ExecutorException`.
  - So when you iterate through the `List<Future>` and call `get` on each, catch `ExecutorException` and invoke an orderly shutdown
- `ExecutorCompletionService`? Why do need one if we have `invokeAll`?
  - A `CompletionService` that uses a supplied Executor to execute tasks. This class arranges that submitted tasks are, upon completion, placed on a queue accessible using take. The class is lightweight enough to be suitable for transient use when processing groups of tasks.
  - Using a `ExecutorCompletionService.poll/take`, you are receiving the `Future`s as they finish, in completion order (more or less). Using `ExecutorService.invokeAll`, you do not have this power; you either block until are all completed, or you specify a timeout after which the incomplete are cancelled.

### Why must wait() always be in synchronized block?

- Let's illustrate what issues we would run into if `wait()` could be called outside of a synchronized block with a **concrete example**.
- Suppose we were to implement a blocking queue.
- A first attempt (without synchronization) could look something along the lines below

```java
class BlockingQueue {
    Queue<String> buffer = new LinkedList<String>();

    public void give(String data) {
        buffer.add(data);
        notify();                   // Since someone may be waiting in take!
    }

    public String take() throws InterruptedException {
        while (buffer.isEmpty()) {    // don't use "if" due to spurious wakeups.
            wait();
        }
        return buffer.remove();
    }
}
```

- This is what could potentially happen
  1. A consumer thread calls `take()` and sees that `buffer.isEmpty()`
  2. Before the consumer thread goes on to call `wait()`, a producer thread comes along and invokes a full `give()`
  3. The consumer thread will now call `wait()` (and miss the `notify()` that was just called)
  4. If unlucky, the producer thread won't produce more `give()` as result of the fact that the consumer thread never wakes up, and we have a dead-lock
- Once you understand the issue, the solution is obvious: Use `synchronized` to make sure `notify` is never called between `isEmpty` and `wait`.

### Tools

- Testcontainers, ArchUnit, Liquibase/Flyway, MapStruct

### Commands related to Java

- `sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_version/bin/java`
- `sudo update-alternatives --config java`
- to get the list of threads
  - `top -H -p $PID`
  - `ps -e -T $PID`
