---
title: 安利一个颜值最高的GitHub小程序
date: 2019-09-21 20:22:33
keywords:
description:
tags:
	- 小程序
categories:
	- 源码推荐

---

{% img /images/blog/安利一个颜值最高的GitHub小程序/cover.jpg %}

> 图片来自 泼辣有图

<!--more-->

作为一个码农，平时少不了要逛一逛`GitHub`（一个号称全球最大的同性交友网站）。

作为一个代码托管网站，他早已经超出了他的职责。

我们可以找到一些需要用到的开源库；

我们可以看到一些组织/个人的技术分享文章；

我们可以找到一些有趣的收集资料；

...

但是，在这个移动设备如此受欢迎的今天，`GitHub`竟然没有推出`App`，就算是码农，也不能整天都带着电脑啊。

好在`GitHub`提供了`Open API`供大家使用，让程序猿们自己解决。今天就给大家安利一个`颜值最高的GitHub小程序	———— Gitter`。

该小程序已经支持以下功能：

1. 实时查看`Trending`
2. 显示用户列表
3. 仓库和用户的搜索
4. 仓库：详情展示、`README.md`展示、`Star/Unstar`、`Fork`、`Contributors`展示、查看仓库文件内容
5. 开发者：`Follow/Unfollow`、显示用户的`followers/following`
6. `Issue`：查看`issue`列表、新增`issue`、新增`issue`评论
7. 分享仓库、开发者
8. ...

不过由于微信小程序的限制，无法做`OAuth`认证，所以想要登录自己的账户稍微有点复杂：

1. 跳转获取`Token`链接：[https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)，并使用个人账户密码登录
2. 填写一个标签，勾选自己想要授权的功能清单后并授权，就可以获得一个`Token`
3. 复制`Token`并填写到小程序中，即可登录个人账户

作者本着开源的精神，将整个项目源码同时放在了`GitHub`中，供大家学习讨论。整个项目采用 `Taro` 框架 + `Taro UI` 进行开发，小程序内数据均来自于 `GitHub Api v3`。并写了一篇文章来记录该小程序的开发过程：[Gitter - 高颜值GitHub小程序客户端诞生记](https://juejin.im/post/5c4c738ce51d4525211c129b)，真的是可玩、可学。

说了这么多，赶紧放几张颜值照片上来：

{% img /images/blog/安利一个颜值最高的GitHub小程序/xcx-1.png %}

{% img /images/blog/安利一个颜值最高的GitHub小程序/xcx-2.png %}

{% img /images/blog/安利一个颜值最高的GitHub小程序/xcx-3.png %}

{% img /images/blog/安利一个颜值最高的GitHub小程序/xcx-4.png %}

是不是很清爽呢？那就长按识别一下下方的小程序码赶紧使用吧。

{% img /images/blog/安利一个颜值最高的GitHub小程序/xcx-5.png %}

对了，还有作者`2k+`标星的`repo`：

> [https://github.com/huangjianke/Gitter](https://github.com/huangjianke/Gitter)


啊啊啊