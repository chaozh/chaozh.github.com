title: 前端开发环境建设经验及个人总结
tags:
  - JS模板
  - 前端库
  - 前端规范
  - 项目管理
id: 476
categories:
  - 前端经验
date: 2013-02-23 12:31:13
---

首先是关于前端开发环境建设的珍贵经验，这些建议来自Google的前端大神Paul Irish的演讲《[Paul Irish on Web Application Development Workflow](http://youtu.be/vDbbz-BdyYc)》(youtube视频，需要翻墙)基本可以按照这个建议的顺序逐步优化前端开发环境与流程。

1.  命令行界面中提示符栏需要显示当前目录，当前git项目及所在分支，当前目录分支本地是否修改
2.  位于github的[dotfiles网站](http://dotfiles.github.com)中有很多有用的程序员分享的git环境设置，提供不少方便的快捷alias设置
3.  配置github所需的ssh的pubkey：第一步`cat ~/.ssh/id_rsa.pub | pbcopy`；第二步粘贴到`~/.ssh/authorised_keys`；可以在`~/.ssh/config`中配置更多
4.  使用[EditorConfig](http://editorconfig.org/)插件控制编辑器的tab与空格设置
5.  关注jshintrc设置，保证code style
6.  尝试使用[yeoman](http://yeoman.io/)(Google引导的前端开发流程项目，包含几乎所有前端开发环境可能需要使用的工具)：第一步`yeoman init`创建项目（可以使用`yeoman server`调试，包含LiveReload功能）；
7.  使用yeoman处理package management的问题：使用`yeoman install`可以将依赖包也自动下载，使用`yeoman ls`可以查看最新的版本信息
8.  测试尝试使用testem改进命令行测试界面，使用BrowserStack，AppThwack，labUp测试手机移动项目（chrome的devtools也可以使用overrides进行自适应调试）
9.  关注WebStorm与Chrome的合作，其LiveEdit功能
10.  使用guard进行简单操作自动化