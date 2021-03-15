### docker 安装docker

```bash
# 拉取镜像
docker pull redis
# 编辑docker-compose.yml文件
version: '3'

services:
   redis:
      image: redis
      restart: always
      container_name: myredis
      ports: 
        - 6379:6379
      volumes: 
        - /usr/local/docker/redis/redis.conf:/usr/local/etc/redis/redis.conf:rw  # 挂载配置文件
        - /usr/local/docker/redis/data:/data:rw  # 数据持久化
      command: 
        /bin/bash -c "redis-server /usr/local/etc/redis/redis.conf" # 指定运行时使用的配置文件
 
# 需要去redis下载配置文件redis.conf然后上传
```

### 常用指令

```bash
# 运行服务器
redis-server -p 6379

# 运行客户端
redis-cli -p 6379

# 清除当前数据库
flushdb

# 清除所有数据库
flushall 

# 查看所有key
keys *

# 查看key是否存在
exists [key的值] # 返回1存在 返回0不存在

# 移动key
move [key的值] [目录数据库] # move test 1 将test移动到1数据库

# 设置key过期时间
expire [key的值] [过期时间单位为秒]

# 查看当前key的剩余时间
ttl [key的值]

# 查看key的类型
type [key的值]

# 删除某个key
del [key的值]

# 向key追加内容；如果key不存在就相当于新建key
append [key的值] [需要追加的内容]

# 获取key的长度
strlen [key的值]

# key的值自增
incr [key的值]

# key增加指定数
incrby [key的值] [指定的值]

# key的值自减
decr [key的值]

# 减少指定数
decrby [key的值] [指定的值]

# 截取字符串
getrande [key的值] [开始的下标] [结束的下标] # 0 -1 表示截取全部字符串

# 替代字符串
setrange [key的值] [开始的下标] [需要替换的字符串] # 字符串一对一替换

# 设置过期时间
setex [key的值] [过期时间] [字符串的值]

# key不存在就设置（分布式锁中常常使用）
setnx [key的值] [字符串] # 普通的set会覆盖value

# 批量设置值
mset [key value...]

# 批量获取值
mget [key...]

# setnx的批量设置版
msetnx [key value...] # 若其中的一个key存在，那么所有的key都会设置失败（事务的原子性）

# 设置对象（json格式）
set [key的值] {name:zhangsan,age:12}

# 设置对象的进阶方式 对象名:{id}:{filed}
127.0.0.1:6379> mset user:1:name lisi user:1:age 23
127.0.0.1:6379> keys *
1) "user:1:name"
6) "user:1:age"
127.0.0.1:6379> mget user:1:name user:1:age
1) "lisi"
2) "23"

# 先get再set
getset [key] [value] # 获取的是set之前的值，如果不存在则返回nil（空）

```

### 解决Linux服务器重启Redis数据部分丢失问题

```bash
vim /etc/sysctl.conf
# 在最后一行添加
vm.overcommit_memory = 1
# 使配置生效
sysctl -p
```

### 五大数据类型

> Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作**数据库**、**缓存**和**消息中间件MQ。** 它支持多种类型的数据结构，如字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

#### String

> 操作指令子在`常用指令`那。
>
> String的使用场景：value除了是我们的字符串还可以是我们的数字
>
> - 计数器
> - 统计多单位的数量
> - 粉丝数
> - 对象缓存存储

#### List

**（链表）（list的值可以重复）**

> - 消息队列

```bash
# 从头部放入数据
lpush [key] [value...] # 从头部放入（相当出栈）

# 从尾部放入数据
rpush [key] [value...]

127.0.0.1:6379> rpush list right
(integer) 6
127.0.0.1:6379> lrange list 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
6) "right"

# 取出数据
lrange [key] [start] [stop] # 先进后出（栈）

# 头出栈
lpop [key] [count]

# 尾出栈
lpop [key] [count]

# 获取某一个值 
lindex [key] [index] # index -1 是最后的一个元素

# 获取list的长度
llen [key]

# 准确移除某一个值
lrem [key] [count] [value] # count > 0从前往后移除；count = 0移除全部；count < 0从后往前移除

# 截取list
ltrim [key] [start] [stop]

# 弹出栈底元素然后加入新栈
rpoplpush [key1] [key2]

# 在key中指定下标设置value 
lset [key] [index] [value] # 其中index不能超出已有的下标范围

# 在key的某几个元素中间插入
linsert [key] [before|after] [被插入的value] [需要插入的value] # 如果有两个相同的value则优先靠近头部的；如果key不存在则什么都不会发生
```

#### Set

**无需不重复集合**

> - 好友列表
> - 关注列表

```bash
# 添加值
sadd [key] [value...] # 添加失败时返回0，添加重复数据时会失败

# 查看值成员
smembers [key]

# 查看是否存在成员
sismember [value] [value] # 存在返回1，不存在返回0

# 查看成员的个数
scard [key]

# 移除某个成员
srem [key] [value]

# 随机获取n个成员
srandmember [key] [count]

# 随机弹出n个成员
spop [key] [count]

# 移动一个指定的成员到另一个set集合中
smove [原key] [目标key] [value] # 如果目标key不存在，则创建一个新的key

# 差集
sdiff [key1] [key2] # key1 和 key2 的差集

# 交集
sinter [key1] [key2]

# 并集
sunion [key1] [key2]
```

#### Hash

>key-map（<key,value>）