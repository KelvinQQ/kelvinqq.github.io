---
title: iOS10下使用AutoLayout动画失效解决
date: 2016-11-01 19:57:31
tags: 
    - iOS10
    - AutoLayout
categories:
    - iOS

---
更新到 `iOS10` 后，原来使用 `AutoLayout` 自定义的控件做了一些简单的动画失效了，但是在 `iOS10` 以下还是正常的。于是 `Google` 了一番，终于又是在 [http://stackoverflow.com](http://stackoverflow.com) 找到了解决办法。

<!--more-->

```
[UIView animateWithDuration:0.3
                          delay:0
                          options:UIViewAnimationOptionCurveEaseOut
                       animations:^{
         [self mas_updateConstraints:^(MASConstraintMaker *make) {
                                                          
         }];
         [self layoutIfNeeded];
     }
                      completion:^(BOOL finished) {
                                              
     }];
```

原来我们的代码应该是类似这样的，使用 `layoutIfNeeded` 强制刷新然后执行动画。`iOS10` 以后需要使用 `[self.superview layoutIfNeeded];` 。这样消失的动画就又出来了。