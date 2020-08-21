# Spring 框架入门教程
[链接](http://c.biancheng.net/spring/)

## Spring 是什么
Spring 是另一个主流的Java Web开发框架，该框架是一个轻量级的应用框架，具有很高的凝聚力和吸引力。Spring框架因其强大的功能以及卓越的性能而受到众多开发人员的喜爱。

Spring 是分层的Java SE/EE full-stack轻量级框架，以IOC(Inverse of Control,控制反转)和AOP(Aspect Oriented Programming,面向切面编程)为内核，使用基本的JavaBean完成以前只可能由EJB完成的工作，取代了EJB臃肿和低效的开发模式。

Spring框架的主要优点：
- 方便解耦，简化开发
    > Spring就是一个大工厂，可以将所有对象的创建和依赖关系的维护交给Spring管理

- 方便集成各种优秀框架
    > Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如 Struts2、Hibernate、MyBatis等）的直接支持。

- 降低Java EE API的使用难度
    > Spring对Java EE开发中非常难用的一些API（JDBC、JavaMail、远程调用等）都提供了封装，使这些API应用的难度大大降低。

- 方便程序的测试
    > Spring 支持 Junit4，可以通过注解方便地测试Spring程序。

- AOP编程的支持
    > Spring 提供面向切面编程，可以方便地实现对程序进行权限拦截和运行监控等功能。

- 声明式事务的支持
    > 只需要通过配置就可以完成对事务的管理，而无须手动编程。

## Spring体系结构详解
Spring 框架采用分层架构，根据不同的功能被划分成了多个模块，这些模块大体可以分为Data Access/Integration、Web、AOP、Aspects、Messaging、Instrumentation、Core Container和Test

![Spring Framework Runtime](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/Spring.png)

- Data Access/Integration(数据访问/集成)
数据访问/集成层包括JDBC、ORM、OXM、JMS和Transactions模块，具体介绍如下
    - JDBC模块：提供了一个JDBC抽象层，大幅度减少了在开发过程中对数据库操作的编码。
    - ORM模块：对流行的对象关系映射API、包括JPA、JDO、Hibernate和iBatis提供了的集成层。
    - OXM模块：提供了一个支持对象/XML映射的抽象层实现，如JAXB、Castor、XMLBeans、JiBX和XStream。
    - JMX模块：旨Java消息服务，包含的功能为生产和消费的信息。
    - Transactions事务模块：支持编程和声明式事务管理实现特殊接口类，并为所有的POJO。
    