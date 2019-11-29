###1.volatile关键字与内存可见性问题
* volatile 关键字：当多个线程进行操作共享数据时，可以保证内存中的数据可见，相较于 synchronized 是一种较为轻量级的同步策略
*  注意：

		1.volatile 不具备“互斥性” （多个线程可以同时访问同一个共享变量，而synchronized访问变量：当某一线程访问是将变量添加一个锁，直到当前线程访问结束，释放当前变量，才能被其他线程访问）
		2.volatile 不能保证变量的“原子性”
###2.原子变量与CAS算法
* 一、i++ 的原子问题：i++ 的操作实际上分为三个步骤：读-改-写

		int i = 10;
		i = i++;
		请问此时的i = ？
		此时i = 10；
		
		
		i++的过程：
		int temp = i;
		i = i + 1;
		i = temp;
		
* 二、原子变量：jdk1.5之后，java.util.concurrent.atomic包下提供了常量的原子变量：

		1. volatile  保证内存可见性
		2. CAS（Compare-And—Swap）算法保证数据的原子性
		       CAS 算法是硬件对于并发操作共享数据的支持
		       CAS 包含三个操作数：
		       内存值 V
		       预估值 A
		       更新值 B
		       当且仅当 V == A 时，V = B；否则不做任何操作
###3.同步容器类ConcurrentHashMap
* CopyOnWriteArrayList/CopyOnWriteArraySet：写入并复制
* 注意：添加操作多时，效率低，因为每次添加时都会进行复制，开销非常大。并发迭代操作多时可以选择。
###4.闭锁
* CountDownLatch：闭锁，在完成某些运算时，只有其他所有线程的运算全部完成，当前运算才能继续执行
###5.创建执行线程方式三
* 创建执行线程方式三：实现Callable接口，相较于实现Runnable接口的方式，方法可以有返回值，并且可以抛出异常
* 执行Callable方式，需要FutureTask实现类支持，用于接收运算结果，FutureTask是Future接口的实现类
###6.同步锁
* 用于解决多线程安全问题的方式

		synchronized：隐式锁
		1. 同步代码块
		2. 同步方法
		
		jdk1.5之后
		3. 同步锁 Lock：显示锁  注意：需要通过lock()方法上锁，必须通过unlock()方法释放锁
###7.线程八锁
* 线程八锁的关键：

		1.非静态方法的锁默认为this，静态方法的锁为对应的Class实例
		2.某一时刻，只能有一个线程持有锁，无论几个方法
###8.线程池
* 一、线程池：提供一个线程队列，队列中保存真所有等待状态的线程。避免创建和销毁的额外开销，提高了响应的速度。
* 二、线程池的体系结构：

		java.util.concurrent.Executor  :  负责线程的使用与调度的根接口
		    |-- ExecutorService  子接口：线程池的主要接口
		         |-- ThreadPoolExecutor  线程池的实现类
		         |-- ScheduledExecutorService 子接口：负责线程池的调度
		              |-- ScheduledThreadPoolExecutor  继承 ThreadPoolExecutor 实现  ScheduledExecutorService
* 三、工具类：Executors

	|返回值                   |方法名                     |注释
	|------------------------|--------------------------|-------------------------
	|ExecutorService         |newFixedThreadPool()      |创建固定大小的线程池
	|ExecutorService         |newCachedThreadPool()     |缓存线程池，线程池的数量不固定，可以根据需求自动更改数量
	|ExecutorService         |newSingleThreadExecutor() |创建单个线程池，线程池中只有一个线程
	|ScheduledExecutorService|newScheduledThreadPool()  |创建固定大小的线程池，可延时或定时执行任务
		
		

