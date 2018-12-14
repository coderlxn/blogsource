---
title: Redis基础
date: 2018-10-11 10:06:24
tags: Redis
---
#### Redis支持5种数据类型
* string 是最基本的数据类型，一个key对应一个value，是二进制安全的，可以存储图片或序列化的对象。最大能存储512MB
```redis
redis 127.0.0.1:6379> SET name "runoob"
OK
redis 127.0.0.1:6379> GET name
"runoob"
```
* hash 是一个键值对集合，适合存储对象。hmset设置元素，hget获取元素，hgetall获取所有元素。每个 hash 可以存储 232 -1 键值对（40多亿）
```redis
redis> HMSET myhash field1 "Hello" field2 "World"
"OK"
redis> HGET myhash field1
"Hello"
redis> HGET myhash field2
"World"
```
* list 是简单的字符串列表，按照插入顺序排序，可以在列表的头部或尾部插入元素。列表最多可存储 232 - 1 元素 (4294967295, 每个列表可存储40多亿)
```redis
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
```
* set 是string类型的无序集合，通过哈希表实现。sadd添加元素，smembers获取元素列表。集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)
```
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "redis"
2) "rabitmq"
3) "mongodb"
```
* zset 有序集合，和set类似，不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
zset的成员是唯一的,但分数(score)却可以重复。添加元素到集合，元素在集合中存在则更新对应score。
```
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> > ZRANGEBYSCORE runoob 0 1000
1) "mongodb"
2) "rabitmq"
3) "redis"
```