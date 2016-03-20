title: 分布式锁实现问题讨论
date: 2016-03-08 12:53:34
categories:
- 系统经验
tags:
- 分布式锁
- Redis
- Zookeeper
- 
---

分布式锁除了使用Zookeeper这个常用工具外，也有使用Redis这个KV内存存储进行实现的。某人介绍了一种基于Redis的分布式锁实现方法(命名为Redlock)并刊登到了redis官网：

[Redlock实现](http://redis.io/topics/distlock)

某人之后还发文详细讨论了实现方法，并说明自己感觉这个算法并不安全

[分布式锁如何实现](http://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)

接着Redlock算法的原作者开始就此事发文详细讨论，说明这个算法足够安全

[Redlock实现安全吗？](http://antirez.com/news/101)

与此同时观众们也在发帖论战

[观众开帖论战](https://news.ycombinator.com/item?id=11065933)

[有人跟帖](http://fpj.me/2016/02/10/note-on-fencing-and-distributed-locks/)

[论证RedLock问题确实存在](http://jvns.ca/blog/2016/02/09/til-clock-skew-exists/)

推荐还是运用[理论](http://blog.acolyer.org/2016/02/09/the-heard-of-model/)证明一下吧

[测试方法](http://colin-scott.github.io/blog/2016/03/04/technologies-for-testing-and-debugging-distributed-systems/)