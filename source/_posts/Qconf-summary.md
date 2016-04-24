# Qcon day 1

## 1.1 七周七并发

```java
for(int n: numbers)
	sum += n;

```

```clojure
(defn sum [numbers]
	(reduce + numbers))

(defn count-words [pages]
	(frequencies (mapcat get-words pages)))

(defn count-words [pages] 
	())
```

## 1.2 JVM G1

performance tuning: throughput/ response time/ footprint/ availability / capacity
* analyzing log
* monitoring
	- lock stats, utilizations[cpu,mem]
* profiling

JVM (class loader + gc + runtime + JIT compiler)

* JVM monitoring
* app profiling
* process & plot GC logs
* online& offline GC monitoring

GC 

* NOT eliminate your memory leaks!(GC heap dump)
* throughput & latency(no footprint) two main drivers
	- throughput max (generational + parallel work 
	- latency (pause + concurrent mark/sweep)
* All GC in OpenJDK HotSpot are generational
	- fast path alloc == thread local alloc buffer == lockfree alloc [epoch 机制！]
	- young gc == reclamation via scavenging(eden -> survivor)
	- old gc(different algothm!) reclamation via parallel mark-swept  compact/ multi-threaded
* All non/partial compacting GC fallbak to full gc [like g1?]

G1

* young collection(partial) -> initial mark(less pause) -> remark(big pause) -> cleanup(little pause) -> mixed collection(partial)
* collect most region(mixed collection)
* heap configuration

Tuning GC

* elapsed time
	- copying costs(in compact!)
* overhead(more possible)
	- alloc rate & promotion rate

1. size generations(faster filled sonner gc trigger)
2. size appropiate (more copy more pause)

adaptive alogithm for gen size(do steps - find in "print")
java performance tuning? monica@codekaram.com (left oracle)
openJDK hotspot vs jeorocket

# sp
kubernet 1000node限制 封装+100ms延迟 
华为王宇冬提问：扩展存储问题（有状态使用ceph，性能下降问题）

## 1.3 网易蜂巢容器云

docker 开发人员与sa关系（改变部署、协作方式)
	- 运维定义仅镜像-》自动化容器云平台
	- 成熟度 Docker（网络、存储坑）/ kubernetes（多租户、有状态坑）/ OpenStack（公有云运用坑多） 

容器网络: 容器互联
复杂情况：
	NAT
	- 长连接问题、IP注册服务发现问题、迁移复杂
	端口映射
	- 运维复杂度、端口冲突、内外IP
	层级网络
	- 不利于故障恢复、IP变化（控制IP分配）

* VxLAN网络(Openstack Neutron) 
	- 每个租户独立私有、外网网卡直接挂载、私有网络>容器网络
* 基础服务 暴露为统一url
* 编排（选择kubernetes）
	- 多租户 node、存储、网络隔离
	- 性能问题：所有操作都在任务队列串行执行、LB聚合多个etcd集群（超过1千节点）、调度器以租户调度(?)
* 诊断工具
	- 统一日志 调用链！rsyslog收集
	- 基础服务 监控定制化(慢查询、死锁、复制等)
* 容器运行
	- 保持状态：网络、存储（远程盘、实现reload命令）
数据库docker化？ ceph或高IO块

## 1.4 Startup大数据平台架构 AppAnnie
王佳 ramon@appannie.com 应用商店分析和市场数据
Startup与大数据(人员成本、运维成本)
架构平台设计原则（每天20TB压缩数据、6+集群、500+数据处理任务、100~200服务器）

* 数据驱动（引入data schema中间层）
* 使用工作流引擎管理（数据节点复用，通过executer来封装底层执行引擎）
	- 自实现 Dagre-D3(js, UI) + Engine(Python RQ，取工作流，找task丢到Redis) + Redis
	- oozie, Azkaban, luigi（内部大数据）
Pig 构建数据管道，HIVE 临时查询与分析，Python 实现各种ETL、算法模型、Pig的UDF函数

## 1.5 OS导致非典型GC停顿（zhenyun）
bg: java(HotSpot JVM) + linux (paging[huge page] + swapping + page cache[writeback] )
理论来自IEEE Cloud 2014
三种场景：

- startup state
- steady state with memory pressure
- steady state with heavy io

workload:

- java app alloc/de objects
- bg app taking mem or disk ios

1. Startup State

Q: java heap（resident size查看）逐渐增大-> 导致OS启动direct page scanning(cpu飙升) -> OS启动swapping
A："-XX:AlwaysPreTouch"初始化 + 设置Swappiness = 1 + cgroup具体设置 

2. Steady State (mem)

