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
1. GIT
1. Maven
1. Agile Scrum
1. JIRA

## Test
1. Unit Test
1. Integration Test
1. JUnit
    - lify cycle
    - @Before @After @Test
    - Runners
1. Mockito
    - What why
1. Code coverage
    - jacoco
1. SonarQube
    - bugs, smell, static 

## CI/CD
1. Jenkins - pipeline 
1. Linux 

## Security
1. Encode / Encryption / Hash 
1. JWT
1. OAuth2
1. SSO
1. Authentication vs Authorization
1. HTTP Authorization Header
1. HTTPS

## Other 
1. Documentation - Swagger RESTful
1. CAP
1. SOLID
1. ACID