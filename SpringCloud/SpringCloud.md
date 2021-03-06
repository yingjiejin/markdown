#  一、微服务概述

## （一）什么是微服务

通常而言，微服务架构是一种架构模式或者说一种架构风格，==它提倡将单一应用程序划分成一组小的服务==，每个服务运行在其独立的自己的==进程中==，服务之间互相协调、互相配合，为用户提供最终价值。服务之间采用轻量级的通信机制互相沟通（通常是基于HTTP的RESTful API）。每个服务都围绕着具体业务进行构建，并且能够独立的部署到生产环境、类生产环境等。另外应尽量避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。



微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每一个微服务提供单个业务功能的服务，一个服务做一件事，从技术的角度看就是一种小而独立的处理过程，类似进程概念，能够自行单独启动或销毁，拥有自己独立的数据库。

## （二）微服务与微服务架构

- 微服务

  强调的是服务的大小，它关注的是某一个点，是具体解决某一问题/提供落地对应服务的一个服务应用，狭义的看，可以看做eclipse里面的一个个微服务工程/或者Module

- 微服务架构

  微服务架构是一种架构模式，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调、互相配合，为用户提供最终价值。每个服务运行在其==独立的进程中==，服务与服务间采用轻量级的通信机制互相协作（通常基于HTTP协议的RESTful API）。每个服务都围绕着具体业务进行构建，并且能够被独立的部署到生产环境、类生产环境等。另外，==应尽量避免统一的、集中式的服务管理机制==，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建。

## （三）微服务优缺点

- ==优点==
  - 每个服务足够内聚，足够小，代码容易理解这样能聚焦一个指定的业务功能或业务需求。
  - 开发简单、开发效率提高，一个服务可能就是专一的只干一件事。
  - 微服务能够被小团队单独开发，这个小团队是2到5人的开发人员组成。
  - 微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的。
  - 微服务能使用不同的语言开发。
  - 易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如Jenkins，Hudson，bamboo.
  - ==微服务只是业务逻辑代码，不会和HTML，CSS或其他界面组件混合。==
  - ==每个微服务都有自己的存储能力，可以有自己的数据库。也可以有统一的数据库。==

- ==缺点==
  - 开发人员要处理分布式系统的复杂性。
  - 多服务运维难度，随着服务的增加，运维的压力也在增大。
  - 系统部署依赖。
  - 服务间通信成本。
  - 数据一致性。
  - 系统集成测试。
  - 性能监控……

## （四）微服务技术栈有哪些

微服务技术栈：多种技术的集合体

| ==微服务条目==                           | ==落地技术==                                                 | ==备注== |
| ---------------------------------------- | ------------------------------------------------------------ | -------- |
| 服务开发                                 | SpringBoot、Spring、SpringMVC                                |          |
| 服务配置与管理                           | Netflix公司的Archaius、阿里的Diamond等                       |          |
| 服务注册与发现                           | Eureka、Consul、Zookeeper等                                  |          |
| 服务调用                                 | Rest、RPC、gRPC                                              |          |
| 服务熔断器                               | Hystrix、Envoy等                                             |          |
| 负载均衡                                 | Ribbon、Nginx等                                              |          |
| 服务接口调用（客户端调用服务的简化工具） | Feign等                                                      |          |
| 消息队列                                 | Kafka、RabbitMQ、ActiveMQ等                                  |          |
| 服务配置中心管理                         | SpringCloudConfig、Chef等                                    |          |
| 服务路由（API网关）                      | Zuul等                                                       |          |
| 服务监控                                 | Zabbix、Nagios、Metrics、Spectator等                         |          |
| 全链路追踪                               | Zipkin、Brave、Dapper等                                      |          |
| 服务部署                                 | Docker、OpenStack、Kubernetes等                              |          |
|                                          | SpringCloud Stream（封装与Redis、Rabbit、Kafka等发送接收消息） |          |
| 事件消息总线                             | Spring Cloud Bus                                             |          |

## （五）为什么选择SpringCloud作为微服务架构

1. 选型依据

   1. 整体解决方案和框架成熟度
   2. 社区热度
   3. 可维护性
   4. 学习曲线

2. 当前各大IT公司用的微服务架构有哪些？

   1. 阿里Dubbo/HSF
   2. 京东JSF
   3. 新浪微博Motan
   4. 当当网DubboX

