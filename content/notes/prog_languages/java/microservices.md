+++
title = 'Microservices'
date = 2024-02-11

+++

- What are the advantages and disadvantages of using a Microservice based architecture ?
  > **Advantages**: Improved Scalability, Fault Isolation, Localised Complexity, Increased Agility, Simplified Debugging and Maintenance, Smaller Development Teams, etc.
  > **Disadvantages**: Complexity (e.g.: Dependencies), Requires accurate pre-planning, End-to-end testing is difficult, Complex deployment procedures, etc.

### Microservice routing patterns

- Service discovery : **Spring Cloud Discovery/Netflix Eureka**
- Service routing : API gateway **Spring Cloud API Gateway**

### Microservice client resiliency patterns

- To prevent a single service from cascading up and out to the consumers of the service.
- Client-side load balancing : **Spring Cloud Load balancer**
- Circuit breaker pattern : **Resilience 4j**
- Fallback pattern : **Resilience 4j**
- Bulkhead pattern : **Resilience 4j**

### Microservice security patterns

- Authentication **Spring Cloud Security/OAuth2**
- Authorization **Spring Cloud Security/OAuth2**
- Credential management and propagation **Spring Cloud Security/OAuth2/JWT**

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
