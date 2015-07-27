title: JavaScript实用代码片段
tags:
  - javascript
  - 算法
id: 969
categories:
  - JavaScript技巧
date: 2015-06-30 22:59:57
---

最近抽空在忙[小项目](http://chaozh.github.io/RiskDemo)，总结了下一些项目中碰到的常见问题，搜集对应的实用JS代码片段，跟大家分享。

## 1.产生限定范围内不重复的随机数

在深js也碰到了完全相同的问题，即抽奖程序的核心算法，结果是bug频出。这里给出[js抽奖程序](https://github.com/jsconfcn/raffle)，大家去鄙视一下。
一种思路是在给定大小数组中每次抽取一个随机位置的数进行剔除，这个程序的问题是必须始终维护抽取的数组，好处是不会重复抽取：
<pre>var originalArray=[];
for (var i=0;i&lt;len;i++) { 
	originalArray[i]= i; 
} 
var getRandom = function(){
	var index=Math.floor(Math.random()*originalArray.length); //随机取一个位置 
	var value = originalArray[parseInt(index)];
	originalArray.splice(index,1);
	return value;
}
</pre>
有什么其他好方法不妨分享一下。

## 2\. 数字保留两位小数

数字进行截断保留两位小数只想到如下方法
<pre>floatNum = Math.round(floatNum*100)/100;</pre>
但是还有情况是仅展示两位小数，这时只要用原生的API方法就可以了
<pre>floatNum.toFixed(2)</pre>

## 3\. 触发change事件

这是在使用AmazeUI中按钮组时碰到的问题，按钮组并没有对应的js封装，所以点击div模拟的按钮并不能触发实际单选按钮的change事件，因此需要jquery来手动触发。估计Bootstrap应该有进行更好的封装吧。
<pre>$button.children().attr('checked',true).trigger("change");</pre>
这里使用`children`仅是为了获得包含在外层`div`下的单选按钮，还得设置check状态

## 4\. 单选按钮选择信息获取

按钮组中究竟选择了哪一个可以使用下面代码获得
<pre>$('input[name="radio-options"]:checked').val()</pre>

## 5\. 中文编解码

在URL中传递中文就需要进行URI编码转为16进制的数字表示
<pre>location.href = "result.html?u="+encodeURIComponent(name)+"&amp;b="+invest_status.profit+"&amp;r="+invest_status.rounds;</pre>
展示也得使用相应的URI函数解码进行
<pre>user = decodeURI(name);</pre>
发现不少浏览器现在已经支持直接在URL中插入中文，估计这里有些变化了。

## 6\. 转换为数字类型

两种方法，一种是
<pre>option = parseInt(IntNumStr);</pre>
另一种是
<pre>option = Number(IntNumStr);</pre>
注意单选按钮的选择值就可以用这个方法进行转换

## 7\. 浏览器url信息处理

主要是处理GET请求的分隔符"&amp;"与值的获得，也就是运用`.split("&amp;")`和`.split("=")`
<pre>infos = url.split('&amp;'), user = (infos[0].split('='))[1];</pre>
搜到一个大而全的[处理工具](https://github.com/websanova/js-url)

## 8\. 网页title设置

直接给`document.title`赋值即可

## 9\. 异步加载与性能优化

promise

## 10.文件上传处理

<pre><input id="input" type="file" />
window.Blob slice
file</pre>

## 11\. 多场景管理

## 12\. 简单模板与双向绑定