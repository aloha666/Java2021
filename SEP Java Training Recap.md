# SEP Java Training Recap
---
## Core Java
1. Collections
    - List / Map / Set Interfaces
    
      ```
      list: can have duplicate,keep insert position
      map:key-value pair
      set: no duplicate
      ```
    
      
    
    - ArrayList / LinkedList / Array / HashMap / TreeMap / LinkedHashMap / TreeSet / HashSet
    
      ```
      ArrayList:
      LinkedList:
      Array:
      HashMap: dynamic array + linkedlist/red-black tree
      TreeMap:
      LinkedHashMap:
      TreeSet:
      HashSet:
      ```
    
      ```
      An object that maps keys to values. A map cannot contain duplicate keys; each key can map to at most one value. In order to make it fast to locate the key-value pair in Big O one time. **Hahsmap internally uses an array of linkedlist**. Each element in this array is a bucket. Hashmap uses the **hashcode()** method to calculate the **index** of the target bucket. After finding the bucket, it uses the **equals()** method to check if there is duplicate key. if the operation is get(), then it will return the key-value paire, if it is put(), it will overwrite the key-value with the new value.If there are two keys having the same hashCode(), due to the nature of hashmap, those two keys will use the same bucket. this is **hash collision**.
      ```
    
      
    
    - equals() hashcode()
    
      ```
      equals(): == by defualt, need override to compare by vales/attribtues.
      hashcode(): if two object equals, then they should have the same hashcode, so we need to overwrite the hashcode() method to contains all the values/attributes.
      ```
    
      
1. Multithreading
    - ConcurrentHashMap / Collections.synchronizedMap(xxxx) -> synchronzedMap / hashtable(X)
    
      ```
      multi-thread safe HashMap, use in high concurrecy situation.将ConcurrentHashMap容器的数据分段存储，每一段数据分配一个Segment（锁），当线程占用其中一个Segment时，其他线程可正常访问其他段数据。
      ```
    
      ```
      SynchronizedMap的读写就加锁 类似hashtable
      ```
    
      ```
      hashtable vs hashmap
      hashtable multithreading safe, not recommaned
      hashtable slow and do not have null value.
      ```
    
    - Synchronized(lock){....}
    
      ```
      **Synchorzied**: used on method, coupling and slow, the execuation will be in a random order (?). In other words, use synchorized with notifyAll( ) cannot contorl the order of thread execution.
      ```
    
      
    
    - Lock interface
    
      ```
      **Lock:** a interface that lock() the resource for the thread to use, will unlock()  after use. wait will release lock, while sleep will not.
      
      **synchronized**: object.wait(), object.notify()/notifyAll(), execuation with random order based on OS
      
      **Lock + condition**: condition.await(), condition.signal()/signalAll(), execution with assigned order based on programmers requiremen
      ```
    
      ```
      join(): 让thread按顺序进行 当前thread结束，才运行下一个thread。
      ```
    
      
