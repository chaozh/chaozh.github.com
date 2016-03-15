title: Java架构师相关面试题
date: 2016-03-08 10:03:34
categories:
- 面试总结
tags:
- 事务
- 数据库
- 无锁并发
---

## java

1. executor 重要的三个参数含义：corePoolSize, maxmumPoolSize, [keepAliveTime], BlockingQueue<Runable>

2. 容器接口的区别：list collection hashmap


3. 多线程方法


4. 原子变量实现 设置unsafe 内存交换


5. 垃圾回收器 优缺点 

6. 四种导致full gc场景

	1. 持久代满了，gc不成功则out of memory
	2. 旧生代满了
	3. 新生代向S0和S1转移数据，S0和S1向旧生代转移数据，两边内存都设置较小，持续进行导致
	4. 直接system.gc

7. 数据库 乐观锁 悲观锁 出现场景，jdbc事务提交使用哪种锁（一般使用DB的悲观锁）

8. NIO与 大致设计 优缺点

## Spring

1. 常用模块
	aop, aspects, beans, core, context, expression, orm, jdbc, jms, tx, test, web, webmvc 
2. 主要接口

3. 五种事务传播性Propagation
	required, supports, mandatory, requires_new, not_supported, never, nested 

## netty

结构 lf缺陷

## Nginx

运用功能 模块实现 核心组件 upstream配置

## Redis

集群设置 集群算法