Q: 以为是heap被swap out导致(55 s时间太大，sys/usr/real time中为55/0不应该cpu这么高) -> linux默认特性THP导致(加速寻址,malloc超过2M自动启动)  
A: mem压力时不用THP，没有再开

3. Steady State (IO)

Q: sys/usr/real(0.01/0.18/11.45) disk io比较高 -> gc之后1.5s停顿(要小写gclog文件导致，但是异步花这么长时间return?) -> stable page write(pdflush启动) / journal commit导致(包括需要alloc新block时启动)  
A：gclog写能否异步？否！

mission control
* 先看usr/sys/real 
* 小心linux新的特性
* 注意各层的问题

QA：CMS容易full gc-> mem leak->静态变量太多 / object size比较大?

## 1.6 (百万量级)细粒度查询意图识别

搜索广告-基于关键字匹配

- 查询短、特征稀疏、歧义强
- 广告缺乏相关性

目标：从日志挖掘查询意图，处理高频与长尾查询
现有方法：Google Rephil系统(Bayesian网络推断)使用在adsense
识别意图3类方法(短文本聚类[长尾无法覆盖，缺失推断能力]、Topic Modeling[不同topic难对应，精确度不足]、查询分类[粒度较粗])
方法：查询聚类(聚类算法抗噪、图挖掘中社团发现算法->MMO算法)
网络构造：2年点击日志-> 抽取query-url关系
问题：过大不纯类、太多细粒度 -> 质量评估（纯度、文本相关性）
意图推断：候选分类发现、拒绝项
应用：广告召回、广告质量保证

## 2.1 Twitter message queue
2012年每个不同基础服务对应一个message
database -- Bookeeper

* kestrel (simple, Fan-out queue, Reliable reads, cross DC rep)
问题：
	- each queue is a file(持久化性能问题)
	- 
	- 读旧数据性能(mem cache设计问题)

* Kafka (小规模订阅的顺序io性能非常好)
	- 依赖OS的page cache(文件多导致random io)
更大问题：各个软件组件维护问题(client, upgrade, hardware)

消息总线目标：1.unified stack; 2. Durable writes(intra-cluster, geo-repli); 3. 多租户; 4. scale resources independent; 5. 易于维护

仍然采用日志追加模型 entry(DLSN, Sequence ID, tid)
	- 没有实现partition
	- 存储层Bookeeper
	- 核心层writer, reader 读写分离
	- 无状态服务层 write proxy(ownership tracker)/read proxy(routing service)
	- 方便服务层(container去做)与存储层扩展	
	- 冷数据存储 HDFS

注意：write proxy写是2PC的(Bookeeper实现or自己逻辑实现？BK使用的是Paxos)
最终一致性实现：LastAddConfirmed(读) & Fencing(写，争强问题？hash)：owner tracking(不需要严格选举，lease机制即可，zk就能提供，failure detect: TickTime=500ms Session Timeout=1000ms)
Geo-repli要求zk多机房部署一个集群
总结：底层保证、不要相信文件系统(page cache)
guosijie_

## 2.2 intel Spark 优化

10%~15%内存给OS做cache(dcache, page cahce)
- memory per node * (85~90)% / executors per node
- 2~5GB per core: * spark.executor.cores

memoryOverhead参数：默认太小，防止offheap mem size导致被yarn kill
serializer参数：kryo序列化 15~20%提升
partition选择：太大(磁盘压力、轮数太多)，先设大值再慢慢降(GC问题)
IO调整：
- storage level 
- compression 默认rdd不压,shuffle压
dynamicAllocation: 在Spark SQL好使
GC：WebUI(GC Time 大部分GC严重开始调)，尝试减少并行度
blktrace 分析io，顺序(大IO,延迟小,磁道数连续)/随机(小IO,延迟大,磁道)分类(通过lba地址时间线判断) -> shuffle read是随机的，易于造成io瓶颈

QA：除非内存超大，全在OS page cache中shuffle，但是提升相比也不多，因为瓶颈主要体现在CPU了

## 2.3 百度分布式计算调度

资源调度+资源管理mesos
Omega论文图片说明历史 Yarn -> mesos(双层) -> Brog/Omega(乐观锁)
作业无法区分 -> 资源利用率 -> 异构系统 (60% CPU利用)
实现：

- 虚拟化交付，资源审计
- 自研RPC
- 丰富调度算法
- 物理队列、逻辑队列、优先级抢占等
- lib raft

## 2.4 服务治理

