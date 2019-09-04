# Spring 实战4 学习笔记

## Spring的核心
### Spring 之旅
_Spring 依赖两个核心特性，依赖注入(dependency injection,DI)和面向切面编程（aspect oriented programming,AOP）_

Spring 为了降低Java开发的复杂性，采取了以下4种关键策略：
- 基于POJO的轻量级和最小侵入性编程
- 通过依赖注入和面向接口实现松耦合
- 基于切面和惯例进行声明式编程
- 通过切面和模板减少样板式代码


容器是Spring矿建的核心，Spring自带了多个容器实现，归纳两种：
- bean工厂（由org.springframework.beans.factory.beanFactory接口定义）是最简单的容器，提供基本的DI支持
- 应用上下文（由org.springframework.context.ApplicationContext接口定义） 基于BeanFactory构建，并提供应用框架级别的服务

**Spring容器负责创建对象，装配，配置并管理整个生命周期**

bean 生命周期  
1. Spring对bean进行实例化
2. Spring将值和bean的引用注入到bean对应的属性中
3. 如果bean实现了BeanNameAware接口，Spring将bean的ID传递给setBeanName()方法
4. 如果bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
5. 如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用上下文的引用传入进来
6. 如果bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessBeforeInitialization()方法
7. 如果bean实现了InitializingBean接口，Spring将调用它们的afterPropertiesSet()方法，类似地，如果bean使用init method声明了初始化方法，该方法也会被调用
8. 如果bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessAfterInitialization()方法
9. 此时，bean已经准备就绪，可以被应用程序使用，它们将一直驻留在应用上下文中，直到该应用上下文被销毁
10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用destory method声明了销毁方法，该方法也会被调用

