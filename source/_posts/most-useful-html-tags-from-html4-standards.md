title: 前端需要熟练掌握的html标签
tags:
  - HTML标签
id: 484
categories:
  - HTML技巧
date: 2013-03-08 07:24:01
---

这是我读《html mastery》一书的总结，这本大概写于2006年的书主要讨论的是html4的标签，而现在2009年w3c于1月22日发布了最新的html5草案，opera称其将使flash技术变得可有可无，相信不久后普及的html5将给web设计带来些重大变化，但现在了解些html4的基础知识显然是必要的，何况听说html5会兼容些本文提到的标签，似乎还是有价值的。

<span style="color: #ff0000;">本文原写于2009年五月28日，而HTML5标准已经从去年开始大放异彩，所以这里算是个回顾吧。</span>

## 1.常用元素

### 1.1 div与span

这是用得最多的两个标签，以后会有专文总结如何使用好他们，现在必须知道的是前者是block元素，后者是inline元素；而block元素与inline元素区别正如名字告诉我们的那样：前者是所包含的内容是一个整体，几个block元素间垂直堆叠，强制后面元素另起一行；而后者，几个inline元素水平排列，相互间只有水平方向上的边距设置才会有效，padding-top，margin-bottom等竖直格式设置会被忽略。不添加css，前者无法并放，后者无法堆叠。即span内部是不能放div的。

<span style="color: #ff0000;">在HTML5中引入了更具有语义的section标签，以及更多具体语义的如article，header，footer，aside等标签来替换千篇一律的div</span>

### 1.2\. doctype

它的最实际与重要的用途是提醒浏览器按照标准模式（standard mode）而非怪异模式（quirks mode）来解析html文档，XHTML1.0有三种可用:

```
<!DOCTYPE html PUBLIC "-//W3C/DTD XHTML 1.0 Strict//EN" "http://www.w3c.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<!DOCTYPE html PUBLIC "-//W3C/DTD XHTML 1.0 Transitional//EN" "http://www.w3c.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<!DOCTYPE html PUBLIC "-//W3C/DTD XHTML 1.0 Frameset//EN" "http://www.w3c.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```
XHTML1.1就只有一个形式：

```
<!DOCTYPE html PUBLIC "-//W3C/DTD XHTML 1.1//EN" "http://www.w3c.org/TR/xhtml1/DTD/xhtml11.dtd">
```

## 2\. 文档标签

### 2.1  p,address与hx，blockquote

`<p>`是用来表示新段落的，而不是表示不间断的空格实体，那该用css的padding-bottom实现。

`<address>`只用来显示与本文档有关的联系信息，是一个block元素，hCard微格式会经常用到。

`<hx>`其实是`<h1>`到`<h6>`，记得把`<h1>`留给最重要的标题，一般只出现一次，还有此类标签可能以后会被统一的`<h>`取代，趁能用赶快学着用吧。（<span style="color: #ff0000;">似乎目前没有被取代的打算</span>）

`<blockquote>`用来说明块引用，有一个cite属性让作者推荐来源

### 2.2 ul，ol与dl

无序，有序（按字母或数字顺序）以及定义（表示对话也可以）列表，列表项使用`<li>`元素标记，不能含block元素，即`<hx>`不能包含其中。

### 2.3  a 与link

`<a>`没啥好多说的，记得有<a href="#top"></a>的用法就好，参考文章http://www.jimthatcher.com/skipnav.htm

`<link>`可有得研究他和`<a>`都有两个重要属性：rel以及rev，rel指出该文档与href指向的链接关系类型，rev则将两对象方向互换，可选类型有：alternative，如果是可选译文，则与lang属性一起用；如果是可选媒介，则用到media属性。

stylesheet，用得多了。start，起点；pre，next，上下一份文档；copyright，指出版权声明，等多了去。还可以是tag，有助于标签网站识别，如<a href="http://technorati.com/tag/ipod/" ref="tag">ipod</a>
还有title，tabindex，acesskey属性，理智应用能加强访问性，但be careful。

### 2.4  del与ins

用于表示文章修订，有cite属性与datatime属性表示修改原因与时间。

## 3\. 表示型标签

### 3.1  i与b

并不推荐使用了，有时可能有用：表示_斜体_，**粗体**，可以使用`span`代替他们，或是使用`em`及`strong`

### 3.2\. hr，pre，sup与sub

`hr`即水平标尺（绝不是边框），`pre`则是留白，`sup`上标，`sub`下标，都是弥补css用的

&nbsp;

## 4\. 短语元素及图像媒体

### 4.1 em,strong

不是用来使文本变斜加粗的，而起强调作用，如果有读屏器，就会发现语速变化，比`<i>`或`<b>`有用

### 4.2 cite，dfn

表示引用与定义的标签，记得ref=“glossary”吗？

### 4.3 code，var，samp，kbd

前面两个用来显示代码，后一个描述输出结果，`<kbd>`指出键盘按键。<span style="color: #ff0000;">需要指出code标签很坑人，如何正确使用详见[<span style="color: #ff0000;">此文</span>](http://www.chaozh.com/what-web-front-developer-must-know-about-code-displaying/ "代码展示的相关知识")</span>

### 4.4 abbr，acronym

缩略文章或词，在title属性给出完整版本，缩写词的区别对东方人来说，不怎么适应

### 4.5 img与object

除非真的是显示图片，否则网页布局该用css背景；object是嵌入标签，用起来麻烦，还是自动生成吧

## 5\. 表格table

### 5.1  caption，th，thead，tfoot，tbody

表格的标题，标题行，表头区等，都不知道除tr，td外还有这么多用于表格

span属性仍然十分必要，

```
<thead>

<tr>

<th>sth</th>

<th>sth</th>

</tr>

</thead>
```

就该如此写

### 5.2 colgroup，col

在表头区分格需要这两个标签，并不实用，不如用scope属性值，rowspan，colspan等属性也可，具体哪种好现在不明。

## 6\. 表单元素

### 6.1  form与input，label

用得太多了，就说form的enctype属性与input有file类型可用有关，而reset类型还是别再用了。

### 6.2 textarea，fieldset，legend

前者是大面积输入的绝佳标签，后者有利表单划分，<legend>是块标题。

<span style="color: #ff0000;">web form2.0将给这些来个大变化，趁能用尽量用对吧。</span>我也不会乱用div和span了，供选择的标签有这么多。