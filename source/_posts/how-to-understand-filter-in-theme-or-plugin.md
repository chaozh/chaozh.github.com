title: filter的使用
tags:
  - WP主题
id: 250
categories:
  - WordPress开发
date: 2012-10-06 13:35:43
---

WordPress开发中action与filter的使用是一个重要的部分，相比action来说filter这个概念更难理解，如果能够完全理解filter，action就根本不是问题。本人也是在开发主题的过程中终于完全理解，于是立即记录分享一下。

filter概念就如名字表示的那样用于过滤输出的内容，相关API很简单，基本就是`add_filter`与`apply_filters`两个函数。`add_filter`用于向filter的tag上注册函数，`apply_filters`则是触发filter的tag从而运行注册在其上的所有函数。

开始本人不理解为何开发需要使用filter时不需要使用`apply_filters`进行触发反而只在使用`add_filter`注册函数，如果你也有相同的疑问，那么看下面这个本人碰到的实际例子：

现在需要根据主题设置修改body上class属性的内容，于是可以定义一个函数注册到tag名为body_class的filter上：
<pre>function theme_layout_classes( $existing_classes ){
    $options = get_theme_options(); //get_theme_options函数可以获得相应主题设置
    $current_layout = $options['theme_layout']; //这里得到相应主题使用的布局设置
    return array_merge( $existing_classes, $current_layout );//返回增加了设置的class内容
}
add_filter( 'body_class', 'theme_layout_classes' );//将我们定义的函数注册到tag名为body_class的filter上</pre>
这样当调用`body_class()`函数时（该函数通常在header.php中使用用于输出body的class内容：`&lt;body &lt;?php body_class(); ?&gt;&gt;`）就会调用我们定义的这个函数，从而将原定输出的class内容加上$current_layout 。

但如果现在需要在显示page时根据page的特殊布局设置覆盖主题的默认布局设置，我们应该如何处理呢？

由于已经把主题设置的class内容加进去了，再使用filter把内容减去就比较困难。实际上解决方法应是将新增的内容进行整体替换，于是我们修改函数theme_layout_classes为：
<pre>function theme_layout_classes( $existing_classes ){
    $options = get_theme_options(); //get_theme_options函数可以获得相应主题设置
    $current_layout = $options['theme_layout']; //这里得到相应主题使用的布局设置
    $classes = apply_filters( 'theme_layout_classes', $current_layout ); //这里实际我们定义了一个新的filter tag
    return array_merge( $existing_classes, $classes );//返回增加了的内容
}
add_filter( 'body_class', 'theme_layout_classes' );//将我们定义的函数注册到tag名为body_class的filter上</pre>
可以看到我们通过`apply_filters`实际定义了一个新的filter tag，这样我们只要将新函数注册到这个tag上就能够整体替换新增的内容。
于是我们再注册一个函数：
<pre>function page_layout_classes( $existing_classes ) {
    global $post;
    $current_page_layout = esc_attr(get_post_meta($post-&gt;ID, 'page_layout', true));//获得page的特殊布局设置
    if (! empty($current_page_layout) ){ //如果存在特殊设置，则返回这个内容
        return $current_page_layout;
    }else{
        return $existing_classes; //否则还是返回原有内容
    }
}
add_filter( 'theme_layout_classes', 'page_layout_classes' );//将这个函数注册到我们定义的theme_layout_classes上，以实现替换新增内容的功能</pre>
这样在调用`theme_layout_classes`时又会因为`apply_filters( 'theme_layout_classes', $current_layout );`触发调用`page_layout_classes`函数，注意`page_layout_classes`函数第一个参数$existing_classes这里会被赋值为$current_layout，于是就能用page的特殊设置覆盖主题的默认设置。

通过这个例子我们知道产生之前疑问的关键是调用函数`body_class()`时实际上里面就有调用`apply_filters( 'body_class')`进行filter的触发（出自`get_body_class:wp-includes/post-template.php`），当然我们不需要再进行触发，只需要使用`add_filter`将我们的函数注册即可。