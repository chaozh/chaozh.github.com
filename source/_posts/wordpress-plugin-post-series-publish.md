title: WordPress专题系列文章管理插件Post Series发布
tags:
  - WP插件
id: 326
categories:
  - WordPress开发
date: 2012-11-04 13:21:58
---

本人第二个WordPress个人制作插件Post Series也完成了。功能是显示一个系列文章的列表，需求很常见，但是搜到的插件要么功能过于复杂，要么就得[收费](http://codecanyon.net/item/post-series/239344)。

当然这个插件主要的展示代码逻辑也是来自一个Nettuts+教程《[Adding Post Series Functionality to WordPress With Taxonomies](http://wp.tutsplus.com/tutorials/plugins/adding-post-series-functionality-to-wordpress-with-taxonomies/)》，但是本人将其发扬光大，为其加入了编辑器按钮及界面，还加入插件设置界面以及相应小工具，而且可以设置自动在文章页面展示系列文章列表，彻底省去添加short code的烦恼。

目前可定制性算是够强，下一步是参考其他Series插件继续完善一些功能。当然目前的第一步是发布到WordPress官网供大家下载。

首先是学习免费插件EG-Series

优点：其同样支持编辑器按钮（支持系列文章展示short code与展示所有系列的short code），可以用拖放的方式管理系列下的文章，为管理员导航栏增加菜单。居然使用的各种short code及tax设置都与本人写的通用。。。要好好学习。。。

缺点：默认样式很一般，勉强可以用

其次是参考免费插件Organize Series，其可以升级为付费版，只有付费版才支持short code与custom post type

优点：普通版可以完整设置展示的template（全部HTML代码都可以更改，这个定制性过强）设置使用默认css样式表，可以为系列添加特色图标，为文章管理页面加上系列一栏（展示系列名称与文章数目），可以进行filter筛选，同样可以为文章自动添加系列展示等（设置使用checkbox input可以学习），可以改变文章在系列中的序数（比较麻烦）。

缺点：默认样式很丑，默认展示位置很诡异，根本不适用，果然是逼人用付费版啊。

优化点Todo：

1.  <del>文章管理页面加上系列的filter</del>，系列支持特色图标
2.  <del>设置是否使用默认css样式表，其他都改用checkbox来展示</del>
3.  增加系列展示的各种设置，如是否显示thumbnail或excerpt
4.  <del>增加系列内的导航，增加show all按钮</del>
5.  系列展示可以使用list或select，改变sort order
6.  <del>拖放的方式管理（同时可以参考收费插件）*</del>
7.  设置short code的名称*
8.  <del>设置自动展示的页面类型与位置及样式设置*</del>
9.  增加更多的模板编码*
10.  设置界面分为几个section*