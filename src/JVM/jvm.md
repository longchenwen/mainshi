# [BIO、NIO、AIO 基础](https://mp.weixin.qq.com/s/YIcXaH7AWLJbPjnTUwnlyQ)

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

2.NIO：NIO是一种**同步非阻塞IO**, 基于Reactor模型来实现的。</br>
其实相当于就是一个线程处理大量的客户端的请求，通过一个线程轮询大量的channel，每次就获取一批有事件的channel，然后对每个请求启动一个线程处理即可。</br>
这里的核心就是非阻塞，就那个selector一个线程就可以不停轮询channel，所有客户端请求都不会阻塞，直接就会进来，大不了就是等待一下排着队而已。</br>
这里面**优化BIO的核心**就是，一个客户端并不是时时刻刻都有数据进行交互，没有必要死耗着一个线程不放，所以客户端选择了让线程歇一歇，只有客户端有相应的操作的时候才发起通知，创建一个线程来处理请求。</br>
NIO：模型图:![img](https://github.com/longchenwen/mainshi/blob/master/src/img/NIO.webp)
Reactor模型:![img](https://github.com/longchenwen/mainshi/blob/master/src/img/Reactor.webp)

3.AIO:AIO：**异步非阻塞IO**，基于Proactor模型实现。
每个连接发送过来的请求，都会绑定一个Buffer，然后通知操作系统去完成异步的读，这个时间你就可以去做其他的事情
等到操作系统完成读之后，就会调用你的接口，给你操作系统异步读完的数据。这个时候你就可以拿到数据进行处理，将数据往回写
在往回写的过程，同样是给操作系统一个Buffer，让操作系统去完成写，写完了来通知你。
这俩个过程都有buffer存在，数据都是通过buffer来完成读写。
这里面的主要的区别在于将数据写入的缓冲区后，就不去管它，剩下的去交给操作系统去完成。
操作系统写回数据也是一样，写到Buffer里面，写完后通知客户端来进行读取数据。</br>
AIO：模型图: ![img](https://github.com/longchenwen/mainshi/blob/master/src/img/AIO%EF%BC%9A%E6%A8%A1%E5%9E%8B%E5%9B%BE.webp)

## 同步非阻塞:
    为什么说NIO为啥是同步非阻塞？
    因为无论多少客户端都可以接入服务端，客户端接入并不会耗费一个线程，只会创建一个连接然后注册到selector上去，这样你就可以去干其他你想干的其他事了,一个selector线程不断的轮询所有的socket连接，发现有事件了就通知你，然后你就启动一个线程处理一个请求即可，这个过程的话就是非阻塞的。但是这个处理的过程中，你还是要先读取数据，处理，再返回的，这是个同步的过程。
## 异步非阻塞
    为什么说AIO是异步非阻塞？
    通过AIO发起个文件IO操作之后，你立马就返回可以干别的事儿了，接下来你也不用管了，操作系统自己干完了IO之后，告诉你说ok了
    当你基于AIO的api去读写文件时， 当你发起一个请求之后，剩下的事情就是交给了操作系统
    当读写完成后， 操作系统会来回调你的接口， 告诉你操作完成在这期间不需要等待， 也不需要去轮询判断操作系统完成的状态，你可以去干其他的事情。同步就是自己还得主动去轮询操作系统，异步就是操作系统反过来通知你。所以来说， AIO就是异步非阻塞的。



