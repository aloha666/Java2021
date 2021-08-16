# Lecture 1 Basic Java I

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

It is a **logical part** of the heap area and is created on virtual machine startup. Needs not to be contiguous连续. 

**字符串常量池则存在于方法区**

![798b04f9ga86a4ef61c5b&690](https://segmentfault.com/img/bVPR6M?w=553&h=322)

在Java虚拟机中 方法区是可提供各个线程共享的运行时内存区域，它存储了每一个类的结构信息，例如运行时常量池,字段和方法数据,构造函数和普通函数的字节码内容，一句话总结就是存储**元数据 (metadata)**地方。

### stack （私有）

A stack is created at the same time when a **thread** is created and is used to store data and partial results which will be needed while returning value for method and performing dynamic linking.Needs not to be contiguous 连续. 

**Local variabels, operating stacks, dynamic linking组成。**

### PC Counter （私有）

由于JVM同时可以处理多个线程所以就涉及到一些线程调度，当cpu暂停运行线程A把时间片让给线程B的时候我们需要保存线程A被暂停执行前的一些现场状态，需要记录当前执行到那一行字节码了，所以具备保存现场的功能。

每条线程都有自己的pc寄存器，在任意时刻虚拟机只会执行一个方法，如果执行的是方法不是native方法 pc寄存器则保存指向当前执行字节码的指令地址，如果执行的是native方法 pc寄存器会保存undefined。

Each **JVM thread** which carries out the task of a specific method has a **program counter register** associated with it. The non native method has a PC which stores the address of the available JVM instruction whereas in a native method, the value of program counter is undefined. PC register is capable of storing the return address or a native pointer on some specific platform.

A PC (Program Counter) Register contains the address of the instruction currently being executed in its associated thread. The PC Register is very small data area and has a fixed size. Java applications do not have any impact on its content and size.

### native method stack （私有，非Java method）

 This memory is allocated分配 for **each thread** when its created. And it can be of fixed or dynamic nature.

 A Native Method Stack stores similar data elements as a JVM Stack and it is used to help executing native (non-Java) methods. Java底层里调用别的语言代码的话就需要用到别的方法栈了,比如Java虚拟机的实现会用到传统的栈(C stack)来调用native方法，

问题： PC Counter 和 native mthod stack 作用？

## ClassLoader

The Java ClassLoader is a part of the Java Runtime Environment that dynamically loads Java classes into the Java Virtual Machine. The Java run time system does not need to know about files and file systems because of classloaders.

Java classes aren’t loaded into memory all at once, but when required by an application. At this point, the Java ClassLoader is called by the JRE and these ClassLoaders load classes into memory dynamically.

## The Class Class & Reflection API

java.lang.Class is one of the most important class in java. It is used to describe the meta information inside a class. When a class is loaded from ClassLoader, one(and only one per classloader) Class object will be created.

Every class or object can call getClass() method or .class field to get the instance of the Class class.

```java
Class c1 = String.class;
Class c2 = "hello".getClass();
Class c3 = "hi".getClass();

System.out.println(c1 == c2); // true
System.out.println(c2 == c3); // true, becuase they all get the same Class object from the classloader.
```

The object of Class class can perform lots of useful/powerful functionalities related to the class and objects of it.

```java
// a demo class
class Apple{
    private String color;

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }
}


public class ChangeDataType {

    public static void main(String[] args) throws Exception {
        Class appleClass = Class.forName("net.antra.reflection.Apple"); // Load the class without create object from Apple class
        Apple a = (Apple) appleClass.newInstance(); // create Apple class using Class.
        Constructor[] constructors = appleClass.getConstructors(); // get constructors.
        Method[] methods = appleClass.getDeclaredMethods(); // get methods
        Annotation[] annotations = appleClass.getDeclaredAnnotations(); // get annotations
        Field[] fields =  appleClass.getDeclaredFields(); // get fields
// During the runtime, all the members inside a class is visible to ClassLoader.
// By using Reflection API.
// All the methods and field can be called. Even for those who are private.
        constructors[0].newInstance();
        methods[0].invoke(a);
        fields[0].set(a,"newValue");
    }
}
```

As the code above, using reflection api, developers don't need to know the class utill at the runtime. Reflection gives us information about the class to which an object belongs and also the methods of that class which can be executed by using the object. Through reflection we can invoke methods at runtime irrespective of the access specifier used with them.

Combine with annotations, using reflection api can achieve lots of framework jobs. 

Below is a small "framework" to print out company value in the annotation Antra.

```java
// Framework code: Annotation
@Target({ElementType.METHOD,ElementType.TYPE,ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Antra {
    String companyValue() default "Java is the best";
}

// Scanning class
public class ScanDemo {

    public static void scanThisClass(String className) {
        try {
            Class clazz = Class.forName(className);
                        
            //Annotation on top of class
            Annotation[] ann = clazz.getDeclaredAnnotations();
            for (Annotation a : ann) {
                if(a instanceof Antra) {
                    System.out.println(((Antra) a).companyValue());
                }
            }
            //Annotations on Fields
            Field[] fields = clazz.getDeclaredFields();
            for (Field f : fields) {
                Annotation[] fAnn = f.getDeclaredAnnotations();
                for (Annotation a : fAnn) {
                    if(a instanceof Antra) {
                        System.out.println(((Antra) a).companyValue());
                    }
                }
            }
            //Annotation on Methods
            Method[] methods = clazz.getDeclaredMethods();
            for (Method m : methods) {
                Annotation[] mAnn = m.getDeclaredAnnotations();           
                for (Annotation a : mAnn) {
                    if(a instanceof Antra) {
                        System.out.println(((Antra) a).companyValue());
                    }
                }
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

}

//////////// with above code, we are able to read the annotation Antra from any class.

//////////// below is usage code.
// Demo Apple 
@Antra(companyValue = "hqweqwei")
public class Apple {

    @Antra(companyValue = ".Net is OK")
    private String color;

    @Antra
    public String getColor(){
        return color;
    }
}

// Demo Test. Run the main method.
public class TestScan {

    public static void main(String[] args) {
        ScanDemo.scanThisClass("net.antra.design.scan.Apple");
    }
}
```



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
  int i = 100; //local variable 
  doSomething(i);
  System.out.println(i); // 100
}


