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

### What are the types of IOC container in spring?

- There are two types of IoC containers in Spring. `BeanFactory` and `ApplicationContext`

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
  - **session**: This scopes a bean definition to an HTTP session. Only valid in the context of a webaware Spring ApplicationContext.
  - **global-session**: This scopes a bean definition to a global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

### What is the disadvantage of Spring singleton scope? How do you ensure thread safety?

- It is not thread safe, and hence cannot be used in multithreaded environment, we need to change the bean scope from singleton scope to prototype to ensure the thread safety.

### What is the reason for singleton bean not providing thread safety?

- The default scope of Spring bean is singleton, so there will be only one instance per context. That means that all the having a class level variable that any thread can update will lead to inconsistent data. Hence in default mode spring beans are not thread-safe.
- However we can change spring bean scope to request, prototype or session to achieve thread-safety at the cost of performance. It’s a design decision and based on the project requirements.

### Topics in Spring core

- Bean Life Cycle
- Init / destroy methods
- Bean Listeners
- Processors
- Scopes
- Loading mechanisms

## Spring JDBC

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

## Spring MVC

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
