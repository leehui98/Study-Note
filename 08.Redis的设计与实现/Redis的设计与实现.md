redis默认采用LZF压缩，可以取消这个选项，就是不压缩

# 1.redis数据类型

## 1. String（字符串）

string是二进制安全的，可以包含任何数据，比如jpg图像、序列化的对象等。

string类型是redis最基本的类型，最大能存储512MB。

```redis
set key value
get key
```

## 2. Hash（哈希）

Redis hash是一个键值对集合。

Redis hash是一个string类型的filed和value的映射表，hash特别适用于存储对象。

```redis
hmset key filed1 value1 filed2 value2
hget key filed1 
```

每个hash可以存储2^32-1个键值对（40多亿），一个key相当于一个hash

## 3. List（列表）

redis列表是简单的字符串列表，按照插入顺序排列， 可以添加一个元素到列表的头部或者尾部

```redis
lpush key value
lrange key 0 10
```

可以存储40多亿个元素，一个key相当于一个list

## 4. Set（集合）

Redis的Set是String类型元素的无序集合，且不重复

集合是通过哈希表实现的，所以添加、删除、查找的复杂度都是O(1)

```redis
sadd key member 添加一个元素，成功返回1，失败返回0。失败的情况是重复插入
smembers key
sinter key [key...]
```

最大成员是是40多亿

## 5. ZSet（sorted set：有序集合）

Redis的ZSet是string类型元素的有序集合，且不重复

每个元素都会关联一个double类型的分数，redis正是通过这个分数实现为集合中的成员进行从小到大的排序。

ZSet的成员是唯一的，但是分数可以重复

```redis
zadd key score member
zrangebyscore key 0 1000
```

![](./image/数据类型及应用场景.png)

# 2.Redis命令

## 1. 客户端连接

```redis
本地连接：redis-cli
远程连接：redis-cli -h host -p port -a password
避免中文乱码：redis-cli --raw
```

## 2. 键

语法

```redis
COMMNND KEY_NAME
```

实例

```shell
SET NAME redis
DEL NAME
#成功返回1，失败返回0
```

Redis keys命令

```shell
DEL key #在key存在时删除key
DUMP key #序列化给定key，并返回被序列化的值
EXISTS key #检查key是否存在
EXPIRE key seconds #为key设置过期时间，时间以秒计
EXPIREAT key timestampj #EXPIREAT和EXPIRE类似，不同的是EXPIREAT命令接受的参数是UNIX时间戳
PEXPIRE key milliseconds #为key设置过期时间，时间以毫秒计
PEXPIREAT key milliseconds-timestamp #设置key过期时间的时间戳，以毫秒计
KEYS pattern #找出符合pattern模式的key
MOVE key db #将档期那数据库的key移动到给定数据库db中
PERSIST key #移除key的过期时间，key将保持到永久
PTTL key #以毫秒返回key的剩余过期时间
TTL key #以秒为单位返回key的剩余过期时间
RANDOMKEY #从当前数据库中随机返回一个key
RENAME key newkey #将key重命名为newkey
RENAMEX key newkey #如果newkey不存在时，将key的名称修改为newkey
SCAN cursor [MATCH pattern] [COUNT count] #迭代数据库中的数据库键
TYPE key #查看key所存储值的类型
```

## 3. 字符串

```shell
set key value
get key
getrange key start end
getset key value #将key的值设为value，并返回key的旧值
getbit key offset #对key所存储的字符串值，获取指定偏移量上的位，string以二进制的形式
MGET key1 [key2...] #获取多个key对应的字符串
setbit key offset values #设置某个位的值
setex key seconds value #将值value关联到key，并将key的过期时间设为seconds
setnx key valu #只有在key不存在时设置value
setrange key offset value #将value指定偏移量的值覆盖为value
strlen key #返回key所存储的字符串的长度
mset key value [key value] #同时设置多个键值对
msetnx key value [key value] #只有在所有key不存在的时候，是指多个键值对
psetex key milliseconds values # 与setex类似，这个时以毫秒为单位
incr key #将key中存储的数值加1，需要是整数
incrby key increment
incrbyfloat key increment #给key增加浮点数增加量
decr key #将key中存储的数值减1，需要是整数
decrby key decrement #将key的数值减去一个量
append key value #将value增加到key-value的末尾
```

