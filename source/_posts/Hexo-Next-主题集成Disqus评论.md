---
title: Hexo Next 主题集成Disqus评论
date: 2017-04-25 18:23:25
tags: Hexo评论
---
hexo默认集成了disqus
<!-- more -->
# Step 1 登录注册disqus
首先进入[Disqus官网](https://disqus.com/)注册登录，然后选择![](http://ww1.sinaimg.cn/mw690/880fe8a4ly1fez4qa0c8fj20xp0m6q9o.jpg)
接下来填写唯一的website Name，这个需要在后面配置hexo主题用到
![](http://ww1.sinaimg.cn/mw690/880fe8a4ly1fez53mg5scj20xs0jsmyk.jpg)
category可以随意填写

下一步 
![](http://ww1.sinaimg.cn/mw690/880fe8a4ly1fez564rgjhj20xt0m8dhy.jpg)
直接选择Configure Disqus website URL填写自己博客的网站域名

# Step 2 修改hexo配置文件
编辑 主题配置文件
* [HEXO_PATH]/themes/next/_config.yml

将 disqus 下的 enable 设定为 true，同时填写在Disqus配置的shortname。count 用于指定是否显示评论数量。

```
disqus:
  enable: true
  shortname: your-short-name
  count: true
```
修改完成后 最好在Terminal运行

```
$ hexo clean
$ hexo g
$ hexo d
```
然后输入hexo server 就会出现Disqus评论效果
![](http://ww1.sinaimg.cn/mw690/880fe8a4ly1fez5abc5llj20lo0hxta2.jpg)

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

