+++
title = 'Exceptions in Java'
date = 2024-02-11

+++

- The first kind of exception is the _checked exception_. These are exceptional conditions that a well-written application should anticipate and recover from.
- Checked exceptions are _subject to_ the Catch or Specify Requirement
- All exceptions are checked exceptions, except for those indicated by `Error`, `RuntimeException`, and their subclasses.
- The third kind of exception is the _runtime exception_. These are exceptional conditions that are internal to the application, and that the application usually cannot anticipate or recover from. Usually logic error or improper use of an API.
- A _stack trace_ is a listing of all pending method calls at a particular point in the execution of a program.
- If the JVM exits while the `try` or `catch` code is being executed, then the `finally` block may not execute. If the thread executing the try or catch code is interrupted or killed, `finally` block may not execute.
- We can open multiple resources in try-with-resources statement separated by a semicolon.
- When multiple resources are opened in try-with-resources, it closes them in the reverse order to avoid any dependency issue.
- For try-with-resources, if exception is thrown in try block and in try-with-resources statement, then method returns the exception thrown in try block.
- You can retrieve suppressed exceptions by calling the `Throwable.getSuppressed` method from the exception thrown by the try block
- If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception. **Checked exceptions are especially important because you expect other developers who use your API to know how to handle the exceptions.**
- If an exception can be properly handled then it should be caught, otherwise, it should be thrown.
- Constructors can throw exceptions
- Don't just throw a `RuntimeException`. Find an appropriate subclass or create your own.
- The Java language has a keyword `assert`. There are two forms: `assert condition;` and `assert condition: expression;`

### Can you throw any exception inside a lambda expression’s body?

- When using a standard functional interface already provided by Java, you can only throw unchecked exceptions because standard functional interfaces do not have a `throws` clause in method signatures:

```java
List<Integer> integers = Arrays.asList(3, 9, 7, 0, 10, 20);
integers.forEach(i -> {
    if (i == 0) {
        throw new IllegalArgumentException("Zero not allowed");
    }
    System.out.println(Math.PI / i);
});
```

- However, if you are using a custom functional interface, throwing checked exceptions is possible if your method declares throws.

### What is checked, unchecked exception and errors?

**1. Checked Exception**:

- These are the classes that extend **Throwable** except **RuntimeException** and **Error**.
- They are programmatically recoverable problems which are caused by unexpected conditions outside the control of the code (e.g. database down, file I/O error, wrong input, etc).
- Example: **IOException, SQLException** etc.

**2. Unchecked Exception**:

- The classes that extend **RuntimeException** are known as unchecked exceptions.
- They are also programmatically recoverable problems but unlike checked exception they are caused by faults in code flow or configuration.
- Example: **ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException** etc.

**3. Error**:

**Error** refers to an irrecoverable situation that is not being handled by a **try/catch**.  
Example: **OutOfMemoryError, VirtualMachineError, AssertionError** etc.

### What is difference between ClassNotFoundException and NoClassDefFoundError?

- `ClassNotFoundException` and `NoClassDefFoundError` occur when a particular class is not found at runtime. However, they occur at different scenarios.
- `ClassNotFoundException` is an exception that occurs when you try to load a class at run time using `Class.forName()` or `loadClass()` methods and mentioned classes are not found in the classpath.
- `NoClassDefFoundError` is an error that occurs when a particular class is present at compile time, but was missing at run time.

### What are important methods of Java Exception Class?

| Method                            | Description                                                                                                                                                                                                                         |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| String getMessage()               | This method returns the message String of Throwable and the message can be provided while creating the exception through it’s constructor.                                                                                          |
| String getLocalizedMessage()      | This method is provided so that subclasses can override it to provide locale specific message to the calling program. Throwable class implementation of this method simply use getMessage() method to return the exception message. |
| synchronized Throwable getCause() | This method returns the cause of the exception or null if the cause is unknown.                                                                                                                                                     |
| String toString()                 | This method returns the information about Throwable in String format, the returned String contains the name of Throwable class and localized message.                                                                               |

### Explain Java finally block behaviour when return statement is encountered.

- If there is a return statement in try block as well as in catch block, **finally** block will execute. The only case where it will not execute is when it encounters **System.exit()**.
- If a method returns a value and also has try, catch and finally blocks in it, then following two rules need to follow.
  - If finally block returns a value then try and catch blocks may or may not return a value.
  - If finally block does not return a value then both try and catch blocks must return a value.
- If try-catch-finally blocks are returning a value according to above rules, then you should not keep any statements after finally block. Because they become unreachable and in Java, Unreachable code gives compile time error.

### What is difference between `throw` and `throws` keyword in Java?

- **throws** keyword is used with method signature to declare the exceptions that the method might throw whereas throw keyword is used to disrupt the flow of program and handing over the exception object to runtime to handle it.
- **throw** keyword in Java is used to explicitly throw an exception from a method or any block of code. We can throw either checked or unchecked exception. The throw keyword is mainly used to throw custom exceptions.
- Important points to remember about throws keyword:
  - throws keyword is required only for checked exception and usage of throws keyword for unchecked exception is meaningless.
  - throws keyword is required only to convince compiler and usage of throws keyword does not prevent abnormal termination of program.
  - By the help of throws keyword we can provide information to the caller of the method about the exception.

### Explain Exception handling in Method overriding.

- **Rule**: An overriding method (the method of child class) can throw any unchecked exceptions, regardless of whether the overridden method (method of base class) throws exceptions or not. However the overriding method should not throw checked exceptions that are new or broader than the ones declared by the overridden method. The overriding method can throw those checked exceptions, which have less scope than the exception(s) declared in the overridden method.
- **Code will work fine**
  - If base class doesn’t throw any exception but child class throws an unchecked exception.
  - When base class and child class both throws a checked exception and child class method is throwing subclass or no exception compared to the same method of base class
- Code will give compilation error
  - If base class doesn’t throw any exception but child class throws an checked exception
  - When child class method is throwing border checked exception compared to the same method of base class
