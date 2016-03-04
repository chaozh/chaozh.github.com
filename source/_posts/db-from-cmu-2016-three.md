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

##Query Execution & Scheduling

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

- Uniform：多cpu与多内存都必须走中心bus
- Non-Uniform：几个cpu之间组成bus，cpu与内存一一对应

数据分配策略：

- 内存分配与cpu绑定，参考linux的`move_pages`
- 使用malloc

##Join Alogrithms I — Hashing

不懂为何hash join算法作为第一个项目作业，而不是在介绍这个之后再布置

##Join Alogrithms II — Sort-Merge



##Logging & Recovery I — Physical Logging



##Logging & Recovery II — Alternative Methods

