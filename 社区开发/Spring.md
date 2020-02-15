# Spring

## Spring 概述

Spring是什么？ 企业级 Java 应用程序开发框架

* 依赖注入（DI） Spring 最认同的技术是控制反转的依赖注入模式。依赖注入是控制反转的一个具体例子。
* 面向切面的程序设计（AOP）：一个程序中跨越多个点的功能被称为横切关注点，他们独立于应用程序的业务逻辑，比如日志记录，声明性事物、安全性和缓存等。AOP 将横切关注点从他们所影响的对象中分离出来，（依赖注入将程序从对象中彼此分离出来）Spring框架的AOP模块提供了面向方面的程序设计实现，可以定义诸如方法拦截器和切入点等从而使实现功能的代码彻底的解藕出来。

## Spring 体系结构

<img src="/home/tqr/Study-Notes/社区开发/第一章/image/arch1.png" alt="https://atts.w3cschool.cn/attachments/image/wk/wkspring/arch1.png" style="zoom:75%;" />

### 核心容器（Core Container）

* spring-core：提供了框架的基本组成部分，包括IoC和依赖注入功能
* spring-beans提供BeanFactory，工厂模式的微妙实现。将配置和依赖从实际编码逻辑中解耦
* spring-Expression提供功能强大的表达式语言
* context 模块在core和beans的基础上建立起来，以一种类似于JNDI注册的方式访问对象。

依赖关系 core依赖commons-logging ；bean依赖core，expression 依赖core，context 依赖（core，beans，aop，expression）

### 数据访问/集成

* JDBC(java data base connectivity) 提供了JDBC抽象层。
* ORM (object relational mapping)对流行的地向关系影射API 的集成，包括JPA、JDO、Hibernate等，让这些ORM 框架可以和spring的其他功能整合。
* OXM (object xml mapping)提供了OXM的支持
* JMS (java message service) 包含生成和消费消息的功能
* 事物模块，声明式时无管理通过注解或者配置由spring自动处理。与此相对的是编程式事物管理，需要自己写beginTransaction()、commit()、rollback()等事务管理方法但是编程式事物的粒度更细。

### web

* web提供面向web的基本功能和面向web的应用上下文。
* web-mvc 提供了mvc和REST Web服务的实现
* web-socket 为WebSocket-based提供了支持，而且在web应用程序中提供了客户端和服务器端之间通信的两种方式。
* web-portlet 提供了用于Portlet环境的mvc实现。

### 其他重要模块

* AOP 提供了面向方面的编程实现，允许定义方法拦截器和切入点对代码进行干净地解耦
* Aspects 提供了与AspectJ的集成，是一个功能强大且成熟的面向切面编程（AOP）框架
* Instrumentation 在一定的应用服务器中提供了类instrumentation的值器和类加载器的实现
* **Messaging** 模块为 STOMP（Simple (or Streaming) Text Orientated Messaging Protocol，简单(流)文本定向消息协议） 提供了支持作为在应用程序中 WebSocket 子协议的使用。
* 测试模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

