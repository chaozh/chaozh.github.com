title: CMU课程15-721数据库系统设计（第二个项目）
date: 2016-02-29 10:03:34
categories:
- 课程总结
tags:
- 数据库
- 索引
- 无锁并发
---
CMU课程15-721最新一期开课啦！

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

## Project #2 - Concurrent Index

实现一个支持无锁并发的BW树作为索引底层数据结构
- PID到指针的映射表
- 改动链
- split/merge
- 动态垃圾回收

论文中几个奇怪概念：

- install CAS 即使用CAS操作来替换映射表中的项目
- epoch机制 即使用版本号保证没有其他操作正在进行（可以执行物理删除）

实现要求：

- 支持重复key且可以配置，如果配置为不允许则`InsertEntry `插入重复key的索引条目时返回`false`，且`DeleteEntry `只删除<key, value>完全匹配的索引条目
- 支持consolidation，需要考虑阈值
- 支持split与merge，特别是并发插入与删除的情况
- 应该有工具函数帮助测试结构完整性并可以修复，类似`fsck`
- 支持动态垃圾回收，调用`Cleanup`函数进行物理释放，会使用`GetMemoryFootprint`统计内存情况
- 支持`reverse iterators`并保证`concurrent mutators`正常工作

## Profile方法

profile工具使用valgrind或perf

1. valgrind

使用memcheck或callgrind动态检查：`valgrind --tool=callgrind --trace-children=yes ./build/src/peloton -D data &> /dev/null&`

2. perf

通过收集linux的cpu周期计数实现，也支持收集其他事件
`perf record -e cycles:u -c 2000`

