### [A guide to accessing databases in Java](https://www.marcobehler.com/guides/a-guide-to-accessing-databases-in-java)

- JDBC drivers do a fair amount of work, from the basics of opening up connections to the database to more advanced features like offering abilities to receive events from the database (Oracle)
- When we look at what the major bottlenecks for a database are, they can be summarized as three basic categories: _CPU_, _Disk_, _Network_. We could add _Memory_ in there, but compared to _Disk_ and _Network_ there are several orders of magnitude difference in bandwidth.
- **connections = ((core_count \* 2) + effective_spindle_count)**
- Especially in web applications you do not want to open up a fresh database connection for every user, rather you want to have a small pool of connections that are always open and that are shared between users. That’s what connection pools are for.
- A `SessionFactory` basically represents the mapping between your Java classes and your database tables
- A session is basically a database connection (or more specifically a wrapper of the JDBC connection), with additional goodies on top
- There are two approaches for interacting with the databases:
  - Java First: where we first write classes and map them with our database and perform transactions with them. Hibernate
  - Database First: design and write the database schema BEFORE writing the corresponding Java classes. JooQ.
- jOOQ generates Java code from your database and lets you build type safe SQL queries through its fluent API.

### [A guide to logging in java](https://www.marcobehler.com/guides/a-guide-to-logging-in-java)

- It’s not only that `Log4j` has sane log level names, like 'error' and 'debug'. It also comes with a ton of different and clever appenders, like _SMTPAppender_ (to e-mail log events), _SyslogAppenders_ (to send events to a remote syslog daemon), _JdbcAppenders_ (to send them to the database)
- **FATAL** : Anything at this level means your Java process cannot continue and will now terminate.
- **ERROR** : A request was aborted and the underlying reason requires human intervention ASAP.
- **WARN** : A request was not serviced satisfactorily, intervention is required soon, but not necessarily immediately.
- **INFO** : Info is the log level developers probably feel most 'comfortable' using and in practice you’ll find that developers print out a ton of statements with the INFO level, from client activities (webapps), progress information (batch jobs) to quite intricate, internal process flow details.
- **DEBUG** : Advanced level detail of internal process flows.
- **TRACE** : More details than debug or reserved for use in specific environments

### Java Gotchas

