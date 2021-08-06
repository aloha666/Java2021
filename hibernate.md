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

## 6. Proxy代理 Objects and Eager and Lazy Fetch拿取 Type

 主表和附表之间的互动，查询主表某个ID时，是不是需要主动查询出所有附表中含有此id的内容？有时需要，有时不需要，就需要进行设定。Hibernate default是不进行联动查询的。叫lazy iniitialaztion.

lazy iniitialaztion/fetch: hibernate 初始设定，不对附表进行联动，查什么给什么。如果只需要查一个表的信息，这种方法很快。

Eager fetch: 对附表进行联动，查主表时主动查附表

```java
@ElementCollection(fetch = FetchType.EAGER)
//ElementCollection meaning?
```



？ proxy object:  Hibernate 自带方法。作用？ 给object做一个proxy，功能和object相同，但是直接对database进行操作 object -> proxy -> database。好处？



## 7. Mapping

One to One Mapping 一对一

主表中加入一个附表ID， 附表不用进行操作。

```java
//in User.class
@OnetoOne //use annotation to clarfy
@JoinColumn(name="VEHICLE_ID") //在表中列名设为 VEHICLE_ID ？
private Vehicle vehicle;

//in test
Vehicle v1 = new Vehicle();
user.setVehicle(v1);

```



One to Many Mapping 一对多



Many to Many Mapping 多对多



## 8.Cascade Type

## 9. Persist, Transient, and Detach

## 

















