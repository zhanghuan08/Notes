# Win10 安装MySQL 5.7.32（解压版）



## MySQL 5.7.32 下载

官网下载（速度慢，不推荐使用）:[https://dev.mysql.com/downloads/mysql/](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdownloads%2Fmysql%2F)

清华镜像站下载（推荐）：https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/

![image-20201124110832958](http://club.codehuan.cn/images/image-20201124110832958.png)

## MySQL5.7.32解压安装

**将文件解压到指定目录，我的解压目录为：E:\MySQL\mysql-5.7.32-winx64**

![image-20201124111035933](http://club.codehuan.cn/images/image-20201124111035933.png)

**进入文件创建my.ini文件**

![image-20201124111127915](http://club.codehuan.cn/images/image-20201124111127915.png)

**打开my.ini，粘贴下面内容**

```shell
[Client]
#设置3306端口
port = 3306
[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=E:\MySQL\mysql-5.7.32-winx64
# 设置mysql数据库的数据的存放目录
datadir=E:\MySQL\mysql-5.7.32-winx64\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
```

## 添加环境变量

**1、新建系统变量**

- **变量名称**：MYSQL_HOME
-  **变量值**：MySQL安装目录。 如: E:\MySQL\mysql-5.7.32-winx64

![image-20201124111619857](http://club.codehuan.cn/images/image-20201124111619857.png)

![image-20201124111641211](http://club.codehuan.cn/images/image-20201124111641211.png)

**2、添加系统变量Path**

 添加 %MYSQL_HOME%\bin 

![image-20201124111731075](http://club.codehuan.cn/images/image-20201124111731075.png)

![image-20201124111744950](http://club.codehuan.cn/images/image-20201124111744950.png)

## 以管理员身份运行cmd

进入到mysql的bin目录，执行mysqld -install命令进行安装



![image-20201124112043020](http://club.codehuan.cn/images/image-20201124112043020.png)

完成之后提示Service successfully installed.

再执行 mysqld --initialize-insecure --user=mysql 命令初始化



![image-20201124112242838](http://club.codehuan.cn/images/image-20201124112242838.png)

执行之后，会在安装目录生成data目录，并且生成root用户



![image-20201124112356865](http://club.codehuan.cn/images/image-20201124112356865.png)

## 命令行启动

**以管理员方式，快捷键win+X+A**

**输入启动命令**: net start mysql

![image-20201124112608017](http://club.codehuan.cn/images/image-20201124112608017.png)

## 设置密码

**刚安装的mysql的root用户是没有密码的，需要设置密码**

**1、进入MySQL:**

```shell
mysql -u root -p
```



![image-20201124113059820](http://club.codehuan.cn/images/image-20201124113059820.png)

![image-20201124113109804](http://club.codehuan.cn/images/image-20201124113109804.png)

**2、输入下面命令设置密码**

```bash
# 切换到mysql数据库
use mysql;
#设置密码：password:新密码；user: 用户
update user set authentication_string=password('root') where user='root';
# 刷新MySQL的系统权限相关表
flush privileges;
```



![image-20201124113400444](http://club.codehuan.cn/images/image-20201124113400444.png)

## 设置远程连接

**1、输入以下命令**

```bash
#进入Mysql  -u:指用户； -p指密码
mysql -u root -p
Enter password: 密码
# 切换到mysql数据库
use mysql;
#设置user用户远程访问
GRANT ALL ON *.* TO user@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
# 刷新MySQL的系统权限相关表
flush privileges;
```

**2、测试远程连接**

![image-20201124113639838](http://club.codehuan.cn/images/image-20201124113639838.png)

安装完成！