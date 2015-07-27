title: font-weight相关
tags:
  - CSS
  - javascript
id: 257
categories:
  - CSS技巧
date: 2012-10-11 12:19:08
---

font-weight算是CSS中最常用的属性之一，值可以使用两套系统：

一种是：lighter，normal，bold，bolder

另一种是：100~900，其中需要关心的是400代表normal，600代表semi bold（最常用），700代表bold

另外需要注意的是JS调用对应的DOM属性object.style.fontWeight的数值也只能是100~900，否则浏览器会提示invalid number并且返回值为空