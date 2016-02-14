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

- T-tree, 基于AVL树（不存key直接存指针），可以用于memDB
- Skip List, 
- Radix Tree(Patricia Tree) 
- MassTree
- Fractal Tree