1. Java 8
    - Stream -> terminal operation(foreach, collection) / intermediate operation(filter, map, reduce, flatmap, sorted)
    
      ```
      A stream does not store data and, in that sense, is not a data structure. It also never modifies the underlying data source.This functionality – java.util.stream – supports functional-style operations on streams of elements, such as map-reduce transformations on collections.
      
      forEach：it loops over the stream elements, calling the supplied function on each element. TERMINAL
      map: produces a new stream after applying a function to each element of the original stream NON-TERMINAL
      collect: its one of the common ways to get stuff out of the stream once we are done with all the processing TERMINAL
      filter： this produces a new stream that contains elements of the original stream that pass a given test (specified by a Predicate). NON-TERMINAL
      reduce -  produce one single result from a sequence of elements, by repeatedly applying a combining operation to the elements in the sequence.一个聚合方法，它可以把一个Stream的所有元素按照聚合函数聚合成一个结果。TERMINAL
      ```
    
      
    
    - Functional Interface (Function, Predicate, Comparator,...)
    
      ```
      Functional interface is a kind of interface that has **only 1 abstract method**.
      
      Functional Interface是指带有 @FunctionalInterface 注解的interface。它的特点是其中只有一个子类必须要实现的abstract方法。如果abstract方法前面带有default关键字，则不做计算。其实这个也很好理解，因为Functional Interface改写成为lambda表达式之后，并没有指定实现的哪个方法，如果有多个方法需要实现的话，就会有问题。
      
      function<T,R>: apply(T) method 作用：使用传入的lambda expression 对传入数据T（一般是collection)进行操作，返回结果R。有进有出
      Consumer<T>: accept(T) method 作用：接受传入参数T，不返回。只进不出，performs the operation on the given argument(print?)
      Predicate<T>: test(T) method 作用：使用传入的lambda expression 对传入数据T（一般是collection)进行判断 返回真假
      Supplier<T>： get() method 作用：不传入参数，返回一个指定的T值（类如randomInt）不劳而获
      
      ```
    
      
    
    - Method References (Class::method)
    
      ```
      Method references are a special type of lambda expressions. They're often used to create simple lambda expressions by referencing existing methods.
      
      Reference to a static method	ContainingClass::staticMethodName
      Reference to an instance method of a particular object containingObject::instanceMethodName
      Reference to an instance method of an arbitrary object of a particular type	ContainingType::methodName
      Reference to a constructor	ClassName::new
      
      ```
    
    - Lambda Expression
    
      ```
      one line statement, a simpified anonymous method. can be used as an argument in the functional interface. 
      ```
    
      
    
    - CompletableFuture(non-blocking Future, vs Future)
    
    - LocalDateTime Api
    
      ```
      LocalDateTime is an immutable date-time object that represents a date-time, often viewed as year-month-day-hour-minute-second.
      LocalDate d = LocalDate.now(); // 当前日期
      LocalTime t = LocalTime.now(); // 当前时间
      LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
      
      LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
      LocalDate d = dt.toLocalDate(); // 转换到当前日期
      LocalTime t = dt.toLocalTime(); // 转换到当前时间
      ```
    
      
1. JVM
    - Heap vs Stack
    
      ```
      Heap Area: It is a shared runtime data area and stores the **actual object** in a memory. It is instantiated during the virtual machine startup. There exists one and only one heap for a running JVM process.
      
      Method Area: It is a **logical part** of the heap area and is created on virtual machine startup. for Metadata? string pool?
      
      Stack:A stack is created at the same time when a **thread** is created and is used to store data and partial results which will be needed while returning value for method and performing dynamic linking.Needs not to be contiguous 连续. 
      
      Native Method Stack: A Native Method Stack stores similar data elements as a JVM Stack and it is used to help executing native (non-Java) methods. such as: start0 in thread.start();
      
      PC Registers: Each **JVM thread** which carries out the task of a specific method has a **program counter register** associated with it. The non native method has a PC which stores the address of the available JVM instruction whereas in a native method, the value of program counter is undefined. PC register is capable of storing the return address or a native pointer on some specific platform.
      ```
    
      
1. Garbage Collection
1. Design Patterns
   - Singleton, factory......
   
     ```
     **creational patterns:** hiding the creation logic, seprating the creation and using of objects. singleton pattern, facttory patter, builder pattern
     
     **strucutral pattern:** concern about class and object composition, using simple objects and combined into complex structure. proxy patter
     
     **behavioral patter:** concern the behaviour of the object, or to say concern about the communication between objects, observer pattern
     
     ### Singleton (important)
     
     only one instance is created. It can be used as logger, driver objects, caching, thread pool and so on.
     
     pro: only one instance means we do not need to create and destory multi instances, which decrease the usage of memory.
     ?con: conflic with single responsity, a class should concern about its inside logic, not about how it should be instanced
     
     use: when only need one global instance
     
     ### Factory
     
     we create objects without exposing the creation logic to the client and refer to newly created objects using a common interface.
     
     pro:to create one object, client only need to know the name.if we want add a new product, just need to add one more class.
     		
     con:every time a new product need a new class, it will add the complexity to the system. and it couples the system to the class.
     
     use: we need some object but we only know the name. Hibernate can just change the 方言 and dirver to connet different database.
     
     ### Builder
     
     builder is a creational design pattern that lets you construct complex objects step by step.
     Instead of declare different type of constructors, the builder pattern can build the object with input features.
     
     pro: easy to build and expandation, same process but different instance.
     con: the product shoud share similiaries, and if there will be a lot of classes created if the changes are complex.
     
     use: when the objects has complex inside logics and contains more than one attribteds.
     
     
     ### Proxy
     
     provide substitute for another object, control the access to the original objects, perform some operations before and after request/response(AOP?)
     
     static: create before invoke
     dyanamic: create when invoke during runtime.
     
     pro: separete the proxy and real object, decoupling the system,protect the orignal object.
     con:lower process speed and increate the comlexity of the system.
     
     use: AOP
     ```
   
     

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

    ```
    A servlet is simply a class which responds to a particular type of network request - most commonly an HTTP request.Basically servlets are usually used to implement web applications .
    ```

    - Filter(pre-post process)

      ```
      A Servlet filter is an object that can intercept HTTP requests targeted at your web application. 
      A filter is an object that is invoked at the preprocessing and postprocessing of a request. It is mainly used to perform filtering tasks such as conversion, logging, compression, encryption and decryption, input validation etc
      ```

    - doGet/doPost

      ```
      You should use doGet() when you want to intercept on HTTP GET requests. You should use doPost() when you want to intercept on HTTP POST requests.
      ```

    - DispachterServlet

      ```
      The job of the DispatcherServlet is to take an incoming URI and find the right combination of handlers (generally methods on Controller classes) and views (generally JSPs) that combine to form the page or resource that's supposed to be found at that location.
      
      DispatcherServlet is essentially a Servlet (it extends HttpServlet ) whose primary purpose is to handle incoming web requests matching the configured URL pattern.
      ```

