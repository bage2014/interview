# Java 基础 #

github地址[https://github.com/bage2014/interview](https://github.com/bage2014/interview)

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

## Java集合 list、map、set ##

### HashMap ###
参考链接 [https://blog.csdn.net/v123411739/article/details/78996181](https://blog.csdn.net/v123411739/article/details/78996181)

结构
jdk 1.7数据结构
hashmap-jdk1.7.png

jdk 1.8数据结构
hashmap-jdk1.8.png


## 常用类 Integer、String、StringBuilder ##

## Java反射 ##

## JDK各个版本特性 ##
