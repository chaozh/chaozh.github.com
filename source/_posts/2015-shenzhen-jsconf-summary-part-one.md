title: 2015 shen JSconf大会总结（第一天）
date: 2015-07-31 19:03:16
categories:
- 前端
tags:
- nodejs
- es6
- WebGL
---
两个星期前7月11～12日在深圳前海举行了第四届中国jsconf会议，第一次参加纪录一下学习内容与个人思考。本次会议由李学斌（hk）推广，wiredcraft公司承办

##Part One 2015-07-11

###1. Database everywhere - EvanYou@Meteor
* UI - Client data - Server data (sync)
Solution: UI - Client data (react, angular, vue)
* How about Client & Server data?
REST (Backbone Model, ngResource, Restangular, Flux Stores)
pull-centric imperative/ trouble with non-conventional APIs/ difficult managing optimistic updates
* IDEA: UI - Server data (GraphSQL, JSON Graph, Om Next) *not ready*

**METEOR**: DDP, Tracker, minimongo(both cli& ser)
UI - minimongo - Server data

publish & subscribe?  component-level template

`$meteor.collection(new Mongo.Collection("Task"))`
`Tasks.find().fetch()` ???

QA: actions record -> rollback

ppt地址 Slide.com [http://slides.com/evanyou/shenjs]

###2. ES6 generators - BIHOLT@Netflix
node 0.12  
as ES2015
* generators: async pro -> sync
(Meteor, Babel use Regenerator)

a function return a generator obj which is iterable

```
const colorGenerator = function*() {
	for (let color of colors) {
		yield color;
	}
	
}
let generator = colorGenerator();
//use generator.next();
//let values = Array.from(generator);
```

simulate ES7 asyc/await

```
co(function* (){ 
	let user = yield 	promiseAjax.get("url"); 
	handleUserData(user);	
});

async function() {
	let user = await promiseAjax.get("url"); 		handleUserData(user);
}
```

solution: gen store state between calls!

<pre>
//for WebSockets or polling
function *poll() {
	while（true) {
		let id = yield;
		req.get(id).accept('json').end(function(err, data){
		logger.next(data);
		})
	}
}
let poller = poll();
//use poller.next(); poller.next(id++)
</pre>

also call `.return()` or `.throw()`[inject into]

ppt地址 speakerdeck.com/btholt
##Lucky Draw
raffle.js漏洞百出

###3. Koa&Toa - Yan
vs express `req, res, next` P：异步流程控制

* koa `this, yield next` generator 函数 每个中间件可以分为前后两部分，都可以终止 P：流程复杂，第三方暴露
* toa vs koa `this, yield` 回到body `this.body`

异步原语：

* promise 标准接口，链式返回promise，任意多promise组合（多异步）
* thunk 新callback形式，返回thunk函数`(thunk(function(callback){ return Promise //thunk}))`
* generator `(function *() { yield promise })`

###4. AngularJS real-time app - loddit(BohanLi)@BearyChat
demo: 二维码刷新 meteor

api server -- RPC -- keep-alive ser

**Protocol**: XMPP / IRC , DDP(Meteor)[quick demo], CRDT(Swarm.js)

Ping/pong to check, use object as singleton, push for everything

Q：分组，消息等信息：普通Obj, Array；事件类型太多（命名，gonn）；

Q：交互性能：减少watch（改用ng-switch）；消息丢失处理：长连接保证，update时序更需要保证；

###5. PM2 - alexandre@Keymetrics
a process manager dedicated to Node.js [distributed!]

* https://keymetrics.io/  code:SHENJS2015
* https://github.com/Unitech/pm2

##NodeBot Session - ajfisher
NodeBots stack [<3] IO Plugin -- NodeJS (Johnny Five) -- HTTP -- clients 

* Adurino.cc IDE
* npm install jornny-five -g
* nodebotsday.com
* github.com/jsconfcn/nodebots-session

##light talk
1. undoZen (`require('co'); co.wrap(fun);`)
2. zhangwenli.com/Polyvia vs Naive Random Method
selected vectile (webgl sobel edge) -> dynamic -> center of mass 视频淘汰率
3. pmq20 `body.toString()`
4. socket 1000连接问题？ ulimit 设置！默认1024
5. 输入法 js
6. Zearlin/重鱼 

###6. NodeJS 分布式应用 - WenTianle@UCloud

C++ -> 分布式RPC架构（配置管理，服务自动发现，自动扩容，脚本）
umaster/zookeeper cluster 名称服务 -> rabbitmq/zookeeper 服务异构(扩容)/配置管理

node-amqp 3.4.1

**遇到的问题**：

1. nodejs [protobuf extension]
2. 异步编程效率[Fibers]
3.  nodejs 内存泄漏（网络框架中回调函数导致）
4.  异步超时处理（定时清理？）
5.  异步中日志记录（Fibers包装为task queue，带上其id）
6.  rabbitmq ha（ser HA-》cli HA ip选择来解决100s定时断问题，heart beat时间约定机制）
7.  rabbitmq ha 网络闪断导致节点分区
8.  zookeeper session expired 网络断导致，client重连成功后由于session key不同导致server要求再次重连，同时产生一次session expired回调 容易导致内存泄漏

###7. JS Frameworks - Eyalarubas.com@EF
全是吹水，外国人包装能力真强！

###8. 前端可视化技术 - pissang @Echarts.baidu
Canvas vs SVG

1. 性能考虑，特效绘制，像素操作
2. demo：尾迹特效(保存上一帧，另一个canvas alpha值调整)，热力图(像素操作)

问题: 没有图像对象(层级，样式，变换)，事件

解决：图像对象管理(借鉴SVG方法)，事件绑定(容器绑定代理，反向循环判断鼠标是否在某图像上，这时要将鼠标进行坐标变换-> 包围盒判断 / 路径的精确判断，js实现`isPointPath`，`isPointInStroke` 不需要重新构建路径)

latest: Path2d 

WebGL

1. 加速二维图像绘制
2. 问题：Shader计算动画小点位置 

demo: lambertShading

2D图形处理：Meth -> canvas surface -> canvas -> 2D objects

### 9. NERD Disco - TimPietrusky@
* web audio

<pre>
var audio_context = new AudioContext();
var audio_anay = audio_context.analyser();
</pre>

try *ndAudio* -> *ndVisualization* -> NeoMatrix Adafruit (nodejs server)

try **web mini api**