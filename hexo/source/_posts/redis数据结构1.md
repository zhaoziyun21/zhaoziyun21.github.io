---
title: redis数据结构
date: 2018-01-22 22:58:44
tags: [redis]
category: [缓存]
---
***Redis支持五种数据类型***
- ***string***
 
        string类型是Redis最基本的数据类型，一个键最大能存储512MB。
        redis 127.0.0.1:6379> SET name "runoob"
        OK
        redis 127.0.0.1:6379> GET name
        "runoob"
- ***hash***
      
      Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。
      redis> HMSET myhash field1 "Hello" field2 "World"
      redis> HGET myhash field1
      "Hello"
- ***list***

        Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
        redis 127.0.0.1:6379> lpush runoob redis
        (integer) 1
        redis 127.0.0.1:6379> lpush runoob mongodb
        (integer) 2
        redis 127.0.0.1:6379> lrange runoob 0 10
        1) "rabitmq"
        2) "mongodb"

- ***set***

       Redis的Set是string类型的无序集合。
       集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
       redis 127.0.0.1:6379> sadd runoob redis
       (integer) 1
       redis 127.0.0.1:6379> sadd runoob mongodb
       (integer) 1
       redis 127.0.0.1:6379> sadd runoob rabitmq
       (integer) 1
       redis 127.0.0.1:6379> sadd runoob rabitmq
       (integer) 0
       redis 127.0.0.1:6379> smembers runoob
    
       1) "rabitmq"
       2) "mongodb"
       3) "redis"
       
- ***zset***
    
       Redis zset 和 set 一样也是string类型元素的集合,且不允许重复 的成员。
       不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进 行从小到大的排序。
       zset的成员是唯一的,但分数(score)却可以重复。
       redis 127.0.0.1:6379> zadd runoob 0 redis
       (integer) 1
       redis 127.0.0.1:6379> zadd runoob 0 mongodb
       (integer) 1
       redis 127.0.0.1:6379> zadd runoob 0 rabitmq
       (integer) 1
       redis 127.0.0.1:6379> zadd runoob 0 rabitmq
       (integer) 0
       redis 127.0.0.1:6379> ZRANGEBYSCORE runoob 0 1000

       1) "redis"
       2) "mongodb"
       3) "rabitmq"