---
layout:     post
title:      "ZooKeeper学习"
date:       2017-12-28 13:44:05
author:     "Pumay"
header-img: "img/star.png"
catalog: true
tags:
    - Big data and analytics
    
---


# 一. 什么是ZooKeeper？

动物园管理员，就相当于是把集群中的客户端当作许多小动物，管理员的作用就是维持他们的秩序，提供他们食物，当某小动物出现状况，如生病、脱毛等一系列现象，管理员自然会知道，并传达给其他动物这个消息，其他动物就不会再拉着这个小动物玩，会让它好好休息。

一个开源的分布式应用协调系统，用来完成统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等工作。

## 1.什么是分布式应用程序

分布式应用程序可以通过在它们之间协调以完成特定的任务，快速且有效的方式在多个系统中的网络在给定时间（同时）运行。
分布式应用程序有两部分，分别是：服务器和客户端应用程序。

## 2.ZooKeeper举栗子

Zookeeper一个最常用的使用场景就是用于担任服务生产者和服务消费者的注册中心，服务生产者将自己提供的**服务注册**到Zookeeper中心，服务的消费者在进行服务调用的时候先到Zookeeper中**查找服务**，获取到服务生产者的详细信息之后，再去**调用服务**生产者的内容与数据，简单示例图如下：

![](/img/in-post/2017-12-28-ZooKeeper/zookeeper_scene.png)


# 二. 设计目标

ZooKeeper封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

它允许分布式进程通过共享的层次结构命名空间进行相互协调，这与标准文件系统类似。

名称空间由ZooKeeper中的数据寄存器组成-称为znode，这些类似于文件和目录。与为存储设计的典型文件系统不同，ZooKeeper数据保存在内存中，这意味着ZooKeeper可以实现高吞吐量和低延迟。

Zookeeper层次结构命名空间示意图如下：

![](https://pic4.zhimg.com/50/v2-26df9b73e9b2248fbe2d287904928f41_hd.jpg)

通过这种树图结构的数据模型，很容易的查找到具体的某一个服务。

# 三. 基本操作

    创建节点
    读节点数据
    更新节点数据
    删除节点
    监控节点变化

# 四. 部署

 1. 如果尚未安装 JDK，请下载安装它（参阅 参考资料）。这是必需的，因为 ZooKeeper 服务器在 JVM 上运行。
 2. 下载 ZooKeeper 3.4.5. tar.gz tarball 并将它解压缩到适当的位置。
 ```
 wget http://www.bizdirusa.com/mirrors/apache/ZooKeeper/stable/zookeeper3.4.5.
 tar.gz tar xzvf zookeeper3.4.5.tar.gz
 ```
 3. 创建一个目录，用它来存储与ZooKeeper服务器有关联的一些状态：mkdir /var/lib/zookeeper。您可能需要将这个目录创建为根目录，并在以后将这个目录的所有者更改为您希望运行ZooKeeper服务器的用户。

 4. 使用：cp zoo_sample.cfg zoo.cfg 命令，复制一份为zoo.cfg文件，这是因为Zookeeper再启动的时候默认使用的是zoo.cfg这个配置文件。
 
 5. 编辑 zookeeper3.4.5/conf/zoo.cfg 文件，使其如下：
 ```
 #zookeeper服务心跳检测时间，单位ms
 tickTime=2000 
 dataDir=/var/lib/zookeeper 
 clientPort=2181
 #投票选取新leader的初始化时间
 initLimit=5 
 syncLimit=2
 server.1=zkserver1.mybiz.com:2888:3888
 server.2=zkserver2.mybiz.com:2888:3888
 server.3=zkserver3.mybiz.com:2888:3888
 ```
 所有三个机器都应该打开端口 2181、2888 和 3888。在本例中，端口 2181 由 ZooKeeper 客户端使用，用于连接到 ZooKeeper 服务器；端口 2888 由对等 ZooKeeper 服务器使用，用于互相通信；而端口 3888 用于领导者选举。您可以选择自己喜欢的任何端口。通常建议在所有 ZooKeeper 服务器上使用相同的端口。
 
 6.	启动ZooKeeper服务器
 ``` 
 cd bin
 ./zkServer.sh start
 ```
 7.	启动 CLI
 ```
 ./zkCli.sh
 ```
 8.	停止ZooKeeper服务器
 ``` 
 ./zkServer.sh stop
 ``` 
 ZooKeeper 提供了Java™、C、Python和其他绑定。您可以通过这些绑定调用客户端API，将Java、C或Python应用程序转换为ZooKeeper客户端。
