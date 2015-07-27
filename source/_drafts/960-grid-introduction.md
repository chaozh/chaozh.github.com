title: 960栅格系统快速使用指南
tags:
  - CSS
  - 栅格系统
id: 225
categories:
  - 前端经验
date: 2012-09-02 11:47:39
---

<wbr>现代浏览器中ctrl+u 用于切换是否显示栅格</wbr>

960的demo分为三个部分，分别展示了几种class的用法：

**top部分**

分为两竖条，24格系统中一条以30开始，40递增

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/01_topsection.png "960栅格系统 - 学大汉武立国 - 草稿箱")

container_24说明将960px分为24栏竖条。实际使用950px，左右两边还有5px的白边

grid_xx说明占多少栏竖条

空白条10px

css实现：

<pre>.grid_xx{

  display:inline;//防止ie6的double margin float bug

  float:left;

  margin-left:5px;

  margin-right:5px;

}

.container_24 .grid_1 {
    width: 30px; 
}
</pre>
16格系统中是40的竖条 20的间隔

**middle部分**

30的竖条

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/02_midsection.png "960栅格系统 - 学大汉武立国 - 草稿箱")

使用prefix_xx和suffix_xx来表明占用的前后需要空有多少竖条，故prefix_xx+suffix_xx=23

css实现
<pre>
.container_24 .prefix_1{
    padding-left: 40px;
}

.container_24 .suffix_1{
    padding-right:40px;
}
</pre>
使用效果：

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/12_midsection3.png "960栅格系统 - 学大汉武立国 - 草稿箱")

注意这个部分每行class应该满足grid+prefix+suffix=24

**bottom部分**

只有两种

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/03_bottomsection.png "960栅格系统 - 学大汉武立国 - 草稿箱")

使用push_xx和pull_xx来改变div显示顺序

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/15_bottomsection3.png "960栅格系统 - 学大汉武立国 - 草稿箱")

显然左右与代码顺序相反

css实现：
<pre>
.push_xx .pull_xx{
  position:relative; 
}

.push_xx{
  left: xx px; 
}

.pull_xx{
  left:-xx px; 
}
</pre>
使用alpha和omega来取消grid_x的左右白边，主要用于父容器内的子元素

![960栅格系统 - 学大汉武立国 - 草稿箱](http://d2o0t5hpnwv4c1.cloudfront.net/765_960gsExplanation/22_nested-divs4.png "960栅格系统 - 学大汉武立国 - 草稿箱")

css实现：
<pre>
.alpha{
  margin-left:0; 
}

.omega{
  margin-right:0; 
}
</pre>
**clear部分**

最后是和浮动元素相关的clear问题

原来采用div class=“clear”方法

css代码:
<pre>
.clear{
  clear:both;

  display:block;

  overflow:hidden;

  visibility:hidden;

  height:0;

  width:0;
}
</pre>
由于必须引入多余的div，故采用clear-fix方法
<pre>
.clearfix:after{
  clear:both;

  content:’’;

  display:block;

  font-size:0;

  line-height:0;

  visibility:hidden;

  width:0;

  height:0;
}
</pre>
上面采用after假类形成插入div clear一样的效果。
<pre>
* html .clearfix,                            //ie6

*:first-child+html .clearfix{      //ie7
  zoom:1; 
}
</pre>
消除ie6,7的问题 ie5不再考虑了