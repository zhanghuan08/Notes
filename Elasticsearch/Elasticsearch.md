# ElasticSearch 简介

​		Elasticsearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。Elasticsearch用于[云计算](https://baike.baidu.com/item/云计算/9969353)中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。官方客户端在Java、.NET（C#）、PHP、Python、Apache Groovy、Ruby和许多其他语言中都是可用的。根据DB-Engines的排名显示，Elasticsearch是最受欢迎的企业搜索引擎，其次是Apache Solr，也是基于Lucene。

# ElasticSearch 安装

官网：https://www.elastic.co/cn/

>Windows 下 安装

1、将下载的安装包 解压

![image-20200524154626185](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524154626185.png)

2、目录结构

```
bin 启动文件
config 配置文件
	log4j2 日志配置文件
	jvm.options 虚拟机相关配置
	elasticsearch.yml  elasticsearch的配置文件 默认端口：9200！ 跨域
lib 	相关的jar
logs 	日志
modules 功能模块
plugins 插件
```

3、启动Elasticsearch 进入到bin 目录下双击 elasticsearch.bat 启动，

<img src="C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524155459857.png" alt="image-20200524155459857"/>

4、访问测试，在浏览器访问 ：http://localhost:9200/

![image-20200524155337159](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524155337159.png)

>安装可视化界面 es head的插件

官网：https://github.com/mobz/elasticsearch-head/

解压文件

![image-20200524160009297](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524160009297.png)

进入 该文件夹进入cmd : 执行 cnpm/npm install 安装依赖

启动：npm run start

![image-20200524160221047](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524160221047.png)

浏览器测试：http://localhost:9100/

![image-20200524160312345](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524160312345.png)



连接测试时 会出现跨域问题

![image-20200524160552031](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524160552031.png)

1、出现跨域问题，在`elasticsearch.yml`需要配置如下：

```yaml
http.cors.enabled: true
http.cors.allow-origin: "*"
```



>kibana安装

官网：https://www.elastic.co/cn/kibana

![image-20200524161723002](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\image-20200524161723002.png)

kibana版本要和es一致

