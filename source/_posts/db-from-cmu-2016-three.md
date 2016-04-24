title: CMU课程15-721数据库系统设计（第三周）
date: 2016-03-01 19:15:00
categories:
- 系统经验
tags:
- 数据库
- 调度
- join
- 日志
---
CMU课程15-721最新一期开课啦！

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

索引技术课程（三节）介绍的技术实际跟并发控制结合很紧密，接下来讨论查询相关技术（穿插了日志技术内容）

## Query Execution & Scheduling

重点介绍查询执行与调度，特别是多用户DB中查询如何并行执行

处理进程模型：

- 进程-woker 依赖OS调度，使用共享内存，例：DB2、Postgres、Oracle；还是因为当时没有linux设计
- 进程池 CPU缓存支持不好
- 线程-worker DB管控调度，新DB都用这个模型规避共享内存

利用多核并发加速执行：

- Intra-Operator 在子集上独立运行相同函数（mapreduce）
- Inter-Operator 运行类似流水线，做一部分工作（stream）

个人补充：MySQL仍然是一个线程完成一个work

补充一点内存访问架构（实际是体系结构中重要部分，由硬件设计决定）

- UMA：多cpu与多内存都必须走中心bus
- NUMA：几个cpu之间组成bus，cpu与内存一一对应(目前常用架构！)

数据分配策略：

- 内存分配与cpu绑定，参考linux的`move_pages`
- 使用malloc

将查询语句变为可调度的执行计划，OLTP简单，OLAP比较难。Hyper就是一个NUMA架构下多核横向分布动态调度架构：

[Morsel-Driven Parallelism: A NUMA-Aware Query Evaluation Framework for the Many-Core Age](http://15721.courses.cs.cmu.edu/spring2016/papers/p743-leis.pdf)

每个核分配一个worker（并行），pull接收分配的任务，数据分布采用RR算法

## Join Alogrithms I — Hashing

原先不懂为何hash join算法作为第一个项目作业，而不是在介绍这个之后再布置。后来发现实际上这里讲解的是最新研究的多核版本算法

## Join Alogrithms II — Sort-Merge



## Logging & Recovery I — Physical Logging

传统DB磁盘日志基于ARIES算法，使用fuzzy cp规则，管理Active Transan Table与Dirty Page Table，赋予LSN并在易失与非易失中都要维护，过程为：analysis,redo,undo。最慢的过程发生在其他事务等待log刷到磁盘。

mem DB就不需要维护Dirty Page Table，因为所有数据都在内存。undo记录也没必要存储。

实例是**Silo**系统，其将log，cp，恢复操作都并行化优化，[论文](http://15721.courses.cs.cmu.edu/spring2016/papers/zheng-osdi14.pdf)将这些设计考虑与最终选择都讲解的非常清晰。

该系统假设每个CPU socket对应有一个存储设备。每个设备分配一个日志线程，CPU socket对应一组工作线程。日志线程每10个epoch创建一个文件，将旧日志文件重命名并记录最大epoch。日志维护两个队列：Free Buffers与Flushing Buffers。会有一个特殊日志线程跟踪最新已经持久化的epoch(即各个日志线程中持久化的epoch最大值)，小于此epoch的事务信息才可以丢弃。

每个disk对应一个cp线程，该线程有可能因为分区而写多个文件（voltDB甚至直接使用MVCC的version来做cp），cp的频率。恢复也可以并行进行。使用YCSB和TPC-C测试，吞吐率损失10%，lantency没有统计。

## Logging & Recovery II — Alternative Methods

讨论一下逻辑日志，内存checkpoint以及Facebook的快速恢复方法（共享内存恢复）。

OLTP中节点失效很少见(数据不大，通常<10个节点)，因此比较适合使用逻辑日志，记录格式通常包括（存储方法名，输入参数，额外安全检测信息），注意逻辑日志协议设计要满足可确定性，因此需要：

- 事务执行前顺序已经固定
- 事务逻辑是确定的

voltDB设计(日志类似MySQL binLog)



