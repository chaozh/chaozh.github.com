title: 正确理解Wordpress设置API
tags:
  - WP主题
  - WP插件
  - WP设置
id: 382
categories:
  - WordPress开发
date: 2012-12-30 12:50:48
---

Wordpress设置API使用起来并不算太方便，很多地方都容易混淆，因此在本人读懂并改写了主题相关的设置类之后，就想把相关容易混淆的地方详细说明一下。

函数`register_setting`与`settings_fields`为一组，它们之间使用参数option_group进行关联，另外函数`register_setting`注册了get_option函数所需要的参数option_name。这两个函数一个用于注册设置信息，一个用于显示设置信息（实际即一组hidden属性的input标签，显示时使用参数option_group）。

函数`add_settings_section`与`add_settings_field`与`add_[theme或options]_page`为一组，它们之间使用参数menu_slug进行关联，由该参数可知这些函数只与某个特别的page相关，用于注册表单展示信息。使用函数`do_settings_sections`来渲染展示表单（显示时使用参数menu_slug）。

关于settings的状态栏使用&lt;?php settings_errors(); ?&gt;，该函数即可显示黄色的状态栏。更多状态栏相关开发参考这个[链接](http://wordpress.stackexchange.com/questions/23701/how-should-one-implement-add-settings-error-on-custom-menu-pages)

&nbsp;