title: 分布式锁实现问题讨论
date: 2016-03-08 12:53:34
categories:
- 面试总结
tags:
- 事务
- 数据库
- 无锁并发
---

redis引出
[分布式锁如何实现](http://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)
[读锁实现安全吗？](http://antirez.com/news/101)

[观众开帖论战](https://news.ycombinator.com/item?id=11065933)

[有人跟帖](http://fpj.me/2016/02/10/note-on-fencing-and-distributed-locks/)

[论证读锁问题确实存在](http://jvns.ca/blog/2016/02/09/til-clock-skew-exists/)

推荐还是运用[理论](http://blog.acolyer.org/2016/02/09/the-heard-of-model/)证明一下吧