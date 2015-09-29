title: 漫谈Web Components技术
tags:
  - html5
  - JS模板
categories:
  - 前端
date: 2014-08-10 21:00:00
---

第一次听说Web Components相关技术是在2013年从[Shadow DOM 101](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/ "Shadow DOM 101")这篇文章得知，只当是个模板技术的浏览器实现。谁想在Google IO 2014竟然将基于Web Components技术的Polymer专门做了个专题进行大篇幅介绍，其中Custom Element加上Imports的组合概念堪称惊艳，有种前端界的MapReduce的感觉。其实Google早在13年就已经花了部分精力对[Web Components技术进行了介绍](http://www.webcomponentsshift.com/#1)(链接是当时演讲里的ppt，部分协议标准已经产生了变化)，只是未像IO 14一样做专题介绍。

比如：
![](https://cms-assets.tutsplus.com/uploads/users/53/posts/21524/image/gangnam.gif)
[Live demo](http://playground.chaozh.com/gangnam.html) - 自己实现了一个，得用现代浏览器(推荐webkit内核)打开才行

使用Custom Element创建后可以通过HTML Import加载，然后直接使用
<pre>&lt;x-gangnam-style&gt;&lt;/x-gangnam-style&gt;</pre>

## 1\. 规范现状

Web Components技术由W3C规范进行定义，实际由四种技术组成：

*   Custom ELements，可以创建自定义html标签元素，而且支持对现有html元素进行扩展，从而自定义新的API
*   HTML Template, 使用template标签包含html相关代码片段，作用行为基本类似于JS模板引擎里的视图(View)
*   Shadow DOM,，可以创建对外隐藏的web片段，关键是支持独立的样式，不会影响所在文档内的其他样式
*   HTML Imports, 提供对模板或者自定义元素这些资源的加载支持，可以像引用JS或CSS一样引用HTML组件
截至2014年8月17日，四个技术标准中HTML Template已经从Web Components规范中移除，并加入到HTML5标准中，这意味着该技术协议已经处于完成状态，不会再出现改动。Custom ELements也进入“Last Call”阶段，今后改动可能性也不大了。另外两个仍然存在修改的可能，因此下面的介绍只能当做谈资了解了解罢了。有关Web Components的规范现状与详细定义可以在这个[链接](http://www.w3.org/TR/#tr_Web_Components)中找到。

## 2. Custom ELements

原本Custom ELements设计是通过新增的element标签来实现自定义html标签元素，但由于种种问题，最终删去了相关规范。目前只能通过`document.registerElement(tagName, prototype)`方法来定义标签。该方法提供元素原型继承的接口，可以通过`Object.create`方法创建元素作为prototype传入。在这个gangnam-style示例中就使用：

```js
var gangnamProto = Object.create(HTMLElement.prototype);

  gangnamProto.createdCallback = function(){
     // deal with element view create 
  }

  gangnamProto.attachedCallback = function(){
    // deal with event bind
  }

  document.registerElement("x-gangnam-style", {
    prototype: gangnamProto 
  });
```

接着就可以直接在html文件中使用x-gangnam-style标签了

## 3. HTML Template

该技术规范已经加入到HTML5规范中，成为一个新增的HTML5标签。使用非常简单，直接在html文件中添加template标签即可。该标签在浏览器中会被渲染处理，但不会展示，因此会比以前使用script做js的模板容器在效率上会高一些。需要注意的是获得template标签中的内容，需要使用其content属性，该属性返回`documentFragment`类型。而将content内容插入到目标元素中后会导致内容丢失，因此需要使用cloneNode方法进行拷贝保证其他元素仍然可以使用这个template中的content内容，或是使用`document.importNode`方法进行拷贝。在这个gangnam-style示例中就使用：

<pre>
&lt;template id="gangnam-style-tmpl"&gt;
&lt;style&gt;
...
&lt;/style&gt;

&lt;div id="maia-signature"&gt;
&lt;div id="sig" class="zg-sig"&gt;
 &lt;div id="psydroid" class="zg-psydroid"&gt;
 &lt;img alt="" class="zg-psydroid-head" src="http://www.google.com/zeitgeist/2012/images/psydroid-head.png"&gt; 
 &lt;img alt="" class="zg-psydroid-body" src="http://www.google.com/zeitgeist/2012/images/psydroid-body.png"&gt; 
 &lt;img alt="" class="zg-psydroid-arm-l" src="http://www.google.com/zeitgeist/2012/images/psydroid-arm-l.png"&gt;
 &lt;img alt="" class="zg-psydroid-arm-r" src="http://www.google.com/zeitgeist/2012/images/psydroid-arm-r.png"&gt;
 &lt;img alt="" class="zg-psydroid-leg-l" src="http://www.google.com/zeitgeist/2012/images/psydroid-leg-l.png"&gt;
 &lt;img alt="" class="zg-psydroid-leg-r" src="http://www.google.com/zeitgeist/2012/images/psydroid-leg-r.png"&gt;
 &lt;/div&gt;
&lt;/div&gt;
&lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
 ...  
gangnamProto.createdCallback = function(){     
   //deal with element view create     
   var thisDoc = document.currentScript.ownerDocument;  
   var tmpl = thisDoc.querySelector("#gangnam-style-tmpl");               
   //shadow.appendChild(tmpl.content.cloneNode(true)); also works!
   shadow.appendChild(document.importNode(tmpl.content, true)); 
} 
...  
&lt;/script&gt;
</pre>

这样就可以在Custom Element中使用template中已经定义好的html结构与样式。当然为保证接口与样式的封装性，这里还将HTML Template与Shadow DOM结合了起来。

## 4\. Shadow DOM

这个技术规范堪称Web Components的灵魂所在，而且早已经用于HTML5标签技术中，只是不曾暴露接口给JS调用罢了。例如HTML5中的video标签就包含了Shadow DOM。现在可以在任何DOM节点上创建Shadow DOM，使用这个技术的好处在于Shadow DOM可以完全封装其中的dom结构与style样式，使得相关信息对用户透明。特别是包含在其中的样式，完全不会影响到其他元素，从而使得形成独立UI组件成为可能。Shadow DOM创建起来非常简单，调用createShadowRoot即可：
<pre> gangnamProto.createdCallback = function(){
   ...
   var shadow = this.createShadowRoot();
   shadow.appendChild(document.importNode(tmpl.content, true));
 }
</pre>
同时Shadow DOM还提供`<content>`标签，该标签可以使用rel属性让浏览器选择性的填充其中的内容。（<del>不知为何该标签在Chrome 36里失效 </del>实际没有失效，只是dev tool展示比较奇怪，让本人误会了）
为方便CSS样式进行Shadow DOM的匹配选择，该技术还提供了多个CSS相关选择符，包括::shadow，/deep/，::host，::host-context，::content等。
目前该技术仍然在修订中，所以还存在不少变数。

## 5\. HTML Import

在前面将Web Component封装在一个HTML文件中后，如何引用该组件并重复运用就成为了问题，这时就需要使用HTML Import技术。该技术使用起来很方便，一句话即可：
<pre>&lt;link rel="import" href="./x-gangnam-style.html"&gt;</pre>
同时可以使用`rel=import`获得该link节点：
<pre>var link = document.querySelector('link[rel=import]');</pre>
目前该技术也仍然在修订中，而且很多细节都在变更。

## 6\. 总结

这些技术目前还没有浏览器完全支持，而且规范还在修改中，因此完全不存在工业界使用的可能性。但是这些技术其实早就在浏览器中不断使用，只是没有暴露相关接口而已，规范需要决定的只是怎么暴露，如何让开发者方便使用而已，因此这些Web Component技术的普及可谓是必然。当然，Web组件的最终形态肯定不是直接使用这些规范，多看看微软GUI组件的发展轨迹就可以知道基于新的Web Component规范的Web组件未来还有很长的路要走。