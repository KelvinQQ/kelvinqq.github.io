---
title: Github源码推荐(1.15~1.21)
date: 2018-01-21 11:21:06
categories:
    - 源码推荐
tags: 
    - Github
---

大家好，一周时间过得真快，本次主题依旧，推荐一些`Github`上的源码，排名不分先后。

## 一、答题`App`莫名的火了起来，随之而来的则是一系列的辅助软件，例如搜狗、百度，先后推出辅助答题助手。这档子事自然少不了广大程序员`Show`一把的场面，下面推荐的就是使用`OCR`做的辅助答题助手。

> ### 特点
> * 使用手机模拟器，快速识别~
> * 浏览器自动搜索显示结果，搜索引擎可配置，结果一目了然~
> * 模拟器还能多开哦~全部答对奖金翻倍，遇到不会的可以多选乱蒙
> * 万英雄/知识超人/冲顶大会都支持哦~

<!--more-->
### 效果图
{% img /images/blog/wenda-helper.gif %}

### 热度
```
Star: 800+
```

### `Github`主页
[https://github.com/rrdssfgcs/wenda-helper](https://github.com/rrdssfgcs/wenda-helper)

## 二、上一周给大家推荐了一个`Objective-C`的日历，这次给大家推荐一个`Swift4`的日历控件，作者写的非常好棒，支持`Storyboard`。

### 效果图
{% img /images/blog/CVCalendar.gif %}

### 热度
```
Star: 2800+
```

### CocoaPods
```
pod 'CVCalendar', '~> 1.6.0'
```

### `Github`主页
[https://github.com/CVCalendar/CVCalendar](https://github.com/CVCalendar/CVCalendar)

## 三、使用`Swift`的时候是不是需要写好多的`Extension`呢？使用这个开源库后就再也不用劳心劳力的写很多的`Extension`了。

> SwifterSwift is a collection of over 500 native Swift extensions, with handy methods, syntactic sugar, and performance improvements for wide range of primitive data types, UIKit and Cocoa classes –over 500 in 1– for iOS, macOS, tvOS and watchOS.

简直不要太强大，而且支持`Swift4`。

### 热度
```
Star: 4200+
```

### CocoaPods
```
pod 'SwifterSwift'
```
你也可以只集成部分`Extension`，例如
```
pod 'SwifterSwift/Foundation'
```
具体还是去作者主页看吧。

### Carthage
```
github "SwifterSwift/SwifterSwift" ~> 4.0
```


### `Github`主页
[https://github.com/SwifterSwift/SwifterSwift](https://github.com/SwifterSwift/SwifterSwift)

## 四、基本上所有的`App`都逃脱不了表单，设置、用户界面等待，都是一些枯燥无味的代码，却又要花费时间、精力去自定义每个`Cell`达到各式各样的效果，此次为大家推荐一个强大的表单开源库，最重要的是`OC`和`Swift`都有，各取所需。

* `OC`版

### 效果图
{% img /images/blog/XLForm.gif %}

### 热度
```
Star: 4800+
```

### CocoaPods
```
pod 'XLForm', '~> 4.0'
```
### Carthage
```
github "xmartlabs/XLForm" ~> 4.0
```

### `Github`主页
[https://github.com/xmartlabs/XLForm](https://github.com/xmartlabs/XLForm)

* `Swift`版

### 效果图
{% img /images/blog/Eureka-1.gif %}

### 热度
```
Star: 7500+
```

### CocoaPods
```
pod 'Eureka'
```
### Carthage
```
github "xmartlabs/Eureka" ~> 4.0
```

### `Github`主页
[https://github.com/xmartlabs/Eureka](https://github.com/xmartlabs/Eureka)

关于使用方法这里就不再介绍了，项目主页上介绍的非常清楚，有需求的小伙伴就自己去查看吧~


好了，暂时就推荐这么多给大家，下周我们再见。

PS：本文中图片部分皆来自于作者`Github`主页，如有侵权，请告知。