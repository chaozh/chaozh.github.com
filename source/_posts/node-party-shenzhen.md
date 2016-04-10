title: 深圳node party小结
date: 2016-04-09 14:05:31
categories:
- 前端
tags:
- js
- node
---

## 实战ES2015

* classes
* export&import 统一标准
* 使用rollup.js打包、HTTP/2

## 平台化运营实践

腾讯MTT对比pm2设计

* 进程管理
* 日志类型（按大小滚动，按时间滚动）
* 日志输出 时间|pid|级别|文件名:行号
* 可配置的服务（不重启）
* TAF协议与JCE规范（编解码,不动态解析方便debug）
* 调用监控（流量、耗时、超时率、异常率、服务+接口）与特性监控（自定义）

日常问题

- 时间计算 `Date.now()`最快，`process.uptime()`最好
- 内存与进程 虚拟核心与物理核心问题
- 用户环境 服务调用只在同环境中进行，服务转发？？
- 染色应用 将整个服务调用链日志统一收集展示
    
    push 类推送业务 （峰值流量->削峰）
    用户访问相关业务 （越平滑流量越大）
    定时拉取任务（持续资源使用->错峰）

`process.setMaxListeners(Infinity)`隐患
load高、cpu不高 -> 查io(socket)、swap使用率

## Nodejs广发证券：koa2与微服务

nodejs定位（实现API Server）

- Restful Extend（Filter&Sort，Bulk Inserts，Pagination，Optional-filds）
- Falcor @ Netflix 
- 使用Koa2 async&await（异步，错误处理）koa-adapter
- 接口验证与数据验证 koa-validate&joi(数据验证)

日常

- es2015使用
- npm scripts（npm-check）
- 入库前检查 使用husky检查，
- 测试覆盖 （经常修改、复杂、多人合作）`npm run coverage:lab` plato（js源码可视化）

线上运行

- StrongLoop 性能优化系列文章
- 上线准备（Event Loop监控，通过环境变量与信号来调整log级别，异常重启比忽略好）
- 支持几千几万的流量（request-debug）
- docker运维，镜像部署方便，链路监控（Zebking）

## TypeScript在Nodejs应用

techird.com 