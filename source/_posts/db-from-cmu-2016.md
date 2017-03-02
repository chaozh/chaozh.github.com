title: CMU课程15-721数据库系统设计（第一周）
date: 2016-02-05 16:27:54
categories:
- 课程总结
tags:
- 数据库
- 隔离级别
- MVCC
---

CMU课程15-721最新一期开课啦！

真是要强烈推荐这门课程：有在线视频，有教案，有作业，甚至还有自动评分工具，除了没老师与TA之外真的分享了几乎所有的课程资源，完全可以让我们这种墙内的后进生们自学。

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

## Course Introduction and History of Database Systems

以回顾数据库历史的方式串讲了数据库系统设计中的难点，也就是以后的课程内容目录：

1. 并发控制
2. 索引
3. 存储模型，压缩
4. join算法
5. 日志与恢复
6. 查询优化
7. 新存储器件

未来所有作业与项目都是用C++11写的（牛逼！），准备好调试多线程
推荐每次上课前写一篇本次课阅读paper的心得，包括：主题思想（两句话）、系统使用与修改情况（一句话）、负载测试情况（一句话）

## In-Memory Databases

因为新型存储器件的飞速发展导致内存数据库（其实还有内存文件系统）需求越来越强烈，原始基于硬盘为主存储结构的数据库设计因此会有哪些变化呢？

总结下传统硬盘数据库设计特点为：

1. 使用内存做缓存池（涉及内存与磁盘上以“页”为单位的数据同步）
2. 页为单位的索引数据组织（页号、slot号、header、tuple、blob等概念设计）
3. 所有请求都会先经过缓存池（worker线程需要pin住该页防止被swap到磁盘）
4. 并发控制保证事务可以等待磁盘数据读取时间，同时锁（lock）要保存到其他数据结构（非页结构）中防止被swap
5. 使用WAL处理日志与恢复问题
6. CPU时间统计：30%缓存池、30%锁、28%日志、12%真正操作

设计内存数据库，首先不要考虑使用mmap，缺点：内存操作无法非阻塞了、内存结构与磁盘必须相同、数据库系统不知道哪些页实际在内存中（说白了就是操作系统通用设计不可能比数据库架构师专业设计更好！） 

内存数据库IO问题退居二线（DRAM 60ns， SSD 25000~30000ns），需要考虑其他性能问题：
1. lock/latch
2. cache-line miss
3. pointer chasing
4. predicate evaluations
5. data move
6. network

总结下设计特点为：

1. 不需要缓存池和页结构（直接用指针取代id索引？定长或变长数据？但是一定得使用checksum保证数据正确）
2. 获得锁时间跟获得数据时间一样！导致可能允许牺牲一点并发度（coarse-grained locking因为数据IO足够快）并且可以将锁信息与数据信息一起保存（增加CPU cache locality，mutex相对太慢，一定改用CAS）
3. 索引设计问题凸显（需要多考虑一下CPU cache）
4. 查询处理方式巨大改变（不需要顺序扫描，传统一次处理一个tuple方式相对太慢），考虑vector-at-a-time
5. 仍然需要WAL，可能可以简化；仍然需要checkpoint操作

内容基本出自92年的文章: [High-Performance Concurrency Control Mechanisms for Main-Memory Databases](http://15721.courses.cs.cmu.edu/spring2016/papers/p298-larson.pdf)

发布第一次项目作业，等本人做完了另找时间写文章说明。

## Concurrency Control I

介绍各种事务模型：nested trx、saga trx（其实在《事务处理》一书里面都已经见识过）

介绍各种并发控制方式：
Two-Phase Locking 和死锁避免方法

- DL_DETECT
- NO_WAIT
- WAIT_DIE

Timestamp Ordering及优化方法

- Basic T/O
- MVCC
- OCC
- H-STORE 

测试结果出自14年的文章: [Staring into the Abyss: An Evaluation of Concurrency Control with One Thousand Cores](http://15721.courses.cs.cmu.edu/spring2016/papers/p209-yu.pdf)

个人补充：MC涉及多线程访问内存值，CC解决多CPU副本一致性

[Memory Consistency和Cache Coherence](http://www.yebangyu.org/blog/2016/01/09/memoryconsistencyandcachecoherence/)

## Concurrency Control II — Multi-versioning

详细介绍隔离级别：比可串行化级别低一些的级别，包括REPEATABLE_READS, READ_COMMITTED 

- Dirty Read 脏读（提前读到后面修改的内容）
- Unrepeatable Read 不可重复读（事务里两次读操作，后一次读的不同）
- Phantom Read 幻读（读到其他事务插入或删除的数据）

产品方面voltDB完美支持可串行化，oracle11g最高支持SNAPSHOT_ISOLATION级别，新增两种级别：

- CURSOR_STABILITY 内部cursor持有锁，有时防止Lost Update（写操作覆盖顺序不对）

- SNAPSHOT_ISOLATION 保证事务所见snapshot相同（不能强化到可串行化级别 2009新paper有可串行化snapshot方案），防止Write Skew（部分写更新）

下面介绍多版本MVCC现代实现方法(timestamp-ordered好处在于不需要lock！)

mvcc设计考虑：

- version chains（oldest-to-newest[w]，newest-to-oldest[r]）
- version storage（全量或delta存[回滚段]）
- garbage collection（额外线程或合作，GC在重写负载造成15%额外负载]）

**Hekaton(SQL Server)**
  
  事务开始结束都赋予时间戳, 为每个tuple维护版本链(BEGING, END, POINTER)，维护全局事务状态图(ACTIVE, VALIDATING[precommited,2PL不需要这个状态，因为锁即可保证]),事务维护元数据(Read set,Write set,Scan set[避免幻读],Commit dependency)，乐观事务在多核表现下明显好于悲观事务
  
  实现方面仅使用lock-free数据结构(没有latch或spin lock，使用bw-tree和skiplist，CAS)
  
  该系统设计11年论文：[High-Performance Concurrency Control Mechanisms for Main-Memory Databases](http://15721.courses.cs.cmu.edu/spring2016/papers/p298-larson.pdf)

## Concurrency Control III — Optimistic

首先讨论存储过程，产生于解决使用交互API问题（可以减少网络通信次数与等待）
- ODBC/JDBC
- DBMS wire protocols

存储过程的问题：不好版本控制与调试，不好移植

Optimistic方法 其中validation步骤：（可以并发进行）

- backword 与已经提交的事务判断
- forword 与还未提交事务判断

本课未学完待补充