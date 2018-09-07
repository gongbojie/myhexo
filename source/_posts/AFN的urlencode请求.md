---
title: 由AFN的url encode请求引发的问题
date: 2018-07-19 14:42:27
tags:
---

在项目中的网络请求大多数情况下使json格式返回，但是有时候也有其他的格式。

## <i class="fab fa-internet-explorer"></i> Postman中使用url encode

![](http://ww1.sinaimg.cn/large/880fe8a4gy1ftfaqhy4ndj20uk092q49.jpg)
![](http://ww1.sinaimg.cn/large/880fe8a4gy1ftfano5tswj20uk0aj3zt.jpg)

<!-- more -->

## AFN常规请求

```objc
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
    manager.requestSerializer=[AFJSONRequestSerializer serializer];//申明请求的数据是json类型
    manager.responseSerializer = [AFJSONResponseSerializer serializer];//申明返回的结果是json类型
    [manager POST:hostUrl parameters:dic success:^(NSURLSessionDataTask *task, id responseObject) {
        NSLog(@"success--%@--%@",[responseObject class],responseObject);
    } failure:^(NSURLSessionDataTask *task, NSError *error) {
        NSLog(@"failure--%@",error);
    }];
```

发现返回结果报错

```json
failure--Error Domain=com.alamofire.error.serialization.response Code=-1011 "Request failed: unsupported media type (415)" 
UserInfo={NSLocalizedDescription=Request failed: unsupported media type (415), NSUnderlyingError=0x1c0646930 
	{Error Domain=com.alamofire.error.serialization.response Code=-1016 "Request failed: unacceptable content-type: text/html" 
	UserInfo={NSLocalizedDescription=Request failed: unacceptable content-type: text/html, NSErrorFailingURLKey=http:

	Headers {
    Content-Type = (
    text/html;charset=utf-8,
);
    Content-Length = (
    1092,
);
    Server = (
    Apache-Coyote/1.1,
);
    Date = (
    Thu, 19 Jul 2018 09:43:07 GMT,
)
```

注意到code=-1011，错误码为415，text/html;charset=utf-8

接着加上请求的编码格式

```objc

后经验证 这一行设置代码可以删除 使用 manager.requestSerializer = [AFHTTPRequestSerializer serializer]; 即可

[manager.requestSerializer setValue:@"application/x-www-form-urlencoded;" forHTTPHeaderField:@"Content-Type"];
```

发现时json编码格式，不对
接着查看AFN的请求Header，加上以下代码

```objc
NSHTTPURLResponse *response = (NSHTTPURLResponse *)task.response;
NSDictionary *allHeaders = response.allHeaderFields;
```

allHeaders中的数据
```json
{
    Content-Type = application/json;
    Content-Length = 63;
    Server = Apache-Coyote/1.1;
    Date = Thu, 19 Jul 2018 09:49:12 GMT;
}
```

可以进行到请求成功里面，但是返回的POST的提交的数据却无法提交，返回结果为提交的数据为空，表示提交的数据编码不对，修改请求的编码序列化
~~manager.requestSerializer = [AFJSONRequestSerializer serializer];~~

```objc
manager.requestSerializer = [AFHTTPRequestSerializer serializer];
```
然后可以正常进行数据请求了。

------
参考文章：

1. [AFNetworking报错:(415 Domain=com.alamofire.error.serialization.response Code=-1011 "Request failed: unsupported media type (415)")](https://www.jianshu.com/p/81e648ac3589)
2. [AFN Post请求中出现的问题](https://www.jianshu.com/p/2580c05d5a1d)
3. [AFNetworking之AFURLRequestSerialization深入学习](https://juejin.im/post/5a71289c5188252edb592f5f)
4. [AFNetWorking 源码分析之 AFHTTPSessionManager](https://juejin.im/entry/58f850eb0ce4630061105c8d)
5. [使用 AFNetworking3.0请求时如何获取响应头文件](https://www.jianshu.com/p/a9f09052eaed)
6. [AFNetworking源码解析](https://www.jianshu.com/p/22075e7db6f7)