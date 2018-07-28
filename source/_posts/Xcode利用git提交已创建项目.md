---
title: Xcode利用git提交已创建项目
date: 2015-09-06 15:39:36
tags: Xcode
---


一般情况下,提交已有项目总是会出现项目名称后面的符号 ***_?_*** ,正确的提交步骤如下:
<!-- more -->

1. 在[oschina](http://git.oschina.net)或者[github](http://github.com)上创建新项目
2. 创建完成后,命令行 git clone + 新项目地址 到本地文件夹,并且接着把已有的项目文件移动到该文件夹
3. 利用终端进入到该工程的目录下

	```
	cd /Users/gong/Desktop/WORK/XXXXX
	git init
	git add . 
	git commit -m ‘initial’ 
	```
		
4. 打开Xcode,点击 Source Control 点击 Push 即可提交所有代码

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">


