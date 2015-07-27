title: '使用data-*属性指南'
tags:
  - html5
id: 862
categories:
  - HTML技巧
  - JavaScript技巧
date: 2014-08-25 22:33:08
---

本文内容参考自MDN的[using-data-attributes-in-javascript-and-css](https://hacks.mozilla.org/2012/10/using-data-attributes-in-javascript-and-css/)一文
<div>

一篇2012年的文章，目前data-*属性作为HTML5规范已经不能算是新特性了，主要是JSconf里看到troopjs使用，而boss问其他小伙伴时他们居然都不知道，于是就想总结一番。

data-*属性使用起来也很直观：
<pre>&lt;article id="electriccars" data-columns="3" data-indexnumber="12314" data-parent="cars"&gt;...&lt;/article&gt;</pre>
;

JS可以使用getAttribute方法获得data-*属性的值，当然HTML5规范里也提供更为简便的方法:
<div>
<pre>    var article = document.querySelector('#electriccars'),
                  data = article.dataset;

    // data.columns -&gt; "3"
    // data.indexnumber -&gt; "12314"
    // data.parent -&gt; "cars"
</pre>
</div>
每个属性返回都是string类型，但是可以直接设置article.dataset.columns = 5 从而改变属性的值

大多数人不知道的是HTML5还规定了data-属性相关CSS接口，可以通过该属性值来填充内容:
<div>
<pre>    article::before {
      content: attr(data-parent);
    }
</pre>
</div>
你也可以在选择器中使用data-*属性:</em>
<div>
<pre>    article[data-columns='3']{
      width: 400px;
    }
    article[data-columns='4']{
      width: 600px;
    }
</pre>
</div>
data-*属性的优势在于相关数据直接与dom节点绑定，css接口使用也挺方便，至于效率方面是否比window storage更高，留待以后测试。未来结合Web Component技术，倒是又可以大放异彩。

</div>