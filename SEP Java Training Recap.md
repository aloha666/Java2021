# SEP Java Training Recap
---
## Core Java
1. Collections
    - List / Map / Set
    - ArrayList / LinkedList / Array / HashMap / TreeMap / LinkedHashMap / TreeSet / HashSet
    - equals() hashcode()
1. Multithreading
    - ConcurrentHashMap / Collections.synchronizedMap(xxxx) -> synchronzedMap / hashtable(X)
    - Synchronized(lock){....}
    - Lock interface
1. Java 8
    - Stream -> terminal operation(foreach, collection) / intermediate operation(filter, map, reduce, flatmap, sorted)
    - Functional Interface (Function, Predicate, Comparator,...)
    - Method References (Class::method)
    - Lambda Expression
    - CompletableFuture(non-blocking Future, vs Future)
    - LocalDateTime Api
1. JVM
    - Heap vs Stack
1. Garbage Collection
1. Design Patterns
   - Singleton, factory......

## SQL vs NoSQL
1. Relationship
    - 1-1, m-1, m-m
1. Joins
    - outer inner 
1. Query
    - select from where group by having order by;
    - join ... on...
1. ORM
    - JPA / Hibernate
    - Lazy Loading/Fetch  eager
    - Cascade (ALL, NONE, MERGE, PERSIST)
1. SQL vs NoSQL
1. MongoDB - JSON- Schemaless - Replica, Sharding

## Web
1. HTTP
    - Request (Method) GET PUT POST DELET (idempotent)
    - Reponse (status code)
1. Servlet
    - Filter(pre-post process)
    - doGet/doPost
    - DispachterServlet
1. Three-Tiers Architecture
    - Web / service-business / dao -repository
    - Interface loose couple

## Web Services
1. RESTful
    - HTTP JSON
    - Easy simple
1. SOAP
    - XML
    - Complicated
1. Microservices
    - popular
    - Docker. cloud. flexble, scalable
    - Service Discovery - Spring Cloud Eureka
    - Ribbon(LB)
    - Sleuth zipkin (trasaction trace)
    - Zuul (APIGateway)
    - Config Server
    - Fault tolerance - Resilience4j(retry-cache-fallback-circuitbreaker)

## Spring Family
1. IOC, DI
    - Loose coupling - @Autowired, @Controller, Service, Repository, Bean
1. AOP
    - PointCut, Advice, Aspect, Joinpoint
    - use cases - secutiry, cache, audit, log, transaction management @Transactional
1. MVC
    - @Controller, @RestController
    - @RequestMapping(URL,Method) -> method
    - input @PathVarible @RequestParam @RequestHeader @RequestBody
    - output @ResponseBody - Json/xml -> Jackson and Jax-b
    - @Validated bean validation
    - @ExceptionHandler -> Exception
    - @ControllerAdvice -> class -> @ExceptionHandler -> global exception handler
1. Data
    - simplify the dao implementation.
    - JPA  MongoDB
1. Security
    - Configuration
    - @Secured @Pre-Authorized
1. Cloud
    - Microservice 
    - AWS
    - Messaging/Integration

## AWS
1. IAAS/PAAS/SAAS

   <img src="https://s7280.pcdn.co/wp-content/uploads/2017/09/saas-vs-paas-vs-iaas.png" alt="SaaS vs PaaS vs IaaS: What&#39;s The Difference &amp; How To Choose – BMC Software  | Blogs" style="zoom: 50%;" />

   ```
   IAAS:Infrastructure as a service (IaaS): AWS
   PAAS:Platform as as service: Windows Azure
   SAAS:Software as a service (IaaS) Software as a service is a software licensing and delivery model in which software is licensed on a subscription basis and is centrally hosted. Dropvix Google apps
   ```

   

1. IAM/VPC

   ```
   IAM:AWS Identity and Access Management (IAM) enables you to manage access to AWS services and resources securely.
   VPC:Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. 
   ```

   

1. EC2/Lambda

   ```
   EC2: Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud.
   Lambda:  AWS Lambda is a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you.
   
   AWS EC2 is a service that represents the traditional cloud infrastructure (IaaS) and allows you to run EC2 instances as VMs, configure environments, and run custom applications. AWS Lambda provides you a serverless architecture and allows you to run a piece of code in the cloud after an event trigger is activated.
   ```

   

1. S3/RDS

   ```
   S3: Amazon S3 or Amazon Simple Storage Service is a service offered by Amazon Web Services that provides object storage through a web service interface.
   RDS:Amazon Relational Database Service is a distributed relational database service by Amazon Web Services.
   
   While S3 is strongly consistent, its consistency is limited to single storage operations. On the other hand, RDS supports transactions that allow one to execute a series of operations while maintaining consistency and even providing an option to roll back the operations in case of the steps go wrong.
   ```

   

1. SNS/SQS

   ```
   SNS:Amazon Simple Notification Service is a notification service provided as part of Amazon Web Services
   SQS:Amazon Simple Queue Service is a distributed message queuing service introduced by Amazon.com
   
   AWS SNS is a publisher subscriber network, where subscribers can subscribe to topics and will receive messages whenever a publisher publishes to that topic. AWS SQS is a queue service, which stores messages in a queue
   ```

   

