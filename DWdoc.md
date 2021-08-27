# Java Basic

### Java Version (Java 8, 11)

Java8 new features: Lambda 表达式, Stream API, Optional 类, Date Time API 

### JVM (Heap, Stack, GC)

Heap: max/min queue from queue interface, use binary tree or array, O(logn)

Stack: FILO,  from list interface, 

GC: heap area, GC root

### Syntax and Keywords

### Collections

### Thread

### ClassLoader

The Java ClassLoader is a part of the Java Runtime Environment that dynamically loads Java classes into the Java Virtual Machine.no

### Reflection: 

Reflection is an API which is used to examine or modify the behavior of methods, classes, interfaces at the run time. (是不是不利于encapsulation)

java中class.forName()和classLoader都可用来对类进行加载。

class.forName()前者除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。Returns the Class object associated with the class or interface with the given string name. Invoking this method is equivalent to: Class.forName(className, true, currentLoader)

而classLoader只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。
Invoking this method is equivalent to invoking loadClass(name, false).

```
The first one (Class.forName("SomeClass");) will:

use the class loader that loaded the class which calls this code
initialize the class (that is, all static initializers will be run)
The other (ClassLoader.getSystemClassLoader().loadClass("SomeClass");) will:

use the "system" class loader (which is overridable)
not initialize the class (say, if you use it to load a JDBC driver, it won't get registered, and you won't be able to use JDBC!)
```



### OOP concepts

# Design Patterns

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

### Adapter

### Proxy

provide substitute for another object, control the access to the original objects, perform some operations before and after request/response(AOP?)

static: create before invoke
dyanamic: create when invoke during runtime.

pro: separete the proxy and real object, decoupling the system,protect the orignal object.
con:lower process speed and increate the comlexity of the system.

use: AOP

...

# Jakarta EE(JavaEE)

### Jakarta EE Specifications

The Java EE Platform specification is **the umbrella specification that defines the** Java EE platform.

### Servlet

Simply put, a Servlet  runs on the Java-enabled web server or application server, is **a class that handles requests, processes them and reply back with a response**.

### JPA

The Java Persistence API (JPA) is one possible approach to ORM. Via JPA the developer can **map, store, update and retrieve data from relational databases to Java objects and vice versa**

### JMS

The Java Message Service (JMS) API is a **messaging standard that allows application components based on the Java Platform Enterprise Edition (Java EE) to create, send, receive, and read messages**.

The Java Message Service (JMS) makes it easy to develop enterprise applications that **asynchronously send and receive business data and events**.

### JAX-B (for xml)

JAXB **allows Java developers to access and process XML data without having** to know XML or XML processing

provides two main features: the ability to marshal Java objects into XML and the inverse, i.e. to unmarshal XML back into Java objects. 

### JAX-RS (for rest api) 

和spring web MVC 同级

Jakarta RESTful Web Services,  is a Jakarta EE API specification that provides support in creating web services according to the Representational State Transfer architectural pattern. **JAX-RS** uses the Restful architectural structure to communicate between a client and a server. 

The Jakarta XML Web Services is a Jakarta EE API for creating web services, particularly SOAP services. JAX-WS uses SOAP as its main method of communication. SOAP ( **Simple Object Access Protocol**) is a message protocol that allows distributed elements of an application to communicate.

# HTTP(Hypertext Transfer Protocol)

### Request-Response model

The typical model for computers communicating on a network is request-response. In the request-response model, a client computer or software requests data or services, and **a server computer or software responds to the request by providing the data or service**.

HTTP stands for Hypertext Transfer Protocol, as a request-response protocol, HTTP gives users a way to interact with web resources such as HTML files by transmitting hypertext messages between clients and servers.

HTTP is a [stateless protocol]. A stateless protocol does not require the [HTTP server] to retain information or status about each user for the duration of multiple requests. However, some [web applications] implement states or [server side sessions] using for instance [HTTP cookies] or hidden [variables] within [web forms].

