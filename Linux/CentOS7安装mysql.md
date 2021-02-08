# CentOS 7 安装MySQL 5.7.32

## 1. 下载

官网下载（速度慢，不推荐使用）:[https://dev.mysql.com/downloads/mysql/](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdownloads%2Fmysql%2F)

清华镜像站下载（推荐）：https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.32-linux-glibc2.12-x86_64.tar.gz

![](http://club.codehuan.cn/images/20210114233437.png)

不要下成 mysql-5.7.32.tar.gz，我已经下载过了，踩过坑了。

## 2.安装

### 2.1. 将安装包移动到CentOS系统上

我这里使用的是 XobaXterm 软件，你们可以选择其他的工具

![](http://club.codehuan.cn/images/20210114234210.png)

### 2.2. 解压

```shell
cd /usr/local
tar -zxvf mysql-5.7.32-linux-glibc2.12-x86_64.tar.gz
```

![image-20210115000325109](http://image.codehuan.cn/image/image-20210115000325109.png)

### 2.3. 移动到服务器的 `/usr/local/mysql` 目录下

```shell
mv mysql-5.7.32-linux-glibc2.12-x86_64 /usr/local/mysql
```

### 2.4. 创建mysql用户与用户组，赋予权限

```shell
groupadd mysql                     #创建用户
useradd -r -g mysql mysql          #创建用户组
mkdir -p  /data/mysql              #创建目录
chown mysql:mysql -R /data/mysql   #赋予权限
```

### 2.5. 配置环境

```shell
vi /etc/my.cnf
```

<img src="http://image.codehuan.cn/image/image-20210115001003403.png" alt="image-20210115001003403"  />

```xml
[mysqld]
bind-address=0.0.0.0
port=3306
user=mysql
basedir=/usr/local/mysql
datadir=/data/mysql
socket=/tmp/mysql.sock
character-set-server=utf8
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
explicit_defaults_for_timestamp=true
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/data/mysql/mysql.err
pid-file=/data/mysql/mysql.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```

### 2.6. 进入 bin 目录初始化

```shell
cd /usr/local/mysql/bin/                    #进入bin目录
./mysqld --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql/ --datadir=/data/mysql/ --user=mysql --initialize               #初始化
```

![image-20210115001300215](http://image.codehuan.cn/image/image-20210115001300215.png)

提示错误，缺少环境

![image-20210115001417699](http://image.codehuan.cn/image/image-20210115001417699.png)

2.7. 下载`libaio`，下载完成之后再进行初始化

```shell
yum install -y libaio
```

![image-20210115001635851](http://image.codehuan.cn/image/image-20210115001635851.png)