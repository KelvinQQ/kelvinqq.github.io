---
title: Github源码推荐(1.8~1.14)
date: 2018-01-14 22:22:30
categories:
    - 源码
tags: 
    - Github
---

本次给大家推荐一些`Github`上的源码，排名不分先后。

## 一、微信小游戏的推出，自然而然是火爆了一把。但是一切都阻止不了程序员，自然是要破解一番的，本次首推的就是利用`Python`开发的`跳一跳`辅助。

### 原理说明(摘自作者原文)
1. 将手机点击到《跳一跳》小程序界面

2. 用 ADB 工具获取当前手机截图，并用 ADB 将截图 pull 上来

3. 计算按压时间
  * 手动版：用 Matplotlib 显示截图，用鼠标先点击起始点位置，然后点击目标位置，计算像素距离；
  * 自动版：靠棋子的颜色来识别棋子，靠底色和方块的色差来识别棋盘；

4. 用 ADB 工具点击屏幕蓄力一跳

<!--more-->
### 热度
```
Star: 11800+
```

### `Github`主页

[https://github.com/wangshub/wechat_jump_game](https://github.com/wangshub/wechat_jump_game) 

## 二、是否觉得系统的相册太不方便，而自定义又太耗费时间，那就试试`DKImagePickerController`吧，作者完全使用`Swift`编写，支持`CocoaPods`。

### 效果图
{% img /images/blog/DKImagePickerController-1.png %}

{% img /images/blog/DKImagePickerController-2.png %}

### 热度
```
Star: 890+
```

### CocoaPods
```
pod 'DKImagePickerController'
```

### `Github`主页
[https://github.com/zhangao0086/DKImagePickerController](https://github.com/zhangao0086/DKImagePickerController) 

## 三、系统邮箱中的交互让很多人大爱，尤其是列表的左右滑动手势，`Github`上有很多人都开源了自己的模仿源码，本次推荐的也是一个使用纯`Swift`编写的源码。

### 效果图
{% img /images/blog/SwipeCellKit.gif %}

### 热度
```
Star: 2700+
```

### CocoaPods
```
pod 'SwipeCellKit'
```
### Carthage
```
github "SwipeCellKit/SwipeCellKit"
```

### `Github`主页
[https://github.com/SwipeCellKit/SwipeCellKit](https://github.com/SwipeCellKit/SwipeCellKit)

## 四、相信很多人都有编写日历的需求，在`Todo`，万年历等中，都需要日历，本次为大家推荐一个`Objective-C`编写的日历，支持横向和纵向滚动，支持切换月视图和周视图，支持`Localization`。

### 效果图
{% img /images/blog/JTCalendar.gif %}

{% img /images/blog/JTCalendar-2.png %}

### 热度
```
Star: 2400+
```

### CocoaPods
```
pod 'JTCalendar', '~> 2.0'
```
### Carthage
```
github "jonathantribouharet/JTCalendar" ~> 2.2
```

### `Github`主页
[https://github.com/jonathantribouharet/JTCalendar](https://github.com/jonathantribouharet/JTCalendar)

好了，暂时就推荐这么多给大家，下周我们再见。

PS：本文中图片部分皆来自于作者`Github`主页，如有侵权，请告知。