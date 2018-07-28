---
title: Xocde无法打开工程
date: 2018-06-19 15:25:52
tags: Xcode
---
在打开项目的.xcworkspace时候，发现出现错误无法运行
![](http://ww1.sinaimg.cn/large/880fe8a4gy1fsgj20beq9j21400oejuy.jpg)
<!-- more -->
![](http://ww1.sinaimg.cn/large/880fe8a4gy1fsgj27kutij21400oeq6o.jpg)

再次打开工程文件.xcodeproj，也是无法打开
![](http://ww1.sinaimg.cn/mw690/880fe8a4gy1fsgizr9ssoj20bs04t0t9.jpg)

```
Failed to load project at 'xxx', incompatible project version.
```
然后排除cocoapods的原因，发现创建项目时使用的是Xcode 9.3而自己的Xcode版本是9.2，所以无法打开。

解决办法：

1. 将Xcode升级到9.3
2. 修改项目中的配置使项目可以用低版本xcode打开![](http://ww1.sinaimg.cn/large/880fe8a4gy1fsgjbeppnqj229i0ke457.jpg)

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">