1. Three-Tiers Architecture
    - Web / service-business / dao -repository
    - Interface loose couple

## Web Services
1. RESTful
    
    ```
    REST, or REpresentational State Transfer, is an architectural style for providing standards between computer systems on the web, making it easier for systems to communicate with each other.
    Restful Web Services is **a lightweight, maintainable, and scalable service that is built on the REST architecture**. 
    ```
    
    - HTTP JSON
    
      ```http
      HTTP/1.1 200 OK                                 //状态行
      Date: Sat, 31 Dec 2005 23:59:59 GMT							//响应头
      Content-Type: text/html;charset=ISO-8859-1
      Content-Length: 122
      
      ＜html＞                                        //响应正文
      ＜head＞
      ＜title＞http＜/title＞
      ＜/head＞
      ＜body＞
      ＜!-- body goes here --＞
      ＜/body＞
      ＜/html＞
      ```
    
      ```http
      GET/sample.jspHTTP/1.1               // 请求方法 URI协议/版本
      Accept:image/gif.image/jpeg,*/*			// Request Header
      Accept-Language:zh-cn
      Connection:Keep-Alive
      Host:localhost
      User-Agent:Mozila/4.0(compatible;MSIE5.01;Window NT5.0)
      Accept-Encoding:gzip,deflate
      
      username=jinqiao&password=1234       //Request body
      ```
    
      
    
      ```
      HTTP body:json
      ```
    
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
    
      ```
      sleth:发牌照,trace id, span id
      zipkin:记录牌照行为(timing data)
      ```
    
      ```
      Spring Cloud Sleuth is used to generate and attach the trace id, span id to the logs so that these can then be used by tools like Zipkin and ELK for storage and analysis. Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures.
      ```
    
    - Zuul (APIGateway)
    
      ```
      advantage:
      ```
    
      
    
    - Config Server
    
    - Fault tolerance - Resilience4j(retry-cache-fallback-circuitbreaker)
    
      ```
      Resilience4j is a lightweight, easy-to-use fault tolerance library.
      retry: try to reconnect in a certain time.Caller A resend the request and see if success, may be connect to other server provide B service.
      cache: B is not avaiable, return previous data saved in cache.
      fallback: execute after ciruitbreaker
      ciruitbreaker: when service B is slow/not available, the caller A may want to shut down service B if the rate is lower than the setting value, the fallback to call another service as replacement.
      ```
    
      

## Spring Family
1. IOC, DI
    - Loose coupling - @Autowired, @Controller, Service, Repository, Bean
