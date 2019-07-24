# dubbo 的学习

我这里只是通过官网的总结一下,通过看官网把自己理解整理出来,大多数都是官网的东西</br>
[官网地址](http://dubbo.apache.org/zh-cn/docs/user/quick-start.html)

## 一.spring 整合 dubbo框架
1.provider.xml(服务提供者配置文件讲解):
* 定义提供服务接口:**DemoService**
```
package org.apache.dubbo.demo;

public interface DemoService {
    String sayHello(String name);
}

```
* 定义提供服务实现的接口:**DemoServiceImpl**
```
package org.apache.dubbo.demo.provider;
 
import org.apache.dubbo.demo.DemoService;
 
public class DemoServiceImpl implements DemoService {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}

```
* 配置文件代码详解:
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
    http://dubbo.apache.org/schema/dubbo
    http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
 
    <!-- 提供方应用信息，用于计算依赖关系,这个名字是自己定义的,没有具体限制 -->
    <dubbo:application name="hello-world-app" />
 
    <!-- 使用multicast广播注册中心暴露服务地址 有两种写法-->
    <dubbo:registry address="multicast://224.5.6.7:1234" /><!-- 第一种写法 -->
    <dubbo:registry protocol="zookeeper" address="172.16.10.220:2181"/><!-- 第二种写法 -->
 
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
 
    <!-- 声明需要暴露的服务接口 -->  注意这是**接口**,ref指定*实现接口*的类
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService" />
 
    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="org.apache.dubbo.demo.provider.DemoServiceImpl" />
</beans>
```
2.consumer.xml(服务提供者配置文件讲解):
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans        
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        
    http://dubbo.apache.org/schema/dubbo        
    http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
 
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-helloworld-app"  />
 
    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
 
    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="org.apache.dubbo.demo.DemoService" />
</beans>
```

## 二.springboot 整合 dubbo框架

0.*提供服务*和*消费服务*都是在主运行程序使用注解开启dubbo:@EnableDubbo

1.springboot只需要在配置文件application.yml文件配置基本的信息:
```
#提供服务和消费服务配置大体相同

dubbo.application.name=service-provider #服务应用的名字
dubbo.registry.address=zookeeper://127.0.0.1:2181 # 注册中心的地址
dubbo.monitor.protocol=registry #注册中心
```
2.*提供服务* 使用@Service 暴露提供的接口 注意:@Service 是dubbo里面的注解:
```
@Service //dubbo注解 com.alibaba.dubbo.config.annotation.Service
public class DemoServiceImpl implements DemoService {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}
```
3.*消费服务*是通过注解@Reference调用服务的
```
@Reference
private DemoService DemoService;//服务提供者的接口
```
===
