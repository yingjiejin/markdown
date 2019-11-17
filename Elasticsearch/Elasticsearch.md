# 一、Elasticsearch概述

- 主要功能
  - 分布式搜索引擎
  - 大数据近实时分析引擎
- 产品特性
  - 高性能，和T+1说不
  - 容易使用/容易扩展

## （一）分布式架构

- 集群规模可以从单个扩展至数百个节点
- 高可用 & 水平扩展
  - 服务和数据两个维度
- 支持不同的节点类型
  - 支持Hot & Warm架构

## （二）多种方式集成接入

- 多种编程语言的类库
  - Java/.NET/Python/Ruby/PHP/Groovy/Perl
- RESTful API v.s Transport API
  - 9200 v.s 9300（建议使用RESTful API）
- JDBC & ODBC

## （三）主要功能

- 海量数据的分户式存储以及集群管理
  - 服务与数据的高可用，水平扩展
- 近实时搜索、性能卓越
  - 结构化/全文/地理位置/自动完成
- 海量数据的近实时分析
  - 聚合功能

## （四）Elastic Stack生态圈

![ElasticStack生态圈](F:\markdown\Elasticsearch\images\ElasticStack生态圈.png)

###  1.Logstash：数据处理管道

- 开源的服务器端数据处理管道，支持从不同来源采集数据，转换数据，并将数据发送到不同的存储库中
- Logstash诞生于2009年，最初用来做日志的采集处理
- Logstash创始人Jordan Sisel
- 2013年被Elasticsearch收购

#### （1）Logstash特性

- 实时解析和转换数据
  - 从IP地址破译出地理坐标
  - 将PII数据匿名化，完全排除敏感字段
- 可扩展
  - 200多个插件（日志/数据库/Arcsigh/Netflow）
- 可靠性安全性
  - Logstash会通过持久化队列来保证至少将运行中的事件送达一次
  - 数据传输加密
- 监控

### 2.Kibana：可视化分析利器

- Kibana名字的含义 = Kiwifruit + Banana
- 数据可视化工具，帮助用户解开对数据的任何疑问
- 基于Logstash的工具，2013年加入Elastic公司

### 3.BEATS：轻量的数据采集器

- go语言开发

### 4.X-Pack：商业化套件

- 6.3之前的版本，X-Pack以插件方式安装
- X-Pack开源之后，Elasticsearch & Kibana 支持OSS版和Basic两种版本
  - 部分X-Pack功能支持免费使用，6.8和7.1开始，Security功能免费
- OSS，Basic，黄金级，白金级
- http://www.elastic.co/cn/subscriptions

### 5.ELK客户及应用场景

- 应用场景
  - 网站搜索/垂直搜索/代码搜索
  - 日志管理与分析/安全指标监控/应用性能监控/WEB抓取舆情分析

### 6.日志管理

#### （1）Elasticsearch与数据库集成

![Elasticsearch与数据库集成](F:\markdown\Elasticsearch\images\Elasticsearch与数据库集成.png)

- 单独使用Elasticsearch存储
- 以下情况可考虑与数据库集成
  - 与现有系统的集成
  - 需考虑事务性
  - 数据更新频繁

### 7.指标分析/日志分析

![指标分析日志分析](F:\markdown\Elasticsearch\images\指标分析日志分析.png)

# 二、Elasticsearch安装

## （一）Elasticsearch安装与简单配置

### 1.Elasticsearch的文件目录结构

| 目录    | 配置文件          | 描述                                                      |
| ------- | ----------------- | --------------------------------------------------------- |
| bin     |                   | 脚本文件，包括启动elasticsearch，安装插件。运行统计数据等 |
| config  | elasticsearch.yml | 集群配置文件，user，role based相关配置                    |
| jdk     |                   | Java运行环境                                              |
| data    | path.data         | 数据文件                                                  |
| lib     |                   | Java类库                                                  |
| logs    | path.log          | 日志文件                                                  |
| modules |                   | 包含所有ES模块                                            |
| plugins |                   | 包含所有已安装插件                                        |

### 2.JVM配置

- 修改JVM - config/jvm.options
  - 7.1下载的默认设置是1GB
- 配置的建议
  - Xmx和Xms设置成一样
  - Xmx不要超过机器内存的50%
  - 不要超过30GB - https://www.elastic.co/blog/a-heap-of-trouble

### 3.Elasticsearch启动

进入到Elasticsearch的bin路径下后，输入elasticsearch

![Elasticsearch启动](F:\markdown\Elasticsearch\images\Elasticsearch启动.png)

启动完成后查看：http://localhost:9200/

![Elasticsearch启动成功](F:\markdown\Elasticsearch\images\Elasticsearch启动成功.png)

### 4.Elasticsearch安装组件

进入到Elasticsearch的bin路径下后，输入==elasticsearch-plugin list==查看已经安装的组件

![Elasticsearch查看已经安装组件](F:\markdown\Elasticsearch\images\Elasticsearch查看已经安装组件.png)

输入==elasticsearch-plugin install analysis-icu==下载安装组件

![Elasticsearch下载安装组件1](F:\markdown\Elasticsearch\images\Elasticsearch下载安装组件1.png)

输入http://localhost:9200/_cat/plugins查看已经安装的插件

![Elasticsearch下载安装组件1展示](F:\markdown\Elasticsearch\images\Elasticsearch下载安装组件1展示.png)

### 5.如何在开发机上运行多个Elasticsearch实例

- bin/elasticsearch -E node.name=node1 -E cluster.name=geektime -E path.data=node1_data -d
- bin/elasticsearch -E node.name=node2 -E cluster.name=geektime -E path.data=node2_data -d
- bin/elasticsearch -E node.name=node3 -E cluster.name=geektime -E path.data=node3_data -d
- 删除进程 ps|grep elasticsearch   kill pid

查看运行的节点：http://localhost:9200/_cat/nodes

![Elasticsearch查看运行的节点](F:\markdown\Elasticsearch\images\Elasticsearch查看运行的节点.png)

## （二）Kibana的安装与界面快速浏览

### 1.Kibana安装

1. 下载并解压缩Kibana
2. 在编辑器中打开config/kibana.yml，设置elasticsearch.url为指向您的Elasticsearch实例
3. 运行bin/kibana（或bin/kibana.bat在Windows上）
4. 将浏览器指向 http://localhost:5601

### 2.Kibana安装插件

- bin/kibana-plugin install plugin_location
- bin/kibana-plugin list
- bin/kibana remove

## （三）Logstash安装与导入数据

### 1. Logstash安装

1. 下载并解压Logstash
2. 准备logstash.conf配置文件
3. 跑bin/logstash -f logstash.conf