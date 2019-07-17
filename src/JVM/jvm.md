# jvm的总结

## BIO、NIO、AIO，分别是什么？
1.**BIO：传统的网络通讯模型，就是BIO，同步阻塞IO**</br>
 它其实就是服务端创建一个ServerSocket， 然后就是客户端用一个Socket去连接服务端的那个ServerSocket， ServerSocket接收到了一个的连接请求就创建一个Socket和一个线程去跟那个Socket进行通讯。
接着客户端和服务端就进行阻塞式的通信，客户端发送一个请求，服务端Socket进行处理后返回响应。
在响应返回前，客户端那边就阻塞等待，上门事情也做不了。
这种方式的缺点：每次一个客户端接入，都需要在服务端创建一个线程来服务这个客户端
这样大量客户端来的时候，就会造成服务端的线程数量可能达到了几千甚至几万，这样就可能会造成服务端过载过高，最后崩溃死掉。</br>
BIO模型图：![img](https://github.com/longchenwen/mainshi/blob/master/src/img/BIO.webp)
