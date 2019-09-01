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

## 后端中的Spring

### 通过Spring和JDBC征服数据库

### 使用对象关系映射持久化数据

### 使用NoSQL数据库

### 缓存数据

### 保护方法应用

## Spring集成

### 使用远程服务

### 使用Spring MVC创建REST API

### Spring消息

### 使用WebSocket和STOMP实现消息功能

### 使用Spring发送Email

### 使用JMX管理Spring Bean

### 借助Spring Boot简化Spring 开发