- Constructor in Java can't be `abstract`, `static`, `final` or `synchronized`
- We may request JVM for garbage collection by calling `system.gc()` method. But this doesn't guarantee that JVM will perform the garbage collection
- Static methods can't be overridden because static method is bound to class whereas method overriding is associated with object.
- Abstract methods can never be `final` and `static` as they can't be overridden
- Abstract classes can have constructors - which will get executed during the child object's instantiation, member variables and concrete methods
- We can have instance and static initialization blocks in an abstract class, whereas we can never have them in the interface
- A `final` method can be inherited/used in the subclass, but it cannot be overridden
- A class declared `final` cannot be inherited.
- When an instance variable is declared as `transient`, then its value doesn't persist when an object is serialized
- When a class extends another class it inherits all **non private** members including fields and methods
- **Access modifier of overridden methods cannot be more restrictive than original method**
- String objects are stored in a special memory area known as string constant pool inside the heap memory
- `StringBuffer` is thread safe i.e multiple threads can't access it simultaneously. Stored in heap
- `StringBuilder` is not thread safe. Stored in stack
- An object that implements `autocloseable` or `closeable` can be passed as a parameter to try statement
- **If superclass method throws an exception, then subclass overridden method can throw the same exception or no exception, but must not throw parent exception of the exception thrown by Superclass method**
- Error class represents the errors which are mainly caused by the environment in which application is running
- Exception class represents the exceptions which are mainly caused by the application itself. `Npe`, `ClasscastException`
- Diamond problem of inheritance: We have two classes B and C inheriting from A. Assume that B and C are overriding an inherited method and they provide their own implementation. Now D inherits from both B and C doing multiple inheritance. D should inherit that overidden method, which overridden method will be used?
- An `InputStreamReader` is a bridge from byte streams to character streams: It reads bytes and decodes them into characters using a specified charset. The charset that it uses may be specified by name or may be given explicitly, or the platform's default charset may be accepted.
- `InputStreamReader(InputStream in)` || `InputStreamReader(InputStream in, Charset cs)`
- Java `BufferedReader` class is used to read the text from a character-based input stream. It can be used to read data line by line by `readLine()` method. It makes the performance fast
- `BufferedReader(Reader rd)` || `BufferedReader(Reader rd, int size)`
- `read()` for reading a single character
- `Files.readAllBytes` : Reads all the bytes from a file. The method ensures that the file is closed when all bytes have been read or an I/O error, or other runtime exception, is thrown. Intended for reading small files.
- `Paths.get(path)` Converts a path string, or a sequence of strings that when joined form a path string, to a Path
- In Java, an abstract class can implement an interface, and not provide implementations of all of the interface's methods. It is the responsibility of the first concrete class that has that abstract class as an ancestor to implement all of the methods in the interface.
- A blocking, or synchronous, API is one whose methods block the calling thread until they complete. Of course, the concept of blocking (or nonblocking) only makes sense when these methods can potentially take a long time to complete (say tens of milliseconds to tens of seconds).
- Another type of API, which is often called nonblocking but here we’ll call them semi-blocking (or semi-asynchronous), has methods that don’t block the calling thread for the duration of the operation. They only initiate an operation and return a future object. The future is then used to block and wait for the operation to complete at some other convenient time.
- Finally, a third type of API, a true non-blocking, or asynchronous, API, also does not block the calling thread. Its methods take an additional parameter – a callback function which is code that will be executed (on some unknown thread) when the operation completes
- Long.parseLong("AA0F245C", 16) to parse a hex to long
- system-level classes such as `Thread`, `OutputStream` and its subclasses, and `Socket` are not serializable. If your serializable class contains such objects, it must mark then as "transient".
- Consider using the interface when our problem makes the statement "**A is capable of $doing_this**". For instance, Clonable is capable of cloning an object. Drawable is capable of drawing a shape.
- Static method, can be called by null reference whereas non-static methods can't.
- Consider using abstract classes and inheritance when our problem makes the evidence "**A is a B**". For example, "Dog is an Animal", "Lamborghini is a Car"
- We can use the Object.hashCode() method to retrieve the hashcode of an object.
- `Objects.hashCode()` is a null-safe method we can use to get the hashcode of an object. It only takes a single object. If we provide `null` we will get 0 back.
- `Objects.hash()` can take one or more objects and provides a hashcode for them.
- `System.out.println(1 + 2 + 3 + "Out" + 1 + 2 + 3);` prints `6Out123`

### Microbenchmarking

- Microbenchmarking : it's measuring the performance of something "small", like a system call to the kernel of an operating system.
- Sometimes you way want to initialize some variables that your benchmark code needs, but which you do not want to be part of the code your benchmark measures. Such variables are called "state" variables
- A state object can be reused across multiple calls to your benchmark method. JMH provides different "scopes" that the state object can be reused in
- The `@Setup` annotation tell JMH that this method should be called to setup the state object before it is passed to the benchmark method. The `@TearDown` annotation tells JMH that this method should be called to clean up ("tear down") the state object after the benchmark has been executed.
- JVM optimizations to avoid during microbenchmarking :
  - Loop optimizations : avoid loops in your benchmark methods.
  - Dead Code Elimination : If JVM detects that the result of some computation is never used, the JVM may consider this computation _dead code_ and eliminate it. To avoid it either return the result of code or pass the calculated value to the `Blackhole` object.
  - Constant Folding : A calculation which is based on constants will often result in the exact same result, regardless of how many times the calculation is performed. The JVM may detect that, and replace the calculation with the result of the calculation. It can just inline the constant also.

