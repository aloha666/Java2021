



## 1. The Problem

- Mapping member variable to columns
- Mapping relationships
- Handling data types
- Managing changes to object state



## 2. Saving without Hibernate

- JDBC Datbase Configuration
- The model object
- Service method to create the model object
- Database Design
- DAO method to save the object using SQL queries



## 3. The Hibernate way

- JDBC Datbase Configuration - Hibernate
- The model object - Annotations
- Service method to create the model object - Use the Hibernate API
- Database Design - No needed!
- DAO method to save the object using SQL queries - No needed!



## 4. Writing the Model Class with Annotations

```java
//Employee Entity

@Entity(name = "ForeignKeyAssoEEntity")
@Table(name = "Employee", uniqueConstraints = {
		@UniqueConstraint(columnNames = "ID"),
		@UniqueConstraint(columnNames = "EMAIL") })
public class EmployeeEntity implements Serializable {

	private static final long serialVersionUID = -1798070786993154676L;

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "ID", unique = true, nullable = false)
	private Integer employeeId;

	@Column(name = "EMAIL", unique = true, nullable = false, length = 100)
	private String email;

	@Column(name = "FIRST_NAME", unique = false, nullable = false, length = 100)
	private String firstName;

	@Column(name = "LAST_NAME", unique = false, nullable = false, length = 100)
	private String lastName;

	@OneToMany(cascade=CascadeType.ALL) //cascade type
	@JoinColumn(name="EMPLOYEE_ID") //set up key for the many 
	private Set<AccountEntity> accounts; //@ElementCollection? 

	public Integer getEmployeeId() {
		return employeeId;
	}

	public void setEmployeeId(Integer employeeId) {
		this.employeeId = employeeId;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public Set<AccountEntity> getAccounts() {
		return accounts;
	}

	public void setAccounts(Set<AccountEntity> accounts) {
		this.accounts = accounts;
	}
}

//Account Entity
//xml line

//在xml中注册entity
<mapping class="com.howtodoinjava.hibernate.oneToMany.foreignKeyAsso.AccountEntity"/>
    
// java class (constructor? the Java compiler will create a default constructor in the byte code with an empty argument.)
@Entity(name = "ForeignKeyAssoAccountEntity") //below class is an entity,name is the entity name
@Table(name = "ACCOUNT", uniqueConstraints = {
		@UniqueConstraint(columnNames = "ID")}) //database location: name and pk, Entity and table name 取哪个？ 有什么区别？ 一般用table. table name is for the table of entity, while entity names still remains
public class AccountEntity implements Serializable //why serializable?
{

	private static final long serialVersionUID = -6790693372846798580L;

	@Id  //pk below 
	@GeneratedValue(strategy = GenerationType.IDENTITY) // auto genrate id, no need provide
	@Column(name = "ID", unique = true, nullable = false) // database location
	private Integer accountId;

	@Column(name = "ACC_NUMBER", unique = true, nullable = false, length = 100) //database laction, a column & name
	private String accountNumber;
	
	@ManyToOne //mapping relation,no need for cascade type?
	private EmployeeEntity employee;
	
	public Integer getAccountId() {
		return accountId;
	}

	public void setAccountId(Integer accountId) {
		this.accountId = accountId;
	}
	
	public String getAccountNumber() {
		return accountNumber;
	}

	public void setAccountNumber(String accountNumber) {
		this.accountNumber = accountNumber;
	}

	public EmployeeEntity getEmployee() {
		return employee;
	}

	public void setEmployee(EmployeeEntity employee) {
		this.employee = employee;
	}
}
```

## Using the Hibernate API

- Create a session factory
- Create a session from the seesion factory
- Use the session to save model objects