### Request

​	Method
​	Header
​	Body

### Response

​	Status Code
​	Header
​	Body

### Cookie (stateless?)

Cookies are created to identify you when you visit a new website.
HTTP cookies (also called web cookies, Internet cookies, browser cookies, or simply cookies) are small blocks of data created by a web server while a user is browsing a website and placed on the user's computer or other device by the user’s web browser.

### HTTPS

In HTTPS, the [communication protocol] is encrypted using [Transport Layer Security] (TLS) or, formerly, Secure Sockets Layer (SSL). The protocol is therefore also referred to as **HTTP over TLS**, or **HTTP over SSL**.
The principal motivations for HTTPS are [authentication]of the accessed [website], and protection of the [privacy] and [integrity] of the exchanged data while in transit. 

### URL components(https://www.xyz.com/path/user/123?name=jackie&tel=321)

**A scheme**. The scheme identifies the protocol to be used to access the resource on the Internet. It can be HTTP (without SSL) or HTTPS (with SSL).
**A host**. The host name identifies the host that holds the resource. For example, www.example.com. A server provides services in the name of the host, but hosts and servers do not have a one-to-one mapping. Refer to Host names.
Host names can also be followed by a port number. Refer to Port numbers. Well-known port numbers for a service are normally omitted from the URL. Most servers use the well-known port numbers for HTTP and HTTPS , so most HTTP URLs omit the port number.

**A path**. The path identifies the specific resource in the host that the web client wants to access. For example, /software/htp/cics/index.html.
**A query string**. If a query string is used, it follows the path component, and provides a string of information that the resource can use for some purpose (for example, as parameters for a search or as data to be processed). The query string is usually a string of name and value pairs; for example, term=bluebird. Name and value pairs are separated from each other by an ampersand (&); for example, term=bluebird&source=browser-search.



### ?For Rest API, the payload(body) could be JSON/XML

# Architecture Design

### Three-Layers Architecture(what and why)

Web layer/presentation layer - Service layer/logic layer - Data Access Object (DAO) layer

Between layers, using interface instead of concrete class. 

1. It gives you the ability to update the technology stack of one tier, without impacting other areas of the application.
2. It allows for different development teams to each work on their own areas of expertise. Today’s developers are more likely to have deep competency in one area, like coding the front end of an application, instead of working on the full stack.
3. You are able to scale the application up and out. A separate back-end tier, for example, allows you to deploy to a variety of databases instead of being locked into one particular technology. It also allows you to scale up by adding multiple web servers.
4. It adds reliability and more independence of the underlying servers or services.
5. It provides an ease of maintenance of the code base, managing presentation code and business logic separately, so that a change to business logic, for example, does not impact the presentation layer.

### Demand on Inversion Of Control(IOC) and DI

​	Loose Coupling

### Service Oriented Architecture( SOA ) - ESB (Enterprise Service Bus)

The Enterprise Service Bus (ESB) is **a software architecture which connects all the services together over a bus like infrastructure**. It acts as communication center in the SOA by allowing linking multiple systems, applications and data and connects multiple systems with no disruption.

disadvantage: not scalable, infrastructure is designed.

### ?Frontend-backend separation

The division of frontend and backend will create a communication gap, leaving both the teams uninformed or unclear information regarding the changes on the respective ends. Keeping the frontend and backend together will lessen the chances of such miscommunications, facilitating smooth application development.

If your front-end is separate from the back-end, **it becomes easier to work on one module keeping the other one untouched**. Modularity also makes it easier for two teams or two people to work on front-end and back-end simultaneously without worrying about overwriting or messing up other person's work. **Scalability** is better.

# Microservices

Microservice architecture – a variant of the service-oriented architecture structural style – arranges an application as a collection of loosely-coupled services. In a microservices architecture, services are fine-grained and the protocols are lightweight

### Service Discovery

