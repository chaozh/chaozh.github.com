title: Google的前端代码规范
tags:
  - CSS
  - html5
  - HTML标签
  - javascript
  - 前端规范
id: 114
comment: false
categories:
  - 开发规范
date: 2012-05-03 10:11:19
---

下面是个人总结的Google的前端代码规范，包括来自Google的[HTML/CSS规范](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml "html and css style guide")以及[JS规范](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml "google javascript style ")。

其中HTML与CSS部分翻译可以参考：[Google HTML/CSS Style Guide 谷歌代码风格指南](http://www.ryanbay.com/?p=285 "Google HTML/CSS Style Guide 谷歌代码风格指南")

JS规范终于有小组全部进行翻译啦，请参考：[Google Javascript代码规范](http://alloyteam.github.com/JX/doc/specification/google-javascript.xml)

## 总规范：

1.  忽略（Omit）协议：如 background: url(http://www.google.com/images/example); 应该写background: url(//www.google.com/images/example);以方便http与https协议切换，除非必须使用某个协议
2.  缩进使用两个空格，不要使用tab
3.  标签属性使用小写
4.  尾部不要留有空格，以防diff
5.  使用 UTF-8 (no BOM)，文档中也加入 &lt;meta charset="utf-8"&gt;
6.  TODO(contact) 以及TODO: action item，不要使用@@

## html与css

### HTML规范：

1.  使用html5的规范&lt;!DOCTYPE html&gt;
2.  注意html标签行为与语义，&lt;li onclick="goToRecommendations();"&gt;All recommendations&lt;/li&gt;应该写为&lt;a href="recommendations/"&gt;All recommendations&lt;/a&gt;
3.  多媒体标签向后兼容，记得加上alt属性
4.  由于utf-8编码的使用，某些记号无需转码，如&amp;mdash;, &amp;rdquo;, or &amp;#x263a;当然&lt;或是&amp;除外
5.  注意[HTML5 specification](http://www.whatwg.org/specs/web-apps/current-work/multipage/syntax.html#syntax-tag-omission)?中规定的可省略标签
6.  type属性不要使用！HTML5中已经规定为默认且无必要
7.  为block标签另起一行，li出现例外可以放在一行例：
<pre>&lt;blockquote&gt;
  &lt;p&gt;&lt;em&gt;Space&lt;/em&gt;, the final frontier.&lt;/p&gt;
&lt;/blockquote&gt;</pre>
<pre>&lt;ul&gt;
  &lt;li&gt;Moe
  &lt;li&gt;Larry
  &lt;li&gt;Curly
&lt;/ul&gt;</pre>
<pre>&lt;table&gt;
  &lt;thead&gt;
    &lt;tr&gt;
      &lt;th scope="col"&gt;Income
      &lt;th scope="col"&gt;Taxes
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;td&gt;$ 5.00
      &lt;td&gt;$ 4.50
&lt;/table&gt;</pre>

### CSS规范：

1.  正确使用缩写，例如navigation就可以缩写为nav，而author就不要缩写
2.  id与类名前不必加上标签类型
3.  属性尽量使用简写形式，如font或background等
4.  0后面不要加上单位
5.  小数前不要加上0
6.  url()中不要加入引号，例如@import url(//www.google.com/css/go.css);
7.  16进制尽量使用3位表示
8.  可以为项目加入前缀，例如.adw-help {} /* AdWords */
9.  分词使用“-”，如前例
10.  属性采用字典序申明，包括前缀如moz安排在webkit之前
<pre>background: fuchsia;
border: 1px solid;
-moz-border-radius: 4px;
-webkit-border-radius: 4px;
border-radius: 4px;
color: black;
text-align: center;
text-indent: 2em;</pre>

11.  {}里面都应该使用缩进，包括属性申明或另一个{}
12.  属性：与值之间用一个空格分开，例如font-weight: bold;
13.  为每个选择符及每个属性申明单独使用一行
<pre>h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}</pre>

14.  规则之间用一个空行分开

## JavaScript

1.  变量声明必须使用var
2.  Constants使用大写名称，并适当加上@const注释（如果名称已经体现就不用添加了），不要使用const关键字，例：
3.  记得使用分号结尾，错误例子：
<pre>//1.
MyClass.prototype.myMethod = function() { return 42; } // No semicolon here.
(function() {
// Some initialization code wrapped in a function to create a scope for locals.
})();
var x = { 'i': 1, 'j': 2 } // No semicolon here.
// 2\. Trying to do one thing on Internet Explorer and another on Firefox.
// I know you'd never write code like this, but throw me a bone.
[normalVersion, ffVersion][isIE]();
var THINGS_TO_EAT = [apples, oysters, sprayOnCheese] // No semicolon here.
// 3\. conditional execution a la bash
-1 == resultOfOperation() || die();</pre>
错误1\. ；错误2\. 解释为x={}[][]();即尝试执行对象的某个方法；错误3\. 解释为THINGS_TO_EAT = [...]-1 == resultOfOperation() || die();即赋值为die()；
4.  随意使用嵌套函数
5.  块级域中不用使用函数声明形式（不符合标准），应该这样处理：
<pre>if (x) {
  var foo = function() {}
}</pre>

6.  随意使用自定义exception
7.  原生类型不要使用包装对象的构造函数来进行声明，但是可以用来进行类型转换如：
<pre>var x = Boolean(0);
if (x) {
  alert('hi');  // This will never be alerted.
}
typeof Boolean(0) == 'boolean';
typeof new Boolean(0) == 'object';</pre>

8.  不推荐多重继承，可以使用[the Closure Library](http://code.google.com/closure/library/)
9.  闭包，注意dom元素的引用计数问题，例：function有对element的引用，而element也有对function的引用
<pre>function foo(element, a, b) {
  element.onclick = function() { /* uses a and b */ };
}</pre>

10.  eval只用于rpc解码json，注意
11.  不要使用with
12.  for in 不要用在遍历array上，只能用在object上
13.  关联数组如hash，map应该使用object实现而不是array
14.  多行的字符串字面量，应该使用+进行字符串拼接，而不是进行换行（不符合标准）
15.  array与object创建使用字面量而不是包装构造函数
16.  不要改变内置对象的prototype
17.  不要使用ie的条件注释
<pre>var f = function () {
    /*@cc_on if (@_jscript) { return 2* @*/  3; /*@ } @*/
};</pre>

18.  命名：`functionNamesLikeThis`,`variableNamesLikeThis`,`ClassNamesLikeThis`,`EnumNamesLikeThis`,?`methodNamesLikeThis`, and`SYMBOLIC_CONSTANTS_LIKE_THIS`.
19.  私有成员使用结尾下划线(trailing undercore)，保护成员不使用结尾下划线；可选参数以opt_开头，如果函数有可变参数，则最后一个参数命名为var_args，但不要使用他而要使用arguments数组进行访问；不要使用get和set关键字，如果要写则使用getFoo();setFoo(val);isFoo()的形式；
20.  永远要为项目添加一个命名空间，不要为外部引入的代码再引入自定义的成员，如果必要则应该使用外部代码暴露的api
21.  为过长的类型名使用别名引用，例如
<pre>/**
     * @constructor
 */
some.long.namespace.MyClass = function() {
};

/**
     * @param {some.long.namespace.MyClass} a
 */
some.long.namespace.MyClass.staticHelper = function(a) {
  ...
};

myapp.main = function() {
  var MyClass = some.long.namespace.MyClass;
  var staticHelper = some.long.namespace.MyClass.staticHelper;
  staticHelper(new MyClass());
};</pre>

22.  但是不要为命名空间使用别名；同时避免访问别名类型的属性，除非其是enum类型；也不要在全局域中使用别名，只能在函数内部使用
23.  文件命名统一使用小写".js"，同时推荐"-"而不是"_"
24.  自定义的toString方法必须保证1.正确性；2.没有副作用；否则很容易在assert时出错
25.  可以推迟初始化
26.  总是使用显式的域，例如不要依赖window一定存在于域中
27.  代码规范：基本是C++规范；”{“总是开始在一行上；可以单行初始化数组或对象；多行的数组或对象初始化应该要缩进2空格，属性与值都不用对齐；每行80单词，如果超过则注意函数参数的排放；函数中调用函数缩进方式例子如下：
<pre>if (veryLongFunctionNameA(
        veryLongArgumentName) ||
    veryLongFunctionNameB(
    veryLongArgumentName)) {
  veryLongFunctionNameC(veryLongFunctionNameD(
      veryLongFunctioNameE(
          veryLongFunctionNameF)));
}</pre>
匿名函数内缩进为两个空格；事实上，除了数组或对象初始化，匿名函数内，其他情况要么与上行左边对齐，要么缩进4个空格！；针对操作符缩进：
<pre>var x = a ? b : c;  // All on one line if it will fit.

// Indentation +4 is OK.
var y = a ?
    longButSimpleOperandB : longButSimpleOperandC;

// Indenting to the line position of the first operand is also OK.
var z = a ?
        moreComplicatedB :
        moreComplicatedC;</pre>

28.  不要在如delete，typeof，void或是return，throw后面添加使用括号（Parentheses）
29.  字符串相对于双引号，推荐使用单引号
30.  可见性规范：推荐使用jsdoc的@private以及@protected；@private与c语言中static类似，同时子类不能访问；@protected中子类可以访问