```java
//configure seesion factory
public class HibernateUtil {
	private static final SessionFactory sessionFactory = buildSessionFactory();
	 
    private static SessionFactory buildSessionFactory() {
        try {
            // Create the SessionFactory from hibernate.cfg.xml
        	Configuration config = new Configuration()
        			.configure(new File(HibernateUtil.class
        					.getClassLoader()
        					.getResource("hibernate.cfg-one-to-many.xml")
        					.getFile()));
            return config.buildSessionFactory();
 
        }
        catch (Throwable ex) {
            // Make sure you log the exception, as it might be swallowed
            System.err.println("Initial SessionFactory creation failed." + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }
 
    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
 
    public static void shutdown() {
    	// Close caches and connection pools
    	getSessionFactory().close();
    }
}
// create session factory, create session, save new object;
public class TestForeignKeyAssociation
{
	
	public static void main(String[] args) 
	{
		Session session = HibernateUtil.getSessionFactory().openSession();
		session.beginTransaction();
     
    //how about account id?
		AccountEntity account1 = new AccountEntity();
		account1.setAccountNumber("Account detail 1");
		
		AccountEntity account2 = new AccountEntity();
		account2.setAccountNumber("Account detail 2");
		
		AccountEntity account3 = new AccountEntity();
		account3.setAccountNumber("Account detail 3");
    
    //Add new Employee object
		EmployeeEntity firstEmployee = new EmployeeEntity();
		firstEmployee.setEmail("demo-user-first@mail.com");
		firstEmployee.setFirstName("demo-one");
		firstEmployee.setLastName("user-one");
		
		EmployeeEntity secondEmployee = new EmployeeEntity();
		secondEmployee.setEmail("demo-user-second@mail.com");
		secondEmployee.setFirstName("demo-two");
		secondEmployee.setLastName("user-two");
		
		Set<AccountEntity> accountsOfFirstEmployee = new HashSet<AccountEntity>();
		accountsOfFirstEmployee.add(account1);
		accountsOfFirstEmployee.add(account2);
		
		Set<AccountEntity> accountsOfSecondEmployee = new HashSet<AccountEntity>();
		accountsOfSecondEmployee.add(account3);
		
		firstEmployee.setAccounts(accountsOfFirstEmployee);
		secondEmployee.setAccounts(accountsOfSecondEmployee);
		//Save Employee
		session.save(firstEmployee);
		session.save(secondEmployee);
		
		session.getTransaction().commit();
		HibernateUtil.shutdown();
  }
}	
```

Other anotations:

```java
@Basic: 
@Transcient: ingore the element (same as in seriliable)
@Generatdvalue: help to auto-genrate data/id(int only?)
```

## 5. Embeded Object

@Embeddable

make a class embeddable, the attributes of the object will be embedded into other entity

@Embeded  

declare an object is embedded, the attributes of the object will be used as part of the database table (not the whole object), @EmbeddedID can be used to embed a pk.

@AttributeOverride (name="desire object attribtue name",column =@Column(name="desire column name"))



## 6. Eager and Lazy Fetch拿取 Type 

 主表和附表之间的互动，查询主表某个ID时，是不是需要主动查询出所有附表中含有此id的内容？有时需要，有时不需要，就需要进行设定。Hibernate default是不进行联动查询的。叫lazy iniitialaztion.

**lazy iniitialaztion/fetch**: hibernate 初始设定，不对附表进行联动，查什么给什么。如果只需要查一个表的信息，这种方法很快。

**Eager fetch:** 对附表进行联动，查主表时主动查附表 

如果是**EAGER**，那么表示取出这条数据时，它关联的数据也同时取出放入内存中 如果是**LAZY**，那么取出这条数据时，它关联的数据并不取出来，在同一个session中，什么时候要用，就什么时候取(再次访问数据库)。但是，在session外，就不能再取了。用EAGER时，因为在内存里，所以在session外也可以取。

```java
@ElementCollection(fetch = FetchType.EAGER)
//ElementCollection meaning?
```

**Fetch vs Cascade?**

