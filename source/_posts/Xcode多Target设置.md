---
title: Xcode多Target设置
date: 2016-10-01 22:32:11
tags: 
    - 多Target
    - 多版本
categories:
    - iOS
---

有时候一个项目会分为多个版本，比如免费版、收费版，或者对于不同的客户定制不同版本。但是大体上功能都是差不多，只是部分页面稍有区别。如果每个版本都建一个工程又显得麻烦了，都放在一个 `Target` 又得写一堆的代码去区分甄别，而且在打包的时候很可能因为参数配置错误需要一而再、再而三的打包。

这个时候我们就可以用多 `Target` 来操作了。具体方法且听我一一道来。

<!--more-->

***

### * 首先我们得有一个工程，这里我就新建一个基本的模板工程。

工程的样子应该是这样。（我已经升级到 `Xcode8` 了，有什么不同之处请不要在意。）

![QQ20161001-0.png](http://upload-images.jianshu.io/upload_images/606479-c9844bad20dccae2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### *  然后我们进入工程设置，右击中间的 `TARGETS` ，会有个选择让你 `Duplicate` 还是 `Delete` ，这里我们选择 `Duplicate`。
![QQ20161001-1.png](http://upload-images.jianshu.io/upload_images/606479-2225d4c608db3235.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

结果就是下面这个样子了，多个一个 `Target` 叫 `MultiTarget copy` ，还多了一个 `plist` 文件叫 `MultiTarget copy-Info.plist`。
![QQ20161001-2.png](http://upload-images.jianshu.io/upload_images/606479-a2f374ee5f20b7e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### * 接下来首先想到的应该是改名字，毕竟 `XXX copy` 不怎么友好。

目前我所知道的方法只有一个一个的改。

囧。

如果你有好的方法，可以留言给我。

改完 `plist` 的名字后，需要在工程设置里面重新选择一下 `Info.plist` 。改完之后就像下图一样。我列了一下我改的几个地方。但是我记得早期版本的 `Xcode` 好像还需要修改 `Build Settings` 里面的一些东西。不过我的 `Xcode8` 好像不需要了。大家在做的时候注意一下。
![QQ20161001-3.png](http://upload-images.jianshu.io/upload_images/606479-0f87e7da284493a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`PS：忘了修改Bundle Identifier了，大家记得改一下`

### * 最后一步就是做版本区分了。

首先我们在 `PRO` 版本中定义一个宏 `PRO_VERSION`，写在 `Build Settings` 里面。一定记得先选择 `PRO` `Target`。这个作用就是告诉编译器，我们在编译该 `Target` 时会有个全局的宏叫做 `PRO_VERSION`。这个时候我们就可以利用这个宏来做一些代码区分了。

![QQ20161001-4.png](http://upload-images.jianshu.io/upload_images/606479-95d137145edbd4e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### * 最后我们测试一下。

我们在 `ViewController` 里面增加一个 `UILabel` ，方便起见，我就直接写 `frame` 了，在两个不同版本显示不同的文本。代码如下。

```
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 50, CGRectGetWidth([UIScreen mainScreen].bounds), 80)];
    label.textAlignment = NSTextAlignmentCenter;
    [self.view addSubview:label];
    
#ifdef PRO_VERSION
    label.text = @"这是PRO版本";
#else
    label.text = @"这是NORMAL版本";
#endif
```

当然，编译哪个版本需要选择对应的 `Scheme`。下面放两张截图。

![PRO版本](http://upload-images.jianshu.io/upload_images/606479-7f3317bd714c2300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![NORMAL版本
![Uploading QQ20161001-9_892356.png . . .]
](http://upload-images.jianshu.io/upload_images/606479-b368c5be845363d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### * 还有个事情就是图标，其实也可以设置的。

打开 `Assets.xcassets`，会发现已经有一个 `AppIcon` 了，我们再`copy`一份出来，然后改个名字，换一下图标，就是这样的效果。

![QQ20161001-5.png](http://upload-images.jianshu.io/upload_images/606479-edcc35f26db7e23c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然并没有结束，因为我们只是添加了资源，并没有用到。还是在工程设置里面，有个 `App Icons Source` ，选择一下就可以了。当然，我们还可以配置启动画面等等，这里就不演示了。

![QQ20161001-6.png](http://upload-images.jianshu.io/upload_images/606479-4245677ed7eec517.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后放一张两个 `App` 的图标，注意修改 `Bundle Identifier`，不然你不会运行出两个 `App` 的。

![QQ20161001-9.png](http://upload-images.jianshu.io/upload_images/606479-08ca83fc44225585.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)