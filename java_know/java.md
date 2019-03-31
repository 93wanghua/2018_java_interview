## 一、基础篇
### 1.1、Java基础

#### 1.1.1 面向对象的特征：继承、封装和多态

```
1.封装：信息封装
2.继承：父类 子类
3.多态：有二种方式，覆盖，重载。
```

#### 1.1.2 final, finally, finalize 的区别

```
1.final 使用过程中不被修改；方法不能重载
2.finally 块则是无论异常是否发生，都会执行finally块的内容，所以在代码逻辑中有需要无论发生什么都必须执行的代码，就可以放在finally块中
3.finalize（）方法是在垃圾收集器删除对象之前对这个对象调用的。
```

#### 1.1.3 Exception、Error、运行时异常与一般异常有何异同
```
Throwable
    Error
    Exception
        runtime exception
        checked exception

1.运行时异常：由java虚拟机抛出的异常。用户不必处理。
2.检测异常：编译报错 需要自行处理
```

#### 1.1.4 请写出5种常见到的runtime exception

```
ClassCastException 强制转换异常
NullPointerException 空指针异常
StringIndexOutOfBoundsException 字符越界异常
IllegalArgumentException 非法参数异常
ArrayIndexOutOfBoundsException 数组越界异常
FileNotFoundException 文件未找到异常 文件未找到异常
IOException io异常
```

#### 1.1.5 创建一个对象，内存中发生了什么？
```
    类的成员变量存在堆内存，随着对象的产生和消失。局部变量存在于栈内存中，随着所属区域的运行而存在，结束而释放。

    Person p = new Person();

    1.先将硬盘上指定位置的Person.class文件加载进内存。

    2.执行main方法时，在栈内存中开辟了main方法的空间（压栈-进栈），然后在main方法的栈区分配了一个变量p。

    3.在堆内存中开辟一个实体空间，分配了一个内存首地址值。

    4.在该实体空间中进行属性的空间分配，并进行了默认初始化。

    5.对空间中的属性进行显示初始化。

    6.进行实体的构造代码块初始化。

    7.调用该实体对应的构造函数，进行构造 函数初始化。

    8.将首地址赋值给p，p变量就引用了该实体。（指向了该对象）
    
```
#### 1.1.6 int 和 Integer 有什么区别，Integer的值缓存范围
```
int 默认0
integer 默认null

Byte，Short，Long 有固定范围: -128 到 127。
对于 Character, 范围是 0 到 127。除了 Integer 可以通过参数改变范围外，其它的都不行。
```

#### 1.1.7 包装类，装箱和拆箱
```
1.Java中的8个包装类分别是：Byte, Short, Integer, Long, Float, Double, Character, Boolean，它们的使用方式都是一样的，可以实现原生数据类型与包装类型的双向转换
```
#### 1.1.8 String、StringBuilder、StringBuffer
  
