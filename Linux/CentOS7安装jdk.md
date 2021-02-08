# 云服务器CentOS 7 安装jdk8

## 1. 下载

### 1.1 下载安装包

在官网下载，选择合适的版本进行下载

[官网]:https://www.oracle.com/cn/java/

<img src="http://image.codehuan.cn/image/image-20210117174421879.png" alt="image-20210117174421879" style="zoom: 70%;" />

[华为云]:https://repo.huaweicloud.com/java/jdk/

![image-20210117173000055](http://image.codehuan.cn/image/image-20210117173000055.png)

[搜搜]:http://java.sousou88.com/spec/version8.html#jdk8u221

![image-20210117173309714](http://image.codehuan.cn/image/image-20210117173309714.png)

选择自己需要的版本进行下载，我这里使用的是  `jdk-8u221-linux-x64.tar.gz`

### 1.2 上传到云服务器

使用连接工具上传，我使用的是 `MobaXterm`工具

[MobaXterm]:https://mobaxterm.mobatek.net/

将安装包放在 `/usr/soft/`目录下面

![image-20210117174549390](http://image.codehuan.cn/image/image-20210117174549390.png)

## 2. 安装

### 2.1 解压

进入到 `/usr/soft`目录下面，将`jdk-8u221-linux-x64.tar.gz`安装包解压

```shell
# 进入目录
cd /usr/soft/
# 解压
tar -zxvf jdk-8u221-linux-x64.tar.gz
```

![image-20210117175002754](http://image.codehuan.cn/image/image-20210117175002754.png)

### 2.2 将解压后的文件移动到 `/usr/local/java/` 目录下面

```shell
# 移动  在soft目录下
mv jdk1.8.0_221/ /usr/local/java/
```

## 3. 配置环境变量

### 3.1 添加环境变量

编辑 `/etc/profile`文件

```shell
# 编辑
vi /etc/profile
```

在文件最下面添加：

```shell
#set java environment
JAVA_HOME=/usr/local/java/jdk1.8.0_221/
JRE_HOME=/usr/local/java/jdk1.8.0_221/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
```

![image-20210117175646251](http://image.codehuan.cn/image/image-20210117175646251.png)

### 3.2 让修改后的文件生效

```shell
source /etc/profile
```

### 3.3 查看java 环境

命令行输入：

```shell
javac
or   
java -version
```

![image-20210117180036576](http://image.codehuan.cn/image/image-20210117180036576.png)

显示 如上图所示，则安装完成！

## 4. 疑问联系

有疑问联系（`QQ、微信`同）：

QQ：1730272469

<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=1730272469&site=qq&menu=yes"><img border="0" src="http://wpa.qq.com/pa?p=2:1730272469:53" alt="点击这里给我发消息" title="点击这里给我发消息"/></a>