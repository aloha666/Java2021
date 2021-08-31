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
      ArrayList:use dynamic arrary to store, slow, may need to adjust the array,better for storing and accessing data.
      LinkedList: use double linked list to store, fast, better for manipulating data.
      Array: fixed length datatype.
      HashMap: dynamic array + linkedlist/red-black tree
      TreeMap: red-black tree
      LinkedHashMap: DLL+hashmap
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

    ```
    system.gc(), runtime.getruntime.gc().
    ```

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

```
sql relationa
```



1. Relationship
    - 1-1, m-1, m-m
1. Joins
    - outer inner 
1. Query
    - select from where group by having order by;
    - join ... on...
1. ORM Object Relation Mapping
    - JPA / Hibernate
    - Lazy Loading/Fetch  eager
    - Cascade (ALL, NONE, MERGE, PERSIST)
1. SQL vs NoSQL
1. MongoDB - JSON- Schemaless - Replica, Sharding

## Web
1. HTTP
    - Request (Method) GET PUT POST DELET (idempotent)
    
      ```
      provider consumer
      ```
    
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
    
    ```
    SOAP: XML
    ```
    
    - XML
    - Complicated
    
1. Microservices
    - popular
    
    - Docker. cloud. flexble, scalable
    
      ```
      Docker is a container to bind the application and running enviorment, build once and run everywhere, differen from VM, it is light weighted and do not require a guest OS, it is isolated at the application level, and can be build as to an image and upload to a repostiry.
      ```
    
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

```
Spring vs Springboot
1Opinionated ‘starter' dependencies to simplify the build and application configuration
Embedded server to avoid complexity in application deployment
Metrics, Health check, and externalized configuration
Automatic config for Spring functionality – whenever possible
```



1. IOC, DI
    - Loose coupling - @Autowired, @Controller, Service, Repository, Bean
    
      ```
      The @SpringBootApplication annotation is equivalent to using @Configuration, @EnableAutoConfiguration, and @ComponentScan
      
      @EnableAutoConfiguration: enable Spring Boot’s auto-configuration mechanism
      @ComponentScan: enable @Component scan on the package where the application is located (see the best practices)
      @Configuration: allow to register extra beans in the context or import additional configuration classes
      
      
      The @EnableAutoConfiguration annotation enables Spring Boot to auto-configure the application context. Therefore, it automatically creates and registers beans based on both the included jar files in the classpath and the beans defined by us.
      
      The Application Context is Spring's advanced container. Similar to BeanFactory, it can load bean definitions, wire beans together, and dispense beans upon request. 
      
       It adds more enterprise-specific functionality such as the ability to resolve textual messages from a properties file and the ability to publish application events to interested **event listener**s.
      ```
      
       
      
      ```
      Bean Type: singleton, prototype, session, global session
      ```
      
      
    
1. AOP
    - PointCut, Advice, Aspect, Joinpoint
    
      ```
      Aspect-Oriented Programming entails包含 breaking down program logic into distinct parts called concerns. The functions that span multiple points of an application are called **cross-cutting concerns** and these cross-cutting concerns are conceptually separate from the application's business logic. 
      
      Dependency Injection helps you **decouple your application objects** from each other and AOP helps you **decouple cross-cutting concerns** from the objects that they affect.
      
      **Advice** 通知  This is the actual action to be taken either before or after the method execution.This is **an actual piece of code** that is invoked during the program execution by Spring AOP framework. 什么时候发生？
      
      **joinpoint**  连接点 **runtime concept, not real code,**  This represents a point in your application where you can plug-in the AOP aspect. 在哪里发生？(code location)  
      
      **pointcut,** 切入点 This is a set of one or more join points where an advice should be executed.（连接点的集合？） You can specify pointcuts using expressions or patterns as we will see in our AOP examples. 发生在谁身上？
      
      **Aspect** 切面是通知和切入点的结合。This is a module which has a set of APIs providing cross-cutting requirements.
      
      **target** 目标 The object being advised by one or more aspects. This object will always be a proxied object, also referred to as the advised object.
      
      **Weaving** Weaving is the process of linking aspects with other application types or objects to create an advised object. This can be done at compile time, load time, or at runtime.
      ```
    
      ```
      advice: around, before, after returnning, after throw,after
      ```
    
      
    
    - use cases - secutiry, cache, audit, log, transaction management @Transactional
    
