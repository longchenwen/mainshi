# 并发容器JUC-AbstractQueuedSynchronzier(AQS)同步组件

## CountDownLatch:
        1. CountDownLatch 一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待,
           即CountDownLatch允许一个或多个线程等待其他线程完成操作.
         2. 应用的原理:线程阻塞.每执行完一个线程countDown()一次,就是减数一次
