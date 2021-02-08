# Redis简介与安装

## 简介

>​		Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

## 安装

1、将下载好的 `redis-5.0.7.tar.gz`压缩包放到Linux系统上的 /opt 目录下面

![image-20200502125752669](http://q936eat5v.bkt.clouddn.com/images/image-20200502125752669.png)

2、解压文件

```shell
tar -zxvf redis-5.0.7.tar.gz
```

3、通过 `which gcc `查看有没有 gcc，如果没有则进行下面命令安装

```shell
yum -y install gcc
```

4、安装完成之后进入到**redis-5.0.7**目录执行命令`make`

```bash
make
```

5、完成之后进入到src 目录下面 执行 `make install`，出现下图说明成功

```bash
make install
```

![image-20200502130116945](http://q936eat5v.bkt.clouddn.com/images/image-20200502130116945.png)

6、在 /usr/local/bin/ 目录下创建 myconfig 目录

```bash
mkdir -p /usr/local/bin/myconfig
```

7、进入到**redis-5.0.7**目录，复制 **redis-5.0.7**下面的 redis.conf 到 /usr/local/bin/myconfig 目录下面

```bash
cp redis-5.0.7 /usr/local/bin/myconfig
```

8、切换到bin目录下，启动时并制定配置文件

![image-20200502131256078](http://q936eat5v.bkt.clouddn.com/images/image-20200502131256078.png)

 ```bash
cd ../usr/local/bin/
redis-server /usr/local/bin/myconfig/redis.conf
 ```

9、修改redis.conf文件，将里面的`daemonize`改为yes

```bash
cd myconfig
vi redis.conf
```

10、修改完成之后启动 redis,进入到bin目录

```
redis-cli -p 6370
```

![image-20200502131917676](http://q936eat5v.bkt.clouddn.com/images/image-20200502131917676.png)



查看redis端口

![image-20200501212101581](http://q936eat5v.bkt.clouddn.com/images/image-20200501212101581.png)

# 数据类型

**五大基本数据类型 ：String、List 、Set、 Hash 、 Zset**

**三种特殊数据类型： geo(Geospatial地理位置) 、hyperloglog、bitmap**

## String（字符串）

> ​		String类型是包含很多种类型的特殊类型，并且是二进制安全的。比如序列化的对象进行存储，比如一张图片进行二进制存储，比如一个简单的字符串，数值等等。

**set和get方法:** 

```bash
设置值: set name value 
取值:get name ( 设置name多次会覆盖）

删除值: del name

移动值：move name 1   1代表数据库

使用setnx (not exist)
name如果不存在进行设置，存在就不需要进行设置了，返回0

使用setex (expired)
setex color 10 red设置color的有效期为10秒，10秒后返回nil (在redis里nil表示空)

使用setrange替换字符串:
set email 174754613@qq.com
setrange email 10 ww (10表示从第几位开始替换， 后面跟上替换的字符串)

使用一次性设置多个和获取多个值的mset、mget方法:
mset key1 bhz key2 bai key3 28;对应的mget key1 key2 key3方法
对应的也有msetnx和mget方法。

一次性 设置和取值的getset方法: 
set key4 CC
getset key4 changchun返回旧值并设置新值的方法。

incr和decr方法:对某一个值进 行递增和递减
incrby和decrby方法:对某个值进行指定长度的递增和递减
append [name]方法:字符串追加方法
strlen [name]方法:获取字符串的长度
```

## Hash

> ​		Hash类型是String类型的field和value的映射表，或者说一个String来合。 它的特别适合存储对象，相比较而言，将一个对象类型存储在Hash类型里要李存储在String类型里占用更少的内存空间，并方便存取整个对象。

```bash
形如: hset myhash field1 hello (含义是hset是hash集合，myhash是集合名字
field1是字段名hello为其值)使用hget myhash field1获取内容，也可以存储多个值。

hmset可以进行批量存储多个键值对: hmset myhash sex nan addr beiing,也可
使用hmget进行批量获取多个键值对。
同样也有hsetnx,和setnx大同小异。

hincrby和hdecrby集合递增和递减。

hexists是否存在key如果存在返回不存在返回0

hlen返回hash集合里的所有的键数值

hdel删除指定hash的field

hkeys返回hash里所有的字段

hvals返回hash的所有value

hgetall返回hash里所有的key和value

```

## List

> ​		List类型是一一个链表结构的集合，其主要功能有push、pop、获取元素等。更详细的说，List类
> 型是一个双端链表的结构，我们可以通过相关操作进行集合的头部或者尾部添加删除元素，list
> 的设计非常简单精巧，即可以做为栈，又可以作为队列。满足绝大多数需求。

```bash
lpush方法:从头部加入元素 (栈)先进后出
形如: lpush list1 "hello" Ipush list1 "world"
Irange list1 0 -1 (表示从头取到末尾)
rpush方法:从尾部加入元素(队列)先进先出
形如: rpush list2 "beijing" rpush list2 "sxt"
Irange list2 0-1
linsert方法:插入元素
形如: linsert list3 before [集合的元素] [插入的元素]
```

![image-20200504000418643](http://q936eat5v.bkt.clouddn.com/images/image-20200504000418643.png)

**lset方法：将指定下表的元素替换掉**

![image-20200504000521172](http://q936eat5v.bkt.clouddn.com/images/image-20200504000521172.png)

**Irem 方法:删除元素 返回删除的个数**

![image-20200504000602219](http://q936eat5v.bkt.clouddn.com/images/image-20200504000602219.png)

**Itrim方法:保留指定key的值范围内的数据**

![image-20200504000644977](http://q936eat5v.bkt.clouddn.com/images/image-20200504000644977.png)

**lpop方法:从list的头部删除元素，并返回删除元素**
**rpop方法:从list的尾部删除元素，并返回删除元素**

![image-20200504000653202](http://q936eat5v.bkt.clouddn.com/images/image-20200504000653202.png)



**rpoplpush方法:**第一 步从尾部删除元素，然后第二步并从头部加入元素
**lindex方法:**返回名称为key的list中index位置的元素
**llen方法:**返回元素的个数

![image-20200504000803677](http://q936eat5v.bkt.clouddn.com/images/image-20200504000803677.png)

## Set

>set集合是string类型的无序集合，set是通过hashtable实现的，对集合我们可以取交集、并集、差集。

```bash
sadd方法:向名称为key的set中添加元素
小结: set集合不允许重复元素smembers查看set集合的元素
srem方法:删除set集 合元素
spop方法:随机返 回删除的key
sdiff方法: 
返回俩个集合的不同元素(哪个集合在前面就以哪个集合为标准)
sdiffstore方法:将返 回的不同元素存储到另外-一个集合里
```

小结:这里是把set1和set2的不同元素(以set1为准)存储到set3集合里

![image-20200504000954860](http://q936eat5v.bkt.clouddn.com/images/image-20200504000954860.png)

```bash
sinter方法:返回集合的交集
sinterstrbe方法:返回交集结果， 存入set3中
```

![image-20200504001117789](http://q936eat5v.bkt.clouddn.com/images/image-20200504001117789.png)



```bash
sunion方法:取并集
sunionstore方法:取得并集，存入set3中
```

![image-20200504001152528](http://q936eat5v.bkt.clouddn.com/images/image-20200504001152528.png)

```bash
smove方法:从一个set集合移动到另一个set集合里
小结将set1中的元素移动到set2中(相当于剪切复制)
```

![image-20200504001230944](http://q936eat5v.bkt.clouddn.com/images/image-20200504001230944.png)

```bash
scard方法:查看集合里元素个数
sismember方法:判断某元素是否为集合中的元素
小结返回1代表是集合中的元素，0代表不是
srandmember方法:随机返回一个元素
```

![image-20200504001343348](http://q936eat5v.bkt.clouddn.com/images/image-20200504001343348.png)

```bash
zadd向有序集合中添加一一个元素，该元素如果存在，则更新顺序
小结在重复插入的时候会根据顺序属性更新
```

![image-20200504001415331](http://q936eat5v.bkt.clouddn.com/images/image-20200504001415331.png)

```bash
zrem:删除名称为key的zset中的元素member
zincrby :以指定值去自动递增或者减少，用法和之前的incrby类似
zrangebyscore :找到指定区间范围的数据进行返回
zremrangebyrank :删除1到1 ( 只删除索引1)
zremrangebyscore :删除指定序号
```

![image-20200504001459914](http://q936eat5v.bkt.clouddn.com/images/image-20200504001459914.png)



```bash
zrank :返回排序索引从小到大排序(升序排序之后再找索引)
注意一个是顺序号一个是索引zrank返回的是索引
zrevrank :返回排序索引从大到小排序(降序排序之后再找索引)
```

![image-20200504001556117](http://q936eat5v.bkt.clouddn.com/images/image-20200504001556117.png)



```bash
zrangebyscore zset1 2 3 withscores :找到指定区间范围的数据进行返回
```

![image-20200504001621700](http://q936eat5v.bkt.clouddn.com/images/image-20200504001621700.png)

```bash
zcard :返回集合里所有元素的个数
```

![image-20200504001715895](http://q936eat5v.bkt.clouddn.com/images/image-20200504001715895.png)

```bash
zcount :返回集合中score在给定区间中的数量
```

![image-20200504001809860](http://q936eat5v.bkt.clouddn.com/images/image-20200504001809860.png)

```bash
zremrangebyrank zset [from] [to] (删除索引)
```

![image-20200504001843077](http://q936eat5v.bkt.clouddn.com/images/image-20200504001843077.png)

```bash
zremrangebyscore zset [from] [to] (删除指定序号)
```

![image-20200504001911885](http://q936eat5v.bkt.clouddn.com/images/image-20200504001911885.png)



## 三种特殊数据类型

### Geospatial地理位置

用来定位，附近的人

**六个命令：**

![image-20200504002410200](http://q936eat5v.bkt.clouddn.com/images/image-20200504002410200.png)

>geoadd 添加地理位置

```bash
# 规则：两级无法直接添加，我们一般会下载城市数据，直接通过java程序一次性导入！
# 有效的经度从-180度到180度。
# 有效的纬度从-85.05112878度到85.05112878度。
# 当坐标位置超出上述指定范围时，该命令将会返回一个错误。
# 127.0.0.1:6379> geoadd china:city 39.90 116.40 beijin
(error) ERR invalid longitude,latitude pair 39.900000,116.400000
# 参数 key 值（）
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqi 114.05 22.52 shengzhen
(integer) 2
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
```

>getpos： 获得当前定位

```bash
127.0.0.1:6379> GEOPOS china:city beijing # 获取指定的城市的经度和纬度！
1) 1) "116.39999896287918091"
2) "39.90000009167092543"
127.0.0.1:6379> GEOPOS china:city beijing chongqi
1) 1) "116.39999896287918091"
2) "39.90000009167092543"
2) 1) "106.49999767541885376"
2) "29.52999957900659211"
```

>GEODIST: 两人之间的距离

单位： m 表示单位为米。 km 表示单位为千米。 mi 表示单位为英里。 ft 表示单位为英尺。

```bash
127.0.0.1:6379> GEODIST china:city beijing shanghai km # 查看上海到北京的直线距离
"1067.3788"
127.0.0.1:6379> GEODIST china:city beijing chongqi km # 查看重庆到北京的直线距离
"1464.0708"
```

>georadius 以给定的经纬度为中心， 找出某一半径内的元素 (场景：附近的人)

```bash
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km # 以110，30 这个经纬度为中心，寻
找方圆1000km内的城市
```

>GEORADIUSBYMEMBER 找出位于指定元素周围的其他元素

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km
1) "beijing"
2) "xian"
```

>GEOHASH 命令 - 返回一个或多个位置元素的 Geohash 表示

该命令将返回11个字符的Geohash字符串!

```bash
# 将二维的经纬度转换为一维的字符串，如果两个字符串越接近，那么则距离越近！
127.0.0.1:6379> geohash china:city beijing chongqing
1) "wx4fbxxfke0"
2) "wm5xzrybty0"

```

>GEO 底层的实现原理其实就是 Zset！我们可以使用Zset命令来操作geo！

```bash
127.0.0.1:6379> ZRANGE china:city 0 -1 # 查看地图中全部的元素
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city beijing # 移除指定元素！
(integer) 1
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
```

### Hyperloglog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法。

优点:	计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。

#### 什么是基数?

>比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

命令

```bash
PFADD key element [element ...]  # 添加指定元素到 HyperLogLog 中。
PFCOUNT key [key ...]  #返回给定 HyperLogLog 的基数估算值。
PFMERGE destkey sourcekey [sourcekey ...] # 将多个 HyperLogLog 合并为一个 HyperLogLog
```

![image-20200504004518095](http://q936eat5v.bkt.clouddn.com/images/image-20200504004518095.png)

PFMERGE 命令将多个 HyperLogLog 合并为一个 HyperLogLog 

如果允许容错，那么一定可以使用 Hyperloglog ！不允许容错，就使用 set 或者自己的数据类型即可！

### Bitmap

#### BitMap是什么?

			通过一个bit位来表示某个元素对应的值或者状态,其中的key就是对应元素本身。Bitmaps 本身不是一种数据结构，实际上它就是字符串（key 对应的 value 就是上图中最后的一串二进制），但是它可以对字符串的位进行操作。 Bitmaps 单独提供了一套命令，所以在 Redis 中使用 Bitmaps 和使用字符串的方法不太相同。可以把 Bitmaps 想象成一个以 位 为单位的数组，数组的每个单元只能存储 0 和 1，数组的下标在Bitmaps中叫做偏移量。

 **BitMap的命令**

SETBIT语法：

```bash
SETBIT key offset value
```



#### 使用场景

##### 1、用户签到

![image-20200504005829522](http://q936eat5v.bkt.clouddn.com/images/image-20200504005829522.png)

查看某一天是否有打卡！

```ba
127.0.0.1:6379> GETBIT dat 4
(integer) 0
```

统计操作，统计 打卡的天数！

```bash
127.0.0.1:6379> BITCOUNT day
(integer) 5
```

##### 2、统计活跃用户

##### 3、用户在线状态



# 主从复制

> 复制原理

Slave启动成功连接到master后会发送一个sync同步命令
Master接到命令,启动后台的存盘进程,同时收集所有接收到的用于修改数据集命令,在后台进程执行完毕之后, master将传送**整个数据文件到slave ,并完成一次完全同步。**
**全量复制:**而slave服务在接收到数据库文件数据后,将其存盘并加载到内存中。
**增量复制:** Master继续将新的所有收集到的修改命令依次传给slave ,完成同步
但是只要是重新连接master , 一次完全同步(全量复制)将被自动执行!我们的数据一-定可以在从机中看到!

>主从配置

**配置一个主节点，两个从节点**

1、复制 redis.conf 

```bash
[root@codehuan bin]# pwd
/usr/local/bin
[root@codehuan bin]# cp myconfig/redis.conf /usr/local/bin/myconfig/redis6380.conf
[root@codehuan bin]# cp myconfig/redis.conf /usr/local/bin/myconfig/redis6381.conf
```





![image-20200503233634527](http://q936eat5v.bkt.clouddn.com/images/image-20200503233634527.png)



2、修改配置文件，修改端口号 port、pidfile、logfile，并且配置主节点信息

```bash
[root@codehuan bin]# vi myconfig/redis6380.conf
```

```bash
port 6379
修改为：
port 6380

pidfile /var/run/redis_6379.pid
修改为：
pidfile /var/run/redis_6380.pid

logfile ""
修改为：
logfile "6380.log"

dbfilename ‘dump.rdb’
改为
dbfilename ‘dump6381.rdb’

增加一行 ：
slaveof 127.0.0.1 6379
```

同理修改其他的

3、启动

```bash
[root@codehuan bin]# redis-server myconfig/redis.conf
[root@codehuan bin]# redis-server myconfig/redis6380.conf
[root@codehuan bin]# redis-server myconfig/redis6381.conf
[root@codehuan bin]# redis-cli -p 6379
[root@codehuan bin]# redis-cli -p 6380
[root@codehuan bin]# redis-cli -p 6381
[root@codehuan bin]# ps -ef | grep redis
root     27587     1  0 23:22 ?        00:00:00 redis-server 0.0.0.0:6380
root     27615 22086  0 23:22 pts/1    00:00:00 redis-cli -p 6380
root     29333     1  0 23:26 ?        00:00:00 redis-server 0.0.0.0:6379
root     29458 18437  0 23:27 pts/0    00:00:00 redis-cli -p 6379
root     30298     1  0 23:29 ?        00:00:00 redis-server 0.0.0.0:6381
root     30314 27815  0 23:29 pts/2    00:00:00 redis-cli -p 6381
root     30788 30021  0 23:30 pts/3    00:00:00 grep --color=auto redis
```

4、到主节点6379下面查看

```bash
info replication
```

![image-20200503234934690](http://q936eat5v.bkt.clouddn.com/images/image-20200503234934690.png)

看到存在两个 从节点6380和6381端口

5、到主节点6380和6381下面查看

![image-20200503235057880](http://q936eat5v.bkt.clouddn.com/images/image-20200503235057880.png)

![image-20200503235108608](http://q936eat5v.bkt.clouddn.com/images/image-20200503235108608.png)

主从配置成功！

# 简单事务

> 开启事务

​		使用`multi` 打开事务，这时设置的数据都会放入队列里进行保存最后使用`exec`执行，把数据依次存储到redis中，使用`discard`方法取消事务。

![image-20200504123312843](http://q936eat5v.bkt.clouddn.com/images/image-20200504123312843.png)

**redis 事务不能保证同时成功或失败进行提交或回滚，所以redis的事务还是比较简单的**

![image-20200504123529098](http://q936eat5v.bkt.clouddn.com/images/image-20200504123529098.png)



# 持久化机制

> ​		redis是一一个支持持久化的内存数据库，也就是说redis需要经常将内存中的数据同步到硬盘来保证持久化。

**持久化的两种方式：**

**1、RDB持久化：(`snapshotting` (快照)默认方式, ):**

**2、AOF持久化：`append-only file` （缩写aof）方式（类似于oracle 日志）：**

## RDB 持久化

> ​		将内存中以快照的方式写入到二进制文件中，默认为dump.rdb.可以通过配置设置自动做快照持久化的方式。我们可以配置redis在n秒内如果超过m个key则修改就自动做快照。

**RDB持久化配置:**

```bash
save 900 1  #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。
save 300 10  #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。
save 60 10000 #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。
```

### RDB 优点：

**1、可以获得不同版本的数据集**

			因为rdb持久化方式 是在每隔一段时间进行执行，在我们有对应的需求时，我们可以在每个小时保存一下过去24小时内的数据，同时每天保存过去30天的数据，这样我们可以得到不同版本的数据集

**2、非常适用于灾难恢复**

	 RDB 是一个紧凑的单一文件,很方便传送到另一个远端数据中心

**3、RDB 持久化方式可以最大化 Redis 的性能**

			RDB 在保存 RDB 文件时父进程唯一需要做的就是 fork 出一个子进程,接下来的工作全部由子进程来做，父进程不需要再做其他 IO 操作

**4、与AOF相比,在恢复大的数据集的时候，RDB 方式会更快一些。**

### RDB  缺点

**1、当出现宕机时，数据容易丢失**

**2、影响性能**

	>​		RDB 需要经常fork子进程来保存数据集到硬盘上,当数据集比较大的时候,fork的过程是非常耗时的,可能会导致Redis在一些毫秒级内不能响应客户端的请求.如果数据集巨大并且CPU性能不是很好的情况下,这种情况会持续1秒,AOF也需要fork,但是你可以调节重写日志文件的频率来提高数据集的耐久度.

###工作原理

当 Redis 需要保存 `dump.rdb` 文件时， 服务器执行以下操作:

- Redis 调用forks. 同时拥有父进程和子进程。
- 子进程将数据集写入到一个临时 RDB 文件中。
- 当子进程完成对新 RDB 文件的写入时，Redis 用新 RDB 文件替换原来的 RDB 文件，并删除旧的 RDB 文件。

这种工作方式使得 Redis 可以从写时复制（copy-on-write）机制中获益。

## AOF 持久化

	>​		由于快照方式是在一定时间间隔做一次，所以很有可能发生redis意外down 的情况就会丢失最后一次快照后的所有修改数据、aof比快照方式有更好的持久化性，是由于在使用aof时，redis会将每一个接收到的写命令都通过write函数追加到命令中，当redis 重新启动时，会重新执行文件中保存的写命令来在内存中重建这个数据的内容，这个文件在bin目录下：

`appendonly.aof `: aof 不是立即写到硬盘上，可以通过配置文件修改强制写在硬盘中。

**aof设置**

```bash
appendonly yes 1/启动aof持久化方式有三种修改方式:
#appendfsync always /收到写命令就立即写入到磁盘，效率最慢，但是保证完全的持久化
#appendfsync everysec 11每秒钟写入磁盘-次， 在性能和持久化方面做了很好的折中
#appendfsync no /1完全依赖os性能最好持玖化没保证
```

### AOF 优点：

**1、使用AOF 会让Redis更加耐久:可以使用不同的 fsync （更新）策略（更加灵活）：**

>​		无 fsync、每秒 fsync 、每次写的时候 fsync .使用默认的每秒 fsync 策略, Redis 的性能依然很好( fsync 是由后台线程进行处理的,主线程会尽力处理客户端请求),一旦出现故障，你最多丢失1秒的数据。

**2、容错率高**

> ​		AOF文件是一个只进行追加的日志文件,所以不需要写入seek,即使由于某些原因(磁盘空间已满，写的过程中宕机等等)未执行完整的写入命令,你也也可使用redis-check-aof工具修复这些问题.

**3、容易阅读、分析**

>​		AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

**4、操作安全，文件不会丢失**

>​		Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。

### AOF 缺点

**1、消耗内存：**

>对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积

**2、速度相对于RDB比较慢：**

>​		根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。 不过在处理巨大的写入载入时，RDB 可以提供更有保证的最大延迟时间（latency）。

### 工作原理

AOF 重写和 RDB 创建快照一样，都巧妙地利用了写时复制机制:

- Redis 执行 fork() ，现在同时拥有父进程和子进程。
- 子进程开始将新 AOF 文件的内容写入到临时文件。
- 对于所有新执行的写入命令，父进程一边将它们累积到一个内存缓存中，一边将这些改动追加到现有 AOF 文件的末尾,这样样即使在重写的中途发生停机，现有的 AOF 文件也还是安全的。
- 当子进程完成重写工作时，它给父进程发送一个信号，父进程在接收到信号之后，将内存缓存中的所有数据追加到新 AOF 文件的末尾。
- 搞定！现在 Redis 原子地用新文件替换旧文件，之后所有命令都会直接追加到新 AOF 文件的末尾。

## 持久化方式切换

### RDB方式切换为AOF方式

- 为最新的 `dump.rdb `文件创建一个备份。

- 将备份放到一个安全的地方。

- 执行以下两条命令:

- ```bash
  redis-cli config set appendonly yes
  redis-cli config set save “”
  ```

- 确保写命令会被正确地追加到 AOF 文件的末尾。

- 执行的第一条命令开启了 AOF 功能： Redis 会阻塞直到初始 AOF 文件创建完成为止， 之后 Redis 会继续处理命令请求， 并开始将写入命令追加到 AOF 文件末尾。

执行的第二条命令用于关闭 RDB 功能。 这一步是可选的， 如果你愿意的话， 也可以同时使用 RDB 和 AOF 这两种持久化功能。

重要:别忘了在 redis.conf 中打开 AOF 功能！ 否则的话， 服务器重启之后， 之前通过 CONFIG SET 设置的配置就会被遗忘， 程序会按原来的配置来启动服务器。



# 哨兵

## 概述

>​		哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

Redis 的 Sentinel 系统用于管理多个 Redis 服务器（instance）， 该系统执行以下三个任务：

- **监控（Monitoring**）： Sentinel 会不断地检查你的主服务器和从服务器是否运作正常。
- **提醒（Notification）**： 当被监控的某个 Redis 服务器出现问题时， Sentinel 可以通过 API 向管理员或者其他应用程序发送通知。
- **自动故障迁移（Automatic failover）**： 当一个主服务器不能正常工作时， Sentinel 会开始一次自动故障迁移操作， 它会将失效主服务器的其中一个从服务器升级为新的主服务器， 并让失效主服务器的其他从服务器改为复制新的主服务器； 当客户端试图连接失效的主服务器时， 集群也会向客户端返回新主服务器的地址， 使得集群可以使用新主服务器代替失效服务器。

![image-20200504165644888](http://q936eat5v.bkt.clouddn.com/images/image-20200504165644888.png)

**哨兵配置：**

1、编辑`sentinel.conf`哨兵配置文件

```bash
vi myconfig/sentinel.conf
```

在里面添加：

```bash
sentinel monitor mymaster 127.0.0.1 6380 1 

还可以配置其他的故障转移时间等等，不配置使用默认的
```

2、启动哨兵

```bash
serve-sentinel myconfig/sentinel.conf
```

3、出现下面

<img src="http://q936eat5v.bkt.clouddn.com/images/image-20200504175654130.png" alt="image-20200504175654130"  />



> 哨兵优点

- 哨兵集群模式是基于主从模式的，所有主从的优点，哨兵模式同样具有。
- 主从可以切换，故障可以转移，系统可用性更好。
- 哨兵模式是主从模式的升级，系统更健壮，可用性更高。

>哨兵缺点

- Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，运维人员在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。
- 配置复杂

> 哨兵集群

与上面一样 继续配置创建一个`sentinel.conf`配置端口信息 等等~~