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

## 4. spring data jpa + dynamic proxy SimpleJpaRepository.class 

## 5. lazy initialization exception 

## 6. @transactional 

## 7.  Entity



























Rest api 1. http + status + http method   http:  stateless  header   /student/{pathVariable}?requestParam=xxx&.. 2. stateless vs stateful(sticky session) 3. status code   200 OK, 201 Created, 204 OK with no content  400 bad request, 401 unauthenticated , 403 unauthroized, 404 not found  500 internal error 4. post create, put patch update, get, head(no response body), delete.. 5. security authentication  authorization (jwt header.payload.signature) https = http + ssl / tls  private(server) + public key(everyone) -> symmetric key csrf sql injection password -> char[] 6. encoding ->base64 -> ascii 1 - 255 ,1 - 127 7bits  1 byte = 8bits 7. spring mvc   @RestController, @RequestParam, @PathVariable, @Get..  @ExceptionHandler 8. documentation -> swagger