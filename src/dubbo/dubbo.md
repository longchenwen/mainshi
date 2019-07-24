# dubbo 的学习

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
 
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService" />
 
    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="org.apache.dubbo.demo.provider.DemoServiceImpl" />
</beans>
```
2.consumer.xml(服务提供者配置文件讲解):

## 二.springboot 整合 dubbo框架

