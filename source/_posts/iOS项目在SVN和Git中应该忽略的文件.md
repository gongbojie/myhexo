---
title: iOS项目在SVN和Git中应该忽略的文件
date: 2018-08-10 20:26:30
tags: Xcode, Git, SVN
---

在开发中，有些文件应该忽略，操作不当，有时候在Xcode项目中打了一个断点，就提示文件修改需要提交。下面就是我们需要注意的问题

<!-- more -->

###.DS_Store

添加忽略

### Pods

Pods 这个目录可以不提交，甚至 Podfile.lock 这个文件也可以不要，继而 .xcworkspace也可以不要，只要留一个Podfile文件，并在里面指定每个依赖库的版本号就够了，只要团队的每个人都有 CocoaPods 环境，每个人 pod install 后，在本地就能跑起来了。

### xcuserdata

在项目根目录下，一般有 .xcodeproj 和 .xcworkspace文件，显示包内容可以看到都有 xcuserdata 文件夹，然后里面放着 username.xcuserdata文件夹，再点进去就是 UserInterfaceState.xcuserstate 和 Breakpoints_v2.xcbkptlist，这些都属于用户个人设置，像断点记录这样的文件肯定不能提交上去合并的，没有意义，也容易导致冲突。

### fastlane

fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots
fastlane/test_output

综上 简单配置

```C
# Xcode
.DS_Store
build
report.xml
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata
*.moved-aside
DerivedData
.idea
*.hmap
*.xccheckout
*.xcworkspace
!default.xcworkspace

#CocoaPods
Pods
Podfile.lock
xcschemes

# fastlane
fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots
fastlane/test_output
```

### 或者gitignore.io 选择自定义配置

在[gitignore.io](https://www.gitignore.io/) 输入需要配置的语言，会自动生成一份配置。比如，输入 Objective-C 和 Swift 会帮助你生成下面的配置。

```C
# Created by https://www.gitignore.io/api/swift,objective-c
### Objective-C ###
# Xcode
#
# gitignore contributors: remember to update Global/Xcode.gitignore, Objective-C.gitignore & Swift.gitignore
## Build generated
build/
DerivedData/
## Various settings
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata/
## Other
*.moved-aside
*.xccheckout
*.xcscmblueprint
## Obj-C/Swift specific
*.hmap
*.ipa
*.dSYM.zip
*.dSYM
# CocoaPods
#
# We recommend against adding the Pods directory to your .gitignore. However
# you should judge for yourself, the pros and cons are mentioned at:
# https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control
#
# Pods/
# Carthage
#
# Add this line if you want to avoid checking in source code from Carthage dependencies.
# Carthage/Checkouts
Carthage/Build
# fastlane
#
# It is recommended to not store the screenshots in the git repo. Instead, use fastlane to re-generate the
# screenshots whenever they are needed.
# For more information about the recommended setup visit:
# https://docs.fastlane.tools/best-practices/source-control/#source-control
fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots
fastlane/test_output
# Code Injection
#
# After new code Injection tools there's a generated folder /iOSInjectionProject
# https://github.com/johnno1962/injectionforxcode
iOSInjectionProject/
### Objective-C Patch ###
### Swift ###
# Xcode
#
# gitignore contributors: remember to update Global/Xcode.gitignore, Objective-C.gitignore & Swift.gitignore
## Build generated
## Various settings
## Other
## Obj-C/Swift specific
## Playgrounds
timeline.xctimeline
playground.xcworkspace
# Swift Package Manager
#
# Add this line if you want to avoid checking in source code from Swift Package Manager dependencies.
# Packages/
# Package.pins
.build/
# CocoaPods
#
# We recommend against adding the Pods directory to your .gitignore. However
# you should judge for yourself, the pros and cons are mentioned at:
# https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control
#
# Pods/
# Carthage
#
# Add this line if you want to avoid checking in source code from Carthage dependencies.
# Carthage/Checkouts
# fastlane
#
# It is recommended to not store the screenshots in the git repo. Instead, use fastlane to re-generate the
# screenshots whenever they are needed.
# For more information about the recommended setup visit:
# https://docs.fastlane.tools/best-practices/source-control/#source-control
# End of https://www.gitignore.io/api/swift,objective-c
```

使用SVN的cornerstone是在设置中添加忽略
![](http://ww1.sinaimg.cn/large/880fe8a4gy1fu4w9qd4ujj20zo0zytlp.jpg)
使用Git的话，直接写入.gitignore文件即可

### 最后
.gitignore 可以忽略没必要提交的文件和目录，极大地减轻冲突几率，也可以让远程仓库更小一些。项目一开始就配置好 .gitignore，只留一个 Podfile 即可。如果项目进行到一半，添加完 .gitignore 后，需要删除追踪文件并重新提交。

PS：使用Cornerstone的话，已经添加到服务器的文件是不能选择忽略的。在第一次提交到服务器的时候就需要做好忽略的。

参考文章： https://bingozb.github.io/37.html

