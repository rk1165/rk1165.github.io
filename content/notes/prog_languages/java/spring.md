+++
title = 'Spring Framework'
date = 2024-02-11

+++

### Spring tutorial

- Spring Data repositories are interfaces with methods supporting CRUD operations against a backend data store
- To wrap your repository with a web layer, you must turn to Spring MVC
- [Spring HATEOAS](https://spring.io/projects/spring-hateoas), a Spring project aimed at helping you write hypermedia-driven outputs.
- This tiny library will give us the constructs to define a RESTful service and then render it in an acceptible format for client consumption.
- A critical ingredient to any RESTful service is adding links to relevant operations

### Inversion of Control (IoC)

- Inversion of Control is a principle in software engineering by which the control of objects or portions of a program is transferred to a container or framework.
- The approach of outsourcing the construction and management of objects.
- Spring Container's Primary functions are
  - Create and manage objects (_Inversion of Control_)
  - Inject object's dependencies (_Dependency Injection_)
- Dependency injection is a pattern through which to implement IoC, where the control being inverted is the setting of object's dependencies.
- In the Spring framework, the IoC container is represented by the interface `ApplicationContext` and `BeanFactory`
- `BeanFactory` is the root interface for accessing Spring container. `ApplicationContext` is a sub-interface of `BeanFactory`.
- Implementations for ApplicationContext
  - ClassPathXmlApplicationContext
  - FileSystemXmlApplicationContext
  - WebApplicationContext
  - AnnotationConfigApplicationContext
- In Spring, a bean is an object that the Spring container instantiates, assembles, and manages.
- In order to assemble beans, the container uses configuration metadata, which can be in the form of XML configurations or annotations.
- Methods for Configuring Spring Container
  - XML configuration file (most legacy apps still use this)
  - Java Annotations (modern)
  - Java Source Code (modern)
- Spring Development Process
  1. Configure your Spring Beans
  2. Create a Spring Container
  3. Retrieve Beans from Spring Container
- Spring container is generically known as **Application Context**
- "dependency" same thing as "helper objects".
- outsource the construction and injection of objects to an external entity (eg. car factory)
- Multiple Application Context can be loaded using comma separated value in `ClassPathXmlApplicationContext`
- **Injection Types**
  - Constructor Injection
  - Setter Injection
- **Constructor Injection**
  1. Define the dependency interface and class
  2. Create a constructor in your class for injections
  3. Configure the dependency injection in Spring config file.
- **Setter Injection** : Inject dependencies by calling setter method(s) on your class.
  1. Create setter method(s) in your class for injections
  2. Configure the dependency injection in Spring config file.
- Spring takes the property name and looks for the `${setName}` method in our class.
- **Injecting Literal values from Properties file**
  1. Create Properties file
  2. Load Properties File in Spring config file
  3. Reference values from Properties File
- Autowire byName, byType, constructor
- byName : Spring looks for the name of the dependencies and checks if xml has beans with the same name. If found it injects them
- byType : Spring looks for type of dependencies and checks if xml has bean with those types. Here we can have only a single type for each dependency.
- constructor : Works similar to byType except instead of setter injection it uses constructor injection

### Bean Scopes

- Scope refers to the lifecycle of a bean
- How long does the bean live?
- How many instances are created?
- How is the bean shared?
- Default Scope: **Singleton**
- What is Singleton?
  - Spring container creates only one instance of bean by default.
  - It is cached in memory
  - All requests for the bean : will return a SHARED reference to the SAME bean.

- We can have multiple container running inside a JVM and each of those container can have a singleton bean.
- ApplicationContext is initialized only once. If we want to refer it in another class we need to implement interface : `ApplicationContextAware` and override the method `setApplicationContext`
- Spring calls the setters from the Aware interfaces.
- If we have defined init and destroy using xml and implementing interfaces then first InitializingBean init is called, then our init and then DisposableBean's destroy and finally our cleanup method.
- BeanPostProcessor are classes that tells Spring there's some processing that Spring needs to do after initializing the bean.
- When every bean is initialized then BeanPostProcessor method is called.

### Bean Lifecycle Methods / Hooks

- You can add custom code during **bean initialization**
  - Calling custom business logic methods
  - Setting up handles to resources (db, sockets, file etc)
- You can add custom code during **bean destruction**
  - Calling custom business logic methods
  - Setting up handles to resources (db, sockets, file etc)
- Dev Process
  1. Define your methods for init and destroy
  2. Configure the method names in Spring config file
- Method signatures of `init-method` and `destroy-method`:
  - **Access modifier** can be anything (public, protected, private)
  - **Return type** can have any return type. "void" is most commonly used.
  - **Method name** can have any method name
  - **Arguments** the method should be no-arg
- For **prototype** scoped beans, Spring does not call the destroy method.

### Java Annotations

- Special labels/markers added to Java classes
- Provide metadata about the class
- Processed at compile time or run-time for special processing.
- At compile time, compiler will check/verify the override.
- Development Process
  1. Enable component scanning in Spring config file
  2. Add the `@Component` Annotation to your Java classes
  3. Retrieve bean from Spring container
- Spring also supports Default Bean IDs
  - Default bean id: the class name, _make first letter lower case_. So TennisCoach has default bean id as tennisCoach

### Spring AutoWiring

- For DI, Spring can use autowiring
- Spring will look for a class that _matches_ the property
  - _matches by type_: class or interface
- Spring will inject it automatically ... hence it is autowired
- Autowiring Injection Types
  - Constructor Injection
  - Setter injection
  - Field injection
- **Constructor Injection**
  1. Define the dependency interface and class
  2. Create a constructor in your class for injections
  3. Configure the dependency injection with `@Autowired` Annotation
- Any method in the class can be used to inject dependencies. Simply give `@Autowired`
- **Field Injection**
  - Inject dependencies by setting field values on class directly. (even private fields)
  - This is accomplished by using Java Reflection
- **Dev Process**
  1. Configure the DI with Autowired Annotation
     - Applied directly to the field
     - No need for setter methods
- Stay consistent with the Injection Types
- What happens if multiple interface implementations?
  - `NoUniqueBeanDefinitionException`
  - To resolve this, we need to tell Spring which specific bean it must use. To do this we use `@Qualifier("$defaultBeanIdThatWeWantSpringToInject")`
  - `@Qualifier` can be used for all three types of injection
- You have to place the `@Qualifier` annotation inside of the constructor arguments.

```java
  TennisCoach(@Qualifier("randomFortuneService") FortuneService theFortuneService)
```

- **Bean Scopes with Annotations** `@Scope("singleton")` `@Scope("prototype")`
- **Bean Lifecycle with Annotations**
  1. Define your methods for init and destroy
  2. Add annotations: `@PostConstruct` and `@PreDestroy`
- For "prototype" scoped beans, Spring does not call the `@PreDestroy` method.
- Ways of configuring Spring container
  1. Full XML config
  2. XML Component scan
  3. Java Configuration class
- Instead of configuring Spring container using XML we will configure the Spring container with Java code.
- Development Process
  1. Create a java class and annotate as `@Configuration`
  2. Add component scanning support: `@ComponentScan` (optional)
  3. Read Spring java configuration class Using `AnnotationConfigApplicationContext`
  4. Retrieve bean from Spring container
- **Defining Spring Beans with Java Code**
  1. Define method to expose bean : using `@Bean`
  2. Inject bean dependencies
  3. Read Spring Java configuration class
  4. Retrieve bean from Spring container

### Spring MVC

- What is Spring MVC? Spring way of building web app UIs in Java
  - Framework for building web applications in Java
  - Based on Model-View-Controller design pattern
  - Leverages features of the Core Spring Framework (IoC, DI)
- Spring MVC Benefits
  - Leverage a set of reusable UI components
  - Help manage application state for web requests
  - Process form data: validation, conversion
  - Flexible configuration for the view layer
- Components of a Spring MVC Application
  - A set of web pages to layout UI components (Web Pages)
  - A collection of Spring beans (controllers, services etc) (Beans)
  - Spring configuration (XML, Annotations or Java) (Spring configuration)
- Spring MVC Configuration Process
  - Part 1: Add configrations to file: `WEB-INF/web.xml`
    1. Configure Spring MVC Dispatcher Servlet
    2. Set up URL mappings to Spring MVC Dispatcher servlet
  - Part 2: Add configurations to file: `WEB-INF/servlet.xml` 3. Add support for Spring component scanning 4. Add support for conversion, formatting and validation 5. Configure Spring MVC View Resolver
- **Creating a Spring Home Controller and View**
  1. Create Controller class
  2. Define Controller method
  3. Add Request Mapping to Controller method
  4. Return View Name
  5. Develop View Page

### Spring Model

- The **Model** is a container for your application data
- In your Controller
  - You can put anything in the **model** - strings, objects, info from database, etc...
- Your View page (JSP) can access data from the model
- If you need to read form data in your controller code then you need to pass in the `HttpServletRequest` (Holds HTML form data) also you can pass the `Model`
- `@RequestParam("studentName") String theName` Spring will read param from request: `studentName` and bind it to the variable : `theName`
- Conflict in request paths: we can add a parent mapping

### Spring MVC Form Tags

- Spring MVC Form Tags are the building block for a web page
- Form Tags are configurable and reusable for a web page
- Spring MVC Form Tags can make use of data binding
- Form tags will generate HTML for you
- JSP page with special Spring MVC form tags
- When form is loaded Spring MVC will call : `getFirstName()` and `getLastName()`
- When form is submitted Spring MVC will call : `setFirstName()` and `setLastName()`
- Drop-Down List is represented by the tag : `<form:select>`
- Radio Button : `<form:radiobutton>`

### Form validation

- Check the user input form for
  - required fields
  - valid numbers in range
  - valid format (postal code)
  - custom business role
- For this we can use Java's Standard Bean Validation API
- Validation feature:
  - required
  - validate length
  - validate numbers
  - validate with reg exp
  - custom validation
- `@Valid` perform validation rules on `Customer` object. Results of that validation will be placed in the `BindingResult`
- the `BindingResult` parameter must be immediately after the model attribute.
- `@InitBinder` annotation works as a pre-processor
- It will pre-process each web request to our controller

### Hibernate

- A framework for persistence / saving Java objects in a database
- Hibernate handles all of the low-level SQL
- Hibernate provides the Object-to-Relational mapping
- The developer defines mapping between Java class and database table
- Hibernate configuration file tells how to connect to the database
- session-factory allows us to get session objects for connecting to database
- Entity class: Java class that is mapped to a database table
- Two options for mapping
  - XML config file (legacy)
  - Java Annotations (modern)
- Map class to database table
- Map fields to database columns
- Why using JPA annotations instead of Hibernate: JPA is a standard specification. Hibernate is an implementation of the JPA specification. Hibernate implements all of the JPA annotations. The Hibernate team recommends the use of JPA annotations as a best practice.
- **SessionFactory**:
  - Reads the hibernate config file
  - Creates Session objects
  - Heavy-weight object
  - Only create once in your app and reuse it over and over again
- **Session**
  - Wrapper around JDBC connection
  - Main object used to save/retrieve objects
  - Short lived object
  - Retrieved from SessionFactory
- Can tell hibernate a strategy to generate the primary key.

### ID Generation strategies

| Name                    | Description                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| GenerationType.AUTO     | Pick an appropriate strategy for the particular database                    |
| GenerationType.IDENTITY | Assign primary keys using database identity column                          |
| GenerationType.SEQUENCE | Assign primary keys using database sequence                                 |
| GenerationType.TABLE    | Assign primary keys using an underlying database table to ensure uniqueness |

- Most commonly used for MySQL GenerationType.IDENTITY
- You can define your own CUSTOM generation strategy
  - create subclass of `org.hibernate.id.SequenceGenerator`
  - Override the method: `public Serializable generate(..)` add your custom business logic
- How to change the `AUTO_INCREMENT` values?
- if not found when reading from the db, session will return null, so include a check for it
- Similar to SQL - HQL, the query language for Hibernate
- **Advanced Mappings**
  - One-to-One
  - One-to-Many
  - Many-to-One
  - Many-to-Many
- Primary key: identify a unique row in a table
- Foreign key:
  - Link tables together
  - a field in one table that refers to primary key in other table
- Cascade
  - You can **cascade** operations
  - Apply the same operation to related entities
  - Save on instructor then also cascade and apply the same operation on Instructor detail
- If we delete an instructor, we should also delete their instructor_detail. This is known as **CASCADE DELETE**
  - Cascade delete depends on the use case. Can't do it in many-to-many
- Uni-directional and Bi-directional

### One-to-One Mapping

- Process
  1. Prep Work - Define database tables
  2. Create InstructorDetail class
  3. Create Instructor class
  4. Create Main App

**Entity Lifecycle**: A set of state that a Hibernate entity can go through when using in our application.

| Operations | Description                                                                      |
| ---------- | -------------------------------------------------------------------------------- |
| Detach     | If entity is detached, it is not associated with a Hibernate session             |
| Merge      | If instance is detached from session, then merge will reattach to session        |
| Persist    | Transitions new instances to managed state. Next flush / commit will save in db  |
| Remove     | Transitions managed entity to be removed. Next flush/commit will delete from db. |
| Refresh    | Reload / synch object with data from db. Prevents stale data                     |

**Cascade Types**

| Cascade Type | Description                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------- |
| PERSIST      | If entity is persisted/saved, related entity will also be persisted                          |
| REMOVE       | If entity is removed/deleted, related entity will also be deleted                            |
| REFRESH      | If entity is refrsehed, related entity will also be refreshed                                |
| DETACH       | If entity is detached (not associated w/ session), then related entity will also be detached |
| MERGE        | If entity is merged, then related entitly will also be merged                                |
| ALL          | All of above cascade types together                                                          |

- `@OneToOne(cascade=CascadeType.ALL)`
- By default no operations are cascaded
- **Bi-directional**
  - If we load an `InstructorDetail`
    - then we'd like to get the associated instructor
    - we can solve this using bi-directional relationship
- Development Process: Only code in Java is changed.
  1. Make updates to InstructorDetail class
     - Add new field to reference Instructor
     - Add getter/setter methods for Instructor
     - Add `@OneToOne` annotation
  2. Create Main app
- mappedBy tells hibernate: `@OneToOne(mappedBy="instructorDetail")` this Instructor field is mapped by instructorDetail property in Instructor class.
- Use information from the Instructor class `@JoinColumn`
- to help find associated instructor
- Delete the InstructorDetail and keep the Instructor
  - Modify the cascadeType to `{CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH}`
  - we need to remove the bi-directional reference
- One-to-Many Mapping

### Fetch Types: Eager vs Lazy

- When we fetch / retrieve data, should we retrieve EVERYTHING
  - **Eager** will retrieve everything
  - **Lazy** will retrieve on request
- Eager Loading will load all dependent entities
  - Load instructor and all of their courses at once
- Prefer Lazy Loading instead of Eager loading
- `fetch=FetchType.LAZY`
- Default Fetch Types:

| Mapping     | Default Fetch Type |
| ----------- | ------------------ |
| @OneToOne   | FetchType.EAGER    |
| @OneToMany  | FetchType.LAZY     |
| @ManyToOne  | FetchType.EAGER    |
| @ManyToMany | FetchType.LAZY     |

- When you lazy load, the data is only retrieved on demand. However this requires an open Hibernate session.
- Retrieve lazy data using
  1. `session.get` and call appropriate getter method(s)
  2. Hibernate query with HQL

### Many-to-Many Mapping

- A course can have many students
- A students can have many courses
- Join Table maintains relationship between course and students
- **Join Table**: A table that provides a mapping between two tables. It has foreign keys for each table to define the mapping relationship.

### Spring-Hibernate-CRUD-APP

- Configuration for Spring + Hibernate
  1. Define database dataSource / connection pool
  2. Setup Hibernate session factory
  3. Setup Hibernate transaction manager
  4. Enable configuration of transactional annotations
- `@Transactional` allows you to minimize or eliminate some of your coding for manually starting and stopping a transaction
- Customer Data Access Object
  - Responsible for interfacing the database
- Customer Data Access Object will have methods such as `saveCustomer()`, `getCustomer()`, `getCustomers()`, `updateCustomer()`, `deleteCustomer()`...
- List Customer step by step
  1. Create Customer.java
  2. Create CustomerDAO.java and CustomerDAOImpl.java
  3. Create CustomerController.java
  4. Create JSP Page: list-customers.jsp
- For Hibernate, our DAO needs a Hibernate SessionFactory
- Customer DAO
  1. Define DAO interface
  2. Define DAO implementation
     - Inject the session factory
- Spring provides the `@Repository` annotation which is applied to DAO implementations
- Service layer sits between Controller and DAO
- Purpose of Service layer
  - **Service Facade** design pattern
  - Intermediate layer for custom business logic
  - Integrate data from multiple sources (DAO/repositories)
- `@Service` applied to Service implementations
  1. Define Service interface
  2. Define Service implementation
  - Inject the CustomerDAO
- Move `@Transactional` to service layer from DAO layer
- In hibernate `session.save()` - insert new record, `session.update()` - update existing record
- `saveOrUpdate()` if (primaryKey/id) empty then _insert_ else _update_

### Aspect Oriented Programming (AOP)

- Two main types of problems
  - Code Tangling
  - Code Scattering
- Programming technique based on concept of an Aspect
- Aspect encapsulates cross-cutting logic
- "Concern" means logic / functionality - it's like infrastructure code
- Aspect can be reused at multiple locations
- Same aspect/class ... applied based on configuration

#### Benefits of AOP

- Code for Aspect is define in a single class
- Business code in you application is cleaner
- Configurable

#### Additional AOP use cases

- Most common
  - logging, security, transactions
- Audit logging
  - who, what, when, where
- Exception handling
  - log exception and notify DevOps team via SMS/email
- API Management
  - how many times has a method been called by user
  - analytics: what are peak times? what is average load? who is top user?

#### AOP Terminology

- **Aspect** : module of code for a cross-cutting concern (logging, security ...)
- **Advice** : What action is taken and when it should be applied
- **Join Point** when to apply code during program execution
- **Pointcut** A predicate expression for where advice should be applied

#### Advice Types

- **Before advice** : run before the method
- **After finally advice** : run after the method (finally)
- **After returning advice** : run after the method (success execution)
- **After throwing advice** : run after method (if exception thrown)
- **Around advice** : run before and after method

#### Weaving

- Connectiog aspects to target objects to create an advised object
- Different types of weaving
  - Compile-time, laod-time or run-time
- Regarding performance: run-time weavin is the slowest

#### AOP Frameworks

- Spring AOP
- AspectJ
- Spring uses run-time weaving of aspects
- `@Before` Advice Run custom code BEFORE the Target Object method call
- `@Before` inject our own custom code before the target call
- `@AfterReturning` inject custom code after the method is processed
- Matching on method-name Pointcut Expression Language
- `execution(modifiers-pattern? return-type-pattern declaring-type-pattern? method-name-pattern(param-pattern) throws-pattern?)`
- The pattern is optional if it has "?"
- Patterns can also make use of wildcards

#### Parameter Pattern Wildcards

- For param-pattern
- () matches a method with no arguments
- (\*) matches a method with one argument of any type
- (..) matches a method with 0 or more arguments of any type
- **Problem**: How can we reuse a pointcut expression?
  - Create a pointcut declaration `@Pointcut(" ") method`
  - Apply pointcut declaration to advice(s)
- **Problem**: How to apply multiple pointcut expressions to single advice?
  - Combine pointcut expresssions using logic operators
  - AND, OR, NOT
- **Problem**: How to control the order of advices being applied?
  - Refactor: Place advices in separate Aspects
  - Control order on Aspects using the `@Order` annotation
- Lower numbers have higher precedence, negative numbers are allowed, order seq. doesn't have to be consecutive.
- What if the aspects ahve the exact same `@Order` annotation. This is undefined.
- **Problem**: When we are in an asepct , How can we access method parameters?
- JoinPoint has metadata about method call

#### AfterReturning Advice

- `@AfterReturning` advice
- Post processing Data (Use Case)
  - Post process the data before returning to caller
  - Format the data or enrich the data
- `returning=paramter_name_to_hold_that_value`
- Calling program will get the modified data, we can use AfterReturning

#### AfterThrowing Advice

- UseCase
  - Notify DevOps team via email or SMS
  - Encapsulate this functionality in AOP aspect for easy reuse
- Need to access the exception object
- `throwing=the_exception_parameter_name`
- If you want to stop the exception propagation
  - then use the `@Around` advice

#### After advice

- `@After` : Runs after a method is completed. Regardless of outcome / exceptions
- Works just like the finally block
- `@After` will execute **before** `@AfterThrowing`
- Code t orun regardless of method outcome. May be cleaning up after the method.
- The After advice doesn't have access to the exception
- Logging/ auditing is the easiest case here

#### Around advice

- Runs before and after method execution
- Use cases
  - Instrumentation / profiling code
    - How long does it take for a section of code to run
- Managing exception
  - Swallow/ handle/stop exception
  - Or can simply rethrow the exception (why?)
- ProceedingJoinPoint - Handle to the target method
- `ProceedingJoinPoint.proceed()` execute the target method

### Maven

- Maven is a Project Management tool
- Most popular use of Maven is for build management and dependencies

| Directory          | Description                                                            |
| ------------------ | ---------------------------------------------------------------------- |
| src/main/java      | Your Java source code                                                  |
| src/main/resources | Properties / config files used by your app                             |
| src/main/webapp    | JSP files and web config files other web assets (images, css, js, etc) |
| src/test           | Unit testing code and properties                                       |
| target             | Destination directory for compiled code.                               |

- POM - Project Object Model file
- There are three levels of POM - Super, Simplest, and Effective POM
- The super POM file defines all the default configurations. Hence, even the simplest form of a POM file will inherit all the configurations defined in the super POM file.
- The simplest POM is the POM that we declare in our Maven project
- Effective POM combines all the default settings from the super POM file and the configuration defined in our application POM.
- `mvn help:effective-pom` to view the effective pom files
- pom.xml structure

  - project metadata: project name, version etc. Output file type: JAR, WAR etc...
  - dependencies: list of projects we depend on Spring, Hibernate, etc..
  - plug ins: Additional custom tasks to run: generate JUnit test reports etc ...

- **Project Coordinates - Elements**

| Name        | Description                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| Group ID    | Name of company, group or organization. Convention is to use reverse domain name : `com.luv2code`           |
| Artifcat ID | Name for this project : `mycoolapp`                                                                         |
| Version     | A specific release version like : 1.0, 1.6, 2.0.. If project is under active development then: 1.0-SNAPSHOT |

#### Maven Archetypes

- Archetypes can be used to create new Maven Projects
- Contains template files for a given Maven project
- Think of it as a collection of "starter files" for a project
  - Java Project, Web-project
- `maven-archetype-quickstart` An archetype to generate a sample maven project
- `maven-archetype-webapp` An archetype to generate a sample maven Webapp project
- `mvn versions:display-dependency-updates`
- `mvn dependency:tree -Dverbose`
- `mvn dependency:tree -Dverbose -DoutputFile=$file`
- `mvn clean install -DskipTests=false/true`. `<skipTests>` can be added in `<properties>`
- `mvn clean install -fae/(fail at end)` | `-ff/ (Fail fast; default)` | `-fn (no fail)`
- `mvn clean compile`
- `mvn clean test-compile` - also compiles source files
- `mvn test`
- `mvn help:effective-settings`
- `mvn help:effective-pom` - entire contents of the pom including the super pom is printed
- `mvn dependency:tree`
- `mvn dependency:sources` - downloads the source code
- `--debug` after any command to run maven in debug mode
- `mvn install:install-file -Dfile=<path-to-file> -DgroupId=<group-id> -DartifactId=<artifact-id> -Dversion=<version> -Dpackaging=<packaging>` : to install third party jar in your local repository. Then add the dependency in the pom file as usual
- `mvn test -T 1C` : to test in parallel

#### Maven Life cycle

- validate : checks if the project is correct and all information is available.
- compile : compiles source code in binary artifacts
- test : executes the test
- package : takes the compiled code and package it.
- integration-test : takes the packaged result and executes additional tests, which required the packaging
- verify : performs checks if the package is valid
- install : install the result of the package phase into the local Maven repository
- deploy : deploys the package to a target, i.e. remote repository

### Gradle

- There are 6 key interface to work with.
- Script : a file ending in .gradle. It is added because of the cross-cutting concerns.
- Project : this is associated with build.gradle file.
- Gradle :
- Settings : to do more with multi-module projects
- Task :
- Action :
- Lifecycle Phases
  - Initialization
  - Configuration
  - Execution
- During the initialization phase gradle determines which projects are gonna take part in the build and creates a project instances for each of these projects. It maps to init.gradle + settings.gradle
- During the configuration phase projects which were created during the initialization phase are configured with the help of build.gradle
- During the execution the build is performed by number of tasks.
- `~/.gradle/wrapper/dists/gradle-6.1-bin/74ow5f78ml5iin9568hvtcqz9/gradle-6.1/init.d` : the path where we added init scripts
- `gradle` object we have access to throughout the build lifecycle
- build.gradle = uses a Script object that implements Script (Interface) + Properties + Methods --> delegates to an object that implements Project (Interface) + Properties + Methods
- Delegate object is different depending on which part of the GBLC we happend to be working in.
- settings.gradle = also has an object that implements Script (Interface) --> delegates to an object that implements Settings (Interface) collection of Project
- init.gradle = Script (Interface) -> delegates to an object that implements the Gradle (interface)
- Project and Settings object has access through a property to the gradle object. That was how we were able to set the timestamps
- wherever we are in BLC we have access to gradle, so that we can set and get properties/
- A project can contain zero or many tasks.
- Configuration phase : Create Tasks, Configure tasks
- Execution phase : Execute tasks
- A task can be made up of zero or many actions.
- doFirst() & doLast() two ways Action or Closure
- Plugins add new tasks
- When we apply plugins a whole lot of tasks are added. If we apply java plugin, clean, compileJava, testClasses, jar are some of the tasks that get added. Other things get added are conventions. One of those convention is project layout.
- wrapper {
  gradleVersion = '4.5'
  }

### Spring Security

- What Spring security can do?
  - User name / password authentication
  - SSO / Okta / LDAP
  - App level Authorization
  - Intra App Authorization like OAuth
  - Microservice security (using tokens, JWT)
  - Method level security
- **Authentication** Answering who you are mostly by providing a proof such as userId/password. This type of authentication is called _Knowledge based authentication_
- There is _Possession based authentication_ : when an app sent you a message like OTP. Other examples includes Key cards and badges.
- **Authorization** : Can this user do what they are trying to do?
- **Principal** : Currently logged in user/account
- **Granted Authority** : to check authorization
- Authorities are fine-grained.
- Whenever a new account is created someone has to assign authorities. This is tedious so there is the concept of **Role**.
- Role is a group of authorities. For ex. role_store_clerk, role_dept_mgr.
- Roles are coarse-grained.
- **Filters** are like cross cutting concerns. They intercept every request and we can do anything with every requests.
- Spring Security by default does following:
  - It adds manadatory authentication for all URLs. It doesn't secure /error
  - It adds a login form
  - Handles login error by providing a form
  - Creates a user and sets a default password.
- Authenitcation manager is what does the authentication. To do authentication we affect it using AuthenticationManagerBuilder.
- To do this we extend WebSecurityConfigurerAdapter and override the `configure(AMB)` method
- Spring Security enforces that we save our passwords in encoded format. To do this we expose a `@Bean` of type `PasswordEncoder`.
- HttpSecurity lets you configure what are the paths and what are the access restrictions for those paths.
- DelegatingFilterProxy delegates to internal filters e.g authentication filter, authorization filter
- `Authentication` class is holder of Credentials before authentication and Principal after successful authentication.
- Providers such as `AuthenticationProvider` does the actual authentication. It has a method `authenticate()` and `supports()` return true if this AuthenticationProvider supports the Authentication
- A single app can have multiple authentication strategies, username/password, OAuth, LDAP and correspondingly there are multiple authentication providers.
- In order for these providers to work with each other there's a special type called `AuthenticationManager`. ProviderManager implements it.
- AuthenticationProvider need to look up Identity Store. Every type of Provider needs to do this.
- The retrieving of user information is extracted by Spring Security in `UserDetailsService`, which loads user by username(). We create this service.
- `Authentication` object is saved in `ThreadLocal`.
- There is another Filter which manages user's session. It takes authenticated Principal and associates it with user's session. That's why we don't authenticate every time.

- Spring security model
  - Spring security defines a framework for security
  - Implemented using Servlet filters in the background
  - Two methods of securing a Web app: declarative and programmatic
- Servlet filters are used to pre-process / post-process web requests
- Servlet filters can route web requests based on security logic
- Programmatic Security
  - Spring Security provides an API for custom application coding
  - Provides greater customization for specific app requirements (we can add company specific business logic)
- Different Login Methods
  - HTTP Basic Authentication
  - Default login form
  - Custom login form
- Create Spring Security Initializer. Register spring-security-filters
- The "Context Root" is the root path for your web application. Same as "Context Path"

| Method                                    | Description                                                                        |
| ----------------------------------------- | ---------------------------------------------------------------------------------- |
| `configure(AuthenticationManagerBuilder)` | Configure users (in memory, database, ldap, etc) _Authentication_                  |
| `configure(HttpSecurity)`                 | Configure security of web paths in application, login, logout etc. _Authorization_ |

#### CSRF

- A security attack where an evil website tricks you into executing an action on a web application that you are currently logged in.
- Embed additonal authentication data/token into all HTML forms So that, on subsequent requests, web app will verify token before processing.
- Filters generate tokens and send to browser, which when sent back is verified by Spring security filters.
- Spring Security uses the Synchronizer Token Pattern
- Spring Security provides JSP custom tags for accessing user id and roles
- Showing content based on roles: `security:authorize access="hasRole('MANAGER)` will only show content to users whose role is manager

#### Spring Security Database

- Spring Security can read user account info from database
- By default, you have to follow Spring Security's predefined table schemas
- Dev Process
  1. Develop SQL Script to set up database tables
  2. Add database support to Maven POM file
  3. Create JDBC properties file
  4. Define DataSource in Spring Configuration
  5. Update Spring Security Configuration
- Spring Security have a default predefined schema
- users and authorities table names and columns
- Password are stored in db using a specific format: `{id}encodedPassword`

| ID     | Description          |
| ------ | -------------------- |
| noop   | Plain text passwords |
| bcrypt | BCrypt password      |

- `@PropertySource` will read the props file from the given class path
- Environment class will hold data that is read from the properties file

#### Spring Security JWT

- **Step 1**
  - A /authenticate API endpoint
    - Accepts user Id and password
    - Returns JWT as response
  - We assume client holds on to JWT and sends in subsequent request header.
  - We need to examine incoming request and get JWT out of it and verify it's valid , if its valid we need to make sure that that is in the security context of the logged in user.
- **Step 2**
  - Intercept all incoming requests
    - Extract JWT from the header
    - Validate and set in exeuction context
- To intercept req. and read header we need filter.

### OAuth

- It is used for authorization
- **Resource**/**Protected Resource** : what is being sought by all the actors in OAuth flow. e.g. photos on google drive
- **Resource owner** : person who has access to the _resource_. Entity who can grant access to a protected resource
- **Resource Server** : the server hosting the protected resource.
- **Client** : an application making protected resource request on behalf of the resource owner and with its authorization
- **Authorization Server** : The server issuing access tokens to the client
- 2rfc0
  [puytyc]

### Spring REST

- Build a client app that provides the weather report for a city
- JAX-RS is a specification for implementing REST web services in Java, currently defined by the JSR-370.
- [Jersey](https://eclipse-ee4j.github.io/jersey/) (shipped with GlassFish and Payara) is the JAX-RS reference implementation, however there are other implementations such as RESTEasy (shipped with JBoss EAP and WildFly) and Apache CXF (shipped with TomEE and WebSphere).
- However, in Spring Framework The REST capabilities are provided by the Spring MVC module (same module that provides model-view-controller capabilities). It is not a JAX-RS implementation and can be seen as a Spring alternative to the JAX-RS standard.

#### JSON

- Lightweight data format for storing and exchanging data
- Data binding is the process of converting JSON data to a Java POJO or convert the other way.
- Also known as serialization/deserialization, marshalling/unmarshalling, mapping
- Spring uses **Jackson Project** that handles data binding between JSON and Java POJO. Supports XML also
- Convert JSON to Java POJO ... call setter methods on POJO
- Convert Java POJO to JSON ... call getter methods on POJO

#### REST HTTP basics

- Leverage HTTP methods for CRUD operations
- POST Create a new entity
- GET Read a list of entities or single entity
- PUT Update an existing entity
- DELETE Delete an existing entity
- The message format is described by MIME Content types
- Multipurpose Internet Mail-Extension
- Basic Syntax: type/sub-type
- Examples
  - text/html, text/plain
  - for REST application/json, application/xml...
- `@RestController` Handles REST requests and responses
- Spring will automatically handle Jackson integration as long as the Jackson project is in the pom
- Jackson will convert `List<Student>` to JSON array
- Handle the exception and return error as JSON { status, message, timestamp}
- We need global exception handlers
  - Promotes reuse
  - Centralizes exception handling
- `@ControllerAdvice` is similar to an interceptor / filter (Real time use of AOP example)
  - Pre-process requests to controllers
  - Post-process responses to handle exceptions
  - Perfect for global exception handling

#### REST API Design

1. Review API requirements
2. Identify main resource / entity
   - look for the most prominent "noun"
   - Convention is to use plural form of resource / entity : /customers
3. Use HTTP methods to assign action on resource

- Don't include actions or verbs in the endpoint
- For null objects Jackson return empty body
- `@RequestBody` annotation binds the POJO to a method parameter
- We overwrite the id with 0, to effectively set it to null/empty, then our backend DAO code will "INSERT" new customer
- For controller to process JSON data, need to set a HTTP request header
  - Content-type: application/json
- Need to configure REST client to send the correct HTTP request header

### SpringBoot

- JAR file includes application code and includes the server
- Spring Boot apps can run standalone (includes embedded server)
- Spring Boot apps can also be deployed in the traditional way
- Deploy WAR file to an external server ... Tomcat, JBoss, WebSphere etc...
- Minimize the amount of manual configuration
- Help to resolve dependency conflicts

#### Spring Boot Directory Structure

- Maven wrapper files
- `mvnw` allows you to run a Maven project
  - No need to have Maven installed or present on your path
  - If correct version of Maven is NOT found on computer **automatically downloads** correct version and runs Maven
- `./mvnw clean compile test`
- If installed maven can delete maven wrapper files
- `mvn package` to package the application
- `mvn spring-boot:run` to run the application
- `@SpringBootApplication` Enables autoconfigure, component scanning, other configuration. It is composed of multiple annotation:
- `@EnableAutoConfiguration`, `@ComponentScan`, `@Configuration`
- `SpringApplication` bootstrap you spring boot application
- Place your main application class in the root package above your other packages.
- Explicitly list base packages to scan if we have other packages
- By default. Spring Boot will load poperites from :`application.properties`
- Read data from : application.properties `@Value"${coach.name}"`
- SpringBoot will load static resources from "/static" directory. Examples of static resources HTML files, CSS, JavaScript, Images etc.
- Do not use the src/main/webapp directory if application pacakged as jar. Only works with WAR packaging
- Spring Boot includes auto-configuration for following template engines. Freemarker, Thymeleaf
- Spring Booot provides a "Starter Parent"
- This is a special starter that provides Maven defaults
- Maven defaults defined in the Starter Parent: Java version, UTF encoding...
- To override a default, set as a property
- Inherit version from starter parent

#### Spring Boot Dev Tools

- **Problem** When running Spring Boot applications
  - If you make changes to source code, need to manually restart the application
- **Solution** spring-boot-devtools, just add a dependency to pom
- devtools-docs

#### Actuator

- **Problem** How can I monitor and manage my application?, How can I check the application health?, How can I access application metrics?
- **Solution** Spring Boot Actuator
  - Exposes endpoints to monitor and manage application
  - Simply add the dependency
  - Automatically exposes endpoints for metrics out-of-the-box
  - Endpoints are prefixed with /actuator. Some examples /health : health info, /info: info about project. These new REST endpoints are free
- `/health` checks the status of your app. Normally used by monitoring apps.
- luv2code.com/actuator-endpoints
- To expose all actuator endpoints over HTTP
  - `management.endpoints.web.exposure.include=*` use wildcard "\*" in application.properties
- `/threaddump` list of all threads running in the application. usefult for analyzing and profiling application's performance
- `/mappings` list all the request mappings
- `/beans` list all the beans

#### Spring Boot Properties

- Spring Boot can be configured in the application.properties file
- Server port, context path, actuator, security etc...
- The properties are roughly grouped into the following categories:
  - Core, Web, Security, Data, Actuator, Integration, DevTools, Testing

#### Spring Boot CRUD

- Spring Boot will automatically configure your data source for you
- Automatically create beans for `DataSource` and `EntityManager`
- EntityManager is from JPA
- JPA: standard API for ORM
- Only a specfication
  - Defines a set of interfaces
  - Requires an implementation to be usable
- `EntityManager` is similar to Hibernate `SessionFactory`
- `EntityManager` can serve as a wrapper for a Hibernate `Session` object
- We can inject `EntityManager` into our DAO
- Various DAO Techniques
  1. Use `EntityManager` but leverage native Hibernate API
  2. Use `EntityManager` and standard JPA API
  3. Spring Data JPA
- JPA also supports a query language: JPQL

| Action                    | Native Hibernate method | JPA Method                  |
| ------------------------- | ----------------------- | --------------------------- |
| Create/save new entity    | session.save()          | entityManager.persist()     |
| Retrieve entity by id     | session.get()/load()    | entityManager.find()        |
| Retrieve list of entities | session.createQuery()   | entityManager.createQuery() |
| Save or update entity     | session.saveOrUpdate()  | entityManager.merge()       |
| Delete entity             | session.delete()        | entityManager.remove()      |

#### Spring Data JPA

- **Problem**
  - Most of the code is same in creating DAO
  - Create a DAO for me, plug in my entity type and primary key, give me all of the basic CRUD features for free
- **Soln** Spring Data JPA
  - Create a DAO and just plug in your entity type and primary key
  - Spring will give you a CRUD implementation for FREE
- Spring Data JPA provides the interface **JpaRepository**
- Exposes methods (some by inheritance from Parents)
- Dev Process
  1. Extend JpaRepository interface
  2. Use your Repository in your app.
- Extend and adding custom queries with JPQL
- Query Domain Specific Language (Query DSL)
- Defining custom methods (low-level coding)
- `Optional` to see if a given value is present
- JPA repository makes use of Optional

#### Spring Data REST

- The Problem
  - Need to create REST API for another entity? Customer, Student, Product, Book
  - Do we have to repeat all of the same code again and again?
- Create a Rest API for me, Use my existing JpaRepostiory (entity, primary key), Give me all of the basic REST API CRUD features for free
- Spring Data REST is the solution
- Spring Data REST will create endpoints based on the entity type
- spring-boot-starter-data-rest
- You need 3 items
  1. Entity
  2. JpaRepository
  3. Maven POM dependency
- Spring Data REST endpoinst are HATEOAS compliant
- HATEOAS: Hypermedia as the Engine of Application State
- Hypermedia-driven sites provide information to access REST interfaces
- Think of it as meta-data for REST data
- Hypertext Application Language
- Spring Data REST advance features
  - Pagination, sorting and searching
- Spring Data REST only uses ID on the URL

#### Spring Data REST Configuration, Pagination, and Sorting

- By default, Spring Data REST will create endpoints based on entity type. Simple pluralized form: adds 's' to the entity type
- Specify plural name / path with an annotation `@RepositoryRestResource(path="")`
- By default, Spring Data REST will return the first 20 elements

### Thymeleaf

- Java templating engine
- Commonly used to generate the HTML views for web apps
- Can use it outside of web apps
- What is a Thymelead template
  - Can be an HTML page with some Thymeleaf expressions
  - Include dynamic content from Thymeleaf expressions
- In a web app. Thymeleaf is processed on the server
- Results included in HTML returned to browser
- Non-web use cases
- Email Template
  - When student signs up for a course then send welcome mail
- CSV Template
  - Generate a monthly report as CSV then upload to Google Drive
- PDF Template
  - Generate a travel confirmaton PDF then send via email
- `@` symbol : Reference context path for your application
- Spring Boot will search following directories for static resources
  1. /META-INF/resources
  2. /resources
  3. /static
  4. /public

### Spring in 100 Steps

- What are the objects it needs to manage and what are its dependencies?
- `@Component` : you need to start managing instances of the class with this annotation
- `@Autowired` : spring starts looking for dependency among the instances it manages.
- Beans: the different objects that are managed by the spring framework
- Autowiring: the process where spring identifies the dependencies, identifies the matches for the dependencies in the instances and populates them
- Autowired can be done in two ways when there are multiple implementation.
  1. `@Primary`
  2. Use the explicit class name.
- `@Primary` has higher precedence than using the name of the class.
- `@Qualifier` assign name to each candidate of implementaion. and use the one when autowiring.
- `@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)` for making a bean as prototype
- What would happen if the dependency of a singleton bean is a prototype bean?
  - Not getting different instances of Prototype bean unless we configure a proxy
- CDI - it is like an interface that defines how to do dependency injection. It provides annotations that needs to be used with DI.
  - `@Inject` (`@Autowired`)
  - `@Named` (`@Component` & `@Qualifier`)
  - `@Singleton` ( Defines a scope of Singleton)

### Spring Web-Application

- JSP - Java Server Pages. It also converts to the servlet.
- In JSP we can write Java code. The way we do something like that is using scriplets.
- Anything written between <% %> is able to run java code. It is inadvisable though.

```java
<%
System.out.println(request.getParameter("name"));
Date date = new Date();
%>
<!-- Scriplet expression -->
<div>Current date is <%=date%></div> -->
```

- Controllers are called **handlers**
- `@ResponseBody` whatever is returned to the dispatcher servlet it is sent back to browser as response.
- There are 5 important levels in log4j
  - TRACE: prints everything
  - DEBUG
  - INFO: start printing information
  - WARN: only prints warning
  - ERROR : only when there is error, then something would be in the log
- Whatever you want to make available to the view put it in the model. That model will be available to the view.
- AutoConfiguration :
- `spring.jpa.hibernate.ddl-auto=none` it doesn't make any changes to the database tables.
- Hibernate to directly create schema: `spring.jpa.hibernate.ddl-auto=create`

### Spring in Action

- Spring data understands find, read, and get as synonymous for fetching one or more entities.
- Spring Security offers several options for configuring a user store, including these:
  - An in-memory user store
  - A JDBC based user store
  - An LDAP backed user store
  - A custom user details service
- The user store can be configured by overriding a `configure()` method in `WebSecurityConfigurerAdapter`
- Most common ways to determine who the user is:
  - Inject a `Principal` object into the controller method.
  - Inject an `Authentication` object into the controller method.
  - Use `SecurityContextHolder` to get at the security context.
  - Use an `@AuthenticationPrincipal` annotated method.
- To start using a MySQL db, add the following configuration properties to application.yml

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost/tacocloud
    username: tacoddb
    password: tacopassword
    driver-class-name: com.mysql.jdbc.Driver (optional, in case spring can't figure out)
```

- to specify the database initialization scripts to run when the application starts the `spring.datasource.schema` and `spring.datasource.data` properties are useful.

```yaml
spring:
  datasource:
    schema:
      - order-schema.sql
      - ingredient-schema.sql
      - taco-schema.sql
      - user-schema.sql
    data:
      - ingredients.sql
```

- To set up embedded server for handling HTTPS requests the first thing to do is create a keystore using the jdk's keytool :
- `keytool -keystore mykeys.jks -genkey -alias tomcat -keyalg RSA`
- Then set up few properties in app.yml that look like this:

```yaml
server:
  port: 8443
  ssl:
    key-store: file:///path/to/mykeys.jks
    key-store-password: letmein
    key-password: letmein
```

- For full control over the logging configuration, we can create a `logback.xml` file in `src/main/resources`, Sample example

```xml
<configuration>
<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
  <encoder>
    <pattern>
      %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
    </pattern>
  </encoder>
</appender>
<logger name="root" level="INFO"/>
<root level="INFO">
  <appender-ref ref="STDOUT" />
</root>
</configuration>
```

- For documentation : [logback](http://logback.qos.ch)

```yaml
logging:
  path: /var/logs/
  file: TacoCloud.log
  level:
    root: WARN
    org:
      springframework:
        security: DEBUG
```

- To support property injection of configuration properties, Spring Boot provides the `@ConfigurationProperties` annotation.
- When placed on any Spring bean, it specifies that the properties of that bean can be injected from properties in the Spring environment.
- `@ConfigurationProperties` are often placed on beans whose sole purpose in the application is to be holders of configuration data.
- `Pageable` is Spring Data's way of selecting some subset of the results by a page number and page size.
- Profiles are a type of conditional configuration where different beans, configuration classes, and configuration properties are applied or ignored based on what profiles are active at runtime.
- application-{profile name}.yml
- All it takes to make a profile active is to include it in the list of profile names given to the `spring.profiles.active` property. However, it is recommended that set spring profile like this: `export SPRING_PROFILES_ACTIVE=prod`
- `@Profile` annotation can designate beans as only being applicable to a given profile.
- PagingAndSortingRepository
- _Hypermedia as the Engine of Application State_, or HATEOAS, is a means for creating self-describing APIs wherein resources returned from an API contain links to related resources.
- HAL [Hypertext Application Language](http://stateless.co/hal_specification.html) a simple and commonly used format for embedding hyperlinks in JSON responses.
- By annotating `TacoResource` with `@Relation`, you can specify how Spring HATEOAS should name the field in the resulting JSON.
- `HttpMessageConvertersAutoConfiguration` : Converts from bean to json. Jackson2Object
- `ErrorMvcAutoConfiguration` : configures the error page. Configures the default error page
- `DispatcherServletAutoConfiguration` : configures the dispatcher servlet
- SpringBoot autoconfiguration is configuring dispatcher servlet.
- Dispatcher Servlet is the front-controller.
- `ResponseEntityExceptionHandler` : to handle exceptions to provide customized handling
- I18n : Internationalizatoin
- We need to Configure
  - LocaleResolver : Default Locale - Locale.US
  - ResourceBundleMessageSource : list of properties that will be internationalized. we need to store those here.
- Versioning
  - Media type versioning (a.k.a "content negotiation" or "accept header") : GitHub
  - (Custom) headers versioning : Microsoft
  - URI versioning : Twitter
  - Request Parameter versioning : Amazon
- Factors to consider for versioning:
  - URI Pollution : 3 & 4
  - Misuse of HTTP Headers : 1 & 2
  - Caching : !(1&2), 3 & 4
  - Can we execute the request from browser : !(1 & 2) but (3 & 4 ) easily be able to do
  - How easy it is to generate API documentation
  - There's no perfect solution. Before you build your API have a versioning strategy and test them.
- Richardson Maturity Model : It is a model to gauge how RESTful are your web services. It defines three levels of restful services
  - LVL 0 : Expose SOAP web services in rest style : http://server/getPosts, http://server/deletePosts (action)
  - LVL 1 : Expose resources with proper URI : http://server/accounts, http://server/accounts/10 (not using proper HTTP methods)
  - LVL 2 : Level 1 + proper use of HTTP methods
  - LVL 3 : Level 2 + HATEOAS (Data + Next Possible Actions)
- Prefer plurals. Use nouns for resources

### Microservices

- Challenges of microservices:
  - how to decide what you should do and not do in a microservice. the challenge of bounded context
  - configuration management : another challenge
  - dynamic scale up and scale down and also how to distribute the load
  - visibility : into what's happening with these microservices. if they are down, disk space is full. needs to be automated
  - how to prevent one microservice being down so that it doesn't take the whole application down.
- Spring Cloud config server : for storing configuration
- Dynamic scale up and down :
  - Naming Server (Eureka) : All the instances of all microservices will register with the naming server. Other feature is Service Discovery
  - Ribbon (Client Side Load balancing)
  - Feign (Easier REST Clients) : makes it easy to call other microservices, rest services. It provides integration with Ribbon.
- Visibility and Monitoring:
  - Zipking Distributed Tracing : to trace a request across multiple components
  - Netflix Zuul API Gateway : provides solution to common services required by microservices e.g. logging, security, analytics
- Fault tolerance
  - Hystrix : helps to configure a default response if a service is down
- `SpringCloudConfigServer` picks up the configuration from a git repo.
- Just like we use a repostiory to talk to JPA, we need to create a feign proxy to talk to external microservices.
- Whenever a new instance of a micro service comes up it registers itself with `EurekaNamingServer`. This is called service registration. Whenever they want details of a service it is called service discovery.
- API Gateways provide common feature
  - Authentication, authorization and security
  - Rate limits
  - Fault tolerance
  - Service Aggregation : external consumer who want to call 15 services, it's better to aggregate those 15 calls and send together.
- http://localhost:8765/{application-name}/{uri} : for passing a request through zuul api gateway
- http://localhost:8765/currency-exchange-service/currency-exchange/from/EUR/to/INR
- http://localhost:8765/currency-conversion-service/currency-converter-feign/from/USD/to/INR/quantity/10
- Distributed Tracing:
  - One place where I can go and see what happened to a specific request. A centralized component.
  - One of the thing is to identify a request by assigning a unique id to a request.
  - Zipkin is a distributed tracing system
  - What we do is all the the log from all the services put it into a queue (Rabbit MQ), and send it to the zipkin server, where it is consolidated.
  - Spring Cloud Sleuth adds a unique ID to a request so that we can trace it across multiple components. However it is distributed through multiple console, that is why we need Zipkin to centralize it.
- Wide variety of solutions for centralized logging:
  - like the ELK stack - where all the log from micro services is consolidated.
- All the microservices will be putting the logging message on RabbitMQ and ZipkinDistributedTracingServer will pick it from there
- RABBIT_URI=amqp://localhost java -jar zipkin.jar :
- http://localhost:8080/application/refresh : to pick up new values from the properties file
- Spring Cloud Bus : provides a url, and once we hit the url, all the instance of the microservices will be updated with the latest configuration. (wasn't able to test locally)
- **Fault Tolerance** : Given an application, if there is a fault; what is the impact of the fault. Part of the functionality goes down or whole application goes down.
- **Resilience** : How many faults a system can tolerate. How much a system can bounce back from fault.
- Issues with microservices
  - A microservice instance goes down : we want to run mutltiple instances so if one goes down another takes its place. Thanks to service discovery we don't have to do a lot.
  - A microservice instance is slow. Other paths independent of the slow microservice can still become slow because of Threads piling up. To solve this problem we can use **Timeout**. But it solves the problem as long as request are coming in at less rate than timeout. If the requests are coming at a faster rate we will be facing the same issue.
  - Other way could be having separate threadpool for A and separate thread pool for B. So that if B is slow it continues to be slow and A works as it is. This is a bulkhead pattern.
- Circuit breaker pattern
  - Detect something is wrong.
  - Take temporary steps to avoid the situation
  - Deactivate the "problem" component so that it doesn't affect downstream components.
- We need to add a fallback when circuit breaks.
  - Throw an error. (not a good idea)
  - Return a fallback "default" response
  - Save previous responses (cache) and use that when possible.
- Config as a service
  - Apache Zookeeper
  - ETCD - distributed key-value store
  - Hashicorp Consul
  - Spring Cloud Configuration Server
- Configuration Strategies:
  - Spring Cloud Config Server has the ability to encrypt and decrypt and save in git repo

### Building a RESTful Web Service

- The Spring Boot Plugin has the following goals:
  - `spring-boot:run` runs your Spring Boot application.
  - `spring-boot:repackage` repackages your jar/war to be executable.
  - `spring-boot:start` and `spring-boot:stop` to manage the lifecycle of your Spring Boot application (i.e. for integration tests).
- `spring-boot:build-info` generates build information that can be used by the Actuator.
- Resource representation class - technical name to model
- In Spring's approach to building RESTful web services, HTTP requests are handled by controller. These components are easily identified by the `@RestController` annotation.
- In MVC controller to perform the server-side rendering we rely on a view technology such as Thymeleaf or Velocity. However, the RESTful web service controller simply populates and returns the object. The object data will be written directly to the HTTP response as JSON
- `@SpringBootApplication` adds all of the following:
- `@Configuration` tags the class as a source of bean definitions for the application context.
- `@EnableAutoConfiguration` tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.
- `@ComponentScan` tells Spring to look for other components, configurations, and services in the hello package, allowing it to find the controllers.

### Consuming a Rest Service

- `RestTemplate` makes interacting with most RESTful services a one-line incantation. And it can even bind that data to custom domain types.
- A class annotated with `@JsonIgnoreProperties` from the Jackson JSON processing library indicates that any properties not bound in the class should be ignored.
- In order for you to directly bind your data to your custom types, you need to specify the variable name exact same as the key in the JSON Document returned from the API. In case your variable name and key in JSON doc are not matching, you need to use `@JsonProperty` annotation to specify the exact key of JSON document.
- `RestTemplate` also supports other HTTP verbs such as POST, PUT, and DELETE.

### Scheduling Tasks

- The `Scheduled` annotation defines when a particular method runs. NOTE: This example uses `fixedRate`, which specifies the interval between method invocations measured from the start time of each invocation. There are other options, like `fixedDelay`, which specifies the interval between invocations measured from the completion of the task. You can also use `@Scheduled(cron=". . .")` [expressions for more sophisticated task scheduling.](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/scheduling/support/CronSequenceGenerator.html)
- `@EnableScheduling` ensures that a background task executor is created. Without it, nothing gets scheduled.

### Accessing Relational Data using JDBC

- Spring provides a template class called `JdbcTemplate` that makes it easy to work with SQL relational databases and JDBC
- When the `Application` class implements Spring Boot's `CommandLineRunner`, which means it will execute the `run()` method after the application context is loaded up.
- For single insert statements, `JdbcTemplate’s insert` method is good. But for multiple inserts, it’s better to use `batchUpdate`.
- you use the `query` method to search your table for records matching the criteria

### Uploading Files

- As part of auto-configuring Spring MVC, Spring Boot will create a `MultipartConfigElement` bean and make itself ready for file uploads.
- Class is annotated with `@Controller` so Spring MVC can pick it up and look for routes.
- `spring.http.multipart.max-file-size` is set to 128KB, meaning total file size cannot exceed 128KB.
- `spring.http.multipart.max-request-size` is set to 128KB, meaning total request size for a multipart/form-data cannot exceed 128KB.

### Messaging with RabbitMQ

- Spring AMQP’s `RabbitTemplate` provides everything you need to send and receive messages with RabbitMQ. Specifically, you need to configure:
  - A message listener container
  - Declare the queue, the exchange, and the binding between them
  - A component to send some messages to test the listener
- You’ll use `RabbitTemplate` to send messages, and you will register a `Receiver` with the message listener container to receive messages. The connection factory drives both, allowing them to connect to the RabbitMQ server.
- The bean defined in the `listenerAdapter()` method is registered as a message listener in the container defined in `container()`. It will listen for messages on the "spring-boot" queue. Because the `Receiver` class is a POJO, it needs to be wrapped in the `MessageListenerAdapter`, where you specify it to invoke receiveMessage.
- JMS queues and AMQP queues have different semantics. For example, JMS sends queued messages to only one consumer. While AMQP queues do the same thing, AMQP producers don’t send messages directly to queues. Instead, a message is sent to an exchange, which can go to a single queue, or fanout to multiple queues, emulating the concept of JMS topics

### Building REST service

- `@Data` is a Lombok annotation to create all the getters, setters, equals, hash, and toString methods,
- `@Entity` is a JPA annotation to make an object ready for storage in a JPA-based data store.
- Spring Data repositories are interfaces with methods supporting reading, updating, deleting, and creating records against a back end data store
- Spring Boot will run ALL `CommandLineRunner` beans once the application context is loaded
- `@RestController` indicates that the data returned by each method will be written straight into the response body instead of rendering a template
- `@ResponseBody` signals that this advice is rendered straight into the response body.
- `@ExceptionHandler` configures the advice to only respond if an `EmployeeNotFoundException` is thrown
- `@ResponseStatus` says to issue an `HttpStatus.NOT_FOUND`, i.e. an HTTP 404
- `curl -v localhost:8080/employees`
- `curl -X POST localhost:8080/employees -H 'Content-type:application/json' -d '{"name": "Samwise Gamgee", "role": "gardener"}'`
- `curl -X PUT localhost:8080/employees/3 -H 'Content-type:application/json' -d '{"name": "Samwise Gamgee", "role": "ring bearer"}'`
- `curl -X DELETE localhost:8080/employees/3`
- `Resource<T>` is a generic container from Spring HATEOAS that includes not only the data but a collection of links.
- `linkTo(methodOn(EmployeeController.class).one(id)).withSelfRel()` asks that Spring HATEOAS build a link to the `EmployeeController` 's `one()` method, and flag it as a self link.
- `linkTo(methodOn(EmployeeController.class).all()).withRel("employees")` asks Spring HATEOAS to build a link to the aggregate root, `all()`, and call it "employees".
- What do we mean by "build a link"? One of Spring HATEOAS’s core types is `Link`. It includes a URI and a rel (relation).
- HAL is a lightweight mediatype that allows encoding not just data but also hypermedia controls, alerting consumers to other parts of the API they can navigate toward
- `Resources<>` is another Spring HATEOAS container aimed at encapsulating collections. It, too, also lets you include links.

### Caching Data

- The `@EnableCaching` annotation triggers a post processor that inspects every Spring bean for the presence of caching annotations on public methods. If such an annotation is found, a proxy is automatically created to intercept the method call and handle the caching behavior accordingly
- The annotations that are managed by this post processor are `Cacheable`, `CachePut` and `CacheEvict`.
- Spring Boot automatically configures a suitable `CacheManager` to serve as a provider for the relevant cache.

### Creating Asynchronous methods

- One approach to scaling services is to run expensive jobs in the background and wait for the results using Java’s `CompletableFuture` interface
- A method flagged with Spring's `@Async` annotation, indicates it will run on a separate thread.
- The method's return type should be `CompletableFuture<>`
- The `@EnableAsync` annotation switches on Spring’s ability to run `@Async` methods in a background thread pool
- With the help of `allOf` factory method we create an array of `CompletableFutures` on which by calling the `join` method makes it possible to wait for the completion of all of the `CompletableFutures`.

### Handling Form Submission

- method uses a Model object to expose a new Greeting to the view template
- The `th:action="@{/greeting}"` expression directs the form to POST to the `/greeting` endpoint, while the `th:object="${greeting}"` expression declares the model object to use for collecting the form data. The two form fields, expressed with `th:field="{id}"` and `th:field="{content}"`, correspond to the fields in the `Greeting` object above.

### TLS in SpringBoot

- TLS can be implemented either one-way or two-way.
- **One-Way TLS** : In one-way TLS, only the client verifies the server to ensure that it receives data from the trusted server. For implementing one-way TLS, the server shares its public certificate with clients
- **Two-Way TLS** : In two-way TLS or Mutual TLS (mTLS), both the client and server authenticate each other to ensure that both parties involved in the communication are trusted. For implementing mTLS, both parties share their public certificates with each other.
- `keytool -genkeypair -alias baeldung -keyalg RSA -keysize 4096 -validity 3650 -dname "CN=localhost" -keypass changeit -keystore keystore.p12 -storeType PKCS12 -storepass changeit`
  The _keystore_ file can be in different formats. The two most popular ones are Java KeyStore (JKS) and PKCS#12. JKS is specific to Java, while PKCS#12 is an industry-standard format belonging to the family of standards defined under Public Key Cryptography Standards (PKCS)

### Spring Webflux backpressure

- Backpressure in software systems is the capability to overload the traffic communication. In other words, emitters of information overwhelm consumers with data they are not able to process.
- In Reactive Streams, backpressure also defines how to regulate the transmission of stream elements. In other words, control how many elements the recipient can consume.
- Some backpressure strategies
  - Controlling the data stream sent would be the first option
  - Buffering the extra amount of data is the second choice
  - Dropping the extra events losing track of them.
- Controlling the events emitted by the publisher
  - Send new events only when the subscriber requests them
  - Limiting the number of events to receive at the client-side
  - Canceling the data streaming when the consumer cannot process more events

### Transactions

- Spring Boot implicitly creates a proxy for the transaction annotated methods. So for such methods, the proxy acts like a wrapper, which takes care of creating a transaction at the beginning of the method call and committing the transaction after the method is executed.
- Transaction propagation indicates if any component or service will or will not participate in a transaction and how will it **behave if the calling component/service already has or does not have a transaction created already**
- There are six types of Transaction Propagations

| Propagation   | Behaviour                                                                                                                                                        |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| REQUIRED      | **Always executes in a transaction**. If there is an existing transaction, it uses it. If none exists, then only a new one is created.                           |
| SUPPORTS      | **It may or may not run in a transaction**. If the current transaction exists, then it is supported. If none exists, then it gets executed without a transaction |
| NOT_SUPPORTED | **Always executes without a transaction**. If there is an existing transaction, it gets suspended.                                                               |
| REQUIRES_NEW  | **Always executes in a new transaction**. If there is an existing transaction, it gets suspended.                                                                |
| NEVER         | **Always executes w/o any transaction**. It throws an exception if there is an existing transaction.                                                             |
| MANDATORY     | **Always executes in a transaction**. If there is an existing transaction, it is used. If there is no existing transaction, it will throw an exception.          |

- In case of **checked exceptions the previously executed transactions do not get rolled back automatically even if we have used transaction annotation**. We need to inform the application how to handle roll back in event of checked exception. This is achieved using the `RollbackFor` annotation.

### Transaction Isolation

- select @@transaction_isolation; // show existing transaction isolation level if mysql version >= 8
- set session transaction isolation level serializable; // set transaction isolation level to serializable.
- The following are the types of Transaction Isolation Levels :
  - **SERIALIZABLE** : If two transactions are executing concurrently then it is as if the transactions get executed serially i.e the first transaction gets committed only then the second transaction gets executed. This is **total isolation**. So a running transaction is never affected by other transactions. However this may cause issues as **performance will be low and deadlock might occur**
  - **REPEATABLE_READ** : If two transactions are executing concurrently - **till the first transaction is committed the existing records cannot be changed by second transaction but new records can be added**. After the second transaction is committed, the new added records get reflected in first transaction which is still not committed. For MySQL the default isolation level is REPEATABLE_READ
  - **READ_COMMITTED** : If two transactions are executing concurrently - **before the first transaction is committed the existing records can be changed as well as new records can be changed by second transaction**. After the second transaction is committed, the newly added and also updated records get reflected in first transaction which is still not committed
  - **READ_UNCOMMITTED** : If two transactions are executing concurrently - before the first transaction is committed the existing records can be changed as well as new records can be changed by second transaction. **Even if the second transaction is not committed the newly added and also updated records get reflected** in first transaction which is still not committed.

### StackOverflow

- How to load initial data:
  - Though normally you shouldn't have to do this since Spring boot already configures Hibernate to create your schema based on your entities for an in memory database.
  - If you really want to use schema.sql you'll have to disable this feature by adding this to your application.properties: `spring.jpa.hibernate.ddl-auto=none`
  - If you're using **Spring boot 2**, database initialization only works for embedded databases (H2, HSQLDB, ...). If you want to use it for other databases as well, you need to change the spring.datasource.initialization-mode property: `spring.datasource.initialization-mode=always`
  - If you're using multiple database vendors, you can name your file **data-h2.sql** or **data-mysql.sql** depending on which database platform you want to use.
  - To make that work, you'll have to configure the spring.datasource.platform property though: `spring.datasource.platform=h2`
  - You can add a spring.datasource.data property to application.properties listing the sql files you want to run. Like this: `spring.datasource.data=classpath:accounts.sql, classpath:books.sql, classpath:reviews.sql`
  - If you want to run DDL type SQL then use: `spring.datasource.schema=classpath:create_account_table.sql`
- It's worth looking at frameworks : [flywaydb](https://flywaydb.org/) or [liquibase](https://www.liquibase.org/)

### Spring for Architects

- Distributed Systems have similar needs
  - Monitoring. Circuit breakers. Consumer Driven Contracts
  - Gateways. Streams. Externalized configuration
  - Functions. Service Discovery. Load balancing. Documentation
- Four golden signals
  - Latency : how long does it take to service a request
  - Traffic : level of demand on the system. Request/second. I/O rate
  - Errors : failed requests. Can be explicit, implicit or policy failure
  - Saturation : how much of a constrained resource is left.
- What could cause a spike in demand? How does that translate to specific services?
- Circuit breaker watches the calls. Once they exceed a failure threshold, the circuit is opened. Redirects to the fallback mechanism. Periodically checks to see if the service is repaired. If so, circuit is closed.

## Spring Core

### What do you understand by Dependency Injection?

- Dependency Injection design pattern allows us to remove the hard-coded dependencies and make our application loosely coupled, extendable and maintainable. We can implement dependency injection pattern to move the dependency resolution from compile-time to runtime. Some of the benefits of using Dependency Injection are: Separation of Concerns, Boilerplate Code reduction, Configurable components and easy unit testing

### What are the common implementations of the ApplicationContext?

- The three commonly used implementation of 'ApplicationContext' are:
  - `FileSystemXmlApplicationContext`: This container loads the definitions of the beans from XML file. Here we need to provide the full path of the XML bean configuration file to the constructor.
  - `ClassPathXmlApplicationContext`: This container loads the definitions of the beans from XML file. Here we do not need to provide the full path of the XML file but we need to set CLASSPATH properly because this container will look bean configuration XML file in CLASSPATH.
  - `WebXmlApplicationContext`: This container loads the XML file with definitions of all beans from within a web application.

### How do you implement caching in Spring framework? How do you remove cache?

- Use `@EnableCaching` at class level, then add the annotation on the method we would need to cache results for.

```java
@Cacheable("employee")
public Employee findEmployee(int empId) {
...
}
```

- To remove cache, e.g. if Employee is deleted from a dao method, we need to have `@CacheEvict` on the delete method.

### Difference between ApplicationContext and BeanFactory?

- ApplicationContext provides a means for resolving text messages, including support for i18n of those messages.
- ApplicationContext provide a generic way to load file resources, such as images.
- ApplicationContext can publish events to beans that are registered as listeners.
- Certain operations on the container or beans in the container, which have to be handled in a programmatic fashion with a bean factory, can be handled declaratively in an ApplicationContext.
- ApplicationContext implements `MessageSource`, an interface used to obtain localized messages, with the actual implementation being pluggable.

### What bean scopes does Spring support? Explain them.

- The Spring Framework supports following five scopes
  - **singleton**: This scopes the bean definition to a single instance per Spring IoC container.
  - **prototype**: This scopes a single bean definition to have any number of object instances.
  - **request**: This scopes a bean definition to an HTTP request. Only valid in the context of a web-aware Spring ApplicationContext.
  - **session**: This scopes a bean definition to an HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.
  - **global-session**: This scopes a bean definition to a global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

### What is the disadvantage of Spring singleton scope? How do you ensure thread safety?

- It is not thread safe, and hence cannot be used in multithreaded environment, we need to change the bean scope from singleton scope to prototype to ensure the thread safety.

### What is the reason for singleton bean not providing thread safety?

- The default scope of Spring bean is singleton, so there will be only one instance per context. That means having a class level variable that any thread can update will lead to inconsistent data. Hence in default mode spring beans are not thread-safe.
- However we can change spring bean scope to request, prototype or session to achieve thread-safety at the cost of performance. It’s a design decision and based on the project requirements.

### What template does Spring JDBC provide to access database?

- Spring provides following templates to access database
  - JdbcTemplate
  - SimpleJdbcTemplate
  - NamedParameterJdbcTemplate

### What are the advantages of JdbcTemplate in spring?

- Less code: By using the JdbcTemplate class, we don't need to create connection, statement, start transaction, commit transaction and close connection to execute different queries. We can execute the query directly.
- So what we have to do is just define the connection parameters and specify the SQL statement to be executed and do the required work for each iteration while fetching data from the database.

### Can you explain more about JdbcTemplate Class?

- The `JdbcTemplate` class executes SQL queries, updates statements, calls stored procedure, performs iteration over ResultSets, and extracts returned parameter values. It also catches JDBC exceptions and translates them to the generic, more informative, exception hierarchy defined in the `org.springframework.dao` package.
- Instances of the `JdbcTemplate` class are thread safe once configured. So we can configure a single instance of a `JdbcTemplate` and then safely inject this shared reference into multiple DAOs.
- A common practice when using the JDBC Template class is to configure a `DataSource` in our Spring configuration file, and then dependency-inject that shared `DataSource` bean into our DAO classes, and the `JdbcTemplate` is created in the setter for the `DataSource`.

### How can you fetch records by Spring JdbcTemplate?

- We can fetch records from the database by the query method of JdbcTemplate. There are two interfaces to do this:
  - `ResultSetExtractor`
  - `RowMapper`

### What is the difference between ResultSetExtractor and RowMapper?

- `ResultSetExtractor` interface can be used to fetch records from the database. It accepts a `ResultSet` and returns the list.
- `RowMapper` interface allows to map a row of the relations with the instance of user-defined class. It iterates the `ResultSet` internally and adds it into the collection. So we don't need to write a lot of code to fetch the records as `ResultSetExtractor`. It saves a lot of code becuase it internally adds the data of `ResultSet` into the collection.
- `RowMapper` is a higher level interface than `ResultSetExtractor`. We would use the latter if you want to deal with the entire `ResultSet`, and translate that to some sort of returned object, whereas `RowMapper` pre-supposes that each row in the `ResultSet` will be mapped to a returned object of some sort. The callback will happen once for each row.

### What is DispatcherServlet and ContextLoaderListener?

- Spring’s web MVC framework is, like many other web MVC frameworks, request-driven, designed around a central Servlet that handles all the HTTP requests and responses. Spring’s DispatcherServlet however, does more than just that. It is completely integrated with the Spring IoC container so it allows us to use every feature that Spring has.
- After receiving an HTTP request, DispatcherServlet consults the HandlerMapping (configuration files) to call the appropriate Controller. The Controller takes the request and calls the appropriate service methods and set model data and then returns view name to the DispatcherServlet. The DispatcherServlet will take help from ViewResolver to pickup the defined view for the request. Once view is finalized, The DispatcherServlet passes the model data to the view which is finally rendered on the browser.

### What is the ViewResolver class? What is the use of it and what is commonly used implementation?

- ViewResolver is an interface to be implemented by objects that can resolve views by name. There are plenty of ways using which we can resolve view names. These ways are supported by various in-built implementations of this interface. Most commonly used implementation is InternalResourceViewResolver class. It defines prefix and suffix properties to resolve the view component

### Can we have multiple Spring configuration files?

- Yes We can have multiple spring context files. There are two ways to make spring read and configure them.
  - Specify all files in web.xml file using contextConfigLocation init parameter.
  - Import them into existing configuration file we have already configured

## Spring Batch

### Explain the spring batch framework architecture.

- Job is an entity that encapsulates the entire batch process and acts as a container for steps. A job combines multiple steps that belong logically together in a flow and allows for configuration of properties global to all steps such as restartability.
- A job is wired using an XML configuration or java based configuration and it is referred as Job configuration. The job configuration contains:
  - The name of the job.
  - Definition and ordering of steps.
  - Whether or not the job is restartable.
- JobInstance represents a logical job run. Consider a batch job that runs every night, there is a logical JobInstance running everyday.
- JobParameters are set of parameters used to start a batch Job. They also distinguish JobInstance from another.

### What is tasklet in Spring batch framework?

- Tasklet is an interface which performs a single task such as setup resource, running sql update, cleaning up resources etc.

### List out some of the practical usage scenario of Spring Batch framework.

- Reading large number of records from a database, file, queue or any other medium, process it and store the processed records into medium, for example, database.
- Concurrent and massively parallel processing.
- Staged, enterprise message-driven processing.
- Sequential processing of dependent steps.
- Whole-batch transaction.
- Scheduled and repeated processing.
- Technical advantages of using Spring Batch Framework from a Developer perspective.
  - Batch framework leverages Spring programming model thus allows developers to concentrate on the business logic or the business procedure and framework facilitates the infrastructure.
  - Clear separation of concerns between the infrastructure, the batch execution environment, the batch application and the different steps/proceses within a batch application.
  - Provides common scenario based, core execution services as interfaces that the applications can implement and in addition to that framework provides its default implementation that the developers could use or partially override based on their business logic.
  - Easily configurable and extendable services across different layers.
  - Provides a simple deployment model built using Maven.

### What are the important features of Spring Batch?

- Restorability: Restart a batch program from where it failed.
- Different Readers and Writers : Provides great support to read from text files, csv, JMS, JDBC, Hibernate, iBatis etc. It can write to JMS, JDBC, Hibernate, files and many more.
- Chunk Processing : If we have 1 Million records to process, these can be processed in configurable chunks (1000 at a time or 10000 at a time).
- Easy to implement proper transaction management even when using chunk processing.
- Easy to implement parallel processing. With simple configuration, different steps can be run in parallel.

## Spring Boot

### What is SpringBoot?

- First of all Spring Boot is not a framework, it is a way to create stand-alone application with minimal or zero configurations. It provides defaults for code and annotation configuration to quick start new spring projects within no time.
- It leverages existing spring projects as well as third party projects to develop production ready applications.
- It provides a set of starter pom’s build files which one can use to add required dependencies and also facilitate auto configuration.
- It avoids writing lots of boilerplate Code, Annotations and XML Configuration.
- It provides Embedded HTTP servers like Tomcat, Jetty etc. to develop and test our web applications very easily.
- Starter POMs are a set of convenient dependency descriptors that we can include in our application. We get a one-stop-shop for all the Spring and related technology that we need, without having to hunt through sample code and copy paste loads of dependency descriptors. For example, if we want to get started using Spring and JPA for database access, just include the spring-boot-starter-data-jpa dependency in our project, and we are good to go.

### What is the use of @EnableAutoConfiguration and @SpringBootApplication? When do we use these?

- `@EnableAutoConfiguration` annotation on a Spring Java configuration class causes Spring Boot to automatically create beans it thinks we need and usually based on classpath contents, it can easily override
- `@SpringBootApplication` is the convenience annotation to be used in Spring Boot application and this is equivalent to having `@Configuration`, `@EnableAutoConfiguration` and `@ComponentScan` in the main class.

## Database SQL

### What is a key difference between Delete, Truncate and Drop?

- **DELETE** command is used to remove rows from a table. A WHERE clause can be used to only remove some rows. If no WHERE condition is specified, all rows will be removed. After performing a DELETE operation you need to COMMIT or ROLLBACK the transaction to make the change permanent or to undo it. Note that this operation will cause all DELETE triggers on the table to fire.
- **TRUNCATE** removes all rows from a table. The operation cannot be rolled back and no triggers will be fired. As such, TRUNCATE is faster and doesn't use as much undo space as a DELETE.
- **DROP** command removes a table from the database. All the tables' rows, indexes and privileges will also be removed. No DML triggers will be fired. The operation cannot be rolled back.
- DROP and TRUNCATE are DDL commands, whereas DELETE is a DML command. DELETE operations can be rolled back (undone), while DROP and TRUNCATE operations cannot be rolled back.

### Explain different JOINs.

- **INNER JOIN** : The INNER JOIN selects all rows from both participating tables as long as there is a match between the columns. A SQL INNER JOIN is same as JOIN clause, combining rows from two or more tables
- **LEFT JOIN** (or **LEFT OUTER JOIN**): Returns all rows from the left table, and the matched rows from the right table; i.e., the results will contain all records from the left table, even if the JOIN condition doesn’t find any matching records in the right table. This means that if the ON clause doesn’t match any records in the right table, the JOIN will still return a row in the result for that record in the left table, but with NULL in each column from the right table.
- **RIGHT JOIN** (or **RIGHT OUTER JOIN**): Returns all rows from the right table, and the matched rows from the left table. This is the exact opposite of a LEFT JOIN; i.e., the results will contain all records from the right table, even if the JOIN condition doesn’t find any matching records in the left table. This means that if the ON clause doesn’t match any records in the left table, the JOIN will still return a row in the result for that record in the right table, but with NULL in each column from the left table.
- **FULL JOIN** (or **FULL OUTER JOIN**): Returns all rows for which there is a match in EITHER of the tables. Conceptually, a FULL JOIN combines the effect of applying both a LEFT JOIN and a RIGHT JOIN; i.e., its result set is equivalent to performing a UNION of the results of left and right outer queries.
- **CROSS JOIN**: Returns all records where each row from the first table is combined with each row from the second table (i.e., returns the Cartesian product of the sets of rows from the joined tables). Note that a CROSS JOIN can either be specified using the CROSS JOIN syntax (“explicit join notation”) or listing the tables in the FROM clause separated by commas without using a WHERE clause to supply join criteria (“implicit join notation”).
- **SELF JOIN**: A self join is a join in which a table is joined with itself (which is also called Unary relationships), especially when the table has a FOREIGN KEY which references its own PRIMARY KEY. To join a table itself means that each row of the table is combined with itself and with every other row of the table.

### What is the difference between UNION and JOIN?

- Joins and Unions can be used to combine data from one or more tables. The difference lies in how the data is combined.
  In simple terms, joins combine data into new columns. If two tables are joined together, then the data from the first table is shown in one set of column alongside the second table’s column in the same row.
- Unions combine data into new rows. If two tables are “unioned” together, then the data from the first table is in one set of rows, and the data from the second table in another set. The rows are in the same result.

### What is the difference between UNION and UNION ALL?

- UNION merges the contents of two structurally-compatible tables into a single combined table. The difference between UNION and UNION ALL is that UNION will omit duplicate records whereas UNION ALL will include duplicate records.
- It is important to note that the performance of UNION ALL will typically be better than UNION, since UNION requires the server to do the additional work of removing any duplicates. So, in cases where it is certain that there will not be any duplicates, or where having duplicates is not a problem, use of UNION ALL would be recommended for performance reasons.

### What is the difference between procedure and function?

- Function must return a value but in Stored Procedure it is optional (Procedure can return zero or n values)
- Functions can be called from Procedure whereas Procedures cannot be called from Function.
- Procedure allows SELECT as well as DML(INSERT/UPDATE/DELETE) statement in it whereas Function allows only SELECT statement in it.
- Stored Procedures cannot be used in the SQL statements anywhere in the WHERE/HAVING/SELECT section whereas Function can be.

### What is index?

- Index is used to increase the performance and allow faster retrieval of records from the table. An index creates an entry for each value and it will be faster to retrieve data.
- There are three types of Indexes in SQL: Unique Index, Clustered Index, NonClustered Index
- **Unique Index** : This indexing does not allow the field to have duplicate values if the column is unique indexed. Unique index can be applied automatically when primary key is defined.
- **Clustered Index** : The clustered index is used to reorder the physical order of the table and search based on the key values. Each table can have only one clustered index.
- **NonClustered Index** : NonClustered Index does not alter the physical order of the table and maintains logical order of data.
