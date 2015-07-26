title: 创建你的hexo博客
date: 2015-07-26 20:57:14
categories:
- 前端
tags: 
- nodejs
- github
- 开源项目
---
目前由于hexo的升级。在github上搭建hexo博客的过程与网络上的文章指导内容会有少许变化。

第一步当然还是安装hexo，接着在本地创建自己的hexo博客目录

```
$ npm install hexo -g
$ hexo init <YOU_BLOG_DIR_NAME>
```

然后切换到那个目录下面，安装必备的node插件
```
$ cd <YOU_BLOG_DIR_NAME>
$ npm install 
$ npm install hexo-deployer-git --save
```

这时你的博客已经准备就绪啦，接着按照自己的需求修改配置文件，需要注意的是deploy部分
```
deploy:
  type: git
  repo: <YOUR_GITHUB_REPO_URL>
```

万事大吉，可以使用`hexo new <POST_NAME>`来创建新文章，写完后使用
```
$ hexo server #本地查看博客效果
$ hexo generate #生成静态文件用于部署
$ hexo deploy #部署同步到Github的项目上
```

有些遗憾的是这个git的deploy插件有点傻，仅把博客的静态文件提交到了远程仓库的master分支，而没有提交原始配置文件。没办法，只能自己动手进行分支同步。
```
$ cd <YOU_BLOG_DIR_NAME>
$ git init
$ git add remote origin <YOUR_GITHUB_REPO_URL>
$ git commit -a
$ git push origin master:<YOUR_REMOTE_BRANCHNAME>
```
好啦，以后任何位置都可以利用Github同步文章写博客啦