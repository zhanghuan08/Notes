# Hystrix （断路器）

>Hystrix简介

​		Hystrix是一 个用于**处理分布式系统的延迟和容错**的开源库,在分布式系统里,许多依赖不可避免的会调用失败,比如超时、异常等,Hystrix能够保证在一个依赖出问题的情况下，**不会导致整体服务失败,避免级联故障,以提高分布式**系统的弹性。

​		“断路器”本身是一种开关装置， 当某个服务单元发生故障之后,通过断路器的故障监控(类似熔断保险丝)，向调用方返回一个符合预期的、可处理的备选响应(FallBack) ，而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用,从而避免了故障在分布式系统中的蔓延，乃至雪崩。

>服务降级

1、什么是服务降级？

​		当consumer 访问 服务器时，服务器出现一些问题，给出一些友好的提示fallback，例如：服务器忙，请稍后再试！

2、什么情况会出现服务降级？

	-	程序运行异常
	-	超时
	-	服务熔断触发服务降级
	-	线程池/信号量打满也会导致服务降级

>服务熔断

1、什么是服务熔断？

​		类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示，就是保险丝！

>服务限流

秒杀高并发等操作，严禁一窝蜂的过来拥挤, 大家排队，一秒钟N个，有序进行