3. 各微服务框架对比

   | 框架           | Netflix/SpringCloud                                          | Motan                                                        | gRPC                      | Thrift   | Dubbo/DubboX     |
   | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------- | -------- | ---------------- |
   | 功能定位       | 完整的微服务框架                                             | RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册/发现 | RPC框架                   | RPC框架  | 服务框架         |
   | 支持Rest       | 是 Ribbon支持多种可插拔的序列化选择                          | 否                                                           | 否                        | 否       | 否               |
   | 支持RPC        | 否                                                           | 是（Hession2）                                               | 是                        | 是       | 是               |
   | 支持多语言     | 是(Rest)形式?                                                | 否                                                           | 是                        | 是       | 否               |
   | 服务注册/发现  | 是（Eureka）Eureka服务注册表，Karyon服务端框架支持服务自注册和健康检查 | 是（zookeeper/consul）                                       | 否                        | 否       | 是               |
   | 负载均衡       | 是(服务端zuul+客户端Ribbon)Zuul-服务，动态路由 云端负载均衡Eureka（针对中间层服务器） | 是(客户端)                                                   | 否                        | 否       | 是(客户端)       |
   | 配置服务       | Netflix Archaius SpringCloud Config Server集中配置           | 是（zookeeper提供）                                          | 否                        | 否       | 否               |
   | 服务调用链监控 | 是(zuul) Zuul提供边缘服务，API网关                           | 否                                                           | 否                        | 否       | 否               |
   | 高可用/容错    | 是（服务端Hystrix+客户端Ribbon）                             | 是（客户端）                                                 | 否                        | 否       | 是（客户端）     |
   | 典型应用案例   | Netflix                                                      | Sina                                                         | Google                    | FaceBook |                  |
   | 社区活跃程度   | 高                                                           | 一般                                                         | 高                        | 一般     | 已经不维护了     |
   | 学习难度       | 中等                                                         | 低                                                           | 高                        | 高       | 低               |
   | 文档丰富度     | 高                                                           | 一般                                                         | 一般                      | 一般     | 高               |
   |                | SpringCloud Bus为我们的应用程序带来了更多管理端点            | 支持降级                                                     | Netflix内部在开发集成gRPC | IDL定义  | 实践的公司比较多 |

   

# 二、SpringCloud入门概述

## （一）SpringCloud是什么

#### 1.官网说明

SpringCloud，基于SpringBoot提供了一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于NetFilx的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。

SpringCloud利用SpringBoot的开发便利性巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式系统的一些工具，包括配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等，它们都可以用SpringBoot的开发风格做到一键启动和部署。

SpringBoot并没有重复造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的微服务框架组合起来，通过SpringBoot风格进行再封装屏蔽掉了复杂的配置和实现原理，==最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包==。

SpringCloud=分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。

#### 2.SpringCloud和SpringBoot是什么关系

SpringBoot专注于快速方便的开发单个个体微服务。

SpringCloud是关注全局的微服务协调治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等集成服务。

SpringBoot可以离开SpringCloud独立使用开发项目，==但是SpringCloud离不开SpringBoot==，属于依赖的关系。

==SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架。==



# 三、Eureka服务注册与发现

## （一）Eureka是什么

Eureka是Netflix的一个子模块，也是核心模块之一。Eureka是一个基于REST服务，用于定位服务，以实现云端中间层服务发现和故障转移。服务注册与发现对于微服务架构来说是非常重要的，有了服务发现与注册，==只需要使用服务的标识符，就可以访问到服务==，而不需要修改服务调用的配置文件了。==功能类似于dubbo的注册中心，比如Zookeeper。==

1. Eureka的基本架构

   > SpringCloud封装了Netflix公司开发的Eureka模块来实现服务注册和发现

   > Eureka采用了C-S的设计架构。Eureka Server作为服务注册功能的服务器，它是服务注册中心。

   > 而系统中的其他微服务，使用Eureka的客户端连接到Eureka Server并维持心跳连接，这样系统的维护人员就可以通过Eureka Server来监控系统中各个微服务是否正常运行。SpringCloud的一些其他模块（比如Zuul）就可以通过Eureka Server来发现系统中的其他微服务，并执行相关的逻辑。

   ==Eureka包含两个组件：Eureka Server 和 Eureka Client==

   - Eureka Server 提供服务注册服务

     > 各个节点启动后，会在Eureka Server 中进行注册，这样Eureka Server 中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

   - Eureka Client是一个java客户端

     >用于简化Eureka Server 的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除（默认90秒）

2. 三大角色

   - Eureka Server提供服务注册和发现
   - Service Provider服务提供方将自身服务注册到Eureka，从而使服务消费方能够找到
   - Service Consumer服务消费方从Eureka 获取注册服务列表，从而能够消费服务

## （二）服务注册中心建立

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka-server</artifactId>
</dependency>
```

```yaml
eureka:
	instance:
		hostname: localhost #eureka服务端的实例名称
	client:
		register-with-eureka: false #false表示不向注册中心注册自己
		fetch-registry: false #false表示自己就是注册中心，职责就是维护服务实例，不需要去检索服务
		service-url: 
			defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
			# 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
```

```java
@SpringBootApplication
@EnableEurekaServer //EurekaServer服务器端启动类，接收其它微服务注册进来
public class EurekaServerApp{
    public static void main(Sring[] args){
        SpringApplication.run(EurekaServerApp.class,args); 
    }
}
```



# 四、Ribbon负载均衡



