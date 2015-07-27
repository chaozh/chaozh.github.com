title: Wordpress摘要相关开发
tags:
  - WP主题
id: 406
categories:
  - WordPress开发
date: 2013-02-05 03:32:22
---

主题开发时又出现使用more标签显示摘要与使用excerpt摘要相关filter出现混淆的问题。

Wordpress主题开发中有两种显示摘要的方法，一种是利用more标签`<!--more-->`，该方法需要作者在文章适当位置中插入该标签；另一种是正宗的摘要使用函数`the_excerpt`显示，该方法既可以在文章编辑页面中填入，也会自动生成摘要。

第一种方法可以在调用函数`the_content`时通过传入参数来定义“阅读更多”链接样式，而与excerpt摘要相关filter完全无关。

第二种方法可以使用多种filter进行“阅读更多”链接定义，使用`excerpt_length`来定义自动生成摘要的长度(系统默认为55)，使用`excerpt_more`定义自动生成摘要时“阅读更多”链接的样式，使用`get_the_excerpt`定义手动添加摘要时“阅读更多”链接的样式。