qlang语言 GO语言的TPL 从富媒体存储开始起步
服务端程序员特殊职责：on call
服务治理：更高可用性、业务开发更简单
服务发现与负载均衡、调度、配置变更。。。
微服务：变多，治理压力更大 -> 业务化组织
* 服务发现：服务的扩容缩容、调度，同时有负载均衡的问题；实现为客户端（性能更好）或负载均衡器（API gateway，治理能力更好）Kubernetes方案就不错，可以参考
* 过载保护：自动扩容可能因为异常导致问题更严重，建议区分好坏请求，扩容增加上限，拒绝部分请求(N*2 高于报警线)
* 服务降级：为重要请求保证资源
* 故障定位：找root cause（首先可以进行集群拓扑发现，根据服务请求链条找根源，预案处理），xushiwei（注：七牛求才）
QA：如何区分资源问题和机器硬件问题（比如慢盘问题，upstream检测）
QA：拓扑发现如何做？

## 2.5 Elastic Stack数据分析

曾勇Medci Elastic于2012年成立
ELK -》Bees
ElasticSearch：搜索与聚合、数据分析(运维日志)、支持超过1700多节点1.3PB数据
Ingest：Logstash（多数据源收集中间件）、Beats（golang，简单收集行为数据，包括三类：网络、系统状态、日志）
Kibana：可视化

## 2.5 分布式存储

设计考虑

- 本地海量小文件 例：Swift 
- 聚合存储 g:方便迁移，EC恢复; b:垃圾回收比较复杂 例：Heystack

美团云设计：

- s3模型 底层对象存储，用户建立域名
- Mangix系统 StoreServ（c语言、并发网络框架） ProxyServ（Golang、http访问入口）SchedServ（Golang 监控 恢复 调度 小文件垃圾回收） MetaDB（Oceanbase，中心化元数据存储）
* 存储设计 [partition] 迁移复制最小单位，对应一个文件256MB，三副本 [record] 2MB落盘，流式写入 [Record Index]Record ID -> File Position
* 恢复时间与可靠性 LRC算法
* 元数据设计 多版本、数据去重(sha-1)、对象覆盖写？
* sharding表如何设计方便EC RecordID(ServerIP+DiskID+timestamp)
* 垃圾回收 标记删除、孤儿数据
* MetaDB 万亿级数据、读多写少：LSM Tree实现就行 
* 进程独占机器 StoreServ/ProxyServ
* 测试：glibc钩，不间断压力测试

consistent hash问题：扩容、数据迁移代价

## 2.6 MVVM与FRP

美团变为平台+业务，挑战：代码复用率低、团队分工 -》分层+组件化

## 2.6 Apache Eagle ebay
分布式策略引擎
- SQL on Streaming
- 实时流处理 Storm+Kalfka

## 3.1 大数据技术演进

Delivery(Kafka/RabbitMQ/Flume[push-based]) -> Processing(spark streaming[batches]/storm/samza/flink[stream]/spark/hadoop[batch]) vs Querying(SQL-on-Hadoop[using both hadoop & db]/Impala/Hive[SQL like]/HBase[KV,pre-comput,range scans]/Druid[column store & query,even BI])[output smaller] -> Storage(HDFS/ALLUXIO[mem]/Kafka)

reaching saturation point
Delivery(Kafka) -> Stream Processing(spark) -> Querying
BI ??? -> TensorFlow DL4J -> AI ???

druid join问题: 只是还没实现好
data mining & trend predict问题：问题仍没有很好解决方案 
传统BI与Query Engine区别问题：开源，支持stream data，性能，功能目前无法比但很快会赶上

## 3.2 大数据、人工智能——京东VP

Hadoop核心思想 -> Partition data(key思路来源于search中倒排索引的token) + Coordinate & Schedule

Hadoop优化（主要集中于DiskIO）：Column存储、利用内存(Spark特别在数据挖掘)

如何开始build，有哪些坑：

* 大数据安全性（更多）
* BI应用（传统数据库已经研究很长时间）data model与Schema设计非常重要（减小数据复制，支持业务）
* 任务调度（10K+ 节点）
* 高性能与吞吐（每天100K+，除了平台，自身业务也要优化） 
* low lantency（实时搜索）

如何挖掘数据价值：

* Deep learning 数据量增大，对多媒体数据分析，SL RL
* Baysian program learning 不需要大数据，必须对问题有深刻了解，LDA

SL

* feature engineering 很常用，人工提供数据，实际上找特征比较难 Regression/GBDT/CRF
* Supervised Deeping Learning nero-network算法降维，自学习隐藏特性

Search ranking/personal recom/customer attrition/product price

* 大数据不一定覆盖重要特征，可能导致model失效
* feature engineering 比模型更重要
* real-time数据使用应该用在最后一步

Thinking Tech 未来big data

AlphaGo

* combine 多个 SL deep learning models & RL models
* Reinforcement learning[NEW!] 

产销决策 jd采用RL策略
Elon Musk's Open AI
data scientist与业务domain之间的gap解决：应该管理来解决（培训）

