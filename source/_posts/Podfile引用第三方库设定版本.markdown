---
title: "Podfile引用第三方库设定版本"
date: 2015-09-25 08:11:54 +0800
tags: 
    - CocoaPods
categories:
    - iOS
---


在使用`cocoapods`引用第三方库时，可以使用如下规则规定引用三方库的版本。

<!--more-->

```
pod 'AFNetworking’      // 不显式指定依赖库版本，表示每次都获取最新版本
pod 'AFNetworking’,  '2.0’     //只使用2.0版本
pod 'AFNetworking’, '>2.0'     //使用高于2.0的版本
pod 'AFNetworking’, '>=2.0'     //使用大于或等于2.0的版本
pod 'AFNetworking’, '<2.0'     //使用小于2.0的版本
pod 'AFNetworking’, '<=2.0'     //使用小于或等于2.0的版本
pod 'AFNetworking’, '~>0.1.2'     //使用大于等于0.1.2但小于0.2的版本，相当于>=0.1.2并且<0.2.0
pod 'AFNetworking’, '~>0.1'     //使用大于等于0.1但小于1.0的版本
pod 'AFNetworking’, '~>0'     //高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本
```