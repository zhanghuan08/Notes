# 6SpringCloud

![image-20201112004159764](http://club.codehuan.cn/images/image-20201112004159764.png)

## 服务注册与发现

### 1、Eureka

### 2、Zookeeper

### 3、Consul

#### 3.1 windows 10 启动命令 :

 consul agent -dev

### 4、三个注册中心异同点

|  组件名   | 语言 | CAP  | 服务健康检查 | 对外暴露接口 | Spring  Cloud集成 |
| :-------: | :--: | :--: | :----------: | :----------: | :---------------: |
|  Eureka   | Java |  AP  |   可配支持   |     HTTP     |      已集成       |
|  Consul   |  Go  |  CP  |     支持     |   HTTP/DNS   |      已集成       |
| Zookeeper | Java |  CP  |     支持     |    客户端    |      已集成       |

#### CAP

**C:Consistency(强一致性)**

**A:Avalibility （可用性）**

**P:Partition tolerance （分区容错性）**

**最多只能同时较好的满足两个**

**CAP理论的核心是:一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，**

因此，根据CAP原理将NoSQL数据库分成了满足CA原则、满足CP原则和满足AP原则三大类:

CAP理论关注粒度是数据，而不是整体系统设计的策略

**CA-单点集群，满足—致性，可用性的系统，通常在可扩展性上不太强大。**
**CP-满足─致性，分区容忍必的系统，通常性能不是特别高。**
**AP-满足可用性，分区容忍性的系统，通常可能对—致性要求低一些。**

![image-20201119085843081](http://club.codehuan.cn/images/image-20201119085843081.png)

## 服务调用

### Ribbon负载均衡

**Ribbon是什么？**

Ribbon其实就是一个软负载均衡的客户端组件，
他可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例。

**1、LB负载均衡(Load Balance)是什么**
简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA(高可用)。

一句话就是：负载均衡+RestTemplate调用

常见的负载均衡有软件Nginx，LVS，硬件F5等。

**2、Ribbon本地负载均衡客户端VS Nginx服务端负载均衡区别**
Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。
Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到VM本地，从而在本地实现RPC远程服务调用技术。

**集中式LB**
即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5,也可以是软件，如nginx),由该设施负责把访问请求通过某种策!略转发至服务的提供方;

**进程内LB**
将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。
**Ribbon就属于进程内LB**，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。

### Ribbon 架构

![image-20201119093141216](http://club.codehuan.cn/images/image-20201119093141216.png)

**Ribbon在工作时分成两步**
第—步先选择EurekaServer ,它优先选择在同一个区域内负载较少的server.

第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。

其中Ribbon提供了多种策略:比如轮询、随机和根据响应时间加权。

### Ribbon核心组件IRule

 IRule：根据特定算法中从服务列表中选取一个要访问的服务

负载均衡规则7种：

com.netflix.loadbalancer.RoundRobinRuleo 轮询

com.netflix.loadbalancer.RandomRule 随机

com.netflix.loadbalancer.RetryRule

WeightedResponseTimeRule

BestAvailableRule

AvailabilityFilteringRule

ZoneAvoidanceRule

### OpenFeign

**Feign是什么**

​		Feign是一个声明式的Web服务客户端，让编写Web服务客户端变得非常容易，**只需创建一个接口并在接口上添加注解**即可

**Feign能干什么**

旨在使编写Java Http客户端变得更容易。
前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，**往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用**。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，**我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)**，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。

**Feign集成了Ribbon**

利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，**通过feign只需要定义服务绑定接口且以声明式的方法**，优雅而简单的实现了服务调用

### Feign与OpenFeign区别

| Feign                                                        | OpenFeign                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Feign是Spring Cloud组件中的一个轻量级RESTful的HTTP服务客户端Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务。Feign的使用方式是:使用Feign的注解定义接口，调用这个接口，就可以调用服务注册中心的服务 | OpenFeign是Spring Cloud在Feign的基础上支持了SpringMVC的注解，如@RequesMapping等等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。 |
| <dependency><br/><groupId>org.springframework.cloud</groupId><br/><artifactId>spring-cloud-starter-feign</artifactId></dependency> | <dependency><br/><artifactId>spring-cloud-starter-openfeign</artifactId></ dependency> |



### OpenFeign超时控制

​		默认Feign客户端只等待一秒钟，但是服务端处理需要超过1秒钟，导致Feign客户端不想等待了，直接返回报错。为了避免这样的情况，有时候我们需要设置Feign客户端的超时控制。

yml文件中开启

```yml
server:
  port: 80

eureka:
  client:
    # 表示是否将自己注册进 EurekaServer 默认为 true
    register-with-eureka: false
    service-url:
#      defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
# 设置feign客户端超时时间(OpenFeign 默认支持ribbon)
ribbon:
# 指的是建立连按所用的时问，适用于网络状况止常的情况下,两端连接所用的时间
  ReadTimeout: 5000
# 指的是建立连接后从服务器读取到可用资源所用的时间
  connectTimeout: 5000
```

### OpenFeign日志打印功能

#### 是什么

​		Feign提供了日志打印功能，我们可以通过配罩来调整日志级别，从而了解Feign中 Http请求的细节。说白了就是**对Feign接口的调用情况进行监控和输出**。

#### 日志级别

**NONE**:默认的，不显示任何日志;

**BAsIC**︰仅记录请求方法、URL、响应状态码及执行时间;

**HEADERS**:除了BASIC中定义的信息之外，还有请求和响应的头信息;

**FULL**:除了HEADERS中定义的信息之外，还有请求和响应的正文及元数据。

#### 日志打印功能使用

在非启动类目录下创建config包，创建`FeignConfig`类

```java
@Configuration
public class FeignConfig
{
    @Bean
    Logger.Level feignLoggerLevel()
    {
        return Logger.Level.FULL;
    }
}
```

在`yml`文件开启日志

```yml
logging:
  level:
    # feign日志以什么级别监控哪个接口
    com.zh.springcloud.service.PaymenFeigntService: debug
```

截图

![image-20201121161131662](http://club.codehuan.cn/images/image-20201121161131662.png)

## 服务熔断降级

### Hystrix 断路器

#### Hystrix是什么

​		Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，**不会导致整体服务失败，Ⅰ避免级联故障，以提高分布式系统的弹性**。
​		"断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝)，**向调用方返回一个符合预期的、可处理的备选响应（FallBack)，而不是长时间的等待或者抛出调用方无法处理的异常**，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

##### Hystrix重要概念

##### 1、服务降级 fallback

**1.1 是什么**

服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback

服务降级可以放到客户端也可以放到服务端，一般放在客户端

**1.2 哪些情况会发出降级**

- 程序运行异常
- 超时
- 服务熔断触发服务降级
- 线程池/信号量打满也会导致服务降级	

###### 1.2.1降级配置

**1、先从提供者自身找问题**

设置自身调用超时时间的峰值，峰值内可以正常运行,超过了需要有兜底的方法处理，作服务降级fallback

**业务类启用** @HystrixCommand

主启动类添加@EnableCircuitBreaker

**业务类**

```java
@Override
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",commandProperties = {
        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public String paymentInfo_TimeOut(Integer id)
    {
        // 模拟复杂的业务，处理时间长
        int timeNumber = 5;
        //int age = 10/0;
        try
        {
            TimeUnit.SECONDS.sleep(timeNumber);
        }
        catch (InterruptedException e)
        {
            e.printStackTrace();
        }
        return "线程池：" + Thread.currentThread().getName() + "payment_TimeOut,id:" + id + "\t" + "O(∩_∩)O哈哈~" + "耗时" + timeNumber + "秒钟";
    }

    public String paymentInfo_TimeOutHandler(Integer id)
    {
        return "线程池：" + Thread.currentThread().getName() + "paymentInfo_TimeOutHandler,id:" + id + "\t" + "┭┮﹏┭┮";
    }
```

超时和运行时异常 都会进入到`paymentInfo_TimeOutHandler`处理方案

当前服务不可用，做服务降级，兜底方案都是`paymentInfo_TimeOutHandler`

**2、消费者自己进行处理** 80fallback

>编写代码注意：
>
>我们自己配置过的热部署方式对java代码的改动明显，但对@HystrixCommand内属性的修改建议重启微服务

2.1 Yml

```yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
feign:
  hystrix:
    enabled: true
```

2.2 主启动类

添加 `@EnableHystrix` 注解

2.3 controller 处理

```java
@GetMapping("/consumer/payment/hystrix/timeout/{id}")
@HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",commandProperties = {
    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1500")
})
public String paymentInfo_TimeOut(Integer id)
{
    String result = paymentHystrixService.paymentInfo_TimeOut(id);
    return result;
}

public String paymentInfo_TimeOutHandler(Integer id)
{
    return "我是消费者80，对方支付系统繁忙，请稍后再试o(╥﹏╥)o";
}
```

###### 目前问题

>每个业务方法对应一个兜底方法，代码膨胀
>
>统一和自定义的分开

###### 解决问题

>1、每个业务方法对应一个兜底方法，代码膨胀

feign接口系列

类上添加`@DefaultProperties(defaultFallback = "payment_Global_FallBackMethod")`注解

方法添加`@HystrixCommand`

![image-20201121215926606](http://club.codehuan.cn/images/image-20201121215926606.png)

![image-20201121220836475](http://club.codehuan.cn/images/image-20201121220836475.png)

> 2、统一和自定义的分开 耦合度高 业务方法与fallback方法混在一起

案例：服务降级，客户端去调用服务端，碰上服务端宕机或关闭

​		本次案例服务降级处理是在客户端80实现完成的，与服务端8001没有关系只需要为Feign客户端定义的接口添加一个服务降级处理的实现类即可实现解耦

未来会遇到异常

- 运行
- 超时
- 宕机

解耦方法：创建一个类实现该service接口

`PaymentHystrixService`

```java
@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)
public interface PaymentHystrixService
{
    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id);
}
```

`PaymentFallbackService`实现`PaymentHystrixService`接口进行处理异常，服务降级

```java
@Component
public class PaymentFallbackService implements PaymentHystrixService
{
    @Override
    public String paymentInfo_OK(Integer id)
    {
        return "-----PaymentFallbackService fall back-paymentInfo_OK ,o(╥﹏╥)o";
    }

    @Override
    public String paymentInfo_TimeOut(Integer id)
    {
        return "-----PaymentFallbackService fall back-paymentInfo_TimeOut ,o(╥﹏╥)o";
    }
}
```



##### 2、服务熔断 break

**2.1 是什么**

​		类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示，就是保险丝。**服务的降级 -->  进而熔断 --> 恢复调用链路** 

**2.2 熔断机制概述**
		熔断机制是应对雪崩效应的一种微服务链路保护机制。当扇出链路的某个微服务出错不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的响应信息。**当检测到该节点微服务调用响应正常后，恢复调用链路**。

​		在Spring Cloud框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会启动熔断机制。熔断机制的注解是@HystrixCommand。

##### 3、服务限流 flowlimit

**3.1 是什么**

​		秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行

#### Hystrix使用



## 服务网关

## 服务分布式配置

## 服务开发