![Bean在Spring生命周期](https://i.loli.net/2019/08/31/puJPtUgSWLYo7If.png)

Spring 模块
- Spring核心容器
- Spring的AOP模块
- 数据访问与集成
- Web与远程调用
- Instrumentation
- 测试
![Spring 6大模块](https://i.loli.net/2019/08/31/475ZBrnDwuXIMJy.png)

### 装配 Bean
_创建对象之间协作关系的行为通常称为装配（wiring）,这也是依赖注入（DI）的本质_

Spring三种主要的装配机制：
- 在XML中进行显示配置
- 在Java中进行显示配置
- 隐式的bean发现机制和自动装配

Spring从两个角度来实现自动化装配：
- 组件扫描（component scanning):Spring会自动发现应用上下文中所创建的bean
- 自动装配（autowiring）:Spring自动满足bean之间的依赖

进行显示配置时，JavaConfig是更好的方案，因为它更为强大、类型安全并且对重构友好

### 高级装配
Spring profile 解决了Spring bean要跨各种部署环境的通用问题，在运行时，通过将环境相关的bean与当前激活的profile进行匹配，Spring能够让相同的部署单元跨多种环境运行，而不需要进行重新构建

bean的作用域
- 单例(Singleton):在整个应用中，只创建bean的一个实（默认）
- 原型(Prototype):每次注入或者通过Spring应用上下文获取的时候，都会创建一个新的bean实例
- 会话(Session):在Web应用中，为每个会话创建一个bean实例
-请求(Request):在Web应用中，为每个请求创建一个bean实例

运行时值注入
- 属性占位符(Property placeholder)
- Spring表达式语言(SpEL)

SpEL运算符
![SpEL运算符](https://i.loli.net/2019/08/31/L9Cfgcwqu2bPA8I.png)

### 面向切面的Spring
定义AOP术语
- 通知(Advice):通知定义了切面是什么以及何时使用
    - 前置通知(Before):在目标方法被调用之前调用通知功能
    - 后置通知(After):在目标方法完成之后调用通知，此时不会关心方法的输出是什么
    - 返回通知(After-returning):在目标方法成功执行之后调用通知
    - 异常通知(After-trowing):在目标方法抛出异常后调用通知
    - 环绕通知(Around):通知包裹了被通知方法，在被通知的方法调用之前和调用之后进行自定义的行为
- 连接点(Join point):连接点是在应用执行过程中能够插入切面的一个点，这个点可以是调用方法时、抛出异常时、甚至是修改一个字段时
- 切点(Pointcut):切点的定义会匹配通知所要织入的一个或多个连接点
- 切面(Aspect):切面是通知和切点的结合，通知和切点共同定义了切面的全部内容——它是什么，在何时和何处完成其功能
- 引入(Introduction):引入允许我们向现有的类添加新方法或属性
- 织入(Weaving)织入是把切面应用到目标对象并创建新的代理对象的过程
    - 编译器:切面在目标类编译时被织入(AsoectJ)
    - 类加载期:切面在目标类加载到JVM时被织入（AspectJ 5）
    - 运行期:切面在应用运行的某个时刻被织入(Spring AOP)

![AOP](https://i.loli.net/2019/08/31/rEpT69aYhVyvdxw.png)

Spring AOP 4种类型
- 基于代理的经典Spring AOP
- 纯POJO切面
- @AspectJ注解驱动的切面
- 注入式AspectJ切面(适用于Spring各版本)

Spring AOP关键知识点
- Spring通知是Java编写的
- Spring在运行时通知对象
- Spring只支持方法级别的连接点

AspectJ五个注解定义通知
![AspectJ五个注解定义通知](https://i.loli.net/2019/08/31/m8xnwPQIj6MiTVW.png)

## Web中的Spring

### 构建Spring Web应用程序
![Spring MVC 请求](https://i.loli.net/2019/09/01/IAbGsdHoVKqU1mh.png)
Spring MVC 允许以多种方式将客户端中的数据传送到控制器的处理方法中:
- 查询参数(Query Parameter)
- 表单参数(Form Parameter)
- 路径变量(Path Variable)

使用表单校验注解
![Java 校验 API 提供的校验注解](https://i.loli.net/2019/09/01/pM82weQ5X9yWlbr.png)

### 渲染Web视图
Spring 提供了多种视图解析器，常用的有:
- InternalResourceViewResolver(一般用于JSP)
- TilesViewResolver(用于Apache Tiles)
- FreeMarkerViewResolver(用于FreeMarker)
- VelocityViewResolver(用于Velocity模板视图)
Spring 13种视图解析器
![Spring 13种视图解析器](https://i.loli.net/2019/09/01/qCWS6KLoMVeJjpP.png)


### Spring MVC的高级技术
- 自定义DispatcherServlet配置
- 自定义Filter
- multipart解析器
    - CommonMultipartResolver:使用Jakarta Commons FileUpload解析multipart请求
    - StandardServletMultipartResolver:依赖于Servlet 3.0对multipart请求的支持(始于Spring 3.1)

Spring 提供多种方式将异常转换为响应:
- 特定的Spring异常将会自动映射为指定的HTTP状态码
- 异常上可以添加@ResponseStatus注解，从而将其映射为某一个HTTP状态码
- 在方法上可以添加@ExceptionHandler注解，使其用来处理异常

Spring 的一些默认异常HTTP状态码
![Spring 的一些默认异常HTTP状态码](https://i.loli.net/2019/09/01/j4Khxg6tYcZwmdX.png)

Spring 3.2控制器通知,带有@ControllerAdvice注解的类，这个类会包含一个或多个以下类型的方法:
- @ExceptionHandler注解标注的方法
- @InitBinder注解标注的方法
- @ModelAttribute注解标注的方法

跨重定向请求传递数据
- 使用URL模板以路径变量或查询参数的形式传递数据
- 通过flash属性发送数据

### 使用Spring Web Flow
_Spring Web Flow 建立于Spring MVC之上_

Spring Web Flow 主要三元素
- 状态
- 转移
- 流程数据


### 保护Web应用
Spring Security是基于Spring的应用程序提供声明式安全保护的安全性框架。Spring Security提供了完整的安全性解决方案，它能够在Web请求级别和方法调用级别处理身份认证和授权。因为基于Spring框架，所以Spring Security充分利用了依赖注入和面向切面的技术。

Spring Security 3.2分为11个模块,应用程序的类路径下至少要包含Core和Configuration这两个模块
![Spring Security 3.2分为11个模块](https://i.loli.net/2019/09/02/vEdo3mhILkHPOx5.png)

- 过滤Web请求
- 编写简单的安全性配置
- 拦截请求
- 使用Spring表达式进行安全保护、
- 强制通道的安全性
- 防止跨站请求伪造
- 认证用户
- 保护视图

## 后端中的Spring

### 通过Spring和JDBC征服数据库
_Spring自带了一组数据访问框架，集成了多种数据访问技术_
JDBC种可能导致抛出SQLException的常见问题包括:
- 应用程序无法连接数据库
- 要执行的查询存在语法错误
- 查询中所使用的表和/或列不存在
- 试图插入或更新的数据违反了数据库约束
一方面，JDBC的异常体系过于简单了——实际上，他算不上一种体系。另一方面，Hibernate的异常体系是其本身所独有的(二十个左右的异常)。我们需要数据访问异常要具有描述性而且又与特定的持久化框架无关。

Spring JDBC提供的数据访问异常体系解决了以上两个问题。

Spring将数据访问过程中固定的和可变的部分明确划分为两个不同的类:模板(template)和回调(callback)。模板管理过程中固定的部分，而回调处理自定义的数据访问代码。
![Spring的数据访问模块类负责通用的数据访问功能](https://i.loli.net/2019/09/02/FhDSXVmYIx5Rd7P.png)

配置数据源
- 通过JDBC驱动程序定义的数据源
- 通过JNDI查找数据源
- 连接池的数据源
- 使用嵌入式的数据源
- 使用profile选择数据源

### 使用对象关系映射持久化数据
相对JDBC更复杂的特性:
- 延迟加载(Lazy loading)
- 预先抓取(Eager fetching)
- 级联(Cascading)
Spring对ORM框架的支持提供了与这些框架的集成点以及一些附加的服务:
- 支持集成Spring声明式事务
- 透明的异常处理
- 线程安全的、轻量级的模板类
- DAO支持类
- 资源管理


### 使用NoSQL数据库
- 使用MongoDB持久化文档数据
    - 通过注解实现对象-文档映射
    - 使用MongoTemplate实现基于模板的数据库访问
    - 自动化的运行时Repository生成功能

- 使用Neo4j操作图数据
- 使用Redis操作key-value数据
### 缓存数据
_Spring自身并没有实现缓存解决方案，但是它对缓存功能提供了声明式的支持，能够与多种流行的缓存实现进行集成_

Spring对缓存的支持有两种方式:
- 注解驱动的缓存
- XML声明的缓存

### 保护方法应用
方法级别的安全性是Spring Security Web级别安全性的一个重要补充。

Spring Security提供了三种不同的安全注解:
- Spring Security自带的@Secured注解
- JSR-250的@RolesAllowed注解
- 表达式驱动的注解，包括@PreAuthorize、@PostAuthorize、@PreFilter和@PostFilter
    - 表述方法访问规则
    - 过滤方法的输入和输出


## Spring集成

### 使用远程服务
可以使用的远程调用技术
- 远程方法调用(Remote Method Invocation,RMI)
    - 基于Java，服务端和客户端必须都是Java开发的
    - RMI使用了Java的序列化机制，通过网络传输的对象类型必须要保证在调用两端的Java运行时中是完全相同的版本
    - RMI很难穿越防火墙
- Caucho的Hessian和Burlap
    - Hessian使用二进制消息进行客户端和服务端的交互
    - Hessian二进制消息可以移植到其他非Java的语言中
    - Burlap是基于XML的远程调用技术
    - Hessian和Burlap可以很好的穿透防火墙
- Spring基于HTTP的远程服务
    -HTTP Invoker使用Java标准的对象序列化机制
    - 基于HTTP的远程调用
    - HTTP Invoker客户端和服务端必须要都是Spring应用

- 使用JAX-RPC和JAX-WS的Web Service

### 使用Spring MVC创建REST API
_Spring对REST的支持是构建在Spring MVC之上的_

REST与RPC几乎没有任何关系，RPC是面向服务的，并关注于行为和动作，而REST是面向资源的，强调描述应用程序的事物名词
- 表述性(Representational):REST资源实际上可以用各种形式来进行表述，包括XML、JSON、甚至HTML——最适合资源使用者的任意形式
- 状态(State):当使用REST的时候，我们更关注资源的状态而不是对资源采取的行为
- 转移(Transfer):REST涉及到转移资源数据，它以某种表述性形式从一个应用转移到另一个应用

Spring支持以下方式来创建REST资源:
- 控制器可以处理所有的HTTP方法，包含四个主要的REST方法，GET、PUT、DELETE以及POST。Spring 3.2以上版本还支持PATCH方法
- 借助@PathVariable注解，控制器能够处理参数化的URL
- 借助Spring的视图和视图解析器，资源能够以多种方式进行标鼠，包括将模型数据渲染为XML、JSON、Atom以及RSS的View实现
- 可以使用ContentNegotiatingViewResolver来选择最合适客户端的表述
- 借助@ResponseBody注解和各种HttpMethodConverter实现，能够替换基于视图的渲染方式
- 类似地，@ResquestBody注解以及HttpMethodConverter实现可以将传入的HTTP数据转化为传入控制器处理方法的Java对象
- 借助RestTemplate，Spring应用能够方便的使用REST资源

### Spring消息
采用同步通信机制访问远程服务的客户端存在几个限制，主要如下:
- 同步通信意味着等待，当客户端调用远程服务方法时，它必须等待远程方法结束后才能继续执行。如果客户端与远程服务频繁通信，或者远程服务响应很慢，就会对客户端应用的性能带来负面影响
- 客户端通过服务端接口与远程服务相耦合，如果服务的接口发生变化，此服务的所有客户端都需要做相应的改变
- 客户端与远程服务的位置耦合。客户端必须配置网络位置，这样它才知道如何与远程服务进行交互。如果网络拓扑进行调整，客户端也需要重新配置新的网络位置
- 客户端与服务的可用性相耦合，如果远程服务不可用，客户端实际也无法正常运行

异步通信如果解决这些问题
- 无需等待
- 面向消息和解耦
- 位置独立
- 确保投递

四种标准的AMQP Exchange：
- Direct:如果消息的routing key与binding的routing key直接匹配的话，消息将会路由到该队列上
- Topic:如果消息的routing key与binding的routing key符合通配符匹配的话，消息将会路由到该队列上
- Headers:如果消息参数表中的头信息和值都与binding参数表中相匹配，消息将会路由到该队列上
- Fanout:不管消息的routing key和参数表的头信息/值时什么，消息将会路由到所有队列上

### 使用WebSocket和STOMP实现消息功能
WebSocket协议提供了通过一个套接字实现全双工通信的功能
Spring 4.0为WebSocket通信提供了支持，包括:
- 发送和接受消息的底层级API
- 发送和接受消息的高级API
- 用来发送消息的模板
- 支持SockJS,用来解决浏览器端、服务器以及代理不支持WebSocket的问题

SockJS会优先选用WebSocket,但是如果WebSocket不可用的话，它将会从如下的方案中挑选最优的可行方案:
- XHR流
- XDR流
- iFrame事件源
- iFrame HTML文件
- XHR轮询
- XDR轮询
- iFrame XHR轮询
- JSONP轮询

### 使用Spring发送Email

### 使用JMX管理Spring Bean
Spring对DI的支持是通过在应用中配置bean属性，这是一种非常不错的方法。不过，一旦应用已经部署并且正在运行，单独使用DI并不能帮助我们改变应用的配置。假设我们希望深入了解正在运行的应用并要在运行时改变应用的配置，此时，就可以是使用Java管理扩展(Java Mangement Extensions，JMX)
JMX规范定义了如下4种类型的MBean:
- 标准MBean:标准MBean的管理接口时通过在固定的接口上执行反射确定的，bean类会实现这个接口
- 动态MBean:动态MBean的管理接口是运行时通过调用DynamicMBean接口的方法来确定的，因为管理接口不是通过静态接口定义的，因此可以在运行时改变
- 开发MBean:开放MBean是一种特殊的动态MBean，其属性和方法只限定于原始类型、原始类型的包装类以及可以分解为原始类型或原始类型包装类的任意类型
- 模型MBean:模型MBean也是一种特殊的动态MBean，用于充当管理接口与受管资源的中介，模型Bean并不像它们所声明的那样来编写。它们通常通过工厂生成，工厂会使用元信息来组装管理接口

### 借助Spring Boot简化Spring 开发