title: Java架构师问题总结
date: 2016-03-08 10:03:34
categories:
- 面试总结
tags:
- JVM
- 事务
- 数据库
---

## 常见面试流程
1. 技术相关
   * java语言（基本、容器、多线程、异常处理、JVM）
   * 设计模式
   * Spring
   * MySQL、Redis等（JDBC）
   * 前端相关
   * Shell相关
   * 算法题

2. 项目相关 

## Java语言基础

### 面向对象

1. 绝对不能在构造函数中调用会被Override的函数
2. QA：builder模式与Config类(InstanceSpec)取舍，builder需要创建静态类
3. QA：构造函数什么时候可以抛异常
4. QA：init私有方法什么时候使用

### String使用
1. 判断是否相等**绝对**不能直接使用"=="进行，可以使用`equals`方法
2. 判断是否为空: `StringUtils.isBlank()`，`Str.isEmpty()`，`Str.length()==0`

### 集合使用
1. Array初始化
```
new ArrayList<Element>(Arrays.asList(array))
```
2. QA：集合接口的区别list collection hashmap


### Exception处理
1. 标准Exception分为两种expcetion，一种继承自RunTimeException，不需要方法捕捉，比如NullPointerException，IllegalStateException；另一种必须catch或throws，比如InterruptedException
2. 推荐使用Preconditions.checkNotNull等工具
3. catch后的代码仍然会执行，返回值与抛出异常都需要满足匹配，try中抛出的异常就会被对应catch捕获，上一个catch中抛出也会被下一个catch捕获，如果分支继续抛异常来结束那么该分支可以忽略返回值
处理方法：1. 异常转义；2. 异常链接getCause
4. 单元测试函数应该抛出任何异常
5. 利用jdk7新语法自动关闭文件流
```java
try(InputStream is = new FileInputStream){
	//文件操作
}
```
6. 对No such method error处理：打印出具体绑定类
```java
<Object>.class.getProtectionDomain();
```

### 注释处理
1. Bean注入
@Autowired
ApplicationContext ac;
lockAnnotation = (LockAnnotation)ac.getBean("LockAnnotation")
2. 
@Service与@Resouce可以配合使用
http://bit1129.iteye.com/blog/2114126
3. AOP
http://www.xdemo.org/springmvc-aop-annotation/

### 多线程
1. 后台定时任务
	```
	//start
	ScheduledExecutorService excutor = 	Excutor.newSingleThreadScheduledExecutor();
	excutor.scheduleWithFixedDelay(runnable, time);
	//end
	excutor.shutdown(); //excutor.shutdownNow();
	```

2. 线程池
	```
	List<Future<Object>> threads = Lists.newArrayList(); 
	ExcutorService excutor = 	Excutors.newCachedThreadPool();
	Future<Object> t = executor.submit(new 	Callable<Object>() {
		@Override
		public Object call() throws Exception {
			//do sth here!
			return null;
		}
	});
	threads.add(t);
	for(Future<Object> t: threads)
		t.get();
	```
	
3. CountDownLatch与Semaphore使用

	CountDownLatch设置初始值，调用`wait()`方法会阻塞直到被`notify()`方法唤醒；调用`countDown()`来减少值；创建时不会阻塞，需要调用`await()`阻塞直至计数值为0；该机制被设计为只会触发一次，因此不会有方法来还原初始值。

	Semaphore设置初始值，调用`acquire()`方法减少该值否则阻塞直至计数值大于0，调用`release(num)`来增加值，调用`availablePermits()`判断是否阻塞

4. 单例模式注意使用double check方法

``` java
public Helper getHelper() {
    if (helper == null) {
        synchronized(this) {
            if (helper == null) {
                helper = new Helper();
            }
        }
    }
    return helper;
}
```
QA：该模式有失效的可能？

5. executor 重要的三个参数含义
corePoolSize, maxmumPoolSize, [keepAliveTime], BlockingQueue<Runable>

6. 多线程的方法

### JVM

1. 有哪些垃圾回收器 优缺点 

2. 四种导致full gc场景

	1. 持久代满了，gc不成功则out of memory
	2. 旧生代满了
	3. 新生代向S0和S1转移数据，S0和S1向旧生代转移数据，两边内存都设置较小，持续进行导致
	4. 直接system.gc

3. 原子变量实现 设置unsafe 内存交换