public void doSomething(int i) {. // the i here is a local variable. value is copied.
  i = 200;  // because value is copied, changing a copied value won't change the i in 
            //method1();
}
----------------------------------------------------
public void method1(){
  Apple a1 = new Apple("RED"); // object/class instance
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
  a = new Apple("RED"); // here we create a new Apple. and a is its reference now. become to a local variable.
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
        public boolean equals(Object o){  //override equals method 此处必须用object 如果直接用watermelon会导致override失效
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

**ConcurrentHashMap**

multi-thread safe HashMap, use in high concurrecy situation.将ConcurrentHashMap容器的数据分段存储，每一段数据分配一个Segment（锁），当线程占用其中一个Segment时，其他线程可正常访问其他段数据。

get操作：所有的value都定义成了volatile类型，volatile可以保证线程之间的可见性，

put操作：

1. 获取锁，保证put操作的线程安全；
2. 定位到HashEntry数组中具体的HashEntry；
3. 遍历HashEntry链表，假若待插入key已存在：

- 需要更新key所对应value（!onlyIfAbsent），更新oldValue -> newValue，跳转到步骤5；
- 否则，直接跳转到步骤5；

4. 遍历完HashEntry链表，key不存在，插入HashEntry节点，oldValue = null，跳转到步骤5；

5. 释放锁，返回oldValue

   

### Linked HashMap

Maintain the insertion order of the key-value pairs. This implementation differs from HashMap in that it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is normally the order in which keys were inserted into the map (insertion-order). Note that insertion order is not affected if a key is re-inserted into the map.

我们构建一个空间占用敏感的资源池，希望可 以自动将最不常被访问的对象释放掉，这就可以利用 LinkedHashMap 提供的机制来实现。**LRU类似(但是修改查询不改变位置)**

```java
package LinkedHashMap;

import java.util.LinkedHashMap;
import java.util.Map;

/**
 * @author jiangwentao
 */
public class LinkedHashMapSample {
    public static void main(String[] args) {
        LinkedHashMap<String, String> accessOrderedMap = new LinkedHashMap<String, String>(16, 0.75F, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
        // 实现自定义删除策略，否则行为就和普遍 Map 没有区别
                return size() > 3;
            }
        };
        accessOrderedMap.put("Project1", "Valhalla");
        accessOrderedMap.put("Project2", "Panama");
        accessOrderedMap.put("Project3", "Loom");
        accessOrderedMap.forEach((k, v) -> System.out.println(k + ":" + v));
        // 模拟访问
        accessOrderedMap.get("Project2");
        accessOrderedMap.get("Project2");
        accessOrderedMap.get("Project3");
        System.out.println("Iterate over should be not affected:");
        accessOrderedMap.forEach((k, v) -> System.out.println(k + ":" + v));
        // 触发删除
        accessOrderedMap.put("Project4", "Mission Control");
        System.out.println("Oldest entry should be removed:");
        accessOrderedMap.forEach((k, v) -> {
            // 遍历顺序不变
            System.out.println(k + ":" + v);
        });
    }
}

/*
Output:
Project1:Valhalla
Project2:Panama
Project3:Loom
--------------------------
Iterate over should be not affected:
Project1:Valhalla
Project2:Panama
Project3:Loom
---------------------------
Oldest entry should be removed:
Project2:Panama
Project3:Loom
Project4:Mission Control
*/
```



##### TreeMap

The map is sorted according to the natural ordering of its keys, or by a Comparator provided at map creation time, depending on which constructor is used.

# Lecture 2 Basic Java II

## JVM runtime map overview + example 

![17](/Users/spikycrown/Desktop/Java2021/images/17.png)

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

An **interface can have methods and variables** just like the class but the methods declared in interface are by default abstract (only method signature, no body, see: Java abstract method). Also, the variables declared in an interface are public, static & final by default.

### 	primitive data type

Auto-boxing/Unboxing	

*Autoboxing* is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes. For example, converting an `int` to an `Integer`, a `double` to a `Double`, and so on. If the conversion goes the other way, this is called *unboxing*.

The Java compiler applies unboxing when an object of a wrapper class is: **Passed as a parameter to a method that expects a value of the corresponding primitive type.**

Wrapper classes are those whose objects wraps a primitive data type within them. In the ***java.lang\*** package java provides a separate class for each of the primitive data type namely Byte, Character, Double, Integer, Float, Long, Short.

Converting primitive datatype to object is called **boxing**.

Converting an object of a wrapper type (`Integer`) to its corresponding primitive (`int`) value is called **unboxing**. The Java compiler applies unboxing when an object of a wrapper class is:

- Passed as a parameter to a method that expects a value of the corresponding primitive type.
- Assigned to a variable of the corresponding primitive type.

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

The Protected access specifier is visible within the s**ame package and also visible in the subclass(even not in same package)**, the Default is a package level access specifier and it can be visible in the same package.

### 	inheritance 

​		implements v.s. extends

​	一个类实现接口和继承抽象类对于抽象方法的实现原则是相同的：
（1）如果这个类是个普通类，那么必须实现这个接口/抽象类的所有抽象方法；
（2）如果这个类是个抽象类，那么不必实现这个接口/抽象类的抽象方法，因为抽象类中可以定义抽象方法。



### 	control flow:

​		break v.s. continues 

### 	instantiation: 

​		this v.s. super

### 	immutable class

final, static 

The main difference between a static and final keyword is that static is keyword is used to define the class member that can be used independently of any object of that class. Final keyword is used to declare, a constant variable, a method which can not be overridden and a class that can not be inherited.

```
静态变量并不是说其就不能改变值，不能改变值的量叫常量。 其拥有的值是可变的 ，而且它会保持最新的值。说其静态，是因为它不会随着函数的调用和退出而发生变化。即上次调用函数的时候，如果我们给静态变量赋予某个值的话，下次函数调用时，这个值保持不变。

父类的普通方法可以被继承和重写，不多作解释，如果子类继承父类，而且子类没有重写父类的方法，但是子类会有从父类继承过来的方法。
静态的方法可以被继承，但是不能重写。
```



### OOP: encapsulation

Encapsulation is used to bind together both data and methods which manipulate those data, to make sure taht certain data can only be used in a proper way and keep them safe from outside. e.g: packages

### 	  abstraction

**抽象方法，是指没有方法体的方法，同时抽象方法还必须使用关键字abstract做修饰。**

**抽象方法必须为public或者protected**（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。

```java
//没有方法体，有abstract关键字做修饰, 需要确定return type.
public abstract void xxx();

//一个类不能实现两个具有名称相同但返回类型不同的方法的接口。它将给出编译时错误。
```



### 	  polymorphism: 

  		1. Inheritance 
  		2. Override and Overload. 
  		3. Upper casting： Upcasting is the typecasting of **a child object to a parent object**. Upcasting gives us the flexibility to access the parent class members but it is not possible to access all the child class members using this feature. 子类可以用到任何需要父类的地方，反之则不行。


```java
1。父类引用指向子类对象，而子类引用不能指向父类对象。

2。把子类对象直接赋给父类引用叫upcasting向上转型，向上转型不用强制转换。

3。把指向子类对象的父类引用赋给子类引用叫向下转型(downcasting)，要强制转换。
  
  Father f1 = new Son(); // 这就叫 upcasting （向上转型)

// 现在f1引用指向一个Son对象

	Son s1 = (Son)f1; // 这就叫 downcasting (向下转型)
```



## Exception Handling

### 	Exception v.s. Error

![Top 20 Java Exception Handling Best Practices - HowToDoInJava](https://howtodoinjava.com/wp-content/uploads/2013/04/exceptionhierarchy3-8391226.png)

### Error

An Error is a subclass of Throwable that indicates serious problems that a reasonable application should not try to catch. Most such errors are abnormal conditions.

An `Error` normally indicates a problem, which *the application can not recover from*. Therefore, they should not be caught.

- OutOfMemoryError

- NoClassDefFoundError :如果 JVM 或者 ClassLoader 实例尝试加载（可以通过正常的方法调用，也可能是使用 new 来创建新的对象）类的时候却找不到类的定义。要查找的类在编译的时候是存在的，运行的时候却找不到了。这个时候就会导致 NoClassDefFoundError。原因：

  1. 打包过程漏掉了部分类。
  2. jar包出现损坏或者篡改。

  ```java
  //对比：
  /*ClassNotFoundException 产生的原因是：产生在runtime
  
  Java 支持使用反射方式在运行时动态加载类，例如使用 Class.forName 方法来动态地加载类时，可以将类名作为参数传递给上述方法从而将指定类加载到 JVM 内存中，如果这个类在类路径中没有被找到，那么此时就会在运行时抛出ClassNotFoundException 异常。
  原因：
  
  常见问题在于类名书写错误。
  当一个类已经被某个类加载器加载到内存中了，此时另一个类加载器又尝试着动态地从同一个包中加载这个类。通过控制动态类加载过程，可以避免上述情况发生。
  ```

  | 异常类型 | ClassNotFoundException                                       | NoClassDefFoundError                                         |
  | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | 继承模型 | 从java.lang.Exception继承，是一个Exception类型               | 从java.lang.Error继承，是一个Error类型                       |
  | 触发原因 | 当动态加载Class的时候找不到类会抛出该异常                    | 程序在编译时可以找到所依赖的类，但是在运行时找不到指定的类文件，运行过程中Class找不到导致抛出该错误 |
  | 触发主体 | 一般在执行Class.forName()、ClassLoader.loadClass()或ClassLoader.findSystemClass()的时候抛出 | JVM或者ClassLoader实例尝试加载类的时候，找不到类的定义而发生，通常在**import**和**new**一个类的时候触发 |
  | 处理方式 | 程序可以从Exception中恢复，ClassNotFoundException可由程序捕获和处理 | 程序无法从错误中恢复，Error是系统错误，用户无法处理          |
  | 可能原因 | 要加载的类不存在；类名书写错误                               | jar包缺失；调用初始化失败的类                                |

### 	Exception: checked/unchecked Exception

Checked Exception: Has to be try-catched or decleared using throws keyword on method. compile编译时检查

UnChecked Exception: Don't need to be try-catched or decleared.可以catch 但没必要， 运行时抛出

Throwable is the parent class of all exceptions and errors.

### 	Exception Handling: 

	 1. try-catch-finally (try with resource) 
	 2. throws Customized Exception

### Unchecked Exception vs Error

```java
/*From the Error Javadoc:

An Error is a subclass of Throwable that indicates serious problems that a reasonable application should not try to catch. Most such errors are abnormal conditions. The ThreadDeath error, though a "normal" condition, is also a subclass of Error because most applications should not try to catch it.

Versus the Exception Javadoc

The class Exception and its subclasses are a form of Throwable that indicates conditions that a reasonable application might want to catch.

So, even though an unchecked exception is not required to be caught, you may want to. An error, you don't want to catch.
```



## Garbage Collector

The ***heap*** is where your object data is stored. This area is then managed by the garbage collector selected at startup. Most tuning options relate to sizing the heap and choosing the most appropriate garbage collector for your situation.

<img src="https://github.com/aloha666/Java2021/blob/main/images/6.png?raw=true" alt="6.png" style="zoom:67%;" />



### Garbage Collection

Automatic garbage collection is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects. An in use object, or a referenced object, means that some part of your program still maintains a pointer to that object. An unused object, or unreferenced object, is no longer referenced by any part of your program. So the memory used by an unreferenced object can be reclaimed.

Java GC uses Mark and Sweep strategy to clean the heap.

- Mark – it is where the garbage collector identifies which pieces of memory are in use and which are not
- Sweep – this step removes objects identified during the “mark” phase

### JVM Generations

<img src="https://github.com/aloha666/Java2021/blob/main/images/7.png?raw=true" alt="7.png" style="zoom:50%;" />

As stated earlier, having to mark and compact all the objects in a JVM is inefficient. As more and more objects are allocated, the list of objects grows and grows leading to longer and longer garbage collection time. However, empirical analysis of applications has shown that most objects are short lived

The heap is broken up into smaller parts or generations. The heap parts are: Young Generation, Old or Tenured Generation, and Permanent Generation( before java 8).

The **Young Generation** is where all new objects are allocated and aged. When the young generation fills up, this causes a ***minor garbage collection\***. Minor collections can be optimized assuming a high object mortality rate. A young generation full of dead objects is collected very quickly. Some surviving objects are aged and eventually move to the old generation.

The **Old Generation** is used to store long surviving objects. Typically, a threshold is set for young generation object and when that age is met, the object gets moved to the old generation. Eventually the old generation needs to be collected. This event is called a ***major garbage collection\***.

 The **Permanent generation** contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. In addition, Java SE library classes and methods may be stored here. From Java 8, Permanent generation is replaced with MetaSpace.

**Metaspace** is native memory grows automatically by default. 

# Lecture 3 Core Java I

## stackoverflow vs out of memory

- `OutOfMemoryError` is related to Heap. is thrown when you are creating objects

- `StackOverflowError` is related to stack. is thrown when you are calling functions.

  

  When you start `JVM` you define how much RAM it can use for processing. `JVM` divides this into certain memory locations for its processing purpose, two of those are **`Stack` & `Heap`**

  If you have large objects (or) referenced objects in memory, then you will see `OutofMemoryError`. If you have strong references to objects, then GC can't clean the memory space allocated for that object. When JVM tries to allocate memory for new object and not enough space available it throws `OutofMemoryError` because it can't allocate the required amount of memory.

  **How to avoid**: Make sure un-necessary objects are available for GC

  All your local variables and methods calls related data will be on the stack. For every method call, one stack frame will be created and local as well as method call related data will be placed inside the stack frame. Once method execution is completed, the stack frame will be removed. ONE WAY to reproduce this is, have an infinite loop for method call, you will see `stackoverflow` error, because stack frame will be populated with method data for every call but it won't be freed (removed).

  **How to avoid**: Make sure method calls are ending (not in an infinite loop)

  

## Generics

Reference to Jakov. 泛型其实就是在定义类、接口、方法的时候不局限地指定某一种特定类型，而让类、接口、方法的调用者来决定具体使用哪一种类型的参数。

## Enum

A *Java Enum* is a special Java type used to define **collections of constants**. More precisely, a Java enum type is a special kind of Java class. An enum can contain constants, methods etc. Java enums were added in Java 5.(Before is Object, after is Enum)

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

Annotations provide data about a program that is **not** part of the program itself. What is an Annotation for ?

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

1、@Deprecated – 所标注内容不再被建议使用；
2、@Override – 只能标注方法，表示该方法覆盖父类中的方法；
3、@Documented --所标注内容可以出现在javadoc中；
**4、@Inherited – 只能被用来标注“Annotation类型”，它所标注的Annotation具有继承性；**
**5、@Retention – 只能被用来标注“Annotation类型”，而且它被用来指定Annotation的RetentionPolicy属性；**
**6、@Target – 只能被用来标注“Annotation类型”，而且它被用来指定Annotation的ElementType属性；**
7、@SuppressWarnings – 所标注内容产生的警告，编译器会对这些警告保持静默；
**8、@interface – 用于定义一个注解；**

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



##### Customized Java Annotations

```java
// Target is to declear where the annotation can be put on.
// Retention is to declear until when this annotation can be read. Indicates how long annotations with the annotated type are to be retained.

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface AntraAnnotation {
    String name() default ""; // declear attribute
}
```

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

# Lecture 4 Core Java II

## IO Stream Continue

![1.png](https://github.com/aloha666/Java2021/blob/main/images/1.png?raw=true)

### InputStreamReader(Transfer Stream) vs FileReader

InputStreamReader读bytes然后转化成character，同时可以指定编码的方式(GBK, UTF-8, etc)，filereader直接读character。

`InputStreamReader` can handle all input streams, not just files. Other examples are network connections, classpath resources and ZIP files.

FileReader reads character from a file in the file system. InputStreamReader reads characters from any kind of input stream. The stream cound be a FileInputStream, but could also be a stream obtained from a socket, an HTTP connection, a database blob, whatever.

"**I usually prefer using an InputStreamReader wrapping a FileInputStream to read from a file because it allows specifying a specific character encoding.**"

**为何要用char[]来读input String？**用来减少IO inter actinons between JVM and hard disk. 比一个一个读要快

**为何不直接创建一个string size的char[]?** trade performance with memory size, 越大越需要memeory空间

**flush():** Flushes the output stream and forces any buffered output bytes to be written out. The general contract of flush is that calling it is an indication that, if any bytes previously written have been buffered by the implementation of the output stream, such bytes should immediately be written to their intended destination. The buffering is mainly done to improve the I/O performance.

**close():** always close a buffer after use.

```java
	@Test
    public void testInputOutputStream() throws IOException {

        FileInputStream fip = null;
        FileOutputStream fop = null;

        File src = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/1.png"); //可以读Png
        File des = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/2.png");

        fip = new FileInputStream(src);
        fop = new FileOutputStream(des);

        byte[] buffer = new byte[5]; //读byte
        int len;

        while ((len = fip.read(buffer)) != -1) {
            fop.write(buffer, 0, len);

        }

        fip.close();
        fop.close();

    }		
@Test
    public void testFileReaderFileWriter1() throws IOException {

        FileReader fr = null;
        FileWriter fw = null;

        File src = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/file1.txt");//只能读txt
        File des = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/file2.txt");

        fr = new FileReader(src);
        fw = new FileWriter(des);

        char[] buffer = new char[5]; //读char
        int len;

        while ((len = fr.read(buffer)) != -1) {
            fw.write(buffer, 0, len);

        }

        fr.close();
        fw.close();

    }


    @Test
    public void testInputOutputStreamReader() throws IOException {

        FileInputStream fip = null;
        FileOutputStream fop = null;

        File src = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/file1.txt"); //只能读txt
        File des = new File("/Users/spikycrown/Desktop/lecp/lec3/src/resources/file2.txt");

        fip = new FileInputStream(src);
        fop = new FileOutputStream(des);

        InputStreamReader isr = new InputStreamReader(fip,"UTF-8");
        OutputStreamWriter osw = new OutputStreamWriter(fop,"GBK");

        char[] buffer = new char[5]; //读char
        int len;

        while ((len = isr.read(buffer)) != -1) {
            osw.write(buffer, 0, len);

        }

        isr.close();
        osw.close();

    }
```



### Object Stream & Serializable Interface

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

UID 也是 static 是否在序列化中保存？ **YES** https://coderanch.com/t/593227/java/serialVersionUID-field-type-static-final

```java
/* Yes it is the exception because serialVersionUID is used by the serialisation system to determine if a class has been changed since the object was serialized ie if it is safe to de-serialize the object to the current class definition. If the serialVersionUID in a serialized object is the same as the in class then it is assumed that no modifications that will effect the de-serialisation have been made to the class. So for instance, if a class has been modified to add a new method then there is probably no need to change the class' serialVersionUID however if you have added/removed/changed the type of an instance variable then you must change the serialVersionUID or provide special handling code for de-serialising objects serialized before the change.
```



transient关键字的作用是：阻止实例中那些用此关键字声明的变量持久化；当对象被反序列化时（从源文件读取字节序列进行重构），这样的实例变量值不会被持久化和恢复。当某些变量不想被序列化，同是又不适合使用static关键字声明，那么此时就需要用transient关键字来声明该变量。

static 和 transient 区别：transient修饰的变量跟普通的成员变量在使用上没有任何区别，但是static就是static变量。

## Java 8 features - Lambda Expression

one line statement, a simpified anonymous method. lambda expression经常用来做argument（work with functional interface).

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

Functional interface is a kind of interface that has **only 1 abstract method**.

Functional interface can have multiple *default methods and static methods.*

https://www.cnblogs.com/javazhiyin/p/12009464.html 从入门到入土：Lambda完整学习指南，包教包会！

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

```java
//Consumer   
@Test
    public void test1(){
        hello("张三", (m) -> System.out.println("你好：" + m));
    }
    public void hello(String st, Consumer<String> con){
        con.accept(st);
    }


 //Supplier<T> 供给型接口 :
    @Test
    public void test2(){
        List list = Arrays.asList(121, 1231, 455, 56, 67,78);
        List<Integer> numList = getNumList(1, () -> (int)(Math.random() * 100));
        for (Integer num : numList) {
            System.out.println(num);
        }
    }
    //需求：产生指定个数的整数，并放入集合中
    public List<Integer> getNumList(int num, Supplier<Integer> sup){
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < num; i++) {
            Integer n = sup.get();
            list.add(n);
        }
        return list;
    }

    //Function<T, R> 函数型接口：
    @Test
    public void test3(){
        String newStr = strHandler("ttt 这是一个函数型接口 ", (str) -> str.trim());
        System.out.println(newStr);
        String subStr = strHandler("这是一个函数型接口", (str) -> str.substring(4, 7));
        System.out.println(subStr);
    }
    //需求：用于处理字符串
    public String strHandler(String str, Function<String, String> fun){
        return fun.apply(str);
    }


  // Predicate<T> 断言型接口：
    @Test
    public void test4(){
        List<String> list = Arrays.asList("Hello", "Java8", "Lambda", "www", "ok");
        List<String> strList = filterStr(list, (s) -> s.length() > 3);
        for (String str : strList) {
            System.out.println(str);
        }
    }
    //需求：将满足条件的字符串，放入集合中
    public List<String> filterStr(List<String> list, Predicate<String> pre){
        List<String> strList = new ArrayList<>();
        for (String str : list) {
            if(pre.test(str)){
                strList.add(str);
            }
        }
        return strList;
    }
```



##### Method Reference



## Java 8 features - Stream API

### What is a stream?

![3.png](https://github.com/aloha666/Java2021/blob/main/3.png?raw=true)

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



# Lecture 5 Core Java III (Optional Function & Multi Threading)



## Java 8 features - Optional function

optional function is to **prevent NullpointerExcenption**.

methods: 

isPresent(): check optional instance is empty or not

of(object): pop out nullpointerexception if object is null

ofNullable(object): accept null value if object is null

1. 创建：
   Optional.empty()： 创建一个空的 Optional 实例

   Optional**.of(T t)**：创建一个 Optional 实例，当 t为null时抛出异常    

   Optional**.ofNullable(T t)**：创建一个 Optional 实例，但当 t为null时不会抛出异常，而是返回一个空的实例

2. 获取：
    get()：获取optional实例中的对象，当optional 容器为空时报错

3. 判断：
    isPresent()：判断optional是否为空，如果空则返回false，否则返回true

    ifPresent(Consumer c)：如果optional不为空，则将optional中的对象传给Comsumer函数

    orElse(T other)：如果optional不为空，则返回optional中的对象；如果为null，则返回 other 这个默认值

    orElseGet(Supplier<T> other)：如果optional不为空，则返回optional中的对象；如果为null，则使用Supplier函数生成默认值other

    orElseThrow(Supplier<X> exception)：如果optional不为空，则返回optional中的对象；如果为null，则抛出Supplier函数生成的异常

4. 过滤：
    filter(Predicate<T> p)：如果optional不为空，则执行断言函数p，如果p的结果为true，则返回原本的optional，否则返回空的optional  

5. 映射：
    map(Function<T, U> mapper)：如果optional不为空，则将optional中的对象 t 映射成另外一个对象 u，并将 u 存放到一个新的optional容器中。

    flatMap(Function< T,Optional<U>> mapper)：跟上面一样，在optional不为空的情况下，将对象t映射成另外一个optional

    区别：map会自动将u放到optional中，而flatMap则需要手动给u创建一个optional

```java
/*学校想从一批学生中，选出年龄大于等于18，参加过考试并且成绩大于80的人去参加比赛。*/

public class Student {
    private String name;
    private int age;
    private Integer score;
    
    //省略 construct get set
}
 
public List<Student> initData(){
    Student s1 = new Student("张三", 19, 80);
    Student s2 = new Student("李四", 19, 50);
    Student s3 = new Student("王五", 23, null);
    Student s4 = new Student("赵六", 16, 90);
    Student s5 = new Student("钱七", 18, 99);
    Student s6 = new Student("孙八", 20, 40);
    Student s7 = new Student("吴九", 21, 88);
 
    return Arrays.asList(s1, s2, s3, s4, s5, s6, s7);
}
//java8 之前写法：

@Test // 在导入junit的jar包后，在方法名的上一行加上“@Test”就可以在没有main的情况下运行该方法。
public void beforeJava8() {
    List<Student> studentList = initData();
 
    for (Student student : studentList) {
        if (student != null) {
            if (student.getAge() >= 18) {
                Integer score = student.getScore();
                if (score != null && score > 80) {
                    System.out.println("入选：" + student.getName());
                }
            }
        }
    }
}
java8 写法：

@Test
public void useJava8() {
    List<Student> studentList = initData();
    for (Student student : studentList) {
        Optional<Student> studentOptional = Optional.of(student);
        Integer score = studentOptional.filter(s -> s.getAge() >= 18).map(Student::getScore).orElse(0);
 
        if (score > 80) {
            System.out.println("入选：" + student.getName());
        }
    }
}

```





## MultiThreading

### Process vs Thread

**process**: an execution of a program, OS will assgin memory for each process.

**thread**: an execution routine of process within a process which share the same memory area.

**why we need multithreading?**  To achieve multi tasking

One CPU: do threads concurrently, may be not parallelly, (jump from 1 to 2, then jump back to 1, etc)

multi CPU: do threads concreeemtly and parellelly

### Multithread vs concurrency

Concurrency is the ability of your program to deal (not doing) with many things at once and is achieved through multithreading. Do not confuse concurrency with parallelism which is about doing many things at once.

![img](http://tutorials.jenkov.com/images/java-concurrency/concurrency-vs-parallelism-1.png)

***Concurrency*** means that an application is making progress on more than one task - at the same time or at least seemingly at the same time (concurrently).

![img](http://tutorials.jenkov.com/images/java-concurrency/concurrency-vs-parallelism-2.png)

**Parallel** execution is when a computer has more than one CPU or CPU core, and makes progress on more than one task simultaneously.

### Thread Creation

**4ways:** 

**1.extend thread class**

**2.implement runnable interface**

**3.implement callable interface**

callable can have a return value

callable can thorw an exception

**4.threadpool (Executors, excutorService, fork-join pool, completableFuture)**



### Thread Methods:

![线程状态图](https://img-blog.csdnimg.cn/20181120173640764.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BhbmdlMTk5MQ==,size_16,color_FFFFFF,t_70)

**Thread.sleep():** Thread. sleep **causes the current thread to suspend execution for a specified period**.  This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system. 暂停当前线程，把cpu片段让出给其他线程，减缓当前线程的执行。**Thread**.**Sleep**(0)的**作用**，就是“触发操作系统立刻重新进行一次CPU竞争，重新计算优先级。

**this.wait():** to suspend a thread

**this.notify():** to wake a thread up

**this.notifyAll():** This method simply wakes all threads that are waiting on this object's monitor. The awakened threads will complete in the usual manner, like any other thread.

<img src="https://github.com/aloha666/Java2021/blob/main/images/16.png?raw=true" alt="16.png" style="zoom:80%;" />

### States of a thread: 

**New 初始状态:** a thread not yet start

**Runnable 可运行状态**: a thread is runnable/executing

**Blocked** **阻塞**: a thread waiting for a monitor **lock**, 线程阻塞于锁。 after calling **object.wait()** 

**Waitting**: a thread waiting for another thread to perform a particular action, 进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断). 

​				after calling **object.wait()** with no time out, **thread.join()** with no time out, **LockSupport.park** 

**Timed_waiting**: thread state for a waiting thread with a specified time. 该状态不同于WAITING，它可以在指定的时间后自行返回。

​				**Thread.Sleep**, **object.wait()** with  time out, **thread.join()** with time out, **LockSupport.park**, **LockSupport.parkUntil**

**Terminated**:表示该线程已经执行完毕。 thread has complete execution and terminated

**sleep vs wait**: wait will release lock, while sleep will not

A thread goes to wait state once it calls `wait()` on an Object. This is called **Waiting** State. Once a thread reaches waiting state, it will need to wait till some other thread calls `notify()` or `notifyAll()` on the object.

Once this thread is notified, it will not be runnable. It might be that other threads are also notified (using `notifyAll()`) or the first thread has not finished his work, so it is still blocked till it gets its chance. This is called **Blocked** State. A Blocked state will occur whenever a thread tries to acquire lock on object and some other thread is already holding the lock.

### **thread.join()** 用法

让thread按顺序进行 当前thread结束，才运行下一个thread。

```java
public class JoinTest {
    
    public static void main(String[] args) {
        
        ThreadBoy boy = new ThreadBoy();
        boy.start();
        
    }
    
    static class ThreadBoy extends Thread{
        @Override
        public void run() {
            
            System.out.println("男孩和女孩准备出去逛街");
            
            ThreadGirl girl = new ThreadGirl();
            girl.start();
            
            try {
                girl.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            System.out.println("男孩和女孩开始去逛街了");
        }
    }
    
    static class ThreadGirl extends Thread{
        @Override
        public void run() {
            int time = 5000;
            
            System.out.println("女孩开始化妆,男孩在等待。。。");
            
            try {
                Thread.sleep(time);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            System.out.println("女孩化妆完成！，耗时" + time);
            
        }
    }
    
}

/*男孩和女孩准备出去逛街
女孩开始化妆,男孩在等待。。。
女孩化妆完成！，耗时5000
男孩和女孩开始去逛街了*/


public class JoinTest {
    
    public static void main(String[] args) {
        
        ThreadBoy boy = new ThreadBoy();
        boy.start();
        
    }
    
    static class ThreadBoy extends Thread{
        @Override
        public void run() {
            
            System.out.println("男孩和女孩准备出去逛街");
            
            ThreadGirl girl = new ThreadGirl();
            girl.start();
            
            int time = 2000;
            try {
                girl.join(time);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            System.out.println("男孩等了" + time + ", 不想再等了，去逛街了");
        }
    }
    
    static class ThreadGirl extends Thread{
        @Override
        public void run() {
            int time = 5000;
            
            System.out.println("女孩开始化妆,男孩在等待。。。");
            
            try {
                Thread.sleep(time);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            System.out.println("女孩化妆完成！，耗时" + time);
            
        }
    }
    
}

/*
这里仅仅把join方法换成了join(time)方法，描述改了点，打印的结果是：
男孩和女孩准备出去逛街
女孩开始化妆,男孩在等待。。。
男孩等了2000, 不想再等了，去逛街了
女孩化妆完成！，耗时5000

```



### Synchronized vs Lock

**Synchorzied**: used on method, coupling and slow, the execuation will be in a random order (?). In other words, use synchorized with notifyAll( ) cannot contorl the order of thread execution.

```java
class A {
  // put on instance method.
  // instance method belong to object.
  // For the same object if two threads calling this method.
  // they have to run one by one.
  // so "this"-current object, is the lock.
  public synchronized void doSomething(){
      System.out.println("hi");
  }

}

A a = new A();

Thread1 call a.doSomething();
Thread2 call a.doSomething(); //<- they have to run one by one.

A a1 = new A();
Thread 1 call a.doSomething();
Thread 2 call a1.doSomething(); //<- they don't have to wait. because "this" of a and a1 are different.
```



The **java.lang.Object.notifyAll()** wakes up all threads that are waiting on this object's monitor. A thread waits on an object's monitor by calling one of the wait methods. notify表示唤醒一个线程 notifyAll也表示唤醒一个线程，但它会notify所有的线程，具体唤醒哪一个线程，由jvm来决定

```java

public class TestNotifyNotifyAll {
 
	private static Object obj = new Object();
	
	public static void main(String[] args) {
		
		//测试 RunnableImplA wait()        
		Thread t1 = new Thread(new RunnableImplA(obj));
		Thread t2 = new Thread(new RunnableImplA(obj));
		t1.start();
		t2.start();
		
		//RunnableImplB notify()
		Thread t3 = new Thread(new RunnableImplB(obj));
		t3.start();
		
		
//		//RunnableImplC notifyAll()
//		Thread t4 = new Thread(new RunnableImplC(obj));
//		t4.start();
	}
	
}
 
 
class RunnableImplA implements Runnable {
 
	private Object obj;
	
	public RunnableImplA(Object obj) {
		this.obj = obj;
	}
	
	public void run() {
		System.out.println("run on RunnableImplA");
		synchronized (obj) {
			System.out.println("obj to wait on RunnableImplA");
			try {
				obj.wait();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			System.out.println("obj continue to run on RunnableImplA");
		}
	}
}
 
class RunnableImplB implements Runnable {
 
	private Object obj;
	
	public RunnableImplB(Object obj) {
		this.obj = obj;
	}
	
	public void run() {
		System.out.println("run on RunnableImplB");
		System.out.println("睡眠3秒...");
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		synchronized (obj) {
			System.out.println("notify obj on RunnableImplB");
			obj.notify();
		}
	}
}
 
class RunnableImplC implements Runnable {
 
	private Object obj;
	
	public RunnableImplC(Object obj) {
		this.obj = obj;
	}
	
	public void run() {
		System.out.println("run on RunnableImplC");
		System.out.println("睡眠3秒...");
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		synchronized (obj) {
			System.out.println("notifyAll obj on RunnableImplC");
			obj.notifyAll();
		}
	}

```



**Lock:** a interface that lock() the resource for the thread to use, will unlock()  after use. wait will release lock, while sleep will not.

lock condition: control flow 

怎么解决死锁？

**Start():** NATIVE METHOD start0( ); writted in C language, will start/run a thread. thread.start();

normally we cannot have a abstract method in a non-abstract class. BUT CAN HAVE A NATIVE METHOD.

总结：

**synchronized**: object.wait(), object.notify()/notifyAll(), execuation with random order based on OS

**Lock + condition**: condition.await(), condition.signal()/signalAll(), execution with assigned order based on programmers requirement.

```java
//为什么JAVA,wait（）要放在while循环里？
//一个线程有可能会在未被通知、打断、或超时的情况下醒来，这就是所谓的“spurious wakeup”。尽管实际上这种情况很少发生，应用程序仍然必须对此有所防范，手段是检查正常的导致线程被唤醒的条件是否满足，如果不满足就继续等待。

//A while loop is the best choice for checking the condition predicate both before and after invoking wait(). When waiting upon a Condition, a "spurious wakeup" is permitted to occur, in general, as a concession to the underlying platform semantics.
```



### Lock vs Semaphore

semaphore 给指定资源发permits， semaphore(1) 作用和lock一样。

但是semaphore可以**解决deadlock**: **通过一个非owner的线程来实现死锁恢复**，但如果你使用的是Lock则做不到，可以把代码中的两个信号量换成两个锁对象试试。很明显，前面也验证过了，要使用Lock.unlock()来释放锁，首先你得拥有这个锁对象，因此非owner线程（事先没有拥有锁）是无法去释放别的线程的锁对象。

```java
public class LockTest {
    public static void main(String[] args) {
        Lock lock=new ReentrantLock();
　　　　//Lock.lock()  注释掉lock，就会报错，因为没有获得锁，就进行释放
        lock.unlock(); //Lock.unlock()之前，该线程必须事先持有这个锁,否则抛出异常
    }
```

```java
public class Semaphore {    
    public static void main(String[] args) {
        Semaphore semaphore=new Semaphore(1);//初始化1个许可
        System.out.println("可用的许可数："+semaphore.availablePermits());
        semaphore.release();
        System.out.println("可用的许可数："+semaphore.availablePermits());
    }
//可用的许可数目为：1
//可用的许可数目为：2
//结果说明了以下2个问题：
//1.并没有抛出异常，也就是线程在调用release()之前，并不要求先调用acquire() 。
//2.我们看到可用的许可数目增加了一个，但我们的初衷是保证只有一个许可来达到互斥排他锁的目的.
```

```java
//解决死锁
class WorkThread2 extends Thread{
    private Semaphore semaphore1,semaphore2;
    public WorkThread2(Semaphore semaphore1,Semaphore semaphore2){
        this.semaphore1=semaphore1;
        this.semaphore2=semaphore2;
    }
    public void releaseSemaphore2(){
        System.out.println(Thread.currentThread().getId()+" 释放Semaphore2");
        semaphore2.release();
    }
    public void run() {
        try {
            semaphore1.acquire(); //先获取Semaphore1
            System.out.println(Thread.currentThread().getId()+" 获得Semaphore1");
            TimeUnit.SECONDS.sleep(5); //等待5秒让WorkThread1先获得Semaphore2
            semaphore2.acquire();//获取Semaphore2
            System.out.println(Thread.currentThread().getId()+" 获得Semaphore2");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }   
    }
}
class WorkThread1 extends Thread{
        private Semaphore semaphore1,semaphore2;
        public WorkThread1(Semaphore semaphore1,Semaphore semaphore2){
            this.semaphore1=semaphore1;
            this.semaphore2=semaphore2;
        }
        public void run() {
            try {
                semaphore2.acquire();//先获取Semaphore2
                System.out.println(Thread.currentThread().getId()+" 获得Semaphore2");
                TimeUnit.SECONDS.sleep(5);//等待5秒，让WorkThread1先获得Semaphore1
                semaphore1.acquire();//获取Semaphore1
                System.out.println(Thread.currentThread().getId()+" 获得Semaphore1");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
}
public class SemphoreTest {
    public static void main(String[] args) throws InterruptedException {
        Semaphore semaphore1=new Semaphore(1);
        Semaphore semaphore2=new Semaphore(1);
        new WorkThread1(semaphore1, semaphore2).start();
        new WorkThread2(semaphore1, semaphore2).start();
        //此时已经陷入了死锁（也可以说是活锁）
　　　　　//WorkThread1持有semaphore1的许可，请求semaphore2的许可
        // WorkThread2持有semaphore2的许可，请求semaphore1的许可
//      TimeUnit.SECONDS.sleep(10);


//      //在主线程是否semaphore1,semaphore2,解决死锁
      System.Out.PrintLn("===释放信号==");
      semaphore1.release();
     semaphore2.release();
    }
 
}
/*输出结果：

获得Semaphore2
获得Semaphore1
当我们打开最后的release代码块：

获得Semaphore2
获得Semaphore1
===释放信号==
获得Semaphore1
获得Semaphore2
*/




```java
//callable can return. runnable do not return value
class MyCallableClass implements Callable<String> { // means it will return String
  public String call() { // can throw exception here
     // do a lot of work and get a result = "Hi"
     //...
     return "Hi";
  }
}

ExecutorService es = Executors.newFixedThreadPool(30);
Future<String> future = es.submit(new MyCallableClass()); //put thread in pool
String result = future.get();
es.shutdown(); //close pool;

```



### Thread Pool

Threads are objects. It takes memory and consumes computation power. To reduce the cost of creating and destroying new threads, and also to confine the memory usage. On the server side applications, we use thread pool to pre-populate certiain number of threads to be reused by different tasks.

A thread pool is used to aviod cosuming resources: repetively using existing thread which could reduce the cost of new and GC threads increase response time: Once the task is ready to be executed, we do not need to wait for the thread creation, which improve multi-thread management.

线程池顾名思义就是事先创建若干个可执行的线程放入一个池中，需要的时候就从池中获取线程不用自行创建，使用完毕不用销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。

**Executor** The \*Executor\* interface has a single \*execute\* method to submit \*Runnable\* instances for execution.

```java
Executor executor = Executors.newSingleThreadExecutor();
executor.execute(() -> System.out.println("Hello World"));
```

**ExecutorService\*** The *ExecutorService* interface contains a large number of methods to **control the progress of the tasks and manage the termination of the service.** 

```java
ExecutorService executorService = Executors.newFixedThreadPool(10);
Future<String> future = executorService.submit(() -> "Hello World");
// some operations
String result = future.get();
```

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

![4.png](https://github.com/aloha666/Java2021/blob/main/images/4.png?raw=true)



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

**-Relationship:** degree of relationship: the number of attributes participating the relationship

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



# Lecture 7  Non-relations Database

## Non-relational Databse

does not use the tabular schema of rows and columns, Instead, it uses a storage model that is optimized for specific requirements of the type of data being stored.

### 1. Document data stores  

Two filelds:

​		○**key**
​		○ **document**: form of JSON documents -> XML, YAML, JSON, **BSON**

代表：**MongoDB** , CouchDB

Documents are addressed in the database via a unique ***key*** that represents that document. This key is a simple [identifier](https://en.wikipedia.org/wiki/Identifier) (or ID), typically a [string](https://en.wikipedia.org/wiki/String_(computer_science)), a [URI](https://en.wikipedia.org/wiki/URI), or a [path](https://en.wikipedia.org/wiki/Path_(computing)). The key can be used to retrieve the document from the database. Typically the database retains an [index](https://en.wikipedia.org/wiki/Database_index) on the key to speed up document retrieval, and in some cases the key is required to create or insert the document into the database.

### Relationship to key-value stores

A document-oriented database is a specialized [key-value store](https://en.wikipedia.org/wiki/Key-value_database), which itself is another NoSQL database category. In a simple key-value store, the document content is opaque. A document-oriented database provides APIs or a query/update language that exposes the ability to query or update based on the internal structure in the *document*.



### 2. columnar data stores or column family 

Divide data into groups/colmns, one column family is one topic/ID.

**de-normalization**: group all information in one column family

 代表：Cassandra, Hbase

![img](http://www.timestored.com/time-series-data/images/column-vs-row-oriented-database.png)



### 3. Key/value data stores

essentially a large hash table, hash value is opaque不透明 to data store

代表：**Redis**, riak



### 4.Graph data stores

○node: entity

○edge: relationship

代表：Neo4j, Hyper GraphDb



## CAP Theory

Consistency: 一致性 all clients will always have the same view/version of data. when you write a piece of data in a system/distributed system, the same data you should get when you read it from any node of the system.

Availability: 可用性 each client can always read and write the data. The system should always be available for read/write operation.

Partition tolerance: 分区容错性 the system works well despite the physical network partition (always achieve in non-relation database since they are distributed)

CAP theorem: satisfying all three at the same time is impossible, **Most systems are not only available or only consistent, they always offer a bit of both**

CP 代表: BigTable, **MongoDB**, Hbase, **Redis**

 AP 代表: DynamoDB, Cassandra, Cassandra, CouchDb, Riak

```java
/*
分区耐受性
保证数据可持久存储，在各种情况下都不会出现数据丢失的问题。为了实现数据的持久性，不但需要在写入的时候保证数据能够持久存储，还需要能够将数据备份一个或多个副本，存放在不同的物理设备上，防止某个存储设备发生故障时，数据不会丢失。
数据一致性
在数据有多份副本的情况下，如果网络、服务器、软件出现了故障，会导致部分副本写入失败。这就造成了多个副本之间的数据不一致，数据内容冲突。
数据可用性
多个副本分别存储于不同的物理设备的情况下，如果某个设备损坏，就需要从另一个数据存储设备上访问数据。如果这个过程不能很快完成，或者在完成的过程中需要停止终端用户访问数据，那么在切换存储设备的这段时间内，数据就是不可访问的。

/*
**why mongoDB and redis did not achieve availability? High consistency > availability**

MongoDB is highly consistent when reads and write go to the same node(the default case). Further, you can choose in MongoDB to read from other secondary nodes instead of reading from only leader/primary.

MongoDB is mostly always available, but, the only time when the leader is down, MongoDB can't accept writes, until it figures out the new leader. Hence, not `highly available`


what is a leader in MongoDB?

A MongoDB cluster needs to have a primary node and a set of secondary nodes in order to be considered a replica set.
只有leader node 接受写操作

*/
```



## Sharding and replica 各有什么好处？

### Sharding of data 分片

将一整个database部署到不同的physical machine 上，有利于 concurrency请求。

• distributes a single logical database system across a cluster of machines
• use range-based partition to distribute documents based on a specific shard key

### Replica 复制

制作小弟，方便查询

• copy of database
• failover (zero downtime) : when the leader node is down, the follower will replace the leader so that the system is 0 downtime. 大哥倒了 小弟补位



## MongoDB

MongoDB是一个基于文档型数据库，MongoDB中文档数据，使用BSON（一种和JSON类似的 (Binary JSON)）东西作为数据格式。

![img](https://static001.infoq.cn/resource/image/0c/e8/0c7485717cce321c7ab85de654f524e8.png)

### MongoDB

```json
{
  "_id": 1,
  "name" : { "first" : "John", "last" : "Backus" },
  "contribs" : [ "Fortran", "ALGOL", "Backus-Naur Form", "FP" ],
  "awards" : [
    {
      "award" : "W.W. McDowell Award",
      "year" : 1967,
      "by" : "IEEE Computer Society"
    }, {
      "award" : "Draper Prize",
      "year" : 1993,
      "by" : "National Academy of Engineering"
    }
  ]
}

```

• **document** store, no-sql database
• **hash-based**, schema-less database
• written in C++
• support API in many computer language:

### Shard

内置`Auto-Sharding`自动分片支持云级扩展性，分片简单.

**Each server can contain multiple shards, but the lead node and the following node will never on one server.**

```java
//lead node? primary mongod and secodary mongods(seconary is the replica of data.)
```



![5.png](https://github.com/aloha666/Java2021/blob/main/5.png?raw=true)

### **Mongod**: database instance, a Node

### **Config Severs**

保存集群的元数据（metadata），包含各个Shard的路由规则，配置服务器由一个副本集(ReplicaSet)组成。To determine which shard to go to?

### Mongos: sharding process (类似router)

• analogous类似 to a database router
• process all request
• decides how many and which mongods should receive the query
• collect the result and send it back

Mongos启动后，会从 Config Server 加载元数据，开始提供服务，并将用户的请求正确路由到对应的Shard。

### Mongo:  an interactive shell



### Functionality of MongoDb

• dynamic schema: meaning?
• document based
• support **secondary indexes** (expect primary index, all other index are secondary index)

• master-slave replication
• horizontal scaling
• range based partition
• no joins and transactions
• CP



## Redis

http://guanzhou.pub/2020/06/04/Redis/

Redis (remote directory server) is an **in-memory,** **key value data**，完全基于内存

### structure store 

• key :  Printable ASCII 

• value :

​		○ primitives: String 

​		○ Containers (of strings) :Hashes, Lists（双向链表）, Sets, Sorted Sets

```c++
//string
redis 127.0.0.1:6379> SET runoob "菜鸟教程"
OK
redis 127.0.0.1:6379> GET runoob
"菜鸟教程"
  
//Hashes
  
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> HMSET runoob field1 "Hello" field2 "World"
OK  
redis 127.0.0.1:6379> HGET runoob field1
"Hello"
redis 127.0.0.1:6379> HGET runoob field2
"World"
  
//list
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabbitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabbitmq"
2) "mongodb"
3) "redis"
  
 //set
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabbitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabbitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "redis"
2) "rabbitmq"
3) "mongodb"
  
//sortSet
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabbitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabbitmq
(integer) 0
redis 127.0.0.1:6379> ZRANGEBYSCORE runoob 0 1000
1) "mongodb"
2) "rabbitmq"
3) "redis"


