# 并发容器JUC-AbstractQueuedSynchronzier(AQS)同步组件

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
## CountDownLatch:
        1. CountDownLatch 一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待,
           即CountDownLatch允许一个或多个线程等待其他线程完成操作.
         2. 应用的原理:线程阻塞.每执行完一个线程countDown()一次,就是减数一次
