---
title: Swift4.0引用3.0第三方库
date: 2017-10-21 23:56:21
tags: [Swift4, iOS]
categories: [iOS]
---

`Swift`已经发布了`4.0`版本，在`Xcode9`中新建项目后，默认是使用`4.0`语法的。项目中的引用的第三方库，虽然有很多已经发不了`4.0`版本，但是还是有一些未及时更新的，那在作者未更新之前我们是否有更好的办法来使用这些第三方库呢？答案当然是**肯定**的，`Xcode9`中是同时支持`3.2`和`4.0`语法的。具体的设置可以看下图。

<!--more-->

![设置Swift语法版本](http://upload-images.jianshu.io/upload_images/606479-6a28ff6e929e5fc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么下面就说说如何设置同时支持`3.2`和`4.0`。

1. 项目中如果使用`Cocoapods`来管理第三方库时，可以找到不支持`4.0`语法的库所在`target`，然后找到`Swift Language Version`选项，改为`3.2`，然后就可以顺利编译通过了。
![Cocoapods库设置](http://upload-images.jianshu.io/upload_images/606479-6d08d94bf9c81df0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 如果还有以源码集成进项目的，那就选择`Edit > Convert > To Current Swift Syntax..`吧