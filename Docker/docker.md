# Docker 简介

>​		Docker是一款针对程序开发人员和系统管理员来开发、部署、运行应用的一款虚拟化平台。Docker 可以让你像使用集装箱一样快速的组合成应用，并且可以像运输标准集装箱一样，尽可能的屏蔽代码层面的差异。Docker 会尽可能的缩短从代码测试到产品部署的时间。 **容器虚拟化技术**

Docker 组件

- The Docker Engine – Docker Engine 是一个基于虚拟化技术的轻量级并且功能强大的开源容器引擎管理工具。它可以将不同的 work flow 组合起来构建成你的应用。
- Docker Hub 可以分享和管理你的images镜像的一个 Saas 服务。

## 运用场景：

- web应用的自动化打包和发布；
- 自动化测试和持续集成、发布；
- 在服务型环境中部署和调整数据库或其他的后台应用；
- 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

## 基本组成：

- **镜像（image）**
- **容器（container）**
- **仓库（repository）**

# CentOS-7 安装、卸载

### 1、查看系统要求

Docker 要求 CentOS 系统的内核版本高于 3.10 ,查看CentOS的内核版本。

`uname -a` 或者` uname -r`

```bash
[root@codehuan ~]#  uname -r  
3.10.0-1062.18.1.el7.x86_64
```

### 2、删除旧版本

```bash
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 3、设置Docker yum源

​		这里设置为阿里的。

```bash
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 4、配置仓库

​	创建或修改 /etc/docker/daemon.json 文件，修改为如下形式: 

```bash
vi /etc/docker/daemon.json
{
    "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

​	仓库地址使用阿里云的，登录阿里云 找到容器镜像服务

![image-20200509213246570](http://club.codehuan.cn/images/image-20200509213246570.png)

需要开通（免费），开通之后找到容器加速器，复制 箭头标记的

{  "registry-mirrors": ["https://xxxxxx.mirror.aliyuncs.com"] } 

![image-20200509213408788](http://club.codehuan.cn/images/image-20200509213408788.png)

### 5、启动

```bash
systemctl start docker	#启动

systemctl restart docker  #重启

systemctl status docker  #查看启动状态

systemctl enable docker	#设置为开机启动

