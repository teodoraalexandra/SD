# Building the Docker Images

Run the following maven command. 

   **mvn clean package docker:build**

Running the above command at the root of the project directory will build all of the projects.  If everything builds successfully you should see a message indicating that the build was successful.

# Running the services

Issue the following docker-compose command:

   **docker-compose -f docker/common/docker-compose.yml up**

If everything starts correctly you should see a bunch of Spring Boot information fly by on standard out.  

# Summaries from "John Carnell - Spring Microservices in Action"

* Microservices are extremely small pieces of functionality that are responsible for one specific area of scope.
* No industry standards exist for microservices. Unlike other early web service protocols, microservices take a principle-based approach and align with the concepts of REST and JSON.
* Writing microservices is easy, but fully operationalizing them for production requires additional forethought. We introduced several categories of microservice development patterns, including core development, routing patterns, client resiliency, security, logging, and build/deployment patterns.
* While microservices are language-agnostic, we introduced two Spring frameworks that significantly help in building microservices: Spring Boot and Spring Cloud.
* Spring Boot is used to simplify the building of REST-based/JSON microservices. Its goal is to make it possible for you to build microservices quickly with nothing more than a few annotations.
* Spring Cloud is a collection of open source technologies from companies such as Netflix and HashiCorp that have been “wrapped” with Spring annotations to significantly simplify the setup and configuration of these services.

---

* To be successful with microservices, you need to integrate in the architect’s, software developer’s, and DevOps’ perspectives.
* Microservices, while a powerful architectural paradigm, have their benefits and trade offs. Not all applications should be microservice applications.
* From an architect’s perspective, microservices are small, self-contained, and distributed. Microservices should have narrow boundaries and manage a small set of data.
* From a developer’s perspective, microservices are typically built using a REST-style of design, with JSON as the payload for sending and receiving data from the service.
* Spring Boot is the ideal framework for building microservices because it lets you build a REST-based JSON service with a few simple annotations.
* From a DevOp’s perspective, how a microservice is packaged, deployed, and monitored are of critical importance.
* Out of the box, Spring Boot allows you to deliver a service as a single executable JAR file. An embedded Tomcat server in the producer JAR file hosts the service.
* Spring Actuator, which is included with the Spring Boot framework, exposes information about the operational health of the service along with information about the services runtime.

---

* Spring Cloud configuration server allows you to set up application properties with environment specific values.
* Spring uses Spring profiles to launch a service to determine what environment properties are to be retrieved from the Spring Cloud Config service.
* Spring Cloud configuration service can use a file-based or Git-based application configuration repository to store application properties.
* Spring Cloud configuration service allows you to encrypt sensitive property files using symmetric and asymmetric encryption.

---

* The service discovery pattern is used to abstract away the physical location of services.
* A service discovery engine such as Eureka can seamlessly add and remove service instances from an environment without the service clients being impacted.
* Client-side load balancing can provide an extra level of performance and resiliency by caching the physical location of a service on the client making the service call.
* Eureka is a Netflix project that when used with Spring Cloud, is easy to set up and configure.
* You used three different mechanisms in Spring Cloud, Netflix Eureka, and Netflix Ribbon to invoke a service. These mechanisms included: Using a Spring Cloud service DiscoveryClient, Using Spring Cloud and Ribbon-backed RestTemplate, Using Spring Cloud and Netflix’s Feign client.

---

* When designing highly distributed applications such as a microservice-based application, client resiliency must be taken into account.
* Outright failures of a service (for example, the server crashes) are easy to detect and deal with.
* A single poorly performing service can trigger a cascading effect of resource exhaustion as threads in the calling client are blocked waiting for a service to complete.
* Three core client resiliency patterns are the circuit-breaker pattern, the fallback pattern, and the bulkhead pattern.
* The circuit breaker pattern seeks to kill slow-running and degraded system calls so that the calls fail fast and prevent resource exhaustion.
* The fallback pattern allows you as the developer to define alternative code paths in the event that a remote service call fails or the circuit breaker for the call fails.
* The bulk head pattern segregates remote resource calls away from each other, isolating calls to a remote service into their own thread pool. If one set of service calls is failing, its failures shouldn’t be allowed to eat up all the resources in the application container.
* Spring Cloud and the Netflix Hystrix libraries provide implementations for the circuit breaker, fallback, and bulkhead patterns.
* The Hystrix libraries are highly configurable and can be set at global, class, and thread pool levels.
* Hystrix supports two isolation models: THREAD and SEMAPHORE.
* Hystrix’s default isolation model, THREAD, completely isolates a Hystrix protected call, but does not propagate the parent thread’s context to the Hystrix managed thread.
* Hystrix’s other isolation model, SEMAPHORE, does not use a separate thread to make a Hystrix call. While this is more efficient, it also exposes the service to unpredictable behavior if Hystrix interrupts the call.
* Hystrix does allow you to inject the parent thread context into a Hystrix managed Thread through a custom HystrixConcurrencyStrategy implementation.

