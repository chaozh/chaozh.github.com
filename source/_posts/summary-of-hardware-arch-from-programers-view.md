title: 程序员实用硬件体系结构归纳
date: 2016-07-02 20:00:11
tags:
- CPU
- memory
- cache coherence
categories:
- 系统经验
---

## 1. 硬件体系结构

硬件关注大致CPU、CPU Cache(L1, L2, L3)、内存、SSD、普通硬盘HDD、网络网卡这几个层次

## 2. CPU架构

CPU核内主要技术点包括Super pipeling、Superscalar
、Simulaneous Multi-Threading(SMT)、out-of-order等，目前CPU都使用多核架构，例如来自Intel Core i7的Nehalem架构Fetch to retire，

CPU使用IMC接口与内存互联，使用QPI连接多个处理器((processors 比如Nehalem Quadcore)

## 2.1 性能指标

参考性能指标包括：

1. 主频：		CPU的时钟频率，内核工作的时钟频率
2. 外频：		系统总线的工作频率
3. 倍频：		CPU外频与主频相差的倍数
4. 前端总线：		将CPU连接到北桥芯片的总线
5. 总线频率：		与外频相同或者是外频的倍数
6. 总线数据带宽：	(总线频率 * 数据位宽) / 8

相关组件有L1,L2,L3 cache (缓存数据与指令)
L1,L2:		core独占；     带宽：20-80GB/S；延时：1-5ns
L3：		core之间共享； 带宽：10-20GB/S；延时：10ns
Cache line size：	64 Bytes

QPI接口
Intel中，连接一个CPU中的多个处理器，直接互联 QPI带宽：~20GB/s

### 2.2 程序优化

## 3. CPU Cache

Cache Line是Cache与内存Memory交换的最小单位，X86架构的CPU基本为64B，以组相联的形式组织

一种方案Write Invalidate写导致其他CPU L1/L2的同一Cache Line失效，另一种Write Update写同时更新其他CPU L1/L2的同一Cache Line

写策略通常有Write Back新脏数据先读到Cache然后修改，Write Through新脏数据直接写到Memory

主要问题是Cache Coherence算法，比如MESI, MOESI等

MESI协议，有四种状态：

* Invalid 无数据, 
* Shared 与Memory一致数据且多节点共享, 
* Exclusive 与Memory一致数据且单节点持有, 
* Modified 最新修改数据且单节点持有。

有六种交互信息：

* Read
* Read Reponse
* Invalidate
* Invalidate Acknowledge
* Read Invalidate
* Writeback



MOESI协议，五种状态：owned 与Shared共存，持有最新修改且Memory过期

这些协议作用于CPU Cache L1/2和内存，与Register无关

## 4. Memory 

正确理解Memory Ordering与并发的关系

### 4.1 Atomic

涉及到内存的读写指令，特别是非对齐的双字操作，可以使用专用指令：CMPXCHG, XCHG, XADD等，或使用lock指令（使用在指令之前表示当前指令操作的内存只被当前CPU所用）

### 4.2 Memory Barrier

### 程序优化

## 参考

1. [CPU架构浅析]()
2. [Memory Barriers: a Hardware View for Software Hackers](http://www.rdrop.com/users/paulmck/scalability/paper/whymb.2010.06.07c.pdf)