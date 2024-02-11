+++
title = 'Core Java'
date = 2024-02-11

+++

### Basics

- **static initialization block** is executed before the main() method.
- A class can have any number of static initialization blocks, and they can appear anywhere in the class body
- There is an alternative to static blocks — you can write a private static method: advantage is that they can be reused later if you need to reinitialize the class variable
- There are two alternatives to using a constructor to initialize instance variables: initializer blocks and final methods (This is especially useful if subclasses might want to reuse the initialization method).
- Initializer blocks for instance variables look just like static initializer blocks, but without the static keyword
- There are 4 access modifiers available in java:
  - `private` : can only be accessible from the class it is declared
  - `default` or no modifier or package private : can be accessible from any class of the same package
  - `protected`: can be accessed from any class of the package or subclass on the class
  - `public` : can be accessed from anywhere

### Strings

- A _code point_ is a code value that is associated with a character in an encoding scheme.
- In the Unicode standard, code points are written in hexadecimal and prefixed with U+, such as U+0041 for the code point of the Latin letter A
- Unicode has code points that are grouped into 17 _code planes_.
- The first code plane, called the _basic multilingual plane_, consists of the “classic” Unicode characters with code points U+0000 to U+FFFF.
- Sixteen additional planes, with code points U+10000 to U+10FFFF, hold the _supplementary characters_.
- The characters in the basic multilingual plane are represented as 16-bit values, called _code units_
- The supplementary characters are encoded as consecutive pairs of code units.
- In Java, the `char` type describes a code unit in the UTF-16 encoding.
- `static char[] readPassword(String prompt, Object... args)`, `static String readLine(String prompt, Object... args)` : displays the prompt and reads the user input until the end of the input line. The args parameters can be used to supply formatting arguments.
- You can use the `s` conversion to format arbitrary objects. If an arbitrary object implements the `Formattable` interface, the object’s `formatTo` method is invoked. Otherwise, the `toString` method is invoked to turn the object into a string
- `Scanner in = new Scanner(Path.of('my-file.txt'), StandardCharsets.UTF_8 )` to read from a file
- To write to a file, construct a `PrintWriter` object. In the constructor, supply the file name and the character encoding: `PrintWriter out = new PrintWriter('my-file.txt', StandardCharsets.UTF_8);`
- `String dir = System.getProperty('user.dir');` to get the current working directory.

### Class Design Hints

- Always keep data private
- Always initialize data.
- Don't use too many basic types in a class.
- Not all fields need individual field accessors and mutators
- Break up classes that have too many responsibilities.
- Make the names of classes and methods reflect their responsibilities.
- Prefer immutable classes.

### Method Calls

- Let's say we call `x.f(args)` and `x` is declared to be an object of class `C`. Here is what happens
  1. The compiler enumerates all methods called `f` in the class `C` and all accessible methods called `f` in the superclasses of `C`.
  2. If among all the methods called `f` there is a unique method whose parameter types are a best match for the supplied arguments, that method is chosen to be called. This process is called _overloading resolution_.
  3. If the method is `private`, `static`, `final` or constructor, then the compiler knows exactly which method to call. This is called _static binding_ Otherwise, the method to be called depends on the actual type of the implicit parameter, and dynamic binding must be used at runtime.
  4. The VM must call the version of the method that is appropriate for the _actual_ type of the object to which `x` refers. Let's say the actual type is `D`, a subclass of `C`. If the class `D` defines method `f(String)`, that method is called. If not, `D`'s superclass is searched for a method f(String), and so on.
- The VM precomputes for each class a _method table_ that lists all method signatures and the actual methods to be called. When a method is called the VM simply makes a table lookup.
- Only one good reason to make a method or class `final`: to make sure its semantics cannot be changed in a subclass.
- One can cast only within an inheritance hierarchy
- Use `instanceof` to check before casting from a superclass to a subclass.
- TIP: In general, it is best to minimize the use of casts and the `instanceof` operator.
- For restricting a method to subclasses only or, less commonly, to allow subclass methods to access a superclass field - declare a class feature as `protected`

### equals method

- TIP: `Objects.equals(a,b)` : returns true if both arguments are null, false if only one is null, and calls a.equals(b) OW.
- Recipe for writing equals method:
  1. Name the explicit parameter `otherObject`
  2. Test whether `this` happens to be identical to `otherObject`
  3. Test whether `otherObject` is null and return false if it is.
  4. Compare the classes of `this` and `otherObject`. If the semantics of `equals` can change in subclasses, use the `getClass` test. `if (getClass() != otherObject.getClass()) return false;` If the same semantics holds for _all_ subclasses, use an `instanceof` test
  5. Cast `otherObject` to a variable of your class type
  6. Now compare the fields as required. If you redefine `equals` in a subclass, include a call to `super.equals(other)`

### Enums

- The constructor of an enumeration is always private.
- `java.lang.Enum<E>`
  - `static Enum valueOf(Class enumClass, String name)` returns the enumerated constant of the given class with given name.
  - `int ordinal()` return the zero-base position of this enumerated constant in the enum declaration
  - `int compareTo(E other)`

### Reflection

- _Reflection_ is the ability to find out more about classes and their properties in a running program.
- Using reflection, Java can support user interface builders, ORMs, and many other development tools that dynamically inquire about the capabilities of classes.
- A program that can analyze the capabilities of classes is called _reflective_.
- When the program is running the Java runtime system always maintains what it calls _runtime type identification_ on all objects.
- `java.lang.Class`
  - `static Class forName(String className)` : returns the Class object representing the class with name `className`
  - `Constructor getConstructor(Class... parameterTypes)` : yields an object describing the constructor with the given parameter types.
  - finds the resource in the same place as the class and then returns a URL or input stream that can be used for loading the resource.
    - `URL getResource(String name)`
    - `InputStream getResourceAsStream(String name)`
  - `Field[] getFields()` returns an array containing `Field` objects for the public fields of this class or its superclasses; `Field[] getDeclaredFields()` returns an array of `Field` objects for all fields of this class.
  - `Method[] getMethods()` returns public methods and includes inherited method; `Method[] getDeclaredMethods()` return all methods of this class or interface but doesn't include inherited methods.
  - `Constructor[] getConstructors` and `Constructor[] getDeclaredConstructors`
  - `String getPackageName()`
- `java.lang.reflect.Constructor`
  - `Object newInstance(Object... params)` constructs a new instance of the constructor's declaring class, passing `params` to the constructor.
- `java.lang.reflect.Field` , `java.lang.reflect.Method`
  - `Class getDeclaringClass`
  - `Class[] getExceptionTypes()`
  - `int getModifiers()`
  - `String getName()`
  - `Class[] getParameterTypes()`
  - `Class getReturnType()`
- `java.lang.reflect.Modifier`
  - `static String toString(int modifiers)`
  - `static boolean isAbstract...`

### Abstract classes and Interfaces

- An _interface_ is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods
- All constant values defined in an interface are implicitly public, static, and final
- When you extend an interface that contains a default method, you can do the following:
  - Not mention the default method at all, which lets your extended interface inherit the default method.
  - Redeclare the default method which makes it `abstract`
  - Redefine the default method, which overrides it.
- _Multiple inheritance of state_ : the ability to inherit fields from multiple classes
- _Multiple inheritance of implementation_ is the ability to inherit method definitions from multiple classes.
- _Multiple inheritance of type_ : is the ability of a class to implement more than one interface.
- The distinction between hiding a static method and overriding an instance method has important implications:
  - The version of the overridden instance method that gets invoked is the one in the subclass.
  - The version of the hidden static method that gets invoked depends on whether it is invoked from the superclass or the subclass
- An overriding method can also return a subtype of the type returned by the overridden method. This subtype is called a covariant return type.

```java
public Employee getBuddy() { ... }
public Manager getBuddy() { ... } // OK to change return type
```

- The two `getBuddy` methods have _covariant_ return types.
- When the supertypes of a class or interface provide multiple default methods with the same signature, the Java compiler follows inheritance rules to resolve the name conflict. These rules are driven by the following two principles:
  - Instance methods are preferred over interface default methods.
  - Methods that are already overridden by other candidates are ignored. This circumstance can arise when supertypes share a common ancestor.
- If two or more independently defined default methods conflict, or a default method conflicts with an abstract method, then the Java compiler produces a compiler error. You must explicitly override the supertype methods.
- Inherited instance methods from classes can override abstract interface methods.
- Static methods in interfaces are never inherited.
- You will get a compile-time error if you attempt to change an instance method in the superclass to a static method in the subclass, and vice versa.
- In a subclass, you can overload the methods inherited from the superclass. Such overloaded methods neither hide nor override the superclass instance methods
- The JVM calls the appropriate method for the object that is referred to in each variable. It does not call the method that is defined by the variable's type. This behavior is referred to as _virtual method invocation_
- Methods and constructors can throw exceptions. So far, there are two kinds of exceptions which can be thrown. There are the ones which have to be handled, and the ones which don't have to be dealt with. When we have to handle the exceptions, we do it either in a try-catch chunk, or throwing them from a method.
- It is also possible to avoid handling the exceptions in a method, and delegate the responsibility to the method caller. We delegate the responsibility of a method by using the statement throws Exception.
- Interfaces can also define the exceptions thrown
- If we want to define superclass variables or methods whose accessibility should be restricted to only its subclasses, we can use the _protected_ field accessability.
- The `super` call must always be in the first line!
- The method which can be called is defined through the variable type. For instance, if a `Student` object reference is saved into a `Person` variable, the object can use only the methods defined in the `Person` class
- The execution method is always chosen based on the object real type, regardless of the variable type which is used
- we can think that if an object owns or is composed of the other objects, inheritance is wrong.
- Inheritance does not exclude using interfaces, and vice-versa. Interfaces are like an agreement on the class implementation, and they allow for the abstraction of the concrete implementation.
- Abstract classes combine interfaces and inheritance. They do not produce instances, but you can create instances of their subclasses.
- The difference between interfaces and abstract classes is that abstract classes provide the program with more structure.
- Because it is possible to define the functionality of abstract classes, we can use them to define the default implementation
- In Java, arrays are covariant. This means that if a type S is a subtype of another type T, then an array of S is considered to be a subtype of array of T.
- The value returned by `hashCode()` is the object's hash code, which is the object's memory address in hexadecimal
- By definition, if two objects are equal, their hash code _must also_ be equal. That's why you should override hashCode() method when you override equals() as the hashCode() implementation by Object class will not be valid anymore.
- Methods called from constructors should generally be declared `final`. If a constructor calls a non-final method, a subclass may redefine that method with surprising or undesirable results.
- with abstract classes, you can declare fields that are not static and final, and define public, protected, and private concrete methods. With interfaces, all fields are automatically public, static, and final, and all methods that you declare or define (as default methods) are public.
- Which should you use, abstract classes or interfaces?
  - Consider using abstract classes if any of these statements apply to your situation:
    - You want to share code among several closely related classes.
    - You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).
    - You want to declare non-static or non-final fields. This enables you to define methods that can access and modify the state of the object to which they belong.
  - Consider using interfaces if any of these statements apply to your situation:
    - You expect that unrelated classes would implement your interface. For example, the interfaces Comparable and Cloneable are implemented by many unrelated classes.
    - You want to specify the behavior of a particular data type, but not concerned about who implements its behavior
    - You want to take advantage of multiple inheritance of type.

