# Centos7安装Tomcat7，并上传JavaWeb项目

## 一、工具

1. Xshell
2. XFTP

1.1、首先将Tomcat7的压缩文件利用XFTP上传到Centos7系统上的 /etc/local/tomcat中

![img](http://q936eat5v.bkt.clouddn.com/images/75700578.png)

1.2 、解压文件，并删除压缩文件（上图是解压好之后的文件！）

```bash
tar zxvf apache-tomcat-7.0.96.tar.gz
rm -f apache-tomcat-7.0.96.tar.gz
```

## 二、修改配置文件

 2.1、 进入tomcat的bin目录后通过vi命令打开catalina.sh文件，并在其中加入如下配置： 

复制下面的命令，加入到catalina.sh文件中，将jdk和tomcat修改为自己的版本

```bash
export TOMCAT_HOWE=/usr/local/tomcat/apache-tomcat-7.0.96
export CATAL INA_HOME=/usr/local/tomcat/apache-tomcat-7.0.96
export JRE_HOME=/usr/local/jdk/jdk1.8.0_221/jre
export JAVA_HONE=/usr/local/jdk/jdk1.8.0_221
```

![image-20200509105922108](http://q936eat5v.bkt.clouddn.com/images/image-20200509105922108.png)

![image-20200509105930712](http://q936eat5v.bkt.clouddn.com/images/image-20200509105930712.png)

2.2、修改一下tomcat端口号

​		进入tomcat的conf目录下，修改server.xml文件，通过vi命令打开文件后直接输入/8080检索到端口号的位置，进入编辑模式后修改端口号为8081（其他端口也行，这是我修改的8081），如下图所示

![image-20200509110002189](http://q936eat5v.bkt.clouddn.com/images/image-20200509110002189.png)

2.3、 启动tomcat

完成之后就可以尝试启动一下tomcat，在tomcat 的bin目录下输入启动命令：

```bash
[root@localhost bin]# ./startup.sh 
```

下面启动成功!!

```bash
[root@localhost bin]# ./startup.sh 
Using CATALINA_BASE:   /usr/local/tomcat/apache-tomcat-7.0.96
Using CATALINA_HOME:   /usr/local/tomcat/apache-tomcat-7.0.96
Using CATALINA_TMPDIR: /usr/local/tomcat/apache-tomcat-7.0.96/temp
Using JRE_HOME:        /usr/local/jdk/jdk1.8.0_221/jre
Using CLASSPATH:       /usr/local/tomcat/apache-tomcat-7.0.96/bin/bootstrap.jar:/usr/local/tomcat/apache-tomcat-7.0.96/bin/tomcat-juli.jar
Tomcat started.
```

## 三、设置防火墙

> CentOS 7中引入了一个更强大的防火墙——Firewall。我们需要在Firewall中开启8081端口

  3.1、 将我们修改的8081端口加入到zone（Firewall的新特性，简单讲它的作用就是定义了网络区域网络连接的可信等级）中。命令如下：

```bash
firewall-cmd --zone=public --add-port=8081/tcp --permanent
```

这样就成功的将8081端口加入了public区域中，permanent参数表示永久生效，即重启也不会失效.

### 删除一个端口

```bash
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
```

如果出现:

![image-20200509110322589](http://q936eat5v.bkt.clouddn.com/images/image-20200509110322589.png)

说明防火墙没有开启，则我们需要开启防火墙

```bash
systemctl start firewalld
```

 查看防火墙是否开启：

```bash
systemctl status firewalld
```

![image-20200509110359624](http://q936eat5v.bkt.clouddn.com/images/image-20200509110359624.png)

出现running 则证明已经开启!!



### 更新防火墙

```bash
firewall-cmd  --reload
```

查看public区域下打开的端口，命令如下：

```bash
firewall-cmd --zone=public --list-ports
```

看到8081/tcp 端口已经成功打开

![image-20200509110539127](http://q936eat5v.bkt.clouddn.com/images/image-20200509110539127.png)

## 四、生成War文件

### 4.1、打War包

  打开Eclipse，选择自己的web项目右键选择**Export**

  然后打开下面的Web中的WAR file

![image-20200509110645930](http://q936eat5v.bkt.clouddn.com/images/image-20200509110645930.png)

双击打开，选择自己保存的路径，点击Finish，war文件就生成！

![image-20200509110704446](http://q936eat5v.bkt.clouddn.com/images/image-20200509110704446.png)

打包好的文件

![image-20200509110721450](http://q936eat5v.bkt.clouddn.com/images/image-20200509110721450.png)



## 五、上传War到linux

使用XFTP连接到自己的虚拟机

![image-20200509110828813](http://q936eat5v.bkt.clouddn.com/images/image-20200509110828813.png)

将自己的War文件上传到tomcat下的webapps中

![image-20200509110840686](http://q936eat5v.bkt.clouddn.com/images/image-20200509110840686.png)

试着在自己电脑访问一下：

![image-20200509110857084](http://q936eat5v.bkt.clouddn.com/images/image-20200509110857084.png)

可以访问到，这样基本的配置就完成了！！