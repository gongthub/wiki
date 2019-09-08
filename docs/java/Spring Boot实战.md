# Spring Boot实战
## 入门
_Spring 简化Java开发，但Spring使用XML配置，而且是很多XML配置，Spring Boot 简化Spring开发。Spring Boot为Spring应用程序的开发提供了一种激动人心的新方式，框架本身带来的阻力很小，自动配置消除了传统Spring应用程序里的很多样板配置；Spring Boot起步依赖让你能通过库所提供的功能而非名称与版本号来指定构建依赖；Spring Boot CLI将Spring Boot的无阻碍开发模型提升到了一个崭新的高度，在命令行里就能快速的用Groovy进行开发；Actuator让你能深入运行中的应用程序，了解Spring Boot做了什么，是怎么做的。_ 

Spring 开发一个简单的Hello World Web应用程序的一些基本需要:
- 一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件，最起码要有Spring MVC和Servlet API这些依赖
- 一个web.xml文件(或者一个WebApplicationInitializer实现)，其中声明了Spring的DispaterServlet.
- 一个启用了Spring MVC的Spring配置
- 一个控制器类，以"Hello World"相应HTTP请求
- 一个用于部署应用程序的Web应用服务器，比如Tomcat

Spring Boot四个核心:
- 自动配置:针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置。
- 起步依赖:告诉Spring Boot需要什么功能，他就能引入需要的库。
- 命令行界面:这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。
- Actuator:让你能够深入运行中的Spring Boot应用程序，一探究竟。

Spring Boot不是什么:
- Spring Boot不是应用服务器，Spring Boot在应用程序里嵌入了一个Servlet容器(Tomcat、Jetty或Undertow)，以此实现这一功能。但这是内嵌的Servlet容器提供的功能，不是Spring Boot实现的。
- Spring Boot没有实现诸如JPA或JMS(Java Message Service,Java消息服务)之类的企业级Java规范。它的确支持不少企业级Java规范，但是要在Spring里自动配置支持那些特性的Bean。
- Spring Boot没有引入任何形式的代码生成，而是利用了Spring 4的条件化配置特性，以及Maven和Gradle提供的传递依赖解析,以此实现Spring应用程序上下文里的自动配置。

Spring Boot CLI几种安装方式:
- 用下载的分发包进行安装
- 用Groovy Environment Manager进行安装
- 通过OS X Homebrew进行安装
- 使用MacPorts进行安装

Spring Initializ几种用法:
- 通过Web界面使用
- 通过Spring Tool Suite使用
- 通过IntelliJ IDEA使用
- 使用Spring Boot CLI使用

## 开发第一个应用程序
_通过Spring Boot的起步依赖和自动配置，你可以更加快速、便捷地开发Spring应用程序。起步依赖帮助你专注应用程序需要的功能类型，而非提供该功能的具体库和版本，同时自动配置把你从样板式的配置中解放出来_

初始化的Spring Boot项目结构:
- build.gradle:Gradle构建说明文件
- ReadingListApplication.java:应用程序的启动引导类(bootstarp class),也是主要的Spring配置类
- application.properties:用于配置应用程序和Spring Boot的属性
- ReadingListApplicationTests.java:一个基本的集成测试类

启动引导Spring:

ReadingListApplication在Spring Boot应用程序的两个作用，配置和启动引导
@SpringBootApplication开启了Spring的组件扫描和Spring Boot的自动配置功能，
@SpringBootApplication由三个注解组合:
- Spring的@Configuration:标明该类使用Spring基于Java配置
- Spring的@ComponentScan:启用组件扫描，这样你写的Web控制器类和其他组件才能被自动发现并注册为Spring应用程序上下文里的Bean
- Spring Boot的@EnableAutoConfiguration:这个不起眼的小注解也可以称为@Abracadabra，就是这行配置开启了Spring Boot自动配置的魔力，让你不用再写成篇的配置了

使用起步依赖:

指定基于功能的依赖:Spring Boot通过提供众多起步依赖降低项目依赖的复杂度。起步依赖本质上是一个Maven项目对象模型(Project Object Model,POM)，定义了对其他库的传递依赖。
- 覆盖起步依赖引入的传递依赖:可以通过构建工具中的功能，选择性地覆盖它们引入的传递依赖的版本号，排除传递依赖，当然还可以为那些Spring Boot起步依赖没有涵盖的库指定依赖

使用自动配置:

