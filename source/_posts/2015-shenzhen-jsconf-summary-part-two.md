title: 2015 shen JSconf总结（第二天）　
date: 2015-07-31 19:03:16
categories:
- 前端
tags:
- nodejs
- javascript
- WebGL
- karma
---
第二天干货非常多，全是实战经验，基本没有什么技术介绍。

##Part Two 2015-07-12

###1. 七牛前端测试实践 - 马逸清
angularjs dom隔离，测试不需mock；依赖注入可以mock (service,  controller) 可以使用stub

stub适合返回值依赖，mock行为验证

官方jasmine 
View状态测试

**phantom，sinon(stub)，fixture(karma)**

###2. Node Profiler - 朴灵@阿里云alinode
hack了部分v8代码，跟CPU Profile相关

* 常用工具: benchmark.js，node-webkit-agent，ab/wrk(压力测试)，--prof & mac-tick-processor(v8)
* v8以JIT方式执行js(产出机器码)，以函数为单位处理
Crankshaft架构(可以运行时优化 fulCompiler -> optimizeCompiler) P：bailout, deoptimize
* Profiler： 更多函数状态，优化建议(v8能否优化) http://alinode.aliyun.com
* (program) 系统空转

? 第一次访问测试
###3. P2P pipes - mafintosh(dat)

demo: multiplayer game(socket)
websockets, webrtc

**ndjson, nc, crypto, dupsh(two app in-out pipe), airpaste(udp multicast with pipe), blecat(blutooth with pipe), webcat(webrtc with pipe)

###4. 微服务架构下的服务通信 - 老雷leizongmin @ucdok.com/ superid.me一登
**后台Node逻辑**
SDK - 多进程？- API服务器（人脸识别模块）

=》 SDK - API服务器 -人脸识别进程（多线程）

=》 - 更多应用（后台微服务架构）

--> 服务注册（消息队列总线式 / P2P）+远程调用

**实现原理**

总线可以用redis实现，通信也用redis订阅机制（服务提供者可以动态加入）
vs amqp-rpc， eureca.io

未来：备份切换，数据量太大

服务发现 + 服务通信

ppt地址 github.com/leizongmin/clouds

**etcd + k8s + docker**

###5. 前端服务化之路 - Herman@阿里CXDC 客服中心
 
 前后端分离-》前端给后端提供接口（服务化）-》web component
 **flipper.js 支持IE8以上 -- webcomponents.js 更好的隔离性
 
###6. JS lang - hax贺诗俊@百姓网
NodeJS + CommonJS -> ES6 module( class, arrow function[this bind], map/set, for..of..[for in] )

WebAssembly

**babel(es6 compiler)**

###7. JS on Fiber - 庄恒飞@孢子社区fibjs-> 那么社区named.cn
fibjs vs nodejs， Fiber-Driven，No Callback

* Module lib: CommonJS，async require
* coroutine.parallel 【改变nodejs单线程】多CPU利用率
js，用
`brew install fibjs [http://fibjs.org]`
fibjs底层多种线程(fiber) -> js线程-》work线程池【实际改动了v8下一层，解决了nodejs的callback与计算密集】

##Lighting Talk
1. tangrui.net (js in JVM: 1.6 ringo 1.8 nashorm(vert.x/avatar.js)) cde.io
2. wangyan @oneapm.com (npm2dot) 转换dot格式
3. dafeng @strikling.com "GraphQL & Relay": restful api问题：依赖多次发送->custom endpoint-》 GraphQL帮忙管理
4. homlinju @fontmin js方案的字体最小化
5. 一狂 @阿里cxdc www.zouyesheng.com angular问题：不单单使用，模块化加载，自己的渲染机制，怎么合作？模块内定义dir最后手动触发bootstrap
6. EvanYou @vue webpack vue-element.js 
7. xeodou crazyapp.net/#/explore

###8. Persistent data structure in JS-NiYue@splunk
persistent data structure [like COW] -> concurrency
real good: 

1. performance（判断状态是否变化）
2. features on copy（undo/redo，recording/reply，time traveling） 

实现策略：COW -> structual sharing
问题：内存消耗，内存回收

**mori, Immutable** 

**一页千级dom元素？**
LightTable演示

###9. WebIDE with React - hulufei @coding.net 
React - Flux - karma

Components: `<WebIDE/>`

```
var App = React.createClass({
	render: function() {
		<div><Menu/><Tree/></div>
	}
})
```
Flux: Dispatchor -> store -> view -> action
`<Editor/>`与`<SettingModel/>` 通信：
SettingStore -> SettingAction

Addons: store或action引用API层

**roit, ace rather than codemirror**
QA：store划分标准：组件需要，共享； 数据放store里面方便做数据缓存，返回promise；Flux写太多模板代码；

###10. WebGL & WebVR - Martin @Zurich - archilogic.com @g33konaut

faces + vertices -> GL buffers -> shader code -> print

```
var renderer = THREE.WebGLRenderer();
var box = new THREE.Mesh(
	new THREE.BoxGeometry(100, 100, 100),
	new Three.MeshBasicMaterial()
);


scene.add(box);

var light = new THREE.PointLight()
light.position.set();
renderer.render(function(){
	box.rotation.y += Math.PI / 200;
});
//OBJMITLoader()

//VR
var vrEffect = new THREE.VREffect(render),
	vrControls = new THREE.VRControls()
```

###11. maintainable nodejs - 死马 不四@天猫前端,dead-horse@github koa
提供固定的输出

异步try-catch -> promise 可以传递error
默认的错误监听函数，可以利用事件方式让外层感知

**bluebird, co**