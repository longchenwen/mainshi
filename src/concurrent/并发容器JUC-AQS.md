# [并发容器JUC-AbstractQueuedSynchronzier(AQS)同步组件](https://blog.csdn.net/qq_34337272/article/details/83655291)

## AQS简介:
        AQS的全称为（AbstractQueuedSynchronizer），这个类在java.util.concurrent.locks包下面.
        AQS是一个用来构建锁和同步器的框架，使用AQS能简单且高效地构造出应用广泛的大量的同步器，
        比如我们提到的ReentrantLock，Semaphore，其他的诸如ReentrantReadWriteLock，
        SynchronousQueue，FutureTask等等皆是基于AQS的。当然，
        我们自己也能利用AQS非常轻松容易地构造出符合我们自己需求的同步器。
## AQS 原理:
        AQS核心思想是，如果被请求的*共享资源*空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。
        如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，
        即将暂时获取不到锁的线程加入到队列中.
## AQS 对资源的共享方式,AQS定义两种资源共享方式
        1.Exclusive（独占）：只有一个线程能执行，如ReentrantLock。又可分为公平锁和非公平锁：
                1.公平锁：按照线程在队列中的排队顺序，先到者先拿到锁
                2.非公平锁：当线程要获取锁时，无视队列顺序直接去抢锁，谁抢到就是谁的
        2.Share（共享）：多个线程可同时执行，Semaphore、CountDownLatCh,CyclicBarrier、ReadWriteLock 
## Semaphore(信号量)
* 允许多个线程同时访问
* synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。
示例代码如下：
```
public class SemaphoreExample1 {
	// 请求的数量
	private static final int threadCount = 550;

	public static void main(String[] args) throws InterruptedException {
		// 创建一个具有固定线程数量的线程池对象（如果这里线程池的线程数量给太少的话你会发现执行的很慢）
		ExecutorService threadPool = Executors.newFixedThreadPool(300);
		// 一次只能允许执行的线程数量。
		final Semaphore semaphore = new Semaphore(20);

		for (int i = 0; i < threadCount; i++) {
			final int threadnum = i;
			threadPool.execute(() -> {// Lambda 表达式的运用
				try {
					semaphore.acquire();// 获取一个许可，所以可运行线程数量为20/1=20
					test(threadnum);
					semaphore.release();// 释放一个许可
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}

			});
		}
		threadPool.shutdown();
		System.out.println("finish");
	}

	public static void test(int threadnum) throws InterruptedException {
		Thread.sleep(1000);// 模拟请求的耗时操作
		System.out.println("threadnum:" + threadnum);
		Thread.sleep(1000);// 模拟请求的耗时操作
	}
}

```
## Semaphore使用的场景:
	控制某个资源被同时访问的个数,例如:mysql仅提供同一时刻20个资源访问
## CountDownLatch （倒计时器）
        1. CountDownLatch 一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待,
           即CountDownLatch允许一个或多个线程等待其他线程完成操作.
        2. 应用的原理:线程阻塞.每执行完一个线程countDown()一次,就是减数一次,直到减到0
## CountDownLatch 的不足:
	CountDownLatch是一次性的，计数器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当CountDownLatch使用完毕后，它不能再次被使用。
## CyclicBarrier(循环栅栏)
	是每个线程相互等待,等到达到设定的计数器时候,所有的线程在执行