```



### Redis usage in cache

- 存储 **缓存** 用的数据；

- 需要高速读/写的场合**使用它快速读/写**

  

### Redis suports two persistence mechanisms （持久化，把内存中数据持久化到disk上）

• RDB (redis database): the RDB persistence performs point-in-time snapshots of dataset as specifiedintervals

按时备份

• AOF (append only file): the AOF persistence logs every write operation received by the server

记录写操作



### Redis 特点

• various data types

 • support persistence mechanism 

• support cluster mode

```java
/*
#### 是否使用过Redis集群，集群的高可用怎么保证，集群的原理是什么？

Redis Sentinal着眼于高可用，在master宕机时会自动将slave提升为master，继续提供服务。

Redis Cluster着眼于扩展性，在单个redis内存不足时，使用Cluster进行分片存储。

#### 单机会有瓶颈，那你们是怎么解决这个瓶颈的？

我们用到了集群的部署方式也就是Redis cluster，并且是主从同步读写分离，类似Mysql的主从同步

Redis cluster 支撑 N 个 Redis master node，每个master node都可以挂载多个 slave node。

这样 Redis 就可以横向扩容了。如果你要支撑更大数据量的缓存，那就横向扩容更多的 master 节点，每个 master 节点就能存放更多的数据了。
*/
```



### Other usage:

• distributed lock 分布式锁 为了保证数据的最终一致性
		○ SETNX (set if not exists)
					• return 0, the key is already locked by some other clients
					• return 1, the client get the lock
• message system
• store configuration information



### MongoDB`与`Redis比较：

1. `MongoDB`文件存储是`Bson`格式，类似`Json`，或自定义的二进制格式。`MongoDB`与`Redis`性能都很依赖内存的大小，`MongoDB`有丰富的数据表达、索引；最类似于关系数据库，支持丰富的查询语言，`Redis`数据丰富，较少的 IO，这方面`MongoDB`优势明显。

2. `MongoDB`不支持事务，靠客户端自身保证，`Redis`支持事务，比较弱，仅能保证事务中的操作按顺序执行，这方面`Redis`优于`MongoDB`。

3. `MongoDB`对海量数据的访问效率提升，`Redis`较小数据量的性能及运算，这方面`MongoDB`优于`Redis`。

4. `MongoDB`有`MapReduce`功能，提供数据分析，`Redis`没有，这方面`MongoDB`优于`Redis`。

5. `MongoDB`应用场景：大规模不需要事务及复杂join支持,需要快速读写及水平扩展：如craglist，

   `Redis`应用场景：高性能，单进程， 多用于缓存，秒杀场景。

   ```java
   /*
   现在说明一下，如果现在做一个秒杀，那么，Redis应该如何结合进行使用?
   
   提前预热数据，放入Redis
   商品列表放入Redis List
   商品的详情数据 Redis hash保存，设置过期时间
   商品的库存数据Redis sorted set保存
   用户的地址信息Redis set保存
   订单产生扣库存通过Redis制造分布式锁，库存同步扣除
   订单产生后发货的数据，产生Redis list，通过消息队列处理
   秒杀结束后，再把Redis数据和数据库进行同步
   */
   ```


# Lecture 8 Sql and no-sql



## SQL vs No-Sql

| SQL                                             | NO-Sql                                        |
| ----------------------------------------------- | --------------------------------------------- |
| relational database management system           | Non-relational or distributed database system |
| have fixed or static or predefined schema架构   | dynamic schema                                |
| vertically scalable                             | horizontal scalable                           |
| not suited for hierarchical 分层的 data storage | suited                                        |
| ACID                                            | CAP                                           |

**vertical scaling:** adding prcoessing power to the server to make it faster 

**horizontal scaling:** add more servers/ machine/ nodes in the cluster

**ACID:**

• atomicity原子性: all changes to data are performed as if they are a single operation

 • consistency持续性: Data is in a consistent state when a transaction starts and ends

 • Isolation独立性: The intermediate state of a transcation is invisible to other transactions

• durability持续性: after a transaction sucessfully completes, changes to data persist and are not undone.



## Index

Indexing is a way to optimize the performace of a database by minimizing the number of disk accesses      索引主要用来提升数据检索速度,在数据量很大的时候很有用. 索引相当于图书馆的图书目录,你要找本书可以在图书目录上找到这本书在哪个书架第几本,这样明显比到书架去找书要快得多,索引就是这个道理. 

- cluster index (primary index)
- non-cluster index (secondary index)

**Cluster Index：** **聚集索引一般是表中的主键索引（也有可能不是），如果表中没有显示指定主键，则会选择表中的第一个不允许为NULL的唯一索引，如果还是没有的话，就采用Innodb存储引擎为每行数据内置的6字节ROWID作为聚集索引。**

- defines the order in which data is **physically stored**

- one table have only one order -> **one cluster index per table**

  ```sql
  create CLUSTERED INDEX 索引名称 ON 表名(字段名)
  ```
  
  

**Non-cluster index**: (自定义index) such as Name 

- doesn't sort the physical data inside the table

- allow to have more than one non-cluster index per table

  ```sql
  create NONCLUSTERED INDEX 索引名称 ON 表名(字段名)
  
  --将指定字段设置成主键非聚集索引
  alter table 表名 
  add constraint 主键约束名称 primary key NONCLUSTERED(字段名) 
  
  --创建表指定主键为非聚集索引,默认不写NONCLUSTERED为聚集索引
  CREATE TABLE Test
  ( 
    ID INT PRIMARY KEY NONCLUSTERED  --非聚集索引
  )
  
  ```

  

where the non-cluster index saved? saved with data?  

```java
/*聚集索引表记录的排列顺序与索引的排列顺序一致

优点是查询速度快，因为一旦具有第一个索引值的纪录被找到，具有连续索引值的记录也一定物理的紧跟其后。
缺点是对表进行修改速度较慢，这是为了保持表中的记录的物理顺序与索引的顺序一致，而把记录插入到数据页的相应位置，必须在数据页中进行数据重排， 降低了执行速度。建议使用聚集索引的场合为：
a. 此列包含有限数目的不同值；
b. 查询的结果返回一个区间的值；
c. 查询的结果返回某值相同的大量结果集。
非聚集索引指定了表中记录的逻辑顺序，但记录的物理顺序和索引的顺序不一致，聚集索引和非聚集索引都采用了B+树的结构，但非聚集索引的叶子层并不与实际的数据页相重叠，而采用叶子层包含一个指向表中的记录在数据页中的指针的方式。
非聚集索引比聚集索引层次多，添加记录不会引起数据顺序的重组。
建议使用非聚集索引的场合为：
a. 此列包含了大量数目不同的值；
b. 查询的结束返回的是少量的结果集；
c. order by 子句中使用了该列。

作者：bobcorbett
链接：https://www.jianshu.com/p/5681ebd5b0ef
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



**B tree vs B+ tree**

B+ tree index is most used cluster index.

![img](https://media.geeksforgeeks.org/wp-content/uploads/20191219160544/Untitled-Diagram111.png)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/Btree.jpg" alt="img" style="zoom: 67%;" />



|      | B tree                                                       | B+ tree                                                      |
| :--- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 1.   | All internal and leaf nodes have data pointers               | Only leaf nodes have data pointers                           |
| 2.   | Since all keys are not available at leaf, search often takes more time. | All keys are at leaf nodes, hence search is faster and accurate.. |
| 3.   | No duplicate of keys is maintained in the tree.              | Duplicate of keys are maintained and all nodes are present at leaf. |
| 4.   | Insertion takes more time and it is not predictable sometimes. | Insertion is easier and the results are always the same.     |
| 5.   | Deletion of internal node is very complex and tree has to undergo lot of transformations. | Deletion of any node is easy because all node are found at leaf. |
| 6.   | Leaf nodes are not stored as structural linked list.         | Leaf nodes are stored as structural linked list.             |
| 7.   | No redundant search keys are present..                       | Redundant search keys may be present..                       |



**B tree 索引 vs Hash索引:**

B tree: good for range search, O(logn) B+ tree?

Hash: efficient for looking up values, not eff for range search



**Bitmap Index索引:** use 1 and 0 to represent yes/no, thus a number 101001 represents yes to column 0, 2 and 5 while no to column 1 and 3.

• columns with low selectivity



## Query execution and optimization

### Typical RDBMS Execution (working flow )

![8.png](https://github.com/aloha666/Java2021/blob/main/images/8.png?raw=true)

### Example SQL Query



##### 1.Parse Tree

![9.png](https://github.com/aloha666/Java2021/blob/main/images/9.png?raw=true)

##### 2.Logical Query Plan

![10.png](https://github.com/aloha666/Java2021/blob/main/images/10.png?raw=true)

##### 3.Improved Logical Query Plan

![11.png](https://github.com/aloha666/Java2021/blob/main/images/11.png?raw=true)

##### 4. Estimate Result Sizes

![12.png](https://github.com/aloha666/Java2021/blob/main/images/12.png?raw=true)

##### 5. Physical Plan

![13.png](https://github.com/aloha666/Java2021/blob/main/images/13.png?raw=true)

![14.png](https://github.com/aloha666/Java2021/blob/main/images/14.png?raw=true)



##### 6. Estimating Plan Cost

![15.png](https://github.com/aloha666/Java2021/blob/main/images/15.png?raw=true)



### SQL Tuning (when sql is slow)

1. using **execution plan** to identify the cause of slowness 诊断原因

2. try to reduce joins. remove unused join and join conditions

<img src="https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png" alt="img" style="zoom:67%;" />

```sql
--SQL JOIN 子句用于把来自两个或多个表的行结合起来，基于这些表之间的共同字段。

SELECT Websites.id, Websites.name, access_log.count, access_log.date
FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id;


```

3. use UNION ALL (do not remove duplicate data) instead UNION (will remove duplicate data)

```sql
--SQL UNION 操作符合并两个或多个 SELECT 语句的结果。默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

4. use the LIMIT to do the pagination 分页 to get part of the data?

```sql
--按照成绩排名，并取出第m名到第n名的学生信息 m-1开始，n-m+1 范围
SELECT * FROM list ORDER BY score LIMIT m-1, n - m + 1
```

5. Create View or stored procedure to improve performance

6. avoid using IN，O(N*M) time

```sql
--IN 操作符允许您在 WHERE 子句中规定多个值。
--语句含义：返回column_name是{value1，value2...}的数据行

SELECT *
FROM table_name
WHERE column_name IN (value1,value2,...);
```



### Join（not in interview）

- index join: if an index  exists

- merge join: if at least one table is sorted

- hash join: if both tables unsorted

- ...

  

```sql
/*index join*/
for each r in R1:
list = index_lookup(R2, C, r.C) for each s in list
output(r, s) I/O time

