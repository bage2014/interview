# Java 多线程 #

github地址 [https://github.com/bage2014/interview](https://github.com/bage2014/interview)

## 多线程理论基础 ##

### 什么是多线程 ###

### 线程和进程 ###

### 进程间通信 ###
- 管道pipe
- 命名管道FIFO
- 消息队列MessageQueue
- 共享存储SharedMemory
- 信号量Semaphore
- 套接字Socket
- 信号 ( sinal ) 

### 并发和并行 ###

### 创建线程 ###
- 集成Thread类
- 实现Runable接口
- 实现Callable接口

### Object的wait、notify方法 ###

### sleep、 await方法差别###

## 多线程常用类 ##
参考地址 [http://www.importnew.com/21889.html](http://www.importnew.com/21889.html)
### Thread ###

### CountDownLatch ###
- 某线程需要等待多个线程执行完毕，再执行。
- 实现多个线程共同等待，同时开始执行任务，不可重用。(此类似于CyclicBarrier，可重用)

### CyclicBarrier ###
- CyclicBarrier允许一组线程互相等待，直到到达某个公共屏障点。
- 与CountDownLatch不同的是该barrier在释放等待线程后可以重用，所以称它为循环（Cyclic）的屏障

### CountDownLatch、CyclicBarrier差别 ###
- CountDownLatch和CyclicBarrier都能够实现线程之间的等待，只不过它们侧重点不同：
CountDownLatch一般用于某个线程A等待若干个其他线程执行完任务之后，它才执行；
而CyclicBarrier一般用于一组线程互相等待至某个状态，然后这一组线程再同时执行；
- CountDownLatch是不能够重用的，而CyclicBarrier是可以重用的。

### Semaphore ###
- Semaphore是一个计数信号量，它的本质是一个”共享锁”。
- 信号量维护了一个信号量许可集。线程可以通过调用acquire()来获取信号量的许可；当信号量中有可用的许可时，线程能获取该许可；否则线程必须等待，直到有可用的许可为止。 线程可以通过release()来释放它所持有的信号量许可。
- Semaphore其实和锁有点类似，它一般用于控制对某组资源的访问权限。

### Exchanger ###
- 用于进行线程间的数据交换。
- 两个线程通过exchange方法交换数据
- 该工具类的线程对象是成对的

### ThreadLocal ###
参考链接 [https://blog.csdn.net/u012088516/article/details/84067841](https://blog.csdn.net/u012088516/article/details/84067841)、[https://blog.csdn.net/bntx2jsqfehy7/article/details/78315161](https://blog.csdn.net/bntx2jsqfehy7/article/details/78315161)

- 每个Thread 维护一个 ThreadLocalMap 映射表，这个映射表的 key 是 ThreadLocal实例本身，value 是真正需要存储的 Object。
- ThreadLocal 本身并不存储值，它只是作为一个 key 来让线程从 ThreadLocalMap 获取 value。值得注意的是图中的虚线，表示 ThreadLocalMap 是使用 ThreadLocal 的弱引用作为 Key 的，弱引用的对象在 GC 时会被回收。
- ThreadLocalMap使用ThreadLocal的弱引用作为key，如果一个ThreadLocal没有外部强引用来引用它，那么系统 GC 的时候，这个ThreadLocal势必会被回收，这样一来，ThreadLocalMap中就会出现key为null的Entry，就没有办法访问这些key为null的Entry的value，如果当前线程再迟迟不结束的话，这些key为null的Entry的value就会一直存在一条强引用链：`Thread Ref -> Thread -> ThreaLocalMap -> Entry -> value`永远无法回收，造成内存泄漏。
- ThreadLocal里面使用了一个存在弱引用的map, map的类型是ThreadLocal.ThreadLocalMap. Map中的key为一个threadlocal实例。这个Map的确使用了弱引用，不过弱引用只是针对key。每个key都弱引用指向threadlocal。 当把threadlocal实例置为null以后，没有任何强引用指向threadlocal实例，所以threadlocal将会被gc回收。
但是，我们的value却不能回收，而这块value永远不会被访问到了，所以存在着内存泄露。因为存在一条从current thread连接过来的强引用。只有当前thread结束以后，current thread就不会存在栈中，强引用断开，Current Thread、Map value将全部被GC回收。最好的做法是将调用threadlocal的remove方法，这也是等会后边要说的。
- 使用static的ThreadLocal，延长了ThreadLocal的生命周期，可能导致内存泄漏。
- 分配使用了ThreadLocal又不再调用get(),set(),remove()方法，那么就会导致内存泄漏，因为这块内存一直存在。

### 线程池 ###
参考链接 [https://www.cnblogs.com/dongguacai/p/6030187.html](https://www.cnblogs.com/dongguacai/p/6030187.html)