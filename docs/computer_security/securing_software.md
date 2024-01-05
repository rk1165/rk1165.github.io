- A _threat_ is an intention to cause damage. The attacker posing a threat is commonly called a _threat actor_
- Vulnerabilities and their statistics can be queried from the [National Vulnerability Database](https://nvd.nist.gov/vuln/search) and from the [Common Vulnerability and Exposure database](https://cve.mitre.org/cve/search_cve_list.html)
- Some recent vulnerabilities
  - [Beast](https://blog.qualys.com/ssllabs/2013/09/10/is-beast-still-a-threat)
  - [Heartbleed](https://heartbleed.com/)
  - [Heist](http://arstechnica.com/security/2016/08/new-attack-steals-ssns-e-mail-addresses-and-more-from-https-pages/)
  - [Krack](https://arstechnica.com/information-technology/2017/10/severe-flaw-in-wpa2-protocol-leaves-wi-fi-traffic-open-to-eavesdropping/)
- The **STRIDE Threat Model** is a useful checklist of questions that can help in the threat-modelling of an application.
- STRIDE is an acronym for the following threat categories: _Spoofing_, _Tampering_, _Repudiation_, _Information Disclosure_, _Denial of Service_, and _Elevation of Privilege_
- Spoofing covers cases where someone is illegally accessing a system using another user’s authentication information.
- Tampering covers cases such as unauthorized changes made to persistent data, whether inside a machine or in the transport.
- Repudiation specifies that a system should be able to trace user operations to provide evidence of what has happened in case of a breach.
- Information Disclosure covers the exposure of information to unauthorized individuals. (This category of threat can also occur within a machine or during transport.)
- Denial of Service refers to cases where the server or service is made temporarily unavailable.
- Elevation of Privilege is a threat type in which an unprivileged user finds a way to gain sufficient privileges to compromise the system.
- The **DREAD risk assessment model** is a mnemonic checklist for prioritizing threats based on their severity, and stands for _Damage_, _Reproducibility_, _Exploitability_, _Affected Users_, and _Discoverability_.

### Part 1

- The `@RequestMapping` annotation defines the request paths (e.g. "/hello") that the subsequent method will handle, and the `@ResponseBody` annotation essentially says that the response that has been created in the method should be sent directly to the user requesting the content.
- Each request may contain information that is being sent to the web application. In principle, there are two ways to handle this:
  1. by adding parameters to the address,
  2. adding parameters to the request body
- The parameters can be accessed using the `@RequestParam`-annotation
- Thymeleaf pages (the so called templates) reside in the project folder `src/main/resources/templates` or underneath it.
- dependency has not yet been downloaded to your machine, it needs to be downloaded. This can be done by either writing `mvn dependency:resolve` (if using terminal)
- Thymeleaf expects that each HTML page has the following preamble.

```html
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
  `
</html>
```

- the request no longer has the annotation `@ResponseBody`. Effectively this means that we do not wish that the response from the method is sent directly to the user, but that it is used to determine the template that will be used to create the view.
- Adding data to the view is done using a _Model-object_. The Model object is automatically taken into use when it is added as a parameter to a method that handles incoming requests.
- The Model is essentially a map with strings as keys and any objects as values.
- One may also include lists to the model
- Iterating a list in the Thymeleaf template can be done using the attribute `th:each`. The basic syntax for `th:each` is as follows.

```java
<p th:each="item : ${list}">
    <span th:text="${item}">hello world!</span>
</p>
```

- The attribute `th:each` can be used in principle with any repeatable element; it could be used, for example, with the `li`-element as follows.

```java
<ul>
    <li th:each="item : ${list}">
        <span th:text="${item}">hello world!</span>
    </li>
</ul>
```

- When adding JavaScript code to a Spring Boot project, it is typically added to the folder `src/main/resources/public/javascript/`
- Given that a Javascript file -- say code.js is in the folder javascript, the script-element is used as follows: `<script th:src="@{/javascript/code.js}"></script>.`
- Changing the value inside an element can be changed -- for example -- using the innerHTML parameter of an element.
- we wish to retrieve data from the server. This can be done using the XMLHttpRequest object as follows.

```javascript
var xmlHttp = new XMLHttpRequest();
xmlHttp.onreadystatechange = function () {
  if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
    var response = JSON.parse(xmlHttp.responseText);
    document.querySelector("#content").innerHTML = response.value.joke;
  }
};
xmlHttp.open("GET", "http://api.icndb.com/jokes/random/", true);
xmlHttp.send(null);
```

- If a method that returns an object is annotated using the `@ResponseBody` annotation, then the object will be returned as a JSON object by default
- Simple CORS support is added through the `@CrossOrigin` annotation, which is given the application addresses, from which requests can be made to the server
- A program that uses a database needs to
  1. create a database connection,
  2. execute a query to the database,
  3. do something with the query results
  4. close the connection
- ResultSet, which provides an access to the query results
- The method `next` moves to the next row in the result table, and the method `getString("column name")` retrieves the value for column "column name" for that row as a String.
- The command `DriverManager.getConnection("jdbc:h2:file:./database", "sa", "");` creates a JDBC connection to a database called "database". The username is "sa" and the password is empty.
- H2 Database Engine provides support for loading schemas and data using the RunScript class.
- the need for transforming database query results into objects arises
- ORM tools offer -- among other things -- the functionality needed to transform existing classes into database schemas and to build queries using objects
- One standard for ORM technique in Java is the Java Persistence Api (JPA), which has been implemented by a set of frameworks such as Hibernate
- The JPA standard states that each class that represents a database table should be defined as an entity; this can be done with the annotation `@Entity`
- each class that represents a table should have an identifier that can be used to identify a specific instance of that class. Such an identifier is typically an object variable which is annotated using the `@Id` annotation
- the class should implement the `Serializable` interface.
- the column names and the database table name can be included using the annotations `@Column` and `@Table`.
- Transactions are used to verify that all the database operations in a group are executed, or that none of them are
- If a method has been annotated using the `@Transactional` annotation, then all the database functionality within that method will be performed within a single transaction
- If the annotation is on the class level (i.e. before the class definition), then all the methods in that class are transactional.
- `@Transactional` also indicates that the entities are managed within the method. That is, the entities that have been loaded from the database are tracked, and the changes that are made to them are written to the database at the end of the method.
- When working with JPA and databases, the programmer needs to define the relationships with annotations. These relationship types are `@OneToMany`, `@ManyToOne` and `@ManyToMany`
- HTTP is a stateless protocol which means that each request that is sent to a server is processed individually, and from the point of view of the server, the requests are not linked with each others
