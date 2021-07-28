# Lecture 1

## Maven

Apache Maven is a **software project management and comprehension tool**.

### pom file

POM stands for "Project Object Model". It is an XML representation of a Maven project held in a file named pom.xml. The POM contains all necessary information about a project, as well as configurations of plugins to be used during the build process.

### dependency

In Maven, dependency is another archive—JAR, ZIP, and so on—which your current project needs in order to compile, build, test, and/or to run. The dependencies are gathered in the pom.xml file, inside of a <dependencies> tag.

### build tool

Maven is a build automation tool used primarily for Java projects.

### life cycle

Maven is based around the central concept of a build lifecycle. What this means is that the process for building and distributing a particular artifact (project) is clearly defined.

- `validate` - validate the project is correct and all necessary information is available
- `compile` - compile the source code of the project
- `test` - test the compiled source code using a suitable **unit testing** framework. These tests should not require the code be packaged or deployed
- `package` - take the compiled code and package it in its distributable format, such as a JAR.
- `verify` - run any checks on results of **integration tests** to ensure quality criteria are met
- `install` - install the package into the local repository, for use as a dependency in other projects locally
- `deploy` - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

## Git/GitHub

## JVM Runtime Memory Structure

![JVM Memory area parts](https://media.geeksforgeeks.org/wp-content/uploads/Memory.png)

![img](https://pic1.zhimg.com/80/v2-4face8109e0d52ef5894c41c69e4ec6b_720w.jpg?source=1940ef5c)

![img](https://img2020.cnblogs.com/other/1218593/202007/1218593-20200728164142971-1491375613.png)

https://www.cnblogs.com/javastack/p/13391983.html

Where a variable is stored depends on whether a variable is a **local variable** or an **instance variable**.**Local** variables are stored on the **stack**. **Instance** and **static** variables are stored on the **heap**.

Simplistic answer: it depends on where the variable is declared, not on its type.

### heap（共享）

在Java虚拟机中堆是所有线程都可以共享的内存区域，是存放所有类实例和数组对象的地方。在虚拟机启动就根据相关堆参数，创建堆，他也是垃圾收集器工作的主要区域。

It is a shared runtime data area and stores the **actual object** in a memory. It is instantiated during the virtual machine startup. There exists one and only one heap for a running JVM process.

### method area （共享）

It is a **logical part** of the heap area and is created on virtual machine startup.Needs not to be contiguous连续. 

**字符串常量池则存在于方法区**

![798b04f9ga86a4ef61c5b&690](https://segmentfault.com/img/bVPR6M?w=553&h=322)

在Java虚拟机中 方法区是可提供各个线程共享的运行时内存区域，它存储了每一个类的结构信息，例如运行时常量池,字段和方法数据,构造函数和普通函数的字节码内容，一句话总结就是存储**元数据 (metadata)**地方。

### stack （私有）

A stack is created at the same time when a **thread** is created and is used to store data and partial results which will be needed while returning value for method and performing dynamic linking.Needs not to be contiguous. 

**Local variabels, operating stacks, dynamic linking组成。**

### PC Counter （私有）

由于JVM同时可以处理多个线程所以就涉及到一些线程调度，当cpu暂停运行线程A把时间片让给线程B的时候我们需要保存线程A被暂停执行前的一些现场状态，需要记录当前执行到那一行字节码了，所以具备保存现场的功能。

每条线程都有自己的pc寄存器，在任意时刻虚拟机只会执行一个方法，如果执行的是方法不是native方法 pc寄存器则保存指向当前执行字节码的指令地址，如果执行的是native方法 pc寄存器会保存undefined。

Each **JVM thread** which carries out the task of a specific method has a program counter register associated with it. The non native method has a PC which stores the address of the available JVM instruction whereas in a native method, the value of program counter is undefined. PC register is capable of storing the return address or a native pointer on some specific platform.

A PC (Program Counter) Register contains the address of the instruction currently being executed in its associated thread. The PC Register is very small data area and has a fixed size. Java applications do not have any impact on its content and size.

### native method stack （私有，非Java method）

 This memory is allocated分配 for **each thread** when its created. And it can be of fixed or dynamic nature.

 A Native Method Stack stores similar data elements as a JVM Stack and it is used to help executing native (non-Java) methods. Java底层里调用别的语言代码的话就需要用到别的方法栈了,比如Java虚拟机的实现会用到传统的栈(C stack)来调用native方法，

问题： PC Counter 和 native mthod stack 作用？

## ClassLoader

The Java ClassLoader is a part of the Java Runtime Environment that dynamically loads Java classes into the Java Virtual Machine. The Java run time system does not need to know about files and file systems because of classloaders.

Java classes aren’t loaded into memory all at once, but when required by an application. At this point, the Java ClassLoader is called by the JRE and these ClassLoaders load classes into memory dynamically.

## 8 basic data type

There are 8: boolean , byte , char , short , int , long , float and double.

They don't have any method. Because primitive type variables are directly pointing to the value not object.

Can be compared with == 

All primiteive 

### Non-primitive Types

Annotations. Class. Interface. Enum. Array.

They are pointing to the reference of an object.

```java
Object o = new Object();
Object o1 = o;
// o1 and o are pointing to the save object
----------------------------------------------------
o1 == o; // true
o1.equals(o); // true, because by default equals is implemented by ==. Check the Object class.

class Apple{
  String color;
  Apple(String color){
    this.color = color;
  }
}

Apple a1 = new Apple("RED");
Apple a2 = new Apple("RED");

a1 == a2;  // false; They are different object.
a1.equals(a2); // false; We didn't override equals().
// to make a1.equals(a2) to return true
class Apple {
  //....
  public boolean equals(Object obj){
   // sanity check, type check
   //...
   Apple a = (Apple)obj;
   return a.color.equals(this.color);
  }
}
```

Compare with == : Will compare the reference value ( the pointed location) 基础数据类型：比较的是他们的值是否相等，比如两个int类型的变量，比较的是变量的值是否一样。引用数据类型：比较的是引用的地址是否相同，比如说新建了两个User对象，比较的是两个User的地址是否一样。

Compare with equals(Object obj) : Will call the equals method of the object (default is ==, but we can override, like String) 用来检测两个对象是否相等，即两个对象的内容是否相等。

```java
//equals()和==的源码定义：
public boolean equals(Object obj) {
return (this == obj);
}
//由equals的源码可以看出这里定义的equals与==是等效的（Object类中的equals没什么区别），不同的原因就在于有些类（像String、Integer等类）对equals进行了重写。
//但是没有对equals进行重写的类就只能从Object类中继承equals方法，其equals方法与==就也是等效的，除非在此类中重写equals。
```

### Passed By Value vs Passed by Reference

All types are passed by value when calling a method.

```java
public void method1(){
  int i = 100;
  doSomething(i);
  System.out.println(i); // 100
}


public void doSomething(int i) {. // the i here is a local variable. value is copied.
  i = 200;  // because value is copied, changing a copied value won't change the i in 
            //method1();
}
----------------------------------------------------
public void method1(){
  Apple a1 = new Apple("RED");
  doSomething(a1);
  System.out.println(a1.color); // GREEN 
}

public void doSomething(Apple a){ // a can be any name, because it will be a copy of 
                                  // reference. a and a1 pointing to the same object, but 
                                  // they are difference references

  a.color="GREEN";                // Object's content is changed. So both a and a1 
                                  //changed.

}
-----------------------------------------------------
public void method1(){
  Apple a1 = new Apple("RED");
  doSomething(a1);
  System.out.println(a1.color); // RED  
}

public void doSomething(Apple a){ 
  a = new Apple("RED"); // here we create a new Apple. and a is its reference now.
  a.color="GREEN";      // so changing the new Object value won't do anything to a1.
}
```



## String, Integer Object

### equals/hashcode method

## Collection data structure: List, Set, Queue, Map

### Set vs List

List: An ordered collection. Allow duplicated(how to define? explaination in below) element. Commonly used ArrayList, LinkedList. Resize factor:0.5

Set: A collection that contains **no duplicate** elements. Sets contain no pair of elements e1 and e2 such that **e1.equals(e2)**, and at most one null element.

### List: ArrayList vs LinkedList

### Set: HashSet, TreeSet, LinkedHashSet

HashSet - Internally using HashMap to save the elements as the keys, value is a dummy Object o = new Object();

```java
Example
a1 = new Apple("red"); 
a2 = new Apple("red");
a1.equals(a2);  // if true, then we must override equals() method in Apple class and indicate the color comparison. 
// also by contract, we have to override hashCode() method to ensure the same elements return same hashCode.
-------------------------------------
class Apple {
  private String color;
  Apple(String color) {
    this.color = color;
  }
}
//...
public void doSomething(){
  Set<Apple> aSet = new HashSet<>();
  aSet.add(new Apple("RED"));
  aSet.add(new Apple("RED"));
  aSet.size(); // 2, because Apple doesn't override equals method. by default ==. 
               // so hashSet treat two new Apple as two objects.
}
//...
// if override equals using color field. aSet.size() will be 1. Try it.
-----------------------------------
public class Main {

    public static class Watermelon{
        String color;
        int size;

        public Watermelon(String color, int size){
            this.color=color;
            this.size=size;
        }

        @Override
        public boolean equals(Object o){  //override equals method
            if(((Watermelon)o).color.equals(color) && ((Watermelon)o).size==size) return true;
            return false;
        }

        @Override
        public int hashCode(){ //override hashcode method
            return Objects.hash(color,size);
        }

    }

    public static void main(String[] args) {
	// write your code here
        Watermelon a1 = new Watermelon("green", 10);
        Watermelon a2 = new Watermelon("green", 10);
        Watermelon a3 = new Watermelon("green", 10);
        Watermelon a4 = a3;

        HashMap<Watermelon,Integer> map= new HashMap<>();

        map.put(a1,1);
        map.put(a2,2);
        map.put(a3,3);
        map.put(a4,4);

        System.out.print(map); // {a4=4}
        System.out.print(map.size()); // 1
    }
}
```

### Map: HashMap, ConcurrentHashMap

An object that maps keys to values. A map cannot contain duplicate keys; each key can map to at most one value.

In order to make it fast to locate the key-value pair in Big O one time. **Hahsmap internally uses an array of linkedlist**.

Each element in this array is a bucket. Hashmap uses the **hashcode()** method to calculate the **index** of the target bucket. 

After finding the bucket, it uses the **equals()** method to check if there is duplicate key. 

if the operation is get(), then it will return the key-value paire, 

if it is put(), it will overwrite the key-value with the new value.

If there are two keys having the same hashCode(), due to the nature of hashmap, those two keys will use the same bucket. this is **hash collision**. 

The solution of hash collision is to use linkedlist to save all k-v pair and use equals() method to check the duplication of key.

```java
// Create a map of Char-occurency in a string
String str = "AABBCCDD";

HashMap<Character, Integer> data = new HashMap<>();

for (Character c : str.toCharArray()) {

    data.put(c, data.getOrDefault(c, 1));

}
```

### Linked HashMap

Maintain the insertion order of the key-value pairs. This implementation differs from HashMap in that it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is normally the order in which keys were inserted into the map (insertion-order). Note that insertion order is not affected if a key is re-inserted into the map.

##### TreeMap

The map is sorted according to the natural ordering of its keys, or by a Comparator provided at map creation time, depending on which constructor is used.

# Lecture 2

## JVM runtime map overview + example 

### ClassLoder:

1.BootStrap ClassLoader

A Bootstrap Classloader is a Machine code which kickstarts the operation when the JVM calls it. It is not a java class. Its job is to load the first pure Java ClassLoader. Bootstrap ClassLoader loads classes from the location rt.jar. Bootstrap ClassLoader doesn’t have any parent ClassLoaders. It is also called as the Primodial ClassLoader.

2.Extension ClassLoader

The Extension ClassLoader is a child of Bootstrap ClassLoader and loads the extensions of core java classes from the respective JDK Extension library. It loads files from jre/lib/ext directory or any other directory pointed by the system property java.ext.dirs.

3.System ClassLoader

An Application ClassLoader is also known as a System ClassLoader. It loads the Application type classes found in the environment variable CLASSPATH, -classpath or -cp command line option. The Application ClassLoader is a child class of Extension ClassLoader.

### Byte-Code Verifier

### Just-In-Time Compile

## Keyword

### 	class v.s. interface

### 	primitive data type

Auto-boxing/Unboxing	

*Autoboxing* is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes. For example, converting an `int` to an `Integer`, a `double` to a `Double`, and so on. If the conversion goes the other way, this is called *unboxing*.

The Java compiler applies unboxing when an object of a wrapper class is: **Passed as a parameter to a method that expects a value of the corresponding primitive type.**

Wrapper classes are those whose objects wraps a primitive data type within them. In the ***java.lang\*** package java provides a separate class for each of the primitive data type namely Byte, Character, Double, Integer, Float, Long, Short.

Converting primitive datatype to object is called boxing.

```java
//Unboxing
import java.util.ArrayList;
import java.util.List;

public class Unboxing {

    public static void main(String[] args) {
        Integer i = new Integer(-8);

        // 1. Unboxing through method invocation
        int absVal = absoluteValue(i);
        System.out.println("absolute value of " + i + " = " + absVal);

        List<Double> ld = new ArrayList<>();
        ld.add(3.1416);    // Π is autoboxed through method invocation.

        // 2. Unboxing through assignment
        double pi = ld.get(0);
        System.out.println("pi = " + pi);
    }

    public static int absoluteValue(int i) {
        return (i < 0) ? -i : i;
    }
}

```



### 	access modifier

default v.s. protected :

The Protected access specifier is visible within the s**ame package and also visible in the subclass(even not in same package)**, the Default is a package 	level access specifier and it can be visible in the same package.

### 	inheritance 

​		implements v.s. extends

### 	control flow:

​		break v.s. continues 

### 	instantiation: 

​		this v.s. super

### 	immutable class

​		final, static 

​		The main difference between a static and final keyword is that static is keyword is used to define the class member that can be used independently of any 		object of that class. Final keyword is used to declare, a constant variable, a method which can not be overridden and a class that can not be inherited.

### OOP: encapsulation

### 	  abstraction

### 	  polymorphism: 

  		1. Inheritance 
  		2. Override and Overload. 
  		3. Upper casting： Upcasting is the typecasting of **a child object to a parent object**. Upcasting gives us the flexibility to access the parent class members but it is not possible to access all the child class members using this feature. 子类可以用到任何需要父类的地方，反之则不行。

## Exception Handling

### 	Exception v.s. Error

![Java Exception Hierarchy](https://i.stack.imgur.com/v2NAj.png)

An Error is a subclass of Throwable that indicates serious problems that a reasonable application should not try to catch. Most such errors are abnormal conditions.

### 	Exception: checked/unchecked Exception

Checked Exception: Has to be try-catched or decleared using throws keyword on method.

UnChecked Exception: Don't need to be try-catched or decleared.

Throwable is the parent class of all exceptions and errors.

### 	Exception Handling: 

​	 1. try-catch-finally (try with resource) 

  	 2. throws Customized Exception



# Lecture 3

## stackoverflow vs out of memory

- `OutOfMemoryError` is related to Heap. is thrown when you are creating objects

- `StackOverflowError` is related to stack. is thrown when you are calling functions.

  

  When you start `JVM` you define how much RAM it can use for processing. `JVM` divides this into certain memory locations for its processing purpose, two of those are **`Stack` & `Heap`**

  If you have large objects (or) referenced objects in memory, then you will see `OutofMemoryError`. If you have strong references to objects, then GC can't clean the memory space allocated for that object. When JVM tries to allocate memory for new object and not enough space available it throws `OutofMemoryError` because it can't allocate the required amount of memory.

  **How to avoid**: Make sure un-necessary objects are available for GC

  All your local variables and methods calls related data will be on the stack. For every method call, one stack frame will be created and local as well as method call related data will be placed inside the stack frame. Once method execution is completed, the stack frame will be removed. ONE WAY to reproduce this is, have an infinite loop for method call, you will see `stackoverflow` error, because stack frame will be populated with method data for every call but it won't be freed (removed).

  **How to avoid**: Make sure method calls are ending (not in an infinite loop)

  

## Generics

Reference to Jakov.

## Enum

A *Java Enum* is a special Java type used to define collections of constants. More precisely, a Java enum type is a special kind of Java class. An enum can contain constants, methods etc. Java enums were added in Java 5.(Before is Object, after is Enum)

Enum的constructor是private，实际上是signleton, aviod change later on, 这样Enum可以作为constants的collection.

```java
public enum Level {
    HIGH,
    MEDIUM,
    LOW
}

Level level = Level.HIGH;
--------------------------------
public enum Level {
    HIGH  (3),  //calls constructor with value 3
    MEDIUM(2),  //calls constructor with value 2
    LOW   (1)   //calls constructor with value 1
    ; // semicolon needed when fields / methods follow


    private final int levelCode;

    private Level(int levelCode) {
        this.levelCode = levelCode;
    }
}
```



## Annotation

https://zhuanlan.zhihu.com/p/85612062 Java注解总结

What is an Annotation for ?

- Compiler instructions
- Build-time instructions
- Runtime instructions

A Java annotation can have elements for which you can set values. An element is like an attribute or parameter.

```java
@Entity(tableName = "vehicles")
```

You can place Java annotations above classes, interfaces, methods, method parameters, fields and local variables.

```java
@Entity
public class Vehicle {
}
```

##### Built-in Java Annotations

```java
//The @Deprecated annotation is used to mark a class, method or field as deprecated, meaning it should no longer be used.
@Deprecated
public class MyComponent {

}

//The @Override Java annotation is used above methods that override methods in a superclass. If the method does not match a method in the superclass, the compiler will give you an error.

public class MySuperClass {

    public void doTheThing() {
        System.out.println("Do the thing");
    }
}


public class MySubClass extends MySuperClass{

    @Override
    public void doTheThing() {
        System.out.println("Do it differently");
    }
}

//The @SuppressWarnings annotation makes the compiler suppress warnings for a given method. For instance, if a method calls a deprecated method, or makes an insecure type cast, the compiler may generate a warning. 

@SuppressWarnings
public void methodWithWarning() {


}
```



##### Constumized Java Annotations



```java
public @interface MyAnnotation {

    String   value() default "hello";
    String   name();
    int      age();
    String[] newNames();

}
---------------
//@Retention: SOURCE, CLASS(DEFAULT), RUNTIME 有什么区别？
  //SOURCE:discard by the compiler
  //CLASS: recoreded in class by complier, but need not be retained by the VM at run time.
  //RUNTIME: recoared in the class file by the compiler and retained by the VM at run time, so they may be reflectively.
//You can specify for your custom annotation if it should be available at runtime, for inspection via reflection. You do so by annotating your annotation definition with the @Retention annotation. Here is how that is done:
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)

@interface MyAnnotation {

    String   value() default "";

}

------
//@Target(Type, FIELD, METHOD,PARAMETER,CONSTRUCTOR,LOCAL_VARIABLE,ANNOTATION_TYPE,PACKAGE,TYPE_PARAMETER,TYPE_USE)
//You can specify which Java elements your custom annotation can be used to annotate. You do so by annotating your annotation definition with the @Target annotation. Here is a @Target Java annotation example:

import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target({ElementType.METHOD})
public @interface MyAnnotation {

    String   value();
}
--------
//@Inherited
//The @Inherited annotation signals that a custom Java annotation used in a class should be inherited by subclasses inheriting from that class. Here is an @Inherited Java annotation example:

java.lang.annotation.Inherited

@Inherited
public @interface MyAnnotation {

}
@MyAnnotation
public class MySuperClass { ... }
public class MySubClass extends MySuperClass { ... }
--------
//@Documented
//The @Documented annotation is used to signal to the JavaDoc tool that your custom annotation should be visible in the JavaDoc for classes using your custom annotation. Here is a @Documented Java annotation example:

import java.lang.annotation.Documented;

@Documented
public @interface MyAnnotation {

}
@MyAnnotation
public class MySuperClass { ... }
```



## IO Stream

##### File Class

要构造一个`File`对象，需要传入文件路径：

```java
public class Main {
    public static void main(String[] args) {
        File f = new File("C:\\Windows\\notepad.txt");
        System.out.println(f);
    }
}

-----------
//创建newfile
File file1 = new File("C:\\Windows\\notepad.txt")；
if(!file1.exists()){
  file1.createNewFile();
}

-----------
boolean mkdir()：//创建当前File对象表示的目录；
boolean mkdirs()：//创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
```

##### ByteStream vs CharacterStream

ByteStream: InputStream, OutputStream 它们以8-bit为单位处理数据，即字节流类读取/写入8位数据。使用这些可以存储字符，视频，音频，图像等。Byte oriented reads byte by byte.

CharacterStream: Reader, Writer 这些以16-bit Unicode处理数据。使用这些只能读取和写入**文本数据**(text files)。

中文乱码？一个中文字符占两个字节 若只读到一半就会乱码。ASCII 码中，**一个**英文字母（不分大小写）为**一个字节**，**一个中文汉字**为两**个字节**。 UTF-8 编码中，**一个**英文字为**一个字节**，**一个中文**为三**个字节**。 Unicode 编码中，**一个**英文为**一个字节**，**一个中文**为两**个字节**。1byte = 8bit 

乱码详解：https://blog.csdn.net/qq_37168286/article/details/80033072

##### FileStream vs BufferStream

File Stream: FileInputStream(Byte based), FileOutputStream(Byte based), FileReader(Character based), FileWriter(Character based)

The *Java* *InputStream* class, `java.io.InputStream`, represents an ordered stream of **bytes**. In other words, you can read data from a Java `InputStream` as an ordered sequence of bytes. This is useful when reading data from a file, or received over the network.

BufferStream: BufferedInputStream, BufferedOutputStream, BufferedReader, BufferedWriter

The *Java* *BufferedInputStream* class, java.io.BufferedInputStream, provides transparent reading of **chunks of bytes** and buffering for a [Java InputStream](http://tutorials.jenkov.com/java-io/inputstream.html), including any subclasses of InputStream. **Reading larger chunks of bytes and buffering them can speed up IO quite a bit.** Rather than read one byte at a time from the network or disk, the BufferedInputStream reads **a larger block at a time into an internal buffer.** When you read a byte from the Java BufferedInputStream you are therefore reading it from its internal buffer. When the buffer is fully read, the BufferedInputStream reads another larger block of data into the buffer. This is typically much faster than reading a single byte at a time from an InputStream, especially for disk access and larger data amounts.

# Lecture 4

## IO Stream Continue

![1](/Users/spikycrown/Desktop/lec notes/1.png)

### InputStreamReader(Transfer Stream) vs FileReader

InputStream读bytes然后转化成character，同时可以指定编码的方式(GBK, UTF-8, etc)，filereader直接读character。

`InputStreamReader` can handle all input streams, not just files. Other examples are network connections, classpath resources and ZIP files.

FileReader reads character from a file in the file system. InputStreamReader reads characters from any kind of input stream. The stream cound be a FileInputStream, but could also be a stream obtained from a socket, an HTTP connection, a database blob, whatever.

"I usually prefer using an InputStreamReader wrapping a FileInputStream to read from a file because it allows specifying a specific character encoding."

**为何要用char[]来读input String？**用来减少IO inter actinons between JVM and hard disk. 比一个一个读要快

**为何不直接创建一个string size的char[]?** trade performance with memory size, 越大越需要memeory空间

**flush():** Flushes the output stream and forces any buffered output bytes to be written out. The general contract of flush is that calling it is an indication that, if any bytes previously written have been buffered by the implementation of the output stream, such bytes should immediately be written to their intended destination. The buffering is mainly done to improve the I/O performance.

**close():** always close a buffer after use.

### Object Stream



### Serializable Interface

Use with objectstream, stream could be unreadable after serialized - writeObject(). Only readable after deserialization-readObject().

序列化：对象的寿命通常随着生成该对象的程序的终止而终止，有时候需要把在内存中的各种对象的状态（也就是实例变量，不是方法）保存下来，并且可以在需要时再将对象恢复。虽然你可以用你自己的各种各样的方法来保存对象的状态，但是Java给你提供一种应该比你自己的好的保存对象状态的机制，那就是序列化。

总结：Java 序列化技术可以使你将一个对象的状态写入一个Byte 流里（系列化），并且可以从其它地方把该Byte 流里的数据读出来（反序列化）

```java
class Person implements Serializable{	
	private static final long serialVersionUID = 1L; //unique id LONG 一个是固定的 1L
// 一个是随机生成一个不重复的 long 类型数据（实际上是使用 JDK 工具，根据类名、接口名、成员方法及属性等来生成）
//这个ID可以很好的track object的class ID
	String name;
	int age;
	public Person(String name,int age){
		this.name = name;
		this.age = age;
	}	
	public String toString(){
		return "name:"+name+"\tage:"+age;
	}
}

    File file = new File("file"+File.separator+"out.txt");
	
	FileOutputStream fos = null;
	try {
		fos = new FileOutputStream(file);
		ObjectOutputStream oos = null;
		try {
			oos = new ObjectOutputStream(fos);
			Person person = new Person("tom", 22);
			System.out.println(person);
			oos.writeObject(person);			//写入对象
			oos.flush();
		} catch (IOException e) {
			e.printStackTrace();
		}finally{
			try {
				oos.close();
			} catch (IOException e) {
				System.out.println("oos关闭失败："+e.getMessage());
			}
		}
	} catch (FileNotFoundException e) {
		System.out.println("找不到文件："+e.getMessage());
	} finally{
		try {
			fos.close();
		} catch (IOException e) {
			System.out.println("fos关闭失败："+e.getMessage());
		}
	}
							
	FileInputStream fis = null;
	try {
		fis = new FileInputStream(file);
		ObjectInputStream ois = null;
		try {
			ois = new ObjectInputStream(fis);
			try {
				Person person = (Person)ois.readObject();	//读出对象
				System.out.println(person);
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			} 
		} catch (IOException e) {
			e.printStackTrace();
		}finally{
			try {
				ois.close();
			} catch (IOException e) {
				System.out.println("ois关闭失败："+e.getMessage());
			}
		}
	} catch (FileNotFoundException e) {
		System.out.println("找不到文件："+e.getMessage());
	} finally{
		try {
			fis.close();
		} catch (IOException e) {
			System.out.println("fis关闭失败："+e.getMessage());
		}
	}
```

How is UID transmitted?

UID is on the template and needed by both serialization and deserialization. (?)

### Static & Transient Keyword

Serialization只能保存对象的非静态成员变量，不能保存任何的成员方法和静态的成员变量，而且Serialization保存的只是变量的值，对于变量的任何修饰符都不能保存。即序列化后不能通过反序列化恢复。

如果把Person类中的name定义为static类型的话，试图重构，就不能得到原来的值，只能得到null。说明对静态成员变量值是不保存的。这其实比较容易理解，序列化保存的是对象的状态，静态变量属于类的状态，因此 序列化并不保存静态变量。 

transient关键字的作用是：阻止实例中那些用此关键字声明的变量持久化；当对象被反序列化时（从源文件读取字节序列进行重构），这样的实例变量值不会被持久化和恢复。当某些变量不想被序列化，同是又不适合使用static关键字声明，那么此时就需要用transient关键字来声明该变量。

static 和 transient 区别：transient修饰的变量跟普通的成员变量在使用上没有任何区别，但是static就是static变量。

## Java 8 features - Lambda Expression

one line statement, a simpified anonymous method.

![img](https://pic3.zhimg.com/80/v2-a712753b42972e094a548ae02fa82987_720w.jpg?source=1940ef5c)

https://www.zhihu.com/question/20125256

(x,y) -> x= x+1; **can not do, lambda expression can not change parameter value.**

双冒号：**类名::方法名**

```java
//表达式:
person -> person.getAge();
//可以替换成
Person::getAge

//表达式
() -> new HashMap<>();
//可以替换成
HashMap::new
```



From Java 8 onwards, **lambda expressions can be used** to represent the instance of a functional interface.

```java

// A simple program to demonstrate the use
// of predicate interface
import java.util.*;
import java.util.function.Predicate;
  
class Test
{
    public static void main(String args[])
    {
  
        // create a list of strings
        List<String> names =
            Arrays.asList("Geek","GeeksQuiz","g1","QA","Geek2");
  
        // declare the predicate type as string and use
        // lambda expression to create object
        Predicate<String> p = (s)->s.startsWith("G");
  
        // Iterate through the list
        for (String st:names)
        {
            // call the test method
            if (p.test(st))
                System.out.println(st);
        }
    }
}
```

```java
//lambda Expression practice

//1.use lambda Expression to implement runnable interface:

//before java8
new Thread(new Runnable(){
  @Override
  public void run(){
    System.out.print("before java 8");
  }
}).start();

//Lambda
new Thread(()->System.out.print("java 8"));

// (params) -> expression
// (params) -> statement
// (params) -> {statement}


//2.use lambda expression to handle action

// before Java 8：
JButton show =  new JButton("Show");
show.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
    System.out.println("Event handling without lambda expression is boring");
    }
}); 

// Java 8：
show.addActionListener((e) -> {
    System.out.println("Light, Camera, Action !! Lambda expressions Rocks");
});


//3. use lambda expression to iterate list/collections

List features = Arrays.asList("Lambdas", "Default Method", "Stream API", "Date and Time API");

//before
for(String s : features) {
  System.out.print(s);
}

//Java 8
features.forEach(n->System.out.print(n));
//or
features.forEach(System.out::print);


//4. use lambda and functional interface predicate

//before
public static void main(String[] args) {
    List<String> languages = Arrays.asList("Java", "Scala", "C++", "Haskell", "Lisp");

    System.out.println("Languages which starts with J :");
    filter(languages, (str)->((String)str).startsWith("J"));

    System.out.println("Languages which ends with a ");
    filter(languages, (str)->((String)str).endsWith("a"));

    System.out.println("Print all languages :");
    filter(languages, (str)->true);

    System.out.println("Print no language : ");
    filter(languages, (str)->false);

    System.out.println("Print language whose length greater than 4:");
    filter(languages, (str)->((String)str).length() > 4);
}

public static void filter(List<String> names, Predicate condition) {
    for(String  name: names)  {
        if(condition.test(name)) {
            System.out.println(name + " ");
        }
    }
}

//java 8
public static void filter(List names, Predicate condition) {
    names.stream().filter((name) -> (condition.test(name))).forEach((name) -> {
        System.out.println(name + " ");
    });
}

//5.lambda map and reduce

List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
//before
for (Integer cost : costBeforeTax) {
    double price = cost + .12*cost;
    System.out.println(price);
}

//java 8 
costBeforeTax.stream().map((cost) -> cost + .12*cost).forEach(System.out::println);
//Or use reduce(折叠操作) to calculate sum
double bill = costBeforeTax.stream().map((cost) -> cost + .12*cost).reduce((sum, cost) -> sum + cost).get();
System.out.println("Total : " + bill); 


//6. use filter
List<String> filtered = strList.stream().filter(x -> x.length()> 2).collect(Collectors.toList());
System.out.printf("Original List : %s, filtered list : %s %n", strList, filtered);
//Original List : [abc, , bcd, , defg, jk], filtered list : [abc, bcd, defg] 


//7.use map to apply function on element
List<String> G7 = Arrays.asList("USA", "Japan", "France", "Germany", "Italy", "U.K.","Canada");
String G7Countries = G7.stream().map(x -> x.toUpperCase()).collect(Collectors.joining(", "));
System.out.println(G7Countries);

//USA, JAPAN, FRANCE, GERMANY, ITALY, U.K., CANADA 

//8. use stream to remove dulplicate

// 用所有不同的数字创建一个正方形列表
List<Integer> numbers = Arrays.asList(9, 10, 3, 4, 7, 3, 4);
List<Integer> distinct = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
System.out.printf("Original List : %s,  Square Without duplicates : %s %n", numbers, distinct); 

//Original List : [9, 10, 3, 4, 7, 3, 4],  Square Without duplicates : [81, 100, 9, 16, 49] 

//9. to calculate colletions's max/min/sum/average

List<Integer> primes = Arrays.asList(2, 3, 5, 7, 11, 13, 17, 19, 23, 29);
IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("Highest prime number in List : " + stats.getMax());
System.out.println("Lowest prime number in List : " + stats.getMin());
System.out.println("Sum of all prime numbers : " + stats.getSum());
System.out.println("Average of all prime numbers : " + stats.getAverage());
//Highest prime number in List : 29
//Lowest prime number in List : 2
//Sum of all prime numbers : 129
//Average of all prime numbers : 12.9

```

### Lambda表达式 vs 匿名类

既然lambda表达式即将正式取代Java代码中的匿名内部类，那么有必要对二者做一个比较分析。一个关键的不同点就是关键字 **this**。匿名类的 **this** 关键字指向匿名类，而lambda表达式的 **this** 关键字指向包围lambda表达式的类。另一个不同点是二者的编译方式。Java编译器将lambda表达式编译成类的私有方法。使用了Java 7的 **invokedynamic** 字节码指令来动态绑定这个方法。



## Java 8 features - Functional Interface

a functional interface only has one abstract method. 

![2](/Users/spikycrown/Desktop/lec notes/2.png)

| 口诀                                 | 接口签名       | 接口方法          |
| ------------------------------------ | -------------- | ----------------- |
| 应用(apply)函数(Function)，有进有出  | Function<T, R> | R apply(T t)      |
| 接受(accept)消费(Consumer)，只进不出 | Consumer<T>    | void accept(T t)  |
| 测试(test)谓词(Predicate)，返回真假  | Predicate<T>   | boolean test(T t) |
| 获得(get)供给(Supplier)，不劳而获    | Supplier<T>    | T get()           |

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}

@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}

@FunctionalInterface
public interface Supplier<T> {
    T get();
}

@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

##### predicate interface

##### Method Reference

## Java 8 features - Stream API

## ![3](/Users/spikycrown/Desktop/lec notes/3.png) 

### What is a stream?

A stream does not store data and, in that sense, is not a data structure. It also never modifies the underlying data source.This functionality – java.util.stream – supports functional-style operations on streams of elements, such as map-reduce transformations on collections.
声明但不计算 最后需要取值再计算， can be infinit.(https://www.runoob.com/java/java8-streams.html)  

 A steam is a list will not save the element by it will do the calculation when needed
 这些操作对Stream来说可以分为两类，一类是转换操作，即把一个Stream转换为另一个Stream，例如map()和filter()，另一类是聚合操作，即对Stream的每个元素进行计算，得到一个确定的结果，例如reduce()。Stream方法： foreach遍历， map映射， filter过滤， limit指定输出，sorted排序好处：work with lambda get rid of helper functions 增加生产力

https://juejin.cn/post/6844903830254010381

Do things in one line. 流表示包含着一系列元素的集合，我们可以对其做不同类型的操作，用来对这些元素执行计算。

```java
List<String> myList =
    Arrays.asList("a1", "a2", "b1", "c2", "c1");

myList
    .stream() // 创建流
    .filter(s -> s.startsWith("c")) // 执行过滤，过滤出以 c 为前缀的字符串
    .map(String::toUpperCase) // 转换成大写
    .sorted() // 排序
    .forEach(System.out::println); // for 循环打印

// C1
// C2
--------------------------------
Arrays.asList("a1", "a2", "a3")
    .stream() // 创建流
    .findFirst() // 找到第一个元素
    .ifPresent(System.out::println);  // 如果存在，即输出

// a1

-------------------------------
  Stream.of("a1", "a2", "a3")
    .findFirst()
    .ifPresent(System.out::println);  // a1


```

**①**：中间操作会再次返回一个流，所以，我们可以链接多个中间操作，注意这里是不用加分号的。上图中的`filter` 过滤，`map` 对象转换，`sorted` 排序，就属于中间操作。

**②**：终端操作是对流操作的一个结束动作，一般返回 `void` 或者一个非流的结果。上图中的 `forEach`循环 就是一个终止操作。

当且仅当存在终端操作时，中间操作操作才会被执行。

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return true;
    });
//不会打印
-----------------------------------------
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return true;
    })
    .forEach(s -> System.out.println("forEach: " + s));
/*
filter:  d2
forEach: d2
filter:  a2
forEach: a2
filter:  b1
forEach: b1
filter:  b3
forEach: b3
filter:  c
forEach: c
*/

//事实上，输出的结果却是随着链条垂直移动的。比如说，当 Stream 开始处理 d2 元素时，它实际上会在执行完 filter 操作后，再执行 forEach 操作，接着才会处理第二个元素。出于性能的考虑。这样设计可以减少对每个元素的实际操作数，

```



# Lecture 5



## Java 8 features - Optional function

optional function is to **prevent NullpointerExcenption**.

methods: 

isPresent(): check optional instance is empty or not

of(object): pop out nullpointerexception if object is null

ofNullable(object): accept null value if object is null

## MultiThreading

### Process vs Thread

**process**: an execution of a program, OS will assgin memory for each process.

**thread**: an execution routine of process within a precess which share the same memeory area.

**why we need multithreading?**  To achiece multi tasking

One CPU: do threads concrrently, may be not parellelly, (jump from 1 to 2, then jump back to 1, etc)

multi CPU: do threads concreeemtly and parellelly



### Thread Creation

**4ways:** 

**1.extend thread class**

**2.implement runnable interface**

**3.implement callable interface**

callabel can have a return value

callable can thorw an exception

**4.threadpool (Executors, excutorService, fork-join pool, completableFuture)**



### Thread Methods:

![img](https://www.baeldung.com/wp-content/uploads/2018/02/Java_-_Wait_and_Notify.png)

**Thread.sleep():** Thread. sleep **causes the current thread to suspend execution for a specified period**.  This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system. 暂停当前线程，把cpu片段让出给其他线程，减缓当前线程的执行。**Thread**.**Sleep**(0)的**作用**，就是“触发操作系统立刻重新进行一次CPU竞争，重新计算优先级。

**this.wait():**to suspend a thread

**this.notify():**to wake a thread up

**this.notifyAll():**This method simply wakes all threads that are waiting on this object's monitor. The awakened threads will complete in the usual manner, like any other thread.



### States of a thread: 

**New :** a thread not yet start

**Runnable**: a thread is runnable/executing

**Blocked**: a thread waiting for a monitor lock, after calling **object.wait()**

**Waitting**: a thread waiting for another thread to perform a paticular action, after calling **object.wait()** with no time out, **thread.join()** with no time out, **LockSupport.park**

**Timed_watiing**: thread state for a waiting thread with a specified time. **Thread.Sleep**, **object.wait()** with  time out, **thread.join()** with time out, **LockSupport.park**, **LockSupport.parkUntil**

**Terminated**: thread has complete execution and terminated

**sleep vs wait**: wait will release lock, while sleep will not



### Synchronized vs Lock

**Synchorzied**: used on method, coupling and slow, the execuation will be in a random order (?). In other words, use synchorized with notifyAll( ) cannot contorl the order of thread execution.

The **java.lang.Object.notifyAll()** wakes up all threads that are waiting on this object's monitor. A thread waits on an object's monitor by calling one of the wait methods.

**Lock:** a interface that lock() the resource for the thread to use, will unlock()  after use. wait will release lock, while sleep will not.

lock condition: control flow 

**Start():** NATIVE METHOD start0( ); writted in C language, will start/run a thread.

normally we cannot have a abstract method in a non-abstract class. BUT CAN HAVE A NATIVE METHOD.

总结：

**synchronized**: wait(), notify()/notifyAll(), execuation with random order based on OS

**Lock + condition**: await(), signal()/signalAll(), execution with assigned order based on programmers requirement.



## Thread Pool

A thread pool is used to aviod cosuming resources: repetively using existing thread which could reduce the cost of new and GC threads increase response time: Once the task is ready to be executed, we do not need to wait for the thread creation, which improve multi-thread management.

线程池顾名思义就是事先创建若干个可执行的线程放入一个池中，需要的时候就从池中获取线程不用自行创建，使用完毕不用销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。

### ExexcutorService

FixedThreadPool: a pool with a fixed size

CachedThreadPool: a pool with a dynamic size

### Customrized pool

### Fork Join

It provides tools to help speed up parallel processing by attempting to use all available processor cores – which is accomplished **through a divide and conquer approach**.

它的原理就是内部通过Fork/Join对大数组分拆进行并行排序，在多核CPU上就可以大大提高排序的速度。

- `fork()`：开启一个新线程（或是重用线程池内的空闲线程），将任务交给该线程处理。
- `join()`：等待该任务的处理线程处理完毕，获得返回值。

```java
//https://blog.csdn.net/qq_32331073/article/details/81504262

public class ForkJoinCalculator implements Calculator {
    private ForkJoinPool pool;
 
    private static class SumTask extends RecursiveTask<Long> {
        private long[] numbers;
        private int from;
        private int to;
 
        public SumTask(long[] numbers, int from, int to) {
            this.numbers = numbers;
            this.from = from;
            this.to = to;
        }
 
        @Override
        protected Long compute() {
            // 当需要计算的数字小于6时，直接计算结果
            if (to - from < 6) {
                long total = 0;
                for (int i = from; i <= to; i++) {
                    total += numbers[i];
                }
                return total;
            // 否则，把任务一分为二，递归计算
            } else {
                int middle = (from + to) / 2;
                SumTask taskLeft = new SumTask(numbers, from, middle);
                SumTask taskRight = new SumTask(numbers, middle+1, to);
                taskLeft.fork();
                taskRight.fork();
                return taskLeft.join() + taskRight.join();
            }
        }
    }
 
    public ForkJoinCalculator() {
        // 也可以使用公用的 ForkJoinPool：
        // pool = ForkJoinPool.commonPool()
        pool = new ForkJoinPool();
    }
 
    @Override
    public long sumUp(long[] numbers) {
        return pool.invoke(new SumTask(numbers, 0, numbers.length-1));
    }
}
```



# Lecture 6 Database Intro



## Database

**Database**: a collection of interrelated data which helps for retrieval, insertion and deletion.

**DBMS**( database management system): software to manage/miniplate database. Oracle, Mysql, SQL Server

**SQL:** Structural Query Language, view or change the data in database (largely the name on DBMS, minor difference), upper and lower case do not matter

​		MySQL: - select character_length(data_string) from tableName

​		MS SQL Server:  - select len(data_string) from tableName

**File System**: a way of arranging files in a storage medium like the hard disk

![4](C:\Users\GrantW\Downloads\md\4.png)



## Process to build database

ideas ->high level design -> relational database schema -> relational DBMS



### ER Model

**Entity relationship model** -> structure of the database

-**Entity** : object or component of data

-**Attribute**: the property of an entity: 

​					**key attribute**: uniquely identify an entity from an entity set

​					**composite attribute**: a combination of other attributes

​					 **multivalued attribute**: an attribute holds multiple values

​					**derived attribute**: dynamic and derived from another attributes

**-Relationship:** degree of relationship: the number of entity participating the relationship

​					**Unary relationship**: Only one entity in the whole model

​					**Binary relationship**

​					**n-ary relationship**

​					**cardinality of relationship:** the number of times of entity participating the relationship

​																• One to one: male, female
​																• One to many: customer places orders.
​																• many to many: students can enroll courses

​					**participation constraint**

​																• total participation: each entity must participate in the relationship
​																• partial participation: each entity may or may not participate in the relationship																



### Constraints in relational Model:

**domain constraints:** each domain must contain atomic value (no composite or multivalued attributes)

**key constraints:** primary key: unique, not null

**Referential Integrity constrains**: 外键必须有效、存在



### mapping from ER model to relational model

• step1: transfer each entity/relationship to a table
• step2: optimize / merge the tables



## NF

normalization: eliminate redundant data and ensure the data is stored logically

### 1NF：消除重复

• each table cell should contain a single value
• each record needs to be unique

### 2NF：消除部份依赖

• be in 1NF
• single column primary key

### 3NF :消除传递依赖

• be in 2NF
• has no transitive functional dependencies
