# 1. MQ介绍

## 1.1 为什么要使用MQ

消息队列是一种“先进先出”的数据结构

![image-20210102140239152](http://club.codehuan.cn/images/image-20210102140239152.png)

其应用场景主要包括以下三个方面

- 应用解耦

系统的耦合性越高，容错性就越低。以电商应用为例，用户创建订单后，如果耦合调用库存系统、物流系统、支付系统，任何一个子系统出了故障或者因为升级等原因暂时不可用，都会造成下单操作异常，影响用户使用体验。

<img src="http://club.codehuan.cn/images/image-20210102140621414.png" alt="image-20210102140621414" style="zoom: 80%;" />

使用消息队列解耦合，系统的耦合性就会提高了。比如物流系统发生故障，需要几分钟才能来修复，在这段时间内，物流系统要处理的数据被缓存到消息队列中，用户的下单操作正常完成。当物流系统回复后，补充处理存在消息队列中的订单消息即可，终端系统感知不到物流系统发生过几分钟故障。

<img src="http://club.codehuan.cn/images/image-20210102140744602.png" alt="image-20210102140744602" style="zoom:80%;" />

- 流量削峰

<img src="http://club.codehuan.cn/images/image-20210102140847310.png" alt="image-20210102140847310" style="zoom:80%;" />

应用系统如果遇到系统请求流量的瞬间猛增，有可能会将系统压垮。有了消息队列可以将大量请求缓存起来
分散到很长一段时间处理，这样可以大大提到系统的稳定性和用户体验。

<img src="http://club.codehuan.cn/images/image-20210102141002009.png" alt="image-20210102141002009" style="zoom:80%;" />

一般情况，为了保证系统的稳定性，如果系统负载超过阈值，就会阻止用户请求，这会影响用户体验，而如果使用消息队列将请求缓存起来，等待系统处理完毕后通知用户下单完毕，这样总不能下单体验要好。
处于经济考量目的:
业务系统正常时段的QPS如果是1000，流量最高峰是10000，为了应对流量高峰配置高性能的服务器显然不划算，这时可以使用消息队列对峰值流量削峰

- 数据分发

<img src="http://club.codehuan.cn/images/image-20210102141256432.png" alt="image-20210102141256432" style="zoom:80%;" />

通过消息队列可以让数据在多个系统更加之间进行流通。数据的产生方不需要关心谁来使用数据，只需要将数据发送到消息队列，数据使用方直接在消息队列中直接获取数据即可

<img src="http://club.codehuan.cn/images/image-20210102141438602.png" alt="image-20210102141438602" style="zoom:80%;" />

## 1.2 MQ的优点和缺点

优点：解耦、削峰、数据分发

缺点：

- 系统可用性降低

系统引入的外部依赖越多，系统稳定性越差。一旦MQ宕机，就会对业务造成影响。

如何保护MQ的高可用？

- 系统复杂度提高

MQ的加入大大增加了系统的复杂度，以前系统间是同步的远程调用，现在是通过MQ进行异步调用。如何保证消息没有被重复消费?怎么处理消息丢失情况?那么保证消息传递的顺序性?

- 一致性问题

A系统处理完业务，通过MQ给B、C、D三个系统发消息数据，如果B系统、C系统处理成功，D系统处理失败。

如何保证消息数据处理的一致性?

## 1.3 各种MQ产品比较

常见的MQ产品包括Kafka、ActiveMQ、RabbitMQ、RocketMQ。

| 特性           |                           ActiveMQ                           |                           RabbitMQ                           |         RocketMQ         |                            kafka                             |
| :------------- | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------: | :----------------------------------------------------------: |
| **开发语言**   |                             java                             |                            erlang                            |           java           |                            scala                             |
| **单机吞吐量** |                             万级                             |                             万级                             |          10万级          |                            10万级                            |
| **时效性**     |                             ms级                             |                             us级                             |           ms级           |                           ms级以内                           |
| **可用性**     |                        高（主从架构）                        |                        高（主从架构）                        |   非常高（分布式架构）   |                     非常高（分布式架构）                     |
| **功能特性**   | 成熟的产品，在很多公司得到应用;有较多的文档;各种协议支持较好 | 基于erlang开发，所以并发能力很强，性能极其好，延时很低，管理界面较丰富 | MQ功能比较完备，扩展性佳 | 只支持主要的MQ功能，像一些消息查询，消息回溯等功能没有提供，毕竟是为大数据准备的，在大数据领域应用广。 |

# 2. RocketMQ快速入门

RocketMQ是阿里巴巴2016年MQ中间件，使用lava语言开发，在阿里内部，RocketMQ承接了例如双11等高并发场景的消息流转，能够处理万亿级别的消息。

## 2.1 准备工作

### 2.1.1 下载RocketMQ

RocketMQ 最新版本 4.8.0

[下载地址]:http://rocketmq.apache.org/dowloading/releases/

### 2.1.2 环境要求

- Linux64位
- JDK1.8(64位)
- 源码安装需要安装Maven3.2.X

## 2.2 安装RocketMQ

### 2.2.1 安装步骤

1. 将二进制文件放到 `/usr/soft` 目录下

2. 解压文件

3. 将解压文件复制到 `/usr/local/rocketmq` 目录下

   ```shell
   mkdir rocketmq
   mv /usr/soft/rocketmq-all-4.8.0-bin-release /usr/local/rocketmq/
   ```

### 2.2.2 目录介绍

- bin：启动脚本，包括shell脚本和CMD脚本
- conf：实例配置文件，包括broker配置文件
- lib：依赖jar包，包括Netty、commons-lang、fastjson等

## 2.3 启动RocketMQ

1. 启动 `NameServer`

```shell
# 启动 NameServer
nohup sh bin/mqnamesrv &
# 查看启动日志
tail -f ~/logs/rocketmqlogs/namesrv.log
```

1. 启动`Broker`

```shell
# 启动 Broker
nohup sh bin/mqbroker -n localhost:9876 &
# 查看启动日志
tail -f ~/logs/rocketmqlogs/broker.log
```

- 问题描述

  ​	RocketMQ默认的虚拟机内存较大，启动Broker如果因为内存不足失败，需要编辑一下两个文件

  ```shell
  # vi runbroker.sh and runserver.sh,To edit jvm-size
  vi runbroker.sh
  vi runserver.sh
  ```

  ## 2.4 测试RocketMQ

  ### 2.4.1 发送消息

  ```shell
  # 设置环境变量
  export NAMESRV_ADDR=localhost:9876
  # 使用安装包的Demo发送消息
  sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
  ```

  ### 2.4.2 接收消息

  ```shell
  # 设置环境变量
  export NAMESRV_ADDR=localhost:9876
  # 接收消息
  sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
  ```

  ## 2.5 关闭RocketMQ

```shell
# 1.关闭NameServer
sh bin/mqshutdown namesrv
# 2.关闭Broker
sh bin/mqshutdown broker
```

# 3. RocketMQ 集群搭建

## 3.1 各角色介绍

- Producter：消息的发送者；举例：发信者
- Consumer：消息接收者；举例：收信者
- Broker：暂存和传输消息；举例：邮局
- NameServer：管理Broker；各个邮局的管理机构
- Topic：区分消息的种类；一个发送者可以发送消息给一个或多个Topic；
- Message Queue：相当于是Topic的分区；用于并行发送和接收消息

![image-20210116141447304](http://image.codehuan.cn/image/image-20210116141447304.png)

## 3.2 集群搭建方式

### 3.2.1 集群特点

### 3.2.2 集群模式

## 3.3 双主双从集群搭建

### 3.3.1 总体架构

### 3.3.2 集群工作流程

### 3.3.3 服务器环境搭建

### 3.3.4 Host添加信息

### 3.3.5 防火墙配置

### 3.3.6 环境变量配置

### 3.3.7 创建消息存储路径

### 3.3.8 broker配置文件

### 3.3.9 修改启动脚本文件

### 3.3.10 服务启动

### 3.3.11 查看进程状态

### 3.3.12 查看日志

# 4. 消息发送样例

## 4.1

