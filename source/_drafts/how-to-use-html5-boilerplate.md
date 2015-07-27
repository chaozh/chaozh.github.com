title: HTML5 Boilerplate使用
tags:
  - html5
  - 前端库
id: 344
categories:
  - 前端经验
date: 2012-12-11 10:53:41
---

个人站制作使用了下HTML5 Boilerplate，问题关键是如何使用。

1.首先从目录入手，需要关注的是js目录，里面libs目录存放有jquery以及modernizr，都是压缩版；mylibs文件夹用来放用户其他的库，script.js以及plugin文件都是空白，都留待用户写。

2.然后是css目录，里面的style.css，文件前面有针对html5的reset，中间空出部分让用户写页面相关的规则，后面有常用的帮助规则以及media query的模板让用户填写。

3.最后最重要的是build目录，里面config目录里有针对ant的配置文件，default.properties自然是默认属性定义，用户可以修改project.properties来定义自己的属性；tools目录下有各种编译工具。

编译时，libs下的文件不会再改变，但mylibs中文件以及另两个都会一起压缩到一个js文件中，style.css也会被压缩，同时images目录下会针对jpg和png优化。js中consloe.log函数将被去除，html文件也可以压缩，默认使用htmlbuildkit，还可以用htmlclean或htmlcompress来压缩。

再看看default.properties里定义哪些选项

build.concat.scripts = true
– 脚本文件将被压缩为一个文件

build.delete.unoptimized = true
– 未优化的文件将被删除

file.exclude = nonexistentfile
– 发布时排除的文件后缀

后面是文件目录的定义，以及编译工具的定义。

4.编译时寻找&lt;!-- scripts concatenated and minified via ant build script--&gt;这句之间空白来插入对压缩代码的引用

编译时使用，ant build即可，buildkit则不会压缩html空白，minify则完全去除空白，text则不进行图片优化。

还有很多文件没有弄明白干嘛用，继续研究中。。。