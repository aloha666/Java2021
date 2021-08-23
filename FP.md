# ORM

## 1. advantages of orm  centralized query language connection pool object mapping lazy loading / eager loading relation 1 - m , m - m, m - 1, 1 - 1 cache  criteria query -> dynamic query 

Object-Relational Mapping (ORM) is a technique that lets you query and manipulate data from a database using an object-oriented paradigm.范例 of your preferred programming language.

### advantages of orm

ORM is the approach of taking object-oriented data and mapping to a relational data store

Advantages: speeds-up development. reduce the development cost and time. overcome the specific SQL deafferents.

Disadvantages: developer may lost understand of what code is doing. has tendency趋势 to be slow.

### ?centralized query language

###  connection pool

A connection pool is a standard technique used to maintain long running connections in memory for efficient re-use, as well as to provide management for the total number of connections an application might use simultaneously.

Particularly for server-side web applications, a connection pool is the standard way to maintain a “pool” of active database connections in memory which are reused across requests.

### ?object mapping

###  lazy loading / eager loading 

In eager loading strategy, if we load the data, it will also load up all child table date associated with it and will store it in a memory. But, when lazy loading is enabled, the child table data won't be initialized and loaded into a memory until an explicit call is made to it.

### relation 1 - m , m - m, m - 1, 1 - 1 

1:n means 'one-to-many'; you have two tables, and each row of table A may be referenced by any number of rows in table B, but each row in table B can only reference one row in table A (or none at all).

n:m (or n:n) means 'many-to-many'; each row in table A can reference many rows in table B, and each row in table B can reference many rows in table A.

A 1:n relationship is typically modelled using a simple foreign key - one column in table A references a similar column in table B, typically the primary key. Since the primary key uniquely identifies exactly one row, this row can be referenced by many rows in table A, but each row in table A can only reference one row in table B.

A n:m relationship cannot be done this way; a common solution is to use a link table that contains two foreign key columns, one for each table it links. For each reference between table A and table B, one row is inserted into the link table, containing the IDs of the corresponding rows.

one to one 实现： FrogienKey JoinTable

one to many 实现： FrogienKey JoinTable

many to many 实现： 需要中间表 JoinTable

### cache(first level and second level)

First: open by default, session level, locally, if session closed, cache is gone
Second: close by default,** session factory level, globally, if seesion factory shut down, cache is gone
Query -> first level cache -> second level cache (need open mannually)-> database

### criteria query  条件查询-> dynamic query 

The Criteria API allows us to build up a criteria query object programmatically, where we can apply different kinds of filtration rules and logical conditions. sql语句固定，往里填值？是static还是dynamic?

Dynamic SQL is a programming technique that **enables you to build SQL statements dynamically at runtime**. You can create more general purpose, flexible applications by using dynamic SQL because the full text of a SQL statement may be unknown at compilation. sql语句不固定，需要根据输入生成？

Static or Embedded SQL are **SQL statements in** an application that do not change at runtime and, therefore, can be hard-coded into the application. Dynamic SQL is SQL statements that are constructed at runtime; for example, the application may allow users to enter their own queries.

## 2. hibernate vs jpa 

Hibernate is an implementation of JPA using ORM technical.

As you state JPA is just a specification, meaning there is no implementation. You can annotate your classes as much as you would like with JPA annotations, however without an implementation nothing will happen. Think of JPA as the guidelines that must be followed or an interface, while Hibernate's JPA implementation is code that meets the API as defined by the JPA specification and provides the under the hood functionality.

When you use Hibernate with JPA you are actually using the Hibernate JPA implementation. The benefit of this is that you can swap out Hibernate's implementation of JPA for another implementation of the JPA specification. When you use straight Hibernate you are locking into the implementation because other ORMs may use different methods/configurations and annotations, therefore you cannot just switch over to another ORM.

## 3. session vs entitymanager 

The Session object is **lightweight and designed to be instantiated each time an interaction is needed with the database**. Persistent objects are saved and retrieved through a Session object.