/*merge join*/
procedure outputTuples:
while R1[i].C == R2[j].C && i < T(R1)
jj = j
while R1[i].C == R2[jj].C && jj <= T(R2):
output(R1[i], R2[jj])
jj ++ i += 1

/*hash join*/
for each r in R1:
list = hash_lookup(R2, C, r.C) for each s in list
output(r, s)

/*left/right/outer join on application level干什么, above join are in physican plan怎么干*/


```

# Lecture 9 Transactions

## Transaction / ACID / Rollback / Commit

### Transaction事务

is an action or a series of actions, carried out by a single user or application, which reads or updates the contents of a database.

### ACID

•Atomicity 要么全部完成，要么全部不完成；
	○tractions are atomic - they don't have parts
	○can't be executed paritcally, either all happen, or nothing happens
•Consistency 
	○transactions take the database from one consistent state into another
•Isolation 一个事务单元需要提交之后才会被其他事务可见；
	○The effects of a tranaction are not visible to other transactions until it has completed
•Durablility 事务提交后即持久化到磁盘不会丢失。
	○Once a transaction has completed, its changes are made permanent

### Commit: success

### Rollback: fail



## Isolation Levels 数据库隔离级别

**Dirty read**: read UNCOMMITED data from another transaction (may did a rollback  by other transaction , so the value read is not correct) 

修改数据不加锁---》脏读（正在修改的数据未提交时被读取）

**Non-repeatable read**: read COMMITED data from an UPDATE query from another transaction 

查询数据不加锁---》不可重复读（正在读取的数据，被修改了再次读取就不一样了）

**Phantom read**: read COMMITTED data from an INSERT or DELETE query from another transaction 

读、修改都加锁---》幻读（读取之后，有数据添加）



**isolation level**
	• read uncommited
	• read committed
	• repeatable read 
	• serizalizable

| Isolation Level                                 | Dirty Reads | Unrepeatable Reads | Phantom Reads |
| ----------------------------------------------- | ----------- | ------------------ | ------------- |
| Read uncommitted 允许其他事务看到没有提交的数据 | Y           | Y                  | Y             |
| Read committed 被读取的数据可以被其他事务修改   | N           | Y                  | Y             |
| Repeatable read                                 | N           | N                  | Y             |
| Serializable 事物依次执行避免幻读               | N           | N                  | N             |

Y-yes to the problem, N- prevent the problem.

**Repeatable read：** 所有被Select获取的数据会生成快照，这样就可以避免一个事务前后读取数据不一致的情况。但是却没有办法控制幻读，因为这个时候其他事务不能更改所选的数据，但是可以增加数据，因为前一个事务没有范围锁。

**串行化（SERIALIZABLE）**：所有事务都一个接一个地串行执行，这样可以避免幻读（phantom reads）。对于基于锁来实现并发控制的数据库来说，串行化要求在执行范围查询（如选取年龄在10到30之间的用户）的时候，需要获取范围锁（range lock）。如果不是基于锁实现并发控制的数据库，则检查到有违反串行操作的事务时，需要滚回该事务。

**range lock间隙锁**： 是一种加在两个索引之间的**锁**，或者加在第一个索引之前，或最后一个索引之后的间隙。 有时候又称为**范围锁**（**Range Locks**），这个**范围**可以跨一个索引记录，多个索引记录，甚至是空的。 使用间隙**锁**可以防止其他事务在这个**范围**内插入或修改记录，**保证两次读取这个范围内的记录不会变**，从而不会出现幻读现象。虽然解决了幻读问题，但是数据库的并发性一样受到了影响.

**transaction**. -> a series of actions
**schedule** -> a series of transactions
				-> one by one **(Serilizable level isolation)**

**lock strategy**: the higher the isolation, the more locks required. and slower.



## Lock

https://www.aneasystone.com/archives/2017/11/solving-dead-locks-two.html 解决死锁之路 - 了解常见的锁类型

### Types of Lock

**Binary lock**
	• locked. (x = 1)
	• unlocked (x =0)
**Shared lock and exclusive lock**
	• share lock == read lock 读可以share can only read  **S锁**
	• exclusive lock == write lock 写必须独有 can write and  read **X锁**



### Read Lock & Write Lock

##### Read Lock:

- 持有读锁的会话可以读表，但不能写表；
- 允许多个会话同时持有读锁；
- 其他会话就算没有给表加读锁，也是可以读表的，但是不能写表；
- 其他会话申请该表写锁时会阻塞，直到锁释放。

##### Write Lock:

- 持有写锁的会话既可以读表，也可以写表；

- 只有持有写锁的会话才可以访问该表，其他会话访问该表会被阻塞，直到锁释放；

- 其他会话无论申请该表的读锁或写锁，都会阻塞，直到锁释放。

  

### **每个级别需要什么锁？**

- 读未提交（Read Uncommitted）：事务读不阻塞其他事务读和写，事务写阻塞其他事务写但不阻塞读；通过对写操作加 “持续X锁”，对读操作**不加锁** 实现；

- 读已提交（Read Committed）：事务读不会阻塞其他事务读和写，事务写会阻塞其他事务读和写；通过对写操作加 “**持续X锁**”，对读操作加 “**临时S锁**” 实现；不会出现脏读；

- 可重复读（Repeatable Read）：事务读会阻塞其他事务事务写但不阻塞读，事务写会阻塞其他事务读和写；通过对写操作加 **“持续X锁”**，对读操作加 “**持续S锁**” 实现；

- 序列化（Serializable）：为了解决幻读问题，**行级锁**做不到，需使用**表级锁**。

  

### Dead lock: 

both transactions wait for other to release a lock.

Deadlock detection -> wait for graph (cycle in graph means deadlock)

**prevent the dead lock (methods?)**

​		• Conservative 2PL：**two-phase locking** (**2PL**) is a [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) method that guarantees [serializability](https://en.wikipedia.org/wiki/Serializability).

```java
Expanding phase: locks are acquired and no locks are released.
Shrinking phase: locks are released and no locks are acquired
```

​		• wait-die or wound-wait
​		• ....

**optimistic lock**

就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在提交更新的时候会判断一下在此期间别人有没有去更新这个数据。乐观锁适用于读多写少的应用场景，这样可以提高吞吐量。

乐观锁：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。

乐观锁一般来说有以下2种方式：

使用数据版本（Version）记录机制实现，这是乐观锁最常用的一种实现方式。何谓数据版本？即为数据增加一个版本标识，一般是通过为数据库表增加一个数字类型的 “version” 字段来实现。当读取数据时，将version字段的值一同读出，数据每更新一次，对此version值加一。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的version值进行比对，如果数据库表当前版本号与第一次取出来的version值相等，则予以更新，否则认为是过期数据。

使用时间戳（timestamp）。乐观锁定的第二种实现方式和第一种差不多，同样是在需要乐观锁控制的table中增加一个字段，名称无所谓，字段类型使用时间戳（timestamp）, 和上面的version类似，也是在更新提交的时候检查当前数据库中数据的时间戳和自己更新前取到的时间戳进行对比，如果一致则OK，否则就是版本冲突。

**pessimistic lock**

悲观锁，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。

Java synchronized 就属于悲观锁的一种实现，每次线程要修改数据时都先获得锁，保证同一时刻只有一个线程能操作数据，其他线程则会被block。



## Distributed Transaction 分布式事务

**分布式事务是为了解决微服务架构（形式都是分布式系统）中不同节点之间的数据一致性问题。这个一致性问题本质上解决的也是传统事务需要解决的问题，即一个请求在多个微服务调用链中，所有服务的数据处理要么全部成功，要么全部回滚**

A distributed transaction is **a set of operations on data that is performed across two or more data repositories** (especially databases). It is typically coordinated across separate nodes connected by a network, but may also span multiple databases on a single server.

### 2PC

**2 phase commitment: prepare and commit**

2pc是一个非常经典的**强一致、中心化的原子提交协议**。这里所说的中心化是指协议中有两类节点：一个是中心化**协调者节点（coordinator）**和**N个参与者节点（partcipant）**。

​		• transaction coordinator sends prepare message to each participating node

​		 • each participating node responds to coordinator with prepared or no

​	     • if coordinator receives all prepared: broadcast commit 

​		• if coordinator receives any no: broadcast abort

**CanCommit**

![640?wx_fmt=png](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/l89kosVutontX5z9jbdn8FnGsmBcghzLYTVwvdWlXg7EtOAsE7fmBGbpYiasptViazc8uYI3HNdmTSiaX6NCo3ssA/640?wx_fmt=png)

**PreCommit**

![640?wx_fmt=png](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/l89kosVutontX5z9jbdn8FnGsmBcghzLQdwgHZWsict28a2K9TB1jz5fFgkCnianEtEhyY0yHpWKN8X6laraQz6w/640?wx_fmt=png)

**DoCommit**

![640?wx_fmt=png](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/l89kosVutontX5z9jbdn8FnGsmBcghzLNUOn74RUywTe6lu56wu9XAdaxmQqvqx1A1eMFY7SHyYJAm1M4txhiaQ/640?wx_fmt=png)

在阶段一中，如果所有的参与者都返回Yes的话，那么就会进入PreCommit阶段进行事务预提交。此时分布式事务协调者会向所有的参与者节点发送PreCommit请求，参与者收到后开始执行事务操作，并将Undo和Redo信息记录到事务日志中。参与者执行完事务操作后（此时属于未提交事务的状态），就会向协调者反馈“Ack”表示我已经准备好提交了，并等待协调者的下一步指令。

否则，如果阶段一中有任何一个参与者节点返回的结果是No响应，或者协调者在等待参与者节点反馈的过程中超时（**2PC中只有协调者可以超时，参与者没有超时机制）**。整个分布式事务就会中断，协调者就会向所有的参与者发送**“abort”**请求。



### Saga 

Saga 是一种补偿协议，在 Saga 模式下，分布式事务内有多个参与者，每一个参与者都是一个冲正补偿服务，需要用户根据业务场景实现其正向操作和逆向回滚操作。

分布式事务执行过程中，依次执行各参与者的正向操作，如果所有正向操作均执行成功，那么分布式事务提交。**如果任何一个正向操作执行失败，那么分布式事务会退回去执行前面各参与者的逆向回滚操作，回滚已提交的参与者，使分布式事务回到初始状态。**

![Saga](https://cdn.nlark.com/yuque/0/2019/jpeg/226702/1565776830706-3a486950-04fb-48c9-9322-c0119e560ff0.jpeg)

##### there are two ways to achieve sagas:

**Choreograph 协调编排:** 

Choreography 是一种协调 sagas 的方法，在此方法中，参与者无需集中控制即可交换事件。 对于     choreography，每个本地事务都会发布触发其他服务中的本地事务的域事件。

![Choreography 概述](https://docs.microsoft.com/zh-cn/azure/architecture/reference-architectures/saga/images/choreography-pattern.png)

​		each local transaction publishes domain events that trigger local transactions in other services

#### 优点

- 适用于需要少量参与者且不需要协调逻辑的简单工作流。
- 不需要额外的服务实现和维护。
- 不会引入单点故障，因为责任分布在 saga 参与者。

#### 缺点

- 添加新步骤时，工作流可能会令人感到困惑，因为这样做很难跟踪哪些 saga 参与者侦听哪些命令。
- Saga 参与者之间存在循环依赖关系，因为它们必须使用彼此的命令。
- 集成测试非常困难，因为所有服务都必须运行才能模拟事务。

 **Orchestration 业务流程**: 

业务流程是一种协调 sagas 的方式，其中集中式控制器告知 saga 参与者要执行的本地事务。 Saga orchestrator (or SEC, saga cordination executor) 处理所有事务，并告诉参与者基于事件执行哪个操作。 Orchestrator 执行 saga 请求，存储和解释每个任务的状态，并处理补偿事务的故障恢复。

![业务流程概述](https://docs.microsoft.com/zh-cn/azure/architecture/reference-architectures/saga/images/orchestrator.png)

an orchestrator tells the participants what local transactions to execute

#### 优点

- 适用于涉及多个参与者的复杂工作流或随时间推移增加的新参与者。
- 适用于控制过程中的每个参与者，并控制活动流的情况。
- 不会引入循环依赖关系，因为 orchestrator 单方面依赖于 saga 参与者。
- Saga 参与者无需知道其他参与者的命令。 清除问题分离可简化业务逻辑。

#### 缺点

- 其他设计复杂性要求实现协调逻辑。
- 还有一个额外的故障点，因为 orchestrator 管理完整的工作流。

自己理解（？）：

**编排（Choreography）**：参与者（子事务）之间的调用、分配、决策和排序，通过交换事件进行进行。是一种去中心化的模式，参与者之间通过消息机制进行沟通，通过监听器的方式监听其他参与者发出的消息，从而执行后续的逻辑处理。由于没有中间协调点，靠参与靠自己进行相互协调。

**控制（Orchestration）**：Saga提供一个控制类，其方便参与者之前的协调工作。事务执行的命令从控制类发起，按照逻辑顺序请求Saga的参与者，从参与者那里接受到反馈以后，控制类在发起向其他参与者的调用。所有Saga的参与者都围绕这个控制类进行沟通和协调工作

### 2PC vs Saga**对比？**

自己理解（？）：

2PC：先问一遍行不行 行就commit，不行就abort 。

Saga: 干了再说，干不下去了就回滚到原来状态。



# Lecture 10 Mysql Operations

## Mysql Operations

```sql
-- CRUD OPREATIONS

-- create schema testdb

-- create a table

-- create table testdb.Persons(
-- personID int primary key,
-- LastName varchar(255),
-- FirstName varchar(255),
-- Address varchar(255),
-- City varchar(255)
-- )

-- create table base on existing table

-- create table testdb.employees as
-- select personId, firstname, lastname
-- from testDB.persons


-- alter table, add column

-- alter table testdb.employees
-- add dep varchar(255);

-- add multiple columns

-- alter table testdb.employees
-- add(
-- salaray int,
-- address varchar(255)
-- ) 

-- alter table drop colmn

-- alter table testdb.employees
-- drop address

-- alter table, modify column
-- alter table testdb.employees
-- modify column dep varchar(144)


-- insert data

-- use testdb;
-- insert into employees(personid, firstname, lastname, dep, salaray)
-- values
-- (1,'lei','li','HR','10000'),
-- (2,'meimei','han','Account','20000')

-- if add all vaule, can ignore the column name
-- use testdb;
-- insert into employees
-- values
-- (3,'san','zhang','sale','20000')


-- update data

-- set sql_safe_updates =0;

-- update testdb.employees
-- set salaray = 40000
-- where lastname = 'zhang';

-- delete 删一行数据 truncate 删所有数据 保留结构  drop 删掉表

```

https://zhuanlan.zhihu.com/p/29413183 Learn SQL | 基础操作综合练习

```sql
-- create table testdb.student
-- (
-- SNO VARCHAR(3) NOT NULL, 
-- SNAME VARCHAR(4) NOT NULL,
-- SSEX VARCHAR(2) NOT NULL,
-- SBIRTHDAY DATETIME,
-- CLASS VARCHAR(5)
-- );

-- create table course
-- (
-- CNO VARCHAR(5) NOT NULL, 
-- CNAME VARCHAR(10) NOT NULL,
-- TNO VARCHAR(10) NOT NULL
-- );

-- create table score
-- ( 
-- SNO VARCHAR(3) NOT NULL, 
-- CNO VARCHAR(5) NOT NULL, 
-- DEGREE NUMERIC(10, 1) NOT NULL
-- );

-- create table teacher
-- ( 
-- TNO VARCHAR(3) NOT NULL, 
-- TNAME VARCHAR(4) NOT NULL, 
-- TSEX VARCHAR(2) NOT NULL, 
-- TBIRTHDAY DATETIME NOT NULL, 
-- PROF VARCHAR(6), 
-- DEPART VARCHAR(10) NOT NULL
-- );