The `CascadeType` in Hib. could be `REFRESH`, `MERGE`, ..., `ALL` you put it under the related entity and **it determines the behavior of the related entity** if the current entity is: refreshed, updated, deleted, e.t.c.. So whenever you entity is affected the `CascadeType` tells if the related entity should be affected as well.

The `FetchType` could be only of 2: `EAGER` and `LAZY`. This one you as well put under the related entity and **it determines whether the related entity should be initialized right away when the current entity is initialized** (`EAGER`) **or only on demand** (`LAZY`).

- **CascadeType** is a property used to define cascading in a relationship between a parent and a child.
- **FetchType** is a property used to define fetching strategies which are used to optimize the Hibernate generated select statement, so that it can be as efficient as possible.

ts simple :Consider two entities 1. Department and 2. Employee and they have one-to-many mappings.That is one department can have many employee **cascade=CascadeType.ALL** and it essentially means that any change happened on DepartmentEntity must cascade to EmployeeEntity as well. If you save an Department , then all associated Employee will also be saved into database. If you delete an Department then all Employee associated with that Department also be deleted.
**Cascade-type all is combination of PERSIST, REMOVE ,MERGE and REFRESH cascade types.** [Example for Cascade type All](http://www.concretepage.com/hibernate/example-cascadetype-all-hibernate)

Fetch type Eager is essentially the opposite of Lazy.Lazy which is the default fetch type for all Hibernate annotation relationships. When you use the Lazy fetch type, Hibernate won’t load the relationships for that particular object instance. **Eager will by default load ALL of the relationships** related to a particular object loaded by Hibernate.

### Default Fetech vs Default Cascade

By default, the JPA @ManyToOne and @OneToOne annotations are fetched EAGERly, while the @OneToMany and @ManyToMany relationships are considered LAZY. This is the default strategy, and Hibernate doesn’t magically optimize your object retrieval, it only does what is instructed to do.

No default cascade for mapping, if not setting up, no cascading will done.

## 7. Proxy代理 Objects

**？ proxy** object:  Hibernate 自带方法。作用？ 给object做一个proxy，功能和object相同，但是直接对database进行操作 object -> proxy -> database。好处？



## 8. Mapping

### JoinColumn

The annotation *javax.persistence.JoinColumn* marks a column for as a join column for an entity association or an element collection. 用来设置外键？

### One to One Mapping 一对一 

主表中加入一个附表ID， 附表不用进行操作。

**实现方法：**

1. ForeighKeyAsso

   通过mappedby 和 join column 实现，双向

   ```java
   //account entity
   @OneToOne(mappedBy="account", optional=false)
   //注意此处mappedBy, use MappedBy to tell Hibernate, we have chosen the other entity to dictate the mapping of the relationship between the two entities.
   //所以只有employee表中有account ID, 而accout表中没有emplyeeID
   private EmployeeEntity employee;
   
   //employee entity
   @OneToOne(optional=false)
   @JoinColumn(name="ACCOUNT_ID", unique=true, nullable=false, updatable=false)
   private AccountEntity account;
   ```

   @JoinColumn 属性

   | `columnDefinition`     |      | 默认值：空 `String`。JPA 使用最少量 SQL 创建一个数据库表列。如果需要使用更多指定选项创建列，请将 `columnDefinition` 设置为在针对列生成 DDL 时希望 JPA 使用的 `String` SQL 片断。 |
   | ---------------------- | :--- | ------------------------------------------------------------ |
   | `insertable`           |      | 默认值：`true`。默认情况下，JPA 持续性提供程序假设它可以插入到所有表列中。如果该列为只读，请将 `insertable` 设置为 `false`。 |
   | `name`                 |      | 默认值：如果使用一个连接列，则 JPA 持续性提供程序假设外键列的名称是以下名称的连接：引用关系属性的名称 +“_”+ 被引用的主键列的名称。引用实体的字段名称 +“_”+ 被引用的主键列的名称。如果实体中没有这样的引用关系属性或字段（请参阅 [@JoinTable](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#JoinTable)），则连接列名称格式化为以下名称的连接：实体名称 +“_”+ 被引用的主键列的名称。这是外键列的名称。如果连接针对“一对一”或“多对一”实体关系，则该列位于源实体的表中。如果连接针对“多对多”实体关系，则该列位于连接表（请参阅 [@JoinTable](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#JoinTable)）中。如果连接列名难于处理、是一个保留字、与预先存在的数据模型不兼容或作为数据库中的列名无效，请将 `name` 设置为所需的 `String` 列名。 |
   | `nullable`             |      | 默认值：`true`。默认情况下，JPA 持续性提供程序假设允许所有列包含空值。如果不允许该列包含空值，请将 `nullable` 设置为 `false`。 |
   | `referencedColumnName` |      | 默认值：如果使用一个连接列，则 JPA 持续性提供程序假设在实体关系中，被引用的列名是被引用的主键列的名称。如果在连接表（请参阅 [@JoinTable](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#JoinTable)）中使用，则被引用的键列位于拥有实体（如果连接是反向连接定义的一部分，则为反向实体）的实体表中。要指定其他列名，请将 `referencedColumnName` 设置为所需的 `String` 列名。 |
   | `table`                |      | 默认值：JPA 持续性提供程序假设实体的所有持久字段存储到一个名称为实体类名称的数据库表中（请参阅 [@Table](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#Table)）。如果该列与辅助表关联（请参阅 [@SecondaryTable](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#SecondaryTable)），请将 `name` 设置为相应辅助表名称的 `String`名称，如[示例 1-8](http://www.oracle.com/technology/global/cn/products/ias/toplink/jpa/resources/toplink-jpa-annotations.html#CHDCEHDA) 所示。 |
   | `unique`               |      | 默认值：`false`。默认情况下，JPA 持续性提供程序假设允许所有列包含重复值。如果不允许该列包含重复值，请将 `unique` 设置为 `true`。 |
   | `updatable`            |      | 默认值：`true`。默认情况下，JPA 持续性提供程序假设它可以更新所有表列。如果该列为只读，则将 `updatable` 设置为 `false` |

   

2. JoinTable

   怎么实现？Jointable+joincolumn 单向？jointable:连接表，真实创建还是只存在在hibernate里？会新建一个连接表

   @JoinTable 属性

   |        属性        | 是否必须 | 说明                                                         |
   | :----------------: | :------: | :----------------------------------------------------------- |
   |        name        |    否    | 指定该连接表的表名                                           |
   |    JoinColumns     |    否    | 该属性值可接受多个`@JoinColumn`，用于配置`连接表`中外键列的信息，这些外键列参照`当前实体对应表`的主键列 |
   | inverseJoinColumns |    否    | 该属性值可接受多个`@JoinColumn`，用于配置`连接表`中外键列的信息，这些外键列参照当前实体的`关联实体对应表`的主键列 |
   |    targetEntity    |    否    | 该属性指定`关联实体`的类名。在默认情况下，Hibernate将通过反射来判断关联实体的类名 |
   |      catalog       |    否    | 设置将该连接表放入指定的catalog中。如果没有指定该属性，连接表将放入默认的catalog |
   |       schema       |    否    | 设置将该连接表放入指定的schema中。如果没有指定该属性，连接表将放入默认的schema |
   | uniqueConstraints  |    否    | 该属性用于为`连接表`增加唯一约束                             |
   |      indexes       |    否    | 该属性值为`@Index`注解数组，用于为该连接表定义多个索引       |

   ```java
   //account entity 
   //no employee entity referred
   
   //employee entity
   @OneToOne(cascade = CascadeType.ALL)
   @JoinTable(name="EMPLOYEE_ACCCOUNT", joinColumns = @JoinColumn(name="EMPLOYEE_ID"), inverseJoinColumns = @JoinColumn(name="ACCOUNT_ID"))
   private AccountEntity account;
   ```

   

3. mapsByID

   怎么实现？通过MapsID  单向

   ```java
   //account entity
   //no employee entity referred
   //employee entity
   @OneToOne 
   @MapsId
   private AccountEntity account;
   ```

   

4. sharedPrimaryKey

   通过mappedby 和 primarykeyjoincolumn 实现，双向?

   ```java
   //account entity
   @OneToOne(mappedBy="account", cascade=CascadeType.ALL)
   private EmployeeEntity employee; 
   
   //employee entity
   @OneToOne(cascade = CascadeType.ALL)
   @PrimaryKeyJoinColumn
   private AccountEntity account;
   ```

其他例子

```java

//reference: https://www.jianshu.com/p/02ddc541d9ac Hibernate入门2-关联和映射
@Entity
public class Professor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "professor_id")
    private Integer id;

    private String name;

    @OneToOne(targetEntity = Student.class)
    // FOREIGN KEY (student_id) REFERENCES student_info (id);
    @JoinColumn(name = "student_id", referencedColumnName = "id")  //设置外键 studdent, column name = student_id
    private Student student;
    
    // getters and setters
}

//操作
public static void addProfessor(Integer student_id) {
    Configuration conf = new Configuration().configure();
    StandardServiceRegistry registry = conf.getStandardServiceRegistryBuilder().build();
    try (SessionFactory sessionFactory = conf.buildSessionFactory(registry); Session session = sessionFactory.openSession()) {
        Transaction transaction = session.beginTransaction();

        Professor professor = new Professor();
        professor.setName("james lee");

        Student student = session.get(Student.class, student_id);
        professor.setStudent(student);

        session.save(professor);
        transaction.commit();
    }
}

public static void main(String[] args) {
    addProfessor(10);
}也可做双向一对一
```



### One to Many Mapping 一对多

1. ForeighKeyAsso

通过 join column 实现

```java
//acount entity
@ManyToOne
private EmployeeEntity employee; //注意此处比onetoone少了mappedby，所以会有employee column

//employee entity
 @OneToMany(cascade=CascadeType.ALL)
 @JoinColumn(name="EMPLOYEE_employeeID") //会在account表中建立一个名为EMPLOYEE_employeeID的column 按照ID对应生成
 private Set<AccountEntity> accounts;

```



2. JoinTable

   

```java
//acount entity

//employee entity
 @OneToMany(cascade=CascadeType.ALL)
@JoinTable(name="EMPLOYEE_ACCOUNT", joinColumns={@JoinColumn(name="EMPLOYEE_ID", referencedColumnName="ID")}, inverseJoinColumns={@JoinColumn(name="ACCOUNT_ID", referencedColumnName="ID")})	
private Set<AccountEntity> accounts;
```



其他例子

```java
//student java
@Entity
@Table(name = "student_info")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    private int age;

    @ManyToOne(targetEntity = Professor.class)
    @JoinColumn(name = "professor_id", referencedColumnName = "professor_id", nullable = false) // remove unique=true!!!
    @Cascade(org.hibernate.annotations.CascadeType.ALL) //指定映射的级联。如果没有这个注解，那我们就必须先save(professor)，才能save(student)。
    private Professor professor;
    
    // getter and setters
}

//professor.java professor是没有任何外键的，student_info有一个外键，引用了professor.id。这就是对于关系映射N-1的情况下，我们应该设计的数据的样子
@Entity
public class Professor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "professor_id")
    private Integer id;

    private String name;
}

//HibernateTest.java
public static void uniManyToOne() {
    Configuration conf = new Configuration().configure();
    StandardServiceRegistry registry = conf.getStandardServiceRegistryBuilder().build();
    try (SessionFactory sessionFactory = conf.buildSessionFactory(registry); Session session = sessionFactory.openSession()) {
        Transaction transaction = session.beginTransaction();

        Professor professor = new Professor();
        professor.setName("professor name one2one");

        Student student = new Student();
        student.setName("student name one2one");
        student.setProfessor(professor);

        Student student2 = new Student();
        student2.setName("student2");;
        student2.setProfessor(professor);

        session.save(student2);

        transaction.commit();
    }
}

public static void main(String[] args) {
    uniManyToOne();
} 
```



### Many to Many Mapping 多对多

1. JoinTable 实现

```java
//reader entity
@ManyToMany(cascade=CascadeType.ALL)
@JoinTable(name="READER_SUBSCRIPTIONS", joinColumns={@JoinColumn(referencedColumnName="ID")}
										, inverseJoinColumns={@JoinColumn(referencedColumnName="ID")})	
private Set<SubscriptionEntity> subscriptions; //新建一个READER_SUBSCRIPTIONS表

//subscription entity
@ManyToMany(mappedBy="subscriptions") //有mappedby,只在READER_SUBSCRIPTIONS表中显示？
private Set<ReaderEntity> readers;
```

其他例子

```java
@Entity
@Table(name = "student_info")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    private int age;

    @ManyToMany(targetEntity = Professor.class)
    @JoinColumn(name = "professor_id", referencedColumnName = "professor_id", nullable = false)
    private List<Professor> professorList = new ArrayList<>();
}

@Entity
public class Professor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "professor_id")
    private Integer id;

    private String name;
}

Transaction transaction = session.beginTransaction();

Professor professor = new Professor();
professor.setName("professor1");
session.save(professor);

Professor professor2 = new Professor();
professor2.setName("professor2");
session.save(professor2);

Student student = new Student();
student.setName("student name one2one");
student.getProfessorList().add(professor);
student.getProfessorList().add(professor2);

session.save(student);

Student student2 = new Student();
student2.setName("student2");
student2.getProfessorList().add(professor);
student2.getProfessorList().add(professor2);

session.save(student2);

transaction.commit();
```

双向many to many

```java
@Entity
public class Professor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "professor_id")
    private Integer id;

    private String name;

    @ManyToMany(targetEntity = Student.class)
    @JoinTable(joinColumns = @JoinColumn(name = "professor_id", referencedColumnName = "professor_id"))
    private List<String> studentList;
}

@Entity
@Table(name = "student_info")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;

    private int age;

    @ManyToMany(targetEntity = Professor.class)
    @JoinTable(joinColumns = @JoinColumn(name = "id", referencedColumnName = "id"))
//    @Column(name = "student_id")
    private List<Professor> professorList = new ArrayList<>();
    
    //@JoinTable：指定用连接表来保存映射关系(像前面的student_info_Professor)。需要注意的是，里面的@JoinColumn，为什么这里和以前的不一样，以前的JoinColumn(name, referencedColumnName)都是不同的，而这里为什么不一样？JoinColumn的意思是，本段的name属性应用了另一个表的referencedColumnName。由于这里指定了@JoinTable，也就是指定了用一个连接表，那么另一个表指的就是连接表。我们当然应该表这个表中的name属性和连接表中的name属性映射起来。即A.a 映射到连接表AB.a， B.b映射到连接到AB.b。

```



## 9.Cascade Type

### CadcadeType.All

*CascadeType.ALL* **propagates all operations — including Hibernate-specific ones — from a parent to a child entity.**

```java
@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;
    private String name;
    @OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    private List<Address> addresses;
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;
    private String street;
    private int houseNumber;
    private String city;
    private int zipCode;
    @ManyToOne(fetch = FetchType.LAZY) //?? 查询address时 不着急查person信息，如果需要查询，则需多一次sql query
    private Person person;
}
```



### CadcadeType.Persist

The persist operation makes a transient instance persistent. **Cascade Type \*PERSIST\* propagates the persist operation from a parent to a child entity.** When we save the *person* entity, the *address* entity will also get saved.

```java
@Test
public void whenParentSavedThenChildSaved() {
    Person person = new Person();
    Address address = new Address();
    address.setPerson(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    session.clear();
}

//followed SQL:
Hibernate: insert into Person (name, id) values (?, ?)
Hibernate: insert into Address (
    city, houseNumber, person_id, street, zipCode, id) values (?, ?, ?, ?, ?, ?)

```



### CadcadeType.Merge

The merge operation copies the state of the given object onto the persistent object with the same identifier. ***CascadeType.MERGE\* propagates the merge operation from a parent to a child entity.**

```java
@Test
public void whenParentSavedThenMerged() {
    int addressId;
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    addressId = address.getId();
    session.clear();

    Address savedAddressEntity = session.find(Address.class, addressId);
    Person savedPersonEntity = savedAddressEntity.getPerson();
    savedPersonEntity.setName("devender kumar");
    savedAddressEntity.setHouseNumber(24);
    session.merge(savedPersonEntity);
    session.flush();
}

//followed SQL:
Hibernate: select address0_.id as id1_0_0_, address0_.city as city2_0_0_, address0_.houseNumber as houseNum3_0_0_, address0_.person_id as person_i6_0_0_, address0_.street as street4_0_0_, address0_.zipCode as zipCode5_0_0_ from Address address0_ where address0_.id=?
Hibernate: select person0_.id as id1_1_0_, person0_.name as name2_1_0_ from Person person0_ where person0_.id=?
Hibernate: update Address set city=?, houseNumber=?, person_id=?, street=?, zipCode=? where id=?
Hibernate: update Person set name=? where id=?
    
//Here, we can see that the merge operation first loads both address and person entities and then updates both as a result of CascadeType.MERGE.
```



### CadcadeType.Remove

As the name suggests, the remove operation removes the row corresponding to the entity from the database and also from the persistent context.

***CascadeType.REMOVE\* propagates the remove operation from parent to child entity.** **Similar to JPA's \*CascadeType.REMOVE\*, we have \*CascadeType.DELETE\*, which is specific to Hibernate.** There is no difference between the two.

```java
@Test
public void whenParentRemovedThenChildRemoved() {
    int personId;
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    personId = person.getId();
    session.clear();

    Person savedPersonEntity = session.find(Person.class, personId);
    session.remove(savedPersonEntity);
    session.flush();
}

//followed sql:
Hibernate: delete from Address where id=?
Hibernate: delete from Person where id=?
//The address associated with the person also got removed as a result of CascadeType.REMOVE.


```



### CadcadeType.Detach

The detach operation removes the entity from the persistent context. **When we use \*CascadeType.DETACH\*, the child entity will also get removed from the persistent context.**

```java
@Test
public void whenParentDetachedThenChildDetached() {
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    
    assertThat(session.contains(person)).isTrue();
    assertThat(session.contains(address)).isTrue();

    session.detach(person);
    assertThat(session.contains(person)).isFalse();
    assertThat(session.contains(address)).isFalse();
}

//Here, we can see that after detaching person, neither person nor address exists in the persistent context.
```

### CadcadeType.Lock

**Unintuitively, \*CascadeType.LOCK\* reattaches the entity and its associated child entity with the persistent context again.**

```java
@Test
public void whenDetachedAndLockedThenBothReattached() {
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    
    assertThat(session.contains(person)).isTrue();
    assertThat(session.contains(address)).isTrue();

    session.detach(person);
    assertThat(session.contains(person)).isFalse();
    assertThat(session.contains(address)).isFalse();
    session.unwrap(Session.class)
      .buildLockRequest(new LockOptions(LockMode.NONE))
      .lock(person);

    assertThat(session.contains(person)).isTrue();
    assertThat(session.contains(address)).isTrue();
}

//As we can see, when using CascadeType.LOCK, we attached the entity person and its associated address back to the persistent context.
```

### CadcadeType.REFRESH

Refresh operations **reread the value of a given instance from the database.** In some cases, we may change an instance after persisting in the database, but later we need to undo those changes.

In that kind of scenario, this may be useful. **When we use this operation with Cascade Type \*REFRESH\*, the child entity also gets reloaded from the database whenever the parent entity is refreshed.**

```java
@Test
public void whenParentRefreshedThenChildRefreshed() {
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    person.setName("Devender Kumar");
    address.setHouseNumber(24);
    session.refresh(person);
    
    assertThat(person.getName()).isEqualTo("devender");
    assertThat(address.getHouseNumber()).isEqualTo(23);
}
//Here, we made some changes in the saved entities person and address. When we refresh the person entity, the address also gets refreshed.
```

### CadcadeType.REPLICATE

**The replicate operation is used when we have more than one data source and we want the data in sync.** With *CascadeType.REPLICATE*, a sync operation also propagates to child entities whenever performed on the parent entity.

```java
@Test
public void whenParentReplicatedThenChildReplicated() {
    Person person = buildPerson("devender");
    person.setId(2);
    Address address = buildAddress(person);
    address.setId(2);
    person.setAddresses(Arrays.asList(address));
    session.unwrap(Session.class).replicate(person, ReplicationMode.OVERWRITE);
    session.flush();
    
    assertThat(person.getId()).isEqualTo(2);
    assertThat(address.getId()).isEqualTo(2);
}
//Because of CascadeType.REPLICATE, when we replicate the person entity, its associated address also gets replicated with the identifier we set.
```

### CadcadeType.SAVE_UPDATE

*CascadeType.SAVE_UPDATE* propagates the same operation to the associated child entity. It's useful when we use **Hibernate-specific operations like \*save\*, \*update\* and \*saveOrUpdate\*.** 

```java
@Test
public void whenParentSavedThenChildSaved() {
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.saveOrUpdate(person);
    session.flush();
}

//followed sql
Hibernate: insert into Person (name, id) values (?, ?)
Hibernate: insert into Address (
    city, houseNumber, person_id, street, zipCode, id) values (?, ?, ?, ?, ?, ?)

//Because of CascadeType.SAVE_UPDATE, when we run the above test case, we can see that the person and address both got saved.

```



## 10. Persist, Transient, and Detached Object



### Transient Object

A `new` instance of a persistent class which is not associated with a `Session`, has no representation in the database and no identifier value is considered ***transient\*** by Hibernate:

```java
Person person = new Person();
person.setName("Foobar");
// person is in a transient state
```

### Persist Object

A ***persistent\*** instance has a representation in the database, an identifier value and is associated with a `Session`. You can make a transient instance ***persistent\*** by associating it with a `Session`:

```java
Long id = (Long) session.save(person);
// person is now in a persistent state
```

### Detached Object

Now, if we `close` the Hibernate `Session`, the persistent instance will become a ***detached\*** instance: it isn't attached to a `Session` anymore (but can still be modified and reattached to a new `Session` later though).



### Other Explanation:

**Transient** - Objects instantiated using the new operator are called transient objects.

An object is transient if it has just been instantiated using the new operator, and it is not associated with a Hibernate Session. It has no persistent representation in the database and no identifier value has been assigned. Transient instances will be destroyed by the garbage collector if the application does not hold a reference anymore.

**Persistent** - An object that has a database identity associated with it is called a persistent object.

A persistent instance has a representation in the database and an identifier value. It might just have been saved or loaded; however, it is by definition in the scope of a Session. Hibernate will detect any changes made to an object in persistent state and synchronize the state with the database when the unit of work completes.

**Detached** - A detached instance is an object that has been persistent, but its Session has been closed.

A detached instance can be reattached to a new Session at a later point in time, making it persistent again. This feature enables a programming model for long running units of work that require user think-time. We call them application transactions, i.e., a unit of work from the point of view of the user.