1. MVC
    - @Controller, @RestController
    
    - @RequestMapping(URL,Method) -> method
    
    - input @PathVarible @RequestParam @RequestHeader @RequestBody
    
    - output @ResponseBody - Json/xml -> Jackson and Jax-b
    
    - @Validated bean validation
    
      ```
      //@valid : method level, or member atteubute
      //@validated: group level
      
      @Validated：用在方法入参上无法单独提供嵌套验证功能。不能用在成员属性（字段）上，也无法提示框架进行嵌套验证。能配合嵌套验证注解@Valid进行嵌套验证。**不能用在类的属性上**
      
      @Valid：用在方法入参上无法单独提供嵌套验证功能。能够用在成员属性（字段）上，提示验证框架进行嵌套验证。能配合嵌套验证注解@Valid进行嵌套验证。
      ```
    
    - @ExceptionHandler -> Exception
    
      ```
      @ExceptionHandler:controller层面异常处理, 优先级最高
      实现HandlerExceptionResolver接口，优先级最后
      @controllerAdvice+@ExceptionHandler，优先级第二
      
      优先级中被一个捕获了就不在执行其他的。
      
      
      ```
    
      
    
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
   
    ```
    Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems.
    Its a unmberlla that covers all the needed tool for MS.
    ```
    
    
    
    - Microservice 
    
      ```
      Microservice architecture – a variant of the service-oriented architecture structural style – arranges an application as a collection of loosely-coupled services. In a microservices architecture, services are fine-grained and the protocols are lightweight
      ```
    
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
   
   QA:Quality Assuance
   UAT: actual users test the software to validate that it is performing according to the required real-life scenarios.
   ```

1. GIT

1. Maven

   ```
   Apache Maven is a **software project management and comprehension tool**.
   
   ​	src - main - java/res; src-main-test; pom file
   
   ​	POM stands for "Project Object Model". The POM contains all necessary information about a project, as well as configurations of plugins to be used during the build process.
   
   ​	**life cycle(compile, test,package, install..)**
   
   ​	validate - compile -test - package - verify (Integration test) - install - deploy
   
   ​	**dependency management**
   
   ​	**plugins**
   ```

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
   continuous integration is an automation process for developers, Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other解决bug finding问题.
   
   Continuous deliver a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to 部署到实时生产环境中 a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and 运维团队 business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code.
   
   Continuous deployment automatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery解决部署缓慢问题. It builds on the benefits of continuous delivery by automating the next stage in the pipeline.
   
   
   CI: 应用代码的新更改会定期构建、测试并合并到共享存储库中. 解决在一次开发中有太多应用分支，从而导致相互冲突的问题。
   CDelivery: 持续交付通常是指开发人员对应用的更改会自动进行错误测试并上传到存储库（如 GitHub 或容器注册表），然后由运维团队将其部署到实时生产环境live production environment 中。这旨在解决开发和运维团队之间可见性及沟通较差的问题。因此，持续交付的目的就是确保尽可能减少部署新代码时所需的工作量。
   CDeployment: 自动将开发人员的更改从存储库发布到生产环境，以供客户使用。它主要为了解决因手动流程降低应用交付速度，从而使运维团队超负荷的问题。持续部署以持续交付的优势为根基，实现了管道后续阶段的自动化.
   
   ```

1. Linux 

## Security
1. Encode / Encryption / Hash 

