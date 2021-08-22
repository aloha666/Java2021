# ORM

## 1. advantages of orm  centralized query language connection pool object mapping lazy loading / eager loading relation 1 - m , m - m, m - 1, 1 - 1 cache  criteria query -> dynamic query 

### advantages of orm

ORM is the approach of taking object-oriented data and mapping to a relational data store

Advantages: speeds-up development. reduce the development cost and time. overcome the specific SQL deafferents.

Disadvantages: developer may lost understand of what code is doing. has tendency to be slow.

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

### cache

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

Hibernate throws the *LazyInitializationException* when it needs to initialize a lazily fetched association to another entity without an active session context. That’s usually the case if you try to use an uninitialized association in your client application or web layer. 在读取数据的时候，Session已经关闭

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

Entities in JPA are nothing but **POJOs representing data that can be persisted to the database**. An entity represents a table stored in a database. Every instance of an entity represents a row in the table.





# Rest api 

## 1. http + status + http method  

 http:  

stateless  header  

 /student/{pathVariable}?requestParam=xxx&..

## 2. stateless vs stateful(sticky session) 

## 3. status code   

200 OK, 201 Created, 204 OK with no content  400 bad request, 401 unauthenticated , 403 unauthroized, 404 not found  500 internal error 



## 4. post create, put patch update, get, head(no response body), delete.. 

## 5. security 

authentication  

authorization (jwt header.payload.signature) 

https = http + ssl / tls  

private(server) + public key(everyone) -> symmetric key 

csrf 

sql injection 

password -> char[] 

## 6. encoding ->base64 -> ascii 1 - 255 ,1 - 127 7bits  

1 byte = 8bits

## 7.spring mvc   

@RestController, 

@RequestParam, 

@PathVariable, 

@Get..  

@ExceptionHandler 

## 8. documentation -> swagger