---
title: OSX High Sierra 10.13 装cocoapods踩坑
date: 2018-07-28 17:35:02
tags:
---

# OSX High Sierra 10.13 装cocoapods踩坑
## 关掉SIP

步骤：
1. 进入Recovery Mode模式: 重启Mac，按住Command + R键直到Apple Logo出现 
2. 关闭SIP：点击工具里的Terminal（终端），输入

```
csrutil disable
```

4. 重启Mac，重启完成后，终端中输入 

```
$ sudo chflags norestricted /usr/local && sudo chown -R $(whoami):admin /usr/local
```

关闭SIP完毕

PS:（如果想重新开启安全设置，则重复1、2步骤，输入csrutil enable就可以了）

原因：10.13版本加强了权限的限制，尤其是对/usr/local目录，默认开通 SIP （System Intergrity Protection），它禁止了软件以root身份在Mac上运行，不管你是在终端中如何运行。

<!-- more -->

## 换源：

请尽可能用比较新的 RubyGems 版本，建议 2.6.x 以上。
```
$ gem update --system # 这里请翻墙一下
$ gem -v
2.6.3
```

更换源

```
$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
$ gem sources -l
https://gems.ruby-china.com
# 确保只有 gems.ruby-china.com
```

## 部署cocoapods
在cocoapods慢到吐血的情况下 选择国内镜像吧 

[清华大学镜像站](https://mirror.tuna.tsinghua.edu.cn/help/CocoaPods/)

新版的 CocoaPods 不允许用pod repo add直接添加master库了，使用以下方法

```
$ cd ~/.cocoapods/repos 
$ pod repo remove master
$ git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git master
```

最后进入自己的工程，在自己工程的podFile第一行加上：

```
source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
```


