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

## Groovy与Spring Boot CLI

## 在Spring Boot中使用Grails

## 深入Actuator

## 部署Spring Boot应用程序

## Spring Boot开发者工具

## Spring Boot起步依赖

## 配置属性

## Spring Boot依赖