---

* Spring Cloud makes it trivial to build a services gateway.
* The Zuul services gateway integrates with Netflix’s Eureka server and can automatically map services registered with Eureka to a Zuul route.
* Zuul can prefix all routes being managed, so you can easily prefix your routes with something like /api.
* Using Zuul, you can manually define route mappings. These route mappings are manually defined in the applications configuration files.
* By using Spring Cloud Config server, you can dynamically reload the route mappings without having to restart the Zuul server.
* You can customize Zuul’s Hystrix and Ribbon timeouts at global and individual service levels.
* Zuul allows you to implement custom business logic through Zuul filters. Zuul has three types of filters: pre-, post, and routing Zuul filters.
* Zuul pre-filters can be used to generate a correlation ID that can be injected into every service flowing through Zuul.
* A Zuul post filter can inject a correlation ID into every HTTP service response back to a service client.
* A custom Zuul route filter can perform dynamic routing based on a Eureka service ID to do A/B testing between different versions of the same service.

---

* OAuth2 is a token-based authentication framework to authenticate users.
* OAuth2 ensures that each microservice carrying out a user request does not need to be presented with user credentials with every call.
* OAuth2 offers different mechanisms for protecting web services calls. These mechanisms are called grants.
* To use OAuth2 in Spring, you need to set up an OAuth2-based authentication service.
* Each application that wants to call your services needs to be registered with your OAuth2 authentication service.
* Each application will have its own application name and secret key.
* User credentials and roles are in memory or a data store and accessed via Spring security.
* Each service must define what actions a role can take.
* Spring Cloud Security supports the JavaScript Web Token (JWT) specification.
* JWT defines a signed, JavaScript standard for generating OAuth2 tokens.
* With JWT, you can inject custom fields into the specification.
* Securing your microservices involves more than just using OAuth2. 
* Use HTTPS to encrypt all calls between services.
* You should use a services gateway to narrow the number of access points a service can be reached through.
* You should limit the attack surface for a service by limiting the number of inbound and outbound ports on the operating system that the service is running on.

---

* Asynchronous communication with messaging is a critical part of microservices architecture.
* Using messaging within your applications allows your services to scale and become more fault tolerant.
* Spring Cloud Stream simplifies the production and consumption of messages by using simple annotations and abstracting away platform-specific details of the underlying message platform.
* A Spring Cloud Stream message source is an annotated Java method that’s used to publish messages to a message broker’s queue.
* A Spring Cloud Stream message sink is an annotated Java method that receives messages off a message broker’s queue.
* Redis is a key-value store that can be used as both a database and cache.

---

* Spring Cloud Sleuth allows you to seamlessly add tracing information (correlation ID) to your microservice calls.
* Correlation IDs can be used to link log entries across multiple services. They allow you to see the behavior of a transaction across all the services involved in a single transaction.
* While correlation IDs are powerful, you need to partner this concept with a log aggregation platform that will allow you to ingest logs from multiple sources and then search and query their contents.
* While multiple on-premise log aggregation platforms exist, cloud-based services allow you to manage your logs without having to have extensive infrastructure in place. They also allow you to easily scale as your application logging volume grows.
* You can integrate Docker containers with a log aggregation platform to capture all the logging data being written to the containers stdout/stderr. 
* While a unified logging platform is important, the ability to visually trace a transaction through its microservices is also a valuable tool.
* Zipkin allows you to see the dependencies that exist between services when a call to a service is made.
* Spring Cloud Sleuth integrates with Zipkin. Zipkin allows you to graphically see the flow of your transactions and understand the performance characteristics of each microservice involved in a user’s transaction.
* Spring Cloud Sleuth will automatically capture trace data for an HTTP call and inbound/outbound message channel used within a Spring Cloud Sleuth enabled service.
* Spring Cloud Sleuth maps each of the service call to the concept of a span. Zipkin allows you to see the performance of a span.
* Spring Cloud Sleuth and Zipkin also allow you to define your own custom spans so that you can understand the performance of non-Spring-based resources (a database server such as Postgres or Redis).
