### Design Patterns

- Design Patterns are simply sets of standardized practices used in the software development industry
- There are three main design patterns categories: **Creational Patterns**, **Structural Patterns**, and **Behavioural Patterns**
- Creational Patterns are most concerned about solutions and options revolving around instantiating objects.
- Structural patterns are concerned about providing solutions and efficient standards regarding class compositions and object structures. they rely on the concept of inheritance and interfaces to allow multiple objects or classes to work together and form a single working whole.
- Behavioral Patterns are concerned about providing solutions regarding object interaction - how do they communicate, how are some dependent on others, and how to segregate them to be both dependent and independent and provide both flexibility and testing capabilities.

### Creational Patterns

#### Factory Method

- In this pattern, a Factory class is created as the parent class of all sub-classes belonging to a certain logical segment of related classes.
- A Factory Pattern or Factory Method Pattern says that just define an interface or abstract class for creating an object but let the subclasses decide which class to instantiate. In other words, subclasses are responsible to create the instance of the class.

#### Abstract Factory

- The Abstract Factory design pattern builds upon the Factory Pattern and acts as the highest factory in the hierarchy. It represents the practice of creating a factory of factories.

#### Builder

- The Builder pattern is used to help build final objects, for classes with a huge amount of fields or parameters in a step-by-step manner

#### Prototype

- The Prototype pattern is used mainly to minimize the cost of object creation, usually when large-scale applications create, update or retrieve objects which cost a lot of resources.
- This is done by copying the object, once it's created, and reusing the copy of the object in later requests, to avoid performing another resource-heavy operation

#### Singleton

- The Singleton pattern ensures the existence of only one object instance in the whole JVM.
- This is a rather simple pattern and it provides the ability to access this object even without instantiating it
- This class is creating a static object of itself, which represents the global instance.
- A static method getInstance() is used as a global access point for the rest of the application

### Structural Patterns

#### Adapter

- The Adapter pattern, as the name implies, adapts one interface to another. It acts as a bridge between two unrelated, and sometimes even completely incompatible interfaces
- Adapter design pattern is one of the structural design pattern and its used so that two unrelated interfaces can work together. The object that joins these unrelated interface is called an Adapter.

#### Bridge

- The Bridge pattern is used to segregate abstract classes from their implementations and act as a bridge between them. This way, both the abstract class and the implementation can change structurally without affecting the other

#### Filter

- The Filter pattern is used when we need a way to filter through sets of objects with different custom criteria. We can chain criteria for an even narrower filter, which is done in a decoupled way.

#### Composite

- The Composite pattern is used when we need a way to treat a whole group of objects in a similar, or the same manner.
- This is usually done by the class that "owns" the group of objects and provides a set of methods to treat them equally as if they were a single object.

#### Decorator

- The Decorator pattern is used to alter an individual instance of a class at runtime, by creating a decorator class which wraps the original class.

#### Facade

- The Facade pattern provides a simple and top-level interface for the client and allows it to access the system, without knowing any of the system logic and inner-workings.

#### Flyweight

- The Flyweight pattern is concerned with reducing the strain on the JVM and its memory
- When a certain application needs to create many instances of the same class, a common pool is created so that similar ones can be reused, instead of created each time.

#### Proxy

- The Proxy pattern is used when we want to limit the capabilities and the functionalities of a class, by using another class which limits it.
- By using this proxy class, the client uses the interface it defines, to access the original class. This ensures that the client can't do anything out of order with the original class since all of his requests pass through our proxy class.

### Behavioral Patterns

#### Interpreter

- The Interpreter pattern is used anytime we need to evaluate any kind of language grammar or expressions.
- Example Google Translate. Java Compiler.

#### Template Method

- The Template Method, otherwise known as Template Pattern is all around us. It boils down to defining an abstract class that provides predefined ways to run its methods. Sub-classes that inherit these methods must also follow the way defined in the abstract class.

#### Chain of Responsibility

- The Chain of Responsibility pattern is widely used and adopted. It defines a chain of objects, that collectively, one after another, process the request - where each processor in the chain has its own processing logic.

#### Command

- Command pattern works by wrapping the request from the sender in an object called a command. This command is then passed to the invoker object, which proceeds to look for the adequate way to process the request

#### Iterator

- It's used to access the members of collections all the while hiding the underlying implementation.

#### Mediator

- The Mediator pattern acts as a bridge and, as the name implies, the mediator between different objects which communicate in any way
- The Mediator pattern addresses this issue by acting as a third party over which the communication is done, decoupling them in the process

#### Memento

- The Memento pattern is concerned with previous states of the object. This means that the pattern is used when we want to save some state of an object, in the case we might want to restore the object to that state later on.
- This pattern relies on the work of three classes, also known as actor classes. The `Memento` object contains a state that we wish to save for later use. The `Originator` object creates and stores states in the `Memento` objects, while the `CareTaker` object takes care of the restoration process.

#### Observer

- The Observer pattern is used to monitor the state of a certain object, often in a group or one-to-many relationship. In such cases, most of the time, the changed state of a single object can affect the state of the rest, so there must be a system to note the change and alert the other objects.

#### State

- The State pattern is used when a specific object needs to change its behavior, based on its state. This is accomplished by providing each of these objects with one or more state objects.

#### Strategy

