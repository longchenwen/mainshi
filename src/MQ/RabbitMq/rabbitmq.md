# rabbitmq 学习
## RabbitMQ 简介:
RabbitMQ是使用Erlang语言开发的开源消息队列系统,基于AMQP协议来实现的.AMQPDE 主要特征是面向消息,队列,路由(包括点对点和发布/订阅),可靠性,安全.AMQP协议更多用在企业系统内,对数据一致性,稳定性和可靠性要求很高的场景,对性能和吞吐量的要求还在其次.
## 为什么要使用RabbitMq?
  1.开源,性能优秀,稳定性保证</br>
  2.**提供可靠性消息投递(confirm),返回模式(return)**</br>
  3.与SpringAMQP整合完美,API丰富</br>
  4.集群模式丰富,表达式配置,高可用模式(ha),镜像队列模式</br>
  5.保证在数据不丢失的前提下做到高可靠性,可用性</br>
## 什么是AMQP高级消息队列协议?
**AMQP（Advanced Message Queue Protocol 高级消息队列协议）**：是一个网络协议，它支持符合条件的客户端和消息代理中间件（message middleware broker）进行通讯.</br>
AMQP协议模型图:![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/AMQP%E5%8D%8F%E8%AE%AE%E6%A8%A1%E5%9E%8B%E5%9B%BE.jpg)
## AMQP核心概念:
  1. server:又称Broker, 接收和分发消息的应用，接受客户端的连接,RabbitMQ Server就是Message Broker.
  2. Connection: publisher/consumer和broker之间的TCP连接。断开连接的操作只会在client端进行，Broker不会断开连接，除非出现网络故障或broker服务出现问题.应用程序和Broker的网络连接.
  3. Channel:网络信道,几乎所有的操作都在channel中进行,channel是进行消息读写的通道.客户端可以建立多个channel,每个channel代表一个会话任务.
  4. Message:消息,服务器和应用程序之间传送的数据,由Properties和Body组成.Properties可以对消息进行修饰,比如消息的优先级,延迟等高级特性,body真是消息体内容.
  5. Virtual Host:虚拟主机,出于多租户和安全因素设计的，把AMQP的基本组（交换机，队列，绑定称为AMQP组件）件划分到一个虚拟的分组中，类似于网络中的namespace概念。当多个不同的用户使用同一个borker（RabbitMQ server）提供的服务时，可以划分出多个vhost，每个用户在自己的vhost创建exchange／queue等.(用于进行逻辑隔离最上层的消息路由.一个Virtual Host里面可以有若干个Exchange和Queue,同一个Virtual Host里面不能有相同名称的exchange或Queue).
  6. 交换机（Exchange）：交换机负责接收消息并按照一定的规则将消息路由给队列。路由的规则存储在交换机与队列绑定时指定的routing key.
  7. 绑定（Binding）：交换机和队列之间通过路由键（routing key）相互绑定起来，并根据路由键将消息路由到对应队列。
  Routing key:一个路由规则,虚拟机可用他来确定如何路由一个特定消息.
  8. 队列（Queue）：消息的载体，exchange中的消息将被路由到队列中，并推送给消费者或者被消费者取走.
## rabbitMq整体架构图:
![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/rabbitmq%E6%95%B4%E4%BD%93%E6%9E%B6%E6%9E%84%E5%9B%BE.jpg)
## RabbitMQ消息流程图:
![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/rabbitmq%E6%B6%88%E6%81%AF%E6%B5%81%E8%BD%AC%E5%9B%BE.jpg)

## RabbitMq的安装:
  1.采用是rpm格式的安装
  2.启动命令:</br>
  rabbitmq-server start &</br>
  模块启动:rabbitmqctl start_app</br>
  管理插件：rabbitmq-plugins enable rabbitmq_management</br>
## 交换机的类型:
  1. Direct exchange（直连交换机):根据消息携带的路由键（routingkey）将消息投递给对应队列的(Queue)</br>
  **生产者和消费者的routingKey必须相同**
  2. Topic exchange（主题交换机）:队列(queue)通过路由键(routingkey)绑定到交换机上，然后，交换机根据消息里的路由值，将消息路由给一个或多个绑定队列.
  3. Fanout exchange（扇型交换机）:不设置路右键,只需要简单的将队列绑定到交换机上.
## 消息队列: 
  1. 消息队列,实际存储消息数据
  2.Durability:是否持久化, Dutable:是,Transient:否to
  3.Auto delete:如果选择yes,代表当最后一个监听被移除后,该Queue会被自动删除
## 什么是生产端的可靠性投递
1. 保证消息的成功发出
2. 保障MQ节点的成功接收
3. 发送端接收到MQ节点(Broker确认应答)
4. 完善的消息进行补偿机制
## 生产端-可靠性投递解决方案
1. 消息落库,对消息状态进行打标</br>
解析图![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/%E7%94%9F%E4%BA%A7%E7%AB%AF%E5%8F%AF%E9%9D%A0%E6%80%A7%E6%8A%95%E9%80%92(1).png)
    1. 要发送的消息保存到数据库中,消息设置一个状态
    2. 在保证数据库持久化成功之后,发送消息到MQ broker
    3. 在生产端异步监听MQ broker 返回的信息,更改消息的状态(无论返回的是ture或者是false都要改变状态)
    4. 如果step3中断,confirm listenser 一直没有接收消息,消息的状态始终是初始化的状态,这样不对,怎么解决
    5. 解决设置一个定时任务,间隔一定的时间在数据库取出状态是初始化的状态,重新发送信息,知道成功为止.冲次发送的次数做限制,最多几次.

2. 消息的延迟投递,做第二次确认,回调检查




  