Eureka

### Load Balancing

Ribbon

### Communication

https://medium.com/the-sixt-india-blog/microservice-communication-53cbc93cf4de

​	REST api
​	RPC
​	Messaging System

### ?Logging

In Java, logging is an important feature that **helps developers to trace out the errors**. In Spring, the log level configurations can be set in the application. properties file which is processed during runtime. Spring supports 5 default log levels, **ERROR , WARN , INFO , DEBUG , and TRACE** , with INFO being the default log level configuration.

```
print:sync, only in console, 
logger:aync, print to define files, have differen levels
```



### ?Monitoring

Spring Boot Actuator

### Fault Tolerance

Fault tolerance is the property that enables a system to continue operating properly in the event of the failure of some of its components. **Hystrix Fault Tolerance and Circuit Breaker can be used.**

# RESTful WebServices

Restful Web Services is **a lightweight, maintainable, and scalable service that is built on the REST architecture**. 

URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。

### Design

​	**Method (GET, POST, PUT, DELETE, PATCH)**

​	URI

​			A **URI is an **identifier** of a specific resource. Like a page, or book, or a document.

​			A **URL** is special type of identifier that also tells you how to access it, such as `HTTPs`, `FTP`, etc.—like **https://**			www.google.com.

```
要让一个资源可以被识别，需要有个唯一标识，在Web中这个唯一标识就是URI(Uniform Resource Identifier)。

URI既可以看成是资源的地址，也可以看成是资源的名称。如果某些信息没有使用URI来表示，那它就不能算是一个资源， 只能算是资源的一些信息而已。

使用?用来过滤资源
很多人只是把?简单的当做是参数的传递，很容易造成URI过于复杂、难以理解。可以把?用于对资源的过滤， 例如/git/git/pulls用来表示git项目的所有推入请求，而/pulls?state=closed用来表示git项目中已经关闭的推入请求， 这种URL通常对应的是一些特定条件的查询结果或算法运算结果。
```



​	**HEADER(Authorization, content-type)**

​		ResultSet object is associated with a header (called meta data), which contains information about the resultset 		object, such as authorization, content type.

​	**Payload(JSON/XML)**

### Implementation

​	Spring Web MVC (@RestController)

### Documentation (Swagger)

Swagger is the most widely used tooling ecosystem for developing APIs with the OpenAPI Specification (OAS)

The format is both machine-readable and human-readable. As a result, it can be used **to share documentation among product managers, testers and developers**, but can also be used by various tools to automate API-related processes.

API documentation is **a technical content deliverable, containing instructions about how to effectively use and integrate with an API**. ... API description formats like the OpenAPI/Swagger Specification have automated the documentation process, making it easier for teams to generate and maintain them.

### Test (POSTman, RestAssured)

# Consume RESTful

### !!!RestTemplate class

Rest Template is used **to create applications that consume RESTful Web Services**.

### OpenFeign Client

FeignClient is **a library for creating REST API clients in a declarative way**. progamm	based on interface.

Feign allows programmer to abstract the mechanics of calling a REST service. Once you configure and annotate the Feign interface, you can call a REST service by making a simple Java function call. The actual implementation of making a REST call is handled at runtime by Feign. This means that the implementation can be configured without changing your business logic code.

advantage: not hard coding url, So there is **no need to write any unit test** as there is no code to test in the first place

disadvantage:more effore when create

### JpaRepository<ReportRequestEntity, String>

#   Family

### Spring Framework

```
Spring Framework is a Java platform that provides comprehensive infrastructure support for developing Java applications.

On one hand, Spring comes with some of the existing technologies like ORM framework, logging framework, J2EE and JDK Timers etc, Hence we don’t need to integrate explicitly those technologies. Spring WEB framework has a well-designed web MVC framework, which provides a great alternate to lagacy web framework. This simplify the programer's work for setting up.

On the other had, Spring has the implematation of DI and AOP, which simplify the programer's work to develop, create a convient enviroment for programming.


```