-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (108 ,'曾华' 
-- ,'男' ,'1977-09-01',95033);
-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (105 ,'匡明' 
-- ,'男' ,'1975-10-02',95031);
-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (107 ,'王丽' 
-- ,'女' ,'1976-01-23',95033);
-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (101 ,'李军' 
-- ,'男' ,'1976-02-20',95033);
-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (109 ,'王芳' 
-- ,'女' ,'1975-02-10',95031);
-- insert into student (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (103 ,'陆君' 
-- ,'男' ,'1974-06-03',95031);




-- insert into course (CNO,CNAME,TNO)VALUES ('3-105' ,'计算机导论',825);
-- insert into course (CNO,CNAME,TNO)VALUES ('3-245' ,'操作系统' ,804);
-- insert into course (CNO,CNAME,TNO)VALUES ('6-166' ,'数据电路' ,856);
-- insert into course (CNO,CNAME,TNO)VALUES ('9-888' ,'高等数学' ,100);


-- insert into score (SNO,CNO,DEGREE)VALUES (103,'3-245',86);
-- insert into score (SNO,CNO,DEGREE)VALUES (105,'3-245',75);
-- insert into score (SNO,CNO,DEGREE)VALUES (109,'3-245',68);
-- insert into score (SNO,CNO,DEGREE)VALUES (103,'3-105',92);
-- insert into score (SNO,CNO,DEGREE)VALUES (105,'3-105',88);
-- insert into score (SNO,CNO,DEGREE)VALUES (109,'3-105',76);
-- insert into score (SNO,CNO,DEGREE)VALUES (101,'3-105',64);
-- insert into score (SNO,CNO,DEGREE)VALUES (107,'3-105',91);
-- insert into score (SNO,CNO,DEGREE)VALUES (108,'3-105',78);
-- insert into score (SNO,CNO,DEGREE)VALUES (101,'6-166',85);
-- insert into score (SNO,CNO,DEGREE)VALUES (107,'6-106',79);
-- insert into score (SNO,CNO,DEGREE)VALUES (108,'6-166',81);


-- insert into teacher (TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
-- VALUES (804,'李诚','男','1958-12-02','副教授','计算机系');
-- insert into teacher (TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
-- VALUES (856,'张旭','男','1969-03-12','讲师','电子工程系');
-- insert into teacher (TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)
-- VALUES (825,'王萍','女','1972-05-05','助教','计算机系');
-- insert into teacher (TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
-- VALUES (831,'刘冰','女','1977-08-14','助教','电子工程系');



-- 1. 查询Student表中的所有记录的sname、ssex和class列
-- select sname,ssex,class
-- from student;

-- 2. 查询教师所有的单位（即不重复的depart列）
-- select distinct depart 
-- from teacher

-- 3. 查询student表的所有记录
-- select *
-- from student

-- 4. 查询score表中成绩在60到80之间的所有记录
-- select * 
-- from score 
-- where degree<=80 and degree>=60;

-- 5.查询score表中成绩为85，86或88的记录
-- select *
-- from score
-- where degree in (85,86,88);

-- 6. 查询student表中"95031"班或性别为"女"的同学记录
-- select *
-- from student
-- where class = '95031' or ssex = '女';

-- 7. 以class降序查询student表的所有记录
-- select * 
-- from student
-- order by class desc;

-- 8. 以cno升序、degree降序查询score表的所有记录
-- select *
-- from score
-- order by cno asc, degree desc;

-- 9. 查询"95031"班的学生人数
-- select count(*)
-- from student 
-- where class = 95031;

-- 10. 查询score表中的最高分的学生学号和课程号
-- select sno, cno,degree
-- from score
-- where degree =
-- (select max(degree) from score);

-- 11. 查询"3-105"号课程的平均分
-- select avg(degree)
-- from score
-- where cno='3-105';

-- 12. 查询score表中至少有5名学生选修的并以3开头的课程的平均分数

-- select avg(degree)
-- from score 
-- where cno like '3%'
-- group by cno
-- having count(*)>=5


-- 13. 查询最低分大于70，最高分小于90的sno列
-- select sno
-- from score
-- group by sno
-- having max(degree)<90 and min(degree)>70;

-- 14. 查询所有学生的sname、cno和degree列
-- select a.sname, b.cno, b.degree
-- from student as a
-- join score as b
-- on a.sno =b. sno;

-- 15. 查询所有学生的sno、cname和degree列

-- select a.sno, a.sname, b.degree
-- from student as a
-- join score as b
-- on a.sno = b.sno;

-- 16. 查询所有学生的sname、cname和degree列
-- select a.sname,b.cname,c.degree
-- from student as a
-- join course as b
-- join score as c
-- on a.sno=c.sno and b.cno = c.cno;


-- 17. 查询"95033"班所选课程的平均分
-- select avg(c.degree)
-- from (
-- select a.sno,a.class,b.degree
-- from student as a 
-- join score as b
-- on a.sno = b.sno
-- ) as c
-- where c.class = '95033';

-- select avg(degree) from score 
-- where sno in (
-- select sno from student where class = '95033'
-- );


-- 18. 假设使用如下命令建立了一个grade表，现查询所有同学的Sno、Cno和rank列，并按照rank列排序
-- create table grade
-- (
-- low numeric(3,0),
-- upp numeric(3,0),
-- grank varchar(1)
-- );

-- insert into grade values (90,100,'A');
-- insert into grade values (80,89,'B');
-- insert into grade values (70,79,'C');
-- insert into grade values (60,69,'D');
-- insert into grade values (0,59,'E');


-- select a.sno,a.cno,b.rank from score a 
-- join grade b 
-- where a.degree between b.low and b.upp
-- order by rank;



-- 19. 查询score表中选修"3-105"课程的成绩高于"109"号同学成绩的所有同学的记录
-- # 解法一
-- select * from score where cno = '3-105'
-- and degree > (
-- select degree from score where sno = 109 and cno = '3-105'
-- );
-- # 解法二
-- select a.* from score a 
-- where a.cno = '3-105' and a.degree > all(select degree from score b 
-- where b.sno = '109' and b.cno = '3-105');


20. 查询score中选学一门以上课程的同学中分数为非最高分成绩的记录
select * from score s inner join (
select ss.sno, max(ss.degree) as maxd from score ss group by ss.sno 
having count(ss.cno)> 1 
) 
a on s.sno=a.sno and s.degree <> a.maxd;
21. 查询和学号为107的同学同年出生的所有学生的sno、sname和sbirthday列
考察日期与时间函数的运用

select sno,sname,sbirthday from student where year(sbirthday) in 
(
select year(sbirthday) from student wehre sno = 107
);
-- 22. 查询"张旭"教师任课的学生成绩
select a.sno,a.cno,a.degree
from score as a
join (select c.cno,c.tno
from course as c
join (select * from teacher where tname ='张旭') as t
on c.tno=t.tno) as b
on a.cno = b.cno;




select c.cno,c.tno
from course as c
join (select * from teacher where tname ='张旭') as t
on c.tno=t.tno;


-- select degree from score where cno in (
-- select cno from course where tno in (
-- select tno from teacher where teacher = '张旭'));
-- # 进阶解法
-- select a.degree from score a 
-- join (teacher b,course c)
-- on a.cno = c.cno and b.tno = c.tno
-- where b.tname = '张旭';

-- 23. 查询选修某课程的同学人数多于5人的教师姓名

select cno
from score 
group by cno
having count(*)>5;


select tname
from teacher
where tno =
(select tno 
from course
where cno = (select cno
from score 
group by cno
having count(*)>5))




-- select a.tname from teacher a 
-- join(course b,score c)
-- on a.tno = b.tno and b.cno = c.cno
-- group by c.cno having count(*) > 5;
-- 24. 查询所有表中关于"95033"班和"95031"班全体学生的信息记录

select *
from student as st
join score as sc
on st.sno = sc.sno
where st.class ='95033' or st.class ='95031' 





-- select * from student a inner join score b 
-- on a.sno = b.sno inner join course c
-- on b.cno = c.cno inner join teacher d
-- on c.tno = d.tno 
-- where a.class = '95033' or a.class = '95031';
-- 25. 查询存在有85分以上成绩的课程cno

select cno
from score
where degree>85
group by cno;


-- # 解法一：
-- select distinct cno from score where degree > 85;
-- # 解法二：
-- select cno from score group by cno having max(degree) >85;
-- 26. 查询出"计算机系"教师所教课程的成绩表

select sc.cno,sc.degree,t.tname
from score as sc
join (course as c,teacher as t)
on sc.cno = c.cno and c.tno = t.tno
where t.DEPART = '计算机系';




-- select a.*,b.cname,c.tname,c.depart from score a
-- join (course b, teacher c)
-- on a.cno = b.cno and b.tno = c.tno
-- where c.depart = '计算机系';
-- 27. 查询"计算机系"中与"电子工程系"没有相同职称的教师的tname和prof






-- select tname,prof from teacher where depart = '计算机系' and prof not in
-- (select prof from teacher where depart = '电子工程系');
-- 28. 查询选修编号为"3-105"课程且成绩高于选修编号为"3-245"的同学的cno、sno和degree,并按degree从高到低次序排序。

select * from score as a,score as b
where a.sno=b.sno
and a.cno = '3-105' and b.cno = '3-245'
and a.degree>b.degree
order by a.degree desc;

-- select * from score as a,score as b
-- where a.cno = '3-105' and b.cno = '3-245'
-- and a.sno = b.sno
-- and a.degree > b.degree
-- order by a.degree desc;
-- 29. 查询所有教师和同学的name、sex和birthday
select s.sname, s.ssex,s.sbirthday
from student as s
where s.ssex = '女'
union
select t.tname, t.tsex, t.tbirthday
from teacher as t
where t.tsex = '女'

-- 30. 查询所有女教师和女同学的name、sex和birthday
-- # 29
-- select sname as name, ssex as sex, sbirthday as birthday from student
-- union
-- select tname as name, tsex as sex, tbirthday as birthday from teacher;
-- # 30
-- select sname as name, ssex as sex, sbirthday as birthday from student
-- where ssex = '女'
-- union
-- select tname as name, tsex as sex, tbirthday as birthday from teacher
-- where tsex = '女';

-- 31. 查询成绩比该课程平均成绩低的同学的成绩表






-- select a.* from score a where degree < (
-- select avg(degree) from score b 
-- where a.cno = b.cno );
-- 32. 查询所有任课教师的tname和depart
-- 33. 查询所有未讲课的教师的tname和depart
-- # 32
-- # 解法一
-- select a.tname,a.depart from teacher a 
-- join course b 
-- on a.tno = b.tno;
-- # 解法二
-- select a.tname,a.depart from teacher a
-- where exists (
-- select * from course b where a.tno = b.tno
-- );
-- # 33
-- select a.tname,a.depart from teacher a
-- where not exists (
-- select * from course b where a.tno = b.tno
-- );
-- 34. 查询至少有2名男生的班号
-- select class from student where ssex = '男' 
-- group by class 
-- having count(ssex) >= 2;
-- 35. 查询Student表中不姓“王”的同学记录
-- select * from student where sname not like '王%';
-- 36. 查询student表中每个学生的姓名和年龄
-- # 解法一
-- select sname, year(curdate())-year(sbirthday) age from student;
-- # 解法二
-- select sname, year(now())-year(sbirthday) age from student;
-- 37. 查询student表中最大和最小的sbirthday日期值
-- select sname,max(sbirthday) birthday from student
-- where sbirthday in (
-- select max(sbirthday) from student )
-- union
-- select sname,min(sbirthday) birthday from student
-- where sbirthday in (
-- select min(sbirthday) from student );
-- 38. 以班号和年龄从大到小的顺序查询student表中的全部记录
-- select * from student 
-- order by class desc,
-- year(now())-year(sbirthday()) desc;
-- 39. 查询"男"教师及其所上的课程
-- select a.tname,b.cname from teacher a 
-- join course b
-- on a.tno = b.tno
-- where a.tsex = '男';
-- 40. 查询和“李军”同性别并同班的同学sname
-- select sname from student where ssex in (
-- select ssex from student where sname = '李军')
-- and class in (
-- select class from student where sname = '李军')
-- and sname != '李军';
-- 41. 查询所有选修“计算机导论”课程的“男”同学的成绩表
-- # 解法一
-- select a.* from score a join (course b,student c)
-- on a.cno = b.cno and a.sno = c.sno
-- where c.ssex = '男' and a.cno in (
-- select cno from course where cname = '计算机导论'
-- );
-- # 解法二
-- select a.* from score a join (course b,student c)
-- using (sno,cno)
-- where c.ssex = '男' and b.cname = '计算机导论';







```



## SQL表问题

1. table里可以有多个主键吗？

   数据库的每张表只能**有**一个**主键**，不可能**有多个主键**. 

   **Every table can have (but does not have to have) a primary key**. The column or columns defined as the primary key ensure uniqueness in the table; no two rows can have the same key. The primary key of one table may also help to identify records in other tables, and be part of the second table's primary key.

2. table里可以有重复行吗？

   没有主键就可以有重复记录了，方便恢复记录。一旦有了主键，必须主键列无重复值。 

3. 什么是联合主键？

   所谓的复合主键 就是指你表的主键含有一个以上的字段组成,不使用无业务含义的自增id作为主键。

   ```sql
   create table test 
   ( 
      name varchar(19), 
      id number, 
      value varchar(10), 
      primary key (name,id) 
   )
   ```

   

## SQL基本操作

### SQL SELECT 语句

SELECT 语句用于从数据库中选取数据。

结果被存储在一个结果表中，称为结果集。

### SQL SELECT DISTINCT 语句

在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。

DISTINCT 关键词用于返回唯一不同的值

### SQL WHERE 子句

WHERE 子句用于提取那些满足指定条件的记录。

### SQL AND & OR 运算符

如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

### SQL ORDER BY 关键字

ORDER BY 关键字用于对结果集按照一个列或者多个列进行排序。

ORDER BY 关键字默认按照升序对记录进行排序。如果需要按照降序对记录进行排序，您可以使用 DESC 关键字。

```sql
SELECT column_name,column_name
FROM table_name
ORDER BY column_name,column_name ASC|DESC;
```

### SQL INSERT INTO 语法

INSERT INTO 语句可以有两种编写形式。

第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：

```sql
INSERT INTO *table_name*
VALUES (*value1*,*value2*,*value3*,...);
```



第二种形式需要指定列名及被插入的值：

```sql
INSERT INTO *table_name* (*column1*,*column2*,*column3*,...)
VALUES (*value1*,*value2*,*value3*,...);
```

### SQL UPDATE 语句

UPDATE 语句用于更新表中已存在的记录。

```sql
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```

### SQL DELETE 语句

DELETE 语句用于删除表中的行。

```sql
DELETE FROM table_name
WHERE some_column=some_value;
```

### SQL SELECT  LIMIT

SELECT TOP 子句用于规定要返回的记录的数目。LIMIT

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

### SQL LIKE 操作符

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern
```

### SQL 通配符

在 SQL 中，通配符与 SQL LIKE 操作符一起使用。

SQL 通配符用于搜索表中的数据。

在 SQL 中，可使用以下通配符：

| 通配符                         | 描述                       |
| :----------------------------- | :------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |



```sql
SELECT * FROM Websites
WHERE url LIKE 'https%';

SELECT * FROM Websites
WHERE url LIKE '%oo%';

SELECT * FROM Websites
WHERE name LIKE '_oogle';

SELECT * FROM Websites
WHERE name LIKE 'G_o_le';
```

### IN 操作符

IN 操作符允许您在 WHERE 子句中规定多个值。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);
```

### SQL BETWEEN 操作符

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

### SQL 别名

通过使用 SQL，可以为表名称或列名称指定别名。

基本上，创建别名是为了让列名称的可读性更强。

```sql
-- 列的 SQL 别名语法
SELECT column_name AS alias_name
FROM table_name;

SELECT name AS n, country AS c
FROM Websites;

-- 表的 SQL 别名语法
SELECT column_name(s)
FROM table_name AS alias_name;

SELECT w.name, w.url, a.count, a.date
FROM Websites AS w, access_log AS a
WHERE a.site_id=w.id and w.name="菜鸟教程";
```



### SQL 连接(JOIN)

SQL JOIN 子句用于把来自两个或多个表的行结合起来，基于这些表之间的共同字段。

最常见的 JOIN 类型：**SQL INNER JOIN（简单的 JOIN）**。 SQL INNER JOIN 从多个表中返回满足 JOIN 条件的所有行

<img src="https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png" alt="img" style="zoom:67%;" />

```sql
SELECT Websites.id, Websites.name, access_log.count, access_log.date
FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id;
```

如果加修饰符，默认join是inner join

### SQL UNION 操作符

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。UNION把行合并在一起， JOIN把列合并在一起.

请注意，UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

**注释：**UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

### SQL SELECT INTO 语句

SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中。 *MySQL 数据库不支持 SELECT ... INTO 语句，但支持* [INSERT INTO ... SELECT](https://www.runoob.com/sql/sql-insert-into-select.html) *。*

```sql
-- mysql
CREATE TABLE 新表
AS
SELECT * FROM 旧表 
```



### SQL INSERT INTO SELECT 语句

INSERT INTO SELECT 语句从一个表复制数据，然后把数据插入到一个已存在的表中。目标表中任何已存在的行都不会受影响。

```sql
INSERT INTO table2
SELECT * FROM table1;

INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
```



### SQL 约束（Constraints）

SQL 约束用于规定表中的数据规则。

如果存在违反约束的数据行为，行为会被约束终止。

约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

##### SQL CREATE TABLE + CONSTRAINT 语法

CREATE TABLE *table_name*
(
*column_name1 data_type*(*size*) *constraint_name*,
*column_name2 data_type*(*size*) *constraint_name*,
*column_name3 data_type*(*size*) *constraint_name*,
....
);

在 SQL 中，我们有如下约束：

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- **CHECK** - 保证列中的值符合指定的条件。
- **DEFAULT** - 规定没有给列赋值时的默认值。





### SQL NOT NULL 约束

NOT NULL 约束强制列不接受 NULL 值。

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);

-- 添加 NOT NULL 约束
ALTER TABLE Persons
MODIFY Age int NOT NULL;

-- 删除 NOT NULL 约束
ALTER TABLE Persons
MODIFY Age int NULL;
```



### SQL UNIQUE 约束

UNIQUE 约束唯一标识数据库表中的每条记录。

UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。

PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。

请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (P_Id)
)

-- ALTER TABLE Persons
ADD UNIQUE (P_Id)

-- 撤销 UNIQUE 约束
ALTER TABLE Persons
DROP INDEX uc_PersonID
```

### SQL PRIMARY KEY 约束

PRIMARY KEY 约束唯一标识数据库表中的每条记录。

主键必须包含唯一的值。

主键列不能包含 NULL 值。

每个表都应该有一个主键，并且每个表只能有一个主键。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (P_Id)
)

-- ALTER TABLE 时的 SQL PRIMARY KEY 约束
ALTER TABLE Persons
ADD PRIMARY KEY (P_Id)

ALTER TABLE Persons
DROP PRIMARY KEY
```



### SQL FOREIGN KEY 约束

一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。

```sql
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
)

-- ALTER TABLE 时的 SQL FOREIGN KEY 约束
ALTER TABLE Orders
ADD FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)

ALTER TABLE Orders
DROP FOREIGN KEY fk_PerOrders
```



### SQL CHECK 约束

CHECK 约束用于限制列中的值的范围。

如果对单个列定义 CHECK 约束，那么该列只允许特定的值。

如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)

-- ALTER TABLE 时的 SQL CHECK 约束
ALTER TABLE Persons
ADD CHECK (P_Id>0)

ALTER TABLE Persons
DROP CONSTRAINT chk_Person
```



### SQL DEFAULT 约束

DEFAULT 约束用于向列中插入默认值。

如果没有规定其他的值，那么会将默认值添加到所有的新记录。

```sql
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)

-- ALTER TABLE 时的 SQL DEFAULT 约束
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES'

ALTER TABLE Persons
ALTER City DROP DEFAULT
```

### SQL CREATE INDEX 语句

CREATE INDEX 语句用于在表中创建索引。

用户无法看到索引，它们只能被用来加速搜索/查询。

**注释：**更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

```sql
CREATE INDEX index_name
ON table_name (column_name)

CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

### SQL DROP 语句

```sql
DROP INDEX index_name ON table_name

DROP TABLE table_name

DROP DATABASE database_name

-- 删除表内的数据，但并不删除表本身
TRUNCATE TABLE table_name
```



### SQL ALTER TABLE 语句

ALTER TABLE 语句用于在已有的表中添加、删除或修改列。

```sql
ALTER TABLE table_name
ADD column_name datatype

ALTER TABLE table_name
DROP COLUMN column_name

ALTER TABLE table_name
MODIFY COLUMN column_name datatype
```

### SQL Date 函数

**MySQL** 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS
- YEAR - 格式：YYYY 或 YY

MySQL 中最重要的内建日期函数：

