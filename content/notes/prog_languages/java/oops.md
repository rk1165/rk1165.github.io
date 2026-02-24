+++
title = 'OOPS Concepts'
date = 2024-02-11

+++

- **Polymorphism** : Polymorphism is the ability to process objects differently based on the data type or class. `overloading`, `overriding` methods
- The most common relation ships between classes are:
  - _Dependence_ ("uses-a") most obvious and most general. e.g, Order class uses the Account class because Order objects need to access Account objects to check for credit status.
  - _Aggregation_ ("has-a") e.g, Order object contains Item objects.
  - _Inheritance_ ("is-a") e.g, a RushOrder class inherits from an Order class.
- `Objects.requireNonNullElse(n, 'unknown')`
- `Objects.requireNonNull(n, 'The name can't be null')`
- As a rule of thumb, always use `clone` to return a copy of a mutable field.
- Use static methods in two situations:
  - When a method doesn't need to access the object state because all needed parameters are supplied as explicit parameters.
  - When a method only needs to access static fields of the class.
- If the subclass constructor doesn't call a superclass constructor, the no-arg constructor of the superclass is invoked. If the superclass doesn't have a no-arg constructor and subclass constructor doesn't call another superclass constructor explicitly, the compiler reports an error.
- The keyword used for inheritance is **extends**.
- `super` keyword is similar to this keyword. Following are the scenarios where the `super` keyword is used.
- A subclass inherits all the members (fields, methods, and nested classes) from its superclass. Constructors are not inherited by subclasses
- A subclass does not inherit the private members of its parent class.
- Access modifier of overridden methods cannot be more restrictive than original method.
- The `synchronized` modifier has no effect on the rules of overriding.
- Constructors cannot be overridden.
- If superclass method throws an exception, then subclass overridden method can throw the same exception or no exception or any unchecked exceptions, but must not throw parent exception of the exception thrown by Superclass method.
- `static` methods can't be overridden because static method is bound to a class whereas method overriding is associated with object
- We can have two or more `static` methods with same name but different input params i.e different number of params and/or different types of parameters.
- If a child class defines a `static` method with same signature as a `static` method in parent class, the method in the parent class hides the method in the child class. Which means if we do inheritance test, it will call parent method.
- `static` methods can be called by `null` reference whereas non-static methods can't.
- `private` methods can be overloaded but not overridden.
- `final` methods can be overloaded but not overridden. Though we can inherit it inside child class
- Since Java 5.0, it is possible to have different return type for an overriding method in child class, but child's return type should be **subtype** of parent's return type. Example `clone` method of `Object`
- Concept of binding data with member functions. Aims at hiding class members from outside interference and misuse. `public`, `protected`, `private`, `default`
- **Hiding internal details and showing functionality** is known as abstraction. In Java, we use abstract class and interface to achieve abstraction.
- **Abstract class**: These classes cannot be **instantiated** and are either partially implemented or not at all implemented. We can not create the object of it. This class contains one or more abstract methods which are simply method declarations without a body.
- An abstract class can implement an interface, and not provide implementations of all of the interface's methods. It is the responsibility of the first concrete class that has that abstract class as an ancestor to implement all of the methods in the interface.
- **Interface**: In Java an interface just defines the methods and not implement them. Interface can include constants. A class that implements the interfaces is bound to implement all the methods defined in an interface.
- Interfaces are limited to public methods and constants with no implementation of methods. Abstract classes can have a partial implementation, protected methods, static methods, etc.
- Consider using the `interface` when our problem makes the statement "**A is capable of $doing_this**". For instance, `Clonable` is capable of cloning an object. `Drawable` is capable of drawing a shape.
- Consider using `abstract` classes and inheritance when our problem makes the evidence "**A is a B**". For example, "Dog is an Animal", "Lamborghini is a Car"
- Marker interface is an interface with no fields or methods in Java. e.g. Serializable, Cloneable, Remote etc
- You cannot apply both `final` and `abstract` keyword at the class.
- Every Interface (that doesn't explicitly extend another interface) has an implicit method declaration for each public method in `Object` class.

### Method Overloading mechanism

- If more than one member method is both accessible and applicable to a method invocation, it is necessary to choose one to provide the descriptor for the run-time method dispatch. Java uses rules such that the most specific method is chosen
- **Note**: Rules that apply for evaluating method call in overloading.
  1. Widening wins over boxing eg. `test(10)` will call `test(long)` instead of `test(Integer)` if both are available.
  2. Widening wins over var-args eg. `test(byte, byte)` will call `test(int, int)` instead of `test(byte...x)` method.
  3. Boxing beats var-args eg `test(byte, byte)` will call `test(Byte, Byte)` instead of `test(byte...x)` method.
  4. Widening of reference variable depends on inheritance tree (so, Integer cannot be widened to Long. But, Integer widened to Number because they are in same inheritance hierarchy).
  5. You **cannot** widen and then box. Eg. `test(int)` cannot call `test(Long)` since to call `test(Long)` the compiler need to convert `int` to `Integer` then `Integer` to `Long` which is not possible.
  6. You **can** box and then widen. Eg. An `int` can be boxed to `Integer` and then widen to `Object`.
  7. var-args can be combined with either boxing or widening.
- Java's widening conversions rules are,
  - From a byte ---> short ---> int ---> long ---> float ---> double
  - From a short ---> int ---> long ---> float ---> double
  - From a char ---> int ---> long ---> float ---> double
  - From an int ---> long ---> float ---> double
  - From a long ---> float ---> double
  - From a float ---> double
- **Java overloaded method call is resolved using 3 steps**
  1. Compiler will try to resolve call without boxing and unboxing and variable argument.
  2. Compiler will try to resolve call by using boxing and unboxing.
  3. Compiler will try to resolve call by using boxing/unboxing and variable argument.
- If call is not resolved by using any of the 3 ways then it gives compile error.
