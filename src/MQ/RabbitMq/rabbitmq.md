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
**AMQP（Advanced Message Queue Protocol 高级消息队列协议）**：是一个网络协议，它支持符合条件的客户端和消息代理中间件（message middleware broker）进行通讯.
