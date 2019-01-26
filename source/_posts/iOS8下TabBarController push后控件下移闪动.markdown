---
layout: post
title: "iOS8下TabBarController push后控件下移闪动"
date: 2015-04-27 16:04:40 +0800
comments: true
tags:  iOS随笔

---
使用`TabBarController`后,如果push下一个页面需要隐藏`TabBar`,而下一个页面中有个控件设置`AutoLayout`的时候设置了和页面底部的距离,那么会有个闪烁动画,控件会以动画的形式下移44px.该现象只在iOS8中有,iOS7未发现.

<!--more-->
我们需要在设置`AutoLayout`的时候如下图修改一下即可修复这个动画.
{% img /images/blog/2015-04-27-16-00.png %}
