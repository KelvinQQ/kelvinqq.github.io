---
title: "NSArray对所有元素操作"
date: 2014-07-23 20:45:17 +0800
tags: 
    - NSArray
categories:
    - iOS

---

看别人的源码无意中看到一个方法,是`NSArray`的实例方法:

```
- (void)makeObjectsPerformSelector:(SEL)aSelector;
- (void)makeObjectsPerformSelector:(SEL)aSelector withObject:(id)argument;
```

<!--more-->

用处是让NSArray中的每一个元素都执行`aSelector`方法,还可以带参数`argument`.
体验如下:

```
[[self.view subviews] makeObjectsPerformSelector:@selector(removeFromSuperview)];
```

再也不用`for`循环去删除每一个子`view`了.