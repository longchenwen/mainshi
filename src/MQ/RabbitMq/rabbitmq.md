# rabbitmq 学习
## RabbitMQ 简介:
RabbitMQ是使用Erlang语言开发的开源消息队列系统,基于AMQP协议来实现的.AMQPDE 主要特征是面向消息,队列,路由(包括点对点和发布/订阅),可靠性,安全.AMQP协议更多用在企业系统内,对数据一致性,稳定性和可靠性要求很高的场景,对性能和吞吐量的要求还在其次.
## 为什么要使用RabbitMq?
  1.开源,性能优秀,稳定性保证
  2.**提供可靠性消息投递(confirm),返回模式(return)**
  3.与SpringAMQP整合完美,API丰富
  4.集群模式丰富,表达式配置,高可用模式(ha),镜像队列模式
  5.保证在数据不丢失的前提下做到高可靠性,可用性
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
