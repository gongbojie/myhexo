---
title: fastlane使用小记
date: 2018-06-29 21:22:26
tags: fastlane
---

之前就用过[fastlane](https://docs.fastlane.tools/)作为持续集成自动化的打包上传的工具，目前为止使用在企业账号打包 上传[fir.im](https://fir.im/)。今天又再次使用和之前的版本有些变化,主要注意的有以下几点

<!--more-->

## <i class="fas fa-rocket"></i> 升级fastlane
因为之前就用过fastlane，所以以免出现问题就需要升级

``` ruby
[sudo] gem install fastlane -NV
```
之前使用

```
sudo gem update fastlane
```
一直升级不成功，后面发现一定要先用

```
sudo gem uninstall fastlane
```
删除旧版本，再重新安装，才能装到最新版本。

## Appfile
使用

```
fastlane init
```
初始化完毕后选择手动lane后就可以在Appfile和Fastfile进行编辑

``` Ruby
app_identifier "com.XXX.XXX" # App的Bundle id
apple_id "email@example.com" # apple id的账号
team_id "XXXXXXXXX" # 开发者Team id 可以在开发者网站进行查看https://developer.apple.com/account/#/membership
```

## Fastfile 打包设置

在这之间编写代码，也就是编写动作[action](https://docs.fastlane.tools/actions)，

``` ruby
lane :XXX do
  # code
end
```

``` Ruby
gym(
      scheme: "XXX",
      # 导出方法选择 adhoc enterprise(企业证书) app-store(上架App Store) development(开发证书)
      export_method:"development",
      export_xcargs: "-allowProvisioningUpdates", # 解决升级Xcode 9 后 fastlane gym 不允许访问钥匙串里的内容造成的打包错误
      export_options: {
        provisioningProfiles: { 
        # 在路径中 查找匹配的证书 ~/Library/MobileDevice/Provisioning Profiles
          "com.XXX.XXX" => "XXXX.mobileprovision",
        }
      },
      output_directory:"./fastlane/build", # 输出路径
    )
```

## 上传firim设置
运行之前要安装插件

```
fastlane add_plugin versioning
fastlane add_plugin firim
```

``` Ruby
# 在fir.im官网查看API token
firim(
      firim_api_token: "API token Number"
    )
```

具体细节可参考:http://devhy.com/2018/01/23/26-fastlane-usage/

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">


