title: 制作Bookmarklet书签
tags:
  - javascript
categories:
  - 前端
date: 2012-02-19 11:32:49
---

基本模板就是这样，主要是javascript:起了作用，导致代码执行。
<pre rel="javascript">javascript:(function(){

if(window.bookmarklet!=undefined){bookmarklet();}
else{document.body.appendChild(document.createElement('script')).src='http://YOURURL/bookmarklets.js';}

})();</pre>