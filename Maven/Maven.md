# Maven下载与安装

## 一、Maven概念

>​		Maven 是一个基于Java平台 自动化构建工具

**发展历程：** Make -->Ant-->Maven-->Gradle

### 功能

- **清理：** 删除编译的结果，为重新编译做准备

- **编译：** java-->class  将java文件转变为class文件
- **测试：** 针对项目中的关键点进行测试，亦可用项目中的测试代码 去测试开发代码
- **报告：** 将测试的结果进行显示
- **打包：** 将项目中包含的多个文件 ，压缩成一个文件，用于安装或部署。（java项目->jar  web项目->war）
- **安装：** 将打成的包放在本地仓库，供其他项目使用
- **部署：** 将打成的 包放到服务器上准备运行

### 作用

- 增加第三方的jar
- jar包之间的依赖关系（当使用一个jar的时候，如果这个jar需要依赖其他jar，则会自动下载所依赖的jar）
- 将项目拆分为若干个模块

**约定配置：**

Maven 提倡使用一个共同的标准目录结构，Maven 使用约定优于配置的原则，大家尽可能的遵守这样的目录结构：

|                目录                |                             目的                             |
| :--------------------------------: | :----------------------------------------------------------: |
|             ${basedir}             |                  存放pom.xml和所有的子目录                   |
|      ${basedir}/src/main/java      |                       项目的java源代码                       |
|   ${basedir}/src/main/resources    |        项目的资源，比如说property文件，springmvc.xml         |
|      ${basedir}/src/test/java      |                项目的测试类，比如说Junit代码                 |
|   ${basedir}/src/test/resources    |                         测试用的资源                         |
| ${basedir}/src/main/webapp/WEB-INF | web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面 |
|         ${basedir}/target          |                         打包输出目录                         |
|     ${basedir}/target/classes      |                         编译输出目录                         |
|   ${basedir}/target/test-classes   |                       测试编译输出目录                       |
|             Test.java              |           Maven只会自动运行符合该命名规则的测试类            |
|          ~/.m2/repository          |                 Maven默认的本地仓库目录位置                  |

##  二、使用Maven

注意：使用之前必须保证有Java环境，否则会失败

### 下载

官网：http://maven.apache.org/

1、进入官网点击 **Download**

![](https://img2020.cnblogs.com/blog/1896749/202004/1896749-20200429114045874-824072041.png)

2、下载最新的版本 为3.6.3，下载最新的版本直接点击  **apache-maven-3.6.3-bin.zip**

![](http://club.codehuan.cn/image-20200429103326918.png)

3、下载其他版本，点击下面的 **archives**

![image-20200429103400506](http://club.codehuan.cn/image-20200429103400506.png)

4、进入之后选择需要下载的版本

<img src="http://club.codehuan.cn/image-20200429103450630.png" alt="image-20200429103450630" style="zoom: 80%;" />

5、选择下载maven-3/3.6.1

![image-20200429103628378](http://club.codehuan.cn/image-20200429103628378.png)

6、点击 **binaries/** 

![image-20200429103726007](http://club.codehuan.cn/image-20200429103726007.png)

7、选择 **apache-maven-3.6.1-bin.zip** ，完成之后得到安装包

![image-20200429103853690](http://club.codehuan.cn/image-20200429103853690.png)

### 安装

选择一个目录，将下载的Maven压缩包进行解压 

![image-20200429104155318](http://club.codehuan.cn/image-20200429104155318.png)



### 配置Maven仓库

1、打开该目录下的 conf/settings.xml 文件

2、在settings标签下面 添加

 ```xml
<localRepository>E:/Maven/repository</localRepository>
 ```



![image-20200429104615765](http://club.codehuan.cn/image-20200429104615765.png)

### 添加远程仓库镜像

找到 mirrors 标签在下面添加：

```xml
<mirrors>

	<!-- 阿里云仓库 -->
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror> 
	
	<!-- 中央仓库1 -->
	<mirror>
		<id>repo1</id>
		<mirrorOf>central</mirrorOf>
		<name>Human Readable Name for this Mirror.</name>
		<url>http://repo1.maven.org/maven2/</url>
	</mirror>

	<!-- 中央仓库2 -->
	<mirror>
		<id>repo2</id>
		<mirrorOf>central</mirrorOf>
		<name>Human Readable Name for this Mirror.</name>
		<url>http://repo2.maven.org/maven2/</url>
	</mirror>

  </mirrors>
```

### 指定Maven 的jdk 版本

在<profiles>标签下面添加：

```xml
<profile>     
		<id>JDK-1.8</id>       
		<activation>       
			<activeByDefault>true</activeByDefault>       
			<jdk>1.8</jdk>       
		</activation>       
		<properties>       
			<maven.compiler.source>1.8</maven.compiler.source>       
			<maven.compiler.target>1.8</maven.compiler.target>       
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>       
		</properties>       
</profile>
```

### 配置Maven环境变量

打开电脑的环境变量配置：右击 **我的电脑** 打开 **属性**，打开高级系统设置

![image-20200429105710436](http://club.codehuan.cn/image-20200429105710436.png)





找到环境变量设置 点击

![image-20200429105742070](http://club.codehuan.cn/image-20200429105742070.png)



在系统变量中 点击新建 填写：

变量名：`MAVEN_HOME`

变量值：`E:\Maven\apache-maven-3.6.1`   就是你解压的Maven 的跟目录

![image-20200429110043453](http://club.codehuan.cn/image-20200429110043453.png)

接着配置PATH：在系统变量中找到PATH，双击打开之后 点击新建 添加： `%MAVEN_HOME%\bin` ,完成之后点击确定

![](http://club.codehuan.cn/image-20200429110236919.png)

### 验证Maven 是否配置完成

win+R 运行CMD

输入 `mvn -version` 或 `mvn -v`，出现下面的则说明 Maven已配置完成

![](http://club.codehuan.cn/image-20200429110534072.png)