> [String、StringBulider、StringBuffer](https://www.cnblogs.com/dolphin0520/p/3778589.html)

> [String详解](https://www.cnblogs.com/heima-jieqi/archive/2012/04/10/2440086.html)

```
    1. String: 
        * String类是final类，也即意味着String类不能被继承，并且它的成员方法都默认为final方法。早期java5之前，final修饰的方法可以提升点效率。
        * String默认通过char数值保存字符串。(private final char value[];)
        * 无论是sub、concat还是replace操作都不是在原有的字符串上进行的，而是重新生成了一个新的字符串对象。也就是说进行这些操作后，最原始的字符串并没有被改变。
        * String str="hello"和Stringstr=new String("hello")的区别
            - 使用了第一种方式，那么当你在声明一个内容也是 "hello "的string时，它将使用串池里原来的那个内存，而不会重新分配内存，也就是说，string str= "hello",将会指向同一块内存。
            - 用第二种new的方式，不管串池里有没有"hello"，它都会在堆中重新分配一块内存，定义一个新的对象。
            - 比如再写个String str= "hello2" 那么虚拟机不会改变原来的对象，而是生成一个新的string对象，然后让str去指向它，如果原来的那个 "hello"没有任何对象去引用它，虚拟机的垃圾回收机制将接收它。

        
    2. StringBulider jdk1.5
        * 效率比StringBuffer高

    3. StringBuffer jdk1.0
        * 底层实现方式使用synchronized 线程安全
```

#### 1.1.9 String 堆栈问题
    
- ![image](https://note.youdao.com/yws/public/resource/6529ab54c8430de029a4aacf61d5d923/xmlnote/8CE95BC0306B40F8A161E01CEAE80D87/11049)
```java

1. String str1 = "abc"; 
    System.out.println(str1 == "abc");

步骤： 
1) 栈中开辟一块空间存放引用str1；
2) String池中开辟一块空间，存放String常量"abc"； 
3) 引用str1指向池中String常量"abc"；
4) str1所指代的地址即常量"abc"所在地址，输出为true；

 

2. String str2 = new String("abc"); 
    System.out.println(str2 == "abc");

步骤： 
1) 栈中开辟一块空间存放引用str2； 
2) 堆中开辟一块空间存放一个新建的String对象"abc"； 
3) 引用str2指向堆中的新建的String对象"abc"；
4) str2所指代的对象地址为堆中地址，而常量"abc"地址在方法区常量池中，输出为false；

 

3. String str3 = new String("abc"); 
    System.out.println(str3 == str2);

步骤： 
1) 栈中开辟一块空间存放引用str3；
2) 堆中开辟一块新空间存放另外一个(不同于str2所指)新建的String对象； 
3) 引用str3指向另外新建的那个String对象 ；
4) str3和str2指向堆中不同的String对象，地址也不相同，输出为false；

 

4. String str4 = "a" + "b"; 
    System.out.println(str4 == "ab");

步骤： 
1) 栈中开辟一块空间存放引用str4； 
2) 根据编译器合并已知量的优化功能，池中开辟一块空间，存放合并后的String常量"ab"； 
3) 引用str4指向池中常量"ab"；
4) str4所指即池中常量"ab"，输出为true；
```

#### 1.1.10 为什么String设计为不可变类？(效率和安全的考虑)

> 1. `字符串池的要求`
```
    创建字符串时，如果该字符串已经存在于池中，则将返回现有字符串的引用，而不是创建新对象。如果字符串是可变的，则使用一个引用更改字符串将导致其他引用的值错误。
```
> 2. `缓存Hashcode`
```
    字符串的哈希码经常在Java中使用。例如，在HashMap或HashSet中。不可变保证哈希码总是相同的，这样它就可以兑现而不用担心变化。这意味着，每次使用时都不需要计算哈希码。这更有效率。
```
> 3. `安全`
```
    String被广泛用作许多java类的参数，例如网络连接，打开文件等。
```
> 4. `线程安全`
```
    由于无法更改不可变对象，因此可以在多个线程之间自由共享它们。这消除了进行同步的要求。
```


#### 1.1.11 覆盖(override)和重写(overwrite)的区别
```java
    @override
    //子类继承父类,覆盖父类的方法

    @overwrite
    public void update(int index);
    public void update();
    //通过方法的重写，一个类可以有多个同名方法，由传递给它们不同的个数和类型的参数来决定使用哪种方法.
```

#### 1.1.12 抽象类和接口有什么区别
```
1.
抽象类是对类的抽象：本身可以有抽象方法和非抽象方法
接口：里面方法都是抽象的

2.
如果一个非抽象类B extends 抽象类A, 必须要实现A中的所有抽象方法(毕竟抽象方法都是没有具体的实现的，等待子类去实现的)
如果非抽象类B implements 接口A, 必须需要实现A的所有方法

抽象类C extends 抽象类A 和 implements X接口,不强制要求实现

3.
接口和抽象类都不能实例化

4.
抽象类可以有构造器，而接口不能有构造器

5.
java 8 接口新增Default方法: 在接口内部包含了一些默认的方法实现（也就是接口中可以包含方法体，这打破了Java之前版本对接口的语法限制）

```
#### 1.1.13 说说反射的用途及实现

> [反射原理](https://www.cnblogs.com/techspace/p/6931397.html)
```
  1. 反射：Java的反射机制是在编译并不确定是哪个类被加载了，而是在程序运行的时候才加载、探知、自审。使用在编译期并不知道的类。这样的特点就是反射。
  2. 用途：
    Spring都是配置化的（比如通过 XML文件配置JavaBean，Action之类的），为了保证框架的通用性，他们可能根据配置文件加载不同的对象或类，调用不同的方法，这个时候就必须用到反射——运行时动态加载需要加载的对象。
    在运行时判断任意一个对象所属的类；
    在运行时构造任意一个类的对象；
    在运行时判断任意一个类所具有的成员变量和方法；
    在运行时调用任意一个对象的方法；
    生成动态代理。
  3. 实现：基于jvm使用ClassLoader将字节码文件(class文件）加载到方法区内存中：
      Class clazz = ClassLoader.getSystemClassLoader().loadClass("com.mypackage.MyClass");
      可见ClassLoader根据类的完全限定名加载类并返回了一个Class对象，而java反射的所有起源都是从这个class类开始的。
```

#### 1.1.14 说说自定义注解的场景及实现
  
> [参考](https://blog.csdn.net/PORSCHE_GT3RS/article/details/80304701)
  ```java

    1、生成文档。这是最常见的，也是java 最早提供的注解。常用的有@param @return 等
    2、跟踪代码依赖性，实现替代配置文件功能。比如Dagger2依赖注入，未来java开发，将大量注解配置，具有很大用处;
    3、在编译时进行格式检查。如@override 放在方法前，如果你这个方法并不是覆盖了超类方法，则编译时就能检查出。

    java.lang.annotation提供了四种元注解，专门注解其他的注解（在自定义注解的时候，需要使用到元注解）：
        @Documented –注解是否将包含在JavaDoc中
        @Retention –什么时候使用该注解
        @Target –注解用于什么地方
        @Inherited – 是否允许子类继承该注解
    
    4. 注解的本质:
        注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。而我们通过反射获取注解时(field.getAnnotation(FruitName.class))，返回的是Java运行时生成的动态代理对象$Proxy1。通过代理对象调用自定义注解（接口）的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池

  ```
#### 1.1.15 HTTP请求的GET与POST方式的区别
```
    1. get 用于信息获取,幂等;   post 可能修改变服务器上的资源的请求。
    2. GET请求的数据会附在URL之后，以?分割URL和传输数据，参数之间以&相连;   POST把提交的数据则放置在是HTTP包的包体中。
    3. get请求数据有大小限制，post基本没有限制
```

#### 1.1.16 Session与Cookie区别

> 1、cookie数据存放在客户的浏览器上，session数据放在服务器上。

> 2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗。考虑到安全应当使用session。

> 3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE。

> 4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

> 5、多台服务器的session跨域问题解决：
> [spring session案例](https://blog.csdn.net/xlgen157387/article/details/60321984)
- spring session + redis：原有的request请求和response都被重新进行了包装(SessionRepositoryRequestWrapper)。我们也就明白了原有的HttpSeesion是如何被Spring Session替换掉的并不需要tomcat来创建session了。

> 6、所以个人建议：

- 实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个Session ID发送到服务器，我就知道你是谁了。
- `将登陆信息等重要信息存放为SESSION。`
- 其他信息如果需要保留，可以放在COOKIE中。
- Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；
- Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。


#### 1.1.17 列出自己常用的JDK包
```java
java.lang   String，Math，System和Thread类等
java.util   集合框架，并发包（JUC）
java.io     写读写文件或者图片等
java.sql    jdbc相关接口和类
```

#### 1.1.18 equals与==的区别
```java
//Object.java
public boolean equals(Object obj) {
        return (this == obj);
}
```
> 默认情况下也就是从超类Object继承而来的equals方法与‘==’是完全等价的，比较的都是对象的内存地址。但我们可以重写equals方法，使其按照我们的需求的方式进行比较，如String类重写了equals方法，使其比较的是字符的序列，而不再是内存地址。

 #### 1.1.19 hashCode和equals方法的区别与联系


> 详细介绍：https://blog.csdn.net/javazejian/article/details/51348320
```java
String s = "ok";
String t = "ok";
```
> - 字符串s与t拥有相同的hashcode，这是因为字符串的散列码是由内容导出的。
> - 而字符串缓冲sb与tb却有着不同的散列码，这是因为StringBuilder没有重写hashCode方法，它的散列码是由Object类默认的hashCode方法计算出来的对象存储地址，所以每new一个对象散列码自然也就不同了


#### 1.1.20 什么是Java序列化和反序列化，如何实现Java序列化？或者请解释Serializable接口的作用
```
    1. 序列化：把对象转换为字节序列的过程称为对象的序列化。
    2. 反序列化：把字节序列恢复为对象的过程称为对象的反序列化。
    3. 如何实现序列化：

      只有实现了 Serializable 或 Externalizable 接口的类的对象才能被序列化，否则抛出异常。Externalizable接口继承自 Serializable接口，实现Externalizable接口的类完全由自身来控制序列化的行为，而仅实现Serializable接口的类可以 采用默认的序列化方式 。

      对于实现了这两个接口，具体序列化和反序列化的过程又分以下3中情况：

      情况1：若类仅仅实现了Serializable接口，则可以按照以下方式进行序列化和反序列化
      ObjectOutputStream采用默认的序列化方式，对对象的非transient的实例变量进行序列化。
      ObjcetInputStream采用默认的反序列化方式，对对象的非transient的实例变量进行反序列化。

      情况2：若类不仅实现了Serializable接口，并且还定义了readObject(ObjectInputStream in)和writeObject(ObjectOutputSteam out)，则采用以下方式进行序列化与反序列化。
      ObjectOutputStream调用对象的writeObject(ObjectOutputStream out)的方法进行序列化。
      ObjectInputStream会调用对象的readObject(ObjectInputStream in)的方法进行反序列化。

      情况3：若类实现了Externalnalizable接口，且类必须实现readExternal(ObjectInput in)和writeExternal(ObjectOutput out)方法，则按照以下方式进行序列化与反序列化。
      ObjectOutputStream调用对象的writeExternal(ObjectOutput out))的方法进行序列化。
      ObjectInputStream会调用对象的readExternal(ObjectInput in)的方法进行反序列化。

    4. 用途：
　　    1）把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；
　　    2）在网络上传送对象的字节序列。
```

#### 1.1.21 Object类中常见的方法，为什么wait  notify会放在Object里边？
```
    1. 常见的方法：
        - Object()
        - clone()
        - finalize()
        - getClass()
        - hashCode()
        - equal()
        - wait()
        - notify() notifyAll()
    2. 为什么wait() notify()在Object类？
        - notify(), wait()依赖于“同步锁” synchronized ，而“同步锁”是对象锁持有，并且每个对象有且仅有一个！这就是为什么notify(), wait()等函数定义在Object类，而不是Thread类中的原因。
```
#### 1.1.22 Java的平台无关性如何体现出来的
#### 1.1.23 JDK和JRE的区别
#### 1.1.24 Java 8有哪些新特性
```
1. Lambda表达式与Functional接口
2. 接口内部的默认方法和静态方法
3. java.util.stream
4. Optional
5. 类型推测
6. LocalDate和LocalTime类
```
---

### 1.2、Java常见集合

- ![image](https://i1.wp.com/javabydeveloper.com/wp-content/uploads/2016/06/Collection-Framework-hierarchy.png?w=1100)

> [集合小抄](http://calvin1978.blogcn.com/articles/collection.html)

> [集合体系](http://www.cnblogs.com/skywang12345/p/3308498.html)

#### 1.2.1 List 和 Set 区别

- conllection
    - List集合：有序，元素可以重复
        - LinkedList
        - ArrayList

    - Set集合：无序，元素不可重复
        - HashSet  Set的一个实现， 其中不保留元素的特定顺序。
            - LinkedHashSet  Set的实现， 其中与LinkedList一样，维护元素的输入顺序
  
        - SortSet
            - TreeSet  NavigableSet的一个实现，其中维护了元素的 排序顺序。


- map
    - HashMap
        - LinkedHashMap

    - SortedMap
       - TreeMap

- ![iamge](https://note.youdao.com/yws/public/resource/798718b1c1e158bf29932d73a284cd15/xmlnote/585E866EE7304B92BF5B7729D4FD69A1/11355)

#### 1.2.2 为什么使用 List <Movie> listOfMovies =  new  ArrayList <Movie>（）
```
    List 接口 = new ArrayList 类

    利用多态的特性，增加灵活性，方便进行切换实现类，比如将ArrayList换成LinkedList Vector 都是没问题的，他们都实现了List接口

    public List<Movies> getMovies(List<String> actors){
        // √ 如果传递ArrayList,LinkedList,Vector都将起作用
    }

    public List<Movies> getMovies(ArrayList actors){
        // × 只能限制传入ArrayList
    }

```
#### 1.2.3 Set和hashCode以及equals方法的联系
> 存放在Set中的对象的所在类必须重写equals()方法和HashCode()方法。因为要想保证元素的唯一性，必须同时覆盖hashCode和equals才行
```
重写hashCode() 用于获得元素的存储位置。 计算hash再计算index。
重写equals() 用于在两个元素的位置相同的时候,比较两个元素是否相等
```

#### 1.2.4 List 和 Map 区别
> list 列表 动态数组

> map 键值对
#### 1.2.5 Arraylist 与 LinkedList 区别

- ![image](http://www.codenuclear.com/wp-content/uploads/2017/11/ArrayList_LinkedList.jpg)

```
LinkList：Implemented with the concept of doubly linked list.
ArrayList：Implemented with the concept of dynamic array.

1. 与ArrayList相比，LinkedList中的插入/删除既简单又快速，因为没有数组变满，则有调整数组大小和将内容复制到新数组的风险，这使得在最坏的情况下，add()到O(n)的ArrayList中，而在LinkedList中add()是O(1)操作。如果你在其他地方插入了什么东西，ArrayList也需要更新它索引，除非在数组的末尾。
2. LinkedList比ArrayList有更多的内存开销，因为在ArrayList中只有每个索引持有实际对象（数据），但在LinkedList下，每个节点都包含数据、next和previous的节点地址。
3. LinkedList get(index) O(n)时间，需要进行遍历。ArrayList get(index) O（1）时间来查找元素。
4. ArrayList只能作为List使用，因为它只实现List接口，其中LinkedList也可以作为List和Queue，因为它实现了List和Deque接口。
```

#### 1.2.6 ArrayList 与 Vector 区别
```
    1. Vector 数组的矢量实现，synchronized 线程安全
    2. ArrayList 动态数组 非线程安全
```

#### 1.2.7 HashMap 和 Hashtable 的区别
```
    1. HashMap
        - 线程不安全
        - 不是同步，性能优于Hashtable
        - 允许null值
        - extends AbstrucrMap
        - fail-fast 快速失败

    2. HashTable
        - 不允许null值
        - synchronized get put 同步的方式
        - HashTable extends 过时的类Dictionary。
        - not fail-fast


```

#### 1.2.8 HashSet 和 HashMap 区别

- ![image](https://4.bp.blogspot.com/-MJpDYFUhETI/VyTbS1HrsHI/AAAAAAAABA4/y3lqbDg0WUwouNFnDfGGr9-pJGxEF3wnACLcB/s1600/hashset-architecture.png)

```
    - HashSet底层用的hashmap来存储数据的，所以HashSet数据无序、不可以重复。
    - 非线程安全
```

#### 1.2.9 TreeSet
```
TreeSet和HashSet的主要不同在于TreeSet对于排序的支持，TreeSet基于TreeMap实现。
```

#### 1.2.10 TreeMap
> 基本单元

![iamge](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/6D6B02F252B64B90BC3946E6BE3E6D1D/11080)

> 存储结构

![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/2DF3C978A8634604A5F8BA16A0F4DE82/11078)
```
 TreeMap基于红黑树的实现，因此它要求一定要有key比较的方法，要么传入Comparator实现，要么key对象实现Comparable借口。在put操作时，基于红黑树的方式遍历，基于comparator来比较key应放在树的左边还是右边，如找到相等的key，则直接替换掉value。
```

#### 1.2.11 HashMap 和 ConcurrentHashMap 的区别

> [Java7/8的HashMap&ConcurrentHashMap](http://www.importnew.com/28263.html#comment-665133)
```
1. ConcurrentHashMap
    - ConcurrentHashMap的keySet 区域的迭代器是fail-safe并且不会抛出ConcurrentModificationExceptoin ..

        synchronized(map){
          if (map.get(key) == null){
              return map.put(key, value);
          } else{
              return map.get(key);
          }
        }

    - 虽然这个代码在HashMap和Hashtable中可以正常工作，但是在concurrent HashMap中不能工作；因为，在put操作期间，整个映射没有被锁定，并且当一个线程正在放值时，其他线程的get ( )调用仍然可以返回null，这导致一个线程重写由其他线程插入的值。当然，您可以将整个代码包装在同步块中，并使其线程安全，但这只会使您的代码成为单线程的。ConcurrentHashMap提供putIfAbsent ( key，value )，它做同样的事情，但是原子性的，因此消除了上述竞争条件。

    - ConcurrentHashMap doesn't allow null as key or value.

    - During the update operation（put(),remove()）, ConcurrentHashMap only locks a portion of Map instead of whole Map.

    - 可以使用ConcurrentHashMap来代替Hashtable，但是要谨慎，因为CHM并没有锁定整个映射。


```
[Read more](https://javarevisited.blogspot.com/2013/02/concurrenthashmap-in-java-example-tutorial-working.html#ixzz5NZ6EzctT)


#### 1.2.10 ConcurrentHashMap 高并发性主要来自于:
```
    1. 分离锁(lock())实现多个线程间的更深层次的共享访问。
    2. HashEntery 对象的不变性来降低执行读操作的线程在遍历链表期间对加锁的需求。
    3. 通过对同一个 Volatile 变量的写/读访问，协调不同线程间读/写操作的内存可见性。
```

#### 1.2.10 HashMap 的工作原理及代码实现，什么时候用到红黑树


- ![image](https://4.bp.blogspot.com/-adRczhctozE/VD_eimhTQbI/AAAAAAAACCg/lfA1G5GZXyM/s1600/How%2BHashMap%2Bworks%2Bin%2BJava%2B(1).jpg)

> [HashMap原理](http://javabypatel.blogspot.com/2015/10/how-hashmap-works-internally-explain-relation-with-hashcode-and-equals-method.html)

> Buckets/数组 默认大小16（0-15）+单向链表
    
1. HashMap的工作原理是哈希的原理，我们put（）和get（）方法从HashMap中储存和检索对象。

2. 当我们把键和值都传递给put()方法存储在HashMap中时，它使用key 的 object hashcode（）方法来计算hashcode，并通过在hashcode上应用哈希码来确定存储值对象的bucket位置。

3. 检索它时使用键对象equals方法来找出与该键相关联的正确键值对和返回值对象。

4. HashMap在发生碰撞时使用分离链表法，对象将存储在单链表的下一个节点中。

5. 此外，HashMap 的K V的tuple的在链表（linklist）的每个节点中存储为Map.Entry
    

6. 如果HashMap的大小超过了由负载系数定义的给定阈值，Java中的HashMap会发生什么？

    - 因为我们使用的是2次幂的扩展（指长度扩为原来2倍），所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置。因此，我们在resize HashMap的时候，不需要重新计算散列值，只需要看看原来的散列值新增的那个位是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+ OLDCAP”

    - 如果Map的大小超过由负载系数定义的给定阈值，例如，默认负载系数为0.75，则一旦Map填充了75 %，它将重新调整map的大小。与ArrayList等其他集合类类似，HashMap通过创建一个新的buckes数组来重新调整自身的大小，该数组的大小是以前HashMap的2倍。~~然后开始将每个旧元素放入新的桶数组中。这个过程被称为重新散列，因为它也应用散列函数来寻找新的桶位置。~~

    * hash(key)  得到hashcode 再计算index=(n - 1) & hash 插入到 buckets[]，如果分到的buckets 是链表（已经有数据了），则继续从第一个开始遍历链表调用equal(key)相互比较，相同就替换v，不同插入

```java
    static final int hash(Object key) {
        //  HashMap计算hash的方法: 如果key是String则先调用String的hashCode方法
        // hashCode()的高16位 异或 低16位 来实现的
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```
```
HashMap的put过程: 
    1. 对key的hashCode()做hash，然后再计算index;
    2. 如果没碰撞直接放到bucket数组里；
    3. 如果碰撞了，以链表的形式存在buckets后；
    4. 如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD=8)，就把链表转换成红黑树；
    5. 如果节点已经存在就替换old value(保证key的唯一性)
    6. 如果bucket满了(超过load factor*current capacity)，就要resize。

HashMap的get过程: 
    1. 直接在bucket里的第一个节点，直接命中 O(1)；
    2. 如果有冲突，则通过key.equals(k)去查找对应的entry
     若为树，则在树中通过key.equals(k)查找，O(logn)；
     若为链表，则在链表中通过key.equals(k)查找，O(n)。
```

#### 1.2.11 多线程情况下HashMap死循环的问题？

    [Read more about HashMap](https://javarevisited.blogspot.com/2011/02/how-hashmap-works-in-java.html#ixzz5NYMFLQKd)

> 在Java中调整HashMap大小，`即resize()`;时存在潜在的竞争情况，如果两个线程同时发现现在HashMap需要调整大小，并且他们都试图调整大小。在Java中调整HashMap大小的过程中，存储在链表中的桶中的元素在迁移到新桶的过程中会按顺序颠倒，因为Java HashMap不会在尾部追加新元素，而是在头部追加新元素，以避免尾部遍历。如果竞争情况发生，将会以无限循环。


#### 1.2.12 HashMap出现Hash DoS(拒绝服务攻击) 的问题
```
原因: 
-由于使用RESTful风格接口:{"action": "create-account","data": "xxx"}, 如果直接把大量产生hash碰撞的数据放入data参数, 服务器使用jsonDecode()进行解析, 默认存放到HashMap, hash冲突造成服务器cup执行过长


解决方法: 
    - 首先我们需要增加权限验证，最大可能的在jsonDecode()之前把非法用户拒绝。
    - 其次在jsonDecode()之前做数据大小与参数白名单验证。

```

#### 1.2.13 LinkedHashMap结构
- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/FC9C7E9E3AA947088931CF4406961E38/11082)
> s

#### 1.2.14 ConcurrentHashMap 的工作原理及代码实现，如何统计所有的元素个数
```
与Hashtable和Synchronized Map不同，它永远不会锁定整个Map，而是将MAP划分为多个段并对其进行锁定。尽管如果许多reader线程大于writer线程数，它的性能会更好。Collections.SynchronizedMap锁定整个Map，而ConcurrentMap仅锁定Map的一部分。
```

#### 1.2.15 手写简单的HashMap
```java
//1.定义一个接口
public interface SimpleHashMap<K,V>{
    V put();
    V get();

    interface Entry<K,V>{
        public K getKey();
        public V getValue();
    }
}

//2.实现接口
public class MyHashMap<> implements SimpleHashMap<K,V>{

    class Entry<K, V> implements SimpleHashMap.Entry<K, V> {

        K k;
        V v;
        Entry<K, V> next = null;

        public Entry(K k, V v, Entry<K, V> next) {
            super();
            this.k = k;
            this.v = v;
            this.next = next;
        }

        @Override
        public K getKey() {
            return k;
        }

        @Override
        public V getValue() {
            return v;
        }
    }
// ......
}





```

#### 1.2.16 看过那些Java集合类的源码
```

```
#### 1.2.17 for循环与iterator的区别:

//Arraylist implements RandomAccess
 As a rule of thumb, a List implementation should implement this interface if,
 for typical instances of the class, this loop:

     for (int i=0, n=list.size(); i &lt; n; i++)
         list.get(i);

 runs faster than this loop:

     for (Iterator i=list.iterator(); i.hasNext(); )
         i.next();



> For循环不允许更新集合（添加或删除）。~~而Iterator允许修改集合。~~ 此外，Iterator可以在不知道将使用哪种类型的集合的情况下使用，因为所有集合都实现了Iterator接口。

> for循环适合顺序的结构,如ArrayList; Iterator适合链式结构,如LinkedList,通过next()， pre()来定位的

--- 

### 1.3、进程和线程

#### 1.3.1 开启线程的方式：

1. extends Thread
2. implement Runnable/Callable（有返回future对象）
3. Executors.newFixedThreadPool(N)

> start()

> 线程池 submit 有返回值，execute 没有返回值

#### 1.3.2 线程和进程的概念、并行和并发的概念
```
    进程: process 进程是线程的容器
    线程: thread 被称为轻量级进程

    并行: 多核cpu,同时执行各个分区处理
    并发: 多个线程竞争同一资源

```
#### 1.3.3 创建线程的方式及实现

#### 1.3.4 进程间通信的方式
```
信号
管道 FIFO
共享内存
套接字socket
```

#### 1.3.5 说说 `同步辅助类` CountDownLatch、CyclicBarrier、Semaphore 原理和区别

> CountDownLatch 是计数器, 线程完成一个就记一个, 就像 报数一样, 只不过是递减的. 比如有一个任务A，它要等待其他4个任务执行完毕之后才能执行，此时就可以利用CountDownLatch来实现这种功能了。CountDownLatch是不能够重用的，而CyclicBarrier是可以重用的。

> CyclicBarrier更像一个水闸, 线程执行就想水流, 在水闸处都会堵住, 等到水满(线程到齐)了, 才开始泄流. 假若有若干个线程都要进行写数据操作，并且只有所有线程都完成写数据操作之后，这些线程才能继续做后面的事情，此时就可以利用CyclicBarrier了。

> Semaphore其实和锁有点类似，它一般用于控制对某组资源的访问权限。例如：3个工人，2台机器，总有一个工人等待另一个人释放机器后去执行任务。

1. CountDownLatch

    一个`同步辅助类`，CountDownLatch一般用于`某个线程A`等待若干个其他线程执行完任务之后，它才执行；

    用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达0之前，await() 方法会一直阻塞。

    之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。如果需要重置计数，请考虑使用 CyclicBarrier。

    使用场景：
    在一些应用场合中，A需要等待某个B,C,D等条件达到要求后才能做后面的事情，等待检测线程去检测，检测成功后执行A；同时当线程都完成后也会触发事件，以便进行后面的操作。
    这个时候就可以使用CountDownLatch。CountDownLatch最重要的方法是`countDown()`和`await()`，前者主要是倒数一次，后者是等待倒数到0，如果没有到达0，就只有阻塞等待了。

2. CyclicBarrier

    让`一组线程`等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。我们暂且把这个状态就叫做`barrier`，当调用`await()`方法之后，线程就处于barrier了。

3. Semaphore
   
    Semaphore可以控同时访问的线程个数，通过 `acquire()` 获取一个许可，如果没有就等待，而 `release()` 释放一个许可。

#### 1.3.6 说说 Semaphore 原理
```
基于AQS（AbstractQueuedSynchronizer）
Semaphore 内部基础AQS的共享模式，所以实现都委托给了Sync类。

Semaphore 有两种模式：

公平模式：
调用 acquire() 的顺序就是获取许可证的顺序，遵循FIFO；

非公平模式：
为抢占式的，可能一个新的获取线程恰好在一个许可证释放时得到这个许可证，而前面还有等待的线程。

```
#### 1.3.7 说说 Exchanger 原理

> Exchanger（交换者）是一个用于线程间协作的工具类。Exchanger用于进行线程间的数据交换。它提供一个同步点，在这个同步点两个线程可以交换彼此的数据。这两个线程通过exchange方法交换数据， 如果第一个线程先执行exchange方法，它会一直等待第二个线程也执行exchange，当两个线程都到达同步点时，这两个线程就可以交换数据，将本线程生产出来的数据传递给对方。因此使用Exchanger的重点是成对的线程使用exchange()方法，当有一对线程达到了同步点，就会进行交换数据。因此该工具类的线程对象是成对的。

> Exchanger类提供了两个方法: 

> String exchange(V x):用于交换，启动交换并等待另一个线程调用exchange；

> String exchange(V x,long timeout,TimeUnit unit)：用于交换，启动交换并等待另一个线程调用exchange，并且设置最大等待时间，当等待时间超过timeout便停止等待。

https://blog.csdn.net/carson0408/article/details/79477280 

#### 1.3.8 ThreadLocal 原理分析，ThreadLocal为什么会出现OOM，出现的深层次原理
> [参考](https://blog.csdn.net/GoGleTech/article/details/78318712)

> 原理：
```
ThreadLocal 提供了线程本地的实例。它与普通变量的区别在于，每个使用该变量的线程都会初始化一个完全独立的实例副本。

ThreadLocal 变量通常被private static修饰。当一个线程结束时，它所使用的所有 ThreadLocal 相对的实例副本都可被回收

ThreadLocal 实现的一个叫做 ThreadLocalMap 的静态内部类。用的 get()、set() 方法其实都是调用了这个ThreadLocalMap类对应的 get()、set() 方法。

最终的变量是放在了当前线程的 ThreadLocalMap 中，并不是存在 ThreadLocal 上，ThreadLocal 可以理解为只是ThreadLocalMap的封装，传递了变量值。
```
> ThreadLocal里面使用了一个存在`弱引用的map`, map的类型是ThreadLocal.ThreadLocalMap. Map中的key为一个threadlocal实例。这个Map的确使用了弱引用，不过弱引用只是针对key。`每个key都弱引用指向threadlocal`。 当把threadlocal实例置为null以后，没有任何强引用指向threadlocal实例，所以threadlocal将会被gc回收。 但是，我们的`value却不能回收，而这块value永远不会被访问到了，所以存在着OOM。`因为存在一条从current thread连接过来的强引用。只有当前thread结束以后，current thread就不会存在栈中，强引用断开，Current Thread、Map value将全部被GC回收。最好的做法是将调用threadlocal的remove方法。

> 例如：存储用户Session


#### 1.3.9 讲讲线程池的实现原理

> 首先，各自存放线程和任务，其中，任务带有阻塞
```java
private final HashSet workers = new HashSet();
private final BlockingQueue workQueue;
```
> 然后，在 execute 方法中 进行 addWorker(command，true)，也就是创建一个线程，把任务放进去执行；或者是直接把任务放入到任务队列中。
> 如果是 addWorker，那么就会 new Worker(task) ->调用其中 run() 方法，在Worker 的run() 方法中，调用 runWorker(this); 方法 -> 在该方法中就会具体执行我们的任务 task.run(); 同时这个 runWorker方法相当于是个死循环，正常情况下就会一直取出 `任务队列 c中的任务来执行，这就保证了线程不会销毁直到停止。


#### 1.3.10 线程池的几种实现方式

- [java线程池](http://www.cnblogs.com/CarpenterLee/p/9558026.html)

```java
    1. newCachedThreadPool()//创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
    2. newFixedThreadPool(int n) //创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
    3. newScheduledThreadPool() //创建一个定长线程池，支持定时及周期性任务执行。
    4. newSingleThreadExecutor() //创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
```
#### 1.3.11 线程的生命周期，状态是如何转移的

1. 新建：当程序使用new创建一个线程后（用new关键字和Thread类或其子类建立一个线程对象）该线程处于新建状态，此时他和其他java对象一样，仅仅由Java虚拟机为其分配内存并初始化成员变量值。Thread r = new Thread() 

2. 就绪：当线程对象调用start()方法后，该线程处于就绪状态，线程计入线程队列排队，此时该状态线程并未开始执行，它仅表示可以运行了。至于该线程何时运行，取决于JVM线程调度器的调度。r.start() 

3. 运行：处于就绪状态的线程获得了CPU，开始执行run()线程执行体，该线程处于执行状态。如果该线程失去了CPU资源，就会又从运行状态变为就绪状态。从图示可以看出，它可以变为阻塞状态、就绪状态和死亡状态。

4. 阻塞：线程运行过程中需要被中断，目的是是其他的线程获得执行的机会。该状态就会进入阻塞状态。思考一下：什么情况下线程会从运行状态变为阻塞状态？？当发生如下情况是，线程会从运行状态变为阻塞状态：

        ①、线程调用sleep方法主动放弃所占用的系统资源

        ②、线程调用一个阻塞式IO方法，在该方法返回之前，该线程被阻塞

        ③、线程试图获得一个同步监视器，但更改同步监视器正被其他线程所持有

        ④、线程在等待某个通知（notify）

        ⑤、程序调用了线程的suspend方法将线程挂起。不过该方法容易导致死锁，所以程序应该尽量避免使用该方法

        注意：阻塞状态不能直接转成运行状态，阻塞状态只能重新进入就绪状态。

5. 死亡：线程调用stop()方法时或run()方法执行结束后，即处于死亡状态。处于死亡状态的线程不具备有继续执行允许的能力。

        注意：在调用stop()方法结束线程易导致死锁，不推荐。同时，主线程结束后，其他线程不受其影响，不会随之结束。一旦子线程启动起来后，就拥有和主线程相等地位，不受主线程影响。

原文：https://blog.csdn.net/wjtyy/article/details/46289271 

#### 1.3.12 可参考：《Java多线程编程核心技术》

--- 

### 1.4、锁机制/并发

- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/46230AF466A1413FA7AC5D562F303A55/10410)

#### 1.4.1 JUC(java.util.concurrent.*)包

  [JUC知识](http://www.cnblogs.com/skywang12345/p/java_threads_category.html)


#### 1.4.2 关于Concurrent Collection 并发集合类

```
ConcurrentMap (接口)
    - ConcurrentNavigableMap(接口)
        - ConcurrentSkipListMap：跳跃表
    - ConcurrentHashMap：局部锁 Segment数组+HashMap
ConcurrentSkipListSet：基于SkipListMap
CopyOnWriteArrayList：
CopyOnWriteArraySet：

```

- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/WEBRESOURCEe6e5294f5837cd9b480bcb7c8cc50e7b/10742)

```
    1.List & Set
        (01) CopyOnWriteArrayList   相当于线程安全的ArrayList，它实现了List接口。CopyOnWriteArrayList是支持高并发的。
            - transient volatile Object[] array;

            - add()/set() 使用可重入锁
                lock.lock() (ReentrantLock)
                //Arrays.copyOf()
                lock.unlock

            - get() 无锁

            - 适合读多于可变操作

        (02) CopyOnWriteArraySet    相当于线程安全的HashSet的集合，它继承于AbstractSet类。CopyOnWriteArraySet内部包含一个CopyOnWriteArrayList对象，它是通过CopyOnWriteArrayList实现的。

            - 创建的内部实现
            public CopyOnWriteArraySet() {
                al = new CopyOnWriteArrayList<E>();
              }

            - 与HashSet相比,同样不包含重复元素 add()-默认调用-> addIfAbsent(e),但是支持多线程对其进行add() 不会产生ConcurrentModifiedException

```
- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/WEBRESOURCE7ebfe42679a2230f4ccb851e65a23c63/10744)

```
ConcurrentHashMap结构:如下图
```

- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/BAE839E92F2B4744BA498BB0950FFC40/10834)

```
ConcurrentSkipListMap的跳表 下图：
  SkipList:
    1. 由多层结构组成，等级是随机产生的概率。
    2. 每个图层都是有序列表，默认为升序，也可以根据比较器提供的创建映射进行排序，具体取决于使用的方法。
    3. 底部(级别1 )包含列表中的所有元素。
    4. 每个节点包含两个指针，一个指向同一列表中下一个元素的指针，一个指向较低元素的指针。
```
- ![image](https://note.youdao.com/yws/public/resource/6529ab54c8430de029a4aacf61d5d923/xmlnote/WEBRESOURCE8a252a4f0111550cdcf76b6e888c470d/10868)
```
    2.Map

        (01) ConcurrentHashMap  是线程安全的哈希表(相当于线程安全的HashMap)；它继承于AbstractMap类，并且实现ConcurrentMap接口。ConcurrentHashMap是通过“锁分段”来实现的，它支持并发。

            - Segment大小决定了可以并行写入Map的线程数量。默认Segment size=16; 通过传入参数 concurrencyLevel=> ConcurrentHashMap m = new ConcurrentHashMap(initialCapacity, loadFactor, concurrencyLevel)
            - Segment是ConcurrentHashMap中的内部类，它就是ConcurrentHashMap中的“锁分段”对应的存储结构。ConcurrentHashMap与Segment是组合关系，1个ConcurrentHashMap对象包含若干个Segment对象。在代码中，这表现为ConcurrentHashMap类中存在“Segment数组”成员。
            - Segment类继承于ReentrantLock类，所以Segment本质上是一个可重入的互斥锁。
            - HashEntry也是ConcurrentHashMap的内部类，是单向链表节点，存储着key-value键值对。Segment与HashEntry是组合关系，Segment类中存在“HashEntry数组”成员，“HashEntry数组”中的每个HashEntry就是一个单向链表。
            - 对于多线程访问对一个“哈希表对象”竞争资源，Hashtable是通过一把锁来控制并发；而ConcurrentHashMap则是将哈希表分成许多片段，对于每一个片段分别通过一个互斥锁来控制并发。ConcurrentHashMap对并发的控制更加细腻，它也更加适应于高并发场景！

        (02) ConcurrentSkipListMap  是线程安全的有序的哈希表(相当于线程安全的TreeMap); 它继承于AbstractMap类，并且实现ConcurrentNavigableMap接口。ConcurrentSkipListMap是通过“跳表”来实现的，它支持并发。
            - 跳表：跳表分为许多层(level)，每一层都可以看作是数据的索引，这些索引的意义就是加快跳表查找数据速度。每一层的数据都是有序的，上一层数据是下一层数据的子集，并且第一层(level 1)包含了全部的数据；层次越高，跳跃性越大，包含的数据越少。
            - ConcurrentSkipListMap继承于AbstractMap类。
            - Index是ConcurrentSkipListMap的内部类，它与“跳表中的索引相对应”。HeadIndex继承于Index，ConcurrentSkipListMap中含有一个HeadIndex的对象head，head是“跳表的表头”。
            - Index是跳表中的索引，它包含“右索引的指针(right)”，“下索引的指针(down)”和“哈希表节点node”。node是Node的对象，Node也是ConcurrentSkipListMap中的内部类。

        (03) ConcurrentSkipListSet 是线程安全的有序的集合(相当于线程安全的TreeSet)；它继承于AbstractSet，并实现了NavigableSet接口。ConcurrentSkipListSet是通过ConcurrentSkipListMap实现的，它也支持并发。
          - public ConcurrentSkipListSet(){
              m = new ConcurrentSkipListMap();
          }

```
- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/WEBRESOURCE13147b1d04afcb58214505b6afc3c890/10746)

```
    3.Queue

    (01) ArrayBlockingQueue是数组实现的线程安全的有界阻塞队列。
      - ArrayBlockingQueue继承于AbstractQueue，并且它实现了BlockingQueue接口。
      - ArrayBlockingQueue内部是通过Object[]数组保存数据的，也就是说ArrayBlockingQueue本质上是通过数组实现的。ArrayBlockingQueue的大小，即数组的容量是创建ArrayBlockingQueue时指定的。
      - ArrayBlockingQueue与ReentrantLock是组合关系，ArrayBlockingQueue中包含一个ReentrantLock对象(lock)。ReentrantLock是可重入的互斥锁，ArrayBlockingQueue就是根据该互斥锁实现“多线程对竞争资源的互斥访问”。而且，ReentrantLock分为公平锁和非公平锁，关于具体使用公平锁还是非公平锁，在创建ArrayBlockingQueue时可以指定；ArrayBlockingQueue默认会使用非公平锁。
      - ArrayBlockingQueue与Condition是组合关系，ArrayBlockingQueue中包含两个Condition对象(notEmpty和notFull)。而且，Condition又依赖于ArrayBlockingQueue而存在，通过Condition可以实现对ArrayBlockingQueue的更精确的访问

        1. 若某线程(线程A)要取数据时，数组正好为空，则该线程会执行notEmpty.await()进行等待；当其它某个线程(线程B)向数组中插入了数据之后，会调用notEmpty.signal()唤醒“notEmpty上的等待线程”。此时，线程A会被唤醒从而得以继续运行。
        2. 若某线程(线程H)要插入数据时，数组已满，则该线程会它执行notFull.await()进行等待；当其它某个线程(线程I)取出数据之后，会调用notFull.signal()唤醒“notFull上的等待线程”。此时，线程H就会被唤醒从而得以继续运行。

    (02) LinkedBlockingQueue是单向链表实现的(默认Integer.MAX大小的)阻塞队列，该队列按 FIFO（先进先出）排序元素。

      - LinkedBlockingQueue继承于AbstractQueue，它本质上是一个FIFO(先进先出)的队列。
      - LinkedBlockingQueue实现了BlockingQueue接口，它支持多线程并发。当多线程竞争同一个资源时，某线程获取到该资源之后，其它线程需要阻塞等待。
      - LinkedBlockingQueue是通过单链表实现的。
        (1) head是链表的表头。取出数据时，都是从表头head处插入。
        (2) last是链表的表尾。新增数据时，都是从表尾last处插入。
        (3) count是链表的实际大小，即当前链表中包含的节点个数。
        (4) capacity是列表的容量，它是在创建链表时指定的。
        (5) putLock是插入锁，takeLock是取出锁；notEmpty是“非空条件”，notFull是“未满条件”。通过它们对链表进行并发控制。

      - LinkedBlockingQueue在实现“多线程对竞争资源的互斥访问”时，对于“插入”和“取出(删除)”操作分别使用了不同的锁。对于插入操作，通过“插入锁putLock”进行同步；对于取出操作，通过“取出锁takeLock”进行同步。此外，插入锁putLock和“非满条件notFull”相关联，取出锁takeLock和“非空条件notEmpty”相关联。通过notFull和notEmpty更细腻的控制锁。

    (03) LinkedBlockingDeque是双向链表实现的(指定大小)双向并发阻塞队列，该阻塞队列同时支持FIFO和FILO两种操作方式。

        - LinkedBlockingDeque继承于AbstractQueue，它本质上是一个支持FIFO和FILO的双向的队列。
        - LinkedBlockingDeque实现了BlockingDeque接口，它支持多线程并发。当多线程竞争同一个资源时，某线程获取到该资源之后，其它线程需要阻塞等待。
        - LinkedBlockingDeque是通过双向链表实现的。
            1 first是双向链表的表头。
            2 last是双向链表的表尾。
            3 count是LinkedBlockingDeque的实际大小，即双向链表中当前节点个数。
            4 capacity是LinkedBlockingDeque的容量，它是在创建LinkedBlockingDeque时指定的。
            5 lock是控制对LinkedBlockingDeque的互斥锁，当多个线程竞争同时访问LinkedBlockingDeque时，某线程获取到了互斥锁lock，其它线程则需要阻塞等待，直到该线程释放lock，其它线程才有机会获取lock从而获取cpu执行权。
            6 notEmpty和notFull分别是“非空条件”和“未满条件”。通过它们能够更加细腻进行并发控制。


    (04) ConcurrentLinkedQueue是单向链表实现的无界队列，继承于AbstractQueue.该队列按 FIFO（先进先出）排序元素。
        - 使用CAS机制实现并发控制,并非用ReentrantLock
            //...
            UNSAFE.compareAndSwapObject(this, v, cmp, val);
        - ConcurrentLinkedQueue内部是通过链表来实现的。它同时包含链表的头节点head和尾节点tail。ConcurrentLinkedQueue按照FIFO（先进先出）原则对元素进行排序。元素都是从尾部插入到链表，从头部开始返回。
        - ConcurrentLinkedQueue的链表Node中的next的类型是volatile，而且链表数据item的类型也是volatile。关于volatile，我们知道它的语义包含：“即对一个volatile变量的读，总是能看到（任意线程）对这个volatile变量最后的写入”。ConcurrentLinkedQueue就是通过volatile来实现多线程对竞争资源的互斥访问的。

    (05) ConcurrentLinkedDeque是双向链表实现的无界队列，该队列同时支持FIFO和FILO两种操作方式。

```

#### 1.4.3 说说线程安全问题，什么是线程安全，如何保证线程安全
```
    1. 存在共享资源，多个线程对同一个资源进行update操作，存在竞态条件

    2. 如何控制线程安全

        - 同步：同一时刻，只有一个线程操作共享数据
            - 阻塞 （悲观策略）
                - synchronized

            - 非阻塞 （乐观策略）基于 CAS(compare and swap)
                - 变量的内存地址V、旧的预期值A，新值B。当且仅当V的指符合A时，处理器用B更新V的值。


        - 异步
            - Reentrant Code
            - ThreadLocal 将共享数据的可见范围限制在同一个线程之内，如消息队列-生产消费模式

```

#### 1.4.4 在Java多线程编程当中，提供了多种实现Java线程安全的方式：
```
    - 最简单的方式，使用Synchronization关键字:Java Synchronization介绍
    - 使用java.util.concurrent.atomic 包中的原子类，例如 AtomicInteger
    - 使用java.util.concurrent.locks 包中的锁
    - 使用java.util.concurrent.conllection线程安全的集合。如ConcurrentHashMap
    - 使用volatile关键字，保证变量可见性（直接从内存读，而不是从线程cache读）
```

#### 1.4.5 重入锁的概念，重入锁为什么可以防止死锁
```
    - ReentrantLock锁在同一个时间点只能被一个线程锁持有；而可重入的意思是，ReentrantLock锁，可以被单个线程多次获取。
    - ReentrantLock分为“公平锁”和“非公平锁”。它们的区别体现在获取锁的机制上是否公平。“锁”是为了保护竞争资源，防止多个线程同时操作线程而出错，ReentrantLock在同一个时间点只能被一个线程获取(当某线程获取到“锁”时，其它线程就必须等待)；ReentraantLock是通过一个FIFO的等待队列来管理获取该锁所有线程的。在“公平锁”的机制下，线程依次排队获取锁；而“非公平锁”在锁是可获取状态时，不管自己是不是在队列的开头都会获取锁。
```

#### 1.4.6 产生死锁的四个条件（互斥、请求与保持、不剥夺、循环等待）
```

```

#### 1.4.7 如何检查死锁（通过jConsole检查死锁）
```

```

#### 1.4.8 synchronized 实现原理
```
 1 .Java 虚拟机中的同步(Synchronization)基于`进入和退出管程(Monitor)对象`实现， 对象监视。字节码中可知 synchronized 语句块的实现使用的是monitor enter 和 monitor exit 指令

 2. 对象在对内存分3块区域: 对象头,实例数据,对齐填充
 
 ---------------
 |  对象头      |
 |  实例变量    |
 |  填充数据    |
 ---------------

```
```java
    //1. 在java中，每一个对象有且仅有一个同步锁。这也意味着，同步锁是依赖于对象而存在。属于悲观锁

    //2. 当我们调用某对象的synchronized方法时，就获取了该对象的同步锁。例如，synchronized(obj)就获取了“obj这个对象”的同步锁。

    //synchronized 对象级别锁
    synchronized inc(){
        count++;
    }

    //static synchronized 是类级别锁
    static synchronized inc(){
        count++
    }


    //3. 不同线程对同步锁的访问是互斥的。也就是说，某时间点，对象的同步锁只能被一个线程获取到！通过同步锁，我们就能在多线程中，实现对“对象/方法”的互斥访问。

    //4. 例如，现在有两个线程A和线程B，它们都会访问“对象obj的同步锁”。假设，在某一时刻，线程A获取到“obj的同步锁”并在执行一些操作；而此时，线程B也企图获取“obj的同步锁” —— 线程B会获取失败，它必须等待，直到线程A释放了“该对象的同步锁”之后线程B才能获取到“obj的同步锁”从而才可以运行。
```

#### 1.4.9 `synchronized` 关键字 与 `lock` 接口 的区别
```java
    //1. 同步块synchronized 不保证等待进入它的线程被授予访问权的顺序 => 不公平的，存在线程饿死的风险(starvation)

    //2. 不能将任何参数传递给synchronized的入口。因此，不可能有一个超时来访问同步块,如lock.tryLock(10,SECONDS)。

    //3. 同步块必须完全包含在单个方法中。lock()可以在单独的方法中调用lock（）和 unlock（）。

    //4. 使用lock.lock() 必要在try finally 进行lock.unlock()


//锁的简单实现 不可重入的，不公平的
public class Lock{

  private boolean isLocked = false;
  //底层使用synchronized
  public synchronized void lock() throws InterruptedException{
    while(isLocked){
      wait();
    }
    isLocked = true;
  }

  public synchronized void unlock(){
    isLocked = false;
    notify();
  }
}


//锁的简单实现 可重入 reentrant，不公平的
public class Lock{

  boolean isLocked = false;
  Thread  lockedBy = null;
  int     lockedCount = 0;

  public synchronized void lock()
  throws InterruptedException{
    Thread callingThread = Thread.currentThread();
    while(isLocked && lockedBy != callingThread){
      wait();
    }
    isLocked = true;
    lockedCount++;
    lockedBy = callingThread;
  }


  public synchronized void unlock(){
    if(Thread.curentThread() == this.lockedBy){
      lockedCount--;

      if(lockedCount == 0){
        isLocked = false;
        notify();
      }
    }
  }
  ...
}
```

#### 1.4.10 AQS(Abstract Queued Synchronizer)同步队列 (关键字: volatile int state,CAS,FIFO)
```
    0.  CountDownLatch, Semaphore, ReentrantLock, ReadWriteLock 都是是基于AQS实现的

    1. AQS是JDK下提供的一套用于实现基于FIFO等待队列的阻塞锁和相关的同步器的一个同步框架。这个抽象类被设计为作为一些可用原子int值来表示状态的同步器的基类。如果你有看过类似 CountDownLatch 类的源码实现，会发现其内部有一个继承了 AbstractQueuedSynchronizer 的内部类 Sync。可见 CountDownLatch 是基于AQS框架来实现的一个同步器.

    2. AQS的核心思想是基于`volatile int state;`这样的volatile变量，配合CAS工具对其原子性的操作来实现对当前锁状态进行修改, 使用Node实现FIFO双向队列来执行资源获取线程的排队工作。

    3. 使用：AQS主要使用方式某个类通过extends AbstractQueuedSynchronizer 并实现它的抽象方法来管理同步状态，对同步状态的修改或者访问主要通过同步器提供的3个方法：

            getState() 获取当前的同步状态
            setState(int newState) 设置当前同步状态
            compareAndSetState(int expect,int update) 使用CAS设置当前状态，该方法能够保证状态设置的原子性。

    4. AQS可以支持独占式的获取同步状态，也可以支持共享式的获取同步状态，这样可以方便实现不同类型的同步组件。AQS也是实现Lock的关键。

```

#### 1.4.11 CAS无锁的概念、乐观锁和悲观锁

- [锁CAS与Unsafe类](https://blog.csdn.net/javazejian/article/details/72772470)
```
    0. CAS(V 需要做更新的值, E 预期值, N 新值)

    1. CAS是一种非阻塞算法,属于乐观锁: 基于共享数据不会被修改的假设.

    2. 乐观锁: 假设没有冲突发生,如果因为冲突失败就重试,直到成功.

    3. 悲观锁: synchronized() 独占锁;假设冲突总会发生,每次都进行加锁.
```

#### 1.4.12 常见的原子操作类
```Java
    1. AtomicInteger、AtomicBoolean、AtomicLong、AtomicReference

    2. 基于CAS无锁（非阻塞）算法 + volatile

    eg: AtomicInteger
         //内部使用volatile关键字
         private volatile int value;
         //利用CPU的CAS指令，同时借助JNI来完成Java的非阻塞算法。其它原子操作都是利用类似的特性完成的
         //因为CAS是基于乐观锁的，也就是说当写入的时候，如果寄存器旧值已经不等于现值，说明有其他CPU在修改，那就继续尝试。所以这就保证了操作的原子性
         int incrementAndGet(){
             compareAndSet(current,next);
         }


```

#### 1.4.13 volatile 实现原理（禁止指令重排、刷新内存）

    ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/71CD1F72A3BC4B84AC90F1071714276F/10611)
```
    0. 访问volatile变量会阻止指令重新排序，使用happens-before 机制。参考：http://tutorials.jenkov.com/java-concurrency/volatile.html

    1. volatile变量的特性：
        - 可见性（共享的）。直接读写到主内存，意味这任意线程读到的变量都是最后写入的。
        - 需要使用synchronized保证原子性（AtomicLong...等就是基于volatile和 synchronized实现）

    2. 原理：直接写入main memory 而非各自线程的cpu cache，保证可见性。
```
-  ![iamge](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/C75A595B0D094010AB670BB7D12F8F4E/10634)

- ![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/586C109F66534653A5B095C0E5C2C85C/10644)
```
    1. 每次读取一个volatile变量都将从计算机的主内存中读取，而不是从CPU缓存中读取，并且每次写入volatile变量都将写入主内存，而不仅仅是CPU缓存。
    2. 未使用volatile的变量，线程没有看到变量的最新值，因为它还没有被另一个线程从cpu cache写回到主内存中，这个问题被称为“可见性”问题。一个线程的更新对其他线程不可见。
    3. 在上面给出的场景中，一个线程( T1 )修改计数器，另一个线程( T2 )读取计数器(但从不修改)，声明计数器变量volatile足以保证T2对计数器变量的写入可见。然而，如果T1和T2都在递增计数器变量，那么声明计数器变量volatile是不够的。稍后再谈。
    4. 对于多个thread写变量，还需要synchronizd保证原子性。（如使用Atomic包下的类）
```

#### 1.4.14 什么是ABA问题，出现ABA问题JDK是如何解决的
```
CAS缺点：
    ABA问题：因为CAS需要在操作值的时候检查下值有没有发生变化，如果没有发生变化则更新，但是一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。

    ABA问题的解决思路就是使用版本号。在变量前面追加版本号，每次变量更新的时候把版本号+1，那么A-B-A就变成了1A-2B-3C。JDK的atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法作用是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

    ABA问题我们可以使用JDK的并发包中的AtomicStampedReference和 AtomicMarkableReference来解决，底层通过版本号（时间戳）来解决。
```

#### 1.4.15 乐观锁的业务场景及实现方式
```
    1. 乐观锁,悲观锁
        悲观锁：假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。
        乐观锁：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性CAS。 乐观锁不能解决脏读的问题。

    http://blog.csdn.net/truelove12358/article/details/54963791
    http://blog.csdn.net/xlgen157387/article/details/47906553
```
#### 1.4.16 Java 8并法包下常见的并发类

```

```
#### 1.4.17 yield() 与 wait()的比较

- 我们知道，wait()的作用是让当前线程由“运行状态”进入“等待(阻塞)状态”的同时，也会释放同步锁。而yield()的作用是让步，它也会让当前线程离开“运行状态”。它们的区别是：

    (01) 
    - wait()是让线程由 “运行状态” 进入到 “等待(阻塞)状态”
    - yield()是让线程由“运行状态”进入到“就绪状态”。

    (02) 
    - wait() 线程会`释放它所持有对象的同步锁`，在调用 wait()之前，线程必须要获得该对象的对象级别锁，即只能在`同步方法`或`同步块`中调用 wait()方法，进入 wait()方法后，当前线程释放锁。
    - yield()方法`不会释放锁`。
    - sleep()也`不会释放锁`，该线程仍然可以同步synchronized控制。

#### 1.4.18 线程的join()

- ![image](https://note.youdao.com/yws/public/resource/6529ab54c8430de029a4aacf61d5d923/xmlnote/0FCA09C5F8E74CADA1D37CE0304540F9/10950)
> Thread的join()方法，底层是Objec类的wait()方法实现
```java
public class Ajoin {
    static class Father extends Thread {
        @Override
        public void run() {
            Son son = new Son();
            son.start();
            try {
                son.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("father is running");
        }
    }

    static class Son extends Thread {
        @Override
        public void run() {
            System.out.println("son is running");
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Father father = new Father();
        father.start();
    }
}
```
```
son is running
[等待3000ms]
father is running
```
```
  1. 上面的有两个类Father(主线程类)和Son(子线程类)。因为Son是在Father中创建并启动的，所以，Father是主线程类，Son是子线程类。

  2. 在Father主线程中，通过new Son()新建“子线程s”。接着通过s.start()启动“子线程s”，并且调用s.join()。在调用s.join()之后，Father主线程会一直等待，直到“子线程s”运行完毕；在“子线程s”运行完毕之后，Father主线程才能接着运行。 这也就是我们所说的“join()的作用，是让主线程会等待子线程结束之后才能继续运行”！
```

#### 1.4.19 数据库死锁的概念
```
    http://blog.csdn.net/qq_16681169/article/details/74784193
    数据库死锁的解决办法
    http://blog.csdn.net/qq_16681169/article/details/73359670
    http://cyqplay.iteye.com/blog/504535
    http://blog.csdn.net/lemontreey/article/details/53067437
```
#### 1.4.20 偏向锁、轻量级锁、重量级锁、自旋锁的概念
> [锁的概念](https://blog.csdn.net/zqz_zqz/article/details/70233767)
```Java
    //1. 偏向锁：顾名思义，它会偏向于第一个访问锁的线程，如果在运行过程中，同步锁只有一个线程访问，不存在多线程争用的情况，则线程是不需要触发同步的，这种情况下，就会给线程加一个偏向锁。 在无竞争的情况下会把整个同步都消除掉。
    
    //2. 轻量级锁
    
    //3. 重量级锁


    //4. 自旋锁: 自旋锁是一种假设在不久将来，当前的线程可以获得锁，因此虚拟机会让当前想要获取锁的线程做几个空循环50-100次(这也是称为自旋的原因)，在经过若干次循环后，如果得到锁，就顺利进入临界区。

public void lock(){
    Thread current = Thread.currentThread();
    while(!sign.compareAndSet(null, current)){
    }
}
    //当线程申请一个已经被其他线程占用的锁，就会出现两种情况。
         //1）一种是没有获得锁的线程会阻塞自己，等到锁被释放后再被唤起，这就是互斥锁；
         //2）另一种是没有获得锁的线程一直循环在那里看是否该锁的保持者已经释放了锁，这就是自旋锁
```
#### 1.4.21 可参考：《Java多线程编程核心技术》

---

### 1.5、JVM

#### 1.5.1 JVM运行时内存区域划分

- [JVM内存划分](https://zhuanlan.zhihu.com/p/25713880)

- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/95C3149D5FE648BC9628E1BA04BA73B5/11074)


1. 程序计数器：线程私有。是一块较小的内存，是当前线程所执行的字节码的行号指示器。是Java虚拟机规范中唯一没有规定OOM（OutOfMemoryError）的区域。

2. Java栈：线程私有。生命周期和线程相同。是Java方法执行的内存模型。执行每个方法都会创建一个栈帧，用于存储局部变量和操作数（对象引用）。局部变量所需要的内存空间大小在编译期间完成分配。所以栈帧的大小不会改变。存在两种异常情况：若线程请求深度大于栈的深度，抛StackOverflowError。若栈在动态扩展时无法请求足够内存，抛OOM。

3. Java堆：所有线程共享。虚拟机启动时创建。存放对象实力和数组。所占内存最大。分为新生代（Young区），老年代（Old区）。新生代分Eden区，Servior区。Servior区又分为From space区和To Space区。Eden区和Servior区的内存比为8:1。 当扩展内存大于可用内存，抛OOM。

4. 方法区：所有线程共享。用于存储已被虚拟机加载的类信息、常量、静态变量等数据。又称为非堆（Non – Heap）。方法区又称“永久代”。GC很少在这个区域进行，但不代表不会回收。这个区域回收目标主要是针对常量池的回收和对类型的卸载。当内存申请大于实际可用内存，抛OOM。

5. 本地方法栈：线程私有。与Java栈类似，但是不是为Java方法（字节码）服务，而是为本地非Java方法服务。也会抛StackOverflowError和OOM。


#### 1.5.2 栈内存和堆内存
- ![iamge](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/5C5EC221DEE74B7893C68D430511FD69/11237)
```
    1. 存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。另外，栈数据可以共享，存取速度快。
    2. 堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。
```

#### 1.5.3 内存溢出OOM和堆栈溢出SOE的示例及原因、如何排查与解决
#### 1.5.4 如何判断对象是否可以回收或存活

> 垃圾回收动作何时执行？

    1. 当年轻代内存满时，会引发一次普通GC，该GC仅回收年轻代。需要强调，年轻代满是指Eden代满，Survivor满不会引发GC
    2. 当年老代满时会引发Full GC，Full GC将会同时回收年轻代、年老代
    3. 当永久代满时也会引发Full GC，会导致Class、Method元信息的卸载
    4. 另一个问题是，何时会抛出OutOfMemoryException，并不是内存被耗空的时候才抛出

    JVM 98%的时间都花费在内存回收
    每次回收的内存小于2%
    满足这两个条件将触发OutOfMemoryException，这将会留给系统一个微小的间隙以做一些Down之前的操作，比如手动打印Heap Dump。

#### 1.5.5 常见的GC回收算法及其含义

什么是 GC：

GC 是将 Java 的无用的堆对象进行清理，释放内存，以免发生内存泄漏。

常见的回收算法：

标记-清除 算法：
标记阶段：先通过根节点，标记所有从根节点开始的可达对象。因此，未被标记的对象就是未被引用的垃圾对象；
清除阶段：清除所有未被标记的对象；

复制算法（新生代的 GC）：
将原有的内存空间分为两块，每次只使用其中一块，在垃圾回收时，将正在使用的内存中的存活对象复制到未使用的内存块中，然后清楚正在使用的内存块中的所有对象；

标记-整理 算法（老年代的 GC）：
标记阶段：先通过根节点，标记所有从根节点开始的可达对象。因此，未被标记的对象就是未被引用的垃圾对象；
整理阶段：将所有的存活对象压缩到内存的一端；之后，清理边界外所有的空间；

分带收集算法：
存活率低：少量对象存活，适合用复制算法：在新生代中，每次 GC 时都发现有大批对象死去，只有少量存活（新生代中98%的对象都是 "朝生夕死"），那就选用复制算法，只需要付出少量存活对象的复制成本就可以完成 GC；

存活率高：大量对象存活，适合用标记-清除/标记-整理：在老年代中，因为对象存活率高，没有额外空间对他进行分配担保，就必须使用 标记-清除/标记-整理 算法进行 GC。


#### 1.5.6 常见的JVM性能监控和故障处理工具类：jps、jstat、jmap、jinfo、jconsole等
```
jmap：观察运行中的 JVM 物理内存的占用情况；
jps：列出所有的 JVM 实例；
jvmtop：一个轻量级的控制台程序用来监控机器上运行的所有 java 虚拟机；
jstack：观察 jvm 中当前所有线程的运行情况和线程当前状态；
jstat：JVM 统计监测工具；
jconsole：内置 Java 性能分析器

```
#### 1.5.7 JVM如何设置参数
- ![iamge](https://note.youdao.com/yws/public/resource/798718b1c1e158bf29932d73a284cd15/xmlnote/838BD82C1E58459F9B9ADD75A842C0AB/11299)
  
#### 1.5.8 JVM性能调优
> https://www.cnblogs.com/csniper/p/5592593.html

#### 1.5.9 类加载器、双亲委派模型、一个类的生命周期、类是如何加载到JVM中的

> 类加载器：
    
    1. 启动类加载器（BootStrap ClassLoader）：负责加载 JAVA_HOME\lib 目录中的，或通过 -Xbootclasspath 参数指定路径中的，且被虚拟机认可（按文件名识别，如 rt、jar ）的类；

    2. 扩展类加载器（Extension ClassLoader）：
    负责加载 JAVA_HOME\lib\ext 目录中的，或通过 java.ext.dirs 系统变量指定路径中的类库；

    3. 应用程序类加载器（Application ClassLoader）：负责加载用户路径（classpath） 上的类库。

> 双亲委派模型：

    工作过程：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委托给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需要加载的类）时，子加载器才会尝试自己去加载。
    
    好处：能够有效确保一个类的全局唯一性，当程序中出现多个限定名相同的类时，类加载器在执行加载时，始终只会加载其中的某一个类；

> 类的生命周期
> 类是如何加载到JVM的？
#### 1.5.10 类加载的过程：加载、验证、准备、解析、初始化

> 加载：寻找并导入Class文件的二进制信息
    
    加载是类加载过程中的一个阶段，这个阶段会在内存中生成一个代表这个类的 java.lang.class 对象，作为方法区这个类的各种数据的入口。

> 链接包括：（验证、准备、解析）
> 验证：保证导入类型的正确和安全性
    
    这一阶段的主要目的是为了确保 class 文件的字节流中包含的信息是否符合当前虚拟机的要求，并且不会危害虚拟机自身的安全；

> 准备：分配内存以及默认值

    准备阶段是正式为类变量分配内存并设置类变量的初始值阶段，即在方法区中分配这些变量所使用的内存空间。

> 解析：虚拟机将常量池中的符号引用替换为直接引用的过程。

    符号引用：与虚拟机实现的布局无关，引用的目标并不一定要已经加载到内存中。各种虚拟机实现的内存布局可以各不相同，但是它们能接受的符号引用必须是一直的，因为符号引用的字面量形式明确定义在 Java 虚拟机规范的 Class 文件格式中；

    直接引用：是指向目标的指针，相对偏移量或是一个能间接定位到目标的句柄。如果有了直接引用，那引用的目标必定已经在内存中存在。

> 初始化：调用Java代码，初始化类变量为指定初始值
    
    初始化阶段是类加载的最后一个阶段。前面的类加载阶段之后，除了在加载阶段可以自定义类加载器以外，其他操作都是由 JVM 主导。到了初始阶段，才开始真正执行类中定义的 Java 程序代码。


#### 1.5.11 强引用、软引用、弱引用、虚引用
```
强引用：只要引用存在，垃圾回收器永远不会回收；
软引用：非必须引用，内存溢出之前进行回收；
弱引用：第二次垃圾回收时回收；
虚引用：垃圾回收时回收，无法通过引用取到对象值。
```
#### 1.5.12 Java内存模型JMM

- [jvm](https://lingsui.github.io/2018/03/30/JVM%E9%9D%A2%E8%AF%95%E9%A2%98/)

- ![iamge](https://note.youdao.com/yws/public/resource/6529ab54c8430de029a4aacf61d5d923/xmlnote/573110A2DAAE46D5A35C930D81B36505/10952)

> new 年轻代：年轻代用来存放JVM刚分配的Java对象

    Eden：Eden用来存放JVM刚分配的对象
    Survivor1：
    Survivro2：两个Survivor空间一样大，当Eden中的对象经过垃圾回收没有被回收掉时，会在两个Survivor之间来回Copy，当满足某个条件，比如Copy次数，就会被Copy到Tenured。显然，Survivor只是增加了对象在年轻代中的逗留时间，增加了被垃圾回收的可能性。

> Tenured 年老代：年轻代中经过垃圾回收没有回收掉的对象将被Copy到年老代

> Perm 永久代：永久代存放Class、Method元信息，其大小跟项目的规模、类、方法的量有关，一般设置为128M就足够，设置原则是预留30%的空间
---

### 1.6、设计模式

#### 1.6.1 常见的设计模式

- [常用的设计模式](https://www.jianshu.com/p/bdf65e4afbb0)

```
    1. 策略模式
    2. 简单工厂模式
    3. 工厂模式
    4. 抽象工厂模式
    5. 装饰者模式
    6. 代理模式
    7. 模板方法模式
    8. 外观模式
    9. 适配器模式
    10. 桥接模式
    11. 建造者模式
    12. 观察者模式
    13. 单例模式
    14. 命令模式

```
#### 1.6.2 设计模式的的六大原则及其含义
```

```
#### 1.6.3 常见的单例模式以及各种实现方式的优缺点，哪一种最好，手写常见的单利模式
```java
//在类库: java.lang.RunTime
//单例模式: getRunTime();

//推荐！ 懒汉模式
class Instance{
    // 私有化构造方法
    // Singleton类的构造函数应该是私有的，因此它阻止从外部创建Singleton实例
    private Instance(){}
    // 申明一个static volatile的变量
    private volatile static Instance ins = null;
    // 实例化---使用双检测!
    public static Instance getInstance(){
        if (ins == null){
            synchronized(Instance.class){
                if (ins == null){
                    ins = new Instance();
                }
            }
        }
        return ins;
    }
}
```

#### 1.6.4 设计模式在实际场景中的应用
#### 1.6.5 Spring中用到了哪些设计模式
#### 1.6.6 MyBatis中用到了哪些设计模式
#### 1.6.7 你项目中有使用哪些设计模式
#### 1.6.8 说说常用开源框架中设计模式使用分析
#### 1.6.9 动态代理很重要！！！
- [java的动态代理机制详解](http://www.cnblogs.com/xiaoluo501395377/p/3383130.html)
```

```

---

### 1.7、数据结构

#### 1.7.1 最大堆、最小堆
```
  1. 当父节点的键值总是大于或等于任何一个子节点的键值时为最大堆。

    100
    / \
  89  98
 / \
23  30

  2. 当父节点的键值总是小于或等于任何一个子节点的键值时为最小堆。

    23
   /  \
  30  89
     /  \
    98  100
```
#### 1.7.2 树（二叉查找树、平衡二叉树、红黑树、B树、B+树）
```
    红黑树的属性:
    1). 每个节点都有红色或黑色。
    2). 树的根始终是黑色。
    3). 没有两个相邻的红色节点（红色节点不能有红色父节点或红色子节点）。
    4. 从根节点到NULL节点的每条路径都有相同数量的黑色节点
```

```

```
#### 1.7.3 深度有限算法、广度优先算法
#### 1.7.4 克鲁斯卡尔算法、普林母算法、迪克拉斯算法
#### 1.7.5 什么是一致性Hash（见分布式一节）
```
    Consistent Hashing：一致性哈希算法是分布式系统中重要的路由算法。
```
#### 1.7.6 常见的排序算法和查找算法：快排、折半查找、堆排序等
```
```
---

### 1.8、网络/IO基础

#### 1.8.1 BIO、NIO、IO Multiplexing、AIO的概念

- ![iamge](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/F00204E8348649A19007AF9AC77F0950/11076)
```
对于一个network io，以read为例，分为两个阶段：
    1). 等待数据准备(把外部数据加载到内核的缓冲区)；
    2). 将数据从内核拷贝到进程中(把内核的缓冲数据拷贝到进程的缓存区)。

1. BIO：Blocking IO        阻塞IO 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销

        a. 用户进程发起recvfrom系统调用，内核执行该系统调用。
        b. 内核等待数据就绪。
        c. 数据就绪。
        d. 内核将数据从内核缓冲区拷贝到用户进程缓冲区，返回成功。
        e. 用户进程从用户进程缓冲区读取数据，处理数据，接着往下执行。
        
        从中这个过程中我们可以观察到，用户进程在发起recvfrom系统调用后，直到数据被内核从内核缓冲区拷贝到用户进程缓冲区这整个过程中，都无法继续执行，处于被阻塞的状态，所以这是一个阻塞(Blocking)I/O。而且我们发现上面的I/O过程在内核部分分为两个步骤：

        内核等待数据就绪（阻塞）
        内核将数据从内核缓冲区拷贝到用户进程缓冲区（阻塞）
        阻塞I/O在这两个步骤上都被阻塞了。



2. NIO：Nonblocking IO     非阻塞IO

        a. 用户进程发起recvfrom系统调用，内核执行该系统调用，并直接返回一个数据未就绪状态给用户进程，然后内核等待数据就绪。
        b. 在内核等待数据就绪过程中，用户进程不断发起recvfrom系统调用（轮询），向内核查询数据是否就绪了，如果数据还未就绪，内核还是返回一个数据未就绪状态给用户进程。
        c. 内核的数据就绪了，内核将数据从内核缓冲区拷贝到用户进程缓冲区，并返回成功。
        d. 用户进程从用户进程缓冲区读取数据，处理数据，接着往下执行。

        这里和阻塞I/O的不同点在于，用户进程发起recvfrom系统调用后，内核会直接返回，如果数据未就绪就返回未就绪的状态，如果数据就绪就从内核缓冲区拷贝数据到用户进程缓冲区中。对于I/O过程中内核执行阶段的两个部分：

        内核等待数据就绪（非阻塞）
        内核将数据从内核缓冲区拷贝到用户进程缓冲区（阻塞）
        我们发现，在第一阶段，用户进程不会被阻塞，但在第二阶段用户进程被阻塞了。



3. IO Multiplexing     多路复用IO
        a. select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。
        b. 它的基本原理就是select/epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。


4. AIO：Asynchronous IO    异步IO

        a. 用户进程发起异步I/O系统调用，内核直接返回并执行系统调用，用户进程此时可以继续往下执行而不会被阻塞。
        b. 内核等待数据就绪。
        c. 内核的数据就绪了，内核将数据从内核缓冲区拷贝到用户进程缓冲区，并发送信号通知用户进程数据可用。
        d. 用户进程的信号处理程序处理数据。
        
        异步I/O过程中，用户进程从头到尾都完全没有被阻塞。

一个比喻：
肚子饿了要吃饭
自己去烧火做饭，饭熟了，再吃饭。BIO
普通电饭煲，帮你烧饭，但要你不停的观察有没有熟。NIO
智能电饭煲自己烧饭，饭熟了，你可以去做其他的事情，好了就通知你吃饭。AIO

```
#### 1.8.2 java NIO与AIO
```java

java NIO编程中，需要理解以下3个对象Channel、Buffer和Selector

Channel
    - Channel 和 IO中的Stream 是差不多一个等级的。只不过Stream是单向的，譬如：InputStream, OutputStream。而Channel是双向的，既可以用来进行读操作，又可以用来进行写操作。
    - NIO中的Channel的主要实现有：FileChannel、DatagramChannel、SocketChannel、ServerSocketChannel；通过看名字分别可以对应文件IO、UDP和TCP（Server和Client）。

Buffer
    - ByteBuffer、CharBuffer、DoubleBuffer、 FloatBuffer、IntBuffer、 LongBuffer、ShortBuffer；分别对应基本数据类型: byte、char、double、 float、int、 long、 short。当然NIO中还有MappedByteBuffer, HeapByteBuffer, DirectByteBuffer（通过full gc来回收内存的）。

Selector
    - Selector 是NIO相对于BIO实现多路复用的基础，Selector 运行单线程处理多个 Channel，如果你的应用打开了多个通道，但每个连接的流量都很低，使用 Selector 就会很方便。例如在一个聊天服务器中。
    - 要使用 Selector , 得向 Selector 注册 Channel，然后调用它的 select() 方法。这个方法会一直阻塞到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如新的连接进来、数据接收等。



AIO就是Java作为对异步IO提供支持的NIO.2
    - 从编程模式上来看AIO相对于NIO的区别在于，NIO需要使用者线程不停的轮询IO对象，来确定是否有数据准备好可以读了，而AIO则是在数据准备好之后，才会通知数据使用者，这样使用者就不需要不停地轮询了。
    - 当然AIO的异步特性并不是Java实现的伪异步，而是使用了系统底层API的支持，在Unix系统下，采用了epoll IO模型，而windows便是使用了IOCP模型。

```

#### 1.8.3 什么是长连接和短连接
```
1. 通常的短连接操作步骤是： 
连接→数据传输→关闭连接

2. 长连接通常就是： 
连接→数据传输→保持连接(心跳)→数据传输→保持连接(心跳)→……→关闭连接

3. 这就要求长连接在没有数据通信时，定时发送数据包(心跳)，以维持连接状态，短连接在没有数据传输时直接关闭就行了

如果Http使用长连接头部会包含: `Connection:keep-alive`
```

#### 1.8.4 Http1.0和2.0相比有什么区别，可参考《Http 2.0》
```
HTTP的长连接和短连接本质上是TCP长连接和短连接。HTTP属于应用层协议，在传输层使用TCP协议，在网络层使用IP协议.


```
#### 1.8.5  Https的基本概念
#### 1.8.6  三次握手和四次挥手、为什么挥手需要四次


#### 1.8.7 从游览器中输入URL到页面加载的发生了什么？可参考《从输入URL到页面加载发生了什么》


---


## 二、数据存储和消息队列

### 2.1、数据库
#### 2.1.1 索引的优缺点
```
索引的优点
    1. 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。
    2. 可以大大加快数据的检索速度，这也是创建索引的最主要的原因。
    3. 可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。
    4. 在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。
    5. 通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。

索引的缺点
    1. 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。
    2. 索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。
    3. 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。
```

#### 2.1.2 什么是二级索引(辅助索引)?

- ![image](https://note.youdao.com/yws/public/resource/798718b1c1e158bf29932d73a284cd15/xmlnote/9AFABD2BBEF246F69AF0723E2EDF07B2/11253)
```
如果表上定义有主键，该主键索引就是聚簇索引。如果未定义主键，MySQL取第一个唯一索引（unique）而且只含非空列（NOT NULL）作为主键，InnoDB使用它作为聚簇索引。如果没有这样的列，InnoDB就自己产生一个这样的ID值，它有六个字节，而且是隐藏的，使其作为聚簇索引。  表中的聚簇索引（clustered index ）就是一级索引，除此之外，表上的其他非聚簇索引都是二级索引，又叫辅助索引（secondary indexes）。

~~--~~--~~--~~--~~--~~

辅助索引: 复合索引、前缀索引、唯一索引.

复合索引：由多列创建的索引称为符合索引，在符合索引中的前导列必须出现在where条件中，索引才会被使用
ALTER TABLE `test`.`users` ADD INDEX `idx_users_id_name` (`name`(10) ASC, `id` ASC) ;

前缀索引：对BLOB和TEXT列进行索引，或者非常长的VARCHAR列，就必须使用前缀索引。为了减小索引体积，提高索引的扫描速度，就用索引的前部分字串索引。

唯一索引：索引值必须唯一，这样的索引选择性是最好的。主键一定是唯一性索引，唯一性索引并不一定就是主键。

~~--~~--~~--~~--~~--~~

1. InnoDB的的二级索引的叶子节点存放的是KEY字段加主键值。因此，通过二级索引查询首先查到是主键值，然后InnoDB再根据查到的主键值通过主键索引找到相应的数据块。

2. MyISAM的二级索引叶子节点存放的还是列值与行号的组合，叶子节点中保存的是数据的物理地址。所以可以看出MYISAM的主键索引和二级索引没有任何区别，主键索引仅仅只是一个叫做PRIMARY的唯一、非空的索引，且MYISAM引擎中可以不设主键。
```

#### 2.1.3 MySQL 索引使用的注意事项
```sql
1. 最左匹配（B+树的特性）：在以通配符%和_开头作查询时，MySQL不会使用索引。

like 'admin%' --索引有效
like '%admin'/like '%admin%' --索引无效

2. 不要在列进行运算，导致索引失效
--不推荐
select * from user where YEAR(add_date) < 2007;
--推荐
select * from user where add_date < '2007-01-01';

3. 不使用 Not In 和 <> 操作

```

#### 2.1.4 DDL、DML、DCL分别指什么
```
  - DDL(Data Define Language) : truncate、drop 立即生效 不会放到rollback segment事务
  - DML(Data Maintain Language) : delete 事务提交后才生效
  - DCL
```

#### 2.1.5 explain命令
```
expain出来的信息有10列，分别是id(SQL执行的顺序)、select_type、table(表名称)、type(访问类型：All表示全表扫描)、possible_keys、key(没有索引时为Null)、key_len、ref、rows、Extra
```

#### 2.1.6 left join，right join，inner join
#### 2.1.7 数据库事物ACID（原子性、一致性、隔离性、持久性）
```
```

#### 2.1.8 事务的隔离级别: 读未提交、读已提交、可重复读、可序列化读
```
  InnoDB默认是可重复读的（REPEATABLE READ）
```
| 隔离级别                     | 脏读（dirty read） | 不可重复读（NonRepeatable read） | 幻读（phantorn read） |
| :--------------------------- | ------------------ | -------------------------------- | --------------------- |
| 未提交读（read uncommitted） | 可能               | 可能                             | 可能                  |
| 已提交读（read committed）   | 不可能             | 可能                             | 可能                  |
| 可重复读（repeatable read）  | 不可能             | 不可能                           | 可能                  |
| 可串行化（serializable）     | 不可能             | 不可能                           | 不可能                |

#### 2.1.9 脏读、幻读、不可重复读
```
  ① 脏读: 脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这个数据。

  ② 不可重复读:是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读。

  ④ 幻读:第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样。
```

#### 2.1.10 数据库的几大范式
```
第一范式(确保每列保持原子性)

第二范式(确保表中的每列都和主键相关)

第三范式(确保每列都和主键列直接相关,而不是间接相关)

~~--~~--~~--~~--~~--~~
第一范式是不可拆分
第二是完全依赖
第三消除传递依赖
```

#### 2.1.12 数据库常见的命令
```bash
create database name; #创建数据库 
use databasename; #选择数据库 
drop database name #直接删除数据库，不提醒 
show tables; #显示表 
describe tablename; #表的详细描述 
#select 中加上distinct去除重复字段 
mysqladmin drop databasename #删除数据库前，有提示。 
#显示当前mysql版本和当前日期 select version(),current_date;
#备份数据库
mysqldump -h host -u root -p dbname >dbname_backup.sql
#恢复数据库
mysqladmin -h myhost -u root -p create dbname

```
#### 2.1.13 说说分库与分表设计
```
    - 拆分其实又分垂直拆分和水平拆分：

    - 案例： 简单购物系统暂设涉及如下表：
    1.产品表（数据量10w，稳定）
    2.订单表（数据量200w，且有增长趋势）
    3.用户表 （数据量100w，且有增长趋势）
    以mysql为例讲述下水平拆分和垂直拆分，mysql能容忍的数量级在百万静态数据可以到千万

    垂直拆分：
      - 解决问题：表与表之间的io竞争
      - 不解决问题：单表中数据量增长出现的压力
      - 方案： 把产品表和用户表放到一个server上 订单表单独放到一个server上

    水平拆分：
      - 解决问题：单表中数据量增长出现的压力
      - 不解决问题：表与表之间的io争夺
      - 方案： 用户表通过性别拆分为男用户表和女用户表 ，订单表通过已完成和完成中拆分为已完成订单和未完成订单 ，产品表 未完成订单放一个server上 已完成订单表 和 男用户表放一个server上 女用户表放一个server上(女的爱购物)
```

#### 2.1.14 分库与分表带来的分布式困境与应对之策（如何解决分布式下的分库分表，全局表？）
```

```

#### 2.1.15 说说 SQL 优化之道
```sql
  1）应尽量避免在 where 子句中使用!=或<>操作符，否则引擎放弃使用索引而进行全表扫描。
  2）应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如：select id from t where num is null可以在num上设置默认值0，确保表中num列没有null值，然后这样查询：select id from t where num=0
  3）很多时候用 exists 代替 in 是一个好的选择
  4）用Where子句替换HAVING 子句 因为HAVING 只会在检索出所有记录之后才对结果集进行过滤
```

#### 2.1.16 MySQL遇到的死锁问题、如何排查与解决
```

```


#### 2.1.17 存储引擎的 InnoDB与MyISAM区别，优缺点，使用场景
```
InnoDB提供：
    1. ACID事务
    2. 行级锁定
    3. 外键约束
    4. 自动崩溃恢复
    5. 表压缩（读/写）
    6. 空间数据类型（无空间索引）
    7. 在InnoDB中，除TEXT和BLOB之外的所有数据最多可占用8,000个字节。
    8. 在MySQL 5.6（2013年2月）之前，InnoDB中没有全文索引。在InnoDB中，COUNT(*)s（何时WHERE，GROUP BY或未JOIN使用）执行速度比MyISAM慢，因为行计数不在内部存储。
    9. InnoDB将 数据 和 索引 存储在一个文件中。InnoDB使用缓冲池来缓存数据和索引。

MyISAM提供：
    1. 快速COUNT(*)s（何时WHERE，GROUP BY或未JOIN使用）
    2. 全文索引（更新：来自MySQL 5.6的InnoDB支持）
    3. 更小的磁盘空间
    4. 非常高的表压缩（只读）
    5. 空间数据类型和索引（R-tree）（更新：来自MySQL 5.7的InnoDB支持）
    6. MyISAM具有表级锁定，但没有行级锁定。没有事务。没有自动崩溃恢复，但它确实提供了修复表功能。没有外键约束。
    7. 与InnoDB表相比，MyISAM表在磁盘上的大小通常更紧凑。如果需要，可以通过使用myisampack进行压缩来进一步大大减小MyISAM表的大小，但是变为只读。
    8. MyISAM将 索引 存储在一个文件中，数据 存储在另一个文件中。 MyISAM使用密钥缓冲区来缓存索引，并将数据缓存管理留给操作系统。
```

#### 2.1.18 索引类别（B+树索引、全文索引、哈希索引）、索引的原理
```
1. B+tree: 

2. FullText: Mysql 5.6（2013）版本之前只支持MyISAM引擎
    ①. 它的出现是为了解决WHERE name LIKE “%word%"这类针对文本的模糊查询效率较低的问题。在没有全文索引之前，这样一个查询语句是要进行遍历数据表操作。
    ②. FULLTEXT索引也是按照分词原理建立索引的。西文中，大部分为字母文字，分词可以很方便的按照空格进行分割。但很明显，中文不能按照这种方式进行分词。Mysql的中文分词插件Mysqlcft，有了它，就可以对中文进行分词。
3. Hash：
    ①. Hash 索引仅仅能满足"=","IN"和"<=>"查询，不能使用范围查询。
    ②. Hash 索引无法被用来进行数据的排序操作。例如：HashMap与有序的TreeMap
    🌂. 遇到大量Hash值相等的情况后性能低。
4. Rtree：
    ①. RTREE的优势在于范围查找，例如：地理空间范围。

```

#### 2.1.19 全文检索倒排索引(ES)

> 常见索引

| ID   | Docs               |
| :--- | ------------------ |
| 1    | apple linux google |
| 2    | blue google        |
| 3    | linux blue         |

> 倒排索引：跟正向的索引比较，也就是做了一个倒置，这就是倒排索引的思想

| Contents | ID  |
| :------- | --- |
| apple    | 1   |
| google   | 1,2 |
| blue     | 2,3 |
| linux    | 1,3 |
```
需要找到包含blue的文档，通过倒排索引很快找到2和3的 doc包含blue。
使用场景：
    1、全文搜索（搜索引擎）
    在一组文档中查找某一单词所在文档及位置

    2、模糊匹配
    通过用户的输入去匹配词库中符合条件的词条

    3、商品搜索
    通过商品的关键字去数据源中查找符合条件的商品
```



#### 2.1.20 [数据库常见问题](https://zhuanlan.zhihu.com/p/23713529)
```
```

#### 2.1.21 什么是自适应哈希索引（AHI）
```
InnoDB存储引擎会监控对表上各索引页的查询。如果观察到建立哈希索引可以带来速度提升，则建立哈希索引，称之为自适应哈希索引(Adaptive Hash Index, AHI)。AHI是通过缓冲池的B+树页构造而来，因此建立的速度很快，而且不需要对整张表构建哈希索引。InnoDB存储引擎会自动根据访问的频率和模式来自动地为某些热点页建立哈希索引。

```


#### 2.1.22 为什么要用 B+tree作为MySQL索引的数据结构？为什么走索引查询快？
```
一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储的磁盘上。这样的话，索引查找过程中就要产生磁盘I/O消耗，相对于内存存取，I/O存取的消耗要高几个数量级。而B-/+/*Tree，经过改进可以有效的利用系统对磁盘的块读取特性，在读取相同磁盘块的同时，尽可能多的加载索引数据，来提高索引命中效率，从而达到减少磁盘IO的读取次数。

```
#### 2.1.23 聚簇索引与非聚簇索引的区别
```
1. 聚簇索引的叶子节点就是数据节点。
2. 非聚簇索引的叶子节点仍然是索引节点，只不过有指向对应数据块的指针。

~~--~~--~~--~~

> 优点: 提高数据访问性能。
1. Innodb, 聚簇索引把索引和数据都保存到同一棵B+树数据结构中，并且同时将索引列与相关数据行保存在一起。这意味着，当你访问同一数据页不同行记录时，已经把页加载到了Buffer中，再次访问的时候，会在内存中完成访问，不必访问磁盘。
2. MyISAM，它将索引和数据没有放在一块，放在不同的物理文件中，索引文件是缓存在key_buffer中，索引对应的是磁盘位置，不得不通过磁盘位置访问磁盘数据。

> 缺点: 
1. 维护索引很昂贵，特别是插入新行或者主键被更新导至要分页(page split)的时候。
2. 如果使用UUID作为主键，使数据存储稀疏，这就会出现聚簇索引有可能有比全表扫面更慢，所以建议使用int的auto_increment作为主键。
3. 如果主键pk 比较大的话，那辅助索引将会变的更大，因为辅助索引的叶子存储的是主键值；过长的主键值，会导致非叶子节点占用占用更多的物理空间。


```

#### 2.1.24 遇到过索引失效的情况没，什么时候可能会出现，如何解决
```
1.如果条件中有or，即使其中有条件带索引也不会使用(这也是为什么尽量少用or的原因),要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引

2.对于多列索引，不是使用的第一部分，则不会使用索引 (假如有INDEX(a,b,c)，当条件为a或a,b或a,b,c时都可以使用索引，但是当条件为b,c时将不会使用索引)

3.like查询是以%开头

4.如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
```

#### 2.1.25 循环查询数据的性能问题及优化
```
1. 使用IN查询替换for循环
2. 使用聚合查询替换for循环
3. 使用pipeline来查询redis
```

#### 2.1.26 limit 20000 加载很慢怎么解决
```

```

#### 2.1.27 如何选择合适的分布式主键方案
```

```

#### 2.1.28 选择合适的数据存储方案
```

```
#### 2.1.29 常见的几种分布式ID的设计方案
  
> [常见的分布式id设计方式](https://gavinlee1.github.io/2017/06/28/%E5%B8%B8%E8%A7%81%E5%88%86%E5%B8%83%E5%BC%8F%E5%85%A8%E5%B1%80%E5%94%AF%E4%B8%80ID%E7%94%9F%E6%88%90%E7%AD%96%E7%95%A5%E5%8F%8A%E7%AE%97%E6%B3%95%E7%9A%84%E5%AF%B9%E6%AF%94/)
```
> 全局唯一
> 趋势有序

1. 使用Redis来生成ID。这主要依赖于Redis是单线程的，所以也可以用生成全局唯一的ID。可以用Redis的原子操作 INCR和INCRBY来实现。

2. 数据库auto_increment
优点：
此方法使用数据库原有的功能，所以相对简单
能够保证唯一性
能够保证递增性
id 之间的步长是固定且可自定义的

缺点：
可用性难以保证：数据库常见架构是 一主多从 + 读写分离，生成自增ID是写请求 主库挂了就玩不转了
扩展性差，性能有上限：因为写入是单点，数据库主库的写性能决定ID的生成性能上限，并且 难以扩展

改进方案：
冗余主库，避免写入单点
数据水平切分，保证各主库生成的ID不重复

3. 单点批量ID生成服务
分布式系统之所以难，很重要的原因之一是“没有一个全局时钟，难以保证绝对的时序”，要想保证绝对的时序，还是只能使用单点服务，用本地时钟保证“绝对时序”。

4. Twitter 开源的 Snowflake 算法SnowflakeIdGenerator 
```

#### 2.1.30 常见的数据库优化方案，在你的项目中数据库如何进行优化的
```
减少数据访问
    > 正确创建索引

返回更少的数据
    > 分页
    > 只返回需要的字段

减少交互的次数
    > jdbc支持batch的提交处理方法,采用batch操作一般不会减少很多数据库服务器的物理IO，但是会大大减少客户端与服务端的交互次数
    > 使用in select * from mytable where id in(id1,id2,...,idn);

减少数据库服务器CPU运算
    > 使用绑定变量 preparestatement
    > 合理使用排序
    > 减少比较
```

#### 2.1.31 Mysql和Oracle的区别
```
```
---

### 2.2、Redis
#### 2.2.1 Redis 有哪些数据类型，可参考《Redis常见的5种不同的数据类型详解》
```
String List Hash  Set  SortedSet
```

#### 2.2.2 Redis 内部结构

  [Redis内部结构](http://zhangtielei.com/posts/server.html)

  [Redis底层数据结构](https://nullcc.github.io/tags/Redis/page/2/)
```
  1. 第一个层面，是从使用者的角度，这一层面也是Redis暴露给外部的调用接口：
    string
    list
    hash
    set
    sorted set

  2.第二个层面，是从内部实现的角度，属于更底层的实现。比如：

    dict：（类似于Java HashMap数据结构）
      - dict本质上是为了解决算法中的查找问题（Searching），一般查找问题的解法分为两个大类：一个是基于各种平衡树，一个是基于哈希表。
      - dict也是一个基于哈希表的算法。和传统的哈希算法类似，它采用某个哈希函数从key计算得到在哈希表中的位置，采用拉链法解决冲突，并在装载因子（load factor）超过预定值时自动扩展内存，引发重哈希（rehashing）。

    sds：(Simple Dynamic String)
      - 可动态扩展内存。sds表示的字符串其内容可以修改，也可以追加。在很多语言中字符串会分为mutable和immutable两种，显然sds属于mutable类型的。
      - 二进制安全（Binary Safe）。sds能存储任意二进制数据，而不仅仅是可打印字符。
      - 与传统的C语言字符串类型兼容。

    robj：
      - 当我们执行Redis的set命令的时候，Redis首先将接收到的值值（string类型）表示成一个type = OBJ_STRING并且编码= OBJ_ENCODING_RAW的robj对象，然后在存入内部存储之前先执行一个编码过程，试图将它表示成另一种更节省内存的编码方式。这一过程的核心代码，是object.c中的tryObjectEncoding函数。
      - robj所表示的就是Redis对外暴露的第一层面的数据结构：string，list，hash，set，sorted set，而每一种数据结构的底层实现所对应的是哪个（或哪些）第二层面的数据结构（dict，sds，ziplist，quicklist，skiplist，等），则通过不同的编码来区分。可以说，robj是联结两个层面的数据结构的桥梁。

    ziplist：

    quicklist：
      - 一个ziplist的双向链表。
    skiplist
```
#### 2.2.3 Redis 使用场景
```
    RDB: 定时的持久机制，但在出现宕机时可能会出现数据丢失。
    AOF: 是基于操作日志的持久机制。
```
#### 2.2.4 Redis 集群方案与实现
```
1）Twitter开发的twemproxy

2）豌豆荚开发的codis

3）redis官方的redis-cluster
    使用去中心化的思想，用hash slot方式将16348个hash slot 覆盖到所有节点上，对于存储的每个key值，使用CRC16(KEY)&16348=slot得到对应的hash slot，并在访问key时就去找他的hash slot在哪一个节点上，然后由当前访问节点从实际被分配了这个hash slot的节点去取数据。节点之间使用轻量协议通信，减少带宽占用，性能很高 自动实现负载均衡与高可用，自动实现failover，并且支持动态扩展，官方已经玩到可以1000个节点，实现的复杂度低 

```
#### 2.2.5 Redis 为什么是单线程的？
```
1）绝大部分请求是纯粹的内存操作（非常快速）

2）采用单线程,避免了不必要的上下文切换和竞争条件

3）非阻塞IO

内部实现采用epoll，采用了epoll+自己实现的简单的事件框架。epoll中的读、写、关闭、连接都转化成了事件，然后利用epoll的多路复用特性，绝不在io上浪费一点时间

这3个条件不是相互独立的，特别是第一条，如果请求都是耗时的，采用单线程吞吐量及性能可想而知了。应该说redis为特殊的场景选择了合适的技术方案。
```
#### 2.2.6 缓存雪崩、缓存穿透、缓存预热、缓存更新、缓存降级
> https://blog.csdn.net/xlgen157387/article/details/79530877
#### 2.2.7 使用缓存的合理性问题
#### 2.2.8 Redis常见的回收策略

--- 

### 2.3、消息队列
#### 2.3.1 消息队列的使用场景
```
    1. 异步处理
    2. 应用解耦
    3. 流量削锋
    4. 消息通讯
```
#### 2.3.2 消息的重发补偿解决思路
#### 2.3.3 消息的幂等性解决思路
#### 2.3.4 消息的堆积解决思路
#### 2.3.5 自己如何实现消息队列
#### 2.3.6 如何保证消息的有序性


## 三、开源框架和容器

### 3.1、SSM/Servlet

#### 3.1.1 Servlet的生命周期

> Servlet的生命周期：
1. 实例化
2. 初始init
```
init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，用于 Servlet的初始化，初始化的数据，可以在整个生命周期中使用
```
3. 接收请求service:
```
service 方法会检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut、doDelete 方法
```
4. 销毁destroy

#### 3.1.2 转发与重定向的区别

> 转发：forward
    
- 共享相同的request对象和response对象，它们属于同一个访问请求和响应过程。
- 请求转发过程结束后，浏览器地址栏保持初始的URL地址不变。


> 重定向：Redirect
- 使用各自的request对象和response对象，它们属于两个独立的访问请求和响应过程。对于同一个WEB应用程序的内部资源之间的跳转。
- 浏览器地址栏中显示的URL会发生改变，由初始的URL地址变成重定向的目标URL；

#### 3.1.3 URL页面- 请求过程分析

> [URL过程](http://blog.csdn.net/wangjianyu0115/article/details/50830473)

> chrome://net-internals/#dns

#### 3.1.4 BeanFactory 和 ApplicationContext 有什么区别
```
Spring框架中，一旦把一个bean纳入到Spring IoC容器之中，这个bean的生命周期就会交由容器进行管理，一般担当管理者角色的是BeanFactory或ApplicationContext。

在Spring IOC容器的代表就是org.springframework.beans包中的BeanFactory接口，BeanFactory接口提供了IOC容器最基本功能；而org.springframework.context包下的ApplicationContext接口扩展了BeanFactory，还提供了与Spring AOP集成、国际化处理、事件传播及提供不同层次的context实现 (如针对web应用的WebApplicationContext)。简单说， BeanFactory提供了IOC容器最基本功能，而 ApplicationContext 则增加了更多支持企业级功能支持。
```
> BeanFactory：一个接口
```
是Spring里面最低层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能；
```
> ApplicationContext：
```
应用上下文，继承BeanFactory接口的接口，它是Spring的一更高级的容器，提供了更多的有用的功能；

1) 国际化（MessageSource）
2) 访问资源，如URL和文件（ResourceLoader）
3) 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层  
4) 消息发送、响应机制（ApplicationEventPublisher）
5) AOP（拦截器）
```

#### 3.1.5 Spring Bean 的生命周期
> 由IOC容器管理的那些组成你应用程序的对象我们就叫它Bean， Bean就是由Spring容器初始化、装配及管理的对象，除此之外，bean就与应用程序中的其他对象没有什么区别了。



#### 3.1.6 Spring IoC(DI) 如何实现
> [自己实现一个Ioc容器](https://blog.csdn.net/zhanghaor/article/details/73431021)
1. java反射机制
2. XML文件解析
3. 工厂模式 Map存储对象
```
Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

Spring的IoC的实现原理利用的就是Java的反射机制，Spring的BeanFactory会帮助我们完成配置文件的读取、利用反射机制注入对象等工作，我们还可以通过Bean的名称获取对象的对象。

IoC：控制反转，就是把原先我们代码里面需要实现的对象创建、依赖的代码，反转给容器来帮忙实现。那么必然的我们需要创建一个容器，同时需要一种描述来让容器知道需要创建的对象与对象的关系。这个描述最具体表现就是我们可配置的 xml ， properties 文件等语义化配置文件表示。
    
        1. 基于XML

        2. 基于注解
            - @Component
            - 开启scan

        3. 自动装配
            - @Repository
        4. 零配置

```
#### 3.1.7 Spring中Bean的作用域，默认的是哪一个
> 单例	`singleton`	整个应用中只创建一个实例(默认)
```
比较常用的是singleton和prototype两种作用域
```

> 原型	prototype	每次注入时都新建一个实例

> 会话	session	为每个会话创建一个实例，同样只有在Web应用中使用Spring时，该作用域才有效

> 请求	request	对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效


#### 3.1.8 说说 Spring AOP、Spring AOP 实现原理
> 1. 说说AOP
```
即面向切面编程。作为面向对象的一种补充，用于处理系统中分布于各个模块的横切关注点，比如事务管理、日志、缓存等等
```
> 2. 实现原理
```
动态代理主要有两种方式：
JDK动态代理和CGLIB动态代理。
```
#### 3.1.9 动态代理（CGLib 与 JDK）、优缺点、性能对比、如何选择

1. 代理模式
2. JDK动态代理
    - JDK动态代理所用到的代理类在程序调用到代理类对象时才由JVM真正创建，JVM根据传进来的 业务实现类对象 以及 方法名，动态地创建了一个代理类的class文件并被字节码引擎执行，然后通过该代理类对象进行方法调用。我们需要做的，只需指定代理类的预处理、调用后操作即可。
    - JDK动态代理的代理对象在创建时，需要使用业务实现类所实现的接口作为参数`（因为在后面代理方法时需要根据接口内的方法名进行调用）`。如果业务实现类是没有实现接口而是直接定义业务方法的话，就无法使用JDK动态代理了。并且，如果业务实现类中新增了接口中没有的方法，这些方法是无法被代理的（因为无法被调用）。
        
3. CGLIB
    - cglib是针对类来实现代理的，原理是对指定的业务类生成一个子类，并覆盖其中业务方法实现代理。因为采用的是`继承`，所以`不能对final修饰的类`进行代理。
        
4. 比较
    -  静态代理是通过在代码中显式定义一个业务实现类一个代理，在代理类中对同名的业务方法进行包装，用户通过代理类调用被包装过的业务方法；

    - JDK动态代理是通过接口中的方法名，在动态生成的代理类中调用业务实现类的同名方法；

    - CGlib动态代理是通过继承业务类，生成的动态代理类是业务类的子类，通过重写业务方法进行代理；

#### 3.1.10 Spring 事务实现方式、事务的传播机制、默认的事务类别

1. 实现方式
```
    a. 编程式事务管理(基于Java编程控制，很少使用)--利用TransactionTemplate将多个DAO操作封装起来

    *b. 声明式事务管理(基于Spring的AOP配置控制)-基于TransactionProxyFactoryBean的方式.(很少使用)--需要为每个进行事务管理的类,配置一个TransactionProxyFactoryBean进行增强.

    c. 基于XML配置(经常使用)--一旦配置好之后,类上不需要添加任何东西。如果Action作为目标对象切入事务，需要在<aop:config>元素里添加proxy-target-class="true"属性。原因是通知Spring框架采用CGLIB技术生成具有事务管理功能的Action类。

    d. 基于注解(配置简单，经常使用)--在applicationContext.xml中开启事务注解配置。
    (applicationContext.xml中只需定义Bean并追加以下元素)
        <bean id="txManager" class="...">
            <property name="sessionFactory">
            </property>
        <tx:annotation-driven transaction-manager="txManager"/>
    在目标组件类中使用@Transactional，该标记可定义在类前或方法前。
```


2. 传播机制：spring的 TransactionDefinition接口中一共定义了六种事务传播属性
    - `PROPAGATION_REQUIRED` -- 支持当前事务，如果当前没有事务，就新建一个事务。这是最`常见的选择`。
    - PROPAGATION_SUPPORTS -- 支持当前事务，如果当前没有事务，就以非事务方式执行。
    - PROPAGATION_MANDATORY -- 支持当前事务，如果当前没有事务，就抛出异常。
    - PROPAGATION_REQUIRES_NEW -- 新建事务，如果当前存在事务，把当前事务挂起。
    - PROPAGATION_NOT_SUPPORTED -- 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
    - PROPAGATION_NEVER -- 以非事务方式执行，如果当前存在事务，则抛出异常。


3. 默认事务类别
   
   - 在Spring中有针对传播特性的多种配置大多数情况下只用其中的一种PROPGATION_REQUIRED这个配置项的意思是说当我调用service层的方法的时候开启一个事务(具体调用那一层的方法开始创建事务，要看你的aop的配置),那么在调用这个service层里面的其他的方法的时候,如果当前方法产生了事务就用当前方法产生的事务，否则就创建一个新的事务。
   
   - 默认情况下当发生RuntimeException的情况下，事务才会回滚，如果你在程序发生错误的情况下，有自己的异常处理机制定义自己的Exception，`必须从RuntimeException类继承 这样事务才会回滚！`


#### 3.1.11 Spring 事务底层原理

a、划分处理单元——IOC：在配置文件设置事务管理器，设置事务的传播特性及隔离机制。

b、AOP拦截需要进行事务处理的类：用 `TransactionProxyFactoryBean` 接口来使用AOP功能，生成proxy代理对象，通过`TransactionInterceptor` 完成对代理方法的拦截，将事务处理的功能编织到拦截的方法中。读取ioc容器事务配置属性，转化为spring事务处理需要的内部数据结构（TransactionAttributeSourceAdvisor），转化为TransactionAttribute表示的数据对象
    
c、结合：PlatformTransactionManager实现了TransactionInterception接口，让其与TransactionProxyFactoryBean结合起来，形成一个Spring声明式事务处理的设计体系。

d. 数据库底层采用redo日志的形式实现日志。

#### Spring事务失效（事务嵌套），JDK动态代理给Spring事务埋下的坑，可参考《JDK动态代理给Spring事务埋下的坑！》
> [spring事务失效](https://blog.csdn.net/andybbc/article/details/52913525)
```
1. 不通过代理对象调用方法也会导致spring事务管理失效. 绕过代理对象最直接的方法就是自己 new MyService().save()。

2. private 方法, final 方法 和 static 方法不能添加事务
```
#### 如何自定义注解实现功能

#### Spring MVC 运行流程

> user --> dispatcher(派送)servlet[总调度]  --> handler mapping --> controller --> model and view --> 指定 的页面

1. 用户向服务器发送请求，请求被Spring 前端控制 `DispatcherServlet` 捕获；
2. `DispatcherServlet` 对请求URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain对象的形式返回；
3. `DispatcherServlet` 根据获得的Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(...)方法）
4. 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：
```
      HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
      数据转换：对请求消息进行数据转换。如String转换成Integer、Double等
      数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等
      数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中
```
5.  Handler执行完成后，向 `DispatcherServlet` 返回一个ModelAndView对象；
6.  根据返回的ModelAndView，选择一个适合的ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给`DispatcherServlet`
7.  ViewResolver 结合ModelAndView，来渲染视图
8.  将渲染结果返回给客户端。

#### Spring MVC 启动流程
1. Web容器初始化过程

> `ContextLoaderListener`类起着至关重要的作用。它读取web.xml中配置的context-param中的配置文件，提前在web容器初始化前准备业务对应的Application context;将创建好的Application context放置于ServletContext中，为springMVC部分的初始化做好准备。

2. SpringMVC中web.xml配置
> 在SpringMVC架构中，DispatchServlet负责请求分发，起到控制器的作用
3. ServletContextListener

4. 认识ContextLoaderListener

5. DispatcherServlet初始化（HttpServletBean • FrameworkServlet • DispatcherServlet）

6. ContextLoaderListener与DispatcherServlet关系

7. DispatcherServlet的设计

8. DispatcherServlet工作原理

#### Spring 的单例实现原理
> Spring框架对单例的支持是采用单例注册表的方式进行实现的: HashMap+Protected RegSingleton()构造方法

```java
 public class SpringSinglePattern {

    private static HashMap<String, SpringSinglePattern> register = new HashMap();

    //受保护的默认构造函数，如果为继承关系，则可以调用，
    // 克服了单例类不能为继承的缺点
    protected SpringSinglePattern() {

    }

    //静态代码块在类启动是被加载
    static {
        SpringSinglePattern ins = new SpringSinglePattern();
        register.put(ins.getClass().getName(), ins);
    }

    //静态工厂方法返回唯一实例
    public static SpringSinglePattern getInstance(String name) throws Exception {

        if (name == null) {
            name = "SpringSinglePattern";
        }
        if (register.get(name) == null) {
            register.put(name, (SpringSinglePattern) Class.forName(name).newInstance());
        }
        return register.get(name);
    }
}

```
#### Spring 框架中用到了哪些设计模式
```
解释器，构建器，工厂方法和抽象工厂
```
#### Spring 其他产品（Srping Boot、Spring Cloud、Spring Secuirity、Spring Data、Spring AMQP 等）
#### 有没有用到Spring Boot，Spring Boot的认识、原理

#### 3.1.12 MVC设计思想
```
Model，View，Controller分离，把较为复杂的web应用分成逻辑清晰的几部分，简化开发。

M（Model）：主要功能提供数据（主要用来提供数据，并不关心数据让谁显示（Controller 负责给M要数据，然后控制数据让哪一个View来显示））；
V（View）：主要功能是展示数据（主要有数据即可，不关心数据来源）；
C（Controller）：主要功能协调V层与M层，作为V层与M层沟通的桥梁。
```

#### 3.1.13 MyBatis的原理
[参考](https://www.cnblogs.com/luoxn28/p/6417892.html)
```
MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架，其主要就完成2件事情：

封装JDBC操作
利用反射打通Java类与SQL语句之间的相互转换
```

#### 3.1.14 Mybaits的缓存机制：分为两级：一级缓存、二级缓存
> 一级缓存

1. 一级缓存是SqlSession级别的缓存，缓存的数据只在SqlSession内有效

2. 使用一个private HashMap<Object,Object> 存储对象

3. 具体流程：

        1. 第一次执行select完毕会将查到的数据写入SqlSession内的HashMap中缓存起来

        2. 第二次执行select会从缓存中查数据，如果select相同切传参数一样，那么就能从缓存中返回数据，不用去数据库了，从而提高了效率

4. 注意事项：

        1. 如果SqlSession执行了DML操作（insert、update、delete），并commit了，那么mybatis就会清空当前SqlSession缓存中的所有缓存数据，这样可以保证缓存中的存的数据永远和数据库中一致，避免出现脏读

        2. 当一个SqlSession结束后那么他里面的一级缓存也就不存在了，mybatis默认是开启一级缓存，不需要配置
 

> 二级缓存

1. 二级缓存是mapper级别的缓存，也就是同一个namespace的mappe.xml，当多个SqlSession使用同一个Mapper操作数据库的时候，得到的数据会缓存在同一个二级缓存区域。
2. 二级缓存默认是没有开启的。需要在setting全局参数中配置开启二级缓存。
```
conf.xml：
<settings>
        <setting name="cacheEnabled" value="true"/>默认是false：关闭二级缓存
<settings>

在userMapper.xml中配置：
<cache eviction="LRU" flushInterval="60000" size="512" readOnly="true"/>当前mapper下所有语句开启二级缓存
```

3. 当一个sqlseesion执行了一次select后，在关闭此session的时候，会将查询结果缓存到二级缓存

4. 当另一个sqlsession执行select时，首先会在他自己的一级缓存中找，如果没找到，就回去二级缓存中找，找到了就返回，就不用去数据库了，从而减少了数据库压力提高了性能　

5. 注意事项：

        1. 如果SqlSession执行了DML操作（insert、update、delete），并commit了，那么mybatis就会清空当前mapper缓存中的所有缓存数据，这样可以保证缓存中的存的数据永远和数据库中一致，避免出现脏读

        2. mybatis的缓存是基于[namespace:sql语句:参数]来进行缓存的，意思就是，SqlSession的HashMap存储缓存数据时，是使用[namespace:sql:参数]作为key，查询返回的语句作为value保存的。例如：-1242243203:1146242777:winclpt.bean.userMapper.getUser:0:2147483647:select * from user where id=?:19


#### spring知识

> http://blog.csdn.net/jaryle/article/details/51219062
    
>http://blog.csdn.net/springsen/article/details/7304270
    
>http://blog.csdn.net/u013256816/article/details/51386182


```
    可参考《为什么会有Spring》
    可参考《为什么会有Spring AOP》
```
---

### 3.2、Netty


#### 3.2.1 Netty实现原理浅析
```
1. NIO
    Buffer      缓冲区
    Channel     通道
    Selector    多路复用
2. Reactor编程模型

3. 
```
#### 3.2.2 为什么选择 Netty

[如何使用API](http://tutorials.jenkov.com/netty/netty-channelpipeline.html)
```
Netty是一个提供`异步事件驱动`的`网络`应用框架，用以快速开发高性能、高可靠性的网络服务器和客户端程序。
```

#### 3.2.3 说说业务中，Netty 的使用场景
```
1. RPC的远程调用的网络请求：Dubbo
2. RibbitMQ/Hadoop节点的通信
3. 通信行业
4. 游戏行业：本身提供了 TCP/UDP 和 HTTP 协议栈，非常方便定制和开发私有协议栈。账号登陆服务器、地图服务器之间可以方便的通过 Netty 进行高性能的通信。
```
#### 3.2.4 原生的 NIO 在 JDK 1.7 版本存在 epoll bug
```
epoll bug，它会导致Selector空轮询，最终导致CPU 100%
```
#### 3.2.5 Netty 线程模型 reactor

1. Netty使用`EventLoop`来处理连接上的读写事件，而一个连接上的所有请求都保证在一个EventLoop中被处理，`一个EventLoop中只有一个Thread`，所以也就实现了一个连接上的所有事件只会在一个线程中被执行。一个`EventLoopGroup`包含`多个EventLoop`，可以把一个EventLoop当做是Reactor线程模型中的一个线程，而一个EventLoopGroup类似于一个ExecutorService。
2. Netty的服务端使用了`两个EventLoopGroup`，而第一个EventLoopGroup通常只有一个EventLoop，通常叫做`bossGroup，负责客户端的连接请求`，然后打开Channel，交给`后面的EventLoopGroup中的一个EventLoop来负责这个Channel上的所有读写事件`，一个Channel只会被一个EventLoop处理，而一个EventLoop可能会被分配给多个Channel来负责上面的事件。

```
EventLoop继承了Java的ScheduledExecutorService，也就是调度线程池。
```

[参考](https://www.jianshu.com/p/38b56531565d)
[链接](https://www.jianshu.com/p/128ddc36e713)


#### 3.2.6 说说 Netty 的零拷贝

> Linux中的sendfile()以及Java NIO中的FileChannel.transferTo()方法都实现了零拷贝的功能，而在Netty中也通过在`FileRegion`中包装了NIO的FileChannel.transferTo()方法实现了零拷贝。


#### 3.2.7 Netty 内部执行流程

> 服务端依次发生的步骤
1. 建立服务端监听套接字ServerSocketChannel，以及对应的管道pipeline；
2. 启动boss线程，将ServerSocketChannel注册到boss线程持有的selector中，并将注册返回的selectionKey赋值给ServerSocketChannel关联的selectionKey变量；
3. 在ServerSocketChannel对应的管道中触发channelRegistered事件；
4. 绑定IP和端口
5. 触发channelActive事件，并将ServerSocketChannel关联的selectionKey的OP_ACCEPT位置为1。
6. 客户端发起connect请求后，boss线程正在运行的select循环检测到了该ServerSocketChannel的ACCEPT事件就绪，则通过accept系统调用建立一个已连接套接字SocketChannel，并为其创建对应的管道；
7. 在服务端监听套接字对应的管道中触发channelRead事件；
8. channelRead事件由ServerBootstrapAcceptor的channelRead方法响应：为已连接套接字对应的管道加入ChannelInitializer处理器；启动一个worker线程，并将已连接套接字的注册任务加入到worker线程的任务队列中；
9. worker线程执行已连接套接字的注册任务：将已连接套接字注册到worker线程持有的selector中，并将注册返回的selectionKey赋值给已连接套接字关联的selectionKey变量；10. 在已连接套接字对应的管道中触发channelRegistered事件；channelRegistered事件由ChannelInitializer的channelRegistered方法响应：将自定义的处理器（譬如EchoServerHandler）加入到已连接套接字对应的管道中；在已连接套接字对应的管道中触发channelActive事件；channelActive事件由已连接套接字对应的管道中的inbound处理器的channelActive方法响应；将已连接套接字关联的selectionKey的OP_READ位置为1；至此，worker线程关联的selector就开始监听已连接套接字的READ事件了。
在worker线程运行的同时，Boss线程接着在服务端监听套接字对应的管道中触发channelReadComplete事件。
客户端向服务端发送消息后，worker线程正在运行的selector循环会检测到已连接套接字的READ事件就绪。则通过read系统调用将消息从套接字的接受缓冲区中读到AdaptiveRecvByteBufAllocator（可以自适应调整分配的缓存的大小）分配的缓存中；
在已连接套接字对应的管道中触发channelRead事件；
channelRead事件由EchoServerHandler处理器的channelRead方法响应：执行write操作将消息存储到ChannelOutboundBuffer中；
在已连接套接字对应的管道中触发ChannelReadComplete事件；
ChannelReadComplete事件由EchoServerHandler处理器的channelReadComplete方法响应：执行flush操作将消息从ChannelOutboundBuffer中flush到套接字的发送缓冲区中；


#### 3.2.8 Netty 重连实现

> [netty代码实现](https://segmentfault.com/a/1190000007432946)

> 使用Netty提供的IdleStateHandler来检测读写操作的空闲时间

> 使用Protocol Buffer序列化

> 客户端write空闲5s后向服务端发送一个心跳包

> 服务端read空闲6s后心跳丢失计数器+1（丢失的心跳包数量）

> 当丢失的心跳包数量超过3个时，主动断开该客户端的channel

> 断开连接后，客户端10s之后重新连接

#### 3.2.9 什么是TCP 粘包/拆包
```
```

#### 3.2.10 TCP粘包/拆包的解决办法
```
```
---

### 3.3、Tomcat
#### 3.3.1 Tomcat的基础架构（Server、Service、Connector、Container）
#### 3.3.2 Tomcat如何加载Servlet的
#### 3.3.3 Pipeline-Valve机制
#### 3.3.4 可参考：《四张图带你了解Tomcat系统架构！》

---

## 四、分布式

### 4.1、Nginx
#### 4.1.1 请解释什么是C10K问题或者知道什么是C10K问题吗？
```
C10K就是 Client 10000 问题:[在同时连接到服务器的客户端数量超过 10000 个的环境中，即便硬件性能足够， 依然无法正常提供服务。简而言之，单机1万并发。]
```
#### 4.1.2 Nginx简介，可参考《Nginx简介》
#### 4.1.3 正向代理和反向代理

- ![image](https://note.youdao.com/yws/public/resource/6529ab54c8430de029a4aacf61d5d923/xmlnote/DA2A2F90935D4378A684934F591F4FBC/11056)
```
    1. 正向代理：设置代理服务器ip或者域名进行访问，由设置的服务器ip或者域名去获取访问内容并返回。
    2. 反向代理：客户端请求服务器，服务器内部会自动根据访问内容进行分配，内容返回，你不知道它最终访问的是哪些机器。
        反向代理的作用比较多了，这里简单列举一下：

            - 保护和隐藏原始资源服务器
            - 加密和SSL加速
            - 负载均衡
            - 缓存静态内容
            - 压缩
            - 减速上传
            - 安全
            - 外网发布
```
#### 4.1.4 Nginx几种常见的负载均衡策略
```
    1. 轮询策略：将每个前端请求按顺序（时间顺序和排列次序）逐一分配到不同的后端服务器节点。
    2. 加权轮询策略：在基本的轮询策略基础上考虑各后端服务器节点接受请求的权重，指定各后端服务器节点被轮询到的机率，主要应用于后端服务器节点性能不均的情况。
    3. ip_hash策略：将前端的访问IP进行hash操作，然后根据hash结果将请求分配到不同的后端服务器节点。这样会使得每个前端访问IP会固定访问一个后端服务器节点，好处是前端用户的session只在一个后端服务器节点上，不必考虑一个session存在多台服务器节点出现session共享问题。
```
#### 4.1.5 Nginx服务器上的Master和Worker进程分别是什么
```
master process
    监控进程，也叫做主进程：不需要处理网络事件，不负责业务的执行，只会通过管理worker进程来实现重启服务、平滑升级、更换日志文件、配置文件实时生效等功能。    
woker process（工作进程），还可能有cache相关进程。
    主要任务是完成具体的任务逻辑

```
#### 4.1.6 使用“反向代理服务器”的优点是什么?
```
见反向代理的作用。
```
---

### 4.2、分布式及其他
#### 4.2.1 谈谈业务中使用分布式的场景,以及优缺点
```
    1. 优点：
        - 传统项目模块之间耦合太高，升级其中之一的功能部署，其他功能都要一起升级部署。分布式服务可以降低项目模块的耦合，提高代码复用性。
        - 传统项目扩展性差。分布式系统扩展性高。
        - 传统项目整合困难。分布式系统整合简单，相互调用接口。
        - 可以灵活部署。
    2. 缺点：
        - 基于网络接口通信，加重网络io。
        - 分布式数据库体系和分布式事务复杂。
```
#### 4.2.2 Session 分布式方案
```
    1. 服务器session复制
        原理：任何一个服务器上的session发生改变（增删改），该节点会把这个 session的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要session，以此来保证Session同步。
        优点：可容错，各个服务器间session能够实时响应。
        缺点：会对网络负荷造成一定压力，如果session量大的话可能会造成网络堵塞，拖慢服务器性能。
        实现方式：设置tomcat的 server.xml 开启tomcat集群功能。
    
    2. session共享机制
        使用分布式缓存方案比如memcached、Redis，但是要求Memcached或Redis必须是集群。在springcloud微服务项目中，使用redis实现session共享是比较主流的。
```
#### 4.2.3 分布式锁的应用场景、分布式锁的产生原因、基本概念
> 应用场景：电商的订单系统、秒杀系统

> 比较敏感的数据比如金额修改，同一时间只能有一个人操作，想象下2个人同时修改金额，一个加金额一个减金额，为了防止同时操作造成数据不一致，需要锁，如果是数据库需要的就是行锁或表锁，如果是在集群里，多个客户端同时修改一个共享的数据就需要分布式锁。

> 基本概念
```
互斥性：和我们本地锁一样互斥性是最基本，但是分布式锁需要保证在不同节点的不同线程的互斥。

可重入性：同一个节点上的同一个线程如果获取了锁之后那么也可以再次获取这个锁。

锁超时：和本地锁一样支持锁超时，防止死锁。

高效，高可用：加锁和解锁需要高效，同时也需要保证高可用防止分布式锁失效，可以增加降级。

支持阻塞和非阻塞：和 ReentrantLock 一样支持 lock 和 trylock 以及 tryLock(long timeOut)。

支持公平锁和非公平锁(可选)：公平锁的意思是按照请求加锁的顺序获得锁，非公平锁就相反是无序的。这个一般来说实现的比较少
```

#### 4.2.4 分布是锁的常见解决方案    
    
- [REDIS分布式锁](http://www.importnew.com/27477.html)

```java
  1. 数据库乐观锁
    - 行级锁: 查询语句后面增加for update，执行业务逻辑后connection.commit()操作来释放锁。数据库会在查询过程中给数据库表增加排他锁，InnoDB引擎在加锁的时候，只有通过索引进行检索的时候才会使用行级锁，否则会使用表级锁。
    - 实现分布式锁，最简单的方式可能就是直接创建一张锁表，然后通过操作该表中的数据来实现了。当我们要锁住某个方法或资源时，我们就在该表中增加一条记录，想要释放锁的时候就删除这条记录。

  2. 基于Redis的分布式锁；
    - setnx(set if not exist) 、expire 两条操作不是原子性；建议使用jedis.set(lockKey, requestId, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);

    - 释放锁：
    String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
    Object result = jedis.eval(script, Collections.singletonList(lockKey), Collections.singletonList(requestId));

  3. 基于ZooKeeper的分布式锁。
    - 使用开源第三方库框架：org.apache.curator
    - 使用zookeeper实现分布式锁的算法流程，假设锁空间的根节点为/lock：
        //临时顺序节点
        String path = zookeeper.create(root + "/" + prefix, data, ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.EPHEMERAL_SEQUENTIAL);
        a. 客户端连接zookeeper，并在/lock下创建`临时`的且`有序`的子节点，第一个客户端对应的子节点为/lock/lock-0000000000，第二个为/lock/lock-0000000001，以此类推。
        b. 客户端获取/lock下的子节点列表，判断自己创建的子节点是否为当前子节点列表中序号最小的子节点，如果是则认为获得锁；否则监听/lock的子节点变更消息，获得子节点变更通知后重复此步骤直至获得锁；
        c. 执行业务代码；
        d. 完成业务流程后，删除对应的子节点释放锁。
        
```

#### 4.2.5 数据库的事务的隔离级别：
```
    Read uncommitted(读未提交)
    Read committed(读已提交) 优先采取
    Repeatable read(重复读)
    Serializable(序列化)
```

#### 4.2.6 一致性Hash及其原理、Hash环问题

[一致性hash原理](http://afghl.github.io/2016/07/04/consistent-hashing.html)
```
1. 普通Hash方式
        - Hash(object) % 节点数；当节点数变化时，所有的object都要进行重新计算放在新的位置。

2. 一致性Hash
        - 把object求hash：var key = Hash(object) % 节点数。
        - cache机器根据ip或其他求hash，然后把object和cache的hash值放入一个hash环空间，通过一定的规则决定每个object落在哪一个cache中。

```
```
1. 比如有4个需要存储的object，先求出它们的hash值，根据hash值映射到环上。
```
- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/66C38BE9BC4542759C4C502ADBA5F81E/11059)

```
2. 假设有三台cache服务器：cache A，cache B，cache C。用同样的方法求出hash值（可根据机器的IP或名字作为key求hash，只要保证hash值足够分散），映射到同一个环上。
```
- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/CFD3E26B3BCA43FBAB54BAE42450DB78/11062)

```
3. 让object在环上顺时针转动，遇到的第一个cache即为对应的cache服务器。根据上面的方法，对object1将被存储到cache A上；object2和object3对应到cache C；object4对应到cache B。

4. 一致性hash算法成功解决了cache服务器增减时key的失效问题。现在，无论增减cache，只有部分key失效。
```
```
理论上，只要cache足够多，每个cache在圆环上就会足够分散。但是在真实场景里，cache服务器只会有很少，所以，引入了“虚拟节点”（virtual node）的概念：

以仅部署cache A和cache C的情况为例，引入虚拟节点，cache A1, cache A2代表了cache A；cache C1，cache C2代表了cache C。

此时，对象到“虚拟节点”的映射关系为：
objec1->cache A2；objec2->cache A1；objec3->cache C1；objec4->cache C2；
```
- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/2E7095D0FB4F4DD8B84BBB186D48535E/11064)

```
5. 虚拟节点
hash算法的一个考量指标是平衡性。在本例中，我们希望每一个object落在任意一个cache的机会都尽可能接近
```
- ![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/DAD17270836544C2AEFCFC951E9CC0A5/11066)

#### 4.2.7 分布式事务的常见解决方案
- [分布式事务解决方案](https://juejin.im/post/5aa3c7736fb9a028bb189bca#heading-13)

> 采用微服务架构，业务系统拥有独立的数据库，因此就出现了跨多个数据库的事务需求，这种事务即为“分布式事务”。
```
分布式服务的事务

 这里举一个分布式事务的典型例子——用户下单过程。
    当我们的系统采用了微服务架构后，一个电商系统往往被拆分成如下几个子系统：商品系统、订单系统、支付系统、积分系统等。整个下单的过程如下：

        1. 用户通过商品系统浏览商品，他看中了某一项商品，便点击下单
        2. 此时订单系统会生成一条订单
        3. 订单创建成功后，支付系统提供支付功能
        4. 当支付完成后，由积分系统为该用户增加积分

    上述步骤2、3、4需要在一个事务中完成。对于传统单体应用而言，实现事务非常简单，只需将这三个步骤放在一个方法A中，再用Spring的@Transactional注解标识该方法即可。Spring通过数据库的事务支持，保证这些步骤要么全都执行完成，要么全都不执行。但在这个微服务架构中，这三个步骤涉及三个系统，涉及三个数据库。
```

#### 3种柔性分布式事务解决方案：

1. 可靠消息的最终一致性方案
2. TCC两阶段型方案

> 假设用户A用他的账户余额给用户B发一个100元的红包，并且余额系统和红包系统是两个独立的系统

Try

创建一条转账流水，并将流水的状态设为交易中
将用户A的账户中扣除100元（预留业务资源）
Try成功之后，便进入Confirm阶段
Try过程发生任何异常，均进入Cancel阶段


Confirm

向B用户的红包账户中增加100元
将流水的状态设为交易已完成
Confirm过程发生任何异常，均进入Cancel阶段
Confirm过程执行成功，则该事务结束


Cancel

将用户A的账户增加100元
将流水的状态设为交易失败


> 在传统事务机制中，业务逻辑的执行和事务的处理，是在不同的阶段由不同的部件来完成的：业务逻辑部分访问资源实现数据存储，其处理是由业务系统负责；事务处理部分通过协调资源管理器以实现事务管理，其处理由事务管理器来负责。二者没有太多交互的地方，所以，传统事务管理器的事务处理逻辑，仅需要着眼于事务完成（commit/rollback）阶段，而不必关注业务执行阶段。


3. 最大努力通知型方案


#### 4.2.7 集群负载均衡的算法与实现
```
    1. 轮询 RoundRobin
        每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
    2. 加权随机/加权轮询
        指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
    3. ip_Hash
        每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
    4. 最小连接数 LeastConnections
        根据后端服务器当前的连接情况，动态地选取其中当前积压连接数最少的一台服务器来处理当前请求，尽可能地提高后端服务器的利用效率，将负载合理地分流到每一台机器。
```

#### 4.2.8 说说分库与分表设计，可参考《数据库分库分表策略的具体实现方案》
```
分库分表的策略比前面的仅分库或者仅分表的策略要更为复杂，一种分库分表的路由策略如下：

    1. 中间变量 = user_id % (分库数量 * 每个库的表数量)

    2. 库 = 取整数 (中间变量 / 每个库的表数量)

    3. 表 = 中间变量 % 每个库的表数量
--------------------- https://blog.csdn.net/winy_lm/article/details/50708493 
```

#### 4.2.9 mysql分库与分表带来的分布式困境与应对之策
```
1. mycat是怎样实现分库分表的？mycat里面通过定义路由规则来实现分片表（路由规则里面会定义分片字段，以及分片算法）。分片算法有多种，你所说的hash是其中一种，还有取模、按范围分片等等。在mycat里面，会对所有传递的sql语句做路由处理（路由处理的依据就是表是否分片，如果分片，那么需要依据分片字段和对应的分片算法来判断sql应该传递到哪一个、或者哪几个、又或者全部节点去执行）

2. mycat适用于哪些场景？相对于海量存储的Nosql的适用场景又如何？数据量大到单机hold不住，而又不希望调整架构切换为NoSQL数据库，这个场景下可以考虑适用mycat。当然，使用前也应该做规划，哪些表需要分片等等。另外mycat对跨库join的支持不是很好，在使用mycat的时候要注意规避这种场景。
```
#### 4.2.10 mycat的原理和使用

| 文件       | 作用                                  |
| ---------- | ------------------------------------- |
| server.xml | Mycat的配置文件，设置账号、参数等     |
| schema.xml | Mycat对应的物理数据库和数据库表的配置 |
| rule.xml   | Mycat分片（分库分表）规则             |

```
    1. 数据切分后,原有的关系数据库中的主键约束在分布式条件下无法使用(自增主键已无法保证自增主键的全局唯一),因此引用外部机制保证数据的唯一标识,就是全局序列号(sequence))
```
---

### 4.3、Dubbo
#### 4.3.1 什么是Dubbo，可参考《Dubbo入门》
```
提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。其核心部分包含：
    远程通讯： 提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求-响应”模式的信息交换方式。
    集群容错：提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
    自动发现：基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。
```
#### 4.3.2 什么是RPC、如何实现RPC、RPC 的实现原理，可参考《基于HTTP的RPC实现》
> 实现: call ID、序列化/反序列化、网络传输。
1. 首先需要有处理`网络连接通讯`的模块，负责连接建立、管理和消息的传输。
2. 其次需要有编解码的模块，因为网络通讯都是传输的字节码，需要将我们使用的`对象序列化`和`反序列化`。
3. 剩下的就是`客户端`和`服务器端`的部分，服务器端暴露要开放的服务接口，客户调用服务接口的一个代理实现，这个代理实现负责收集数据、编码并传输给服务器然后等待结果返回。
#### 4.3.3 Dubbo中的SPI是什么概念
#### 4.3.4 Dubbo的基本原理、执行流程

Provider: 暴露服务的服务提供方。 
Consumer: 调用远程服务的服务消费方。 
Registry: 服务注册与发现的注册中心。 
Monitor: 统计服务的调用次数和调用时间的监控中心

```
- Apache MINA 框架基于Reactor模型通信框架，基于tcp长连接
- Dubbo缺省协议采用单一长连接和NIO异步通讯

- 基本原理如下：
    
    1. client一个线程调用远程接口，生成一个唯一的ID（比如一段随机字符串，UUID等），Dubbo是使用AtomicLong从0开始累计数字的
    
    2. 将打包的方法调用信息（如调用的接口名称，方法名称，参数值列表等），和处理结果的回调对象callback，全部封装在一起，组成一个对象object
    
    3. 向专门存放调用信息的全局ConcurrentHashMap里面put(ID, object)
    
    4. 将ID和打包的方法调用信息封装成一对象connRequest，使用IoSession.write(connRequest)异步发送出去
    
    5. 当前线程再使用callback的get()方法试图获取远程返回的结果，在get()内部，则使用synchronized获取回调对象callback的锁， 再先检测是否已经获取到结果，如果没有，然后调用callback的wait()方法，释放callback上的锁，让当前线程处于等待状态。
    
    6. 服务端接收到请求并处理后，将结果（此结果中包含了前面的ID，即回传）发送给客户端，客户端socket连接上专门监听消息的线程收到消息，分析结果，取到ID，再从前面的ConcurrentHashMap里面get(ID)，从而找到callback，将方法调用结果设置到callback对象里。
    
    7. 监听线程接着使用synchronized获取回调对象callback的锁（因为前面调用过wait()，那个线程已释放callback的锁了），再notifyAll()，唤醒前面处于等待状态的线程继续执行（callback的get()方法继续执行就能拿到调用结果了），至此，整个过程结束。
```
---

## 五、微服务

### 5.1、微服务
#### 5.1.1 前后端分离是如何做的？
```
前端是静态页面，后端是springcloud，中间件nginx

在前后端分离架构中，后端只需要负责按照约定的数据格式向前端提供可调用的API服务即可。前后端之间通过HTTP请求进行交互，前端获取到数据后，进行页面的组装和渲染，最终返回给浏览器。

```
#### 5.1.2 微服务哪些框架
```
https://segmentfault.com/a/1190000015237862
```
```
 - springcloud
 - dubbo
 - service mesh
```
#### 5.1.3 Spring Could的常见组件有哪些？可参考《Spring Cloud概述》
```
    1. Eureka/Consul 服务注册与发现
    2. Zuul 服务网关
    2. Ribbon 服务调用负载均衡
    3. Hystrix 服务调用熔断器
    4. Feign 整合了Ribbon以及Hystrix
    5. Config 分布式统一配置
```
#### 5.1.4 领域驱动有了解吗？什么是领域驱动模型？充血模型、贫血模型
#### 5.1.5 JWT有了解吗，什么是JWT，可参考《前后端分离利器之JWT》
#### 5.1.6 你怎么理解 RESTful
1. 采用URI标识资源 
2. 使用“链接”关联相关的资源 
3. 使用统一的接口 
> crud
4. 使用标准的HTTP方法 
> HEAD和OPTIONS相对少见; POST、PUT、PATCH和DELETE
5. 支持多种资源表示方式 
6. 无状态性

#### 5.1.7 说说如何设计一个良好的 API
```
1. 版本号
2. 资源路径
3. 请求方式
4. 查询参数:offset、 limit、 orderby 
5. 状态码
6. 异常响应
7. 请求参数
8. 响应参数
```
#### 5.1.8 如何理解 RESTful API 的幂等性

> 幂等性（Idempotent）是一个数学上的概念，在这里表示发送一次和多次请求引起的边界效应是一致的。在网速不够快的情况下，客户端发送一个请求后不能立即得到响应，由于不能确定是否请求是否被成功提交，所以它有可能会再次发送另一个相同的请求，幂等性决定了第二个请求是否有效。

> 3种安全的HTTP方法（GET、HEAD和OPTIONS）均是幂等方法。由于DELETE和PATCH请求操作的是现有的某个资源，所以它们是幂等方法。对于PUT请求，只有在对应资源不存在的情况下服务器才会进行添加操作，否则只作修改操作，所以它也是幂等方法。

> 至于最后一种POST，由于它总是进行添加操作，如果服务器接收到两次相同的POST操作，将导致两个相同的资源被创建，所以这是一个非幂等的方法。


#### 5.1.9 如何保证接口的幂等性

1. 查询操作：查询一次和查询多次，在数据不变的情况下，查询结果是一样的。select是天然的幂等操作；

2. 删除操作：删除操作也是幂等的，删除一次和多次删除都是把数据删除。(注意可能返回结果不一样，删除的数据不存在，返回0，删除的数据多条，返回结果多个) ；
3. 唯一索引，防止新增脏数据。比如：支付宝的资金账户，支付宝也有用户账户，每个用户只能有一个资金账户，怎么防止给用户创建资金账户多个，那么给资金账户表中的用户ID加唯一索引，所以一个用户新增成功一个资金账户记录。要点：唯一索引或唯一组合索引来防止新增数据存在脏数据（当表存在唯一索引，并发时新增报错时，再查询一次就可以了，数据应该已经存在了，返回结果即可）；
4. token机制，防止页面重复提交。业务要求： 页面的数据只能被点击提交一次；发生原因： 由于重复点击或者网络重发，或者nginx重发等情况会导致数据被重复提交；解决办法： 集群环境采用token加redis(redis单线程的，处理需要排队)；单JVM环境：采用token加redis或token加jvm内存。处理流程：
   
        1. 数据提交前要向服务的申请token，token放到redis或jvm内存，token有效时间；
        2. 提交后后台校验token，同时删除token，生成新的token返回。token特点：要申请，一次有效性，可以限流。注意：redis要用删除操作来判断token，删除成功代表token校验通过，如果用select+delete来校验token，存在并发问题，不建议使用；

5. 悲观锁——获取数据的时候加锁获取。select * from table_xxx where id='xxx' for update; 注意：id字段一定是主键或者唯一索引，不然是锁表，会死人的悲观锁使用时一般伴随事务一起使用，数据锁定时间可能会很长，根据实际情况选用； 
6. 乐观锁——乐观锁只是在更新数据那一刻锁表，其他时间不锁表，所以相对于悲观锁，效率更高。乐观锁的实现方式多种多样可以通过version或者其他状态条件：
   
        1. 通过版本号实现update table_xxx set name=#name#,version=version+1 where version=#version#如下图(来自网上)；
   
        2. 通过条件限制 update table_xxx set avai_amount=avai_amount-#subAmount# where avai_amount-#subAmount# >= 0要求：quality-#subQuality# >= ，这个情景适合不用版本号，只更新是做数据安全校验，适合库存模型，扣份额和回滚份额，性能更高；
   
7. 分布式锁——还是拿插入数据的例子，如果是分布是系统，构建全局唯一索引比较困难，例如唯一性的字段没法确定，这时候可以引入分布式锁，通过第三方的系统(redis或zookeeper)，在业务系统插入数据或者更新数据，获取分布式锁，然后做操作，之后释放锁，这样其实是把多线程并发的锁的思路，引入多多个系统，也就是分布式系统中得解决思路。要点：某个长流程处理过程要求不能并发执行，可以在流程执行之前根据某个标志(用户ID+后缀等)获取分布式锁，其他流程执行时获取锁就会失败，也就是同一时间该流程只能有一个能执行成功，执行完成后，释放分布式锁(分布式锁要第三方系统提供)；

#### 5.1.10 说说 CAP 定理、BASE 理论

1. 在分布式系统领域有个著名的CAP定理：这三个特性在任何分布式系统中不能同时满足，最多同时满足两个
   
        C——数据一致性 Consistency。
        A——服务可用性 Availability。
        P——服务对分区故障的容错性 Partition tolerance。(分布式系统的根本)

2. Zookeeper保证的是CP。使用Zookeeper获取服务列表时，如果zookeeper正在选主，或者Zookeeper集群中半数以上机器不可用，那么将就无法获得数据了。所以说，Zookeeper不能保证服务可用性（A）。

3. 只能通过牺牲一致性来换取系统的可用性和分区容错性。这也就是下面要介绍的BASE理论。
        - 所谓的“牺牲一致性”并不是完全放弃数据一致性，而是牺牲强一致性（ACID原则）换取弱一致性。

        - BA：Basic Available 基本可用。整个系统在某些不可抗力的情况下，仍然能够保证“可用性”，即一定时间内仍然能够返回一个明确的结果。只不过“基本可用”和“高可用”的区别是：“一定时间”可以适当延长，当举行大促时，响应时间可以适当延长，给部分用户返回一个降级页面，从而缓解服务器压力。但要注意，返回降级页面仍然是返回明确结果。

        - S：Soft State：柔性状态。同一数据的不同副本的状态，可以不需要实时一致。

        - E：Eventual Consisstency：最终一致性。同一数据的不同副本的状态，可以不需要实时一致，但一定要保证经过一定时间后仍然是一致的。


#### 5.1.11 怎么考虑数据一致性问题
```
保证至少一次语义。
```
#### 5.1.12 说说最终一致性的实现方案

1. 基于事务型消息队列的最终一致性

     借助消息队列，在处理业务逻辑的地方发送消息，业务逻辑处理成功后，提交消息，确保消息是发送成功的，之后消息队列投递来进行处理，如果成功，则结束，如果没有成功，则重试，直到成功，不过仅仅适用业务逻辑中，第一阶段成功，第二阶段必须成功的场景。对应上图中的C流程。

2. 基于消息队列+定时补偿机制的最终一致性

     前面部分和上面基于事务型消息的队列，不同的是，第二阶段重试的地方，不再是消息中间件自身的重试逻辑了，而是单独的补偿任务机制。其实在大多数的逻辑中，第二阶段失败的概率比较小，所以单独独立补偿任务表出来，可以更加清晰，能够比较明确的直到当前多少任务是失败的。

#### 5.1.13 微服务的优缺点，可参考《微服务批判》

> 优点
1. 通过分解巨大单体式应用为多个服务方法解决了复杂性问题，每个微服务相对较小
2. 每个单体应用不局限于固定的技术栈，开发者可以自由选择开发技术，提供API服务。
3. 每个微服务独立的开发，部署
4. 单一职责功能，每个服务都很简单，只关注于一个业务功能
5. 易于规模化开发，多个开发团队可以并行开发，每个团队负责一项服务
6. 改善故障隔离。一个服务宕机不会影响其他的服务

> 缺点
1. 开发者需要应对创建分布式系统所产生的额外的复杂因素：如测试复杂、分布式事务的处理
2. 部署复杂
3. 内存占用量更高

#### 5.1.14 微服务与 SOA（资源调度和治理中心） 的区别
```
微服务相比于SOA更加精细，微服务更多的以独立的进程的方式存在，互相之间并无影响；

微服务提供的接口方式更加通用化，例如HTTP RESTful方式，各种终端都可以调用，无关语言、平台限制；

微服务更倾向于分布式去中心化的部署方式，在互联网业务场景下更适合；
```
#### 5.1.15 如何拆分服务、水平分割、垂直分割
```
原则是拆分粒度应该保证微服务具有业务的独立性和完整性，服务的拆分围绕业务模块进行拆。
```
#### 5.1.16 如何应对微服务的链式调用异常
> 一个业务功能可能需要多个服务协作才能实现，一个请求到达服务A，服务A需要依赖服务B，服务B又依赖服务C，甚至C仍需依赖其他服务，形成一个调用链条，即调用链。

> 使用zipkin或者Pinpoint监控链路调用，进行追踪。
#### 5.1.17 如何快速追踪与定位问题
```
基于zipkin: Zipkin分布式跟踪系统；它可以帮助收集时间数据，解决在microservice架构下的延迟问题；它管理这些数据的收集和查找
spring cloud sleuth可以结合zipkin，将信息发送到zipkin.
```
#### 5.1.18 如何保证微服务的安全、认证

---

### 5.2、安全问题
#### 5.2.1 如何防范常见的Web攻击、如何方式SQL注入
> 如何防范XSS攻击
```
跨站点脚本攻击，指攻击者通过篡改网页，嵌入恶意脚本程序，在用户浏览网页时，控制用户浏览器进行恶意操作的一种攻击方式。

1. 前端，服务端，同时需要字符串输入的长度限制。
2. 前端，服务端，同时需要对HTML转义处理。将其中的”<”,”>”等特殊字符进行转义编码
```

> 什么是CSRF攻击
```
跨站点请求伪造，指攻击者通过跨站请求，以合法的用户的身份进行非法操作。可以这么理解CSRF攻击：攻击者盗用你的身份，以你的名义向第三方网站发送恶意请求。CRSF能做的事情包括利用你的身份发邮件，发短信，进行交易转账，甚至盗取账号信息。

1. 安全框架，例如Spring Security。
2. token机制。在HTTP请求中进行token验证，如果请求中没有token或者token内容不正确，则认为CSRF攻击而拒绝该请求。
3. 验证码。通常情况下，验证码能够很好的遏制CSRF攻击，但是很多情况下，出于用户体验考虑，验证码只能作为一种辅助手段，而不是最主要的解决方案。
4. referer识别。在HTTP Header中有一个字段Referer，它记录了HTTP请求的来源地址。如果Referer是其他网站，就有可能是CSRF攻击，则拒绝该请求。但是，服务器并非都能取到Referer。很多用户出于隐私保护的考虑，限制了Referer的发送。在某些情况下，浏览器也不会发送Referer，例如HTTPS跳转到HTTP。

```

> 文件上传漏洞
```
文件上传漏洞，指的是用户上传一个可执行的脚本文件，并通过此脚本文件获得了执行服务端命令的能力。许多第三方框架、服务，都曾经被爆出文件上传漏洞，比如很早之前的Struts2，以及富文本编辑器等等，可能被一旦被攻击者上传恶意代码，有可能服务端就被人黑了。

1. 文件上传的目录设置为不可执行。
2. 判断文件类型。在判断文件类型的时候，可以结合使用MIME Type，后缀检查等方式。因为对于上传文件，不能简单地通过后缀名称来判断文件的类型，因为攻击者可以将可执行文件的后缀名称改为图片或其他后缀类型，诱导用户执行。
3. 对上传的文件类型进行白名单校验，只允许上传可靠类型。
4. 上传的文件需要进行重新命名，使攻击者无法猜想上传文件的访问路径，将极大地增加攻击成本，同时向shell.php.rar.ara这种文件，因为重命名而无法成功实施攻击。
5. 限制上传文件的大小。
6. 单独设置文件服务器的域名。
```

> 防止SQL注入
```
1. 不用拼接SQL字符串。
2. 使用预编译的PrepareStatement。
3. 有效性检验。(为什么服务端还要做有效性检验？第一准则，外部都是不可信的，防止攻击者绕过Web端请求)
4. 过滤SQL需要的参数中的特殊字符。比如单引号、双引号。
```
#### 5.2.2 服务端通信安全攻防
#### 5.2.3 HTTPS原理剖析、降级攻击、HTTP与HTTPS的对比
> HTTP + 加密 + 认证 + 完整性保护 = HTTPS

> 对比
1. https协议需要到ca申请证书，一般免费证书很少，需要交费。

2. http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议

3. http和https使用的是完全不同的连接方式用的端口也不一样,前者是80,后者是443

---

### 5.3、性能优化
> [性能优化概述](http://blog.51cto.com/freeloda/1438052)
#### 5.3.1 性能指标有哪些
```
吞吐量 –> 是单位时间内完成的用户或系统的请求数量。

并发数 –> 同时能接受多少用户的访问请求

响应时间 –> 用户发出请求到收到响应的时间间隔。
```
#### 5.3.2 如何发现性能瓶颈
1. 关注数据流向
2. 日志文件异常
3. 服务器系统的硬件✅状况：cpu占用率、内存占用率、IO情况
4. 数据库的监控：Oracle本身提供了ASH进行监控

#### 5.3.3 数据库性能调优的常见手段
1. 优化SQL语句
```
通过Explain和Describe关键字分析select查询语句，从而使开发人员知道查询效率低的原因。

通过添加索引进行优化
    （1）使用多列索引时，查询条件必须使用索引的第一个字符。
    （2）like关键字配置的字符串不能以符号“%”开头。
    （3）or关键字连接的所有条件都必须使用索引。如果or前后有一个条件的列不是索引，那么查询中讲不使用索引。

优化Order by
    有两种方式如下：
    （1）索引优化：对by后的列添加索引，从而大达到优化的目的。
    （2）where+order by 的组合优化：通过where进行限制后在进行order by
优化group by    (同3中通过加索引和where进行优化)

优化limit

优化子查询
```

2. 优化数据库结构
```
1、优化字段类型

    数据库最耗时的就是IO处理，所以尽可能减少IO读写量。
    数字类型：尽量不要使用double类型，不仅是长度问题，还有精度问题。对于整数，数据量大时区分TinyInt、Int、BigInt，如果确定不实用负数，添加unsigned定义。
    字符类型：尽量不要使用text类型，性能低于char和varchar，定长使用char，不定长使用varchar
    时间类型：尽量不使用Timestamp，只精确到某一天的话，可用Date类型。
    Enum和Set：状态字段用enum，如果存放可预先定义的属性数据可以尝试用set

2、优化字符编码

3、适当进行拆分

4、适当增加冗余

5、优化数据库表

```
3. 优化Mysql服务器
```
内存中的数据要比磁盘上的数据访问的快。

让数据尽可能长时间的留在内存里能减少磁盘读写活动的工作量。

让索引信息留在内存里要比让数据记录的内容留在内存里更为重要。
```

#### 5.3.4 说说你在项目中如何进行性能调优
> 对服务器的健康监控
> Java程序内存的占用率
```
服务端：并发、锁机制
数据库：sql优化
```

---

## 六、其他
### 6.1、设计能力
#### 6.1.1 说说你在项目中使用过的UML图
```
时序图
类图
```
#### 6.1.2 你如何考虑组件化、服务化、系统拆分
#### 6.1.3 秒杀场景如何设计
#### 6.1.4 可参考：《秒杀系统的技术挑战、应对策略以及架构设计总结一二！》

---

### 6.2、业务工程
#### 6.2.1 说说你的开发流程、如何进行自动化部署的
#### 6.2.2 你和团队是如何沟通的
#### 6.2.3 你如何进行代码评审
#### 6.2.4 说说你对技术与业务的理解
#### 6.2.5 说说你在项目中遇到感觉最难Bug，是如何解决的
#### 6.2.6 介绍一下工作中的一个你认为最有价值的项目，以及在这个过程中的角色、解决的问题、你觉得你们项目还有哪些不足的地方

---

### 6.3、软实力
#### 6.3.1 说说你的优缺点、亮点
#### 6.3.2 说说你最近在看什么书、什么博客、在研究什么新技术、再看那些开源项目的源代码
#### 6.3.3 说说你觉得最有意义的技术书籍
#### 6.3.4 工作之余做什么事情、平时是如何学习的，怎样提升自己的能力
#### 6.3.5 说说个人发展方向方面的思考
#### 6.3.6 说说你认为的服务端开发工程师应该具备哪些能力
#### 6.3.7 说说你认为的架构师是什么样的，架构师主要做什么
#### 6.3.8 如何看待加班的问题

## 七、一些面经网站

### 7.1 综合
- https://mp.weixin.qq.com/s/i4LEkQbu6Ctz5J814KxLJg
- https://juejin.im/post/5aa4a2e35188255589496eb8

- https://juejin.im/post/5aa4a2e35188255589496eb8

- https://zhuanlan.zhihu.com/p/44164592

- https://mp.weixin.qq.com/s/44FO5Jx-qZwz3qJ5qCTXjA
- https://blog.csdn.net/v123411739/article/details/71437307

### 7.2 细分

- [B树，R树](https://blog.csdn.net/v_july_v/article/details/6530142)
