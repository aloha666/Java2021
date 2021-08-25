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

Jakarta EE Specifications
Servlet
JPA
JMS
JAX-B (for xml)
JAX-RS (for rest api)

# HTTP(Hypertext Transfer Protocol)

Request-Response model
Request
	Method
	Header
	Body
Response
	Status Code
	Header
	Body

Cookie
HTTPS
URL components(https://www.xyz.com/path/user/123?name=jackie&tel=321)
For Rest API, the payload(body) could be JSON/XML

# Architecture Design

Three-Layers Architecture(what and why)
	Demand on Inversion Of Control(IOC) and DI
	Loose Coupling
Service Oriented Architecture( SOA ) - ESB (Enterprise Service Bus)
Microservices
	Basic components
Frontend-backend separation

# Microservices

Service Discovery
Load Balancing
Communication
	REST api
	RPC
	Messaging System
Logging
Monitoring
Fault Tolerance

# RESTful WebServices

Design
	Method (GET, POST, PUT, DELETE, PATCH)
	URI
	HEADER(Authorization, content-type)
	Payload(JSON/XML)
Implementation
	Spring Web MVC (@RestController)
Documentation (Swagger)
Test (POSTman, RestAssured)

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