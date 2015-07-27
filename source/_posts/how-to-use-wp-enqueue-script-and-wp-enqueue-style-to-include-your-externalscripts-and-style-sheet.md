title: wp_enqueue_script与wp_enqueue_style相关函数的使用
tags:
  - CSS
  - 脚本加载
id: 75
comment: false
categories:
  - WordPress开发
date: 2012-03-05 12:50:56
---

很多主题都未使用WP系统提供的api来引用额外脚本与样式表，而是在前端页面代码中加入或是在某些函数中输出。就最终结果来讲其实影响不大（当然如果使用wp_minify这个插件来进行站点性能优化，可能会出现某些js代码或样式表遗漏的状况），但这会给代码管理上带来困难，特别是当你需要修改这些外部引用代码位置的时候。

其实WP提供了wp_register_style，wp_register_script，wp_enqueue_style，wp_enqueue_script四个函数来简化额外样式表与JS代码脚本的引用。

前面两个用于向WP注册引用信息，后面两个用于真正插入样式或脚本。实现待补充。

**特别需要注意**的是wp_enqueue_script使用时必须在调用wp_header函数之前，否则注册时wp_register_script是否在尾部加入的参数设置不会起到效果，甚至会影响wp_enqueue_style的插入地点，变为一律在最后调用函数的位置处插入脚本与样式表，这对样式来说问题就比较大咯。