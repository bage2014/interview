# Java 面试知识总结大全

## 1. 前言

本文档汇总了Java后端面试中常见的知识点，包括Java基础、集合、多线程、JVM、Spring、微服务、数据库等多个领域。内容涵盖了核心概念、原理分析、常见问题及解答，旨在帮助开发者系统复习面试知识点，提高面试通过率。

## 2. Java基础

### 2.1 面向对象特性
- **封装**：将数据和行为包装在类中，通过访问修饰符控制访问权限
- **继承**：子类继承父类的属性和方法，实现代码复用
- **多态**：同一方法在不同对象上有不同的实现，包括方法重载和方法重写
- **抽象**：通过抽象类和接口定义规范，隐藏实现细节

### 2.2 关键字
- **final**：修饰类（不可继承）、方法（不可重写）、变量（不可修改）
- **static**：修饰变量（类变量）、方法（类方法）、代码块（静态代码块）
- **this**：指向当前对象的引用
- **super**：指向父类对象的引用
- **volatile**：保证内存可见性，防止指令重排序
- **transient**：标记变量不参与序列化

### 2.3 异常体系
- **Throwable**：所有异常的父类
  - **Error**：系统级错误，如OutOfMemoryError
  - **Exception**：程序级异常
    - **RuntimeException**：运行时异常，如NullPointerException、ClassCastException
    - **Checked Exception**：受检异常，如IOException

### 2.4 字符串处理
- **String**：不可变字符串，底层是char数组
- **StringBuilder**：可变字符串，非线程安全，性能高
- **StringBuffer**：可变字符串，线程安全，性能较低

### 2.5 反射
- **概念**：在运行时获取类的信息并操作类的成员
- **用途**：动态加载类、创建对象、调用方法、访问属性
- **应用**：框架开发、注解处理、动态代理

### 2.6 序列化
- **概念**：将对象转换为字节流的过程
- **反序列化**：将字节流转换回对象的过程
- **实现**：实现Serializable接口
- **注意**：static和transient修饰的变量不会被序列化

### 2.7 Java 8+新特性
- **Lambda表达式**：函数式编程风格
- **方法引用**：简化Lambda表达式
- **Stream API**：流式处理集合
- **Optional**：处理空指针问题
- **LocalDateTime**：新的日期时间API
- **默认方法**：接口中可以有默认实现

### 2.8 注解
- **元注解**：@Retention、@Target、@Documented、@Inherited
- **自定义注解**：使用@interface关键字定义
- **注解处理器**：通过反射获取注解信息

## 3. Java集合

### 3.1 集合框架体系
- **Collection**：单列集合
  - **List**：有序可重复
    - **ArrayList**：基于数组，随机访问快，插入删除慢
    - **LinkedList**：基于链表，插入删除快，随机访问慢
    - **Vector**：线程安全的ArrayList
  - **Set**：无序不可重复
    - **HashSet**：基于HashMap，哈希表实现
    - **LinkedHashSet**：基于LinkedHashMap，有序哈希表
    - **TreeSet**：基于TreeMap，红黑树实现，有序
- **Map**：双列集合
  - **HashMap**：基于哈希表，线程不安全
  - **LinkedHashMap**：有序的HashMap
  - **TreeMap**：基于红黑树，有序
  - **Hashtable**：线程安全的HashMap
  - **ConcurrentHashMap**：并发安全的HashMap

### 3.2 HashMap原理
- **数据结构**：JDK 1.7 数组+链表，JDK 1.8 数组+链表+红黑树
- **哈希冲突**：使用链表解决，当链表长度超过8时转为红黑树
- **扩容机制**：当元素数量超过容量*负载因子时扩容为原来的2倍
- **线程安全**：非线程安全，多线程环境下可能导致死循环

### 3.3 ConcurrentHashMap原理
- **JDK 1.7**：分段锁机制，将数组分为多个段，每个段有独立的锁
- **JDK 1.8**：CAS+ synchronized，只锁定链表或红黑树的首节点

## 4. 多线程

### 4.1 线程基础
- **线程与进程**：进程是资源分配的单位，线程是执行的单位
- **线程状态**：新建、就绪、运行、阻塞、死亡
- **创建线程**：继承Thread类、实现Runnable接口、实现Callable接口

### 4.2 线程同步
- **synchronized**：关键字，可修饰方法或代码块
- **Lock**：接口，如ReentrantLock、ReadWriteLock
- **volatile**：保证内存可见性
- **Atomic**：原子类，如AtomicInteger

