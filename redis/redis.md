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

![内存](F:\markdown\redis\images\内存.png)

![](F:\markdown\redis\images\内存2.png)

### 2.持久化（断电不丢数据）

Redis所有数据保持在内存中，对数据的更新将异步地保存到磁盘上。

### 3.多种数据结构

![多种数据结构](F:\markdown\redis\images\多种数据结构.png)

- BitMaps：位图
- HyperLogLog：超小内存唯一值计数
- GEO：地理信息定位