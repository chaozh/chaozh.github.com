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

本节就是对应用在传统B树索引上的各种锁和latch技术进行讲解

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

本节关注latch实现方法: 

- Compare&Swap CAS操作，使用`__sync_bool_compare_and_swap(&M, 20, 30)` 是其他实现的基础
- Os Mutex 使用`pthread_mutex_t` (,about 25ns invocation)
- Test&Set spinlock 使用`std::atomic_flag`(non-scalable, cpu缓存不友好)
- Queue spinlock 使用`std::atomic_flag`(比mutex性能好，cpu缓存友好) lock chain
- rw locks 可以使用spin lock实现

其中non-scalable指锁阻塞后性能不能线性增长
[详细解释](https://pdos.csail.mit.edu/6.828/2009/lec/l-mcs.html)

个人补充：工程实践中共享对象的读写一致性方案(读写锁，COW与引用计数)，引用计数并发可以使用HazardPointer解决
个人补充：[spinlock vs mutex](http://www.yebangyu.org/blog/2016/01/24/spinlock-and-mutex/)

重点讲解几个典型mem DB中OLTP索引设计：

1. BW树(Hekaton)

- latch free B+树
- deltas 没有直接修改(使用额外record或info，物理page上挂链表)，建少cpu缓存失效，使用epoch方便垃圾回收
- mapping table 树所有都使用pid，需要维护pid到page物理位置的指针表，CAS使用在该指针上 

该技术来自微软Hekaton的这篇文章：[The Bw-Tree: A B-tree for New Hardware](http://15721.courses.cs.cmu.edu/spring2016/papers/bwtree-icde2013.pdf)

2. Concurrent skip list(MemSQL), latch free
仅使用CAS操作实现insert与update无锁化

该技术来自这篇文章：[Concurrent Maintenance of Skip Lists](http://15721.courses.cs.cmu.edu/spring2016/papers/pugh-skiplists1990.pdf)

3. Adaptive基树(Hyper)

## Indexes III — OLAP

OLAP的不同在于可以drill down、roll up、slice、pivot等维度变化操作

本节主要讲解行索引设计与bitmap

Star Schema & Snowflake Schema => 采用行数据库模式


## Storage Models & Data Layout

主要mem DB的数据存储模型，注意C++使用`reinterpret_cast`在编译期强制转换类型，需要注意缓存对齐中的按字对齐(e.g. 64-bit word)

Null处理：定义特殊值, Bitmap表示, Null Flag

存储模型分为：
- N-ary Storage Model(NSM) 适合OLTP，insert-heavy，物理上使用堆组织或索引组织元组(比如使用聚族索引)
- Decomposition Storage Model(DSM) 行存储又叫纵向扩展，适合OLAP,元组物理上使用固定长度或嵌入元组id
- Hybrid Model 可以切换或同时适配

可以观察到刚插入的数据一般都比较活跃，时间推移后期一般变成只读数据，OLTP通过ETL过程进入OLAP数据仓库



