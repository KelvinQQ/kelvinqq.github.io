---
title: "App内打开AppStore"
date: 2015-05-19 14:53:17 +0800
categories : 
  - iOS
tags:
  - AppStore

---

iOS7以后可以在应用内打开AppStore展示另一个App.这样就可以直接下载了.

我们只要使用这段代码就可以了.

* 应用库文件

我们可以这样引用: `@import StoreKit;`

<!--more-->

* 请求App

```
SKStoreProductViewController *storeProductVc = [[SKStoreProductViewController alloc] init];
            storeProductVc.delegate = self;
            [storeProductVc loadProductWithParameters:@{
                                                        SKStoreProductParameterITunesItemIdentifier: @"XXXXXXX",
                                                        }
                                      completionBlock:^(BOOL result, NSError *error) {
                                          if (result) {
                                              [self presentViewController:storeProductVc animated:YES completion:nil];
                                          }
                                      }];
```

* 实现`delegate` `dismiss` `SKStoreProductViewController`

```
- (void)productViewControllerDidFinish:(SKStoreProductViewController *)viewController
{
    [viewController dismissViewControllerAnimated:YES completion:nil];
}
```