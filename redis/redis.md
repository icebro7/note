# redis

基于内存的Nosql数据存储系统



# 一、基本操作

```
# 启动Redis
redis-server

# 连接到redis命令行
 redis-cli	
 redis-cli --raw (显示中文)
```



# 二、常用命令

### 1、基础部分

```
# 设置
SET key value
SETNX key value (键不存在时设置value)


# 读取
GET key
 
 
# 删除
DEL key [key ...]
FLUSHALL (删所有键)
 
 
# 查找
KEYS *


# 判断
EXISTS key
 
 
# 设置键10s过期时间
EXPIRE key 10
SETEX key 10 value (键值对过期)


# 查看键存活时间
TTL key
```



### 2、LIST

```
# 增
# 设置列表并添加元素,表头（左侧）
LPUSH key element [element ...]

# 设置列表并添加元素,表尾（右侧）
RPUSH key element [element ...]

# 删
# 表头删
LPOP key [count ...]

# 表尾删
RPOP key [count ...]

# 删除指定元素
LREM key count element

# 删除start和stop之间以外的元素
LTRIM key start stop



# 改
# 将索引位置元素修改
LSET key index element


# 查
# 返回列表key中，位于start和stop之间的元素，包含
LRANGE key start stop

# 返回列表key的⻓度
LLEN key

# 返回索引元素
LINDEX key index

```



### 3、SET

```
# 增
# 将⼀个或多个元素加⼊到集合key中
SADD key member [member ...]


# 删
# 将⼀个或多个元素从集合key中移除
SREM key member [member ...]

# 删除随机元素
SPOP key [count]


# 查
# 查看所有
SMEMBERS key

# 查看元素是否在集合中
SISMEMBER key member

# 查看集合长度
SCARD key


```



### 4、SortedSets

分数-成员

```
# 增
# 将⼀个或多个member元素及其分数score加⼊到有序集合key中
ZADD key [NX|XX] [GT|LT] [CH] [INCR] score member [score 
member ..]



# 删
# 将⼀个或多个成员从集合key中移除
ZREM key member [member ..]

# 指定排名区间内的所有member(从⼩到⼤)
ZREMRANGEBYRANK key start stop

# 指定分数区间内的所有member(从⼩到⼤)
 ZREMRANGEBYSCORE key min max

# 指定字典序区内的所有member(从⼩到⼤)
ZREMRANGEBYLEX key min max


# 查
# 查看成员member的分数值
ZSCORE key member

# 查看长度
ZCARD key

# 分数值score在min和max之间（包括等于）的成员的数量
ZCOUNT key min max

# 指定区间内的成员（从⼩到⼤排列）
ZRANGE key start stop [BYSCORE|BYLEX] [REV] [LIMIT offset count] 
[WITHSCORES]

# 指定区间内的成员（从⼤到⼩排列）
ZREVRANGE key start stop [WITHSCORES]


# 值在指定区间内的成员（从⼩到⼤排列）
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]

# 值在指定区间内的成员（从⼤到⼩排列）
 ZREVRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset 
count]

# 成员member的排名(从⼩到⼤)
ZRANK key member

# 成员member的排名(从⼤到⼩)
ZREVRANK key member

# 所有字典序在指定区间内的成员(从⼤到⼩)
ZRANGEBYLEX key min max [LIMIT offset count]

# 指定字典序区间内的所有member的数量
ZLEXCOUNT key min max
```



### 5、Hashes

```
# 增
# key中的域field的值设置为value
HSET key field value [field value ...]

# field不存在的时候，表key中的域field的值设置为value
 HSETNX key field value
 
 
 # 删
 # 删除key中的field
 HDEL key field [field ...]
 

 
 # 查
 # 查找key中给定域field的值
 HGET key field
 
 # 判断field是否存在于哈希表key中
 HEXISTS key field
 
 # key中field的数量
 HLEN key
 
 # 查找field中的value的字符串长度
  HSTRLEN key field
  
  #返回哈希表key中⼀个或多个给定域field的值
  HMGET key field [field ...]
  
  # 返回哈希表key中的所有域
  HKEYS key 
 
 # 返回哈希表key中所有域的值。
 HVALS key 
 
 # 返回哈希表key中所有的域和值
 HGETALL key
```



