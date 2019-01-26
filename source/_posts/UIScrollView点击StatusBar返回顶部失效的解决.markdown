---
title: "UIScrollView点击StatusBar返回顶部失效的解决"
date: 2015-08-21 08:47:45 +0800
tags: 
    - ScrollView
categories:
    - iOS

---

前几天看到一篇文章，关于如何解决在一个`Vieww`中有多个`UIScrollView`或者`UIScrollView`子类时，点击`StatusBar`无法使`UIScrollView`返回到顶部的文章。总得思想是自定义一个`View`，然后覆盖在`StatusBar`上，给这个`View`添加点击事件。具体文章链接在[这里](http://www.cocoachina.com/ios/20150814/12949.html)。有兴趣的朋友可以看一下。
<!--more-->
其实我们查看`UIScrollView`的头文件就可以找到这段注释：

```
// When the user taps the status bar, the scroll view beneath the touch which is closest to the status bar will be scrolled to top, but only if its `scrollsToTop` property is YES, its delegate does not return NO from `shouldScrollViewScrollToTop`, and it is not already at the top.
// On iPhone, we execute this gesture only if there's one on-screen scroll view with `scrollsToTop` == YES. If more than one is found, none will be scrolled.
@property(nonatomic) BOOL  scrollsToTop;          // default is YES.
```

这样我们就可以很清楚的了解到，`scrollsToTop`的默认值是`YES`，然而当有多个`UIScrollView`的时候，用户点击`StatusBar`，系统就不知道让哪一个`UIScrollView`来执行`scrollsToTop`这个动作了，所以就导致失效了。

这样一来，解决方法就很简单了，设置你想要执行`scrollsToTop`的`UIScrollView`的`@property(nonatomic) BOOL  scrollsToTop;`属性值为`YES`，其他的`UIScrollView`都为`NO`即可。

注意：凡是`UIScrollView`以及`UIScrollView`的子类都要设置。如:`UITableView`，`UIWebView`，`UICollectionView`等。