1. AOP
    - PointCut, Advice, Aspect, Joinpoint
    
      ```
      Aspect-Oriented Programming entails包含 breaking down program logic into distinct parts called concerns. The functions that span multiple points of an application are called **cross-cutting concerns** and these cross-cutting concerns are conceptually separate from the application's business logic. 
      
      Dependency Injection helps you **decouple your application objects** from each other and AOP helps you **decouple cross-cutting concerns** from the objects that they affect.
      
      **Advice** 通知  This is the actual action to be taken either before or after the method execution.This is **an actual piece of code** that is invoked during the program execution by Spring AOP framework. 什么时候发生？
      
      **jointpoint**  连接点 **runtime concept, not real code,**  This represents a point in your application where you can plug-in the AOP aspect. 在哪里发生？(code location)  
      
      **pointcut,** 切入点 This is a set of one or more join points where an advice should be executed.（连接点的集合？） You can specify pointcuts using expressions or patterns as we will see in our AOP examples. 发生在谁身上？
      
      **Aspect** 切面是通知和切入点的结合。This is a module which has a set of APIs providing cross-cutting requirements.
      
      **target** 目标 The object being advised by one or more aspects. This object will always be a proxied object, also referred to as the advised object.
      
      **Weaving** Weaving is the process of linking aspects with other application types or objects to create an advised object. This can be done at compile time, load time, or at runtime.
      ```
    
      
    
    - use cases - secutiry, cache, audit, log, transaction management @Transactional
1. MVC
    - @Controller, @RestController
    - @RequestMapping(URL,Method) -> method
    - input @PathVarible @RequestParam @RequestHeader @RequestBody
    - output @ResponseBody - Json/xml -> Jackson and Jax-b
    - @Validated bean validation
    - @ExceptionHandler -> Exception
    - @ControllerAdvice -> class -> @ExceptionHandler -> global exception handler(?)
1. Data
    - simplify the dao implementation.
    - JPA  MongoDB
1. Security
    - Configuration
    
    - @Secured @Pre-Authorized
    
      ```
      @Secured:The Spring Method Level security is used in Spring Boot applications that have user Roles and Authorities configured.
      @Pre-Authorized:This annotation contains a Spring Expression Language (SpEL) snippet that is assessed to determine if the request should be authenticated.
      ```
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

   ![CI/CD Flow](https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/ci-cd-flow-desktop.png?itok=2EX0MpQZ)

   ```
   continuous integration is an automation process for developers, Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other.
   
   Continuous deliver a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code.
   
   Continuous development utomatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery. It builds on the benefits of continuous delivery by automating the next stage in the pipeline.
   
   
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

## Add

What will you do after getting a new task?
What if the project is running slow?
How to fix an error or exception occured on the server?
How do you do code review? or what kind of code is good code?
What are the aspects to consider to start a new project if you are the technical lead?
Do you have a complete picture of SDLC?
What is the typical day of a software engineer?

## Project



1. what is a rest controller?

   ```
   RestController = @Controller + @ResponseBody
   
   ```

   

2. how to get pathvariable? what is a @requestparam?

3. what is the ResponseEntity?

4. what is a logger, why we use a logger?

5. what is constrcutor injection? what benefits? what problem?

6. what is a thread pool? 

7. what is the conpletebFuture? what benefit? what usage?

   ```java
   //future: to get the result of the async result, use future.get(); 虽然Future以及相关使用方法提供了异步执行任务的能力，但是对于结果的获取却是很不方便，只能通过阻塞或者轮询的方式得到任务的结果。
   //future.get() will block the main thread until the result is return, to solve this, we ues completablefuture
   //completablefuture will not block the thread, it will provide a return method or catch exception.
   
   //run a new thread; exectuor:
   public static CompletableFuture<Void> 	runAsync(Runnable runnable, Executor executor)
   //当CompletableFuture的计算结果完成，或者抛出异常的时候，我们可以执行特定的Action。
   //run after complete: 方法不以Async结尾，意味着Action使用相同的线程执行
   public CompletableFuture<T> whenComplete(BiConsumer<? super T,? super Throwable> action)
   //catch exception:
   exceptionally(Function<Throwable,? extends T> fn)
   
       
   //vairable used in lambda should be final?
   ```

   

8. what does @transactional do?

9. what is a report VO?

10. what is an inputstream?

11. what is an application poerperties?

12. what is eureka.instance.prefer-ip-address mean?

13. what is eureka.server.wait-time-in-ms-when-sync-empty mean?

14. what is eureka? what is ribbon?@loadbalance?irule()?

15. what is config service? what advantage?

16. what is bootstrap.yml?

17. what is zuul service? what benefit?

18. why use this name? 命名原则？

19. %s : insert a string here

20. Hystrix? 服务中断 服务降级