title: Geode数据管理平台
date: 2015-11-10 21:23:08
tags: [geode]
---
# 概述

Geode是一个提供实时、一致访问大型分布式云平台下数据密集型应用的数据管理平台。最近开源啦！

Geode 通过跨多进程，把内存、CPU、网络资源和可选的本地磁盘汇集起来，来管理应用程序对象及其行为。它使用动态复制和数据分片技术，来实现高可用性，改善性能、可伸缩性和容错。Geode 除了是一个分布式数据容器，它还是一个内存数据管理系统，提供了可靠的异步事件通知和有保证的消息传递。

Geode 是一个非常成熟、健壮的产品，它的前身可以追溯到第一个由 Smalltalk 构造的对象数据库：GeoStone。作为一个事务性、低延迟的数据引擎，多个华尔街交易平台首次将 Geode（称为GemFire™）部署在金融部门。如今已有超过 600 家企业用户将 Geode 用于大规模、7*24 业务核心应用程序中。其中一个应用案例就是中国国家铁路将 Geode 用于整个国家的铁路票务系统，10 个节点集群管理着 2TB 的内存热数据，以及 10 个备份节点作为高可用性和弹性扩展。



# 主要概念和组件

在 Geode 分布式系统中，“缓存”是用来形容一个节点的抽象概念。

在每个缓存中，由您定义数据区域。数据区域（Data region）类似于关系型数据库的表，并且作为“name/value 对”以分布式方式管理数据。复制区域（replicated region）存储着 {分布式系统中每个缓存成员数据的} 相同副本。分区区域（partitioned region）在缓存成员之间传播数据。系统配置之后，客户端应用 {在不了解底层系统架构的情况下} 也可访问区域中的分布式数据。当数据发生改变的时候，您可以定义监听器来接收通知，并且您也可以定义过期条件，来删除区域中的过期数据。

定位器（Locator）提供发现服务和负载均衡服务。您可通过定位器服务列表来配置客户端，定位器维护着一个动态成员服务器列表。默认情况下，Geode 客户端和服务器使用40404端口，以及通过使用多播互相通信。



Geode 包含以下特性：

* 结合冗余、复制和“非共享”的持久化架构，来实现故障安全可靠性（译者解释：高可用，防止单点故障）和性能。

* 可水平扩展至成千上万个缓存成员，并结合多个缓存拓扑来满足不同的企业需求。该缓存可以分布在多个计算机中。

* 异步和同步缓存更新传播（propagation）。

* Delta 仅在一个对象（delta）新旧不同版本之间传播，而不是整个对象，从而极大降低了分发成本。

* 可靠的异步事件通知，优化后的、低延迟的分布层保证了消息传递。

* 无需额外的硬件，应用程序即可提速 4~40 倍。

* 数据敏感和实时业务智能。如果在您检索时数据发生更改，您能立即看到数据的变化。

* 与 Spring 框架集成，以加速并简化可扩展的事务型企业应用程序的开发。

* 支持 JTA 事务。

* 整个集群范围的配置，可以持久化，并可导出到其他集群。

* 通过 HTTP 即可实现对集群远程管理。

* 为 REST 应用程序开发提供 REST API 支持。

* 在主版本发布之间滚动升级。

# Geode 5分钟入门

从 Pivotal 获取源文件，从源文件中提取并构建（注：目前 Geode 支持 jdk1.7.75）：

```
$ cd geode
$ ./gradlew build installDist
```

启动定位器和服务器：

```
$ cd gemfire-assembly/build/install/geode
$ ./bin/gfsh
gfsh> start locator --name=locator
gfsh> start server --name=server
```

创建一个区域：

```
gfsh> create region --name=region --type=REPLICATE
```


编写一个客户端应用程序：

HelloWorld.java

```
import java.util.Map;
import com.gemstone.gemfire.cache.Region;
import com.gemstone.gemfire.cache.client.*;
public class HelloWorld {
public static void main(String[] args) throws Exception {
ClientCache cache = new ClientCacheFactory()
.addPoolLocator("localhost", 10334)
.create();
Region<String, String> region = cache
.<String, String>createClientRegionFactory(ClientRegionShortcut.CACHING_PROXY)
.create("region");
region.put("1", "Hello");
region.put("2", "World");
for (Map.Entry<String, String> entry : region.entrySet()) {
System.out.format("key = %s, value = %sn", entry.getKey(), entry.getValue());
}
cache.close();
}
}
```

编译并运行 HelloWorld.java。classpath 中应包含 gemfire-core-dependencies.jar 包：

```
javac -cp /some/path/geode/gemfire-assembly/build/install/geode/lib/gemfire-core-dependencies.jar HelloWorld.java
java -cp .:/some/path/geode/gemfire-assembly/build/install/geode/lib/gemfire-core-dependencies.jar HelloWorld
```

应用程序开发

Geode 应用程序可以用很多种客户端技术实现：

Java 通过使用 Geode 客户端 API，或者嵌入式使用 Geode API

Spring Data GemFire或 Spring Cache

Python（https://github.com/gemfire/py-gemfire-rest）

REST

memcached
英文出处：Geode
译文出处：伯乐在线-Martin
译文链接：http://blog.jobbole.com/87810/