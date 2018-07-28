---
title: wxPython安装
date: 2017-04-19 18:54:30
tags: python
---

# <i class="fab fa-python fa-spin"></i> wxPython安装步骤
1. 安装homebrew

	```
	> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	```
<!-- more -->
2. 利用homebrew安装wxpython

	```
	brew install wxpython
	```
输入命令：brew info wxpython 查看安装信息。这时wxpython就安装上了。

3. 接下来就是要将wx库和python连接到一起。
首先（Python console输入）

	```
	import site; site.getsitepackages()
	```
	得到
	
	```
	['/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages', '/System/Library/Frameworks/Python.framework/Versions/2.7/lib/site-python', '/Library/Python/2.7/site-packages']
	```
其中：/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python为系统的python安装路径，/Library/Python/2.7/site-packages就是python的 site-packages 目录。

	然后找到wxpython的wx模块，它在下面的目录中wx文件夹：
	
	```
	/usr/local/Cellar/wxpython/3.0.2.0/lib/python2.7/site-packages/wx-3.0-osx_cocoa
	```
	
	接下来就是，给wx文件夹制作一个软链接，并将这个软链接放到python的 site-packages 目录下。具体步骤：
	1.进入python的 site-packages 目录：
	
	```
	cd /Library/Python/2.7/site-packages/
	```
	2.输入命令：
	
	```
	sudo ln -s /usr/local/Cellar/wxpython/3.0.2.0/lib/python2.7/site-packages/wx-3.0-osx_cocoa/wx wx
	```
	其中ln -s是制作软链接的命令，后面为wx模块路径，最后的wx为软链接的名字。


暴力方法(目前还未使用到)：
如果上面的方法不行的话，直接到wx模块所在的路径下，为wx文件夹制作替身，然后将替身拷贝到python的site-packages目录下就OK了。

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