### 4.3 线程通信
- **wait()/notify()/notifyAll()**：Object类的方法
- **CountDownLatch**：计数器，等待多个线程完成
- **CyclicBarrier**：循环屏障，等待所有线程到达屏障点
- **Semaphore**：信号量，控制并发访问数量
- **Exchanger**：交换器，两个线程交换数据

### 4.4 线程池
- **核心参数**：核心线程数、最大线程数、空闲线程存活时间、阻塞队列、拒绝策略
- **工作原理**：当线程数小于核心线程数时，创建新线程；当核心线程数满时，将任务加入阻塞队列；当队列满时，创建新线程直到最大线程数；当最大线程数满时，执行拒绝策略
- **拒绝策略**：AbortPolicy、DiscardPolicy、DiscardOldestPolicy、CallerRunsPolicy
- **常用线程池**：FixedThreadPool、CachedThreadPool、ScheduledThreadPool、SingleThreadExecutor

### 4.5 ThreadLocal
- **概念**：线程本地变量，每个线程有独立的副本
- **原理**：Thread类中有一个ThreadLocalMap，键是ThreadLocal实例，值是线程本地变量
- **内存泄漏**：ThreadLocalMap使用弱引用作为键，当ThreadLocal实例被回收后，键为null，但值仍存在，可能导致内存泄漏
- **解决方案**：使用完毕后调用remove()方法清理

### 4.6 锁优化
- **自旋锁**：线程不放弃CPU，而是自旋等待
- **锁消除**：编译器优化，消除不必要的锁
- **锁粗化**：将多个细粒度锁合并为一个粗粒度锁
- **偏向锁**：减少无竞争情况下的锁开销
- **轻量级锁**：减少竞争情况下的锁开销

## 5. JVM

### 5.1 内存结构
- **程序计数器**：线程私有，记录当前线程执行的字节码行号
- **虚拟机栈**：线程私有，存储局部变量表、操作数栈、方法出口等
- **本地方法栈**：线程私有，存储本地方法的栈帧
- **堆**：线程共享，存储对象实例，分为新生代（Eden、From Survivor、To Survivor）和老年代
- **方法区**：线程共享，存储类信息、常量、静态变量等
- **运行时常量池**：方法区的一部分，存储字面量和符号引用

### 5.2 垃圾回收
- **判断对象存活**：引用计数法、可达性分析算法
- **引用类型**：强引用、软引用、弱引用、虚引用
- **垃圾回收算法**：标记-清除算法、复制算法、标记-整理算法、分代收集算法
- **垃圾收集器**：Serial、ParNew、Parallel Scavenge、CMS、G1

### 5.3 类加载机制
- **加载**：将类的字节码加载到内存
- **链接**：验证、准备、解析
- **初始化**：执行类构造器`<clinit>()`方法
- **双亲委派机制**：避免类的重复加载，保证安全性
- **类加载器**：Bootstrap ClassLoader、Extension ClassLoader、Application ClassLoader、Custom ClassLoader

### 5.4 JVM参数
- **堆参数**：-Xms（初始堆大小）、-Xmx（最大堆大小）、-Xmn（新生代大小）
- **栈参数**：-Xss（线程栈大小）
- **GC参数**：-XX:+PrintGC、-XX:+PrintGCDetails、-XX:+UseG1GC

### 5.5 性能调优
- **分析工具**：jps、jstat、jmap、jstack、jinfo
- **调优策略**：合理设置堆大小、选择合适的垃圾收集器、优化代码

## 6. Spring

### 6.1 Spring核心概念
- **IoC**：控制反转，将对象的创建和依赖管理交给Spring容器
- **DI**：依赖注入，Spring容器自动为对象注入依赖
- **AOP**：面向切面编程，将横切关注点与业务逻辑分离

### 6.2 Spring容器
- **BeanFactory**：基础容器，延迟加载
- **ApplicationContext**：高级容器，立即加载，提供更多功能

### 6.3 Bean生命周期
1. 实例化Bean
2. 依赖注入
3. 调用BeanNameAware.setBeanName()
4. 调用BeanFactoryAware.setBeanFactory()
5. 调用ApplicationContextAware.setApplicationContext()
6. 调用BeanPostProcessor.postProcessBeforeInitialization()
7. 调用初始化方法（init-method）
8. 调用BeanPostProcessor.postProcessAfterInitialization()
9. Bean就绪
10. 调用DisposableBean.destroy()
11. 调用销毁方法（destroy-method）

