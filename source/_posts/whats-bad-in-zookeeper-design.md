title: Zookeeper系统的不足
date: 2015-10-15 20:00:11
tags:
- zookeeper
- paxos
categories:
  - 系统经验
---

## 1.实际使用暴露的不足

回答Zookeeper使用时还有哪些问题

1. API使用复杂
2. 实际客户端逻辑复杂
3. ZK异常状态判断
4. 回调次数限制
5. Zab协议与全系统的有效性（能否避免脑裂发生）
6. 性能（不算问题的问题）
	
	吞吐率、响应时间、网络、缓存

7. 服务极限与可扩展性
8. 存储极限与可扩展性

## 2.需求满足缺陷

设计层面上的不足

1. 事务API能力不足
2. 中心的仲裁能力
3. 回调次数
4. 可扩展性

## 3. 怎样更好的设计？

1. 更丰富有效的API
    
    支持多IP，自动处理重连与恢复，服务端可以动态加入移除
2. 委托服务端处理能力
3. 横向可扩展性
4. JVM

## 4. 继任者etcd？

1. raft协议取代zab
2. 

## 5. 参考资料
1. [zookeeper节点数与watch的性能测试](http://codemacro.com/2014/09/21/zk-watch-benchmark)
2. [对Zookeeper的一些分析](http://blog.csdn.net/wwwsq/article/details/7644445)
3. [ZooKeeper真的low吗？上千节点场景配置服务讨论](http://cloud.51cto.com/art/201508/487445.htm)
4. [剖析etcd](http://www.infoq.com/cn/articles/coreos-analyse-etcd)