`Session` is a hibernate-specific API, `EntityManager` is a standardized API for JPA. You can think of the `EntityManager` as an adapter class that wraps `Session` (you can even get the `Session` object from an `EntityManager` object via the `getDelegate()` function).

This is not dissimilar to other Java APIs around (for example, JDBC is a standard API, each vendor adapts its product to the API via a driver that implements the standard functions).

## ?4. spring data jpa + dynamic proxy & 

## SimpleJpaRepository.class 

Spring Data JPA aims to **significantly improve the implementation of data access layers** by reducing the effort to the amount that's actually needed. As a developer you write your repository interfaces, including custom finder methods, and Spring will provide the implementation automatically.

A dynamic proxy class is **a class that implements a list of interfaces specified at runtime** such that a method invocation through one of the interfaces on an instance of the class will be encoded and dispatched to another object through a uniform interface.

The SimpleJpaRepository is **the default implementation of Spring Data JPA repository interfaces**. ... We can create a custom base repository class by following these steps: Create a BaseRepositoryImpl class that has two type parameters: The T type parameter is the type of the managed entity.

## ?5. lazy initialization exception 

Hibernate throws the *LazyInitializationException* when it needs to initialize a lazily fetched association to another entity without an **active session context**. That’s usually the case if you try to use an uninitialized association in your client application or web layer. 在读取数据的时候，Session已经关闭,not saved into cache/memory?

## 6. @transactional 

how is transaction AOP? The annotation will tell Spring to start a transaction before the method run and then close it after the method finished.

It can start a new transaction and commit or rollback if runtime exception/error happen (check exception will be handled), will not automatically rollback for checked exception.

