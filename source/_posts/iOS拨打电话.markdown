---
title: "iOS拨打电话"
date: 2014-09-05 21:53:07 +0800
tags: 
    - 电话
categories:
    - iOS

---

iOS拨打电话公开的方法有两种,其他的是调用私有方法,是会被苹果拒绝的.下面记录一下.

第一种方法拨打完之后会停留在拨号盘,不会返回应用。

<!--more-->
```
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"tel://123456789"]];

```

第二种方法拨打玩电话后会返回应用，但是拨打之前会有一个`UIAlertView`提示是否拨打。


```
UIWebView *callWebview =[[UIWebView alloc] init]; // 如果无效,则将callWebView置为成员变量
NSURL *telURL =[NSURL URLWithString:@"tel:10086"];
[callWebview loadRequest:[NSURLRequest requestWithURL:telURL]];
[self.view addSubview:callWebview];
```