* You can supply a default implementation for any interface method. You must tag such a method with the default modifier.
* A default method can call other methods.
* An important use for default methods is _interface evolution_
* Rules for resolving default method conflicts:
  1. Superclasses win. If a superclass provides a concrete method, default methods with the same name and parameter are simply ignored.
  2. Interfaces clash. If an interface provides a default method, and another interface contains a method with the same name and parameter types (default or not), then you must resolve the conflict by overriding that method.
* The `Cloneable` interface is one of a handful of _tagging interfaces_ that Java provides. Some programmers call them _marker interfaces_. A tagging interface has no methods; its only purpose is to allow the use of `instanceOf` in a type inquiry. Recommended not to use in our programs

### Lambdas

- You can supply a lambda expression whenever an object of an interface with a single abstract method is expected. Such an interface is called _functional interface_
- A functional interface is any interface that contains only one abstract method
- A functional interface may contain one or more default methods or static methods.
- default methods can be overridden whereas static methods cannot.
- A Supplier has no arguments and yields a value of type T when it is called. Suppliers are used for _lazy evaluation_
- `Person[] people = Stream.toArray(Person[] :: new)`
- A lambda expression has three ingredients
  1. A block of code.
  2. Parameters
  3. Values for the _free_ variables - that is, the variables that are not parameters and not defined inside the code.
- In a lambda-expression, one can only reference variables whose value doesn't change.
- It is also illegal to refer, in a lambda expression, to a variable that is mutated outside.
- The rule is that any captured variable in a lambda expression must be _effectively final_
- ![common_functional_interfaces](/images/common_functional_interfaces.png)
- ![fi_for_primitive_types](/images/fi_for_primitive_types.png)
- The static `comparing` method takes a "key extractor" function that maps a type T to a comparable type (such as `String`). `Arrays.sort(people, Comparator.comparing(Person :: getName));`
- We can chain comparators with the `thenComparing` method for breaking ties.

### Nested classes

- An inner class method gets to access both its own data fields _and_ those of the outer object creating it.
- An object of inner class always gets an implicit reference to the object that created it.
- Only inner classes can be private. Regular classes always have either package or public access.
- Any static fields declared in an inner class must be final and initialized with a compile-time constant.
- An inner class cannot have `static` methods.
- You refer to an inner class as _outerclass.innerClass_ when it occurrs outside the scope of the outer class.
- If an inner class accesses a private data field, then it is possible to access that data field through other classes added to the package or the outer class.
- We can define a class _locally in a single method_ if the class name is used only once. Local classes are never declared with an access modifier.
- Use a static inner class whenever the inner class does not need to access an outer class object.
- A simple rule of thumb is that if you don't use any instance members of the outer class, make the nested class static
- Unlike regular inner classes, static inner classes can have static fields and methods.
- Inner classes that are declared inside an interface are automatically `static` and `public`
- Proxies can be used to create at runtime, new classes that implement a given set of interfaces. Proxies are only necessary when you don't yet know at compile time which interfaces you need to implement.
- Nested classes are divided into two categories: **static** and **non-static**.
- Nested classes that are declared static are called _static nested classes_.
- Non-static nested classes are called _inner classes_
- inner classes have access to other members of the enclosing class, even if they are declared private.
- Static nested classes do not have access to other members of the enclosing class
- There are two special kinds of inner classes:
  - _local classes_ (an inner class within the body of a method )
  - _anonymous classes_ (an inner class within the body of a method without naming the class).
- Local classes are similar to inner classes because they cannot define or declare any static members
- Local classes in static methods can only refer to static members of the enclosing class.
- Local classes are non-static because they have access to instance members of the enclosing block.
- You cannot declare an interface inside a block; interfaces are inherently static.
- You cannot declare static initializers or member interfaces in a local class.
- A local class can have static members provided that they are constant variables.
- A local class can only access local variables that are declared final or are _effectively_ final.
- Like local classes, anonymous classes can capture variables; they have the same access to local variables of the enclosing scope.
- **Local classes** Use it if you need to create more than one instance of a class, access its constructor, or introduce a new, named type (because, for example, you need to invoke additional methods later)
- **Anonymous class** : Use it if you need to declare fields or additional methods.
- **Lambda expression**
  - Use it if you are encapsulating a single unit of behaviour that you want to pass to other code.
  - Use it if you need a simple instance of a functional interface and none of the preceding criteria apply.
- **Nested class**
  - Use it if requirements are similar to local class, you want to make the type more widely available, and you don't require access to local variables or method parameters.
  - Use a non-static nested class (or inner class) if you require access to an enclosing instance's non-public fields and methods. Use a static nested class if you don't require this access.

### Inheritance

- Interface inheritance (what): Simply tells what the subclasses should be able to do.
  - e.g. all lists should be able to print themselves, how they do it is up to them.
- Implementation inheritance (how): Tells the subclasses how they should behave.
  - Lists should print themselves exactly this way: by getting each element in order and then printing them.
- By using the extends keyword, subclasses inherit all members of the parent class. "Members" include:
  - All instance and static variables,
  - All methods
  - All nested classes

* Note that constructors are not inherited, and private members cannot be directly accessed by subclasses.
* if the implementation details of a module are kept internally hidden and the only way to interact with it is through a documented interface, then that module is said to be encapsulated.
* Implementation inheritance breaks encapsulation
* Method calls have compile-time type equal to their declared type.
* Compile-time type is also called _static_ type.
* Invocation of overridden methods follows two simple rules:
  - Compiler plays it safe and only allows us to do things according to the static type.
  - For overridden methods (not overloaded methods), the actual method invoked is based on the dynamic type of the invoking expression
  - Can use casting to overrule compiler type checking
* interfaces in Java provide us with the ability to make callbacks. Sometimes, a function needs the help of another function that might not have been written yet (e.g. max needs compareTo). A callback function is the helping function (in the scenario, compareTo). In some languages, this is accomplished using explicit function passing; in Java, we wrap the needed function in an interface.
* A Comparable says, "I want to compare myself to another object". It is embedded within the object itself, and it defines the natural ordering of a type.
* A Comparator, on the other hand, is more like a third party machine that compares two objects to each other. Since there's only room for one compareTo method, if we want multiple ways to compare, we must turn to Comparator
* Type upper bounds in generic methods (e.g. `K extends Comparable<K>`)
* `am.new KeyIterator();` This construction allows us to instantiate a non-static nested class, meaning a class that needs to be associated with a particular instance of the enclosing class
* **Iterable**, the interface that makes a class able to be iterated on, and requires the method `iterator()`, which returns an Iterator object. And we've seen **Iterator**, the interface that defines the object with methods to actually do that iteration. You can think of an Iterator as a machine that you put onto an iterable that facilitates the iteration. Any iterable is the object on which the iterator is performing

### Annotations

- _Annotations_ are a form of metadata, provide data about a program that is not part of the program itself. Annotations have no direct effect on the operation of the code they annotate.
- Annotations have a number of uses, among them:
  - **Information for the compiler**
  - **Compile-time and deployment-time processing**
  - **Runtime processing**
- Annotations can be applied to declarations: declarations of classes, fields, methods, and other program elements.
- As of the Java SE 8 release, annotations can also be applied to the use of types. This form of annotation is called a _type annotation_. A few examples

```java
new @Interned MyObject(); // Class instance creation expression

myString = (@NonNull String) str;   // Type cast

class UnmodifiableList<T> implements @ReadOnly List<@ReadOnly T> { ... }  // implements clause

void monitorTemperature() throws @Critical TemperatureException { ... }   // Thrown exception declaration
```

- `@Retention` this describes up to which step in the compilation process the annotation will be available. RetentionPolicy.{SOURCE, CLASS, RUNTIME}
- `@Target` annotation marks another annotation to restrict what kind of Java elements the annotation can be applied to. This defines where the annotation can be set
- Type Annotations were created to support improved analysis of Java programs by way of ensuring stronger type checking. For example: to ensure that a particular variable in a program is never assigned to null; to avoid triggering a NullPointerException. One can write a custom plug-in to check for this. You would then modify your code to annotate that particular variable, indicating that it is never assigned to null. The variable declaration might look like this: `@NonNull String str;`
- There are some situations where you want to apply the same annotation to a declaration or type use. As of the Java SE 8 release, repeating annotations enable you to do this
- For instance you want a cleanup method to be called in month end and every friday at 11 p.m. So same `@Schedule` annotation can be used on the method with differnt values
- This repeating annotations are stored in _container annotation_

### Generics

- generics enable _types_ (classes and interfaces) to be parameters when defining classes, interfaces and methods. type parameters provide a way to re-use the same code with different inputs
- using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe
- A _generic type_ is a generic class or interface that is parameterized over types.
- A type variable can be any non-primitive type you specify: any class type, any interface type, any array type, or even another type variable.
- The most commonly used type parameter names are:
  - E - Element, K - Key, N - Number, T - Type, V - Value, S,U,V etc. - 2nd, 3rd, 4th types
