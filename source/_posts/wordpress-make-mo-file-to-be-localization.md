title: WordPress中文本地化方法总结与相关问题
tags:
  - WP主题
  - 中文汉化
id: 30
comment: false
categories:
  - WordPress开发
date: 2012-02-18 12:36:24
---

终于基本完成本站主题的中文汉化工作，不得不佩服WordPress系统为本地化工作设计的API，尽管不算尽善尽美，但是使用起来还是比较方便的。以及所谓.po和.mo的本地化文件的使用，不知是哪个人发起的主意，确实简化了本地化的工作。废话就不多说，总结下chaozh主题汉化过程中学到的技巧及遇到的相关问题。这些问题都是典型新手会碰到问题，而且尚未完全解决，如果哪位高人看到，可以说出更好的解决方案，本人十分之欢迎！

如果你只想快速了解如何进行WordPress汉化，那么可以参考这篇文章《[poEdit制作WordPress主题汉化,插件汉化攻略](http://simonleung.me/archives/10400)》，是本人看到最完整清晰的中文本地化攻略。如果仍有问题，请继续耐心看完下面的内容。

** 补充**：（这些补充出自此[链接](http://simonleung.me/archives/10400)，该文章也大致说了下中文汉化的方法。）

—如果只有.mo文件的话,则需要编译成.po文件处理，可以使用GetText来反编译。

下载地址为：[http://www.gnu.org/software/gettext/](http://www.gnu.org/software/gettext/)。

—翻译中注意保留变量的原有形态 例如：阅读所有 %s 的文章
—HTML语句中的内容不要翻译
—保存出来的MO文件名是区分大小写的，正确的命名如： zh_CN.mo，可对照language目录其他语言的命名方式。

## 1.WordPress本地化API

那么首先说说WordPress系统为本地化工作设计的API。在开发过程中可以对需要翻译的部分使用多种函数进行标记，以方便后面的翻译工作，可以使用的函数大致有__()，_e()，_x()以及_ex()，还有_n()等。前面的那些比较常用，_n()主要用于区分单复数，所以中文就不太用得着，而且也可以使用其他方法来规避改函数的使用。使用最多的头两个函数接受两个参数，第一个是要显示（翻译）的字符串，第二个是使用的命名空间。这两个函数的区别在于__()返回翻译后的文字字符串，而_e()直接把字符串打印出来，而且相信你已经看出_x()和_ex()这对也是类似的区别。命名空间的使用可以用来区分主题，一般主题中function.php文件中有这样的代码来完成本地化文件的读取：
<pre>load_theme_textdomain('chaozh', TEMPLATEPATH . '/languages');
$locale = get_locale();
$locale_file = TEMPLATEPATH. "/languages/$locale.php";
if (is_readable($locale_file))
require_once ($locale_file);</pre>
其中，load_theme_textdomain函数就是用来设置命名空间的，经过本人实验如果在这里设置了命名空间"chaozh"，而后面直接写`__('Page')`而不加上相应的命名空间参数，会导致该信息没有被翻译显示。所以应该写为`__('Page'，'chaozh')`来获得翻译内容。

再来说说_x()系列，该函数使用频率其实也不小。该函数接受三个参数，还有一个参数来表明上下文（context），主要用于英文内容相同但希望译文不同的情况。经典例子是管理员菜单中文章目录及页面目录下相同的“Add New”的翻译：如果还使用__()函数会导致同样的英文内容只能采取相同的翻译方式，这样就无法对添加post和page的按钮进行区分，所以要加入上下文环境来帮助翻译。添加文章的信息可以写为`_x('Add New','for adding post',‘chaozh’)`而添加页面的信息可以写为`_x('Add New',‘for adding page’,'chaozh')`,这样就可以区分了。

[![添加post翻译效果](http://www.chaozh.com/wp-content/uploads/2012/02/post_add.png "post_add")](http://www.chaozh.com/wp-content/uploads/2012/02/post_add.png)[![添加page翻译效果](http://www.chaozh.com/wp-content/uploads/2012/02/page_add.png "page_add")](http://www.chaozh.com/wp-content/uploads/2012/02/page_add.png)

参考[官方介绍](http://codex.wordpress.org/Function_Reference/_x)

## 2.Poedit软件使用遇到的问题

具体用法可以一步步参考前面的文章，这里主要说说需要注意的问题。一个是更新代码后记得点击软件的同步按钮，以把新修改的内容加入到翻译队列中。另一个是改造过程中，本来wp主题文件都是utf-8编码，但是Poedit软件默认的是ansi编码，所以有需要的话要在项目设置上填上，否则会导致文件需要翻译的内容无法同步。

但Poedit软件最大的一个问题是，即使在项目设置的关键字加入_x，该软件还是会无视上下文参数，无法区别显示需要翻译的内容。并且即使把相应地点进行翻译，生成的mo文件最后还是无法显示翻译后的内容。由于本人目前只会使用这个软件来进行汉化工作，所以直接导致目前这个主题里都无法使用_x()函数，目前还不知道如何来解决。

<span style="color: #ff0000;">这个问题已经找到一个可行的解决方案，即项目设置的关键字加入时使用_n:1,2；_x:1,2c；_ex:1,2c；这里:1,2说明有两个参数，而c则说明第二个参数是comment类型 具体可以参见[<span style="color: #ff0000;">这篇文章</span>](http://wp.tutsplus.com/tutorials/theme-development/translating-your-theme/)  于2012-03-31 最新修改！</span>

## 3.主题开发中碰到的问题

目前还碰到一个更诡异的问题，本人使用theme-options.php文件来生成主题设置界面，其中有段代码：
<pre>$controls = array(
array("name" =&gt; __('Blog Promotion Options','chaozh'), "type" =&gt; "heading"),
array('name' =&gt; __('Display Custom Sharing Widget?','chaozh'),
"desc" =&gt; __('Check here to &lt;b&gt;display&lt;/b&gt;.','chaozh')...</pre>
但不知何故里面的部分就是无法显示翻译后的信息，默认主题中也有类似的代码：
<pre>$layout_options = array(
'content-sidebar' =&gt; array(
'value' =&gt; 'content-sidebar',
'label' =&gt; __( 'Content on left', 'twentyeleven' )...</pre>
而显然该主题就没有什么问题。显示方法都是调用echo函数，而且本主题还是有返回原英文信息，这说明__()函数还是运行了的，但是为什么会没返回翻译内容呢？这点很是诡异。期待以后能把这个问题解决吧。

## 4.插件汉化问题 <span style="color: #ff0000;">new！</span>

进行插件汉化的过程中，又遇到新的问题，也是无论怎么命名，都无法显示翻译后的内容。相关插件汉化方法，其实与前文所诉一致，就是多了对load_plugin_textdomain函数的使用。而插件汉化的问题基本都是出在这个函数上面，该函数接受多个参数，第一个自然是domain，即命名空间，关键是这个可选的第二个参数。本人汉化的插件名为breadcrumb_trail，但是文件夹无法使用'_'来命名，于是插件文件夹默认名就变为'breadcrumb-trail'，这就导致使用原有的下面的代码无法定位翻译文件：（详情可以参考[这篇文章](http://lichao.net/weblog/web-development/blogging/41.html)，里面有分析）

`load_plugin_textdomain( 'breadcrumb_trail');`

因为这默认告诉wp来找名为breadcrumb_trail文件夹下的breadcrumb_trail-zh_CN.mo文件，显然没有这个文件夹，所以必须改为：

`load_plugin_textdomain( 'breadcrumb_trail','/wp-content/plugins/breadcrumb-trail/');`

即加入指定路径方可成功显示翻译信息。所以这也告诉我们，插件的命名空间命名最好不要使用'_'来进行分隔，或者不要图省事使用第一种代码，而应该加入第二个参数，不给别人本地化工作带来麻烦。

<span style="color: #ff0000;">第二个参数可以这样添加：`PLUGINDIR . '/' . dirname(plugin_basename (__FILE__))`，这样就不需要根据目录名来调整参数代码了。但是第一个参数即插件的命名空间一定要与翻译文件名称相同   于2012-03-18 最新修改！</span>

<span style="color: #ff0000;">目前第二个参数已经被废弃了，但是依旧保留，实际上官方推荐使用第三个参数，该参数只要给出.mo文件相对于插件根目录的相对路径即可：dirname(plugin_basename (__FILE__)) . '/languages'。这样设计科学多了，毕竟绝对路径很没必要，谁会没事干瞎改插件根目录  </span><span style="color: #ff0000;">于2012-10-26 最新修改！</span>