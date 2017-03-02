title: CMU课程15-721数据库系统设计（第一个项目）
date: 2016-02-17 10:03:34
categories:
- 课程总结
tags:
- 数据库
- 索引
---
CMU课程15-721最新一期开课啦！

[课程安排](http://15721.courses.cs.cmu.edu/spring2016/schedule.html)

## Project #1 - Hash Join Operator

做这个项目前必须了解几个join算法，问题是这些算法在本次课程里都没有讲解过。

1.嵌套循环连接(Nested Loops)

适用范围两个表, 一个叫外部表, 一个叫内部表。如果外部输入非常小，而内部输入非常大并且已预先建立索引，那么嵌套循环联接将特别有效率。循环嵌套连接查找内部循环表的次数等于外部循环的行数，当外部循环没有更多的行时，循环嵌套结束。

2.合并联接(Merge)

指两个表在on的过滤条件上都有索引, 都是有序的, 这样join时, sql server就会使用性能更好的Merge join算法。如果一个有索引,一个没索引,则会选择Nested Loops join算法。首先从两个输入集合中各取第一行，如果匹配，则返回匹配行。假如两行不匹配，则有较小值的输入集合+1。

3.哈希联接(Hash)

如果两个表在on的过滤条件上都没有索引, 则就会使用Hash join算法。也就是说, 使用Hash join算法是由于缺少现成的索引。

哈希匹配分为两个阶段,分别为生成和探测阶段：首先是生成阶段，将输入源中的每一个条目经过散列函数的计算都放到不同的Hash Bucket中；接下来是探测阶段，对于另一个输入集合，同样针对每一行进行散列函数，确定其所应在的Hash Bucket,在针对这行和对应Hash Bucket中的每一行进行匹配，如果匹配则返回对应的行。

同时做这个项目需要了解Peloton tile-based架构几个关键数据结构：

`logical_tile`设计 [backend/executor/logical_tile] 

其中

- `position_lists_`为`vector<vector<oid_t>>` 用来描述tiles/colums, 例(1,2,3),(1,1,1)
- `schema_`为`vector<LogicalTile::ColumnInfo>` 用来描述logical tile的表字段组合, 会存在`position_list_idx`与position list对应, 例(Tile A-1 ATTR1, Tile A-1 ATTR2), (Tile A-1 ATTR3, Tile A-2 ATTR1)
- `visible_rows_`为`vector<bool>`用来描述position lists中每一行的可见性

`ColumnInfo`设计 [backend/executor/logical_tile]

在logical tile中用来描述一组表字段信息, 例如(Tile A-1 ATTR1, Tile A-1 ATTR2), 其中

- `position_list_idx` 指回Position Lists中对应编号
- `base_tile`为`shared_ptr<storage::Tile>`指向存储Tile，直接使用指针而不是oid
- `origin_column_id` 存储Tile中选择的表字段编号

`ContainerTuple`设计 [backend/expression/container_tuple]

用于包装logical tile中元组, 传入LogicalTile指针与元组行id(logical_tile, row_idx)

`HashMapType` 设计 [backend/executor/hash_executor]

用于定义hash表结构`unordered_map<expression::ContainerTuple<LogicalTile>, unordered_set<pair<size_t, oid_t>, boost::hash<pair<size_t, oid_t>>>, expression::ContainerTupleHasher<LogicalTile>, expression::ContainerTupleComparator<LogicalTile>>`
其中pair<size_t, oid_t> 表示 <size_t tile_idx, oid_t row_idx>

使用类型为`vector<unique_ptr<executor::LogicalTile>>`的`left_result_tiles_`与`right_result_tiles_`缓存
最终hash操作返回结果为`vector<LogicalTile *> result`使用`BuildOutputLogicalTile`方法(主要调用`BuildSchema`方法得到表字段组合)返回得到`unique_ptr<LogicalTile>`