| [NOW()](https://www.runoob.com/sql/func-now.html)            | 返回当前的日期和时间                |
| ------------------------------------------------------------ | ----------------------------------- |
| [CURDATE()](https://www.runoob.com/sql/func-curdate.html)    | 返回当前的日期                      |
| [CURTIME()](https://www.runoob.com/sql/func-curtime.html)    | 返回当前的时间                      |
| [DATE()](https://www.runoob.com/sql/func-date.html)          | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.runoob.com/sql/func-extract.html)    | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.runoob.com/sql/func-date-add.html)  | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.runoob.com/sql/func-date-sub.html)  | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.runoob.com/sql/func-date-format.html) | 用不同的格式显示日期/时间           |

```sql
SELECT * FROM Orders WHERE OrderDate='2008-11-11'

SELECT * FROM Orders WHERE OrderDate='2008-11-11 00：00：00'

-- Datetime 如果没有时间部分，默认时间为 00:00:00。
```



## SQL 函数

### AVG() 函数

AVG() 函数返回数值列的平均值。

```sql
SELECT AVG(column_name) FROM table_name
```

### COUNT() 函数

COUNT() 函数返回匹配指定条件的行数。

```sql
-- COUNT(column_name) 函数返回指定列的值的数目（NULL 不计入）：
SELECT COUNT(column_name) FROM table_name;

-- COUNT(*) 函数返回表中的记录数：
SELECT COUNT(*) FROM table_name;
```



### FIRST() 函数

FIRST() 函数返回指定的列中第一个记录的值。(mysql 不支持)

```sql
-- mysql实现
SELECT column_name FROM table_name
ORDER BY column_name ASC
LIMIT 1;
```



### LAST() 函数

```sql
-- mysql实现
SELECT column_name FROM table_name
ORDER BY column_name DESC
LIMIT 1;
```



### MAX() 函数

MAX() 函数返回指定列的最大值。

```sql
SELECT MAX(column_name) FROM table_name;
```



### MIN() 函数

MIN() 函数返回指定列的最小值。

```sql
SELECT MIN(column_name) FROM table_name;
```



### SUM() 函数

SUM() 函数返回数值列的总数。

```sql
SELECT SUM(column_name) FROM table_name;
```



### GROUP BY 语句

GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```



### HAVING 子句

在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。

HAVING 子句可以让我们筛选分组后的各组数据。

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

Having 和 where区别？

“Where” 是一个约束声明，使用Where来约束来之数据库的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。 “**Having**”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在**Having**中可以使用聚合函数

### EXISTS 运算符

EXISTS 运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回 True，否则返回 False。

```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```



### LEN() 函数

LEN() 函数返回文本字段中值的长度。

```sql
SELECT LENGTH(column_name) FROM table_name;
```

### ROUND() 函数

ROUND() 函数用于把数值字段舍入为指定的小数位数。

```sql
SELECT ROUND(column_name,decimals) FROM table_name;
```

### NOW() 函数

NOW() 函数返回当前系统的日期和时间。

```sql
SELECT NOW() FROM table_name;
```







## SQL 通用数据类型

| CHARACTER(n)                       | 字符/字符串。固定长度 n。                                    |
| ---------------------------------- | ------------------------------------------------------------ |
| VARCHAR(n) 或 CHARACTER VARYING(n) | 字符/字符串。可变长度。最大长度 n。                          |
| BINARY(n)                          | 二进制串。固定长度 n。                                       |
| BOOLEAN                            | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或 BINARY VARYING(n)  | 二进制串。可变长度。最大长度 n。                             |
| INTEGER(p)                         | 整数值（没有小数点）。精度 p。                               |
| SMALLINT                           | 整数值（没有小数点）。精度 5。                               |
| INTEGER                            | 整数值（没有小数点）。精度 10。                              |
| BIGINT                             | 整数值（没有小数点）。精度 19。                              |
| DECIMAL(p,s)                       | 精确数值，精度 p，小数点后位数 s。例如：decimal(5,2) 是一个小数点前有 3 位数，小数点后有 2 位数的数字。 |
| NUMERIC(p,s)                       | 精确数值，精度 p，小数点后位数 s。（与 DECIMAL 相同）        |
| FLOAT(p)                           | 近似数值，尾数精度 p。一个采用以 10 为基数的指数计数法的浮点数。该类型的 size 参数由一个指定最小精度的单一数字组成。 |
| REAL                               | 近似数值，尾数精度 7。                                       |
| FLOAT                              | 近似数值，尾数精度 16。                                      |
| DOUBLE PRECISION                   | 近似数值，尾数精度 16。                                      |
| DATE                               | 存储年、月、日的值。                                         |
| TIME                               | 存储小时、分、秒的值。                                       |
| TIMESTAMP                          | 存储年、月、日、小时、分、秒的值。                           |
| INTERVAL                           | 由一些整数字段组成，代表一段时间，取决于区间的类型。         |
| ARRAY                              | 元素的固定长度的有序集合                                     |
| MULTISET                           | 元素的可变长度的无序集合                                     |
| XML                                | 存储 XML 数据                                                |



# Lecture 11 JDBC & Hiebernate

## JDBC

JDBC (java database connectivity) Driver : software component that enables java applications to interact with the database.

### steps for building JDBC -> database

1. allocate 分配a connection object
2. allocate a statement
3. write sql quey and execute the query 
4. process the query result
5. close the statement, connection object

```java
Connection conn = DriverManager.getConnection( url,
username,
passwored );
Statement stmt = conn.createStatement(); ResultSet resultSet = stmt.executeQuery(strSelect);

//don't forget to close the connection and statement
```

### Try with resources

The try -with-resources statement is a try **statement that declares one or more resources**. A resource is an object that must be closed after the program is finished with it. The try -with-resources statement ensures that each resource is closed at the end of the statement. Any object that implements java

可以理解为是一个声明一个或多个资源的 try语句（用分号隔开），
 一个资源作为一个对象，并且这个资源必须要在执行完关闭的，
 try-with-resources语句确保在语句执行完毕后，每个资源都被自动关闭 。
 任何实现了** java.lang.AutoCloseable**的对象, 包括所有实现了 **java.io.Closeable** 的对象

```java
try(
Connection conn = DriverManager.getConnection( JdbcConfig.getUrl(),
JdbcConfig.getUser(), JdbcConfig.getPassword()
);
Statement stmt = conn.createStatement(); ){
} catch (E) { }

```



### Atomic Transaction (commit and rollback)

 an atomic transaction is a group of sql statement, which either all succeed or non succeeds. This prevents the partial update to the database



### Manage trascition in JDBC

 first disable the default auto-commit

```java
conn.setAutoCommit(false);
```

then issue a commit() to commit all the changes or rollback() to discard all the changes since the last commit

```java
  // update something
 stmt.executeUpdate("update books set qty = qty + 1 where title = 'Data'");
 stmt.executeUpdate("update books set qty = qty + 1 where title = 'Math'");
 conn.commit();

 // update but rollback
 stmt.executeUpdate("update books set qty = qty + 1000 where title = 'Data'");
 stmt.executeUpdate("update books set qty = qty + 1000 where title = 'Math'");
 conn.rollback();

//rollback in try and catch block
try {
                conn.setAutoCommit(false);
                // insert two statements
                stmt.executeUpdate("insert into books values ('AWS', 98, 12)");
                // duplicate primary key, which will trigger a SQLException
                stmt.executeUpdate("insert into books values ('AWS', 90, 15)");
                conn.commit();
            } catch (SQLException throwables) {
                System.out.println("rolling back changes");
                conn.rollback();
                throwables.printStackTrace();
            }
```



### resultSetMetaData

ResultSet object is associated with a header (called meta data), which contains information about the resultset object

```java
 ResultSet rset = stmt.executeQuery("select * from books");
 ResultSetMetaData rsetMD = rset.getMetaData();
 int numColumns = rsetMD.getColumnCount();
```



### sql injection?

黑客手段 在sql语句中注入查询语句，获得database信息 -Retriving hidden data

SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

How to prevent?

Most instances of SQL injection can be prevented by using parameterized queries (also known as prepared statements) instead of string concatenation within the query.

只能插入特定值，不能自己构造语句。

### PreparedStatement

• allows you to pass parameters into a sql statement

 • pre-complied

 • execute the same sql statement multiple times

```java
PreparedStatement pstmt = conn.prepareStatement(
                        "insert into books values (?, ?, ?)"
                );
```

### batch processing

- each statement can be added into the batch via .addBatch() method
- execute the batch by executeBatch()

```java
conn.setAutoCommit(false);

            pstmt.setString(1, "Go");
            pstmt.setInt(2, 123);
            pstmt.setInt(3, 321);
            pstmt.addBatch();

            pstmt.setString(1, "Json");
            pstmt.setInt(2, 345);
            pstmt.addBatch();

```



## Hiebernate

connect the java program to sql database, based on JDBC, slower but easier to use.



### ORM (object relational mapping )

is the approach of taking object-oriented data and mapping to a relational data store (e.g. tables in an RDBMS)

Hibernate: -->. easy orm 

Sequelize. ->. good for node.js 

SQLAlchemy

MyBatis .....



### JPA: Java persistence API

defines the management of relational data in java application

JPA is the specification, Hibernate is the implementation using ORM techniques.

```java
/* Hibernate vs ORM vs JPA

1. ORM is the approach of taking object-oriented data and mapping to a relational data store (e.g. tables in an RDBMS)

2. Hibernate is an implementation of JPA and uses ORM techniques.

3. JPA is the EE standard specification for ORM in Java EE.

4. The reference implementation for JPA is EclipseLink. If you don't explictly configure a provider, EclipseLink is used under the covers.

5. Hibernate is another implementation of the JPA specification, in that you can use the standard JPA APIs and configure your application to use Hibernate as the provider of the spec under the covers.

6.Hibernate also provides a superset of the ORM features beyond what is specified in the JPA spec. Meaning, that while it provides an implementation of the JPA API, it also provides more features beyond what JPA specifies.
```







### First Level Cache vs Second Level Cache (必考)

**First**: **open by default, session level, locally, if session closed, cache is gone**

save、update、saveOrupdate、load、list、iterate、lock会向一级缓存存放数据；

每次查都先看看一级缓存里是否存在要查询的记录，如果没有再去数据库查询。可以通过在一个session中查询两次相同数据验证。

![18](/Users/spikycrown/Desktop/Java2021/images/18.png)

**Second**: **close by default,** session factory level, globally, if seesion factory shut down, cache is gone

Second 现在基本不用， 被redis代替。

流程： Query -> first level cache -> second level cache (need open mannually)-> database

什么样的数据适合二级缓存？

\1) 很少被修改的数据 

\2) 不是很重要的数据，允许出现偶尔并发的数据 

\3) 不会被并发访问的数据 

\4) 参考数据,指的是供应用参考的常量数据，它的实例数目有限，它的实例会被许多其他类的实例引用，实例极少或者从来不会被修改。





### Mapping (必考)

one to one 

one to many

many to many 需要中间表

Fetch type : lazy loading, eager loading;



### Fetch Type (必考)

Lazy: 不与child table 联动

Eager: 与child table联动



### Cascade Type 级联 (必考)

Entity relationships often depend on the existence of another entity, for example the *Person*–*Address* relationship. Without the *Person*, the *Address* entity doesn't have any meaning of its own. When we delete the *Person* entity, our *Address* entity should also get deleted.

Cascading is the way to achieve this. **When we perform some action on the target entity, the same action will be applied to the associated entity.**

#### JPA Cascade Type

- *ALL*: **propagates all operations — including Hibernate-specific ones — from a parent to a child entity.**

  包含所有持久化方法

  

- *PERSIST*: The persist operation makes a transient instance persistent. **Cascade Type \*PERSIST\* propagates the persist operation from a parent to a child entity.** 

  获取A对象里也同时也重新获取最新的B时的对象。即会重新查询数据库里的最新数据，并且，只有A类新增时，会级联B对象新增。若B对象在数据库存（跟新）在则抛异常（让B变为持久态），对应EntityManager的presist方法,调用JPA规范中的persist()，不适用于Hibernate的save()方法
  
- *MERGE*: The merge operation copies the state of the given object onto the persistent object with the same identifier. ***CascadeType.MERGE\* propagates the merge operation from a parent to a child entity**

  指A类新增或者变化，会级联B对象（新增或者变化） ，对应EntityManager的merge方法，调用JPA规范中merge()时，不适用于Hibernate的update()方法

  

- *REMOVE*: as the name suggests, the remove operation removes the row corresponding to the entity from the database and also from the persistent context.

  ***CascadeType.REMOVE\* propagates the remove operation from parent to child entity.** **Similar to JPA's \*CascadeType.REMOVE\*, we have \*CascadeType.DELETE\*, which is specific to Hibernate.** There is no difference between the two.

  只有A类删除时，会级联删除B类,即在设置的那一端进行删除时，另一端才会级联删除，对应EntityManager的remove方法，调用JPA规范中的remove()时，适用于Hibernate的delete()方法

  

- *REFRESH*: Refresh operations **reread the value of a given instance from the database.** In some cases, we may change an instance after persisting in the database, but later we need to undo those changes.

  In that kind of scenario, this may be useful. **When we use this operation with Cascade Type \*REFRESH\*, the child entity also gets reloaded from the database whenever the parent entity is refreshed.**

  获取order（一或多）对象里也同时也重新获取最新的items（多）的对象，对应EntityManager的refresh(object)，调用JPA规范中的refresh()时，适用于Hibernate的flush()方法

  

- *DETACH*:The detach operation removes the entity from the persistent context. **When we use \*CascadeType.DETACH\*, the child entity will also get removed from the persistent context.**



#### Hibernate Cascade Type

Hibernate supports three additional Cascade Types along with those specified by JPA. 

- *REPLICATE*: **The replicate operation is used when we have more than one data source and we want the data in sync.** With *CascadeType.REPLICATE*, a sync operation also propagates to child entities whenever performed on the parent entity.
- *SAVE_UPDATE*: *CascadeType.SAVE_UPDATE* propagates the same operation to the associated child entity. It's useful when we use **Hibernate-specific operations like \*save\*, \*update\* and \*saveOrUpdate\*.** 
- *LOCK*:**Unintuitively, \*CascadeType.LOCK\* reattaches the entity and its associated child entity with the persistent context again**



## JDBC vs Hibernate

**1、JDBC**

  我们平时使用jdbc进行编程，大致需要下面几个步骤：
  1、使用jdbc编程需要连接数据库，注册驱动和数据库信息；
  2、操作Connection，打开Statement对象；
  3、通过Statement对象执行SQL，返回结果到ResultSet对象；
  4、使用ResultSet读取数据，然后通过代码转化为具体的POJO对象；
  5、关闭数据库相关的资源。

 Jdbc的缺点：
   1、工作量比较大，需要连接，然后处理jdbc底层事务，处理数据类型，还需要操作Connection，Statement对象和ResultSet对象去拿数据并关闭他们。
   2、我们对jdbc编程可能产生的异常进行捕捉处理并正确关闭资源。

  由于JDBC存在的缺陷，在实际工作中我们很少直接使用jdbc进行编程，用的更多的是ORM对象关系模型来操作数据库，Hibernate就是一个ORM模型。



**2、Hibernate**

​    Hibernate是建立在若干POJO通过xml映射文件（或注解）提供的规则映射到数据库表上的。我们可以通过POJO直接操作数据库的数据，他提供的是一种全表映射的模型。相对而言，Hibernate对JDBC的封装程度还是比较高的，我们已经不需要写SQL，只要使用HQL语言就可以了。

  Hibernate的优点：
  1、消除了代码的映射规则，它全部分离到了xml或者注解里面去配置。
  2、无需在管理数据库连接，它也配置到xml里面了。
  3、一个会话中不需要操作多个对象，只需要操作Session对象。
  4、关闭资源只需要关闭一个Session便可。

  这就是Hibernate的优势，在配置了映射文件和数据库连接文件后，Hibernate就可以通过Session操作，非常容易，消除了jdbc带来的大量代码，大大提高了编程的简易性和可读性。Hibernate还提供了级联，缓存，映射，一对多等功能。Hibernate是全表映射，通过HQL去操作pojo进而操作数据库的数据。

  Hibernate的缺点：
  1，全表映射带来的不便，比如更新时需要发送所有的字段。
  2，无法根据不同的条件组装不同的SQL。
  3，对多表关联和复杂的sql查询支持较差，需要自己写sql，返回后，需要自己将数据封装为pojo。
  4，不能有效的支持存储过程。
  5，虽然有HQL，但是性能较差，大型互联网系统往往需要优化sql，而hibernate做不到。

**3、Mybatis**
  为了解决Hibernate的不足，Mybatis出现了，Mybatis是半自动的框架。之所以称它为半自动，是因为它需要手工匹配提供POJO，sql和映射关系，而全表映射的Hibernate只需要提供pojo和映射关系即可。
  Mybatis需要提供的映射文件包含了一下三个部分：sql，映射规则，pojo。在Mybatis里面你需要自己编写sql，虽然比Hibernate配置多，但是Mybatis可以配置动态sql，解决了hibernate表名根据时间变化，
  不同条件下列不一样的问题，同时你也可以对sql进行优化，通过配置决定你的sql映射规则，也能支持存储过程，所以对于一些复杂和需要优化性能的sql查询它就更加方便。Mybatis几乎可以做到jdbc所有能做到的事情。


**4、什么时候使用Hibernate，Mybatis？**

  Hibernate作为留下的Java orm框架，它确实编程简易，需要我们提供映射的规则，完全可以通过IDE生成，同时无需编写sql确实开发效率优于Mybatis。此外Hibernate还提供了缓存，日志，级联等强大的功能，
  但是Hibernate的缺陷也是十分明显，多表关联复杂sql，数据系统权限限制，根据条件变化的sql，存储过程等场景使用Hibernate十分不方便，而性能又难以通过sql优化，所以注定了Hibernate只适用于在场景不太复杂，要求性能不太苛刻的时候使用。
  如果你需要一个灵活的，可以动态生成映射关系的框架，那么Mybatis确实是一个最好的选择。它几乎可以替代jdbc，拥有动态列，动态表名，存储过程支持，同时提供了简易的缓存，日志，级联。
  但是它的缺陷是需要你提供映射规则和sql，所以开发工作量比hibernate要大些。

**5、Jdbc,Mybatis,Hibernate的区别**
1）从层次上看，JDBC是较底层的持久层操作方式，而Hibernate和MyBatis都是在JDBC的基础上进行了封装使其更加方便程序员对持久层的操作。
2）从功能上看，JDBC就是简单的建立数据库连接，然后创建statement，将sql语句传给statement去执行，如果是有返回结果的查询语句，会将查询结果放到ResultSet对象中，通过对ResultSet对象的遍历操作来获取数据；Hibernate是将数据库中的数据表映射为持久层的Java对象，对sql语句进行修改和优化比较困难；MyBatis是将sql语句中的输入参数和输出参数映射为java对象，sql修改和优化比较方便。

3）从使用上看，如果进行底层编程，而且对性能要求极高的话，应该采用JDBC的方式；如果要对数据库进行完整性控制的话建议使用Hibernate；如果要灵活使用sql语句的话建议采用MyBatis框架。

# Lecture 12 Design Pattern & Reflection

## Design Pattern

Total 23 design patterns.

### SOLID原则

**Single Responsibility Principle**

一个类应该只有一个发生变化的原因。

**Open Closed Principle** **开闭原则**

一个软件实体，如类、模块和函数应该**对扩展开放，对修改关闭**。

**Liskov Substitution Principle**

所有引用基类的地方必须能透明地使用其子类的对象。

**Interface Segregation Principle**

1、客户端不应该依赖它不需要的接口。
2、类间的依赖关系应该建立在最小的接口上。

**Dependence Inversion Principle**

1、上层模块不应该依赖底层模块，它们都应该依赖于抽象。
2、抽象不应该依赖于细节，细节应该依赖于抽象。



### creational patterns: hiding the creation logic

对类的实例化过程进行了抽象，能够将软件模块中**对象的创建**和对象的使用分离。

○ Singleton Pattern
○ Factory Pattern
○ Builder Pattern
○ Prototype Pattern
○ ....

###  structural patterns: concern class and object compositions

关注于对象的组成以及对象之间的依赖关系，描述如何将类或者对象结合在一起形成更大的结构，就像**搭积木**，可以通过简单积木的组合形成复杂的、功能更为强大的结构。

○ Proxy Pattern (static / dynamic)
○ Adapter Pattern
○ Decorator Pattern
○ Bridge Pattern
○ .....

### behavioral patterns: concerned with communication between objects

关注于对象的行为问题，是对在不同的对象之间划分责任和算法的抽象化；不仅仅关注类和对象的结构，而且重点关注它们之间的**相互作用**

○ Observer Pattern
○ Interpreter Pattern
○ Iterator Pattern
○



### Singleton Design pattern（优缺点？）

• only one single object been created

Usage for Singleton
• logger
• drivers objects
• caching
• thread pool

```java
//A: eager loading- create instance when load class
//B: lazy loading - create instace when class first called (not thread safe)
//C: sychornaze getInstace() method, thread safe, but slow.  why sychronaze is slow?
//D: check if instance is exist or not, if not then sychornaze the class. Also use volatile for the instance () to make the implement of the creating new instance in order.

public class SingletonD {
	private static volatile SingletonD instance= null; //voltile keyword can make sure the instace is created before it is referenced & return
	private SingletonD(){}

    public static SingletonD getInstance(){
		if(instance == null){
		synchronized(SingletonD.class){
			if(instance == null){
				instance = new SingletonD();
				}
			}
		}
	return instance;
	}
}

/* instance == new new SingletonD();
1. allocate memory space
2. instaniate the new sington object
3. point to the new object, after this step the instace will not be null;
 Normally: 1->2->3
 Possible: 1->3 instance is not null->2
 use voltile can prevent CPU instruction reorder and to aviod the possible situation.
 */


```

```
保证一个类仅有一个实例，并提供一个访问它的全局访问点。
优点：
         a.在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例，
         b.避免对资源的多重占用（比如写文件操作）
      缺点：
         a.与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化；
      使用场景：
         a.全局唯一id、web页面计数器
```

### Builder

builder is a creational design pattern that lets you construct complex objects step by step

Instead of declare different type of constructors, the builder pattern can build the object with input features.

```
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
优点：
         a.建造者独立，易扩展
         b.便于控制细节风险
      缺点：
         a.产品必须有共同点，范围有限制。
         b.如内部变化复杂，会有很多的建造类。
 使用场景：
 需要生成的产品对象有复杂的内部结构，这些产品对象通常包含多个成员属性。
需要生成的产品对象的属性相互依赖，需要指定其生成顺序。
隔离复杂对象的创建和使用，并使得相同的创建过程可以创建不同的产品。
```

### Factory Design Pattern 

``

创建好产品/类，根据需求调用， factory类提供接口，由client决定需要什么。

```
定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂方法使一个类的实例化延迟到其子类。
优点：
         a.一个调用者想创建一个对象，只要知道其名称就可以了
         b.扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以
         c.屏蔽产品的具体实现，调用者只关心产品的接口
      缺点：
         a.每次增加一个产品时，都需要增加一个具体类和修改对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。
         
使用场景：
1、您需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。 2、Hibernate 换数据库只需换方言和驱动就可以。
```

### Proxy Design (Static&Dynamic) pattern: 

• provide substitute for another object 

• control the access to the original objects

• perform some operations before and after request/response

```
Static:提前创建好，client直接用
Dynamic: Dynamic proxy is essentially the proxy design pattern, in which the proxy object is created dynamically during runtime.在运行时根据client输入的method call来创建代理，此处用到InvocationHandler.


为其他对象提供一种代理以控制对这个对象的访问

优点：1）代理模式能将代理对象与真正被调用的对象分离，在一定程度上降低了系统的耦合度。2）代理模式在客户端和目标对象之间起到一个中介作用，这样可以起到保护目标对象的作用。代理对象也可以对目标对象调用之前进行其他操作。

缺点：1）在客户端和目标对象增加一个代理对象，会造成请求处理速度变慢。2）增加了系统的复杂度


使用场景：

远程代理: 为一个位于不同的地址空间的对象提供一个本地 的代理对象，这个不同的地址空间可以是在同一台主机中，也可是在 另一台主机中
安全代理: 控制对一个对象的访问，可以给不同的用户提供不同级别的使用权限
智能代理: 当一个对象被引用时，提供一些额外的操作，如将此对象被调用的次数记录下来等
虚拟代理: 如果需要创建一个资源消耗较大的对象，先创建一个消耗相对较小的对象来表示，真实对象只在需要时才会被真正创建。
```



## Reflection

Reflection is an API which is used to examine or modify the behavior of methods, classes, interfaces
at the run time. (是不是不利于encapsulation)

Java反射机制主要提供了以下功能：

l 在运行时判断任意一个对象所属的类 - Class；

l 在运行时构造任意一个类的对象；

l 在运行时判断任意一个类所具有的成员变量和方法 - Field and Method；

l 在运行时调用任意一个对象的方法；

l 生成动态代理。

常用方法：

```java
Class 常用反射API
  
getClass():获得类
getName()：获得类的完整名字。
getFields()：获得类的public类型的属性。
getDeclaredFields()：获得类的所有属性。
getMethods()：获得类的public类型的方法。
getDeclaredMethods()：获得类的所有方法。
getMethod(String name, Class[] parameterTypes)：获得类的特定方法，name参数指定方法的名字，parameterTypes参数指定方法的参数类型。
getConstrutors()：获得类的public类型的构造方法。
getConstrutor(Class[] parameterTypes)：获得类的特定构造方法，parameterTypes参数指定构造方法的参数类型。
newInstance()：通过类的不带参数的构造方法创建这个类的一个对象。

Method常用反射API
  
exp: Method methodcall1 = cls.getDeclaredMethod("method2", int.class);
  
1.Method.invoke()，方法自己调用自己，方法调用必须通过object.method()方式，method对象本身是无法调用自己的。

2.Method.getParameterTypes()获得参数类型

3.Method.getReturnType()获得返回值类型

4.Method.getParameterCount()获得方法的参数个数

5.Method.getName()获得方法名称

6.Method.getExceptionTypes()获得方法抛出哪些异常

7.method.getAnnotation()获得注解
  
Field常用反射API

1.field.getAnnotations()返回属性的注解
```



```java
package com.antra.reflection;

// A simple Java program to demonstrate the use of reflection
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
import java.lang.reflect.Field;
import java.lang.reflect.Constructor;

// class whose object is to be created
class Test {
    // creating a private field
    private String s;

    // creating a public constructor
    public Test()  {  s = "123456789"; }

    // Creating a public method with no arguments
    public void method1()  {
        System.out.println("The string is " + s);
    }

    // Creating a public method with int as argument
    public void method2(int n)  {
        System.out.println("The number is " + n);
    }

    // creating a private method
    private void method3() {
        System.out.println("Private method invoked");
    }


    @Deprecated
    public void method4() {
        System.out.println("in method4 of test class");
    }
}


public class ReflectionDemo {

    public static void main(String args[]) throws Exception
    {
        // Creating object whose property is to be checked
        Test obj = new Test();

        // Creating class object from the object using
        // getclass method
        Class cls = obj.getClass();
        System.out.println("The name of class is " +
                cls.getName()); //Output:The name of class is com.antra.reflection.Test

        // Getting the constructor of the class through the
        // object of the class
        Constructor constructor = cls.getConstructor();
        System.out.println("The name of constructor is " +
                constructor.getName()); //Output:The name of constructor is com.antra.reflection.Test

        System.out.println("The public methods of class are : ");

        // Getting methods of the class through the object
        // of the class by using getMethods
        Method[] methods = cls.getMethods(); // public method + object method
        //Method[] methods = cls.getDeclaredMethods(); //public method + private method
        System.out.println("-------------------");
        // Printing method names 
        for (Method method:methods)
            System.out.println(method.getName());

        System.out.println("-------------------");


        // creates object of desired method by providing the
        // method name and parameter class as arguments to
        // the getDeclaredMethod
        obj.method1();    /* Output: The string is 123456789 */
      
        Method methodcall1 = cls.getDeclaredMethod("method2",
                int.class);

        // invokes the method at runtime
        methodcall1.invoke(obj, 19);    /* Output: The number is 19 */

        // creates object of the desired field by providing
        // the name of field as argument to the
        // getDeclaredField method
        Field field = cls.getDeclaredField("s");

        // allows the object to access the field irrespective
        // of the access specifier used with the field
        field.setAccessible(true);

        // takes object and the new value to be assigned
        // to the field as arguments
        field.set(obj, "JAVA");    // set s as "JAVA"

        // Creates object of desired method by providing the
        // method name as argument to the getDeclaredMethod
        Method methodcall2 = cls.getDeclaredMethod("method1");

        // invokes the method at runtime
        methodcall2.invoke(obj); /* Output:	The string is JAVA	*/

        // Creates object of the desired method by providing
        // the name of method as argument to the
        // getDeclaredMethod method
        Method methodcall3 = cls.getDeclaredMethod("method3");

        // allows the object to access the method irrespective
        // of the access specifier used with the method
        methodcall3.setAccessible(true);

        // invokes the method at runtime
        methodcall3.invoke(obj);  /* Output:	Private method invoked */

        System.out.println("-------------------");
        // get the method 4
        Method methodcall4 = cls.getDeclaredMethod("method4");
        Annotation[] annotations = methodcall4.getDeclaredAnnotations();
        for(Annotation annotation : annotations){
            System.out.println(annotation.annotationType()); // Output:interface java.lang.Deprecated

        }


    }

}

```

**normally**
•write code
•compile
•run it

**with reflection**
•write code
•compile
•run
•modify the code with reflection

### dosent-reflection-api-break-the-very-purpose-of-data-encapsulation?

Yes and no.

- Yes, some uses of the reflection API *can* break data encapsulation.
- No, not all uses of the reflection API *do* break data encapsulation. Indeed, a wise programmer only breaks encapsulation via the reflection API when there is a good reason to do so.
- No, reflection API does not change the *purpose* of data encapsulation. The *purpose* of data encapsulation remains the same ... even if it someone wilfully breaks it.

> Why do we have to use Reflection API?

There are **many** uses of reflection that **DO NOT** break encapsulation; e.g. using reflection to find out what super types a class has, what annotations it has, what members it has, to invoke accessible methods and constructors, read and update accessible fields and so on.

And there are situations where is is acceptable (to varying degrees) to use the encapsulation breaking varieties of reflection:

- You might need to look inside an encapsulated type (e.g. access / modify private fields) as the simplest way (or only way) to implement certain unit tests.
- Some forms of Dependency Injection (aka IoC), Serialization and Persistence entail accessing and/or updating private fields.
- Very occasionally, you need to break encapsulation to work around a bug in some class that you cannot fix.

# Lecture 14 HTTP & Serverlet

## HTTP 

### HTTP特点

1、支持客户/服务器模式；client/server, request/response

2、简单快速；客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

3、灵活；HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type（Content-Type是HTTP包中用来表示内容类型的标识）加以标记。

**4、无连接；**每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输

**5、无状态**。**无状态**是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。即我们给服务器发送 HTTP 请求之后，服务器根据请求，会给我们发送数据过来，但是，发送完，不会记录任何信息。

HTTP is a [stateless protocol](https://en.wikipedia.org/wiki/Stateless_protocol). A stateless protocol does not require the [HTTP server](https://en.wikipedia.org/wiki/HTTP_server) to retain information or status about each user for the duration of multiple requests. However, some [web applications](https://en.wikipedia.org/wiki/Web_application) implement states or [server side sessions](https://en.wikipedia.org/wiki/Session_(computer_science)) using for instance [HTTP cookies](https://en.wikipedia.org/wiki/HTTP_cookie) or hidden [variables](https://en.wikipedia.org/wiki/Variable_(computer_science)) within [web forms](https://en.wikipedia.org/wiki/Form_(web)).

### Protocol/ HTTPS

HTTPS 协议（HyperText Transfer Protocol over Secure Socket Layer）：一般理解为HTTP+SSL/TLS，通过 SSL证书来验证服务器的身份，并为浏览器和服务器之间的通信进行加密。

In HTTPS, the [communication protocol](https://en.wikipedia.org/wiki/Communication_protocol) is encrypted using [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) or, formerly, Secure Sockets Layer (SSL). The protocol is therefore also referred to as **HTTP over TLS**,[[3\]](https://en.wikipedia.org/wiki/HTTPS#cite_note-3) or **HTTP over SSL**.

The principal motivations for HTTPS are [authentication](https://en.wikipedia.org/wiki/Authentication) of the accessed [website](https://en.wikipedia.org/wiki/Website), and protection of the [privacy](https://en.wikipedia.org/wiki/Information_privacy) and [integrity](https://en.wikipedia.org/wiki/Data_integrity) of the exchanged data while in transit. 

### HTTP请求 (来自Browser)

- 请求方法URI协议/版本

- 请求头(Request Header)

- 请求正文

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



### HTTP响应 (来自Server)

- 状态行
- 响应头(Response Header)
- 响应正文

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



### HTTP methods

GET is used to retrieve remote data. query database.
POST 添加is used to insert remote data. update database. **POST means "create new**" as in "Here is the input for creating a user, create it for me". PUT 更新means "insert, replace if already exists" as in "Here is the data for user 5".

**Idempotency**. Idempotence is an important concept in the HTTP specification that states idempotent HTTP requests will result in the same state on the server no matter how many times that same request is executed. GET , HEAD , PUT , and DELETE all have this attribute, but **POST does not**.

一个HTTP方法是**幂等**的，指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的。换句话说就是，幂等方法不应该具有副作用（统计用途除外）。在正确实现的条件下， [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) ， [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) ， [`PUT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PUT) 和 [`DELETE`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/DELETE) 等方法都是**幂等**的，而 [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 方法不是。

### HTTP status codes

100 信息类(Information),表示收到Web浏览器请求，正在进一步的处理中 informational response

200  请求成功 Success

- 200 OK成功  The response that is returned is dependent on the request. For example, **for a GET request, the response will be included in the message body. For a PUT/POST request, the response will include the resource that contains the result of the action.**
- 201 This is the status code that confirms that the request was successful and, as a result, **a new resource was created**. Typically, this is the status code that is sent after a POST/PUT request.(200 vs 201?)
- 204 This status code confirms that the server has fulfilled the request but does not need to return information. Examples of this status code include delete requests or if a request was sent via a form and the response should not cause the form to be refreshed or for a new page to load.

300  重定向，需要进一步的操作以完成请求 Redirection

- 301：Moved Permanently，客户请求的文档在其他地方，新的URL在Location头中给出，浏览器应该自动地访问新的URL。

- 304：The is status code used for browser caching. If the response has not been modified, the client/user can continue to use the same response/cached version. For example, a browser can request if a resource has been modified since a specific time. If it hasn’t, the status code 304 is sent. If it has been modified, a status code 200 is sent, along with the resource.  Not Modified 客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告 诉客户，原来缓冲的文档还可以继续使用。

400  客户端错误，请求包含语法错误或无法完成请求 Client errors

- 404 NOT Found，意味着请求中所引用的文档不存在。The server cannot understand and process a request due to a client error. Missing data, domain validation, and invalid formatting are some examples that cause the status code 400 to be sent.
- 401  This status code request occurs when **authentication is required** but has failed or not been provided.
- 403  Very similar to status code 401, a status code 403 happens when a valid request was sent, but the server refuses to accept it. **This happens if a client/user requires the necessary permission or they may need an account to access the resource.** Unlike a status code 401, authentication will not apply here.
- 409 A status code 409 is sent when a request **conflicts with the current state of the resource**. This is usually an issue with simultaneous updates, or versions, that conflict with one another.
- 410 Resource requested is no longer available and will not be available again.

500  服务器错误，服务器在处理请求的过程中发生了错误 Server errors

- 503 

- 

  

```java
// what is HTML?
// HTML is a Language while HTTP is a Protocol.

//HTML (Hypertext Markup Language) is a language for marking the normal text so that it gets converted into hypertext. Again, not so clear. Basically, HTML tags (e.g. “<head>”, “<body>” etc.) are used to tag or mark normal text so that it becomes hypertext and several hypertext pages can be interlinked with each other resulting in the Web. 

//HTTP (Hypertext Transfer Protocol) is a protocol for transferring the hypertext pages from Web Server to Web Browser. For exchanging web pages between Server and Browser, an HTTP session is setup using protocol methods (e.g. GET, POST etc.). This would be explained in another post.


```

```java
/*从输入 URL 到页面展示到底发生了什么?

//https://zhuanlan.zhihu.com/p/133906695

1、输入地址

2、浏览器查找域名的 IP 地址 (Domain Name System -> IP)

3、浏览器向 web 服务器发送一个 HTTP 请求 (浏览器和服务器建立了TCP连接之后，发起一个http请求。)
 
4、服务器的永久重定向响应
6、服务器处理请求
7、服务器返回一个 HTTP 响应
8、浏览器显示 HTML
9、浏览器发送请求获取嵌入在 HTML 中的资源（如图片、音频、视频、CSS、JS等等）

```



## TCP	

​	Three-way handshake connection 三次握手
​		SYN -> SYN + ACK -> ACK

```java
/**第一次握手：**客户端A将标志位SYN置为1,随机产生一个值为seq=J（J的取值范围为=1234567）的数据包到服务器，客户端A进入SYN_SENT状态，等待服务端B确认；

**第二次握手：**服务端B收到数据包后由标志位SYN=1知道客户端A请求建立连接，服务端B将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给客户端A以确认连接请求，服务端B进入SYN_RCVD状态。

**第三次握手：**客户端A收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给服务端B，服务端B检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，客户端A和服务端B进入ESTABLISHED状态，完成三次握手，随后客户端A与服务端B之间可以开始传输数据了。
```

​	Four-way handshake disconnection 四次挥手
​			FIN -> FIN & ACK -> ACK

![img](https://pic4.zhimg.com/80/v2-a1956234c6575bad2c2ea5297a6fe38f_720w.jpg)

```java
/*第一次挥手： Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。

第二次挥手： Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与- SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。

第三次挥手： Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。

第四次挥手： Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手。

//.为什么建立连接是三次握手，而关闭连接却是四次挥手呢？
//这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送。
```

​	package loss/disorder 



## Java Web Application

​	Client && Server
​		request && response
​	Web Resources
​	Web Server
​	Tomcat Architecture
​		bin, conf, lib, logs, temp, webapps, work	
​	Tomcat CMD 
​		startup, shutdown, Catalina run 
​	Tomcat Version
​		related to Servlet/JSP, JavaEE, Runtime
​	Tomcat Config 
​		port 
​		config file 
​		

## Tomcat vs Servlet

Tomcat是一个免费的开放源代码的Servlet容器。

Tomcat服务器接受客户请求并做出响应的过程如下，与上图类似：

1）客户端（通常都是浏览器）访问Web服务器，发送HTTP请求。
2）Web服务器接收到请求后，传递给Servlet容器。
3）Servlet容器加载Servlet，产生Servlet实例后，向其传递表示请求和响应的对象。
4）Servlet实例使用请求对象得到客户端的请求信息，然后进行相应的处理。
5）Servlet实例将处理结果通过响应对象发送回客户端，容器负责确保响应正确送出，同时将控制返回给Web服务器。

Servlet容器也叫做Servlet引擎，是Web服务器或应用程序服务器的一部分，用于在发送的请求和响应之上提供网络服务，解码基于 MIME的请求，格式化基于MIME的响应。Servlet没有main方法，不能独立运行，它必须被部署到Servlet容器中，由容器来实例化和调用 Servlet的方法（如doGet()和doPost()），Servlet容器在Servlet的生命周期内包容和管理Servlet。在JSP技术 推出后，管理和运行Servlet/JSP的容器也称为Web容器。

## SpringMVC vs Servlet



## Servlet LifeCycle

​	constructor	
​	init	
​	service
​	destroy

## Servlet

​	inheritance architecture
​		Servlet interface 
​		GenericServlet abstract class
​		HttpServlet abstract class
​			doGet()
​			doPost()

​	

## HTTPSevlet

​	ServletConfig
​	ServletContext
​	HttpRequest v.s. HttpResponse

## Relative path v.s. Absolute path

## Forward v.s. Redirection

##   Session v.s. Cookie

Cookie指某些网站为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过[加密](https://zh.wikipedia.org/wiki/加密).）Cookie就是用来绕开HTTP的无状态性的“额外手段”之一

**Cookie是检查用户身上的”通行证“来确认用户的身份，Session就是通过检查服务器上的”客户明细表“来确认用户的身份的。Session相当于在服务器中建立了一份“客户明细表”**。

# Lecture 15

### 

## Servlet

servlet is not thread safe by default.

## 3-layer

### Web layer/presentation layer

### Service layer/logic layer

### Data Access Object (DAO) layer

Between layers, using interface instead of concrete class. 

Q: how can you instance an interface in Spring？

The interface is not instanced, it is autowired. 

# Lecture 16 Spring Basic

## Loose Coupling

A want B, No need to create the object, just ask for it from the container. 

## Spring

### Process：

1. spring will start with creating application context,there is a bean factory in application context

2. bean factory will scan all class with annotations to create objects. If no annotation provide, no object create. 默认每个类会生成一个object。

   **annotations:**

   **@component** Indicates that an annotated class is a “component”. Such classes are considered as candidates for auto-detection when using annotation-based configuration and classpath scanning.

   

   **@service,** Indicates that an annotated class is a “Service”. This annotation serves as a specialization of @Component, allowing for implementation classes to be autodetected through classpath scanning.

   

   **@Controller,** The @Controller annotation is used to indicate the class is a Spring controller. This annotation can be used to identify controllers for Spring MVC or Spring WebFlux.

   

   **@Repository**** Indicates that an annotated class is a “Repository”. This annotation serves as a specialization of @Component and advisable to use with [DAO](https://www.journaldev.com/16813/dao-design-pattern) classes.

   

   **@Autowired**是什么意思？向factory要bean？[Spring @Autowired annotation](https://www.journaldev.com/2623/spring-autowired-annotation) is used for automatic injection of beans. Spring @Qualifier annotation is used in conjunction with Autowired to avoid confusion when we have two of more bean configured for same type.

   

   **@Configuration** Used to indicate that a class declares one or more `@Bean` methods. These classes are processed by the Spring container to generate bean definitions and service requests for those beans at runtime.

   **@Bean**是什么意思？use on method,  执行后产生一个bean放入factory。 为什么要用bean？have no control of source code/class, such as "String class ". 

   ```java
   @Bean(name="myString")
   public String getAString(){
     return "hello"; //will create a String Bean, name="myString", value = "hello";
   }
   
   @Bean(name="myString2")
   @Primary
   public String getAString2(){
     return "hi"; 
   }
   
   @Autowired
   @Quilifier("myString")
   private String message;
   ```

   **@Primary** 相同种类bean，如果不specify，Spring will assign the one with @Primary annotation

   **@Qualifer** used under @Autowired, to specify which one to use

   

3. Put created objects in application context, there is a bean factory in application context. object name is the lowercase of class name by default.



### What **application context** do?

The Application Context is Spring's advanced container. Similar to BeanFactory, it can load bean definitions, wire beans together, and dispense beans upon request. 

 It adds more enterprise-specific functionality such as the ability to resolve textual messages from a properties file and the ability to publish application events to interested **event listener**s.

**java name convention?**

![img](https://pic4.zhimg.com/80/v2-59c4d62185a8ff082bf331e361f51c2f_720w.jpg)



## Dependency Injection / Inverse of  Control (必考)

Inversion of control is a design principle which helps to invert the control of object creation. instead of the programmer controlling the flow of a program, the external sources (framework, services, other components) take control of it. 

Dependency Injection is a design pattern which implements IOC principle. Dependency injection is basically providing the objects that an object needs (its dependencies) instead of having it construct them itself.

bean factory will send the object needed, the class just call it instead of create the object, this tis dependency injection.

getbean(): get the object



### DI Types(必考)

pros and cons?

**Field Injection** throw everything in field-normal used, use @Autowired

Pro: Easy to use, no constructors or setters required.Can be easily combined with the constructor and/or setter approach

Con: Less control over object instantiation. A number of dependencies can reach dozens until you notice that something went wrong in your design. No immutability — the same as for setter injection.



**Constructor Injection** use a constructor to inject objects,@Autowired can be omitted. (recommended)

Pro: 1.**easy for test**; 2.can add check lines since it's method (same as setter injection)  

Con: may have **cycle situation** A->B->C->A, can not create any object if autowire in constructor.



**Setter Injection** use@Autowired and setter

Pro: Flexibility in dependency resolution or object reconfiguration, it can be done anytime. Plus, this freedom solves the circular dependency issue of constructor injection.

Con:Null checks are required, because dependencies may not be set at the moment. more error-prone and less secure than constructor injection due to the possibility of overriding dependencies.

```java
//field injection
@Autowired
DemoDAO aDAO;

//constructor injection
@Autowired //can omit
public setd(DemoDAO d){
  aDAO = d; 
}
//setter injection
@Autowired
public void setaDAO(DemoDAO aDAO){
  this.aDAO = aDAO;
}
```

 

## Factory Design Pattern & Reflection API & Class Loader

Bean factory - factory design pattern

  

## Bean Scope

### Singleton Scope

is a singleton scope. Only one object of each class is created in the begining of Spring by default. unless manually create more than one object. (getString() and getString2() create 2 String objects.)

### Prototype Scope

If the scope is set to prototype, the Spring IoC container creates a new bean instance of the object every time a request for that specific bean is made.

### Session?

This scopes a bean definition to an HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

### Global Session?

This scopes a bean definition to a global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.

Seesion 和 Global session有什么不同？

## Component vs Bean

| @Component                                                   | @Bean.                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| service based class                                          | Factory more tailor made objects                             |
| Preferable for component scanning and automatic wiring.      | Sometimes automatic configuration is not an option. **When?** Let's imagine that you want to wire components from 3rd-party libraries (you don't have the source code so you can't annotate its classes with @Component), so automatic configuration is not possible. |
|                                                              | The **@Bean** annotation **returns an object** that spring should register as bean in application context. The **body of the method** bears the logic responsible for creating the instance. |
| **does not decouple** the declaration of the bean from the class definition | **decouples** the declaration of the bean from the class definition. |
| **auto detects** and configures the beans using classpath scanning | **explicitly declares** a single bean, rather than letting Spring do it automatically. |
| a **class level annotation**                                 | a **method level annotation** and name of the method serves as the bean name |
| **need not to be used with the @Configuration** annotation   | has to be **used within the class which is annotated with @Configuration** |
| **cannot create a bean** of a class using @Component, if the class is outside spring container | **can create a bean** of a class using @Bean even if the class is present **outside the spring container** |
| has **different specializations** like @Controller, @Repository and @Service | has **no specializations**                                   |



## EntityManagerFactory (for JDBC)



# Lecture17 AOP & Spring MVC

## AOP

### what is AOP?

Aspect-Oriented Programming entails breaking down program logic into distinct parts called concerns. The functions that span multiple points of an application are called **cross-cutting concerns** and these cross-cutting concerns are conceptually separate from the application's business logic. 

Dependency Injection helps you **decouple your application objects** from each other and AOP helps you **decouple cross-cutting concerns** from the objects that they affect.

自己理解：

A called B, we inject some code between A and B. In order to use AOP, all the objects should in the bean container. If create a object outside container, AOP will not kick in.

spring用**代理类**包裹切面，把他们织入到Spring管理的bean中。也就是说代理类伪装成目标类，它会截取对目标类中方法的调用，让调用者对目标类的调用都先变成调用伪装类，伪装类中就先执行了切面（aspect injection?），再把调用转发给真正的目标bean。

### how to implement AOP?

对method执行/annotation 进行monitor if method with certain keyword is execute, the AOP code will execute.

### why we need AOP?

1. some code we want to run in a lot of different places.

就是为了方便，看一个国外很有名的大师说，编程的人都是“懒人”，因为他把自己做的事情都让程序做了。用了aop能让你少写很多代码，这点就够充分了吧

2. dont want to increate the caller burden to call all the classes, let the caller focus on their logic and task

就是为了更清晰的逻辑，可以让你的业务逻辑去关注自己本身的业务，而不去想一些其他的事情，这些其他的事情包括：安全，事物，日志等。

### 特点

very generic, if we don't know AOP is implement, its hard to track the execution and easy to get confused. Slow.

Is AOP also de-coulping?

### 跟proxy关系？

Aspect is in IOC container. AOP实际上是A call B 的proxy时发生的(proxy design pattern used, dynamic proxy)， a calls b's proxy, then aspect kicks in.

如果call发生在一个bean内部，AOP不会执行，因为proxy在bean的外部。



### keywords 关键字（必考）

 solve **cross cutting concerns.**

what is cross cutting concerns？such as security, transaction, cache, auditing, log. Not the main business logics, but have a lot side effect and extra features to help us manage the program.

<img src="https://www.baeldung.com/wp-content/uploads/2017/11/Program_Execution-300x180.jpg" alt="img" style="zoom:150%;" />

**Advice** 通知  This is the actual action to be taken either before or after the method execution.This is **an actual piece of code** that is invoked during the program execution by Spring AOP framework. 什么时候发生？

**jointpoint**  连接点 **runtime concept, not real code,**  This represents a point in your application where you can plug-in the AOP aspect. 在哪里发生？(code location)  

**pointcut,** 切入点 This is a set of one or more join points where an advice should be executed.（连接点的集合？） You can specify pointcuts using expressions or patterns as we will see in our AOP examples. 发生在谁身上？

```java
@Around("execution(* *.save*(..))") // exectute when method have "save" in the name,advice + pointcut 

@Around("@annotation(Cache)") //can also use annotation as pointcut,location: @Cache

//annotation example : @Transactional
```

**Aspect** 切面是通知和切入点的结合。This is a module which has a set of APIs providing cross-cutting requirements.

**target** 目标 The object being advised by one or more aspects. This object will always be a proxied object, also referred to as the advised object.

**Weaving** Weaving is the process of linking aspects with other application types or objects to create an advised object. This can be done at compile time, load time, or at runtime.



### **Type of Advice**(实操？)

**before** Run advice before the a method execution. 在 join point 前被执行的 advice. 虽然 before advice 是在 join point 前被执行, 但是它并不能够阻止 join point 的执行, 除非发生了异常(即我们在 before advice 代码中, 不能人为地决定是否继续执行 join point 中的代码)

**after** Run advice after the method execution, regardless of its outcome.

**after-returning** Run advice after the a method execution only if method completes successfully.

**after-throwing** Run advice after the a method execution only if method exits by throwing an exception.

**around** Run advice before and after the advised method is invoked. run两次？

**Around advice** runs "around" a matched method execution. It has the opportunity to do work both before and after the method executes, and to determine when, how, and even if, the method actually gets to execute at all. Around advice is often used if you need to **share state before and after a method execution** in a thread-safe manner (starting and stopping a timer for example).



## **@transactional  (必考)** 

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

**@Cacheable**

Spring use A ConcurrentHashmap(?) to save all cache data.



## SpringMVC

### FrontController / DispatchedSevlert

used to Connect HTTP & Spring Controllers

**web request -> TOMCAT -> DispatchedSevlert ->Spring MVC Controller**

### HandlerMapping

@RequestMapping 



# Lecture18 Rest

## Spring Restful Web Service

### what is a Restful API?（必考）

A RESTful API is an architectural style for an application program interface ([API](https://searchapparchitecture.techtarget.com/definition/application-program-interface-API)) that uses HTTP requests to access and use data. That data can be used to GET, PUT, POST and DELETE data types, which refers to the reading, updating, creating and deleting of operations concerning resources.



###  what is an URI?

A **Uniform Resource Identifier** (URI) is a generic term for the names of all resources connected to the World Wide Web. URIs enable the protocols over the internet to conduct the interactions between and among resources.

1. A **URI is an **identifier** of a specific resource. Like a page, or book, or a document.
2. A **URL** is special type of identifier that also tells you how to access it, such as `HTTPs`, `FTP`, etc.—like **https://**www.google.com.
3. If the protocol (`https`, `ftp`, etc.) is either present or implied for a domain, **you should call it a \**URL\****—even though it’s also a **URI**.



### 分析http://www.antra.com:80/user/123?id=1&sex=male

**http**: protocol

**www.antra.com:** domain

**80**: port number, 80 for http, 443 for https

**user/123**: path variable (Controller handle start here)

**id =1&sex=male**: query parameters

**#tab**: used for UI only

HTTP -> dispatched servlet -> controller -> DB -> controller -(user obj) -> dispachted servlet - (json file) response body -> HTTP



### what is the different between Post & Put?

Post is not idempotent. Mostly used for create.

Put is idempotent. Mostly used for update.



### what is a pagination?(必考)

按page返回结果，分批返回（例子：浏览Amazon）

read whole database -> slice

read a slice from database -> return

read from cache -> return





## Spring Restful Annotations

**@RestController** = @Controller + @ResponseBody  is singleton

When you annotate a controller class with @RestController it does two purposes, first, it says that the controller class is handling a request for REST APIs and second you don't need to annotate each method with the @ResposneBody annotation to signal that the response will be converted into a Resource using various HttpMessageConverers.

**@Controller** 

This annotation is used to make a class as a web controller, which can handle client requests and send a response back to the client.

**@GetMapping("/user/{id}")** = @RequestMapping(value="/user/{id}", method = RequestMethod.GET)

**@RequestMapping("/api")**

It's a method level annotation that is specified over a handler method.

**@PathVariable**  **写在method argument里

is used to retrieve data from the URL,his annotation enables the controller to handle a request for parameterized URLs like URLs that have variable input as part of their path

```java
http://localhost:8080/books/900083838

@RequestMapping(value="/books/{ISBN}",
                        method= RequestMethod.GET)
public String showBookDetails(@PathVariable("ISBN") String id,
Model model){
   model.addAttribute("ISBN", id);
   return "bookDetails";
}
```



**@RequestParam **写在method argument里

is used to bind HTTP parameters into method arguments of handler methods.

```java
http://localhost:8080/books?ISBN=900083838

@RequestMapping("/book") 
public String showBookDetails( @RequestParam("ISBN") String ISBN, Model model){ 		
  											model.addAttribute("ISBN", ISBN); return "bookDetails"; 
											}
```



**@RequestBody**

This annotation can convert inbound HTTP data into Java objects passed into the controller's handler method.

```java
@RequestMapping(method=RequestMethod.POST, consumers= "application/json") 
public @ResponseBody Course saveCourse(@RequestBody Course aCourse){
  								return courseRepository.save(aCourse); 
						}

```



**@ResponseBody**

is used to transform a Java object returned from he a controller to a resource representation requested by a [REST client](https://javarevisited.blogspot.com/2017/02/how-to-consume-json-from-restful-web-services-Spring-RESTTemplate-Example.html). It can completely bypass the view resolution part.

```java
@RequestMapping(method=RequestMethod.POST,consumers= "application/json") 
public @ResponseBody Course saveCourse(@RequestBody Course aCourse){ 
  							return courseRepository.save(aCourse); 
							}
```



@RequestHeader



### ResponseEntity? 

ResponseEntity represents the whole HTTP response: status code, headers, and body. As a result, we can use it **to fully configure the HTTP response**. If we want to use it, we have to return it from the endpoint; Spring takes care of the rest.

```java
 @RequestMapping("/handle")
 public ResponseEntity<String> handle() {
   HttpHeaders responseHeaders = new HttpHeaders();
   responseHeaders.set("MyResponseHeader", "MyValue");
   return new ResponseEntity<String>("Hello World", responseHeaders, HttpStatus.CREATED);
 }
```



## Swagger Tool

api test tool



## Spring操作总结

### Message?

```java
	Constants messages;

	@Autowired
	public UserRestController(UserService userService,Constants messages) {
		this.userService = userService;
		this.messages = messages;
	}

//@Component
public class Constants {
    private Map<String, String> messages;

    @Autowired
    LookupRepository lookupRepo;

    @PostConstruct
    public void init() {
       messages = new HashMap<>();
       List<LookupEntity> userMessages = lookupRepo.findByType("USER_MESSAGE");
       userMessages.forEach(m -> messages.put(m.getName(), m.getValue()));
    }

    public String getMessage(String msgName) {
       return messages.get(msgName);
    }
}
```





### validate

验证传入数据是否有效,embeded in Spring MVC. From Hibernate Validator.

Hibernate validator has nothong to do with ORM, it validate the objects inside the projects (meaning?).

```java
/** create a user **/
//use @Validate to check User
	@ApiOperation(value = "create a user")
	@RequestMapping(value = "/user", method = RequestMethod.POST, consumes = "application/json")
	public ResponseEntity<ResponseMessage> createUser(@Validated @RequestBody User user, UriComponentsBuilder ucBuilder) {
		User savedUser = userService.saveUser(user);
		HttpHeaders headers = new HttpHeaders();
		headers.setLocation(ucBuilder.path("/api/user/{id}").buildAndExpand(user.getId()).toUri());
		return new ResponseEntity<ResponseMessage>(new ResponseMessage(messages.getMessage("USER_CREATED"),savedUser), headers, HttpStatus.CREATED);
	}

//validate in user class

public class User{
	
	private Long id;

		@NotNull //validate name not null
    private String name; 
     @Max(100) //validate age max 100
	 	@Min(10) //validate age min 10
    private Integer age;
     
    private Double salary;

    public User() {
    	
    }

	}
```

### ExceptionHandler (AOP)

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> exceptionHandlerUserNotFound(Exception ex) {
        ErrorResponse error = new ErrorResponse();
        error.setErrorCode(HttpStatus.NOT_FOUND.value());
        error.setMessage(ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

}

```



### Logging

**slf4j: **Simple Logging Facade for Java 抽象层

**SpringBoot** 默认选择的是 **SLF4J** + **Logback** 的组合

```java
private static Logger logger = LoggerFactory.getLogger(UserRestController.class);
```



### Pagnation

List findAll（Sort sort）            返回所有实体，按照指定顺序排序返回

List findAll（Pageable pageable）  返回实体列表，实体的offset和limit通过pageable来指定

Pageable接口用于构造翻页查询，PageRequest是其实现类，

```java
//可以通过提供的工厂方法创建PageRequest：
public static PageRequest of(int page, int size)
  
//也可以在PageRequest中加入排序：
public static PageRequest of(int page, int size, Sort sort)
  
 //exmaple
 Page<UserEntity> page1 = userRepo.findAll(PageRequest.of(page, size, sort)); // return only one page
```

**方法中的参数，page总是从0开始，表示查询页，size指每页的期望行数**。

### Cache

**@EnableCaching：**在启动类注解@EnableCaching开启缓存

```java
@SpringBootApplication
@EnableCaching  //开启缓存
public class DemoApplication{
 
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
 
}
```

**@Cacheable：**配置了findByName函数的返回值将被加入缓存。同时在查询时，会先从缓存中获取，若不存在才再发起对数据库的访问。

value、cacheNames：两个等同的参数（cacheNames为Spring 4新增，作为value的别名），用于指定缓存存储的集合名。由于Spring 4中新增了@CacheConfig，因此在Spring 3中原本必须有的value属性，也成为非必需项了

key：缓存对象存储在Map集合中的key值，非必需，缺省按照函数的所有参数组合作为key值，若自己配置需使用SpEL表达式，比如：@Cacheable(key = “#p0”)：使用函数第一个参数作为缓存的key值

condition：缓存对象的条件，非必需，也需使用SpEL表达式，只有满足表达式条件的内容才会被缓存，比如：@Cacheable(key = “#p0”, condition = “#p0.length() < 3”)，表示只有当第一个参数的长度小于3的时候才会被缓存

unless：另外一个缓存条件参数，非必需，需使用SpEL表达式。它不同于condition参数的地方在于它的判断时机，该条件是在函数被调用之后才做判断的，所以它可以通过对result进行判断。

keyGenerator：用于指定key生成器，非必需。若需要指定一个自定义的key生成器，我们需要去实现org.springframework.cache.interceptor.KeyGenerator接口，并使用该参数来指定。需要注意的是：该参数与key是互斥的

cacheManager：用于指定使用哪个缓存管理器，非必需。只有当有多个时才需要使用

cacheResolver：用于指定使用那个缓存解析器，非必需。需通过org.springframework.cache.interceptor.CacheResolver接口来实现自己的缓存解析器，并用该参数指定。

```java

public class BotRelationServiceImpl implements BotRelationService {
    @Override
    @Cacheable(value = {"newJob"},key = "#p0")
    public List<NewJob> findAllLimit(int num) {
        return botRelationRepository.findAllLimit(num);
    }
    .....
}


```

**@CachePut：**主要针对方法配置，能够根据方法的请求参数对其结果进行缓存，和 @Cacheable 不同的是，它每次都会触发真实方法的调用 。简单来说就是用户更新缓存数据。但需要注意的是该注解的value 和 key 必须与要更新的缓存相同，也就是与@Cacheable 相同。示例：

```java

    @CachePut(value = "newJob", key = "#p0")  //按条件更新缓存
    public NewJob updata(NewJob job) {
        NewJob newJob = newJobDao.findAllById(job.getId());
        newJob.updata(job);
        return job;
    }
```

**@CacheEvict：**配置于函数上，通常用在删除方法上，用来从缓存中移除相应数据。除了同@Cacheable一样的参数之外，它还有下面两个参数：

allEntries：非必需，默认为false。当为true时，会移除所有数据。如：@CachEvict(value=”testcache”,allEntries=true)

beforeInvocation：非必需，默认为false，会在调用方法之后移除数据。当为true时，会在调用方法之前移除数据。 如：@CachEvict(value=”testcache”，beforeInvocation=true)

```java

@Cacheable(value = "emp",key = "#p0.id")
    public NewJob save(NewJob job) {
        newJobDao.save(job);
        return job;
    }
 
    //清除一条缓存，key为要清空的数据
    @CacheEvict(value="emp",key="#id")
    public void delect(int id) {
        newJobDao.deleteAllById(id);
    }
 
    //方法调用后清空所有缓存
    @CacheEvict(value="accountCache",allEntries=true)
    public void delectAll() {
        newJobDao.deleteAll();
    }
 
    //方法调用前清空所有缓存
    @CacheEvict(value="accountCache",beforeInvocation=true)
    public void delectAll() {
        newJobDao.deleteAll();
    }
```

**@CacheConfig：** 统一配置本类的缓存注解的属性，在类上面统一定义缓存的名字，方法上面就不用标注了，当标记在一个类上时则表示该类所有的方法都是支持缓存的

```java
@CacheConfig(cacheNames = {"myCache"})
public class BotRelationServiceImpl implements BotRelationService {
    @Override
    @Cacheable(key = "targetClass + methodName +#p0")//此处没写value
    public List<BotRelation> findAllLimit(int num) {
        return botRelationRepository.findAllLimit(num);
    }
    .....
}
```



### Multithreading

Spring MVC do not garatuee thread safe. Multi-threading safe need to be implmented in code.

# Lecture 19 SpringBoot

## FeignClient & Assessment



## SpringBoot

### what is SpringBoot?

what is slf4j？

to write logs in Spring.

why use log? what is the different between logger and system.out.print()?

how to decide thread-safe or not?

How to solve theard-safe/concurrency  issue?

**@SprinbBootApplication**

This is a relatively new annotation but very useful if you are using Spring Boot for creating Java web application with Spring. This single annotation combines three annotations like @Configuration, @EnableAutoConfiguration, and @ComponentScan. If you use [Spring Boot](http://www.java67.com/2018/06/5-best-courses-to-learn-spring-boot-in.html), then you can run your application without deploying it into a web server, as it comes with an embedded Tomcat server.

### what is the advantage of SpringBoot?



# Lec 20 Security



## Encryption

## Hashing

signature purpose

## Encode

### URL Encoder

### File Encoder

Base64?

## Client Side Encryption

xss, csrf,cors

## Authentication

valid of user, username&password

## Authorization授权

has Rule or not 授权？

## JWT token
