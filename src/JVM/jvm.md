# BIO、NIO、AIO 基础

## BIO、NIO、AIO，分别是什么？
1.**BIO：传统的网络通讯模型，就是BIO，同步阻塞IO**</br>
 它其实就是服务端创建一个ServerSocket， 然后就是客户端用一个Socket去连接服务端的那个ServerSocket， ServerSocket接收到了一个的连接请求就创建一个Socket和一个线程去跟那个Socket进行通讯。
接着客户端和服务端就进行阻塞式的通信，客户端发送一个请求，服务端Socket进行处理后返回响应。
在响应返回前，客户端那边就阻塞等待，上门事情也做不了。
这种方式的缺点：每次一个客户端接入，都需要在服务端创建一个线程来服务这个客户端
这样大量客户端来的时候，就会造成服务端的线程数量可能达到了几千甚至几万，这样就可能会造成服务端过载过高，最后崩溃死掉。</br>
BIO模型图：![img](https://github.com/longchenwen/mainshi/blob/master/src/img/BIO.webp)

Acceptor：
传统的IO模型的网络服务的设计模式中有俩种比较经典的设计模式：一个是多线程， 一种是依靠线程池来进行处理。
如果是基于**多线程的模式**来的话，就是这样的模式，这种也是Acceptor线程模型。</br>
![img](https://github.com/longchenwen/mainshi/blob/master/src/img/Acceptor.webp)

2.NIO：NIO是一种**同步非阻塞IO**, 基于Reactor模型来实现的。
其实相当于就是一个线程处理大量的客户端的请求，通过一个线程轮询大量的channel，每次就获取一批有事件的channel，然后对每个请求启动一个线程处理即可。
这里的核心就是非阻塞，就那个selector一个线程就可以不停轮询channel，所有客户端请求都不会阻塞，直接就会进来，大不了就是等待一下排着队而已。
这里面优化BIO的核心就是，一个客户端并不是时时刻刻都有数据进行交互，没有必要死耗着一个线程不放，所以客户端选择了让线程歇一歇，只有客户端有相应的操作的时候才发起通知，创建一个线程来处理请求。</br>
NIO：模型图:![img](https://github.com/longchenwen/mainshi/blob/master/src/img/NIO.webp)

