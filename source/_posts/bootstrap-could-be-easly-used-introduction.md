title: Bootstrap简易使用指南
tags:
  - HTML标签
  - javascript
  - 前端库
  - 栅格系统
id: 215
categories:
  - 前端经验
date: 2012-08-01 11:33:22
---

看完使用手册后所作的笔记，可以当做简易使用指南使用。

## 1.框架

### 1.1全局样式

使用HTML5的doctype，scaffolding.less中定义全局样式，从2开始使用normalize.css，并使用reset.less进行简化

### 1.2默认栅格系统

940px宽12列栅格，使用row与span[NUM]的class来控制，使用offset[NUM]来控制偏移，于non-fluid可以直接嵌套，提供了四种响应式方案

### 1.3流动栅格系统

基于百分比，将row改为row-fluid即可使用，内嵌注意宽度是按照父列的百分比进行计算的

### 1.4自定义栅格

于variables.css中改变变量，默认列12，宽60px，间隔20px，要保证响应性还得修改responsive.less中内容

### 1.5布局

container为940px居中，container-fluid则为流体布局

### 1.6 响应式设计

responsive.less中提供了一组media query：

智能手机《=480px；流式列，非固定宽度

垂直平板《=767px；流式列，非固定宽度

水平平板》=768px；42px 20px

默认》=980px； ? ? ?60px ?20px

大分辨率》=1200px；70px 30px

要求添加meta标签，&lt;meta name="viewport" content="width=device-width, initail-scale=1.0"&gt;

有诸如.visible-phone等支持类

## 2.基础CSS

### 2.1 排版

整个排版单位基于variables.less中@baseFontSize与@baseLineHeight两个变量；

强调：string加粗，em倾斜，abbr缩写【title属性存放显示信息，.initialism会减小缩略词字体】，address【使用br换行】

引用：blockquote【cite属性存放来源URL，.pull-left或right决定内容居左右】，small用于引言作者【会在内容前加入破折号】

列表：ul无序号有黑点，ul.unstyled无样式，ol有数字序号，dl描述，dl.dl-horizontal水平描述

### 2.2代码

code行级代码，pre块级【&lt;&gt;需要转义，.pre-scrollable可以设置350px最大高度】，应用.prettyprint和.linenums来美化代码【使用google prettify】

### 2.3表格

table thead【tr】 tbody【tr】tr【td或th】th【必须在thead之内】 caption；

.table行之间有水平线分割【2.0开始为默认】 .table-borderd 【边角为圆角】.table-striped 奇偶分开【使用:nth-child ie7-8不支持】 .table-condensed 紧凑竖直方向padding减半 几个可以组合使用

### 2.4表单

四种表单：.form-vertical【2.0后默认，控件标签文字左对齐】.form-inline【左对齐，控件inline-block】 .form-search【文本框圆化】 .form-horizontal【左浮动，标签与控件居于同一行且文字右对齐】

支持控件：文本输入框，单选，复选，下拉，多选，上传，文本域

控件组：.control-group .control-label以及.controls【默认label应该与控件在同一行？】

设计了各种控件状态【如focus，disabled，去除webkit的outline】，包含.error .warning .success验证样式

扩张控件：.span*来指定输入框大小，使用.input-mini或small或medium或big来指定input和select控件大小，2.0开始对.checkbox或.radio应用.inline即可实现行级，用label.checkbox包含input[type=checkbox]即可罗列，前置或后置文本保证.add-on与input在同行, .help-inline与.help-block设置帮助文本

### 2.5 按钮

可以应用到a button及input标签上，.btn .btn-primary .btn-info等样式【ie9不兼容】，.btn-large small mini等尺寸，.disabled类或disabled属性可以禁用

### 2.6 图标

使用.icon-前缀设置，用&lt;i&gt;x显示图标，用.icon-white显示反白图标，图标定义在sprites.less中

## 3.组件

### 3.1按钮

#### 3.1.1按钮组

建议一个组里只用一种元素&lt;a&gt;或&lt;button&gt;，使用.btn-group，组合.btn-toolbar包装.btn-group即可合成工具条组件

#### 3.1.2按钮下拉菜单

下拉菜单也得嵌套在.btn-group中，使用dropdown-toggle与ul.dropdown-menu类，支持Bootstrap下拉插件，箭头使用.caret，.dropdown-menu最近父标签应用.dropup即可变为上弹菜单【会改变.caret箭头方向】

### 3.2导航

#### 3.2.1默认项

基类.nav，对齐使用.pull-left或.pull-right【依赖float】，标签页ul.nav-tabs，胶囊链接ul.nav-pills

#### 3.2.2叠放式导航

指竖直叠放ul.nav-stacked

#### 3.2.3下拉项

综合使用下拉按钮【js下拉项插件】，参考3.1.2

#### 3.2.4导航列表

&lt;i&gt;使用标签，.pider空表项显示为水平间隔，.active选中项，.nav-header列表头

#### 3.2.5 标签页切换导航

用.tabbale的p嵌套.nav-tabs，存放容器为.tab-content，内容页使用.tab-pane，标签置底用.tabs-below，标签居左.tabs-left，居右.tabs-right

