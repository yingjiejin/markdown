# 一、初识Redis

- 高性能的Key-Value服务器
- 多种数据结构
- 丰富的功能
- 高可用分布式支持

## （一）Redis的特性

- 速度快
- 持久化
- 多种数据结构
- 支持多种编程语言
- 功能丰富
- 简单
- 主从复制

### 1.速度快

10w OPS

数据是存储在内存当中，C语言实现（50000 line），单线程

![内存](F:\markdown\redis\images\初识redis\内存.png)

![](F:\markdown\redis\images\初识redis\内存2.png)

### 2.持久化（断电不丢数据）

Redis所有数据保持在内存中，对数据的更新将异步地保存到磁盘上。

### 3.多种数据结构

![多种数据结构](F:\markdown\redis\images\初识redis\多种数据结构.png)

- BitMaps：位图
- HyperLogLog：超小内存唯一值计数
- GEO：地理信息定位

### 4.支持多种客户端语言

- java
- php
- python
- ruby
- lua
- node

### 5.功能丰富

- 发布订阅
- Lua脚本
- 事务
- pipeline

### 6.简单

- 不依赖外部库（like libevent）
- 单线程模型

### 7.主从复制

![主从复制](F:\markdown\redis\images\初识redis\主从复制.png)

### 8.高可用、分布式

![高可用分布式](F:\markdown\redis\images\初识redis\高可用分布式.png)

## （二）Redis典型应用场景

- 缓存系统
- 计数器
- 消息队列系统
- 排行榜
- 社交网络
- 实时系统

### 1.缓存系统

![缓存系统](F:\markdown\redis\images\初识redis\缓存系统.png)

### 2.计数器

![计数器](F:\markdown\redis\images\初识redis\计数器.png)

### 3.消息队列系统

![消息队列系统](F:\markdown\redis\images\初识redis\消息队列系统.png)

### 4.排行榜

![排行榜](F:\markdown\redis\images\初识redis\排行榜.png)

### 5.社交网络

![社交网络](F:\markdown\redis\images\初识redis\社交网络.png)

## （三）Redis的三种启动方式介绍

### 1.Linux安装

- wget http://download.redis.io/releases/redis-3.0.7.tar.gz
- tar -xzf redis-3.0.7.tar.gz
- ln -s redis-3.0.7 redis
- cd redis
- make && make install

### 2.Redis可执行文件说明

| 文件             | 说明                    |
| ---------------- | ----------------------- |
| redis-server     | Redis服务器             |
| redis-cli        | Redis命令行客户端       |
| redis-benchmark  | Redis性能测试工具       |
| redis-check-aof  | AOF文件修复工具         |
| redis-check-dump | RDB文件检查工具         |
| redis-sentinel   | Sentinel服务器(2.8以后) |

### 3.三种启动方法

- 最简启动
- 动态参数启动
- 配置文件启动

#### （1）最简启动

- redis-server

> 验证
>
> ps -ef | grep redis
>
> netstat -antpl | grep redis
>
> redis-cli -h ip -p port ping

#### （2）动态参数启动

- redis-server --port 6380

#### （3）配置文件启动

- redis-server configPath

#### （4）三种启动方式比较

- 生产环境选择配置启动
- 单机多实例配置文件可以用端口区分开

### 4.Redis客户端连接

![redis连接](F:\markdown\redis\images\初识redis\redis连接.png)

### 5.Redis客户端返回值

![redis返回值1](F:\markdown\redis\images\初识redis\redis返回值1.png)

![redis返回值2](F:\markdown\redis\images\初识redis\redis返回值2.png)

## （四）Redis常用配置

| 配置项    | 说明                    |
| --------- | ----------------------- |
| daemonize | 是否是守护进程(no\|yes) |
| port      | Redis对外端口号         |
| logfile   | Redis系统日志           |
| dir       | Redis工作目录           |
| 6379      | 默认端口                |

# 二、Redis API的使用和理解

## （一）通用命令

| 命令               | 说明               |
| ------------------ | ------------------ |
| keys               | redis中所有的键    |
| dbsize             | redis数据库的大小  |
| exists key         | 判断key是否存在    |
| del key [key...]   | 删除key            |
| expire key seconds | 设置key的有效时间  |
| type key           | 查看 key的数据类型 |

#### 1.keys

![keys(1)](F:\markdown\redis\images\RedisAPI\keys(1).png)

![keys(2)](F:\markdown\redis\images\RedisAPI\keys(2).png)

> ==keys命令一般不在生产环境使用==

> keys*怎么用
>
> - 热备从节点
> - scan

#### 2.dbsize

![dbsize](F:\markdown\redis\images\RedisAPI\dbsize.png)

#### 3.exists

![exists](F:\markdown\redis\images\RedisAPI\exists.png)

#### 4.del

![del](F:\markdown\redis\images\RedisAPI\del.png)

#### 5.expire、ttl、persist

![expire(1)](F:\markdown\redis\images\RedisAPI\expire(1).png)

![expire(2)](F:\markdown\redis\images\RedisAPI\expire(2).png)

![expire(3)](F:\markdown\redis\images\RedisAPI\expire(3).png)

#### 6.type

![type](F:\markdown\redis\images\RedisAPI\type.png)

#### 7.时间复杂度

| 命令   | 时间复杂度 |
| ------ | ---------- |
| keys   | O(n)       |
| dbsize | O(1)       |
| del    | O(1)       |
| exists | O(1)       |
| expire | O(1)       |
| type   | O(1)       |

## （二）数据结构与内部编码

