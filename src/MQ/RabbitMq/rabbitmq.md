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
![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/%E7%94%9F%E4%BA%A7%E7%AB%AF%E5%8F%AF%E9%9D%A0%E6%80%A7%E6%8A%95%E9%80%92%E4%BA%8C.png)
     1. 生成两条消息,第二条消息**延迟**第一条消息发送
     2. 消费端接收消息,发送确认消息大MQ broker ,这是回调服务接收到这条确认消息,存储到数据库
     3. 回调服务同时还监听延迟发送的消息,监听到消息和数据库做对比,如果有就完事,没有就补偿
     4. 在回到开始发送消息的服务,让再次发送一次信息.

## 什么是幂等性:
    多次操作的结果始终是相同的. 例如:消费只消费一次即使是多个相同的消息
## 在海量订单产生的业务高峰期,如何避免消息的重复消费问题?(消息的幂等性)
## 主流幂等性操作
1. 唯一ID + 指纹码机制
    1. 利用数据库主键去重
    2. 好处 : 实现简单
    3. 坏处 : 高并发下有数据库写入的性能瓶颈
    4. 根据ID进行分库分表进行算法路由
2. 利用redis原子性实现
    1. 需要关注的问题:是否要进行数据落库, 如果落库的话, 数据库和缓存如何做到原子性
    2. 如果不落库, 数据都存储到缓存中, 如何设置定时同步的策略
## Confirm确认消息
  1. 消息确认的理解:是指生产者投递消息后, 如果Broker收到消息, 则会给我们产生一个应答,生产者进行接收应答, 用来确定这条消息是否正常发送到Broker, 这种方式也是消息的可靠性投递的核心保障
  2. 如何实现Confirm确认消息(生产端)
    1. 在channel上开启确认模式 : channel.confirmSelect()
    2. 在channel上添加监听 : addConfirmListener, 监听成功和失败的返回结果, 根据具体的结果对消息进行重新发送, 或记录日志等后续处理
    		
    	```
    		//添加一个确认监听
		channel.addConfirmListener(new ConfirmListener() {
			@Override //失败
			public void handleNack(long deliveryTag, boolean multiple) throws IOException {
				System.err.println("-------no ack!-----------");
			}
			
			@Override //成功
			public void handleAck(long deliveryTag, boolean multiple) throws IOException {
				System.err.println("-------ack!-----------");
			}
		});
    
## Return消息机制(生产端)
	1. Return Listener用于处理一些不可路由的消息
	2. 正常情况下消息生产者通过指定一个Exchange和RoutingKey, 把消息送到某一个队列中去, 然后消费者监听队列, 进行消费操作
	3. 但在某些情况下, 如果在发送消息的时候, 当前的exchange不存在或者指定的路由key路由不到, 这个时候如果我们需要监听这种不可达的消息, 就要使用Return Listener
	4. 在基础API中有一个关键的配置项Mandatory : 如果为true, 则监听器会接收到路由不可达的消息, 然后进行后续处理, 如果为false, 那么broker端自动删除该消息

## 消费端自定义监听及自定义消费者
```
/**
 * 消费端自定义监听及自定义消费者
 */
public class MyConsumer extends DefaultConsumer {

