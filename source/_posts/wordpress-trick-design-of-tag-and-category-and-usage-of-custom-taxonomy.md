title: tag与category的使用与自定义标签的设计
tags:
  - WP主题
  - WP插件
  - WP设置
id: 165
categories:
  - WordPress开发
date: 2012-06-05 11:02:25
---

WordPress中存在两种类型的分类：一种是tag，另一种是category。这两种分类的区别在于: category通常是有层次的，而tag是平行平等的。

一些著名博客如Smashing Magzine以及Nettut+是如何安排使用tag和category的呢？

最新Smashing Magzine主题是轻博客类型，完全放弃有层次的category，而全部采用平等的tag。

而传统博客类型的Nettut+基本采用category，其URL结构为/articles/javascript或是/tutorials/ 。tag用来标记类型如video或tips，该主题甚至可以结合两者进行查询。

WordPress中可以使用函数`register_taxonomy注册`自定义标签，区分是category类型还是tag类型的关键就是参数hierarchical。