## 3.3 JS选型 - Dylan Schieman@dojo, sitepen

挑战：Browser release；new platforms(even watches & VR)；
优势：Ubiquitous；Easy；Zero install；
挑战：各种标准的冲突；缺少长期稳定一致；

1. 模块化 （包括HTTP2支持） 
2. 测试
3. 独立性

typescript帮助代码健壮(特别是动态属性)，interface帮助选择
A limited Test Drive问题
fat-controller & bi-directional slow -> virtual dom(maquette) & Redux
templating vs JSX vs HyperScript(dojo2)

## 3.4 数据库高可用——网易

同步方案：
DRBD性能 SAN价格 BINLOG MMM一致性问题 MHA都在用，主机硬盘问题
Semi-Replication事务提交顺序 Galera锁冲突比较多 NDB新存储引擎，使用人少

优化：

* 异步复制->同步复制？ 5.5.20-v3 => 5.7.2吸收
* 组提交：Binary log Group Commit 5.5.20-v3 => 5.6.6吸收
* 实时切换：从机并行回放（组里） => 5.6不同db进行；5.7完全吸收
* 可用域和存储池

切换成本问题（应用断连接）SQL响应时间长 80%切换可用避免（可以提前预知）

数据库健康检查：

1. 索引设计 主键索引(无主键[复制性能差、多表插入性能]或主键非自增) 冗余索引或无效索引(影响插入性能) 低效索引(索引区分度[information.cardinality/tables] 低于0.1则有问题)
2. 容量规划 cpu利用率、buffer_pool命中率(判断mem设置)、带宽、iops等
3. 参数配置 Binlog(expire_logs_days/sync_binlog[不开不一致]/binlog_format[raw]) Redo(Innodb_file_log_size过小造成SSD下间歇hang住/Innodb_flush_log_at_trx_commit=1) 内存(pre-thread buffers * max_connections + 其他共享内存 < 物理内存)
4. 服务安全 
5. 用户访问 死锁(error log中判断) 慢查询(按照SQL类型归并)

## 3.5 Druid介绍 @imply

开源分布式数据库(2011) 应用场景是billion~trillion的数量级
1. History & Motivation: scalable/Multi-tenancy/Real-time
power for BI/OLAP

traditional data warehouse - star schema -> Aggregates tables/query caches (very slow!)
KV stores - Hbase etc. pre-compute slow as data grows
Column stores - Druid! after fail with postgre & HBase

2. Design

supports custom column format -> summarization(agre on index -> shard on time[segment]) -> Multi-tenancy(deal with segment each)

3. Arch

data->Hadoop--> Historical Node[segments] <--Broker Node<-query
streaming data -> Real-time Nodes(改为reverse index Nodes) --> Historical Node <--Broker Node<-query
Real-time Nodes: convert write optimized(hash map) -> read optimized(segments) 
UI: Pivot/Grafama/Panoramix ...
shard保证数据字典变得太大 random hash
summary rollup会丢失秒级数据
为何选择time做hash 可以很容易filter
传统数据库导入：first export raw data format
cache也是根据时间(segments为单位)
grafana UI 新指标添加比较慢 
tableu在大数据表现不好[crash data]

## 3.6 数据访问层 ele.me

Netty.io 踩的坑
ele.me 纯python平台 -> 引入java生态
限流削峰、连接复用、用户隔离、分表、熔断、读写分离
* 指标：单价1万QPS(极限4.8万、日志trace都关了->2万)、延迟0.6ms、1.5万降为1千
* DAL功能引入演进：小功能2周左右
限流削峰 -> 主从分离 -> 二维分表(用户、商家的订单在一张表，拆分同时映射) 先用户再商户 显式commit -> 一维分表
DAL架构选型（康威定律）2015/04 -> 独立进程服务（非库服务，多语言，mysql协议进程通信）-> 业务可以回滚接入前
HAProxy -> Netty NIO -> MySQL解码 -> 限流队列(痛点！) -> 事件驱动SQL状态机 -> Netty NIO -> MySQL
非核心功能可以在线关闭 网络层/jdbc
同步（看起来是顺序的）-> 客户端/服务端超时 Netty IO异步 -> 清理资源不能释放两次 -> 加锁（标志）加好多！-> 去掉并发 SQL事件线程排队
切换工具长时间能否工作 -> 系统升级与容灾切换一起做 -> 注意事务没有commit的情况
CMS垃圾收集 170ms停顿->G1 20ms停顿 指定目标工作时间即可
客户端至少1s超时，因此GC不会太影响
Netty线程性限制 解决流量 减小CPU压力
读写数据一致性，从库10ms延迟，应用读从库需要忍受秒级延迟 