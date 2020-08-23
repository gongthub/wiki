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
    
- Web 模块
Spring的Web层包括Web、Servlet、Struts和Portlet组件，具体介绍如下：
    - Web模块：提供了基本的Web开发集成特性，例如多文件上传功能、使用的Servlet监听器的IoC容器初始化以及Web应用上下文。
    - Servlet模块：包括Spring模型-视图-控制器（MVC）实现Web应用程序。
    - Struts模块：包含支持类内的Spring应用程序，集成了经典的Struts Web层。
    - Portlet模块：提供了在Portlet环境中使用MVC实现，类似Web-Servlet模块的功能。

- Core Container（核心容器）
Spring 的核心容器时其他模块建立的基础，由Beans模块、Core核心模块、COntext上下文模块和Expression Language表达式语言模块组成，具体介绍如下：
    - Beans模块：提供了BeanFactory，是工厂模式的经典实现，Spring将管理对象称为Bean。
    - Core核心模块：提供了Spring框架的基本组成部分，包括IoC和DI功能。
    - COntext上下文模块：建立在核心和Beans模块基础之上，它是访问定义和配置任何对象的媒介。ApplicationContext接口是上下文模块的焦点。
    - Expression Language模块：是运行时查询和操作对象图的强大的表达式语言。

- 其他模块
Spring的其他模块还有AOP、Aspects、Instrumentation以及Test模块，具体介绍如下：
    - AOP模块：提供了面向切面编程实现，允许定义方法拦截器和切入点，将代码按照功能进行分离，以降低耦合性。
    - Aspects模块：提供与AspectJ的集成，是一个功能强大且成熟的面向切面编程（AOP）框架。
    - Instrumentation模块：提供了类工具的支持和类加载器的实现，可以在特定的应用服务器中使用。
    - Test模块：支持Spring组件，使用JUnit或TestNG框架的测试。

## Spring目录结构和基础JAR包介绍

| 名称 | 作用
| --- | ---
| docs | 包含Spring的API文档和开发规范
| libs | 包含开发需要的JAR包和源码包
| schema | 包含开发所需要的schema文件，在这些文件中定义了Spring相关配置文件的约束
在libs目录中，包含了Spring框架提供的所有JAR文件，其中四个JAR文件是Spring框架的基础包，分别对应Spring容器的四个模块：
| 名称 | 作用
| --- | ---
| spring-core-3.2.13.RELEASE.jar | 包含Spring框架基本的核心工具类，Spring其他组件都要用到这个包中的类，是其他组件的基本核心
| spring-beans-3.2.13.RELEASE.jar | 所有应用都要用到的，它包含访问配置文件、创建和管理bean以及进行Inversion of Control（IoC）或者Dependency Injection（DI）操作相关的所有类。
| spring-context-3.2.13.RELEASE.jar | Spring提供在基础IoC功能上的扩展服务，此外还提供许多企业级服务的支持，如邮件服务、任务调度、JNDI定位、EJB集成、远程访问、缓存以及各种视图层的封装等。
| spring-expression-3.2.13.RELEASE.jar | 定义了Spring的表达式语言。需要注意的是，在使用Spring开发时，除了Spring自带的JAR包以外，还需要一个第三方JAR包commons.logging处理日志信息

## Spring Ioc容器
IoC是指在程序开发中，实例的创建不再由调用者管理，而是由Spring容器床架。Spring容器会负责控制程序之间的关系，而不是由程序代码支持控制，因此，控制权由程序代码转移到了Spring容器中，控制权发生了反转，这就是Spring的IoC思想。  
Spring提供了两种IoC容器，分别为BeanFactory和ApplicationContext
- BeanFactory
    > BeanFactory是基础类型的Ioc容器，它由org.springframework.factory.BeanFactory接口定义，并提供了完整的IoC服务支持。简单来说，BeanFactory就是一个管理Bean的工厂，它主要负责初始化各种Bean，并调用他们的生命周期方法。
    BeanFactory接口由多个实现类，最常见的是org.springframework.beans.factory.xml.XmlBeanFactory,它是根据XML配置文件中定义装配Bean的。  
    ```
    BeanFactory beanfactory = new XmlBeanFactory(new FileSystemResource("D://applicationContext.xml"));

- ApplicationContext
    > ApplicationContext是BeanFactory的子接口，也被称为应用上下文。该接口的全路径为org.springframework.context.ApplicationContext,它不仅提供了BeanFactory的所有功能，还添加了对i18n（国际化）、资源访问、事件传播等方面的良好至此。

    ApplicationContext接口有两个常用的实现类，具体如下：
    1. ClassPathXmlApplicationContext
        > 该类从类路径ClassPath中寻找制定的XML配置文件，找到并装载完成ApplicationContext的实例化工作
        ```
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(String configLocation);
    
    2. FileSystemXmlApplicationContext
        > 该类从指定的文件系统路径中寻找指定的XML配置文件，找到并装载完成ApplicationContext的实例化工作
        ```
        ApplicationContext applicationContext = new FileSystemXmlApplicationContext(String configLocation);

        它与ClassPathXmlApplicationContext的区别是：在读取Spring的配置文件时，FileSystemXmlApplicationContext不再从类路径中读取配置文件，而是通过参数指定配置文件的位置，他可以获取类路径之外的资源，如“F:/workspaces/applicationContext.xml”  

        通常在Java项目中，会采用通过ClassPathXmlApplicationContext类实例化ApplicationContext容器方式，而在Web项目中，ApplicationContext容器的实例化工作会交由Web服务器完成。
    
 > 需要注意的是，BeanFactory和ApplicationContext都是通过XML配置文件加载Bean的。  
 > 二者的主要区别在于，如果Bean的某一个属性没有注入，则使用BeanFactory加载后，在第一次调用getBean()方法时会抛出异常，而ApplicationContext则在初始化时自检，这样有利于检查所依赖的属性是否注入。
 > 因此，在实际开发中，通常都选择使用ApplicationContext，而只有在系统资源较少时，才考虑使用BeanFactory