#### 3.2.6 导航条

固定导航条div.navbar与.navbar-fixed-top【必须预留40px或更多padding】，导航项ul.nav，li.pider-vertical分隔条，项目名称a.brand，表单.navbar-form，对form.navbar-search中输入框使用.search-query得到搜索框，下拉菜单参考3.2.3，导航条文本使用&lt;p&gt;，响应式嵌套在.nav-collapse.collapse并对按钮都应有.btn-navbar【需要js切换插件】

#### 3.2.7面包屑导航

ul.breadcrumb

#### 3.2.8页码

div.pagination&gt;ul，同样使用.active与.disabled，页码对齐使用.pagination-centered或.pagination-right，前后页ul.pager【居于左右端li.previous与li.next】

### 3.3行内标签

span.label默认样式，span.label.label-success成功等

### 3.4 标号

span.badge默认样式，span.badge.badge-success等

### 3.5 排版

主角单元div.hero-unit中嵌套，标题h1，可以嵌入small，

### 3.6 缩略项

ul.thumbnails&gt;li.span*&gt;a.thumbnail&gt;img链接图像，div.thumbnail块状内容

### 3.7通知

基类div.alert【2.0开始替代.alert-message】，例子：div.alert&gt;a.close+strong，加强.alert-block提供更大的padding而.alert-heading修饰标题，语义强化.alert-error或success或info

### 3.8进度条

基本div.progress&gt;div.bar[style="width:60%"]，条纹效果div.progress.progress-striped【动画效果加上.active，使用css3渐变动画，不支持ie】，语义加强.progress-info或success等

### 3.9杂项

消息墙div.well，关闭图标a.close

## 4 jQuery插件

### 4.1对话框【bootstrap-modal.js】

$().modal({backdrop:true背景,keyboard:true支持ESC,show:true初始化显示}) ，

触发设置data-toggle="modal"然后data-target="#foo"或href=“#foo”，

对话框设置div.modal#foo即可：div.modal-header&gt;a.close[data-dismiss="modal"]+div.modal-body+div.modal-footer【显示动画效果bootstrap-transition.js，对.modal应用.fade即可】，方法.modal("toggle")或.modal(“show”)或.modal("hide")，事件show,shown,hide,hidden

### 4.2 下拉项【bootstrap-dropdown.js】

样式应用导航栏与胶囊链接，方法$().dropdown()，设置data-toggle="dropdown"【也可以使用data-target="#foo"或href=“#foo”来关联下拉项与链接】

### 4.3 滚动侦测【bootstrap-scrollspy.js】

$('#navbar').scrollspy()

标记添加data-spy="scroll"【导航链接必须有href="#id"且对应有dom#id】，选项offset【默认为10】

### 4.4 可切换的标签页【bootstrap-tab.js】

方法$('#myTab').tab('show') 标签页需要设置data-target='#id'或href='#id'

标记添加data-toggle="tab"或data-toggle="pill"，

事件show与shown 【event.target指向激活标签，event.relatedTarget指向之前激活的标签】

### 4.5 工具提示【bootstrap-tooltips.js】

$('#example').tooltip(options) 中选项animation:true，placement:'top'，selector，title，trigger:'hover'，delay:{show:num, hide:100}

工具提示可以单独设置data-属性实现与js调用同样的功能，指定一个selector即可【设置rel="tooltip"】

方法：.tooltip('show')?.tooltip('hide')?.tooltip('toggle')

### 4.6 弹出提示【bootstrap-popover.js】

$('#example').popover(options)中选项animation:true，placement:'top'，selector，trigger:'hover'，title，content，delay

同样可以单独设置data-属性，方法也相同

### 4.7 通知消息【bootstrap-alert.js】

$(".alert").alert()

用在通知，对关闭按钮设置data-dismiss="alert"即可定时关闭

方法$(".alert").alert(‘close’)，事件close closed

### 4.8 按钮【bootstrap-button.js】

应用在btn与btn-group，设置data-toggle="button"与data-toggle="button-checkbox"与data-toggle="button-radio"样式

方法$().button('toggle') 按下

$().button('loading') 载入文本data-loading-text属性中

$().button('reset')重置按钮状态

### 4.9 折叠手风琴【bootstrap-collapse.js】

$().collapse({toggle:false})，事件show，shown，hide，hidden

设置data-toggle=“collapse”和data-target即可变为折叠式，data-target接收一个css选择器以选取元素添加，元素上需要添加.collapse，默认打开用.in

### 4.10 轮播【bootstrap-carousel.js】

$().carousel({interval:5000, pause:'hover'})

标记用data-属性提供前后翻页，data-slide="prev或next" 方法.carousel('cycle或pause或number或prev或next') 事件slide，slid

### 4.11 输入提醒【bootstrap-typeahead.js】

$().typeahead({source:[]数据源, items:8列表显示个数, matcher:fn, sorter:fn, highlighter:fn})，

设置data-provide="typeahead"

## 5\. LESS

mixins.less中保存所有混合，编译安装npm intall -g less uglify-js lessc ./lib/bootstrap.less &gt; bootstrap.css压缩使用--compress，引用less.js 也可以重新编译.less文件并进行本地存储