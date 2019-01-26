---
layout: post
title: "hidesBottomBarWhenPushed用法"
date: 2014-08-02 10:12:57 +0800
tags: 
    - TabBar
categories:
    - iOS

---

# 前言
前两天看论坛又看到有人在问`hidesBottomBarWhenPushed`到底怎么用,为什么他用总是不对.所以在这里还是总结下,希望可以帮助更多的人.
<!--more-->
# 正文
在使用`TabBar`的时候,需要在`push`到下个页面的时候隐藏`TabBar`,再`pop`回来的时候显示`TabBar`.以前有很多博客是说使用`hidden`参数,不过总是有黑边,遂又有一堆博客解决如何去掉黑边,甚至有人自定义了`TabBar`.

其实看看`API`就会发现有`hidesBottomBarWhenPushed`这个属性可以使用.不过很多人用了发现有点问题,比如有`A->B->C`这个`push`流程,其中`A`中有`TabBar`,`B`和`C`中需要隐藏,很多人从`A` `push`到 `B`是可以隐藏`TabBar`,可是`B` `push`到 `C`又冒出来了.或者从`C` `pop`到 `B`又冒出来了.反正就是不对.

说到底,还是用法有误.`Apple`不会给一个错误的`API`的.其实你可以在`B`和`C`的`init`函数中使用`self.hidesBottomBarWhenPushed = YES`,这样就可以达到你想要的效果了.


