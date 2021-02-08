# Spring

## 一、搭建Spring环境

### 1.1下载jar

>http://maven.springframework.org/release/org/springframework/spring/spring-framework-4.3.9.RELEASE-dist.zip

**开发spring至少需要使用的jar(5个+1个):**

>spring-aop.jar        开发AOP特性时需要的JAR
>
>spring-beans.jar       处理Bean的jar <bean>
>
>spring-context.jar      处理spring上下文的jar <context>
>
>spring-core.jar        spring核心jar
>
>spring-expression.jar   spring表达式 
>
>三方提供的日志jar
>
>commons-logging.jar 日志

### 1.2 编写配置文件

为了编写时有一些提示、自动生成一些配置信息：

- **方式一：增加sts插件**

可以给eclipse增加 支持spring的插件：spring tool suite(https://spring.io/tools/sts/all)

下载springsource-tool-suite-3.9.4.RELEASE-e4.7.3a-updatesite.zip,然后在Eclipse中安装：Help-Install new SoftWare.. - Add

- **方式二：下载sts工具**

 直接下载sts工具（相当于一个集合了Spring tool suite的Eclipse）: https://spring.io/tools/sts/



## Spring IOC



## Spring AOP

### 什么是 AOP

> ​		AOP（Aspect-Oriented Programming，面向方面编程），通过预编译的方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

### AOP 的目的

​		AOP能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

### AOP 术语

- **横切关注点**：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点，如日志，安全，缓存，事务...
- **切面**:切面是通知和切点的集合，通知和切点共同定义了切面的全部功能， 就是一个类Log

- **通知（Advice）**：切面必须要完成的工作，它是类中的一个方法     Log 中的一个方法
- **目标（Target）**：被通知的对象
- **代理（Proxy）**：向目标对象应用通知之后创建的对象
- **切入点（ASPECT）**:切面通知执行的 "何处"的定义
- **连接点（JointPoint）**：连接点是一个应用执行过程中能够插入一个切面的点。
- **引入**:引入允许我们向现有的类中添加方法或属性
- **织入**:织入是将切面应用到目标对象来创建的代理对象过程。

### 在 XML 中声明切面

1. Spring 的 AOP 配置元素简化了基于 POJO 切面声明

| AOP 配置元素              | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| <aop : advisor>           | 定义 AOP 通知器                                              |
| <aop : after>             | 定义 AOP 后置通知（不管被通知方法是否执行成功）              |
| <aop : after-returing>    | 定义 AOP after-returing 通知                                 |
| <aop : after-throwing>    | 定义 AOP after-throwing 通知                                 |
| <aop : around>            | 定义 AOP 环绕通知                                            |
| <aop : aspect>            | 定义切面                                                     |
| <aop : aspectj-autoproxy> | 启动 @AspectJ 注解驱动的切面                                 |
| <aop : before>            | 定义 AOP 前置通知                                            |
| <aop : config>            | 顶层的 AOP 配置元素，大多数 <aop : *> 元素必须包含在 <aop : config>元素内 |
| <aop : declare-parents>   | 为被通知的对象引入额外接口，并透明的实现                     |
| <aop : pointcut>          | 定义切点                                                     |