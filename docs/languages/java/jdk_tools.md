### All Platforms

- [jaotc](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jaotc.html) - the Java static compiler that produces native code for compiled Java methods
- [jar](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jar.html) - create an archive for classes and resources, and manipulate or restore individual classes or resources from an archive
- [jarsigner](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jarsigner.html) - sign and verify Java Archive (JAR) files
- [java](https://docs.oracle.com/en/java/javase/14/docs/specs/man/java.html) - launch a Java application
- [javac](https://docs.oracle.com/en/java/javase/14/docs/specs/man/javac.html) - read Java class and interface definitions and compile them into bytecode and class files
- [javadoc](https://docs.oracle.com/en/java/javase/14/docs/specs/man/javadoc.html) - generate HTML pages of API documentation from Java source files
- [javap](https://docs.oracle.com/en/java/javase/14/docs/specs/man/javap.html) - disassemble one or more class files
- [jcmd](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jcmd.html) - send diagnostic command requests to a running Java Virtual Machine (JVM)
- [jconsole](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jconsole.html) - start a graphical console to monitor and manage Java applications
- [jdb](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jdb.html) - find and fix bugs in Java platform programs
- [jdeprscan](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jdeprscan.html) - static analysis tool that scans a jar file (or some other aggregation of class files) for uses of deprecated API elements
- [jdeps](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jdeps.html) - launch the Java class dependency analyzer
- [jfr](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jfr.html) - parse and print Flight Recorder files
- [jhsdb](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jhsdb.html) - attach to a Java process or launch a postmortem debugger to analyze the content of a core dump from a crashed Java Virtual Machine (JVM)
- [jinfo](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jinfo.html) - generate Java configuration information for a specified Java process
- [jjs](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jjs.html) - command-line tool to invoke the Nashorn engine
- [jlink](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jlink.html) - assemble and optimize a set of modules and their dependencies into a custom runtime image
- [jmap](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jmap.html) - print details of a specified process
- [jmod](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jmod.html) - create JMOD files and list the content of existing JMOD files
- [jpackage](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jpackage.html) - package a self-contained Java application
- [jps](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jps.html) - list the instrumented JVMs on the target system
- [jrunscript](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jrunscript.html) - run a command-line script shell that supports interactive and batch modes
- [jshell](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jshell.html) - interactively evaluate declarations, statements, and expressions of the Java programming language in a read-eval-print loop (REPL)
- [jstack](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jstack.html) - print Java stack traces of Java threads for a specified Java process
- [jstat](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jstat.html) - monitor JVM statistics
- [jstatd](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jstatd.html) - monitor the creation and termination of instrumented Java HotSpot VMs
- [keytool](https://docs.oracle.com/en/java/javase/14/docs/specs/man/keytool.html) - manage a keystore (database) of cryptographic keys, X.509 certificate chains, and trusted certificates
- [rmic](https://docs.oracle.com/en/java/javase/14/docs/specs/man/rmic.html) - generate stub and skeleton class files using the Java Remote Method Protocol (JRMP)
- [rmid](https://docs.oracle.com/en/java/javase/14/docs/specs/man/rmid.html) - start the activation system daemon that enables objects to be registered and activated in a Java Virtual Machine (JVM)
- [rmiregistry](https://docs.oracle.com/en/java/javase/14/docs/specs/man/rmiregistry.html) - create and start a remote object registry on the specified port on the current host
- [serialver](https://docs.oracle.com/en/java/javase/14/docs/specs/man/serialver.html) - return the `serialVersionUID` for one or more classes in a form suitable for copying into an evolving class

### Windows Only

- [jabswitch](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jabswitch.html) - enable or disable Java Access Bridge
- [jaccessinspector](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jaccessinspector.html) - examine accessible information about the objects in the Java Virtual Machine using the Java Accessibility Utilities API
- [jaccesswalker](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jaccesswalker.html) - navigate through the component trees in a particular Java Virtual Machine and present the hierarchy in a tree view
- [javaw](https://docs.oracle.com/en/java/javase/14/docs/specs/man/javaw.html) - launch a Java application without a console window
- [kinit](https://docs.oracle.com/en/java/javase/14/docs/specs/man/kinit.html) - obtain and cache Kerberos ticket-granting tickets
- [klist](https://docs.oracle.com/en/java/javase/14/docs/specs/man/klist.html) - display the entries in the local credentials cache and key table
- [ktab](https://docs.oracle.com/en/java/javase/14/docs/specs/man/ktab.html) - manage the principal names and service keys stored in a local key table

### jcmd

- swiss army knife of JVM diagnostic tools. It sends a command to a running JVM to accomplish most of what other tools allow for and more. Use `jcmd <pid> help` to view what commands are available for a given process.
- `jcmd` : displays list of all currently running JVM processes
- `jcmd <PID> help` : to see what diagnostic commands are available for a given JVM
- `jcmd <PID> <COMMAND_NAME>` : to either run a diagnostic command or get an error message asking for command arguments
- `jcmd <PID> help <COMMAND_NAME>` : to get more information about a command
- `jcmd <PID> Thread.print` : prints out the current stack trace for all threads running in the JVM.
- `jcmd jmodern.Main PerfCounter.print` : print out a long list of various JVM performance counters.
- `jcmd  <PID> GC.class_histogram`

### jstat

- jstat is like the `top` for the JVM, only it displays information about GC and JIT activity.

### Other Monitoring tools

- [jolokia](https://jolokia.org/)
- [hawtio](https://hawt.io/)
- [metrics](https://metrics.dropwizard.io/4.1.2/)
- [visualvm](https://visualvm.github.io/)
- [java mission control](https://www.oracle.com/java/technologies/jdk-mission-control.html)

### A box of tools to spy on Java

- **jps** - launched without any flags it will simply **list all Java processes and their pids**. With flag `-V` (lists arguments passed to the JVM), `-m` (lists arguments passed to the main method), or `-l` (shows package names)
- **jstack** - **prints all Java thread stack traces with useful information**, such as what state the thread is in and what code it's currently executing. Very useful for identifying deadlocks, livelocks and similar.
- Running `jstack` with the VMID of the desired process will generate a stack dump.
- `jstack`'s `-l` parameter offers up a slightly longer dump that includes more detailed information about the locks held by each of the Java threads.
- There is much more useful information, such as _​daemon_prio​_ (priority inside JVM), ​*os_prio* (priority in the OS), *​tid*​ (id of the thread), *​nid*​ (id of the thread in the OS), address on heap. Especially ​*nid*​ might come in useful to use some more advanced OS tools to learn more about the thread
- **jmap** - this tool gives a **look into the heap**. Since JDK 11 `jcmd` for dumping the heap. Some useful flags include
  - `clstats` - display stats for each class
  - `histo` - produces a text histogram of the objects currently strongly referenced in the heap, sorted by the total numebr of bytes consumed by that particular type.
  - `finalizerinfo` - to view classes which are gathered by Garbage Collector but their `finalize()` methods have not been called yet.
  - `jmap -histo $pid` : get a histogram of which kinds of objects are taking up all the space
- **jstat** - samples a running JVM for selected metrics. Useful to quickly identify easy-to-spot issues but won't help with more ephemeral ones.
- `jstat -options` to view all the options available
- Majority of jstat's options are used to gather performance information about the garbage collector, or just parts of it.
- The `-class` option reveals loaded and unloaded classes (making it a great utility for detecting `ClassLoader` leaks), and both `-compiler` and `-printcompilation` display information about the Hotspot JIT compiler.
- If you want snapshots taken at regular intervals, specify the intervals in milliseconds following the -options command. `jstat` will continuously display snapshots of the monitored process's information. To take a specific number of snapshots before terminating, specify that number after the interval/period value.
- If 5756 were the VMID for a running SwingSet2 process, then the following command would tell `jstat` to produce a gc snapshot dump every 250 ms for 10 iterations : `jstat -gc 5756 250 10`
  - `gcutil` - for stats on GC
  - `printcompilation` - displays the last successful JIT compilation
- **jhsdb**​ (Java HotSpot debugger) is your go-to tool for ​post-mortem analysis​. If you provide it with a core dump file and a path to the JVM that was used to launch the process ​**it will let you launch most of the aforementioned tools on a dead process​**.

  - Usage: `​jhsdb jstack|jmap|jinfo --core <path-to-core-dump> --exe <path-to-JVM>​`.
  - It can also be used to debug a living process. Remember you need to enable core dumps first (with ​ulimit​).

- There are 2 kinds of locks: original ones based on `synchronized` keyword and `Object.wait/notifyAll` methods and the `java.util.concurrent` locks introduced in Java 5. The major difference between them is that former are bound to the stack frame where you enter the `synchronized` section and were always available in the thread dumps. The latter (`java.util.concurrent`), on the other hand, are not stack frame bound – you can enter the lock in one method and leave it in the different one.

### JMC

- The first powerful feature of JMC are event triggers. Triggers allow you to run various actions in response to certain JMX counters exceeding and (optionally) staying above the threshold for a given period of time.
- Running JFR is a 3 step process:
  1. You need to create a JFR template file containing desired settings. In order to do this, run jmc and go to Window -> Flight Recording Template Manager menu.
  2. `jcmd <PID> VM.unlock_commercial_features`
  3. `jcmd <PID> JFR.start name=test duration=60s settings=template.jfc filename=output.jfr`
