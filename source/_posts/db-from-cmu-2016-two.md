title: CMU课程15-721数据库系统设计（第二周）
date: 2016-02-13 09:14:57
categories:
- 系统经验
tags:
- 数据库
- 锁
---
CMU课程15-721最新一期开课啦！

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

并发控制课程（一共三节）已经结束，但具体细节需要结合索引技术（仍然是三节）进行讲解

## Indexes I — Locking & Latching

本节就是对应用在B树索引上的各种锁和latch技术进行讲解

技术总结信息来自这篇文章：[A Survey of B-Tree Locking Techniques](http://15721.courses.cs.cmu.edu/spring2016/papers/a16-graefe.pdf)

在讲解锁相关技术前，需要把B树家族的各个数据结构先总结说明一下。

B树(键值都在) -> B+树(值都在叶子节点) 其中B+树设计选择有：

- Non-Unique索引 实现方法有1.允许键重复 2.值list
- 变长键 实现方法有1.指针 2.节点长度可变 3.key map（即slot pages）

其他变形：

- T-tree, 基于AVL树（不存key直接存指针），用于内存DB
- Skip List, MemSQL完全使用以替代B+结构，实现上为一组多层list，最低层为排序单链key，每层key减半，插入时几率插入每一层；优势是省空间，无需额外平衡操作，易并发；缺点是反向查找复杂
- Radix Tree 基树
- MassTree
- Fractal Tree

对索引加锁需要注意索引结构可能变化

- lock: 事务粒度上保护索引逻辑，txn duration，可以回滚，(ex,sh,up,i)
- latch: 线程粒度上保护索引内部实现，op duration，不需要回滚，(r,w)

因此lock-free index可以为no lock(MVCC)或no latch(cow,shadow page)

索引锁类型

- predicate lock 在where条件上加锁，不好实现
- key-value lock 在B树key上加锁
- gap lock 在B树key后面gap上加锁
- key-range lock 在B树key及后面gap上加锁
- hierachical lock 一组key-range lock纵向上组合(IX, EX)

## Indexes II — OLTP

latch实现方法: 

- Compare&Swap CAS操作，使用`__sync_bool_compare_and_swap(&M, 20, 30)` 是其他实现的基础
- Os Mutex 使用`pthread_mutex_t` (,about 25ns invocation)
- Test&Set spinlock 使用`std::atomic_flag`(non-scalable, cpu缓存不友好)
- Queue spinlock 使用`std::atomic_flag`(比mutex性能好，cpu缓存友好) lock chain
- rw locks 可以使用spin lock实现

其中non-scalable指锁阻塞后性能不能线性增长
[详细解释](https://pdos.csail.mit.edu/6.828/2009/lec/l-mcs.html)

个人补充：[spinlock vs mutex](http://www.yebangyu.org/blog/2016/01/24/spinlock-and-mutex/)

现代OLTP索引设计：

1. BW树(Hekaton)

- latch free B+树
- deltas 没有直接修改(使用额外record或info)，建少cpu缓存失效，使用epoch方便垃圾回收
- mapping table CAS使用在page的物理位置信息上 

2. Concurrent skip list(MemSQL), latch free
仅使用CAS操作实现insert与update无锁化

3. Adaptive基树(Hyper)

## Indexes III — OLAP

OLAP的不同在于可以drill down、roll up、slice、pivot等维度变化操作

Star Schema & Snowflake Schema => 采用行数据库模式





