title: firebug的命令行
tags:
  - console
  - firebug
id: 348
categories:
  - 前端经验
date: 2012-12-11 11:00:18
---

<span style="font-family: Verdana;"><span style="color: #333333;"><span>控制台命令行状态下:</span></span><span>

<span style="color: #ff0000;">$(id)</span>
<span style="color: #3d85c6;">Returns a single element with the given id.</span>
<span style="color: #333333;">按指定ID返回一个元素</span>

<span style="color: #ff0000;">$$(selector)</span>
<span style="color: #3d85c6;">Returns an array of elements that match the given CSS selector.</span>
<span style="color: #333333;">以CSS选择器一样的语法进行选择, 并返回匹配的元素数组</span>

<span style="color: #ff0000;">$x(xpath)</span>
<span style="color: #3d85c6;">Returns an array of elements that match the given XPath expr<wbr>ession.</wbr></span>
<span style="color: #333333;">以XPATH语法选择, 并返回匹配的元素数组</span>

<span style="color: #ff0000;">dir(object)</span>
<span style="color: #3d85c6;">Prints an interactive listing of all properties of the object. This looks identical to the </span>
<span style="color: #333333;">输出一个对象的所有属性.(个人认为它比DOM查看的要稍准确些)</span>

<span style="color: #ff0000;">dirxml(node)</span>
</span></span><span><span style="font-family: Verdana;"><span style="color: #3d85c6;">Prints the XML source tree of an HTML or XML element. This looks identical to the view that
you would see in the HTML tab. You can click on any node to inspect it in the HTML tab.</span>
<span style="color: #333333;">打印HTML或XML指定节点的源码树, 其功能和控制台按钮左侧的HTML功能相同.</span>

<span style="color: #ff0000;">cd(window)</span>
</span><span style="font-family: Verdana;"><span style="color: #3d85c6;">By default, command line expressions are relative to the top-level window of the page. cd()
allows you to use the window of a frame in the page instead.</span>
<span style="color: #333333;">默认命令行是在顶级的WINDOW下工作, CD命令则会进入指定框架内的WINDOW.</span>

<span style="color: #ff0000;">clear()</span>
<span style="color: #3d85c6;">Clears the console.</span>
<span style="color: #333333;">清除当前控制台上的信息.</span>

<span style="color: #ff0000;">inspect(object[, tabName])</span>
</span><span style="font-family: Verdana;"><span style="color: #3d85c6;">Inspects an object in the most suitable tab, or the tab identified by the optional argument
tabName.
The available tab names are "html", "css", "script", and "dom".</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">查看一个指定对象或DOM节点的信息. 此对象或节点可以用第二个参数来指定其所指类型.
例1: inspect(Exam, "script")
查看javas<wbr>cript代码内的Exam对象
例2: inspect(document.body, "html");
查看html源码树的BODY节点.</wbr></span>

<span style="color: #ff0000;">keys(object)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Returns an array containing the names of all properties of the object.
以数组形式返回一个对象所有属性的键值也就是属性的名字, 但不会返回其值</span>

<span style="color: #ff0000;">values(object)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Returns an array containing the values of all properties of the object.
以数组形式返回一个对象所有属性的值.</span>

<span style="color: #ff0000;">debug(fn)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Adds a breakpoint on the first line of a function.
在指定的函数定义处添加一个断点.</span>

<span style="color: #ff0000;">undebug(fn)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Removes the breakpoint on the first line of a function.
取消对一个函数的断点.</span>

<span style="color: #ff0000;">monitor(fn)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Turns on logging for all calls to a function.
监视调用指定函数时的参数等相关信息.</span>

<span style="color: #ff0000;">unmonitor(fn)</span>
</span><span style="font-family: Verdana;"><span style="color: #333333;">Turns off logging for all calls to a function.
取消对一个函数的监视.</span>

<span style="color: #ff0000;">monitorEvents(object[, types])</span>
</span><span style="font-family: Verdana;"><span style="color: #3d85c6;">Turns on logging for all events dispatched to an object. The optional argument types may
specify a specific family of events to log. The most commonly used values for types are
"mouse" and "key".
The full list of available types includes "composition", "contextmenu", "drag", "focus",
"form", "key", "load", "mouse", "mutation", "paint", "scroll", "text", "ui", and "xul".</span>
<span style="color: #333333;">监视一个对象所发生的事件, types选项可以指定事件类型.</span>

<span style="color: #ff0000;">unmonitorEvents(object[, types])</span>
<span style="color: #3d85c6;">Turns off logging for all events dispatched to an object.</span>
<span style="color: #333333;">取消对一个对象的事件监视.</span>

<span style="color: #ff0000;">pro<wbr>file([title])</wbr></span>
</span><span style="font-family: Verdana;"><span style="color: #3d85c6;">Turns on the JavaS<wbr>cript profiler. The optional argument title would contain the text to be
printed in the header of the pro<wbr>file report.</wbr></wbr></span>
</span></span><span style="font-family: Verdana;"><span><span style="color: #333333;">对脚本的指定代码片断进行分析并产生报告, 分析结果包括函数的调用次数, 调用时间, 最大调用时间,
最小调用时间等. pro<wbr>file([title])为代码分析的开始处. title参数是提示的标题, 也可以为空.</wbr></span>

<span style="color: #ff0000;">profileEnd()</span>
<span style="color: #3d85c6;">Turns off the JavaS<wbr>cript profiler and prints its report.</wbr></span>
<span style="color: #333333;">指定的代码片断结尾处. 请查看pro<wbr>file([title]).</wbr></span></span></span>