### 6.4 Bean作用域
- **singleton**：单例（默认）
- **prototype**：原型
- **request**：请求
- **session**：会话
- **globalSession**：全局会话

### 6.5 Spring AOP
- **通知**：Before、After、AfterReturning、AfterThrowing、Around
- **切点**：定义通知应用的位置
- **连接点**：所有可能应用通知的位置
- **切面**：通知和切点的组合
- **实现原理**：JDK动态代理（接口）、CGLIB动态代理（类）

### 6.6 Spring事务
- **事务特性**：原子性、一致性、隔离性、持久性
- **隔离级别**：DEFAULT、READ_UNCOMMITTED、READ_COMMITTED、REPEATABLE_READ、SERIALIZABLE
- **传播行为**：REQUIRED、SUPPORTS、MANDATORY、REQUIRES_NEW、NOT_SUPPORTED、NEVER、NESTED
- **实现原理**：AOP

### 6.7 Spring MVC
- **请求流程**：
  1. 客户端发送请求到DispatcherServlet
  2. DispatcherServlet根据请求路径调用HandlerMapping获取HandlerExecutionChain
  3. DispatcherServlet调用HandlerAdapter处理请求
  4. HandlerAdapter调用Controller处理业务逻辑
  5. Controller返回ModelAndView
  6. DispatcherServlet调用ViewResolver解析视图
  7. ViewResolver返回具体视图
  8. DispatcherServlet渲染视图并返回响应

### 6.8 Spring Boot
- **核心特性**：自动配置、 starters、嵌入式容器、生产就绪
- **启动流程**：
  1. 加载主类
  2. 初始化SpringApplication
  3. 执行run方法
  4. 刷新应用上下文
  5. 启动嵌入式容器

## 7. 微服务

### 7.1 微服务架构
- **核心概念**：将应用拆分为多个独立的服务，每个服务专注于特定的业务功能
- **优势**：灵活性高、可扩展性强、技术栈多样化、容错性好
- **挑战**：分布式复杂性、服务治理、数据一致性、监控运维

### 7.2 服务注册与发现
- **Eureka**：Netflix开源，AP设计，支持自我保护
- **Consul**：HashiCorp开源，CP设计，支持健康检查
- **ZooKeeper**：Apache开源，CP设计，强一致性

### 7.3 服务调用
- **RESTful API**：基于HTTP协议
- **RPC**：如Dubbo、gRPC

### 7.4 负载均衡
- **客户端负载均衡**：如Ribbon
- **服务端负载均衡**：如Nginx

### 7.5 服务熔断与降级
- **Hystrix**：Netflix开源，防止服务雪崩
- **Sentinel**：阿里巴巴开源，轻量级熔断降级库

### 7.6 API网关
- **Zuul**：Netflix开源
- **Gateway**：Spring Cloud官方

### 7.7 配置中心
- **Config**：Spring Cloud官方
- **Nacos**：阿里巴巴开源

### 7.8 分布式事务
- **2PC**：两阶段提交
- **3PC**：三阶段提交
- **TCC**：Try-Confirm-Cancel
- **Saga**：长事务协调
- **本地消息表**：基于消息队列

### 7.9 分布式锁
- **基于Redis**：使用SETNX命令
- **基于ZooKeeper**：使用临时节点
- **基于数据库**：使用唯一索引

## 8. 数据库

### 8.1 数据库基础
- **关系型数据库**：MySQL、Oracle、PostgreSQL
- **非关系型数据库**：Redis、MongoDB、Elasticsearch
- **数据库三范式**：第一范式（原子性）、第二范式（无部分依赖）、第三范式（无传递依赖）

### 8.2 MySQL
- **存储引擎**：InnoDB（默认，支持事务）、MyISAM（不支持事务）
- **索引**：B+树索引、哈希索引、全文索引
- **事务**：ACID特性
- **锁**：表锁、行锁、间隙锁
- **SQL优化**：使用索引、避免全表扫描、优化JOIN语句

### 8.3 数据库设计
- **ER模型**：实体-关系模型
- **表设计**：合理设计表结构、字段类型、索引
- **分库分表**：水平分库分表、垂直分库分表

### 8.4 数据库索引
- **索引类型**：主键索引、唯一索引、普通索引、组合索引
- **索引原理**：B+树结构，提高查询效率
- **索引失效**：全值匹配、最左前缀原则、范围查询、函数操作

