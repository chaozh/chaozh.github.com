title: Xshell及Vim中文乱码解决
tags:
  - Linux设置
  - 中文汉化
id: 713
categories:
  - 系统经验
date: 2013-09-15 09:32:54
---

首先要保证Linux本地语言设置，主要是LC_ALL的设置（该设置会覆盖其他设置），可以使用locale查看。临时修改使用export LC_ALL="zh_CN.UTF-8"，永久修改需要修改文件/etc/profile增加或修改export LC_ALL=zh_CN.UTF-8，修改完后无需重启。

其次要保证Xshell的连接编码使用utf-8而非默认编码。最后设置vim修改~/.vimrc增加set fileencodings=utf-8,gbk一行