### Spring IOC/DI

​	Basic usage/annotations

```java
//@valid : method level, or member atteubute
//@validated: group level

@Validated：用在方法入参上无法单独提供嵌套验证功能。不能用在成员属性（字段）上，也无法提示框架进行嵌套验证。能配合嵌套验证注解@Valid进行嵌套验证。**不能用在类的属性上**

@Valid：用在方法入参上无法单独提供嵌套验证功能。能够用在成员属性（字段）上，提示验证框架进行嵌套验证。能配合嵌套验证注解@Valid进行嵌套验证。
```

### Spring AOP

```
Aspect-Oriented Programming entails包含 breaking down program logic into distinct parts called concerns. The functions that span multiple points of an application are called **cross-cutting concerns** and these cross-cutting concerns are conceptually separate from the application's business logic. 

Dependency Injection helps you **decouple your application objects** from each other and AOP helps you **decouple cross-cutting concerns** from the objects that they affect.
```



### Spring MVC (rest api)

### SpringBoot

```
Advantages:
Opinionated ‘starter' dependencies to simplify the build and application configuration
Embedded server to avoid complexity in application deployment
Metrics, Health check, and externalized configuration
Automatic config for Spring functionality – whenever possible
```

### Spring Security

### Spring Data

```
Spring Data is a high level SpringSource project whose purpose is to unify and ease the access to different kinds of persistence stores, both relational database systems and NoSQL data stores.

JpaRepo:
MonoRepo:
```

### Spring Cloud (for microservices)

```java
Spring Cloud is defined as an open-source library that provides tools for quickly deploying the JVM based application on the clouds
```

SpringBoot vs Spring

```
SpringBoot:
Opinionated ‘starter' dependencies to simplify the build and application configuration
Embedded server to avoid complexity in application deployment
Metrics, Health check, and externalized configuration
Automatic config for Spring functionality – whenever possible

All of the Spring configuration above is automatically included by adding the Boot web starter through a process called auto-configuration.What this means is that Spring Boot will look at the dependencies, properties, and beans that exist in the application and enable configuration based on these.

In a few words, we can say that Spring Boot is simply an extension of Spring itself to make development, testing, and deployment more convenient.
```



# Amazon WebServices (AWS)

Compute
	EC2
	Lambda
Storage
	S3
Integration
	SQS
	SNS
DB
	RDS

DynamoDB
DocumentDB
IAM
ECS/ECR

# Build

### SpringBoot

​	**executable Jar**

​	The spring-boot-loader modules lets Spring Boot support executable jar and war files. If you use the Maven plugin or 	the Gradle plugin, executable jars are automatically generated, and you generally do not need to know the details of 	how they work.

​	

### Maven / Gradle

​	**structure**

​	Apache Maven is a **software project management and comprehension tool**.

​	src - main - java/res; src-main-test; pom file

​	POM stands for "Project Object Model". It is an XML representation of a Maven project held in a file named pom.xml. 	The POM contains all necessary information about a project, as well as configurations of plugins to be used during 	the build process.

​	**life cycle(compile, test,package, install..)**

​	validate - compile -test - package - verify (Integration test) - install - deploy

​	**dependency management**

​	**plugins**

​	The Spring Boot Maven Plugin provides Spring Boot support in Apache Maven. It allows you to package executable 	jar or war archives, run Spring Boot applications, generate build information and start your Spring Boot application 	prior to running integration tests.

​	**plugin is a Jar file which executes the task, and dependency is a Jar which provides the class files to execute the task.**

# Project Management

### Maven

### VersionControl(Source Control) System

Git
SVN: Version Control Subversion is one of many version control options available today. It's often abbreviated as 	 SVN.

### Issue Tracking tools

Jira:Jira Software is part of a family of products designed **to help teams of all types manage work**. Originally, Jira was designed as a bug and issue tracker