- The Strategy pattern is employed in situations where algorithms or class' behavior should be dynamic. This means that both the behavior and the algorithms can be changed at runtime, based on the input of the client.
- Strategy design pattern is one of the behavioral design pattern. Strategy pattern is used when we have multiple algorithm for a specific task and client decides the actual implementation to be used at runtime.
- Example: Simple Shopping Cart where we have two payment strategies – using Credit Card or using PayPal.

#### Visitor

- The Visitor pattern is used to move the operational logic from each individual element of a group, into a new class, which does the operation for them utilizing the data from each individual element.
- This is done by making all of the elements accept a "visitor". This visitor will perform changes in a separate class, without changing the structure of the visited class at all. This makes it easy to add new functionality without changing visited classes at all.

### J2EE Patterns

#### MVC Pattern

- **Models** are basically objects, or POJO's to be exact, used as blueprints/models for all of the objects that will be used in the application.
- **Views** represent the presentational aspect of the data and information located in the models.
- **Controllers** controls both of these. They serve as a connection between the two. Controllers both instantiate, update and delete models, populate them with information, and then send the data to the views to present to the end-user.

#### Business Delegate Pattern

- The Business Delegate pattern is used to decouple the presentation layer from the business layer to minimize the number of requests between the client (presentation) and the business tiers.

#### Composite Entity Pattern

- The Composite Entity pattern represents a graph of objects, which when updated, triggers an update for all the dependent entities in the graph.

#### Data Access Object Pattern

- The Data Access Object pattern, most often shortened to DAO is a pattern in which objects are dedicated to the communication with the Data Layer.
- These objects often instantiate "SessionFactories" for this purpose and handle all of the logic behind communicating with the database.
- The standard practice is to create a DAO interface, followed by a concrete class implementing the interface and all methods defined in it.

#### Front Controller Pattern

- Upon sending a request, the Front Controller is the first controller it reaches. Based on the request, it decides which controller is the most adequate to handle it, after which it passes the request to the chosen controller.
- The Front Controller is most often used in Web Applications in the form of a _Dispatcher Servlet_.
- The front controller design pattern is used to provide a centralized request handling mechanism so that all requests will be handled by a single handler. This handler can do the authentication/authorization/logging or tracking of request and then pass the requests to corresponding handlers. Following are the entities of this type of design pattern.
  - Front Controller - Single handler for all kinds of requests coming to the application (either web based/ desktop based).
  - Dispatcher - Front Controller may use a dispatcher object which can dispatch the request to corresponding specific handler.
  - View - Views are the object for which the requests are made.

#### Intercepting Filter pattern

- Filters are used before the request is even passed to the adequate controllers for processing. These filters can exist in the form of a Filter Chain and include multiple filters, or simply exist as one Filter.
- Nevertheless, they run checks on authorization, authentication, supported browsers, whether the request path violates any constraints and restrictions etc

#### Service Locator Pattern

- A pattern often seen in Web Applications, the Service Locator pattern is used to decouple the Service Consumers and the concrete classes like DAO implementations.
- The pattern looks for the adequate service, saves it in cache storage to reduce the number of requests and therefore the strain on the server and p rovides the application with their instances.

#### Transfer Object Pattern

- This pattern is used to transfer objects with lots of fields and parameters in one go. The Transfer Object pattern employs new objects, used only for transfer purposes, usually passed to the DAO.
- These objects are **serializable POJOs**. They have fields, their respective getters and setters, and no other logic.

### When and where to use the Singleton Pattern?

- Use the singleton pattern to encapsulate a resource that should only ever be created (initialised) once per application. You usually do this for resources that manage access to a shared entity, such as a database. A singleton can control how many concurrent threads can access that shared resource. i.e. because there is a single database connection pool it can control how many database connections are handed out to those threads that want them. A Logger is another example, whereby the logger ensures that access to the shared resource (an external file) can be managed appropriately. Oftentimes singletons are also used to load resources that are expensive (slow) to create.
- There are many objects we only need one of: thread pools, caches, dialog boxes, objects that handle preferences and registry settings, objects used for logging, and objects that act as device drivers to devices like printers and graphic cards. For many of these types of object if we were to intantiate more than one we would run intol all sorts of problems like incorrect program behavior or overuse of resources.  A few real world examples include the the SessionFactory class in Hibernate – it’s actually a singleton. Or with log4j, when you call its logger, it uses a singleton class to return it.

### What is the difference between factory and abstract factory pattern?

- The Factory Method is usually categorised by a switch statement where each case returns a different class, using the same root interface so that the calling code never needs to make decisions about the implementation.
- For example credit card validator factory which returns a different validator for each card type.

```java
public ICardValidator GetCardValidator(String cardType)
{
    switch (cardType.ToLower())
    {
        case "visa":
            return new VisaCardValidator();
        case "mastercard":
        case "ecmc":
            return new MastercardValidator();
        default:
            throw new CreditCardTypeException("Do not recognise this type");
    }
}
```

- Abstract Factory patterns work around a super-factory which creates other factories. This factory is also called as factory of factories. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.
- In Abstract Factory pattern an interface is responsible for creating a factory of related objects without explicitly specifying their classes. Each generated factory can give the objects as per the Factory pattern.

### When do you use Flyweight pattern?

### What is difference between dependency injection and factory design pattern?

### Difference between Adapter and Decorator pattern?

### Difference between Adapter and Proxy Pattern?

### What is Template method pattern?

### When do you use Visitor design pattern?

### When do you use Composite design pattern?

### Difference between Abstract factory and Prototype design pattern?
