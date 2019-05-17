# Java 基础 #

知识点汇总github地址 [https://github.com/bage2014/interview](https://github.com/bage2014/interview)
Spring 实践demo地址[https://github.com/bage2014/spring-study](https://github.com/bage2014/spring-study)

## Spring 理论基础 ##

### 谈谈你对Spring的理解 ###
关键点
- 企业框架，目前最流行，没有之一
- AOP、IOC、Spring MVC

### Spring中用到了哪些设计模式 ###
- 工厂模式，比如 BeanFactory
- 代理模式，在Aop实现中用到了JDK的动态代理
- 单例模式，Bean的创建默认就是单利的

### IoC的启动过程 ###
- Resource文件的定位，即找到bean的配置文件
- 通过特定的reader解析该bean配置文件，抽象成beanDefinition类
- 将beanDefinition向容器注册，写入到一个大的HashMap中


### Spring 容器启动过程 ###


### BeanFactory 和 ApplicationContext 区别 ###
参考链接：[https://blog.csdn.net/lxyso/article/details/45446757](https://blog.csdn.net/lxyso/article/details/45446757)、[https://www.cnblogs.com/aspirant/p/9082858.html](https://www.cnblogs.com/aspirant/p/9082858.html)

- 功能，BeanFactory负责读取bean配置，管理bean的加载，实例化，维护bean之间的依赖关系，负责bean的声明周期；ApplicationContext除了提供上述BeanFactory所能提供的功能之外，还提供了国际化支持、资源访问、事件传递、队Web的支持等功能
- BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化；而ApplicationContext则相反，它是在容器启动时，一次性创建了所有的Bean。
- BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册

### Bean 的生命周期 ###
代码实现 [https://github.com/bage2014/study/tree/master/study-spring/src/main/java/com/bage/lifecircle](https://github.com/bage2014/study/tree/master/study-spring/src/main/java/com/bage/lifecircle)

1. 实例化一个Bean，也就是我们通常说的new
2. 按照Spring上下文对实例化的Bean进行配置，也就是IOC注入
3. 如果这个Bean实现了BeanNameAware接口，会调用它实现的setBeanName(String beanId)方法，此处传递的是Spring配置文件中Bean的ID
4. 如果这个Bean实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()，传递的是Spring工厂本身（可以用这个方法获取到其他Bean）
5. 如果这个Bean实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文，该方式同样可以实现步骤4，但比4更好，以为ApplicationContext是BeanFactory的子接口，有更多的实现方法
6. 如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法，BeanPostProcessor经常被用作是Bean内容的更改，并且由于这个是在Bean初始化结束时调用After方法，也可用于内存或缓存技术
7. 如果这个Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法
8. 如果这个Bean关联了BeanPostProcessor接口，将会调用postAfterInitialization(Object obj, String s)方法
注意：以上工作完成以后就可以用这个Bean了，那这个Bean是一个single的，所以一般情况下我们调用同一个ID的Bean会是在内容地址相同的实例
9. 当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean接口，会调用其实现的destroy方法
10. 最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法

### Bean的作用域 ###

- **singleton**(默认): 在Spring的IoC容器中只存在一个对象实例，所有该对象的引用都共享这个实例。Spring 容器只会创建该bean定义的唯一实例，这个实例会被保存到缓存中，并且对该bean的所有后续请求和引用都将返回该缓存中的对象实例，一般情况下，无状态的bean使用该scope。
- **prototype**:每次对该bean的请求都会创建一个新的实例，一般情况下，有状态的bean使用该scope。
- **request**：每次http请求将会有各自的bean实例，类似于prototype。
- **session**：在一个http session中，一个bean定义对应一个bean实例。
- **global session**：在一个全局的http session中，一个bean定义对应一个bean实例。典型情况下，仅在使用portlet context的时候有效。

### AOP ###

#### 实现原理 ####
默认，接口基于JDK动态代理，类为cglib

#### 注意事项 ####
- 嵌套失效问题
- final类直接报错问题(动态代理的实现原理)

### Spring 事务 ###

#### 实现原理 ####
基于AOP，所以，嵌套的事务会失效

#### 传播机制 ####
- 1
- 2
- 3
- 4

### Spring MVC ###
#### 请求过程 ####
1.	用户请求DispatchServlet
2.	DispatchServlet根据请求路径调用具体HandlerMapping返回一个HandlerExcutionChain
3.	DispatchServlet调用HandlerAdapter适配器
4.	HandlerAdapter调用具体的Handler处理业务
5.	Handler处理结束返回一个具体的ModelAndView给适配器
6.	适配器将ModelAndView给DispatchServlet
7.	DispatchServlet把视图名称给ViewResolver视图解析器
8.	ViewResolver返回一个具体的视图给DispatchServlet
9.	渲染视图，展示给用户