	public MyConsumer(Channel channel) {
		super(channel);
	}
	/**
	 * 获取到消费的信息
	 * @param consumerTag:消费标签
	 * @param envelope:对象
	 * @param properties:
	 * @param body:
	 * @throws IOException
	 */
	@Override
	public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
		System.err.println("-----------consume message----------");
		System.err.println("consumerTag: " + consumerTag);
		System.err.println("envelope: " + envelope);
		System.err.println("properties: " + properties);
		System.err.println("body: " + new String(body));
	}


}
```
## 什么是消费端限流?
假设一个场景,首先,我们Rabbitmq服务器有上成千上万条未处理的消息,我们随便打开一个消费者客户端,会出现,巨量的消息瞬间全部推送过来,但是我们单个客户端无法同时处理这么多数据,这时候我们就要在消费端限制推送过来的数据的条数.

## 消费端的限流 
1. RabbitMQ提供了一种qos(服务质量保证)功能, 即在非自动确认消息的前提下, 如果一定数目的消息(通过consumer或者channel设置qos的值)未被确认前, 不进行消费新的消息.自动签收要设置成false, 建议实际工作中也设置成false
2. void basicQos(int prefetchSize, int prefetchCount, boolean global) throws IOException;</br>
prefetchSize : 消息大小限制, 一般设置为0, 消费端不做限制</br>
prefetchCount : 会告诉RabbitMQ不要同时给一个消费者推送多于N个消息, 即一旦有N个消息还没有ack, 则该consumer将block(阻塞), 直到有消息ack</br>
global : true/false 是否将上面设置应用于channel, 简单来说就是上面的限制是channel级别的还是consumer级别</br>
注意 :</br>
prefetchSize和global这两项, RabbitMQ没有实现, 暂且不关注, prefetchCount在autoAck设置false的情况下生效, 即在自动确认的情况下这两个值是不生效的

## 消费端ACK和NACK
	1. 消费端的手工ACK和NACK, ACK是确认成功消费, NACK表示消息处理失败, 会重发消息
	2. 消费端进行消费的时候, 如果由于业务异常我们可以进行日志的记录, 然后进行补偿
	3. 如果由于服务器宕机等严重问题, 就需要手工进行ACK保障消费端消费成功
## 消费端重回队列
	1. 消费端重回队列是为了对没有处理成功的消息, 把消息重新回递给Broker
	2.一般在实际应用中, 都会关闭重回队列, 也就是设置为False

## TTL队列/消息
	1.TTL是Time To Live的缩写, 也就是生存时间
	2.RabbitMQ支持消息的过期时间, 在消息发送时可以进行指定
	3.RabbitMQ支持队列的过期时间, 从消息入队列开始计算, 只要超过了队列的超时时间配置, 那么消息会自动清除
	
## RabbitMQ和spring AMQP整合
## 消息模板RabbitTemplate
1. 它是在之前于 SpringAMQP 整合的时候进行发送消息的关键类。
2. 它提供了丰富的发送消息方法，包括可靠性投递消息方法、回调监听消息接口 ConfirmCallback、返回值确认接口 ReturnCallback 等等。同样我们需要进行注入到 Spring 容器中，然后直接使用。RabbitTemplate 在 Spring 整合时需要实例化，但是在 Springboot 整合时，在配置文件里添加配置即可

## SimpleMessageListenerContainer 即简单消息监听容器

关注
SpringAMQP 消息容器 - SimpleMessageListenerContainer
	简述:SimpleMessageListenerContainer 即简单消息监听容器。
	这个类非常的强大，我们可以对他进行很多的设置，用对于消费者的配置项，这个类都可以满足。它有监听单个或多个队列、自动启动、自动声明功能。

1. 它可以设置事务特性、事务管理器、事务属性、事务并发、是否开启事务、回滚消息等。但是我们在实际生产中，很少使用事务，基本都是采用补偿机制。

2. 它可以设置消费者数量、最小最大数量、批量消费。

3. 它可以设置消息确认和自动确认模式、是否重回队列、异常捕获 Handler 函数。

4. 它可以设置消费者标签生成策略、是否独占模式、消费者属性等。

5. 它还可以设置具体的监听器、消息转换器等等。

6.注意: SimpleMessageListenerContainer 可以进行动态设置，比如在运行中的应用可以动态的修改其消费者数量的大小、接收消息的模式等。很多基于 rabbitMQ 的自制定化后端管控台在进行设置的时候，也是根据这一去实现的。所以可以看出 SpringAMQP 非常的强大
## MessageListenerAdapter  消息监听适配器

## MessageConverter 消息转化器
1. 我们在进行,发送消息的时候,正常情况下消息体为二进制的数据方式进行传输,如果希望内部帮我们进行转换,或者指定自定义的转换器,就需要用到MessageConverter	
2. 自定义常用转化器:MessageConverter,一般来讲都需要实现这个接口
3. 重写下面两个方法:
		1.toMzessage:java对象转换为Message对象
		2.fromMessage:Message对象转换为java对象
4. json转换器:Jackson2JsonMessageConverter:可以进场java的转换功能
5. DefaultJackson2JavaTypeMapper映射器:可以进行java对象的映射关系

## RabbitMq 集群架构模式

## 1.主备模式
	实现RabbitMQ的高可用集群，一般在并发和数据量不高的情况下，这种模式非常的好且简单。主备模式也称为Warren模式
	主备模式:主节点提供读写，从节点不提供读写服务，只是负责提供备份服务,备份节点的主要功能是在主节点宕机时，完成自动切换 从-->主
	主从模式:主节点提供读写，从节点只读
1. 主备模式：所谓rabbitmq另外一种模式就是warren（兔子窝），就是一个主/备方案（主节点如果挂了，从节点提供服务而已，和activemq利用zookeeper做主/备一样）如图下所示:
  ![img](https://github.com/longchenwen/mainshi/blob/master/src/MQ/RabbitMq/rabbitmq%E4%B8%BB%E5%A4%87%E6%A8%A1%E5%BC%8F.png)
  ```
  1. HaProxy配置：
  	listen rabbitmq_cluster
  	bind 0.0.0.0:5672 
 	mode tcp  #配置TCP模式
  	balance roundrobin #简单的轮询
  	server bhz76 192.168.11.12:5672 check inter 5000 rise 2 fall 3 #主节点
  	server bhz77 192.168.11.13:5672 backup check inter 5000 rise 2 fall 3 #备用节点
```
**备注**
	rabbitmq集群节点配置 #inter 每隔5秒对mq集群做健康检查，2次正确证明服务器可用，3次失败证明服务器不可用，并且配置主备机制
	
## 2. 镜像模式
* 镜像模式：集群模式非常经典的就是Mirror镜像模式，保证100%数据不丢失，在实际工作中用的最多的。并且实现集群非常的简单，一般互联网大厂都会构建这种镜像集群模式。
* Mirror镜像队列，目的是为了保证rabbitmq数据的高可靠性解决方案，主要就是实现数据的同步，一般来讲是2-3个实现数据同步（对于100%数据可靠性解决方案一般是3个节点）集群架构如下：
