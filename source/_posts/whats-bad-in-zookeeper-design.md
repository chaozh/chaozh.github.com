title: Zookeeper系统的不足
date: 2015-10-15 20:00:11
categories:
- 系统经验
tags:
- zookeeper
- raft
- paxos
---

之前总结过Zookeeper的各种设计优点，但是这个系统的缺陷与优点同样突出，本文就是结合自己的使用经验，业界给出的评价对ZK的缺点进行的归纳，一方面归纳使用表现上的不足，另一方面根据个人经验总结出系统本身功能设计时的就存在的缺陷。同时也思考了相应对策与改进的办法，算是本人对ZK设计的完整的思考总结吧。最后还关注了下etcd这个后起之秀的设计，看看它是否已经弥补了ZK的不足，能否担当后继者。

## 1.实际使用暴露的不足

尽管ZK已经在工业界大量使用，但实际使用ZK时就会发现自己仍然面临种种问题，这里总结一下实际使用中系统暴露出的种种不足。

1. API使用复杂

   使用ZK最直观的感觉就是客户端API使用起来特别麻烦，因为ZK的实际逻辑模型是文件存储，其他逻辑都需要结合使用临时节点和回调机制来实现，API接口与实际需求之间差距非常大，说白了就是ZK官方提供的API太过底层，而且表现力不足。没有http访问接口，只能通过API访问，导致兼容性与表现各异。比如一个细节很容易坑人：

   >
   > ZK不能确保任何客户端能够获取（即Read Request）到一样的数据，需要客户端自己同步：方法是客户端在获取数据之前调用org.apache.zookeeper.Async(Callback.VoidCallback, java.lang.Object) 完成sync操作. 
   >

2. 实际客户端逻辑复杂

   因为ZK提供的是非常底层的API，所以要实现一个基础需求，开发者用户自己需要依靠这些底层API在客户端实现并维护一个异常复杂的逻辑，甚至官方自己都一度不能提供正确的逻辑。比如实现分布式锁时，需要开发者自己在客户端处理异步请求锁的时间判断与仲裁，很容易出错。因此实现了多种ZK常用功能的开源项目Curator取代了官方成为ZK事实标准上的客户端了，总算让开发者使用ZK时稍微轻松一些，能够专注到自己的业务逻辑上面。但是Curator方案仍然有问题无法避免。

   > 而 Curator 这类客户端的复杂性使得支持多语言环境较难，怎样保证两个语言的 recipe 的是行为一致的？基本没有办法通过自动化测试来保证正确性，只能不停地线上踩坑一个个排查。

3. ZK异常状态判断

   比逻辑更复杂的是开发者总是需要自己同时处理ZK通信中的异常状态，需要正确处理CONNECTION LOSS(连接断开) 和 SESSION EXPIRED(Session 过期)两类连接异常。一个典型的难题就是session的超时机制的问题

   >The client will stay in disconnected state until the TCP connection is re-established with the cluster, at which point the watcher of the expired session will receive the "session expired" notification.

   甚至需要考虑包括业务方系统load变高，或者发生长时间gc，导致ZK重连甚至session过期的问题。

4. 回调次数限制

   ZK中所有Watch回调通知都是一次性的。同一个ZK客户端对某一个节点注册相同的watch，也只会收到一次通知。节点数据的版本变化会触发NodeDataChanged回调，这导致需要重复注册，而如果节点数据的更新频率很高的话，客户端肯定就无法收到所有回调通知了。

5. Zab协议与全系统的有效性

   实际应用中ZK系统整体可靠性也不一定有保证，比如在跨机房部署方面，由于ZK集群只能有一个Leader，因此一旦机房之间连接出现故障，Leader就只能照顾一个机房，其他机房运行的业务模块由于没有Leader也都只能停掉。于是所有流量集中到有Leader的那个机房，很容易造成系统crash。

   同时ZK对于网络隔离极度敏感，导致系统对于网络的任何风吹草动都会做出激烈反应。这是Zab协议本身的设计，一旦出现网络隔离，ZK就要发起选举流程。悲催的是Zab协议的选举过程比较慢，期间集群没有Leader也不能提供服务。造成本来半秒一秒的网络隔离造成的不可用时间被放大为选举不可用时间。

6. 性能

   ZK本身的性能比较有限。典型的ZK的tps大概是一万多，单个节点平均连接数是6K，watcher是30万，吞吐似乎还可以，但是时延就没那么乐观了。特别


   响应时间、网络、缓存

   ​

7. 服务极限与可扩展性

   ​

8. 存储极限与可扩展性

   ​

## 2.需求满足缺陷

设计层面上的不足

1. 事务API能力不足
2. 中心无仲裁能力
3. 回调次数
4. 可扩展性
5. Zab协议

## 3. 怎样更好的设计

1. 更丰富有效的API

    支持多IP，自动处理重连与恢复，服务端可以动态加入移除
2. 委托服务端处理能力
    script能力？像redis支持luaScript
    节点单独超时时间设计
3. 横向可扩展性
4. JVM

## 4. 继任者etcd？

1. raft协议取代zab
2. go GC
3. restful API

## 5. 参考资料
1. [zookeeper节点数与watch的性能测试](http://codemacro.com/2014/09/21/zk-watch-benchmark)
2. [对Zookeeper的一些分析](http://blog.csdn.net/wwwsq/article/details/7644445)
3. [ZooKeeper真的low吗？上千节点场景配置服务讨论](http://cloud.51cto.com/art/201508/487445.htm)
4. [Zookeeper常见问题整理](http://www.aboutyun.com/blog-1328-2362.html)
5. [ZAB问题总结](https://www.douban.com/note/277477728/)
6. [剖析etcd](http://www.infoq.com/cn/articles/coreos-analyse-etcd)