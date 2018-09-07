---
title: Xcode打包提交第三方包出错问题
date: 2018-08-27 17:43:11
tags: Xcode, App Store
---

![](http://ww1.sinaimg.cn/large/880fe8a4gy1fuoc0p6djaj20f705iq4a.jpg)

1. 第三方框架的plist文件，查看plist文件中，找出key是CFBundleExecutable(或者是Executable file)的配置行。一般都是在某些第三方的plist文件中。

![](http://ww1.sinaimg.cn/large/880fe8a4gy1fuoc29g7i4j20yi0q8q9a.jpg)

2. 将找出所有第三方plist文件中的CFBundleExecutable行，CFBundleSupportedPlatforms行删除
3. 重新打包，提交

[参考链接](https://stackoverflow.com/questions/32622899/itms-90535-unable-to-publish-ios-app-with-latest-google-signin-sdk)