### 6、发布订阅

```
# 发送消息到指定频道
PUBLISH

# 订阅频道
SUBSCRIBE
```



### 7、Streams

```
# 增
# 追加⼀个新的消息
 XADD key [NOMKSTREAM] [MAXLEN|MINID [=|~] threshold [LIMIT 
count]] *|id field value [field value ...]

# 创建一个消费者组
 XGROUP CREATE key groupname id|$ [MKSTREAM] [ENTRIESREAD 
entries_read]

# 在消费者组groupname中创建⼀个消费者consumername
 XGROUP CREATECONSUMER key groupname consumername


# 删
# 从key中消除消息
XDEL key id [id ...]

# 从流的开头删除消息
 XTRIM key MAXLEN|MINID [=|~] threshold [LIMIT count]
 
 # 从消费者组groupname中删除⼀个消费者consumername。
  XGROUP DELCONSUMER key groupname consumername
  
# 删除⼀个消费者组groupname
XGROUP DESTROY key groupname











# 查
# key中消息的数量
XLEN key

# id位于start和end之间的消息，可用+ -返回所有消息
 XRANGE key start end [COUNT count]
 
 # 读取消息
 XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] 
id [id ...]

# 返回key的值
 XCLAIM key group consumer min-idle-time id [id ...]
 
 # 返回key的信息。
 XINFO STREAM key [FULL [COUNT count]]
 
 # 返回被消费者组成功接受收的消息数量
 XACK key group id [id ...]
 
 # 消费者组kgroupname中的消费者列表
 XINFO CONSUMERS key groupname
 
 # key的消费者组列表
 XINFO GROUPS key 
```



### 8、Geospatial

```
# 增
# 添加⼀个或者多个成员到地理空间中
 GEOADD key [NX|XX] [CH] longitude latitude member [longitude 
latitude member ...]




# 查
# 两个成员的距离
GEODIST key member1 member2 [M|KM|FT|MI]

# 搜索member并返回。可以按成员（FROMMEMBER）,或按经纬度
（FROMLONLAT）,范围可以是圆形（ BYRADIUS）或矩形（BYBOX）
GEOSEARCH key FROMMEMBER member|FROMLONLAT longitude latitude 
BYRADIUS radius M|KM|FT|MI|BYBOX width height M|KM|FT|MI 
[ASC|DESC] [COUNT count [ANY]] [WITHCOORD] [WITHDIST] 
[WITHHASH]

```



### 9、HyperLogLog

```
# 添加元素
PFADD key [element [element ...]]

# 查看基数
PFCOUNT key [key ...]

# 合并sourcekey至destkey
 PFMERGE destkey sourcekey [sourcekey ...]
```



### 10、位图

```
# 设置位图
SETBIT key offset value

# 读取位图的值
GETBIT key offset

# start到end为⽌的所有设置位（bit为1的位）的数量
 BITCOUNT key [start [end [BYTE|BIT]]]
 
 # 第⼀个bit位（0或者1）的位置
  BITPOS key bit [start [end [BYTE|BIT]]]
```



### 11、Bitfields

将很多小的整数存储到一个较大的位图中

```
# 增加
 BITFIELD key [GET encoding offset|[OVERFLOW WRAP|SAT|FAIL] 
  <SET encoding offset value|INCRBY encoding offset increment> 
  [GET encoding offset|[OVERFLOW WRAP|SAT|FAIL]  
  <SET en
  
  # 读取
  GET KEY:1
```



### 12、事务

```
# 创建消息队列
MULTI

# 执行命令
EXEC
```



### 13、持久化

#### RDB方式：指定间隔时间内将数据存入磁盘

```
# 修改配置文件以存储
vi redis.conf

# 手工存储
save

# 单独创建子进程将内存的数据写入磁盘
bgsave
```



#### AOF方式：执行写命令的同时，写入内存和追加的文件中。日志形式

```
# 修改配置文件以存储
vi redis.conf

appendonly yes
```



### 14、主从复制

### 15、哨兵模式























