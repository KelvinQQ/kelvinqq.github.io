---
title: "Runtime在Storyboard中的应用"
date: 2014-09-01 21:11:19 +0800
tags: 
    - Runtime
categories:
    - iOS

---
# 正文
Runtime真是无处不在啊,打开Storyboard后,我们添加一个View到界面中,选中View,切换属性卡到第三个,有一项是 `User Defined Runtime Attributes`,我们添加如下图两个`Key Path`并设置值.
<!--more-->

{% img /images/blog/runtime_storyboard_1.png %}

然后运行,果然可以看到我们设置的属性生效了.顿感强大.

{% img /images/blog/runtime_storyboard_2.png %}


