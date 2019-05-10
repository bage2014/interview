# Java 基础 #

知识点汇总github地址 [https://github.com/bage2014/interview](https://github.com/bage2014/interview)
java基础代码实现demo地址[https://github.com/bage2014/study/tree/master/study-java/src/main/java/com/bage/study/java](https://github.com/bage2014/study/tree/master/study-java/src/main/java/com/bage/study/java)

## 面向对象理论基础 ##

### 面向对象的特征：继承、封装和多态 ###
- 多态体现
 - 接口和接口之间的继承。
 - 类和类直接直接的继承。
 - 方法重载。
 - 方法重写。
### final、finally、finalize ###
- final
 - java关键字
 - 被修饰的变量将不可重复赋值，只能在首次赋值
 - 被修饰的方法将不可以被重写
- finally
 - try-catch-finally块之一，一般用来进行资源的释放
- finalize
 - Java的析构函数

### Exception、Error ###
- 继承关系

简单表示如下

     Throwable
       /   \
	Error  Exception

- 产生原因

 - Error 一般为系统错误，比如内存不够等，一般不需要程序进行处理
 - Exception 一般是用户的异常，比如空指针异常，可能需要程序研发人员进行捕获处理


### 运行时异常与一般异常 ###
- 继承关系

简单表示如下

            Exception
              /    \
	xxxException  RuntimeException

- 两者差异
 - RuntimeException ，比如空指针异常，一般不需要程序研发人员进行捕获处理，但是可以避免，比如增加判断是否为null之后再进行业务操作
 - 常见的RuntimeException
		1. NullPointerException - 空指针引用异常
		2. ClassCastException - 类型强制转换异常
		3. IllegalArgumentException - 传递非法参数异常
		4. ArithmeticException - 算术运算异常
		5. IndexOutOfBoundsException - 下标越界异常
		6. NumberFormatException - 数字格式异常

### String、StringBuilder、StringBuffer ###
- String
	1. 字符串操作类，final类
	2. 需要了解其常用方法和简单实现
- StringBuilder
	1. 字符串拼接类
	2. 非线程安全
- StringBuffer
	1. 字符串拼接类
	2. 线程安全，实现方式为在方法上加上synchronized关键字

### 重载和重写 ###
- 重载，方法名一样，但是参数个数或参数类型不一样，比如print(int a)和print(String str)
- 重写，子类重写父类中定义的方法，比如父类Parent类中存在print(int a)方法，但是子类重新对其进行了实现

### 抽象类和接口有什么区别 ###
- 抽象类
	1. Abstract 关键字修饰的class
	2. 可以存在方法默认的实现
- 接口
1. interface 关键字修饰
2. jdk8及之后版本，可以存在方法默认实现，jdk8之前必须为空方法，仅为方法声明

### int和Integer ###
- Integer是int的包装类
- Integer的值缓存范围[-128~127]

### 装箱和拆箱 ###
参考链接 [https://www.cnblogs.com/wang-yaz/p/8516151.html](https://www.cnblogs.com/wang-yaz/p/8516151.html)

- 当一个基础数据类型与封装类进行==、+、-、*、/运算时，会将封装类进行拆箱，对基础数据类型进行运算
- 它必须满足两个条件才为true == 类型相同 + 内容相同
- Integer可能会抛出空指针异常。所以，有拆箱操作时一定要特别注意封装类对象是否为null

### 反射的用途及实现 ###
参考链接 [https://blog.csdn.net/snn1410/article/details/44978457](https://blog.csdn.net/snn1410/article/details/44978457)、[https://www.jianshu.com/p/d6035d5d4d12](https://www.jianshu.com/p/d6035d5d4d12)

- 反射机制值得是程序在运行时能够获取自身的信息。在java中，只要给定类的名字，那么就可以通过反射机制来获得类的所有信息
- Java反射框架提供以下功能：
	1.	在运行时判断任意一个对象所属的类
	2.	在运行时构造任意一个类的对象
	3.	在运行时判断任意一个类所具有的成员变量和方法（通过反射可以调用 private方法）
	4.	在运行时调用任意一个对象的方法
- 可以通过配置文件来动态配置和加载类，以实现软件工程理论里所提及的类与类，模块与模块之间的解耦。最重要的用途就是开发各种通用框架

### 常用的JDK包 ###
- 基本类型，byte,int,char,double,float,boolean
- 基本包装类，String、StringBuilder、Integer、BigInteger
- 集合类，Map、Set、Table、List
- 文件类，File等IO
- 多线程的基本使用
- 等等

### equals与== ###
- equals比较内容，==比较地址
- 字符串一定是需要Equals方法

### wait、notify方法放在Object里边？ ###
- 因为synchronized中的这把锁可以是任意对象，所以任意对象都可以调用wait()和notify()；所以wait和notify属于Object。
- 因为这些方法在操作同步线程时，都必须要标识它们操作线程的锁，只有同一个锁上的被等待线程，可以被同一个锁上的notify唤醒，不可以对不同锁中的线程进行唤醒。也就是说，等待和唤醒必须是同一个锁。而锁可以是任意对象，所以可以被任意对象调用的方法是定义在object类中。

### 线程间通信 ###
参考链接 [https://www.cnblogs.com/hapjin/p/5492619.html](https://www.cnblogs.com/hapjin/p/5492619.html)

- 同步
- while轮询的方式
- wait/notify机制
- 管道通信就是使用java.io.PipedInputStream 和 java.io.PipedOutputStream进行通信

### Java的平台无关性如何体现出来的 ###

### JDK和JRE的区别 ###


### hashCode和equals方法 ###
- 重写equals必须重写hashcode
- 当自定义对象作为集合类的key时，必须重写此两个方法

### Java序列化和反序列化 ###
参考链接 [https://blog.csdn.net/onceing/article/details/80969369](https://blog.csdn.net/onceing/article/details/80969369)

- 基本概念
	1.	Serialization-序列化：可以看做是将一个对象转化为二进制流的过程
	2.	Deserialization-反序列化：可以看做是将对象的二进制流重新读取转换成对象的过程
- 注意点
	1.	若类仅仅实现了Serializable接口，则采用默认的序列化方式，对对象的非transient的实例变量进行序列化。ObjcetInputStream采用默认的反序列化方式，对对象的非transient的实例变量进行反序列化。
	2.	若类不仅实现了Serializable接口，并且还定义了readObject(ObjectInputStream in)和writeObject(ObjectOutputSteam out)，则ObjectOutputStream调用对象的writeObject(ObjectOutputStream out)的方法进行序列化。ObjectInputStream会调用对象的readObject(ObjectInputStream in)的方法进行反序列化。
	3.	若类实现了Externalnalizable接口，且类必须实现readExternal(ObjectInput in)和writeExternal(ObjectOutput out)方法，则ObjectOutputStream调用对象的writeExternal(ObjectOutput out))的方法进行序列化。ObjectInputStream会调用对象的readExternal(ObjectInput in)的方法进行反序列化。
	4.	被transient和static修饰的成员变量不会被序列化
	5.	假设A实现了Serializable接口，且A为B的子类，B没有实现Serializable接口。那么在序列化和反序列话的时候，B的无参构造函数负责B的相关属性的序列化和反序列化。特殊的，当B没有无参构造函数的时候，将A对象进行序列化时不会报错，但是反序列化获取A的时候报错
	6.	序列化时，只对对象的状态进行保存，而不管对象的方法
	7.	当一个父类实现序列化，子类自动实现序列化，不需要显式实现Serializable接口
	8.	当一个对象的实例变量引用其他对象，序列化该对象时也把引用对象进行序列化
	9.	并非所有的对象都可以序列化，至于为什么不可以，有很多原因了，比如：安全方面的原因，比如一个对象拥有private，public等field，对于一个要传输的对象，比如写到文件，或者进行RMI传输等等，在序列化进行传输的过程中，这个对象的private等域是不受保护的；资源分配方面的原因，比如socket，thread类，如果可以序列化，进行传输或者保存，也无法对他们进行重新的资源分配，而且，也是没有必要这样实现；
	10.	声明为static和transient类型的成员数据不能被序列化。因为static代表类的状态，transient代表对象的临时数据。
	11.	序列化运行时使用一个称为 serialVersionUID 的版本号与每个可序列化类相关联，该序列号在反序列化过程中用于验证序列化对象的发送者和接收者是否为该对象加载了与序列化兼容的类。为它赋予明确的值。显式地定义serialVersionUID有两种用途：在某些场合，希望类的不同版本对序列化兼容，因此需要确保类的不同版本具有相同的serialVersionUID；在某些场合，不希望类的不同版本对序列化兼容，因此需要确保类的不同版本具有不同的serialVersionUID。
	12.	Java有很多基础类已经实现了serializable接口，比如String,Vector等。但是比如HashTable就没有实现serializable接口；
	13.	如果一个对象的成员变量是一个对象，那么这个对象的数据成员也会被保存！这是能用序列化解决深拷贝的重要原因；

- 序列化步骤
	1.	将对象实例相关的类元数据输出。
	2.	递归地输出类的超类描述直到不再有超类。
	3.	类元数据完了以后，开始从最顶层的超类开始输出对象实例的实际数据值。
	4.	从上至下递归输出实例的数据

### Java 8新特性 ###
参考链接 [http://www.runoob.com/java/java8-new-features.html](http://www.runoob.com/java/java8-new-features.html)

- Lambda 表达式 − Lambda允许把函数作为一个方法的参数（函数作为参数传递进方法中，更详细的例子，请参考[被姜太公钓的鱼](https://blog.csdn.net/swh1314)的文章[https://blog.csdn.net/swh1314/article/details/82687052](https://blog.csdn.net/swh1314/article/details/82687052)。
- 方法引用 − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
- 默认方法 − 默认方法就是一个在接口里面有了一个实现的方法。
- 新工具 − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。
- Stream API −新添加的Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中。
- Date Time API − 加强对日期与时间的处理。
- Optional 类 − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。
- Nashorn, JavaScript 引擎 − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。



## Java集合 list、map、set ##

### List 和 Set 区别 ###
### List 和 Map 区别 ###
### Arraylist 与 LinkedList 区别 ###
### ArrayList 与 Vector 区别 ###
### HashMap 和 Hashtable 的区别 ###
### HashSet 和 HashMap 区别 ###
### HashMap 和 ConcurrentHashMap 的区别 ###
- HashMap不是线程安全的，ConcurrentHashMap则在某一个方法的执行上是线程安全的。
- 数据结构
	1.	都是数组+拉链实现的哈希表，但是具体实现上差别大了
- 并发
	1.	Hashtable全表锁
	2.	HashMap多线程不安全，需要自己封装
	3.	ConcurrentHashMap加细粒度锁，读不加锁，如果读到空值再加锁。
- null
	1.	Hashtable不允许用 null作为键和值
	2.	HashMap允许全局最多一个null键，但是允许无数个null值
	3.	ConcurrentHashMap不允许用 null作为键和值

### hashCode以及equals方法的联系 ###


### HashMap 的工作原理及代码实现 ###
参考链接 [https://blog.csdn.net/v123411739/article/details/78996181](https://blog.csdn.net/v123411739/article/details/78996181)

结构
jdk 1.7数据结构
hashmap-jdk1.7.png

jdk 1.8数据结构
hashmap-jdk1.8.png

### ConcurrentHashMap 的工作原理及代码实现 ###

### ConcurrentHashMap 常用方法 ###

### ConcurrentHashMap统计所有的元素个数 ###

### 多线程HashMap问题 ###