![数据结构与内部编码](F:\markdown\redis\images\RedisAPI\数据结构与内部编码.png)

- RedisObject

![redisObject](F:\markdown\redis\images\RedisAPI\redisObject.png)

## （三）单线程

![Redis单线程1](F:\markdown\redis\images\RedisAPI\Redis单线程1.png)

> 单线程为什么这么快？
>
> 1. 纯内存
> 2. 非阻塞IO
> 3. 避免线程切换和竞态消耗 

![单线程模型](F:\markdown\redis\images\RedisAPI\单线程模型.png)

## （四）字符串

### 1.字符串键值结构

![字符串键值结构](F:\markdown\redis\images\RedisAPI\字符串键值结构.png)

### 2.使用场景

- 缓存
- 计数器
- 分布式锁

### 3.get、set、del

![get、set、del-1](F:\markdown\redis\images\RedisAPI\get、set、del-1.png)

![get、set、del-2](F:\markdown\redis\images\RedisAPI\get、set、del-2.png)

### 4.incr、decr、incrby、decrby

![incr、decr、incrby、decrby-1](F:\markdown\redis\images\RedisAPI\incr、decr、incrby、decrby-1.png)

![incr、decr、incrby、decrby-2](F:\markdown\redis\images\RedisAPI\incr、decr、incrby、decrby-2.png)

### 5.实战

#### （1）查找视频

```java
public VideoInfo get(long id){
    String redisKey = redisPrefix + id;
    VideoInfo videoInfo = redis.get(redisKey);
    if(videoInfo == null){
        videoInfo = mysql.get(id);
        if(videoInfo != null){
            //序列化
            redis.set(redisKey,serialize(videoInfo));
        }
    }
    return videoInfo;
}
```

#### （2）分布式id生成器

![redis分布式id生成器](F:\markdown\redis\images\RedisAPI\redis分布式id生成器.png)

### 6. set、setnx、setxx

![set、setnx、setxx-1](F:\markdown\redis\images\RedisAPI\set、setnx、setxx-1.png)

![set、setnx、setxx-2](F:\markdown\redis\images\RedisAPI\set、setnx、setxx-2.png)

### 7.mget、mset

![mget、mset-1](F:\markdown\redis\images\RedisAPI\mget、mset-1.png)

![mget、mset-2](F:\markdown\redis\images\RedisAPI\mget、mset-2.png)

### 8.n次get 和 1次mget

![n次get](F:\markdown\redis\images\RedisAPI\n次get.png)

![1次mget](F:\markdown\redis\images\RedisAPI\1次mget.png)

### 9.getset、append、strlen

![getset、append、strlen-1](F:\markdown\redis\images\RedisAPI\getset、append、strlen-1.png)

![getset、append、strlen-2](F:\markdown\redis\images\RedisAPI\getset、append、strlen-2.png)

### 10.incrbyfloat、getrange、setrange

![incrbyfloat、getrange、setrange-1](F:\markdown\redis\images\RedisAPI\incrbyfloat、getrange、setrange-1.png)

 ![incrbyfloat、getrange、setrange-2](F:\markdown\redis\images\RedisAPI\incrbyfloat、getrange、setrange-2.png)

### 11.字符串总结

| 命令          | 含义                         | 复杂度（服务端开销） |
| ------------- | ---------------------------- | -------------------- |
| set key value | 设置key-value                | o(1)                 |
| get key       | 获取key-value                | o(1)                 |
| del key       | 删除key-value                | o(1)                 |
| setnx setxx   | 根据key是否存在设置key-value | o(1)                 |
| incr decr     | 计数                         | o(1)                 |
| mget mset     | 批量操作key-value            | o(n)                 |

## （五）hash

### 1.哈希键值结构

![哈希键值结构](F:\markdown\redis\images\RedisAPI\哈希键值结构.png)

 ### 2.hget、hset、hdel

![hget、hset、hdel-1](F:\markdown\redis\images\RedisAPI\hget、hset、hdel-1.png)

![hget、hset、hdel-2](F:\markdown\redis\images\RedisAPI\hget、hset、hdel-2.png)

### 3.hexists、hlen

![hexists、hlen-1](F:\markdown\redis\images\RedisAPI\hexists、hlen-1.png)

![hexists、hlen-2](F:\markdown\redis\images\RedisAPI\hexists、hlen-2.png)

### 4.hmget、hmset

![hmget、hmset-1](F:\markdown\redis\images\RedisAPI\hmget、hmset-1.png)

![hmget、hmset-2](F:\markdown\redis\images\RedisAPI\hmget、hmset-2.png)

### 5.实战

#### （1）记录网站每个用户个人主页的访问量

```
hincrby user:1:info pageview count
```

#### （2）缓存视频的基本信息（数据源在mysql中）伪代码

```java
public VideoInfo get(long id){
    String redisKey = redisPrefix + id;
    Map<String,String> hashMap = redis.hgetAll(redisKey);
    VideoInfo videoInfo = transferMapToVideo(hashMap);
    if(videoInfo == null){
        videoInfo = mysql.get(id);
        if(videoInfo != null){
            redis.hmset(redisKey,transferVideoToMap(videoInfo))
        }
    }
    return videoInfo;
}
```

### 6.hgetAll、hvals、hkeys

![hgetAll、hvals、hkeys-1](F:\markdown\redis\images\RedisAPI\hgetAll、hvals、hkeys-1.png)

![hgetAll、hvals、hkeys-2](F:\markdown\redis\images\RedisAPI\hgetAll、hvals、hkeys-2.png)