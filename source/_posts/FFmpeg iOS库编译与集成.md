---
title: FFmpeg iOS库编译与集成
date: 2016-09-26 21:44:05
tags: 
	- FFmpeg
categories:
	- FFmpeg

---


## 以下系列文章基于FFmpeg 3.1.1和Xcode8。


#### [FFmpeg iOS库编译与集成](http://historyzhang.github.io/2016/09/26/FFmpeg%20iOS%E5%BA%93%E7%BC%96%E8%AF%91%E4%B8%8E%E9%9B%86%E6%88%90/)

#### [FFmpeg解码流程](http://historyzhang.github.io/2016/09/30/FFmpeg%E8%A7%A3%E7%A0%81%E6%B5%81%E7%A8%8B/)


由于FFmpeg工程太大，很难一下子理解透彻，所以就边看边记一些笔记，理清一下思路，顺便也留给其他人一些意见。

<!--more-->

## 1. 下载FFmpeg的源码编译iOS库。
而编译FFmpeg还需要另外两项的支持

* [https://github.com/libav/gas-preprocessor](https://github.com/libav/gas-preprocessor)
* yasm

这样就比较复杂，如果想自己一步一步的按照流程来做，可以参考这篇文章 [iOS配置FFmpeg框架(原创)
](https://cnbin.github.io/blog/2015/05/19/iospei-zhi-ffmpegkuang-jia/) 。

所以 Github 上有个开源的脚本，[https://github.com/kewlbear/FFmpeg-iOS-build-script](https://github.com/kewlbear/FFmpeg-iOS-build-script) ，下载之后，直接 `./build-ffmpeg.sh`，脚本会自动帮你下载相关文件以及配置。

编译成功之后，就会在文件夹里面看到 `FFmpeg-iOS` 的文件夹，里面就是静态库，还有个 `ffmpeg-3.1.1` 的文件夹，就是源码。当然，如果你熟悉脚本语言可以看一下里面的脚本，可以修改一些配置，达到你想要的结果。这里暂时先不展开了。

## 2. 集成静态库至Xcode
* 新建工程。

新建一个 `Single View Application` ，然后将 `FFmpeg-iOS` 文件夹拖进工程。然后需要在 `Build Setting` 里面配置一下 `Header Search Paths` ，需要将 `include` 以及 `include` 下面的子文件夹都配置进去。

* 添加依赖库。

需要添加以下几个 `framework` 和 `lib` ： `CoreMedia.framework` ， `VideoToolbox.framework` ， `AudioToolbox.framework` ， `libiconv.2.4.0.tbd` ， `libbz2.1.0.tbd` ， `libz.1.2.5.tbd`。

* 编译

在 `ViewController` 里包含头文件 `#import "avcodec.h"`， 然后在 `viewDidLoad` 中调用 `avcodec_register_all();` ，应该就可以编译通过了。

在我的 `Xcode8` 中会有一堆警告，提示 `empty paragraph passed to @param command` ，这里我们需要处理一下。在引用头文件的时候使用宏包含一下。

```
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wdocumentation"

#import "avcodec.h"

#pragma clang pop
```

至此，`FFmpeg` 就集成完毕了。