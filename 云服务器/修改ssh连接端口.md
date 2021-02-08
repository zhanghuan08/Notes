# CentOS7修改SSH端口

## 1. 修改ssh配置文件

### 1.1 查看默认端口

```shell
# 查看当前ssh服务器端口号
netstat -tunlp | grep "ssh"
```

![image-20210117182713908](http://image.codehuan.cn/image/image-20210117182713908.png)

这里我是修改过了，默认的话是22

### 1.2 修改端口

执行命令：

```shell
vi /etc/ssh/sshd_config 
```

![image-20210117182930953](http://image.codehuan.cn/image/image-20210117182930953.png)

首次打开发现 Port 22是被注释的

去掉前面的 #,再增加一条Port 202，添加的监听端口号最好为10000~65535区间之内，即2的16次方

```
Port 22
Port 202
```

这样做防止202端口不能连接的情况下还可以使用22端口连接

## 2. 防火墙放行

### 2.1 查看防火墙状态

```shell
firewall-cmd --state
```

![image-20210117185051786](http://image.codehuan.cn/image/image-20210117185051786.png)

防火墙如果关闭，则需要打开防火墙，执行命令：

```shell
# 打开防火墙
systemctl start firewalld
# 开机时启动firewall
systemctl enable firewalld.service    
```

### 2.2 防火墙放行端口 202

```shell
firewall-cmd --zone=public --add-port=202/tcp --permanent
```

### 2.3 重启防火墙

```shell
# 重启防火墙
systemctl restart firewalld
# 重新加载防火墙策略
firewall-cmd --reload
```

### 2.4查看已开启端口

```shell
 firewall-cmd --list-port
```

![image-20210117185326080](http://image.codehuan.cn/image/image-20210117185326080.png)

## 3. 向SELinux中添加修改的SSH端口

### 3.1安装SELinux的管理工具 semanage

执行命令：

```shell
yum install policycoreutils-python -y 
```

查询当前 ssh 服务端口：

![image-20210117202951929](http://image.codehuan.cn/image/image-20210117202951929.png)

### 3.2向 SELinux 中添加 ssh 端口：

```shell
semanage port -a -t ssh_port_t -p tcp 202
```

查询添加完之后的 ssh 服务端口：![image-20210117203123509](http://image.codehuan.cn/image/image-20210117203123509.png)

### 3.3 重启ssh服务

```shell
systemctl restart sshd
```

测试成功后，将22端口注释掉即可！

## 4. 疑问联系

有疑问联系（`QQ、微信`同）：

QQ：1730272469

<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=1730272469&site=qq&menu=yes"><img border="0" src="http://wpa.qq.com/pa?p=2:1730272469:53" alt="点击这里给我发消息" title="点击这里给我发消息"/></a>