### 8.5 事务隔离级别
- **READ UNCOMMITTED**：读未提交，可能导致脏读
- **READ COMMITTED**：读已提交，可能导致不可重复读
- **REPEATABLE READ**：可重复读，可能导致幻读（MySQL默认）
- **SERIALIZABLE**：串行化，完全隔离

### 8.6 数据库优化
- **硬件优化**：增加内存、使用SSD
- **配置优化**：调整参数
- **SQL优化**：编写高效SQL
- **索引优化**：合理创建索引
- **分库分表**：解决大数据量问题

## 9. 数据结构与算法

### 9.1 数据结构
- **线性结构**：数组、链表、栈、队列
- **非线性结构**：树、图、堆、哈希表
- **特殊结构**：跳表、布隆过滤器、LRU缓存

### 9.2 算法
- **排序算法**：冒泡排序、选择排序、插入排序、归并排序、快速排序、堆排序
- **查找算法**：线性查找、二分查找、哈希查找
- **图算法**：深度优先搜索、广度优先搜索、最短路径算法
- **动态规划**：解决最优子结构问题
- **贪心算法**：解决局部最优问题
- **回溯算法**：解决组合优化问题

### 9.3 时间复杂度
- **O(1)**：常数时间
- **O(log n)**：对数时间
- **O(n)**：线性时间
- **O(n log n)**：线性对数时间
- **O(n²)**：平方时间
- **O(2ⁿ)**：指数时间

## 10. Web知识

### 10.1 HTTP协议
- **HTTP/1.0**：无连接、无状态
- **HTTP/1.1**：持久连接、管道化、分块传输
- **HTTP/2.0**：二进制传输、多路复用、头部压缩
- **HTTP/3.0**：基于QUIC协议

### 10.2 HTTPS
- **原理**：HTTP + SSL/TLS
- **加密方式**：对称加密、非对称加密、数字证书

### 10.3 Cookie与Session
- **Cookie**：存储在客户端，有大小限制
- **Session**：存储在服务端，通过Cookie或URL重写传递SessionID

### 10.4 过滤器与监听器
- **Filter**：过滤请求和响应
- **Listener**：监听容器事件

### 10.5 Servlet
- **生命周期**：初始化（init）、服务（service）、销毁（destroy）
- **工作原理**：基于请求-响应模型

## 11. Redis

### 11.1 Redis基础
- **数据类型**：String、List、Set、Hash、ZSet、Bitmap、HyperLogLog、Geo
- **持久化**：RDB（快照）、AOF（追加文件）
- **内存管理**：过期策略、淘汰策略

### 11.2 Redis高级特性
- **发布订阅**：消息队列
- **事务**：MULTI、EXEC、DISCARD、WATCH
- **Pipeline**：批量执行命令
- **Lua脚本**：原子操作
- **集群**：主从复制、哨兵模式、Cluster集群

### 11.3 Redis应用场景
- **缓存**：热点数据缓存
- **计数器**：访问量、点赞数
- **分布式锁**：基于SETNX
- **消息队列**：发布订阅
- **会话存储**：Session共享

## 12. Tomcat

### 12.1 Tomcat架构
- **Server**：服务器，包含多个Service
- **Service**：服务，包含Connector和Engine
- **Connector**：连接器，处理HTTP请求
- **Engine**：引擎，处理请求
- **Host**：虚拟主机
- **Context**：Web应用

### 12.2 Tomcat启动过程
1. 加载配置文件
2. 初始化Server
3. 初始化Service
4. 初始化Connector
5. 初始化Engine
6. 启动Server

### 12.3 Tomcat优化
- **JVM优化**：设置合理的堆大小
- **线程池优化**：调整最大线程数
- **连接数优化**：调整最大连接数
- **静态资源优化**：使用Nginx处理静态资源

## 13. 设计模式

### 13.1 创建型模式
- **单例模式**：确保只有一个实例
- **工厂模式**：工厂方法、抽象工厂
- **建造者模式**：分步构建复杂对象
- **原型模式**：通过复制创建对象

### 13.2 结构型模式
- **适配器模式**：转换接口
- **装饰器模式**：动态添加功能
- **代理模式**：控制对象访问
- **组合模式**：树形结构
- **外观模式**：提供统一接口
- **桥接模式**：分离抽象和实现
- **享元模式**：共享对象

