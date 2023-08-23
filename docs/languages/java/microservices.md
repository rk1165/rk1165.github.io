- What are the advantages and disadvantages of using a Microservice based architecture ?
  > **Advantages**: Improved Scalability, Fault Isolation, Localised Complexity, Increased Agility, Simplified Debugging and Maintenance, Smaller Development Teams, etc.
  >
  > **Disadvantages**: Complexity (e.g.: Dependencies), Requires accurate pre-planning, End-to-end testing is difficult, Complex deployment procedures, etc.
- What is an API Gateway ? What "problem" does it solve ?
- What is a Circuit Breaker ? What "problem" does it solve ? How is the `@HystrixCommand` annotations works in a Spring Cloud implementation?
- Explain the concept of **Observability** ?
  - What is the **Log Aggregation** pattern ? What problem does it solve ? Can you give examples of **Log Aggregation** solutions (e.g.: AWS Cloud Watch) ?
  - What is the **Application Metrics** pattern ? What problem does it solve ? What are the two models of aggregating metrics (push. vs pull) ? Can you give examples of **Application Metrics** solutions ?
  - What is the **Distributed Tracing** pattern ?
- You have applied the **Database per Service** pattern, so each microservice has its own database. Some business transactions are spanning over multiple services, so you need a mechanism to implement transaction that Span Services. How to implement transaction that span services ? (e.g.: **Saga Pattern**)
- You have applied the **Database per Service** pattern, so each microservice has its own database. Additionally, you have implemented the **Event Sourcing** Pattern and data is no longer easily queryable. How to implement a query that retrieves data from multiple services in a micorservice architecture ? (e.g.: Command Query Responsibility Segregation - CQRS).

### Core Microservice development patterns

- Service granularity
- Communication protocols
- Interface design
- Configuration management of services : to move it across environments **Spring Cloud Config**
- Event processing between services **Spring Cloud Stream**

### Microservice routing patterns

- Service discovery : **Spring Cloud Discovery/ Netflix Eureka**
- Service routing : API gateway **Spring Cloud API Gateway**

### Microservice client resiliency patterns

- To prevent a single service from cascading up and out to the consumers of the service.
- Client-side load balancing : **Spring Cloud Load balancer**
- Circuit breaker pattern : **Resilience 4j**
- Fallback pattern : **Resilience 4j**
- Bulkhead pattern : **Resilience 4j**

### Microservice security patterns

- Authentication **Spring CLoud Security/OAuth2**
- Authorization **Spring CLoud Security/OAuth2**
- Credential management and propagation **Spring CLoud Security/OAuth2/JWT**

### Microservice logging and tracing patterns

- Log correlation : tie together all the logs produced between services for a single user transaction **Spring Cloud Sleuth**
- Log aggregation : how to pull together all of the logs produced by your microservices **Spring Cloud Sleuth (ELK Stack)**
- Microservice tracing : **Spring Cloud Sleuth/Zipkin**

### Application metrics pattern

- Metrics
- Metrics service : it can obtain the metrics using the pull or push style
- Metrics visualization

### Microservice build/deployment patterns

- Build and deployment pipelins : **Jenkins**
- Infrastructure as a code : **Docker**
- Immutable servers : Once a microservice image is created, how you ensure that it’s never changed after it has been deployed. **Docker**
- Phoenix servers : How you ensure that servers that run individual containers get torn down on a regular basis and re-created from an immutable image.