_每当应用程序启动的时候，Spring Boot的自动配置都要做将近200个这样的决定，涵盖安全、集成、持久化、Web开发等诸多方面，所有的这些自动配置就是为了尽量不让你自动写配置_

Spring Boot提供的条件化注解:
![Spring Boot提供的条件化注解:](https://i.loli.net/2019/09/06/T5xBs7zet8A2QNk.png)

## 自定义配置
_使用显示配置进行覆盖和使用属性进行精细化配置_

**覆盖Spring Boot自动配置**

Spring Boot自动配置自带了很多配置类，每一个都能运用在你的应用程序里。它们都使用了Spring 4.0的条件化配置，可以在运行时判断这个配置是该被运用，还是该被忽略。

**通过属性文件外置配置**

Spring Boot自动配置的Bean提供了300多个用于微调的属性，当你调整设置时，只要在环境变量、Java系统属性、JNDI(Java Naming and Directory Interface)、命令行参数或者属性文件里进行指定就好了

Spring Boot能从多种属性源获取属性，包括:
- 命令行参数
- java:comp/env里的JNDI属性
- JVM系统属性
- 操作系统环境变量
- 随机生成的带random.*前缀的属性(在设置其他属性时，可以引用它们，比如${random.long})
- 应用程序以外的application.properties或者application.yml文件
- 打包在应用程序内的application.properties或者application.yml文件
- 通过@PropertySource标注的属性源
- 默认属性

application.properties和application.yml文件能放在以下四个位置:
- 外置，在相对于应用程序运行目录的/config子目录里
- 外置，在应用程序运行的目录里
- 内置，在config包内
- 内置，在Classpath根目录

*这个列表按照优先级排序，也就是说，/config子目录里的application.properties会覆盖应用程序Classpath里的application.properties中的相同属性*



## 测试
_编写单元测试的时候，Spring通常不需要介入，Spring鼓励松耦合、接口驱动的设计，这些都能让你很轻松的编写单元测试。但是集成测试要用到Spring，如果生产应用程序使用Spring来配置并组装组件，那么测试就需要它来配置并组装那些组件。Spring的SpringJUnit4ClassRunner可以在基于Junit的应用程序测试里加载Spring应用程序上下文。在测试Spring Boot应用程序时，Spring Boot除了拥有Spring集成测试支持，还开启了自动配置和Web服务器，并提供了不少实用的测试辅助工具_

*集成测试自动配置*  
自Spring 2.5开始，集成测试支持的形式就变成了SpringJUnit4ClassRunner。这是一个JUnit类运行器，会为JUnit测试加载Spring应用程序上下文，并为测试类自动织入所需的Bean。

除了加载应用程序上下文，SpringJUnit4ClassRunner还能通过自动织入从应用程序上下文里向测试本身注入Bean

- @RunWith:参数是SpringJUnit4ClassRunner.class，开启了Spring集成测试支持
- @ContextConfiguration:指定了如何加载应用程序上下文

*测试Web应用程序*

- Spring Mock MVC:能在一个近似真实的模拟Servlet容器里测试控制器，而不用实际启动应用服务器
- Web集成测试:在嵌入式Servlet容器(比如Tomcat或Jetty)里启动应用程序，在真实的应用服务器里执行测试

这两种方法各有利弊，启动一个应用服务器会比模拟Servlet容器要慢一些，但毫无疑问基于服务器的测试会更接近真实环境，更接近部署到生产环境运行的情况

*测试运行中的应用程序*
- 用随机端口启动服务器
    - 将server.port属性设置为0
    - @WebIntegrationTest的value属性接受一个String数组
    - @WebIntegrationTest(randomport=true)
- 使用Selenium测试Html页面

Spring Framework以JUnit类运行器的方式提供了集成测试支持，JUnit类运行器会加载Spring应用程序上下文，把上下文里的Bena注入测试。Spring Boot在Spring的集成测试之上又增加了配置加载器，以Spring Boot的方式加载应用程序上下文，包括了对外属性的支持和Spring Boot日志。Spring Boot还支持容器内测试Web应用程序，让你能用和生产环境一样的容器启动应用程序。这样一来，测试在验证应用程序行为的时候，会更加接近真实的运行环境。

## Groovy与Spring Boot CLI
_Spring Boot CLI是一个命令行工具，将强大的Spring Boot和Groovy结合在一起，针对Spring应用程序形成了一套简单而又强大的开发工具。_

- Spring Boot CLI项目没有严格的项目结构要求
- 通过Groovy消除代码噪声
- CLI可以利用Spring Boot的自动配置和起步依赖
- CLI可以检测到正在使用的特定类，自动解析适合的依赖库来支持那些类
- CLI知道多数常用类都在哪些包里，如果用到了这些类，它会把那些包加入Groovy的默认包里
- 应用自动依赖解析和自动配置后，CLI可以检测到当前运行的是一个Web应用程序，并自动引入嵌入式Web容器(默认是Tomcat)供应用程序使用

对于CLI无法自动解析的库，基于CLI的应用程序可以利用Grape的@Grab注解，不用构建说明也能显式的声明依赖

## 在Spring Boot中使用Grails
- Grails和Spring Boot都旨在让开发者的生活更简单、大大简化基于Spring的开发模型，因此两者看起来是相互竞争的框架。
- GORM和GSP视图，两个都是知名的Grails特性，，GORM是Spring Boot里一个受欢迎的特性，能让你直接针对领域模型执行持久化操作，消除了对模型仓库的需求。
- Grails3是Grails构建于Spring Boot之上的最新版本。

## 深入Actuator
Spring Boot Actuator提供了很多生产级的特性，比如监控和度量Spring Boot应用程序。Actuator的这些特性可以通过众多的REST端点、远程shell和JMX获得。
Actuator

*揭秘Actuator的端点*  
Spring Boot Actuator关键特性是在应用程序里提供众多Web端点，通过他们了解应用程序运行时的内部状况
![Actuator的端点](https://i.loli.net/2019/09/08/itucJ8qkD9NQE7m.png)
- 查看配置明细
    - 获取Bean装配报告
    - 详解自动配置
    - 查看配置属性
    - 生成端点到控制器的映射
- 运行时度量
    - 查看应用程序的度量值
    - 追踪Web请求
    - 导出线程活动
    - 监控应用程序健康情况
- 关闭应用程序
- 获取应用信息

*连接Actuator的远程shell*  
Spring Boot集成了CRaSH，一种能嵌入任意Java应用程序的shell。Spring Boot还扩展了CRaSH，添加了不少Spring Boot特有的命令，提供了与Actuator端点类似的功能。
- 查看autoconfig报告
- 列出应用程序的Bean
- 查看应用程序的度量信息
- 调用Actuator端点

*通过JMX监控应用程序*  
除了REST端点和远程shell，Actuator还把它的端点以MBean的方式发布了出来，可以通过JMX来查看和管理。
Actuator的端点都发布在org.springframework.boot域下

*定制Actuator*
- 重命名端点
- 启用和禁用端点
- 自定义度量信息
- 创建自定义仓库来存储跟踪信息
- 插入自定义的健康指示器

*保护Actuator端点*  
很多Actuator端点发布的信息都可能设计敏感数据，还有一些端点(比如/shutdown)非常危险，可以用来关闭应用程序。因此，保护这些端点尤为重要，能访问他们只能是那些经过授权的客户端。
Actuator的端点保护可以用和其他URL路径一样的方式，使用Spring Security。

## 部署Spring Boot应用程序

*多种部署方式*  
Spring Boot应用程序有多种构建和运行方式

- 在IDE中运行应用程序(涉及Spring ToolSuite和IntelliJ IDEA)
- 使用Maven的spring-boot:run或Gradle的bootrun,在命令行里运行
- 使用Maven或Gradle生成可运行的JAR文件，随后在命令行中运行
- 使用Spring Boot CLI在命令行中运行Groovy脚本
- 使用Spring Boot CLI来生成可运行的JAR文件，随后在命令行中运行

Spring Boot应用程序可以用多种方式打包  
![Spring Boot部署选项](https://i.loli.net/2019/09/08/IAW2km6GTxMrq4l.png)

*部署到应用服务器*  
- 构建WAR文件
- 创建生产Profile
- 开启数据库迁移
    - Flyway
    - Liquibase

## Spring Boot开发者工具
Spring Boot 1.3引入了一组新的开发者工具，可以让你在开发时更方便地使用Spring Boot，包括功能如下:

- 自动重启:当Classpath里的文件发生变化时，自动重启运行中的应用程序
- LiveReload支持:对资源的修改自动触发浏览器刷新
- 远程开发:远程部署时支持自动重启和LiveReload
- 默认的开发时属性值:为一些属性提供有意义的默认开发时属性值
- 全局配置开发者工具

## Spring Boot起步依赖

## 配置属性

## Spring Boot依赖