1. JWT

   ```
   Jason Web Token, JSON Web Token is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims. The tokens are signed either using a private secret or a public/private key. 
   用户第一次登录服务器
   在服务器上为该用户生成一个token
   该token存储在用户的浏览器中
   用户随后发出的任何请求都将包含该token
   如果用户退出登录或token失效（如过期），则该token将被销毁。用户必须重新登录以获得新的toke
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
    Authentication confirms that usersname and password. Authorization gives those users permission to access a resource.
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

```
Cause No. 1: You have one big thread
Cause No. 2: Your database is !@#$
```

How to fix an error or exception occured on the server?
How do you do code review? or what kind of code is good code?
What are the aspects to consider to start a new project if you are the technical lead?
Do you have a complete picture of SDLC?
What is the typical day of a software engineer?

## Project

0. what is restTemplate? what is openFeign?

   ```
   Rest Template is used **to create applications that consume RESTful Web Services**.
   
   ### OpenFeign Client
   
   FeignClient is **a library for creating REST API clients in a declarative way**. progamm	based on interface.
   
   Feign allows programmer to abstract the mechanics of calling a REST service. Once you configure and annotate the Feign interface, you can call a REST service by making a simple Java function call. The actual implementation of making a REST call is handled at runtime by Feign. This means that the implementation can be configured without changing your business logic code.
   
   advantage: not hard coding url, So there is **no need to write any unit test** as there is no code to test in the first place
   
   disadvantage:more effore when create
   ```

1. what is a rest controller?

   ```
   @RestController = @Controller + @ResponseBody
   @RequestBody and @ResponseBody annotations are used to bind the HTTP request/response body with a domain object in method parameter or return type
   ```
   
2. how to get pathvariable? what is a @requestparam?

   ```
   @pathvariable: extract values from the URI path:
   @requestparam: extract values from the query string
   ```

   ```java
   //http://localhost:8080/foos/abc
   // ----
   //ID: abc
   
   @GetMapping("/foos/{id}")
   @ResponseBody
   public String getFooById(@PathVariable String id) {
       return "ID: " + id;
   }
   ```

   ```java
   //http://localhost:8080/foos?id=abc
   //----
   //ID: abc
   @GetMapping("/foos")
   @ResponseBody
   public String getFooByIdUsingQueryParam(@RequestParam String id) {
       return "ID: " + id;
   }
   ```

3. what is the ResponseEntity?

   ```java
   //ResponseEntity represents the whole HTTP response: status code, headers, and body. As a //result, we can use it to fully configure the HTTP response.
   
   @GetMapping("/age")
   ResponseEntity<String> age(
     @RequestParam("yearOfBirth") int yearOfBirth) {
    
       if (isInFuture(yearOfBirth)) {
           return new ResponseEntity<>(
             "Year of birth cannot be in the future", 
             HttpStatus.BAD_REQUEST);
       }
   
       return new ResponseEntity<>(
         "Your age is " + calculateAge(yearOfBirth), 
         HttpStatus.OK);
   }
   
   ```

4. what is a logger, why we use a logger?

   ```
   In Java, logging is an important feature that **helps developers to trace out the errors**. In Spring, the log level configurations can be set in the application. properties file which is processed during runtime. Spring supports 5 default log levels, **ERROR , WARN , INFO , DEBUG , and TRACE** , with INFO being the default log level configuration.
   
   print:sync, only in console, 
   logger:aync, print to define files, have differen levels 
   ```

5. what is constrcutor injection? what benefits? what problem?

   ```
   **Field Injection** throw everything in field-normal used, use @Autowired
   
   Pro: Easy to use, no constructors or setters required.Can be easily combined with the constructor and/or setter approach
   
   Con: Less control over object instantiation. A number of dependencies can reach dozens until you notice that something went wrong in your design. No immutability — the same as for setter injection.
   
   
   **Constructor Injection** use a constructor to inject objects,@Autowired can be omitted. (recommended)
   
   Pro: 1.**easy for test**; 2.can add check lines since it's method (same as setter injection)  
   
   Con: may have **cycle situation** A->B->C->A, can not create any object if autowire in constructor.
   
   **Setter Injection** use@Autowired and setter
   
   Pro: Flexibility in dependency resolution or object reconfiguration, it can be done anytime. Plus, this freedom solves the circular dependency issue of constructor injection.
   
   Con:Null checks are required, because dependencies may not be set at the moment. more error-prone and less secure than constructor injection due to the possibility of overriding dependencies.
   ```

6. what is a thread pool? 

   ```
   A thread pool is a pool threads that can be "reused" to execute tasks, so that each thread may execute more than one task. A thread pool is an alternative to creating a new thread for each task you need to execute.
   ```

7. what is the completebFuture? what benefit? what usage?

   ```java
   //https://blog.csdn.net/qq_31865983/article/details/106137777
   //future: to get the result of the async result, use future.get(); 虽然Future以及相关使用方法提供了异步执行任务的能力，但是对于结果的获取却是很不方便，只能通过阻塞get()或者轮询的方式isdone(得到任务的结果。
   //future.get() will block the main thread until the result is return, to solve this, we ues completablefuture
   //completablefuture will not block the thread, it will provide a return method or catch exception.
   
   //run a new thread; exectuor:
   public static CompletableFuture<Void> 	runAsync(Runnable runnable, Executor executor)
   //当CompletableFuture的计算结果完成，或者抛出异常的时候，我们可以执行特定的Action。
   //run after complete: 方法不以Async结尾，意味着Action使用相同的线程执行
   public CompletableFuture<T> whenComplete(BiConsumer<? super T,? super Throwable> action)
   //catch exception:
   exceptionally(Function<Throwable,? extends T> fn)
   //when complete 执行完成后会将执行结果和执行过程中抛出得到异常传入回调方法 如果无异常则异常为null
   whenComplete(Result T, Exception e)
     
   //vairable used in lambda should be final?
   ```

   ```
   Execute: -supplyAsync: 带返回值
   		-runAsync:不带返回值
   Callback: -thenApply: 某个任务完成后执行的操作 带返回值
   		 -thenApplyAync: 由线程池再分配
   		 
   		 -thenAccept:接受上一个任务的返回值，但无返回
   		 -thenRun: 没有入参， 也不返回
   		 
   		 -exceptionally:某个任务执行异常时执行的回调方法
   		 
   		 -whenComplate:某个任务执行完成后执行的回调方法，无返回
   		 -handle:同whenComplete但是有返回值
   ```

8. what does @transactional do?

   ```
   how is transaction AOP? The annotation will tell Spring to start a transaction before the method run and then close it after the method finished.
   
   It can start a new transaction and commit or rollback if runtime exception/error happen (check exception will be handled), will not automatically rollback for checked exception.
   ```

   | 事务隔离级别                                                 | 说明                                                |
   | ------------------------------------------------------------ | --------------------------------------------------- |
   | @Transactional(isolation = Isolation.READ_UNCOMMITTED)       | 读取未提交数据(会出现脏读， 不可重复读)，基本不使用 |
   | @Transactional(isolation = Isolation.READ_COMMITTED)(SQLSERVER默认) | 读取已提交数据(会出现不可重复读和幻读)              |
   | @Transactional(isolation = Isolation.REPEATABLE_READ)        | 可重复读(会出现幻读)                                |
   | @Transactional(isolation = Isolation.SERIALIZABLE)           | 串行化                                              |

   | 事务传播行为                                     | 说明                                                   |
   | ------------------------------------------------ | ------------------------------------------------------ |
   | @Transactional(propagation=Propagation.REQUIRED) | 如果有事务， 那么加入事务， 没有的话新建一个(默认情况) |

9. what is @joincolumn?@jointable?

   ```
   @joincolumn: foreign key (one to one) primary key(one to many)
   @jointable(joinclumn=pk,inversejoincolumn=f): associate table
   ```

10. what is an inputstream?

    ```
    InputStream , represents an ordered stream of bytes. In other words, you can read data from a Java InputStream as an ordered sequence of bytes. This is useful when reading data from a file, or received over the network.
    ```

11. what is an application poerperties?

    ```
    Properties files are used to keep 'N' number of properties in a single file to run the application in a different environment.
    ```

12. what is eureka.instance.prefer-ip-address mean?

    ```
    preferIpAddress to true and, when the application registers with eureka, it uses its IP address rather than its hostname
    ```

13. what is eureka.server.wait-time-in-ms-when-sync-empty mean?

    ```
    When the eureka server is started, you do not wait for the instance registration information from the peer node. 同步为空时 等待时间
    ```

14. what is eureka? what is ribbon?@loadbalance?irule()?

    ```
    Microservice architecture – a variant of the Service-Oriented Architecture structural style – arranges an application as a collection of loosely-coupled services. In a microservices architecture, services are fine-grained and the protocols are lightweight
    ```

    ```
    Eureka: 遵循AP原则，is a service registry, means , it knows which ever microservices are running and in which port. Eureka is deploying as a sperate application and we can use @EnableEurekaServer annotation along with @SpringBootApplication to make that app a eureka server. So our eureka service registery is UP and running. From now on all microservices will be registered in this eureka server by using @EnableDiscoveryClient annotation along with @SpringBootAPplication in all deployed microservices.
    Eureka Server：提供服务的注册与发现
    Service Provider：服务生产方，将自身服务注册到Eureka中，从而使服务消费方能找到
    Service Consumer：服务消费方，从Eureka中获取注册服务列表，从而找到消费服务
    
    Eurka自我保护机制：好死不如赖活着
    一句话总结就是：某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！
    如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制。
    
    Eureka保证的是AP
    
    ​ Eureka看明白了这一点，因此在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台Eureka还在，就能保住注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，Eureka还有之中自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：
    
    Eureka不在从注册列表中移除因为长时间没收到心跳而应该过期的服务
    Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上 (即保证当前节点依然可用)
    当网络稳定时，当前实例新的注册信息会被同步到其他节点中
    ```

    ```
    Ribbon: Client/Consumer side load balancer
    给restTemplate加@loadbalanced,将获取服务地址改成application name
    
    Spring Cloud Ribbon 是基于Netflix Ribbon 实现的一套客户端负载均衡的工具。
    
    集中式LB Server Side Load Balancer?
    即在服务的提供方和消费方之间使用独立的LB设施，如Nginx(反向代理服务器)，由该设施负责把访问请求通过某种策略转发至服务的提供方！
    进程式 LB
    将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
    Ribbon 就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！
    
    同一服务的application name一样 instance id地址不同
    通过一个IRule Bean制定规则，若不制定，则是轮流规则
    
    ```

15. what is config service? what advantage?

    ```
    Spring Cloud Config为分布式系统中的外部配置提供服务器和客户端支持。使用Config Server，您可以在所有环境中管理应用程序的外部属性。
    ​ spring cloud config 为微服务架构中的微服务提供集中化的外部支持，配置服务器为各个不同微服务应用的所有环节提供了一个中心化的外部配置。
    
    Central configuration server provides configurations (properties) to each micro service connected. As mentioned in the above diagram, Spring Cloud Config Server can be used as a central cloud config server by integrating to several environments.
    ```

16. what is bootstrap.yml?

    ```
    bootstrap.yml is loaded before application.yml.
    It is typically used for the following:
     -when using Spring Cloud Config Server, you should specify spring.application.name and spring.cloud.config.server.git.uri inside bootstrap.yml
     -some encryption/decryption information
    ```

17. what is zuul service? what benefit?

    ```
    Zull包含了对请求的路由(用来跳转的)和过滤两个最主要功能：Router & Filter
    
    ​ 其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础，而过滤器功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础
    
    @EnableZuulProxy
    
    Zuul Server is an API Gateway application. It handles all the requests and performs the dynamic routing of microservice applications. It works as a front door for all the requests. It is also known as Edge Server. Zuul is built to enable dynamic routing, monitoring, resiliency, and security.
    ```

    ```java
    @EnableZuulProxy
    @SpringBootApplication
    public class RoutingAndFilteringGatewayApplication {
    
      public static void main(String[] args) {
        SpringApplication.run(RoutingAndFilteringGatewayApplication.class, args);
      }
    
      @Bean
      public SimpleFilter simpleFilter() {
        return new SimpleFilter();
      }
    
    }
    ```

    ```
    Zuul大部分功能都是通过过滤器来实现的。Zuul中定义了四种标准过滤器类型，这些过滤器类型对应于请求的典型生命周期。
    (1) PRE：这种过滤器在请求被路由之前调用。我们可利用这种过滤器实现身份验证、在集群中选择请求的微服务、记录调试信息等。
    (2) ROUTING：这种过滤器将请求路由到微服务。这种过滤器用于构建发送给微服务的请求，并使用Apache HttpClient或Netfilx Ribbon请求微服务。
    (3) POST：这种过滤器在路由到微服务以后执行。这种过滤器可用来为响应添加标准的HTTP Header、收集统计信息和指标、将响应从微服务发送给客户端等。
    (4) ERROR：在其他阶段发生错误时执行该过滤器。
    ```

18. why use this name? 命名原则？

    ![img](https://pic4.zhimg.com/80/v2-59c4d62185a8ff082bf331e361f51c2f_720w.jpg)

19. %s : insert a string here

20. Hystrix? 服务中断 服务降级

    ```
    微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”，如果扇出的链路上某个微服务的调用响应时间过长，或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”。
    Hystrix 能够保证在一个依赖出问题的情况下，不会导致整个体系服务失败，避免级联故障，以提高分布式系统的弹性。
    
    CicketBreaker “断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控 (类似熔断保险丝) ，向调用方返回一个服务预期的，可处理的备选响应 (FallBack) ，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。
    
    @EnableCircuitBreaker
    @HystrixCommand(fallbackMethod = "fallbackMethod")
    ```

    ```java
    //main.class
    @SpringBootApplication
    @EnableCircuitBreaker
    public class MainClientApplication {
        @LoadBalanced
        @Bean
        public RestTemplate restTemplate() {
            return new RestTemplate();
        }
    
        public static void main(String[] args) {
            SpringApplication.run(MainClientApplication.class, args);
        }
    
    }
    //serviceimpl
    @HystrixCommand(fallbackMethod = "fallbackMethod", commandProperties = {@HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds", value = "2000")})
    private void sendDirectRequests(ReportRequest request) {
    
        }
    //fallback method
    public void fallbackMethod(ReportRequest request){
            log.error("Something wrong to generate record in sync, use Async instead");
            generateReportsAsync(request);
        }
    ```


```java
@GetMapping("/report")
public ResposenEntiry<GeneralResponse> listReport(){
    return ResponseEntity.ok(new GeneralResponse(resportService.getList()));
}

@DeleteMapping("/report/{id}")
public ResponseEntity<GeneralResponse> deleteById(@pathVariable String id){
    try{
     reportService.deleteById(id);   
    }catch(RequestNotFoundException e){
        return ResponsEntity<>(HttpStatus.BAD_REQUEST);
    }
    return ResponseEntity.ok(newGeneralResponse());
}



```

## Self

Reflection API & Classloader

```
### ClassLoader

The Java ClassLoader is a part of the Java Runtime Environment that dynamically loads Java classes into the Java Virtual Machine.no

### Reflection: 

Reflection is an API which is used to examine or modify the behavior of methods, classes, interfaces at the run time. (是不是不利于encapsulation)

java中class.forName()和classLoader都可用来对类进行加载。

class.forName()前者除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。Returns the Class object associated with the class or interface with the given string name. Invoking this method is equivalent to: Class.forName(className, true, currentLoader)

而classLoader只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。
Invoking this method is equivalent to invoking loadClass(name, false).
```





















