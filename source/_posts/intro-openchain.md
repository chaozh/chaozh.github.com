title: Open Chain系统设计
date: 2016-03-07 21:34:00
categories:
- 系统经验
tags:
- 数据库
- 区块链
- 事务
---

## OpenChain简介

[openchain](https://github.com/openchain/openchain/)使用C#开发的分布式总账系统，有比较强的横向扩展能力，并利用比特币的区块链保证不可更改性。

## 常见问题

1. openchain是一种区块链吗？
   答：不是，其不使用块这个概念，并直接使用事务链。
2. openchain是一条侧链吗？
   答：提供了模块将openchain部署为侧链，但并非必要步骤。
   
## 部署方法

提供docker方法直接部署openchain的服务器，使用sqlite作为默认存储，运行在.NET Core和.NET Framework上面

## 系统架构

1. 事务流
   openchain的服务节点可以分为两种：validator节点、observer节点：前者接收事务并进行验证，只有通过验证的才计入总账（验证规则由管理员配置）；后者会下载所有通过验证的事务信息流（形成备份？）
2. 利用区块链
    即利用公链提交事务形成hash（使用OP_RETURN）
3. 数据结构
    使用Protocol Buffers进行客户端与服务器之间的编解码操作
4. 总账结构
    kv结构的有层级的总账