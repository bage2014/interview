# Java 多线程 #

知识点归类github地址 [https://github.com/bage2014/interview](https://github.com/bage2014/interview)
多线程代码demo实现github地址[https://github.com/bage2014/study/tree/master/study-java/src/main/java/com/bage/study/java/multhread](https://github.com/bage2014/study/tree/master/study-java/src/main/java/com/bage/study/java/multhread)

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
参考链接 [https://www.cnblogs.com/dongguacai/p/6030187.html](https://www.cnblogs.com/dongguacai/p/6030187.html)、[https://www.cnblogs.com/szwh/p/7761171.html](https://www.cnblogs.com/szwh/p/7761171.html)、[https://blog.csdn.net/shahuhubao/article/details/80311992](https://blog.csdn.net/shahuhubao/article/details/80311992)、[https://justsee.iteye.com/blog/999189](https://justsee.iteye.com/blog/999189)

#### 构造函数参数 ####

		int corePoolSize = 2; // 核心线程池大小
		int maximumPoolSize = 5; // 最大线程池大小
		long keepAliveTime = 10; // 线程池中超过corePoolSize数目的空闲线程最大存活时间；可以allowCoreThreadTimeOut(true)使得核心线程有效时间
		TimeUnit unit = TimeUnit.SECONDS; // keepAliveTime时间单位
		BlockingQueue<Runnable> workQueue; // 阻塞任务队列
		ThreadFactory threadFactory; // 新建线程工厂
		RejectedExecutionHandler handler; //  当提交任务数超过maxmumPoolSize+workQueue之和时，任务会交给RejectedExecutionHandler来处理

#### BlockingQueue阻塞队列 ####
- ArrayBlockingQueue

基于数组实现的一个阻塞队列，在创建ArrayBlockingQueue对象时必须制定容量大小。并且可以指定公平性与非公平性，默认情况下为非公平的，即不保证等待时间最长的队列最优先能够访问队列。

- LinkedBlockingQueue

基于链表实现的一个阻塞队列，在创建LinkedBlockingQueue对象时如果不指定容量大小，则默认大小为Integer.MAX_VALUE。

- PriorityBlockingQueue[praɪˈɒrəti]

以上2种队列都是先进先出队列，而PriorityBlockingQueue却不是，它会按照元素的优先级对元素进行排序，按照优先级顺序出队，每次出队的元素都是优先级最高的元素。注意，此阻塞队列为无界阻塞队列，即
容量没有上限（通过源码就可以知道，它没有容器满的信号标志），前面2种都是有界队列。

- DelayQueue

基于PriorityQueue，一种延时阻塞队列，DelayQueue中的元素只有当其指定的延迟时间到了，才能够从队列中获取到该元素。DelayQueue也是一个无界队列，因此往队列中插入数据的操作（生产者）永远不会
被阻塞，而只有获取数据的操作（消费者）才会被阻塞。

#### RejectedExecutionHandler拒绝策略 ####
超出线程范围和队列容量的任务的处理程序

- ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出(默认)RejectedExecutionException异常。 
- ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。 
- ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
- ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务
		
#### 提交过程 ####

	1.	校验当前执行的线程数，是否小于 corePoolSize ，小于corePoolSize，则创建一个线程执行任务
	2.	否则，尝试添加到阻塞队列中，如果能够添加，则提交过程结束
	3.	否则，如果阻塞队列已经无法添加，则校验当前线程数是否达到maximumPoolSize个数，如果没有达到，创建新线程执行任务
	4.	否则，如果已经达到maximumPoolSize个数，此时采取RejectedExecutionHandler拒绝策略进行拒绝操作

- 源代码

		public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }

#### excute vs submit ####
- excute 无返回值，submit有返回值
- submit里面还是调用了excute方法

#### shutdown vs shutdownnow ####
- shutDown() 
	1. 线程池的状态编程SHUTDOWN
	2. 不能再往线程池中添加任何任务，否则将会抛出RejectedExecutionException异常。
	3. 此时线程池不会立刻退出，会把线程池中的任务都已经处理完成，才会退出。

- shutdownNow() 
	1. 线程池的状态立刻变成STOP状态，并通过调用Thread.interrupt()方法试图停止所有正在执行的线程，不再处理还在池队列中等待的任务
	2. 它会返回那些未执行的任务。
	3. ShutdownNow()并不代表线程池就一定立即就能退出，它可能必须要等待所有正在执行的任务都执行完成了才能退出。

#### 线程池状态 ####
Running、ShutDown、Stop、Tidying、Terminated

### 线程池默认4个实现类 ###