- perform a _generic type invocation_, which replaces T with some concrete value
- A _raw type_ is the name of a generic class or interface without any type arguments
- Static and non-static generic methods are allowed, as well as generic class constructors.
- The syntax for a generic method includes a list of type parameters, inside angle brackets, which appears before the method's return type.
- For static generic methods, the type parameter section must appear before the method's return type.
- to restrict the types that can be used as type arguments in a parameterized type _bounded type parameters_ are used.
- To declare a bounded type parameter, list the type parameter's name, followed by the extends keyword, followed by its upper bound
- In addition to limiting the types you can use to instantiate a generic type, bounded type parameters allow you to invoke methods defined in the bounds:
- A type parameter can have _multiple bounds_: `<T extends B1 & B2 & B3>`
- A type variable with multiple bounds is a subtype of all the types listed in the bound. If one of the bounds is a class, it must be specified first.
- Given two concrete types A and B (for example, Number and Integer), `MyClass<A>` has no relationship to `MyClass<B>`, regardless of whether or not A and B are related. The common parent of `MyClass<A>` and `MyClass<B>` is Object.
- In generic code, `?`called the wildcard, represents an unknown type.
- upper bounded wildcard restricts the unknown type to be a specific type or a subtype of that type and is represented using the _extends_ keyword.
- The unbounded wildcard type is specified using the wildcard character (?), for example, List<?>. This is called _a list of unknown type_. There are two scenarios where an unbounded wildcard is a useful approach:
  - When writing a method that can be implemented using functionality provided in the Object class.
  - When the code is using methods in the generic class that don't depend on the type parameter
- It's important to note that List<Object> and List<?> are not the same. You can insert an Object, or any subtype of Object, into a List<Object>. But you can only insert null into a List<?>
- a _lower bounded_ wildcard restricts the unknown type to be a specific type or a _super type_ of that type
- A lower bounded wildcard is expressed using the wildcard character ('?'), following by the _super_ keyword, followed by its lower bound: `<? super A>`.
- ![hierarchy](/images/hierarchy.png)
- a list may be defined as `List<?>` but, when evaluating an expression, the compiler infers a particular type from the code. This scenario is known as _wildcard capture_
- `copy(src, dest)` : src is the "in" variable, dest is the "out" variable.
- use the "in" and "out" principle when deciding whether to use a wildcard and what type of wildcard is appropriate. The following list provides the guidelines to follow:
  - An "in" variable is defined with an upper bounded wildcard, using the `extends` keyword.
  - An "out" variable is defined with a lower bounded wildcard, using the super keyword.
  - In the case where the "in" variable can be accessed using methods defined in the Object class, use an unbounded wildcard.
  - In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.
- These guidelines do not apply to a method's return type. Using a wildcard as a return type should be avoided because it forces programmers using the code to deal with wildcards.
- A _reifiable_ type is a type whose type information is fully available at runtime. This includes primitives, non-generic types, raw types, and invocations of unbound wildcards.
- _Non-reifiable types_ are types where information has been removed at compile-time by type erasure
- _Heap pollution_ occurs when a variable of a parameterized type refers to an object that is not of that parameterized type.
- Restrictions on Generics:
  - Cannot instantiate generic types with primitive types.
  - Cannot create instances of type parameters : `E elem = new E()` compile time error. As a workaround, can create an object of a type parameter through reflection. `E elem = cls.newInstance()` where `Class<E> cls`
  - Cannot declare static fields whose types are type parameters
  - Cannot use casts or `instanceOf` with parameterized types.
  - Cannot create arrays of parameterized types.
  - Cannot create, catch, or throw objects of parameterized types.
  - Cannot overload a method where the formal parameter types of each overload erase to the same raw type.

### I/O

- An _I/O Stream_ represents an input source or an output destination.
- Programs use byte streams to perform input and output of 8-bit bytes. All byte stream classes are descended from `InputStream` and `OutputStream`.
- Byte streams should only be used for the most primitive I/O.
- There are many byte stream classes - `FileInputStream` and `FileOutputStream`
- All character stream classes are descended from `Reader` and `Writer`. As with byte streams, there are character stream classes that specialize in file I/O: `FileReader` and `FileWriter`
- _Unbuffered I/O_ means each read or write request is handled directly by the underlying OS which often triggers disk access, network activity.
- _Buffered I/O_ solves the above problem. Buffered input streams read data from a memory area known as a _buffer_ and calls native API only when buffer is empty. Buffered o/p streams write data to a buffer, and the native o/p API is called only when the buffer is full.
- `BufferedInputStream` and `BufferedOutputStream` create buffered byte streams, while `BufferedReader` and `BufferedWriter` create buffered character streams.
- When you need to create a formatted output stream, instantiate PrintWriter, not PrintStream.
- All data streams implement either the DataInput interface or the DataOutput interface. Data streams support binary I/O of primitive data type values as well as String values
- object streams support I/O of objects.
- The object stream classes are ObjectInputStream and ObjectOutputStream. These classes implement ObjectInput and ObjectOutput,
- A stream can only contain one copy of an object, though it can contain any number of references to it.
- `toAbsolutePath` method converts a path to an absolute path.
- `toRealPath` method returns the _real_ path of an existing file.
- to construct a path from one location in the file system to another location use the `relativize` method
- A relative path cannot be constructed if only one of the paths includes a root element
- The `Files` methods work on instances of `Path` objects
- An _atomic file operation_ is an operation that cannot be interrupted or "partially" performed. Either the entire operation is performed or the operation fails.
- `Files` class is "link" aware. Every `Files` method either detects what to do with a sym link or provides an option to configure.
- You can copy a file or directory by using the copy(Path, Path, CopyOption...) method. The copy fails if the target file exists, unless the REPLACE_EXISTING option is specified.
- You can move a file or directory by using the move(Path, Path, CopyOption...) method. The move fails if the target file exists, unless the REPLACE_EXISTING option is specified.
- You can use the `FileStore` class to learn information about a file store, such as how much space is available.

## 6.031

### Reading 1 : Static Typing

- **Static checking** can catch:
  - syntax errors, like extra punctuation or spurious words
  - wrong names
  - wrong number of arguments
  - wrong argument types
  - wrong return types
- **Dynamic checking** can catch:
  - illegal argument values.
  - illegal conversions : e.g `Integer.valueOf("hello")` is a dynamic error.
  - unrepresentable return values : e.g. `Integer.valueOf("8000000000")`
  - out of range indices
  - calling a method on a null object reference
- It’s good practice to use `final` for declaring the parameters of a method and as many local variables as possible.
- goal should be to produce software that is
  - **Safe from bugs** : Correctness and defensiveness are required in any software we build.
  - **Easy to understand** : code has to communicate to future programmers
  - **Ready for change** : designs should make it easy to make changes

### Reading 2 : Basic Java