### JDBC

1. 何时使用乐观锁 悲观锁 出现场景，jdbc事务提交使用哪种锁（一般使用DB的悲观锁）

## Spring

调优：tcp窗口，事务大小

1. 常用模块
	aop, aspects, beans, core, context, expression, orm, jdbc, jms, tx, test, web, webmvc 
2. 主要接口

3. 五种事务传播性Propagation
	required, supports, mandatory, requires_new, not_supported, never, nested 

## netty

结构 lf缺陷

## MySQL
1. 设置AUTO_INCREMENT
http://www.searchdatabase.com.cn/showcontent_38510.htm
2. 使用datetime或timestamp类型，查询范围写法：send_time <= '2015-09-24 15:24:16'
> 区别：首先 DATETIM和TIMESTAMP类型所占的存储空间不同，前者8个字节，后者4个字节，这样造成的后果是两者能表示的时间范围不同。前者范围为1000-01-01 00:00:00 ~ 9999-12-31 23:59:59，后者范围为1970-01-01 08:00:01到2038-01-19 11:14:07。所以可以看到TIMESTAMP支持的范围比DATATIME要小,容易出现超出的情况.
> 其次，TIMESTAMP类型在默认情况下，insert、update 数据时，TIMESTAMP列会自动以当前时间（CURRENT_TIMESTAMP）填充/更新。
> 第三，TIMESTAMP比较受时区timezone的影响以及MYSQL版本和服务器的SQL MODE的影响
>所以一般来说，我比较倾向选择DATETIME，至于你说到索引的问题，选择DATETIME作为索引，如果碰到大量数据查询慢的情况，也可以分区表解决。
> **最佳实践：** 只要用int类型存格林时间的unix timestamp值就好了，使用TIMESTAMP主要是为了方便记current_timestamp

3. 使用txt类型取代nvarchar
SQLserver 中使用nvarchar存储变长unicode字符串
4. 使用tinyint(8)或bool(1)取代bit类型, int(32), bigint(64)
5. DEFAULT 为0，则可以查询''（空字符串）；DEFAULT为NULL，则查询''（空字符串）不匹配
6. 业务ID自增设计
`insert msgId into [table] value(select max(msgId) from [table] + 1)`
这里max函数会导致锁表
解决方法：单记一个max version表，利用行锁进行多线程同步
7. 当发生Too many connections时，即使是DBA也无法登录到数据库，一般的做法是修改配置文件的max_connections参数，然后重启数据库，这样业务就有几秒钟的中断。
还有一个hack的方法，用过gdb直接修改mysqld内存中max_connections的值，具体做法如下：
`gdb -ex "set max_connections=5000" -batch -p 'pgrep -x mysqld' `

## JBOSS
调优方法
1. connector 改为Nio
2. 使用G1回收器，+UseG1GC 提供一种计算方法预期最大停止时间
3. 调整内存使用

## Nginx

运用功能 模块实现 核心组件 upstream配置

## Redis
1. 连接数
2. 集群设置 集群算法

## Curator
1. 使用InterProcessMutex中的RevocationListener因为在另一个专用线程因此没法直接调用release释放，Revoke唤醒需要使用对应的lock节点而不是目录
QA：这个设计很奇怪，按常理revoke应该调用在lock的path上然后自动通知当前获得锁的lock结构，并且应该有方法允许释放【可能状态比较难统一】
2. 2.9版本之后可以自动清理锁目录，需要设置container.checkIntervalMs
3. 共享锁实现
http://www.tuicool.com/articles/yQBVfeA
4. start方法只能在初始化后调用一次，close调用后只能销毁

## Dubbo
1. 使用transient关键字保证成员不被串行化

实现思路
### 1. WS服务转RPC
分为两个难点：wsdl信息提取，RPC服务端的API生成
wsdl信息提取：例如从BPM提取出ReceiveMsg方法与参数
RPC服务端API生成：
http://dubbo.io/User+Guide-zh.htm#UserGuide-zh-API%E9%85%8D%E7%BD%AE
API方法所依赖的接口service与实现serviceImpl可以使用通用的泛化实现接口与实现
http://dubbo.io/Generic+Implementation-zh.htm

### 2. RPC服务转WS
分为两个难点：参考Dubbo客户端实现RPC客户端自动生成，WS服务端API生成
RPC信息提取：
WS服务端API生成：