![Spring @Transactional原理及使用2](https://res-static.hc-cdn.cn/fms/img/dc9535dea4a39751c8610585dd1fd26f1603795225464.jpg)

Spring事务管理器回滚一个事务的推荐方法是在当前事务的上下文内抛出异常。Spring事务管理器会捕捉任何未处理的异常，然后依据规则决定是否回滚抛出异常的事务。
默认配置下，Spring只有在抛出的异常为运行时unchecked异常时才回滚该事务，也就是抛出的异常为RuntimeException的子类(Errors也会导致事务回滚)。而抛出checked异常则不会导致事务回滚。

**isolation?** default is what? 有几种， 啥意思？

| 事务隔离级别                                                 | 说明                                                |
| ------------------------------------------------------------ | --------------------------------------------------- |
| @Transactional(isolation = Isolation.READ_UNCOMMITTED)       | 读取未提交数据(会出现脏读， 不可重复读)，基本不使用 |
| @Transactional(isolation = Isolation.READ_COMMITTED)(SQLSERVER默认) | 读取已提交数据(会出现不可重复读和幻读)              |
| @Transactional(isolation = Isolation.REPEATABLE_READ)        | 可重复读(会出现幻读)                                |
| @Transactional(isolation = Isolation.SERIALIZABLE)           | 串行化                                              |

**propagation?** required by default 有几种， 啥意思？(不必记，知道require即可)

| 事务传播行为                                          | 说明                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| @Transactional(propagation=Propagation.REQUIRED)      | 如果有事务， 那么加入事务， 没有的话新建一个(默认情况)       |
| @Transactional(propagation=Propagation.NOT_SUPPORTED) | 容器不为这个方法开启事务                                     |
| @Transactional(propagation=Propagation.REQUIRES_NEW)  | 不管是否存在事务，都创建一个新的事务，原来的挂起，新的执行完毕，继续执行老的事务 |
| @Transactional(propagation=Propagation.MANDATORY)     | 必须在一个已有的事务中执行，否则抛出异常                     |
| @Transactional(propagation=Propagation.NEVER)         | 必须在一个没有的事务中执行，否则抛出异常(与Propagation.MANDATORY相反) |
| @Transactional(propagation=Propagation.SUPPORTS)      | 如果其他bean调用这个方法，在其他bean中声明事务，那就用事务。如果其他bean没有声明事务，那就不用事务 |

## 7.  Entity

Entities in JPA are nothing but **POJOs representing data that can be persisted to the database**. An entity represents a table stored in a database. Every instance of an entity represents a row in the table.一个entity一张表



# Rest api 

## 1. http + status + http method  

 **http**:  HTTP stands for Hypertext Transfer Protocol, as a request-response protocol, HTTP gives users a way to interact with web resources such as HTML files by transmitting hypertext messages between clients and servers.

**stateless**: HTTP is a [stateless protocol]. A stateless protocol does not require the [HTTP server] to retain information or status about each user for the duration of multiple requests. However, some [web applications] implement states or [server side sessions] using for instance [HTTP cookies] or hidden [variables] within [web forms].

 **header**:  The HTTP Header **contains information about the HTTP Body and the Request/Response**. 

- [Request headers](https://developer.mozilla.org/en-US/docs/Glossary/Request_header) contain more information about the resource to be fetched, or about the client requesting the resource.
- [Response headers](https://developer.mozilla.org/en-US/docs/Glossary/Response_header) hold additional information about the response, like its location or about the server providing it.
- [Representation headers](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) contain information about the body of the resource, like its [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types), or encoding/compression applied.
- [Payload headers](https://developer.mozilla.org/en-US/docs/Glossary/Payload_header) contain representation-independent information about payload data, including content length and the encoding used for transport.

​				request: request method + protocol version, request header, request body
​				response: response status + protocol version, response header, response body

 **/student/{pathVariable}?requestParam=xxx&..**

how to pass the varibale and prarameters in URL?

## 2. stateless vs stateful(sticky session) 

In Stateless, server is not needed to keep the server information or session details to itself. In stateful, **a server is required to maintain the current state and session information**. In stateless, server and client are loosely coupled and can act independently. In stateful, server and client are tightly bound.

With sticky sessions, **a load balancer assigns an identifying attribute to a user, typically by** issuing a cookie or by tracking their IP details. Then, according to the tracking ID, a load balancer can start routing all of the requests of this user to a specific server for the duration of the session.

## 3. status code   

200 OK, 201 Created, 204 OK with no content  400 bad request, **401 unauthenticated , 403 unauthroized**, 404 not found  500 internal error 

```
100-informational response 收到Web浏览器请求，正在进一步的处理中
200-请求成功 Success The response that is returned is dependent on the request
201-This is the status code that confirms that the request was successful and, as a result, **a 		new resource was created**. (POST)
204-This status code confirms that the server has fulfilled the request but does not need to 			return information.
300- 重定向，需要进一步的操作以完成请求 Redirection
301- Moved Permanently，客户请求的文档在其他地方，新的URL在Location头中给出，浏览器应该自动地访问新的URL。
304-  A 304 Not Modified response code indicates that the requested resource has not been 					modified since the previous transmission. This typically means there is no need to 						retransmit the requested resource to the client, and a cached version can be used, 						instead.
400- client side errors
401- This status code request occurs when **authentication is required** but has failed or not 			been provided.
404- NOT Found，意味着请求中所引用的文档不存在。
403- need accout/necessay permission, need authorization.
409-  a request **conflicts with the current state of the resource**. This is usually an issue 			with simultaneous updates, or versions, that conflict with one another.
410- Resource requested is no longer available and will not be available again.
500- server side error
503- Service Unavailable 服务器由于维护或者负载过重未能应答。
```



## 4. post create, put patch update, get, head(no response body), delete.. 

GET: to retreive data
POST: to create new data, not idempotent
PUT: to update existing data, can also use to create
DELETE: to delete date

PATCH: used to make changes to **part of** the resource at a location 与put区别：only part of, not whole resource

HEAD: 与get方法类似，但不返回message body内容，仅仅是获得获取资源的部分信息（content-type、content-length）；

**Idempotency**. Idempotence is an important concept in the HTTP specification that states idempotent HTTP requests will result in the same state on the server no matter how many times that same request is executed. GET , HEAD , PUT , and DELETE all have this attribute, but **POST does not**.



## 5. security 

### authentication  & authorization

authentication is the process of verifying who a user is, while **authorization is the process of verifying what they have access to**. 

###  jwt (header.payload.signature) 

JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. 

**Structure:**

 **header.** The header *typically* consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA

**payload.**The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: *registered*, *public*, and *private* claims.

**signature** To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

### https = http + ssl / tls  

In HTTPS, the [communication protocol] is encrypted using [Transport Layer Security] (TLS) or, formerly, Secure Sockets Layer (SSL). The protocol is therefore also referred to as **HTTP over TLS**, or **HTTP over SSL**.
The principal motivations for HTTPS are [authentication]of the accessed [website], and protection of the [privacy] and [integrity] of the exchanged data while in transit.

### private(server) + public key(everyone) -> symmetric key 

### csrf 

**Cross-Site Request Forgery** (CSRF) is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated. 会话劫持.

Cross-site request forgery attacks (CSRF or XSRF for short) are **used to send malicious requests from an authenticated user to a web application**. The attacker can't see the responses to the forged requests, so CSRF attacks focus on state changes, not theft of data

solve: For stateful software use the synchronizer token pattern.For stateless software use double submit cookies

### sql injection 

*SQL injection* is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database.

solve: parameter sql qurey.

### password -> char[] 

String 不能改 容易被catch到

**Strings are immutable**. That means once you've created the `String`, if another process can dump memory, there's no way (aside from [reflection](https://en.wikipedia.org/wiki/Reflection_(computer_programming))) you can get rid of the data before [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) kicks in.

With an array, you can explicitly wipe the data after you're done with it. You can overwrite the array with anything you like, and the password won't be present anywhere in the system, even before garbage collection.

## 6. encoding ->base64 -> ascii 1 - 255 ,1 - 127 7bits  

1 byte = 8bits

## 7.spring mvc   

@RestController, 

@RequestParam, 

@PathVariable, 

@Get..  

@ExceptionHandler 

```
**@RestController** = @Controller + @ResponseBody  is singleton

When you annotate a controller class with @RestController it does two purposes, first, it says that the controller class is handling a request for REST APIs and second you don't need to annotate each method with the @ResposneBody annotation to signal that the response will be converted into a Resource using various HttpMessageConverers.

**@Controller** 

This annotation is used to make a class as a web controller, which can handle client requests and send a response back to the client.

**@GetMapping("/user/{id}")** = @RequestMapping(value="/user/{id}", method = RequestMethod.GET)

**@RequestMapping("/api")**

It's a method level annotation that is specified over a handler method.

**@PathVariable**  **写在method argument里

is used to retrieve data from the URL,his annotation enables the controller to handle a request for parameterized URLs like URLs that have variable input as part of their path

@ExceptionHandler
works at the @Controller level. The @ExceptionHandler annotated method is only active for that particular Controller, not globally for the entire application. Of course, adding this to every controller makes it not well suited for a general exception handling mechanism.
```

### 延伸：how to handle an exception in Spring Application?



## 8. documentation -> swagger

Swagger is a set of open-source tools built around the OpenAPI Specification that can **help you design, build, document and consume REST APIs**. The major Swagger tools include: Swagger Editor – browser-based editor where you can write OpenAPI specs. Swagger UI – renders OpenAPI specs as interactive API documentation.

# Spring 

## 1. IOC + AOP  

IOC: Inversion of control is a design principle which helps to invert the control of object creation. instead of the programmer controlling the flow of a program, the external sources (framework, services, other components) take control of it. 

AOP: Aspect-Oriented Programming entails包含 breaking down program logic into distinct parts called concerns. The functions that span multiple points of an application are called **cross-cutting concerns** and these cross-cutting concerns are conceptually separate from the application's business logic. 

Dependency Injection helps you **decouple your application objects** from each other and AOP helps you **decouple cross-cutting concerns** from the objects that they affect.

A called B, we inject some code between A and B. In order to use AOP, all the objects should in the bean container. If create a object outside container, AOP will not kick in.

spring用**代理类**包裹切面，把他们织入到Spring管理的bean中。也就是说代理类伪装成目标类，它会截取对目标类中方法的调用，让调用者对目标类的调用都先变成调用伪装类，伪装类中就先执行了切面（aspect injection?），再把调用转发给真正的目标bean。

## 2. dependency injection, application context 

DI:**Dependency Injection** is a design pattern which implements IOC principle. Dependency injection is basically providing the objects that an object needs (its dependencies) instead of having it construct them itself.

**bean factory** will send the object needed, the class just call it instead of create the object, this tis dependency injection.

The **Application Context** is Spring's advanced container. Similar to BeanFactory, it can load bean definitions, wire beans together, and dispense beans upon request. 

 It adds more enterprise-specific functionality such as the ability to resolve textual messages from a properties file and the ability to publish application events to interested **event listener**s.

## 3.@Autowired

### field + setter + constructor injection 

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



### constructor : final + null + eager 

he *@Autowired* annotation can also be used on constructors. In the below example, when the annotation is used on a constructor, an instance of *FooFormatter* is injected as an argument to the constructor when *FooService* is created:

```java
@Component("fooFormatter")
public class FooFormatter {
 
    public String format() {
        return "foo";
    }
}
```

```java
public class FooService {
 
    private FooFormatter fooFormatter;
 
    @Autowired //can omit
    public FooService(FooFormatter fooFormatter) {
        this.fooFormatter = fooFormatter;
    }
}
```

### setter : lazy   

**By default, Spring creates all singleton beans eagerly at the startup/bootstrapping of the application context.** The reason behind this is simple: to avoid and detect all possible errors immediately rather than at runtime.

However, there're cases when we need to create a bean, not at the application context startup, but when we request it.

When we put @Lazy annotation over the @Configuration class, it indicates that all the methods with @Bean annotation should be loaded lazily

### @Autowired + @Qualifier

```java
@Bean
//use on method,  执行后产生一个bean放入factory。 为什么要用bean？have no control of source code/class, such as "String class ". 
@Primary 
//相同种类bean，如果不specify，Spring will assign the one with @Primary annotation, use with @Bean together
@Qualifer 
//used under @Autowired, to specify which one to use by name, use with @Autowired together
```



## 4.ByName + ByType 

The difference between byType and byName autowiring is as follows : **Autowire byType will search for a bean in configuration file**, whose id match with the property type to be wired whereas **autowire byName will search for a bean whose id is matching with the property name to be wired**.

## 5. Annotations

### @Component + @Service + @Repository + @Controller + @Bean  

```java
@component
//Indicates that an annotated class is a “component”. Such classes are considered as candidates for auto-detection when using annotation-based configuration and classpath scanning.

@service
//Indicates that an annotated class is a “Service”. This annotation serves as a specialization of @Component, allowing for implementation classes to be autodetected through classpath scanning.

@Controller
// The @Controller annotation is used to indicate the class is a Spring controller. This annotation can be used to identify controllers for Spring MVC or Spring WebFlux.

@Repository 
//Indicates that an annotated class is a “Repository”. This annotation serves as a specialization of @Component and advisable to use with [DAO]

@Autowired
// is used for automatic injection of beans. Spring @Qualifier annotation is used in conjunction with Autowired to avoid confusion when we have two of more bean configured for same type.

@Configuration 
// Used to indicate that a class declares one or more `@Bean` methods. These classes are processed by the Spring container to generate bean definitions and service requests for those beans at runtime.
```

### scope of a bean

singleton, prototype, request, session, global session 

```
### Singleton Scope

is a singleton scope. Only one object of each class is created in the begining of Spring by default. unless manually create more than one object. (getString() and getString2() create 2 String objects.)

### Prototype Scope

If the scope is set to prototype, the Spring IoC container creates a new bean instance of the object every time a request for that specific bean is made.

### request

This scopes a bean definition to an HTTP request. Only valid in the context of a web-aware Spring ApplicationContext.

### Session?

This scopes a bean definition to an HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

### Global Session?

This scopes a bean definition to a global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

Seesion 和 Global session有什么不同？
```

## 6.Proxy and handler

AOP + dynamic proxy 

AOP proxy: **an object created by the AOP framework in order to implement the aspect contracts** (advise method executions and so on). In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy. 

Static proxy, proxy class need to write their own code. ... **Dynamic Proxy dynamically generates proxy class through Proxy**, but it also specifies an implementation class of InvocationHandler. The purpose of proxy pattern is to enhance the function of existing code.

?instance = Proxy.newInstance(classloader + interface(get) + handler) 

?instance.get() -> invoke() in handler 

## ？7. log 

Spring Boot uses [Commons Logging](https://commons.apache.org/logging) for all internal logging but leaves the underlying log implementation open. Default configurations are provided for [Java Util Logging](https://docs.oracle.com/javase/8/docs/api//java/util/logging/package-summary.html), [Log4J2](https://logging.apache.org/log4j/2.x/), and [Logback](https://logback.qos.ch/). In each case, loggers are pre-configured to use console output with optional file output also available.

SLF4J and Apache Commons Logging APIs allow us the flexibility to change our logging framework with no impact on our code.

And we can **use Lombok's \*@Slf4j\* and \*@CommonsLog\* annotations** to add the right logger instance into our class: *org.slf4j.Logger* for SLF4J and *org.apache.commons.logging.Log* for Apache Commons Logging.

Log levels:

Spring supports 5 default log levels, ERROR , WARN , INFO , DEBUG , and TRACE , with INFO being the default log level configuration.理解？

# Spring boot

## 1. embedded tomcat

For example, for a Spring Boot Application, you can generate an **application jar** which contains Embedded Tomcat. You can run a web application as a normal Java application! Embedded server implies that our deployable unit contains the binaries for the server (example, tomcat. jar).

**Spring Boot** has a complete **Tomcat** inside. It builds a so-called fat-jar with everything needed inside. You don't need **Tomcat** installed in your system. BTW: **Spring Boot** also supports other application servers like Jetty



## 2. main method => start application

A Spring Boot application's main class is a class that contains a **public static void main() method** that starts up the Spring ApplicationContext. By default, if the main class isn't explicitly specified, Spring will search for one in the classpath at compile time and fail to start if none or multiple of them are found.

## ?3. actuator 

Spring Boot Actuator module **helps you monitor and manage your Spring Boot application by providing production-ready features like health check-up, auditing, metrics gathering, HTTP tracing etc**. ... Actuator uses Micrometer, an application metrics facade to integrate with these external application monitoring systems.

In a Spring Boot application, we expose a REST API endpoint by using **the @RequestMapping annotation in the controller class**. For getting these endpoints, there are three options: an event listener, Spring Boot Actuator, or the Swagger library.

## 4. application.properties

Properties files are used **to keep 'N' number of properties in a single file** to run the application in a different environment. In Spring Boot, properties are kept in the application. properties file under the classpath. The application.properties file is located in the src/main/resources directory.

## 5. autoconfiguration -> spring.factories

Spring Boot auto-configuration **attempts to automatically configure your Spring application based on the jar dependencies that you have added**. For example, If HSQLDB is on your classpath, and you have not manually configured any database connection beans, then we will auto-configure an in-memory database.

The apparent purpose of **@EnableAutoConfiguration** is to enable automatic configuration features of the Spring Boot application, which automatically configures things if certain classes are present in classpath

 @SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan .

# Microservice

## 1.advantages 

### more request

### scale

### deploy -> geo location

###  rpc / soap / rest api / message

###  one service to one team

###  easy to release new version

## 2. disadvantages

### complex

### centralized configuration