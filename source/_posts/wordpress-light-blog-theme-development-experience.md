title: WordPress相关轻博客主题开发经验
tags:
  - CSS
  - WP主题
  - 栅格系统
id: 389
categories:
  - WordPress开发
date: 2013-01-03 07:50:45
---

轻博客类型的主题已经是现今个人站的设计风尚，不少大型设计博客站点也投入轻博客风格的怀抱，典型的就是[Smashing Magzine](http://www.smashingmagazine.com/ "smashing magazine")，其站点的样式充分体现了目前实用类博客的结构设计思路，其结构可归纳为：

导航栏 nav.toplevel

主内容区 div#container &gt; div.sidebar+div.main(fluid)

博客导航区 div#wpsidebar

底部功能区  div#footer

实际上，设计时需要正确理解轻博客的意图，要知晓该风格主题定制性一般不如博客。sidebar功能中小工具功能不可能充分发挥（一般除广告外，只能放置一到两个widget，否则就会拥挤），menu的功能一般也归放到边栏sidebar展示，而很少像以前一样出现在站点中间位置。（网站Smashing Magzine即放到边栏的博客导航区）

本站的最新设计就尽量在经典博客与轻博客风格中寻找一种平衡，但是设计出发点总是信息的有效传达。设计博客的关键在于阅读体验好，在于查找内容方便，所以目前sidebar还是使用了多个widget，menu也仍然放在站点中间。

轻博客主题的另一大特色就是Responsive design，即一次设计必须能够自适应多种阅读终端设备。虽然本人很早就接触过这些内容，但只在本站制作过程中才真正开始使用这些内容。个人感觉自适应设计最关键的技术点在两个地方：一个是字体大小的安排，一个是media query的安排。

字体的大小安排在目前的主题中有两种风格，一种是默认主题twentyeleven中使用的em组织方式，另一种是最新默认主题twentytwelve中使用的rem组织方式。前一种方式的问题主要是使用em为单位需要叠加计算字体大小（可以使用`body{font-size:62.5%;}`将默认的1em=16px调整为10px，这样计算就方便一些），布局使用的margin或padding也都必须使用百分比或em，故而比较复杂，但是浏览器兼容较好。后一种方法使用了CSS3中加入的rem单位，即相对于根标签html的字体大小，最新默认主题twentytwelve中即使用`html{ font-size: 87.5%;}`将默认的1rem=16px调整为14px，于是其他所有属性如margin等都以14px=1rem为标准进行编写。但是可惜的是该单位ie8以下都不认识，所以还是必须用px进行兼容，于是还是必须写出` body { font-size: 14px; font-size: 1rem;}`这样的代码来。

media query的安排必须了解各个阅读终端的屏幕尺寸，下面这个视频对了解苹果系列产品的分辨率及尺寸很有帮助。本站选择了800px与480px两个分界线，来分别对应平版终端（通常需要处理的是竖着拿的情况）与手机终端（横竖都必须处理）。
<div><object id="sinaplayer" width="480" height="370" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="allowScriptAccess" value="always" /><param name="src" value="http://you.video.sina.com.cn/api/sinawebApi/outplayrefer.php/vid=95693618_1987543647_Zhq0SiZuWTaP+Eh0HTWxve0D+/cXuvDogG+yslGiJAZPE1XaapWcZNQE5ijeFqwbrz0xHcZkeP8wkkR5ZatR1jQvbQoVhlM/s.swf" /><param name="pluginspage" value="http://www.macromedia.com/go/getflashplayer" /><param name="allowfullscreen" value="true" /><param name="allowscriptaccess" value="always" /><embed id="sinaplayer" width="480" height="370" type="application/x-shockwave-flash" src="http://you.video.sina.com.cn/api/sinawebApi/outplayrefer.php/vid=95693618_1987543647_Zhq0SiZuWTaP+Eh0HTWxve0D+/cXuvDogG+yslGiJAZPE1XaapWcZNQE5ijeFqwbrz0xHcZkeP8wkkR5ZatR1jQvbQoVhlM/s.swf" allowScriptAccess="always" pluginspage="http://www.macromedia.com/go/getflashplayer" allowfullscreen="true" allowscriptaccess="always" /></object></div>
<div></div>
<div>有关响应式设计与开发可以参考[这篇文章](http://www.uisdc.com/guidelines-for-responsive-web-design)</div>