## SDLC / Project management
1. Dev-Test-UAT-Prod environment

   ```
   develop-test-User Acceptance Test-Production
   
   UAT: actual users test the software to validate that it is performing according to the required real-life scenarios.
   ```

1. GIT

1. Maven

1. Agile Scrum

1. JIRA

## Test
1. Unit Test

    ```
    Unit Testing is defined as a type of software testing where individual components of a software are tested.
    ```

1. Integration Test

    ```
    Integration Test: Integration testing is the process of testing the interface between two software units or modules. Its focus is on determining the correctness of the interface.
    ```

    |                         Unit Testing                         |                     Integration Testing                      |
    | :----------------------------------------------------------: | :----------------------------------------------------------: |
    | In unit testing each module of the software is tested separately. | In integration testing all modules of the the software are tested combined. |
    | In unit testing tester knows the internal design of the software. | In integration testing doesn’t know the internal design of the software. |
    |  Unit testing is performed first of all testing processes.   | Integration testing is performed after unit testing and before system testing. |
    |             Unit testing is a white box testing.             |         Integration testing is a black box testing.          |
    |    Unit testing is basically performed by the developer.     |       Integration testing is performed by the tester.        |
    |        Detection of defects in unit testing is easy.         |  Detection of defects in integration testing is difficult.   |
    | It tests parts of the project without waiting for others to be completed. |       It tests only after the completion of all parts.       |
    |                 Unit testing is less costly.                 |             Integration testing is more costly.              |

1. JUnit
    
    ```
    JUnit is a Java unit testing framework that's one of the best test methods for **regression testing**. An open-source framework, it is used to write and run repeatable automated tests.
    ```
    
    - lify cycle
    
      ```
      The lifecycle consists of three pieces: setup, test and teardown, all executed in sequence.
      
      teardown: called to do any requied post precessing after a test. "clean up"
      ```
    
    - @Before @After @Test
    
      ```
      @Beforeclass @AfterClass execute once before/after all test cases
      @Before @After execute every time before/after all test cases
      ```
    
    - Runners
    
      ```
      A JUnit Runner is class that extends JUnit's abstract Runner class. Runners are used for running test classes. 干活的
      ```
    
      
    
1. Mockito
    - What why
    
      ```
      Mockito is a mocking framework, JAVA-based library that is used for effective unit testing of JAVA applications. Mockito is used to mock interfaces so that a dummy functionality can be added to a mock interface that can be used in unit testing.
      ```
    
1. Code coverage
    - jacoco
    
      ```
      JaCoCo is an open source toolkit for measuring **code coverage** in a code base and reporting it through visual reports
      visulized the code
      ```
    
      
    
1. SonarQube
    - bugs, smell, static 
    
      ```
      SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities on 20+ programming languages.
      ```

## ?CI/CD
1. Jenkins - pipeline 

   ```
   continuous integration Continuous deliver&development
   
   ```

   

1. Linux 

## Security
1. Encode / Encryption / Hash 

1. JWT

   ```
   Jason Web Token, JSON Web Token is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims. The tokens are signed either using a private secret or a public/private key. 
   ```

1. OAuth2

   ```
   OAuth doesn't share password data but instead uses authorization tokens to prove an identity between consumers and service providers. OAuth is an authentication protocol that allows you to approve one application interacting with another on your behalf without giving away your password.
   ```

1. SSO

   ```
   Single sign-on (SSO) is an authentication method that enables users to securely authenticate with multiple applications and websites by using just one set of credentials.
   ```

1. Authentication vs Authorization

   ```
    Authentication confirms that users are who they say they are. Authorization gives those users permission to access a resource.
   ```

   

1. HTTP Authorization Header

   ```
   HTTP basic authentication is a simple challenge and response mechanism with which a server can request authentication information (a user ID and password) from a client. The client passes the authentication information to the server in an Authorization header. The authentication information is in base-64 encoding.
   ```

   

1. HTTPS

   ```
   In HTTPS, the [communication protocol] is encrypted using [Transport Layer Security] (TLS) or, formerly, Secure Sockets Layer (SSL). The protocol is therefore also referred to as **HTTP over TLS**, or **HTTP over SSL**.
   The principal motivations for HTTPS are [authentication]of the accessed [website], and protection of the [privacy] and [integrity] of the exchanged data while in transit. 
   ```

   

## Other 
1. Documentation - Swagger RESTful

1. CAP

1. SOLID?

   ```
   The Single-responsibility principle: every class should have only one responsibility.
   The Open–closed principle: " open for extension, but closed for modification."
   The Liskov substitution principle: 一个对象在其出现的任何地方，都可以用子类实例做替换，
   The Interface segregation principle: "Many client-specific interfaces are better than one general-purpose interface."个类实现的接口中，包含了它不需要的方法。将接口拆分成更小和更具体的接口，有助于解耦，从而更容易重构、更改。
   The Dependency inversion principle: "Depend upon abstractions, [not] concretions."
   ```

   

1. ACID