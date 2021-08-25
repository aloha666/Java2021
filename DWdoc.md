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

### ?Monitoring

Spring Boot Actuator

### Fault Tolerance

Fault tolerance is the property that enables a system to continue operating properly in the event of the failure of some of its components. **Hystrix Fault Tolerance and Circuit Breaker can be used.**

# RESTful WebServices

### Design

​	Method (GET, POST, PUT, DELETE, PATCH)
​	URI
​	HEADER(Authorization, content-type)
​	Payload(JSON/XML)

### Implementation

​	Spring Web MVC (@RestController)

### Documentation (Swagger)

### Test (POSTman, RestAssured)

# Consume RESTful

RestTemplate class
OpenFeign Client

# Spring Family

Spring IOC/DI
	Basic usage/annotations
Spring AOP
Spring MVC (rest api)
SpringBoot
Spring Security
Spring Data
Spring Cloud (for microservices)

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

SpringBoot
	executable Jar
Maven / Gradle
	structure
	life cycle(compile, test,package, install..)
	dependency management
	plugins

# Project Management

Maven
VersionControl(Source Control) System
	Git
	SVN
Issue Tracking tools
	Jira
Analysis Tools
	SonarQube
Document
	Wiki / confluence

# Test / Quality Analysis

Unit Test vs Integration Test
Manual Test vs Automation Test
Maven 
JUnit
Mockito
API test( RESTAssured), PostMan
PMD/SonaQube Report(Static code scan), Jacoco coverage report

# CI/CD

Jenkins Pipeline

# Security

Basic HTTP Authentication(username/password)
Token based (JWT)
OAuth2
Single-sign-on(SSO)
Authentication vs Authorization
Server Side validation (@Valid)
CORS
Basic Spring Security configuration

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

Basic SSH knowledge
Jar file vs War file
Docker
image
container

# Questions

What will you do after getting a new task?
What if the project is running slow?
How to fix an error or exception occured on the server?
How do you do code review? or what kind of code is good code?
What are the aspects to consider to start a new project if you are the technical lead?
Do you have a complete picture of SDLC?
What is the typical day of a software engineer?