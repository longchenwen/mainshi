# spring的学习

## 1.spring aop(面向切面)作用:
	在不修改源码分前提下,增强方法的功能

## 2.Spring的AOP实现方式有两种：
	1.Java(jdk)代理方式；JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口
	2.Cglib(代理)动态增强方式

## 3.IOC(控制反转)原理;
	1.反射 +配置工厂+文件
	2.创建配置文件,解析配置文件,得到类的全路径,通过 反射得到对象

## 4.spring 管理事物方式:
	1.@Transactional 注解声明事物
	2.Apeectj aop 方式

## 5.spring 事物管理失效:
	1.private 方法, final 方法 和 static 方法不能添加事务

## 6.spring 支持几种 bean 的作用域？
	1.singleton
	2.prototype
	3.request
	4.session
	5.global session

## 7.spring 事务实现方式有哪些？
	1.事物的四种特性:
		1.原子性
		2.隔离性
		3.一致性
		4.持久性
	2.事务的传播特性,spring定义7中传播特性,required 默认:支持当前事物,如果当前没有事物,就新建一个事物
 	事物实现方式:
		1.编程式事务管理(不用)
		2.声明式事务管理
			1.基于TransactionProxyFactoryBean的声明式事务管理
			2.基于@Transactional 的声明式事务管理
		3.基于Aspectj AOP配置事务
		
## Spring 框架中都用到了哪些设计模式？
* 代理模式 — 在 AOP 和 remoting 中被用的比较多。
* 单例模式 — 在 Spring 配置文件中定义的 Bean 默认为单例模式。
* 模板方法 — 用来解决代码重复的问题。比如 RestTemplate、JmsTemplate、JdbcTemplate 。
* 前端控制器 — Spring提供了 DispatcherServlet 来对请求进行分发。
* 视图帮助(View Helper) — Spring 提供了一系列的 JSP 标签，高效宏来辅助将分散的代码整合在视图里。
* 依赖注入 — 贯穿于 BeanFactory / ApplicationContext 接口的核心理念。
* 工厂模式 — BeanFactory 用来创建对象的实例。
		
