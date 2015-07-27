title: python技巧：逆序循环
tags:
  - python
id: 593
categories:
  - 后端语言
date: 2013-08-14 22:30:04
---

Python可以很简单实现C语言中for(i = len; i&gt;=0; --i)的逆序循环，而且有不止一种写法：

第一种，`for i in range(len, -1, -1)` ，简单易懂

第二种，`for i in range(0, len+1)[::-1]`，利用list的[start:end:step]排序功能，当然在大数组下性能堪忧