title: Modernizr使用
tags:
  - html5
  - javascript
  - 前端库
  - 脚本加载
id: 199
categories:
  - 前端经验
date: 2012-06-26 11:08:46
---

Modernizr作为开发HTML5必要的js工具，提供有以下多种功能

## 1.css

可以通过feature检测（包括html5元素如canvas，或是css3属性如border-radius）为html的加入相应的类，对于不支持的feature则加入以no-为前缀的类。

可以为html加入名为“no-js”的类，这样即使没有js环境来执行Modernizr，也提供了相应的fallback类，而一旦Modernizr执行，就会自动将no-js类替换为js类名，并且加入各种feature检测结果类名。

## 2.js

加入Modernizr全局变量，可以通过调用 Modernizr.&lt;featurename&gt;来检测是否支持某个feature

## 3.yepnop

可以将YepNop功能引入，通过调用Modernizr.load()实现按需加载
<pre>&lt;script src="modernizr.js"&gt;&lt;/script&gt;
&lt;script&gt;
Modernizr.load(
  test: Modernizr.inputtypes.date,
  nope: ['http://ajax.useso.com/ajax/libs/jquery/1.4.4/jquery.min.js', 'http://ajax.useso.com/ajax/libs/jqueryui/1.8.7/jquery-ui.min.js', 'jquery-ui.css'],
  complete: function () {    
    $('input[type=date]').datepicker({ dateFormat: 'yy-mm-dd' }); 
  }
});
&lt;/script&gt;</pre>
YepNop很好使用，基本参考下面的帮助即可：

    yepnope([{
      test : /* boolean(ish) - Something truthy that you want to test */,
      yep : /* array (of strings) | string - The things to load if test is true */,
      nope : /* array (of strings) | string - The things to load if test is false */,
      both : /* array (of strings) | string - Load everytime (sugar) */,
      load : /* array (of strings) | string - Load everytime (sugar) */,
      callback : /* function ( testResult, key ) | object { key : fn } */,
      complete : /* function */ }, /* ... */ 
    ]);