### Commands

- sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_version/bin/java
- sudo update-alternatives --config java
- to get the list of threads

  - `top -H -p $PID`
  - `ps -e -T $PID`

- The `invokeAny()` method takes a collection of Callable objects, or subinterfaces of Callable. Invoking this method does not return a `Future`, but returns the result of one of the Callable objects. You have no guarantee about which of the Callable's results you get. Just one of the ones that finish.
- If one of the tasks complete (or throws an exception), the rest of the Callable's are cancelled.
- The `invokeAll()` method invokes all of the Callable objects you pass to it in the collection passed as parameter. The `invokeAll()` returns a list of `Future` objects via which you can obtain the results of the executions of each Callable.
- Keep in mind that a task might finish due to an exception, so it may not have "succeeded". There is no way on a `Future` to tell the difference.
- Handling Exceptions for `ThreadPoolExecutor`
  - When you submit a task to the executor, it returns you a `FutureTask` instance.
  - `FutureTask.get()` will re-throw any exception thrown by the task as an `ExecutorException`.
  - So when you iterate through the `List<Future>` and call `get` on each, catch `ExecutorException` and invoke an orderly shutdown
- `ExecutorCompletionService`? Why do need one if we have `invokeAll`?
  - A `CompletionService` that uses a supplied Executor to execute tasks. This class arranges that submitted tasks are, upon completion, placed on a queue accessible using take. The class is lightweight enough to be suitable for transient use when processing groups of tasks.
  - Using a `ExecutorCompletionService.poll/take`, you are receiving the `Future`s as they finish, in completion order (more or less). Using `ExecutorService.invokeAll`, you do not have this power; you either block until are all completed, or you specify a timeout after which the incomplete are cancelled.

### Why must wait() always be in synchronized block?

- Let's illustrate what issues we would run into if wait() could be called outside of a synchronized block with a **concrete example**.
- Suppose we were to implement a blocking queue (I know, there is already one in the API :)
- A first attempt (without synchronization) could look something along the lines below

```java
class BlockingQueue {
    Queue<String> buffer = new LinkedList<String>();

    public void give(String data) {
        buffer.add(data);
        notify();                   // Since someone may be waiting in take!
    }

    public String take() throws InterruptedException {
        while (buffer.isEmpty())    // don't use "if" due to spurious wakeups.
            wait();
        return buffer.remove();
    }
}
```

- This is what could potentially happen
  1. A consumer thread calls `take()` and sees that `buffer.isEmpty()`
  2. Before the consumer thread goes on to call `wait()`, a producer thread comes along and invokes a full `give()`, that is, `buffer.add(data); notify();`
  3. The consumer thread will now call `wait()` (and miss the `notify()` that was just called)
  4. If unlucky, the producer thread won't produce more `give()` as result of the fact that the consumer thread never wakes up, and we have a dead-lock
- Once you understand the issue, the solution is obvious: Use `synchronized` to make sure `notify` is never called between `isEmpty` and `wait`.

### Reading a file:

```java
Path path = Paths.get(filename);
Scanner scanner = new Scanner(path);
while (scanner.hasNextLine()) {
  line = scanner.nextLine();
}
```

### Sorting a Map in java by values

```java
List<Map.Entry<String, Integer>> list = new ArrayList<>(distance.entrySet());
list.sort(Map.Entry.comparingByValue());

Map<String, Integer> sorted = new LinkedHashMap<>();
for (Map.Entry<String, Integer> entry : list) {
  sorted.put(entry.getKey(), entry.getValue());
}
```

### Tools

- Gradle/Maven, Spring Boot, Spring Data/JPA/Hibernate, JUnit, Mockito, AssertJ, Testcontainers, ArchUnit, Liquibase/Flyway, Lombok, MapStruct, Jackson, GraphQL, Docker