- The keys of Java `Map` must be **hashable**
- The objects of Java `Set` must be **hashable**
- When the set of values is small and finite, it makes sense to define all the values as named constants, which Java calls an **enumeration**.
- [java.io.File](http://docs.oracle.com/javase/8/docs/api/?java/io/File.html) represents a file on disk. We can test whether the file is readable, delete the file, see when it was last modified…
- [java.io.FileReader](http://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/io/FileReader.html) lets us read text files.
- [java.io.BufferedReader](http://docs.oracle.com/javase/8/docs/api/?java/io/BufferedReader.html) lets us read in text efficiently, and it also provides a very useful feature: reading an entire line at a time.

### Reading 3 : Testing

- A **module** is a part of a software that can be designed, implemented, tested, and reasoned about separately from the rest of the system.
- A **specification** describes the behavior of a module. The **specification** describes the input and output behavior of the function. For a function it gives the types of the parameters and any additional constraints on them. It also gives the type of the return value and how the return value relates to the inputs.
- A module has an _implementation_ that provides its behavior, and _clients_ that use the module.
- A _test case_ is a particular choice of inputs, along with expected o/p behavior required by the spec.
- A _test suite_ is a set of test cases for a module.
- Systematic testing is the goal of designing a test suite with three desirable properties:
  - **Correct** : A correct test is a legal client of the specification.
  - **Thorough** : A thorough test suite finds actual bugs in the implementation.
  - **Small** : A small test suite, with few test cases, is faster to write in the first place.
- We divide the input space into **subdomains**, each consisting of a set of inputs.
- The idea behind subdomains is to divide the input space into sets of similar inputs on which the program has similar behaviour. Then we use one representative of each set.

* A test case must obey the requirements of the function’s specification.
* All the assertions in Java follow this order : expected first, actual second

- **Blackbox testing** means choosing test cases only from the specification, not the implementation of the function.
- **Glass box testing** means choosing test cases with knowledge of how the function is actually implemented.
- A test that tests an individual module (where a module is a method or a class), in isolation if possible, is called a **unit test**.
- The opposite of a unit test is an **integration test** , which tests a combination of modules, or even the entire program.
- **Automated testing** means running the tests and checking their results automatically.
- It’s very important to rerun your tests when you modify your code. This prevents your program from regressing — introducing other bugs when you fix new bugs or add new features. Running all your tests after every change is called **regression testing**.
- So **automated regression testing** is a best-practice of modern software engineering.
- Always set your programming editor to insert space characters when you press the Tab key.
- In general, change global variables into parameters and return values, or put them inside objects that you’re calling methods on

### Reading 4: Code Review

- SFB - Safe from Bugs
- ETU - Easy to Understand
- RFC - Ready for change
- General principles of good code
  - DRY
  - Comments where needed
  - Fail fast
  - Avoid magic numbers
  - One purpose for each variable
  - Use good names
  - No global variables
  - Return results, don't print them
  - Use whitespace for readability

### Reading 5: Version Control

- All of the operations we do with Git — clone, add, commit, push, log, merge, … — are operations on a graph data structure that stores all of the versions of files in our project, and all the log entries describing those changes.
- The **Git object graph** is stored in the .git directory of your local repository.
- The history of a Git project is **DAG**
- Each node in the history graph is a `commit`.
- Except for the initial commit, each commit has a pointer to a **parent** commit
- Some commits have the same parent: they are versions that diverged from a common previous version. And some commits have two parents: they are versions that tie divergent histories back together.
- `git log --name-status` `A` means a file was added, `M` means modified. `D` is deleted.
- `git log -p hello.txt` will show the history of hello.txt including diffs,
- Each commit is a snapshot of our entire project, which Git represents with a **tree** node
- the Git object graph stores each version of an individual file once, and allows multiple commits to _share_ that one copy
- `git show <commit hash>:` show what was in the repo at a particular commit (i.e files)

### Reading 6 : Specifications

- A specification of a method consists of several clauses:
  - a _precondition_ indicated by the keyword _requires_
  - a _postcondition_ indicated by the keyword _effects_
- primitives cannot be `null`
- **Avoid null**
- Instead of returning a special variable such as -1 or null when some condition is not satisfied we can throw exceptions.
- _checked exceptions_ are called that because they are checked by the compiler:
  - If a method might throw a checked exception, the possibility must be declared in its signature.
  - If a method calls another method that may throw a checked exception, it must either handle it, or declare the exception itself.
- _Unchecked exceptions_ are used to signal bugs. These exceptions are not expected to be handled by the code except perhaps at the top level.
- `Throwable` is the class of objects that can be thrown or caught.
- `RuntimeException`, `Error`, and their subclasses are **unchecked** exceptions.
- All other throwables — `Throwable`, `Exception`, and all of their subclasses except for those of the `RuntimeException` and `Error` lineage — are **checked** exceptions
- When you define your own exceptions, you should either subclass `RuntimeException` (to make it an unchecked exception) or `Exception` (to make it checked)
- You should use an **unchecked** exception only to signal an unexpected failure (i.e. a bug), or if you expect that clients will usually write code that ensures that exception will not happen, because there is a convenient and inexpensive way to avoid the exception; Otherwise you should use a **checked** exception.
- Exceptions that are used to signal unexpected failures – bugs in either the client or the implementation – are not part of the postcondition of a method, so they should not appear in either `@throws` or `throws`.
- Exceptions that signal a special result are always documented with a Javadoc `@throws` clause, specifying the conditions under which that special result occurs

### Reading 7: Designing Specifications

- **deterministic** does the spec define only a single possible output for a given input, or allow the implementor to choose from a set of legal outputs?
- **declarative** does the spec just characterize _what_ the output should be, or does it explicitly say _how_ to compute the output?
- **strong** does the spec have a small set of legal implementations, or a large set?
- Roughly speaking, there are two kinds of specifications.
  - _Operational_ specifications give a series of steps that the method performs.
  - _Declarative_ specifications don’t give details of intermediate steps. Instead, they just give properties of the final outcome, and how it’s related to the initial state. (Almost always preferable)
- A specification S2 is stronger than or equal to a specification S1 if
  - S2's precondition is weaker than or equal to S1's
  - S2's postcondition is stronger than or equal to S1's, for the states that satisfy S1's precondition
- If the above is the case, then an implementation that satisfies S2 can be used to satisfy S1 as well, and it’s safe to replace S1 with S2 in your program.

#### Designing good specifications

- The specification should be coherent. A method should do only one thing
- The results of a call should be informative
- the specification should be strong enough
- The specification should also be weak enough
- The specification should use _abstract types_ when possible

### Reading 8: Mutability & Immutability

- You should never use `Date` Use one of the classes from package `java.time` : `LocalDateTime` or `Instant` since they are immutable
- `BigInteger` and `BigDecimal` are immutable
- An object is immutable if its state cannot change after construction. The object's fields are initialized only once inside the constructor and never change.
- Steps for creating an immutable class:
  - Make it final.
  - Make all fields private.
  - Don't expose setter methods.
  - Make all mutable fields final so that it's value can be assigned only once.
  - Initialize all the fields via a constructor performing deep copy.
  - Perform cloning of objects in the getter methods to return a copy rather than acutal object reference.
- Immutable classes are normally used for caching purposes and they are by definition thread-safe.
- We can also use Builder Pattern to easily create immutable classes.

### Reading 9: Avoiding Debugging

- A Java assert statement may also include a description expression. he description follows the asserted expression, separated by a colon. For example: `assert x >= 0 : "x is " + x;`
- What to Assert?
  - Method argument requirements
  - Method return value requirements
- If an assertion is obvious from its local context, leave it out.
- Never use assertions to test conditions that are external to your program, such as the existence of files, the availability of network, or the correctness of input typed by a human user.
- Asserted expressions should not have _side-effects_

### Reading 11: Abstraction Functions and Rep Invariants

- Most important property of a good ADT is that it **preserves its own invariants**.
- defensive copying: making a copy of a mutable object to avoid leaking out references to the rep
- If any of the types are mutable, make sure your implementation doesn’t return direct references to its representation
- so using unmodifiable lists, maps, and sets can be a very good way to reduce the risk of bugs.

### Explain JVM, JRE and JDK?

- **JVM (Java Virtual Machine)** : It is an abstract machine. It is a specification that provides run-time environment in which java bytecode can be executed. It follows three notations:
  - **Specification**: It is a document that describes the implementation of the Java virtual machine. It is provided by Sun and other companies.
  - **Implementation**: It is a program that meets the requirements of JVM specification.
  - **Runtime Instance**: An instance of JVM is created whenever you write a java command on the command prompt and run the class.
- **JRE (Java Runtime Environment)** : JRE refers to runtime environment in which java bytecode can be executed. It implements the JVM (Java Virtual Machine) and provides all the class libraries and other support files that JVM uses at runtime. So JRE is a software package that contains what is required to run a Java program. Basically, it’s an implementation of the JVM which physically exists.
- **JDK(Java Development Kit)** : It is the tool necessary to compile, document and package Java programs. The JDK completely includes JRE which contains tools for Java programmers. The Java Development Kit is provided free of charge. Along with JRE, it includes an interpreter/loader, a compiler (javac), an archiver (jar), a documentation generator (javadoc) and other tools needed in Java development. In short, it contains JRE + development tools.

### Can we assign List of String into a List of Object variable? e.g. `List<Object> var = new ArrayList<String>;`

- No, because list of `Object` can hold any type of `Object`. But actual list object is restricted to hold only `String` type. So it will give compile time error.

### What is difference between the Inner Class and Sub Class?

- Nested Inner class can access any private instance variable of outer class. Like any other instance variable, we can have access modifier private, protected, public and default modifier.

### When to throw an exception?

- If the function's assumptions about its inputs are violated, it should throw an exception instead of returning normally.
- The other side of this equation is: if you find your functions throwing exceptions frequently, then you probably need to refine their assumptions.

### Difference between >> and >>>?

- `>>` is arithmetic shift right, `>>>` is logical shift right / unsigned shift.
- In an arithmetic shift, the sign bit is extended to preserve the signedness of the number.

### When to call System.exit

- System.exit() can be used to run [shutdown hooks](http://docs.oracle.com/javase/1.5.0/docs/guide/lang/hook-design.html) before the program quits. This is a convenient way to handle shutdown in bigger programs, where all parts of the program can't (and shouldn't) be aware of each other. Then, if someone wants to quit, he can simply call System.exit(), and the shutdown hooks (if properly set up) take care of doing all necessary shutdown ceremonies such as closing files, releasing resources etc.
- in the standard [Runtime](http://download.oracle.com/javase/6/docs/api/java/lang/Runtime.html)

### Explain the SOLID principles?

- **Single responsibility** principle : a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
- **Open/Closed** principle : software entities should be open for extension, but closed for modification
- **Liskov substitution** principle : objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program. Child can be substituted in place for parent.
- **Interface segregation** principle : many client-specific interfaces are better than one general-purpose interface
- **Dependency inversion** principle : one should "Depend upon Abstractions. Do not depend upon concretions". Dependency should be inverted to user instead of code.

### What is the purpose of using `BufferedInputStream` and `BufferedOutputStream` classes?

- `BufferedInputStream` and `BufferedOutputStream` class is used for buffering an input and output stream while reading and writing, respectively. It internally uses buffer to store data. It adds more efficiency than to write data directly into a stream. So, it makes the performance fast.

### What are the restrictions that are applied to the Java `static` methods?

- If a method is declared as `static`, it is a member of a class rather than belonging to the object of the class. It can be called without creating an object of the class. A static method also has the power to access static data members of the class.

* There are a few restrictions imposed on a static method
  - The static method cannot use non-static data member or invoke non-static method directly.
  - The `this` and `super` cannot be used in static context.
  - The static method can access only static type data (static type instance variable).
  - There is no need to create an object of the class to invoke the static method.
  - A static method cannot be overridden in a subclass

```java
class Parent {
   static void display() {
      System.out.println("Super class");
   }
}
public class Example extends Parent {
   void display()  // trying to override display() {
      System.out.println("Sub class");
   }
   public static void main(String[] args) {
      Parent obj = new Example();
      obj.display();
   }
}
```

This generates a compile time error. The output is as follows −

```
Example.java:10: error: display() in Example cannot override display() in Parent
void display()  // trying to override display()
     ^
overridden method is static
1 error
```

### Name some classes present in java.util.regex package?

- **Java Regex**: The Java Regex or Regular Expression is an API to define a pattern for searching or manipulating strings.
- **java.util.regex package**
  - MatchResult interface
  - Matcher class
  - Pattern class
  - PatternSyntaxException class

```java
import java.util.regex.*;
public class RegexExample {
   public static void main(String args[]) {
      //1st way
      Pattern p = Pattern.compile(".s"); // represents single character
      Matcher m = p.matcher("as");
      boolean b = m.matches();

      //2nd way
      boolean b2 = Pattern.compile(".s").matcher("as").matches();

      //3rd way
      boolean b3 = Pattern.matches(".s", "as");

      System.out.println(b + " " + b2 + " " + b3);
   }
}
```

### In Java, How many ways can you take input from the console?

- In Java, there are three different ways for reading input from the user in the command line environment(console).
  1. **Using Buffered Reader Class**: This method is used by wrapping the `System.in` (standard input stream) in an `InputStreamReader` which is wrapped in a `BufferedReader`, we can read input from the user in the command line.
  2. **Using Scanner Class**: The main purpose of the `Scanner` class is to parse primitive types and strings using regular expressions, however it also can be used to read input from the user in the command line.
  3. **Using Console Class**: It has become a preferred way for reading user’s input from the command line. In addition, it can be used for reading password-like input without echoing the characters entered by the user; the format string syntax can also be used (like System.out.printf()).

```java
public class Sample
{
   public static void main(String[] args) {
      // Using Console to input data from user
      String name = System.console().readLine();
      System.out.println(name);
   }
}
```

### What is the difference between Serializable and Externalizable interface?

| SERIALIZABLE                                                                                    | EXTERNALIZABLE                                                                                                         |
| ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| It is a marker interface i.e. does not contain any method.                                      | it contains two methods writeExternal() and readExternal() which implementing classes MUST override.                   |
| It passes the responsibility of serialization to JVM and it’s default algorithm.                | Externalizable provides control of serialization logic to programmer – to write custom logic.                          |
| Mostly, default serialization is easy to implement, but has higher performance cost.            | Serialization done using Externalizable, add more responsibility to programmer but often result in better performance. |
| It’s hard to analyze and modify class structure because any change may break the serialization. | It’s more easy to analyze and modify class structure because of complete control over serialization logic.             |
| Default serialization does not call any class constructor.                                      | A public no-arg constructor is required while using Externalizable interface.                                          |

### What is the purpose of using javap?

- `javap` command displays information about the fields, constructors and methods present in a class file. The `javap` command (also known as the Java Disassembler) disassembles one or more class files. `javap Simple.class`

### What is the difference between creating String as new() and literal?

- When you create String object using `new()` operator, it always creates a new object in heap memory. On the other hand, if you create object using String literal syntax e.g. "Java", it may return an existing object from String pool, if it already exists. Otherwise it will create a new string object and put in string pool for future re-use.

```java
String a = "abc";
String b = "abc";
System.out.println(a == b);  // true

String c = new String("abc");
String d = new String("abc");
System.out.println(c == d);  // false
```

### What is a Memory Leak? How can a memory leak appear in garbage collected language?

- The standard definition of a memory leak is a scenario that occurs when **objects are no longer being used by the application, but the Garbage Collector is unable to remove them from working memory** – because they’re still being referenced. - As a result, the application consumes more and more resources – which eventually leads to a fatal OutOfMemoryError.

```java
// Java Program to illustrate memory leaks
import java.util.Vector;
public class MemoryLeaksDemo
{
   public static void main(String[] args) {
      Vector v = new Vector(214444);
      Vector v1 = new Vector(214744444);
      Vector v2 = new Vector(214444);
      System.out.println("Memory Leaks Example");
   }
}
```

Output

```
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space exceed
```

### What are different types of Memory Leaks in Java?

- Memory Leak through static Fields
- Unclosed Resources/connections
- Adding Objects With no `hashCode()` and `equals()` Into a HashSet
- Inner Classes that Reference Outer Classes
- Through `finalize()` Methods
- Calling `String.intern()` on Long String

### How are strings represented in memory?

- A `String` instance in Java is an object with two fields: a `char[]` _value_ field and an `int` _hash_ field. The _value_ field is an array of chars representing the string itself, and the _hash_ field contains the _hashCode_ of a string which is initialized with zero, calculated during the first `hashCode()` call and cached ever since. As a curious edge case, if a `hashcode` of a string has a zero value, it has to be recalculated each time the `hashCode()` is called.
- Important thing is that a `String` instance is immutable: you can't get or modify the underlying `char[]` array. Another feature of strings is that the static constant strings are loaded and cached in a string pool. If you have multiple identical `String` objects in your source code, they are all represented by a single instance at runtime.

### What is string constant pool?

- Since String is one of the most used type in any application, Java designer took a step further to optimize uses of this class. They know that Strings will not going to be cheap, and that's why they come up with an idea to cache all String instances created inside double quotes e.g. "Java". These double quoted literal is known as String literal and the cache which stored these String instances are known as as String pool. In earlier version of Java, I think up-to Java 1.6 String pool is located in permgen area of heap, but in Java 1.7 updates its moved to main heap area. Earlier since it was in PermGen space, it was always a risk to create too many String object, because its a very limited space, default size 64 MB and used to store class metadata e.g. .class files. Creating too many String literals can cause java.lang.OutOfMemory: permgen space. Now because String pool is moved to a much larger memory space, it's much more safe.
- Whenever you create a string object using string literal, JVM first checks the content of the object to be created. If there exist an object in the string constant pool with the same content, then it returns the reference of that object. It doesn’t create a new object. If the content is different from the existing objects then only it creates new object.
  - String is a final class in java.lang package which is used to represent the set of characters in java.
  - String is a derived type and not primitive type.
  - There are two ways to create string objects in java. One is using new operator and another one is using string literals.
  ```java
  String s1 = new String("abc");  //Creating string object using new operator
  String s2 = "abc";             //Creating string object using string literal
  ```
- One special thing about string objects is that you can create string objects without using new operator i.e using string literals. This is not possible with other derived types (except wrapper classes). One more special thing about strings is that you can concatenate two string objects using ‘+’. This is the relaxation java gives to string objects as they will be used most of the time while coding. And also java provides string constant pool to store the string objects.

### What is a `StringBuilder` and what are its use cases? What is the difference between appending a string to a `StringBuilder` and concatenating two strings with a + operator? How does `StringBuilder` differ from `StringBuffer`?

- `StringBuilder` allows manipulating character sequences by appending, deleting and inserting characters and strings. This is a mutable data structure, as opposed to the String class which is immutable.
- When concatenating two String instances, a new object is created, and strings are copied. This could bring a huge garbage collector overhead if we need to create or modify a string in a loop. `StringBuilder` allows handling string manipulations much more efficiently.
- `StringBuffer` is different from `StringBuilder` in that it is thread-safe. If you need to manipulate a string in a single thread, use `StringBuilder` instead.

### What is a compile time constant in Java? What is the risk of using it?

- If a primitive type or a string is defined as a constant and the value is known at compile time, the compiler replaces the constant name everywhere in the code with its value. This is called a compile-time constant.
- **Compile time constant must be:**
  - declared final
  - primitive or String
  - initialized within declaration
  - initialized with constant expression
- They are replaced with actual values at compile time because compiler know their value up-front and also knows that it cannot be changed during run-time. `private final int x = 10;`

### Do you know Generics? How did you use it in your coding?

- Generics allow types (Integer, String, … etc and user defined types) to be a parameter to methods, classes and interfaces. For example, classes like HashSet, ArrayList, HashMap, etc use generics very well.
- **Advantages**
  - **Code reuse** : We can write a method/class/interface once and use for any type we want.
  - **Type-safety**: We can hold only a single type of objects in generics. It doesn't allow to store other objects. Generics make errors to appear compile time than at run time.
  - **Type Casting**: There is no need to typecast the object.
  - **Compile-Time Checking**: It is checked at compile time so problem will not occur at runtime.

### What are Generics and why are they used? How can you restrict types on a generic class/method?

- Generics refers to a technique of writing the code for a class, without specifying the data type that the class works with.
- Benefits:
  - This allows a generic class to be specialized for many different data types without having to rewrite the class.
  - As the type T is defined when using generics, instead of using an object type, the type does not need to be boxed/unboxed.
- Sometimes we don’t want whole class to be parameterized, in that case we can create Java generics method. Since constructor is a special kind of method, we can use generics type in constructors too.
- Following are the rules to define Generic Methods −
  - All generic method declarations have a type parameter section delimited by angle brackets (< and >) that precedes the method's return type .
  - Each type parameter section contains one or more type parameters separated by commas. A type parameter, also known as a type variable, is an identifier that specifies a generic type name.
  - The type parameters can be used to declare the return type and act as placeholders for the types of the arguments passed to the generic method, which are known as actual type arguments.
  - A generic method's body is declared like that of any other method. Note that type parameters can represent only reference types, not primitive types (like int, double and char).
- **Java Generics supports multiple bounds also**, i.e `<T extends A & B & C>.` In this case A can be an interface or class. If A is class then B and C should be interfaces. **We can’t have more than one class in multiple bounds**.
- Question mark (?) is the wildcard in generics and represent an unknown type. The wildcard can be used as the type of a parameter, field, or local variable and sometimes as a return type. **We can’t use wildcards while invoking a generic method or instantiating a generic class**.

### What are Java Generics Upper Bounded Wildcard

- Upper bounded wildcards are used to relax the restriction on the type of variable in a method. Suppose we want to write a method that will return the sum of numbers in the list, so our implementation will be something like this.

```java
public static double sum(List<Number> list) {
   double sum = 0;
   for (Number n : list) {
      sum += n.doubleValue();
   }
   return sum;
}
```

- Now the problem with above implementation is that it won’t work with List of Integers or Doubles because we know that `List<Integer>` and `List<Double>` are not related, this is when upper bounded wildcard is helpful. We use generics wildcard with **extends** keyword and the **upper bound** class or interface that will allow us to pass argument of upper bound or it’s subclasses types.
- The above implementation can be modified like below program. `public static double sum(List<? extends Number> list)`
- In above method we can use all the methods of upper bound class Number.

### What are Generics unbounded Wildcard

- Sometimes we have a situation where we want our generic method to be working with all types, in this case unbounded wildcard can be used. Its same as using `<? extends Object>`. e.g. `public static void printData(List<?> list)`

### What do you mean by Generics Type Erasure

- Generics in Java was added to provide type-checking at compile time and it has no use at run time, so java compiler uses **type erasure** feature to remove all the generics type checking code in byte code and insert type-casting if necessary. Type erasure ensures that no new classes are created for parameterized types; consequently, generics incur no runtime overhead.

### What are Generics Lower Bounded WildCard

- Suppose we want to add Integers to a list of integers in a method, we can keep the argument type as `List<Integer>` but it will be tied up with `Integer` where as `List<Number>` and `List<Object>` can also hold integers, so we can use lower bound wildcard to achieve this. We use generics wildcard (?) with **super** keyword and lower bound class to achieve this.
- We can pass lower bound or any super type of lower bound as an argument in this case, Java compiler allows to add lower bound object types to the list.

```java
public static void addIntegers(List<? super Integer> list) {
   list.add(new Integer(50));
}
```

### Runtime order of static init, init, constructor, inner class init

- Class/static initializer are executed in the order they are defined when the class is loaded. (Actually when it is resolved)
- Instance initializer are executed in the order defined when the class is instantiated, immediately before the constructor code is executed, immediately after the invocation of the super constructor.

### Serialization/Deserialization

- Process of serialization
  - write serialization stream magic data STREAM_MAGIC, STREAM_VERSION etc.
  - walk up inheritance tree and write class metadata till you reached java.lang.Object class then walk down the inheritance tree and write data for class instance property.
- Why serialversionUID : for class versioning. If not provided while doing deserialization `InvalidClassException` will come.
- deserialization : a hidden constructor which never uses class constructor to build object state.
- custom serialization :
  - `public void readObject(ObjectInputStream ois)`
  - `public void writeObject(ObjectOutputStream oos)`
- Static fields are not serialized because we are serializing an object and static fields belong to class not object
- When a class implements the _Serializable_ interface, all its sub-classes are serializable as well.
- Parent class without serializable interface : while doing serialization, properties of parent class would not be serialized.
- composite class not serializable : then while doing serialization of container class, JVM will throw `NotSerializableException`
- Externalizable extends Serializable interface. methods `public void readExternal(ObhectInput inp)` and `public void writeExternal(ObjectOutput o)`
- Serialization with singleton : use `private Object readResolve()` method, return same static instance.
- If don't want to provide serialization : override readObject and writeObject method, throw `SerializationNotSupportedException`
- Serializable vs Externalizable : During deserialization, Serializable interface doesn't call default constructor to create object. It uses value assignment using reflection. Whereas Externalizable calls default constructor.
- readResolve and writeReplace : hook method so JVM will call those method. We need to provide implmentation for those methods.
- On the contrary, when an object has a reference to another object, these objects must implement the Serializable interface separately.
- The JVM associates a version (long) number with each serializable class. It is used to verify that the saved and loaded objects have the same attributes and thus are compatible on serialization.

### Why we give Serial Version UID?

- The serialization runtime associates with each serializable class a version number, called a `serialVersionUID`, which is used during deserialization to verify that the sender and receiver of a serialized object have loaded classes for that object that are compatible with respect to serialization. If the receiver has loaded a class for the object that has a different `serialVersionUID` than that of the corresponding sender's class, then deserialization will result in an InvalidClassException. A serializable class can declare its own `serialVersionUID` explicitly by declaring a field named `serialVersionUID` that must be static, final, and of type `long`

### What will be output of below code snippet?

```java
String s1 = "abc";
StringBuffer s2 = new StringBuffer(s1);
System.out.println(s1.equals(s2));
```

- It will print false because s2 is not of type String. If you will look at the equals method implementation in the String class, you will find a check using instanceof operator to check if the type of passed object is String? If not, then return false.

```java
String s1 = "abc";
String s2 = new String("abc");
s2.intern();
System.out.println(s1 ==s2);
```

- It’s a tricky question and output will be false. We know that intern() method will return the String object reference from the string pool, but since we didn’t assigned it back to s2, there is no change in s2 and hence both s1 and s2 are having different reference. If we change the code in line 3 to `s2 = s2.intern();` then output will be true.

### What is String interning using intern() method

- Java by default doesn't put all String object into String pool, instead they gives you flexibility to explicitly store any arbitrary object in String pool. You can put any object to String pool by calling intern() method of java.lang.String class
- Though, when you create String using literal notation of Java, it automatically calls intern() method to put that object into String pool, provided it was not present in the pool already. This is another difference between string literal and new string, because in case of new, interning doesn't happen automatically, until you call intern() method on that object.
- Look at the below example. Object ‘s1’ will be created in heap memory as we are using new operator to create it. When we call intern() method on s1, it creates a new string object in the string constant pool with “JAVA” as it’s content and assigns it’s reference to s2. So, **s1 == s2** will return false because they are two different objects in the memory and s1.equals(s2) will return true because they have same content.

```java
public class StringExamples {
   public static void main(String[] args) {
        String s1 = new String("JAVA");
        String s2 = s1.intern();            // Creating String Intern
        System.out.println(s1 == s2);       // Output : false
        System.out.println(s1.equals(s2));  // Output : true
    }
}
```

- Look at this example. Object s1 will be created in string constant pool as we are using string literal to create it and object s2 will be created in heap memory as we are using new operator to create it. When you call intern() method on s2, it returns reference of object to which s1 is pointing as it’s content is same as s2. It does not create a new object in the pool. So, **s1 == s3** will return true as both are pointing to same object in the pool.

```java
public class StringExamples {
    public static void main(String[] args) {
        String s1 = "JAVA";
        String s2 = new String("JAVA");
        String s3 = s2.intern();       		// Creating String Intern
        System.out.println(s1 == s3);      	// Output : true
    }
}
```

- String Literals Are Automatically Interned : When you call intern() on the string object created using string literals it returns reference of itself. Because, you can’t have two string objects in the pool with same content. That means string literals are automatically interned in java.

```java
public class StringExamples {
    public static void main(String[] args) {
        String s1 = "JAVA";
        String s2 = s1.intern();      // Creating String Intern
        System.out.println(s1 == s2); // Output : true
    }
}
```

### What is the use of interning the string?

- **To Save The memory Space :** Using interned string, you can save the memory space. If you are using lots of string objects with same content in your code, than it is better to create an intern of that string in the pool. Use that intern string whenever you need it instead of creating a new object in the heap. It saves the memory space.
- **For Faster Comparison :** Assume that there are two string objects s1 and s2 in heap memory and you need to perform comparison of these two objects more often in your code. Then using `s1.intern() == s2.intern()` will be fater than `s1.equals(s2)`. Because, equals() method performs character by character comparison where as “==” operator just compares references of objects.
- String Pool is possible only because String is immutable in Java and it’s implementation of String interning concept. String pool is also example of Flyweight design pattern

### How many objects will be created in the following code and where they will be stored in the memory?

```java
String s1= new String("java");
String s2 = new String("java");
String s3 = "java";
String s4 = "java";
System.out.println(s3==s4); //true
System.out.println(s1==s3); //false
System.out.println(s2==s4); //false
System.out.println(s1==s2); //false
```

![interning](/images/interning.png)
We can see total 3 objects are created here.

### Why String class is immutable?

1. **String pool** is possible only because String is immutable in java, this way Java Runtime saves a lot of java heap space because different String variables can refer to same String variable in the pool. If String would not have been immutable, then String interning would not have been possible because if any variable would have changed the value, it would have been reflected to other variables also.
2. If String is not immutable then it would cause severe security threat to the application. For example, database username, password are passed as String to get database connection and in **socket programming** host and port details passed as String. Since String is immutable it’s value can’t be changed otherwise any hacker could change the referenced value to cause security issues in the application.
3. Since String is immutable, it is safe for **multithreading** and a single String instance can be shared across different threads. This avoid the usage of synchronization for thread safety, Strings are implicitly thread safe.
4. Strings are used in Java **Classloader** and immutability provides security that correct class is getting loaded by Classloader. For example, think of an instance where you are trying to load java.sql.Connection class but the referenced value is changed to myhacked.Connection class that can do unwanted things to your database.
5. Since String is immutable, its **hashcode** is cached at the time of creation and it doesn’t need to be calculated again. This makes it a great candidate for key in a Map and it’s processing is fast than other HashMap key objects. This is why String is most used Object as HashMap keys.

### Is it possible to call a constructor from another (within the same class, not from a subclass)? What is constructor chaining?

- Yes it is possible and it is called constructor chaining. Constructor chaining is the process of calling one constructor from another constructor with respect to current object.
- Constructor chaining can be done in two ways:
  - **Within same class**: It can be done using **this()** keyword for constructors in same class
  - **From base class**: by using **super()** keyword to call constructor from the base class.
- **Note**: `this` keyword is very different from `this()`. Notice, **this** refers the current object and **this()** is used to call one constructor from another.
- The real purpose of Constructor Chaining is that you can pass parameters through a bunch of different constructors, but only have the initialization done in a single place. This allows you to maintain your initializations from a single location, while providing multiple constructors to the user. If we don’t chain, and two different constructors require a specific parameter, you will have to initialize that parameter twice, and when the initialization changes, you’ll have to change it in every constructor, instead of just the one.
- As a rule, constructors with fewer arguments should call those with more

![Constructor chaining](/images/chaining_constructor.png)

- **Important points about constructor**
  1. Every class has a constructor whether it’s a normal class or an abstract class.
  2. Constructors are not methods and they don’t have any return type.
  3. Constructor name should match with class name.
  4. Constructor can use any access specifier, they can be declared as private also. Private constructors are possible in java but there scope is within the class only.
  5. Like constructors method can also have name same as class name, but still they have return type, thorugh which we can identify them that they are methods and not constructors.
  6. If you don’t implement any constructor within the class, compiler provides one.
  7. this() and super() should be the first statement in the constructor code. If you don’t mention them, compiler does it for you accordingly.
  8. Constructor overloading is possible but overriding is not possible, which means we can have overloaded constructor in our class but we can’t override a constructor.
  9. Constructors can’t be inherited.
  10. If Super class doesn’t have a no-arg(default) constructor then compiler would not insert a default constructor in child class as it does in normal scenario.
  11. Interfaces do not have constructors.
  12. Abstract class can have constructor and it gets invoked when a class which implements it is instantiated. (i.e. object creation of concrete class).
  13. A constructor can also invoke another constructor of the same class – By using this(). If you want to invoke a parameterized constructor then do it like this: **this(parameter list)**.

### Explain Type Casting in Java – Implicit and Explicit Casting.

- **Type Casting** in Java is nothing but converting a primitive or interface or class in Java into other type. There is a rule in Java Language that classes or interface which shares the same type hierarchy only can be typecasted. If there is no relationship between then Java will throw `ClassCastException`. Type casting are of two types:
  1. Implicit Casting (Widening)
  2. Explicit Casting (Narrowing)
- Implicit Casting in Java / Widening / Automatic type conversion
- Automatic type conversion can happen if both type are compatible and target type is larger than source type.
- byte -> short -> int -> long -> float -> double : widening
- **Implicit Casting of a Primitive**
  - No Explicit casting required for the above mentioned sequence.
    ```java
    byte i = 50;
    // No casting needed for below conversion
    short j = i;
    int k = j;
    long l = k;
    ```
- **Implicit Casting of a Class Type**
  - We all know that when we are assigning **smaller type** to a **larger type**, there is no need for a casting required. Same applies to the class type as well. Lets look into the below code.
    ```java
    Parent p = new Child();
    p.disp();
    ```
  - Here Child class is smaller type we are assigning it to Parent class type which is larger type and hence no casting is required.
- **Explicit Casting in Java / Narrowing**
  - When you are assigning a larger type to a smaller type, then Explicit Casting is required.
    ```java
    double d = 75.0;
    // Explicit casting is needed for below conversion
    float f = (float) d;
    long l = (long) f;
    ```
  - double -> float -> long -> int -> short -> byte : narrowing
  - We all know that when we are assigning larger type to a smaller type, then we need to explicitly type cast it. Lets take the same above code with a slight modification
    ```java
    Parent p = new Child();
    p.disp();
    Child c = p;
    c.disp();
    ```
  - When we run the above code we will be getting exception stating
    ```java
    `Exception in thread "main" java.lang.Error: Unresolved compilation problem:
    `Type mismatch: cannot convert from Parent to Child
    ```
  - In order to fix the code, then we need to cast it to Child class `Child c = (Child) p;`

### Explain `super` keyword and its usage.

- The **super** keyword in java is a reference variable which is used to refer immediate parent class object.
  Whenever you create the instance of subclass, an instance of parent class is created implicitly which is referred by super reference variable.
- Usage of java super Keyword
  1. super can be used to refer immediate parent class instance variable.
  2. super can be used to invoke immediate parent class method.
  3. super() can be used to invoke immediate parent class constructor.
- super("Hahaha"); //calls parent class constructor with String argument
- **Important points**
  1. Call to super() must be first statement in Derived(Student) Class constructor.
  2. If a constructor does not explicitly invoke a superclass constructor, the Java compiler automatically inserts a call to the no-argument constructor of the superclass. If the superclass does not have a no-argument constructor, you will get a compile-time error. super() is added in each class constructor automatically by compiler if there is no super() or this().
  3. If a subclass constructor invokes a constructor of its superclass, either explicitly or implicitly, you might think that a whole chain of constructors called, all the way back to the constructor of Object. This, in fact, is the case. It is called constructor chaining.

### Why we use instance initializer block(IIBs)?

- In a Java program, operations can be performed on methods, constructors and initialization blocks. Instance Initialization Blocks or IIB are used to initialize instance variables. IIBs are executed before constructors. They run each time when object of the class is created.
  - Initialization blocks are executed whenever the class is initialized and before constructors are invoked.
  - They are typically placed above the constructors within braces.
  - It is not at all necessary to include them in your classes.
  - We can also have multiple IIBs in a single class. If compiler finds multiple IIBs, then they all are executed from top to bottom
  - The Instance Initialization Block is invoked after the parent class constructor is invoked (i.e. after super() constructor call)
- **Instance Initialization Block with parent class**
  You can have IIBs in parent class also. Instance initialization block code runs immediately after the call to super() in a constructor. The compiler executes parents class’s IIB before executing current class’s IIBs.
- Order of execution in this case will be as follows:
  1. Instance Initialization Block of super class
  2. Constructors of super class
  3. Instance Initialization Blocks of the class
  4. Constructors of the class

### Explain Downcasting using instanceof operator.

- The java `instanceof` operator is used to test whether the object is an instance of the specified type (class or subclass or interface).
- The `instanceof` in java is also known as type comparison operator because it compares the instance with type. It returns either true or false. If we apply the `instanceof` operator with any variable that has null value, it returns false when Subclass type refers to the object of Parent class, it is known as downcasting. If we perform it directly, compiler gives Compilation error. If you perform it by typecasting, ClassCastException is thrown at runtime. But if we use `instanceof` operator, downcasting is possible.
- `Dog d = new Animal(); // Compilation error`
- If we perform downcasting by typecasting, ClassCastException is thrown at runtime.
- `Dog d = (Dog) new Animal(); // Compiles successfully but ClassCastException is thrown at runtime`

```java
class Animal { } 

class Dog extends Animal { 
 	static void method(Animal a) { 
 	   if(a instanceof Dog) {
 	      Dog d = (Dog) a; //downcasting 
         System.out.println("ok downcasting performed"); 
      }
   } 

   public static void main (String [] args) { 
 	   Animal a = new Dog(); 
 	   Dog.method(a); 
   }
} 
// Output:ok downcasting performed
```

### Explain Static and final modifier.

- **static keyword in java** : *static* is a non-access modifier in Java which can be applied on variables, methods, blocks, import and inner classes.
- The **static keyword** in java is used for memory management mainly. The static keyword belongs to the class rather than instance of the class.
- When a member is declared static, it can be accessed before any objects of its class are created, and without reference to any object.
- **Static blocks**

  - If you need to do computation in order to initialize your **static variables**, you can declare a static block that gets executed exactly once, when the class is first loaded. A class can have multiple static blocks and these will be executed in the same sequence in which they appear in class definition.

  ```java
  class DataObject 
  {
      public Integer nonStaticVar;
      public static Integer staticVar;    //static variable

      static {				             // It will be executed first
          staticVar = 40;
          //nonStaticVar = 20;    // Not possible to access non-static members
      }
  }
  ```

- **Static variables**
  - When a variable is declared as static, then a single copy of variable is created and shared among all objects at class level. Static variables are, essentially, global variables. All instances of the class share the same static variable.The static variable can be used to refer the common property of all objects (that is not unique for each object) e.g. company name of employees, college name of students etc.
  - The static variable gets memory only once in class area at the time of class loading.

### When to use static variables?

- Advantage of static variable is to makes your program **memory efficient**
  ```java
  class Student{  
       int rollno;  
       String name;  
       String college="ITS";  
  }
  ```
- Suppose there are 500 students in my college, now all instance data members will get memory each time when object is created.All student have its unique rollno and name so instance data member is good. Here, college refers to the common property of all objects. If we make it static, this field will get memory only once.

### Are static local variables allowed in Java?

- Unlike C/C++, static local variables are not allowed in Java. For example, following Java program fails in compilation with error “Static local variables are not allowed”
- **Static varibale can be used to keep the count of object created for particuler class**

### What are different types of `static` constructs?

1. **Static methods** When a method is declared with static keyword, it is known as static method. The most common example of a static method is  main( ) method. Methods declared as static have several restrictions:
   - They can only directly call other static methods.
   - They cannot refer to this or super in any way.
   - They can access only static variables inside static methods. If you try to access any non-static variable, the compiler error will be generated with message “Cannot make a static reference to the non-static field nonStaticVar“.
   - Static methods can be accessed via it’s class reference, and there is no need to create an instance/object of class. Though you can access using instance reference as well but it will have not any difference in comparison to access via class reference.
2. **Static Import Statement** The normal import declaration imports classes from packages, so that they can be used without package reference. Similarly **the static import declaration imports static members from classes** and allowing them **to be used without class reference**.
3. **Static Class** We cannot declare top-level class with a static modifier, but we can declare nested classes as static. Such type of classes are called Nested static classes. Just like other static members, nested classed belong with class scope so the inner static class can be accessed without having an object of outer class.

   ```java
   class DataObject 
   {
       public Integer nonStaticVar;
       public static Integer staticVar;         //static variable
    
       static class StaticInnerClas {
         Integer innerNonStaticVar = 60; 
         static Integer innerStaticVar = 70;   //static variable inside inner class
       }
   }

   // To access innerStaticVar we will use below statement.
   // DataObject.StaticInnerClas.innerStaticVar
   ```

   - Please note that an static inner class cannot access the non-static members of outer class. It can access only static members from outer class.

### What are different types of `final` constructs?

1. **final variable** If you make any variable as final, you cannot change the value of final variable (It will be constant). This also means that you must initialize a final variable.
   - **Initializing a final variable** : We must initialize a final variable, otherwise compiler will throw compile-time error.A final variable can only be initialized once, either via an initializer or an assignment statement. There are three ways to initialize a final variable :
   - You can initialize a final variable when it is declared.This approach is the most common. A final variable is called **blank final variable**, if it is **not** initialized while declaration. Below are the two ways to initialize a blank final variable.
     1. A blank final variable can be initialized inside instance-initializer block or inside constructor. If you have more than one constructor in your class then it must be initialized in all of them, otherwise compile time error will be thrown.
     2. A blank final static variable can be initialized inside static block.
   - **When to use a final variable :** The only difference between a normal variable and a final variable is that we can re-assign value to a normal variable but we cannot change the value of a final variable once assigned. Hence final variables must be used only for the values that we want to remain constant throughout the execution of program.

- **Reference final variable:** When a final variable is a reference to an object, then this final variable is called reference final variable. For example, a final StringBuffer variable looks like `final StringBuffer sb;`
  - As you know that a final variable cannot be re-assign. But in case of a reference final variable, internal state of the object pointed by that reference variable can be changed. Note that this is not re-assigning. This property of *final* is called *non-transitivity*. To understand what is mean by internal state of the object, see below example :

```java
// Java program to demonstrate reference final variable

    public static void main(String[] args) {
// a final reference variable sb
        final StringBuilder sb = new StringBuilder("Geeks");
        System.out.println(sb);
// changing internal state of object reference by final reference variable sb
        sb.append("ForGeeks");
        System.out.println(sb);
    }

// Output
// Geeks
// GeeksForGeeks
```

- The *non-transitivity* property also applies to arrays, because arrays are objects in java. Arrays with final keyword are also called final arrays.
- **Important Note :**
  1. A final variable cannot be reassign, doing it will throw compile-time error.
  2. When a final variable is created inside a method/constructor/block, it is called local final variable, and it must initialize once where it is created.
  3. Note the difference between C++ *const* variables and Java *final* variables. const variables in C++ must be assigned a value when declared. For final variables in Java, it is not necessary as we see in above examples. A final variable can be assigned value later, but only once.
  4. *final* with foreach loop : final with for-each statement is a legal statement.

```java
int arr[] = {1, 2, 3};
// final with for-each statement legal statement
for (final int i : arr)
 	System.out.print(i + " ");
```

- **Explanation** : Since the i variable goes out of scope with each iteration of the loop, it is actually re-declaration each iteration, allowing the same token (i.e. i) to be used to represent multiple variables.

2. **Java final classes**

- When a class is declared with final keyword, it is called a final class. A final class cannot be extended(inherited). There are two uses of a final class :
  1. One is definitely to prevent inheritance, as final classes cannot be extended. For example, all Wrapper Classes like Integer,Float etc. are final classes. We can not extend them.

```java
final class A { }

class B extends A {
   // The following class is Illegal. COMPILE-ERROR! Can't subclass A
}
```

2. The other use of final with classes is to create an immutable class like the predefined String class.You can not make a class immutable without making it final.

3. **Java final methods** When a method is declared with final keyword, it is called a final method. A final method cannot be overridden. The Object class does this—a number of its methods are final.We must declare methods with final keyword for which we required to follow the same implementation throughout all the derived classes.

### Can we declare a constructor final?

- No, because constructor is never inherited.

### Can we make local variable final in Java?

- Yes, you can make local variable final in Java. In fact, this was mandatory, if you want to access the local variable inside an Anonymous class until Java 8. From Java 8 onward, you don't need to make it final but make sure you don't change the value once assigned. This is also known as an effectively final variable in Java.

### Can you change the state of the object to which a final reference variable is pointing?

- Yes, you can change the state of the object referred by a final variable. This is one of the tricky concept in Java and often cause subtle errors. One of the most common examples of this is Collection classes e.g. ArrayList or HashMap referenced by a final variable. You can still add, remove and update elements but you cannot change the final variable to point to another collection. i.e. final variable cannot be swapped with another Collection

### Can we make an array final in Java? Can you change its elements?

- Yes, you can make an array final in Java and you can change it's elements as well. This is actually the follow-up to the previous question, both array and collection classes can be made final and you can still change their elements.

### What is the use of final class in Java?

- You make a class final when you think it's complete and nobody should alter the feature by creating a subclass. Generally, security sensitive classes are made final in Java e.g. String. Another reason is performance, compiler, and JIT both can make a lot of assumption if a class is final because they know overriding or polymorphism will not come into the picture.

### Can we make an abstract method final in Java?

- No, you cannot make an abstract method final in Java because, in order to use an abstract method, you must override it but the final method cannot be overridden in Java.

### What is constant in Java?

- A static final variable is known as constant in Java. They are also known as a compile time constant because of their value at the time of compilation. They are also inlined at the client end, means if you are using a static final variable then its value will be copied to your class at compile time. Which also means that you need to recompile all the classes which use the static final variable, whenever you change the value of a static final field.

### Explain Java Inner Classes

- **Java inner class** or nested class is a class which is declared inside the class or interface.
- We use inner classes to logically group classes and interfaces in one place so that it can be more readable and maintainable.
  Additionally, it can access all the members of outer class including private data members and methods.
- **Advantage of java inner classes**
  1. Nested classes represent a special type of relationship that is it can access all the members (data members and methods) of outer class including private.
  2. Nested classes are used to develop more readable and maintainable code because it logically group classes and interfaces in one place only
  3. Code Optimization: It requires less code to write
- Difference between nested class and inner class in Java is Inner class is a part of nested class. Non-static nested classes are known as inner classes.
- Types of Nested classes : Tere are two types of nested classes non-static and static nested classes. The non-static nested classes are also known as inner classes.
  - Non-static nested class (inner class)
    1. Member inner class - A class created within class and outside method.
    2. Anonymous inner class - A class created for implementing interface or extending class. Its name is decided by the java compiler.
    3. Local inner class - A class created within method.
  - Static nested class - A static class created within class.
- **Nested Inner class/ Member inner class** It can access any private instance variable of outer class. Like any other instance variable, we can have access modifier private, protected, public and default modifier.
  - Like class, interface can also be nested and can have access specifiers.
    Following example demonstrates a nested class.

```java
class Outer {
   // Simple nested inner class
   class Inner {
      public void show() {
           System.out.println("In a nested class method");
      }
    }
}
class Main {
   public static void main(String[] args) {
       Outer.Inner in = new Outer().new Inner();
     in.show();
   }
}

// O/p : In a nested class method
```

- As a side note, we can’t have static method in a nested inner class because an inner class is implicitly associated with an object of its outer class so it cannot define any static method for itself. For example the following program doesn’t compile.
- **Internal working of Java member inner class** The java compiler creates two class files in case of inner class. The class file name of inner class is "Outer$Inner". If you want to instantiate inner class, you must have to create the instance of outer class. In such case, instance of inner class is created inside the instance of outer class.
- **Anonymous inner classes** A class that have no name is known as anonymous inner class in java. It should be used if you have to override method of class or interface. Since an anonymous class has no name, it is not possible to define a constructor for an anonymous class. Java Anonymous inner class can be created by two ways:
  - **As subclass of specified type**

```java
class Demo {
   void show() {
      System.out.println("I am in show method of super class");
   }}

class Flavor1Demo {
   //  An anonymous class with Demo as base class
   static Demo d = new Demo() {
       void show() {
           super.show();
System.out.println("I am in Flavor1Demo class");
       }};
   public static void main(String[] args){
       d.show();
   }}

// O/p
// I am in show method of super class
// I am in Flavor1Demo class
```

- In the above code, we have two class Demo and Flavor1Demo. Here demo act as super class and anonymous class acts as a subclass, both classes have a method show(). In anonymous class show() method is overridden.

  - **As implementer of the specified interface**

```java
class Flavor2Demo {

    // An anonymous class that implements Hello interface
    static Hello h = new Hello() {
        public void show() {
            System.out.println("I am in anonymous class");
        }
    };

    public static void main(String[] args) {
        h.show();
    }
}

interface Hello {
    void show();
}

// o/p : I am in anonymous class
```

- In above code we create an object of anonymous inner class but this anonymous inner class is an implementer of the interface Hello. Any anonymous inner class can implement only one interface at one time. It can either extend a class or implement interface at a time.
- **Java Local inner class** A class i.e. created inside a method is called local inner class in java. If you want to invoke the methods of local inner class, you must instantiate this class inside the method.

```java
class Outer {
    void outerMethod() {
        System.out.println("inside outerMethod");
        // Inner class is local to outerMethod()
        class Inner {
            void innerMethod() {
                System.out.println("inside innerMethod");
            }
         }
        
        Inner y = new Inner();
        y.innerMethod();
    }
}
class MethodDemo {
    public static void main(String[] args) {
        Outer x = new Outer();
        x.outerMethod();
    }
}

// o/p
// Inside outerMethod
// Inside innerMethod
```

**Note** :

1. Local inner class cannot access non-final local variable till JDK 1.7. Since JDK 1.8, it is possible to access the non-final local variable in method local inner class. The main reason we need to declare a local variable as a final is that local variable lives on stack till method is on the stack but there might be a case the object of inner class still lives on the heap. Method local inner class can’t be marked as private, protected, static and transient but can be marked as abstract and final, but not both at the same time. A local inner class can access all the members of the enclosing class and local final variables in the scope it’s defined.
2. Local inner class cannot be invoked from outside the method.

- **Java static nested class** A static class i.e. created inside a class is called static nested class in java. It can be accessed by outer class name.
  - It can access static data members of outer class including private.
  - Static nested class cannot access non-static (instance) data member or method.

```java
class Outer {
   private static void outerMethod() {
     System.out.println("inside outerMethod");
   }

   // A static inner class
   static class Inner {
        public static void main(String[] args) {
          System.out.println("inside inner class Method");
          outerMethod();
        }
   }
}

// O/p
// inside inner class Method
// inside outerMethod
```

- Static class object can be created with following `statement.OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();`

### Is Java “pass-by-reference” or “pass-by-value”?

- Pass by Value: The method parameter values are copied to another variable and then the copied object is passed, that’s why it’s called pass by value.
- Pass by Reference: An alias or reference to the actual parameter is passed to the method, that’s why it’s called pass by reference.
- When an object is passed by value, this means that a copy of the object is passed. Thus, even if changes are made to that object, it doesn’t affect the original value. When an object is passed by reference, this means that the actual object is not passed, rather a reference of the object is passed. Thus, any changes made by the external method, are also reflected in all places.
- Java is always pass-by-value. Unfortunately, they decided to call the location of an object a "reference". When we pass the value of an object, we are passing the reference to it. This is confusing to beginners.
  It goes like this:

```java
public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    // we pass the object to foo
    foo(aDog);
    // aDog variable is still pointing to the "Max" dog when foo(...) returns
    aDog.getName().equals("Max"); // true
    aDog.getName().equals("Fifi"); // false
}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true
    // change d inside of foo() to point to a new Dog instance "Fifi"
    d = new Dog("Fifi");
    d.getName().equals("Fifi"); // true
}
```

- In the example above aDog.getName() will still return "Max". The value aDog within main is not changed in the function foo with the Dog "Fifi" as the object reference is passed by value. If it were passed by reference, then the aDog.getName() in main would return "Fifi" after the call to foo.

### Explain forEach loop.

- The for-each loop introduced in Java5. It is mainly used to traverse array or collection elements. 
  Syntax : for(data_type variable : array | collection){}
- Important Points :for-each loop is applicable only to Java array and Collection classes which implements Iterable interface, and since all built-in Collection classes implements java.util.Collection interface, which already extends Iterable, this detail mostly gets unnoticed.

### Here we are iterating over ArrayList using standard iterator and for-each loop and subsequently removing elements as well, you need to find out which code will throw ConcurrentModificationException and why?

```java
Collection<String> list = new ArrayList<String>();
    list.add("Android");
    list.add("iPhone");
    list.add("Windows Mobile");

    // example 1
    Iterator<String> itr = list.iterator();
    while(itr.hasNext()){
        String lang = itr.next();
        list.remove(lang);
    }

     // example 2
    for(String language: list){
        list.remove(language);
    }
```

- Mostly people will say that first code block will throw `ConcurrentModificationException`, because we are not using Iterator's remove method for removing elements, instead we are using ArrayList's `remove()` method. But, not many people will say same thing about for-each loop, because we are not using Iterator there. In reality, second code snippet will also throw ConcurrentModificationException. Since for-each loop internally uses Iterator to traverse over Collection, it also call's Iterator.next(), which checks for modification and throws ConcurrentModificationException.

### Why Serializable is an interface?

### Let's say two singleton object are already persisted into the files and we want to deserialize them.

### Give me an example of design pattern which is based upon open closed principle?

### What is Law of Demeter violation? Why it matters?

### What is differences between External Iteration and Internal Iteration?

### Define enums? Can you call an enum constructor from outside? What is a constant specific method?

### What is the difference between a **Stream** and an **Iterator**?

### Advanced modifiers : volatile, transient, native, strictfp