docker version	#查看版本
```

### 6、安装完成

![image-20200509213636897](http://club.codehuan.cn/images/image-20200509213636897.png)

### 7、卸载

**1、卸载Docker引擎、CLI和Containerd包：**

```shell
$ sudo yum remove docker-ce docker-ce-cli containerd.io
```

**2、删除Images（镜像）、containers（容器）、volumes（卷）**

```shell
$ sudo rm -rf /var/lib/docker
```

# Docker常用命令

## 帮助命令

**1、查看版本信息 `docker version`**

```shell
docker version 	
```

**2、显示docker的系统信息，镜像和容器数量`docker info `**

```shell
docker info
```

**3、帮助命令**

```shell
docker 命令 --help	
```

文档：https://docs.docker.com/reference/

## 镜像命令

**1、列出本地主机上的镜像`docker images`** 

```shell
[root@codehuan ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
[root@codehuan ~]#

#属性说明
	REPOSITORY : 镜像的仓库源 	
	TAG : 镜像的标签	
	IMAGE ID ：镜像ID			
	CREATED ：镜像创建时间
	SIZE ：镜像大小
	
# 参数说明
-a：列出所有镜像(含中间镜像层) 
-q： 只显示镜像ID 
--digests：显示镜像的摘要信息
--no-trunc：显示完整的镜像信息
```

**2、查找镜像`docker search [OPTIONS]	#镜像名`** 

```shell
[root@codehuan ~]# docker search tomcat	# 查找Tomcat镜像

#参数说明
-s：列出stars数不小于指定数的
--no-trunc：显示完整的镜像信息
--automated:只列出automated build（自动构建）类型的镜像
```

**3、下载镜像`docker pull [OPTIONS]	#镜像名`**

```shell
[root@codehuan ~]# docker pull tomcat:latest

#冒号后面写的是镜像的版本号，如果不写，默认是latest最新版本
```



**4、删除镜像`docker rmi [OPTIONS]	#镜像名或ID`**

```shell
# 删除单个
docker rmi -f 镜像ID

##删除多个
docker rmi -f 镜像名1：TAG 镜像名2:TAG

###删除全部
docker rmi -f $(docker images -qa)
```

**5、为容器创建新的镜像 `docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`**

```shell
docker commit -a "codehuan" -m "my tomcat" d03312117bb0 tomcat:v1

OPTIONS说明：

-a :提交的镜像作者；

-c :使用Dockerfile指令来创建镜像；

-m :提交时的说明文字；

-p :在commit时，将容器暂停。
```

## 容器命令

**1、有镜像才能创建容器，启动一个centos镜像进行演示**

```shell
docker run -it centos

# OPTIONS说明
-name="容器新名字"：为容器指定一个名称
-d：后台运行容器，并返回容器ID，也即启动守护式容器；
-i：以交互模式运行容器，通常与-t同时使用；
-t：为容器重新分配一个伪输入终端，通常与-i同时使用
-P：随机端口映射
-p：指定端口映射，有以下四种格式
    ip：hostPort:containerPort
    ip::containerPort
    hostPort：containerPort
    containerPort
-m :设置容器使用内存最大值；

```

**2、列出当前所有正在运行的容器`docker ps [OPTIONS]`**

```shell
docker ps


#参数说明

-a :显示所有的容器，包括未运行的。

-f :根据条件过滤显示的内容。

--format :指定返回值的模板文件。

-l :显示最近创建的容器。

-n :列出最近创建的n个容器。

--no-trunc :不截断输出。

-q :静默模式，只显示容器编号。

-s :显示总的文件大小。
```

**3、退出容器**

```shell
exit # 停止退出
Ctrl+P+Q #不停止退出
```

**4、启动容器**

```shell
docker start 容器ID或者容器名
```

**5、重启容器**

```shell
docker restart 容器ID或者容器名
```

**6、停止容器**

```shell
docker stop 容器ID或者容器名
```

**7、强制停止容器**

```shell
docker kill 容器ID或者容器名
```

**8、删除已停止容器**

```shell
docker rm  容器ID或者容器名

#参数
-f：强制删除
```

**9、一次性删除多个容器**

```shell
docker rm -f $(docker ps -a -q)
docker ps -a -q |xargs docker rm
```

## 守护式容器

**1、启动守护式容器**

```shell
docker run -d centos #容器名
```

**2、查看容器日志**

```shell
docker logs -f -t --tail 容器ID

##参数说明
-t：是加入时间戳
-f：跟随最新的日志打印
--tail：数字 显示最后多少条
```

**3、查看容器内运行的进程**

```shell
docker top 容器ID
```

**4、查看容器内部细节**

```shell
docker inspect 容器ID
```

**5、进入正在运行的容器并以命令行交互**

```shell
docker exec it 容器id /bin/bash
docker exec -t 容器id ls-l /tmp

docker attach 容器ID

# exec 和 attach 的区别
attach：直接进入容器启动命令的终端，不会启动新的进程
exec：是在容器中打开新的终端，并且可以启动新的进程
```

**6、从容器内拷贝文件到主机上**

```shell
docker cp 容器ID：容器内路径 目的主机路径
docker cp 1c1810c6b353:/etc/yum.conf /root
```

# Docker 镜像

# 容器数据卷

## 1、数据卷

数据卷是一个可供一个或多个容器使用的特殊目录，它绕过 UFS，可以提供很多有用的特性：

- 数据卷可以在容器之间共享和重用
- 对数据卷的修改会立马生效
- 对数据卷的更新，不会影响镜像
- 卷会一直存在，直到没有容器使用

*数据卷的使用，类似于 Linux 下对目录或文件进行 mount。

### 1.1 创建一个数据卷

​		在用 `docker run` 命令的时候，使用 `-v` 标记来创建一个数据卷并挂载到容器里。在一次 run 中多次使用可以挂载多个数据卷。

下面创建一个 web 容器，并加载一个数据卷到容器的 `/webapp` 目录。

```shell
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```

*注意：也可以在 Dockerfile 中使用 `VOLUME` 来添加一个或者多个新的卷到由该镜像创建的任意容器。

### 1.2 挂载一个主机目录作为数据卷

使用 `-v` 标记也可以指定挂载一个本地主机的目录到容器中去。

```bash
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```















