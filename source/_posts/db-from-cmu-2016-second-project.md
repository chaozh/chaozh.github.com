title: CMU课程15-721数据库系统设计（第二个项目）
date: 2016-02-29 10:03:34
categories:
- 系统经验
tags:
- 数据库
- 索引
- 无锁并发
---
CMU课程15-721最新一期开课啦！

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

## Project #2 - Concurrent Index

实现一个支持无锁并发的BW树作为索引底层数据结构
- CAS映射表
- 改动链
- split/merge
- 动态垃圾回收

论文中几个奇怪概念：

- install CAS 
- epoch机制

## Profile方法

profile工具使用valgrind或perf

1. valgrind

使用memcheck或callgrind动态检查：`valgrind --tool=callgrind --trace-children=yes ./build/src/peloton -D data &> /dev/null&`

2. perf

通过收集linux的cpu周期计数实现，也支持收集其他事件
`perf record -e cycles:u -c 2000`

