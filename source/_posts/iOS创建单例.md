---
title: iOS创建单例
date: 2016-11-01 20:01:31
tags: 
    - 单例
categories:
    - iOS

---
在开发过程中经常会遇到需要单例的时候，然后很多时候大家写的单例其实并不符合要求。下面介绍一个标准的单例。

一般来说，我还是喜欢用`GCD`来创建单例，使用`dispatch_once`很方便。
<!--more-->
```
static id _instance;   
 
+ (instancetype)sharedInstance  
{   
    static dispatch_once_t onceToken;   
    dispatch_once(&onceToken, ^{   
        _instance = [[self alloc] init];   
    });   
    return _instance;   
} 

```

上面说的并不符合要求就是这样创建出来的单例。很多人以为这样就可以了，`dispatch_once`保证了只运行一次。然而，如果一个不知情的人调用了你写的类，你无法保证他不去调用`alloc`，`copy`来生成实例。
所以我们还要做一些其他的处理。

```
+ (instancetype)allocWithZone:(struct _NSZone *)zone  
{   
    static dispatch_once_t onceToken;   
    dispatch_once(&onceToken, ^{   
        _instance = [super allocWithZone:zone];   
    });   
    return _instance;   
}   
 
- (id)copyWithZone:(NSZone *)zone   
{   
    return _instance;   
}  
 
- (id)mutableCopyWithZone:(NSZone *)zone {   
    return _instance;   
}
```

这样就可以了。