### 13.3 行为型模式
- **观察者模式**：发布-订阅
- **策略模式**：算法族
- **模板方法模式**：固定算法骨架
- **命令模式**：命令封装
- **职责链模式**：链式处理请求
- **状态模式**：状态转换
- **访问者模式**：分离算法和数据结构
- **中介者模式**：减少对象间耦合
- **迭代器模式**：遍历集合
- **解释器模式**：语法解释
- **备忘录模式**：保存对象状态

## 14. 分布式

### 14.1 分布式基础
- **CAP理论**：一致性、可用性、分区容错性，三者不可兼得
- **BASE理论**：基本可用、软状态、最终一致性
- **分布式系统挑战**：网络延迟、网络分区、节点故障、数据一致性

### 14.2 分布式ID
- **UUID**：全局唯一，但无序
- **雪花算法**：Twitter开源，有序
- **数据库自增**：基于数据库
- **Redis自增**：基于Redis

### 14.3 一致性哈希
- **原理**：将节点和数据映射到哈希环上
- **优势**：节点增减时只影响相邻节点
- **虚拟节点**：解决数据分布不均问题

### 14.4 负载均衡算法
- **轮询**：依次分配
- **随机**：随机分配
- **权重**：基于权重分配
- **一致性哈希**：基于哈希值分配
- **最少连接**：选择连接数最少的节点

## 15. 性能优化

### 15.1 系统性能指标
- **响应时间**：从请求到响应的时间
- **吞吐量**：单位时间处理的请求数
- **并发数**：同时处理的请求数
- **CPU使用率**：CPU使用情况
- **内存使用率**：内存使用情况
- **磁盘IO**：磁盘读写速度
- **网络IO**：网络传输速度

### 15.2 代码优化
- **算法优化**：选择合适的算法
- **数据结构优化**：选择合适的数据结构
- **避免重复计算**：缓存计算结果
- **减少IO操作**：批量处理、使用缓存
- **避免创建过多对象**：使用对象池

### 15.3 数据库优化
- **索引优化**：合理创建索引
- **SQL优化**：编写高效SQL
- **分库分表**：解决大数据量问题
- **读写分离**：提高并发能力

### 15.4 缓存优化
- **多级缓存**：本地缓存 + 分布式缓存
- **缓存策略**：LRU、LFU、FIFO
- **缓存穿透**：布隆过滤器
- **缓存击穿**：热点数据过期
- **缓存雪崩**：大量缓存同时过期

### 15.5 并发优化
- **线程池**：合理使用线程池
- **减少锁竞争**：使用无锁数据结构
- **异步处理**：使用消息队列

## 16. Linux

### 16.1 常用命令
- **文件操作**：ls、cd、pwd、mkdir、rm、cp、mv
- **文本处理**：cat、grep、sed、awk
- **进程管理**：ps、top、kill、pkill
- **网络管理**：ifconfig、netstat、ss、ping
- **系统监控**：df、du、free、vmstat

### 16.2 Shell脚本
- **变量**：环境变量、局部变量
- **控制结构**：if-else、for、while、case
- **函数**：自定义函数
- **重定向**：输入重定向、输出重定向
- **管道**：| 符号

### 16.3 Docker
- **镜像**：Docker镜像
- **容器**：运行中的镜像实例
- **仓库**：存储镜像
- **Dockerfile**：构建镜像的配置文件
- **Docker Compose**：编排多个容器

## 17. 安全

### 17.1 Web安全
- **XSS攻击**：跨站脚本攻击，注入恶意脚本
- **CSRF攻击**：跨站请求伪造，利用用户身份执行操作
- **SQL注入**：注入恶意SQL语句
- **XXE攻击**：XML外部实体注入
- **SSRF攻击**：服务器端请求伪造
- **文件上传漏洞**：上传恶意文件
- **认证授权漏洞**：绕过认证、越权访问

### 17.2 安全防护
- **输入验证**：验证用户输入
- **输出编码**：编码输出内容
- **使用HTTPS**：加密传输
- **设置Cookie属性**：HttpOnly、Secure、SameSite
- **使用CSRF Token**：防止CSRF攻击
- **使用参数化查询**：防止SQL注入
- **最小权限原则**：授予最小必要权限
- **定期更新**：更新依赖库和系统

## 18. 总结

本文档汇总了Java后端面试中常见的知识点，涵盖了多个技术领域。面试前，建议开发者系统复习这些知识点，结合实际项目经验，做到融会贯通。同时，保持持续学习的习惯，关注技术发展趋势，不断提升自己的技术水平。

祝大家面试顺利，早日拿到心仪的offer！