### Analysis Tools

SonarQube: SonarQube is a Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project. It combines static and dynamic analysis tools and enables quality to be measured continually over time.

### Document

Wiki / confluence: Confluence is the wiki software for today's modern team, giving every project and person their own Space to create and share. 

# Test / Quality Analysis

### Unit Test vs Integration Test

Unit Test: Unit Testing is defined as a type of software testing where individual components of a software are tested.

Integration Test: Integration testing is the process of testing the interface between two software units or modules. Its focus is on determining the correctness of the interface.

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



### Manual Test vs Automation Test

Autaomation Test:Test automation can **automate some repetitive but necessary tasks in a formalized testing process already in place**, or perform additional testing that would be difficult to do manually. Test automation is critical for continuous delivery and continuous testing.

### Maven 

### JUnit

JUnit is a Java unit testing framework that's one of the best test methods for **regression testing**. An open-source framework, it is used to write and run repeatable automated tests.

### Mockito

Mockito is a mocking framework, JAVA-based library that is used for effective unit testing of JAVA applications. Mockito is used to mock interfaces so that a dummy functionality can be added to a mock interface that can be used in unit testing.

### API test( RESTAssured), PostMan

### PMD/SonaQube Report(Static code scan), Jacoco coverage report

# CI/CD

Jenkins Pipeline

# Security

### Basic HTTP Authentication(username/password)

HTTP basic authentication is a simple challenge and response mechanism with which a server can request authentication information (a user ID and password) from a client. The client passes the authentication information to the server in an Authorization header. The authentication information is in base-64 encoding.

### Token based (JWT)

Jason Web Token, JSON Web Token is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims. The tokens are signed either using a private secret or a public/private key. 

### OAuth2

OAuth doesn't share password data but instead uses authorization tokens to prove an identity between consumers and service providers. OAuth is an authentication protocol that allows you to approve one application interacting with another on your behalf without giving away your password.

### Single-sign-on(SSO)

Single sign-on (SSO) is an authentication method that enables users to securely authenticate with multiple applications and websites by using just one set of credentials.

### Authentication vs Authorization

 Authentication confirms that users are who they say they are. Authorization gives those users permission to access a resource.

### Server Side validation (@Valid)

Server-side code is used to validate the data before the data is saved in the database or otherwise used by the application. If the data fails validation, a response is sent back to the client with corrections that the user needs to make.

### CORS

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading of resources. ... For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts.

### Basic Spring Security configuration

# Database

Basic DB concepts(normalization, view, trigger, stored procedure)
Basic Query(CRUD)
Joins(outer, inner)
Relationships (1-1, 1-m, m-m)
ORM
JPA / Hibernate
Eager/lazy loading
Cascade type
Mapping

MongoDB why mongoDB? advantage

# Deployment

### Basic SSH knowledge

### Jar file vs War file

Generally, a JAR file contains Java related resources like libraries, classes etc. JAR file is like winzip file except that Jar files are platform independent. A WAR file is simply a JAR file but **contains only Web related Java files like Servlets, JSP, HTML**.

### ?Docker

Docker is **an open platform for developing, shipping, and running applications**. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications.

### image

Docker images are read-only templates used to build containers. Containers are deployed instances created from those templates. Images and containers **are closely related**, and are essential in powering the Docker software platform.

### container

# Questions

What will you do after getting a new task?
What if the project is running slow?
How to fix an error or exception occured on the server?
How do you do code review? or what kind of code is good code?
What are the aspects to consider to start a new project if you are the technical lead?
Do you have a complete picture of SDLC?
What is the typical day of a software engineer?

# Project Questions

1. what is constrcutor injection? what benefits? what problem?
2. what is a thread pool? 
3. what is the conpletebFuture? what benefit? what usage?
4. what is eureka? what is ribbon?@loadbalance?irule()?
5. what is config service?
6. why use this name? 命名原则？